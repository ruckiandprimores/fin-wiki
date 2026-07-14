---
title: "An Empirical Model of Market Impact in Cryptocurrency Trading (Talos / Hoch 2025)"
status: seedling
created: 2026-07-12
updated: 2026-07-12
tags: [source, practitioner, market-impact, execution, slippage, market-microstructure, square-root-law, crypto, day-swing-applicable]
---

# An Empirical Model of Market Impact in Cryptocurrency Trading (Talos / Hoch 2025)

> **TL;DR**: The first large-sample **crypto-specific empirical market-impact model** — Talos (institutional crypto execution platform) calibrated a three-component execution-cost decomposition (**spread cost + physical/temporary impact + time risk**) on proprietary data: **50,000+ parent (meta) orders / ~50M child orders, top-60 spot AND perpetual contracts, Jun 2024–Jul 2025**. The physical-impact term is a **sigmoid-modified square-root law** in participation rate — notably, the plain square-root law *underestimates* actual slippage by ~4bp in the sub-0.5% participation regime where most institutional orders live. **The full white paper is gated** (registration download); this page carries the public layer only. This is the crypto empirical complement to [[foundation/sources/bouchaud-trades-quotes-prices]] and the natural calibration source for the paper-fill→live-fill translation question.

## Citation

Eliad Hoch (Head of Quantitative Execution Services, Talos), *An Empirical Model of Market Impact in Cryptocurrency Trading: Decomposing Execution Costs into Spread, Temporary and Risk Components*, Talos white paper, published Dec 23 2025. Full paper behind a gated download; the insights page states a peer-reviewed journal publication is forthcoming. Public-layer ingest 2026-07-12 from the insights page + the public companion explainer ("Understanding Market Impact in Crypto Trading: The Talos Model for Estimating Execution Costs") + the VWAP-vs-TWAP companion.

## Position in lineage

The **crypto-specific empirical complement to [[foundation/sources/bouchaud-trades-quotes-prices]]**: Bouchaud et al. supply the theory (square-root law, transient impact, latent liquidity) calibrated on equities/futures — with the wiki's standing caveat (verifier 0-3) that the square-root law is NOT a plug-in crypto slippage primitive without crypto validation. This paper **is** that crypto validation, at institutional scale, on both spot and perps — including the finding that the plain square-root form needs modification (sigmoid adjustment) at low participation. Sibling of [[foundation/sources/maitrier-metaorder-reconstruction]], which shows how to measure the same quantities from *public* data.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order flow (execution cost = price of consuming liquidity).
- **Axis 2 — Driver**: capacity constraint (finite book depth; impact scales with participation rate).
- **Axis 3 — Crypto applicability**: **direct** — the model is crypto-native, calibrated on crypto spot + perp order flow.

## Key findings (public layer)

**VERIFIED = read on Talos's public pages. Everything not listed under "gated" below comes from the public insights page and the two public companion articles; the white paper itself has not been read.**

### Dataset (public, VERIFIED)

- Talos proprietary high-frequency trade + quote data, **Jun 2024 – Jul 2025**.
- **Top 60 spot AND perpetual contracts**; **50,000+ parent (meta) orders**; **~50M child orders**.
- Distinctive claim: the dataset "uniquely captures the complete lifecycle of long-duration orders," enabling impact estimation over the **full execution horizon** — "a feature rarely available in public datasets."

### The three-component decomposition (public companion page, VERIFIED)

| Component | What it is | Public functional form |
|---|---|---|
| **Spread cost** | Immediate cost of crossing the bid-ask spread; "independent of order size and duration" | `S_C = S × c₁` (S = spread, c₁ calibrated coefficient) |
| **Physical (temporary) impact** | Cost of consuming order-book liquidity | `I_P = c₂ × σ × √π × φₚ(π)` — modified square-root law in participation rate `π = Q/(V·T)`, scaled by intraday volatility σ, with a **sigmoid adjustment φₚ(π)** for edge cases |
| **Time risk** | Exposure to price movement over the execution horizon ("risk of missing the arrival price"); flagged as especially material in 24/7 crypto markets | formula **(gated — not yet extracted)** |

### Headline empirical findings (public companion page, REPORTED via companion-page extraction)

