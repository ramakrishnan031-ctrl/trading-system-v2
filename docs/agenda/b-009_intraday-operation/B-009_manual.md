# B-009 INTRADAY OPERATION: LEGACY MANUAL

Status: FROZEN 2026-10-20 (revision 2, DR-046). ChatGPT's value-added review and its revision-2 corrections (both 20-Oct-2026) were applied (sections 19 and 20); Rama approved the final files on 20-Oct-2026. No erratum to a frozen manual.
Date: 2026-10-20 (revision 2; revisions 0 and 1 the same day)
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; W-1). No VM, service, host or broker operation and no change to legacy source; this manual records a retired system, not an operating procedure.
Working copy of: docs/inputs/manuals/B-009_INTRADAY_OPERATION_MANUAL_DRAFT_20-OCT-2026.md (ChatGPT's draft). With it come ChatGPT's reconciliation of 20-Oct, the answer sheet and prompt of 20-Oct, and Claude's repository answers of 20-Oct; all are stored unchanged at the freeze. The draft carried its source claims as "pending recheck"; this manual rechecked them at the baseline (B-009_review_notes.md section 3) and corrects two of Claude's own answer lines (B-009_reconciliation_addendum.md item 1).
Labels: FACT (SOURCE-VERIFIED) = trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb, "the baseline", read 2026-10-20 through the masking filter; "file L-L" are line numbers there. FACT (RECORD-DERIVED) = a dated record, true for its own date and deployed commit, never the baseline unless it says so. INTERPRETATION = a reading of source or records, limits stated. KNOWN DEFECT = a source-path defect with code evidence, not a measured outcome. UNKNOWN / NOT ESTABLISHED. DEFER TO <topic>.
Security: no credential, token, account or client identifier, host, address or person name is reproduced.

## 1. Purpose and boundary
B-009 records what the legacy service did between the first entry of the day (B-008, FROZEN, DR-045) and the hand-off to end of day (B-010):
- signals arriving and being processed;
- the prices it used;
- entries, fills and partial fills;
- how open positions were watched;
- the cut-offs and kill states that stopped entries;
- what it did when something failed.
- Out of scope, and cross-referenced only: start-up (B-001 to B-006), readiness (B-007), market open (B-008), end of day (B-010), shutdown (B-011), restart (B-012), and the detail owned by the C to O topics (section 14).

## 2. Evidence
| Evidence | What it covers | Limit |
|---|---|---|
| Baseline source | 330 Python files outside tests/, all checked out; selected files read (section 15) | Source is not proof of what was installed or ran on the VM on any date |
| Strategy files | config/strategies/, 16 YAML files, selected keys | Configuration, not observed eligibility |
| Records in the production repository | Four records: the census of 19-Jul, the exit-rejection forensics of 22-Jul, the orphan-adoption record of 22-Jul, and the incident of 03-Sep (section 13) | Each at its own deployed commit; the census cites 7319d06, the incident 2d08436 |
| Frozen manuals | B-005 to B-008 | Cross-reference only; not reopened |
| Not inspected | The sandbox and records repositories | Rama's instruction of 20-Oct-2026 |
| Operator recollection | None supplied for B-009 | Operator-only questions stay UNKNOWN |

## 3. The session model (FACT, SOURCE-VERIFIED unless marked)
- There is no session state. No flag, event or database row marks the change from market open to the session.
  - Searched across all 330 runtime files for a session flag, a session-open event or "INTRADAY_ACTIVE": none.
  - "intraday_active" in capital/fund_manager.py 116-142 and main.py 2693-2713 is a capital-split input for the active trade types.
- Each gate reads the clock when it is consulted:
  - the webhook, per request: signals/webhook_receiver.py 580-583;
  - the processor, per signal: signals/signal_processor.py 909-912 for the global window and 974-982 for the per-strategy window;
  - the placer, before each broker call: orders/order_placer.py 1073-1097.
- The window test is core/market_windows.py 143-150: not a holiday, and entry_start <= now < entry_end. The end is exclusive.
- The time controls are distinct and must not be merged:

| Time (IST) | Control | Owner of the code path | What it does | What it does not do |
|---|---|---|---|---|
| 10:00 | Global entry start (config/system_config.yaml 28) | MarketWindows, at the three gates above | Opens entries; floors every strategy's 09:25 | - |
| 15:00 | Global entry end (config 47) | Same | Closes eligibility (exclusive) | Does not cancel or close anything |
| 15:03 | MIS PASS 1 (config 59, 90) | orders/mis_autosquareoff.py, poller thread (B-006 TH3) | Price-seeking exit of MIS positions, limit then market (mis_autosquareoff.py 32-36) | - |
| 15:06 | MIS PASS 2 (config 91) | Same | Certainty exit, market, no grace | - |
| 15:09 | MIS protective cutoff (config 59) | Same | The deadline both passes are timed against | Not an action itself |
| 15:15 | Placement cut-off, eod_entry_cutoff (config 48) | OrderPlacer (order_placer.py 1076-1097) | Refuses any order; the trade is marked REJECTED and the reservation released | - |
| 15:15 | Tracked-entry cancellation and SOFT_KILL; historical source name "force-close" (circuit_breaker.force_close_time, config 740) | Order monitor's poll cycle (broker/order_monitor.py 614-615, 794-830), callback main.py 688-724 | Cancels every tracked ENTRY order, then SOFT_KILL | Closes no position (main.py 693-701; mis_autosquareoff.py 7-8) |
| 15:17 | EOD routine (config 49) | orders/eod_squareoff.py 326 onward, poller every 5 s (config 361) | Accounting and emergency backstop | DEFER TO B-010 |

- The 15:15 entry cancellation runs inside the order monitor's 2-second cycle. If that monitor has stopped (section 8, D3), the cancellation and its SOFT_KILL do not fire. The 15:15 placement cut-off is separate and does not depend on the monitor. INTERPRETATION from 594-615; no dated case is known.

## 4. Strategies at the session (FACT, SOURCE-VERIFIED)
- Inventory: 16 files in config/strategies/ (B-008 section 4). The loader loads every file and does not filter on `enabled` (strategies/loader.py 128-149).
- Eligibility is decided per signal by strategy control (signal_processor.py 955-972; strategies/control.py 18-24): a strategy trades only if all three hold:
  - it is enabled;
  - trade_type permits its intent;
  - the force_intraday_only breaker does not block it.
- At the baseline, trade_type is BOTH, force_intraday_only false and delivery_enabled true (config 142, 149, 155).
- So, by configuration, 15 strategies were eligible and 1 was not:

| Group | Files | Intent | Declared order_protocol | Window | pullback_wait_enabled |
|---|---|---|---|---|---|
| A | first_pullback_long, first_pullback_short, open_high_breakdown_short, open_low_breakout_long, vwap_bounce_long, vwap_rejection_short | INTRADAY | CO_PLUS_TGT | 09:25-15:00 | true |
| B | gap_fade_long, gap_fade_short, gap_go_long, gap_go_short, range_breakout_long, range_breakout_short | INTRADAY | CO_PLUS_TGT | 09:25-15:00 in every YAML (gap_fade_long: see the drift note below) | false |
| C | positional_momentum_long, positional_sector_rotation, positional_swing_long | DELIVERY | LIMIT_TRIPLE | 09:25-15:00 | false |
| Not eligible | pb01_breakout_retest (enabled: false, line 30) | INTRADAY | CO_PLUS_TGT | 09:25-11:00 | false |

- pb01_breakout_retest is disabled. Its capture and entry-stage workers (B-006 TH9, TH10) are a separate path, not ordinary strategy eligibility.
- The 24-Sep briefing's 15 rows (B-008) are a report list, not this eligibility.
- Group C is delivery (A-007 owns product types). It is in B-009 only because it enters through the same session path.
- Windows: both the global and the strategy window apply and the narrower wins (market_windows.py 152-175). The global 10:00 binds the start of every eligible strategy, since each YAML declares 09:25.
- The code reads each strategy's end from its YAML (169-175). Every eligible YAML declares 15:00, so in the traced code the end is 15:00 for all of them, gap_fade_long included.
  - A malformed strategy time falls back to the global window (171-173).
- KNOWN DOC DRIFT:
  - market_windows.py 160-162 (a docstring) and signal_processor.py 975-976 (a comment) say gap_fade_long cuts off at 11:30; gap_fade_long.yaml line 50 says 15:00.
  - No code holds 11:30 for gap_fade_long: a search of the 330 runtime files finds 11:30 only in that docstring and comment and in unrelated report, dashboard and test files.
  - Which value was intended is not established, and this conflict is left unresolved.
  - The docstring default "09:30 / 13:30" (market_windows.py 164) matches no file.
  - The config comments count 16 (line 32) and 15 (line 44) strategies; B-008 known ambiguity 1.

## 5. Signal lifecycle (FACT, SOURCE-VERIFIED; C topics own the detail)

```text
POST /webhook/<scanner>            signals/webhook_receiver.py
  authentication .......... 401    (549)
  unknown scanner ......... 404    (554)
  EOD-type scanner ........ separate handler (562-564)
  kill switch active ...... 403    (567-568)
  queue >= 80% of 300 ..... 503    (570-578; config 125-126)
  outside entry window .... 403    (580-583)   body not parsed
  parse body .............. 400 on malformed
  per symbol (895-1072):
    invalid symbol/price .. INVALID_*          response only
    age > 600 s ........... EXPIRED            response only (916-926; config 127)
    symbol already claimed  IN_PROCESS         response only (934-935)
    (symbol, scanner) seen
      within 300 s ........ DUPLICATE          response only (937-947; TTL 237, config 337)
    fingerprint row exists  DUPLICATE          or re-queue of a QUEUE_FULL row (981-1015)
    INSERT signals ........ QUEUED             (963-980)
    queue full ............ QUEUE_FULL         row kept, claims rolled back (1050-1068)
        |
        v  in-memory queue
sp-dispatcher -> pool of 5 workers   signals/signal_processor.py 377-399
  PROCESSING (897)
  kill switch .................. REJECTED_KILL_SWITCH (905-907)
  global window ................ REJECTED_OUTSIDE_ENTRY_WINDOW (909-912)
  age > 600 s .................. REJECTED_EXPIRED (914-921)
  shadow inning ................ REJECTED_SHADOW_INNING_ACTIVE (928)
  scanner -> strategy .......... REJECTED_UNKNOWN_STRATEGY (933-953)
  strategy control ............. REJECTED_STRATEGY_CONTROL / _TRADE_TYPE (955-972)
  strategy window .............. REJECTED_OUTSIDE_ENTRY_WINDOW (974-982)
  strategy circuit breaker ..... REJECTED_STRATEGY_CIRCUIT_BREAKER (984-990)
  secondary screen ............. PASSED / REJECTED_<step> / SKIPPED_<reason> (998-1020)
  entry and SL derived ......... (1028-1030); re-anchored on a fresh quote only if
                                 pullback_wait_enabled is false (1081-1106)
  sizing ....................... REJECTED_SIZING_* (1111-1128)
  under portfolio lock (1302-1344):
    per-strategy position cap, one trade per symbol and direction per day (on, config 302),
    risk approval, capital reservation ......... RESERVED
  target derived; "INTRADAY SIGNAL" alert sent (1368-1380)
  kill switch again ............ REJECTED_KILL_SWITCH_LATE (1389-1396)
  shutdown ..................... REJECTED_SHUTDOWN (1398-1405)
  entry throttle ............... REJECTED_ENTRY_THROTTLED (1410-1418)
  OrderPlacer.place() (1446-1460)
    success .................... PROCESSED (1543)
    broker timeout ............. TIMEOUT; trade UNKNOWN_IN_FLIGHT; reservation left to the
                                 reconciler (1464-1484)
    client rate limit .......... re-queued, at most 3 times (1486-1532)
    other failure .............. PLACEMENT_FAILED (1233-1246)
```

- On any REJECTED_* the reservation, if any, is released (1193-1217). The in-flight claim is released on every path, except re-queue and an allocator hand-off (1248-1271). The allocator is in shadow mode, so no hand-off happens.
- An EXPIRED reject at the processor sends a warning at most once a minute (1221-1231).
- The webhook's EXPIRED, IN_PROCESS and DUPLICATE answers are not signals rows. The webhook audit row is written for every request (B-008 section 3).
- The signal age limit is 600 s. The processor's constructor default of 60 (signal_processor.py 175) is overridden by main.py 3613-3614 with config 127; the webhook reads the same value (webhook_receiver.py 679).
  - The order monitor's 60 s is a different control: the unfilled-entry timeout (section 7).
- No path parks a signal for a later window. The retest diverter is configured off (B-006 TH11).
- The same pipeline is repeated in continue_from_gate (2010 onward) and continue_from_retest (2375 onward). No source path at the baseline reaches either for a newly processed signal: KNOWN DEFECT D1, and the diverter is off.

## 6. Prices and market data (FACT, SOURCE-VERIFIED unless marked; E topics own the detail)
- Every decision quote is a REST call. main.py passes broker_adapter.get_quote to:
  - the screener (3281-3285);
  - the processor (3575, 3616);
  - the entry gate (3839-3840);
  - the reconciler (3124, 3133);
  - the smart-target manager (2973, 2978);
  - the delivery GTT placer (3027, 3033).
- Live get_quote calls the broker's quote endpoint and builds one Quote per symbol returned (broker/zerodha_adapter.py 1757-1806). A symbol missing from the reply is simply absent: no partial-reply check on this branch (1771-1798).
- In paper mode get_quote calls the injected provider (1742-1755; main.py 2394). That provider logs a WARNING naming missing symbols (main.py 552).
- The WebSocket feed connects in live mode but subscribes to nothing at start. Its only subscriber is the exit retry (orders/order_placer.py 3397-3412). Ticks therefore do not reach any entry decision, and the 60-second candles built from ticks (main.py 2964-2971) have nothing to build from (B-006 section 6).
  - FACT (RECORD-DERIVED, 25-Jul, B-006 sections 6 and 14): the tick/candle path was recorded as dormant.
  - A connected feed is not a receiving feed.
- Quote failure:
  - the screener marks SKIPPED_QUOTE_UNAVAILABLE and the processor drops the candidate, with no retry and no worker stop (secondary_screener.py 170-193; signal_processor.py 1008-1013);
  - a failed momentum re-anchor logs a warning and keeps the stale webhook price (1088-1106).
- Four prices: the webhook trigger; the derived entry (with an offset, or re-anchored); the submitted LIMIT price; and the fill.
  - The placer's slippage guard compares against the trigger (order_placer.py 1099 onward).
  - The P1 accept record carries trigger, final entry, the re-anchor flag, SL, target and quantity (signal_processor.py 1427-1445).
  - Formulas: DEFER TO D, E and G.

## 7. Entries, fills and the order monitor (FACT, SOURCE-VERIFIED; G topics own the detail)
- Protocol actually selected: OrderPlacer uses self._default_protocol (order_placer.py 961). Its constructor default is "LIMIT_TRIPLE" (572), and main.py's construction (3061-3087) passes no protocol. All three callers of place() (signal_processor.py 1446, 2251, 2553) pass no protocol either; the strategy's order_protocol is never read on this path. KNOWN DEFECT D2.
  - LIMIT_TRIPLE is two-phase. The entry is a LIMIT order; SL (stop-limit) and TGT (LIMIT) are placed at the actually filled quantity once the entry fills (order_placer.py header 116-130; config 204).
- Pending entry:
  - the trade is set PENDING before the broker call (1061-1071);
  - the order is tracked (order_monitor.py 252-310) and polled every 2 s (config 193; 594-631);
  - an OPEN entry is cancelled after 60 s (config 194; 1074-1107), or earlier if price moves toward the target (1128-1192);
  - exit legs are exempt from both (1085).
- Fill: OrderFilled (only on COMPLETE) goes to the placer, which commits capital, records the fill, sets OPEN and places SL and TGT (order_placer.py 1767-1786; OP-NS2, 121-124).
- Partial fill:
  - an ENTRY order is cancelled at the first PARTIAL (order_monitor.py 910-940);
  - OrderPartiallyTerminated then makes the placer commit and protect the filled quantity (order_placer.py 1788-1830);
  - the rest is not chased.
  - KNOWN AMBIGUITY: the docstring at 891-892 says exit legs keep the timeout path, but 943-946 exempts SL, TGT and EOD. So the 5-minute PARTIAL timeout (config 739) reaches only a leg with no recognised label.
- Exit placement failing after an entry fill: emergency marketable-LIMIT flatten, then HARD_KILL (OP-NS5, order_placer.py 141-150). A target-only failure no longer kills (RECORD-DERIVED, forensics 22-Jul, lines 34-36).

## 8. Watching open positions (FACT, SOURCE-VERIFIED unless marked; G-015 owns reconciliation detail)
- Protection rests at the broker: the SL and TGT orders of section 7.
  - The smart-target trail and CO logic branch on "CO_PLUS_TGT" (order_placer.py 1881-1889, 2071-2079) and are not reached (D2).
  - RECORD-DERIVED: smart TGT never fired (B-006 TH22).
- The order monitor (2 s) turns broker fills into events. The reconciler poll (every 15 s, config 469; order_reconciler.py 660-667) runs one cycle at a time, skipping if one is running (633-642). Each cycle runs (816-1060):
  - CHECK1 manual close;
  - CHECK2 orphan;
  - CHECK4 partial close;
  - CHECK5 growth;
  - CHECK6 orphan orders;
  - G5b, a missing SL replaced;
  - duplicate-exit removal;
  - CHECK7 capital drift;
  - CHECK8 CO SL drift (alert only);
  - an unrealised MTM refresh;
  - in-flight entry recovery (4169 onward).
- TGT retry every 30 s (config 488; B-006 TH23).
- Ticks drive no exit (section 6).
- KNOWN DEFECT D3: the order monitor stops itself.
  - Separate counters for authentication failures and other API failures (226, 233-234), each reaching 3, call the HARD_KILL callback and set the monitor's own stop event (640-654, 663-689; config 741).
  - The counters are per monitor, not per order, and any successful order poll resets both (702-703).
  - It is started once (main.py 3960). No restart path was found in the 330 runtime files.
  - After it stops, the monitor no longer polls and no longer emits fill events through its own polling path, and the 15:15 entry cancellation no longer runs (section 3).
  - INTERPRETATION: no other emitter of OrderFilled was traced for B-009, so a resting SL or TGT that fills would then be seen through the reconciler's checks. Not established as complete.
  - Its liveness appears on /health (main.py 4067), which nothing in the trading path consumes (B-007 section 10, frozen).
  - An external supervisor on the host is outside this evidence.

## 9. What stopped entries, and what kept running (FACT, SOURCE-VERIFIED; I topics own kill semantics)
- is_active("entry") is true for SOFT_KILL and HARD_KILL (capital/kill_switch.py 510-525). Both the webhook and the processor check it (sections 5, 9).

| Trigger | Kill | Source |
|---|---|---|
| Orphan order | SOFT_KILL | main.py 820-843 |
| 15:15 entry cancellation ("force-close") | SOFT_KILL | main.py 688-724 |
| API failure count reaching api_failure_threshold 3 | SOFT_KILL | config 356-357; signal_processor.py records failures at 1122-1125, 1319-1335, 1478-1479, 1490-1491, 1533-1536 |
| Clock skew over 30 s | SOFT_KILL | config 186 |
| Daily loss | EOD fire_now, then SOFT_KILL | main.py 756-817 |
| Order monitor: 3 API or authentication failures | HARD_KILL, monitor stops | main.py 727-752 |
| Exits failing after an entry fill | HARD_KILL after a flatten | OP-NS5 |
| Per strategy | Strategy circuit breaker | signal_processor.py 984-990; config 732 onward |

- Entries stop while protection continues on four paths:
  - SOFT_KILL;
  - after 15:00 every day;
  - the degraded webhook start (main.py 4006-4054: "entries cannot arrive, exits/reconciler/EOD squareoff continue");
  - an unreachable webhook.
- The reverse, nothing running at all, is the 17-Jul outage (RECORD-DERIVED: main.py 4009-4020 comment; config/cron_registry.yaml 336-340).
- The alert precedes placement. "INTRADAY SIGNAL" is sent at 1370-1380, before the late kill, shutdown and throttle checks and before the broker call, so it is not proof of an order.

## 10. Failures during the session (FACT, SOURCE-VERIFIED)
| Failure | Path | Result |
|---|---|---|
| Quote fails at screening | secondary_screener.py 170-193 | SKIPPED_QUOTE_UNAVAILABLE; candidate dropped |
| Fresh quote fails (momentum) | signal_processor.py 1088-1106 | Continues on the stale trigger price |
| Broker timeout in place() | 1464-1484 | No retry; TIMEOUT / UNKNOWN_IN_FLIGHT; reconciler recovery owns it |
| Client rate limit in place() | 1486-1532 | Re-queued up to 3 times, then PLACEMENT_FAILED |
| Broker error in sizing, risk or reserve | 1122-1125, 1319-1322, 1332-1335 | REJECTED_*_BROKER_ERROR; counted toward SOFT_KILL |
| Order poll timeout | order_monitor.py 655-662 | That order skipped this cycle |
| Order poll error or auth failure x3 | 640-689 | HARD_KILL; monitor stops (D3) |
| Pipeline exception | signal_processor.py 405 onward, 1233-1246 | PLACEMENT_FAILED; the worker survives |

- Worker failure behaviour in general: B-006 section 7 (frozen).
- Restart and network loss: DEFER TO B-012.

## 11. Hand-off to end of day
- The MIS passes at 15:03 and 15:06, the 15:15 placement cut-off, the 15:15 entry cancellation with SOFT_KILL, and the 15:17 routine are distinct (section 3).
- The 15:17 routine first clears the entry gate and parked retest candidates (eod_squareoff.py 340-352); the rest is B-010's.
- FACT (RECORD-DERIVED, mis_autosquareoff.py 18-24): the MIS passes were added after a broker auto square-off of a closing-auction stock at 15:12:47 on 27-Aug-2026. Before 03-Sep the passes ran at 15:07 and 15:10 (37-43).

## 12. Paper and live (FACT, SOURCE-VERIFIED unless marked)
- Live only:
  - the feed;
  - the clock-skew probe;
  - token monitoring (B-006 TH13 to TH19).
- Paper only: stale paper orders are cancelled at start (main.py 3948-3951); quotes come from the paper provider with its own alias map and warning (section 6).
- FACT (RECORD-DERIVED, census 19-Jul, lines 429-430): all 361 trades between 12-Jun and 16-Jul were LIVE. No paper data exists in that window to compare.
- Parity on other dates: UNKNOWN / NOT ESTABLISHED.

## 13. Records (FACT, RECORD-DERIVED; each true for its own scope)
| Record | Scope | What it shows | What it does not show |
|---|---|---|---|
| docs/audit/signal_mortality_census_19jul2026.md | Signals 12-Jun to 16-Jul; complete era 09-Jul to 16-Jul (6 trading days); code cited at 7319d06 | See the census summary below this table | A rate for any other period; that REJECTED_* counts before 09-Jul survive (they were pruned, lines 43-53, 96) |
| docs/incident/2026-09-03_naked_position_ANANTRAJ.md | One trade, 03-Sep; deployed 2d08436 | Entry filled 10:01:33 with separate SL and TGT orders; at the 15:07 MIS pass both were cancelled and the exit was not submitted; G5b replaced the SL about 11.5 s later; the position was closed manually (lines 16-45) | Normal daily operation; the signal's arrival |
| docs/audit/orphan_adoption_22jul2026.md | June orphan events | Two broker positions with no local trade, on 19-Jun (unexplained) and 25-Jun (the system's own duplicate-exit oversell); CHECK2 does not adopt (lines 18-35) | Other dates |
| docs/audit/exit_rejection_hard_kill_forensics_22jul2026.md | The June exit rejections | Three broker rejection variants of SL and TGT (15, 16 and 19-Jun) led to HARD_KILL; all were fixed by 24-Jun; HARD_KILL on unprotected exits was a pre-live decision, later softened (lines 3-39) | Any rejection after 22-Jul |

- Census summary:
  - Funnel, 09-Jul to 16-Jul (lines 175-187):

    | Stage | Count |
    |---|---:|
    | Arrived | 165,111 |
    | Admitted | 30,769 |
    | Reached the risk engine | 5,845 |
    | Approved | 217 |
    | Ordered | 78 |
    | Filled | 48 |

  - 19.00% of admitted signals reached the risk engine; de-duplication explains the gap from arrivals (189-198).
  - Leading terminal statuses (lines 142-155): REJECTED_SCORE 10,949; REJECTED_STRATEGY_CONTROL 5,685; REJECTED_DAILY_TRADES 5,146; REJECTED_SIZING_CONCENTRATION 3,098; REJECTED_SHADOW_INNING_ACTIVE 2,801.
  - SKIPPED_QUOTE_UNAVAILABLE: 725 in the complete era and 1,876 in the pruned residue of 12-Jun to 08-Jul (91).
  - The scanner POSTed each scan about once a minute (192).
- Named in the tree, not read for B-009 (B-009_review_notes.md section 5): tick_candle_dormancy (read for B-006), s4_boot_outage, liveness_alarm, throttle_selection_and_record_correction, consecutive_losses_gate_wired, kill_drill, mo9_slippage_reference_hold, pending_reconciliation, mr4_block2_order_reconciliation, monday_post_session_results, orphan_adoption_forensics, pre15jun_large_orphans.

## 14. Source-to-consumer trace (source wiring at the baseline, not host execution)
| Component | Input | Logic | Guard | Output | Storage / hand-off | Next consumer |
|---|---|---|---|---|---|---|
| Webhook (webhook_receiver.py 549-1072) | POST: scanner, symbols, prices, time | Parse; per-symbol checks | Auth, kill, backpressure, window, 600 s, in-flight claim, 300 s dedup, fingerprint | QUEUED row; queue entry; HTTP answer | signals, webhook audit; queue | Dispatcher |
| Signal processor (signal_processor.py 867-1276) | Queue entry | Section 5 stages | Kill (twice), windows, age, control, breaker, caps, risk, throttle | Status per stage; place() | signals.status; reservation | OrderPlacer |
| Secondary screener (secondary_screener.py 170-297) | Signal, REST quote | Steps and score (D) | Quote present | PASSED / REJECTED_* / SKIPPED_* | signals.status | Processor |
| Entry gate (entry_gate.py; main.py 3839-3844, 3975) | No source path writes new rows; start() reloads any gate_state rows (156, 179-235) | 5 s LTP poll, release | Timeout, 3 quote failures | Release callback (main.py 870-880) | Gate state | continue_from_gate; not fed by new signals (D1) |
| OrderPlacer (order_placer.py 961-1097, 1767-1830) | Trade fields | LIMIT_TRIPLE entry; exits at fill | Last-mile kill, 15:15 cut-off, slippage, R:R | Trade PENDING to OPEN; SL and TGT | trades, orders; fill map | Order monitor, reconciler |
| Order monitor (order_monitor.py 594-1192) | Tracked orders | History poll every 2 s | 60 s, partial cancel, price move, 15:15, 3-failure stop | OrderFilled, OrderPartiallyTerminated, status events | Event bus | Placer, order manager, slippage recorder |
| Reconciler (order_reconciler.py 633-1060, 4169 onward) | Broker positions and orders; local trades | CHECK1-8, G5b, dedupe, MTM, recovery | One cycle at a time | Close, flatten, SL, alerts | trades; fund ledger | Operator alerts; kill switch |
| MIS square-off (mis_autosquareoff.py) | Open MIS positions | PASS 1, PASS 2 | 15:09 cutoff | Exit orders | orders | Order monitor; B-010 |
| EOD (eod_squareoff.py 326 onward) | Day state | Clear gate, cancel, exit, summary, reset | Once per day | Square-off actions, summary and reset; a flat book is the intent, the outcome is not established by this trace | DB | B-010 |
| Feed (data/live_feed.py) | Ticks, none subscribed at start | Candle build | Watchdog armed on first tick | Exit-retry LTP | candles (dormant) | Placer exit retry |

## 15. Known defects, ambiguities and negative findings
1. KNOWN DEFECT D1, the pullback gate is never fed.
   - Group A sets pullback_wait_enabled: true, but the processor only uses the flag to skip the fresh-quote re-anchor (signal_processor.py 1082). No runtime file adds a signal to the entry gate. The only add() reference is the gate's own docstring (entry_gate.py 103); its other constructor call is store rehydration (209).
   - The gate is still constructed and polled. At start it reloads gate_state rows (entry_gate.py 156, 179-235). The only writer of those rows is add() (332), which has no caller, and the 15:17 routine clears them, in memory and in the store (eod_squareoff.py 340-347; entry_gate.py 260-279). So no source path at the baseline gives the gate new entries. What a host's gate_state held, for example rows left by an earlier version, is UNKNOWN.
   - So, on the source path, a Group A signal reaches no pullback wait. Its entry is derived as for every strategy (signal_processor.py 1028-1030), with the webhook trigger as the reference and without the fresh-quote re-anchor. The submitted price is that derived price, not the raw trigger.
   - The comment at signal_processor.py 1074-1075 ("EntryGate already waits") and the gate's header (24, 30) disagree with the wiring.
   - Search scope: all 330 Python files outside tests/.
   - The realised effect on any trade is not established.
2. KNOWN DEFECT D2, the declared order protocol is not honoured (section 7).
   - Every order on the traced path is LIMIT_TRIPLE.
   - Groups A and B declare CO_PLUS_TGT and pb01_breakout_retest does too; Group C declares LIMIT_TRIPLE and so is unaffected.
   - Consistent with the records: separate SL and TGT orders on 03-Sep; smart TGT never fired.
   - Which protocol each historical order used is established only where a record shows the order.
3. KNOWN DEFECT D3, the order monitor stops itself after 3 failures, with no restart path in source (section 8).
4. Partial-timeout docstring against code (section 7).
5. Window comments against strategy files (section 4).
6. The tick/candle path is dormant; decisions use REST quotes (section 6).
7. The signal alert precedes placement (section 9).
8. Strategy counts: 16 files, 15 eligible by configuration, 15 rows in the 24-Sep briefing. These are three different things (section 4).
- Negative searches over all 330 runtime files found none of the following:
  - a session flag (section 3);
  - a caller of EntryGate.add();
  - a reader of a strategy's order_protocol on the placement path;
  - a second start of the order monitor;
  - any live_feed.subscribe caller besides order_placer.py 3404.

## 16. Answer-sheet status (all 38 IDs, B9-A1 to B9-H4; detail in Claude's repository answers, as corrected by the addendum)
| Status | IDs |
|---|---|
| ANSWERED, SOURCE-VERIFIED | A1, A2, A4, B3, C1, C3, D2, E1 (source part), E2, E4, F1 |
| ANSWERED, SOURCE and RECORD | B2, B4, B5, D4, D6, E3, E5, F2, F5, F6, G1 |
| PARTLY ANSWERED | A3 (declared jobs; installed crontab unknown), B1 (scanner named in comments and the census; usage period unknown), C2, C4, D5, F3, G3, G4, H1, H4 |
| UNKNOWN / NOT ESTABLISHED, OPERATOR-ONLY | D1 (what was seen), D3 (manual checks), G2 (what was used), H3 |
| ANSWERED (provenance) | H2 |
| DEFER | F4 to B-012 |

## 17. Cross-references
- B-005 startup reconciliation (frozen, DR-041).
- B-006 worker start-up, feed and worker failures (frozen, DR-043).
- B-007 readiness and /health consumers (frozen, DR-044).
- B-008 market open (frozen, DR-045).
- B-010 end of day; B-012 restart.
- A-007 product types.
- C: webhook and signal detail.
- D: screening and decision formulas.
- E: market data.
- F: sizing and capital.
- G: orders, positions, reconciliation (G-015).
- I: kill semantics.
- L: alerts, logs, evidence.
- M: cron and deployment.

## 18. Final B-009 statement
- The legacy session had no session state. Clock gates decided eligibility at the webhook, the processor and the placer.
- Signals came in as webhook POSTs, were de-duplicated and queued, and went through a fixed pipeline. Each stage wrote a status.
- Decisions used REST quotes. The tick feed connected in live mode but subscribed to nothing at start; it was recorded dormant (25-Jul).
- On the traced placement path at the baseline, OrderPlacer used LIMIT_TRIPLE regardless of the strategy's declared order_protocol. Protection rested at the broker, watched by a 2-second order poller and a 15-second reconciler.
- Entries stopped at 15:00, at a kill or at the 15:15 cut-off, while protection continued until the MIS passes and the 15:17 routine.
- Three source defects stand out:
  - the pullback gate that nothing feeds;
  - the ignored order protocol;
  - an order poller that stops itself.
- What actually ran on any date is known only where a record says so.

## 19. Revision 1 (2026-10-20)
ChatGPT's value-added review of 20-Oct-2026 (B-009_VALUE_ADDED_REVIEW_TO_CLAUDE_20-OCT-2026.txt) was applied. Its seven items A to G and where each was applied:

| Item | Change | Sections |
|---|---|---|
| A | D1 price wording; the gate's reload versus "empty" resolved from source | 5, 14, 15 |
| B | D3 limited to the monitor's own polling and event path | 8 |
| C | LIMIT_TRIPLE scoped to the traced path at the baseline | 18 |
| D | The 15:15 entry cancellation named by its effect; "force-close" kept as the historical source name | 3, 9, 11 |
| E, F, G | Re-read for source, record and runtime boundaries, the corrected counts, and feed wording | 18 |

No other change.

## 20. Revision 2 and freeze (2026-10-20)
ChatGPT's revision-2 corrections of 20-Oct-2026 (B-009_REVISION_2_REQUIRED_CORRECTIONS_FOR_CLAUDE_20-OCT-2026.txt) were applied:
- Correction 1: the answer sheet has 38 IDs, not 44. Section 16 now says 38. Each ID appears exactly once in its table (B-009_review_notes.md section 10).
- Correction 2: the gap_fade_long conflict is kept visible (section 4 and the Group B table).
  - Partly accepted. The review calls the 11:30 "source logic". It is a docstring and a comment only; the code reads the YAML's 15:00.
  - The manual states what the code reads and leaves the intended value unresolved.
- Wording caution: the EOD output in section 14 is stated as intent, not outcome.
- Rama approved the final files on 20-Oct-2026. B-009 is FROZEN (DR-046).

