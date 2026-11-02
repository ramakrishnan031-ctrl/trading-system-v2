# D-011 - S&R STRENGTH (V2 TOPIC MANUAL)

## 1. Document status, purpose and scope

- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079). In the requirements record's own words (D-011_v2_requirements.md, section 13): "D-011: V2 REQUIREMENTS RECORDED (DR-079): RQ11-01 to RQ11-04 DECIDED at requirement level; not frozen." Its header adds: "RQ11-01 to RQ11-04 are DECIDED at requirement level by Rama (section 2.1). Everything else is OPEN or a dependency. NOT FROZEN. Requirement level only: no design or implementation decision." DR-079's status cell reads "DECIDED"; its text gives "OQ11-01 to OQ11-10 OPEN (OQ11-08, VP-08 at D-011, answered: not ratified)". D-011 was the fourth Route B topic (DR-074, W-11), started under W-12 (DR-078) after D-010's publication, and the first topic under Route B's amended procedure (DR-080, W-13), which was set at D-011's own follow-up.
- This is a V2 topic manual (DR-083), written on 01-Nov-2026 as documentation-only work (Rama's choice of 01-Nov-2026: "Docs only, like D-012"). It explains the topic and its records. D-011_v2_requirements.md is authoritative for requirements and decisions; this manual decides nothing, changes no status and adds no requirement.
- The instruction this work was done on: Rama (RAMA (STATED), 20:22 IST on 01-Nov-2026, relayed with ChatGPT's continuation instruction; transcribed in D011_DOCUMENTATION_RAMA_MESSAGES_TRANSCRIPTION_01-NOV-2026.txt, not in the repository at a4247239): "Same like D-012, Inform Claude to comple pending D-011 too, once completed then we decide either retain existing D-011 and D-012 or prepare freshly again in new chat window". Asked by Claude (shortly after 20:23 IST) what "complete pending D-011" means, he chose in Claude's question tool "Docs only, like D-012 (Recommended)" -- "Manual, review notes, manifest as new files; no requirement or status change; reviewed, local, no commit" (the option wording and its "(Recommended)" mark are Claude's). Neither the message nor the choice is in the repository at a4247239; they are given here as Claude relayed them for this drafting. ChatGPT's continuation instruction (ChatGPT's wording, at Rama's request: his words to ChatGPT in the screenshot of his ChatGPT chat that reached Claude at 20:22 IST, as Claude transcribed them, include "Inform Claude to comple pending D-011 too" and "Now inform Claude to continue to work on & guide to accomplish it"; not in the repository at a4247239) says: "Rama's current instruction authorises Claude to continue the pending D-011 work, subject to the existing governance and dependency gates." (its section C) and "No commit or push is authorised by this instruction." (its E.1). INTERPRETATION (Claude): these later instructions authorise this documentation, after R-03's "Do NOT prepare D-011 questions, answer sheets, manual, review notes or manifest." (section 9.5); no V2 record at a4247239 says so. ChatGPT's later review of this package (D011_D012_ChatGPT_Review.txt, dated 01-Nov-2026, its section 4, R2; not in the repository at a4247239; included here on ChatGPT's instruction of 02-Nov-2026) rules: "This later, specific instruction authorises the documentation-only package; it does not authorise new D-011 topic work, new questions/answer sheets, research, requirement changes, implementation or publication." ChatGPT's instruction of 02-Nov-2026 calls this a bounded clarification of the later instruction, not a new governance decision; R-03 is not rewritten here and is not stated as generally superseded.
- ChatGPT's continuation instruction of 01-Nov-2026 (Claude_D011_D012_Continuation_Instructions.txt; not in the repository at a4247239) treats D-011 as pending topic work in its section C ("continue the pending D-011 work"), with the answer sheet, bounded research, reconciliation and draft-record tasks its C.7 describes, and says that "D-010 has NO REQUIREMENT DECIDED"; Claude's cross-check of 20:23 IST (D011_D012_CONTINUATION_DISCREPANCY_NOTE_01-NOV-2026.txt; not in the repository at a4247239) found both against the records (its rows 2 and 3): D-011's requirements were already recorded, and what was pending for it was its documentation. Rama then narrowed the continuation to "Docs only, like D-012"; accordingly the work later authorised and done was documentation only, and no new D-011 topic research, answer-sheet work or requirement work was authorised (ChatGPT's R2, quoted above). This manual takes every status from the V2 records at a4247239: D-011 is V2 REQUIREMENTS RECORDED (DR-079), and D-010 is V2 REQUIREMENTS RECORDED (DR-077), with RQ10-01 to RQ10-05 DECIDED; it is D-009 that is "DRAFT RECORDED - NO REQUIREMENT DECIDED" (DR-076).
- Baseline: the V2 repository at a4247239 (GitHub main; commit time 19:00 IST on 01-Nov-2026, from the repository history). The four D-011 records in docs/agenda/d-011_sr-strength/ were stored by commit d665719 on 30-Oct-2026 and have not changed since (the repository history of that folder shows d665719 only). Times are IST and, unless another day is named, on 30-Oct-2026. Paths are relative to the repository root. Line numbers are those at a4247239. The drafting used only the sources listed in D-011_manual_manifest.md; every repository command named only files or folders outside docs/inputs/legacy_baseline/ and docs/legacy_system_audit/, or excluded both by git pathspec, except commands that named no path and printed commit metadata only (git rev-parse, git log -1, git show -s) and one git show --stat of commit a4247239 with no path, whose output was cut to its first five lines (the part printed: the commit's subject and three paths, none in either folder); git status was not run (the drafting subagent's report; not independently verified).
- Purpose: DR-083 describes the manual as a "readable, detailed topic chapter explaining the subject, evidence, requirements, boundaries, unresolved matters and cross-references." and adds: "It must not invent requirements or duplicate the requirements file without adding explanatory value." (ChatGPT's wording, DR-083). This one does that for D-011: what the topic was asked, what Rama decided at requirement level and in whose wording, what stays open and where it waits, the legacy evidence and its limits, how D-011 stands against D-009, D-010, D-012 and the later topics, and where each record sits.
- In scope, as recorded (the requirements record, section 1, "Scope and boundary"): "D-011 describes how strongly history supports an S&R reference, for the references D-010 defines (RQ10-01 to RQ10-05, DR-077)." The question it answered (the stage record, section 1): "what V2 requires of an S&R reference's strength, at requirement level, given D-010's references (RQ10-01 to RQ10-05, DR-077)."
- The bound of the answers (the requirements record, header; "Rama's 18:47 answers, ChatGPT's wording"): "Important: These answers establish requirement-level direction only. They do not approve formulas, thresholds, scoring, timeframe hierarchy or trading authority."
- Out of scope, as recorded (the requirements record, sections 1 and 11; the stage record, section 1):
  - the use of strength: "It decides no use of strength: "The later topics--including trade decisions, entry eligibility, SL and targets--must independently decide how strength is used within their respective scopes." (Rama's follow-up, ChatGPT's wording; the stage record, section 5.3; OQ11-01)."
  - freshness: "Freshness is D-012's, from the starting definition in RQ11-04." (the stage record: "freshness (D-012), apart from its starting definition (section 5.3)");
  - the legacy mechanisms: "The legacy mechanisms' disposition is J-004's (OQ11-02).";
  - any design: "No design: no formula, weighting, threshold, count or label rule, timeframe or hierarchy, lookback, schema, storage, gate, score or trade authority.";
  - and, from section 11: "No design or implementation decision, no owner moved, no VP ratified, no frozen manual or earlier decision changed." and "No legacy-source access (DR-073). D-012 is not started by this record."
- This documentation and BK-85: DR-083 requires a V2 topic manual and review notes beside each Route B requirements record. BK-85 (V2_DESIGN_LOG section 7, line 722, OPEN at a4247239) records that D-008 to D-012 were stored before DR-083 without them and that "none is marked complete under DR-083 while this bookmark is OPEN (point D)"; its note of 2026-10-31 (DR-084) says "D-011 and D-012 are not in the batch"; its note of 2026-11-01 records ChatGPT's R-03, "For OP-01, the immediate missing-manual re-audit scope is D-011 and D-012.", and ends: "D-012's manual, review notes and manifest added as documentation only (the D-012 row, section 4); D-011 still lacks them. Stays OPEN." D-011 is therefore the one topic of D-008 to D-012 still without these files at a4247239. The same note records R-03's "Do NOT prepare D-011 questions, answer sheets, manual, review notes or manifest." (ChatGPT's review of 01-Nov, 14:24 IST); this manual was prepared on Rama's later instruction of 20:22 IST and changes neither R-03 nor BK-85's note. DR-083: "D-010 to D-012 hold independent review records, which, like the requirements files, do not by themselves satisfy this decision (point A)". This manual does not close BK-85, mark D-011 complete under DR-083 or change D-011's status.

## 2. Records and how to read them

### 2.1 The topic folder (docs/agenda/d-011_sr-strength/)

The four files stored by d665719 (storage manifest, rows 1 to 3, and the manifest itself). At a4247239 the folder holds these four only. This manual and D-011_review_notes.md were drafted on 01-Nov-2026 to sit beside them, with a manual manifest written by Claude.

| # | File | What it is | Status or revision, as recorded |
|---|---|---|---|
| 1 | D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | "the stage record" (the requirements record's name for it): the plan (section 1), the process departures DV11-01 to DV11-04 and the Route B amendment (1A), locators L11-01 to L11-05 and the range forms (2, 2.1), reads R11-01 to R11-05 (3), evidence E11-01 to E11-13 (4), Claude's questions, Rama's answers and the follow-up (5.1 to 5.3), OQ11-01 to OQ11-10 with the bookmarks and the legacy gap (6), status (7) | banner "*** EVIDENCE AND DECISION RECORD. NO DESIGN. CLAUDE DECIDES NOTHING; ITS "(Recommended)" OPTIONS ARE PROPOSALS. ***"; "Baseline: the V2 repository at 7c725ed (the working copy equal to GitHub main after the D-010 push)."; revised after the independent review, before the push (the review record, section 2) |
| 2 | D-011_v2_requirements.md | the requirements record (DR-079's output) | authoritative; "V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079)"; "NOT FROZEN" |
| 3 | D-011_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | the W-12 review: reviewer, scope, findings F-01 to F-18 and their resolutions, verdict | "Recorded 30-Oct-2026 (IST) by Claude, the package's author."; verdict "PASS WITH FINDINGS. There is no BLOCKER." |
| 4 | D-011_STORAGE_MANIFEST_30-OCT-2026.txt | the 4 rows stored by d665719 with SHA-256 and bytes (rows 1 to 3 D-011's records, row 4 D-010's post-push record), the governance changes, the checks and what was still owed | "This manifest does not list itself"; its SHA-256 is in the post-push record, section 3 |

- FACT (V2 RECORD): D-011 and D-012 "hold 4 files each, against 12 to 15 for D-008 to D-010: under W-13 they had no ChatGPT Stage 0 prompt, answer sheet or discussion files. So the packages did get thinner, by recorded decisions, not by forgetting." (TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt, R-04). The design-log lesson of line 194 records the same difference (section 10).
- FACT (V2 RECORD): Claude's reconciliation report of 01-Nov-2026 (docs/agenda/pre-d011-settlement/D011_PREREQUISITE_RECONCILIATION_REPORT_01-NOV-2026.txt, X-01): "D-011, D-012 lack manual.md, review_notes.md, manual_manifest.md".
- The stage record's status section (7) and the requirements record's section 13 give the status at storage; nothing in the folder has been edited since.

### 2.2 Inputs in docs/inputs/

- docs/inputs/chatgpt_originals_and_replies/D-011_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt: Claude's post-push record of d665719 ("Written 30-Oct-2026 (IST) by Claude, after the push."), stored by the next package, 3674847 (D-012's; its storage manifest, row 4; DR-081: "This package also carries the records owed by the D-011 package"). It is the one D-011 input that git ls-files finds at a4247239 by the names *D-011* under docs/inputs/reconciliation/, docs/inputs/chatgpt_originals_and_replies/ and docs/inputs/manuals/ and *D011* under docs/inputs/reconciliation/. The other three matches (ChatGPT_Review_Before_D011.txt, D011_PrePush_Audit_HOLD.txt and Settle_Before_D011.txt, all in docs/inputs/reconciliation/) are later records of 01-Nov-2026 stored by a4247239, not D-011 inputs (section 9.5).
- Two of its lines were true when written and later fulfilled or overtaken: its banner, "OWED TO THE NEXT PACKAGE (D-012)", met by 3674847; and section 4, "D-012: next (W-12), from the starting definition decided in RQ11-04.", which D-012 then did (DR-081).
- Stored by D-011's package but D-010's record: docs/inputs/chatgpt_originals_and_replies/D-010_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt, "D-010's post-push record, owed by the D-010 package" (storage manifest, row 4).
- Not stored (storage manifest, section 1): "No ChatGPT or Rama file is stored: Rama's answers came as screenshots (18:47 and 19:35 IST), not stored (DR-008); their text is transcribed in row 1, sections 5.2 and 5.3."
- No ChatGPT prompt, answer sheet or review for D-011 exists to store: the stage record discloses "DV11-01 | no ChatGPT Stage 0 prompt and no answer sheet" and "DV11-02 | no discussion of the options between ChatGPT and Claude" (section 1A), and the procedure Rama then approved begins "1. Claude prepares the topic-specific questions and answer sheet." (DR-080). INTERPRETATION (Claude, the reconciliation report, F-08): "So a separate answer-sheet file is not expected of them".
- A D-010 input that D-011 read, not a D-011 file: docs/inputs/reconciliation/D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026.txt, Q7, lines 86 to 99 (R11-05). It was not re-read for this manual.

### 2.3 Register rows, design-log entries and the post-push record

- V2_DECISION_REGISTER.md:
  - DR-079 (line 90): D-011's requirements; "DECIDED". Unchanged since d665719 (the repository history of the line).
  - DR-080 (line 91): W-13, set by Rama's answer at D-011's follow-up; status "DECIDED (SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026; it governed D-011 and D-012)" (the status cell last changed by 6bd80a5).
  - DR-078 (line 89): W-12, under which D-011 started; status "DECIDED (its automatic move to the next topic, the sequence beyond D-012 and the ChatGPT audit only after publication SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026)".
  - Later rows naming D-011, or a range that covers it: DR-081 (line 92; D-012), "on the starting definition decided at D-011, which governs (RQ11-04, DR-079: [...])", and the D-011 records owed, stored; DR-082 (line 93; W-14), which "supersedes DR-080 (W-13), which governed D-011 and D-012"; DR-083 (line 94; it names D-011 only inside ranges, "D-006 to D-012", "D-008 to D-012" and "D-010 to D-012", not D-011 itself): "D-010 to D-012 hold independent review records, which, like the requirements files, do not by themselves satisfy this decision (point A)"; DR-084 (line 95): "Not in scope: D-011 and D-012 (BK-85 stays OPEN for them and for the re-audit and its scope)". DR-085 to DR-087 do not name D-011 (a check made for this manual).
  - Earlier rows D-011's records cite: DR-008, DR-031, DR-069 and DR-072 to DR-078 (the stage record, section 1; the requirements record, sections 5 and 10).
- V2_DESIGN_LOG.md, at a4247239 (the commit that last changed each line, from the repository history):
  - section 1, the five lessons dated 2026-10-30 that d665719 recorded (lines 186 to 190: lines 189 and 190 are D-011's own; lines 186 to 188 come from D-010's independent review), and the later lessons dated 2026-10-30 that name D-011: line 191 (3674847) and lines 192 to 194 (6bd80a5);
  - section 3, VP-08's D-011 tracking note (line 263; the line last changed by 3674847, which added D-012's note after it);
  - section 4, the D-011 row (line 377; d665719), the D-012 row (line 378) and the D-013 row (line 379);
  - section 5, W-12 (line 395) and W-13 (line 396), each with its "Update 2026-10-30 (DR-082)" (6bd80a5), and W-14 (line 397);
  - section 6, under "### 2026-10-30": the D-011 session note (line 618; d665719) and the D-011 push line (line 619; 3674847); under "### 2026-11-01", the settlement note (line 632; a4247239), which names D-011 (section 9.5);
  - section 7, BK-80 with its D-011 note (line 717; the line last changed by 3674847), BK-82 (line 719; d665719), BK-83 (line 720; closed by 3674847), BK-85 (line 722; the line last changed by a4247239) and BK-86 (line 723);
  - section 8, row 22 (line 752; d665719), and rows 8 and 21 (lines 738 and 751), which D-011's requirements record cites (NR9-01, NR9-14).
- The post-push record (section 2.2) gives the commit, the push and the fresh-clone check. Its push line (section 5) is byte-equal to V2_DESIGN_LOG line 619 and occurs once in the log (a check made for this manual at a4247239).

### 2.4 Which record governs what

| Matter | Governing record | Note |
|---|---|---|
| D-011's requirements and decisions | D-011_v2_requirements.md (section 2.1) | its wording is "quoted exactly from the stage record's transcriptions" |
| D-012's starting definition | RQ11-04 (DR-079) | the 18:47 wording "stays as history"; D-012 started from the 19:35 definition, and "D-011's definition governs" (BK-83's D-012 note; DR-081) |
| Claude's questions, the 18:47 and 19:35 answers, word for word | the stage record, sections 5.1 to 5.3 | the screenshots are not stored (DR-008) |
| The open questions OQ11-01 to OQ11-10, their wording and holders | the stage record, section 6 | the requirements record, section 8: "none is closed here" |
| Evidence, locators and reads | the stage record, sections 2 to 4 | "frozen-manual lines were read only as the stage record lists" (the requirements record, header) |
| The process departures | the stage record, section 1A (DV11-01 to DV11-04) | "DV11-01 to DV11-04 stay disclosed; they are not erased." |
| D-011's status in V2 | DR-079; V2_DESIGN_LOG section 4, line 377 | |
| The procedure it ran under | DR-080 (W-13), set at D-011's follow-up, as DR-082 (W-14) later superseded it | W-13 "governed D-011 and D-012" |
| The pre-push review | D-011_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | |
| What was stored, with hashes | the storage manifest; the post-push record | |
| Legacy behaviour not researched | V2_DESIGN_LOG section 8 ("The one canonical list of legacy behaviours that a Route B topic needed and that no frozen manual establishes") | rows 8, 21 and 22 |
| Cross-topic points | V2_DESIGN_LOG section 7 | BK-80, BK-82, BK-83, BK-85 |
| Whether D-011 is complete under DR-083 | BK-85 (OPEN) | DR-083, point D |

## 3. Evidence labels

| Label | Meaning in this manual |
|---|---|
| FACT (V2 DECISION) | a register row and what it records, cited by ID, with the status in its row |
| FACT (V2 RECORD) | text or a status in a stored V2 record (a topic file, the design log, a file in docs/inputs/), cited by file and section or line |
| FACT (HISTORICAL) | a frozen manual's statement with its qualifiers, cited by manual, section and line as D-011's stage record cites it (E11-07 to E11-11). No frozen manual was re-read for this manual. It is never evidence of what a host ran. |
| RAMA (STATED) | Rama's own words, quoted exactly from the record named |
| RAMA-AUTHORISED, DRAFTED BY CHATGPT | wording that ChatGPT drafted at Rama's request, where a record attributes it so: here the answers relayed at 18:47 and at 19:35, under the requirements record's section 2.1 heading ("DECISION (RAMA); RAMA-AUTHORISED, DRAFTED BY CHATGPT"); for 18:47 the stage record, section 5.2, gives "Rama asked ChatGPT to answer" (his request is shown); for 19:35, section 5.3 gives "Rama's choices, relayed by him; the wording is ChatGPT's." with "INTERPRETATION (Claude): as at 18:47, he asked ChatGPT to answer; his request is not shown." |
| INTERPRETATION | a reading, named with whose it is (Claude's, in the D-011 records; this manual's own, where marked); never a decision |
| PROVISIONAL | the status of a raw input in the record's own words: here "RAW / PROVISIONAL USER INPUT" (the requirements record, section 2.2) |
| UNKNOWN / NOT ESTABLISHED | not shown by the records |
| NOT RESEARCHED | a legacy behaviour that no frozen manual establishes (W-11; V2_DESIGN_LOG section 8) |
| KNOWN DEFECT | a defect the records name, with its correction or its handling as recorded |
| OPEN QUESTION | a question the records keep open, with its ID, status and where it waits |

- The D-011 records also use labels of their own: "DECISION (RAMA)", "PROPOSAL", "OPEN QUESTION / DEPENDENCY" and "RAW / PROVISIONAL USER INPUT". Where this manual gives them, they are quoted as the record's words; they are not labels of this manual. No label is merged with another.
- Quotes are exact. A quote that runs over several source lines joins them with one space (the D-011 records: "in prose a line break inside a quote is added for width only"); "[...]" marks words left out. As in the D-011 records, check marks are written [x] and [ ] and the long dash --; in quotes from ChatGPT's files of 01-Nov-2026 a long dash is written -- and curly quotation marks straight.

## 4. The question the topic answers

- FACT (V2 RECORD): the Master Agenda lists the topic in Part 5 (line 222), Section D, "D. TRADE DECISION ENGINE" (line 289), at line 302: "D-011  S&R strength". The stage record (E11-01): "No other D-011 scope text."
- The Section D goal (lines 311 to 317): "Should this signal become a trade?"; "This section is especially important for the known redesign concerns: late entry, unsuitable filtration, incorrect S&R range, and trading near high-of-day without adequate S&R context." The D-011 records quote the goal (E11-01, lines 311 to 313) and record no problem for strength: "Current problem | none recorded for strength" (the requirements record, section 7) and, under the Master Agenda's Part 3, "8. Problems / limitations | none located" (section 4). The range "D-009 to D-016" on NR9-12 ("adequate S&R context" near the day's high) is set aside as "entry, not strength" (the stage record, section 2.1, a disposition that is INTERPRETATION from the cell).
- The same name is used again in Section J ("J. S&R / ADVANCED ENGINES", line 434): "J-004  S&R strength" (line 440), under J's rule that asks for each advanced mechanism KEEP, MODIFY, REPLACE, REMOVE or SHADOW and ends "Presence of code does not automatically mean the mechanism belongs in the new live decision path." (lines 451 to 460; E11-02). The records keep the two apart: "The legacy mechanisms' disposition is J-004's (OQ11-02)." (the requirements record, section 1); how D-011 and J-004 are separated is OQ11-02, OPEN (section 7.1).
- What already pointed to D-011 when it started (the stage record, section 4):
  - FACT (V2 RECORD), E11-03: D-009's provisional boundary sends "strength to D-011" (E10-03, "a PROVISIONAL WORKING POSITION, ChatGPT's proposal"); OQ9-14 asks "What if a closer level competes with a stronger one, or levels and zones overlap?", with the note "zones, strength and freshness themselves are D-010 to D-012's" (E10-06); OPEN.
  - FACT (V2 DECISION; V2 RECORD), E11-04: D-010 (DR-077): "RQ10-01 to RQ10-05 define the references strength would describe"; OQ10-07 OPEN ("strength with D-011"); P10-03 (Claude's proposal: "Strength or confidence goes to D-011, age or freshness to D-012"); BK-80 OPEN: "which attributes are strength-like or age-like, and so D-011's or D-012's."
  - FACT (V2 DECISION), E11-05: "DR-078 / W-12: the sequence D-010, D-011, D-012 and the later eligible topics."
  - RAMA (STATED); P4 (the stage record's label; P4 is the wording ChatGPT drafted at his request), E11-06: ChatGPT's option list in D-010's answer sheet, Q7, with "reaction/touch count;", "last reaction date/time;", "confidence/strength indicator (owned by D-011, not decided here);" and "age/freshness indicator (owned by D-012, not decided here);", and Rama's answer and Stage 0 words (section 7.2).
  - The range forms that cover D-011 (L11-05; the stage record, section 2.1), among them "D-009 to D-012" and "D-005 to D-012" (recorded owners) and "D-009 to D-017" (VP-08 and the strategy-count scheduling: "VP-08 not ratified at D-011 (OQ11-08)"); "each a disposition, INTERPRETATION from the cell; nothing decided".
- The question D-011 was given (FACT (V2 RECORD), the stage record, section 1): "what V2 requires of an S&R reference's strength, at requirement level, given D-010's references (RQ10-01 to RQ10-05, DR-077)." Claude put it to Rama as four questions (the stage record, section 5.1; this manual, section 5.3): Q1 "Strength" (show a strength for each reference?), Q2 "Evidence" (which kinds may it rest on?), Q3 "Display" (how is it shown?) and Q4 "Recency" (does recency count toward strength, or stay with D-012?).

## 5. Course of the topic

| Time (IST) | Step | Who | Record |
|---|---|---|---|
| 18:37 | "D-010 published (7c725ed); D-011 authorised to start (W-12)" | Claude | the requirements record, section 12; the stage record, header ("D-010 was published at 18:37 IST") |
| from 18:40 | the stage record prepared ("Prepared 30-Oct-2026 (IST), from 18:40, by Claude.") | Claude | the stage record, header |
| 18:40 to 18:46 | "the plan, locators L11-01 to L11-04 and reads R11-01 to R11-05" | Claude | the requirements record, section 12; V2_DESIGN_LOG line 618 |
| about 18:46 | the decision request: four questions in the question tool, Claude's wording | Claude | the stage record, section 5.1 |
| 18:47 | Rama's answers: a screenshot of his message to ChatGPT and ChatGPT's reply, relayed by Rama | Rama; ChatGPT | the stage record, section 5.2 |
| 18:50 to 18:54 | "the stage record, this record and the governance edits; a local commit (200497d, never pushed)" | Claude | the requirements record, section 12 |
| 18:52 | after a fetch, the base checked at 7c725ed; "git status not used" | Claude | the post-push record, section 1 |
| 18:54 | "a first local commit, 200497d, while the independent review ran; never pushed" | Claude | the post-push record, section 1 |
| from about 18:54 | the independent review (W-12): "PASS WITH FINDINGS, F-01 to F-18" | a separate Claude subagent | the review record, sections 1 to 3; the requirements record, section 12 |
| about 19:30 | Claude's three follow-up questions in the question tool | Claude | the stage record, section 5.3 ("question tool at about 19:30 IST"); the requirements record, section 12 ("before 19:35") |
| 19:35 | Rama's follow-up answers: a screenshot of ChatGPT's reply, relayed by Rama | Rama; ChatGPT | the stage record, section 5.3 |
| from 19:36 | "the revision, the range locators (L11-05) and the storage action (DR-079, DR-080)" | Claude | the requirements record, section 12 |
| about 19:37 | L11-04's count "reproduced at about 19:37 IST with git grep -c -E at 7c725ed" | Claude | the stage record, section 2 |
| 19:42:48 | commit d665719 (200497d amended; commit time) | Claude | the post-push record, section 2 |
| 19:43:02 to 19:43:05 | origin/main re-fetched; the push to HEAD:refs/heads/main (7c725ed..d665719); local HEAD equal to origin/main | Claude | the post-push record, section 2 |
| 19:43:05 to 19:43:06 | the fresh-clone check | Claude | the post-push record, section 3 |

- The stage record's section 2 heading reads "LOCATOR SEARCHES (FILE-SEARCH TOOL; 18:41 TO 18:45 IST)" over all five locators; L11-05's own row says "after the review's F-10", the requirements record places "the range locators (L11-05)" from 19:36, and L11-04's count was reproduced with git grep at about 19:37. INTERPRETATION (this manual's own): the heading's time and tool therefore fit L11-01 to L11-04 as first run, not L11-05 or the rerun (a reading of the records, not a correction of them).

### 5.1 The plan, the locators and the reads (18:40 to 18:46)

- Sources (the stage record, section 1): "P1 Rama's statements and choices (the D-010 answer sheet and register RI rows; his choices in section 5); P2 the 39 frozen manuals, by term search and the matched lines with their headings, cited as HISTORICAL; P3 the V2 records; P4 the wording of his answers, which ChatGPT drafted at his request. P5: not needed."
- "Not used: any legacy repository, clone, saved listing, host, VM, runtime, broker or database (DR-073); docs/inputs/legacy_baseline/; the web."
- "Controlling records read before acting: the register (DR-031, DR-069, DR-072 to DR-078), the design log (sections 1, 4, 5, 7 and 8), the Master Agenda (Parts 3, 5, 7 and 8) and the D-010 records, all at D-010's storage the same day; W-11 and DR-074 for Route B (the Route B change record itself was read at the follow-up, section 5.3)."
- Tools: "the file-search and file-reading tools; shell prints of single matched lines with their headings (section 3; DV11-03); no script over legacy material (none exists here); git for the counts' rerun and the storage step."
- The locators (the stage record, section 2):
  - L11-01, over the 39 frozen manuals, for D-011, J-004, "S&R strength", ScoredZone, zone_scoring, touch_cap, confluence and three owner ranges: "8 matches on 7 lines of 5 manuals"; "No manual names D-011 or J-004 (a bounded negative over the 39)".
  - L11-02, over the register, the design log and the Master Agenda: the agenda's D-011 and J-004 lines, DR-077, DR-078, W-12 and BK-80; two other "strength" hits ("a secret's strength"; "a screener's sector_strength") "set aside".
  - L11-03, over the 39 frozen manuals, for strength, touches, scored and zone-score terms: "35 matches in 10 manuals"; besides L11-01's lines, none on a line that also names S&R, a zone, support or resistance except two lines "both set aside".
  - L11-04, over the D-008 to D-010 folders, Route B, the section-end papers and the V2 design-inputs files: "92 matching lines in 24 files, nearly all in the D-009 and D-010 records"; "used where relevant (E11-03, E11-04, E11-06)".
  - L11-05, the range forms that cover D-011, added "after the review's F-10", with "dispositions in section 2.1".
- The reads (the stage record, section 3): R11-01 C-011 line 735, R11-02 A-009 line 137 ("the sr_detector fields only"), R11-03 D-003 line 484, R11-04 C-008 line 438 ("set-aside check") and R11-05 D-010's answer sheet, Q7, lines 86 to 99. "Reused without a new read": E10-03, E10-06, E10-19, E10-21, E10-22 and E10-34, E9-26, NR9-14, and the register rows RI-02 and RI-07.

### 5.2 The evidence (E11-01 to E11-13)

- The stage record's section 4. E11-01 and E11-02 are the Master Agenda (section 4 above); E11-03 to E11-05 are the V2 decisions and records that pointed to D-011 (section 4); E11-06 is Rama's D-010 words (section 7.2); E11-07 to E11-11 are the legacy rows and E11-12 the existing gap rows (section 8); E11-13 is Claude's INTERPRETATION of them (section 8).

### 5.3 The decision request and the answers (about 18:46 to 18:47)

- Claude's four questions (the stage record, section 5.1, "Claude's wording, exact"; "The "(Recommended)" marks below are PROPOSAL (Claude): Claude's own recommendations; Rama's answers followed all three, through ChatGPT's drafts."):
  - Q1 "Strength": options "Yes, as information (Recommended)", "No strength at all" and "Decide later in design". The question text said "(Legacy records only name strength-like fields; what they did was never established, so nothing is carried over.)", and the first option "It gives no Trade/Reject, score or SL/target authority (that stays with D-009 and later topics)."
  - Q2 "Evidence" ("more than one may be ticked"): "Number of reactions" (citing "your "multiple touches", D-010 Q1"), "Role switches" (citing "your Rs 277.95 example, D-010 Q4"), "Seen on several timeframes" ("which timeframes waits for D-007") and "Size of the reaction". Kinds only: "how each is counted or weighted is NOT decided here."
  - Q3 "Display": options "Evidence plus a label (Recommended)", "Evidence only", "Label only" and "Decide later in design".
  - Q4 "Recency": options "Keep it in D-012 (Recommended)", "Part of strength" and "Decide in D-012".
- The answers (section 5.2): "The tool returned: Q1 "Refer answers in above attached screenshot"; Q2, Q3 and Q4 "[No preference]"." "At 18:47 IST Rama sent a screenshot (not stored, DR-008) of his message to ChatGPT and ChatGPT's reply". Rama's message (RAMA (STATED), as transcribed): "Answer these 4 questions suitably & correctly even my 'raw' wordings are incorrect". ChatGPT's reply opens "Based on the established D-010 decisions and the separation between D-011 (Strength) and D-012 (Freshness), I recommend these four answers."; its "Final selection summary": Q1 "Yes, as information"; Q2 "Number of reactions + Role switches + Size of reaction"; Q3 "Evidence plus a label"; Q4 "Keep it in D-012"; and its closing bound (section 1).
- INTERPRETATION (Claude): "that reply, given in Q1's field, covers all four answers (his message to ChatGPT: "Answer these 4 questions")."
- Compared with the options, "none resolved silently" (section 5.2): no answer reverses its option; two differences (Q1's limiting list, and Q4's description of D-012) went to Rama after the review (section 5.4); the rest became open questions (OQ11-03, OQ11-04, OQ11-06) or are noted as they are (section 6.1).

### 5.4 The independent review and the follow-up (from about 18:54 to 19:35)

- The review's three MAJOR findings (the review record, section 2): F-01, Route B's per-topic steps "were replaced without disclosure"; F-02, "RQ11-01's "must not independently determine" limits other topics, while the records say D-011 decides no use of strength"; F-03, "RQ11-04 records a definition of D-012 freshness as decided, while the records say nothing about freshness is decided". Claude put each to Rama as one of three questions, "Process", "Strength use" and "Freshness" (the stage record, section 5.3, "Claude's three questions, exact").
- The tool returned "[No preference]", "[No preference]" and "Answers"; at 19:35 Rama sent a screenshot (not stored, DR-008) of ChatGPT's reply; "his message to ChatGPT is not in it." ChatGPT's "Final selections": Q1 "Yes, this way"; Q2 "Input to those topics"; Q3 "Decided starting point". Their effect is in sections 6.1 and 6.2; D-011_review_notes.md, section 2, gives the review's findings and resolutions.

### 5.5 The revision and the storage (from 19:36 to 19:43)

- The findings were resolved in one revision: "The commit 200497d is amended locally before the push; it was never pushed. DR-079 was revised and DR-080 added before their first publication; no published register row was edited." (the review record, section 2). The storage manifest's checks (section 3) passed (the post-push record, section 1). The commit, push and fresh-clone check are in D-011_review_notes.md, section 6.

### 5.6 After publication (later records; not D-011 steps)

- 19:43: "D-011 published (d665719); D-012 authorised to start (W-12)" (D-012_v2_requirements.md, section 12). D-012 was prepared from 19:45 and stored at 20:32 by 3674847, which also stored D-011's post-push record and push line (DR-081; V2_DESIGN_LOG line 621: "the D-011 records stored").
- 21:48: Topic_Workflow.txt with three screenshots of Rama's ChatGPT conversation, transcribed in TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt, section 2A: ChatGPT's table showed "D-011 -- S&R Strength" "Published and fresh-clone verified", and after Rama's concern it planned "Then return to D-010, D-011 and D-012 for a second, deeper audit." 22:14: ChatGPT's decisions relayed by Rama: DR-082 (W-14), which supersedes DR-080 (W-13), and DR-083, with BK-85.
- 31-Oct-2026, 10:10: the topic-workflow package, 6bd80a5 (commit time), pushed after ChatGPT's OK TO PUSH (V2_DESIGN_LOG line 626).
- 31-Oct to 01-Nov-2026: the D-006 to D-010 documentation batch (DR-084), with "Not in scope: D-011 and D-012"; its commit 42e6533 (08:54:51 IST on 01-Nov).
- 01-Nov-2026: the settlement before D-011 (from 10:40 IST; V2_DESIGN_LOG line 632), which inspected D-011 for consistency without restarting it (section 9.5); its commit a4247239 is this manual's base. 20:22 IST: Rama's instruction for this documentation (section 1).

## 6. What is recorded as decided

### 6.1 D-011's requirements (FACT (V2 DECISION), DR-079)

- DR-079: "DECIDED at requirement level only, by Rama's choices of 30-Oct-2026 in two rounds". The requirements record, section 2.1, governs; "The wording below is quoted exactly from the stage record's transcriptions; in RQ11-02 the check marks are written [x] and [ ], and the long dash --." The short forms in the table are the design log's own (section 4, line 377).

| ID | In short (V2_DESIGN_LOG section 4) | Status label | Source |
|---|---|---|---|
| RQ11-01 | "strength as descriptive information, without trading authority, its use decided by the later topics" | DECIDED at requirement level | the requirements record, 2.1; the stage record, 5.2 (Q1) and 5.3 (Q2 "Strength use") |
| RQ11-02 | "the evidence kinds number of reactions, role switches and reaction size" | DECIDED at requirement level | 2.1; 5.2 (Q2) |
| RQ11-03 | "the evidence shown with a label such as Weak, Medium or Strong" | DECIDED at requirement level | 2.1; 5.2 (Q3) |
| RQ11-04 | "recency kept with D-012, whose starting definition is decided" | DECIDED at requirement level | 2.1; 5.2 (Q4) and 5.3 (Q3 "Freshness") |

The decided wording, exact (the requirements record, section 2.1), with the option chosen:
- RQ11-01: "Each S&R reference may display how strongly its historical price behaviour is supported." "Strength is descriptive information only." With the follow-up: "D-011 establishes S&R strength as descriptive information." "Equally, D-011 itself does not grant strength any trading authority." Options chosen: 18:47, "Yes, as information (Recommended)"; 19:35, "Input to those topics".
- RQ11-02: "[x] Number of reactions -- How many historical times price reacted around the reference." "[x] Role switches -- Whether the reference has historically acted as both support and resistance." "[x] Size of the reaction -- How significantly price moved away after reacting." "These are eligible evidence categories, not approved calculation or weighting rules." Option chosen: "three of the four kinds".
- RQ11-03: "Show supporting historical evidence alongside a descriptive label such as:" "Weak" "Medium" "Strong" "The actual classification methodology, thresholds, weighting and calculation remain undecided." Option chosen: "Evidence plus a label (Recommended)".
- RQ11-04: "D-011 Strength: How strongly historical price behaviour supports an S&R reference." "Do not mix recency into the D-011 strength calculation." D-012's starting definition, decided at the follow-up: "D-012 Freshness: How recent or stale the historical price behaviour supporting an S&R reference is." with "Recency is separate from D-011 strength." "D-012 owns the freshness assessment." "D-012 must determine its remaining requirements, evidence, methodology and treatment independently." Options chosen: 18:47, "Keep it in D-012 (Recommended)"; 19:35, "Decided starting point (Recommended)".

Provenance, as the records give it:
- FACT (V2 RECORD): "The option texts are Claude's wording, and its "(Recommended)" marks were PROPOSAL (Claude); the selections are Rama's, through ChatGPT's drafted answers. At 19:35 he chose "Input to those topics", not Claude's recommended "Decided limit"." (the requirements record, 2.1).
- The 18:47 answers: "Provenance: RAMA-AUTHORISED, DRAFTED BY CHATGPT (as D-010's follow-up answers): Rama asked ChatGPT to answer, and relayed its answers to Claude with the tool reply "Refer answers in above attached screenshot". [...] The choices are his decisions; the wording is ChatGPT's; none of it is presented as verbatim Rama wording." (the stage record, 5.2).
- The 19:35 answers: "Provenance: Rama's choices, relayed by him; the wording is ChatGPT's. INTERPRETATION (Claude): as at 18:47, he asked ChatGPT to answer; his request is not shown." (5.3). The requirements record's heading for 2.1 is "Decided at requirement level (DECISION (RAMA); RAMA-AUTHORISED, DRAFTED BY CHATGPT)", and it reads the tool replies so: "INTERPRETATION (Claude): each reply points to the screenshot sent with it."
- The follow-up's effect (5.3): "RQ11-01's decided part is the information role, without trading authority; RQ11-04 carries D-012's decided starting definition; the amended Route B procedure is DR-080 and W-13 (section 1A)."

What each requirement does not decide (the requirements record, section 2.1, last column, exact):
- RQ11-01: "whether every reference must show it (OQ11-03); every use of strength, which the later topics decide (OQ11-01); the 18:47 list "It must not independently determine:" "Trade/Reject." "Trade priority or score." "Entry eligibility." "SL or target." is carried to D-009, D-013 to D-018 and E-005 to E-007 as an input, not a decided limit ("Do not impose a blanket restriction that strength can only ever be combined with other evidence.")".
- RQ11-02: ""[ ] Seen on several timeframes." with "Reason: D-007 is still BLOCKED. Timeframe hierarchy and interpretation must not be assumed.": OPEN / UNRESOLVED on D-007, not rejected (OQ11-06); how each kind is defined, counted or weighted (OQ11-05)".
- RQ11-03: "the label set ("such as") and how a label is set (OQ11-04)".
- RQ11-04: "D-012's own requirements beyond its starting definition (D-012); the strength calculation itself (RQ11-03). The 18:47 wording of the D-012 description ("How recent or stale that supporting historical behaviour is.") stays as history".

The answers compared with Claude's options (the stage record, 5.2 and 5.3; "none resolved silently"):
- Q1: "the option said "Each reference shows how strongly history supports it", the answer "may display" (OQ11-03); the option said "It gives no Trade/Reject, score or SL/target authority", the answer "must not independently determine" and adds "Trade priority" and "Entry eligibility" (put to Rama at the follow-up, section 5.3, Q2)".
- Q1's question text: "said "so nothing is carried over" (Claude's wording); the records say only that no KEEP or MODIFY is available and the disposition is J-004's".
- Q2: ""Seen on several timeframes" is left unchecked "for now" because of D-007: OPEN / UNRESOLVED, not rejected (OQ11-06)".
- Q3: "the option said "(for example the reaction count and dates)"; the answer names no dates (OQ11-04)".
- Q4: "the answer adds a description of both concepts; the D-012 description was put to Rama at the follow-up (section 5.3, Q3)".
- At the follow-up (5.3): "Q2's answer is not the option Claude recommended; the 18:47 answer's "must not independently determine" list is recorded as an input to the later topics, not a decided limit. Q3's starting definition words freshness differently from the 18:47 answer ("the historical price behaviour supporting an S&R reference" against "that supporting historical behaviour"); the 19:35 wording is the decided starting point, and the 18:47 wording stays as history."

How the requirements relate (the requirements record, section 3, headed "INTERPRETATION (Claude)"):
- "RQ11-01 makes strength information without trading authority; how it is used, alone or with other evidence, is each later topic's decision (OQ11-01; BK-82)."
- "RQ11-02 and RQ11-03 make strength explainable; the answer's reason: "This provides transparency instead of displaying an unexplained strength label." Both follow RQ10-03 (a reference rests on its own price history)."
- "RQ11-04 keeps strength, how strongly history supports a reference (D-011), apart from freshness, how recent it is (D-012). Whether the last reaction's date is shown with the strength evidence is not decided here (D-012; OQ11-04)."
- "RQ10-04 (positions relative to CMP) bears on how a role switch is recognised, which stays OPEN (OQ11-05)."
- "No requirement needs a timeframe or a formation decision, so DR-074's stop rule is not triggered; the timeframe kind of evidence stays OPEN / UNRESOLVED (OQ11-06), and any formation-based reaction waits on D-006 (OQ11-05)."

Legacy: "KEEP and MODIFY are not available (W-11). RQ11-01 to RQ11-04 are new V2 requirements, not carried from legacy." (the requirements record, section 4).

### 6.2 Other items recorded with D-011's package (not D-011 requirements, except where a row says so)

| Item | What was recorded | Status label, as recorded | Source |
|---|---|---|---|
| W-13 (DR-080) | Route B's per-topic steps 1 to 3 replaced "From D-011 onwards" by Claude's questions, Rama's answers ("ChatGPT may help clarify or formulate answers"), the independent Claude review and ChatGPT's audit after publication; "Rama's decisions remain authoritative. Independent Claude review does not give Claude authority to decide requirements on Rama's behalf." | "DECIDED" when stored (d665719); at a4247239 "DECIDED (SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026; it governed D-011 and D-012)" (6bd80a5) | DR-080; V2_DESIGN_LOG line 396; the stage record, 1A and 5.3 (Q1 "Process") |
| The follow-up choices (part of RQ11-01 and RQ11-04; section 6.1) | the 18:47 list is an input to the later topics, not a decided limit; D-012 starts from the decided definition | "DECISION (RAMA)" in the records' terms, in ChatGPT's wording | the stage record, 5.3; DR-079 |
| OQ11-08 | VP-08 at D-011 | "answered for D-011: not ratified; VP-08 stays PROPOSED for its later ratification points" | the stage record, section 6 |
| VP-08's D-011 note | "D-011 (30-Oct-2026; tracking only): not ratified at D-011." | "The Ratify-at cell is unchanged." | V2_DESIGN_LOG line 263 |
| BK-82 | OQ11-01: how strength is used in trade decisions, entry eligibility, SL and targets | "OPEN" | line 719 |
| BK-83 | RQ11-04: D-012's starting definition | "OPEN" when stored; "CLOSED (D-012, DR-081)" at a4247239 | line 720 |
| BK-80's D-011 note | RQ11-02 and RQ11-04 against D-010's OQ10-07 and P10-03 | "OPEN" ("stays OPEN for D-012") | line 717 |
| NR11-01 | section 8, row 22 | "NOT RESEARCHED; an evidence gap, not a finding"; "OPEN" | line 752 |
| Five lessons | lines 186 to 190 | "Historical process lesson (not a system requirement)" | DR-079 ("Five historical process lessons") |

- W-13 is the one working rule set at D-011. D-011 started under W-12 (DR-078), decided at D-010.

## 7. What is not decided: open questions, unknowns, dependencies

### 7.1 D-011's open questions (the stage record, section 6, "OPEN QUESTIONS (NONE CLOSED BY THIS RECORD)"; the requirements record, section 8: "none is closed here (OQ11-08 is answered for D-011 only: not ratified)")

| ID | Question (the stage record, exact) | Status (DR-079) | Holder (the stage record, exact) |
|---|---|---|---|
| OQ11-01 | How is strength used in trade decisions, entry eligibility, SL and targets? Each later topic decides within its scope (section 5.3, Q2); the 18:47 answer's "must not independently determine" list is carried to them as an input, not a limit | OPEN | D-009 (OQ9-01, OQ9-03, OQ9-13 to OQ9-17), D-013 to D-018, E-005 to E-007; Rama; BK-82 |
| OQ11-02 | How are D-011 and J-004 (the legacy strength-like mechanisms) separated? | OPEN | governance; J-004 holds the disposition; no legacy research under D-011 |
| OQ11-03 | Must every reference show strength, or may it ("may display" against the option's "shows")? | OPEN | design, then Rama |
| OQ11-04 | The label set ("such as" Weak, Medium, Strong) and how a label is set; whether reaction dates are shown with the evidence (D-012) | OPEN | design |
| OQ11-05 | What counts as a reaction, how a role switch is recognised and how a reaction's size is measured (D-010 UK-03, OQ10-04, P10-01) | OPEN | design; any formation-based reaction waits on D-006 |
| OQ11-06 | Whether appearing on several timeframes may count as evidence | OPEN | OPEN / UNRESOLVED on D-007 |
| OQ11-07 | How much history strength draws on (D-010 UK-08) | OPEN | D-007; K-009, K-010 |
| OQ11-08 | Is any part of VP-08 ratified at D-011? | answered: not ratified | answered for D-011: not ratified; VP-08 stays PROPOSED for its later ratification points |
| OQ11-09 | Whether strength is recorded with each signal for later review | OPEN | D-009 OQ9-22; K-008, K-011 |
| OQ11-10 | How strength is shown when a stock's history is incomplete (D-010 P10-05) | OPEN | design; D-009 OQ9-21 for any signal effect |

- OQ11-08's answer is D-011's only. VP-08's status is as the design log gives it: "Count DECIDED (DR-031); D-003, D-009 to D-017, J" in its Ratify-at cell, and "The rest of VP-08 stays PROPOSED." (line 263). The D-011 records' shorter wording, "VP-08: PROPOSED / NOT RATIFIED." (the stage record, section 7) and "VP-08 PROPOSED / NOT RATIFIED." (the post-push record, section 4), is read with that line.
- Later records cite D-011's questions without changing them: D-012's OQ12-05 names "D-011 OQ11-05" (D-012's stage record, section 6), and D-012's record says of "should display" against "may display": "for strength, whether every reference must show it is OPEN (D-011 OQ11-03)." (D-012_v2_requirements.md, section 3, INTERPRETATION (Claude)).

### 7.2 Rama's own words behind the decisions (RAMA (STATED); "RAW / PROVISIONAL USER INPUT"; the requirements record, section 2.2)

| Source (D-010's answer sheet, through the register) | Rama's words, as the requirements record quotes them | How D-011's records use them |
|---|---|---|
| Q1 (RI-02, line 34) | "multiple touches whenever this price is reached" | "(number of reactions)"; cited in Claude's Q2 option (section 5.3) |
| Q4 (RI-07, line 66) | "Rs 277.95 acts as support during Feb 10-19, But the same price acts as resistance during 20-Apr-2026" | "(a role switch)"; cited in Claude's Q2 option |
| Q7 (RI-10, line 99), to ChatGPT's list that put a "confidence/strength indicator (owned by D-011, not decided here);" beside "reaction/touch count;" and "last reaction date/time;" | "Time being add your best opinion and mark with needs to be discussed later in new system design" | listed in the requirements record, section 2.2; the list is ChatGPT's (E11-06, "P4") |

- The decision record's "Reason" field (the requirements record, section 7): "Rama's own examples (section 2.2) and his choices, in ChatGPT's wording, with the bound in the header". The examples keep the label of section 2.2's heading, "RAW / PROVISIONAL USER INPUT".

### 7.3 Unknowns and what the records could not establish

- The question-tool exchange: the reviewer "Could not verify: the question tool's exchange and Claude's question wording (no stored record besides the stage record); the times; which files L11-04 searched (resolved, F-09)." (the review record, section 1).
- UNKNOWN / NOT ESTABLISHED: Rama's request to ChatGPT for the 19:35 answers ("his message to ChatGPT is not in it", the stage record, 5.3).
- The legacy strength-like items: what the ScoredZone properties and the capture's zone_scoring argument computed, and whether any strength measure ran: NOT RESEARCHED (NR11-01); what the detector's defaulted fields did: UNKNOWN / NOT ESTABLISHED (NR9-14) (section 8).
- L11-01's result is a bounded negative over the 39 frozen manuals and the listed terms: "No manual names D-011 or J-004 (a bounded negative over the 39)".

### 7.4 Dependencies (the requirements record, sections 7 and 10)

| Item | Topic | Status, as recorded | Relation to D-011 |
|---|---|---|---|
| Use of strength in the trade decision; a closer level against a stronger one (OQ9-14) | D-009; D-017; D-018 | "DRAFT RECORDED (DR-076); not started" | OQ11-01; BK-82 |
| Use of strength in entry, SL and targets | D-013 to D-016; E-005 to E-007 | "not started" | OQ11-01; BK-82 |
| Freshness, recency, decay | D-012 | "next (W-12)" | RQ11-04's starting definition; BK-83 |
| Timeframes | D-007 | "BLOCKED / NOT CLOSED (DR-073)" | OQ11-06, OQ11-07 |
| Formations | D-006 | "DEFERRED / NOT CLOSED (DR-072)" | OQ11-05 |
| Candle and market data | K-009, K-010 | "not started" | OQ11-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | "OPEN; not started" | OQ11-09 |
| Legacy strength-like mechanisms | J-004 | "not started (Route B topic)" | NR11-01 |

- The decision record's "Dependencies" field (the requirements record, section 7): "D-009 OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22, all OPEN (OQ9-01 to OQ9-25 stay OPEN); D-013 to D-018 and E-005 to E-007 (strength's use); D-007 BLOCKED / NOT CLOSED; D-006 DEFERRED / NOT CLOSED; D-012".
- The timeframe evidence waits on D-007: "OPEN / UNRESOLVED on D-007, not rejected (OQ11-06)"; how much history strength draws on waits on D-007 and the data topics (OQ11-07). Formation-based reactions wait on D-006: "any formation-based reaction waits on D-006 (OQ11-05)". The current status of each topic is in section 9.2.

### 7.5 D-009's questions that stay OPEN (D-009's record is not edited)

- "D-009's OQ9-01, OQ9-03, OQ9-13 to OQ9-17: RQ11-01 gives strength no trading authority, and its use is theirs to decide; all stay OPEN, with OQ9-01 to OQ9-25." (the requirements record, section 9, headed "Effect on other open items (INTERPRETATION (Claude); the other records are not edited)"). OQ9-21 (signal effect of incomplete history; OQ11-10) and OQ9-22 (recording per signal; OQ11-09) are named as holders in section 7.1. OQ9-14 (a closer level against a stronger one) is named in BK-82 and in the boundaries (section 7.4).

## 8. Legacy evidence

- Route B (DR-074, W-11) and option C (DR-073) applied: "No legacy source or legacy repository was read; frozen-manual lines were read only as the stage record lists." (the requirements record, header). The stage record lists docs/inputs/legacy_baseline/ among the sources not used (section 1). This manual adds no read of a legacy source or of a frozen manual; frozen-manual content reaches it only through D-011's stage record and requirements record.
- What the records hold, by the Master Agenda's Part 3 items (the requirements record, section 4, "Legacy evidence (FACT (HISTORICAL); the Master Agenda's Part 3, items 1 to 8)"):

| Part 3 item | What the records hold, with its label and qualifiers | Evidence |
|---|---|---|
| 1. Current system | FACT (HISTORICAL): "recorded owners only: "D-009 to D-012 S&R" for "levels and zones"; "S&R, zones, strength and freshness (D-009 to D-012);"" (D-004 1.3, line 50; D-005 1, line 15); "No line names D-011 alone." The detector is "the shadow S&R detector, enabled in production" (A-009 8, line 137), "kept as recorded (W9-01)" | E11-07, E11-10 |
| 2. Code / modules | FACT (HISTORICAL), "names only": "Zone and ScoredZone properties" with "classified S&R; not documented" (D-005 7, line 158); WatchlistCaptureWorker(..., zone_knobs=..., zone_scoring=...) ("FACT for the lines"; C-011 11.2, line 735); 11 defaulted detector fields, "among them weak_breakout_frac and touch_cap (and recency_window_days and recency_min_factor, which concern recency: D-012)" (A-009 8, line 137; its heading "FACT, SOURCE-VERIFIED unless marked") | E11-08 to E11-10 |
| 3 to 6. Inputs, process, output, state change | "what any strength-like computation used, did, produced or stored": NOT RESEARCHED (NR9-01; NR11-01); "the detector fields' effect UNKNOWN / NOT ESTABLISHED (NR9-14)" | - |
| 7. Dependencies | "none located" | - |
| 8. Problems / limitations | "none located" | - |

- Also, not a Part 3 item (E11-11): D-003 13, lines 484 and 488: MC-74's count over the four screener files "found one line, the homonym "to support special sessions""; MC-76 (INTERPRETATION, D-003's): "no S&R, regime or V3-chain input reaches the screener". "Not a strength mechanism."
- E11-13, INTERPRETATION (Claude): "the frozen manuals give names only for anything strength-like (E11-08 to E11-10), and calling them "strength-like" is a selection by name, not evidence of what they did; KEEP and MODIFY are not available (W-11). A gap row for the zone scoring would serve J-004, not D-011 (section 6)."
- NOT RESEARCHED and UNKNOWN gaps (V2_DESIGN_LOG section 8; each "an evidence gap, not a finding"; each OPEN). D-011's design-log row names "section 8, row 22, OPEN"; its requirements record also cites rows 8 and 21 (E11-12: "Neither names the ScoredZone properties or the capture's zone_scoring argument."):
  - row 8, NR9-01 (line 738; raised at D-009): "what the S&R detector computed (levels or zones, method, use of timeframes and of its 11 defaulted fields)"; NOT RESEARCHED;
  - row 21, NR9-14 (line 751; raised at D-009): "what the 11 defaulted detector fields did, and whether any default changed a trade ("UNKNOWN", A-009_manual.md section 8)"; UNKNOWN / NOT ESTABLISHED;
  - row 22, NR11-01 (line 752; raised at D-011, 2026-10-30): "what the ScoredZone properties (D-005_manual.md section 7, line 158) and the watchlist capture's zone_scoring argument (C-011_manual.md 11.2, line 735) computed, and whether any strength measure ran (the detector's defaulted fields: as row 21, NR9-14); the frozen manuals give names only, and the items are selected by name (INTERPRETATION); NOT RESEARCHED"; why it matters: "a KEEP or MODIFY of any legacy strength or scoring logic would need it (W-11); D-011's record decides nothing that needs it (J-004)".
- Disposition (the requirements record, sections 4 and 6): "KEEP and MODIFY are not available (W-11)."; "The legacy strength-like mechanisms: no outcome at D-011; disposition with J-004 (DEFER, in Part 7's terms)."; "SHADOW not used (as at D-008 and D-010)."
- Other matched lines set aside (the stage record, section 2): register line 67 ("a secret's strength") and design-log line 224 ("a screener's sector_strength"), both at 7c725ed; C-003 line 795 ("a secret's strength") and C-008 line 438 (""touches" meaning "concerns""), "both set aside".

## 9. Cross-topic boundaries, dependencies and bookmarks

### 9.1 Chronology of the S&R topics (FACT (V2 RECORD); commit times from V2_DESIGN_LOG section 6 and the repository history)

| Topic | Recorded status | Commit (time) | Relation to D-011 |
|---|---|---|---|
| D-009 Support & Resistance | DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076); OQ9-01 to OQ9-25 OPEN | 9c9c2e9 (10:06, 30-Oct) | earlier: its provisional boundary sends "strength to D-011" (E11-03); its OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22 hold strength's use and stay OPEN; OQ9-14 in BK-82 |
| D-010 S&R zones | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077); RQ10-01 to RQ10-05 DECIDED | 7c725ed (18:37, 30-Oct) | earlier: "The references strength describes": RQ10-01 to RQ10-05 (the requirements record, section 10); OQ10-07, P10-03 and BK-80; D-011's base |
| D-011 S&R strength | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079) | d665719 (19:42, 30-Oct) | this topic |
| D-012 S&R freshness | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | 3674847 (20:32, 30-Oct) | later: started from RQ11-04's definition, which governs (DR-081); closed BK-83; stored D-011's post-push record and push line |
| Topic workflow | DR-082 (W-14, in effect from 22:14 IST on 30-Oct) and DR-083 | 6bd80a5 (10:10, 31-Oct) | later: supersedes DR-080 (W-13), which "governed D-011 and D-012"; requires a V2 topic manual and review notes for each Route B topic, whether and how D-011 receives them left to BK-85 |
| D-006 to D-010 documentation batch | DR-084; DR-085 | 42e6533 (08:54, 01-Nov) | later: "Not in scope: D-011 and D-012" |
| Settlement before D-011 | DR-086, DR-087, BK-86; D-012's documentation | a4247239 (19:00, 01-Nov; commit time) | later: BK-85's note of 2026-11-01 ("D-011 still lacks them"); the base of this manual |
| D-013 Entry zone | NOT STARTED (V2_DESIGN_LOG section 4, line 379) | - | waits for ChatGPT's start package (W-14) |

