---
title: "Failed-Breakout Reversal — Raschke's Turtle Soup"
status: growing
created: 2026-05-04
updated: 2026-05-21
tags: [mechanism, schwager, raschke, breakout, reversal, liquidation-cascade-aftermath, day-swing-applicable, first-test-rejected, short-asymmetry-partial-truth, regime-conditional-companion]
---

# Failed-Breakout Reversal — Raschke's Turtle Soup

> **TL;DR**: When a textbook channel breakout *fails* to extend within N bars, it was a stop-run rather than a trend ignition. The stop-runners are now offside; the reverse trade has high hit-rate as they cover. Originated by Linda Bradford Raschke ([[foundation/sources/schwager-market-wizards|Schwager NMW 1992]]) — the name "Turtle Soup" comes from inverting the Turtle Trader 20-day breakout system. **Crypto carrier hypothesis: this is the canonical mechanism for liquidation-cluster fade trades** — Coinglass-visible stop clusters at obvious technical levels make this *more* observable in crypto than in equities.
>
> **⚠️ Status update (2026-05-05): First test on bare-mechanism default rejected.** First real backtest of this hypothesis at the backtesting lab fired strategy-level falsifier on 3 of 3 criteria (medSh −0.77, medWR 33.3%, medATP −$0.70) on BTCUSDT 4h, 2025-05-03 → 2026-04-28. Bare-mechanism rejection: see [[trading/rejected/turtle-soup-4h-2026-05]]. **Partial truth retained on short-side asymmetry** (long sleeve med −$74.50 vs short sleeve +$19.70) — consistent with 2025-26 bearish-dominant regime; salvage paths (short-only mutation, confluence-required version, phase-context-conditioned version) documented but untested. The full operational specification below (§Operational Specification — confluence + macro filter requirements) was **not** implemented in the bare-mechanism backtest; spec'd version remains untested.

## Hypothesis (Structured Format)

```
Hypothesis:    A breakout above a well-defined N-day extreme (or below a
               well-defined N-day low) that fails to extend within K bars
               and reclaims the prior level produces above-random returns
               when traded in the reverse direction.

Mechanism:     Channel breakouts attract two flows:
               (a) Trend-followers / breakout-buyers who place stop-buys
                   above the resistance and stop-sells below support
               (b) Liquidation engines that auto-trigger on perp
                   leverage when their margin breaches at the stop level

               When the breakout was a *stop-run* (intentional manipulation
               or organic flow exhausting itself at the stop cluster) rather
               than a *trend ignition* (genuine new flow building above the
               level), the breakout-buyers + liquidated shorts (long-side
               case) become the offside-position pool. Their forced cover
               creates the recovery move — reverse trade catches that
               cover flow.

               The failure-to-extend is the diagnostic: real trend
               ignitions extend within hours. Failed breakouts revert
               within K bars (typically 1-3 on the relevant timeframe).

Observable:    For each candidate:
               - Well-defined N-day extreme (typically N=20; Turtle's
                 breakout level)
               - Price exceeds the level by small margin (e.g., 0.5-2x ATR)
               - Reversal back through the level within K bars
               - Volume on the failed-breakout bar > 30-day median (real
                 stops being hit, not low-volume noise)

Expected:      Short entries on confirmed failed-upside-breakouts
               (and long entries on failed-downside-breakouts) produce
               2-4x R returns within 1-5 bars; win rate 55-70%;
               R:R 1:2 to 1:3 with stops just beyond the failed-breakout
               extreme.

Falsifier:     If failed-breakout-flagged trades (with the listed
               observability criteria) do not outperform random-direction
               trades of equivalent volatility-matched assets — the
               mechanism is not present in the tested market.
               Specifically: backtest BTC perps 2023-2026 of failed
               breakouts at 20-day extremes with reclaim within 3 bars;
               should outperform random-direction trades by ≥1 standard
               deviation in median return. Out-of-sample persistence in
               2/3+ of walk-forward windows.
```

## The Source — Raschke's "Turtle Soup"

