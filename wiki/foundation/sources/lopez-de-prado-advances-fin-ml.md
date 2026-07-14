---
title: "Advances in Financial Machine Learning — Marcos López de Prado"
status: growing
created: 2026-05-05
updated: 2026-05-05
tags: [source, lopez-de-prado, validation-discipline, backtest-methodology, multiple-testing, modern-quant]
---

# Advances in Financial Machine Learning — Marcos López de Prado

> **TL;DR**: The wiki's first rigorous source for **backtest validation discipline**. López de Prado (Head of ML at AQR; Cornell faculty; one of the most-published authors on backtest overfitting) catalogs *why most published backtests are false discoveries* and provides specific corrective tooling: purged k-fold CV with embargo, the Probabilistic and Deflated Sharpe Ratios (PSR/DSR), Probability of Backtest Overfitting (PBO) via combinatorially symmetric CV. **Validation discipline is universal across market regimes** — the wiki's "pre-regulation behavioral wisdom > modern quant" framing in Part I §1 applies to *idea generation*, not to the question of whether a test was honest. This source closes a structural gap: every wiki page asserting "no overfit worship" or "falsifiers required" now has methodological backbone.

## Source Details

| Field | Value |
|-------|-------|
| **Author** | Marcos López de Prado (1974–) |
| **Book** | *Advances in Financial Machine Learning*. Wiley, 2018. |
| **Source ingested** | EPUB (`Downloads/advances-in-financial-machine-learning.epub`); chapters 4, 7, 11, 14 extracted to `raw/books/lopez-de-prado/` via pandoc. **Minimum-viable subset** per upstream task spec. Other chapters skipped or cite-only — see Chapter Map below. |
| **Format** | Academic-practitioner monograph; Python snippets per chapter |
| **Original Markets** | Institutional equities/futures, ML-driven strategies |
| **Mechanism Tags** | Validation methodology, statistical testing, multiple-testing correction |
| **Behavioral Driver** | Researcher-side selection bias on multiple backtest trials |
| **Crypto Applicability** | **Direct.** PSR / DSR / PBO / purged-CV apply to any backtest regardless of asset class. Crypto's combination of (a) regime non-stationarity, (b) high researcher-side trial counts (parameter sweeps, mechanism families), and (c) public-domain idea pool makes selection-bias risk *higher* than in equities — the methodology is more load-bearing here, not less. |

## Position in Lineage

See [[foundation/sources/lineage]].

This source occupies a **complementary axis** to the wiki's existing source spine. The pre-regulation behavioral cluster (Lefèvre, Wyckoff, Kindleberger) explains *why* edges exist (manipulation, behavioral patterns, crowd dynamics). Modern microstructure (Harris, Murphy) explains *how* markets transmit those edges. Practitioner breadth (Schwager, Lynch, Buffett) shows *what* methods exploit them. **López de Prado addresses a fourth axis**: *whether the test of a method is honest*. None of the existing sources rigorously cover this — see "Why This Source Matters" below.

**This source builds on:**
- Multiple-testing literature: Benjamini-Hochberg (1995) FDR control; Harvey-Liu-Zhu (2016) cross-section critique; Bailey & López de Prado co-authored work on Sharpe-ratio statistics (2012, 2014).
- Bagging / ensemble theory (Breiman) — used to motivate purging and embargoing.
- Backtest-overfitting research: Bailey, Borwein, López de Prado, Zhu (2014, 2017).

**This source influenced:**
- Most subsequent ML-finance research; "deflated Sharpe ratio" is now a standard term in factor-zoo literature.
- Practitioner discipline at quant firms (López de Prado was Head of ML at AQR Capital Management; methodology adopted at major hedge funds).

