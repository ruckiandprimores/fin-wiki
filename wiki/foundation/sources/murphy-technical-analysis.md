---
title: "Technical Analysis of the Financial Markets — John J. Murphy"
status: growing
created: 2026-04-29
updated: 2026-04-29
tags: [source, technical-analysis, trading, day-swing, classics]
---

# Technical Analysis of the Financial Markets — John J. Murphy

> **TL;DR**: The single standard reference of modern technical analysis. Murphy's 1999 update of his earlier futures-only book is the comprehensive textbook that every working technical trader has on the shelf. The book partially fills the **Edwards & Magee** node in the [[foundation/sources/lineage|lineage]] (TA chart-pattern formalization, modernized) and is the foundational reference for the trading branch's day/swing instrumentation.

## Source Details

| Field | Value |
|-------|-------|
| **Author** | John J. Murphy (with Greg Morris on candlesticks) |
| **Edition** | New York Institute of Finance, 1999 (revision/expansion of *Technical Analysis of the Futures Markets*, 1986) |
| **Pages** | ~600 |
| **Original Market** | Futures (commodities, financials), updated to cover stocks and intermarket relationships |
| **Mechanism Tags** | Trend-following, mean-reversion (oscillators), microstructure (volume), pattern recognition |
| **Behavioral Driver** | Crowd psychology made visible in price action; Newton's-first-law momentum; support/resistance memory |
| **Crypto Applicability** | Direct — chart patterns, support/resistance, MA crossovers, RSI/MACD divergence are all directly applicable to crypto perps and spot |

## Position in Lineage

See [[foundation/sources/lineage]] for the full intellectual graph.

**This source builds on:**
- ☐ Charles Dow / Hamilton / Rhea (Dow Theory ~1900–1932) *(unfilled)* — Murphy's Chapter 2 is the canonical modern summary
- ☐ Edwards & Magee (*Technical Analysis of Stock Trends*, 1948) *(unfilled but largely subsumed)* — Murphy's chart-pattern chapters extend and modernize Edwards & Magee
- ☐ Wyckoff *(unfilled — top wiki priority)* — Murphy's volume / open-interest chapters draw heavily on Wyckoff's accumulation/distribution framework, citation pending
- ☐ Elliott (Elliott Wave Theory, 1930s) *(unfilled)* — Murphy's Chapter 13 covers it

**This source builds *parallel to* (not from):**
- ✅ [[foundation/sources/the-intelligent-investor|Graham]], ✅ [[foundation/sources/buffett-essays|Buffett]] — Murphy explicitly addresses the technical-vs-fundamental debate (Ch 1) and is in the *opposite* school. Buffett's "I have not known a single person who has consistently or lastingly made money by following the market" (the anti-TA quote in [[foundation/sources/the-intelligent-investor|Graham]]) is rebutted by Murphy's three premises.

**This source influenced:**
- All modern TA textbooks (Pring, Edwards-Magee-Bassetti, Bulkowski) — Murphy is the reference they all cite
- ☐ Modern crypto-trading culture *(unfilled crypto-native sources)* — the chart-pattern vocabulary used in CT (head & shoulders, doubles, MACD divergence, RSI overbought/oversold) descends from Murphy

