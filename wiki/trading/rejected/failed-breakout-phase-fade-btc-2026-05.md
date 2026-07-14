---
title: "failed_breakout_phase_fade_btc — REJECTED on BTC continuation character (2026-05)"
status: evergreen
created: 2026-05-18
updated: 2026-05-19
tags: [rejected, failed-breakout-reversal, fade-at-extreme, btc-continuation-character, phase-conditioned, mechanism-class-symbol-mismatch, reversed-mechanism, graveyard-rebuild-failure, day-swing]
---

# failed_breakout_phase_fade_btc — REJECTED on BTC continuation character (2026-05)

> **Lead learning**: The phase-conditioned-fade build that salvages the bare turtle-soup rejection on ETH (where fakeout-fade character holds) **FAILS DECISIVELY on BTC**. N=161 trades / Sharpe −1.26 / both directions lose / TP fires only 5/161 times. The mechanism class is geometrically broken on this symbol. **Primary contributor to the BTC continuation character codification** (N=3 evidence; this is the largest-N witness). **Verdict: REJECT on BTC; the failed_breakout_phase_fade mechanism class is symbol-specific to ETH (and the SOL long-only variant is the remaining open question).**

## What we learned

1. **BTC's range-extreme wicks CONTINUE, they don't revert.** N=161 / Sharpe −1.26 with TP firing 5/161 means the mechanism's TP geometry (2.5 × ATR back into range) is essentially never reached. Wicks that "fail" geometrically (close back inside) are STILL followed by continuation in the wick direction more often than reversion. This is the load-bearing structural finding.

2. **Mechanism class is symbol-specific.** Same DSL, same parameters, same regime gate — works on ETH (short side N=77 / +$133), fails on BTC. Cross-symbol non-generalization codification (2026-05-14 research process arms) gets a clean reinforcing case.

3. **"Phase-conditioning as salvage path" doesn't rescue mechanism-symbol mismatch.** The phase data layer's `is_stable=True` gate was the salvage hypothesis for the bare turtle-soup rejection (worldview prior #6 graveyard rebuild). The salvage WORKS on ETH but doesn't transfer to BTC. The lesson is: regime conditioning rescues mechanisms that fit the symbol; it cannot make a wrong-symbol mechanism right.

4. **The reversed-mechanism mutation direction is the actionable salvage.** When BTC wicks through a range extreme and closes back inside, trade IN the wick direction (continuation), not against it. Pending lab build: `phase_breakout_continuation_btc`.

## Hypothesis (Structured Format)

```
Hypothesis:    During stable phases (is_stable=True), intra-bar wicks
               through phase-range running extremes (range_max_so_far /
               range_min_so_far) that close back inside the range are
               failed breakouts. Stop-runners offside; fade the wick
               direction. Phase-conditioning is the salvage path for
               the bare turtle-soup rejection (which used arbitrary
               20-day extremes); the phase data layer provides
               STRUCTURAL levels where liquidation clusters
               concentrate.

Mechanism:     Failed-breakout reversal (Raschke turtle-soup, Schwager
               NMW 1992) + crypto liquidation-cluster visibility +
               phase-data-layer structural levels. Phase gate built
               INTO entry logic per worldview prior #10.

Observable:    BTCUSDT 4h, 2024-05-20 → 2026-05-08, $1000 capital,
               4 bps fee, risk_pct(0.01). Entry on bar where intra-bar
               wick exceeds prior-bar's running extreme AND close
               returns inside. SL 1.0 ATR / TP 2.5 ATR / time stop 12h.

Expected:      Per ETH companion's behavior, cross-symbol generalization
               would give Sharpe 0.5-1.0 on BTC if the mechanism class
               transfers. Per-symbol asymmetry codification (2026-05-15)
               flagged BTC as continuation-character — predicted MISMATCH
               with this mechanism class.

Falsifier:     Sharpe < 0.5 → reject. Per 2026-05-15 per-symbol asymmetry
               prediction, falsifier expected to fire on BTC.
```

**Result**: Sharpe −1.26 (falsifier fires cleanly). N=161. Long 87 / -$77 / WR 47%; Short 74 / -$141 / WR 41%. TP 5 / SL 56 / time_stop 51 / regime_break 49. MDD 23.9%.

**Cause of failure (named)**: BTC range-exit character is **continuation, not reversion**. Wicks through `range_max_so_far` are stop-cluster sweeps that CONTINUE upward; wicks through `range_min_so_far` are stop-cluster sweeps that CONTINUE downward. The "close back inside" geometric trigger is a NOISE bar in a continuation move, not a reversal signal. TP firing 5/161 (3%) demonstrates the mechanism never reaches its target — the wick-close-back is followed by another wick in the same direction, not by reversion back into the range.

This isn't a parameter problem (tighter SL / wider TP / different time stop don't change the underlying continuation character). It's a symbol-mechanism class mismatch.

## Why this confirms (not just rejects) {#confirmation-not-just-rejection}

The strategy's hypothesis docstring already FLAGGED the predicted mismatch:

> **Cross-symbol asymmetry alignment (from strategy .md)**:
> | Symbol | Range-exit character | Mechanism alignment |
> | BTC | Continuation | **MISMATCH** — failed breakouts rarer; mechanism poorly aligned |
> | ETH | Fakeout-fade | **ALIGNED** — failed breakouts structurally common |

The lab result confirms the prediction at higher confidence than the prior would have suggested. Together with `phase_breakout_meta_btc` (continuation triggers work, PSR 97.5%) and `busy_hour_flow_continuation_btc` (inverted by phase — works in unstable trending regime), this is N=3 cross-mechanism evidence for the BTC continuation character codification. **See [[../../foundation/asset-classes/btc-market-structure]] for the load-bearing codification.**

