---
title: "Borri, Liu, Tsyvinski, Wu — Cryptocurrency as an Investable Asset Class: Coming of Age (2025)"
status: growing
created: 2026-05-15
updated: 2026-05-15
tags: [source, academic, crypto-native, perpetual-futures, funding-rate, carry-strategy, factor-pricing, jumps, regime-conditioning, day-swing-applicable, funding-carry-relevant]
---

# Borri, Liu, Tsyvinski, Wu — Cryptocurrency as an Investable Asset Class: Coming of Age

> **TL;DR**: Major academic asset-pricing paper on crypto, Oct 2025. Authors include **Aleh Tsyvinski (Yale)** — one of the most-cited finance academics actively publishing on crypto. **arXiv:2510.14435v2**. Organizes findings into **10 stylized empirical regularities**. **Headline finding for the wiki: cryptocurrency carry strategy Sharpe ratio was 6.45 (full-sample Aug 2020 – May 2025), collapsed to 4.06 from 2024 onwards, turned NEGATIVE in 2025** — strongest academic-grade evidence yet that the ETF-era distortion is undermining the funding-rate-carry mechanism (per [[trading/mechanisms/etf-era-carry-trade-funding-distortion|etf-era distortion thesis]]). Carry strategy uses Binance perp 8h funding; carry was driven by ~8% annualized mean funding rate with low 0.8% vol — a coherent persistent edge until 2024. The Sharpe-6.45-to-negative trajectory **walks back the academic-grade expectation that funding-carry is a reliable persistent mechanism** in the same way the wiki walked back v6 BTC prod (different mechanism class; same epistemic walk-back). **Other headline findings**: (i) crypto-equity correlation jumped from 2% pre-2020 to 37% post-2020 (regime-break); (ii) jump probability BTC 40% pre-2020 → 18.8% post-2020 (still >5× equity baseline ~5.3%); (iii) optimal portfolio allocation 3.1% retail / 5.5% institutional; (iv) cross-exchange price discount half-life **1.1 weeks** (concrete arbitrage decay rate); (v) blockchain data explains ~8% of return variation; (vi) higher-order factor terms (cubic momentum, squared market) explain ~25% additional cross-sectional variance beyond linear factors.

## Citation

**Borri, N., Liu, Y., Tsyvinski, A., Wu, X.** (2025). *Cryptocurrency as an Investable Asset Class: Coming of Age*. arXiv:2510.14435 [q-fin.GN]. Oct 16, 2025 (v2 from Mar 21, 2026; v4 stamp per arxiv).

- arXiv: https://arxiv.org/abs/2510.14435
- arXiv HTML v2: https://arxiv.org/html/2510.14435v2

Authors are top-tier finance academics:
- **Aleh Tsyvinski** — Yale; one of the most-cited finance researchers on crypto; co-author of the seminal 3-factor crypto model (Liu, Tsyvinski, Wu 2022)
- **Yukun Liu** — University of Rochester; co-author on crypto factor research
- **Nicola Borri** — LUISS University, Rome; crypto FX/carry research
- **Xi Wu** — UC Berkeley

## 3-Axis Tags

- **Economic mechanism**: Carry / funding-rate (primary focus); factor asset pricing (size, momentum, value); volatility / jump dynamics
- **Behavioral/structural driver**: Regime transition (pre-2020 vs post-2020 crypto-equity correlation jump; pre-2024 vs post-2024 carry decay); institutional adoption flow; capacity constraint (carry-edge erosion)
- **Crypto applicability**: Direct (paper is entirely about crypto markets)

## Key empirical findings

### 1. Cryptocurrency carry strategy — Sharpe trajectory 6.45 → 4.06 → negative {#carry-trajectory}

**Strategy**: long perpetual futures with positive funding rates / short perpetual futures with negative funding rates, on **Binance perp 8h funding rate** observations, weighted by funding-rate magnitude. **Sample**: Aug 1, 2020 — May 31, 2025.

