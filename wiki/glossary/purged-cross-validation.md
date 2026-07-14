---
title: "Purged Cross-Validation"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [glossary, validation, cross-validation, lopez-de-prado, terminology]
---

# Purged Cross-Validation

A k-fold cross-validation scheme adapted for financial time-series where labels overlap in time, by **purging** training observations whose label horizons overlap the test set, and applying an **embargo** window after the test set to prevent leakage from serially correlated features.

## Why standard k-fold CV fails in finance

Standard k-fold CV assumes IID observations. Financial labels — particularly triple-barrier or holding-period labels — span an interval `[t_i,0, t_i,1]`, so consecutive labels share return components by construction. When a test fold contains label `Y_j` and the training set contains `Y_i` with overlapping horizon, **outcome information leaks across the fold boundary**. Performance metrics are inflated.

## The purging step

For each test fold, drop from the training set every observation `i` whose label horizon overlaps any test observation `j`. Two labels `Y_i` and `Y_j` overlap if any of:

1. `t_{j,0} ≤ t_{i,0} ≤ t_{j,1}`
2. `t_{j,0} ≤ t_{i,1} ≤ t_{j,1}`
3. `t_{i,0} ≤ t_{j,0}` and `t_{j,1} ≤ t_{i,1}`

## The embargo step

Even after purging, **serially correlated features** can leak across the test→train boundary in the post-test direction. Drop training observations within an embargo window of size `h ≈ 0.01 × T` after the test set. (Pre-test training data needs no embargo — its information was available at test time.)

## Validation that it worked

If purging + embargo is sufficient, performance plateaus as `k → T`. If performance keeps improving with larger `k`, leakage remains; widen the embargo.

## CPCV (Combinatorial Purged CV)

LdP Ch 12 generalizes purged k-fold CV to **Combinatorial Purged Cross-Validation (CPCV)** — instead of one path through the data, generate combinations of train/test partitions, producing many backtest paths. Each path is purged + embargoed. The result is a *distribution* of OOS metrics rather than a single point estimate.

## Why this matters

The wiki user-memory rule "no multi-year walk-forward for day/swing crypto" names what *not* to do. **Purged k-fold CV (and CPCV) is what to do instead** — preserves chronological structure within folds, avoids leakage, and does not require multi-year stationarity.

## Related

- [[foundation/methodology/strategy-validation-discipline]] — full discipline
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — primary source (Ch 7, Ch 12)
- [[glossary/sample-uniqueness]] — companion correction for non-IID labels
- [[glossary/probability-of-backtest-overfitting]] — uses related CSCV scheme

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*, Ch 7 (Purged k-fold CV) and Ch 12 (CPCV).
