# SECTION C END: DECISION PAPER (CLOSED)

Status: CLOSED 2026-10-25. Rama answered on 25-Oct (section 10); his decision is recorded in V2_DECISION_REGISTER.md as DR-065, and the S-1 to S-3 moves and the C-1 corrections with the close of Section C as DR-066. Revision 0, reviewed by ChatGPT on 25-Oct-2026 (SECTION-C_END_CHATGPT_STAGE1_REVIEW_25-OCT-2026.txt, stored beside this paper: "REVIEW ACCEPTED FOR RAMA'S ANSWERS"; no blocker, no change asked), is the version that was put to Rama; sections 1 to 9 are kept as presented, except the closing status-and-boundary paragraph, so the options and recommendations below show what he chose from, not what was decided. Rama decided; ChatGPT's review alone decided nothing (V2_DESIGN_LOG W-6).
Date: 2026-10-25 (IST)
Place at: docs/agenda/c-section-end/SECTION-C_END_decision_paper.md
Why this step: all eleven Section C legacy manuals are frozen (DR-053, DR-055 to DR-064), and each of those rows ends "No V2 decision is made". Ten proposals still name a Section C topic among their ratification points. The record reserves three items for this paper: Q-46, on DR-028's scope, to be settled here "with VP-45 and VP-56" (Rama's choice of 21-Oct: "Section C end paper (Recommended)"), and VP-08 and VP-44, which kept C-001 "to be reviewed at the Section C end decision paper" (DR-053; Rama's answer "Keep for Section C end (Recommended)"). Q-20's area names Section C, two audit leads still name Section C owners (Q-37 (f); Q-45 (h), whose owners are only C topics), and four owner cells in V2 inputs name only completed Section C topics. This paper sorts these items, puts to Rama only what is ready, and moves the rest by scheduling only.
Sources (pre-decision baseline): the trading-system-v2 repository, main at 3761e3b (25-Oct-2026 15:39 IST): V2_DESIGN_LOG.md (blob 589ce689, 572 lines), V2_DECISION_REGISTER.md (blob 8afbeaae, 64 rows to DR-064), MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt (blob a9b7bbf2); the Section A and B end papers in the repository (process precedent only; whether they equal the copies Rama attached in ChatGPT is UNKNOWN / NOT ESTABLISHED); the frozen C-001 to C-011 records and the V2 inputs files at the lines cited. No legacy source was read for this paper. Reading record: SECTION-C_END_SOURCE_INVENTORY_AND_METHOD_2026-10-25.txt, section 12. Item-by-item evidence: SECTION-C_END_DISPOSITION_CROSSCHECK.txt. Exact changes after the answers: SECTION-C_END_POST_ANSWER_CHANGE_PLAN.txt.
Labels: FACT (RECORD-DERIVED) for what a stored V2 record says, with its line; FACT (SOURCE-VERIFIED) only as cited from the frozen manual that verified it; HISTORICAL (Rama), his words quoted; INTERPRETATION; KNOWN DOC DRIFT; UNKNOWN / NOT ESTABLISHED; PROPOSED / OPEN. "Line N" is a line of V2_DESIGN_LOG.md at 3761e3b and "register line N" a line of V2_DECISION_REGISTER.md, unless another file is named.
Security: no credential values, account or client identifiers, mailbox or host addresses, session links or person names other than Rama's.

## 1. What this step decides, and what it does not
- Puts to Rama: one substantive decision, D-1: how DR-028's "one Zerodha account trading in production" reads (Q-46). Answered A or B, it becomes its own register row (DR-065), so that it can later be superseded alone. An answer of A or B ratifies the exact proposed register text shown in section 4, unless Rama changes the wording in his answer.
- Records for acceptance: ownership and scheduling moves (S-1: ten proposals' ratification points and one tracking row; S-2: Q-20's area and two audit-lead owners; S-3: four owner cells in V2 inputs) and documentation-only corrections (C-1). These change points, owners or bookkeeping, never the content of a proposal, question or lead.
- Does not decide: which strategies V2 has, scanner conditions, identity or naming schemes, credentials, queue, persistence or expiry design, or any mechanism, value or process owned by Sections D to O. It answers no bookmark or audit lead, and closes no question except Q-46, and that only if D-1 is answered A or B. A ratified reading authorises no implementation (DR-016).
- Does not reopen: DR-053 to DR-064 (the Section C freezes), the Section A and B ends (DR-027 to DR-034, DR-050 to DR-052), or any erratum. No frozen manual, companion or addendum changes.

Three kinds of item (as in the Section A and B papers):
- A substantive decision answers what V2 will do, at the level of intent or scope; it gets a register row.
- A scheduling or ownership change moves where a proposal, question or lead will be decided or met next; its text is unchanged and nothing is decided.
- An unresolved item stays PROPOSED or OPEN with a named owner or step; carrying it forward is not a decision about its content.

## 2. Section C in brief

| Topic | Frozen as | V2 outcome at the freeze |
|---|---|---|
| C-001 Chartink strategy / scanner | DR-053 (errata A-008 E-4, B-002 E-2; its own E-1 and E-2 were accepted at DR-055) | No V2 decision; VP-45 dropped C-001; VP-08 and VP-44 kept C-001 for this paper (Rama); Q-20 and Q-34 unchanged; his input "Ignore existing legacy system's strategies ..." recorded in C-001_v2_design_inputs.md (PROPOSED / OPEN) |
| C-002 Scanner -> webhook | DR-055 (nine errata, among them C-001 E-1 and E-2; its own E-1 was accepted at DR-056) | No V2 decision; VP-45 dropped C-002; Q-46 opened for this paper (Rama); NC-7 kept in C-002 only; C-001's Chartink URL clean-up closed as not needed, not as done |
| C-003 Authentication | DR-056 (four errata, among them C-002 E-1) | No V2 decision; no VP or Q row named C-003, so no move; tracking notes on VP-50, VP-55, VP-56 and Q-36; no V2 inputs file |
| C-004 Validation | DR-057 (errata A-004 E-1, A-009 E-3) | No V2 decision; VP-27 not ratified (Rama: "I prefer Option-B decision later"); V2-1 and V2-2 in C-004_v2_design_inputs.md |
| C-005 Deduplication | DR-058 | No V2 decision; VP-12 not ratified (BK-02); Q-33 and Q-45 (h) annotated; section 7 (bookmarks) started: BK-01 (C-004's VP-27) to BK-09 |
| C-006 Queue | DR-059 | No V2 decision; VP-59 not ratified (BK-18); BK-10 to BK-18 |
| C-007 Signal persistence | DR-060 | No V2 decision; VP-59 not ratified (BK-18); Q-37 (f) annotated; BK-19 to BK-30 |
| C-008 Signal processing | DR-061 (erratum B-009 E-3) | No V2 decision; VP-11, VP-24, VP-31 and VP-59 not ratified (the authorisation, item E); OQ-01 kept as BK-43; BK-31 to BK-43; no V2 inputs file |
| C-009 Signal expiry | DR-062 | No V2 decision; VP-24 and VP-59 not ratified (Rama's Stage 0 decision OQ-04 (a)); BK-44 to BK-47 |
| C-010 Scanner -> strategy mapping | DR-063 | No V2 decision; VP-45 not ratified (OQ-03); BK-27 kept OPEN; BK-48 to BK-50, owners not assigned |
| C-011 EOD / Daily signal routing | DR-064 | No V2 decision; VP-14 not ratified (Stage 0 OQ-04; its cell unchanged); BK-08 and BK-30 kept OPEN; BK-51 to BK-53 |

FACT (register lines 64 to 75; lines 307 to 317). For every topic, the manual's status line, its agenda row and its register row agree on date, revision and DR, and one commit on main published it; those eleven commits are the only commits after the Section B end push (5d575d1). The Master Agenda's goal for Section C is understanding: "Establish exactly how an external signal becomes an internal trading candidate." (agenda lines 284 to 286). As the Section B paper found for Section B, the project keeps legacy fact and V2 proposals in separate layers (W-1, W-6), so the agenda's per-topic "final decision" step (agenda Part 3, item 11) has, in practice, been DEFER for every Section C topic (agenda Part 7).

## 3. How the Section C items were sorted
Readiness test applied to each item, in this order (the Section B paper's test, with Section C topics):
1. Its current ratification point or owner (V2_DESIGN_LOG at 3761e3b) names a Section C topic or "C", or the record reserves it for this paper.
2. Its content is Rama's own recorded instruction, not a Claude or ChatGPT consideration.
3. No recorded dependency or deferral condition is still open.
4. What stays open is mechanism or values that a named later owner can take, not a second policy choice hidden inside it.
An item that passes all four is put to Rama as a decision (with an option to move it instead); one that fails is moved by scheduling only, or left OPEN.

| Item | Points or owners now (line) | Test result | Treatment |
|---|---|---|---|
| VP-08 (the choice of the six strategies) | "Count DECIDED (DR-031); C-001 (kept for the Section C end, DR-053), D-003, D-009 to D-017, J" (216) | Reserved here; fails 3 and 4: which strategies V2 has needs D-003, D-009 to D-017 and J, which DR-031 already names | S-1 |
| VP-44 (their scanner conditions) | "C-001 (kept for the Section C end, DR-053), D-001, D-013 to D-016, E-002" (251) | Reserved here; fails 4: the conditions are those owners' | S-1 |
| VP-11 (every signal reaches a recorded terminal outcome) | "C-008, L-009" (219) | Fails 3: left for the whole-design review (C-008's authorisation, item E) | S-1 |
| VP-12 (one trade per stock per day) | "C-005, F-007" (220) | Passes 2 (Rama's Q-33 answer (a), 09-Oct); fails 3: "left for the whole-design review with F-007" at C-005 (BK-02) | S-1 |
| VP-14 (EOD "analyse today, trade tomorrow") | "C-011" (222) | Fails 3 and 4: C-011 left it to "its designated process" (OQ-04); whether any of V2's strategies trades next day is VP-08's open choice, and VP-25's service window waits on it ("post-market only if VP-14 is adopted", line 232) | S-1, with a new destination (section 5) |
| VP-24 (outside-window signals rejected, not queued) | "C-009, C-008; links VP-11" (231) | Passes 2; fails 3: Rama's own choice at C-009, OQ-04 (a), "Leave for whole-design review" | S-1 |
| VP-27 (remove the static symbol blacklist) | "D-002, C-004" (234) | Fails 3: Rama, "I prefer Option-B decision later" (DR-057) | S-1 |
| VP-31 (daily limit counts filled trades) | "F-010, F-011, C-008, G" (238) | Fails 3: Q-26 open; item E | S-1 |
| VP-45 (Chartink identity) | "C-010, configuration design (build phase)" (252) | Passes 2; fails 3: the identity questions of C-002_v2_design_inputs.md section 4 are open; C-010's OQ-03 | S-1; D-1 reads DR-028 only |
| VP-59 (signals across stop, kill and crash) | "C-006 to C-009, I-009" (266) | Fails 2 to 4: Rama asked for an investigation (A-011 Q31), not a rule; "OPEN: a durable queue or an in-memory queue with closure at restart"; BK-18 | S-1 |
| Q-46 (DR-028 per system or for V2 as a whole) | "To be settled at the Section C end decision paper" (202) | Passes: reserved here; a reading of a decided row that Rama's own words address; what stays open has named owners | D-1 |
| Q-20 (instrument lookup, exchange, unknown symbol) | area "A-004 / C" (176) | Not answered in full: C-008 kept it OPEN after answering its part; the cache users C-004 did not read remain | S-2, with a new destination |
| Q-37 (f) (non-terminal rows after crashes) | owners "C-007, K-003, I-009" (193) | An audit lead for the post-agenda legacy audit (DR-016), not a decision | S-2 |
| Q-45 (h) (in-flight claim evictions) | owners "C-005, C-008" (201) | An audit lead; its code-path part answered, its records questions open | S-2, with a new destination |
| Four V2 inputs owner cells (C-001 row 11; C-002 item 5i; B-007 item 12; B-008 row 2) | only completed C topics | Considerations for owners that froze legacy only; the files have no default owner | S-3, new destinations |
| BK-01 to BK-53 | section 7 (lines 520 to 572) | Reviewed at the topic each names or at the whole-design review (the rule at line 516; W-9) | No change |
| Three companion status lines; the C-011 publication note | C-008 and C-011 folders; section 6 | Documentation only | C-1 |

Searched and not affected (every VP row's last cell parsed and every row searched; every Q row and owner segment; every V2 inputs file's owner cells; register rows DR-001 to DR-052 searched, DR-053 to DR-064 read in full): VP-28, VP-50, VP-55, VP-56 and VP-58 cite Section C manuals only in tracking notes and ratify outside Section C. Q-33 (CLOSED), Q-34 and Q-36 carry Section C notes but no Section C owner. The C-005 to C-007 V2 inputs send items whose owners have completed to the whole-design review by their own header rule, and the section 3 and 6 tables of the C-001, C-002 and C-004 inputs are snapshots at their freezes. Register rows that name a Section C topic as an owner or step (DR-026, DR-031, DR-032, DR-034, DR-052, DR-053, DR-055) stay as written (append only, register line 4); the closing row records the moves, as DR-034 and DR-052 did. Outside Section C and not proposed here: owner cells that name only completed Section A or B topics (B-001_v2_design_inputs.md section 5; B-003_v2_design_inputs.md items 7 and 9), noted for ChatGPT (cross-check, part J). Each item's full row, source line and reasoning: SECTION-C_END_DISPOSITION_CROSSCHECK.txt, parts B to J.

## 4. Questions for Rama
Option sets: Q-46's own row gives the two readings ("Read per system, the two agree; read for V2 as a whole, they do not") and says that "a new register row, referencing it, will record which reading holds"; D-1's options A and B are those readings, and C is a move. No other item needs Rama's choice of content: every other item fails the readiness test or is bookkeeping.

### D-1 DR-028's scope: one production account per system, or for V2 as a whole (Q-46, with VP-45 and VP-56)
- Current record:
  - DR-028 (register line 39), in its words: "Zerodha only, with one Zerodha account trading in production (development and pre-live testing are left to Q-01 and VP-56)".
  - Q-46 (line 202): "Read per system, the two agree; read for V2 as a whole, they do not. DR-028 is not edited; a new register row, referencing it, will record which reading holds. To be settled at the Section C end decision paper (Rama's choice of 21-Oct: "Section C end paper (Recommended)"), with VP-45 and VP-56."
- HISTORICAL (Rama, 21-Oct-2026; C-002_v2_design_inputs.md section 1):
  - To Claude, answering the question on DR-028, verbatim: "Simple, 2 different VM's with 2 different zerodha account (So basically both VM's has their own separate resources like capital, telgram/email credentials, broker accounts, API etc) but what matters means single Chartink website for singal, So identification purposes I'll add zerodha client code in strategy name that's the only thing, So you can assure both system will trade on its own independent to other".
  - To ChatGPT (paraphrased in that file, with placeholders for the values): one Chartink account for two systems on two VMs, told apart by scan names that carry each system's client code; "So ensure how we are going to deal in new system".
- FACT (RECORD-DERIVED; legacy context only): "The address in an alert's webhook URL decided which VM received the alert, and its path segment decided the scanner key" (C-002_v2_design_inputs.md section 3, citing C-002_manual.md sections 4.2 and 7.2).
- What D-1 does not settle:
  - scanner and alert identity across systems, including whether a real client code or a non-sensitive alias appears in names, payloads, logs or files (VP-45, with VP-51 and VP-55; C-002_v2_design_inputs.md section 4, questions 2 to 4, 6, 7, 10 and 11, and ChatGPT's privacy consideration there, item 5c);
  - credentials and secrets per system and per environment (VP-56, VP-50; section 4, question 5; item 5d);
  - detecting and handling a Chartink outage that affects both systems (VP-34; section 4, question 9; item 5h), and whether the Chartink subscription supports the alerts needed (U-10; question 8, with VP-45);
  - whether, when and how a second system is built and deployed (M-003, M-007, O-010), the capital of each (Section F), and pre-live testing (Q-01).
- Options:
  - A: per system. Ratify the reading below: each V2 system trades one Zerodha account in production, and two independent systems, each with its own account, are within DR-028.
  - B: V2 as a whole. At most one Zerodha account trades in production across all V2 systems; a second production system would need a new register row.
  - C: decide nothing now; Q-46 stays OPEN and moves to the whole-design review with VP-45 and VP-56 (scheduling only).
- Recommendation (PROPOSED): A. Rama's own words answer the question as Q-46 puts it, the reading changes no word of DR-028, and everything that stays open has a named owner. C is consistent with the record if Rama prefers to settle the reading together with the identity design. B would run against his stated plan.
- Proposed register text (option A), as DR-065: "DR-028's 'one Zerodha account trading in production' applies per V2 system: each V2 system trades one Zerodha account in production. Two independent V2 systems, each with its own Zerodha account and its own capital, alert credentials and broker API access, sharing only the Chartink login, are within DR-028. DR-028 is not edited; this row records its reading (Q-46). This decision does not decide whether, when or how a second system is built or deployed. Owned elsewhere: scanner and alert identity across systems, including whether a client code or a non-sensitive alias appears in scanner names, payloads, logs or files (VP-45, with VP-51 and VP-55); credentials and secrets per system and per environment (VP-56, VP-50); detection and handling of a Chartink outage that affects both systems (VP-34); deployment and VM operation (M-003, M-007, O-010); capital per system (Section F); pre-live testing (Q-01)."
- Proposed register text (option B), as DR-065: "DR-028's 'one Zerodha account trading in production' applies to V2 as a whole: at most one Zerodha account trades in production across all V2 systems. A second system trading its own account in production needs a new register row that supersedes this reading. DR-028 is not edited; this row records its reading (Q-46). Owned elsewhere: scanner and alert identity (VP-45, with VP-51 and VP-55); credentials and secrets per environment (VP-56, VP-50); pre-live testing (Q-01)."
- Under C there is no register row; the closing row records the move.
- The reason column of DR-065 would quote Rama's answer and his 21-Oct words above, and state that ChatGPT's and Claude's reviews decided nothing (SECTION-C_END_POST_ANSWER_CHANGE_PLAN.txt, part 2).

## 5. Scheduling and ownership changes (S-1 to S-3; ownership and scheduling only)
None of these rows changes the content of a proposal, question or lead, or decides anything. Each was checked against V2_DESIGN_LOG and V2_DECISION_REGISTER at 3761e3b. The C topic in each row froze legacy only ("No V2 decision is made", register lines 64 to 75). Each moved row will carry one tracking sentence, as earlier moves did ("At the Section C close (DR-0nn, scheduling only): ..."); the exact sentences are in the change plan, part 3.

S-1, proposal ratification points:

| VP | Points now | Proposed | Why (record) |
|---|---|---|---|
| VP-08 | Count DECIDED (DR-031); C-001 (kept for the Section C end, DR-053), D-003, D-009 to D-017, J | Count DECIDED (DR-031); D-003, D-009 to D-017, J | Reviewed here, as DR-053 asked. No Section C topic chose a V2 strategy. Rama's 21-Oct input "agrees with DR-031 and DR-032 and changes no decision" (DR-053); DR-031 already says "The legacy 16-strategy model is not carried forward" |
| VP-44 | C-001 (kept for the Section C end, DR-053), D-001, D-013 to D-016, E-002 | D-001, D-013 to D-016, E-002 | Reviewed here, as DR-053 asked. C-001 found no scan conditions in the repository's code and configuration and no new evidence on signal timing (the VP-44 row's C-001 note) |
| VP-11 | C-008, L-009 | L-009, whole-design review | C-008's authorisation, item E: "Leave them for the separately governed whole-design review" (DR-061) |
| VP-12 | C-005, F-007 | F-007, whole-design review (BK-02) | "not ratified at C-005: left for the whole-design review with F-007" (the VP-12 row; DR-058) |
| VP-14 | C-011 | whole-design review | C-011 was its only point; OQ-04: "leave V2 governance to its designated process" (DR-064). New destination; the alternative is the later D topics to which C-011's Stage 0 sheet sends the route's downstream ("later D topics", OQ-02; by title D-014 to D-016) |
| VP-24 | C-009, C-008; links VP-11 | whole-design review; links VP-11 | Rama's OQ-04 (a) at C-009, "Leave for whole-design review" (DR-062); item E (DR-061) |
| VP-27 | D-002, C-004 | D-002 | Rama left it "pending for a later decision ... (no later review named by Rama; D-002 remains in it)" (DR-057); BK-01 stays OPEN |
| VP-31 | F-010, F-011, C-008, G | F-010, F-011, G, whole-design review | Item E (DR-061); Q-26 (area F-011 / G) stays OPEN |
| VP-45 | C-010, configuration design (build phase) | configuration design (build phase), whole-design review | C-010's OQ-03: "VP-45 remains a ratification point for the later whole-design review" (DR-063) |
| VP-59 | C-006 to C-009, I-009 | I-009, whole-design review (BK-18) | "Not ratified at C-006: left for the whole-design review (bookmark BK-18)", and the same at C-007 to C-009 (DR-059 to DR-062) |

S-1 also adds a tracking row, "Whole-design review (after the agenda)", to V2_DESIGN_LOG section 4, as DR-037 added "Configuration design (build phase)": it lists the proposals, leads and V2 inputs items whose point is the review that W-9 schedules once the agenda is complete, so that they stay accountable; it schedules nothing earlier and decides nothing. Claude's proposal; S-1's moves stand without it.

S-2, question and audit-lead owners (V2_DESIGN_LOG section 2; each item stays OPEN, the leads for the post-agenda legacy audit, DR-016; nothing is answered by these moves):

| Item | Owners now | Proposed | Note |
|---|---|---|---|
| Q-20 (area) instrument lookup, exchange, unknown symbol | A-004 / C | A-004 / E-012, K-009 | C-008 kept Q-20 OPEN after answering its part (line 176). Its open part is the instrument-cache users C-004 did not read ("the other cache users were not read", C-004_v2_design_inputs.md section 3); E-012 and K-009 are where VP-28, the instrument master, ratifies. The fixed "NSE" stays an audit lead in C-001_manual.md section 15. New destination; the alternative is "A-004 / audit", as Q-21 |
| Q-37 (f) which signal rows crashes left non-terminal, and how they were cleaned up | C-007, K-003, I-009 | K-003, I-009 | C-007 recorded Rama's "No, never" and the code paths; which rows were left stays NOT ESTABLISHED (C-007 U-02) |
| Q-45 (h) the in-flight sweeper's evictions on production | C-005, C-008 | I-008 | Both owners are C topics (B-009 was dropped at DR-052). The code-path part is answered at the pin; "whether evictions occurred and how long claims were held remain records questions" (the row's C-005 note). I-008 (Webhook failure) is the next topic that meets the subject by title, the rule the Section B paper used for its sole-B leads. New destination; the alternative is the whole-design review, where the V2 side goes (C-005_v2_design_inputs.md items 3 and 4; BK-03, BK-06) |

S-3, owner cells in V2 inputs files (PROPOSED / OPEN considerations; owner text only, as the Section B close corrected six cells; each file's status line records the change):

| File and cell | Owner now | Proposed | Why (record) |
|---|---|---|---|
| C-001_v2_design_inputs.md section 2, row 11 (how V2 receives Chartink webhooks: the transport, and where the shared secret travels) | C-002, C-003 | whole-design review (with VP-50, VP-55, VP-56) | Neither topic took it. C-003: "V2's own authentication design is not this step's subject (DR-001, Q12 (d)); VP-50, VP-55 and VP-56 are named as tracking destinations only" (C-003_RESEARCH_NOTES_AND_OPEN_QUESTIONS_22-OCT-2026.txt lines 269 and 270) |
| C-002_v2_design_inputs.md section 5, item i (transport security) | "(C-003)"; "TLS and authentication are C-003's subject" | "(whole-design review, with VP-50, VP-55, VP-56)"; "C-003 recorded the legacy only (DR-056), and V2's TLS and webhook authentication go to the whole-design review" | As row 11 |
| B-007_v2_design_inputs.md section 2, item 12 (a claim on in-flight work as a lease; what its expiry does) | C-005, C-008, B-009 | whole-design review (with C-005_v2_design_inputs.md item 3) | All three froze legacy only; C-005's item 3 carries the same consideration ("(B-007 item 12)") to the whole-design review under that file's own rule |
| B-008_v2_design_inputs.md row 2 (whether an early alert is refused, recorded as refused, or held) | C-002, C-006, C-007 | VP-24, VP-59; whole-design review | All three froze legacy only; the subject is VP-24's (outside-window signals rejected with a recorded reason, not queued) and VP-59's (refusals at the webhook stage recorded), both moved by S-1 |

The new destinations (VP-14 in S-1, Q-20 and Q-45 (h) in S-2, the four S-3 cells, and the tracking row) are Claude's proposals: the record does not already name them. They are flagged under ChatGPT's authorisation (its section 6) for review rather than presented as mere scheduling, and Rama may name another topic for any of them. Every other move applies a destination the record already gives.

Bookmarks: BK-01 to BK-53 stay OPEN with no row changed. Section 7's rule (line 516) already reviews each "at the topic it names or at the whole-design review after the agenda", and W-9 schedules that review. Twenty-two bookmarks whose cells name no later point (seventeen) or say "not assigned" (BK-48 to BK-52) reach the whole-design review through that rule; writing an owner into them would be a new ownership choice, so none is proposed (cross-check, part D).

## 6. Documentation-only corrections (C-1; no decision content)
1. Three companions, frozen with their manuals and "stored unchanged beside it" (DR-061, DR-064; each manual's line 6), keep the status lines they had when delivered:
   - C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md line 3: "... (a draft; not frozen, not committed, not pushed) ...";
   - C-011_DRAFT_EVIDENCE_AND_GAP_CHECK.txt line 42: "Status: DRAFT ONLY. Nothing frozen, committed or pushed; no final ZIP or manifest; no V2 decision. ...";
   - C-011_DRAFT_REVISION_5_CHANGE_NOTE_25-OCT-2026.txt line 5: "Status: DRAFT ONLY - NOT FROZEN, COMMITTED OR PUSHED. No final ZIP or manifest was made."
   Recommended (PROPOSED): no change to the frozen files; the Section C close row records the three lines as KNOWN DOC DRIFT, as DR-052 did for the B-008 addendum, so that a reader who opens a companion first is not misled. The process lines of the C-009, C-010 and C-011 evidence sheets ("STOPPED for ChatGPT's reconciliation" and the like) state what was done when they were delivered and claim no draft or frozen state; no record is proposed for them.
2. V2_DESIGN_LOG section 6: the C-011 publication (3761e3b, 35 files, confirmed by Claude against the remote on 25-Oct) has no session note yet; add it with this step's notes, citing the commit and the file count only, as each earlier publication note does.

## 7. Answer sheet (labels as in sections 4 to 6)
- D-1 Q-46, DR-028's scope: A per system / B V2 as a whole / C keep OPEN, moved to the whole-design review with VP-45 and VP-56
- S-1 ratification points and the tracking row: accept / change (name the row; for VP-14, name the topic if not the whole-design review)
- S-2 Q-20's area and two audit-lead owners: accept / change (name the row; for Q-20 or Q-45 (h), take the alternative shown or name a topic)
- S-3 four V2 inputs owner cells: accept / change (name the cell)
- C-1 corrections: accept / change (name the item); item 1 recorded as drift (recommended)
Partial answers are fine; an unanswered item stays where it is. An answer of A or B to D-1 ratifies the exact proposed register text unless the answer changes its wording. The compact form is SECTION-C_END_CHATGPT_ANSWER_SHEET_DRAFT.txt.

## 8. Current external facts
None is used. D-1 reads a decided row in Rama's own words and depends on no external rule. Broker and exchange rules that bear on running two accounts through APIs are current external facts, to be checked and dated when their owning topics take them (W-5).

## 9. After Rama answers
The full procedure, with exact before and after text for every governance change, is SECTION-C_END_POST_ANSWER_CHANGE_PLAN.txt. In short:
1. Each answer is recorded as given, with any qualification Rama adds.
2. One register row for D-1 when answered A or B (DR-065). A moved or unanswered item gets no decision row.
3. One closing row, "Section C close (ownership and scheduling only)" (DR-066, or DR-065 if D-1 is C), records S-1 to S-3 and C-1 as answered and Q-46's outcome, that the eleven legacy manuals are frozen (DR-053, DR-055 to DR-064), that BK-01 to BK-53 stay OPEN, and that the next step is Section D.
4. V2_DESIGN_LOG:
   - the S-1 cells changed, each row with its tracking sentence ("scheduling only"), and the tracking row added if accepted;
   - Q-20's area and the two lead owners changed one at a time, each with its tracking sentence;
   - Q-46 updated as answered, and VP-45 and VP-56 annotated with D-1's outcome;
   - a "Section C end" agenda row added after C-011;
   - session notes, including the C-011 publication note; one optional lesson (change plan, part 3).
5. The four V2 inputs files: owner text only, with a status-line sentence in each, as at the Section B close.
6. Before anything is stored:
   - an exact before/after diff of every changed file;
   - a check that each register row carries Rama's exact words, not a paraphrase that broadens the decision, and agrees with DR-028 and the other rows it names;
   - a cross-check of this paper against those files;
   - a re-run of the full disposition audit (every VP ratification cell, every Q owner segment, every bookmark, every V2 inputs owner cell and the Section C agenda rows), so that no completed Section C topic stays a ratification point or owner unless Rama chose to keep it;
   - a check of citations, file paths, line ranges and commit provenance;
   - a scan of the changed files for identifiers, addresses, secrets and session links;
   - the package and push commands prepared under W-10. Claude and ChatGPT commit or push nothing; Rama stores and pushes.
7. No frozen legacy manual, companion or addendum changes.

Status and boundary: the Section C end decision is CLOSED and recorded in DR-065, with the close of Section C in DR-066. Proposals and parts not explicitly decided here remain PROPOSED or OPEN in V2_DESIGN_LOG.md with their owners. These decisions do not authorise implementation; the post-agenda legacy audit (DR-016) and the remaining governance and design steps still apply. These remain open:
- Q-01 (pre-live validation), Q-05 (environments), Q-11 (fresh credentials), Q-21 (the legacy ETF and SME path) and the other open questions of V2_DESIGN_LOG section 2, with their owners; Q-20, Q-37 (f) and Q-45 (h) among them, moved by S-2 and still OPEN.
- Every lead of the Audit rows, and every lead, defect and unknown inside the frozen Section C manuals, for the post-agenda legacy audit (DR-016).
- BK-01 to BK-53, for the topics they name or the whole-design review (W-9).
- VP-45's identity questions and VP-56's per-system and per-environment secrets; DR-065 decides neither.
- Every proposal not named in sections 4 and 5, and every undecided part of those named, with its current owners; VP-14 stays not ratified and VP-27 pending, as recorded.
No V2 implementation is authorised (DR-016).

## 10. Rama's answers (25-Oct-2026) and the register rows

| Item | Rama's answer, as given | Register row |
|---|---|---|
| D-1 DR-028's scope (Q-46) | "A: Per system (Recommended)", the question adopting the exact option A text of section 4 | DR-065 |
| S-1 ratification points and the tracking row | "Accept S-1 (Recommended)" | DR-066 |
| S-2 and S-3 question, lead and V2 inputs owners | "Accept both (Recommended)" | DR-066 |
| C-1 documentation corrections | "Accept; drift (Recommended)" | DR-066 |

Rama answered through Claude's multiple-choice questions on 25-Oct-2026, at about 17:44 IST (W-10), after ChatGPT's Stage 1 review of the drafts. The D-1 question adopted the exact option A text of section 4, which DR-065 carries word for word; its preview showed that text, with the "Owned elsewhere" list in short form.

What the records now show:
- DR-065 records that DR-028 applies per V2 system; DR-028 is not edited and stays DECIDED; Q-46 is CLOSED.
- The ten S-1 rows are moved, each with a "scheduling only" tracking sentence; VP-45 and VP-56 carry DR-065's reading; the "Whole-design review (after the agenda)" tracking row is added to the agenda table.
- Q-20's area and the owners of Q-37 (f) and Q-45 (h) are moved one at a time, each with a tracking sentence; each stays OPEN.
- Four owner cells in the C-001, C-002, B-007 and B-008 V2 inputs are moved, owner text only, each file's status line recording it.
- The three companions' stale status lines are recorded in DR-066 as KNOWN DOC DRIFT; the frozen files are unchanged.
- The Section C end agenda row, the C-011 publication note, this step's session notes and one lesson are added.
- Every other part stays PROPOSED or OPEN with its owner.

## Changes at closure (25-Oct-2026; documentation only)
- Title marked CLOSED; the status line, the place line and the closing status-and-boundary paragraph replaced; section 10 and this change note added. Sections 1 to 9 are otherwise as presented to Rama (revision 0, unchanged after ChatGPT's review).
- Storage settled at packaging: ChatGPT's Stage 0 review and Stage 1 authorisation is stored beside this paper with its Stage 1 review, as the C topics' Stage 1 authorisations are stored in their topic folders; the change plan (part 5) had proposed docs/inputs/reconciliation/ for it.
