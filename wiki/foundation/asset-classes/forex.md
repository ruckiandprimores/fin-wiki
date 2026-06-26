---
title: "Forex (FX) — Asset Class Reference"
status: seedling
created: 2026-04-30
updated: 2026-04-30
tags: [asset-class, forex, fx, market-structure, reference]
---

# Forex (FX) — Asset Class Reference

> **TL;DR**: FX is the largest financial market in the world ($7+ trillion daily volume), 24-hour, OTC, dealer-driven, with retail-accessible leverage from 1:50 to 1:500. The the wiki architecture (ARCHITECTURE.md) framework calls FX one of the primary mature-market sources for [[foundation/sources/lineage|cross-market transfer to crypto]]. Many crypto perp market features (24-hour, leverage, sessions, spread + funding economics) are FX-derived. This page is the asset-class reference for understanding the source market that crypto perps borrow from.

## What Forex Is

The market for trading one currency against another. Quoted as currency pairs:
- **EUR/USD** = price of 1 EUR in USD (e.g., 1.0850 = 1 EUR costs 1.0850 USD)
- **USD/JPY** = price of 1 USD in JPY (e.g., 150.00 = 1 USD costs 150 JPY)

When you "buy EUR/USD," you simultaneously buy EUR and sell USD.

## Market Structure

| Feature | FX | (For comparison: Crypto perps) |
|---------|-----|-------------------------------|
| **Daily volume** | $7+ trillion | $50-200B (varies by venue/cycle) |
| **Hours** | 24/5 (closed weekends) | 24/7 |
| **Venue** | OTC, dealer-driven; ECN aggregators for retail | CEX-driven (Binance, OKX); DEX growing |
| **Spread mechanic** | Spread-only (bid/ask) | Spread + funding rate (perps) |
| **Leverage** | 1:50 (US retail) to 1:500 (offshore) | 1:10 to 1:1000 (varies by venue) |
| **Settlement** | T+2 (spot); cash for retail/leveraged | Real-time (no settlement T+N) |
| **Regulation** | Heavy (NFA, FCA, etc.) | Variable to none |
| **Manipulation risk** | Low (large market, dealer competition) | Variable (high on lower-cap) |
| **Common pairs** | EUR/USD, USD/JPY, GBP/USD, USD/CHF, USD/CAD, AUD/USD, NZD/USD | BTC/USDT, ETH/USDT, BTC/USD perps |

## The Three Trading Sessions

FX never sleeps weekday-wise but volatility is concentrated by session:

| Session | UTC hours | Characteristic |
|---------|-----------|----------------|
| **Asia (Tokyo)** | 00:00 - 09:00 | Lower volatility; JPY pairs most active |
| **London** | 07:00 - 16:00 | Highest volatility; majors active; sets daily trend |
| **New York** | 12:00 - 21:00 | High volatility through London overlap; USD news drops |

The London/NY overlap (12:00 - 16:00 UTC) is the highest-volume window. Many trend-day moves happen during this overlap.

**Crypto translation**: crypto markets show weaker but still measurable session effects. The LDN/NY overlap correlates with measurable BTC volatility upticks. The Asia session often sets the day's range. This is one of the cleanest cross-market transfers from FX to crypto.

## Currency Pair Categories

| Category | Examples | Characteristics |
|----------|----------|-----------------|
| **Major pairs** | EUR/USD, USD/JPY, GBP/USD, USD/CHF | Tightest spreads, deepest liquidity, most-watched |
| **Minor pairs** (cross pairs) | EUR/GBP, EUR/JPY, GBP/JPY | Wider spreads; volatility from secondary central banks |
| **Exotic pairs** | USD/TRY, USD/ZAR, EUR/HUF | Wide spreads; high volatility; political risk |

For the trader's crypto context: BTC/USD ≈ a major pair (deep, watched, tight); altcoin/BTC pairs ≈ minor pairs (less liquidity, more idiosyncratic); micro-cap altcoin pairs ≈ exotic pairs (wide spreads, manipulation risk).

## The Pip — The FX Unit of Measurement

A "pip" is the smallest standard price increment:
- For most pairs: 0.0001 (the 4th decimal)
- For JPY pairs: 0.01 (the 2nd decimal)

Trade sizing in FX is typically:
- **Standard lot** = 100,000 units of base currency (1 pip = $10 in account value)
- **Mini lot** = 10,000 units (1 pip = $1)
- **Micro lot** = 1,000 units (1 pip = $0.10)

The pip framework lets traders standardize stop-loss and take-profit distances across different pair conventions.

**Crypto translation**: crypto doesn't have a true "pip" but has equivalent concepts (basis points; satoshi; gwei). The standard unit varies by token. Position sizing in [[investment/allocation/equity-management-rules|equity management]] terms is what matters, regardless of the unit.

## Why FX Matters for Crypto (the wiki architecture (ARCHITECTURE.md) Section 5)

