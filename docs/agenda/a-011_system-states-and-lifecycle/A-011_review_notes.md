# A-011 SYSTEM STATES AND LIFECYCLE: REVIEW NOTES

Date: 2026-10-09
Status: CLOSED 2026-10-09. ChatGPT's value-added review (09-Oct-2026, APPROVE THE SUBSTANCE WITH REQUIRED CLARIFICATIONS) applied (section 12). Rama answered Q-33 on 09-Oct: (a). The A-011 legacy manual is FROZEN (revision 1, DR-026) together with the five errata accepted in the same review (section 6); the V2 inputs remain PROPOSED / OPEN (A-011_v2_design_inputs.md). First pass written the same day. Erratum E-1 to the A-011 manual (section 7, the exit-5 wording: exit 5 is in the unit's RestartPreventExitStatus= directive, not its comment block) added 2026-10-09 during B-001 (B-001_review_notes.md section 6), reviewed by ChatGPT on 10-Oct-2026, approved by Rama on 10-Oct-2026 and recorded in DR-035 together with the B-001 freeze. It corrects only that line; the A-011 freeze (DR-026) is unchanged. Erratum E-2 to the A-011 manual (sections 5, 7, 13 and 20: the live pre-load's exit codes; the accounts.csv exit by mode; the interactive start; a pointer to B-002's exit-code matrix) added 2026-10-12 during B-002 (B-002_review_notes.md section 6), revised 2026-10-13 after ChatGPT's value-added review, accepted in ChatGPT's final acceptance check and approved by Rama on 13-Oct-2026, and recorded in DR-037 together with the B-002 freeze. It corrects only those passages; the A-011 freeze (DR-026) is unchanged.
Place at: docs/agenda/a-011_system-states-and-lifecycle/A-011_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-011_manual.md; V2 inputs in A-011_v2_design_inputs.md and V2_DESIGN_LOG. External facts labelled per W-5.
Security: no credential values, account IDs, mailbox addresses, host addresses or person names. The reading method is in section 8.

