---
title: "Zhivkov, Todorov, Georgiev — Temporal Dynamics of Market Microstructure in Cryptocurrency Perpetual Futures (IJFS 2026)"
status: growing
created: 2026-05-15
updated: 2026-05-15
tags: [source, academic, crypto-native, funding-rate, cross-venue, cex-dex, rolling-window, granger-causality, garch, structural-break, intraday-patterns, day-swing-applicable]
---

# Zhivkov, Todorov, Georgiev — Temporal Dynamics of Market Microstructure in Cryptocurrency Perpetual Futures

> **TL;DR**: Rolling-window econometric extension of the companion [[foundation/sources/two-tiered-funding-rate-markets|Two-Tiered]] paper from the same lead author. Published April 2026 in MDPI *International Journal of Financial Studies* 14(5):103. Sample: **14 November 2025 – 13 January 2026** (60 usable days); **9.1 million hourly observations** across **26 exchanges** (11 CEX + 15 DEX) and **812 symbols**; **53 overlapping 7-day rolling windows**. **Five major findings** that materially refine the companion paper: (1) **Two-tier structure persists** (52 of 53 windows positive) but **varies substantially in magnitude** — integration gap ranges from **−0.041 to 0.222**; (2) **gap WIDENS over the sample** (0.069 in mid-Nov → 0.143 in late-Dec/Jan), driven primarily by DEX-DEX correlation decline (0.170 → 0.064); (3) **IGARCH finding from companion is EPISODIC, not structural** — BTC GARCH persistence averages 0.815 across windows with near-IGARCH (>0.95) appearing in only **24.5% of windows** (13 of 53), clustered in late November + late December/early January; (4) **Granger causality challenges size-based hierarchy** — **Bybit Granger-causes Binance in 62% of windows vs reverse 21%**; OKX→Binance 55% vs reverse 25%; even HYPERLIQUID Granger-causes Binance in 25% of windows (vs reverse 13%). Mid-tier exchanges and DEXes can lead the largest CEX; (5) **Intraday pattern linked to settlement times** — spreads peak ~2 hours after 8h settlement times (02:00, 10:00, 18:00 UTC); peak-trough magnitude 2.5 bps. **Structural break tests detect ZERO discrete breaks** (Bai-Perron) — market evolves through gradual drift; CUSUM confirms parameter instability driven by trend, not discontinuity. **Wiki-relevance**: extends the cross-venue funding-rate empirical cluster; nuances both the IGARCH risk-model implication AND the CEX-leadership simplification; provides temporal stability evidence for cross-venue spread strategies; documents settlement-time intraday patterns useful for execution-timing in funding-carry strategies. Status `growing` — second wiki-encoded paper on cross-venue crypto funding-rate market structure; first wiki paper on rolling-window temporal-stability of these dynamics.

## Citation

**Zhivkov, P., Todorov, V., Georgiev, S.** (2026). *Temporal Dynamics of Market Microstructure in Cryptocurrency Perpetual Futures: Econometric Evidence from Centralized and Decentralized Exchanges*. *Int. J. Financial Stud.* 14(5):103. https://doi.org/10.3390/ijfs14050103

Author affiliations:

- Petar Zhivkov — Institute of Information and Communication Technologies, Bulgarian Academy of Sciences (BAS); Angel Kanchev University of Ruse; Centre of Excellence in Informatics and ICT
- Venelin Todorov — same primary affiliation cluster
- Slavi Georgiev — Department of Informational Modeling, Institute of Mathematics and Informatics, BAS; Department of Applied Mathematics and Statistics, Angel Kanchev University of Ruse

Published Open Access (CC BY); funded by EU NextGenerationEU through Bulgarian National Recovery and Resilience Plan + Bulgarian National Science Fund.

## 3-Axis Tags

- **Economic mechanism**: Cross-venue arbitrage; price discovery; basis-trade infrastructure (rolling-window dimension)
- **Behavioral/structural driver**: Liquidity-fragmentation evolution; settlement-mechanism-driven intraday patterns; size-doesn't-determine-leadership
- **Crypto applicability**: Direct (crypto-only)

## Key empirical findings

### 1. Sample window + universe — 2-month rolling-window extension

