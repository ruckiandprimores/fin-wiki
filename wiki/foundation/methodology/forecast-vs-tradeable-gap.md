---
title: "Forecast-vs-Tradeable Gap — when a real predictive effect doesn't translate to strategy edge"
status: growing
created: 2026-05-06
updated: 2026-06-11
tags: [foundation, methodology, forecasting-vs-trading, effect-magnitude, stop-width, trade-horizon, validation, slop-trap, day-swing, realization-gap]
---

# Forecast-vs-Tradeable Gap — when a real predictive effect doesn't translate to strategy edge

> **TL;DR**: A statistical effect on raw bars (e.g., "high-vol-down bars predict −0.09% over the next 4h") does **not** automatically translate to a tradeable strategy when stop width and trade horizon are incompatible with the effect magnitude. The 2026-05-05 `volume_z_asymmetric_4h` operationalization is the canonical worked example: a real W2 forecasting effect (n=63, direction stable) was operationalized with 2×ATR stops (~2% on BTC) and 3×ATR targets, multi-bar holds — and the asymmetric prediction **inverted** because trades hit stops or time-stops before the predicted continuation registered. The forecasting effect (~0.09%) was **~5% of stop magnitude** — too small to express within the trade mechanics. This page articulates the discipline: a forecasting effect is a *necessary* condition for tradeable edge, not sufficient. **Companion to** [[foundation/methodology/strategy-validation-discipline|strategy-validation-discipline]] (LdP Second Law — data independence) and [[foundation/methodology/deployment-edge-threshold|deployment-edge-threshold]] (friction floor — net-of-friction edge); this page documents the third translation-loss layer between *forecasting evidence* and *deployable strategy*.

## 1. The Gap, Stated

Three layers stand between "the data shows this pattern" and "this strategy makes money in production":

| Layer | What is being claimed | What can fail |
|---|---|---|
| **L1 — Forecasting evidence** | "Variable X predicts Y over horizon H, magnitude M, on dataset D" | Selection bias, multiple testing, data leakage (LdP Second Law) |
| **L2 — Tradeable strategy** | "A strategy operationalizing X predicts profitable trades with stop S, target T, hold H' in trade-mechanics frame F" | **Effect-magnitude / trade-mechanics incompatibility** — this page |
| **L3 — Deployable strategy** | "After friction (slippage, spread, impact, funding), the strategy clears the friction floor at realistic capital + position scale" | Friction floor (deployment-edge-threshold §4) |

Each layer is *necessary* for the next; **none is sufficient**. A clean L1 effect can fail L2; a clean L2 strategy can fail L3.

The wiki has documented L1 → L3 directly via [[foundation/methodology/deployment-edge-threshold|deployment-edge-threshold]] but the L1 → L2 layer was implicit. This page makes it explicit.

## 2. Worked Example — `volume_z_asymmetric_4h` (2026-05-05)

### The forecasting effect (L1)

W2 raw-data scan of BTCUSDT perp 4h 2025-05/2026-04 found:

- **Bars with high volume Z-score AND negative direction** (n=63 over the 1y window) predicted **−0.091% return on the following 4h bar**
- Effect direction was stable across the window
- Statistical: small-n but consistent

This is a real L1 finding: a clean predictive effect on raw bars.

Source: the backtesting lab Finding 4.

### The strategy operationalization (L2 attempt)

`volume_z_asymmetric_4h` operationalized the L1 finding:

- **Bias gate**: HTF-EMA-bias (`EMA(20) vs EMA(50)`)
- **Entry trigger**: volume Z-score extreme (>2.0) + bar-direction-aligned
- **Stop**: 2× ATR (≈ 2% on BTC at typical ATR)
- **Target**: 3× ATR (≈ 3%)
- **Hold**: multi-bar (until stop, target, or time-stop)
- **Asymmetric prediction (load-bearing)**: short ≥ 2× long per-trade ATP, based on the W2 finding that the effect concentrated on high-vol-down bars

### The result

| | Predicted | Observed |
|---|---|---|
| Long ATP | smaller | **+$1.527** |
| Short ATP | ≥ 2× long | **−$0.162** |
| Asymmetry direction | short dominates | long 9× short, **inverted** |

Source: the backtesting lab §"`volume_z_asymmetric_4h`" §"Asymmetry test".

