---
title: "Vol Regime Detection — Vol Cone, RV Estimators, IV/RV Spread"
status: seedling
created: 2026-05-08
updated: 2026-05-08
tags: [mechanism, volatility, vol-cone, regime-detection, rv-estimators, iv-rv-spread, sinclair, day-swing-applicable, embedded-bias-filter, capability-gap-options-data]
---

# Vol Regime Detection — Vol Cone, RV Estimators, IV/RV Spread

> **TL;DR**: A **regime classifier built from realized volatility percentiles** (the vol cone) plus the spread between implied and realized vol. Operates as an **embedded gating attribute** for any volatility-sensitive strategy: tells you whether you're in a "vol expansion expected" regime, "vol contraction expected" regime, or noise. Two operational primitives: (1) **vol cone** — store percentile bands of RV across multiple lookback windows (1d, 7d, 30d, 90d) and compare current RV to that distribution; (2) **IV-RV spread** — variance premium signature (IV typically above RV; how far above signals positioning/fear). Sinclair (2013) is the practitioner reference. Crypto translation is direct: vol cones built on BTC perp 4h returns, with DVOL/BVIV providing the IV side. **Status: seedling pending DSL primitives** — `realized_vol(window)`, `iv_at(tenor)`, and percentile-rank features are not yet in the backtesting lab.

## Mechanism Story

### Two stylized facts that anchor the regime

From Sinclair Ch 3 (stylized facts), confirmed across decades of equity, commodity, FX, and now crypto data:

1. **Volatility clusters and mean-reverts.** Today's RV is the best estimator of tomorrow's RV (clustering); over multi-week windows, vol pulls toward its long-term mean (mean reversion). The interplay produces *regime persistence* — once vol is high, it stays high for a while, then collapses; once low, it stays low.
2. **Volatility distribution is heavily right-skewed (approximately log-normal).** Vol spends much more time in low states than in high states; the median is well below the mean; rare high-vol regimes contribute most of the variance-of-vol.

Stylized fact 1 makes regime classification *useful* (the regime predicts the next bar). Stylized fact 2 makes percentile-based classification *robust* (median bands are stable; mean-based bands chase outliers).

### Why percentile bands rather than absolute thresholds

A naive threshold ("RV > 30% = high vol") fails when applied across markets or regimes — 30% is high for SPY, low for BTC, normal for an altcoin. **The vol cone is the operational fix**: store the empirical 10/25/50/75/90 percentile bands of RV over a rolling lookback (typically 2-4 years), and classify current RV by where it sits in that distribution. This is regime detection as *relative* state, not absolute level.

Sinclair (Ch 4):

> "Selling one-month implied volatility at 35 percent because this is in the 90th percentile for one-month volatility over the past two years can form the basis of a sensible trading plan. Selling 35 percent because GARCH is forecasting the realized volatility to be 20 percent is less sensible."

The vol cone is the operational form of Sinclair's discipline: **the point forecast isn't as important as the possible distribution of results.**

### Why IV-RV spread complements the cone

The vol cone tells you about RV; IV adds a second signal. Three regimes appear when the two are crossed:

| RV cone state | IV-RV spread | Interpretation | Typical action |
|---|---|---|---|
| Low percentile (< 25th) | High (IV >> RV) | Variance premium expanded; markets pricing future risk that hasn't materialized | Sell vol (capture premium) |
| Low percentile | Low or negative (IV ≈ or < RV) | Quiet regime; market doesn't expect breakout; consensus calm | Don't trade vol; wait for catalyst |
| Mid percentile (25-75th) | Mild (IV slightly > RV) | Normal regime | Strategy-specific |
| High percentile (> 75th) | High (IV >> RV) | Vol panic + fear premium | Often best vol-selling moment historically (per Sinclair Ch 11 — but with massive DD risk) |
| High percentile | Low or negative | Vol blowing up unhedged; positioning broken | Don't fade — let it run; catastrophe regime |

The IV-RV cross is **the operational signature** of behavioral state. A standalone RV-cone tells you what *was*; the IV side tells you what the option market *expects*.

