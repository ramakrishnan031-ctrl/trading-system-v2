# A-001 SYSTEM PURPOSE: LEGACY MANUAL (REVISED)

Status: FROZEN. Approved by Rama on 2026-10-05 (after the ChatGPT value-added review). Recorded as DR-013.
Date: 2026-10-05
Layer: LEGACY ONLY. It describes what the legacy system was and was intended to be. V2 proposals live in docs/governance/V2_DESIGN_LOG.md (section 3) and decisions in V2_DECISION_REGISTER.md. Nothing here is a V2 decision.
Working copy of: docs/inputs/manuals/A-001_SYSTEM_PURPOSE_FROZEN_MANUAL.md (original untouched; this copy removes 9 hidden citation markers and a formatting fault).
Labels: FACT, HISTORICAL, KNOWN DEFECT (USER-REPORTED), UNKNOWN / NOT ESTABLISHED, INTERPRETATION.
Security: no credential values.

## 1. Legacy system purpose

> The legacy system was a headless, automated trading application for NSE equities (instrument universe per A-004). It consumed externally generated Chartink scanner signals and used Zerodha as its trading broker, processing signals through configured validation, trade qualification, risk and capital controls, order execution, position and exit management, reconciliation, alerts and reporting. Intraday trading was its core operating mode; it also contained delivery/positional/holding handling whose behaviour and quality are documented separately. Normal operation was intended to be unattended, with explicit operator safety controls permitting manual intervention when required.

This is a purpose statement. It does not claim that every mechanism worked correctly or that every repository component was active in production.

## 2. Primary role

| Item | Label | Finding |
|---|---|---|
| Market and segment | FACT | NSE, equity |
| Broker | FACT | Zerodha (KiteConnect adapter) |
| Signal source | FACT | Chartink |
| Signal ingress | FACT | HTTP webhook |
| Implementation | FACT | Python |
| Operation | FACT | Headless, unattended in normal operation |

## 3. External signal source
- FACT: Chartink was the configured external scanner/signal source. The system consumed its alerts and generated no scanner signals of its own. Rama states that 100% of signals came from Chartink.
- Detail (scanner, webhook, authentication, deduplication, queue) belongs to section C.

## 4. Broker and execution boundary
- FACT: the system was intended to place orders through Zerodha, and it did place real broker orders (Rama; repository adapter broker/zerodha_adapter.py).
- HISTORICAL / RECORD-DERIVED: the repository README states that live trading with real capital began on 11-May-2026.
- DEFERRED: exact order types, SL/target responsibility, fill handling, retries and reconciliation authority belong to section G. Rama asked that these be re-established from the current system.

## 5. Automation and operating model
- FACT: designed for unattended normal operation on a VM, with start-up and shutdown following configured service windows and trading days following NSE (holidays and weekends excluded).
- Rama's description of the intended day: start-up, health/pre-flight check, Zerodha token, instrument list, config check, receive Chartink signals, validate, decide (entry or reject), trade follow-up, capital management, P&L, Telegram and email alerts, reports, close, independent cron jobs, preparation for the next trading day (INTERPRETATION: "next trading day", not the next calendar day).
- FACT: operator safety controls existed, including SOFT_KILL and HARD_KILL with manual interruption and release.
- FACT: the code supported paper and live modes (README: main.py --mode paper|live). Production ran live. Paper behaviour is out of scope here.
- HISTORICAL: Ubuntu VM for production, Windows 11 PC for development and debugging (Rama). See section 10 for the VM evidence boundary.

## 6. Major lifecycle responsibilities (FACT, from the production baseline)
Signal ingress, pre-flight, scanner-to-strategy resolution, strategy controls, secondary screening with hard gates and score, entry/SL derivation, position sizing, risk and capital admission, capital reservation, order placement, fill monitoring, SL/target protection, reconciliation, exit, capital release and P&L, end-of-day lifecycle, alerts, reports and evidence.
- FACT (Rama; baseline): trade decisions used one centralised secondary screening/score for all signal types.
- Evidence rule: code presence is not proof of activation. The README states that the regime, S&R detector, V3 chain and allocation engines were shadow / default-OFF.

