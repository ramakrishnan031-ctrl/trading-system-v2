# D-012 S&R freshness - V2 requirements

- Agenda ID: D-012, "S&R freshness" (Master Redesign Agenda, Part 5, Section D, line 303).
- Route B topic (DR-074, W-11), run under Route B's amended procedure (DR-080, W-13): a V2 requirements record. No
  legacy manual is written for D-012.
- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081), by the storage action that stores this file. RQ12-01 to
  RQ12-04 are DECIDED at requirement level by Rama (section 2.1), on the starting definition decided at D-011
  (RQ11-04, DR-079). Everything else is OPEN or a dependency. NOT FROZEN. Requirement level only.
- The boundary (Rama's 19:53 answers, ChatGPT's wording): "Governance boundary: These answers establish
  requirement-level direction for D-012, not an algorithm, expiry rule, threshold, timeframe hierarchy or
  trading-authority rule."
- Started under W-12 (DR-078) after D-011 was published (GitHub main d665719, 19:43 IST). Prepared by Claude from 19:45
  IST. Checked before the push by an independent Claude reviewer that did not draft it
  (D-012_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt).
- Built from D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt ("the stage record": locators L12-, evidence
  E12-, the decisions in section 5, open questions OQ12-) and the D-010 and D-011 records it cites. No legacy source or
  legacy repository was read, and no frozen-manual passage was newly read.

Labels: DECISION (RAMA): a choice Rama made on 30-Oct-2026; RAMA-AUTHORISED, DRAFTED BY CHATGPT: wording ChatGPT wrote
for his answers, none of it presented as verbatim Rama wording; RAMA (STATED): his own words; FACT (V2 DECISION / V2
RECORD / HISTORICAL); INTERPRETATION; PROPOSAL; UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION / DEPENDENCY.
Quotes are exact; in prose a line break inside a quote is added for width only.

## 1. Scope and boundary

- FACT (V2 RECORD) (E12-01): D-012 is "S&R freshness" under Section D's goal "Should this signal become a trade?";
  J-005, also "S&R freshness", is under J's KEEP / MODIFY / REPLACE / REMOVE / SHADOW rule. No other scope text.
- FACT (V2 DECISION) (E12-02): D-012 starts from "D-012 Freshness: How recent or stale the historical price behaviour
  supporting an S&R reference is.", with "Recency is separate from D-011 strength." and "D-012 owns the freshness
  assessment." (Rama's D-011 follow-up, ChatGPT's wording).
- D-012 describes freshness for the references D-010 defines (RQ10-01 to RQ10-05). It decides no use of freshness:
  "Later topics determine how they use it." (RQ12-01; Rama's choice, ChatGPT's wording; OQ12-01), and the answers'
  clauses on trade decisions and on exclusion from use are inputs to those topics (Rama, 20:27; the stage record,
  section 5.3). The legacy mechanisms' disposition is J-005's.
- No design: no measure, formula, weighting, threshold, expiry rule, label rule, timeframe or hierarchy, lookback,
  schema, storage, gate, score or trade authority.

## 2. Requirements

### 2.1 Decided at requirement level (DECISION (RAMA); RAMA-AUTHORISED, DRAFTED BY CHATGPT)

Rama relayed ChatGPT's answers to Claude's four questions in a screenshot at 19:53 IST; in the question tool his
replies were "[No preference]" (Q1 to Q3) and "Answers" (Q4), which point to that screenshot (INTERPRETATION
(Claude)). After the independent review he chose, in the tool itself, that the answers' limiting clauses are inputs
to the later topics and that D-011's definition governs (about 20:27 IST; the stage record, section 5.3). The wording
below is quoted exactly from the stage record's transcription (section 5.2); in RQ12-03 the check marks are written
[x] and the long dash --.

