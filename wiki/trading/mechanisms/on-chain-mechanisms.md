---
title: "On-Chain Mechanisms — Cohort Analysis, Cost Basis, Exchange Flow"
status: seedling
created: 2026-05-09
updated: 2026-05-09
tags: [trading, mechanisms, on-chain, glassnode, cryptoquant, cohort-analysis, cost-basis, exchange-flow, capability-gap, research-substrate]
---

# On-Chain Mechanisms — Cohort Analysis, Cost Basis, Exchange Flow

> **TL;DR**: On-chain data is **the most distinctive crypto data layer** versus traditional finance — visible cohort behavior, transparent supply dynamics, identifiable smart-money flow. Six mechanism families documented: (1) **cohort-based cost basis** (LTH/STH/realized price as binary regime levels); (2) **MVRV ratio** (cycle-stage classifier; >3.5 late-bull, <1.0 capitulation); (3) **NUPL** (aggregate unrealized P&L); (4) **exchange flow** (intent-to-sell vs intent-to-hold); (5) **whale-wallet activity** (top-10 share of inflows); (6) **miner outflow** (forced-supply on miner stress). **Status: seedling** — all six mechanisms wiki-encoded as research direction; **none currently backtestable** until Glassnode / CryptoQuant ingest lands per this research program capability roadmap rank #2. Companion to [[trading/mechanisms/funding-cycle-dynamics]] (perp-side mechanisms) and [[trading/mechanisms/liquidation-cascade-aftermath]] (post-cascade mechanisms); orthogonal data layer to both.

## Why This Page Exists

Per the 2026-05-08 this research program capability-blocked reframe: 25+ mechanisms are blocked at backtest level by missing data feeds + DSL primitives. **Glassnode / on-chain is rank #2** in the capability-investment roadmap (6+ mechanisms unlocked: exchange flow, stablecoin mint/burn, whale, miner, LTH/STH cohorts). Per a standing convention memory: substrate research can run AHEAD of capability availability and SHARPENS the capability-investment decision.

**This page is research substrate** — wiki-encoded mechanism family that becomes testable when Glassnode primitives land. It mirrors the pattern of [[trading/mechanisms/vol-regime-detection]] (Sinclair-substrate, capability-blocked on Deribit/DVOL).

## Why On-Chain Data Is Distinctive

Per [[foundation/market-structure/crypto-carrier-features|crypto-carrier-features]] catalog, on-chain data is in the "on-chain flow" carrier category — distinct from derivatives microstructure, cycle/structural, cost-basis & positioning carriers. **What makes it distinctive vs traditional finance**:

| TradFi equivalent | Why it's hard in equities | Why it's available in crypto |
|---|---|---|
| Insider transactions | Form 4 lag (2 days), reported only by insiders defined by SEC | Blockchain transparency makes ALL holder behavior visible |
| Cost basis tracking | Aggregate-level only (no per-investor data) | Realized Price = average cost basis of all coins (UTXO-anchored) |
| Cohort segmentation | Survey-based (slow, sample-bias) | UTXO age = cohort definition (LTH >155 days) |
| Exchange flow | Not observable in equities | Wallet labels classify inflow/outflow by venue |
| Smart-money flow tracking | Illegal in equities (insider trading) | Legal and observable in crypto |

**Implication per [[foundation/market-wisdom/tape-reading|Lefèvre tape-reading]]**: on-chain data is the **digitally-native tape** that Livermore had to read from order flow on physical tickers. The same diagnostic patterns (genuine accumulation vs distribution; smart-money fingerprints; cohort behavior signatures) are *more* observable in crypto than they ever were in pre-1980s equities — confirming the Part I §1 worldview prior that crypto ≈ pre-regulation US equities, with the additional twist that **the transparency advantage favors disciplined readers**.

## Six Mechanism Families

### Family 1: Cohort-Based Cost Basis

