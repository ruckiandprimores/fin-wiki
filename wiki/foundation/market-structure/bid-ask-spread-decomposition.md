---
title: "Bid/Ask Spread Decomposition"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, spread, adverse-selection]
---

# Bid/Ask Spread Decomposition

> **TL;DR**: Harris's most important single concept. The bid/ask spread is not one number — it has two distinct components: (1) the **transaction cost / transitory component** that pays the dealer for cost-of-doing-business and creates "bid/ask bounce," and (2) the **adverse selection / permanent component** that compensates the dealer for losing to informed traders. Harris explicitly calls Section 14.3 "the most important lesson in this book for most readers": **uninformed traders lose to informed traders regardless of whether they use limit or market orders. They lose simply because they trade.** This is the canonical microstructure result and the deepest reason most retail traders systematically lose.

## The Two Components

Every bid/ask spread you observe is the sum of two economically distinct components:

| Component | Also called | What it pays for | Price-impact behavior |
|-----------|-------------|------------------|----------------------|
| **Transaction cost component** | Transitory component | Dealer's normal costs of doing business + monopoly profit (if any) + inventory risk premium | Price changes are **transitory** — they reverse |
| **Adverse selection component** | Permanent component | Dealer's recovery of losses to informed traders | Price changes are **permanent** — random walk |

Dealers don't quote the components separately. They quote a single bid and a single ask. But the two components have *different statistical properties* that econometricians can disentangle.

### Transaction cost component (transitory)

> "The transaction cost spread component is the part of the bid/ask spread that compensates dealers for their normal costs of doing business... If all traders knew instrument values with complete certainty, the transaction cost component would constitute the entire spread."
> — Harris, Ch 14.2.1

What the dealer is being paid for:
- Capital costs (financing inventory)
- Operational costs (staff, exchange dues, telecoms, settlement, accounting)
- Inventory risk premium (compensation for holding stuff while prices move)
- Any monopoly markup if the dealer has market power

This component creates **bid/ask bounce** — the rapid back-and-forth between bid and ask prices as buyers and sellers alternate hitting the dealer. Each individual price change reverses; the average price stays flat.

### Adverse selection component (permanent)

> "Since dealers do not know fundamental values well, they expose themselves to adverse selection from better-informed traders when they offer liquidity... If dealers set their spreads to reflect only their normal costs of doing business, their losses to well-informed traders would eventually force them out of business. Dealers must widen their spreads further to cover their losses to informed traders."
> — Harris, Ch 14.2.2

Mechanism:
1. Some traders know more than the dealer (informed traders)
2. Informed traders choose the *favorable* side — buying when value > dealer's ask, selling when value < dealer's bid
3. Each informed trade is a loss to the dealer (the dealer just sold below value or bought above value)
4. The dealer can't tell informed from uninformed traders in advance
5. The dealer widens the spread on *all* trades to recover the average informed-trader loss
6. Uninformed traders thus pay an *adverse selection tax* embedded in the spread

This is a *permanent* component because each trade carries information — the dealer revises their value estimate after each trade, and the price level moves with the revision. Unlike transitory bounce, these moves don't reverse on average.

## The Glosten-Milgrom Theorem

A famous result Harris proves: the size of the adverse selection component, computed two different ways, comes out the same.

- **Information perspective**: the adverse selection component equals (probability of informed trader) × (pricing error if informed)
- **Accounting perspective**: the adverse selection component equals the average loss to informed traders, charged across all trades

These match. The dealer's spread widening exactly equals the dealer's expected loss per trade. In equilibrium, the dealer's expected profit on uninformed trades pays for their expected losses on informed trades.

## "The Most Important Lesson In This Book For Most Readers"

Harris explicitly labels Section 14.3 with this title. The lesson:

> "Uninformed traders thus ultimately lose to informed traders regardless of how they trade. They lose simply because they trade. They can avoid the problem only by not trading."
> — Harris, Ch 14.3

This is the microstructure result that explains *systematic* retail losses. It applies regardless of order type:

| Order type | How adverse selection hurts |
|------------|----------------------------|
| **Market order** | Pays the full spread including adverse selection component; trades against the dealer's wider quote |
| **Limit order — overpriced** | Fills quickly because informed traders take the favorable side; the trader regrets trading |
| **Limit order — competitive** | When competing with informed traders, often doesn't fill because price moves away; the trader regrets *not* trading |
| **Coin-flip strategy** | Fair coin → 50% right, 50% wrong on direction; but adverse selection charge applies to all trades; net negative |

The empirical implication: even if an uninformed trader had a perfect coin-flip on direction, they'd lose money on average just from the adverse selection cost embedded in every spread they cross.

## Empirical Decomposition

