---
title: "Intermarket Analysis — Murphy's Cross-Market Framework"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [macro, intermarket, murphy, regime-detection, day-swing-applicable]
---

# Intermarket Analysis — Murphy's Cross-Market Framework

> **TL;DR**: Murphy's signature contribution. Stocks, bonds, dollar, and commodities don't move independently — they relate in predictable (regime-dependent) ways. The four-asset relationship: dollar ↔ commodities (inverse), commodities ↔ bonds (inverse), bonds ↔ stocks (positive), stocks ↔ dollar (regime-dependent). For crypto: the framework expands to BTC ↔ ETH ↔ alts ↔ stablecoin supply ↔ macro liquidity. Key insight: confirmation across markets dramatically improves signal quality vs. single-market analysis.

## The Classic Four-Asset Framework

Murphy's framework (Ch 17) traces relationships between US Treasuries, US dollar, commodities, and equities:

```
                    Bonds
                  ↑       ↓
                  │       │
                Stocks   Commodities
                  ↑       ↓
                  │       │
                    Dollar
```

The directional logic:
- **Dollar ↑ → Commodities ↓** (commodities priced in USD; stronger USD = lower commodity prices)
- **Commodities ↓ → Bonds ↑** (lower inflation expectations → lower yields → higher bond prices)
- **Bonds ↑ → Stocks ↑** (lower yields make stocks more attractive; supports higher equity multiples)
- **Stocks ↑ → Dollar ↑** (capital flows into dollar assets; this leg is most regime-dependent)

The framework is *not* a deterministic system. It's a *regime guide*: when the four are aligned (e.g., dollar up, commodities down, bonds up, stocks up), the macro narrative is internally consistent. When they diverge, regime change is potentially underway.

## Why The Framework Works

The mechanism is causal, not coincidental:

1. **Inflation transmission** — commodities are inflation. When commodities fall, inflation expectations fall, allowing central banks to cut rates, lifting bonds.
2. **Discount rate** — bond yields are the discount rate for equity valuations. Lower yields → higher equity multiples mathematically.
3. **Capital flow** — global capital chases the highest risk-adjusted return. When US equities outperform, USD demand rises (foreign investors buying USD to buy US stocks).
4. **Currency arbitrage** — commodity prices in non-USD currencies adjust inversely to USD strength.

These mechanisms operate concurrently. The framework summarizes their net effect.

## Regime Identification — The Core Use

The framework's primary value: **identifying which regime the market is in**.

Murphy implicitly identifies four regimes:

| Regime | Bonds | Stocks | Commodities | Dollar | Macro state |
|--------|-------|--------|-------------|--------|-------------|
| **Reflation** | Down | Up | Up | Mixed | Growth + inflation |
| **Disinflation** | Up | Up | Down | Up | Growth + falling inflation (rate-cut cycle) |
| **Deflation / risk-off** | Up | Down | Down | Up | Recession; flight to safety |
| **Stagflation** | Down | Down | Up | Down | Inflation + stagnation (1970s, partially 2022) |

Each regime has different optimal positioning. The framework tells you which set of relationships to expect to hold.

## Confirmation Across Markets — The Tradable Signal

The most-actionable use of intermarket analysis: **only take same-direction trades that align across multiple markets**.

Examples:
- **Long stocks signal**: stronger if accompanied by bonds rising and dollar falling (reflationary backdrop)
- **Long commodities signal**: stronger if accompanied by dollar weakening and bond yields rising
- **Short stocks signal**: stronger if accompanied by bond yields rising sharply (rate-shock scenario) or dollar surging (risk-off)

Single-market signals against the cross-market grain are lower-conviction. The framework filters out narrative-driven trades that don't fit the macro regime.

## Crypto Translation — Murphy's Framework Expanded

Crypto has its own intermarket relationships, layered on top of Murphy's classical four:

### The crypto-internal cross-market hierarchy

