---
title: "Online (Prospective) Regime Detection — Filtering vs Smoothing"
status: growing
created: 2026-06-07
updated: 2026-06-11
tags: [foundation, methodology, regime, regime-detection, online, change-point, prospective, real-time, filtering, day-swing, capability-gap, cold-start]
---

# Online (Prospective) Regime Detection — Filtering vs Smoothing

> **TL;DR**: The team's regime-classification workstream has a single load-bearing constraint — a regime label must be **callable at the start of a range using only past data** to gate a *live* directional trade. This is the **filtering** problem (estimate the current regime from data up to now), which is structurally different from the **smoothing** problem (label every historical period with full hindsight). Almost every easy regime classifier — including the team's own retrospective stable-range detector and the Viterbi-decoded HMM — solves the *smoothing* problem and silently fails the live-deployment bar. This page names the distinction, then catalogues two methods purpose-built for the **online/filtering** setting: **Issa & Horvath's signature-MMD online detector** (non-parametric, path-dependent, demonstrated on crypto) and **Tsaknaki–Lillo–Mazzarisi's score-driven Bayesian Online Change-Point Detection (BOCPD)** (real-time regime shifts with within-regime temporal correlation). It closes with **practitioner real-time proxies** (CoinMarketCap Fear & Greed / Altcoin-Season / BTC-Dominance) as the cheap, callable-now baseline the academic methods must beat.

## Why this page exists

Per this research program, the team's gating workstream is regime classification — labelling multi-day (~7d) ranges as **bull/bear** to gate directional strategies, with a separate volatility-state axis (calm / compression / expansion / high_vol / panic). The open challenge explicitly flagged: *"prospective (real-time) vs retrospective classification — a label must be callable at the range's start from past data only to gate a live trade (ties to the team's retrospective-classifier live-deployment gap)."*

The wiki already has the **retrospective** side covered:
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — 15 named regimes, written with hindsight (the page itself flags this: only the 2026-Q2 "live" entry is contemporaneous).
- [[foundation/methodology/team-stable-range-detector-methodology]] — the team's two-stage detector, which classifies periods *between* stable ranges as bull/bear/undefined using ZigzagRegression (needs N=5 confirmed extremums) + Mann-Kendall. **Both require future bars to confirm a label.** That is the live-deployment gap, stated structurally.

This page is the **methodological backbone for the prospective side** — the half the team is actually building. It is deliberately a *methodology* page (cross-market-transfer of method, per Part I §5), not a strategy: neither method is yet in the backtesting lab DSL, so this is **capability-blocked, not research-blocked** (per a standing convention — the research sharpens the capability-investment decision itself).

## The filtering–smoothing distinction (the load-bearing idea) {#filtering-vs-smoothing}

Borrowed from state-space estimation, three estimation targets for "what regime are we in at time *t*":

| Target | Information set | Question answered | Deployable live? |
|---|---|---|---|
| **Filtering** | data up to *t* only | "What is the regime *right now*, given everything I've seen so far?" | ✅ **Yes** — this is the live-trade gate |
| **Prediction** | data up to *t* | "What regime will the *next* bar be in?" | ✅ Yes (one-step-ahead) |
| **Smoothing** | the *whole* sample (past + future) | "In retrospect, what regime was each historical bar in?" | ❌ **No** — uses future data |

**The trap**: the cleanest-looking regime labels are almost always smoothed. An HMM fit to the full series and decoded with the **Viterbi** algorithm gives a beautiful, low-noise regime path — because each label is informed by what happened *after* it. The team's stable-range detector is the same shape: a range is confirmed "bull" only once 5 zigzag extremums (which extend past the range's start) have printed. Both are *smoothers in disguise*. A backtest gated on smoothed labels is reading tomorrow's newspaper — it will look excellent and fail to deploy.

**The discipline**: any regime classifier intended to gate a live trade must be evaluated in its **filtering** mode — relabel using only data available at decision time, accept the higher label noise and lag, and re-run the backtest. The drop from smoothed to filtered performance *is* the live-deployment haircut. Measuring it is the point. This is the regime-classification analogue of the bar-level haircut in [[foundation/methodology/bar-level-validation-overlay]] and the L1→L2 translation loss in [[foundation/methodology/forecast-vs-tradeable-gap]].