Econometric methods (Glosten-Harris, Madhavan-Smidt, others) can decompose observed spreads into the two components:

- **Equities** — adverse selection typically accounts for 30-70% of total spread; varies by stock liquidity and information environment
- **Treasuries** — predominantly transaction cost; low information asymmetry
- **High-information stocks** (small-cap, biotech, news-driven) — adverse selection dominates
- **Low-information stocks** (large-cap blue-chips on calm days) — transaction cost dominates

> "These analyses indicate that in most markets the adverse selection spread component accounts for more of the total spread than does the transaction cost spread component."
> — Harris, Ch 14.2.4

For most stocks, *more than half* of the spread is adverse selection cost — the dealer's anticipated loss to informed traders, charged to uninformed flow.

## Crypto Translation

The decomposition framework applies cleanly to crypto, with crypto-specific characteristics.

### CEX spreads

| CEX type | Typical adverse selection component | Why |
|----------|-------------------------------------|-----|
| **BTC/USDT on Binance** | Modest (likely 20-40%) | Highly liquid, many participants, low single-trader information advantage |
| **Mid-cap alts on Tier-2 CEXs** | Substantial (likely 50-70%) | Asymmetric information dominates; insiders, exchanges, and large players know things retail doesn't |
| **Low-cap alts** | Extreme (often >80%) | Extreme information asymmetry; insiders know unlocks, partnerships, exit timing |

This explains the *empirical fact* that retail crypto traders lose dramatically more on small-cap alts than on BTC — it's not just that small-caps are volatile, it's that the spread is structurally more loaded against the uninformed trader.

### AMM spreads

AMMs (Uniswap, SushiSwap, etc.) are uniquely transparent about their spread structure:

- **Transaction cost component** → the LP fee (0.05%, 0.3%, 1% etc.)
- **Adverse selection component** → "loss versus rebalancing" (LVR) — the systematic value LPs lose to arbitrageurs

For volatile pairs, LVR substantially exceeds LP fees. This means **LPs in volatile AMM pairs are systematically *paying* the adverse selection cost rather than capturing it.** A passive LP is the structural loser per Harris's framework — they're providing liquidity uninformed about short-term price moves while informed arbitrageurs systematically pick them off.

The academic literature on LVR (Milionis et al., 2022) is a direct application of Harris's framework to AMMs.

### Crypto perp spreads

Perp spreads on liquid pairs (BTC, ETH) are extremely tight (often <1bp) because:
- Multiple market makers compete intensely
- Order flow is well-known to be mostly uninformed (retail leverage gambling)
- Adverse selection is low because true informed flow is small relative to total volume

This is why **crypto perp market making is profitable on liquid pairs** — high volume × tiny spread × low adverse selection ≈ steady positive expectancy.

But the same logic inverts on stressed events: when liquidations cascade, market makers face extreme adverse selection (they keep filling longs into a falling market until they pull). The empirical pattern of MMs *withdrawing during stress* is exactly this dynamic — they widen spreads or pull entirely when adverse selection spikes.

## Operational Implications

For the trader's day/swing trading:

1. **Prefer liquid markets where adverse selection is small.** BTC/ETH spot and perps have low embedded adverse selection cost. Low-cap alts have high cost.

2. **Avoid taking liquidity on news.** Just after news, the adverse selection component is at its peak — informed traders are actively trading; the spread widens; you're paying maximum adverse selection cost.

3. **Be careful as a crypto AMM LP.** You're a passive dealer paying the adverse selection cost (LVR) without knowing it. Provide liquidity only:
   - On low-volatility pairs (stablecoin/stablecoin)
   - With explicit hedging (delta-neutral LP strategies)
   - At fee tiers high enough to cover LVR (Uniswap v3 1% pools for volatile pairs)

4. **Know your adverse selection footprint.** When you take a market order, ask: am I paying the dealer's adverse-selection insurance, or am I one of the informed traders the insurance is sold against?

5. **The honest answer for most retail traders is "I'm paying the insurance."** This is the systematic reason retail loses. It's not a strategy issue — it's a structural microstructure issue.

## Connection to Existing Wiki Pages

This page deepens multiple existing wiki pages:

- [[foundation/market-structure/zero-sum-game]] — adverse selection is *the specific microstructure mechanism* by which uninformed traders lose to informed traders
- [[foundation/market-structure/order-flow-externality]] — adverse selection cost is the *load-bearing share* of why the order-flow externality is valuable to capture
- [[foundation/market-structure/microstructure-themes]] — Theme 1 (information asymmetries) operationalized
- [[foundation/market-structure/trader-taxonomy]] — explains why dealers' spreads have to be wider than naive cost-recovery would suggest
- [[trading/mechanisms/volume-analysis]] — high volume can mask high adverse selection if information is asymmetric
- [[investment/allocation/equity-management-rules]] — adverse selection is a hidden cost component that R:R calculations should account for

