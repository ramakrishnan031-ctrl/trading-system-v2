# B-003 DATABASE / STATE INITIALIZATION: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, revision 2 (2026-10-14; revision 1 2026-10-14; first pass 2026-10-13). Nothing in this file is a V2 decision or an approved requirement. ChatGPT's value-added review of 14-Oct-2026 kept this boundary (its point 8) and is applied to items 3 and 8 only; its value-added final review of the same day kept it too (its point 6) and is applied to item 2 and the wording of section 3 only. Item 2's list of conditions was completed after ChatGPT's acceptance check of revision 2. At the B-003 freeze (14-Oct-2026, DR-039) two scheduling moves were recorded and applied (section 3); no item changed state. Items become decisions only as rows in V2_DECISION_REGISTER.md, at the topic that owns them (V2_DESIGN_LOG W-6). A ChatGPT review alone never turns an item here into a decision.
Date: 2026-10-13 (first pass); 2026-10-14 (revisions 1 and 2)
Place at: docs/agenda/b-003_database-state-initialization/B-003_v2_design_inputs.md
Why this file exists: Rama's B-003 answers contain no request for the new system (his note on the sheet asks only that the answers be treated as raw input, line 776). The legacy manual records legacy fact only (W-1), so Claude's considerations drawn from the B-003 findings are carried here to the topics that own them, as B-002_v2_design_inputs.md section 3 did. No VP row is opened by this file; three existing rows are annotated (section 3).
Sources: B-003_manual.md (legacy facts, sections cited per item); Rama's answer sheet (docs/inputs/chatgpt_originals_and_replies/B-003_DATABASE_STATE_INITIALIZATION_ANSWER_SHEET_13-OCT-2026.txt); ChatGPT's 13-Oct reconciliation and manual draft; the master agenda's owners (K-001 database architecture, K-002 StateStore, K-011 retention, K-012 recovery from the database, K-013 data consistency; I-005 database failure; L-003 critical alerts; M-003 deployment, M-008 backup, M-009 restore, M-010 runbooks; N-001 unit testing, N-005 failure testing).
Labels: OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision and not a requirement. LEGACY = what the old system did, for comparison only, not a requirement.
Security: no credential values, account IDs, mailbox addresses, host addresses or host names.

## 1. Order of intent (latest last)
- A-001: VP-07, backup and restore design, with the rule that no machine is torn down without a verified backup (lesson 2026-10-05).
- A-003 and B-001: VP-23, time discipline, including a defined timezone for stored timestamps.
- A-011: VP-57, one explicit state model; VP-65, lifecycle evidence for every start, stop, crash, kill and clear.
- B-001 and B-002: VP-67, start prerequisites and the fail-closed start ("Do not begin trading initialization; retain the failure reason and report it through the approved alerts/evidence path"); at the B-002 freeze its range became B-003 to B-007.
- B-003 (this file): no new request from Rama; considerations below.

## 2. Legacy findings carried to their owners (CLAUDE CONSIDERATIONS, not decisions)

