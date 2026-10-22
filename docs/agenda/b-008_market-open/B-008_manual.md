# B-008 MARKET-OPEN OPERATION: LEGACY MANUAL

Status: FROZEN 2026-10-20 (revision 1, DR-045). ChatGPT's value-added review, final acceptance review and final value-added review (all 20-Oct-2026) were applied (B-008_review_notes.md sections 6 and 8); Rama approved the freeze on 20-Oct-2026. No erratum to a frozen manual. Erratum E-1 to this manual (section 3: the request audit's coverage at ingress) added 2026-10-21 during C-002, accepted in principle in ChatGPT's value-added finalisation review of C-002 (22-Oct-2026), approved by Rama on 22-Oct-2026 and recorded in DR-055 (2026-10-22) together with the C-002 freeze.
Date: 2026-10-20 (revision 1)
Scope: legacy facts only; a legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; W-1). No VM, service, host or broker operation and no change to legacy source; this manual records a retired system, not an operating procedure.
Working copy of: docs/inputs/manuals/B-008_MARKET_OPEN_MANUAL_DRAFT_19-OCT-2026.md (ChatGPT's draft, with its reconciliation and Rama's answer sheet of 17-Oct; to be stored unchanged at the freeze). The draft left the pre-10:00 signal path, the strategy count and the commit unpinned; this manual traces them at the baseline.
Labels: as B-007_manual.md (FACT (SOURCE-VERIFIED) = trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb, "the baseline", read on 2026-10-20; FACT (RECORD-DERIVED); HISTORICAL (Rama) = his corrections in the answer sheet; RAMA (EXPECTED) = the generated answers, "a torchlight" only; INTERPRETATION; UNKNOWN / NOT ESTABLISHED). The draft's evidence E4 (a main.py object ID with no commit) is replaced by the baseline commit. Search scope for any "not found": the INSPECTED SCOPE of B-007_manual.md (330 Python files outside tests/ and 28 non-Python runtime files at the baseline). What a host ran is NOT ESTABLISHED without a dated record (W-7).
Additional record (Rama's, reference only, not stored, DR-008): the Cron Morning Briefing e-mail of Thu 24-Sep-2026 09:20 (PDF, two pages), cited by date and content only; it carries an account identifier that is not reproduced.

## 1. Purpose and boundary
B-008 records what the legacy service did between the end of the start (B-006, B-007) and the first entry of the day: the exchange's open against the system's entry window, what happened to a Chartink signal before the window opened, the pre-market schedule around it, and the strategy set. Readiness and health are B-007's; the feed and ticks B-004 section 10 and B-007 section 9; intraday operation after the first entry B-009. B-005, B-006 and B-007 are FROZEN (DR-041, DR-043, DR-044); B-008 cites their findings without reopening them.

### 1.1 Evidence matrix

| Question | Source at 970aabf proves | 24-Sep briefing reports | Rama recalls | Unknown |
|---|---|---|---|---|
| When entries could start | Global window 10:00 to 15:00 (section 2) | Strategy rows with 09:25 to 15:00 windows | Trading starts at 10:00 | Whether production ran 10:00 every day |
| Alert before 10:00 | Intraday webhook path: 403 before parsing, audit row only (section 3) | - | No processing before 10:00 | Chartink retries; untraced routes |
| Strategy set | 16 files in config/strategies/ | 15 rows, without pb01_breakout_retest | 16 strategies | Which were enabled or eligible that day |
| Pre-market jobs | Registry and scripts exist (B-001) | 08:15 to 09:15 jobs done; READY · warnings | Boot ~08:15, headless refresh, pre-flight | Host of the e-mail; job side effects |
| Weekends, holidays | Holiday guard exits 0 (B-007 G1) | - | No start | Every historical day |

## 2. Exchange open and the system's entry window (FACT, SOURCE-VERIFIED unless marked)
- HISTORICAL (Rama): the NSE regular session opens at 09:15 IST.
- The global entry window is config/system_config.yaml trading_hours.entry_start "10:00" and entry_end "15:00" (lines 27 to 47). The comment calls 10:00 a "[LAUNCH-PHASE]" floor (line 28) over the strategies' own windows: "the 16 strategies declaring entry_start_time "09:25" cannot trade at 09:25 — the floor binds" (lines 29 to 39). eod_entry_cutoff "15:15" is a separate order-placement deadline (line 48; orders/order_placer.py line 1077).
- main.py builds MarketWindows from these values at Phase 0d (lines 2316 to 2325). core/market_windows.py: is_entry_allowed is false on a trading holiday and otherwise true for entry_start <= now < entry_end (lines 143 to 150); is_entry_allowed_for_strategy requires the global window and then the strategy's entry_start_time/entry_end_time, and falls back to the global window alone if those are missing or malformed (lines 152 to 175).
- So: NSE regular-session open 09:15 (HISTORICAL, Rama); configured system entry floor 10:00 (source); 10:00 is not the exchange open, and the strategies' 09:25 windows shown in the briefing do not override it. The floor gates entries only: the service, its workers and the pre-market jobs ran before 10:00 (B-006, section 5), so nothing here means that all processing waited for 10:00. Whether production ran a different value on any day is NOT ESTABLISHED (no dated host configuration read).

## 3. A Chartink signal before 10:00 (FACT, SOURCE-VERIFIED for the intraday webhook path traced; the draft's B8-OQ4)
Scope: the path of an intraday Chartink alert through /webhook/<scanner> at the baseline; other routes (the EOD scanner's capture, the entry gate and retest resumptions) are named where they meet a time check, and no other ingress was traced. In order, in signals/webhook_receiver.py (_handle_webhook, from line 477, calling _process_request, from line 536):
1. The request passes the shutdown flag and per-IP rate limit (to line 504), authentication (401, lines 547 to 549) and the known-scanner check (404, lines 552 to 554).
2. A scanner whose scanner_type is "eod" leaves here for the watchlist capture and skips the window check (lines 556 to 564).
3. Kill switch active: 403 (lines 566 to 568). Queue at the backpressure threshold: 503 (lines 570 to 578).
4. Outside the global entry window: 403 "Outside entry window" (lines 580 to 583), before the body is parsed, so no signal row is stored and nothing is queued; the request leaves only the webhook audit row written in the finally block (lines 529 to 534) and the HTTP 403 to Chartink.
- So before 10:00 an intraday Chartink alert on this path is received, authenticated and refused; it is not evaluated, stored as a signal or queued, and its only trace is the webhook audit row. HISTORICAL (Rama): signals and orders were not processed for trading before 10:00; the code-level finding is narrower: the refusal at the webhook. Whether Chartink retried a 403 is NOT ESTABLISHED (outside the repository).
- Defence in depth, later in the pipeline: signals/signal_processor.py re-checks the global window (line 911) and then the strategy's window (lines 974 to 982) for a queued signal, and the two resume paths (the entry gate and the retest monitor) re-check the global window at the last mile (lines 2041 to 2044 and 2399 to 2401); a failure is the recorded status OUTSIDE_ENTRY_WINDOW.
- Stage table (each row: source, input, logic and guard, output, storage or hand-off, next consumer):

| Stage | Source (970aabf) | Input | Logic / guard | Output | Storage / hand-off | Next |
|---|---|---|---|---|---|---|
| Ingress | webhook_receiver.py _handle_webhook, 477 to 534 | HTTP POST from Chartink | shutdown flag, per-IP limit | 503 or 429, or on | ~~audit row (531 to 534) on every request~~ (superseded by erratum E-1) audit row (531 to 534) on every request whose body is read, unless the insert fails (C-002_manual.md section 10.3) | _process_request |
| Door checks | _process_request, 536 to 583 | raw body | auth (549), scanner (554), EOD route (564), kill (568), queue (578), global window (583) | 401, 404, 403, 503, or on | No signal row or queue on refusal; the webhook audit row remains (lines 529 to 534) | parsing |
| Parse and claim | _process_request to _process_signal, 585 onward and 892 to 1060 | JSON body | field checks, in-flight claim (934), duplicate check | signal row | store transaction (964 to 1053), queue.put_nowait (1049) | signal processor |
| Pipeline | signal_processor.py _process_one, 867 onward | queued signal | kill (906), global window (911), expiry (917), strategy control (959), strategy window (977) | reject with status, or on | signal status in the store | _admit_and_place (1277) |
| Resumptions | continue_from_gate (2010), continue_from_retest (2375) | parked signal | kill, global window (2043, 2400) | reject or placement | signal status | order placer |
| Order boundary | orders/order_placer.py | admitted signal | eod_entry_cutoff (1077) | broker order | orders store | B-009 |

```text
Chartink alert ──> ingress ──> door checks ──> [before 10:00] 403 "Outside entry window" (audit row only)
                                           └─> [10:00 to 15:00] parse, store, queue ──> pipeline: window again, strategy window
                                                                                          └─> admit and place (before 15:15)
```

- INTERPRETATION: a signal accepted at 09:59:59 and processed after 15:00, or a parked signal resumed outside the window, is the case these re-checks cover; no dated occurrence was read.
- The PB-01 watchlist's next-morning entry stage has its own window 09:20 to 11:00 (config lines 619 to 627). Whether its entries also passed the global 10:00 floor depends on which resume path they used, which was not traced: NOT ESTABLISHED (B-009).

## 4. The strategy set (16 against 15)
- FACT (SOURCE-VERIFIED, tree listing at the baseline): config/strategies/ holds 16 YAML files: first_pullback_long and _short, gap_fade_long and _short, gap_go_long and _short, open_high_breakdown_short, open_low_breakout_long, pb01_breakout_retest, positional_momentum_long, positional_sector_rotation, positional_swing_long, range_breakout_long and _short, vwap_bounce_long, vwap_rejection_short. HISTORICAL (Rama): 16 strategies. The config comment counts "16 strategies declaring entry_start_time 09:25" (line 32) and, for entry_end, "all 15 strategies" (line 44).
- FACT (RECORD-DERIVED, 24-Sep briefing, page 2): 15 rows, the same names without pb01_breakout_retest. This explains the displayed count at the list level only; it does not show which of the 16 were enabled, eligible or meant to trade that day, and the briefing has no values for pb01. The table comes from scripts/cron_officer.py _strategy_status_payload via scripts.strategy_status (lines 461 to 470); why pb01 is absent was not traced. INTERPRETATION: pb01 is the EOD scanner's strategy (config line 621), routed to the watchlist, not the intraday table. The strategy YAMLs' contents were not read for this pass.

## 5. Pre-market schedule (RECORD-DERIVED unless marked; B-001 owns the start, B-007 the pre-flight)
- The 24-Sep briefing (09:20, Mode Live, run 20260924_091502_c, 18 of 45 plan items done) reports done: 08:15 auto_refresh_token, 08:20 monitoring_canary, 08:30 preflight_phase_a, 08:35 fetch_fno_ban, 08:55 gemini_premarket_brief, 09:00 refresh_instruments, 09:14 preflight_phase_b, 09:15 preflight_phase_c; cron_officer_briefing 09:20 pending; pre-flight "READY · warnings", 0 critical, 1 warning (not named). It is the briefing's report, not job output; its host is not named in it, and it shows no order. These jobs prepared and monitored; none is wired to entry permission in the inspected path (B-007 section 10).
- The service start: the token watcher after the 08:15 refresh, on the recorded days (B-001 section 6; B-007 section 3.1). HISTORICAL (Rama): earlier, the token was refreshed on the PC and copied by a .bat file; later headless (A-010 section 7).
- Weekends and holidays: the start ends at the holiday guard with exit 0 (B-007 G1; B-001 section 7); the 08:15 refresh skips non-trading days (B-001 section 7). HISTORICAL (Rama): no start on weekends or holidays; the source is consistent but does not show what the production scheduler did on every such date. The generated answer's "boots and idles on a holiday" is rejected.
- The pre-flight is alert-only (B-007 section 7); the briefing's "READY · warnings" is reported as written, and whether orders were permitted that day is not inferred from it. scripts/premarket_healthcheck.py is marked DEPRECATED (draft section 4.4; not re-read).

## 6. Feed, candles and the first order
- No tick reached the service up to 25-Jul (nothing subscribed; B-007 section 9; B-006 section 6), so "ticks at 09:15" and tick-built first candles (RAMA (EXPECTED)) do not describe the inspected path. How strategies obtained candles at the open (REST fetches) is B-009's.
- The first possible entry of a day: an intraday alert at or after 10:00 that passes the receiver, the processor's checks (global and strategy window, kill switch, expiry, strategy control) and the risk and capital stages; its order is placed only before eod_entry_cutoff. No dated first-order record was read.

## 7. Rama's answers against the source (summary; RAMA (EXPECTED) unless marked)
- Right: entry start 10:00 not 09:15 (Rama's correction); NSE equity via Chartink (Rama's correction of the NFO/options answers, which have no source); no start on weekends or holidays; headless token refresh at 08:15.
- Not matching: options, CE/PE, option chains; ticks and candles from 09:15; idling on holidays; every failed check calling sys.exit(1) (B-007 section 5); manual OAuth at 08:30 (earlier stage only).
- No record found for the generated incidents (HTTP 429, WebSocket drops, SQLite locks, restarts).

## 8. Known defects and ambiguities
1. KNOWN AMBIGUITY: the config comments say 16 strategies (line 32) and 15 (line 44); 16 files exist; the briefing shows 15.
2. The strategies' 09:25 windows were inert under the 10:00 floor (by the config's own comment); the floor was labelled temporary ("LAUNCH-PHASE").
3. On the inspected intraday Chartink webhook path, an alert received before 10:00 was rejected with HTTP 403 before parsing and was not retained as a signal or queued; the request was still represented by a webhook audit row. The effect on other ingress or resumption paths was not established.

## 9. Unknown / not established
The briefing's host; the warning behind "READY · warnings"; whether Chartink retried refused alerts; whether PB-01 entries passed the 10:00 floor; the first order of any day; whether production ran the repository's 10:00 value on every day; the installed crontab (B-001).

## 10. Final B-008 statement
Rama identifies 09:15 IST as the NSE regular-session open; the inspected baseline configures the system's entry floor at 10:00 IST, a launch-phase global floor over the strategies' own 09:25 windows. On the inspected intraday webhook path, an alert before 10:00 was authenticated and then refused with 403 before parsing: no signal row, nothing queued, one webhook audit row; later stages re-checked the window, and other routes were not fully traced. The pre-market hours carried the 08:15 token refresh, the service start by the watcher and the alert-only pre-flight, none of which gated entries. Sixteen strategy files existed; the morning briefing listed fifteen, without the EOD PB-01 strategy. These are legacy facts, not V2 requirements.

## Changes from ChatGPT's 19-Oct draft
- Pinned to the baseline commit; the pre-10:00 path traced (section 3); the 16/15 difference identified (pb01_breakout_retest); B-005, B-006 and B-007 statuses updated (all frozen).
- Kept: the evidence classes, the rejection of the options and holiday-idle answers, the separation of declaration, installed state and record.

## Revision 1 (2026-10-20)
ChatGPT's value-added review of 20-Oct-2026 ("REVIEW FEEDBACK — NOT A FREEZE"): applied the scope of the 403 finding and its audit row (section 3), the stage table and flow diagram, the evidence matrix (section 1.1), the qualification of 16 files against 15 rows (section 4), the schedule as preparation (section 5), the holiday wording and the frozen status of B-005 to B-007. No finding changed. ChatGPT's final acceptance review of 20-Oct-2026 ("ACCEPT WITH REQUIRED EDITS") narrowed section 8 item 3 and the final statement's evidence class (Rama's 09:15). The reconciliation's corrections are in B-008_reconciliation_addendum.md; the original stays unchanged.