```
                    BTC
                  ↑   ↓
                  │   │
                ETH   Stablecoin supply
                  ↑   ↓
                  │   │
                  Alts (and DeFi)
```

- **BTC dominance** ↔ **alt strength** (BTC.D rising = alts compressing; BTC.D falling = alt season)
- **Stablecoin supply** ↔ **all crypto risk assets** (rising stablecoin float = dry powder building; falling = capital exiting)
- **ETH/BTC ratio** ↔ **risk-on/off within crypto** (ETH outperforming = risk-on; BTC outperforming = risk-off)

### Crypto-classical cross-market

Crypto has a real, regime-dependent relationship with traditional markets:

| Macro | Crypto effect |
|-------|---------------|
| **DXY (dollar) up** | Crypto down (inverse correlation, like commodities) |
| **US 10Y yield up** | Crypto down (rate competition; risk-off) |
| **Nasdaq up** | Crypto up (correlated risk-on; especially since 2020) |
| **Gold up** | Mixed (sometimes correlated as "alternative store of value", sometimes inversely as "real assets vs crypto") |
| **VIX spiking** | Crypto down (risk-off correlation) |
| **M2 money supply rising** | Crypto up (liquidity tailwind; major in 2020-21 cycle) |

### The macro-liquidity → crypto link

The single most important crypto cross-market relationship for the trader's day/swing horizon:

```
Global Liquidity (M2, central bank balance sheets) → Risk-on assets → Crypto
```

When global liquidity expands (Fed easing, China stimulus, BOJ holding rates), crypto is supported. When liquidity contracts (rate hikes, QT, USD strength), crypto is constrained.

This isn't just correlation — it's the underlying causal driver. The 2020-21 bull market was a global-liquidity event; the 2022 bear was the QT/rate-hike unwind. The 2024-25 cycle dynamics depend on which way liquidity moves.

## The Four-Layer Cross-Market Stack For Crypto Day/Swing

For practical use, the trader's day/swing trades should be filtered through four layers:

| Layer | Question | Tools |
|-------|----------|-------|
| **1. Macro regime** | What's global liquidity doing? | M2 growth rate; Fed funds rate trajectory; DXY trend |
| **2. Crypto regime** | Is BTC in bull or bear regime? | BTC vs 200-day MA; LTH cost basis; stablecoin supply trajectory |
| **3. Sector regime** | Is this an alt season or BTC dominance phase? | BTC.D direction; ETH/BTC ratio; total alt market cap vs BTC |
| **4. Asset regime** | What is *this specific token's* trend? | The Murphy/Lynch toolkit on the specific chart |

A trade aligned across all four layers is high-conviction. A trade that's pretty across layer 4 but works against layers 1-3 is fighting the macro grain.

## Confirmation vs. Divergence Signals

### Confirmation (high-conviction)
- BTC rallying + DXY falling + global M2 rising + funding rates calm → genuine demand-driven rally
- BTC selling off + DXY rising + bond yields spiking + risk-off everywhere → genuine deleveraging

### Divergence (regime-change warning)
- BTC rallying while DXY rising → unusual; one of them is wrong; watch for reversion
- Stocks selling off while BTC holding → potential decoupling moment; rare but historically marked transition periods
- Bonds and stocks both falling → typical of inflation shocks; risk-off everywhere except real assets / commodities

The divergence signals are *signals to investigate*, not signals to trade — they precede regime changes by hours to weeks.

## Honest Caveats

- **Correlations are not constants.** The "BTC = high-beta tech stock" correlation that defined 2020-21 weakened in 2024-25 with ETF flows. Frameworks must be re-validated periodically.
- **Macro signals are slow.** The framework operates at multi-week to multi-month timescales. For intraday or short-swing trades, intermarket analysis filters more than it triggers.
- **Self-reflexivity.** When a relationship becomes consensus, it weakens — too many traders position for it. The "BTC follows Nasdaq" trade was crowded by 2023.
- **Regime changes are abrupt.** The framework tells you the current regime; it can't tell you when the regime shifts. Position sizing must allow for regime-change-induced gaps.