Per the wiki architecture:

> "Cross-Market Transfer: The Dominant Idea Generation Mode... Instead of asking AI to invent from nothing, feed it mature-market strategies from FX, commodities, equities, hedge fund literature and ask it to translate or mutate for crypto."

FX is on this priority list specifically because:

1. **24-hour OTC dealer-driven structure** is the closest equity-like analog to crypto's 24-hour CEX-driven structure
2. **Leverage availability** matches crypto's perp leverage
3. **Session dynamics** translate cleanly to crypto
4. **Spread economics** + (in FX, occasionally) overnight swap costs ≈ crypto's spread + funding economics
5. **Carry trade** — the textbook FX strategy of borrowing low-yield currency, investing in high-yield currency — has a direct crypto analog (perp funding-rate carry, basis trades, stablecoin yield arbitrage)
6. **Counter-trend mean reversion** in major pairs at extreme RSI / Bollinger band readings transfers well to BTC/ETH perp regimes

The wiki has [[foundation/sources/forex-10-essentials]] as a tactical retail-FX source. Higher-priority FX sources still missing:
- ☐ **Currency Trading and Intermarket Analysis** (Lien) — patterns and central-bank dynamics
- ☐ **Inside the House of Money** (Drobny) — global macro interviews; FX/macro lens

## FX-Native Mechanisms Worth Knowing

These are FX-specific concepts that may translate to crypto:

### Carry Trade

Borrow in low-yield currency (e.g., JPY at 0%), invest in high-yield currency (e.g., AUD at 4%), capture the rate differential. Works until exchange-rate moves against you.

**Crypto analog**: perp funding rate carry. When BTC perp funding is +0.05%/8h, longs pay shorts. A funding-rate carry strategy: short the perp + long the spot = market-neutral, capture funding. This is a direct FX-carry translation. Documented in the wiki architecture (ARCHITECTURE.md) as the "a funding-carry strategy" strategy already running in the trader's portfolio.

### Central Bank Calendar Trading

FX prices respond strongly to central bank rate decisions, FOMC meetings, ECB meetings, BoJ interventions. Calendar-aware trading times entries/exits around these events.

**Crypto analog**: FOMC meeting impact on BTC; CPI release impact on crypto; SEC enforcement actions; ETF flow data releases. Less standardized than FX's calendar but equally regime-defining.

### Risk-On / Risk-Off (RORO)

When global markets are "risk-on," AUD, NZD, EM currencies strengthen vs. JPY, CHF (safe havens). When risk-off, the inverse.

**Crypto analog**: BTC behaves as a risk-on asset in most regimes; gold and stablecoins behave as safe havens. The RORO framework directly applies to crypto's macro positioning.

### Pair Cointegration

Currency pairs sometimes cointegrate (e.g., EUR/USD and GBP/USD), enabling pairs-trading strategies.

**Crypto analog**: ETH/BTC pair trading; altcoin/BTC ratio trades; basis trades on the same asset across exchanges.

## Crypto-vs-FX: Important Differences

Where the FX framework breaks down for crypto:

| Difference | Implication |
|------------|------------|
| Crypto has no central bank → no rate-decision calendar | RORO works without specific events |
| Crypto cycle is supply-shock driven (halvings) → no analog in FX | Cyclical mechanism (Lynch's framework) is more important than carry |
| Crypto manipulation possible at coin level → not common in FX major pairs | TA reliability lower for low-cap alts |
| Crypto on-chain data → no FX equivalent | On-chain confluence is a crypto-only edge |
| Crypto liquidations → cleaner than FX margin call mechanics | Liquidation cluster trading is crypto-specific |

## Open Questions

- **What's the empirical correlation between BTC volatility and FX major-pair sessions?** Anecdotally strong; deserves measurement.
- **Does the FX carry-trade analog (perp funding carry) survive at scale, or does it get arbitraged away as institutional capital enters crypto?** Currently working per the wiki architecture ("a funding-carry strategy"); long-term durability uncertain.
- **Are there FX-specific mechanisms not yet translated to crypto?** Likely yes — central bank intervention play, breakout-from-range plays around FX fixings.

## Related

- [[foundation/sources/forex-10-essentials]] — tactical retail FX source
- [[foundation/sources/murphy-technical-analysis]] — Murphy's Ch 17 on intermarket analysis touches FX
- [[foundation/macro/intermarket-analysis]] — Murphy's framework where FX/dollar/commodities/bonds interact
- [[foundation/sources/lineage]] — FX cluster (mostly unfilled — Lien, Drobny pending)
- [[trading/mechanisms/cyclical-timing]] — crypto's cycle differs from FX's; this is where the frameworks diverge

## Sources

- Martinez, Jared F. *The 10 Essentials of Forex Trading*, Ch 2 ("Introduction to the FOREX"), 2007.
- BIS Triennial Survey 2022 (FX volume figure).
