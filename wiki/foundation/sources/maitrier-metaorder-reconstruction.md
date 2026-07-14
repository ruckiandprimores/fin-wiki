---
title: "Generating Realistic Metaorders from Public Data (Maitrier, Loeper & Bouchaud 2025)"
status: growing
created: 2026-07-12
updated: 2026-07-12
tags: [source, academic, preprint, market-impact, market-microstructure, square-root-law, execution, slippage, metaorder, methodology, day-swing-applicable]
---

# Generating Realistic Metaorders from Public Data (Maitrier, Loeper & Bouchaud 2025)

> **TL;DR**: A method for **reconstructing metaorders (parent orders) from the public trade tape alone** — no proprietary execution data needed. Synthetic trader IDs are probabilistically assigned to anonymous trades; same-sign trade runs per synthetic trader define metaorders. The striking result: these *synthetic* metaorders reproduce the full empirical impact phenomenology — **square-root law (Y ≈ 0.5) over four decades of size, concave execution profiles (√φ), and Bucci-style post-execution decay (β ≈ 0.2 vs 0.22 on real proprietary metaorders)**. Since a synthetic metaorder carries no trader intention by construction, **short-term impact must be mechanical, not informational** — supporting latent-liquidity theory over Kyle-type information models, and explaining the square-root law's universality. Practically: this is a recipe for **measuring impact in-house from public Binance/Bybit tapes** instead of trusting equity-calibrated constants.

## Citation

Guillaume Maitrier, Grégoire Loeper, Jean-Philippe Bouchaud, *Generating realistic metaorders from public data*, arXiv:2503.18199 [q-fin.TR], submitted Mar 23 2025 (v2 Apr 5 2025). Full text free on arXiv. Ingested from the full arXiv HTML 2026-07-12 (VERIFIED — full text read).

## Position in lineage

Direct extension of the [[foundation/sources/bouchaud-trades-quotes-prices]] research program (same group — Bouchaud is a co-author): the book establishes square-root impact / latent liquidity from proprietary metaorder datasets (CFM, Ancerno…); this paper shows the same phenomenology is recoverable from **public data**, and pushes the interpretation further ("mechanical, not informational"). It also **operationally unblocks** the wiki's standing caveat that the square-root law's constant Y and volume normalization V need crypto-specific validation — this is the method to do that validation without proprietary data. Sibling of [[foundation/sources/talos-crypto-market-impact]] (proprietary-data crypto calibration by a practitioner).

## 3-Axis Tags

- **Axis 1 — Economic mechanism**: microstructure / order flow (metaorder impact; liquidity consumption).
- **Axis 2 — Driver**: capacity constraint (latent liquidity locally linear around price → √ impact) + coordination (order splitting).
- **Axis 3 — Crypto applicability**: **mutation needed** — validated on EuroStoxx futures + French equities; the method is data-generic (needs only timestamped signed trades) but crypto replication has to be run, not assumed.

## The reconstruction algorithm (VERIFIED)

1. Load the public trade tape: **timestamps, quantities, prices, buy/sell (aggressor) side**. Lit-market trades from a **single exchange** (their recommendation); strip opening/closing auctions.
2. Compute per-day totals: daily volume `V_D`, intraday volatility `σ_D`.
3. **Mapping function** — assign a synthetic trader ID to every trade, preserving true chronological order. Only **two degrees of freedom**: (a) the number of distinct traders per day, (b) the distribution of trading frequency across them (homogeneous or power-law). Assignment is probabilistic, sampling without replacement; deliberately agnostic to true identities.
4. Sort by (trader, timestamp). A **metaorder = a maximal run of same-sign trades from the same synthetic trader**.
5. Extract per-metaorder features: start/end log-prices, child-order count, total volume Q → measure impact `I(Q)` vs `Q/V_D`, execution profiles, post-execution paths.

Robustness claim (verbatim): under "random variations in the matching between real traders and orders, the square impact law is preserved, including its prefactor Y."

## Validation results (VERIFIED)

- **Datasets**: EuroStoxx futures (Eurex, Sep 2016–Aug 2018); seven French equities (Euronext Paris, Jan 2021–Dec 2023); BNP Paribas 2020–2023 case study.
- **Square-root law**: `I(Q)/σ_D = Y·√(Q/V_D)` — "remarkably clean square root law over four decades" on EuroStoxx with **Y ≈ 0.5**; French stocks show "almost no variation in the prefactor (Y ≈ 0.5)" — matching prior proprietary-data literature. Noisy below `Q/V_D ≲ 5×10⁻⁶`.
- **Concave execution profile**: impact during execution follows `ℐ(φQ) = √φ · I(Q)` (φ = fraction executed) — consistent with a locally-linear latent order book.
- **Post-execution decay**: propagator form `ℐ(Q,z) = I(Q)·[z^(1−β) − (z−1)^(1−β)]`, z = t/T ≥ 1, with **β ≈ 0.2** — "exactly as found by Bucci et al. using real metaorders" (β = 0.22). Sharp initial decay, very slow tail, "perhaps a small but non-zero permanent component."

