# D-011 S&R strength - V2 requirements

- Agenda ID: D-011, "S&R strength" (Master Redesign Agenda, Part 5, Section D, line 302).
- Route B topic (DR-074, W-11), the first run under Route B's amended procedure (DR-080, W-13): a V2 requirements
  record. No legacy manual is written for D-011.
- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079), by the storage action that stores this file. RQ11-01 to
  RQ11-04 are DECIDED at requirement level by Rama (section 2.1). Everything else is OPEN or a dependency. NOT
  FROZEN. Requirement level only: no design or implementation decision.
- The bound (Rama's 18:47 answers, ChatGPT's wording): "Important: These answers establish requirement-level direction
  only. They do not approve formulas, thresholds, scoring, timeframe hierarchy or trading authority."
- Started under W-12 (DR-078) after D-010 was published (GitHub main 7c725ed, 18:37 IST). Prepared by Claude from 18:40
  IST. Checked before the push by an independent Claude reviewer that did not draft it
  (D-011_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt); its findings led to the follow-up of 19:35 IST.
- Built from D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt ("the stage record": locators L11-, reads R11-,
  evidence E11-, the process departures DV11-01 to DV11-04, the decisions in sections 5.2 and 5.3, open questions
  OQ11-) and the D-010 records it cites. No legacy source or legacy repository was read; frozen-manual lines were read
  only as the stage record lists.

Labels: DECISION (RAMA): a choice Rama made on 30-Oct-2026; RAMA-AUTHORISED, DRAFTED BY CHATGPT: wording ChatGPT wrote
for his answers, none of it presented as verbatim Rama wording; RAMA (STATED): his own words; FACT (V2 DECISION / V2
RECORD / HISTORICAL); INTERPRETATION; PROPOSAL; UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION / DEPENDENCY.
Quotes are exact; in prose a line break inside a quote is added for width only.

## 1. Scope and boundary

- FACT (V2 RECORD) (E11-01, E11-02): D-011 is "S&R strength" under Section D's goal "Should this signal become a
  trade?"; J-004, also "S&R strength", is under J's KEEP / MODIFY / REPLACE / REMOVE / SHADOW rule. No other scope text.
- D-011 describes how strongly history supports an S&R reference, for the references D-010 defines (RQ10-01 to
  RQ10-05, DR-077). It decides no use of strength: "The later topics--including trade decisions, entry eligibility,
  SL and targets--must independently decide how strength is used within their respective scopes." (Rama's follow-up,
  ChatGPT's wording; the stage record, section 5.3; OQ11-01). Freshness is D-012's, from the starting definition in
  RQ11-04. The legacy mechanisms' disposition is J-004's (OQ11-02).
- No design: no formula, weighting, threshold, count or label rule, timeframe or hierarchy, lookback, schema, storage,
  gate, score or trade authority.

## 2. Requirements

### 2.1 Decided at requirement level (DECISION (RAMA); RAMA-AUTHORISED, DRAFTED BY CHATGPT)

