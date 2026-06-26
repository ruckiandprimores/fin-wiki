---
title: "meta_session_v1 — standalone session-boundary REJECTED; 2026-05"
status: evergreen
created: 2026-05-11
updated: 2026-05-11
tags: [rejected, calendar-effects, session-boundary, equity-crypto-translation-failure, forecast-vs-tradeable-gap, day-swing]
---

# meta_session_v1 — Standalone Session-Boundary REJECTED; 2026-05

> **TL;DR**: Pre-registered test of whether session-boundary signal at UTC h7 (Asia→Europe), h15 (Europe→US), and h21–23 (US end-of-day) exists as a **standalone** tradeable signal on BTCUSDT perp 1h, 2025-05-03 → 2026-04-28. Decisively no. 0% of 500 cohort configs full-positive; cohort ATP −$0.57 vs pre-registered ≥$0.50 threshold; all 4 sub-strategy hypothesis-level falsifiers fire; train→val corr **−0.915** (extreme anti-predictive on a broken cohort). The earlier #3 finding (h7 ATP +$1.69) was a **confluence signal** (session-hour × funding × bar-direction), not a session-boundary signal — session-hour was a co-occurrence marker, not the edge generator. Equity session-boundary literature (Hasbrouck 2007, Harris 2003) rests on a forced-close mechanic that does not exist in 24/7 crypto perpetuals. **Mechanism does not translate at standalone op form.** Family closed for future strategy-generation at standalone form.

## Hypothesis (Structured Form)

