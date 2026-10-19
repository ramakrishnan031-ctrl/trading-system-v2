# B-006 — Worker Initialization
## Legacy System Technical Manual — Cautious Draft

**Prepared:** 16-Oct-2026 (IST)  
**Status:** DRAFT — not frozen; source/call-site audit incomplete  
**System status:** Legacy is DEAD / REFERENCE ONLY  
**Input:** Rama's B-006 answer sheet, treated throughout as RAW / PROBABLE / EXPECTED pending independent verification.

> This chapter records the legacy system only. It is not an operational runbook and must not be used to start, repair, or revive the system. B-005 remains parked until Sunday morning IST. No V2 architecture or behavior is decided here.

## 1. Purpose and boundaries

B-006 documents how legacy runtime workers/components were constructed, started, owned, connected, and stopped—only to the extent source and records establish those facts.

| Agenda topic | Boundary |
|---|---|
| B-005 Startup Reconciliation | Startup comparison/recovery of local state against broker state. Parked; not covered or resolved by this chapter. |
| B-006 Worker Initialization | Worker objects, thread/task/process creation, startup calls, connections/subscriptions, and lifecycle. |
| B-007 Readiness / Health | Whether the system is considered operationally ready, including any data-flow or health gates. Do not infer readiness from a worker merely being started. |

The B-003/B-004 manuals are governance/context cross-references, not proof that legacy code implements every described phase.

## 2. Evidence and confidence scheme

- **FACT — SOURCE-VERIFIED (module-level):** directly visible in the retrieved source blob; does not automatically establish caller/runtime behavior.
- **FACT — SOURCE-VERIFIED (call-site):** requires a verified constructor/start call and exact branch/ordering in the reviewed commit. Not yet fully established in this draft.
- **FACT — RECORD-DERIVED:** supported by dated runtime/config/deployment records, identifying date and environment.
- **HISTORICAL — RAMA:** personal recollection from the submitted answer sheet.
- **INTERPRETATION / AUDIT LEAD:** plausible explanation or direction for further source tracing, not fact.
- **UNKNOWN / NOT ESTABLISHED:** evidence reviewed does not establish the claim.
- **KNOWN DEFECT:** only where a specific defect is demonstrated and sourced; none is assigned solely from the answer sheet.

Rama's answers, including “source-verified” labels, are considered unverified input. Claims need their own source/record support.

## 3. Source inventory reviewed

| Source | What the inspected material establishes | Limit |
|---|---|---|
| Production `main.py` | File describes itself as the single orchestration entry point; imports threading, LiveFeedManager, OrderReconciler, OrderMonitor, StateStore and other subsystems. | Import is not construction/start. Full main() order, conditions, exception paths and shutdown not yet line-audited. |
| Production `data/live_feed.py` | Module/class describes KiteTicker WebSocket management, bounded tick queue and one consumer thread; constructor contains queue/callback/thread/ticker/stop/reconnect/watchdog fields. | Does not establish production invocation, actual subscriptions, connection success, callback consumers, or paper deployment. |
| Production `orders/order_reconciler.py` | Module header describes start/stop/reconcile_once, daemon polling and startup reconciliation; configured cadence is described in its design header. | Effective config and caller/start behavior must be checked in implementation and main.py. |
| Production `broker/order_monitor.py` | Module header describes a single daemon polling thread, order tracking and state transitions; says it does not persist state across restarts. | Actual start call, poll settings, shutdown and runtime evidence remain to be traced. |
| Production `utils/startup_checks.py` | Startup checks are described as library functions; module says it does not itself construct/start/stop subsystems or control process. | Does not establish main's ordering or whether checks block worker startup. |
| Sandbox `docs/SYSTEM_MAP.md` and `scripts/reconcile_positions.py` | Provide sandbox operational/documentary context; reconcile script header describes an EOD standalone utility. | Sandbox is separate evidence. Not proof of production behavior, installed cron, or actual execution. |
| Records repo `SNAPSHOT_MANIFEST.md` | Known manifest describes a multi-pass 17–18 Sep 2026 records snapshot with possible stale/unrefreshed copies. | Not proof of final host state. Specific worker incidents and installed scheduler/service evidence were not established in this review. |
| V2 B-001/B-003/B-004 and governance docs | Clarify agenda boundaries and previous documentation context. | Not a substitute for legacy source/runtime proof. |

Source links and retrieved blob SHAs are listed in the companion `B-006_reconciliation.txt`. The final frozen chapter must add exact commit SHA and line spans for each implementation claim.

## 4. Candidate worker/component inventory

This is a **candidate inventory**, not a verified complete list of active production workers.

