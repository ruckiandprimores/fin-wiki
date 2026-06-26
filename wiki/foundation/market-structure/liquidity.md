---
title: "Liquidity — Harris's Framework"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, liquidity]
---

# Liquidity — Harris's Framework

> **TL;DR**: Harris's most-quoted definition: "liquidity is the ability to trade when you want to trade." Decomposed into four measurable dimensions — **immediacy** (speed), **width** (spread cost), **depth** (size you can trade without moving the price), **resilience** (how fast prices recover after a temporary impact). Different traders need different liquidity dimensions; venues compete by offering different liquidity profiles. The framework converts a vague concept into a measurable, tradable market property — directly applicable to evaluating any crypto venue.

## The Definition

> "Liquidity is the ability to trade when you want to trade."
> — Harris, throughout

That definition sounds simple but contains the structure: *when you want to* (immediacy), *the trade* (size and price), *to trade* (executability). Harris decomposes liquidity into four operationally-distinct dimensions:

## The Four Dimensions of Liquidity

### 1. Immediacy

How fast you can complete a trade. Measured in time from order submission to fill.

- High immediacy = trade completes in milliseconds-seconds (liquid markets)
- Low immediacy = takes hours or days to fill (illiquid markets, large blocks)

The market order vs. limit order tradeoff is fundamentally an immediacy choice — market orders prioritize immediacy and pay the spread; limit orders save the spread but sacrifice immediacy.

### 2. Width

The cost of trading per unit, typically measured as the bid/ask spread (or, more rigorously, *effective spread* = (fill price − arrival mid) × 2).

- Narrow width = low transaction cost (tight spreads)
- Wide width = high transaction cost (wide spreads, illiquid markets)

Width is the most-watched dimension because it's the most visible — every quote shows the spread. But width alone is misleading without depth.

### 3. Depth

How much you can trade at the displayed price. Often a small spread is only available for tiny size; trying to trade larger size pushes through deeper levels of the order book at progressively worse prices.

- Deep market = can trade large size at displayed price (or close to it)
- Thin market = displayed quote good only for small size; large orders move the price substantially

The combination of width + depth is what serious traders care about. A 1bp spread on $1000 of size is meaningless if you need to trade $10M.

### 4. Resilience

How fast prices recover after a temporary order-flow imbalance. After a large market order pushes price down, does it bounce back to the prior level (resilient market) or stay at the new lower level (un-resilient)?

- High resilience = temporary impacts decay quickly
- Low resilience = price impacts are sticky / permanent-looking

Resilience matters most for traders who need to execute in pieces over time — they want to know whether their early pieces will be undone by liquidity-replenishment, or whether each piece moves the durable price.

## The Iceberg of Transaction Costs

Harris's "iceberg" metaphor (Ch 21) captures the full cost of liquidity:

```
        EXPLICIT (visible above the surface)
        ┌──────────────────────────┐
        │ Commissions              │
        │ Exchange fees            │
        │ Settlement fees          │
        └──────────────────────────┘
        ─────────── (water line) ───────────
        IMPLICIT (hidden below the surface)
        ┌──────────────────────────┐
        │ Bid/ask spread (width)   │
        │ Market impact (depth)    │
        │ Opportunity cost         │
        │ Delay cost               │
        │ Adverse selection        │
        └──────────────────────────┘
```

Most novice traders see only the explicit costs. Experienced traders know that implicit costs typically *dominate* — often 5-10x larger than commissions for institutional-size orders.

For the trader's day/swing trading on crypto perps:
- Explicit: 0.04-0.06% per side (taker fee on Binance/Bybit)
- Implicit: spread + market impact + funding accumulation + adverse selection on news events

Total round-trip cost for a typical day/swing trade in crypto perps is probably 0.15-0.40% — meaningfully more than just the visible commission.

## Liquidity Suppliers (Who Provides It)

From Ch 13-18:
- **Dealers / market makers** — supply liquidity in exchange for spread capture
- **Value traders** — supply liquidity at extreme prices when they perceive value (the "outside spread" of any market)
- **Block traders** — facilitate large trades that wouldn't fit normal liquidity supply
- **Arbitrageurs** — supply liquidity by trading temporary price disparities
- **Buy-side traders** with limit orders — incidental liquidity supply

In crypto:
- CEX market makers (Cumberland, GSR, Wintermute, Jump) — primary
- AMM LPs — passive but quantitatively important
- Arbitrageurs — cross-exchange and cross-instrument
- OTC desks — block-size facilitators
- Limit-order writers on CEX/DEX limit-order books

## Liquidity Demanders (Who Consumes It)

