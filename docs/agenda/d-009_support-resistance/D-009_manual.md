# D-009 - SUPPORT & RESISTANCE (V2 TOPIC MANUAL: DRAFT RECORDED - NO REQUIREMENT DECIDED)

## 1. Document status, purpose and scope

- Status: DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076); OQ9-01 to OQ9-25 OPEN. The second Route B topic (DR-074, W-11); no legacy manual. As D-009_v2_requirements.md records it (lines 5 to 8): "Status: DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076), by the storage action that stores this file, after ChatGPT's final review of revision 1 ... NOT FROZEN. NO D-009 REQUIREMENT IS DECIDED in this record." Its requirement areas RQ9-01 to RQ9-11 stay "RAW IDEA, PROVISIONAL WORKING POSITION, OPEN / UNRESOLVED or NOT DECIDED" (section 7 of that record).
- This is a V2 topic manual (DR-083), written in the documentation batch of 31-Oct-2026 (DR-084). It explains the topic and its records. D-009_v2_requirements.md is authoritative for requirements and decisions; this manual decides nothing, changes no status and adds no requirement.
- Purpose: to explain, from the stored V2 records, what D-009 asks; how it was run on 29 and 30-Oct-2026 and who did what; what Rama answered, which answers adopt ChatGPT's suggested wording, and why his answers stay raw ideas; why the record decides no requirement; what stays open and with whom; which legacy facts the topic cites through its Stage 1 evidence sheet and which it could not establish; and where its boundaries, dependencies and later downstream records lie.
- Basis: the V2 repository at 6bd80a5 (GitHub main), read only:
  - the 14 files of docs/agenda/d-009_support-resistance/ and the 23 files in docs/inputs/ whose names carry D-009 or D009 (docs/inputs/legacy_baseline/ not used, as Claude reports; see PD-01 in the batch decision record);
  - the register rows that name D-009 (DR-031, DR-069, DR-076, DR-077, DR-079, DR-081, DR-083) and the status rows DR-072 to DR-075;
  - V2_DESIGN_LOG.md (sections 1 and 4 to 8), the Route B change record (docs/agenda/route-b/V2_REQUIREMENTS_FIRST_GOVERNANCE_CHANGE_RECORD_28-OCT-2026.txt, section 5) and the Master Redesign Agenda (docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt);
  - the frozen manuals and V2 design-input files only as the D-009 records cite them: the quoted strings were checked by text search at the cited lines, and nothing else in them was used.