**Concepts originated/canonicalized here (now in wiki):**
- **Combinatorially purged cross-validation (purged k-fold CV with embargo)** → [[foundation/methodology/strategy-validation-discipline]]
- **Probabilistic Sharpe Ratio (PSR) + Deflated Sharpe Ratio (DSR)** → [[foundation/methodology/strategy-validation-discipline]] + [[glossary/deflated-sharpe-ratio]]
- **Probability of Backtest Overfitting (PBO) via CSCV** → [[foundation/methodology/strategy-validation-discipline]] + [[glossary/probability-of-backtest-overfitting]]
- **Sample uniqueness for non-IID labels** → [[foundation/methodology/strategy-validation-discipline]] + [[glossary/sample-uniqueness]]
- **Marcos' Second Law of Backtesting** ("don't research under the influence of a backtest")
- **Marcos' Third Law of Backtesting** (every backtest result must be reported with all trials involved in producing it)

## Why This Source Matters For The Wiki

The wiki has stated the principle "no overfit worship — good backtest metrics on a specific window mean nothing without out-of-sample validation" in Part IV Quality Standards and asserted "Falsifiers required" as gate. **It has had no methodological source backing those principles.** Murphy gives entry mechanics; Harris gives microstructure; Schwager gives practitioner discipline; Wyckoff/Lefèvre give behavioral mechanism. None of them cover *the statistics of honestly assessing whether a backtest is signal or noise*.

This gap is not abstract:

- **Apr 28, 2026** — Wyckoff distribution coherence-collapse hypothesis backtested on BTC perps; equity curves negative across all 5 PNL quantiles. Team verified no implementation bugs. **Falsifier fired correctly** — but this happened only because the hypothesis had a falsifier articulated. Most upstream content does not.
- **Apr 30, 2026** — [[trading/rejected/candle-morphology-batch-2026-04|candle-morphology batch]]: 10 strategies, all backtested negative, shared upstream premise was the bug. **Validation gap visible**: even after 10 negative backtests, the team had no statistical procedure for distinguishing "all 10 truly failed" from "we ran 10 trials and the noise hadn't yet produced a winner — keep going." López de Prado's PBO framework formalizes this.
- **May 4, 2026** — the backtesting lab analysis: train→val correlation negative for 7/8 strategies on a 1y window with single 70/30 split. The single-split walk-forward pattern is *exactly* what Ch 11.6 critiques.

The book's claim is sharp:

> "Most backtests published in journals are flawed... The maddening thing about backtesting is that, the better you become at it, the more likely false discoveries will pop up. Beginners fall for the seven sins... Professionals may produce flawless backtests, and will still fall for multiple testing, selection bias, or backtest overfitting." — Ch 11

For the wiki, this means: **every future backtest result page should report against PSR / DSR thresholds and against the number of trials run on the dataset**. The methodology page is the load-bearing artifact.

## Chapter Map — What Was Extracted, What Was Skipped

