---
title: "Bubbles, Crashes, and Circuit Breakers"
status: growing
created: 2026-04-30
updated: 2026-04-30
tags: [foundation, market-structure, harris, microstructure, crashes, bubbles]
---

# Bubbles, Crashes, and Circuit Breakers

> **TL;DR**: Harris's Ch 28 catalogs the canonical equity-market crashes (1929, 1987, etc.) and their microstructure causes. The most-instructive lesson: the 1987 crash was caused by **portfolio insurance** — a dynamic hedging strategy that mechanically required selling more when prices fell. The crypto translation is direct: stop-loss cascades, leveraged-perp liquidation cascades, and AMM LVR amplification under stress all exhibit the same dynamic-trading-strategy-as-self-reinforcing-cause pattern. Crypto's flash crashes are 1987's portfolio-insurance crash structurally.

## Harris's Crash Catalog

Harris reviews seven bubbles/crashes; the 1929 and 1987 events are the most instructive for crypto:

### 1929 — The Speculative-Margin Crash

- DJIA dropped 25% in two days (Oct 28-29); ultimately 89% from peak by 1932
- Cause: excessive use of margin (broker loans) to buy stocks
- Speculators borrowed heavily to participate in the late-1920s tech narrative (radio, RCA, etc.)
- When prices fell, margin calls forced selling at any price
- The forced-selling cascade compounded the decline

**Crypto application** — directly analogous:
- Excessive use of perp leverage (often 10x-50x retail; 100x+ available)
- When prices fall, automatic liquidations force selling regardless of price
- Liquidation cascades compound declines (May 2021, June 2022, March 2023)
- Coinglass-visible liquidation maps make these cascades *predictable in direction*

The 1929 mechanism is *literally* the crypto liquidation cascade mechanism, just executed via different infrastructure (broker loans → exchange margin requirements → perp liquidation engines).

### 1987 — The Portfolio Insurance Crash

The most-important crash for crypto students. The cause:

> "Portfolio insurance is a dynamic trading strategy that portfolio managers use to replicate the combined returns of a portfolio plus a put option... Portfolio insurance is highly destabilizing to market prices. When prices rise, portfolio insurers must buy stock. When prices fall, they must sell stock. Portfolio insurance therefore has the same effect on the market as stop orders."
> — Harris, Ch 28.2.2

The mechanism:

1. Institutional managers used Black-Scholes-derived formulas to "synthesize" a put option via dynamic trading
2. The formula said: as prices fall, sell more stock to maintain the synthetic protection
3. By 1987, ~$100B was managed with portfolio insurance — substantial fraction of the market
4. When prices started falling (already 17% from August peak by Friday before the crash), portfolio insurers began executing their formulas
5. Their selling pushed prices further down
6. Lower prices triggered more selling per the formula
7. Order anticipators front-ran the predictable insurance flow
8. Other traders pulled buy-side liquidity, expecting the cascade
9. Result: 23% drop in one day on Black Monday

> "When sellers want to sell large quantities at the same time, prices can fall very substantially. Moreover, when everyone knows that large sellers will come onto the market when prices fall, order anticipators will quickly sell at the first sign of a price drop. Likewise, buyers will withdraw from the market until prices have dropped substantially. Expectations about portfolio insurance selling therefore can drive the market down, even if no portfolio insurance selling occurs."

The deepest insight: **expectations of cascading selling are themselves cascading-selling-causing**. Even before the formula-driven sell orders arrive, traders anticipate them and exit, creating the very price decline the formulas were trying to protect against.

## The Universal Crash Pattern

From these and other crashes Harris documents, a universal pattern emerges:

```
1. Prior period of price appreciation (often justified by fundamentals plus narrative)
2. Build-up of correlated leveraged positions (margin / portfolio insurance / perp leverage)
3. Trigger event — usually ambiguous, often macro-news (1987: trade deficit + Iran; 1929: rate hikes)
4. Initial price decline triggers the leveraged-position-unwind mechanism
5. Mechanism-driven selling pushes prices down faster
6. Order anticipators front-run the predictable mechanism flow
7. Buy-side liquidity withdraws (everyone waits for "when sellers are done")
8. Cascade — price drops far below fundamental value
9. Eventually, value buyers / new flow stabilizes the market
10. Recovery — sometimes quickly (1987 recovered fully within 2 years), sometimes slowly (1929 took 25 years)
```

The cascade is *not* primarily about fundamental value — it's about microstructure dynamics during forced unwinds.

## Crypto Crash Map

Every major crypto crash exhibits the universal pattern:

### May 19, 2021 BTC Flash Crash (~30% in hours)
- Trigger: Elon Musk environmental tweet + China mining ban news
- Mechanism: long perp liquidations cascading
- Compounding: leveraged-fund unwinds (3AC, others); MEV bots front-running liquidations
- Recovery: partial within days; full BTC ATH took 4-5 months