- **Buy-side traders with utilitarian needs** — hedgers, indexers, liquidating traders
- **Information-motivated traders** — informed traders moving on news
- **Speculators** — profit-motivated trading on opinions or patterns

In crypto:
- Retail traders (large pool of liquidity demand on perps)
- Crypto funds executing positions
- Hedgers (limited; some miners, some treasury operations)

## The Resilience-Liquidity Tradeoff

Harris emphasizes that resilience is often *traded off* against width and depth. A market with extremely narrow visible spreads (HFT-dominated markets) often has poor resilience — quotes vanish during stress, and the apparent liquidity disappears just when you need it.

This is well-documented in equity markets:
- 2010 Flash Crash: visible liquidity disappeared in seconds
- 2014 Treasury flash crash: similar pattern
- 2020 March: bid/ask spreads widened dramatically across markets

In crypto:
- 2021 May 19 BTC crash: visible liquidity vanished; effective spreads widened 10x+
- Stablecoin de-pegs (UST 2022, USDC March 2023): apparent liquidity disappeared
- Liquidation cascades systematically demonstrate poor crypto perp resilience

The lesson: visible width during normal times is a poor predictor of true liquidity during stress.

## Latent Liquidity

Harris discusses (in Ch 19 with the Liquidnet example) the concept of *latent liquidity* — orders that exist somewhere (in trader blotters, in dark pools, in indication-of-interest systems) but aren't displayed publicly.

Latent liquidity matters because:
- A market with deep displayed liquidity may have little latent liquidity (HFT-dominated — quotes withdraw under stress)
- A market with thin displayed liquidity may have substantial latent liquidity (institutional venues with negotiation)

In crypto:
- OTC desks hold latent liquidity (block-size institutional orders not on exchange)
- Some DEX aggregators access latent liquidity through RFQ networks (CoW, 1inch Fusion)
- On-chain MEV market is partly latent liquidity (block builders see flow before it executes)

## Why Liquidity Matters For Strategy

Liquidity directly affects every trading strategy:

1. **Strategy capacity** — illiquid markets can't absorb size. A backtest that shows 100% returns on a strategy that requires daily trading 1% of average daily volume will dramatically underperform in production.

2. **Implementation shortfall** — the gap between paper backtest and real fills. Driven entirely by implicit liquidity costs.

3. **Stop-loss reliability** — stops in illiquid markets often slip through wide spreads, especially during stress. The "effective stop" can be far below the "set stop" in cascading-liquidation scenarios.

4. **Strategy selection** — high-frequency strategies require deep, narrow markets. Long-horizon strategies tolerate wider spreads. Match strategy to liquidity profile of target market.

## Crypto-Specific Liquidity Patterns

### CEX vs DEX

- **CEX liquid pairs (BTC/USDT)**: Best width + depth; reasonable resilience under normal conditions; resilience degrades sharply under stress
- **CEX small-cap alts**: Modest width; limited depth; often poor resilience
- **DEX (Uniswap-style AMMs)**: Width determined by pool fee tier; depth determined by TVL; resilience is *better* than CEX in some senses (no quote-pulling) but width can be unfavorable for size due to constant-product slippage
- **DEX limit-order books (dYdX, Hyperliquid)**: Operate like CEX from a liquidity perspective

### Liquidity profile by token tier

| Tier | Typical effective spread | Depth | Resilience |
|------|-------------------------|-------|-----------|
| BTC, ETH | 1-5 bps | Very deep | Generally high; degrades under cascades |
| Top-50 alts | 5-25 bps | Moderate | Moderate |
| Top-200 alts | 25-100 bps | Limited | Poor |
| Long-tail / micro-caps | 100-500+ bps | Minimal | Essentially zero under stress |

This taxonomy directly informs position sizing and venue selection. Trying to execute a $1M position in a top-200 alt on a single CEX will reveal the depth limit immediately.

## Hypothesis (Structured Format)

```
Hypothesis:    Crypto liquidity dimensions (width, depth, resilience) are
               *positively* correlated under normal conditions but
               *negatively* correlated during stress events. The
               correlation breakdown is predictable from market structure
               (HFT-dominated venues lose liquidity faster than passive-LP
               venues during stress).

Mechanism:     HFT market makers operate algorithms that withdraw quotes
               during high-uncertainty events to avoid adverse selection.
               Passive AMM LPs cannot withdraw — their capital is in the
               pool. So during stress, AMMs paradoxically maintain liquidity
               (with worse pricing) while CEX visible liquidity collapses.

Observable:    Effective spread + depth at multiple price levels measured
               at: quiet hours, pre-event, during event, post-event. For
               Binance vs Uniswap on the same asset (e.g., ETH).

Expected:      During stress: CEX effective spreads widen 5-20x; AMM
               effective spreads widen 1.5-3x. Depth at displayed levels
               drops dramatically on CEX, less on AMM.

Falsifier:     If CEX and AMM exhibit similar liquidity-degradation patterns
               during stress, the structural difference in market-making
               doesn't translate to liquidity-supply behavior as predicted.
```

