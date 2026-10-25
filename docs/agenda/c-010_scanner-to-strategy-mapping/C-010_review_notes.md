# C-010 SCANNER-TO-STRATEGY MAPPING: REVIEW NOTES

Status: CLOSED 2026-10-24. C-010_manual.md revision 1 is FROZEN (DR-063). Its companions are stored unchanged beside it: the five evidence sheets of Stages 1 to 1E, the Stage 1 searched-files list, ChatGPT's Stage 1 authorisation review sheet and the register key map.  
Topic: C-010 Scanner-to-strategy mapping (master agenda, section C).

## 1. Course of the topic (24-Oct-2026, IST; the file time of each file received or delivered)

- 15:24 to 15:25: C-009 was pushed by Rama (82f8450), and Claude confirmed it on GitHub. This is the C-010 base.
- 15:36: ChatGPT's Stage 0 set arrived (C010_Stage0_Claude_Prompt_RESHARE.txt, C010_Stage0_Answer_Sheet_RESHARE.txt).
- 15:45: Claude's Stage 0 answers, from path names and metadata only: a register of candidate files, a scope proposal and five questions (OQ-01 to OQ-05).
- 15:58: ChatGPT's Stage 1 research prompt and authorisation review sheet (C-010_STAGE1_AUTHORISATION_REVIEW_SHEET_24-OCT-2026.txt, in this folder). It decided OQ-01 to OQ-05: the S1-1 count-only searches named; the scope of Stage 0 section 5.2; VP-45 not ratified at C-010; BK-27 kept OPEN; no chapter search, with CH-09 cited.
- 16:05 to 16:16: Stage 1. W1 ran Q01 to Q35 (32 searches and 3 positive controls) and W2 ran reads 001 to 017 (176 lines). Deviation D-1 and register gap G-1 arose (section 4).
- 16:29: the Stage 1 evidence sheet and its searched-files companion.
- 16:47: ChatGPT's Stage 1 reconciliation: reconciled with follow-up required. Finding F (the readers) was incomplete; D-1 was kept; G-1 went into the register.
- 16:57: Stage 1B proposal. 17:08: accepted (D1 to D4). 17:08 to 17:10: W3 and W4 (Q36 and Q37; reads 018 to 042, 292 lines). 17:18: the Stage 1B sheet. 17:25: ChatGPT's reconciliation (bounded findings accepted; 57 consumer lines NOT ESTABLISHED).
- 17:30: Stage 1C proposal (its header says 17:40: R-1). 17:40: accepted (D5 to D8; D9 declined). 17:43 to 17:45: W5 and W6 (Q38 to Q42; reads 043 to 050, 76 lines; D-2, O-6 and R-2). 18:00: the Stage 1C sheet. 18:05: ChatGPT's reconciliation (accepted with qualifications).
- 18:17: Stage 1D proposal. 18:38: accepted (D10 to D12). 18:39 to 18:40: W7 and W8 (Q43 to Q48; reads 051 to 058, 205 lines). 18:48: the Stage 1D sheet. 19:06: ChatGPT's reconciliation (the E1 finding accepted as INTERPRETATION; O-7 and O-8 left open).
- 19:11: Stage 1E proposal. 19:19: accepted (D13 and D14; D15 declined). 19:20 to 19:24: checks, tests and W9 (read 059, 2 lines; R-3). 19:39: the Stage 1E sheet.
- 19:43, and again at 19:55: ChatGPT's reconciliation and manual-draft prompt (the two copies are byte-identical).
- 20:49: revision 0 and its completion note. Before delivery, a separate agent checked the draft and found 2 blockers and 12 smaller points; all were applied.
- 21:15: ChatGPT's manual review notes (four bounded corrections). 21:22: revision 1 and its completion note.
- 21:41: ChatGPT's final review: revision 1 acceptable for the freeze, with package actions A to E. This package follows it.

## 2. From the drafts to the frozen manual

