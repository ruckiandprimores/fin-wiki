---
title: "Dealer Economics — How Market Makers Actually Make Money"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, dealers, market-making]
---

# Dealer Economics — How Market Makers Actually Make Money

> **TL;DR**: Dealers (market makers) profit by capturing the bid/ask spread on trades, but the math is more complex than "buy bid, sell ask, pocket the difference." They face two distinct risks — diversifiable inventory risk (price moves while holding) and adverse selection risk (informed traders systematically pick the favorable side). Spread widening covers both. The dealer's job is *learning value from order flow* — every trade tells them something. Universal across equities, FX, crypto perps, AMM LPs.

## What a Dealer Does

A dealer (or market maker) takes the *other side* of trades that clients want to make. Unlike a broker (agency trader) who *finds* a counterparty, a dealer *becomes* the counterparty.

Mechanically:
1. Dealer quotes a bid (price to buy) and ask (price to sell)
2. Client wants to sell → hits dealer's bid → dealer's inventory increases
3. Client wants to buy → lifts dealer's ask → dealer's inventory decreases
4. Dealer's profit comes from buying at bid, selling at ask, capturing the spread

The economics get interesting because the dealer holds inventory between trades, and inventory is risky.

## The Two Risks

### Risk 1: Diversifiable inventory risk

Random price movements while the dealer holds inventory.

> "Diversifiable inventory risks are due to events that cause price changes no one can predict... On average, they are zero. Otherwise, they would be predictable."
> — Harris, Ch 13

A dealer who holds 1000 shares of XYZ overnight faces price risk in either direction. Sometimes the price moves favorably; sometimes unfavorably. On average, neither direction is more likely than the other, so the *expected* P&L from inventory holding is zero (before fees).

But the *variance* is real, and dealers must hold capital to absorb it. The capital cost is part of the spread.

This risk is *diversifiable* across many instruments — losses on one are offset by gains on another. A dealer making markets in 50 different stocks faces less aggregate volatility than 50 single-stock dealers.

### Risk 2: Adverse selection risk

Informed traders systematically picking the favorable side. *Not* diversifiable.

> "If future price changes are inversely correlated with their inventory imbalances, the risk is an adverse selection risk."
> — Harris, Ch 13

The mechanism:
1. Informed trader knows price is about to move up
2. Informed trader buys from dealer (lifts the ask)
3. Dealer's inventory just decreased; price moves up
4. Dealer would now have to buy at the higher price to restore inventory
5. Dealer loses

This is *not random* — it correlates with the dealer's inventory imbalance because informed traders by definition pick the side that's about to win.

Adverse selection risk is *not* diversifiable across instruments because it's caused by the *systematic* presence of informed traders, not by random fluctuations.

## How Dealers Recover Their Costs

The bid/ask spread does the work. Per [[foundation/market-structure/bid-ask-spread-decomposition|spread decomposition]]:

```
Total spread = Transaction cost component + Adverse selection component
```

- The transaction cost component covers diversifiable inventory risk + cost of doing business
- The adverse selection component covers losses to informed traders

Dealers set spreads such that uninformed flow + dealer expertise + competition collectively make the dealer's overall expectancy positive. They can't make spreads too wide or competitors capture the flow; they can't make them too narrow or they go broke.

In equilibrium, dealers earn *normal economic profit* — fair return on capital + fair compensation for skill, but no excess. New entrants narrow spreads; exits widen them.

## Learning From Order Flow

> "Dealers learn about values from the order flow."
> — Harris, Ch 13

The dealer's secondary job (after capturing spreads) is *Bayesian updating* on instrument value:

1. Dealer starts with prior belief about value (V₀)
2. Trade arrives — buy or sell
3. Each trade is a signal: a buy suggests value > V₀; a sell suggests value < V₀
4. Dealer updates V₀ in the direction of the trade
5. Dealer's bid/ask updates accordingly
6. Process repeats