The L1 effect was real. The L2 operationalization **failed and inverted** the predicted direction.

### Why the L2 failure happens

Decompose the magnitudes:

- **L1 effect magnitude**: −0.09% over 4h on raw bars
- **Stop magnitude (2× ATR)**: ~2% on BTC
- **Target magnitude (3× ATR)**: ~3%
- **Ratio**: L1 effect ≈ **5% of stop magnitude**, ≈ **3% of target magnitude**

When a trade enters on a high-vol-down bar with a 2% stop and 3% target, the trade is exposed to ~2% of bar-level noise *between* entry and stop/target. The L1 effect (−0.09%) is buried in the noise — random walk variation at hourly/4h scale on BTC dominates 0.09% drift many times over.

Multi-bar holds compound the problem. The L1 effect was measured at *exactly* +4h horizon. By holding the trade for multiple bars (waiting for stop or target), the strategy exits at a horizon where the L1 effect has decayed or reversed.

The asymmetry inverted specifically because the bearish-leaning effect (−0.09% on high-vol-down bars) was too small to overcome the bullish drift in the broader 2025-Q4 / 2026-Q1 sub-window, where the trend cluster was net-positive on long entries (per [[foundation/macro/regime-conditional-edge-2025-26]]). At the trade-mechanics scale, regime drift dominated bar-level forecasting drift.

### What the L1 effect is good for

The L1 effect is real evidence. It just doesn't translate to *this* trade-mechanics frame. It could translate to:

- **Sub-bar mechanics**: tighter stops (0.2% — equal to L1 effect magnitude) and same-bar exits would express the effect — but at retail-accessible 4h aggregation this isn't operationally feasible
- **Position-sizing modulation**: tilt position size based on the L1 signal rather than gating entries; preserve full mechanics
- **Different mechanism altogether**: long-only at *moderate* Z (1.0–2.0, not extreme >2) extracts a different (and larger) effect on the long side; that is the actual mutation the data points at

## 2A. Second worked example — `meta_session_v1` (2026-05-08)

A second N=1 case study, structurally similar to volume_z but on a different mechanism class — equity-source session-boundary effects rather than informed-flow microstructure. Useful as a second instance of the same general L2 translation failure mode on a different mechanism class, confirming the diagnostic is mechanism-class-invariant.

### The forecasting effect (L1)

2026-05-05 cross-strategy pattern mining (W2 raw addendum) reported forecasting effects at specific UTC hours on BTCUSDT perp 1h:

| Hour | L1 effect | Stability |
|---|---|---|
| h21 | +0.042% fwd-1h | 5/5 quarters positive |
| h22 | +0.058% fwd-1h | 5/5 quarters positive |
| h23 | −0.045% fwd-1h short bias | 5/5 quarters consistent |

Combined naive-360d accumulation suggested ~+23.5% gross annual at $1K (no friction modeling, no OOS, no stop-target geometry). The L1 effect is real at the forecast frame — confluence with the equity literature on session-boundary effects (Hasbrouck 2007, Harris 2003 Ch 28).

**Translation to strategy (L2).** [[trading/rejected/meta_session_v1-2026-05|`meta_session_v1`]] (2026-05-06, frame-fixed 2026-05-08 PM) built 4 sub-strategies on these hour-of-day patterns: h7 / h15 with bar-direction; h21–22 long; h23 short. Stops 0.75–1.0×ATR, targets 1.5–2.0×ATR, time_stops 1–2 hours. 500-config grid sweep.

**Result (2026-05-08 grid, 500 configs).**

| Metric | Value |
|---|---|
| Cohort full-positive ratio | **0% of 500** |
| Best val combo | −$98 |
| Cohort ATP | **−$0.57 per trade** (vs pre-registered ≥$0.50) |
| Trades/year | 1239 |

The L1 effect was real at forecast frame; the strategy operationalization produced uniform cohort-negative across all parameter combinations. **Hypothesis-level falsifier fired on all 4 sub-mechanisms.** Full rejection rationale: [[trading/rejected/meta_session_v1-2026-05|meta_session_v1-rejected]].

**Mechanism diagnosis.** Decompose the magnitudes:

- **L1 effect** (h21): +0.042% per fwd-1h
- **Friction floor at trade scale**: 1239 trades/year × ~8bp roundtrip = mechanism's gross-edge requirement of ~+0.08% per trade just to break even at the strategy's trade frequency
- **Ratio**: L1 effect ≈ **52% of friction floor** — below the break-even line by a factor of ~2×

Equivalent diagnostic check vs the §3 checklist:

| Question | Result |
|---|---|
| Q3.1 — Effect / stop magnitude ratio ≥3? | 0.042% / ~1% stop = 0.042 — **FAILS** by factor ~70× below the 3× target |
| Q3.2 — Trade horizon ≤ 1.5× L1 horizon? | L1 horizon 1h; strategy time-stop 2h → ratio 2 — **FAILS** marginally |
| Q3.3 — Mechanics compatibility | bar-close measurement → market-order-at-next-open execution + 8h funding mid-trade — **partial failure** |

All three diagnostic questions fail or partially fail. The structural failure signature is consistent with the §3 checklist's predictions.

**Exit-profile signature.** Time-stops fired more often than SL/TP combined:

| Exit | Count per 1239 | Share |
|---|---:|---:|
| Time-stop | 609.9 | 49% |
| SL | 472.6 | 38% |
| TP | 156.1 | 13% |

Trades dying in the middle at small per-trade P&L; friction (~$0.40/trade at $500 position scale) eats the entire gross edge. The mechanism couldn't sustain hold-through-bar-noise long enough for the L1 effect to register; per §3.2, trade horizon exceeded the L1 measurement horizon, and the strategy exits at points where the L1 effect has decayed or reversed.

**Lesson generalized.** Forecasting effects of magnitude ≤0.1% per fwd-N-bars at typical crypto-perp friction (~8bp roundtrip) require **either**:

1. **Very high firing frequency** where per-trade friction is amortized across many compounding occurrences — but `meta_session_v1` at 1239 trades/year is already in that regime and it's still not enough
2. **Stop-target geometry that doesn't introduce noise-driven exits** — single-bar exit (entry at bar close, exit at next bar close) would express the L1 effect cleanly, but at 1h frame with 2h time_stop, typical bar noise drives mid-trade exits before the effect can register

`meta_session_v1` had neither geometry. The mechanism diagnosis is decisive: this is the same L2 translation failure as volume_z, on a different mechanism class, with sharper magnitude diagnostics. **Two N=1 cases now from independent mechanism families** (informed-flow microstructure + equity session-boundary) confirm the L1→L2 translation gap is mechanism-class-invariant — it's about effect-magnitude / mechanics geometry, not about which mechanism class.

