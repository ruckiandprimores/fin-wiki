---
title: "Skew-Based Positioning Signals — Sticky Strike, Sticky Delta, Risk Reversal"
status: seedling
created: 2026-05-08
updated: 2026-05-08
tags: [mechanism, volatility, skew, risk-reversal, sticky-strike, sticky-delta, behavioral-positioning, sinclair, day-swing-applicable, capability-gap-options-data]
---

# Skew-Based Positioning Signals — Sticky Strike, Sticky Delta, Risk Reversal

> **TL;DR**: The implied-volatility *strike dimension* (skew) carries two distinct mechanism types: **(A) skew-as-positioning-indicator** — the slope of IV across strikes reveals net market positioning (puts richer = bearish positioning; calls richer = bullish/lottery-buying); a sharp shift in skew is a positioning-unwind precursor. **(B) sticky-strike vs sticky-delta dynamics** — when the underlying moves, IV-strike-curve either stays put (range-bound) or floats with delta (trending); each rule admits its own arbitrage and tells you the regime. Sinclair (Ch 5) is the practitioner reference. **Crypto-specific finding**: skew sign is *regime-conditional* in crypto (puts richer in bear markets, calls richer in bull mania) — inverse to equity which is structurally put-skewed. Skew-flip detection becomes a regime-transition signal. **Status: seedling pending Deribit per-strike IV-feed ingest.**

## Mechanism Story

### A. Why Skew Exists (Sinclair Ch 5)

The IV smile is not a random shape. It's structurally produced by the interaction of:

1. **End-user demand** — equity longs naturally buy downside protection (puts) and sell upside calls (covered call yield). This bids puts and offers calls → put-skew (puts richer than calls).
2. **Takeover hedge** — for individual equities, upside calls trade at premium because of takeover-jump risk. Single-stock skew can flatten or invert.
3. **Market-maker hedging** — when MM is short OTM puts (the customer-bought-protection side), MM is short downside-hedged. As underlying drops toward MM puts, MM dynamic-hedging amplifies the move (short-gamma exposure). This is **self-fulfilling** — the skew exists because MM hedging makes it real.
4. **Index correlation effect** — index skew steeper than component skew, because index vol = sum of component vols + correlation × cross-terms; correlation rises in crashes; the put skew is a *correlation hedge* in disguise.
5. **True non-normality** — actual returns are negatively skewed and fat-tailed (Sinclair Ch 3); skew prices reflect this empirical truth.

**Sinclair's framing**: skew is largely a *positioning shape*, not just a distributional reflection. The first three reasons above are positioning-driven; the fifth is reality-driven. **Skew shifts come from positioning shifts.**

### B. Sticky-Strike vs Sticky-Delta (Sinclair Ch 5)

When the underlying moves, the IV-strike-curve responds in one of two stylized ways:

| Rule | Behavior | Regime where it dominates | Trade implication |
|---|---|---|---|
| **Sticky strike** | The IV of a *given strike* stays fixed as underlying moves. Smile shape stays in absolute strike space. | **Range-bound markets** | Set up call-spread or put-spread arbitrages — Sinclair Ch 5 Eq 5.6-5.7 documents the arb |
| **Sticky delta** | The IV of a *given delta* stays fixed as underlying moves. Smile shape floats with the underlying. | **Trending markets** | Risk-reversal arbitrage — Sinclair Ch 5 documents the convexity arb |

Derman 1999 (cited by Sinclair): tested S&P 500 options. Range-bound regime → sticky-strike. Trending regime → sticky-delta. **Each heuristic works some of the time; neither is "true" universally.**

**Operational use as regime classifier**: regress recent IV moves vs underlying moves at fixed strike vs fixed delta; whichever rule has lower residual is the dominant rule for the current regime; the regime label feeds into strategy gating.

### C. Risk Reversal as Positioning Pulse

The 30-delta risk reversal (RR30) = IV(30Δ call) − IV(30Δ put). A canonical positioning indicator:

| RR30 sign | Market positioning |
|---|---|
| RR30 << 0 (puts much richer) | Heavy downside hedging; consensus bearish or insurance demand high |
| RR30 ≈ 0 | Neutral skew |
| RR30 > 0 (calls richer) | Bullish positioning; lottery-buying or upside-chase |

