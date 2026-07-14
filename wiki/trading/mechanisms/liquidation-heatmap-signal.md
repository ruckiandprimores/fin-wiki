---
title: "Liquidation Heatmap as Signal — Construction & the Liquidation-Magnet Hypothesis"
status: seedling
created: 2026-07-12
updated: 2026-07-12
tags: [mechanism, liquidation, heatmap, microstructure, stop-hunting, forced-flow, crypto, day-swing-applicable, research-grade]
---

# Liquidation Heatmap as Signal — Construction & the Liquidation-Magnet Hypothesis

> **TL;DR**: Liquidation heatmaps (Hyblock, Coinglass) are *modeled* maps of where leveraged positions would be force-closed — built by projecting observed/assumed position openings through leverage-tier liquidation arithmetic into price bins. The candidate trading signal is the **liquidation-magnet hypothesis**: price gravitates toward dense clusters because stop-hunters, liquidation engines, and hedging market makers all push flow toward known forced-flow pools. **Honest status: practitioner lore, zero academic validation (2026-07 research pass)** — research-grade only, but in-house testable because Hyblock's API serves historical signed cluster snapshots. Promotes the "liquidation-magnet trades" candidate from [[trading/mechanisms/liquidation-cascade-aftermath]] §Candidate mechanisms into a testable page.

## How the maps are constructed (synthesis)

Full extraction in [[foundation/sources/hyblock-coinglass-liquidation-heatmaps]]; the load-bearing points:

1. **Input is positioning, not orders.** Hyblock anchors on *observed position-opening clusters*; Coinglass on open interest + "market trading volume, leverage usage, and other relevant market data" (its own words — the rest is opaque). Neither sees an exchange liquidation book, because none is published.
2. **Leverage-tier projection.** Each opening is projected to the liquidation price implied by discrete leverage anchors — Hyblock documents 100x/50x/25x for both longs and shorts (API buckets L1–L5). This puts mechanical bright bands ~1%/2%/4% beyond consolidation zones — partly method geometry, not pure positioning evidence.
3. **Price-binned, signed output.** Levels accumulate in price buckets; Hyblock's API returns per-bucket notional `size` + `side` (long/short), so above-price short pools and below-price long pools are separately measurable.
4. **Decay on sweep only.** Levels are removed when price intersects them ("hit"); voluntarily closed positions leave stale levels → both vendors state the map **over-counts**: "the actual number of liquidations will be lower."
5. **Model risk is unauditable at Coinglass.** Three model variants (model1/2/3) with undocumented differences; leverage-distribution assumptions unpublished. Hyblock is auditable in shape (anchors, bins, sides) but not substance (how openings are inferred is undisclosed).

