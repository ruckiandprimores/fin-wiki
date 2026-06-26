---
title: "Insurance Premium Harvest — Overview / Index"
status: seedling
created: 2026-05-18
updated: 2026-05-18
tags: [meta, mechanism-family, insurance-premium-harvest, vrp, delta-neutral-funding, comparison, index]
---

# Insurance Premium Harvest — Overview / Index

> **TL;DR**: In crypto, **two distinct insurance-premium harvest families coexist** and the wiki encodes them as separate mechanism pages: (a) **options-based variance risk premium (VRP) harvest** via delta-hedged short-vol — paid by options buyers (retail directional + protection-seekers); (b) **perp-based delta-neutral funding harvest** via spot-long + perp-short — paid by leveraged longs. Both are insurance-premium families, but the **counterparties, instruments, convexity profiles, and failure modes are structurally distinct**. This page is the single-screen comparison + cross-link index; depth lives on the two child pages.

## When to use this page

- You're evaluating whether a strategy proposal belongs to the VRP family, the funding-harvest family, or a different mechanism class entirely
- You're composing a portfolio that includes one or both insurance-premium mechanisms and want the cross-mechanism risk view
- You're deciding capability-investment priority (Deribit options data feed vs multi-venue perp execution)
- You need the wiki-level discipline statement on regime-decay non-transfer between premium families

## VRP vs Funding Distinction — single-screen comparison {#vrp-vs-funding-distinction}

| Dimension | VRP Harvest (options-based) | Delta-Neutral Funding Harvest (perp-based) |
|---|---|---|
| **Instrument** | Short strangle / straddle on Deribit options + delta-hedge | Short perp + long spot (or LST), equal notional |
| **Who pays the premium** | Options *buyers* — retail directional speculators (long calls in mania) + protection-seekers (long puts in fear) | Leveraged longs paying funding to shorts because they can't borrow spot cheaply / want cheap leverage |
| **Why they pay** | Loss aversion (Bakshi-Madan 2006), lottery preference, hedging demand for tail-risk insurance | Leverage demand / convenience / can't borrow spot at scale |
| **Convexity** | **Short gamma** — bounded upside, unbounded downside on tails | **Linear** — funding netted continuously; no convex blowup at mechanism level |
| **Hedging requirement** | Continuous delta-hedge (high-frequency, high-cost on gas/fees) | Discrete rebalance on size drift (lower cost) |
| **Capacity ceiling** | Limited by options OI (~$30B BTC + ETH combined on Deribit) | Larger — perp OI ~$100B+ across venues; Boros + Ethena haven't saturated |
| **Sharpe (literature)** | ~0.8 BTC delta-hedged short straddle (Almeida 2024 academic backtest) | 1.5-2.0 institutional cash-and-carry (varies by venue mix; Bybit / Investing.com 2025 data) |
| **2025-26 yield magnitude** | **~14% annualized VRP** (Almeida); LV cluster favorable (DVOL 37%) | 3.7% (sUSDe Q1 2026) → 6-11% (cross-exchange Boros) |
| **Failure mode (tail)** | **Short-gamma blowup** — vol spikes faster than delta-hedge adjusts | **Venue / oracle failure** — Oct 2025 USDe Binance $0.65 depeg; mechanism layer survived |
| **Falsifier (mechanism inversion)** | BVIX² < RV for >30 days (Almeida criterion) | Funding negative >14 days on majors |
| **Venue concentration risk** | Deribit ~85-90% market share (high) | Multi-venue accessible (Binance / Bybit / OKX / Hyperliquid) |
| **Lab capability status** | ❌ Capability gap — no Deribit feed | ⚠️ Partial — perp feed exists; multi-venue execution pending |
| **Substrate urgency (2026-05-18)** | Medium — research-substrate-ready | **High** — a team member's spot-hedge bot pending sync |
| **Wiki page** | [[variance-risk-premium-crypto]] | [[delta-neutral-funding-harvest]] |

## Both mechanisms share — the umbrella framing {#umbrella-framing}

Both VRP and delta-neutral funding harvest are:

1. **Insurance-premium families** — the buyer cohort pays a structural premium to the seller cohort because of risk-aversion / leverage-demand / hedging-demand mismatches
2. **Capacity-constrained edges** per a standing convention — neither has been arbed away despite being well-known to institutional vol funds and basis-trade desks
3. **Negative-skew payoffs** — both pay steady and lose big in tail events; risk-adjusted Sharpe targets need to be earned through regime conditioning, not assumed
4. **Day/swing-horizon compatible** — both have holding periods compatible with the wiki's ≤1-week target (per [[../foundation/the wiki architecture (ARCHITECTURE.md)|this research program §Trading style target]])
5. **Crypto-magnitude advantage over equity** — VRP is ~7× SPX; funding is no equity analog at all (crypto-native)

