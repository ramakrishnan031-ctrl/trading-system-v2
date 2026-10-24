# C-008 SIGNAL PROCESSING: REVIEW NOTES

Status: CLOSED 2026-10-24. C-008_manual.md revision 3 is FROZEN (DR-061), with its two companion files stored unchanged
beside it.
Topic: C-008 Signal processing (master agenda, section C); ChatGPT's working title: signal processing / decision pipeline.

## 1. Course of the topic (23 and 24-Oct-2026, IST)

- 21:13 on 23-Oct: C-007 pushed by Rama (dce3164); the C-008 base.
- About 21:17: ChatGPT's Stage 0 research prompt and answer sheet (inventory and planning only).
- 21:41: Claude's Stage 0 answers and source inventory: 38 register rows (121 files), 35 count-only searches (Q01 to Q35), a
  proposed scope; the read tool locked, so that no source content could be opened at Stage 0.
- 21:44: ChatGPT's review of Stage 0 and Stage 1 authorisation (C-008_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_FOR_CLAUDE_
  23-OCT-2026.txt, in this folder): the scope and owners (items A to D), VP-11, VP-24, VP-31 and VP-59 not to be ratified
  (E), records R34 to R36 not to be read and the July and host records excluded (F), the project-side manual index (G).
- 21:45: the Stage 0 lock lifted as a step of its own (the tool's SHA-256 recorded before and after); 21:46: the gate test
  passed.
- 21:49 to 22:29: 123 gated reads of 20 files at the pin; 21:56 to 22:07: 14 count-only searches (Q36 to Q49).
- Until about 23:21: the draft; a separate agent's check of a snapshot (22 findings: 2 MUST-FIX, 14 SHOULD-FIX, 6 NOTE),
  each re-checked against the saved evidence and applied from 23:03 to 23:21; one finding (IC-21) needed two more gated
  reads, 124 and 125 (23:11).
- About 23:22: revision 0 and its inventory delivered (550 and 864 lines).
- 23:53: ChatGPT's Stage 1 review ("ACCEPTABLE FOR GOVERNED REVISION; NOT APPROVED FOR FREEZE"; RC-15 recommended as an
  erratum for Rama's decision; the entry throttle's owner and the index reconciliation open).
- About 23:58: Rama's decisions (multiple choice): RC-15 approved as erratum B-009 E-3, to be applied at the C-008 freeze;
  revision 1 to be returned now, the index reconciliation later.
- 00:04 on 24-Oct: revision 1.
- 00:29: ChatGPT's review of revision 1 ("ACCEPTABLE AS A GOVERNED REVISION; NOT APPROVED FOR FREEZE YET"; the index
  reconciliation a freeze blocker), with LEGACY_SYSTEM_MANUAL_INDEX.md, which Rama supplied from ChatGPT's project.
- 00:32: four positive-control searches (Q50 to Q53); 00:40: revision 2 (the index reconciliation, IX-01 to IX-11).
- 00:47: ChatGPT's review of revision 2 ("REVIEW HOLD - NOT APPROVED FOR FREEZE"): a statement-level check against chapters
  02 and 03 of the 18-Sep manual required before the freeze.
- 00:50: ChatGPT's decision note (the method: a bounded comparison, a table, a stop on any contradiction).
- 00:51: Rama's instruction to check the GitHub repositories and download the chapters if found; 00:52 to 00:53: the public
  records repository cloned without checkout, at depth 1, and only the two chapter blobs fetched; about 00:54: the same two
  files uploaded by Rama from ChatGPT's project, byte-identical.
- 00:54 to 01:32: 18 logged document reads of the chapters (X001 to X018); 01:38: the cross-check (61 rows) and the proposed
  revision-3 changes delivered.
- 08:05: ChatGPT's value-added review of the cross-check and the proposals ("REVIEW ONLY - NOT APPROVED FOR FREEZE";
  recommendations for D-1 to D-7; twelve guards).
