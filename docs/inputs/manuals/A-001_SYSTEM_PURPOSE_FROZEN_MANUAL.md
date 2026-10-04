# A-001 — SYSTEM PURPOSE

**Status: FROZEN — Phase 1 legacy documentation baseline**  
**System status: DEAD / REFERENCE ONLY**

## 1\. Frozen purpose

**FACT**

The legacy system was an automated Python-based trading system for Indian NSE equity trading, integrated with Zerodha as its broker and consuming external Chartink scanner signals through an HTTP webhook.

Its implemented lifecycle included signal validation/processing, screening and trade decision, price derivation, sizing, risk/capital checks, capital reservation, broker order placement, fill/order supervision, trade/exit management, broker reconciliation, alerts and reporting.

**Frozen one-sentence purpose:**

> An automated, headless NSE equity trading system that consumed external Chartink signals and, subject to its configured controls and decision pipeline, processed, executed, managed, reconciled, and reported trades through Zerodha.

## 2\. Scope

|Item|Classification|Finding|
|-|-|-|
|Market|FACT|Indian NSE|
|Trading segment|FACT|NSE Equity|
|Broker|FACT|Zerodha|
|External signal source|FACT|Chartink|
|Signal ingress|FACT|HTTP webhook|
|Automated lifecycle|FACT|Signal → decision → sizing/risk → execution → management → reconciliation/reporting|

The system overview explicitly describes an NSE equity system using Zerodha and external Chartink signals. fileciteturn2file1

**UNKNOWN / NOT ESTABLISHED:** The exact set of NSE equity instruments actually traded historically cannot be proven from repository code alone.

## 3\. Signal origin

**FACT**

Chartink was the external signal source. The system consumed scanner alerts through its webhook receiver rather than generating the originating scanner signals itself. The preserved lifecycle starts with Chartink → webhook receiver → signal processor. fileciteturn2file2

The webhook path included authentication, validation, scanner lookup, deduplication, persistence and queueing before processing. fileciteturn2file2

## 4\. Automation scope

**FACT**

The preserved architecture documents this lifecycle:

Chartink → webhook → validation → signal processing → screening → price construction → sizing → risk approval → capital reservation → order placement → Zerodha → fill/order monitoring → open trade → SL/TGT/protection/reconciliation → exit → P\&L/capital release → reporting/alerts.

The signal processor is described as an orchestrator rather than one monolithic decision function. fileciteturn2file7

**Evidence rule:** Code/module presence alone is not treated as proof that a feature was enabled or effective in every production runtime.

## 5\. Broker and execution

**FACT**

Zerodha was the broker integrated into the trading path. The repository/source evidence identifies `broker/zerodha\_adapter.py` and the KiteConnect SDK as the broker interface. fileciteturn2file1

**HISTORICAL / RECORD-DERIVED**

The preserved system documentation describes live real-money production operation.

**UNKNOWN / NOT ESTABLISHED**

The complete historical production runtime configuration, broker-side state and database state were not directly inspected.

Detailed execution mechanics are deferred to the execution/order agendas.

## 6\. Operating modes

**FACT**

The legacy code supported both **paper** and **live** modes.

Paper used a simulation path; live constructed and used the real KiteConnect path. The preserved source also records paper as the CLI default and live as the service mode. fileciteturn2file9

The user's future preference for a live-only replacement is **not part of the legacy A-001 freeze**.

## 7\. Operating model

**FACT**

The system was designed for unattended automated operation with startup/pre-flight checks, scheduled jobs, service-window behavior, shutdown/EOD behavior and operational controls.

**HISTORICAL / RECORD-DERIVED**

The preserved evidence describes a testing VM/Ubuntu environment and a Windows PC development/operations environment.

**UNKNOWN / NOT ESTABLISHED**

The complete historical production service configuration, crontab and database state were not directly inspected.

## 8\. Control and safety boundary

**FACT**

The system contained controls including kill-switch handling, capital/risk controls, reconciliation, screening/rejection controls, health checks and alerting.

Their purpose-level role was to control execution and operational state, protect capital/state consistency and provide intervention/visibility mechanisms.

Detailed effectiveness or defects belong to later agenda sections.

## 9\. System boundaries

**FACT — IN SCOPE**

* NSE equity trading.
* Zerodha integration.
* Chartink-originated signals.
* Automated qualification and execution lifecycle.
* Position/trade management.
* Reconciliation.
* Alerts/reporting.
* Scheduled operational functions.

**FACT — OUT OF SCOPE**

* The system was not itself the external Chartink scanner generating the originating alerts.

**UNKNOWN / NOT ESTABLISHED**
The available evidence does not support describing the legacy system as a complete long-term portfolio-management platform in the broad investment-management sense.

## 10\. Purpose vs implementation

**FACT**

The implementation was a staged pipeline:

Signal ingress → orchestration → screening → price construction → capital/risk admission → execution → lifecycle management → reconciliation → evidence/alerts/operations. fileciteturn2file7

**KNOWN DEFECT / HISTORICAL OBSERVATIONS**

The user's review identified later-investigation issues including:

* slippage/cushion price handling;
* ambiguity among Chartink price, system price, LTP, submitted price and actual fill;
* entry-timeout behavior reportedly not following the intended timeout.

These observations do **not** modify the frozen system purpose.

## 11\. Paper vs live

**FACT**

Both paper and live implementations existed and used materially different broker/execution paths. fileciteturn2file9

**UNKNOWN / NOT ESTABLISHED**

Complete runtime parity/differences across every component cannot be established from repository code alone.

## 12\. Historical status and evidence boundary

**FACT**

The system is now DEAD / REFERENCE ONLY for Phase 1.

The preserved system overview states that the production machine itself was never read; production configuration, crontab, units and database state are therefore not independently established unless record-derived. fileciteturn2file1

## 13\. A-001 evidence classification

* **FACT:** Core purpose, NSE equity scope, Zerodha integration, Chartink signal ingress, automated lifecycle, paper/live capability.
* **HISTORICAL:** Retired operating model and preserved production behavior.
* **KNOWN DEFECT:** Only explicitly identified historical observations; detailed analysis is deferred.
* **UNKNOWN / NOT ESTABLISHED:** Direct production-runtime state where production was not inspected; exact historical traded universe; complete production configuration/database/crontab state.
* **INTERPRETATION:** Any explanation of why a particular implementation choice was made unless source evidence establishes it.
* **OPEN QUESTION:** Detailed formulas, S\&R, capital allocation, order processing, configuration, etc.; these belong to later agenda items.

## 14\. Final A-001 freeze

### FROZEN PURPOSE

> \*\*An automated, headless NSE equity trading system that consumed external Chartink signals and, subject to its configured controls and decision pipeline, processed, executed, managed, reconciled, and reported trades through Zerodha.\*\*

### FROZEN SCOPE

**NSE Equity + Zerodha + Chartink-originated signals + automated trading lifecycle + operational/reporting controls.**

### FROZEN NON-SCOPE

The system did not originate the external Chartink scanner signals.

### GOVERNANCE

No V2 architecture, redesign, fix, tuning or replacement behavior is included in A-001.

**A-001 is now FROZEN.**

## 15\. Cross-reference

Detailed implementation evidence is deferred to the corresponding later manuals: startup/runtime, market data, signals/strategies, filtering/decision, S\&R, entry/R:R, risk/capital, order processing, position management, exits, reconciliation, alerts, reports/evidence, database, configuration, security/deployment, testing, recovery and end-to-end walkthrough.