## Both mechanisms differ — the structural axes {#structural-axes}

1. **Counterparty cohort**: who is structurally on the other side
2. **Convexity profile**: short-gamma (VRP) vs linear (funding)
3. **Venue concentration**: Deribit-only (VRP) vs multi-venue-accessible (funding)
4. **Capability requirement**: options-Greek computation + delta-hedge execution (VRP) vs perp + spot rebalance (funding)
5. **Tail failure mode**: vol-spike blowup (VRP) vs venue/oracle failure (funding)
6. **Forward-looking signal**: DVOL term structure (VRP) vs Boros fixed APR (funding)

## Critical regime-decay non-transfer finding {#regime-decay-non-transfer}

**The single most important wiki-level discipline statement from this overview:**

[[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class#carry-trajectory|Borri et al. (2025)]] established that **broad crypto carry strategy Sharpe collapsed 6.45 → 4.06 → NEGATIVE in 2025**. This is a load-bearing wiki alarm for any crypto premium-harvesting mechanism.

**But VRP and delta-neutral funding harvest did NOT decay analogously** — because they have **different counterparty stacks** than broad-carry's "leveraged longs over-paying shorts at scale" frame. Specifically:

- **VRP** persists because options *buyers* (retail directional + protection-seekers) haven't been arbed away
- **Delta-neutral funding harvest** persists at compressed magnitude because the delta-neutral construction harvests funding even when directional carry inverts

**Generalizable wiki-level discipline** (N=1 worked example, watch for N=2): regime-decay findings on one premium-harvesting mechanism family do **NOT** automatically transfer to adjacent families. The discriminator is **counterparty-stack analysis** — same counterparty stack → similar decay sensitivity; different counterparty stack → independent regime trajectory.

This is also a sharpening of [[scaffold-as-mechanism]]'s caveat about per-mechanism-character matching — at a higher abstraction level, premium-harvest mechanisms are not fungible across counterparty stacks.

## Cross-portfolio composition implications {#portfolio-composition}

Because the two mechanisms have **different tail failure modes** (short-gamma blowup vs venue/oracle failure), they are **structurally diversifying** at the portfolio level:

- VRP tail event (vol-spike) does NOT typically coincide with funding-harvest tail event (venue/oracle failure)
- Oct 10-11 2025 cascade was an exception: BOTH triggered — VRP would have suffered short-gamma loss; USDe depegged on Binance. **But the mechanism layers both survived** — protocol-level Ethena profited as ETH dropped; Sinclair-style discipline would have paused VRP via BVIX² < RV check
- Combined exposure should NOT exceed the operator's tolerance for the worse of the two tail modes

**Operational rule**: when composing a portfolio with both, use [[../foundation/methodology/small-sample-validation-discipline#portfolio-vs-individual-fragility|small-sample-validation-discipline §portfolio-vs-individual-fragility]] aggregate-test rule: don't trim one mechanism for individual fragility metrics; test combined Sharpe / MDD / Calmar at trim variants.

## Forward-looking signals — what to watch {#forward-signals}

Both mechanisms have published forward-looking signals; tracking them is the entry/exit discipline:

| Mechanism | Signal | Current state | Implication |
|---|---|---|---|
| VRP | DVOL level + LV/HV cluster | DVOL 37% (Aug 2025) — LV cluster | **Favorable** for harvest |
| VRP | DVOL term structure shape | Pending fresh empirical check | Contango → carry positive |
| Funding | Boros fixed APR | BTC 11.4% (Oct 31 2025) → 6.42% (Nov 28 2025) | 44% compression / 1 month — carry decaying |
| Funding | Funding 14-day rolling mean | Pending empirical check | Negative >14 days → falsifier (a) |

## Anchor reading list {#anchor-reading}

For the trader / future research process sessions wanting to deep-dive:

| Source | Family | Why it's the anchor |
|---|---|---|
| **[[../foundation/sources/sinclair-volatility-trading]]** Ch 4 + 11 | VRP | Equity-options theoretical foundation; Sinclair-Almeida reconciliation pending |
| **Almeida, Grith, Miftachov (2024) arXiv 2410.15195** | VRP | Most rigorous academic crypto-VRP paper (sample 2017-2022, 7.8M Deribit options) |
| **Deribit Insights — "Finding Edge in Four Years of Vol Regimes"** | VRP | Best applied empirical practitioner piece |
| **[[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]]** | Funding | Broad-carry trajectory; load-bearing for "does NOT transfer" framing |
| **Stablecoin Insider Q1 2026 Ethena Report** | Funding | Productized version + Oct 2025 cascade behavior |
| **Pendle Boros docs + Medium cross-exchange writeup** | Funding | Yield-curve / forward-looking signal infrastructure |

## Open questions cross-cutting both mechanisms {#cross-cutting-questions}

1. **Does the Boros funding term structure correlate with DVOL term structure?** If yes, the two insurance-premium families share a forward-looking regime signal (huge operational implication for portfolio composition).
2. **Sinclair-Almeida regime reconciliation** — Sinclair's GARCH-grounded vol-regime framing and Almeida's LV/HV cluster framing both inform when carry is favorable. Are they describing the same regimes?
3. **N=2 confirmation of regime-decay non-transfer discipline** — need another adverse-event window where one premium family decays and the other doesn't, to promote the principle from N=1 worked example to standard wiki discipline.
4. **Skew-decomposition flip threshold** ([[skew-based-positioning-signals]]) — at what point does bull-mania skew flip signal that VRP harvest tail risk is extreme? Composes with funding-harvest signal (if funding also overheated → BOTH families overheated → reduce composite exposure).
5. **Capacity ceiling for combined deployment** — what's the joint scale where one or both mechanisms saturate? Boros $6.9B OI is ~11% of perp baseline; Deribit options $30B; combined deployment ceiling unknown.

## Honest caveats

- **Both pages are status `seedling`** — landed 2026-05-18 PM-6, no first backtest yet. Watch for N=1 → N=2 evidence accumulation through lab testing before upgrading to `growing`.
- **VRP page has capability gap** — lab cannot test until Deribit feed lands
- **Delta-neutral funding harvest** is the substrate-urgent path (a team member spot-hedge bot pending sync) — empirical validation will come from there first
- **The "does NOT decay analogously" finding is N=1** — Borri broad-carry 2025-negative is the single regime that distinguishes broad-carry from VRP / delta-neutral funding. Need next major regime stress to confirm the non-transfer holds
- **This overview is meta-content** — depth lives on the two child pages; cross-mechanism reasoning lives here

## Operationalization status

| Component | Wiki encoded | Lab implemented |
|---|---|---|
| Comparison framework | ✅ this page | — |
| Cross-mechanism reasoning | ✅ this page | — |
| Regime-decay non-transfer discipline | ✅ §regime-decay-non-transfer | — |
| Portfolio composition rule | ✅ §portfolio-composition | ❌ no combined deployment yet |
| Forward-looking signals | ✅ §forward-signals | ⚠️ partial (Boros + DVOL feeds pending) |

## Related

### Children (depth lives here)
- [[variance-risk-premium-crypto]] — options-based VRP harvest mechanism
- [[delta-neutral-funding-harvest]] — perp-based delta-neutral funding harvest mechanism

### Sister mechanism family pages
- [[funding-cycle-dynamics]] — family-level umbrella for funding mechanisms (delta-neutral is a new operational form alongside Templates 1-3)
- [[etf-era-carry-trade-funding-distortion]] — environment-level for funding harvest
- [[vol-regime-detection]] — regime classifier for VRP
- [[skew-based-positioning-signals]] — skew decomposition for VRP variant selection
- [[options-gamma-perp-effects]] — adjacent options mechanism (gamma flow into perps)
- [[term-structure-dynamics]] — IV term structure for VRP

### Methodology
- [[../foundation/methodology/small-sample-validation-discipline#portfolio-vs-individual-fragility]] — portfolio composition discipline
- [[../foundation/methodology/deployment-edge-threshold]] — net-edge floor + opportunity-cost gate
- [[../foundation/methodology/path-quality-diagnostics]] — path-quality applies to both families (especially short-gamma blowup events)

### Sources
- [[../foundation/sources/sinclair-volatility-trading]] — equity-options theoretical foundation
- [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]] — broad-carry trajectory
- [[../foundation/sources/two-tiered-funding-rate-markets]] — cross-venue funding spread empirical anchor
- [[../foundation/sources/temporal-dynamics-funding-rates]] — funding rate rolling-window dynamics

### Glossary
- [[../glossary/variance-premium]] — general concept (companion)
- [[../glossary/vol-cone]], [[../glossary/iv-term-structure]], [[../glossary/vol-skew]] — VRP-side companions
- [[../glossary/funding-rate-carry]], [[../glossary/basis-trade-crypto]] — funding-side companions
- [[../glossary/deflated-sharpe-ratio]] — validation discipline

## Sources

This page consolidates anchors from the two child pages — see [[variance-risk-premium-crypto#sources]] and [[delta-neutral-funding-harvest#sources]] for full citations.

Key cross-mechanism deliverables:
- 2026-05-18 research-analyst output (substrate behind this page) — produced under VRP+spot-hedge research thread per this research program spot-hedge bot reframe (2026-05-15)