| Period | Carry strategy Sharpe |
|---|---:|
| Full sample (Aug 2020 – May 2025) | **6.45** |
| 2024 onwards | **4.06** |
| 2025 (year-to-date) | **Negative** |

**Mean funding rate full-sample**: ~8% annualized
**Volatility of carry returns**: 0.8% (low vol producing the high Sharpe)

**Cumulative performance trajectory**: $1 invested grows substantially through 2023, then sharply deteriorates post-2024.

**Wiki-relevance — direct funding-carry deployment framing**:

This finding is **the single most important external empirical anchor for the funding-carry deployment framing on the wiki**. The full-sample Sharpe 6.45 establishes that **the academic carry-strategy benchmark was historically far above** a live funding-carry deployment's risk-adjusted return (per [[foundation/methodology/deployment-edge-threshold#opportunity-cost-gate|deployment-edge-threshold §5A]]). The Sharpe gap implies a deployment's selective-entry logic was *capturing a fraction* of the broader funding-carry opportunity available pre-2024.

**Crucially, the Sharpe trajectory matters more than the level**:

- Full-sample 6.45 already includes the 2024-2025 deterioration in the average
- 2024-only Sharpe **4.06** is the post-ETF-era best estimate of the broad-carry-strategy Sharpe — still above a typical live deployment's realized return
- 2025 **negative** is a serious warning: the broad carry strategy is currently a losing trade. A live deployment's selective-entry may still capture residual edge but the persistent-edge framing is now empirically suspect

**Cross-link to wiki's existing etf-era distortion thesis**: [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] hypothesized that institutional cash-and-carry is mixing into the funding signal and breaking the classical fade-the-extreme interpretation. The Borri et al. (2025) carry-strategy 2025-negative result is **direct empirical confirmation** that broad funding-carry is now broken in the ETF era. The wiki's hypothesis is no longer hypothesis — it has academic-grade empirical confirmation.

**Implication for funding-carry deployment expectations**:

| Sharpe | Source | Period | Direction |
|---|---:|---|---|
| 6.45 | Borri et al. broad academic carry | 2020-2025 | Historical baseline; not deployment-grade signal |
| 4.06 | Borri et al. broad academic carry | 2024 onwards | Post-ETF baseline still profitable |
| Negative | Borri et al. broad academic carry | 2025 YTD | **Edge-collapsed regime** |

If broad academic carry is currently Sharpe-negative and a live deployment is still positive, the deployment's selective-entry logic must be the source of the gap. **Watch for deployment Sharpe deterioration if the broad-carry-negative regime persists** — the selective-entry alpha may decay as the underlying carry signal continues to invert.

