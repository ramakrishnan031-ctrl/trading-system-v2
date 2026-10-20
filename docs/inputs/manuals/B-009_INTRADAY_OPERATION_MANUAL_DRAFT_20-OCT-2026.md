# B-009 — Intraday Operation

> **Status:** Draft for independent review — not frozen, not committed, not pushed  
> **Evidence baseline:** `trading-system-prod-snapshot` at `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
> **Review basis:** Claude’s repository-backed answer file dated 20-Oct-2026, with cross-references to frozen B-005–B-008 and selected dated records. All source references must be rechecked at the pinned baseline before freeze.  
> **Scope rule:** The legacy system is DEAD / REFERENCE ONLY. This document records historical source-path behaviour; it does not repair, revive, operate, or redesign the system.

## 1. Purpose and scope

B-009 documents the legacy system’s normal intraday lifecycle after market-open handling (B-008) and before the detailed end-of-day workflow (B-010). It covers signal arrival and processing, market-data use, entry and order handling, position monitoring, intraday controls, degradation, paper/live code differences, and evidence available in the inspected source and selected records.

It does **not** independently establish what was installed or running on the production VM on every historical date. Repository declarations and thread wiring are not proof of host execution. No VM, broker, database, or service was contacted for this review, and no legacy code was run.

### 1.1 Evidence labels

| Label | Meaning |
|---|---|
| FACT (SOURCE-VERIFIED) | Supported by the cited source at the stated repository ref, subject to final line-level recheck. |
| FACT (RECORD-DERIVED) | Reported by a dated record; applies only to that record’s scope/date/commit. |
| HISTORICAL (Rama-reported) | Operator recollection explicitly supplied by Rama. None is introduced by this draft without a supporting statement. |
| INTERPRETATION / AUDIT LEAD | A conclusion drawn from source or records; limits must be stated. |
| KNOWN DEFECT | Source-path inconsistency or defect supported by code evidence; not a measured production outcome. |
| UNKNOWN / NOT ESTABLISHED | Available evidence does not settle the point. |
| DEFER TO B-XXX | Detailed treatment belongs to another agenda/manual. |

### 1.2 Repository boundary

The primary source inspected by Claude was `trading-system-prod-snapshot` at commit `970aabf3c78d777d9d34549e021f7c8e756c7bdb`. The inspection was selected-path, not a complete-tree audit. The sandbox and records repositories were not inspected for this pass, following the later instruction to use the production repository as primary. Selected dated records inside the production repository have their own record/deployment commit contexts and must not be treated as if created at the source baseline.

## 2. Intraday session model

### 2.1 No explicit session-transition flag

**FACT (SOURCE-VERIFIED, pending final line recheck):** The inspected baseline does not establish a single `INTRADAY_ACTIVE` session flag or market-open event that flips the system from pre-open to intraday. Eligibility is evaluated dynamically at multiple points:
- on each webhook request;
- while each queued signal is processed;
- before the order placer’s broker call, including the later order-entry cutoff.

The market-window predicate cited in the research file is `entry_start <= now < entry_end`, subject to the trading-day/holiday check. The source configuration sets the global entry floor to 10:00 and the global entry end to 15:00. B-008 DR-045 owns the market-open interpretation and the pre-10:00 webhook rejection.

This means the source path is a series of clock-based gates, not evidence of a separate “session active” database toggle. Actual host behaviour on any given date remains UNKNOWN / NOT ESTABLISHED without dated runtime evidence.

### 2.2 Distinct time controls

| Time (IST) | Control described by inspected source | Scope / limitation |
|---|---|---|
| 10:00 | Global entry floor | Overrides earlier strategy start windows where they overlap. B-008 owns the market-open evidence. |
| 15:00 | Global entry end | Entry eligibility closes; the configured end is exclusive. |
| 15:03 | MIS square-off PASS 1 | First protective pass, described in source as limit attempt then market path. |
| 15:06 | MIS square-off PASS 2 | Follow-up market pass. |
| 15:09 | MIS protective cutoff | Deadline associated with the two MIS passes. |
| 15:15 | Order-monitor entry cancellation / SOFT_KILL and placement cutoff | Distinct from the MIS passes; source says it cancels tracked entry orders and blocks entries, not that it itself closes open positions. |
| 15:17 | EOD routine / backstop | Boundary into B-010; detailed accounting and square-off behaviour belongs to B-010. |

These are source/configuration times, not proof that the controls ran on a specific historical date. Verify the exact loop wiring and time checks at the pinned baseline before freezing.

## 3. Signal ingress and lifecycle

### 3.1 High-level flow

```text
Chartink-labelled HTTP webhook ingress
        |
        v
