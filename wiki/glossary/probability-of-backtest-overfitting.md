---
title: "Probability of Backtest Overfitting (PBO)"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [glossary, validation, multiple-testing, cscv, lopez-de-prado, terminology]
---

# Probability of Backtest Overfitting (PBO)

The estimated probability that the **in-sample-best strategy from a batch of `N` trials underperforms the median strategy out-of-sample**. PBO ≈ 0.5 means picking the IS-best is no better than picking randomly — pure overfitting. PBO ≈ 0 means IS performance is informative about OOS.

## The procedure (CSCV)

Estimated via **Combinatorially Symmetric Cross-Validation (CSCV)**:

1. Form a `T × N` matrix where columns are the N trial PnL series and rows are time observations.
2. Partition into `S` equal-size submatrices by row (typical: S=16).
3. For every combination of `S/2` submatrices forming a "training" set and the complement forming a "test" set (~12,780 combinations for S=16):
   a. Find the trial with best in-sample performance.
   b. Compute its rank in out-of-sample performance.
   c. Compute logit `λ_c = log(rank / (1 - rank))`.
4. PBO = `Pr(λ_c < 0)` across all combinations — the fraction where IS-best underperformed the OOS median.

## Decision rule

- **PBO ≈ 0**: IS-best is genuinely informative about OOS. Selecting from the batch is signal.
- **PBO ≈ 0.5**: pure overfitting. The "best" IS strategy is a random pick OOS.
- **PBO > 0.5**: anti-correlation — picking IS-best actively hurts OOS, indicating systematic mining.

## When to apply

Whenever a *batch* of related strategies is evaluated:
- Parameter sweeps (`N` thresholds tried, pick best)
- Mechanism-family explorations (`N` related strategies in one family)
- Direction tests (long vs short variants)

For a single strategy with no parameter exploration, PBO is not meaningful (but [[glossary/deflated-sharpe-ratio|DSR]] is).

## Why this matters for the wiki

Direct application to the [[trading/rejected/candle-morphology-batch-2026-04|candle-morphology batch]]: 10 trials shared an upstream premise. CSCV across the 10 PnL series would have produced a quantitative confidence in the batch-level rejection — different from "all 10 negative happened to occur."

For any future *batch* evaluation, CSCV → PBO is the discipline.

## Operationalization status

Not implemented in pipeline. Wiki-encoded as required discipline for batch evaluations; engineering work to implement CSCV computation across strategy run outputs.

## Distinction from purged CV

[[glossary/purged-cross-validation|Purged CV]] is a *training/testing protocol* for a single strategy. PBO via CSCV is a *batch-evaluation statistic* for selecting among many strategies. They solve different problems and can be combined — purged CV produces honest single-strategy performance; CSCV evaluates the selection process across a batch of those.

## Related

- [[foundation/methodology/strategy-validation-discipline|Strategy validation discipline]] — full discipline
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — primary source (Ch 11.6)
- [[glossary/deflated-sharpe-ratio|Deflated Sharpe Ratio]] — companion correction; DSR for single strategy with N trials, PBO for batch evaluation
- [[glossary/purged-cross-validation]] — different layer of validation
- [[trading/rejected/candle-morphology-batch-2026-04]] — concrete batch this would apply to

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*, Ch 11.6.
- Bailey, D., Borwein, J., López de Prado, M., Zhu, J. (2017a). "The probability of backtest overfitting." *Journal of Computational Finance*, Vol. 20, No. 4.
- Bailey, D., Borwein, J., López de Prado, M., Zhu, J. (2014). "Pseudo-mathematics and financial charlatanism." *Notices of the American Mathematical Society*, Vol. 61, No. 5.
