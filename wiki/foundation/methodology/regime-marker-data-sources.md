---
title: "Regime-Marker Data Sources — the Backtestable-Free Core"
status: growing
created: 2026-06-07
updated: 2026-07-12
tags: [foundation, methodology, regime, data-sources, observability, backtestable, on-chain, sentiment, capability, day-swing]
---

# Regime-Marker Data Sources — the Backtestable-Free Core

> **TL;DR**: The wiki *names* a regime-marker set ([[foundation/macro/btc-regime-taxonomy-2020-2025|BTC regime taxonomy]]: price-vs-200d-MA, realized vol, funding, OI, stablecoin supply, BTC dominance, LTH cost basis, treasury-vehicle mode, dollar-liquidity) and now has prospective-detection *methods* ([[foundation/methodology/online-regime-detection]]). What was missing is the **data layer between them** — where to get each marker as a **historical time-series so it's backtestable**, not just glanceable live. This page is that map. The actionable headline: a small **free + public-API + history** core makes most markers backtestable without paying for a data vendor — **alternative.me Fear & Greed** (full history to 2018, documented methodology), **DefiLlama** (stablecoin supply + TVL, no-key API), **Coin Metrics community API** (Realized Cap, MVRV, no-key), and the **`chaindl`** scraper (bridges the otherwise glance-only on-chain dashboards into CSV). Coinglass and CoinGecko are great *live* but their *history* is paywalled — route funding/OI/dominance through exchange-native APIs instead. **This is the data substrate for the filtered-haircut experiment** (`questions/index` C4) and any regime-gated backtest.

## Why this page exists

You cannot backtest a regime gate without the marker's history. The [[foundation/macro/btc-regime-taxonomy-2020-2025|taxonomy]] tells you *which* markers matter; [[foundation/methodology/online-regime-detection]] tells you *how* to call a regime live. This page answers *where the numbers come from* — and crucially, **which sources give free historical series (backtestable) vs only a live snapshot (glance-only)**. The distinction is load-bearing: a live-glance dashboard cannot validate a regime classifier; only a historical series can. Sibling to [[foundation/sources/vertical-data-sources]] (the long-horizon / money-flow data catalogue) on the trading-regime side.

**Domain-fit note**: this is a *data-source / observability* page, not a mechanism page. Per a standing convention, cataloguing the free backtestable feeds sharpens the capability-investment decision — most of the regime workstream's data need turns out to be **free**, the gap is code (ingestion + a feature store), not a vendor spend.

## The backtestable-free core (use these first) {#free-core}

Four sources clear the **free + public API + historical** bar cleanly. Everything else is either also-free-API-but-narrower, or paywalled-history.

### 1. alternative.me Crypto Fear & Greed Index — free API, full history to 2018 {#alternative-me}

The **original** crypto Fear & Greed index (since Feb 2018), and the **backtestable substitute for CoinMarketCap's proprietary one** (which is undocumented and not historically exportable — see [[foundation/methodology/online-regime-detection#practitioner-proxies|CMC cite-only verdict]]).