| Component | Source-supported observation | Status / next proof needed |
|---|---|---|
| `data.live_feed.LiveFeedManager` | Source module exists; documents KiteTicker management, queue/callback distribution and consumer-thread structure. | Module-level supported. Trace main.py constructor/start, ticker connect, subscriptions, callback registration and stop. |
| `orders.order_reconciler.OrderReconciler` | Source module exists; design header documents periodic reconciliation thread and start/stop interface. | Module-level supported. Verify effective poll config, startup pass, main caller and exception actions. |
| `broker.order_monitor.OrderMonitor` | Source module exists; design header documents polling thread/order-state tracking. | Module-level supported. Verify main caller, tracked-order population, poll interval and stop. |
| `core.state_store.StateStore` | Imported by main.py and described as shared state/persistence infrastructure. | It is infrastructure, not necessarily a “worker.” Verify construction, DB lifecycle, sharing/locking and consumers. |
| Candle builder/resampler, strategy evaluator, alert dispatcher | Rama recalls these as workers/handlers. | HISTORICAL — RAMA / UNKNOWN topology. Identify actual modules, callback paths, and whether separate threads/tasks or synchronous callbacks. |
| Token monitor, health server, webhook receiver, watchdog, instrument cache/download, other managers | Candidate background or startup paths named in answer sheet/source imports. | Trace each exact constructor/start/schedule and classify as thread, server, synchronous function, or external job. |
| `scripts/eod_broker_reconcile.py`, `scripts/reconcile_positions.py` | Standalone script candidates. Sandbox reconcile script header describes EOD use. | Do not claim production cron/systemd execution absent installed schedule and dated execution evidence. |

## 5. Worker initialization lifecycle — what can currently be said

### 5.1 Construction
**Source → Input → Logic → Guard → Output → Handoff**

- **Source:** production `main.py` imports worker classes; worker module definitions exist.
- **Input:** specific constructor dependencies/configuration are worker-specific. LiveFeedManager constructor includes API key/token parameters, logger, mode and reconnect/watchdog/subscription options; never document credential values.
- **Logic:** object construction initializes fields, synchronization objects and internal references as shown in the class implementation.
- **Guard:** exact constructor validation and startup gating are not yet traced.
- **Output:** instantiated objects and their internal state/handles.
- **Storage/handoff:** main/coordinator or subsystem references; exact owner/retained handle must be established from assignments.
- **Next consumer:** worker start/connect/subscription calls and registered callbacks, once call sites are traced.

**Status:** imports and constructor shape are module-level source facts; production instantiation/ownership/order remain UNKNOWN pending main() trace.

### 5.2 Thread/task/process creation
The LiveFeedManager, OrderReconciler and OrderMonitor module headers describe thread-based background behavior (with queue consumer or polling thread as applicable). That supports their documented design, but the actual production lifecycle requires reading each `.start()` implementation and its caller.

**Not yet established:** that every candidate worker runs in one Python OS process; that there is no multiprocessing/IPC; that all threads are daemon threads; that all imported workers are started; or that the service manager runs the same repository version.

### 5.3 Market-data connection and event flow
The LiveFeedManager module describes a KiteTicker-based feed and a bounded queue with a single consumer thread. Constructor fields include tracked subscriptions, callback references, a queue (capacity constant shown as 10,000), stop event, consumer-thread reference, ticker reference, reconnect settings and watchdog state.

A cautious conceptual path, pending exact implementation trace:

`KiteTicker callback → LiveFeedManager tick handling → bounded queue / consumer → registered callback(s) → downstream consumers`

The exact ordering, filtering, queue-full behavior, callback error handling, reconnect sequence, and consumer list must be sourced from the method bodies and caller registrations. Do not state candle/strategy processing occurs on a distinct worker unless code demonstrates it.

**Connected / subscribed / receiving:** treat as distinct potential conditions. No verified system-level readiness predicate is established here. A transport callback or subscription call alone does not prove ticks are flowing or that B-007 readiness passed.

### 5.4 Reconciliation and order monitoring
OrderReconciler source header describes start/stop and a daemon poll loop, with startup reconciliation triggered by `start()` and a configured interval. OrderMonitor header describes a daemon polling thread tracking broker orders and driving order-state transitions.

The manual must distinguish:
- a module's documented contract,
- the effective config value,
- the actual call to `start()`,
- successful operation observed in records.

Do not conflate the B-005 startup reconciliation decision/gate with B-006 starting the reconciler worker or B-007 readiness.

### 5.5 External scheduled utilities
Some answer-sheet claims name standalone scripts and cron/systemd. The sandbox `reconcile_positions.py` header describes an EOD script and intended schedule. This is **sandbox-only documentary evidence** until production source identity and host schedule/execution records are verified.

For each external job, final documentation must show:
`script → scheduler/CLI invocation → environment/config → inputs → operation → output/log/DB → next consumer`
and separately label “declared schedule” versus “installed schedule” versus “observed run.”

## 6. Startup order, conditionality, and readiness

**Current status: UNKNOWN / NOT ESTABLISHED at end-to-end level.**

The answer sheet asserts a strict sequence (database/state initialization, startup reconciliation, then worker starts), mode/market-hours conditions, fail-fast behavior and readiness barriers. These are hypotheses to test against the complete production main.py and exact worker implementations—not facts to repeat.

Required trace table for final revision:

| Order | Source line(s) | Component/action | Condition | Blocks/waits? | Failure path | Evidence class |
|---|---|---|---|---|---|---|
| TBD | TBD | Load config / establish dependencies | TBD | TBD | TBD | TBD |
| TBD | TBD | Initialize StateStore | TBD | TBD | TBD | TBD |
| TBD | TBD | B-005 startup reconciliation interaction | TBD | TBD | TBD | TBD |
| TBD | TBD | Construct/start OrderReconciler / OrderMonitor | TBD | TBD | TBD | TBD |
| TBD | TBD | Construct/start LiveFeedManager / subscribe | TBD | TBD | TBD | TBD |
| TBD | TBD | Readiness gate (B-007) | TBD | TBD | TBD | TBD |

Do not populate the order from recollection or from imports. Check actual call sequence and control flow.

## 7. Failure, retries, partial startup, shutdown, and restart

The submitted answer sheet makes broad claims about fail-fast startup, retry ownership, cleanup, exception propagation, trading halt/entry blocking, no automatic flatten, graceful stop, daemon termination and restart reconstruction. These remain **unverified** until traced.

Final review must separately document:
1. Construction exception.
2. Thread/task launch exception.
3. Initial connection/authentication/subscription failure.
4. Runtime disconnect/stale feed/queue overflow.
5. Polling-worker exception and broker timeout/authentication error.
6. Critical callback / kill-switch / entry-gate action.
7. Normal SIGINT/SIGTERM shutdown.
8. Abrupt process crash and supervisor restart.
9. Duplicate `.start()` / duplicate subscription / overlapping external job.
10. State persistence and in-flight order/event handling.

For each scenario, state the actual mechanism, exact branch/action, log/event/DB output and consumer. “Fail-safe,” “fail-fast,” “all workers stop,” “positions flatten,” or “existing positions remain managed” must not be asserted without direct evidence.

## 8. Paper versus live

**Parity is NOT ESTABLISHED.**

A `paper_mode` parameter in LiveFeedManager is evidence that mode is represented in its interface, not that paper mode used a real broker WebSocket. To establish behavior, inspect:
- how main.py constructs the feed in paper and live branches,
- adapter/feed provider selection,
- order execution and position-state implementation,
- test harnesses and mocks,
- dated evidence showing actual paper run behavior.

Record separately: market-data source, broker API access, order simulation, persistence target, alerts, and worker activation. No parity inference from shared module names.

## 9. Observability and incidents

The answer sheet reports several possible failures/incidents, but the submission does not tie them to dated logs or identifiable records. They remain **HISTORICAL — RAMA / NOT CORROBORATED**.

Before including an incident as record-derived, capture:
- date/time and timezone,
- production/testing and paper/live,
- specific symptom,
- artifact path and relevant lines,
- whether it proves occurrence or only a possible cause.

Do not fabricate sample lifecycle log messages. Quote actual source strings only when line-cited.

## 10. Known gaps / open questions

1. Complete production main.py worker startup/shutdown trace and exact line references.
2. Actual production instantiation/start of LiveFeedManager, OrderReconciler and OrderMonitor.
3. Worker ownership/handle retention and any intermediate coordinator.
4. Complete callback consumer chain, candle/strategy topology, and queue overflow semantics.
5. Effective config for polling/reconnect/watchdog and mode/market-hour conditions.
6. B-005 call/gate versus B-006 worker-start boundary versus B-007 readiness predicate.
7. Exact retry/reconnect and critical-failure handling.
8. Stop/join/disconnect and exception/finally cleanup.
9. Installed production systemd/crontab/timer and dated run evidence.
10. Paper/live implementation and test parity.
11. Dated incident records and log destinations.
12. Production/sandbox blob equality for every source relied upon.

## 11. Legacy lessons / V2 inputs

This section is **not a V2 design**. No retain/reject/redesign decision is made here. Potential future inputs may be listed only as **PROPOSED / OPEN**, and only after the legacy facts are verified. Keep them out of frozen legacy conclusions.

## 12. Final evidence disposition

At this stage, source inspection supports that relevant worker modules exist and that their module documentation/constructor fields describe certain threading, queue, and polling mechanisms. It does **not** yet establish the full live production startup order, actual host execution, all worker topology, paper/live behavior, failure policy, incident history, or installed scheduler/service state.

**Recommended status:** continue source/call-site/records verification; do not freeze B-006 yet. Rama's answer sheet remains a useful raw research map, not a validated technical account.

## Source links

- Production `main.py`: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/main/main.py
- Production LiveFeedManager: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/main/data/live_feed.py
- Production OrderReconciler: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/main/orders/order_reconciler.py
- Production OrderMonitor: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/main/broker/order_monitor.py
- Production startup checks: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/main/utils/startup_checks.py
- Sandbox SYSTEM_MAP: https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot/blob/main/docs/SYSTEM_MAP.md
- Sandbox reconcile_positions: https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot/blob/main/scripts/reconcile_positions.py
- Records snapshot manifest: https://github.com/ramakrishnan031-ctrl/trading-system-records/blob/main/SNAPSHOT_MANIFEST.md
- V2 governance/context repository: https://github.com/ramakrishnan031-ctrl/trading-system-v2
