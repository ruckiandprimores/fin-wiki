---
title: "Ingest Queue — Researched Source Candidates"
status: living
created: 2026-07-12
updated: 2026-07-12
tags: [meta, sources, ingest-queue, planning, research-pass]
---

# Ingest Queue — Researched Source Candidates

> **TL;DR**: The prioritized queue of ingest candidates, grounded in wiki gaps and validated by research passes (availability, reputation, mechanism-level fit). Companion to [[foundation/sources/lineage]] (which maps what's already ingested); this page holds what's *next* and why. Refreshed on each ingest-research pass; ingested items move to a one-line "done" record.

## How this queue is built

Each entry traces to (a) a named wiki gap (open question, unfilled lineage node, or active vertical with no source base) and (b) a research-pass validation (2026-07-12: three parallel research arms — investing-branch, trading-branch crypto-native, recent methodology papers). Priority weights: active-thread relevance (state-monitor era for trading; newly active verticals for investing) > pre-committed obligations > cost (free/fast first).

## Tier 1 — INGESTED 2026-07-12

One-line record; see the wiki activity log entry of 2026-07-12 for the full ingest:

1. ✅ Hyblock + Coinglass liquidation-heatmap docs → [[foundation/sources/hyblock-coinglass-liquidation-heatmaps]] + [[trading/mechanisms/liquidation-heatmap-signal]]
2. ✅ Fear & Greed causality evidence (contradiction pair + 141-predictor study) → sentiment-evidence source pages
3. ✅ Kolchinsky *The Great American Drug Deal* (research-synthesis grade) → [[foundation/sources/kolchinsky-great-american-drug-deal]]
4. ✅ Kiel rearmament reports + CRS defense primers → [[foundation/sources/kiel-european-rearmament]] + [[foundation/sources/crs-defense-primers]] + [[foundation/asset-classes/defense-economics-primer]]
5. ✅ Talos crypto market-impact + Maitrier metaorder reconstruction → [[foundation/sources/talos-crypto-market-impact]] + [[foundation/sources/maitrier-metaorder-reconstruction]]

## Tier 2 — active-vertical depth (next up)

| Source | Gap | Notes |
|---|---|---|
| **UBS/DMS Global Investment Returns Yearbook 2026 summary** (free PDF) + **AQR "International Diversification — Still Not Crazy After All These Years"** (2023, free) | Ex-US leg — empirical base rates | 125yr cross-country returns + the long-horizon diversification evidence; not yet represented in the ex-US thesis (it cites CMA numbers, not this layer). Free, fast. |
| **Rogoff, *Our Dollar, Your Problem*** (2025, Yale UP) | Ex-US leg — dollar-cycle macro foundation | Maps onto the thesis's "hedge, not collapse-call" framing; base rates for the #1 falsifier (the dollar). Book purchase. |
| **Eichengreen, *Exorbitant Privilege*** (2011, OUP) | Reserve-currency mechanism baseline | The model Rogoff 2025 updates; the 2011-vs-2025 delta is itself informative. Book purchase. |
| **Pisano, *Science Business*** (2006, HBS Press) | Pharma CoC book #2 | R&D-productivity economics; relevant to the tools/bioprocessing thesis (TMO/DHR). Structural material holds; case studies dated. Owner-reading track. |
| **ETH market-structure cluster**: builder-market decentralization (arXiv:2405.01329) + Ethena optimal-control (arXiv:2605.11263) + PBS SoK (arXiv:2506.18189) | Open question C1 — no ETH structure page | One pass answers C1: MEV supply chain anchor + the ETH-vs-BTC structural basis difference (staking-yield floor under funding; Ethena ~5% of ETH perp OI as structural short). All free. |
| **CryptoQuant User Guide** (userguide.cryptoquant.com) | C2 — crypto-native metric literacy | Winner among the three unfilled reference nodes: per-metric definition+formula+interpretation, on-chain AND derivatives, machine-readable (JSON catalog + OpenAPI + MCP). Glassnode Academy as follow-on; Coinglass NOT the literacy node. Free. |
| **Risk-Based Auto-Deleveraging** (arXiv:2603.15963) | Post-cascade venue mechanics | Downstream extension of [[foundation/sources/ali-oct-2025-liquidation-cascade]]: insurance-fund exhaustion → ADL, with Oct-2025 + Jan-2026 events as case data. Free. |

## Tier 3 — methodology watch (selective, MED)

| Source | Fit | Caveat |
|---|---|---|
| **Signature novelty-detection on path space** (Lyons/Salvi group, arXiv:2512.03243) | Calibrated-significance layer for the ingested [[foundation/sources/issa-horvath-online-regime-detection|sig-MMD detector lineage]] — "how to set the alarm threshold" | Validated on non-finance data; transfer work needed |
| **Multi-Scale MS-GARCH** (arXiv:2606.06190) | Closest published analog to 15m/4h/1d state composition (27-state cross-scale probability tensor) | Solo author, FX not crypto; **verify filtered-vs-smoothed before borrowing any mechanic** (the SM2 trap) |
| **Statistical jump models** (Shu/Yu/Mulvey, arXiv:2402.05272; `jumpmodels` on PyPI) | Fourth detector family (vs HMM/BOCPD/sig-MMD); evaluated WITH transaction costs; regime→risk-gating (prior-#14-compatible) | 2024, outside the recent window — flagged not force-fit |
| **Trend-horizon redundancy** (arXiv:2510.23150) | Evidence mid-band horizons can be informationally redundant — bears on whether the 4h leg adds independent state information | Futures trend premia; days-to-months bands — maps by analogy only |
| **Geometric observables for regime detection** (arXiv:2605.17117) | Orthogonal detector family + a strong evaluation protocol (OOS effect sizes + false-alarm rates, 46 baselines) | "Quantum" framing is hype-flavored; ingest for protocol, discount narrative |

## Tier 4 — lineage backlog (reading-rhythm dependent)

Unchanged unfilled nodes from [[foundation/sources/lineage]]: Munger *Poor Charlie's Almanack* → Graham & Dodd *Security Analysis* → Fisher *Common Stocks and Uncommon Profits* (value lineage); **Edwards & Magee** (the one unfilled node in the top-priority pre-regulation chain); Chancellor *Devil Take the Hindmost*; Kahneman → Mandelbrot → Taleb (behavioral/risk cluster, entirely unfilled); Ilmanen *Expected Returns*; Quantpedia; Klein & Pettis *Trade Wars Are Class Wars* (ex-US depth layer); Gansler *Democracy's Arsenal* + **Brose *The Kill Chain*** (defense depth + the falsifier-side read — flagged in [[foundation/asset-classes/defense-economics-primer]]); Vardi *For Blood and Money* (pharma case-study complement); Chan; Grinold & Kahn; Drobny; Lien.

**Deprioritized after the 2026-07-12 research pass**: Werth *Billion Dollar Molecule* (superseded by Vardi for an investor), Eban *Bottle of Lies* (generics fraud — tangential to a large-cap-innovator + tools CoC), Coinglass docs as a metric-literacy node (API reference, thin conceptual layer).

## Negative findings — recorded as open questions, NOT ingests (2026-07-12 pass)

1. **No academic test of the liquidation-magnet hypothesis exists** — practitioner lore only. In-house testable (Hyblock free API + historical snapshots); experiment sketch in [[trading/mechanisms/liquidation-heatmap-signal]] §Open Questions.
2. **No ETH-vs-BTC basis comparison paper exists** — genuine literature gap; flag on the future ETH structure page (C1).
3. **No 2025-26 paper quantifies the smoothed→filtered regime-detection haircut** — the team's own lookahead-correction measurement is ahead of the public literature (SM2).
4. **No academic paper-to-live degradation study exists** — the daily observation-trail loop is primary research (SM3); noted in [[foundation/methodology/deployment-edge-threshold]].
5. **No new evidence-grade work on funding-rate extremes as contrarian signal** beyond the already-ingested funding papers — extract as an in-house falsifiable hypothesis, not an ingest.

## Related

- [[foundation/sources/lineage]] — the ingested-source graph this queue feeds
- [[questions/index]] — open questions; several queue entries trace to C1/C2/B2/SM2/SM3
- [[foundation/idea-sourcing-methodology]] — the sourcing meta-process

## Sources

- 2026-07-12 ingest-research pass: three parallel research arms (investing-branch sources; trading-branch crypto-native; 2025-26 methodology papers) — full findings synthesized in the 2026-07-12 wiki activity log entry. Availability/reputation claims verified via web at pass time; re-verify before purchase/ingest.