Rama asked ChatGPT to "Answer these 4 questions suitably & correctly even my 'raw' wordings are incorrect" and relayed
its answers at 18:47 IST; after the independent review he relayed three more at 19:35 IST (the stage record, sections
5.2 and 5.3). In the question tool his replies were "Refer answers in above attached screenshot" (Q1's field) and
"[No preference]" (the other fields), and then "[No preference]", "[No preference]" and "Answers"; INTERPRETATION
(Claude): each reply points to the screenshot sent with it. The wording below is quoted exactly from the stage
record's transcriptions; in RQ11-02 the check marks are written [x] and [ ], and the long dash --.

| ID | Requirement (the decided wording, exact) | Option chosen | Not decided by it |
|---|---|---|---|
| RQ11-01 | "Each S&R reference may display how strongly its historical price behaviour is supported." "Strength is descriptive information only." With the follow-up: "D-011 establishes S&R strength as descriptive information." "Equally, D-011 itself does not grant strength any trading authority." | 18:47: "Yes, as information (Recommended)"; 19:35: "Input to those topics" | whether every reference must show it (OQ11-03); every use of strength, which the later topics decide (OQ11-01); the 18:47 list "It must not independently determine:" "Trade/Reject." "Trade priority or score." "Entry eligibility." "SL or target." is carried to D-009, D-013 to D-018 and E-005 to E-007 as an input, not a decided limit ("Do not impose a blanket restriction that strength can only ever be combined with other evidence.") |
| RQ11-02 | "[x] Number of reactions -- How many historical times price reacted around the reference." "[x] Role switches -- Whether the reference has historically acted as both support and resistance." "[x] Size of the reaction -- How significantly price moved away after reacting." "These are eligible evidence categories, not approved calculation or weighting rules." | three of the four kinds | "[ ] Seen on several timeframes." with "Reason: D-007 is still BLOCKED. Timeframe hierarchy and interpretation must not be assumed.": OPEN / UNRESOLVED on D-007, not rejected (OQ11-06); how each kind is defined, counted or weighted (OQ11-05) |
| RQ11-03 | "Show supporting historical evidence alongside a descriptive label such as:" "Weak" "Medium" "Strong" "The actual classification methodology, thresholds, weighting and calculation remain undecided." | "Evidence plus a label (Recommended)" | the label set ("such as") and how a label is set (OQ11-04) |
| RQ11-04 | "D-011 Strength: How strongly historical price behaviour supports an S&R reference." "Do not mix recency into the D-011 strength calculation." D-012's starting definition, decided at the follow-up: "D-012 Freshness: How recent or stale the historical price behaviour supporting an S&R reference is." with "Recency is separate from D-011 strength." "D-012 owns the freshness assessment." "D-012 must determine its remaining requirements, evidence, methodology and treatment independently." | 18:47: "Keep it in D-012 (Recommended)"; 19:35: "Decided starting point (Recommended)" | D-012's own requirements beyond its starting definition (D-012); the strength calculation itself (RQ11-03). The 18:47 wording of the D-012 description ("How recent or stale that supporting historical behaviour is.") stays as history |

- Compared with the options (the design-log lesson of 2026-10-30 on restated options): none of the answers reverses
  its option. The stage record, section 5.2, lists each difference; those that needed Rama went to him at the
  follow-up (section 5.3), and the rest are open questions (OQ11-03, OQ11-04, OQ11-06).
- The option texts are Claude's wording, and its "(Recommended)" marks were PROPOSAL (Claude); the selections are
  Rama's, through ChatGPT's drafted answers. At 19:35 he chose "Input to those topics", not Claude's recommended
  "Decided limit".

### 2.2 Rama's own words behind the decisions (RAMA (STATED); RAW / PROVISIONAL USER INPUT)

- D-010 answer sheet, Q1 (RI-02, line 34): "multiple touches whenever this price is reached" (number of reactions).
- D-010 answer sheet, Q4 (RI-07, line 66): "Rs 277.95 acts as support during Feb 10-19, But the same price acts as
  resistance during 20-Apr-2026" (a role switch).
- D-010 answer sheet, Q7 (RI-10, line 99): "Time being add your best opinion and mark with needs to be discussed later
  in new system design", to ChatGPT's list that put a "confidence/strength indicator (owned by D-011, not decided
  here);" beside "reaction/touch count;" and "last reaction date/time;" (E11-06).

## 3. How the requirements relate (INTERPRETATION (Claude))

- RQ11-01 makes strength information without trading authority; how it is used, alone or with other evidence, is
  each later topic's decision (OQ11-01; BK-82).
- RQ11-02 and RQ11-03 make strength explainable; the answer's reason: "This provides transparency instead of
  displaying an unexplained strength label." Both follow RQ10-03 (a reference rests on its own price history).
- RQ11-04 keeps strength, how strongly history supports a reference (D-011), apart from freshness, how recent it
  is (D-012). Whether the last reaction's date is shown with the strength evidence is not decided here (D-012;
  OQ11-04).
- RQ10-04 (positions relative to CMP) bears on how a role switch is recognised, which stays OPEN (OQ11-05).
- No requirement needs a timeframe or a formation decision, so DR-074's stop rule is not triggered; the timeframe
  kind of evidence stays OPEN / UNRESOLVED (OQ11-06), and any formation-based reaction waits on D-006 (OQ11-05).

