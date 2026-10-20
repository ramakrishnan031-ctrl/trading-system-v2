# B-007 — Readiness / Health Gating (Legacy System)
**Draft manual — independent source reconciliation, 16-Oct-2026**  
**Status: NOT FROZEN. Legacy is DEAD / REFERENCE ONLY.**

> Evidence discipline: this chapter distinguishes source-verified repository behavior from host/runtime facts and historical recollection. Repository declarations do not prove deployed or executed behavior. The exact repository ref/commit corresponding to the intended legacy baseline must be recorded before final acceptance. No V2 design is made here.

## 1. Scope and boundary

B-007 documents how the legacy application represented or checked startup readiness, operational health, feed freshness, worker liveness, and status visibility—only to the extent established by source and records.

- **B-005 Startup Reconciliation:** parked; its sequence and semantics are not settled here.
- **B-006 Worker Initialization:** separate draft; do not assume its sequence is verified.
- **B-008 Market-Open / Market-Hours Gating:** separate agenda; this chapter does not settle its boundary.
- Production, sandbox, paper, live, and installed-host behavior are distinct evidence domains.

## 2. Evidence inventory and provenance

| Evidence | What it can establish | Limitation |
|---|---|---|
| Production snapshot files fetched from GitHub | Declared code paths in the retrieved repository contents | Exact commit identity and installed host state not established in this pass |
| V2 frozen B-001/B-003/B-004 manuals | Governance scope and already-frozen adjacent agenda findings | Do not use them as proof of unreviewed B-007 details or infer B-005/B-006 sequence |
| Rama’s returned B-007 answer sheet | Historical recollection, observations claimed, and source leads | Sheet explicitly says answers are raw/probable and Gemini-prepared; corroboration required |
| VM service/cron captures, logs, tests, screenshots | Could establish dated installation/execution/test observations | No such supporting artifacts were supplied with the answer sheet |

Source files checked in the production snapshot (blob SHA noted):
- `main.py` — `445fb0d8237a6d535ed00e5eb968c8176efd74f3`
- `scripts/healthcheck_server.py` — `c094b5eb402353be5c74b569620aae0943cfad8b`
- `utils/startup_checks.py` — `61ff67676d8230640a2aafb6bd6d75d7a185fbb6`
- `data/live_feed.py` — `10ebd00ee728c449c6df4dcc5e576a92f3361e10`
- `orders/order_reconciler.py` — `beef3e1584c9ad665dec4d1e1498bde001326c70`
- `core/state_store.py` — `11866117801b9329773a1be605448faef3c96f8d`

These are file/blob identities, not a complete repository commit attestation.

## 3. Overall readiness model

### 3.1 What is established

**FACT — SOURCE-VERIFIED (bounded):** the fetched source contains multiple separate mechanisms:
1. A startup-check aggregator returning a structured `StartupReport`.
2. A `/health` HTTP route aggregating selected checks.
3. Component-local live-feed tick-age watchdog and reconnect handling.
4. Daemon thread liveness functions consumed by the health endpoint.
5. A kill-switch component and health/reconnect-related call paths.
6. Order reconciliation and durable reconciliation/cron-heartbeat records.

**FACT — SOURCE-VERIFIED:** `utils/startup_checks.py::run_all_startup_checks` collects results rather than exiting at the first failure. It identifies blocking failures and warnings; its caller decides what action follows. The documented checks include configuration/secrets/scenario/clock, config hash, holiday, scanner connectivity, optional webhook reachability, instrument-cache size, strategy config, SDK version, NTP, temporary config, disk space, and DB permissions. Some checks are warning-only or conditional/optional.

**UNKNOWN / NOT ESTABLISHED:** whether the system had a single canonical application READY/DEGRADED state, whether `/health` was polled by an external monitor, and whether health status directly gated new order submission. An aggregate HTTP verdict is not, by itself, proof of a trading-permission state machine.

### 3.2 Startup check flow (bounded source description)

`run_all_startup_checks(...)` → runs ordered checks → accumulates `blocking_failures` and `warnings` → returns `StartupReport(ok, scenario, ...)` → **caller in `main.py`** decides whether to proceed/abort.

