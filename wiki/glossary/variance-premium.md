---
title: "Variance Premium"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, volatility, options, sinclair, terminology]
---

# Variance Premium

The persistent tendency for **implied volatility (IV) to exceed subsequent realized volatility (RV)**. Selling vol against this spread has historical positive expectancy in equity indices; the spread is the option market's risk-neutral pricing of vol that systematically over-states the physical-measure (statistical) vol.

## Sinclair's quantification (Ch 4)

For SPY 30-day RV vs VIX (1990-mid-2012):

| VIX bucket | Days in bucket | Avg IV-RV spread |
|---|---:|---:|
| VIX < 20 | 3,102 | 27.75% |
| 20 < VIX < 30 | 1,925 | 19.78% |
| 30 < VIX < 40 | 407 | 13.73% |
| 40 < VIX < 50 | 111 | 9.26% |
| VIX > 50 | 56 | −11.52% |

Average ~3 vol-points; **percentage spread shrinks as IV rises**. Counterintuitive — the variance premium is larger in *calm* regimes than in *crisis* regimes (when premium-capture is most attractive in absolute terms, the per-unit spread is smaller).

## Three components (Sinclair Ch 11) {#three-components}

1. **ATM variance premium** — straight insurance / risk aversion compensation; smallest of the three components in absolute terms.
2. **OTM put variance premium** — downside protection demand; largest single component in equity indices; Kozhan-Neuberger-Schneider (2011) attribute ~half to realized skewness (correlation between returns and vol).
3. **OTM call variance premium** — lottery-buyer premium; calls priced for upside that rarely materializes.

## Why it persists

Per Sinclair Ch 4 + 11:
- **Insurance demand**: protective-put buying creates structural bid; market-makers charge premium to take the risk.
- **Behavioral**: investors overestimate crash probabilities (Bakshi-Madan 2006 — risk aversion + non-normality model).
- **Microstructure**: market-makers bias quotes upward to defend against adverse selection (small structural bias each trade).
- **Correlation premium** (index-specific): index variance = component variance × correlation; correlation rises in crashes, so index vol-of-vol > component vol-of-vol; index OTM puts price the correlation tail.

## Crypto applicability

Variance premium is **real and persistent on Deribit** for BTC and ETH options:
- DVOL (Deribit's VIX-equivalent) consistently above subsequent 30d realized vol (~2-5 vol-points spread on average per crypto-options literature 2020-2025)
- Magnitude varies more than equity (regime non-stationarity)
- Skew-decomposition flips: in bull mania, OTM-call premium can exceed OTM-put premium (lottery-buyers > protection-buyers)

### PDF-grade quantification (Almeida, Grith, Miftachov 2024) — added 2026-05-18

Single most rigorous academic source for crypto VRP magnitudes (sample Jul 2017 – Dec 2022, 7.8M Deribit option transactions):

- **BTC annualized variance premium = 0.14** (risk-neutral variance 0.72 vs realized variance 0.58)
- **SPX equivalent ~0.02** → **BTC VRP ~7× SPX**
- **Counterintuitive regime conditioning** (mirrors Sinclair's equity finding but sharper):
  - HV cluster: BVIX² 0.86, RV 0.74 → premium **0.12**
  - **LV cluster: BVIX² 0.46, RV 0.29 → premium 0.17** (LARGER)
- **2025 regime context**: DVOL hit 2-year low of 37% (Aug 2025) — places market in LV cluster favorable for harvest
- **Non-transfer from broad-carry collapse**: per [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]], broad-carry Sharpe collapsed 6.45 → negative in 2025; VRP did NOT decay analogously because the counterparty stack is different (options buyers vs leveraged longs). See [[../trading/mechanisms/variance-risk-premium-crypto#vrp-vs-broad-carry|variance-risk-premium-crypto §vrp-vs-broad-carry]].

## Concrete strategies that capture it (Sinclair Ch 11-12)

- **Short straddles/strangles** on QQQ second-month options 2000-2010: Sharpe 1.1-1.3 unconditional; 20-35% drawdowns
- **VIX-conditional filter** (sell only when VIX < 35): smoother PnL, similar return
- **VIX-EWMA-conditional filter**: Sharpe 1.68 / 10% DD historically — but curve-fit-prone per Sinclair's caveats
- **Sell OTM puts on indices**: captures the OTM-put-skewness component specifically
- **Crypto analog**: short DVOL-implied straddles on BTC; short OTM puts on Deribit; short OTM calls during bull mania

## Honest caveats

- **Tail-risk catastrophe**: the trade pays steadily then loses big; the 2008 vol-explosion or 2022 LUNA/FTX cascades wipe out years of premium capture.
- **Capacity limits**: more vol-sellers entering Deribit each year compresses the premium gradually.
- **Magnitude is not stationary**: post-2010 equity variance-premium has compressed; assume crypto premium is currently larger but trending toward equity-like over multi-year horizon.
- **Per-trade economics**: even at Sharpe 1.0+ unconditional, friction-floor (bid-ask + commission) can eat 30-50% of gross edge at retail scale.

## Related

- **[[../trading/mechanisms/variance-risk-premium-crypto]]** — **NEW 2026-05-18**: crypto-specific operational mechanism (this glossary is the general concept; that page is the deployable mechanism family)
- **[[../trading/mechanisms/insurance-premium-harvest-overview]]** — **NEW 2026-05-18**: cross-mechanism index (VRP harvest vs delta-neutral funding harvest)
- **[[../trading/mechanisms/delta-neutral-funding-harvest]]** — **NEW 2026-05-18**: sibling insurance-premium family on the perp axis (different counterparty stack)
- [[../trading/mechanisms/vol-regime-detection]] — IV-RV spread is the regime classifier input
- [[../trading/mechanisms/term-structure-dynamics]] — variance premium has term-structure (longer-tenor more premium-laden)
- [[../trading/mechanisms/skew-based-positioning-signals]] — skew decomposes variance premium across strikes
- [[../foundation/sources/sinclair-volatility-trading]] — primary source (Ch 4, 11)
- [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]] — broad-carry trajectory; non-transfer to VRP discipline
- [[vol-cone]] — companion glossary
- [[iv-term-structure]] — companion glossary
- [[vol-skew]] — companion glossary
- [[funding-rate-carry]] — sister concept on perp axis

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 4 (Table 4.3 quantification), Ch 11 (decomposition)
- Coval, J., Shumway, T. (2001). *Expected option returns*. Journal of Finance.
- Bakshi, G., Madan, D. (2006). *A theory of volatility spreads*. Management Science.
- Carr, P., Wu, L. (2009). *Variance risk premiums*. Review of Financial Studies.
- Kozhan, R., Neuberger, A., Schneider, P. (2011). *The skew risk premium in the equity index market*.
