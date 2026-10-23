# C-006 QUEUE HANDLING: REVIEW NOTES

Status: CLOSED 2026-10-23. C-006_manual.md revision 1 is FROZEN (DR-059).
Topic: C-006 Queue (master agenda, section C); ChatGPT's working title: Queue handling.

## 1. Course of the topic (23-Oct-2026, IST)

- 16:17: ChatGPT's Stage 0 prompt and answer file (source inventory only), after C-005's push (3e26ec1, 16:13).
- 16:21: Claude's Stage 0 answers (C-006_CLAUDE_STAGE0_INVENTORY_ANSWERS_23-OCT-2026.txt): pin verified, 30 register rows (32
  candidate files), 15 path-name hits, eight queue-like mechanisms recorded, a Stage 1 plan; no source content opened.
- 16:29: ChatGPT's Stage 0 review (C-006_STAGE0_REVIEW_AND_CLARIFICATION_REQUEST_23-OCT-2026.txt, in this folder): accepted with
  one blocking clarification: is config/system_config.yaml one of the 102 protected paths?
- 16:32 to 16:43: Claude's clarification (C-006_CLAUDE_STAGE0_CLARIFICATION_23-OCT-2026.txt): not protected (it matches none of
  the 13 never-open rules); a refusal-only test of the gate; a revised Stage 1 order; no source content opened.
- 16:50: ChatGPT's Stage 1 authorisation (C-006_STAGE1_AUTHORIZATION_FOR_CLAUDE_23-OCT-2026.txt, in this folder): "APPROVED
  WITH SCOPE CONTROLS".
- 16:50 to 17:24: Claude's Stage 1 research: 29 count-only searches (16:55 to 17:07) and 73 gated reads at the pin (16:59:41 to
  17:16:01), each after the gate's self-test; repository answers and source inventory stamped 17:24 and delivered.
- By 17:38: ChatGPT's reconciliation notes and manual draft (revision 0), stored unchanged in docs/inputs/reconciliation/ and
  docs/inputs/manuals/.
- After 17:38: Claude's review of both (section 2), the frozen files and the push commands, under W-4 (DR-017) and W-9.

## 2. Corrections applied to ChatGPT's manual draft (revision 0 to revision 1)

Each was checked against the read log and the saved evidence of the logged reads. None reverses a finding of the draft.
- R-1. Section 2.1: the import of the queue module is cited (main.py line 29); the repository values, which sat under the
  SOURCE-VERIFIED heading, are labelled FACT (CONFIG).
- R-2. Section 4.1 introduced its list as the gates "inside _process_request"; the shutting-down check and the per-IP limit run
  in _handle_webhook before _process_request is called (receiver lines 483-511).
- R-3. Section 4.2 put the new signal_id before the validation and repeat checks; the checks run first and the signal_id is
  created just before the INSERT (receiver lines 956-957; C-005_manual.md sections 3 and 5). The queue-full consequence now
  names the QUEUE_FULL mark and, when that mark fails, bookmark BM-02.
- R-4. Section 5: a row for the receiver's retry when the queue is still full (receiver lines 994-1001), the unwired mark on the
  FIX-007 row, and the placement timeout, which is not re-queued (processor lines 1464-1484).
- R-5. Section 5.1: the two tests that set the limiter on the object are cited (test_signal_processor lines 2155 and 2227).
- R-6. Section 7: each status write is cited; the bare REJECTED of the pre-check path is added (processor lines 435-437); the
  status function is named as the store's (core/state_store.py lines 1408-1426), which the processor calls.
- R-7. Section 9: the log lines are cited; the dashboard row says only what was read (the limit's source line was not read; the
  backpressure point is round(backpressure_pct x limit), capacity.py lines 171-181).
- R-8. Section 10 placed the /health tests in tests/unit/test_webhook_receiver.py; they are in
  tests/unit/test_fix134_backpressure.py (lines 80-87 and 166-197). The test lines are cited, and the crash-test inspector
  (state_inspector.py lines 61-67) is added.