## 1. Sources reviewed
- docs/inputs/chatgpt_originals_and_replies/A-011_SYSTEM_STATES_AND_LIFECYCLE_DISCUSSION_CHECKLIST_01-OCT-2026.txt (Rama's answers Q1 to Q87 and sections 20 and 21)
- docs/inputs/reconciliation/A-011_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_03-OCT-2026.txt
- docs/inputs/manuals/A-011_SYSTEM_STATES_AND_LIFECYCLE_LEGACY_MANUAL_03-OCT-2026.md
- Production source at 970aabf (commit dated 2026-09-11) and eight records in the same repository (list in A-011_manual.md section 2); the testing snapshot ade89ff (object comparison and five files); systemd's man-page sources (checked 2026-10-09).
- The supplied 01-Oct-2026 Cron Daily report is known only through ChatGPT's reconciliation; the report itself was not supplied to Claude.

## 2. Overall
ChatGPT's 03-Oct set has the right shape and its central findings stand: the internal state model is richer than the operator's labels; the service lifecycle is separate from cron; a startup reconcile precedes entries; SOFT_KILL is not a process stop; the recollected 14:45 is not a source time. It was written without reading the code paths end to end. Read directly, the source settles most of its UNKNOWNs at code level and changes five of its statements: every prior-day kill, HARD_KILL included, was cleared at the next start (Rama's 20-Jun-2026 decision, recorded in the code); systemd restarted exits 6 to 9 as well as 1 and 2, and the watcher restarted exit 5; the webhook checks the kill switch, capacity and window before parsing; drained signals on a clean stop were rejected, not executed; fund-manager capital drift did escalate to kills. The baseline's own records explain Rama's main lifecycle memory: until 25-Aug-2026 a carried delivery position held the service open, so the next day had no start and no entries unless he stopped it by hand, which records of 05-Aug and 06-Aug show. Two of Rama's answers conflict with the source (Q23, Q86, section 4). Five frozen manuals need errata (section 6), all accepted in ChatGPT's review. Q-33 was put to Rama and answered (section 7).

## 3. Findings

| # | Finding | Evidence | Manual |
|---|---|---|---|
| F-1 | Five state layers in five places (systemd unit, start-up scenario, kill switch, time windows, object states). No STARTING, READY or STOPPING state exists in the service code; READY is only the pre-flight verdict the GUI shows. | main.py; startup_checks.py; kill_switch.py; schema.sql; ops_dashboard | 3, 4 |
| F-2 | The daily start: the token watcher starts the unit only when it is not active, the token is today's, the hour is 08 to 15 and the last exit allows it; the boot runs the checks and token gate before building components and two reconcile passes before signals. | token_watcher.sh; main.py; order_reconciler.py | 5, 14 |
| F-3 | At start, clear_stale_state clears every prior-day kill, SOFT or HARD, scheduled or emergency ("Rama's 2026-06-20 decision", "HEADLESS GUARANTEE"); same-day scheduled kills are cleared when flat; HALT is a same-day kill that survives both. | kill_switch.py; main.py; 26-Aug boot record | 6 |
| F-4 | A --resume start disarms the EOD self-exit gate; resume.sh is the systemd-friendly resume; the watcher's HALT message still points to main.py --resume. | main.py; resume.sh; token_watcher.sh | 6 |
| F-5 | Exit codes: systemd restarts 1, 2 and 6 to 9 after 10 s (start-limit defaults cannot be reached at that delay); 3, 4, 5 not restarted by systemd; the watcher restarts exit 5 up to three times an hour. | unit; token_watcher.sh; systemd docs | 7 |
| F-6 | The kill state is memory-authoritative; a kill written by another process (System Manager at 18:45) only reaches the next start, which clears it as a prior-day kill. The F2 operator SOFT_KILL route was dropped on 25-Jul; systemctl stop was the only halt. | kill_switch.py; system_manager.py; F2 record | 8.1, 8.3, 11 |
| F-7 | Trigger inventory from every call site: 15 SOFT_KILL rows (two scheduled; one cron-only; one unscheduled script; the live feed's own calls inert) and the six HARD_KILL callers. ChatGPT listed a subset. | all non-test call sites | 8.3 |
| F-8 | Capital drift: fund-manager sources escalate (Rs 1,000 SOFT and Rs 2,500 HARD on one event; Rs 250 to 1,000 SOFT after three events); the reconciler's G3 drift alerts only; code comments say the kill rungs never fired. | drift_handler.py; order_reconciler.py | 8.3, 8.4 |
| F-9 | HARD_KILL flatten: asynchronous; CNC spared since 03-Aug; a same-symbol mixed book can still lose CNC quantity (latent, unfixed at the baseline); "never fired" as of the 03-Aug drill. | kill_switch.py; position_helpers.py; kill drill | 8.2 |
| F-10 | 14:45 is a Telegram pre-alert, not a kill; entries stop at 15:00 by the window; the daily scheduled SOFT_KILL is at 15:15. This settles A-003's open point: the cut-off did not use the kill switch. | main.py; market_windows.py; order_monitor.py | 8.4, 9 |
| F-11 | The EOD self-exit gate counted every open position until 25-Aug-2026 ("Option A, Rama"); since then a cleanly identified CNC carry does not count. Its due test compares time of day only. Records show manual evening stops for CNC carries (05-Aug, 06-Aug, planned 31-Aug). | main.py; stop procedures; 30-Aug handoff | 10 |
| F-12 | The next trading day depended on the previous process having exited: the clears run only at start, so a held process kept yesterday's SOFT_KILL and refused every signal with 403. | main.py comment of 25-Aug; webhook_receiver.py | 10, 12 |
| F-13 | Shutdown order; it cancels only tracked entry orders, never squares off, cancels a GTT or releases capital; an external stop waits 15 s for a flatten. | main.py; order_monitor.py; 05-Aug record | 11 |
| F-14 | Webhook gate order (shutdown, rate limit, auth, scanner, EOD route, kill, capacity, window, then parsing and per-symbol checks); door refusals leave no signals row; on a clean stop drained signals are rejected as REJECTED_SHUTDOWN; a crash loses the in-memory queue. | webhook_receiver.py; signal_processor.py | 12 |
| F-15 | Auth paths: the order monitor's three-strike BrokerAuthError path trips HARD_KILL and stops the monitor; the reconciler's trips SOFT_KILL; the token rename has no inspected trigger (A-010 E-1). | order_monitor.py; order_reconciler.py; main.py; live_feed.py | 13 |
| F-16 | Persistence, rehydration and duplicate safeguards (in-flight entries matched by broker tag; fingerprint; one GTT per trade; terminal-status trigger). | main.py; order_reconciler.py; schema.sql | 14 |
| F-17 | Status sets per table (signals open set; trades; orders by the state machine); ChatGPT's list mixed trade and order statuses. | schema.sql; order_state_machine.py | 15 |
| F-18 | Uncertain exits: the 03-Sep incident; at the baseline only the MIS square-off passes market protection, so the other MARKET exits and fallbacks send none. | incident record; order_reconciler.py; eod_squareoff.py; kill_switch.py; zerodha_adapter.py | 17, 22 |
| F-19 | Cron: 46 enabled jobs (31 market-day-only, 15 not), matching the supplied report's 46; the report's 01-Oct date sits after A-001's estimate of the VM deletion. | cron_registry.yaml; reconciliation | 19 |
| F-20 | Testing ran the same lifecycle code: 41 of the 46 lifecycle files compared (a selection for this topic, not the whole repository) identical by object; the rest add a log-only shadow, force_qty, CRLF endings and configuration values (the 09:30 entry start among them). | object comparison; five files | 21 |
| F-21 | The GUI controls were read-only; the planned control server is not started by the inspected trading code. | ops_dashboard | 20 |
| F-22 | Incident records: 17-Jun, 18-Jun, 23-Jun, 17-Jul, 03-Sep. | records and code comments | 14 |
| F-23 | Added after ChatGPT's review, while re-reading the HARD_KILL call paths for its point 3.1: under HARD_KILL the reconciler's in-flight recovery closes a filled in-flight entry whatever its product (no product filter on that path), with the intraday intent because the trade row it reads has no product column, as a MARKET order without market protection. Code path only, not observed (HARD_KILL had not fired as of 03-Aug). It concerns an unresolved entry, not an established delivery position, so A-007 needs no change. | orders/order_reconciler.py (_adopt_or_fail, _recovery_flatten_under_kill, _emergency_market_close); core/schema.sql; core/state_store.py | 8.2, 14, 22 |

## 4. Rama's answers against the source

| Question | Rama | Source (manual section) |
|---|---|---|
| Q1 to Q3 labels | STARTING and STOPPING only; no READY | No such code states; Telegram markers; READY is the pre-flight verdict (4) |
| Q4, Q5 | Alive with entries blocked; positions managed | Consistent (4, 8.1) |
| Q6 boot chain | 08:15 token, instruments, pre-flight, briefing, wait, trade, cron, stop 17:30 | Consistent in order; pre-flight and briefings are cron jobs beside the service (5) |
| Q7 | systemd (guess) | Consistent (5) |
| Q8 | UNKNOWN | Still UNKNOWN; the scripts record an 18-Jun collision from a standalone --resume (5) |
| Q9, Q10 | UNKNOWN | Exit 3 before trading starts; restart rules per exit code (6, 7) |
| Q11 | systemd restarts; refusal to stop with open delivery; manual stop | Restarts: 1, 2 and 6 to 9 (systemd), 5 (watcher); the 08:15 start is the watcher's; the refusal is the self-exit gate before 25-Aug (7, 10) |
| Q12 | Clean stop differs from a crash | Consistent: SHUTDOWN event against CRASH (7, 11) |
| Q13 | --resume used (guess) | Frequency UNKNOWN; resume.sh is the systemd path (6) |
| Q14, Q17 | HALT = the stop command | In the code HALT is a start-up scenario; the stop is a service action (6, 11) |
| Q15, Q16 | HALT survives restarts; next day boots itself | Same-day kills survive; prior-day kills are cleared at start (6) |
| Q18 | SOFT_KILL at about 14:45 | Scheduled SOFT_KILL at 15:15; 14:45 is a pre-alert; the window closes at 15:00 (8.4) |
| Q19, Q22 | HARD_KILL never tried; flatten UNKNOWN | Record: never fired by 03-Aug; intraday flattened, CNC spared with a same-symbol defect (8.2) |
| Q20 | Orphan cancel; 14:45; capital drift (unsure); check the rest | Full inventory (8.3, 8.4) |
| Q21 | Positions protected under SOFT_KILL | Consistent (8.1) |
| Q23 | Hard kill needs a manual resume; only soft kill resumes next day | Conflict: since 20-Jun both clear at the next start; manual resume only for a same-day restart (6) |
| Q24 | Soft-kill alerts on Telegram and email | Consistent for emergency kills; scheduled kills at WARNING since 26-Jul (8.1) |
| Q25 to Q27 | Day timeline; refusal with open delivery; next-day collision | Consistent with the gate before 25-Aug and the records; times differ (9, 10) |
| Q28 | Alive after close | Consistent; the reconcile and report jobs are cron (9) |
| Q29 | UNKNOWN | Reception continued while degraded (12) |
| Q30 | No webhook-stop mechanism; NOTE | Consistent; F2 dropped; NOTE (12; V2 inputs 3) |
| Q31, Q32 | Queued signals dropped (guess); NOTE on Q31 | Rejected with a status on stop or kill; lost from memory on a crash; NOTE (12; V2 inputs 4) |
| Q33, Q34 | No auth failure; auth failure stops entries | Consistent (13) |
| Q35 | No token, no trades; management not needed | Exits are allowed but need a token; orders already resting at the broker may remain, but monitoring, reconciliation and new orders need broker access (13) |
| Q36 | Headless worked; "Manual not required in new system" | V2 input (13; V2 inputs 10) |
| Q37 | Boot, then cron | Consistent (5) |
| Q38, Q39 | UNKNOWN | Persisted and rehydrated state listed (14) |
| Q40 | No duplicates (guess) | Safeguards listed; incidents UNKNOWN (14) |
| Q41 | UNKNOWN; NOTE | Reconcile before entries, with an assertion and a second pass; NOTE (14; V2 inputs 5) |
| Q42 | Partially | Consistent; status sets per table (15) |
| Q43 to Q46 | UNKNOWN; NOTE | Answered at code level; NOTE (15; V2 inputs 6) |
| Q47, Q48 | Intraday and delivery chains (guess) | Consistent (16) |
| Q49, Q50 | GTT moves responsibility to the broker | Consistent; since 25-Aug no longer holds the process (16) |
| Q51, Q52 | No distinct EOD state; entries blocked during square-off | EOD log states and the scheduled SOFT_KILL; consistent on blocking (9, 10) |
| Q53 | Broker confirmation (guess) | The EOD unit does not wait for fills; the MIS unit verifies by 15:08 (17) |
| Q54 | One second check; "nothing is handled"; NOTE | The process stays up and acts; the 03-Sep defect; NOTE (17; V2 inputs 9) |
| Q55 | P&L reset and reports | Consistent (17) |
| Q56 to Q60 | Periodic; alert only (guess); drift and quantity alerts | Periodic, consistent; the in-process reconciler repaired state, the cron jobs only alert (18) |
| Q61 to Q64 | UNKNOWN; systemd restart at 08:15 | Code paths; the 08:15 start is the watcher's (7, 14) |
| Q65 | No same-day restarts; NOTE | Records list incidents; NOTE (14; V2 inputs 12) |
| Q66 to Q69 | Stop at 17:30; open positions not safe | Consistent at 17:35; a stop leaves positions unmonitored (10, 11) |
| Q70 to Q72 | No start on holidays; cron independent | Consistent; 15 of 46 jobs not limited to market days (19) |
| Q73 to Q80 | Paper used; testing mirrors production | Legacy only; 41 of 46 lifecycle files identical (21) |
| Q81 to Q84 | Manual actions | Consistent; GUI read-only; CHECK1 and CHECK2 (20) |
| Q85 | UNKNOWN | Defects listed (14, 22) |
| Q86 | No case of "running but unable to trade" | Conflict with records: after a CNC carry before 25-Aug the service ran with yesterday's SOFT_KILL and refused all signals; on 23-Jun a false SOFT_KILL refused every later signal (10, 14) |
| Q87 | No | Cron continues by design (19) |
| Section 20 | The example lifecycle is "Correct behaviour" | Broadly; HALT and the kill clears are added (3) |

## 5. ChatGPT's 03-Oct set: corrections and additions
- Kill clearing: "conditionally true, not universal for every emergency same-day kill" is refined: every prior-day kill of any kind is cleared; only same-day scheduled kills with a flat book are cleared on a restart (manual 6).
- systemd restarts: "exit 1/2 restart; 0/3/4/5 not" omits exits 6 to 9 (restarted) and the watcher's restart of exit 5 (manual 7).
- Webhook order: validation follows the kill, capacity and window checks; shutdown, rate limit, scanner and EOD route added (manual 12).
- Clean-stop drain: drained signals are rejected before placement (REJECTED_SHUTDOWN) (manual 12).
- Capital drift: fund-manager drift escalates by thresholds; the reconciler's G3 drift does not (manual 8.3).
- Status lists split per table (manual 15).
- 14:45: identified as the pre-alert in source, not only "historical/uncertain" (manual 8.4).
- Token invalidation chain: has no inspected trigger (manual 13).
- Delivery and the process: the 25-Aug change and the operator records added (manual 10).
- The supplied report: its count corroborated by the registry; the date ambiguity added (manual 19).
- Section 28 moved to A-011_v2_design_inputs.md section 14 (W-1).
- "A-011 documentation is CLOSED / FROZEN": in this project the manual stays DRAFT until ChatGPT's value-added review (W-4).

## 6. Corrections to frozen manuals (accepted in ChatGPT's 09-Oct review; recorded in DR-026)
- A-003 erratum E-1 (A-003_manual.md section 6): the self-exit predicate and the interpretation about carried delivery holdings hold only before 25-Aug-2026; at the baseline a cleanly identified CNC carry does not count. The user-reported defect is kept and dated.
- A-006 erratum E-3 (A-006_manual.md section 10): "after 3 consecutive cycles" applies only to drift between Rs 250 and Rs 1,000; Rs 1,000 and Rs 2,500 act on one event; only fund-manager sources escalate.
- A-007 erratum E-2 (A-007_manual.md sections 7, 12 and 17): "a delivery position survives HARD_KILL" gets the same-symbol qualifier, as the 03-Aug record asked ("true except on a same-symbol mixed book").
- A-009 erratum E-1 (A-009_manual.md sections 5 and 21): "exit 5, no restart" holds for systemd only; the token watcher restarts an exit 5 up to three times per rolling hour, with one Telegram warning a day at its limit.
- A-010 erratum E-1 (A-010_manual.md section 8): the token rename after a BrokerAuthError has no identified trigger: the critical-failure callback's only caller is the live feed, whose reasons never contain "BrokerAuthError".
- Each original line stays visible, struck through, with an Erratum section in the manual. None changes another fact in its manual. ChatGPT accepted all five (review section 6); they are recorded in DR-026, and each manual's status line and Erratum section say so. Per the review, A-009 E-1 states the source-defined restart policy, not an observed frequency, and A-010 E-1 states that no trigger was identified in the inspected baseline path. The exactly-once check is in section 12.

## 7. Points for Rama
- Q-33 (same-symbol rule for the new system): ANSWERED by Rama on 09-Oct: (a) one trade per stock per day in any direction; once a stock is traded, no opposite-direction trade that day, across intraday and delivery. Recorded as PROPOSED in VP-12, ratified at C-005 / F-007 (no register row, as with Q-27 to Q-29); Q-33 closed. As put to Rama (the A-009, A-010 and A-011 answers do not settle it): (a) one trade per stock per day in any direction (no same-day reversal), as in your A-002, A-005 and A-007 answers; or (b) one trade per stock and direction per day (a long, then a short the same day is allowed), as in your A-008 answers. Both versions span intraday and delivery.
- For awareness, no action: your Q23 answer (a hard kill needed a manual resume) differs from the 20-Jun-2026 decision recorded in the code (every prior-day kill cleared at the next start). For the new system this is an open choice at I-002 (VP-61), not something to settle in the legacy record.
- For awareness, no action: your Q36 ("Manual not required in new system") and the A-010 safeguard list for M-005 (a manual login kept as a fallback) are both recorded; M-005 decides.
- For awareness, no action now: once A-011 is frozen, Section A is complete, and the items scheduled for the end of Section A come due: VP-01 and VP-02 (V2 purpose and scope; DR-013 records that they are ratified at the end of Section A), VP-08 (strategy count), VP-37 and VP-41 (product intent), and the proposals that list A-011 among their ratification points (VP-04, VP-19, VP-22, VP-47, VP-52, VP-54). Claude will set these out for your decision as a separate step after the freeze; A-011 itself freezes legacy fact only (W-1, W-6).

## 8. Reading method (process note)
- The production clone was blob-less with an explicit no-cone sparse list: 46 lifecycle files, 17 records and configuration files, then every non-test Python file (304 files in total), so that every kill call site could be listed with local tools. After each checkout a check confirmed that none of the blobs of .env.example, config/accounts.csv, config/accounts_multi_example.csv, config/security.yaml, config/reference_data/ or docs/web_claude/ was in the local object store.
- The testing snapshot was compared by object IDs from the tree listing, so identical files were never fetched; only five differing files were checked out, with the same check (no sensitive blob present).
- Code and records were read through the masking filter. History: only the commit dates of the two snapshot heads. systemd documentation was read from its repository's man-page sources (sparse checkout of three files).
- Process slip, corrected: one parsed extract of the cron registry was printed by a direct Python call instead of through the filter, and one line of working output showed an account ID. Nothing was written to any file, and the value is not repeated anywhere. Lesson recorded in V2_DESIGN_LOG (2026-10-09).
- Inputs (value-blind counts only): the three A-011 input files contain no credential-assignment, mailbox or IPv4 pattern; one client-ID-shaped token appears in the reconciliation, inside the Cron Daily file name (line 24). The files are immutable inputs in the private repository.

## 9. Cross-topic evidence found while verifying A-011
- Q-36 (d): systemd's documentation confirms that exits 6 and 9 are restarted and that the default start limit cannot stop a 10-second loop; the watcher adds an exit-5 loop; occurrence still UNKNOWN. (e): the order-monitor HARD_KILL after three auth failures is source-verified (main.py wires it to the API-failure callback) and the monitor stops polling afterwards; the 03-Aug record says no HARD_KILL had fired.
- Q-31 (e): the 03-Sep record shows the MIS passes at 15:07 and 15:10 on that day (the cut-off was 15:12 until 03-Sep), as main.py's comment says; 15:03 and 15:06 apply from the 03-Sep change.
- A-003 section 3: the open point "whether the code used the SOFT_KILL state for the cut-off" is settled: it did not (A-011 manual 8.4).
- A-001: the VM termination date stays UNKNOWN; the 01-Oct report date suggests the production VM ran later than A-001's estimate (Q-37 (a)).
- VP-11: legacy left no per-signal row for refusals at the webhook stage (A-011 manual 12).

## 10. V2 items (to V2_DESIGN_LOG, PROPOSED)
VP-57 to VP-65 added; VP-04, VP-11, VP-19, VP-22, VP-36 and VP-54 extended; Q-37 opened (A-011 audit leads); Q-33 put to Rama; Q-14 and Q-36 annotated; five lessons (call paths through callbacks and flags; every supervisor of the process; behaviour dated across a code change; masking of derived output; and, from ChatGPT's review, a mechanism's existence written so it does not read as a guarantee). At the freeze: Q-33 closed with Rama's answer (VP-12 extended); ChatGPT's cautions added to VP-57 to VP-65; each Q-37 lead mapped to its owning topic.

## 11. Next step
Done on 09-Oct: ChatGPT's review applied (section 12), Rama's Q-33 answer recorded, A-011 frozen and DR-026 recorded with the five accepted errata; the other topics' review notes (A-003, A-006, A-007, A-009, A-010) carry one line each. Section A is complete. Next: the end-of-Section-A decision step (section 7), presented to Rama separately and not frozen with this manual; then Section B.

## 12. ChatGPT's value-added review (09-Oct-2026): how each point was applied

| Review point | Applied |
|---|---|
| 1.1, 5, 8 (Q-33) | Put to Rama on 09-Oct; answered (a). Recorded as PROPOSED in VP-12, ratified at C-005 / F-007; Q-33 closed. No V2 rule is frozen by it. |
| 1.2, 3.1 (HARD_KILL semantics) | Manual 8.1 rewritten per call path, read in source: new entries blocked on every inspected entry path (webhook, signal processor, the order placer's last-mile check, risk engine); the TGT retry manager pauses under any kill; the flatten worker and the reconciler's emergency close call the broker adapter directly, and the adapter has no kill-switch check; the "exit" test is used only by the reconciler; the EOD and MIS square-off units do not gate their exits on the kill state; no flat book guaranteed. The final statement follows. |
| 1.3, 3.2 (final statement) | Replaced with the review's wording, adapted to say that the 03-Sep incident happened with the process running. |
| 1.4, 6, 7 (errata) | All five accepted and recorded in DR-026. Checked: one Erratum section per manual; each superseded line struck through and followed once by its replacement; status lines say "accepted ... recorded in DR-026"; A-009 E-1 states the source-defined policy, not an observed frequency; A-010 E-1 states that no trigger was identified in the inspected baseline path; A-007's sections 7, 12 and 17 point to A-011 section 8.2. |
| 1.5, 7 (freeze sequence) | A-011 frozen only after Rama's answer and the checks; V2 inputs remain PROPOSED / OPEN; DR-026 records the freeze and the accepted errata only; the end-of-Section-A items are a separate step. |
| 2 (inputs) | Rama's recollections kept as HISTORICAL; the 03-Oct manual untouched; code paths not presented as observed operation. |
| 3.3, 3.5, 3.6 | Kept as written (start-up clears; trigger inventory; the self-exit change dated 25-Aug). |
| 3.4 | Section 7's restart-rate reading labelled as the repository unit read with systemd's documented defaults, not an installed-VM result or an observed incident; section 22 item 4 aligned. |
| 3.7 | Section 12 states the three signal outcomes separately; "right in effect" replaced. |
| 3.8 | Section 13: valid authentication was needed for monitoring, reconciliation and new or replacement orders; resting orders may remain. |
| 3.9 | Section 14: in-flight outcomes per branch, read in source (adopt with a G5b recovery SL due and a TGT retry flagged; FAILED with release on confirmed absence or no fill; deferral while resting or unreachable; deferral with a CRITICAL alert on an ambiguous match; under HARD_KILL the emergency close, success not established). |
| 3.10, 3.11 | Signal statuses marked as an open set with status families; the EOD re-fire statement scoped to the inspected path. |
| 3.12 | Label SUPPLIED RECORD (INDIRECT); the VM termination date stays UNKNOWN. |
| 3.13 | The testing comparison is scoped to the 46 lifecycle files compared; the 18 configuration differences are leaf settings (A-009 wording unchanged). |
| 3.14 | No value or file name repeated. |
| 4 | Cautions added in the V2 inputs (sections 2 to 12 and 14) and in VP-57 to VP-65; authentication worded as Rama's preferences with M-005 as owner; the strategy-count wording marked as superseded. |
| 8 | Q23: no new question. Q36: carried to M-005. VM date: UNKNOWN. Q-37: kept open, each lead mapped to its owning topic in V2_DESIGN_LOG. |
| Added after the review | F-23 (in-flight recovery under HARD_KILL with no product filter), found while verifying point 3.1; code path only; recorded in manual 8.2, 14 and 22 (item 15) and as a V2 consideration (VP-61); no change to A-007. |