- Line numbers are those of the files at 6bd80a5 (the documentation batch's commit moves V2_DESIGN_LOG lines 195 to 393 down by 2, lines 394 to 619 by 3 and later lines by 5). The D-009 records cite V2_DESIGN_LOG.md lines at 8e60e9f, before the log grew (VP-08 was then line 243 and is now line 259; Q-35 218 and 234; Q-44 227 and 243; VP-44 278 and 294); the row IDs are the same.
- In scope, as recorded (D-009_v2_requirements.md section 1):
  - FACT (V2 RECORD): "D-009 has a title and the Section D goal only." "The agenda gives D-009 no separate scope text (OQ9-01)."
  - PROVISIONAL (a PROVISIONAL WORKING POSITION, "not frozen"; ChatGPT's proposal in the guide, which Rama asked for, "as per your best opinion now", guide lines 102 and 110): "D-009 holds whether S&R is used in V2's trade decision and its high-level role. Zones go to D-010, strength to D-011 and freshness to D-012. Detailed entry use goes to D-013 to D-016, stop and target mechanics to the named E and J topics, and the legacy engines' disposition to J (OQ9-01 to OQ9-03)."
- Out of scope, as recorded (sections 1 and 11 of the record):
  - "The boundary between D-009 and the legacy S&R engine stays unresolved. D-009 keeps, modifies or revives nothing of it, and no frozen manual establishes what it did at runtime (section 4)."
  - "No design is chosen: no S&R definition, calculation, method, point or zone representation, timeframe or hierarchy, selection rule, buffer, threshold, SL or target formula, R:R rule, gate, score, shadow phase, data rule, schema or storage."
  - "No legacy-source access (DR-073). NR9-01 to NR9-14 are not researched by Route B." "No D-006, D-007, D-010 to D-018, E or J work is started by this record."
- This manual is not a legacy manual (W-11: "no new legacy manual"; DR-083: "not a reconstructed legacy manual") and not a second requirements record. Where it gives a status or a requirement area, it quotes the requirements record.

## 2. Records and how to read them

The topic folder (14 files, all stored by the D-009 package, commit 9c9c2e9):

| File | What it is | Status or revision |
|---|---|---|
| D-009_v2_requirements.md | the Route B requirements record (DR-074's output file): scope (section 1), requirement areas RQ9-01 to RQ9-11 (section 2), Rama's 17 answer lines verbatim (2.1), how the raw inputs relate and the preserved conflicts (3), legacy evidence and NR9-01 to NR9-14 (4), Rama's earlier statements (5), V2 rows (6), outcome (7), decision record (8), OQ9-01 to OQ9-25 (9), boundaries (10), course (12) | revision 1; only its status lines and section 12 brought up to date at storage (10:03 IST, 30-Oct; the manifest's notes list them); DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076); NOT FROZEN; revision 0 stored unchanged in docs/inputs/manuals/ |
| D-009_STAGE0_SOURCE_INVENTORY_AND_RESEARCH_PLAN_29-OCT-2026.txt | Stage 0 output 1: permitted records U-1 to U-6, preconditions P-1 to P-4, the locator method, the inventory summary, the proposed Stage 1 (S1-Q1 to S1-Q7) | "LIMITED / INCOMPLETE: A LOCATOR-ONLY INVENTORY"; ChatGPT: "STAGE 0 ACCEPTED FOR PLANNING PURPOSES" (14:33 IST) |
| D-009_STAGE0_CANDIDATE_SOURCE_REGISTER_29-OCT-2026.txt | Stage 0 output 2: candidate locators R9-01 to R9-61 (Groups A to C), V2 rows V9-01 to V9-13, design inputs I9-01 to I9-06, D-008 rows P9-01 to P9-04, hits set aside X9-01 to X9-04, agenda text M9-01 to M9-08 | "CANDIDATE LOCATORS ONLY. NO PASSAGE TEXT WAS READ; NOTHING HERE IS A FINDING." |
| D-009_STAGE0_OPEN_QUESTIONS_AND_SCOPE_BOUNDARY_29-OCT-2026.txt | Stage 0 output 3: the scope boundary and OQ9-01 to OQ9-12, each with Claude's proposal (INTERPRETATION; not a decision) | "PLANNING ONLY" |
| D-009_STAGE1_EVIDENCE_SHEET_29-OCT-2026.txt | Stage 1 output 1: frozen-manual statements with their qualifiers and Rama's earlier statements (E9-01 to E9-39), V2 rows, NR9-01 to NR9-14, recorded boundaries B9-01 to B9-11, wording notes W9-01 to W9-05 | "EVIDENCE ONLY"; ChatGPT: "ACCEPTED WITH RECORDED PROCESS DEVIATIONS, FOR EVIDENCE-REVIEW PURPOSES ONLY" (15:29 IST) |
| D-009_STAGE1_READ_LOG_29-OCT-2026.txt | Stage 1 output 2: reads S1-01 to S1-89, re-reads RR-01 to RR-30, locators L1-01 and L1-02; deviations DV-01 and DV-02 and method notes M-01 to M-04 | as the evidence sheet |
| D-009_STAGE1_CANDIDATE_RAMA_ANSWER_SHEET_QUESTIONS_29-OCT-2026.txt | Stage 1 output 3: candidate questions CQ9-01 to CQ9-14 for ChatGPT's answer sheet, which it "may reword, merge, drop or reorder" | "CANDIDATE QUESTIONS ONLY. THIS IS NOT THE ANSWER SHEET." |
| D-009_CLAUDE_REVIEW_NOTES_ON_RAMA_RAW_IDEAS_29-OCT-2026.txt | Claude's notes on Rama's answers and on ChatGPT's register, revision 0: provenance, checks C-1 to C-7 on the examples, points P-1 to P-6 | 19:35 IST, 29-Oct; ChatGPT: "ACCEPT CLAUDE'S REVIEW NOTES AS USEFUL REVIEW INPUT" |
| D009_RAW_IDEAS_STATUS_AND_V2_DESIGN_DISCUSSION_REGISTER_REV1_29-OCT-2026.txt | the raw-ideas register: ChatGPT's revision 0 with its corrections A to F applied by Claude, and correction CR-1; items 1 to 16 (paraphrases), ambiguities A to J, dependencies, a future workflow | revision 1 with CR-1, "_REV1" added on storage (PKG9-Q6); ChatGPT: "ACCEPTED FOR CARRY-FORWARD TO THE D-009 STORAGE PACKAGE." (20:38 IST) |
| D-009_RAW_IDEAS_REGISTER_REV1_CHANGE_LOG_29-OCT-2026.txt | the register's change log, CL-00 to CL-29 | accepted with the register |
| D-009_RAW_IDEAS_REGISTER_REV1_PROVENANCE_CHECKLIST_29-OCT-2026.txt | checks 1 to 9: the guide not edited, no raw answer silently corrected, quotations exact, ChatGPT's wording not presented as Rama's, nothing decided, limits | written 19:45 to 19:48 IST; it "needed no change" for CR-1 (change log, closing note) |
| D-009_STORAGE_PLAN_30-OCT-2026.txt | the storage plan: rows 1 to 38, the proposed DR-076 and design-log entries E-1 to E-7, ChatGPT's PKG9-Q1 to PKG9-Q8 decisions, the storage checks | revision 1 as accepted, renamed only; its status lines describe it before storage ("DRAFT, REVISION 1, FOR CHATGPT'S REVIEW") |
| D-009_PACKAGE_CHANGE_LOG_30-OCT-2026.txt | the package change log: PC-01 to PC-16, PR-01 to PR-07, the revision 1 corrections, the PKG9 decisions, WC-01 to WC-05, UO-01 to UO-15 | as the storage plan |
| D-009_STORAGE_MANIFEST_30-OCT-2026.txt | the package manifest: the 37 other committed files with SHA-256 and bytes, and notes on row 1's storage edits | written at storage; its own SHA-256 is in the post-push record (SP-C1) |

Inputs in docs/inputs/ (23 files whose names carry D-009 or D009, stored unchanged under W-3; ChatGPT's files under their received names without the upload prefix):
- docs/inputs/reconciliation/ (19 files):
  - start and Stage 0: CHATGPT_REVIEW_D008_POST_PUSH_AND_D009_NEXT_STEP_29-OCT-2026.txt; D009_CLAUDE_PROMPT_STAGE0_INVENTORY_ONLY_29-OCT-2026.txt; D009_STAGE0_STANDING_SCOPE_AND_WORKING_ANSWERS_29-OCT-2026.txt;
  - Stage 1: CHATGPT_REVIEW_D009_STAGE0_AND_STAGE1_DECISION_29-OCT-2026.txt; D009_CLAUDE_PROMPT_STAGE1_BOUNDED_EVIDENCE_READS_29-OCT-2026.txt; CHATGPT_REVIEW_D009_STAGE1_EVIDENCE_PACKAGE_29-OCT-2026.txt; D009_CLAUDE_HANDOFF_AFTER_STAGE1_REVIEW_29-OCT-2026.txt;
  - Rama's answers: D-009_RAMA_ANSWER_SHEET_29-OCT-2026.txt (ChatGPT's sheet, left blank as the question record); D009_RAMA_ANSWER_GUIDE_REAL_MARKET_EXAMPLES_AND_CHATGPT_SUGGESTIONS_29-OCT-2026.txt (ChatGPT's guide, holding Rama's answers in its "RAMA'S ANSWER" lines);
  - the raw-ideas register: D009_RAW_IDEAS_STATUS_AND_V2_DESIGN_DISCUSSION_REGISTER_29-OCT-2026.txt (ChatGPT's revision 0); CHATGPT_REVIEW_D009_CLAUDE_RAW_IDEAS_NOTES_29-OCT-2026.txt; CHATGPT_REVIEW_D009_RAW_IDEAS_REGISTER_REV1_29-OCT-2026.txt; D009_FINAL_REVIEW.doc (a binary Word file, stored unchanged, PKG9-Q7);
  - drafts and storage: D009_NEXT_STEP_CLAUDE_HANDOFF_30-OCT-2026_RESHARE.txt; CHATGPT_REVIEW_D009_THREE_DRAFTS_AND_PACKAGE_DECISIONS_30-OCT-2026.txt; D009_CLAUDE_PROMPT_CORRECT_THREE_DRAFTS_30-OCT-2026.txt; CHATGPT_FINAL_REVIEW_D009_REVISION1_AND_STORAGE_AUTHORIZATION_30-OCT-2026.txt; D009_CLAUDE_PROMPT_STORAGE_EXECUTION_30-OCT-2026.txt; CHATGPT_POST_PUSH_REVIEW_D009_30-OCT-2026.txt.
- docs/inputs/chatgpt_originals_and_replies/ (3 files): D-009_STORAGE_PLAN_DRAFT_REV0_30-OCT-2026.txt and D-009_PACKAGE_CHANGE_LOG_DRAFT_REV0_30-OCT-2026.txt (Claude's revision 0 drafts); D-009_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt (Claude's post-push record).
- docs/inputs/manuals/ (1 file): D-009_SUPPORT_RESISTANCE_V2_REQUIREMENTS_REV0_30-OCT-2026.md (revision 0 of the requirements record).
- The D-009 package stored 19 of these; the D-010 package (7c725ed, DR-077) stored the other four, which the D-009 package owed: the post-push record, ChatGPT's final review of revision 1, its storage execution prompt and its post-push review.

Governance rows (lines at 6bd80a5):
- Register: DR-076 (the D-009 row; status "DRAFT RECORDED - NO REQUIREMENT DECIDED"); DR-031 and DR-069 (earlier rows that name D-009 as an owner or ratification point); DR-072 to DR-075 (the statuses D-009 keeps); DR-077, DR-079 and DR-081 (later topics' rows that name D-009; section 9); DR-083 (the V2 topic manual decision, which names D-009's review notes).
- V2_DESIGN_LOG.md: the section 4 D-009 row (line 371); in section 6, under "### 2026-10-30", the D-009 note (line 608) and the D-009 push line (line 609); under "### 2026-10-29", the D-008 push line (line 605), which the D-009 package stored (entry E-2); the section 1 lessons of 2026-10-29 (lines 179 to 181); the section 7 bookmarks BK-76 to BK-78 (lines 700 to 702), and the later BK-82, BK-84 and BK-85 (lines 706, 708, 709); the section 8 gap rows 8 to 21 (lines 724 to 737); the tracking annotations in Q-35 (line 234) and VP-08 (line 259).
- The post-push record: docs/inputs/chatgpt_originals_and_replies/D-009_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt ("RESULT: PASS"), with ChatGPT's docs/inputs/reconciliation/CHATGPT_POST_PUSH_REVIEW_D009_30-OCT-2026.txt ("VERDICT: POST-PUSH ACCEPTED").

Which record governs what:

| Question | Governing record |
|---|---|
| D-009's status, and the status of every RQ9 area and OQ9 question | D-009_v2_requirements.md (sections 2 and 9; section 13); DR-076 records it in the register |
| Rama's exact words | the guide's "RAMA'S ANSWER" lines, "the source of truth for his wording" (the requirements record, lines 23 to 25), copied byte for byte into its section 2.1; the register's items are "PARAPHRASED, NOT VERBATIM" |
| What ChatGPT suggested | the guide's "ChatGPT suggestion" blocks; the register, revision 0 (ChatGPT's) and revision 1 with CR-1 (ChatGPT's text corrected by Claude under ChatGPT's corrections A to F) |
| Legacy facts | the frozen manuals, through the Stage 1 evidence sheet (E9- IDs); "Its frozen-manual citations were not re-read" for the requirements record (its section 4) |
| Legacy gaps | V2_DESIGN_LOG section 8, rows 8 to 21 ("The one canonical list of legacy behaviours that a Route B topic needed and that no frozen manual establishes"), from NR9-01 to NR9-14 |
| Cross-topic tracking | V2_DESIGN_LOG section 7 (BK-76 to BK-78) and the VP-08 and Q-35 annotations, tracking only |
| How the package was prepared and corrected | the storage plan and the package change log; ChatGPT's PKG9 answers there "settle package-planning questions only. They approve no S&R behaviour, formula or requirement." |
| What was stored and pushed | the storage manifest and the post-push record |

## 3. Evidence labels

| Label | Meaning in this manual |
|---|---|
| FACT (V2 DECISION) | a register row whose status is DECIDED, quoted or cited by ID (for example DR-031, DR-073 to DR-075, DR-083). A DECIDED row of a later topic (DR-077, DR-079, DR-081) is that topic's decision, not D-009's. |
| FACT (V2 RECORD) | what a stored V2 record says, with its own status: the D-009 records; DR-076, whose status is "DRAFT RECORDED - NO REQUIREMENT DECIDED", and DR-072, whose status is "DEFERRED / NOT CLOSED", neither of them DECIDED; the design-log rows; the Master Agenda; the manifest and the post-push record. Process facts are as Claude recorded them unless ChatGPT's own check is named. |
| FACT (HISTORICAL) | a frozen manual's statement, cited by manual, section and line with its own qualifiers, as the Stage 1 evidence sheet carries it. It concerns the pinned repository or the record it names, never what a host ran (W-7). |
| RAMA (STATED) | Rama's words, quoted from the guide's "RAMA'S ANSWER" lines or from a record that quotes him, with that record's qualifier. Where an answer repeats or adopts ChatGPT's suggested wording, the provenance is stated beside it, as the requirements record does (section 2.1). |
| RAMA-AUTHORISED, DRAFTED BY CHATGPT | wording that ChatGPT drafted at Rama's request or that Rama issued as his, where a record attributes it so (for example DR-083's package wording). The D-009 records do not use this label for Rama's D-009 answers: they keep each answer as his input and name ChatGPT's suggestion as the source of any wording it repeats. |
| INTERPRETATION | a reading by Claude or ChatGPT, named; never a decision. The requirements record's section 3 is INTERPRETATION unless marked. |
| PROVISIONAL | a recorded RAW IDEA ("Rama's preliminary input") or PROVISIONAL WORKING POSITION ("ChatGPT's suggestion, which Rama adopted "now" or agreed with provisionally; not his own reasoned requirement"), always given with the record's own status value and words (the requirements record, lines 68 to 73). |
| UNKNOWN / NOT ESTABLISHED | a record says so, or no record read establishes the point. |
| NOT RESEARCHED | a legacy behaviour that no frozen manual establishes (W-11); never inferred, and not filled from a legacy source (DR-073). |
| KNOWN DEFECT | a defect recorded as such in a frozen manual, as the evidence sheet carries it. The D-009 records record no defect of their own (section 10). |
| OPEN QUESTION | an OQ9 row, or a question a record keeps open, with its recorded holder. |

