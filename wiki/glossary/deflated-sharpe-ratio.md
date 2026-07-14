---
title: "Deflated Sharpe Ratio (DSR)"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [glossary, sharpe, validation, multiple-testing, lopez-de-prado, terminology]
---

# Deflated Sharpe Ratio (DSR)

A correction to the Sharpe Ratio that adjusts for **(1) finite sample length, (2) non-Normal returns, and (3) multiplicity of trials**. Builds on the Probabilistic Sharpe Ratio (PSR) by raising the benchmark threshold to reflect how many strategies were tested.

## The motivation

Under the null hypothesis `H_0: SR = 0` (no skill), the **expected maximum** observed SR across `N` trials is greater than zero — and grows with `N`. So the SR threshold for "significant" performance must rise with the trial count. The naïve "SR > 1" or "SR > 2" thresholds were never adjusted for this.

## The Probabilistic Sharpe Ratio (PSR) — base layer

PSR estimates the probability that the **true SR** exceeds a benchmark `SR*`, given an observed `SR_hat` over `T` returns with skewness and kurtosis:

```
PSR(SR*) = Z[ (SR_hat - SR*) × √(T-1) / √(1 - skew × SR_hat + ((kurt-1)/4) × SR_hat²) ]
```

where `Z[.]` is the standard normal CDF. PSR > 0.95 means 95% confidence the true SR exceeds the benchmark.

PSR adjustments:
- ↑ track record `T` → ↑ PSR
- ↑ positive skew → ↑ PSR
- ↑ kurtosis (fatter tails) → ↓ PSR
- ↓ negative skew → ↓ PSR

## The Deflated Sharpe Ratio

DSR = PSR with the benchmark `SR*` raised to reflect multiplicity of trials:

```
SR* = √V[SR_n] × ((1 - γ) × Z⁻¹(1 - 1/N) + γ × Z⁻¹(1 - 1/(N × e)))
```

where `γ` ≈ 0.5772 (Euler-Mascheroni), `N` is number of independent trials, and `V[SR_n]` is the variance of SRs across trials.

DSR > 0.95 → after multiplicity correction, 95% confidence the true SR is non-zero.

## Decision threshold

The standard 5% significance threshold means **DSR > 0.95** for a strategy to "pass" validation. A strategy with PSR=0.96 but DSR=0.5 has noteworthy in-sample performance that may dissolve under multiple-testing correction.

## Crypto-specific concern

Crypto strategies frequently have:
- **Strong negative skew** (long winners, occasional cascade-driven losers)
- **High kurtosis** (Black Thursdays, liquidation cascades)
- **High implicit trial counts** because the public-domain idea pool reaches more saturation faster than equities

A naïve SR of 1.5 on a crypto strategy can correspond to PSR ≈ 0.7 (failing) and DSR ≈ 0.5 (clearly failing) once corrections are applied properly.

## Operationalization status

Strategy-lab pipeline does not currently emit PSR or DSR. Wiki-encoded; engineering work required to compute skewness, kurtosis, and trial count per run.

## Related

- [[foundation/methodology/strategy-validation-discipline|Strategy validation discipline]] — full discipline
- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — primary source (Ch 14)
- [[glossary/probability-of-backtest-overfitting]] — companion test for batch evaluations

## Sources

- López de Prado, M. (2018). *Advances in Financial Machine Learning*, Ch 14.7.2-14.7.3.
- Bailey, D. and López de Prado, M. (2012). "The Sharpe ratio efficient frontier." *Journal of Risk*, Vol. 15, No. 2.
- Bailey, D. and López de Prado, M. (2014b). "The deflated Sharpe ratio." *Journal of Portfolio Management*, Vol. 40, No. 5.