| Ch | Title | Status | Why |
|----|-------|--------|-----|
| 1 | Financial ML as a Distinct Subject | Skip | Framing-only; wiki has equivalent framing in the wiki architecture (ARCHITECTURE.md) Part I |
| 2 | Financial Data Structures | Skip | Bar-formation theory (tick/dollar/volume/imbalance bars) — not load-bearing for current 4h-bar pipeline; revisit if microstructure-bar work begins |
| 3 | Labeling | Skip | Triple-barrier method useful as concept but not pipeline-critical; Ch 4 references suffice |
| 4 | Sample Weights | **Partial extract** | §4.2-§4.4 extracted (overlapping outcomes + sample uniqueness) — required by Ch 7. Snippets and sequential bootstrap not extracted (engineering-detail layer) |
| 5 | Fractionally Differentiated Features | Skip | HFT/intra-bar feature engineering; not in scope at 4h timeframe |
| 6 | Ensemble Methods | Cite-only | Bagging mechanics referenced by Ch 7 and Ch 11 — concepts assumed; no full extract |
| 7 | **Cross-Validation in Finance** | ✅ **Full extract** | Purged k-fold CV + embargo. Core methodology. |
| 8 | Feature Importance | Skip | ML-feature-engineering; revisit only if ML strategy work begins |
| 9 | Hyper-Parameter Tuning | Skip | ML-specific |
| 10 | Bet Sizing | Cite-only | Wiki already covers via [[investment/allocation/equity-management-rules]] + Druckenmiller conviction-tiered sizing |
| 11 | **The Dangers of Backtesting** | ✅ **Full extract** | 7 sins + Marcos' Second Law + 6 recommendations + CSCV/PBO. Core methodology. |
| 12 | Backtesting through Cross-Validation | Cite-only | CPCV (Combinatorial Purged CV) — the synthesis of Ch 7 + Ch 11 ideas; concept noted in methodology page; full extract deferred to future session if pipeline implements purged CV |
| 13 | Backtesting on Synthetic Data | Skip | Monte Carlo backtest paths; useful but not minimum-viable |
| 14 | **Backtest Statistics** | ✅ **Full extract** | PSR + DSR + Marcos' Third Law + drawdown/TuW metrics. Core methodology. |
| 15 | Understanding Strategy Risk | Skip | Drawdown analysis depth; basics covered by Ch 14 extract |
| 16-22 | Asset Allocation, Microstructure, Big Data, HPC | Skip | Out of scope for current pipeline |

**Net extract: ~3 full chapters + 1 partial = ~4 hours of careful reading. Source-coverage confidence: high for the extracted slice, low for the rest. Future sessions can deepen if/when pipeline graduates to ML strategies or implements CPCV.**

## Honest Caveats

### Voice mismatch

López de Prado writes in a precise, formal-academic register. The wiki's voice has been more practitioner-narrative (Lefèvre, Schwager, Lynch). The methodology page deliberately translates LdP's formalism into plain-language operational rules — but the underlying claims are mathematically grounded, and the methodology page is more *instructional* and less *experiential* than other foundation pages. This is a feature, not a bug — validation discipline benefits from precision — but reads differently from the rest of the wiki.

### Engineering-operationalization gap

**Wiki encoding ≠ pipeline implementation.** As of 2026-05-05, the backtesting lab pipeline does **not** implement:
- Purged k-fold CV with embargo (uses single 70/30 split)
- Sample-uniqueness-weighted bootstrap or `max_samples` adjustment
- Probabilistic or Deflated Sharpe Ratio reporting
- PBO estimation via CSCV
- Trial-count tracking for DSR multiplicity correction

The methodology page is therefore **aspirational discipline**, not a description of current practice. Each concept is flagged with an "Operationalization status: not implemented" marker so the wiki does not overstate the state of the pipeline. Once any of these are implemented in code, update the corresponding flag in the methodology page.

### ML focus may seem off-center

The book is titled *Advances in Financial Machine Learning* and many chapters are pure ML. The wiki's current strategies are mostly DSL-expressed rule-based systems (no ML). **However**, Ch 7 / 11 / 14 are the *non-ML-specific* chapters of the book — the validation methodology applies to any backtest, ML or not. Selection-bias correction does not depend on the strategy being ML. This is why the minimum-viable subset is exactly these three chapters; the ML-engineering chapters (Ch 5, 8, 9, 16+) were correctly skipped.

### Domain-fit gate

Per Part I and the trader-memory rule "off-domain books, even excellent ones, get skipped or cite-only — quality is necessary but not sufficient for wiki ingest": this book passes the domain-fit gate decisively. *Backtest validation* is the central activity of the wiki's trading branch; a rigorous source for it is structurally essential, not adjacent.

### Crypto-specific limitations

- **No crypto examples.** All examples are equities/futures. Crypto-specific patterns (overlapping perp funding cycles, on-chain label leakage from public mempool data, regime-non-stationary triple-barrier outcomes) are not discussed. Translation is straightforward but the trader must do it.
- **Multi-instrument / cross-asset gap.** The wiki's existing Section 8 limitation — the backtesting lab is single-symbol — means several LdP techniques (cross-validation across instruments) are not currently implementable.