## What this DOESN'T reject {#preserved-not-rejected}

- **The ETH companion** — `failed_breakout_phase_fade_eth` (Sharpe +0.60 aggregate, short side carries +$133/N=77/WR 51%). Different symbol, different character, mechanism works.
- **The phase data layer** — `is_stable`, `range_max_so_far`, `range_min_so_far` are mechanically clean. The data layer didn't fail; the mechanism choice failed.
- **The bare turtle-soup mechanism class on OTHER symbols** — partial-truth on short-side asymmetry retained from [[turtle-soup-4h-2026-05]]. Not all variants are dead; just BTC-targeted ones.
- **Salvage via reversed-direction mutation** — `phase_breakout_continuation_btc` is a NEW candidate, not a re-proposal of this rejection. When wick exceeds + close-back, trade IN the wick direction. Worldview prior #6 obligation met by the directionality reversal.

## Cross-symbol comparison {#cross-symbol}

| Strategy | Symbol | Sharpe | N | WR | Long | Short | Reading |
|---|---|---:|---:|---:|---|---|---|
| failed_breakout_phase_fade_btc | BTC | **−1.26** | 161 | 44% | -$77 / 47% | -$141 / 41% | Both legs lose; continuation character |
| failed_breakout_phase_fade_eth | ETH | +0.60 | 166 | 49% | -$17 / 48% | +$133 / 51% | Short carries; fakeout-fade character |
| failed_breakout_phase_fade_sol | SOL | -0.11 | 162 | 45% | +$51 / 50% | -$83 / 41% | Long ONLY positive; long-bias regime amplifier |

The three results align with per-symbol asymmetry framework:
- BTC continuation → both directions fail (geometric mismatch)
- ETH fakeout-fade → short side works (down-breaks reverse predicted; up-breaks fade work for shorts)
- SOL long-bias regime amplifier → long side works (down-breaks reverse upward predicted; long-only salvage candidate)

## Mutation directions {#mutation-directions}

1. **Reverse the mechanism direction** (highest-EV) — `phase_breakout_continuation_btc`. When wick exceeds the range running extreme AND close returns inside, trade IN the wick direction (continuation hypothesis). Test against the BTC continuation character prior; if it works, the codification gets a 4th evidence point.

2. **Time-of-day overlay** — could BTC fade mechanism work in specific session windows (US close, Asia open)? Adversarial: probably not, since structural character doesn't shift by session. But cheap to test if a session-window strategy gets built.

3. **Cross-regime test on bear data** (2022-04 → 2023-09) — bear regimes have more flush-and-recover behavior; the fade mechanism MIGHT work in bear BTC. Gated on bear BTC parquet with phase columns merged.

4. **DROP THE FAMILY ENTIRELY on BTC.** Worldview prior #2 (AI edge baseline ≈ 0) + N=3 BTC continuation evidence + ETH-specific salvage path: BTC range-extreme-fade is a wrong-symbol mechanism class. Resources better spent on continuation triggers (phase_breakout_meta_btc family expansion).

## Methodological notes {#methodology-notes}

- **Cross-symbol simultaneous test was decisive** — running the same DSL on BTC + ETH + SOL exposed the per-symbol asymmetry in one batch. Without cross-symbol comparison, ETH's positive result might have looked like a general mechanism finding; BTC's negative result might have looked like a parameter-tuning problem. The cross-symbol matrix made both clear in one shot.
- **TP fire rate is the diagnostic signal** — 5/161 (3%) is the single most informative number in this rejection. Healthy mechanisms have TP firing 15-40% of trades; a healthy mean-reversion mechanism specifically should be in 30-50% TP range (matched up wins). 3% TP fire = mechanism's exit assumption is geometrically untenable. **Generalizable methodology**: when post-decomposing a backtest verdict, TP-vs-SL fire rate at the strategy level is a fast diagnostic for mechanism-vs-implementation problems.
- **Hypothesis docstring flagged the mismatch BEFORE backtest** — the .md file already had a "CROSS-SYMBOL ASYMMETRY ALIGNMENT" table predicting BTC failure. The strategy was BUILT as an ETH-targeted strategy with BTC as a deliberate "cross-symbol test would expose ETH-only character" companion. The rejection confirms the hypothesis-level prediction, which is high-confidence learning even at single-window evidence.

## Related artifacts

- Strategy package: the backtesting lab (moved to tested 2026-05-18)
- Companion (working on ETH): the backtesting lab
- Companion (open question on SOL): the backtesting lab (long-only ablation pending P1.2)
- Backtest artifacts: the backtesting lab + `trades.csv` + `equity.csv`
- Predecessor: [[turtle-soup-4h-2026-05]] (bare mechanism, salvage attempt produced this BTC failure)

## Related

- [[../../foundation/asset-classes/btc-market-structure]] — codification this rejection feeds (N=3 evidence)
- [[turtle-soup-4h-2026-05]] — graveyard predecessor
- [[auction-theory-range-fade-2026-05]] — related family rejection (mechanism class also fails on BTC per detector continuation character)
- [[../mechanisms/failed-breakout-reversal]] — mechanism page; BTC-character override noted here
- [[../mechanisms/scaffold-as-mechanism]] — cross-symbol non-generalization codification anchor
- [[../../foundation/asset-classes/sol-market-structure]] — per-symbol asymmetry framework sister page

## Sources

- 2026-05-18 14-strategy batch (the backtesting lab)
- Per-symbol asymmetry prediction: [[../../foundation/asset-classes/sol-market-structure]] framework
- Hypothesis docstring (predicted mismatch BEFORE backtest): `strategies/tested/failed_breakout_phase_fade_btc/failed_breakout_phase_fade_btc.md`
- Cross-symbol decomposition: the research task queue Tier 3 section