| Dimension | Value |
|---|---:|
| Sample period | **14 November 2025 – 13 January 2026** |
| Calendar days | 61 (60 usable; 22 Nov excluded) |
| Full-coverage days | 56 |
| Partial-coverage days | 4 |
| Raw 1-min observations | 543,195,787 |
| Hourly observations after aggregation | **9,119,614** |
| Unique symbols | **812** (vs 749 in companion paper) |
| Exchanges | 26 (11 CEX + 15 DEX); 25 from Jan 3 onward when one DEX ceased reporting |
| CEX observations share | **77.7%** (7,084,211 hourly obs) |
| DEX observations share | 22.3% (2,035,403 hourly obs) |
| Rolling windows | **53 overlapping 7-day windows** advancing by 1 day |

Cross-exchange spread observations: 994,088 after 5000bps outlier cap. Breakdown by pair type: **CEX-CEX 68.8%, CEX-DEX 27.1%, DEX-DEX 4.0%**.

### 2. Integration gap evolution — persistent but variable {#integration-gap-evolution}

**Headline finding**: integration gap positive in **52 of 53 windows** but ranges **−0.041 to +0.222** (5× variation).

Summary statistics across rolling windows (Table 2):

| Metric | Mean | Std | Min | Max |
|---|---:|---:|---:|---:|
| CEX-CEX correlation | **0.226** | 0.048 | 0.149 | 0.355 |
| DEX-DEX correlation | **0.117** | 0.049 | 0.042 | 0.212 |
| CEX-DEX correlation | 0.132 | 0.037 | 0.075 | 0.239 |
| **Integration gap** Δ_int | **0.110** | 0.056 | **−0.041** | **0.222** |
| Arbitrage prevalence (%) | 18.8% | 1.3 | 15.8% | 20.9% |
| Mean spread (bps) | 15.4 | 0.7 | 14.6 | 16.5 |

**Single reversal — window 21 (6-12 December)**: DEX-DEX correlation (0.212) briefly exceeded CEX-CEX (0.172), producing the only negative integration gap. Reversal short-lived; gap returned to positive territory next window.

**Wiki-relevance**: a researcher sampling **the same market 3 weeks apart** would have reached qualitatively opposite conclusions on whether DEX is more or less integrated than CEX. Any static-snapshot framework underestimates this variance.

### 3. Temporal trend — gap WIDENS over the sample {#gap-widens}

**First 17 windows (Nov 15 – early Dec)**: Mean integration gap = **0.069**
**Last 17 windows (Late Dec – mid-Jan)**: Mean integration gap = **0.143**

Decomposition:

| | First 17 windows | Last 17 windows | Δ |
|---|---:|---:|---:|
| CEX-CEX mean corr | 0.265 | 0.225 | **−0.040** |
| DEX-DEX mean corr | 0.170 | 0.064 | **−0.106** |
| Integration gap | 0.095 | 0.161 | **+0.066** |

**Gap widening is driven primarily by DEX-DEX decline** (correlation collapsed from 0.170 to 0.064), NOT by CEX-CEX rise. **DEX integration is DETERIORATING** over the sample — venues becoming less synchronized with each other.

**Concurrent**: arbitrage prevalence rises **16-17% in November → 19-21% in late December and early January**. More fragmentation = more arbitrage opportunities.

**Wiki interpretation**: this is **early empirical evidence that DEX perp market integration is regressing** — potentially due to DEX expansion (new venues like Aster, Hibachi, Ethereal launching), creating fragmentation at the venue-population level even as aggregate DEX volume grows. Calibration warning for any **multi-DEX strategy assuming single DEX-tier behavior**: the within-DEX correlation is structurally low AND declining.

### 4. Structural break tests — gradual drift, NOT regime switching

Two complementary tests:

| Test | Result | Interpretation |
|---|---|---|
| **CUSUM** (Brown et al. 1975) | **Rejects stability** at 5% (max stat 7.27 vs critical 6.90) | Parameter unstable; CUSUM path crosses bands early and stays out |
| **Bai-Perron** (1998, 2003) multiple break test | **Zero significant breaks** at all penalty levels λ ∈ {1, 2, 5, 10} | No discrete regime shifts |