**Mechanism**: each on-chain coin is anchored to its last-transaction price → aggregated cost-basis lines for cohorts function as **regime-binary levels** (above = bull bias; reclaim = regime change; below = bear bias).

**Key metrics**:
- **Realized Price** — average cost basis of ALL bitcoin in supply, valued at last-on-chain-transaction price. Cycle-floor anchor in bear markets (multiple historical cycles bottomed near or below).
- **STH Realized Price (STH cost basis)** — average cost basis of Short-Term Holders (UTXOs <155 days). **Binary bull-bear trigger**: price reclaiming STH cost basis tends to flip regime; price losing STH cost basis tends to confirm bear regime.
- **LTH Realized Price (LTH cost basis)** — average cost basis of Long-Term Holders (>155 days). Slower-moving anchor; rare for price to lose LTH cost basis (only in deep bears).

**Operational use**: as a **gating attribute** for any directional strategy (per [[trading/mechanisms/regime-gate-vs-bias-filter|bias-filter discipline]]) — go long only above STH cost basis; go short only below.

**Live example (May 2026 per [[foundation/macro/btc-regime-taxonomy-2020-2025|regime markers]])**: STH cost basis at $80,700 functions as bull-trigger reclaim threshold; True Market Mean at $79,000 as regime-uncertainty zone; BTC currently slightly below = mixed-regime signature.

**Capability gap**: Glassnode metrics not in pipeline; manual lookup only.

### Family 2: MVRV Ratio (Cycle-Stage Classifier)

**Mechanism**: Market Value (current market cap) / Realized Value (sum of UTXO values at last-transaction price) = aggregate unrealized profit ratio. Empirical cycle-stage indicator.

**Key thresholds (Glassnode-derived)**:
- **MVRV > 3.5**: late-bull / euphoria / heavy distribution probability
- **MVRV 1.5-3.5**: mid-bull / normal expansion
- **MVRV 1.0-1.5**: early-bull or topping
- **MVRV < 1.0**: capitulation / late-bear accumulation (most cycles bottom here)

**Operational use**: regime classifier for capital-allocation decisions (bigger positions when MVRV<1.0; smaller when MVRV>3.5) — **timing signal is coarse** (months-scale, not days).

**MVRV Z-Score** variant: MVRV minus moving average / standard deviation — normalized for cycle-comparability across epochs. Better for cross-cycle calibration.

**Capability gap**: same as Family 1.

### Family 3: NUPL (Net Unrealized Profit/Loss)

**Mechanism**: aggregate (Market Value − Realized Value) / Market Value. Functionally similar to MVRV but expressed as a percentage of supply in profit vs loss.

**Operational stages** (named by Glassnode):
- **NUPL > 0.75**: Euphoria / Greed (cycle peak proximity)
- **NUPL 0.5-0.75**: Belief / Denial
- **NUPL 0.25-0.5**: Optimism / Anxiety
- **NUPL 0-0.25**: Hope / Fear
- **NUPL < 0**: Capitulation (entire supply at unrealized loss)

**Operational use**: similar to MVRV; redundant signal at family level. Use one or the other, not both.

### Family 4: Exchange Flow (Intent Signal)

**Mechanism**: BTC moving INTO exchange wallets signals intent-to-sell (or trade); BTC moving OUT signals intent-to-hold (or transfer to cold storage).

**Key metrics**:
- **Exchange inflow / outflow** (daily; net flow = inflow − outflow)
- **Exchange Whale Ratio** = top-10 inflows / total inflows. **>0.5**: whale-dominated inflow (potential institutional sell pressure); **<0.3**: retail-dominated.
- **Exchange Reserve** (total BTC held on exchanges) — secular decline since 2020 = institutional cold-storage shift

**Recent example (2026)**: CryptoQuant flagged exchange whale ratio at 0.64 in early 2026 — highest since October 2015. This is a regime-relevant signal (whale-dominated inflow precedes distribution).

