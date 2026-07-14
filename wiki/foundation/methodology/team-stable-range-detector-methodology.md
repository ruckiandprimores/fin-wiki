---
title: "Team Stable-Range Phase Detector — Methodology & Refinement"
status: growing
created: 2026-05-21
updated: 2026-05-21
tags: [foundation, methodology, data-layer, regime, phase-detection, stable-range, btc, strategy-discipline, day-swing]
---

# Team Stable-Range Phase Detector — Methodology & Refinement

> **TL;DR**: The team's stable-range phase detector (in `finance/stable-range-phase/`) is the **data-layer dependency** for ≥6 lab strategies (regime_v1/v2/v3, cell_accumulation_long_btc_v2, range_fade_v2, meta_btc cells). It uses a two-stage algorithm: Stage-1 TPO-based stability scoring on 5m bars with daily evaluation + `merge_gap_days=1` adjacency merging; Stage-2 ZigzagRegression then Mann-Kendall classification for the gaps. **Two structural issues identified 2026-05-21**: (1) 28% of phases labeled UNDEFINED despite median POC shift 4.23%; (2) 73% of "stable" periods contain multi-range directional moves — only 25.4% of 4h bars inside "stable" labels are actually in `[range_low, range_high]`. **Refinement validated 2026-05-21**: POC-distance merge criterion + bar-level validation overlay + Option-A undefined reclassification. Without the refinements, `regime_v1` caught falling knives in merged-bear periods (-$472 net); with them, `regime_v3` cleared falsification for the first time in the regime_v* arc (Sharpe 0.97, PSR(>0) 88%, +$384). This page is the canonical reference for ANY strategy gating on the team's `is_stable` / `in_range` / `range_low` / `range_high` columns.

## What this detector does

Identifies stable price ranges in 5m OHLCV via TPO-based stability scoring; classifies the periods BETWEEN stable ranges as bull / bear / undefined.

**Outputs:**
- `stable_range_periods.csv` — period-level rows (start, end, range_low, range_high, poc_24h_entry, is_stable)
- `stable_range_daily_obs.csv` — daily granularity observations
- `stable_range_backtest.csv` — 190 phases at intra-day granularity for BTC

**Consumers in lab:** regime_v1, regime_v2, regime_v3, cell_accumulation_long_btc_v2, range_fade_v2, meta_btc cells (3-of-6 currently use the layer; the rest reference adjacent fields).

## Two-stage classification algorithm

### Stage 1 — StablePhaseDetector (`stable_phase/detector.py`)

- **TPO-based stability scoring** — 24h × 72h × 168h value-area overlaps + variance
- **Daily evaluation** at UTC midnight using past 168h of 5m data
- **Threshold**: `stability_score ≥ 65` → stable
- **Exit hysteresis**: 2 consecutive non-stable days
- **Time-based merge**: `merge_gap_days=1` — adjacent stable sub-ranges with ≤1 calendar-day gap are joined into one merged period. **This is the concept-level issue** documented in §"Issue 2" below.

### Stage 2 — CombinedDetector for gaps (`trend_phase/combined.py`)

For each gap between consecutive stable periods:

- **ZigzagRegression for first 152h** of the gap:
  - ATR-based zigzag (`deviation = ATR × atr_multiplier`)
  - Needs N=5 confirmed extremums to classify
  - OLS regression on 5 extremums → BULL if slope > 0 else BEAR
  - Returns UNDEFINED if fewer than N=5 extremums confirm
- **Mann-Kendall + Theil-Sen beyond 152h**:
  - `p < 0.05` required for non-UNDEFINED
  - Theil-Sen slope sign → BULL / BEAR

## Known structural issues

### Issue 1 — Undefined phases hide directional moves {#issue-undefined-phases}

53 of 190 phases (28%) in BTC 2024-2026 are labeled UNDEFINED. But:

- **Median POC change** between adjacent stable periods bounding the undefined gap: **4.23%**
- 49 of 53 are short (≤44h) — Stage-1 didn't reach the 5-extremum threshold
- These are **NOT "true sideways"** — they are statistically unconfirmed transitions that nevertheless carry directional content

Strategies that drop UNDEFINED bars without inspection lose 28% of available bars to a signal that's mostly directional in substance.

### Issue 2 — Merged "stable" periods hide multi-range moves {#issue-merged-stable}

The team's `merge_gap_days=1` merge logic:

- **Keeps** the FIRST sub-range's `range_low` / `range_high` / `poc_24h_entry`
- **Extends** `exit_date` to LAST merged sub-range's exit

