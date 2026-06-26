---
title: "funding_settlement_window_1h — settlement-cycle hypothesis rejected; h7/h15 partial truth (2026-05)"
status: evergreen
created: 2026-05-06
updated: 2026-05-06
tags: [rejected, settlement-cycle, funding-cycle, session-boundary, forecast-vs-tradeable, h23-not-replicating, mechanism-reframe, day-swing]
---

# funding_settlement_window_1h — settlement-cycle hypothesis rejected; h7/h15 partial truth (2026-05)

> **Lead learning**: The pre-funding-settlement positioning mechanism (W2 raw-data h23 finding: fwd-1h returns negative; conditional inversion at Q5 funding) **does not replicate** when conditioned on a funding+bar filter at 4h frame. The W2 h23 raw finding is a *forecast effect*, not a tradeable strategy — sub-bp magnitudes don't survive friction + execution timing. The 3-window post-process (load-bearing pre-registered falsifier) showed h23 essentially DEAD ($0.01 ATP) under the filter; **h7 (Europe open) and h15 (US open) carry the signal instead — mechanism reframe to session-boundary effects** with h7 robust across BOTH train and val. Verdict: REJECT settlement-cycle mechanism as defined; partial truth retained at session-boundary mechanism family.

## What we learned

1. **W2 raw h23 finding does not survive filtering.** The unconditional W2 raw-data scan (2026-05-05) showed h23 fwd-1h returns at −0.045% (apparent active hour). Under the funding+bar filter required to make it a tradeable strategy at 4h frame, h23 is **DEAD** (ATP $0.01, n=21 across full year). Per [[foundation/methodology/forecast-vs-tradeable-gap|forecast-vs-tradeable-gap]] methodology: forecast effects with sub-bp magnitudes do not survive the L1→L2 translation gate.

2. **Where the signal actually lives: h7 + h15.** The 3-window post-process (designed as a calendar-artifact falsifier — predicting that if h23 was a true settlement-cycle effect, it should dominate h7 and h15) inverted: h7 dominates (ATP +$1.69), h15 secondary (ATP +$1.00), h23 dead. **Opposite of the predicted "h23 dominates → calendar artifact" pattern.** The active hours are session boundaries (Europe open h7, US open h15 UTC), not the funding settlement boundary (h23-h0 UTC).

3. **h7 carries in BOTH train and val periods** — most-stable session-boundary signal identified in the lab to date. Mechanism-reframe partial truth retained at [[trading/mechanisms/session-boundary-effects]] family.

4. **The adversarial-audit verdict from spec was correct in spirit.** Strategy spec flagged this as "cleanest case of confirmation bias if it backtests positive" → CONFIRMED — strategy backtested positive at best config but with mechanism that doesn't match the declared story (settlement-cycle). The pre-registered 3-window falsifier did its job: caught the mechanism mismatch when raw P&L would have looked acceptable.

## Hypothesis (Structured Format)