- **INPUT:** state store, kill switch, time authority, broker adapter, market windows, app config, scanner/webhook functions, required-secret names, optional instrument cache and disk/DB paths.
- **LOGIC:** collect results; some failures append blocking names; some append warnings; optional checks can be skipped.
- **OUTPUT:** structured report, not a direct process exit.
- **HANDOFF:** main startup caller.
- **UNKNOWN:** exact caller branch/exit mapping and complete startup ordering require a pinned-ref line-by-line trace before this chapter can claim the full bootstrap sequence.

## 4. HTTP health endpoint

### 4.1 Route and response

**FACT — SOURCE-VERIFIED:** `scripts/healthcheck_server.py` defines Flask `GET /health`. The route checks a DB query (including trades-today count), token validity, and kill-switch status. If providers are wired, it also incorporates TGT-retry health and daemon liveness. It emits JSON containing `status` (`healthy` or `degraded`), checks, uptime, trades-today, and timestamp; returns HTTP 200 when all included checks are `ok`, otherwise 503.

**FACT — SOURCE-VERIFIED:** `main.py` imports `start_healthcheck_server` and wires it into runtime startup on port 8080 in the fetched source. The supplied daemon-liveness provider reports:
- `order_monitor` thread alive
- `order_reconciler` thread alive
- `eod_scheduler` thread alive
- `live_feed` connection state as informational/non-gating (`ok: True`, with `connected` field)

**SOURCE → INPUT → LOGIC → VALIDATION → OUTPUT → HANDOFF**
- **Source:** `scripts/healthcheck_server.py::health`; startup wiring in `main.py`.
- **Input:** state-store query; token check; kill-switch check; optional TGT-retry and daemon snapshots.
- **Logic:** aggregate each check’s `ok`; `all(...)` determines endpoint verdict.
- **Validation/guard:** failed check makes overall result degraded/503.
- **Output:** JSON health document and HTTP status.
- **Storage/handoff:** endpoint response; caller/monitor may consume it, but actual external polling is NOT ESTABLISHED.
- **Next consumer:** HTTP client/monitor if configured; internal startup also has a distinct webhook self-check. Do not conflate `/health` with the webhook receiver’s `/health` probe without verifying the route target and wiring.

**UNKNOWN / NOT ESTABLISHED:** whether the health server successfully started in the deployed environment, which host/interface was actually bound after configuration, whether an external poller existed, whether 503 led to service restart, and whether this endpoint blocked trading.

## 5. Feed connection and freshness

### 5.1 Connection state vs tick freshness

**FACT — SOURCE-VERIFIED:** `data/live_feed.py` has connection state and a tick-age watchdog. The watchdog is described as live-mode only; it checks tick age against a configurable threshold (constructor default shown as 30 seconds) and, under its arming/window conditions, can alert and force-close the ticker to trigger reconnect.

**Important qualification:** the watchdog is not equivalent to a global READY gate:
- It is armed only after a tick has been seen; no first-tick freshness verdict is established by this watchdog.
- It checks active-market/entry-window conditions when market windows are wired.
- The checked excerpt does not establish per-symbol freshness or full subscribed-symbol coverage.
- Its documented action is alert/reconnect; downstream trading block, kill-switch consequence, and recovery timing require tracing the callback and consumers.

**SOURCE → INPUT → LOGIC → VALIDATION → OUTPUT → HANDOFF**
- **Source:** `data/live_feed.py` watchdog loop and tick callback.
- **Input:** last-tick timestamp/age, connection state, paper/live mode, optional market-window predicate.
- **Logic:** periodic age comparison once armed.
- **Guard:** mode, connection, window, prior tick, and one-alert-per-stale-episode conditions.
- **Output:** critical log/alert path and forced reconnect request.
- **Handoff:** KiteTicker reconnect/callback path and other components consuming feed events.
- **UNKNOWN:** exact effective threshold after configuration; source timestamp semantics; per-symbol gap detection; whether every consumer suppresses signals on stale feed; reconnect exhaustion effects beyond source call sites.

## 6. Worker liveness and reconciliation

**FACT — SOURCE-VERIFIED:** `orders/order_reconciler.py` has a startup reconciliation call and launches a daemon polling thread. Its poll interval is configured (`poll_interval_sec`); this excerpt does not establish a fixed 30–60 second cadence. `is_alive()` exposes whether its poll thread is alive.