- **Revision 0 (20:49).** Claude drafted it under the manual-draft prompt from the 24-file evidence set (C-010_manual.md section 2.3). Before delivery, a separate agent that had not seen it being written checked it against the evidence. Its two blockers were applied: FACT labels on the path key and the queue hand-over (now HISTORICAL), and the C-008 range cited for place(), which does not reach line 996. Its twelve smaller points were applied too. Revision 0 is stored unchanged in docs/inputs/manuals/ as C-010_SCANNER_TO_STRATEGY_MAPPING_REV0_24-OCT-2026.md (625 lines; SHA-256 8929C5C392C1D8D9B534472303881C364D9006B1A0D6FFB09306C07D41F23AC7). It was delivered as C-010_SCANNER_TO_STRATEGY_MAPPING.md, the name the prompt fixed.
- **Revision 1 (21:22).** It applies the four corrections of ChatGPT's review notes and that review's final QA:
  - section 11.6 split;
  - OQ-1D-01 with its sub-items OQ-1D-01a to OQ-1D-01c;
  - the register collisions marked UNRESOLVED, with a key map proposed;
  - the masked-value counts tied to their greps, with main.py's omission from the Stage 1E list described as a record discrepancy;
  - from the final QA, six ledger rows and one sentence of section 11.5 tightened.

  Revision 1 is stored unchanged as C-010_SCANNER_TO_STRATEGY_MAPPING_REV1_24-OCT-2026.md (645 lines; SHA-256 2350441E384B9147DC010A129E9B516D97F0E8F515ED7661D7A396C25C61C2E7). It was delivered under the same name as revision 0.
