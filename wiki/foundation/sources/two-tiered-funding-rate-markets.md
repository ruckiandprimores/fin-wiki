---
title: "Two-Tiered Structure of Cryptocurrency Funding Rate Markets (Mathematics 2026)"
status: growing
created: 2026-05-15
updated: 2026-05-15
tags: [source, academic, crypto-native, funding-rate, cross-venue, cex-dex, price-discovery, granger-causality, market-microstructure, day-swing-applicable]
---

# Two-Tiered Structure of Cryptocurrency Funding Rate Markets

> **TL;DR**: Recent (Jan 2026) high-frequency empirical study of cryptocurrency funding-rate markets across 26 exchanges (11 CEX + 15 DEX) and 749 symbols over 8 days, comprising 35.7M one-minute observations. **Headline finding**: a **two-tiered market structure** — CEX dominates price discovery with **61% higher integration than DEX**; Granger causality tests show **all significant information flow runs CEX→DEX with zero reverse causality**. Funding-rate arbitrage spreads are economically significant in **17% of observations (≥20bp)**, but only **40% of top opportunities generate positive returns after transaction costs + spread reversals**. **Why it matters for the wiki**: provides empirical backing for the cross-venue funding-spread discrimination signal flagged as **capability-blocked** on [[trading/mechanisms/etf-era-carry-trade-funding-distortion|etf-era page §discrimination signals]] — the DEX-vs-CEX funding spread is *measurable* (this paper does it), it's just not yet in the backtesting lab DSL. Sharpens [[trading/mechanisms/funding-cycle-dynamics|funding-cycle-dynamics]] with cross-venue market structure data and provides the first wiki-encoded N=1 academic confirmation that DEX perp markets have a structurally distinct participant mix from CEX (which the wiki had hypothesized but not anchored). **Honest scope**: only 8 days of data is the load-bearing limitation — paper provides snapshot structural evidence, not multi-regime stability.

## Citation

Published in *Mathematics* (MDPI), Vol 14 Issue 2, paper 346, January 2026.

- Journal page: https://www.mdpi.com/2227-7390/14/2/346
- ResearchGate: https://www.researchgate.net/publication/399936354_The_Two-Tiered_Structure_of_Cryptocurrency_Funding_Rate_Markets

## 3-Axis Tags

- **Economic mechanism**: Cross-venue arbitrage; price discovery; basis-trade infrastructure
- **Behavioral/structural driver**: Capacity constraint (only 40% of arbitrage opportunities net-positive after costs — friction binds); information asymmetry (CEX has structurally more institutional flow → faster price discovery)
- **Crypto applicability**: Direct (paper studies crypto specifically; entire dataset is crypto perp + funding rates)

## Key empirical findings

### 1. Dataset characteristics — 35.7M one-minute observations, 26 exchanges

| Dimension | Value |
|---|---:|
| Total observations | **35,700,000** one-minute snapshots |
| CEX exchanges | 11 |
| DEX exchanges | 15 |
| Symbols (across all exchanges) | 749 |
| Time window | 8 consecutive days |
| Granularity | 1-minute |

**Honest-scope caveat**: 8 days is a snapshot, not a multi-regime study. Findings apply at structural-snapshot level; stability across regime shifts (ETF inflow surge vs ebb; bull vs bear; FOMC weeks vs quiet weeks) is not validated by this paper.

### 2. CEX-dominates-price-discovery finding (61% higher integration than DEX)

- **CEX exchanges show 61% higher integration** than DEX in funding-rate cross-correlation analysis
- **Granger causality**: ALL significant information flow runs **CEX → DEX**
- **Zero reverse causality**: no significant DEX → CEX information flow detected

**Mechanism interpretation**: institutional flow concentrates on CEX (where ETF cash-and-carry arbitrage requires CEX perp leg matched to CEX/custodian-held spot); CEX funding rates reflect this flow first; DEX funding rates follow via slower arbitrage paths.

### 3. Cross-venue arbitrage opportunity findings — 17% observation rate, 40% net-positive

