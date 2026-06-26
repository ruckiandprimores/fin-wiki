---
title: "Universal Stop-Target Prior — Empirical Parameter Framework"
status: growing
created: 2026-05-05
updated: 2026-06-11
tags: [meta, parameter-prior, empirical-framework, design-discipline, day-swing, w2-finding, stop-target, atr, mechanism-family-conditional, rr-by-family, regime-conditional-exception, holding-horizon, noise-stop]
---

# Universal Stop-Target Prior — Empirical Parameter Framework

> **TL;DR**: Across 13 strategies × top-Sharpe-quartile parameter modes (W2 cross-strategy analysis 2026-05-05), an empirical pattern emerges: **mean-reversion mechanisms cluster at stop=0.5–1.5×ATR (tight); trend mechanisms cluster at stop=2.5×ATR (wider); target=3.0×ATR is universal** across mechanism classes (8 of 11 strategies). The pattern matches mechanism speed: mean-reversion expects fast resolution → tight stops match; trend expects sustained move → wider stops absorb pullbacks. Universal target ≈ R/R 2:1 to 6:1 depending on stop, which fits crypto win-rates of 30-44%. This is a **parameter-prior framework**, not a strategy. Use it to seed initial parameter ranges for new strategies; reduces multi-testing surface and sharpens DSR adjustment per [[foundation/methodology/strategy-validation-discipline|validation discipline]]. **Caveat**: single-window observation (1y, 2025-26 BTC bearish-dominant); priors should not be applied as laws.

## Empirical Observation (W2 Source Data)

Across 13 strategies on BTCUSDT perp 4h 2025-05-03 → 2026-04-28, the **most-frequent param value in the top-Sharpe quartile** for each strategy:

| Strategy class | Strategy | Mode `stop_atr_mult` | Mode `target_atr_mult` | Avg Sharpe at mode |
|---|---|---:|---:|---:|
| Mean-reversion | bollinger_squeeze_4h | **1.0** | **1.5** | +1.44 |
| Mean-reversion | bollinger_squeeze_4h_short_only | **1.0** | 5.0 | +2.76 |
| Mean-reversion | funding_flip_recovery_4h | **0.5** | **3.0** | +2.48 |
| Mean-reversion | funding_flip_recovery_4h_short_only | **0.5** | **3.0** | +3.30 |
| Mean-reversion | asia_range_sweep_4h | **1.5** | **3.0** | +2.22 |
| Mean-reversion | wyckoff_spring_4h | **1.0** | **3.0** | +0.14 |
| Mean-reversion | turtle_soup_4h | **1.5** | **3.0** | +1.26 |
| Trend | ema_bias_breakout_4h | **2.5** | **3.0** | +1.71 |
| Trend | ema_bias_breakout_4h_counter | **2.5** | 6.0 | +1.19 |
| Trend | ema_bias_breakout_4h_v2 | **2.5** | 3.5 | +1.97 |
| Trend | htf_aligned_breakout_4h | **1.0** | **1.5** | +1.79 |

## Pattern

- **Mean-reversion mechanisms**: stop **0.5–1.5×ATR** (tight); target 3.0×ATR most common
- **Trend mechanisms**: stop **2.5×ATR** (wider); target 3.0×ATR most common
- **target=3.0×ATR is universal** across mechanism classes (appears as the modal target in 7 of 11 strategies)
- **Outliers**:
  - `htf_aligned_breakout_4h` — failed strategy with tight stops + tight targets; the parameter pattern reflects the *mechanism issue*, not a counter-example to the prior
  - `bollinger_squeeze_4h_short_only` and `ema_bias_breakout_4h_counter` — wider targets (5.0, 6.0) than the universal 3.0; worth investigating whether asymmetric exit makes sense for these specific cases

## Mechanism Story (Why the Pattern Holds)

### Mean-reversion expects fast resolution

A mean-reversion setup is the bet that price has *over-extended* and will revert. Resolution is expected within 1-3 bars (4-12 hours on 4h timeframe). Two consequences:

- **Tight stop catches false signals quickly.** If the reversion thesis is wrong (price keeps extending), you want to exit fast — before slow-decay damage accumulates. Tight stop at 0.5-1.5×ATR provides this.
- **Large target unnecessary.** The mechanism is intra-bar / few-bar, so a 3.0×ATR target is generous; the move is expected to complete in a fraction of that distance. The 3.0×ATR target gives runway without forcing premature exits.