### Single-author opinion

The book is highly authored — Marcos' Laws are *his* assertions, and the field has not unanimously adopted DSR thresholds. Some prominent researchers (e.g., Andrew Lo) prefer alternative Sharpe-ratio adjustments. The wiki adopts LdP's framework as the *current best-available* methodology, not as universal canon.

## Key Concepts Extracted (Index)

For the full operational discipline, see [[foundation/methodology/strategy-validation-discipline]]. Index:

1. **Why standard k-fold CV fails in finance** — non-IID labels (overlapping outcome horizons) + serially correlated features → information leaks across train/test boundaries
2. **Purged k-fold CV** — for any test fold, drop training observations whose label horizons overlap the test set
3. **Embargo** — drop training observations *immediately after* the test set (typical: ~1% of T)
4. **Sample uniqueness** — `u_i = average over i's lifespan of (1 / # concurrent labels at t)`; controls bagging `max_samples` and sequential bootstrap to correct for non-IID
5. **Seven sins of quantitative investing** (Luo et al.) — survivorship bias, look-ahead bias, storytelling, data mining, transaction costs, outliers, shorting
6. **Marcos' Second Law** — "Backtesting while researching is like drinking and driving. Do not research under the influence of a backtest."
7. **Six recommendations against overfitting** — entire-asset-class universes, bagging, complete-research-first, record-all-trials, scenario-not-historical, restart-on-failure
8. **Probabilistic Sharpe Ratio (PSR)** — adjusts SR for non-Normal returns + finite track record; should exceed 0.95 for 5% significance
9. **Deflated Sharpe Ratio (DSR)** — PSR with rejection threshold raised by `SR*(N, V[SR_n])` reflecting multiplicity of trials
10. **Marcos' Third Law** — "Every backtest result must be reported in conjunction with all the trials involved in its production. Absent that information, it is impossible to assess the backtest's 'false discovery' probability."
11. **Probability of Backtest Overfitting (PBO)** — estimated via Combinatorially Symmetric Cross-Validation (CSCV); fraction of IS-best strategies that underperform OOS median
12. **Walk-forward critique** — single chronological path is easily overfit; needs randomization across paths to be robust
13. **Backtest-statistic taxonomy** — general (capacity, leverage, hit ratio, holding period); runs (HHI concentration, drawdown, time-under-water); efficiency (SR, IR, PSR, DSR)

## Worked Failure Pattern (Crypto-Specific)

Mapping LdP's framework to the wiki's documented failures:

| Failure | LdP framework explanation | Concrete fix |
|---------|---------------------------|--------------|
| **[[trading/rejected/candle-morphology-batch-2026-04|Candle-morphology batch]] — 10 strategies all negative** | The team ran 10 trials with shared upstream premise. Even if PBO had been measured, this would have been informative: *if 10 negative, the strategy family has low estimated edge — but PBO across the 10 is the discipline we lacked.* The deeper failure is what LdP calls the *Second Law violation* — strategies were tweaked *during* backtesting (parameter ranges adjusted, directions hedged) rather than fully specified before testing. | Specify hypothesis + falsifier *before* running any backtest; pre-register parameter range; run once; if negative, do not adjust |
| **Apr 28 Wyckoff distribution backtest — falsifier fired** | This was the *correct* operation of the framework. Hypothesis stated, falsifier articulated, test run, mechanism rejected. **No PSR/DSR adjustment needed because no trial multiplicity to correct for.** | Continue this pattern. Document the trial count in the rejected page so future related ideas can apply DSR multiplicity correction |
| **May 4 train→val correlation negative for 7/8** | Single 70/30 split is the exact pattern Ch 11.6 critiques — easily overfit because there is only one path of testing that can be re-run with parameter tweaks. | Migrate to purged k-fold CV (Ch 7 protocol) once pipeline supports it. Until then, treat single-split results as suggestive, not validating |

