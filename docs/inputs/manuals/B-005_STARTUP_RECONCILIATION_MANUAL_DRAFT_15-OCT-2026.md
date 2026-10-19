# B-005 — Startup Reconciliation

**Prepared:** 15-Oct-2026 (IST)  
**Status:** Draft for Claude review — not frozen / not approved  
**Evidence baseline:** Production snapshot `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
**Purpose:** Factual legacy documentation only. This chapter does not repair, revive or redesign the legacy system.

> Rama's completed answer sheet was prepared by Gemini and is treated as raw/probable input. Tests establish tested behavior, not historical production incidents. Module headers establish documented intent, not necessarily actual runtime behavior.

## 1. Executive summary

The source contains an in-process `OrderReconciler` that compares local open-trade state with broker state, performs reconciliation/recovery checks, records non-cosmetic actions and repeats checks on a daemon poll thread.

`main.py` explicitly calls `order_reconciler.reconcile_once()` before starting `signal_processor` and `entry_gate`. Later, `main.py` calls `order_reconciler.start()`, which invokes `reconcile_once()` again before starting the poll thread. The normal traced path therefore contains two reconciliation calls unless runtime conditions make one a no-op.

**Critical qualification:** the source does not establish a universal “any mismatch or API failure blocks startup” rule. `reconcile_once()` catches broad exceptions, logs and returns `[]`; `main.py` sets `_startup_reconcile_done = True` after the explicit call returns. `BrokerTimeoutError` is documented to skip the affected check and continue. Therefore “call returned” must not be treated as equivalent to “all required checks passed.”

## 2. Evidence baseline and source map

Production repository: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot  
Production ref: `970aabf3c78d777d9d34549e021f7c8e756c7bdb`

| File | Blob SHA at inspected ref | Role |
|---|---|---|
| `main.py` | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | Startup sequence and signal-pipeline gate |
| `orders/order_reconciler.py` | `beef3e1584c9ad665dec4d1e1498bde001326c70` | In-process reconcile/recovery loop |
| `scripts/reconcile_positions.py` | `a790550c9fb4d4e9f20563b69e978c7b0d9b8413` | Standalone EOD symbol-level comparison |
| `scripts/eod_broker_reconcile.py` | `266cac23afecf2c85516ac48212b9a0c7f849b2c` | Separate EOD broker-authoritative verdict |
| `core/state_store.py` | `11866117801b9329773a1be605448faef3c96f8d` | SQLite persistence |
| `config/cron_registry.yaml` | `42ff638145ba8c05eba907a33d5b391ae3fc6910` | Intended/generated cron registry |
| `tests/unit/test_order_reconciler.py` | `fc16c0cee145f57c407474af60fa3a7f71bb2556` | Reconciler unit tests |

Useful direct links:
- [main.py](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/main.py)
- [OrderReconciler](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/orders/order_reconciler.py)
- [Startup checks](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/utils/startup_checks.py)
- [Position reconciliation utility](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/scripts/reconcile_positions.py)
- [EOD broker reconciliation](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/scripts/eod_broker_reconcile.py)
- [StateStore](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/core/state_store.py)
- [Cron registry](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/config/cron_registry.yaml)

### Production/sandbox boundary

The checked sandbox files `orders/order_reconciler.py`, `scripts/reconcile_positions.py` and `scripts/eod_broker_reconcile.py` have the same blob SHAs as production. Sandbox `main.py` differs (`795e3c136b5db4252ec9d6fb320ddaffdf3492b0`) and `utils/startup_checks.py` also differs. These are file-specific results only, not proof of full repository or runtime parity. The records manifest describes multi-pass collection; it is not proof of final installed host state.

## 3. Component boundaries

| Component | Source-supported responsibility | Limitation |
|---|---|---|
| `main.py` | Calls pre-flight checks and reconciler; controls startup order | A call-order assertion does not prove a clean reconciliation result |
| `utils/startup_checks.py` | Pre-flight checks and scenario detection | Module contract says no DB mutation, process exit or subsystem/thread startup |
| `orders/order_reconciler.py` | In-process position/order recovery checks and periodic polling | Individual checks may be skipped; exceptions may be converted to `[]` |
| `broker/order_monitor.py` | Order-state/fill monitoring | Related component, not the same as position reconciliation |
| `scripts/reconcile_positions.py` | Daily EOD symbol-by-symbol comparison | Paper mode writes a row unless `dry_run=True` |
| `scripts/eod_broker_reconcile.py` | Separate EOD verdict process | Different status vocabulary and persistence path |
| `core/state_store.py` | SQLite state APIs | Cross-reference frozen B-003; do not invent table names |
| `config/cron_registry.yaml` | Source for generated cron | Does not prove installed host crontab or a historic run |

## 4. Startup call sequence

### 4.1 SOURCE → INPUT → LOGIC → GUARD → OUTPUT → HANDOFF

**SOURCE:** `main.py`; `orders/order_reconciler.py::OrderReconciler.start()` and `reconcile_once()`.

**INPUT:** StateStore, broker adapter, FundManager, kill switch, notifier, event bus, `OrderReconcilerConfig`, `quote_fn=broker_adapter.get_quote`, `broker_orders_fn=broker_adapter.get_open_orders`, plus optional CNC/GTT and market-hours dependencies.

**LOGIC / CALL ORDER:**
1. `main.py` constructs `OrderReconciler`.
2. Startup prepares feed/monitor state; the inspected path starts candle storage/connects the feed and rehydrates order monitor/fill map before the explicit reconciliation call.
3. `main.py` calls `recon_actions = order_reconciler.reconcile_once()` before signal-pipeline startup.
4. It sets `_startup_reconcile_done = True` after the method returns.
5. Later it starts `order_monitor`, then calls `order_reconciler.start()`.
6. `start()` performs a CNC overnight-position check, optionally attempts CNC GTT reconciliation (logs and continues on exception), calls `reconcile_once()` again, and starts the daemon poll thread.
7. `main.py` asserts `_startup_reconcile_done` before starting `signal_processor` and `entry_gate`.

**VALIDATION / GUARDS:**
- `reconcile_once()` acquires a non-blocking lock. If another cycle is running, it returns `[]`.
- Any unhandled exception caught inside `reconcile_once()` is logged and converted to `[]`.
- `BrokerTimeoutError` in a check logs a warning, skips that check and continues.
- `BrokerAuthError` is counted at cycle level; after three consecutive cycles containing auth errors, `kill_switch.soft_kill()` is called.

**OUTPUT:** `List[ReconciliationAction]` with `check_name`, `tier`, `symbol`, nullable `trade_id`, `description`, `action_taken`, `success`.

**STORAGE / HANDOFF:** Non-COSMETIC actions are sent to `StateStore.insert_reconciliation_log()`. Persistence failure is logged. The main call logs action count if non-empty and sets the startup flag after return.

**NEXT CONSUMER:** signal pipeline startup follows the explicit call and flag assertion; the daemon poll thread performs subsequent cycles at the configured interval.

### 4.2 Correct interpretation

**FACT — SOURCE-VERIFIED:** a synchronous call occurs before signal pipeline startup.

**FACT — SOURCE-VERIFIED:** the call returning is not proof that all checks completed cleanly. An empty list can mean no actions, lock skip or caught exception; timeout can skip a check. The flag is set after return.

**INTERPRETATION / AUDIT LEAD:** the startup flag may prove call completion rather than reconciliation success. Claude must trace every independent readiness/failure guard before stating the system was universally fail-closed.

**UNKNOWN / NOT ESTABLISHED:** whether any historic production boot reached READY after a partial/failed pass. This needs dated logs or preserved records.

## 5. Reconciliation cycle and comparison logic

### 5.1 Header check categories

`orders/order_reconciler.py` RC5 names:
- `MANUAL_CLOSE` — local OPEN/PARTIAL trade with no broker position;
- `ORPHAN_ADOPTION` — broker position with no local trade;
- `HEALTHY` — quantities match;
- `PARTIAL_CLOSE` — local quantity exceeds broker quantity;
- `POSITION_GREW` — broker quantity exceeds local quantity;
- `ORPHAN_ORDER` — local `PENDING_FILL` order missing from broker open orders, only when `broker_orders_fn` is supplied.

These are the header's six categories, not the entire implementation. The body also has GTT and in-flight-entry prepasses, crash-recovery SL, capital/margin drift and specialized oversell handling. Describe each with its actual function, conditions and action tier before freezing.

### 5.2 Position comparison

**SOURCE:** `orders/order_reconciler.py::_reconcile()` and its check helpers.  
**INPUT:** `adapter.get_positions()` and `StateStore.get_all_open_trades()`.

The visible code maps broker positions using `p.symbol` and looks up each local trade by `trade["symbol"]`. The visible quantity branch uses local `qty_filled` and absolute broker quantity. A broker-position timeout logs a warning and skips checks 1–5 for that cycle.

**Important:** the answer sheet's universal claim that the in-process matcher always uses symbol + exchange + product + signed quantity + account/order ID is not supported by this visible path. It is a high-priority audit lead. Whether other safeguards mitigate collisions in particular paths remains open.

### 5.3 Orphan/adoption and external positions

The source explicitly names `ORPHAN_ADOPTION` and includes adoption/recovery paths. Therefore, “manual/orphan positions are never adopted” is not safe as a universal statement. Exact eligibility, stored fields, protective-order consequences and alerts require method-level tracing.

The source also contains a specialized `SYSTEM_OVERSELL` branch that can place a MARKET cover order when an untracked broker position is identified as a duplicate exit leg for a previously closed trade. This is not the same as flattening every arbitrary external position.

**UNKNOWN:** the outcome for every possible manual trade, unrecognized tag, product, or carried position.

### 5.4 Missing stop-loss recovery

The module header documents G5b: for eligible OPEN/PARTIAL trades without active SL coverage, fetch LTP via `quote_fn` and place a fresh stop-limit SL or MARKET exit. This proves that both paths exist in the documented implementation; it does not prove a fresh SL is always placed or that market exit is never used.

The implementation includes a settling window intended to avoid a race with the normal entry-fill/SL/TGT path. Exact trigger validity, market-window guards, rejection handling and fallback behavior need a function-level review.

## 6. Failure and concurrency behavior

| Condition | Source-defined behavior | Do not infer |
|---|---|---|
| Lock already held | Log and return `[]` without waiting | `[]` does not mean clean |
| Unhandled exception in `reconcile_once()` | Log and return `[]` | Not automatically a fatal startup exception |
| `BrokerTimeoutError` | Skip affected check and continue | Not universal startup abort |
| `BrokerAuthError` | Soft-kill after three consecutive error cycles | Not immediate exit on first error |
| Action persistence failure | Log failure; does not propagate from this persistence block | Not proof every action is durable |
| Optional CNC/GTT startup reconcile exception | Log and continue | Not a universal fail-fast gate |

Main.py has its own exit-code taxonomy for different startup failures and missing/expired token paths. Do not transfer one exit code to every reconciler failure.

## 7. Persistence and standalone EOD utilities

### 7.1 In-process action log
Non-COSMETIC `ReconciliationAction` records are passed to `StateStore.insert_reconciliation_log()`. This does not prove every clean cycle, skipped check or full broker snapshot is stored. Persistence exceptions are caught and logged.

### 7.2 `scripts/reconcile_positions.py`
The header describes a daily EOD symbol-by-symbol comparison at 15:45 IST. Tests establish:
- paper mode skips broker fetch;
- paper mode writes `position_reconciliation` rows;
- `dry_run=True` suppresses those DB writes;
- live tests mock broker fetch and assert match, broker-only, system-only and quantity mismatch outcomes.

Therefore, this script is not always read-only. Its source schedule does not prove the host actually ran it.

### 7.3 `scripts/eod_broker_reconcile.py`
Separate verdict process. Tests use `VERIFIED`, `ISSUES`, `UNVERIFIED`, `NOT_CHECKED`. Missing required broker data yields `UNVERIFIED`; orphan/missing positions or required P&L mismatch yield `ISSUES`. Margin is supplemental/reliability-gated and can be `NOT_CHECKED` without preventing VERIFIED. Do not merge these statuses with in-process action names.

**UNKNOWN:** actual historic production execution, installed schedule and surviving runtime outputs.

## 8. Configuration, paper/live and scheduling

The reconciler receives typed `OrderReconcilerConfig`; its header explicitly names `poll_interval_sec` and `capital_drift_tolerance`. Extract all other fields/defaults from the config schema before freezing. Do not document guessed keys such as `ENABLE_STARTUP_RECONCILIATION`, `AUTO_RECOVER_STOP_LOSS`, `SHADOW_MODE`, `POSITION_DRIFT_TOLERANCE` or `RECONCILER_EXCHANGES` unless literal source references are found.

`config/cron_registry.yaml` declares itself the source for generated `deploy/cron/trading-system.cron`; `enabled:false` jobs are excluded from generation. This is repository-defined scheduling intent, not proof of installed crontab or historic execution.

`main.py` constructs `ZerodhaAdapter` with paper/live mode. In paper mode, `kite_client=None`, but a quote provider is injected and its source comment says it reads the token file for real Kite quotes. The reconciler says paper behavior is owned by the adapter/notifier, and some capital/margin checks run only when the adapter reports broker-equivalent margins. Do not claim all paper paths are local-only or that sandbox equals production.

## 9. Historical events, defects and audit leads

The answer sheet describes crash/SL recovery, expired-token startup failure, stale SQLite state and no false-PASS incidents. These remain **HISTORICAL / RECOLLECTION** without dated logs, DB exports, incident reports or other artifacts. Unit tests are simulated cases, not proof of historic production incidents.

Source comments contain FIX/audit labels, but a claim that a specific incident caused a fix requires commit/issue/PR/post-mortem evidence.

| Classification | Current treatment |
|---|---|
| FACT — SOURCE-VERIFIED | Startup call order; duplicate call through start(); lock behavior; exception-to-empty-list; timeout skip; three-cycle auth soft-kill; non-COSMETIC action persistence; symbol-keyed comparison path; distinct EOD scripts |
| FACT — TEST-VERIFIED | EOD verdict semantics and paper/dry-run persistence behavior in position utility |
| HISTORICAL / RECOLLECTION | Claimed incidents without dated artifacts |
| UNKNOWN / NOT ESTABLISHED | Actual host schedule/run, installed code/config, false-PASS history, full sandbox parity |
| INTERPRETATION / AUDIT LEAD | Startup flag semantics; symbol-only matching implications; completeness of reconciliation log |
| KNOWN DEFECT | No new defect declared without proven scope/impact/version |
| PROPOSED / OPEN V2 INPUT | Gemini's V2 suggestions remain proposals, not decisions or legacy facts |

## 10. Open questions before freeze

1. Why does main.py explicitly call `reconcile_once()` and then call `start()`, which calls it again?
2. Can an empty list caused by a caught exception or lock skip still allow startup to proceed? What independent guard prevents it?
3. What conditions trigger unrecoverable action, soft-kill, process stop or warning only?
4. Trace exact branches for MANUAL_CLOSE, ORPHAN_ADOPTION, PARTIAL_CLOSE, POSITION_GREW and ORPHAN_ORDER.
5. What qualifies a broker position for adoption?
6. What are the exact G5b LTP/trigger guards, stop-limit placement and MARKET exit fallback?
7. Are there product/exchange safeguards that mitigate the symbol-keyed position map?
8. What are all actual OrderReconcilerConfig fields/defaults?
9. What exactly is persisted in `reconciliation_log`; what can be omitted?
10. Which cron registry entries are enabled, and is there dated host evidence of installation/execution?
11. Which claimed incidents have dated artifacts versus recollection only?
12. Does the final chapter need a broader sandbox call-graph/config comparison?

## 11. Cross-references

- B-001 — VM/service startup (frozen; do not silently alter).
- B-002 — Configuration Loading.
- B-003 — Database/State Initialization (frozen revision 2 under DR-039; use for schema/lifecycle cross-check).
- B-004 — Broker Connection (draft/not frozen at time of this review).
- [A-006 — Broker Model](https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/agenda/a-006_broker-model/A-006_manual.md)
- [A-010 — Secrets/Environment](https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/agenda/a-010_secrets-environment/A-010_manual.md)
- [V2 Design Log](https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/governance/V2_DESIGN_LOG.md)
- [V2 Decision Register](https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/governance/V2_DECISION_REGISTER.md)

## 12. Freeze criteria

Before freeze, Claude and Rama should verify the duplicate call; `[]`/exception/timeout semantics; actual readiness gate; symbol-keyed comparison and safeguards; adoption, manual-close, oversell and G5b paths; exact config fields; EOD cron evidence; and incident evidence classifications. Do not change frozen chapters, make V2 decisions, operate/restart the legacy system, or commit/push from this draft.