## The "mechanical, not informational" conclusion (VERIFIED)

Verbatim core: "By construction, a synthetic metaorder has no connection to a trader's intention to trade based on a predictive signal. Yet, its impact is indistinguishable from that of a real metaorder." Therefore "average realized short-term price impact is not due to information revelation (as in the Kyle framework) but has a mechanical origin which could explain the universality of the Square Root Law." Favors latent-liquidity / latent-LOB theories over information-based models.

**Trading-relevant corollary**: the price move any executed flow produces is mostly *liquidity consumption*, not *information being priced in* — and it substantially **reverts after execution** (decay to a small permanent component). Reading impact as "informed flow → follow it" is, at metaorder horizon, largely wrong; this is mechanism-level support for fade-the-flow over follow-the-flow at short horizons.

## Stated limitations (VERIFIED)

- Synthetic metaorders are **short** (avg 2–5 min) vs proprietary datasets' day-long metaorders (though consistent with Tokyo Stock Exchange ID-tagged data).
- Illiquid assets may need parameter fine-tuning of the mapping function to recover the square-root law.
- Child-order inter-trade-time decay exponent comes out μ ≈ 3.5 vs theory's 1 ≤ μ ≤ 2 — a known mismatch.
- "The theoretical justification for the success of our procedure is not yet completely understood" (future paper).

## Replication path — measuring impact on Binance/Bybit public tapes in-house

What the algorithm needs vs what crypto public data provides:

| Requirement | Binance/Bybit public data | Status |
|---|---|---|
| Timestamped trades with size + price | `aggTrades` / trade streams + full historical dumps (Binance Vision) | ✅ available, free, ms timestamps |
| **Aggressor side** (buyer- vs seller-initiated) | `isBuyerMaker` flag on Binance aggTrades; Bybit trade feed has side | ✅ available — *better* than equities, where side must be inferred (Lee-Ready) |
| Trader IDs | Not provided (as everywhere) | ➖ not needed — the whole point is synthetic assignment |
| Single-venue recommendation | Trivially satisfied (run per-venue, e.g. Binance perp only) | ✅ |
| Auction-period stripping | No auctions in 24/7 crypto; instead strip known cascade windows / funding timestamps if desired | ⚠️ judgment call |
| Daily volume V_D, intraday vol σ_D | Computable from same tape | ✅ (wash-trading contamination of V on lower-tier venues is the known hazard — [[foundation/sources/cong-crypto-wash-trading]]; top-venue BTC/ETH perps are the clean starting point) |
| Mapping-function parameters | #traders/day + frequency distribution — must be chosen/tuned for crypto | ⚠️ the actual research work |

**Verdict: fully replicable on public crypto data** — no gated dataset required. The tape (with aggressor flags) is public and archived; the algorithm is described completely; compute is modest. The open research questions are crypto-specific: does Y ≈ 0.5 hold on BTC/ETH perps? does the sub-0.5%-participation deviation Talos reports show up? is the mapping function stable across vol regimes? A BTCUSDT-perp replication would convert the wiki's "square-root law is not a plug-in crypto primitive" caveat from a blocker into a measured answer, and would give the team's backtesting lab a venue-specific impact curve for the friction model. Expect the illiquid-asset caveat to bite on alts.

## Why it matters / honest scope

- **Closes the proprietary-data moat on impact measurement** — the practical obstacle to crypto-calibrating [[foundation/methodology/deployment-edge-threshold]]'s impact term was never math, it was data; this removes it.
- **Honest scope**: arXiv **preprint, not peer-reviewed**; validated on equities/futures only — the crypto transfer is *plausible-by-design* (data requirements met) but **unproven until run**; the authors themselves flag that the theoretical grounding of why synthetic assignment works is open; impact numbers at retail size remain ~nil regardless (spread + fees dominate — see the Talos decomposition).

## Related

- [[foundation/sources/bouchaud-trades-quotes-prices]] — parent lineage; theory this paper operationalizes and extends
- [[foundation/sources/talos-crypto-market-impact]] — proprietary-data crypto impact calibration; the two are complementary routes to the same number
- [[foundation/methodology/deployment-edge-threshold]] — the friction floor whose impact term this can calibrate
- [[foundation/methodology/forecast-vs-tradeable-gap]] — translation-loss sibling discipline
- [[foundation/sources/cong-crypto-wash-trading]] — why V_D needs venue hygiene in crypto
- [[foundation/market-structure/liquidity]] — depth dimension this quantifies

## Sources

- [arXiv:2503.18199](https://arxiv.org/abs/2503.18199) — abstract; [full HTML v2](https://arxiv.org/html/2503.18199v2) read 2026-07-12 (algorithm, datasets, Y ≈ 0.5, β ≈ 0.2, limitations all VERIFIED from full text).
- Bucci et al. post-execution decay (β = 0.22) — cited within the paper as the proprietary-data benchmark the synthetic method matches.
