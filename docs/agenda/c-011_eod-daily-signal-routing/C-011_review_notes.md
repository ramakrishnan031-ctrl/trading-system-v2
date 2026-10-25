# C-011 EOD / DAILY SIGNAL ROUTING: REVIEW NOTES

Status: CLOSED 2026-10-25. C-011_manual.md revision 5 is FROZEN (DR-064). Its companions are stored unchanged beside it: the Stage 1 evidence sheet, the evidence and gap check (revision 5) and the revision 5 change note.  
Topic: C-011 EOD / Daily signal routing (master agenda, section C).

## 1. Course of the topic (25-Oct-2026, IST; the file time of each file received or delivered)

- 08:16 to 08:19: C-010 was pushed by Rama (9b4cb68), and Claude confirmed it on GitHub. This is the C-011 base.
- 08:46: ChatGPT's Stage 0 set arrived (C-011_STAGE0_CLAUDE_RESEARCH_PROMPT_25-OCT-2026.txt, C-011_STAGE0_ANSWER_SHEET_25-OCT-2026.txt).
- 09:20: Claude's Stage 0 answers, from path names and metadata only: 61 inventory rows covering 161 files, a Stage 1 plan and seven questions (OQ-01 to OQ-07, of which OQ-01 to OQ-03 blocking).
- 11:24: Rama forwarded ChatGPT's Stage 1 prompt (C-011_STAGE1_CLAUDE_PROMPT_DRAFT_25-OCT-2026.txt) with a screenshot of ChatGPT's Stage 0 reply, which recommended option (a) of OQ-01 to OQ-03 and asked Rama to confirm them. ChatGPT's decision sheet itself was not attached. At 11:26 Claude told Rama it was treating the forwarding as approval (section 4).
- 11:28 to 11:36: Stage 1. W1 ran S1-1 (Q01 to Q32, 32 count-only searches, with the positive controls Q33 to Q36); W2 ran S1-2 (reads 001 to 020 of 15 files, 340 printed lines, planned at 388 under the 400-line stop). The read lock was engaged again after each window.
- 11:48: the Stage 1 evidence sheet.
- 12:05: ChatGPT's Stage 1 reconciliation and the manual-drafting prompt.
- 12:45: revision 0 and its evidence and gap check.
- 12:52: ChatGPT's draft review (a targeted revision: qualify the drafting interpretations; BK-08 and BK-30 open; VP-14 not ratified; the Stage 0 sheet needed). 13:04: revision 1.
- 13:10: ChatGPT's N-1 check of the database duplicate rule. 13:22: revision 2 and its change note.
- 13:27: ChatGPT's reply that it had seen only a screenshot; the revision 2 files were re-sent unchanged.
- 13:34: ChatGPT's final verification: N-1 accepted; the OQ-01 to OQ-03 authorization record to be reconciled.
- 13:57: revision 3: no explicit confirmation by Rama in the conversation record up to 13:34, so the authorization was recorded as UNKNOWN / NOT ESTABLISHED, and his decision and the Stage 0 sheet were requested.
- 14:28: Rama's "Approved", sent with ChatGPT's governance reconciliation, which records his confirmation in ChatGPT ("1. I'm okay 2. Files attached"). 14:38: revision 4, recording the confirmation as a later event.
- 14:44: ChatGPT's review of revision 4: acceptable; the Stage 0 sheet to be attached to Claude. 14:50: the Stage 0 decision sheet received. 14:58: revision 5, recording its receipt and check.
- 15:07: ChatGPT's final review of revision 5: "ACCEPTED FOR FINAL STORAGE-PACKAGE PREPARATION". This package follows it.

## 2. From the drafts to the frozen manual

Claude drafted every revision from the Stage 1 evidence sheet and ChatGPT's reconciliation, under the drafting prompt, which fixed the delivered name C-011_EOD_DAILY_SIGNAL_ROUTING_DRAFT.md. Each revision answered one ChatGPT review and was delivered with its evidence and gap check; from revision 2 a change note went with it. Every revision is stored unchanged in docs/inputs/manuals/:

| Revision | Delivered | Answers | Lines | SHA-256 | Stored as |
|---|---|---|---|---|---|
| 0 | 12:45 | the drafting prompt and the Stage 1 reconciliation | 984 | 0EF25850640561BF845593265CC025942F42C8F4F85776A7C728750F8D326BBE | C-011_EOD_DAILY_SIGNAL_ROUTING_REV0_25-OCT-2026.md |
| 1 | 13:04 | the draft review: DI-01 conditional, DI-02 bounded, labels at every use, the drafting unknowns kept apart | 991 | 6216124FE3CD57904ECFE043BA9B0EFBC1CFA16A99E61A8527C1B43887712C4A | C-011_EOD_DAILY_SIGNAL_ROUTING_REV1_25-OCT-2026.md |
| 2 | 13:22 | N-1: the duplicate rule's source text FACT, its skip effect INTERPRETATION (DI-10) | 999 | 896A36583DCD5F25B594A946E6A9E3CB23F743691F1FC3A00AED25B4E67F2E93 | C-011_EOD_DAILY_SIGNAL_ROUTING_REV2_25-OCT-2026.md |
| 3 | 13:57 | the final verification: the OQ-01 to OQ-03 authorization record | 1,008 | 2F43B00AE149E746D4E2924C06B466A137E3B4F5BAF19DF3912E74C9F9785429 | C-011_EOD_DAILY_SIGNAL_ROUTING_REV3_25-OCT-2026.md |
| 4 | 14:38 | the governance reconciliation: Rama's confirmation recorded | 1,005 | A102DB39B3C5BA0D573F66EFC9DDA5F2BC19E11A5AFA0935269755E213940100 | C-011_EOD_DAILY_SIGNAL_ROUTING_REV4_25-OCT-2026.md |
| 5 | 14:58 | the review of revision 4: the Stage 0 sheet's receipt and check | 1,006 | 99A3C3611BB2161FEB4693D1E9608B0E011C33E28FDB7DE25272F8A333258197 | C-011_EOD_DAILY_SIGNAL_ROUTING_REV5_25-OCT-2026.md |

- The evidence and gap check of revision 5 is stored beside the manual under its delivered name; its revisions 0 to 4 are stored in docs/inputs/chatgpt_originals_and_replies/ under REV0 to REV4 names, with the change notes of revisions 2 to 4, as provenance. Each one's SHA-256 is in the companion's header and in the package manifest. The companion's parts H and J to M list every change of revisions 1 to 5.
- **C-011_manual.md** is revision 5 with freeze edits only: the status line, the companion-files line and one added bullet at the end of section 18 (1,007 lines).
- **Checks of the frozen manual** (document level, as for revision 5):
  - 179 file:line citations, each a line printed by a Stage 1 read;
  - 62 excerpt lines equal to their printed lines after the stated normalization;
  - 161 transcribed passages, 158 found in the printed text or the governance records and 3 marked illustrations;
  - the 43 frozen-manual pairs all among the evidence sheet's 44;
  - every evidence ID found in the evidence files, and the new bookmark IDs BK-51 to BK-53 defined in V2_DESIGN_LOG section 7;
  - ASCII only, with no CR, session link or credential pattern;
  - the value-blind identifier scan finds only known hexadecimal IDs;
  - tables well formed.
