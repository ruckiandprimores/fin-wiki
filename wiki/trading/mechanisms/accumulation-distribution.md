---
title: "Accumulation/Distribution — Wyckoff's Tradable Cycle (with Spring & Upthrust)"
status: seedling
created: 2026-05-03
updated: 2026-05-21
tags: [mechanism, wyckoff, accumulation, distribution, spring, upthrust, liquidation-cascade-aftermath, day-swing-applicable, distribution-asymmetry-caveat]
---

# Accumulation/Distribution — Wyckoff's Tradable Cycle (with Spring & Upthrust)

> **TL;DR**: The operational mechanism derived from [[foundation/market-wisdom/composite-operator|Composite Operator]] phases 1+3 (accumulation, distribution) and the transition events at their ends — **spring** (sharp break below support at end of accumulation, immediate recovery, captures stops + acquires final inventory cheaply) and **upthrust** (sharp thrust above resistance at end of distribution, fails to follow through, distributes remaining inventory to last-wave buyers). Crypto carrier: liquidation cascade visible on Coinglass — the spring/upthrust translates almost line-for-line. Sits in the trader's prioritized **liquidation cascade aftermath** mechanism family per Apr 28 update. **Note on terminology**: "spring" and "upthrust" are [[foundation/sources/wyckoff-method|successor labels (Evans 1940s+)]] for concepts present in primary Wyckoff (Studies Ch VII shakeouts; Method TR Sec 2-A "quick upward thrust to catch shorts"). The wiki uses the labels because they're clearer pedagogy, while flagging attribution.

## Hypothesis (Structured Format) — Spring Setup

```
Hypothesis:    Long entries on confirmed Wyckoff spring setups (sharp break
               below well-defined support at the end of an accumulation
               range, followed by immediate recovery on equal-or-greater
               volume) produce significantly above-random returns when
               combined with the appropriate crypto carriers.

Mechanism:     The Composite Operator (or aggregate-large-interest equivalent)
               drives price below known support to capture clustered stops
               of weak holders, provoke fresh selling from late longs, and
               acquire the final inventory needed cheaply. The shakeout is
               an *intentional* terminal event of accumulation. Once stops
               are cleared and weak holders are out, the CO no longer faces
               the supply overhang that prevented markup. Markup begins.

Observable:    For each spring candidate:
               - well-defined accumulation range with a clearly identifiable
                 support level (multiple prior tests of the same level)
               - sharp break below the support on a single bar / wick
               - immediate recovery (within 1-3 bars on the relevant
                 timeframe) above the broken support
               - rally on equal-or-greater volume than the breakdown bar
               - "test" — secondary visit to the low on lower volume
                 (confirms no fresh supply at the level)

Expected:      Long entries on confirmed springs should produce
               2-5x R returns over 1-2 weeks (BTC daily timeframe); win rate
               60-70% with proper confluence; R:R 1:2 to 1:5 with stops
               just below the spring low.

Falsifier:     If spring-flagged longs (with the listed observability
               criteria) do not outperform random-direction longs of
               equivalent volatility-matched assets — the mechanism is
               not present in the tested market.
               Specifically: backtest BTC perps 2023-2026 of springs
               identified by liquidation-cluster + immediate-recovery
               criteria; should outperform random-direction longs by
               ≥1 standard deviation in median return. Out-of-sample
               persistence in 2/3+ of walk-forward windows.
```

### Distribution-mirror asymmetry caveat (2026-05 N=1) {#distribution-mirror-asymmetry-caveat}