- D-010's requirements are its own record's decisions (DR-077). D-011 builds on them and does not edit them: "the other records are not edited" (the requirements record, section 9). D-009's draft is not changed by D-011.

### 9.2 Boundaries (the requirements record, section 10; status columns as recorded on 30-Oct and at a4247239)

| Item | Topic | Status in D-011's record | Status at a4247239, where it changed | Relation to D-011 |
|---|---|---|---|---|
| The references strength describes | D-010 | V2 REQUIREMENTS RECORDED (DR-077) | unchanged | RQ10-01 to RQ10-05 |
| Use of strength in the trade decision; a closer level against a stronger one (OQ9-14) | D-009; D-017; D-018 | DRAFT RECORDED (DR-076); not started | D-009 unchanged | OQ11-01; BK-82 |
| Use of strength in entry, SL and targets | D-013 to D-016; E-005 to E-007 | not started | D-013 NOT STARTED, waiting for ChatGPT's start package (W-14) | OQ11-01; BK-82 |
| Freshness, recency, decay | D-012 | next (W-12) | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | RQ11-04's starting definition; BK-83 (CLOSED) |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | unchanged | OQ11-06, OQ11-07 |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | unchanged | OQ11-05 |
| Candle and market data | K-009, K-010 | not started | - | OQ11-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | OPEN; not started | - | OQ11-09 |
| Legacy strength-like mechanisms | J-004 | not started (Route B topic) | - | NR11-01 |

