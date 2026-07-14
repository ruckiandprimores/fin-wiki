---
title: "Trades, Quotes and Prices (Bouchaud, Bonart, Donier & Gould 2018)"
status: growing
created: 2026-06-26
updated: 2026-07-12
tags: [source, academic, market-microstructure, limit-order-book, market-impact, square-root-law, liquidity, execution, methodology, foundation]
---

# Trades, Quotes and Prices (Bouchaud, Bonart, Donier & Gould 2018)

> **TL;DR**: The foundational empirical/statistical-physics reference on market microstructure — builds **bottom-up from individual order arrivals to macro-scale price statistics**. Core results: order-flow signs are *long-memory* (highly persistent) yet prices stay ~diffusive, which forces market impact to be **transient** (the propagator model); metaorder impact follows the near-universal **square-root law**; and the visible order book is a sliver of **latent liquidity**. It is the canonical reference *beneath* the crypto-LOB preprints — fills the wiki's self-flagged microstructure/methodology gap. **Critical caveat: do NOT codify the square-root impact law as a plug-in crypto slippage primitive without validation** (the verifier killed that transfer 0-3).

## Citation

Jean-Philippe Bouchaud, Julius Bonart, Jonathan Donier, Martin Gould, *Trades, Quotes and Prices: Financial Markets Under the Microscope*, **Cambridge University Press**, 2018. ISBN 9781107156050.

## Position in lineage

The microstructure-theory counterpart to the already-ingested practitioner reference [[harris-trading-exchanges|Harris, *Trading and Exchanges*]] and the methodology of [[lopez-de-prado-advances-fin-ml|López de Prado]]. It is the foundation **beneath** the crypto-LOB preprints surfaced in the 2026-06-26 ingest-queue research (Wang LOB-inputs, Bieganowski-Ślepaczuk OFI, Le funding-aware market-making) — those build on its primitives. Fills the self-flagged LOB/microstructure foundation gap.

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order flow + liquidity provision (the cost of demanding vs supplying liquidity).
- **Axis 2 — Driver**: capacity constraint + forced flow (impact = the price you pay to consume finite liquidity) + information asymmetry (adverse selection in market-making).
- **Axis 3 — Crypto applicability**: **mutation / transfer** — calibrated on equities/futures; the mechanisms are candidate-transferable to crypto LOBs but require crypto-specific validation (see caveat).

## Key concepts

- **Stylized facts**: fat-tailed returns, volatility clustering, and — load-bearing — **long-range autocorrelation of order-flow signs** (market-order direction is predictable far into the future, power-law decaying).
- **The diffusivity puzzle + transient impact**: persistent order flow + near-martingale prices can only coexist if **impact is transient (mean-reverting)** — formalized as the **propagator model**. Liquidity dynamically offsets the predictable component, so predictable flow does *not* create predictable price trends.
- **The square-root law of metaorder impact**: impact of a metaorder of size *Q* ≈ *Y·σ·√(Q/V)* — grows as the **square root of participation rate**, remarkably universal across markets/eras. Mostly transient, small permanent component.
- **Latent liquidity**: the visible book is a tiny fraction of true supply/demand; the Donier-Bouchaud **latent order book** (locally-linear around price) explains the square-root law.
- **LOB dynamics**: order arrival / cancellation / queue statistics; book shape.
- **Optimal execution**: impact + risk minimization (Almgren-Chriss extended for transient impact).
- **Market making & adverse selection**: the dealer's inventory problem; the spread as compensation for trading against informed flow.

## Why it matters / honest scope

This is the reference for **how price impact and liquidity actually work** — directly relevant to the wiki's [[../methodology/deployment-edge-threshold|deployment-edge / friction-estimation]] discipline (impact is the dominant cost at scale) and to any order-flow read ([[../market-wisdom/tape-reading|tape-reading]], [[../../trading/mechanisms/volume-analysis|volume analysis]]). It also gives the *mechanism* (order-flow long memory + transient impact) that the crypto-LOB ML preprints only measure.

**Critical caveat (verifier-flagged, 0-3 refute):** the square-root impact law is **not** a directly-transferable, plug-in crypto slippage primitive. The functional form is broadly universal, but the constant *Y*, the volume normalization *V* (fragmented across crypto venues + fake-volume-contaminated — see [[cong-crypto-wash-trading|wash trading]]), and even the exponent need crypto-specific validation. Ingest as **foundational microstructure reference**, not as a slippage formula to wire into the friction model. Honest grade: book-overview / research-verified; full-read deep-dive optional.

## Related

- [[harris-trading-exchanges]] — practitioner-side microstructure companion
- [[lopez-de-prado-advances-fin-ml]] — ML/validation methodology sibling
- [[../market-structure/liquidity]] — Harris's four liquidity dimensions; impact is the depth cost
- [[../market-structure/bid-ask-spread-decomposition]] — the spread's adverse-selection component
- [[../market-structure/dealer-economics]] — the market-maker's problem this formalizes
- [[../methodology/deployment-edge-threshold]] — friction/slippage estimation (square-root impact is the *reference*, not a transferable constant)
- [[maitrier-metaorder-reconstruction]] — same group, 2025: metaorders reconstructed from *public* tape reproduce the square-root law (Y ≈ 0.5) + Bucci decay → impact is mechanical, not informational; the public-data route to the crypto validation this page's caveat demands
- [[cong-crypto-wash-trading]] — why the volume normalization *V* is unreliable in crypto

## Sources

- Bouchaud, Bonart, Donier & Gould, *Trades, Quotes and Prices* (Cambridge UP, 2018), ISBN 9781107156050.
- Verified via deep-research adversarial pass (2026-06-26): existence + topic coverage (price impact, optimal execution, liquidity fragility, order-flow regularities) confirmed; the square-root-law-as-transferable-crypto-slippage-primitive claim **refuted 0-3** and flagged on-page.