In **equity index** (per Sinclair) RR30 is structurally negative — puts always richer than calls because the put-skew sources (1, 3, 4 above) dominate.

In **crypto**, the sign **flips with regime**. Bear regimes (2018, 2022): RR30 << 0 (puts richer, classical equity-like). Bull mania (late 2017, mid-2021, 2025-Q4): RR30 > 0 (calls richer — lottery-buying dominates protective put demand).

**Skew-flip moments are regime-transition signals**:
- Bear-to-bull-rotation: RR30 climbs from very negative to near zero, then positive
- Bull-mania-to-correction: RR30 moves from positive back to negative
- Each transition leads price by some bars; IV positioning shifts before realized regime change

This is the operational handoff to [[trading/mechanisms/behavioral-positioning-unwind|behavioral-positioning-unwind]] — skew-flip detects the unwind preconditions.

### D. Skew + Variance Premium Decomposition (Sinclair Ch 11)

Per Sinclair, the variance premium is composed of:

- **ATM variance premium** — straight insurance / risk aversion compensation.
- **OTM put variance premium** — downside protection demand; biggest single component in equity indices; Kozhan et al. (2011) show ~half of OTM-put excess return comes from realized skew (correlation between returns and vol).
- **OTM call variance premium** — lottery-buyer premium (calls priced for upside that rarely materializes).

**Crypto-specific decomposition**: in equity, OTM-put premium dominates. In crypto bull mania, OTM-call premium can dominate (lottery-buyers chasing 10x). The dispersion of who-pays-whose-premium is regime-conditional.

**Trade implication**: in equity, "sell skew" usually means short OTM puts. In crypto, "sell skew" depends on the regime — short OTM calls in bull mania, short OTM puts in bear consolidation. The skew sign tells you which side to sell.

## Hypothesis (structured, per Part I §4)

```
Hypothesis A — Skew-Flip as Regime-Transition Signal

Hypothesis:    The 30-delta risk reversal (RR30 = IV(30Δ call) −
               IV(30Δ put)) on Deribit BTC options changes sign or
               moves significantly across zero in advance of major
               regime transitions (bear-to-bull rotation; bull-mania
               peak). The flip leads price by 5-20 days.

Mechanism:     Skew is positioning-driven (Sinclair Ch 5). When
               positioning rotates (e.g., institutions stop buying
               puts and start buying calls; or vice versa), skew
               flips before realized price action because IV
               quotations adjust faster than the underlying flow
               that's about to drive price. This is the
               IV-as-leading-indicator mechanism, but at the
               positioning-pulse level, not the volatility-level
               level.

Observable:    Daily RR30 on Deribit BTC 30d ATM options (or
               7d/60d alternate tenors). Track RR30 sign and
               magnitude; flag sign-change events; flag
               magnitude > 2-σ-from-rolling-90d-mean.

Expected:      Direction: skew-flip from negative to positive
                          → bullish regime imminent; from
                          positive to negative → bearish regime
                          imminent
               Magnitude: lead-time 5-20 days; price magnitude
                          regime-dependent
               Time horizon: 7-21 days for the regime change to
                             play out
               expected_net_edge:
                 - source:    Sinclair Ch 5 (skew dynamics);
                              equity literature (Driessen-Lin-
                              Van Hemert 2011 on skew-anchoring
                              effects); crypto literature thin
                 - value:     unknown; literature suggests ~30-
                              60bp on regime transitions if
                              detected accurately; absolute is
                              regime-dependent
                 - independence: equity literature, fully
                              independent of crypto deploy
                 - research-grade-only verdict default

Falsifier:     Over 20+ skew-flip events:
                 - Lead-time < 5 days median (signal lag too
                   short to be tradeable)
                 - Price direction over 7-21 days post-flip
                   has no positive correlation with flip
                   direction
                 - Sign-flips happen too often (>1 per month)
                   to be meaningful regime transitions
```

