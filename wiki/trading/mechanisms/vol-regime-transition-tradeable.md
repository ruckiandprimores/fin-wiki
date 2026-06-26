---
title: "Volatility Regime Transition (Tradeable) — first non-bias-scaffold mechanism"
status: growing
created: 2026-05-06
updated: 2026-05-13
tags: [mechanism, crypto-native, garch-grounded, non-htf-bias-scaffold, phase-b-novelty-validated, vol-regime, regime-switching, day-swing, oos-pending, cohort-positive-evidence]
---

# Volatility Regime Transition (Tradeable) — first non-bias-scaffold mechanism

> **TL;DR**: First **non-HTF-bias-scaffold mechanism** in the lab inventory with cleanly orthogonal trade-timing signal. `vol_regime_transition_4h` PASSES its pre-registered novelty falsifier (corr <+0.4 with all 5 bias-cluster baselines). Mechanism story: GARCH-grounded regime-switching (Engle 1982 / Bollerslev 1986 / Hamilton 1989/1990) operationalized as compressed-vol → expanded-vol transitions. **Status: growing** (upgraded from seedling 2026-05-11 based on `meta_vol_v1` 99.6% cohort full-positive + train→val corr +0.886 + monotone Q1→Q5 transfer). **2026-05-11 PM v6 batch produced 3-symbol production-candidate portfolio** with asset-vol-scaling N=3 anchor (BTC vt.stop ≈ 0.13 / ETH ≈ 0.31 / SOL ≈ 0.32) — mechanism translation is structural across symbols with asset-scaled parameters. **The v6 "first deployment-ready strategy package" framing was WALKED BACK 2026-05-13 per path-quality audit**: Top-5 trades = 224% of total P&L on BTC prod; cell-carving cosmetic (`vol_transition` carries entire $656 edge; `range_fade` is net drag −$100); strategy verdicts now `ship-research-grade-only`. The mechanism's positive-cohort evidence at meta-strategy form is preserved; what walked back was the lottery-shape path-quality of the specific v6 production candidates. **Lab's lead deployment candidate redirected to [[trading/mechanisms/liquidation-cascade-aftermath|meta_post_cascade_btc_prod_v1]]** (Sharpe 2.54, dd 11.4%, first per-sub PASS in lab history). 2024 OOS (March ATH break + August yen-carry-unwind) remains load-bearing for deployment-grade promotion of the vol-regime mechanism specifically.

## TL;DR

- **First non-HTF-bias-scaffold mechanism in lab** with cleanly orthogonal signal.
- **Pre-registered novelty falsifier PASSES**: corr <+0.4 with all 5 baselines (max +0.327 with `confluence_trend_4h`).
- **GARCH-grounded mechanism story** — regime-switching from compressed to expanded vol regime captures sustained directional moves in HTF-bias direction.
- **Status: growing** (upgraded from seedling 2026-05-11 based on `meta_vol_v1` cohort evidence: 99.6% of 500 configs full-positive; train→val corr +0.886; strict monotone Q1→Q5).
- **Asset-vol-scaling N=3** (2026-05-11 PM): BTC vt.stop ≈ 0.13 / ETH ≈ 0.31 / SOL ≈ 0.32 — productive vt.stop scales ~2.5× from BTC to alts.
- **v6 production-candidate batch (2026-05-11 PM)** assembled deployment-package framing — **WALKED BACK 2026-05-13** per path-quality audit (Top-5 = 224% of total P&L on BTC prod; cell-carving cosmetic). All three (BTC + ETH + SOL prod) are `ship-research-grade-only`. The mechanism-level cohort evidence is unchanged; the specific v6 production strategies failed Gate 4 path-quality.
- **Lead deployment candidate is now [[trading/mechanisms/liquidation-cascade-aftermath|meta_post_cascade_btc_prod_v1]]** (different mechanism family; first per-sub PASS in lab history).
- **Caveats requiring further validation**: 2024 OOS still load-bearing for mechanism deployment-grade promotion; v6-cohort path-quality issues need v2.1-architecture-grade rework to surface a deployment-ready vol-regime strategy.

## Mechanism story (GARCH regime-switching)

### Theoretical grounding

Volatility regime-switching is a well-documented econometric phenomenon:

