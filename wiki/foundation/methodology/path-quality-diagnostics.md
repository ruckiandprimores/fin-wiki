---
title: "Path-Quality Diagnostics — when aggregate metrics pass but the equity curve is lottery-shaped"
status: growing
created: 2026-05-13
updated: 2026-05-13
tags: [foundation, methodology, deployment-decision, path-quality, event-concentration, val-period-scrutiny, lottery-shape, distributed-edge, cell-carving, day-swing]
---

# Path-Quality Diagnostics — when aggregate metrics pass but the equity curve is lottery-shaped

> **TL;DR**: Aggregate Sharpe / Calmar / max_dd / cohort-positivity do **not** surface path-quality. Two strategies with identical aggregate metrics can be distributed-edge (100 small winners covering steady losses) vs lottery-shape (5 event-driven outliers covering 100+ losers). Path-quality is an **independent deployment-risk dimension** alongside statistical significance ([[foundation/methodology/strategy-validation-discipline]]) and economic significance ([[foundation/methodology/deployment-edge-threshold]]). This page documents the 4-filter diagnostic catalog surfaced by the 2026-05-12 fresh-eye audit of the v6 BTC prod deployment claim: (1) **event-concentration** (Top-1 / Top-5 % of total P&L); (2) **per-sub contribution PASS** (every sub net-positive on its own gate cell — falsifies cosmetic cell-carving); (3) **split-timing scrutiny** (train/val splits falling on regime boundaries produce artifact verdicts); (4) **val-period cohort dispersion** (replaces full-window `quantile_curves` endpoint). **The v6 BTC prod walk-back is the canonical worked example**: Top-5 trades = 224% of total P&L; `vol_transition` sub carries the entire $656 edge; `range_fade` is a net drag (−$100); `vol_expansion_short` is ~$0 noise. Aggregate metrics (Sharpe 1.70, dd 29%) looked deployment-ready but the equity-curve path is event-driven, not distributed. **Origin**: 2026-05-12 fresh-eye review triggered by review-raised concern about v6 BTC prod equity curve; load-bearing methodology reframe per this research program 2026-05-12 reframe. **Status: growing** — N≥3 evidence on event-concentration; N=2 on split-timing + val-period scrutiny; N=1 first PASS on per-sub. Codification reflects substrate-load-bearing status, not full validation.

## 1. The Gap — aggregate metrics don't surface path-quality

The lab's standard validation discipline gates on:

| Gate | Diagnostic | What it catches |
|---|---|---|
| Statistical significance | LdP CSCV / DSR / corr(train,val) / cohort-positive-ratio / quantile transfer Q1→Q5 | Overfitting; trial multiplicity; noise-fitting |
| Economic significance | Net-of-friction edge at deployment scale | Gross-edge that disappears at realistic friction |
| Translation-loss | L1 forecast effect / stop ratio; horizon match; bar-mechanics compatibility | Effect-magnitude too small for the trade mechanics |

These gates are necessary but **not sufficient** for deployment-decision. A strategy can pass all three and still be a lottery-shape — e.g.:

- 167 trades over 2 years, 30 winners (17.9% WR), of which 5 trades carry 224% of total P&L and the remaining 162 trades net to a loss
- Aggregate Sharpe 1.70, max_dd 29% — looks tradeable at the summary-statistic level
- But the equity-curve is 5 spikes punctuating a downward drift. Deployment risk is dominated by "did those 5 events fire?" rather than "is the mechanism real?"

The standard diagnostics are aggregate-summary-statistic shaped; path-quality is a distributional property of the trade sequence. **Path-quality + cohort-dispersion + event-concentration are independent deployment-risk dimensions** that need their own diagnostics.

## 2. The Four Path-Quality Filters

### 2.1 Event-concentration (Top-1 / Top-5 % of total P&L) {#event-concentration}

**Diagnostic**: for the full trade sequence in a prod_run (or per-config in a grid), compute:

- `pct_top_1` = (largest single trade P&L) / (total P&L)
- `pct_top_5` = (sum of top-5 trade P&L by absolute magnitude) / (total P&L)

**Thresholds** (calibrated on the 2026-05-12 audit; treat as initial brackets, revise as N grows):

| `pct_top_5` | Interpretation | Deployment posture |
|---|---|---|
| < 30% | **Distributed-edge** — many small winners; equity-curve robust to single-event failure | `ship` candidate (pending other filters) |
| 30–60% | **Mid-band** — moderate concentration; deployment-tolerable with event-risk caveat | `ship` with event-risk caveat; consider risk-parity sizing against other strategies |
| 60–100% | **High concentration** — most P&L from a handful of trades | `ship-research-grade-only`; do not size up |
| > 100% | **Lottery-shape** — top-5 trades exceed total P&L (remaining trades net negative) | Downgrade to `ship-research-grade-only` regardless of aggregate metrics; binary "did those events fire" risk |

**Why a threshold above 100% is meaningful**: when `pct_top_5 > 100%`, the remaining (n − 5) trades have negative net P&L. The strategy is **structurally** a "wait for the rare event and survive the drag" pattern — deployable only if the operator (a) has high conviction the event will recur in the deployment window, and (b) can sit through the drag without intervening. Most the backtesting lab strategies are not designed for this shape.

**Empirical evidence (N=3 lottery + 1 mid-band as of 2026-05-13)**:

| Strategy | `pct_top_1` | `pct_top_5` | Shape verdict |
|---|---:|---:|---|
| meta_vol_btc_prod_v1 (v6 BTC) | 55% | **224%** | Lottery |
| meta_vol_sol_prod_v1 (v6 SOL) | — | **210%** | Lottery |
| meta_vol_eth_prod_v1 (v6 ETH) | — | high | Likely-lottery (inherits event-concentration shape per substrate) |
| meta_post_cascade_btc_prod_v1 | — | 84% | Mid-band (deployable with caveat) |

See §3.1 + §3.2 for worked examples.

### 2.2 Per-sub contribution PASS — cell-carving falsifier {#per-sub-pass}

**Discipline (per [[trading/mechanisms/scaffold-as-mechanism]] §cell-carving)**: a meta-strategy's claim that cell-carving "diversifies edge across sub-strategies" is only valid if **every active sub-strategy is net-positive on its own gate cell**. Any sub with cohort-negative P&L on its own regime cell is drag, not a diversifier — the cell-carving aggregation claim is **cosmetic** in that case.

**Operationalization**: requires per-trade `sub_strategy` tagging in the engine output. Prod_run mode emits this column today; grid-run mode does NOT (a team engineer blocker per this research program engine output layer).

**Two failure shapes**:

- **Cosmetic cell-carving** — aggregate P&L positive but driven entirely by 1 sub; other subs are drag or noise. The "diversification" framing is wrong; the meta-strategy is a single-sub strategy with overhead.
- **Mechanism-coherent cell-carving** — every sub net-positive on its own gate. The diversification is real; the architecture is justified.

**Empirical evidence as of 2026-05-13**:

| Strategy | Sub 1 | Sub 2 | Sub 3 | Verdict |
|---|---:|---:|---:|---|
| meta_vol_btc_prod_v1 | vol_transition +$656 | range_fade **−$100** | vol_expansion_short ~$0 | **Cosmetic** — 1-sub strategy with 2-sub overhead |
| meta_vol_sol_prod_v1 | vol_transition (+) | range_fade_long **−$37** | — | Cosmetic |
| meta_post_cascade_btc_prod_v1 | vol_spike_reversal +$158 | bearish_capitulation_long +$58 | bullish_exhaustion_short +$53 | **PASS** — first PASS in lab history |