In effect, dealers' value estimates incorporate the information content of order flow. Dealers who do this well stay solvent; those who don't get systematically picked off and exit the market.

This is why **dealer quotes contain information** beyond just price points. Order flow imbalance, quote adjustments after trades, and quote stability all tell other market participants something.

## The Three Types of Dealer Profit Sources

A dealer's gross revenue per trade has three sources:

### 1. Transaction cost capture

For routine flow from uninformed traders, the dealer simply captures the spread on each round-trip. With high enough flow, this dominates.

Example: a market maker on BTC/USDT on Binance handles 100,000 trades per day at average 1bp effective spread. That's $100K of gross revenue per $1B notional traded. Most goes to operating expenses; some is profit.

### 2. Information advantage

Sophisticated dealers maintain an *information edge* — they know more about value than the average client. This lets them:
- Adjust quotes faster than competitors when news hits
- Skew quotes (asymmetric bid/ask) when inventory imbalance is undesirable
- Pull quotes during periods of high adverse selection risk

Dealers with information edge earn more than the simple spread captures would suggest.

### 3. Inventory positioning

Dealers can deliberately accumulate or reduce inventory based on directional view. A dealer who's bullish on the underlying takes more buy orders (offering tighter ask), accumulating long inventory. If their view is correct, they profit on the position move plus the spreads.

This converts the dealer from a pure liquidity-provider to a part-time directional speculator. Pure dealers minimize this; aggressive prop-trading dealers do it intentionally.

## Crypto Translation

### Centralized exchange market makers

Cumberland, GSR, Wintermute, Jump, B2C2, etc. operate exactly like equity dealers:
- Quote tight spreads on liquid pairs
- Profit on volume × small spread
- Pull or widen during stressed events
- Use information edge from cross-exchange data, on-chain monitoring, news flow

Their profit drivers map directly to Harris's framework.

### AMM LPs (passive dealers)

Uniswap LPs are the cleanest case of *passive* dealers:

- They quote spreads via the constant-product invariant (k = x*y)
- They have zero information about value — pure passive
- Adverse selection cost is *immense* (loss versus rebalancing — LVR)
- Their compensation is the LP fee (0.05%, 0.3%, 1%)

For volatile pairs, LP fees are typically *less* than LVR — meaning LPs lose money on volatile pairs after accounting for adverse selection. They're systematically the wrong side of Harris's dealer math.

The LPs who survive:
- Concentrated liquidity strategies (Uniswap v3) where they actively manage range
- Stable-stable pairs (low volatility → low LVR → LP fees > LVR)
- LPs who hedge price risk externally

This is one of the most counterintuitive findings in DeFi — *passive AMM LPing is structurally negative-EV* on volatile pairs, despite the appearance of "earning fees."

### Crypto perp market making

Perp MMs have the most-favorable dealer economics in crypto:
- Tight spreads on liquid pairs
- High volume
- Funding rate provides *additional* revenue stream beyond spreads
- Leverage available for capital efficiency

Profitable perp MM is a documented business (multiple firms public about scale). The economics work because retail flow is large, mostly uninformed, and structurally pays funding to short-side MMs in persistent-positive-funding regimes.

This is the structural reason the trader's existing a funding-carry strategy works — it's a dealer-economics play, capturing the funding component of spread.

### Crypto OTC desks

OTC desks combine dealer (taking principal positions) and broker (agency for clients) — broker-dealer in Harris's terminology. They face larger inventory risk (block trades) but can charge wider spreads to compensate. Profit per trade is higher; volume is lower.

## What Causes Dealer Failure

Dealers fail when:

1. **Adverse selection becomes unmanageable** — too many informed traders, dealer gets picked off systematically (e.g., MMs during 2020 oil price collapse)
2. **Inventory grows unmanageable** — sustained one-sided flow forces inventory accumulation faster than dealer can rebalance (cascading liquidations on perps)
3. **Capital is insufficient** — losing streak exhausts capital before the strategy normalizes (LVR for AMM LPs in volatile regimes)
4. **Operational failures** — exchange outage, technology failure, key personnel events

