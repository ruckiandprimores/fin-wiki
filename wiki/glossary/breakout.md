---
title: "Breakout"
status: evergreen
created: 2026-04-29
updated: 2026-04-29
tags: [glossary, technical-analysis, murphy, terminology]
---

# Breakout

The event when price exits a defined range — either a horizontal trading range, a chart pattern boundary, or a trendline.

## Confirmation criteria

A breakout is *meaningful* (vs. a false breakout) when:

1. **Price clearly exits the range** — typically by ≥1-3% beyond the boundary, depending on the asset's volatility
2. **The breakout closes** beyond the boundary on the relevant timeframe (intraday wicks don't count for daily-timeframe analysis)
3. **Volume expands** in the direction of the breakout (per [[trading/mechanisms/volume-analysis|volume confirmation]])
4. **Other technical indicators confirm** (oscillators not divergent against the breakout direction)

## Types

| Type | What's broken |
|------|---------------|
| **Horizontal breakout** | Price exits a [[trading/mechanisms/support-resistance|support or resistance]] level |
| **Pattern breakout** | Price exits a [[trading/mechanisms/chart-patterns|chart pattern]] (triangle, rectangle, flag) boundary |
| **Trendline breakout** | Price crosses a drawn trendline |
| **MA breakout** | Price crosses a [[trading/mechanisms/moving-averages|moving average]] |

## False breakouts

When price briefly exits the range and then reverts back inside. Common in:
- Low-volume markets
- Markets near major levels with concentrated stops (stop-hunt scenarios — Lefèvre's "stop hunting" applies)
- Markets without confirming volume

The mechanism: *intentional* stop-hunts (or unintentional ones via thin liquidity) push price through a level to trigger stops, then revert. Common in crypto perps, where liquidation clusters at known levels create magnetism.

## Retesting

After a confirmed breakout, price often retraces to test the broken level (now reversed in role: former resistance becomes support, former support becomes resistance). The retest is one of the cleanest entry setups available — the breakout is confirmed, the level is retested, the trade has clear stop placement.

## Breakout target

For pattern-based breakouts, the price target is typically the *measured move* — the height of the pattern projected from the breakout point. See [[trading/mechanisms/chart-patterns]].

## Crypto-specific breakout dynamics

- **Liquidation cascades** can produce sharp wicks that look like breakouts but immediately reverse
- **Stablecoin supply growth** before breakouts is a leading indicator (dry powder building)
- **Funding-rate flips** at breakouts often confirm or deny the move
- **On-chain whale accumulation** before breakouts increases breakout reliability
- **Pivotal-point breakouts** (round numbers, prior-cycle highs/lows) have additional structural confluence — see [[trading/mechanisms/pivotal-points]]

For the central catalog of crypto-native carrier features that make breakouts (and other classical mechanisms) tradable in crypto, see [[foundation/market-structure/crypto-carrier-features]].

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of carriers (liquidation maps, funding, on-chain flow, pivotal-point confluence factors) that operationalize crypto-specific breakout dynamics
- [[foundation/market-wisdom/bias-filter-vs-entry-signal]] — breakouts are *hybrid signals* (direction + timing in one event); knowing this is the discipline that separates working breakout strategies from regime-blind ones
- [[trading/mechanisms/support-resistance]] — what's being broken
- [[trading/mechanisms/pivotal-points]] — round-number / prior-range breakouts with structured hypothesis
- [[trading/mechanisms/chart-patterns]] — patterns whose completion is a breakout
- [[trading/mechanisms/volume-analysis]] — confirmation requirement
- [[trading/mechanisms/moving-averages]] — MA crossovers are a form of breakout

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets*, Chs 4–6, 1999.