- **Engle (1982)** — *Autoregressive Conditional Heteroskedasticity (ARCH)*: returns exhibit volatility clustering — periods of low volatility cluster together, periods of high volatility cluster together. Volatility itself is autoregressive.
- **Bollerslev (1986)** — *Generalized ARCH (GARCH)*: extends Engle with lagged variance terms; standard model for volatility prediction and clustering.
- **Hamilton (1989, 1990)** — *Markov-switching* models: explicit two-regime (low-vol / high-vol) framework where regime is a hidden state with probabilistic transitions; Hamilton 1990 specifically applied to financial time series.
- **Catania & Grassi (2017)** — *Modelling Crypto-Currencies Financial Time-Series*: documents GARCH-style regime-switching in crypto, with stronger regime persistence and sharper transitions than equity GARCH.

### Operationalization (compressed → expanded transition)

The strategy pre-registers a tradeable form of regime-switching:

1. **Compressed-vol regime detection**: vol_period over `vol_lookback` is in bottom-percentile band (`compressed_threshold`).
2. **Expansion onset signal**: vol jumps above `expanded_threshold` within `transition_window` bars.
3. **Direction inference**: trade in the direction of HTF bias (or session-aligned direction if no HTF bias) — the regime transition releases stored directional energy.

Mechanism story: in a compressed-vol regime, dealers, MM systems, and stop-clusters accumulate around tight ranges. The expansion-onset is the regime-shift point at which accumulated positioning unwinds. The direction is information-revealed *during* the transition (not predicted *before* it) — the strategy is reactive to the regime state, not predictive of which regime is next.

### Why this is genuinely non-bias-scaffold

The HTF-EMA-bias scaffold (per [[trading/mechanisms/scaffold-as-mechanism]]) determines timing by EMA crossover regime. Vol-regime-transition determines timing by **vol-state crossover**, which is structurally different:

- HTF-EMA-bias fires whenever EMA(20) > EMA(50) (continuous bias state); secondary axis selects sub-set.
- Vol-regime-transition fires only at the **transition event** (compressed → expanded), which is a point-in-time crossing rather than a continuous state.

The scaffolds share the bar-direction-aligned entry trigger (both are HTF-trend-followers) but their gating axes are orthogonal. This is what produces the cluster correlation <+0.4 in Phase B analysis.

## Pocket evidence (run #4733)

The strategy's broad grid is noise-dominated (66.4% eligible; median Sharpe −0.68; median ATP −$0.50) — false-positive transition detection dominates outside the pocket. **The pocket itself is sharply defined**:

| Param | Value |
|---|---|
| `vol_period` | 15 bars |
| `vol_lookback` | 240 bars |
| `compressed_threshold` | 0.20 percentile |
| `expanded_threshold` | 0.70 percentile |
| `transition_window` | 8 bars |
| `stop_atr_mult` | 1.0 |
| `target_atr_mult` | 5.0 |

**Pocket performance**:

| Metric | Value |
|---|---:|
| Sharpe | **4.66** |
| ATP | **+$24.22** |
| Trades | 25 |
| Win-rate | 52% |
| long_pnl total | +$324 |
| short_pnl total | +$281 |

Both directions strongly positive (asymmetry would have suggested a scaffold-residual; balanced PnL across long/short suggests genuine mechanism). The pocket sits at 1:5 R:R — consistent with the [[trading/mechanisms/universal-stop-target-prior|family-conditional R:R signature for event-driven mechanisms]] (target=5×ATR; stop=1×ATR).

### Why outside the pocket is noise

The compressed and expanded thresholds (0.20 / 0.70 percentile) are sharply tuned. Outside this range:
- Lower compressed threshold → more bars qualify as "compressed" → more false-positive transition detections (random vol noise crossing arbitrary band).
- Wider transition_window → captures bars after the actual transition has resolved → no edge.
- Looser thresholds → mechanism degrades into "any vol expansion in any direction" (which is just trend-following).

The mechanism is theoretically interpretable: regime transitions are **rare events with sharply defined detection criteria**. The grid noise outside the pocket reflects the structural rarity of clean transitions.

## Phase B novelty falsifier verdict

The strategy spec's pre-registered novelty falsifier (in `vol_regime_transition_4h.md` falsifier section) required cross-strategy timing correlation **<+0.4** against:

- `confluence_trend_4h` v1
- `confluence_trend_4h_v2`
- `ema_bias_breakout_4h_v2`
- `bollinger_squeeze_4h`
- `range_fade_4h`

Phase B (2026-05-06) extended cross-strategy correlation matrix to 15 strategies using Q5 mean realized P&L per-bar series from `quantile_curves.csv`. Result:

| Baseline | Correlation vs `vol_regime_transition_4h` |
|---|---:|
| `confluence_trend_4h_v2` | +0.322 |
| `confluence_trend_4h` | +0.327 |
| `ema_bias_breakout_4h_v2` | +0.272 |
| `bollinger_squeeze_4h` | +0.070 |
| `range_fade_4h` | +0.027 |

**All 5 baselines pass <+0.4 threshold.** Max correlation +0.327 (with `confluence_trend_4h`) — well below threshold. **Novelty falsifier PASSES.**

This is the first crypto operationalization of a GARCH-style regime-switching strategy that doesn't sit in the HTF-EMA-bias scaffold cluster (which has internal corr +0.59-0.75 per [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism N=7 evidence]]).

## Empirical evidence (2026-05-08 backtest) — cohort-99.6%-positive at meta-strategy form

The 2026-05-06 single-strategy form documented above (run #4733 pocket on `vol_regime_transition_4h`) was a single-config pocket against a noise-dominated grid — research-grade-only at that operationalization. The 2026-05-08 `meta_vol_v1` operationalization swept the same mechanism across 500 parameter configs and produced **cohort-grade evidence**: not a single-config pocket, but uniformly-positive edge across the parameter region.

### Cohort result

`meta_vol_v1` 500-config grid sweep on BTCUSDT perp 4h, window 2025-05-03 → 2026-04-28. Sweep on the `V_TRANSITION` leading sub-strategy (`vt.stop ∈ [0.5, 1.5]`, `vt.target ∈ [3.0, 7.0]`, `vt.risk_pct ∈ [0.005, 0.02]`).

| Metric | Value |
|---|---:|
| Cohort full-positive ratio | **99.6% of 500** |
| Cohort val-positive ratio | 100% of 500 |
| Cohort median P&L | +$202 |
| Cohort max | +$1,119 |
| Win-rate cohort | 35–51% (median 47%) |
| Trade-count cohort | 46–63 per year |

### Ranking transfer — train→val corr +0.886

| Metric | Value |
|---|---:|
| train→val corr | **+0.886** (rare; structural-edge signature) |
| Q1 → Q5 val mean | $36 → $63 → $88 → $129 → **$219** |
| Q1 → Q5 monotonicity | Strict monotone, 6.1× spread |

Train-rank-based config selection cleanly identifies the val-best region. Combined with the cohort-positive base, this is the **"real structural edge, not single-config luck"** signature per [[foundation/methodology/strategy-validation-discipline|validation-discipline]] (cohort-positive + monotone train→val transfer + high rank correlation — the inverse of the [[trading/rejected/meta_session_v1-2026-05|meta_session_v1]] broken-cohort signature on the same week's batch).

### Best-region drivers

| Parameter | Pocket region |
|---|---|
| `vt.stop_atr_mult` | 0.5 – 0.61 (lower-wall region) |
| `vt.target_atr_mult` | 6.5 – 7.0 (upper-wall region in v1; extended bounds in v2 pending) |
| `vt.risk_pct` | 0.02 (upper-wall; partially leverage-Sharpe-inflated) |

The target-side parameter pins at the upper-wall (7.0) — strong evidence the true optimum is **higher than the v1 grid bounds tested**. `meta_vol_v2` (extended-bounds grid) is pre-registered to test whether the param-wall was a v1 ceiling artifact or whether the mechanism has a genuine ceiling at 7×ATR. If v2 top region is INTERIOR at extended bounds, the mechanism ceiling is bounded; if still at wall, the true target is even higher.

### Mechanism R:R signature — right-tail of mean-reversion family

The pocket sits at R:R 9:1 to 14:1; cohort-median R:R producing edge at 5:1 to 7:1; friction-bound below 3:1. This places vol-regime-transition at the **right tail of the mean-reversion family** distribution per [[trading/mechanisms/universal-stop-target-prior#right-tail-mean-reversion-vol-regime-transitions|universal-stop-target-prior]] — distinct from the 1:5–1:6 family default. Mechanism honesty: this is a **rare-event capture** profile (46% win-rate compensated by high R:R asymmetry), not the conventional 50%-win-rate mean-reversion default.

### Status upgrade rationale: seedling → growing

The 2026-05-06 single-config pocket was research-grade-only under [[foundation/methodology/strategy-validation-discipline|LdP Second Law]] (same-window pocket selection). The 2026-05-08 cohort-grade evidence on 500 configs is structurally different:

- Cohort-positive ≠ single-config-positive — the parameter region (not just one point) carries edge
- train→val corr +0.886 with monotone Q1→Q5 transfer is the cohort-stationarity signature
- Mechanism R:R signature is interpretable (right-tail of mean-reversion family), not heat-map-derived

Page upgraded **seedling → growing**. Promotion is pre-deployment only — 2024 OOS still load-bearing for deployment-grade. The 2024 OOS test is **pre-registered, not heat-map-derived** — per [[foundation/methodology/strategy-validation-discipline|Marcos' Second Law]] this is the clean independence path. 2024 OOS failure would NOT close the mechanism (per a standing convention, 1y windows vary structurally) but would require regime-conditioned deployment rather than regime-invariant deployment.

### Falsifier-attempt (open)

- **v2 grid A** (`strategies/meta_vol_v2`) — tests whether the param-wall position was the v1 ceiling or just the bounds being too tight. INTERIOR optimum at extended bounds = mechanism ceiling found; STILL at wall = ceiling is higher than v2 tests. **Resolved 2026-05-11 — see v2 evidence section below.**
- **2024 OOS** — load-bearing. If the +0.886 train→val corr replicates across the 2024 bullish-direction (March ATH break) + 2024 bearish-direction (August yen-carry-unwind cascade) transitions, mechanism is deployment-grade. The 2024 transitions are independent of the 2025-26 training window — clean LdP independence per §"Caveats requiring OOS validation".

## Empirical evidence (2026-05-11 v2 batch) — extended bounds + corr diagnostic + leverage curve

The v2 batch ran two grids on BTCUSDT perp 4h (same 2025-05-03 → 2026-04-28 window as v1) targeting the open v1 falsifier-attempts: Grid A extended the param bounds to test whether v1's upper-wall on target was a ceiling artifact; Grid B swept leverage to map the Kelly inflection.

### v2 Grid A (extended bounds, fixed risk_pct=0.01)

`meta_vol_v2` extended `vt.stop_atr_mult` lower bound to 0.25 and `vt.target_atr_mult` upper bound to 12.0 at fixed `risk_pct=0.01`. 64-config grid.

| Metric | Value |
|---|---:|
| Cohort full-positive | 100% |
| Cohort median full_pnl | **$302** (vs v1 sub-cohort at risk=0.01 ≈ $180 — +68%) |
| Best val | $391 at vt.stop=0.25, vt.target=7.0 |

The +68% cohort lift at fixed leverage indicates the v1 leverage was substituting for the missing tight-stop region — de-leveraged-v2 form matches leveraged-v1 region. **vt.stop=0.25 still hits the (new) lower wall**; v3 grid should extend stop ∈ [0.15, 0.40].

### Stop-axis-conditional ranking transfer — corr-break diagnostic

**Aggregate corr(train,val) on v2 grid = −0.305** appears as a regression from v1's +0.81 at the risk=0.01 sub-cohort. Diagnostic investigation revealed this is a **mixing artifact**, not a uniform break:

| Stop region | corr(train,val) | Interpretation |
|---|---:|---|
| stop ≤ 0.464 | +0.732 to +0.840 | Cleanly tunable |
| stop = 0.571 | −0.068 | Inflection |
| stop ≥ 0.679 | −0.46 to −0.49 | Anti-predictive |

**Sharp inflection at stop ≈ 0.5×ATR.** Two regimes within the same mechanism family:

- **Tight-stop regime (<0.5×ATR)**: trades cut quickly on adverse moves; P&L depends on capturing the same fat-tail moves regardless of period; train and val ranks align. **Tunable.**
- **Wider-stop regime (≥0.5×ATR)**: trades hold through bar noise; P&L becomes path-specific; train and val ranks diverge. **Untunable on this window.**

The strategy's best val cohort sits in the tight-stop region (top 5 val all at stop=0.25). v3 grid restricted to stop ∈ [0.15, 0.45] would operate entirely in the tunable regime; corr should land near +0.7+ and ranking-grade deployment becomes possible.

**Methodology generalization**: aggregate corr(train,val) can MASK a tunable sub-region by mixing it with an anti-predictive sub-region. Always split corr by param-axis before declaring a cohort untunable. See [[foundation/methodology/strategy-validation-discipline]] for the broader corr-diagnostic discipline.

### Target-axis saturation at 7×ATR

At fixed vt.stop=0.25, configs at `vt.target_atr_mult` = 7.0, 8.0, 9.0, 10.0, 11.0, 12.0 produced **identical** val P&L ($267.15), train P&L ($108.92), Sharpe (2.10), and trade count (74) — to two decimal places. The `vol_neutral` exit (vol_rank returning to [0.25, 0.65]) fires before targets above ~7×ATR can be reached.

**Target axis past 7 is wasted grid space.** This is a new diagnostic shape — *param-axis saturation* — distinct from a param-wall; see [[universal-stop-target-prior#target-axis-saturation]] for the methodology-layer encoding of the wall-vs-saturation distinction.

### v2 Grid B (leverage sensitivity)

`meta_vol_v2_leverage` swept `vt.risk_pct ∈ [0.005, 0.04]` at fixed stop=0.5, target=7.0. 13 configs.

| Metric | Behavior |
|---|---|
| Sharpe | Monotone-rising 2.06 → 3.44 (no Kelly inflection in range) |
| corr(train,val) | **+0.993** (cleanest signal in lab) |
| max_drawdown | Scales linearly with risk_pct: 11% at risk=0.005; 33% at risk=0.04 |

**Deployment leverage cap is set by max_dd tolerance, not Sharpe.** The Grid B anchor (stop=0.5) sits AT the Grid A inflection — possibly suboptimal vs the tight-stop region. Grid B v3 should re-anchor at Grid A v3's true optimum.

### Cross-grid implications

1. Grid A v3 needs `stop ∈ [0.15, 0.45]` (tunable regime only).
2. Grid B v3 should re-anchor at Grid A v3's true optimum (current Grid B anchor stop=0.5 is at the inflection).
3. Deployment policy: pick leverage by max_dd tolerance; train-rank parameter selection works **within the tight-stop region**.
4. 2024 BTC perp OOS still load-bearing for deployment-grade promotion.

## Empirical evidence (2026-05-11 PM v6 batch) — 3-symbol production-candidate portfolio + asset-vol-scaling N=3

> **⚠️ WALKED BACK 2026-05-13 — path-quality audit reveals lottery-shape.** The "first deployment-ready strategy package" framing in the section below was based on aggregate metrics (Sharpe / Calmar / max_dd / cohort-positivity) and did not include path-quality diagnostics. The 2026-05-12 fresh-eye audit found `meta_vol_btc_prod_v1` is a **5-event lottery shape on 2y window**: Top-1 trade = 55%, Top-5 trades = **224%** of total P&L; 167 trades / 30 winners / 17.9% WR; cell-carving "diversification" is **cosmetic** — `vol_transition` carries the entire $656 edge, `range_fade` is a net drag (−$100), `vol_expansion_short` is ~$0 noise. N=2 confirmation on SOL prod (Top-5 = 210%, `range_fade_long` = −$37). **Verdict revision**: v6 BTC prod is `ship-research-grade-only` regardless of aggregate metrics; cell-carving fails the per-sub PASS filter; deployment risk dominated by event-recurrence. See [[foundation/methodology/path-quality-diagnostics#v6-btc-prod-lottery-shape-the-2026-05-13-walk-back|path-quality-diagnostics §3.1]] for the full audit + [[foundation/methodology/path-quality-diagnostics#event-concentration|§2.1 event-concentration]] and [[foundation/methodology/path-quality-diagnostics#per-sub-pass|§2.2 per-sub PASS]] for the diagnostic methodology. The asset-vol-scaling N=3 finding below (productive vt.stop scales across symbols) survives — that is mechanism-translation evidence, independent of the lottery-shape path-quality issue.

The v6 batch compressed the v3 → v4 → v5 → v6 evolution across BTC + ETH + SOL into a single session, producing what the 2026-05-11 framing called the lab's "first deployment-ready strategy package" (walked back 2026-05-13 — see banner above): three vol-regime-transition strategies on identical 2y window (2024-05-18 → 2026-05-08).

### Production-candidate metrics (BTC + ETH + SOL, 2y)

| Strategy | net_pnl ($1K, 2y) | Sharpe | max_dd | Direction | Path-quality (2026-05-13 audit) |
|---|---:|---:|---:|---|---|
| BTC prod | $556 | 1.70 | 29% | Bidirectional | **Lottery** — Top-5 = 224% of total P&L; cell-carving cosmetic |
| ETH prod | $265 | 1.29 | 14.5% | Long-only | Likely-lottery (inherits event-concentration shape per substrate) |
| SOL prod | $270 | 1.28 | 19.9% | Long-only | **Lottery** — Top-5 = 210% of total P&L |

**BTC's unique bidirectional capability** traces to ETF cash-and-carry counter-flow per [[etf-era-carry-trade-funding-distortion]]; ETH and SOL lack the structural mediator (no ETF cash-and-carry leg of comparable size), so the productive direction is bullish-dominant only on the 2024-2026 window. Originally framed as "BTC is the primary deployment candidate; ETH/SOL are asset-scaled diversifiers" — but per the 2026-05-13 walk-back, all three are `ship-research-grade-only` until either (a) path-quality improves on a longer window or different parameter region, or (b) the strategies are composed into a portfolio with uncorrelated event-timing strategies to lift the aggregate path-quality. The lab's actual lead deployment candidate as of 2026-05-13 is [[trading/mechanisms/liquidation-cascade-aftermath|meta_post_cascade_btc_prod_v1]] (mid-band concentration, first per-sub PASS in lab history) — not the v6 vol-regime portfolio.

Annualized-return numbers are **not** taken from the engine's `summary.json` `annualized_return_pct` field — that field uses a non-standard convention. Use simple or compound annualization computed from net_pnl + window length; see [[foundation/methodology/forecast-vs-tradeable-gap#engine-output-gotcha-annualized_return_pct]].

### Asset-vol-scaling — productive vt.stop scales with asset volatility (N=3) {#asset-vol-scaling}

The productive `vt.stop_atr_mult` region differs systematically across assets:

| Symbol | Productive vt.stop region | Scaling factor vs BTC |
|---|---:|---:|
| BTCUSDT | ≈ 0.13 | 1.0× (anchor) |
| ETHUSDT | ≈ 0.31 | 2.4× |
| SOLUSDT | ≈ 0.32 | 2.5× |

**Mechanism translation is structural across symbols** (works on all 3 with asset-scaled parameters); the parameters themselves need asset-specific bounds. Tight stops measured in ATR units fire on bar noise differently across assets — higher-vol assets have wider intra-bar excursions relative to their ATR, so a tight-in-BTC-units stop hits in-bar wicks on ETH/SOL and produces noise-dominated results.

### Methodology implication — per-asset parameter bounds

Multi-symbol mechanism work should adopt **per-asset parameter bounds by default**, not single-config-everywhere. The "copy BTC bounds to ETH/SOL" approach systematically fails when asset-vol scales differ.

The discovery tool is the **per-stop diagnostic** (corr split by stop level — see Grid A v2 section above). Per-stop diagnostic reveals where the mechanism inflection sits on each asset; productive-side bounds should leave a buffer past the observed inflection (see [[foundation/methodology/strategy-validation-discipline]] §post-diagnostic-bounds-narrowing if/when codified — N=1 candidate at present).

### Scope of N=3

N=3 is **within-mechanism, across-asset** (one mechanism family — vol-regime-transition — tested on three assets with asset-scaled parameter bounds). The broader claim *"asset-vol-scaling applies to any mechanism family in multi-symbol translation"* is **N=1 at the cross-mechanism-family level** — only vol-regime-transition has been tested across multiple assets so far. Treat as a working hypothesis pending the next multi-symbol mechanism test.

### Pre-deployment gates remaining (substrate, 2026-05-11 PM)

1. Single-window 2y caveat — the v6 batch evidence is one window; cross-window stationarity unverified
2. Cross-strategy correlation measurement on the 3-symbol portfolio
3. Per-trade `sub_strategy` tagging (a team engineer route) — unblocks hypothesis-level falsifier
4. Deployment-policy decision (single / pair / triple; equal-tranche vs risk-parity) — user-only call
5. Stakeholder-facing return numbers must use simple/compound annualization, not engine's `annualized_return_pct` field

## Caveats requiring OOS validation before deployment-grade

### 1. Same-window pocket selection (LdP Second Law concern)

Per [[foundation/methodology/strategy-validation-discipline|Marcos' Second Law]] §2.1: *"Do not research under the influence of a backtest."* The pocket params (run #4733) were selected by ranking grid output on the same window the test will be evaluated on. Per Part I §4.5 expected_net_edge independence rule, this is overfit by construction; default verdict = **research-grade-only** until tested on independent data.

### 2. Thin transition sample (n=3 regime transitions in 1y)

The BTC perp 2025-26 window has only ~3 documented regime transitions:

- Q4-2025 cycle peak rejection (compressed → expanded, bearish direction)
- Q1-2026 ranging compression breakdown
- Late-Q2-2026 recovery transition

A pocket Sharpe of 4.66 on 25 trades distributed across ~3 regime events is **highly subject to event-selection effects**. The mechanism may be real but the magnitude estimate has wide uncertainty bands.

### 3. Single 1y window

The current pipeline tests a single window only (per [[foundation/methodology/strategy-validation-discipline]] §11). Walk-forward / multi-window is not implemented; cross-window stationarity of the pocket is unverified.

### 4. 2024 OOS is the load-bearing validation window

The 2024 BTC perp data contains two well-documented vol regime transitions:

- **March 2024 ATH break** (compressed → expanded, bullish direction)
- **August 2024 yen-carry-unwind cascade** (compressed → expanded, bearish direction)

These are **independent of the 2025-26 training window** (no temporal overlap). The pocket holding on 2024 OOS is the binary validation:

- **If 2024 OOS pocket holds**: page graduates seedling → growing/evergreen; mechanism promoted to lab's first deployment-grade non-bias-scaffold strategy.
- **If 2024 OOS pocket fails**: page becomes a partial-truth + same-window-fitting cautionary entry; mechanism rejected with the canonical "novelty validated at correlation level, edge specific to single regime sample" framing.

The OOS test is **pre-registered**, not heat-map-derived. Per Marcos' Second Law, this is the clean independence path the methodology requires.

## Open research questions

1. **1h frame test**: pocket exists at 4h; does the same mechanism fire at 1h with different `vol_lookback` calibration? 1h frame would multiply trade count and OOS sample.
2. **Alternative direction operationalizations**: current strategy uses HTF-bias direction; does session-aligned direction (e.g., direction of leader index break during transition) work as well or better?
3. **Multi-asset extension**: GARCH regime-switching is documented in equity / FX / commodities; does the BTC pocket transfer to ETH / SOL / DOGE? (Currently blocked by single-symbol pipeline limitation.)
4. **Cross-regime stationarity**: pocket params (compressed_threshold=0.20, expanded_threshold=0.70) are 2025-26-specific. Do absolute vol-percentile thresholds hold across regimes, or do they need to be regime-recalibrated?

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC taxonomy]]) | Pattern stability | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Pattern derived from this regime** | Source data (run #4733) |
| Bearish continuation (e.g. 2026-Q1) | Pattern likely-stable — same compressed→expanded dynamics | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Bullish trending (e.g. March 2024 ATH break) | **Load-bearing OOS test** — regime-switching is structural, not direction-dependent | Theoretical grounding (Hamilton 1990); 2024 OOS pending |
| Chop / transitional | **Mechanism doesn't fire** — no compressed-to-expanded transition without macro impulse | Theoretical |
| Cascade aftermath (e.g. August 2024 yen-carry) | **Load-bearing OOS test** — cascade-driven transition is the canonical event-driven case | Theoretical; 2024 OOS pending |

**Notes:**
- The mechanism is **structural (regime-switching as a real econometric phenomenon)**, not regime-derived.
- What changes across regimes: the **direction** of the transition's net edge (bullish vs bearish dominance) and the **frequency** of transitions (more in volatile market windows).
- 2024 OOS validation across both bullish-direction (March) and bearish-direction (August) transitions is what would establish cross-regime stationarity.

## Key Takeaways

- **First non-HTF-bias-scaffold mechanism in lab** with cleanly orthogonal trade-timing signal.
- **Pre-registered novelty falsifier PASSES** (corr <+0.4 with all 5 baselines).
- **GARCH-grounded mechanism story** (Engle / Bollerslev / Hamilton / Catania-Grassi) — regime-switching is a documented real econometric phenomenon, not a heat-map artifact.
- **Pocket evidence is sharp but same-window**: Sharpe 4.66 in pocket; broad grid noise-dominated outside it.
- **OOS validation on 2024 (March ATH + August yen-carry-unwind) is load-bearing** before deployment-grade promotion.
- **Diversification candidate**: combined with positioning-extreme (`funding_extreme_exhaustion_4h_v2`, corr ≤+0.07 with everything), these two mechanisms are the lab's first cleanly orthogonal pair to the bias-cluster.
- **Status: seedling**. Will graduate or be rejected based on 2024 OOS test outcome.

## Related

- [[trading/mechanisms/scaffold-as-mechanism]] — N=7 evidence; this is one of two non-bias-scaffold mechanisms with material orthogonality (other: positioning-extreme via `funding_extreme_exhaustion_4h_v2`)
- [[trading/mechanisms/funding-cycle-dynamics]] — sibling non-bias-scaffold mechanism family; positioning-extreme is the most-orthogonal-in-lab (corr ≤+0.07 with everything)
- [[trading/mechanisms/universal-stop-target-prior]] — family-conditional R:R signature; this strategy's 1:5 R:R fits the event-driven family default
- [[trading/mechanisms/moving-averages]] — Bollinger / vol-band canonical reference; sibling mean-reversion-at-band mechanism (range_fade_4h) sits in same compression detection but trades opposite to expansion
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Second Law (no research under influence of backtest); pocket is research-grade-only until OOS
- [[foundation/methodology/deployment-edge-threshold]] — net-of-friction floor; pocket #4733 evidence is gross; net-of-friction OOS check is part of validation gate
- [[foundation/methodology/forecast-vs-tradeable-gap]] — translation-loss methodology; this strategy's mechanism IS the tradeable form (no L1→L2 gap)
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — regime documentation including the 2024 transitions (March, August) that constitute the OOS validation window
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cross-strategy finding; vol-regime-transition fits the same Q4-2025 / Q1-2026 concentration pattern

## Sources

### Theoretical grounding
- Engle, R. (1982). *Autoregressive Conditional Heteroskedasticity with Estimates of the Variance of United Kingdom Inflation*. Econometrica, 50(4), 987-1007. — ARCH foundation.
- Bollerslev, T. (1986). *Generalized Autoregressive Conditional Heteroskedasticity*. Journal of Econometrics, 31(3), 307-327. — GARCH extension; standard volatility-clustering model.
- Hamilton, J. (1989). *A New Approach to the Economic Analysis of Nonstationary Time Series and the Business Cycle*. Econometrica, 57(2), 357-384. — Markov-switching framework.
- Hamilton, J. (1990). *Analysis of Time Series Subject to Changes in Regime*. Journal of Econometrics, 45(1-2), 39-70. — Markov-switching applied to financial series; foundation for regime-switching strategies.
- Catania, L., Grassi, S. (2017). *Modelling Crypto-Currencies Financial Time-Series*. — Documents GARCH regime-switching in crypto; stronger persistence + sharper transitions than equity.

### Empirical evidence (this batch)
- the backtesting lab — strategy spec + pre-registration + grid output (run #4733 pocket)
- the backtesting lab (#5) — batch-level synthesis + per-strategy verdict
- the backtesting lab — Phase B 15-strategy correlation matrix; row for `vol_regime_transition_4h` showing all baselines <+0.4
- the backtesting lab — meta-strategy spec; `V_TRANSITION` sub-strategy operationalizes this mechanism
