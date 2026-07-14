---
title: "Lorig — Optimal Control of the Ethena Yield-Bearing Stablecoin"
status: growing
created: 2026-07-14
updated: 2026-07-14
tags: [source, academic, crypto-native, ethereum, ethena, staking-basis, delta-neutral, funding-rate, stochastic-control, day-swing-applicable]
---

# Lorig — Optimal Control of the Ethena Yield-Bearing Stablecoin

> **TL;DR**: Theoretical (stochastic-control) paper modeling Ethena's delta-neutral USDe strategy — long staked ETH (stETH) + equal-notional short ETH perpetual futures. **Full text fetched and read (arxiv.org/html/2605.11263) — the model structure and solution below are VERIFIED against the paper.** Core mechanism: Ethena's own trading has **permanent price impact that compresses the basis it depends on for funding income** — buying stETH pushes spot up, shorting perps pushes the perp price down, and the combined effect erodes future carry. The paper solves for the optimal accumulation speed that balances immediate mark-to-market gains against this self-inflicted basis erosion, in both infinite-horizon and finite-horizon (with forced liquidation near expiry) settings. **Important scope note: this is a pure theory paper — it contains NO empirical data on Ethena's actual size, its share of ETH perpetual open interest, or realized yield numbers.** Any such figures (e.g., Ethena's OI share, realized APY) must come from other sources — see [[trading/mechanisms/delta-neutral-funding-harvest]] for the wiki's existing empirical anchors on that side. *Analysis, not advice.*

## Citation

**Lorig, M.** *Optimal Control of the Ethena Yield-Bearing Stablecoin*. arXiv:2605.11263 [q-fin.MF]. Submitted May 11, 2026. 19 pages, 2 figures.

- arXiv: https://arxiv.org/abs/2605.11263
- HTML (fetched): https://arxiv.org/html/2605.11263

## 3-Axis Tags

