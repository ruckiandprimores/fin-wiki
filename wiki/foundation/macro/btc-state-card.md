---
title: "BTC State Card (living)"
description: "Single referenceable page carrying BTC's current objective market state (regime / slow-trend / volatility / levels / sentiment-positioning / multi-TF tendency) plus an append-only evolution log. Overwritten each daily-review iteration."
status: living
created: 2026-06-26
updated: 2026-06-29
tags: [macro, btc, regime, state-card, living, daily-review, descriptive-only]
---

# BTC State Card (living)

> **TL;DR (as of 2026-06-29 ~05:00 UTC):** BTC **$59,806** — confirmed **bear** (strength 0.298),
> **slow_down**, and the late-June relief bounce **FAILED**: it rejected the 60.5–62k broken-support
> wall and regression flipped to **all-timeframes-aligned DOWN** (4h −8.8% clean, R² 0.66). Now
> **high_vol / turbulent**, F&G **12** (deeper extreme fear), sliding back toward the **$58,030**
> week-low; **−53% from ATH**. The L1-HODL pilot is **PAUSED** (IBIT-led ETF outflows; re-arm unmet).
> **Descriptive only — not a forecast** ([[foundation/methodology/online-regime-detection|prior #14: gate state/risk, not side]]).

> **What this page is.** A *living* card — the wiki-resident companion to the live state
> monitor. Each daily-review iteration **overwrites the CURRENT block** and **appends one
> dated line to EVOLUTION**. It records the machine-described objective state only; the
> human's directional / de-risk call is **not** recorded here (it lives in the monitor
> journal / decision log). This is the first instance of the **daily top-assets review**
> card format — BTC is the trading/crypto frame; the investing frame is
> [[investment/tech-leaders-card|Tech Leaders + Key ETFs]].
>
> **Provenance ([[foundation/methodology/research-agent-design|prior #7]]):** every
> field is regenerable from the state monitor (`state` / `context` / `regression` /
> `levels` / `digest`). No fabricated numbers.

## CURRENT (overwritten each iteration)

| Field | Reading | Source cmd |
|---|---|---|
| As-of | 2026-06-29 ~05:00 UTC · **$59,806** | `state` |
| Slow trend | `slow_down` (daily −1, weekly −1) | `state` / `digest` |
| Regime | bear, strength 0.298 · no breaks (up/dn) · macro_bull no | `state` |
| Volatility | **`high_vol` (turbulent — size caution)** · flipped up from calm | `state` |
| Regression | **ALL-TF aligned DOWN** — 15m −1.2% / 4h −8.8% (clean R² 0.66) / 1d −11% · the bounce failed | `regression` |
| Resistance | 59,846 (+0.1% ⟲S→R) · 60,162 (+0.6% ⟲S→R) — the broken-support wall that rejected the bounce | `levels` |
| Support | 59,531 (−0.5%) · 59,153 (−1.1%) · **58,030 (week/month low)** | `levels` |
| Sentiment | Fear & Greed **12 (extreme fear)** (20 → 18 → 12, deteriorating) | `context` |
| Positioning | funding +0.0066%/8h (~+7.2%/yr) · OI 104,391 BTC, +1.3%/24h | `context` |
| Drawdown | **−53% from $126,198 ATH** (Oct 2025) | derived |
| ETF flows | **IBIT-led outflows, 7th straight day (Jun 26 −$444.5M); pilot PAUSED; re-arm = IBIT net-inflows (UNMET)** | research arm |

**Objective read (descriptive):** the relief bounce **failed at the broken-support wall** →
aligned downtrend resuming toward the $58,030 week-low, in high_vol. Trading: a breakdown short
firms on a *confirmed-held* loss of 58,030 (high_vol = size-caution, not chase); an up-break would
need reclaim of the 60.5–62k cluster (none). Investing: L1-HODL pilot **paused**, re-arm on IBIT
net-inflows (unmet), with Strategy's STRC forced-sale event (record Jun 30 / pay Jul 15) adding
near-term supply pressure. Falsifier for "structural-bid intact": an ETF-flow 90-day net-outflow
streak (wounded, not fired — see [[foundation/macro/treasury-vehicle-cascade-watch|treasury-vehicle cascade-watch]]).

## EVOLUTION (append-only, one line per iteration)

- 2026-06-26 — $59,392 · bear/slow_down · F&G 13 · −5.1% wk · OI +6.6% (leverage building) · coiled 59,060–58,030 support vs broken-support wall; no break.
- 2026-06-27 — $60,418 · bounced INTO the 60.5–62k broken-support shelf · regression CONFLICTING (15m up, transition) · funding mild-long + OI −1.2% (the down-move liquidated longs) · staged short dual-trigger (shelf-rejection / 58,030-loss).
- 2026-06-29 — $59,806 · **bounce FAILED** — rejected the shelf, regression all-TF-aligned-DOWN, vol→high_vol · F&G 12 · sliding toward 58,030 · pilot PAUSED (IBIT 7-day outflow, re-arm unmet); Strategy STRC forced-sale Jun30/Jul15.

## Related

- [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC Regime Taxonomy 2020–2025]] — this is **Regime 15 (2026-H1 bear)**, the first contemporaneously-watched episode
- [[foundation/macro/regime-conditional-edge-2025-26|Regime-Conditional Edge 2025-26]] — what carries edge in which regime
- [[foundation/macro/treasury-vehicle-cascade-watch|Treasury-Vehicle Cascade-Watch]] — the ETF-flow / forced-seller falsifier surface
- [[foundation/methodology/online-regime-detection|Online (Prospective) Regime Detection]] — why the card's detectors are computed causally
