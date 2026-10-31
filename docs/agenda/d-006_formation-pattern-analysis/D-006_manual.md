# D-006 - FORMATION / PATTERN ANALYSIS (TOPIC MANUAL: DEFERRED / NOT CLOSED)

## 1. Document status, purpose and scope

- Status: DEFERRED / NOT CLOSED (DR-072, 2026-10-28); Phase C NOT RUN; Phase E NOT AUTHORISED; Q-M6 OPEN. D-006 is not frozen, has no legacy manual and makes no V2 decision. The required status wording, verbatim from ChatGPT's bounded storage decision (its section 3) as DR-072 records it: "D-006 Phase C did not execute. The attempt was blocked before implementation; all planned tests are NOT RUN. The proposed design remains unvalidated. The Stage 1 counts remain provisional candidate locators. D-006 is deferred, not closed. Phase E is not authorised. Q-M6 remains unresolved."
- This topic manual was written in the documentation batch of 31-Oct-2026 (DR-084). It documents the topic and its recorded status only. It does not resolve the deferral, design any formation rule, approve any pattern or claim deployed or runtime behaviour.
- Purpose: to explain in one place what D-006 is, what happened to it on 28-Oct-2026, what its stored records establish and what they leave open, why it is deferred, and which later V2 records carry an open dependency on it.
- Scope, as recorded: the Master Agenda's D-006 line, "Formation / pattern analysis" (section 4), the four D-006 records stored on 28-Oct-2026 and their post-push record, and what the decision register, the design log and the later topic records say about D-006.
- Out of scope:
  - any formation or pattern rule, definition, list or approval;
  - any statement of legacy formation or pattern behaviour;
  - any method, tool, test or route for further legacy-source work;
  - the content of the D-006 records that were identified but never stored (section 2.3), beyond what the stored records say of them;
  - D-007's own block, beyond D-006's place in it (docs/agenda/d-007_timeframe-analysis/D-007_manual.md).
- Inputs: the V2 records of section 2 and the frozen D-004 and D-005 manuals at the lines cited, read in the V2 repository at 6bd80a5. Line numbers are those at 6bd80a5; the documentation batch's commit moves V2_DESIGN_LOG lines 195 to 393 down by 2, lines 394 to 619 by 3 and later lines by 5. No legacy repository, clone, host record or saved list was used (option C, DR-073), and nothing under docs/inputs/legacy_baseline/ was used, as Claude reports (PD-01 in the batch decision record).
- Independence: "Statements about Claude's own actions are Claude-reported; ChatGPT has not independently verified them." (D-006_DEFERRAL_STATUS_RECORD_28-OCT-2026.txt, header). The qualifier applies to every process fact that this manual repeats from the D-006 records.

## 2. Records and how to read them

### 2.1 The topic folder (docs/agenda/d-006_formation-pattern-analysis/)

| File | What it is | Status and identity (as recorded) |
|---|---|---|
| D-006_DEFERRAL_STATUS_RECORD_28-OCT-2026.txt | Claude's status record of the deferral: the required status wording and a status table (section 1), the history of 28-Oct (section 2), what the package stores (section 3), the identities of every D-006 record, stored or not (section 4), and the open items carried forward (section 5) | NEW in the deferral package (dd14668); 144 lines, 14,847 bytes, SHA-256 38C08179E775FB9438C6EC9E5C45ED666AB531F03451948D4CF72CCC0E942619 |
| D-006_PHASE_C_STOP_REPORT_28-OCT-2026.txt | Claude's checkpoint report to ChatGPT after Phase C stopped (18:27 IST): the result, the test status, the artefacts (none), the confirmations and the decision requested | CARRIED unchanged as delivered; 51 lines, 2,842 bytes, SHA-256 8C4BB03E0C8E8DFE99F76C7F43B0971DB031350F052145667216AC03C4FA9077 |
| D-006_DEFERRAL_STATUS_NOTE_28-OCT-2026.txt | Claude's planning-only note to ChatGPT after its stop decision: the required status wording, the recommendation, what stays UNKNOWN / NOT ESTABLISHED, what is preserved, the obligations outstanding and the confirmations | CARRIED unchanged as delivered; 31 lines, 1,859 bytes, SHA-256 50ABCA18809D976279C2BC902D1E66C9605906F0E6E7C611EA069440404BE4AA |
| D-006_DEFERRAL_STORAGE_MANIFEST_28-OCT-2026.txt | the deferral package's manifest: the base, the 7 paths, the checks K-1 to K-10 run before the commit (18:58 to 19:00 IST) and what the package does not hold | NEW; 43 lines, 4,402 bytes, SHA-256 AEAFB7DC31D782F3737F9B4C7F878B1FC463F82166218C8040F6DED1A00A85B7 (given in the post-push record; the manifest cannot list its own hash) |
| D-006_manual.md | this topic manual (documentation batch of 31-Oct-2026) | decides nothing; changes no status |
| D-006_review_notes.md | the topic's review record (same batch) | decides nothing; changes no status |