The meta_post_cascade per-sub PASS (33 + 17 + 21 = 71 trades distributed across 3 subs; total +$269) is the first empirical evidence that cell-carving can satisfy the filter — see §3.2 + [[trading/mechanisms/scaffold-as-mechanism#per-sub-pass-2026-05-13|scaffold-as-mechanism §per-sub PASS]].

### 2.3 Split-timing scrutiny — regime-boundary artifacts {#split-timing}

**Diagnostic**: when corr(train,val) reads anti-predictive or noisy on a working cohort (cohort-positive ≥ 80%, mean Sharpe > 0), check whether the train/val split timestamp falls on or near a regime boundary. If it does, the anti-predictive verdict may be a **split-timing artifact** rather than a real "untunable" finding.

**Mechanism**: train half learns the parameter optimum for the regime that dominates the train sample; val half is dominated by a different regime; parameter optima genuinely shift between regimes; corr(train,val) reads anti-predictive — but the cohort is still positive because the mechanism is real in both regimes, just under different parameter optima.

**Resolution path**: re-run on a longer window that spans multiple regime cycles (2y multi-cycle minimum for crypto). If corr(train,val) recovers to positive and a productive parameter zone emerges, the original verdict was split-timing artifact.

**Empirical evidence (N=2 as of 2026-05-13)**:

| Strategy | Original window + verdict | Re-run | Re-run verdict |
|---|---|---|---|
| meta_post_cascade_v1 | 1y 2025-05/2026-04 70/30 split; corr −0.598 → "anti-predictive ranking, untunable" | 2y 2024-05/2026-05 multi-cycle | **87.4% cohort positive, corr +0.593, top Sharpe 2.54, dd 11.4%, $269/2y; productive zone [stop 0.9-1.5] cleanly tunable. Verdict REVERSED.** |
| (Watch list — recurrence) | — | — | — |

The meta_post_cascade reversal is the canonical case. The 1y window with 70/30 split fell on a regime boundary between bearish-Q4-2025 and the post-Jan-2026 recovery; parameter optima legitimately shifted; 2y multi-cycle dissolved the artifact.

**Discipline**: anti-predictive corr on a working cohort (cohort-positive but corr negative) should trigger split-timing scrutiny before "untunable" verdict closes the strategy. Watch for N≥2 recurrence before promoting from N=1 to standard verdict-shape filter.

### 2.4 Val-period cohort dispersion {#val-period-dispersion}

**Diagnostic**: instead of (or in addition to) full-window `quantile_curves.csv` endpoint as the cohort-stationarity signal, segment the val period and check cohort dispersion within the val window. A strategy with full-window cohort positive but val-period cohort dispersed across positive/negative regions may have a closed mechanism that revived (or vice versa).

**Mechanism**: full-window cohort positivity averages over regimes. If the val period contains a regime shift from "mechanism closed" to "mechanism active", the val-half cohort positivity rises relative to the full window — and conversely if the regime shifts toward closure.

**Operational pattern**: when a strategy reads as "closed" on full-window verdict, segment the val window further. If a sub-window within val shows cohort revival (e.g., 87% positive + corr +0.5), the closure verdict may be over-stated.

**Empirical evidence (N=2 closure-reversals as of 2026-05-13)**:

| Strategy | Original verdict | Val-period dispersion finding | Revised verdict |
|---|---|---|---|
| meta_funding_v1 | "Family closed at standalone form for this era" (2026-05-11 batch) | Val window 2026-01-10 → 2026-04-28 (~3.5 months) shows 87% positive + corr +0.506 | **Mechanism revived in early-2026 as ETF cash-and-carry distortion normalized**; closure verdict reversed; treat as regime-conditioned, not closed |
| (Watch list — recurrence) | — | — | — |

**Generalizable pattern**: closure verdicts need val-period scrutiny. A "this mechanism is closed in modern crypto" verdict from a 1y window may be conflating closure with regime-conditioning that resolves within the same year. See [[trading/mechanisms/etf-era-carry-trade-funding-distortion#meta-funding-revival-2026-05-13|etf-era §revival]] for the worked example.

## 3. Worked Examples

### 3.1 v6 BTC prod — lottery shape (the 2026-05-13 walk-back) {#v6-btc-prod-lottery-shape-the-2026-05-13-walk-back}

The 2026-05-11 PM substrate framed meta_vol_btc_prod_v1 as the lab's "first deployment-ready strategy package": $556/2y on $1K, Sharpe 1.70, max_dd 29%, 2y window 2024-05-18 → 2026-05-08. Wiki content was landed on [[trading/mechanisms/vol-regime-transition-tradeable]] reflecting this framing.

**The 2026-05-12 fresh-eye audit walked the claim back**:

| Path-quality finding | Value |
|---|---:|
| Total trades | 167 |
| Winners | 30 (17.9% WR) |
| Top-1 trade % of total P&L | 55% |
| Top-5 trades % of total P&L | **224%** |
| `vol_transition` sub contribution | +$656 |
| `range_fade` sub contribution | **−$100 (net drag)** |
| `vol_expansion_short` sub contribution | ~$0 (noise) |

The aggregate metrics (Sharpe 1.70, dd 29%) looked deployment-ready. **The path is event-driven, not distributed**: 5 event trades carry the entire P&L plus cover ~$100 of drag from the remaining 162 trades. Cell-carving "diversification" is COSMETIC — `vol_transition` carries the entire edge.

**Verdict revision**: meta_vol_btc_prod_v1 is `ship-research-grade-only` regardless of aggregate metrics. Cell-carving claim fails the §2.2 filter. Deployment risk is dominated by event-recurrence ("will the same vol-transition events fire in deployment?") rather than mechanism integrity.

N=2 confirmation on SOL prod (Top-5 = 210%, `range_fade_long` = −$37) and N=1 inference on ETH prod (low parameter risk but inherits the event-concentration shape per substrate).

### 3.2 meta_post_cascade — first per-sub PASS + mid-band concentration caveat {#meta-post-cascade-first-per-sub-pass-mid-band-concentration-caveat}

meta_post_cascade_btc_prod_v1 (locked single-config stop=0.9444, target=3.78, risk=0.01; selected from 500-config grid top region; Sharpe 2.54, dd 11.4%, $269/2y on $1K) is the lead deployment candidate as of 2026-05-13 per this research program.

**Per-sub PASS** (the first in lab history):

| Sub | Trades | Total P&L | Avg P&L | Win Rate | Contribution % |
|---|---:|---:|---:|---:|---:|
| vol_spike_reversal | 33 | +$158.18 | $4.79 | 30.3% | 58.9% |
| bearish_capitulation_long | 17 | +$57.80 | $3.40 | 41.2% | 21.5% |
| bullish_exhaustion_short | 21 | +$52.73 | $2.51 | 38.1% | 19.6% |

All 3 subs net-positive on their own gates → filter §2.2 **PASSES**. Cell-carving is mechanism-coherent, not cosmetic.

**Mid-band event-concentration caveat**: Top-5 trades = 84.3% of total P&L. NOT cleanly distributed-edge. Strategy clears 3 of 4 Move-3 filters (per-sub PASS ✓; split-timing scrutiny ✓; val-period dispersion ✓ on the 2y revival) but mid-band concentration is the binding caveat. Deployment posture per §2.1: `ship` with event-risk caveat; consider risk-parity sizing if combined with other strategies.

**Side-asymmetry sub-finding**: long $82 / short $186 (short side carries 69% of edge despite only 15% more trades). Consistent with the 2025-26 ETF-era directional asymmetry per [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — even on a non-funding mechanism, the directional asymmetry of the 2024-2026 window manifests.

**Final verdict pending prod_run with event-concentration filter applied**: per substrate, `pct_top_5 < 60%` → `ship`-promotable; `[60%, 100%]` → ship with event-risk caveat; `> 100%` → downgrade. Move-3 regime-suite-preference filter currently caps single-mechanism at `ship-research-grade-only` regardless; full `ship` requires v2 architecture OR explicit 1-cell-v2-override.

### 3.3 meta_funding — val-period scrutiny revives a "closed" verdict

meta_funding_v1 was verdicted "family closed at standalone form for this era" in the 2026-05-11 v2 batch per this research program prior framing — full-window cohort 0.4% positive on fade-extreme-positive-funding; both long_pnl and short_pnl negative; pre-registered falsifier fired on 3 of 4 criteria.

**Val-period dispersion finding (2026-05-13)**: segmenting the val window (2026-01-10 → 2026-04-28, ~3.5 months) shows 87% positive + corr +0.506 — mechanism revived in early 2026, likely as ETF cash-and-carry distortion normalized (per [[trading/mechanisms/etf-era-carry-trade-funding-distortion]]).

**Verdict revision**: closure verdict reversed; treat as regime-conditioned (closed in mid-2025-bearish + institutional-bid; revived in early-2026 normalization) rather than mechanism-closed. The 2021 OOS test described in the etf-era page remains load-bearing for the regime-conditioned-vs-closed disambiguation, but the val-period revival already shifts the prior away from closed.

**Generalizable pattern**: closure verdicts need val-period scrutiny. A "this mechanism is closed in modern crypto" verdict from a 1y window may be conflating closure with regime-conditioning that resolves within the same year. The check is cheap (segment val and re-compute cohort + corr); the cost of skipping it is permanently archiving a working mechanism. Standard discipline: when verdicting closure, document the val-period dispersion as well.

## 4. Connection to the deployment-decision gate stack {#connection-to-deployment-decision-gate-stack}

The wiki's deployment-decision discipline now stacks five gates (mechanism articulation upstream of all four computed gates):

| Gate | Methodology page | What it catches |
|---|---|---|
| **0. Mechanism articulation** | the research program Structured Hypothesis | Empty / unfalsifiable / nonsense ideas |
| **1. Statistical significance** | [[foundation/methodology/strategy-validation-discipline]] | Overfitting; multiplicity; noise-fitting |
| **2. Translation-loss** | [[foundation/methodology/forecast-vs-tradeable-gap]] | L1 effect / stop ratio < 3; L2 mechanics incompatible with L1 |
| **3. Economic significance** | [[foundation/methodology/deployment-edge-threshold]] | Net-of-friction below floor at deployment scale |
| **4. Path-quality** | **this page** | Aggregate-metrics pass but lottery-shape / cosmetic cell-carving / split-timing artifact / val-period mis-read |

Each gate is *necessary*; **none is sufficient**. Path-quality is the gate that catches "your strategy passed statistical + economic + translation but the equity curve is 5 events covering 162 losers" — independent failure mode from the other three.

A strategy must pass all five gates for `ship` verdict. Failure at gate 4 downgrades to `ship-research-grade-only` regardless of gates 0-3.

## 5. Open Questions

- **Event-concentration threshold calibration**. The §2.1 thresholds (30/60/100%) are calibrated on the 2026-05-12 audit. As N grows, the boundaries may shift — particularly the mid-band (30-60%) which depends on the operator's tolerance for event-risk and the diversification strategy of the portfolio.
- **Path-quality + capacity interaction**. Event-driven strategies have asymmetric capacity profiles — the 5 event trades may have liquidity constraints not captured in aggregate friction estimates. Capacity-conditioned path-quality is unexplored.
- **Multi-strategy path-quality composition**. If two lottery-shape strategies have *uncorrelated* event timings, the combined portfolio may be distributed-edge even though each strategy alone is event-driven. Quantifying cross-strategy event correlation is the unblock for "lottery-shape strategies as portfolio components" framing.
- **Pre-deployment event-concentration prediction**. Currently the filter is computed *post-hoc* from prod_run trades. Can event-concentration be predicted from mechanism class alone (e.g., regime-transition mechanisms are inherently event-driven; positioning-extreme mechanisms are more distributed)?
- **Split-timing scrutiny as standard discipline**. Currently N=2 (meta_post_cascade reversal). Watch for recurrence before promoting from worked-example to standard verdict-shape filter; consider whether 2y multi-cycle should become the default window rather than 1y + manual scrutiny.

## Key Takeaways

- Aggregate Sharpe / Calmar / max_dd / cohort-positivity do NOT surface path-quality. Path-quality is an independent deployment-risk dimension.
- **Event-concentration filter** (§2.1): `pct_top_5` is the headline number; > 100% = lottery; 60-100% = high; 30-60% = mid-band; < 30% = distributed.
- **Per-sub contribution filter** (§2.2): cell-carving is cosmetic unless every sub is net-positive on its own gate. First PASS in lab history is meta_post_cascade_btc_prod_v1 (2026-05-13).
- **Split-timing scrutiny** (§2.3): anti-predictive corr on a working cohort should trigger split-timing check before "untunable" verdict; meta_post_cascade verdict reversal is the canonical case.
- **Val-period cohort dispersion** (§2.4): closure verdicts need val-period scrutiny; meta_funding revival via 3.5-month val window is the canonical case.
- The **v6 BTC prod walk-back** is the canonical worked example for all 4 filters (lottery + cosmetic; passed split-timing + val-period scrutiny but failed event-concentration + per-sub).
- Path-quality is the **fifth deployment-decision gate**; failure at this gate downgrades to `ship-research-grade-only` regardless of the other gates passing.
- Codification status: N≥3 evidence on event-concentration; N=2 on split-timing + val-period scrutiny; N=1 first PASS on per-sub. Page reflects substrate-load-bearing status, not full validation.

## Related

- [[foundation/methodology/strategy-validation-discipline]] — Gate 1 (statistical significance); LdP framework
- [[foundation/methodology/deployment-edge-threshold]] — Gate 3 (economic significance); net-of-friction floor
- [[foundation/methodology/forecast-vs-tradeable-gap]] — Gate 2 (translation-loss); L1 → L2 → L3 layer model
- [[trading/mechanisms/scaffold-as-mechanism]] — Cell-carving discipline; canonical home for per-sub PASS empirical evidence
- [[trading/mechanisms/vol-regime-transition-tradeable]] — v6 BTC prod walk-back lives here
- [[trading/mechanisms/liquidation-cascade-aftermath]] — meta_post_cascade verdict reversal (split-timing scrutiny worked example)
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — meta_funding revival (val-period scrutiny worked example)
- [[foundation/macro/regime-conditional-edge-2025-26]] — companion empirical finding on regime-concentration of edge; relevant to split-timing scrutiny
- [[foundation/methodology/sweep-validation-pattern-taxonomy]] — sibling "aggregate metric misleads" lens at the sweep-selection layer (train-val structure); both read past a single summary number
- [[foundation/methodology/backtest-analysis-diagnostics]] — sibling backtest-interpretation diagnostics (exit-reason decomposition, asymmetric-N overlap, ablation baseline)

## Sources

- **Origin**: 2026-05-12 fresh-eye review of v6 BTC prod equity curve (review-raised concern), substrate write-up in this research program 2026-05-12 methodology reframe
- **Diagnostics**: codified into the research process §14 (research process-internal); this page is the wiki-substrate counterpart for the methodology layer (per a standing convention: wiki encodes substrate, research process-arm operational discipline is separate)
- **Empirical evidence**:
  - the backtesting lab — v6 BTC prod path-quality audit
  - the backtesting lab — v6 SOL prod path-quality audit
  - the backtesting lab §"2026-05-13 prod_run analysis" — per-sub PASS table + side-asymmetry sub-finding
  - 2026-05-13 meta_post_cascade 2y multi-cycle re-run (87.4% cohort positive; corr +0.593) — split-timing scrutiny worked example
  - 2026-05-13 meta_funding val-window dispersion (87% positive on 2026-01-10 → 2026-04-28 sub-window) — val-period scrutiny worked example
