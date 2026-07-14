---
title: "asia_range_sweep_4h — rejected (mechanism failure in 2025-26 BTC regime); 2026-05"
status: evergreen
created: 2026-05-05
updated: 2026-05-05
tags: [rejected, behavioral-exploitation, session-boundary, mutation-rejected, calibration-failure, day-swing]
---

# asia_range_sweep_4h — rejected (mechanism failure in 2025-26 BTC regime); 2026-05

> **TL;DR**: SMC sweep-and-reverse at London open (FX session-boundary mechanism translated to BTC) does not operate in 2025-26 BTC perp 4h regime. Strategy-level falsifier fired (medSh −0.68; win rate 27.7%; %PF>1 34.9%). The pre-registered calibration concern was load-bearing — *"ETF launch shifted volume to NY session; Korean premium compressed since 2017"*. Companion to [[trading/rejected/turtle-soup-4h-2026-05|turtle_soup_4h]] (time-agnostic version also fails); session-gating discipline doesn't recover the mechanism in current regime.

## Hypothesis (Structured Form)

```
Hypothesis:    Asia session range break + London open reversal — when
               Asia compresses BTC into a tight range, London-open
               participants sweep the range and reverse, providing fade
               opportunity at the sweep extreme.

Mechanism:     FX session-boundary microstructure (Lien, Drobny, Lipschutz)
               translated to BTC: institutional desk handoffs at session
               boundaries create predictable liquidity transitions.
               Asia desk closes; London desk fades the Asia range; NY
               desk extends London's bias.

Observable:    BTCUSDT perp 4h, 2025-05-03 → 2026-04-28, $1000 capital,
               fee 0.04%. Pre-registered falsifier: strategy-level
               medSh ≥ 0; win rate ≥ 50%; %PF>1 ≥ 50%.

Expected:      Sharpe +0.30 to +0.70; win rate 50-60%; medATP ≥ $1.00.
               expected_net_edge: at $1K capital, gross ≥ $5/trade after
               session-quality filter.

Falsifier:     If any of strategy-level criteria fails, mechanism does
               not transfer to BTC perp 4h in current regime.
```

## Result (2025-05-03 → 2026-04-28, BTCUSDT perp, $1000, fee 0.04%)

| Metric | Value | Falsifier criterion | Fired? |
|--------|---:|---|:---:|
| Median Sharpe | **−0.68** | ≥ 0 | **YES** |
| Median win rate | **27.7%** | ≥ 50% | **YES** |
| %PF>1 | **34.9%** | ≥ 50% | **YES** |
| Median ATP | weak | ≥ $1.00 | YES |

## Net-of-Friction (per [[foundation/methodology/deployment-edge-threshold|deployment-edge methodology]])

| Component | Value |
|---|---:|
| Gross/yr ($1K capital) | negative |
| Friction/yr (1bp roundtrip × $5K position × N trades) | ~$15-20 |
| **Net/yr** | **−$142** |

Strategy is gross-negative; friction makes it worse. **Far below deployment-grade threshold** ($120/yr at $1K capital ≈ 12% return).

## Falsifier verdict: FIRED on all 3 criteria

The pre-registered calibration concern was the load-bearing issue: *"ETF launch shifted volume to NY session; Korean premium compressed"*. The 2026-05-05 BTC regime has different session microstructure than the 2017-2019 regime where Asia-session premium was structural. Mechanism likely operated in earlier crypto regime; doesn't operate now.

## Partial Truth Retained

- **Time-agnostic confirmation**: companion [[trading/rejected/turtle-soup-4h-2026-05|turtle_soup_4h]] failed on the broader failed-breakout-reversal mechanism. Session-gating discipline (concentrating entries at London open) does NOT recover the mechanism. The mechanism family doesn't operate in this window regardless of timing filter.
- **Cross-market transfer caveat**: FX session-boundary effects don't transfer cleanly to BTC perps in 2025-26 microstructure (24/7 trading; ETF flow dominance; institutional-side US-day concentration). Per [[foundation/sources/forex-10-essentials|FX 10 Essentials]] cross-market transfer attempt — calibration to crypto microstructure is required, not just direct translation.

## Trial Count on Dataset (per [[foundation/methodology/strategy-validation-discipline|Marcos' Third Law]] §2.2)

≥13 strategies tested on BTCUSDT perp 2025-05/2026-04 window in this batch. Falsifier fired across multiple metrics — robust to multiplicity correction.

## Reason Classification

- [x] **Falsifier fired** — 3/3 strategy-level criteria
- [x] **Mechanism not present in current crypto microstructure** — calibration failure (Asia-session edge compressed)
- [x] **Cross-market transfer failure** — FX session microstructure doesn't translate
- [ ] No falsifier articulated
- [ ] Already arbitraged
- [ ] Graveyard duplicate

## Key Takeaways

- **Mechanism rejected in current BTC regime.** May have operated in 2017-2019 (Korean premium era); compressed since.
- **Session-gating discipline didn't help** — companion turtle_soup_4h showed the time-agnostic failed-breakout-reversal also fails. Mechanism family is dormant in this window.
- **Cross-market transfer requires calibration** — FX session microstructure ≠ BTC session microstructure under ETF era.
- **Pre-registered calibration concerns matter** — when the spec flags concerns about regime-applicability, those concerns should be tested explicitly, not assumed away.

## Related

- [[trading/mechanisms/session-boundary-effects]] — family page; family operates in chop-regime markets, less in trends
- [[trading/rejected/turtle-soup-4h-2026-05]] — companion rejection (time-agnostic version of the same family failure)
- [[foundation/sources/forex-10-essentials]] — FX session-boundary source; cross-market transfer translation
- [[foundation/methodology/deployment-edge-threshold]] — net-edge gate
- [[foundation/methodology/strategy-validation-discipline]] — falsifier discipline
- [[foundation/macro/regime-conditional-edge-2025-26]] — strategies don't have year-round edge; this one didn't have any-quarter edge

## Sources

- the backtesting lab — strategy files + pre-registration
- the backtesting lab — full reasoning per-strategy in companion stocktake report