## 4. Legacy evidence (FACT (HISTORICAL); the Master Agenda's Part 3, items 1 to 8)

| Part 3 item | Citation | Statement, with its qualifiers | ID |
|---|---|---|---|
| 1. Current system | D-004 1.3, line 50; D-005 1, line 15 | recorded owners only: "D-009 to D-012 S&R" for "levels and zones"; "S&R, zones, strength and freshness (D-009 to D-012);". The detector is "the shadow S&R detector, enabled in production" (A-009 8, line 137), kept as recorded (W9-01) | E11-07, E11-10 |
| 2. Code / modules | D-005 7, line 158; C-011 11.2, line 735; A-009 8, line 137 | names only: "Zone and ScoredZone properties", "classified S&R; not documented"; WatchlistCaptureWorker(..., zone_knobs=..., zone_scoring=...) ("FACT for the lines"); 11 defaulted detector fields, among them weak_breakout_frac and touch_cap (and recency_window_days and recency_min_factor, which concern recency: D-012) | E11-08 to E11-10 |
| 3 to 6. Inputs, process, output, state change | none | what any strength-like computation used, did, produced or stored | NOT RESEARCHED (NR9-01; NR11-01); the detector fields' effect UNKNOWN / NOT ESTABLISHED (NR9-14) |
| 7. Dependencies | none located | - | - |
| 8. Problems / limitations | none located | - | - |

- Also (not a Part 3 item): D-003 13, lines 484 and 488: the screener count found only a homonym, and D-003's
  INTERPRETATION is "no S&R, regime or V3-chain input reaches the screener" (E11-11).