## Realized Volatility Estimators — Choose Carefully

Sinclair Ch 2 catalogs five RV estimators with different efficiency-vs-bias tradeoffs:

| Estimator | Inputs | Sampling efficiency vs Close-Close | Bias | Crypto fitness |
|---|---|---|---|---|
| **Close-to-Close** | C_t | 1× (baseline) | Unbiased (large N), low for small N (Jensen) | Works; throws away O/H/L |
| **Parkinson (1980)** | H, L | ~5× more efficient | Biased low (discrete sampling) | Direct apply; recommend bias correction |
| **Garman-Klass (1980)** | O, H, L, C | ~8× more efficient | Biased low | Direct apply for 24/7 markets — **best practical choice for crypto** |
| **Rogers-Satchell-Yoon (1991/1994)** | O, H, L, C | High efficiency; drift-robust | Slight downward (continuity assumption) | Apply where there's a known drift — useful for trending crypto |
| **Yang-Zhang (2000)** | O, H, L, C + overnight gap | Up to 14× efficient when overnight jumps are not dominant | Slightly high under realistic conditions | **Reduces to Garman-Klass for crypto** (no overnight gap in 24/7 trading) |

**Operational recommendation for crypto**:
- **Default**: Garman-Klass on hourly or 4h OHLC. Fast convergence, drift-tolerant, no overnight-gap component to worry about.
- **For trending markets**: Rogers-Satchell-Yoon if you suspect significant drift in the lookback window.
- **For long-window stability**: Close-to-close on daily — slowest but most-understood.

**Sample-size discipline** (Sinclair Ch 2, the 30-day-vol problem): a 30-period close-to-close RV has ±25% sampling-error 95% CI. **Don't treat single-window RV as a precise number — treat it as a draw from a distribution.** The vol cone *is* this distribution.

### High-frequency adjustment for crypto

Sinclair (Ch 2) notes that in equities the overnight return contains ~15% of total variance for stocks, ~10% for indices. In **24/7 crypto** there is no overnight gap — but there *is* intraday seasonality at session boundaries (Asia/EU/US handoffs) per [[trading/mechanisms/session-boundary-effects]]. For RV estimation in crypto:
- Default to symmetric clock (Garman-Klass / RSY on intraday OHLC).
- For session-aware applications (e.g., gating by EU-open session vs Asia-quiet session), build separate RV cones per session bucket.
- Sampling at < 5min crypto intervals introduces microstructure noise (bid-ask bounce) — Sinclair recommends 15-30min as the sweet spot; for crypto, 1h or 4h bars are practical.

## Hypothesis (structured, per Part I §4)

