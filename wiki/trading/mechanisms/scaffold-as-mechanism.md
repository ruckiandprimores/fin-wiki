---
title: "Scaffold-as-Mechanism — when shared timing IS shared mechanism"
status: growing
created: 2026-05-06
updated: 2026-05-13
tags: [mechanism-meta, design-pattern, timing-correlation, novelty-claim, diversification, day-swing, empirical-evidence]
---

# Scaffold-as-Mechanism — when shared timing IS shared mechanism

> **TL;DR**: When ≥3 strategies on the same dataset cluster at trade-timing correlation ≥+0.6 despite differing in their *secondary* signal axes, the **scaffold** (regime gate + entry trigger primary axes) is the mechanism — the secondary axes are different *expressions* of the same underlying force, not distinct mechanism classes. Empirical observation extended through 2026-05-06 batch (**N=7 supporting cases now**): the HTF-EMA-bias scaffold cluster spans `ema_bias_breakout_4h_v2`, `confluence_trend_4h`, `confluence_trend_4h_v2`, `taker_buy_asymmetric_4h`, `volume_z_asymmetric_4h`, plus the long-only mutations `taker_buy_long_only_4h` and `volume_z_long_moderate_4h` (all clustering +0.39 to +0.75 with each other). Operational consequence: portfolio diversification requires *different scaffolds*, not different secondary axes on the same scaffold; "novel signal axis" claims need pre-registered timing-correlation predictions falsifiable on grid run. **N=7 also surfaces a conditional finding on the P3 direction-restriction mutation operator** — direction restriction lifts edge only when the working-direction signal is *differentiated from the scaffold*; when the working direction IS the scaffold (as in #4 / #6 below), P3 is scaffold-residual and produces no per-trade lift.

## Definition

**Scaffold** = the regime gate + entry trigger primary axes that determine *when* a strategy fires.

The scaffold answers: in which bars does this strategy consider entering at all?

| Scaffold class | Bias gate | Entry trigger primary axis |
|---|---|---|
| **HTF-EMA-bias** | `EMA(20) vs EMA(50)` (or similar HTF EMAs) | bar-direction-aligned (breakout, taker-imbalance, vol-Z, etc.) |
| **Vol-state** | `vol_rank vs threshold` (compressed vs expansion) | breakout direction (squeeze) or fade direction (range) |
| **Funding-regime** | funding rank vs threshold (extreme persistent vs flip) | direction-of-flip or direction-of-persistence |
| **Calendar/session** | hour-of-day, day-of-week, expiry proximity | time-window entry |
| **Liquidation-cascade** | post-cascade window (cluster cleared + recovery confirmed) | reclaim-of-level direction |
| **Microstructure-asymmetry** | depth/spread imbalance regime | order-flow continuation |

The **secondary axis** is whatever signal lives *inside* the scaffold's eligible bars: which specific bars among the eligible ones do we enter on?

The empirical claim is that **the scaffold determines timing** (which bars produce P&L) far more than the secondary axis (which sub-set of eligible bars actually fires the entry). Two strategies on the same scaffold but different secondary axes produce highly correlated trade timing.

## Empirical observation (2026-05-05 batch, N=4)

Four strategies on BTCUSDT perp 4h 2025-05/2026-04, all sharing the HTF-EMA-bias scaffold but with distinct secondary axes:

| Strategy | Bias gate | Secondary axis |
|---|---|---|
| `ema_bias_breakout_4h_v2` | EMA(20) vs EMA(50) | N-bar high/low breakout |
| `confluence_trend_4h` | EMA(20) vs EMA(50) | breakout × vol-expansion |
| `taker_buy_asymmetric_4h` | EMA(20) vs EMA(50) | taker-buy ratio extreme |
| `volume_z_asymmetric_4h` | EMA(20) vs EMA(50) | volume Z-score extreme |

Pairwise per-bar Q5 mean realized P&L correlation (`quantile_curves.csv` Q5 series):

| Strategy pair | Correlation |
|---|---:|
| confluence_trend × ema_bias_v2 | **+0.711** |
| taker_buy × ema_bias_v2 | **+0.704** |
| volume_z × ema_bias_v2 | **+0.586** |
| confluence_trend × taker_buy | **+0.677** |
| confluence_trend × volume_z | **+0.650** |
| taker_buy × volume_z | **+0.686** |

All within +0.59 to +0.71. **The scaffold determines timing; secondary axes are expressions, not distinct mechanisms.**

## Empirical observation extended (2026-05-06 batch, N=7)

The 2026-05-06 6-strategy batch added three more strategies on the same HTF-EMA-bias scaffold, with mutation-axis variations rather than novel secondary axes. Phase B cross-strategy correlation analysis (15 strategies; per-bar Q5 series in `strategies/analyses/2026-05-06-cross-strategy-correlation-extended.csv`) confirmed the cluster persists.

### #1 confluence_trend_4h_v2 — vol-expansion confluence dilutes per-trade economics

`confluence_trend_4h_v2` ran the full `stop_atr_mult` ∈ [1.0, 1.5, 2.0, 2.5, 3.0] sweep that the v1 grid missed (forclosure of the v1 "param-region miss" objection — see [[trading/rejected/confluence-trend-4h-2026-05]]). Verdict: same-scaffold mutation, not a distinct mechanism.

| Pair | Correlation |
|---|---:|
| confluence_trend_4h_v2 × ema_bias_breakout_4h_v2 | **+0.754** |
| confluence_trend_4h_v2 × confluence_trend_4h | (high; same family) |

Per-trade economics: ATP −$0.78/trade vs single-axis bias-filter parent (`ema_bias_breakout_4h_v2` ATP +$0.62). The secondary vol-expansion axis is decisively dilutive, not additive — confluence as quality filter only works when the added axis selects bars where the base mechanism's edge is amplified, which vol-expansion in 2025-26 BTC perp 4h is not.

### #4 taker_buy_long_only_4h — direction-restriction is scaffold-residual

`taker_buy_long_only_4h` is a P3-style direction-restriction mutation of `taker_buy_asymmetric_4h` (drop the entry_short side, keep entry_long only). Result: **identical Sharpe (+0.82) and ATP (+$0.59) to the parent**, halved trade count, no per-trade economics change.

| Pair | Correlation |
|---|---:|
| taker_buy_long_only_4h × ema_bias_breakout_4h_v2 | **+0.387** |
| taker_buy_long_only_4h × taker_buy_asymmetric_4h | (high; direct mutation) |

Cluster correlation halves (from +0.704 to +0.387) but the scaffold signature is intact — the long-only mutation removes half the trades but the remaining trades are still scaffold-driven. **Removing trades doesn't change per-trade economics when the working direction IS the scaffold.**

### #6 volume_z_long_moderate_4h — two-stacked mutations, same scaffold-residual signature

`volume_z_long_moderate_4h` stacks two mutations on `volume_z_asymmetric_4h`: direction-restriction P3 (long-only) + threshold-restriction (Z band [1.0, 2.0], later refined to [1.5, 2.5] per [[trading/rejected/volume-z-moderate-2026-05]]). Pre-registered prediction: ATP lifts from parent's +$1.53 to +$2.00+ via concentration. **Result: ATP +$0.67 vs predicted +$2.00; medSh +0.66 vs target +0.94.** Same scaffold-residual signature as #4.

| Pair | Correlation |
|---|---:|
| volume_z_long_moderate_4h × ema_bias_breakout_4h_v2 | **+0.384** |
| volume_z_long_moderate_4h × volume_z_asymmetric_4h | (high; direct mutation) |

Cluster correlation halves to +0.384 (same pattern as #4) — long-only mutation halves but doesn't break the scaffold signature.

### N=7 cluster summary

| Strategy | Correlation with `ema_bias_breakout_4h_v2` (cluster anchor) | Notes |
|---|---:|---|
| `confluence_trend_4h` | **+0.711** | Original N=4 batch |
| `confluence_trend_4h_v2` | **+0.754** | Forclosure run; cluster confirmed |
| `taker_buy_asymmetric_4h` | **+0.704** | Original N=4 batch |
| `volume_z_asymmetric_4h` | **+0.586** | Original N=4 batch |
| `taker_buy_long_only_4h` | **+0.387** | Long-only mutation; halved correlation, intact scaffold |
| `volume_z_long_moderate_4h` | **+0.384** | Stacked mutations; same pattern |

Original full-symmetric strategies cluster +0.586 to +0.754 with the anchor; long-only mutations halve to +0.384–+0.387. **The mutations don't escape the scaffold; they sample a different sub-set of scaffold-eligible bars while leaving per-trade economics unchanged.**

For comparison, strategies on **different scaffolds** are far less correlated with this cluster:

| Strategy (different scaffold) | Max correlation with cluster |
|---|---:|
| `range_fade_4h` (vol-state, compressed) | −0.09 |
| `bollinger_squeeze_4h_short_only` (vol-state, expansion) | +0.26 |
| `funding_flip_recovery_4h_short_only` (funding-regime) | +0.03 |

All <+0.30 with the trend cluster. Different-scaffold strategies are genuinely different mechanism classes.

## Threshold framework

| Pairwise timing correlation | Reading |
|---|---|
| **≥ +0.6** | Same mechanism class. Secondary axes are signal expressions of one underlying force. |
| **+0.4 to +0.6** | Ambiguous. Investigate: shared scaffold component? overlapping regime selection? |
| **< +0.4** | Different mechanism class. Strategies are diversifying. |

The threshold is empirical — not a derived statistic. N=4 supporting cases for the +0.6 boundary; +0.4 lower bound is set conservatively. With more cohorts the bands sharpen.

**The threshold is per-bar P&L correlation, not return correlation.** Per-bar P&L correlation captures whether two strategies fire in the same bars and produce same-direction outcomes — i.e., timing identity. Return correlation aggregates across windows and obscures bar-level redundancy.

**Asymmetric-N exception (added 2026-06-08) — Pearson understates relation when one strategy's entries are a strict subset of the other's.** When the trade-count ratio is lopsided (N ratio > 2× or < 0.5×), the per-bar Pearson is dominated by the all-zero bars (both strategies flat) and reads near-zero even for same-trigger-family pairs. Mitigation: for asymmetric-N pairs, compute **co-occurrence overlap within ±N bars** as the load-bearing metric, not Pearson. Worked example: `range_tunnel` (N=65) vs `failed_breakout_phase_fade_eth` (N=166) — same-direction co-occurrence **77–80%** but Pearson only **0.013–0.031** (would misread as "different mechanism"). Use Pearson for symmetric-N pairs, overlap for asymmetric. See [[foundation/methodology/backtest-analysis-diagnostics#asymmetric-n-overlap]].

## Implications for design

### 1. Diversification requires scaffold-distinct strategies

A portfolio of 4 strategies all on the HTF-EMA-bias scaffold provides **one mechanism's worth of edge**, sampled four times. Capital allocation across them is roughly equivalent to running one strategy with 4× capital — no risk reduction, no edge stacking.

**Genuine diversification** comes from combining strategies on different scaffolds. The 2026-05-05 cohort's only non-trend-cluster member with positive net edge — `bollinger_squeeze_4h_short_only` — is on the vol-state scaffold and correlates +0.13 to +0.26 with the trend cluster. *That* combination diversifies; adding another HTF-EMA-bias strategy to it does not.

### 2. "Novel signal axis" claims need pre-registered timing-correlation predictions

Both `taker_buy_asymmetric_4h` and `volume_z_asymmetric_4h` were spec'd with claims that taker-imbalance and volume-Z constitute *distinct* signal axes (informed-flow microstructure, volume-clustering) — implicitly predicting low correlation with HTF-trend strategies.

The data falsified the novelty claim cleanly: both strategies cluster at +0.59 to +0.70 with the existing HTF-bias trend cluster. The "distinct signal axis" framing is real at the *spec level* (what the secondary signal measures) but collapses at the *timing level* (which bars produce P&L).

**Pre-registration discipline**: future strategy specs should include a predicted timing correlation with named comparators. Falsifier criterion: if observed correlation is ≥+0.6 with any comparator, the novelty claim is falsified — strategy is a same-mechanism mutation, not a new mechanism class.

This is the trading-side analog of [[foundation/methodology/strategy-validation-discipline|LdP Second Law]]: pre-register your novelty claim, run the test, accept the verdict.

### 3. N independent failures vs 1 structural failure × N

When 3+ strategies fail in a batch and they share a scaffold, that's **1 structural failure sampled N times**, not N independent failures. The trial-count reckoning per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] should treat them as a single trial of the scaffold — not separate trials.

Conversely, when 3+ strategies *succeed* in a batch and share a scaffold, that's also one mechanism's worth of evidence sampled N times — the meta-claim ("this scaffold works in this regime") needs scaffold-comparable evidence (other-scaffold strategies tested in same window), not just within-scaffold replication.

### 4. Scaffold catalog (current lab inventory)

Mapping the 2026-05-05 + 2026-05-06 batches + comparators to scaffolds:

| Scaffold | Strategies on it |
|---|---|
| **HTF-EMA-bias** | `ema_bias_breakout_4h_v2`, `confluence_trend_4h`, `confluence_trend_4h_v2`, `taker_buy_asymmetric_4h`, `taker_buy_long_only_4h`, `volume_z_asymmetric_4h`, `volume_z_long_moderate_4h` (7) |
| **Vol-state (expansion)** | `bollinger_squeeze_4h` family (incl. `_short_only`, `_short_regime`) (3) |
| **Vol-state (compression)** | `range_fade_4h` (1) |
| **Vol-regime-transition (GARCH)** | `vol_regime_transition_4h` (1; first non-bias-scaffold mechanism with corr <+0.4 with all bias-cluster baselines — see [[trading/mechanisms/vol-regime-transition-tradeable]]) |
| **Funding-regime (flip)** | `funding_flip_recovery_4h` family (3) |
| **Funding-regime (extreme persistent / positioning-extreme)** | `funding_extreme_exhaustion_4h` (v1 untestable; **v2 fires at persistence=3 — most-orthogonal in lab, corr ≤+0.07 with everything**) |
| **Session-boundary / settlement-cycle** | `asia_range_sweep_4h` (mechanism failure flagged); `funding_settlement_window_1h` (#3 of 2026-05-06 — settlement-cycle hypothesis rejected, h7/h15 partial truth retained — see [[trading/mechanisms/session-boundary-effects]]) |
| **Liquidation-cascade** | `wyckoff_spring_4h` (capability-gap-blocked) (1) |

The HTF-EMA-bias scaffold dominates by count (7 of 18+ testable strategies; 4 of 5 in 2026-05-05; 4 of 6 in 2026-05-06). Per implication #1, this is the lab's biggest **diversification gap**. The 2026-05-06 batch surfaced two non-bias-scaffold mechanisms with material orthogonality (`funding_extreme_exhaustion_4h_v2` and `vol_regime_transition_4h`) — those are the lab's diversification candidates.

### 5. Conditional P3 (direction-restriction) mutation operator

The W2 P3 mutation operator (direction-restriction: drop the failing entry side, keep the working side — per Part I §9 direction-restriction discipline) was validated at N=3 prior cases (`bollinger_squeeze_4h_short_only`, `funding_flip_recovery_4h_short_only`, `bollinger_squeeze_4h_short_regime` directional analog). Mean Sharpe gap when applicable: ≥+1.3, single transformation, no parameter tuning.

The 2026-05-06 batch produced two **non-lift** cases — #4 `taker_buy_long_only_4h` and #6 `volume_z_long_moderate_4h` (the latter stacked with a threshold restriction). In both cases, P3 produced **identical or worse per-trade economics vs parent**:

| Strategy | Parent ATP | P3 ATP | Per-trade lift | Parent Sharpe | P3 Sharpe |
|---|---:|---:|---:|---:|---:|
| `bollinger_squeeze_4h_short_only` | medATP varies | (working) | +1.3+ Sharpe gap | (negative) | **+1.02** |
| `funding_flip_recovery_4h_short_only` | medATP varies | (working) | +0.61 Sharpe gap | −0.11 | **+0.50** |
| `taker_buy_long_only_4h` | +$0.59 | **+$0.59** | **0** | +0.82 | **+0.82 (identical)** |
| `volume_z_long_moderate_4h` | +$1.53 (parent) | +$0.67 | **−$0.86 (worse)** | (parent variable) | +0.66 |

**Condition for P3 to lift edge:** the working-direction signal must be *differentiated* from the scaffold. In #4 / #6, the "working direction" is just the bias-filter scaffold itself — there's no differentiated signal to concentrate. P3 removes half the trades but the remaining trades have the same per-trade economics as the parent because they're all scaffold-driven.

**Operational rule:** before applying P3 as the mutation, check whether the secondary axis has a directional signature *independent of the scaffold direction*. Diagnostic:

| Diagnostic | P3 likely lifts | P3 likely scaffold-residual |
|---|---|---|
| Asymmetry comes from a **non-scaffold-aligned** structure (vol-expansion direction, funding flip direction, range-boundary side) | ✅ | — |
| Asymmetry just **mirrors the bias scaffold** (long-only because bias is bullish; short-only because bias is bearish) | — | ✅ |
| Parent's `long_pnl` vs `short_pnl` median sign aligns with the bias-scaffold direction | — | ✅ |
| Parent's `long_pnl` vs `short_pnl` median sign reflects a different mechanism (e.g., short-side funding-flip-recovery in a bullish-bias regime) | ✅ | — |

The condition is **not** "P3 always lifts" — it's "P3 lifts when the directional asymmetry comes from somewhere other than the scaffold." Without the differentiation check, P3 risks producing a same-mechanism mutation that's harder to operate (half the trade count, no change in per-trade economics — identical to running the parent at half size).

This sharpens the Part I §9 discipline: the mutation should be the *first* tried when grid asymmetry appears, AND the diagnostic should run before commitment. The 2026-05-06 batch is the first surfacing of cases where the operator's preconditions weren't met.

#### Variance-offset caveat (added 2026-05-11)

P3 direction-restriction is **not pure drag-removal**. Direction-restricted strategies often lose offsetting variance benefit from the suppressed side; the net gain is typically LESS than the suppressed side's standalone P&L loss. Empirical evidence (N=2 from 2026-05-11 v2 batch):

| Strategy | Predicted lift (suppressed side standalone) | Observed lift | Variance behaviour |
|---|---:|---:|---|
| `meta_trend_v2` (long-only on v1 architecture) | +$19/year | **+$7/year** | Missing $12 was offsetting variance benefit the short side contributed |
| `meta_post_cascade_v2` (short-only on v1 architecture) | variance predicted to drop | variance **rose +45–59%** | Direct cross-confirmation |

**Pattern**: P3 mutations have variance-offset effects beyond their P&L sign. **Pre-register variance prediction alongside P&L prediction** in future P3 specs. Treat the "pure-drag-removal" case as a best-case bound, not the central estimate. Central estimate: 30–60% of suppressed-side standalone P&L recovery.

## Empirical observation (2026-05-11 v2 batch, ranking-grade)

### First ranking-grade signal in lab history — `meta_trend_v2`

`meta_trend_v2` (long-only direction-restriction on the `meta_trend_v1` cell-carved architecture) produced the **first cohort-grade result in lab history where train-rank parameter selection is cleanly safe**. Across 10 strategies tested in the v1 + v2 meta-strategy batches, this is the only strategy where Q5 outperforms cohort mean with positive corr(train,val) AND monotone Q3→Q5 transfer.

| Metric | meta_trend_v2 | meta_trend_v1 |
|---|---:|---:|
| Cohort full-positive ratio | **95.8% of 500** | (cohort-positive at v1 form) |
| Cohort median full P&L | $111 | $104 |
| corr(train, val) | **+0.548** | +0.490 |
| Q5 val mean | **$134 (1.7× cohort mean $80)** | $74 |
| Q3 → Q4 → Q5 val | $62 → $97 → $134 (strict monotone) | (non-monotone) |
| Best val | $322 (bvc.stop=0.4, bvc.target=3.83) | — |

All other strategies in the v1+v2 batch are either anti-predictive (`meta_vol_v2`, `meta_post_cascade_v1+v2`, `meta_session_v1`) or noisy-non-monotone. The combination of (cell-carving via credible-volume Z) **+** (direction-restriction within the cell-carve setup) is what produces the clean-tuning property — neither operator alone produces it.

### Direction-restriction as productive cell-carving operator (regime-conditional)

The v1 finding established cell-carving via credible-volume Z lifts edge ~2.7× over the standalone bias-baseline. v2 extends: **direction-restriction within the cell-carve setup is productive when the regime favors one bias.** 2025-26 BTC perp was bullish-dominant (BTC ~+50% over the test window); dropping the `bias_baseline T_BEAR` short side both added cohort edge AND improved ranking signal. The clean-tuning property emerges from the combined operators, not either alone.

This sharpens the §5 conditional-P3 rule above: P3 lifts when the working-direction signal is differentiated from scaffold (the original 2026-05-06 finding) AND when the regime favours the kept direction. The regime-favours-direction condition is the v2-batch addition; previously implicit in the bias-cluster results.

### Param-wall position

`bvc.stop_atr_mult` top region at 0.4 — the new lower wall of the v2 grid (was 0.75 in v1). Same wall-hitting pattern as `meta_vol_v2`; tight-stop region wants further extension. v3 candidate grid: `bvc.stop ∈ [0.25, 0.6]`.

### Pre-deployment status

Research-grade with deployment-grade potential. Gated on:

1. v3 grid with `bvc.stop` extended below 0.4 (param-wall confirmed-or-bounded)
2. 2024 BTC perp OOS — load-bearing for deployment-grade promotion
3. Per-trade `sub_strategy` tagging (a team engineer blocker per `this research program` §1 engine output layer) for hypothesis-level falsifier *"per-sub ATP retention ≥ 80% of standalone"*

If the 2024 OOS holds with Q5 ranking-grade preserved, `meta_trend_v2` becomes the lab's **first deployment-grade candidate**. Until then, status is research-grade-ranking-grade — a new tier above the prior 9 statistical-grade strategies in the lab.

## Empirical evidence (2026-05-13) — cell-carving falsifier resolves: first PASS + worked failures {#per-sub-pass-2026-05-13}

The cell-carving discipline (a meta-strategy's claim that distributed sub-strategies produce diversified edge) is **falsifiable** via the per-sub contribution PASS filter: every active sub must be net-positive on its own gate cell. Any sub with cohort-negative P&L on its own regime cell is drag, not a diversifier — the cell-carving aggregation claim is **cosmetic** in that case.

The filter requires per-trade `sub_strategy` tagging in engine output. **prod_run mode emits this column today**; grid-run mode does not (a team engineer blocker per `this research program` §1 engine output layer). The first empirical results landed 2026-05-13 from prod_run analyses on lead deployment candidates.

### First PASS in lab history — `meta_post_cascade_btc_prod_v1`

| Sub | Trades | Total P&L | Avg P&L | Win Rate | Contribution % |
|---|---:|---:|---:|---:|---:|
| vol_spike_reversal | 33 | +$158.18 | $4.79 | 30.3% | 58.9% |
| bearish_capitulation_long | 17 | +$57.80 | $3.40 | 41.2% | 21.5% |
| bullish_exhaustion_short | 21 | +$52.73 | $2.51 | 38.1% | 19.6% |
| **Total** | **71** | **+$268.71** | — | — | **100%** |

All 3 subs net-positive on their own gates → filter **PASSES**. This is the **first empirical evidence in lab history** that cell-carving can produce mechanism-coherent (not cosmetic) edge distribution. The 71 trades distribute across all 3 subs in a 47/24/30 ratio, with avg-P&L-per-trade in a 1.9× range ($4.79 / $3.40 / $2.51) — concentration is moderate, not pathological. Win rates differ by sub but each clears profitability on its own gate.

### Worked failures (N=2) — cosmetic cell-carving exposed

The same 2026-05-13 audit batch found two strategies that aggregate-passed but per-sub-failed:

| Strategy | Sub 1 | Sub 2 | Sub 3 | Filter verdict |
|---|---:|---:|---:|---|
| `meta_vol_btc_prod_v1` (v6) | vol_transition +$656 | range_fade **−$100** | vol_expansion_short ~$0 | **FAIL** — 1-sub strategy with 2-sub overhead |
| `meta_vol_sol_prod_v1` (v6) | vol_transition (+) | range_fade_long **−$37** | — | **FAIL** — cosmetic |

These are the v6 vol-regime "deployment-ready package" walk-back evidence ([[trading/mechanisms/vol-regime-transition-tradeable#empirical-evidence-2026-05-11-pm-v6-batch-3-symbol-production-candidate-portfolio-asset-vol-scaling-n-3|vol-regime-transition-tradeable §v6 batch — walked back]]). At the meta-strategy aggregate level (Sharpe 1.70, dd 29%) the strategies looked deployable; at the per-sub level the diversification claim falls apart. `vol_transition` carries the entire $656 edge on BTC prod; the other two subs are drag or noise.

### Discipline implications

1. **Cell-carving is falsifiable.** Aggregate cohort-positivity + train→val corr are necessary but not sufficient. A meta-strategy can pass the statistical-significance gate while failing the cell-carving claim at the per-sub level.
2. **Cell-carving can PASS.** meta_post_cascade is the empirical existence-proof — the discipline isn't a forever-blocked hypothesis. Mechanism-coherent cell-carving is achievable when the sub-strategies map to genuinely distinct mechanism aspects (in this case: vol_spike_reversal = post-cascade volatility burst; bearish_capitulation_long = wash-out bottom; bullish_exhaustion_short = parabolic-top fade).
3. **Future meta-strategies can be designed for filter-PASS.** The pre-registration question for any new meta-strategy: "What is the prior that each sub will be net-positive on its own gate, and what observable would falsify it?" Where the prior is weak (e.g., one sub is mostly there for diversification optics), drop the sub from the design.
4. **Retroactive analysis is unblocked once a team engineer ships grid-run `sub_strategy` tagging** — all 10+ v1+v2 meta-strategy `grid_results.csv` files can be re-analyzed against this filter without re-running backtests.

See [[foundation/methodology/path-quality-diagnostics#per-sub-pass|path-quality-diagnostics §2.2]] for the filter's role in the deployment-decision gate stack, and [[foundation/methodology/path-quality-diagnostics#meta-post-cascade-first-per-sub-pass-mid-band-concentration-caveat|§3.2]] for the meta_post_cascade case in full context (per-sub PASS plus mid-band event-concentration caveat).

## Caveats

- **Scaffold ≠ mechanism in all cases.** Sometimes the scaffold is just gating, and the *mechanism* IS the secondary axis — the scaffold restricts when the mechanism fires but doesn't determine the P&L generator. This would manifest as <+0.4 correlation across same-scaffold strategies despite shared scaffold structure. The framework is **empirical-by-correlation**, not definitional-by-design.
- **Pairwise correlation matrix is a *necessary*, not sufficient, scaffold-identity check.** Two strategies with high correlation could share a scaffold OR could share a *regime sensitivity* (both fire only in trending bars regardless of scaffold construction). Distinguishing requires construction-level inspection: do the strategies share regime gate primary axes? entry-trigger primary axes? Without that, high correlation is suggestive, not conclusive.
- **N=7 (one regime, 2025-26 BTC perp 4h).** Threshold values (+0.6 / +0.4) are now better-supported but still working approximations; cross-window and cross-cohort validation still needed before promotion to firm rules. The N=7 cluster is internally consistent (full-symmetric strategies +0.586 to +0.754; long-only mutations +0.384 to +0.387) — but it's still one scaffold tested in one regime.
- **Single-window observation.** 2025-05/2026-04 BTCUSDT 4h only. Whether the same scaffold cluster persists in different regimes is the load-bearing missing test.
- **Scaffold catalog is informal.** Strategies may sit on multiple scaffolds simultaneously (e.g., HTF-bias *and* funding-regime). The taxonomy assumes a primary scaffold; ambiguous cases need explicit pre-registration of which scaffold dominates.
- **Regime-label mechanism-character must match the mechanism family** (negative corollary, N=1 worked failure, 2026-05-15). The framework's claim is that scaffolds CAN serve as mechanism; the negative corollary is that **a regime label carries mechanism-character that the strategy must match — assuming the regime is reversion-character when it's continuation-character produces N=2 family rejection**. Worked failure: the team's stable-range phase detector (`RollingZigzag + atr_mult=0.3 + threshold=65`) finds continuation-at-extremes character, not reversion-at-extremes — naive auction-theory range-fade rejected on both VA-edge-to-POC (v1) and literal-range-extreme (v2) framings; ablation (v2b) confirmed no slow-reversion edge cut short. See [[../rejected/auction-theory-range-fade-2026-05#detector-continuation-character|auction-theory-range-fade-2026-05 §detector continuation character]]. Discipline implication: when designing on a new regime-label scaffold, verify the regime's mechanism-character (continuation vs reversion vs neither) before mechanism-translation — separate from scaffold-cluster correlation check.
- **Per-symbol mechanism asymmetry** (N=3 codification, 2026-05-18 update with BTC continuation character codified independently). On phase-transition breakout events: BTC shows clean up-continuation (87.5% rate on N=8 ups, both directions profit at PSR 97.5%); ETH shows down-side fakeout-fade (down-breaks reverse 83%); SOL shows long-bias on BOTH directions (up-breaks continue 87.5%, down-breaks reverse 71% upward). The same `phase_breakout` mechanism family can't be applied uniformly — **per-symbol structural substrate determines which mechanism variant fires**. SOL's bidirectional long-bias is structurally explained by retail-leveraged derivatives + asymmetric institutional flow (ETF absorption + DAT buying + no basis-trade short flow on nascent CME SOL futures); BTC's continuation character is structurally explained by deep hedge-fund basis-trade infrastructure + thinnest stop layer + most-watched levels. See [[../../foundation/asset-classes/sol-market-structure|sol-market-structure]] and [[../../foundation/asset-classes/btc-market-structure|btc-market-structure]] for full per-symbol structural-substrate framing. **N=3 cross-mechanism evidence on BTC alone** (per [[../../foundation/asset-classes/btc-market-structure#empirical-anchor|btc-market-structure §empirical-anchor]]): `phase_breakout_meta_btc` (continuation works, PSR 97.5%) + `failed_breakout_phase_fade_btc` (fade fails, Sharpe -1.26, N=161) + `busy_hour_flow_continuation_btc` (continuation in unstable phase works, +$118; stable-phase mis-conditioned). Discipline implication: when claiming a mechanism transfers across symbols, audit the per-symbol structural substrate first — a working BTC mechanism may fail on ETH/SOL for structural-not-noise reasons.
- **Direction-restriction ablation methodology — position-blocking-as-inadvertent-filter** (N=2 cross-symbol evidence, 2026-05-19). Bidirectional position-management can act as an inadvertent quality filter — when a position in one direction is open, signals in the opposite direction are blocked. **Worked finding (ETH fakeout-fade)**: parent's short-side standalone Sharpe 0.91 (with bidirectional filter) > ablation Sharpe 0.49 (filter removed; 14 additional shorts net -$4.60/trade). **Worked finding (SOL long-bias)**: position-blocking NEUTRAL (parent's long-subset 0.44 ≈ ablation 0.44). See [[../rejected/failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] for the full ETH arc + cross-symbol comparison. **Methodology sharpening**: direction-restriction ablation comparison should be vs **parent's same-side SUBSET**, NOT vs parent aggregate. Position-blocking value-add hypothesis (N=2 codification candidate): correlates with symbol fakeout-fade character; structural-absorption character suppresses the filter's value.

## Companion to regime-gate-vs-bias-filter

[[trading/mechanisms/regime-gate-vs-bias-filter]] documents that an **entry-embedded directional bias filter outperforms a separate post-hoc regime gate** — N=4 supporting cases. That page concerns *how to combine* a regime axis with an entry signal in one strategy.

This page generalizes: once a scaffold is chosen, the *secondary* signal axes don't differentiate the strategy at the timing level. The two findings together imply:

- **Pick scaffold deliberately** (this page): scaffold determines timing; choose for diversification value.
- **Embed scaffold's directional axis in entry signal** (regime-gate-vs-bias-filter): once chosen, build it into entry rather than layering on top.
- **Do not stack regime gate on already-scaffolded strategy**: you'd be adding a second scaffold-component that then dominates the secondary signal axis you wanted to test.

## Operationalization status

This is wiki-encoded conceptual framework. **Not pipeline-implemented as automated check.** Strategy authors should manually compute timing correlation matrix when running batches that may share a scaffold; threshold flags above are guidance, not enforced.

Future engineering candidates:
- Automated scaffold-cluster detection in batch reports (compute pairwise Q5 P&L correlation; flag clusters at ≥+0.6)
- Pre-registration field for `predicted_timing_correlation_with: {strategy_X: <+0.4}` with auto-falsifier on grid run
- Scaffold catalog maintained alongside strategy registry

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC taxonomy]]) | Pattern stability | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Pattern derived from this regime** | Source data |
| Bearish continuation (e.g. 2026-Q1) | Pattern likely-stable — same regime dynamics | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Bullish trending | **Pattern likely persists structurally** — scaffold-determines-timing is a structural property of strategy construction, not regime-dependent. *Direction* of cluster's net edge would flip; identity-as-cluster would persist. | Theoretical |
| Chop / transitional | **Pattern uncertain** — if scaffolds depend on directional regime markers (bias-filter EMAs), all scaffolds may degrade simultaneously; clustering at low magnitude. | Theoretical |
| Cascade aftermath | **Scaffold identities may break down** during cascades — fast regime transitions can de-correlate timing as different secondary axes lag/lead. | Theoretical |

**Notes:**
- The framework is structural, not regime-derived: scaffold determines timing because of how the entry logic is built, not because of what the regime selects.
- What changes across regimes: the *direction* of the cluster's net edge (bullish vs bearish dominance), not the *identity* of the cluster.
- Cross-regime validation is the load-bearing missing test — only 2025-26 BTC perp data so far.

## Key Takeaways

- **Scaffold = the regime gate + entry trigger primary axes that determine when a strategy fires.** Secondary axes are signal expressions of the scaffold, not distinct mechanisms.
- **Empirical threshold (N=7 supporting): ≥+0.6 timing correlation = same mechanism class.** <+0.4 = different mechanism class. [+0.4, +0.6] = ambiguous, investigate. Long-only mutations halve cluster correlation but preserve scaffold signature (they fall to ~+0.38, near the lower threshold).
- **Diversification requires scaffold-distinct strategies**, not different secondary axes on the same scaffold.
- **"Novel signal axis" claims need pre-registered timing-correlation predictions** falsifiable on grid run.
- **HTF-EMA-bias scaffold dominates current lab** (7 of 18+ testable strategies; 4 of 5 in 2026-05-05; 4 of 6 in 2026-05-06) — biggest diversification gap.
- **Conditional P3 (direction-restriction) mutation operator**: P3 lifts edge only when the working-direction signal is *differentiated from the scaffold*. When the working direction IS the scaffold (#4 / #6), P3 is scaffold-residual — half the trades, identical per-trade economics. Diagnostic check before commitment.
- **First non-bias-scaffold mechanisms with material orthogonality** (2026-05-06): `funding_extreme_exhaustion_4h_v2` (corr ≤+0.07 with everything) and `vol_regime_transition_4h` (corr <+0.4 with all 5 bias-cluster baselines). The lab's diversification candidates.
- **Caveat**: framework is empirical-by-correlation, not definitional-by-design; high pairwise correlation is *necessary*, not sufficient, for shared-scaffold identification.

## Related

- [[trading/mechanisms/regime-gate-vs-bias-filter]] — companion page; how to combine scaffold's directional axis with entry signal (embedded > separate)
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — taxonomy of signal types (bias filter vs entry signal); foundational distinction
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law (trial count compounding); within-scaffold replication is one trial, not N
- [[foundation/methodology/deployment-edge-threshold]] — friction floor; same-scaffold strategies don't compound friction-overcoming edge
- [[foundation/macro/regime-conditional-edge-2025-26]] — W2 cross-strategy finding; the cohort generating scaffold evidence had concentrated edge in Q4 2025
- [[trading/mechanisms/vol-regime-transition-tradeable]] — first non-bias-scaffold mechanism with cleanly orthogonal signal (Phase B novelty falsifier passed; 2026-05-08 cohort-grade evidence at 99.6% cohort full-positive + train→val corr +0.886 upgrades the page seedling → growing, the lab's first cohort-grade non-scaffold result)
- [[trading/mechanisms/funding-cycle-dynamics]] — positioning-extreme is most-orthogonal-in-lab mechanism (corr ≤+0.07 with everything)

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"Cross-strategy correlation matrix" + §"Implications" points 3–4
- 2026-05-06 batch stocktake: the backtesting lab per-strategy verdicts + cross-strategy synthesis section (#1, #4, #6)
- 2026-05-06 cross-strategy correlation matrix: the backtesting lab (15 strategies)
- the backtesting lab Finding 2 (cross-strategy independence framework)
- 7 supporting wiki entries: `confluence_trend_4h-2026-05`, `confluence_trend_4h_v2` (forclosure run inside same entry), `taker_buy_asymmetric_4h-novelty-2026-05`, `volume_z_asymmetric_4h-asymmetry-2026-05`, `volume-z-moderate-2026-05`, `ema-bias-breakout-4h-v2-2026-05`, plus per-strategy `strategies/tested/<name>/<name>.md` files for #4 (`taker_buy_long_only_4h`) and #6 (`volume_z_long_moderate_4h`)
