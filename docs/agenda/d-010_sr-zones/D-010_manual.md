# D-010 - S&R ZONES (V2 TOPIC MANUAL)

## 1. Document status, purpose and scope

- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077). In the requirements record's own words (D-010_v2_requirements.md, section 13): "D-010: V2 REQUIREMENTS RECORDED (DR-077): RQ10-01 to RQ10-05 DECIDED at requirement level; the rest OPEN or PROPOSED; not frozen." Its header adds: "Requirement level only: no design or implementation decision." DR-077's status cell reads "DECIDED". D-010 was the third Route B topic (DR-074, W-11).
- This is a V2 topic manual (DR-083), written in the documentation batch of 31-Oct-2026 (DR-084). It explains the topic and its records. D-010_v2_requirements.md is authoritative for requirements and decisions; this manual decides nothing, changes no status and adds no requirement.
- Baseline: the V2 repository at 6bd80a5. The fifteen D-010 records in docs/agenda/d-010_sr-zones/ were stored by commit 7c725ed on 30-Oct-2026 and have not changed since (the repository history of that folder shows 7c725ed only). Times are IST and, unless another day is named, on 30-Oct-2026. Paths are relative to the repository root. Line numbers are those at 6bd80a5; the documentation batch's commit moves V2_DESIGN_LOG lines 195 to 393 down by 2, lines 394 to 619 by 3 and later lines by 5. Nothing under docs/inputs/legacy_baseline/ was used for this manual, as Claude reports (PD-01 in the batch decision record: the D-010 drafting subagent's git grep read the text of that folder's files).
- Purpose: DR-083 describes the manual as a "readable, detailed topic chapter explaining the subject, evidence, requirements, boundaries, unresolved matters and cross-references." This one does that for D-010: what the topic was asked, what Rama decided at requirement level, what stays open and where it waits, the legacy evidence and its limits, how D-010 stands against its neighbours, and where each record sits.
- In scope, as recorded (the requirements record, section 1, "Working boundary (P4: ChatGPT's Step 2 reconciliation, sections 2.1 and 2.4; not frozen)"): "D-010 records how V2 represents and describes S&R references, at requirement level"; it may record representation requirements "without assigning S&R any Trade/Reject authority, screening gate, score, ranking or decision effect" (OQ10-19).
- Terminology (OQ10-20, "ChatGPT's recommendation, applied"): the records use "S&R zone" for D-010's subject, apart from D-013's "Entry zone", and "S&R reference" for "one support or resistance item". This manual uses both terms in that sense. The requirements record adds, as INTERPRETATION (Claude): "in RQ10-01's terms an S&R zone is a reference's surrounding reaction area; this is a reading, not a definition."
- Out of scope, as recorded (the requirements record, section 1):
  - D-009 keeps whether and how S&R informs the trade decision (OQ9-13 OPEN), "nearest" in the trade-fit sense (OQ9-14), SL, target and R:R relations, short-side trade symmetry (OQ9-18) and strategy use (OQ9-20);
  - D-011 strength; D-012 freshness; D-013 to D-018 and the E topics: entry, timing, score, Trade / Reject, SL and R:R;
  - J-003 and J-013: the disposition of the legacy zone mechanisms, with no legacy research under D-010 (OQ10-02, OQ10-16);
  - any design: "No design: no method for finding a reference or sizing its area, width, tolerance, touch count, pivot or swing rule, timeframe or hierarchy, lookback, merge or cluster rule, strength or freshness measure, field list, schema, storage, gate, score or trade rule."
- This documentation and BK-85: DR-083 requires a V2 topic manual and review notes beside each Route B requirements record, and BK-85 (V2_DESIGN_LOG section 7, OPEN at 6bd80a5) records that D-010 was stored without them and that "none is marked complete under DR-083 while this bookmark is OPEN (point D)". This manual does not close BK-85, mark D-010 complete or change D-010's status.

## 2. Records and how to read them

### 2.1 The topic folder (docs/agenda/d-010_sr-zones/)

The fifteen files stored by 7c725ed (storage manifest, rows 1 to 14, and the manifest itself). This manual and D-010_review_notes.md are added beside them by the documentation batch of 31-Oct-2026.