**Combined verdict**: market integration evolves through **gradual drift**, not abrupt regime changes. **Rules out the hypothesis** that market integration shifts abruptly in response to isolated events (exchange listings, regulatory announcements, liquidity crises).

When Bai-Perron is **forced** to identify exactly 2 breaks, estimated dates are window 15 (30 November) and window 30 (15 December) — these divide the sample into approximate thirds with mean integration gaps 0.096 / 0.058 / 0.152. Middle period coincides with the brief Dec 6-12 convergence; final period captures the widening.

**Wiki-relevance**: this is **methodology-relevant** for [[foundation/methodology/path-quality-diagnostics#split-timing|path-quality-diagnostics §2.3 split-timing scrutiny]] — when the underlying market evolves through gradual drift rather than discrete breaks, train/val splits on different fraction-of-period landmarks don't fall on regime boundaries because there ARE no boundaries. The split-timing-artifact pattern (meta_post_cascade reversal worked example) only triggers on markets with episodic regime structure; the rolling-window gradual-drift pattern is a DIFFERENT regime structure where split-timing artifacts wouldn't arise.

### 5. IGARCH walk-back — the companion paper's finding is EPISODIC, not structural {#igarch-walk-back}

The [[foundation/sources/two-tiered-funding-rate-markets|companion Two-Tiered paper]] found GARCH(1,1) α + β ≈ 1.0 for BTC over its 8-day sample — **IGARCH behavior implies permanent volatility shocks**, which would fundamentally undermine convergence-arbitrage viability.

**This extension paper walks the finding back**: across **53 rolling 7-day windows**:

- **BTC mean persistence: 0.815** (std 0.160)
- **Near-IGARCH (α+β > 0.95) appears in 13 of 53 windows = 24.5%**
- IGARCH-prone clusters concentrate in TWO specific periods: late November (windows 3-4, Nov 18-25) + late Dec/early Jan (windows 44-51, Dec 29 - Jan 11)
- In the remaining 75% of windows, persistence ranges 0.407 to 0.930 — substantially below the IGARCH threshold

**ETH GARCH persistence**: ranges from near-zero to 1.0; mean ~0.77; greater variability than BTC
**SOL GARCH persistence**: averages ~0.86; relatively consistent

**Stationarity** (ADF test):
- **BTC funding-rate spreads stationary in 52 of 53 windows** (one borderline exception in window 36)
- ETH: 7 windows fail to reject unit root (concentrated late Nov - early Dec)
- SOL: stationary in 51 of 53 windows

**Wiki-relevance — major risk-model implication**:

| Setting | Risk model | Implication |
|---|---|---|
| Always-IGARCH (companion paper static finding) | Volatility shocks permanent; spread variance grows without bound | Convergence arbitrage non-viable; spread risk overestimated |
| Episodic IGARCH (this paper's rolling finding) | Volatility behaves normally 75% of time; permanent-shock pattern only in specific clusters | Convergence arbitrage viable in normal regime; risk models need regime-conditional persistence parameters |

**The companion paper's IGARCH finding was a sampling artifact** — 8-day samples happen to fall in IGARCH-prone clusters by chance with non-trivial probability. Risk models calibrated to the static IGARCH finding would systematically overestimate spread risk in 75% of market conditions.

### 6. Granger causality challenges size-based price-discovery hierarchy {#granger-size-paradox}

The companion paper found **CEX → DEX directional causality**. This extension paper finds **the within-CEX hierarchy is INVERTED relative to exchange size**:

| Pair direction | Significant windows / 53 | Fraction |
|---|---:|---:|
| **BYBIT → BINANCE** | **33/53** | **62%** |
| BINANCE → BYBIT | 11/53 | 21% |
| **OKX → BINANCE** | **29/53** | **55%** |
| BINANCE → OKX | 13/53 | 25% |
| KUCOIN → MEXC | 18/53 | 34% |
| MEXC → KUCOIN | 14/53 | 26% |
| **HYPERLIQUID → BINANCE** (DEX→CEX!) | **13/53** | **25%** |
| BINANCE → HYPERLIQUID | 7/53 | 13% |
| DRIFT → HYPERLIQUID (DEX→DEX) | 19/53 | 36% |
| HYPERLIQUID → DRIFT | 16/53 | 30% |

**Bybit Granger-causes Binance ~3× more often than the reverse** despite Binance being the larger-volume venue.

**Even more counterintuitive**: **HYPERLIQUID Granger-causes BINANCE in 25% of windows** — DEX leading the largest CEX in a non-trivial fraction of time. This **partially contradicts** the companion paper's "zero DEX→CEX causality" framing (which was at the per-pair level on the 8-day snapshot).

**Author interpretation** (Section 5.2):

> "Information discovery in perpetual futures funding rates does not follow a simple hierarchy in which the largest exchange leads."

Possible explanation: exchanges that attract **directional / speculative traders** (Bybit, OKX, HYPERLIQUID) generate funding-rate signals that propagate to **higher-volume venues where hedging dominates** (Binance). Compositional, not size-based.

**Wiki implication for cross-venue strategies + capability investment** (per [[trading/mechanisms/etf-era-carry-trade-funding-distortion#discrimination-signals-capability-gap|etf-era page]]):

- **Multi-venue funding-rate feed cannot focus on Binance alone**. Information flows FROM Bybit/OKX/Hyperliquid TO Binance, not the reverse, in majority of windows. **For early-warning signals on funding-rate regime shifts, monitor Bybit + OKX + HYPERLIQUID more closely than Binance**.
- This **partially answers** the etf-era page's refined open question on within-DEX heterogeneity: HYPERLIQUID specifically (not DEX-aggregate) shows non-trivial price-discovery role; Hyperliquid's order-book design + Solana settlement may matter
- The size-doesn't-determine-leadership pattern is methodology-relevant for the wiki's a standing convention discipline — capability investment should target informativeness, not just volume

### 7. Intraday spread patterns linked to settlement times {#intraday-settlement-pattern}

F-test for equality of hourly mean spreads: **F = 8.95, p < 10⁻³⁰** (highly significant).

**Peak hour**: **02:00 UTC** — 16.85 bps mean spread; 21.4% arbitrage prevalence (Asian session)
**Trough hour**: **17:00 UTC** — 14.35 bps; 16.5% arbitrage prevalence (early North American session)
**Peak-trough magnitude**: ~2.5 bps (17% of trough value)

**Mechanism — settlement-time-driven divergence**:

Exchanges with 8h settlement intervals process payments at **00:00, 08:00, 16:00 UTC**. Spreads spike approximately **2 hours AFTER each settlement time**: peaks at **02:00, 10:00, 18:00 UTC**.

**Interpretation**: rates diverge immediately after payments are processed (different exchanges recalibrate funding at slightly different times + their post-payment new rates initially diverge); 2-hour lag is the convergence time before market re-equilibrates.

**Pattern is most pronounced for CEX-DEX pairings** — cross-type integration is the slowest, so settlement-time divergence amplifies most across the CEX-DEX boundary.

**Wiki-relevance — execution timing**:

- Funding-carry / cross-venue arbitrage strategies should **enter positions in the 2-hour post-settlement window** when spreads are widest, NOT mid-cycle when spreads have converged
- 02:00 UTC is the highest-spread hour (Asian session activity layered on top of settlement divergence); 17:00 UTC is the lowest (early NY session normalization)
- 2.5 bps peak-trough is small relative to typical transaction costs — **the pattern alone doesn't support an intraday timing strategy**, but combined with selective high-spread opportunity identification, timing matters

### Day-of-week patterns (weak)

F = 9.69, p < 10⁻⁹ (statistically significant; economically modest):

- **Friday**: highest mean spread 15.78 bps
- **Tuesday**: lowest mean spread 14.73 bps
- ~1 bps spread between best and worst weekdays

**Weekend effect is NOT pronounced** (consistent with 24/7 crypto markets having no institutional market closures). 3 trading sessions: Asia 15.35, Europe 15.47, North America 15.10 — economically equivalent.

### Window-width sensitivity (Section 4.5)

Across 5-day, 7-day, 10-day windows:

| Width | Mean integration gap | Std |
|---|---:|---:|
| 5 days | 0.103 | 0.065 |
| **7 days** (baseline) | **0.109** | 0.057 |
| 10 days | 0.120 | 0.049 |

Wider windows smooth fluctuations. **Qualitative patterns appear robust** to window-width choice.

## Honest caveats (author-stated)

1. **2-month sample remains short** by financial econometrics standards. The trends documented (especially widening integration gap) could reflect transient conditions specific to Nov 2025 - Jan 2026. Six-month or longer sample needed for full structural-trend confirmation.
2. **Bivariate Granger tests don't control for third-party exchange information flow**. Multivariate VAR encompassing all 26 exchanges simultaneously was computationally prohibitive. Future work: sparse VAR or network-based methods.
3. **Seasonal analysis uses simple F-tests** without panel-corrected standard errors for time-varying volatility / cross-sectional dependence.
4. **Cointegration not tested** at long-run integration level — paper uses rolling Pearson correlations. Long-run equilibrium assumption may not hold given temporal-dynamics focus.
5. **Granger-causes denotes statistical predictive precedence only** — no structural / economic causation claimed.

## Implications for wiki priorities

### Sharpening [[foundation/sources/two-tiered-funding-rate-markets|companion Two-Tiered paper]]

The companion paper's findings need to be read **conditional on the temporal-stability picture this paper provides**:
- 8-day IGARCH finding is sampling-artifact (only 24.5% of windows IGARCH-prone) → revise risk-model implications
- "Zero DEX→CEX causality" finding is per-pair on snapshot; 25% windows show HYPERLIQUID→BINANCE causality on rolling-window → CEX-leadership not absolute
- 61% higher CEX integration gap is the mean; in any specific 7-day period the gap can be anywhere in [−0.041, 0.222]

### Sharpening [[trading/mechanisms/funding-cycle-dynamics]] §Academic anchors

The Academic-grade-empirical-anchors section gets a deeper rolling-window dimension:
- Intraday settlement-time patterns (02:00, 10:00, 18:00 UTC peaks) provide execution-timing discipline
- Granger size-paradox (Bybit→Binance leadership) reframes which venues to monitor for funding-rate regime detection

### Sharpening [[foundation/methodology/path-quality-diagnostics#split-timing|path-quality-diagnostics §2.3]]

The split-timing-scrutiny discipline applies when underlying market has **episodic regime structure** (meta_post_cascade reversal worked example). This paper's gradual-drift finding identifies a DIFFERENT regime structure where the discipline doesn't fire. Wiki should note both regime structures.

### Sharpening [[trading/mechanisms/etf-era-carry-trade-funding-distortion#discrimination-signals-capability-gap|etf-era page]]

Multi-venue feed capability investment should prioritize **information-rich venues** (Bybit, OKX, HYPERLIQUID) not just **volume-largest venues** (Binance). The 25% HYPERLIQUID→BINANCE causality rate partially answers the within-DEX-heterogeneity question.

## Related

- [[foundation/sources/two-tiered-funding-rate-markets]] — companion paper from same lead author (Petar Zhivkov); same dataset infrastructure
- [[foundation/sources/he-manela-fundamentals-perpetual-futures]] — single-venue pricing-fundamentals companion (different lens, complementary scope)
- [[foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class]] — asset-pricing + carry-trajectory companion (Binance-only)
- [[trading/mechanisms/funding-cycle-dynamics]] — family page; cross-venue temporal dynamics + intraday settlement patterns sharpen the family
- [[trading/mechanisms/etf-era-carry-trade-funding-distortion]] — distortion thesis; cross-venue empirics + within-DEX heterogeneity (HYPERLIQUID specifically) refine the discrimination-signal framing
- [[foundation/methodology/path-quality-diagnostics]] §2.3 split-timing scrutiny — gradual-drift regime structure as distinct from episodic-regime structure

## Sources

- Journal: https://www.mdpi.com/2227-7072/14/5/103 (Int. J. Financial Studies 14(5):103, MDPI April 2026)
- DOI: https://doi.org/10.3390/ijfs14050103
- User-supplied PDF (18 pages) ingested 2026-05-15 PM
- Funded by EU NextGenerationEU + Bulgarian National Science Fund
- Author cluster: Petar Zhivkov + Venelin Todorov + Slavi Georgiev (Bulgarian Academy of Sciences + Angel Kanchev University of Ruse)
