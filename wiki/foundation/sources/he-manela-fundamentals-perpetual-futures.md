---
title: "He, Manela, Ross, von Wachter — Fundamentals of Perpetual Futures (2022, latest v5 2024)"
status: growing
created: 2026-05-15
updated: 2026-05-15
tags: [source, academic, crypto-native, perpetual-futures, funding-rate, basis-trade, arbitrage, momentum, day-swing-applicable]
---

# He, Manela, Ross, von Wachter — Fundamentals of Perpetual Futures

> **TL;DR**: Foundational academic paper on crypto perpetual futures pricing. Authors Songrun He (WashU), Asaf Manela (WashU), Omri Ross, Victor von Wachter. arXiv:2212.06888, latest v5 August 2024 (peer-review-feedback-incorporated across 5 versions Dec 2022 → Aug 2024). **Headline empirical findings**: (1) crypto perp pricing deviations from no-arbitrage are **60-90% absolute per year** — substantially larger than traditional currency markets (Du-Tepper-Verdelhan 2018 baseline); (2) random-maturity arbitrage strategy yields **Sharpe 1.8 at retail trading costs, up to 3.5 for market makers with no fees** on Bitcoin (ETH and altcoins even better); (3) deviations **comove strongly across cryptocurrencies** — attributed to commonality in arbitrageur funding + liquidity constraints; (4) deviations **decline ~11% per year** — markets converging toward efficiency; (5) **past-return momentum explains >50% R²** of the futures-spot gap (positive-feedback trading is mechanically embedded). **Mechanism descriptions**: 8-hour funding payment mechanism described; perps cannot be replicated by rolling 8h forwards (8h forwards converge to spot, perps don't). **Why it matters for the wiki**: first wiki-encoded academic primary source for perpetual-futures pricing mechanics (Hayes is practitioner-narrative; this is the formal empirical paper). Provides **arbitrage-Sharpe benchmark** that contextualizes funding-carry capture (the operational form of this arbitrage strategy) and constrains expectations on funding-cycle-dynamics strategies.

## Citation

**He, S., Manela, A., Ross, O., von Wachter, V.** (2024). *Fundamentals of Perpetual Futures*. arXiv:2212.06888 [q-fin.PR]. Originally submitted Dec 13, 2022; revised through 5 versions; latest v5 Aug 21, 2024.

- arXiv: https://arxiv.org/abs/2212.06888
- HTML v5: https://arxiv.org/html/2212.06888v5

## 3-Axis Tags

- **Economic mechanism**: Carry / basis-trade (perp-vs-spot basis tracker mechanically built into funding mechanism)
- **Behavioral/structural driver**: Capacity constraint (arbitrageur liquidity insufficient to eliminate deviations); momentum / positive-feedback (futures-spot gap explained >50% by past returns)
- **Crypto applicability**: Direct — paper studies crypto perpetual futures specifically

## Key empirical findings

### 1. Pricing deviation magnitudes — substantially larger than traditional currency markets

| Metric | Crypto perpetual futures (this paper) | Traditional currency markets (Du-Tepper-Verdelhan 2018) |
|---|---|---|
| Mean absolute deviation from no-arbitrage | **60-90% per year** across cryptocurrencies | Much smaller (CIP violations measured in basis points per month) |
| Statistical significance of mean deviation | Modest and statistically insignificant on average | — |
| Volatility of deviation around mean | High | Lower |

**The benchmark is useful despite the volatility** — deviations are mean-reverting around the no-arbitrage price, but the path is noisy. The 60-90% absolute per year is the volatility around the mean, not a persistent bias.

### 2. Arbitrage strategy Sharpe ratios — concrete deployment-grade benchmark

Random-maturity arbitrage strategy across the 8-hour funding cycle:

| Setting | Asset | Sharpe ratio |
|---|---|---|
| Retail trading costs (Binance highest-tier fees) | Bitcoin | **1.8** |
| Market-maker with no fees | Bitcoin | up to **3.5** |
| (Same setup) | Ethereum | "Even better than Bitcoin" |
| (Same setup) | Other cryptocurrencies | "Even better than Bitcoin" |

**Wiki-relevance for funding-carry deployment**: a live funding-carry deployment is the operational form of exactly this arbitrage class — short perp + long spot (or vice versa) capturing the funding-rate carry. The paper's Sharpe 1.8 at retail costs is roughly comparable to a live deployment's risk-adjusted return; a deployment's higher Sharpe likely reflects (a) market-maker-tier execution rather than retail, (b) selective trade entries on FRA signal vs random-maturity, (c) the 2024-2026 ETF era specifically driving the basis-trade-favoring side of the carry distribution. The 1.8-3.5 academic Sharpe range is the **theoretical bound benchmark** for funding-carry strategies.

### 3. Cross-asset comovement — basis spread is systematic, not idiosyncratic

**Strong comovement of the futures-spot gap across different cryptocurrencies.** Authors attribute this to:

- **Commonality in funding + liquidity** faced by arbitrageurs operating across multiple cryptocurrencies
- **Common sentiment effects** across crypto

**Wiki-relevance for etf-era + cross-asset analysis**: the cross-asset comovement finding sharpens the substrate observation (per this research program asset-vol-scaling N=3) that vol-regime-transition translates across BTC/ETH/SOL with asset-scaled parameters — there's a deeper structural commonality across crypto perp markets that maps onto basis-tracker shared dynamics. The asset-vol-scaling N=3 finding is one piece of the cross-asset commonality pattern this paper documents at the price-deviation level.

### 4. Time-decay of deviations — ~11% per year — crypto markets becoming efficient

**Deviations decline approximately 11% annually**, attributed to increasing arbitrage capital + competition.

**Wiki-relevance**:

- **Per a standing convention + a standing convention**: this is empirical evidence that the funding-carry edge **decays over time** as arbitrage capital grows. Strategy persistence reason ≠ "hidden-via-transfer"; it's "capacity-constrained" — and the capacity is increasing. A live funding-carry deployment may see Sharpe decay over the 2-3 year horizon as competing arbitrageurs scale.
- Sharpens the funding-carry-as-persistent-edge framing on [[trading/mechanisms/funding-cycle-dynamics]] — edge is real, edge is decaying. Calibrates deployment-horizon expectations.

### 5. Momentum / positive-feedback dynamics — futures-spot gap is partly trend-following

**Past return momentum significantly explains the futures-spot gap with R² over 50%.** Mechanism: positive-feedback trading behavior — when crypto rallies, demand for long perp pushes perp price above spot, funding goes positive, sustained until arbitrageurs unwind. Conversely on declines.

**Wiki-relevance for behavioral-positioning-unwind + funding-cycle-dynamics**:

- The R² > 50% explanation IS the [[trading/mechanisms/behavioral-positioning-unwind]] mechanism at the funding-rate observable level — crowded directional positioning translates mechanically into funding-rate extremes.
- This is the most quantitatively-anchored empirical evidence for the wiki's claim that funding-rate extremes reflect speculative positioning (pre-2024) AND for the [[trading/mechanisms/etf-era-carry-trade-funding-distortion|distortion thesis]] that institutional flow (ETF cash-and-carry) mixes a non-momentum driver into the same signal in the ETF era.

### 6. Liquidity insufficiency — primary mechanism for deviation persistence

**Liquidity in crypto markets is insufficient for arbitrageurs to eliminate the no-arbitrage violations.** This is the structural reason deviations don't immediately collapse to zero.

**Wiki-relevance**: the wiki's `crypto-carrier-features.md` page (not yet read in this session) likely covers liquidity as a feature; this paper provides academic-grade empirical backing for the claim that **crypto's structurally lower liquidity** is why mechanisms that traded away in equity / traditional FX still survive in crypto. Per the wiki architecture (ARCHITECTURE.md) Part I §1 "crypto ≈ pre-1980s US equities" prior — this paper's liquidity-insufficiency finding is the empirical anchor.

## Mechanism description — 8-hour funding payment mechanism

The paper documents:

- Perp funding paid every 8 hours
- Funding rate approximately equals the average futures-spot spread over the preceding 8 hours
- **Perps cannot be replicated by rolling over 8-hour maturity futures** — 8h forwards converge to spot at expiry; perps don't expire and don't converge mechanically

This formalizes the perp design narrative in [[foundation/sources/hayes-essays]] (Hayes co-built the BitMEX XBTUSD perp 2016). Hayes is practitioner-narrative ("how we built it and why"); this paper is the formal pricing model ("what the no-arbitrage benchmark is and how far perp deviates from it").

## Honest caveats

- **Window**: data extends through mid-2024; ETF-era (Jan 2024 onward) is only partially covered. Post-ETF-launch dynamics that the wiki's [[trading/mechanisms/etf-era-carry-trade-funding-distortion|etf-era page]] documents (institutional cash-and-carry mixing into funding signal) are largely outside the paper's primary sample.
- **Academic-vs-deployment Sharpe**: the 1.8-3.5 range is a paper-study Sharpe; a live deployment's realized return is operational. Direct comparison requires accounting for venue-specific execution + slippage + funding-payment timing (per [[foundation/methodology/deployment-edge-threshold#opportunity-cost-gate|deployment-edge-threshold §5A]] live-vs-backtest caveats).
- **No-arbitrage benchmark is theoretical**: deviation magnitudes (60-90%) assume the no-arbitrage price is computable; in practice the "true" no-arbitrage requires zero funding + liquidity costs + counterparty risk, which is unrealistic.
- **Sample is pre-Oct-2025 cascade**: October 10-11 2025 + September 2025 cascade events (per [[trading/mechanisms/liquidation-cascade-aftermath|liquidation-cascade-aftermath]] §7 case study) likely produced extreme deviation observations that may pull the long-tail of the 60-90% absolute distribution; paper's averages may understate post-2025 magnitudes.

## What the paper does NOT cover

- Specific 2024-2025 ETF-era empirics (out of sample for the primary results)
- Cross-venue funding-rate dispersion (covered by [[foundation/sources/two-tiered-funding-rate-markets]] instead)
- Institutional vs retail decomposition of the funding signal
- Forward-looking funding rate prediction models

## Implications for wiki priorities

- **Funding-cycle-dynamics**: sharpening — add academic-paper anchor on the 8h mechanism + arbitrage Sharpe range + time-decay finding
- **etf-era page**: existing distortion thesis remains valid (this paper's sample largely pre-ETF); cross-asset comovement provides theoretical scaffolding
- **Funding-carry deployment framing**: per the opportunity-cost gate (deployment-edge-threshold §5A), a live deployment's risk-adjusted return sits at the upper end of this paper's theoretical 1.8-3.5 range, consistent with selective-entry execution + ETF-era favorable conditions; **expect downside on long horizon as the 11%/yr deviation decay compounds**
- **Behavioral-positioning-unwind**: the R²>50% momentum-explains-basis finding is the deepest empirical anchor for the family's mechanism story
- **Deployment-edge-threshold**: arbitrage Sharpe 1.8-3.5 range as theoretical-bound reference

## Related

- [[foundation/sources/hayes-essays]] — practitioner narrative on perp design (BitMEX 2016); this paper is the formal-pricing companion
- [[foundation/sources/two-tiered-funding-rate-markets]] — sister academic paper covering cross-venue funding-rate market structure (sister-source, distinct dimension)
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; this paper's empirical anchors slot under positioning-extreme + carry-capture sub-mechanisms
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — extends this paper's pre-2024 framing into the ETF-era distortion thesis
- [[trading/mechanisms/behavioral-positioning-unwind]] — momentum R²>50% finding is the empirical-anchor for the family
- [[foundation/methodology/deployment-edge-threshold]] §5A opportunity-cost gate — funding-carry deployment Sharpe context vs this paper's 1.8-3.5 range

## Deep-dive findings (2026-05-15 PDF read — full extraction)

User-supplied PDF of v6 (Aug 2024 final version, arXiv:2212.06888v6, 64 pages) ingested. Key empirical tables now wiki-encoded:

### Sample window + universe

- **5 cryptocurrencies**: BTC, ETH, BNB, DOGE, ADA — single venue (Binance)
- **Sample**: Jan 8, 2020 — Mar 11, 2024 (1,495 daily observations)
- Hourly granularity for ρ deviation; daily aggregation for regressions
- Coauthor cluster: Songrun He (WashU), Asaf Manela (WashU + Reichman), Omri Ross (Copenhagen + eToro), Victor von Wachter (Copenhagen)

### Theoretical contribution — closed-form no-arbitrage price

The paper's main result: the no-arbitrage perpetual futures price is **proportional to spot**:

```
F_t = λ · S_t,  where  λ = κ / (κ − (r − r'))
```

- κ = funding-rate intensity (proportionality between funding rate and futures-spot spread)
- r = cash-borrowing interest rate
- r' = spot-cost-of-carry

The futures-spot spread is **larger when cash borrowing is expensive relative to the funding rate**. This is "random-maturity arbitrage" — opportunity exists but with uncertain time-to-close.

### Table 4 — Per-asset mean absolute deviation magnitudes

The headline "60-90% per year deviations" decomposes per-asset:

| Asset | Mean ρ | Median ρ | Std(ρ) | Mean \|ρ\| | Median \|ρ\| | Std(\|ρ\|) |
|---|---:|---:|---:|---:|---:|---:|
| BTC | -0.06 | -0.32 | 0.74 | **0.57** | 0.51 | 0.48 |
| ETH | 0.04 | -0.24 | 0.85 | **0.63** | 0.53 | 0.57 |
| BNB | -0.10 | -0.17 | 1.22 | **0.88** | 0.70 | 0.85 |
| DOGE | 0.01 | -0.28 | 1.38 | **0.86** | 0.64 | 1.07 |
| ADA | 0.03 | -0.21 | 1.14 | **0.83** | 0.70 | 0.79 |

Mean ρ is statistically **indistinguishable from zero** (Newey-West p-values 0.31-0.85) — confirms the theoretical benchmark is unbiased. Mean |ρ| is highly significant (p < 0.01 for all 5 assets) — confirms there's substantial volatility around the benchmark.

**BTC has smallest |ρ| (0.57); BNB/DOGE/ADA have largest (~0.85)** — implies altcoin perp markets are more dislocated than BTC perp.

### Table 5 — Cross-asset correlation matrix for ρ deviations

Strong cross-asset comovement of futures-spot gap:

| | ρ_ETH | ρ_BNB | ρ_DOGE | ρ_ADA |
|---|---:|---:|---:|---:|
| ρ_BTC | **0.89** | 0.58 | 0.76 | 0.82 |
| ρ_ETH | — | 0.61 | 0.76 | 0.84 |
| ρ_BNB | — | — | 0.58 | 0.56 |
| ρ_DOGE | — | — | — | 0.78 |

BTC-ETH correlation = 0.89 is exceptionally high. Funding rates also highly correlated (BTC-ETH funding rate corr = 0.86). **Spot returns** of these cryptos are also correlated (BTC-ETH spot return corr 0.84) — common market factor.

Author interpretation: comovement reflects **shared funding/liquidity constraints faced by arbitrageurs** (Brunnermeier-Pedersen 2009; Gârleanu-Pedersen 2011) + common investor sentiment.

### Table 6 — Random-maturity arbitrage Sharpe by asset × fee tier

The headline "Sharpe 1.8 to 3.5 on Bitcoin" decomposes:

| Asset | No fees | Low fees | Medium fees | High fees |
|---|---:|---:|---:|---:|
| BTC | **3.53** | 2.20 | 2.16 | **1.80** |
| ETH | **4.83** | 2.95 | 2.86 | **2.55** |
| BNB | **12.31** | 7.40 | 5.76 | **4.84** |
| DOGE | **10.35** | 6.69 | 4.85 | **3.58** |
| ADA | **10.43** | 5.53 | 3.46 | **2.68** |

Fee tiers in basis points: Spot (Futures) — Low 2.25 (0.18); Medium 4.5 (0.72); High 6.75 (1.44).

**Annualized returns at high fees**: BTC 6.38%, ETH 9.59%, BNB 18.41%, DOGE 23.31%, ADA 12.03%.

**Active time** (fraction of year strategy is in a position): No-fees ~100%; high-fees 20-35% — selective entry materially increases per-trade Sharpe.

**Alpha vs Liu-Tsyvinski-Wu (2022) 3-factor model**: highly significant for all 5 assets, even at high fees (BTC α = 4.38, t-stat 1.89 at high fees; BNB α = 20.36 at high fees, t-stat 6.82). Strategy is NOT capturing market beta — it's a structural arbitrage edge.

**Wiki-relevance N=1 against funding-carry deployment framing**:

- A live funding-carry deployment's risk-adjusted return sits within the academic range of roughly **1.8–2.6** (BTC high-fee 1.80 to ETH high-fee 2.55). A deployment trading BTC that lands materially ABOVE the academic random-maturity BTC high-fee 1.80 baseline would confirm selective-entry alpha (vs the paper's threshold-mechanical strategy that activates on every ρ exceeding bound)
- The 4.84 BNB / 3.58 DOGE / 2.68 ADA Sharpes at high fees suggest **altcoin perp carry strategies could outperform BTC perp carry** materially — informs multi-asset extension expectations for funding-carry strategies
- BTC altcoin Sharpe declines monotonically with fee tier; **fee-tier optimization is the highest-leverage variable** for deployment-grade carry strategies (high-fee Sharpe is 30-40% of no-fee Sharpe across all assets)

### Table 10 — Time-trend regression: the source of the 11%/yr decay claim

The "deviations decline 11%/yr" finding comes from a panel regression with currency fixed effects:

```
|ρ_{i,t}| = β₀_i + β₁·(t/365) + β₂·R_{i,t−120:t−1} + β₃·σ_{i,t−120:t−1} + ε_{i,t}
```

Estimated coefficients (full specification, Table 10 col 6):

| Variable | β | t-stat |
|---|---:|---:|
| Time trend (yrs) | **−0.10*** | −5.00 |
| Past returns (120d ann.) | 0.06*** | 5.00 |
| Past volatility (120d ann.) | −0.01 | −1.50 |
| Currency FE | Yes | — |
| R² | 0.14 | — |
| N | 7,235 | — |

**The decay is LINEAR** (not exponential) at 10-14%/yr depending on specification. Confirmed across 3 specifications with currency FE. Newey-West t-statistics with 120-day lag.

For ρ (signed, not absolute): time trend β = −0.14*** to −0.25*** depending on spec; R² up to 0.33.

**Volatility coefficient is NOT significant** — the funding-constraint hypothesis (volatility proxy → arbitrageur VaR binding) fails empirically. The driver of comovement is **demand-side sentiment**, not supply-side capital constraints. This is a load-bearing methodology finding for [[trading/mechanisms/behavioral-positioning-unwind|behavioral-positioning-unwind]] — the family-mechanism story (positioning-extreme reflects behavioral overshoot) is consistent with the demand-side driver this paper identifies.

**R² of 14% on |ρ|** — much smaller than the abstract's claimed ">50%" for "past-return momentum explains futures-spot gap". The 50% figure must reference a different specification (per-currency? univariate?) not surfaced in the Table 10 main regression. Wiki-encoding the conservative Table 10 R²=0.14 figure pending follow-up.

### Trading-volume context

| Year | Daily volume (median) | Perp/spot ratio |
|---|---:|---:|
| 2020 | **$17.8 bn** | — |
| 2021 | **$132.0 bn** | — |
| 2022 | **$101.9 bn** | 2-3× spot |

Per [[trading/mechanisms/liquidation-cascade-aftermath#7-october-10-11-2025-etf-era-19b-tariff-triggered-deleveraging-cascade|Oct 2025 cascade]]: perp monthly volume ~$1.33T (~$44B daily) — market has grown 2-3× since this paper's sample.

### Three explained market episodes

**March 2020 COVID**: "funding rates turned substantially negative in most exchanges" — perpetual prices below spot during liquidity vacuum. Sharpens [[liquidation-cascade-aftermath#1-march-12-13-2020-covid-liquidity-vacuum|liquidation-cascade-aftermath case #1]].

**Early 2021 bull-run**: "funding rates turned highly positive" — perpetual prices above spot during retail leverage extension. Sharpens [[foundation/macro/btc-regime-taxonomy-2020-2025|btc-regime-taxonomy]] funding-rate marker calibration.

**FTX collapse (Nov 2022)**: funding rates on solvent exchanges turned **substantially negative** (perp prices below spot — shorts paying longs) while FTX showed opposite pattern. Authors attribute to FTX investors liquidating short positions to reduce exposure to failing exchange OR involuntary liquidation. Sharpens [[liquidation-cascade-aftermath#4-november-8-11-2022-ftx-collapse|liquidation-cascade-aftermath case #4]] with funding-mechanism-specific channel for the FTX-era price dislocation.

### Cross-currency strong factor structure

Authors identify a **common factor** across crypto futures-spot deviations consistent with Lustig-Roussanov-Verdelhan (2011) traditional FX literature. This factor is **distinct from the common market factor** in spot returns:

- Spot returns correlation BTC-ETH: 0.84
- Funding rate correlation BTC-ETH: 0.86
- ρ deviation correlation BTC-ETH: 0.89
- BUT: ρ vs spot returns correlation: only 0.00 to 0.18 (weak)

**Different forces drive spot-return-comovement vs ρ-comovement** — useful caveat against conflating these as the same factor. Implications for cross-asset trade design.

### Alameda + 3AC implications (paper-author interpretation)

Per pp. 4-5: Alameda Research + Three Arrows Capital "seem to have pivoted from such arbitrage activity around late 2021 to early 2022 and started taking more directional bets". The 11%/yr decay finding — driven by arbitrage-capital growth + competition (Kondor 2009) — is consistent with major arbitrage capital exiting the carry trade in pursuit of higher-Sharpe directional bets, which subsequently bankrupted both firms when crypto declined in 2022.

**Wiki implication**: the funding-carry edge has a **capital-rotation regime** dimension. When carry-Sharpe falls below opportunity-cost-of-directional-bets, sophisticated arbitrageurs exit, which paradoxically RAISES carry-Sharpe for remaining participants. This is consistent with the Borri-et-al-2025 carry-trajectory revival hypothesis — if 2025-negative drives capital out, the bottom may form when carry-Sharpe rises enough to attract patient capital again.

## Companion sources cluster (2026-05-15)

The wiki now has an **academic-anchor cluster** on perpetual-futures empirics:

| Paper | Lens | Scope |
|---|---|---|
| He, Manela, Ross, von Wachter (2024) [this page] | Pricing fundamentals + arbitrage | Single-venue (Binance); 5 assets; 4-year sample (2020-2024) |
| [[foundation/sources/two-tiered-funding-rate-markets|Zhivkov (Math 2026)]] | Cross-venue market structure | 26 exchanges; 749 symbols; 8-day snapshot |
| [[foundation/sources/temporal-dynamics-funding-rates|Zhivkov, Todorov, Georgiev (IJFS 2026)]] | Cross-venue temporal evolution | 26 exchanges; 812 symbols; 2-month rolling-window |
| [[foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class|Borri, Liu, Tsyvinski, Wu (2025)]] | Asset-pricing + carry trajectory | Binance perp; full sample 2020-2025; 10 stylized facts |

This is the wiki's first multi-paper academic-anchor cluster on any single mechanism family. Reduces single-author-lineage risk (per a standing convention companion-source de-risk discipline).

## Sources

- arXiv: https://arxiv.org/abs/2212.06888 (abstract + metadata)
- arXiv HTML v5: https://arxiv.org/html/2212.06888v5 (full text — abstract + theory + introduction; quantitative tables require full PDF)
- Cited prior work in the paper: Du, Tepper, Verdelhan (2018) "Deviations from Covered Interest Parity" — traditional currency-market baseline this paper compares against
- Cited prior work: Liu, Tsyvinski, Wu (2022) 3-factor crypto model — this paper's arbitrage Sharpe is computed vs that model's alphas
- Companion higher-resolution source: [[foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class|Borri, Liu, Tsyvinski, Wu (2025)]] — year-by-year carry-trajectory data; concrete Sharpe numbers post-2024