**Stablecoin flow analog**: USDT inflows to exchanges signal **dry powder** for buying. Recent collapse from $616M/day (Nov 2025) to $27M/day (early 2026) is a meaningful regime marker — stablecoin buy-pressure has compressed.

**Operational use**: short-term directional bias (1-7 day horizon) + regime classifier (multi-week trend in flows).

**Capability gap**: CryptoQuant API + wallet-labeling data required; not in current pipeline.

### Family 5: Whale-Wallet Activity

**Mechanism**: large holders (>1,000 BTC, >10,000 BTC) drive disproportionate price impact when they accumulate or distribute. Visible on-chain via clustering analysis.

**Key metrics**:
- **Whale wallet count by tier** (1K-10K BTC; 10K+ BTC)
- **Aggregate whale balance trend** (accumulating = bullish; distributing = bearish)
- **Whale exchange-deposit activity** (from CryptoQuant — deposits = potential sell pressure)
- **HODL waves** (Glassnode visualization of supply by holding period)

**Operational use**: **smart-money confirmation signal** — when whale accumulation aligns with cohort-cost-basis bullish bias, signals stack. Conversely, whale distribution at MVRV>3.5 is a cycle-top-signature.

**Honest caveat**: whale wallets can include exchange custodial wallets (BlackRock IBIT now ~812K BTC ≈ 3.8% of supply) — proper labeling distinguishes "ETF custodial" (long-only structural) from "trading whale" (cyclical). Capability requires high-quality wallet labeling.

### Family 6: Miner Outflow (Forced-Supply Signal)

**Mechanism**: miners must sell BTC to cover operating costs (electricity, hardware depreciation). Their outflow rate reveals **stress level** in the mining cohort. Stressed miners = forced supply at any price.

**Key metrics**:
- **Miner outflow** (BTC leaving miner pool wallets)
- **Hash Ribbon Compression** (mining hashrate slowing — proxy for miners turning off rigs)
- **Puell Multiple** = daily issuance value / 365-day moving average. Low values = miner stress (forced selling); high values = miner profitability.

**Cycle context**:
- Post-halving miner stress is structurally elevated (issuance halved, costs unchanged)
- The 2024 halving showed **muted miner-stress signature** vs prior epochs (per [[foundation/macro/btc-regime-taxonomy-2020-2025|2024 halving update]]) — institutional-bid + ETF flow absorbed forced supply

**Operational use**: contrarian-buying signal at extreme miner-stress lows (Hash Ribbon compression historically accompanies cycle bottoms).

**Capability gap**: miner-pool wallet labeling + hashrate tracking required.

## Cross-Family Signal Stacking

The wiki principle from [[foundation/relations-and-themes|relations-and-themes]] applied to on-chain: **single signals are noisy; stacked signals are operational**. Examples:

| Stack | Signal | Interpretation |
|---|---|---|
| MVRV<1.0 + Hash Ribbon compression + STH RP loss | Capitulation | Cycle-bottom proximity; high-conviction long entry zone |
| MVRV>3.5 + whale exchange deposits rising + STH cost-basis flip | Distribution | Cycle-top proximity; reduce sizing |
| STH RP reclaim + exchange outflow accelerating + LTH balance rising | Bullish reversal | Regime change confirmation |
| Stablecoin exchange inflow surge + Funding compressed | Loaded buy-pressure | Bullish setup awaiting catalyst |

**These are not backtested empirical patterns — they're signal-stacking hypotheses to test once Glassnode primitives land.**

## DSL Primitive Requirements (capability gap)

Per the [[trading/mechanisms/liquidation-cascade-aftermath|liquidation-cascade-aftermath]] precedent for Tier-1/2/3 DSL extension consolidation, on-chain mechanisms require:

**Tier 1 — Glassnode core (5-6 mechanisms)**:
- `realized_price(window, cohort)` — STH/LTH/all-supply realized price
- `mvrv(window)` — MVRV ratio + Z-Score variant
- `nupl()` — Net Unrealized P&L
- `exchange_flow(direction, window)` — inflow/outflow/netflow
- `exchange_balance()` — exchange reserve total
- `cohort_supply(age_band)` — supply by holding-period band