| # | File | What it is | Status or revision, as recorded |
|---|---|---|---|
| 1 | D-010_STAGE0_SOURCE_INVENTORY_AND_RESEARCH_PLAN_30-OCT-2026.txt | Stage 0 output 1 (Claude): permitted sources, method, candidates C10-01 to C10-41, the proposed Stage 1; discloses DV-01 to DV-03 (section 7) | banner "LIMITED / INCOMPLETE - LOCATOR-ONLY; NO FINDINGS; NO DECISION."; accepted "as a candidate list, not as a validated evidence set" (ChatGPT's Stage 0 review, section 2) |
| 2 | D-010_STAGE0_CANDIDATE_SOURCE_REGISTER_30-OCT-2026.txt | Stage 0 output 2: locator rows L10-01 to L10-81, provenance classes, NF-1 to NF-7 | as row 1 |
| 3 | D-010_STAGE0_OPEN_QUESTIONS_AND_SCOPE_BOUNDARY_30-OCT-2026.txt | Stage 0 output 3: the scope table and the wording of OQ10-01 to OQ10-20 | Revision 1 (14:19 to 14:31): SC-01 for OQ10-18 only; "the rest of the file is as delivered at Stage 0" |
| 4 | D-010_STAGE1_EVIDENCE_SHEET_30-OCT-2026.txt | Stage 1 evidence rows E10-01 to E10-42, candidate dispositions, U10-01 to U10-13 | "ACCEPTED AS A BOUNDED EVIDENCE COLLECTION, SUBJECT TO THE QUALIFICATIONS BELOW." (D010_Stage1_Review.txt, section 1) |
| 5 | D-010_STAGE1_READ_LOG_30-OCT-2026.txt | baseline, locator searches LOC-01 to LOC-10, reads R10-01 to R10-24, what was not read and why | as row 4 |
| 6 | D010_STAGE1_SUPPLEMENTAL_RAMA_INPUT_REGISTER_30-OCT-2026.txt | "the register": Rama's Stage 0 answers quoted exactly (RI-01 to RI-17); the chart (IN-02) and the CSV (IN-03) as supplied input artifacts | accepted "as raw/provisional input record" (D010_Supplemental_Package_Review_30Oct2026.txt, section 7) |
| 7 | D010_STAGE1_REVIEW_RESPONSE_AND_OPEN_ITEMS_30-OCT-2026.txt | "the open-items file": AD-01 to AD-12, CF, RW and IP rows, UK-01 to UK-13, OD-01 to OD-09 | accepted "as an issue/clarification register" (the same review) |
| 8 | D010_STAGE1_CHANGE_CONTROL_NOTE_30-OCT-2026.txt | the files made in the supplemental step and how the inputs were read | accepted "as the local-file change record for this supplemental step" (the same review) |
| 9 | D-010_STAGE1_CLOSURE_RECORD_30-OCT-2026.txt | the Step 1 closure: Stage 1 and the supplement accepted, DV-01 to DV-03 kept, OQ10-18, the governance status at 14:35 | "ACCEPTED." (D010_Step2_Reconciliation_Review_30Oct2026.txt, section 1) |
| 10 | D010_STAGE1_SUPPLEMENTAL_CLARIFICATIONS_30-OCT-2026.txt | RC-01 and RC-02 (Rama-authorised, drafted by ChatGPT), RC-03a (Rama's words), RC-03b (ChatGPT's expanded wording) | Revision 1 (RC-03b's heading, on D010Review.txt); accepted (ChatGPT's chat, 15:19) |
| 11 | D010_STAGE1_PACKAGE_MANIFEST_AND_HANDOFF_30-OCT-2026.txt | navigation aid and status snapshot of Stages 0 and 1 | its A.1 heading corrected on D010_Correction_Only_Instructions.txt; accepted by ChatGPT (the status record, section 1.2) |
| 12 | D-010_STAGE2_STATUS_RECORD_AND_EXECUTION_PLAN_30-OCT-2026.txt | "the status record": from 16:00 D-010's working status record; the Stage 2 authority, plan (S2-T1 to S2-T11), execution log (X2- and R2- IDs), the decision request and Rama's answers (section 4) and the closing status (section 5) | banner "WRITTEN FROM 16:05 IST"; section 5, S2-SC-02 to S2-SC-04, supersedes the banner's 16:05 state, which stays as written |
| 13 | D-010_v2_requirements.md | the requirements record (DR-074's output) | authoritative; "V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077)"; NOT FROZEN |
| 14 | D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | the W-12 review: reviewer, scope, findings F-01 to F-18 and their resolutions, verdict | recorded by Claude, "the package's author", from the reviewer's report; verdict "PASS WITH FINDINGS. There is no BLOCKER" |
| 15 | D-010_STORAGE_MANIFEST_30-OCT-2026.txt | the 34 rows stored by 7c725ed with SHA-256 and bytes, the governance changes, the naming choices and the checks | does not list itself; its SHA-256 is in the post-push record, section 3 |

- Rows 1 to 11 keep status lines that were true when written ("Stage 1 is NOT AUTHORISED.", "Stage 2: NOT AUTHORISED.", "Local file only"). The storage manifest (section 1) calls them historical snapshots, under TS_D010_Authorise.txt, Part 2: "do not rewrite them"; the status record, section 1.2, lists each with its line.
- Five file names begin "D010_". For three of them (rows 6 to 8) the closure instruction wrote "D-010_"; the closure record keeps the prefix "that D010_Stage1_Review.txt, section 9, set", and ChatGPT's Step 2 says "No rename is required." Rows 10 and 11 carry the names ChatGPT's instructions gave them (D010_Claude.txt; D010_Next_Instructions.txt).

### 2.2 Inputs in docs/inputs/

- docs/inputs/reconciliation/ (stored unchanged by 7c725ed under the names received, the 8-character upload prefix dropped; storage manifest rows 15 to 30):
  - ChatGPT's Stage 0 set, received 10:40: D-010_CLAUDE_RESEARCH_PROMPT_STAGE0_30-OCT-2026.txt; D-010_STAGE0_CHATGPT_REVIEW_CHECKLIST_30-OCT-2026.txt; D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026_BLANK_AS_ISSUED.txt (the sheet as issued; renamed because the completed sheet has the same name).
  - Rama's completed answer sheet, received 13:39: D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026.txt. It is the source of Rama's Stage 0 words (register IN-01); the register quotes his answer lines exactly.
  - ChatGPT's reviews and instructions: D010_Stage0_Review_Stage1_Authorization.txt (11:17); D010_Stage1_Review.txt (13:39); D010_Supplemental_Package_Review_30Oct2026.txt (14:13); D010_OQ10-18_Administrative_Closure_Instruction.txt (14:19); D010_Claude_Instruction_Step1_Closure_Only.txt (14:35); D010_Step2_Reconciliation_Review_30Oct2026.txt (14:45); D010_Claude.txt (15:05); D010Review.txt (15:15); D010_Next_Instructions.txt (15:21); D010_Correction_Only_Instructions.txt (within 15:21 to 15:46).
  - Rama's authority, through ChatGPT: TS_D010_Authorise.txt (16:00).
  - Rama's supplied CSV, received as EQUITY_L.csv at 13:39: EQUITY_L_AS_RECEIVED_2026-10-30.csv (register IN-03). The dated name is "the day it was received, not the file's own date, which is UNKNOWN (UK-05)" (storage manifest, section 3). Its name does not carry the topic ID, so a search of docs/inputs/ by the ID does not find it; the storage manifest lists it (row 30).
- docs/inputs/chatgpt_originals_and_replies/: D-010_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt, Claude's post-push record of 7c725ed, stored with the next package (d665719, D-011).
- Not stored: Rama's BANKBARODA chart (IN-02) and every screenshot of 30-Oct (DR-008; their content used is transcribed in the records); three ChatGPT files that never arrived (package manifest, A.3); ChatGPT's chat acceptance of 14:31 (a screenshot, not stored) and its messages of 14:46 and 15:19, which "exist as no file" (package manifest, A.3).

### 2.3 Register rows, design-log entries and the post-push record

- V2_DECISION_REGISTER.md:
  - DR-077 (line 88): D-010's requirements; "DECIDED".
  - DR-078 (line 89): W-12, the workflow rule set at D-010; status "DECIDED (its automatic move to the next topic, the sequence beyond D-012 and the ChatGPT audit only after publication SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026)".
  - Later rows naming D-010: DR-079 (line 90, D-011, "started under W-12 (DR-078) after D-010's publication"; its package carries "the records owed by the D-010 package"); DR-082 (line 93, W-14; the supersession in DR-078); DR-083 (line 94; "D-010 to D-012 hold independent review records, which, like the requirements files, do not by themselves satisfy this decision (point A)").
  - Earlier rows D-010's records cite: DR-031, DR-069 and DR-072 to DR-076 (the requirements record, section 6).
- V2_DESIGN_LOG.md, at 6bd80a5:
  - section 4, the D-010 row (line 372);
  - section 5, W-12 (line 391), with its "Update 2026-10-30 (DR-082)";
  - section 6, under "### 2026-10-30": the D-010 session note (line 610) and the D-010 push line (line 611);
  - section 1, lessons dated 2026-10-30: lines 182 to 185 (D-010's four, DR-077), 186 to 188 (from D-010's independent review, recorded with the D-011 package) and 192 and 194 (recorded with the topic-workflow package);
  - section 3, VP-08's D-010 tracking note (line 259);
  - section 7, BK-79 to BK-81 (lines 703 to 705), with D-011's and D-012's notes on BK-80, and BK-85 (line 709);
  - section 8, rows 8, 12 and 18 (lines 724, 728 and 734): "the evidence trail" (the section 4 row); no new row.
- The post-push record (section 2.2) gives the commit, the push and the fresh-clone check. Its push line is byte-equal to line 611.

### 2.4 Which record governs what

| Matter | Governing record | Note |
|---|---|---|
| D-010's requirements and decisions; the status now of each OQ10 question | D-010_v2_requirements.md (sections 2.1 and 9) | the Stage 0 open-questions file holds the questions' wording and "is not edited" |
| The question-tool text and Rama's answers, word for word | the status record, section 4 | the requirements record quotes the decided wording from it exactly |
| Rama's Stage 0 words | the completed answer sheet in docs/inputs/reconciliation/; the register (RI-01 to RI-17) | the questions, options and notes are ChatGPT's (register, section 2) |
| RC-01 to RC-03 and their attribution | the clarifications file, Revision 1 (with D010_Claude.txt) | |
| Stage 1 evidence | the evidence sheet and read log | they describe Stage 1's reads only (closure record, section 1) |
| Unknowns and dependencies (UK, OD) and their handling | the open-items file; ChatGPT's Step 2, sections 2.3 and 2.4 | |
| D-010's status in V2 | DR-077; V2_DESIGN_LOG section 4, line 372 | |
| The workflow rule set at D-010 | DR-078 and W-12, as DR-082 (W-14) later superseded parts of them | |
| The pre-push review | D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | |
| What was stored, with hashes | the storage manifest; the post-push record | |
| Legacy behaviour not researched | V2_DESIGN_LOG section 8 ("The one canonical list of legacy behaviours that a Route B topic needed and that no frozen manual establishes") | |
| Cross-topic points | V2_DESIGN_LOG section 7 | |

## 3. Evidence labels

| Label | Meaning in this manual |
|---|---|
| FACT (V2 DECISION) | a register row and what it records, cited by ID, with the status in its row |
| FACT (V2 RECORD) | text or a status in a stored V2 record (a topic file, the design log, a file in docs/inputs/), cited by file and section or line |
| FACT (HISTORICAL) | a frozen manual's statement with its qualifiers, cited by manual and section as D-010's accepted Stage 1 evidence sheet cites it. No frozen manual was re-read for this manual. It is never evidence of what a host ran. |
| RAMA (STATED) | Rama's own words, quoted exactly from the record named |
| RAMA-AUTHORISED, DRAFTED BY CHATGPT | wording that ChatGPT drafted at Rama's request or that Rama issued as his, where a record attributes it so: here RC-01, RC-02 and the follow-up answers relayed at 17:55 |
| INTERPRETATION | a reading, named with whose it is (Claude's or ChatGPT's, in the D-010 records; this manual's own, where marked); never a decision |
| PROVISIONAL | the status of a raw input or a working position in the record's own words, for example the register's "RAW / PROVISIONAL USER INPUT" |
| UNKNOWN / NOT ESTABLISHED | not shown by the records |
| NOT RESEARCHED | a legacy behaviour that no frozen manual establishes (W-11; V2_DESIGN_LOG section 8) |
| KNOWN DEFECT | a defect the records name, with its correction as recorded |
| OPEN QUESTION | a question the records keep open, with its ID, status and where it waits |

- The D-010 records also use labels of their own, among them "DECISION (RAMA)", "PROPOSAL (Claude)", "OPEN QUESTION / DEPENDENCY" and "SUPPLIED INPUT ARTIFACT". Where this manual gives them, they are quoted as the record's words; they are not labels of this manual. No label is merged with another.
- Quotes are exact. A quote that runs over several source lines joins them with one space; "[...]" marks words left out. Where a source has non-ASCII characters, the ASCII substitution its D-010 record states is used (the rupee sign as INR, a long dash as "--", curly quotation marks as straight ones).

## 4. The question the topic answers

- FACT (V2 RECORD): the Master Agenda lists the topic in Part 5, Section D (TRADE DECISION ENGINE), at line 301: "D-010  S&R zones". It gives D-010 no other scope text: NF-4, "none located: UNKNOWN / NOT ESTABLISHED" (Stage 0 output 2, section 7).
- The Section D goal (lines 311 to 317): "Should this signal become a trade?"; "This section is especially important for the known redesign concerns: late entry, unsuitable filtration, incorrect S&R range, and trading near high-of-day without adequate S&R context."
- The same name is used again in Section J ("J. S&R / ADVANCED ENGINES"): J-003, "S&R zones" (line 439), under J's rule that asks for each advanced mechanism KEEP, MODIFY, REPLACE, REMOVE or SHADOW and ends: "Presence of code does not automatically mean the mechanism belongs in the new live decision path." (lines 459 to 460). The records keep the two apart: "D-010 collects V2 requirements; J-003 later reviews the legacy mechanism's disposition under its own governance." (ChatGPT's Stage 0 review, OQ10-02).
- What already pointed to D-010 when it started (FACT (V2 RECORD), as the Stage 1 evidence sheet quotes D-009's records):
  - D-009's OQ9-25, "Does V2 use S&R points, zones or both?", with "D-010 (zones)" in its who-decides cell and the note "D-009 records the dependency only" (E10-05);
  - D-009's working boundary, "Zones go to D-010", recorded as "a PROVISIONAL WORKING POSITION, not frozen" and ChatGPT's proposal (E10-03);
  - OQ9-14's note "zones, strength and freshness themselves are D-010 to D-012's" (E10-06);
  - section 8 row 12's routing cell, "zone semantics are D-010's" (E10-08).
- The two Section D concerns that name S&R, as the records hold them:
  - "incorrect S&R range": OQ10-15, UNKNOWN / NOT ESTABLISHED. The A-001 frozen manual records a user report, "Incorrect or unsuitable S&R range or interpretation." (A-001_manual.md section 9, line 63, as NR9-11 records it; E10-17), and Rama said at D-009: "No, I do not recall". ChatGPT's Step 2: "Do not infer that it refers to a zone or reinterpret it."
  - "trading near high-of-day without adequate S&R context": the D-010 records quote it as part of the goal and take it no further. D-008 routes "Late entries; entries near the day's high" to D-009 to D-016 (E10-40), and D-009's BK-78 sends its related item to D-013 to D-016.
- The questions D-010 was given (FACT (V2 RECORD)):
  - ChatGPT's Stage 0 prompt says "The exact requirement scope is not yet established." and lists candidate questions "to locate (not answer)": what a zone means against a single price, a band and its boundaries, the evidence that may define a zone, whether support and resistance are treated symmetrically, whether timeframes stay separate or are grouped, overlapping zones, and attributes. It adds: "These are research prompts only. Do not define the correct answer or select a model." (D-010_CLAUDE_RESEARCH_PROMPT_STAGE0_30-OCT-2026.txt, "D-010 SUBJECT BOUNDARY").
  - ChatGPT's answer sheet put fourteen questions to Rama, Q1 "WHAT DO YOU MEAN BY AN S&R ZONE?" to Q14 "ANY OTHER REQUIREMENT OR EXAMPLE?", with an "IMPORTANT CONFIRMATION" that it asks him to approve no width, pivot, touch count, timeframe, strength, freshness, entry, SL, target, R:R, gate, shadow phase or schema.
  - Claude's Stage 0 framed OQ10-01 to OQ10-20 (output 3, section 2), which the requirements record, section 9.1, carries with their status now (section 7.1 below).

## 5. Course of the topic

| Time (IST) | Step | Who | Record |
|---|---|---|---|
| 10:06 | the D-009 package committed (9c9c2e9, 38 files; commit time) and pushed, the base for D-010 | Claude | V2_DESIGN_LOG section 6, line 609 |
| 10:40 | ChatGPT's Stage 0 prompt, review checklist and blank answer sheet received | ChatGPT, sent by Rama | Stage 0 output 1, header |
| by 11:05 | Claude's three Stage 0 files, locators only, with DV-01 to DV-03 disclosed (DV-03 at about 11:03) | Claude | output 1, section 7; the requirements record, section 12 |
| 11:17 | ChatGPT's Stage 0 review: "STAGE 0 ACCEPTED WITH DISCLOSED PROCESS DEVIATIONS."; "STAGE 1: CONDITIONALLY AUTHORISED WITH THE BOUNDARIES IN SECTION 5." | ChatGPT | D010_Stage0_Review_Stage1_Authorization.txt |
| 11:22:35 to 11:27:00 | the Stage 1 reads R10-01 to R10-24 (24 of the 25 allowed), after locators LOC-01 to LOC-10 | Claude | read log, sections 2 to 4 |
| by 11:44 | the Stage 1 evidence sheet and read log | Claude | the requirements record, section 12 |
| 13:39 | Rama's completed answer sheet, the BANKBARODA chart, EQUITY_L.csv and ChatGPT's Stage 1 review received | Rama; ChatGPT | register, section 1 (IN-01 to IN-05) |
| by 13:55 | the register, the open-items file and the change-control note (the note: prepared "between 13:39 and about 14:00") | Claude | the requirements record, section 12; change-control note, header |
| 14:13 | ChatGPT's supplemental review: "ACCEPTED AS A SUPPLEMENTAL, LOCAL, RAW-INPUT AND OPEN-ITEM RECORD", with its own 242.69 correction | ChatGPT | D010_Supplemental_Package_Review_30Oct2026.txt |
| 14:19 to 14:31 | the OQ10-18 instruction (14:19); Revision 1 of Stage 0 output 3 (SC-01); ChatGPT's acceptance in chat (14:31) | ChatGPT; Claude | Stage 0 output 3, section 6; closure record, section 4 |
| 14:35 to 14:45 | the closure instruction (14:35); the Stage 1 closure record; ChatGPT's acceptance and its Step 2 reconciliation (14:45) | ChatGPT; Claude | D010_Step2_Reconciliation_Review_30Oct2026.txt |
| 14:46 | ChatGPT amended its clarification approach "so that Rama answers in his own words" (a chat message relayed as text) | ChatGPT | package manifest, sections A.3 and C |
| 15:05 | D010_Claude.txt: clarifications 1 and 2 drafted by ChatGPT at Rama's request, clarification 3 from his message; the clarifications file | Rama; ChatGPT; Claude | clarifications file, section 1 |
| 15:15 to 15:19 | D010Review.txt: "ACCEPTED WITH ONE WORDING CORRECTION RECOMMENDED."; Revision 1 (RC-03b's heading); ChatGPT's acceptance in chat (15:19) | ChatGPT; Claude | clarifications file, header |
| 15:21 to 15:46 | D010_Next_Instructions.txt (15:21); the package manifest (from 15:23); D010_Correction_Only_Instructions.txt: "ACCEPTED WITH ONE FACTUAL QUALIFICATION REQUIRED."; the A.1 heading corrected; ChatGPT's acceptance, ending "Next: Claude remains on HOLD. Stage 2 is NOT AUTHORISED." | ChatGPT; Claude | the requirements record, section 12; the status record, section 1.2 |
| 16:00 | TS_D010_Authorise.txt (Rama, through ChatGPT), with three screenshots of Rama's messages to ChatGPT: the permanent workflow rule and the Stage 2 authorisation | Rama, through ChatGPT | the status record, sections 1.1 and 1.2 |
| 16:04 to 16:28 | the controlling records re-read (X2-01 to X2-05); the status record and plan (from 16:05); the reads R2-01 to R2-07 | Claude | the status record, sections 2 and 3 |
| about 16:30 to 16:31 | Claude's four questions in the question tool (Claude's wording) and Rama's answers | Claude; Rama | the status record, sections 4 and 4.1 |
| about 16:40 | Claude's two follow-up questions (the reversed role wording; the review step) | Claude | the status record, section 4.2 |
| 17:55 | Rama's screenshot of his exchange with ChatGPT, with "Continue from where you left": ChatGPT's recommended answers to both | Rama; ChatGPT | the status record, section 4.2 |
| from 18:00 | the requirements record and the storage package | Claude | the requirements record, section 12 |
| about 18:07 to about 18:32 | the independent review (W-12): "PASS WITH FINDINGS", F-01 to F-18 | a separate Claude subagent | D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt |
| about 18:12 | a first local commit, e6722f5, never pushed | Claude | the post-push record, section 1 |
| 18:37:15 | commit 7c725ed (e6722f5 amended, all 18 findings resolved), pushed to main; fresh-clone check 18:37:37 to 18:37:39 | Claude | the post-push record, sections 2 and 3 |

### 5.1 Stage 0 (locators only)

- FACT (V2 RECORD): the prompt allowed locators only ("Do not read full passage content to make findings"). Claude located 1,236 lines with a term hit in 84 of the 107 files searched and grouped them as candidates C10-01 to C10-41 (output 1, sections 3 and 4) and locator rows L10-01 to L10-81 (output 2). Bounded negatives were recorded: no frozen manual contains "D-010" or "J-003" (NF-1), and no line attributed to Rama in the permitted records used "zone" (NF-2; elsewhere UNKNOWN / NOT ESTABLISHED).
- The proposed Stage 1 (output 1, section 6) set seven questions, S1-Q1 to S1-Q7, and three reading tiers with caps; it was "NOT AUTHORISED; NOTHING HERE IS RUN".
- Process deviations DV-01 to DV-03 were disclosed in output 1, section 7 (section 10 below).

### 5.2 Stage 1 (bounded evidence)

- ChatGPT authorised Tier 1 (named V2 records) and Tier 2 (four frozen-manual lines: D-004 lines 50 and 64; D-005 lines 15 and 158), at most 25 reads, one section or row each, with no scripts, shell, git or execution; Tier 3 was "NOT AUTHORISED at this time" (Stage 0 review, section 5).
- Claude reports (read log): 24 of the 25 reads, ten locator searches listed separately, no script, shell or git command, and "None found" under deviations. ChatGPT reviewed the two files "as documents": "This is not an independent reproduction of Claude's source reads or a byte-for-byte verification of the fresh clone." (D010_Stage1_Review.txt, section 2).
- The result: E10-01 to E10-42 and U10-01 to U10-13. No candidate was dropped; C10-29 and C10-33 narrowed (evidence sheet, section 9).

### 5.3 Rama's answers and the supplemental package (13:39 to 14:13)

- Rama answered Q1 to Q13 of the Stage 0 sheet; Q12's last item and Q14 were left blank (register, section 2). ChatGPT received the answers "AS NEW, RAW USER INPUT" that do not "automatically become a requirement" (D010_Stage1_Review.txt, section 1), set nine distinctions to carry forward (section 7) and eight focused open questions (section 8), and asked for the three supplemental files (section 9).
- The register quotes his answer lines exactly ("RAW / PROVISIONAL USER INPUT ONLY - NOT REQUIREMENTS"); the open-items file separates record facts, raw inputs, interpretations (IP-01 to IP-08), unknowns (UK-01 to UK-13) and dependencies (OD-01 to OD-09).

### 5.4 OQ10-18, the Stage 1 closure, Step 2 and the clarifications (14:19 to 15:46)

- OQ10-18 asked a process question: does Rama answer the Stage 0 sheet now or a revised sheet later? SC-01 records it as "ADMINISTRATIVELY RESOLVED / ANSWERED", because "Rama answered the Stage 0 answer sheet; no revised sheet is required for this process question." It is "An administrative, process-only closure. It closes, answers or alters no technical D-010 question" (Stage 0 output 3, section 6).
- The closure record (14:35) is an "AUDIT / STATUS RECORD ONLY - NOT AN EVIDENCE SHEET, REQUIREMENTS DRAFT OR DECISION RECORD." Its section 5 records the status then: "D-010: NO REQUIREMENT DECIDED."; "Stage 1: completed and reviewed."; "Stage 2: NOT AUTHORISED."
- ChatGPT's Step 2 (14:45) reconciled OQ10-01 to OQ10-20, U10, UK and OD and D-009's linked questions without a new register. It recommended only two targeted clarifications from Rama (UK-01, UK-02) and one on "COMMON", and "Record no requirement as approved until Rama explicitly agrees and the requirement is written into the controlled D-010 requirements record." (section 5, item 6).
- The clarifications (15:05): ChatGPT had put three questions to Rama; he asked it to answer them "on behalf of me to Claude" and gave his own words for the third (the clarifications file, section 1). RC-01 and RC-02 carry the label "Rama-authorised clarification, drafted by ChatGPT"; RC-03a is his own message; RC-03b is ChatGPT's expanded wording (section 6.1 below).
- The package manifest (from 15:23) is "NAVIGATION AND STATUS RECORD ONLY"; after its heading correction ChatGPT accepted it and kept Claude on hold.

### 5.5 Stage 2 (16:00 to the storage)

- Authority (TS_D010_Authorise.txt, Part 2, as the status record quotes it): "Rama now explicitly authorises commencement of D-010 Stage 2 -- S&R Zones -- under the existing Route B / W-11 and option C governance path." Recorded as S2-SC-01, NOT AUTHORISED to "AUTHORISED / IN PROGRESS", effective 16:00. Part 2 also kept "D-010: NO REQUIREMENT DECIDED until requirements are actually considered and approved." and "The accepted Stage 0/Stage 1 package and its disclosed process deviations remain historical records; do not rewrite them."
- The plan (the status record, section 2; INTERPRETATION of the controlling records, "no scope is added"): finish the authorised V2-record evidence; state the legacy evidence from accepted rows only; record Rama's inputs and the assistants' proposals with their labels; "put the genuine decisions to Rama once, together"; write the requirements record; store and publish. Tasks S2-T1 to S2-T11; sources P1 to P4, with P5 "not needed"; the ambiguity check found "No ambiguity found that changes the permitted sources, the scope or the decision authority".
- The reads R2-01 to R2-07 (section 3.2): Master Agenda J (lines 433 to 460), DR-031, DR-072, DR-069 by a bounded search, BK-76 to BK-78, Q-35 and Q-44, and VP-44. "Not read, by design: any frozen-manual passage (OQ10-16)".
- The decision request (section 4) asked "only what his firm D-010 answers already state, and the meaning of COMMON" and put nothing to him that he had asked to "discuss later in new system design". Section 6 below gives the answers and their provenance.
- Closing status (section 5): S2-SC-02, D-010 to "V2 REQUIREMENTS RECORDED (DR-077): RQ10-01 to RQ10-05 DECIDED at requirement level; everything else OPEN, PROPOSED or a dependency"; S2-SC-03, Stage 2 to "COMPLETED"; S2-SC-04, D-011, then D-012 and the later eligible topics, to "AUTHORISED to start in agenda order, each after the previous one is published (W-12)". S2-SC-04's sequence beyond D-012 was later superseded by DR-082 (section 6.2).

## 6. What is recorded as decided

### 6.1 D-010's requirements (FACT (V2 DECISION), DR-077)

- DR-077: "DECIDED at requirement level only, by Rama's answers of 30-Oct-2026 in the question tool". The requirements record, section 2.1, governs; it quotes the decided wording exactly from the status record, section 4. The short forms in the table are the design log's own (section 4, line 372).

| ID | In short (V2_DESIGN_LOG section 4) | Status label | Source |
|---|---|---|---|
| RQ10-01 | "a reference price plus its reaction area, where historical price behaviour supports one" | DECIDED at requirement level | the requirements record, 2.1; the status record, 4.1 (Q1) |
| RQ10-02 | "per stock, shared across strategies" | DECIDED at requirement level | 2.1; 4.1 (Q2) |
| RQ10-03 | "history-based, never arbitrary" | DECIDED at requirement level | 2.1; 4.1 (Q3, item 1) |
| RQ10-04 | "the role follows the reference's position against CMP" | DECIDED at requirement level | 2.1; 4.1 (Q3, item 2) and 4.2 (the correction) |
| RQ10-05 | "the next reference kept separate" | DECIDED at requirement level | 2.1; 4.1 (Q3, item 3) |

The decided wording, exact (the requirements record, section 2.1; RQ10-01's and RQ10-04's "- " list markers are the answers' own):
- RQ10-01: "Each S&R reference should retain: - Its reference price. - Its surrounding reaction area, where historical price behaviour supports one. The method for calculating or sizing the area remains undecided."
- RQ10-02: "Each stock has its own S&R references, shared across all strategies.  How individual strategies consume those references remains open under OQ9-20."
- RQ10-03: "S&R references must originate from the stock's own historical price behaviour, not arbitrary offsets such as CMP + 1%."
- RQ10-04: "- An S&R reference below the current market price (CMP) is a potential support reference. - An S&R reference above CMP is a potential resistance reference. - As price moves, the same historical reference can change its functional role. - These are relative market-context classifications, not permanent labels or guaranteed reversal points."
- RQ10-05: "The next historical reference in the direction of movement is separate from the reference currently being tested or broken."

Provenance, as the records give it:
- FACT (V2 RECORD): "the answers were submitted by Rama, so the choices are his decisions." The questions and their options, with their "(Recommended)" marks, were Claude's wording ("The wording is Claude's"; the status record, section 4).
- RQ10-01 and RQ10-02: Rama's Q1 and Q2 answers; who drafted their wording is "not established" (the status record, 4.1, which adds as INTERPRETATION that ChatGPT "probably drafted them too"). "None of it is presented as verbatim Rama wording."
- RQ10-03 and RQ10-05: Rama's Q3 answer, items 1 and 3, in ChatGPT's wording (ChatGPT's reply at 17:55: "My earlier Q3 wording").
- RQ10-04: Rama's Q3 answer, item 2, as corrected by the follow-up: RAMA-AUTHORISED, DRAFTED BY CHATGPT. The tool returned "[No preference]" for Claude's role check, and the correction came in the other question's "Review step" field and again in the 17:55 relay (the requirements record, RQ10-04's row; KNOWN DEFECT below).
- Rama's own words behind them (RAMA (STATED); register and clarifications file):
  - RI-01, line 33: "a professional trader always looks at chart and takes reference from historical movement when certain price is break-out or Break-down then what's the next possible price to be reached from current price";
  - RI-04, line 38: "S&R means it is expected price points when certain price is broken (rais/falls) then where is the next expected price range stock is going to settled";
  - RI-05, line 45: "So we must follow the same logic instead of playing with random numbers/prices";
  - RI-07, line 66: "when CMP is Rs 285 then Rs 277.95 acts as 'Support' whereas when CMP is Rs 270 then Rs 277.95 acts as resistance";
  - RI-12, line 118: "S&R remains same irrespective of scanned stocks/used strategies, So make it as 'COMMON'";
  - RI-15, line 145: "so these were acts like zones when it reached then either stock will go up/down";
  - RC-03a, his message to ChatGPT: "S&R points/things are not strategy specific instead it's a global thing".
- RAMA-AUTHORISED, DRAFTED BY CHATGPT, behind RQ10-01 and RQ10-05: RC-01, "I do not mean that every S&R reference must be represented only as one exact price."; RC-02, "So, the S&R area being tested or broken is one reference; the next historical S&R area in the direction of movement is a separate reference. It is a possible area for price to reach or react around, not a guaranteed destination or prediction."

The bound and what each requirement does not decide (as recorded):
- The bound closing Rama's Q3 answer, in ChatGPT's wording: "These are bounded requirements only. They do not grant Trade/Reject, scoring, SL or target authority". It covers RQ10-03 to RQ10-05; "RQ10-01 and RQ10-02 carry the same limit through the OQ10-19 boundary" (the requirements record, header; F-04).
- Not decided by them (the requirements record, section 2.1, last column): for RQ10-01, "the method and size of an area, and when history "supports one" (design); which price action counts (OQ10-04)"; for RQ10-02, "how each strategy uses the references (D-009 OQ9-20)"; for RQ10-03, "which price action counts (OQ10-04; D-006 dependency); the lookback (D-007; UK-08)"; for RQ10-04, "the trade side (long / short) of his Q4 words: D-009 OQ9-18; what counts as touching or breaking a reference (UK-03)"; for RQ10-05, "which reference is "next" or "nearest" when several compete (D-009 OQ9-14); whether it serves as an entry or a target (D-013 to D-016; E-007); RC-02: "not a guaranteed destination or prediction"".
- On RQ10-05 the record adds: "The option ticked also said "not a target or a prediction" (Claude's wording); the decided wording does not, and his examples set the TGT at the next reference" (F-09).
- KNOWN DEFECT, corrected before recording: Rama's first Q3 answer restated item 2 as "Above CMP, the reference acts as support; below CMP, it acts as resistance.", the reverse of the option ticked and of RI-07. Claude asked once more ("Not recorded until confirmed."), and the correction arrived as described above ("My earlier Q3 wording had these reversed. Correct it."). RQ10-04 carries the corrected wording (the status record, 4.2).
- INTERPRETATION (Claude), in the requirements record: "market-context" in RQ10-04 "means the reference's position relative to the stock's price, not D-008's broad-market context." The record also states: "RQ10-02 does not mean one price map for all stocks: each stock has its own references (the answer's own words)."
- RQ10-02 and Rama's D-009 words: his D-009 Q4 answer opens with ChatGPT's suggested wording, "Common baseline, with strategy-specific differences only where explicitly defined and tested", and on 30-Oct he chose "Per stock, all strategies", not the option "Shared baseline plus extras". The record concludes (INTERPRETATION (Claude)): "So the references are common; how strategies use them, including any strategy-specific treatment, stays OPEN (OQ9-20)." (section 3; F-11).
- Legacy: "RQ10-01 to RQ10-05 are new V2 requirements, not carried from legacy." (the requirements record, section 4).
- INTERPRETATION (Claude), in the requirements record: "RQ10-01 and RQ10-03 need no timeframe or formation decision: they say what a reference keeps and where it comes from, not how it is found. So neither D-007 nor D-006 blocks them (DR-074's stop rule is not triggered)." (section 3).

### 6.2 Other decisions recorded with D-010's package (not D-010 requirements)

| Item | What was recorded | Status label, as recorded | Source |
|---|---|---|---|
| Stage 2 authorisation | S2-SC-01, then S2-SC-03 at storage | "AUTHORISED / IN PROGRESS" (16:00), then "COMPLETED" | TS_D010_Authorise.txt, Part 2; the status record, sections 1.1 and 5 |
| The permanent workflow rule, the topic sequence and the review step | W-12 and DR-078, from TS_D010_Authorise.txt, Part 1, Rama's Q4 answer (who drafted its wording is not established) and his follow-up (RAMA-AUTHORISED, DRAFTED BY CHATGPT, relayed at 17:55) | DR-078: "DECIDED (its automatic move to the next topic, the sequence beyond D-012 and the ChatGPT audit only after publication SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026)" | the status record, sections 1.4 and 4.3; V2_DESIGN_LOG W-12 |
| OQ10-18 | a process question only | "ADMINISTRATIVELY RESOLVED / ANSWERED" (SC-01); no technical question closed | Stage 0 output 3, section 6 |

- What stayed of W-12 after DR-082: "the independent Claude review and the list of actions that need explicit approval stay" (W-12's update of 2026-10-30). Lesson 192 records how the "Next topic" question was read (section 10).

## 7. What is not decided: open questions, provisional positions, raw ideas, unknowns

### 7.1 D-010's open questions (the requirements record, section 9.1; the questions' wording is in Stage 0 output 3, section 2)

| ID | Question (the record's short form) | Status now (exact) | Where it waits, as recorded |
|---|---|---|---|
| OQ10-01 | D-010 against D-009, D-011, D-012 | OPEN; the working boundary of section 1 applied; not frozen | ChatGPT's working boundary (Step 2, 2.1); Stage 0's proposed handling, INTERPRETATION: "A governance decision (ChatGPT, then Rama)." |
| OQ10-02 | D-010 against J-003 | OPEN as a governance boundary; no legacy research under D-010 | J-003 and J-013 (OD-07) |
| OQ10-03 | point, band, both or other | DECIDED at requirement level (RQ10-01); the area's method OPEN (design) | the design |
| OQ10-04 | which price action evidences a reference | OPEN; D-006 dependency; P10-01 | D-006, DEFERRED / NOT CLOSED (OD-02) |
| OQ10-05 | per timeframe or combined | OPEN / UNRESOLVED on D-007 | D-007, BLOCKED / NOT CLOSED (OD-01) |
| OQ10-06 | overlapping or nearby | OPEN; P10-02 | OQ9-14 (D-009); the design |
| OQ10-07 | attributes | OPEN; P10-03; strength with D-011, freshness with D-012 | BK-80 (OPEN) |
| OQ10-08 | support and resistance as mirrors | role relative to CMP DECIDED (RQ10-04); trade-side symmetry OPEN (OQ9-18) | D-009 OQ9-18 |
| OQ10-09 | the view relative to the current price | OPEN; P10-04; "nearest" with D-009 (OQ9-14) | D-009 OQ9-14 |
| OQ10-10 | common or strategy-specific | DECIDED (RQ10-02); strategy use OPEN (OQ9-20) | D-009 OQ9-20 |
| OQ10-11 | incomplete evidence | OPEN; P10-05; OQ9-21; K-009, K-010 | K-009 and K-010 (BK-79); D-009 OQ9-21 |
| OQ10-12 | reconstructability | OPEN; P10-06; OQ9-22 | D-009 OQ9-22; K-008 and K-011 |
| OQ10-13 | weekly and monthly data | OPEN on K-009 and K-010; OQ9-24 | K-009 and K-010 (BK-77, BK-79) |
| OQ10-14 | VP-08 at D-010 | OPEN; not ratified at D-010 | Rama, "through the proper governance record" (ChatGPT's Stage 0 review, OQ10-14) |
| OQ10-15 | "incorrect S&R range" | UNKNOWN / NOT ESTABLISHED | NR9-11 (section 8, row 18) |
| OQ10-16 | legacy zone gap rows | no further D-010 legacy research; no new gap row | J-003 and J-013; section 8 rows 8, 12 and 18 |
| OQ10-17 | Stage 1 reads outside P2 and P3 | process only; the authorised Stage 1 scope was applied | - |
| OQ10-18 | answer the Stage 0 sheet now or later | ADMINISTRATIVELY RESOLVED / ANSWERED (SC-01) | - (process only) |
| OQ10-19 | representation while OQ9-13 is open | boundary confirmed by ChatGPT for the requirements discussion; applied | D-009 OQ9-13 (OPEN) |
| OQ10-20 | "S&R zone" against D-013's "Entry zone" | applied as a naming note (section 1); nothing decided for D-013 | BK-81; D-013 (NOT STARTED) |

### 7.2 Claude's proposals P10-01 to P10-07 (FACT (V2 RECORD) of proposals; the requirements record, section 3A)

- The record's label: "PROPOSAL (Claude); not Rama's; not requirements". Each answers one of Rama's requests for a "best opinion" and is marked "TO BE DISCUSSED IN THE NEW-SYSTEM DESIGN". "ChatGPT has not reviewed them; the independent review checked their labels, their fit with the records and that they decide no design". No stored record at 6bd80a5 shows a later ChatGPT review of them.

| ID | Rama's request it answers (register) | The proposal, in the record's words | Bound, as recorded |
|---|---|---|---|
| P10-01 | Q3, which price action counts (RI-06) | "Record the kinds of evidence a reference may rest on, without rules: [...]"; kinds resting on formations "wait for D-006" | no touch count, tolerance or candle rule (UK-03); D-006 DEFERRED |
| P10-02 | Q6, overlapping or nearby references (RI-09) | "Show overlapping or nearby references so that the overlap is visible; do not merge them silently." | no distance or merge rule; OQ9-14 |
| P10-03 | Q7, what each reference shows (RI-10) | "Each reference makes visible: the stock; its reference price and reaction area (RQ10-01); its current role relative to CMP (RQ10-04); the timeframe or timeframes it rests on (OQ10-05; D-007); the historical reactions it rests on, with their dates, so that it can be traced (RQ10-03); the time it was computed; and whether its evidence is complete (P10-05). Strength or confidence goes to D-011, age or freshness to D-012" | "a list of things to show, not a field list or schema" |
| P10-04 | Q8, the view relative to the current price (RI-11; RI-15, line 146) | references below (potential support) and above (potential resistance), "with each one's distance from the current price", marking "the reference being tested and the next one beyond it (RQ10-05)" | display only; "nearest" in the trade-fit sense stays D-009's (OQ9-14) |
| P10-05 | Q10, incomplete history (RI-13) | "mark its S&R evidence incomplete and show it so, never as complete"; listing and corporate-action data "goes to K-009 and K-010" | what happens to a signal stays D-009's (OQ9-21); no listing-age threshold |
| P10-06 | Q11, reconstructability (RI-14) | "If S&R is used at a signal or decision (OQ9-13), keep the S&R references in use at that time as they were then, so that a later review can see what V2 saw" | the minimum record, retention and storage stay with D-009 OQ9-22 and K-008, K-011 |
| P10-07 | Q13, what to leave to later topics (RI-16) | keep the recorded boundaries: "every use, measure and data source stays with its own topic" | not an ownership decision |

- Q12, line 144 ("what price did around those areas"): the record makes "no proposal. It asks for a reading of the example's chart, which needs verified candle data first (UK-04)."
- Later records touch P10-03 without closing it: BK-80's notes of D-011 (RQ11-02, RQ11-04, DR-079) and D-012 (RQ12-03, DR-081), each read as INTERPRETATION (Claude) that the later requirement "bears on, and does not close" the point (section 9.3).

### 7.3 Rama's other inputs (RAMA (STATED); PROVISIONAL, the register's "RAW / PROVISIONAL USER INPUT"; the requirements record, section 2.2)

| Area | Rama's words (register row; sheet line) | Status, as recorded | Open item or dependency |
|---|---|---|---|
| Which price action counts | RI-06 (61): "I Guess all of the above"; "add your opinion" | RAW; "all of the above" refers to ChatGPT's list, which held "other" and "not sure" (AD-10); P10-01 | OQ10-04; D-006 (OD-02); UK-03, UK-12 |
| Timeframes | RI-08 (77): the 1Hr + 1D + 1W and 1D + 1W + 1M sets; "pick which is nearest S&R point then lock the timeframe" | RAW IDEA; no set, hierarchy, selection or lock decided | OQ10-05; D-007 BLOCKED / NOT CLOSED (OD-01) |
| Overlapping or nearby references | RI-09 (82): "Sorry not understood well, So time being add your best opinion and mark with needs to be discussed later in new system design" | no preference; P10-02 | OQ10-06; OQ9-14 |
| What each reference shows | RI-10 (99): "Time being add your best opinion and mark with needs to be discussed later in new system design" | no preference; P10-03 | OQ10-07; D-011, D-012 |
| The view relative to the current price | RI-11 (111): the same words as RI-10 | no preference; P10-04 | OQ10-09; OQ9-14 |
| Incomplete history | RI-13 (130): "So we need a control & monitoring mechanism", with NSE's security lists as a "Master File/register" | RAW; P10-05; his JPL date differs from IN-03's (UK-05) | OQ10-11; OQ9-21; K-009, K-010 (OD-06; BK-79) |
| Reconstructability | RI-14 (135, 136): "So pulling candles is one time activity before making each trades of that day" | RAW; "it describes a workflow and does not answer the question"; P10-06 | OQ10-12; OQ9-22; K-008, K-011; UK-06, UK-07 |
| The worked example | RI-02, RI-03 (34, 36) and RI-15 (140 to 147): BANKBARODA on a BSE daily chart; the daily instrument file; 3-year and 5-year lookbacks | "illustration only"; figures and dates "as written and unreconciled" (register, section 6; AD-01 to AD-05) | UK-04, UK-08 to UK-11, UK-13; D-007; K-009 (BK-79) |
| Boundaries | RI-16 (162): "add your best opinion, we will discuss later in new system design" | the recorded boundaries stand; P10-07 | OQ10-01, OQ10-02 |
| Anything else | RI-17 (165) left blank; Q12's last item (147) also blank | - | - |

- The example's figures stay as written, not normalised: for the resistance "Rs 277.95" and "Rs 277.57" against the chart label 277.94; for the support "Rs 256.86" and "256.56" against 256.81; the chart is a BSE chart, while V2's stated scope is NSE equities (register, section 6; AD-05; UK-09). ChatGPT's correction of its own reading stands: 242.69 is "a horizontal reference line across the chart" (its supplemental review, section 2).

### 7.4 Unknowns and dependencies

- UK-01 to UK-13 (the open-items file, section 3.4). ChatGPT's Step 2 (section 2.3) asked Rama about UK-01 and UK-02 only and kept UK-03 to UK-13 under "KEEP OPEN / DO NOT ASK RAMA NOW". The requirements record (9.2) reads, as INTERPRETATION (Claude): "UK-01 is addressed by RQ10-01 and UK-02 by RQ10-05 (Rama's decisions); UK-03 to UK-13 are unchanged".
- The open unknowns, in short: UK-03, what counts as touching or breaking a reference; UK-04, reconciling the example with candle data; UK-05, the CSV's source, version and date and the JPL date; UK-06, the unit of "one time activity"; UK-07, a point-in-time snapshot; UK-08, whether the 3-year and 5-year lookbacks are minima; UK-09, whether a BSE chart stands for NSE prices; UK-10, which chart line is the SL; UK-11, the red rectangle; UK-12, what "all of the above" covers; UK-13, the label "20-Apr-206".
- U10-01 to U10-13 (the evidence sheet, section 10), as the requirements record (9.2) reads them, INTERPRETATION (Claude): "U10-01 and U10-02 are addressed for D-010 by RQ10-01 (U10-01 restates OQ9-25, which stays OPEN; section 9.3)"; "U10-07: the references DECIDED (RQ10-02), their use OPEN (OQ9-20)"; "U10-08: RQ10-04 treats support and resistance alike, as positions relative to CMP; the trade side stays OQ9-18"; "U10-03 to U10-06 and U10-09 to U10-12: unchanged"; "U10-13: a Stage 1 snapshot (Step 2, section 2.2)".
- OD-01 to OD-09 (the open-items file, section 3.5; ChatGPT's Step 2, section 2.4): timeframes with D-007 (OD-01); formation-based evidence with D-006 (OD-02); trade use with D-009 (OD-03); strength and freshness with D-011 and D-012 (OD-04); entry, score, Trade / Reject, SL and R:R with D-013 to D-018 and the E topics (OD-05); market and candle data with K-009 and K-010 and OQ9-24 (OD-06); legacy disposition with J-003 and J-013 (OD-07); Rama's requests for opinions (OD-08; the requirements record, section 3A, gives Claude's labelled proposals for them, which decide nothing); and the process question settled by SC-01 (OD-09).

### 7.5 D-009's questions that stay OPEN (D-009's record is not edited)

- ChatGPT's Step 2, section 3, listed the D-009 questions "specifically linked to D-010 inputs" and said "Do not close or answer them from the D-010 answers": OQ9-13 (whether and how S&R is a trade-decision input), OQ9-14 (what "nearest S&R" means), OQ9-15 to OQ9-17 (SL and target), OQ9-18 (short-side symmetry), OQ9-20 (common against strategy-specific use), OQ9-21 (missing or suspect data and signal handling), OQ9-22 (the minimum record per signal), OQ9-24 (weekly and monthly data) and OQ9-25 (points, zones or both).
- OQ9-25's who-decides cell names "D-010 (zones)". The requirements record (9.3) reads, as INTERPRETATION (Claude), that "RQ10-01 bears on it"; it "stays OPEN in D-009's record", because ChatGPT's Step 2 says not to answer it from the D-010 answers, TS_D010_Authorise.txt keeps OQ9-01 to OQ9-25 OPEN, and Rama's answers did not address it. The independent review's F-08 corrected an earlier draft that had said it was answered for D-010.

## 8. Legacy evidence

- Route B (DR-074, W-11) and option C (DR-073) applied: no legacy source, repository, clone, host, VM, runtime, broker or database was accessed for D-010. Frozen-manual content reached D-010 only through the accepted Stage 1 evidence sheet, and "its frozen-manual citations were not re-read" (the requirements record, section 4). ChatGPT's Step 2 set OQ10-16 as "NO FURTHER D-010 LEGACY RESEARCH." This manual adds no read of a legacy source; how its drafting touched docs/inputs/legacy_baseline/ is a case of PD-01 (the batch decision record, section 5): the D-010 drafting subagent's git grep read the text of that folder's files, and that nothing from it was displayed or used is Claude's report, not independently verified.
- What the evidence sheet records (by the Master Agenda's Part 3 items, as the requirements record, section 4, sets them out):

| Part 3 item | What the records hold, with its label and qualifiers | Evidence |
|---|---|---|
| 1. Current system | FACT (HISTORICAL), recorded owners only: D-004_manual.md 1.3, line 50 ("levels and zones" under "D-009 to D-012 S&R", in a section headed "recorded owners kept; nothing closed, reassigned or reinterpreted"); D-005_manual.md 1, line 15 ("S&R, zones, strength and freshness (D-009 to D-012);", its list lead-in not read). No line read names D-010 alone. | E10-19, E10-21 |
| 2. Code and modules | FACT (HISTORICAL), names only: D-005_manual.md 7, line 158 ("classified S&R; not documented"); the zone warmer and `_nearest_zone_json`, as FACT (V2 RECORD) of HISTORICAL text through D-009's accepted rows | E10-22, E10-26, E10-24 |
| 3. Inputs | the zone warmer is among a candle fetch's readers (D-004_manual.md 7, line 230, through E9-17); what it computed NOT RESEARCHED | E10-26; NR9-01 |
| 4. Process | what any legacy zone mechanism computed, and how a zone was chosen: NOT RESEARCHED (NR9-01); the PB-01 helper's body UNKNOWN (G-03) | - |
| 5. Output | the PB-01 row's `sr_zone_json`, with the code comment "Record the nearest 30m/1h S&R zone to LEVEL (validation programme; fail-safe)." and the schema comment "nearest 30m/1h sr_detector zone to LEVEL (S&R validation programme)." (C-011_manual.md 7.6, 10.2, through E9-21) | E10-24; NR9-05 |
| 6. State change | whether rows were written, and what the entry stage did with them: UNKNOWN / NOT ESTABLISHED | NR9-05 |
| 7. Dependencies | the E3 retest path's "zone band"; with the repository configuration "the zone warmer, the retest monitor and the structure-exit manager are not started" and "E3 has no caller"; deployed configuration not established (C-008_manual.md 9.2, through E9-30; B-006 TH11) | E10-25 |
| 8. Problems and limitations | user-reported: "Incorrect or unsuitable S&R range or interpretation." (A-001_manual.md 9, line 63, through NR9-11); its meaning UNKNOWN / NOT ESTABLISHED; Rama: "No, I do not recall" | E10-17, E10-36 |

- The qualifiers that travel with these rows (evidence sheet, conventions): "A file name, heading, code identifier, import, docstring, code comment, schema comment or configuration default inside a quote proves no runtime activation, behaviour or V2 requirement." And the requirements record, section 4: "A repository value is not a deployed value (W-7), and code, comments and configuration prove no runtime behaviour."
- NOT RESEARCHED and UNKNOWN gaps (V2_DESIGN_LOG section 8; each "an evidence gap, not a finding"; each OPEN; raised at D-009, 30-Oct):
  - row 8, NR9-01 (line 724): "what the S&R detector computed (levels or zones, method, use of timeframes and of its 11 defaulted fields)"; NOT RESEARCHED;
  - row 12, NR9-05 (line 728): "the PB-01 zone record: how _nearest_zone_json chose a zone and handled failure, the capture's candle source, whether rows were written, and what the entry stage did with them"; UNKNOWN / NOT ESTABLISHED; routing cell "as row 8; zone semantics are D-010's";
  - row 18, NR9-11 (line 734): what the user-reported "Incorrect or unsuitable S&R range or interpretation." referred to; UNKNOWN / NOT ESTABLISHED.
- No new gap row (OQ10-16; E10-37): the zone behaviours met fall under NR9-01, NR9-05 and NR9-11. The zone lines Stage 0 located in C-006, C-007, C-011 (row H10 and line 947), A-011 and B-011 were Tier 3, not authorised, and stay NOT RESEARCHED (E10-37; read log, section 5.2).
- Disposition: "KEEP and MODIFY are not available for any legacy zone mechanism (W-11): the frozen manuals establish names, comments and repository configuration, not what ran." DR-076 already records "no KEEP or MODIFY" for the legacy S&R mechanism, with its disposition "with J-001 to J-009" (E10-27). For the zone mechanisms the outcome at D-010 is none: "Their disposition is J-003's and J-013's (DEFER, in Part 7's terms)." SHADOW is not used (the record's INTERPRETATION, "as at D-008": "it presumes an existing mechanism kept observational, and what the legacy mechanisms did is not established") (the requirements record, section 7).

## 9. Cross-topic boundaries, dependencies and bookmarks

### 9.1 Chronology of the S&R topics (FACT (V2 RECORD); commit times from V2_DESIGN_LOG section 6)

| Topic | Recorded status | Commit (time, 30-Oct) | Relation to D-010 |
|---|---|---|---|
| D-009 Support & Resistance | DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076); OQ9-01 to OQ9-25 OPEN | 9c9c2e9 (10:06) | earlier: D-010's base; it routed the point-or-zone question to D-010 (OQ9-25) and keeps OQ9-13 and the other questions of section 7.5 |
| D-010 S&R zones | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077) | 7c725ed (18:37) | this topic |
| D-011 S&R strength | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079) | d665719 (19:42) | later: "D-011 describes how strongly history supports an S&R reference, for the references D-010 defines (RQ10-01 to RQ10-05, DR-077)." (D-011_v2_requirements.md); its package stored D-010's post-push record and push line |
| D-012 S&R freshness | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | 3674847 (20:32) | later: "D-012 describes freshness for the references D-010 defines (RQ10-01 to RQ10-05)." (D-012_v2_requirements.md) |
| Topic workflow | DR-082 (W-14) and DR-083, in effect from 22:14 IST | 6bd80a5 (the base of this batch) | later: supersedes parts of DR-078 (W-12); requires this manual and review notes (BK-85) |
| D-013 Entry zone | NOT STARTED (V2_DESIGN_LOG section 4, line 375) | - | "Entry zone" is not D-010's "S&R zone" (OQ10-20; BK-81) |

- D-011's and D-012's requirements are their own records' decisions. They are not D-010 decisions, and D-010's records were not edited by them. D-009's draft is not changed by D-010 (the requirements record, sections 9.3 and 11).

### 9.2 Boundaries (the requirements record, section 10; status columns as recorded on 30-Oct and at 6bd80a5)

| Item | Topic | Status in D-010's record | Status at 6bd80a5, where it changed | Relation to D-010 |
|---|---|---|---|---|
| Whether and how S&R informs the trade decision; "nearest"; SL / target fit; short-side symmetry; strategy use; signal handling with missing data; the record per signal | D-009 | DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076) | unchanged | OQ9-13, OQ9-14, OQ9-15 to OQ9-18, OQ9-20 to OQ9-22; RQ10-01 to RQ10-05 grant no decision authority |
| Strength or confidence of a reference | D-011 | not started (authorised next, W-12) | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079) | P10-03; BK-80 |
| Age, freshness or decay of a reference | D-012 | not started (authorised after D-011, W-12) | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | P10-03; BK-80 |
| Entry zone, breakout / breakdown, retest / reclaim, entry timing | D-013 to D-016 | not started | D-013 NOT STARTED, waiting for ChatGPT's start package (W-14) | RQ10-05's next reference and the naming note (BK-81) |
| Score / ranking; the final Trade / Reject | D-017; D-018 | not started | - | no D-010 effect (Rama's bound) |
| SL, targets and R:R | E-005 to E-008 | not started | - | whether the next reference serves as a target is not decided here (RC-02: "not a guaranteed destination or prediction") |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | unchanged | OQ10-05; no hierarchy |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | unchanged | OQ10-04 |
| Market and candle data; the instrument file; listing and corporate-action data | K-009, K-010 | not started | - | OQ10-11, OQ10-13; BK-77, BK-79 |
| Audit state and retention | K-008, K-011 | not started | - | OQ10-12; P10-06 |
| The legacy zone mechanisms | J-003, J-013 | not started (Route B topics) | - | disposition is theirs; D-010 keeps, modifies or revives nothing |
| S&R architecture | O-006 | Section O after A to N | - | no D-010 effect |
| Testing | Section N | not started | - | none proposed here |

- Other rows the requirements record (section 6) lists as touching D-010, all unchanged by it: DR-031, an owner allocation only ("S&R, R:R and the admission test [...] stay with C-001, D-003, D-009 to D-017 and J"); DR-069 ("within the bounded evidence no S&R, regime or V3-chain input reaches the screener (INTERPRETATION, MC-76)"); VP-08, "not ratified at D-010", with a tracking note (OQ10-14); VP-44, whose ratification points do not include D-010; Q-35 (e) and Q-44 (a), of which the record says: "Neither bears on V2 representation."
- VP-08's D-010 note (V2_DESIGN_LOG section 3, line 259), INTERPRETATION (Claude): D-010's decided requirements "neither ratify nor reject the sentence "S&R logic is set after strategies and processors, and R:R honours S&R."; RQ10-02 (each stock's references shared across all strategies) bears on its first half without settling it. The Ratify-at cell is unchanged."

### 9.3 Bookmarks (V2_DESIGN_LOG section 7; tracking only; nothing decided)

- BK-79 (D-010; OPEN): OQ10-11 and OQ10-13, with Rama's raw inputs (NSE's security lists as a master file, RI-13; the broker's instrument file refreshed each day, RI-15; the 3-year and 5-year lookbacks as raw figures) and Claude's P10-05; the supplied CSV's "source and date semantics UNKNOWN" (UK-05). Cross-check: K-009 (Market data), K-010 (Candle data).
- BK-80 (D-010; OPEN): OQ10-07 and P10-03, which attributes are strength-like or age-like and so D-011's or D-012's, and which are D-010's. D-011's note (RQ11-02, RQ11-04) reads, as INTERPRETATION (Claude), that this "bears on, and does not close, the point", and the bookmark "stays OPEN for D-012"; D-012's note (RQ12-03) reads that it "bears on, and does not close, which attributes D-010's references themselves carry, P10-03", and the bookmark "stays OPEN for the whole-design review".
- BK-81 (D-010; OPEN): OQ10-20 and RQ10-05: "whether it serves as an entry or a target is not decided (RC-02: "not a guaranteed destination or prediction"; Rama's examples set the TGT at the next reference)". Cross-check: D-013 to D-016; E-007 (Target determination).
- D-009's BK-76 (E-006, E-008), BK-77 (K-009) and BK-78 (D-013 to D-016) were read in Stage 2 (R2-05); each is OPEN and D-009's.
- BK-85 (W-14; OPEN): whether and how D-008 to D-012 receive a V2 topic manual and review notes "is not decided"; ChatGPT plans "a second, deeper audit" of D-010 to D-012, and whether it also covers D-008 and D-009 "is for Rama and ChatGPT to decide". This documentation batch concerns it (section 1).

### 9.4 D-006 and D-007

- D-007 (BLOCKED / NOT CLOSED, DR-073): "no decided requirement needs a timeframe decision; OQ10-05 stays OPEN / UNRESOLVED, and Rama's timeframe ideas stay raw ideas with no hierarchy." D-006 (DEFERRED / NOT CLOSED, DR-072): "RQ10-03 names no kind of price action; OQ10-04 stays OPEN with the D-006 dependency." The record's reading that "DR-074's stop rule is not triggered" is labelled INTERPRETATION (the requirements record, section 7).
- What waits on them, as recorded: OQ10-05, U10-05, UK-08, RI-08's timeframe sets and RI-15's lookbacks on D-007 (OD-01); OQ10-04, UK-12, and the formation-based kinds of P10-01 ("rejection candles among them") on D-006 (OD-02; IP-06).

## 10. Known defects, process deviations and lessons, as recorded

- KNOWN DEFECT: the reversed restatement in Rama's first Q3 answer, corrected before recording (section 6.1).
- Stage 0 deviations, disclosed by Claude in output 1, section 7, and treated by ChatGPT (Stage 0 review, section 3); each stays disclosed, "not erased, reclassified or treated as precedent" (package manifest, section C):
  - DV-01, scripts and shell reads: the term search and the hash check of the 39 manuals ran as read-only Python scripts, and shell read commands printed folders and sections, against the prompt's guard; ChatGPT: "ACCEPTED AS A DISCLOSED DEVIATION; NOT A PRECEDENT.";
  - DV-02, reads beyond locators: the accepted D-009 row E9-39, the D-009 read log's header with its S1- row cells and two short fragments, and the Route B frozen-manual source inventory; ChatGPT: "ACCEPTED AS A DISCLOSED DEVIATION; NOT A PRECEDENT.", and "Do not rely on the extra passages as Stage 0 findings.";
  - DV-03, at about 11:03, one count-only search for the phrase "OPEN / UNRESOLVED" over the clone's whole docs/ folder, whose scope covered docs/inputs/legacy_baseline/ if that folder exists; no line was printed and no such path appeared; ChatGPT: "MATERIAL SCOPE DEVIATION, DISCLOSED.", with "No inference is made that prohibited content was read."
- Later steps: the read log reports "None found" for Stage 1, and the change-control note "Deviations: none found." for the supplemental step. ChatGPT: the latter "does not erase or supersede the three previously disclosed Stage 0 deviations (DV-01 to DV-03)" (supplemental review, section 1 C).
- A later record names DV-01: DR-079 lists among D-011's own disclosed departures "shell prints of single lines, the method of D-010's DV-01". That is D-011's record; DV-01's treatment in D-010's records is unchanged.
- Corrections made during the topic, as recorded: ChatGPT's own reading of the chart label 242.69 ("Correction accepted.", supplemental review, section 2); RC-03b's heading, which became "ChatGPT's expanded wording of Rama's clarification (not verbatim Rama wording):" (D010Review.txt); the package manifest's A.1 heading, from "Present in the working folder (all of them local and uncommitted)" to "Present in the working folder (repository tracking/commit status not checked)" (D010_Correction_Only_Instructions.txt).
- The independent review's eighteen findings, three MAJOR, twelve MINOR and three NOTE, were all resolved before the push; "Not changed: the decided wording of RQ10-01 to RQ10-05, which the reviewer found exact." (D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt, section 3). D-010_review_notes.md, section 2, gives them.
- What the reviewer could not verify: "anything resting on unstored screenshots or on the question tool's raw output"; byte-identity of the eleven earlier records and ChatGPT's files with what was reviewed or received; "times found only in the requirements record's section 12 and the design-log session note" (the review record, section 2).
- Lessons (V2_DESIGN_LOG section 1, all dated 2026-10-30, each "Historical process lesson (not a system requirement)"; the recording commit is from the repository history):
  - line 182 (7c725ed): DV-03; rule: "A search's scope is set to the permitted paths before it runs, including a count-only search made during a final check; a wider scope is a deviation even when only counts are printed.";
  - line 183 (7c725ed): the manifest heading; rule: "A record states only the states it checked; an unchecked state is written as "not checked".";
  - line 184 (7c725ed): the reversed Q3 restatement; rule: a restatement "is compared with the option and with the speaker's earlier words before it is recorded; a conflict is asked about once, never resolved silently.";
  - line 185 (7c725ed): "Dude, don't stop! [...] Move on to next agenda, don't just chat"; recorded as W-12 (DR-078);
  - line 186 (d665719, the D-011 package): the Q3 bound first applied to all five requirements and OQ9-25 first recorded as answered (F-04, F-08); rule: "A quoted limit or decision covers only what its source answer covers; a mapping from a decision to another topic's open item is labelled INTERPRETATION and changes no status.";
  - line 187 (d665719): "not a target" and "not an entry" (F-09); rule: "A statement about what a later topic will decide uses the source's own words, or says "not decided here".";
  - line 188 (d665719): ChatGPT's drafted wording first presented as "Rama's answers", and a quoted message given the wrong addressee (F-03, F-07); rule: each decision's wording "carries its drafter (Rama, ChatGPT at his request, or not established) wherever it is restated";
  - line 192 (6bd80a5, the topic-workflow package): D-010's question "Next topic" offered "Yes, continue in order (Recommended)"; the answer chose it, and Claude ran D-011 and D-012 and began D-013's searches, while Rama "had meant one topic at a time"; rule: "A question that would widen Claude's own authority or autonomy carries no "(Recommended)" mark and states plainly what each option would let Claude do without asking.";
  - line 194 (6bd80a5): D-008 to D-012 stored without the manual and review notes that A-001 to D-005 hold (DR-083); rule: the folder "is compared with the established folder structure" before drafting and before publication.
- DR-077 counts the "Four historical process lessons" of lines 182 to 185; DR-079's five lessons, lines 186 to 190, include the three of lines 186 to 188 that come from D-010's independent review.

## 11. Status

- D-010: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077): RQ10-01 to RQ10-05 DECIDED at requirement level, with the Q3 bound and the OQ10-19 boundary; everything else OPEN, PROPOSED (P10-01 to P10-07) or a dependency; not frozen. OQ10-18 ADMINISTRATIVELY RESOLVED / ANSWERED only.
- Unchanged: D-009 DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076), with OQ9-01 to OQ9-25 OPEN; D-006 DEFERRED / NOT CLOSED (DR-072); D-007 BLOCKED / NOT CLOSED (DR-073); VP-08 PROPOSED except its count, not ratified at D-010; Route B (DR-074, W-11) and option C (DR-073) in force; section 8 rows 8, 12 and 18 OPEN; BK-79 to BK-81 OPEN; BK-85 OPEN. D-013 NOT STARTED.
- Companion files in docs/agenda/d-010_sr-zones/: D-010_v2_requirements.md (authoritative), the fourteen stage, review and storage records of section 2.1, and, from the documentation batch (31-Oct-2026, DR-084), D-010_review_notes.md and D-010_manual_manifest.md. The manifest lists this manual's sources with their SHA-256 at the base and the checks run on it.
- Nothing here changes a decision or status. This manual adds no requirement, no design, no legacy read and no erratum to any record; how its drafting touched docs/inputs/legacy_baseline/ is PD-01 (section 8; the batch decision record, section 5).