- KEEP and MODIFY are not available (W-11). RQ11-01 to RQ11-04 are new V2 requirements, not carried from legacy.
- New gap row NR11-01 (V2_DESIGN_LOG section 8, row 22): what the ScoredZone properties and the capture's zone_scoring
  argument computed, and whether any strength measure ran (the detector's fields: as NR9-14); selected by name
  (INTERPRETATION); NOT RESEARCHED; an evidence gap, not a finding.

## 5. V2 records that touch D-011 (FACT, row level)

- D-009: the provisional "strength to D-011" (E11-03); OQ9-14's "a closer level competes with a stronger one" (OPEN).
- D-010 (DR-077): RQ10-01 to RQ10-05; OQ10-07 OPEN; P10-03; BK-80 (OPEN).
- DR-078 / W-12: the topic sequence. DR-080 / W-13: Route B's amended procedure. DR-031: S&R among the owner
  allocation "C-001, D-003, D-009 to D-017 and J". The other ranges that cover D-011: the stage record, section 2.1.
- VP-08 (PROPOSED except its count; Ratify at "Count DECIDED (DR-031); D-003, D-009 to D-017, J"): not ratified at
  D-011 (OQ11-08).

## 6. Outcome (Master Agenda Part 7, as Route B revises it)

- S&R strength in V2: new V2 requirements, RQ11-01 to RQ11-04; not KEEP or MODIFY.
- The legacy strength-like mechanisms: no outcome at D-011; disposition with J-004 (DEFER, in Part 7's terms).
- SHADOW not used (as at D-008 and D-010). D-007 and D-006: see section 3; not triggered.

## 7. Decision record (Master Agenda Part 8)

| Field | Entry |
|---|---|
| Agenda ID / Topic | D-011 / S&R strength |
| Current Production behaviour | names only (section 4); what ran NOT RESEARCHED |
| Relevant modules / files | frozen-manual citations only (section 4, item 2) |
| Current problem | none recorded for strength |
| Discussion | the stage record; D-010's records; the independent review; Rama's answers relayed from ChatGPT at 18:47 and 19:35 IST |
| Options considered | the question tool's options (the stage record, sections 5.1 and 5.3) |
| Final decision | RQ11-01 to RQ11-04, at requirement level |
| Reason | Rama's own examples (section 2.2) and his choices, in ChatGPT's wording, with the bound in the header |
| Dependencies | D-009 OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22, all OPEN (OQ9-01 to OQ9-25 stay OPEN); D-013 to D-018 and E-005 to E-007 (strength's use); D-007 BLOCKED / NOT CLOSED; D-006 DEFERRED / NOT CLOSED; D-012 |
| Affected components | not designed |
| Migration impact | none: no legacy element is carried forward |
| Testing requirement | to be defined with the design (Section N) |
| Status | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079); NOT FROZEN |
| Date | 30-Oct-2026 (IST) |
| Approval / reference | Rama's answers through ChatGPT (18:47 and 19:35 IST); W-12; W-13; the independent review |

## 8. Open questions

OQ11-01 to OQ11-10, as the stage record, section 6, states them; none is closed here (OQ11-08 is answered for D-011
only: not ratified).

## 9. Effect on other open items (INTERPRETATION (Claude); the other records are not edited)

- D-010's OQ10-07 and BK-80: RQ11-02 lists the number of reactions, role switches and reaction size as strength
  evidence, and RQ11-04 keeps recency with D-012. This bears on, and does not close, which attributes D-010's
  references themselves carry (P10-03); BK-80 gets a D-011 note and stays OPEN for D-012.
- D-009's OQ9-01, OQ9-03, OQ9-13 to OQ9-17: RQ11-01 gives strength no trading authority, and its use is theirs to
  decide; all stay OPEN, with OQ9-01 to OQ9-25.

## 10. Cross-topic boundaries and dependencies

| Item | Topic | Status | Relation to D-011 |
|---|---|---|---|
| The references strength describes | D-010 | V2 REQUIREMENTS RECORDED (DR-077) | RQ10-01 to RQ10-05 |
| Use of strength in the trade decision; a closer level against a stronger one (OQ9-14) | D-009; D-017; D-018 | DRAFT RECORDED (DR-076); not started | OQ11-01; BK-82 |
| Use of strength in entry, SL and targets | D-013 to D-016; E-005 to E-007 | not started | OQ11-01; BK-82 |
| Freshness, recency, decay | D-012 | next (W-12) | RQ11-04's starting definition; BK-83 |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | OQ11-06, OQ11-07 |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | OQ11-05 |
| Candle and market data | K-009, K-010 | not started | OQ11-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | OPEN; not started | OQ11-09 |
| Legacy strength-like mechanisms | J-004 | not started (Route B topic) | NR11-01 |

## 11. Not in this record

- No design or implementation decision, no owner moved, no VP ratified, no frozen manual or earlier decision changed.
- No legacy-source access (DR-073). D-012 is not started by this record.

## 12. Course of the topic (times IST, 30-Oct-2026)

| Time | Step |
|---|---|
| 18:37 | D-010 published (7c725ed); D-011 authorised to start (W-12) |
| 18:40 to 18:46 | the plan, locators L11-01 to L11-04 and reads R11-01 to R11-05 |
| about 18:46 | the decision request (four questions) |
| 18:47 | Rama's answers, drafted by ChatGPT at his request and relayed in a screenshot |
| 18:50 to 18:54 | the stage record, this record and the governance edits; a local commit (200497d, never pushed) |
| from about 18:54 | the independent review (W-12): PASS WITH FINDINGS, F-01 to F-18 |
| before 19:35 | the follow-up questions (process, strength use, freshness) |
| 19:35 | Rama's follow-up answers, relayed in a screenshot |
| from 19:36 | the revision, the range locators (L11-05) and the storage action (DR-079, DR-080) |

## 13. Status

- D-011: V2 REQUIREMENTS RECORDED (DR-079): RQ11-01 to RQ11-04 DECIDED at requirement level; not frozen.
- Route B's per-topic procedure amended from D-011 onwards (DR-080, W-13); the earlier steps stay as written.
- D-006: DEFERRED / NOT CLOSED. D-007: BLOCKED / NOT CLOSED. D-009: DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076);
  OQ9-01 to OQ9-25 OPEN. D-010: V2 REQUIREMENTS RECORDED (DR-077). Route B and option C in force.
