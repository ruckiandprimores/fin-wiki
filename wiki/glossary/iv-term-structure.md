---
title: "IV Term Structure (IVTS)"
status: evergreen
created: 2026-05-08
updated: 2026-05-08
tags: [glossary, volatility, term-structure, sinclair, terminology]
---

# IV Term Structure (IVTS)

The **shape of implied volatility across expiries** at fixed strike (typically ATM). The fundamental indicator of how the option market prices vol decay over time.

## Definition

Sinclair Ch 12 defines the simple form: **IVTS = VIX(30d) / VXV(90d)**.

| IVTS value | Term-structure shape | Regime |
|---|---|---|
| IVTS > 1 | **Backwardation** (front IV > back IV) | Vol-panic / event-imminent / fear-spike |
| IVTS ≈ 1 | Flat | Transitional |
| IVTS < 1 | **Contango** (front IV < back IV) | Calm / normal / vol expected to expand |

## Interpretation

- **Contango is the default state** in equity (and crypto). Markets price future vol slightly higher than current vol because (a) variance premium accumulates with time, (b) options sellers demand insurance for longer tenors, (c) mean reversion expectation pulls front vol toward long-term average from below.
- **Backwardation appears in stress regimes**. Front IV spikes (immediate fear); back IV catches up but lags. The term-structure inverts.
- **The flip from contango to backwardation is itself a regime indicator** — it captures the moment when forward expectation shifts.

## Operational uses

- **Regime classifier**: gate strategies by IVTS bucket (Donninger 2011 dynamic VXX/VXZ allocation example in Sinclair Ch 12).
- **Basis trade input**: IVTS ratio tells you when vol-futures basis is meaningfully dislocated.
- **Implied event jump**: from front and back IV around an event, derive expected event magnitude (Sinclair Eq 5.3 — see [[trading/mechanisms/term-structure-dynamics]]).

## Sinclair's Donninger 2011 example weights

| IVTS bucket | VXX weight | VXZ weight |
|---|---:|---:|
| IVTS ≤ 0.91 | −0.60 | 0.40 |
| 0.91 < IVTS ≤ 0.97 | −0.32 | 0.68 |
| 0.97 < IVTS ≤ 1.05 | −0.25 | 0.75 |
| IVTS > 1.05 | −0.10 | 0.90 |

(Strategy summary: short short-dated VXX in contango — capture roll-down decay; reduce short and add VXZ as IVTS approaches 1; lighten short and lean long-vol-ish in backwardation.)

Reported Sharpe 2.62 / 12% DD on 2010-2012 data — Sinclair flags as overfit-suspicious due to small window + parameter tuning. Pattern is real; magnitude not durable.

## Crypto translation

- **DVOL_30d / DVOL_90d** is the direct analog of VIX/VXV.
- Multi-tenor Deribit options provide per-strike-per-expiry IV chains; can build IVTS at any strike (not just ATM).
- DVOL futures (Deribit, since 2023) enable basis-trade analog to VIX-basis.
- Crypto term-structure responds heavily to scheduled events (FOMC, halving, ETF approvals, OPEX); can flip on news.

## Related

- [[trading/mechanisms/term-structure-dynamics]] — operational mechanism page using IVTS
- [[trading/mechanisms/vol-regime-detection]] — companion vol-mechanism
- [[foundation/sources/sinclair-volatility-trading]] — primary source (Ch 5, 12)
- [[glossary/vol-cone]] — companion glossary
- [[glossary/variance-premium]] — companion glossary

## Sources

- Sinclair, Euan. *Volatility Trading*, 2nd ed. (2013) Ch 5 (Stein 1989), Ch 12 (IVTS regime classifier, Donninger 2011)
- Stein, J. (1989). *Overreactions in the options market*. Journal of Finance.
- Donninger, K. (2011). *Strategy weights for VXX/VXZ-based ETN allocation*.
- Simon, D., Campasano, J. (2012). *VIX Futures Pricing*.