- R-9. Section 11 called the two qualifications "corrections": no frozen manual is changed and no erratum is issued; each is a
  bookmark (W-9; the reconciliation's section 3). New section 11.1 lists the frozen statements re-verified at the pin.
- R-10. Section 1 states the read discipline from the read log (every read matched its pinned blob, after the gate's self-test;
  the protected-path check zero before every read and at delivery).
- R-11. Header, a sources table with the pinned blobs (new section 1.1), the freeze status and a revision section 15, in the form
  of the other frozen manuals; typography made ASCII and labels written FACT (SOURCE-VERIFIED) and so on. No statement changed
  by it.
Check of revision 1: its 146 path-and-line citations (and its bare line references) were each printed by a logged read.

## 3. Response to ChatGPT's reconciliation notes

- Agreed in full: its review decision (section 1), the ten findings that must stay visible (section 2, A to J), the routing of
  the eight bookmarks (section 3), the boundary and limitation check (section 4) and its decision (section 6). No correction to
  Claude's research files was raised, and none is made: the Stage 0 answers, the clarification and the two Stage 1 files are
  stored as delivered.
- Its checklist before freezing (section 5), checked for revision 1: the eight Stage 0 questions are answered or marked unknown
  or open (manual sections 2 to 9; the answers, section 1); important statements keep their path and line evidence and label
  (R-1, R-4 to R-8 add lines where revision 0 pointed to the companion files); the boundaries hold (sections 1, 2.2, 6, 7); the
  lifecycle claims no runtime occurrence (sections 4.1, 8, 9, 10 and 14); the bookmarks are carried into the design log as
  BK-10 to BK-17, unresolved; the package is stored only by Rama's push, which Claude then checks on GitHub.
- The files found by count only (the answers, section 6: X-1 to X-4) are not approved and were not read; the two July 2026
  queue records stay excluded.
- Its limitation stands: no independent agent check was run in this topic (Rama's lean workflow).
- No reconciliation addendum is written: the notes need no answer beyond this section.

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- The Stage 0 answers cited "102 never-open paths (21 config)" and listed config/system_config.yaml without stating its
  classification, so the review could not tell that it was permitted; the clarification settled it by path classification only.
- C-005's count-only searches had run without a per-file gate (the scope lists held no protected path, so nothing was exposed).
  For Stage 1 the gate covered every read and every count search, ran a self-test before each call, refused by protected blob as
  well as by name, and refused the two July records the authorisation excluded (also left out of the search scope S-ALL6).
- Before delivery, eleven citations covered test ranges of which only grep lines had been printed; those ranges were read (reads
  066 to 073) and every citation was then checked against the saved evidence (284 citations).
- Deviations as recorded in the Stage 1 answers (section 8, D-1 to D-5); the only spill, CFG lines 122-123 (a comment line and a
  blank line before the queue block), holds no value.

## 5. Errata to frozen manuals

None. Two frozen statements are qualified by bookmarks instead (W-9; the reconciliation's section 3): A-011_manual.md section 12,
Q31, on what a clean stop did to queued signals (BM-03, BK-12), and C-005_manual.md section 6.2's rate-limit pre-check re-queue,
present in code but not wired at the pin (BM-06, BK-15). The other frozen statements re-verified at the pin are consistent
(C-006_manual.md section 11.1).

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- VP-59 lists C-006 to C-009 and I-009 as its ratification points; its legacy line on a clean stop is qualified by BM-03. VP-59
  is not ratified at C-006: it is left for the whole-design review (bookmark BK-18), as VP-12 at C-005 and VP-27 at C-004. VP-59
  annotated.
- BK-04 (C-005 BM-02 / C5-L4) is re-verified and extended by BM-01 (BK-10); BK-04's row is annotated and stays OPEN.
- V2 considerations from this topic: C-006_v2_design_inputs.md (PROPOSED / OPEN).
- Design log: three lessons (section 1), the VP-59 note, the BK-04 note, the agenda status row, session notes (with the owed
  record of the C-005 push) and bookmarks BK-10 to BK-18.

## 7. Freeze record

Prepared under W-4 (DR-017): ChatGPT reviewed the topic (its Stage 0 review, its Stage 1 authorisation and its reconciliation
notes) and its points are applied; the notes leave no question for Rama. The freeze takes effect when Rama declares the package
final and runs PART 2 of the push commands (C-006_PUSH_COMMANDS_23-OCT-2026.txt). Claude stores, commits and pushes nothing.

## 8. Package

1. docs/agenda/c-006_queue/C-006_manual.md (new; FROZEN, revision 1)
2. docs/agenda/c-006_queue/C-006_review_notes.md (new; this file)
3. docs/agenda/c-006_queue/C-006_v2_design_inputs.md (new; PROPOSED / OPEN)
4. docs/agenda/c-006_queue/C-006_STAGE0_REVIEW_AND_CLARIFICATION_REQUEST_23-OCT-2026.txt (new; ChatGPT, unchanged)
5. docs/agenda/c-006_queue/C-006_STAGE1_AUTHORIZATION_FOR_CLAUDE_23-OCT-2026.txt (new; ChatGPT, unchanged)
6. docs/inputs/chatgpt_originals_and_replies/C-006_CLAUDE_RESEARCH_PROMPT_23-OCT-2026.txt (new; ChatGPT, unchanged)
7. docs/inputs/chatgpt_originals_and_replies/C-006_ANSWER_FILE_FOR_CLAUDE_23-OCT-2026.txt (new; ChatGPT, unchanged)
8. docs/inputs/chatgpt_originals_and_replies/C-006_CLAUDE_STAGE0_INVENTORY_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
9. docs/inputs/chatgpt_originals_and_replies/C-006_CLAUDE_STAGE0_CLARIFICATION_23-OCT-2026.txt (new; Claude, as delivered)
10. docs/inputs/chatgpt_originals_and_replies/C-006_CLAUDE_REPOSITORY_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
11. docs/inputs/chatgpt_originals_and_replies/C-006_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (new; Claude, as delivered)
12. docs/inputs/manuals/C-006_QUEUE_HANDLING_MANUAL_DRAFT_23-OCT-2026.md (new; ChatGPT's revision 0, unchanged)
13. docs/inputs/reconciliation/C-006_CHATGPT_RECONCILIATION_NOTES_23-OCT-2026.txt (new; ChatGPT, unchanged)
14. docs/governance/V2_DECISION_REGISTER.md (DR-059 added)
15. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)
Fifteen files. The list is closed; README.md is not changed (the agenda status row is the repository's only status index).
