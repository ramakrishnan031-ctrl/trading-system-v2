# D-009 Support & Resistance - V2 requirements (DRAFT)

- Agenda ID: D-009, "Support & Resistance" (Master Redesign Agenda, Part 5, Section D).
- Route B topic (DR-074, W-11): a V2 requirements record. No legacy manual is written for D-009.
- Status: DRAFT ONLY, revision 0, prepared 30-Oct-2026 from 08:48 IST by Claude, for ChatGPT's review, under
  D009_NEXT_STEP_CLAUDE_HANDOFF_30-OCT-2026_RESHARE.txt. NOT FROZEN. NO D-009 REQUIREMENT IS DECIDED in this draft.
- Rama's D-009 answers are rough / raw ideas. His message to ChatGPT on 29-Oct-2026 (a screenshot, not stored,
  DR-008) said: "Consider these are all 'rough or raw ideas', but we need to discuss deeply while designing new
  system". Nothing below turns them into an approved formula, threshold, rule or gate.
- Requirement level only: no design or implementation decision.
- Built from the reviewed D-009 records only:
  - Rama's answers in the guide's "RAMA'S ANSWER" lines
    (D009_RAMA_ANSWER_GUIDE_REAL_MARKET_EXAMPLES_AND_CHATGPT_SUGGESTIONS_29-OCT-2026.txt, the source of truth for his
    wording; cited as "guide line N");
  - the raw-ideas register, revision 1 with correction CR-1 (accepted by ChatGPT for carry-forward,
    D009_FINAL_REVIEW.doc);
  - the Stage 1 evidence sheet (accepted with recorded process deviations), cited by its E9-, V9-, B9-, NR9- and W9-
    IDs;
  - the Stage 0 files, cited by their OQ9-, R9- and M9- IDs.
- No legacy source, legacy repository or frozen manual was read for this file. Frozen-manual content comes through the
  Stage 1 evidence sheet, whose citations were not re-read. V2 records were read for format and IDs only:
  D-008_v2_requirements.md, the register and the design log, at local main 8e60e9f.

Labels (as the handoff names them; the W-11 equivalent in brackets):
- EXPLICIT RAMA INPUT [RAMA (STATED)]: Rama's words, quoted from the guide with the line number. Each one is a raw idea
  unless he decided otherwise; none of them did.
- FACT [HISTORICAL; V2 DECISION / V2 PROPOSAL]: a frozen manual's statement with its own qualifiers, through the Stage
  1 evidence sheet, or a V2 record's row with the status in its own cell. A manual's FACT is about the pinned
  repository or the record it names, never about what a host ran.
- INTERPRETATION / PROPOSAL: a reading or a proposal by Claude or ChatGPT, always named. Never a decision.
- UNKNOWN / NOT ESTABLISHED, and NOT RESEARCHED for legacy behaviour that no frozen manual establishes.
- OPEN QUESTION / DEPENDENCY.

## 1. Scope and boundary

- FACT (the agenda, quoted in Stage 0, M9-01 and M9-04): D-009 has a title and the Section D goal only. The goal
  names "incorrect S&R range" and "trading near high-of-day without adequate S&R context". The agenda gives D-009 no
  separate scope text (OQ9-01).
- FACT (the agenda, M9-02 and M9-05 to M9-07): the agenda keeps these topics separate from D-009:
  - D-010 S&R zones; D-011 S&R strength; D-012 S&R freshness;
  - J-001 to J-009, the legacy S&R engines and their uses (for each mechanism KEEP, MODIFY, REPLACE, REMOVE or
    SHADOW; "Presence of code does not automatically mean the mechanism belongs in the new live decision path.");
  - J-013 retest and structure engines; E-006 S&R-based SL; O-006 S&R architecture.
- Working boundary for this draft: a PROVISIONAL WORKING POSITION, not frozen. It is ChatGPT's proposal in the
  guide, which Rama asked for ("as per your best opinion now", guide lines 102 and 110). D-009 holds whether S&R is
  used in V2's trade decision and its high-level role. Zones go to D-010, strength to D-011 and freshness to D-012.
  Detailed entry use goes to D-013 to D-016, stop and target mechanics to the named E and J topics, and the legacy
  engines' disposition to J (OQ9-01 to OQ9-03).
- The boundary between D-009 and the legacy S&R engine stays unresolved. D-009 keeps, modifies or revives nothing of
  it, and no frozen manual establishes what it did at runtime (section 4).