- No label is merged with another. A repository value, a code path, a comment or a test is not evidence of deployed behaviour (W-7; C-008_manual.md line 26: "A code path is not proof that a signal, order or trade took that path.").
- The requirements record uses its handoff's labels with their W-11 equivalents (its lines 35 to 43): EXPLICIT RAMA INPUT [RAMA (STATED)]; FACT [HISTORICAL; V2 DECISION / V2 PROPOSAL]; INTERPRETATION / PROPOSAL; UNKNOWN / NOT ESTABLISHED, and NOT RESEARCHED; OPEN QUESTION / DEPENDENCY. Its status values are RAW IDEA, PROVISIONAL WORKING POSITION, OPEN / UNRESOLVED and NOT DECIDED.
- Quotations are exact. A line break inside a quoted passage is not reproduced; "..." marks words left out; in ChatGPT's files curly quotes are written straight and a long dash as "-", the form the D-009 records use for the status value.

## 4. The question the topic answers

- FACT (V2 RECORD), the Master Agenda, Part 5, section D (TRADE DECISION ENGINE, line 289), line 300: "D-009  Support & Resistance".
- The section D goal (lines 311 to 317): "Completely revisit the question: "Should this signal become a trade?"" and "This section is especially important for the known redesign concerns: late entry, unsuitable filtration, incorrect S&R range, and trading near high-of-day without adequate S&R context."
- FACT (V2 RECORD), the requirements record, section 1: "D-009 has a title and the Section D goal only. The goal names "incorrect S&R range" and "trading near high-of-day without adequate S&R context". The agenda gives D-009 no separate scope text (OQ9-01)."
- The agenda's neighbours (FACT (V2 RECORD)): D-010 "S&R zones", D-011 "S&R strength" and D-012 "S&R freshness" (lines 301 to 303); D-013 to D-018, from "Entry zone" to "Final Trade / Reject decision" (lines 304 to 309); E-006 "S&R-based SL" (328) and E-008 "R:R" (330); J-001 "Existing S&R detector" to J-009 "S&R shadow", and J-013 "Retest / structure engines" (lines 437 to 449), under the rule "For each advanced mechanism explicitly decide:" KEEP, MODIFY, REPLACE, REMOVE, SHADOW, and "Presence of code does not automatically mean the mechanism belongs in the new live decision path." (lines 451 to 460); K-009 "Market data" (474); O-006 "S&R architecture" (551).

The recorded concerns behind the question:
- FACT (HISTORICAL), user-reported, as A-001 records it and as the requirements record labels E9-35 (A-001_manual.md section 9, lines 63 and 64): "Incorrect or unsuitable S&R range or interpretation." and "Entry near high-of-day without adequate S&R context."; their mechanics "deferred to section D and later" (line 60). What they meant in practice is UNKNOWN / NOT ESTABLISHED (NR9-11, NR9-12); in D-009 Rama answered "No, I do not recall" (guide line 155).
- RAMA (STATED), "verbatim as recorded; decides nothing" (A-009_v2_design_inputs.md line 123, his Q39; E9-36): "In S&R requires further discuss on revising existing functionality, ... shadow configuration also requires discussion on existing mechanism to revised one while implementing the same".
- FACT (HISTORICAL), as A-009 records his answers (A-009_manual.md section 14, line 230; E9-37): testing differed from production in S&R, and "the differences that could change a trade were the start time and S&R"; his Q26 on the testing code's S&R targets and stops stays NOT ESTABLISHED (E9-38; Q-35 (e) OPEN).
- FACT (V2 DECISION), an owner allocation only (DR-031): "Which strategies, their scanner conditions (VP-44), filtration (VP-03), S&R, R:R and the admission test (no subset or near-copy) stay with C-001, D-003, D-009 to D-017 and J." The requirements record: "This is an owner allocation; it decides no S&R content."
- FACT (V2 RECORD), VP-08 (PROPOSED except its count): "S&R logic is set after strategies and processors, and R:R honours S&R."; "Whose words this sentence is, is not stated in the row." (the requirements record, section 6).
- FACT (V2 RECORD), VP-44 (PROPOSED; Ratify at "D-001, D-013 to D-016, E-002"): its considerations include defining "late" with "room to target and S&R"; "D-009 is not one of its ratification points."
- FACT (V2 RECORD), D-008's hand-off (D-008_v2_requirements.md line 202, under DR-075): the row "Late entries; entries near the day's high" names "D-009 to D-016 (VP-44)" with the status "OPEN in those topics".

How the question was put to Rama:
- FACT (V2 RECORD): ChatGPT prepared D-009_RAMA_ANSWER_SHEET_29-OCT-2026.txt (Q1 to Q14 and additional notes) "after review of Stage 1 evidence" (its header; OQ9-09), with Claude's candidate questions CQ9-01 to CQ9-14 before it, and a guide with "realistic ILLUSTRATIONS with invented prices and circumstances" (guide line 10) and a ChatGPT suggestion for each question. The guide states: "D-009 answers are user-stated preferences only. They do not themselves freeze requirements." (line 15).
- The fourteen questions, in short: Q1 what the reported S&R problem meant; Q2 "adequate S&R context" near the day's high; Q3 whether S&R should affect V2 trade decisions, and in what role; Q4 common or strategy-specific treatment; Q5 which existing S&R functionality his Q39 meant; Q6 a log-only / shadow evidence phase; Q7 what "R:R honours S&R" means (VP-08); Q8 D-009's boundaries with D-010 to D-012 and J; Q9 the boundary with entry, stop-loss and target; Q10 a timeframe preference, which "MUST STAY OPEN FOR D-007"; Q11 missing, late, inconsistent or suspect S&R data; Q12 recording S&R context for audit; Q13 room to S&R, late entry and an extension guard; Q14 his recollection of the testing VM's targets and stops.

## 5. Course of the topic

Times IST, from the requirements record (section 12), the design-log note of 2026-10-30 (line 608), the storage plan and the post-push record ("the times are those of Rama's messages and of Claude's file writes"). ChatGPT's files reached Claude through Rama.

| When | Step | By | Record |
|---|---|---|---|
| 14:05, 29-Oct | the D-008 package pushed (8e60e9f): the D-009 base | Claude | requirements record, section 12 |
| 14:15 | the Stage 0 prompt, the standing scope and working answers, and the review of the D-008 post-push record: "Proceed with D-009 Stage 0 planning only" | ChatGPT | CHATGPT_REVIEW_D008_POST_PUSH_AND_D009_NEXT_STEP_29-OCT-2026.txt |
| 14:27 | the three Stage 0 files | Claude | the three Stage 0 files |
| 14:33 | the Stage 0 review ("STAGE 0 ACCEPTED FOR PLANNING PURPOSES."), guardrails A to H and the Stage 1 prompt | ChatGPT | CHATGPT_REVIEW_D009_STAGE0_AND_STAGE1_DECISION_29-OCT-2026.txt |
| 14:33:56 to 14:47:10 | Stage 1: reads S1-01 to S1-89, re-reads RR-01 to RR-30 and locators L1-01 and L1-02 of the frozen manuals and V2 records | Claude | read log, section 1 |
| 15:02 to 15:03 | the three Stage 1 files | Claude | the three Stage 1 files |
| 15:29 | the Stage 1 review ("ACCEPTED WITH RECORDED PROCESS DEVIATIONS, FOR EVIDENCE-REVIEW PURPOSES ONLY"), the answer sheet and the handoff | ChatGPT | CHATGPT_REVIEW_D009_STAGE1_EVIDENCE_PACKAGE_29-OCT-2026.txt |
| 19:31 | the guide with Rama's answers, and ChatGPT's raw-ideas register, revision 0; Rama's statement that his answers are "rough or raw ideas" (a screenshot of that time, not stored, DR-008) | Rama; ChatGPT | the guide; storage plan, section 4 |
| 19:35 | review notes on the raw ideas | Claude | D-009_CLAUDE_REVIEW_NOTES_ON_RAMA_RAW_IDEAS_29-OCT-2026.txt |
| 19:42 | the review of the notes, with corrections A to F to ChatGPT's register | ChatGPT | CHATGPT_REVIEW_D009_CLAUDE_RAW_IDEAS_NOTES_29-OCT-2026.txt |
| 19:45 to 19:48 | the register, revision 1, its change log (CL-00 to CL-28) and the provenance checklist | Claude | the three register files |
| 19:55 | the review of revision 1: "ONE REQUIRED WORDING CORRECTION" | ChatGPT | CHATGPT_REVIEW_D009_RAW_IDEAS_REGISTER_REV1_29-OCT-2026.txt |
| 19:56 to 19:57 | correction CR-1 (CL-29) | Claude | the register; its change log |
| 20:38 | the final review of the register: "ACCEPTED FOR CARRY-FORWARD TO THE D-009 STORAGE PACKAGE." | ChatGPT | D009_FINAL_REVIEW.doc |
| 20:39 | a local carry-forward list (not stored; superseded by the storage plan) | Claude | package change log, PC-06 |
| 08:40, 30-Oct | the next-step handoff, reshared after the 29-Oct copy did not download | ChatGPT | D009_NEXT_STEP_CLAUDE_HANDOFF_30-OCT-2026_RESHARE.txt |
| 08:42 to 08:47 | read-only pre-write checks | Claude | storage plan, section 1 |
| from 08:48 (to 08:53) | revision 0 of the requirements draft, the storage plan and the package change log | Claude | the revision 0 copies in docs/inputs/ |
| 09:17 | the review of the three drafts ("suitable for a controlled correction round"): RQ-C1, SP-C1 to SP-C6 and CL-C1, and the answers to PKG9-Q1 to PKG9-Q8; with the correction prompt | ChatGPT | CHATGPT_REVIEW_D009_THREE_DRAFTS_AND_PACKAGE_DECISIONS_30-OCT-2026.txt |
| 09:19 | revision 0 of the three drafts kept with cp -p before any revision 1 edit | Claude | package change log, PC-11 |
| after 09:19 (re-checks from 09:34) | revision 1 of the three drafts, and a validation report (not stored) | Claude | package change log, section 3 |
| 09:58 | the final review of revision 1 ("ACCEPTED FOR THE D-009 STORAGE STEP, SUBJECT TO THE EXECUTION GUARDS BELOW."), with the storage execution prompt | ChatGPT | CHATGPT_FINAL_REVIEW_D009_REVISION1_AND_STORAGE_AUTHORIZATION_30-OCT-2026.txt |
| 10:01:48 | fetch; GitHub main equal to the expected base 8e60e9f | Claude | post-push record, section 2 |
| 10:03 | the storage action (DR-076): row 1's status lines and section 12 brought up to date | Claude | requirements record, section 12 |
| 10:06:41 | commit 9c9c2e9, 38 files; pushed at 10:06:54 and compared at 10:06:57 | Claude | post-push record, section 4 |
| 10:07:20 to 10:07:22 | the fresh-clone check: "RESULT: PASS" | Claude | post-push record, sections 1 and 5 |
| 30-Oct (time not stated in the records read) | the post-push review: "VERDICT: POST-PUSH ACCEPTED" | ChatGPT | CHATGPT_POST_PUSH_REVIEW_D009_30-OCT-2026.txt |
| 18:37, 30-Oct | the D-010 package (7c725ed, DR-077) stores the four D-009 records owed and the D-009 push line | Claude | DR-077; V2_DESIGN_LOG line 611 |

