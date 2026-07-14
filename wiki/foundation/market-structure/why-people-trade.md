---
title: "Why People Trade — Utilitarian vs. Profit-Motivated"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, motivation, zero-sum]
---

# Why People Trade — Utilitarian vs. Profit-Motivated

> **TL;DR**: Harris's framework for understanding *who's on the other side of any trade*. Two categories of traders: **utilitarian** (trade to solve an external problem — hedging, time-shifting cash flow, raising capital, asset-class conversion) and **profit-motivated** (trade to make money from trading itself). Utilitarian traders are *willing losers* — they expect to pay for liquidity. Profit-motivated trading is zero-sum among the profit-motivated. Most retail "investors" are profit-motivated traders without realizing it. The framework directly extends [[foundation/market-structure/zero-sum-game]] by identifying which trades have a "willing loser" on the other side.

## The Core Distinction

| Type | Why they trade | What they're willing to pay |
|------|---------------|----------------------------|
| **Utilitarian** | Solve a non-trading problem (hedge, raise capital, transfer risk, time-shift cash) | Some "premium" for liquidity — they need to trade *now*, not when prices are best |
| **Profit-motivated** | Make money from the trading itself | Nothing — they expect to extract value, not pay for liquidity |

The mix of these two categories at any moment determines:
- How much *informed* trading is happening (mostly profit-motivated)
- How wide the spread can be without driving away utilitarian flow
- Whether the venue is "easy" or "hard" to make money on

## Utilitarian Trading — The Willing Losers

Utilitarian traders have problems originating outside the trading market. Trading is a *means*, not the end. Examples:

### Investment / saving / retirement

Most pension funds, mutual funds, retail savings flows. Trade to move purchasing power across time. They don't expect to beat the market; they need to *be in* the market. Willing to pay liquidity premium.

### Borrowing / capital raising

Companies issuing stock or bonds; governments issuing treasuries. Trade because they need capital. Willing to underprice the issuance to ensure it sells.

### Asset exchange

Real estate transactions, business sales, commodity producers selling output. Trade to convert one type of asset (or claim) to another. Willing to pay liquidity premium for size and timing.

### Hedging

Corporates hedging FX or commodity exposure; airlines hedging fuel; banks hedging interest-rate exposure; miners hedging gold/copper production. Trade to *transfer risk*. Willing to pay a hedging premium because the alternative (unhedged exposure) is worse.

### Indexing / mechanical rebalancing

Index funds rebalancing on schedule. Trade because the rules require it, not because prices are favorable. Willing losers in the short-term sense; their "loss" is the cost of being mechanically diversified.

### Liquidating

Forced sellers — bankruptcy, redemption, regulatory deadline, liquidation of estates. Willing losers because they *must* exit by a date. Their "loss" is the discount they accept for time-pressure.

### Gambling / entertainment

Some retail traders trade for the experience, the emotional engagement, the social signal. They're willing to lose because the *trading experience* has utility independent of profit. Casinos profit from this category; retail brokers attract it.

## Profit-Motivated Trading — The Zero-Sum Crowd

Profit-motivated traders trade *because they expect to make money on the trade itself*. The category includes:

### Informed traders

Have superior information about value or upcoming order flow. Profit by trading *before* the information becomes broadly known.

### Arbitrageurs

Profit by trading temporary price disparities (across venues, across instruments, across time).

### Dealers / market makers

Profit by capturing the spread on trades they intermediate. Their profit comes from utilitarian flow; their losses come from informed flow.

### Front-runners and order anticipators

Profit by trading ahead of expected order flow. Per [[foundation/market-structure/market-manipulation-taxonomy|manipulation taxonomy]].

### Trend-followers / momentum traders

Profit (when they do) by trading on persistence of price moves driven by either informed flow or behavioral momentum.

### Mean-reversion traders

Profit (when they do) by trading on temporary mispricing reversion.

### Speculators (Lynch's amateur traders)