Empirical result on BTC 2024-2026 (52 stable periods):

| Diagnostic | Count | Share |
|---|---:|---:|
| Periods where exit price is OUTSIDE the reported range | **42 / 52** | **80.8%** |
| Periods that moved more than their range width | **38 / 52** | **73.1%** |
| Worst case: 2025-02 → 2025-04 (72d "stable") | -18% price move | 3% range width |

At the 4h bar level inside these "stable" periods (3,177 bars total):

| Bar-level state | Count | Share |
|---|---:|---:|
| `stable_anchored` (close ∈ `[range_low, range_high]`) | **808** | **25.4%** |
| `stable_breaking_up` (close > `range_high`) | 1,062 | 33.4% |
| `stable_breaking_down` (close < `range_low`) | 1,307 | 41.1% |

**Three quarters of "stable"-labeled bars are NOT actually in their reported range.** This is the load-bearing finding for strategy authors gating on the raw label.

### Cross-regime confirmation — bear window (2022-04 → 2023-09)

Phase 4 audit on the bear parquet (2,310 bars labeled `stable`):

| Bar-level state | Count | Share |
|---|---:|---:|
| `stable_anchored` | 858 | 37.1% |
| `stable_breaking_up` + `stable_breaking_down` | 1,452 | 62.9% |

Pattern holds across regime types: bar-level mismatch is structural to the merge logic, not a bull-window artifact. The bar-anchored share is modestly higher in bear (37% vs 25%) but still <half — the data-layer issue is robust across regimes.

## Refinement approach (validated 2026-05-21)

### Fix 1 — POC-distance merge criterion {#fix-poc-distance-merge}

Replace `merge_gap_days=1` with:

> Merge adjacent stable sub-ranges only if `|next.poc - cur.poc| / cur.poc < 0.05` (5%) AND temporal gap ≤ 3 days.

