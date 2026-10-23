# C-007 SIGNAL PERSISTENCE: V2 DESIGN INPUTS

Status: PROPOSED / OPEN (23-Oct-2026). Written with C-007_manual.md revision 2 (FROZEN, DR-060); it stays PROPOSED / OPEN after
the C-007 manual is frozen. Nothing here is a V2 decision (W-1, W-6); each item is a consideration for the whole-design review
that Rama scheduled for after the agenda (W-9), or for the owning topic named.
Sources: C-007_manual.md (legacy facts, sections cited per item); C-006_v2_design_inputs.md items 1, 3 and 5; V2_DESIGN_LOG VP-11,
VP-59, Q-37 and bookmarks BK-19 to BK-30.
C-006_v2_design_inputs.md items 1, 3 and 5 name C-007 among their owners. C-007 adds their legacy facts at the level of the
stored row (C-006 item 1: item 3 below; C-006 item 3: items 2 and 9; C-006 item 5: bookmark C7/BM-03, BK-21) and decides none
of them.

| # | Legacy fact (C-007_manual.md) | Consideration for V2 (PROPOSED / OPEN) | Owners |
|---|---|---|---|
| 1 | The status CHECK is an open set (20 values and five GLOB families); ten enumerated values and the DROPPED_* family have no writer in the inspected scope (sections 2.2, 5.3; BK-28) | One documented status vocabulary with one owner, shared by the database constraint, the code and the tests | K-003; whole-design review |
| 2 | No status write checks the row's current status, except the cleanup's expiry, and no status-change time is stored (sections 4.1, 9.2; BK-19) | Guarded transitions (a write names the status it expects to replace) and a recorded time and reason for every change, from one transition table | K-003, C-008; read with C-006_v2_design_inputs.md item 3 |
| 3 | After a crash, non-terminal rows are neither repaired nor pruned by the inspected paths, except earlier-dated PROCESSING rows, which the cleanup expires; Rama reports no hand clean-up (sections 6.3, 9.2; BK-25) | A defined closure of non-terminal rows at start (with a reason) or a durable hand-off, as VP-59 proposes | K-003, K-012, B-012, I-009; VP-59; read with C-006_v2_design_inputs.md item 1 |
| 4 | Refused symbols leave no signals row, while the table's comment says every signal has a final status (section 9.1, D-01) | Whether every received signal is recorded with its outcome, including refusals at the webhook stage (VP-11, VP-59) | C-008, L-009; VP-11, VP-59 |
| 5 | Retention (the cleanup's step 4) deletes only EXPIRED, DUPLICATE and REJECTED* rows older than the window (90 days in the repository configuration) that have no trades row; no inspected path deletes rows of the other statuses, final ones included; the step is named for fingerprints but deletes whole rows and their children (section 7; K-03, D-05) | One retention policy over every final status, keeping the trade-linked guard, with a validated window and a name that says what it deletes | K-011 |
| 6 | The readers read classify statuses each in their own way (the status report, the dashboard, the system manager, the cleanup's prune, the crash harness); no single definition of a terminal status was found, and they disagree on QUEUE_FULL, DUPLICATE and EXPIRED (section 8, D-06; BK-26) | One shared classification (terminal, rejected, accepted and so on) used by every reader and test | L-010, L-011; K-003 |
| 7 | The strategy column stores the scanner name (section 2.1; C-001 D-05; BK-27) | Separate, correct scanner and strategy identities in the stored row (DR-006 traceability) | C-010, K-003 |
| 8 | expires_at is stored from received_at while both age checks measure from triggered_at (section 9.2, K-04; BK-23) | One expiry basis, stored as the value the checks apply | C-009 |
| 9 | A status and its companion row (screener verdict and screener_results; gate_state and GATE_WAITING; retest_state and RETEST_WAITING) are written by two separate calls whose failures are only logged (section 4.2) | A status change and its companion rows in one transaction | K-002, C-008; read with C-006_v2_design_inputs.md item 3 |
| 10 | signals.trade_id is set before the broker execution while the schema's comment says post-fill (section 9.1, D-08) | When the signal-to-trade link is written, and what a link without a fill means | G topics, K-005 |
| 11 | The crash harness's status model does not match the pinned writers (section 9.1, D-07; BK-29) | Tests and harnesses read the same transition table as the code (item 2) | N-005 |
| 12 | Status names are composed from check names; one path would store a doubled prefix (section 5.2; BK-22) | Status names built in one place, with the rejecting check kept as a separate field | C-008 |

PROPOSED / OPEN. C-007 decides nothing for V2.
