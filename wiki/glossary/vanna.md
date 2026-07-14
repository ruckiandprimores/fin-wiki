---
title: "Vanna"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, options, greeks, sinclair, terminology]
---

# Vanna

Second-order options greek: **the rate of change of delta with respect to implied volatility**, equivalently the rate of change of vega with respect to underlying:

```
Vanna = ∂²C / ∂S∂σ = ∂Δ/∂σ = ∂Vega/∂S
```

Where C = option price, S = underlying, σ = implied volatility.

## Operational meaning

Vanna captures how delta-hedging behavior shifts as IV moves. A position with significant vanna means:
- A vol-up move *also* changes delta (forces re-hedging even if underlying doesn't move)
- An underlying move *also* changes vega (interacts with vol-positioning)

For options dealers running large books, vanna creates a flow channel: when IV moves, dealers must rebalance underlying position even if spot is steady. Aggregated, vanna-driven flow can amplify or attenuate price moves.

## Sign and interpretation

- Vanna > 0 (typical for OTM calls): vol-up makes delta more positive; dealer-short-this-call must buy more underlying as vol rises
- Vanna < 0 (typical for OTM puts): vol-up makes delta more negative; dealer-short-this-put must sell more underlying as vol rises

## Why it matters for crypto

The "**vanna flow**" concept is increasingly cited in crypto-options analytics (Greeks.live, Amberdata) as a complement to GEX. When implied vol shifts (e.g., around scheduled events), dealer-aggregate vanna determines residual flow even when spot is range-bound.

## In Sinclair (Ch 1, Ch 6)

Sinclair derives vanna implicitly in his hedging discussion (Ch 6) — when a hedger's delta needs to track shifts in IV (sticky-delta vs sticky-strike interaction with vol moves), the cross-term that drives the re-hedging is vanna. He doesn't use the name "vanna" extensively; the term is more common in market-maker practice than in his presentation.

## Related

- [[glossary/gamma-exposure-gex]] — companion higher-order greek
- [[glossary/charm]] — time-decay analog
- [[trading/mechanisms/options-gamma-perp-effects]] — adjacent operational mechanism
- [[foundation/sources/sinclair-volatility-trading]] — primary source

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 1, Ch 6 (hedging mechanics where vanna appears)
- Industry usage: market-maker risk-management literature; crypto-options analytics providers