**Concepts originated/canonicalized here (now in wiki):**
- The three premises of technical analysis → [[foundation/market-wisdom/dow-theory-and-ta-premises]]
- Dow Theory in modern form → [[foundation/market-wisdom/dow-theory-and-ta-premises]]
- Trend / support / resistance formalized → [[trading/mechanisms/support-resistance]]
- Chart pattern taxonomy (reversal + continuation) → [[trading/mechanisms/chart-patterns]]
- Moving average crossovers → [[trading/mechanisms/moving-averages]]
- Oscillators + divergence framework → [[trading/mechanisms/oscillators-divergence]]
- Volume / open interest as confirmation → [[trading/mechanisms/volume-analysis]]
- Intermarket analysis → [[foundation/macro/intermarket-analysis]] (Murphy's distinctive contribution)

## Why This Book Matters For The Wiki

Up until this ingest, the trading branch had exactly one mechanism page ([[trading/mechanisms/cyclical-timing]] from the Lynch ingest). The rest of the wiki was value-investing-flavored, useful for investment but not for the trader's stated day/swing horizon (≤1 week SL/TP per the wiki architecture Apr 28 update).

Murphy provides the **vocabulary and mechanism catalog** that the trading branch needs:
- Specific entry/exit triggers (breakouts, MA crossovers, oscillator divergence)
- Specific patterns with measurable price targets (head & shoulders, triangles, flags)
- Specific risk-management mechanics (trailing stops at support, position sizing relative to volatility)
- A taxonomy for cross-market work (intermarket analysis: stocks vs. bonds vs. dollar vs. commodities)

Each of these maps directly to the trader's day/swing trading task.

## Murphy's Three Premises of Technical Analysis

1. **Market action discounts everything.** Anything that can affect price — fundamental, political, psychological — is already in the price. Therefore study price.
2. **Prices move in trends.** With Newton's-first-law corollary: a trend in motion tends to continue.
3. **History repeats itself.** Chart patterns reflect crowd psychology, which doesn't fundamentally change.

> "The technical approach includes the fundamental. If the fundamentals are reflected in market price, then the study of those fundamentals becomes unnecessary. Chart reading becomes a shortcut form of fundamental analysis."

This is the technical school's answer to value investing. It coexists with — rather than refutes — the value approach: TA describes *how* prices move; value analysis describes *what they should be*. Murphy himself admits "Most traders classify themselves as either technicians or fundamentalists. In reality, there is a lot of overlap."

## The Murphy / Buffett Tension (worth flagging now)

Murphy's premise #1 ("market discounts everything") and Buffett's [[foundation/market-wisdom/modern-finance-theory-critique|critique of EMH]] look like they should clash, but actually don't:

- Murphy: prices already reflect known information → study prices
- Buffett: prices *imperfectly* reflect information → study businesses

Both authors agree fundamentals matter; they disagree on whether *price action itself* is a sufficient summary of fundamentals.

The deeper tension is between:
- **Murphy's premise #2** (trend continuation; trend-following works)
- **Buffett's "no one has consistently made money following the market"** (Graham *Intelligent Investor* quote)

These genuinely conflict. The empirical test: trend-following strategies *have* produced sustained returns (Mandelbrot's empirical work, Hurst exponents, momentum factor in Fama-French) but not without significant drawdowns. The trader's day/swing horizon is exactly where trend-following has the strongest empirical support. Treating this as a productive tension, not a contradiction to resolve.

See also: [[foundation/market-wisdom/value-vs-growth-false-dichotomy]] for the complementary resolution at the value-investing level.

## Notable Chapter Map (for selective re-reference)

| Chapter | Topic | Wiki page |
|---------|-------|-----------|
| 1 | Philosophy of Technical Analysis | [[foundation/market-wisdom/dow-theory-and-ta-premises]] |
| 2 | Dow Theory | [[foundation/market-wisdom/dow-theory-and-ta-premises]] |
| 3 | Chart Construction | (not extracted; OHLC bars, candlesticks, P&F) |
| 4 | Basic Concepts of Trends | [[trading/mechanisms/support-resistance]] |
| 5 | Major Reversal Patterns | [[trading/mechanisms/chart-patterns]] |
| 6 | Continuation Patterns | [[trading/mechanisms/chart-patterns]] |
| 7 | Volume and Open Interest | [[trading/mechanisms/volume-analysis]] |
| 8 | Long-Term Charts | (not extracted) |
| 9 | Moving Averages | [[trading/mechanisms/moving-averages]] |
| 10 | Oscillators and Contrary Opinion | [[trading/mechanisms/oscillators-divergence]] |
| 11 | Point and Figure Charting | (not extracted; lower priority for crypto) |
| 12 | Japanese Candlesticks (Greg Morris) | (covered in [[trading/mechanisms/chart-patterns]] reference) |
| 13 | Elliott Wave Theory | (not extracted; controversial subject) |
| 14 | Time Cycles | (not extracted) |
| 15 | Computers and Trading Systems | (not extracted; programming-side, less novel) |
| 16 | Money Management and Trading Tactics | (not extracted; risk management; future page) |
| 17 | Stocks and Futures: Intermarket Analysis | [[foundation/macro/intermarket-analysis]] |
| 18 | Stock Market Indicators | (not extracted) |

## Crypto Adaptation Notes

Murphy's framework translates to crypto more directly than the value-investing trilogy did. The mechanisms work because:

- **Premise #1 is stronger in crypto.** With on-chain data publicly available, "the market discounts everything" is more literally true — institutional information advantage is smaller, so price is a tighter summary.
- **Premise #2 is verified in crypto.** Crypto is one of the most-trending asset classes ever observed. The 4-year halving cycle, alt rotations, BTC dominance cycles all exhibit trend persistence well above random-walk levels.
- **Premise #3 is stronger in crypto.** Pattern recognition works because the same retail psychology drives every cycle. The 2017, 2021, 2025 cycles each rhymed with the previous.

**Specific crypto-native applications by mechanism:**

| Murphy concept | Crypto application |
|----------------|---------------------|
| Support/resistance | Round-number BTC levels (10K, 50K), prior cycle highs/lows, on-chain cost-basis levels (URPD) |
| Trendlines / channels | Cleanly applicable to BTC weekly/monthly charts |
| Moving averages (200d, 50d) | Famous BTC bull-bear regime indicator (price vs 200-week MA) |
| MACD / RSI / stochastics | Standard tools on every crypto charting platform; well-tested |
| Volume confirmation | Cross-exchange volume, on-chain volume, funding-as-volume proxy |
| Open interest | **Strong** in crypto perps — OI buildup signals are distinct and quantifiable |
| Chart patterns | Work in crypto, often more cleanly than equities (less institutional intervention) |
| Intermarket analysis | BTC ↔ ETH ↔ alts ↔ stablecoin supply ↔ macro (Fed, DXY) |

**Honest crypto-specific caveats:**
- TA on low-cap alts is unreliable due to manipulation (see Section 1 — pre-regulation character)
- Patterns can fail at higher rates due to leverage cascades / liquidation hunts
- Volume on centralized exchanges can be wash-traded; on-chain volume is more trustworthy
- The "self-defeating signal" problem: when too many traders use the same RSI threshold, it stops working at that threshold

## Honest Caveats About Murphy's Framework

- **Confirmation bias risk is high.** Chart patterns can be seen retroactively; in real-time the same pattern often "fails." Bulkowski's empirical work on pattern-completion rates suggests most patterns work only modestly above random.
- **Self-defeating prophecy.** The most well-known patterns (head & shoulders, double tops) have higher failure rates than less-known ones — because participants front-run the breakout.
- **Many indicators are derivatives of price.** RSI, MACD, stochastics — they all transform the same price series and can give false comfort that multiple indicators agree.
- **Murphy is skeptical of fundamentals; we shouldn't be.** Per the value-investing trilogy, fundamentals matter a lot. The right synthesis is technical timing + fundamental selection, not technical-only.
- **Some chapters age badly.** Elliott Wave (Ch 13) is famously unfalsifiable and Murphy doesn't grade it harshly enough. Time cycles (Ch 14) are similarly fragile. Treat these chapters as catalog entries, not load-bearing.

## Pages Extracted From This Source

| Page | Topic | Branch |
|------|-------|--------|
| [[foundation/market-wisdom/dow-theory-and-ta-premises]] | TA philosophy + Dow's six tenets | Foundation |
| [[trading/mechanisms/support-resistance]] | Trends, support, resistance, trendlines | Trading |
| [[trading/mechanisms/chart-patterns]] | Reversal + continuation patterns; measurable price targets | Trading |
| [[trading/mechanisms/moving-averages]] | MA types, crossover signals, envelopes/Bollinger | Trading |
| [[trading/mechanisms/oscillators-divergence]] | RSI, stochastics, MACD; the divergence framework | Trading |
| [[trading/mechanisms/volume-analysis]] | Volume + open interest as confirmation; cross-references future Wyckoff | Trading |
| [[foundation/macro/intermarket-analysis]] | Stocks ↔ bonds ↔ dollar ↔ commodities relationships | Foundation/Macro |
| [[glossary/divergence]] | Term: indicator and price disagree | Glossary |
| [[glossary/breakout]] | Term: price exits a range | Glossary |

## Sources

- Murphy, John J. *Technical Analysis of the Financial Markets: A Comprehensive Guide to Trading Methods and Applications*. New York Institute of Finance / Penguin, 1999.
- Full text extracted from EPUB: `raw/books/murphy-technical-analysis-epub.workdir/output.md` (711K chars, 40 sections)
- Note: original PDF in Downloads was scan-only with no text layer; EPUB version was used for extraction. EasyOCR was installed during the process and is now available for any future scan-only PDFs.

---

*Murphy completes the trading-instrumentation gap that the value-investing trilogy structurally couldn't fill. The trading branch went from 1 mechanism page to 7 with this ingest. The lineage page (Section 7 cluster: TA family) now has its modern node filled, with edges to the still-unfilled Dow / Edwards & Magee / Wyckoff predecessors.*
