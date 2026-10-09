# SECTION A END: DECISION PAPER (CLOSED)

Status: CLOSED 2026-10-09. Rama answered on 09-Oct (section 8); his decisions are recorded in V2_DECISION_REGISTER.md as DR-027 to DR-033, and the S-1 scheduling moves with the close of Section A as DR-034. Revision 1 (after ChatGPT's value-added review of 09-Oct-2026) is the version that was put to Rama; sections 1 to 7 are kept as presented, so the options and recommendations below show what he chose from, not what was decided. Rama decided; ChatGPT's review alone decided nothing (V2_DESIGN_LOG W-6).
Date: 2026-10-09
Place at: docs/agenda/a-section-end/SECTION-A_END_decision_paper.md
Why this step: DR-013 records that the V2 purpose and scope (VP-01, VP-02) "are ratified at the end of Section A". Three more proposals carry an end-of-Section-A ratification point (VP-08 count, VP-37, VP-41). With A-011 frozen as legacy only (DR-026), nine proposals still name a finished Section A topic as a ratification point. ChatGPT's A-011 review (09-Oct, section 7) asked for these to be presented "as a separate decision step", not frozen with the manual.
Sources: V2_DESIGN_LOG.md (VP and Q rows in the pre-decision baseline pushed in 5ab0034; the decisions themselves were pushed later, in 7b6e6de); V2_DECISION_REGISTER.md (DR-001 to DR-026, the same baseline); the frozen A-001 to A-011 manuals and their V2 inputs; Rama's checklist answers in docs/inputs; broker and exchange pages accessed on 2026-10-09 (section 6).
Labels: FACT; HISTORICAL (Rama); UNKNOWN / NOT ESTABLISHED; CURRENT EXTERNAL FACT (with the date accessed); INTERPRETATION / DESIGN RATIONALE (Claude's reasoning for a recommendation, not measured); PROPOSED (a policy for Rama to choose).
Security: no credential values, account IDs, mailbox addresses or host addresses.

## 1. What this step decides, and what it does not
- Decides: five V2 decision areas, D-1 to D-5 (section 3). Each answered decision becomes its own register row (DR-027 onward); the scope area's separately answerable parts (D-2e ETFs, D-2f SME) get rows of their own, so that any one can later be superseded alone.
- Records: scheduling changes, S-1 (section 4). They change ownership or the ratification point only, not proposal content.
- Does not decide: anything owned by Sections B to O (mechanics, formulas, thresholds, the state machine, the kill policy, the login method). A ratified intent does not authorise implementation: the agenda, the post-agenda legacy audit (DR-016) and frozen norms come first (project instructions, section 2). No frozen legacy manual changes in this step.

## 2. Section A in brief (inputs to these decisions)

| Topic | Frozen as | Bearing on this step |
|---|---|---|
| A-001 System purpose | DR-013 (+ DR-015) | Legacy: a headless, automated trading application for NSE equities fed by Chartink, with Zerodha as broker; intraday the core mode, with delivery handling; unattended, with operator safety controls. V2 purpose, scope and operator boundary left PROPOSED (VP-01, VP-02, VP-04). |
| A-002 Trading objectives | DR-014 | Legacy objectives; Rama's V2 ideas VP-08 to VP-17 (about six strategies in VP-08). |
| A-003 Trading hours | DR-018 | Time model; VP-16, VP-19 to VP-26. |
| A-004 Instruments | DR-019 | Legacy series filter {eq, be, n, bt, il}; Silver and Gold ETFs traded (Rama); SME inclusion UNKNOWN (Q-21). |
| A-005 Capital model | DR-020 | Risk/SL-based quantity with a capital ceiling (VP-13). |
| A-006 Broker model | DR-021 | Zerodha facts; Q-27 and Q-28 answered (MIS for intraday; CNC then OCO GTT). |
| A-007 Product types | DR-022 | Legacy placed only MIS and CNC; VP-41. |
| A-008 Strategy model | DR-023 | 16 legacy YAMLs in about five condition families; Rama: about six for V2. |
| A-009 Configuration model | DR-024 | Configuration proposals moved to B-002 and configuration design. |
| A-010 Secrets and environment | DR-025 | Zerodha only, live only (Rama Q35); Q-29 (b) PROPOSED with M-005 conditions. |
| A-011 System states and lifecycle | DR-026 | Operator halt, kill release and restart evidence; VP-57 to VP-65; Q-33 answered (a). |

## 3. Decisions for Rama

### D-1 V2 purpose (VP-01; DR-013)
- Current proposal (VP-01): "An automated, headless, live-only trading system for NSE equities via Zerodha that consumes Chartink scanner signals and, through its configured controls and decision logic, validates, decides, executes, manages, reconciles and reports intraday and positional trades."
- HISTORICAL (Rama, A-001 checklist): "An Fully headless automated system which trades on behalf of me"; "Entire system must be automated right from headless system starting > ... > Prepare the system for next trading day"; "new system must have Live capability only" (DR-001).
- What Section A changed:
  - A-007 named the two product paths intraday and delivery; "positional" is no longer used.
  - Rama prefers a headless daily login (A-010 Q-29 (b); A-011 Q36, "Manual not required in new system"). ChatGPT (A-011 review): a user preference, not a compliance finding. Zerodha staff replies say exchange rules need a manual login at least once a day and advise against automating it (VP-36). M-005 decides.
- Meaning of two words (ChatGPT review, point 1):
  - "Live-only" means no paper-trading operating mode (DR-001). It does not prohibit development, replay, scenario testing or pre-live validation; how decisions are validated before they risk capital stays open (Q-01).
  - "Headless" means no routine manual operation. It gives no permission to automate a login method that the broker or exchange does not allow; M-005 owns the daily login method under the rules current at that time.
- Options:
  - A (recommended): ratify with the updates above: "intraday and delivery trades", and the meaning of "live-only" and "headless" written into the decision.
  - B: ratify VP-01 exactly as written.
  - C: defer.
- Proposed register text (option A): "V2 purpose: an automated, headless, live-only trading system for NSE equities via Zerodha that consumes Chartink scanner signals and, through its configured controls and decision logic, validates, decides, executes, manages, reconciles and reports intraday and delivery trades. 'Live-only' means no paper-trading operating mode (DR-001); it does not prohibit development, replay, scenario testing or pre-live validation (Q-01). 'Headless' means no routine manual operation; it gives no permission to automate a login method the broker or exchange does not allow, and the daily login method is decided at M-005 under the rules then current."

### D-2 V2 scope (VP-02; DR-013)
Current proposal (VP-02): "NSE equity only; Zerodha only; Chartink sole signal source; intraday and positional, with product types finalised at A-007. Decide whether ETFs (traded in legacy) and SME are in scope (A-004)."

ETF and SME inclusion are V2 scope choices. Broker mechanics inform them but do not dictate them.

#### D-2a to D-2c: market, broker and signal source

| Part | Proposed scope | Evidence | Recommendation |
|---|---|---|---|
| D-2a Market | NSE cash equity only: no F&O, currency or commodity trading; no BSE; no index trading, meaning no trading of an index directly or through derivatives. NSE-listed ETF units, index ETFs included, are a separate question (D-2e) | HISTORICAL (Rama, A-004): "Exchange is unchangeable (NSE is fixed)", "F&O not allowed (No chartink support & Internal signal generation/Greek Calculation not possible)"; no F&O or index trades in legacy | Ratify |
| D-2b Broker and account | Zerodha only; one Zerodha account trades in production. Nothing is implied for development or pre-live testing, which Q-01 and VP-56 own | HISTORICAL (Rama, A-010 Q35): "YES, 100% Future system uses Zerodha only + Live Only [So No more: AngelOne + Paper mode]". FACT (A-010 section 4): the legacy account registry required exactly one primary account; HISTORICAL (Rama): only one account was loaded for trading | Ratify |
| D-2c Signal source | Chartink scanner alerts only; no internal signal generation | HISTORICAL (Rama, A-004): "System 100% relied or consumer of Chartink signals only"; VP-14 (end-of-day capture) also uses Chartink | Ratify |
| D-2d Product paths | As D-4 | A-007 | Per D-4 |

#### D-2e ETFs (answered separately)
- HISTORICAL (Rama, A-004): Silver and Gold ETFs were traded in legacy. UNKNOWN: how they passed the legacy series filter (Q-21); the legacy ETF path is not established.
- CURRENT EXTERNAL FACT (Zerodha Z-Connect ETF article, published 2012, page last modified 02-Feb-2024, accessed 09-Oct-2026): ETFs are bought with CNC for investing and traded intraday with MIS. Zerodha's support pages (accessed 09-Oct-2026) say intraday can be blocked for any instrument under its risk policy, so MIS availability is per instrument and checked at order time (VP-41).
- FACT (A-004): ETFs were excluded from NSE's 1-paisa tick change, so ticks come from the broker's instrument dump (VP-28).
- INTERPRETATION / DESIGN RATIONALE (not measured): a scanner condition written for stocks can produce a signal on an ETF for reasons that may not fit the strategy, because an ETF tracks an index or a commodity. This does not mean ETFs are unsuitable.
- Options:
  - (i) In scope for every strategy.
  - (ii) In scope, allowed per strategy: each strategy.yaml says whether it accepts ETFs; the default is not allowed.
  - (iii) Out of scope.
- Recommendation (PROPOSED policy): (ii). ETFs stay possible where a strategy is designed for them, without every strategy receiving them by default.

#### D-2f SME, NSE Emerge (answered separately)
- CURRENT EXTERNAL FACT (Zerodha support, accessed 09-Oct-2026): SME scrips must be traded "in multiples of their lot size". NSE's EMERGE trading-lot page gives standardised lots by price band, for example 4,000 shares at Rs 25 to 35 (about Rs 1.0 to 1.4 lakh a lot). The table is marked "Updated on: 03/01/2023"; it is not verified here as the complete current list.
- UNKNOWN: whether Chartink covers SME symbols (no official statement was found in the sources checked; that is not proof that it does not). Whether the legacy universe ever admitted or traded SME symbols is also UNKNOWN: "no SME trade was reported" is not proof that SME was excluded (Q-21).
- INTERPRETATION / DESIGN RATIONALE (not measured): lot multiples may conflict with a risk-based quantity (VP-13) or the per-trade capital ceiling. A quantity below one lot would be rejected, and one lot can exceed the ceiling. Zerodha's MIS pages list low liquidity and narrow circuit limits as reasons to block intraday, without naming SME. None of this proves that SME trading cannot work.
- Options:
  - (i) In scope.
  - (ii) Out of scope; a later change needs a new register row.
- Recommendation (PROPOSED scope choice): (ii).

#### Proposed register text (recommended options; three rows)
- D-2 (a to d): "V2 scope: NSE cash equity only (no F&O, currency or commodity trading; no BSE; no trading of an index directly or through derivatives); Zerodha only, with one Zerodha account trading in production (development and pre-live testing are left to Q-01 and VP-56); Chartink scanner alerts are the only signal source (no internal signal generation); two product paths as decided under D-4."
- D-2e: "ETFs (NSE-listed ETF units, index ETFs included) are in scope only for strategies whose strategy.yaml allows them; the default is not allowed."
- D-2f: "SME (NSE Emerge) securities are out of scope; including them later needs a new register row."

### D-3 Strategy count (VP-08, count part)
- Current proposal (VP-08): "at most 6 strategies (legacy had 16)". The rest of VP-08 (own filtration, S&R, admission) is owned by C-001, D-003, D-009 to D-017 and J.
- HISTORICAL (Rama): "Future V2 may use a maximum of approximately six strategies" (A-002, restated in A-008 Part 14.1); "Future V2 should not blindly carry the legacy 16-strategy model" (14.9); "Almost duplicate of some strategies with different names ... meaning no distinct unique strategy" (Q32.3).
- FACT (A-008 section 17): 16 YAMLs, being 15 trading definitions plus a shadow, in about five condition families, with one scanner-condition duplicate and several scanner subsets.
- What is counted: active strategy definitions in the V2 configuration, not scanner alert names or historical YAML files. A retired strategy is not enabled. Whether it is deleted, archived or kept as non-executable history is a configuration-governance question (B-002 and configuration design), not part of the count.
- Options:
  - A: a hard cap of six active strategies; a seventh needs a new register row. This is a stricter reading of "approximately six", proposed for governance; it is not Rama's wording.
  - B: "approximately six" recorded as a target, with the final number set during strategy review (D-003 / D-017).
- Recommendation: A, for governance clarity: a fixed ceiling that changes only by a recorded decision. B is equally consistent with Rama's words.
- Under either option: the 16-strategy model is not carried forward. The admission test (no subset or near-copy of another strategy) is written at D-003.
- Not settled by D-3: which strategies, their scanner conditions (VP-44), filtration (VP-03), S&R and R:R.

### D-4 Product intent (VP-37, VP-41)
- HISTORICAL (Rama, in order):
  - A-007 checklist 22.4 (27-Sep): "Basically we are going to use only 2 windows (Intraday: BO/CO & Delivery: GTT) ignore incorporating other windows like NRML, SL, SL-M etc [No complications]". The aim was an automatic SL and target with each entry (Q-32). BO was discontinued in March 2020, and CO attaches only a stoploss (A-007).
  - Q-27 (answered 06-Oct): A, "Every intraday trade uses MIS; if the stock is not eligible for MIS, reject the signal rather than silently changing the product." Q-32 was closed from it (ChatGPT A-007 review, point 3).
  - Q-28 (answered 06-Oct): A, "CNC buy → OCO GTT for SL + Target".
- Records to correct when D-4 is answered, from Rama's later answers:
  - VP-37 still carries "CO optional, subject to eligibility" from A-006, which the 06-Oct Q-27 answer replaced.
  - VP-37 summarises 22.4 as "NRML, CO, BO and other paths not used". That was Claude's reading after A-007 section 3 (A-007_v2_design_inputs.md section 4), not Rama's words.
  - SL and SL-M are order types, not products. "Ignore SL, SL-M" is read as "no extra product paths", not "no stop orders" (A-007 inputs).
- FACT (A-007 section 5, A-008): legacy placed only MIS and CNC; all three delivery strategies were LONG.
- CURRENT EXTERNAL FACT (Zerodha support, accessed 09-Oct-2026): "You can short-sell stocks using intraday orders only"; a CNC sell without the shares in holdings is rejected ("RMS:Rule: Check holdings"). So delivery is long only.
- What the rule does not promise (ChatGPT review, point 4):
  - "The broker's order-time acceptance is final" means the broker is authoritative on whether it accepts the actual order. A pre-check does not guarantee acceptance, and an accepted entry does not guarantee a protective order. The eligibility pre-check, the entry-order outcome and the GTT outcome are separate events.
  - The intended delivery sequence is not proof that an OCO GTT is created, accepted, or covers the filled quantity. These are owned by G/H and must be specified and tested there before implementation:
    - partial fills;
    - GTT creation and validation;
    - rejection, modification and cancellation;
    - residual exposure.
- Options:
  - A (recommended): ratify the text below.
  - B: ratify with changes Rama names.
  - C: defer.
- Proposed register text (option A): "Two product paths only. Intraday: every intraday strategy requests MIS; instrument eligibility is checked against current broker data, and the broker is authoritative on whether it accepts the actual order; a signal whose instrument cannot be traded as MIS is rejected with a recorded reason and never converted to CNC. Delivery: long only; a normal CNC buy, then, after the confirmed fill and for the actual filled quantity, a broker-side OCO GTT intended to hold the SL and the target. CO, BO, NRML and MTF are not used. The eligibility pre-check, the entry-order outcome and the GTT outcome are separate events; this decision does not guarantee that a protective order exists. Owned elsewhere: protective-leg mechanics and the SL order type (G-001, G-010 to G-012, H-011); partial fills, GTT creation, validation, rejection, modification, cancellation and residual exposure (G and H, specified and tested before implementation); Zerodha's GTT rules documented and validated before implementation (G-002); the eligibility pre-check mechanism (D-002); the same-symbol rule (VP-12, C-005 / F-007)."

### D-5 Operator boundary (VP-04)
- Current proposal (VP-04): "routine operation is fully automated; the only manual actions are kill-switch release and operator interrupt, including a simple halt command instead of stopping the VM from the cloud console (A-002)". VP-04 also reads the project instructions' "zero manual intervention" (5.5) as targeting routine moves, renames and triggers.
- HISTORICAL (Rama, A-001): "Automated Operations, But some current system has some features like 'Removal' of soft/hard kill the system manually"; controls exist for "Manual interruption whenever required & which is necessary".
- What Section A added (A-011): legacy's only operator halt was systemctl stop, which also stopped exit management; an operator SOFT_KILL route was dropped on 25-Jul-2026. Later proposals add other manual actions:
  - an entries-blocked control (VP-58);
  - a kill-clearing policy (VP-61, Rama's choice at I-002);
  - a manual procedure for a position still open after the square-off (VP-63);
  - sensitive-change approval (VP-52);
  - a manual login fallback if M-005 requires one (VP-36);
  - the human-order policy (VP-60).

  So "the only manual actions are kill-switch release and operator interrupt" is narrower than those topics may need.
- Meaning of "reported on Telegram and email" (ChatGPT review, point 5). In this proposal it means notification only: each manual action is reported on both channels when it is requested and when it takes effect.
  - It is not a human acknowledgement step and not an authorisation step. Authorisation comes from the authenticated control itself.
  - Acknowledgement, timeouts, channel failure and any out-of-band approval (VP-52) are designed at I and M.
  - Telegram and email delivery is never the security control, and neither channel is assumed to be always available (VP-34).
- Options:
  - A (recommended): ratify the principle and leave the list to its owners. Routine operation is automated end to end. Manual actions are limited to a named list, each with a supported, authenticated and audited mechanism, and each reported on both channels. There are no VM console stops and no direct database edits. The list is set at I-001, I-002, I-013 and M-010. Candidates:
    - halt;
    - block new entries;
    - kill release;
    - the residual-position procedure;
    - sensitive-change approval;
    - the login fallback, if M-005 requires it.
  - B: ratify VP-04 as written.
  - C: defer VP-04 to I and M (S-1 then moves its A-011 point to O-004).
- Proposed register text (option A): "Operator boundary: routine operation is automated end to end; manual actions are limited to a named list, each with a supported, authenticated and audited mechanism and each reported on Telegram and email (notification only: authorisation, acknowledgement, timeouts and channel failure are designed at I and M; channel delivery is never the security control); no VM console stops and no direct database edits; the list and its mechanisms are set at I-001, I-002, I-013 and M-010."

## 4. S-1 Scheduling changes (ownership and scheduling only)
These rows change ownership or the ratification point, never proposal content. Each was checked against V2_DESIGN_LOG and V2_DECISION_REGISTER as pushed in 5ab0034 (the pre-decision baseline). None is a Section A system-contract item: each is owned by the topic it moves to, so no substantive decision is being postponed only because A-011 was named. VP-37 and VP-41 are decided only through D-4; their technical details stay with G/H, D-002 and their other named owners.

| VP | Ratification point now | Proposed | Why |
|---|---|---|---|
| VP-04 | A-011 (states) and I (safety) | If D-5 is A: DECIDED (principle), with the list at I-001, I-002, I-013, M-010. If B: DECIDED as written. If C: O-004 (states), I (safety) | A-011 froze legacy only; the state machine is O-004 |
| VP-06 | "As listed": inside the text, "at A-003 and B" (start/stop by calendar) and "at A-010 and M-005" (daily token) | "at B" and "at M-005" | A-003 and A-010 froze legacy only |
| VP-16 | A-003, B | B-001, B-008 to B-010 | Calendar, start and day lifecycle belong to Section B (with VP-62) |
| VP-19 | A-003 follow-up in B-008 / B-009, A-011 | B-008, B-009, I-001 | A-011's part was the cut-off versus SOFT_KILL semantics, now I-001 (VP-61) |
| VP-22 | B-010 to B-012, A-011 | B-010 to B-012 | Carried with VP-62 |
| VP-45 | C-001, C-002, C-010, A-010, B-002 / configuration design (build phase) | C-001, C-002, C-010, B-002 / configuration design (build phase) | A-010's part (account identifiers in names) is carried by VP-51 and VP-55 |
| VP-47 | B-001, B-002, L-003, I, A-011 | B-001, B-002, B-012, L-003, I | A-011's part was a restart while positions are open, now B-012 and I-012 |
| VP-52 | M-003, L-008, A-011 | M-003, L-008, B-012 | A-011's part was restart timing for a change, now B-012 |
| VP-54 | B-001, B-002, A-011, I, K, L-003 | B-001, B-002, O-004, I, K, L-003 | A-011's part was the written state machine, now O-004 (VP-57) |

## 5. Answer sheet (for Rama; labels as in section 3)
- D-1 purpose: A ratify with updates / B ratify as written / C defer
- D-2a to D-2c (NSE cash equity only; Zerodha only, one production account; Chartink only): ratify / change / defer
- D-2e ETFs: (i) all strategies / (ii) per strategy, default not allowed / (iii) out of scope
- D-2f SME: (i) in scope / (ii) out of scope
- D-3 strategy count: A hard cap of six / B approximately six as a target
- D-4 product intent: A ratify / B ratify with changes / C defer
- D-5 operator boundary: A principle / B as written / C defer
- S-1 scheduling: accept / change

## 6. Current external facts (accessed 2026-10-09; each supports only the text quoted)
- Zerodha support, "Short sell shares in EQ and F&O segments": "You can short-sell stocks using intraday orders only."
- Zerodha support, "RMS:Rule: Check holdings": "This error occurs if a CNC sell order is placed without having those particular shares in holdings."
- Zerodha support, "Intraday orders not allowed for some stocks" and "MIS not allowed on certain scrips": "Zerodha may block intraday orders for any instrument based on its risk management policies". Reasons include volatility, low liquidity, narrow circuit limits, high margin requirements and regulatory categories (Trade-to-Trade, GSM). Neither page mentions SME stocks or ETFs, and neither shows an update date.
- Zerodha support, "How do I trade scrips listed on NSE Emerge and BSE SME?": "you must trade them in multiples of their lot size, and you cannot buy a single share or any odd quantity." The page gives no product or intraday rule and no update date.
- NSE, "EMERGE SME trading lots": standardised lot sizes by price band, for IPOs and for secondary-market trading (10,000 shares up to Rs 14, down to 100 shares above Rs 1,000). The table is marked "Updated on: 03/01/2023"; it is not verified as a complete current list.
- Zerodha Z-Connect, "ETF – Exchange Traded Funds" (published 2012, page last modified 02-Feb-2024): CNC for investing; for intraday, "use the product type as MIS, but you will have to square it off before the end of the day". Per-instrument MIS availability still follows Zerodha's risk policy.
- Chartink: no official statement was found on whether its scanners cover NSE Emerge (SME) symbols; this is not established from the sources checked.

## 7. After Rama answers
1. Each answer is recorded as given, with any qualification Rama adds.
2. One register row is written per distinct decision (DR-027 onward): D-1, D-2 (a to d), D-2e, D-2f, D-3, D-4 and D-5, as answered. A deferred item gets no row.
3. Only the matching proposals are marked DECIDED in V2_DESIGN_LOG; parts owned elsewhere stay PROPOSED or OPEN.
4. S-1 is applied separately. A closing row for Section A records it as an ownership and scheduling change only.
5. The paper, the register, the design log and the agenda references are checked against each other, then scanned for identifiers, addresses and secrets before the files are sent.
6. No frozen legacy manual changes.

Status and boundary: the Section A decisions are CLOSED and recorded in DR-027 to DR-034. Proposals not explicitly decided here remain PROPOSED or OPEN in V2_DESIGN_LOG.md and with their owning topics. These decisions do not authorise implementation; the post-agenda legacy audit (DR-016) and the remaining governance and design steps still apply. These questions remain open: Q-01 (pre-live validation), Q-05 (environments), Q-11 (fresh credentials), Q-21 (the legacy ETF and SME path; the V2 scope itself is decided in DR-029 and DR-030) and the Section B to O items carried in V2_DESIGN_LOG.

## 8. Rama's answers (09-Oct-2026) and the register rows

| Item | Rama's answer, as given | Register row |
|---|---|---|
| D-1 purpose | Option A, ratify with updates | DR-027 |
| D-2a to D-2c scope | Ratify all three | DR-028 |
| D-2e ETFs | His own answer: "Since Chartink is the Only signal source, So Chartink fires signals including ETF (Except F&O) that signals only be processed in system, So Zerodha Instrumesnts allows ETF then ETF=Normal Equity shares for ordering, So no special treatment required for ETF". This matches option (i): ETFs in scope for every strategy, treated as normal equity shares. The recommendation (ii) was not taken | DR-029 |
| D-2f SME | First: "Option (ii) Out of Scope, But additional informations as per Q3" (Q3 being his ETF answer). Asked to resolve the two parts, he chose "Treat SME like ETFs", offered as "If Chartink fires it and Zerodha lists it, trade it like a normal share. This puts SME in scope after all." Final: (i), in scope, treated like ETFs | DR-030 |
| D-3 strategy count | His own answer: "Why? I put maximum 6 strategies? Simple once system performs well after implementation/Testing phase with 6 strategies, I'll add more strategies + each new strategy's secondary signal processor, So Time being six strategies (4 intraday with 2 long & 2 short sell + 2 delivery strategies) to aoid much complications till system is tested, So accordingly take a note or do the needful". This was neither option A nor option B: it fixes the initial set and its mix | DR-031 |
| D-4 product intent | Option A, ratify | DR-032 |
| D-5 operator boundary | Option A, the principle | DR-033 |
| S-1 scheduling | Accept | DR-034 |

Consequences noted in the rows, as general rules rather than ETF- or SME-specific ones:
- An SME order must be in multiples of the lot size, so its quantity follows the lot size in the broker's instrument data (VP-13, VP-28, PROPOSED), and a quantity below one lot cannot be placed.
- Intraday ETF and SME signals stay subject to MIS eligibility (DR-032).
- Whether Chartink fires on SME symbols is UNKNOWN.

V2_DESIGN_LOG marks VP-01, VP-02, VP-04 (in principle), VP-08 (count only), VP-37 and VP-41 (intent only) as decided. VP-37's summary of checklist 22.4 is corrected with Rama's words, and the S-1 ratification points are moved. Every other part stays PROPOSED or OPEN with its owner.

## Changes in revision 1 (after ChatGPT's value-added review, 09-Oct-2026)
- D-1: "live-only" defined as no paper-trading mode, with development, replay, scenario testing and pre-live validation not prohibited (Q-01). "Headless" gives no permission to automate a login the broker or exchange does not allow (review point 1).
- D-2: "no index trading" defined as no trading of an index directly or through derivatives, apart from NSE-listed ETF units (D-2e). Evidence labelled by class. The ETF and SME rationales are labelled INTERPRETATION / DESIGN RATIONALE. The NSE table is dated and not presented as a complete current list. The Chartink and legacy-SME unknowns are kept as unknowns. D-2b is limited to production. ETF and SME get register rows of their own (point 2).
- D-3: the hard cap is labelled a stricter governance reading of "approximately six". Option B is "approximately six" as a target, and the former defer option is dropped as covered by B. The count is defined as active strategy definitions, with retirement handling left to configuration governance (point 3).
- D-4: order-time acceptance, the pre-check and the GTT outcome are worded as separate events. A sentence says the intended sequence does not guarantee protection, with partial fills, GTT handling and residual exposure owned by G/H (point 4).
- D-5: "confirmed" replaced by "reported (notification only)", with authorisation, acknowledgement, timeouts and channel failure left to I and M (point 5).
- S-1: re-checked against the pushed design log and register. The ownership-only nature is stated, and VP-04 is handled for each D-5 answer (point 6).
- Sources: each external page is dated by access, with the limits of what it supports (point 7). The answer-sheet labels match section 3 (point 8).

## Changes after closure (09-Oct-2026, ChatGPT's final QA note; documentation only)
- Title marked CLOSED; the pre-answer "Status and boundary" paragraph replaced, with the open questions kept and marked as still open; the 5ab0034 references labelled as the pre-decision baseline. No decision, register row or frozen manual changed.
