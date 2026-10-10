# B-001 VM AND SERVICE STARTUP: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md, at the topic that owns them (V2_DESIGN_LOG W-6). First pass 2026-10-09; revised 2026-10-10 after ChatGPT's value-added review (its section 3, every point applied; B-001_review_notes.md section 12). The B-001 legacy manual is FROZEN (DR-035); this file stays OPEN, and DR-035 decides nothing in it (it records scheduling moves only, section 11).
Date: 2026-10-10 (first pass 2026-10-09)
Place at: docs/agenda/b-001_vm-service-startup/B-001_v2_design_inputs.md
Why this file exists: Rama answered Q-B001-01 to 08, 11, 13 and 14 as V2 input and wrote: "Treat my all of the above answers as 'Suggestion or raw input only', So if you (ChatGPT/Claude/Both) have any improved suggestions then I'm very open to adopt it after further discussion(s)". On Q-B001-05 he asked: "[Take careful note in Manual for best option/opinion]". The legacy manual records legacy fact only (W-1, W-3), so his answers and Claude's considerations are here, and the legacy facts behind them are in B-001_manual.md. Summary rows: V2_DESIGN_LOG VP-66, VP-67 and the extended rows VP-05, VP-16, VP-23, VP-25, VP-46, VP-47, VP-53, VP-54, VP-57, VP-62 and VP-64.
Sources: docs/inputs/chatgpt_originals_and_replies/B-001_VM_SERVICE_STARTUP_DISCUSSION_CHECKLIST_09-OCT-2026.txt (Rama's answers); ChatGPT's 09-Oct reconciliation and manual, and its 10-Oct value-added review (docs/agenda/b-001_vm-service-startup/CLAUDE_VALUE_ADDED_REVIEW_B-001_FINALISATION_10-OCT-2026.txt); legacy evidence in B-001_manual.md and the frozen A-009, A-010 and A-011 manuals; earlier PROPOSED items in V2_DESIGN_LOG.
Labels: RAMA = Rama's answer, quoted (in his words, "Suggestion or raw input only"). OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision and not a request to decide now. CARRIED = an earlier PROPOSED item (a VP row) referred to here; B-001 does not ratify it. DECIDED = an earlier register row, cited as settled. LEGACY = what the old system did, for comparison only, not a requirement (B-001_manual.md). CURRENT EXTERNAL FACT = vendor or broker documentation with its check date.
Terms: as in B-001_manual.md: HOST BOOT, SERVICE START, DAILY START, APPLICATION INITIALISATION, READINESS.
Security: no credential values, account IDs, mailbox addresses, host addresses or host names.

## 1. Order of intent (latest last)
- A-001 and A-002 (CARRIED): VP-05 (environments: Windows PC for development, an Oracle Ubuntu VM for production); VP-06 (start and stop by calendar).
- A-003 (CARRIED): VP-16 (calendar; auto-start only on valid trading days); VP-23 (time discipline); VP-25 (cron review).
- A-009 (CARRIED; Q7, Q8): VP-47 ("make sure everything correct then only system boots, else no booting instead trigger alerts in Telegram and Email simultaneously").
- A-010 (CARRIED): VP-51 (secret storage), VP-52 (prior authorisation for sensitive changes), VP-53 (host access and copy protection), VP-54 (start-up secret validation), VP-56 (per-environment credentials); Q-01 and Q-05 open.
- A-011 (CARRIED): VP-57 (state model), VP-60 (reconcile before entries), VP-61 (kill governance), VP-62 (the trading session), VP-64 (supervision and exit codes), VP-65 (lifecycle evidence).
- Section A end (09-Oct, DECIDED): DR-027 (live-only means no paper-trading mode), DR-028 (one production Zerodha account), DR-032 (delivery is a CNC buy then an OCO GTT), DR-033 (manual actions limited to a named list; no VM console stops).
- B-001 (09-Oct, RAMA): Q-B001-01 to 14, below; new PROPOSED rows VP-66 and VP-67.

## 2. Rama's B-001 answers at a glance

| Question | Rama's choice (quoted in full in the section named) | Section |
|---|---|---|
| Q-B001-01 Host | "A. A dedicated cloud VM (Oracle VM with ubuntu OS)" | 3 |
| Q-B001-02 Environments | "A. One production VM, with development remaining on the Windows PC; no separate validation VM" | 3 |
| Q-B001-03 Start initiators | "B, C, D, and E"; not A (host boot) | 4 |
| Q-B001-04 Prerequisites | "Mark all as 'REQUIRED'", failure classes "DEFER TO OWNER" | 5 |
| Q-B001-05 Daily start model | "C. Not decided"; "[Take careful note in Manual for best option/opinion]" | 6 |
| Q-B001-06 Supervision | "B. Start the main process and restart it for specified failure classes" | 7 |
| Q-B001-07 Helper services | "C. Have different lifecycle rules depending on their purpose." | 7 |
| Q-B001-08 Outside the window | "A. Refuse the start and exit cleanly with a recorded reason." | 4 |
| Q-B001-11 Start failure | "A. Do not begin trading initialization; retain the failure reason and report it through the approved alerts/evidence path." | 5 |
| Q-B001-13 Host constraints | Restrictions and alerts for sensitive access and copying | 8 |
| Q-B001-14 Completeness | "I Guess none, but if there is once welcomes" | 9 |

Q-B001-09, 10 and 12 are historical; they are in B-001_manual.md section 13, with Rama's 10-Oct clarification of Q-B001-12 item 7.

## 3. Host and environments (RAMA Q-B001-01, Q-B001-02; VP-05, CARRIED)
- RAMA (Q-B001-01): "A. A dedicated cloud VM (Oracle VM with ubuntu OS)." Reason: "24*7 uninterrupted automation is possible, when local pc is on/off".
- RAMA (Q-B001-02): "A. One production VM, with development remaining on the Windows PC; no separate validation VM." Clarification: "Legacy system has local production/Bug fix along with Testing '/tests' for running or testing all the codes inside the project root same thing has to do to avoid complications."
- Status: Rama's preference and raw input, not an approved architecture (ChatGPT's 10-Oct review, point 3.2). The project instructions (section 11) assume the same pairing, "not yet independently reconfirmed as V2's"; it is ratified with Q-05 at M-007.
- LEGACY (B-001_manual.md section 3): a production VM, a testing VM that was its twin, and the PC; one login user with passwordless sudo; one shared virtual environment; tests in the repository's tests/ folder; a 19-Jul record shows instance-lock tests run on the production VM, which left a lock file in its /tmp; the VM's public IP was registered with the broker (A-010 section 13).
- CLAUDE CONSIDERATIONS:
  - Host availability around the clock and the trading process's lifetime are separate choices (section 6; ChatGPT's reconciliation makes the same point).
  - Order placement needs a static public IP registered with Kite (CURRENT EXTERNAL BROKER FACT, Kite Connect FAQ checked 2026-10-08; A-010 section 13), so whichever host places orders needs one, and a PC on a dynamic IP cannot stand in for it.
  - If there is no validation VM, as Rama prefers, pre-live validation would run on the PC (replay and scenario tests; the method is Q-01, open), and unless another pre-live step is chosen, the first live run of a change would be on production. Whether that is acceptable, and the deployment window and tested rollback it would need, are open questions, not approved practice (M-003, N-011).
  - Claude's reading of the tests sentence: tests stay in the repository's tests/ folder, as in legacy. Whether any test may run on the production VM is a separate question: tests that open ports, lock files, the database or the broker on the live host can interfere with the running service (legacy's lock tests left a lock file in the production VM's /tmp, 19-Jul record). A candidate: the full suite on the PC, and on the production VM only a read-only post-deploy check that cannot touch live state. To confirm with Rama at N-002 and M-003.
  - Parity (project instructions 9.4) would then cover the PC and the production VM for code and structure, never secret values (VP-56, CARRIED).
  - No machine is torn down without a verified backup (VP-07, CARRIED; lesson of 2026-10-05).