- About 08:08: Rama's decisions D-1 to D-7 (multiple choice); the one re-view of a saved output that D-5 allowed.
- 08:22: revision 3, with its inventory and the companion cross-check.
- 09:08: ChatGPT asked for the two companion files before reviewing; its review covers all three files.
- 09:13: ChatGPT's review of revision 3 ("SUBSTANTIVE REVISION-3 REVIEW: PASS, WITH THE ABOVE GOVERNANCE CONDITIONS";
  "FREEZE / COMMIT / PUSH: NOT AUTHORISED BY THIS REVIEW").
- 09:15: Rama's approval under W-4 (multiple choice): "Approve, prepare the freeze (Recommended)" for exactly the three files
  of revision 3; "Yes, ChatGPT checks it first (Recommended)" for this package.
- About 09:27: Rama's answers on two questions about the package: "Add it, flagged (Recommended)" (the E-3 status sentence in
  B-009_review_notes.md, section 5); "No, bookmarks only (Recommended)" (no V2 design-inputs file for C-008).
- 09:30: the freeze-time recheck against the live register and main (section 5); then the freeze files and the push
  commands (sections 7 and 8), under W-4 (DR-017) and W-9.

## 2. From the delivered drafts to the frozen manual

- Revision 0 (Claude's Stage 1 draft, 23-Oct): written from the saved evidence of the logged reads and searches, under the
  authorisation's scope; checked before delivery by a separate agent (section 4).
- Revision 1 (ChatGPT's Stage 1 review and Rama's two decisions of 23-Oct): RC-15 accepted as erratum B-009 E-3, with the
  texts to apply at the freeze (manual section 14.6); the index reconciliation left pending by Rama's choice; no fact changed.
- Revision 2 (ChatGPT's review of revision 1, with the index Rama supplied): the reconciliation against
  LEGACY_SYSTEM_MANUAL_INDEX.md (section 14.3: no conflict, no erratum); four positive-control searches; OQ-02 answered and
  OQ-03 added.
- Revision 3 (ChatGPT's review of revision 2, its decision note and its review of the cross-check, with Rama's decisions D-1
  to D-7): the statement-level reconciliation against chapters 02 and 03 of the 18-Sep manual (section 14.7 and the
  companion cross-check): 61 statements, 41 CONSISTENT, 8 PARTIAL, 3 CONFLICT, 9 NOT COMPARABLE; the three conflicts
  recorded as differences between documents about two trees; SS-03 corrected; W-08 and the Lines note made more precise; a
  pointer added to U-01 for the testing tree only; OQ-03 answered.
- Freeze edits (no statement changed; the manual's section 19.6 lists them): the title; the status line (FROZEN, ChatGPT's
  review of revision 3 and Rama's approval); the companions line (where each file is stored, and the documents not
  stored); one bullet in section 1.2 (ChatGPT's review of revision 3 and Rama's approval); RC-15's result cell (applied,
  DR-061); section 14.6 (its heading, the freeze-time recheck, and DR-061 in place of "DR-0NN" in the two texts and in its
  item 4); section 19.6.
- Checks of the frozen file against revision 3 as approved: the same 619 citations (as a multiset), each printed by a
  logged read at its pinned blob; the same 205 claim IDs in the same order; the same evidence-label counts; every
  backticked repository path in the pinned listing; every 40- and 64-character hexadecimal ID known (pinned blobs, the
  pin, V2 main, the records repository's commit and blobs, the SHA-256 of a recorded input); ASCII only; no session link,
  absolute path or credential pattern; the value-blind identifier scan finds only long hexadecimal tokens and the two
  chapter read IDs X001 and X018.
- The companions are stored as approved, so their own header lines are as delivered: the inventory names itself the
  companion to the revision 3 draft, and the cross-check says it is frozen with revision 3 if Rama approves it under W-4,
  which he did at 09:15 IST.

## 3. Responses to ChatGPT's reviews

- Stage 0 review and Stage 1 authorisation: applied as recorded in the manual's sections 1 and 1.2 (the lock lifted as a
  step of its own; the count-only searches described as inspecting file bytes and printing counts only; the narrow
  contextual ranges; the trace table, section 4.3). Its item G (the project-side index) could not be applied until
  Rama supplied the file at 00:29 IST on 24-Oct (section 4, P-1).
- Stage 1 review: no factual or editorial change asked; RC-15 decided by Rama (erratum E-3); OQ-01 kept open; the index
  reconciliation pending by Rama's choice; the points to preserve kept (the manual's section 19.3).