## 7. Boundaries: what it did and did not do
- FACT: did not generate its own scanner signals.
- FACT (Rama): traded only NSE equity through Zerodha. The exact eligible universe belongs to A-004.
- FACT (Rama): trade decisions were automated from configuration, not discretionary manual decisions.
- NOT ESTABLISHED: evidence reviewed for A-001 does not establish that the system was designed as a general-purpose long-term portfolio-management platform. Delivery/positional/holding capability did exist (section 8).

## 8. Historical scope: intraday core, with delivery/positional handling
- FACT: the repository README describes the system as an automated NSE intraday trading system, and the production baseline repeats this.
- FACT / HISTORICAL: delivery/positional handling existed: separate intraday and positional capital buckets with no cross-bucket borrowing, broker-side CNC protection for supported overnight delivery positions, and end-of-day scanners routed to a watchlist.
- KNOWN DEFECT (USER-REPORTED): Rama considers the holdings handling unsatisfactory: holdings had no auto square-off, and there was no per-holding Telegram status (purchase date, quantity, entry, SL, target).
- Detailed capital, product and lifecycle mechanics: A-005, A-007 and later topics.

## 9. Known purpose-relevant defects (user-reported; mechanics deferred to section D and later)
- Late entry.
- Unsuitable or incomplete filtration; decision-criteria bugs in general.
- Incorrect or unsuitable S&R range or interpretation.
- Entry near high-of-day without adequate S&R context.
- Price ambiguity: Chartink price, system price, live price, submitted price and actual fill are confused.
- Slippage/cushion prices not honoured.
- Entry timeout: a fixed 10-minute entry timeout was intended, but orders were cancelled within about 2 minutes, with no reliable watcher.
- Holdings handling (section 8).

## 10. Evidence status and period
- Repository: trading-system-prod-snapshot, branch main, 1,537 commits. Baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (HEAD verified 2026-10-05).
- Period: the README claims live trading since 11-May-2026. The legacy storage record shows commits dated 17-Apr and 18-Apr-2026, so development history started no later than mid-April 2026. The exact first and last commit dates were not retrieved: UNKNOWN.
- Both Oracle VMs were terminated and their instances deleted about 1-2 weeks before 2026-10-05 (exact date not recorded). No backup of the production or testing DB, logs or crontab was taken.
- The production machine was never read (legacy record), and it can no longer be read. Runtime claims therefore rest on the repository, preserved records and preserved captures (D:\Projects\_preservation, trading-system-evidence, _snapshots).

## 11. Production versus sandbox
Production and the testing (sandbox) VM were separate systems with different code states and configuration. This manual describes the production baseline (970aabf); do not mix in sandbox findings.

## 12. Unknown / not established
- The exact historical traded instrument universe (A-004).
- Production runtime configuration, crontab and database state.
- Whether each repository component was active (live, shadow or dormant) in production.
- Exact first and last commit dates; exact VM termination date.
- Detailed execution responsibilities (section G).

## 13. Historical / V2 boundary
V2 ideas raised during A-001 are not part of this manual. They are recorded as PROPOSED in V2_DESIGN_LOG.md: the V2 purpose and scope statements, per-signal-type processors, the operator boundary, V2 environments, the holdings Telegram summary, config-driven start/stop, the headless token, and backup and restore. Live-only (DR-001) and the GUI download (DR-009) are already V2 decisions in the register.

## 14. Cross-references
A-002 (objectives), A-003 (hours), A-004 (instruments), A-005 (capital), A-006 (broker), A-007 (product types), A-008 (strategy), A-009 (configuration), A-010 (secrets), A-011 (states and lifecycle); later sections C, D, G, H, L.

## 15. Final statements (FROZEN, approved 2026-10-05)
- PURPOSE: section 1.
- SCOPE: NSE equity; Zerodha; Chartink signals; automated lifecycle from signal to reconciliation and reporting; intraday core with delivery/positional handling.
- NON-SCOPE: the system did not originate Chartink scanner signals; it is not established as a general-purpose portfolio-management platform.
- GOVERNANCE: no V2 architecture, redesign, fix, tuning or replacement behaviour is included in A-001.