**Tier 2 — CryptoQuant whale + miner (3-4 mechanisms)**:
- `whale_ratio(top_n)` — top-N inflow concentration
- `whale_balance(tier)` — wallet-tier balance trend
- `miner_outflow()` — miner pool outflow rate
- `puell_multiple()` — issuance-value normalization
- `hash_ribbon()` — hashrate compression signal

**Tier 3 — labeling + classification**:
- ETF custodial vs trading-whale distinction
- Stablecoin treasury wallets vs trading wallets
- Mining-pool aggregation vs individual-miner clustering

**Estimated unlock count**: 6-9 mechanisms when Tier 1 lands; +3-5 with Tier 2; cleaner signal-stacking with Tier 3. Aligns with this research program capability roadmap rank #2 ("6+ mechanisms").

## Honest Caveats / Failure Modes

- **All wiki-encoded; none backtested** in the current backtesting lab. Status `seedling` until DSL primitives land + first mechanism gets backtested. **Don't treat any of these as deployment-ready** — they're research-grade hypotheses.
- **Cohort definition arbitrariness**: 155-day LTH/STH split is Glassnode's definition; alternative thresholds (50, 100, 200 days) yield different cohorts. The 155-day choice has historical empirical support but is not ground truth.
- **Wallet labeling error**: ETF custodial wallets, treasury-vehicle wallets (MSTR), exchange cold storage all show up in "whale" data without proper labeling. Misinterpretation risk is real.
- **MVRV cross-cycle stationarity is uncertain**: thresholds (>3.5 = late bull) are calibrated on 2013-2022 cycles. The 2024-? cycle (per [[foundation/macro/btc-regime-taxonomy-2020-2025|halving diminishing returns]]) may have different MVRV peak — institutional flow + ETF access changes the historical calibration.
- **Exchange flow signal is noisy short-term**. Single-day spikes are usually meaningless; 7-day moving averages or weekly aggregates are operationally useful.
- **Stablecoin flow can be misleading**. USDT issuance is partly USDT-on-Tron (retail Asian flow) vs USDT-on-Ethereum (DeFi/trader flow) — aggregate signals miss the cohort split.
- **Self-fulfilling effects**: as more participants watch on-chain metrics, signals can become self-fulfilling (everyone buys at MVRV<1.0). This compresses edge over time. **Crypto-carrier-features burden-of-proof**: edge persistence requires structural barriers; MVRV-based discretionary trading has weakening edge as the metric becomes consensus.
- **Cycle-classification signals are coarse-time**. Don't use MVRV / NUPL for short-horizon trades; they're months-scale regime classifiers.

## Open Questions for Future Investigation

1. **Backtest feasibility per Tier**: which Tier-1 metrics can be backtested with what window? Realized price has full history; whale labeling has narrower history.
2. **Cross-cycle calibration of MVRV thresholds for 2024-? cycle**: is the >3.5 = late-bull threshold still valid given ETF flow + institutional access?
3. **Cohort cost-basis as embedded gating attribute**: per regime-gate-vs-bias-filter discipline, can STH RP be encoded as bias-filter for trading strategies (long-only above; short-only below)?
4. **Whale labeling accuracy for 2026**: ETF custody wallets are large but structurally long-only. Proper classification critical.
5. **Stablecoin flow → directional signal lag**: what's the typical delay between USDT exchange-inflow surge and BTC price impact? Hours? Days?
6. **Multi-asset on-chain mechanisms**: ETH-specific (gas burn, staking flow), stablecoin-specific (USDT mint pace), DeFi-specific (TVL flow). Currently BTC-centric.

## Related

