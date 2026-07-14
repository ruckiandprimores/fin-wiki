---
title: "Charm"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, options, greeks, sinclair, terminology]
---

# Charm

Second-order options greek: **the rate of change of delta with respect to time** (calendar time):

```
Charm = ∂Δ / ∂t = ∂²C / ∂S∂t
```

Where C = option price, S = underlying, t = time.

## Operational meaning

Charm describes how delta drifts as time passes, holding underlying constant. Even a static spot generates re-hedging requirements over time because option deltas drift toward {0, ±1} as expiry approaches.

For options dealers approaching a major expiry (especially monthly or quarterly), charm-driven flow becomes substantial:
- ITM options' deltas → 1 or −1 (charm pushes deltas to expiry value)
- OTM options' deltas → 0

Cumulatively, charm forces predictable end-of-day re-hedging in the days leading up to expiry. Visible as **time-of-day or day-of-week flow** patterns near expiry.

## Sign

- Charm < 0 for OTM options approaching expiry (delta decays toward 0)
- Charm > 0 for ITM options approaching expiry (delta moves toward ±1)
- Charm crosses zero at the inflection point in the option's lifecycle

## Why it matters for crypto

Charm-driven flow is part of the **expiry-pinning** mechanism (see [[trading/mechanisms/options-gamma-perp-effects]]). The pre-expiry pinning is gamma-driven; the *intraday timing* of the pinning flow is partly charm-driven (dealers re-hedge predictably as time passes).

In Deribit BTC options:
- Last 5-7 days before monthly expiry: charm-driven hedging accelerates
- Last few hours: charm becomes the dominant residual flow as gamma decays
- 8:00 UTC settlement: residual delta exposure flips to spot

## In Sinclair (Ch 1, Ch 6)

Sinclair derives charm implicitly in his hedging-cost discussion (Ch 6 — costs of regular re-hedging). Like vanna, he doesn't use the name extensively; the term is more standard in market-maker practice.

## Related

- [[glossary/gamma-exposure-gex]] — companion higher-order greek
- [[glossary/vanna]] — vol-cross-delta analog
- [[trading/mechanisms/options-gamma-perp-effects]] — adjacent operational mechanism
- [[foundation/sources/sinclair-volatility-trading]] — primary source

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 1, Ch 6 (hedging mechanics where charm appears)
- Industry usage: market-maker risk-management literature; crypto-options analytics providers