| ID | Requirement (the decided wording, exact) | Option chosen | Not decided by it |
|---|---|---|---|
| RQ12-01 | Decided: "Each S&R reference should display freshness information alongside its reference details." and that freshness is descriptive information ("Freshness is descriptive information, [...] Later topics determine how they use it."). An input to the later topics, not decided here (Rama, 20:27): "not an automatic Trade/Reject, entry, SL or target decision" | "Yes, as information (Recommended)"; 20:27: "Input to those topics (Recommended)" | how later topics use freshness (OQ12-01; BK-84) |
| RQ12-02 | Decided: "An old historical S&R reference does not automatically cease to exist merely because time has passed." "An older reference may still be historically relevant." "Historical lookback periods remain subject to D-007 and data-related topics." A restatement of D-011's definition, which governs (Rama, 20:27): "Freshness describes how recent or stale its supporting historical price behaviour is." An input to the later topics and the design, not decided here (Rama, 20:27): "Whether a reference should be excluded from use is a separate decision, not an automatic consequence of age." | "No automatic expiry (Recommended)"; 20:27: "Input to those topics (Recommended)" and "D-011's definition (Recommended)" | who decides any exclusion from use, and on what basis (OQ12-03); the lookback (OQ12-07) and its effect on old references (OQ12-12) |
| RQ12-03 | "[x] Time since last reaction -- Calendar time elapsed since the last reaction." "[x] Candles since last reaction -- Number of candles elapsed. Actual timeframe selection remains D-007-dependent." "[x] Crossed since last reaction -- Whether price has subsequently crossed through the reference." "[x] Recent re-tests -- Whether price has returned to the reference recently." "These are evidence categories only. Measurement, weighting and interpretation remain undecided." | all four kinds | how each is measured (OQ12-05); the timeframe of a candle count: OPEN / UNRESOLVED on D-007 (OQ12-06); how crossing relates to role changes (OQ12-11); whether the four are the only kinds (OQ12-13) |
| RQ12-04 | "Display supporting evidence alongside a descriptive label:" "Fresh" "Aging" "Stale" "The classification methodology, thresholds and calculation remain open for later design." | "Evidence plus a label (Recommended)" | whether the label set is fixed (the option said "such as"; OQ12-04); how a label is set |

- Compared with the options (the design-log lesson of 2026-10-30 on restated options): none of the answers reverses
  its option; the stage record, section 5.2, lists the differences, those that needed Rama went to him at the
  follow-up (section 5.3), and the rest are open questions.
- The option texts are Claude's wording, and its "(Recommended)" marks were PROPOSAL (Claude); the selections are
  Rama's, through ChatGPT's drafted answers.

### 2.2 Rama's own words behind the decisions (RAMA (STATED); RAW / PROVISIONAL USER INPUT)

- D-010 answer sheet, Q9 (RI-12, line 118), an answer to whether zones are common or strategy-specific: historical
  S&R points are "fixed/unchangeable". INTERPRETATION (Claude): it bears on no automatic expiry (RQ12-02); Claude's Q2
  cited it (the stage record, section 5.1).
- D-010 answer sheet, Q12 (RI-15, line 141): "S&R points is to be checked minimum 3 years for Intray and 5 years for
  Delivery at the same time we need to discuss further [...] so take note cautiously" (the lookback, OQ12-07; raw
  figures that depend on D-007, D-010 UK-08).
- D-010 answer sheet, Q7 (RI-10, line 99): "Time being add your best opinion and mark with needs to be discussed later
  in new system design", to ChatGPT's list with "last reaction date/time;" and "age/freshness indicator (owned by
  D-012, not decided here);" (E12-04).

## 3. How the requirements relate (INTERPRETATION (Claude))

- RQ12-01 says each reference "should display" freshness; for strength, whether every reference must show it is
  OPEN (D-011 OQ11-03). Freshness's use is the later topics' (OQ12-01; BK-84).
- RQ12-02: an old reference "does not automatically cease to exist merely because time has passed"; who decides any
  exclusion from use, and on what basis, is not decided here (OQ12-03).
- RQ12-03 and RQ12-04 make freshness explainable, as RQ11-02 and RQ11-03 do for strength. A crossing through a
  reference (freshness evidence) and a role switch (strength evidence, RQ11-02) may describe the same price events;
  how they relate is OPEN (OQ12-11), and D-010's RQ10-04 bears on it.