- No design is chosen: no S&R definition, calculation, method, point or zone representation, timeframe or hierarchy,
  selection rule, buffer, threshold, SL or target formula, R:R rule, gate, score, shadow phase, data rule, schema or
  storage.

## 2. Requirement areas (none decided)

Status values:
- RAW IDEA: Rama's preliminary input.
- PROVISIONAL WORKING POSITION: ChatGPT's suggestion, which Rama adopted "now" or agreed with provisionally; not his
  own reasoned requirement.
- OPEN / UNRESOLVED.
- NOT DECIDED.

| ID | Area: what V2 must eventually decide | EXPLICIT RAMA INPUT (guide line; full text in 2.1) | Status | Open / dependency |
|---|---|---|---|---|
| RQ9-01 | Whether S&R is an input to V2's trade decision at all, and in which role | Q3 (53): "Not understood well, So we will discuss later" | NOT DECIDED. The examples of Q1, Q2, Q7 and Q13 suggest a "reject if the SL and target do not fit" concept (INTERPRETATION; section 3); it must not become a gate by implication | OQ9-13 |
| RQ9-02 | Which S&R levels count: what "nearest" means, and from which timeframes | Q1 (34, 35); Q10 (118): "OPEN / UNRESOLVED: The probable time frames ..."; Q13 (142) | RAW IDEA; the timeframes OPEN / UNRESOLVED on D-007 | OQ9-06, OQ9-14 |
| RQ9-03 | Whether, and how, a trade's SL and target must sit relative to S&R | Q1 (34); Q2 (45); Q7 (88); Q13 (142) | RAW IDEA; the conflicts in section 3 are preserved | OQ9-15 to OQ9-19 |
| RQ9-04 | Common or strategy-specific S&R treatment | Q4 (62): ChatGPT's suggested wording, then "Yes correct only Breakout strategies & pull back strategies logic is equally opposite, like wise S&R considerations also applied accordingly" | PROVISIONAL WORKING POSITION, plus Rama's own point | OQ9-20 |
| RQ9-05 | What happens when S&R data is missing, late, inconsistent or suspect | Q11 (126): "Yes agreed with your suggestion" | PROVISIONAL WORKING POSITION: ChatGPT's suggestion (make the condition visible, record it and alert; whether the signal proceeds, is deferred or is rejected is a later explicit governance decision; no hidden gate) | OQ9-21 |
| RQ9-06 | Recording S&R context for later audit | Q12 (134): "Yes agreed with your suggestion" | PROVISIONAL WORKING POSITION: ChatGPT's suggestion (enough to reconstruct what S&R information was available and whether or how it influenced the decision; fields, retention and implementation later) | OQ9-22 |
| RQ9-07 | Any log-only (shadow) phase before S&R may affect a trade | Q6 (80): "Not understood, So as per your best opinion now, But we will discuss later the relevant issue in new system" | OPEN / UNRESOLVED. No shadow or log-only phase is approved, and whether to evaluate one is undecided | OQ9-23 |
| RQ9-08 | D-009's scope against D-010, D-011, D-012 and J | Q8 (102): the same words as Q6 | PROVISIONAL WORKING POSITION (ChatGPT's proposal, section 1); not frozen | OQ9-01, OQ9-02 |
| RQ9-09 | D-009 against the entry, SL and target topics | Q9 (110): the same words as Q6 | PROVISIONAL WORKING POSITION (ChatGPT's proposal, section 1); not frozen | OQ9-03 |
| RQ9-10 | Room to S&R and late entry | Q13 (142): "We will discuss later, But the concept is simple ..." | RAW IDEA (the fit concept of RQ9-03); its boundary with D-013 to D-016 and E-008 OPEN | OQ9-04 |
| RQ9-11 | VP-08's S&R wording: "S&R logic is set after strategies and processors, and R:R honours S&R." | Q7 (88): "Defer detailed R:R behavior to the E-topic, But the concept is: ..." | NOT DECIDED. VP-08 is PROPOSED except its count and is not ratified at D-009 | OQ9-08 |
| (Q5) | Which legacy S&R component Rama meant | Q5 (72): "I don't recall which component; ..." | UNKNOWN / NOT ESTABLISHED | J-001 to J-009 |
| (Q14) | The testing VM's S&R targets and stops | Q14 (150): "No, I do not recall that behaviour or Not sure" | UNKNOWN / NOT ESTABLISHED; Q-35 (e) OPEN | - |
| (notes) | A direction for the later design discussion | additional notes (155) | not a requirement | - |

### 2.1 Rama's inputs as written (EXPLICIT RAMA INPUT; byte for byte from the guide)

- Q1, line 34: "1. For Intraday: Timeframes for S&R is 1Hr + 1D + 1W, So support & resistance points to be followed in
  these time frames with whichever is nearest S&R, Example: If ITC's S&R points as per 1Hr=Rs 250, 1D=Rs 260, 1W=Rs
  270 and CMP is Rs 246 and strategy.yaml says R:R is Rs 1:2 (Risk 1: Return 2) then system checks the CMP against
  nearest S&R (1Hr > 1D > 1W) so nearest support with 1Hr=Rs 250, So R:R might be Rs 5: Rs 10 then Rs 246+Rs 5 = Rs
  251 which is above then Rs 250 so stock can't be traded or rejected, Assume CMP is Rs 251 then can be traded why? 1D
  S&R is at Rs 260 this will accommodate R:R"
- Q1, line 35: "2. For Delivery: Time frames will be 1D + 1W + 1M with same concept as above"
- Q1, line 36: "* Note is cautiously, While designing new system we will discuss deeply and this is for rough idea only,
  meaning whether follow these time frame or something else including improved logic if any"
- Q2, line 45: "Same as Q1: Check which is next and nearest S&R point? (1Hr > 1D > 1W) then calculate the CMP > S&R
  points can accommodate R:R, if its accommodates then make trade else outright reject, So strategy signal Rs 996 to
  Rs 1000 (assume nearest S&R) so Rs 4 tgt fits within S&R? meaning SL Rs 2: TGT Rs 4 then pass else reject, We will
  discuss later deeply so note down cautiously"
- Q3, line 53: "Not understood well, So we will discuss later"
- Q4, line 62: "Common baseline, with strategy-specific differences only where explicitly defined and tested, Yes
  correct only Breakout strategies & pull back strategies logic is equally opposite, like wise S&R considerations also
  applied accordingly, So we will discuss later"
- Q5, line 72: "I don't recall which component; the records should be reviewed separately, So we will discuss later
  the relevant issue in new system"
- Q6, line 80; Q8, line 102; Q9, line 110: "Not understood, So as per your best opinion now, But we will discuss later
  the relevant issue in new system"
- Q7, line 88: "Defer detailed R:R behavior to the E-topic, But the concept is: Once stock is eligible to make order
  then immediately pull nearest S&R points by comparing 1hr + 1D + 1W then check the R:R whether this R:R points
  within S&R (meaning for risk point is above 'support point' and for reward point must be below 'resistance point'
  why? safest approach) if it holds then place the order else reject, So we will discuss later"
- Q10, line 118: "OPEN / UNRESOLVED: The probable time frames for S&R (For Intraday: 1Hr + 1D + 1W and For Delivery:
  1D + 1W + 1M) and entry conformations may be 1-5mins, we will discuss later"