- this research program capability roadmap rank #2 — Glassnode capability-investment context
- [[foundation/market-structure/crypto-carrier-features]] — on-chain flow as a carrier category
- [[foundation/market-wisdom/tape-reading]] — on-chain as digitally-native tape; Lefèvre's diagnostic patterns operationalized
- [[foundation/market-wisdom/composite-operator]] — Wyckoff's CO; on-chain whale-tracking is the modern operationalization
- [[foundation/market-wisdom/wyckoff-three-laws]] — Effort vs Result via on-chain volume + cost basis
- [[trading/mechanisms/funding-cycle-dynamics]] — perp-side mechanism family; on-chain is orthogonal data layer
- [[trading/mechanisms/liquidation-cascade-aftermath]] — pre-registered cascade detector uses Coinglass; on-chain provides post-cascade confirmation
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — STH cost basis as embedded bias-filter is the design pattern application
- [[trading/mechanisms/stablecoin-mechanisms]] — companion research-substrate page
- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — Regime 15 cites on-chain markers (Glassnode True Market Mean, STH cost basis)
- [[foundation/methodology/regime-marker-data-sources]] — **added 2026-06-07**: the free backtestable-history source for each on-chain cycle marker (MVRV/NUPL/SOPR/Reserve Risk/Puell via Coin Metrics community API + `chaindl`)

## Sources

**Glassnode core**:
- [MVRV Ratio — Glassnode Docs](https://docs.glassnode.com/guides-and-tutorials/metric-guides/mvrv/mvrv-ratio)
- [Mastering the MVRV Ratio — Glassnode Insights](https://insights.glassnode.com/mastering-the-mvrv-ratio/)
- [Breaking up On-Chain Metrics for Short and Long Term Investors — Glassnode Insights](https://insights.glassnode.com/sth-lth-sopr-mvrv/)
- [Introducing Investor Cost Basis On-Chain — Glassnode Insights](https://insights.glassnode.com/unveiling-investor-cost-basis-on-chain/)
- [BTC Short-Term Holder Realized Price and MVRV — Glassnode Studio](https://studio.glassnode.com/charts/btc-sth-realized-price-mvrv?a=BTC)
- [Indicators API — Glassnode Docs](https://docs.glassnode.com/basic-api/endpoints/indicators)
- [MVRV Z-Score — Bitcoin Magazine Pro](https://www.bitcoinmagazinepro.com/charts/mvrv-zscore/)

**CryptoQuant whale + miner + flows**:
- [Bitcoin Exchange Whale Ratio — CryptoQuant](https://cryptoquant.com/asset/btc/chart/flow-indicator/exchange-whale-ratio)
- [Bitcoin Exchange Flows — CryptoQuant](https://cryptoquant.com/asset/btc/chart/exchange-flows)
- [Exchange In/Outflow and Netflow — CryptoQuant User Guide](https://userguide.cryptoquant.com/cryptoquant-metrics/exchange/exchange-in-outflow-and-netflow)
- [Miner Outflow — CryptoQuant User Guide](https://dataguide.cryptoquant.com/miner-flows-indicators/miner-outflow)
- [All Stablecoins Exchange Flows — CryptoQuant](https://cryptoquant.com/asset/stablecoin/chart/exchange-flows)
- [CryptoQuant Flags Whale-Led Bitcoin Deposits — Bitbo](https://bitbo.io/news/cryptoquant-whale-exchange-deposits/)
- [CryptoQuant: bitcoin whale deposit activity grows — The Block](https://www.theblock.co/post/390712/cryptoquant-bitcoin-whale-deposit-ongoing-bear-phase)

**Academic / research**:
- [Forecasting Bitcoin volatility from whale transactions and CryptoQuant — arXiv 2211.08281](https://ar5iv.labs.arxiv.org/html/2211.08281)

---

*Research-substrate page — `seedling` status. Companion to [[trading/mechanisms/stablecoin-mechanisms]]. Will lift to `growing` when first DSL primitive lands and at least one mechanism is backtested. None of these mechanisms are deployment-ready in current pipeline.*