Result on BTC 2024-2026: 90 raw unmerged sub-ranges → 47 merged periods (vs team's 52 with time-based merge). The new merge logic refuses to join sub-ranges that have drifted >5% in price even if they're temporally adjacent.

### Fix 2 — Bar-level validation overlay {#fix-bar-level-validation}

Even with POC-based merge, individual bars can have close outside the merged range. Per-bar check:

| Bar state | Condition | Interpretation |
|---|---|---|
| `stable_anchored` | `close ∈ [range_low, range_high]` | TRUE stability — fade / mean-reversion mechanism applicable |
| `stable_breaking_up` | `close > range_high` | merged-bull artifact / live breakout — breakout-direction strategies apply |
| `stable_breaking_down` | `close < range_low` | merged-bear artifact / live breakdown — breakdown-direction strategies apply |

Strategies needing TRUE adherence to the stability claim (range-fade, mean-reversion-at-bounds) should gate on `stable_anchored` only. Strategies that want to capture the merged-artifact drift as a directional signal (regime_v3's `breakup_entry` / `breakdown_entry` subs) can gate on the `breaking_*` flavors. See [[bar-level-validation-overlay]] for the generalized pattern.

### Fix 3 — Option-A undefined reclassification {#fix-undefined-reclassification}

For each gap labeled UNDEFINED, compare adjacent stable POCs:

- `next.poc > prior.poc × 1.01` → reclassify as `bull_inferred`
- `next.poc < prior.poc × 0.99` → `bear_inferred`
- Otherwise keep as UNDEFINED (truly sideways)

Result on BTC: 44 of 53 undefined phases reclassified (83%); 9 stay UNDEFINED (the genuine sideways gaps).

## When to use the refined layer

ANY strategy gating on `is_stable` / `in_range` / `range_low` / `range_high` from the team's CSVs should use the refined data layer.

- **Refined parquet**: `data/btcusdt/futures/4h.parquet` (post-2026-05-21)
- **Backup** (original team-only): `4h_backup_pre_team_phase_v2.parquet`
- **Refinement script**: `strategy_lab/scripts/detector_team_phase_refined.py`

## Falsification evidence — what happened without the refinements {#falsification-evidence}

| Strategy | Data layer | Result |
|---|---|---|
| `regime_v1` (fades gated on raw `is_stable`) | team-only, no bar-level overlay | **-$472 net**; falling-knife trades in merged-bear periods (price near `range_low` because mid-crash, not bouncing) |
| `regime_v2` (sweep 6561 configs on raw layer) | team-only | train-val correlation **0.015**, DSR 0.8%, **0.23% of configs positive** |
| `regime_v3` (fades gated on `team_is_stable_anchored`) | refined layer | smoke Sharpe **0.97**, PSR(>0) **88%**, **+$384 net** — first regime_v* to clear falsification bar |

Same period-level label, opposite outcomes. **Bar-level validation IS the load-bearing fix.** Per-mechanism reasoning: fade strategies depend on TRUE stability (price bouncing within a range). Gating fades on the raw `is_stable` label means 74.6% of fade entries fire in bars where price has actually moved out of range. Gating on `team_is_stable_anchored` means only 25.4% of formerly-eligible bars qualify, but those are the bars where the mean-reversion mechanism is **actually applicable**.

## Methodological notes {#methodology-notes}

- **Period-level labels are computed retrospectively over a window** (a phase, a regime). They describe what the window WAS as a whole. They do NOT make bar-time claims about the window's interior. This page documents one instance of a generalizable failure mode — see [[bar-level-validation-overlay]] for the cross-mechanism abstraction.
- **The fix is data-layer, not strategy-layer**. Once the parquet carries `team_is_stable_anchored`, downstream strategies inherit the fix without per-strategy logic changes. The 1.5 days of regime_v1/v2 falsification work was spent reasoning about strategy mechanics when the bug was upstream.
- **Forecast-quality vs trade-quality gap (per [[forecast-vs-tradeable-gap]])**: the team's period-level signal has high forecast quality at the period level — when the period is labeled stable, the period IS stable as a window-aggregate. The trade-quality gap is at the bar level — the signal doesn't gate which bars inside the window are usable.

## Open empirical questions

1. **Does the bar-anchored ratio hold cross-asset?** BTC shows 25% bull / 37% bear. ETH and SOL parquets have the same team-detector layer but no audit yet. Cross-symbol test: would inform whether the refinement transfers symbol-by-symbol or needs per-symbol calibration.
2. **POC-distance threshold sensitivity** — the 5% threshold is from a single calibration on BTC bull window. Bear window has wider price swings; would 7% or 10% produce cleaner merges in bear? Capability gap: no sensitivity sweep yet.
3. **Stage-1 stability-score threshold** — `≥65` is the team's default. Strategy authors haven't tested whether `≥75` (stricter) or `≥55` (looser) produces materially different bar-anchored ratios at the cost of fewer/more labeled periods.
4. **Bear-regime breaking-up vs breaking-down asymmetry** — bull window shows 33.4% breaking-up / 41.1% breaking-down (slight downside bias); bear window asymmetry not yet computed. Would inform whether the breakup_entry / breakdown_entry subs have asymmetric expected edge in bear regimes.

## Related

- [[online-regime-detection]] — **added 2026-06-07**: this detector solves the *smoothing* (retrospective) problem — a range is confirmed bull/bear only after forward extremums (Zigzag N=5) and full-gap Mann-Kendall print. Live deployment needs the *filtering* (prospective) problem (label callable at the range's start from past data only). That page names the upgrade path (re-grade existing labels in filtering mode + measure the haircut; score-driven BOCPD / signature-MMD as online-native alternatives) and is the methodological backbone for the team's retrospective-classifier live-deployment gap.
- [[bar-level-validation-overlay]] — generalized methodology pattern; this page is the primary worked example
- [[../../trading/mechanisms/regime-gate-vs-bias-filter]] — bar-level state IS the validated bias filter layer; period-level state is the candidate that needs validation
- [[strategy-validation-discipline]] — pre-deployment audits should include bar-level validation checks on any period-level signal consumer
- [[../../trading/mechanisms/scaffold-as-mechanism]] — strategies sharing this data layer cluster as a scaffold; bar-level overlay is a clean architectural fix that doesn't require per-strategy work
- [[forecast-vs-tradeable-gap]] — period-level forecast quality ≠ bar-level trade quality; this detector is a canonical worked example
- [[../../trading/mechanisms/breakout-direction-asymmetry-by-macro]] — uses the refined data layer; breakout subs gate on `team_is_stable_breaking_up` / `team_is_stable_breaking_down`

## Sources

- Detector source: `finance/stable-range-phase/{stable_phase/detector.py, trend_phase/combined.py}`
- Refined detector script: `strategy_lab/scripts/detector_team_phase_refined.py`
- Audit + Phase 4 cross-regime validation: the research task queue
- Falsification evidence: the backtesting lab, `regime_v2/grid_results_1y/`, `regime_v3/`
- Refined parquet + backup: the backtesting lab{4h.parquet, 4h_backup_pre_team_phase_v2.parquet}`
