# D-010 S&R zones - V2 requirements

- Agenda ID: D-010, "S&R zones" (Master Redesign Agenda, Part 5, Section D, line 301).
- Route B topic (DR-074, W-11): a V2 requirements record. No legacy manual is written for D-010.
- Status: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077), by the storage action that stores this file. RQ10-01 to
  RQ10-05 are DECIDED at requirement level by Rama (section 2.1). Everything else is OPEN, PROPOSED or a dependency.
  NOT FROZEN. Requirement level only: no design or implementation decision.
- The bound: Rama's Q3 answer of 30-Oct-2026 (ChatGPT's wording) closes with "These are bounded requirements only.
  They do not grant Trade/Reject, scoring, SL or target authority", which bounds RQ10-03 to RQ10-05. RQ10-01 and
  RQ10-02 carry the same limit through the OQ10-19 boundary (section 1).
- Stage 2 was authorised by TS_D010_Authorise.txt (Rama, through ChatGPT; received 16:00 IST on 30-Oct-2026). Claude
  prepared this record from 18:00 IST. Before the push it was checked by an independent Claude reviewer that did not
  draft it (W-12; D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt).
- Built from the D-010 records stored with it in docs/agenda/d-010_sr-zones/:
  - D-010_STAGE2_STATUS_RECORD_AND_EXECUTION_PLAN_30-OCT-2026.txt ("the status record"): the Stage 2 authority, plan,
    reads (X2- and R2- IDs) and Rama's answers of 30-Oct (section 4);
  - D010_STAGE1_SUPPLEMENTAL_RAMA_INPUT_REGISTER_30-OCT-2026.txt ("the register"): Rama's Stage 0 answers quoted
    exactly (RI-01 to RI-17), the chart (IN-02) and the CSV (IN-03);
  - D010_STAGE1_SUPPLEMENTAL_CLARIFICATIONS_30-OCT-2026.txt: RC-01, RC-02, RC-03a and RC-03b;
  - D010_STAGE1_REVIEW_RESPONSE_AND_OPEN_ITEMS_30-OCT-2026.txt ("the open-items file"): AD, IP, UK and OD rows;
  - D-010_STAGE1_EVIDENCE_SHEET_30-OCT-2026.txt (E10- and U10- rows) and its read log;
  - the Stage 0 files (C10-, L10-, NF- and OQ10- IDs), with Revision 1 of the open-questions file (SC-01);
  - ChatGPT's Step 2 reconciliation (D010_Step2_Reconciliation_Review_30Oct2026.txt, stored in docs/inputs/).
