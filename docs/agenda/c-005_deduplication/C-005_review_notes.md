# C-005 DEDUPLICATION AND IN-FLIGHT HANDLING: REVIEW NOTES

Status: CLOSED 2026-10-23. C-005_manual.md revision 1 is FROZEN (DR-058).
Topic: C-005 Deduplication (master agenda, section C); ChatGPT's working title: Deduplication and in-flight handling.

## 1. Course of the topic (23-Oct-2026, IST)

- 14:40: ChatGPT's research prompt and answer file for Stage 0 (source inventory only).
- About 15:10: Claude's Stage 0 answers (C-005_CLAUDE_STAGE0_INVENTORY_ANSWERS_23-OCT-2026.txt): pin verified, 40 files in 32
  register rows, 138 path-name hits classified, a Stage 1 plan; no source content opened.
- 15:20: ChatGPT's Stage 0 review and Stage 1 go-ahead (C-005_CHATGPT_STAGE0_REVIEW_AND_STAGE1_GO_AHEAD_23-OCT-2026.txt, in this
  folder), with one clarification to record (the two populations of 102).
- 15:22 to 15:40: Claude's Stage 1 research (46 logged reads at the pin; 25 count-only searches); repository answers and source
  inventory delivered at about 15:41.
- 15:43: ChatGPT's reconciliation draft and manual draft (revision 0), stored unchanged in docs/inputs/reconciliation/ and
  docs/inputs/manuals/.
- After 15:44: Claude's review of both drafts (section 2), the frozen files and the push commands, under W-4 (DR-017) and Rama's
  rule of 23-Oct-2026 (W-9): simple answers, final files, bookmarks for cross-topic points, no solving now.

## 2. Corrections applied to ChatGPT's manual draft (revision 0 to revision 1)

Each was checked against the pinned source or the read log. None reverses a finding of the draft.
- R-1. Section 4.2 cited R lines 171-175 for the integer conversion and its fallback, and R lines 230-237 for the minimum; the
  conversion and fallback are at R lines 232-235 and the minimum at R line 236 (R lines 171-175 resolve the webhook block).
- R-2. Section 6.2: after a failed re-queue the claim is released explicitly on the rate-limit pre-check path (P lines 456-460),
  not through the processor's finalisation; the placement path does go through it (P lines 1526-1532, 1248-1271).
- R-3. Section 6.4: "Shutdown stops the receiver" overstated stop(): it makes new requests answer 503 and stops the sweeper; the
  server thread lives on (R lines 1166-1176, 485-490; C-002 section 6.4).
- R-4. Section 2.1 said the protected-path checks "reportedly" stayed at zero; the read log records the check before every read
  (46 rows, nev0=ok) and the inventory records the check at delivery.
- R-5. Section 10.2: the EOD statement now names its basis, the term search of R lines 751-835.
- R-6. Section 5.1: the schema's status check still lists IN_PROCESS and DUPLICATE (SQL lines 60-65), which the receiver path
  never writes (added; it bears on bookmark BM-03 and on lead C5-L3).
- R-7. Section 11: the processor test lines listed (the draft pointed to the answer sheet).
- R-8. Section 13: a row for Q-45 (h), the B-007 audit lead owned by C-005 and C-008; new section 13.1 lists the frozen
  statements re-verified at the pin (C-004 L-04 and L-13, B-007 sections 11, 19 and 20, B-009 section 5, B-006 TH4, A-011
  section 14, C-002 sections 6.1, 6.4, 13.4 and 13.6, C-003 section 6.5, C-001 A-L7 and A-L9) and records that no erratum is
  needed.
- R-9. Header, section 17's two open items and a revision section 18 in the form of the other frozen manuals; typography made
  ASCII (the frozen manuals are ASCII only) and labels written FACT (SOURCE-VERIFIED) and so on. No statement changed by it.

## 3. Response to ChatGPT's reconciliation

- Agreed in full: the evidence and method assessment (its section 2), the reconciled account (section 3), BM-01 (section 4), the
  scope decision (section 5), the labels (section 6) and the bookmark register (section 7). No correction to Claude's research
  files was raised, and none is made.
- The two expansions are not approved and were not read: scripts/eod_cleanup.py (bookmark BM-03; C-007, B-010) and the one line
  of core/config_loader.py (B-002). The manual states both as not established.