Linda Bradford Raschke (interviewed in *The New Market Wizards*, 1992) is the popularizer of this mechanism. The name "Turtle Soup" is a deliberate inversion of the Turtle Trader 20-day breakout system:
- **Turtle system**: enter long on close above 20-day high (ride the breakout)
- **Turtle Soup**: enter short on close above 20-day high *that fails to extend within 4 bars and reverses back below* (fade the breakout when it's a stop-run)

The same principle applies inverted on the short side. Raschke's framing in the interview emphasizes:
- This is *not* a contrarian-by-default rule (don't fade every breakout)
- The discipline is to *wait for the failure to be confirmed* — not to anticipate it
- The reversal back through the level is the trigger; without it, the trade isn't on
- Volume + speed of the reversal are confirming factors

> "If the breakout doesn't extend, the move was a fakeout. The traders who bought the breakout are now trapped — they're going to bail. That's your trade." — Raschke (paraphrased from secondary sources)

Per [[foundation/sources/schwager-market-wizards|Schwager source page]] honest caveat: the Raschke material in the wiki is reconstructed from secondary sources (the Brandeis primary covers only the 1989 volume; Raschke is in the 1992 *NMW*). The mechanism's *concept* is well-documented across multiple secondary sources; specific numerical thresholds may vary by writeup.

## Why It Works (Mechanism Decomposition)

### Two flow types around channel breakouts

Around any well-defined N-day extreme, two flow types cluster:
- **Breakout buyers** (long-side case): stop-buy orders above resistance, expecting trend ignition
- **Stopped shorts** (long-side case): protective stops above resistance, getting force-covered when level breaks

When the level is breached:
1. Stop-buy orders fill mechanically
2. Short stops trigger (forced cover)
3. Both flows are *non-discretionary* — they fill whether the breakout is real or not
4. The price moves in the breakout direction *because* of these mechanical flows, regardless of underlying flow

The breakout's *validity* depends on what happens *next*:
- If genuine new flow continues to build above the level → trend ignition; price extends
- If the mechanical flows were *all* the buying → price stalls; sellers absorb it; reverse

### The diagnostic: failure to extend

Real trend ignitions show:
- Continued buying at the level (volume sustained)
- Pullbacks held above the level
- Extension within 1-2 bars

Stop-runs show:
- Volume spike at the breakout, then declining
- No follow-through
- Reversal back through the level within K bars (typically 1-3 on relevant timeframe)

The failure to extend within K bars is the trigger. Before that, the trade isn't on. The discipline is to *wait for confirmation* of failure, not to anticipate it.

### Why the reverse trade has edge

Once the failure is confirmed:
- Breakout buyers who entered on the trigger are now offside (they bought at or above the level; price is now back below)
- Some breakout buyers placed protective stops just inside the level (e.g., at the 20-day high minus 0.5%); these stops trigger as price returns through
- The combined cover flow creates a self-reinforcing reversal

The reverse trade catches this cover flow. Edge is in the *forced* nature of the cover — non-discretionary stop-driven selling that doesn't represent informed view, just position-management.

## Crypto Carrier — Why This Translates Beautifully

This is **the canonical mechanism for liquidation-cluster fade trades** in crypto. From [[foundation/market-structure/crypto-carrier-features|the carrier-features catalog]]:

| Carrier | What to look for |
|---------|------------------|
| **Coinglass liquidation map** | Clustered short-side liquidations density just above the resistance level |
| **Short liquidation cascade event** | The cascade IS the failed-breakout — wick up through resistance with high liquidation volume |
| **Funding rate spike** | Funding spikes deeply positive on the squeeze, then resets |
| **On-chain whale flow** | Net distribution by whale wallets to exchanges *during* the upthrust (whales selling into the FOMO) |
| **OI dynamics** | OI collapses on the squeeze (shorts liquidating); rebuilds on the short side as price falls back |
| **Failure to hold** | Within hours, price back below the broken resistance |

This is the same mechanism as [[trading/mechanisms/accumulation-distribution|Wyckoff upthrust]] but with explicit Schwager / Raschke framing. The two pages compose:
- This page: explicit failed-breakout-reversal trade with structured hypothesis
- Accumulation-distribution: phase-context — upthrust occurs at end-of-distribution
- Both: same operational signature in crypto (liquidation cluster + recovery)

The key crypto advantage: **liquidation maps make stop clusters visible in real time**. In equities, stop clusters are inferred. In crypto perps, they're displayed.

## Operational Specification

Default parameterization for backtest:

| Aspect | Rule |
|--------|------|
| **Setup precondition** | BTC/ETH perp at well-defined 20-day extreme; macro regime non-trending OR trending against the breakout direction (avoid fading breakouts in strong trends) |
| **Failed-breakout trigger** | Price exceeds the extreme by 0.5-2× ATR within a single bar; bar volume > 30-day median; price closes back inside the prior range within 3 bars |
| **Confluence requirement** | At least 3 of: liquidation cluster density visible on Coinglass at the level, volume spike on the breakout bar, on-chain whale distribution to exchanges during the wick, funding-rate flip on the cascade, RS divergence with sector |
| **Entry** | First close back inside the prior range with the close in the lower half of the range (long-side case: close below the broken-resistance with close in the lower half of the bar) |
| **Position size** | Per [[investment/allocation/equity-management-rules]] (≤3-5% risk per trade); volatility-adjusted per Hite/Basso method |
| **Stop placement** | Just above the failed-breakout high (long-side case: just above the wick high) |
| **First target** | Mid-range or 1.5× initial-risk distance, whichever is closer |
| **Trail rule** | After 1× initial-risk move favorable, trail with 1× ATR or to first higher-low (whichever is tighter) |
| **Time stop** | Exit if position is not 1×R favorable within 5 bars (failed breakouts that don't reverse quickly are not Turtle Soup setups) |

## Distinction from Adjacent Mechanisms

### vs. [[trading/mechanisms/pivotal-points|Pivotal-point breakouts]]

Pivotal-point breakouts are *long-on-confirmed-extension*. Failed-breakout reversal is *short-on-failure-of-extension*. They're inverse sides of the same setup:
- If the level extends → pivotal-point breakout long is the trade
- If the level fails → failed-breakout reversal short is the trade
- The *discipline* is to wait for the resolution before committing

A trader can run both setups with a unified rulebook: pre-position a buy-stop above the level + a sell-stop below; whichever fires first determines the direction; if the first fires and reverses, take the failed-breakout reversal.

### vs. [[trading/mechanisms/accumulation-distribution|Wyckoff upthrust]]

Wyckoff upthrust is the *phase-context* version (upthrust occurs at end-of-distribution). Failed-breakout reversal is the *operational-trigger* version (the wick + reclaim is the entry signal). They overlap heavily:
- An upthrust at end-of-distribution is a high-conviction failed-breakout reversal
- A failed-breakout reversal in the middle of accumulation is a noise event
- Phase context (Wyckoff) + operational trigger (Raschke) compose for highest-conviction setup

### vs. [[foundation/market-wisdom/composite-operator|Wyckoff Composite Operator]] testing

The CO's "test of resistance" via small probes is the same conceptual phenomenon. CO advances 1-2 points to see if the group follows; if no follow, CO stops; if the price returns inside the prior range, that's the failed-breakout signal. Raschke is explicit operational; Wyckoff is behavioral framework.

## First Test Result (2026-05-05)

**Backtest**: the backtesting lab, BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital, fee 0.04%, 3072 parameter combinations. Bug-fix verified (prior 0/3072 opportunities issue resolved).

**Strategy-level falsifier fired on 3 of 3 criteria:**

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | −0.77 | < 0 | **YES** |
| Median win rate | 33.3% | < 45% | **YES** |
| Median ATP | −$0.70 | ≤ random (≈0) | **YES** |

**Partial truth — short-side asymmetry:**
- Long sleeve (failed breakdown → reclaim → buy): median P&L **−$74.50**
- Short sleeve (failed breakout → reclaim → sell): median P&L **+$19.70**

The asymmetry is consistent with the 2025-26 bearish-dominant regime; **the asymmetry is a regime feature, not a mechanism feature**. Same window saw similar long-fails / short-positive pattern in `bollinger_squeeze`, `funding_flip_recovery`, and other mechanisms — see [[trading/mechanisms/regime-gate-vs-bias-filter]] for the cross-strategy synthesis.

**What the test rejects:**
- Bare-mechanism applicability with default Raschke-style parameterization in 2025-26 BTC perp 4h
- Symmetric (long + short) framing in this window

**What the test does NOT reject (untested salvage paths):**
- Short-only mutation conditioned on bearish regime
- Confluence-required version (the wiki page §Operational Specification specifies ≥3 of liquidation cluster / volume / whale flow / funding flip / RS divergence; bare backtest used none)
- Phase-context-conditioned version (Wyckoff upthrust at end-of-distribution)

**Hypothesis-level paired falsifier missing:** the no-recent-breakout-required mirror was specified in pre-registration but not built. Strategy-level falsifier fired anyway on 3/3 metrics, which is sufficient for the rejection. Owner: tooling work for paired-falsifier construction in pipeline.

**Per-test full entry**: [[trading/rejected/turtle-soup-4h-2026-05]] documents trial-count discipline (≥13 strategies on dataset), [[foundation/methodology/strategy-validation-discipline|validation-discipline]] caveats, and structured salvage-path table.

**Status update**: this page moves from `seedling` (open hypothesis, untested) to `growing` (tested-once-on-bare-mechanism, rejected; spec'd-version untested; salvage paths documented). The mechanism is **not invalidated as a research direction** — the spec'd version with confluence + macro filter remains untested, and the short-side asymmetry signal worth tracking — but the *bare default* should not be re-tested without addressing the salvage paths.

## Cross-symbol evidence update (2026-05-18/19) — phase-conditioned variant tested on 3 symbols {#cross-symbol-evidence-2026-05}

The phase-conditioned salvage path flagged in 2026-05-05's "untested salvage paths" list was tested in the 2026-05-18 14-strategy batch + 2026-05-19 ablation cycle. **Phase-conditioning works as a mechanism-quality filter but does NOT make a wrong-symbol mechanism right**.

| Symbol | Variant | Verdict | Rejection entry |
|---|---|---|---|
| BTC | `failed_breakout_phase_fade_btc` (bidirectional) | Sharpe -1.26, N=161, TP fires 5/161 = mechanism geometrically broken | [[../rejected/failed-breakout-phase-fade-btc-2026-05]] |
| ETH | `failed_breakout_phase_fade_eth` (bidirectional) | Sharpe +0.60 / PSR(>0) 79% — **research-grade, not deployment-grade** | [[../rejected/failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] |
| ETH | `failed_breakout_phase_fade_eth_short_only` (ablation) | Sharpe +0.49 — falsifier-fires, BELOW parent's short-subset 0.91 standalone | (same as above) |
| SOL | `failed_breakout_phase_fade_sol` (bidirectional) | Sharpe -0.11 aggregate (short-side drag); long-subset standalone 0.44 | [[../rejected/failed-breakout-phase-fade-sol-with-long-only-ablation-2026-05]] |
| SOL | `failed_breakout_phase_fade_sol_long_only` (ablation) | Sharpe +0.42 — marginal falsifier-fail (sub-deployment magnitude) | (same as above) |

### Per-symbol asymmetry framework {#per-symbol-asymmetry}

The 3-symbol matrix aligns cleanly with the per-symbol mechanism asymmetry codification (N=3 BTC continuation + N=2 SOL long-bias amplifier + N=2 ETH fakeout-fade):

- **BTC**: continuation character means failed-breakout-fade is structurally wrong — both directions fail. See [[../../foundation/asset-classes/btc-market-structure#empirical-anchor|btc-market-structure §empirical-anchor]].
- **ETH**: fakeout-fade character makes the mechanism class work — but only on short side; long side flat-negative. Bidirectional aggregate is research-grade.
- **SOL**: long-bias regime amplifier character produces working long side at sub-deployment magnitude — mechanism qualitatively confirms the framework but lacks deployment-grade edge.

### Methodology discovery: position-blocking-as-inadvertent-filter {#position-blocking-finding}

The ETH ablation arc surfaced a **load-bearing methodology learning** (N=2 cross-symbol):

- ETH parent (bidirectional) short subset standalone Sharpe **0.91**
- ETH ablation (short-only, filter removed) Sharpe **0.49** — 14 additional unblocked shorts contributed −$4.60/trade
- **Bidirectional position-management acts as an inadvertent quality filter** — when a position in one direction is open, signals in the opposite direction are blocked; on ETH that blocking selects against systematically-losing continuation-bound signals
- SOL check: position-blocking NEUTRAL (long-bias amplifier means unblocked shorts aren't systematically losing); N=2 cross-symbol evidence that filter value-add is symbol-specific

**Discipline sharpening**: direction-restriction ablation comparison should be vs **parent's same-side SUBSET**, NOT vs parent aggregate. Full discussion at [[../rejected/failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05#position-blocking-mechanism|the ETH rejection page §position-blocking-mechanism]] + [[scaffold-as-mechanism]] §Caveats (codification candidate).

### Companion attempt — multi-bar + flow filter (range_tunnel) {#range-tunnel-attempt}

A separate operational form of mutation-direction #1 — Wyckoff multi-bar + `taker_buy_z` flow filter — was tested on ETH across a v1→v2 mutation cycle. Both rejected; **edge-saturation character discovered** (mechanism produces real K≈3-bar drift then mean-reverts). One operational form within the broader mutation-direction-#1 space; explicit untested-variants enumeration at [[../rejected/range-tunnel-wyckoff-eth-multi-bar-flow-2026-05#untested-mutations]].

### What remains untested for this mechanism family {#remaining-untested}

- BTC `phase_breakout_continuation_btc` (REVERSED-direction mutation — trade IN the wick direction when BTC wicks-and-closes-back) — highest-EV candidate per BTC continuation character
- Cross-regime (2022-23 bear) — gated on bear parquet with phase columns merged
- ETH spec'd-confluence variant (≥3 of liquidation cluster / volume / whale flow / funding flip / RS divergence) — different operational form
- Different multi-bar pattern variants beyond range_tunnel's specific operationalization
- Cross-symbol-pooling deployment (multi-symbol ensemble with per-symbol direction restrictions) — untested

## Regime Applicability

| Regime (per [[foundation/macro/btc-regime-taxonomy-2020-2025\|BTC taxonomy]]) | Mechanism state | Evidence |
|---|---|---|
| Cycle peak rejection / bearish (e.g. 2025-Q4) | **Bare-mechanism rejected** on default params; short-side asymmetry partial-truth | [[trading/rejected/turtle-soup-4h-2026-05]] — falsifier fired 3/3 on bare default; long sleeve −$74.50, short sleeve +$19.70 |
| Bearish continuation (e.g. 2026-Q1) | Untested separately — partial overlap with the rejected-test window | Same window as the rejection |
| Bullish trending | Untested — would expect inverse asymmetry (long sleeve positive in bull) | Theoretical |
| Chop / transitional | Untested — mechanism may operate when neither bullish nor bearish dominates | Theoretical |
| Cascade aftermath | **Mechanism canonical** — Wyckoff upthrust as phase-context | Per family page; not isolated in current backtest |
| Pre-expiry options pinning periods | **Confluence high** — failed breakouts INTO high-gamma strikes are double-mechanism setups | Untested; per [[trading/mechanisms/options-gamma-perp-effects]] |

**Notes:**
- Mechanism is **regime-conditional**: the bare default failed in 2025-Q4 BTC perp 4h window; salvage paths (short-only mutation conditioned on bearish regime; confluence-required version; phase-context-conditioned version) all remain untested
- Required regime markers for productionize: clear bearish regime confirmation (per [[foundation/macro/regime-conditional-edge-2025-26]]) + cascade or stop-cluster confluence
- Anti-regime conditions (don't deploy when): bare-mechanism on default params in any regime (rejected); bullish trending without bearish-regime confirmation
- Pipeline capability gap: confluence requirements per §Operational Specification (liquidation cluster + funding flip + on-chain whale flow + RS divergence) require data primitives not currently in DSL

Cross-strategy regime context: per [[foundation/macro/regime-conditional-edge-2025-26|W2 finding]], strategies in this mechanism class show regime-asymmetric P&L distributions consistent with the broader bearish-dominant cohort pattern.

## Honest Caveats

- **Selection bias in pattern recognition.** "Failed breakouts" are easy to identify retrospectively; in real time, distinguishing failed-breakout-from-shallow-pullback is the actual difficulty. The K-bar threshold is the hard part.
- **False false-breakouts.** A breakout that briefly fails (3 bars), retests, and then extends will look like a failed breakout at first. Time stop discipline mitigates but doesn't eliminate. Some real trend ignitions have a "gut check" pullback before extending.
- **Counter-trend fades fail in strong trends.** Fading a breakout in a strong macro trend (BTC making new ATH in a confirmed bull regime) has worse expected outcomes than fading in chop. The macro filter is essential.
- **Self-defeating risk.** Liquidation-cluster fades are widely watched; the simple version is partly priced. Edge requires confluence + macro-regime-conditioning.
- **Successor terminology disclosure.** "Turtle Soup" is Raschke's coinage. The Turtle Trader system is Richard Dennis's. The relationship is inversion-by-design — Raschke deliberately built her system as the negative of Dennis's.
- **Mechanism family alignment.** This mechanism sits in the trader's prioritized **liquidation cascade aftermath** family per Apr 28 update. It is also closely related to the **behavioral positioning unwind** family — the mechanism IS the unwind of stop-clustered late breakout buyers.

## Key Takeaways

- Failed channel breakout = stop-run, not trend ignition.
- Trigger: price exceeds N-day extreme + reverses back through it within K bars (typically 1-3).
- Edge from forced cover of stopped breakout-buyers + liquidated shorts (long-side case mirrored).
- Crypto-direct: liquidation maps make stop clusters visible; upthrust setups are the canonical Wyckoff parallel.
- Multi-channel confluence required (liquidation cluster + volume + whale flow + funding flip + RS divergence).
- Time-stop within 5 bars is essential — failed breakouts that don't reverse quickly aren't Turtle Soup.
- Macro filter required: don't fade breakouts in strong macro trends.
- Falsifier specifications testable; out-of-sample persistence required.

## Related

- [[foundation/market-structure/crypto-carrier-features]] — central catalog; carriers used (liquidation maps, funding, on-chain whale flow, OI dynamics)
- [[foundation/sources/schwager-market-wizards]] — source; Raschke's NMW 1992 interview
- [[trading/rejected/turtle-soup-4h-2026-05]] — first test result entry (bare-mechanism rejection + salvage-path table)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — cross-strategy synthesis of the regime asymmetry observed in this test
- [[trading/mechanisms/accumulation-distribution]] — Wyckoff upthrust as phase-context; spring as inverse (failed-downside-breakout)
- [[trading/mechanisms/pivotal-points]] — inverse side of the same setup; the discipline of waiting for confirmation
- [[foundation/market-wisdom/composite-operator]] — Wyckoff CO testing pattern is the behavioral framework
- [[foundation/market-wisdom/tape-reading]] — failure-to-extend is a tape-reading diagnostic (absorption pattern)
- [[foundation/market-wisdom/sit-tight-discipline]] — required for the time-stop discipline
- [[foundation/market-structure/market-manipulation-taxonomy]] — squeezer-category in stop-run setups; bluffer-category in deliberate fakeouts
- [[investment/allocation/equity-management-rules]] — position sizing for the strategy
- [[foundation/methodology/strategy-validation-discipline]] — Marcos' Third Law trial counting + falsifier discipline (relevant context for the first-test rejection)
- [[foundation/asset-classes/btc-market-structure]] — BTC continuation character; this mechanism class FAILS on BTC structurally (N=3 cross-mechanism evidence) — fade-direction is structurally wrong
- [[foundation/asset-classes/sol-market-structure]] — SOL long-bias regime amplifier; this mechanism class produces LONG-side edge at sub-deployment magnitude on SOL
- [[trading/rejected/failed-breakout-phase-fade-btc-2026-05]] — N=161 BTC rejection (mechanism geometrically broken)
- [[trading/rejected/failed-breakout-phase-fade-eth-with-position-blocking-finding-2026-05]] — ETH parent + ablation arc; position-blocking-as-inadvertent-filter methodology discovery
- [[trading/rejected/failed-breakout-phase-fade-sol-with-long-only-ablation-2026-05]] — SOL ablation arc; long-bias regime amplifier confirmed
- [[trading/rejected/range-tunnel-wyckoff-eth-multi-bar-flow-2026-05]] — multi-bar + flow filter variant; edge-saturation character discovered
- [[trading/mechanisms/breakout-direction-asymmetry-by-macro]] — **inverse mechanism context**: BTC 4h breakouts work in bull at R:R 1:2 / fail in bear; this page documents when bear-regime BTC produces the failed-breakouts that THIS mechanism would aim to capture (note: failed-breakout-reversal is rejected on BTC per the per-symbol asymmetry framework — the BTC-bear-failure of *outgoing* breakouts in the companion page does NOT salvage failed-breakout-reversal on BTC)
- [[foundation/methodology/team-stable-range-detector-methodology]] — refined data layer; bar-level overlay made the breakout-mechanism / fade-mechanism direction distinction operationally clean
- [[foundation/methodology/bar-level-validation-overlay]] — generalized pattern; per fade/mean-reversion mechanism-class mapping, this mechanism (when re-run on refined data layer) should gate on `team_is_stable_anchored` (NOT a breakout-direction bar). Existing test results in the 2026-05-18/19 batch used the raw `is_stable` layer pre-refinement.
- [[foundation/methodology/small-sample-validation-discipline]] — PSR/CI discipline applied to per-symbol asymmetry evidence
- Apr 28 mechanism families — sits in *liquidation cascade aftermath* and *behavioral positioning unwind*

## Sources

- Schwager, *The New Market Wizards* (1992), Linda Bradford Raschke interview. Source: [[foundation/sources/schwager-market-wizards]]. Note: secondary-source reconstruction; primary text not in wiki.
- Distillation notes at `raw/books/schwager-market-wizards/distillation.md` Section D.3.
- Cross-source: [[foundation/sources/wyckoff-method]] for upthrust phase-context.
