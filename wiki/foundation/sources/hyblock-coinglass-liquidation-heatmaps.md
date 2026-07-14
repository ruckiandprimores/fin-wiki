---
title: "Liquidation Heatmap Methodology — Hyblock & Coinglass (Reference Docs)"
status: growing
created: 2026-07-12
updated: 2026-07-12
tags: [source, reference-docs, liquidation, derivatives, heatmap, microstructure, market-structure, day-swing-applicable]
---

# Liquidation Heatmap Methodology — Hyblock & Coinglass (Reference Docs)

> **TL;DR**: Reference-doc extraction of how the two market-watched liquidation heatmaps are actually built. **Both are modeled estimates, not exchange liquidation-engine output** — Hyblock projects liquidation prices from *observed position-opening clusters* at fixed high-leverage anchors (100x/50x/25x, long and short) into price buckets with signed long/short notional; Coinglass states only that it "calculat[es] liquidation risks across different price ranges using market trading volume, leverage usage, and other relevant market data" and keeps the model otherwise opaque (three undocumented model variants). Coinglass matters anyway because the market watches it — the reflexivity, not the fidelity, is the tradable object.

## Citation

- Hyblock Capital Academy, *Liquidation Levels* and *Liquidation Heatmap* tool docs (academy.hyblockcapital.com), plus Hyblock API reference *Liquidation Heatmap* endpoint (docs.hyblockcapital.com). Fetched 2026-07-12.
- CoinGlass, *How to use Liquidation Heatmaps to assist trading?* (Learning Center, first-party) plus CoinGlass API reference *Pair Liquidation Heatmap Model1* (docs.coinglass.com). Fetched 2026-07-12.

## Position in lineage

