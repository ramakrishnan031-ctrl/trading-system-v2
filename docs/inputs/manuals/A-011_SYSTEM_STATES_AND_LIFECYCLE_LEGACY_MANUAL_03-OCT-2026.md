# A-011 --- SYSTEM STATES AND LIFECYCLE

## Legacy Technical Manual --- Frozen Documentation Baseline

**Date:** 03-Oct-2026\
**System:** DEAD / REFERENCE ONLY\
**Status:** CLOSED / FROZEN\
**Primary:** Production GitHub\
**Comparison:** Sandbox/Testing\
**Historical:** Records + operator checklist

## 1. Scope

This chapter documents the legacy lifecycle as evidenced. It does not
repair, redesign or revive the system.

Evidence classes: **FACT**, **HISTORICAL**, **UNKNOWN / NOT
ESTABLISHED**, **KNOWN DEFECT**, **INTERPRETATION**, **OPEN QUESTION**.

## 2. Architecture

``` text
SYSTEMD / OPERATOR
      |
      v
HOLIDAY/WEEKEND GUARD
      |
      v
SERVICE START GUARD
      |
      v
CONFIG + DB + ENV
      |
      v
KILL STATE + STARTUP SCENARIO
(COLD/WARM/CRASH/HALT)
      |
      v
STARTUP CHECKS
      |
      v
BROKER/TOKEN/INSTRUMENT
      |
      v
STARTUP RECONCILIATION
      |
      v
WEBHOOK + SIGNAL PROCESSOR
      |
      v
ENTRY / POSITION MANAGEMENT
      |
      +--> SOFT_KILL: entries blocked, management continues
      |
      +--> HARD_KILL: all orders blocked, cancel/flatten path
      |
      v
EOD / MIS SQUARE-OFF
      |
      v
RECONCILIATION / REPORTING
      |
      v
POSITION-SAFE SERVICE EXIT
      |
      v
CLEAN SHUTDOWN
```

Independent cron:

``` text
CRON REGISTRY
  + backups
  + reconciliation
  + EOD verification
  + reports
  + metrics
  + control/monitoring
  + evidence/retention
```

**FACT:** trading-service lifecycle and cron lifecycle are separate.

## 3. Operator-visible lifecycle

The operator remembered only `STARTING` and `STOPPING`, with no named
normal RUNNING/READY state. This is **HISTORICAL USER RECOLLECTION**,
not the complete internal state model.

## 4. Internal startup state model

Production `utils/startup_checks.py` defines:

  State   Historical meaning
  ------- --------------------------------------------------
  COLD    first/new trading-day startup
  WARM    clean same-day restart evidence
  CRASH   same-day session without clean shutdown evidence
  HALT    active kill condition

HALT is blocking unless `--resume` is supplied.

## 5. Kill-switch model

### INACTIVE

Normal state.

### SOFT_KILL

-   persisted state;
-   blocks new entry orders;
-   allows exits and monitoring;
-   does not itself stop the process.

### HARD_KILL

-   persisted state;
-   blocks all orders;
-   cancellation/flatten path;
-   current Production adapter path uses asynchronous flattening;
-   shutdown drains the flatten worker before DB close.

### RESUME

`KillSwitch.resume()` is explicit operator action and records the
operator identity/reason.

## 6. Startup / boot

Production `main.py` stages:

1.  parse CLI;
2.  trading-day guard;
3.  service START-window guard;
4.  logging/config/environment;
5.  state/DB initialization;
6.  KillSwitch state handling;
7.  COLD/WARM/CRASH/HALT detection;
8.  startup checks;
9.  broker/token/instrument setup;
10. startup reconciliation;
11. component startup;
12. webhook/health startup;
13. startup event/session write;
14. runtime wait.

`deploy/systemd/trading-system.service` runs Live mode headlessly, uses
SIGINT on stop, and restarts only on failure exits. Direct Production
`python main.py` use is UNKNOWN.

## 7. Startup guards and service window

Current Production source/config establishes:

-   service may START from 08:00 until before 18:15 IST;
-   configured service stop is 17:35 IST;
-   EOD self-exit is position-aware and does not exit while a required
    active position or HARD_KILL flatten remains.

This resolves the important distinction between start cutoff and stop
time.

## 8. Startup reconciliation

The reconciler performs an initial reconciliation. Production then
asserts that this startup reconciliation completed before
`SignalProcessor.start()`.

Therefore:

`BOOT → RECONCILE → SIGNAL PROCESSING`

is a documented lifecycle dependency.

## 9. Signal lifecycle

`signals/webhook_receiver.py`:

`HTTP → auth → validation → KillSwitch → capacity → expiry → DB signal row → queue`

KillSwitch-active requests are rejected.

`signals/signal_processor.py` uses a dispatcher + worker pool and has a
clean stop/drain path.

**UNKNOWN:** queue survival after abrupt crash. Do not record "all
queued signals were dropped" as fact.

## 10. Broker authentication lifecycle

``` text
BrokerAuthError
   -> mark failure
   -> invalidate/rename token
   -> prevent same-token restart loop
   -> later headless start fails until refresh
```