- **Low-participation correction**: "the square-root law consistently underestimat[es] actual slippage by approximately 4 basis points in the critical 0–0.5% participation rate range" — the sigmoid-adjusted model corrects this systematic underestimation in exactly the regime where many institutional orders execute. (Spread cost, which is size-independent, is the intuitive reason a size-scaling law alone under-prices small orders.)
- **Slippage magnitudes**: over 26% of institutional trades fall in the **0–5bp actual-slippage** range, with minimal model prediction error there — i.e., institutional-grade crypto execution on majors is a single-digit-bp problem, not a tens-of-bp problem.
- **Practical use**: separating physical impact from time risk enables execution-speed decisions (trade faster = more impact, less time risk; slower = the reverse); alpha/reversion views plug into the time-risk term. The VWAP-vs-TWAP companion applies the model qualitatively: "expected market impact is inversely proportional to available liquidity"; "a VWAP schedule is only as good as its volume forecast" — under unstable volume patterns TWAP's robustness wins.

### Gated — not yet extracted

- Exact coefficient values (c₁, c₂, c₃) and the calibration methodology.
- The sigmoid function's parameters and full functional form.
- The time-risk component's formula.
- Per-asset / spot-vs-perp / size-bucket result tables, residual analysis, figures.
- Any decomposition of temporary vs permanent impact, decay dynamics.

**Do not reconstruct these from the square-root literature — the whole point of the paper is that the plain form needed modification in crypto.** If the numbers matter for a decision, download the gated paper (registration) or wait for the journal version.

## Why it matters / honest scope

- **This is the calibration layer for the paper-fill→live-fill translation** in [[foundation/methodology/deployment-edge-threshold]]: the wiki's working slippage numbers (0.5–2bp roundtrip on majors, "working knowledge, not formally measured") now have an institutional-scale empirical anchor — and the anchor broadly *confirms* the single-digit-bp regime for majors at institutional size.
- **Retail-size implication the decomposition makes explicit**: spread cost is size- and duration-independent; physical impact scales with participation rate. At retail order sizes (participation ≈ 0), **spread + fees dominate and impact is ~nil** — which is exactly the wiki's existing working assumption, now with a decomposition that says *why*. The interesting retail lesson is the inverse one: naive square-root plug-ins mis-price small orders (the ~4bp underestimate) because they ignore the size-independent spread floor.
- **Honest scope**: practitioner research, **not peer-reviewed** (journal publication stated as forthcoming); single-firm proprietary dataset (Talos client flow — institutional, algo-executed, possibly unrepresentative of aggregate market flow); **the primary document is unread** — every quantitative claim above is from Talos's own public marketing/companion pages, extracted secondhand. Status stays `seedling` until the gated paper (or journal version) is actually ingested. Sample period (Jun 2024–Jul 2025) predates the Oct-2025 cascade / 2026-H1 bear — regime-conditional friction (cascade slippage) is out of sample.

## Related

- [[foundation/sources/bouchaud-trades-quotes-prices]] — the theory beneath this; the crypto-validation caveat this paper partially answers
- [[foundation/sources/maitrier-metaorder-reconstruction]] — the public-data route to measuring the same quantities in-house
- [[foundation/methodology/deployment-edge-threshold]] — the friction-floor discipline this calibrates (§2 slippage components map ~1:1 onto the Talos decomposition)
- [[foundation/methodology/forecast-vs-tradeable-gap]] — L3 (deployable) layer sibling
- [[foundation/market-structure/bid-ask-spread-decomposition]] — the spread-cost component's microstructure
- [[foundation/market-structure/liquidity]] — width + depth; the decomposition operationalizes both

## Sources

- [Talos insights page (paper landing, gated download)](https://www.talos.com/insights/an-empirical-model-of-market-impact-in-cryptocurrency-trading) — abstract + dataset description (Dec 23 2025).
- [Public companion explainer: "Understanding Market Impact in Crypto Trading: The Talos Model"](https://www.talos.com/insights/understanding-market-impact-in-crypto-trading-the-talos-model-for-estimating-execution-costs) — decomposition, formula skeletons, ~4bp / 26% findings.
- [VWAP or TWAP for Crypto Execution: A Market Impact Perspective](https://www.talos.com/insights/vwap-or-twap-for-crypto-execution-a-market-impact-perspective) — qualitative application.
- Talos also published a talk: "The Talos Market Impact Model for Institutional Crypto Trading — Presented by Eliad Hoch" (YouTube) — not yet watched; candidate for extracting gated-layer detail without registration.
