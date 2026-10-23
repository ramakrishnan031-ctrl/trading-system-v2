# C-007 SIGNAL PERSISTENCE: REVIEW NOTES

Status: CLOSED 2026-10-23. C-007_manual.md revision 2 is FROZEN (DR-060).
Topic: C-007 Signal persistence (master agenda, section C); ChatGPT's working title: Persistence, status transitions, database
schema and retention.

## 1. Course of the topic (23-Oct-2026, IST)

- 18:06: C-006 pushed by Rama (642fd76); the C-007 base.
- 18:09: ChatGPT's Stage 0 research prompt and answer sheet (inventory and planning only).
- 18:44: Claude's Stage 0 answers and source inventory: 49 register rows (95 files), 39 count-only searches, a proposed scope
  S-1; the read gate locked, so that no source content could be opened at Stage 0.
- 18:48: ChatGPT's acceptance of Stage 0 and Stage 1 authorisation (C-007_STAGE1_AUTHORIZATION_AND_REVIEW_NOTES_23-OCT-2026.txt,
  in this folder): S-1 approved and bounded; records R43 to R47 names only; the two July records excluded.
- 18:49: the Stage 0 lock lifted as a step of its own (the tool's SHA-256 recorded before and after); the gate test passed.
- 18:49:48 to 19:01:47: 120 gated reads of 37 files at the pin and 8 count-only searches, each after the gate's self-test;
  19:34: the Stage 1 repository answers and source inventory stamped and delivered.
- 19:49: ChatGPT's reconciliation review (items A to E: four process deviations and the independent-check question).
- 19:49 to 19:55: Claude's reconciliation table (items A to F and OQ-02); the cron evidence copy redacted at 19:53.
- 20:07: ChatGPT's spot-check of 13 claims at the pin and its review of the table.
- About 20:10: Rama's answers on a question card: OQ-02 "No, never" (no hand clean-up of non-terminal signals rows); the transcript
  residual of item A "Accept as is"; drafting "Yes, draft now".
- 20:20: Claude's manual draft, revision 0.
- 20:30: ChatGPT's review notes on revision 0 (two required wording corrections, one optional clarification).
- 20:32: revision 1.
- 20:38: ChatGPT's final review note on revision 1 (one count clarification).
- 20:39: revision 2.
- 20:45: ChatGPT's review of revision 2 ("CHATGPT REVIEW ACCEPTABLE; AWAIT RAMA'S EXPLICIT APPROVAL") and Rama's approval
  ("Approved").
- After 20:45: the freeze files and the push commands (sections 7 and 8), under W-4 (DR-017) and W-9.

## 2. From the delivered drafts to the frozen manual

- Revision 0 (Claude's draft) carried the Stage 1 answers' facts unchanged and applied the reconciliation (the table's items A to
  F): the process deviations and Rama's answers in section 1.2; KNOWN DEFECT only for prior defects re-verified (C-001 D-05,
  B-009 D1); K-01 and K-02 as source facts plus a separately labelled interpretation; FACT (TEST TEXT) for the crash harness and
  a test docstring; the narrowed description of scripts/t2_cnc_gtt_realtest.py; "the inspected code path would ..." wording;
  the absence claim of section 5.3 bounded.
- Revision 1 (ChatGPT's review notes on revision 0): the main.py role in section 1.1 bounded to its logged search, and the same
  fix for capital/kill_switch.py; section 6.3's crash-residue sentence names the non-PROCESSING residues and the exception for
  earlier-dated PROCESSING rows, with K-02 aligned; section 4.2's transaction statement scoped to later status-update calls.
- Revision 2 (ChatGPT's final review note on revision 1): section 1.2 explains the two citation counts: 415 cited line ranges in
  the Stage 1 answers and 454 path:lines ranges in the manual, both checked only for having been printed by a logged read.
- Freeze edits (no statement changed): the status line; the inputs, with ChatGPT's review of revision 2 and the three drafts
  added and where each is stored; the pointer to the V2 inputs file; the approval line of section 1.2; the freeze status;
  section 15.4.
- Checks of the frozen file: its 454 path:lines ranges were each printed by a logged read at the pinned blob; every repository
  path it names is in the pinned listing; its hexadecimal IDs are pinned blobs, the pin or the SHA-256 of a stored input; ASCII
  only; no session link, absolute path or credential pattern; the value-blind identifier scan finds only long hexadecimal
  tokens (the blob IDs and hashes).

## 3. Responses to ChatGPT's reviews

- Reconciliation review: answered by the reconciliation table (stored in docs/inputs/chatgpt_originals_and_replies/). Items A to D
  closed; E closed by ChatGPT's spot-check, with the limit on Q4-a kept; F applied in revision 0. OQ-02 asked once and answered
  by Rama.
- Spot-check review: accepted as recorded in the manual's section 1.2 (13 claims: 11 verified; S-02/S-03 partly verified, the
  queue-full range not re-fetched; the absence claim Q4-a not independently verified and kept bounded). Its note on "D-04's
  script description" concerns the description of scripts/t2_cnc_gtt_realtest.py: claim Q3-g of the Stage 1 answers,
  corrected under item C of the reconciliation table (deviation D-3); D-04 of the answers is the UUID4 comment. The corrected
  wording is in the manual's section 4.1.
- Review notes on revision 0: the two required corrections and the optional clarification applied. The same wording fix was
  extended to capital/kill_switch.py, whose role had the same contradiction; ChatGPT's final review note on revision 1 recorded
  it as present. K-02 was aligned with section 6.3 because it restates the same point.
- Final review note on revision 1: the count clarification applied; nothing else changed.
- Review of revision 2: no further correction; its review limit stands (it checked the affected passages and did not reproduce
  the 454 citation checks).

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- D-1: read 083, one gated grep of deploy/cron/trading-system.cron, printed three job lines whose commands hold the deployed
  project directory. The Stage 1 files called this "working output only"; the gate had also kept the output as an evidence file
  in Claude's private scratch area, which was redacted at 19:53 (an append-only correction record keeps its SHA-256 before and
  after; the read log is unchanged). The conversation's own copy cannot be edited by Claude; Rama accepted it as is. No file of
  this package holds an absolute path, a home-relative path or a cron job line (value-blind counts on the 21 files).
- D-2 and D-3: three files outside the Stage 0 register and three Stage 0 NO rows were read narrowly, each for one S-1 question
  (the reconciliation table, items B and C); the Stage 0 record is stored as delivered, and its R29 reason is corrected in the
  Stage 1 inventory, section 5.
- D-4: six non-test follow-up reads came after the test reads; no source claim depends on a test.
- Before the Stage 1 delivery, a check of every claim against the saved evidence corrected four statements of Claude's draft (13
  columns to 14; B-009's D1 is in its section 15; three cron job lines, not two; the R29 reason).
- The citation checker missed citations wrapped onto a continuation line; it now joins them. Re-run on C-006's Stage 1 answers:
  287 citations (284 before), none uncovered.
- No independent agent check was run (Rama's lean workflow); ChatGPT's spot-check is the only independent source check.

## 5. Errata to frozen manuals

None. The frozen statements re-verified at the pin are consistent (C-007_manual.md section 11.1); where C-007 adds detail (the
run-date condition of the expiry step under C-006 section 8.3; every status writer unconditional, extending C-006 sections 5.2
and 7), it is recorded as a re-verification, not a correction.

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- VP-59 lists C-006 to C-009 and I-009 as its ratification points; C-007's facts on refusals, residues and unconditional writes
  are noted on it. VP-59 is not ratified at C-007: it stays with the whole-design review (bookmark BK-18).
- Q-37 (f) is annotated with Rama's statement; which rows crashes left non-terminal stays open.
- BK-04, BK-05, BK-10, BK-11 and BK-14 are annotated with C-007's re-verification and stay OPEN.
- The twelve bookmarks C7/BM-01 to C7/BM-12 are carried into the design log as BK-19 to BK-30, unresolved.
- V2 considerations from this topic: C-007_v2_design_inputs.md (PROPOSED / OPEN).
- Design log: three lessons (section 1), the VP-59, Q-37 and bookmark notes, the agenda status row, session notes (with the owed
  record of the C-006 push) and bookmarks BK-19 to BK-30.

## 7. Freeze record

Prepared under W-4 (DR-017): ChatGPT reviewed the topic at each stage (its Stage 1 authorisation, its reconciliation review, its
spot-check review, its review notes on revision 0, its final review note on revision 1 and its review of revision 2) and its
points are applied; Rama answered the open question, accepted the transcript residual and approved revision 2 (20:45 IST). The
freeze takes effect when Rama declares the package final and runs PART 2 of the push commands
(C-007_PUSH_COMMANDS_23-OCT-2026.txt). Claude stores, commits and pushes nothing.

## 8. Package

1. docs/agenda/c-007_signal-persistence/C-007_manual.md (new; FROZEN, revision 2)
2. docs/agenda/c-007_signal-persistence/C-007_review_notes.md (new; this file)
3. docs/agenda/c-007_signal-persistence/C-007_v2_design_inputs.md (new; PROPOSED / OPEN)
4. docs/agenda/c-007_signal-persistence/C-007_STAGE1_AUTHORIZATION_AND_REVIEW_NOTES_23-OCT-2026.txt (new; ChatGPT, unchanged)
5. docs/inputs/chatgpt_originals_and_replies/C-007_CLAUDE_STAGE0_RESEARCH_PROMPT_23-OCT-2026.txt (new; ChatGPT, unchanged)
6. docs/inputs/chatgpt_originals_and_replies/C-007_STAGE0_ANSWER_SHEET_FOR_CLAUDE_23-OCT-2026.txt (new; ChatGPT, unchanged)
7. docs/inputs/chatgpt_originals_and_replies/C-007_CLAUDE_STAGE0_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
8. docs/inputs/chatgpt_originals_and_replies/C-007_CLAUDE_STAGE0_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (new; Claude, as
   delivered)
9. docs/inputs/chatgpt_originals_and_replies/C-007_CLAUDE_REPOSITORY_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
10. docs/inputs/chatgpt_originals_and_replies/C-007_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (new; Claude, as delivered)
11. docs/inputs/chatgpt_originals_and_replies/C-007_STAGE1_RECONCILIATION_TABLE_23-OCT-2026.txt (new; Claude, as delivered)
12. docs/inputs/reconciliation/C-007_STAGE1_CHATGPT_RECONCILIATION_REVIEW_23-OCT-2026.txt (new; ChatGPT, unchanged)
13. docs/inputs/reconciliation/C-007_CHATGPT_SPOTCHECK_AND_REVIEW_23-OCT-2026.txt (new; ChatGPT, unchanged)
14. docs/inputs/reconciliation/C-007_CHATGPT_REVIEW_NOTES_23-OCT-2026.txt (new; ChatGPT, unchanged)
15. docs/inputs/reconciliation/C-007_REV1_FINAL_REVIEW_NOTE_23-OCT-2026.txt (new; ChatGPT, unchanged)
16. docs/inputs/reconciliation/C-007_REV2_CHATGPT_FINAL_REVIEW_23-OCT-2026.txt (new; ChatGPT, unchanged)
17. docs/inputs/manuals/C-007_SIGNAL_PERSISTENCE_MANUAL_DRAFT_23-OCT-2026.md (new; Claude's revision 0, as delivered)
18. docs/inputs/manuals/C-007_SIGNAL_PERSISTENCE_MANUAL_DRAFT_REV1_23-OCT-2026.md (new; revision 1, as delivered)
19. docs/inputs/manuals/C-007_SIGNAL_PERSISTENCE_MANUAL_DRAFT_REV2_23-OCT-2026.md (new; revision 2, as approved)
20. docs/governance/V2_DECISION_REGISTER.md (DR-060 added)
21. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)
Twenty-one files. The list is closed; README.md is not changed (the agenda status row is the repository's only status index).