- Q11, line 126, and Q12, line 134: "Yes agreed with your suggestion"
- Q13, line 142: "We will discuss later, But the concept is simple whether this trade falls with in my R:R by
  considering nearest S&R point (1Hr > 1Month as stated above for intraday to Delivery), if the R:R within S&R then
  take trade else reject ('Within' means both SL & TGT points below its intended S&R, Example: Support is Rs 100 then
  SL points must be till Rs 100 say Rs 99.50-Rs.99.90 like wise If resistance is Rs 200 then TGT must till Rs 200 say
  Rs 199.50-Rs 199.90)"
- Q14, line 150: "No, I do not recall that behaviour or Not sure"
- Additional notes, line 155: "No, I do not recall, But in new system we must deeply discuss on suitable time frames
  for intraday and Delivery; Improved S&R logic if any; and any other improvements if any, we will discuss later, note
  carefully!"

Line breaks inside the quotations are added for width only. Where an answer repeats ChatGPT's suggested wording, the
guide's ChatGPT suggestion is the source of that wording:
- Q4 and Q5 open with ChatGPT's wording, and so do Q7 ("Defer detailed R:R behavior to the E-topic") and Q14.
- Q6, Q8 and Q9 ask for ChatGPT's best opinion.
- Q11 and Q12 agree with ChatGPT's suggestion.
None of these is an independent, fully reasoned requirement from Rama.

