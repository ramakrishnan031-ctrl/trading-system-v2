# A-011 SYSTEM STATES AND LIFECYCLE: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md, at the topic that owns them (V2_DESIGN_LOG W-6). First pass 2026-10-09; reviewed by ChatGPT the same day (structure and transfer of VP-57 to VP-65 approved, with the cautions applied below). The A-011 legacy manual was frozen on 09-Oct (DR-026); this file stays PROPOSED / OPEN, and the review authorises no V2 architecture or implementation decision.
Date: 2026-10-09
Place at: docs/agenda/a-011_system-states-and-lifecycle/A-011_v2_design_inputs.md
Why this file exists: Rama asked that nine points be noted "into manual" (Q30, Q31, Q41, Q43 to Q46, Q54, Q65), two of them explicitly for the new system's implementation (Q30, Q41), and gave one new-system direction (Q36). The legacy manual records legacy fact only (W-1, W-3), so the legacy facts behind those notes are in A-011_manual.md sections 12, 14, 15 and 17, and the new-system material is here and summarised in V2_DESIGN_LOG (VP-57 to VP-65).
Sources: Rama's answers in docs/inputs/chatgpt_originals_and_replies/A-011_SYSTEM_STATES_AND_LIFECYCLE_DISCUSSION_CHECKLIST_01-OCT-2026.txt (Q1 to Q87, sections 20 and 21); ChatGPT's 03-Oct manual (section 28) and reconciliation (section 25); legacy evidence in A-011_manual.md; earlier PROPOSED items (VP-11, VP-19, VP-21, VP-22, VP-24, VP-34, VP-35, VP-36, VP-54).
Labels: RAMA = Rama's suggestion (quoted or closely paraphrased; his answers are suggestions only). OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision. LEGACY = what the old system did (A-011_manual.md), for comparison only, not a requirement. CURRENT EXTERNAL FACT = vendor documentation with its check date.
Security: no credential values, account IDs, mailbox addresses or host addresses.

## 1. Order of intent (latest last)
- A-003 (05-Oct): VP-19 (the intraday entry cut-off kept apart from SOFT_KILL), VP-21 (square-off with two checks), VP-22 (clean shutdown with carried CNC), VP-24 (out-of-window signals rejected with a reason).
- A-006 (24-Sep) Q4, Q5: on authentication failure no new entries, keep protecting and reconciling where broker access permits, alert at once (VP-36); Q2, Q20, Q29 to Q31: broker authoritative for broker state (VP-35).
- A-010 (09-Oct): Q-29 answered (b), a scripted login as in legacy, with two M-005 conditions (VP-36); auth-failure states proposed with A-011 (VP-54).
- A-011 (01-Oct): Q30, Q31, Q41, Q43 to Q46, Q54, Q65 (notes requested) and Q36 (headless, no manual login). The section 21 list of the checklist and ChatGPT's section 25 are kept in section 14 below.

## 2. One explicit state model (VP-57)
- LEGACY (A-011_manual.md sections 3 to 6): five layers in five places (systemd unit, start-up scenario, kill switch, time windows, object states); no named service state; operator labels STARTING and STOPPING were not code states; HALT was a start-up scenario that ended the process.
- CLAUDE CONSIDERATIONS:
  - Name every layer's states and their allowed transitions in one document (O-004), for example a service layer (STARTING, READY, TRADING, ENTRIES_BLOCKED, MANAGING_ONLY, STOPPING, STOPPED, FAILED) separate from the trading gate and the object states. The names are illustrative, not proposed values.
  - READY gates TRADING: the service accepts signals only after start-up checks and reconciliation have passed (section 5); pre-flight checks that run beside the service either gate READY or are explicitly advisory.
  - Record every transition with time, reason and actor (system component or operator), so a later reader can rebuild the day (Q-03 traceability; VP-11).
  - One operator-visible status (Telegram and GUI) derived from the same states, so "what is the system doing now" has one answer.