The two CARRIED files are Claude's checkpoint files "for ChatGPT's review (a checkpoint, not a request for Rama's approval)" (their headers), stored byte for byte as delivered (manifest, section 2).

### 2.2 Other records that concern D-006

- docs/inputs/chatgpt_originals_and_replies/D-006_DEFERRAL_POST_PUSH_GITHUB_VERIFICATION_RESULT_28-OCT-2026.txt: Claude's post-push verification of the deferral package (RESULT: PASS; "Claude-reported; not independently verified by ChatGPT."), stored unchanged by the D-007 package (cecdc57, DR-073). At 6bd80a5 it was the only file in docs/inputs/ whose name names D-006 (a file-name search of the tracked files, docs/inputs/legacy_baseline/ excluded); the documentation batch adds a second, its instruction, docs/inputs/reconciliation/D006_D010_Documentation_Batch.txt (DR-084).
- docs/governance/V2_DECISION_REGISTER.md:
  - DR-072 governs D-006's status and lists what was recorded with it (section 6).
  - Later rows restate the status and change nothing: DR-073, DR-074, DR-075, DR-076, DR-077, DR-079 and DR-081 ("D-006 stays DEFERRED / NOT CLOSED"); DR-078 ("the statuses of D-006 and D-007 [...] are unchanged"). DR-077 also lists among D-010's undecided items "which price action counts (OQ10-04, OPEN on D-006)".
  - DR-083's reason cell records Claude's folder check at 3674847 (TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt, R-04): "D-006 to D-012 neither file", that is, no <ID>_manual.md and no <ID>_review_notes.md.
- docs/governance/V2_DESIGN_LOG.md:
  - section 4: the D-006 row (line 367), and the rows of D-009 to D-012 (lines 371 to 374), which name their D-006 dependencies (section 8);
  - section 6, 2026-10-28: the D-006 note (line 596) and the deferral package's push line (line 597);
  - no section 1 lesson, section 2 open question, section 3 proposal, section 7 bookmark or section 8 gap row names D-006 at 6bd80a5 (a search of the file for D-006 and D006).
- docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt: line 297 and Section D (lines 289 to 317).
- Route B: docs/agenda/route-b/V2_REQUIREMENTS_FIRST_GOVERNANCE_CHANGE_RECORD_28-OCT-2026.txt (DR-074) and ChatGPT's review of the proposal, docs/inputs/reconciliation/CHATGPT_REVIEW_AND_DECISIONS_ROUTE_B_PROPOSAL_28-OCT-2026.txt (RB-Q2): how later topics treat D-006 (sections 7 and 8).
- The later topic records that name D-006: the requirements records of D-008 to D-012, D-010's Stage 1 open-items file and the D-011 and D-012 stage records, as cited in section 8.
- The frozen manuals cited: docs/agenda/d-004_market-data-requirements/D-004_manual.md and docs/agenda/d-005_candle-analysis/D-005_manual.md (section 4).

### 2.3 Records identified but not stored

- The status record, section 4, identifies by bytes and SHA-256:
  - 22 Claude records: the three Stage 0 files (the source inventory and research plan, the candidate file register, the open questions and scope boundary); the three Stage 1 files (the count log, the preflight and self-test report, the proposed read list); and the no-read remediation plan in revisions 0 to 2, with the D-001 to D-005 impact note and a change register from revision 1;
  - 27 ChatGPT files: its Stage 0 set, its reviews, decisions, prompts, checklists and packages, from the Stage 0 handoff package to the deferred storage authorisation package.
- They are "Not stored in this package (outside its authorised scope); identified here so that a later authorised package can store them unchanged. They exist now only in Claude's session workspace". Claude's scratch tools and saved Stage 1 outputs "are not listed or stored here" (status record, section 4).
- At 6bd80a5 no later package has stored any of them (a file-name search of the tracked files). Their content is known here only as the stored records summarise it, and this manual cites the stored record each time.

### 2.4 Which record governs what

| Question | Governing record |
|---|---|
| D-006's status, and what was recorded with it | DR-072, with the status record, section 1 |
| The history of 28-Oct-2026 | the status record, section 2; DR-072's reason cell; the design log's D-006 note (section 6, line 596) |
| The identities of every D-006 record, stored or not | the status record, section 4 |
| Phase C's result and test status | the Phase C stop report |
| The package contents and the checks before the commit | the deferral storage manifest |
| The push and its verification | the post-push record and the push line (design log section 6, line 597) |
| How the later topics treat D-006 | DR-074 and the Route B change record; each later topic's own requirements record |
| This manual | nothing: where it differs from a record above, the record governs |

## 3. Evidence labels

| Label | Meaning in this manual |
|---|---|
| FACT (V2 DECISION) | a register row whose status is DECIDED, quoted or summarised with its ID: here the governance and workflow decisions DR-073, DR-074 and DR-082. DR-073 records "No V2 design decision is made." and DR-074 "No V2 design decision about trading behaviour is made." |
| FACT (V2 RECORD) | what a stored V2 record states, cited by file and section. DR-072 is cited under this label: its status is DEFERRED / NOT CLOSED and it records "No V2 decision is made." A statement about Claude's own actions stays Claude-reported, as the D-006 records say |
| FACT (HISTORICAL) | a frozen manual (D-004, D-005), cited by manual, section and line with its qualifiers; it describes the legacy system as that manual documents it, not V2 |
| INTERPRETATION | a reading by Claude or ChatGPT, with its basis: marked where a record marks it, and where this manual groups records |
| UNKNOWN / NOT ESTABLISHED | not shown by any stored record, with the record that says so where one does |
| OPEN QUESTION | a recorded question without an answer: Q-M6, and the later topics' questions that wait on D-006 |
| RAMA (STATED); RAMA-AUTHORISED, DRAFTED BY CHATGPT; PROVISIONAL; NOT RESEARCHED; KNOWN DEFECT | not used. No Rama statement or decision specific to D-006 is recorded. No provisional working position or raw idea belongs to D-006. D-006 is not a Route B topic and has no V2_DESIGN_LOG section 8 row. No D-006 record names a known defect (RD-5 is recorded as a report wording error). |

No label is merged with another. A count, a file name, a test plan or a proposed design is never evidence that the legacy system had or used any formation or pattern.

## 4. The topic

- FACT (V2 RECORD): the Master Agenda lists "D-006  Formation / pattern analysis" (line 297) in Section D, "TRADE DECISION ENGINE" (line 289), between D-005 Candle analysis and D-007 Timeframe analysis. Section D's goal is to completely revisit the question "Should this signal become a trade?" (lines 311 to 313), and "This section is especially important for the known redesign concerns: late entry, unsuitable filtration, incorrect S&R range, and trading near high-of-day without adequate S&R context." (lines 315 to 317). The agenda gives D-006 no other description.
- FACT (HISTORICAL): the frozen D-005 manual, which documents individual-candle (per-bar) measures, lists "multi-candle formations and patterns (D-006)" among the subjects outside its scope (D-005_manual.md, section 1, line 12) and records them in its boundary table as a "scope boundary only", status "not documented" (section 7, line 155). D-005 is "factual legacy documentation only" (FROZEN 2026-10-28, DR-071).
- FACT (HISTORICAL): the frozen D-004 manual does not cover "The analytic use of candles, patterns, timeframes, market context and S&R (D-005 to D-012), and entry, ranking and the final verdict (D-013 to D-018): their data inputs only are recorded here." (D-004_manual.md, section 1.4, line 64). D-004 is "factual legacy documentation only" (FROZEN 2026-10-28, DR-070).
- FACT (V2 RECORD): ChatGPT's Stage 0 review authorised a count-only Stage 1 of T01 to T11, "the GUI configuration removed from every scope" (status record, section 2).
- UNKNOWN / NOT ESTABLISHED: the topic's planned questions and boundaries in detail. The records that set them out are not stored (section 2.3): Claude's Stage 0 inventory, candidate file register and open questions; ChatGPT's Stage 0 repository review and topic proposal, its preliminary candidate source map and its working answers and standing scope; and the Stage 1 expression table. The deferral note leaves "Every D-006 formation/pattern question that needs more than the provisional Stage 1 locators" UNKNOWN / NOT ESTABLISHED (section 3).
- FACT (V2 DECISION): D-006 is not a Route B topic. Route B (DR-074) covers D-008 to D-018 and E-001 to N-011; in the Route B change record, D-006 "stays DEFERRED / NOT CLOSED as a legacy topic (DR-072); Q-M6 OPEN" (section 2).

## 5. What happened (28-Oct-2026, IST)

Times are those of the status record (section 2), DR-072's reason cell and the design log's D-006 note (line 596), and, for the storage, the manifest and the post-push record.

- Stage 0, 15:20 to 16:06:
  - 15:20: ChatGPT's Stage 0 set.
  - 15:56 to 15:57: Claude's Stage 0 inventory, candidate file register and open questions, "from path names and metadata only".
  - 16:06: ChatGPT's Stage 0 review and count-only Stage 1 authorisation (T01 to T11, the GUI configuration removed from every scope).
- Stage 1, 16:12 to 16:46:
  - 16:12 to 16:14: the gate tests and the 24 count-only runs of T01 to T11 at 970aabf, with synthetic controls only (the counts 16:13 to 16:14, DR-072).
  - 16:33 to 16:34: the Stage 1 count log, the preflight and self-test report and the proposed read list.
  - 16:39: ChatGPT's Stage 1 review: deviation DV-01; no content read authorised; Group B rejected, Groups A and C not authorised. 16:46: a lighter re-share.
- The no-read remediation plan, 16:59 to 18:08:
  - 16:59 to 17:06: revision 0.
  - 17:17: ChatGPT's decisions Q-M1 to Q-M6 (Q-M1 (d), no D-006 content read; RD-1 a separate deviation; Q-M6 open).
  - 17:33 to 17:37: revision 1, with the D-001 to D-005 impact note.
  - 17:57: ChatGPT's decisions Q-R1 to Q-R5.
  - 18:06 to 18:08: revision 2. Report correction RD-5 is recorded with these revisions (design log, line 596).
- Phase C, 18:17 to 18:27:
  - 18:17: Phase C, synthetic-only tests, conditionally authorised. Claude's first attempt to write the Phase C tools was halted by an assistant-side safety control during generation; nothing was created.
  - 18:27: the Phase C stop report.
- The deferral, 18:36 to 18:54:
  - 18:36: ChatGPT's stop decision: Phase C stopped / blocked, D-006 deferred. Claude's first attempt at the planning response was also halted by the safety control; the deferral note followed.
  - 18:42: ChatGPT accepted the deferral status and allowed D-007 Stage 0 planning with D-006 carried as unresolved.
  - 18:54: the bounded storage decision for the package, with its prompt.
- Storage, 18:58 to 19:00:52:
  - 18:58 to 19:00: the checks K-1 to K-10, all PASS, on the V2 clone only (manifest, section 3).
  - 19:00:28: the commit dd14668; 19:00:38: the push to GitHub main; 19:00:49 to 19:00:52: the check on a fresh clone, RESULT: PASS (post-push record).
- Afterwards (FACT (V2 RECORD)):
  - from 19:11: D-007 began, with D-006 carried as an unresolved dependency (DR-072);
  - 21:15: the D-007 package (cecdc57) stored D-006's post-push record and push line ("D-006 handoff CLEARED", D-007_BLOCKED_POST_PUSH_GITHUB_VERIFICATION_RESULT_28-OCT-2026.txt, section 5);
  - 22:25: Route B (8aac5d1, DR-074) kept D-006 DEFERRED / NOT CLOSED as a legacy topic;
  - 29 and 30-Oct: D-008 to D-012 restated the status and recorded their own D-006 dependencies (section 8);
  - 31-Oct: this manual and the review notes (DR-084).
- Who did what: ChatGPT set each step by its reviews, decisions and prompts. Claude ran Stage 0 and Stage 1, wrote the plan revisions and the checkpoint files, and prepared, stored and pushed the package "under W-10 as updated on 27-Oct (V2_DESIGN_LOG section 5) and the bounded storage decision" (DR-072). No Rama decision specific to D-006 is recorded.

## 6. What the records establish and what they do not

### 6.1 Established by the records (FACT (V2 RECORD) unless marked)

- Stage 0 was made "from path names and metadata only" (status record, section 2; DR-072).
- Stage 1 was "24 count-only runs of T01 to T11 at 970aabf, synthetic controls only, NO VALID REAL CONTROL for every run", "whose counts are provisional candidate locators only, not established findings" (DR-072). The status record: "PROVISIONAL CANDIDATE LOCATORS ONLY; not established findings about the legacy system" (section 1). The number of runs is a process fact. This manual gives no count result, and no count is a finding.
- No content read was authorised: "Content reads (Q-M1 (d))" is "none authorised in D-006" (status record, section 1); Q-M1 (d) is "no D-006 content read; generic-only masking not approved" (DR-072).
- The remediation design, revisions 0 to 2, is "unvalidated; no test ran" (status record, section 1).
- Phase C "stopped at step B (implementation), before any tool, fixture or test was created." (stop report, section 1). Every planned test group (TG-0 to TG-13, TG-L and the static tests) is NOT RUN, and "No property of the revision 2 design is shown. Nothing here concerns real-repository or production behaviour." (section 2). A search by modification time "found no file created or changed after 18:17 IST before this report" (section 1, METADATA).
- Phase E, "any real-repository activity, including a count rerun", is NOT AUTHORISED (DR-072).
- Three process records, each separate (DR-072; status record, section 1):
  - DV-01: "the masking filter loaded HR-1 and HR-2 as token sources in every count run and gate test; accepted as documented, not proven" (DR-072). ChatGPT's Stage 1 review, as the design log quotes it: ("Value-blind" and "not printed" do not mean "not read.") (line 596).
  - RD-1: "masked matching lines passed through an in-memory pipe in the count path; a separate process/implementation deviation" (DR-072), "accepted as documented, not proven" (status record).
  - RD-5: "a Stage 1 report's "absent from every scope", wrong for s_all" (DR-072), a "report wording error, accepted as documented" (status record).
- ChatGPT's decisions are recorded by ID: "Q-M1 (d) (no D-006 content read; generic-only masking not approved), Q-M2 (R1, conditional), Q-M3 (names-only baseline proposed, unproven), Q-M4, Q-M5, Q-R1 to Q-R5 and Q-S1 to Q-S5" (DR-072). Beyond these words, their content is in ChatGPT's files, which are not stored (section 2.3): UNKNOWN / NOT ESTABLISHED here.
- The assistant-side safety control halted two of Claude's responses: the first attempt to write the Phase C tools, and the first attempt at the planning response after the stop decision. Neither was repeated: "Claude will not repeat or rework that attempt." (stop report, section 1); "It is not reproduced or reworked here." (deferral note, section 2).
- The stop report confirms, for Phase C, no network call or remote contact, no command against any clone or real repository, no legacy source-content read, no access to the HR-1, HR-2 or HR-3 locations, no read of the saved listing or of any saved scope-list content, no count rerun, no repository change, and no change to the Stage 1 outputs, the revision 0 or 1 files or the D-001 to D-005 records (section 4). The deferral note confirms the same for its step (section 6).
- The storage step used "No legacy repository, protected record, saved scope list, test, count or tool" (manifest, scope; check K-10).
- No earlier record, freeze row or repository was changed (DR-072).

### 6.2 Not established (UNKNOWN / NOT ESTABLISHED, with the record that says so)

- "Every D-006 formation/pattern question that needs more than the provisional Stage 1 locators." (deferral note, section 3).
- "Every property of the revision 2 design (no test ran)." (deferral note, section 3; stop report, section 2).
- The "External disclosure, display, persistence and matched-text outcomes of DV-01 and RD-1" (status record, section 1; deferral note, section 3).
- Why the safety control fired: "Why the control fired is not established." (status record, section 2; the deferral note says the same of its own halt, section 2).
- Whether ChatGPT's D-006 masking ruling has any consequence for D-001 to D-005: OPEN QUESTION Q-M6 (section 7.2).
- The content of the records that were not stored (section 2.3).
- INTERPRETATION: no stored D-006 record supports any statement about legacy formation or pattern behaviour. The frozen D-005 manual records the subject as "not documented" (section 4), and D-006 established nothing beyond provisional locators.

## 7. The deferral: its recorded reasons and what the records say would be needed to resume

### 7.1 Recorded reasons

- FACT (V2 RECORD), DR-072's reason cell: "Phase C could not run in this session and no other evidence route was proposed; deferring keeps D-006 visible and unclosed without inferring any result (ChatGPT's stop decision, 18:36 IST; its deferral and handoff review, 18:42 IST; the bounded storage decision and its prompt, 18:54 IST, 28-Oct)."
- FACT (V2 RECORD): the stop report applied the execution prompt's rule 8 ("If any test fails or cannot run, stop at the first failure. Do not fix the implementation and rerun within this checkpoint") and asked ChatGPT "How D-006 should continue, given that the phase C build cannot proceed as planned in this session. Claude makes no proposal on reworking the halted content." (sections 1 and 5).
- FACT (V2 RECORD): the deferral note's recommendation: "D-006 should remain DEFERRED. Claude proposes no alternative evidence route in this session." (section 2).
- INTERPRETATION, as the design log records it (line 596), from the deviation note, revision 2, section 9 (not stored): "D-006's boundary excluded the host records outright, while D-004 and D-005 had allowed the filter's value-blind use, and Claude carried that practice forward without raising the difference first (the lesson of 2026-10-24)". The lesson proposed in that note is "PROPOSED, not recorded as a design-log lesson" (status record, section 5).