- **C-010_manual.md** is revision 1 with freeze edits only: the status line, one companion-files line and the last bullet of section 18 (647 lines).
- **Checks of the frozen manual:**
  - 114 backticked line citations, each printed by a logged read (read 010 excluded) and given with its read number;
  - 106 internal section references, all resolved;
  - every hop, ledger, bookmark and open-question ID defined;
  - the recorded register reproduced byte for byte (the 54 lines of the Stage 1E sheet's section 3, SHA-256 EE1D615C0FB2127535F08F15FA0A5E2DCA79FAB98EC67A6CA7C941D73C00490D);
  - ASCII only, with no CR, session link or credential pattern;
  - the value-blind identifier scan finds only known hexadecimal IDs and the "C010" topic number in two file names;
  - tables well formed.
- The companions are stored as delivered, so their own status lines are as written at the time (each evidence sheet says "STOPPED for ChatGPT's reconciliation" and that the C-010 manual was not yet started).

## 3. Responses to ChatGPT's reviews and decisions

- **Stage 1 authorisation review sheet (15:58).** OQ-01 to OQ-05 were decided and applied (C-010_manual.md sections 1 and 2).
- **Reconciliations of Stages 1 to 1D (16:47, 17:25, 18:05, 19:06).** Each was applied. Claude proposed each next stage, ChatGPT accepted it in writing, and it was run only as accepted. D9 and D15 were declined and not run.
- **Manual-draft prompt (19:43, 19:55).** Applied in revision 0. Its completion note gave the three items the prompt asked for.
- **Manual review notes (21:15).** The four corrections were applied in revision 1, and the revision 1 completion note answers its final-QA list.
- **Final review (21:41).** Its package actions are applied in this package:
  - A: the key map is confirmed and stored as C-010_REGISTER_KEY_MAP.md, beside the unchanged register (the Stage 1E sheet's section 3 and the manual's section 3). The R15, R28 and R33 qualifications are carried there as notes.
  - B: PB-1 to PB-3 are stored as OPEN bookmarks BK-48 to BK-50 (V2_DESIGN_LOG section 7), each with its cross-check "not assigned". No existing register establishes an owner.
  - C: BK-27 stays OPEN and VP-45 NOT RATIFIED, neither status changed; each is annotated (tracking only).
  - D: revision 1 (with its frozen form), the final review, the 24-file evidence set, the unchanged register and the key map are stored together. Both completion notes are stored as provenance.
  - E: the Stage 1E record discrepancy and the lessons D-1, D-2, R-2 and R-3 are recorded in V2_DESIGN_LOG section 1; the historical records are unchanged.

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- **D-1 (Stage 1).** Rule O8 took the word "signals" at db_reader.py line 3994, a dictionary key, as use of the signals table. So read 010 (lines 3989 to 3999) was made although the authorisation's condition was not met. It is recorded and not relied on.
- **G-1 (Stage 1).** The Stage 0 register left out three files that C-001 cites for this topic and four runtime files naming the map. The gap was closed in Stage 1B (R25 to R31).
- **D-2 (Stage 1C).** Three named synthetic cases were not run as worded. No result depended on them. From Stage 1D, every named test ID printed ok under its own ID.
- **O-6 (Stage 1C).** G6's rule reached scripts/t2_cnc_gtt_realtest.py, a file outside the gate test's list, and the reader's own gate admitted it. One line was printed and recorded as a lead. From Stage 1D, the gate test and the driver shared one approved read list.
- **R-1 (Stage 1C) and R-3 (Stage 1E).** Times typed by hand in a header, a run-log row and a notes line were wrong. Correction rows were added and the originals kept.
- **R-2 (Stage 1C).** The mask counts' coverage was misdescribed (whole file for a grep, the range for a range read). No count changed.
- **Record discrepancy.** The Stage 1E sheet's security list of masked KV-shaped values left out main.py's 10 (read 003, Stage 1). The independent check of revision 0 found it, and the manual's section 19 records it. It is not a new finding, and no value was examined.
- **Observations.** O-1 to O-5, O-7 and O-8 are as in C-010_manual.md section 15.
- **No new access after Stage 1E.** The read lock stayed engaged from E-36 (19:23:53 IST) to the end. Nothing in the legacy repository was read, searched or counted for the manual, its revisions or this package.

## 5. Errata to frozen manuals

None. The frozen statements C-010 cites agree with its own lines. In one place a citation's reach was narrower than a hop needed: C-008 4.1 cites order_placer.py lines 871 to 958 for place(), while the create_trade call is at line 996. There, C-010 labels the hop INTERPRETATION instead of changing C-008.

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- **VP-45** names C-010 in its Ratify-at cell. It is not ratified here: ChatGPT's Stage 1 authorisation (OQ-03) keeps it for the whole-design review. It is annotated with the C-010 facts that bear on it; the cell is unchanged.
- **BK-27** names C-010. It is annotated (the INSERT re-verified; the E1 INTERPRETATION; the merged view) and stays OPEN (OQ-04).
- **New bookmarks**, OPEN, each with its cross-check "not assigned" (final review, action B):
  - BK-48: PB-1, OQ-1D-01a to OQ-1D-01c;
  - BK-49: PB-2, OQ-1B-01, OQ-1B-03 and OQ-1B-06;
  - BK-50: PB-3, OQ-1C-01.
- There is no V2 design-inputs file and no reconciliation addendum, as at C-008 and C-009.
- **Design log updates:** seven lessons (section 1), the VP-45 note, the BK-27 note, the agenda status row, the session notes (the owed record of the C-009 push, and C-010's course) and bookmarks BK-48 to BK-50.
- **Owed by the C-009 package:** the C-009 push line, now recorded verbatim in V2_DESIGN_LOG section 6.

## 7. Freeze record

Prepared under W-4 (DR-017), W-9 and W-10. ChatGPT authorised and reconciled each stage, reviewed revision 0 and found revision 1 acceptable for the freeze; its package actions are applied. Rama's rule of 14:06 IST removes any approval step between finalisation and storage. The three inputs of the freeze are unchanged since they were received:

| File | Lines | SHA-256 | In this package |
|---|---|---|---|
| C-010_RECONCILIATION_AND_MANUAL_DRAFT_PROMPT_24-OCT-2026.txt | 62 | 02ED4483B27E7B37806F0980B19242AE65A26E80AA8679FE508FB59568D96368 | docs/inputs/reconciliation/, unchanged |
| C-010_CHATGPT_MANUAL_REVIEW_NOTES_24-OCT-2026.txt | 38 | F2F38047EAEF903B4D75021B58B260E8A5D64D21F6D33101651BCE70AD720C79 | docs/inputs/reconciliation/, unchanged |
| C-010_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt | 38 | 58E2BC3FBA62F9E6D68D31E4017FBB7515419C3E34F488970FF0A18ABF17EB08 | docs/inputs/reconciliation/, unchanged |

The freeze takes effect when Rama runs PART 2 of the push commands (C-010_PUSH_COMMANDS_24-OCT-2026.txt). Claude then confirms on GitHub that the stored files equal the manifest. The record of the push is stored with the next package, as the C-009 push is recorded with this one. Claude stores, commits and pushes nothing.

## 8. Package

1. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_manual.md (new; FROZEN, revision 1)
2. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_REGISTER_KEY_MAP.md (new; the register key map, companion; final review, action A)
3. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_review_notes.md (new; this file)
4. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_CLAUDE_STAGE1_EVIDENCE_SHEET_24-OCT-2026.txt (new; Claude, reads 001 to 017, companion, as delivered)
5. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_STAGE1_SEARCHED_FILES_PINNED_BLOBS_24-OCT-2026.txt (new; Claude, the Stage 1 counts and pinned blobs, companion, as delivered)
6. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_CLAUDE_STAGE1B_EVIDENCE_SHEET_24-OCT-2026.txt (new; Claude, reads 018 to 042, companion, as delivered)
7. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_CLAUDE_STAGE1C_EVIDENCE_SHEET_24-OCT-2026.txt (new; Claude, reads 043 to 050, companion, as delivered)
8. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_CLAUDE_STAGE1D_EVIDENCE_SHEET_24-OCT-2026.txt (new; Claude, reads 051 to 058, companion, as delivered)
9. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_CLAUDE_STAGE1E_EVIDENCE_SHEET_24-OCT-2026.txt (new; Claude, read 059 and the register as recorded, companion, as delivered)
10. docs/agenda/c-010_scanner-to-strategy-mapping/C-010_STAGE1_AUTHORISATION_REVIEW_SHEET_24-OCT-2026.txt (new; ChatGPT's Stage 1 authorisation, unchanged)
11. docs/inputs/chatgpt_originals_and_replies/C010_Stage0_Claude_Prompt_RESHARE.txt (new; ChatGPT, unchanged)
12. docs/inputs/chatgpt_originals_and_replies/C010_Stage0_Answer_Sheet_RESHARE.txt (new; ChatGPT, unchanged)
13. docs/inputs/chatgpt_originals_and_replies/C-010_CLAUDE_STAGE0_ANSWERS_24-OCT-2026.txt (new; Claude, as delivered)
14. docs/inputs/chatgpt_originals_and_replies/C-010_STAGE1_CLAUDE_RESEARCH_PROMPT_24-OCT-2026.txt (new; ChatGPT, unchanged)
15. docs/inputs/chatgpt_originals_and_replies/C-010_STAGE1B_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; Claude, as delivered)
16. docs/inputs/chatgpt_originals_and_replies/C-010_STAGE1C_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; Claude, as delivered)
17. docs/inputs/chatgpt_originals_and_replies/C-010_STAGE1D_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; Claude, as delivered)
18. docs/inputs/chatgpt_originals_and_replies/C-010_STAGE1E_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; Claude, as delivered)
19. docs/inputs/chatgpt_originals_and_replies/C-010_MANUAL_DRAFT_COMPLETION_NOTE_24-OCT-2026.txt (new; Claude, revision 0's completion note, provenance, as delivered)
20. docs/inputs/chatgpt_originals_and_replies/C-010_MANUAL_REV1_COMPLETION_NOTE_24-OCT-2026.txt (new; Claude, revision 1's completion note, provenance, as delivered)
21. docs/inputs/reconciliation/C-010_STAGE1_CHATGPT_RECONCILIATION_AND_NEXT_STEP_24-OCT-2026.txt (new; ChatGPT, unchanged)
22. docs/inputs/reconciliation/C-010_STAGE1B_CHATGPT_ACCEPTED_AUTHORISATION_24-OCT-2026.txt (new; ChatGPT, unchanged)
23. docs/inputs/reconciliation/C-010_STAGE1B_CHATGPT_RECONCILIATION_24-OCT-2026.txt (new; ChatGPT, unchanged)
24. docs/inputs/reconciliation/C-010_STAGE1C_ACCEPTED_AUTHORIZATION_AND_PROMPT_24-OCT-2026.txt (new; ChatGPT, unchanged)
25. docs/inputs/reconciliation/C-010_STAGE1C_CHATGPT_RECONCILIATION_24-OCT-2026.txt (new; ChatGPT, unchanged)
26. docs/inputs/reconciliation/C-010_STAGE1D_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; ChatGPT, unchanged)
27. docs/inputs/reconciliation/C-010_STAGE1D_RECONCILIATION_AND_NEXT_STEP_24-OCT-2026.txt (new; ChatGPT, unchanged)
28. docs/inputs/reconciliation/C-010_STAGE1E_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (new; ChatGPT, unchanged)
29. docs/inputs/reconciliation/C-010_RECONCILIATION_AND_MANUAL_DRAFT_PROMPT_24-OCT-2026.txt (new; ChatGPT, unchanged)
30. docs/inputs/reconciliation/C-010_CHATGPT_MANUAL_REVIEW_NOTES_24-OCT-2026.txt (new; ChatGPT, unchanged)
31. docs/inputs/reconciliation/C-010_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (new; ChatGPT, unchanged)
32. docs/inputs/manuals/C-010_SCANNER_TO_STRATEGY_MAPPING_REV0_24-OCT-2026.md (new; Claude's revision 0, unchanged; delivered as C-010_SCANNER_TO_STRATEGY_MAPPING.md)
33. docs/inputs/manuals/C-010_SCANNER_TO_STRATEGY_MAPPING_REV1_24-OCT-2026.md (new; revision 1 as reviewed, unchanged; delivered under the same name)
34. docs/governance/V2_DECISION_REGISTER.md (DR-063 added)
35. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)

Thirty-five files. The list is closed. README.md is not changed (the agenda status row is the repository's only status index).