- **Economic mechanism**: Carry / yield differential (staking yield + funding-rate carry) combined with market-impact/execution theory
- **Behavioral/structural driver**: Capacity constraint — the protocol's own flow is large enough to move the market it's trying to harvest, creating a self-limiting feedback loop (distinct from an external capacity constraint; this one is self-inflicted by the strategy's own size)
- **Crypto applicability**: Direct (built specifically to model Ethena's live construction)

## The model (VERIFIED from fetched text)

### Setup

The protocol simultaneously (a) buys stETH and (b) opens an equal-notional short ETH perpetual position, at control rate γₜ (the instantaneous rate of position growth). Wealth accrues from three flows:

1. **Staking yield** on stETH, at rate r > 0
2. **Funding payments** from the short-perp leg, at rate q, when the basis is positive
3. **Mark-to-market P&L** from basis movements

### The basis-compression mechanism — the paper's central point {#basis-compression}

The paper defines the basis ℬₜ = perp price − spot price, and models it as controlled by the protocol's own trading:

> Buying stETH shifts the spot mid price upward **permanently** at rate μ₁γₜ; shorting the perpetual shifts the perpetual mid price downward **permanently** at rate μ₂γₜ (Section 2.2).

Both effects compress the basis, via combined coefficient μ = μ₁ + μ₂. The controlled basis dynamics:

```
dℬₜ = [ −κ(ℬₜ − m) − μγₜ ] dt + c dWₜ
```

where κ(ℬₜ − m) is ordinary mean-reversion toward a long-run mean m, and the added term **−μγₜ** is the protocol's own permanent-impact drag on the basis. **The mechanism, in plain terms**: the faster/larger Ethena accumulates, the more it permanently erodes the very basis spread that funds its future income — a self-destructive feedback the optimal control must internalize.

### Optimal control solution

**Infinite-horizon (Proposition 2)** — linear feedback in the state variables:

```
γ* = γ̄·ℬₜ + γₙ·γₜ + γ₀
```

- **γ̄** (basis-chasing term) is positive — accumulate faster when the basis is wide
- **γₙ** (inventory brake) is negative — slows accumulation as the existing position grows, i.e. the strategy self-limits its own size
- **γ₀** (baseline drift) is driven by the staking yield r and long-run basis mean m — a floor level of accumulation exists even from a zero position, because staking yield alone is a positive baseline return

All coefficients solve a hierarchical cascade of algebraic (Riccati-type) equations, valid under a stability condition κ* := ρ + 2κ − c² > 0.

**Finite-horizon (Proposition 6)** — time-dependent feedback governed by backward Riccati ODEs, with a **"liquidation urgency near expiry"** feature: as t → T, the position-growth coefficient forces rapid unwinding **regardless of the current basis level** — i.e., near a horizon/redemption date, the model prescribes closing the position even if the basis still looks attractive, purely because of remaining-time constraints.

### What the paper explicitly does NOT do

- **No empirical estimate of Ethena's ETH perpetual open-interest share.** The paper is silent on this; it is a pure control-theory exercise with generic parameters (μ₁, μ₂, κ, m, c, r, q), not calibrated to Ethena's actual balance sheet.
- **Does not model staking yield as endogenously setting a floor on funding rates.** The paper treats the funding rate q as an exogenous mean-reverting process around m — it does NOT derive q from staking yield availability, or model a causal "staking yield floor pushes basis/funding down" relationship. **This means the paper does not confirm (or deny) a mechanical claim that ETH's staking yield structurally caps ETH perp funding/basis below BTC's** — that claim, if made elsewhere in the wiki, needs an independent source. This paper only shows that the protocol's *own hedging activity* compresses the basis it harvests, which is a related but distinct mechanism from "staking yield sets a funding floor."

## Honest caveats

- **Purely theoretical** — no backtests, no calibration to historical Ethena data, no numbers on realized yield, TVL, or OI share. Do not cite this paper for any of those; see [[trading/mechanisms/delta-neutral-funding-harvest]] for the wiki's empirical anchors on Ethena's realized yield trajectory (18% APY 2024 → 3.72% Q1 2026, per that page's existing sourcing — not from this paper).
- **Single-instrument, single-protocol model** — γₜ is one aggregate position; no multi-venue (Binance/Bybit/OKX/Deribit) decomposition, no modeling of the Oct 2025 depeg-style venue/oracle failure mode documented elsewhere in the wiki.
- **Parameters are illustrative, not fitted** — μ₁, μ₂, κ, c etc. are free parameters in the exposition; the paper does not report fitted values from real Ethena trading data.

## Implications for wiki priorities

- Provides the **theoretical mechanism** (self-impact basis compression) that complements the wiki's existing **empirical** Ethena coverage in [[trading/mechanisms/delta-neutral-funding-harvest]] — that page has the yield/TVL/cascade empirics; this paper explains *why* a protocol at Ethena's scale would structurally see its own carry compress as it grows, independent of external funding-market conditions.
- For [[foundation/asset-classes/eth-market-structure]]: use this paper ONLY for the mechanism description (self-impact basis compression, liquidation-urgency-near-expiry), NOT for any OI-share or yield figure — those remain sourced from [[trading/mechanisms/delta-neutral-funding-harvest]] or flagged as open/unverified.

## Related

- [[trading/mechanisms/delta-neutral-funding-harvest]] — the wiki's empirical home for Ethena/USDe (yield trajectory, TVL, Oct 2025 cascade); this paper is the theoretical complement
- [[foundation/asset-classes/eth-market-structure]] — synthesizes this paper's mechanism into the ETH structural picture (staking-yield-floor discussion there is flagged per the scope note above)
- [[foundation/sources/he-manela-fundamentals-perpetual-futures]] — companion academic anchor on perp basis/arbitrage generally (empirical, not control-theoretic)
- [[foundation/sources/two-tiered-funding-rate-markets]] — cross-venue funding empirics; distinct lens (venue integration, not single-protocol impact)

## Sources

- arXiv:2605.11263 (v1, May 11 2026), fetched in full via https://arxiv.org/html/2605.11263
- Abstract page: https://arxiv.org/abs/2605.11263