### Trend expects sustained move

A trend setup is the bet that price will *continue* in a direction over many bars. The mechanism is multi-bar / multi-day. Two consequences:

- **Wider stop absorbs normal volatility.** Trends don't move in straight lines; pullbacks within trend are expected. A 2.5×ATR stop survives normal pullback magnitude. A 0.5-1.0×ATR stop would be hit by routine intra-trend noise.
- **Same 3.0×ATR target works** because trends take longer to develop the same R/R distance. The 3.0×ATR target on a trend setup represents 1.2x stop (trivial R/R) but the trend's *expected duration* compensates — many setups extend well past 3.0×ATR.

### Universal target around 3.0×ATR maps to R/R compatible with crypto win-rates

| Stop | Target | R/R | Required win-rate for breakeven |
|---|---|---:|---:|
| 0.5×ATR | 3.0×ATR | 6:1 | 14.3% |
| 1.0×ATR | 3.0×ATR | 3:1 | 25% |
| 1.5×ATR | 3.0×ATR | 2:1 | 33% |
| 2.5×ATR | 3.0×ATR | 1.2:1 | 45% |

**Crypto win-rates cluster ~30-44%** in the W2 dataset (median ~37%). This fits R/R 2:1 to 4:1 — exactly the range produced by the universal target combined with the mechanism-class-appropriate stops. The pattern is internally consistent: tight stops + universal target give R/R that's just barely covered by typical win rates; wider stops give R/R that's harder to cover, which fits trend strategies' need for larger absolute moves.

## 2026-05-06 BTC perp evidence — R:R varies by mechanism family

The 2026-05-06 6-strategy batch surfaced a sharper pattern at the working pockets across mechanism families. The "universal" framing should be **branched by mechanism family** rather than treated as a single prior:

| Strategy | Mechanism family | Stop | Target | R:R | Win-rate (pocket) | Pocket source |
|---|---|---:|---:|---:|---:|---|
| `funding_extreme_exhaustion_4h_v2` (#2) | Mean-reversion (positioning-extreme fade) | 0.5×ATR | 3.0×ATR | **1:6** | 38% | run #359 |
| `vol_regime_transition_4h` (#5) | Event-driven (regime transition) | 1.0×ATR | 5.0×ATR | **1:5** | 52% | run #4733 |
| `volume_z_long_moderate_4h` (#6) | Trend continuation (bias-aligned momentum) | 1.0×ATR | 3.0×ATR | **1:3** | 50% | run #19634 |
| `taker_buy_long_only_4h` (#4) | Trend continuation (bias scaffold residual) | 1.0×ATR | 3.0×ATR | **1:3** | 42% | parent pocket |
| `range_fade_4h` (parent) | Range fade at boundary | 1.0×ATR | 1.5×ATR | **1:1.5** | ~58% | run #3494 |

### Pattern by family

| Mechanism family | Typical stop | Typical target | R:R | Mechanism logic |
|---|---:|---:|---:|---|
| Mean-reversion (positioning-extreme fade) | 0.5–1.0×ATR | 3.0–5.0×ATR | **1:5–1:6** | Tight stops cut false-positive entries on rare-event filter; wide targets capture rare directional moves when the unwind happens |
| Event-driven (regime transition) | 1.0×ATR | 5.0×ATR | **1:5** | Regime transitions produce sustained directional moves; wide target captures the full transition; tight stop kills false-transition signals fast |
| Trend continuation (bias-aligned momentum) | 1.0–2.5×ATR | 3.0×ATR | **1:1.2–1:3** | Edge comes from win-rate near 50% × moderate target capture (already covered by original universal-prior framing) |
| Range fade at boundary | 1.0×ATR | 1.5×ATR | **1:1.5** | Quick reversion exit; high-win-rate / small-ATP composition; mean-reversion-at-band has tight target by design |

### Win-rate × ATP composition (across 5 working pockets in 2026-05-06)

- **Mean-reversion / event-driven**: 35–52% win-rate × **large ATP per win** = positive expected value at high R:R asymmetry. The mechanism is: be wrong often but small; when right, the rare wins pay for many losses.
- **Trend continuation**: 40–50% win-rate × **moderate ATP per win** = positive at 1:3 R:R. The mechanism is: be right roughly half the time with moderate-magnitude wins/losses; small edge × steady frequency.
- **Range fade**: 50–60% win-rate × **small ATP per win** = positive at 1:1.5 R:R. The mechanism is: be right often with small wins; quick reversion + tight target.

### Right-tail mean-reversion: vol-regime transitions

A sub-family within mean-reversion sits at the **right tail of the R:R distribution** — wanting target ≥7×ATR, not the 1:5–1:6 family default. Vol-regime-transition mechanisms (compressed-vol → expanded-vol with directional follow-through) are the canonical case: the entry is mean-reversion-shaped (fade the compression) but the post-transition move can be a multi-day directional extension that captures right-tail volatility, not the typical post-touch reversion.

**Evidence (`meta_vol_v1`, 2026-05-08 backtest, 500-config grid):**

| Population segment | Realized R:R |
|---|:---:|
| Top val configs | **9:1 to 14:1** |
| Cohort-median R:R producing edge | 5:1 to 7:1 |
| Below 3:1 | friction-bound bottom of cohort |
| `vt.target_atr_mult` modal | **pinned at the 7.0 upper bound** (suggests true optimum is higher; v2 extended bounds pending) |

Mechanism-honesty note: this is a **rare-event capture** profile — low win-rate (~46% at pocket) compensated by high R:R asymmetry. Structurally distinct from the conventional mean-reversion 1:5 family default that assumes ~50% win-rate. The win-rate × R:R composition is different: rare-event capture trades fewer setups for larger expected per-trade payoff, where the median trade is a small loss and the rare wins carry the cohort.

**Strategy-design implication.** Mechanisms whose informational signal predicts a **regime-state change** (compressed-vol → expanded-vol; cascade aftermath; structural-level breakout) — not just a price reversion — want `target_atr_mult ≥ 7`. Grid bounds should extend through 10+ to find the actual right-tail capture limit; v1's upper-wall finding at 7.0 is a parameter-bounds artifact, not a mechanism ceiling.

See [[trading/mechanisms/vol-regime-transition-tradeable]] for the cohort evidence in full (99.6% cohort full-positive on the right-tail R:R region; train→val corr +0.886; first non-HTF-bias-scaffold mechanism in the lab with cohort-grade right-tail evidence).

#### Target-axis saturation — mechanism exit-logic ceiling {#target-axis-saturation}

The right-tail-of-mean-reversion R:R signature has an **empirical ceiling at the mechanism's natural exit horizon**. For vol-regime-transition (with `vol_neutral` state exit firing when vol_rank returns to [0.25, 0.65]), the realizable target ceiling is approximately **7×ATR**: above that, trades exit on the regime-context return before reaching target.

**Empirical evidence (2026-05-11 `meta_vol_v2` grid):** at fixed `vt.stop_atr_mult=0.25`, configs at `vt.target_atr_mult` ∈ {7.0, 8.0, 9.0, 10.0, 11.0, 12.0} produced **identical** val P&L ($267.15), train P&L ($108.92), Sharpe (2.10), and trade count (74) — to two decimal places. Five different target values; one outcome. The right-tail-capture mechanism's profit-realization window plateaus at ~7×ATR because the `vol_neutral` exit fires before wider targets can be reached.

**Diagnostic shape — saturation vs wall.** This is a new diagnostic shape distinct from a param-wall:

| Pattern | Signature | Next move |
|---|---|---|
| **Param-wall** | Top region sits at the edge of tested bounds; modal value is at min or max | Grid bounds were too tight — extend the axis |
| **Param-axis saturation** | Configs above (or below) a threshold produce IDENTICAL results — row-identity across the axis | Mechanism's exit logic caps the realizable parameter region; **don't extend; drop the axis past the saturation point** |

The two patterns warrant different next moves. Walls suggest a higher (or lower) optimum exists beyond the grid; saturation indicates the mechanism's natural exit horizon truncates the productive region.

**Implications for grid design.**

1. For vol-regime-transition strategies, set `vt.target_atr_mult` grid to span [3, 7]; drop ≥ 8 entirely.
2. For other mechanism families with regime-context exits (cascade-aftermath, options-pin-release), expect a similar exit-logic ceiling; do **not** reflexively extend target axes past where the mechanism naturally exits.
3. Reporting practice: when grid output shows row-identity across a param axis at fixed-other-params, flag it as saturation and drop the axis from future iterations.

### Implication: the original "universal 2.5/3.0" recommendation is the trend-continuation default

The 2026-05-05 W2 finding (mean-reversion stops 0.5–1.5×ATR, trend stops 2.5×ATR, target 3.0×ATR universal) reflected the **HTF-EMA-bias-dominated cohort**. Once the lab inventory expanded to non-bias-scaffold mechanisms (positioning-extreme, vol-regime-transition), the family-conditional R:R signatures surfaced clearly. The universal-target=3.0×ATR pattern was real for the bias cluster; for mean-reversion/event-driven/range-fade families, target ranges from 1.5 to 5.0×ATR depending on family.

### Implication for next-batch strategy generation

Specify R:R per mechanism family rather than as a fixed prior:

| Family | Stop default | Target default |
|---|---:|---:|
| **Mean-reversion (positioning-extreme fade)** | 0.5–1.0×ATR | 3.0–5.0×ATR |
| **Event-driven (regime transition / cascade aftermath)** | 1.0×ATR | 5.0×ATR |
| **Trend continuation (bias-aligned momentum)** | 1.0–2.5×ATR | 3.0×ATR |
| **Range fade at boundary** | 1.0×ATR | 1.5×ATR |

These are starting points — pocket params surface during grid sweep. The discipline is to **pick the family default first, then sweep around it**, not to start with a uniform broad grid that would inflate trial multiplicity per Marcos' Third Law.

## BTC 4h breakout-class — regime-conditional exception {#btc-4h-breakout-class-exception}

**Status as of 2026-05-21**: N=2 empirical evidence from regime_v2 `phase_change_target` sweep and regime_v3 `break_entry_target` sweep BOTH on bull window (2024-05 → 2026-05), with Phase 4 bear-window validation **confirming the finding is REGIME-CONDITIONAL** (initially over-generalized; corrected within same day). See [[breakout-direction-asymmetry-by-macro]] for the full mechanism analysis.

### The bull-window pattern

On BTC 4h **in bull-dominant macro**, breakout-style triggers (price crossing prior range boundary; stable-phase ending into directional move) behave with mean-reversion R:R closer to **1:2** rather than trend-continuation 1:3.

| Sweep angle | Bull window | Val-best `target_mult` |
|---|---|---:|
| regime_v2 `phase_change_target_mult` | 2024-05 → 2026-05 (6,561 configs) | **2.0** |
| regime_v3 `break_entry_target_mult` | same window | **2.0** (val PnL +$206 vs +$79 at 5.0) |

Two independent sweep angles converge on R:R 2.0 for breakout-class triggers in bull macro.

### The bear-window failure

The same breakout mechanism FAILS in bear-dominant macro at any R:R. Phase 4 validation (Run 5649, Sharpe-optimal config from regime_v3 sweep) tested on `data/btcusdt_bear/futures` (2022-04 → 2023-09):

| Sub | Bull window | Bear window |
|---|---|---|
| `breakup_entry_long` | +$93 (101 trades, 36% WR) | **-$18** (67 trades, 33% WR) |
| `breakdown_entry_short` | +$56 (92 trades, 34% WR) | **-$91** (95 trades, 24% WR) |

Combined breakout PnL: bull **+$149**, bear **-$109**. Breakouts in either direction lack reliable follow-through in bear regimes; the mechanism's geometric assumption (clean break + follow-through to next range) is violated.

### Practical guidance

**For BTC 4h backtest building:**
- If your backtest window is **bull-dominant**: use R:R 1:2 for breakout-class subs (NOT wiki's universal 1:3)
- If your backtest window is **bear-dominant OR mixed**: breakout subs likely won't validate at any R:R; consider gating them by macro direction or cutting them
- If your backtest window **spans a regime shift**: expect breakout sub PnL to be net-positive overall but with significant bear-window drain — per-window decomposition matters more than aggregate metrics for these subs (per [[../../foundation/methodology/path-quality-diagnostics]])

**For deployment in unknown forward regime:**
- Breakout subs are EXPECTED to underperform in bear-leaning periods
- Consider sizing breakout subs **smaller than phase-entry subs** (which are cross-regime robust: regime_v3 `phase_entry_target=5.0` was val-best at 73% WR in bear-window `bear_phase_entry_short`)
- Cross-reference [[breakout-direction-asymmetry-by-macro]] for the full regime-mechanism analysis

### Triggers in this exception

- Stable phase ending into directional gap
- Price crossing prior period's `range_high` (breakup) or `range_low` (breakdown)
- Phase transitions within a same-direction macro

### Triggers NOT in this exception

- Explicit team-classified bull/bear phase entries (the team's `phase_type='bull'` or `'bear'` labels) — these are **cross-regime robust** with R:R 1:5+ trend-continuation working (regime_v3 `phase_entry_target=5.0` was val-best; 73% WR in bear-window `bear_phase_entry_short`)
- Range-fade mean-reversion (already on 1:1.5 prior per the §"Pattern by family" range-fade family default above)

### Source pages
- [[breakout-direction-asymmetry-by-macro]] — full mechanism analysis (N=2 bull + N=2 bear-window evidence + structural mechanism)
- [[../../foundation/methodology/team-stable-range-detector-methodology]] — the data layer this builds on
- [[../../foundation/macro/btc-regime-taxonomy-2020-2025]] — regime context for bull-vs-bear classification

## Multi-horizon coherence check — stop geometry must clear the ATR of the intended holding horizon {#multi-horizon-coherence}

**Added 2026-06-11** (state-monitor evaluator build + live validation). The per-family priors above express stops in ATR multiples of a single, strategy-native timeframe. That hides a cross-horizon failure mode: a stop that looks reasonable as a percentage can be incoherent with the *intended holding horizon*.

**The rule**: before sizing a stop, express its distance in **ATR(14) units of every candidate holding horizon** (4h / daily / weekly). A stop inside ~1×ATR of a horizon's bar is **noise-stopped at that horizon** — it will be hit by ordinary bar-to-bar variation, not by thesis failure. Therefore:

> **The stop geometry *declares* the position's true maximum holding horizon: the longest horizon whose ATR the stop clears.** A mismatch between stated intent ("swing, hold days") and geometry ("stop = 0.2× daily ATR") is a design error catchable before entry.

**Worked example (2026-06-11, live)**: BTC ~$63.1k, futures long with SL 1% ($631). As a number, 1% looks reasonable. Measured per horizon: >1× 4h ATR (ok), but ≈ **0.2× daily ATR** — fails daily, fails weekly. The geometry only fits a ≤4h holding horizon, while the stated frame was multi-day. The check caught the mismatch pre-entry.

**Corollary for R/R**: structural (level-based) R/R must be read **jointly** with the noise verdict. The same session produced a short with structural R/R 1:2.4 that was correctly degraded by the check ("stop inside 1d+1w noise") — a good-looking R/R with a noise-stop is not realizable at the intended horizon, because the stop fires on noise before the target leg can play out. This is the position-design-level form of the [[../../foundation/methodology/forecast-vs-tradeable-gap|forecast-vs-tradeable gap]]'s stop-width/horizon incompatibility argument (and its §2B ~50% realization gap is what survives even when the geometry is coherent).

**Liquidation note (leveraged positions)**: the naive 1/leverage liquidation bound should also be expressed in horizon-ATR units. The de-risk urgency asymmetry — spot drawdown is recoverable, futures drawdown is liquidation-terminal — rides on whether the liquidation distance clears the holding horizon's ATR, not just the stop.

**Implementation**: the check is automated in the state-monitor's `monitor.py evaluate` (ATR on 4h/daily/weekly; every SL/TP/liquidation distance in all three units + holding-horizon verdict). The principle applies beyond the tool: any manual position design should run the same three-row arithmetic.

## Use as Parameter Prior

When parameterizing a new strategy:

### Step 1 — Identify mechanism class

Is the entry signal a *mean-reversion* (fade, reversal, exhaustion) or *trend* (continuation, breakout, momentum) mechanism? Hybrid strategies tend toward whichever class their entry trigger most resembles.

### Step 2 — Use family-specific defaults (sharpened 2026-05-06)

**Mean-reversion (positioning-extreme fade) default grid:**
```
stop_atr_mult ∈ {0.5, 1.0}
target_atr_mult ∈ {3.0, 4.0, 5.0}
# 1:5–1:6 R:R; e.g. funding_extreme_exhaustion_4h_v2 #359
```

**Event-driven (regime transition / cascade aftermath) default grid:**
```
stop_atr_mult ∈ {0.75, 1.0}
target_atr_mult ∈ {4.0, 5.0}
# 1:5 R:R; e.g. vol_regime_transition_4h #4733
```

**Trend continuation (bias-aligned momentum) default grid:**
```
stop_atr_mult ∈ {1.0, 1.5, 2.0, 2.5}
target_atr_mult ∈ {2.5, 3.0, 3.5}
# 1:1.2–1:3 R:R; e.g. ema_bias_breakout_4h_v2; volume_z_long_moderate_4h #19634
```

**Range-fade-at-boundary default grid:**
```
stop_atr_mult ∈ {0.75, 1.0, 1.25}
target_atr_mult ∈ {1.0, 1.5, 2.0}
# 1:1.5 R:R; e.g. range_fade_4h #3494
```

**Always test target=3.0×ATR** as one of the candidate values for trend-continuation mechanisms. The universal-target modality holds for the bias-cluster; family-specific defaults above replace it for non-bias families.

### Step 3 — Justify deviations

If the mechanism *theoretically* requires stops or targets outside these ranges, document the reason in the strategy's pre-registration. Examples of legitimate deviation:

- **Pre-expiry options pinning** (per [[trading/mechanisms/options-gamma-perp-effects|gamma-effects]] Hypothesis A): may require very tight stops if pinning is mechanism-typical narrow range
- **Post-expiry gamma flush** (per same page Hypothesis B): may require very wide targets if expansion is the entire thesis
- **Multi-day Wyckoff cycle plays**: target may extend to 5-6×ATR if the structural move spans days
- **Tight-stop scalp setups**: stop may be 0.25×ATR if the mechanism is literally bar-fade with no tolerance for adverse movement

In each case, the deviation has a *mechanism-grounded reason* — not "we found this in the heat map." Heat-map-derived deviations should be suspected of overfitting per [[foundation/methodology/strategy-validation-discipline|Marcos' Second Law]] (no research under influence of backtest).

### Step 4 — Smaller grid → tighter DSR

Per [[foundation/sources/lopez-de-prado-advances-fin-ml|López de Prado]] and [[glossary/deflated-sharpe-ratio|DSR]] §8.3: trial multiplicity penalizes apparent edge. A grid with 3 stops × 3 targets = 9 combinations has tighter DSR adjustment than 5 stops × 5 targets = 25 combinations. Using parameter priors to *reduce* the grid is a discipline that compounds with the validation-discipline gates. Fewer trials → lower trial multiplicity → tighter DSR threshold passable with less luck.

## Worked Example — Applying the Prior to a New Mechanism

Suppose a new mechanism is proposed: "Asia session range fade after London open" — a mean-reversion entry on a London-session reversal of an Asia-session range break.

**Without prior**: parameterize broadly. Stop ∈ {0.25, 0.5, 0.75, 1.0, 1.5, 2.0, 2.5, 3.0} × Target ∈ {1.0, 1.5, 2.0, 2.5, 3.0, 4.0, 5.0, 6.0} = 64 combinations. DSR adjustment significant.

**With prior**: identify as mean-reversion. Use {0.5, 1.0, 1.5} × {2.0, 3.0, 4.0} = 9 combinations. DSR adjustment manageable; if 9-combo grid produces a clearly-passing strategy, the post-multiplicity confidence is meaningfully higher.

**Decision**: 9-combo prior-driven grid is the discipline. If results are weak, a follow-up *with new pre-registration* might widen the grid — but per Part I §9 Mutation Operators, that's a parameter-sweep mutation requiring fresh trial-count accounting.

## Caveats

- **Single 1y window.** This empirical observation comes from one window (2025-05 → 2026-04, 2025-26 BTC bearish-dominant per [[foundation/macro/regime-conditional-edge-2025-26|regime-conditional-edge finding]]). The pattern may not transfer to:
  - Other regimes (bullish trending, low-vol, high-vol). ATR-relative stops may behave differently when ATR itself is in a different absolute band.
  - Other timeframes. 4h was the universal timeframe in the cohort; 1h or 1d may show different modes.
  - Other instruments. BTCUSDT majors only; alts may show different vol regimes and different optimal R/R.
- **R/R is not deployment-grade by itself.** Per [[foundation/methodology/deployment-edge-threshold|deployment-edge threshold]] methodology, gross R/R must clear net-of-friction floor. A "passing" stop-target pair on the prior may still fail deployment-grade if gross ATP is too small.
- **Strategy class assignment matters.** If a "mean-reversion" strategy actually has trend characteristics (e.g., the "fade" thesis works because price *continues* in the direction of fade), the prior misleads. Mechanism articulation upstream is the load-bearing discipline.
- **Individual mechanisms may have specific reasons to deviate.** This is a default, not a law. The discipline is to *use it as the starting point* and document deviations with mechanism reasons.
- **Outliers matter.** `bollinger_squeeze_4h_short_only` (target 5.0, Sharpe +2.76) and `ema_bias_breakout_4h_counter` (target 6.0, Sharpe +1.19) deviate from the universal target. These aren't disproof — they may indicate sub-classes within mechanism families where the prior bifurcates. Worth investigating per-strategy reasons.
- **`htf_aligned_breakout_4h` is the cautionary tale**: parameter modes (1.0, 1.5) match the *mean-reversion* prior despite being a trend-class strategy. The mismatch is itself diagnostic — the strategy is failing because its parameter modes don't match its mechanism class. Use the prior as a *check on mechanism articulation* as well as a parameter starting point.

## What This Page Is Not

- **Not a strategy.** Universal stops/targets aren't a tradeable mechanism on their own. They're a parameter framework for *expressing* mechanisms in DSL grids.
- **Not a substitute for falsifier discipline.** A stop/target pair from the prior still produces a strategy that needs pre-registered hypothesis, mechanism, observable, and falsifier per Part I §4.
- **Not a substitute for net-edge analysis.** Gross R/R from the prior must still clear the [[foundation/methodology/deployment-edge-threshold|deployment-edge floor]] for deployment-grade.
- **Not a regime-stationary law.** Per [[foundation/macro/regime-conditional-edge-2025-26|regime-conditional-edge finding]], all 13 strategies' edges concentrate in 2025-Q4 + 2026-Q1. The parameter modes are *for that regime*; out-of-regime testing may produce different optima.

## Operationalization Status

| Element | Wiki encoded | Pipeline implemented |
|---------|:---:|:---:|
| Empirical observation (modal params per strategy) | ✅ this page | ✅ raw data in the backtesting lab |
| Mechanism-class taxonomy | ✅ | ⚠️ implicit in the backtesting lab; not formalized as a metadata field |
| Default grid recommendations per class | ✅ | ❌ — no automated parameter-prior application |
| Deviation-justification discipline | ✅ this page | ❌ — manual discipline at strategy-design time |
| DSR-aware grid sizing | ✅ noted | ❌ — DSR computation itself not pipeline-implemented |

**Net**: this page is wiki-encoded; pipeline integration would auto-suggest grids based on declared mechanism class. Engineering work owner-flagged.

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025|BTC taxonomy]]) | Pattern stability | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Confirmed** — modes derived from this regime | W2 source data; 13 strategies |
| Bearish continuation (e.g. 2026-Q1) | Likely-stable — same ATR regime, similar setups firing | Q1 2026 partial coverage in W2 |
| Mild uptrend / chop (e.g. 2025-Q2/Q3) | Untested — strategies underperform in these windows; modal params may differ | Per [[foundation/macro/regime-conditional-edge-2025-26]] |
| Cycle peak / euphoria | Untested — dramatically different ATR regime; priors likely shift | Theoretical concern only |
| Cascade aftermath | Untested — dramatic vol expansion may invalidate ATR-relative framing | Open question |

**Notes:**
- Pattern is **regime-derived, not regime-universal**. Use as starting point; revalidate priors when applied to a different regime
- Required regime markers for prior to apply confidently: ATR magnitude similar to 2025-26 BTC perp 4h (i.e., not a 10× vol expansion or compression regime)
- Anti-regime conditions (don't apply prior without revalidation): post-Black-Thursday-style cascades, halving-week vol blowoffs, ATH breakout phases

## Key Takeaways

- **Family-conditional R:R signatures** (sharpened 2026-05-06 per N=5 working pockets): mean-reversion 1:5–1:6, event-driven 1:5, trend continuation 1:3, range-fade 1:1.5. The "universal" 2.5/3.0 prior is best understood as the **trend-continuation default**.
- **Mean-reversion / event-driven**: tight stop (0.5–1.0×ATR) + wide target (3–5×ATR). Tight stops cut false-positive entries on rare-event filters; wide targets capture rare directional moves.
- **Trend continuation**: stop 1.0–2.5×ATR + target 3.0×ATR (the original universal pattern; held for bias-cluster strategies).
- **Range fade at boundary**: stop 1.0×ATR + target 1.5×ATR. Quick reversion exit; high-win-rate / small-ATP composition.
- **Win-rate × ATP composition is family-conditional**: mean-reversion 35–52% × large ATP; trend 40–50% × moderate ATP; range-fade 50–60% × small ATP. All produce positive EV at family-appropriate R:R.
- **Mechanism story holds**: stop width matches mechanism speed; target width matches expected continuation magnitude.
- **Multi-horizon coherence check (2026-06-11)**: measure the stop in ATR(14) of every candidate horizon (4h/daily/weekly); SL < ~1×ATR(horizon) ⇒ noise-stopped at that horizon. The stop geometry declares the position's true max holding horizon; read structural R/R jointly with the noise verdict.
- **Use as parameter prior**, not as law. Identify mechanism family → use family-specific defaults → justify deviations with mechanism reasons → smaller grid → tighter DSR.
- **`htf_aligned_breakout_4h` mismatch is diagnostic** — parameter modes don't match its mechanism class; this *itself* is signal that the strategy is misarticulated.
- **Single-window caveat**: 2025-26 BTC bearish-dominant only. Revalidate for other regimes.
- **Compounds with validation-discipline gates** (smaller grid → tighter DSR adjustment) and deployment-edge gates (R/R is not net-edge).

## Related

- [[foundation/sources/lopez-de-prado-advances-fin-ml]] — parameter-search discipline + DSR multiplicity-correction grounding
- [[foundation/sources/murphy-technical-analysis]] — Ch 9 (Moving Averages) covers volatility-aware stops
- [[foundation/methodology/strategy-validation-discipline]] §7 — anti-overfitting recommendations including narrower parameter ranges
- [[foundation/methodology/deployment-edge-threshold]] — R/R is not deployment-grade; net-of-friction edge is the gate
- [[foundation/methodology/forecast-vs-tradeable-gap]] — the forecast-level form of stop-width/horizon incompatibility; the §multi-horizon coherence check is its position-design-level operationalization; its §2B realization gap (~50%) is what coherent geometry actually captures
- [[foundation/macro/regime-conditional-edge-2025-26]] — companion W2 finding on regime concentration of edge
- Part I §4 — Structured Hypothesis Format (mechanism articulation matters for class assignment)
- Part I §9 — Mutation Operators (parameter-sweep mutation requires fresh trial-count accounting)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — embedded bias filter design pattern; same mechanism-class diagnostic principle applies
- [[investment/allocation/equity-management-rules]] — risk_pct + R/R math for the deployment-grade math the prior feeds into

## Sources

- the backtesting lab Finding 5 — primary source for the original empirical pattern (W2 13-strategy)
- the backtesting lab cross-strategy synthesis — primary source for the **family-conditional R:R sharpening (2026-05-06)**
- 13 + 6 strategies' `grid_results_1y/` outputs in the backtesting lab
- Pocket-specific sources: `strategies/tested/funding_extreme_exhaustion_4h_v2/` (#2 / #359 / 1:6), `strategies/tested/vol_regime_transition_4h/` (#5 / #4733 / 1:5), `strategies/tested/volume_z_long_moderate_4h/` (#6 / #19634 / 1:3), `strategies/tested/range_fade_4h/` (#3494 / 1:1.5)