Crypto MMs have all four failure modes. Notable failures: Alameda Research (FTX), various AMM LP exits during 2022 volatility, smaller MMs during May 2021 BTC crash.

## Operational Implications

For the trader as a *taker* of liquidity:

1. **Recognize you're paying the dealer's edge** every time you cross the spread
2. **The "spread" you see is mostly compensation for the dealer's adverse-selection risk** (not their cost-of-business)
3. **Dealers widen on news / volatility** — exactly when retail wants to trade most aggressively. The cost of trading *during* these events is structurally elevated
4. **Limit orders give you the dealer's role partially** — you become a passive dealer, capturing some spread but bearing some adverse selection

For the trader as a *provider* of liquidity:

1. **Active LP-ing on volatile AMM pairs is structurally negative-EV** without hedging — LVR exceeds fees
2. **Limit-order writing on illiquid CEX pairs** is similar — adverse selection is high
3. **The viable LP strategies** are: (a) stable-stable pools, (b) concentrated liquidity with active management, (c) LP-with-hedge, (d) very-high-fee tiers on volatile pairs
4. **The trader's a funding-carry strategy** is a dealer-style play — capturing the structural-funding component of spread on perps

## Connection to Existing Wiki Pages

This page extends:

- [[foundation/market-structure/trader-taxonomy]] — dealers are sell-side; this page operationalizes the dealer side
- [[foundation/market-structure/bid-ask-spread-decomposition]] — dealer's spread setting motivated by these two risks
- [[foundation/market-structure/zero-sum-game]] — dealers' net-positive expectancy comes from utilitarian flow and uninformed profit-motivated flow
- [[foundation/market-structure/order-flow-externality]] — dealers benefit from order flow (free options); pay for it via the externality cost on their inventory

## Key Takeaways

- Dealers profit by capturing the bid/ask spread, but face two risks: diversifiable inventory risk and non-diversifiable adverse selection risk
- Spread widening compensates for both risks
- Dealers learn value from order flow via Bayesian updating
- Profit sources: transaction cost capture (volume × spread) + information advantage + intentional inventory positioning
- Crypto MMs (Cumberland, GSR, Wintermute, etc.) profit on tight-spread × high-volume × low-information-asymmetry pairs
- AMM LPs are passive dealers — adverse selection (LVR) typically exceeds fee revenue on volatile pairs → structurally negative EV
- Profitable AMM LP strategies require: stable pairs, concentrated liquidity, or external hedging
- Crypto perp MM economics are favorable: tight spreads + high volume + funding-rate revenue
- The trader's a funding-carry strategy is dealer-side economics applied to perp funding — sustainable edge as long as retail-leveraged-long flow persists

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; a funding-carry strategy is funding-rate carrier, AMM LP economics is LVR carrier — both catalogued there as derivatives-microstructure features
- [[foundation/sources/harris-trading-exchanges]] — source
- [[foundation/market-structure/trader-taxonomy]] — dealer is a sell-side role
- [[foundation/market-structure/bid-ask-spread-decomposition]] — what dealers earn
- [[foundation/market-structure/zero-sum-game]] — dealer profits come from somewhere
- [[foundation/market-structure/order-flow-externality]] — dealers exploit; LPs pay
- [[foundation/market-structure/why-people-trade]] — dealer's profit pool is utilitarian + uninformed-profit-motivated counterparties

## Open Questions

- **What's the empirical Sharpe ratio of professional crypto perp market making?** Public data limited; estimated 5-15+ for top firms.
- **Can retail traders systematically operate dealer-style strategies in crypto?** a funding-carry strategy suggests yes for funding-carry; AMM LPing suggests no without sophistication.
- **Has the rise of MEV protection (CoW, Flashbots Protect) measurably reduced AMM LP adverse selection?** Hypothesis: yes; quantification useful.

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 13 ("Dealers"), Oxford University Press, 2003.