- OPEN: the state names, the layer boundaries, and which transitions need a human (section 7). The illustrative names are examples, not an adopted state machine; later design must define the allowed transitions, their guards, ownership and persistence (ChatGPT, 09-Oct).
- Carried to: O-004, B-001 to B-012, L-005.

## 3. Blocking entries without stopping the service (RAMA Q30; VP-58)
- RAMA (Q30, verbatim): "UNKNOWN, But from my knowledge only soft or hard-kill stops processing new entries (including flatten existing positions), But there is no new mechanism to stop webhook signals [Good catch - Appreciated, take a note into manual for implementation the same]".
- LEGACY (A-011_manual.md sections 8, 11, 12): no webhook-acceptance state; the kill switch and the entry window gated acceptance; the only operator halt was systemctl stop, which also stopped exit management; an operator SOFT_KILL route was scoped and dropped on 25-Jul-2026, and the record found that a database write by another process "halts nothing" because the kill state is held in memory.
- CLAUDE CONSIDERATIONS:
  - A supported operator action that blocks new entries while exits, reconciliation and the EOD sequence continue, distinct from stopping the service; reversible; audited (who, when, why); confirmed back to the operator on both channels (VP-34).
  - Webhook acceptance as an explicit state with defined responses (accept; refuse and record; refuse because stopping), so a sender's retry behaviour is known.
  - Design the hand-off from an out-of-process request (CLI, GUI, phone) to the running process first; the legacy record is clear that writing to the database alone does not work.
  - The action should also exist as an automatic trigger (section 7) so that operator and system use the same mechanism.
  - Keep three things apart: webhook acceptance (is a request taken), trade admission (may an accepted signal become an order) and process liveness (is the service running) (ChatGPT, 09-Oct).
- OPEN: the interface (CLI, authenticated endpoint, GUI); its authentication (A-010 VP-52); what "block" covers, including the separate choice whether pending entry orders at the broker are cancelled or left pending.
- Carried to: I-001, I-008, I-013, L-005, M-010.

## 4. Signals across stop, kill and crash (RAMA Q31; VP-59)
- RAMA (Q31, verbatim): "UNKNOWN, But I guess all the queued signals were killed (ie, cancelled or let it dropped) since I found no more new entries executed, But need to investigate/review repo [Good catch, take a note into manual]".
- LEGACY (A-011_manual.md section 12): on a stop or kill the queued signals were rejected with a recorded status (REJECTED_SHUTDOWN, REJECTED_KILL_SWITCH); a crash lost the in-memory queue and left rows in non-terminal statuses; refusals at the webhook stage (kill, window, capacity, invalid, expired, duplicate) left no per-signal row, only a request count.
- CLAUDE CONSIDERATIONS:
  - Proposal, not a legacy feature: every received signal gets a recorded terminal outcome, including signals refused at the door and signals in flight at a crash (extends VP-11). The three cases stay distinct in design: a refusal at the webhook, an accepted signal with a terminal outcome, and recovery after a crash (ChatGPT, 09-Oct).
  - At start, signals left in a non-terminal status are closed with a reason (for example EXPIRED_AT_RESTART) rather than silently resumed; resuming old signals after a restart needs its own decision because a pattern seen earlier gives no assurance later (VP-24).
  - The queue's capacity, back-pressure and expiry are configuration with recorded rejections, not silent drops.
- OPEN: durable queue or in-memory queue with restart closure; the per-signal record for door refusals.
- Carried to: C-006, C-007, C-008, C-009, I-009.

