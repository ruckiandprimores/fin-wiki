---
title: "Sample Uniqueness"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [glossary, validation, non-iid, lopez-de-prado, terminology]
---

# Sample Uniqueness

A measure of how independent a label is from its neighbors when label horizons overlap. Equals 1 for fully independent labels (no overlap), approaches 0 for heavily overlapping labels. Used to correct bagging / bootstrap procedures that assume IID observations.

## The setup

In financial labeling — triple-barrier method, holding-period returns, "next-N-bars" classifications — each label `Y_i` spans an interval `[t_{i,0}, t_{i,1}]`. When `t_{i,1} > t_{i+1,0}`, labels `Y_i` and `Y_{i+1}` share a common return component **by construction**. They are not independent.

Two labels `Y_i` and `Y_j` are **concurrent at time `t`** if both are functions of the return at `t`.

## The metrics

For each label `i`:
- `c_t` = number of labels concurrent at time `t`
- `u_{t,i} = 1 / c_t` if label `i` is alive at `t`, else 0
- `ū_i` = average of `u_{t,i}` over label `i`'s lifespan

`ū_i = 1` for fully independent labels (no concurrent labels at any `t`). `ū_i << 1` for heavily overlapping labels.

## Why it matters

Standard bootstrap (used in random forests, bagging) assumes IID. With overlapping labels, the bootstrap **oversamples redundant information**:
- In-bag observations are redundant with each other
- Out-of-bag observations are too similar to in-bag observations
- OOS accuracy is grossly inflated (false confidence)

## The corrections

Three approaches per LdP Ch 4:

1. **Drop overlapping observations entirely** — wastes information; LdP advises against.
2. **Weight bagging by uniqueness**: set `BaggingClassifier(max_samples = mean(ū_i))` so in-bag observations are not sampled at higher frequency than their effective uniqueness.
3. **Sequential bootstrap**: sample observations one at a time with probability inversely proportional to overlap with already-sampled observations.

## Application beyond ML

For DSL / rule-based strategies (no bagging), sample uniqueness is not directly used as a correction. **However** the underlying claim — *overlapping labels reduce effective sample size* — still applies. A backtest with 1000 nominal trades but heavy holding-period overlap may have effective N closer to 200. **Confidence intervals on Sharpe ratio and other statistics should be widened accordingly.**

## Operationalization status

Not implemented in current pipeline. Most directly relevant if/when ML strategies enter the pipeline. For current rule-based DSL strategies, primarily relevant for *honest interpretation* of trade counts and confidence intervals.

## Related

- [[foundation/methodology/strategy-validation-discipline|Strategy validation discipline]] — full discipline
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — primary source (Ch 4)
- [[glossary/purged-cross-validation]] — companion correction; purging removes train-test leakage, sample uniqueness corrects within-train redundancy

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*, Ch 4.2-4.5.