## 3. How the raw inputs relate (INTERPRETATION unless marked)

- One concept recurs in Q1, Q2, Q7 and Q13: take the trade only if its SL and target fit within the nearest S&R, and
  otherwise reject it (Q2: "else outright reject"). As a raw idea that would be a Trade / Reject role (RQ9-01). Rama
  answered Q3 "Not understood well", so the role is NOT DECIDED. The concept must not become a live rejection gate by
  implication; explicit governance decides whether S&R is informational, affects ranking, affects Trade / Reject, or
  takes part in entry or risk calculations.
- The following conflicts are preserved, not resolved. They are recorded in the raw-ideas register, revision 1 with
  CR-1, sections C and D.
  - The ITC example (Q1) has label and arithmetic inconsistencies:
    - At a CMP of Rs 246 it calls the Rs 250 level "nearest support", although the level sits above the price.
    - At "1:2 (Risk 1: Return 2)" with a Rs 5 risk the target would be Rs 256, while the example writes "Rs 246+Rs 5
      = Rs 251". Both figures are above Rs 250.
    - At a CMP of Rs 251 the same Rs 5 risk gives a target of Rs 261, above the Rs 260 level. So "can be traded" is
      not consistent with the example's own figures.
  - Which side of support the SL sits on. Q7: "for risk point is above 'support point'". Q13: an SL of Rs 99.50 to
    99.90 for support at Rs 100, i.e. below it.
  - Whether the target sits at the level or below it. Q2 sets the target at Rs 1,000, the level itself ("TGT Rs 4"
    from Rs 996). Q13 keeps it below (Rs 199.50 to 199.90 for Rs 200).
  - Whether the SL is set by the strategy first and then checked against S&R, or derived from support: OPEN /
    UNRESOLVED.
  - Every example is a long trade. Short-side symmetry and exceptions are OPEN / UNRESOLVED; DR-031's six strategies
    include two intraday short strategies.
- Timeframe notation. "1Hr > 1D > 1W" (Q1, Q2) and "1Hr > 1Month" (Q13) may list the timeframes rather than rank
  them. In the ITC example the level used was the nearest one in the trade's direction, whatever its timeframe. No
  hierarchy is set (D-007 BLOCKED / NOT CLOSED).
- Timing. Q7's "immediately pull nearest S&R points" once a stock is eligible bears on when levels are computed and on
  missing data (RQ9-05). It is a dependency, not a decision (OQ9-24).

## 4. Legacy evidence (FACT through the accepted Stage 1 evidence sheet; the Master Agenda's Part 3, items 1 to 8)

Each row is as the evidence sheet records it. Its frozen-manual citations were not re-read for this draft. A
repository value is not a deployed value (W-7). Code, configuration and tests are not proof of deployed runtime
behaviour.

