---
title: "Liquidation Cascade Aftermath — Mechanism Family"
status: growing
created: 2026-05-04
updated: 2026-05-15
tags: [mechanism-family, liquidation-cascade, cascade-aftermath, day-swing-applicable, crypto, capability-blocked]
---

# Liquidation Cascade Aftermath — Mechanism Family

> **TL;DR**: Mechanism family covering trades that exploit *predictable price behavior in the period immediately after a forced-liquidation event*. The cascade itself is mechanically-driven (margin calls, stop-runs, exchange auto-liquidation engines) and produces a measurable overshoot. The aftermath features price exhaustion, cover flow, and recovery dynamics that are tradable. **One of the trader's prioritized day/swing families per Apr 28 mechanism families.** This family is *crypto-uniquely-tradable* because liquidation maps (Coinglass) make the setup observable in real time. **Status growing as of 2026-05-08, extended 2026-05-15 with academic-paper anchors + N=7 case studies**: substrate from 5 ingested sources (Wyckoff, Lefèvre, Kindleberger, Harris, Schwager) consolidated; **N=7 historical crypto cascades empirically catalogued** (Oct 10-11 2025 added as mixed-microstructure+macro third-category case); numerical operational thresholds defined; DSL capability gap mapped; [[foundation/sources/he-manela-fundamentals-perpetual-futures|He, Manela et al. 2024]] academic-paper anchor added for positive-feedback momentum mechanism (R²>50% explains futures-spot gap). **All three templates currently capability-blocked** at backtest level — Coinglass liquidation feed (capability roadmap #3) is the load-bearing unlock.

## Family Definition

**WHO is moving money during the cascade**: leveraged retail (forced liquidations), trend-following systems (stop-outs), discretionary panic-sellers (door-shut behavior).

**WHO is moving money in the aftermath**: shorts covering (forced or discretionary), bottom-fishers, market makers re-bidding, mean-reversion systems re-entering.

**WHY the aftermath is tradable**: the cascade is *mechanical* (forced-liquidation + stop-out flow), not informed. The overshoot is therefore non-fundamental — it represents *position-management*, not *valuation change*. Once the forced selling exhausts, the bid returns and price recovers some portion of the cascade.

**WHEN cascades occur**: high open interest + leveraged positioning + clustered stops at known levels + a triggering macro or technical event.

## Members of the Family

### Already in wiki — full mechanism page

| Mechanism | Wiki page | Aftermath specifics |
|-----------|-----------|---------------------|
| **Wyckoff Spring** | [[trading/mechanisms/accumulation-distribution]] (spring subsection) | The canonical cascade-aftermath trade: sharp break below well-defined support + immediate recovery + on-chain accumulation during wick. **The most-developed crypto-direct mechanism in the wiki.** |
| **Wyckoff Upthrust** | [[trading/mechanisms/accumulation-distribution]] (upthrust subsection) | Mirror — short-side cascade aftermath: sharp thrust above resistance + failure to follow through + return below |
| **Failed-breakout reversal / Turtle Soup** | [[trading/mechanisms/failed-breakout-reversal]] | Cascade-as-stop-run interpretation: failed breakout = stop-clustered traders forced to cover; cover flow IS the aftermath |
| **Bubble-cycle Stage 5 (Panic) bottoming** | [[foundation/market-wisdom/bubble-cycle-stages]] (Panic stage) | Macro-scale cascade aftermath; multi-week to multi-month aftermath play vs intraday for spring |

### Partial coverage in wiki

| Mechanism | Wiki page | Coverage |
|-----------|-----------|----------|
| **Microstructure-cascade recovery distinguished from fundamental cascade** | [[foundation/market-structure/bubbles-crashes-circuit-breakers]] | Harris/Kindleberger: microstructure-driven cascades recover quickly; fundamental cascades take longer. **Trading edge** = correctly classifying the cascade type to predict recovery timing |
| **Pivotal-point breakout failure as cascade trigger** | [[trading/mechanisms/pivotal-points]] (false breakout caveats) | Cascade as the *failure mode* of pivotal-point breakouts |

### Candidate mechanisms (not yet in wiki)

- **Liquidation-magnet trades**: when liquidation cluster density is high above current price, price often *gravitates* toward the cluster (predictable directional bias). Trade: position long below the cluster anticipating the magnet effect.
- **Cascade-cause classification trade**: distinguish microstructure cascade (recovers quickly) from fundamental cascade (long bear) using on-chain whale-cohort response. Trade direction depends on classification.
- **Multi-cascade exhaustion trade**: in a sustained bear, multiple cascade events occur. Each one exhausts more weak hands. The *final* cascade (after which no further capitulation occurs) marks the cycle bottom; multi-cascade pattern recognition.

## Source Excerpts

The five sources from the wiki's pre-regulation behavioral cluster + microstructure layer all describe cascade-aftermath mechanics with complementary emphasis. Verbatim quotes below; full distillations in `raw/books/<source>/distillation.md`.

### Wyckoff — Shakeout-as-accumulation-tell (1910 *Studies*, Ch V + Ch VII)

> "When insiders shake other people out it means that they want the stock themselves. These are good times for us to get in, for then we will enjoy having Mr. Frick and his friends work for us." (Studies Ch VII)

> "On the other hand, when there is a gradual hardening in prices; when bear raids fail to dislodge considerable quantities of stock; when stocks do not decline upon unfavorable news, we may look for an advancing market in the near future." (Studies Ch VII)

> "If we drilled into the market at the top of the recent rise, we should have found that the bulk of the floating supply in Steel, Reading and some others was held by a class of traders who buy heavily in booms and on bulges. These people operate with a comparatively small supply of margins, nerve and experience. They are exceedingly vulnerable, hence the stocks in which they operate suffer the greatest declines when the market receives a jar." (Studies Ch VI)

