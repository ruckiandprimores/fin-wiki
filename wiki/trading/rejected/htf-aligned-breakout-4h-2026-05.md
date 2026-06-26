---
title: "htf_aligned_breakout_4h — REJECTED (HTF gate destroyed edge); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, trend-following, multi-frame, regime-gate-rejected, anti-pattern, day-swing]
---

# htf_aligned_breakout_4h — REJECTED (HTF gate destroyed edge); 2026-05

> **TL;DR**: Multi-frame alignment (4h breakout + 1d EMA bias gate) **destroyed the edge** rather than enhanced it. Result: medSh −0.04; gross −$5/yr; net −$38/yr at $1K capital. The 4h breakout signal alone (per [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05|ema_bias_breakout_4h]] — also rejected for deployment-grade reasons but with positive Sharpe) was producing edge; **adding the separate HTF gate eliminated trade volume in profitable windows**. **Worked anti-pattern** for [[trading/mechanisms/regime-gate-vs-bias-filter|the bias-filter > separate regime-gate design pattern]] — one of N=4 supporting cases, this strategy is the loser-half of the comparison.

## Hypothesis (Structured Form)

```
Hypothesis:    Stacking 4h breakout entry signal with 1d EMA bias gate
               (HTF higher-timeframe alignment) produces higher Sharpe
               than 4h breakout alone, by filtering out counter-trend
               breakouts.

Mechanism:     Classical multi-frame TA: HTF determines bias; entry
               timeframe (4h) provides signal; combination concentrates
               trades on HTF-aligned setups.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%, ~3000 parameter combinations.

Expected:      medSh ≥ +0.30; HTF gate adds value vs no-HTF-gate baseline
               (parent ema_bias_breakout_4h Sharpe +0.54 statistical-grade);
               gap ≥ +0.10.

Falsifier:     Strategy-level: medSh < 0 OR win rate degenerate.
               Hypothesis-level: HTF-gated Sharpe ≤ no-HTF-gate baseline
               (gate doesn't add value).
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **−0.04** | ≥ +0.30 | YES (close to zero; below threshold) |
| Sharpe gap vs parent (ema_bias_breakout_4h) | **−0.58** (parent +0.54, this −0.04) | ≥ +0.10 (gate should add value) | **HYPOTHESIS-LEVEL FIRED** |
| Median ATP | weak | — | — |

**Hypothesis-level falsifier fired**: HTF gate hurts edge rather than adds value.

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital) | **−$5** (gross-negative) |
| Friction/yr | **$33** |
| **Net/yr** | **−$38** |

Strategy is gross-negative; friction makes it strictly worse. **Far below deployment-grade threshold**.

## Falsifier verdict: HYPOTHESIS-LEVEL FIRED — separate HTF gate is anti-pattern

The pre-registered hypothesis was that HTF gate ADDS value. Instead, HTF gate REMOVES value:
- Parent (`ema_bias_breakout_4h`) Sharpe: +0.54 (statistical-grade pass)
- This (HTF-gated): Sharpe −0.04 (loses parent's edge)
- Gap: −0.58 (HTF gate produces 0.58 Sharpe of *negative* value)

**The separate HTF gate destroys edge.**

## Why It Failed — Classic "Separate Regime Gate" Failure Mode

Per [[trading/mechanisms/regime-gate-vs-bias-filter|the design pattern page]] §Three reasons:

1. **Timing decoupling penalty**: HTF (1d EMA) lags entry signal (4h breakout). When 4h breakout fires on a regime turn, HTF still classifies the prior regime → gate vetoes → miss the regime-turn trade.
2. **Trade volume preservation failure**: HTF gate's "must align" requirement eliminates trades during regime transitions and during chop. Even if those filtered-out trades would have been negative, the *total* trade volume drops, and friction-amortization deteriorates.
3. **Bias-filter design alternative wins**: parent strategy `ema_bias_breakout_4h` uses *embedded* EMA(20)/EMA(50) bias filter at the entry signal's timescale — this works; this strategy's *separate* HTF gate at 1d timescale doesn't.

This is **the canonical worked example** of the separate-regime-gate anti-pattern.

## Companion Cases for the Design Pattern

| Strategy class | Bias-filter version | Regime-gate version | Outcome |
|----------------|---------------------|---------------------|---------|
| EMA-aligned breakout | `ema_bias_breakout_4h` (embedded) Sharpe +0.54 | **`htf_aligned_breakout_4h` (this; separate HTF gate) Sharpe −0.04** | Bias filter wins by 0.58 |
| Bollinger squeeze | (no embedded version tested) | `bollinger_squeeze_4h_short_regime` operates only on 0.6% of grid | INCONCLUSIVE (selection bias) |
| Funding-flip recovery | (no embedded version tested) | `funding_flip_recovery_4h_short_regime` Sharpe +0.34 vs control +0.50 (gap −0.16) | Regime gate hurts |
| Direction restriction | `bollinger_squeeze_4h_short_only` +1.02; `funding_flip_recovery_4h_short_only` +0.50 | Symmetric parents −1.01, −0.11 | Direction restriction wins |

**N=4 supporting cases, this strategy is one of them.** Documented in [[trading/mechanisms/regime-gate-vs-bias-filter]] §Supporting evidence.

## Connection to Universal Stop-Target Prior

Per [[trading/mechanisms/universal-stop-target-prior]]: this strategy's modal parameters (stop_atr_mult 1.0, target_atr_mult 1.5) match the **mean-reversion prior** despite being a trend-class strategy. The mismatch is itself diagnostic — the strategy is failing because its parameter modes don't match its mechanism class. The HTF gate forces the strategy to operate in conditions where the mean-reversion-style tight stops + tight targets are the only configurations producing trades, but those configurations are mechanism-misaligned.

## Trial Count on Dataset

≥13 strategies tested in this batch. Hypothesis-level falsifier fired with margin (gap −0.58); robust to multiplicity correction.

## Reason Classification

- [x] **Hypothesis-level falsifier fired** — HTF gate degrades vs no-HTF baseline
- [x] **Strategy-level borderline** — medSh −0.04 close to zero
- [x] **Deployment-grade fails** — net −$38/yr
- [x] **Anti-pattern documented** — separate-HTF-gate design pattern fails
- [x] **One of N=4 supporting cases** for [[trading/mechanisms/regime-gate-vs-bias-filter|bias-filter > separate regime gate]] design pattern
- [ ] No falsifier articulated
- [ ] Mechanism absent

## Key Takeaways

- **HTF gate destroyed edge** rather than enhanced it: Sharpe gap −0.58 vs parent without HTF gate.
- **Anti-pattern documented**: separate HTF gate at longer timescale than entry signal introduces timing decoupling + trade volume loss + bias-filter design alternative wins.
- **One of N=4 supporting cases** for [[trading/mechanisms/regime-gate-vs-bias-filter|the embedded-bias-filter > separate-regime-gate design pattern]].
- **Parameter mode mismatch is diagnostic**: this strategy's modes (1.0 stop, 1.5 target) match mean-reversion prior despite being trend-class — confirms the strategy is misarticulated per [[trading/mechanisms/universal-stop-target-prior]] §Outliers.
- **Lesson for future strategies**: prefer embedded bias filter at design time over separate post-hoc HTF gate. Same principle as the Part I §9 direction-restriction discipline applied at the bias-filter timescale.

## Related

- [[trading/rejected/ema-bias-breakout-4h-symmetric-2026-05]] — parent strategy (embedded-bias-filter version; statistical-grade pass; deployment-grade fail) — HTF-gated this strategy is the comparison loser
- [[trading/rejected/ema-bias-breakout-4h-v2-2026-05]] — heat-map-refined parent variant (verdict reversal under deployment-grade)
- [[trading/mechanisms/regime-gate-vs-bias-filter]] — design pattern; this strategy is one of N=4 supporting cases (the canonical separate-HTF-gate-fails worked example)
- [[trading/mechanisms/moving-averages]] — canonical bias-filter mechanism
- [[trading/mechanisms/universal-stop-target-prior]] — parameter-mode mismatch as mechanism-misarticulation diagnostic
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate
- [[foundation/methodology/strategy-validation-discipline]] — paired-falsifier discipline (parent passed; this one failed)
- Part I §9 — embedded vs separate filter discipline (analogous to direction-restriction discipline)

## Sources

- the backtesting lab — strategy files
- the backtesting lab — parent (no-HTF-gate baseline for comparison)
- the backtesting lab — full reasoning