## How To Use Intermarket Analysis

For the day/swing trader:

1. **At the start of each trading week**: assess the four layers — macro regime, crypto regime, sector regime, asset regime
2. **Set bias**: trades should be biased in the direction the four-layer stack supports
3. **For each candidate trade**: check whether it aligns with the bias or fights it
4. **Size conviction by alignment**: full size for fully-aligned trades; reduced size or skip for misaligned trades
5. **Monitor for regime change**: divergence signals between layers (e.g., crypto rallying while macro liquidity contracting) flag for caution

This is the most important macro-overlay framework the wiki has. It complements [[trading/mechanisms/cyclical-timing]] (which is more cycle-position than regime-state).

## Hypothesis (Structured Format)

```
Hypothesis:    BTC long entries that align with all four cross-market
               layers (macro reflationary, crypto bull regime, BTC
               dominance phase, asset trend up) produce significantly
               higher returns than entries that only align at the asset
               level.

Mechanism:     Each cross-market layer adds a regime-confirmation filter,
               removing trades that fight macro liquidity, crypto regime,
               or sector flow. Even if individual signals at the asset
               level look identical, the macro tailwind/headwind makes
               the difference between fighting and riding.

Observable:    For each entry: macro regime (M2 growth, DXY trend),
               crypto regime (BTC vs 200d MA), sector regime (BTC.D
               direction), asset signal.

Expected:      Four-aligned entries produce 1.5-2x the win rate of
               asset-only entries, with similar or larger average
               winners.

Falsifier:     If alignment doesn't improve outcomes by ≥10 percentage
               points on win rate, the cross-market layers aren't adding
               edge.
```

## Key Takeaways

- Murphy's classical framework: bonds ↔ stocks ↔ commodities ↔ dollar form a regime-dependent quadrilateral
- The framework identifies *which regime* the market is in (reflation, disinflation, deflation, stagflation)
- The most-actionable use is *confirmation*: only take trades that align across multiple markets
- Crypto extends the framework: BTC ↔ ETH ↔ alts ↔ stablecoin supply, plus the crypto/macro link via liquidity (M2, central bank balance sheets, USD)
- Four layers for crypto day/swing: macro regime, crypto regime, sector regime (alt season vs BTC dominance), asset regime
- Aligned trades across all four layers are high-conviction; misaligned trades fight the macro grain
- Regime-changes are abrupt; correlations are not constants

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; the four-layer crypto stack (macro / crypto regime / sector / asset) populates each layer with carrier features (BTC.D, ETH/BTC, stablecoin supply, LTH cost basis, etc.)
- [[foundation/sources/murphy-technical-analysis]] — source
- [[foundation/macro/inflation-and-investments]] — Graham's complementary inflation framework
- [[trading/mechanisms/cyclical-timing]] — Lynch's cycle-position framing combines with Murphy's regime framing
- [[trading/mechanisms/group-leader-divergence]] — within-class version of intermarket analysis
- [[foundation/market-wisdom/dow-theory-and-ta-premises]] — Dow tenet 4 (averages must confirm) is the equity-only version of intermarket confirmation

## Open Questions

- **What's the empirically-strongest crypto-specific cross-market signal?** Hypothesis candidates: M2 growth rate; stablecoin supply velocity; BTC.D rate-of-change.
- **Has the BTC-Nasdaq correlation permanently weakened post-ETF, or is it regime-dependent?** Affects how much weight the macro layer should carry.
- **Can the four-layer stack be operationalized as a single composite filter (e.g., "trade only when ≥3/4 layers are aligned")?** Testable.

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets*, Ch 17 ("Stocks and Futures: Intermarket Analysis"), 1999.
- Murphy, John J. *Intermarket Technical Analysis* (1991) — earlier dedicated treatment; not in wiki.
- Murphy, John J. *Intermarket Analysis: Profiting from Global Market Relationships* (2004) — the modernized standalone book; not in wiki.