### 7.2 What the records say would be needed (no new proposal)

- FACT (V2 RECORD): Phase E, "any real-repository activity, including a count rerun", is NOT AUTHORISED (DR-072). No record authorises it.
- FACT (V2 DECISION): legacy-source work as a whole waits on option C, "No legacy-source work for any remaining agenda topic until a compliant method is explicitly approved. Planning-only work may occur only where already permitted and must not be used to bypass the block." It "approves no method", and "D-006 stays DEFERRED / NOT CLOSED and Q-M6 OPEN (DR-072)" (DR-073). Under Route B, "legacy-source work stays blocked for every topic" (change record, section 1), and the risk "Route B used as a path to legacy reads" is met by "section 4; option C unchanged; any read needs a new explicit decision" (section 7). DR-082 keeps option C: "Decision: Keep Option C. Do not reopen legacy access now."
- FACT (V2 DECISION): the D-006 records are not evidence for later topics. Route B excludes, "As evidence: D-006's provisional counts, D-007's 15 candidate path references, and any record not frozen" (change record, section 4), and answers the risk "D-006 or D-007 records treated as results" with "excluded as evidence; statuses unchanged" (section 7).
- FACT (V2 RECORD): a V2-only pass on the subject is not started now. ChatGPT's RB-Q2: "V2-only passes for D-006 and D-007 subjects: NOT NOW; revisit only if a concrete dependency requires one." (CHATGPT_REVIEW_AND_DECISIONS_ROUTE_B_PROPOSAL_28-OCT-2026.txt, section 2).
- FACT (V2 DECISION): DR-074 records "a V2-only pass on formation or timeframes" as "Not part of this change".
- FACT (V2 RECORD): the records not stored are "to be stored unchanged only by a later authorised package" (status record, section 5).
- OPEN QUESTION: Q-M6, "whether ChatGPT's D-006 masking ruling has any consequence for D-001 to D-005", is OPEN; "no earlier record, freeze row or repository changed" (status record, section 5).
- UNKNOWN / NOT ESTABLISHED: who holds Q-M6 and how it would be answered; the records name neither.
- This manual proposes no method, route, test, tool or sequence for resuming D-006.

