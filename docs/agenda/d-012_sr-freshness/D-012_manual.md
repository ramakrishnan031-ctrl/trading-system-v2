# D-012 - S&R FRESHNESS (V2 TOPIC MANUAL)

## 1. Document status, purpose and scope

- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081). In the requirements record's own words (D-012_v2_requirements.md, section 13): "D-012: V2 REQUIREMENTS RECORDED (DR-081): RQ12-01 to RQ12-04 DECIDED at requirement level; not frozen." Its header adds: "RQ12-01 to RQ12-04 are DECIDED at requirement level by Rama (section 2.1), on the starting definition decided at D-011 (RQ11-04, DR-079). Everything else is OPEN or a dependency. NOT FROZEN. Requirement level only." DR-081's status cell reads "DECIDED"; its text gives "OQ12-01 to OQ12-13 OPEN (OQ12-08, VP-08 at D-012, answered: not ratified)". D-012 was the fifth Route B topic (DR-074, W-11), started under W-12 (DR-078) and run under Route B's amended procedure (DR-080, W-13).
- This is a V2 topic manual (DR-083), written on 01-Nov-2026 as documentation-only work (ChatGPT's review of 01-Nov-2026, R-03). It explains the topic and its records. D-012_v2_requirements.md is authoritative for requirements and decisions; this manual decides nothing, changes no status and adds no requirement.
- Baseline: the V2 repository at 42e6533 (commit time 08:54 IST on 01-Nov-2026, from the repository history). The four D-012 records in docs/agenda/d-012_sr-freshness/ were stored by commit 3674847 on 30-Oct-2026 and have not changed since (the repository history of that folder shows 3674847 only). Times are IST and, unless another day is named, on 30-Oct-2026. Paths are relative to the repository root. Line numbers are those at 42e6533. The drafting used only the sources listed in D-012_manual_manifest.md; every repository command named only files or folders outside docs/inputs/legacy_baseline/ and docs/legacy_system_audit/, or excluded both by git pathspec, except one git log of commit subjects only, which named no path (the drafting subagent's report; Claude's check of its transcript, which is not stored, found no command that listed, searched or read either folder; not independently verified).
- Purpose: DR-083 describes the manual as a "readable, detailed topic chapter explaining the subject, evidence, requirements, boundaries, unresolved matters and cross-references." and adds: "It must not invent requirements or duplicate the requirements file without adding explanatory value." (ChatGPT's wording, DR-083). This one does that for D-012: what the topic was asked, what Rama decided at requirement level and in whose wording, what stays open and where it waits, the legacy evidence and its limits, how D-012 stands against D-010, D-011 and the later topics, and where each record sits.
- In scope, as recorded (the requirements record, section 1, "Scope and boundary"): "D-012 describes freshness for the references D-010 defines (RQ10-01 to RQ10-05)." It starts (FACT (V2 DECISION), E12-02) from "D-012 Freshness: How recent or stale the historical price behaviour supporting an S&R reference is.", with "Recency is separate from D-011 strength." and "D-012 owns the freshness assessment." (Rama's D-011 follow-up, ChatGPT's wording).
- The boundary of the answers (the requirements record, header; "Rama's 19:53 answers, ChatGPT's wording"): "Governance boundary: These answers establish requirement-level direction for D-012, not an algorithm, expiry rule, threshold, timeframe hierarchy or trading-authority rule."
- Out of scope, as recorded (the requirements record, sections 1 and 11; the stage record, section 1):
  - the use of freshness: "It decides no use of freshness: "Later topics determine how they use it." (RQ12-01; Rama's choice, ChatGPT's wording; OQ12-01), and the answers' clauses on trade decisions and on exclusion from use are inputs to those topics (Rama, 20:27; the stage record, section 5.3)";
  - the legacy mechanisms: "The legacy mechanisms' disposition is J-005's.";
  - any design: "No design: no measure, formula, weighting, threshold, expiry rule, label rule, timeframe or hierarchy, lookback, schema, storage, gate, score or trade authority.";
  - and, from section 11: "No design or implementation decision, no owner moved, no VP ratified, no frozen manual or earlier decision changed." and "No legacy-source access (DR-073). D-013 is not started by this record."
- This documentation and BK-85: DR-083 requires a V2 topic manual and review notes beside each Route B requirements record. BK-85 (V2_DESIGN_LOG section 7, line 714, OPEN at 42e6533) records that D-008 to D-012 were stored before DR-083 without them and that "none is marked complete under DR-083 while this bookmark is OPEN (point D)"; its note of 2026-10-31 (DR-084) says "D-011 and D-012 are not in the batch". DR-082: "D-012 is recorded as published (DR-081); whether it is complete under DR-083 is BK-85's." ChatGPT's review of 01-Nov-2026 (R-03) writes "D-012 is already completed/published. Its missing DR-083 documentation package may be prepared as documentation-only work, without reopening requirements"; the V2 records keep D-012's publication apart from its completeness under DR-083 (ChatGPT's final pre-push audit of 31-Oct, point D: "The package does not mark D-012 complete under DR-083."). This manual does not close BK-85, mark D-012 complete or change D-012's status.

## 2. Records and how to read them

### 2.1 The topic folder (docs/agenda/d-012_sr-freshness/)

The four files stored by 3674847 (storage manifest, rows 1 to 3, and the manifest itself). At 42e6533 the folder holds these four only. This manual and D-012_review_notes.md were drafted on 01-Nov-2026 to sit beside them; ChatGPT's review of 01-Nov-2026, R-03, names a manual_manifest.md as the third file of the package.

| # | File | What it is | Status or revision, as recorded |
|---|---|---|---|
| 1 | D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | "the stage record" (the requirements record's name for it): the plan (section 1), locators L12-01 to L12-04 (2), evidence E12-01 to E12-07 (3), range dispositions (4), Claude's questions, Rama's answers and the follow-up (5.1 to 5.3), OQ12-01 to OQ12-13 with the bookmarks and legacy gaps (6), status (7) | banner "*** EVIDENCE AND DECISION RECORD. NO DESIGN. CLAUDE DECIDES NOTHING; ITS "(Recommended)" OPTIONS ARE PROPOSALS. ***"; "Baseline: the V2 repository at d665719."; revised after the independent review, before the push (the review record, section 2) |
| 2 | D-012_v2_requirements.md | the requirements record (DR-081's output) | authoritative; "V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081)"; "NOT FROZEN" |
| 3 | D-012_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | the W-12 review: reviewer, scope, findings F-01 to F-11 and their resolutions, verdict | "Recorded 30-Oct-2026 (IST) by Claude, the package's author."; verdict "PASS WITH FINDINGS (no BLOCKER)" |
| 4 | D-012_STORAGE_MANIFEST_30-OCT-2026.txt | the 4 rows stored by 3674847 with SHA-256 and bytes, the governance changes, the checks and what was still owed | "This manifest does not list itself"; its SHA-256 is in the post-push record, section 3 |

- FACT (V2 RECORD): D-011 and D-012 "hold 4 files each, against 12 to 15 for D-008 to D-010: under W-13 they had no ChatGPT Stage 0 prompt, answer sheet or discussion files." (TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt, R-04). The design-log lesson of line 194 records the same difference (section 10).
- The stage record's status section (7) and the requirements record's section 13 give the status at storage; nothing in the folder has been edited since.

### 2.2 Inputs in docs/inputs/

- docs/inputs/chatgpt_originals_and_replies/D-012_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt: Claude's post-push record of 3674847 ("Written 30-Oct-2026 (IST) by Claude, after the push."), stored by the next package, 6bd80a5 (the topic-workflow package; its storage manifest, row 5; DR-082). It is the one D-012 file that git ls-files finds at 42e6533 under docs/inputs/reconciliation/, docs/inputs/manuals/ and docs/inputs/chatgpt_originals_and_replies/ by names containing "12" (the other matches are B-002 and B-012 files).
- Two of its lines were true when written and later superseded: its banner, "OWED TO THE NEXT PACKAGE (D-013)", and section 4, "D-013 (Entry zone): next under W-12 and W-13." DR-082: they "were true when it was written and are superseded by W-14."
- Stored by D-012's package but D-011's record: docs/inputs/chatgpt_originals_and_replies/D-011_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt, "owed by the D-011 package" (storage manifest, row 4).
- Not stored (storage manifest, section 1): "No ChatGPT or Rama file is stored: Rama's 19:53 answers came as a screenshot (not stored, DR-008), transcribed in row 1, section 5.2; his 20:27 choices were made in the question tool and are recorded in row 1, section 5.3." Rama's own message to ChatGPT "is not in it" (the stage record, section 5.2).
- No ChatGPT prompt, answer sheet or review for D-012 exists to store: under W-13 "1. Claude prepares the topic-specific questions and answer sheet." (DR-080), and "W-13's step 1 ("the topic-specific questions and answer sheet") is met by Claude's four questions in the question tool (section 5.1), with the answers recorded in section 5.2; no process departure from W-13." (the stage record, section 1).
- ChatGPT's review of 01-Nov-2026, the authority for this manual, is stored unchanged by the settlement package before D-011 as docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt (not at 42e6533).

### 2.3 Register rows, design-log entries and the post-push record

- V2_DECISION_REGISTER.md:
  - DR-081 (line 92): D-012's requirements; "DECIDED". Unchanged since 3674847 (the repository history of the line).
  - DR-080 (line 91): W-13, the procedure D-012 ran under; status "DECIDED (SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026; it governed D-011 and D-012)".
  - DR-078 (line 89): W-12, under which D-012 started; status "DECIDED (its automatic move to the next topic, the sequence beyond D-012 and the ChatGPT audit only after publication SUPERSEDED by DR-082 from 22:14 IST on 30-Oct-2026)".
  - DR-079 (line 90): D-011, whose RQ11-04 is D-012's starting definition; cited here only as D-012's records cite it.
  - Later rows naming D-012: DR-082 (line 93; W-14), which "supersedes DR-080 (W-13), which governed D-011 and D-012" and "also carries the records owed by the D-012 package: its post-push result file (docs/inputs/chatgpt_originals_and_replies/) and its push line (V2_DESIGN_LOG section 6)"; DR-083 (line 94): "D-010 to D-012 hold independent review records, which, like the requirements files, do not by themselves satisfy this decision (point A)"; DR-084 (line 95): "Not in scope: D-011 and D-012 (BK-85 stays OPEN for them and for the re-audit and its scope)".
  - Earlier rows D-012's records cite: DR-008, DR-031 and DR-072 to DR-080 (the stage record, section 1; the requirements record, sections 5 and 10).
- V2_DESIGN_LOG.md, at 42e6533 (the commit that last changed each line, from the repository history):
  - section 1, lessons dated 2026-10-30 that name D-012: line 190 (d665719, D-011's lesson, which D-012's F-01 repeated), line 191 (3674847, D-012's own) and lines 192 to 194 (6bd80a5, the topic-workflow package);
  - section 3, VP-08's D-012 tracking note (line 261; 3674847);
  - section 4, the D-012 row (line 376; 3674847) and the D-013 row (line 377);
  - section 5, W-12 (line 393) and W-13 (line 394), each with its "Update 2026-10-30 (DR-082)", and W-14 (line 395);
  - section 6, under "### 2026-10-30": the D-012 session note (line 617; 3674847) and the D-012 push line (line 618; 6bd80a5);
  - section 7, BK-80 with its D-012 note (line 709), BK-83 closed by D-012's note (line 712), BK-84 (line 713) and BK-85 (line 714);
  - section 8, rows 8 and 21 (lines 729 and 742): "rows 8 and 21 the evidence trail; no new row" (the section 4 row).
- The post-push record (section 2.2) gives the commit, the push and the fresh-clone check. Its push line (section 5) is byte-equal to V2_DESIGN_LOG line 618 (a check made for this manual at 42e6533).

### 2.4 Which record governs what

| Matter | Governing record | Note |
|---|---|---|
| D-012's requirements and decisions | D-012_v2_requirements.md (section 2.1) | it quotes the decided wording "exactly from the stage record's transcription (section 5.2)" |
| D-012's starting definition | D-011's RQ11-04 (DR-079), which governs (Rama's choice at about 20:27) | RQ12-02's "Freshness describes [...]" sentence is "a restatement" of it |
| Claude's questions, the 19:53 answers and the follow-up, word for word | the stage record, sections 5.1 to 5.3 | the screenshot is not stored (DR-008) |
| The open questions OQ12-01 to OQ12-13, their wording and holders | the stage record, section 6 | the requirements record, section 8: "none is closed here" |
| Evidence and locators | the stage record, sections 2 to 4 | "NO NEW READ OF A FROZEN-MANUAL PASSAGE; ROWS REUSED AS CITED" (section 3's heading) |
| D-012's status in V2 | DR-081; V2_DESIGN_LOG section 4, line 376 | |
| The procedure it ran under | DR-080 (W-13), as DR-082 (W-14) later superseded it | W-13 "governed D-011 and D-012" |
| The pre-push review | D-012_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | |
| What was stored, with hashes | the storage manifest; the post-push record | |
| Legacy behaviour not researched | V2_DESIGN_LOG section 8 ("The one canonical list of legacy behaviours that a Route B topic needed and that no frozen manual establishes") | rows 8 and 21 |
| Cross-topic points | V2_DESIGN_LOG section 7 | BK-80, BK-83, BK-84, BK-85 |
| Whether D-012 is complete under DR-083 | BK-85 (OPEN) | DR-082; ChatGPT's pre-push review of 31-Oct, point D |

## 3. Evidence labels

| Label | Meaning in this manual |
|---|---|
| FACT (V2 DECISION) | a register row and what it records, cited by ID, with the status in its row |
| FACT (V2 RECORD) | text or a status in a stored V2 record (a topic file, the design log, a file in docs/inputs/), cited by file and section or line |
| FACT (HISTORICAL) | a frozen manual's statement with its qualifiers, cited by manual, section and line as D-012's stage record cites it (E12-05, E12-06). No frozen manual was re-read for this manual. It is never evidence of what a host ran. |
| RAMA (STATED) | Rama's own words, quoted exactly from the record named |
| RAMA-AUTHORISED, DRAFTED BY CHATGPT | wording that ChatGPT drafted at Rama's request or that Rama issued as his, where a record attributes it so: here the answers relayed at 19:53, which the stage record labels so as INTERPRETATION (Claude), "although his request to ChatGPT is not shown" (section 5.2) |
| INTERPRETATION | a reading, named with whose it is (Claude's, in the D-012 records; this manual's own, where marked); never a decision |
| PROVISIONAL | the status of a raw input in the record's own words: here "RAW / PROVISIONAL USER INPUT" (the requirements record, section 2.2) |
| UNKNOWN / NOT ESTABLISHED | not shown by the records |
| NOT RESEARCHED | a legacy behaviour that no frozen manual establishes (W-11; V2_DESIGN_LOG section 8) |
| KNOWN DEFECT | a defect the records name, with its correction or its handling as recorded |
| OPEN QUESTION | a question the records keep open, with its ID, status and where it waits |

- The D-012 records also use labels of their own: "DECISION (RAMA)", "PROPOSAL", "OPEN QUESTION / DEPENDENCY" and "RAW / PROVISIONAL USER INPUT". Where this manual gives them, they are quoted as the record's words; they are not labels of this manual. No label is merged with another.
- Quotes are exact. A quote that runs over several source lines joins them with one space (the D-012 records: "in prose a line break inside a quote is added for width only"); "[...]" marks words left out. As in the D-012 records, check marks are written [x] and the long dash --; in quotes from ChatGPT's review of 01-Nov-2026 a long dash is written -- and curly quotation marks straight.

## 4. The question the topic answers

- FACT (V2 RECORD): the Master Agenda lists the topic in Part 5 (line 222), Section D, "D. TRADE DECISION ENGINE" (line 289), at line 303: "D-012  S&R freshness". The stage record (E12-01): "No other D-012 scope text."
- The Section D goal (lines 311 to 317): "Should this signal become a trade?"; "This section is especially important for the known redesign concerns: late entry, unsuitable filtration, incorrect S&R range, and trading near high-of-day without adequate S&R context." The D-012 records quote the goal (E12-01, lines 311 to 313) and record no problem for freshness: "Current problem | none recorded for freshness" (the requirements record, section 7) and, under the Master Agenda's Part 3, "8. Problems / limitations | none located" (section 4).
- The same name is used again in Section J ("J. S&R / ADVANCED ENGINES"): "J-005  S&R freshness" (line 441), under J's rule that asks for each advanced mechanism KEEP, MODIFY, REPLACE, REMOVE or SHADOW and ends "Presence of code does not automatically mean the mechanism belongs in the new live decision path." (lines 451 to 460). The records keep the two apart: "The legacy mechanisms' disposition is J-005's." (the requirements record, section 1); how D-012 and J-005 are separated is OQ12-02, OPEN (section 7.1).
- What already pointed to D-012 when it started (FACT (V2 RECORD) and FACT (V2 DECISION), as the stage record gives them):
  - D-011's RQ11-04 (DR-079), with "Recency is separate from D-011 strength.", "D-012 owns the freshness assessment." and "D-012 must determine its remaining requirements, evidence, methodology and treatment independently." ("Rama's follow-up of 19:35, ChatGPT's wording"); BK-83 (E12-02);
  - D-009's provisional boundary, which sends "freshness to D-012" (E10-03, ChatGPT's proposal), and OQ9-14's note, "zones, strength and freshness themselves are D-010 to D-012's" (E10-06) (E12-03);
  - D-010's OQ10-07 ("OPEN; freshness with D-012"), Claude's proposal P10-03 ("age or freshness to D-012") and BK-80 ("OPEN; D-011's note: recency kept with D-012") (E12-03);
  - ChatGPT's list in D-010's answer sheet, Q7, with "last reaction date/time;" and "age/freshness indicator (owned by D-012, not decided here);" (E12-04);
  - the owner ranges that cover D-012 (L12-03; the stage record, section 4), among them "D-010 to D-012" (13 occurrences): "the boundary that sends freshness to D-012 (E12-03)".
- The question D-012 was given (FACT (V2 RECORD), the stage record, section 1): "what V2 requires of an S&R reference's freshness, at requirement level, starting from the definition decided at D-011: "D-012 Freshness: How recent or stale the historical price behaviour supporting an S&R reference is." (RQ11-04, DR-079)." Claude put it to Rama as four questions (the stage record, section 5.1; this manual, section 5.3): Q1 "Freshness" (show freshness for each reference?), Q2 "Expiry" (does an old reference stop being one?), Q3 "Evidence" (which kinds may freshness rest on?) and Q4 "Display" (how is it shown?).

## 5. Course of the topic

| Time (IST) | Step | Who | Record |
|---|---|---|---|
| 19:42 | the D-011 package committed (d665719, 7 files; commit time), the base for D-012 | Claude | V2_DESIGN_LOG section 6, line 616 |
| 19:43 | "D-011 published (d665719); D-012 authorised to start (W-12)" | Claude | the requirements record, section 12 |
| from 19:45 | the stage record prepared ("Prepared 30-Oct-2026 (IST), from 19:45, by Claude.") | Claude | the stage record, header |
| 19:45 to 19:52 | "the plan and locators L12-01 to L12-03" | Claude | the requirements record, section 12; V2_DESIGN_LOG line 617 |
| about 19:50 | the decision request: four questions in the question tool, Claude's wording | Claude | the stage record, section 5.1 |
| 19:53 | Rama's answers: a screenshot of ChatGPT's reply, relayed by Rama | Rama; ChatGPT | the stage record, section 5.2 |
| 19:55 to 20:00 | "the stage record, this record and the governance edits; a local commit (0a13b9b, never pushed)" | Claude | the requirements record, section 12; the post-push record, section 1 ("About 20:00 IST") |
| from about 20:00 | the independent review (W-12): "PASS WITH FINDINGS, F-01 to F-11"; "The container restarted while the report was being delivered" | a separate Claude subagent | D-012_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt, sections 1 to 3; the verdict as quoted, the requirements record, section 12 |
| about 20:27 | Claude's two follow-up questions and Rama's choices in the tool | Claude; Rama | the stage record, section 5.3 |
| from 20:28 | "the revision and the storage action (DR-081)"; the range and term locators L12-03 and L12-04 | Claude | the requirements record, section 12; V2_DESIGN_LOG line 617 |
| 20:32:02 | commit 3674847 (0a13b9b amended; commit time) | Claude | the post-push record, section 2 |
| 20:32:15 to 20:32:19 | origin/main re-fetched; the push to HEAD:refs/heads/main (d665719..3674847); local HEAD equal to origin/main | Claude | the post-push record, section 2 |
| 20:32:20 to 20:32:21 | the fresh-clone check | Claude | the post-push record, section 3 |

- The stage record's section 2 heading reads "LOCATOR SEARCHES (19:45 TO 19:52 IST)" over all four locators; the review record (F-07: "L12-03's exact command and its counts recorded [...]; L12-04, a term search over the V2 records, added") and the design log (line 617: "the revision, the range and term locators (L12-03, L12-04)") place L12-04, and L12-03's recorded form, in the revision after the review.

### 5.1 The plan and the locators (19:45 to 19:52)

- Sources (the stage record, section 1): "P1 Rama's statements and choices (the D-010 answer sheet through the register's RI rows; his choices in section 5); P2 the 39 frozen manuals, by term search and matched lines with their headings, cited as HISTORICAL; P3 the V2 records; P4 the wording of his answers, drafted by ChatGPT. P5: not needed."
- "Not used: any legacy repository, clone, saved listing, host, VM, runtime, broker or database (DR-073); docs/inputs/legacy_baseline/; the web; no image other than Rama's screenshot of 19:53 IST."
- Tools: "the file-search tool for L12-01 and L12-02; git grep (read-only) to reproduce L12-01's line count and for L12-03 and L12-04; git for the storage step. No shell print of a frozen-manual passage."
- The locators (the stage record, section 2):
  - L12-01, over the 39 frozen manuals, for D-012, J-005, "S&R freshness", freshness, recency, the two detector field names, decay and three zone phrases: "50 matches on 38 lines of 12 manuals". "No manual names J-005; "D-012" occurs only in the owner ranges of D-004 lines 50 and 64 and D-005 lines 15 and 158".
  - L12-02, the lines that also name S&R, a zone, support, resistance or sr_: "5 lines: A-009 137 (recency_window_days, recency_min_factor), D-005 15 (owners), D-004 285 (the S&R detector's data cache), B-003 106 (a list of table names), D-001 214 (signal freshness, "support" in another sense)".
  - L12-03, the owner ranges that cover D-012, over docs/agenda and docs/governance with docs/inputs excluded (its command is recorded); L12-04, a term count over named V2 records: "187 matching lines in 33 files, nearly all in the D-009 to D-011 records".
  - Set aside, as INTERPRETATION from the matched lines: "every other "freshness" hit is data, signal or health freshness [...]: none is the freshness of an S&R reference."

### 5.2 The evidence (E12-01 to E12-07)

- The stage record's section 3 heading: "EVIDENCE (NO NEW READ OF A FROZEN-MANUAL PASSAGE; ROWS REUSED AS CITED)". E12-01 is the Master Agenda (section 4 above); E12-02 and E12-03 are the V2 decisions and records that pointed to D-012 (section 4); E12-04 is Rama's D-010 words (section 7.2); E12-05 to E12-07 are the legacy rows (section 8).

### 5.3 The decision request and the answers (about 19:50 to 19:53)

- Claude's four questions (the stage record, section 5.1, "Claude's wording, exact"; "The "(Recommended)" marks are PROPOSAL (Claude)."):
  - Q1 "Freshness": options "Yes, as information (Recommended)", "No freshness shown" and "Decide later in design".
  - Q2 "Expiry": options "No automatic expiry (Recommended)", "Old references expire" and "Decide later in design". It quoted Rama's D-010 Q9 words, "fixed/unchangeable"; the stage record's note: "[The question cited Rama's D-010 Q9 words, which answered whether zones are common or strategy-specific (E12-04); using them for expiry was Claude's reading.]"
  - Q3 "Evidence" ("more than one may be ticked"): "Time since last reaction", "Candles since last reaction", "Crossed since last reaction" and "Recent re-tests". "Q3 offered no "decide later" or "none" option (Claude's question)." (section 5.2).
  - Q4 "Display": options "Evidence plus a label (Recommended)", "Evidence only", "Label only" and "Decide later in design".
- The answers (section 5.2): "The tool returned "[No preference]" for Q1 to Q3 and "Answers" for Q4. At 19:53 IST Rama sent a screenshot (not stored, DR-008) of ChatGPT's reply; his own message to ChatGPT is not in it." INTERPRETATION (Claude): ""Answers" points to that screenshot, and the reply covers all four questions." ChatGPT's "Final selection summary": Q1 "Yes, as information"; Q2 "No automatic expiry"; Q3 "All four evidence categories"; Q4 "Evidence plus a label".
- Compared with the options, "none resolved silently" (section 5.2): no answer reverses its option; two differences (Q1's and Q2's limiting clauses, and Q2's second wording of the definition) went to Rama after the review (section 5.4); the rest became open questions (OQ12-03, OQ12-04) or are noted as they are.

### 5.4 The independent review and the follow-up (from about 20:00 to about 20:27)

- The review's F-01 (MAJOR): "two answer clauses that limit later topics [...] recorded as decided without the question the D-011 lesson requires"; its F-10 (NOTE) named, among other open points, "two wordings of the definition". Claude put both to Rama as two questions, "Use limits" and "Definition" (the stage record, section 5.3, "Claude's two questions, exact").
- "Rama's answers, selected by him in the tool (DECISION (RAMA); the option wording is Claude's): "Input to those topics (Recommended)" and "D-011's definition (Recommended)"." Their effect is in section 6.1. D-012_review_notes.md, section 2, gives the review's findings and resolutions.

### 5.5 The revision and the storage (from 20:28 to 20:32)

- The findings were resolved in one revision; "DR-081 was revised before its first publication; no published register row was edited. The commit 0a13b9b is amended locally before the push; it was never pushed." (the review record, section 2). The storage manifest's checks (section 3) passed (the post-push record, section 1). The commit, push and fresh-clone check are in D-012_review_notes.md, section 6.

### 5.6 After publication (later records; not D-012 steps)

- 20:32 to 20:42: "Claude wrote D-012's post-push record, told Rama that D-013 was next, and began D-013's read-only searches (no question, no file, no repository change)"; at 20:42, Rama (RAMA (STATED), as transcribed): "Stop here and please answer me that D-012 is completed?? Are you currently working on D-013 if yes wait" (TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt, section 1).
- 21:48: Topic_Workflow.txt with three screenshots that include ChatGPT's plan for "a second, deeper audit" of D-010 to D-012 (V2_DESIGN_LOG line 619). 22:14: ChatGPT's decisions relayed by Rama: DR-082 (W-14), which supersedes DR-080 (W-13), and DR-083, with BK-85.
- 31-Oct-2026, 10:10: the topic-workflow package, 6bd80a5 (commit time), pushed after ChatGPT's "OK TO PUSH -- FOR THIS EXACT COMMIT ONLY." (received 10:23); it stored D-012's post-push record and push line (V2_DESIGN_LOG line 623: "the D-012 records stored").
- 31-Oct to 01-Nov: the D-006 to D-010 documentation batch (DR-084), with "Not in scope: D-011 and D-012"; its commit 42e6533 is this manual's base.
- 01-Nov-2026: ChatGPT's review, R-03, sets this documentation-only work (section 1).

## 6. What is recorded as decided

### 6.1 D-012's requirements (FACT (V2 DECISION), DR-081)

- DR-081: "DECIDED at requirement level only, on the starting definition decided at D-011, which governs (RQ11-04, DR-079: "D-012 Freshness: How recent or stale the historical price behaviour supporting an S&R reference is."), by Rama's choices of 30-Oct-2026". The requirements record, section 2.1, governs; "The wording below is quoted exactly from the stage record's transcription (section 5.2); in RQ12-03 the check marks are written [x] and the long dash --." The short forms in the table are the design log's own (section 4, line 376).

| ID | In short (V2_DESIGN_LOG section 4) | Status label | Source |
|---|---|---|---|
| RQ12-01 | "freshness shown with every reference as descriptive information, its use decided by later topics" | DECIDED at requirement level | the requirements record, 2.1; the stage record, 5.2 (Q1) and 5.3 ("Use limits") |
| RQ12-02 | "no automatic expiry of an old reference" | DECIDED at requirement level | 2.1; 5.2 (Q2) and 5.3 ("Use limits", "Definition") |
| RQ12-03 | "the evidence kinds time since the last reaction, candles since the last reaction, crossed since the last reaction and recent re-tests" | DECIDED at requirement level | 2.1; 5.2 (Q3) |
| RQ12-04 | "the evidence shown with a label, Fresh, Aging or Stale" | DECIDED at requirement level | 2.1; 5.2 (Q4) |

The decided wording, exact (the requirements record, section 2.1), with the option chosen:
- RQ12-01, decided: "Each S&R reference should display freshness information alongside its reference details." and that freshness is descriptive information ("Freshness is descriptive information, [...] Later topics determine how they use it."). The record continues: "An input to the later topics, not decided here (Rama, 20:27): "not an automatic Trade/Reject, entry, SL or target decision"". Options chosen: "Yes, as information (Recommended)"; at 20:27, "Input to those topics (Recommended)".
- RQ12-02, decided: "An old historical S&R reference does not automatically cease to exist merely because time has passed." "An older reference may still be historically relevant." "Historical lookback periods remain subject to D-007 and data-related topics." The record continues: "A restatement of D-011's definition, which governs (Rama, 20:27): "Freshness describes how recent or stale its supporting historical price behaviour is." An input to the later topics and the design, not decided here (Rama, 20:27): "Whether a reference should be excluded from use is a separate decision, not an automatic consequence of age."" Options chosen: "No automatic expiry (Recommended)"; at 20:27, "Input to those topics (Recommended)" and "D-011's definition (Recommended)".
- RQ12-03: "[x] Time since last reaction -- Calendar time elapsed since the last reaction." "[x] Candles since last reaction -- Number of candles elapsed. Actual timeframe selection remains D-007-dependent." "[x] Crossed since last reaction -- Whether price has subsequently crossed through the reference." "[x] Recent re-tests -- Whether price has returned to the reference recently." "These are evidence categories only. Measurement, weighting and interpretation remain undecided." Option chosen: "all four kinds".
- RQ12-04: "Display supporting evidence alongside a descriptive label:" "Fresh" "Aging" "Stale" "The classification methodology, thresholds and calculation remain open for later design." Option chosen: "Evidence plus a label (Recommended)".

Provenance, as the records give it:
- FACT (V2 RECORD): "The option texts are Claude's wording, and its "(Recommended)" marks were PROPOSAL (Claude); the selections are Rama's, through ChatGPT's drafted answers." (the requirements record, 2.1).
- The 19:53 answers: "Provenance: Rama's choices, relayed by him; the wording is ChatGPT's. INTERPRETATION (Claude): RAMA-AUTHORISED, DRAFTED BY CHATGPT, as at D-011, although his request to ChatGPT is not shown. None of it is presented as verbatim Rama wording." (the stage record, 5.2). The record's own heading for 2.1 is "DECISION (RAMA); RAMA-AUTHORISED, DRAFTED BY CHATGPT".
- The 20:27 choices: made by Rama in the tool, on Claude's option wording ("DECISION (RAMA); the option wording is Claude's"; the stage record, 5.3).
- The follow-up's effect (the stage record, 5.3): the two clauses "are inputs to D-009, D-013 to D-018, E-005 to E-007 and the design, not decided limits; D-011's definition governs, and RQ12-02's "Freshness describes how recent or stale its supporting historical price behaviour is." is a restatement of it."

What each requirement does not decide (the requirements record, section 2.1, last column, exact):
- RQ12-01: "how later topics use freshness (OQ12-01; BK-84)".
- RQ12-02: "who decides any exclusion from use, and on what basis (OQ12-03); the lookback (OQ12-07) and its effect on old references (OQ12-12)".
- RQ12-03: "how each is measured (OQ12-05); the timeframe of a candle count: OPEN / UNRESOLVED on D-007 (OQ12-06); how crossing relates to role changes (OQ12-11); whether the four are the only kinds (OQ12-13)".
- RQ12-04: "whether the label set is fixed (the option said "such as"; OQ12-04); how a label is set".

The answers compared with Claude's options (the stage record, 5.2; "none resolved silently"):
- Q1: "the option said "with no trading authority" and "as for strength"; the answer says freshness is "not an automatic Trade/Reject, entry, SL or target decision" and "Later topics determine how they use it", with the boundary's "not [...] trading-authority rule"; "as for strength" is not in it. The limiting clause went to Rama (section 5.3, Q1)."
- Q2: ""An old reference stays a reference" became "does not automatically cease to exist merely because time has passed"; "freshness only describes" became "Freshness describes" (no "only"), a second wording of the definition (section 5.3, Q2); "An older reference may still be historically relevant." is added (permissive); "(your 3 and 5 years)" is not in it; "Whether a reference should be excluded from use is a separate decision, not an automatic consequence of age." is added (section 5.3, Q1; OQ12-03)."
- Q3: "the answer adds "weighting and interpretation" to what stays undecided, beyond the question's "how each is measured"."
- Q4: "the option said "a simple label such as fresh / aging / stale"; the answer lists "Fresh" "Aging" "Stale" after "a descriptive label:", without "such as"; whether the set is fixed is OPEN (OQ12-04)."

How the requirements relate (the requirements record, section 3, headed "INTERPRETATION (Claude)"):
- "RQ12-01 says each reference "should display" freshness; for strength, whether every reference must show it is OPEN (D-011 OQ11-03). Freshness's use is the later topics' (OQ12-01; BK-84)."
- "RQ12-03 and RQ12-04 make freshness explainable, as RQ11-02 and RQ11-03 do for strength. A crossing through a reference (freshness evidence) and a role switch (strength evidence, RQ11-02) may describe the same price events; how they relate is OPEN (OQ12-11), and D-010's RQ10-04 bears on it."
- "No requirement needs a timeframe or a formation decision, so DR-074's stop rule is not triggered; the candle-count kind depends on D-007 (OQ12-06), and any formation-based reaction waits on D-006 (OQ12-05)."

Legacy: "RQ12-01 to RQ12-04 are new V2 requirements, not carried from legacy." (the requirements record, section 4).

### 6.2 Other items recorded with D-012's package (not D-012 requirements)

| Item | What was recorded | Status label, as recorded | Source |
|---|---|---|---|
| The follow-up choices | the answers' limiting clauses are inputs, not decided limits; D-011's definition governs | "DECISION (RAMA)" in the record's terms | the stage record, 5.3; DR-081 |
| OQ12-08 | VP-08 at D-012 | "answered for D-012: not ratified; VP-08 stays PROPOSED for its later ratification points" | the stage record, section 6 |
| VP-08's D-012 note | "D-012 (30-Oct-2026; tracking only): not ratified at D-012." | "The Ratify-at cell is unchanged." | V2_DESIGN_LOG line 261 |
| BK-83 | D-011's bookmark for D-012's starting definition | "CLOSED (D-012, DR-081)" | V2_DESIGN_LOG line 712 |
| BK-84 | how freshness is used, and who decides any exclusion from use | "OPEN" | line 713 |
| BK-80's D-012 note | RQ12-03 against D-010's OQ10-07 and P10-03 | "OPEN" ("stays OPEN for the whole-design review") | line 709 |
| One lesson | line 191 | "Historical process lesson (not a system requirement)" | DR-081 ("One historical process lesson") |

- No working rule was set at D-012: it started under W-12 (DR-078) and ran under W-13 (DR-080), both decided earlier (at D-010 and D-011).

## 7. What is not decided: open questions, unknowns, dependencies

### 7.1 D-012's open questions (the stage record, section 6; the requirements record, section 8: "none is closed here (OQ12-08 is answered for D-012 only: not ratified)")

| ID | Question (the stage record, exact) | Status (DR-081) | Holder (the stage record, exact) |
|---|---|---|---|
| OQ12-01 | How do later topics use freshness ("Later topics determine how they use it.")? The "not an automatic Trade/Reject, entry, SL or target decision" clause is their input (section 5.3) | OPEN | D-009 (OQ9-01, OQ9-03, OQ9-13 to OQ9-17), D-013 to D-018, E-005 to E-007; Rama; BK-84 |
| OQ12-02 | How are D-012 and J-005 (any legacy freshness or recency mechanism) separated? | OPEN | governance; J-005 holds the disposition; no legacy research under D-012 |
| OQ12-03 | Who decides whether a reference is excluded from use, and on what basis? The answer's clause ("a separate decision, not an automatic consequence of age") is an input to them (section 5.3) | OPEN | the later topics and the design; Rama; BK-84 |
| OQ12-04 | Is the label set fixed as Fresh, Aging, Stale, or an example? How is a label set? | OPEN | design, then Rama |
| OQ12-05 | What counts as a reaction, as crossing through a reference and as "recently" (D-010 UK-03; D-011 OQ11-05) | OPEN | design; any formation-based reaction waits on D-006 |
| OQ12-06 | Which timeframe counts candles since the last reaction? | OPEN | OPEN / UNRESOLVED on D-007 |
| OQ12-07 | How far back history is read (Rama's 3 and 5 years, RI-15; D-010 UK-08) | OPEN | D-007; K-009, K-010 |
| OQ12-08 | Is any part of VP-08 ratified at D-012? | answered: not ratified | answered for D-012: not ratified; VP-08 stays PROPOSED for its later ratification points |
| OQ12-09 | Is freshness recorded with each signal for later review? | OPEN | D-009 OQ9-22; K-008, K-011 |
| OQ12-10 | How is freshness shown when a stock's history is incomplete (D-010 P10-05)? | OPEN | design; D-009 OQ9-21 for any signal effect |
| OQ12-11 | How does "Crossed since last reaction" (freshness) relate to D-010's role change (RQ10-04) and D-011's "Role switches" (strength evidence)? | OPEN | design |
| OQ12-12 | How does "no automatic expiry" (RQ12-02) work with a finite lookback (OQ12-07), which leaves older references out of view? | OPEN | D-007; K-009, K-010; design |
| OQ12-13 | Are the four evidence kinds of RQ12-03 the only ones, or the eligible ones so far? | OPEN | design, then Rama |

- OQ12-12 and OQ12-13 were added on the independent review's F-10 (the review record, section 2).

### 7.2 Rama's own words behind the decisions (RAMA (STATED); "RAW / PROVISIONAL USER INPUT"; the requirements record, section 2.2)

| Source (D-010's answer sheet, through the register) | Rama's words, as the stage record quotes them (E12-04) | How D-012's records use them |
|---|---|---|
| Q9 (RI-12, line 118), an answer to "SHOULD ZONES BE COMMON OR STRATEGY-SPECIFIC?" | "I Guess historical S&R points (ie, where price is settles once certain price is broken) is fixed/unchangeable based on which only traders puts their TGT & SL prices, [...] So make it as 'COMMON' [...]" | INTERPRETATION (Claude): "it bears on no automatic expiry (RQ12-02); Claude's Q2 cited it (the stage record, section 5.1)" |
| Q12 (RI-15, line 141) | "S&R points is to be checked minimum 3 years for Intray and 5 years for Delivery at the same time we need to discuss further like if merger/demerger/unlisted/debut recently), so take note cautiously" | "the lookback, OQ12-07; raw figures that depend on D-007, D-010 UK-08" |
| Q7 (RI-10, line 99), to ChatGPT's list with "last reaction date/time;" and "age/freshness indicator (owned by D-012, not decided here);" | "Time being add your best opinion and mark with needs to be discussed later in new system design" | listed in the requirements record, section 2.2; the list is ChatGPT's (E12-04: "Q7's list (ChatGPT's)") |

- The independent review's F-03 found the Q9 words "used outside their context (commonality) and mapped to expiry without a label"; the mapping is now labelled INTERPRETATION and "Claude's Q2 citation disclosed" (the review record, section 2).

### 7.3 Unknowns and what the records could not establish

- The 19:53 exchange: the reviewer "Could not verify: the question tool's exchange and times; whether the screenshot shows ChatGPT's whole reply, and Rama's request to ChatGPT" (the review record, section 1). UNKNOWN / NOT ESTABLISHED: Rama's request to ChatGPT ("his own message to ChatGPT is not in it", the stage record, 5.2).
- The legacy recency fields: "Names only; what they did is UNKNOWN / NOT ESTABLISHED (NR9-14)"; what any freshness or recency computation used, did, produced or stored: NOT RESEARCHED (NR9-01) (section 8).
- E12-07 is a bounded negative over the matched lines only: "a passage without the terms may still matter."

### 7.4 Dependencies (the requirements record, sections 7 and 10)

| Item | Topic | Status, as recorded | Relation to D-012 |
|---|---|---|---|
| Use of freshness, and any exclusion from use | D-009; D-013 to D-018; E-005 to E-007 | "DRAFT RECORDED (DR-076); not started" | OQ12-01, OQ12-03; BK-84 |
| Timeframes | D-007 | "BLOCKED / NOT CLOSED (DR-073)" | OQ12-06, OQ12-07 |
| Formations | D-006 | "DEFERRED / NOT CLOSED (DR-072)" | OQ12-05 |
| Candle and market data; lookback | K-009, K-010 | "not started" | OQ12-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | "OPEN; not started" | OQ12-09 |
| Legacy freshness or recency mechanisms | J-005 | "not started (Route B topic)" | NR9-01, NR9-14 |

- The decision record's "Dependencies" field (the requirements record, section 7): "D-009 OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22, all OPEN (OQ9-01 to OQ9-25 stay OPEN); D-013 to D-018 and E-005 to E-007 (freshness's use); D-007 BLOCKED / NOT CLOSED; D-006 DEFERRED / NOT CLOSED".

### 7.5 D-009's questions that stay OPEN (D-009's record is not edited)

- "D-009's OQ9-01, OQ9-03, OQ9-13 to OQ9-17: freshness's use is theirs, with the answers' clauses as inputs; all stay OPEN, with OQ9-01 to OQ9-25." (the requirements record, section 9, INTERPRETATION (Claude)). OQ9-21 (signal effect of incomplete history; OQ12-10) and OQ9-22 (recording per signal; OQ12-09) are named as holders in section 7.1.

## 8. Legacy evidence

- Route B (DR-074, W-11) and option C (DR-073) applied: "No legacy source or legacy repository was read, and no frozen-manual passage was newly read." (the requirements record, header). The stage record lists docs/inputs/legacy_baseline/ among the sources not used (section 1). This manual adds no read of a legacy source or of a frozen manual; frozen-manual content reaches it only through D-012's stage record.
- What the stage record holds (by the Master Agenda's Part 3 items, as the requirements record, section 4, sets them out):

| Part 3 item | What the records hold, with its label and qualifiers | Evidence |
|---|---|---|
| 1. Current system | FACT (HISTORICAL) for the cells, INTERPRETATION for their reading as owners: "recorded owners only: "D-009 to D-012 S&R" for "levels and zones"" (D-004 1.3, line 50; "E10-19 calls reading the first cell as the owner an INTERPRETATION"); "S&R, zones, strength and freshness (D-009 to D-012);" (D-005 1, line 15; "its list lead-in was not read, so whether the list states D-005's scope or what it leaves to other topics is not established"). The detector is "the shadow S&R detector, enabled in production" (A-009 8, line 137), "kept as recorded (W9-01)" | E12-05, E12-06 |
| 2. Code / modules | FACT (HISTORICAL), heading "FACT, SOURCE-VERIFIED unless marked": "names only: the detector's defaulted fields recency_window_days and recency_min_factor" (A-009 8, line 137; D-011's E11-10) | E12-06 |
| 3 to 6. Inputs, process, output, state change | "what any freshness or recency computation used, did, produced or stored": NOT RESEARCHED (NR9-01); "the fields' effect UNKNOWN / NOT ESTABLISHED (NR9-14)" | - |
| 7. Dependencies | "none located" | - |
| 8. Problems / limitations | "none located" | - |

- E12-07, INTERPRETATION (Claude), "a bounded negative over L12-01 and L12-02": "in the 39 frozen manuals, no matched line describes a freshness mechanism for S&R beyond the two field names of E12-06; a passage without the terms may still matter."
- NOT RESEARCHED and UNKNOWN gaps (V2_DESIGN_LOG section 8; each "an evidence gap, not a finding"; each OPEN; raised at D-009, 30-Oct), which D-012's design-log row names as "rows 8 and 21 the evidence trail; no new row":
  - row 8, NR9-01 (line 729): "what the S&R detector computed (levels or zones, method, use of timeframes and of its 11 defaulted fields)"; NOT RESEARCHED;
  - row 21, NR9-14 (line 742): "what the 11 defaulted detector fields did, and whether any default changed a trade ("UNKNOWN", A-009_manual.md section 8)"; UNKNOWN / NOT ESTABLISHED.
- No new gap row: "Legacy gaps: none new; NR9-01 and NR9-14 (rows 8 and 21) already cover the detector's recency fields (E12-07)." (the stage record, section 6).
- Disposition (the requirements record, sections 4 and 6): "KEEP and MODIFY are not available (W-11)."; "Any legacy freshness or recency mechanism: no outcome at D-012; disposition with J-005 (DEFER, in Part 7's terms)."; "SHADOW not used."
- Other matched lines set aside (the stage record, section 2, INTERPRETATION from the matched lines): D-004 line 285's cache "is a data freshness item (K-009, K-010); B-003 line 106 lists table names only."

## 9. Cross-topic boundaries, dependencies and bookmarks

### 9.1 Chronology of the S&R topics (FACT (V2 RECORD); commit times from V2_DESIGN_LOG section 6 and the repository history)

| Topic | Recorded status | Commit (time) | Relation to D-012 |
|---|---|---|---|
| D-009 Support & Resistance | DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076); OQ9-01 to OQ9-25 OPEN | 9c9c2e9 (10:06, 30-Oct) | earlier: its provisional boundary sends "freshness to D-012" (E12-03); its OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22 hold freshness's use and stay OPEN |
| D-010 S&R zones | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077) | 7c725ed (18:37, 30-Oct) | earlier: "The references freshness describes": RQ10-01 to RQ10-05 (the requirements record, section 10); OQ10-07, P10-03 and BK-80 |
| D-011 S&R strength | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079) | d665719 (19:42, 30-Oct) | earlier: "Strength, kept apart from recency": RQ11-04, whose definition governs; BK-83; OQ12-11 |
| D-012 S&R freshness | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | 3674847 (20:32, 30-Oct) | this topic |
| Topic workflow | DR-082 (W-14, in effect from 22:14 IST on 30-Oct) and DR-083 (relayed at 22:14 IST) | 6bd80a5 (10:10, 31-Oct) | later: supersedes DR-080 (W-13), which "governed D-011 and D-012"; requires a V2 topic manual and review notes for each Route B topic, whether and how D-012 receives them left to BK-85; stored D-012's post-push record and push line |
| D-006 to D-010 documentation batch | DR-084; DR-085 | 42e6533 (08:54, 01-Nov) | later: "Not in scope: D-011 and D-012"; the base of this manual |
| D-013 Entry zone | NOT STARTED (V2_DESIGN_LOG section 4, line 377) | - | waits for ChatGPT's start package (W-14); "D-013 is not started by this record." (the requirements record, section 11) |

- D-010's and D-011's requirements are their own records' decisions (DR-077, DR-079). D-012 builds on them and does not edit them: "the other records are not edited" (the requirements record, section 9). D-009's draft is not changed by D-012.

### 9.2 Boundaries (the requirements record, section 10; status columns as recorded on 30-Oct and at 42e6533)

| Item | Topic | Status in D-012's record | Status at 42e6533, where it changed | Relation to D-012 |
|---|---|---|---|---|
| The references freshness describes | D-010 | V2 REQUIREMENTS RECORDED (DR-077) | unchanged | RQ10-01 to RQ10-05 |
| Strength, kept apart from recency | D-011 | V2 REQUIREMENTS RECORDED (DR-079) | unchanged | RQ11-04; OQ12-11 |
| Use of freshness, and any exclusion from use | D-009; D-013 to D-018; E-005 to E-007 | DRAFT RECORDED (DR-076); not started | D-009 unchanged; D-013 NOT STARTED, waiting for ChatGPT's start package (W-14) | OQ12-01, OQ12-03; BK-84 |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | unchanged | OQ12-06, OQ12-07 |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | unchanged | OQ12-05 |
| Candle and market data; lookback | K-009, K-010 | not started | - | OQ12-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | OPEN; not started | - | OQ12-09 |
| Legacy freshness or recency mechanisms | J-005 | not started (Route B topic) | - | NR9-01, NR9-14 |

- Other V2 records that touch D-012 (the requirements record, section 5, "FACT, row level"): "DR-078 / W-12: the topic sequence. DR-080 / W-13: the procedure. DR-031: S&R among the owner allocation "C-001, D-003, D-009 to D-017 and J"."; "VP-08 (PROPOSED except its count; Ratify at "Count DECIDED (DR-031); D-003, D-009 to D-017, J"): not ratified at D-012 (OQ12-08)."
- VP-08's D-012 note (V2_DESIGN_LOG section 3, line 261), INTERPRETATION (Claude): RQ12-01 to RQ12-04 "describe freshness as information and, in their boundary's words (ChatGPT's wording of Rama's answers), are "not an algorithm, expiry rule, threshold, timeframe hierarchy or trading-authority rule", so they neither ratify nor reject the sentence "S&R logic is set after strategies and processors, and R:R honours S&R." The Ratify-at cell is unchanged."

### 9.3 Bookmarks (V2_DESIGN_LOG section 7; tracking only; "it closes only by a note in its row")

- BK-80 (D-010; OPEN; line 709): D-010's OQ10-07 and P10-03, which attributes are strength-like or age-like. D-012's note: "RQ12-03 (DR-081; Rama's choices, ChatGPT's wording) records the freshness evidence kinds, among them the time since the last reaction (INTERPRETATION (Claude): this bears on, and does not close, which attributes D-010's references themselves carry, P10-03); stays OPEN for the whole-design review". Cross-check: D-011, D-012.
- BK-83 (D-011; CLOSED; line 712): D-012's starting definition. D-012's note: "reviewed at the topic it names, under the rule at the head of this section; D-012 started from this definition and recorded RQ12-01 to RQ12-04 (DR-081), and D-011's definition governs (Rama's choice at D-012's follow-up); closed by this note". Status "CLOSED (D-012, DR-081)".
- BK-84 (D-012; OPEN; line 713): OQ12-01 and OQ12-03, "how freshness is used, and who decides whether a reference is excluded from use"; the two clauses "are inputs to these topics and the design, not decided limits (Rama's follow-up choice, DR-081); the use and the exclusion are not decided". Cross-check: D-009, D-013 to D-016, D-017, D-018, E-005 to E-007 and the design.
- BK-85 (W-14; OPEN; line 714): whether and how D-008 to D-012 receive a V2 topic manual and review notes "is not decided"; ChatGPT plans "a second, deeper audit" of D-010 to D-012; its note of 2026-10-31 (DR-084): "D-011 and D-012 are not in the batch; ChatGPT's re-audit and its scope are neither started nor decided; stays OPEN." This manual concerns it (section 1).

### 9.4 D-006 and D-007

- D-007 (BLOCKED / NOT CLOSED, DR-073): the candle-count kind of RQ12-03 has its timeframe "OPEN / UNRESOLVED on D-007 (OQ12-06)", and the lookback (OQ12-07) and its effect on old references (OQ12-12) wait on D-007 and the data topics. D-006 (DEFERRED / NOT CLOSED, DR-072): "any formation-based reaction waits on D-006 (OQ12-05)". The record's reading that "DR-074's stop rule is not triggered" is labelled INTERPRETATION (Claude) (the requirements record, section 3; section 6: "D-007 and D-006: see section 3; not triggered.").

### 9.5 Later records that speak of D-012 (not D-012 decisions)

- ChatGPT's pre-push review of the topic-workflow package, 31-Oct (docs/inputs/reconciliation/Topic_Workflow_PrePush_Review.txt), point D: "Preserve the distinction between D-012 being published (DR-081) and whether it is complete under DR-083 (BK-85). Do not mark it fully complete merely because its requirements package was published."
- ChatGPT's pre-push audit of the documentation batch (docs/inputs/reconciliation/D006_D010_ChatGPT_PrePush_Audit.txt): "D-011 and D-012 remain outside this batch."
- ChatGPT's review of 01-Nov-2026 (A.4, "D-011 / D-012"): "Do not reopen existing requirements:" and "D-012 freshness remains descriptive and has no automatic Trade/Reject, entry, SL or target consequence." FACT (V2 RECORD) for the reader: in D-012's records the clause "not an automatic Trade/Reject, entry, SL or target decision" is "An input to the later topics, not decided here (Rama, 20:27)" (the requirements record, RQ12-01's row; DR-081: "Inputs to D-009, D-013 to D-018, E-005 to E-007 and the design, not decided limits (Rama's follow-up choice)"). D-012_v2_requirements.md governs; this manual records both texts and decides nothing between them. ChatGPT's pre-push audit of 01-Nov-2026 (docs/inputs/reconciliation/D011_PrePush_Audit_HOLD.txt, stored unchanged by the settlement package before D-011; not at 42e6533), section 1, corrects that wording: it "was too categorical and must NOT be recorded as a decided D-012 limit", and "D-012_v2_requirements.md remains authoritative and MUST NOT be changed." (a correction to ChatGPT's earlier wording, not a change to D-012).

## 10. Known defects, process deviations and lessons, as recorded

- KNOWN DEFECT, corrected before the push (F-01, MAJOR): "two answer clauses that limit later topics [...] recorded as decided without the question the D-011 lesson requires"; its resolution: "put to Rama with the definition question (F-10) at about 20:27: "Input to those topics (Recommended)". Both clauses are now inputs [...]; a new lesson records the repeat" (the review record, section 2).
- The review's other findings, F-02 to F-08 (MINOR) and F-09 to F-11 (NOTE), were resolved in the same revision before the push (the review record, section 3); D-012_review_notes.md, section 2, gives them. What the reviewer could not verify is in section 7.3.
- KNOWN DEFECT, kept as published: the second independent review of the topic-workflow package (TOPIC_WORKFLOW_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt, G-02, MINOR) found the base wording "equal to the local main after a fetch" inaccurate and added: "The same wording in the published records of D-011 and D-012 (their manifests and post-push records; D-012's is row 5 of this package) is inaccurate in the same way, since the local branch main has stayed at 9c9c2e9 from 10:06 IST on 30-Oct (its reflog); there it stands for origin/main, against which the base was checked (INTERPRETATION (Claude)). They stay as published". In D-012's records: the storage manifest's "equal to the local main after a fetch" and the post-push record's "After a fetch, local main = origin/main = d6657197ed719ed1db8021022d70921f7bd30458" (TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt, section 4: "the record stays unchanged (the second review, G-02)").
- Process (the stage record, section 1): "W-13's step 1 [...] is met by Claude's four questions in the question tool (section 5.1), with the answers recorded in section 5.2; no process departure from W-13." D-012 records no process deviation of its own drafting.
- The independent reviewer's own disclosed deviations (the review record, section 1): "no report under docs/claude_code_reports/ (the task was read-only); file-name listings of docs/inputs/ subfolders and tree-only diffs with docs/inputs as the pathspec, never docs/inputs/legacy_baseline/; line-number and line-length prints of two earlier post-push files."
- Lessons (V2_DESIGN_LOG section 1, all dated 2026-10-30, each "Historical process lesson (not a system requirement)"; the recording commit is from the repository history):
  - line 190 (d665719, D-011's): its first records recorded as decided an answer that limits later topics and "a definition of D-012's subject"; rule: "When an answer limits another topic's use or defines a later topic's subject, Rama is asked whether it is a decided limit or an input to that topic (W-6) before it is recorded.";
  - line 191 (3674847, D-012's own): "D-012's first records again recorded answer clauses that limit later topics as decided [...] although the lesson written at D-011 required asking first"; rule: "When answers arrive, each clause is checked against the lessons' tests (another topic's use; a later topic's subject; a second wording of a decided definition; the options compared) before anything is recorded, and the checks are written into the stage record.";
  - line 192 (6bd80a5): D-010's question "Next topic" and its "(Recommended)" mark; "Claude ran D-011 and D-012 and began D-013's searches", while Rama had meant one topic at a time; rule: "A question that would widen Claude's own authority or autonomy carries no "(Recommended)" mark and states plainly what each option would let Claude do without asking.";
  - line 193 (6bd80a5): "D-011 and D-012 were each decided on four questions, with a follow-up of three (D-011) and two (D-012) after the independent review"; rule: "A topic's questions are set by what the topic needs, never by a tool's limit";
  - line 194 (6bd80a5): D-008 to D-012 stored without the manual and review notes, "and D-011 and D-012 with four files each, against 12 to 15 for D-008 to D-010"; rule: "Before a package is drafted and again before it is published, its folder is compared with the established folder structure and the most recently completed comparable topics (W-14; Topic_Workflow.txt section 5)".
- DR-081 counts "One historical process lesson" (line 191).

## 11. Status

- D-012: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081): RQ12-01 to RQ12-04 DECIDED at requirement level, on D-011's starting definition, which governs; the answers' clauses on trade decisions and on exclusion from use are inputs to the later topics and the design, not decided limits; OQ12-01 to OQ12-13 OPEN (OQ12-08 answered for D-012: not ratified); not frozen. Published (DR-081); whether it is complete under DR-083 is BK-85's (DR-082).
- Unchanged: D-009 DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076), with OQ9-01 to OQ9-25 OPEN; D-010 (DR-077) and D-011 (DR-079) V2 REQUIREMENTS RECORDED; D-006 DEFERRED / NOT CLOSED (DR-072); D-007 BLOCKED / NOT CLOSED (DR-073); VP-08 not ratified at D-012; Route B (DR-074, W-11) and option C (DR-073) in force; section 8 rows 8 and 21 OPEN; BK-80, BK-84 and BK-85 OPEN; BK-83 CLOSED. D-013 NOT STARTED.
- Companion files in docs/agenda/d-012_sr-freshness/: D-012_v2_requirements.md (authoritative) and the three stage, review and storage records of section 2.1; drafted with this manual on 01-Nov-2026: D-012_review_notes.md, and the manual manifest that ChatGPT's review of 01-Nov-2026, R-03, names.
- Nothing here changes a decision or status. This manual adds no requirement, no design, no legacy read and no erratum to any record.
