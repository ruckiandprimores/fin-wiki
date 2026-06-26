---
title: "Auction-theory range-fade family — rejected on team's stable-range detector (v1 + v2; mutation-1 partial coverage added 2026-05-19)"
status: evergreen
created: 2026-05-15
updated: 2026-05-19
tags: [rejected, range-fade, auction-theory, mean-reversion, stable-range-detector, poc-magnet, continuation-vs-reversion, day-swing, phase-conditioned, n2-family-rejection, mutation-direction-1-partially-tested, edge-saturation-character]
---

# Auction-theory range-fade family — rejected on team's stable-range detector (v1 + v2; mutation-1 partial coverage added 2026-05-19)

> **Lead learning**: The team's stable-range phase detector (`RollingZigzag + atr_mult=0.3 + threshold=65`, deployed 2026-05-15) finds consolidations with **continuation-at-extremes character**, NOT reversion-at-extremes character. Naive auction-theory range-fade mechanisms — both VA-edge-to-POC (v1) and literal-range-extreme-to-opposite-extreme (v2) — reject. Win rate at new extremes is ~50% (continuation and reversion equally likely); regime-break exit ablation (v2b) confirms there's no slow-reversion edge being cut short. **Verdict: REJECT auction-theory range-fade family on this detector as currently tuned.** Distinct from [[range-fade-4h-2026-05]] (Bollinger envelope range-fade, rejected as regime-absent / mechanism-untested) — this rejection is mechanism-tested-and-rejected on a different regime-identification layer.
>
> **As of 2026-05-19**, one out-of-naive-form operationalization of mutation-direction #1 — `range_tunnel` (Wyckoff multi-bar + `taker_buy_base_vol / volume` z-score flow filter, v1 12h/2.5 ATR + v2 48h/1.5 ATR exit configs) — has also been tested and rejected via the v1→v2 mutation cycle (see [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]]). **Cumulative: 4 strategies tested across 2 trigger families on this detector × ETH × 4h × bull window corner; family-level rejection requires further mutation-direction-#1 variants to be tested** (see updated "Mutation directions" section).

## What we learned

1. **The detector defines "stable" via 70% value-area bounds (TPO-derived), not literal range bounds.** Price exits the value-area bounds **~45%** of the time during stable phases (BTC 45.2% / ETH 42.3% / SOL 47.0%). Fading VA edges is NOT fading literal range extremes.

