---
title: "Vol Skew (Volatility Smile)"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, volatility, skew, smile, sinclair, terminology]
---

# Vol Skew (Volatility Smile)

The **shape of implied volatility across strikes** at fixed expiry. In equities the smile is structurally **put-skewed** (OTM puts have higher IV than OTM calls); in crypto the skew sign is **regime-conditional** — puts richer in bear, calls richer in bull mania.

## The smile shape

Plot IV on the y-axis vs strike (or moneyness, or delta) on the x-axis at fixed expiry. The shape has standard descriptive forms:

- **Monotone put-skew** — IV decreases monotonically with strike; OTM puts most expensive. Typical for equity indices.
- **Smile** — IV is U-shaped, lowest near ATM, higher both OTM-call and OTM-put sides. Typical for individual equities, FX, crypto.
- **Reverse skew (smirk)** — IV increases monotonically with strike; OTM calls most expensive. Crypto bull-mania regime.

## Why the skew exists (Sinclair Ch 5)

1. **End-user demand asymmetry** — equity longs naturally buy puts (downside hedge) and sell covered calls; flow-driven put-skew.
2. **Takeover hedge** — single-name calls bid up by takeover-jump expectation.
3. **Market-maker dynamic hedging** — when MM short OTM puts, hedging amplifies downward moves; self-fulfilling skew.
4. **Index correlation effect** — index OTM puts price the correlation tail (correlation rises in crashes); makes index skew steeper than component skew.
5. **True non-normality** — actual returns are negatively skewed and fat-tailed (Sinclair Ch 3); the smile reflects this.

## Operational measures

- **Risk reversal (RR)**: IV(call_at_delta) − IV(put_at_delta). Most common at 25Δ or 30Δ. Sign indicates positioning.
- **Butterfly (BF)**: IV(call_at_delta) + IV(put_at_delta) − 2 × IV(ATM). Convexity / kurtosis indicator.
- **Skew slope**: regression slope of IV vs log-moneyness across strike chain.

## Sticky-strike vs sticky-delta (Derman 1999 / Sinclair Ch 5)

When the underlying moves, the smile responds in one of two stylized ways:

| Rule | Behavior | Regime where it dominates |
|---|---|---|
| **Sticky strike** | IV at fixed strike stays constant; smile stays in absolute strike space | Range-bound markets |
| **Sticky delta** | IV at fixed delta stays constant; smile floats with underlying | Trending markets |

Each rule admits its own arbitrage trade (call-spread under sticky-strike; risk-reversal under sticky-delta). Useful as **regime classifier** by which rule dominates.

## Crypto-specific finding: skew sign is regime-conditional

| Regime | Typical RR30 sign |
|---|---|
| Bear market (e.g., 2018, 2022) | Strongly negative (puts richer — protection demand dominates) |
| Recovery / accumulation | Mildly negative or near-zero |
| Bull trending | Near-zero or mildly positive |
| Bull mania (e.g., late 2017, mid-2021, 2025-Q4 peak) | Positive (calls richer — lottery-buyers dominate) |

**Skew-flip across zero is itself a regime-transition indicator** (see [[trading/mechanisms/skew-based-positioning-signals]]). This contrasts with equity where put-skew is structurally negative regardless of regime.

## Implied skewness via Corrado-Su (Sinclair Ch 5)

Beyond simple risk-reversal pricing, Corrado-Su (1996) extends BSM with skew/kurt parameters:

```
C = C_BSM + μ3 × Q3 + (μ4 − 3) × Q4
```

where μ3 = implied skewness, μ4 = implied kurtosis, Q3 and Q4 are Gram-Charlier expansion adjustment terms. **Caveat**: the Gram-Charlier expansion is a probability density only inside a bounded region of (μ3, μ4) space (Sinclair Fig 5.16 — Jondeau-Rockinger boundary). Direct skew-modeling can produce negative-probability artifacts; use simpler RR/butterfly metrics first.

## Crypto data sources

- **Deribit per-strike IV chain** — primary source for BTC/ETH skew measurement
- **DVOL** — Deribit's VIX-equivalent (ATM-based; doesn't capture skew)
- **BVIV / EVIV (Volmex)** — IV indices with skew components
- **Greeks.live, Amberdata, Glassnode** — pre-computed RR / butterfly metrics

## Related

- [[trading/mechanisms/skew-based-positioning-signals]] — operational mechanism page using skew
- [[trading/mechanisms/vol-regime-detection]] — companion vol-mechanism
- [[trading/mechanisms/term-structure-dynamics]] — companion vol-mechanism
- [[foundation/sources/sinclair-volatility-trading]] — primary source (Ch 5)
- [[glossary/variance-premium]] — companion glossary
- [[glossary/iv-term-structure]] — companion glossary

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 5 (smile dynamics, sticky-strike vs sticky-delta, Corrado-Su)
- Derman, E. (1999). *Regimes of volatility*. Risk.
- Corrado, C., Su, T. (1996). *Skewness and kurtosis in S&P 500 index returns implied by option prices*.
- Kozhan, R., Neuberger, A., Schneider, P. (2011). *The skew risk premium in the equity index market*.
- Jondeau, E., Rockinger, M. (1999/2001). *Gram-Charlier expansion validity boundaries*.