`BrokerAuthError` is deliberately not counted as the transient
connectivity breaker.

User reports no actual failed headless-auth trading day: **HISTORICAL**.

Exact open-position behavior during auth outage: **UNKNOWN**.

## 11. Persistent DB state

SQLite stores session/system events, signals, orders, trades, capital
ledger, kill-switch and reconciliation state.

Clean shutdown writes a `SHUTDOWN` event. Startup uses this evidence to
classify WARM/CRASH. Relevant order components rehydrate persistent
state before live processing.

## 12. Order / position state

Persisted statuses include:

`PENDING_FILL → OPEN/PARTIAL → EXITING → CLOSED/CLOSED_MANUAL`

with terminal order statuses including `CANCELLED`, `FAILED`, `EXPIRED`,
`COMPLETE`.

HARD_KILL treats `OPEN`, `PARTIAL`, `PENDING_FILL` as live exposure.

This is a set of related state machines, not one universal state enum.

## 13. Intraday lifecycle

``` text
Signal → qualification → entry → position → SL/Target → same-day MIS/CO square-off
```

A-011 records lifecycle only; strategy, sizing, risk and price semantics
are covered in other manuals.

## 14. Delivery lifecycle

``` text
Signal → CNC position → broker-side GTT → overnight holding → GTT/manual exit → reconciliation
```

Delivery/CNC is explicitly excluded from normal EOD square-off.

The application therefore does not equate market close with "all
positions flat".

## 15. EOD / MIS square-off

`orders/eod_squareoff.py` sequence:

1.  SOFT_KILL;
2.  cancel pending intraday entries;
3.  cancel pending intraday SL/TGT legs;
4.  exit intraday positions;
5.  verification/recovery;
6.  preserve Delivery/CNC.

`orders/mis_autosquareoff.py` documents approximately:

-   PASS_1 15:03;
-   PASS_2 15:06;
-   protective cutoff 15:09.

A separate 15:15 force-close SOFT_KILL path exists.

The user's remembered 14:45 timing is retained only as
HISTORICAL/UNCERTAIN.

## 16. Reconciliation / recovery

`orders/order_reconciler.py`:

-   15-second daemon;
-   startup reconcile;
-   broker/local comparison;
-   reconciliation action persistence;
-   capital-drift checking;
-   crash/orphan recovery;
-   CNC/GTT reconciliation;
-   auth-error handling.

The source therefore contradicts a simple "reconciliation only alerts"
interpretation. It contains actual state/recovery actions. Exact
historical operator-visible outcomes remain partly UNKNOWN.

## 17. Crash / exception recovery

Same-day session without clean shutdown evidence is classified CRASH
unless completed EOD evidence permits WARM.

systemd: - restarts exit 1/2; - does not restart exit 0/3/4/5.

Startup reconciliation precedes signal processing after restart.

## 18. Clean shutdown

SIGINT/SIGTERM sets the shutdown event. `_shutdown()` then drains
HARD_KILL flattening within external-stop grace, stops managers, cancels
pending ENTRY orders, disconnects feed, writes SHUTDOWN, checkpoints WAL
and closes DB.

systemd intentionally sends SIGINT.

## 19. Weekend / holiday

Trading service exits before normal startup on non-trading days. Cron
jobs are independent and task-specific; universal weekend/holiday
execution is not established.

## 20. Paper vs Live

CLI defaults to paper; Production systemd explicitly runs live. Paper
was used for testing according to user recollection. User-reported
differences in prices/timing/capital are HISTORICAL observations.
Complete lifecycle parity is not established.

## 21. Testing vs Production

Sandbox contains the same major lifecycle mechanisms but is not
config-identical.

Verified example:

-   Production `entry_start=10:00`.
-   Sandbox `entry_start=09:30`.

Therefore architecture is broadly parallel, but exact behavioral
equivalence is not established.

## 22. Operator intervention

Manual mechanisms include systemd stop, `--resume`, manual/interactive
token login and operational order/configuration actions. Exact
historical frequency is not fully established.

Important: `systemctl stop trading-system` is a service stop, not the
same state transition as SOFT_KILL.

## 23. Supplemental 01-Oct Cron evidence

The uploaded Live-mode Cron Daily report shows 46 jobs, 39 done, 1
failed and 2 missed. It records, among others, 15:45 reconciliation,
15:58 broker reconciliation, 17:05 control tower, 18:45 system manager
EOD and 18:50 cron officer EOD. It also records security watcher and
alert path as alive.

This corroborates the separate cron lifecycle and must not be
interpreted as proof that the trading service itself remained alive
after its own service-window exit.

## 24. Known lifecycle findings

Historical source findings relevant to A-011:

-   token invalidation/restart-loop prevention;
-   separate START cutoff and STOP time;
-   webhook self-check degradation preserving exits/reconciliation;
-   asynchronous HARD_KILL flattening and shutdown drain;
-   startup reconciliation ordering;
-   reconciler/orphan recovery;
-   Delivery exclusion from normal EOD square-off.