- The companions are stored as delivered, so their own status lines are as written at the time (the evidence sheet says it stopped for ChatGPT's reconciliation; the evidence and gap check and the change note say DRAFT ONLY).

## 3. Responses to ChatGPT's reviews and decisions

- **Stage 0 decision sheet** (received by Claude only at 14:50): its recommendations for OQ-01 to OQ-03 are the options (a) that Stage 1 and every revision used; its positions on OQ-04 to OQ-07 agree with the manual (the companion, part E). Its status line, "Stage 1 NOT YET AUTHORISED BY RAMA", is its state when written.
- **Stage 1 prompt** (11:24): run as written (S1-1, then S1-2 inside the line budget).
- **Stage 1 reconciliation and drafting prompt** (12:05): applied in revision 0; the mandatory precision points are mapped in the companion's part A.
- **Draft review** (12:52): P-1 to P-6 applied in revision 1 (the companion, part H).
- **N-1 check** (13:10): applied in revision 2 (the companion, parts I and J).
- **Reply on the screenshot** (13:27): the three revision 2 files re-sent unchanged.
- **Final verification** (13:34): the authorization record reconciled in revision 3 (part K).
- **Governance reconciliation** (14:28): Rama's confirmation recorded in revision 4 as a later event, with revision 3's finding kept as history (part L).
- **Review of revision 4** (14:44): the Stage 0 sheet's receipt and the targeted check recorded in revision 5 (part M).
- **Final review of revision 5** (15:07): applied in this package: the zip and manifest; the manual, the companion, the revision 5 change note and the Stage 0 sheet stored unchanged; the archive checked against the manifest after packaging; the package, manifest, check result and push commands returned for Rama.

## 4. Process record (Claude's side; lessons in V2_DESIGN_LOG section 1)

- **Approval reading (11:26).** ChatGPT's Stage 0 reply asked Rama to confirm OQ-01 to OQ-03 before the Stage 1 prompt was used. Rama forwarded the prompt with no words of his own, and Claude took the forwarding as approval under his no-approval rule (W-10), telling him so and stating it in the evidence sheet's header. ChatGPT's final verification found the confirmation not established; revision 3 recorded that, and revision 4 recorded Rama's explicit confirmation as a later event. The Stage 1 work itself followed the recommended options, which Rama then confirmed without amendment.
- **The Stage 0 sheet.** It was attached only in ChatGPT. Claude named it as missing in its Stage 1 reply and in later replies, each time inside a longer message; it reached Claude at 14:50.
- **N-1.** Revisions 0 and 1 stated a database effect (a duplicate skipped) as FACT; revision 2 kept FACT for the source text only.
- **Labels.** The draft review asked for the drafting interpretations to be labelled at every material use; a per-line check was added to Claude's checker.
- **Wording source.** In revision 2, one documentation page of the SQLite project was read for the wording of the ON CONFLICT clause only; it is not cited in the manual and establishes nothing about the legacy database.
- **No new access after Stage 1.** The read lock stayed engaged from LK-4 (11:36:13 IST) to the end, and Claude's read, search and lock logs were unchanged at each later check. Nothing in the legacy repository was read, searched or counted for the manual, its revisions or this package.

## 5. Errata to frozen manuals

None. No frozen statement conflicts with the C-011 evidence (C-011_manual.md section 13). RC-06, the heartbeat per EOD alert of C-002 10.7, is a precision recorded in C-011 only (ChatGPT's Stage 1 reconciliation), and is tracked as bookmark BK-53.

## 6. V2 governance at this freeze (tracking only; no V2 decision)

- **VP-14** names C-011 in its Ratify-at cell. It is not ratified here (Stage 0 OQ-04, as the Stage 0 decision sheet recommends; ChatGPT's draft review, P-3). It is annotated with the C-011 facts that bear on it; the cell is unchanged.
- **BK-08 and BK-30** name C-011. Each is annotated with the evidence (sections 10.4 and 5.3 of the manual) and stays OPEN (Stage 0 OQ-05).
- **Q-44 (a)**, which PB-01 workers ran on the production host, is unchanged: a DR-016 audit lead for topic K-009 (Stage 0 OQ-07).
- **New bookmarks**, OPEN:
  - BK-51: the KNOWN DEFECT of section 14.1 (the stale pointer in config/expected_managers.yaml);
  - BK-52: the OPEN QUESTION G-06 of sections 11.4 and 14.3 (capture_fetch_lookback_days);
  - BK-53: the precision note RC-06 of section 9.5 (against C-002 10.7).
- There is no V2 design-inputs file and no reconciliation addendum, as at C-008 to C-010.
- **Design log updates:** four lessons (section 1), the VP-14 note, the BK-08 and BK-30 notes, the agenda status row, the session notes of 2026-10-25 (the owed record of the C-010 push, and C-011's course) and bookmarks BK-51 to BK-53.
- **Owed by the C-010 package:** the C-010 push line, now recorded verbatim in V2_DESIGN_LOG section 6.

## 7. Freeze record

Prepared under W-4 (DR-017), W-9 and W-10. ChatGPT set the Stage 1 scope through its Stage 0 decision sheet and Stage 1 prompt, with OQ-01 to OQ-03 confirmed by Rama (recorded in revision 4); it reconciled Stage 1, reviewed every revision and accepted revision 5 for the final storage package. Rama's rule of 24-Oct-2026 (14:06 IST, W-10) removes any approval step between finalisation and storage. The inputs of the freeze are unchanged since they were received:

| File | Lines | SHA-256 | In this package |
|---|---|---|---|
| C-011_MANUAL_DRAFTING_PROMPT_25-OCT-2026.txt | 59 | 2E6D873795F86D766D68C82380BB1C61041961304CB547BB1768FDA43F2BE8E1 | docs/inputs/reconciliation/, unchanged |
| C-011_STAGE0_RECONCILIATION_AND_DECISIONS_25-OCT-2026.txt | 46 | 812FA842FC9AFAE572F7657249EF3F94A370F97691423406D1A2F63BE1FBF61F | docs/inputs/reconciliation/, unchanged |
| C-011_CHATGPT_FINAL_REVIEW_REV4_25-OCT-2026.txt | 29 | D50E9EDFB08FED31938A36CE347B7CB1D4515F1A6D753CF08A2F39C61DD23698 | docs/inputs/reconciliation/, unchanged |
| C-011_CHATGPT_FINAL_REVIEW_REV5_AND_PACKAGE_GO-AHEAD_25-OCT-2026.txt | 28 | B58F5B91E2CB09C53F56E6C37FFA3C79B92C52782F3C60F02FCB2B38E2505A57 | docs/inputs/reconciliation/, unchanged |

The freeze takes effect when Rama runs PART 2 of the push commands (C-011_PUSH_COMMANDS_25-OCT-2026.txt). Claude then confirms on GitHub that the stored files equal the manifest. The record of the push is stored with the next package, as the C-010 push is recorded with this one. Claude stores, commits and pushes nothing.

## 8. Package

1. docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md (new; FROZEN, revision 5)
2. docs/agenda/c-011_eod-daily-signal-routing/C-011_review_notes.md (new; this file)
3. docs/agenda/c-011_eod-daily-signal-routing/C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt (new; Claude, reads 001 to 020, companion, as delivered)
4. docs/agenda/c-011_eod-daily-signal-routing/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK.txt (new; Claude, revision 5, companion, as delivered)
5. docs/agenda/c-011_eod-daily-signal-routing/C-011_DRAFT_REVISION_5_CHANGE_NOTE_25-OCT-2026.txt (new; Claude, companion, as delivered)
6. docs/inputs/chatgpt_originals_and_replies/C-011_STAGE0_CLAUDE_RESEARCH_PROMPT_25-OCT-2026.txt (new; ChatGPT, unchanged)
7. docs/inputs/chatgpt_originals_and_replies/C-011_STAGE0_ANSWER_SHEET_25-OCT-2026.txt (new; ChatGPT, unchanged)
8. docs/inputs/chatgpt_originals_and_replies/C-011_CLAUDE_STAGE0_ANSWERS_25-OCT-2026.txt (new; Claude, as delivered)
9. docs/inputs/chatgpt_originals_and_replies/C-011_STAGE1_CLAUDE_PROMPT_DRAFT_25-OCT-2026.txt (new; ChatGPT, unchanged)
10. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK_REV0_25-OCT-2026.txt (new; Claude, provenance, as delivered under the companion's name)
11. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK_REV1_25-OCT-2026.txt (new; as 10)
12. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK_REV2_25-OCT-2026.txt (new; as 10)
13. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK_REV3_25-OCT-2026.txt (new; as 10)
14. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_EVIDENCE_AND_GAP_CHECK_REV4_25-OCT-2026.txt (new; as 10)
15. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_REVISION_2_CHANGE_NOTE_25-OCT-2026.txt (new; Claude, provenance, as delivered)
16. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_REVISION_3_CHANGE_NOTE_25-OCT-2026.txt (new; as 15)
17. docs/inputs/chatgpt_originals_and_replies/C-011_DRAFT_REVISION_4_CHANGE_NOTE_25-OCT-2026.txt (new; as 15)
18. docs/inputs/reconciliation/C-011_STAGE0_RECONCILIATION_AND_DECISIONS_25-OCT-2026.txt (new; ChatGPT, unchanged)
19. docs/inputs/reconciliation/C-011_STAGE1_CHATGPT_RECONCILIATION_25-OCT-2026.txt (new; ChatGPT, unchanged)
20. docs/inputs/reconciliation/C-011_MANUAL_DRAFTING_PROMPT_25-OCT-2026.txt (new; ChatGPT, unchanged)
21. docs/inputs/reconciliation/C-011_CHATGPT_DRAFT_REVIEW_25-OCT-2026.txt (new; ChatGPT, unchanged)
22. docs/inputs/reconciliation/C-011_CHATGPT_VALUE_ADDED_REPLY_TO_CLAUDE_25-OCT-2026.txt (new; ChatGPT, unchanged)
23. docs/inputs/reconciliation/C-011_CHATGPT_VALUE_ADDED_REPLY_TO_CLAUDE_25-OCT-2026_REV2.txt (new; ChatGPT, unchanged)
24. docs/inputs/reconciliation/C-011_CHATGPT_FINAL_VERIFICATION_VALUE_ADDED_REPLY_25-OCT-2026.txt (new; ChatGPT, unchanged)
25. docs/inputs/reconciliation/C-011_CHATGPT_GOVERNANCE_RECONCILIATION_FOR_CLAUDE_REV4_25-OCT-2026.txt (new; ChatGPT, unchanged)
26. docs/inputs/reconciliation/C-011_CHATGPT_FINAL_REVIEW_REV4_25-OCT-2026.txt (new; ChatGPT, unchanged)
27. docs/inputs/reconciliation/C-011_CHATGPT_FINAL_REVIEW_REV5_AND_PACKAGE_GO-AHEAD_25-OCT-2026.txt (new; ChatGPT, unchanged)
28. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV0_25-OCT-2026.md (new; Claude's revision 0, unchanged; delivered as C-011_EOD_DAILY_SIGNAL_ROUTING_DRAFT.md)
29. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV1_25-OCT-2026.md (new; revision 1, unchanged; delivered under the same name)
30. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV2_25-OCT-2026.md (new; revision 2, as 29)
31. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV3_25-OCT-2026.md (new; revision 3, as 29)
32. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV4_25-OCT-2026.md (new; revision 4, as 29)
33. docs/inputs/manuals/C-011_EOD_DAILY_SIGNAL_ROUTING_REV5_25-OCT-2026.md (new; revision 5 as reviewed, unchanged; as 29)
34. docs/governance/V2_DECISION_REGISTER.md (DR-064 added)
35. docs/governance/V2_DESIGN_LOG.md (updated as in section 6)

Thirty-five files. The list is closed. README.md is not changed (the agenda status row is the repository's only status index).