```
Hypothesis:    During regime A (low RV percentile + high IV-RV spread),
               variance-premium-capture strategies (short vol or
               vol-fade) have meaningfully positive expected edge.
               During regime B (high RV percentile + IV-RV spread
               compressing), vol-expansion strategies (long vol,
               trend continuation) have meaningfully positive edge.
               During regime C (mid-percentile + mild spread),
               vol-conditional strategies are NOT load-bearing —
               trade something else.

Mechanism:     Vol-clustering (today's RV ≈ tomorrow's RV) makes
               regime-persistent within multi-bar horizon. Mean-
               reversion drags extreme regimes back to the cone
               median. Variance premium reflects insurance demand
               + market-maker risk aversion + behavioral
               overestimation of crash risk. The IV-RV spread
               is a behavioral fear barometer; the vol cone is
               a regime-state classifier.

Observable:    - 30-day RV at 5th-percentile band on the 2-year cone
               - 30-day at-the-money IV at 30 vol points
               - IV-RV spread = 30 - 12 = 18 vol points (high)
               → Regime A (sell-vol candidate)

               - 30-day RV at 90th-percentile band
               - 30-day ATM IV moderating (was 80, now 60)
               - IV-RV spread compressing
               → Regime B (vol-expansion likely done; long-vol fading)

Expected:      Direction: regime-conditional (short vol in A; long
                          vol or vol-conditional trend in B)
               Magnitude: variance-premium captures 2-5 vol-points
                          per cycle on Deribit ATM 30d
                          (literature average; subject to crypto
                          regime sample variability)
               Time horizon: regime persistence ranges 5-30 days
                             in BTC; sometimes longer in deep
                             low-vol regimes
               expected_net_edge: regime-detection on its own is
                                  NOT a strategy — it's a gating
                                  attribute. Expected edge is
                                  *delta* between gated-regime
                                  performance and unconditional
                                  performance for the strategy
                                  it gates.
                 - source:       Sinclair Ch 11 (Coval-Shumway 2001
                                 Sharpe 1.2 weekly straddle sales
                                 on equity index; Sinclair's QQQ
                                 backtests 2000-2010 Sharpe 1.1-
                                 1.3 unconditional, sub-VIX-35
                                 filter smooths)
                 - value:        when used as gating attribute on
                                 a strategy with regime-conditional
                                 edge, the *delta* often runs
                                 +30-50bp Sharpe improvement
                                 historically; absolute edge is
                                 strategy-dependent
                 - independence: Sinclair backtests on 2000-2010
                                 SPY/QQQ — independent of any
                                 crypto deploy window. Regime
                                 framework is theoretical /
                                 mechanism-based.

Falsifier:     Vol-cone regime tagging fails to predict
               next-bar / next-week RV better than chance:
                 - Cone-percentile-based regime label has zero
                   predictive correlation with future-bar
                   realized vol (correlation < 0.05 over n>100)
                 - IV-RV spread has zero predictive correlation
                   with subsequent vol-strategy P&L over n>50
                   trades
               Either falsifier alone fires → mechanism dormant
               in tested regime; document and move on.
```

## Vol Cone Construction — Operational Procedure

Per Sinclair Ch 4 (Burghart-Lane 1990 + Hodges-Tompkins 2002 overlapping-data correction):

1. **Choose lookback windows.** For crypto day/swing: typically {1d, 7d, 14d, 30d, 90d}. For longer-horizon: add {180d, 365d}.
2. **For each window N, compute rolling N-period RV** over a 2-year (or 4-year, longer if regime-stable) historical sample.
3. **For overlapping rolling windows, apply Hodges-Tompkins adjustment factor** `m = 1 / (1 - h/n + (h²-1)/3n²)` where h = window length, n = number of subseries — corrects for variance bias from overlap. For 60-day variance from 1006-day series, factor ≈ 1.06 on variance, ~1.03 on vol. For most operational use, this adjustment is small enough to ignore at default windows.
4. **Compute percentile bands** at {5, 10, 25, 50, 75, 90, 95}.
5. **At runtime**, compute current RV at each window, look up its percentile rank, and use that as the regime feature.

| Output feature | Type | Description |
|---|---|---|
| `rv_pct_rank_1d` | float ∈ [0, 1] | Where current 1-day RV sits in 2-year distribution |
| `rv_pct_rank_30d` | float ∈ [0, 1] | Where current 30-day RV sits |
| `rv_regime_30d` | enum | low / mid / high based on band |
| `iv_rv_spread_30d` | float (vol points) | IV(30d ATM) − RV(30d) |
| `iv_rv_spread_pct_rank` | float ∈ [0, 1] | Where current spread sits in 2-year distribution |

## Use as Embedded Bias Filter

Per [[trading/mechanisms/regime-gate-vs-bias-filter|regime-gate-vs-bias-filter]], the wiki's discipline is **embedded bias filter > separate regime gate**. Vol-regime detection should be integrated into a strategy's entry logic as a gating *attribute*, not as a separate post-hoc filter.

**Example integration patterns**:

| Strategy family | Vol-regime integration |
|---|---|
| Mean-reversion (e.g., funding extreme, range-fade) | **Only fire when `rv_pct_rank_30d > 0.5`**: high-vol regime where reversion magnitude > friction floor |
| Trend continuation (e.g., breakout, momentum) | **Only fire when `rv_pct_rank_7d > 0.6`**: trend continuation hits friction floor in low-vol regime |
| Premium-capture (short vol) | **Only fire when `iv_rv_spread_pct_rank > 0.7`**: capture only when spread is meaningfully expanded |
| Vol-expansion (long vol, straddle) | **Only fire when `rv_pct_rank_30d < 0.3` AND scheduled-event-imminent**: low-vol pre-event |
| Vol-regime-transition (e.g. [[trading/mechanisms/vol-regime-transition-tradeable]]) | **Native dependency**: this strategy IS a vol-regime transition signal |