- No legacy source, legacy repository or frozen manual was read for this file. Frozen-manual content comes through
  the Stage 1 evidence sheet, whose citations were not re-read (OQ10-16, ChatGPT: "NO FURTHER D-010 LEGACY
  RESEARCH").

Labels:
- DECISION (RAMA): an answer Rama submitted on 30-Oct-2026 in the question tool. The choice is his. The wording's
  drafter is named where known (the status record, 4.1 and 4.2); none of it is presented as verbatim Rama wording.
- RAMA (STATED): Rama's own words, quoted exactly from the record named. Unless decided, RAW / PROVISIONAL USER INPUT.
- RAMA-AUTHORISED, DRAFTED BY CHATGPT: wording ChatGPT wrote at Rama's request (RC-01, RC-02, the follow-up answers).
- FACT (V2 DECISION), FACT (V2 RECORD), FACT (HISTORICAL): a register or log row, a V2 record, or a frozen manual's
  statement with its qualifiers, through the Stage 1 evidence sheet. HISTORICAL is never about what a host ran.
- INTERPRETATION and PROPOSAL: a reading or a proposal, always named (Claude's or ChatGPT's); never a decision.
- UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED (legacy behaviour no frozen manual establishes); KNOWN DEFECT; OPEN
  QUESTION / DEPENDENCY.
- Quotes are exact. In prose, a line break inside a quote is added for width only; "[...]" would mark an omission.

## 1. Scope and boundary

- FACT (V2 RECORD) (C10-01; status record X2-05): D-010 is "S&R zones" (agenda line 301). The Section D goal is
  "Should this signal become a trade?", and its known concerns include "incorrect S&R range" and "trading near
  high-of-day without adequate S&R context" (lines 311 to 317). The agenda gives D-010 no other scope text (NF-4).
- FACT (V2 RECORD) (R2-01): J-003, also "S&R zones" (line 439), is under J, whose rule asks KEEP, MODIFY, REPLACE,
  REMOVE or SHADOW for each advanced mechanism; "Presence of code does not automatically mean the mechanism belongs
  in the new live decision path." (lines 451 to 460).
- Working boundary (P4: ChatGPT's Step 2 reconciliation, sections 2.1 and 2.4; not frozen):
  - D-010 records how V2 represents and describes S&R references, at requirement level;
  - D-009 keeps whether and how S&R informs the trade decision (OQ9-13 OPEN), "nearest" in the trade-fit sense
    (OQ9-14), SL / target / R:R relations, short-side trade symmetry (OQ9-18) and strategy use (OQ9-20);
  - D-011 strength; D-012 freshness; D-013 to D-018 and the E topics, entry, timing, score, Trade / Reject, SL and R:R;
  - J-003 and J-013, the disposition of the legacy zone mechanisms, with no legacy research under D-010 (OQ10-02,
    OQ10-16);
  - D-010 may record representation requirements "without assigning S&R any Trade/Reject authority, screening gate,
    score, ranking or decision effect" (OQ10-19).
- Terminology (OQ10-20, ChatGPT's recommendation, applied): "S&R zone" names D-010's subject, apart from D-013's
  "Entry zone". This record says "S&R reference" for one support or resistance item. INTERPRETATION (Claude): in
  RQ10-01's terms an S&R zone is a reference's surrounding reaction area; this is a reading, not a definition.
- No design: no method for finding a reference or sizing its area, width, tolerance, touch count, pivot or swing
  rule, timeframe or hierarchy, lookback, merge or cluster rule, strength or freshness measure, field list, schema,
  storage, gate, score or trade rule.

## 2. Requirements

### 2.1 Decided at requirement level (DECISION (RAMA), 30-Oct-2026)

The decided wording is quoted exactly from the status record, section 4, which states its ASCII substitutions;
none of them falls in the wording below. RQ10-04's list markers ("- ") are the answer's own.

| ID | Requirement (the decided wording, exact) | Provenance and Rama's inputs behind it | Not decided by it |
|---|---|---|---|
| RQ10-01 | "Each S&R reference should retain: - Its reference price. - Its surrounding reaction area, where historical price behaviour supports one. The method for calculating or sizing the area remains undecided." | DECISION (RAMA): Q1 answer; drafter of the wording not established (status record 4.1). Behind it: his Stage 0 answers (RI-01, RI-04; RI-15, line 145: "these were acts like zones") and RC-01 (RAMA-AUTHORISED, DRAFTED BY CHATGPT) | the method and size of an area, and when history "supports one" (design); which price action counts (OQ10-04) |
| RQ10-02 | "Each stock has its own S&R references, shared across all strategies.  How individual strategies consume those references remains open under OQ9-20." | DECISION (RAMA): Q2 answer; drafter not established. Behind it: RC-03a (RAMA (STATED): "S&R points/things are not strategy specific instead it's a global thing"), RI-12 ("So make it as 'COMMON'") | how each strategy uses the references (D-009 OQ9-20) |
| RQ10-03 | "S&R references must originate from the stock's own historical price behaviour, not arbitrary offsets such as CMP + 1%." | DECISION (RAMA): Q3 answer, item 1; ChatGPT's wording (its 17:55 reply: "My earlier Q3 wording"). Behind it: RI-05 ("instead of playing with random numbers/prices") | which price action counts (OQ10-04; D-006 dependency); the lookback (D-007; UK-08) |
| RQ10-04 | "- An S&R reference below the current market price (CMP) is a potential support reference. - An S&R reference above CMP is a potential resistance reference. - As price moves, the same historical reference can change its functional role. - These are relative market-context classifications, not permanent labels or guaranteed reversal points." | DECISION (RAMA): Q3 answer, item 2, as corrected by the follow-up (RAMA-AUTHORISED, DRAFTED BY CHATGPT; status record 4.2): the tool returned "[No preference]" for the role check, and the correction came in its "Review step" field and again through the 17:55 relay. Behind it: RI-07 (his Rs 285 / Rs 270 example of the Stage 0 sheet's Q4) and the option he ticked | the trade side (long / short) of his Q4 words: D-009 OQ9-18; what counts as touching or breaking a reference (UK-03) |
| RQ10-05 | "The next historical reference in the direction of movement is separate from the reference currently being tested or broken." | DECISION (RAMA): Q3 answer, item 3; ChatGPT's wording. Behind it: RC-02 (RAMA-AUTHORISED, DRAFTED BY CHATGPT); RI-01 and RI-04 (his "next" wording). The option ticked also said "not a target or a prediction" (Claude's wording); the decided wording does not, and his examples set the TGT at the next reference (RI-02, RI-03) | which reference is "next" or "nearest" when several compete (D-009 OQ9-14); whether it serves as an entry or a target (D-013 to D-016; E-007); RC-02: "not a guaranteed destination or prediction" |

- KNOWN DEFECT, corrected before recording: the first Q3 answer read "Above CMP, the reference acts as support; below
  CMP, it acts as resistance.", the reverse of the option ticked and of RI-07. Claude asked once more; the tool
  returned "[No preference]" for that question, and the correction came in the other question's field and again in
  the 17:55 relay ("My earlier Q3 wording had these reversed. Correct it."). RQ10-04 carries the corrected wording
  (status record 4.2).
- "market-context" in RQ10-04 is the answer's own word. INTERPRETATION (Claude): it means the reference's position
  relative to the stock's price, not D-008's broad-market context.
- RQ10-02 does not mean one price map for all stocks: each stock has its own references (the answer's own words).

### 2.2 Rama's other inputs (RAW / PROVISIONAL USER INPUT; none decided)

| Area | Rama's input (register row; Stage 0 sheet line) | Status | Open / dependency |
|---|---|---|---|
| Which price action counts as evidence | RI-06 (61): "I Guess all of the above"; "add your opinion" | RAW; PROPOSAL P10-01; "all of the above" refers to ChatGPT's list, which held "other" and "not sure" (AD-10) | OQ10-04; D-006 (OD-02); UK-03, UK-12 |
| Timeframes | RI-08 (77): the 1Hr + 1D + 1W and 1D + 1W + 1M sets, "nearest S&R point", "lock the timeframe" | RAW IDEA; no set, hierarchy, selection or lock decided | OQ10-05; D-007 BLOCKED / NOT CLOSED (OD-01) |
| Overlapping or nearby references | RI-09 (82): "Sorry not understood well, So time being add your best opinion and mark with needs to be discussed later in new system design" | no preference; PROPOSAL P10-02 | OQ10-06; OQ9-14 |
| What each reference shows | RI-10 (99): the same request | no preference; PROPOSAL P10-03 | OQ10-07; D-011, D-012 |
| The view relative to the current price | RI-11 (111): the same request | no preference; PROPOSAL P10-04 | OQ10-09; OQ9-14 |
| Incomplete history | RI-13 (130): new listings, merger / demerger, "a control & monitoring mechanism" with NSE's security lists as a master file; "add your best opinion then mark it as needs further discussion" | RAW; PROPOSAL P10-05; his JPL date differs from IN-03's (RI-13; UK-05); the CSV's fields: AD-07 to AD-09 | OQ10-11; OQ9-21; K-009, K-010 (OD-06; BK-79) |
| Reconstructability | RI-14 (135, 136): candles pulled once a stock is an eligible candidate, "one time activity"; "add your best opinions" | RAW; it describes a workflow and does not answer the question; PROPOSAL P10-06 | OQ10-12; OQ9-22; K-008, K-011; UK-06, UK-07 |
| The worked example | RI-02, RI-03 (34, 36), RI-15 (140 to 147): BANKBARODA on a BSE daily chart; the daily instrument file; 3-year and 5-year lookbacks | illustration only; figures and dates as written and unreconciled (register section 6; AD-01 to AD-05) | UK-04, UK-08 to UK-11, UK-13; D-007; K-009 (BK-79) |
| Boundaries | RI-16 (162): "add your best opinion, we will discuss later in new system design" | the recorded boundaries stand (section 1); PROPOSAL P10-07 | OQ10-01, OQ10-02 |
| Anything else | RI-17 (165): left blank; Q12's last item (147) also blank | - | - |

### 2.3 Rama's words behind the decisions (RAMA (STATED))

Each "Q:" line quotes, exactly, Rama's answer text on one line of his completed Stage 0 answer sheet (register IN-01;
the line number before it): the whole line, or the text after the sheet's own label ("Your answer:" or an item
label), which is ChatGPT's template. The "M:" line is his message to ChatGPT (RC-03a, from his screenshot).
- RI-01, line 33:
  Q: Support & Resistance (S&R) means that In Stock trading where the current stock's price (CMP) is going to settled (meaning either upward or downward) once certain price or point is break-out or break-down, Why? a professional trader always looks at chart and takes reference from historical movement when certain price is break-out or Break-down then what's the next possible price to be reached from current price (refer the attached BANKBARODA chart where Blue line is resistance and Green line is Support identification purpose)
- RI-04, line 38:
  Q: ** So basically, S&R means it is expected price points when certain price is broken (rais/falls) then where is the next expected price range stock is going to settled (What is the resistance or support) by reviewing the historical chart with same CMP
- RI-05, line 45:
  Q: Shortly, All traders tries to move  or continue the stock movement when certain price is broken (break-out or down) to some price which is based on historical price touch, once that expected price is touched then there will not such further immediate movement of same stock (ie, consolidation phase of stock), So we must follow the same logic instead of playing with random numbers/prices
- RI-07, line 66:
  Q: Yes surely, Not only mirrors for Long > Short or Short > Long trades, Even Once a Resistance zone/point will acts as Support in another period and Vice-Versa, Simple meaning: The so called lines Support or Resistance is basically self-assumed price levels where will be price to be reached & not a scientifically proven lines, Example: refer the BANKBARODA chart, Rs 277.95 acts as support during Feb 10-19, But the same price acts as resistance during 20-Apr-2026, So same line sometimes considered as resistance or support based on CMP why? when CMP is Rs 285 then Rs 277.95 acts as 'Support' whereas when CMP is Rs 270 then Rs 277.95 acts as resistance, So based on CMP and type of trade (Long/Short) the S&R points differs.
- RI-12, line 118:
  Q: I Guess historical S&R points (ie, where price is settles once certain price is broken) is fixed/unchangeable based on which only traders puts their TGT & SL prices, So may be strategies differs to scan stocks, but S&R remains same irrespective of scanned stocks/used strategies, So make it as 'COMMON' also add your best opinion, if required we discuss later in new system design
- RI-15, line 145:
  Q: I Guess probaly the same price acts as resistance/support once certain price is broken, so these were acts like zones when it reached then either stock will go up/down, add your best opinion, we will discuss later if required
- RC-03a, his message to ChatGPT (clarifications file, section 4):
  M: And for Q3 yes S&R points/things are not strategy specific instead it's a global thing as you referred above how S&R works irrespective of strategy specific things
- His other answers (RI-02, RI-03, RI-06, RI-08 to RI-11, RI-13 to RI-16) are quoted exactly in the register, section 3.

## 3. How the inputs relate (INTERPRETATION (Claude) unless marked)

- His Stage 0 answers join two things: a historical reaction reference, and the next expected price after a break
  (IP-01, ChatGPT's and Claude's reading). RQ10-05 now keeps the second a separate reference. Whether either serves
  as an entry or a target is not decided here (D-013 to D-016; E-007); his own examples set the TGT at the next
  reference.
- RQ10-01 addresses UK-01 (a reference price, plus a reaction area where history supports one) and RQ10-05 addresses
  UK-02.
- RQ10-02 concerns the references themselves. His answer to the D-009 sheet's Q4 opens with ChatGPT's suggested
  wording, "Common baseline, with strategy-specific differences only where explicitly defined and tested", and adds
  "like wise S&R considerations also applied accordingly" (E10-14, E10-10). On 30-Oct he chose "Per stock, all
  strategies", not the option "Shared baseline plus extras" (the status record, section 4). So the references are
  common; how strategies use them, including any strategy-specific treatment, stays OPEN (OQ9-20).
- RQ10-04 states his Stage 0 Q4 example as a rule of position. The trade-type part of that answer ("based on CMP and
  type of trade (Long/Short) the S&R points differs") is trade use and stays with D-009 (OQ9-18).
- The entry, TGT, SL, R:R and order-or-reject steps in his examples are trade use, not D-010's (IP-07; OQ9-13 OPEN;
  the bounds of section 2.1).
- RQ10-01 and RQ10-03 need no timeframe or formation decision: they say what a reference keeps and where it comes
  from, not how it is found. So neither D-007 nor D-006 blocks them (DR-074's stop rule is not triggered).

### 3A. Proposals for Rama's "best opinion" requests (PROPOSAL (Claude); not Rama's; not requirements)

His answers ask for an opinion and to "mark with needs to be discussed later in new system design" (RI-09). Each
proposal below is so marked: TO BE DISCUSSED IN THE NEW-SYSTEM DESIGN. Requirement level only. ChatGPT has not
reviewed them; the independent review checked their labels, their fit with the records and that they decide no
design (D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt).

| ID | Request | Proposal (Claude) | Bounds |
|---|---|---|---|
| P10-01 | Q3, which price action counts (RI-06) | Record the kinds of evidence a reference may rest on, without rules: repeated reactions near a price (touches), reversals, consolidation areas, and earlier breakout or breakdown prices. Kinds that rest on candle formations (rejection candles among them) wait for D-006 | no touch count, tolerance or candle rule (UK-03); D-006 DEFERRED |
| P10-02 | Q6, overlapping or nearby references (RI-09) | Show overlapping or nearby references so that the overlap is visible; do not merge them silently. Whether and how references are merged or grouped is a design item, and grouping across timeframes waits for D-007 | no distance or merge rule; OQ9-14 |
| P10-03 | Q7, what each reference shows (RI-10) | Each reference makes visible: the stock; its reference price and reaction area (RQ10-01); its current role relative to CMP (RQ10-04); the timeframe or timeframes it rests on (OQ10-05; D-007); the historical reactions it rests on, with their dates, so that it can be traced (RQ10-03); the time it was computed; and whether its evidence is complete (P10-05). Strength or confidence goes to D-011, age or freshness to D-012 | a list of things to show, not a field list or schema |
| P10-04 | Q8, the view relative to the current price (RI-11; RI-15, line 146) | For the current price, show the references below (potential support) and above (potential resistance), with each one's distance from the current price, and mark in each direction the reference being tested and the next one beyond it (RQ10-05) | display only; "nearest" in the trade-fit sense stays D-009's (OQ9-14) |
| P10-05 | Q10, incomplete history (RI-13) | When a stock's history is too short or broken (a new listing, a merger or demerger, a suspension), mark its S&R evidence incomplete and show it so, never as complete. The source of listing and corporate-action data, such as NSE's security lists or the broker's instrument file, goes to K-009 and K-010 | what happens to a signal stays D-009's (OQ9-21); no listing-age threshold |
| P10-06 | Q11, reconstructability (RI-14) | If S&R is used at a signal or decision (OQ9-13), keep the S&R references in use at that time as they were then, so that a later review can see what V2 saw | the minimum record, retention and storage stay with D-009 OQ9-22 and K-008, K-011 |
| P10-07 | Q13, what to leave to later topics (RI-16) | Keep the boundaries of section 1 and section 10: D-010 holds the representation and description of S&R references (RQ10-01 to RQ10-05); every use, measure and data source stays with its own topic | not an ownership decision |

- Q12, line 144 ("what price did around those areas"; RI-15: "add your best opinion, we will discuss later"): no
  proposal. It asks for a reading of the example's chart, which needs verified candle data first (UK-04).

## 4. Legacy evidence (FACT through the accepted Stage 1 evidence sheet; the Master Agenda's Part 3, items 1 to 8)

Each row is as the evidence sheet records it; its frozen-manual citations were not re-read. A repository value is
not a deployed value (W-7), and code, comments and configuration prove no runtime behaviour.

| Part 3 item | Citation (frozen manual, heading, line) | Statement, with the manual's qualifiers | ID |
|---|---|---|---|
| 1. Current system | D-004_manual.md 1.3, line 50; D-005_manual.md 1, line 15 | recorded owners only: "D-009 to D-012 S&R" for "levels and zones", under the heading "recorded owners kept; nothing closed, reassigned or reinterpreted"; "S&R, zones, strength and freshness (D-009 to D-012);" (its list lead-in not read). No line read names D-010 alone | E10-19, E10-21 |
| 2. Code / modules | D-005_manual.md 7, line 158; D-004_manual.md 7, line 230 (through E9-17); C-011_manual.md 7.6 (through E9-21) | names only: "Zone and ScoredZone properties, sr_detector/flags.py 159-162, and zone_builder's session_vwap import", "classified S&R; not documented"; the zone warmer; _nearest_zone_json | E10-22, E10-26, E10-24 |
| 3. Inputs | D-004_manual.md 7, line 230 (through E9-17) | the zone warmer is among a candle fetch's readers; what it computed NOT RESEARCHED | E10-26; NR9-01 |
| 4. Process | none | what any legacy zone mechanism computed, and how a zone was chosen | NOT RESEARCHED (NR9-01); the PB-01 helper's body UNKNOWN (G-03) |
| 5. Output | C-011_manual.md 7.6, line 461; 10.2, line 658 (through E9-21) | the PB-01 row's sr_zone_json; comment "Record the nearest 30m/1h S&R zone to LEVEL (validation programme; fail-safe)."; schema comment "nearest 30m/1h sr_detector zone to LEVEL (S&R validation programme)." | E10-24; NR9-05 |
| 6. State change | as item 5 | whether rows were written, and what the entry stage did with them | UNKNOWN / NOT ESTABLISHED (NR9-05) |
| 7. Dependencies | C-008_manual.md 9.2, lines 317, 327 (through E9-30); B-006 TH11 | the E3 retest path's "zone band"; with the repository configuration "the zone warmer, the retest monitor and the structure-exit manager are not started" and "E3 has no caller"; deployed configuration not established | E10-25 |
| 8. Problems / limitations | A-001_manual.md 9, line 63 (through NR9-11) | user-reported: "Incorrect or unsuitable S&R range or interpretation."; its meaning UNKNOWN / NOT ESTABLISHED; Rama: "No, I do not recall" | E10-17, E10-36 |

- KEEP and MODIFY are not available for any legacy zone mechanism (W-11): the frozen manuals establish names,
  comments and repository configuration, not what ran. DR-076: "no KEEP or MODIFY"; disposition with J-001 to J-009
  (E10-27). RQ10-01 to RQ10-05 are new V2 requirements, not carried from legacy.
- No new gap row: the zone behaviours met fall under NR9-01, NR9-05 and NR9-11 (E10-37), and ChatGPT's Step 2 keeps
  them as the evidence trail (OQ10-16).

## 5. Rama's statements on S&R before D-010 (FACT, as the records quote them)

| Evidence ID | Source | Statement | Label |
|---|---|---|---|
| E10-11 to E10-14, E10-16 | D-009_v2_requirements.md 2.1 (his D-009 answers) | point wording ("S&R points", "support point", "resistance point", "nearest S&R"); timeframe ideas; the fit-within-S&R concept | RAMA (STATED); rough / raw ideas (DR-076) |
| E10-15 | A-009_v2_design_inputs.md line 123 (Q39) | "In S&R requires further discuss on revising existing functionality, [...]" | RAMA (STATED, verbatim as recorded); decides nothing |
| E10-17 | A-001_manual.md 9, line 63 (as NR9-11 records it) | "Incorrect or unsuitable S&R range or interpretation." | user-reported; meaning UNKNOWN |
| E10-18 | Stage 1's reads | no line read that a record attributes to him used "zone", "band" or "level" | a bounded negative for those reads only; his Stage 0 answers later use "zone", "range" and "levels" (AD-12) |

## 6. V2 records that touch D-010 (at 9c9c2e9; FACT, row level)

- DR-031 (DECIDED; R2-02): "Which strategies, their scanner conditions (VP-44), filtration (VP-03), S&R, R:R and the
  admission test (no subset or near-copy) stay with C-001, D-003, D-009 to D-017 and J." An owner allocation only;
  the six strategies include two intraday short strategies.
- DR-069 (DECIDED; R2-04): "within the bounded evidence no S&R, regime or V3-chain input reaches the screener
  (INTERPRETATION, MC-76)".
- DR-072 (D-006 DEFERRED / NOT CLOSED), DR-073 (option C; D-007 BLOCKED / NOT CLOSED), DR-074 (Route B), DR-075
  (D-008), DR-076 (D-009 DRAFT RECORDED - NO REQUIREMENT DECIDED; OQ9-01 to OQ9-25 open) (E10-28 to E10-30; R2-03).
- VP-08 (PROPOSED except its count; E10-31): "S&R logic is set after strategies and processors, and R:R honours
  S&R." Not ratified at D-010 (OQ10-14); its Ratify-at cell is unchanged.
- VP-44 (PROPOSED; R2-07): "room to target and S&R" among its considerations; D-010 is not one of its ratification
  points.
- Q-35 (e) and Q-44 (a) (OPEN; R2-06): the testing code's S&R targets and stops; which optional workers ran. Neither
  bears on V2 representation.
- BK-76 to BK-78 (OPEN; R2-05): E-006 / E-008; K-009; D-013 to D-016.
- Gap rows 8, 12 and 18 (NR9-01, NR9-05, NR9-11; OPEN; E10-32): evidence gaps, not findings.

## 7. Outcome (Master Agenda Part 7, as Route B revises it)

- S&R references in V2: new V2 requirements, RQ10-01 to RQ10-05. They are not KEEP or MODIFY (section 4).
- The legacy zone mechanisms: no outcome at D-010. Their disposition is J-003's and J-013's (DEFER, in Part 7's
  terms).
- SHADOW is not used (INTERPRETATION, as at D-008): it presumes an existing mechanism kept observational, and what
  the legacy mechanisms did is not established.
- D-007 (BLOCKED / NOT CLOSED): no decided requirement needs a timeframe decision; OQ10-05 stays OPEN / UNRESOLVED,
  and Rama's timeframe ideas stay raw ideas with no hierarchy. D-006 (DEFERRED / NOT CLOSED): RQ10-03 names no kind
  of price action; OQ10-04 stays OPEN with the D-006 dependency. DR-074's stop rule is not triggered
  (INTERPRETATION).

## 8. Decision record (Master Agenda Part 8)

| Field | Entry |
|---|---|
| Agenda ID / Topic | D-010 / S&R zones |
| Current Production behaviour | FACT, section 4: names, comments and repository configuration of legacy zone items; what ran NOT RESEARCHED or UNKNOWN |
| Relevant modules / files | frozen-manual citations only (section 4, item 2) |
| Current problem | user-reported (A-001 section 9): "Incorrect or unsuitable S&R range or interpretation."; meaning UNKNOWN (NR9-11) |
| Discussion | Stage 0 and Stage 1 records; Rama's Stage 0 answers; the clarifications RC-01 to RC-03; ChatGPT's Step 2 reconciliation; Rama's answers of 30-Oct-2026 (the status record, section 4) |
| Options considered | the question tool's options (the status record, section 4) and ChatGPT's earlier option lists; Claude's proposals P10-01 to P10-07 |
| Final decision | RQ10-01 to RQ10-05, at requirement level (section 2.1); nothing else decided |
| Reason | Rama's own examples and answers (sections 2.2 and 2.3); the bounds of section 2.1: no Trade / Reject, scoring, SL or target authority |
| Dependencies | section 10; D-006 DEFERRED / NOT CLOSED; D-007 BLOCKED / NOT CLOSED; D-009 OQ9-13 to OQ9-18, OQ9-20 to OQ9-22, OQ9-24 and OQ9-25 OPEN |
| Affected components | not designed |
| Migration impact | none: no legacy element is carried forward |
| Testing requirement | to be defined with the design (Section N); none proposed here |
| Status | V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077); NOT FROZEN |
| Date | 30-Oct-2026 (IST) |
| Approval / reference | Rama's answers in the question tool and his follow-up through ChatGPT (17:55 IST); TS_D010_Authorise.txt; the independent review (W-12) |

## 9. Open questions

### 9.1 Stage 0's questions, with their status now (the Stage 0 file is not edited)

| ID | Question (short) | Status now |
|---|---|---|
| OQ10-01 | D-010 against D-009, D-011, D-012 | OPEN; the working boundary of section 1 applied; not frozen |
| OQ10-02 | D-010 against J-003 | OPEN as a governance boundary; no legacy research under D-010 |
| OQ10-03 | point, band, both or other | DECIDED at requirement level (RQ10-01); the area's method OPEN (design) |
| OQ10-04 | which price action evidences a reference | OPEN; D-006 dependency; P10-01 |
| OQ10-05 | per timeframe or combined | OPEN / UNRESOLVED on D-007 |
| OQ10-06 | overlapping or nearby | OPEN; P10-02 |
| OQ10-07 | attributes | OPEN; P10-03; strength with D-011, freshness with D-012 |
| OQ10-08 | support and resistance as mirrors | role relative to CMP DECIDED (RQ10-04); trade-side symmetry OPEN (OQ9-18) |
| OQ10-09 | the view relative to the current price | OPEN; P10-04; "nearest" with D-009 (OQ9-14) |
| OQ10-10 | common or strategy-specific | DECIDED (RQ10-02); strategy use OPEN (OQ9-20) |
| OQ10-11 | incomplete evidence | OPEN; P10-05; OQ9-21; K-009, K-010 |
| OQ10-12 | reconstructability | OPEN; P10-06; OQ9-22 |
| OQ10-13 | weekly and monthly data | OPEN on K-009 and K-010; OQ9-24 |
| OQ10-14 | VP-08 at D-010 | OPEN; not ratified at D-010 |
| OQ10-15 | "incorrect S&R range" | UNKNOWN / NOT ESTABLISHED |
| OQ10-16 | legacy zone gap rows | no further D-010 legacy research; no new gap row |
| OQ10-17 | Stage 1 reads outside P2 and P3 | process only; the authorised Stage 1 scope was applied |
| OQ10-18 | answer the Stage 0 sheet now or later | ADMINISTRATIVELY RESOLVED / ANSWERED (SC-01) |
| OQ10-19 | representation while OQ9-13 is open | boundary confirmed by ChatGPT for the requirements discussion; applied |
| OQ10-20 | "S&R zone" against D-013's "Entry zone" | applied as a naming note (section 1); nothing decided for D-013 |

### 9.2 Unknowns and evidence points

- INTERPRETATION (Claude): UK-01 is addressed by RQ10-01 and UK-02 by RQ10-05 (Rama's decisions); UK-03 to UK-13
  are unchanged, handled as ChatGPT's Step 2, section 2.3, sets out (kept open; not asked again).
- INTERPRETATION (Claude): U10-01 and U10-02 are addressed for D-010 by RQ10-01 (U10-01 restates OQ9-25, which stays
  OPEN; section 9.3). U10-07: the references DECIDED (RQ10-02), their use OPEN (OQ9-20). U10-08: RQ10-04 treats
  support and resistance alike, as positions relative to CMP; the trade side stays OQ9-18. U10-03 to U10-06 and
  U10-09 to U10-12: unchanged. U10-13: a Stage 1 snapshot (Step 2, section 2.2).

### 9.3 D-009's questions (D-009's record is not edited)

- OQ9-25 ("Does V2 use S&R points, zones or both?"): its who-decides cell names "D-010 (zones)". INTERPRETATION
  (Claude): RQ10-01 bears on it. ChatGPT's Step 2 (section 3) says not to close or answer OQ9-25 from the D-010
  answers, TS_D010_Authorise.txt, Part 2, keeps OQ9-01 to OQ9-25 OPEN, and Rama's answers did not address it, so it
  stays OPEN in D-009's record.
- OQ9-13, OQ9-14, OQ9-15 to OQ9-18, OQ9-20 to OQ9-22 and OQ9-24 stay OPEN, as section 10 places them.

## 10. Cross-topic boundaries and dependencies

| Item | Topic | Status | Relation to D-010 |
|---|---|---|---|
| Whether and how S&R informs the trade decision; "nearest"; SL / target fit; short-side symmetry; strategy use; signal handling with missing data; the record per signal | D-009 | DRAFT RECORDED - NO REQUIREMENT DECIDED (DR-076) | OQ9-13, OQ9-14, OQ9-15 to OQ9-18, OQ9-20 to OQ9-22; RQ10-01 to RQ10-05 grant no decision authority |
| Strength or confidence of a reference | D-011 | not started (authorised next, W-12) | P10-03; BK-80 |
| Age, freshness or decay of a reference | D-012 | not started (authorised after D-011, W-12) | P10-03; BK-80 |
| Entry zone, breakout / breakdown, retest / reclaim, entry timing | D-013 to D-016 | not started | RQ10-05's next reference and the naming note (BK-81) |
| Score / ranking; the final Trade / Reject | D-017; D-018 | not started | no D-010 effect (Rama's bound) |
| SL, targets and R:R | E-005 to E-008 | not started | whether the next reference serves as a target is not decided here (RC-02: "not a guaranteed destination or prediction") |
| Timeframes | D-007 | BLOCKED / NOT CLOSED (DR-073) | OQ10-05; no hierarchy |
| Formations | D-006 | DEFERRED / NOT CLOSED (DR-072) | OQ10-04 |
| Market and candle data; the instrument file; listing and corporate-action data | K-009, K-010 | not started | OQ10-11, OQ10-13; BK-77, BK-79 |
| Audit state and retention | K-008, K-011 | not started | OQ10-12; P10-06 |
| The legacy zone mechanisms | J-003, J-013 | not started (Route B topics) | disposition is theirs; D-010 keeps, modifies or revives nothing |
| S&R architecture | O-006 | Section O after A to N | no D-010 effect |
| Testing | Section N | not started | none proposed here |

## 11. Not in this record

- No design or implementation decision, no owner moved, no VP ratified, and no frozen manual, erratum, companion or
  earlier decision changed.
- No legacy-source access (DR-073); NR9-01, NR9-05 and NR9-11 are not researched by Route B.
- No D-006, D-007 or D-009 status change. D-011 and D-012 are authorised to start after this record is published
  (W-12); neither is started by it.

## 12. Course of the topic (times IST, 30-Oct-2026)

| Time | Step |
|---|---|
| 10:40 | ChatGPT's Stage 0 prompt, checklist and blank answer sheet |
| by 11:05 | Claude's three Stage 0 files (DV-01 to DV-03 disclosed in output 1, section 7) |
| 11:17 | ChatGPT's Stage 0 review: "STAGE 0 ACCEPTED WITH DISCLOSED PROCESS DEVIATIONS."; Stage 1 authorised within limits |
| by 11:44 | the Stage 1 evidence sheet and read log |
| 13:39 | Rama's answers, the chart, the CSV and ChatGPT's Stage 1 review |
| by 13:55 | the supplemental register, the open-items file and the change-control note |
| 14:13 | ChatGPT accepted the supplemental package (with its 242.69 correction) |
| 14:19 to 14:31 | OQ10-18 administratively resolved (Revision 1, SC-01); accepted |
| 14:35 to 14:45 | the Stage 1 closure record; accepted; ChatGPT's Step 2 reconciliation |
| 15:05 to 15:19 | the clarifications RC-01 to RC-03 (Revision 1 corrects RC-03b's heading); accepted |
| 15:21 to 15:46 | the package manifest; its heading corrected on ChatGPT's instruction |
| 16:00 | TS_D010_Authorise.txt: the permanent workflow rule and Stage 2 authorised |
| 16:05 to 16:28 | the Stage 2 status record and plan; reads R2-01 to R2-07 |
| about 16:31 | Rama's answers in the question tool |
| about 16:40 | the follow-up on the reversed role wording and on the review step |
| 17:55 | the follow-up answers, relayed from ChatGPT with "Continue from where you left" |
| from 18:00 | this record and the storage package; the independent review; the storage action (DR-077, DR-078) |

## 13. Status

- Route B in force (DR-074, W-11); option C in force (DR-073).
- D-006: DEFERRED / NOT CLOSED. D-007: BLOCKED / NOT CLOSED. D-008: V2 REQUIREMENTS RECORDED (DR-075). D-009: DRAFT
  RECORDED - NO REQUIREMENT DECIDED (DR-076); OQ9-01 to OQ9-25 OPEN.
- D-010: V2 REQUIREMENTS RECORDED (DR-077): RQ10-01 to RQ10-05 DECIDED at requirement level; the rest OPEN or
  PROPOSED; not frozen.