These are historical findings only, not V2 decisions.

## 25. End-to-end legacy walkthrough

### Morning

`systemd/token watcher → holiday guard → service-start guard → config/DB → kill state → startup scenario → startup checks → broker/token/instruments → startup reconcile → components → webhook`

### Trading

`Chartink webhook → validation/auth → kill/queue checks → DB signal → queue → SignalProcessor → filtering/pricing/sizing/risk → order → monitor/reconcile`

### SOFT_KILL

`new entries blocked; existing exits/monitoring continue`

### HARD_KILL

`all orders blocked; cancel/flatten worker; persisted emergency state`

### EOD

`SOFT_KILL → cancel intraday entries → cancel exit legs → square off intraday → preserve Delivery → reconciliation/reporting`

### Shutdown

`position-safe service exit → SIGINT-driven shutdown → SHUTDOWN event → WAL checkpoint → DB close`

## 26. A-011 unresolved evidence

Remain UNKNOWN / NOT ESTABLISHED:

-   routine direct Python Production startup use;
-   exact queue survival after crash;
-   universal pending-order reconstruction;
-   universal broker-vs-local authority conflict rule;
-   complete auth-outage management of existing positions;
-   exact historical reconciliation repair outcomes;
-   exact historical 14:45 SOFT_KILL timing;
-   historical same-day crash/duplicate-order incidents;
-   historical lifecycle gating by shadow/evaluation components where
    source does not prove gating.

## 27. Frozen conclusion

The legacy system had a layered lifecycle rather than one visible
RUNNING/STOPPED state. The operator remembered STARTING/STOPPING; the
implementation contained COLD/WARM/CRASH/HALT,
INACTIVE/SOFT_KILL/HARD_KILL, persistent operational state, startup
reconciliation, EOD square-off, broker-managed Delivery/GTT, clean
shutdown and systemd recovery controls.

The trading-service lifecycle and cron lifecycle are separate.
Unsupported recollections remain explicitly classified.

**A-011 is CLOSED / FROZEN.**

# 28. NON-A-011 CROSS-TOPIC MEMORY PRESERVATION

These items are intentionally stored for future production/V2 governance
to avoid repeated typing.

## Broker/authentication

-   Zerodha-primary context already established.
-   Headless authentication is the desired operational context.
-   Auth-failure state machine remains future governance input.
-   New-entry vs existing-position behavior during auth outage remains
    future governance input.
-   Broker-authoritative vs local DB authority remains open governance.
-   Token refresh/restart sequence must remain explicit.

## Kill switch / governance

-   SOFT_KILL blocks new entries; it is not process shutdown.
-   HARD_KILL is distinct from normal shutdown.
-   Manual resume authorization must be explicit.
-   Persistence, audit and alert/escalation behavior must remain
    explicit.
-   Distinguish scheduled kill, emergency kill and systemd stop.
-   Distinguish entry blocking from service stopping.

## Restart / recovery

-   Preserve reconciliation-before-entry.
-   Preserve duplicate-order prevention evidence.
-   Preserve capital-state recovery questions.
-   Preserve pending-order recovery and position reconstruction
    questions.
-   Preserve crash evidence and startup scenario classification.

## EOD

-   Preserve exact square-off sequence/timing.
-   Preserve broker acknowledgement/retry questions.
-   Preserve Delivery/GTT handoff separately from MIS square-off.
-   Do not replace source timing with operator recollection when
    evidence differs.

## Configuration / timing

-   Production and Sandbox are not config-identical.
-   Production `entry_start=10:00`; Sandbox `entry_start=09:30`.
-   Production service stop config is `17:35`; independent cron jobs
    continue later.
-   Recalled `14:45` is historical unless independently evidenced.

## Signals / strategy / filtering / S&R

-   Do not silently carry legacy queue, expiry, strategy, S&R, R:R,
    secondary-filter or entry-price behavior into V2.
-   A-008 strategy-count verification remains OPEN.
-   Lifecycle documentation is not a V2 design decision.

## Capital / sizing / risk

-   A-005 frozen historical capital model remains authoritative for the
    legacy baseline.
-   Preserve its explicit ambiguities; do not normalize them.
-   Do not infer a lifecycle kill rule from a capital-drift alert
    without source evidence.

## Products / order management

-   A-007 remains authoritative for product facts/unknowns.
-   Intraday and Delivery are distinct lifecycle paths.
-   GTT/broker-managed Delivery must not be conflated with application
    lifetime.

## Alerts / reports / evidence

-   Telegram/email paths are operational evidence, not proof every alert
    was received.
-   Cron Officer, reconciliation and EOD reports should be retained as
    evidence.
-   Keep source code, runtime evidence, operator recollection and
    interpretation separate.

## Security

-   Never expose passwords, API keys/secrets, access tokens, TOTP
    secrets or webhook secrets.
-   Document only category/location/role.

## Governance note

A-011 is closed as a documentation topic. Preserved UNKNOWN/OPEN items
are evidence gaps and future governance inputs, not silently decided V2
behavior.