The Wyckoff-symmetric framing on this page treats accumulation and distribution as mirror mechanisms. **N=1 BTC 4h falsification suggests they have asymmetric counterparty base rates in crypto**: `cell_accumulation_long_btc_v2` validates cleanly (PSR(>0) 93.1%) but its direction-flipped mirror `cell_distribution_short_btc` falsifies (trade Sharpe −0.74, WR 11%, N=9). Three structural counterparty asymmetries (retail spot accumulation has no DCA-sell mirror; ETF flow is net-positive even in bear; basis carry has no symmetric backwardation case) suggest distribution lacks the structural-counterparty base rate that accumulation has. **Pending N=2 confirmation** before promoting from per-rejection note to mechanism-page sharpening. See [[../rejected/cell-distribution-short-btc-2026-05#asymmetric-base-rates]] for the structural explanation.

## Hypothesis (Structured Format) — Upthrust Setup

```
Hypothesis:    Short entries on confirmed Wyckoff upthrust setups (sharp
               thrust above well-defined resistance at the end of a
               distribution range, failure to follow through, return below
               resistance) produce significantly above-random returns when
               combined with the appropriate crypto carriers.

Mechanism:     The CO drives price above known resistance to capture
               clustered stops of late shorts, induce one last wave of
               public buying (FOMO into the apparent breakout), and unload
               final inventory into that demand. The upthrust is the
               mirror-image terminal event of distribution. Once shorts
               are squeezed and last-wave buyers are absorbed, no buying
               support remains; markdown begins.

Observable:    For each upthrust candidate:
               - well-defined distribution range with clearly identifiable
                 resistance level (multiple prior failed attempts at the level)
               - sharp thrust above resistance on a single bar / wick
               - failure to follow through — return below resistance within
                 1-3 bars
               - subsequent rally fails to make new high (sign of weakness)
               - breakdown on increasing volume

Expected:      Short entries on confirmed upthrusts should produce
               2-5x R returns over 1-2 weeks; win rate 55-65%;
               R:R 1:2 to 1:4 with stops just above the upthrust high.

Falsifier:     If upthrust-flagged shorts do not outperform random-direction
               shorts on volatility-matched assets — the mechanism is not
               present in the tested market. Same backtest discipline as
               spring; out-of-sample persistence in 2/3+ of walk-forward
               windows.
```

## The Wyckoff Source

The primary articulation of the spring concept is in [[foundation/sources/wyckoff-method|Wyckoff *Studies in Tape Reading*]], Ch VII:

> "When insiders shake other people out it means that they want the stock themselves. These are good times for us to get in, for then we will enjoy having Mr. Frick and his friends work for us."

> "On the other hand, when there is a gradual hardening in prices; when bear raids fail to dislodge considerable quantities of stock; when stocks do not decline upon unfavorable news, we may look for an advancing market in the near future."

The primary articulation of upthrust is in Method Div 2 Sec 2-A, where Wyckoff diagrams "Quick upward thrust to catch shorts" with annotations of subsequent failure.

The named labels "spring" and "upthrust" are successor synthesis from [[foundation/sources/wyckoff-method|Robert G. Evans's Stock Market Institute teaching]] (1940s+). The concepts are primary Wyckoff; the labels are post-1934. We use the labels because they're clearer than "shakeout-at-end-of-accumulation."

## Why Springs and Upthrusts Work (Mechanism Decomposition)

### The CO needs the final inventory

By the time accumulation is in late stage, the CO has acquired most of the line he intends to mark up. The remaining supply is held by:
- Stubborn longs who won't sell at any price within the range
- Weak/late longs whose stops are clustered just below the well-known support level
- Floating short interest betting on a continuation of the prior decline

The CO can do one of two things:
1. **Wait** for the stubborn longs to slowly distribute. Slow.
2. **Provoke a flush** through the support level. Fast and more profitable — captures stops, scares stubborn longs into the bid, removes shorts' confidence.

A spring is option 2.

### Why the recovery is the key tell

After a *real* breakdown of accumulation (no spring; genuine breakdown to lower prices), supply continues, price stays below the broken level, and the next move is downward.

After a *spring* (engineered shakeout), the supply is exhausted in a single wick — no follow-through selling exists because the weak holders are already out. Price recovers immediately because the CO is *the bid* on the way back up.

The **immediate recovery on equal-or-greater volume** is the diagnostic that distinguishes spring from genuine breakdown. The recovery without it is just dead-cat-bounce noise.

### Mirror logic for upthrust

A distribution range ends when the CO has unloaded most of his line into public demand. Remaining demand sits with:
- Late retail bulls who haven't bought yet (FOMO buyers waiting for breakout)
- Short stops clustered just above resistance
- Trend followers using "buy on close above resistance" rules

The CO drives price above resistance to:
1. Trigger short stops → forced buying → CO sells into it
2. Induce FOMO breakout buying → CO sells into it
3. Trigger trend-follower buys → CO sells into it

Once these buyers are absorbed, no buying support remains. Markdown follows.

## Crypto Carriers — Why This Translates Beautifully

This is **the highest-priority Wyckoff translation for crypto perps** because crypto's stop clusters are *more visible* than 1910 NYSE order flow ever was. From [[foundation/market-structure/crypto-carrier-features|the carrier-features catalog]]:

### Spring carriers

| Carrier | What to look for |
|---------|------------------|
| **Coinglass liquidation map** | Clustered long-side liquidations density just below the support level; visible in real time |
| **Long liquidation cascade event** | The cascade itself = the wick down through support; visible per-exchange and aggregate |
| **Funding rate flip** | Funding flips deeply negative on the cascade (shorts pay; everyone short) |
| **On-chain whale flow** | Net accumulation by known whale wallets *during* the wick (taking the other side of the cascade) |
| **OI dynamics** | OI collapses during the cascade (positions liquidating); rebuilds after on the long side |
| **Quick recovery** | Within hours, price back above the broken support; the spring footprint |

### Upthrust carriers

| Carrier | What to look for |
|---------|------------------|
| **Coinglass liquidation map** | Clustered short-side liquidations density just above the resistance level |
| **Short liquidation cascade event** | The cascade = the wick up through resistance |
| **Funding rate spike** | Funding spikes deeply positive on the squeeze |
| **On-chain whale flow** | Net distribution by whale wallets to exchanges *during* the upthrust (taking advantage of FOMO) |
| **OI dynamics** | OI collapses on the squeeze (shorts liquidating); rebuilds on the short side as price falls back |
| **Failure to hold** | Within hours, price back below the broken resistance |

### Why this is sharper than the 1910 version

Wyckoff had to *infer* shakeout intent from price-volume tape and the absence of follow-through. Crypto traders see:
- The pre-event positioning structure (Coinglass shows where liquidations will trigger)
- The event itself (cascade visible in real time)
- The recovery (price action immediately after)
- The flow context (on-chain whale wallets net-accumulating during the wick)
- The aftermath (funding flip, OI rebalance)

This is the most observable Wyckoff mechanism in any market in history. Edge availability is correspondingly under threat from crowding — see "Honest Caveats" below.

## Operational Specification — Spring Trade

Default parameterization for backtest:

| Aspect | Rule |
|--------|------|
| **Setup precondition** | BTC perp in defined accumulation range ≥ 5 days; support level tested ≥ 2 prior times; macro regime non-bear (BTC above 200-day MA OR clear regime-bottom signals) |
| **Spring trigger** | Wick down ≥ 1.5% below the well-defined support, with high liquidation cluster density visible on Coinglass below that support |
| **Recovery confirmation** | Price closes back above the broken support within 4h on a daily-timeframe spring; volume on the recovery bar ≥ volume on the breakdown bar |
| **Confluence requirement** | At least 3 of: liquidation cluster density, funding flip negative, on-chain whale accumulation visible, OI collapse on cascade then rebuild, prior support level confluence |
| **Entry** | First close back above the broken support with volume confirmation; or on the test of the low on lower volume |
| **Position size** | Per [[investment/allocation/equity-management-rules]] (≤3-5% risk per trade) |
| **Stop placement** | Just below the spring low (the lowest wick; not the broken support level — the wick low) |
| **First target** | Top of the accumulation range (1.5-2× initial-risk distance) |
| **Trail rule** | After 1× initial-risk move, trail stop with 2× ATR (sit-tight on validated breakouts; per [[foundation/market-wisdom/sit-tight-discipline]]) |
| **Time stop** | Exit if position is not 1×R favorable within 7 days (springs that don't start markup quickly are not springs) |

## Operational Specification — Upthrust Trade

Mirror parameters:

| Aspect | Rule |
|--------|------|
| **Setup precondition** | BTC perp in defined distribution range ≥ 5 days; resistance level tested ≥ 2 prior times; macro regime non-bull (BTC below 200-day MA OR clear regime-top signals) |
| **Upthrust trigger** | Wick up ≥ 1.5% above the well-defined resistance, with high short-liquidation cluster density visible on Coinglass above that resistance |
| **Failure confirmation** | Price closes back below the broken resistance within 4h on a daily-timeframe upthrust; subsequent rally fails to make new high |
| **Confluence requirement** | At least 3 of: short-liquidation cluster density, funding spike positive, on-chain whale distribution to exchanges, OI collapse on squeeze then rebuild on short side, prior resistance confluence |
| **Entry** | First close back below the broken resistance with volume confirmation; or on the failed-rally lower high |
| **Position size** | Per [[investment/allocation/equity-management-rules]] |
| **Stop placement** | Just above the upthrust high |
| **First target** | Bottom of the distribution range |
| **Trail rule** | 2× ATR trail after 1×R favorable |
| **Time stop** | 7 days |

## Falsifier Specifications

For both spring and upthrust:

- Backtest BTC perps 2023-2026, identifying all candidates meeting the criteria above.
- Compare returns vs.:
  - Random-direction trades on volatility-matched assets in the same period.
  - Same strategy without confluence requirement (just liquidation cluster cleared, no other carriers).
  - Same strategy without recovery/failure confirmation (just the wick).
- Walk-forward by 6-month windows; spring/upthrust edge should persist in at least 2/3 of out-of-sample windows.
- Failure criteria: if either backtest produces returns within 1 standard deviation of random, mechanism is not present in the tested market parameterization.
- **Honest measurement of edge erosion**: re-run the backtest annually. Liquidation-cluster patterns are increasingly watched; the simple version is at risk of crowding. Edge persistence is the open empirical question.

## The Full Cycle Trade (Beyond Spring/Upthrust)

The spring and upthrust are *terminal events* of the cycle. The full cycle offers additional trade structures:

### Late accumulation entry (post-spring confirmation)

Enter long after the spring confirms (recovery + test of the low on lower volume). Hold through the entire markup phase. Exit on first sign of distribution (abnormal volume in narrow range at top, breadth deterioration, upthrust signal). Highest-conviction long; longest hold; largest move.

### Late distribution entry (post-upthrust confirmation)

Mirror — short after upthrust confirms. Hold through markdown. Exit on first sign of accumulation. Highest-conviction short.

### Phase-transition straddles

When the accumulation range is well-defined but the spring hasn't yet triggered, one option is to wait at the support level with both a spring-anticipation long order (below) and a breakdown-confirmation short order (further below). The CO's action will trigger one or the other.

This is more capital-intensive but doesn't require predicting which direction the phase will resolve.

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Mechanism state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Distribution active** — late-cycle distribution mechanics dominate | Per [[trading/rejected/funding-flip-recovery-regime-gate-2026-05]] cross-strategy worldview update; W2 cohort regime concentration |
| Bearish continuation (e.g. 2026-Q1) | Active — markdown phase progression | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Bullish trending (e.g. 2025-Q2 mild uptrend) | **Accumulation active in inverse** — markup phase begins | `wyckoff_spring_4h` tested in W2 cohort: medSh +0.14 (weak); insufficient signal for productionize-grade |
| Chop / transitional | **Phase B/D activity** — building cause; tests of range | Per Wyckoff schematic; not isolated in current backtest |
| Cascade aftermath | **Spring canonical** — late-stage capitulation/spring overlap | Per [[trading/mechanisms/liquidation-cascade-aftermath]] family page; Wyckoff spring is the cascade-aftermath setup |
| Euphoria peak | **Upthrust canonical** — distribution at top is the canonical setup | Per Wyckoff cycle phase progression |

**Notes:**
- Mechanism is **regime-aware**: phase identification IS the regime classification
- Required regime markers for productionize: clear phase identification (volume, price action, range structure); confluence with on-chain whale flow + funding-rate flip + Coinglass liquidation cluster
- Anti-regime conditions (don't deploy when): regime transitions (uncertain phase classification); chop without identifiable phase structure
- `wyckoff_spring_4h` low Sharpe in W2 cohort suggests bare-mechanism on default params is insufficient; spec'd version (multi-confluence + macro filter) untested

Cross-strategy regime context: per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], wyckoff_spring is one of the 13 strategies in the cohort; specific evidence weak; family follows broader cohort pattern (Q4 2025 + Q1 2026 concentration).

## Honest Caveats

- **Successor terminology disclosure.** "Spring" and "upthrust" are post-Wyckoff labels (Evans 1940s+). The concepts are primary Wyckoff; the labels are not. Per [[foundation/sources/wyckoff-method|the source page]] honest-caveats section.
- **The A-E phase schematic is also successor.** Modern Wyckoff teaching labels accumulation phases A through E (A = stopping action; B = building cause; C = test/spring; D = markup; E = trend) and distribution phases A-E in mirror. **The labeled schematic is not in primary Wyckoff (1910/1931).** Wyckoff describes the four-phase cycle (accumulation/markup/distribution/markdown) without the sub-labels. The wiki avoids the A-E labeling; primary Wyckoff is sufficient.
- **Self-defeating prophecy risk.** Spring and upthrust patterns are *very* widely watched in crypto. The simple "fade the liquidation cluster" rule is increasingly priced. Edge requires the multi-confluence version *and* regime-conditioning, not the simple version.
- **False springs / false upthrusts.** A wick below support without recovery is a genuine breakdown, not a spring. The pattern only confirms in retrospect. Stop discipline is essential — wick can extend further than expected before recovery (or never recover).
- **Time-stop discipline.** A real spring or upthrust resolves within days. Setups that go sideways for weeks after the wick are not what the mechanism describes; exit and re-evaluate.
- **Regime dependence.** Springs work best in non-bear regimes (the eventual markup needs macro tailwind). In bear markets, the "spring" often turns out to be just one more failed support level. Cross-confirm with [[foundation/macro/intermarket-analysis|macro regime layer]].
- **Multi-asset confluence.** A spring in a single asset is weaker than a spring confirmed across BTC + ETH + sector breadth. Per [[foundation/market-wisdom/composite-operator|CO logic]], the aggregate-large-interest position should be coherent across the market, not isolated to one asset.
- **Manipulation can fake springs.** Sophisticated counterparties can engineer false springs (wick down to capture longs' stops, brief recovery, then drop further). Defense: cross-check with on-chain whale flow during the wick — real spring shows net accumulation, fake spring shows no flow change or net distribution.
- **Mechanism family alignment.** This sits in the trader's prioritized **liquidation cascade aftermath** family per Apr 28 update. The structured-hypothesis frame is required by Quality Standards; the underlying mechanism is from primary Wyckoff (Studies Ch VII, 1910) — not AI-generated.

## Salvage Path for Rejected Ideas

Per [[trading/rejected/candle-morphology-batch-2026-04|the candle-morphology rejected batch]], directions 5.1/5.2 (close-sequence accumulation/distribution) failed because they were articulated as candle-shape directives without mechanism. The salvage path:

- 5.1/5.2 → **re-derive as accumulation/distribution detection** using the structured-hypothesis form on this page
- Required reformulation: stop describing the candle pattern; start asking "is the CO absorbing or distributing here, and what does the price-volume-OI-flow signature say about which phase we're in?"
- Required carriers: on-chain whale flow + exchange in/out + funding rate + OI dynamics + Coinglass liquidation cluster
- Required test: Wyckoff's [[foundation/market-wisdom/wyckoff-three-laws|effort-vs-result diagnostic]] — does the volume on each move match the price change?

The original 10 strategies should not be re-tested. New strategies derived through this Wyckoff salvage path must pass the v5 prompt's mechanism + carrier + falsifier requirements before being elaborated.

## Cross-Pattern Considerations

### vs. [[trading/mechanisms/pivotal-points]]

Pivotal points (round-number breakouts, prior-range pierces) are entry triggers; the spring/upthrust is a *terminal event* with its own trade structure. They compose: a spring near a major pivotal level is a higher-conviction setup than either alone.

### vs. [[trading/mechanisms/group-leader-divergence]]

Group-leader divergence is a regime-change diagnostic across assets. Springs/upthrusts are within-asset terminal events. They confirm each other: a spring in BTC + breadth confirmation in alts (or upthrust in BTC + breadth deterioration in alts) is the highest-conviction phase-transition setup.

### vs. [[trading/mechanisms/support-resistance]]

S/R defines the *levels* where springs/upthrusts occur. They depend on each other: spring trades require a well-defined accumulation range with multiple-tested support; that's what S/R analysis identifies.

### vs. [[trading/mechanisms/volume-analysis]]

Volume analysis is the diagnostic *tool*; spring/upthrust are *mechanisms*. The "volume on recovery bar ≥ volume on breakdown bar" check uses volume-analysis principles applied to the spring-confirmation question.

## Key Takeaways

- Accumulation and distribution are the cycle's *quiet phases* (CO building/unloading position); spring and upthrust are the *loud terminal events* that mark phase transitions.
- The **immediate recovery on equal-or-greater volume** is the spring diagnostic; the **failure to follow through with subsequent lower high** is the upthrust diagnostic.
- Crypto's liquidation map visibility makes this *the* most observable Wyckoff mechanism — and the most at risk of being crowded.
- Multi-channel carrier confluence (liquidation cluster + funding + on-chain flow + OI) is the discipline that distinguishes real from fake.
- Position sizing per equity-management rules; stops just below spring low / above upthrust high; time-stop within 7 days.
- The mechanism family is "liquidation cascade aftermath" per the trader's prioritized day/swing families.
- Honest about successor terminology: spring and upthrust are Evans's labels (1940s+); the concepts are primary Wyckoff (Studies Ch VII; Method TR Sec 2-A).
- The full cycle (accumulation → markup → distribution → markdown) offers longer-hold trades for higher conviction; spring/upthrust are the terminal-event triggers.

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog of carriers used (Coinglass, funding, on-chain whale flow, OI dynamics)
- [[foundation/sources/wyckoff-method]] — primary source; Studies Ch VII; Method TR Secs 2-A, 5, 10
- [[foundation/market-wisdom/composite-operator]] — the *who* and *what*; this page is the operationalization of phases 1+3 transitions
- [[foundation/market-wisdom/wyckoff-three-laws]] — the diagnostic framework applied to determine spring/upthrust validity
- [[foundation/market-wisdom/tape-reading]] — the broader discipline; spring/upthrust are specific tape patterns
- [[foundation/market-wisdom/sit-tight-discipline]] — required to hold the markup/markdown move after the trigger
- [[trading/mechanisms/pivotal-points]] — entry-trigger mechanism; composes well with spring near pivotal level
- [[trading/mechanisms/group-leader-divergence]] — regime-change diagnostic; cross-confirmation
- [[trading/mechanisms/support-resistance]] — defines the levels; spring/upthrust occur at well-defined S/R
- [[trading/mechanisms/volume-analysis]] — diagnostic tool used in confirmation
- [[foundation/market-structure/market-manipulation-taxonomy]] — squeezer/order-anticipator categories that engineer false springs
- [[foundation/market-structure/bubbles-crashes-circuit-breakers]] — cascade dynamics; springs occur during the recovery from microstructure cascades
- [[trading/rejected/candle-morphology-batch-2026-04]] — salvage path for 5.1/5.2 runs through this page
- [[trading/rejected/cell-distribution-short-btc-2026-05]] — N=1 distribution-mirror falsification; per-direction asymmetry caveat
- Apr 28 mechanism families — this mechanism is in the *liquidation cascade aftermath* family

## Sources

- Wyckoff (as Rollo Tape), *Studies in Tape Reading*, Ch VII (shakeouts), 1910.
- Wyckoff, *The Richard D. Wyckoff Method of Trading in Stocks*, Method Div 2, TR Sec 2-A (upthrust diagrams), 1931.
- Source page: [[foundation/sources/wyckoff-method]] — for primary-vs-successor attribution.
- Successor labeling and formalized schematic (flagged but not ingested): Robert G. Evans (Wyckoff Stock Market Institute, 1940s+); Hank Pruden (*The Three Skills of Top Trading*, 2007); Bruce Fraser (Stockcharts.com Wyckoff Power Charting series, ongoing).
