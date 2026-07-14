---
title: "Open Questions — Wiki Index"
status: growing
created: 2026-05-15
updated: 2026-07-12
tags: [meta, index, open-questions, research-priorities, state-monitor-era]
---

# Open Questions — Wiki Index

> **Purpose**: a strategically-prioritized index of open questions across the wiki. the wiki architecture (ARCHITECTURE.md) Part V flags "no open questions tracked" as a Red Flag — the wiki should always have 3-5 active research questions surfaced. This page provides that surface, organized by strategic priority. Per-page Open Questions sub-sections remain the source of depth; this index is the navigation layer.
>
> **Refresh cadence**: regenerated as wiki state shifts. **Full regeneration 2026-06-26** — re-anchored from the archived deployment thread (PORTFOLIO_v7/v8 + spot-hedge) to the **state-monitor era**. The prior strategic ordering was decisively superseded: the discovery track **CONCLUDED 2026-06-08** (prospective direction unpredictable — full stop; Path A directional-classifier FALSIFIED, Path C gate-state-not-side is what works; all causal/portfolio strategy folders archived to the backtesting lab, no auto-pilot deployment). The active thread is now the **human-in-the-loop state-monitor** (`state-monitor` skill delivered + venue-verified on Binance Demo) and the research that sharpens it. Worldview prior **#14** codified: *gate state/risk, not side*.

## What changed since the last refresh (2026-05-19 → 2026-06-26)

