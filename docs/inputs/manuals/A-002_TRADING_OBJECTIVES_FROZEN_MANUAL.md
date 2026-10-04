# A-002 — TRADING OBJECTIVES

**Status:** FROZEN  
**Phase:** Legacy Documentation — Phase 1  
**System:** DEAD / REFERENCE ONLY  
**Freeze date:** 22-Sep-2026

## 1. Purpose
A-001 established what the legacy system was. A-002 establishes the trading
objectives supported by preserved evidence. This chapter records historical
objectives only and does not define V2.

## 2. Primary objective
**FACT / HISTORICAL:** The legacy system was intended to automate processing
and execution of externally generated Chartink opportunities for NSE equities,
subject to configured qualification, risk, capital, execution and lifecycle
controls.

The preserved overview describes Chartink → webhook → decision → sizing →
order supervision → intraday square-off → reconciliation → reporting.

“Bug-free system” and “improved system” are future project goals, not legacy
trading objectives.

## 3. Signal-to-trade objective
**FACT / HISTORICAL:** Signals were processed through validation/screening and
decision gates before entry. A failed screen stopped the signal.

**OBJECTIVE:** Avoid signals failing configured admission/risk conditions.

**UNKNOWN:** A formal historical objective that every valid Chartink signal
must become a trade is not established.

## 4. Trade-selection objective
**FACT / HISTORICAL:** The system contained screening and quality-scoring
mechanisms. The screener produced pass/reject status, score, tier, rejected
step, step results, errors, latency and market-data snapshot.

**UNKNOWN / NOT FROZEN:** Universal “quality over quantity”, fixed 10
intraday/2–3 delivery counts, or a universal ranking policy are not established
as formal historical objectives.

## 5. Entry objective
**FACT / HISTORICAL:** The entry path qualified signals, derived entry/SL,
applied configured controls and placed/supervised orders. The preserved source
also states that a fresh live quote could re-anchor entry, SL, target and sizing.

**FACT:** Price/slippage controls existed in the execution architecture.

**OPEN DETAIL:** Exact semantics of Chartink price, webhook price, fresh LTP,
calculated entry, release price, submitted price, fill price and timeout clock
belong to later entry/execution chapters.

The proposed future 5-minute confirmation rule is not frozen here.

## 6. Risk/loss-control objective
**FACT / HISTORICAL:** Risk controls, sizing, capital admission, stop-loss
handling and capital reservation were part of the trading lifecycle.

**OBJECTIVE:** Prevent execution when configured risk/capital conditions fail.

Exact numerical risk/daily-loss/drawdown/position limits are deferred to the
configuration and risk chapters.

The repository contains concentration-related mechanisms; therefore they are
not erased from the historical record merely because future design may remove
them.

## 7. Profit/exit objective
**FACT / HISTORICAL:** Stop-loss and target handling were part of the lifecycle.

**FACT:** Intraday MIS/CO positions were designed to be flattened the same day;
delivery CNC positions could carry overnight with broker-side OCO GTT.

**UNKNOWN:** No universal profitability, win-rate or return target is frozen
without direct configuration evidence. Strategy-specific R:R values belong
to later configuration/strategy chapters.

## 8. Intraday vs delivery
**FACT / HISTORICAL**
- Intraday: MIS/CO; same-day flattening.
- Delivery: CNC; overnight carry; broker-side OCO GTT.
- Capital handling distinguished intraday and positional buckets.

Detailed product, GTT, square-off and capital mechanics are deferred.

## 9. Capital objective
**FACT / HISTORICAL:** Capital management was an explicit objective. Evidence
shows separate buckets, risk admission, reservation before order placement,
release, broker reconciliation and the invariant:

`available + reserved + used == total`

The capital ledger uses write-ahead behavior and checks the invariant after
mutation.

Detailed numerical broker-margin semantics are deferred to the capital chapter.

## 10. Execution-quality objective
**FACT / HISTORICAL:** The execution path included price/slippage controls,
order monitoring and cancellation/timeout mechanisms.

**OBJECTIVE:** Avoid uncontrolled/stale execution and use the configured
execution basis.

**KNOWN DEFECT / OPEN DETAIL:** User-reported slippage, timeout and price
semantic issues remain later audit subjects; they do not rewrite the objective.

## 11. Automation objective
**FACT / HISTORICAL:** The system was designed for unattended/headless
operation with startup/pre-flight checks, scheduled jobs, service operation,
kill-switch controls, EOD processing, reconciliation and alerts.

Human intervention was principally an operational/emergency control.

## 12. Trading-day objective
**FACT / HISTORICAL:** Weekend/holiday guards and service-window controls
supported operation on valid trading days. Detailed schedules belong to A-003.

## 13. Reporting/visibility objective
**FACT / HISTORICAL:** Telegram/email alerts and report-generation capability
were part of the preserved system.

Future GUI reporting is excluded from the legacy baseline.

## 14. Performance targets
The following remain **UNKNOWN / NOT ESTABLISHED as universal legacy
objectives** unless later configuration evidence proves them:
- numerical profitability target;
- universal return percentage;
- universal win-rate target;
- universal expectancy;
- universal fill-rate target;
- universal rejected-signal-rate target;
- universal execution-latency target.

## 15. Objective vs implementation vs defect

| Objective | Evidence/mechanism | Classification |
|---|---|---|
| External signal automation | Chartink webhook → signal pipeline | FACT |
| Signal qualification | screening / hard gates / score | FACT |
| Controlled capital use | risk admission + reservation + ledger | FACT |
| Loss protection | SL/risk controls | FACT |
| Profit exit | target/exit lifecycle | FACT |
| Intraday completion | MIS/CO EOD square-off | FACT |
| Delivery continuation | CNC + GTT/OCO | FACT |
| Unattended operation | service/startup/scheduler/control planes | FACT |
| Operational visibility | Telegram/email + reports | FACT |
| Fixed 10-minute successful timeout | implementation issue under audit | KNOWN DEFECT / OPEN |
| Universal 1:1.5 R:R | not proven universal | UNKNOWN |
| Fixed 10/2–3 trade counts | user-stated target, not proven historical | EXCLUDED |
| Six future strategies | future redesign | EXCLUDED |
| Live-only operation | future redesign | EXCLUDED |
| Independent future processors | future redesign | EXCLUDED |
| New S&R/filtering | future redesign | EXCLUDED |
| Future GUI reporting | future redesign | EXCLUDED |

## 16. Evidence boundary
The legacy manual identifies the PC worktree/testing-VM-preserved evidence as
the primary inspected basis and states that production itself was never read.
Production statements therefore remain record-derived or not established.

## 17. FINAL FROZEN A-002 STATEMENT

> **The legacy trading objective was to automate the processing and execution
> of externally generated Chartink trading signals for NSE equities through
> Zerodha, while applying configured qualification, risk, capital, execution,
> position-management and exit controls, supporting separate intraday and
> delivery books, and providing automated operational visibility and
> reconciliation.**

## 18. Explicit exclusions
Future V2 architecture, live-only operation, paper-mode removal, six-strategy
scope, independent future processors, redesigned filtering/S&R, revised price
semantics, redesigned timeout/watchman, future GUI reporting and unproven
universal numerical performance targets are excluded from the legacy baseline.

## 19. Freeze decision
**A-002 — FROZEN.**

Detailed mechanics, formulas, configuration values, defects and implementation
evidence remain owned by their respective later agenda/manual chapters.

**END OF A-002**
