# B-001 — VM / Service Startup

**Project:** Trading System — Legacy Baseline and V2 Redesign  
**Evidence/review date:** 09-Oct-2026  
**Status:** OPEN — principle-level discussion input; not frozen  
**Scope:** Host/VM boundary and initiation of the main service only

> **Governance boundary:** The legacy system is DEAD / REFERENCE ONLY. This chapter documents source-defined behavior and Rama's stated preferences. It does not authorize operation, repair, redesign, deployment, or V2 implementation. B-002 through B-012 and the relevant M/I/L topics retain ownership of their detailed subjects.

## 1. Purpose

B-001 establishes the boundary between the cloud host, the main trading process, the service supervisor, the token watcher, and external start requests. It records what the accessible source says, what Rama recalls, and what remains unproven about historical production execution.

B-001 does **not** fully document configuration loading, database/state initialization, broker connection, startup reconciliation, worker initialization, readiness, intraday behavior, EOD, shutdown, or recovery. Those are separate agenda items.

## 2. Evidence basis and limits

Repositories referenced:

- Production code reference: [trading-system-prod-snapshot](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot)
- Sandbox/testing snapshot and captured records: [trading-system-sbx-snapshot](https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot)
- Supporting records: [trading-system-records](https://github.com/ramakrishnan031-ctrl/trading-system-records)

Evidence classes used in this chapter:

| Label | Meaning |
|---|---|
| **SOURCE-DEFINED** | Behavior or declaration present in a repository file. |
| **RECORD-DERIVED** | A preserved host capture records a state at a specified time. |
| **USER HISTORICAL RECOLLECTION** | Rama's recollection, retaining the confidence level he stated. |
| **USER PREFERENCE / RAW INPUT** | Desired principle or option for discussion; not automatically an approved requirement. |
| **INTERPRETATION** | Cautious inference from source evidence, not an independently observed event. |
| **UNKNOWN / NOT ESTABLISHED** | Accessible evidence does not establish the fact. |

The project’s existing startup manual explicitly states that the production host was not directly read; machine-side facts in that manual are derived from preserved testing-VM captures. Consequently, repository files can establish what the source declares but cannot alone prove which production units were enabled, which versions were installed at a historical time, or which event caused a service to start.

## 3. System boundary

The relevant conceptual boundary is:

```text
Cloud VM / operating system
        |
        +--> systemd main-service unit
        |         |
        |         +--> main.py --mode live
        |
        +--> systemd token-watcher unit
                  |
                  +--> token_watcher.sh
                            |
                            +--> conditional systemctl start request
```

This diagram shows mechanisms defined by source, not a proven historical event sequence. A host boot, service start request, process initialization, and trading readiness are different events.

### 3.1 Host uptime versus process uptime

Rama's stated preference is a dedicated Oracle cloud VM running Ubuntu and intended to be available continuously, while the Windows PC can be switched off.

**Interpretation:** continuous host availability does not decide whether the main process should run continuously, start daily, or exit and be started again. That is explicitly unresolved under Q-B001-05 and depends on later lifecycle evidence.

## 4. Source-defined startup mechanisms

### 4.1 Main service unit

**Source:** `deploy/systemd/trading-system.service`

The production repository's unit declares:

| Directive | Source-defined value | B-001 significance |
|---|---|---|
| `Type` | `simple` | systemd supervises the process; this alone is not an application-readiness guarantee. |
| `User` / `Group` | `ubuntu` / `ubuntu` | Declared service identity. |
| `WorkingDirectory` | `/home/ubuntu/systems/trading-system` | Declared working directory. |
| `EnvironmentFile` | `/home/ubuntu/systems/trading-system/.env` | The unit expects environment values from this file; secret values are not reproduced here. |
| `ExecStart` | virtual-environment Python invoking `main.py --mode live` | Declares the startup command and mode argument. |
| `StandardInput` | `null` | Headless service; no interactive input is expected. |
| `KillSignal` | `SIGINT` | Stop requests are intended to enter the application's interrupt/shutdown path. |
| `TimeoutStopSec` | `30` seconds | Declared stop timeout. |
| `Restart` | `on-failure` | systemd-level restart policy. |
| `RestartSec` | `10` seconds | Declared restart delay. |
| `RestartPreventExitStatus` | `3 4 5` | These exit statuses suppress systemd's automatic restart under this unit. |
| `WantedBy` | `multi-user.target` | Unit is eligible for boot activation if enabled. |

**Important limitation:** `WantedBy=multi-user.target` does not prove that the unit was enabled on the historical production VM. The `--mode live` argument does not prove that a process ran, authenticated with the broker, or placed orders.

### 4.2 Token watcher unit and script

**Sources:**
- `deploy/systemd/token-watcher.service`
- `deploy/token_watcher.sh`

The watcher unit declares `Restart=always` and launches the shell watcher. The script polls the main service and considers:

- service active state;
- last main-service exit status and exit date;
- whether the access-token file exists and contains today's date and a non-empty access token;
- whether the current time is inside the watcher's service-start window;
- the count of crash restarts in the preceding hour.

The script's defaults include a 30-second polling interval, a longer 300-second backoff for selected cases, and a maximum of three crash restarts per hour. It differentiates today's clean exit, today's HALT, today's startup-check failure, prior-day outcomes, and crash exits. Depending on the branch and checks, it may call `systemctl start trading-system.service`.

These are **SOURCE-DEFINED** behaviors. Actual historical enablement, process state, start events, and restart history remain **UNKNOWN / NOT ESTABLISHED** without contemporaneous machine records.

### 4.3 Cron's role at the startup boundary

**Source:** `deploy/cron/trading-system.cron`

The canonical crontab includes:
- token-file cleanup at 05:00 daily;
- token refresh at 08:15 Monday–Friday;
- preflight phases at 08:30, 09:14, and 09:15 Monday–Friday;
- independent backup, monitoring, EOD, retention, and reporting jobs.

No inspected canonical cron line directly executes `systemctl start trading-system.service`. The source-defined component that can request the main service start after checking token, time, and service state is the token watcher.

Detailed scheduler inventory and live-crontab reconciliation belong to the scheduler manual and agenda item M-002. This chapter does not claim that the canonical crontab was installed unchanged on the historical production host.

## 5. Start-window discrepancy

Two source-defined windows differ:

| Source | Window | Evidence |
|---|---|---|
| `main.py` | `[08:00, 18:15)` IST | `_within_service_window` and the start guard in `main()` |
| `deploy/token_watcher.sh` | `[08:00, 16:00)` IST | `within_service_window()` in the watcher script |

These must not be silently merged into one legacy rule. The code shows two different checks; the exact operational interaction on the historical production host is **UNKNOWN / NOT ESTABLISHED**.

Relevant dependencies: B-008 (market-open operation), B-012 (restart/recovery), M-001 (systemd), M-002 (cron), and M-005 (token management).

## 6. Main-process early guards and meaning of a start

The source in `main.py` checks command-line arguments, then the holiday/weekend guard, and then the service-start-window guard before ordinary logging setup and validated configuration loading.

The guarded holiday and out-of-window paths return exit code `0`. Therefore:

- a service command may run and exit cleanly without full application initialization;
- exit code `0` does not prove trading readiness;
- a successful `systemctl start` command does not prove that all prerequisites passed;
- an active process does not by itself prove that the signal/trading path is ready.

The main guard prints a reason, but because it runs before ordinary logging setup, this chapter does not claim that every such reason is durably persisted through the application's normal logging path. Exact recording and alert behavior requires a separate trace of the relevant paths.

Configuration loading is owned by B-002; database/state initialization by B-003; broker connection by B-004; reconciliation by B-005; worker initialization by B-006; readiness/health by B-007.

## 7. Reconciled record of Rama's answers

| Question | Rama's answer, preserved in substance | Reconciliation / status |
|---|---|---|
| **Q-B001-01 — Host/VM model** | Dedicated Oracle Ubuntu VM; intended 24×7 automation; PC may be off. | **Preference captured.** Does not decide continuous main-process uptime. |
| **Q-B001-02 — Environment separation** | One production VM; development on Windows PC; no separate validation VM. Legacy production, bug fixes and tests used the same project root, including `/tests`. | **Preference plus historical recollection.** The recollection is not independently verified. No new topology is approved here. |
| **Q-B001-03 — Start initiators** | B, C, D, E; avoid direct VM-boot triggering because an unexpected overnight reboot could initialize the service too early. | **Principle captured.** Distinguish host boot, scheduled start, token event, retry, and operator action. None is unconditional permission to trade. |
| **Q-B001-04 — Prerequisites** | All listed prerequisites required; classify failure/recovery at the owning module. | **Safety principle captured.** Exact checkpoints and failure actions are deferred to B-002–B-007/B-012 and operations owners. |
| **Q-B001-05 — Daily start model** | Not decided; revisit after B-008/B-011/B-012. | **OPEN / DEFERRED.** Do not choose fresh daily process versus long-lived process. |
| **Q-B001-06 — Supervision** | Start the main process and restart for specified failure classes; reviewer may assess complexity. | **Preference captured.** Exact restart contract is deferred to B-012/M-001. |
| **Q-B001-07 — Main service and watcher** | Different lifecycle rules according to purpose. | **Principle captured.** Historical enablement/running state of either unit remains unknown. |
| **Q-B001-08 — Outside-window start** | Refuse start and exit cleanly with a recorded reason. | **Partially corroborated.** Source has clean guarded exits, but windows differ and durable recording is not established for every early guard. |
| **Q-B001-09 — Historical production start** | Believes it started automatically at VM boot, but is not fully sure. | **USER HISTORICAL RECOLLECTION / LOW CONFIDENCE.** Repository cannot prove unit enablement or the cause of a historical start. |
| **Q-B001-10 — Sandbox live-mode capture** | Unknown; leave NOT ESTABLISHED. | **Preserve as UNKNOWN.** Unit text does not prove actual live trading or order placement. |
| **Q-B001-11 — Startup failure** | Do not begin trading initialization; retain failure reason and report via approved alerts/evidence. | **Safety principle captured.** Exact failure paths and persistence/alert guarantees are deferred. |
| **Q-B001-12 — Historical incidents** | Historical/unknown; no fully evidenced incident timeline provided. | **No incidents inferred.** Add only dated evidence-backed events. |
| **Q-B001-13 — Host security constraints** | Consider alerts/restrictions for `.env` changes, copying system code (excluding generated CSV reports), simultaneous access, and VM-to-PC sync/approval. Goal is to prevent unauthorized access/duplication. | **Security governance input only.** Alert-only versus prevention, scope, approvals, and evidence remain undecided. |
| **Q-B001-14 — Completeness** | No further topic identified, but welcomes any reviewer additions. | **Completeness not confirmed.** Reviewer cross-check remains open; Rama need not invent topics. |

## 8. Open questions and dependencies

1. Was `trading-system.service` enabled on the historical production host, and during which dates?
2. Was `token-watcher.service` enabled and running on that host?
3. Did a historical start originate from VM boot activation, token watcher, manual operator action, or another mechanism?
4. Which deployed versions of `main.py`, the service unit, and watcher script were present at each relevant historical time?
5. How did the 08:00–18:15 main guard and 08:00–16:00 watcher guard interact on the actual host?
6. Did any start occur before the daily token refresh? Source does not establish the historical sequence.
7. Does “no trading day = no system booting/starting” mean no VM boot, no main-process start, or no trading initialization?
8. What exact failure classes, retry limits, backoff, and manual-resume rules should apply? Defer to B-012/M-001.
9. Do proposed host security controls require alerting, access prevention, or both? Defer to relevant governance topics.
10. What durable evidence path records reasons emitted before ordinary logging is configured?

## 9. B-001 status and closure criteria

**Current status: OPEN — NOT CLOSED / NOT FROZEN.**

Captured as discussion input:
- desired dedicated Oracle Ubuntu VM and availability;
- one-VM plus Windows-PC development preference;
- allowed start-event principle;
- prerequisite-required safety principle;
- defined failure-class supervision preference;
- distinct lifecycle rules for main service and watcher;
- outside-window refusal preference;
- startup-failure fail-closed principle;
- candidate host-security controls.

Explicitly deferred:
- daily process lifecycle;
- exact restart and manual-resume contract;
- ownership-specific prerequisite failure behavior;
- security-control scope and implementation.

Unknown / not established:
- historical production unit enablement and actual boot-start causality;
- actual watcher enablement/running state;
- actual sandbox live trading behavior;
- historical production runtime configuration and event history;
- historical interaction of the two source-defined time windows.

B-001 should close only after Rama and reviewers agree the principle-level outcome, record the dependencies and source discrepancies, and update the project decision register. Do not treat questionnaire answers as automatically approved V2 requirements.

## 10. Cross-references

- **B-002:** Configuration loading
- **B-003:** Database/state initialization
- **B-004:** Broker connection and token prerequisite
- **B-005:** Startup reconciliation
- **B-006:** Worker initialization
- **B-007:** Readiness/health
- **B-008:** Market-open operation and allowed time semantics
- **B-011:** Shutdown
- **B-012:** Restart/recovery
- **M-001:** systemd
- **M-002:** cron/scheduler
- **M-005:** token management
- **I-series:** safety and recovery
- **L-series:** alerts, observability, evidence, audit

## 11. Source references

Repository paths reviewed for this chapter:

- `deploy/systemd/trading-system.service`
- `deploy/systemd/token-watcher.service`
- `deploy/token_watcher.sh`
- `deploy/cron/trading-system.cron`
- `main.py` — `main()`, `_within_service_window()`, start guard

Supporting project references:
- `03_STARTUP_RUNTIME_AND_SCHEDULER.md` — prior startup/scheduler evidence map, whose header explicitly distinguishes the testing VM from production.
- `MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt` — agenda order and decision governance.
- `B-001_VM_SERVICE_STARTUP_DISCUSSION_CHECKLIST_09-OCT-2026(1).txt` — Rama's completed answer sheet.

**Evidence limitation:** This chapter summarizes the accessible repository declarations and answer sheet. It does not assert that the historical production VM matched the repository at every point in time.

---
**End of B-001 manual — status OPEN.**
