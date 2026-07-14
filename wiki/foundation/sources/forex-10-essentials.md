---
title: "The 10 Essentials of Forex Trading — Jared Martinez"
status: seedling
created: 2026-04-30
updated: 2026-04-30
tags: [source, forex, retail-trading, tactical, equity-management]
---

# The 10 Essentials of Forex Trading — Jared Martinez

> **TL;DR**: Tactical retail FX trading manual by Jared Martinez (founder, Market Traders Institute). Mixed quality — most TA content overlaps with [[foundation/sources/murphy-technical-analysis|Murphy]], the motivational tone consumes meaningful page count, and the book partly functions as a funnel for the author's paid education programs. **Two genuinely useful contributions for the wiki**: (1) a concrete equity-management framework filling the [[foundation/relations-and-themes|wiki's risk-management gap]], and (2) a more practical Fibonacci-retracement treatment than Murphy's brief coverage. Not a foundational source; a tactical-rules supplement.

## Source Details

| Field | Value |
|-------|-------|
| **Author** | Jared F. Martinez (FXChief, founder of Market Traders Institute) |
| **Edition** | McGraw-Hill, 2007 |
| **Pages** | 236 |
| **Original Market** | FX (currency pairs, retail FX brokers in 2007 era) |
| **Mechanism Tags** | Risk management / equity sizing, Fibonacci retracement, candlestick + S/R confluence (overlaps Murphy) |
| **Behavioral Driver** | Retail-trader emotional discipline (the book's main psychological focus) |
| **Crypto Applicability** | Mutation needed — risk-management rules transfer cleanly; FX-specific mechanics (currency pair logic, sessions, spreads) translate as crypto-specific layer; Fibonacci retracements apply universally |

## Position in Lineage

See [[foundation/sources/lineage]].

**This source builds on:**
- ✅ [[foundation/sources/murphy-technical-analysis]] — extensively. Most TA content (candlesticks, S/R, trends, Fibonacci, fundamental analysis) is reformulated Murphy material at a more retail-friendly level
- ☐ Edwards & Magee → Murphy → this — the chart-pattern lineage continues here at a more applied level

**This source contributes:**
- A specific, mechanical **equity management framework** that no prior wiki source provided in detail
- A more thorough **Fibonacci retracement** treatment than Murphy's brief coverage
- **FX-specific market context** — sessions, currency pair conventions, spread/leverage mechanics

**This source does NOT meaningfully contribute:**
- New chart patterns (already in Murphy)
- Novel TA premises (Murphy's three premises are foundational)
- Fundamental analysis (Chapter 10 is a basic economic-indicators primer, less rigorous than [[foundation/sources/the-intelligent-investor|Graham]] or [[foundation/sources/buffett-essays|Buffett]])
- Behavioral / pre-regulation insights (lineage's top priorities — Lefèvre, Wyckoff — remain unfilled)

## Honest Quality Assessment

The wiki has been built around source quality. Being explicit about this source:

**What's good:**
- The equity-management chapter (Ch 12) provides concrete rules: risk per trade as % of margin, minimum 1:1 risk:reward (target 1:1.5), "percentages mean nothing" insight (a 30% win rate with 6:1 R/R can still be net positive)
- The Fibonacci retracement treatment (Ch 8–9) is more applied and less abstract than Murphy's
- The author's tone is approachable for amateurs

**What's weak:**
- Heavy motivational padding ("Two Wolves," "Find your pot of gold," anecdotal stories about losing traders) consumes ~30-40% of the page count
- Many "rules" are pattern-recognition without rigorous backtesting or falsifiable formulation
- The book functions partly as a funnel for Market Traders Institute paid programs (forex.com, markettraders.com)
- Some claims are stated as universal truths but are clearly retail-FX-2007-specific (broker mechanics, leverage rules)
- Limited treatment of regime-dependence — mechanisms are presented as if they always work

**Implication for wiki**: extract the concrete tactical rules (equity management, Fibonacci, FX market context) but don't elevate the book's framing to foundational-source status.

## Genuine Contributions

### 1. The Equity Management Framework (Ch 12)

> "Great traders are only right 50 to 60 percent of the time. They make lots of money because they keep their losses small and gains big."

Concrete rules extracted:

1. **Risk per trade ≤ 5% of trading margin** (Martinez prefers 3% or less for sustainability)
2. **Minimum 1:1 risk-to-reward** — never enter a trade where potential loss > potential gain
3. **Target 1:1.5 risk-to-reward** as the *habit* — for every $X risked, plan to make $1.5X
4. **Number-of-losses-you-can-endure** as the safety metric (a 5% margin risk on a $15K account = 30 losses possible before account exhaustion; a 50% risk = 1 loss)
5. **Quantify potential loss BEFORE entering** — attach a dollar amount and a deadline; whichever hits first, exit
6. **The win-rate inversion**: winning 30% of the time can still be profitable if your average win is much larger than your average loss

The "percentages mean nothing" framing is arguably the book's single most-load-bearing insight: traders fixate on win rate, but **win rate × average win − loss rate × average loss = expectancy** is what actually matters.

This translates directly into [[investment/allocation/equity-management-rules]] (the new wiki page).

### 2. Fibonacci Retracement Trading (Ch 8–9)

The Fibonacci numerical sequence (1, 1, 2, 3, 5, 8, 13, 21...) generates ratios (61.8%, 38.2%, etc.) that empirically often appear at retracement levels in trending markets.

Standard retracement levels:
- 38.2%
- 50% (technically not Fibonacci but commonly used)
- 61.8%
- 78.6%

Standard extension levels (after retracement, projecting target):
- 1.27
- 1.618
- 2.618

Martinez's contribution beyond Murphy: a more mechanical entry framework using Fibonacci confluence with support/resistance and candlestick patterns.

Honest caveat: Fibonacci ratios working in markets is well-documented as a **self-fulfilling prophecy** — enough traders draw the same retracement levels that the levels become real support/resistance through coordinated buying/selling. Whether the underlying mathematical claim ("markets retrace at golden-ratio levels because of natural mathematical structure") is true is a separate, less-defensible question.

This translates into [[trading/mechanisms/fibonacci-retracements]] with appropriate caveats.

### 3. FX Market Context (Ch 2)

The retail FX market has structural features the wiki hadn't documented:
- 24-hour trading split into Asia / London / NY sessions, each with different volatility and liquidity profiles
- Currency pair conventions (base/quote currency, "going long EUR/USD" = long EUR + short USD)
- Major pairs (EUR/USD, GBP/USD, USD/JPY, USD/CHF) vs. minors and exotics
- Leverage / margin mechanics (50:1 to 500:1 leverage common in 2007; now restricted in many jurisdictions)
- Spread-only execution — no commissions on retail FX (the broker takes the spread)

This translates into [[foundation/asset-classes/forex]] (a new page, fills the empty asset-classes section).

**Crypto perp connection**: retail crypto perp markets share many of these features (24-hour, high leverage, spread + funding rate, session dynamics). The FX framing is a useful precursor.

## Crypto Adaptation Notes

The book's tactical content translates with adjustment:

| Forex framework | Crypto application |
|-----------------|---------------------|
| 5% max risk per trade | Same; arguably tighter (3-5%) for crypto's higher volatility |
| 1:1.5 risk:reward minimum | Same |
| FX session boundaries | Crypto has weaker sessions but still has Asia / EU / US flow patterns |
| Currency pair conventions | Crypto pair conventions (BTC/USD, ETH/BTC) — easier than FX (no quote-currency complexity) |
| Fibonacci on FX charts | Same on crypto charts; crypto traders use Fibonacci heavily |
| Leverage 50:1 - 500:1 | Crypto perps offer 100x typical, up to 1000x at some venues — even higher amplification of losses |

The risk-management rules transfer most cleanly. The Fibonacci levels transfer (with the same self-fulfilling-prophecy caveat). The FX-specific mechanics translate as crypto-specific mechanics on perps.

## Pages Extracted From This Source

| Page | Topic | Branch |
|------|-------|--------|
| [[investment/allocation/equity-management-rules]] | Risk per trade, R:R ratio, win-rate inversion | Investment/Allocation (cross-applicable to Trading) |
| [[trading/mechanisms/fibonacci-retracements]] | Fib retracement framework with empirical caveats | Trading |
| [[foundation/asset-classes/forex]] | FX market context as asset-class reference | Foundation/Asset Classes |

(Candlestick / chart-pattern / fundamental-analysis chapters not separately extracted — covered in [[trading/mechanisms/chart-patterns]] from Murphy.)

## What This Source Does NOT Belong In

- **Foundation/market-wisdom** — the book's "wisdom" content is motivational rather than structural
- **Trading/mechanisms (extensively)** — most "mechanisms" duplicate Murphy with weaker rigor
- **Foundation/sources/lineage as a primary node** — added as a tactical node off Murphy, not as a primary lineage source

## Sources

- Martinez, Jared F. *The 10 Essentials of Forex Trading: The Rules for Turning Patterns into Profit*, McGraw-Hill, 2007.
- Full text extracted via pdf-streamer: `raw/books/forex-10-essentials.workdir/output.md` (2,482 lines, 418 KB)

---

*Lower-priority ingest than the lineage page's top targets (Lefèvre, Wyckoff). Useful for filling the equity-management gap and modest Fibonacci depth, not for foundational mechanism work.*