```
Hypothesis B — Sticky-Strike vs Sticky-Delta Regime Detection

Hypothesis:    Recent IV-vs-underlying behavior (sticky-strike
               vs sticky-delta dominance) classifies the trend
               regime. When sticky-strike dominates, the
               market is range-bound; when sticky-delta
               dominates, the market is trending.

Mechanism:     Per Derman 1999 / Sinclair Ch 5, the dominant
               rule reflects how option-market participants
               are quoting; that in turn reflects whether they
               see the underlying as range-bound (volatility
               unconditional on level → sticky-strike) or
               trending (volatility decomposes around path →
               sticky-delta).

Observable:    Daily Deribit BTC IV at multiple strikes; for
               each underlying-price-step, regress IV change
               vs strike change; the slope tells you which
               rule dominates.

Expected:      Use as regime classifier; gate other strategies
               by it (sticky-strike → mean-reversion strategies;
               sticky-delta → trend-continuation strategies).
               Direct edge: low (this is a gating attribute,
               not a strategy).
               expected_net_edge: as gating attribute, expected
                                  to add 20-40bp Sharpe to
                                  paired strategies; standalone
                                  edge zero.

Falsifier:     Either rule (sticky-strike or sticky-delta)
               dominates < 60% of the time → both equally
               weak → mechanism doesn't generalize. Or:
               regime label has zero correlation with
               trend/range realized state over n>50 days.
```

## Crypto-Specific Skew Considerations

| Element | Equity (Sinclair) | Crypto (Deribit) | Translation |
|---|---|---|---|
| Default skew sign | Put-rich (RR30 < 0) structural | Regime-conditional | **Inverse possible** |
| Skew sources | End-user puts + correlation effect + non-normality | Same + crypto-specific lottery buyers + halving-cycle dynamics | More variables |
| Skew anchoring (sticky strike) | Documented (Derman 1999) | Not yet directly tested | Open question |
| Skew flow | Quarterly options expiry concentrates flow | Monthly Deribit expiry concentrates more (smaller market) | Tighter expiry-clustering |
| Risk-reversal pricing depth | Multi-decade history | 5-7 years; Deribit listings 2018+ | Adequate for 2-year cone construction |
| Regime non-stationarity | Single major regime (US-equity bull bias) | Multiple regimes per cycle (bull/bear/recovery/peak) | Bigger overfit risk in skew-based-strategies |
| Skew as positioning indicator | Standard practice (e.g. SVXY traders track RR) | Standard practice in crypto-options (Greekslab, Amberdata) | **Direct apply** |

### Crypto regime case study — late-2025 cycle peak

Per [[foundation/macro/btc-regime-taxonomy-2020-2025]], 2025-Q4 was a cycle-peak rejection regime. Deribit RR30 in October-November 2025 ran near zero or slightly positive (lottery-buyers chasing $100K+ targets). By December 2025 (around the $24B options expiry — see [[trading/mechanisms/options-gamma-perp-effects]]), RR30 had flipped negative as protection-buyers replaced lottery-buyers. The skew flip preceded the late-2025 BTC peak rejection by 2-3 weeks; the sign-change was the regime-transition signal.

This is **the canonical worked example for Hypothesis A**. Pre-registration of the test framework is required before claiming this as evidence (per [[foundation/methodology/strategy-validation-discipline|LdP discipline]]); the post-hoc observation is illustrative, not validating.

## Use as Embedded Bias Filter

Per [[trading/mechanisms/regime-gate-vs-bias-filter|wiki discipline]]:

| Strategy family | Skew gating |
|---|---|
| Trend continuation | **Only fire when sticky-delta dominates** + RR30 sign aligns with trend (RR30 > 0 + uptrend; RR30 < 0 + downtrend) |
| Mean reversion | **Only fire when sticky-strike dominates** + RR30 magnitude > 1 (positioning extreme = unwind candidate) |
| Premium capture (short OTM call) | **Only fire when RR30 > 0** (calls expensive, lottery-buyer premium worth fading) |
| Premium capture (short OTM put) | **Only fire when RR30 < 0** (puts expensive, protection premium worth fading) |
| Behavioral-positioning-unwind | **Native dependency**: skew-flip IS one of the load-bearing positioning-shift signals |