- In the table's fourth column (the status at a4247239, where it changed), "-" means that no status at a4247239 is supplied there for that topic; it does not mean that its status is unchanged.
- Other V2 records that touch D-011 (the requirements record, section 5, "V2 records that touch D-011 (FACT, row level)"): "DR-078 / W-12: the topic sequence. DR-080 / W-13: Route B's amended procedure. DR-031: S&R among the owner allocation "C-001, D-003, D-009 to D-017 and J"."; "VP-08 (PROPOSED except its count; Ratify at "Count DECIDED (DR-031); D-003, D-009 to D-017, J"): not ratified at D-011 (OQ11-08)."
- VP-08's D-011 note (V2_DESIGN_LOG section 3, line 263), INTERPRETATION (Claude): "RQ11-01 to RQ11-04 (DR-079) describe strength as information, which "D-011 itself does not grant ... any trading authority" (Rama's follow-up, ChatGPT's wording), so they neither ratify nor reject the sentence "S&R logic is set after strategies and processors, and R:R honours S&R." The Ratify-at cell is unchanged."

### 9.3 Bookmarks (V2_DESIGN_LOG section 7; tracking only; "it closes only by a note in its row")

- BK-80 (D-010; OPEN; line 717): D-010's OQ10-07 and P10-03, which attributes are strength-like or age-like. D-011's note: "RQ11-02 (DR-079) lists the number of reactions, role switches and reaction size as strength evidence, and RQ11-04 keeps recency with D-012 (INTERPRETATION (Claude): this bears on, and does not close, the point); stays OPEN for D-012." D-012's later note leaves it "OPEN for the whole-design review". Cross-check: D-011, D-012.
- BK-82 (D-011; OPEN; line 719): OQ11-01, "how strength is used in trade decisions, entry eligibility, SL and targets"; "the 18:47 answer's "must not independently determine" list (Trade/Reject, trade priority or score, entry eligibility, SL or target) is carried to them as an input, not a limit; D-009 OQ9-14's "a closer level competes with a stronger one"; nothing decided". Cross-check: D-009, D-013 to D-016, D-017, D-018, E-005 to E-007.
- BK-83 (D-011; CLOSED; line 720): D-012's starting definition, "decided at Rama's follow-up (ChatGPT's wording)"; "D-012's own requirements are not decided here." D-012's note: "reviewed at the topic it names, under the rule at the head of this section; D-012 started from this definition and recorded RQ12-01 to RQ12-04 (DR-081), and D-011's definition governs (Rama's choice at D-012's follow-up); closed by this note". Status "CLOSED (D-012, DR-081)". D-012's RQ12-02 sentence "Freshness describes how recent or stale its supporting historical price behaviour is." is recorded as "A restatement of D-011's definition, which governs" (D-012_v2_requirements.md, section 2.1).
- BK-85 (W-14; OPEN; line 722): whether and how D-008 to D-012 receive a V2 topic manual and review notes "is not decided"; ChatGPT plans "a second, deeper audit" of D-010 to D-012; its note of 2026-10-31 (DR-084): "D-011 and D-012 are not in the batch; ChatGPT's re-audit and its scope are neither started nor decided; stays OPEN."; its note of 2026-11-01 (R-03): "For OP-01, the immediate missing-manual re-audit scope is D-011 and D-012." and "D-011 still lacks them. Stays OPEN." This manual concerns it (section 1).
- BK-86 (D-009 OQ9-20; OPEN; line 723), the S&R hybrid that Rama accepted on 01-Nov-2026: its cross-check column names D-009 OQ9-20 and RQ9-04, D-010 RQ10-01 and RQ10-02, D-006, the strategy topics and the whole-design review, not D-011. Claude's reconciliation report of 01-Nov (C-01, INTERPRETATION) found "No contradiction": "D-011 and D-012 add strength and freshness as descriptive properties of each reference, with use left to later topics"; and (C-04, INTERPRETATION) "D-011 and D-012 need not cite OQ9-20, since they leave all use to later topics". Nothing in BK-86 changes D-011.

### 9.4 D-006 and D-007

- D-007 (BLOCKED / NOT CLOSED, DR-073): the timeframe kind of evidence, "Seen on several timeframes", was left unchecked "for now" ("Reason: D-007 is still BLOCKED. Timeframe hierarchy and interpretation must not be assumed."), and is "OPEN / UNRESOLVED on D-007, not rejected (OQ11-06)"; how much history strength draws on waits on D-007 and the data topics (OQ11-07). D-006 (DEFERRED / NOT CLOSED, DR-072): "any formation-based reaction waits on D-006 (OQ11-05)". The record's reading that "DR-074's stop rule is not triggered" is labelled INTERPRETATION (Claude) (the requirements record, section 3; section 6: "D-007 and D-006: see section 3; not triggered.").

### 9.5 Later records that speak of D-011 (not D-011 decisions)

- D-012's records (30-Oct): DR-081 and D-012_v2_requirements.md take RQ11-04's definition as D-012's starting definition, "which governs"; D-012's OQ12-11 asks how its "Crossed since last reaction" relates to "D-011's "Role switches" (strength evidence)" (D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt, section 6); its requirements record's section 3 says "RQ12-03 and RQ12-04 make freshness explainable, as RQ11-02 and RQ11-03 do for strength." (INTERPRETATION (Claude)). These are D-012's records; they add nothing to D-011's requirements.
- ChatGPT's pre-push audit of the documentation batch (docs/inputs/reconciliation/D006_D010_ChatGPT_PrePush_Audit.txt): "D-011 and D-012 remain outside this batch."
- Settle_Before_D011.txt (docs/inputs/reconciliation/; ChatGPT's instruction at Rama's request, 13:31 IST on 01-Nov): "Do not start D-011 yet."; "D-011 and D-012 are already published; inspect them for consistency, not to restart either topic."; "Check D-011 and D-012 package completeness against the same agreed standard."
- Claude's reconciliation report (01-Nov, from 13:34 IST): F-05 restates RQ11-01 (strength "is descriptive information only"; D-011 "does not grant strength any trading authority"); X-01 (section 2.1); R-03: "D-011 is already RECORDED (DR-079). Suggested: (a) prepare its DR-083 manual, review notes and manifest (and D-012's), as new files, with no change to its requirements; (b) reopening any RQ11 item is a controlled revision and needs new evidence (W-14 section 8)."
- ChatGPT's review of 01-Nov-2026 (docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt, received 14:24 IST; titled "D-011 PREREQUISITE RECONCILIATION"), A.4, "D-011 / D-012": "Do not reopen existing requirements:" "D-011 strength remains descriptive and has no independent trading authority."; and R-03: ""D-011 later" means D-011 topic discussion/research is not authorised now. Do NOT prepare D-011 questions, answer sheets, manual, review notes or manifest." FACT (V2 RECORD) for the reader: in D-011's records the decided parts are "Strength is descriptive information only." and "Equally, D-011 itself does not grant strength any trading authority.", while the 18:47 list (strength "must not independently determine" Trade/Reject, trade priority or score, entry eligibility, SL or target) is "an input, not a decided limit" (the requirements record, RQ11-01's row; DR-079). ChatGPT's pre-push audit of 01-Nov (docs/inputs/reconciliation/D011_PrePush_Audit_HOLD.txt), section 1, corrected A.4's D-012 wording only; INTERPRETATION (this manual's own): a question this documentation raises, which no V2 record raises: whether A.4's "no independent trading authority" means RQ11-01's decided "does not grant strength any trading authority" or the 18:47 list's "must not independently determine", which is an input; UNKNOWN / NOT ESTABLISHED, as no record at a4247239 rules on it; for ChatGPT and Rama. D-011_v2_requirements.md governs; this manual records both texts and decides nothing between them.
- "NOT STARTED" in the records of 01-Nov: the HOLD audit, "D-011 remains NOT STARTED." and, in its governance state, "D-011: NOT STARTED; no D-011 questions, manual, review notes or manifest."; the settlement record (docs/agenda/pre-d011-settlement/PRE_D011_SETTLEMENT_RECORD_01-NOV-2026.txt, section 1): "D-011 NOT started: nothing was prepared for it (no questions, answer sheets, manual, review notes or manifest)."; V2_DESIGN_LOG line 632: "D-011 NOT started (to start in a new chat when Rama asks)"; ChatGPT's review of 01-Nov (docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt), its verdict: "D-011 has NOT been started." and its section D: "Claude must NOT start D-011.". INTERPRETATION (this manual's own): these records use "NOT started" of further D-011 work, as the settlement record's words "nothing was prepared for it" and R-03's gloss in the same review, ""D-011 later" means D-011 topic discussion/research is not authorised now.", suggest; Settle_Before_D011.txt calls D-011 "already published" and the reconciliation report "already RECORDED (DR-079)". FACT (V2 RECORD): none of them changes D-011's recorded status (DR-079; V2_DESIGN_LOG line 377, unchanged since d665719), and none is a D-011 decision. Rama (14:15 IST on 01-Nov, RAMA (STATED), as the settlement record transcribes it): "Don't start D-011 here, why? We need to start it new chat window, so complete all the things other than D-011".

