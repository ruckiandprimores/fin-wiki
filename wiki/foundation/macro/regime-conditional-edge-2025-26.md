---
title: "Regime-Conditional Edge — Cross-Strategy Empirical Evidence (2025-2026)"
status: growing
created: 2026-05-05
updated: 2026-05-05
tags: [foundation, macro, regime-conditional, cross-strategy-empirical, w2-finding, deployment-discipline, day-swing]
---

# Regime-Conditional Edge — Cross-Strategy Empirical Evidence (2025-2026)

> **TL;DR**: Across 13 strategies × ~110K grid runs on BTCUSDT perp 4h 2025-05-03 → 2026-04-28, **edge is concentrated in 2025-Q4 + 2026-Q1** (the bearish-dominant sub-regimes per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC regime taxonomy]]). 13/13 strategies positive in Q4 2025 (avg +$95 Q5 P&L); 10/13 in Q1 2026 (avg +$48); near-zero average in Q2/Q3 2025 and Q2 2026. **The 1y "validated" edge is actually a 5-6 month edge averaged over 12 months.** Strategies do not have year-round edge; they have **regime-conditional edge concentrated in bearish sub-regimes**. This is the empirical case for regime-aware deployment as a pre-condition for any productionize candidate.

## Headline Finding (W2 Source Data)

Cross-strategy pattern mining of the backtesting lab outputs:

| Sub-period | Strategies positive | Avg Q5 P&L gain | Wiki regime label (per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC taxonomy]]) |
|------------|:---:|---:|---|
| 2025-Q2 | 4/8 | −$2.4 | mild uptrend / chop |
| 2025-Q3 | 3/9 | +$1.7 | mild uptrend / pre-cycle-peak |
| **2025-Q4** | **13/13** | **+$95.4** | **cycle peak rejection / bearish** |
| 2026-Q1 | 10/13 | +$47.7 | continuation downtrend |
| 2026-Q2 | 3/12 | −$16.1 | early data / transitional |

**Reading**: the year-over-year backtest-positive verdict masks two distinct regimes:
- **5-6 month "edge window"** (Q4 2025 + Q1 2026) where strategies fire profitably with high cross-strategy concordance
- **6-7 month "noise window"** (Q2/Q3 2025 + Q2 2026) where strategies are net flat / negative across most of the cohort

The 1y average comes out positive *because* of the Q4-Q1 concentration. Without Q4-Q1, all 13 strategies are net negative.

## Why This Matters

### Implication 1: Live regime classification is necessary for deployment

Without knowing which regime is currently active, deployment risks firing strategies in non-edge windows (Q2/Q3 2025 + Q2 2026 analogs) when the *median* outcome is loss-making. A regime classifier per the research process's W3 spec is the load-bearing missing tooling.

### Implication 2: Direction-asymmetric mutation discipline inherits regime conditionality

Per Part I §9 sharpening 2026-05-05, direction restriction (drop entry_long when long_pnl negative + short_pnl positive) was promoted to explicit mutation operator with N=3 supporting cases. **The asymmetry is a regime feature** — same mutation in a bullish-dominant year would call for long-only restriction. The regime-conditioning observed here is the *upstream cause* of the direction-asymmetry signal:

- 2025-26 was bearish-dominant → short-side captures regime tailwind → long-side fights it
- A bullish-dominant year would invert the asymmetry
- Regime classification determines which restriction direction applies, not the mechanism itself

### Implication 3: Even "deployment-grade" strategies need regime context

Per [[foundation/methodology/deployment-edge-threshold|deployment-edge threshold]] methodology: a strategy with concentrated 5-6 month edge × full-year friction has lower net-of-friction edge than the gross numbers suggest. Strategies that look "borderline" on full-year grid (e.g. `bollinger_squeeze_4h_short_only` at Net +$8/yr) become *clearly* not-deployment-grade once you account for the fact that all $8 of edge is concentrated in 5-6 months while friction accrues all 12. This compounds the deployment gap identified in [[trading/backtest-results/direction-restriction-short-only-2026-05]].

### Implication 4: Portfolio diversification doesn't save the deployment math

Strategies in the 13-strategy cohort are largely independent at the *trade-by-trade* level (median pairwise P&L correlation +0.01) **but they share regime concentration** — all 13 strategies' edge windows overlap in Q4 2025 + Q1 2026. Combining them in a portfolio doesn't give you 13 independent edge sources; it gives you 13 versions of the same regime-window edge. **Independence at the trade level is not independence at the regime level.** This is the higher-order correlation that grid-level analysis misses.