## Adjacent Mechanisms — Distinction

### vs. [[trading/mechanisms/vol-regime-detection|vol-regime-detection]]

vol-regime-detection covers the *level* of vol (where current RV/IV sits in the percentile distribution). skew-based-positioning-signals covers the *shape* of the IV curve across strikes. Both are independent regime axes.

### vs. [[trading/mechanisms/term-structure-dynamics|term-structure-dynamics]]

Term structure is the *expiry* dimension; skew is the *strike* dimension. Together they characterize the full IV surface. A complete vol-aware strategy gates on both — different IV-shape regimes call for different strategies.

### vs. [[trading/mechanisms/behavioral-positioning-unwind|behavioral-positioning-unwind]]

Behavioral-positioning-unwind operates at the *positioning-extreme + unwind* level (general). Skew-flip detection is the *options-positioning-pulse* signal that feeds into behavioral-positioning-unwind. Skew-flip happens at the positioning-rotation moment; the unwind plays out over the following bars.

### vs. [[trading/mechanisms/options-gamma-perp-effects|options-gamma-perp-effects]]

Options-gamma-perp covers dealer-side flow at expiry. Skew is the *upstream* positioning shape that conditions where dealer flow concentrates. Heavy skew → dealers heavily exposed in one direction → expiry effects amplified.

### vs. [[foundation/macro/btc-regime-taxonomy-2020-2025|btc-regime-taxonomy]]

The regime taxonomy classifies *price-side regime states* (bull cycle, bear, peak, etc.). Skew patterns are *options-side regime indicators* that lead or coincide with price-side states. Skew-flip + price-regime-transition pairs are the high-conviction confluence cases.

### vs. [[trading/mechanisms/variance-risk-premium-crypto|variance-risk-premium-crypto]] (NEW 2026-05-18)

Skew decomposes the variance premium across strikes — that's the structural integration with VRP harvest. The VRP harvest mechanism (delta-hedged short-vol) chooses its variant (short straddle vs short OTM put vs short OTM call) based on the skew shape: when OTM-put premium dominates (protection-fear regime), short OTM puts captures the skewness-component; when OTM-call premium dominates (bull-mania lottery-buying regime), the skew-decomposition flip signals tail risk extreme — pause short-vol per VRP page's falsifier (c). Skew is the **variant-selection signal** for VRP harvest at the strike level; vol-regime-detection is the **harvest-timing signal** at the level dimension.

## Operationalization Status

| Element | Wiki encoded | DSL primitive | Backtested |
|---|:---:|:---:|:---:|
| Mechanism articulation (this page) | ✅ | n/a | n/a |
| RR30 computation | ✅ | ❌ — `risk_reversal(delta, tenor)` | ❌ |
| Skew slope computation | ✅ | ❌ — `iv_slope(tenor, strike_range)` | ❌ |
| Sticky-strike vs sticky-delta classification | ✅ | ❌ — `sticky_regime(lookback)` | ❌ |
| Skew-flip event detection | ✅ | ❌ — `rr30_sign_change()` | ❌ |
| Implied-skewness from Corrado-Su | ✅ Eq 5.10-5.21 | ❌ — major DSL/math gap | ❌ |
| Deribit per-strike IV feed | ❌ | ❌ — engineering project | ❌ |

**Net**: wiki-encoded as mechanism family; **all dependent on Deribit per-strike IV-feed ingest**. Same blocking constraint as term-structure-dynamics — engineering work owner-flagged.

## Capability Gap (DSL + Data)

For the backtesting lab integration:

1. **Deribit per-strike IV chain** — for each tenor, full strike-level IV; minimum 1y depth.
2. **`risk_reversal(delta, tenor)`** — IV(call_at_delta) − IV(put_at_delta).
3. **`iv_slope(tenor, strike_range)`** — linear regression slope of IV vs log-moneyness.
4. **`sticky_regime(lookback)`** — return enum{strike, delta, neutral} based on recent IV-vs-underlying response.
5. **`butterfly(delta, tenor)`** — IV(call_at_delta) + IV(put_at_delta) − 2×IV(ATM); kurtosis indicator.
6. **Multi-leg options DSL** — risk-reversal, butterfly, condor structures for execution if deployed on options-side directly.