- Who did what (FACT (V2 RECORD)): ChatGPT prepared the prompts, the standing scope, the answer sheet, the guide, the register's revision 0 and every review; Rama answered in the guide and relayed ChatGPT's files; Claude prepared its own records, and the package was "Prepared, stored and pushed by Claude under W-10 as updated on 27-Oct" (DR-076).
- INTERPRETATION (Claude): the stages above follow the Route B change record's per-topic steps (its section 5: Stage 0, Stage 1, Stage 2 and storage), which W-13 (DR-080) replaced from D-011 onwards and which "stay as written for the topics run under them"; D-009 was stored before W-12 (DR-078) existed.

## 6. What is recorded and why nothing is decided

Why no requirement is decided:
- RAMA (STATED), his message to ChatGPT on 29-Oct-2026 (a screenshot, not stored, DR-008; quoted in the requirements record, lines 18 to 20, and in DR-076): "Consider these are all 'rough or raw ideas', but we need to discuss deeply while designing new system". The record adds: "Nothing below turns them into an approved formula, threshold, rule or gate."
- FACT (V2 RECORD), the requirements record's decision record (section 8): Final decision "NONE. No D-009 requirement is decided (section 2)"; Reason "Rama's statement that his answers are "rough or raw ideas" for deep discussion in the new-system design".
- FACT (V2 RECORD), ChatGPT's package decision PKG9-Q1 (its review of 09:17, section B): "(a) Store the D-009 record now as a draft record with no requirement decided."; "This is a record of evidence and current discussion state, not a requirements freeze, approval or ratification."
- FACT (V2 RECORD), DR-076: "It records the evidence and the current state of the discussion; it is not a requirements freeze, approval or ratification. NO D-009 REQUIREMENT IS DECIDED."
- FACT (V2 RECORD), the provenance of the answers (the requirements record, lines 132 to 137): "Where an answer repeats ChatGPT's suggested wording, the guide's ChatGPT suggestion is the source of that wording:"
  - "Q4 and Q5 open with ChatGPT's wording, and so do Q7 ("Defer detailed R:R behavior to the E-topic") and Q14."
  - "Q6, Q8 and Q9 ask for ChatGPT's best opinion."
  - "Q11 and Q12 agree with ChatGPT's suggestion."
  - "None of these is an independent, fully reasoned requirement from Rama."

The requirement areas, as recorded (the requirements record, section 2, governs; statuses quoted from its table):