## Hypothesis (Structured Format)

```
Hypothesis:    Adverse selection cost is a measurable, persistent component of
               crypto perp spreads that varies systematically with information
               asymmetry. Trades initiated within X minutes of major news/events
               (where information asymmetry peaks) have demonstrably worse
               implementation than trades during quiet periods.

Mechanism:     Per Glosten-Milgrom: dealers widen spreads when probability of
               informed counterparty is high. News events, ETF flow data
               releases, FOMC decisions, exchange listings/delistings all
               create temporary information asymmetry spikes. Market makers
               widen during these windows; uninformed retail trading during
               windows pays excessive adverse selection.

Observable:    Effective spread (transaction cost + slippage) measured at:
               - Quiet hours (e.g., 02:00 UTC weekday)
               - Pre-FOMC announcement
               - Within 15 minutes post-FOMC
               - On listing announcement of major token
               
Expected:      Effective spread 2-5x higher in news-window trades compared
               to quiet-window trades for the same notional size, on the
               same instrument.

Falsifier:     If effective spreads are similar across regimes (within 1.5x)
               for the same instrument, adverse selection is not significantly
               regime-dependent in this market.
```

## Test Plan

For empirical validation:

1. Pick BTC perp on a major venue (Binance, Bybit, Hyperliquid)
2. Define event windows (major news, FOMC, ETF flow announcements) and quiet windows
3. Measure effective spread = (fill price − mid at order arrival) for marketable orders of comparable size
4. Compare distributions
5. Test whether the difference is significantly different from zero

If the hypothesis holds, the operational rule is concrete: **avoid trading during high-information-asymmetry windows**. If it doesn't hold, the framework still predicts the structural cost, just less time-varying.

## Key Takeaways

- The bid/ask spread has two distinct components: transitory (transaction cost) and permanent (adverse selection)
- Transaction cost component creates "bid/ask bounce" — reversing
- Adverse selection component compensates dealers for losing to informed traders — non-reversing
- For most stocks, adverse selection accounts for >50% of total spread
- Glosten-Milgrom theorem: the size of the adverse selection component is the same whether computed from information or accounting perspectives
- "Uninformed traders lose simply because they trade" — Harris's most important lesson for retail
- AMM LPs are *systematically* paying the adverse selection cost (loss versus rebalancing) on volatile pairs
- For crypto perps, adverse selection is small on liquid majors, large on small-caps and during stress events
- Avoid trading during high-information-asymmetry windows (news, FOMC, listings)

## Related
- [[../sources/bouchaud-trades-quotes-prices|Bouchaud et al. — Trades, Quotes and Prices (2018)]] — the adverse-selection component formalized

- [[foundation/sources/harris-trading-exchanges]] — source
- [[foundation/market-structure/microstructure-themes]] — Theme 1 (information asymmetries) operationalized here
- [[foundation/market-structure/zero-sum-game]] — adverse selection is *the specific mechanism* of uninformed-trader losses
- [[foundation/market-structure/order-flow-externality]] — adverse selection cost ≈ the externality value flowing from uninformed to informed traders
- [[foundation/market-structure/trader-taxonomy]] — dealers' need to recover from uninformed flow what they lose to informed flow
- [[trading/mechanisms/volume-analysis]] — high volume + low information asymmetry = good for taking liquidity; high volume + high asymmetry = trap
- [[investment/allocation/equity-management-rules]] — adverse selection is a hidden cost in R:R math
- [[foundation/methodology/deployment-edge-threshold]] — adverse-selection component of friction is part of the net-edge math; this page explains *why* spread is real cost, deployment-edge page applies the math

## Open Questions

- **What's the empirical magnitude of LVR for typical crypto AMM LP positions, and does fee revenue offset it?** Academic papers say no for volatile pairs; practical measurement on real positions would be useful.
- **Does adverse selection vary measurably in crypto perps across the FOMC announcement window?** Hypothesis: yes, by 2-5x.
- **Can retail traders systematically time entries to low-adverse-selection windows?** If so, the gain might be 30-60bp per trade — meaningful at scale.

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 14 ("Bid/Ask Spreads"), particularly 14.2 (spread components) and 14.3 ("The most important lesson in this book"), Oxford University Press, 2003.
- Glosten, Lawrence R., and Paul R. Milgrom. "Bid, Ask and Transaction Prices in a Specialist Market with Heterogeneously Informed Traders." *Journal of Financial Economics*, 1985 (referenced; not in wiki).
- Milionis, Moallemi, Roughgarden, Zhang. "Automated Market Making and Loss-Versus-Rebalancing." 2022 (referenced; not in wiki).