## 8. Topics that depend on it

### 8.1 D-007, and any later topic, by D-006's own records

- FACT (V2 RECORD): "D-007 may begin at Stage 0 planning only, with D-006 carried as an unresolved dependency." (DR-072). D-007's status record: "D-006 stays carried as an unresolved dependency (DR-072)." (section 1).
- FACT (V2 RECORD): "Dependency note for D-007 and later topics: the D-006 formation and pattern questions remain unresolved. No topic may infer their answers or treat the provisional Stage 1 counts as established facts." (status record, section 5). The design log's D-006 row: "carried into D-007 as an unresolved dependency: no topic infers D-006's formation or pattern answers or treats its provisional counts as established" (line 367).
- FACT (V2 DECISION): Route B's rule for a topic that meets D-006's subject: it "records any timeframe or formation dependency as OPEN / UNRESOLVED" (change record, section 2), and the risk "A topic depends on D-006 or D-007 subject matter" is met by "recorded OPEN / UNRESOLVED or as a bookmark (W-9); the RB-Q2 stop rule applies" (section 7; DR-074).

### 8.2 The Route B topics D-008 to D-012 (FACT (V2 RECORD); each topic's own record governs its items)

| Topic and status | Item | What the record says | Source |
|---|---|---|---|
| D-008, V2 REQUIREMENTS RECORDED 2026-10-29 (DR-075) | none | its D-006 row: status DEFERRED / NOT CLOSED (DR-072), relation "no dependency identified" | D-008_v2_requirements.md, section 10 |
| D-009, DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076) | OQ9-07, "The formation dependency on D-006" | "OPEN / UNRESOLVED; no D-009 input needs it now (INTERPRETATION)"; "If S&R levels are later defined from pivots or swings, that dependency is recorded OPEN / UNRESOLVED (INTERPRETATION; OQ9-07)." | D-009_v2_requirements.md, sections 7, 9 and 10 |
| D-010, V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077) | OQ10-04, "which price action evidences a reference" | "OPEN; D-006 dependency; P10-01"; the decided RQ10-03 leaves open "which price action counts (OQ10-04; D-006 dependency)" | D-010_v2_requirements.md, sections 2.1, 9.1 and 10 |
| D-010 | OD-02, "Evidence that rests on formations" | held by "D-006, DEFERRED / NOT CLOSED"; inputs RI-06 (IP-06) and OQ10-04 | D010_STAGE1_REVIEW_RESPONSE_AND_OPEN_ITEMS_30-OCT-2026.txt, section 3.5; D-010_v2_requirements.md, section 2.2 |
| D-010 | P10-01, a proposal for OQ10-04 | "Kinds that rest on candle formations (rejection candles among them) wait for D-006", in a section the record labels "PROPOSAL (Claude); not Rama's; not requirements" | D-010_v2_requirements.md, section 3A |
| D-011, V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079) | OQ11-05, what counts as a reaction, how a role switch is recognised and how a reaction's size is measured | holder: "design; any formation-based reaction waits on D-006" | D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt, section 6; D-011_v2_requirements.md, sections 3, 8 and 10 |
| D-012, V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081) | OQ12-05, what counts as a reaction, as crossing through a reference and as "recently" | holder: "design; any formation-based reaction waits on D-006" | D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt, section 6; D-012_v2_requirements.md, sections 3, 8 and 10 |

