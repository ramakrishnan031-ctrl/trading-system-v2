# B-010 END-OF-DAY OPERATION: LEGACY MANUAL

Status: FROZEN 2026-10-20 (revision 1, DR-047). ChatGPT's review response of 20-Oct-2026 found no blocking issue; its one wording polish to erratum B-009 E-1 is applied (section 21). Rama approved the freeze and accepted erratum B-009 E-1 on 20-Oct-2026; the erratum is applied in place to the B-009 manual and addendum and recorded in DR-047.
Date: 2026-10-20 (revision 1; revision 0 the same day)
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; W-1). No VM, service, host or broker operation and no change to legacy source; this manual records a retired system, not an operating procedure.
Working copy of: docs/inputs/manuals/B-010_END_OF_DAY_OPERATION_MANUAL_DRAFT_20-OCT-2026.md (ChatGPT's draft). With it come ChatGPT's reconciliation of 20-Oct, the answer sheet and research prompt of 20-Oct, and Claude's repository answers and source inventory of 20-Oct; all are stored unchanged at the freeze. The draft's structure and wording are kept. Paths are made full, line references were rechecked at the baseline, and the changes are listed in section 20.
Baseline: trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb, read 2026-10-20 through the masking filter. Line numbers are at this commit.
Security: no credential, token, account or client identifier, host, address or person name is reproduced.

> The legacy system is DEAD / REFERENCE ONLY. This document records historical source behaviour. It is not operating guidance and must not be used to revive or run the system.

## 1. Evidence and interpretation rules

| Label | Meaning |
|---|---|
| FACT (SOURCE-VERIFIED) | Established by source code/configuration at the pinned commit. |
| FACT (RECORD-DERIVED) | Established only by a cited historical record and its stated date/deployed commit. |
| INTERPRETATION | A consequence inferred from source paths; not evidence that it happened in production. |
| KNOWN DEFECT | A concrete source-level mismatch or failure path. |
| KNOWN DOC DRIFT | Comments/docstrings/configuration descriptions disagree with executable source. |
| UNKNOWN / NOT ESTABLISHED | The inspected evidence cannot establish the fact. |
| OPEN QUESTION | Requires more evidence or an operator record. |

Source code does not prove that a particular version was installed or ran on a VM. Cron declarations do not prove installation or execution. Tests were inventoried but not run; no test-pass claim is made.

## 2. Scope and boundaries

B-010 covers:
- the separate MIS AutoSquareoff passes configured for 15:03 and 15:06, and the 15:09 cutoff;
- the 15:15 order-monitor entry cancellation / SOFT_KILL control, insofar as it precedes EOD;
- the general EodSquareoff routine configured for 15:17;
- cancellation and exit paths, completion logging, WAL checkpoint, event publication, daily P&L reset and summary alert;
- source-level restart recovery and the declared post-market cron schedule.

Related topics are delegated rather than redefined:
- B-009: intraday signal/order lifecycle and the 15:00 entry end;
- B-001–B-006: startup, configuration, state initialization, broker connection, startup reconciliation and workers;
- B-007: health/readiness semantics;
- B-011: service self-exit/shutdown;
- B-012: restart detail;
- G topics: order/position lifecycle and reconciliation;
- I topics: kill-switch semantics;
- L topics: alerts/reports;
- M topics: cron installation.

## 3. Architecture and ownership

### 3.1 Main components

| Component | Role | Source evidence |
|---|---|---|
| `EodSquareoff` | General 15:17 compound EOD routine | `main.py` 3300–3319; `orders/eod_squareoff.py` 202–236, 326–570 |
| `MisAutoSquareoff` | Separate MIS-only two-pass safety routine | `main.py` 3323–3380; `orders/mis_autosquareoff.py` |
| `MisSquareoffNotifier` | Separate human-facing MIS critical notification path | `main.py` 3338–3357; B-006 thread record |
| `OrderMonitor` | Tracks EOD exit orders and runs the 15:15 tracked-entry cancellation/SOFT_KILL path | `orders/eod_squareoff.py` 1313–1323; `broker/order_monitor.py` 794–830 |
| `OrderReconciler` | Periodic reconciliation; EOD calls its stale-order sweep | `main.py` 3384; `orders/order_reconciler.py` 472–537 |
| `FundManager` | Releases reservations after successful cancellation and resets daily P&L | `orders/eod_squareoff.py` 907–922, 529–534; `capital/fund_manager.py` 1687–1717 |
| `StateStore` | EOD log, candidate queries, order/trade state and WAL checkpoint | `core/state_store.py` cited ranges below |
| Event bus | Publishes `EodSquareoffComplete` | `orders/eod_squareoff.py` 512–527; `core/events.py` 173–186 |
| Telegram notifier | General EOD summary; also direct MIS notification attempt | `orders/eod_squareoff.py` 559–565; `orders/mis_autosquareoff.py` 1295–1324 |

“Constructed/wired” means the source builds the component and connects the described path. It does not prove that the component was alive on a host.

### 3.2 EOD wiring

At the baseline, `main.py` constructs `EodSquareoff` with the broker adapter, store, fund manager, state machine, event bus, market windows, time authority, kill switch, logger, order monitor, notifier and configuration values:
- `inter_order_delay_ms` from configuration;
- `market_close = "15:30"`;
- `exit_protocol = "LIMIT_THEN_MARKET"`;
- `limit_aggressive_pct = 0.01`;
- `limit_grace_sec = 120`.

The constructor call does **not** pass `entry_gate`. It later wires the stale-order sweep to `OrderReconciler.sweep_stale_orders`, late-binds the EOD instance for the daily-loss callback, and calls `post_wire_init()` for restart recovery. Relevant paths: `main.py` 3300–3321, 3384, 3866; `orders/eod_squareoff.py` 132, 340–347.

**Known defect / cross-manual erratum:** `_fire()` contains conditional entry-gate clearing, but the gate is not passed by the baseline `main.py` wiring. The clear operation is therefore inert in the wired path. This corrects the statement in the frozen B-009 manual that the 15:17 routine clears the gate. It does not change B-009’s separate finding about no runtime path feeding new candidates into `EntryGate.add()`. Do not silently edit the frozen B-009 manual; record an approved erratum.

## 4. Clock and schedule model

Configured times at the baseline (declarations, not proof of execution):

| IST | Control | Meaning |
|---|---|---|
| 14:45 | EOD pre-alert | Warning notification only |
| 15:00 | `entry_end` | Normal entry window ends |
| 15:03 | MIS PASS 1 | First MIS-only safety pass |
| 15:06 | MIS PASS 2 | Second MIS-only safety pass |
| 15:09 | MIS cutoff | Deadline used in outcome classification; no independent action |
| 15:15 | Placement cutoff / order-monitor control | Placer refuses new orders; monitor cancels tracked entry orders and sets SOFT_KILL; closes no positions |
| 15:17 | General EOD | Compound cancellation, exit, persistence and accounting sequence |
| 15:30 | `market_close` | Fixed recovery-path limit when no EOD log row exists |
| 15:40–18:50 | Post-market jobs | Cron declarations in registry/generated file |
| 17:35 | `service_window_end` | Service self-exit path; detailed treatment is B-011 |

Sources: `config/system_config.yaml` (trading hours and EOD configuration); `core/mis_squareoff_timing.py` 146–159; `core/market_windows.py` 93–106, 143–205; `main.py` 931–970, 4093–4161; `broker/order_monitor.py` 794–830.

The EOD poller checks every five seconds. The MIS poller also checks every five seconds. The EOD scheduler checks the effective date-specific EOD time and holiday status; the MIS pass times are built from fixed configured offsets. Startup validates ordering of entry end, MIS pass times, cutoff and EOD time.

**Special-session limitation:** the EOD due check can use a configured special-session EOD time. The MIS CHECK_1/CHECK_2/cutoff, global entry window and 15:15 control remain fixed clock values. The baseline `special_sessions` list is empty. Actual special-session behaviour is UNKNOWN / NOT ESTABLISHED.

**Cron evidence boundary:** `config/cron_registry.yaml` is read by the registry loader and used by `scripts/generate_crontab.py` to generate `deploy/cron/trading-system.cron`. The repository contains declarations, not proof of the installed crontab/timers or of any run.

## 5. Separate MIS AutoSquareoff unit

### 5.1 Purpose and eligibility

`MisAutoSquareoff` is a separate routine from general EOD. It scans broker positions whose product is exactly `MIS`, with a symbol and non-zero quantity. CNC, CO, NRML and unknown products are excluded by this unit. The inspected path does not test whether a position is system-owned.

Sources: `orders/mis_autosquareoff.py` 459–476, 549–569; construction in `main.py` 3359–3380.

### 5.2 Pass timing and order behaviour

| Pass | Configured time | Exit type | Protection band | Retry / verification |
|---|---:|---|---:|---|
| PASS 1 | 15:03 | MARKET | 1.5% | Cancels and verifies local MIS SL/TGT before exit; inconclusive broker state can remain retryable |
| PASS 2 | 15:06 | MARKET | 2.5% | Resolves PASS 1 residuals, reads open orders, cancels/verifies a live PASS 1 order, re-reads position before sizing; no grace retry loop |
| Deadline | 15:09 | Classification cutoff | — | Verification can report `MIS_REMAINS` or `DEADLINE_BREACH`; alerts do not themselves close positions |

Sources: `core/mis_squareoff_timing.py` 103–159; `orders/mis_autosquareoff.py` 320–387, 431–510, 702–916, 1177–1256.

For each candidate, the unit reads local resting MIS SL/TGT orders, attempts cancellation and checks broker order history for confirmation. If cancellation fails or is not confirmed, it attempts to restore the original stop loss and does not submit that exit on that path. Restoration reads the original SL row, checks broker open orders, re-places the SL and verifies order history. The TGT is deliberately not restored. If source state is insufficient or confirmation times out, the result can remain unknown.

Post-pass verification immediately re-reads broker positions. The source does not establish a settle delay between exit placement and verification; an in-flight MARKET order can therefore still appear open at that instant (interpretation).

### 5.3 MIS documentation drift and notification paths

**Known documentation drift:** the module header describes PASS 1 as `LIMIT_THEN_MARKET` with capped grace, but the executable PASS 1 path places MARKET with a 1.5% protection band. `effective_grace_sec` and `_retry_remaining_mis_positions` have no caller in the reviewed runtime search; the stored `limit_grace_sec` is not used on the executable path. The timing helper docstring also retains an older 15:07 / 15:10 / 15:12 schedule.

**Known notifier defect, scoped precisely:** `_emit()` calls the direct notifier as `send(severity=..., title=..., message=detail)`, but `TelegramNotifier.send()` requires `body` and `source_module` and has no `message` parameter. The direct send attempt raises and is caught/logged. However, `main.py` separately wires a `critical_sink` to `MisSquareoffNotifier`, called before the direct notifier attempt (`orders/mis_autosquareoff.py` 1306-1314). That notifier's own Telegram call is correctly formed (`send(severity, title, body, source_module)`), and it has an e-mail fallback (`main.py` 3343-3353; `alerts/mis_squareoff_notifier.py` 290-310). Only the direct, duplicate call is defective; the MIS critical path as a whole is not broken in source. Actual notification delivery on any date is UNKNOWN / NOT ESTABLISHED.

Sources: `orders/mis_autosquareoff.py` 1295–1324; `alerts/telegram_notifier.py` 348–356; `main.py` 3338–3357, 3377–3379.

### 5.4 Historical records (limited scope)

The production repository contains:
- a 03-Sep-2026 ANANTRAJ incident record, identified as deployed commit `2d08436`;
- a 10-Sep-2026 MIS square-off rejection audit;
- a 22-Jul-2026 investigation;
- a 03-Sep-2026 exit-path design draft (design, not an operational record).

The records report, within their own stated scope, a naked-position window after protective-order cancellation, an unprotected MARKET rejection, and manual closure. Source comments mention 27-Aug TATAPOWER and 02-Sep COALINDIA, but no separate record file for those dates was found by name in the scoped search. Later source code does not prove that the revised path was deployed or executed after those incidents.

## 6. General EOD trigger and sequence

### 6.1 Trigger and duplicate prevention

`main.py` starts the EOD polling path; `EodSquareoff.start_polling()` runs a daemon poll loop every five seconds and calls `check_and_fire(now_ist())`. The due check requires the configured effective EOD time and a non-holiday date. A per-date in-memory flag under a lock prevents a second scheduled fire in the same process. Exceptions reset the flag and are retried by the poll loop.

A late start can fire at the first poll after the due time; `check_and_fire()` has no 15:30 upper bound. The no-log-row restart recovery path does have a 15:30 limit.

Sources: `orders/eod_squareoff.py` 202–236, 290–312; `core/market_windows.py` 194–205.

### 6.2 `_fire()` executable sequence

| Step | Operation | Evidence / qualification |
|---:|---|---|
| 0 | Timestamp and duration start | `orders/eod_squareoff.py` 334–338 |
| 1 | Clear entry gate if wired | Conditional; inert in baseline wiring |
| 2 | Clear retest monitor if wired | Only when that monitor is provided |
| 3 | Write `IN_PROGRESS` EOD log row | Insert failure is logged and sequence continues |
| 4 | Set SOFT_KILL only if no kill is already active | Records whether EOD itself set it |
| 5a | Cancel pending entry orders selected by StateStore | Known status mismatch below |
| 5b | Cancel pending SL/TGT exit legs for open positions | Failures are counted; pass 2 continues |
| 6 | Sleep two seconds | Intended to let cancel-race/“phantom” fills reach monitor/state |
| 7 | Re-query and exit open intraday positions | LIMIT-then-MARKET path and residual sweep |
| 8 | Call stale-order sweep if wired | Best effort; exceptions do not abort EOD |
| 9 | Log summary counts/duration | Counts are order actions, not confirmed fills |
| 10 | Update log to `COMPLETE`; fallback insert only if UPDATE raises | Zero-row UPDATE is not detected |
| 11 | SQLite WAL checkpoint in TRUNCATE mode | Failure logged; sequence continues |
| 12 | Publish `EodSquareoffComplete` | Event means sequence/counts, not flat book |
| 13 | `fund_manager.reset_daily_pnl()` | After exits are placed, before their fills necessarily occur |
| 14 | Resume SOFT_KILL only if EOD set it | Does not resume a kill owned by another path |
| 15 | Build `EodFireResult` | Contains counts, duration and recovery flag |
| 16 | Send daily summary if notifier exists | Best-effort |
| 17 | Increment effect telemetry and return | `EodFireResult` produced |

Primary source: `orders/eod_squareoff.py` 326–570.

### 6.3 Entry cancellation query mismatch

`StateStore.get_pending_intraday_orders()` selects trades whose status is exactly `PENDING_FILL` and whose ENTRY product is MIS or CO (`core/state_store.py` 1015–1039). The order placer attempts to transition the trade to `PENDING` before calling the broker (`orders/order_placer.py` 1061–1071). The reviewed runtime search found no update back to `PENDING_FILL`.

**Known defect:** a resting entry whose status is `PENDING` can be missed by EOD's pending-entry query. The relevant test seeds `PENDING_FILL` directly and does not exercise the transition. This does not establish that the independent 15:15 OrderMonitor cancellation never runs; that path depends on the monitor being alive. Host state is unknown.

### 6.4 Position selection and order pricing

The local query selects OPEN or PARTIAL trades with positive filled quantity and ENTRY product MIS or CO (`core/state_store.py` 1207–1250). EOD then reads broker positions and filters to non-zero MIS/CO positions. If a local symbol is absent from the broker view it is skipped with a warning. If the broker read fails, source falls back to local DB quantities. Broker quantity is used for sizing where available. CNC/delivery is excluded.

For the configured `LIMIT_THEN_MARKET` path:
1. Read quotes in a batch.
2. Submit a LIMIT at LTP adjusted by the configured aggressive percentage (baseline 1%) in the exit direction.
3. If no LTP is available, attempt MARKET.
4. Track the EOD order with OrderMonitor and persist an order row.
5. Wait the configured grace (baseline 120 seconds), re-read broker positions, cancel unfilled LIMITs and attempt MARKET for remaining quantity.
6. Run a residual broker-position sweep.

Sources: `orders/eod_squareoff.py` 1027–1425, 1427–1590, 1592–1722; `broker/zerodha_adapter.py` 2116–2119.

**Known defect / risk:** the examined EOD MARKET call sites omit `market_protection`. The adapter does not send a protection band when none is supplied. Historical records report broker rejection for MARKET orders without protection; that is record-derived evidence, not proof that this baseline code executed on a specific date. If promotion re-reading fails, the source leaves LIMITs in place. If a MARKET fallback fails, the trade is marked `EOD_EXIT_FAILED`.

**Quantity interpretation:** EOD maps broker quantity by symbol, while iterating local trades. With multiple local trades for one symbol, the same broker quantity could be considered for more than one trade. This is an inferred risk; occurrence is UNKNOWN / NOT ESTABLISHED.

### 6.5 Exit legs, fills and completion semantics

EOD attempts to cancel SL/TGT legs before flattening. A failed cancellation does not stop pass 2; a surviving protective leg could later fill after an EOD exit and create reverse exposure (interpretation). The stale-order sweep only marks local order rows cancelled when the parent trade is terminal; it does not itself cancel broker orders.

EOD does **not** wait for all exits to fill or verify a flat broker book. It registers placed exit orders with OrderMonitor. `positions_succeeded` counts actions such as exit orders placed or accepted CO cancels/promotions; it is not a fill count. Later fills are handled asynchronously through order-monitor events and reconciliation.

Therefore:
- a `COMPLETE` EOD row does not prove all positions are flat;
- `EodSquareoffComplete` does not prove all positions are flat;
- a successful WAL checkpoint does not prove broker or trade correctness.

## 7. Persistence, ledger, events and summaries

### 7.1 `eod_squareoff_log`

Schema: `core/schema.sql` 611–639. Principal columns include unique `fired_date`, `fired_at`, attempted/succeeded/failed counts, duration, status and completion timestamp. EOD writes `IN_PROGRESS` at start and later updates to `COMPLETE`, with an insert fallback if the update raises. Recovery reads this table.

**Known persistence defect:** the COMPLETE update does not check affected-row count. If the write-ahead insert failed but the update returned successfully while changing zero rows, the fallback is not entered and the date may have no row. A later recovery fire is a possible consequence, not a confirmed production event.

### 7.2 Restart recovery

`post_wire_init()` invokes `_check_restart_recovery()` after event-bus subscriptions are wired. Source-level cases:
- COMPLETE row: mark the date fired; no retry.
- IN_PROGRESS row: mark fired and run a recovery fire; an exception alerts and does not automatically retry within that recovery method.
- No row before EOD time or on a holiday: return.
- No row after 15:30 with positions open or position state unknown: log/alert `EOD_SKIPPED_LATE`; do not fire.
- No row after EOD time but before 15:30: run a recovery fire.

The recovery helper's existence does not prove that a host restarted or that recovery ran. The source ordering indicates recovery can run before startup reconciliation and before OrderMonitor's polling thread starts; this is an interpretation from initialization order. General restart governance is B-012.

### 7.3 Daily P&L reset

`FundManager.reset_daily_pnl()` reads the day's realised net P&L and writes a `RESET_PNL` ledger row:
- amount = 0;
- balance_before = previous value;
- balance_after = 0;
- pnl_delta = negative previous value.

Reserved and used capital are not reset by this method. It is called after EOD exit orders have been placed but before they necessarily fill. Consequently, fills that occur later may be booked after the reset row. Repeated EOD fires can create repeated reset rows. This describes source ordering only; no accounting recommendation is made.

### 7.4 Event and summary

`EodSquareoffComplete` carries the fired date and counts. The event docstring describes successful positions as MARKET exits even though the configured baseline protocol is LIMIT_THEN_MARKET; this is documentation drift. The inspected runtime search found `ShadowTracker` as the subscriber, and that feature is disabled by the baseline config.

The daily summary reads trades for the date and sends an INFO Telegram message. The body formatter was not fully line-reviewed for this draft. Because the summary is called immediately after exit placement, trades whose exit fills are pending may not yet have final P&L. Actual alert delivery is UNKNOWN / NOT ESTABLISHED.

### 7.5 WAL checkpoint

EOD calls the StateStore checkpoint in TRUNCATE mode and logs the result; exceptions are logged and do not abort the routine. A separate `wal_checkpoint` cron job is declared for 16:00 and uses PASSIVE mode. Checkpoint success only concerns SQLite WAL state at that moment.

## 8. Failures and partial completion

| Failure | Source behaviour | Evidence boundary |
|---|---|---|
| Per-order cancellation fails | Log CRITICAL, increment failure count, continue | Source fact |
| Exit placement fails | Count failure; mark `EOD_EXIT_FAILED` on relevant paths; continue | Source fact |
| Broker positions read fails during main position selection | Fall back to local DB view/quantity | Source fact; inferred safety consequences must be labelled |
| Quote read fails | MARKET fallback path | Source fact; unprotected MARKET issue applies |
| Promotion position re-read fails | Leave LIMITs in place | Source fact |
| Residual broker read fails | Skip residual sweep | Source fact |
| Store read in selected unguarded paths raises | May abort `_fire()` after write-ahead/kill-switch steps; scheduled poll retries after flag reset | Source fact from cited call paths |
| Write-ahead insert fails | Log error and continue | Source fact |
| COMPLETE update raises | Attempt fallback insert | Source fact |
| COMPLETE update returns zero rows without raising | Fallback not entered; row can be missing | Known defect |
| Scheduled fire has partial failure without exception | In-memory flag remains set; no scheduled retry that day | Source fact |
| `fire_now()` has partial failure | Resets date flag; 15:17 may rerun sequence | Source fact |
| Checkpoint/event/summary errors | Logged; later steps generally continue | Source fact |

`EodFireResult` contains attempted/succeeded/failed counts, duration and recovery flag. `recovery_fire` is in the result object but not in the persisted EOD log: the table has no such column (`core/schema.sql` 618-636).

## 9. Kill-switch and daily-loss path

For ordinary EOD, SOFT_KILL is set only if no kill switch is already active. EOD resumes it only if EOD itself set it. The independent 15:15 path also applies SOFT_KILL when the OrderMonitor runs.

The daily-loss callback (`main.py` 756–817) alerts, calls `fire_now(reason="daily_loss_limit_breached", triggered_by="fund_manager")`, catches failure, and sets SOFT_KILL afterward. `fire_now()` bypasses time/holiday checks. If the EOD object is unavailable or its fire fails, the callback still sets SOFT_KILL but position closure is not established by that fallback alone. The brief interval between EOD's conditional resume and the callback's subsequent SOFT_KILL is a source-order interpretation.

Detailed kill-switch semantics belong to the I topics; this section records only the EOD interaction.

## 10. Post-market jobs

The generated crontab lists these declared jobs among others:

| Time | Job |
|---:|---|
| 15:40 | `fetch_daily_candles` |
| 15:45 | `reconcile_positions` |
| 15:50 | `eod_cleanup` |
| 15:50 | `reconstruct_excursions` |
| 15:55 | `eod_verify` |
| 15:58 | `eod_broker_reconcile` |
| 15:58 | `sr_detector_backfill` |
| 16:00 | `wal_checkpoint` |
| 16:01 | `generate_screened_csv` |
| 16:07 | `daily_trade_review` |
| 16:10 | `trade_journal` |
| 16:15 | `compute_strategy_metrics` |
| 16:16 | `metrics_summary` |
| 16:20 | `gemini_log_review` |
| 16:22 | `strategy_registry_officer` |
| 16:40 | `gemini_trade_coach` |
| 17:00 | `gemini_data_integrity_check` |
| 17:05 | `control_tower` |
| 18:00 | `check_cron_drift` |
| 18:15 | `forward_shadow_record` |
| 18:50 | `cron_officer_eod` |

Source: `config/cron_registry.yaml`, `scripts/generate_crontab.py`, `deploy/cron/trading-system.cron`. The job declarations and generated file do not prove installation or execution. Post-market script bodies were not reviewed for this draft. `daily_report` is marked disabled/retired in the registry.

## 11. Paper versus live

The EOD and MIS classes have no major mode-specific control branch in the inspected paths; both call through the adapter. The paper adapter simulates order placement/fills, returns positions from in-memory paper fills and simulates cancellations. Alert titles include a mode label.

**UNKNOWN / NOT ESTABLISHED:** operational parity between paper and live, and which mode was used on any particular day. A separate census referenced by B-009 reported no paper trades in its specific observation window; that does not establish all-time behaviour.

## 12. Historical evidence and limits

| Evidence | Scope | Does not establish |
|---|---|---|
| 03-Sep-2026 ANANTRAJ incident record, deployed `2d08436` | Describes the incident and the source/deployment then under review | Behaviour after that date |
| 10-Sep-2026 MIS square-off rejection audit | Reports an unprotected MARKET rejection and manual close within its record scope | Whether later baseline code was deployed |
| 22-Jul-2026 investigation | Records findings about `fire_now` and caller scope at that time | Deployment of any subsequent change |
| 03-Sep-2026 exit-path design draft | Design proposal only | Operational execution |
| Source comments about 27-Aug TATAPOWER and 02-Sep COALINDIA | Historical pointers in code comments | Independent incident evidence |

The source inventory says some named records, reports, post-market scripts and systemd files were not read. Do not imply they were audited. A broker-book export was deliberately not opened because it may contain account data.

## 13. End-to-end source walkthrough

### Ordinary configured day
1. At 15:03 and 15:06, the MIS unit scans broker MIS positions and attempts its two safety passes.
2. At 15:15, if OrderMonitor is running, its separate control cancels tracked entries and sets SOFT_KILL; it does not close positions. With that kill already active, EOD at 15:17 sets no kill of its own and resumes none (`orders/eod_squareoff.py` 380-392, 536-544).
3. Around 15:17, the EOD poll sees the due time and claims the date in memory.
4. `_fire()` attempts gate/retest cleanup (gate cleanup is inert in baseline wiring), writes IN_PROGRESS, conditionally sets SOFT_KILL, cancels selected entries and exit legs, waits two seconds, then re-queries positions and places exit orders.
5. It runs a best-effort stale-order sweep, writes completion counts, checkpoints the WAL, publishes the event, resets daily P&L, conditionally resumes the kill switch, sends the summary and increments telemetry.
6. Exit fills may arrive later through OrderMonitor; the reconciler runs independently. EOD completion is not proof of a flat broker book.
7. Post-market cron jobs are declared for 15:40 onward; actual execution is unknown. The service self-exit is covered by B-011.

### Daily-loss path
1. FundManager's callback alerts.
2. It calls `fire_now()`, which bypasses EOD clock/holiday guards.
3. EOD executes the same compound sequence; the callback then applies SOFT_KILL.
4. If the fire partially fails or raises, a later 15:17 run may repeat the sequence, including another P&L reset row. Host occurrence is unknown.

## 14. Known defects and documentation drift register

| ID | Classification | Finding |
|---|---|---|
| D-1 | Known defect / wiring | EOD entry-gate clear is conditional, but baseline `main.py` does not pass the gate; the step is inert. |
| D-2 | Known defect | EOD pending-entry query selects `PENDING_FILL`; placer transitions to `PENDING`; no runtime transition back was found in the reviewed search. |
| D-3 | Known defect / record-supported risk | EOD MARKET call sites omit market protection; historical records report broker rejection for unprotected MARKET orders. |
| D-4 | Known defect | COMPLETE update does not verify affected-row count; a missing write-ahead row can remain missing without raising. |
| D-5 | Known defect | Direct MIS notifier call uses unsupported `message` parameter and omits required `body` and `source_module`; separate critical_sink path remains distinct. |
| D-6 | Known documentation drift | MIS PASS 1 header says LIMIT_THEN_MARKET; executable path uses MARKET with protection. |
| D-7 | Known documentation drift | MIS timing helper docstring retains old 15:07/15:10/15:12 schedule. |
| D-8 | Known documentation drift | MIS `is_alive` docstring says visible to `/health`; inspected health wiring does not include it. |
| D-9 | Known documentation drift | `auto_resume_kill_switch` is declared but no other runtime reader was found in the reported search. |
| D-10 | Known documentation drift | Event docstring describes MARKET exits despite LIMIT_THEN_MARKET configuration. |
| D-11 | Known documentation drift | EOD module header describes a simpler MARKET/kill-switch sequence than the executable sequence. |

## 15. Open questions / unknowns

- What crontab and systemd timers were installed on the host, and which jobs ran?
- Did EOD, MIS passes, recovery or `fire_now()` run on any specific date beyond the cited records?
- Were any notifications delivered on a specific date, including the separate MIS critical_sink route?
- Did the COMPLETE zero-row update path occur in production?
- Was the baseline source deployed after the historical incidents, and were subsequent changes deployed?
- Was any external supervisor configured, and what did it do?
- Did special-session behaviour ever occur?
- What was actual paper/live parity?
- Did a `gate_state` row exist on the host?
- What did the operator observe between 15:03 and 15:17 on ordinary days, and how were manual closes performed?

## 16. Inspection scope and completeness

The companion source inventory documents selected paths, line ranges, search patterns, test inventory, records read/not read, and a disclosed process exception. This was a selected-path review, not a complete-tree audit. No code or tests were run; no VM, broker or database was contacted. Eight test files were inspected by names/selected assertions only; no test result is claimed. Several named tests and records, report modules, post-market script bodies and systemd files remain unread.

## 17. Cross-references

- B-009: intraday signal/order lifecycle and 15:00 entry end (frozen manual requires approved erratum for the EOD gate-clear statement).
- B-011: service self-exit and shutdown.
- B-012: restart detail.
- B-001–B-007: startup, configuration, state, broker connection, reconciliation, workers, readiness.
- G topics: order/position lifecycle and reconciliation.
- I topics: kill-switch semantics.
- L topics: alerts and reports.
- M topics: cron installation.

## 18. Rechecks at the baseline
Every line reference in this manual was read at 970aabf on 2026-10-20 (B-010_review_notes.md section 3), including:
- the COMPLETE update's missing row-count check (`core/state_store.py` 1386-1406);
- the direct MIS notifier signature (`orders/mis_autosquareoff.py` 1316-1322 against `alerts/telegram_notifier.py` 348-356);
- the separate critical_sink path (`main.py` 3343-3353, 3377-3378).

Negative searches cover all 330 Python files outside tests/. Unresolved items stay UNKNOWN / NOT ESTABLISHED (section 15).

## 19. Erratum B-009 E-1 (ACCEPTED, DR-047)
- Finding: `main.py` constructs `EodSquareoff` without `entry_gate` (3300-3319). The only `entry_gate=` in runtime code is the shutdown call (4254). The retest monitor is set only on the disabled retest path (3794). So `_fire`'s first two steps (`orders/eod_squareoff.py` 340-356) are inert at the baseline (section 3.2, D-1).
- The frozen B-009 files state otherwise in four places. Each would be struck through in place with "(superseded by erratum E-1)" and the replacement given; history is not rewritten.

| B-009 file and line | Current text | Replacement |
|---|---|---|
| B-009_manual.md line 245 (section 11) | "The 15:17 routine first clears the entry gate and parked retest candidates (eod_squareoff.py 340-352); the rest is B-010's." | "The 15:17 routine's first steps would clear the entry gate and parked retest candidates (eod_squareoff.py 340-356), but main.py passes neither component to EodSquareoff (3300-3319; the retest monitor only on the disabled retest path, 3794), so the steps are inert at the baseline (B-010_manual.md section 3.2); the rest is B-010's." |
| B-009_manual.md line 294 (section 14, EOD row, Logic) | "Clear gate, cancel, exit, summary, reset" | "Cancel, exit, summary, reset (the gate clear is inert: no gate is passed)" |
| B-009_manual.md line 300 (section 15, item 1) | "and the 15:17 routine clears them, in memory and in the store (eod_squareoff.py 340-347; entry_gate.py 260-279)" | "and nothing clears them at the baseline: EntryGate.clear_all (entry_gate.py 260-279) would, but the 15:17 routine is not given the gate (main.py 3300-3319)" |
| B-009_reconciliation_addendum.md line 53 (item 7) | "and EOD clears them (entry_gate.py 260-279)" | "and nothing clears them at the baseline (EOD is not given the gate; B-010 D-1)" |

- Unaffected: B-009's conclusion that no source path feeds new signals to the gate (D1), and its statement that a host's gate_state contents are UNKNOWN. With nothing clearing the table, rows left by an earlier version would persist across days; whether any existed is UNKNOWN (section 15).
- Accepted by Rama on 20-Oct-2026 and recorded in DR-047. Applied in place: the B-009 manual (three passages, a status sentence and a closing "Erratum" section), the B-009 addendum (item 7 and a status sentence) and one sentence in B-009's review notes.

## 20. Changes from ChatGPT's draft (revision 0)
1. The header is replaced by this project's status block; the working-copy and baseline lines are added.
2. Partial paths are made full (`broker/order_monitor.py`, `orders/order_reconciler.py`).
3. Section 5.3: the critical_sink's correctly formed Telegram call and its e-mail fallback are cited from source.
4. Section 8: `recovery_fire` not persisted, now cited from the schema.
5. Section 13: on an ordinary day EOD sets and resumes no kill when the 15:15 kill is already active.
6. Section 18: the checklist is replaced by the recheck record.
7. Section 19: the B-009 correction is set out as erratum E-1, with exact text, PROPOSED.
8. Everything else is unchanged from ChatGPT's draft.

## 21. Revision 1 and freeze (2026-10-20)
- ChatGPT's review response of 20-Oct-2026 (B-010_CHATGPT_REVIEW_RESPONSE_20-OCT-2026.txt) answered the review notes' three questions:
  - erratum E-1's wording is acceptable, with one polish;
  - the V2 inputs are decision-neutral;
  - there is no blocking issue.
- The polish is applied in section 19: "main.py passes neither component to EodSquareoff".
- Rama approved the freeze and accepted E-1 on 20-Oct-2026. B-010 is FROZEN (DR-047). No other change.