Without an embedded vol-regime gate, vol-sensitive strategies waste trades on noise regimes — the friction-floor problem the wiki keeps surfacing. Per the **"regime as conditioning attribute"** discipline ([[trading/mechanisms/regime-gate-vs-bias-filter]]).

## Crypto-Specific Considerations

| Element | Equity (Sinclair) | Crypto (Deribit + perps) | Translation |
|---|---|---|---|
| RV computation | OHLC bars; overnight gap = 15% of variance | OHLC bars; no overnight gap | Use Garman-Klass (drops overnight term) |
| IV source | VIX (model-free), ATM options | DVOL (Deribit's VIX equivalent), BVIV (Volmex), per-strike Deribit options | DVOL ≈ VIX directly; per-strike is thinner |
| Cone depth | 4-year US-equity RV history is plenty | 4-year BTC history covers 2 cycles; data quality good post-2020 | Cone depth = 2-3 yr practical; 4yr risks regime non-stationarity |
| Regime sample diversity | Bull / bear / crisis / quiet (multiple cycles) | Bull cycle 2020-21, bear 2022, recovery 2023-24, peak 2025; 1-2 cycles | **Bigger overfit risk** — re-validate cone if regime shifts |
| Variance premium | ~3 vol-points S&P 30d | ~2-5 vol-points BTC 30d (DVOL vs RV); subject to crypto-cycle regime | Premium is real; magnitude varies more |
| Mean reversion in IV | Strong (VIX Bollinger-band strategy 62% wins per Sinclair) | Likely; not yet directly tested for DVOL | **Open question** — empirical test pending |
| Skew/leverage-effect sign | Stable (bear → vol↑) | Regime-conditional (bull mania can flip sign) | Document the sign per regime; per [[foundation/macro/btc-regime-taxonomy-2020-2025]] |

## Adjacent Mechanisms — Distinction

### vs. [[trading/mechanisms/vol-regime-transition-tradeable|vol-regime-transition-tradeable]]

vol-regime-transition is a *strategy* that fires on vol-regime *changes* (regime A→B at the moment of transition). vol-regime-detection is a *gating attribute* that classifies the current regime. The transition strategy CONSUMES the detection feature.

### vs. [[trading/mechanisms/regime-gate-vs-bias-filter|regime-gate-vs-bias-filter]]

regime-gate-vs-bias-filter is the wiki's **architectural discipline** (embed regime conditioning in strategy logic, don't post-hoc gate). vol-regime-detection is **one specific regime axis** that complies with that discipline. The wiki may eventually have multiple regime axes (vol regime, trend regime, funding regime, on-chain regime); vol-regime is the first formally articulated.

### vs. [[trading/mechanisms/scaffold-as-mechanism|scaffold-as-mechanism]]

Scaffold-as-mechanism observes that strategies sharing a *signal scaffold* (e.g., HTF-EMA-bias) cluster at trade-timing correlation regardless of secondary axis. Vol-regime-detection is a candidate scaffold-DISTINCT axis: a strategy gated by vol regime should cluster differently from a strategy gated by HTF-EMA-bias. **The diversification path** for the lab is exactly this — see [[foundation/sources/sinclair-volatility-trading]] §"Why this source matters" point 1.

### vs. [[foundation/methodology/forecast-vs-tradeable-gap|forecast-vs-tradeable-gap]]