## Action Items Surfaced

These are flagged for user / team review, not unilaterally executed:

1. **Strategy-lab pipeline upgrade** — implement purged k-fold CV with embargo (Ch 7) as alternative to single 70/30. Engineering work; owner candidate: a team engineer per Apr 28 division of labor.
2. **Trial-count tracking** — extend backtest-results page schema to log "# of trials on this dataset" so DSR can be applied. Wiki schema update + pipeline metadata enrichment.
3. **PSR/DSR reporting** — extend backtest-results page schema to include skewness, kurtosis, PSR, DSR. Once pipeline emits these, treat 0.95 threshold as gate for "passed."
4. **PBO methodology** — for any future *batch* of strategies (like the candle-morphology 10), compute PBO via CSCV. Methodology defined in wiki; engineering work to implement.
5. **Pre-registration discipline** — for each strategy, the hypothesis + falsifier + parameter ranges should be locked in a wiki page **before** the backtest runs, per Marcos' Second Law. The wiki already has the the backtest-results section folder structure ready for this — populate it.

## Key Takeaways

- **Validation discipline is the missing structural axis** in the wiki — this source closes the gap.
- **Most published backtests are false discoveries** — and the better the researcher gets at avoiding the seven sins, the more multiple-testing bias dominates.
- **Marcos' Second Law** ("don't research under the influence of a backtest") and **Third Law** ("report every backtest with all trials involved") are the two operational rules with the highest leverage. Adopt before adopting any of the math.
- **PSR / DSR / PBO are statistical tools** — useful, but *operational discipline* (pre-register hypothesis, count trials, restart on failure) matters more than the math.
- **Crypto applicability is high** — selection-bias risk is *higher* in crypto than in equities given regime non-stationarity and high researcher trial counts. The methodology is more load-bearing here, not less.
- **Engineering-operationalization gap is real** — wiki page is aspirational; pipeline does not yet implement these techniques. Per-concept "operationalization status: not implemented" flag in the methodology page tracks the gap honestly.

## Related

- [[foundation/methodology/strategy-validation-discipline]] — the load-bearing extract from this source
- [[foundation/sources/lineage]] — this source occupies the validation-discipline axis (complementary to behavioral / microstructure / practitioner axes)
- [[trading/rejected/candle-morphology-batch-2026-04]] — direct example of failure pattern this methodology addresses
- Part IV Quality Standards — "no overfit worship" + "falsifiers required" — these principles now have a methodological backbone
- [[glossary/purged-cross-validation]]
- [[glossary/deflated-sharpe-ratio]]
- [[glossary/probability-of-backtest-overfitting]]
- [[glossary/sample-uniqueness]]

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*. Wiley. ISBN 978-1119482086.
- Bailey, D. and López de Prado, M. (2012): "The Sharpe ratio efficient frontier." *Journal of Risk*, Vol. 15, No. 2 — PSR foundation paper. <https://ssrn.com/abstract=1821643>
- Bailey, D. and López de Prado, M. (2014b): "The deflated Sharpe ratio." *Journal of Portfolio Management*, Vol. 40, No. 5 — DSR foundation paper. <https://ssrn.com/abstract=2460551>
- Bailey, D., Borwein, J., López de Prado, M., and Zhu, J. (2014, 2017a): backtest overfitting / PBO — CSCV foundation papers. <https://ssrn.com/abstract=2308659> | <http://ssrn.com/abstract=2326253>
- Luo, Y. et al. (2014): "Seven sins of quantitative investing." Deutsche Bank Markets Research — referenced in Ch 11.
- Harvey, C., Liu, Y., and Zhu, H. (2016): "...and the cross-section of expected returns." *Review of Financial Studies* — multiple-testing critique of factor zoo.
- EPUB: `Downloads/advances-in-financial-machine-learning.epub` (extracted to `raw/books/lopez-de-prado/`)