The mechanism that produced the equity-market session-boundary effect (forced-close mechanic) is also structurally absent in 24/7 crypto perpetuals — see [[trading/rejected/meta_session_v1-2026-05]] for the mechanism-translation failure analysis. The strategy fails for **two independent reasons** (L2 translation gap + mechanism doesn't translate across asset classes); the forecast-vs-tradeable diagnostic alone would have flagged the L2 failure pre-spec.

## 2B. The realization gap quantified — discrete-bar TP/SL exits capture ~50% of the forward-return signal {#realization-gap}

The two worked examples above show L2 translation *failing outright*. There is a milder, quantified version of the same loss that applies even when the translation **succeeds**: path-dependent TP/SL exits on discrete bars intrinsically capture only **~50%** of the theoretical end-of-window mean. Forward-signal strength does not translate proportionally to realized backtest Sharpe.

**Evidence (N=4, 2026-05-14 portfolio-build session; cell-conditioned 4h long strategies, v1 exits):**

| Strategy | Forward return / t-stat | Realized backtest Sharpe |
|---|---|---:|
| cell_accumulation_long_btc | +1.20% / t 5.19 | 1.47 |
| cell_bull_exhaustion_long_eth | +4.35% / t 10.49 | 0.86 |
| cell_stealth_accumulation_long_sol | +3.45% / t 8.57 | 0.87 |
| cell_accumulation_long_btc (4 exit variants v1–v4) | — | 1.32–1.47 (flat) |

Two load-bearing reads:

1. **The inversion**: the *highest* forward t-stats (ETH t 10.49, SOL t 8.57) realized the *lowest* Sharpe. Strong forecast ≠ strong tradeable strategy — the gap is intrinsic to discrete-bar exit mechanics, not a deficiency of the signal.
2. **Tuning exits has limited upside**: 4 exit variants on the same strategy all landed Sharpe 1.32–1.47. The realization fraction is a property of the path-dependent exit *class*, not of any particular exit configuration. For cell-conditioned 4h longs under v1 exits, ~Sharpe 1.5 was a realistic per-strategy ceiling regardless of forward-signal strength.

**Diagnostic use**: when a forward signal is strong but realized Sharpe disappoints, suspect the discrete-bar realization gap *before* assuming the exit config is wrong. Expect roughly half the theoretical mean to survive TP/SL path-dependence; budget the other half as structural loss when sizing `expected_net_edge` upstream.

**Provenance caveat**: primary evidence is the now-concluded cell-strategy discovery track (single window, 4h longs, one exit family). The ~50% figure is an anchor, not a law — but the *direction* of the claim (path-dependent exits < end-of-window mean; exit tuning is low-leverage) generalizes to any TP/SL strategy. Stop-geometry R:R signatures per mechanism family live at [[trading/mechanisms/universal-stop-target-prior]].

## 3. Diagnostic Checklist — Before Operationalizing a Forecasting Effect

Three questions to ask **before** spec'ing a strategy from a forecasting effect:

### 3.1 Is the effect magnitude ≥3× the typical stop width at the trade horizon?

**Working rule**: effect magnitude / stop width ≥ 3 → forecast *can* express within trade mechanics. Below that ratio, stop noise dominates the signal.

For the volume_z case: 0.09% / 2% = **0.045** (ratio ~5%, far below 3×). The diagnostic would have flagged the operationalization at design time.

For comparison, [[trading/backtest-results/direction-restriction-short-only-2026-05|bb_short_only]] has gross ATP $1.50 ≈ 0.15% on $1K — at 2×ATR stops, ratio is ~0.075, also low. The strategy survives because it has high *win rate* on the directional-asymmetric edge, not because the per-trade magnitude clears stop noise. This is borderline; many strategies in this regime sit here.

### 3.2 Does the trade horizon match the effect's predicted persistence?

The L1 effect was measured at **+4h forward return**. The strategy held trades for multiple bars (until stop, target, or time-stop = up to 24+ hours). Effect decay at longer horizons turns the original signal into noise.

**Working rule**: trade horizon should be ≤ 1.5× the L1 effect's measurement horizon. Longer holds invite effect decay; shorter holds risk premature exit before effect realizes.

### 3.3 Are bar-level mechanics compatible with the effect's discovery context?

L1 effects are typically measured on *idealized* bars: assume entry at bar close, exit at next bar close, no slippage, no spread cost, no minimum tick.

Trade mechanics impose:
- **Entry timing**: market or limit at next bar's open vs midnight bar close — these are different bars
- **Fill assumptions**: marketable orders cross spread; limit orders may not fill on small bars
- **Tick / lot size**: minimum trade unit constrains size for low-priced assets
- **Funding-rate settlements**: 8h funding cycles change the holding cost mid-trade

When the L1 measurement context is "bar-close-to-bar-close mid prices" but the trade mechanics are "market-order at next-bar-open through spread", the effect can be lost in the translation gap *before* even hitting stop/target dynamics.

## 4. Connection to LdP Second Law

[[foundation/methodology/strategy-validation-discipline|LdP Second Law]] requires that **expected-edge estimates use data independent of the test window** — same-data round-trips are overfit by construction. See [[foundation/methodology/deployment-edge-threshold|deployment-edge §4.5]].

The forecast-vs-tradeable gap is **downstream** of the Second Law. Even if an L1 effect is data-independent (literature-sourced, theoretical, or strictly held-out OOS), the L2 translation can still fail per the diagnostic checklist above. **Data independence is necessary but not sufficient for tradeable edge.**

The volume_z case shows this hybrid failure: the *mechanism literature* (informed-flow microstructure, Harris/Easley-O'Hara) is independent of the test window; the *effect-size estimate* used in strategy spec was partly W2-derived (honestly flagged in spec); even with cleaner L1 evidence, the L2 translation would have failed for the same magnitude/horizon reasons. **Both layers need to clear independently.**

Operational sequencing:

1. **L1 cleanup first**: confirm effect is from independent data (literature, OOS chunk, theoretical) — Second Law check
2. **L2 cleanup second**: run the diagnostic checklist (effect magnitude vs stop, horizon match, mechanics compatibility) — this page
3. **L3 cleanup third**: net-of-friction floor at realistic capital — deployment-edge §4

## 5. Connection to Deployment-Edge Threshold

[[foundation/methodology/deployment-edge-threshold|deployment-edge-threshold]] documents the **friction floor**: gross edge per trade must exceed slippage + spread + impact + funding by enough margin to be profitable at realistic capital scale.

The friction floor is one specific binding constraint *if L2 succeeds*. The forecast-vs-tradeable gap is the **more general translation-loss issue** that can fail before friction even gets evaluated:

- **Pre-friction L2 failure** (this page): the L1 effect doesn't survive translation into trade mechanics; gross edge collapses before friction layer
- **Post-L2 friction failure** (deployment-edge): gross edge survives translation but doesn't clear net-of-friction floor

Both are deployment-blockers. This page covers the first; deployment-edge covers the second.

## 5A. Engine-output interpretation gotchas

L3 → stakeholder-communication is its own translation-loss layer. Engine output fields are computed under specific conventions; reporting them directly to stakeholders without checking the convention can systematically misrepresent strategy expectations.

### `summary.json` annualized_return_pct uses a non-standard convention {#engine-output-gotcha-annualized_return_pct}

The the backtesting lab engine's `summary.json` field `annualized_return_pct` is **not** simple-annualized or compound-annualized from `net_pnl + window length`. It uses a different internal convention that produces values substantially higher than either standard convention.

**Worked example (v6 BTC prod, 2026-05-11):**

| Computation | Value |
|---|---:|
| net_pnl | $556 over 2y on $1,000 |
| Simple annualized: $556 / 2 / $1,000 | **27.8%/yr** |
| Compound annualized: (1.556)^(1/2) − 1 | **24.7%/yr** |
| Engine `annualized_return_pct` | **104.18%/yr** |

The engine's internal Calmar consistency is preserved (Calmar = `annualized_return_pct` / `max_dd` = 104.18 / 29 = 3.58) but the `annualized_return_pct` component itself is non-standard. **Reporting "104%/yr" to stakeholders would significantly overstate expectations.**

**Operating discipline until engineering fix lands:**

1. For stakeholder-facing return numbers, compute simple or compound annualization manually from `net_pnl` + window length. Do **not** cite `annualized_return_pct` directly.
2. Do not compose engine-reported `annualized_return_pct` values across multiple strategies to derive portfolio-level annualized returns — the composition math will be systematically wrong.
3. `sharpe_ratio` is computed independently and trusted.
4. Calmar is engine-consistent but uses the same non-standard ann_return — flag if cited; better to recompute Calmar manually as (simple-annualized-return / max_dd) if reporting it.

**Engineering follow-up.** A companion task is routed to a team engineer (the research task queue) to either (A) change the engine computation to a standard convention, or (B) document the convention explicitly in the engine output schema. Once resolved, this sub-section can be revised or retired.

## 6. Implications for Strategy Pre-Registration

Strategy specs that derive an entry signal from a forecasting effect should pre-register:

1. **The L1 effect**: variable, measurement window, effect magnitude, effect horizon, data independence per LdP Second Law
2. **The diagnostic checklist verdicts**:
   - Effect magnitude / stop width ratio (target ≥ 3)
   - Trade horizon / effect horizon ratio (target ≤ 1.5)
   - Bar-mechanics compatibility (qualitative; flag known incompatibilities)
3. **The L2 falsifier**: predicted ATP magnitude *consistent with* the L1 effect surviving translation; falsifier-fires-if observed ATP < some-fraction of L1-implied ATP

If the diagnostic checklist verdicts are unfavorable (ratio <3, horizon >1.5×, mechanics incompatible), the strategy spec should explicitly acknowledge that it is operationalizing a *theoretical* mechanism, not the *measured* L1 effect — and the falsifier should be designed accordingly.

The volume_z case retroactively illustrates: had the spec pre-registered "effect magnitude 0.09% / stop 2% = 0.045 ratio (FAILS diagnostic 3.1)", the team would have reframed the spec to either (a) tighten stops to 0.2% (incompatible with 4h volatility), (b) shorten hold to single-bar exit (would have been the cleaner test), or (c) abandon the asymmetric-prediction spec and frame as long-only-moderate-Z directly.

## 7. Operationalization Status

This is wiki-encoded discipline. **Not pipeline-implemented as automated check.** Strategy authors apply the diagnostic at design time.

Future engineering candidates:
- Pre-registration field for `l1_effect: {magnitude, horizon, source}` + auto-computed diagnostic-checklist values from spec parameters
- Auto-flag in strategy review when L1 effect magnitude / stop ratio < 3
- Standardized L2 falsifier template that scales L1 effect through to expected ATP

## 8. What This Page Does NOT Claim

- **Does not claim every forecasting effect needs ratio ≥3 to be tradeable.** Strategies can be profitable with smaller ratios via high win rates, position sizing, or compounding effects. The diagnostic is a flag, not a gate.
- **Does not claim L1 effects are useless when L2 fails.** Failed L2 translations are informative — they can point to position-sizing modulation, different trade-mechanics frames, or different mechanisms entirely (as the volume_z long-only-moderate-Z mutation illustrates).
- **Does not invalidate the underlying mechanism literature.** Informed-flow microstructure (Harris/Easley-O'Hara) is real; volume clustering is real. The L2 translation is what fails, not the L1 evidence.
- **Does not replace the falsifier discipline.** Pre-registered falsifiers for individual strategies are still required; this page adds an upstream design check before falsifier construction.

## Key Takeaways

- **Three layers between data and deployment**: L1 forecasting evidence → L2 tradeable strategy → L3 deployable strategy. Each is necessary; none sufficient for the next.
- **L2 failure mode**: when L1 effect magnitude is too small relative to stop width, trade horizon exceeds L1 measurement horizon, or bar mechanics differ from L1 measurement context — the strategy doesn't express the effect.
- **Diagnostic checklist** (3 questions): magnitude ratio ≥3, horizon ratio ≤1.5, mechanics-compatibility check.
- **Worked example**: volume_z's W2-derived −0.09% fwd-4h effect inverted in operational form because effect was 5% of stop magnitude.
- **Realization gap (~50%)**: even when L2 succeeds, discrete-bar TP/SL exits capture only ~half the theoretical end-of-window mean (N=4, including a forward-t-stat/Sharpe inversion); exit tuning has limited upside. Suspect this before blaming the exit config.
- **Data independence (LdP Second Law) is necessary but not sufficient.** L2 translation is a separate layer.
- **Friction floor (deployment-edge) is one specific L3 binding constraint** *after* L2 succeeds; this page covers the more general L2 translation issue that can fail upstream.
- **Operational sequencing for new strategies**: clean L1 (Second Law) → clean L2 (this page diagnostic) → clean L3 (deployment-edge friction floor).

## Related

- [[foundation/methodology/strategy-validation-discipline]] — LdP Second Law (data independence); upstream L1-cleanup discipline
- [[foundation/methodology/deployment-edge-threshold]] — friction floor; downstream L3-cleanup discipline; specifically §4.5 (predictive vs descriptive estimation)
- [[trading/rejected/volume-z-asymmetric-4h-asymmetry-2026-05]] — first canonical worked example (forecast inverted in operational form)
- [[trading/rejected/meta_session_v1-2026-05]] — second canonical worked example (forecast below friction floor at trade scale); confirms diagnostic is mechanism-class-invariant
- [[trading/mechanisms/scaffold-as-mechanism]] — companion design framework; L2 translations interact with scaffold choice
- [[trading/mechanisms/universal-stop-target-prior]] — stop/target R:R priors per mechanism family; §2B's realization fraction is what those geometries actually capture of the forward signal; its multi-horizon coherence check is the position-design-level operationalization of this page's stop-width/horizon incompatibility argument
- the wiki schema Part I §4 — Structured Hypothesis Format (this page operates downstream of `expected_net_edge` schema field)

## Sources

- 2026-05-05 batch stocktake: the backtesting lab §"`volume_z_asymmetric_4h`" + §"Implications + decisions for next iteration" point 5
- W2 raw-data scan: the backtesting lab Finding 4 (high-vol-down bars predict −0.091% fwd-4h)
- Auto-memory: a standing convention (Second Law); a standing convention (the discipline cycle that surfaced this)
- López de Prado 2018 *Advances in Financial Machine Learning* — Marcos' Second Law (Ch 11.5); foundation for the data-independence layer