- Review of revision 1: the index reconciled (section 14.3); the freeze-time steps for E-3 kept for the freeze (section
  14.6); its review limit recorded (section 1.2; section 19.4).
- Review of revision 2 and the decision note: the statement-level check made within the bounds the note set, first
  delivered for review with the proposed changes, then taken into revision 3 after Rama's decisions; the restriction on the
  records repository superseded by Rama for the two chapter files only (D-7) (sections 1.2, 14.7 and 19.5).
- Value-added review of the cross-check: D-1 to D-7 as Rama decided them, and its twelve guards (section 19.5).
- Review of revision 3: its conditions met at this freeze: OQ-01 stays OPEN (the manual's section 15; bookmark BK-43, not
  closed by implication); erratum E-3 applied only now, its anchor and number verified against the live register and main,
  the approved texts applied to the exact passages with both "DR-0NN" placeholders replaced by DR-061 (section 5); the three
  files are those Rama approved, unchanged since the review (section 7); the companions are kept with the manual; the
  freeze decision is DR-061. Its review limits stand: it did not reproduce Claude's gated reads, masking, protected-path
  checks, clone and fetch commands, the 125 source reads, the 18 searches or the 61-row comparison.

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- P-1: the authorisation named LEGACY_SYSTEM_MANUAL_INDEX.md as a project-side input; Claude found it neither in the Claude
  Project, the session's attachments, V2 main nor the legacy listing (rechecked at about 23:55 IST on 23-Oct), recorded it
  as not available and returned revisions 0 and 1 with the reconciliation pending; Rama supplied it at 00:29 IST. Then
  ChatGPT's review of revision 2 required the chapter 02 and 03 check before the freeze. Lesson: ask for a named input
  before the first read; at Stage 0, list the overlapping written chapters of the 18-Sep manual.
- P-2: the review of revision 2 said Claude was not to fetch the chapters from the records repository and asked Rama to
  supply them or revise the source authorisation; Rama's instruction of 00:51 IST revised it, and Claude fetched the two
  files itself (D-7). The clone's commands are not in the part of the session log that is kept; its pack records show its
  scope (one commit, its 27 trees, the two chapter blobs, no other blob). Lesson: record each such fetch in the topic's
  notes when it is run.
- P-3: saved outputs of earlier logged reads (011, 019 and 042) were re-viewed for wording while the cross-check was
  prepared, and 042 once more under D-5; no new legacy source read was made for revision 3, and the read log still holds 125
  reads (the manual's section 1.2).
- P-4: SS-03 had left out a decrement of the in-flight count that the saved output of read 042 had printed; the chapter 02
  comparison found it. Lesson: reconcile any list of sites with every occurrence of the name.
- P-5: the independent check of revision 0 found RC-15 (B-009 section 10); lesson on counts toward a threshold.
- P-6: the independent check was run once, on a snapshot of revision 0; the applied fixes and revisions 1 to 3 were not
  checked again by a separate agent. ChatGPT's reviews are the other checks; none reproduced Claude's local checks.
- Documents read but not stored: LEGACY_SYSTEM_MANUAL_INDEX.md (124 lines, SHA-256 DFE1AC19...3287) and chapters 02 and 03
  of the 18-Sep manual (SHA-256 8B5AEC74...6D49 and F7D38F2D...3087; blobs f6aadb17 and 7be2f68f in the records
  repository). They hold a production host's address, PC paths, a deployed directory, loopback addresses and a user name,
  which this project does not reproduce; they stay in ChatGPT's project and the records repository.

## 5. Errata to frozen manuals

One: B-009 E-3, approved by Rama on 23-Oct-2026 (about 23:58 IST), applied at this freeze with the texts of
C-008_manual.md section 14.6.
- Freeze-time recheck (09:30 IST on 24-Oct): V2 main fetched again: HEAD, origin/main and the remote's main all dce3164; the
  register's last row DR-060, so this freeze's row is DR-061; B-009_manual.md has errata E-1 (DR-047) and E-2 (DR-055) only,
  so E-3 is the next number; its line 235 is exactly the row quoted in section 14.6 item 1; the search of item 4 over the
  186 files under docs/agenda found the same four lines (A-011 line 129, B-004 lines 109 and 164, B-009 line 235).
- Applied to B-009_manual.md: (1) line 235's third cell, the struck text and the replacement; (2) the sentence on E-3 after
  the sentence on E-2 in the status line; (3) the erratum section at the end of the file, after E-2's. Each text is the
  approved one with "DR-0NN" replaced by DR-061 and nothing else changed; the tool that applied them checks this, and that
  each anchor occurs once at the line stated.
- Applied to B-009_review_notes.md, by Rama's decision of about 09:27 IST ("Add it, flagged"), as at E-1 and E-2: one status
  sentence. It is NOT among the texts of section 14.6, which ChatGPT's review of revision 3 asked to be applied alone, and is
  flagged for ChatGPT's check of this package: "Erratum E-3 to the B-009 manual (section 10: which broker errors count
  toward the automatic SOFT_KILL) added 2026-10-23 during C-008 (C-008_review_notes.md section 5), recommended in
  ChatGPT's Stage 1 review of C-008, approved by Rama on 23-Oct-2026 and recorded in DR-061 together with the C-008 freeze.
  It qualifies only that table cell; the B-009 freeze (DR-046) and errata E-1 and E-2 are unchanged."
