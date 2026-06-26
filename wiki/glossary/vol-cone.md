---
title: "Vol Cone"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, volatility, regime-detection, sinclair, terminology]
---

# Vol Cone

Empirical **percentile bands of realized volatility** computed across multiple lookback windows on a long historical sample. Provides distributional context for current RV and IV — the operational answer to "is this vol high or low?"

## Construction (Sinclair Ch 4 / Burghart-Lane 1990)

1. Choose lookback windows. Typical: {20, 40, 60, 120, 240} trading days for equity (1mo, 2mo, 3mo, 6mo, 1yr); for crypto day/swing typically {1d, 7d, 14d, 30d, 90d, 180d}.
2. For each window N, compute rolling N-period RV over a multi-year historical sample (typically 2-4 years).
3. Compute percentiles {5, 10, 25, 50, 75, 90, 95} of the rolling RV distribution per window.
4. Plot percentiles against window length → cone shape (short windows wider, long windows narrower).

Sinclair's MSFT example (Ch 4 Table 4.1):

| Percentile | 20-day | 40-day | 60-day | 120-day |
|---|---:|---:|---:|---:|
| Max | 0.465 | 0.352 | 0.287 | 0.258 |
| 75% | 0.213 | 0.213 | 0.225 | 0.200 |
| Median | 0.159 | 0.172 | 0.169 | 0.181 |
| 25% | 0.123 | 0.149 | 0.147 | 0.161 |
| Min | 0.062 | 0.096 | 0.091 | 0.136 |

Short-window vol fluctuates widely; long-window vol is stable.

## Why it works

Three reasons (per Sinclair):

1. **Vol mean-reverts**, so historical bands provide reliable centers
2. **Distribution of vol is approximately log-normal** (right-skewed); percentile bands beat mean-based for skewed distributions
3. **Sampling error in single-window RV is large** (±25% 95% CI on 30-day RV per Sinclair's Table 2.2); the cone re-frames this as distributional rather than point-estimate

## Operational uses

- **Regime classification**: where does current 30-day RV sit on the cone? Below 25th percentile → low-vol regime; above 75th → high-vol regime.
- **IV vs cone comparison**: is the current 30-day IV above the 90th percentile of historical 30-day RV? If yes → variance premium maximal; short-vol candidate.
- **Sample-error context**: a single 30-day RV reading is imprecise; the cone replaces "current RV is X" with "current RV is at the Yth percentile of its distribution."

## Cone depth tradeoff

- **Too shallow** (< 1yr): not enough sample diversity; bands are noisy; regime-change can rotate the entire band structure.
- **Too deep** (> 4yr): includes regimes that no longer exist; bands cover irrelevant historical states.
- **Practical default**: 2-year cone for crypto, 4-year for equity.

## Hodges-Tompkins overlap correction

When using rolling windows (overlapping data), variance estimates from the overlap series are biased. Hodges-Tompkins (2002) correction factor:

```
m = 1 / (1 - h/n + (h² - 1)/(3n²))
```

where `h` = window length, `n` = T - h + 1 = number of subseries. For a 60-day variance from 1006 daily observations: `m ≈ 1.06` (variance) or `~1.03` (volatility). Small enough to ignore in most operational use.

## Crypto adaptation

- 24/7 trading: drop overnight-jump component from RV estimator; **Garman-Klass on hourly or 4h OHLC** is the practical default.
- Cone depth: 2 years covers ~1 cycle reliably; 4 years risks regime non-stationarity.
- Interaction with funding-cycle / scheduled events: a low-vol percentile *2 days before halving* is not the same as low-vol mid-cycle; pair with calendar awareness.

## Related

- [[trading/mechanisms/vol-regime-detection]] — operational mechanism page using vol cone
- [[trading/mechanisms/term-structure-dynamics]] — companion glossary
- [[foundation/sources/sinclair-volatility-trading]] — primary source (Ch 4)
- [[glossary/iv-term-structure]] — companion glossary
- [[glossary/variance-premium]] — companion glossary

## Sources

- Burghart, G., Lane, M. (1990). *How to tell if options are cheap*. Journal of Portfolio Management.
- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 4
- Hodges, S., Tompkins, R. (2002). *Volatility cones and their sampling properties*. Journal of Derivatives.