## Worked-Evidence Cross-References

### `ema_bias_breakout_4h_v2` Q-by-Q P&L attribution

Per [[trading/rejected/ema-bias-breakout-4h-v2-2026-05|the v2 entry]]: 73% of annual P&L from Q4 2025 + Q1 2026. The remaining 27% spread across Q2 2025, Q3 2025, Q2 2026. This is the canonical worked example — a strategy that passed pre-registered statistical thresholds but whose edge is regime-concentrated.

### Direction-restriction mutations

Per [[trading/backtest-results/direction-restriction-short-only-2026-05]]: 2-of-2 short_only mutations show regime-asymmetric P&L distributions. Both mutations win because the regime favors short side; both would *fail* (or invert) in a regime favoring long side.

### Counter-bias mirror finding

`ema_bias_breakout_4h_counter` (the counter-bias mirror per [[foundation/market-wisdom/bias-filter-vs-entry-signal|bias-filter-vs-entry-signal]]) shows positive Q2 2025 P&L (+$50) at the same time the parent (`ema_bias_breakout_4h`) is losing money. **The counter has its own regime** — it captures pullback opportunities the trend-aligned parent misses during the chop / pre-cycle-peak windows. This is consistent with the regime-conditioning hypothesis: different strategies match different regimes, and the regime determines which strategy "works" rather than the strategy determining whether it works.

## Cross-Strategy Concordance Diagnostic

A diagnostic worth running on any new strategy ingest:

| Metric | Question | Threshold |
|--------|---------|----------|
| **Q-by-Q P&L concentration** | What fraction of annual P&L comes from the top-2 quarters? | <60% = regime-agnostic; >75% = regime-concentrated |
| **Cross-cohort edge-window overlap** | Does this strategy's positive months overlap with the 13-strategy cohort's positive months? | High overlap = regime-shared edge; low overlap = idiosyncratic edge |
| **Negative-months distribution** | Are the negative months random, or clustered in Q2/Q3 2025 + Q2 2026? | Clustered = regime-conditional; random = mechanism-broken |

A strategy with regime-concentrated edge that overlaps the 13-strategy edge window is **expected** to be a sample-of-the-regime, not an independent edge source. Pricing that into deployment expectations is the discipline.

## Caveats

- **Single 1y window.** This empirical observation comes from one window (2025-05 → 2026-04). The pattern may not generalize: the 2026-Q1 → Q2 transition is the regime turn; if Q3 2026 reverts to a similar pattern as Q4 2025, the regime-conditioning hypothesis strengthens; if not, the finding may be window-specific.
- **Out-of-window validation absent.** Per [[foundation/methodology/strategy-validation-discipline]] §13 crypto-specific concerns, regime non-stationarity is real. The 13/13 Q4 2025 result is *in-sample* for the regime taxonomy that *post-hoc* labeled Q4 2025 as "cycle peak rejection."
- **Single-instrument (BTCUSDT only).** The 13 strategies are all BTCUSDT perp; cross-asset diversification would *partially* mitigate the regime concentration if other instruments' regimes desynchronized. Multi-instrument testing is a known Section 8 capability gap.
- **Regime labels are post-hoc.** "2025-Q4 = cycle peak rejection" is a label assigned with hindsight. Live regime classification (W3 spec) requires the labels to be assigned *contemporaneously* — that's a different problem and the reason regime-classifier engineering work is on the action-item list.
- **The 13-strategy cohort itself may be biased.** Strategies were selected for backtesting based on author judgment; mechanism families included may have over-represented those that work in bearish regimes. A truly random sample of mechanism families might show different regime concentration. Selection bias at the *cohort* level is plausible.

## Operationalization Status

| Element | Wiki encoded | Pipeline implemented |
|---------|:---:|:---:|
| Per-strategy Q-by-Q P&L attribution | ✅ this page; per-entry tables | ⚠️ partial — backtest engine outputs Q-segmented metrics but not standardized |
| Cross-strategy regime concentration diagnostic | ✅ this page (manual procedure) | ❌ not automated |
| Live regime classification | ❌ no implementation | ❌ — W3 spec exists at research process level; engineering work owner-flagged |
| Regime-conditioned position-sizing | ❌ no implementation | ❌ — would scale exposure based on regime confidence |
| Cross-asset regime diversification | ❌ single-instrument only | ❌ — multi-instrument is broader Section 8 capability gap |