Vol-cone percentile is a **forecast (L1)** — does not yet pay anything. To translate to L2 (tradeable strategy) requires (a) effect/stop ratio ≥ 3 (vol-regime gate must have effect size large vs the strategy's typical stop), (b) horizon match (regime persistence horizon must match strategy holding horizon), (c) mechanics compatibility. The vol-regime feature passes these gates only when paired with a strategy whose mechanism aligns with the regime — naked vol-regime-prediction is L1-only.

### vs. [[trading/mechanisms/variance-risk-premium-crypto|variance-risk-premium-crypto]] (NEW 2026-05-18)

vol-regime-detection is the **regime classifier**; variance-risk-premium-crypto is a **harvest mechanism** that USES the regime classification. They compose: vol-regime-detection identifies LV vs HV cluster (per Almeida 2024); variance-risk-premium-crypto exploits the LV cluster's 0.17 premium (vs HV cluster's 0.12). The counterintuitive Almeida finding — VRP is LARGER in LOW-vol regimes — is a sharpening of the vol-regime layer: low-vol regimes are not "boring / no edge" regimes; they're "highest insurance-premium-harvest" regimes for options-based strategies.

## Operationalization Status

| Element | Wiki encoded | DSL primitive | Backtested |
|---|:---:|:---:|:---:|
| Mechanism articulation (this page) | ✅ | n/a | n/a |
| Vol cone construction (procedure) | ✅ | ❌ — `realized_vol(window)`, percentile-rank | ❌ |
| RV estimators (Garman-Klass etc.) | ✅ documented | ❌ — `garman_klass_rv(period)` | ❌ |
| IV ingest (DVOL / Deribit per-strike) | ✅ flagged need | ❌ — no IV feed in pipeline | ❌ |
| IV-RV spread feature | ✅ | ❌ — composite of above | ❌ |
| Regime-conditional integration in existing strategies | ✅ pattern documented | ❌ — needs DSL gating | ❌ |

**Net**: this is **wiki-encoded as a research direction + design pattern**; nothing is yet backtested. DSL gap (RV estimator primitives, IV ingest, percentile-rank features) is engineering work. Per Apr 28 division of labor.

## Capability Gap (DSL + Data)

For the backtesting lab integration:

1. **`realized_vol(window, estimator)`** — Garman-Klass / Rogers-Satchell / Yang-Zhang on the configured bar series. Window in periods.
2. **`pct_rank(feature, lookback)`** — generic percentile-rank within rolling lookback. Useful beyond vol; required for cone implementation.
3. **`iv_at(tenor)`** — implied vol pull from DVOL / Deribit per-strike feed; tenor in days.
4. **`iv_rv_spread(tenor)`** — composite of above two.
5. **DVOL futures + per-strike Deribit options data ingest** — separate engineering project; gates everything IV-side.

Without these, no strategy can express vol-regime gating natively; current pipeline's only RV-style feature is short-window stdev which has none of the percentile-rank discipline. Engineering work owner-flagged.

## Honest Caveats