| Observation | Magnitude |
|---|---:|
| Fraction of observations with funding-rate arbitrage spread ≥ 20bp | **17%** |
| Fraction of "top" arbitrage opportunities that generate positive returns after costs + spread reversals | **40%** |

**Interpretation**: arbitrage spreads ARE economically significant — 17% of the time, an arbitrageur could see a 20+bp spread between venues. **But** transaction costs + the risk of the spread reversing during the trade make only 40% of the largest opportunities actually profitable. The other 60% face friction-eaten edge OR the spread closes before the trade completes.

**Wiki-relevance for capability-blocked discrimination signal**: per [[trading/mechanisms/etf-era-carry-trade-funding-distortion#discrimination-signals-capability-gap|etf-era §discrimination signals]], "cross-venue funding spread" was listed as one of 4 discrimination signals to disambiguate institutional cash-and-carry funding from speculative positioning funding — and it was flagged as capability-blocked (no multi-venue feed in the backtesting lab DSL). This paper demonstrates **the cross-venue spread is empirically measurable and structurally non-zero** — confirming that the capability is worth investing in, AND that it's not free money (only 40% net-positive after costs).

### 4. Information flow direction — CEX as price-discovery venue

The CEX → DEX one-directional Granger causality finding implies:

- **Strategies using DEX funding signals lag the actual market by some interval** (paper-specific Granger lag values would be in the full text, not yet fetched)
- **The wiki's hypothesis that DEX perp markets have a different participant mix** (per [[trading/mechanisms/etf-era-carry-trade-funding-distortion#open-questions]] §"Is stablecoin-collateralized DEX perp funding less distorted?") gets first empirical confirmation — DEX is structurally lagging, which means whatever distortion exists at CEX-level institutional-cash-and-carry leg, it propagates to DEX with delay and attenuation rather than originating there.

## Implications for wiki priorities

### Sharpening [[trading/mechanisms/etf-era-carry-trade-funding-distortion]]

- The capability-blocked discrimination signal "cross-venue funding spread" has its first academic-grade empirical anchor: 17% observation rate of ≥20bp spreads, 40% net-positive after costs. **Capability investment in multi-venue feed is empirically motivated.**
- Open question §"Is stablecoin-collateralized DEX perp funding less distorted?" partially answered: DEX **lags** CEX (Granger CEX → DEX, zero reverse). If the distortion-thesis is right (institutional cash-and-carry at CEX), DEX funding may be a delayed reflection of CEX distortion rather than a clean signal. **DEX is not a discriminator; it's a lagged-CEX shadow.**

### Sharpening [[trading/mechanisms/funding-cycle-dynamics]]

- Family-page sharpening with cross-venue market-structure section: funding-rate market is fragmented across 26+ venues but **one-directional information flow** (CEX → DEX) suggests structurally efficient hierarchy rather than parallel-equivalent markets.
- Single-venue funding-carry capture: paper's 40%-net-positive-after-costs finding constrains expectations — even on the "obvious" top-spread opportunities, friction eats 60%.

### Sharpening [[foundation/market-structure/]] cluster

- The paper is wiki's first **academic-grade** cross-venue analysis. Slot into asset-classes or market-structure as foundational reference for crypto-market-fragmentation.

### Companion to [[foundation/sources/he-manela-fundamentals-perpetual-futures]]

- Fundamentals paper (He, Manela et al.) is single-venue + pricing-deviation focused
- Two-Tiered paper is cross-venue + market-microstructure focused
- Together they form the wiki's first **academic-anchor cluster** for crypto-perp/funding-rate mechanism class

## Honest caveats

- **8-day window is the load-bearing limitation.** Multi-regime stability not validated. The findings could be regime-specific to the 8-day sample.
- **Top-opportunity selection bias risk**: the 40%-net-positive-after-costs figure depends on how "top opportunities" is defined; paper-specific definition would need full-text review.
- **Transaction cost assumptions**: the 40% figure depends on the paper's cost model; aggressive cost assumptions inflate the friction-eaten fraction; conservative ones deflate it.
- **DEX heterogeneity**: 15 DEX exchanges include a wide range of designs (AMM-based, order-book-based, hybrid). Treating "DEX" as a single bucket may obscure structural differences within DEX category. Wiki should not generalize "DEX lags CEX" to specific DEX implementations without verification.
- **Funding-rate is one dimension**: the paper studies funding rates specifically. Other observables (basis, OI, premium index) may show different cross-venue dynamics.

## What the paper does NOT cover

- Multi-regime stability across ETF-flow cycles
- Decomposition of funding rate into institutional cash-and-carry vs speculative positioning components (the etf-era distortion thesis)
- Stablecoin-collateralized DEX perp specifically (Hyperliquid, GMX, dYdX) vs other DEX types
- Time-evolution of the CEX→DEX causality direction (does it strengthen or weaken over the 8-day window?)

## Related

- [[foundation/sources/he-manela-fundamentals-perpetual-futures]] — sister-source on perp-pricing fundamentals; complementary dimension (single-venue pricing vs cross-venue market structure)
- [[foundation/sources/hayes-essays]] — practitioner narrative on perp design; this paper documents the market structure that grew on top of the 2016 BitMEX design
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; cross-venue structure adds to family understanding
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — discrimination-signal capability-gap empirically backed; DEX-as-discriminator hypothesis nuanced
- [[foundation/market-structure/crypto-carrier-features]] — cross-venue fragmentation as carrier feature (cross-link if/when read)

## Deep-dive findings (2026-05-15 PM PDF read — full extraction)

User-supplied PDF (20 pages, MDPI Mathematics 14(2):346) ingested. Single-author paper: **Petar Zhivkov** (Bulgarian Academy of Sciences + Angel Kanchev University of Ruse + Centre of Excellence in Informatics and ICT, Bulgaria). Published Jan 20, 2026.

### Sample window + universe (now precise)

- **Sample period**: 8-15 November 2025 (8 consecutive days)
- **Granularity**: 1-minute intervals
- **Total observations**: 35,674,300
- **Unique symbols**: 749 (1417 raw → 731 after multi-exchange filter for ≥2 exchanges per symbol)
- **Exchanges**: 26 (11 CEX + 15 DEX)
- **Missing data**: < 0.01%

### Exchange names (full list)

**11 CEX**: BINANCE, BYBIT, OKX, KUCOIN, MEXC, GATEIO, BITGET, BINGX, PHEMEX, CRYPTOCOM, HTX

**15 DEX**: HYPERLIQUID, DRIFT, PARADEX, KUMA, VEST, BLUEFIN, PACIFICA, LIGHTER, EXTENDED, EDGEX, HIBACHI, ETHEREAL, VARIATIONAL, ASTER, WOOFI

### Funding-rate normalization

Different exchanges report funding rates at 1h, 4h, or 8h intervals. Paper normalizes to **8h-equivalent in basis points** via linear scaling. 20bp 8h-equivalent = **219% APY** (the abstract's headline threshold).

### Volume-share context

Perpetual futures account for **~93% of cryptocurrency futures trading volume** + **over $90 trillion cumulative trading volume** [paper's headline framing]. **CEX share** of perp futures volume: ~75-80% during the sample period. **DEX-to-CEX ratio grew from 3.7% to 11.7% over 2025** — DEX expansion is real but CEX still dominant.

### Transaction-cost model (sample-specific)

- **Taker fees**: 0.01-0.05% across exchanges; conservative estimate 0.05% used in simulations
- **Slippage**: 0.1% for liquid pairs (OI rank < 50); **0.5% for illiquid pairs**
- **Gas (DEX)**: < $0.01 per trade (most DEX in sample on Solana / Arbitrum / Optimism)
- **Per-position round-trip cost on $10K position**:
  - Liquid markets: **$60** ($30 entry + $30 exit)
  - Illiquid markets: **$220** ($110 entry + $110 exit)

### Descriptive statistics (CEX vs DEX, Table 2)

| Metric | CEX | DEX | Difference |
|---|---:|---:|---:|
| Mean funding (bps, 8h) | −1.91 | −1.74 | 0.18 |
| Median (bps) | 1.00 | 0.80 | −0.20 |
| Std Dev (bps) | **22.82** | **17.24** | −5.58 |
| Min (bps) | −2400 | −1847 | 553 |
| Max (bps) | 1600 | 224 | −1376 |
| IQR (bps) | 1.20 | 1.60 | 0.40 |

**Central tendencies are economically indistinguishable** (CEX -1.91 vs DEX -1.74 bps; statistically significant only because of huge sample size). **CEX has higher dispersion** (Levene F = 9232, p < 0.001) — wider tails, more extreme funding rates.

**Wiki-relevance**: market frictions manifest in cross-platform **spreads**, not in average levels. The arbitrage opportunity isn't from average-funding-rate-difference; it's from **temporary spread divergences** at specific symbol×timestamps.

### Arbitrage spreads (Table 3 — top 10 by spread)

665,251 observations (**17.1% of sample**) exceed 20bp threshold. Mean spread on these: 51.6 bps; max 2401 bps.

**Top 10 by average spread**:

| Rank | Symbol | Avg spread (bps) | Max spread (bps) | Avg APY | Duration above 20bp |
|---|---|---:|---:|---:|---|
| 1 | INTC | 472.4 | 600.5 | 5,173% | 1h 12m (0.7%) |
| 2 | ELX | 307.3 | 1508.2 | 3,365% | 7h 7m (4.2%) |
| 3 | CPOOL | 283.0 | 2401.0 | 3,099% | 1h 26m (0.8%) |
| 4 | AIA | 260.2 | 1623.8 | 2,850% | 3d 13h 20m (**50.2%**) |
| 5 | LSK | 259.1 | 2400.1 | 2,837% | 2d 16h 34m (**38.0%**) |
| 6 | ASML | 192.1 | 618.5 | 2,103% | 4h 37m (2.7%) |
| 7 | MERL | 173.7 | 1440.2 | 1,903% | 1d 12h 14m (21.3%) |
| 8 | 0G | 166.5 | 1597.4 | 1,823% | 3d 15h 48m (**51.6%**) |
| 9 | LA | 162.2 | 1601.0 | 1,776% | 1d 22h 40m (27.4%) |
| 10 | PARTI | 159.0 | 1200.0 | 1,741% | 1d 17h 54m (24.6%) |

**Pattern**: top opportunities split between **short brief** (INTC/CPOOL/ASML — large but transient) and **persistent** (AIA/LSK/0G — moderately-large but for 38-52% of the sample). The persistent variety is more deployable.

**Spread distribution by pair type**:

| Pair type | Share of significant spreads | Avg spread |
|---|---:|---:|
| CEX-DEX | **84%** | 52.4 bps |
| CEX-CEX | 16% | 47.2 bps |
| DEX-DEX | ~0% | 30.3 bps |

**The cross-type boundary (CEX-DEX) is where the opportunity lives** — within-CEX is highly-integrated; within-DEX is fragmented but limited in absolute spread magnitude. **For practitioners**: cross-platform arbitrage strategies must navigate CEX-DEX bridge, not intra-type.

### Time-series properties (Tables 4, 5)

**Stationarity (ADF/KPSS)**:

| Symbol | ADF stat | KPSS stat | Conclusion |
|---|---:|---:|---|
| BTC | −6.01 | 0.81 | Mixed |
| **ETH** | **−7.20** | **0.30** | **Stationary** |
| SOL | −5.04 | 5.86 | Mixed |
| BNB | −1.86 | 4.88 | Non-stationary |
| XRP | −4.16 | 5.54 | Mixed |
| ZEC | −2.44 | 0.64 | Non-stationary |
| DOGE | −4.16 | 1.12 | Mixed |
| HYPE | −4.52 | 5.44 | Mixed |
| ASTER | −2.57 | 4.36 | Non-stationary |
| SUI | −6.50 | 6.49 | Mixed |

**Only ETH is clearly stationary**. ETH funding-rate spread half-life = **20 minutes** (AR(1) coefficient 0.966, R² = 0.93). Other symbols show AR(1) > 0.96 implying half-lives in **hours to days**.

**Implication**: ETH is the only symbol where short-horizon convergence arbitrage is reliable; other symbols require patient capital tolerating long mean-reversion timescales (or DON'T mean-revert at all in the 8-day window).

**Autocorrelation**: First-order autocorrelation **0.966-0.998** (extreme persistence). Even at lag 8, autocorrelations ≥ 0.80. Ljung-Box Q statistics 39,970-51,736 (overwhelmingly reject no-autocorrelation null).

**GARCH(1,1) for funding-rate-spread volatility**:

| Symbol | α | β | α+β |
|---|---:|---:|---:|
| BTC | 0.777 | 0.223 | **1.000** |
| ETH | 0.926 | 0.074 | **1.000** |
| SOL | 0.202 | 0.798 | **1.000** |

**IGARCH** behavior — volatility shocks are permanent. **Note**: paper itself flags this may be artifact of 8-day window; rolling-window companion ([[temporal-dynamics-funding-rates]]) finds IGARCH appears in only 24.5% of 7-day rolling windows — so this finding is **episodic, not structural**.

### Granger causality results (Table 6 — full breakdown)

**Test setup**: 92 bidirectional tests across 5 exchange pairs × 10 cryptocurrencies. 57 significant (62%). AIC lag selection (max 10 lags); for most pairs AIC selects 1-3 lags.

**CEX → DEX causality**: **14 of 14 significant** (100% directional). Zero reverse causality.

**Significant F-statistics range from 4.23 to 127.83**:

| Symbol | Direction | F-stat | Verdict |
|---|---|---:|---|
| ETH | BINANCE → HYPERLIQUID | 4.23 | Significant |
| ETH | HYPERLIQUID → BINANCE | 58.27 | Significant (reverse!) |
| SOL | BINANCE → HYPERLIQUID | 5.33 | Significant |
| **ZEC** | **BINANCE → HYPERLIQUID** | **127.83** | **Significant (strong)** |
| BNB | BINANCE → HYPERLIQUID | 4.54 | Significant |
| BNB | HYPERLIQUID → BINANCE | 337.60 | Significant (reverse, very strong!) |
| BTC | BINANCE → BYBIT | 5.24 | Significant |
| BTC | BYBIT → BINANCE | 86.54 | Significant (reverse, strong) |

**⚠️ Wiki correction**: The abstract framing "all significant information flow runs CEX-to-DEX with zero reverse causality" is **not strictly accurate at the per-symbol-per-pair level**. The actual finding is more nuanced — some specific symbols (ETH, BNB) DO show DEX→CEX causality with very high F-stats. The general pattern of CEX-leadership holds but isn't absolute. This is sharpened by the [[temporal-dynamics-funding-rates|Temporal Dynamics companion paper]] which finds even more reversal in rolling-window analysis.

**Intra-CEX bidirectional**: **35 significant relationships**. BINANCE↔BYBIT show bidirectional causality on 9 of 10 symbols.

**Intra-DEX limited**: 8 significant relationships, concentrated in DRIFT-HYPERLIQUID. HYPERLIQUID Granger-causes DRIFT on 5 symbols → **HYPERLIQUID is the dominant DEX**.

**Robustness across sampling frequencies**:

| Direction | 1-min | 5-min | 15-min |
|---|---:|---:|---:|
| Total significant | 56/92 (60.9%) | 44/92 (47.8%) | 27/92 (29.3%) |
| CEX→DEX | **14/14** | **7/7** | **6/6** |
| DEX→CEX | **0/14** | **0/7** | **0/6** |
| CEX↔CEX | 35 | 23 | 12 |
| DEX↔DEX | 7 | 4 | 3 |

CEX→DEX **directional asymmetry is robust** across sampling frequencies (this is the structural finding); intra-type causality declines at coarser timescales (more of a microstructure artifact).

### Correlation matrix (Table 8 — 2,194 unique exchange pairs)

| Pair type | N pairs | Mean corr | Median corr | Std |
|---|---:|---:|---:|---:|
| CEX-CEX | 576 | **0.246** | 0.203 | 0.284 |
| CEX-DEX | 1,141 | **0.175** | 0.131 | 0.247 |
| DEX-DEX | 477 | **0.153** | 0.124 | 0.234 |

**61% higher integration** = (0.246 − 0.153) / 0.153 = 0.608.

t-tests:
- CEX-CEX vs DEX-DEX: t = 5.77, p < 0.001
- CEX-CEX vs CEX-DEX: t = 5.39, p < 0.001
- DEX-DEX vs CEX-DEX: t = −1.67, **p = 0.096 (NOT significant)** — within-DEX is statistically indistinguishable from cross-type

**Bitcoin correlation heatmap** (Figure 1) shows CEX-CEX correlations >0.4 forming a "warm-colored block"; DEX-DEX correlations 0.1-0.3 with several near-zero or negative — visually distinct two-tier structure.

### Portfolio simulation results (Table 9 — top 20 arbitrage opportunities)

**Setup**: $10,000 per side; transaction costs per §3.5; forced exit when spread < 0 bps.

| Metric | Value |
|---|---:|
| Number of profitable portfolios | **8 of 20 (40%)** |
| Loss-making | 12 of 20 |
| Average net P&L | **+$22** (close to break-even) |
| Best (AIA) | **+$1,433 over 2 days** (12,953% APY) |
| Worst (PYR) | −$219 (2-min hold; forced exit immediately) |
| **Average Sharpe** | **−7.40** |
| Best Sharpe (AIA) | −2.73 (yes, even the WINNER has negative Sharpe) |
| Average Sortino | −1.93 |
| Average max drawdown | −$117 (−1.17% of position size) |
| Reversal-pattern portfolios | **19 of 20 (95%)** |
| Forced exits | 12 of 20 (60%) |

**Why even winners have negative Sharpe**: portfolios held through high-volatility periods; final P&L positive but path-quality (volatility) is bad. **Sharpe ratio is severely punishing for this strategy class** despite some positive expected value at the spread-level.

**Spread reversal economics** — minimum-viable-spread math:

A 20bp spread on $10K position accrues funding at $0.042 per minute (= $10K × 20bp / 10,000 / 480 min). Needs **5,263 minutes (3.7 days)** to accumulate enough funding to cover the $220 illiquid round-trip cost. **Most opportunities don't last 3.7 days before reversing** → most are unprofitable.

**Profitable winners** (8 of 20): AIA, PARTI, KAVA, MERL, ASML, 0G, GODS, LAYER — all had either very high spreads (AIA 732 bps) OR very long durations.

**Wiki-relevance for funding-carry deployment + cross-venue strategies**:

- A live funding-carry deployment's selective-entry approach vs Zhivkov's portfolio Sharpe −7.40 demonstrates **selective-entry alpha is massive** — taking every above-threshold opportunity systematically loses; only careful selection of high-spread-AND-long-duration opportunities wins
- Cross-venue arbitrage is **operationally difficult**: 95% reversal-pattern rate; 60% forced-exit rate; transaction-cost-eaten edge on most "obvious" spreads
- A funding-carry deployment likely uses **single-venue funding-carry** rather than cross-venue spread capture — different mechanism from Zhivkov's strategy. The "cross-venue funding spread" discrimination signal flagged on etf-era page is empirically harder to capture than the simpler within-venue selective-entry approach

## Companion source

The [[foundation/sources/temporal-dynamics-funding-rates]] page (NEW, 2026-05-15 PM) extends this paper with rolling-window analysis: confirms two-tier structure persists qualitatively but with substantial temporal variation (integration gap −0.041 to 0.222); **walks back the IGARCH finding** (episodic, only 24.5% of windows); **challenges the size-based price-discovery hierarchy** (Bybit Granger-causes Binance more frequently than reverse). Read together, the two papers form a CEX-DEX cross-venue research cluster.

## Sources

- Journal: https://www.mdpi.com/2227-7390/14/2/346 (Mathematics 14(2):346, MDPI Jan 2026)
- User-supplied PDF (20 pages) ingested 2026-05-15 PM
- Author: Petar Zhivkov (Bulgarian Academy of Sciences + Angel Kanchev University of Ruse + Centre of Excellence in Informatics and ICT, Bulgaria)
- Companion paper: [[foundation/sources/temporal-dynamics-funding-rates|Temporal Dynamics]] — same lead author + Todorov + Georgiev; rolling-window extension