Trade on opinions about value, often without rigorous analysis. Some make money; most don't (per [[foundation/market-structure/zero-sum-game]] zero-sum dynamics).

## The Zero-Sum Among Profit-Motivated

Within the profit-motivated category alone, trading is **strictly zero-sum** at the gross level (negative-sum after fees). Every dollar that one profit-motivated trader makes is a dollar that another profit-motivated trader loses (or that a utilitarian trader paid as liquidity premium).

This is the *deepest framing* of why most retail traders lose:

1. Most retail traders self-classify as "investors" but operate as profit-motivated traders (timing trades for short-term profit)
2. They face informed traders, arbitrageurs, dealers, market makers — who collectively are profit-positive
3. The math requires *some* profit-motivated traders to be net losers
4. Retail is consistently the largest category of profit-motivated traders without information advantage
5. Therefore retail systematically loses to the rest of the profit-motivated population

## The Utilitarian-Profit-Motivated Mix

Different markets have different mixes:

| Market | Mostly utilitarian | Mostly profit-motivated |
|--------|---------------------|--------------------------|
| Treasury bonds | Yes (banks managing duration; pensions) | Smaller |
| Spot FX major pairs | Mixed | Mixed |
| Equity markets | Mixed (retail/index = utilitarian; HFT = profit-motivated) | |
| Equity options | Smaller | Yes (mostly speculation/hedging) |
| Crypto perps | Almost none | **Almost entirely profit-motivated** |
| Memecoin tokens | None | **Almost entirely** |
| Commodity futures (large) | Yes (producers/consumers hedging) | Mixed |

**The implication for crypto**: most crypto markets — especially perps and small-caps — are *almost entirely profit-motivated*. Almost no one *needs* to trade BTC perps for utilitarian reasons (a few miners hedging production; small). This means:

- Spreads must be very tight (no utilitarian flow to extract from)
- Profit-motivated traders are zero-sum against each other
- The "easy money" is harvested by faster, more-informed, better-resourced participants
- Retail is the largest pool of profit-motivated-without-edge traders

This is why retail loses dramatically more on crypto perps than on equities — the *category mix* is structurally hostile.

## Where Utilitarian Flow Exists in Crypto

There is *some* utilitarian flow in crypto, mostly in spot:

- **Spot BTC accumulation** by long-term holders treating BTC as savings ≈ utilitarian (time-shifting purchasing power)
- **Stablecoin treasury operations** by corporates ≈ utilitarian (operating-cash-flow management)
- **OTC desk flow from miners selling production** ≈ utilitarian (asset exchange)
- **ETF flow** ≈ utilitarian (mostly retirement/savings allocations)
- **Hedging by mining companies** ≈ utilitarian
- **Stablecoin redemption** by treasuries ≈ utilitarian

Most of this flow goes through OTC desks, ETFs, and regulated venues — not through retail-accessible perp markets. The trader's day/swing trading on perps therefore faces an almost-pure profit-motivated counterparty pool.

## Implications for Strategy Design

### What this framework forces

Before any strategy, articulate the **counterparty category**:

- "I'm trading against a forced seller / liquidator" → utilitarian counterparty; sustainable edge
- "I'm trading against a hedger paying for protection" → utilitarian counterparty; sustainable edge (this is the carry trade thesis)
- "I'm trading against indexers rebalancing on schedule" → utilitarian counterparty; sustainable edge
- "I'm trading against retail momentum chasers" → profit-motivated but lower-skilled counterparty; sustainable but eroding edge as retail gets smarter
- "I'm trading against equally-skilled professionals" → zero-sum war of attrition; unsustainable in expectation
- "I'm trading against more-informed professionals" → I'm the loser

### The "edge" question

A trader has edge if and only if their counterparty pool includes one of:

1. **Willing utilitarian losers** (carry trades, hedger-counterparty strategies)
2. **Less-informed profit-motivated traders** (front-running of patterns retail traders adopt)
3. **Regulatory or structural arbitrage opportunities** (cross-venue, cross-jurisdiction)
4. **Information advantage** that hasn't been priced in (insider info, on-chain analysis advantage, faster news interpretation)