```
Hypothesis:    Pre-funding-settlement positioning unwind creates
               directionally-biased fwd-1h returns at h23 UTC (W2 raw-
               data finding: fwd-1h −0.045%). Conditioning on a funding+
               bar filter (Q5 extreme funding regime, bar-direction-
               aligned) should concentrate the effect into a tradeable
               4h strategy.

Mechanism:     8h funding settlement clock at h0/h8/h16 UTC creates
               predictable flow at h23 UTC (pre-settlement positioning
               adjustment by leveraged longs avoiding funding cost).
               Top-decile funding regime + reversal-bar trigger should
               filter for the cleanest pre-settlement unwind setups.

Observable:    BTCUSDT perp 1h+4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%. Pre-registered 3-window post-process
               (calendar-artifact falsifier): if h23 is a true settlement
               effect, h23 ATP should dominate h7 / h15 ATP. Strategy-
               level falsifier: deployment-grade gate (net ≥ $120/yr) +
               eligibility ≥80%.

Expected:      h23 ATP > h7 ATP, h15 ATP. Net ≥ $120/yr at $1K capital.
               Both directions positive (long carry-unwind ≈ short
               positioning-extreme).
               expected_net_edge:
                 source: W2 raw-data scan (different time window, no
                         overlap with backtest)
                 value: $50-100/yr at $1K (conservative; raw effect
                        −0.045% × ~250 firings × Q5 conditioning)
                 independence: confirmed — W2 raw scan used 2024-05/
                              2025-04 window; backtest uses 2025-05/
                              2026-04 (no overlap)

Falsifier:     3-window calendar-artifact test: if h7 or h15 ATP > h23
               ATP, settlement-cycle hypothesis rejected.
               Strategy-level: net < $120/yr → deployment fail.
               Direction sanity: if either long_pnl or short_pnl
               negative at median, mechanism direction wrong.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

### Pre-registered falsifier outcomes (all FIRED at minimum 3 of 4)

| Falsifier | Pre-reg threshold | Observed | Verdict |
|---|---|---|---|
| Eligibility | ≥ 80% | 88.9% | PASS |
| Strategy-level deployment gate | net ≥ $120/yr | −$33/yr median; ATP −$0.34 | **FIRED** (both net + ATP) |
| 3-window calendar-artifact (h23 dominance) | h23 ATP ≥ h7+h15 average | h23 = $0.01 ≈ 0% of (h7+h15)/2 = $1.35 | **FIRED** (opposite of predicted) |
| Direction sanity (median) | both directions ≥ 0 | long_pnl $0.00; short_pnl −$22.78 | **FIRED** (short side negative at median) |

### 3-window post-process (load-bearing falsifier; run #1526, best both>0 config)

Run config: funding_lookback=540, extreme_high=0.75, extreme_low=0.05, atr=20, stop=1.0, target=2.0.

| Pre-settlement signal hour | Entry hour | n | ATP | Total |
|---|---|---:|---:|---:|
| **h23** (signal at h23, entry h0) | h0 | 21 | **+$0.01** | +$0.29 |
| **h7** (signal at h7, entry h8) | h8 | 21 | **+$1.69** | +$35.57 |
| **h15** (signal at h15, entry h16) | h16 | 21 | +$1.00 | +$20.98 |

**h23 is essentially 0% of (h7+h15)/2 — opposite of "h23 dominates → calendar artifact" prediction.** The settlement-cycle hypothesis required h23 to carry the signal. Instead, h7 dominates, h15 is secondary, h23 is dead.

### By period (train vs val)

| Period | Hour | n | ATP |
|---|---|---:|---:|
| train | h0 (h23 signal) | 5 | −$1.08 |
| train | h8 (h7 signal) | 10 | +$0.94 |
| train | h16 (h15 signal) | 8 | +$0.25 |
| val | h0 (h23 signal) | 16 | +$0.36 |
| val | h8 (h7 signal) | 11 | +$2.38 |
| val | h16 (h15 signal) | 13 | +$1.46 |

**h7 carries in both train AND val periods.** Most-stable session-boundary signal identified to date in the lab.

## Falsifier verdict: 3 of 4 FIRED

- **Strategy-level (deployment-grade gate)**: net −$33/yr vs ≥+$120/yr threshold + ATP −$0.34 → **FIRED**
- **3-window calendar-artifact (load-bearing)**: h23 ≈ 0% of (h7+h15)/2 → **FIRED in opposite direction** (mechanism reframe surfaces here)
- **Direction sanity**: long_pnl $0.00, short_pnl −$22.78 at median → **FIRED on short side**
- Eligibility passed (88.9%); the mechanism design wasn't broken structurally — it was the wrong mechanism.

Adversarial-audit verdict from spec ("cleanest case of confirmation bias if it backtests positive") fired correctly: positive raw P&L at best config, but mechanism reframe required by 3-window falsifier.

## What partial truth remains?

- **Session-boundary effects at h7 / h15 UTC** — most-stable signal identified to date, robust across train and val. Mechanism reframe load-bearing partial truth retained at [[trading/mechanisms/session-boundary-effects]] (h7/h15 dominance update appended 2026-05-06).
- **W2 raw h23 is unconditionally present (forecast effect) but not tradeable under filter.** Two implications: (1) raw fwd-1h microscale effects don't survive filtering + friction at 4h frame — canonical forecast-vs-tradeable-gap; (2) session-boundary signal at h7/h15 may be funding-co-occurrence-dependent (the funding+bar filter is doing real work, not just noise reduction). The latter is an open research question — does standalone h7 (without funding filter) still carry edge?
- **Top-decile funding regime + bar-reversal trigger filter is structurally sound** (88.9% eligibility); the mechanism it filters for is the wrong one. Reusable filter for session-boundary strategies that need a regime confirmation.

## Reason classification

- [x] **Falsifier fired** — 3 of 4 falsifiers fired (strategy-level + 3-window calendar-artifact load-bearing + direction sanity)
- [x] **Forecast effect doesn't translate** — W2 raw h23 forecast magnitude (~0.045%) is too small to survive friction at 4h tradeable frame; canonical [[foundation/methodology/forecast-vs-tradeable-gap]] case
- [x] **Mechanism reframe to different family** — partial truth retained at session-boundary effects (h7/h15 dominance)
- [ ] No falsifier articulated (clean pre-registration)
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## Trial count on dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥19 strategies tested on BTCUSDT perp 2025-05/2026-04 window so far (13 prior + 5 in 2026-05-05 + ≥6 in 2026-05-06). Falsifier-fired result is robust to multiplicity correction — pre-registered 3-window calendar-artifact falsifier fires independently of statistical-significance gates, and the direction-sanity failure (short_pnl −$22.78 at median) is itself paired-falsifier-grade evidence.

## Caveats per [[foundation/methodology/strategy-validation-discipline|validation discipline]]

- **Single-window backtest** — 2025-05/2026-04 only. Per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], the broader cohort had concentrated edge in Q4 2025 + Q1 2026; settlement-cycle rejection is window-specific. h7 dominance robust across both train (Q2-Q4 2025) and val (Q1-Q2 2026) sub-windows is suggestive of regime-stability but not multi-year confirmed.
- **3-window n=21 each** — small per-window samples. Direction is robust (h7 positive in 100% of train+val both periods); magnitude has wide uncertainty bands.
- **Filter dependency open question** — h7/h15 carries under funding+bar filter; whether it carries WITHOUT filter is the load-bearing follow-up test (`meta_session_v1` strategy spec is the planned answer).

## Key Takeaways

- **Settlement-cycle mechanism (pre-funding-positioning unwind) does NOT replicate at 4h frame under funding+bar filter** in 2025-26 BTC perp regime. W2 raw h23 finding is a forecast effect, not a tradeable strategy.
- **h7 (Europe open) and h15 (US open) UTC carry the signal instead** — h7 robust across BOTH train and val periods (most-stable session-boundary effect in the lab).
- **3-window calendar-artifact falsifier did its job**: caught the mechanism mismatch when raw P&L at best config would have looked acceptable. Pre-registration discipline operated correctly.
- **Mechanism reframe to session-boundary effects** retained as partial truth; strategy class closed for settlement-cycle as defined.
- **Filter dependency open question**: does h7 dominance hold without funding+bar filter? `meta_session_v1` is the planned standalone test.
- **Real learning**: forecast-magnitude raw effects (~0.045%) at sub-bp scale don't survive friction + execution timing translation to a 4h tradeable strategy. Canonical [[foundation/methodology/forecast-vs-tradeable-gap]] case.

## Related

- [[trading/mechanisms/session-boundary-effects]] — partial-truth retained mechanism family; h7/h15 dominance evidence appended 2026-05-06
- [[trading/mechanisms/funding-cycle-dynamics]] — sibling family; pre-settlement flow subsection mostly arbitraged at simple level (this rejection adds: mechanism doesn't survive even with filter)
- [[foundation/methodology/forecast-vs-tradeable-gap]] — canonical translation-loss methodology; this rejection is a clean worked case of L1 forecast → L2 tradeable failure
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Second Law (pre-registration); 3-window falsifier worked cleanly
- [[foundation/methodology/deployment-edge-threshold]] — strategy-level net-of-friction floor fired
- [[trading/mechanisms/scaffold-as-mechanism]] — settlement/session-boundary scaffold catalog entry
- [[foundation/macro/regime-conditional-edge-2025-26]] — regime context
- Apr 28 mechanism families — session-boundary effects is one of the 5 prioritized day/swing families

## Sources

- 2026-05-06 batch stocktake: the backtesting lab (#3 verdict)
- the backtesting lab — strategy spec + pre-registration + grid output
- W2 raw-data scan (2026-05-05): h23 fwd-1h −0.045% finding; the backtesting lab (raw-pattern findings section)
- the backtesting lab — forward-looking standalone session-boundary mechanism test (planned)