2. **Even with the geometric correction, mean-reversion edge is absent at new period extremes.** v2 used period-running cumulative `max(high)` / `min(low)` — fade fires when price prints a new period-extreme. Win rate stayed ~50% across BTC/ETH/SOL. Designed R:R was ~3.3 (vs v1's ~0.5) but the win-rate hurdle wasn't met (~50% vs need 23%+).

3. **Regime-break exit (`~is_stable`) is damage control, not edge-cutting.** Ablation test (v2b): removing the regime-break exit made BTC slightly better (-0.22 → 0.00 Sharpe), ETH slightly worse (-0.81 → -0.94), and SOL substantially worse (+0.28 → -0.26). The 38-44% of trades exiting on regime-break were predominantly losing-direction trades being cut early.

4. **Detector behavior implication** {#detector-continuation-character} — when price stretches to new period extremes during "stable" phases, the period tends to END (continuation breakout) rather than REVERT. The detector's `is_stable=True` correlates with **"consolidation that will break when stretched,"** not with "consolidation that will revert when stretched." This is a fundamental insight about what the detector identifies and contradicts the naive auction-theory prior.

5. **Mutation-direction #1 (retest + rejection) partially tested via range_tunnel multi-bar + `taker_buy_z` form (v1 + v2, 2026-05-18/19).** Wyckoff spring/upthrust with multi-bar pattern + Effort-vs-Result `taker_buy_z` flow filter + ATR-based exits at two configs (12h/2.5 ATR + 48h/1.5 ATR) produces mechanism-real-but-time-limited drift on ETH `is_stable=True` phase. v1 bidirectional: both legs falsifier-failed; "mechanism-real-but-clipped" was the initial diagnosis. v2 short-only with extended time-stop: exit-config sub-falsifiers all passed, but leg-level Sharpe collapsed to 0.18 (walk-forward aggregate −0.14). **Discovered: edge-saturation character** — mechanism produces ~3-bar positive drift then mean-reverts. **This is one specific operationalization of mutation-direction #1, not the full direction.** See [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] for the full mutation arc + explicit untested-variants list.

## Hypothesis (Structured Format) — v1

```
Hypothesis:    During detector-confirmed stable-range phase (is_stable=True),
               fade range extremes back toward Point of Control (POC).
               Auction-theory mean-reversion: POC = time-weighted-acceptance
               price magnet; range edges are liquidity-pool / stop-hunt
               rejection zones.

Mechanism:     Layered (a) auction-theory: POC magnetism within value
               area; (b) liquidity-pool crypto overlay: stop-loss clusters
               just outside range_high/range_low get absorbed back into
               range on touch.

Observable:    BTC/ETH/SOL perp 4h, 2024-05-20 → 2026-05-08, $1000 capital,
               4 bps fee. Entry: long when close ≤ range_low (is_stable=True),
               short when close ≥ range_high. TP = poc_24h frozen at signal.
               SL = 1.5 × ATR_14. Time stop 12 bars. Regime-break exit.

Expected:      Mean-reversion produces +0.5-1.5% per trade. Sharpe 0.8-1.5
               per symbol on a single instrument.

Falsifier:     Sharpe < 0.5 on any single symbol with default parameters,
               OR cross-symbol generalization shows >50% drop from best to
               worst symbol.
```

**v1 result**: BTC -0.67, ETH -0.97, SOL -1.50 Sharpe. **Falsifier fired symmetric** — all 3 symbols failed with same shape.

**v1 cause of failure (named)**: VA-vs-range semantic confusion. `range_low/range_high` are 70% value-area bounds (TPO-derived), not literal range bounds. POC sits at midpoint of VA. TP (entry → POC) ≈ 0.9 ATR; SL = 1.5 ATR; **R:R = 0.52, breakeven WR = 65.8%, observed WR 55%.** Geometrically broken before mechanism was tested.

## Hypothesis (Structured Format) — v2

```
Hypothesis:    Same auction-theory mean-reversion as v1, but using LITERAL
               range bounds (period-running cumulative extremes) instead
               of value-area bounds.

Mechanism:     Same as v1. Geometric fix is the only change.

Observable:    Same data + capital + fees. Entry: long when this bar's
               close prints a new period-LOW (close < SHIFT(running_min, 1))
               AND is_stable=True. Short symmetric. TP = opposite running
               extreme frozen at signal. SL = 1.5 × ATR_14. Time stop 12.
               Regime-break exit.

Expected:      Period-running width ~5.3 ATRs (median); TP move ≈ full range
               width; R:R ≈ 3.3; needs ~23% WR to break even.

Falsifier:     Sharpe < 0.5 on any single symbol with default parameters
               (geometry now favors mechanism; if it still rejects, mechanism
               is more deeply broken than v1's confusion).
```

**v2 result**: BTC -0.22, ETH -0.81, SOL +0.28 Sharpe. **Falsifier fires on BTC and ETH**; SOL marginal positive but below the 0.5 bar.

**v2 cause of failure (named)**: No reversion edge at new period extremes. Win rate stayed ~50% despite favorable geometry. 38-44% of trades exited on regime-break before TP — periods END shortly after price stretches to new extremes (continuation behavior, not reversion). Ablation test (v2b removing regime-break exit) confirmed: without the damage-control exit, the same continuation-driven trades just run to SL or time_stop — net Sharpe drops further on 2 of 3 symbols. **The mechanism class doesn't fit the detector's "stable period" character.**

## Why this rejects the mechanism FAMILY, not just one expression {#family-rejection}

Two semantically-distinct naive-form expressions (VA-fade-to-POC and literal-range-fade-to-opposite-extreme) on the same data layer, **plus a third trigger family (Wyckoff multi-bar + `taker_buy_z` flow filter) tested across two exit configs — total 4 strategies on the same detector × ETH × 4h × bull window corner**, all failing with overlapping symptoms (low win rate at extremes for the naive forms; edge-saturation character for the Wyckoff form). This gives **strong evidence for the "consolidation-before-continuation" detector character on the tested corner.**

**The detector's `is_stable=True` regime is not a mean-reverting regime. It is a "consolidation-before-continuation" regime.**

A range-fade strategy is by definition betting on mean-reversion at extremes. If extremes are predominantly continuation events, range-fade has no edge regardless of TP/SL geometry, entry buffer, or exit-policy mutations.

**Scope caveat (added 2026-05-19)**: this is family-level rejection for the corner tested (single detector × symbol × TF × bull window) with TWO trigger families and FOUR strategies. **Family-level rejection ACROSS the FULL mutation-direction-#1 space** would require coverage of: alternative flow filters (volume_z, CVD, OI-delta, on-chain), alternative multi-bar patterns, alternative exit configs (regime-break, scale-out, mid-range time-stops), alternative timeframes (1h, daily), and alternative detector tunings. **The detector's continuation-character finding is robust for the 2 operational forms tested; broader claim requires broader coverage.**

## What WOULD make range-fade work on this detector (not tested) {#mutation-directions}

Mutation directions for future range-fade proposals:

1. **Different entry trigger** — wait for new-extreme + bounce-back (retest + rejection), not enter ON the new-extreme bar. Filter out continuation-bound new extremes by requiring a bar of consolidation post-touch.

   **PARTIAL COVERAGE 2026-05-18/19**: One specific operationalization — Wyckoff multi-bar + `taker_buy_base_vol / volume` z-score flow filter + ATR-based exits at 12h/2.5 ATR (v1) + 48h/1.5 ATR (v2) — TESTED AND REJECTED via [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]]. The operational form exhibits **edge-saturation character** (real K≈3-bar drift, mean-reverts afterward).

   **Variants of mutation-direction #1 that remain UNTESTED** (valid future candidates — do NOT fire prior #6 against these without operationalizing them):
   - Different flow filter — `volume_z` alone, CVD divergence, OI-delta, funding-rate divergence, on-chain whale flow, multi-filter stacks
   - Different multi-bar pattern — 3-bar (touch + 2-bar recovery), 5-bar, body-vs-wick definitions, intra-bar timing
   - Different exit logic — regime-break exit on `~is_stable` transition, scale-out partial profits, mid-range time-stops (24h, 36h), patient SL
   - Different SL geometry — ATR-adaptive (tighter in low-vol), regime-aware
   - Different timeframe — 1h, 2h (faster than v1's K=3-bar saturation might suggest the mechanism's natural timeframe), or daily (closer to Wyckoff primary timeframe)
   - Cross-regime — bear-window ETH (currently gated on bear ETH parquet with phase columns merged)
   - Different symbol within mutation-direction #1 (BTC variant in OPPOSITE direction per continuation character; SOL variant — neither tested at multi-bar form)
   - Different detector tuning (team-side work; also noted in mutation directions 2 + 3 below)

2. **Detector re-tune (team work)** — current `RollingZigzag + atr_mult=0.3 + threshold=65` produces continuation-character periods. A different tuning (tighter threshold, larger atr_mult, alternate detector) might find reversion-character periods. This is a team conversation, not a research process mutation.

3. **Compound regime filter** — combine `is_stable=True` with another signal that specifically identifies reverting setups (e.g., extreme funding rate divergence, low vol_compression, taker_buy ratio at extremes). The data-layer's `is_stable` alone isn't a reversion gate; pairing it with one might unlock the family.

4. **Drop the family entirely on this detector.** If team detector re-tune doesn't happen, the data layer is more useful for other mechanism classes (regime-gating trend strategies, breakout detection, coil-expansion) than for range-fade.

## Cross-symbol observations

- **v1 failed symmetric** (all 3 negative similarly) — VA-vs-range confusion is symbol-independent
- **v2 showed asymmetry**: SOL marginally positive (+0.28), BTC near-zero (-0.22), ETH worst (-0.81). Possible interpretation: SOL stable periods may have slightly more mean-reversion-after-stretch character than BTC/ETH; but the effect is below the 0.5 falsifier bar so the asymmetry isn't deployment-relevant on its own. **Subsequent structural framing (2026-05-18)**: SOL's marginal-positive v2 is consistent with the broader per-symbol asymmetry codified in [[../../foundation/asset-classes/sol-market-structure|sol-market-structure]] — SOL's bull-regime structural-absorption flows produce long-bias on BOTH break directions (down-breaks reverse upward), which is geometrically what v2 short-on-new-low captured marginally. The mechanism is not "range-fade works on SOL" but "any down-break-fade-with-upward-target works on SOL in this regime" — direction-restriction sub-case rather than rescue of the auction-theory framing.
- **Detector params identical across symbols** (no per-symbol tuning by team) — the cross-symbol asymmetry in v2 reflects real differences in how each symbol's stable phases behave, not differences in detector calibration

## What this DOESN'T reject {#preserved-not-rejected}

- **The data layer itself** (`is_stable`, `range_low/high`, `poc_24h`, `range_max_so_far`, `range_min_so_far`) — mechanically clean, well-specified, no look-ahead
- **Regime-gating (using `is_stable` as a binary filter on OTHER strategies)** — separate mechanism class, untested at the time of this rejection
- **Coil-expansion mechanisms** (trading the END of stable phases as continuation breakouts) — actually CONSISTENT with this detector's character finding per `{#detector-continuation-character}`; worth future test
- **Future range-fade with different entry trigger** (see `{#mutation-directions}` above)

## Methodological notes {#methodology-notes}

- **Cross-symbol comparison was decisive**: per `cross-symbol-non-generalization` codification (2026-05-14 research process arms). v1 + v2 were tested simultaneously on BTC/ETH/SOL with identical DSL logic. Without the multi-symbol test, v2's SOL marginal positive could have looked like a deployment candidate; the cross-symbol failure pattern made the rejection robust.
- **Ablation testing matters**: v2b (regime-exit ablation) ruled out a specific failure hypothesis (regime-break exit cutting profitable trades). Without the ablation, the rejection would be less crisp.
- **Semantic verification of new data layers** {#semantic-verification-lesson} — v1's failure was driven by misinterpreting what `range_low/range_high` meant. The first sanity check after a new data layer lands should be: *"what do these columns actually represent vs what I assume they represent?"* — 30 minutes of column-semantics verification would have caught v1's geometric problem at idea-spec time. **Generalizable methodology learning**: new data layers warrant a semantic-verification pass before mechanism-translation, alongside the existing look-ahead / column-alignment checks per [[../../foundation/methodology/strategy-validation-discipline#3-the-seven-sins-baseline-hygiene|strategy-validation-discipline §3]].

## Related artifacts

- Strategy packages (rejected): the backtesting lab, `range_fade_v2/`, `range_fade_v2b_no_regime_exit/`
- Data-layer source: `finance/stable-range-phase/` (team's detector code) + the backtesting lab{btcusdt,ethusdt,solusdt}/` (deposited 2026-05-15)
- Data-layer integration: 8 columns merged into the backtesting lab{symbol}/futures/4h.parquet` via forward-fill + period-running extremes computation; lab loader whitelist extended at `strategy_lab/data/loader.py`

## Related

- [[range-fade-4h-2026-05]] — companion range-fade rejection (Bollinger envelope, rejected regime-absent / mechanism-untested in 2025-26 window). This page is the **mechanism-tested-and-rejected** counterpart on the team's detector.
- [[range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] — **one operationalization of mutation-direction #1** (Wyckoff multi-bar + `taker_buy_z` flow filter, v1 + v2) tested-and-rejected; full mutation arc + new mechanism-class learning (edge-saturation character) + explicit untested-variants list. **Sharpens the partial-coverage scope of mutation-direction #1.**
- [[../mechanisms/scaffold-as-mechanism]] — range-fade family rejection on this specific detector adds **N=1 negative-corollary evidence** for the scaffold codification: not all regime-gate signals translate to mean-reversion edge; regime labels carry mechanism-character that the strategy must match
- [[../../foundation/methodology/strategy-validation-discipline]] — v1→v2→v2b progression illustrates the semantic-then-mechanism rejection sequence + cross-symbol non-generalization discipline
- [[../mechanisms/regime-gate-vs-bias-filter]] — companion-page on regime-gating-as-design-pattern; the detector's continuation-character finding informs which mechanism classes the detector PAIRS with cleanly (coil-expansion / breakout) vs poorly (mean-reversion / fade)

## Sources

- 2026-05-15 research process session output (v1 + v2 + v2b ablation testing)
- Auction theory / Wyckoff range mechanics: Wyckoff trading range, Harris microstructure POC-as-magnet framing
