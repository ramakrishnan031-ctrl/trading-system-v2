# A-002 TRADING OBJECTIVES: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-05 (revision 2, after the ChatGPT value-added review). Approved by Rama. Recorded as DR-014.
Date: 2026-10-05
Layer: LEGACY ONLY. Historical objectives, not V2. V2 ideas are PROPOSED in docs/governance/V2_DESIGN_LOG.md section 3.
Working copy of: docs/inputs/manuals/A-002_TRADING_OBJECTIVES_FROZEN_MANUAL.md (frozen 22-Sep-2026 in the ChatGPT phase; original untouched). Changes are listed at the end.
Labels: FACT, HISTORICAL, KNOWN DEFECT (USER-REPORTED), UNKNOWN / NOT ESTABLISHED, INTERPRETATION, OPEN.
Security: no credential values.

## 1. Purpose
A-001 established what the legacy system was. A-002 records the trading objectives supported by preserved evidence. It does not define V2.

## 2. Primary objective
- FACT / HISTORICAL: automate the processing and execution of externally generated Chartink opportunities for NSE equities, subject to configured qualification, risk, capital, execution and lifecycle controls.
- The preserved overview describes Chartink, webhook, decision, sizing, order supervision, intraday square-off, reconciliation and reporting.
- "Bug-free system" and "improved system" are future project goals, not legacy trading objectives.

## 3. Signal-to-trade objective
- FACT / HISTORICAL: signals passed validation, screening and decision gates before entry; a failed screen stopped the signal.
- OBJECTIVE: avoid signals that fail configured admission or risk conditions.
- FACT: signals were deduplicated at the webhook.
- FACT: a legacy duplicate-avoidance rule allowed one trade per symbol and direction per day within the applicable trading segment/product context (baseline; Rama). Exact cross-segment behaviour is documented in A-007.
- UNKNOWN: an objective that every valid Chartink signal must become a trade is not established.
- KNOWN DEFECT (USER-REPORTED): no end-to-end confirmation of what happened to each received signal.