- Its limitation stands: no independent agent check was run in this topic (Rama's lean workflow).
- No reconciliation addendum is written: the reconciliation needs no answer beyond this section.
- Correction to Claude's own files: the two Stage 1 files gave their work window as "15:22 to 15:45 IST", an end time written
  before delivery, against the lesson of 2026-10-21 (a header ends at the last write). The stored copies say "15:22 to 15:40 IST",
  their last write; no other byte changed (the delivered copies are kept outside the repository).

## 4. Errata to frozen manuals

None. BM-01 found the four frozen statements on the claim's lifetime consistent (C-005_manual.md sections 6, 7 and 13.1).

## 5. V2 governance at this freeze (tracking only; no V2 decision)

- VP-12 lists C-005 / F-007 as its ratification point. C-005's subject, intake deduplication, does not carry the same-symbol
  trade rule: the receiver's guards refuse repeats for a window only, and the legacy same-symbol rule lived in the processor and
  the risk engine (A-008_manual.md section 14). VP-12 is not ratified here; it is left for the whole-design review with F-007
  (bookmark BK-02 in V2_DESIGN_LOG section 7), as VP-27 was left at C-004 under Rama's Option B of 23-Oct-2026. VP-12 and Q-33
  annotated.
- Q-45 (h) annotated with the code-path answer; it stays OPEN (records question).
- V2 considerations from this topic: C-005_v2_design_inputs.md (PROPOSED / OPEN).
- Design log: two lessons (section 1), working rule W-9 (section 5), the agenda status row, the session notes, and a new
  section 7, BOOKMARKS, started with this topic's points and the pending C-004 ratification.

## 6. Freeze record

Prepared under W-4 (DR-017): ChatGPT reviewed the topic (its Stage 0 review and its reconciliation) and its points are applied;
ChatGPT's reconciliation leaves no question for Rama. The freeze takes effect when Rama declares the package final and runs PART 2
of the push commands (C-005_PUSH_COMMANDS_23-OCT-2026.txt). Claude stores, commits and pushes nothing.

## 7. Package

1. docs/agenda/c-005_deduplication/C-005_manual.md (new; FROZEN, revision 1)
2. docs/agenda/c-005_deduplication/C-005_review_notes.md (new; this file)
3. docs/agenda/c-005_deduplication/C-005_v2_design_inputs.md (new; PROPOSED / OPEN)
4. docs/agenda/c-005_deduplication/C-005_CHATGPT_STAGE0_REVIEW_AND_STAGE1_GO_AHEAD_23-OCT-2026.txt (new; ChatGPT, unchanged)
5. docs/agenda/c-004_validation/C-004_FINAL_REVIEW_AND_C-005_GO_AHEAD_23-OCT-2026.txt (new; ChatGPT's C-004 final review,
   unchanged)
6. docs/inputs/chatgpt_originals_and_replies/C-005_CLAUDE_RESEARCH_PROMPT_23-OCT-2026.txt (new; ChatGPT, unchanged)
7. docs/inputs/chatgpt_originals_and_replies/C-005_ANSWER_FILE_FOR_CLAUDE_23-OCT-2026.txt (new; ChatGPT, unchanged)
8. docs/inputs/chatgpt_originals_and_replies/C-005_CLAUDE_STAGE0_INVENTORY_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
9. docs/inputs/chatgpt_originals_and_replies/C-005_CLAUDE_REPOSITORY_ANSWERS_23-OCT-2026.txt (new; Claude; header end time
   corrected, section 3)
10. docs/inputs/chatgpt_originals_and_replies/C-005_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (new; Claude; header end time
   corrected, section 3)
11. docs/inputs/manuals/C-005_DEDUPLICATION_AND_IN_FLIGHT_HANDLING_DRAFT_23-OCT-2026.md (new; ChatGPT's revision 0, unchanged)
12. docs/inputs/reconciliation/C-005_RECONCILIATION_DRAFT_23-OCT-2026.md (new; ChatGPT, unchanged)
13. docs/governance/V2_DECISION_REGISTER.md (DR-058 added)
14. docs/governance/V2_DESIGN_LOG.md (updated as in section 5)
Fourteen files. The list is closed; README.md is not changed (the agenda status row is the repository's only status index).