- **API**: `https://api.alternative.me/fng/` — free, **no key**, attribution requested. `limit=0` returns the **full daily history** (verified: 3,045 points, oldest 2018-02-01). Fields: `value` (0–100), `value_classification`, `timestamp`. `format=csv` supported.
- **Documented 6-component methodology** (the reason it's analysable, not a black box on *weights*):

  | Component | Weight | Reads |
  |---|---:|---|
  | Volatility | 25% | BTC vol + max drawdown vs 30/90d avg |
  | Market momentum / volume | 25% | volume + momentum vs historical |
  | Social media | 15% | X/Twitter hashtag interaction rate |
  | Surveys | 15% | **currently paused** |
  | Dominance | 10% | BTC market-cap share |
  | Trends | 10% | Google Trends BTC search |

- **Maps to**: composite sentiment regime marker; overlaps the vol-state axis (volatility component) + BTC.D marker.
- **Honest caveat**: weights are documented but the *underlying series are not published* — backtest it as a **signal**, not a reconstructable formula. Its vol + dominance sub-drivers **double-count** markers you already track independently, so don't stack it naively on top of those. Surveys component paused since ~2024 (composition drift).
- **Verdict: INGEST-WORTHY** — the only free, documented, fully-historical composite-sentiment series.
- **Evidence grade (added 2026-07-12): reactive context, not a standalone signal.** A 2018–2025 daily VAR with an out-of-sample test (Gessie 2026) finds ∆FGI does **not** Granger-cause BTC returns and adds no OOS gain (OOS R² ≈ −1.1%), while returns strongly Granger-cause ∆FGI; the lone counterpoint (Cavalheiro et al. 2024) is a 1-year weekly in-sample study. Backtest the series as a **state descriptor / regime marker**, not an entry signal; contrarian-at-extremes remains untested either way. Full contradiction pair: [[foundation/sources/fgi-causality-evidence-pair]].

### 2. DefiLlama — free no-key API, historical stablecoin supply + TVL {#defillama}

- **API**: `https://api.llama.fi` / `https://stablecoins.llama.fi` — **no key, no auth, no rate limit** on free endpoints (Pro $300/mo only for higher limits).
- `/stablecoins` + `/stablecoincharts` → **historical stablecoin supply / market cap** = the wiki's **stablecoin-supply regime marker, directly backtestable**. TVL by chain/protocol = a risk-on/off liquidity proxy.
- **Maps to**: stablecoin-supply marker (macro liquidity into crypto); TVL as DeFi risk-appetite.
- **Verdict: INGEST-WORTHY** — one of only two native free+historical+API sources; turns a named marker into a series with zero friction.

### 3. Coin Metrics community API — free no-key, Realized Cap + MVRV {#coinmetrics}

- **API**: `https://community-api.coinmetrics.io` — **no key, free, historical**, ~1.6 RPS cap.
- Carries `CapRealizedUSD` (**Realized Cap** — aggregate cost basis, the base layer under MVRV/NUPL), MVRV, supply, network data.
- **Maps to**: realized-cap / MVRV cohort-valuation markers; the only **native** free programmatic on-chain source (vs scraping).
- **Verdict: INGEST-WORTHY** — the clean programmatic path for the cost-basis marker family.

### 4. `chaindl` — open-source scraper bridge for on-chain dashboards {#chaindl}

- **Tool**: `github.com/dhruvan2006/chaindl` — a no-key Python library that scrapes the *free public chart JSON* behind Checkonchain, Bitcoin Magazine Pro (the old `lookintobitcoin.com` now 301-redirects here), Bitbo, Glassnode-free, The Block, Dune → returns a pandas DataFrame / CSV.
- **Why it matters**: the canonical on-chain cycle markers (MVRV-Z, NUPL, SOPR, Puell, Reserve Risk) are viewable-free on those dashboards but **glance-only** on their native tiers. `chaindl` is the bridge that makes them **historical CSV without paying Glassnode/CryptoQuant API tiers.** Highest-leverage single tool for the on-chain marker layer.
- **Verdict: INGEST-WORTHY (tool reference)** — flag the scrape-dependency fragility (breaks if a dashboard changes its JSON), so treat as a convenience layer, not a production feed.

## On-chain cycle/valuation markers — mechanism + where to get history {#onchain-markers}

The canonical on-chain regime/cycle indicators. Mechanisms live in [[trading/mechanisms/on-chain-mechanisms]]; this table adds the **free-historical source** column. All are BTC-centric.

| Marker | Mechanism (who moves money) | Free + historical source |
|---|---|---|
| **MVRV / MVRV Z-Score** | Market cap vs realized cap (aggregate cost basis). Z-score extremes = unrealized profit/loss stress → distribution near tops, capitulation near bottoms. | Coin Metrics community (`CapMVRVCur`); `chaindl` (BM Pro / Checkonchain) |
| **SOPR (STH / LTH split)** | Realized profit/loss ratio of coins moved. STH-SOPR = reactive short-term-holder behaviour; LTH-SOPR = long-term-holder distribution. Crosses of 1.0 = profit/loss inflection. | `chaindl`; Glassnode/CryptoQuant paid for clean STH/LTH split |
| **NUPL** | Net unrealized profit/loss of supply → euphoria / optimism / anxiety / capitulation zones (maps to the bull/bear range axis). | `chaindl` (BM Pro / Checkonchain free chart) |
| **Realized Cap** | Coins valued at last-moved price = aggregate cost basis; base layer under MVRV/NUPL. | **Coin Metrics community (`CapRealizedUSD`)** — cleanest free programmatic |
| **Reserve Risk** | LTH conviction vs price (HODL opportunity cost). Low = strong hands accumulating cheap = bottom signal. | `chaindl` (BM Pro / Checkonchain) |
| **Puell Multiple** | Daily miner issuance USD ÷ 365d avg → miner sell-pressure regime; cycle low/high. | `chaindl`; or compute from issuance + price |
| **Mayer Multiple** | Price ÷ 200d MA (over/under-extension). | **Compute from free OHLC** (Binance/CoinGecko) — no dependency |

## Live-but-history-paywalled (use exchange-native instead for backtest) {#live-only}

| Source | Free? | History? | Maps to | Verdict |
|---|---|---|---|---|
| **Coinglass** | Website free (funding, OI, long/short, **liquidation heatmap** across 30+ venues) | **API paid** (~$29–299/mo); no free API tier | funding, OI, liquidation-cascade markers | **INGEST the page** for the [[trading/mechanisms/liquidation-cascade-aftermath|liquidation-heatmap / magnetic-zone]] concept; for backtestable funding/OI prefer **exchange-native APIs** (Binance/Bybit funding + OI endpoints — free + historical, already in the team's stack) |
| **CoinGecko Demo API** | Free (no-cost key, 100 calls/min) | **live `/global` dominance + total-mcap free; historical `/global/market_cap_chart` PAID** | BTC dominance, total market cap | **INGEST for the live layer**; flag the **historical-dominance gap** — reconstruct from free `/coins/markets` per-coin caps, or accept a paid tier |

## Cite-only / glance-only {#cite-only}

- **Glassnode free tier** — live/weekly *snapshots* of MVRV/SOPR/exchange flows; real-time + STH/LTH split + API history are paid. Their docs (`docs.glassnode.com`) are an excellent **methodology reference** — cite for definitions. Their GEX research feeds [[trading/mechanisms/gamma-exposure-regime]].
- **CryptoQuant free** — MVRV/SOPR/funding charts free-glance; $39/mo for depth + API. The metric user-guide pages are clean one-line mechanism statements — cite-only.
- **CoinMarketCap Fear & Greed / Altcoin-Season** — proprietary, not historically exportable; superseded by alternative.me for backtest. Cite-only baseline per [[foundation/methodology/online-regime-detection#practitioner-proxies]].
- **The Block / Blockchain.com Charts** — free dashboards; The Block reachable via `chaindl` for specific charts; Blockchain.com too low-level for regime markers directly (feeds Puell/issuance).
- **Velo (velodata.app)** — claims a free derivatives/funding API tier with history; a candidate free Coinglass-API alternative but **unverified this pass** — flagged for confirmation before relying on it.

## Named-marker → backtestable-source map (the operational lookup) {#marker-map}

For each [[foundation/macro/btc-regime-taxonomy-2020-2025|taxonomy]] marker, the cheapest path to a backtestable series:

| Regime marker | Backtestable source | Free? |
|---|---|---|
| Price vs 200d MA + slope, **Mayer Multiple** | free OHLC (Binance/CoinGecko), compute | ✅ |
| Realized vol (30d) | free OHLC, compute | ✅ |
| **Funding rate** | **exchange-native** (Binance/Bybit funding endpoints) — free + historical | ✅ |
| **Open interest** | exchange-native — free + historical | ✅ |
| **Stablecoin supply** | **DefiLlama** `/stablecoincharts` | ✅ |
| BTC dominance | CoinGecko live free; **history paid** → reconstruct from per-coin caps | ⚠️ |
| Realized Cap / MVRV / cohort cost basis | **Coin Metrics community** + `chaindl` | ✅ |
| LTH/STH SOPR, NUPL, Reserve Risk | `chaindl` (clean STH/LTH split = Glassnode paid) | ◐ |
| Composite sentiment | **alternative.me F&G** | ✅ |
| Treasury-vehicle mode | quarterly disclosures (manual; per [[foundation/macro/treasury-vehicle-cascade-watch]]) | manual |
| Dollar-liquidity overlay | FRED (RRP/SRF), MOVE index — free | ✅ |

**The encouraging conclusion**: of the wiki's named markers, the majority have a **free backtestable source**. The genuine gaps are (a) clean STH/LTH SOPR split (Glassnode-paid or noisy free proxy), and (b) historical BTC dominance (reconstruct or pay). Neither blocks the regime workstream.

## Open questions {#open-questions}

- **Velo free-tier verification** — is it a genuine free Coinglass-API alternative with historical funding/OI? Unverified; confirm before relying.
- **Historical-dominance reconstruction** — reconstruct BTC.D from CoinGecko free per-coin caps, or accept a paid tier? BTC.D is a named marker but not freely historical from the obvious source.
- **Does alternative.me F&G add signal beyond its components?** Since it double-counts vol + dominance, the test is whether the *composite* beats those markers used directly on a filtered regime backtest (ties to [[foundation/methodology/online-regime-detection#open-questions|the CMC-baseline falsifier]]). **Partially answered 2026-07-12**: for next-day BTC returns the composite adds nothing OOS ([[foundation/sources/fgi-causality-evidence-pair]]); the regime-gate (state-description) half of the question is still open.
- **Feature-store build** — turning this catalogue into an actual ingested historical marker table is the code-level capability that unblocks the C4 filtered-haircut experiment.

## Key Takeaways

- **Backtestable-free core**: alternative.me F&G (sentiment, history to 2018, documented), DefiLlama (stablecoin supply + TVL), Coin Metrics community (Realized Cap/MVRV), `chaindl` (on-chain dashboard → CSV bridge). No vendor spend required.
- **alternative.me > CoinMarketCap** for regime work: documented methodology + full historical API makes it backtestable; CMC's is proprietary and live-glance.
- **Funding / OI / dominance**: Coinglass + CoinGecko are great *live* but history is paywalled — use **exchange-native APIs** (already in the stack) for backtestable funding/OI; reconstruct dominance.
- **Most named markers have a free backtestable source** — the remaining gaps (clean STH/LTH SOPR, historical dominance) don't block the workstream.
- This is the **data substrate** for the filtered-haircut experiment and any regime-gated backtest — capability-blocked (code/feature-store), not research-blocked, and not vendor-blocked.

## Related

- [[foundation/macro/btc-regime-taxonomy-2020-2025]] — the marker set this page sources; live feeds noted in its Sources section
- [[foundation/methodology/online-regime-detection]] — the prospective methods this data feeds; CMC-baseline falsifier
- [[trading/mechanisms/on-chain-mechanisms]] — the mechanisms behind the on-chain cycle markers (this page adds the source column)
- [[trading/mechanisms/liquidation-cascade-aftermath]] — Coinglass liquidation-heatmap / magnetic-zone consumer
- [[trading/mechanisms/funding-cycle-dynamics]] — funding-rate marker (exchange-native source)
- [[foundation/macro/treasury-vehicle-cascade-watch]] — treasury-vehicle-mode marker (manual quarterly source)
- [[foundation/sources/vertical-data-sources]] — sibling data-source catalogue (long-horizon / money-flow side)
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — why funding markers need ETF-era calibration before naive use

## Sources

- alternative.me — [Fear & Greed Index](https://alternative.me/crypto/fear-and-greed-index/), [API docs](https://alternative.me/crypto/api/), endpoint `api.alternative.me/fng/` (history verified to 2018-02-01, 2026-06-07).
- [DefiLlama API docs](https://api-docs.defillama.com) — free no-key stablecoin + TVL endpoints.
- [Coin Metrics community API](https://docs.coinmetrics.io/api/v4) — free no-key historical (`CapRealizedUSD`, MVRV).
- `chaindl` — github.com/dhruvan2006/chaindl (on-chain dashboard scraper).
- [Coinglass](https://www.coinglass.com) + [pricing](https://www.coinglass.com/pricing) (API paid); [CoinGecko API](https://www.coingecko.com/en/api) (live global free, history paid).
- On-chain marker definitions: Glassnode Academy / `docs.glassnode.com`; Checkonchain; Bitcoin Magazine Pro (ex-LookIntoBitcoin).
- Research synthesis 2026-06-07 (live API verification of free/paid + history depth per source).