| ID | Area: what V2 must eventually decide | Rama's input (guide line) | Status, as recorded | Label | Open / dependency |
|---|---|---|---|---|---|
| RQ9-01 | Whether S&R is an input to V2's trade decision at all, and in which role | Q3 (53) | NOT DECIDED. The examples of Q1, Q2, Q7 and Q13 suggest a "reject if the SL and target do not fit" concept (INTERPRETATION; section 3); it must not become a gate by implication | OPEN QUESTION | OQ9-13 |
| RQ9-02 | Which S&R levels count: what "nearest" means, and from which timeframes | Q1 (34, 35); Q10 (118); Q13 (142) | RAW IDEA; the timeframes OPEN / UNRESOLVED on D-007 | PROVISIONAL (RAW IDEA) | OQ9-06, OQ9-14 |
| RQ9-03 | Whether, and how, a trade's SL and target must sit relative to S&R | Q1 (34); Q2 (45); Q7 (88); Q13 (142) | RAW IDEA; the conflicts in section 3 are preserved | PROVISIONAL (RAW IDEA) | OQ9-15 to OQ9-19 |
| RQ9-04 | Common or strategy-specific S&R treatment | Q4 (62) | PROVISIONAL WORKING POSITION, plus Rama's own point | PROVISIONAL (PROVISIONAL WORKING POSITION) | OQ9-20 |
| RQ9-05 | What happens when S&R data is missing, late, inconsistent or suspect | Q11 (126) | PROVISIONAL WORKING POSITION: ChatGPT's suggestion (make the condition visible, record it and alert; whether the signal proceeds, is deferred or is rejected is a later explicit governance decision; no hidden gate) | PROVISIONAL (PROVISIONAL WORKING POSITION) | OQ9-21 |
| RQ9-06 | Recording S&R context for later audit | Q12 (134) | PROVISIONAL WORKING POSITION: ChatGPT's suggestion (enough to reconstruct what S&R information was available and whether or how it influenced the decision; fields, retention and implementation later) | PROVISIONAL (PROVISIONAL WORKING POSITION) | OQ9-22 |
| RQ9-07 | Any log-only (shadow) phase before S&R may affect a trade | Q6 (80) | OPEN / UNRESOLVED. No shadow or log-only phase is approved, and whether to evaluate one is undecided | OPEN QUESTION | OQ9-23 |
| RQ9-08 | D-009's scope against D-010, D-011, D-012 and J | Q8 (102) | PROVISIONAL WORKING POSITION (ChatGPT's proposal, section 1); not frozen | PROVISIONAL (PROVISIONAL WORKING POSITION) | OQ9-01, OQ9-02 |
| RQ9-09 | D-009 against the entry, SL and target topics | Q9 (110) | PROVISIONAL WORKING POSITION (ChatGPT's proposal, section 1); not frozen | PROVISIONAL (PROVISIONAL WORKING POSITION) | OQ9-03 |
| RQ9-10 | Room to S&R and late entry | Q13 (142) | RAW IDEA (the fit concept of RQ9-03); its boundary with D-013 to D-016 and E-008 OPEN | PROVISIONAL (RAW IDEA) | OQ9-04 |
| RQ9-11 | VP-08's S&R wording: "S&R logic is set after strategies and processors, and R:R honours S&R." | Q7 (88) | NOT DECIDED. VP-08 is PROPOSED except its count and is not ratified at D-009 | OPEN QUESTION | OQ9-08 |
| (Q5) | Which legacy S&R component Rama meant | Q5 (72) | UNKNOWN / NOT ESTABLISHED | UNKNOWN / NOT ESTABLISHED | J-001 to J-009 |
| (Q14) | The testing VM's S&R targets and stops | Q14 (150) | UNKNOWN / NOT ESTABLISHED; Q-35 (e) OPEN | UNKNOWN / NOT ESTABLISHED | - |
| (notes) | A direction for the later design discussion | additional notes (155) | not a requirement | RAMA (STATED) | - |

Rama's answers, byte for byte from the guide (RAMA (STATED); the guide line; provenance as the requirements record states it):
- Q1, line 34: "1. For Intraday: Timeframes for S&R is 1Hr + 1D + 1W, So support & resistance points to be followed in these time frames with whichever is nearest S&R, Example: If ITC's S&R points as per 1Hr=Rs 250, 1D=Rs 260, 1W=Rs 270 and CMP is Rs 246 and strategy.yaml says R:R is Rs 1:2 (Risk 1: Return 2) then system checks the CMP against nearest S&R (1Hr > 1D > 1W) so nearest support with 1Hr=Rs 250, So R:R might be Rs 5: Rs 10 then Rs 246+Rs 5 = Rs 251 which is above then Rs 250 so stock can't be traded or rejected, Assume CMP is Rs 251 then can be traded why? 1D S&R is at Rs 260 this will accommodate R:R"
- Q1, line 35: "2. For Delivery: Time frames will be 1D + 1W + 1M with same concept as above"
- Q1, line 36: "* Note is cautiously, While designing new system we will discuss deeply and this is for rough idea only, meaning whether follow these time frame or something else including improved logic if any"
- Q2, line 45: "Same as Q1: Check which is next and nearest S&R point? (1Hr > 1D > 1W) then calculate the CMP > S&R points can accommodate R:R, if its accommodates then make trade else outright reject, So strategy signal Rs 996 to Rs 1000 (assume nearest S&R) so Rs 4 tgt fits within S&R? meaning SL Rs 2: TGT Rs 4 then pass else reject, We will discuss later deeply so note down cautiously"
- Q3, line 53: "Not understood well, So we will discuss later"
- Q4, line 62 (opens with ChatGPT's suggested wording, then Rama's own point): "Common baseline, with strategy-specific differences only where explicitly defined and tested, Yes correct only Breakout strategies & pull back strategies logic is equally opposite, like wise S&R considerations also applied accordingly, So we will discuss later"
- Q5, line 72 (opens with ChatGPT's suggested wording): "I don't recall which component; the records should be reviewed separately, So we will discuss later the relevant issue in new system"
- Q6, line 80; Q8, line 102; Q9, line 110 (each asks for ChatGPT's best opinion): "Not understood, So as per your best opinion now, But we will discuss later the relevant issue in new system"
- Q7, line 88 (opens with ChatGPT's suggested wording, "Defer detailed R:R behavior to the E-topic"): "Defer detailed R:R behavior to the E-topic, But the concept is: Once stock is eligible to make order then immediately pull nearest S&R points by comparing 1hr + 1D + 1W then check the R:R whether this R:R points within S&R (meaning for risk point is above 'support point' and for reward point must be below 'resistance point' why? safest approach) if it holds then place the order else reject, So we will discuss later"
- Q10, line 118: "OPEN / UNRESOLVED: The probable time frames for S&R (For Intraday: 1Hr + 1D + 1W and For Delivery: 1D + 1W + 1M) and entry conformations may be 1-5mins, we will discuss later"
- Q11, line 126; Q12, line 134 (each agrees with ChatGPT's suggestion): "Yes agreed with your suggestion"
- Q13, line 142: "We will discuss later, But the concept is simple whether this trade falls with in my R:R by considering nearest S&R point (1Hr > 1Month as stated above for intraday to Delivery), if the R:R within S&R then take trade else reject ('Within' means both SL & TGT points below its intended S&R, Example: Support is Rs 100 then SL points must be till Rs 100 say Rs 99.50-Rs.99.90 like wise If resistance is Rs 200 then TGT must till Rs 200 say Rs 199.50-Rs 199.90)"
- Q14, line 150 (opens with ChatGPT's suggested wording; "in the answer options the guide offered", as the Q-35 annotation puts it): "No, I do not recall that behaviour or Not sure"
- Additional notes, line 155: "No, I do not recall, But in new system we must deeply discuss on suitable time frames for intraday and Delivery; Improved S&R logic if any; and any other improvements if any, we will discuss later, note carefully!"

What is recorded besides the answers (FACT (V2 RECORD); none of it is a requirement):
- the evidence (section 8 here) and the gaps NR9-01 to NR9-14, written to V2_DESIGN_LOG section 8 as rows 8 to 21;
- the conflicts and readings of the raw inputs (section 7 here), "preserved, not resolved";
- the open questions OQ9-01 to OQ9-25 (section 7 here);
- the bookmarks BK-76 to BK-78 and the VP-08 and Q-35 annotations, tracking only (section 9 here);
- three historical process lessons (section 10 here);
- the outcome (the requirements record, section 7): "V2 S&R requirements: none decided in this draft."; "The legacy S&R mechanism: no outcome at D-009. Its disposition is J's (J-001 to J-009); KEEP and MODIFY are not available (section 4)."; "SHADOW: not chosen. No shadow or log-only phase is approved, and whether to evaluate one is OPEN (RQ9-07)."

What was decided in connection with D-009 (package and governance only; none is an S&R requirement):
- FACT (V2 RECORD): DR-076 records the draft; its status is "DRAFT RECORDED - NO REQUIREMENT DECIDED", "ChatGPT's wording (PKG9-Q1), with its long dash shown as "-"" (storage plan, section 5).
- FACT (V2 RECORD), ChatGPT's answers to PKG9-Q1 to PKG9-Q8 (the package change log, section 4), which "settle package-planning questions only":

| ID | ChatGPT's package decision, in short |
|---|---|
| PKG9-Q1 | "(a) Store the D-009 record now as a draft record with no requirement decided."; status "DRAFT RECORDED - NO REQUIREMENT DECIDED"; the row says "that OQ9-01 to OQ9-25 remain open unless a question is purely administrative and separately identified" (the package change log); DR-076 closes none |
| PKG9-Q2 | "(a) Include them as evidence gaps, not findings." (NR9-01 to NR9-14 in section 8) |
| PKG9-Q3 | "(a) Add tracking-only bookmarks", only for the three direct dependencies (BK-76 to BK-78) |
| PKG9-Q4 | tracking-only annotations for VP-08 and Q-35 (e): "VP-08 remains PROPOSED and is NOT ratified at D-009."; "Q-35(e) remains OPEN." |
| PKG9-Q5 | "Use docs/agenda/d-009_support-resistance/." |
| PKG9-Q6 | the stored names: "_DRAFT" removed; the register's revision 0 under its original name in docs/inputs/reconciliation/, revision 1 with CR-1 with "_REV1" in the topic folder |
| PKG9-Q7 | "(a) Store the original binary .doc unchanged."; no derived rendering |
| PKG9-Q8 | "(a) Record the three lessons as historical process lessons, not normative system requirements" |

- Earlier rows D-009 keeps and does not decide: FACT (V2 DECISION), DR-001 (live-only, which OQ9-23 cites), DR-031 (the owner allocation), DR-073 (option C; D-007 BLOCKED / NOT CLOSED), DR-074 (Route B) and DR-075 (D-008); FACT (V2 RECORD), DR-072 (D-006, status "DEFERRED / NOT CLOSED").

## 7. What is not decided: open questions, provisional positions, raw ideas, unknowns

OPEN QUESTION, all 25 open: "OQ9-01 to OQ9-25 remain open, and none is closed by this record; OQ9-09 to OQ9-12, Stage 0's process questions, are identified separately, each with its status as recorded" (DR-076). The requirements record, section 9, governs.

Stage 0 questions (status now, as recorded):

| ID | Question | Status now, as recorded |
|---|---|---|
| OQ9-01 | What does D-009 own, as distinct from D-010, D-011 and D-012? | OPEN; PROVISIONAL WORKING POSITION (Q8) |
| OQ9-02 | How is D-009 separated from J-001 to J-009? | OPEN; PROVISIONAL WORKING POSITION (Q8); DR-075's J-011 pattern is a precedent only |
| OQ9-03 | Are S&R use in entry and in SL outside D-009? | OPEN; PROVISIONAL WORKING POSITION (Q9) |
| OQ9-04 | Which part of "near high-of-day without adequate S&R context" belongs to D-009? | OPEN; RAW IDEA (Q2); the entry rule with D-013 and D-016 |
| OQ9-05 | What did "incorrect S&R range" mean? | OPEN: the historical meaning is UNKNOWN / NOT ESTABLISHED (NR9-11); Rama does not recall |
| OQ9-06 | The timeframe dependency on D-007 | OPEN / UNRESOLVED; Rama's ideas recorded as raw ideas |
| OQ9-07 | The formation dependency on D-006 | OPEN / UNRESOLVED; no D-009 input needs it now (INTERPRETATION) |
| OQ9-08 | Is any part of VP-08 ratified at D-009? | OPEN; Rama's decision; not ratified |
| OQ9-09 | Does D-009 need its own answer sheet? | answered by events: ChatGPT prepared it and Rama answered on 29-Oct as raw ideas; closing it is ChatGPT's call |
| OQ9-10 | Rama's raw answer sheets in docs/inputs/ | unchanged: not searched |
| OQ9-11 | The frozen manuals' companions | ChatGPT's decision: "not opened; not required for answer-sheet preparation; may be reconsidered only on a specific evidence need" |
| OQ9-12 | Stage 1 order and read boundary | Stage 1 accepted with recorded deviations (DV-01, DV-02, M-01, M-03, M-04) |

New questions from the raw inputs (the requirements record's "Who decides" is the holder):

| ID | Question, in short | Source | Who decides | Note, as recorded |
|---|---|---|---|---|
| OQ9-13 | Should S&R be an input to V2's trade decision at all, and in which role (a Trade / Reject condition, a score or ranking input, entry / SL / target placement, recorded context only, or another)? | Q3; Q1, Q2, Q7, Q13 | Rama, after discussion | until decided, no S&R gate exists by implication |
| OQ9-14 | What does "nearest S&R" mean, for long and short trades; one timeframe first or all compared; a closer level against a stronger one; overlapping levels and zones? | Q1, Q2, Q7, Q13 | design, then Rama | zones, strength and freshness themselves are D-010 to D-012's |
| OQ9-15 | Does the SL sit above support (Q7) or just below it (Q13)? | Q7, Q13 | Rama | detailed SL rules are E-006's |
| OQ9-16 | May a target sit at the resistance level (Q2) or must it stay below it (Q13)? | Q2, Q13 | Rama | detail with the E topics |
| OQ9-17 | Is the SL (risk) set by the strategy and then checked against S&R, or derived from support? | Q1 (the Rs 251 case), Q13 | Rama | E-006 |
| OQ9-18 | Short-side symmetry, and any exceptions | all examples long; DR-031 | Rama | - |
| OQ9-19 | How is the ITC example to be read: its labels, its arithmetic and the Rs 251 case? | Q1 | Rama | kept as a rough illustration, not an approved example |
| OQ9-20 | Which parts of S&R treatment are common and which strategy-specific (breakout against pullback), and how are differences governed and tested? | Q4 | Rama; strategy filtration itself with D-003 and D-017 (VP-03) | - |
| OQ9-21 | With missing, late, inconsistent or suspect S&R data, does the signal proceed, wait or get rejected; what is alerted; how does the check avoid becoming a hidden gate? | Q11 | Rama, as an explicit governance decision | the alert design with its owner (not named) |
| OQ9-22 | What minimum S&R record is kept with each evaluated signal and decision, and which later step and owner define its fields, retention and storage? | Q12 | agenda governance, then Rama | - |
| OQ9-23 | Should any log-only (shadow) phase be evaluated; evidence before any V2 trading, or V2 trading without S&R authority during a log-only period, given live-only V2 (DR-001); measures, sample, acceptance criteria, owner and approval gate? | Q6 | explicit governance review | J-009 owns the review of the legacy S&R shadow |
| OQ9-24 | When are S&R levels computed relative to a signal, and where do weekly and monthly candles come from? | Q7 ("immediately pull"); Q1, Q10 (1W, 1M) | design | K-009 (market data) |
| OQ9-25 | Does V2 use S&R points, zones or both? | Rama's examples use single prices | D-010 (zones) | D-009 records the dependency only |

The conflicts preserved in Rama's examples (INTERPRETATION unless marked: the requirements record, section 3; the register, revision 1 with CR-1, sections C and D). They are kept as conflicts. This manual resolves none of them, and the questions that carry them stay OPEN (DR-076; the later DR-077, DR-079 and DR-081 each state that D-009's OQ9-01 to OQ9-25 stay OPEN):
- "One concept recurs in Q1, Q2, Q7 and Q13: take the trade only if its SL and target fit within the nearest S&R, and otherwise reject it (Q2: "else outright reject")." "Rama answered Q3 "Not understood well", so the role is NOT DECIDED. The concept must not become a live rejection gate by implication; explicit governance decides whether S&R is informational, affects ranking, affects Trade / Reject, or takes part in entry or risk calculations."
- The ITC example (Q1) "has label and arithmetic inconsistencies": "At a CMP of Rs 246 it calls the Rs 250 level "nearest support", although the level sits above the price."; "At "1:2 (Risk 1: Return 2)" with a Rs 5 risk the target would be Rs 256, while the example writes "Rs 246+Rs 5 = Rs 251". Both figures are above Rs 250."; "At a CMP of Rs 251 the same Rs 5 risk gives a target of Rs 261, above the Rs 260 level. So "can be traded" is not consistent with the example's own figures." The register keeps the example as "Rama's rough illustration; do not use it as an approved example or formula" (OQ9-19).
- Which side of support the SL sits on: "Q7: "for risk point is above 'support point'". Q13: an SL of Rs 99.50 to 99.90 for support at Rs 100, i.e. below it." (OQ9-15)
- Whether the target sits at the level or below it: "Q2 sets the target at Rs 1,000, the level itself ("TGT Rs 4" from Rs 996). Q13 keeps it below (Rs 199.50 to 199.90 for Rs 200)." (OQ9-16)
- "Whether the SL is set by the strategy first and then checked against S&R, or derived from support: OPEN / UNRESOLVED." (OQ9-17)
- "Every example is a long trade. Short-side symmetry and exceptions are OPEN / UNRESOLVED; DR-031's six strategies include two intraday short strategies." (OQ9-18)
- Timeframe notation: ""1Hr > 1D > 1W" (Q1, Q2) and "1Hr > 1Month" (Q13) may list the timeframes rather than rank them. In the ITC example the level used was the nearest one in the trade's direction, whatever its timeframe. No hierarchy is set (D-007 BLOCKED / NOT CLOSED)." (OQ9-06, OQ9-14)
- Timing: "Q7's "immediately pull nearest S&R points" once a stock is eligible bears on when levels are computed and on missing data (RQ9-05). It is a dependency, not a decision (OQ9-24)."

PROVISIONAL, as PROVISIONAL WORKING POSITION (ChatGPT's suggestions; not Rama's own reasoned requirements):
- The working boundary (RQ9-08, RQ9-09; section 1 here): ChatGPT's proposal, adopted by Rama "now" (Q8, Q9), "not frozen".
- RQ9-04 (Q4): ChatGPT's wording "Common baseline, with strategy-specific differences only where explicitly defined and tested", "plus Rama's own point" that breakout and pullback logic is "equally opposite". The register adds: "Need later to define what is common, what is strategy-specific, and how exceptions are governed and tested." (section F; OQ9-20).
- RQ9-05 (Q11) and RQ9-06 (Q12): ChatGPT's suggestions, with Rama's "Yes agreed with your suggestion", in the words of the table in section 6. The register limits Q11's agreement to "recording and alerting the quality condition"; "Alerting must not become an implicit trade blocker or system-start blocker." (section H; OQ9-21). For Q12, "Storage design is not decided." (item 13; OQ9-22).
- Q6 is recorded differently: although Rama's answer also asks for ChatGPT's best opinion, the record gives RQ9-07 the status OPEN / UNRESOLVED, not PROVISIONAL WORKING POSITION, and the register's section I, after CR-1, reads: "No shadow or log-only phase is approved. Whether to evaluate such a phase remains OPEN / UNRESOLVED."

PROVISIONAL, as RAW IDEA (Rama's preliminary input):
- RQ9-02 (Q1, Q10, Q13): the timeframe ideas, intraday "1Hr + 1D + 1W", delivery "1D + 1W + 1M", entry confirmations "may be 1-5mins"; "These are ideas, not a hierarchy." (the register, item 11). They stay OPEN / UNRESOLVED on D-007 (OQ9-06).
- RQ9-03 (Q1, Q2, Q7, Q13): the fit concept, with its conflicts above (OQ9-15 to OQ9-19).
- RQ9-10 (Q13): room to S&R and late entry, "the fit concept of RQ9-03"; its boundary with D-013 to D-016 and E-008 OPEN (OQ9-04).

UNKNOWN / NOT ESTABLISHED:
- (Q5) Which legacy S&R component Rama meant in Q39 (J-001 to J-009).
- (Q14) The testing VM's S&R targets and stops; Q-35 (e) OPEN; "a personal recollection, not legacy evidence" (the Q-35 annotation).
- The legacy behaviours NR9-01 to NR9-14 (section 8 here).

Other items the package records as unresolved (the package change log, section 6): UO-01 ("No D-009 requirement is decided"; Rama, after the deep design discussion); UO-02 (OQ9-01 to OQ9-25 stay open); UO-04 and UO-06 (copies not compared or not received; section 10 here); UO-10 (NR9-01 to NR9-14); UO-11 (the D-006 and D-007 dependencies, "their own governance").

## 8. Legacy evidence

FACT (HISTORICAL), through the accepted Stage 1 evidence sheet (the requirements record, section 4, which follows the Master Agenda's Part 3 items 1 to 8). "Each row is as the evidence sheet records it. Its frozen-manual citations were not re-read for this draft. A repository value is not a deployed value (W-7). Code, configuration and tests are not proof of deployed runtime behaviour."

| Part 3 item | What the frozen manuals record, with their qualifiers | Evidence |
|---|---|---|
| 1. Current system | D-004_manual.md section 7, line 227: FACT (CONFIG), "repository values at the pin, not the deployed values (gap G-03)": "sr_detector enabled true, timeframes day, 60minute and 30minute, lookback 180 days, cache 1,800 s, wait_for_retest_enabled false ..."; B-006_manual.md section 3, line 33: worker TH5 "S&R detector (sr-detector)", built and started in Phase 0e when "sr_detector.enabled true (509)", repository configuration only; its thread count "INTERPRETATION: a count from the code path and the repository configuration, not a measurement on a host." (line 62) | E9-04, E9-05, E9-08 |
| 1. Shadow wording | A-001_manual.md section 6, line 46 records the README: "The README states that the regime, S&R detector, V3 chain and allocation engines were shadow / default-OFF."; A-009_manual.md section 8, line 137: "the shadow S&R detector, enabled in production". "Conflicting wording, preserved as conflicting. Neither establishes the deployed configuration, that the detector ran on a host, or that it affected a trade" | E9-01, E9-02, E9-32; W9-01 |
| 2. Code / modules | the detector files D-004 cites, read for its data contracts only (D-004_manual.md section 19.2, lines 452 to 455); the shared closure "feeds the S&R detector" (section 7, line 226) when one of six feature flags is on, the flags' definitions not read (gap G-04) | E9-20, E9-06 |
| 3. Inputs | historical candles from broker historical data through OhlcFetcher: one request per interval over [now - lookback, now], a TTL cache, and it "returns None or omits the interval on any failure, never raising" (D-004_manual.md section 7, line 228) | E9-16, E9-17 |
| 4. Process | what the detector computed | NOT RESEARCHED (NR9-01) |
| 5. Output | sr_detector_results recorded "by name only (definitions not read)" (D-004_manual.md section 14, line 292); the PB-01 watchlist row's sr_zone_json, with the code comment "Record the nearest 30m/1h S&R zone to LEVEL (validation programme; fail-safe)." (C-011_manual.md section 7.6, line 461) and its helper's body UNKNOWN (G-03); what a zone is belongs to D-010 and is not interpreted | E9-19, E9-21, E9-22 |
| 6. State change | the PB-01 capture's code path writes a row with sr_zone_json and status "PENDING"; whether rows were written, and their use, not established | E9-21, E9-22 (NR9-05) |
| 7. Dependencies | the six feature flags (G-04); the cache of 1,800 s; the testing-only "S&R SHADOW CONTRACT v1.3 - LOG-ONLY structural shadow ... GATES NOTHING" (A-009_manual.md section 14, line 223), testing configuration being "comparison evidence only" | E9-06, E9-18, E9-12 to E9-14 |
| 8. Problems / limitations | A-001_manual.md section 9, lines 63 and 64 (user-reported; section 4 here); A-009_manual.md section 18, line 271: "The testing code's S&R target and stop behaviour (Rama, Q26)." UNKNOWN | E9-35, E9-38 |
| Decision path | FACT (COUNT): count Q03 found only the homonym "to support special sessions" in the four screener files (D-003_manual.md section 13, line 484); the retest diverter "runs only when a diverter is set and enabled" (line 486), and C-008 records that "none is set" (C-008_manual.md section 4.1, line 155); INTERPRETATION (D-003's), for the screener only: "no S&R, regime or V3-chain input reaches the screener" (line 488); ST-13 derives entry and SL "from the trigger price" (C-008 line 153); the S&R observer runs "after PROCESSED only" (line 171); E3 sets "a structure SL from the zone band and the configured buffer" (line 317), and "On the pinned wiring with the repository configuration, E3 has no caller." (line 327) | E9-24 to E9-26, E9-29, E9-30, E9-10 |

- The one candle measure on the retest path, `_strong_close` (DR-071; D-005_manual.md section 4), is recorded as "a candle measure on the retest path, owned by D-015; not an S&R level calculation" (E9-31).
- "KEEP and MODIFY are not available for any legacy S&R behaviour (W-11). The frozen manuals establish only its repository configuration and code paths, not what ran." (the requirements record, section 4). The disposition of the legacy engines is J's (J-001 to J-009).
- Rows read and dropped as Rama's S&R words are listed in E9-39: B-002 line 254 (his B2-02 answer is "UNKNOWN"), C-004 line 551 (his Q7 answer has no S&R term), DR-031 (the S&R owner sentence is the register's text), VP-08 (its S&R sentence has no stated author) and the A-008 and C-001 V2 design inputs (Rama's quoted words there have no S&R term).
- "Pivot" occurs only as legacy names (the field name pivot_n_by_tf and the class Pivot; E9-02, E9-15); no formation evidence (OQ9-07).
- Wording notes, nothing resolved (the evidence sheet, section 10): W9-01 (the shadow and on/off wording, unreconciled; NR9-13); W9-02 (C-011's DU-04 against D-004's MC-30: "No erratum exists, and whether MC-30 settles DU-04 for the PB-01 route is not decided here."); W9-03 (VP-08's Ratify-at as C-001 recorded it, superseded in the design-log row); W9-04 (a line number of VP-08 cited by D-003; the same row ID); W9-05 (D-008's accepted citations agree with D-009's reads).
- OQ9-11: three companion records named in the passages read were not opened, by ChatGPT's decision: C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md, C-011's companion file and D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt.

The gaps, as written to V2_DESIGN_LOG section 8 (NOT RESEARCHED or UNKNOWN / NOT ESTABLISHED, as the requirements record gives them; each row "an evidence gap, not a finding"; every row OPEN):

| NR ID | Section 8 row | Legacy fact not established | Status in the requirements record |
|---|---|---|---|
| NR9-01 | 8 | what the detector computed: levels or zones, method, timeframe use, its fields' effect | NOT RESEARCHED |
| NR9-02 | 9 | whether the detector (TH5) ran on any host, and from when | UNKNOWN / NOT ESTABLISHED (Q-44 (a): "TH5 ... not established") |
| NR9-03 | 10 | what the observer's hand-off after PROCESSED led to; who calls the detector's factory | NOT RESEARCHED |
| NR9-04 | 11 | sr_detector_results: definition, writer, reader, content | UNKNOWN / NOT ESTABLISHED |
| NR9-05 | 12 | the PB-01 zone: its selection and failure handling, the candle source, whether rows were written, what the entry stage did with them | UNKNOWN / NOT ESTABLISHED |
| NR9-06 | 13 | whether any S&R output set or changed a production Trade / Reject, entry, SL or target | NOT RESEARCHED |
| NR9-07 | 14 | the six feature flags gating the shared closure, and which were on in operation | UNKNOWN / NOT ESTABLISHED |
| NR9-08 | 15 | the structure exit's logic and its use of _strong_close | UNKNOWN / NOT ESTABLISHED |
| NR9-09 | 16 | the retest confirmation's frac value and its use of _strong_close | UNKNOWN / NOT ESTABLISHED |
| NR9-10 | 17 | the testing S&R shadow v1.3's output and database; the testing code's S&R target and stop behaviour | UNKNOWN / NOT ESTABLISHED (Q-35 (e) OPEN) |
| NR9-11 | 18 | what "Incorrect or unsuitable S&R range or interpretation" referred to | UNKNOWN / NOT ESTABLISHED (Rama: "No, I do not recall", guide line 155) |
| NR9-12 | 19 | what "adequate S&R context" for an entry near the day's high meant | UNKNOWN / NOT ESTABLISHED |
| NR9-13 | 20 | how the README's "shadow / default-OFF" relates to "enabled in production" and the repository flag true | UNKNOWN / NOT ESTABLISHED (W9-01) |
| NR9-14 | 21 | what the 11 defaulted detector fields did, and whether any default changed a trade | UNKNOWN / NOT ESTABLISHED |

- "These are evidence gaps, not findings that the behaviour did or did not exist or run. Route B does not research them, and no legacy source is read to fill them." (the requirements record, section 4). Section 8's rule (V2_DESIGN_LOG line 713): "An item closes only by a note in its row: after a separately approved evidence route (DR-073), or a decision that it is not needed."

## 9. Cross-topic boundaries, dependencies and bookmarks

Boundaries, as the requirements record states them (section 10, with each status as it was at D-009's storage on 30-Oct; the right-hand column names later records, which are not D-009 decisions):

| Item | Topic | Relation to D-009, as recorded | Status at D-009's storage, as recorded | Later record (after 10:06 IST on 30-Oct) |
|---|---|---|---|---|
| S&R zones; strength; freshness | D-010; D-011; D-012 | kept separate (OQ9-01, OQ9-25) | not started | V2 REQUIREMENTS RECORDED: DR-077, DR-079, DR-081 (below) |
| The legacy S&R engines and their uses | J-001 to J-009 | disposition is J's; D-009 keeps, modifies or revives nothing | not started (Route B topics) | - |
| Retest and structure engines | J-013 | the E3 path and the structure exit (NR9-08) | not started | - |
| Entry zone, breakout, retest, entry timing | D-013 to D-016 | the high-of-day and room-to-S&R parts (OQ9-04; RQ9-10) | not started | D-013 NOT STARTED (V2_DESIGN_LOG section 4) |
| Score / ranking; the final Trade / Reject | D-017; D-018 | where any S&R role would act (OQ9-13) | not started | - |
| S&R-based SL; the R:R work | E-006; the E topics (E-008 is named at A-008_v2_design_inputs.md line 28) | OQ9-15 to OQ9-17 | not started | - |
| Timeframes | D-007 | OQ9-06; no hierarchy | BLOCKED / NOT CLOSED (DR-073) | unchanged |
| Formations | D-006 | OQ9-07 | DEFERRED / NOT CLOSED (DR-072) | unchanged |
| Market context | D-008 | separate from stock-level S&R | V2 REQUIREMENTS RECORDED (DR-075) | unchanged |
| Market data (weekly and monthly candles) | K-009 | OQ9-24 | not started | - |
| Data-quality alerts | the alert topics (owner not named) | OQ9-21 | not started | - |
| Recording design | not named | OQ9-22 | - | - |
| Testing | Section N | none proposed here | not started | - |

The D-006 and D-007 dependencies (the requirements record, section 7):
- "D-007 (BLOCKED / NOT CLOSED, DR-073): Rama's timeframe ideas are recorded as raw ideas only, with no hierarchy; the timeframe dependency stays OPEN / UNRESOLVED." DR-074's rule "is that a topic which cannot be completed without a timeframe decision records it OPEN / UNRESOLVED and stops with a narrow V2-only proposal. INTERPRETATION: this draft does not trigger that rule, because it decides no requirement; any later decision on RQ9-02 or RQ9-03 would." (OQ9-06)
- "D-006 (DEFERRED / NOT CLOSED, DR-072): no D-009 input needs a formation decision. "Pivot" occurs only as legacy names (E9-02, E9-15). If S&R levels are later defined from pivots or swings, that dependency is recorded OPEN / UNRESOLVED (INTERPRETATION; OQ9-07)."
- Neither topic's status changes (the package change log, RQ-C1; DR-076).

D-008 (FACT (V2 RECORD), the requirements record, section 6): "DR-075 (DECIDED; D-008): late entries and entries near the day's high go to D-009 to D-016, "OPEN in those topics". RQ8-01 keeps broad-market context out of Trade / Reject qualification; stock-level S&R is a separate matter." The quoted status is the cell of D-008_v2_requirements.md line 202.

Bookmarks and annotations, tracking only (V2_DESIGN_LOG; each OPEN; "none is a requirement or a design decision, and none changes an owner", storage plan E-5):

| ID | Point, in short | Cross-check with | Status |
|---|---|---|---|
| BK-76 | RQ9-03 and OQ9-15 to OQ9-17, with the recorded conflicts; "no SL or R:R rule is decided at D-009" | E-006 (S&R-based SL); E-008 (R:R) | OPEN |
| BK-77 | OQ9-24: when S&R levels are computed, and where the weekly and monthly candles of Rama's raw timeframe ideas come from; "the timeframes themselves wait on D-007 (BLOCKED / NOT CLOSED, DR-073)" | K-009 (Market data) | OPEN |
| BK-78 | OQ9-04 and RQ9-10 (room to S&R and late entry, a raw idea); "no entry rule is decided at D-009" | D-013 (Entry zone), D-014 (Breakout / breakdown conditions), D-015 (Retest / reclaim), D-016 (Entry timing) | OPEN |

- VP-08 (line 259), annotation of 30-Oct-2026: "D-009 (30-Oct-2026; tracking only): not ratified at D-009; Rama's D-009 answers are raw ideas (DR-076); his Q7 answer opens with the guide's suggested wording, "Defer detailed R:R behavior to the E-topic", and adds a rough concept, kept as a raw idea, of checking "whether this R:R points within S&R" before an order (D-009_v2_requirements.md, RQ9-03 and RQ9-11; OQ9-08). The Ratify-at cell is unchanged." Its Ratify-at cell reads "Count DECIDED (DR-031); D-003, D-009 to D-017, J".
- Q-35 (line 234), item (e): "Note 2026-10-30 (D-009; tracking only): (e) Rama's answer to the D-009 answer sheet, Q14, in the answer options the guide offered: "No, I do not recall that behaviour or Not sure"; a personal recollection, not legacy evidence; the testing shadow's output and the testing code's S&R target and stop behaviour are section 8, row 17; (e) stays OPEN."
- VP-44 (line 294) is not annotated at D-009; D-009 is not among its ratification points.

Later downstream records (chronology). D-009 was stored at 10:06 IST on 30-Oct (9c9c2e9). Later the same day came D-010 (DR-077; 7c725ed, 18:37 IST), D-011 (DR-079; d665719, 19:42 IST) and D-012 (DR-081; 3674847, 20:32 IST), then the topic workflow (DR-082, DR-083; 6bd80a5). Each later record names D-009's questions as its own dependencies and leaves them open; none revised a D-009 record, and none is a D-009 decision. FACT (V2 DECISION) for each topic's own rows:
- D-010 (DR-077): "D-009's OQ9-01 to OQ9-25 stay OPEN; RQ10-01 bears on OQ9-25 (INTERPRETATION), which stays OPEN." Its RQ10-02 (D-010_v2_requirements.md line 74): "Each stock has its own S&R references, shared across all strategies. How individual strategies consume those references remains open under OQ9-20." Its decision record lists "D-009 OQ9-13 to OQ9-18, OQ9-20 to OQ9-22, OQ9-24 and OQ9-25 OPEN" among its dependencies (line 237), and its section 4 row uses section 8 rows 8, 12 and 18 as "the evidence trail; no new row". D-009's RQ9-04 keeps its recorded status (PROVISIONAL WORKING POSITION) and OQ9-20 stays OPEN.
- D-011 (DR-079): its 18:47 answer's list (strength "must not independently determine" Trade/Reject, trade priority or score, entry eligibility, or SL or target) "is carried to D-009, D-013 to D-018 and E-005 to E-007 as an input, not a decided limit"; "D-009's OQ9-01 to OQ9-25 stay OPEN." BK-82 (line 706) cross-checks with D-009 and cites OQ9-14's "a closer level competes with a stronger one"; section 8 row 22 refers to row 21 (NR9-14).
- D-012 (DR-081): "Inputs to D-009, D-013 to D-018, E-005 to E-007 and the design, not decided limits (Rama's follow-up choice)"; "D-009's OQ9-01 to OQ9-25 stay OPEN." BK-84 (line 708) cross-checks with D-009; its section 4 row uses section 8 rows 8 and 21 as "the evidence trail; no new row".
- VP-08 was annotated again at D-010, D-011 and D-012, each time "not ratified" and "The Ratify-at cell is unchanged."

This documentation (FACT (V2 DECISION), DR-083; FACT (V2 RECORD), BK-85):
- DR-083 requires each Route B topic to have a V2 topic manual and review notes beside its requirements record, and records that "D-009 holds Claude's review notes on Rama's raw ideas, and D-010 to D-012 hold independent review records, which, like the requirements files, do not by themselves satisfy this decision (point A). Their published records stay as they are, and their recorded statuses are unchanged: none is marked complete under this decision while BK-85 is OPEN (point D)."
- BK-85 (line 709, OPEN) leaves open whether and how D-008 to D-012 receive these files, and whether ChatGPT's re-audit also covers D-008 and D-009: "for Rama and ChatGPT to decide". D-009's own status does not change because this manual exists.

## 10. Known defects, process deviations and lessons, as recorded

Known defects:
- The D-009 records record no KNOWN DEFECT of their own.
- KNOWN DEFECT and ambiguity labels carried from frozen manuals, as the evidence sheet records them: E9-11 (C-008_manual.md section 9.1, line 305, labelled "KNOWN DEFECT (305) as recorded"); E9-38 (A-009_manual.md section 19, item 12, "defect or ambiguity"); E9-33 (A-008_manual.md section 17, a "KNOWN AMBIGUITY" in a scanner description).
- The inconsistencies in Rama's ITC example are not defects: the register records them "as inconsistencies in the raw example, not corrected" (section C), and the provenance checklist confirms that "No raw answer was silently corrected" (check 2).

Process deviations and limits, as recorded (none is new here):
- Stage 1 (the read log, section 2): DV-01 (four reads each covered two adjacent headings, against "One heading-bounded section per read"); DV-02 (a locator printed two lines outside the authorised headings, not used as evidence); M-01 (two locator searches); M-02 (re-reads for verbatim quotation); M-03 (adjacent V2 rows in one call); M-04 (B-006 section 15 read for TH11). ChatGPT: "ACCEPTED AS DISCLOSED, NOT ERASED"; "Do not describe the process as perfectly compliant or repeat these deviations in later stages."
- The register's revision 1 before CR-1 was edited in place and not kept; CL-29 records the change word for word, and ChatGPT: "the comparison gap must remain disclosed" (UO-05).
- Copies not compared or not received (UO-04, UO-06; the manifest's notes; ChatGPT's post-push review, section 7): the four suffixed ChatGPT copies (the guide "(1)", the register "(1)" and "(2)", the change log "(1)") NOT COMPARED; the copy that Rama's screenshot of 09:58 names "D-009_V2_REQUIREMENTS_DRAFT(1)" not compared with the stored bytes; ChatGPT's 29-Oct handoff, the ZIP packages its messages name and the screenshot of Claude's delivery summary not received.
- Changes beyond the review's listed corrections, logged (UO-15) and kept by ChatGPT ("KEEP"): RQ-C1a (the D-007 and D-006 bullets of section 7), OC-03 (the D-009 note under its storage date) and OC-07 (row 1's storage-ready treatment).
- The provenance checklist's check 9: SHA-256 identities, a machine ASCII check and the guide copies' byte comparison "NOT DONE in this step"; they were left to storage.
- Hashes are "Claude-reported local hashes, not independently verified by ChatGPT" (the manifest); ChatGPT's post-push review later checked the commit, tree, file counts and the manifest's hash on GitHub, but not a fresh clone of all 38 files.
- The post-push record's disclosure 1: "The first fetch command, at 10:01:33 IST, did not run in the repository ... nothing was fetched, read or written."; the checks were run again at 10:01:48 IST.
- Reviewer paraphrases replaced by Rama's words in the register (WC-03): Q7 "for risk point is above 'support point'" (the review had "must be above support"); Q2 "else outright reject" (the review had "otherwise reject").

Lessons (V2_DESIGN_LOG section 1; each labelled "Historical process lesson (not a system requirement)"; the rule column is ChatGPT's wording, PKG9-Q8):
- 2026-10-29 (line 179), from the Stage 1 deviations: "A bounded source read should be logged against the approved heading/section boundary; deviations must be disclosed, not erased."
- 2026-10-29 (line 180), from ChatGPT's paraphrase of Q7: "A reviewer paraphrase must not replace the speaker's exact words in a provenance-sensitive answer register."
- 2026-10-29 (line 181), from the answers that adopt the guide's wording: "Where a response adopts reviewer-suggested wording, label that provenance instead of presenting it as an independently originated requirement."
- A later lesson that names D-009 (2026-10-30, line 194, recorded with DR-083): D-008 to D-012 were stored without the manual and review-notes files (D-009 holding Claude's review notes on Rama's raw ideas); its rule: "Before a package is drafted and again before it is published, its folder is compared with the established folder structure and the most recently completed comparable topics (W-14; Topic_Workflow.txt section 5); each missing item is supplied or its absence justified in the package, and a defect found in an earlier folder is reported, not copied."

## 11. Status

- D-009: DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076); NOT FROZEN; no D-009 requirement is decided; RQ9-01 to RQ9-11 RAW IDEA, PROVISIONAL WORKING POSITION, OPEN / UNRESOLVED or NOT DECIDED, as recorded; OQ9-01 to OQ9-25 OPEN, none closed.
- NR9-01 to NR9-14 OPEN in V2_DESIGN_LOG section 8 (rows 8 to 21), evidence gaps, not findings. BK-76 to BK-78 OPEN, tracking only. VP-08 PROPOSED except its count, not ratified at D-009; Q-35 (e) OPEN.
- D-006 DEFERRED / NOT CLOSED (DR-072); D-007 BLOCKED / NOT CLOSED (DR-073); D-008 V2 REQUIREMENTS RECORDED (DR-075). Route B (DR-074, W-11) and option C (DR-073) in force; no legacy source was accessed for this manual, and docs/inputs/legacy_baseline/ was not used, as Claude reports (PD-01 in the batch decision record).
- Companion files in docs/agenda/d-009_support-resistance/: D-009_v2_requirements.md (authoritative), the 13 other records listed in section 2, and, from this batch, D-009_review_notes.md and D-009_manual_manifest.md.
- Nothing here changes a decision or status. This manual decides no requirement, closes no question, ratifies no VP row, moves no owner and does not mark D-009 complete under DR-083; BK-85 stays OPEN.