**Two distinct signal readings of the same map:**
- **Magnet (pre-touch):** dense cluster nearby → directional drift *toward* it (this page's hypothesis).
- **Fuel (post-touch):** cluster swept → forced one-way flow → overshoot → aftermath reversion — that is the already-developed [[trading/mechanisms/liquidation-cascade-aftermath]] family. The magnet is the *approach leg*; the aftermath is the *exhaust leg*. They are separate hypotheses and must be tested separately.

## The Liquidation-Magnet Hypothesis

```
Hypothesis:    When modeled liquidation clusters are materially asymmetric around
               spot (e.g., notional in clusters within ±5% above price ≥ 2× the
               notional below, per Hyblock signed buckets), price is more likely
               to travel toward the dominant cluster than away from it over a
               4h–3d horizon, and dense clusters are "touched" at a rate above
               what unconditional volatility implies.

Mechanism:     Three actor classes move money toward the pool:
               (1) Stop-hunters / large aggressive traders — a visible cluster is
                   a map of where forced market orders will fire; pushing price
                   into it converts the pool into exit liquidity for the pusher
                   (the crypto version of the pre-regulation stop-run; legal and
                   routine in perps).
               (2) Exchange liquidation engines — once the first band is touched,
                   auto-liquidation emits one-way market flow that carries price
                   deeper into the adjacent bands (self-reinforcing burst).
               (3) Market makers — anticipating the burst, they thin quotes /
                   pre-hedge on the cluster side, lowering the resistance of the
                   path toward the pool.
               Reflexivity amplifier: Coinglass is watched enough that the bright
               zone is a Schelling focal point — the coordination effect operates
               even if the underlying model is wrong.

Observable:    Hyblock liquidation-heatmap API: per-price-bucket signed notional
               (size + side) at leverage tiers L1–L5, snapshotted via the
               `timestamp` parameter. Cluster-asymmetry factor: (cluster notional
               above spot − below spot) / total, within a ±k·ATR window. Cross-
               check attention channel with Coinglass bright zones (opaque, but
               the watched map).

Expected:      Conditional drift toward the dominant cluster; touch-rate of top-
               decile clusters above ATR-random-walk baseline; magnitude bounded
               by distance-to-cluster (self-liquidating signal: edge, if any, is
               a few % per event at swing horizon).
               expected_net_edge:
                 - source:       NONE AVAILABLE — no academic or independent
                                 empirical estimate exists (2026-07 research
                                 pass); practitioner guides assert the effect
                                 without methodology. No Fermi estimate offered
                                 yet because the over-count bias of the maps
                                 makes cluster notional an unknown multiple of
                                 true fuel.
                 - value:        unknown
                 - independence: N/A
               → "research-grade only — not expecting deployment-grade edge."
               Reason for testing anyway: mechanism mapping — this is a named
               candidate mechanism of the cascade-aftermath family, the
               observable is now API-accessible with historical snapshots, and
               a null result would retire a widely-circulated piece of
               practitioner lore from the idea queue.

Falsifier:     Pre-registered window, BTC + ETH perps, ≥12 months of Hyblock
               snapshots: (a) forward returns conditioned on top-quartile
               cluster asymmetry are not distinguishable in direction from the
               unconditional volatility-matched baseline; AND/OR (b) touch-rate
               of top-decile clusters ≤ ATR-random-walk expectation; AND (c) a
               simple toward-cluster entry with family-default stops fails the
               net-of-friction P&L floor computed from independent data per
               [[foundation/methodology/deployment-edge-threshold]] §4.5.
               Any of (a)+(b) failing kills the magnet reading even if (c) is
               untestable.
```

## Honest evidence status

- **No academic test of the magnet effect exists.** A 2026-07 research pass found no peer-reviewed paper, preprint, or rigorous empirical study testing whether modeled liquidation clusters attract price. Everything retrievable is vendor marketing, exchange education pages, and trading-guide folklore (including an unsourced "price visits 60–70% of major hotspots" claim — treat as unverified).
- **It is practitioner lore.** Widely believed, widely repeated, plausible mechanism — and exactly the profile the wiki flags for extra scrutiny: plausible-sounding, crowded, undated. Per CLAUDE.md's AI-limitations discipline, the burden is on an in-house test, not on the lore.
- **The maps themselves are modeled estimates** with a documented over-count bias and (Coinglass) opaque assumptions — so even a positive in-house result validates "the *map* has signal," not "true liquidation levels attract price." These are different claims; the first is the tradable one.
- **Self-defeating risk** (inherited from the family page): the more the map is watched, the more the simple version is priced; conversely the reflexivity may *sustain* the focal-point effect. Which force wins is an empirical question — another reason this page stays `seedling` until tested.

## Open Questions

1. **Magnet-validation experiment (sketch).** Pull historical Hyblock heatmap snapshots (free API tier; `timestamp` parameter — current free-tier limits unverified) at fixed cadence (e.g. 1h) for BTC/ETH perps over ≥12 months; join against the team's backtesting data layer OHLCV. Compute the cluster-asymmetry factor within ±3/±5% of spot per snapshot. Tests, pre-registered: (a) conditional 4h/1d/3d forward-return sign vs. volatility-matched baseline; (b) top-decile-cluster touch-rate vs. ATR random-walk; (c) event-study around cluster *sweeps* — does the post-touch path match the aftermath family's reversion signatures? (c) doubles as the first data-driven bridge between this page and [[trading/mechanisms/liquidation-cascade-aftermath]] Template 1.
2. **Magnet vs. fuel decomposition.** If clusters "attract" price, is the drift pre-touch (true magnet — actor classes 1+3) or is the measured effect just the post-touch burst dragging averages (actor class 2 only)? Separating the legs decides whether there is an *entry* signal or only a *context* signal.
3. **Cross-vendor divergence as model-noise gauge.** Where Hyblock and Coinglass disagree on cluster location/intensity, does either dominate predictively? Divergence zones are a natural experiment on model fidelity vs. attention (Coinglass = attention instrument, Hyblock = measurement instrument).
4. **Leverage-tier information content.** Do nearest-tier (100x/L1) clusters behave differently from 25x/L5 clusters? The 100x band is mechanically close to price (high touch base-rate); the informative test may live in the mid tiers.
5. **Staleness half-life.** Both maps over-count via un-decayed closed positions. Can a staleness discount (age-weighted cluster notional) be estimated from sweep events where realized liquidation prints (per-exchange feeds) are far below mapped notional?

## Related

- [[foundation/sources/hyblock-coinglass-liquidation-heatmaps]] — the reference-doc source page behind this synthesis
- [[trading/mechanisms/liquidation-cascade-aftermath]] — parent family; this page promotes its "liquidation-magnet trades" candidate mechanism; §Operational Thresholds + Template 1 are the post-touch counterpart
- [[foundation/sources/ali-oct-2025-liquidation-cascade]] — what a fully-loaded map looks like when it clears (Oct 2025, ~$19B)
- [[trading/mechanisms/gamma-exposure-regime]] — sibling forced-flow map (dealer hedging); confluence candidate
- [[trading/mechanisms/failed-breakout-reversal]] — stop-run mechanics at the single-level scale; the magnet is the same logic at pool scale
- [[foundation/market-structure/crypto-carrier-features]] — liquidation maps as crypto-unique carrier
- [[foundation/methodology/deployment-edge-threshold]] — §4.5 governs the expected_net_edge / independence discipline this hypothesis defaults under
- [[foundation/methodology/strategy-validation-discipline]] — pre-registration requirements for the experiment in Open Questions

## Sources

- [Hyblock Academy — Liquidation Levels](https://academy.hyblockcapital.com/tools/liquidation-levels) / [Liquidation Heatmap](https://academy.hyblockcapital.com/tools/liquidation-levels-1) / [API endpoint](https://docs.hyblockcapital.com/liquidation-heatmap) — construction anchors, signed buckets, historical snapshots
- [CoinGlass Learning Center — liquidation heatmap guide](https://www.coinglass.com/learn/how-to-use-liqmap-to-assist-trading-en) / [API Model1](https://docs.coinglass.com/reference/liquidation-heatmap) — first-party estimate framing + relative-intensity disclaimer
- 2026-07 research pass (WebSearch, academic + practitioner): no academic test of the magnet effect found; practitioner guides only (unverified lore)
