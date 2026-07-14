---
title: "Sources Lineage — Intellectual Influence Graph"
status: growing
created: 2026-04-29
updated: 2026-07-12
tags: [meta, lineage, sources, dependencies, planning]
---

# Sources Lineage — Intellectual Influence Graph

> **TL;DR**: Map of how the wiki's source-level knowledge flows. Shows what's been ingested (filled nodes), what's the next priority (empty nodes), and how concepts trace from one source to another. Update this page whenever a new source is added — it's the artifact that prevents the wiki from becoming a flat catalog of independent books.

> **Sibling pages:** [[foundation/relations-and-themes]] (page-level themes); [[foundation/idea-sourcing-methodology]] (meta-process for sourcing); [[foundation/market-structure/crypto-carrier-features]] (carrier-feature catalog — what makes classical mechanisms not-already-arbitraged in crypto; the burden-of-proof gate every cross-market-transfer hypothesis must satisfy); **[[foundation/sources/ingest-queue]] (the researched what's-next queue — refreshed per ingest-research pass; the graph below shows what's IN, the queue shows what's NEXT).**
>
> **Note (2026-07-12)**: the graph below predates the 2026-05→07 ingests (regime-detection cluster, microstructure/funding papers, Ali cascade, and the 2026-07-12 tier-1 batch: liquidation-heatmap docs, sentiment-evidence papers, Kolchinsky, Kiel/CRS defense stack, Talos/Maitrier execution-cost pair). The pre-2026-05 nodes remain accurate; consult [[foundation/sources/ingest-queue]] + the sources folder for the current full census. Graph redraw is queued maintenance.

## Why This Page Exists

A wiki organized by topic (the rest of this wiki) makes it easy to find "what do we know about X." But that organization hides *which sources contributed which concepts*, and *which sources should be ingested next given current gaps*.

This page is the source-level graph. It complements the topic-level wiki by:

1. **Making the intellectual lineage visible** — Graham didn't invent everything; Buffett got most of it from Graham; Lynch got the framing but his concrete approach owes debts to earlier observation-based investors
2. **Surfacing missing nodes** — listing the unfilled sources right next to the filled ones, so prioritization is visualized rather than buried in the wiki architecture (ARCHITECTURE.md)
3. **Tracking concept dependencies** — when an unfamiliar idea shows up in a new source, the graph shows where it came from
4. **Disciplining the ingest queue** — the next source to ingest is the one that fills the highest-leverage missing edge

## The Core Lineage Graph

Filled nodes = ingested sources. Empty nodes = priority targets (per the wiki architecture (ARCHITECTURE.md) Section 7) not yet ingested.

```
                    ┌──────────────────────────────────┐
                    │   PRE-REGULATION / BEHAVIORAL    │
                    │   (HIGHEST PRIORITY for crypto)  │
                    └──────────────────────────────────┘

                       ✅ Lefèvre / Livermore
                       (Reminiscences, 1923) — INGESTED 2026-05-03
                       Tape reading; pivotal points; sit-tight discipline;
                       group-leader divergence; manipulation operations
                                  │
                                  ▼
                       ✅ Wyckoff Method
                       (Studies 1910 + Method Div 2 1931) — INGESTED 2026-05-03
                       Composite Operator; Three Laws (Supply/Demand,
                       Cause/Effect, Effort/Result); accumulation-distribution
                       cycle; spring/upthrust (successor labels)
                                  │
                                  ▼
                       ☐ Edwards & Magee
                       (Technical Analysis of Stock Trends, 1948)
                       Chart patterns formalized
                                  │
                                  ▼
                       ✅ Murphy
                       (Technical Analysis of the Financial Markets, 1999)
                       Modern TA reference; trading-branch backbone
                       Edge to ☐ Wyckoff (volume), ☐ Dow Theory (premises)

                    ┌──────────────────────────────────┐
                    │     VALUE INVESTING TRILOGY      │
                    │     (INGESTED — wiki spine)      │
                    └──────────────────────────────────┘

                       ✅ Graham & Dodd / Graham
                       (Security Analysis 1934 → Intelligent Investor 1949)
                       Investment vs. speculation, margin of safety, Mr. Market
                                  │
                       ┌──────────┴───────────┐
                       │                      │
                       ▼                      ▼
              ☐ Phil Fisher              ✅ Lynch
              (Common Stocks &          (One Up on Wall Street 1989)
               Uncommon Profits 1958)   Six categories, amateur edge
              Qualitative growth        Observation-driven application
                       │                      │
                       └──────────┬───────────┘
                                  ▼
                          ☐ Munger
                          (Poor Charlie's Almanack 2005)
                          Mental models, "wonderful business" framing
                                  │
                                  ▼
                       ✅ Buffett (Cunningham ed.)
                       (Essays 1997)
                       Capital allocation, owner earnings,
                       institutional imperative, MFT critique

                    ┌──────────────────────────────────┐
                    │       BUBBLE / CRISIS HISTORY    │
                    └──────────────────────────────────┘

                       ✅ Kindleberger
                       (Manias, Panics, and Crashes 1978/2000) — INGESTED 2026-05-04
                       5-stage cycle (Displacement → Boom → Euphoria → Distress
                       → Panic); Minsky three-finance taxonomy; LOLR framework
                                  │
                                  ▼
                       ☐ Chancellor
                       (Devil Take the Hindmost 1999)

                    ┌──────────────────────────────────┐
                    │     BEHAVIORAL / RISK / TAILS    │
                    └──────────────────────────────────┘

                       ☐ Kahneman
                       (Thinking, Fast and Slow 2011 — research from 1970s)
                                  │
                                  ▼
                       ☐ Mandelbrot ──→ ☐ Taleb
                       (Misbehavior     (Fooled by Randomness 2001 →
                        of Markets       Black Swan 2007 → Antifragile 2012)
                        2004)

                    ┌──────────────────────────────────┐
                    │   PRACTITIONER / MICROSTRUCTURE  │
                    └──────────────────────────────────┘

                       ✅ Schwager — Market Wizards series (1989-2020)
                       INGESTED 2026-05-04 (research-synthesis from Brandeis
                       study notes covering 1989 vol; secondary sources for
                       later vols). 70+ traders interviewed across 5 books.
                       Universal Wizards Lessons (8 themes); 15 mechanisms
                       harvested; Druckenmiller conviction-tiered sizing;
                       Raschke failed-breakout reversal.
                       ✅ Harris (Trading and Exchanges, 2003) — COMPLETE
                       All 657 pages, 29 chapters (full ingest 2026-04-30)
                       Foundation taxonomy + 10 themes + order flow + zero-sum
                       + bid/ask decomposition + manipulation + dealer economics
                       + liquidity + bubbles/crashes + insider trading
                       ☐ Chan (Quantitative Trading)
                       ☐ Grinold & Kahn (Active Portfolio Management)

                    ┌──────────────────────────────────┐
                    │       MACRO / FX / COMMODITIES   │
                    └──────────────────────────────────┘

                       ✅ Martinez (10 Essentials of Forex Trading, 2007)
                       Tactical retail FX manual; modest contribution
                       Useful: equity management + Fibonacci + FX context
                       (Limitation: motivational tone, retail-focused, mostly
                        Murphy material at applied level)
                       ☐ Drobny (Inside the House of Money) — global macro
                       ☐ Lien (Currency Trading) — FX patterns + central banks
                       ☐ CME Futures Guides

                    ┌──────────────────────────────────┐
                    │   CRYPTO-NATIVE (REFERENCE)      │
                    └──────────────────────────────────┘

                       ☐ Glassnode docs
                       ☐ Coinglass / Skew
                       ☐ Deribit Insights
                       ☐ CryptoQuant
```

## Concept Lineage — Who Originated What

Tracing key wiki concepts back to their source.

| Concept | Originator | Refined by | Currently in wiki at |
|---------|-----------|-----------|----------------------|
| Investment vs. speculation distinction | Graham 1934 | Buffett (constant theme) | [[glossary/investment-vs-speculation]] |
| Mr. Market allegory | Graham 1949 | Buffett 1987 letter | [[foundation/market-wisdom/mr-market]] |
| Margin of safety | Graham 1934 | Buffett (everywhere) | [[foundation/market-wisdom/margin-of-safety]] |
| Defensive vs. enterprising | Graham 1949 | — | [[investment/allocation/defensive-investor-portfolio]] |
| Endogenous vs. exogenous speculation | Graham 1958 speech | — | [[foundation/market-wisdom/the-new-speculation]] |
| Tape reading (formal definition + discipline) | Wyckoff 1910 (Studies Ch I — formal definition) | Livermore 1923 (experiential); Murphy 1999 (operationalized) | [[foundation/market-wisdom/tape-reading]] |
| Sit-tight discipline ("It was always my sitting") | Lefèvre 1923 — Old Turkey, Ch V | — | [[foundation/market-wisdom/sit-tight-discipline]] |
| Pivotal points / points of resistance / round-number breakouts | Wyckoff 1910 — Studies Chs IV, V (terminology + concept) | Lefèvre 1923 — Chs IX, XIV (popularization); Murphy 1999 | [[trading/mechanisms/pivotal-points]] |
| Group-leader divergence / "leaders cease to lead" | Wyckoff 1910 — Studies Ch III (group-responsiveness tests) | Lefèvre 1923 — Chs XIV, XVII; modern breadth analysis | [[trading/mechanisms/group-leader-divergence]] |
| Line of least resistance / stream-and-dam metaphor | **Wyckoff 1910 — Studies Ch V** (earliest published articulation) | Lefèvre 1923 — Chs X, XXI (popularization) | section in [[trading/mechanisms/support-resistance]] |
| Bear-raid-as-inverted-tip rule | Lefèvre 1923 — Chs XV, XXIII | — | section in [[foundation/market-structure/market-manipulation-taxonomy]] |
| Tip-as-hope-delivery (Pattern C) | Lefèvre 1923 — Chs XVI, XXIV | — | Pattern C in [[foundation/idea-sourcing-methodology]] |
| Composite Operator framing | **Wyckoff 1931 — Method TR Sec 10** (explicit, named) | 1910 Studies Ch I has plural-manipulators precursor; successor teaching extends | [[foundation/market-wisdom/composite-operator]] |
| Three Laws (Supply/Demand explicit; Cause/Effect + Effort vs. Result operational) | Wyckoff 1910/1931 (operational logic); successor formalization (Evans 1940s+) | Pruden, Weis, Fraser refine | [[foundation/market-wisdom/wyckoff-three-laws]] |
| Accumulation / distribution four-phase cycle | **Wyckoff 1910/1931** (Studies Ch V; Method TR Sec 10) | Successor schematic (A-E phases) is post-1934 (Evans, Pruden) | [[trading/mechanisms/accumulation-distribution]] |
| Spring / shakeout at end of accumulation (concept; not the label) | Wyckoff 1910 — Studies Ch VII | Successor label "spring" (Evans 1940s+) | [[trading/mechanisms/accumulation-distribution]] |
| Upthrust at end of distribution (concept; not the label) | Wyckoff 1931 — Method TR Sec 2-A diagrams | Successor label "upthrust" (Evans 1940s+) | [[trading/mechanisms/accumulation-distribution]] |
| Effort vs. Result diagnostic (operational, unnamed in primary) | Wyckoff 1910/1931 | Successor formalization as "Law 3" (Evans) | section in [[foundation/market-wisdom/wyckoff-three-laws]]; section in [[trading/mechanisms/volume-analysis]] |
| Wave Chart (composite-of-leaders proxy for trend) | Wyckoff 1931 — Method TR Sec 2 | Modern moving-average / breadth analogs | section in [[trading/mechanisms/volume-analysis]] |
| Cause-and-Effect P&F horizontal count | Wyckoff 1910 — Studies Ch VIII | Evans formalization | concept noted in [[foundation/market-wisdom/wyckoff-three-laws]] |
| 50% retracement diagnostic | Wyckoff 1931 — Method TR Sec 7 | Fibonacci tradition reaches similar level | concept noted in [[trading/mechanisms/fibonacci-retracements]] |
| Stop hunting / squeeze plays | Lefèvre 1923 | Wyckoff 1910 (Ch VII shakeouts as same mechanism) | section in [[foundation/market-structure/market-manipulation-taxonomy]] + [[trading/mechanisms/accumulation-distribution]] |
| 5-stage bubble cycle (Displacement → Boom → Euphoria → Distress → Panic) | **Kindleberger 1978** (popularization); Minsky 1992 FIH (analytical base) | Variant Perception, Pragmatic Capitalism, Galbraith *A Short History of Financial Euphoria* | [[foundation/market-wisdom/bubble-cycle-stages]] |
| Minsky three-finance taxonomy (Hedge / Speculative / Ponzi) | **Minsky FIH 1992**; Kindleberger Ch 2 popularizes | Carlos Ponzi (1920s) is the eponym, not the originator of the framework | [[foundation/market-wisdom/minsky-three-finance]] |
| Pro-cyclical credit supply as endogenous fragility mechanism | Minsky FIH | Wicksell, Fisher, Mill antecedents | section in [[foundation/market-wisdom/minsky-three-finance]] |
| "Where does the cash come from?" diagnostic | Kindleberger Ch 2 | applies as direct test | section in [[foundation/market-wisdom/minsky-three-finance]] |
| Lender of Last Resort framework (Bagehot rule + crypto LOLR open question) | Bagehot 1873 *Lombard Street* (PD); Kindleberger Ch 2 evaluates | Modern: Conti-Brown, Sissoko on Bagehot scholarship | section in [[foundation/sources/kindleberger-manias-panics-crashes]] |
| Displacement as exogenous shock concept | Kindleberger Ch 2 (synthesizes earlier work) | Schumpeterian shock; Minsky FIH | section in [[foundation/market-wisdom/bubble-cycle-stages]] |
| 4-channel international propagation (arbitrage, trade, capital, psychology) | Kindleberger Ch 2 | Black Monday 1987 as worked example | section in [[foundation/market-wisdom/bubble-cycle-stages]] |
| Crypto 2020-2022 cycle K-framework worked example | This wiki synthesis (multiple secondary sources) | First contemporaneous-style cycle classification in wiki | [[foundation/macro/crypto-2020-2022-cycle]] |
| Universal Wizards Lessons (8 cross-trader themes) | Schwager (across all 5 vols, 1989-2020) | Synthesis of recurring patterns across ~70 wizards | [[foundation/market-wisdom/universal-wizards-lessons]] |
| Failed-breakout reversal / Turtle Soup | Linda Bradford Raschke (Schwager NMW 1992) | Inversion of Richard Dennis's Turtle Trader breakout system | [[trading/mechanisms/failed-breakout-reversal]] |
| Conviction-tiered sizing / top-decile-only | Stanley Druckenmiller (Schwager NMW 1992) | "It takes courage to be a pig" — meta-mechanism for which trades to size up | [[investment/allocation/conviction-tiered-sizing]] |
| Volatility-adjusted position sizing | Larry Hite (MW 1989), refined by Tom Basso (NMW 1992) | Position size = MIN(risk-budget, volatility-budget) | section in [[investment/allocation/equity-management-rules]] |
| Reverse-pyramid sizing rule | Paul Tudor Jones + Marty Schwartz (Schwager MW 1989) | Trader's-own-equity-curve as risk regulator | section in [[foundation/market-wisdom/sit-tight-discipline]] |
| Symmetric scale-in / scale-out | Bill Lipschutz (Schwager NMW 1992) | TWAP-style entries + mirror on exits | section in [[foundation/market-wisdom/sit-tight-discipline]] |
| Triple-confirmation 5x sizing | Michael Marcus (Schwager MW 1989) | Fundamentals + technicals + tone alignment | section in [[investment/allocation/equity-management-rules]] |
| Volatility circuit-breaker | Larry Hite (Schwager MW 1989) | Suspend trading when vol regime exceeds threshold | section in [[investment/allocation/equity-management-rules]] |
| Variant perception trade | Michael Steinhardt (Schwager MW 1989) | Articulated belief at variance with consensus + named catalyst | (referenced from [[foundation/idea-sourcing-methodology]]; further documentation deferred) |
| Forced-selling discount (airdrops) | Joel Greenblatt (Schwager HFMW 2012) | Spinoff dynamics → crypto airdrops | (referenced from [[foundation/market-structure/market-manipulation-taxonomy]]) |
| Chart-pattern formalization | Edwards & Magee 1948 | All modern TA texts | ☐ unfilled |
| Fast-grower / stalwart / cyclical taxonomy | Lynch 1989 | — | [[foundation/market-wisdom/lynch-six-categories]] |
| Amateur edge / Wall Street oxymorons | Lynch 1989 | Buffett (institutional imperative deepens it) | [[foundation/market-wisdom/amateur-edge]] |
| 13 attributes of perfect stock | Lynch 1989 | — | [[foundation/market-wisdom/perfect-stock-attributes]] |
| Tenbagger / diworseification | Lynch 1989 (coinages) | — | [[glossary/tenbagger]], [[glossary/diworseification]] |
| Cyclical timing (P/E inversion) | Lynch 1989 | — | [[trading/mechanisms/cyclical-timing]] |
| Owner earnings | Buffett 1986 letter | — | [[foundation/market-wisdom/owner-earnings]] |
| Circle of competence | Buffett 1996 letter | (Munger reinforces) | [[foundation/market-wisdom/circle-of-competence]] |
| Institutional imperative | Buffett 1989 letter | — | [[foundation/market-wisdom/institutional-imperative]] |
| Economic vs. accounting goodwill | Buffett 1983 letter | — | [[foundation/market-wisdom/economic-goodwill]] |
| "Wonderful business at fair price" | Munger → Buffett pivot 1972+ | — | [[investment/allocation/wonderful-business-fair-price]] |
| Beta / EMH critique | Buffett 1993 letter | — | [[foundation/market-wisdom/modern-finance-theory-critique]] |
| Three premises of TA | Murphy 1999 (canonicalized; informal earlier) | — | [[foundation/market-wisdom/dow-theory-and-ta-premises]] |
| Dow Theory tenets | Charles Dow ~1900 → Hamilton 1922 → Rhea 1932 → Murphy 1999 | — | [[foundation/market-wisdom/dow-theory-and-ta-premises]] |
| Trend / support / resistance | Edwards & Magee 1948 → Murphy 1999 | — | [[trading/mechanisms/support-resistance]] |
| Chart patterns (taxonomy) | Edwards & Magee 1948 → Murphy 1999 → Bulkowski 2000s | — | [[trading/mechanisms/chart-patterns]] |
| Moving averages / Bollinger | Granville/Appel/Bollinger ~1970s → Murphy 1999 | — | [[trading/mechanisms/moving-averages]] |
| RSI / MACD / Stochastics / Divergence | Wilder 1978 / Appel 1970s / Lane 1950s → Murphy 1999 | — | [[trading/mechanisms/oscillators-divergence]] |
| Volume / open interest analysis | Wyckoff 1930s → Granville 1963 (OBV) → Murphy 1999 | — | [[trading/mechanisms/volume-analysis]] |
| Intermarket analysis | Murphy 1991/1999/2004 (signature contribution) | — | [[foundation/macro/intermarket-analysis]] |
| Equity management rules (risk per trade, R:R, expectancy) | Tharp ~1990s; Vince ~1990s; Martinez 2007 (this wiki's source) | — | [[investment/allocation/equity-management-rules]] |
| Fibonacci retracements | Elliott Wave 1930s; Martinez 2007 (applied) | Murphy briefly | [[trading/mechanisms/fibonacci-retracements]] |
| FX market structure (sessions, leverage, pip, carry) | Standard FX retail literature; Martinez 2007 | — | [[foundation/asset-classes/forex]] |
| Microstructure 10 recurrent themes | Harris 2003 (canonicalized) | — | [[foundation/market-structure/microstructure-themes]] |
| Buy-side / sell-side trader taxonomy | Harris 2003 (canonicalized) | — | [[foundation/market-structure/trader-taxonomy]] |
| Order flow externality | Harris 2003 (signature contribution) | — | [[foundation/market-structure/order-flow-externality]] |
| Zero-sum game framing of trading | Harris 2003 | — | [[foundation/market-structure/zero-sum-game]] |
| Bid/ask spread decomposition (transitory + adverse selection) | Glosten-Milgrom 1985 → Harris 2003 (canonical synthesis) | — | [[foundation/market-structure/bid-ask-spread-decomposition]] |
| Market manipulation taxonomy (order anticipators, bluffers, squeezers) | Harris 2003 | — | [[foundation/market-structure/market-manipulation-taxonomy]] |
| Utilitarian vs profit-motivated trader motivations | Harris 2003 | — | [[foundation/market-structure/why-people-trade]] |
| Dealer two-risk model (diversifiable inventory + adverse selection) | Harris 2003 | — | [[foundation/market-structure/dealer-economics]] |
| Liquidity four dimensions (immediacy, width, depth, resilience) | Harris 2003 | — | [[foundation/market-structure/liquidity]] |
| Crash anatomy (portfolio insurance / leverage cascade dynamics) | Harris 2003 (synthesizing 1987 analysis) | — | [[foundation/market-structure/bubbles-crashes-circuit-breakers]] |
| Insider-trading detection framework + crypto analogs (MEV, pre-launch) | Harris 2003 | — | [[foundation/market-structure/insider-trading]] |
| Bubble dynamics / narrative cycle | Kindleberger 1978 | Chancellor, Shiller, Soros | ☐ unfilled |
| Cognitive biases applied to markets | Kahneman 1970s+ → behavioral finance | Thaler, Shiller | ☐ unfilled |
| Fat tails / non-Gaussian markets | Mandelbrot 1960s+ | Taleb | ☐ unfilled |
| Antifragility / barbell strategy | Taleb 2012 | — | ☐ unfilled |
| Mechanism families per market type | Schwager (Market Wizards interviews) | — | ☐ unfilled |
| Market microstructure formalism | Harris 2002 | Modern academic literature | ☐ unfilled |
| Purged k-fold cross-validation + embargo | López de Prado 2018 (Ch 7) | Bailey et al. (CSCV) | [[foundation/methodology/strategy-validation-discipline]] |
| Probabilistic Sharpe Ratio (PSR) | Bailey & López de Prado 2012 | López de Prado 2018 (Ch 14) | section in [[foundation/methodology/strategy-validation-discipline]]; [[glossary/deflated-sharpe-ratio]] |
| Deflated Sharpe Ratio (DSR) | Bailey & López de Prado 2014 | López de Prado 2018 (Ch 14) | [[glossary/deflated-sharpe-ratio]] |
| Probability of Backtest Overfitting (PBO) via CSCV | Bailey, Borwein, López de Prado, Zhu 2014/2017 | López de Prado 2018 (Ch 11.6) | [[glossary/probability-of-backtest-overfitting]] |
| Sample uniqueness for non-IID financial labels | López de Prado 2018 (Ch 4) | — | [[glossary/sample-uniqueness]] |
| Marcos' Second + Third Laws of Backtesting | López de Prado 2018 (Ch 11, Ch 14) | — | [[foundation/methodology/strategy-validation-discipline]] |
| Filtering vs smoothing for live regime labelling | State-space estimation (Kalman/HMM forward-filter vs forward-backward smoother) | Applied to regime-deployment gap (this wiki, 2026-06-07) | [[foundation/methodology/online-regime-detection]] |
| Online (signature-MMD) regime detection | Issa & Horvath 2023 (arXiv:2306.15835); builds on Lyons signatures + Gretton MMD | — | [[foundation/sources/issa-horvath-online-regime-detection]] |
| Score-driven Bayesian Online Change-Point Detection | Tsaknaki, Lillo & Mazzarisi 2023 (arXiv:2307.02375); builds on Adams–MacKay BOCPD + Creal–Koopman–Lucas GAS | — | [[foundation/sources/tsaknaki-lillo-mazzarisi-bocpd]] |
| Markov-Switching GARCH crypto vol-regimes (high/low, one-step-ahead) | Ardia, Bluteau, Rüede 2019 + Guelph WP 2022-02 (292-coin); builds on Bollerslev GARCH + Hamilton regime-switching | — | [[foundation/sources/msgarch-crypto-vol-regimes]] |
| 4-state NHHM bull/bear/calm regime switching (BTC/ETH) | Koki, Leonardos, Piliouras 2020/2024 (arXiv:2011.03741); builds on HMM + non-homogeneous transitions | — | [[foundation/sources/nhhm-crypto-regime-switching]] |
| Bitcoin variance risk premium + regime-conditioning (BVRP higher in calm) | Almeida, Grith, Miftachov, Wang 2024 (arXiv:2410.15195); builds on Breeden-Litzenberger RND + Carr-Wu VRP | refined into [[trading/mechanisms/variance-risk-premium-crypto]] | [[foundation/sources/almeida-bitcoin-risk-premia]] |
| Gamma exposure (GEX) as vol-regime classifier (dealer hedging sign) | SPX dealer-gamma practitioner lineage → Glassnode taker-flow GEX 2025; crypto translation | — | [[trading/mechanisms/gamma-exposure-regime]] |
| Crypto vol-regime by term-structure state + DVOL + Senti-Meter | Deribit Insights / Block Scholes (crypto-native); applies Sinclair to crypto | — | [[foundation/sources/deribit-insights]] |
| Regime-marker backtestable data sources (free core) | This wiki synthesis 2026-06-07 (alternative.me / DefiLlama / Coin Metrics / chaindl) | — | [[foundation/methodology/regime-marker-data-sources]] |

## Why the Behavioral Pre-Regulation Cluster Is the Top Priority

Per Part I Section 1:

> Crypto today resembles **pre-regulation US equity markets** more than current equities... Pre-modern-regulation trading wisdom (Livermore, Wyckoff, Edwards & Magee) is *more* applicable than 21st-century institutional quant literature.

The value-investing trilogy currently in the wiki (Graham → Lynch → Buffett) is about *long-horizon equity ownership in regulated, transparent markets*. Crypto's character — manipulated, retail-driven, lightly regulated, high information asymmetry, 24/7, leveraged — is closer to 1920s equities than to 2026 equities.

The behavioral pre-regulation cluster gives the wiki the analytical lens that the value-investing trilogy structurally cannot:

| What the trilogy covers | What it doesn't | What pre-regulation classics provide |
|-------------------------|-----------------|--------------------------------------|
| What to buy (categories, attributes) | When manipulation is happening | Tape reading (Livermore) |
| Quality + price discipline | Smart-money flow detection | Wyckoff accumulation/distribution |
| Long-horizon compounding | Short-horizon position dynamics | Edwards & Magee patterns |
| Avoid bubbles by valuation discipline | Recognize bubbles by their dynamics | Kindleberger / Chancellor |
| Behavioral discipline (Mr. Market) | Specific cognitive biases catalog | Kahneman / Thaler |
| Avoid catastrophic loss (margin of safety) | Tail behavior of returns | Mandelbrot / Taleb |

So the next priority isn't "add more value-investing depth." It's "add the behavioral lens that the trilogy isn't built to provide."

## Ingest Priority Queue

In strict priority order per the lineage gaps and the wiki architecture (ARCHITECTURE.md) weighting:

| Rank | Source | Why this priority | Estimated leverage |
|------|--------|-------------------|---------------------|
| ~~1~~ | ~~**Lefèvre / Livermore — Reminiscences of a Stock Operator (1923)**~~ | **✅ INGESTED 2026-05-03.** Source page: [[foundation/sources/lefevre-reminiscences]]. 5 new pages + 6 page-updates. Was "single most important book" per the wiki architecture (ARCHITECTURE.md); foundational document of tape reading; gap filled. | — |
| ~~1~~ | ~~**The Wyckoff Method (Studies 1910 + Method 1931)**~~ | **✅ INGESTED 2026-05-03.** Source page: [[foundation/sources/wyckoff-method]]. 4 new pages + cross-references. The analytical formalization of pre-regulation tape-reading; reads in stereo with Lefèvre. Composite Operator + Three Laws + accumulation/distribution cycle + spring/upthrust now in wiki. Salvage path for candle-morphology rejected batch 5.1/5.2 formalized. | — |
| ~~3~~ | ~~Edwards & Magee~~ | **Largely subsumed by ✅ Murphy ingest** (2026-04-29). Murphy modernizes and extends the Edwards & Magee chart-pattern catalog. Original Edwards & Magee remains valuable for historical depth but is no longer top priority. | Lowered to Medium |
| ~~1~~ | ~~**Kindleberger — Manias, Panics, and Crashes (1978/2000)**~~ | **✅ INGESTED 2026-05-04.** Source page: [[foundation/sources/kindleberger-manias-panics-crashes]]. 4 new pages + cross-references. The bubble dynamics dimension; Minsky FIH operationalized. Pre-regulation/behavioral cluster's *core complete* — experiential (Lefèvre), analytical microstructure (Wyckoff), modern microstructure formalism (Harris), chart patterns (Murphy), bubble dynamics (Kindleberger). Crypto 2020-2022 cycle worked-example confirms framework applicability. | — |
| ~~1~~ | ~~**Schwager — Market Wizards (interview series, 1989-2020)**~~ | **✅ INGESTED 2026-05-04.** Source page: [[foundation/sources/schwager-market-wizards]]. 4 new pages + cross-references. 8 universal wizards lessons + 15 mechanism harvest + Druckenmiller conviction-tiered sizing as meta-mechanism + Raschke failed-breakout reversal as full structured-hypothesis trading mechanism. Practitioner-breadth dimension complete. | — |
| ~~off-axis~~ | ~~**López de Prado — Advances in Financial Machine Learning (2018)**~~ | **✅ INGESTED 2026-05-05.** Source page: [[foundation/sources/lopez-de-prado-advances-fin-ml]]. 1 source page + 1 methodology page ([[foundation/methodology/strategy-validation-discipline]]) + 4 glossary entries. **Off the behavioral-pre-regulation axis** — this fills the wiki's *validation-discipline* dimension, which is universal across markets and orthogonal to the behavioral-cluster. Triggered by 2026-05-04 the backtesting lab analysis (train→val correlation negative for 7/8 strategies on single 70/30 split) + Apr 30 candle-morphology-batch failure pattern. Closes structural gap: "no overfit worship" + "falsifiers required" principles in the wiki architecture (ARCHITECTURE.md) Part IV now have methodological backbone. | — |
| ~~off-axis~~ | ~~**Sinclair — Volatility Trading 2nd ed. (2013)**~~ | **✅ INGESTED 2026-05-08 AM.** Source page: [[foundation/sources/sinclair-volatility-trading]]. 1 source + 3 mechanism pages + 7 glossary entries. **Off the cash-equity / perp-side cluster** — fills the *vol/options* dimension; activates the wiki's least-saturated mechanism family per scaffold-diversification-gap analysis. Crypto-translation discipline applied per concept (overnight-jump absent in 24/7; skew sign regime-conditional in crypto; term-structure shorter and event-driven). Capability gap surfaced: all 3 new mechanism pages depend on Deribit per-strike IV-feed + DVOL-futures data ingest. | — |
| ~~crypto-native~~ | ~~**Hayes — Crypto Trader Digest essays (2024-2026 Substack window)**~~ | **✅ INGESTED 2026-05-08 PM-2.** Source page: [[foundation/sources/hayes-essays]]. 1 source + 3 glossary entries + 5 cross-link sharpenings (funding-cycle-dynamics, behavioral-positioning-unwind, btc-regime-taxonomy, bubble-cycle-stages, liquidation-cascade-aftermath). **First crypto-native primary source** — closes the wiki's most-embarrassing structural gap (11 prior sources, 0 crypto-native). 9 essays curated; themes: dollar-liquidity → BTC transmission (5-essay redundancy), perp funding mechanics primary-source (Adapt or Die — Hayes co-built BitMEX XBTUSD perp 2016), stablecoin → treasury flow, multi-currency credit-impulse cycle calibration. Reduced scope: pre-2024 BitMEX-blog classics (Dollar Milkshake / Maelstrom intro / 2020-2023 cascade post-mortems) inaccessible (404 / blog migrated / Substack pagination broken). Single-author lineage risk acknowledged; companion crypto-native sources (CoinMetrics, Glassnode research, arXiv crypto microstructure) flagged for follow-up. | — |
| **1** | **Chancellor — Devil Take the Hindmost (1999)** | **Now top priority** following Schwager ingest. History of speculation across 400 years. Pattern-recognition source; complements Kindleberger with more narrative depth. | Medium-High |
| **3** | **Kahneman — Thinking, Fast and Slow (2011)** | Cognitive bias catalog. Underlies all behavioral finance. | Medium-High |
| **4** | **Taleb — Fooled by Randomness / Black Swan / Antifragile** | Tail risk framework. Crypto is one of the most fat-tailed asset classes; this is essential. | Medium-High |
| **8** | **Munger — Poor Charlie's Almanack** | The piece of the value-investing trilogy currently missing — the mental-models layer that influenced Buffett's pivot. | Medium |
| **9** | **Harris — Trading and Exchanges (2002)** | Modern market microstructure reference. Essential for the trading branch. | Medium |

## What "Position in Lineage" Should Mean for Future Source Entries

Going forward, every source entry created in `foundation/sources/` should include a "Position in Lineage" section near the top, structured as:

```markdown
## Position in Lineage

**This source builds on:**
- [[link to predecessor source]] — what it took
- [[link to predecessor source]] — what it took

**This source influenced:**
- [[link to successor source]] — what it gave
- (or: not yet ingested → see [[foundation/sources/lineage]])

**Concept lineage map** — concepts originated/refined here:
- Concept 1 — first formalized in this source
- Concept 2 — refined from earlier source X
```

This makes the source's *graph position* explicit at the top, rather than buried in the "Related Sources" section at the bottom. The lineage page (this page) is then an aggregation of these per-source declarations.

## How to Use This Page

**When you start work on a new source:**
1. Find its node in the graph above
2. Check what predecessors are already ingested vs. missing
3. If a critical predecessor is missing, ingest it first (or note the dependency in the new entry)

**When you're prioritizing what to ingest next:**
1. Look at the empty nodes
2. Pick the one with highest leverage per the priority table
3. After ingesting, update this page (move ☐ to ✅ in the graph + add new edges)

**When a contradiction surfaces between two sources:**
1. Check whether they're in the same lineage (same school) or different schools
2. Same-school contradiction → Knowledge Evolution Protocol applies (per Part IV)
3. Different-school contradiction → likely productive tension; document both positions and their schools

**When you encounter an unfamiliar concept while reading wiki content:**
1. Find the concept in the "Concept Lineage" table above
2. Trace it back to its originator
3. If the originator source is unfilled (☐), the wiki has a structural gap

## Maintenance

This page is updated:
- Every time a new source is ingested (move ☐ to ✅; add new graph edges)
- Whenever a new concept is added to the wiki (append a row to Concept Lineage table)
- After any LINT that changes priority order

This page is a *living artifact*. A stale lineage page is worse than no lineage page; if you're updating sources without updating this, the gap will grow silently.

## Open Questions About the Lineage Itself {#open-questions}
- **Is "value investing → Buffett's modern synthesis" really a single lineage, or does Munger represent a parallel tradition (mental models, Charlie's specific influences) that fed into the same destination?** Probably the latter. When Munger's *Almanack* is ingested, the graph should show two converging streams.
- **Does crypto deserve its own lineage cluster, separate from the equity-derived ones?** Currently treating crypto-native sources (Glassnode, etc.) as data references rather than intellectual sources. May need its own cluster as the wiki matures.
- **What's the right way to represent *anti*-influences** — sources that are explicitly framed in opposition to other sources (e.g., Buffett's modern-finance-theory critique is *anti*-Markowitz/Sharpe/Fama)? Currently treated as one-way "rebuts" but the graph could show explicit opposition edges.

## Related
- [[de-la-vega-confusion-de-confusiones|de la Vega — Confusión de Confusiones (1688)]] — earliest behavioral-market source; predates the cluster
- [[neill-tape-reading-market-tactics|Neill — Tape Reading and Market Tactics (1931)]] — price-volume-confirmation member of the pre-regulation cluster

- the wiki architecture (ARCHITECTURE.md) Section 7 — the original curated reading list, which this page operationalizes as a graph
- [[foundation/sources/the-intelligent-investor]]
- [[foundation/sources/one-up-on-wall-street]]
- [[foundation/sources/buffett-essays]]
- [[foundation/sources/murphy-technical-analysis]]
- [[foundation/sources/forex-10-essentials]]
- [[foundation/sources/harris-trading-exchanges]]
- [[foundation/sources/lefevre-reminiscences]]
- [[foundation/sources/wyckoff-method]]
- [[foundation/sources/kindleberger-manias-panics-crashes]]
- [[foundation/sources/schwager-market-wizards]]
- [[foundation/sources/lopez-de-prado-advances-fin-ml]]
- Activity log — when each source was ingested and what was created

## Sources

- The lineage claims in this page are largely from authors' own statements about their influences (Buffett's "85% Graham, 15% Fisher"; Lynch's repeated references to *Reminiscences*; Cunningham's introduction to the Buffett essays).
- Where claims are contested or uncertain, marked as such.
