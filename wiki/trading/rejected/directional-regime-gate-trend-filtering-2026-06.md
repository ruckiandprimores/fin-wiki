---
title: "directional regime gate (trailing-trend filtering) — REJECTED (no prospective directional edge on BTC, 2024-26) (2026-06)"
status: evergreen
created: 2026-06-07
updated: 2026-06-07
tags: [rejected, regime-detection, filtering-vs-smoothing, directional-gate, falsified, prospective, day-swing]
---

# directional regime gate (trailing-trend filtering) — REJECTED; 2026-06

> **Lead learning**: A prospectively-callable (**filtering-mode**) bull/bear regime label built from a trailing-trend signal carries **no forward directional edge on BTC** — not continuation, not contrarian. The falsifier fired cleanly: every moving-block-bootstrap CI on next-7d return by regime straddles zero (block lengths 7/14/21); the bull−bear spread CI is [−3.47, +1.93]. The apparent bear→+0.74% "inversion" in point estimates is a window-artifact of overlapping forward windows + daily autocorrelation, not a tradeable contrarian signal. This **independently reproduces** a team engineer's stateful-classifier null (mature bull P(up) 0.486 vs bear 0.536, spread ≈ 0). Crucially, what is rejected is the **directional label content**, NOT the apparatus: the filtering mechanism is sound and proven causal (leakage self-test passed), and the companion **vol-state axis is not rejected** (separate axis, N=3+ external support). This is the empirical filtering-mode test that [[../../foundation/methodology/online-regime-detection]] called for in its open questions, and it is the load-bearing evidence behind this research program's "direction is unpredictable — gate state, not side" conclusion.

## What we learned

1. **The directional half of the regime workstream has no prospective edge — confirmed by two independent methods.** The team's gating program (this research program) hinged on classifying multi-day (~7d) ranges as bull/bear to gate directional strategies. Two from-scratch tests, built differently, agree the directional label has zero forward content on BTC:
   - **a team engineer's stateful causal classifier** (the backtesting lab): mature bull P(up)=0.486 vs bear 0.536 — mildly *inverted*; bull−bear forward-4h spread ≈ 0 vs shuffle null.
   - **This session's from-scratch daily filter** (`regime_filter_causal.py`) + block-bootstrap CIs (`regime_filter_forward_ci.py`): every forward-return CI straddles 0 at block lengths 7/14/21.

   The convergence of two independent designs onto the same null is the strength of the result — it is not one detector's idiosyncratic failure.

2. **The "inversion" was overlapping-window noise, not a contrarian edge.** Naive point estimates put bear forward-7d at +0.74% (vs bull +0.04%), which looks like a fade-the-regime signal. Under a moving-block bootstrap that respects window overlap + daily autocorrelation, the bear mean's P(>0) ≈ 0.77 — far short of significance. Tempting "the regime label works inverted" reads die here; this is a textbook case of overlapping forward windows manufacturing apparent edge.