| # | LEGACY finding (B-003_manual.md) | CLAUDE CONSIDERATION for discussion | Owner |
|---|---|---|---|
| 1 | Schema migrations ran as a side effect of opening the database: before 14-Jul by whichever process opened it first with newer code, afterwards only in main.py's start outside market hours, with every other process refusing until then (a refuse window, measured once on 27/28-Jul) (sections 6.3, 6.4) | Whether schema changes are applied by an explicit step of their own (with a window, a pre-change backup, a result record and an alert), separate from opening the database; and what other processes do while a change is pending (refuse, wait or run without the changed tables). The legacy left this choice open (its "M1", two directions, neither chosen) | K-002, M-003 |
| 2 | An existing database with no schema_meta table, no version row, a version value that was not an integer, or another OperationalError at the version read was treated as a new database, skipping the version checks, the migration gate and the rebuilds (sections 4.2 and 8, D12; section 17, item 1) | Distinguish "no database", "existing database without version metadata" (no schema_meta table or no version row) and "version unreadable", and fail closed on the last two | K-002, I-005 |
| 3 | A migration run was atomic per table, not as a whole; the repository test covered a failure in the first table only; and the analytics relocation ran one transaction over two WAL files, which SQLite documents as atomic per file but not across the set, a source-backed limitation of the legacy code path with no historical incident established (sections 6.2, 14, 17) | Define the unit of atomicity (a whole version step, or a resumable run with recorded progress), test a failure at a later step, and decide what check would detect and resolve a partial cross-file change; any V2 remedy is undecided | K-001, K-002, K-013, N-001, N-005 |
| 4 | A missing database was created empty and the start continued (section 8, D7) | Whether a start requires an existing, version-checked database, with first-time creation as an explicit, separate action (links VP-67's prerequisites) | K-002, B-007 |
| 5 | The database path was fixed in code, relative to the working directory, and the same for paper and live (section 3) | One configured, absolute location per environment, checked at the start (links the configuration design step and VP-15) | K-001 |
| 6 | Every state-store opener wrote at least the version row; direct openers set their own connection settings and left foreign keys off (sections 9, 10) | One connection policy for every process: read-only openers truly read-only, and the same integrity settings on every writing connection | K-001, K-002, K-013 |
| 7 | --status and --dry-run passed through the migrating, writing open (section 7) | Diagnostic and status commands that read only (links VP-57) | B-007 |
| 8 | No notifier alert was sent at the database step: the failures were logged CRITICAL, and a migration refusal also wrote its own CRITICAL line and a best-effort sentinel (section 8) | Every database-stage failure reported through the approved alert path, as VP-67 already proposes for failed prerequisites | I-005, L-003 |
| 9 | At shutdown only the main thread's connection was closed; the checkpoint statistics described the main file only (section 9) | Carried to B-011 (connection closing and checkpoint evidence) | B-011 |
| 10 | The kill state started INACTIVE when it could not be read (section 17, item 4) | An unreadable kill state as a failed prerequisite, not as "no kill" (VP-67 lists "no unresolved HALT or kill condition from the previous run") | B-007, I-005 |
| 11 | Backups were daily online copies on the same disk, with no automated off-machine copy, one manual off-host copy recorded, no drill result in either repository, and a plain-copy step in the runbook (section 12) | Carried to VP-07: off-machine copies, a verified restore with its evidence, and one documented backup and restore procedure | M-008, M-009, M-010 |
| 12 | A scheduled job deleted old rows from six tables and vacuumed both files weekly (section 11) | Retention as a recorded policy per table, with what may never be deleted | K-011 |
| 13 | Production and testing differed by host only; paper and live shared one file (section 3) | Paper is out of scope (DR-001); environment separation for V2 stays with the open question on V2's own environments (project instructions section 12, item e) | K-001, M-007 |

## 3. VP rows annotated in V2_DESIGN_LOG (no ratification point moved)
- VP-07: the legacy backup facts of B-003_manual.md section 12.
- VP-57: the diagnostic modes' passage through the migrating, writing open (section 7).
- VP-67: the database step's legacy behaviour (a missing database created, migration in the start, exit codes, no notifier alert but CRITICAL log lines and the refusal's sentinel) (sections 3, 6, 8).
No scheduling move was made before the freeze. At the B-003 freeze (14-Oct-2026, DR-039, scheduling only): VP-57's range becomes O-004, B-004 to B-012, L-005, and VP-67's B-004 to B-007, B-012, M-006, I-010, L-003; no proposal content changes.

## 4. Status and boundary
PROPOSED / OPEN. B-003 decides nothing for V2. Each consideration is discussed and, if adopted, decided at its owner. B-005 owns reconciliation, B-006 workers, B-007 readiness, B-011 shutdown and B-012 recovery.