```
Hypothesis:    Session-boundary signal at major UTC handovers
               (Asia→Europe at h7, Europe→US at h15, US end-of-day
               h21–23) creates predictable directional pressure on
               BTCUSDT perp **independent of any confluence filter**
               (funding state, bar direction beyond hour-gate).

Mechanism:     Equity-market literature: institutional desk handovers
               + forced-close mechanics at session boundaries create
               discontinuous liquidity → reliably-skewed return
               distributions at the boundary (Hasbrouck 2007;
               Harris 2003 Ch 28). ETF launch institutionalizes BTC
               flow further; 24/7 crypto imports the equity-market
               session pattern.

Observable:    BTCUSDT perp 1h, 2025-05-03 → 2026-04-28, $1000
               capital, fee 0.04%. 4 sub-strategies — h7 / h15 bidirectional
               with bar-direction; h21–22 long-only; h23 short-only.
               500 parameter configs (h7.stop × h7.target × h7.risk_pct
               sweep; h15 / h21-22 / h23 fixed at spec defaults).

Expected:      h7 standalone ATP ≥ $0.50 (≥30% of the #3 filtered
               pocket at $1.69); h15 standalone ATP ≥ $0.30; h21–22
               long-side ATP > 0; h23 short-side ATP > 0; aggregate
               ≥ $120/yr at $1K capital.
               expected_net_edge: independent source — equity-literature
               + W2 raw forecasting-effect ledger (different sub-window).
               Per [[foundation/methodology/deployment-edge-threshold|deployment-edge §4.5]]
               this clears the independence bar; verdict is the L2
               translation test.

Falsifier:     If ALL four sub-mechanisms fail (ATP < threshold each),
               session-boundary as a standalone mechanism rejected at
               this operationalization. Path forward conditional on
               which sub-mechanism(s) fire.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp 1h, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Cohort full-positive ratio | **0% of 500** | ≥ 1 config positive | **YES** (universal failure) |
| Cohort val-positive ratio | 0% of 500 | — | — |
| Cohort ATP (full window) | **−$0.57 per trade** | ≥ $0.50 | **YES** by $1.07 |
| Best val combo | −$98 | > 0 | **YES** (no positive region) |
| Mean Sharpe | −4.69 | ≥ 0 | **YES** |
| train→val corr | **−0.915** | ≥ +0.20 | **YES** (extreme anti-predictive) |
| Q1→Q5 val | −$126 → −$171 | monotone-positive | **YES** (strict monotone WORSE) |

### Falsifier verdict per pre-registered sub-mechanism

| Pre-registered hypothesis | Observed | Verdict |
|---|---|---|
| h7 standalone ATP ≥ $0.50 | Aggregate cohort ATP −$0.57; h7 best −$0.08 | **FAILS by $0.58–$1.07** |
| h15 standalone ATP ≥ $0.30 | Aggregate negative; cannot isolate h15 positively | **FAILS at aggregate** |
| h21–22 long ATP > 0 | long_pnl cohort mean −$438 | **FAILS** |
| h23 short ATP > 0 | short_pnl cohort mean −$266 | **FAILS** |

**All four fire. Reject at hypothesis level.**

## Structural diagnostics

### Trade-count invariance — 1239 trades exactly across all 500 configs

The parameter sweep changes WHEN trades exit (stop / target / time_stop) but not WHETHER they fire — hour-of-day gates fire on every qualifying bar without signal filtering. This is the **gate-only-entry pathology**: if any edge existed it would have to come from the gate alone; no exit-parameter combination can rescue a strategy whose entry signal lacks edge. The trade-count invariance is itself diagnostic — confirms the rejection is at the mechanism layer, not the parameterization layer.

### Exit profile dominated by time-stops

| Exit type | Mean count per 1239 trades | Share |
|---|---:|---:|
| Time-stop | 609.9 | 49% |
| SL | 472.6 | 38% |
| TP | 156.1 | 13% |

Time-stops firing more often than SL is the **trades-dying-in-the-middle** signature. At time_stop = 2h on a 1h frame, most trades neither reach target nor stop — they expire mid-move at small per-trade P&L, which friction (~$0.40/trade at $500 position scale) eats fully. The mechanism couldn't sustain hold through normal bar noise; per [[foundation/methodology/forecast-vs-tradeable-gap]] §3.2, trade horizon exceeded the forecasting-effect's measurement horizon.

### corr(train, val) = −0.915 — broken-cohort signature

The train→val rank correlation is extreme anti-predictive on a uniformly-negative cohort. This is structurally distinct from a *working*-cohort anti-predictive rank (which would suggest "mechanism real but window-untunable"). Here, no parameter region produces edge anywhere in the cohort; the ranking inversion is noise on a no-edge cohort, not edge that the ranking misallocates.

## Mechanism translation diagnosis — why equity session-boundary doesn't translate

Equity session-boundary literature (Hasbrouck, Harris) rests on the **forced-close mechanic**: at the 4pm EST NYSE close, positions are required to flatten (mark-to-market obligations, fund NAV calculation, hedge-fund close-of-day cuts). This creates *discontinuous liquidity* at handovers, which the literature documents as tradeable.

**24/7 crypto perpetuals have no forced-close mechanic.** No participant is structurally required to flatten at any UTC hour. The mechanism that generates the equity-market session-boundary signal — forced position closure → liquidity gap → predictable directional pressure — is structurally absent in crypto perpetual markets. The session-handover hour is a calendar marker, not a flow-generating event.

This is a clean N=1 case of **equity-crypto mechanism translation failure**. The mechanism is real where it was sourced; the carrier (forced-close mechanic) does not exist in the target market.

## What this rejects vs. what remains

### REJECTED at standalone op form

- **Session-hour at h7 (Asia→Europe) as standalone signal** — no edge
- **Session-hour at h15 (Europe→US) as standalone signal** — no edge
- **Session-hour at h21–22 (US carry-into-close, long-only)** — no edge; the W2 raw forecasting effect did NOT survive friction + stop-target geometry
- **Session-hour at h23 (US-close, short-only)** — no edge

### REMAINS valid (different mechanism)

The original #3 finding (`funding_settlement_window_1h` at h7 with funding-state + bar-direction filter; ATP +$1.69 at pocket) is a **confluence signal**, not a session-boundary signal. The session-hour was a co-occurrence marker; the funding-state + bar-direction filter is the actual edge generator. Lab artifact remains at `strategies/tested/funding_settlement_window_1h/`. The confluence form has not been falsified — it tested a different mechanism (funding-settlement-window) with different operational pre-registration.

A possible follow-up `meta_session_confluence_v3` could test the confluence form rather than standalone — but if it does, it should be **named** as a confluence-form strategy, not a session-boundary strategy. Naming matters: the rejected mechanism is "session-hour generates edge directly"; the surviving candidate is "funding × bar × hour interaction generates edge."

## Reason Classification

- [x] **Falsifier fired** on all four pre-registered sub-mechanisms
- [x] **Mechanism does not translate** — equity forced-close mechanic absent in 24/7 crypto perp
- [x] **Standalone op form closed** — session-hour alone has no edge in 2025-26 BTCUSDT perp 1h
- [x] **Partial-truth retained**: confluence form (session × funding × bar per #3) is a different mechanism and has not been tested-and-falsified at confluence-form pre-registration
- [ ] Mechanism absent (the equity-market mechanism IS real where it lives; this rejection is about translation, not mechanism existence)

## Trial Count on Dataset

500 configs × 1239 trades each. Strategy-level falsifier fired with massive margin (0% of cohort full-positive vs ≥1 expected; cohort ATP −$0.57 vs ≥$0.50 threshold). Robust to multiplicity correction per [[foundation/methodology/strategy-validation-discipline|LdP §8 multiplicity]] — when the entire cohort is negative, no trial-count adjustment recovers a positive verdict.

## Cross-strategy pattern significance

This is the **canonical N=1 case study** for [[foundation/methodology/forecast-vs-tradeable-gap]]. The W2 raw forecasting effects (h21 +0.042%, h22 +0.058%, h23 −0.045% fwd-1h) IS real at forecast frame; it did NOT survive translation to trading frame because:
- Effect magnitude (≤ 0.1% per fwd-1h) is below the friction floor at this trade frequency
- Time_stop = 2h on 1h frame produces noise-driven mid-move exits before the forecasting effect can register

See the case study in `forecast-vs-tradeable-gap.md` §"Case study (N=1): W2 raw → meta_session_v1" for the full L1→L2 decomposition.

## Key Takeaways

- **Session-boundary as a standalone signal does not exist on BTCUSDT perp 1h 2025-26.** 0% of 500 cohort configs profitable; all 4 sub-mechanisms fail at hypothesis level.
- **Equity session-boundary literature rests on a forced-close mechanic that 24/7 crypto perpetuals don't have.** The mechanism is real where sourced; the carrier doesn't exist in the target market.
- **The earlier #3 finding (h7 ATP +$1.69) was a confluence signal, not a session-boundary signal.** Session-hour is a co-occurrence marker; the funding + bar filter is the edge generator.
- **Forecasting effects at +0.04–0.06% per fwd-1h are below the friction floor at 1239 trades/year.** Without amortization geometry that doesn't introduce noise-driven mid-move exits, the L1 effect doesn't survive to L2.
- **Future strategy-generation should not re-propose session-boundary as a standalone mechanism for crypto perp.** If session-hour is used at all, it must be as a confluence factor with a flow-generating carrier (funding state, calendar event), not as the primary signal.

## Related

- [[trading/rejected/wyckoff-spring-4h-2026-05]] — different mechanism family; same lesson at the operationalization level (equity-market mechanism didn't translate to crypto operational form; capability-gap-blocked there, mechanism-translation-blocked here)
- [[foundation/methodology/forecast-vs-tradeable-gap]] — canonical N=1 case-study cite; full L1→L2 decomposition documented there
- [[trading/mechanisms/funding-cycle-dynamics]] — confluence form (#3 source: `funding_settlement_window_1h`) is the surviving artifact
- [[trading/mechanisms/liquidation-cascade-aftermath]] — companion equity-source family with a different translation outcome (capability-blocked but mechanism likely translates)
- Lab artifact: `strategies/tested/meta_session_v1/`
- Decomposition: `strategies/analyses/2026-05-11-meta-batch-decomposition/meta_session_v1.md`

## Sources

- Hasbrouck, J. (2007). *Empirical Market Microstructure* — equity session-boundary mechanism (forced-close mechanic foundation)
- Harris, L. (2003). *Trading and Exchanges* Ch 28 — adverse-selection / session-handover flow dynamics; per [[foundation/sources/harris-trading-exchanges]]
- 2026-05-06 #3 trade-level analysis (`funding_settlement_window_1h`) — original confluence-form finding
- 2026-05-05 W2 raw addendum — the backtesting lab (raw forecasting effects)
- 2026-05-11 `meta_session_v1` decomposition — the backtesting lab