Authentication / kill-switch / backpressure / entry-window guards
        |
        +-- rejected request (e.g., 403 or 503) --> response + applicable audit evidence
        |
        v
Payload parsing and per-symbol validation
        |
        +-- invalid / expired / in-flight / duplicate / queue-full --> response/status
        |
        v
signals row: QUEUED  --> in-memory signal queue
        |
        v
signal processor: PROCESSING
        |
        v
kill + global window + expiry + shadow guard + strategy mapping/control
        |
        v
per-strategy window + circuit breaker
        |
        v
secondary screening / quote-dependent checks
        |
        v
entry derivation --> sizing --> caps / duplicate-direction guard
        |
        v
risk approval + capital reservation
        |
        v
target derivation + pre-placement alert
        |
        v
late kill / shutdown / throttle guards
        |
        v
OrderPlacer.place()
        |
        +-- success --> signal PROCESSED; trade/order lifecycle continues
        +-- timeout --> signal TIMEOUT; trade UNKNOWN_IN_FLIGHT; reconciler recovery
        +-- failure/reject --> terminal or retry status; reservation handling
```

**Evidence boundary:** This is a source-path diagram reconstructed from Claude’s answer file. Recheck the source functions/line ranges before freeze. It is not a trace of one actual production signal.

### 3.2 Webhook boundary and initial guards

Claude’s source trace identifies one HTTP POST webhook ingress keyed by scanner name. The inspected path applies authentication, kill-switch, queue-backpressure and global entry-window checks before payload parsing. Unknown scanner names can be rejected. EOD-type scanners are described as going to a separate handler.

The B-008 finding is especially important for the early-session boundary: the inspected intraday webhook rejects a pre-10:00 request with HTTP 403 before parsing. The request is not stored as a signal or queued, but the webhook audit row remains. This finding is limited to the inspected path; alternate ingress or resume routes must not be assumed identical.

### 3.3 Per-symbol validation, de-duplication and queueing

The source trace reports the following checks in `signals/webhook_receiver.py`:
- symbol/price validity;
- expiry using configured `signal_queue.expiry_sec` of 600 seconds;
- in-flight claim for an already-processing symbol;
- recent `(symbol, scanner)` duplicate detection;
- fingerprint-row duplicate detection;
- creation of a `signals` row with `QUEUED` status;
- queue insertion, with `QUEUE_FULL` handling.

The research file distinguishes webhook responses from signal rows: statuses returned before the insert are not themselves stored as signal rows. Confirm exact transaction/order-of-operations at the pinned baseline, especially the interaction between queue-full rows and fingerprint retries.

### 3.4 Processor stages and terminal states

The reported processor sequence is:
1. Mark `PROCESSING`.
2. Check kill state, global entry window, signal age, shadow guard, strategy mapping/control, per-strategy window and strategy circuit breaker.
3. Run secondary screening.
4. Derive entry/stop-loss fields.
5. Size the position.
6. Apply per-strategy position cap and same-symbol/direction protection.
7. Obtain risk approval and reserve capital.
8. Derive target and send the “INTRADAY SIGNAL” alert.
9. Recheck late kill, shutdown and entry throttle.
10. Call `OrderPlacer.place()`.

The reported outcomes include `REJECTED_*`, `SKIPPED_*`, `RESERVED`, `PROCESSED`, `TIMEOUT`, `UNKNOWN_IN_FLIGHT` and `PLACEMENT_FAILED`. The source trace states that rejected paths release the reservation. These status transitions and release paths require final line-level verification. Detailed scoring, screening formulas, sizing, S&R, R:R and capital calculations belong to their owning C/D/E/F/G manuals and should not be redefined here.

### 3.5 Expiry and window handling

The repository answer reports an effective 600-second signal expiry, with a 60-second constructor default overridden during application setup. This must be verified at the pinned baseline and described as:
- configured effective value;
- constructor default;
- exact override/caller.

Do not confuse the 600-second signal age limit with the order monitor’s separate 60-second unfilled-entry timeout.

Outside-window behaviour reported by the source trace:
- a request arriving outside the global window is rejected at webhook ingress;
- a queued signal whose processing time has moved outside the window is rejected rather than parked for later;
- a per-strategy window can reject a signal;
- the placer refuses an order after its separate 15:15 cutoff.

No source path was identified that parks ordinary signals until a later window. The `WAIT_FOR_RETEST` diverter was reported disabled; this is a source configuration finding, not a claim about a V2 design.

## 4. Strategy windows and strategy registry

The research file reports that 15 intraday strategies are enabled in the inspected path, while the baseline inventory contains 16 strategy files. B-008 separately recorded that the 24-Sep briefing displayed 15 rows and omitted `pb01_breakout_retest`; the briefing discrepancy does not establish which strategies were enabled or eligible on that date.

**Required wording:** distinguish:
- 16 strategy files in the selected baseline inventory;
- the 15 strategies the B-009 research identifies as enabled for ordinary intraday entries;
- any disabled strategy and its separate observer/capture workers;
- the 15 rows in the dated briefing.

Do not state that 16 strategies were enabled. Verify loader, mapping and enabled flags before freezing this count.

The source trace says both global and strategy-specific windows apply and the narrower window wins. It also reports documentation drift: comments for `gap_fade_long` describe an 11:30 cutoff while the strategy file states 15:00, and a docstring default differs from the file values. Record this as **KNOWN DOC DRIFT** only after rechecking the cited config and comments.

## 5. Market data and price usage

### 5.1 REST quotes versus WebSocket ticks

Claude’s source trace reports:
- the secondary screener uses a `quote_fn` for REST quote retrieval;
- momentum strategies may re-fetch a quote and re-anchor entry;
- the live quote provider routes to the broker;
- the WebSocket feed connects in live mode but subscribes to no symbols at startup;
- the only subscriber identified is an exit-retry path;
- consequently, tick-built 60-second candles do not feed the ordinary entry decision path at the baseline.

This distinction matters: a connection attempt is not evidence that ticks are arriving, and a live feed object is not proof that candles reach the decision logic. Frozen B-006/B-007 records describe the tick/candle path as dormant in the evidence they cover; that does not prove every date behaved identically.

Historical candle retrieval and shadow observers are described as separate from the intraday entry decision path. Their detailed data-source, storage and calculation behaviour belongs to E topics.

### 5.2 Quote failure behaviour

The source trace reports:
- a secondary-screen quote failure becomes `SKIPPED_QUOTE_UNAVAILABLE`, and the candidate is dropped without retry at that point;
- a failed momentum re-anchor logs a warning and continues using the stale webhook price;
- the inspected path does not stop a worker solely because the screening quote failed.

The research file also cites a census record reporting missing-quote counts and differences in live/paper logging. Those are record-derived, period-specific observations, not a timeless failure rate. Do not generalise “no missing-quote warning” beyond the exact branch and evidence checked.

### 5.3 Price stages

The answer file identifies four distinct prices:
1. `trigger_price` supplied by the webhook;
2. derived `entry_price`, potentially re-derived from a fresh quote for momentum strategies;
3. submitted LIMIT order price;
4. broker fill price, used for slippage evidence.

The source trace reports a slippage guard against the signal trigger and an acceptance evidence record containing trigger, final entry, re-anchor flag, stop-loss, target and quantity. Dated examples were not established in the records read. Exact formulas and price transformations belong to G/L or other designated detailed agendas.

## 6. Entry orders and order monitoring

### 6.1 Pending order lifecycle

The source trace reports a trade is marked `PENDING` before the broker placement call and tracked by the order monitor. The monitor polls broker order history every two seconds. An open entry order is cancelled after 60 seconds and may be cancelled earlier if price moves toward the target before fill. Exit legs are reported exempt from the entry timeout. At 15:15, tracked entry orders are cancelled and SOFT_KILL fires.

A placement timeout produces `TIMEOUT` for the signal and `UNKNOWN_IN_FLIGHT` for the trade, with the reconciler responsible for recovery. These are source-path states; whether any particular timeout occurred on a date requires records.

### 6.2 Partial fills

The answer file reports an entry order is cancelled on its first partial fill. The filled quantity is then used to commit capital, mark the trade open, and place SL and target legs for the filled quantity; the remaining quantity is cancelled rather than chased. Failure to place exit legs can trigger kill/flatten paths.

It also identifies a mismatch between an order-monitor docstring and the actual partial-timeout branch. Preserve this as a code/documentation ambiguity until the exact branch is rechecked. Actual partial-fill events remain UNKNOWN / NOT ESTABLISHED unless dated records prove them.

### 6.3 KNOWN DEFECT D1 — pullback EntryGate is never fed (reported source finding)

The repository answer says six strategy configurations enable pullback waiting, and `EntryGate` is constructed and polled, but no runtime caller of `EntryGate.add()` was found outside the gate module. The processor uses `pullback_wait_enabled` to skip a fresh-quote re-anchor, but does not appear to enqueue the signal into the gate.

If final source verification confirms this, the consequence is that the configured pullback-wait path does not perform its declared wait and may place based on the webhook-trigger basis. This is a **source-path defect finding**, not evidence that a particular historical trade entered at a particular price. State the negative-search scope (reported as 325 runtime Python files outside tests) and check all alternate callers before freezing.

### 6.4 KNOWN DEFECT D2 — strategy order protocol appears ignored (reported source finding)

The repository answer says strategy configuration declares `CO_PLUS_TGT` for ordinary intraday strategies, but the constructed `OrderPlacer` uses its default `LIMIT_TRIPLE` and the traced placement path does not read the per-strategy `order_protocol`. It reports separate SL and target orders at fill, with smart-target trail and CO-specific logic not reached through that path.

Before freezing, verify:
- strategy loader and protocol field propagation;
- all `OrderPlacer` constructors and `place()` callers;
- all reads of `order_protocol`;
- exact branches for LIMIT_TRIPLE and CO_PLUS_TGT;
- whether records support the actual historical protocol used.

Until that trace is complete, label the conclusion **KNOWN DEFECT (source path)** or **INTERPRETATION / AUDIT LEAD**, not a claim about every historical order.

### 6.5 Order monitor self-stop (reported source finding)

The answer file says three consecutive authentication or API failures trigger HARD_KILL and set the monitor’s stop event, with no restart path found in inspected source. If so, the monitor ceases polling for the rest of that process life. Resting broker SL/target orders may still execute at the broker, but the local process would not receive fills through that stopped poller; the answer file says reconciliation can later detect some effects.

Verify counter reset semantics, thread ownership, restart/supervisor path and downstream recovery. “No restart path found in inspected source” does not prove an external host supervisor does not exist. The monitor’s health visibility and lack of a trading-path consumer are cross-references to frozen B-007, not a reason to reopen B-007.

## 7. Position monitoring and reconciliation

### 7.1 Source-wired monitoring

The answer file describes:
- broker-resting SL and target orders placed at entry fill;
- order monitor polling every two seconds and emitting fill events;
- reconciler polling every 15 seconds, comparing broker positions/orders with local trades;
- reconciliation checks for manual closes, orphan positions/orders, partial closes, position growth, duplicate exits and capital drift;
- G5b missing-stop recovery;
- target retry every 30 seconds;
- unrealised MTM refresh.

These are source-wired responsibilities. They do not prove successful operation every 15/30 seconds on the production host. B-005 startup reconciliation and B-006 worker initialization remain frozen references. Detailed reconciliation semantics belong to their owning agenda; this manual describes the intraday hand-off only.

### 7.2 Reconciler evidence and dated incidents

The research file cites:
- a 03-Sep incident record describing an ANANTRAJ position with both protective orders cancelled, an MIS pass that did not submit the exit, G5b restoring an SL 11.5 seconds later, and a final manual close;
- a 22-Jul orphan-adoption record describing broker positions without local trades on two dates, including one attributed by the record to duplicate-exit oversell and one described as unexplained.

These are **FACT (RECORD-DERIVED)** only for the incidents as documented. The records cite deployed commits other than the source baseline. They do not establish the normal path on every day or resolve all historical cases.

### 7.3 Intraday exit paths

The source answer identifies:
- broker SL fill;
- broker target fill;
- MIS PASS 1 at 15:03;
- MIS PASS 2 at 15:06;
- 15:17 EOD backstop;
- daily-loss-triggered close/kill path;
- reconciler flatten/recovery paths;
- HARD_KILL flatten;
- manual close detected by reconciliation.

The 15:15 force-close is a distinct control described as cancelling tracked entry orders and firing SOFT_KILL; it should not be described as the control that closes open positions. B-010 owns the detailed EOD routine.

## 8. Kill switches, time cut-offs and degradation

### 8.1 Entry-only blocking versus position management

The answer file reports SOFT_KILL blocks new entries while leaving exit management active. Reported sources include orphan-order handling, 15:15 force-close, API-failure threshold, clock skew and daily loss. HARD_KILL sources include order-monitor failures and failed exit placement after entry fill. Strategy circuit breakers can block a particular strategy. The degraded webhook startup is described as preventing new entries while exits, reconciliation and EOD square-off continue.

These are source behaviours. Actual activation dates and whether each path was observed require dated evidence. Cross-reference frozen B-007 for readiness/health semantics; do not claim that `/health` alone blocked trades unless a consumer is proven.

### 8.2 Cut-off sequence

The manual must keep four separate controls distinct:
- global entry end at 15:00;
- MIS passes at 15:03 and 15:06 with a 15:09 protective cutoff;
- 15:15 placement cut-off and order-monitor entry cancellation/SOFT_KILL;
- 15:17 EOD accounting/backstop.

Do not merge the MIS protective cutoff, SOFT_KILL, and EOD backstop into one “square-off” event. Verify source loops/callers and use configuration declarations only as configured values.

## 9. Errors, retries and recovery

### 9.1 Broker/API failure paths

The repository answer reports:
- an order-monitor timeout skips the current cycle;
- repeated API/authentication failures reach a threshold, trigger HARD_KILL and stop the monitor;
- placement timeout produces `UNKNOWN_IN_FLIGHT` without immediate retry;
- a client-side rate limit requeues up to three times;
- other placement failures result in `PLACEMENT_FAILED`;
- some sizing/risk/reservation broker errors are translated to `REJECTED_*_BROKER_ERROR`.

Verify whether counters are shared or separate, what resets them, and which exceptions follow each branch. Do not invent retry outcomes or assume all broker failures are equivalent.

### 9.2 Worker failures

The answer file identifies:
- order monitor self-stop;
- pullback gate not receiving entries;
- a target-retry worker described in source comments as dead or crash-looping for days;
- smart target never firing in B-006 records.

Separate code-level defect findings from historical incident statements. A source comment about an earlier post-mortem is a record pointer, not proof that the same state persisted at the baseline or on every later date.

### 9.3 Restart and network loss

Detailed restart/network-loss behaviour is deferred to B-012. The source pointers include order-monitor/fill-map rehydration, startup reconciliation and EOD restart recovery. Do not repeat B-005/B-006 internals here; record only the intraday dependency and cross-reference.

## 10. Paper/live differences

The answer file reports these source differences:
- feed, clock-skew probe and token monitor are live-only;
- paper mode cancels stale paper orders at startup;
- paper quotes use a separate alias map and rate limit;
- live quote retrieval uses the broker provider.

A cited census record says all 361 trades in its 12-Jun to 16-Jul window were LIVE and zero were paper. This is a period-specific record-derived fact, not evidence of paper/live parity or of behaviour outside that window.

**UNKNOWN / NOT ESTABLISHED:** whether paper and live behaviour was otherwise equivalent, and what exact branches ran on other dates. Do not infer parity merely because the same function name is called.

## 11. Reports, alerts and operational evidence

The answer file identifies source evidence surfaces:
- Telegram signal alerts;
- email path for critical sentinels;
- `/health` on port 8080;
- logs and an operations dashboard;
- `signals`, `trades`, and `webhook_audit` tables;
- selected `docs/audit` and `docs/incident` records.

The processor sends the “INTRADAY SIGNAL” alert before the final placement call. Consequently, a notification may precede a later kill, shutdown, throttle, cutoff or broker failure. It must not be interpreted as proof of a broker order or fill.

The research file also cites a record that some CHECK1 alerts labelled a close as manual even though system orders had filled, and a 17-Jul outage record with no alert. These are record-derived findings for those dates. What Rama actually watched or retained operationally remains UNKNOWN / NOT ESTABLISHED.

## 12. Historical metrics and incident evidence

The 19-Jul signal-mortality census is reported to contain 30,769 signal rows across its documented era. Its leading terminal statuses include:
- `REJECTED_SCORE`: 10,949 (35.58%);
- `REJECTED_STRATEGY_CONTROL`: 5,685 (18.48%);
- `REJECTED_DAILY_TRADES`: 5,146 (16.72%);
- `REJECTED_SIZING_CONCENTRATION`: 3,098 (10.07%);
- `REJECTED_SHADOW_INNING_ACTIVE`: 2,801 (9.10%);
- `REJECTED_STRATEGY_CIRCUIT_BREAKER`: 898 (2.92%);
- `SKIPPED_QUOTE_UNAVAILABLE`: 725 (2.36%).

The same record is reported to state 19% of admitted signals reached the risk engine and 81% of raw arrivals in six complete days were intentional deduplication. These figures are **RECORD-DERIVED**, scoped to the census methodology, retention window and its own deployed commit. They are not timeless strategy-quality or execution-quality estimates. The census reports that EOD cleanup pruned `REJECTED_*` rows, so counts surviving in the database may not represent the full era.

A complete signal-arrival-to-exit trace was not established in the records read. The 03-Sep ANANTRAJ incident begins at the entry fill, not at the original signal arrival.

## 13. Source-to-consumer trace matrix

| Component | Source / input | Logic and guard | Output | Storage / hand-off | Next consumer |
|---|---|---|---|---|---|
| Webhook ingress | HTTP POST with scanner, symbol and price fields | Authentication, kill, backpressure, global window, parsing, validity, expiry, in-flight and duplicate checks | HTTP response; queued signal where accepted | `webhook_audit`, `signals`, queue (confirm exact transaction ordering) | Signal dispatcher |
| Signal processor | Queue tuple and strategy mapping | Window, age, kill, strategy control/circuit breaker, screening, price, sizing, caps, risk, reserve, late guards | Reject/skip/reserve/placement outcome | `signals.status`, capital reservation, evidence record | OrderPlacer |
| Secondary screener | Signal plus quote function | Screening steps and score; quote availability | `PASSED`, `REJECTED_*`, `SKIPPED_*` | Signal status | Signal processor |
| EntryGate | Configured pullback candidates (expected) | Poll/release logic; source answer says no runtime add caller found | No normal queued candidates if defect confirmed | Watchlist/state (verify) | Processor/release callback |
| OrderPlacer | Trade/entry fields | Last-mile guards, selected protocol, entry submission, SL/target placement after fill | Pending/open/exit order lifecycle | Trades/orders/fill map/capital ledger | Order monitor/reconciler |
| Order monitor | Tracked broker orders | Poll history, fill/partial handling, timeout, price-move cancellation, 15:15 cancellation, failure threshold | Fill events and order/trade updates; possible self-stop | Event bus / order state | OrderPlacer, manager, evidence path |
| Reconciler | Broker positions/orders and local state | Periodic checks, orphan/partial/growth/drift handling, recovery | Adopt/close/flatten/SL repair/alerts | Trades and capital ledger | Operator alerts / kill switch |
| MIS square-off | Open MIS positions | PASS 1, PASS 2 and cutoff | Exit orders / cancellation paths | Orders | Order monitor and B-010 boundary |
| EOD routine | Intraday state | Cancel/exit/reset/accounting flow | Flat-book attempt and summary | Database / next-day state | B-010 |
| Live feed | WebSocket ticks (subscription status requires verification) | Candle build/watchdog/exit-retry LTP | Tick/candle path; answer file says entry decisions do not consume ticks | Candle storage (reported dormant) | Candle timer / exit retry |

All rows are source-path summaries and require final citation check before freeze. The “EntryGate” row intentionally labels the expected input separately from the reported observed wiring.

## 14. Known defects, ambiguities and negative findings

The following are the principal issues reported by Claude and must remain explicit, with source evidence rechecked:

1. **Pullback EntryGate not fed:** gate is started/polled but no `EntryGate.add()` caller was found in the reported search scope; pullback-enabled strategies may not wait as configured.
2. **Strategy order protocol ignored:** strategy metadata says `CO_PLUS_TGT` for intraday strategies, but the traced placer appears to use default `LIMIT_TRIPLE`.
3. **Order monitor self-stop:** after three consecutive qualifying API/authentication failures, monitor stops itself; no in-source restart path was found.
4. **Partial-fill timeout doc drift:** docstring and code differ on which exit legs are subject to the partial timeout.
5. **Strategy-window documentation drift:** `gap_fade_long` comments reportedly cite 11:30 while its YAML states 15:00; a docstring default also differs from actual strategy files.
6. **WebSocket/candle dormancy:** source/records report no startup symbol subscriptions and no entry-decision tick consumer.
7. **Signal alert precedes placement:** alert is not evidence of an accepted broker order.
8. **Strategy-count ambiguity:** 16 files vs 15 enabled/briefing rows; do not conflate inventory, enabled state and report rows.

Negative findings must state their search scope. “No caller found in 325 runtime Python files outside tests” is not equivalent to proof that no external supervisor, host job or alternate repository path exists.

## 15. Open questions and evidence gaps

The repository answer leaves these points unknown or partially answered:
- actual installed crontab and which jobs ran on a given date;
- which webhook senders/operators actually used and when;
- what Rama saw/remembered during specific intraday sessions;
- historical pending orders, partial fills and exact signal-to-exit traces;
- whether the order monitor stopped on any given date and whether an external supervisor restarted it;
- paper/live behavioural parity;
- actual health of feed, workers, alerts and order paths on specific dates;
- full records from listed but unread incident/audit files;
- full-tree and alternate-repository findings not covered by the selected-path review.

Do not turn these into questions for Rama if source or records can answer them. Conversely, do not invent an answer where evidence is absent. Rama may later supply an operator recollection, which must be labelled as such.

## 16. End-to-end illustrative walkthrough

The following is a **source-path walkthrough**, not a claim that this exact sequence occurred on a historical date.

1. A scanner-labelled HTTP POST arrives at the webhook.
2. Authentication, kill-switch, queue-backpressure and global entry-window checks run. Before the configured 10:00 floor, the inspected intraday path rejects the request before parsing, as documented in frozen B-008.
3. An eligible request is parsed and each symbol is validated. Expiry, in-flight and duplicate checks run before a queued signal row and queue hand-off.
4. A worker consumes the queued signal and marks it `PROCESSING`. It rechecks kill, global window, age, shadow guard, strategy mapping/control, per-strategy window and circuit breaker.
5. The secondary screener requests a quote and applies configured filters. A quote failure is reported to skip/drop the candidate in the traced branch; it is not evidence of an automatic retry.
6. The processor derives entry/stop fields, sizes the trade, checks caps and duplicate direction, obtains risk approval and reserves capital.
7. The target is derived and an alert is sent before the final placement checks. A late kill, shutdown, throttle, cutoff or broker failure can still prevent a successful order after the alert.
8. The placer submits the entry order. A timeout can leave the trade `UNKNOWN_IN_FLIGHT`; the reconciler has a recovery path.
9. The order monitor polls broker order history. On fill, the source path updates the trade and protective legs; partial-fill and exit-placement failure branches have their own cancellation/kill handling.
10. The reconciler periodically compares broker and local state and can repair selected orphan/missing-stop conditions.
11. The distinct MIS passes, 15:15 entry cancellation/SOFT_KILL and 15:17 EOD routine form separate later controls. Detailed EOD logic is deferred to B-010.

The exact code path, transaction ordering, exception branch and resulting records for any real trade must be proven by a dated record; this example does not establish an actual production execution.

## 17. Cross-reference index

| Topic | Owning reference |
|---|---|
| Market-open handling and pre-10:00 rejection | B-008 (frozen DR-045) |
| Startup reconciliation | B-005 (frozen) |
| Worker initialization, feed/candle evidence, worker failure semantics | B-006 (frozen) |
| Readiness/health gating and health consumers | B-007 (frozen) |
| Detailed signal/strategy filtering and decision formulas | C-series agenda |
| Order processing and position/order lifecycle details | D-series agenda |
| S&R, entry price and technical calculations | E-series agenda |
| Risk, sizing, capital reservation and limits | F-series agenda |
| R:R and order execution quality/slippage | G-series agenda |
| Reporting, logs, evidence and performance measurements | L-series agenda |
| Detailed EOD square-off/accounting | B-010 |
| Restart/network-loss and recovery | B-012 |

## 18. Final source verification checklist

Before this manual can be accepted for freeze:
- [ ] Verify all cited source lines at `970aabf3c78d777d9d34549e021f7c8e756c7bdb`.
- [ ] Verify strategy count, enabled state and PB-01 treatment through loader/config/runtime mapping.
- [ ] Recheck all `EntryGate.add()` references and every `pullback_wait_enabled` branch.
- [ ] Recheck all `order_protocol` reads, constructor arguments and `OrderPlacer.place()` callers.
- [ ] Recheck order-monitor error counters, stop event and any in-source restart/supervisor path.
- [ ] Recheck the effective 600-second expiry assignment and distinguish it from the 60-second order timeout.
- [ ] Recheck REST quote provider wiring and WebSocket subscription/consumer path.
- [ ] Verify each dated record’s own commit, date/window and exact supported claim.
- [ ] Confirm all operator-only/host-runtime claims remain UNKNOWN / NOT ESTABLISHED.
- [ ] Run Markdown, cross-reference, hash and sensitive-data checks.
- [ ] Preserve source answer sheet and original research input byte-identically.
- [ ] No legacy code executed; no V2 decisions; no freeze/commit/push without Rama’s explicit approval.

---

**End of B-009 draft.** This manual records the system as described by the cited source and records; it is not a repair plan or a V2 specification.
