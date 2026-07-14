---
title: "Trader Taxonomy — Buy Side, Sell Side, and Who Trades"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, taxonomy]
---

# Trader Taxonomy — Buy Side, Sell Side, and Who Trades

> **TL;DR**: Harris's classification of every trader in every market into a small number of structural roles. The critical distinction: "buy side" and "sell side" refer to *exchange services* (liquidity), not direction of trade. Both sides regularly buy and sell. The buy side *consumes* liquidity (pays the spread); the sell side *supplies* liquidity (captures the spread). Knowing which side you're operating on at any moment is foundational microstructure literacy.

## The Core Distinction — Buy Side vs. Sell Side

The terminology is famously confusing because it's about *services*, not direction:

| Side | What they buy/sell | What they want |
|------|----------------------|----------------|
| **Buy side** | Buys liquidity (consumes it) | To trade when they want to trade |
| **Sell side** | Sells liquidity (supplies it) | To profit from spread + commissions |

Both sides will buy or sell securities/instruments depending on the moment. The terminology is about *who's paying for the service*.

> "The buy and sell sides of the trading industry have nothing to do with whether a trader is a buyer or a seller of an instrument. Traders on both sides of the trading industry regularly buy and sell securities and contracts."
> — Harris, Ch 3.1

## The Buy Side

> "The buy side of the trading industry includes individuals, funds, firms, and governments that use the markets to help solve various problems they face. These problems typically originate outside of trading markets."
> — Harris, Ch 3.1.1

Subcategories:

| Subcategory | Examples | Why they trade |
|-------------|----------|----------------|
| **Individual investors** | Retail traders, day traders | Speculation, savings, retirement |
| **Investment sponsors** | Pension funds, mutual funds, trusts, endowments, foundations | Manage external capital |
| **Investment advisers** | RIAs, portfolio managers, hedge funds | Execute investment decisions |
| **Buy-side traders** | Institutional execution traders | Execute the adviser's orders efficiently |
| **Beneficiaries** | Retirees, fund holders | The ultimate principals (whose money it is) |
| **Hedgers** | Corporates managing FX, commodity, rate exposure | Risk transfer |
| **Borrowers** | Issuers raising capital | Solve cash-flow timing |
| **Asset exchangers** | Real estate-for-cash transactions | Convert one asset class to another |
| **Gamblers** | Pure speculators | Entertainment, action-seeking |

The buy side is heterogeneous — different sub-roles trade for different reasons. Some are utilitarian (hedgers, borrowers, asset exchangers); some are profit-motivated (active investors, gamblers). The mix matters because it determines how *informed* the average buy-side participant is at any moment.

## The Sell Side

> "The sell side of the trading industry includes dealers and brokers who provide exchange services to the buy side. Both types of traders help buy-side traders trade when they want to trade."
> — Harris, Ch 3.1.2

The sell side has two fundamentally different roles, often confused:

### Dealers (proprietary trading; principal trading)

> "Dealers accommodate trades that their clients want to make by trading with them when their clients want to trade. Dealers profit when they buy low and sell high."

A dealer takes the *other side* of client trades, holding inventory in between. Their profit comes from buying at the bid and selling at the ask — capturing the spread.

The dealer's risk: holding inventory while prices move against them. Their compensation: spread = inventory risk premium + adverse selection premium + processing cost.

### Brokers (agency trading; commission trading)

> "Brokers trade on behalf of their clients. Brokers arrange trades that their clients want to make by finding other traders who will trade with their clients. Brokers profit when their clients pay them commissions for arranging trades with other traders."

A broker doesn't take the other side. They find a counterparty. Their profit comes from commissions, not from the spread.

The broker's risk: their reputation depends on getting clients good fills. Their compensation: commission per trade.

### The Confusion: Broker-Dealers

> "Many sell-side firms employ traders who both deal and broker trades. These firms therefore are known as **broker-dealers** or **dual traders**."