| Prior thread | Status now |
|---|---|
| PORTFOLIO_v7/v8 deployment decision | **CLOSED** — no auto-pilot deployment; full automation off the table (2026-06-08). Validated mechanics are now *signal inputs* to the monitor, not standalone deployables. |
| In-strategy directional discovery (busy_hour BTC compound, reversed-direction mutations, cross-regime validation gates) | **CLOSED** — directional-classifier path falsified across 5+ detector designs. These questions are frozen, not active. |
| Spot-hedge funding bot (a team member) | **ACTIVE, renamed SFA (Cross bot)** — in testing + working as of 2026-06-12; BingX integration next. See [W4](#w4). |
| Funding-carry broad-carry-negative watch | **STILL LIVE** — a live funding-carry deployment running; see [W3](#w3). |
| Cross-venue funding spread / ETH deployability | **STILL LIVE** — now feeds the SFA bot decision. See [B1](#b1). |
| Smoothed→filtered haircut (regime detector) | **ELEVATED** — the causal-detector live-update path is now a named state-monitor research gap. See [SM2](#sm2). |

---

## Strategic priority — state-monitor era (the active thread)

Per this research program current direction (set 2026-06-11): *shape a semi-automated trading approach through daily monitor use.* The machine describes the objective state (its proven strength); the human makes the regime/de-risk call. The questions below are load-bearing for that thread.

### SM1. Which validated mechanics belong on the monitor card as signals? {#sm1}

The central active research question. The discovery track produced validated *mechanics* (causal regime detection, vol-state transitions, confirmation-delay filters, breakout-direction asymmetry) that are now **signal inputs to the human-driven monitor**, not standalone strategies. **Open**: mine the archived strategies' validated mechanics ([[../trading/mechanisms/breakout-direction-asymmetry-by-macro|breakout asymmetry]], [[../trading/mechanisms/vol-regime-transition-tradeable|vol-regime transition]], [[../trading/mechanisms/regime-gate-vs-bias-filter|regime-gate vs bias-filter]], the three causal frontier variants) to determine which additional signals earn a place on the card. Card-signal proposals are gated at N≥2 from the human observation trail (the backtesting lab).

- Source: this research program (current direction #2) + observation-trail mechanism
- Status: **active research** — accumulating pattern observations across regimes (Regime 15 bear is the first live-watched episode)
- Decides: the monitor card's signal roster; the shape of the semi-automated approach

### SM2. Smoothed→filtered haircut + causal-detector live-update path {#sm2}

Per [[../foundation/methodology/online-regime-detection#open-questions|online-regime-detection]]: the team's regime labels (stable-range detector, regime_v3) are produced in **smoothing** mode (confirmed only after forward extremums + full-gap Mann-Kendall). Live deployment needs **filtering** mode (callable at a range's start from past data only). **The highest-value runnable experiment**: re-grade existing regime labels in filtering mode, re-run the gated backtests, and measure the smoothed→filtered performance drop — that drop *is* the live edge. Needs no new feed (it's code). The monitor already computes its detectors causally (per the 2026-06-11 [[../foundation/methodology/online-regime-detection#compute-full-history-then-slice|full-history-then-slice corollary]]); the open gap is quantifying the haircut vs the smoothed labels and benchmarking a bespoke online detector (score-driven BOCPD per [[../foundation/sources/tsaknaki-lillo-mazzarisi-bocpd|Tsaknaki et al.]]) against the free CMC Fear & Greed filtered baseline.

- Source: [[../foundation/methodology/online-regime-detection]] + [[../foundation/sources/issa-horvath-online-regime-detection]] + [[../foundation/methodology/regime-marker-data-sources]]
- Status: methodology landed; haircut experiment unrun (highest-value next regime step)
- Decides: magnitude of the retrospective-classifier live-deployment gap; whether a custom online detector beats the free baseline

### SM3. Deployment-phase engine gaps — slippage + live-vs-backtest calibration {#sm3}

Per this research program (current direction #3): sharpening deployment-phase engine gaps is in-scope. **Open**: the backtest engine has no slippage model calibrated to live fills; the monitor's live-execution path (venue-verified on Binance Demo) now generates real fills that can be diffed against backtest assumptions. How large is the live-vs-backtest gap on the monitor's own signals, and which engine assumptions (fill price, fee tier, latency) dominate it?

- Source: this research program (deployment-phase engine gaps) + [[../foundation/methodology/forecast-vs-tradeable-gap#realization-gap|realization-gap §2B]] (~50% of forward signal captured by discrete-bar exits)
- Status: capability now exists (live-demo fills); calibration unrun. **Calibration sources ingested 2026-07-12**: [[../foundation/sources/talos-crypto-market-impact]] (crypto-specific empirical impact decomposition, institutional scale) + [[../foundation/sources/maitrier-metaorder-reconstruction]] (public-data method for an in-house venue-specific impact curve on Binance/Bybit tapes). Honest gap confirmed by the same research pass: **no academic paper-to-live degradation study exists** — the observation-trail loop remains primary research here.
- Decides: trust level on backtest metrics for monitor signals; sizing of the realization-gap haircut

### SM4. State-monitor build gaps — per-lot tracking, resting orders, multi-symbol cards {#sm4}

Skill roadmap items still open (this research program skill roadmap):
- **(g) Per-lot position tracking** — v1 merges same-side adds into one averaged position per account; surfaced twice (close-one-tranche request + SL-replacement-on-add + PnL-misattribution at averaged entry). Journal preserves per-fill records meanwhile. Highest-friction known gap.
- **(c) Live-execution enhancements** — market entries/exits only; **resting/limit entry orders + close-by-USD remain unimplemented** (optional enhancement). Protective SL/TP already ride as venue conditional/OCO orders.
- **(b) Multi-symbol cards** — causal detector is already symbol-generic via `--src-*`; the card surface is not yet multi-symbol.

- Source: this research program skill roadmap (b)/(c)/(g)
- Status: per-lot tracking is the priority gap; the others are optional enhancements
- Decides: monitor operational completeness for guided trading

### SM5. Daily pattern-observation accumulation → card-signal proposals {#sm5}

Per this research program (current direction #2) + SKILL.md §4 post-trade learning loop: every real-session close appends an entry-vs-outcome record to the observation trail (the backtesting lab); the Card-gap field feeds card-signal proposals at N≥2. **Open**: which card gaps recur across regimes? The 2026-H1 bear (Regime 15) is the first regime watched live — what does the daily trail surface that the card misses? This is the empirical feeder for SM1.

- Source: this research program + [[../foundation/macro/btc-regime-taxonomy-2020-2025#regime-15-june-markers|Regime 15 June-episode markers]]
- Status: active — observation trail accumulating
- Decides: which signals graduate from observation to card (N≥2 gate)

---

## Live operational watches (monitoring, not research)

These are named falsifier-adjacent watches on running tracks. State should be precise in substrate, not session memory.

### W1. ETF-flow falsifier state — L1 HODL sleeve guard {#w1}

The L1 HODL sleeve was activated as a paper opening tranche this cycle; the ETF-flow falsifier guards it. State as of 2026-07-12: a **second streak** (Jun 18 → Jul 1, 10 days, −$2.73B; IBIT's own ≥12 sessions) ended Jul 2 (+$221.7M, largest inflow in ~2 months); June 2026 = **worst month on record** (~$4.1–4.5B out); since the break the tape is choppy (+4/−2 days through Jul 10, which printed +$90.4M) — **fragile stabilization, NOT a confirmed inflow regime**. Cumulative since May 7: −$8.95B over 34 negative days. **Falsifier arithmetic: max streak 13d (May 15 → Jun 3/4) vs the 90-day trigger — far from firing.** Watch: streak restarts; a confirmed inflow regime is what stabilization would look like.

- Source: [[../foundation/macro/btc-regime-taxonomy-2020-2025#regime-15-june-markers]] + [[../foundation/macro/treasury-vehicle-cascade-watch#status-update]]
- Status: falsifier far from firing; structural bid wounded not unwound
- Decides: L1 HODL sleeve continued-accumulation vs pause

### W2. Treasury-vehicle cascade propagation — periphery → core? {#w2}

Per [[../foundation/macro/treasury-vehicle-cascade-watch#status-update|cascade-watch (2026-07-12)]]: the June "partial reversal" read is superseded — **the core mode-shift is now STRUCTURAL** (Strategy's largest-ever sale, 3,588 BTC / ~$216M Jun 29–Jul 5, + a formalized $1.25B BTC Monetization Program: sell-to-fund-dividends is a standing mechanism), and the periphery progressed **from forced sales to complete exits** (K Wave first full corporate liquidation ~Jul 2; Sequans + Genius Group DAT-model exits). **Open**: does the Q2 disclosure window (July–August, now open) re-classify additional top-10 vehicles into actively-manage-mode (condition 1 needs 3+ synchronous; currently ~1)? Watch also: follow-on sales under the $1.25B authorization; Metaplanet's mNAV path (a forced pivot there = second top-10 mode-shift). Supply-shock trigger still NOT met.

- Source: [[../foundation/macro/treasury-vehicle-cascade-watch]]
- Status: core mode-shift structural (managed, not forced); periphery unwinding; Q2-disclosure watch live
- Decides: crypto-investment Layer 1 + Layer 3 trim triggers

### W3. Funding-carry deployment Sharpe trajectory vs broad-carry-negative-2025 {#w3}

Per [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class#carry-trajectory|Borri et al. (2025)]]: academic carry Sharpe 6.45 → 4.06 → **negative** (2025). A live funding-carry deployment rides selective-entry alpha above broad carry. **If broad-carry-negative persists, the deployment's selective-entry alpha may decay.** N=1 non-transfer worked example: VRP + delta-neutral funding harvest did NOT decay analogously (different counterparty stacks); watch for N=2 at the next adverse-event window.

- Source: [[../foundation/sources/borri-liu-tsyvinski-wu-investable-asset-class#carry-trajectory]] + [[../trading/mechanisms/insurance-premium-harvest-overview#regime-decay-non-transfer]]
- Status: empirical monitoring (live funding-carry P&L)
- Decides: funding-carry continued-deployment + premium-harvest deployment timing

### W4. SFA (Cross bot) spot-hedge funding arb — testing → BingX → deploy {#w4}

Per this research program: the spot-hedge funding-rate bot (owner a team member) is **in testing and working as of 2026-06-12** — a team member integrated exchanges directly (around the no-lending-pools blocker). Next: connect BingX. Delta-neutral funding arbitrage, reliable at any capital scale (per a standing convention — not a TradFi carry that needs scale). **Open**: which venue-pairs clear friction, and does the empirical feedback sharpen the [[../trading/mechanisms/delta-neutral-funding-harvest#spot-hedge-bot|delta-neutral-funding-harvest]] page's friction/term-structure sections?

- Source: this research program (SFA) + [[../trading/mechanisms/delta-neutral-funding-harvest]]
- Status: in testing; BingX integration pending
- Decides: delta-neutral-funding-harvest page upgrade + venue-mix operationalization

---

## Capability-investment decision factors

Per a standing convention — research runs ahead of capability and sharpens the capability-investment decision itself.

### B1. Cross-venue funding spread — is ETH the only deployable symbol? {#b1}

Per [[../foundation/sources/two-tiered-funding-rate-markets|Two-Tiered]] portfolio simulation: 8 of 20 portfolios profitable; average net P&L +$22; average Sharpe −7.40; 95% reversal-pattern. **ETH funding-spread half-life = 20 minutes** (AR(1) 0.966) is unique among 10 symbols. Question sharpens to: **is ETH the only deployable symbol, and what venue-pair / sizing clears friction?** Now feeds the SFA bot decision ([W4](#w4)).

- Source: [[../foundation/sources/two-tiered-funding-rate-markets]] + [[../foundation/sources/temporal-dynamics-funding-rates#granger-size-paradox|Granger size-paradox]]
- Status: portfolio-simulation-grade evidence; ETH-specific deployment open
- Decides: multi-venue feed priority + SFA venue-pair selection

### B2. Liquidation-cluster feed as a monitor card signal {#b2}

Per [[../trading/mechanisms/liquidation-cascade-aftermath#capability-gap-reinforcement|liquidation-cascade-aftermath]]: actual liquidation events would discriminate true cascades from noise wicks. Reframed for the state-monitor era: rather than lifting an archived meta-strategy's path-quality, **would a Coinglass liquidation feed earn a place as a CONTEXT-block card signal** (alongside funding + OI + F&G already in `monitor.py context`)? The Regime-15 cascade magnitudes ($1.8B/$1.1B/$1.6B days) are exactly the kind of marker the card currently reads only indirectly.

- Source: [[../trading/mechanisms/liquidation-cascade-aftermath]] + [[../foundation/methodology/regime-marker-data-sources]]
- Status: capability-not-yet-built; reframed as card-signal candidate. **Advanced 2026-07-12**: heatmap construction methodology now ingested ([[../foundation/sources/hyblock-coinglass-liquidation-heatmaps]] — both vendors are MODELED estimates; Hyblock auditable-in-shape with a free API + historical snapshots, Coinglass opaque but market-watched); the magnet hypothesis is now a structured research-grade page with a pre-registerable validation experiment ([[../trading/mechanisms/liquidation-heatmap-signal]]). No academic test of the magnet effect exists (2026-07 research pass) — the validation is in-house work, not literature.
- Decides: liquidation-feed priority on the capability roadmap (Hyblock API now the leading measurement candidate over Coinglass)

### B3. Within-DEX heterogeneity + integration-gap persistence {#b3}

Per [[../foundation/sources/temporal-dynamics-funding-rates#granger-size-paradox|Temporal Dynamics]]: HYPERLIQUID → BINANCE 25% of windows (vs reverse 13%) — compositional-not-size price discovery; integration gap widened 0.069 → 0.143 over Nov 2025–Jan 2026. **Is the size-paradox structural across regimes, or specific to the 60-day sample? Does the gap-widening persist past Jan 2026?** Forward-rolling re-test pending fresh 2026+ data.

- Source: [[../foundation/sources/temporal-dynamics-funding-rates]]
- Status: N=1 strong anchor; cross-regime stationarity untested
- Decides: DEX-as-discriminator + cross-venue arbitrage capability prioritization

### B4. Deribit options feed for VRP harvest deployment {#b4}

Per [[../trading/mechanisms/variance-risk-premium-crypto#operationalization-status|VRP-crypto]]: BTC VRP ~14% annualized; LV cluster favorable; mechanism is research-ready but capability-blocked (no Deribit feed, no delta-hedge execution, no Greeks in lab). **When does the capability investment clear vs current priorities (SFA bot, monitor build gaps, cross-venue)?**

- Source: [[../trading/mechanisms/variance-risk-premium-crypto]] + [[../foundation/sources/deribit-insights]]
- Status: capability-blocked; substrate-ready
- Decides: VRP mechanism-family deployment timing

---

## Mechanism-family extension + wiki gaps

### C1. ETH market-structure page — ✅ CLOSED 2026-07-14 {#c1}

**RESOLVED**: [[../foundation/asset-classes/eth-market-structure]] created 2026-07-14 from the queued 3-paper cluster (builder-market decentralization arXiv:2405.01329 + Ethena/staking-basis arXiv:2605.11263 + PBS SoK arXiv:2506.18189 — all ingested at full-text grade). Completes the 3-symbol asymmetry framework (BTC continuation / SOL long-bias amplifier / ETH structural: staking-yield floor + MEV/PBS concentration). The successor open questions live on the new page: the **ETH-vs-BTC basis-comparison literature gap** (confirmed 2026-07-12 — no published paper), post-Oct-2024 builder-concentration drift, ePBS's empirical effect once live, Ethena's actual ETH-perp-OI share (unverifiable from the fetched paper — flagged, not asserted), and a primary-source recheck of the 28–30% staked-share figure.

- Status: **closed** — page exists; successor questions tracked on-page
- Decided: the 3-symbol framework is complete; lab-empirical ETH breakout-character study remains a separate (unqueued) follow-on

### C2. Companion crypto-native sources — Glassnode + CoinMetrics ingest {#c2}

Per this research program (flagged across sessions): Glassnode research + CoinMetrics blog ingest pending. The 2026-06-07 academic cluster (BOCPD, NHHM, MSGARCH, Issa-Horvath, Almeida, Deribit) plus the 4-paper perp-futures cluster reduce single-author-lineage risk; the practitioner on-chain-data layer remains uncovered.

- Status: pending wiki-session prioritization. **Research-pass verdict 2026-07-12**: the highest-value structured ingest is the **CryptoQuant User Guide** (per-metric definition+formula+interpretation, on-chain AND derivatives, machine-readable catalog); Glassnode Academy as follow-on; Coinglass docs NOT the literacy node. Queued in [[../foundation/sources/ingest-queue]] Tier 2.
- Decides: closes the on-chain practitioner-source gap

### C3. MM (Market Making) mechanism page {#c3}

Per this research program: paused (user-driven). Pairs naturally with [[../trading/mechanisms/delta-neutral-funding-harvest|delta-neutral-funding-harvest]].

- Status: paused; resumption gated on user
- Decides: when to author the dedicated MM page

---

## Methodology codification watch — FROZEN (discovery track concluded 2026-06-08)

> **Status note**: these N=1/N=2 codification candidates were generated by the strategy-discovery batch track. That track concluded 2026-06-08 — the engine that would produce N=3 recurrence is no longer running. They are **frozen at their current N**, not active. New evidence would have to come from **monitor-research backtests** (the in-scope signal-discovery work), not the discovery batches. Kept as a navigation surface for the validation-discipline methodology pages; do not expect movement without a monitor-research backtest resurfacing the pattern.

| # | Candidate | N | Page | Note |
|---|---|---|---|---|
| D1 | Bounds-narrowing buffer (≥1 grid-step PAST inflection) | N=1 | [[../foundation/methodology/sweep-validation-pattern-taxonomy#bounds-buffer]] | **LANDED 2026-06-11** (released from hold; landed as generalizable methodology) — resolved, not frozen |
| D2 | Split-timing scrutiny | N=2 | [[../foundation/methodology/path-quality-diagnostics#split-timing]] | promote to standard filter at N=3 |
| D3 | Val-period scrutiny | N=2 | [[../foundation/methodology/path-quality-diagnostics#val-period-dispersion]] | promote at N=3 |
| D4 | Mixed-cascade third category | N=2 | [[../trading/mechanisms/liquidation-cascade-aftermath]] | binary → ternary cascade taxonomy |
| D5 | Portfolio-vs-individual fragility | N=1 | [[../foundation/methodology/small-sample-validation-discipline#portfolio-vs-individual-fragility]] | naive trim-by-walk-forward can hurt a portfolio |
| D6 | Position-blocking-as-inadvertent-filter | N=2 | [[../trading/rejected/failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] | ablation-vs-same-side-subset methodology |
| D7 | Edge-saturation character | N=1 | [[../trading/rejected/range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] | distinct failure mode (real K-bar drift then reverts) |
| D8 | Compound selectivity (different-axis vs same-axis) | N=1 | [[../trading/rejected/busy-hour-flow-continuation-family-2026-05#compound-selectivity-classes]] | different-axis ADDS edge; same-axis SUBTRACTS |
| D9 | Stable-phase mis-conditioning (mandatory per-phase split) | N=4 (single batch) | [[../trading/rejected/options-expiry-friday-btc-2026-05]] | watch for cross-batch recurrence |
| D10 | Regime-decay non-transfer between premium families | N=1 | [[../trading/mechanisms/insurance-premium-harvest-overview#regime-decay-non-transfer]] | counterparty-stack analysis as discriminator — **still live** via [W3](#w3) |

---

## Long-horizon investment thread (operational system active)

Per this research program: the operational methodology layer is complete; five runnable deployment paths exist (pilot-sized per a standing convention).

### E1. First NEW-CASH decision-mechanism run in production shape {#e1}

Per this research program (five runnable paths): AI/Tech tranche / Pharma XDWH pilot / Crypto-HODL gap (BTC TRIGGERED) / REITs direct-first / DHR standalone. The first production run surfaces decision-mechanism gaps the dry-run methodology can't. The Crypto-HODL L1 sleeve is already moving (paper opening tranche; guarded by [W1](#w1)).

- Source: this research program + [[../foundation/methodology/investment-thesis-protocol#exit-mechanism-frame-coherence]] (exit-frame coherence: falsifier-exit for HODL, price-stop for trading)
- Status: L1 HODL paper tranche active; other four paths user-initiated
- Decides: decision-mechanism gaps under production load

### E2. Watchlist trigger calibration drift {#e2}

Per this research program open follow-ups: aged multiple-of-EPS triggers drift faster than quarterly cadence assumes (3 substrate corrections in 11 days). Specific: TLN trigger re-anchor (pre- vs post-AWS-PPA EPS); COIN trigger metric BROKEN in loss-making quarters (add fwd P/S secondary + EPS-stability gate, N=1 per a standing convention).

- Source: this research program watchlist triggers
- Status: calibration questions, not data errors
- Decides: refresh-cadence operational variable; trigger-metric upgrades

### E3. Sizing-by-CoC-depth + behavioral-structure multi-application audit {#e3}

Per a standing convention (weak-CoC areas get Tier-2 sizing + CoC-build path, N=1) + [[../foundation/methodology/behavioral-structure-design#open-questions|behavioral-structure-design]] (N=1 worked example; multi-application audit pending → status upgrade `seedling` → `growing`).

- Status: N=1 each; pending deployment-cycle empirical evidence
- Decides: sizing-discipline codification; behavioral-structure page upgrade

---

## Insurance-premium-harvest mechanism family

### G1. DVOL ↔ Boros term-structure correlation (cross-cutting) {#g1}

Per [[../trading/mechanisms/insurance-premium-harvest-overview#cross-cutting-questions]]: if VRP and funding-harvest share a forward-looking regime signal, large portfolio-composition implication.

- Status: empirical-research-pending (sharpens with [W4](#w4) SFA empirical feedback)
- Decides: cross-mechanism portfolio-composition logic

### G2. Sinclair-Almeida regime reconciliation {#g2}

Per [[../trading/mechanisms/variance-risk-premium-crypto#open-questions]] + [[../foundation/sources/almeida-bitcoin-risk-premia]]: Sinclair's GARCH-grounded vol-regime framing and Almeida's LV/HV cluster framing both inform when VRP is favorable. Same regimes?

- Status: literature-cross-walk pending
- Decides: VRP-harvest regime-gating discipline

---

## All open-questions sections across the wiki

For depth on any topic above, see the page-specific Open Questions sub-sections. Compiled via grep 2026-06-26.

### Foundation — methodology

- [[../foundation/methodology/online-regime-detection#open-questions]] — filtering-mode haircut; bespoke-vs-free-baseline (SM2)
- [[../foundation/methodology/regime-marker-data-sources#open-questions]] — which live marker feeds are callable now vs capability-blocked
- [[../foundation/methodology/path-quality-diagnostics#5-open-questions]] — threshold calibration, capacity interaction, multi-strategy composition
- [[../foundation/methodology/small-sample-validation-discipline#open-questions]] — sample-size verdict-line; N=30 floor calibration; pooling rule; portfolio-vs-individual fragility
- [[../foundation/methodology/behavioral-structure-design#open-questions]] — 5-feature framework multi-application audit
- [[../foundation/methodology/reit-evaluation-methodology#open-questions]] — REIT-specific factors not yet integrated
- [[../foundation/methodology/value-investing-sourcing-funnel#open-questions]] — sourcing-method effectiveness measurement

### Foundation — sources (2026-06-07 regime-detection cluster + earlier)

- [[../foundation/sources/issa-horvath-online-regime-detection]] / [[../foundation/sources/tsaknaki-lillo-mazzarisi-bocpd]] / [[../foundation/sources/nhhm-crypto-regime-switching]] / [[../foundation/sources/msgarch-crypto-vol-regimes]] — online/regime-switching detector lineage (feeds SM2)
- [[../foundation/sources/almeida-bitcoin-risk-premia]] / [[../foundation/sources/deribit-insights]] — VRP / options-vol lineage (feeds B4, G2)
- [[../foundation/sources/marks-mastering-the-cycle#open-questions]] / [[../foundation/sources/soros-alchemy#open-questions]] / [[../foundation/sources/lineage#open-questions]] — primary-text ingest priority; source-acquisition cadence

### Foundation — macro

- [[../foundation/macro/treasury-vehicle-cascade-watch#open-questions]] — cascade dynamics (W2)
- [[../foundation/macro/btc-regime-taxonomy-2020-2025]] — Regime 15 re-verify-on-completion; extended-Boom-vs-cycle-top leaning resolved
- [[../foundation/macro/ai-cycle-calibration-2024-2026#open-questions]] — AI cycle stage; comparative base-rate analogs
- [[../foundation/macro/crypto-2020-2022-cycle#open-questions]] / [[../foundation/macro/intermarket-analysis#open-questions]]

### Foundation — asset classes + meta

- [[../foundation/asset-classes/btc-market-structure]] — 5 open empirical questions (bear-regime gate now partially answered by Regime 15)
- [[../foundation/asset-classes/sol-market-structure#empirical-anchor]] — 5 open empirical questions (bear-regime gate highest priority)
- [[../foundation/asset-classes/forex#open-questions]] / [[../foundation/asset-classes/pharma-economics-primer#open-questions]]
- [[../foundation/idea-sourcing-methodology#open-questions]] / [[../foundation/relations-and-themes#open-questions]]

### Trading — mechanisms

- [[../trading/mechanisms/gamma-exposure-regime#open-questions]] — dealer-gamma regime (2026-06-07 NEW)
- [[../trading/mechanisms/regime-gate-vs-bias-filter#counter-evidence-sought-open-questions]] — gate-vs-filter design-pattern boundary (feeds SM1)
- [[../trading/mechanisms/breakout-direction-asymmetry-by-macro#cross-symbol-open-questions]] — cross-symbol regime-conditional breakout (feeds SM1)
- [[../trading/mechanisms/insurance-premium-harvest-overview#cross-cutting-questions]] — DVOL ↔ Boros; Sinclair-Almeida; N=2 regime-decay confirmation
- [[../trading/mechanisms/variance-risk-premium-crypto#open-questions]] / [[../trading/mechanisms/delta-neutral-funding-harvest#open-questions]] — VRP + funding-harvest deployment (B4, W4, G1/G2)
- [[../trading/mechanisms/etf-era-carry-trade-funding-distortion#open-questions]] — ETF-flow threshold; distortion-lift; DEX heterogeneity (now a live regime-watch per the 2026-06-11 flag)
- [[../trading/mechanisms/on-chain-mechanisms#open-questions]] — Glassnode/CryptoQuant ingest (C2)
- [[../trading/mechanisms/vol-regime-transition-tradeable#open-research-questions]] — 1h frame; multi-asset; cross-regime stationarity (feeds SM1)

### Investment

- [[../investment/allocation/equity-management-rules#open-questions]] / [[../investment/allocation/lynch-portfolio-design#open-questions]] / [[../investment/allocation/wonderful-business-fair-price#open-questions]]

## Related

- [[../index|Wiki Index]] — main navigation
- Wiki Log — chronological activity record
- this research program — substrate (project state + active priorities)

## Sources

- Synthesized from per-page Open Questions sub-sections via grep on 2026-06-26
- Strategic-priority ordering driven by this research program — **state-monitor era** (human-in-the-loop monitor + signal-discovery research) after the 2026-06-08 discovery-track conclusion (worldview prior #14: gate state/risk, not side)
- Prior refresh (2026-05-19) was anchored to the PORTFOLIO_v7/v8 + spot-hedge deployment thread, superseded by the conclusion; this regeneration re-homes every active question and freezes the discovery-track codification watch