### June 2022 — UST/Luna collapse + 3AC
- Trigger: UST de-peg in Anchor protocol drainage
- Mechanism: stablecoin death spiral + leveraged-fund insolvency cascade
- Compounding: Celsius freeze, BlockFi, Voyager bankruptcies
- Recovery: extended bear market

### March 10-11, 2023 — USDC de-peg
- Trigger: Silicon Valley Bank failure (USDC reserves at SVB)
- Mechanism: USDC briefly de-pegged to $0.87
- Cascade: DEX-trading-USDC LPs faced extreme adverse selection; CEX traders panic-rotated
- Recovery: USDC re-pegged within 48h after Treasury action; a relatively contained event

### November 2022 — FTX collapse
- Different category — settlement / custody failure rather than market crash
- But still exhibits cascade dynamics: bank run on FTX, contagion to BlockFi, Genesis, others
- Demonstrates [[foundation/market-structure/microstructure-themes#9-trustworthiness-and-creditworthiness|Theme 9]] (creditworthiness) as load-bearing in crypto

## Circuit Breakers — The Regulatory Response

After the 1987 crash, U.S. equity markets implemented **circuit breakers** — automatic trading halts triggered by large price declines:

- 7% one-day decline → 15-minute trading halt
- 13% one-day decline → 15-minute trading halt
- 20% one-day decline → trading halts for the rest of the day

The intent: provide pause time for participants to evaluate fundamentals rather than panic-trade. Critics argue circuit breakers create their own problems (exits become unavailable; the implicit guarantee of halt creates moral hazard for leverage).

**Crypto equivalent**: most crypto venues have *no* circuit breakers. Crypto is uniquely vulnerable to cascade dynamics because:
- 24/7 trading (no overnight cooling-off)
- Cross-venue arbitrage failure during stress (each venue can crash independently)
- Liquidation engines designed to prioritize exchange solvency over orderly markdowns
- No "halt and reset" mechanism

Some venues (Hyperliquid, dYdX) have implemented mark-price systems and oracle-based liquidations to reduce cascade severity, but no crypto venue has equity-style halts.

This is structurally important — **crypto's lack of circuit breakers means cascade dynamics play out fully** rather than getting interrupted by regulatory pause.

## Implications for Crypto Day/Swing Traders

The framework has direct operational implications:

### 1. Recognize the cascade setup before it happens

Pre-cascade signatures (from the 1987 analog):
- Sustained price appreciation (months) creating complacency
- Build-up of leveraged positions (high open interest; persistent positive funding)
- Concentrated stop-loss / liquidation clusters at predictable levels (visible on Coinglass)
- Reduced "buy-the-dip" eagerness (traders waiting to time the bottom rather than accumulating)

When all four are present, cascade probability is elevated.

### 2. Position-size for cascade survival

The 1987 crash was 23% in one day. Crypto cascades can be 30-50%+ in hours. Position sizing must allow for these scenarios:

- Don't run leverage that gets liquidated by 1.5x normal daily move
- Keep some allocation in cash/stables to *buy* during cascades (Lynch's "buy the crash" framework)
- Monitor liquidation maps for your own exposure

### 3. Don't trade during the cascade itself

During cascade execution, liquidity is essentially absent (per [[foundation/market-structure/liquidity|liquidity page]] — resilience collapses). Effective spreads can be 10-50x normal. Entries and exits during cascade are catastrophically expensive.

The discipline: have plans set *before* cascades, execute *after* (at least 30-60 minutes after maximum dislocation).

### 4. The recovery edge

Harris notes 1987 recovered fully within 2 years. 1929 took 25 years. The difference: 1987 was structural (microstructure-driven); 1929 was fundamental (real economic depression underway).

Crypto cascades that are microstructure-driven (May 2021, March 2023) tend to recover quickly. Cascades that are fundamental (June 2022 UST collapse triggering 3AC bankruptcy) take longer.

The post-cascade trade is usually a high-probability long, *if* the cause was microstructure rather than fundamental insolvency. This is one of the most-tradable patterns in crypto — and exactly the [[trading/mechanisms/cyclical-timing|cyclical timing]] mechanism applied to flash crashes.

## Hypothesis (Structured Format)

```
Hypothesis:    Crypto perp liquidation cascades exhibit predictable directional
               recovery dynamics. After a cascade with magnitude >X% in <Y hours,
               and with no concurrent fundamental insolvency news, BTC recovers
               80%+ of the cascade decline within 7 days.

Mechanism:     Per Harris 1987 analysis: cascades driven by leverage-unwind
               mechanics (not fundamentals) overshoot fundamental value
               because (1) order anticipators amplify the move and (2) buy-side
               liquidity withdraws expecting more selling. Once mechanism flow
               exhausts, value traders re-enter and price re-rates toward
               pre-cascade fundamental level.

Observable:    Cascade event (e.g., -10%+ in 4h with funding flip from positive
               to extreme negative); concurrent fundamental news classification
               (microstructure vs fundamental); 7-day post-cascade BTC return.

Expected:      In microstructure-only cascades, 80%+ recovery within 7 days
               with high probability (>70%). In fundamental cascades (UST,
               FTX), recovery is slower or non-occurring.

Falsifier:     If post-cascade returns are random direction, OR if
               microstructure vs fundamental classification doesn't predict
               recovery speed, the framework doesn't apply to crypto cascades.
```

This is one of the most-tradable hypotheses the wiki has surfaced. It maps directly to the trader's day/swing horizon (≤1 week) and combines multiple existing wiki concepts ([[trading/mechanisms/cyclical-timing|cyclical timing]], [[foundation/market-structure/market-manipulation-taxonomy|squeezer-cascade dynamics]], [[foundation/market-structure/liquidity|liquidity collapse]]).

## Connection to Existing Wiki Pages

This page integrates with:

- [[foundation/market-structure/market-manipulation-taxonomy]] — squeezer category (liquidation cascades) is the crypto manifestation; the 1987 crash is the historical equity manifestation
- [[foundation/market-structure/liquidity]] — liquidity collapses during cascades; resilience dimension fails first
- [[foundation/market-structure/why-people-trade]] — during cascades, the counterparty pool is forced sellers (utilitarian-by-coercion); buying from forced sellers has clear edge
- [[trading/mechanisms/cyclical-timing]] — Lynch's cyclical timing applies to flash-crash recoveries
- [[foundation/market-wisdom/the-new-speculation]] — Graham's framing: bubbles built on high-quality-stocks-at-high-prices is exactly the 1987 setup (and the 2021 crypto bull setup)
- the wiki architecture (ARCHITECTURE.md) Section 1 — crypto's pre-regulation character means circuit breakers don't exist; cascade dynamics play out fully

## Key Takeaways

- Crashes follow universal pattern: appreciation + leveraged buildup + trigger + mechanism unwind + cascade + recovery
- 1987 crash teaches the most for crypto: dynamic-trading-strategy (portfolio insurance) was the cause, not fundamentals
- Crypto liquidation cascades are *literally* the 1987 portfolio-insurance dynamic on different infrastructure
- Order anticipators systematically amplify cascades (front-running the predictable forced-selling)
- Buy-side liquidity withdraws *expectation* of cascade-selling — the expectation itself causes price decline
- Crypto lacks circuit breakers; cascades play out fully rather than getting interrupted
- Recovery depends on cause: microstructure cascades recover quickly (1987-style); fundamental cascades slowly (1929-style; UST 2022)
- The post-microstructure-cascade recovery trade is one of the most-tradable patterns in crypto

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of crypto-native carriers; cascade dynamics rely directly on liquidation-engine visibility (Coinglass), funding-rate extremes, and OI buildup — all carrier features catalogued there
- [[foundation/sources/harris-trading-exchanges]] — source for the *microstructure* layer of cascade dynamics (1987 portfolio insurance)
- [[foundation/sources/kindleberger-manias-panics-crashes]] — source for the *credit-cycle* layer of bubble dynamics. Harris and Kindleberger compose: Harris tells you the *mechanics* of a cascade once it starts; Kindleberger tells you *which stage* you're in (and thus whether a cascade is coming).
- [[foundation/market-wisdom/bubble-cycle-stages]] — the 5-stage Kindleberger framework that contextualizes when crashes occur (Stage 5 panic). Liquidation cascades are the panic-stage dynamic; the bubble-stage framework tells you when to expect that dynamic.
- [[foundation/market-wisdom/minsky-three-finance]] — Ponzi-finance composition shifts pro-cyclically; the system's *pre-crash* fragility is measured by Ponzi share, not by the trigger event itself
- [[foundation/macro/crypto-2020-2022-cycle]] — worked example of FTX cascade in full bubble-cycle context
- [[foundation/market-structure/market-manipulation-taxonomy]] — squeezer category = liquidation cascades = crypto's 1987
- [[foundation/market-structure/liquidity]] — resilience dimension fails during cascades
- [[foundation/market-structure/why-people-trade]] — cascade-trapped sellers are forced (utilitarian-by-coercion)
- [[trading/mechanisms/cyclical-timing]] — post-cascade recovery is a cyclical-timing trade
- [[foundation/market-wisdom/the-new-speculation]] — Graham's bubble framework
- [[foundation/market-structure/microstructure-themes]] — Theme 9 (creditworthiness) explains FTX-style fundamental cascades

## Open Questions

- **What's the empirical post-cascade recovery rate for crypto microstructure-only cascades?** The May 2021 and March 2023 cases support the 80%+ recovery hypothesis; rigorous backtesting would strengthen.
- **Can microstructure vs fundamental cascade classification be done in real-time?** Heuristic: if the news is about exchange solvency / insolvency, fundamental; if news is macro / event-driven, microstructure. Verification needed.
- **Does the recent introduction of mark-price-based liquidations on some venues (Hyperliquid) measurably reduce cascade severity?** Anecdotally yes; quantification useful.

## Sources

- Harris, Larry. *Trading and Exchanges*, Ch 28 ("Bubbles, Crashes, and Circuit Breakers"), Oxford University Press, 2003.