3. **Filtering ≠ smoothing, measured.** The causal filtered labels agree with the smoothed `team_phase_v2` daily labels only **43.5%** of the time — the live-deployment haircut on the smoothed directional labels is large (consistent with the strategy-level haircut measured separately on regime_v3; see [[../../foundation/methodology/online-regime-detection#measured-haircut]]). A backtest gated on the smoothed labels reads tomorrow's newspaper; the filtered version is what deploys, and the directional content does not survive the swap.

4. **Reject the label content, keep the apparatus.** This is the discriminating distinction. The filtering skeleton (causal trend t-stat + hysteresis + leakage self-test) is the correct machinery for any live regime gate — it just should not be asked to call *direction*. Re-scope it from "directional gate" to "causal vol-state + context emitter." See [[#what-survives]].

## Hypothesis (Structured Format)

```
Hypothesis:    A regime classifier that, at each bar, calls the multi-day
               (~2-week) trend direction from PAST DATA ONLY (filtering
               mode) can gate directional strategies — go long in
               bull-labelled regimes, short in bear, stand aside in
               neutral.

Mechanism:     Trend persistence. If a trailing ~2-week trend is up, the
               next ~week continues up often enough to gate longs (and
               symmetrically for bear). Recognise the established regime
               with acceptable lag (Livermore "identify the trend, sit
               tight") — do not predict it.

Observable:    BTCUSDT daily, 2024-05-18 → 2026-05-08 (721 bars). Causal
               label = 3-state hysteresis on trend_t = mean/std of last
               14 daily log-returns (ENTER 0.18 / EXIT 0.06). Forward
               metric: next-7d log-return by regime. Leakage self-test:
               each label recomputed on df[:t+1] and asserted unchanged
               (PASSED).
               expected_net_edge:
                 - source: literature on trend persistence implies
                   bull-labelled fwd return > bear-labelled
                 - value: spread CI excluding 0 (directional-gate viability
                   floor)
                 - independence: forward-window metric is out-of-label by
                   construction; block bootstrap respects autocorrelation

Expected:      bull fwd-7d > 0, bear fwd-7d < 0, bull−bear spread CI > 0.

Falsifier:     If the bull−bear forward-return spread CI includes 0
               (and/or neither directional label's CI excludes 0) under a
               moving-block bootstrap that respects window overlap +
               autocorrelation, the directional gate has no prospective
               edge.
```

## Falsifier evidence {#falsifier-evidence}

BTC daily 2024-05 → 2026-05; moving-block bootstrap, B=5000.

Point estimates (naive, overlapping windows): bull +0.04% / neutral −0.19% / bear +0.74%; bull−bear −0.70%.

| Quantity (next-7d %) | Point | 95% CI (block=14d) | Excludes 0? |
|---|---:|---|:---:|
| bull | −0.02 | [−1.76, +1.58] | no |
| neutral | −0.22 | [−1.56, +1.12] | no |
| bear | +0.78 | [−1.28, +2.82] | **no** |
| bull − bear | −0.79 | [−3.47, +1.93] | **no** |
| bear − neutral | +1.00 | [−1.26, +3.08] | no |

Stable across block lengths 7 / 14 / 21 — not a block-length artifact. P(>0) for the bear mean ≈ 0.77 (≪ 0.975).

**Filter behaviour**: 234 bull / 284 neutral / 203 bear bars; 94 regime switches; median run 5 d (twitchy for a "2-week" regime — the lag-vs-whipsaw band is untuned). Smoothed-vs-filtered agreement vs `team_phase_v2` (daily): **43.5%**.

**Verdict: FIRED.** The bull−bear spread CI straddles 0 at every block length; no individual directional label's CI excludes 0. No prospective directional edge — in either direction. The contrarian residual was overlapping-window noise.

## What is NOT rejected {#what-survives}

- **The filtering apparatus** — causal, leakage-tested; the correct skeleton for any live regime gate. Re-scope `regime_filter_causal.py` from "directional gate" to "causal vol-state + context emitter." The dual-window build of this same apparatus is documented as the reusable baseline in [[../../foundation/methodology/online-regime-detection#implemented-baseline]].
- **The vol-state axis** (calm / high_vol / panic) — a *separate* axis, prospectively callable one-step-ahead, with N=3+ external support (MSGARCH 292-coin; NHHM 4-state; Bitcoin variance-risk-premium). See [[../../foundation/methodology/online-regime-detection#method-comparison]].
- **Regime as context / stability gate + position-lifecycle exits** — a team engineer's clearest edges (shorts-into-panic exit; compression→expansion wait-for-confirmation) need no direction oracle. The conclusion is *gate state and risk, not side* (this research program Path C).
- **Break/event mechanisms** — the causal classifier drives break (vol-expansion) subs well; it is only the directional phase-entry subs that lose edge (see the per-mechanism haircut in [[../../foundation/methodology/online-regime-detection#measured-haircut]]).

## Graveyard reverence — the re-proposal bar {#re-proposal-bar}

To re-propose a directional regime gate, a future session must articulate what changed:

1. **A method that does NOT assume i.i.d.-within-regime / trend-continuation** — score-driven BOCPD or signature-MMD per [[../../foundation/methodology/online-regime-detection]], not another trailing-trend hysteresis variant (which is the falsified design here).
2. **Multi-symbol evidence** (BTC + ETH + SOL, per-symbol) AND a real **bear-window** leg — this test is single-symbol, single-window (2024-26, no sustained bear).
3. **A forward spread CI that excludes 0** under a window-overlap-respecting bootstrap.
4. **Beating the free CMC Fear & Greed filtered baseline** ([[../../foundation/methodology/online-regime-detection#practitioner-proxies]]) — the bespoke directional build earns its complexity only by clearing the callable-now baseline on a *filtered* backtest.

Absent all four, the directional gate stays in the graveyard. This is the same discipline that the N≥7 state-type bear-direction failure stack enforces ([[cell-distribution-short-btc-2026-05#cross-symbol-pattern-context]]) — direction-flipping and direction-gating both keep failing prospectively; the productive ground is event-type and state/risk gating, not directional state labels.

## Methodological notes {#methodology-notes}

- **Overlapping-window forward metrics need block bootstrap, always.** The +0.74% bear point estimate would have passed a naive t-test and seeded a "fade the regime" strategy. The moving-block bootstrap (respecting both the 7-day window overlap and daily return autocorrelation) is what kills the false positive. Any forward-return-by-bucket analysis with overlapping windows must use this — a per-bucket mean ± naive SE is structurally overconfident.
- **Two-independent-method convergence is the codification trigger.** Single-detector nulls are weak (could be that detector's design fault). The match between a team engineer's stateful classifier and this from-scratch daily filter is what promotes "no prospective directional edge" from one experiment to a worldview-grade claim — direction-unpredictability is now treated as codification-ready.
- **Reject the content, name what survives.** A rejected entry that only says "X failed" loses the apparatus. The discriminating work here is separating the *directional label* (rejected) from the *filtering skeleton* and the *vol-state axis* (both preserved) — so the next session rebuilds from the surviving parts, not from scratch.

## Provenance / artifacts

- the backtesting lab — causal filter + leakage self-test; output `data/btcusdt/a derived dataset.
- the backtesting lab — block-bootstrap CI table.
- the backtesting lab — a team engineer's independent reproduction.

## Related

- [[../../foundation/methodology/online-regime-detection]] — filtering vs smoothing; this rejection is the empirical filtering-mode test its open questions called for, and its §measured-haircut + §implemented-baseline are the sibling results
- [[../../foundation/methodology/bar-level-validation-overlay]] — sibling "clean label hides a deployment haircut" finding at the bar level; the 43.5% smoothed-vs-filtered agreement here is the regime-label analog
- [[cell-distribution-short-btc-2026-05]] — N≥7 state-type bear-direction failure stack; direction-flipping fails the same way direction-gating does here
- [[../mechanisms/regime-gate-vs-bias-filter]] — regime as conditioning attribute (state/risk), not direction oracle — the surviving use
- [[../../foundation/macro/btc-regime-taxonomy-2020-2025]] — the retrospective (smoothed) regime reference; this is the prospective-mode falsification of its directional gating use
- this research program — the "direction unpredictable; gate state, not side" conclusion this evidence anchors

## Sources

- Block-bootstrap CI table: the backtesting lab output (B=5000, blocks 7/14/21).
- Independent reproduction: the backtesting lab (a team engineer).
- Filtering/smoothing framing: [[../../foundation/methodology/online-regime-detection]] (2026-06-07).