- FACT (V2 RECORD): each of D-009 to D-012 records that its decided content needs no formation decision, so Route B's stop rule is not triggered (an INTERPRETATION in each record):
  - D-009 decides no requirement, and "no D-009 input needs a formation decision" (section 7);
  - D-010: "RQ10-01 and RQ10-03 need no timeframe or formation decision [...] So neither D-007 nor D-006 blocks them (DR-074's stop rule is not triggered)." (section 3);
  - D-011 and D-012: "No requirement needs a timeframe or a formation decision, so DR-074's stop rule is not triggered" (section 3 of each).
- FACT (V2 RECORD): the design log's section 4 rows say the same in short: D-009, "any formation dependency (D-006) OPEN / UNRESOLVED" (line 371); D-010, "the timeframe (D-007) and formation (D-006) dependencies OPEN / UNRESOLVED" (line 372); D-011 and D-012, "formation-based reactions wait on D-006" (lines 373 and 374).
- INTERPRETATION (this manual's grouping; nothing is decided by it): every recorded dependency on D-006 concerns what S&R evidence may rest on: levels defined from pivots or swings (D-009), price action that rests on candle formations (D-010), and formation-based reactions (D-011, D-012). None of them is a decided requirement, and none settles a D-006 question.

### 8.3 Bookmarks and other rows

- No V2_DESIGN_LOG section 7 bookmark names D-006 at 6bd80a5 (BK-75, BK-77 and BK-79 name D-007 only). No section 2 open question, section 3 proposal or section 8 gap row names D-006.

## 9. Cross-references and bookmarks

- D-005 (FROZEN, DR-071): its boundary lines leave multi-candle formations and patterns to D-006, "not documented" (section 4).
- D-004 (FROZEN, DR-070): section 1.4 leaves the analytic use of patterns, among others, to D-005 to D-012 (section 4).
- D-007 (BLOCKED / NOT CLOSED, DR-073): it began with D-006 carried as an unresolved dependency; its package stored D-006's post-push record and push line; its records restate D-006's status (docs/agenda/d-007_timeframe-analysis/D-007_manual.md).
- Route B (DR-074, W-11): D-006 stays a legacy topic; its provisional counts are excluded as evidence; RB-Q2 (sections 7.2 and 8.1).
- DR-083 and BK-85: DR-083 adds a V2 topic manual and review notes for each Route B topic, and BK-85 tracks D-008 to D-012. D-006 is not a Route B topic. DR-083's reason cell records that the D-006 folder held neither file at 3674847.
- README.md, the agenda/ line, at 6bd80a5: "D-006 and D-007: status records only, DR-072 and DR-073". The documentation batch brings that line up to date (DR-084).
- Bookmarks: none names D-006 (section 8.3).
- Lessons: no V2_DESIGN_LOG section 1 row names D-006; the rows of 2026-10-28 (lines 170 to 173) concern D-005. The root cause in section 7.1 cites the lesson of 2026-10-24 (line 136), unchanged.

## 10. Status

- DEFERRED / NOT CLOSED (DR-072); Phase C NOT RUN; Phase E NOT AUTHORISED; Q-M6 OPEN. Not frozen; no legacy manual; no V2 decision; the Stage 1 counts are provisional candidate locators only.
- Stored in docs/agenda/d-006_formation-pattern-analysis/: this manual, D-006_review_notes.md and D-006_manual_manifest.md (31-Oct-2026, DR-084), beside the four records of 28-Oct-2026 (section 2.1). The manifest lists this manual's sources with their SHA-256 at the base and the checks run on it. The post-push record is in docs/inputs/chatgpt_originals_and_replies/.
- Nothing here changes a decision or status. Where this manual and a record differ, the record governs.