**Critical non-transfer finding (2026-05-18)** — the broad-carry collapse does NOT automatically transfer to adjacent premium-harvesting mechanism families. Per [[../../trading/mechanisms/insurance-premium-harvest-overview#regime-decay-non-transfer]]:

- **Variance Risk Premium (VRP) harvest on options** persists at ~14% BTC annualized (Almeida 2024); 2025 DVOL low of 37% places market in LV cluster where Almeida shows VRP is LARGEST (0.17 vs 0.12 in HV). Different counterparty stack (options buyers vs leveraged longs) — has NOT been arbed away.
- **Delta-neutral funding harvest on perps** persists at compressed magnitude (sUSDe 18% → 3.72% Q1 2026) — same trajectory as broad-carry compression but mechanism is alive because the delta-neutral construction harvests funding even when directional carry inverts.

**Generalizable wiki-level discipline** (N=1; watch for N=2 confirmation): regime-decay findings on one premium-harvesting mechanism family do not automatically transfer to adjacent families. Counterparty-stack analysis is the discriminator. See [[../../trading/mechanisms/variance-risk-premium-crypto#vrp-vs-broad-carry|variance-risk-premium-crypto §vrp-vs-broad-carry]] and [[../../trading/mechanisms/delta-neutral-funding-harvest|delta-neutral-funding-harvest]] for the full structural explanation.

### 2. Regime-break in crypto-equity correlation (2020) {#crypto-equity-correlation}

| Period | Crypto-equity correlation |
|---|---:|
| Pre-2020 | **2%** |
| Post-2020 | **37%** |

**Mechanism**: institutional adoption + macro-overlay (per Hayes Theme 1 dollar-liquidity transmission to BTC — see [[foundation/sources/hayes-essays]]).

**Wiki-relevance**:
- Sharpens [[foundation/macro/btc-regime-taxonomy-2020-2025]] — the 2020 break-point is the load-bearing dimension transition; the academic-grade empirical anchor confirms what the wiki has framed as macro-regime transition since the post-COVID era
- Diversification framing: pre-2020 crypto was a true diversifier (correlation 2%); post-2020 it's a **higher-return macro-correlated asset** (correlation 37%). Different role in portfolio construction
- The 37% correlation is high enough that crypto should be treated as a **macro-correlated risk asset** in portfolio construction, not as an uncorrelated alternative

### 3. Jump dynamics — crypto 5× more jumpy than equities {#jump-dynamics}

| Asset | Full sample | Pre-2020 | Post-2020 |
|---|---:|---:|---:|
| Bitcoin | 28.0% | 40.0% | 18.8% |
| Ethereum | 26.0% | 39.7% | 15.5% |
| US stocks (DJ30 avg) | 5.3% | 5.2% | 5.6% |

*Jump probability = fraction of days with statistically significant jumps at 0.1% confidence.*

**Median jump contribution to realized variance**: 26-47% for crypto; 38% for equities. Crypto jumps are MORE FREQUENT but NOT LARGER in conditional magnitude.

**Wiki-relevance**:
- Sharpens [[trading/mechanisms/liquidation-cascade-aftermath]] — the cascade-event-detector pre-registered threshold (price ≥1.5×ATR + vol ≥3×trailing) is designed for crypto's higher base jump frequency; 5× more jumpy than equities means generic equity-derived cascade detectors will under-fire on crypto and need crypto-specific calibration
- Sharpens [[foundation/methodology/path-quality-diagnostics]] §2.1 event-concentration framework — high base jump frequency means event-driven strategies in crypto have a **structural** event-concentration profile, not an artifact-of-window-selection. The lottery-shape pattern is partly inherent to the crypto regime
- Crypto jump frequency **declined post-2020** (40% → 18.8% for BTC) — markets becoming more mature; aligns with the He-Manela 11%/yr deviation-decay finding. But absolute frequency still 3-4× equity baseline

### 4. Cross-exchange price discount half-life: 1.1 weeks {#cross-exchange-decay}

Cross-exchange price discounts average **3.7% to −1.8%** with half-life of **1.1 weeks**.

**Wiki-relevance**:
- Companion to [[foundation/sources/two-tiered-funding-rate-markets]] — that paper found 17% of observations have ≥20bp cross-venue spreads (8-day snapshot); this paper finds the half-life is 1.1 weeks (longer time-series perspective)
- **Cross-venue arbitrage is structurally slow** — 1.1-week half-life means arbitrageurs need patient capital to capture the spread; this is consistent with the Two-Tiered finding that only 40% of top opportunities net-positive after costs (the time-to-convergence is a cost factor)
- Sharpens the etf-era page §discrimination-signals — cross-venue funding spread is empirically anchored AND empirically slow; capability investment is worth it for the persistent arbitrage opportunity, but expectations for fast capture are calibrated

### 5. Portfolio allocation — 3.1% retail / 5.5% institutional {#portfolio-allocation}

Optimal portfolio allocation to crypto:
- Retail investors: **3.1%**
- Institutional investors: **5.5%**

Context: CalPERS holds comparable magnitudes in emerging-market debt (5.3%) and high-yield bonds (5.3%).

**Wiki-relevance**:
- Sharpens [[investment/allocation/]] cluster — Tier-1 allocation reference for crypto-as-asset-class in investment portfolios
- The 5.5% institutional allocation reframes the AMC (per this research program) framing: an AMC structure formalizing crypto-investing operations should be designed around the academic-grade 3-5.5% allocation benchmark; client portfolios aimed at this benchmark are "investable-crypto-as-asset-class" exposure, not "all-in crypto" exposure
- Cross-link to [[investment/allocation/conviction-tiered-sizing]] — institutional 5.5% baseline is roughly the high end of Tier-2 sizing; an AMC managing client crypto exposure as Tier-2 sizing aligns with academic-grade portfolio allocation guidance

### 6. Factor-pricing decomposition — 47-72% R² depending on model

- C-4 model (baseline 4-factor): **47.3%** R² of cross-sectional variation
- C-4 + Kolmogorov-Arnold single factor: **72.3%** R²
- C-4 + forward-selection higher-order terms (cubic momentum, squared market, interaction terms): **70.2%** R²

Higher-order terms (non-linear) add ~25% explanatory power beyond linear factors.

**Wiki-relevance**:
- Sharpens [[trading/mechanisms/scaffold-as-mechanism]] — cross-sectional return predictability via momentum + size + value + non-linear interactions confirms the mechanism-class framework; ~70% R² is high but not perfect — there's room for additional alpha
- Provides context for the lab's strategy-generation framework: the C-4 model captures most cross-sectional variation; novel mechanisms need to provide edge orthogonal to C-4 to add value

### 7. Blockchain-data explains ~8% of returns {#on-chain-explanatory-power}

On-chain user adoption metrics explain ~8% of return variation.

**Wiki-relevance**:
- Sharpens [[trading/mechanisms/on-chain-mechanisms]] — currently seedling, capability-blocked at strategy-test level. The 8% explanatory power is small but non-trivial; combined with on-chain liquidation cluster data (per liquidation-cascade-aftermath capability roadmap #3), the on-chain layer could provide meaningful mechanism diversification

### 8-10. Other stylized facts (cite-only summary)

- Crypto returns/volatility 10× higher than equities but Sharpe ratios comparable (~0.12-0.14) — risk-adjusted, crypto isn't dominant
- Smart beta factors work (size, momentum, value) — Liu-Tsyvinski-Wu 3-factor model is the baseline
- ASU 2023-08 accounting standard improves markets (regulation-improves-efficiency anchor)

## The 10 stylized facts in compressed form (for reference)

| # | Fact | Magnitude |
|---|---|---|
| 1 | High return, high volatility, normal Sharpe | 10× higher than equities in both; Sharpe ~0.12-0.14 |
| 2 | Crypto-equity correlation regime-break 2020 | 2% pre-2020 → 37% post-2020 |
| 3 | Diversification benefits | Optimal allocation 3.1% retail / 5.5% institutional |
| 4 | Smart beta factors work | C-4 model R² = 47.3% (linear); +25% from non-linear |
| 5 | Jumps frequent | BTC 28% (full), 40% pre-2020, 18.8% post-2020 vs equity 5.3% |
| 6 | Higher-order terms matter | ~25% additional R² from non-linear factor interactions |
| 7 | Blockchain data drives prices | ~8% of return variation from on-chain adoption |
| 8 | Cross-exchange inefficiencies persist | 3.7% to −1.8% average discount; 1.1-week half-life |
| 9 | Carry compressed | Sharpe 6.45 (full) → 4.06 (2024+) → negative (2025) |
| 10 | Regulation improves markets | ASU 2023-08 disclosure standards strengthen efficiency |

## Honest caveats

- **Window: Aug 2020 – May 2025.** Recent enough to capture ETF era but pre-Oct-2025-cascade. May 2025 was before the September + October cascades that ate ~$36B in liquidations combined
- **Sample is Binance perp.** Bitcoin-only for carry strategy; cross-asset extension (ETH, SOL) not provided in this paper
- **Single funding-rate frequency**: 8h Binance perp. DEX perp funding (Hyperliquid, dYdX) and other CEX perp funding NOT analyzed in this paper for the carry strategy
- **Carry decay direction is empirical, not predictive.** Paper documents Sharpe 6.45 → negative trajectory; doesn't model what conditions would restore profitability. Per [[trading/mechanisms/etf-era-carry-trade-funding-distortion#meta-funding-revival-2026-05-13|etf-era §meta_funding revival]], the wiki has independent N=1 evidence that early-2026 saw partial revival via val-period scrutiny — Borri et al.'s 2025-negative finding does NOT preclude regime-conditional revival
- **Academic Sharpe is gross-of-deployment-friction.** Direct comparison to a live deployment's realized return needs adjustment per [[foundation/methodology/deployment-edge-threshold#opportunity-cost-gate|deployment-edge §5A]] live-vs-backtest caveats. Live carry on $100K+ scale faces venue limits, funding-payment timing slippage, execution drag — the broad-academic-carry Sharpe is achievable only at small scale or with market-maker access

## What the paper does NOT cover

- DEX perp carry strategy
- Cross-asset carry (BTC vs ETH vs altcoin)
- Q4 2025 cascade events (sample ends May 2025)
- Practical selective-entry carry algorithms (paper studies broad-carry without entry filters)
- ETF flow data as carry-driver (the paper notes 2024 break-point but doesn't decompose institutional vs retail components — that's [[trading/mechanisms/etf-era-carry-trade-funding-distortion|etf-era page]]'s territory)

## Implications for wiki priorities

- **Funding-cycle-dynamics**: sharpening — Sharpe 6.45 → 4.06 → negative trajectory is the strongest external empirical anchor for the family's deployment-grade-decay narrative; replaces He-Manela's 11%/yr abstract decay with concrete year-by-year numbers
- **Deployment-edge-threshold §5A**: funding-carry opportunity-cost gate framing strengthened — academic broad-carry baseline establishes expectations for a deployment's Sharpe trajectory; if broad-carry stays negative, the deployment's selective-entry edge is the variable in question
- **etf-era page**: distortion-thesis-confirmed empirically by the Sharpe-trajectory data; the wiki's hypothesis is no longer hypothesis
- **btc-regime-taxonomy**: 2020 break-point in crypto-equity correlation is empirical anchor for the regime-taxonomy's macro-overlay dimension
- **liquidation-cascade-aftermath**: 5× higher base jump frequency in crypto vs equities sharpens cascade-detector calibration discipline
- **investment/allocation/**: 5.5% institutional allocation reference is Tier-1 anchor for AMC client-portfolio sizing
- **on-chain-mechanisms**: 8% explanatory power from on-chain data motivates capability investment in Glassnode/CoinMetrics ingest

## Related

- [[foundation/sources/he-manela-fundamentals-perpetual-futures]] — sister-source on perp pricing fundamentals; this paper's carry-trajectory finding is the year-by-year empirical anchor for He-Manela's abstract 11%/yr decay claim
- [[foundation/sources/two-tiered-funding-rate-markets]] — cross-venue funding-rate market structure; this paper's 1.1-week half-life on cross-exchange price discounts is the longer-horizon companion
- [[foundation/sources/hayes-essays]] — practitioner narrative on dollar-liquidity → BTC transmission; this paper's 2020 crypto-equity correlation break is the empirical anchor
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; Sharpe-trajectory finding lands here
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — distortion thesis empirically confirmed by 2025-negative carry trajectory
- [[foundation/methodology/deployment-edge-threshold]] §5A opportunity-cost gate — funding-carry deployment Sharpe contextualized against academic carry trajectory
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — 2020 break-point empirical anchor
- [[trading/mechanisms/liquidation-cascade-aftermath]] — crypto-jump-frequency 5× equity baseline
- [[investment/allocation/conviction-tiered-sizing]] — 5.5% institutional allocation as Tier-2 reference
- [[trading/mechanisms/on-chain-mechanisms]] — 8% on-chain explanatory power motivates capability investment

## Sources

- arXiv: https://arxiv.org/abs/2510.14435 (abstract + metadata)
- arXiv HTML v2: https://arxiv.org/html/2510.14435v2 (full text, fetched 2026-05-15)
- Cited prior work: Liu, Tsyvinski, Wu (2022) — the 3-factor crypto model that this paper extends