## 5. Reconcile before entries (RAMA Q41; VP-60)
- RAMA (Q41, verbatim): "UNKNOWN, Need to review/investigate into GitHub repo [Good catch, Keep a careful note into Manual for new system implementation design]".
- LEGACY (A-011_manual.md section 14): a synchronous startup reconcile ran before the signal processor and the webhook, guarded by an assertion and followed by a second pass; any kill active afterwards aborted the start; the stated reason was releasing capital held for trades that were flat at the broker.
- CLAUDE CONSIDERATIONS:
  - Keep the invariant as a hard rule: no signal can be sized or placed until a start-up reconciliation against the broker has completed and its findings are resolved or have blocked entries. Test it (an ordering test, not only an assertion).
  - Make the result explicit: READY only after reconciliation (section 2); a reconciliation that cannot reach the broker leaves the system in a defined not-ready state with an alert, not in a half-started process.
  - Resolve in-flight entries from broker evidence (the legacy matched orders by tag: adopt and protect, or close and release), and never re-place an entry because the local record is uncertain.
- OPEN: what blocks READY (discrepancy severity: any discrepancy, or only material ones); how long the system waits for the broker before alerting; whether a degraded mode exists; the outcome rules for in-flight entries. The invariant itself is a candidate, not a decision (ChatGPT, 09-Oct).
- Carried to: B-005, B-012, I-012, G-015.

## 6. Pending orders and positions after a restart (RAMA Q43 to Q46; VP-60, VP-35)
- RAMA (Q43 to Q46, verbatim, the same note on each): "UNKNOWN, Need to review/investigate into GitHub repo [Good catch, take a careful note into manual]".
- LEGACY (A-011_manual.md section 15): pending orders were rebuilt from the local orders table and then checked against the broker; positions were compared with the broker every 15 seconds; the broker decided positions and fills, a local trade the broker no longer held was closed at the broker's exit price, and a broker position with no local trade was treated as a human order and left alone; the kill state was local.
- CLAUDE CONSIDERATIONS:
  - Reconstruct from the broker for broker state (orders and their status, fills, positions, GTTs) and from the local store only for what the broker does not hold (signal identity, strategy, intended SL and target, capital reservations), with the conflict rule written down per field (VP-35).
  - List which states may be pending while the process runs (entry orders, exit legs, GTTs, in-flight entries) and give each a timeout or owner, so nothing pending is unowned after a restart.
  - Decide the human-order policy explicitly for V2 (legacy: not adopted, not protected, not flattened, logged once a day).
- OPEN: the local database's role (VP-35 OPEN); reconciliation frequency and automatic actions.
- Carried to: K-001, K-002, K-012, G-015, H-005, H-006, I-004.

## 7. Kill-switch governance (VP-61)
- LEGACY (A-011_manual.md sections 6, 8): about twenty triggers in many components; two scheduled reasons (the 15:15 force close and the EOD square-off) and everything else "emergency"; every prior-day kill, HARD_KILL included, was cleared automatically at the next start by Rama's 20-Jun-2026 decision ("a new trading day ALWAYS starts with a clean slate"); same-day kills made a restart exit as HALT, so a crash after an emergency kill left open positions without in-process management; kills written by cron processes affected only the next start; scheduled kills alerted at WARNING, others at CRITICAL.
- RAMA (A-011 Q23, 01-Oct): a hard kill needed a manual resume; only a soft kill resumed automatically the next day. This differs from the recorded 20-Jun decision; for V2 it is an open choice, not a contradiction to resolve in the legacy record.
- CLAUDE CONSIDERATIONS:
  - Keep the daily entry cut-off and the end-of-day stop as named, scheduled controls, separate from emergency kills (VP-19).
  - For each kill type, state who may clear it and when: automatic at the next session, automatic when its condition clears, or a human only; and whether an emergency kill may carry over to the next day.
  - A restart must never turn "entries blocked" into "nothing is managed": a process that restarts under a kill should come up in an entries-blocked, positions-managed state (section 2), not exit.
  - A kill requested from another process reaches the running process through a designed hand-off, or is refused with an error (section 3).
  - One inventory of triggers, each with an owner, a reason code, an alert path on both channels (VP-34) and a test.
  - Every path that acts under a kill applies one product rule (legacy: the in-flight recovery under HARD_KILL had no product filter and sent its close as MIS; A-011_manual.md section 8.2; links VP-43).
  - The legacy "clean slate next day" rule is not carried forward by default: V2 needs an explicit policy per kill reason or type and per position state (ChatGPT, 09-Oct).