- **Cone depth vs regime non-stationarity tradeoff is real.** Too-deep cone (4yr+) includes regimes that no longer exist; too-shallow (6mo) lacks sample diversity. 2-year cone is the practical default but should be re-tuned per regime shift.
- **Sample-error on RV is large.** 30-day RV has ±25% 95% CI per Sinclair; treat any single value as imprecise and rely on the cone distribution for context.
- **IV-RV spread sign is regime-dependent in crypto.** Sinclair documents persistent IV > RV in equity indices. Crypto literature is thinner; spread can flip during cascades. Don't assume always-positive.
- **Mean-reversion-of-vol is a stylized fact, not a guarantee.** Crisis regimes can persist months; vol-regime gating that assumes mean-reversion-within-30-days will misfire in 2008-2009-style or 2022-FTX-style aftermath.
- **Interaction with funding cycle / scheduled events.** Vol-regime classification operates at multiple timescales; a low-vol regime two days before halving is NOT the same as a low-vol regime in mid-cycle. Pair regime classification with calendar-effect awareness.
- **Capacity.** Variance-premium-capture strategies have shown decay in equity markets post-2010 (see Sinclair's caveats and the academic literature); in crypto the premium is currently larger but is partially crowding-driven (more vol-sellers entering Deribit each year).

## Key Takeaways

- **Vol cone + IV-RV spread = regime classifier** for any vol-sensitive strategy.
- **Use as embedded bias filter, not post-hoc gate** (per [[trading/mechanisms/regime-gate-vs-bias-filter|wiki discipline]]).
- **Garman-Klass is the practical default RV estimator** for 24/7 crypto markets.
- **Treat single-window RV as imprecise**; the cone distribution is the operational object.
- **Crypto translation is direct** with adjustments for (a) overnight component absent, (b) regime non-stationarity, (c) skew/leverage sign regime-dependent.
- **Capability gap blocks deployment**: DSL primitives (RV estimators, percentile-rank, IV ingest) are engineering pre-reqs. Mechanism is wiki-encoded as research direction.
- **First scaffold-distinct mechanism in the wiki's diversification gap** — pairs with [[trading/mechanisms/funding-cycle-dynamics|funding-cycle-dynamics]] (perp side) and [[trading/mechanisms/options-gamma-perp-effects|options-gamma-perp-effects]] (options-flow side) to populate the vol/options family.

## Related

- [[foundation/sources/sinclair-volatility-trading]] — primary source
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — companion methodology layer for backtest discipline
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — embedding discipline
- [[trading/mechanisms/scaffold-as-mechanism]] — diversification gap context
- [[trading/mechanisms/vol-regime-transition-tradeable]] — strategy that consumes regime detection
- [[trading/mechanisms/term-structure-dynamics]] — companion vol-mechanism page (this ingest)
- [[trading/mechanisms/skew-based-positioning-signals]] — companion vol-mechanism page (this ingest)
- [[trading/mechanisms/options-gamma-perp-effects]] — adjacent options-flow mechanism
- [[trading/mechanisms/gamma-exposure-regime]] — **added 2026-06-07**: dealer-gamma (GEX) sign as a complementary vol-state classifier (positive = vol-dampening, negative = vol-amplifying)
- [[foundation/methodology/online-regime-detection]] — **added 2026-06-07**: prospective (filtering) regime methods; this vol-cone is a filtered vol-state input
- [[foundation/sources/msgarch-crypto-vol-regimes]] — **added 2026-06-07**: model-based vol-state classifier (MSGARCH one-step-ahead); complements this model-free vol-cone
- [[foundation/sources/deribit-insights]] — **added 2026-06-07**: crypto-native term-structure-state vol-regime classification + DVOL
- [[foundation/methodology/regime-marker-data-sources]] — **added 2026-06-07**: where the IV/DVOL data sits (Deribit feed = paid)
- [[trading/mechanisms/funding-cycle-dynamics]] — perp-side analog
- [[trading/mechanisms/calendar-effects]] — regime-event sibling family
- [[trading/mechanisms/session-boundary-effects]] — intraday seasonality interaction
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — when this mechanism is most/least useful
- [[foundation/macro/regime-conditional-edge-2025-26]] — empirical case for regime conditioning
- [[foundation/methodology/strategy-validation-discipline]] — pre-registration discipline for vol-strategy testing
- [[foundation/methodology/deployment-edge-threshold]] — net-edge thresholds
- [[foundation/methodology/forecast-vs-tradeable-gap]] — L1 vol-cone forecast → L2 strategy translation gates
- [[glossary/vol-cone]] — defined term
- [[glossary/variance-premium]] — defined term
- Part I §4 — structured-hypothesis format compliance

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 2 (RV estimators), Ch 3 (stylized facts), Ch 4 (vol cone, variance premium), Ch 5 (IV dynamics)
- Burghart & Lane (1990) — original vol cone paper (cited by Sinclair Ch 4)
- Yang & Zhang (2000) — drift-independent OHLC RV estimator
- Garman & Klass (1980); Parkinson (1980); Rogers, Satchell & Yoon (1991/1994) — RV estimator family
- Hodges & Tompkins (2002) — overlapping-data adjustment
- Cont (2001) — survey of stylized facts referenced by Sinclair
- Coval & Shumway (2001) — variance premium quantification
- 2026-05-08 ingest task — wiki:2026-05-06-ingest-sinclair-volatility-trading