## Erratum (2026-10-21, found during C-002)
- E-1 (section 3, the stage table's Ingress row). Status: ACCEPTED. Proposed during C-002 (revision 0, 2026-10-21), accepted in principle in ChatGPT's value-added finalisation review of C-002 (22-Oct-2026), approved by Rama on 22-Oct-2026, independently of the C-002 freeze and of the other errata, and recorded in DR-055 (2026-10-22) together with the C-002 freeze. The struck text stays visible; the replacement is the text in force. Authoritative passage: C-002_manual.md section 10.3 (NR-1 to NR-4).
- What changed: the row gave the audit row "on every request". A row is written only for a request that reaches the handler and whose body is read, and only if the insert succeeds; the framework's own answers, a body over 1 MiB and a failed insert leave none (C-002_manual.md section 10.3, NR-1 to NR-4).
- Passages checked and not changed: section 3's finding that an intraday alert before 10:00 is refused with 403 and that "its only trace is the webhook audit row" (the 403 is answered after the body read, so the row is written unless the insert fails); section 1's stage summary; section 9 (retries NOT ESTABLISHED). B-008_reconciliation_addendum.md item 2 stays unchanged as a record; its "the webhook audit row is written for every request (lines 529 to 534)" is superseded by this erratum.
- Reason: a docstring's "regardless of outcome" is a claim; the body read and the framework's own answers come before or outside the audit write (V2_DESIGN_LOG lesson of 2026-10-07).
- Impact: wording only; C-001 erratum E-1, A-010 erratum E-4 and B-009 erratum E-2 cite the same passage. No V2 decision depends on the old wording.