Without one of these, "edge" is illusory — the strategy is zero-sum against equally-equipped competitors.

### Connection to existing a funding-carry strategy

The trader's existing a funding-carry strategy / funding-rate carry strategy (per the wiki architecture) explicitly targets utilitarian counterparties:

- Persistent positive funding = retail longs are paying funding to hold leveraged exposure
- Retail leverage longs are not utilitarian — they're profit-motivated speculators
- BUT — there's a structural willing-loser dynamic: leveraged longs are paying for the *option* to hold the position (a kind of self-imposed utilitarianism)
- The carry captures this funding flow

This is a hybrid of utilitarian-flow capture and exploiting profit-motivated-but-less-informed traders. Per Harris's framework, this is sustainable because the counterparty pool (retail leveraged longs) is structurally re-supplied as new traders enter the market.

## Connection to Existing Wiki Pages

This page operationalizes:

- [[foundation/market-structure/zero-sum-game]] — the loser-identification discipline; this page categorizes the loser types
- [[foundation/market-structure/trader-taxonomy]] — buy-side / sell-side describes services; utilitarian / profit-motivated describes motivations
- [[foundation/market-structure/microstructure-themes]] — Theme 10 (zero-sum) operationalized

Connects to:

- [[trading/mechanisms/cyclical-timing]] — cycle troughs concentrate forced sellers (utilitarian-loser flow); cycle peaks concentrate retail FOMO buyers (profit-motivated-without-edge flow)
- [[foundation/market-wisdom/institutional-imperative]] — Buffett's institutional imperative produces *willing losers* in institutional flow; aligns with utilitarian framing
- [[foundation/market-wisdom/lynch-six-categories]] — Lynch's "asset plays" are often valuations driven by utilitarian indexer flow; "fast growers" exit when pattern-followers crowd in
- [[foundation/idea-sourcing-methodology]] — methodology should ask "who's the willing loser, or which less-informed profit-motivated traders are systematically losing?" before any candidate is approved

## Key Takeaways

- Two categories of traders: utilitarian (trade to solve external problem) and profit-motivated (trade to make money from trading)
- Utilitarian traders are *willing losers* — they expect to pay liquidity premium for what trading does for them
- Profit-motivated trading is zero-sum among the profit-motivated category
- Most retail "investors" operate as profit-motivated traders without realizing it
- Crypto perps and small-caps are almost entirely profit-motivated — no utilitarian flow to extract from
- Sustainable edge requires either utilitarian counterparties OR less-informed profit-motivated counterparties OR structural advantage OR information advantage
- The trader's a funding-carry strategy exploits a hybrid: leveraged longs as quasi-utilitarian (paying for position-holding option) + structural funding mechanism

## Related

- [[foundation/sources/harris-trading-exchanges]] — source
- [[foundation/market-structure/zero-sum-game]] — operationalized here
- [[foundation/market-structure/trader-taxonomy]] — services-based vs motivation-based taxonomy
- [[foundation/market-structure/bid-ask-spread-decomposition]] — utilitarian flow funds dealers' adverse-selection losses to informed flow
- [[foundation/market-wisdom/institutional-imperative]] — institutional flow includes willing-loser patterns
- [[trading/mechanisms/cyclical-timing]] — cycle dynamics concentrate different counterparty types at different phases
- [[foundation/idea-sourcing-methodology]] — counterparty identification should be a sourcing step

## Open Questions

- **What fraction of crypto perp volume is utilitarian (miner hedging, treasury operations)?** Probably <2-5%; most is profit-motivated retail and prop.
- **Has the rise of crypto ETFs increased utilitarian flow proportions?** Anecdotally yes; quantification would be useful.
- **Can we systematically identify which trades have utilitarian counterparties?** Some signatures: end-of-day flow, fixed-time rebalances, post-event flow. Tradable.

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 8 ("Why People Trade"), Oxford University Press, 2003.