| Part 3 item | Citation (frozen manual, heading, line) | Statement, with the manual's qualifiers | ID |
|---|---|---|---|
| 1. Current system | D-004_manual.md 7, line 227; C-008_manual.md 3, line 128; B-006_manual.md 3, line 33 | FACT (CONFIG), "repository values at the pin, not the deployed values (gap G-03)": "sr_detector enabled true, timeframes day, 60minute and 30minute, lookback 180 days, cache 1,800 s, wait_for_retest_enabled false ..."; worker TH5 built and started in Phase 0e when "sr_detector.enabled true (509)", repository configuration only | E9-04, E9-05, E9-08 |
| 1 | A-001_manual.md 6, line 46; A-009_manual.md 8, line 137 | the README, as A-001 records it: "... S&R detector ... shadow / default-OFF"; A-009: "the shadow S&R detector, enabled in production" (schema defaults applied for 11 fields). Conflicting wording, preserved as conflicting. Neither establishes the deployed configuration, that the detector ran on a host, or that it affected a trade | E9-01, E9-02; W9-01 |
| 2. Code / modules | D-004_manual.md 19.2, lines 452 to 455; 7, line 226 | the detector files D-004 cites, read for its data contracts only; the shared closure "feeds the S&R detector" when one of six feature flags is on, the flags' definitions not read (G-04) | E9-20, E9-06 |
| 3. Inputs | D-004_manual.md 3, line 177; 7, line 228 | historical candles from broker historical data through OhlcFetcher: one request per interval over [now - lookback, now]; a TTL cache; "returns None or omits the interval on any failure, never raising" | E9-16, E9-17 |
| 4. Process | none | what the detector computed | NOT RESEARCHED (NR9-01) |
| 5. Output | D-004_manual.md 14, line 292; C-011_manual.md 7.6, line 461; 10.2, line 658 | sr_detector_results "by name only (definitions not read)"; the PB-01 row's sr_zone_json, code comment "nearest 30m/1h sr_detector zone to LEVEL (S&R validation programme)", the helper's body UNKNOWN (G-03) | E9-19, E9-21, E9-22; NR9-04, NR9-05 |
| 6. State change | C-011_manual.md 7.6, lines 461 to 464; 16, line 932 | the PB-01 capture's code path writes a row with sr_zone_json and status "PENDING"; whether rows were written, and their use, not established | E9-21, E9-22; NR9-05 |
| 7. Dependencies | D-004_manual.md 7, line 226; 13, line 285; A-009_manual.md 14, line 223; B-006_manual.md 15, line 174 | the six feature flags (G-04); the cache of 1,800 s; the testing-only "S&R SHADOW v1.3 - LOG-ONLY structural shadow", "GATES NOTHING" | E9-06, E9-18, E9-12 to E9-14 |
| 8. Problems / limitations | A-001_manual.md 9, lines 63, 64 (user-reported); A-009_manual.md 18, line 271 | "Incorrect or unsuitable S&R range or interpretation."; "Entry near high-of-day without adequate S&R context."; "The testing code's S&R target and stop behaviour (Rama, Q26)." UNKNOWN | E9-35, E9-38 |
| Decision path | D-003_manual.md 13, lines 484, 486, 488; C-008_manual.md 4.1, lines 153, 155, 171; 9.2, line 327 | FACT (COUNT): count Q03 found only a homonym in the four screener files; the retest diverter runs "only when a diverter is set and enabled", and "none is set"; INTERPRETATION (D-003's), for the screener only: "no S&R, regime or V3-chain input reaches the screener"; ST-13 derives entry and SL "from the trigger price"; the S&R observer runs "after PROCESSED only"; E3, with its "structure SL from the zone band", "has no caller" on the repository configuration | E9-24 to E9-26, E9-29, E9-30, E9-10 |

Legacy behaviour that no frozen manual establishes (W-11). These are evidence gaps, not findings that the behaviour
did or did not exist or run. Route B does not research them, and no legacy source is read to fill them. Whether they
go to V2_DESIGN_LOG section 8 at storage is a package question (the storage plan, PKG9-Q2).

| ID | Legacy fact | Status |
|---|---|---|
| NR9-01 | what the detector computed: levels or zones, method, timeframe use, its fields' effect | NOT RESEARCHED |
| NR9-02 | whether the detector (TH5) ran on any host, and from when | UNKNOWN / NOT ESTABLISHED (Q-44 (a): "TH5 ... not established") |
| NR9-03 | what the observer's hand-off after PROCESSED led to; who calls the detector's factory | NOT RESEARCHED |
| NR9-04 | sr_detector_results: definition, writer, reader, content | UNKNOWN / NOT ESTABLISHED |
| NR9-05 | the PB-01 zone: its selection and failure handling, the candle source, whether rows were written, what the entry stage did with them | UNKNOWN / NOT ESTABLISHED |
| NR9-06 | whether any S&R output set or changed a production Trade / Reject, entry, SL or target | NOT RESEARCHED |
| NR9-07 | the six feature flags gating the shared closure, and which were on in operation | UNKNOWN / NOT ESTABLISHED |
| NR9-08 | the structure exit's logic and its use of _strong_close | UNKNOWN / NOT ESTABLISHED |
| NR9-09 | the retest confirmation's frac value and its use of _strong_close | UNKNOWN / NOT ESTABLISHED |
| NR9-10 | the testing S&R shadow v1.3's output and database; the testing code's S&R target and stop behaviour | UNKNOWN / NOT ESTABLISHED (Q-35 (e) OPEN) |
| NR9-11 | what "Incorrect or unsuitable S&R range or interpretation" referred to | UNKNOWN / NOT ESTABLISHED (Rama: "No, I do not recall", guide line 155) |
| NR9-12 | what "adequate S&R context" for an entry near the day's high meant | UNKNOWN / NOT ESTABLISHED |
| NR9-13 | how the README's "shadow / default-OFF" relates to "enabled in production" and the repository flag true | UNKNOWN / NOT ESTABLISHED (W9-01) |
| NR9-14 | what the 11 defaulted detector fields did, and whether any default changed a trade | UNKNOWN / NOT ESTABLISHED |

KEEP and MODIFY are not available for any legacy S&R behaviour (W-11). The frozen manuals establish only its
repository configuration and code paths, not what ran.

## 5. Rama's statements on S&R before D-009 (FACT, as the records quote them)

| Evidence ID | Source | Statement | Label |
|---|---|---|---|
| E9-35 | A-001_manual.md 9, lines 63, 64 | "Incorrect or unsuitable S&R range or interpretation."; "Entry near high-of-day without adequate S&R context." | user-reported, as A-001 records it |
| E9-36 | A-009_v2_design_inputs.md line 123 (Q39) | "In S&R requires further discuss on revising existing functionality, ... shadow configuration also requires discussion on existing mechanism to revised one while implementing the same" | RAMA (STATED), verbatim as recorded; decides nothing |
| E9-37 | A-009_manual.md 14, line 230; A-010_v2_design_inputs.md line 131 | testing differed from production in S&R; "the differences that could change a trade were the start time and S&R" (as A-009 records his answers) | as recorded; Q26 NOT ESTABLISHED |

## 6. V2 records that touch D-009 (at 8e60e9f; FACT, row level)

- DR-031 (DECIDED): "Which strategies, their scanner conditions (VP-44), filtration (VP-03), S&R, R:R and the
  admission test (no subset or near-copy) stay with C-001, D-003, D-009 to D-017 and J." This is an owner allocation;
  it decides no S&R content.
- VP-08 (PROPOSED except the count; Ratify at "Count DECIDED (DR-031); D-003, D-009 to D-017, J"): "S&R logic is set
  after strategies and processors, and R:R honours S&R." Whose words this sentence is, is not stated in the row. Not
  ratified at D-009 (RQ9-11).
- VP-44 (PROPOSED; Ratify at "D-001, D-013 to D-016, E-002"): its considerations include defining "late" with "room
  to target and S&R". D-009 is not one of its ratification points.
- Q-35 (OPEN), item (e): "did the testing code place targets and stops from S&R levels (Rama, A-009 Q26)?"
- Q-44 (OPEN), item (a): which optional workers, the S&R detector among them, ran on the production host.
- DR-069 and DR-071 (DECIDED; the D-003 and D-005 freezes): no S&R input reaches the screener (INTERPRETATION, MC-76);
  the _strong_close measure on the retest path, its use UNKNOWN.
- DR-075 (DECIDED; D-008): late entries and entries near the day's high go to D-009 to D-016, "OPEN in those
  topics". RQ8-01 keeps broad-market context out of Trade / Reject qualification; stock-level S&R is a separate
  matter.
- DR-072 (D-006 DEFERRED / NOT CLOSED), DR-073 (option C; D-007 BLOCKED / NOT CLOSED), DR-074 (Route B).
- The lesson of 2026-10-12: two files are identical only by object ID at named commits. D-009 makes no new
  comparison.
- V2 design inputs (PROPOSED by title): A-008_v2_design_inputs.md lines 12, 25, 28 and 98; A-009_v2_design_inputs.md
  lines 129 and 143; A-011_v2_design_inputs.md line 148 ("No silent carry of legacy ... S&R ... behaviour");
  B-004_v2_design_inputs.md lines 7 and 33.

## 7. Outcome (Master Agenda Part 7, as Route B revises it)

- V2 S&R requirements: none decided in this draft. The areas in section 2 stay RAW IDEA, PROVISIONAL WORKING POSITION,
  OPEN / UNRESOLVED or NOT DECIDED.
- The legacy S&R mechanism: no outcome at D-009. Its disposition is J's (J-001 to J-009); KEEP and MODIFY are not
  available (section 4).
- SHADOW: not chosen. No shadow or log-only phase is approved, and whether to evaluate one is OPEN (RQ9-07).
- D-007: Rama's timeframe ideas are recorded as raw ideas only, OPEN / UNRESOLVED, with no hierarchy. DR-074's rule
  is that a topic which cannot be completed without a timeframe decision records it OPEN / UNRESOLVED and stops with a
  narrow V2-only proposal. INTERPRETATION: this draft does not trigger that rule, because it decides no requirement;
  any later decision on RQ9-02 or RQ9-03 would.
- D-006: no D-009 input needs a formation decision. "Pivot" occurs only as legacy names (E9-02, E9-15). OPEN /
  UNRESOLVED if S&R levels are later defined from pivots or swings (INTERPRETATION; OQ9-07).

## 8. Decision record (Master Agenda Part 8)

| Field | Entry |
|---|---|
| Agenda ID / Topic | D-009 / Support & Resistance |
| Current Production behaviour | FACT, section 4: a detector in the repository configuration, flag true; conflicting shadow / on-off wording; what ran NOT RESEARCHED or UNKNOWN |
| Relevant modules / files | frozen-manual citations only (section 4, item 2) |
| Current problem | user-reported (A-001 section 9): "Incorrect or unsuitable S&R range or interpretation." and "Entry near high-of-day without adequate S&R context."; what they meant in practice UNKNOWN (NR9-11, NR9-12) |
| Discussion | the Stage 1 evidence; Rama's answers Q1 to Q14 as raw ideas; the raw-ideas register, revisions 0 and 1 with CR-1, each reviewed by ChatGPT (section 12) |
| Options considered | ChatGPT's answer sheet and guide, with its suggestions (not decisions) |
| Final decision | NONE. No D-009 requirement is decided (section 2) |
| Reason | Rama's statement that his answers are "rough or raw ideas" for deep discussion in the new-system design |
| Dependencies | section 10; D-007 OPEN / UNRESOLVED; D-006 OPEN / UNRESOLVED |
| Affected components | not designed |
| Migration impact | none: no legacy element is carried forward |
| Testing requirement | to be defined with the design (Section N); none proposed here |
| Status | DRAFT ONLY, revision 0, NOT FROZEN |
| Date | 30-Oct-2026 (IST) |
| Approval / reference | ChatGPT's review of this draft (pending) |

## 9. Open questions (none closed in this draft)

Stage 0 questions, with their status now:

| ID | Question | Status now |
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

New questions from the raw inputs:

| ID | Question | Source | Who decides | Note |
|---|---|---|---|---|
| OQ9-13 | Should S&R be an input to V2's trade decision at all? If so, in which role: a Trade / Reject condition, a score or ranking input, entry / SL / target placement, recorded context only, or another? | Q3; Q1, Q2, Q7, Q13 | Rama, after discussion | until decided, no S&R gate exists by implication |
| OQ9-14 | What does "nearest S&R" mean? For a long trade, which support below price and resistance above price count, and for a short trade the reverse? Is one timeframe considered first, or all compared? What if a closer level competes with a stronger one, or levels and zones overlap? | Q1, Q2, Q7, Q13 | design, then Rama | zones, strength and freshness themselves are D-010 to D-012's |
| OQ9-15 | Does the SL sit above support (Q7) or just below it (Q13)? | Q7, Q13 | Rama | detailed SL rules are E-006's |
| OQ9-16 | May a target sit at the resistance level (Q2) or must it stay below it (Q13)? | Q2, Q13 | Rama | detail with the E topics |
| OQ9-17 | Is the SL (risk) set by the strategy and then checked against S&R, or derived from support? | Q1 (the Rs 251 case), Q13 | Rama | E-006 |
| OQ9-18 | Short-side symmetry, and any exceptions | all examples long; DR-031 | Rama | - |
| OQ9-19 | How is the ITC example to be read: its labels, its arithmetic and the Rs 251 case? | Q1 | Rama | kept as a rough illustration, not an approved example |
| OQ9-20 | Which parts of S&R treatment are common and which strategy-specific (breakout against pullback), and how are differences governed and tested? | Q4 | Rama; strategy filtration itself with D-003 and D-017 (VP-03) | - |
| OQ9-21 | With missing, late, inconsistent or suspect S&R data, does the signal proceed, wait or get rejected? What is alerted, and how does the check avoid becoming a hidden gate? | Q11 | Rama, as an explicit governance decision | the alert design with its owner (not named) |
| OQ9-22 | What minimum S&R record is kept with each evaluated signal and decision, and which later step and owner define its fields, retention and storage? | Q12 | agenda governance, then Rama | - |
| OQ9-23 | Should any log-only (shadow) phase be evaluated? If so, should evidence be gathered before any V2 trading, or should V2 trade without S&R authority during a log-only period, given live-only V2 (DR-001)? What are the measures, sample, acceptance criteria, owner and approval gate? | Q6 | explicit governance review | J-009 owns the review of the legacy S&R shadow |
| OQ9-24 | When are S&R levels computed relative to a signal, and where do weekly and monthly candles come from? | Q7 ("immediately pull"); Q1, Q10 (1W, 1M) | design | K-009 (market data) |
| OQ9-25 | Does V2 use S&R points, zones or both? | Rama's examples use single prices | D-010 (zones) | D-009 records the dependency only |

## 10. Cross-topic boundaries and dependencies

| Item | Topic | Status | Relation to D-009 |
|---|---|---|---|
| S&R zones; strength; freshness | D-010; D-011; D-012 | not started | kept separate (OQ9-01, OQ9-25) |
| The legacy S&R engines and their uses | J-001 to J-009 | not started (Route B topics) | disposition is J's; D-009 keeps, modifies or revives nothing |
| Retest and structure engines | J-013 | not started | the E3 path and the structure exit (NR9-08) |
| Entry zone, breakout, retest, entry timing | D-013 to D-016 | not started | the high-of-day and room-to-S&R parts (OQ9-04; RQ9-10) |
| Score / ranking; the final Trade / Reject | D-017; D-018 | not started | where any S&R role would act (OQ9-13) |
| S&R-based SL; the R:R work | E-006; the E topics (E-008 is named at A-008_v2_design_inputs.md line 28) | not started | OQ9-15 to OQ9-17 |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | OQ9-06; no hierarchy |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | OQ9-07 |
| Market context | D-008 | V2 REQUIREMENTS RECORDED (DR-075) | separate from stock-level S&R |
| Market data (weekly and monthly candles) | K-009 | not started | OQ9-24 |
| Data-quality alerts | the alert topics (owner not named) | not started | OQ9-21 |
| Recording design | not named | - | OQ9-22 |
| Testing | Section N | not started | none proposed here |

## 11. Not in this record

- No requirement decided, no design decision, no owner moved, no VP ratified, and no frozen manual, erratum,
  companion or earlier decision changed.
- No legacy-source access (DR-073). NR9-01 to NR9-14 are not researched by Route B.
- No D-006, D-007, D-010 to D-018, E or J work is started by this record.

## 12. Course of the topic (times IST, 29 and 30-Oct-2026; Rama's messages and Claude's file writes)

| Time | Step |
|---|---|
| 14:05, 29-Oct | D-008 package pushed (8e60e9f) |
| 14:15 | ChatGPT's D-009 Stage 0 prompt, standing scope and working answers, and its review of the D-008 post-push record |
| 14:27 | Claude's three Stage 0 files |
| 14:33 | ChatGPT's Stage 0 review and Stage 1 decision, with the Stage 1 prompt |
| 14:33:56 to 14:47:10 | Stage 1 reads of the frozen manuals and V2 records |
| 15:02 to 15:03 | the three Stage 1 files |
| 15:29 | ChatGPT's Stage 1 review ("ACCEPTED WITH RECORDED PROCESS DEVIATIONS, FOR EVIDENCE-REVIEW PURPOSES ONLY"), its answer sheet and its handoff |
| 19:31 | the guide with Rama's answers, and ChatGPT's raw-ideas register (revision 0) |
| 19:35 | Claude's review notes on the raw ideas |
| 19:42 | ChatGPT's review of the notes, with corrections A to F |
| 19:45 to 19:48 | the register, revision 1, its change log and the provenance checklist |
| 19:55 | ChatGPT's review of revision 1: one wording correction |
| 19:56 to 19:57 | correction CR-1 and the change log updated |
| 20:38 | ChatGPT's final review (D009_FINAL_REVIEW.doc): "ACCEPTED FOR CARRY-FORWARD TO THE D-009 STORAGE PACKAGE." |
| 20:39 | Claude's local carry-forward list |
| 08:40, 30-Oct | ChatGPT's next-step handoff, reshared (the 29-Oct copy did not download) |
| from 08:48 | this draft, the storage plan draft and the package change log draft |

## 13. Status

- Route B in force (DR-074, W-11); option C in force (DR-073).
- D-006: DEFERRED / NOT CLOSED. D-007: BLOCKED / NOT CLOSED.
- D-008: V2 REQUIREMENTS RECORDED (DR-075).
- D-009: DRAFT ONLY, revision 0, for ChatGPT's review. Nothing is frozen, written to the V2 repository, committed or
  pushed. Every open question stays OPEN.
