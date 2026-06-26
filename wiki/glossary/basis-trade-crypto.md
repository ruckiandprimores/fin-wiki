---
title: "Basis Trade (Crypto)"
status: seedling
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, perp, basis, funding-rate, hayes, terminology]
---

# Basis Trade (Crypto)

The crypto-specific form of the **basis trade**: a market-neutral structure exploiting the spread between perp/futures price and spot price, with the spread realized through **funding payments** (perps) or **calendar decay** (dated futures).

## Core mechanics

**Cash-and-carry (long-side basis trade)**:
- Buy spot BTC
- Short equivalent BTC perp (or near-month dated future)
- Collect funding payments (perps) or basis decay (futures) until convergence

When funding is persistently positive (perp trading at a premium to spot), the structure earns the funding payments as P&L without directional exposure. Inverse direction works during persistent-negative-funding regimes (rare; typically post-cascade).

**Hayes's primary-source framing (Adapt or Die, Nov 2025)**:
> "If the perp traded at an average 1% premium to spot over the last eight hours … longs would pay 1% to shorts."

This is the load-bearing definition: funding rate is the **realized price of the basis trade per settlement period**.

## Operational variants (per [[trading/mechanisms/funding-cycle-dynamics]])

- **Spot-perp cash-and-carry**: hold spot BTC; short equivalent BTC perp; collect funding (and lose any spot-perp basis decay)
- **Stablecoin-funded perp short**: hold USDT/USDC; short BTC perp; collect funding (no spot exposure; pure funding play)
- **DeFi variant**: deposit ETH on lending protocol; borrow ETH; short ETH perp; capture funding minus borrow cost
- **Dated-futures variant**: short near-month CME or Deribit futures; long spot; capture basis-decay-to-expiry (less perp-specific; mechanically simpler)

The trader's running a funding-carry strategy is the canonical funding-cycle / basis-trade structure in production deployment.

## Macro context (Hayes adaptation)

Per [[foundation/sources/hayes-essays|Hayes essays]], the basis-trade ecosystem connects to dollar liquidity:

- **Bank-issued stablecoins** (per Quid Pro Stablecoin): deposits convert to JPMD-class stablecoins, freeing up to $6.8T treasury-buying capacity → stablecoin issuance is structurally bid by US fiscal funding need → permanent dollar-liquidity injection conduit
- **IORB elimination**: removing Fed interest-on-reserves forces banks to convert reserves to treasuries, releasing $3.3T → further stablecoin / T-bill demand expansion

These flow conduits make stablecoin-funded basis-trade structurally well-supplied (collateral side); the persistent-positive-funding regime is part of why.

## Honest caveat

- Basis-trade is a **carry strategy**, not a directional edge. P&L = funding-collected − borrow-cost − cascade-tail-risk. Cascade events (per [[trading/mechanisms/liquidation-cascade-aftermath]]) can briefly invert funding by 100+ bp/8h, requiring careful margin / capital management
- The trade is widely-known and crowded; spreads compress when capital is abundant
- Primary risk: forced delivery / exchange counterparty risk (FTX customer perps shorts had positions but no counterparty in Nov 2022)

## Related

- [[trading/mechanisms/funding-cycle-dynamics]] — primary mechanism family page
- [[foundation/sources/hayes-essays]] — Adapt or Die is the primary-source narrative on funding-as-basis-tracker
- [[glossary/funding-rate-carry]] — adjacent term; the funding rate is the basis-trade-per-period yield
- [[glossary/dollar-milkshake-thesis]] — macro overlay; stablecoin → treasury demand is one milkshake channel
- [[trading/mechanisms/liquidation-cascade-aftermath]] — cascade-tail-risk for basis-trade carriers