**Net**: the empirical observation is wiki-encoded and reproducible from the backtesting lab outputs. The *deployment discipline* it implies (live regime classification, regime-conditioned sizing) is engineering work owner-flagged per Apr 28 division of labor.

## Companion Disciplines (Cross-Links)

This finding sits between three existing disciplines:

| Discipline | Page | Relationship |
|-----------|------|--------------|
| **Regime taxonomy** | [[foundation/macro/btc-regime-taxonomy-2020-2025]] | The framework this finding operationalizes — Q-by-Q labels here map to taxonomy regimes |
| **Statistical validation** | [[foundation/methodology/strategy-validation-discipline]] §7.2 | "Regime-conditioning as anti-overfitting recommendation" is the upstream discipline; this finding is the downstream empirical case |
| **Economic validation** | [[foundation/methodology/deployment-edge-threshold]] | Concentrated 5-6 month edge × full-year friction is the deployment-grade penalty this finding implies |

Together: regime taxonomy describes WHEN markets behave differently; this page documents that strategies *empirically* have regime-conditional edge; validation-discipline says regime-conditioning should be designed in upstream; deployment-edge says the regime concentration compounds the deployment gap.

## Operational Recommendations

For any future strategy:

1. **Pre-register regime conditions** — at hypothesis stage (per Part I §4), state which regime(s) the mechanism is expected to work in. "Works year-round" is suspicious by default given this evidence.
2. **Q-by-Q reporting** — every backtest result page should include Q-by-Q P&L attribution, not just annual statistics. Per Part I §9 direction-restriction discipline: "always split long_pnl vs short_pnl in cross-strategy analysis"; analogous discipline applies to time periods.
3. **Cross-cohort concordance check** — for any strategy joining a portfolio, check whether its edge window overlaps the cohort's edge window. High overlap = treat as sample-of-regime, not independent edge.
4. **Capacity-aware deployment** — concentrate position sizing in regime-confirmed windows; reduce or pause during regime-uncertain windows. Requires live regime classification (W3) before this is implementable.
5. **Mechanism families to test cross-regime** — explicitly seek mechanisms expected to work in bullish-dominant or chop regimes to balance the cohort's bearish concentration.

## Key Takeaways

- **13/13 positive in Q4 2025; 10/13 in Q1 2026; noise elsewhere** — empirical concentration of edge in bearish sub-regimes
- **The 1y "validated" edge is 5-6 months × full-year friction** — deployment-grade math is worse than gross numbers suggest
- **Independence at the trade level ≠ independence at the regime level** — 13 strategies in a portfolio is 13 versions of the same regime-window edge
- **Regime-asymmetric mutation discipline () inherits this** — direction-restriction's 2-of-2 supporting cases ARE the regime conditionality, not separate evidence
- **Live regime classification (W3) is the load-bearing missing tooling** — without it, deployment risks firing strategies in non-edge windows
- **Selection bias at cohort level plausible** — mechanism families chosen may over-represent bearish-favorable types

## Related

- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — the regime framework this finding operationalizes
- [[foundation/methodology/strategy-validation-discipline]] §7.2 — regime-conditioning as anti-overfitting recommendation; upstream discipline
- [[foundation/methodology/deployment-edge-threshold]] — deployment-grade requires regime awareness; concentrated edge compounds the deployment gap
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — canonical worked example (73% of P&L from Q4 2025 + Q1 2026)
- [[trading/backtest-results/direction-restriction-short-only-2026-05]] — direction-asymmetry as regime feature
- [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] — separate-regime-gate rejection (regime classification *implementation* is what's hard, not *concept*)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; embedded bias filter > separate regime gate
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — counter-bias mirror as regime-asymmetry diagnostic
- Part I §9 — Mutation Operators (direction-restriction discipline)
- Part I §4 — Structured Hypothesis Format (pre-register regime conditions)

## Sources

- the backtesting lab — full W2 analysis (primary source)
- 13 strategies' `grid_results_1y/` outputs in the backtesting lab
- Q-by-Q decomposition methodology: per-strategy summary.json + grid metadata