- No requirement needs a timeframe or a formation decision, so DR-074's stop rule is not triggered; the candle-count
  kind depends on D-007 (OQ12-06), and any formation-based reaction waits on D-006 (OQ12-05).

## 4. Legacy evidence (FACT (HISTORICAL); the Master Agenda's Part 3, items 1 to 8)

| Part 3 item | Citation | Statement, with its qualifiers | ID |
|---|---|---|---|
| 1. Current system | D-004 1.3, line 50; D-005 1, line 15 | recorded owners only: "D-009 to D-012 S&R" for "levels and zones"; "S&R, zones, strength and freshness (D-009 to D-012);". The detector is "the shadow S&R detector, enabled in production" (A-009 8, line 137), kept as recorded (W9-01) | E12-05, E12-06 |
| 2. Code / modules | A-009 8, line 137 | names only: the detector's defaulted fields recency_window_days and recency_min_factor | E12-06 |
| 3 to 6. Inputs, process, output, state change | none | what any freshness or recency computation used, did, produced or stored | NOT RESEARCHED (NR9-01); the fields' effect UNKNOWN / NOT ESTABLISHED (NR9-14) |
| 7. Dependencies | none located | - | - |
| 8. Problems / limitations | none located | - | - |

- KEEP and MODIFY are not available (W-11). RQ12-01 to RQ12-04 are new V2 requirements, not carried from legacy.
- No new gap row: NR9-01 and NR9-14 (V2_DESIGN_LOG section 8, rows 8 and 21) already hold the gap (E12-07).

## 5. V2 records that touch D-012 (FACT, row level)

- D-009: the provisional "freshness to D-012" (E12-03); OQ9-14's note "zones, strength and freshness themselves are
  D-010 to D-012's" (OPEN).
- D-010 (DR-077): RQ10-01 to RQ10-05; P10-03; BK-80 (OPEN). D-011 (DR-079): RQ11-04's starting definition; BK-83.
- DR-078 / W-12: the topic sequence. DR-080 / W-13: the procedure. DR-031: S&R among the owner allocation "C-001,
  D-003, D-009 to D-017 and J". The other ranges that cover D-012: the stage record, section 4.
- VP-08 (PROPOSED except its count; Ratify at "Count DECIDED (DR-031); D-003, D-009 to D-017, J"): not ratified at
  D-012 (OQ12-08).

## 6. Outcome (Master Agenda Part 7, as Route B revises it)