## Honest Caveats

- **Crypto skew sign is regime-conditional**; any strategy assuming structural put-skew (equity-style) will misfire in bull-mania regimes. Don't port equity-skew strategies naively.
- **Skew-flip detection is signal-extraction, not strategy.** Knowing skew flipped tells you something happened; the trade requires pairing with mechanism (e.g., behavioral-positioning-unwind, regime-rotation entry).
- **Per-strike IV depth is thin in crypto altcoins**; only BTC and ETH have reliable enough strike chains to compute meaningful skew. Don't try this on smaller alts.
- **Sticky-strike vs sticky-delta classification is regime-fragile** — Derman 1999 documented for SPX; transfer to crypto is hypothesis-only.
- **Implied-skewness via Corrado-Su (Sinclair Ch 5) requires careful numerical implementation** and runs into the Gram-Charlier-positivity boundary (Sinclair Fig 5.16); any direct skew-modeling risks negative-probability artifacts. Use simpler RR30/butterfly metrics first.
- **Capacity is small for OTM-options-direct strategies**; if executed via options, market-impact at scale is a binding constraint. Perp-side trades informed by options-state (the cleaner architecture per [[trading/mechanisms/options-gamma-perp-effects]]) avoid this.

## Key Takeaways

- **Skew is a positioning shape**, not a distribution reflection alone.
- **RR30 sign and magnitude track positioning** — flips lead regime transitions in crypto (5-20 days lead-time hypothesis).
- **Sticky-strike vs sticky-delta classification** distinguishes range-bound from trending regimes.
- **Crypto skew sign is regime-conditional** — inverse to equity in bull mania.
- **Use as embedded bias filter** for vol-sensitive strategies (per [[trading/mechanisms/regime-gate-vs-bias-filter|wiki discipline]]).
- **Capability-gated**: requires Deribit per-strike IV-feed; engineering pre-requisite.
- **Diversification value high** — orthogonal to HTF-EMA-bias scaffold cluster; complements term-structure and vol-regime axes.

## Related

- [[foundation/sources/sinclair-volatility-trading]] — primary source (Ch 5 especially)
- [[trading/mechanisms/vol-regime-detection]] — companion vol-mechanism page (this ingest); level dimension
- [[trading/mechanisms/term-structure-dynamics]] — companion vol-mechanism page (this ingest); expiry dimension
- [[trading/mechanisms/options-gamma-perp-effects]] — adjacent; downstream of skew positioning shape
- [[trading/mechanisms/behavioral-positioning-unwind]] — sibling; skew-flip detection feeds the unwind signal
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — embedding discipline
- [[trading/mechanisms/scaffold-as-mechanism]] — diversification gap context
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — when this mechanism is most/least useful (regime-conditional)
- [[foundation/methodology/strategy-validation-discipline]] — pre-registration discipline for skew-strategy testing
- [[foundation/methodology/deployment-edge-threshold]] — net-edge thresholds and §4.5 independence
- [[foundation/methodology/forecast-vs-tradeable-gap]] — L1 skew-detection → L2 strategy translation gates
- [[glossary/vol-skew]] — defined term
- [[glossary/variance-premium]] — defined term
- Part I §4 — structured-hypothesis format compliance

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 5 (smile dynamics, sticky-strike vs sticky-delta, Corrado-Su skew/kurt expansion), Ch 11 (skewness premium decomposition)
- Derman, E. (1999) — sticky-strike vs sticky-delta empirical testing on S&P 500 options (cited by Sinclair Ch 5)
- Driessen, J., Lin, T., Van Hemert, O. (2011) — skew anchoring at 52-week highs/lows
- Corrado, C., Su, T. (1996) — Gram-Charlier expansion for skew/kurt-extended BSM (Sinclair Ch 5 Eq 5.14)
- Kozhan, R., Neuberger, A., Schneider, P. (2011) — skew-swap profitability decomposition
- 2026-05-08 ingest task — wiki:2026-05-06-ingest-sinclair-volatility-trading
