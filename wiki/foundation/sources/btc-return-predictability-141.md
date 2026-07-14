---
title: "A Comprehensive Look at Bitcoin Return Predictability — 141 Predictors OOS (Sanhueza, Hardy & Magner 2025)"
status: seedling
created: 2026-07-12
updated: 2026-07-12
tags: [source, academic, preprint, forecasting, out-of-sample, technical-indicators, sentiment, macro, fundamental, day-swing-applicable]
---

# A Comprehensive Look at Bitcoin Return Predictability — 141 Predictors OOS (Sanhueza, Hardy & Magner 2025)

> **TL;DR**: The widest single out-of-sample sweep of BTC return predictors located to date — **141 variables** (technical / fundamental / sentiment / macro), **daily data Sept 2014 – Apr 2025**, evaluated OOS (R²oos + Clark–West) against **random-walk and AR benchmarks**, plus economic-value tests (annualized return, Sharpe) and PCA per-family indices. **Abstract-level headline: trend-based technical indicators consistently beat the benchmarks; macro and fundamental variables are mixed; sentiment is conspicuously NOT named among the winners.** ⚠️ **Seedling / abstract-grade**: the full text could not be accessed this pass (SSRN posting is abstract-only; ResearchGate copy is behind an interactive security gate) — the per-predictor verdict table the paper promises is NOT yet extracted.

## Citation

Aliro Joel Sanhueza (corresponding; PhD candidate), Nicolás Hardy, Nicolás S. Magner — all **Universidad Diego Portales, Santiago, Chile** — *A Comprehensive Look at Bitcoin Return Predictability*, **SSRN Working Paper 5660581**, posted 25 Oct 2025. DOI [10.2139/ssrn.5660581](https://doi.org/10.2139/ssrn.5660581). Title page states: preprint **submitted to International Review of Financial Analysis**, **not peer reviewed**, and constitutes the second chapter of the corresponding author's PhD thesis.

**Author-verification result (2026-07-12):** names, affiliations, and emails verified against the SSRN title page (read directly) and OpenAlex/Crossref records — real researchers at a real institution; Hardy and Magner appear as faculty co-authors on the PhD candidate's thesis chapter. Venue verification: **SSRN preprint only** as of this pass (a ResearchGate mirror exists); no published journal version found.

## Position in lineage

The breadth complement to the wiki's depth-first sources: where [[foundation/sources/fgi-causality-evidence-pair]] interrogates ONE sentiment composite in detail, this paper ranks **141 predictors side by side under a common OOS harness** — the closest thing to a map of *which predictor families carry any out-of-sample signal at all*. Its two benchmarks (random walk, AR) and its two-layer test (R²oos sample-level; Clark–West population-level) mirror the validation discipline this wiki already enforces on its own backtests.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: momentum/trend (the winning family) vs the full menu — the paper is a cross-mechanism referee rather than a single-mechanism study.
- **Axis 2 — Driver**: not mechanism-specific; the trend-following result is conventionally attributed to herding/underreaction in a retail-dominated market (attribution is this wiki's gloss, not the abstract's claim).
- **Axis 3 — Crypto applicability**: **direct** — BTC is the test asset.

## Key findings

**VERIFIED (read directly from the SSRN title page + abstract):**

| Item | Content |
|---|---|
| Predictors | 141 variables "widely used to forecast Bitcoin returns" |
| Sample | daily, September 2014 – April 2025 |
| Design | OOS performance across multiple training and forecasting windows, **including multi-step-ahead forecasts** |
| Metrics | R²oos (sample-level) + **Clark–West** (population-level), vs **random walk** and **AR** benchmarks |
| Economic value | signal-based strategies scored on annualized return + Sharpe |
| Aggregation | PCA indices summarizing the most informative signals per group |
| **Headline verdict** | **"Trend-based technical indicators consistently outperform benchmarks, while macro and fundamental variables yield mixed results."** |

**REPORTED (secondary descriptions — ResearchGate landing snippets; not independently read):**

- Predictors are classified into **four families: technical, fundamental, sentiment-based, macroeconomic**.
- Two-stage design: (1) in-sample fit screen keeping only statistically significant predictors → (2) OOS evaluation of survivors across evaluation windows, training schemes, and horizons; a PCA factor built from significant variables is also evaluated.

**UNVERIFIED (circulated claims that could NOT be confirmed this pass — treat as provisional):**

- Specific OOS horizons of **1/7/14/28 days** — plausible given "multi-step-ahead" in the abstract, but the exact horizon grid is not in any accessible material.
- A count of **564 forecasting exercises** — not confirmable from the abstract.

**The per-family OOS verdict table, abstract-grade (all this pass can honestly support):**

| Family | OOS verdict | Confidence |
|---|---|---|
| Technical — trend-based | **Consistently outperforms** RW + AR benchmarks | Abstract-verified |
| Technical — non-trend | Not separated in the abstract | Unknown |
| Macroeconomic | **Mixed** | Abstract-verified |
| Fundamental (on-chain/network) | **Mixed** | Abstract-verified |
| Sentiment-based | **No verdict stated in the abstract** — its omission from the winners is informative but weaker than a tested null | Inference only |
| PCA per-family indices | Proposed as a scalable summary tool; performance detail inaccessible | Abstract-verified (existence only) |

## Why it matters / honest scope

- **Convergent with the wiki's standing priors**: trend/momentum carrying the only consistent OOS signal matches the "don't fight the slow trend" evidence line and the regime-detection stance ([[foundation/methodology/online-regime-detection]]) that the durable machine edge is *with-the-trend* mechanics — while the weak/mixed showing of everything else matches the prior that most published predictors don't survive OOS.
- **Sentiment triangulation**: read together with [[foundation/sources/fgi-causality-evidence-pair]], the picture is consistent — sentiment composites describe state, they don't lead returns. But note the asymmetry in evidence quality: the FGI pair contains a *tested* null; here sentiment's weakness is only *inferred from omission* in an abstract.
- **Honest scope (load-bearing)**: **non-peer-reviewed preprint, abstract-only access, PhD-thesis chapter.** The headline could shift in review; the per-predictor table, horizon-by-horizon results, transaction-cost handling, and the sentiment family's explicit numbers all remain unextracted. This page should be upgraded (status → growing, table filled) if/when the full text becomes accessible or the IRFA-published version lands. Until then: **directional evidence, not a citable result grid.**
- Also note the survivorship structure of the design (in-sample screen → OOS on survivors, per REPORTED description): family-level conclusions survive this, but any *individual* predictor's OOS stat would need the usual multiple-testing caution.

## Related

- [[foundation/sources/fgi-causality-evidence-pair]] — the sentiment-composite deep dive this sweep complements
- [[foundation/methodology/regime-marker-data-sources]] — where the underlying feeds (sentiment, on-chain, macro) live as backtestable series
- The trend-based descriptors on the live operational state card (outside this wiki) are exactly the family this paper's OOS evidence favors
- [[foundation/methodology/online-regime-detection]] — trend/state description as the machine-durable layer

## Sources

- [SSRN 5660581](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5660581) — abstract page + posted title-page PDF read directly 2026-07-12 (the SSRN posting itself is **1 page**: title/abstract/declarations only).
- [ResearchGate 396908181](https://www.researchgate.net/publication/396908181_A_Comprehensive_Look_at_Bitcoin_Return_Predictability) — full-text copy exists but sits behind an interactive security check; only landing-page snippets (via search) used, flagged REPORTED above.
- Crossref + OpenAlex metadata cross-check 2026-07-12.
