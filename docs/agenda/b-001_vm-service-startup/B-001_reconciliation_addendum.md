# B-001 VM AND SERVICE STARTUP: RECONCILIATION ADDENDUM (VERSION 2)

Status: FROZEN with the B-001 legacy manual (DR-035), 2026-10-10. Version 2 of the B-001 answer-to-source reconciliation. Version 1, ChatGPT's docs/inputs/reconciliation/B-001_RECONCILIATION_ANSWERS_VS_GITHUB_09-OCT-2026.txt, stays unchanged (docs/inputs/ is immutable; DR-008). This addendum supersedes version 1 where they differ, because version 1 was written before the baseline's dated records were read (ChatGPT's 10-Oct review, points 1 and 4).
Date: 2026-10-10
Place at: docs/agenda/b-001_vm-service-startup/B-001_reconciliation_addendum.md
Method: each statement of version 1 is marked KEPT, REFINED (kept, with a qualifier or more precision) or CORRECTED (superseded), with the evidence and where it is recorded. Anything in version 1 not listed here is KEPT.
Labels: as in B-001_manual.md (SOURCE-VERIFIED, RECORD-DERIVED, TESTING CAPTURE, SUPPLIED RECORD, HISTORICAL (Rama), INTERPRETATION / AUDIT LEAD, UNKNOWN / NOT ESTABLISHED, KNOWN DEFECT); anything about V2 is PROPOSED or a CLAUDE CONSIDERATION and sits in B-001_v2_design_inputs.md. Terms as in B-001_manual.md: HOST BOOT, SERVICE START, DAILY START, APPLICATION INITIALISATION, READINESS.
Security: no credential values, account IDs, mailbox addresses, host addresses or host names.

## 1. Evidence boundary (version 1, section 1)
- KEPT: the label set; "The production repository is a code reference, not proof of what was installed, enabled, running, or executed on the historical production VM"; "Do not turn repository configuration into a claim about historical host execution"; no implementation, host inspection or operation is authorised.
- REFINED: version 1 notes that "production was not directly read". The production repository also holds the baseline's own dated records (the system map, audit notes, dated code comments), which record host states and events on their dates (RECORD-DERIVED). B-001 read them. The VM itself is deleted, so its final state still cannot be read (B-001_manual.md section 2).
- Added: TESTING CAPTURE as its own label (the testing VM only), and SUPPLIED RECORD.

## 2. Executive reconciliation (version 1, section 2)

| # | Version 1 | Status | Version 2 and where recorded |
|---|---|---|---|
| 1 | Host uptime and trading-process uptime are different questions | KEPT | Design inputs sections 3 and 6 |
| 2 | Two start windows: "main.py: 08:00–18:15 IST; token_watcher.sh: 08:00–16:00 IST" | REFINED | main.py [08:00, 18:15) IST since 25-Jul ([08:00, 16:00) from 19-Jun); the watcher hours 08 to 15, that is 08:00:00 to 15:59:59, since 19-Jun; never merged (manual sections 7.2, 8) |
| 3 | Two supervision layers; "Their actual historical enabled/running states are unproven" | CORRECTED in part | RECORD-DERIVED: the main unit "enabled/loaded" on 19-Jul; the token watcher "active" after the 03-Jul host reboot (so enabled then) and starting the service on 05-Aug and 26-Aug. Still unproven: the states at the end and on undocumented dates (manual sections 4, 9) |
| 4 | "No inspected canonical cron entry directly starts trading-system.service" | REFINED | KEPT for direct starts. Added: the 08:30 pre-flight can restart token-watcher.service and re-run the token refresh, and either can lead to a later start by the watcher (manual section 6, item 6) |
| 5 | A start request or an "active" state does not prove initialisation or readiness | KEPT | The three levels kept apart (manual section 7) |
| 6 | The unit's `--mode live` does not prove the sandbox traded live | KEPT | Added: the testing manifest shows the service ran on 17-Sep (inactive after its own 17:35:04 shutdown); broker reach and orders stay NOT ESTABLISHED (manual section 13) |
| 7 | Automatic start at VM boot stays recollection; "GitHub cannot settle whether the relevant unit was enabled and running at that time" | CORRECTED | Too broad (ChatGPT's 10-Oct review, point 1): dated records establish the 19-Jul enablement and the watcher's daily starts on recorded days, with no host boot recorded those mornings. Rama's recollection stays HISTORICAL with his stated uncertainty; what "booted" meant to him is not established (the records call the daily start a "boot") |
| 8 | The daily process model is deferred | KEPT | DEFERRED until B-008, B-011, B-012, M-001 and O-010 (VP-62) |
| 9 | Host security ideas are raw input, not approved controls | KEPT | What each control must specify: design inputs section 8 |
| 10 | No implementation, host inspection, deployment or live operation is authorised | KEPT | |

## 3. Source findings (version 1, section 3)
- 3.1 Main service unit: KEPT. REFINED: the directive list left out the [Unit] section (After= and Wants=network-online.target), the PATH and PYTHONUNBUFFERED environment, journal output and the hardening lines, PrivateTmp=true among them (manual section 5). The installer enables the unit for host boot ("start on boot") while its header says the token watcher starts the service (manual section 4). "It does not prove that it was enabled on the historical VM" is superseded for 19-Jul by the "enabled/loaded" record and stands for every other date.
- 3.2 Token watcher: KEPT. REFINED: its window is hours 08 to 15 (08:00:00 to 15:59:59). "Actual enablement, running status, event history, and historical start causality on production are NOT ESTABLISHED" is CORRECTED for the recorded dates: active after the 03-Jul host reboot; starts on 05-Aug (a trace naming the watcher) and 26-Aug (the watcher's own log lines). 17-Jul fits the same chain, with the starter not named in its record.
- 3.3 Start-window discrepancy: KEPT ("Do not merge these into one claimed legacy window"). The cause is dated: on 25-Jul main.py's cutoff moved to 18:15 and the watcher kept its window, with a comment that still says it "Mirrors the main.py guard".
- 3.4 Cron: KEPT, with the refinement in section 2, item 4.
- 3.5 Guarded clean exits: KEPT. Added: the watcher reads an exit 0 dated today as the completed session, so a trading-day host boot before 08:00 that started the service would have lost the day. KNOWN DEFECT — CONDITIONAL CODE PATH / LATENT; NO OBSERVED OCCURRENCE FOUND (manual section 10, with its six conditions).

## 4. Question by question (version 1, section 4)

| Question | Version 1 status | Version 2 |
|---|---|---|
| Q-B001-01 | CAPTURED; NOT FROZEN | KEPT. V2 input, PROPOSED (design inputs section 3; VP-05) |
| Q-B001-02 | CAPTURED; DISCUSSION INPUT | KEPT. Rama's preference, not an approved architecture; its open questions are listed in design inputs section 3 |
| Q-B001-03 | CAPTURED; NEEDS BOUNDARY CLARIFICATION; "the source alone cannot establish which mechanism actually initiated historical production starts" | REFINED. Source plus records: the watcher started the service on 05-Aug and 26-Aug (17-Jul fits; starter not named); host-boot activation existed (the unit enabled on 19-Jul), with no recorded occurrence. The boundary is PROPOSED in VP-66, with the host-boot trade-off left open (design inputs section 4) |
| Q-B001-04 | CAPTURED; FAILURE HANDLING DEFERRED | KEPT. Four candidate categories (hard gate, restricted recovery gate, advisory check, post-start readiness gate) listed for the owners (design inputs section 5) |
| Q-B001-05 | OPEN / DEFERRED | KEPT. Rama's request for the best option is answered as a CLAUDE CONSIDERATION, not a decision (design inputs section 6) |
| Q-B001-06 | CAPTURED; DETAILS DEFERRED | KEPT (VP-64) |
| Q-B001-07 | CAPTURED; HISTORICAL ENABLEMENT UNKNOWN | REFINED. Enablement is recorded for the main unit (19-Jul) and, through the 03-Jul host reboot, for the token watcher; the watchman's enablement is NOT ESTABLISHED. The watchman is in the production repository and was installed on the production VM; the main unit's Wants= drop-in started it with the service (03-Jul record; A-010 section 5) |
| Q-B001-08 | PARTIALLY SOURCE-CORROBORATED; WINDOW/RECORDING DETAIL OPEN | KEPT. The recording detail: the guard's reason reaches the journal unbuffered, with no application log line and no alert (on a holiday, one Telegram a day); the journal's retention on the VM is NOT ESTABLISHED (manual sections 5, 7) |
| Q-B001-09 | UNKNOWN / NOT ESTABLISHED AS A VERIFIED HISTORICAL FACT | KEPT for his recollection; CORRECTED for the mechanism on the recorded dates (section 2, item 7) |
| Q-B001-10 | UNKNOWN / NOT ESTABLISHED | KEPT; the 17-Sep run added (section 2, item 6) |
| Q-B001-11 | CAPTURED; IMPLEMENTATION MAPPING DEFERRED | KEPT (VP-67) |
| Q-B001-12 | OPEN FOR EVIDENCE; NO EVENT CLAIM ADDED | REFINED. Dated records cover several items (manual section 13): the 18-Jun overnight start conflicts with "started outside the intended time window: NO"; the A-011 records (06-Aug, 23-Jun) conflict with "remained active but could not accept trades: NO"; the 17-Jul self-shutdown relates to the start-failure item. UNKNOWN answers stay UNKNOWN. Item 7 was clarified by Rama on 10-Oct-2026: option A, "worked as intended" |
| Q-B001-13 | CAPTURED; APPROVAL AND SCOPE OPEN | KEPT (design inputs section 8; VP-53) |
| Q-B001-14 | OPEN | KEPT; ten subjects proposed with owners (design inputs section 9) |

## 5. Open questions (version 1, section 5)

| # | Version 1 question | Version 2 |
|---|---|---|
| 1 | Was trading-system.service enabled on the historical production host, and during which dates? | The installer enables it; "enabled/loaded" on 19-Jul (record). Other dates and the end: NOT ESTABLISHED (Q-38 (a)) |
| 2 | Was token-watcher.service enabled and running on that host? | Active after the 03-Jul host reboot, so enabled then; running on 05-Aug and 26-Aug (records). The end: NOT ESTABLISHED |
| 3 | Which mechanism originated historical starts? | The watcher on 05-Aug and 26-Aug; 17-Jul fits, starter not named; the watcher at 23:22 on 18-Jun; no host-boot start recorded |
| 4 | Which versions of main.py, the units and the watcher were installed when? | Partly recorded: the installed units on 03-Jul, 05-Jul and 28-Jul (manual section 9, declared against observed); the end NOT ESTABLISHED (Q-35 (a), Q-38 (b)) |
| 5 | How did the 08:00–18:15 main guard and the watcher's guard interact on the host? | Answered at code level: the watcher starts the service only from 08:00:00 to 15:59:59; main.py accepts a start from any initiator in [08:00, 18:15); the host-boot timeline follows (manual section 10). Not observed on a host |
| 6 | Did any start occur before the daily token refresh? | 18-Jun at 23:22, on a leftover evening token (a recorded incident); no recorded start between 08:00 and the refresh; the code path for a host boot then (an exit-6 loop) is in manual section 10 |
| 7 | What does "no trading day = no system booting/starting" mean: no VM boot, no main process start, or no trading initialisation? | Clarified by Rama on 10-Oct-2026: option A, "worked as intended": it never started on a non-trading day, and never started a second time after a clean end-of-day exit. Which of the three levels "started" refers to was not asked; the source facts sit beside his answer in manual section 13 (until 17-Jul a weekday holiday could bring a refused service start) |
| 8 | Restart classes, limits and manual-resume rules | DEFERRED to B-012 and M-001 (VP-64) |
| 9 | Alerting, access prevention, or both? | Candidate prevention and detection listed per item; nothing decided (design inputs section 8) |
| 10 | The evidence path for reasons emitted before normal logging | The journal, unbuffered (PYTHONUNBUFFERED=1; StandardOutput=journal); retention NOT ESTABLISHED (journald's storage default is set when systemd is built); the V2 consideration is in design inputs section 4, point 5 |

## 6. Status (version 1, section 6)
- Version 1's "B-001: OPEN — NOT CLOSED / NOT FROZEN" is superseded: after ChatGPT's 10-Oct review, Rama's clarification and his approval on 10-Oct-2026, the legacy manual is FROZEN (revision 1, DR-035). The V2 inputs stay PROPOSED / OPEN, as version 1 asked ("Do not close B-001 by treating every questionnaire answer as an approved decision").
- Version 1's "Unknown / not established" list, updated: production service enablement, recorded on 19-Jul and unknown at the end; boot-start causality, no host-boot start recorded, with the watcher's daily starts recorded; token-watcher enablement and running state, recorded on 03-Jul, 05-Aug and 26-Aug and unknown at the end; sandbox live trading, NOT ESTABLISHED; production runtime configuration and event history, partly recorded (manual sections 9 and 13); the interaction of the two windows, answered at code level and not observed.

## 7. Cross-references
B-001_manual.md (FROZEN, DR-035); B-001_v2_design_inputs.md (PROPOSED / OPEN); B-001_review_notes.md (CLOSED); ChatGPT's 10-Oct review (CLAUDE_VALUE_ADDED_REVIEW_B-001_FINALISATION_10-OCT-2026.txt, in this folder); V2_DESIGN_LOG Q-38.