- S&R freshness in V2: new V2 requirements, RQ12-01 to RQ12-04; not KEEP or MODIFY.
- Any legacy freshness or recency mechanism: no outcome at D-012; disposition with J-005 (DEFER, in Part 7's terms).
- SHADOW not used. D-007 and D-006: see section 3; not triggered.

## 7. Decision record (Master Agenda Part 8)

| Field | Entry |
|---|---|
| Agenda ID / Topic | D-012 / S&R freshness |
| Current Production behaviour | two field names only (section 4); what ran NOT RESEARCHED |
| Relevant modules / files | frozen-manual citations only (section 4, item 2) |
| Current problem | none recorded for freshness |
| Discussion | the stage record; D-010's and D-011's records; Rama's answers relayed from ChatGPT at 19:53 IST; the independent review; his follow-up choices at about 20:27 IST |
| Options considered | the question tool's options (the stage record, sections 5.1 and 5.3) |
| Final decision | RQ12-01 to RQ12-04, at requirement level, on D-011's starting definition |
| Reason | Rama's own words (section 2.2) and his choices, in ChatGPT's wording, with the boundary in the header |
| Dependencies | D-009 OQ9-01, OQ9-03, OQ9-13 to OQ9-17, OQ9-21 and OQ9-22, all OPEN (OQ9-01 to OQ9-25 stay OPEN); D-013 to D-018 and E-005 to E-007 (freshness's use); D-007 BLOCKED / NOT CLOSED; D-006 DEFERRED / NOT CLOSED |
| Affected components | not designed |
| Migration impact | none: no legacy element is carried forward |
| Testing requirement | to be defined with the design (Section N) |
| Status | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081); NOT FROZEN |
| Date | 30-Oct-2026 (IST) |
| Approval / reference | Rama's answers through ChatGPT (19:53 IST) and his own choices in the tool (about 20:27 IST); W-12; W-13; the independent review |

## 8. Open questions

OQ12-01 to OQ12-13, as the stage record, section 6, states them; none is closed here (OQ12-08 is answered for D-012
only: not ratified).

## 9. Effect on other open items (INTERPRETATION (Claude); the other records are not edited)

- D-010's OQ10-07 and BK-80: RQ12-03 records the freshness evidence kinds (Rama's choices, ChatGPT's wording), among
  them the time since the last reaction; this bears on, and does not close, which attributes D-010's references
  themselves carry (P10-03). BK-80 gets a D-012 note and stays OPEN for the whole-design review.
- D-011's BK-83: D-012 started from the definition it records, and D-011's definition governs (Rama, 20:27). BK-83 is
  CLOSED by a note in its row, at the topic it names, under the bookmark rule at the head of V2_DESIGN_LOG section 7
  ("it closes only by a note in its row").
- D-009's OQ9-01, OQ9-03, OQ9-13 to OQ9-17: freshness's use is theirs, with the answers' clauses as inputs; all stay
  OPEN, with OQ9-01 to OQ9-25.

## 10. Cross-topic boundaries and dependencies

| Item | Topic | Status | Relation to D-012 |
|---|---|---|---|
| The references freshness describes | D-010 | V2 REQUIREMENTS RECORDED (DR-077) | RQ10-01 to RQ10-05 |
| Strength, kept apart from recency | D-011 | V2 REQUIREMENTS RECORDED (DR-079) | RQ11-04; OQ12-11 |
| Use of freshness, and any exclusion from use | D-009; D-013 to D-018; E-005 to E-007 | DRAFT RECORDED (DR-076); not started | OQ12-01, OQ12-03; BK-84 |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | OQ12-06, OQ12-07 |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | OQ12-05 |
| Candle and market data; lookback | K-009, K-010 | not started | OQ12-07 |
| Recording per signal | D-009 OQ9-22; K-008, K-011 | OPEN; not started | OQ12-09 |
| Legacy freshness or recency mechanisms | J-005 | not started (Route B topic) | NR9-01, NR9-14 |

## 11. Not in this record

- No design or implementation decision, no owner moved, no VP ratified, no frozen manual or earlier decision changed.
- No legacy-source access (DR-073). D-013 is not started by this record.

## 12. Course of the topic (times IST, 30-Oct-2026)

| Time | Step |
|---|---|
| 19:43 | D-011 published (d665719); D-012 authorised to start (W-12) |
| 19:45 to 19:52 | the plan and locators L12-01 to L12-03 |
| about 19:50 | the decision request (four questions) |
| 19:53 | Rama's answers, drafted by ChatGPT and relayed in a screenshot |
| 19:55 to 20:00 | the stage record, this record and the governance edits; a local commit (0a13b9b, never pushed) |
| from about 20:00 | the independent review (W-12): PASS WITH FINDINGS, F-01 to F-11 |
| about 20:27 | Rama's follow-up choices in the question tool (the limiting clauses are inputs; D-011's definition governs) |
| from 20:28 | the revision and the storage action (DR-081) |

## 13. Status

- D-012: V2 REQUIREMENTS RECORDED (DR-081): RQ12-01 to RQ12-04 DECIDED at requirement level; not frozen.
- D-006: DEFERRED / NOT CLOSED. D-007: BLOCKED / NOT CLOSED. D-009: DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076);
  OQ9-01 to OQ9-25 OPEN. D-010 (DR-077) and D-011 (DR-079): V2 REQUIREMENTS RECORDED. Route B (as amended, W-13) and
  option C in force.