Most modern sell-side firms are broker-dealers. Goldman Sachs, JPMorgan, Morgan Stanley — all act as both broker (executing client orders) and dealer (taking principal positions). The dual role creates the [[foundation/market-structure/microstructure-themes#8-principal-agent-problems|principal-agent problem]] (Harris Theme 8): the broker-dealer's interests as broker (best fill for client) often conflict with their interests as dealer (best fill for themselves).

## Long Positions vs. Short Positions

> "Traders have **long positions** when they own something. Traders with long positions profit when prices rise. They try to buy low and sell high.
>
> Traders have **short positions** when they have sold something that they do not own. Traders with short positions hope that prices will fall so they can repurchase at a lower price. When they repurchase, they cover their positions. Short sellers profit when they sell high and buy low."

Both directions are first-class. The asymmetric framing in retail trading culture ("longs are normal, shorts are exotic") is a cultural bias, not a microstructure reality. In FX markets, every long is automatically a short (long EUR/USD = long EUR + short USD). In crypto perps, every long is balanced by a short — perps don't exist without shorts.

## Crypto Translation

The Harris taxonomy maps cleanly to crypto with adjustments for venue type:

### Crypto buy side

| Harris category | Crypto equivalent |
|------------------|-------------------|
| Individual investors | Retail spot buyers; on-chain wallets |
| Investment sponsors | Crypto funds (Pantera, Multicoin, Paradigm); ETF issuers |
| Investment advisers | Crypto VCs, hedge funds |
| Buy-side traders | OTC desk client-side traders |
| Beneficiaries | LP token holders, ETF shareholders |
| Hedgers | Miners hedging BTC production via perps; corporates with crypto treasuries |
| Borrowers | Token issuers raising via ICOs/IDOs |
| Asset exchangers | Stablecoin → BTC swappers; cross-asset rotators |
| Gamblers | Memecoin traders; perp gamblers |

### Crypto sell side

| Harris category | Crypto equivalent |
|------------------|-------------------|
| Dealers | Centralized market makers (Cumberland, GSR, Wintermute, Jump); AMM LPs (passive dealers) |
| Brokers | OTC desks (when acting as agency); aggregators (1inch, ParaSwap, CoW) |
| Broker-dealers | Centralized exchanges (act as both — match orders + run their own market making); custody desks |
| Wirehouses (full-service) | Coinbase Prime, Galaxy, FalconX |

### Critical crypto-specific roles not in Harris

These exist in crypto but didn't in 2003 equity markets:

- **AMM Liquidity Providers** — passive dealers who deposit capital into AMM pools and earn fees. They are *uniquely systematic option-writers* (per [[foundation/market-structure/microstructure-themes#2-options|Theme 2]]) — they write free options to arbitrageurs on every price move.
- **MEV searchers / block builders** — extract value from transaction ordering. A novel role with no equity-market analog at this scale.
- **Validators / miners** — partially analog to specialists in equity markets, but with different incentive structure.
- **Bridge operators** — provide cross-chain liquidity; partial dealer + custodian + creditor.
- **Stablecoin issuers** — quasi-broker (settlement intermediary) + quasi-dealer (FX-style spread on mint/burn) + quasi-bank (reserve management).

These crypto-specific roles don't fit cleanly into Harris's buy/sell taxonomy, suggesting microstructure concepts beyond the 2003 framework.

## Why The Taxonomy Matters

For the trader's day/swing trading, knowing which side you're on at each moment determines:

1. **Whether you're paying or capturing the spread**
   - Market orders → buy-side, paying
   - Limit orders that rest → sell-side, capturing (when filled)
   - This single distinction explains why patient limit-order traders outperform impatient market-order traders on average

2. **Who your counterparty is**
   - When you submit a market order to a CEX, you're trading against a market maker (sell-side dealer)
   - When you submit to a DEX, you're trading against an AMM (passive dealer) or a JIT LP (active dealer)
   - The other side is *always* a sell-side actor when you cross the spread

3. **What information asymmetry you face**
   - Sell-side actors observe order flow; you don't
   - This is the fundamental asymmetry per [[foundation/market-structure/microstructure-themes#1-information-asymmetries|Theme 1]]

4. **What strategies are available to you**
   - As a buy-side trader, you can choose timing and venue
   - As a sell-side trader (e.g., providing liquidity on a DEX as an LP), you have different optimization problems
   - These are not the same job — confusing them is a common error

## Operational Test

Before every trade, ask:

1. Am I buy-side or sell-side at this moment?
2. If buy-side: am I paying for liquidity (market order) or willing to wait (limit order)?
3. If sell-side: am I a dealer (taking principal risk) or a broker (collecting commissions)?
4. Who's the other side? What do they know that I don't?
5. What's my edge in this trade given the answer to (4)?

A trader who can't articulate the answers is operating without microstructure literacy and is likely the systematic loser per [[foundation/market-structure/microstructure-themes#10-the-zero-sum-game|Theme 10]].

## Key Takeaways

- "Buy side" / "sell side" refers to *liquidity services*, not trade direction
- Buy side consumes liquidity (pays spread); sell side provides liquidity (captures spread)
- Dealers take principal risk + earn spread; brokers find counterparty + earn commission
- Most large firms are broker-dealers — creating principal-agent problems
- Crypto has the same buy/sell taxonomy + new roles (AMM LPs, MEV searchers, bridge operators) without equity-market analogs
- Knowing your role at each trade moment is foundational microstructure literacy

## Related

- [[foundation/sources/harris-trading-exchanges]] — source
- [[foundation/market-structure/microstructure-themes]] — themes 1, 2, 3, 8, 10 all interact with this taxonomy
- [[foundation/market-structure/order-flow-externality]] — sell-side firms compete for buy-side order flow
- [[foundation/market-structure/zero-sum-game]] — the buy-side / sell-side framing is the easiest way to identify "who profits"
- [[foundation/asset-classes/forex]] — FX market has the cleanest broker-dealer taxonomy in retail markets

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 3.1 ("Who Are the Players?"), Oxford University Press, 2003.