- Not changed: B-009's other passages and its reconciliation addendum (the search of item 4 finds no other line);
  B-004_manual.md, which already records the whitelist; the 18-Sep manual, which is not a manual of this series (section
  14.7).

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- VP-11, VP-24, VP-31 and VP-59 name C-008 in their Ratify-at cells; none is ratified at C-008 (the authorisation, item
  E), and the cells are unchanged. Each is annotated with the C-008 facts that bear on it (the manual's section 14.4).
- Q-20 (the processor's part: no instrument lookup of its own, SS-06) and Q-45 (h) (the processor's side, SS-01 and SS-04)
  are annotated; both stay OPEN.
- BK-02, BK-03, BK-06, BK-07, BK-12, BK-14, BK-15, BK-17, BK-18, BK-19, BK-21, BK-22 and BK-24 are annotated with C-008's
  notes (the manual's section 14.4) and stay OPEN.
- The twelve bookmarks C8/BM-01 to C8/BM-12 are carried into the design log as BK-31 to BK-42, and OQ-01 as BK-43, an OPEN
  QUESTION for the master agenda review; none is resolved.
- No V2 design-inputs file (Rama, about 09:27 IST: "No, bookmarks only"); the points wait for the whole-design review (W-3,
  W-9).
- Design log: four lessons (section 1), the VP, Q and bookmark notes, the agenda status row, session notes (the owed record
  of the C-007 push, and C-008's course under 24-Oct) and bookmarks BK-31 to BK-43.

## 7. Freeze record

Prepared under W-4 (DR-017): ChatGPT reviewed the topic at each stage (its Stage 0 review and authorisation, its Stage 1
review, its reviews of revisions 1 and 2, its decision note, its review of the cross-check and its review of revision 3)
and its points are applied; Rama decided RC-15, D-1 to D-7 and the two package questions, and approved revision 3 at 09:15
IST on 24-Oct-2026, exactly these files, re-hashed when the freeze files were built and unchanged since ChatGPT's review:

| File (as delivered) | Lines | SHA-256 | In this package |
|---|---|---|---|
| C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV3_24-OCT-2026.md | 714 | 39CC1AA06B4DE4761E3C3C4B9F2306E6247E2C42B43F93B7B5E4BDF30747FCE4 | docs/inputs/manuals/, unchanged; its frozen form is C-008_manual.md |
| C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV3_24-OCT-2026.txt | 1,083 | 7AE9FA01BDA1823B66CE222E8746814B956265A2C89845EC3D35EC5D962BFCC3 | this folder, unchanged |
| C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md | 186 | 9B1878B02869C7700B1F86A3BC41348F9CE93CA4341D7C679AC8C2CF079B4347 | this folder, unchanged |

At Rama's choice, ChatGPT checks this package before the push. The freeze takes effect when Rama declares the package final
and runs PART 2 of its push commands (C-008_PUSH_COMMANDS_24-OCT-2026.txt). ChatGPT's check of the package and the record
of the push are stored with the next package, as the C-007 push is recorded with this one. Claude stores, commits and
pushes nothing.

## 8. Package

1. docs/agenda/c-008_signal-processing/C-008_manual.md (new; FROZEN, revision 3)
2. docs/agenda/c-008_signal-processing/C-008_review_notes.md (new; this file)
3. docs/agenda/c-008_signal-processing/C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV3_24-OCT-2026.txt (new; the
   companion inventory, as approved)
4. docs/agenda/c-008_signal-processing/C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md (new; the companion
   cross-check, as approved)
5. docs/agenda/c-008_signal-processing/C-008_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_FOR_CLAUDE_23-OCT-2026.txt (new;
   ChatGPT, unchanged)
6. docs/inputs/chatgpt_originals_and_replies/C-008_CLAUDE_STAGE0_RESEARCH_PROMPT_23-OCT-2026.txt (new; ChatGPT, unchanged)
7. docs/inputs/chatgpt_originals_and_replies/C-008_STAGE0_ANSWER_SHEET_FOR_CLAUDE_23-OCT-2026.txt (new; ChatGPT, unchanged)
8. docs/inputs/chatgpt_originals_and_replies/C-008_CLAUDE_STAGE0_ANSWERS_23-OCT-2026.txt (new; Claude, as delivered)
9. docs/inputs/chatgpt_originals_and_replies/C-008_CLAUDE_STAGE0_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (new; Claude,
   as delivered)
10. docs/inputs/chatgpt_originals_and_replies/C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_23-OCT-2026.txt (new;
    Claude, revision 0's inventory, as delivered)
11. docs/inputs/chatgpt_originals_and_replies/C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV1_24-OCT-2026.txt (new;
    revision 1's, as delivered)
12. docs/inputs/chatgpt_originals_and_replies/C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV2_24-OCT-2026.txt (new;
    revision 2's, as delivered)
13. docs/inputs/chatgpt_originals_and_replies/C-008_CHAPTER_02_03_CROSS_CHECK_24-OCT-2026.md (new; Claude, the cross-check
    as first delivered)
14. docs/inputs/chatgpt_originals_and_replies/C-008_REV3_PROPOSED_CHANGES_24-OCT-2026.md (new; Claude, as delivered)
15. docs/inputs/reconciliation/C-008_CHATGPT_STAGE1_REVIEW_AND_OPEN_DECISIONS_23-OCT-2026.txt (new; ChatGPT, unchanged)
16. docs/inputs/reconciliation/C-008_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (new; ChatGPT's review of revision 1, unchanged)
17. docs/inputs/reconciliation/C-008_CHATGPT_REVIEW_REV2_24-OCT-2026.txt (new; ChatGPT, unchanged)
18. docs/inputs/reconciliation/C-008_DECISION_AND_NEXT_STEPS_24-OCT-2026.txt (new; ChatGPT, unchanged)
19. docs/inputs/reconciliation/C-008_CHATGPT_VALUE_ADDED_REVIEW_24-OCT-2026.txt (new; ChatGPT, unchanged)
20. docs/inputs/reconciliation/C-008_CHATGPT_VALUE_ADDED_REVIEW_REV3_24-OCT-2026.txt (new; ChatGPT, unchanged)
21. docs/inputs/manuals/C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_23-OCT-2026.md (new; Claude's revision 0, as delivered)
22. docs/inputs/manuals/C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV1_24-OCT-2026.md (new; revision 1, as delivered)
23. docs/inputs/manuals/C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV2_24-OCT-2026.md (new; revision 2, as delivered)
24. docs/inputs/manuals/C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV3_24-OCT-2026.md (new; revision 3, as approved)
25. docs/agenda/b-009_intraday-operation/B-009_manual.md (erratum E-3 applied)
26. docs/agenda/b-009_intraday-operation/B-009_review_notes.md (one status sentence added)
27. docs/governance/V2_DECISION_REGISTER.md (DR-061 added)
28. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)
Twenty-eight files. The list is closed; README.md is not changed (the agenda status row is the repository's only status
index).
