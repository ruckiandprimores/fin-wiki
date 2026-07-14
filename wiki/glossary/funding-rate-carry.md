---
title: "Funding Rate Carry"
status: seedling
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, perp, funding-rate, carry, hayes, terminology]
---

# Funding Rate Carry

The yield component of crypto perpetual-future positions: a periodic payment between perp longs and perp shorts proportional to the perp-spot basis, settled every 8h on most major CEXes (1h on Bitfinex; 4h variants).

## Mechanism

Each settlement period, exchanges compute the average perp-spot premium over the period and apply a funding rate:

- **Funding rate > 0** (perp trading premium to spot): longs pay funding to shorts
- **Funding rate < 0** (perp trading discount to spot): shorts pay funding to longs

The mechanism anchors perp price to spot without requiring expiration / delivery. Per Hayes's primary-source framing in [[foundation/sources/hayes-essays|Adapt or Die]]:

> "If the perp traded at an average 1% premium to spot over the last eight hours … longs would pay 1% to shorts."

## Carry interpretation

For a market-neutral position (long spot + short perp), funding payments are pure carry yield with no directional exposure (modulo basis decay between settlements). Annualized:

```
Carry yield (annualized) = funding_rate_8h * 3 * 365
```

A persistent +30bp/8h funding rate annualizes to ~33% per year — large in absolute terms vs equity risk-free rate, reflecting **the leverage premium retail longs pay to maintain levered exposure without rolling**.

**The market-neutral construction is a distinct mechanism family** documented at [[../trading/mechanisms/delta-neutral-funding-harvest]] — separate from directional FRA-signal trading (a funding-carry strategy-style) and from broad-carry-as-trajectory ([[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]]). The delta-neutral variant **persists at compressed magnitude** even when broad directional carry decays (Borri 2025-negative). Q1 2026 realized yields: ~3.7% via Ethena sUSDe, ~6-11% via cross-exchange Boros construction.

## Regime conditioning

- **Persistent top-decile funding** = retail crowded long → carry trade highly profitable + positioning-extreme signal for contrarian short setups (per [[trading/mechanisms/funding-cycle-dynamics]] §Positioning-extreme signals)
- **Funding flips (positive → deeply negative or vice versa)** = cascade event signature; aftermath setup per [[trading/mechanisms/liquidation-cascade-aftermath]] §Operational Thresholds (≥40bp swing in 8h is a cascade detection threshold)
- **Bottom-decile funding regime** = retail crowded short OR forced post-cascade — long-side carry available; squeeze setup possible

## Macro overlay context

Per [[glossary/dollar-milkshake-thesis]] and [[foundation/sources/hayes-essays|Hayes Theme 1]]: persistent-positive funding regimes correlate with dollar-liquidity expansion (bank-issued-stablecoin growth + retail leverage availability + risk-on positioning). Funding-regime is a leveraged-positioning indicator that aligns with macro liquidity conditions, not an independent crypto-internal variable.

## Honest caveat

- Funding rate carry has **cascade tail risk**: cascade events can briefly invert funding by 100+ bp/8h, and structures with insufficient margin can be liquidated at the worst moment. The trader's a funding-carry strategy explicitly handles this via pump-detection + 1h-delay tricks
- Funding-rate is **already widely-traded as a primary signal**; edge from naive funding-extreme strategies is largely arbitraged out. Per recent the backtesting lab evidence ([[trading/rejected/funding-extreme-exhaustion-4h-2026-05]]), naive funding-extreme strategies showed corr ≤+0.07 with everything (most-orthogonal in lab) but persistence requirements limit eligibility — operational form is delicate

## Related

- [[../trading/mechanisms/funding-cycle-dynamics]] — primary mechanism family page
- **[[../trading/mechanisms/delta-neutral-funding-harvest]]** — **NEW 2026-05-18**: market-neutral construction documented as standalone mechanism (this glossary is the concept; that page is the operational mechanism)
- **[[../trading/mechanisms/insurance-premium-harvest-overview]]** — **NEW 2026-05-18**: cross-mechanism index — funding harvest sits alongside VRP harvest as the two insurance-premium families
- [[basis-trade-crypto]] — sibling concept; basis-trade is the structure that captures the carry
- [[../foundation/sources/hayes-essays]] — Adapt or Die is primary-source on perp funding-as-basis-tracker design
- [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]] — broad-carry trajectory (directional carry, not delta-neutral)
- [[../trading/mechanisms/liquidation-cascade-aftermath]] — cascade aftermath signature uses funding-flip threshold (≥40bp/8h)
- [[../trading/mechanisms/etf-era-carry-trade-funding-distortion]] — environment-level context for the funding-rate level
- [[dollar-milkshake-thesis]] — macro overlay
- [[../trading/mechanisms/behavioral-positioning-unwind]] — sibling family using funding as positioning-extreme indicator
- [[variance-premium]] — sister concept on the options axis