- OPEN: the clearing policy per kill type (Rama's choice, at I-002); whether HARD_KILL flattens (legacy flattened intraday and spared CNC, with a same-symbol defect and the in-flight exception above).
- Carried to: I-001, I-002, I-013, H-010, L-003.

## 8. The day boundary and the next day's start (VP-62; extends VP-22)
- LEGACY (A-011_manual.md section 10): the next day began only if the previous day's process had exited, because the kill clears ran only at start and the watcher started the service only when the unit was inactive; until 25-Aug-2026 a carried CNC position kept the process alive, so the next day had no start, kept the 15:15 SOFT_KILL and took no entries unless the operator stopped the service by hand; after midnight the self-exit check was not due again until 17:35; from 25-Aug a cleanly identified CNC carry no longer counted.
- CLAUDE CONSIDERATIONS:
  - Make the trading day an explicit session with its own start (token, daily resets, kill clearing per section 7) that runs on schedule whether or not the process stayed up overnight.
  - Keep "managing positions after the window" apart from "trading tomorrow": positions that need the process (an intraday survivor) are handled and alerted, while broker-protected carries (CNC with GTT) never keep the process up (VP-22).
  - Compare dates, not only times of day, in every end-of-day check.
- OPEN: whether V2 runs as one long-lived process or one process per session (O-010, M-001).
- Carried to: B-001, B-010, B-011, B-012, M-001.

## 9. End of day and uncertain exits (RAMA Q54; VP-63)
- RAMA (Q54, verbatim): "NO, I Guess once system processed auto-square off then makes another check within 1-3minutes (not sure about the duration) that's it, Meaning just makes second check only but nothing is handled [Good catch - Note it in Manual]".
- LEGACY (A-011_manual.md section 17): the process stayed up and the reconciler kept trying, but on 03-Sep-2026 the MIS square-off cancelled the protection without placing an exit, the emergency MARKET exit was rejected eight times for lack of market protection, and a human closed the position; at the baseline only the MIS square-off passed market protection.
- CLAUDE CONSIDERATIONS:
  - After the square-off, confirm flat books from the broker (two checks per VP-21) and handle any position still open actively until it is flat or handed to a defined manual procedure with an alert on both channels; a square-off step never removes protection before its replacement exit is accepted.
  - Every market order carries market protection as the current broker rules require (VP-38; A-006).
  - The end-of-day state (books flat, carries protected, residue owned) is recorded before the session closes.
- The 03-Sep incident is evidence for one path, not proof that every emergency exit path fails; requiring an accepted replacement exit before existing protection is cancelled is a design proposal, not a V2 decision (ChatGPT, 09-Oct).
- OPEN: the retry and escalation ladder; the manual procedure.
- Carried to: H-011, B-010, G-001, G-009, G-014.

## 10. Authentication failure and the daily token (RAMA Q36; VP-36, VP-54)
- RAMA (Q36, verbatim): "Existing legacy system has 2 things both: Headless & Manual (ie, create token locally then upload using .bat file command], Since headless doing without any issues we will follow the same in new system also [Manual not required in new system]".
- LEGACY (A-011_manual.md section 13; A-010 sections 7, 8): token-monitor SOFT_KILL; order-monitor HARD_KILL after three auth failures, after which the monitor stopped polling; reconciler SOFT_KILL after three cycles; IP refusals alerted without a kill; the token rename meant to stop restart loops had no trigger; the start gate exited 6 and systemd would restart it every 10 seconds.
- CLAUDE CONSIDERATIONS:
  - Q36 is consistent with Rama's Q-29 answer (b) (VP-36). The A-010 safeguard list for M-005 keeps a manual login as the fallback for the day the scripted login fails; Q36 says a manual login is not required. Both are recorded; which applies is for M-005, not A-011 (W-6).
  - The auth-failure states in VP-54 (AUTH_LOST, RECOVERING, IP_REFUSED) fit the service layer of section 2: entries blocked, positions protected at the broker where possible, recovery probes, reconciliation before entries resume, and no restart loop on a missing token.
- Rama's Q-29 answer (a scripted login) and his Q36 ("Manual not required in new system") are user preferences, not a compliance finding. M-005 owns the decision; the legacy scripted login is not an approved V2 requirement, and this review authorises no implementation (ChatGPT, 09-Oct).
- OPEN: as VP-36 and VP-54.
- Carried to: M-005, I-006, B-004.

## 11. Supervision, exit codes and liveness (VP-64)
- LEGACY (A-011_manual.md sections 7, 19): systemd restarted every non-zero exit except 3, 4 and 5 after 10 seconds; the token watcher restarted exit 5 up to three times an hour although the unit treated it as final; exits 1, 2, 6 and 9 could restart about every 10 seconds; until 17-Jul no monitor watched the trading service, and a clean exit 0 at 08:16 on 17-Jul cost a day without any alarm.
- CURRENT EXTERNAL FACT (systemd documentation, checked 2026-10-09): Restart=on-failure restarts on non-zero exits unless listed in RestartPreventExitStatus=; restarts are rate-limited by StartLimitIntervalSec= and StartLimitBurst= (defaults 10 s and 5).
- CLAUDE CONSIDERATIONS:
  - One exit-code contract for every supervisor layer (systemd and anything else that starts the service), with "needs operator" exits that no layer restarts.
  - Bounded restarts with an alert at the limit, set so that the limit can actually be reached with the chosen restart delay.
  - An external liveness check of the trading service during the session that distinguishes "stopped by design" from "down".
  - A failed non-critical check degrades and alarms; it never stops protection silently (the legacy 27-Jul rule: fail fast only on a deliberate act, degrade and alarm on an environmental cause).
- OPEN: the supervisor design (O-010, M-001); the exit-code list. The legacy restart-rate reading applies to systemd's documented defaults; the installed overrides and any actual occurrence remain UNKNOWN (ChatGPT, 09-Oct).
- Carried to: M-001, M-006, I-009, L-003.

## 12. Restart and incident evidence (RAMA Q65; VP-65)
- RAMA (Q65, verbatim): "UNKNOWN? Why? No such restarts happened as much as possible in same day, meaning if restart required usually I'll do it with next day  booting [Good catch, So take careful note into Manual]".
- LEGACY (A-011_manual.md section 14): incident records exist (17-Jun, 18-Jun, 23-Jun, 17-Jul, 03-Sep), but the database and logs are gone, so restart effects cannot be checked now.
- CLAUDE CONSIDERATIONS:
  - Record every start (classification and reason), stop (who and how), crash, kill and clear as evidence kept outside the trading database's lifetime (backed up per VP-07).
  - A daily lifecycle summary (starts, stops, kills, clears, restarts, residual positions) in the EOD report.
  - Boot proof: a start is "proven" only when its own STARTUP record exists (the legacy 26-Aug rule).
- OPEN: format and retention (L-008, K-011). Keeping lifecycle evidence outside the trading database is a future design input, not a legacy feature (ChatGPT, 09-Oct).
- Carried to: L-008, L-009, N-010, K-011.

## 13. Live-only and the testing environment (DR-001; project instructions 9.4)
- LEGACY (A-011_manual.md section 21): paper used the same lifecycle code; testing ran the same lifecycle code except a log-only shadow worker and an earlier entry start.
- V2: live-only (DR-001); no paper lifecycle. When V2 has a testing environment, lifecycle changes are applied and reconciled in both (9.4), and any intentional difference is listed.
- Carried to: N-002, N-005, Q-05.

## 14. ChatGPT's 03-Oct cross-topic items (kept so nothing is lost)

| Item (ChatGPT section 25 / checklist section 21) | Where it lives now |
|---|---|
| Auth-failure state machine; new entries against existing positions during an auth outage; token refresh and restart sequence; broker against local authority | Section 10; VP-36, VP-54; VP-35; M-005, I-006 |
| SOFT_KILL is not a process stop; HARD_KILL distinct from shutdown; manual resume authorisation; persistence, audit, alerts; scheduled against emergency kills against systemd stop; entry blocking against service stopping | Sections 3, 7; VP-58, VP-61; I-001, I-002 |
| Reconciliation before entry; duplicate-order prevention; capital-state recovery; pending-order recovery and position reconstruction; crash evidence and start classification | Sections 5, 6, 12; VP-60, VP-65; B-005, B-012, K-012 |
| Square-off sequence and timing; broker acknowledgement and retry; Delivery/GTT hand-off separate from MIS square-off; no timing from recollection when the evidence differs | Section 9; VP-21, VP-63; H-011, A-003 section 14 |
| Production and testing not configuration-identical; entry_start 10:00 against 09:30; service stop 17:35 with cron later; recalled 14:45 is historical | A-011_manual.md sections 9, 21; A-003 |
| No silent carry of legacy queue, expiry, strategy, S&R, R:R, filter or entry-price behaviour; the A-008 strategy count ("remains OPEN" in ChatGPT's text) | VP-24, VP-59; C, D, E. ChatGPT's 03-Oct "remains OPEN" wording on the count is superseded: the legacy production baseline had 16 strategy YAMLs (A-008 section 3; Q-17 closed in A-007). The V2 strategy count is a separate end-of-Section-A decision (VP-08) |
| A-005 capital model authoritative for the legacy baseline; no kill rule inferred from a drift alert | A-005; A-011_manual.md 8.3 (drift rules read from source) |
| A-007 authoritative for products; intraday and delivery separate lifecycles; GTT not conflated with process lifetime | A-007; section 8; VP-22, VP-62 |
| Telegram and email paths are evidence, not proof of receipt; Cron Officer, reconciliation and EOD reports kept as evidence; source, runtime evidence, recollection and interpretation kept apart | VP-34, VP-65; L |
| Security: no secrets; category, location and role only | Project instructions 8.3; A-010 |

## 15. External references (checked 2026-10-09)
- systemd man-page sources in the systemd repository (main branch, commit dated 2026-10-08): man/systemd.service.xml (Restart=, RestartPreventExitStatus=, RestartSec=), man/systemd.unit.xml (StartLimitIntervalSec=, StartLimitBurst=), man/systemd-system.conf.xml (DefaultStartLimitIntervalSec= 10s, DefaultStartLimitBurst= 5).

## 16. Status and boundary
- Everything here is PROPOSED or OPEN; a ChatGPT review alone does not turn any of it into a decision (W-6).
- New proposals in V2_DESIGN_LOG: VP-57 (state model), VP-58 (entries-blocked control), VP-59 (signals across stop, kill and crash), VP-60 (restart recovery before entries), VP-61 (kill-switch governance), VP-62 (day boundary), VP-63 (uncertain exits at EOD), VP-64 (supervision and liveness), VP-65 (restart evidence). Extended: VP-04, VP-11, VP-19, VP-22, VP-36, VP-54, and VP-12 (Rama's Q-33 answer, at the freeze).
- Questions: Q-37 opened (A-011 audit leads); Q-14 and Q-36 annotated; Q-33 answered by Rama on 09-Oct: (a) one trade per stock per day in any direction, so no same-day opposite-direction trade, across intraday and delivery (PROPOSED in VP-12; ratified at C-005 / F-007).
- Carried to: O-004, O-010, B-001 to B-012, C-006 to C-009, G-001, G-009, G-014, G-015, H-005, H-006, H-010, H-011, I-001, I-002, I-004, I-006, I-008, I-009, I-012, I-013, K-001, K-002, K-011, K-012, L-003, L-005, L-008, L-009, M-001, M-005, M-006, M-010, N-002, N-005, N-010.