**FACT — SOURCE-VERIFIED:** `main.py` includes liveness for selected daemon workers in its health provider; the health route can therefore report degraded when those configured checks fail.

**UNKNOWN / NOT ESTABLISHED:** whether all workers are monitored; whether thread liveness detects stalled-but-alive work; the exact poll interval and exception policy in effective configuration; whether any health failure disables entries or changes existing-order/position management.

**B-005 boundary:** the source includes synchronous reconciliation calls in startup paths, but this chapter does not certify their complete ordering relative to B-006 worker startup. That belongs to B-005/B-006 reconciliation and must be checked against the exact baseline and frozen/draft manuals.

## 7. Kill switch, alerts, and failure effects

**FACT — SOURCE-VERIFIED:** `main.py` constructs/uses a kill-switch component, and `data/live_feed.py` includes a reconnect-exhaustion path with a kill-switch call. The health route also checks kill-switch status.

**FACT — SOURCE-VERIFIED:** source includes alert integrations (including Telegram and critical-alert sentinel paths). Some sends are best-effort and exception-handled.

**UNKNOWN / NOT ESTABLISHED:** exact kill-switch transition semantics; whether each trigger blocks entries, permits exits, cancels orders, liquidates positions, or terminates the process; whether alert deliveries succeeded; whether host-level alert watchers/pollers were installed.

Do not preserve the answer-sheet categorical claims “no kill switch,” “no external alerts,” or “health failures only passively stall entries” as facts. Equally, do not claim that an observed source call proves the resulting risk effect without tracing the recipient and execution path.

## 8. Durable state and observability

**FACT — SOURCE-VERIFIED:** `core/state_store.py` contains reconciliation persistence helpers and cron-heartbeat insert/read helpers. This establishes durable records for certain reconciliation actions and scheduled-job heartbeats.

**UNKNOWN / NOT ESTABLISHED:** whether any such heartbeat is an application-readiness heartbeat; exact table schema/callers/retention; whether a global health row exists elsewhere; production log rotation, journald capture, dashboards, and external alerts.

## 9. Production / sandbox / paper / live

The returned answer sheet describes substantial divergence, but the supplied materials do not include a pinned sandbox comparison or artifacts demonstrating observed behavior.

- **UNKNOWN / NOT ESTABLISHED:** exact sandbox mock behavior, parity of watchdog/health checks, live-data paper mode, simulated fills, and whether health checks vary by mode.
- **UNKNOWN / NOT ESTABLISHED:** host-installed service restart policy, cron execution, endpoint polling, or runtime outcomes.
- Do not infer parity from common filenames or repository labels.

## 10. Historical reports (not source-confirmed)

Rama’s sheet reports token-auth startup aborts, intraday WebSocket disconnect/reconnect, existing positions remaining protected by broker-side stops, log-centric operator checks, and delayed reconciliation detection. Preserve these as **HISTORICAL (Rama; uncorroborated in supplied evidence)** until dated logs, service journal, broker records, screenshots, or tests are provided.

## 11. Open evidence items before acceptance

1. Pin the exact production baseline commit and verify these files against it; include repo/ref/commit/date.
2. Trace startup report consumption in `main.py`, including blocking vs warning handling and exit codes.
3. Trace health server bind/start/failure and all consumers; distinguish health route from webhook receiver route.
4. Trace watchdog wiring, effective threshold, timestamp source, market-window conditions, per-symbol coverage, alert and reconnect effects.
5. Trace kill-switch methods and callers, including new-entry, exit/order management, persistence and reset effects.
6. Verify reconciler/order-monitor effective cadences, exception behavior, and liveness checks.
7. Compare sandbox at a pinned ref for the same components.
8. Obtain dated host/runtime records for systemd, cron, health polling, alerts, incidents, and tests.
9. Reconcile B-005 and B-006 separately; do not freeze B-007 until relevant findings and cross-references are accepted.

## 12. Status

**B-007 MANUAL DRAFT — NOT FROZEN.** This draft corrects several answer-sheet assertions that conflict with fetched repository code, but deliberately leaves runtime, host, parity, and order/position consequences open. No V2 design decision is proposed.