> **Read every regime-method paper through this lens.** The first question for any cited result is *"is the reported metric filtered or smoothed?"* A one-step-ahead forecast metric is prospective; an in-sample Viterbi labelling is retrospective. Many papers report the latter and imply the former.

### Operational corollary — compute over full history, then slice the window {#compute-full-history-then-slice}

The filtering discipline above guards the *future* side (don't use data after *t*). There is a mirror failure on the *past* side: when **summarizing or windowing** the output of a path-dependent causal detector (regime state, vol-state, slow trend — anything carrying state forward bar-to-bar), recomputing the detector *from scratch inside the window* discards history the detector causally owned. Near the window's left edge the detector hasn't converged from its cold start, so "what was the regime in week X" silently depends on where the window began — same detector, different answer.

> **The rule**: compute the per-bar series over the **full available history**, then **slice the window from the result**. Never re-run a stateful detector cold from the window's first bar.

Hit concretely in the 2026-06-11 state-monitor digest build: all three monitor detectors compute full per-bar series and take `.iloc[-1]` for the card; the digest reuses those same full series and slices — zero new detector logic, by design. The rule is cheap and prevents a subtle class of cold-start bugs in any backtest report, digest, or research notebook that summarizes regime state over a window. The per-bar check in [[foundation/methodology/bar-level-validation-overlay]] applies to the *sliced* series too — slicing correctly doesn't exempt the labels from bar-level validation.

## Method 1 — Signature-MMD online regime detection (Issa & Horvath 2023) {#signature-mmd}

**Source:** [[foundation/sources/issa-horvath-online-regime-detection]] (arXiv 2306.15835). The single most on-target paper found — its *central contribution* is the online problem, and it is demonstrated on crypto price paths.

**What it is.** A non-parametric, model-free regime detector built from three pieces:
1. **Rough-path signatures** as the feature map — a path is summarised by its iterated-integral signature, which captures order and interaction of moves (not just marginal distribution). This is why the method is **path-dependent / non-Markovian** — exactly the structure of a multi-day range.
2. **Maximum Mean Discrepancy (MMD)** as a similarity metric between distributions over these signature features.
3. A **path-wise two-sample test**: is the incoming window drawn from the same distribution as the baseline window, or has the distribution shifted?

**Why it fits the team's problem.**
- **Online by design** — the authors explicitly optimise the detector "to the setting where the size of new incoming data is particularly small, for faster reactivity." That is the filtering setting: react to a shift as soon as a few new bars arrive, not after the range completes.
- **Non-parametric** — no assumption of Gaussian regimes or a fixed number of states (unlike HMM/MSGARCH). It detects *that* the distribution changed without pre-committing to what the regimes are.
- **Companion clustering** — a second technique groups historical windows into regimes of "approximately similar market activity," giving a principled distance metric to bucket ranges into bull/bear/vol-state *without arbitrary thresholds* (contrast the team's `stability_score ≥ 65` cutoff).
- **Demonstrated on crypto** — tested on synthetic data of increasing complexity, high-dimensional equity baskets, **and cryptocurrency price evolution**; "swiftly and accurately indicated historical periods of market turmoil."

**Honest scope.** Higher implementation complexity (signature truncation level, MMD kernel + bandwidth, baseline-window length, detection threshold are all choices that need their own validation per [[foundation/methodology/strategy-validation-discipline]]). The paper demonstrates *turmoil detection* (a vol-state / break signal) more directly than *bull-vs-bear direction* — direction likely needs the clustering layer plus a directional read on the post-shift segment. Signature computation is a real engineering lift; this is the capability-gap line item.

## Method 2 — Score-driven Bayesian Online Change-Point Detection (Tsaknaki–Lillo–Mazzarisi 2023) {#score-driven-bocpd}

**Source:** [[foundation/sources/tsaknaki-lillo-mazzarisi-bocpd]] (arXiv 2307.02375). The canonical *online* change-point framework, with an upgrade that fixes its usual weakness.

**What it is.** **Bayesian Online Change-Point Detection (BOCPD)** maintains, at every step, a posterior over the **run length** — how long since the last regime change — and updates it in real time as each new observation arrives. It natively answers "has the regime just changed, given data up to now?" (filtering).

The paper's contribution is a **score-driven** extension: standard BOCPD assumes observations are **i.i.d. *within* each regime**, which is false for financial data (order flow and returns are autocorrelated *inside* a regime). The score-driven mechanism lets parameters vary with time *within* a regime (a GAS / score-driven update), so the detector doesn't mistake ordinary within-regime drift for a regime change.

**Why it fits the team's problem.**
- **Real-time by construction** — "identify regime shifts in real-time and enable online predictions." Pure filtering; no hindsight.
- **The within-regime time-variation is exactly the 7-day-range pain point.** A multi-day crypto range is *not* i.i.d. inside itself — it drifts, compresses, and trends in sub-segments. A naive change-point detector fragments such a range into spurious regimes; the score-driven version tolerates within-range dynamics and only flags genuine breaks. This directly attacks the over-segmentation failure mode.
- **Empirically beats i.i.d.-within-regime models** out-of-sample on NASDAQ order flow, with well-specified residuals (distribution + temporal correlation).

**Cross-market-transfer note.** The empirical work is on **NASDAQ order flow**, not crypto — so this is a Category-1 cross-market transfer per [[foundation/idea-sourcing-methodology]], and the transfer burden applies (does the mechanism survive crypto's 24/7, retail-heavy microstructure? per [[foundation/market-structure/crypto-carrier-features]]). But the *method* is asset-agnostic, and it pairs naturally with the wiki's existing perp-microstructure ingests ([[foundation/sources/temporal-dynamics-funding-rates]], [[foundation/sources/two-tiered-funding-rate-markets]]). It also connects regime detection to **order-flow imbalance** as a leading feature — the same signal flagged in newer crypto microstructure work as a precursor to liquidation cascades / regime breaks.

## How the two methods relate, and where HMM/MSGARCH sit {#method-comparison}

| Method | Native mode | State assumption | Within-regime dynamics | Direction vs vol-state | Crypto-demonstrated |
|---|---|---|---|---|---|
| **Signature-MMD** (Issa–Horvath) | Online (filtering) | None (non-parametric) | Path-dependent (handles it) | Turmoil/break first; direction via clustering | ✅ Yes |
| **Score-driven BOCPD** (Tsaknaki et al.) | Online (filtering) | Parametric per-regime | Score-driven (handles it) | Break-timing; direction from segment read | NASDAQ (transfer needed) |
| **HMM / NHHM** (e.g. Koki–Leonardos–Piliouras, BTC/ETH bull/bear/calm) | Filtering *or* smoothing | Fixed # states | i.i.d. within state (weak) | Direct bull/bear/calm states | ✅ Yes |
| **MSGARCH** | One-step-ahead (filtering) | 2–3 vol regimes | GARCH within state | **Vol-state axis only** | ✅ Yes (292-coin) |
| **Team stable-range detector** | **Smoothing** (needs future extremums) | range / bull / bear / undefined | — | Direction, retrospective | BTC (the live gap) |

Two practical readings:
1. **The vol-state axis is the easier, better-supported half.** Multiple independent results (MSGARCH across 292 coins; the Deribit term-structure vol-regime work; the Bitcoin variance-risk-premium literature) converge on a high-vol-vs-calm bifurcation that is callable **one-step-ahead** (prospectively). This is N=3+ external support for the team's choice to make vol-state a *separate axis* from direction.
2. **The 7-day directional-range label is the genuine frontier.** It is what the signature-MMD and score-driven-BOCPD machinery exist for, precisely because neither assumes the i.i.d.-within-regime structure that breaks HMM-Viterbi labelling on multi-day ranges. If the team wants one method to prototype first, **score-driven BOCPD** is the lower-lift entry (well-documented framework, parametric, clear filtering semantics); **signature-MMD** is the higher-ceiling, higher-effort option (non-parametric, path-native, already shown on crypto).

## Implemented baseline detector — dual-window causal (2026-06-07) {#implemented-baseline}

The method catalogue above lists the *candidate* online detectors. The team has since built and run a transparent **first filtering-mode detector** — a no-ML, hand-built dual-window classifier. It is documented here as the **reusable, reproducible baseline** the academic methods (and any future build) must beat — the code analog of the CMC baseline below. It is **symbol-generic** (a ready template for ETH/SOL).

**Causal by construction**: every value at bar *t* uses only data ≤ *t*, verified by a leakage self-test — each label is recomputed on the truncated series `df[:t+1]` and asserted unchanged. It is a **detector, not a predictor**: it recognizes the current regime and flags transitions; it does NOT call forward direction (that was falsified — see [[trading/rejected/directional-regime-gate-trend-filtering-2026-06]]).

Script: the backtesting lab (generic; trend helper reused from `regime_filter_causal.py`, dual-window from `regime_filter_dualwindow.py`).

### Part A — direction (bull/neutral/bear), dual-window

1. **Trend strength** (scale-free, causal) = `mean(last N daily log-returns) / std(last N)` — a trailing per-bar trend t-stat. Two windows: **SHORT 14d** (~2 weeks), **LONG 42d** (~6 weeks).
2. **Hysteresis** per window: enter bull at trend ≥ +0.18, exit only below +0.06 (mirror for bear). Sticky → no bar-to-bar flicker.
3. **Confirmed regime** = the LONG window's call, committed only after **3-day persistence** (a 1-bar blip can't flip it).
4. **Confidence = cross-window agreement** — `(tanh(|t_short|/ENTER) + tanh(|t_long|/ENTER))/2`, ×1.0 if the two windows agree else ×0.4. An `in_transition` flag marks disagreement. (Agreement bars ~0.54 confidence; transition bars ~0.23.)
5. **Causal forward-fill to 4h** — each 4h bar takes the most recent daily label whose day had ALREADY CLOSED (`merge_asof(direction="backward")` on `close_time`); never its own unfinished day, never the future. (4h-native windows were degenerate — the long window almost never confirmed direction — so daily-computed + causal-ffill is the working calibration.)

### Part B — break events (short-term), 4h-native

`break_up` / `break_down` = a FRESH volatility expansion (short-window realized vol > 1.5× long-window vol, first such bar) AND the short trend's sign. Mirrors the team's break signal causally. A magnitude+direction EVENT, not a forecast.

### Emitted columns (per bar)

`causal_regime_confirmed`, `causal_is_bull/bear`, `causal_confidence`, `causal_in_transition`, `causal_just_entered_bull/bear`, `causal_just_entered_break_up/down`, `causal_phase_just_changed`. (Registered in the data loader's `_OHLCV_OPTIONAL_ORDER` allowlist so the DSL can read them via `s.field()`.)

### How a meta strategy consumes it (regime_v3_causal)

The classifier is a **router/conditioner**: each emitted column is a GATE deciding which sub-mechanism may fire on a bar. The mechanism (entry/stop/target) is separate — the classifier only opens/closes the door ("regime as a [[foundation/methodology/regime-marker-data-sources|conditioning attribute]]," per a standing convention).

- Subs 1–2 (directional phase entry) ← `just_entered_bull/bear`; exit on `phase_just_changed`.
- Subs 3–4 (breakup/breakdown) ← `just_entered_break_up/down`.
- Subs 5–8 (fades/cells) ← team range geometry (held smoothed; NOT yet causal).

### Honest current-state limits

- Directional subs (1–2) consume the causal direction call but **lose their edge causally** — directional phase-entry has no causal edge in any tested gate formulation (this is the [[trading/rejected/directional-regime-gate-trend-filtering-2026-06|directional-gate rejection]] showing up at the strategy layer). Break subs (3–4) are the part the causal classifier drives well. Range subs (5–8) don't use the causal classifier yet.
- Single symbol (BTC), single window (2024-26, no real bear leg). Daily direction switches only ~17×/2yr → directional entries are rare by design.
- Param re-fitting on the causal layer is overfit-prone (train-val corr −0.189) — **use default params**.

### Upgrade path

Replace this hand-built dual-window filter with the page's online-native methods ([[#score-driven-bocpd|score-driven BOCPD]] / [[#signature-mmd|signature-MMD]]) when the directional axis needs a real detector; extend the script to ETH/SOL (it is symbol-generic); build causal range geometry to bring subs 5–8 onto the causal layer. The measured haircut from swapping smoothed→causal directional gates is below.

## Practitioner real-time proxies — the callable-now baseline {#practitioner-proxies}

Before building a signature detector, there is a class of regime signals that are **already filtered** (published in real time from past data only) and free. They are the baseline any custom classifier must beat. **CoinMarketCap** publishes three that map onto the team's axes:

| CMC index | What it measures | Maps to | Callable live? |
|---|---|---|---|
| **[Fear & Greed Index](https://coinmarketcap.com/charts/fear-and-greed-index/)** (0–100) | Composite sentiment: price momentum (top-10 by mcap), volatility (Volmex BTC/ETH implied-vol indices), BTC/ETH options put/call ratio, + further components | Vol-state + sentiment overlay | ✅ Yes (real-time) |
| **[Altcoin Season Index](https://coinmarketcap.com/charts/altcoin-season-index/)** | Share of top-100 coins outperforming BTC over trailing 90d (≥75% = "altseason") | Internal-class rotation (alt vs BTC) | ✅ Yes (trailing window) |
| **[Bitcoin Dominance](https://coinmarketcap.com/charts/bitcoin-dominance/)** | BTC market cap ÷ total crypto market cap | BTC.D regime marker (already in the [[foundation/macro/btc-regime-taxonomy-2020-2025|taxonomy marker set]]) | ✅ Yes (real-time) |

**Why they matter here, beyond convenience.** These are *honestly filtered* — computed only from past data, published continuously — so they are deployable today as gating inputs, and they set the bar: a bespoke online detector earns its complexity only by beating the Fear & Greed / BTC-Dominance baseline on the *filtered* (not smoothed) backtest. They are available programmatically via the CMC `/v1/global-metrics/quotes/latest` endpoint (global market cap, 24h volume, F&G, altseason gauge, BTC/ETH dominance), which makes them a candidate live-regime data feed.

**Honest caveats** (the reason this is a baseline, not the answer):
- **Composite-index opacity** — the Fear & Greed weighting is proprietary; you cannot fully decompose *why* it moved, and its components (momentum, options put/call) are partly endogenous to price, so it lags genuine regime turns and is reflexive near extremes.
- **Volmex IV dependency** — the volatility component rides Volmex's BTC/ETH implied-vol indices; that is a third-party data dependency with its own methodology.
- **No 7-day directional-range label** — none of the three answers the team's hard question (is *this range* bull or bear at its start). They are sentiment/rotation overlays, not the directional regime classifier.
- **Verdict: CITE-ONLY data source**, not a deep ingest. Use as (a) a live regime-marker feed feeding the [[foundation/macro/btc-regime-taxonomy-2020-2025|taxonomy marker set]], and (b) the benchmark a custom online classifier must outperform on a filtered backtest.

## Operational implications for the team {#operational}

1. **Re-grade the existing detector in filtering mode.** Before any new build: take the team's stable-range / regime_v3 labels, relabel using only data available at each range's start (no forward extremums, no Mann-Kendall over the full gap), and re-run the gated backtests. The smoothed→filtered performance drop is the real live edge. This is a methodology task, not a capability task — runnable now.
2. **Prototype score-driven BOCPD first** on BTC 4h returns (and/or funding/order-flow series) as the lower-lift online detector; compare its filtered regime calls against both the smoothed team labels and the CMC baseline.
3. **Keep the vol-state axis separate and lean on MSGARCH-class one-step-ahead methods** — it's the better-supported, prospectively-callable half.
4. **Treat signature-MMD as the high-ceiling R&D track** — non-parametric, crypto-demonstrated, but a real engineering investment (signature computation + MMD tuning).
5. **Capability-gap framing**: none of these are in the backtesting lab DSL. Per a standing convention, this ingest sharpens *which* capability to buy — and the answer it points to is "online change-point detection on the existing OHLCV/funding feeds," which needs no new data feed (unlike the Deribit/Coinglass gaps), only new code.

## Measured result — smoothed→filtered haircut at the strategy level (2026-06-07) {#measured-haircut}

The open question this page flagged as *"the highest-value next experiment"* — what is the smoothed→filtered haircut on regime_v3 — **is now measured.** Quantified two ways on the SAME BTC 4h window (2024-05 → 2026-05):

- **Label level**: a causal filtering re-label disagrees with the smoothed `team_phase_v2` labels **56%** of the time (`regime_filter_causal.py`). (The daily-label version of this disagreement is 43.5%, reported in the [[trading/rejected/directional-regime-gate-trend-filtering-2026-06|directional-gate rejection]].)
- **Strategy (P&L) level**: regime_v3 with its directional gates swapped smoothed→causal — **Sharpe 1.638 → 1.289** (net $451 → $229; ~21% Sharpe / ~half P&L). Win rate UP 53%→66%, max drawdown DOWN 9.5%→7.8%, profit factor unchanged at 1.29.

**Headline lesson — the edge SURVIVES filtering, but the haircut is real and localizable.** The filtered Sharpe 1.289 sits well above the 0.7 deployment falsifier — the smoothed backtest was *not* mostly lookahead. But the haircut is not a uniform discount: it concentrates **entirely in the directional phase-entry subs** that gated on the smoothed bull/bear labels. The break + cell mechanisms survive cleanly (breakup_entry even *improved*). So the live-deployment haircut is a **per-mechanism diagnostic**, not a single multiplier — directional-label-dependent subs pay it; mechanism-structural subs (breaks, cells, fades) do not.

Per-sub haircut (baseline → causal):

| sub | baseline net / n / WR | causal net / n / WR | read |
|---|---|---|---|
| bull_phase_entry_long | +$170 / 23 / 65% | **−$28 / 4 / 25%** | collapsed — rode the smoothed bull label |
| bear_phase_entry_short | +$120 / 23 / 57% | +$39 / 4 / 75% | survives per-trade, tiny N |
| breakup_entry_long | +$93 / 101 / 36% | +$98 / 23 / 61% | improved — causal break more selective |
| breakdown_entry_short | +$56 / 92 / 34% | +$54 / 43 / 49% | survives, cleaner |
| cell_short_in_anchored | +$34 / 95 / 68% | +$55 / 72 / 74% | stable-to-better |
| cell_long_in_anchored | −$3 / 101 / 70% | +$16 / 96 / 72% | stable |

The directional collapse here is the **strategy-layer image** of the [[trading/rejected/directional-regime-gate-trend-filtering-2026-06|directional-gate rejection]]: the same smoothed→filtered loss that voids the standalone directional CI also voids the directional subs' contribution, while the non-directional mechanisms are untouched.

**Generalizable claim**: any strategy gating on a smoothed/period label should expect the haircut to **localize to the label-dependent components**, not spread uniformly. This is the strategy-level analog of the bar-level overlay finding in [[foundation/methodology/bar-level-validation-overlay]] ("a clean period label hides a deployment haircut concentrated where the label is load-bearing"). Read the haircut per-mechanism, not as an aggregate discount.

**Caveats**: single window (no 2022-23 bear leg yet); params held at smoothed-tuned Run-5649 values (conservative — a causal re-fit could *narrow* the haircut, but is overfit-prone per the baseline detector's train-val corr −0.189); the causal directional subs are tiny-N (4 trades), so the per-sub directional reads are suggestive — the aggregate (1.289 survives) is the robust claim.

Artifacts: `build_causal_4h_gates.py` (gate builder → `data/btcusdt/futures_causal/4h.parquet`); `strategies/regime_v3/regime_v3_causal.py`; `strategies/regime_v3/CAUSAL_RECONDITIONING_{SCOPE,RUNBOOK}.md`; runs `regime_v3_baseline_samewindow` vs `regime_v3_causal_primary`.

## Open questions {#open-questions}

- **What is the smoothed→filtered haircut on regime_v3?** **MEASURED 2026-06-07** (see [[#measured-haircut]]): Sharpe 1.638→1.289 (survives the 0.7 floor); the haircut localizes entirely to directional phase-entry subs, leaving break/cell mechanisms intact. Remaining open: re-measure on a 2022-23 bear leg (single-window caveat), and whether a causal re-fit narrows the directional haircut without overfitting. (Ties to [[foundation/macro/btc-regime-taxonomy-2020-2025#caveats|taxonomy hindsight caveat]] and `questions/index` regime threads.)
- **Does score-driven BOCPD over-segment or under-segment a 7-day BTC range vs the team's detector?** Direct head-to-head pending capability.
- **Can the CMC Fear & Greed index, as a free filtered baseline, be beaten on a filtered backtest by any custom classifier?** If not, the bespoke build isn't worth the complexity — that is the falsifier for this whole research direction.
- **Direction from a non-parametric break detector** — signature-MMD flags *that* a regime changed; extracting bull-vs-bear *direction* from the post-break segment in real time is unspecified and needs design.
- **Cross-market-transfer survival** — does score-driven BOCPD's NASDAQ order-flow result survive translation to crypto's 24/7 retail microstructure? (per [[foundation/market-structure/crypto-carrier-features]] burden of proof).

## Key Takeaways

- **Filtering (callable now) ≠ smoothing (hindsight).** The live-trade gate needs filtering; most clean regime labels — including the team's detector and Viterbi-HMM — are smoothers in disguise. Always ask whether a cited metric is filtered or smoothed.
- **Two online-native methods** fit the team's prospective problem: **signature-MMD** (non-parametric, path-dependent, crypto-demonstrated; high ceiling, high effort) and **score-driven BOCPD** (parametric, real-time, fixes the i.i.d.-within-regime flaw that fragments multi-day ranges; lower lift — prototype first).
- **The vol-state axis is the easy, well-supported half** (MSGARCH-class, one-step-ahead, N=3+ external support); **the 7-day directional-range label is the frontier** the online methods exist for.
- **CoinMarketCap's Fear & Greed / Altcoin-Season / BTC-Dominance** are free, honestly-filtered, callable-now regime proxies — the **baseline a custom detector must beat** and a candidate live-regime feed. Cite-only, not a deep ingest.
- **Capability-blocked, not research-blocked** — the highest-value runnable step needs no new data feed: re-grade the existing regime labels in filtering mode and measure the haircut.

## Related

- [[trading/rejected/directional-regime-gate-trend-filtering-2026-06]] — the empirical filtering-mode falsification this page called for: a trailing-trend directional gate has no prospective edge on BTC; the §measured-haircut and §implemented-baseline sections are its strategy-level and apparatus-level companions
- [[foundation/methodology/team-stable-range-detector-methodology]] — the team's detector; this page names its filtering-mode upgrade path (the detector solves smoothing; live deployment needs filtering)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — the retrospective regime reference; its hindsight caveat is the smoothing problem stated; CMC indices feed its marker set live
- [[foundation/methodology/bar-level-validation-overlay]] — sibling discipline (period-level label wrong at bar level); same family of "the clean label hides a deployment haircut" findings
- [[foundation/methodology/forecast-vs-tradeable-gap]] — the L1→L2 translation-loss gate; smoothed→filtered is the regime-classification analogue
- [[foundation/methodology/strategy-validation-discipline]] — the detector's own hyperparameters (signature level, MMD kernel, BOCPD priors) need this discipline
- [[foundation/sources/issa-horvath-online-regime-detection]] — Method 1 source
- [[foundation/sources/tsaknaki-lillo-mazzarisi-bocpd]] — Method 2 source
- [[foundation/sources/temporal-dynamics-funding-rates]], [[foundation/sources/two-tiered-funding-rate-markets]] — perp-microstructure ingests that pair with the order-flow feature side of BOCPD
- [[foundation/idea-sourcing-methodology]] — score-driven BOCPD is a Category-1 cross-market transfer (NASDAQ → crypto)
- [[foundation/market-structure/crypto-carrier-features]] — the transfer burden of proof
- this research program — the regime-classification workstream this page serves

## Sources

- Issa, Z. & Horvath, B. (2023). *Non-parametric online market regime detection and regime clustering for multidimensional and path-dependent data structures.* arXiv:2306.15835. https://arxiv.org/abs/2306.15835
- Tsaknaki, I.-Y., Lillo, F. & Mazzarisi, P. (2023, rev. 2024). *Online Learning of Order Flow and Market Impact with Bayesian Change-Point Detection Methods.* arXiv:2307.02375. https://arxiv.org/abs/2307.02375
- CoinMarketCap — [Fear & Greed Index](https://coinmarketcap.com/charts/fear-and-greed-index/), [Altcoin Season Index](https://coinmarketcap.com/charts/altcoin-season-index/), [Bitcoin Dominance](https://coinmarketcap.com/charts/bitcoin-dominance/), [API global-metrics endpoint](https://coinmarketcap.com/api/).
- Filtering/smoothing framing: standard state-space estimation (Kalman/HMM forward-filtering vs forward-backward smoothing); applied here to the regime-deployment gap.
- Adjacent support (now ingested as source pages 2026-06-07, the vol-state-axis N=3 convergence): [[foundation/sources/msgarch-crypto-vol-regimes|Markov-Switching GARCH crypto vol-regime cluster]] (Ardia lineage; 292-coin); [[foundation/sources/nhhm-crypto-regime-switching|NHHM 4-state bull/bear/calm on BTC/ETH]] (Koki, Leonardos, Piliouras, *Digital Finance* 2024); [[foundation/sources/almeida-bitcoin-risk-premia|Bitcoin variance-risk-premium regime-conditioning]] (Almeida et al. 2024).
- Live regime-marker data layer (where these markers come from as backtestable series): [[foundation/methodology/regime-marker-data-sources]] (added 2026-06-07).