- OPEN, each at its owner: whether having no separate validation VM is acceptable (M-007, N-011); the PC's pre-live validation method (Q-01); whether any test may run on production (N-002, M-003); deployment and rollback controls (M-003); the static IP, provider, shape and region (M-007); the OS release and runtime pinning (M-003, M-007).
- Carried to: M-007, Q-05, Q-01, N-002, M-003, N-011.

## 4. What may start the service, and the host-boot boundary (RAMA Q-B001-03, Q-B001-08; VP-66)
- RAMA (Q-B001-03): "B, C, D, and E to ensure your trading bot wakes up at the right time, authenticates properly with your broker, recovers safely if it crashes, and allows for manual oversight. and why not A? Avoid A (Boot-up trigger): Booting purely on server restart is risky for trading bots. If the cloud server reboots unexpectedly due to a maintenance patch at 2:00 AM, I do not want my trading service initializing in the middle of the night before the market calendar or broker connections are ready." Conditions or exclusions: "As per your best Opinion/Judgement". (B = a daily scheduler after the trading-day gate; C = a successful daily broker-authentication/token readiness event; D = a restart controller after an eligible unexpected process failure; E = an authenticated operator action from the list governed by I and M.)
- RAMA (Q-B001-08): "A. Refuse the start and exit cleanly with a recorded reason."
- LEGACY (B-001_manual.md sections 6, 7 and 10): six start paths, including host-boot activation; at 02:00 main.py's start-window guard refused the start with exit 0 and a journal line, so the application did not initialise, which is what Rama asks; but the token watcher read any exit 0 dated today as the completed session, so a trading-day host boot before 08:00 that started the service would have lost the day (a conditional code path; no occurrence found). Exit 0 also meant "not a trading day" and "session ended". Before the start windows existed, the watcher started the service at 23:22 on 18-Jun and it ran overnight with false alerts.
- PROPOSED POLICY (RAMA, not a legacy fact and not a final design): a host boot alone never begins trading initialisation.
- CLAUDE CONSIDERATIONS (Rama invited improvements):
  1. A host boot and a supervisor are different things (ChatGPT's 10-Oct review, point 3.3). The host's service manager comes up at every host boot; what is open is whether a trading supervisor or scheduler also starts then, to evaluate the time, the token, the prerequisites and recovery without initialising trading by itself. Rama's D, a restart controller after an unexpected failure, can act after a host reboot only if that controller starts at host boot.
  2. The trade-off stays OPEN:

| Option at host boot | After an intraday reboot with open positions | At a 02:00 reboot | Main risk |
|---|---|---|---|
| (i) Nothing trading-related starts; the next scheduled event starts the session | Positions have no in-process management (linked exit legs, the end-of-day square-off, reconciliation; A-011 sections 11, 16 and 17) until the next scheduled event or an operator, with only the broker-side orders that actually exist | Nothing starts | Positions unmanaged for the rest of the session |
| (ii) The supervisor starts and sends the same gated start request as any initiator | The request passes the gates and goes to recovery, with reconciliation before any entry (VP-60) | The request is refused and recorded | The gates carry the whole burden: a gate defect or a misread state could start trading at the wrong time |
| (iii) The supervisor starts, checks state and alerts, and leaves any start during the session to an operator (E, from the named list; DR-033) | Positions wait for the operator; the alert says what is open | Nothing starts; the refusal is recorded | Depends on the operator being available |

  3. Whatever the option, every initiator (B, C, D, E, and a supervisor after a host boot under (ii)) passes the same gates (section 5).
  4. A refused start is its own outcome. It never uses an exit code or state that a supervisor or monitor reads as "session completed" (the legacy lost-day path). Each refusal carries one recorded reason (not a trading day; outside the start window; token not ready; a named prerequisite failed) and a named retry owner (the scheduler tries again at a set time, or nothing retries today) under VP-64's exit-code contract.
  5. "A recorded reason" means a record that outlives the process and does not depend on the application's logger or configuration having loaded (the legacy guard left one journal line whose retention was not established); which refusals alert is decided per class at L-003 (an expected holiday refusal needs no alert; an unexpected refusal on a trading day does).
  6. Planned host maintenance, OS updates that reboot included, happens outside market hours (M-007). That narrows the intraday-reboot case but does not remove it: provider maintenance and host faults remain.
- OPEN: the option at host boot (point 2); the mechanism (scheduler, controller); the exit codes and retry ownership (B-012, M-001, VP-64); the alert classes (L-003); VM restart handling (I-010).
- Carried to: B-012, M-001, M-007, I-010, L-003.

## 5. Prerequisites before initialisation, and failing closed (RAMA Q-B001-04, Q-B001-11; VP-67)
- RAMA (Q-B001-04): "Mark all as 'REQUIRED', Except: Classification of failure / recovery for each prerequisite which can be classified as 'DEFER TO OWNER', Why? To handle case-by-case, as specific failure-recovery mechanics are usually designed by the developer maintaining that specific module (Example: should it retry after 5 minutes, or shut down completely?)". The prerequisites listed in the checklist: a valid trading day; inside an allowed start window; the daily authentication/token step succeeded; required configuration available; the prior service instance not already running; no unresolved HALT/kill condition left by the previous run; the host's required network/service dependencies available.
- RAMA (Q-B001-11): "A. Do not begin trading initialization; retain the failure reason and report it through the approved alerts/evidence path."
- LEGACY (B-001_manual.md sections 7, 11 and 12; A-009 section 5; A-011 sections 5 to 7):

| Prerequisite | Where legacy checked it | On failure | What was recorded |
|---|---|---|---|
| Trading day | Holiday guard before logging, on the host's local date; a missing holiday file let the start continue | Exit 0 | A journal line; one Telegram a day |
| Start window | main.py [08:00, 18:15) IST; the watcher 08:00:00 to 15:59:59 | Exit 0, or no start | A journal line |
| Token | The watcher's file-date test; main.py's token gate | No start, or exit 6 restarted every 10 s | A log line, no alert |
| Configuration | The strict loader | Exit 5 | A log line; no main.py alert on that path |
| Prior instance | A lock file in /tmp (its layer possibly private to the service under PrivateTmp: an audit lead, B-001_manual.md section 7.3) and loopback port 5001 | Exit 1, restarted | A log line |
| HALT or kill | The start-up scenario, after the prior-day clears | Exit 4 | A log line; the watcher's Telegram once a day |
| Network | The unit ordered after network-online.target; the broker check | Waits; exit 9 | A log line |
| Host health (RAM, disk, clock) | The 08:30 pre-flight, beside the service | Alert only | The pre-flight report |

- Four categories for discussion (ChatGPT's 10-Oct review, point 3.4; legacy had no such classes, and this is not a description of it):
  - HARD GATE: failing it stops the start before application initialisation, with a recorded refusal.
  - RESTRICTED RECOVERY GATE: the process starts to manage existing positions, and new entries stay blocked until the condition clears.
  - ADVISORY HEALTH CHECK: recorded and alerted, without stopping the start.
  - POST-START READINESS GATE: passed after initialisation and before the first entry.
- Candidate placement, for discussion at the owners. Rama's "REQUIRED" stays the principle; the category says where and how each requirement would be enforced:

| Check (Rama's list, plus legacy evidence) | Candidate category | Owner |
|---|---|---|
| A valid trading day | Hard gate | B-008 to B-010 (VP-16) |
| Inside the allowed start window | Hard gate for a new session; recovery inside the window as VP-66 settles | B-008, B-012 |
| The daily authentication/token step succeeded | Hard gate (no broker access without it) | B-004, M-005 |
| Required configuration available and valid | Hard gate (VP-47) | B-002 |
| No prior instance running | Hard gate | B-012, M-001 |
| No unresolved HALT or kill condition | Restricted recovery gate where positions are open (managed, entries blocked; VP-61); hard gate where nothing is open | B-012, I-013 |
| The host's network and service dependencies | Hard gate for the broker path; the others advisory | B-004, M-007 |
| Host health (clock sync, disk, memory) | Advisory, or hard above a threshold (for example clock skew) | M-006, I-010 |
| Startup reconciliation with the broker | Post-start readiness gate before any entry (VP-60) | B-005 |
| Workers and feeds running | Post-start readiness gate | B-006, B-007 |

- CLAUDE CONSIDERATIONS:
  1. Run each check where it can run: some need the configuration, the state store or the broker (B-002 to B-005), as ChatGPT notes; order the checks from cheap host-level ones to costly ones.
  2. One time source for every date and time gate; legacy mixed the host's local date with a fixed IST offset (VP-23, CARRIED).
  3. Each check has one named, tested category; an advisory check is never relied on as a gate (VP-57, CARRIED).
  4. The single-instance guard holds however a second instance is started (the unit, an operator's shell, a test run); legacy's file layer may have been private to the service (an audit lead, B-001_manual.md section 7.3).
  5. Each refusal has its own reason code and a durable record (section 4, point 5), and the refusal path never depends on what failed (VP-47, VP-54, CARRIED).
  6. Failure classes per prerequisite go to their owners, as Rama asks, under one exit-code contract (VP-64, CARRIED), so that every class means the same to every supervisor.
- OPEN: the gate list, the categories, the order and the failure classes.
- Carried to: B-002 to B-008, B-012, M-001, M-005, M-006, M-007, I-010, I-013, L-003.

## 6. The daily start model: one process per session or one long-lived process (RAMA Q-B001-05; VP-62)
- RAMA (Q-B001-05): "C. Not decided; decide after B-008/B-011/B-012 evidence and discussion". Reason: "why? If I Choose'A' I don't where are the issues arises (Example: Delivery trade status even though GTT order, Previous days alert if any like positions?), At the same time if choose 'B' then tarting fresh every day eliminates "memory leaks," clears out stale cache data, ensures your daily configuration and broker tokens are freshly loaded, and prevents weird lingering bugs from compounding over multiple days, So let's discuss and decide with me + ChatGPT + Claude later [Take careful note in Manual for best option/opinion], Appreciated for adding here". (A = a fresh main-service process for each valid trading day; B = a long-lived main process that stays up outside market hours.)
- Claude's reading of the reason (to confirm in discussion): under A he is unsure how delivery status (with its GTT) and the previous day's alerts or positions would be handled; choosing B gives up the benefits of starting fresh every day, which he lists.
- LEGACY: in effect A. The watcher started the process at 08:15 and the self-exit gate ended it from 17:35 once nothing needed it (A-011 sections 5 and 10). Its defects: the next day's start depended on the previous process having exited (A-011 section 10); a trading-day host boot before 08:00 that started the service would have lost the day (a conditional code path; B-001_manual.md section 10); and before 25-Aug a carried CNC position kept the process alive overnight.
- What neither model settles (ChatGPT's 10-Oct review, point 3.1):
  - A fresh process resets process memory; it does not make persisted state clean. The database and files carried from the previous session still need validation and reconciliation at every start.
  - A long-lived process need not treat its memory as authoritative; it can reconcile persisted and broker state on a schedule.
  - Broker-side protection is not a property of the process model. A delivery position's OCO GTT (DR-032) protects it only while it exists and is active, so each position's protective orders and their status must be verified against broker data under either model (the 03-Sep naked-position incident, A-011 section 14, shows protection lost while the process ran).

| Consideration | A: a fresh process each trading session | B: one long-lived process |
|---|---|---|
| Fresh state (Rama: memory leaks, stale caches, configuration and token reloaded) | Built in for process memory: each session starts clean | Needs explicit daily resets and reloads (VP-62); stale state is the risk Rama names |
| Persisted state (database, files) | Validated and reconciled at each start; a new process does not clean it | The same, on a schedule rather than only at start |
| The broker token (it expires daily; A-006) | Loaded at each start | Must be replaced inside the running process |
| Configuration changes | Applied at the next start; VP-15's question of safe changes during the day stays either way | The same, plus an in-process reload path |
| Delivery holdings (a CNC position with its OCO GTT, DR-032) | Need no process overnight; each start reads holdings and protective orders from the broker and verifies them | Held in memory, still reconciled with the broker, which is authoritative; protective orders verified the same way |
| Monitoring outside market hours | By scheduled jobs | Possible in-process, within what the token and broker data allow |
| The day boundary | The start is the boundary; it must not depend on yesterday's exit (the legacy defect) | Needs an explicit session boundary that runs on schedule (VP-62) |
| Recovery after a crash or a host reboot | The same path as the daily start: one path to test | A path different from normal running unless designed as a session restart |
| Faults that build up | End with the session | Can compound across days (Rama) |
| Supervision | "Exit means done" fits a service manager's restart rules, if refusals are distinct (section 4) | Needs internal scheduling, idle states and a liveness test that tells idle from hung |
| Jobs on holidays and weekends | Outside the trading process either way (VP-25) | The same |

- Evidence wanted before a decision (ChatGPT's point 3.1): measured start-up and reconciliation duration; an intraday host reboot with open positions; broker/API availability at the start; duplicate-start fencing; restart recovery; alert delivery; session-boundary tests under each model. To be measured at N (testing) with M-001.
- CLAUDE CONSIDERATION (Claude's view, as Rama asked: a candidate, not a proposal to decide now; DEFERRED until B-008, B-011, B-012, M-001 and O-010 have been discussed): A, one trading process per trading session, under an always-on supervisor (the host's service manager and its scheduler, with no separate watcher holding its own restart policy), on five conditions:
  1. The session starts on schedule whatever happened to the previous process (VP-62).
  2. A refused start is recorded and retried by the scheduler, never read as a completed day (section 4).
  3. Every start (daily, after a crash, after a host reboot) runs the same gates and the start-up reconciliation before any entry (VP-60), so recovery is simply a start.
  4. Delivery holdings are protected only by broker-side orders that actually exist. At every start and in a morning holdings report (VP-06), each holding's protective order and its status are checked against broker data, and a missing or mismatched one is alerted (VP-60, VP-63; the order design is G's and H's). This also answers Rama's "Previous days alert if any like positions": at each start, broker positions, holdings and GTTs are compared with the store and any residue is alerted.
  5. Jobs that must run on holidays and weekends (backups, retention, security checks, reports) run outside the trading process (VP-25).
  - Why: it keeps the fresh daily process state Rama values; one tested path serves the daily start and recovery; and the session boundary is the process boundary, which legacy showed is easy to see and safe once the start no longer depends on yesterday's exit.
  - What would change this view: a monitoring need outside trading hours that a scheduled job cannot meet; a start (gates plus reconciliation) too slow for recovery during the session; or session-boundary tests that a long-lived process passes more simply; each measured as above.
- OPEN: the decision, DEFERRED until B-008, B-011, B-012, M-001 and O-010 have been discussed (Rama: after B-008, B-011 and B-012).
- Carried to: B-008, B-010, B-011, B-012, O-010, M-001.

## 7. Supervision and helper services (RAMA Q-B001-06, Q-B001-07; VP-64)
- RAMA (Q-B001-06): "B. Start the main process and restart it for specified failure classes. Why not chosen 'C'? Architectural Overhead & Complexity + Built-in Application Monitoring + Coordination Risk, Leaving it for your best opinion/judgement". After a normal exit: "Do not restart; record completion status, flush logs, and transition to idle/standby until the next scheduled trading day, But at the same time ensure 'Cron' jobs runs during holiday & weekends". After an abnormal exit: "Suggestion & Not decision: Attempt an automatic restart for transient faults with backoff, up to a maximum retry limit; permanently halt and alert an operator if the failure repeats or if caused by a critical risk/kill condition."
- RAMA (Q-B001-07): "C. Have different lifecycle rules depending on their purpose."
- LEGACY (B-001_manual.md sections 4, 6, 8 and 9): two supervisors with separate policies (systemd and the token watcher); helper units with mixed lifecycles: the alert watcher respawned 101,570+ times under an unsuitable restart policy until 16-Jul; the security watcher used Restart=always on purpose as a heartbeat; the watchman was bound with BindsTo=, so a start of the watchman on its own would also have started the trading service (a conditional path, never recorded), and the main unit's drop-in started the watchman with every service start; the GUI was deliberately left unbound; the cron watchdog ran from a systemd timer. Units were installed by copy and edited in place, and drifted from the repository.
- CLAUDE CONSIDERATIONS:
  1. One supervisor of record for the trading process, with one exit-code contract that every starter honours (VP-64, CARRIED). Rama's B fits this. A second component with its own restart policy for the same process (the legacy watcher) is avoided, or its policy is derived from the same contract.
  2. The restart policy fits the process type: a long-running service restarts on failure with a limit that can actually be reached; a periodic job runs from a timer, not from Restart=always on a process that exits.
  3. Rama's C for helpers: a service-scoped helper (for example a log observer) starts and stops with the trading process and can never start it on its own (legacy's BindsTo= could have, had the watchman been started alone); a monitor whose job is to notice the trading process being down (liveness, alert delivery, security) runs independently of it.
  4. Normal exit, as Rama asks: the supervisor never restarts a completed session. "Idle/standby" depends on section 6 (under A the process exits and the scheduler starts the next session). Jobs that must run on non-trading days stay independent of the trading process (VP-25, CARRIED).
  5. Abnormal exit, Rama's suggestion: bounded restarts with backoff and an alert at the limit; "needs operator" exits (a risk or kill condition, HALT, invalid configuration) are never restarted (VP-64, VP-47, VP-61); a restart while positions are open comes up with entries blocked and positions managed, never by exiting (VP-61; the restricted recovery gate, section 5).
  6. Units, drop-ins and timers are deployed by the same scripted route as the code and compared with the commit after every deployment (VP-46, CARRIED), so an installed unit cannot drift unnoticed.
- OPEN: the supervisor design and the exit codes (M-001, B-012).
- Carried to: M-001, M-002, M-003, B-012, I-009, L-003.

## 8. Host security constraints (RAMA Q-B001-13; VP-52, VP-53)
- RAMA (Q-B001-13): "Implement new restrictions for accessing new system: For important security alerts, Some important commands triggers alerts (example: changing .env file; Copying systems codes-not system generated .csv reports; number of simultaneous system access-use config.yaml; VM>PC sync restrictions/approval etc), Purpose: Unauthorised system access or Duplicating or copying entire system for unauthorised usage".
- LEGACY (A-010 sections 14 to 16; B-001_manual.md sections 3 to 5): several of these existed as alert-only controls: content-hash alerts on watched files, .env among them (CRITICAL); an alert when active SSH sessions exceeded a limit (WARNING, default 2, set in config/security.yaml); a copy gate that blocked VM-initiated copies at night, above 2 SSH sessions or without a self-issued token, while PC-initiated pulls were only detected; auditd rules watching .env, the configuration and the units (record). The login user had passwordless sudo, the watcher ran as root, and SSH was open to the internet.
- Status: candidate controls, not approved requirements; none is designed or implemented in B-001 (ChatGPT's 10-Oct review, point 3.5).
- What each control needs when its owning topic designs it: the protected asset or action; the threat or actor; prevention or detection; the enforcement point; the exception and approval path; evidence retention; the alert recipient and severity; false-positive and recovery behaviour. Two limits already stand: a YAML value enforces a concurrent-access limit only when a real component or an OS control reads and applies it; and a copy alert is not copy prevention.

| Rama's item | Candidate prevention (access and permissions) | Candidate detection (alert) | Owner |
|---|---|---|---|
| Changing the .env file | The secrets owned by a service identity; no write access for the login user | A content-hash alert (legacy had one) | M-001, M-007; VP-51 |
| Copying system code (not generated reports) | Who can log in, and what that user can read; the code also lives in the private repository (DR-007) | Audited reads or copy-tool wrappers (legacy's copy gate only detected PC-initiated pulls) | M-007; VP-53 |
| Number of simultaneous system accesses | Enforced at each channel: the SSH service or the OS for logins, the GUI for its sessions | An alert above the limit (legacy's only control) | M-001, M-007 |
| VM to PC sync restrictions or approval | Permissions on the sync path; approvals per VP-52 (an approval code over Telegram and email is never the sole control) | Alerts on sync events | M-003; VP-52 |

- CLAUDE CONSIDERATIONS (options for M-001 and M-007, not decisions): permissions and access paths prevent, and alerts are the second line (VP-53, VP-52, CARRIED); examples to weigh there: a dedicated service user without a login shell that owns the secrets and runs the service; no passwordless sudo for the login user; SSH only over a private network, with keys protected by a passphrase or held in hardware. These alerts follow the global alert rule (VP-34) and never carry a secret (VP-55).
- OPEN: the host design (M-001, M-007), the operator actions (I-013, M-010), the alert classes (L-003).
- Carried to: M-001, M-003, M-007, I-013, M-010, L-003.

## 9. Subjects to add to the startup scope (RAMA Q-B001-14: "I Guess none, but if there is once welcomes")

| Subject | Why it matters (legacy evidence in B-001_manual.md) | Owner |
|---|---|---|
| Host clock and time zone | Start-path components used different time sources and agreed only because the host ran on IST (section 11) | VP-23; K, I-010, M-007 |
| Network readiness at host boot | The unit waited for network-online.target, whose meaning depends on the host's network service (section 5) | M-001, M-007 |
| Service identity and privileges | The login user ran the service with passwordless sudo; the watcher ran as root (sections 3, 4) | M-001, VP-53 |
| How unit files reach the host | Units were copied and edited in place; the deployment hook did not carry them (section 9) | M-003, VP-46 |
| Listening ports and exposure | The webhook on all interfaces with no proxy; the health server; the GUI and lock ports on loopback (section 3) | C-002, C-003, M-007; A-010 Q-36 (h) |
| Host health: gate or advisory | Pre-flight checks ran beside the service (section 12) | M-006, VP-57 |
| Planned maintenance and host reboots | The time of a host boot decided the start path (section 10) | M-007, I-010 |
| Runtime version pinning | A shared venv outside the project; Python 3.12.3 recorded against a guide's 3.11 (section 3) | M-003 (project instructions 5.5) |
| External liveness | A documented external monitor not established; "stopped by design" against "down" (section 9) | M-006, VP-64 |
| The meaning of each exit and refusal | Exit 0 had three meanings (section 7.5) | VP-64, VP-66 |

## 10. External references (checked 2026-10-09)
- systemd man-page sources in the systemd repository (main branch, commit dated 2026-10-09): systemd.unit (WantedBy=, Wants=, Requires=, BindsTo=), systemd.service (Type=simple), systemd.exec (PrivateTmp=), systemd.special (network-online.target, multi-user.target), systemctl (--timestamp=), journald.conf (Storage=). The restart rules: A-011 section 7 (checked 2026-10-09).
- Broker: the static-IP requirement for order placement (A-010 section 13, checked 2026-10-08).

## 11. Status and boundary
Nothing in this file is decided. The B-001 legacy manual is FROZEN (DR-035) and records legacy fact only (W-1, W-6); Rama's answers stay his raw input, and Claude's considerations stay considerations. Recorded in DR-035 as scheduling only, with no proposal content decided:
- B-001 is dropped from the ratification points of VP-16 (B-008 to B-010 remain; its start-initiation part is carried by VP-66), VP-47 (B-002, B-012, L-003 and I remain), VP-54 (B-002, O-004, I, K and L-003 remain) and VP-62.
- VP-62's ratification points become B-008, B-010 to B-012, M-001 and O-010 (Rama's Q-B001-05: decide after B-008, B-011 and B-012; ChatGPT's 10-Oct review, point 3.1, adds M-001 and O-010).
- VP-57's range becomes B-002 to B-012 (its B-001 part is carried by VP-66 and VP-67).
- New PROPOSED rows: VP-66, ratified at B-012, M-001, M-007, I-010 and L-003; VP-67, ratified at B-002 to B-007, B-012, M-006, I-010 and L-003.
No V2 architecture or implementation decision is authorised.