The **instrumentation layer** beneath the cascade-aftermath family. [[foundation/sources/ali-oct-2025-liquidation-cascade|Ali (2025)]] supplies the worked case study of a cascade firing and [[trading/mechanisms/liquidation-cascade-aftermath]] the mechanism family; this page documents how the *maps of the fuel* are constructed — which is prerequisite reading before treating any heatmap level as an input (the family's Coinglass dependency is capability roadmap #3, and its templates cite `liq_cluster_density` as a required primitive). Deliberately ingested as a pair: **Hyblock = methodology-transparent** (construction partially documented, API exposes raw buckets), **Coinglass = methodology-opaque but market-watched** (the reflexive focal point). Feeds the new mechanism page [[trading/mechanisms/liquidation-heatmap-signal]].

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order flow (forced-flow pool mapping) + manipulation-aware (stop hunting toward visible clusters) + liquidity provision (market makers hedging around known pools).
- **Axis 2 — Driver**: forced flow (exchange auto-liquidation engines convert clusters into one-way market orders) + coordination problem (a widely-watched map becomes a Schelling focal point) + information asymmetry inverted (modeled maps make retail leverage pools visible to everyone).
- **Axis 3 — Crypto applicability**: **direct** — crypto-native instrumentation with no clean TradFi analog at retail visibility; the maps only exist because perp leverage and OI data are public.

## Key findings

### Hyblock — construction (partially documented)

The construction chain, assembled from the two Academy pages + API reference:

1. **Position-opening detection.** Levels are "based on clusters of price points where highly leveraged traders open long or short positions" (Academy, *Liquidation Levels*). The heatmap "predicts where liquidation levels are opening but not closing" (Academy, *Liquidation Heatmap*). So the input is *observed position-opening flow*, not a static OI snapshot. **How openings are inferred (OI-delta vs. taker-flow attribution) is NOT disclosed** — see honest scope below.
2. **Leverage-tier anchors.** "High leverage is identified as 100x, 50x, and 25x leverages used for both long and short positions" (Academy, *Liquidation Levels*). Each detected opening is projected to the liquidation prices implied by those leverage anchors. The API generalizes this to five buckets: `leverage` parameter "L1: Highest Leverage" through "L5: Lowest Leverage", or `all`.
3. **Price bins.** "The calculated levels are then added to a price bucket on the chart" (Academy). API response = buckets with `startingPrice` / `endingPrice`.
4. **Signed long-vs-short values.** Each API bucket carries `size` (notional USD, e.g. `37116941.87`) and `side` (`long` | `short`) — long and short pools are separately measurable, not blended into one intensity number.
5. **Position-size tiers.** "We show various position sizes (tiers) that are dynamic for each coin and exchange" — higher tiers show fewer/larger levels; tier boundaries are per-coin/per-exchange, not fixed dollar thresholds.
6. **Level removal.** Levels that have been "hit" (intersected with price) are marked/removed — the map decays as price sweeps through it. No removal mechanism for *voluntarily closed* positions is documented (over-count by construction; Hyblock states "the actual number of liquidations will be lower").
7. **Coverage/parameters.** Default exchange set `binance_perp_stable, bitmex_perp_coin, bybit_perp_coin` (+ aggregated views); `lookback` from 12h (1-minute resolution) to 2y (daily); `scaling` relative or max; a `timestamp` parameter allows **historical snapshots** — which is what makes the magnet hypothesis in-house testable (see [[trading/mechanisms/liquidation-heatmap-signal]]).

### Coinglass — construction (first-party statements only; model opaque)

- **What it is (first-party):** "a tool designed to help traders gain insight into market dynamics by *predicting potential large-scale liquidation points*" built by "calculating liquidation risks across different price ranges using market trading volume, leverage usage, and other relevant market data" (Learning Center). Rendering: intensity gradient "from low-intensity purple to high-intensity bright yellow"; bright yellow = "price ranges with a high concentration of potential liquidations". Timeframes 12h–1y.
- **What it is NOT (first-party):** "The liquidation levels displayed on the heatmap represent a *relative intensity indicator*… The actual liquidation amounts that occur may be lower than the levels shown." I.e. Coinglass itself frames the output as a relative-intensity estimate that over-counts.
- **API surface:** three heatmap model variants per scope — pair-level `liquidation-heatmap` (model1) / `model2` / `model3`, plus coin-aggregated equivalents, plus discrete `liquidation-map` endpoints. **The difference between model1/2/3 is not documented** (unverified). Model1 response = `y_axis` price array + `liquidation_leverage_data` triplets of (x-index, y-index, liquidation amount USD) + candlesticks. Update frequency stated as "Real time for all the API plans"; heatmap endpoints are gated to Professional/Enterprise API tiers.
- **Third-party reconstruction (NOT first-party; lower confidence):** practitioner write-ups describe the pipeline as: snapshot OI per exchange → assume a leverage distribution (calibrated to historical patterns / exchange-reported aggregates) → apply standard liquidation-price formulas incl. maintenance margin → aggregate/normalize into price-density. Directionally consistent with Coinglass's own vague statement, but Coinglass has never published the leverage-distribution assumptions or formulas (unverified). One blog-level claim that "price visits ~60–70% of major hotspots within their timeframe" circulates; **no methodology behind that number was found — treat as unverified folklore.**

### Shared structural properties (both vendors)

- **Modeled, not observed.** No exchange publishes its full liquidation book; the public per-exchange liquidation feeds are sampled/throttled (Binance's forced-order stream has been rate-limited since 2021 — widely reported; unverified in this pass), so *every* public heatmap is a reconstruction.
- **Over-count by construction.** Both vendors state actual liquidations will be lower than mapped: closed/adjusted/partially-deleveraged positions leave stale levels on the map until price sweeps the bucket.
- **Leverage-anchor artifact.** Because levels are projected at discrete leverage anchors, the bright bands sit at mechanical distances from recent price consolidation (~1% for 100x, ~2% for 50x, ~4% for 25x, before maintenance-margin adjustment) — dense bands just beyond local highs/lows are partly a *geometry* of the method, not independent evidence of positioning.

## Why it matters / honest scope

- **CRITICAL: both maps are modeled estimates, not exchange data.** Nothing on either heatmap is a confirmed resting order or a confirmed pending liquidation. Any strategy language like "there are $X of liquidations at $Y" is category error; the correct statement is "the model projects $X of *potential* liquidations at $Y, conditional on undisclosed inference assumptions."
- **Coinglass is opaque end-to-end.** Three model variants with undocumented differences, no published leverage-distribution assumptions, no published formulas. Its own docs describe the output as a *relative intensity indicator*. It cannot be independently validated — only its *predictions* (does price interact with bright zones?) can be tested.
- **Hyblock is transparent about anchors, opaque about detection.** The 100x/50x/25x long+short anchor scheme, price buckets, signed sides, and size tiers are documented — but the load-bearing step (how a "position opening" is inferred from market data) is not. Hyblock's construction is therefore *auditable in shape but not in substance*.
- **The reflexivity angle — why Coinglass matters even if wrong.** Coinglass is the map the market watches. A widely-watched cluster is a Schelling focal point: stop-hunters have a target, and other traders pre-position around it, *regardless of whether the underlying model is accurate*. This is the same logic as widely-watched round numbers or the classic pivot levels — accuracy is secondary to common knowledge. Consequence: Hyblock is the better *measurement* instrument (raw signed buckets via API); Coinglass is the better *crowd-attention* instrument.
- Consistent with the wiki's standing caveat in [[trading/mechanisms/liquidation-cascade-aftermath]] §Honest Caveats: liquidation-cluster fades are widely watched and the simple version is increasingly priced.

## Related

- [[trading/mechanisms/liquidation-heatmap-signal]] — the mechanism page this source feeds: construction synthesis + liquidation-magnet hypothesis (research-grade)
- [[trading/mechanisms/liquidation-cascade-aftermath]] — the mechanism family whose Template 1 requires `liq_cluster_density`; §Crypto Carriers lists Coinglass maps as the cluster-visibility carrier
- [[foundation/sources/ali-oct-2025-liquidation-cascade]] — sibling source; the Oct 2025 cascade is what a mapped leverage stack looks like when it clears
- [[foundation/market-structure/crypto-carrier-features]] — carrier catalog (liquidation maps as crypto-unique carrier)
- [[foundation/methodology/regime-marker-data-sources]] — data-source routing note (Coinglass heatmap API paid; funding/OI routable via exchange-native)
- [[trading/mechanisms/gamma-exposure-regime]] — the other forced-flow map; GEX sign + liquidation-pool location are two lenses on one forced-flow regime

## Sources

- [Hyblock Academy — Liquidation Levels](https://academy.hyblockcapital.com/tools/liquidation-levels) — leverage anchors (100x/50x/25x long+short), position-opening clusters, dynamic size tiers, hit-removal
- [Hyblock Academy — Liquidation Heatmap](https://academy.hyblockcapital.com/tools/liquidation-levels-1) — price buckets, black→yellow scale, opening-not-closing over-count caveat, exchange/lookback coverage
- [Hyblock API — Liquidation Heatmap endpoint](https://docs.hyblockcapital.com/liquidation-heatmap) — L1–L5 leverage buckets, signed `size`+`side` per price bucket, historical `timestamp` parameter
- [CoinGlass Learning Center — How to use Liquidation Heatmaps to assist trading?](https://www.coinglass.com/learn/how-to-use-liqmap-to-assist-trading-en) — first-party "predicting potential… liquidation points" framing + relative-intensity disclaimer
- [CoinGlass API — Pair Liquidation Heatmap Model1](https://docs.coinglass.com/reference/liquidation-heatmap) — response schema, "real time" claim, Professional/Enterprise gating; model2/model3 + aggregated variants exist with undocumented differences
- Third-party reconstructions of the Coinglass pipeline (OI snapshot + assumed leverage distribution + maintenance-margin formulas): practitioner guides (Kalena, DEXTools, 2026) — **flagged: not first-party, unverified**
