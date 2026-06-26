---
title: "Gamma Exposure (GEX)"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, options, gamma, dealer-flow, sinclair, terminology]
---

# Gamma Exposure (GEX)

The **estimated aggregate gamma position of options dealers** at a given underlying price. Computed from open-interest distribution + per-strike IV + Black-Scholes greeks. Dealer GEX shape determines the **direction and magnitude of obligated dealer-hedging flow**, which in turn drives short-horizon mean-reversion or trend-amplification in the underlying.

## Definition

For a single option, gamma `Γ` is the second derivative of option price with respect to underlying:

```
Γ = ∂²C / ∂S²
```

Aggregate dealer GEX at price S₀:

```
GEX(S₀) = Σ over all listed options [ Γ_i(S₀) × OI_i × dealer_position_sign_i × contract_multiplier ]
```

Result is in units of "$ change in dealer delta per 1% change in underlying."

## Sign convention (industry standard)

- **Positive GEX**: dealers are net long gamma. They sell into rallies / buy dips to remain delta-hedged → **mean-reverting flow** in underlying.
- **Negative GEX**: dealers are net short gamma. They buy rallies / sell dips → **trend-amplifying flow**, can cause cascade.

## Operational uses

1. **Pinning detection** (Sinclair Ch 5; concrete crypto evidence in [[trading/mechanisms/options-gamma-perp-effects]]):
   - High positive GEX concentrated near a strike → underlying tends to oscillate around that strike, suppressed range
2. **Cascade prediction**:
   - Negative GEX above a strike → if underlying falls past it, dealers must sell-on-the-way-down → cascade amplification
3. **Expiry roll-off**:
   - Large positive GEX near expiry → at expiry the GEX evaporates → suppressed-vol regime ends → free move follows
4. **Implied directional bias**:
   - GEX skew (more positive on one side) tells you where dealers are pinning to and where they're vulnerable

## Per-strike "gamma profile"

Plot per-strike GEX contribution along a price grid; the resulting "gamma profile" reveals:
- **Zero-gamma flip point** — price at which dealer position transitions from net positive to net negative gamma
- **Gamma walls** — strikes where GEX concentration is high; underlying tends to pin there (positive GEX) or break decisively past them (negative GEX)
- **Asymmetry** — bullish vs bearish GEX imbalance shows where dealers will hedge harder

## Crypto-specific computation

- **Source**: Deribit per-strike OI + per-strike IV + spot BTC/ETH price
- **Aggregation**: ideally Deribit + CME + OKX combined, but Deribit alone is >80% of BTC options OI
- **Public providers**: Glassnode "Taker-Flow-Based Gamma Exposure", MenthorQ, Greeks.live
- **Primary signal**: large positive GEX during pre-expiry windows is the dominant pinning signature; see [[trading/mechanisms/options-gamma-perp-effects]] for Dec 2025 worked example

## Sinclair's framing

Sinclair (Ch 5) doesn't use the term "GEX" specifically (it's a more recent industry term), but **Equation 1.7** establishes the direct link between underlying-move-squared and option P&L; and Ch 5 documents the dealer-side hedging mechanics that give rise to what we now call GEX. The term originated post-2015 in equity options literature; crypto adoption ~2022.

## Honest caveats

- **Dealer-position sign assumption**: industry-standard GEX assumes dealers are short the OTM puts (customer protection-buying) and long short-dated calls — but this is a stylized assumption. Actual dealer net-position is opaque; methodology pages from MenthorQ/Glassnode/etc. make explicit assumptions worth checking.
- **Single-venue blind spot**: if hedging concentrates on a different venue than where OI is observed, GEX estimate misses flow.
- **Implementation gap**: multiple GEX-computation methodologies exist (Spotgamma, MenthorQ, Glassnode each compute differently); cross-source comparison can show ±25% disagreement.

## Related

- [[trading/mechanisms/options-gamma-perp-effects]] — operational mechanism page using GEX
- [[trading/mechanisms/skew-based-positioning-signals]] — companion vol-mechanism
- [[foundation/sources/sinclair-volatility-trading]] — primary source (Ch 5 dealer hedging mechanics)
- [[foundation/market-structure/dealer-economics]] — foundational dealer mechanics
- [[glossary/vanna]] — companion higher-order greek
- [[glossary/charm]] — companion higher-order greek

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 1 (BSM gamma derivation), Ch 5 (dealer hedging dynamics)
- MenthorQ. *Dealer Flow in BTC Options Guide*.
- Glassnode. *Taker-Flow-Based Gamma Exposure* methodology.
- Spotgamma. *Equity options GEX framework* (2015+).