**Mechanism extracted**: the shakeout is not just an arbitrary wick — it is the deliberate-or-emergent mechanism by which weak holders (Wyckoff's "vulnerable" overextended-margin retail) are cleared so the next markup phase can proceed. Trade structure: long entries placed immediately above (not at) the cleared support, after the recovery candle confirms. **The 1910 framing predates Coinglass by 113 years but identifies the same operational object**: clustered weak holders below known support, predictable forced-selling on the break.

### Lefèvre — The 1907 panic and the "raid" libel inversion (1923 *Reminiscences*)

> "On October 24, 1907, the money market post became the storm centre of the day's history. Brokers' calls for money went unanswered at any price. Mr. Livermore was short the entire market. He had the power to crater the market further. Then a banker came to him and asked him to please stop." (Ch IX, paraphrase of the canonical Money-Post episode; Morgan-Stillman-Atterbury "$10M, take it easy" rescue)

> "The natural tendency when a stock breaks badly is to sell it. There is a reason — an unknown reason but a good reason; therefore, get out. But it is not wise to get out when the break is the result of a raid by an operator… Inverted tips!" (Ch XV)

> "I have done my share of trading and have kept fairly well posted on the stock market for many years and I can say that I do not recall an instance when a bear raid caused a stock to decline extensively. What was called bear raiding was nothing but selling based on accurate knowledge of real conditions." (Ch XXIII)

**Mechanism extracted**:
- (i) Acute cascade has a *flow-exhaustion* termination: the marginal seller runs out (Livermore's covering reversal at the 1907 bottom).
- (ii) "Bear raid" framing of a sustained decline is the inverted-tip — a *true* microstructure cascade aftermath recovers; a *fake* cascade (where the selling is informed) keeps going down. Distinguishing these is the cascade-cause classification problem.

### Kindleberger — Stage 5 Panic terminations (1978 *Manias*, Ch 2)

> "Revulsion and discredit may lead to panic (or as the Germans put it, *Torschlusspanik*, 'door-shut-panic') as investors crowd to get through the door before it slams shut." (Ch 2)

> "The panic feeds on itself until prices have declined so far and have become so low that investors are tempted to buy the less liquid assets, or until trade in the assets is stopped... or a lender of last resort succeeds in convincing investors that money will be made available in the amounts needed." (Ch 2)

> "In domestic crises, government or the central bank has responsibility to act as a lender of last resort. At the international level, there is neither a world government nor any world bank adequately equipped to serve as a lender of last resort." (Ch 2)

**Mechanism extracted**: three terminating conditions for a panic — (a) price exhaustion, (b) circuit-breaker / exchange halt, (c) credible LOLR. **Crypto has no LOLR by design**, so panics terminate on (a) or (b). This is why crypto cascade aftermaths bottom on price exhaustion and are larger in magnitude than equivalent equity cascades. (BTC -77% in 2022 vs S&P -56% in 2008 is consistent with this architectural difference.)

### Harris — Microstructure cascade vs fundamental cascade (2003 *Trading and Exchanges*, Ch 28)

> "When sellers want to sell large quantities at the same time, prices can fall very substantially. Moreover, when everyone knows that large sellers will come onto the market when prices fall, order anticipators will quickly sell at the first sign of a price drop. Likewise, buyers will withdraw from the market until prices have dropped substantially. Expectations about portfolio insurance selling therefore can drive the market down, even if no portfolio insurance selling occurs." (Ch 28, on the 1987 crash mechanic)

**Mechanism extracted**:
- 1987 = the canonical microstructure cascade — correlated leveraged positions (portfolio insurance), trigger event, reflexive selling, buy-side withdrawal in anticipation, recovery once dynamic-hedging supply is exhausted.
- **The cascade is microstructure-driven, not informational.** Recovery is fast (1987 recovered fully in 2 years; modern flash-crashes recover in minutes). The trade is the recovery.
- In contrast, **fundamental cascades** (1929 — banking-system insolvency; 2008 — solvency cascade) recover slowly because the cascade revealed information about lower asset value.
- Crypto translation: leveraged-perp liquidation cascade is the direct portfolio-insurance-cascade analog. **Crypto venues mostly lack circuit breakers, so cascades play out fully** — both magnitude and visibility are larger than in equities. See [[foundation/market-structure/bubbles-crashes-circuit-breakers]] for full Harris extraction.

### Schwager — Raschke "turtle soup" + Rogers "I short hysteria" (Market Wizards 1989, NMW 1992)

> "I short hysteria. Just about every time you go against panic, you will be right if you can stick it out." — Jim Rogers, MW 1989

> "Most 20-day breakouts in modern markets are stop-runs, not genuine trend ignitions. The reversal trade has high hit-rate when the breakout reverses within 1-3 bars." — Linda Bradford Raschke (paraphrase, NMW 1992; the Turtle Soup mechanism)

**Mechanism extracted**: failed-breakout-reversal (Turtle Soup) is an *intra-day-scale* cascade-aftermath mechanism — the breakout = stop-run = mini-cascade; the failure = aftermath; the reversal = the trade. Rogers's "short hysteria" is the *macro-scale* version — fade the panic-low only when the underlying thesis would have produced the move regardless. **Rogers's caveat is critical: most "buy panic" trades fail because there was no underlying bull thesis; the panic was correct.** This separates microstructure cascades (recover) from fundamental cascades (don't).

### Cross-source synthesis

The five sources converge on a **two-question diagnostic** for cascade aftermath:

1. **Is the cascade microstructure-driven or fundamental?** (Harris explicit; Lefèvre via "bear raid" inversion; Schwager via Rogers caveat)
2. **What terminates the cascade?** (Kindleberger explicit: price exhaustion / halt / LOLR; Lefèvre via 1907 episode: marginal seller runs out)

A trade is in the family if-and-only-if both are answerable:
- Microstructure-driven (forced flow, not new information) → recovery is the trade
- Termination by price exhaustion or halt (no LOLR in crypto) → recovery starts when the marginal forced seller is cleared

## Sample Structured Hypotheses (Family Templates)

Use these as templates when designing strategies in the family.

### Template 1 — Spring entry on confirmed cascade aftermath

```
Hypothesis:    Long entries at well-defined support levels following a
               long-side liquidation cascade with confluence (cascade
               exhaustion + recovery + on-chain accumulation during
               wick) produce above-random returns over 1-3 day windows.

Mechanism:     Per [[trading/mechanisms/accumulation-distribution|Wyckoff
               spring]]. Cascade clears stop-clustered weak holders;
               Composite Operator (or aggregate large-interest equivalent)
               accumulates during wick. Once forced selling exhausts,
               the bid returns; aftermath is recovery move.

Observable:    Liquidation cluster density at the swept level (Coinglass);
               recovery candle on equal-or-greater volume; funding flip
               from positive to deeply negative on cascade then snap-back
               to neutral; on-chain net accumulation during wick.

Expected:      Long entries on confirmed setups produce 2-5x R returns
               over 1-2 weeks; win rate 60-70% with proper confluence.

Falsifier:     Backtest BTC perps 2023-2026; setups underperform
               random-direction longs on volatility-matched assets.
```

This is fully documented at [[trading/mechanisms/accumulation-distribution]] under "Operational Specification — Spring Trade." Reference there for full detail.

**DSL primitives required (not yet implemented):** `liquidations_at_level`, `liq_cluster_density`, `oi_pct_change`, `funding_rate(symbol, exchange, window)`, `whale_netflow(cohort, direction, window)`. See [[#DSL Primitive Requirements / Capability Gap|DSL section below]] — Tier 1 (Coinglass) + partial Tier 2 (Glassnode).

### Template 2 — Cascade-cause classification trade

```
Hypothesis:    The recovery trajectory after a cascade depends on whether
               the cascade was microstructure-driven (forced-liquidation
               + leverage cascade) or fundamental (revealed information
               about reduced asset value). Microstructure cascades recover
               within weeks; fundamental cascades take months-to-years.

Mechanism:     Microstructure cascade: positions forced to liquidate
               regardless of valuation; once positions clear, valuation-
               based bid returns; price recovers most of cascade.
               Fundamental cascade: cascade reveals new information
               (counterparty insolvency, regulatory action, technology
               failure); valuation-based bid is now lower; recovery
               requires new buyers at new lower valuation.

Observable:    Cascade trigger event classification: was the trigger
               news (fundamental) or technical (microstructure)?
               On-chain whale-cohort response: net buyers (microstructure)
               or net sellers (fundamental)?
               Stablecoin supply trajectory: stable (microstructure) or
               contracting (fundamental)?

Expected:      Classification-conditioned long entry: long microstructure
               cascades aggressively; longs after fundamental cascades
               require additional confluence (multi-week base, cohort
               flow turn, narrative reset).

Falsifier:     Classification heuristic does not predict 1-month vs
               1-year recovery timing better than unconditional baseline.
```

**DSL primitives required (not yet implemented):** `event_classifier(news_feed, event_window)`, `stablecoin_supply_delta(window)`, `whale_flow_direction(cohort, exchange_dir, window)`, `funding_persistence(threshold, duration)`, `oi_rebuild_velocity(window)`. See [[#DSL Primitive Requirements / Capability Gap|DSL section below]] — Tier 2 (Glassnode) + Tier 3 (calendar/news feed).

### Template 3 — Multi-cascade exhaustion bottom

```
Hypothesis:    In sustained bear markets with multiple cascade events,
               the final cascade (after which no further panic-send
               occurs from major cohorts) marks the cycle bottom and
               produces above-random forward returns over multi-month
               windows.

Mechanism:     Each cascade exhausts a layer of weak holders. After the
               final cascade, no marginal seller exists at distressed
               prices; weak hands have already capitulated. Bid resumes
               from durable cohorts (LTH, accumulators).

Observable:    Cascade sequence count over 12-month window; on-chain
               panic-send peak-then-exhausts; LTH cost basis stops
               compressing; STH realized loss ratio peaks then declines;
               miner capitulation event coincident.

Expected:      Long entries after confirmed multi-cascade exhaustion
               produce above-random 3-12 month returns. (Multi-week
               accumulation-base required; not a 1-day trade.)

Falsifier:     Multi-cascade-exhaustion-flagged longs do not outperform
               unconditional longs at the same prices over walk-forward
               12-month windows.

Caveat:        Hard to validate in real time; mostly a regime-classifier
               for swing-trade horizon entries.
```

**DSL primitives required (not yet implemented):** `cascade_event_count(window)`, `lth_cost_basis(time_t)`, `sth_realized_loss_ratio(window)`, `miner_capitulation_signal(window)`. See [[#DSL Primitive Requirements / Capability Gap|DSL section below]] — Tier 2 (Glassnode load-bearing).

## Historical Case Studies — Crypto Cascades

Six canonical crypto cascades, each annotated with cascade trigger, magnitude, recovery profile, and mechanism classification (per the two-question diagnostic above). These ground the operational thresholds defined in the next section.

### 1. March 12-13, 2020 — COVID liquidity vacuum

| Field | Value |
|-------|-------|
| **Trigger** | Global risk-off as COVID lockdowns escalate; equity vol spike; cross-asset deleveraging |
| **Magnitude** | BTC -52% in 36 hours ($7,900 → $3,800 on March 12-13). $1.2B+ in liquidations on BitMEX |
| **Microstructure trigger** | BitMEX engine hit ~$700M in long liquidations in <60 minutes; mark-price-vs-spot dislocation reached >5%; rumors of BitMEX engine halt for "DDoS" added to flow vacuum |
| **Time to recovery** | ~2 months back to pre-cascade ($7,000 by mid-May 2020) |
| **Classification** | **Microstructure cascade** with cross-asset trigger (recovery-the-trade) |
| **Aftermath signatures** | Funding rate deeply negative for 3-5 days post-cascade; OI rebuilt over 4-6 weeks; spot premiums to perps emerged for first time |

### 2. May 19, 2021 — China crackdown + leveraged purge

| Field | Value |
|-------|-------|
| **Trigger** | Sequence of negative China headlines (mining ban, payment-channel restrictions); funding rates were extreme positive going in (>+80bp/8h sustained for 6+ weeks) |
| **Magnitude** | BTC -30% in single 24h candle ($43K → $30K intraday low). $8B+ in liquidations across exchanges (record at the time) |
| **Microstructure trigger** | Cascade clearing of clustered long liquidations between $42K and $36K visible on Coinglass heatmap pre-event |
| **Time to recovery** | Lower-low retest end-July 2021 ($29K); full recovery to pre-event by Aug 2021; new ATH Nov 2021 |
| **Classification** | **Mixed** — microstructure-cascade aftermath (immediate $7K recovery within 24h), but with fundamental aftershock (China ban was real new information) |
| **Aftermath signatures** | Funding flipped from +60bp to -30bp in 8 hours; on-chain whale wallets net accumulated during wick (Glassnode); STH cost basis stopped compressing |

### 3. May-June 2022 — Terra/UST collapse + 3AC contagion

| Field | Value |
|-------|-------|
| **Trigger** | Algorithmic stablecoin death spiral (UST → $0.01 over 7 days, May 9-15); contagion to 3AC (June), Celsius (June 12), Voyager (July) |
| **Magnitude** | Multi-stage: BTC $40K → $26K in May (UST); $26K → $17K in June (3AC). Total -57% over 60 days |
| **Microstructure trigger** | Algorithmic-stable mechanism failure (fundamental, not microstructure); subsequent cascades were forced selling by insolvent funds |
| **Time to recovery** | NONE in 2022 — bear continued to Nov bottom. Eventually recovered Oct 2023 (~$30K), 18 months |
| **Classification** | **Fundamental cascade** (Rogers's "panic was correct" case). Naive microstructure-aftermath trade would have failed; the underlying thesis change was real |
| **Aftermath signatures** | Funding stayed negative for 6+ weeks (atypical); OI reset to pre-2021 levels; multi-cascade exhaustion sequence began (UST → 3AC → Celsius → FTX) |

### 4. November 8-11, 2022 — FTX collapse

| Field | Value |
|-------|-------|
| **Trigger** | CoinDesk Alameda balance-sheet leak (Nov 2) → CZ tweets (Nov 6) → bank run (Nov 7-8) → halt withdrawals (Nov 8) → bankruptcy (Nov 11) |
| **Magnitude** | BTC $20K → $15.5K in 5 days. Bottom Nov 21, 2022 ($15.5K). Total cycle drawdown -77% from $69K |
| **Microstructure trigger** | Counterparty-default cascade (FTX customer balances unrecoverable); contagion to Genesis, BlockFi, DCG |
| **Time to recovery** | 8 weeks until cycle bottom (Nov 21, 2022); first leg of recovery Q1 2023 ($15.5K → $25K) |
| **Classification** | **Fundamental cascade** at the cycle level, but the specific Nov 8-11 acute phase had microstructure-cascade-aftermath properties (immediate $4-5K bounce off $15.5K — Template-3 multi-cascade-exhaustion signature visible) |
| **Aftermath signatures** | OI collapsed to multi-year lows; funding negative-but-stable (no longs left to liquidate); whale on-chain net accumulating Nov 21 wick — cycle bottom marker |

### 5. March 10-12, 2023 — SVB / USDC depeg flash event

| Field | Value |
|-------|-------|
| **Trigger** | Silicon Valley Bank failure → Circle's USDC reserves at SVB ($3.3B) frozen → USDC briefly de-pegs to $0.87 |
| **Magnitude** | USDC -13% briefly; BTC initially -5% then *rallied* +30% in 6 days as flight from USD-banking risk |
| **Microstructure trigger** | Stablecoin reserve uncertainty + weekend market closure (TradFi) creating mispricing window |
| **Time to recovery** | USDC peg restored within 36h after FDIC SVB resolution announcement; BTC continued rally through to ~$30K |
| **Classification** | **Microstructure cascade** (reserves were intact; mispricing was forced-selling-flow); aftermath was the trade |
| **Aftermath signatures** | DAI/USDT spread to USDC widened 1000bp briefly; BTC funding flipped negative as cascade began, then strongly positive as rally took hold (regime flip in 48h) |

### 6. August 5, 2024 — Yen carry unwind global flush

| Field | Value |
|-------|-------|
| **Trigger** | BoJ rate hike + Friday weak NFP → yen carry trade unwind cascade across global markets (Nikkei -12% Aug 5; cross-asset deleveraging) |
| **Magnitude** | BTC -19% in 24h ($65K → $52K); ~$1B in crypto liquidations (lower than 2021/22 events due to lower aggregate leverage entering) |
| **Microstructure trigger** | Cross-asset margin calls forced crypto deleveraging despite no crypto-specific catalyst |
| **Time to recovery** | Pre-cascade level reclaimed within 3 weeks (Aug 26 ~$65K); full recovery in 6 weeks |
| **Classification** | **Microstructure cascade with cross-asset trigger** (cleanest case — trigger was external, no informational change for crypto, recovery was direct) |
| **Aftermath signatures** | Funding briefly negative but flipped positive within 48h; OI rebuilt fast (vs slow rebuilds in fundamental cascades) |

### 7. October 10-11, 2025 — ETF-era $19B tariff-triggered deleveraging cascade

Paper-grade case study (Ali 2025 SSRN paper "Anatomy of the October 10–11, 2025 Crypto Liquidation Cascade", Zeeshan Ali, University of Central Asia, Khorog, Tajikistan; working paper Oct 16 2025, SSRN abstract ID 5611392, ingested via user-supplied PDF 2026-05-15 PM).

| Field | Value |
|-------|-------|
| **Trigger** | **Trump 100% tariff on Chinese imports** (effective Nov 1 2025), in response to China's rare-earth export restrictions. Posted to Truth Social ~2:00 p.m. ET Oct 10 — the **decisive catalyst** that converted a correction into a systemic deleveraging event |
| **Magnitude** | **~$19B liquidations affecting >1.6M traders** within 36 hours; BTC −14% to $104,782; ETH −15% to $3,447; S&P 500 −2.7% intraday. BTC alone accounted for ~$2.5B of the liquidation total. **$7B in hourly liquidations peak** in the post-tariff-announcement window |
| **Microstructure trigger** | Macro shock interacted with the ETF-era leverage stack: Bitcoin OI peaked at $54.7B during 2025 (82% above year-start $30B — largest aggregate leverage in BTC history). Algorithmic liquidations propagated the macro shock through the long-stack. Per Ali §4.1: BTC conditional variance peaked at **0.10 shortly after 2:00 p.m. ET** — direct empirical confirmation of the tariff-announcement → vol-spike → cascade chain |
| **Macro context** | 10-year Treasury yield 4.05%; DXY +0.4% week-over-week; declining stablecoin liquidity from regulatory pressure. Conditions made crypto leverage particularly exposed to tightening financial conditions |
| **Time to recovery** | Multi-week partial recovery; full pre-cascade level NOT reclaimed within 30 days (distinguishes from clean microstructure cascades like Aug 2024 which recovered in 3 weeks) — suggests partial-fundamental component (macro-regime shift was real, not just margin-call mechanic) |
| **Classification** | **Mixed microstructure + macro-fundamental cascade** — primary mechanic was forced deleveraging (microstructure) but the trigger was a regime-relevant macro signal that didn't fully unwind, putting this case at the boundary of Harris's two-category framework |
| **Aftermath signatures** | Funding rates briefly extreme negative as forced-cover unwound longs (consistent with positioning-extreme fade-the-extreme being LESS reliable in ETF-era per [[etf-era-carry-trade-funding-distortion]] — the negative funding was institutional cash-and-carry forced-unwind, not retail-bearish-positioning); OI rebuild slower than 2024 microstructure case (consistent with partial-fundamental classification) |
| **Crypto-specific structural lesson** | **Concentration in perpetual futures created systemic risk** even though aggregate leverage was *better collateralized* than 2021-22. The lesson is: collateralization-improvement alone doesn't prevent cascades when the *venue concentration* (single-asset-class perp) creates correlation across notionally-independent participants. ETF cash-and-carry arbitrage couples the spot and perp markets mechanically — a forced unwind in one propagates to the other |

#### Quantitative econometric findings (Ali 2025)

The paper provides paper-grade empirical confirmation across 10 cryptocurrencies (BTC, ETH, BNB, SOL, XRP, DOGE, ADA, TRX, TON, AVAX) using GARCH/EGARCH/DCC-GARCH/VAR models. Sample window: Sep 1 - Oct 15, 2025 (45 daily observations).

**GARCH(1,1) volatility persistence — average α + β ≈ 0.92** (Student-t-corrected 0.925; Table 3):

| Symbol | ω | α | β | α+β |
|---|---:|---:|---:|---:|
| BTC | 0.012 | 0.105 | **0.815** | **0.920** |
| ETH | 0.016 | 0.115 | **0.805** | **0.920** |
| SOL | 0.018 | 0.120 | **0.810** | **0.930** |
| BNB | 0.014 | 0.110 | 0.812 | 0.922 |
| DOGE | 0.017 | 0.125 | 0.795 | 0.920 |
| AVAX | 0.018 | 0.130 | 0.786 | 0.916 |

**All p < 0.05; t-stats on β range 14.5-15.4** — highly significant volatility clustering. Persistence near 0.92 confirms long-lasting shocks; BTC, ETH, SOL emerge as primary volatility transmitters in the network.

**Post-tariff realized volatility more than doubled** relative to pre-event window.

**Cross-asset contagion (DCC-GARCH, §4.3)**:

| Period | Average inter-asset correlation |
|---|---:|
| Pre-event | 0.55 |
| Post-event | **0.89** |
| BTC-ETH peak (within 12 hours) | **0.91** |

**15% stronger contagion than 2018 trade war** (Autor, Dorn, Hanson 2020 baseline). Johansen/ADF tests confirmed no long-run cointegration → **short-term volatility transmission dominates**.

**Event-study regression (Table 4) — tariff dummy on BTC volatility**:

| Variable | Coefficient | t-stat | p-value |
|---|---:|---:|---:|
| **Tariff Dummy** | **0.62** | **4.5** | **<0.001** |
| DXY | 0.10 | 2.6 | 0.009 |
| 10-Year Yield | 0.07 | 1.8 | 0.072 |
| Constant | 0.01 | 1.0 | 0.317 |

R² = 0.52, N = 45 days. **Placebo test on Oct 1-7 period**: β = 0.02, p = 0.52 — confirms causality (no spurious effect pre-event).

**VAR(2) Granger causality**: tariff Granger-caused volatility changes at **p = 0.002**. The macro shock is statistically established as the volatility driver, not coincidental timing.

**Per-asset peak liquidations** (Table 2, Sep 1 - Oct 15 max-day, billions USD):

| Symbol | Peak liq (B$) | Mean OI (B$) | Mean vol (M) | Mean funding (%) | Daily vol (%) |
|---|---:|---:|---:|---:|---:|
| BTC | **9.2** | 6.8 | 3,200 | -0.008 | 4.2 |
| ETH | **4.8** | 3.0 | 1,700 | -0.010 | 4.8 |
| SOL | 2.3 | 0.7 | 1,100 | -0.013 | 6.5 |
| XRP | 1.4 | 0.5 | 850 | -0.009 | 5.2 |
| DOGE | 1.1 | 0.4 | 750 | -0.016 | 6.8 |
| ADA | 0.9 | 0.3 | 650 | -0.008 | 4.7 |
| TRX | 0.8 | 0.3 | 550 | -0.011 | 4.5 |
| TON | 0.7 | 0.2 | 450 | -0.010 | 5.3 |
| AVAX | 0.9 | 0.4 | 600 | -0.014 | 5.9 |
| BNB | 1.2 | 0.6 | 900 | -0.007 | 4.0 |

BTC + ETH together account for ~$14B of the $19B total — **~74% of cascade liquidations concentrated in the 2 largest cryptocurrencies**. Smaller cryptos (DOGE, ADA, TRX, TON, AVAX) contribute incrementally but the macro-shock-propagation pattern was BTC/ETH-dominant.

#### Pre-registered detector validation

Wiki's cascade-event detector (≥3-of-5 from: liq vol ≥$200M/4h, OI delta ≥10%/4h, funding swing ≥40bp/8h, price ≥1.5×ATR, vol ≥3×trailing) maps onto this event:

| Threshold | Oct 10-11 evidence | Result |
|---|---|---:|
| Liq vol ≥$200M/4h | $7B/hour peak = $28B/4h peak; **140× threshold** | ✓ |
| Price ≥1.5×ATR | BTC -14% in one day vs ~3% daily ATR = **~4.7× ATR** | ✓ |
| Vol ≥3×trailing | Conditional variance peaked at 0.10, more than doubled vs pre-event | ✓ |
| OI delta ≥10%/4h | $19B/$54.7B OI = 35% over 36h; sustained large drops | ✓ |
| Funding swing ≥40bp/8h | Funding turned briefly extreme negative; magnitude per paper not specified | (likely ✓) |

**4-5 of 5 thresholds breached** → detector fires definitively. The Oct 10-11 cascade is a **clean detector worked example**.

#### Author's policy recommendations (Ali §5)

Aligned with Financial Stability Board (FSB) 2024 recommendations:
1. **Dynamic margin systems** — collateral requirements adjust automatically with real-time volatility
2. **Cross-exchange circuit breakers** synchronized through inter-exchange protocols
3. **Tariff-Liquidity Index (TLI)**: `TLI_t = β₁·ΔTariff_t + β₂·ΔLiquidity_t + ε_t` — proposed geopolitical-risk monitor

**Wiki note**: the TLI proposal is forward-looking methodology; not yet empirically validated outside the Oct 2025 case. Worth tracking as candidate diagnostic if multi-cascade applicability surfaces. Could anchor a future macro-shock-cascade-detector mechanism page if N≥2 cases warrant it.

### Cross-case patterns (extended to N=7 with Oct 10-11 2025)

- **Microstructure cascades recover within weeks-to-months** (March 2020, May 19 2021 day-cascade, March 2023 SVB, Aug 2024). **Fundamental cascades take months-to-years** (Terra/3AC, FTX). The Harris diagnostic is empirically validated across the original N=6.
- **NEW with N=7 (Oct 10-11 2025): a third category — mixed microstructure + macro-fundamental cascade.** The trigger is regime-relevant macro (not just margin-call mechanic) but the primary unwind is mechanical. Recovery time is intermediate — slower than clean microstructure (Aug 2024) but faster than pure fundamental (Terra/FTX). The two-category Harris framework (microstructure vs fundamental) is **empirically validated for ~85% of cases** but the boundary case (Oct 2025) needs explicit third-category treatment. Practical implication: in real-time classification, "mixed-cascade-pending-fundamental-resolution" should be a holding category rather than forcing premature binary classification.
- **Aftermath duration scales with trigger magnitude AND fundamental component**. May 19 2021 was both — partial microstructure (longs cleared) + partial fundamental (China ban) → short-term recovery was real but lower-low retest occurred. Oct 10-11 2025 fits the same pattern (mixed; partial recovery; full reclamation pending macro resolution).
- **No-LOLR architecture confirmed empirically**: in every fundamental cascade, no rescue arrived. FTX customer funds were not socialized. Terra holders were not made whole. **The "wait for rescue" trade does not exist in crypto** (Kindleberger insight operationalized).
- **Aftermath signatures are visible across all cases**: funding rate flip + OI collapse + on-chain whale flow + (when cycle bottom is reached) STH cost-basis compression stops. These are the operational signatures defined numerically below.
- **ETF-era scale lesson (NEW with N=7)**: 2025's $19B Oct cascade + $16.7B Sep cascade are the **two largest crypto-derivatives stress events in history**, despite better collateralization than the 2021-22 cycle. The lesson is that **venue concentration** (perp-futures-dominant) creates correlated cascade-risk across notionally-independent participants — improved-margin-discipline at the individual-account level doesn't prevent system-level cascades when the venue mix concentrates exposure. Cross-link to the [[etf-era-carry-trade-funding-distortion]] thesis: institutional cash-and-carry mechanically couples spot and perp markets, so a forced-unwind in either propagates immediately to the other. Capacity-decay (per [[foundation/sources/he-manela-fundamentals-perpetual-futures|He, Manela et al.]] 11%/yr deviation decay) doesn't eliminate cascade-risk; it just narrows arbitrage spreads in non-cascade conditions.

## Empirical evidence (2026-05-08 backtest)

### Mechanism family rescued at multi-sub operational form

**Prior failure (single-strategy form).** [[trading/rejected/wyckoff-spring-4h-2026-05|`wyckoff_spring_4h`]] was REJECTED in the 2026-05-05 deployment-grade stocktake — medSh −1.77; %PF>1 10.4%; win rate 27.3%; net −$153/yr at $1K. Verdict at the time: mechanism real (5-source convergence: Wyckoff + Lefèvre + Kindleberger + Harris + Schwager all converge on cascade-aftermath as tradable); operational form fails (4h price+volume only insufficient to distinguish real cascades from noise wicks); capability gap (Coinglass liquidation cluster + Glassnode whale flow) is the bottleneck, not the mechanism.

**Subsequent test (multi-sub triangulation).** `meta_post_cascade_v1` (2026-05-08) tested whether **ATR-extreme + price-Z + bar-reversal proxies** distributed across 3 mutually-exclusive sub-strategies could capture the mechanism without Coinglass dependency. The three subs:

- `vol_spike_reversal` — vol > 75th percentile + price-Z > ±2.5σ + reversal-bar trigger
- `bearish_capitulation_long` — −3 ATR move within 24h + bullish bar follow-through
- `bullish_exhaustion_short` — +3 ATR move within 24h + bearish bar follow-through

**Result on 500-config grid** (BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital, fee 0.04%):

| Metric | Value |
|---|---:|
| Cohort full-positive ratio | **98.4% of 500** |
| Cohort median P&L | +$67 |
| Cohort max | +$122 |
| Mean Sharpe | +2.03 |
| Median win rate | 37% |
| Realized R:R | 4–5:1 (mean-reversion family default per [[trading/mechanisms/universal-stop-target-prior]]) |
| short_pnl mean | +$58 |
| long_pnl mean | +$7 |

The directional asymmetry — short-side fills carry; long-side contributes marginally — is consistent with the 2025-26 bearish-dominant regime per [[foundation/macro/regime-conditional-edge-2025-26]]; the family-level mechanism survives the regime but the long-side sub-mechanism appears regime-quiet.

**Honest-scope caveat: per-sub attribution is inferred, not measured.** The "short-side carries; long-side contributes marginally" framing is inferred from aggregate `long_pnl` vs `short_pnl` columns across the cohort, *not* from per-trade attribution to specific sub-strategies (`vol_spike_reversal` vs `bearish_capitulation_long` vs `bullish_exhaustion_short`). The engine's grid_results.csv does not currently carry a `sub_strategy` column — per-trade sub-attribution is an explicit DSL/engine-output blocker on a team engineer's track (per `this research program` §1 "engine output layer"). Until that column lands, hypothesis-level falsifiers like *"per-sub ATP retention ≥ 80% of standalone"* cannot be evaluated for any meta-strategy. The per-sub split here is suggestive; treat as cohort-level evidence pending per-trade attribution.

### Mechanism-family-status updated

The wiki's 2026-05-05 verdict on the family ("mechanism real; capability gap blocks single-strategy form") **understated the operational tractability**. Coinglass dependency is no longer load-bearing for *some* edge — multi-sub triangulation of ATR-extreme + price-Z + bar-reversal proxies recovers cohort-wide edge without on-chain data. Coinglass would sharpen the edge (discriminate "real cascade" from "noise wick"; reduce false-positive entries) but is no longer required for *any* edge.

**The diagnostic finding is the operational form matters.** Single-proxy operationalization (price wick + volume only at 4h) fails to distinguish cascades from noise; triangulation across multiple weakly-correlated cascade proxies recovers the signal even without the load-bearing primitive. This is a useful pattern for other capability-blocked mechanism families — multi-proxy triangulation as a salvage path when the canonical primitive is unavailable.

### Anti-predictive ranking on a working cohort — sample-heterogeneity signature

> **⚠️ Updated 2026-05-13 — this section's "untunable" verdict was a split-timing artifact.** See §"Verdict reversal (2026-05-13 update)" below. The 1y window's 70/30 train/val split fell on a regime boundary between bearish-Q4-2025 and post-Jan-2026 recovery; parameter optima legitimately shifted between halves; corr read anti-predictive; cohort was real. 2y multi-cycle re-run resolves the artifact (corr +0.593; productive zone [stop 0.9-1.5] cleanly tunable). Original framing below preserved for the methodology-learning context. See also [[foundation/methodology/path-quality-diagnostics#split-timing|path-quality-diagnostics §2.3 split-timing scrutiny]] — meta_post_cascade is the canonical worked example for that diagnostic.

train→val rank correlation on the 500-config grid: **−0.598**. Quantile transfer Q1 → Q5 maps to val mean $73 → $29 (strict monotone DECREASING). Train-rank-based config selection actively destroys ~60% of val edge on a cohort that is otherwise 98.4% full-positive.

**Original interpretation (2026-05-08).** The 2025-26 window contains ~5-10 cascade events total per the case-study cadence above; train-half (May 2025 → Jan 2026) and val-half (Jan-Apr 2026) draw from different cascade events with different microstructure-vs-fundamental profiles. Parameter optima shift between halves because the underlying event distribution is non-stationary. The cohort IS positive (mechanism is real); the ranking signal is broken because the parameter-optimum within the cohort isn't stable across halves.

**This interpretation is RESCINDED as of 2026-05-13** — see verdict reversal section below. The sample-heterogeneity framing isn't wrong in general, but in this specific case the heterogeneity was concentrated on the regime-boundary itself; once the window extends through multiple cycles, the parameter-optimum stabilizes and corr recovers.

The structural distinction from broken-cohort anti-predictive ranking remains valid: this strategy's anti-predictive corr was on a 98.4%-positive cohort, structurally distinct from [[trading/rejected/meta_session_v1-2026-05|meta_session_v1]]'s broken-cohort signature (universally-negative cohort + extreme anti-predictive corr −0.915 on noise). Cohort-positive ratio is what discriminates *sample-heterogeneity* from *no-edge*. The methodology lesson holds; the specific deployment verdict for meta_post_cascade does not.

### Deployment policy implications

> **⚠️ Updated 2026-05-13 — the "do not tune by train P&L" advice was based on the split-timing artifact.** See verdict reversal below. The 2y multi-cycle re-run shows corr(train,val) = +0.593 — train-rank parameter selection IS safe on the longer window. The locked single-config deployment candidate `meta_post_cascade_btc_prod_v1` (stop=0.9444, target=3.78, risk=0.01) was selected from the 2y grid top region using train-rank-based selection.

Original 2026-05-08 deployment-policy guidance below preserved for context; **do not act on it** for the 2y window. The robust-defaults recommendation is no longer load-bearing once the productive zone is identified.

- **(Rescinded 2026-05-13) Do NOT tune by train P&L.** Train-rank-based parameter selection destroys val edge. Deploy at robust mid-cohort defaults (median-of-cohort parameter config across the sweep) rather than train-top-ranked configs.
- **The cohort is positive at robust defaults.** Cohort median +$67 and Mean Sharpe +2.03 at 98.4% cohort full-positive — the strategy is deployment-grade at family-default parameters per [[trading/mechanisms/universal-stop-target-prior]]. *(Holds independent of the split-timing artifact.)*
- **(Rescinded 2026-05-13) Per-cascade-event hit-rate variance is the binding uncertainty**, not parameter selection. Until the sample-size grows (multi-year window or cross-asset extension), the strategy is research-grade at the cohort level; reasonable to deploy at median-config size if treated as one mechanism in a diversified set rather than as a single capital-concentrated bet.

### Capability gap reinforcement

This finding is the strongest argument in the wiki for **Coinglass liquidation cluster data** (capability roadmap #3). A dedicated cascade-event flag would let the strategy:

1. **Condition on actual liquidation events** rather than ATR proxies — likely shrinks the false-positive rate (proxies fire on news-volatility wicks that aren't real cascades; cluster data would discriminate)
2. **Stabilize the parameter optimum across train/val halves** — if the gating signal is event-aware rather than proxy-aware, the parameter optimum shouldn't drift with cascade-event distribution shifts
3. **Sharpen the cascade-cause classification** (Template 2) — currently classification is impossible at the cluster-volume level; Coinglass enables it

The Tier-1 DSL extension list in §"DSL Primitive Requirements / Capability Gap" below is now empirically motivated, not just theoretically argued.

## Empirical evidence (2026-05-13 update) — verdict reversal on 2y multi-cycle window + first per-sub PASS

### Verdict reversal — the prior "anti-predictive ranking, untunable" was a split-timing artifact

The 2026-05-08 grid was a 1y window (2025-05-03 → 2026-04-28) with a 70/30 train/val split. The split timestamp fell on a regime boundary between bearish-Q4-2025 and the post-Jan-2026 recovery. Parameter optima legitimately shifted between halves — but the cohort was positive in both halves; the mechanism was real in both regimes; the corr read as anti-predictive because the parameter-optimum tracked the regime.

The 2y multi-cycle re-run (2024-05-18 → 2026-05-08, spanning multiple regime cycles) dissolves the artifact:

| Metric | 1y grid (2026-05-08) | 2y multi-cycle re-run (2026-05-13) |
|---|---:|---:|
| Window | 2025-05-03 → 2026-04-28 (1y) | 2024-05-18 → 2026-05-08 (2y) |
| Cohort full-positive ratio | 98.4% | **87.4%** |
| corr(train, val) | **−0.598** (anti-predictive) | **+0.593** (cleanly predictive) |
| Top config Sharpe | — | **2.54** |
| Top config max_dd | — | **11.4%** |
| Top config net_pnl | — | **+$269 / 2y on $1K** |
| Productive parameter zone | (no stable zone — split-timing artifact) | **stop ∈ [0.9, 1.5] cleanly tunable** |

This is the wiki's first explicit case of a **closure verdict reversed by window extension**. The 2026-05-08 deployment-policy guidance (`Do NOT tune by train P&L`) and the framing of "Per-cascade-event hit-rate variance is the binding uncertainty" were both downstream of the split-timing artifact. With the 2y window, both are rescinded — train-rank parameter selection is safe; the binding uncertainty shifts to path-quality (event-concentration mid-band caveat — see next sub-section).

Methodology generalization: **split-timing scrutiny** as a standard discipline — when corr(train,val) reads anti-predictive on a working cohort (cohort-positive ≥ 80%), check whether the train/val split falls near a regime boundary before closing the verdict. See [[foundation/methodology/path-quality-diagnostics#split-timing|path-quality-diagnostics §2.3]] for the discipline + meta_post_cascade as canonical N=1 worked example (watching for N=2 recurrence before promoting to standard filter).

### Lead deployment candidate — `meta_post_cascade_btc_prod_v1`

Per this research program (2026-05-13), the 2y multi-cycle re-run identified a locked single-config from the top region:

| Param | Value |
|---|---|
| `stop_atr_mult` | 0.9444 |
| `target_atr_mult` | 3.78 |
| `risk_pct` | 0.01 |
| Window | 2y (2024-05-18 → 2026-05-08) |
| Sharpe | **2.54** |
| max_drawdown | **11.4%** |
| net_pnl on $1K | **+$269** |
| Cohort-positive at top region | 87% |

This is the lab's **lead deployment candidate as of 2026-05-13** — selected from the 500-config 2y grid top region; structurally-distributed edge (per the 87% cohort positivity at the top region — distinct from the v6 vol-regime lottery-shape per [[trading/mechanisms/vol-regime-transition-tradeable#v6-batch|vol-regime-transition-tradeable §v6 walk-back]]).

### First per-sub PASS in lab history (cell-carving falsifier resolves)

The prod_run on the locked config produces per-trade `sub_strategy` tagging (prod_run mode emits this column; grid-run does not). The 71-trade prod_run breaks down across the 3 subs as:

| Sub | Trades | Total P&L | Avg P&L | Win Rate | Contribution % |
|---|---:|---:|---:|---:|---:|
| vol_spike_reversal | 33 | +$158.18 | $4.79 | 30.3% | 58.9% |
| bearish_capitulation_long | 17 | +$57.80 | $3.40 | 41.2% | 21.5% |
| bullish_exhaustion_short | 21 | +$52.73 | $2.51 | 38.1% | 19.6% |
| **Total** | **71** | **+$268.71** | — | — | **100%** |

**All 3 subs net-positive on their own gates → cell-carving filter PASSES.** First empirical PASS in lab history; the discipline is falsifiable AND can pass.

The 2026-05-08 honest-scope caveat (per-sub attribution inferred from aggregate `long_pnl` vs `short_pnl` columns, not measured per-trade) is **resolved for this strategy** — prod_run mode unblocks per-trade attribution without waiting for a team engineer's grid-run scope. Retroactive grid-level analysis on `meta_post_cascade_v1` and other v1+v2 metas waits on the a team engineer grid-run scope, but the lead deployment candidate clears the per-sub filter today.

See [[trading/mechanisms/scaffold-as-mechanism#per-sub-pass-2026-05-13|scaffold-as-mechanism §per-sub PASS]] for the cell-carving discipline + N=2 cosmetic-failure worked examples (v6 BTC and SOL prod), and [[foundation/methodology/path-quality-diagnostics#meta-post-cascade-first-per-sub-pass-mid-band-concentration-caveat|path-quality-diagnostics §3.2]] for the full path-quality audit on meta_post_cascade.

### Mid-band event-concentration caveat — `ship` with event-risk caveat, not full `ship`

Per the 2026-05-13 path-quality audit: Top-5 trades = 84.3% of total P&L. NOT cleanly distributed-edge.

Per [[foundation/methodology/path-quality-diagnostics#event-concentration|path-quality-diagnostics §2.1]] event-concentration thresholds: 60–100% range is the high-concentration band — `ship-research-grade-only` recommended; the strategy clears 3 of 4 Move-3 filters (per-sub PASS ✓; split-timing scrutiny ✓; val-period dispersion N/A on the 2y revival window) but mid-band concentration is the binding caveat.

**Deployment posture**: `ship` with event-risk caveat. The Move-3 regime-suite-preference filter (per research process the research arms) currently caps single-mechanism at `ship-research-grade-only` regardless; full `ship` would require v2 architecture (regime-conditioning built in + N≥3 regime-conditioned val tests) OR explicit 1-cell-v2-override. Final verdict pending the prod_run with event-concentration filter applied (per substrate, the trader-action task the research task queue carries this gate).

### Side-asymmetry sub-finding

Per prod_run aggregate: long_pnl = $82, short_pnl = $186 (short side carries 69% of edge despite only 15% more trades). Consistent with the 2025-26 ETF-era directional asymmetry per [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — even on a non-funding mechanism family, the directional asymmetry of the 2024-2026 window manifests. Suggests a regime-conditioning layer: the cascade-aftermath mechanism has the same 2024-2026-era asymmetry as the funding family, hinting at a common upstream driver (institutional flow shape during the ETF era). Worth investigating cross-family.

## Operational Thresholds — Numerical Confirmation Criteria

Cascade-aftermath setups need quantitative confirmation thresholds, not just qualitative description. The following thresholds derive from the case-study empirical patterns above and from carrier signatures in [[foundation/market-structure/crypto-carrier-features]]. Pre-registered for falsifier-honesty per [[foundation/methodology/strategy-validation-discipline]] §2.1.

### Cascade event detection

A cascade event is **confirmed** when ≥3 of the following are simultaneously true within a 4-hour window:

| Signature | Threshold | Evidence source |
|-----------|-----------|-----------------|
| **Liquidation volume** | ≥$200M aggregate liquidations on a single side within 4h (BTC perps; scale by symbol OI for alts) | Coinglass per-exchange feed |
| **OI delta** | Open interest drops ≥10% within 4h | Coinglass / per-exchange OI |
| **Funding rate magnitude swing** | ≥40bp swing in 8h funding (e.g., +30bp → -10bp) | Per-exchange funding (Binance, Bybit, OKX) |
| **Price magnitude** | ≥1.5× ATR(14, 4h) in single 4h bar | OHLC data |
| **Volume magnitude** | ≥3× trailing 20-bar 4h volume | Spot + perp aggregate |

Calibrated against case studies: March 2020 (5/5 trigger), May 2021 (5/5), Terra/3AC May 2022 (3/5 — mostly funding + OI; not a 4h-bar event), FTX Nov 2022 (4/5 across cumulative day window), SVB March 2023 (3/5 — micro-event, smaller magnitude), Aug 2024 (4/5).

### Cascade aftermath confirmation (entry trigger)

After cascade event, a **tradable aftermath** requires ≥2 of:

| Signature | Threshold | Notes |
|-----------|-----------|-------|
| **Recovery candle volume** | Recovery 4h candle volume ≥1.0× cascade-bar volume | Wyckoff Effort vs Result; less volume = supply still present |
| **Funding rate snap-back** | Funding rate moves toward neutral within 16h post-cascade (e.g., -50bp → -10bp) | If funding stays extreme, more shorts piling on; cascade not done |
| **On-chain whale net flow** | Net accumulation by >1K BTC wallets during cascade wick (Glassnode entity-adjusted) | Composite-Operator analog; absent or reversed = fundamental cascade signature |
| **STH cost basis** | Short-term-holder realized loss peaks then declines within 1-2 weeks | Cycle-bottom signature; not single-event |

### Cascade-cause classification (microstructure vs fundamental)

| Signal | Microstructure cascade | Fundamental cascade |
|--------|------------------------|---------------------|
| **Trigger event class** | Forced-flow (margin calls, options expiry, stop-cluster sweep, oracle dislocation, cross-asset margin) | Information event (counterparty fail, regulatory action, technology breakage, fraud reveal) |
| **Whale flow during wick** | Net accumulation | Net distribution / no flow |
| **Stablecoin supply** | Stable or expanding | Contracting (redemptions) |
| **Recovery speed** | Recovers ≥50% of cascade in <72h | <30% recovery in 72h; multiple lower lows |
| **Funding mean-reversion** | Within 24-48h | Sustained extreme for >5 days |
| **OI rebuild** | Rebuilds within 1-2 weeks | Stays at or below post-cascade level for 4+ weeks |

**Operational rule:** if ≥3 signals point to microstructure → trade aftermath aggressively; if ≥3 point to fundamental → wait for multi-week base before any long. Mixed cases (e.g., May 19 2021) → reduced size + tighter time-stops + lower-low retest tolerance built in.

### Position-sizing implications

Per [[foundation/methodology/deployment-edge-threshold]], cascade-aftermath trades have:
- **Higher edge magnitude per trade** (recovery move can be 3-10% in 24-72h on confirmed microstructure cascades) — supports passing the friction floor that defeats most 0.3-1% per-trade strategies
- **Lower trade frequency** (cascade events are 2-8 per year per major symbol per the case-study cadence) — limits annualized return regardless of per-trade edge
- **Variable hit rate by classification** (microstructure ~65-75% per case-study aftermath patterns; fundamental ~30-40% for naive aftermath trade — fundamentally biased to wait)

→ **Family is best-suited to size-large, trade-rare, classification-disciplined deployment.** Worst-suited: size-small, trade-frequent, classification-naive (will repeatedly catch falling knife on fundamental cascades — the May/June 2022 sequence is the canonical caution).

## Crypto Carriers (per [[foundation/market-structure/crypto-carrier-features|carrier-features catalog]])

This family is **the most crypto-uniquely-tradable family** because cascade visibility is significantly better than in TradFi:

| Carrier | Role in family |
|---------|---------------|
| **Coinglass liquidation maps** | Real-time cluster visibility; defines cascade trigger zones AND cascade-target levels |
| **Liquidation feed (per-exchange)** | Cascade event detection in real time |
| **Funding rate** | Cascade aftermath signature: deep negative on long-cascade, deep positive on short-squeeze |
| **Open interest** | Cascade collapses OI; aftermath rebuild signals direction |
| **On-chain whale wallets** | Cascade aftermath cohort classification: who is buying/selling |
| **Stablecoin supply** | Macro-scale aftermath: stable supply persistence vs contraction distinguishes microstructure vs fundamental cascade |

Per [[foundation/sources/kindleberger-manias-panics-crashes|Kindleberger source page]] LOLR section: crypto has no LOLR by design — cascades terminate on price exhaustion or exchange halts, not central-bank rescue. **This family's setups are MORE prevalent in crypto than equities** because the absence of LOLR allows cascades to fully run their course.

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Family state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Most-active** — frequent cascades; cascade-aftermath fades clean | W2 cohort heavy concentration in Q4 2025 + Q1 2026 |
| Bearish continuation (e.g. 2026-Q1) | Active — sustained cascades during downtrend phase | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Bullish trending | Less frequent — cascades rare in sustained bull | Inferred; not directly tested |
| Chop / transitional | Sparser cascades; mechanism opportunistic only | Inferred |
| Cascade aftermath itself | **Definitionally active** — this IS the family | Family-defining condition |
| Vol blowoff (e.g. May 2021, Mar 2020, Aug 2024) | **Family canonical** — historical worked examples | Per regime taxonomy regimes 1, 4, 6, 7 + case studies 1, 2, 6 above |

**Notes:**
- Family is **regime-aware by construction** — mechanism *requires* a cascade event as trigger; cascade frequency varies with regime
- Required regime markers for productionize: documented cascade event (Coinglass liquidation cluster + sharp price move + on-chain whale flow + funding-rate flip + OI collapse — all defined in §Operational Thresholds above)
- Anti-regime conditions (don't deploy when): mid-cycle without recent cascades; sustained low-vol regimes
- Family-member rejected entries: [[trading/rejected/turtle-soup-4h-2026-05]] (failed-breakout-reversal bare-mechanism — partial-truth on short-side asymmetry retained)
- Family-member status: most family-member mechanisms (Wyckoff spring/upthrust, multi-cascade exhaustion, Stage-5 panic bottoming) untested in pipeline; capability gap = no Coinglass liquidation cluster primitive (see §DSL section below)

Cross-strategy regime context: per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], 13/13 strategies positive in 2025-Q4 (which included multiple cascade events); family's regime profile aligns with broader cohort pattern.

## DSL Primitive Requirements / Capability Gap

Per this research program (2026-05-08): the wiki has shifted from idea-blocked to capability-blocked. This family's mechanisms are wiki-encoded but **none are currently expressible in the backtesting lab DSL** — the data feeds and primitives required do not exist in the lab pipeline. This section consolidates what's needed for hand-off to a team engineer (DSL extension) per the action item in the research program.

### Per-template DSL requirements

#### Template 1 — Spring entry on cascade aftermath

| Required primitive | Current state | Capability dependency |
|-------------------|---------------|----------------------|
| `liquidations_at_level(price_band, side, lookback)` | Not implemented | Coinglass per-exchange API |
| `liq_cluster_density(price, atr_band)` | Not implemented | Coinglass liquidation heatmap data |
| `oi_pct_change(window)` | Not implemented | Per-exchange OI feed (Binance, Bybit, OKX, Deribit) |
| `funding_rate(symbol, exchange, window)` | Not implemented | Per-exchange funding feed |
| `whale_netflow(cohort, direction, window)` | Not implemented | Glassnode / on-chain analytics provider |

Roadmap dependency: **capability #3 (Coinglass) is load-bearing**; capability #2 (Glassnode) sharpens via whale-flow confirmation.

#### Template 2 — Cascade-cause classification

| Required primitive | Current state | Capability dependency |
|-------------------|---------------|----------------------|
| `event_classifier(news_feed, event_window)` | Not implemented; requires news/event data feed | News/event feed (capability roadmap #7) |
| `stablecoin_supply_delta(window)` | Not implemented | Glassnode supply data (capability #2) |
| `whale_flow_direction(cohort, exchange_dir, window)` | Not implemented | Glassnode entity-adjusted flow (capability #2) |
| `funding_persistence(threshold, duration)` | Not implemented | Per-exchange funding feed (capability #3 / partial) |
| `oi_rebuild_velocity(window)` | Not implemented | Per-exchange OI feed (capability #3) |

Roadmap dependency: **capabilities #2 + #3 + (later) #7 (calendar/news feed)**.

#### Template 3 — Multi-cascade exhaustion

| Required primitive | Current state | Capability dependency |
|-------------------|---------------|----------------------|
| `cascade_event_count(window)` | Composable from cascade-event detector if Coinglass available | Capability #3 |
| `lth_cost_basis(time_t)` | Not implemented | Glassnode (capability #2) |
| `sth_realized_loss_ratio(window)` | Not implemented | Glassnode (capability #2) |
| `miner_capitulation_signal(window)` | Not implemented | Glassnode miner-cohort metrics (capability #2) |

Roadmap dependency: **capability #2 (Glassnode) is load-bearing** for this template.

### Consolidated capability gap

| Capability (per this research program roadmap) | Templates unblocked | Mechanisms unblocked |
|--------------------------------------------|---------------------|----------------------|
| **#2 Glassnode / on-chain** | Templates 2 + 3 (partial 1) | LTH/STH dynamics, whale flow, miner capitulation, stablecoin supply |
| **#3 Coinglass liquidation** | Template 1 (full), Template 2 (cascade detection) | Cascade event detection, liq-cluster magnet, OI dynamics, funding-rate primitives |
| **#7 Calendar / news feed** | Template 2 (event-classifier) | Cascade trigger classification |

**Critical-path observation:** the family's *first-test* template (Template 1, Spring entry) requires Coinglass (capability #3) + funding/OI feed. **Coinglass is the unlock for testing the family's most-tractable mechanism.** Glassnode (capability #2) is the unlock for testing the harder mechanisms (Templates 2 + 3) that distinguish microstructure from fundamental cascades.

### Sequencing recommendation

For DSL extension hand-off (per this research program "Action: assemble a prioritized DSL-extension list from the capabilities roadmap and hand to a team engineer"):

1. **Tier 1 (Coinglass-dependent — Template 1 testable)**: `liquidations_at_level`, `liq_cluster_density`, `oi_pct_change`, `funding_rate` per-exchange aggregator
2. **Tier 2 (Glassnode-dependent — Template 2 + 3 testable)**: `whale_netflow`, `stablecoin_supply_delta`, `lth_cost_basis`, `sth_realized_loss_ratio`, `miner_capitulation_signal`
3. **Tier 3 (Calendar/news — Template 2 classifier sharpens)**: `event_classifier`, `event_window_proximity`

Until Tier 1 lands, all templates remain wiki-status `seedling` at the *strategy* level (research-direction-only). Family page status `growing` reflects substrate readiness, not strategy testability.

## Source Pointers

- [[foundation/sources/wyckoff-method|Wyckoff]] — Spring/Upthrust = canonical cascade-aftermath mechanism; primary source for the operational structure (Studies Ch V, VI, VII)
- [[foundation/sources/lefevre-reminiscences|Lefèvre/Livermore]] — bucket-shop-fingerprint Ch I (sharp instant decline + recovery) is the same pattern as cascade aftermath; 1907 panic walkthrough (Ch IX) illustrates flow-exhaustion termination; "raid libel" (Ch XV, XXIII) provides the inverted-tip diagnostic for fundamental cascades
- [[foundation/sources/kindleberger-manias-panics-crashes|Kindleberger]] — bubble-cycle Stage 5 (Panic) is the macro-scale version; "The panic feeds on itself until prices have declined so far that investors are tempted to buy" = aftermath dynamics; LOLR architecture analysis = why crypto cascades terminate on price exhaustion only
- [[foundation/sources/harris-trading-exchanges|Harris]] — Ch 28 (Bubbles, Crashes, Circuit Breakers): 1987 portfolio-insurance cascade analysis is the canonical microstructure cascade; microstructure-driven cascades recover quickly distinction; crypto's lack of circuit breakers means cascades play out fully
- [[foundation/sources/schwager-market-wizards|Schwager]] — Raschke's failed-breakout reversal (Turtle Soup) = cascade-aftermath trade with named operational rules; Rogers's "I short hysteria... go against panic" with the critical caveat that the underlying thesis must be intact
- [[foundation/sources/hayes-essays|Hayes (2026-05-08 ingest)]] — *forward-cascade-trigger watch*: This Is Fine (Feb 2026) speculates on AI-displacement → bank-credit cascade ($557B losses ≈ 13% US commercial bank equity write-down at 20% knowledge-worker baseline). Speculative not historical; belongs as regime-watch input to cascade-trigger taxonomy. Also: $HYPE Man (Mar 2026) introduces ADV/OI ratio as venue-quality / wash-volume detector — relevant when calibrating cascade-event detection thresholds across exchanges (high-ADV/OI venues' liquidation feeds may be unreliable)

## Honest Caveats

- **Self-defeating risk**: liquidation-cluster fades are widely watched; the simple version is increasingly priced. Edge requires multi-channel confluence + macro-regime alignment. Per [[foundation/methodology/deployment-edge-threshold]], naive cluster fades likely cleared the friction floor in 2020-2022 but may be marginal by 2026 — the case-study aftermath signatures need cross-validation on recent windows.
- **Cascade vs noise**: distinguishing "cascade aftermath" from "noise wick" requires cluster confluence. Without confirmation criteria (the §Operational Thresholds above), every wick looks tradable; most aren't.
- **Cascade-cause classification is hard**: the mechanism that distinguishes microstructure from fundamental requires multi-channel reading (price action + on-chain flow + macro context); not a single-indicator decision. Template 2 will be the highest-failure-rate template until news/event classifier matures.
- **Stop discipline essential**: cascade overshoots can extend further than the recovery candle; stop just below the wick low (long-side) is operationally tight but may need volatility scaling per case-study (May 2021 retest occurred 2 months later — beyond a tight time-stop).
- **Time-stop required**: a "spring" that doesn't recover within 4 hours (intraday) or 5 days (daily) wasn't a spring. Exit and reassess.
- **Fundamental-cascade catch-falling-knife risk**: the May/June 2022 sequence (UST → 3AC) is the canonical cautionary tale — naive aftermath trades on each successive cascade lost money continuously for 6 weeks. Template 2's classification gate is not optional; it is the difference between Family-as-edge and Family-as-graveyard.
- **Family scope vs specific mechanism**: this is a family-aggregator page, not a specific tradable mechanism. For operational trade specs, use the linked mechanism pages (accumulation-distribution, failed-breakout-reversal).
- **Backtest infeasibility today**: until Coinglass + Glassnode primitives land in the DSL, none of the three templates can be cleanly backtested. The wiki research has run *ahead* of capability availability per the 2026-05-08 capability-blocked reframe; this sharpens the capability-investment decision but does not bypass it.

## Operational Checklist for New Strategies in This Family

When designing a new strategy in this family:

1. **Identify the cascade trigger**: technical level break, news event, exchange halt, funding flip
2. **Identify the cohort being liquidated**: long-side leverage, short-side leverage, crowded narrative, etc.
3. **Identify the post-cascade carrier**: what observable signals exhaustion vs continuation? (Funding floor, OI rebuild direction, on-chain whale response, cohort-classifier divergence)
4. **Define the entry trigger**: cascade event + N bars + recovery confirmation per §Operational Thresholds. Don't anticipate; wait for confirmation.
5. **Define stop**: just beyond cascade extreme (no further extension expected if mechanism is real).
6. **Define time-stop**: cascade-aftermath trades resolve quickly. Aftermath that extends >N bars without recovery = wrong setup.
7. **Articulate falsifier**: cascade-conditioned trades not outperforming random; recovery rate ≤ unconditional baseline.
8. **Run cascade-cause classification first** (per §Operational Thresholds): if signals point to fundamental, do not deploy aftermath template directly — shift to multi-cascade-exhaustion template (waits for multiple cascade events) or stand aside.
9. **Map DSL primitive requirements** (per §DSL Primitive Requirements section) — surface any unimplemented primitives explicitly and include in capability-investment decision.

## Related
- [[../../foundation/sources/ali-oct-2025-liquidation-cascade|Ali (2025) — Anatomy of the Oct 10-11 2025 Cascade]] — the SSRN source behind the §7 case study

- [[foundation/market-structure/crypto-carrier-features]] — central catalog; cascade carriers documented there
- [[trading/mechanisms/accumulation-distribution]] — primary specific mechanism in this family (spring + upthrust)
- [[trading/mechanisms/failed-breakout-reversal]] — Raschke/Turtle Soup variant of cascade-aftermath
- [[foundation/market-structure/bubbles-crashes-circuit-breakers]] — Harris microstructure layer; cascade-cause classification; full Harris Ch 28 extraction
- [[foundation/market-wisdom/bubble-cycle-stages]] — Stage 5 Panic = macro-scale cascade aftermath
- [[trading/mechanisms/behavioral-positioning-unwind]] — sibling family; positioning-unwind cascades feed cascade-aftermath setups
- [[trading/mechanisms/funding-cycle-dynamics]] — sibling family; funding-extreme cascades are funding-cycle mechanism overlap
- [[trading/mechanisms/gamma-exposure-regime]] — **added 2026-06-07**: negative dealer-gamma (GEX) sells into weakness alongside margin liquidations — aligned forced flow that amplifies cascades; the liquidation magnetic-zone (where the fuel sits) + GEX sign (whether dealer hedging adds to the burn) are two lenses on one forced-flow regime
- [[foundation/methodology/regime-marker-data-sources]] — **added 2026-06-07**: Coinglass liquidation-heatmap is the magnetic-zone data source (API paid; route funding/OI via exchange-native for backtest)
- [[foundation/market-wisdom/composite-operator]] — CO accumulating during cascade aftermath = the absorbing-counterparty
- [[foundation/macro/crypto-2020-2022-cycle]] — multi-cascade exhaustion case study (May 2022 → June 2022 → November 2022)
- [[foundation/methodology/deployment-edge-threshold]] — cascade-aftermath sizing implications discussed in §Position-sizing
- [[foundation/methodology/strategy-validation-discipline]] — pre-registration of operational thresholds per §2.1
- Apr 28 mechanism families — this is one of the 5 prioritized day/swing families
- this research program — capability-blocked reframe (2026-05-08); DSL roadmap context

## Sources

- Family aggregator; primary sources are the wiki pages cross-referenced above plus the workdir distillations cited in §Source Excerpts:
  - Wyckoff (1910 *Studies in Tape Reading* + 1931 *Method of Trading*) — `raw/books/wyckoff-method/distillation.md`
  - Lefèvre (1923 *Reminiscences of a Stock Operator*) — `raw/books/lefevre-reminiscences/distillation.md`
  - Kindleberger (1978 *Manias, Panics, and Crashes* Ch 2) — `raw/books/kindleberger/distillation.md`
  - Harris (2003 *Trading and Exchanges* Ch 28) — `raw/books/harris-trading-exchanges-full.workdir/output.md`
  - Schwager (*Market Wizards* 1989, *New Market Wizards* 1992) — `raw/books/schwager-market-wizards/distillation.md`
- Case-study sources (timing, magnitude, mechanism classification): trader-side public records of the six events — Coinglass historical liquidation summaries, Glassnode regime markers per [[foundation/macro/btc-regime-taxonomy-2020-2025]], and contemporaneous press for the FTX/UST/SVB events