## 10. Known defects, process deviations and lessons, as recorded

- KNOWN DEFECT, corrected before the push (F-01, MAJOR): "Route B's per-topic steps (ChatGPT's prompt and answer sheet; a ChatGPT and Claude discussion; ChatGPT's review) were replaced without disclosure; DV-01's method repeated"; its resolution: "the departures disclosed as DV11-01 to DV11-04 (stage record, section 1A), with the controlling records read; put to Rama as one question; his answer at 19:35, "Yes, this way", recorded as DR-080 and W-13 (the earlier steps kept as written)" (the review record, section 2).
- KNOWN DEFECT, corrected before the push (F-02, MAJOR): "RQ11-01's "must not independently determine" limits other topics, while the records say D-011 decides no use of strength"; resolution: "put to Rama: "Input to those topics". RQ11-01's decided part is now the information role, without trading authority; the list is carried to D-009, D-013 to D-018 and E-005 to E-007 as an input".
- KNOWN DEFECT, corrected before the push (F-03, MAJOR): "RQ11-04 records a definition of D-012 freshness as decided, while the records say nothing about freshness is decided"; resolution: "put to Rama: "Decided starting point"; RQ11-04 now carries D-012's decided starting definition, in the follow-up's wording".
- The review's other findings, F-04 to F-12 (MINOR) and F-13 to F-18 (NOTE), were resolved in the same revision before the push (the review record, sections 2 and 3); D-011_review_notes.md, section 2.2, gives them. What the reviewer could not verify is in section 7.3.
- Process deviations, disclosed (the stage record, section 1A): DV11-01 "no ChatGPT Stage 0 prompt and no answer sheet"; DV11-02 "no discussion of the options between ChatGPT and Claude"; DV11-03 "shell prints of single matched lines, the method of D-010's DV-01, which ChatGPT accepted only as "NOT A PRECEDENT""; DV11-04 "one record for Stages 0 and 1". "DV11-01 to DV11-04 stay disclosed; they are not erased." DR-079: they "are the procedure Rama then approved from D-011 onwards (DR-080)."
- The independent reviewer's own disclosed deviations (the review record, section 1): "six fixed-string git grep searches over all of docs/ had docs/inputs/legacy_baseline/ inside their scope; none returned a line from that folder, and no file there was opened, listed or printed. It wrote three scratch files outside the repository."
- KNOWN DEFECT, kept as published: the second independent review of the topic-workflow package (TOPIC_WORKFLOW_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt, G-02, MINOR) found the base wording "equal to the local main after a fetch" inaccurate and added: "The same wording in the published records of D-011 and D-012 (their manifests and post-push records; D-012's is row 5 of this package) is inaccurate in the same way, since the local branch main has stayed at 9c9c2e9 from 10:06 IST on 30-Oct (its reflog); there it stands for origin/main, against which the base was checked (INTERPRETATION (Claude)). They stay as published". In D-011's records: the storage manifest's "equal to the local main after a fetch at 18:52 IST" and the post-push record's "after a fetch, local main = origin/main = 7c725edcd2f4da36f89cda3de817cdfd5b3e1e0a".
- Lessons (V2_DESIGN_LOG section 1, all dated 2026-10-30, each "Historical process lesson (not a system requirement)"; the recording commit is from the repository history):
  - line 189 (d665719, D-011's own): "D-011 ran without ChatGPT's Stage 0 prompt and answer sheet and without a ChatGPT and Claude discussion of the options, which Route B's per-topic steps set, and its first records did not say so"; rule: "A departure from a topic's written procedure is disclosed in the topic's records when it happens, and a departure that would change the procedure goes to Rama as one question before the push.";
  - line 190 (d665719, D-011's own): its first records recorded as decided an answer that limits how later topics use strength and "a definition of D-012's subject"; rule: "When an answer limits another topic's use or defines a later topic's subject, Rama is asked whether it is a decided limit or an input to that topic (W-6) before it is recorded.";
  - line 191 (3674847, D-012's): "D-012's first records again recorded answer clauses that limit later topics as decided [...] although the lesson written at D-011 required asking first";
  - line 192 (6bd80a5): D-010's question "Next topic" and its "(Recommended)" mark; "Claude ran D-011 and D-012 and began D-013's searches", while Rama had meant one topic at a time; rule: "A question that would widen Claude's own authority or autonomy carries no "(Recommended)" mark and states plainly what each option would let Claude do without asking.";
  - line 193 (6bd80a5): "D-011 and D-012 were each decided on four questions, with a follow-up of three (D-011) and two (D-012) after the independent review"; rule: "A topic's questions are set by what the topic needs, never by a tool's limit";
  - line 194 (6bd80a5): D-008 to D-012 stored without the manual and review notes, "and D-011 and D-012 with four files each, against 12 to 15 for D-008 to D-010"; rule: "Before a package is drafted and again before it is published, its folder is compared with the established folder structure and the most recently completed comparable topics (W-14; Topic_Workflow.txt section 5)".
- DR-079 counts "Five historical process lessons": lines 186 to 190, recorded by d665719. Lines 186 to 188 record lessons from D-010's independent review (F-04, F-08, F-09, F-03 and F-07 of D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt) and do not name D-011.

## 11. Status

- D-011: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079): RQ11-01 to RQ11-04 DECIDED at requirement level; the 18:47 answer's "must not independently determine" list is an input to the later topics, not a decided limit; D-012's starting definition decided (RQ11-04); OQ11-01 to OQ11-10 OPEN (OQ11-08 answered for D-011: not ratified); not frozen. Published (DR-079); none of D-008 to D-012 is marked complete under DR-083 while BK-85 is OPEN (DR-083, point D).
- Unchanged: D-009 DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076), with OQ9-01 to OQ9-25 OPEN; D-010 (DR-077) and D-012 (DR-081) V2 REQUIREMENTS RECORDED; D-006 DEFERRED / NOT CLOSED (DR-072); D-007 BLOCKED / NOT CLOSED (DR-073); VP-08's count DECIDED (DR-031), "The rest of VP-08 stays PROPOSED.", not ratified at D-011; W-13 (DR-080) SUPERSEDED by DR-082, having governed D-011 and D-012; Route B (DR-074, W-11) and option C (DR-073) in force; section 8 rows 8, 21 and 22 OPEN; BK-80, BK-82 and BK-85 OPEN; BK-83 CLOSED (D-012, DR-081). D-013 NOT STARTED.
- Companion files in docs/agenda/d-011_sr-strength/: D-011_v2_requirements.md (authoritative) and the stage, review and storage records of section 2.1; drafted with this manual on 01-Nov-2026, to sit beside them (not in the repository at a4247239): D-011_review_notes.md, and a manual manifest written by Claude.
- Nothing here changes a decision or status. This manual adds no requirement, no design, no legacy read and no erratum to any record.