## 4. Trade-selection objective
- FACT: one central configuration. The preserved flow distinguished intraday and delivery/positional processing paths, with shared central components (configuration, screening, order management, reporting) where evidenced. Rama describes them as independent pipelines.
- FACT: multiple Chartink scanners were mapped to strategy identities, with strategy configuration kept in strategy YAML files.
- UNKNOWN / NOT ESTABLISHED (A-008 owns it): the exact production strategy-YAML count and membership. Evidence differs: 16 historical Chartink scanner definitions (including PB01, marked SHADOW) and a locked-decision record listing 15 strategy YAML filenames (both per ChatGPT's A-008 evidence); 16 strategy YAMLs in the storage record, which describes the testing VM; Rama recalls 16 strategies. The 16 scanners are not a count of production strategies or YAMLs.
- FACT: one centralised secondary screening served all strategies (pass/reject, score, tier, minimum score), with centralised order management, alerts and reports (Rama; baseline).
- FACT: the screener produced pass/reject status, score, tier, rejected step, step results, errors, latency and a market-data snapshot.
- FACT: concentration-related mechanisms existed (sector data capping; a concentration quantity in sizing). They are not erased because a future design may drop them.
- KNOWN DEFECT (USER-REPORTED): some secondary-filter criteria were placeholders and not applied.
- UNKNOWN / NOT FROZEN: a universal "quality over quantity" rule, fixed 10 intraday / 2-3 delivery trade counts, or a universal ranking policy.

## 5. Entry objective
- FACT / HISTORICAL: the entry path qualified signals, derived entry and SL, applied configured controls and placed and supervised orders. A fresh live quote could re-anchor entry, SL, target and sizing.
- FACT: price and slippage controls existed in the execution architecture.
- HISTORICAL (Rama): the intent was an entry released with a slippage cushion based on the stock's price, so the fill lands within the intended price.
- KNOWN DEFECT (USER-REPORTED): the slippage cushion was not honoured (A-001).
- OPEN DETAIL: the exact meaning of Chartink price, webhook price, fresh LTP, calculated entry, release price, submitted price, fill price and the timeout clock belongs to later entry and execution chapters.
- A 5-minute confirmation rule is a future proposal (VP-09) and is not frozen here.

## 6. Risk / loss-control objective
- FACT / HISTORICAL: risk controls, sizing, capital admission, stop-loss handling and capital reservation were part of the trading lifecycle.
- OBJECTIVE: prevent execution when configured risk or capital conditions fail.
- CONFLICT (OPEN, resolve at A-005): Rama recalls capital per trade = capital including margin / maximum trades per day, capped by a strategy limit. The production baseline shows risk-based sizing (the minimum of risk, capital and concentration quantities, times a tier multiplier). A-002 does not resolve the sizing formula; both accounts are retained.
- Exact numerical risk, daily-loss, drawdown and position limits are deferred to the configuration and risk chapters.

## 7. Profit / exit objective
- FACT / HISTORICAL: stop-loss and target handling were part of the lifecycle, including software OCO (the other leg is cancelled when SL or target fills).
- FACT: intraday MIS/CO positions were flattened the same day; delivery CNC positions could carry overnight with broker-side OCO GTT.
- HISTORICAL (user recollection): fresh entries stopped at a configured cut-off time, and intraday auto square-off ran around 15:10. Exact production timing is deferred to A-003.
- OPEN: whether a "smart target manager" that adjusted target ranges existed (Rama's mention) is to be verified at E-007 / H.
- UNKNOWN: no universal profitability, win-rate or return target is frozen without configuration evidence. Strategy R:R values belong to later chapters.

## 8. Intraday vs delivery (FACT / HISTORICAL)
- Intraday: MIS/CO; same-day flattening.
- Delivery: CNC; overnight carry; broker-side OCO GTT.
- Capital handling separated the intraday and positional buckets.
- Context only, not an objective (HISTORICAL, Rama): intraday capital was treated with 5x margin and delivery with 1x. This does not mean every intraday instrument or order received 5x. Leverage and margin semantics belong to A-005, A-006 and A-007.
- Detailed product, GTT, square-off and capital mechanics are deferred (A-005, A-007).

## 9. Capital objective
FACT / HISTORICAL: capital management was an explicit objective: separate buckets, risk admission, reservation before order placement, release, broker reconciliation and the invariant `available + reserved + used == total`, with a write-ahead ledger checked after every change. Detailed broker-margin semantics are deferred to A-005.

## 10. Execution-quality objective
- FACT / HISTORICAL: execution controls existed: price/slippage controls, order monitoring and cancellation/timeout mechanisms.
- OBJECTIVE: avoid uncontrolled, stale or duplicate execution and use the configured execution basis.
- KNOWN DEFECT (USER-REPORTED): the intended timeout, slippage cushion and price/time semantics were not reliably honoured (the 10-minute entry timeout cancelled early; price and time meant different things in different modules).
- The existence of these controls does not show that execution quality was achieved.

## 11. Automation objective
FACT / HISTORICAL: designed for unattended, headless operation with start-up and pre-flight checks, scheduled jobs, service operation, kill-switch controls, EOD processing, reconciliation and alerts. Human intervention was mainly an operational or emergency control.

## 12. Trading-day objective
- FACT / HISTORICAL: weekend/holiday guards and service-window controls supported operation on valid trading days. Detailed schedules belong to A-003.
- FACT: EOD scanner signals were routed to a watchlist/EOD path and never entered the intraday queue (baseline).
- UNKNOWN: whether the system traded next day on stored EOD signals.
- "Analyse today, trade tomorrow" is a V2 proposal (VP-14), not a legacy objective.

## 13. Reporting / visibility objective
- FACT / HISTORICAL: Telegram/email alerts and report generation were part of the preserved system.
- Holding-status reporting was a later request, not a legacy objective (Rama).
- Future GUI reporting is excluded from the legacy baseline.

## 14. Performance targets
- HISTORICAL (Rama): trading values were configuration-driven (one config file plus per-strategy YAMLs) rather than fixed in code.
- UNKNOWN / NOT ESTABLISHED as universal legacy objectives unless later configuration evidence proves them: numerical profitability target; universal return percentage; universal win rate; universal expectancy; universal fill rate; universal rejected-signal rate; universal execution latency; a universal 1:1.5 R:R.

## 15. Objective vs implementation vs defect

| Objective | Evidence / mechanism | Classification |
|---|---|---|
| External signal automation | Chartink webhook to signal pipeline | FACT |
| Signal qualification | Screening, hard gates, score | FACT |
| Duplicate avoidance | Webhook dedup; one trade per symbol and direction per day within segment/product context | FACT |
| Controlled capital use | Risk admission, reservation, ledger | FACT |
| Loss protection | SL and risk controls | FACT |
| Profit exit | Target and exit lifecycle, software OCO | FACT |
| Intraday completion | MIS/CO EOD square-off | FACT |
| Delivery continuation | CNC with GTT/OCO | FACT |
| EOD signal capture | Watchlist/EOD path | FACT |
| Unattended operation | Service, start-up, scheduler, controls | FACT |
| Operational visibility | Telegram/email and reports | FACT |
| Signal outcome visibility | No end-to-end confirmation per signal | KNOWN DEFECT |
| Complete filtration | Some criteria were placeholders | KNOWN DEFECT |
| Fixed 10-minute timeout | Not honoured; under audit | KNOWN DEFECT / OPEN |
| Consistent price/time meaning | Differs across modules | KNOWN DEFECT |
| Sizing basis | Rama's recollection vs baseline differ | CONFLICT / OPEN (A-005) |
| Production strategy-YAML count | 15 vs 16 evidence; PB01 marked SHADOW | UNKNOWN / OPEN (A-008) |
| Next-day trading on EOD signals | Not established | UNKNOWN |
| Universal 1:1.5 R:R | Not proven universal | UNKNOWN |
| Fixed 10 / 2-3 trade counts | User-stated target, not proven historical | EXCLUDED |
| Future items (V2_DESIGN_LOG VP-01 to VP-18) | Future redesign | EXCLUDED |

## 16. Evidence boundary
- Primary basis: the production snapshot at baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (as named in the A-002 checklist), with the production baseline document, records and Rama's replies.
- Conflict recorded: the original manual named the PC worktree / testing-VM evidence as its primary basis. Production and sandbox were separate systems; statements here describe production unless marked otherwise.
- The production machine was never read. Both VMs were deleted about 1-2 weeks before 2026-10-05 with no backups, so production statements are record-derived or not established (as in A-001 section 10).

## 17. Final A-002 statement
> The legacy trading objective was to automate the processing and execution of externally generated Chartink trading signals for NSE equities through Zerodha, while applying configured qualification, risk, capital, execution, position-management and exit controls, supporting separate intraday and delivery books, and providing automated operational visibility and reconciliation.

## 18. Explicit exclusions
Future V2 items are excluded from the legacy baseline: live-only operation, paper-mode removal, the six-strategy scope, per-strategy processors and filtration, new S&R and filtering, 5-minute confirmation, slippage slabs, revised price semantics, redesigned timeout/watchman, one trade per stock per day, removal of concentration caps, capital-per-trade sizing, intraday config changes, holding-status alerts, GUI reporting, metric research and the post-agenda audit. They are recorded as PROPOSED in V2_DESIGN_LOG.md (VP-01 to VP-18).

## 19. Freeze decision
FROZEN 2026-10-05 (DR-014). Detailed mechanics, formulas, configuration values, defects and implementation evidence stay with their later chapters.

## Changes from the original (22-Sep-2026)
Revision 2 (after ChatGPT review): strategy count moved to UNKNOWN (A-008); duplicate rule qualified by segment/product context; 15:10 recorded as recollection; 5x/1x recorded as context only; processing paths no longer called fully independent; execution-quality and EOD wording tightened.
Added in revision 1: duplicate avoidance (3); legacy structure, concentration detail and placeholder criteria (4); slippage-cushion intent (5); sizing conflict (6); software OCO, 15:10 square-off, smart-target question (7); 5x/1x margin note (8); outcome-visibility, timeout and price/time defects (3, 10); EOD watchlist routing (12); holding status as later request (13); configuration-driven values (14); new table rows (15); evidence boundary rewritten (16); exclusions list widened (18).
