# SECTION B END: DECISION PAPER (CLOSED)

Status: CLOSED 2026-10-21. Rama answered on 21-Oct (section 10); his decisions are recorded in V2_DECISION_REGISTER.md as DR-050 and DR-051, and G-1, T-1, the S-1 and S-2 moves and the C-1 corrections with the close of Section B as DR-052. Revision 2 (after ChatGPT's value-added review and final consistency review of 21-Oct-2026) is the version that was put to Rama; sections 1 to 9 are kept as presented, except the closing status-and-boundary paragraph, so the options and recommendations below show what he chose from, not what was decided. Each point of ChatGPT's first review is answered in CLAUDE_RESPONSE_TO_CHATGPT_VALUE_ADDED_REVIEW_SECTION_B_END_21-OCT-2026.txt (stored beside this paper). Rama decided; ChatGPT's reviews alone decided nothing (V2_DESIGN_LOG W-6).
Date: 2026-10-21 (IST)
Place at: docs/agenda/b-section-end/SECTION-B_END_decision_paper.md
Why this step: all twelve Section B legacy manuals are frozen (DR-035, DR-037, DR-039 to DR-041, DR-043 to DR-049), and none of those rows decided anything for V2. Fifteen proposals still name a Section B topic (or "B") among their ratification points. B-008 and B-009 left theirs unmoved "for Rama's later review" (V2_DESIGN_LOG section 4, lines 241 and 242), and B-010 to B-012 made "no scheduling move" (lines 243 to 245). Q-39 names the Section B end as its natural decision point (line 137). The evidence Rama's own condition for VP-62 named ("decide after B-008/B-011/B-012 evidence and discussion") now exists. This paper sorts these items, puts to Rama only what is ready, and moves the rest by scheduling only.
Sources (pre-decision baseline): the trading-system-v2 repository, main at b390ab0 (21-Oct-2026 08:31 IST): V2_DESIGN_LOG.md (blob 678c5be9, 391 lines), V2_DECISION_REGISTER.md (blob dfede582, 60 rows to DR-049), MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt (blob a9b7bbf2); the Section A end paper and ChatGPT's two Section A reviews (process precedent only); the frozen B-001 to B-012 manuals, addenda, review notes and V2 inputs; Rama's checklists in docs/inputs; SEBI's circular of 15-Oct-2026 and three broker pages, accessed on 21-Oct-2026 (section 8). Full list, blob IDs and line ranges: SECTION-B_END_SOURCE_INVENTORY_AND_METHOD_2026-10-21.txt (stored in docs/inputs/chatgpt_originals_and_replies/).
Labels: FACT (SOURCE-VERIFIED) for legacy code at 970aabf, cited through the frozen manual that verified it; FACT (RECORD-DERIVED); HISTORICAL (Rama), his words quoted; CURRENT EXTERNAL FACT, with the access date (W-5); INTERPRETATION / AUDIT LEAD; KNOWN DOC DRIFT / DEFECT; UNKNOWN / NOT ESTABLISHED; PROPOSED / OPEN.
Security: no credential values, account identifiers, mailbox or host addresses.

## 1. What this step decides, and what it does not
- Puts to Rama: two substantive decisions in principle (D-1, D-2), one documentation-governance question (G-1, Q-39) and one timing question (T-1, VP-62 with VP-22). Each answered decision becomes its own register row (DR-050 onward), so that each can later be superseded alone.
- An answer of A or B (or G-1 (a) or (b)) ratifies the exact proposed register text shown in section 4, unless Rama changes the wording in his answer. A recommendation from Claude or ChatGPT is not approval.
- Reviews so far: ChatGPT's value-added review (21-Oct) supports D-2 A, G-1 (c) and T-1 (i), and D-1 A with the narrowing applied in revision 1. It recommends recording C-1 item 1 as drift without editing the frozen file, which is now the recommendation. Its final consistency review of revision 1 (21-Oct) confirmed the three points Claude asked about, with the clarifications applied in revision 2. Neither review decides anything.
- Records for acceptance: scheduling and ownership moves (S-1 for proposal ratification points, S-2 for audit-lead owners) and documentation-only corrections (C-1). These change owners, points or wording errors, never proposal content.
- Does not decide: any mechanism, time value, threshold, state machine, process model, kill policy or login method; nothing owned by Sections C to O. A ratified principle authorises no implementation: the rest of the agenda, the post-agenda legacy audit (DR-016) and frozen norms come first.
- Does not reopen: DR-027 to DR-034 (Section A), the twelve Section B freezes, or any erratum. No frozen manual or addendum changes, except as G-1 (a) or (b), or C-1 item 1's erratum alternative, would do if Rama chooses them.

Three kinds of item (the distinction ChatGPT's Section A review drew):
- A substantive decision answers what V2 will do, at the level of intent; it gets a register row and marks the proposal (or its part) DECIDED.
- A scheduling or ownership change moves where a proposal or question will be decided; the proposal's text is unchanged and nothing is decided.
- An unresolved item stays PROPOSED or OPEN with a named owner; carrying it forward is not a decision about its content.

## 2. Section B in brief

| Topic | Frozen as | V2 outcome at the freeze |
|---|---|---|
| B-001 VM / service startup | DR-035 (errata E-1 DR-036, E-2 DR-037, E-3 DR-044) | No V2 decision; VP-66, VP-67 added; points moved for VP-16, VP-47, VP-54, VP-57, VP-62 |
| B-002 Configuration loading | DR-037 (erratum E-1 DR-040) | No V2 decision; VP-68 added; B-002 dropped from ten VP rows and Q-25; VP-47, VP-57, VP-67 points moved |
| B-003 Database / state initialisation | DR-039 | No V2 decision; VP-57, VP-67 ranges moved |
| B-004 Broker connection | DR-040 (erratum E-1 DR-041) | No V2 decision; VP-57, VP-67 ranges and Q-40 (a) moved |
| B-005 Startup reconciliation | DR-041 (erratum E-1 DR-043; DR-042 is the related A-011 E-5) | No V2 decision; VP-57, VP-67 ranges moved; B-005 dropped from VP-60 |
| B-006 Worker initialisation | DR-043 | No V2 decision; VP-57, VP-67 ranges and Q-40 (g) moved |
| B-007 Readiness / health | DR-044 | No V2 decision; VP-57 range moved; B-007 dropped from VP-67 and from seven audit leads' owners (Q-40 (g), Q-41 (c), Q-42 (a), Q-43 (a), Q-44 (a), (b), (d)); Q-41 (g) passed to B-012 |
| B-008 Market-open operation | DR-045 | No V2 decision; no move ("for Rama's later review": VP-16, VP-19, VP-57, VP-62, Q-40 (d)) |
| B-009 Intraday operation | DR-046 (erratum E-1 DR-047) | No V2 decision; no move ("for Rama's later review": VP-19, VP-20, Q-45 (h)) |
| B-010 EOD operation | DR-047 | No V2 decision; no move |
| B-011 Shutdown | DR-048 | No V2 decision; no move |
| B-012 Restart / recovery | DR-049 | No V2 decision; no move |

FACT (V2_DECISION_REGISTER lines 46 to 60; V2_DESIGN_LOG section 4, lines 235 to 246). Every manual, addendum and review note carries a FROZEN or CLOSED status line except one addendum whose line was not updated at the freeze (C-1 item 1). The Master Agenda's goal for Section B is understanding: "Understand the complete system lifecycle from process start to process termination/recovery" (agenda lines 264 to 266). The project records legacy fact and V2 proposals in separate layers (W-1, W-6), so the agenda's per-topic "final decision" step (agenda Part 3, item 11) has, in practice, been DEFER for every Section B topic (agenda Part 7). Part 3's list of outcomes leaves DEFER out; Part 7 includes it.

## 3. How the Section B items were sorted
Readiness test applied to each item, in this order:
1. Its current ratification point (V2_DESIGN_LOG, read at b390ab0) names a Section B topic or "B" (answer-sheet rule 4).
2. Its content is Rama's own recorded instruction, not a Claude or ChatGPT consideration.
3. No recorded dependency or deferral condition is still open.
4. What stays open is mechanism or values that a named later owner can take, not a second policy choice hidden inside it.
An item that passes all four is put to Rama as a decision (with an option to move it instead); one that fails is moved by scheduling only, or its timing is asked (T-1).
Precedent: Section A put to Rama its end-of-Section-A items and also VP-04, whose point named a finished Section A topic ("A-011 (states) and I (safety)"). VP-04 was ratified as a principle, with its list left to the owners (Section A paper line 6 and D-5; DR-033). Its other finished-topic points were moved (S-1, DR-034).

| Item | Points now (design log line) | Test result | Treatment |
|---|---|---|---|
| VP-06 (start and stop by calendar part) | "As listed": "start and stop by calendar at B" (155) | Passes: Rama's words (A-001 line 66; A-002 line 120) | D-1 |
| VP-16 (calendar, auto-start) | B-008 to B-010 (165) | Passes for the calendar and start parts, with two recorded links set out in D-1 (DR-034 moved it "with VP-62"; its "failing safe" is also VP-67's trading-day prerequisite); its two time values belong elsewhere | D-1, with the times moved |
| VP-19 (entry cut-off by product) | B-008, B-009, I-001 (167) | Passes for the cut-off part (Rama, A-003 lines 126, 143, 146); the SOFT_KILL part is already I-001's (DR-034) | D-2 |
| VP-20 (cancel pending intraday entries at the cut-off) | G-014, B-009 (168) | Fails 4: a mechanism | S-1 |
| VP-21 (square-off before the broker and two checks) | H-011, B-010 (169) | Fails 4: square-off design is H-011's | S-1 |
| VP-22 (clean shutdown with carried holdings) | B-010 to B-012 (170) | Fails 3: its first clause assumes a daily process end, which is VP-62's open choice; "protected at the broker by GTT" needs verified protection (DR-032) | T-1 (moves with VP-62) |
| VP-25 (cron review) | M-002, B (173) | Fails 4: a review task, owned by M-002 | S-1 |
| VP-47 (configuration boot gate) | B-007, configuration design, B-012, L-003, I (195) | Fails 3 and 4: "no start" on a restart with open positions meets the open restart-under-kill policy (VP-61, I-012); what the gate checks is the configuration design step's; B-007 was not dropped at DR-044 | S-1 |
| VP-52 (sensitive-change authorisation) | M-003, L-008, B-012 (200) | Fails 4: restart timing for a change is deployment (M-003) | S-1 |
| VP-57 (one state model) | O-004, B-008 to B-012, L-005 (205) | Fails 3 and 4: the state machine is O-004's; the B range was a tracking range | S-1 |
| VP-60 (restart recovery before entries) | B-012, G-015, I-004, I-012, K-001, K-002, K-012, H-005, H-006 (208) | Fails 3: what blocks READY, the broker wait and the local store's role stay open at I and K | S-1 |
| VP-62 (the trading day as a session) | B-008, B-010 to B-012, M-001, O-010 (210) | Fails 3 as recorded ("DEFERRED until B-008, B-011, B-012, M-001 and O-010"); the evidence Rama's own condition named now exists | T-1 |
| VP-63 (uncertain exits at end of day) | H-011, B-010, G-001, G-009, G-014 (211) | Fails 4: the retry ladder and manual procedure are H and G's | S-1 |
| VP-66 (start initiators, host boot) | B-012, M-001, M-007, I-010, L-003 (214) | Fails 3 and 4: the host-boot option is an open trade-off (B-001_v2_design_inputs.md lines 61 to 73) | S-1 |
| VP-67 (start prerequisites, fail closed) | B-012, M-006, I-010, L-003 (215) | Fails 3: "no unresolved HALT or kill" as a hard gate meets VP-61's open restart-under-kill policy (I-002, I-013) | S-1; its "valid trading day" item goes with D-1 if Rama answers A |
| Q-39 (evidence lines in A-005 to A-011) | "Owner: Rama; a natural point is the Section B end" (137) | Due now; a documentation-governance choice, not V2 design | G-1 |
| Q-37 (b), (i); Q-38 (j); Q-40 (d); Q-41 (c), (g); Q-43 (a), (c), (i); Q-44 (b), (c), (e), (f), (g); Q-45 (a), (h) | Audit leads with a B-008 to B-012 owner (135 to 143) | Audit questions for the post-agenda legacy audit (DR-016), not decisions | S-2 |

Searched and not affected (scope: every VP row of V2_DESIGN_LOG section 3 at b390ab0): seventeen proposals cite Section B manuals only as evidence and ratify outside Section B (VP-05, VP-07, VP-13, VP-15, VP-23, VP-32, VP-36, VP-42, VP-43, VP-45, VP-46, VP-48, VP-51, VP-53, VP-54, VP-64, VP-68); no change. No register row names a Section B topic as a future decision point (DR-001 to DR-049 read).

## 4. Questions for Rama
Option sets: no owning record gives options for VP-06, VP-16 or VP-19. A-001 to A-003 have no V2 inputs file; their review notes list the proposals only (A-002_review_notes.md line 45; A-003_review_notes.md lines 24 and 39), as does A-011_v2_design_inputs.md (lines 12 and 73). So in D-1 and D-2, B is the row as written, A is a principle form of Rama's words, and C is a move. G-1's (a) and (b) come from the Q-39 row; T-1's options come from the VP-62 row and Rama's B-001 answer.

### D-1 Trading days and the session calendar (VP-16, calendar and start parts; VP-06, start-and-stop part)
- Current proposals:
  - VP-16 (design log line 165): "Trading calendar and day lifecycle: maintained NSE holiday/weekend list for 2026 and 2027, failing safe when the current year's file is missing (A-003: legacy had only 2026); auto-start only on valid trading days; fresh-entry cut-off per config; intraday auto square-off at 15:10 (review legacy behaviour, then keep)." Ratify at: B-008 to B-010 only.
  - VP-06 (line 155), its part "start/stop driven by config with NSE trading days", ratified "at B" since DR-034.
  - The B-001 checklist deferred "the precise market calendar, start time and service-stop time ... to B-008/B-011 and related owners" (B-001 checklist lines 288 to 290). B-008 and B-011 froze legacy only.
  - Two recorded links:
    - DR-034 moved VP-16 into Section B "with VP-62" (line 165; Section A paper S-1, line 159). VP-16's day lifecycle sits beside VP-62's session model, which T-1 asks about.
    - Rama's Q-B001-04 answer made "a valid trading day" a REQUIRED start prerequisite. VP-67 records it (line 215), with his Q-B001-11 posture for a failed prerequisite. S-1 moves VP-67 without deciding it.
- HISTORICAL (Rama):
  - A-002 checklist line 120: "Yes, So maintain list of holidays & weekends list for 2026 & 2027 (just placeholder file now) > So system will auto-starts only on valid trading days".
  - A-001 checklist line 66: "Trading day as per NSE [Holidays + Weekends to be considered] & Service window as per config file settings".
  - B-001 checklist lines 261 and 271 (Q-B001-04): "The date is a valid trading day" marked REQUIRED before initialisation. Line 389 (Q-B001-11): "A. Do not begin trading initialization; retain the failure reason and report it through the approved alerts/evidence path."
  - Line 527: these B-001 answers are "Suggestion or raw input only", open to improvement after discussion.
- FACT (frozen manuals):
  - The legacy holiday guard read the host's local date and let the start continue when the year's holiday file was missing (B-001_manual.md section 7.1). The configuration load then ended that start with exit 5 (B-002_manual.md section 6, row M1). Only a 2026 file existed (DR-024).
  - A missing, empty or incomplete holiday file ended a start with exit 5 (rows M1, M2, M4, M5). A malformed one ended it with exit 2 before logging and before the window guard, at any hour (row M3: a conditional code path, no occurrence found; Q-40 (d) keeps the question open).
  - On weekends and listed holidays a start ended at the holiday guard with exit 0 (B-008_manual.md section 5, line 64; B-007 G1).
  - B-011_manual.md section 8 reports that special-session logic could move market open, market close and the EOD square-off for a date, but not the service's self-exit time. That a late special session could outlive the self-exit is INTERPRETATION there.
- What D-1 does not settle:
  - the calendar's authoritative source, file format, refresh process and special sessions (M-002, proposed because it already owns Q-40 (d): holiday-file failures and a 2027 file);
  - how the calendar is validated, including the check that the next year is present (VP-47, configuration design step);
  - the service window's values (VP-25, M-002);
  - whether the service process starts or stays up on a day that is not a trading day, and which non-trading jobs and monitoring run then (M-001; M-002 with VP-25, where Rama asked that cron jobs run on holidays and weekends; M-006). D-1 concerns trading initialisation only;
  - the start initiators and the host-boot option (VP-66);
  - whether a session is one process or part of a long-lived one (VP-62, T-1);
  - the other start prerequisites (VP-67);
  - the entry cut-off (D-2, VP-19) and the square-off times (VP-21, H-011);
  - alert classes (L-003).
- Options:
  - A: ratify the principle below. Two parts of it are Claude's proposed wording, not frozen policy; they become policy only if Rama accepts them. One is "at least the current and the next calendar year", his "2026 & 2027" written as a rule. The other is the reading of "failing safe" as no trading initialisation when the day cannot be confirmed, taken from his Q-B001-04 and Q-B001-11 answers, which he gave as "Suggestion or raw input only" (line 527). So A also decides VP-67's trading-day item, and only that item; S-1 moves the rest of VP-67. The text holds under either VP-62 model. If Rama would rather keep VP-16 with VP-62, or leave the fail-safe clause to VP-67, B or C does that.
  - B: ratify VP-16's calendar and start parts as written ("2026 and 2027"; "failing safe" undefined).
  - C: decide nothing now; move the calendar and start parts to M-001 and M-002 (scheduling only, content unchanged).
- Under A or B, VP-16's other two parts move without a decision: "fresh-entry cut-off per config" to VP-19 (D-2); "intraday auto square-off at 15:10 (review legacy behaviour, then keep)" to H-011 with VP-21. The legacy behaviour has been reviewed (B-010_manual.md sections 4 and 5). The exchange's closing times for closing-auction stocks changed on 3-Aug-2026, with Zerodha's MIS square-off times (Q-18), and change again on 7-Dec-2026; Zerodha's MIS times after that date are UNKNOWN / NOT ESTABLISHED (section 8).
- Recommendation (PROPOSED): A. The principle is Rama's own, the Section B evidence it waited for is complete, and what remains open is mechanism with named owners. C is equally consistent with the record. ChatGPT's review supports A with the narrowing now written into it (trading initialisation only; proposed wording; the calendar's mechanics left open).
- Proposed register text (option A): "V2 runs a trading session only on a valid NSE trading day, determined from one maintained exchange calendar that covers at least the current and the next calendar year (Rama, A-002: 'maintain list of holidays & weekends list for 2026 & 2027 ... So system will auto-starts only on valid trading days'). Session start and end times come from configuration (Rama, A-001: 'Service window as per config file settings'). If the calendar cannot confirm that today is a trading day (missing, unreadable or not covering today), no trading initialisation begins; the reason is retained and reported through the approved alert and evidence path (Rama, Q-B001-04 and Q-B001-11). This concerns trading initialisation only: it does not decide whether the service process starts or stays up on such a day, or which non-trading jobs and monitoring run then (M-001, M-002, M-006). This settles VP-67's 'valid trading day' prerequisite only; VP-67's other prerequisites stay open. This decision does not decide whether a session is a separate process or part of a long-lived one (VP-62). Owned elsewhere: the calendar's authoritative source, file format, refresh process and special sessions (M-002); its validation, including the next-year check (VP-47, configuration design step); the service window's values (VP-25, M-002); start initiators and mechanism (VP-66, M-001); the entry cut-off (VP-19) and square-off times (VP-21, H-011); alert classes (L-003)."

### D-2 The entry cut-off applies to intraday only (VP-19, cut-off part)
- Current proposal (VP-19, line 167): "Entry cut-off applies to intraday only; delivery may enter until 15:30 (BTST) if its strategy window allows. Consideration: keep the intraday entry cut-off a separate, named control from the SOFT_KILL kill-switch state (legacy conflated them)." Ratify at: B-008, B-009, I-001. DR-034 moved its SOFT_KILL part to I-001 (VP-61), so the cut-off part's only points are B-008 and B-009.
- HISTORICAL (Rama, A-003 checklist):
  - Line 126: "As of now both Intraday & delivery ends at same time ... in new system I'll set soft kill or new entry cut-off applicability only to Intraday trades, So Delivery trades can be traded till 15:30 if that respective strategy time also allows".
  - Line 143: "[Remember: In new system this needs to applied only on 'Intraday' trades why? Delivery trades for BTST till 15:30 requires]".
  - Line 146: "but in new system it must be applied on 'Intraday' only [Delivery=BTST calls requires till 15:30 IST]".
- FACT (frozen manuals):
  - The legacy global entry window was 10:00 to 15:00, and a strategy's own window applied only inside it (B-008_manual.md section 2). The three delivery strategies entered through the same session path with 09:25 to 15:00 windows, so one end time bound both books (B-009_manual.md section 4).
  - The 15:15 eod_entry_cutoff was a separate order-placement deadline, with the tracked-entry cancellation and a scheduled SOFT_KILL inside the order monitor's cycle (B-008 section 2; B-009 sections 3 and 9; DR-046).
  - Delivery was long only in legacy (three delivery strategies, all LONG: A-008_manual.md line 28) and is long only in V2 (DR-032).
- CURRENT EXTERNAL FACT (accessed 21-Oct-2026; section 8):
  - From 3-Aug-2026, continuous trading in closing-auction stocks (Category I, the stocks with F&O trading) stops at 3:15 PM, while other stocks trade until 3:30 PM. Source: Zerodha's published page, broker-published context.
  - From 7-Dec-2026, SEBI's circular of 15-Oct-2026 runs the closing auction from 3:30 p.m. to 3:40 p.m. on all trading days (paras 4.2.1 and 6.1). Zerodha's bulletin of 16-Oct says that regular trading in all stocks continues until 3:30 PM; the circular's timing table implies the same (INTERPRETATION).
  - So Rama's "till 15:30" holds today for stocks outside the auction, and for all stocks from 7-Dec-2026.
- What D-2 does not settle:
  - Whether the scheduled end-of-day SOFT_KILL also spares delivery. Rama's request named both ("soft kill or new entry cut-off", A-003 line 126), and it was recorded as VP-19 (A-003_review_notes.md line 24). DR-034 moved VP-19's cut-off-versus-SOFT_KILL part to I-001 (VP-61), where it stays open.
  - The time values and each strategy's window: configuration, checked against current rules (VP-49), with VP-24 at C-009.
  - Whether any entry may use a closing auction session (G-001). The end of regular trading in option A is not a permission to enter the auction.
  - Cancelling pending intraday entries at the cut-off (VP-20, G-014).
  - Late fills and the delivery GTT (DR-032; G and H).
  - Broker order acceptance and any broker-specific timing (G topics; VP-49).
- Options:
  - A: ratify the principle below, with the end time taken from the exchange's rules for the stock at the time.
  - B: ratify VP-19's cut-off part as written ("delivery may enter until 15:30 (BTST) if its strategy window allows").
  - C: decide nothing now; move the cut-off part to C-009 (with VP-24) and I-001 (scheduling only).
- Recommendation (PROPOSED): A. Rama's rule is explicit and repeated, and the Section B evidence it waited for is complete. A follows his "till 15:30" without fixing a clock time that the exchange is changing twice this year. ChatGPT's review also recommends A.
- Proposed register text (option A): "The fresh-entry cut-off applies to the intraday entry path only; that is the whole scope of this decision. Delivery entries (DR-032) are not stopped by it: a delivery strategy may enter later in the session, within its own configured window and no later than the end of the exchange's regular trading for that stock under the rules then current (Rama, A-003: 'Delivery trades can be traded till 15:30 if that respective strategy time also allows'). That end is an upper bound for entries in the continuous session only; it gives no permission to enter in a closing auction session. The cut-off is a named, scheduled control. Owned elsewhere: whether the scheduled end-of-day SOFT_KILL applies to delivery (I-001, VP-61, where DR-034 moved that part of VP-19); the time values and any broker-specific timing (configuration, checked against current exchange and broker rules, VP-49); entry in a closing auction session (G-001); cancellation of pending intraday entries at the cut-off (VP-20, G-014); late fills and the delivery GTT (G and H); broker order acceptance (G topics)."

### G-1 The "Source priority" lines of the frozen A-005 to A-011 manuals (Q-39)
- Q-39 (line 137): the frozen A-005 to A-011 manuals still show a universal "Source priority" ranking. The B-001 manual's line was replaced by erratum E-1. DR-036 (register line 47) adopted W-7 ("Evidence is claim-specific") and says those lines "are read under it, without errata". ChatGPT's 11-Oct review left a later choice between narrow errata and a cross-reference, with "no bulk edit and no reopening without Rama's approval of the exact scope". Owner: Rama; "a natural point is the Section B end".
- FACT (exact scope): seven lines: A-005_manual.md line 19; A-006 to A-011 manuals line 8. None in A-001 to A-004. B-002 onward state W-7 instead (W-7, design log line 256).
- Options:
  - (a) Seven narrow errata. The line is struck, the W-7 wording follows, and each manual gets an erratum section. Recorded in one register row, which supersedes DR-036's "without errata" for these seven lines only.
  - (b) Each line is kept, with one added sentence: "Read under W-7 (V2_DESIGN_LOG; DR-036)". Recorded in one register row; no line struck.
  - (c) No file change. Q-39 is closed as settled by DR-036. The design log, read at every session start (CLAUDE.md), carries W-7.
- Recommendation (PROPOSED): (c). DR-036 already binds the reading, and (c) leaves the frozen manuals untouched. (b) is the option if Rama wants a reader of a single manual to see the rule without opening the design log. ChatGPT's review also recommends (c).

### T-1 When to decide VP-62, the trading day as a session (with VP-22)
- VP-62 (line 210) asks whether V2 runs one trading process per session or one long-lived process (Rama's Q-B001-05, options A and B). Ratify at: B-008, B-010 to B-012, M-001, O-010.
- Rama's own condition (B-001 checklist line 284): "C. Not decided; decide after B-008/B-011/B-012 evidence and discussion". Its evidence part is met: B-008, B-011 and B-012 are frozen (DR-045, DR-048, DR-049). Its discussion part has not happened; option (ii) would hold it. He also asked to "discuss and decide with me + ChatGPT + Claude later" (line 285).
- The recorded condition is not met. It reads "DEFERRED until B-008, B-011, B-012, M-001 and O-010 have been discussed" (B-001_v2_design_inputs.md line 154, which gives Rama's own condition as "after B-008, B-011 and B-012"; the VP-62 row cites ChatGPT's 10-Oct review, point 3.1). The agenda takes Section O last ("NOT discussed in detail until Sections A-N have been sufficiently settled", lines 561 to 563).
- The evidence that review asked for does not exist yet. It listed measured start-up and reconciliation duration, an intraday host reboot with open positions, broker availability at the start, duplicate-start fencing, restart recovery, alert delivery and session-boundary tests under each model: "To be measured at N (testing) with M-001" (B-001_v2_design_inputs.md line 145). These are V2 test measurements. The legacy hosts are deleted, so no comparable legacy measurement can now be taken (B-001_manual.md section 2; B-003_manual.md section 2).
- VP-22 (line 170) ratifies at B-010 to B-012 only. Its first clause ("the service shuts down normally when only CNC holdings remain") assumes a daily process end, which is VP-62's choice; DR-034 already records it as "carried with VP-62". "Protected at the broker by GTT" also needs verified protection: DR-032 does not guarantee that a protective order exists, and legacy treated the CNC product alone as broker-protected (B-011_manual.md section 3.3).
- What Section B added for VP-62 (FACT unless marked):
  - The daily start ran after the 08:15 token through the token watcher (B-012_manual.md section 4).
  - The 17:35 self-exit ended the process only when nothing required it (B-011 section 3).
  - Prior-day kills were cleared only when a new process started. INTERPRETATION (B-012 section 6.2): a process that stayed up overnight kept them.
  - Two supervisors decided restarts independently (B-012 sections 3 and 4).
- Options:
  - (i) Keep the recorded deferral. VP-62 and VP-22 ratify at M-001 and O-010, and their Section B points are dropped (scheduling only).
  - (ii) Hold a dedicated VP-62 discussion now, as its own step before Section C (Rama, ChatGPT, Claude), and decide there. This paper does not decide VP-62 under either option.
- Recommendation (PROPOSED): (i). The measurements the decision was to rest on cannot exist before testing. Rama can still bring VP-62 forward at any time by asking. ChatGPT's review also recommends (i), with VP-22 kept with VP-62.

## 5. Scheduling and ownership changes (S-1, S-2; ownership and scheduling only)
None of these rows changes proposal content or decides anything. Each was checked against V2_DESIGN_LOG and V2_DECISION_REGISTER at b390ab0. The B topic in each row froze legacy only ("No V2 decision", register lines 56 to 60), and the substantive question belongs to the owner named. Each moved row will carry one tracking sentence, as earlier moves did ("At the Section B close (DR-0nn, scheduling only): ..."), citing the B manual sections that hold its legacy evidence. The conditional rows (VP-06, VP-16, VP-19, VP-22, VP-62) move only after the matching D-1, D-2 or T-1 answer, and a DECIDED mark covers only the exact part named.

S-1, proposal ratification points:

| VP | Points now | Proposed | Why |
|---|---|---|---|
| VP-06 (start and stop part) | "at B" | If D-1 is A or B: DECIDED with D-1. If C: M-001, M-002 | Start and stop mechanics are systemd and cron (agenda M-001, M-002) |
| VP-16 | B-008 to B-010 | If D-1 is A or B: calendar and start parts DECIDED; cut-off part to VP-19; square-off part to H-011. If C: M-001, M-002 (calendar, start); VP-19 (cut-off); H-011 (square-off) | B-008 to B-010 froze legacy only (DR-045, DR-047) |
| VP-19 | B-008, B-009, I-001 | If D-2 is A or B: cut-off part DECIDED; I-001 keeps the SOFT_KILL part. If C: C-009 (with VP-24), I-001 | B-008, B-009 froze legacy only (DR-045, DR-046) |
| VP-20 | G-014, B-009 | G-014 | Cancellation is G-014's; B-009 recorded the legacy 15:15 cancellation (B-009_manual.md sections 3 and 9) and the order monitor's self-stop that ends it (section 8; D3 in section 15) |
| VP-21 | H-011, B-010 | H-011 | EOD square-off is H-011's; legacy passes and times in B-010 sections 4 and 5. The tracking sentence can also point H-011 to the 7-Dec-2026 closing-time change (section 8), which Q-18's 06-Oct answer predates |
| VP-22 | B-010 to B-012 | If T-1 is (i): M-001, O-010 (with VP-62). If (ii): the VP-62 step | DR-034: "carried with VP-62" |
| VP-25 | M-002, B | M-002 | A cron review task; legacy registry and jobs in B-012 section 7 |
| VP-47 | B-007, configuration design (build phase), B-012, L-003, I | Configuration design (build phase), L-003, I (I-012 for a restart while positions are open, DR-034) | B-007 froze legacy only (DR-044) but was not dropped from this row; this move corrects that by scheduling only, with the proposal's text unchanged. B-007's part, where the gate sits in the start (DR-037), joins the configuration design step, which already owns what the gate checks, beside VP-67's placement of each start check. That step is the design log's tracking row "Configuration design (build phase)" (line 247, created by DR-037), which already lists VP-47; it is not a Master Agenda topic. B-012 froze legacy only (DR-049); its part, a restart while positions are open, stays at I-012 |
| VP-52 | M-003, L-008, B-012 | M-003, L-008 | B-012's part was restart timing for a change (DR-034), which is deployment (M-003) |
| VP-57 | O-004, B-008 to B-012, L-005 | O-004, L-005 | The B range was a tracking range moved at each freeze since DR-035; the attribution of the 15-Aug ruling stays OPEN with VP-57 (DR-044) |
| VP-60 | B-012, G-015, I-004, I-012, K-001, K-002, K-012, H-005, H-006 | The same without B-012 | Restart reconciliation is I-012 and K; legacy recovery in B-012 section 6.3 and B-005 |
| VP-62 | B-008, B-010 to B-012, M-001, O-010 | If T-1 is (i): M-001, O-010. If (ii): the VP-62 step | B-008, B-010 to B-012 froze legacy only |
| VP-63 | H-011, B-010, G-001, G-009, G-014 | H-011, G-001, G-009, G-014 | Legacy EOD paths and defects in B-010 sections 5, 6 and 14 |
| VP-66 | B-012, M-001, M-007, I-010, L-003 | M-001, M-007, I-010, L-003 | B-012's part (exit codes and retry ownership) sits with VP-64 at M-001 |
| VP-67 | B-012, M-006, I-010, L-003 | M-001, M-006, I-010, I-013, L-003. If D-1 is A: its "valid trading day" prerequisite is DECIDED with D-1, and the rest moves | B-001's own placement table named "B-012, M-001" for "No prior instance running" and "B-012, I-013" for "No unresolved HALT or kill condition" (B-001_v2_design_inputs.md lines 105 and 106); with B-012 dropped, M-001 and I-013 are added so that both checks keep a named owner |

S-2, audit-lead owners (V2_DESIGN_LOG section 2; every lead stays OPEN for the post-agenda legacy audit, DR-016; no lead is answered by this move):

| Lead | Owners now | Proposed | Note |
|---|---|---|---|
| Q-37 (b) carried delivery positions holding the service open after 25-Aug | B-010, B-011 | M-001 | Sole B owners; the code path (CNC alone counted as broker-protected) is in B-011_manual.md section 3.3; occurrence not established |
| Q-37 (i) --resume, resume.sh and manual stops; a --resume start past 17:35 | B-012, M-010 | M-010 | Code paths in B-011 section 4.2 and B-012 sections 6.1 and 6.2 |
| Q-38 (j) the instance lock under PrivateTmp | M-001, B-012 | M-001 | B-012 section 5 adds a conditional reading (the lock released while a flatten worker may run) |
| Q-40 (d) holiday-file parse failures; a 2027 file | B-008, M-002 | M-002 | Not answered in B-008 |
| Q-41 (c) --status or --dry-run on production making the next start a CRASH start | B-012 | M-010 | Sole B owner; not answered in B-012 |
| Q-41 (g) the kill state failing to load at a start (B-003 D15) | B-012 | I-002, K-002 | Sole B owner (passed to B-012 by DR-044 as the next topic meeting the kill-state load); not answered in B-012; the next topics meeting it are the kill switch (I-002) and the state store (K-002) |
| Q-43 (a) a startup pass that aborted | B-012, I-006, I-012 | I-006, I-012 | |
| Q-43 (c) a kill set during a startup pass, then HALT | B-012, I-001, I-002, M-001 | I-001, I-002, M-001 | |
| Q-43 (i) reconcile_positions' alert level | B-010, L-003 | L-003 | Not answered in B-010 |
| Q-44 (b) the MIS poller acting while a start reconciled | B-010 | H-011, I-012 | Sole B owner; not answered in B-010 |
| Q-44 (c) a start ending after workers started | B-011, B-012, I-009 | I-009 | |
| Q-44 (e) a poller cycle meeting a closed store at shutdown | B-011 | K-002, I-009 | Sole B owner; B-011 section 6 lists the daemon threads that _shutdown() does not join (pre-alert, margin-sync, EOD, self-exit, the candle timer), and section 5.2 says the store's close closes only the calling thread's connection; occurrence not established |
| Q-44 (f) a service stop with a running pre-flight child | B-011, M-001 | M-001 | |
| Q-44 (g) the event-bus pools and a non-daemon thread | B-011 | I-009, M-001 | Sole B owner; B-011 section 6 left "other pools" not completely traced |
| Q-45 (a) how the 10-Aug-2026 start ended | B-012, I-002, M-006, L-003 | I-002, M-006, L-003 | |
| Q-45 (h) the in-flight sweeper's evictions | C-005, C-008, B-009 | C-005, C-008 | |

The successor owners for the eight sole-B leads are Claude's proposals (each names the next agenda topic that meets the lead's subject, as DR-044 did for Q-41 (g)); Rama may change any of them. Each lead is recorded on its own (16 entries), never grouped by owner.

## 6. Documentation-only corrections (C-1; no decision content)
1. B-008_reconciliation_addendum.md line 3 still reads "Status: DRAFT, with the B-008 manual". DR-045 (register line 56), B-008_review_notes.md line 4 and the design log's B-008 row (line 241) record it as frozen with the manual. This was Claude's omission at the B-008 freeze.
   - Recommended (PROPOSED; also ChatGPT's recommendation): no change to the frozen addendum. The Section B close row records the stale line as KNOWN DOC DRIFT, and DR-045 stays the authoritative record of the freeze.
   - Alternative, only on Rama's explicit choice: an erratum in the form used for B-009 E-1 (DR-047: "corrected in place, struck text kept"). The old line is struck and followed by "Status: FROZEN 2026-10-20 with the B-008 manual (DR-045); status line corrected at the Section B close (DR-0nn)".
2. Owner cells in PROPOSED V2 inputs files that name a section the agenda does not have or a closed Section A topic. Only the wrong part of each cell changes:
   - B-011_v2_design_inputs.md row 3 "O topics, P topics": there is no Section P (agenda sections A to O). Proposed: "O topics, VP-64, VP-65 (M-006, L-008, L-009)".
   - B-011 row 4 "A-003 follow-up, F topics": A-003 is closed, and Section F (capital) does not hold the row's subject, which is one calendar source for every time-based control. Proposed: "VP-16 (M-002), VP-23 (K, I-010)".
   - B-012_v2_design_inputs.md row 4 "P topics". Proposed: "M-002, L-003, N-010".
   - B-008_v2_design_inputs.md row 1 "A-003, A-008, VP-57". Proposed: "C-010, configuration design (build phase), VP-57".
   - B-008 row 3 "L-005, A-008". Proposed: "L-005, C-010".
   - B-009_v2_design_inputs.md row 1 "A-008, A-009, N-001". Proposed: "configuration design (build phase), VP-68, N-001".
   - All six cells are Claude's, written on 20-Oct. The replacement owners are Claude's proposals, as in S-2.
   - Every replacement owner is one of three kinds: an existing Master Agenda topic or section, a VP row of the design log, or an existing design-log tracking row. The topic IDs are in the Master Agenda (C-010 line 281, I-010 425, L-003 490, L-005 492, L-008 495, L-009 496, M-002 509, M-006 513, N-001 526, N-010 535), as are Sections K and O (lines 463 and 543). The VP rows are in the design log. "Configuration design (build phase)" is the third kind: the design log's tracking row (line 247, created by DR-037), which nine VP rows already name. It is not a Master Agenda topic.
   - Only the owner text changes; each change is shown as a before/after diff in the post-answer package.
   - B-012 row 3's "Operations topics" stays: it names Section M by its agenda title ("M. OPERATIONS").
3. V2_DESIGN_LOG section 4: the B-007 row sits after B-012 (line 246); restore agenda order when the Section B end row is added. Claude's slip: the B-008 package (9827e40) inserted its row above the B-007 row, and each later package followed.
4. V2_DESIGN_LOG section 6: the B-012 publication (b390ab0, 13 files, confirmed against the remote on 21-Oct) has no session note yet; add it with this step's notes. Evidence: b390ab0 equals origin/main (fetched again on 21-Oct before revision 1), and each of the commit's 13 files matched the delivered file at the 21-Oct publication check. The note cites the commit and the file count only, with no URLs or machine details.

## 7. Answer sheet (labels as in sections 4 to 6)
- D-1 trading days and calendar: A ratify the principle / B ratify VP-16's parts as written / C move to M-001, M-002
- D-2 entry cut-off intraday only: A ratify the principle / B ratify as written ("until 15:30") / C move to C-009, I-001
- G-1 Q-39 evidence lines: (a) seven errata / (b) a cross-reference line in each / (c) no change, close under DR-036
- T-1 VP-62 (with VP-22): (i) keep deferral to M-001, O-010 / (ii) a dedicated discussion now
- S-1 and S-2 scheduling: accept / change (name the row)
- C-1 corrections: accept / change (name the item); item 1 recorded as drift (recommended) or, only if Rama chooses it, an erratum
Partial answers are fine; an unanswered item stays where it is. An answer of A or B ratifies the exact proposed register text unless the answer changes its wording.

## 8. Current external facts (accessed 2026-10-21; each supports only the text quoted)
The broker pages below are broker-published, contextual evidence that market hours are changing. They do not replace the exchange's or SEBI's own documents, or a check against the broker's current rules and API before any V2 time value is set (VP-49). Claude read SEBI's circular of 15-Oct-2026 for the 7-Dec-2026 change: its page and the text of its PDF. The paragraph and table extracts below are Claude's reading, recorded in the source inventory. ChatGPT's final consistency review confirmed the circular's title, number and date from SEBI's page; it opened the PDF viewer but did not extract the PDF text line by line. Not read: the SEBI circular of 16-Jan-2026, which introduced the closing auction (as the 15-Oct circular says), and the exchanges' own circulars. Which stocks the auction covers is therefore taken here from the broker pages only.
- SEBI circular HO/47/11/11(3)2025-MRD-POD2/I/23843/2026, 15-Oct-2026, "Review of certain aspects of the Closing Auction Session, market timings and settlement methodology for derivatives contracts" (https://www.sebi.gov.in/legal/circulars/oct-2026/review-of-certain-aspects-of-the-closing-auction-session-market-timings-and-settlement-methodology-for-derivatives-contracts_105293.html; its text: https://www.sebi.gov.in/sebi_data/attachdocs/oct-2026/1792079571420.pdf):
  - para 4.2.1: "CAS shall be implemented as a separate session of 10 minutes from 3:30 p.m. to 3:40 p.m., on all trading days."
  - para 4.2.2: "The order entry session shall close randomly anytime between 3:38 p.m. to 3:40 p.m."
  - para 6.1: the changes other than those in para 4.8 "shall come into effect from December 07, 2026."
  - para 4.2.1's timing table, row 1: "Reference price calculation / Transition from CTS to CAS 3:30 p.m. 1 mins". INTERPRETATION: continuous trading (CTS) therefore ends at 3:30 p.m.; the text read does not say so in words.
- Zerodha Z-Connect, "Everything you need to know about Closing Auction Session (CAS)", 10-Jul-2026, with an update note of 16-Oct-2026 (https://zerodha.com/z-connect/general/everything-you-need-to-know-about-closing-auction-session-cas; the first pass cited the same article by another URL form, ?p=452459):
  - "Stocks that form part of CAS: trading stops at 3:15 pm."
  - "Stocks that don't form part of CAS: regular trading will continue till 3:30 pm."
  - "Category I: Stocks on which F&O trading is allowed"; "CAS will be applicable only for Category I stocks".
  - MIS square-off from 3-Aug-2026: "Equity (stocks under CAS): 3:12 pm"; "Equity (stocks not under CAS): 3:25 pm".
  - Update of 16-Oct-2026: "SEBI has announced changes to the Closing Auction Session (CAS), market closing timings, and F&O settlement methodology", effective 7-Dec-2026.
- Zerodha bulletin, "Changes to Closing Auction Session (CAS) from December 7, 2026", 16-Oct-2026 (https://zerodha.com/marketintel/bulletin/461085/changes-to-closing-auction-session-cas-from-december-7-2026), which links the SEBI circular above:
  - "Regular trading in all stocks, including those covered under CAS, will continue until 3:30 PM."
  - "CAS will be shortened from 20 minutes to 10 minutes, running from 3:30 PM to 3:40 PM."
  - UNKNOWN / NOT ESTABLISHED: Zerodha's MIS and CO square-off times from 7-Dec-2026; the bulletin says nothing about them.
- Relation to the record: Q-18 (CLOSED 06-Oct; design log line 116) holds Zerodha's MIS times from 3-Aug-2026: 3:12 PM for CAS stocks (the F&O list) and 3:25 PM for other stocks. That answer predates the 16-Oct announcement, so those times are not carried past 7-Dec-2026 without new evidence. VP-21 keeps the broker time in configuration.
- Motilal Oswal learning centre, "Introduction of Closing Auction Session (CAS) from 03 August 2026" (page dated 14-Aug-2026; https://www.motilaloswal.com/learning-centre/2026/8/introduction-of-closing-auction-session-cas-from-03-august-2026): F&O-eligible stocks "09:15 AM – 03:15 PM: Continuous Trading", then a transition and two auction order-entry sessions to a random close between 3:28 and 3:30 PM. A broker's summary, used only to corroborate the Zerodha page. Claude reached it on 21-Oct-2026, in the first pass and again before revision 1; ChatGPT's fetches of the same URL, in both its reviews, returned 404. It is Claude-retrieved corroboration only, not independently reverified, and no claim rests on it alone.

## 9. After Rama answers
1. Each answer is recorded as given, with any qualification Rama adds.
2. One register row per distinct decision, from DR-050: D-1 and D-2 when answered A or B; G-1 when answered (a) or (b). A deferred or moved item gets no decision row.
3. One closing row, "Section B close (ownership and scheduling only)", records the S-1 and S-2 moves as answered, T-1's outcome, G-1 if answered (c), and C-1 as answered (item 1 as recorded drift, or as an erratum only if Rama chooses it). It also records that the twelve legacy manuals are frozen (DR-035 to DR-049) and that the next step is Section C.
4. V2_DESIGN_LOG:
   - only the matching parts of VP-06, VP-16 and VP-19 are marked DECIDED, with VP-67's trading-day item if D-1 is A;
   - the moved rows get their tracking sentences, each saying "scheduling only" and leaving the proposal text unchanged;
   - the S-2 moves are recorded one lead at a time (16 entries), never grouped by owner;
   - Q-39 is updated as answered;
   - a "Section B end" agenda row is added, with the B-007 row put back in order;
   - session notes are added, including the b390ab0 publication (commit and file count only).
5. Before Rama stores anything:
   - an exact before/after diff of every changed file: the register (its new rows, including the Section B close row), the design log, and each affected V2 inputs file;
   - a check that each register row carries Rama's exact words, not a paraphrase that broadens the decision;
   - a cross-check of this paper against those files;
   - a re-run of the VP and Q disposition audit (every VP ratification column and every Q owner segment), so that no completed B topic stays a ratification point or owner unless Rama chose to keep it;
   - checks that conditional moves follow the matching answer and only accepted parts are marked DECIDED; that Q-39's status matches G-1; that the B-008 addendum is unchanged if drift is chosen; and that the B-007 row order and the B-012 session note are right;
   - a scan of the changed files for identifiers, addresses, secrets and session links;
   - the complete diff presented to Rama for approval. Claude and ChatGPT commit or push nothing; Rama stores and pushes.
6. No frozen legacy manual or addendum changes, except as G-1 (a) or (b), or C-1 item 1's erratum alternative, specify, if Rama chooses them.

Status and boundary: the Section B end decisions are CLOSED and recorded in DR-050 to DR-052. Proposals and parts not explicitly decided here remain PROPOSED or OPEN in V2_DESIGN_LOG.md with their owning topics. These decisions do not authorise implementation; the post-agenda legacy audit (DR-016) and the remaining governance and design steps still apply. These remain open:
- Q-01 (pre-live validation), Q-05 (environments), Q-11 (fresh credentials) and Q-21 (the legacy ETF and SME path).
- The other questions still open or partly answered in V2_DESIGN_LOG section 2 (Q-03, Q-04, Q-06, Q-12 to Q-15, Q-20, Q-23, Q-25, Q-26), with their owners.
- Every audit lead in the Audit rows (Q-30, Q-31, Q-34 to Q-38, Q-40 to Q-45), for the post-agenda legacy audit (DR-016).
- The attribution of the 15-Aug ruling (VP-57, DR-044).
- Q-43 (h), the HARD_KILL history.
- VP-62 (one trading process per session or one long-lived process), with VP-22, deferred to M-001 and O-010 (T-1).
- Every proposal not named in sections 4 and 5, and every undecided part of those named, with its current owners.
No V2 implementation is authorised (DR-016).

## 10. Rama's answers (21-Oct-2026) and the register rows

| Item | Rama's answer, as given | Register row |
|---|---|---|
| D-1 trading days and the session calendar | "A: Ratify principle (Recommended)", with the exact proposed register text shown (section 4, D-1) | DR-050 |
| D-2 the entry cut-off applies to intraday only | "A: Ratify principle (Recommended)", with the exact proposed register text shown (section 4, D-2) | DR-051 |
| G-1 Q-39, the "Source priority" lines | "(c) No change, close Q-39 (Recommended)" | DR-052 (Q-39 closed) |
| T-1 when to decide VP-62, with VP-22 | "(i) Keep deferral (Recommended)" | DR-052 |
| S-1 and S-2 scheduling moves | "Accept both (Recommended)" | DR-052 |
| C-1 documentation corrections | "Accept; addendum as drift (Recommended)" | DR-052 |

Rama answered through Claude's multiple-choice questions (DR-017, W-4). The D-1 and D-2 options showed the exact proposed register text, which DR-050 and DR-051 carry word for word.

What the records now show:
- V2_DESIGN_LOG marks as decided only VP-06's start-and-stop part, VP-16's calendar and start parts, VP-19's cut-off part and VP-67's "valid trading day" item.
- The S-1 rows and the sixteen S-2 leads are moved one by one, each with a "scheduling only" tracking sentence; every lead stays OPEN.
- Q-39 is closed under DR-036 and W-7, with no change to the frozen A-005 to A-011 manuals.
- The frozen B-008 reconciliation addendum is unchanged; its stale status line is recorded in DR-052 as KNOWN DOC DRIFT.
- Six owner cells in the B-008, B-009, B-011 and B-012 V2 inputs are corrected, owner text only.
- In the agenda table, the B-007 row is back in order and the Section B end row is added; the B-012 publication note and three lessons are added.
- Every other part stays PROPOSED or OPEN with its owner.

## Changes in revision 1 (after ChatGPT's value-added review, 21-Oct-2026)
- D-1: "at least the current and the next calendar year" and the reading of "failing safe" marked as proposed wording for Rama to accept; the decision limited to trading initialisation, with the service, non-trading jobs and monitoring left to M-001, M-002 and M-006; the calendar's source, format, refresh, special sessions and validation named as undecided.
- D-2: the end of regular trading stated as no permission to enter in a closing auction session (G-001 stays open); scope sentence added; broker order acceptance and broker-specific timing named as owned elsewhere.
- Section 8: the broker pages labelled as broker-published context; SEBI's circular of 15-Oct-2026 read; post-7-Dec MIS square-off times marked UNKNOWN.
- C-1 item 1: recording the drift, with no change to the frozen addendum, made the recommendation; the erratum kept only as an alternative.
- S-1 and S-2: conditional rows tied to their answers; each lead recorded on its own; the configuration design step identified as the design log's tracking row.
- Section 9: an exact diff, a cross-check and a re-run of the VP and Q disposition audit required before storage; an answer of A or B stated to ratify the exact proposed text.
- Each point's disposition is in CLAUDE_RESPONSE_TO_CHATGPT_VALUE_ADDED_REVIEW_SECTION_B_END_21-OCT-2026.txt.

## Changes in revision 2 (after ChatGPT's final consistency review, 21-Oct-2026)
- C-1 item 2: the replacement owners described by kind (a Master Agenda topic or section, a VP row, or a design-log tracking row), with "Configuration design (build phase)" named as a tracking row, not a Master Agenda topic.
- Section 8: the SEBI extracts attributed to Claude's reading, with what ChatGPT's check covered; the Motilal Oswal page labelled Claude-retrieved corroboration only; Q-18's MIS times not carried past 7-Dec-2026 without new evidence.
- Section 9: ChatGPT's post-answer quality gates added.
- No finding, option or recommendation changed.

## Changes at closure (21-Oct-2026; documentation only)
- Title marked CLOSED; the status line, the place line and the closing status-and-boundary paragraph replaced; section 10 and these change notes added. Sections 1 to 9 are otherwise as presented to Rama.
