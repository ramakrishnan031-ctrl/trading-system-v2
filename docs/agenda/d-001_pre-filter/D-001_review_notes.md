# D-001 PRE-FILTER: REVIEW NOTES

Status: CLOSED 2026-10-26. D-001_manual.md revision 1 is FROZEN (DR-067). Its companions are stored unchanged beside it: the Stage 2 change note and final check (revision 1) and the seven files of the Stage 1 package it cites, the provenance clarification revision 1 among them.  
Topic: D-001 Pre-filter (master agenda, section D). Scope: Option C, a boundary-map chapter (Rama's scope decision, 26-Oct-2026).

## 1. Course of the topic (25 and 26-Oct-2026, IST; each time from a file time or the file's own record)

- 18:03 to 18:06 on 25-Oct: the Section C end package was pushed by Rama (ad73016, commit time 18:03), and Claude confirmed it on GitHub (checked at 18:05). This is the D-001 base.
- 18:15: ChatGPT's Stage 0 set arrived: its Stage 0 prompt, its source review note and the Stage 0 answer sheet.
- 18:43: Claude's Stage 0 files, from path names and metadata only: the source inventory and method, the gap and boundary register, the change note and the final check (10 checks PASS).
- 18:49: ChatGPT's Stage 0 review (the inventory accepted with controlled follow-ups) and Stage 1 authorisation, with the Stage 1 prompt.
- 18:54 to 19:03: Stage 1. The read lock was lifted (LK-1, 18:54:07) and the gate test passed; 20 count-only searches with 2 positive controls, then 60 logged reads of 16 files at their pinned blobs through the masking filter; the lock was engaged again (LK-2, 19:03:40).
- 19:18: the Stage 1 package: the source evidence, the files-read log, the boundary matrix, the gap and boundary register, the change note and the final check (10 checks PASS).
- 19:52: ChatGPT's Stage 1 reconciliation review (the evidence package accepted, with one process-provenance clarification), the provenance clarification prompt and the scope decision sheet for Rama.
- 20:05: the provenance clarification, from existing records only, as the prompt required.
- 20:20: Rama's authorisation to do whatever is needed in the repositories to answer ChatGPT and finalise the files.
- 20:23: the provenance clarification, revision 1: the two host records' presence, file times and pinned blobs checked value-blind (20:21), and the filter's use of them found recorded since B-001 (its review notes, section 8) and in C-009's S1-1 results; no content of either record seen.
- 08:54 on 26-Oct: ChatGPT's plain-English scope explainer, sent by Rama with a request for Claude's opinion. Claude's reply: Option C, as the explainer recommends, with a note that D-001 be the authoritative map of the early checks (their order, hand-offs and status), the detailed rules staying with their owners.
- 09:04: Rama's selection of Option C, recorded in ChatGPT's scope decision record, with ChatGPT's Stage 2 prompt.
- 09:43: revision 0 (draft 1) with its change note and final check (11 checks PASS), after a separate reviewer's read-through (23 findings, all applied).
- 09:50: ChatGPT's review: substantially acceptable as a draft, with one required clarification (OSQ-01) and its prompt.
- 09:53: revision 1, with its change note and final check (12 checks PASS).
- 10:00: ChatGPT's final freeze review: "PASS - APPROVED FOR D-001 DOCUMENTATION FREEZE", subject to the project's normal freeze and storage procedure. This package follows it (DR-067; 37 files).

## 2. From the drafts to the frozen manual

Claude drafted both revisions with the accepted Stage 1 package as their only evidence (the Stage 1 files, the provenance clarification revision 1 and ChatGPT's Stage 1 reconciliation): revision 0 under the Stage 2 prompt and the scope decision record, revision 1 under ChatGPT's review of revision 0 and its OSQ-01 prompt. The Stage 2 prompt fixed the delivered names (the manual D-001_PRE_FILTER_BOUNDARY_MAP_MANUAL_DRAFT_26-OCT-2026.md, its change note and its final check), and each revision was delivered with its change note and final check. Both revisions are stored unchanged in docs/inputs/manuals/:

| Revision | Delivered | Answers | Lines | SHA-256 | Stored as |
|---|---|---|---|---|---|
| 0 | 09:43 | the Stage 2 prompt and the scope decision record (Option C); read by a separate reviewer before delivery | 404 | C77176180F6451EAB88A3F2CCB91779C201590E5BC90CF5DA2F0C1E91CDECFEA | D-001_PRE_FILTER_BOUNDARY_MAP_REV0_26-OCT-2026.md |
| 1 | 09:53 | ChatGPT's review of revision 0: the OSQ-01 clarification (8 governance edits) | 407 | 29A0B451DD9D2404DD7261D7362F89C23CD6845B09D5479266F0032732271748 | D-001_PRE_FILTER_BOUNDARY_MAP_REV1_26-OCT-2026.md |

- The change note and final check of revision 1 are stored beside the manual under their delivered names; those of revision 0 are stored in docs/inputs/chatgpt_originals_and_replies/ under REV0 names, as provenance. The revision 1 change note records both revisions (sections 1 to 5: revision 0; section 6: revision 1).
- **D-001_manual.md** is revision 1 with freeze edits only: the title ("DRAFT 1" removed, as at the C-008 freeze), the status line, the companion-files line and section 13.4 (412 lines).
- **Checks of the frozen manual** (document level, as for revision 1; rerun on the frozen file):
  - 148 file:line citations (115 distinct), the same list as revision 1's, each a Stage 1 citation whose lines a logged Stage 1 read printed (60 evidence rows, none mismatched);
  - 84 distinct claim IDs, all in the Stage 1 package;
  - 58 record IDs, each already in the Stage 1 package or the D-001 inputs, or new in this package (DR-067 in the register; BK-54 to BK-58 in V2_DESIGN_LOG section 7);
  - 36 quotations, each found in the Stage 1 package, the provenance revision, the Stage 2 inputs or the final freeze review after the stated normalization;
  - the 13 sections in order, with section 13.4 after 13.3;
  - ASCII only, with no CR, tab, session link or credential pattern; the value-blind identifier scan finds only known hexadecimal IDs;
  - 11 tables, well formed.
- The companions are stored as delivered, so their own status lines are as written at the time (for example, the Stage 2 change note and final check say D-001 is not accepted, frozen or published until ChatGPT's final freeze review; the Stage 1 final check and the provenance clarification say they wait for ChatGPT).

## 3. Responses to ChatGPT's reviews and decisions

- **Stage 0 prompt, source review note and answer sheet** (18:15): answered from path names and metadata only (the Stage 0 files).
- **Stage 0 review and Stage 1 authorisation** (18:49): Stage 1 kept to its file list and read modes: 16 of the 27 authorised files read in logged ranges and greps; no test, strategy YAML or Group E record opened; nothing run.
- **Stage 1 reconciliation** (19:52): the provenance question answered from existing records (20:05) and, after Rama's authorisation, in revision 1 (20:23), which the scope decision record accepts as the provenance erratum for the pass; the scope sheet answered by Rama (Option C).
- **Scope decision record and Stage 2 prompt** (09:04): revision 0 maps the three layers (Layer 1 with its strategy checks, Layer 1b, in the processor; Layer 2, the early checks inside screen(); Layer 3, the separate HardGate.evaluate battery), keeps every recorded owner, leaves OSQ-02 to OSQ-06 OPEN and OSQ-07 and VP-44 deferred, and keeps the record's explicit guards (the final check, checks 8 and 9).
- **Review of revision 0** (09:50): OSQ-01 marked RESOLVED FOR D-001 DOCUMENTATION SCOPE in the required words, reproduced in ASCII; its Stage 1 history kept; the matrix's Open column labelled historical (revision 1).
- **Final freeze review** (10:00): its conditions are met by this package: the manual and its two companions frozen as revision 1, kept together, with their hashes in DR-067; the Stage 1 package and the governance records preserved as cited; the storage-package items of the change note completed (the D-001 Stage 0 to Stage 2 inputs and files; the Section C end post-push result file and its publication note); no legacy file opened and no legacy code, test or script run. Its review limit stands as stated: the citation, claim-ID, blob and read-log checks are Claude's recorded validation, not rerun by ChatGPT.

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- **The masking filter's host-record inputs.** Stage 1's files-read log said in one sentence that the filter also reads the two host records value-blind, while its final check said 16 files were read. The provenance clarification, revision 1, established that the filter loads both records in full, in memory, on every run, that neither is a Group E, protected or never-open record, and that the design has been on record since B-001 (10-Oct). The scope decision record accepted it as the provenance erratum for the pass and set a standing rule for future passes (lesson 1).
- **A typed time.** The header of the provenance clarification, revision 1, gives 20:35 IST as its preparation time; its file time is 20:23:36 IST, and it was delivered then (section 1). The time was typed by hand, against the lesson of 2026-10-24 ("Take every time in a record from the clock or a file time, never by typing it"). The file is stored as delivered and accepted; this note is the correction, as the same lesson asks ("correct a wrong time by an added row, never by overwriting").
- **Independent read-through.** Revision 0 was read by a separate reviewer before delivery (a sub-agent, within DR-002's limit of two); its one must-fix was an owner given to the receiver's age check that no Stage 1 file records (lesson 2).
- **OSQ-01.** Revision 0 left OSQ-01 OPEN for ChatGPT to confirm, as the scope decision record did not state its status; ChatGPT confirmed it resolved for documentation scope (lesson 3).
- **No new legacy access after Stage 1.** The read lock stayed engaged from LK-2 (19:03:40 IST on 25-Oct), and the gate's read, search and lock logs are unchanged since. After the Stage 1 package (19:18), the clone was touched only by the value-blind checks of 20:21 IST, under Rama's authorisation: the two host records' presence, sizes, file times and hashes, the sparse-checkout list and HEAD, with no content shown. Nothing in the legacy repository was read, searched or counted for the manual, its revisions or this package, and no masking filter has run since the last Stage 1 read, so there is no further token-source input to disclose.

## 5. Errata to frozen manuals

None. The manual found no conflict with the frozen records it re-read at the pin (section 12.2, G-07: "No conflict found in the ranges read"), and no row of its matrix moves an owner (section 10.1).

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- **VP-44** names D-001 in its Ratify-at cell. It is not ratified, rejected or moved (OSQ-07 deferred it to the D-001 close, and ChatGPT's final freeze review keeps it deferred). It is annotated with D-001's map of the three signal-age rules (tracking only); the Ratify-at cell is unchanged.
- **Owner-cell search** (the lesson of 2026-10-25): three V2 inputs lines name D-001 (A-008_v2_design_inputs.md lines 24 and 122; C-001_v2_design_inputs.md line 34, VP-44's Ratify-at text), each together with open topics; unchanged. Before this package, only VP-44 named D-001 in the design log; no open question or bookmark did.
- **New bookmarks**, OPEN, for the open questions that point to other topics:
  - BK-54: OSQ-03 (D-002; J);
  - BK-55: OSQ-04 (D-003; BK-46);
  - BK-56: OSQ-05 (C-008 [OQ-01]; D-016; F-010);
  - BK-57: OSQ-06 (C-011; J-010);
  - BK-58: the observation G-14 (D-002; D-003).
- OSQ-02 stays OPEN in the manual only: the pre-check owners are as C-008 recorded, and bookmarks would be the mechanism only if ChatGPT wants D-001 to cover them (section 12.1).
- There is no V2 design-inputs file and no reconciliation addendum, as at C-008 to C-011.
- **Design log updates:** three lessons (section 1), the VP-44 note (section 3), the D-001 agenda status row (section 4), the session notes (section 6: the owed Section C end push line under 2026-10-25, and D-001's course under a new 2026-10-26 heading) and bookmarks BK-54 to BK-58 (section 7).
- **Owed by the Section C end package:** its push line, now recorded in V2_DESIGN_LOG section 6 in the form of the earlier push lines, and Claude's post-push result file, stored in docs/inputs/chatgpt_originals_and_replies/.

## 7. Freeze record

Prepared under W-4 (DR-017), W-9 and W-10. ChatGPT set Stage 1's scope (its Stage 0 review and Stage 1 prompt), reconciled Stage 1, recorded Rama's Option C in its scope decision record, reviewed revision 0 and passed revision 1 for the documentation freeze. Its final freeze review allows a commit or publication only under the project's agreed procedure; that procedure is W-10 (Rama's rule of 24-Oct-2026, 14:06 IST): no approval step between finalisation and storage; Claude prepares the files as one zip, Rama runs the push commands, and Claude then verifies the stored files on GitHub. The inputs received, each stored unchanged:

| File | Received (IST) | Lines | SHA-256 | In this package |
|---|---|---|---|---|
| D-001_CLAUDE_PROMPT_STAGE0_25-OCT-2026.txt | 25-Oct 18:15 | 77 | D74A12370A739DB16B6592CF3E0C19EA3050C01D2D2D5AD826A6481F32B99623 | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_CHATGPT_SOURCE_REVIEW_NOTE_25-OCT-2026.txt | 25-Oct 18:15 | 22 | 8BAC0FD835BB38A1CC3F38A12A1D1F6584733CAEE7E7BD3B0F5D47CA2C6E9550 | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_RAMA_ANSWER_SHEET_STAGE0_25-OCT-2026.txt | 25-Oct 18:15 | 32 | 2C234B01D43E5ABC4B50495962A513C7882073CA6A15789F132FBC3368DF5FDB | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_25-OCT-2026.txt | 25-Oct 18:49 | 49 | E48642B21DC2D2A61665A256DE7560625671F4CA77BB606063D5E5381A1985CC | docs/inputs/reconciliation/, unchanged |
| D-001_CLAUDE_PROMPT_STAGE1_SOURCE_EVIDENCE_25-OCT-2026.txt | 25-Oct 18:49 | 110 | 7FA59E76B7BB89D5BBB1BCA2CEEFED9D81D2D65C52BEFCBB571848AF147EC8B5 | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_CHATGPT_STAGE1_RECONCILIATION_REVIEW_25-OCT-2026.txt | 25-Oct 19:52 | 42 | 7C58BFE8D7288985C9A2516348BE9C87453E0E9D0CB7E6124A2EBFC455F37394 | docs/inputs/reconciliation/, unchanged |
| D-001_CLAUDE_PROVENANCE_CLARIFICATION_PROMPT_25-OCT-2026.txt | 25-Oct 19:52 | 25 | 0E65EA6F9B8E224E4F5E1048D02DDCFCB48A2F3CA7ADDA345A7A48DBF8036C4F | docs/inputs/reconciliation/, unchanged |
| D-001_RAMA_SCOPE_DECISION_SHEET_25-OCT-2026.txt | 25-Oct 19:52 | 40 | 1CB8318A275576101010E28A18EF65DC88D80802C1979C8AFE3DB667A21710B6 | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_RAMA_SCOPE_DECISION_EXPLAINER_PLAIN_ENGLISH_25-OCT-2026.txt | 26-Oct 08:54 | 232 | CED8084867EB8D40FE01C6EFD8CEE42444F9E84EF28956D81E28E6A29BE33F8E | docs/inputs/chatgpt_originals_and_replies/, unchanged |
| D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt | 26-Oct 09:04 | 32 | D2587788CE916C4F39BFE4268CD24388F198A896E33EB6765664BDC06BCFDEE8 | docs/inputs/reconciliation/, unchanged |
| D-001_CLAUDE_PROMPT_STAGE2_MANUAL_DRAFT_26-OCT-2026.txt | 26-Oct 09:04 | 61 | 90E18370E494BA9A4C389F788573B5DE230E0009539F42A3E1BB06B5B4F1E871 | docs/inputs/reconciliation/, unchanged |
| D-001_CHATGPT_REVIEW_REQUIRED_CLARIFICATION_26-OCT-2026.txt | 26-Oct 09:50 | 48 | 2C87AB85A4AE0FB8EA629021C03D87F7D64EA95B9DAFA989D9759EA8B6EE366F | docs/inputs/reconciliation/, unchanged |
| D-001_CLAUDE_PROMPT_OSQ01_CLARIFICATION_26-OCT-2026.txt | 26-Oct 09:50 | 34 | 16DD60AFAE253808CF4E081F05DC4A8BA8C2DD73F3057B459239EAAACFB9449F | docs/inputs/reconciliation/, unchanged |
| D-001_CHATGPT_FINAL_FREEZE_REVIEW_26-OCT-2026.txt | 26-Oct 10:00 | 35 | 6D0F853E89C4D0E4AF0B33D5A98E006B614D6F3DF9866743247F9C1D7D57309D | docs/inputs/reconciliation/, unchanged |

Hand-off zips, not stored (each holds the named files):

- D-001_OPTION_C_STAGE2_HANDOFF_26-OCT-2026.zip (SHA-256 17AF54EEBE32D18C68A43269384E452B71729BF3099FCFBE7540C6143D714312): the scope decision record and the Stage 2 prompt, byte for byte.
- D-001_CHATGPT_REVIEW_HANDOFF_26-OCT-2026.zip (SHA-256 2E33EF4BF6D293439432E1392E39131A8B94C6A26792E27D8E6B0D7F2FFD6464): the review of revision 0 and the OSQ-01 prompt, byte for byte.
- D-001_FINAL_FREEZE_REVIEW_HANDOFF_26-OCT-2026.zip (SHA-256 5B1C139A37F9E91068AA555BF4C704FD21E0BDD41C4D2CDF4E42B4AF56457981): the final freeze review, byte for byte.

The freeze takes effect when Rama runs PART 2 of the push commands (D-001_PUSH_COMMANDS_26-OCT-2026.txt). Claude then confirms on GitHub that the stored files equal the manifest. The record of the push is stored with the next package, as the Section C end push is recorded with this one. Claude stores, commits and pushes nothing.

## 8. Package

1. docs/agenda/d-001_pre-filter/D-001_manual.md (new; FROZEN, revision 1 with freeze edits only)
2. docs/agenda/d-001_pre-filter/D-001_review_notes.md (new; this file)
3. docs/agenda/d-001_pre-filter/D-001_STAGE2_CHANGE_NOTE_26-OCT-2026.txt (new; Claude, revision 1, companion, as delivered)
4. docs/agenda/d-001_pre-filter/D-001_STAGE2_FINAL_CHECK_26-OCT-2026.txt (new; as 3)
5. docs/agenda/d-001_pre-filter/D-001_STAGE1_SOURCE_EVIDENCE_25-OCT-2026.txt (new; Claude, the Stage 1 package, companion, as delivered)
6. docs/agenda/d-001_pre-filter/D-001_STAGE1_FILES_READ_LOG_25-OCT-2026.txt (new; as 5)
7. docs/agenda/d-001_pre-filter/D-001_STAGE1_BOUNDARY_MATRIX_25-OCT-2026.txt (new; as 5)
8. docs/agenda/d-001_pre-filter/D-001_STAGE1_GAP_AND_BOUNDARY_REGISTER_25-OCT-2026.txt (new; as 5)
9. docs/agenda/d-001_pre-filter/D-001_STAGE1_CHANGE_NOTE_25-OCT-2026.txt (new; as 5)
10. docs/agenda/d-001_pre-filter/D-001_STAGE1_FINAL_CHECK_25-OCT-2026.txt (new; as 5)
11. docs/agenda/d-001_pre-filter/D-001_STAGE1_PROVENANCE_CLARIFICATION_REV1_25-OCT-2026.txt (new; Claude, the erratum to the Stage 1 read count, companion, as delivered)
12. docs/inputs/chatgpt_originals_and_replies/D-001_CLAUDE_PROMPT_STAGE0_25-OCT-2026.txt (new; ChatGPT, unchanged)
13. docs/inputs/chatgpt_originals_and_replies/D-001_CHATGPT_SOURCE_REVIEW_NOTE_25-OCT-2026.txt (new; ChatGPT, unchanged)
14. docs/inputs/chatgpt_originals_and_replies/D-001_RAMA_ANSWER_SHEET_STAGE0_25-OCT-2026.txt (new; ChatGPT, unchanged)
15. docs/inputs/chatgpt_originals_and_replies/D-001_SOURCE_INVENTORY_AND_METHOD_25-OCT-2026.txt (new; Claude, Stage 0, as delivered)
16. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE0_GAP_AND_BOUNDARY_REGISTER_25-OCT-2026.txt (new; as 15)
17. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE0_CHANGE_NOTE_25-OCT-2026.txt (new; as 15)
18. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE0_FINAL_CHECK_25-OCT-2026.txt (new; as 15)
19. docs/inputs/chatgpt_originals_and_replies/D-001_CLAUDE_PROMPT_STAGE1_SOURCE_EVIDENCE_25-OCT-2026.txt (new; ChatGPT, unchanged)
20. docs/inputs/chatgpt_originals_and_replies/D-001_RAMA_SCOPE_DECISION_SHEET_25-OCT-2026.txt (new; ChatGPT, unchanged)
21. docs/inputs/chatgpt_originals_and_replies/D-001_RAMA_SCOPE_DECISION_EXPLAINER_PLAIN_ENGLISH_25-OCT-2026.txt (new; ChatGPT, unchanged)
22. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE1_PROVENANCE_CLARIFICATION_25-OCT-2026.txt (new; Claude, the first provenance clarification, as delivered; superseded by its revision 1 (11))
23. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE2_CHANGE_NOTE_REV0_26-OCT-2026.txt (new; Claude, revision 0's change note, provenance, as delivered as D-001_STAGE2_CHANGE_NOTE_26-OCT-2026.txt)
24. docs/inputs/chatgpt_originals_and_replies/D-001_STAGE2_FINAL_CHECK_REV0_26-OCT-2026.txt (new; Claude, revision 0's final check, provenance, as delivered as D-001_STAGE2_FINAL_CHECK_26-OCT-2026.txt)
25. docs/inputs/chatgpt_originals_and_replies/SECTION-C_END_POST_PUSH_GITHUB_VERIFICATION_RESULT_25-OCT-2026.txt (new; Claude, the Section C end post-push result, as delivered (owed by that package))
26. docs/inputs/reconciliation/D-001_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_25-OCT-2026.txt (new; ChatGPT, unchanged)
27. docs/inputs/reconciliation/D-001_CHATGPT_STAGE1_RECONCILIATION_REVIEW_25-OCT-2026.txt (new; ChatGPT, unchanged)
28. docs/inputs/reconciliation/D-001_CLAUDE_PROVENANCE_CLARIFICATION_PROMPT_25-OCT-2026.txt (new; ChatGPT, unchanged)
29. docs/inputs/reconciliation/D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt (new; ChatGPT, unchanged)
30. docs/inputs/reconciliation/D-001_CLAUDE_PROMPT_STAGE2_MANUAL_DRAFT_26-OCT-2026.txt (new; ChatGPT, unchanged)
31. docs/inputs/reconciliation/D-001_CHATGPT_REVIEW_REQUIRED_CLARIFICATION_26-OCT-2026.txt (new; ChatGPT, unchanged)
32. docs/inputs/reconciliation/D-001_CLAUDE_PROMPT_OSQ01_CLARIFICATION_26-OCT-2026.txt (new; ChatGPT, unchanged)
33. docs/inputs/reconciliation/D-001_CHATGPT_FINAL_FREEZE_REVIEW_26-OCT-2026.txt (new; ChatGPT, unchanged)
34. docs/inputs/manuals/D-001_PRE_FILTER_BOUNDARY_MAP_REV0_26-OCT-2026.md (new; Claude's revision 0, unchanged; delivered as D-001_PRE_FILTER_BOUNDARY_MAP_MANUAL_DRAFT_26-OCT-2026.md)
35. docs/inputs/manuals/D-001_PRE_FILTER_BOUNDARY_MAP_REV1_26-OCT-2026.md (new; revision 1 as reviewed, unchanged; delivered under the same name)
36. docs/governance/V2_DECISION_REGISTER.md (DR-067 added)
37. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)

Thirty-seven files. The list is closed. README.md is not changed (the agenda status row is the repository's only status index).