## Operational Implications

For the trader's day/swing trading:

1. **Measure liquidity, don't assume it.** Visible spreads ≠ true liquidity. Test small orders to gauge depth and resilience before committing size.

2. **Don't trade at displayed quote during stress events.** Wait for liquidity to normalize, or accept that you'll pay multi-x normal width.

3. **Different venues for different needs**:
   - Speed/immediacy → CEX market orders on liquid pairs
   - Best price → CEX limit orders or DEX aggregators (CoW, 1inch)
   - Block size → OTC desks (Cumberland, FalconX) for >$1M in alt, regardless of CEX appearance

4. **Stop placement awareness**: in illiquid markets, "stop at -3%" can become "filled at -8%" during cascades. Size positions to survive the *effective* stop, not the *intended* stop.

5. **Know which liquidity dimensions matter for your strategy**:
   - Day/swing trades on liquid majors: width matters most
   - Block trades: depth matters most
   - Stop-driven trades: resilience matters most
   - Iceberg execution: latent liquidity matters most

## Connection to Existing Wiki Pages

This page operationalizes:

- [[foundation/market-structure/microstructure-themes]] — Themes 1, 3 (information asymmetry, externality) drive liquidity supply behavior
- [[foundation/market-structure/bid-ask-spread-decomposition]] — Width is the spread; this page adds depth, resilience, immediacy as additional dimensions
- [[foundation/market-structure/dealer-economics]] — Dealers are the primary suppliers; their behavior under stress drives resilience
- [[foundation/market-structure/order-flow-externality]] — Latent liquidity is partly the externality value not yet captured by displayed quotes

Connects to:

- [[trading/mechanisms/volume-analysis]] — Volume is one observable correlate of liquidity but not equivalent to it
- [[investment/allocation/equity-management-rules]] — Position sizing must account for liquidity (max position ≤ X% of average depth at intended stop level)

## Key Takeaways

- Liquidity = ability to trade when you want to trade
- Four dimensions: immediacy, width, depth, resilience — each operationally distinct and measurable
- Iceberg of transaction costs: explicit costs (commissions) are visible; implicit costs (spread, impact, opportunity, adverse selection) typically dominate
- Resilience matters most under stress and is often the dimension that breaks first
- Latent liquidity (orders not on the book) can be substantial and accessible via RFQ / OTC / aggregators
- Crypto liquidity profile varies dramatically by token tier and by venue type
- Stress events expose the gap between visible liquidity (HFT-dominated) and true liquidity (which is much worse)
- Strategy capacity, implementation shortfall, stop reliability, and venue selection all hinge on liquidity assessment

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; cross-venue liquidity asymmetry (CEX vs DEX, cross-CEX) is catalogued there as a microstructure-asymmetry carrier
- [[foundation/sources/harris-trading-exchanges]] — source
- [[foundation/market-structure/microstructure-themes]] — Themes that drive liquidity dynamics
- [[foundation/market-structure/bid-ask-spread-decomposition]] — Width dimension expanded
- [[foundation/market-structure/dealer-economics]] — Liquidity suppliers' decision-making
- [[foundation/market-structure/order-flow-externality]] — Latent liquidity is partial-externality
- [[foundation/market-structure/bubbles-crashes-circuit-breakers]] — Liquidity collapse during crashes
- [[trading/mechanisms/volume-analysis]] — Volume vs liquidity distinction
- [[foundation/methodology/deployment-edge-threshold]] — slippage = function of width + depth; methodology page for net-of-friction edge floor uses this framework

## Open Questions

- **What's the empirical resilience profile of BTC perps across recent flash crashes (May 2021, June 2022, March 2023)?** Quantification would enable better stress-aware position sizing.
- **Is there a tradable signal in the ratio of CEX liquidity to AMM liquidity during stress events?** Hypothesis: when CEX collapses faster than AMM, it signals stress is HFT-driven, not fundamental.
- **Does latent liquidity in crypto OTC desks remain elevated during stress, or does it disappear too?** Anecdotally OTC desks widen but remain available; quantification useful.

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 19 ("Liquidity") and Ch 21 ("Liquidity and Transaction Cost Measurement"), Oxford University Press, 2003.
