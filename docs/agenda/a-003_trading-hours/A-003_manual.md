# A-003 TRADING HOURS: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-06 (revision 2, after the ChatGPT value-added review of 06-Oct-2026). Recorded as DR-018.
Date: 2026-10-05, frozen 2026-10-06
Layer: LEGACY ONLY. The historical time model, not V2 hours. V2 ideas are PROPOSED in docs/governance/V2_DESIGN_LOG.md section 3.
Working copy of: docs/inputs/manuals/A-003_TRADING_HOURS_LEGACY_MANUAL_23-SEP-2026.txt (frozen 23-Sep-2026 in the ChatGPT phase; original untouched). Changes are listed at the end.
Scope: WHEN the legacy system was permitted or expected to operate and WHEN trading actions were permitted. systemd, cron implementation, start-up internals, queues, order management, MIS, GTT and reconciliation belong to later sections.
Labels: FACT, HISTORICAL, RECORD-DERIVED (from preserved documentation, not verified against executable source here), KNOWN DEFECT (USER-REPORTED), UNKNOWN / NOT ESTABLISHED, INTERPRETATION.
Security: no credential values.

## 1. Purpose
Document the historical trading-hours contract without redesigning it.

## 2. Production time contract (FACT, production config)

| Function | Production value | Meaning |
|---|---:|---|
| Market open | 09:15 IST | NSE regular-session open |
| Entry start | 10:00 IST | Global entry floor |
| Entry end | 15:00 IST | Global strategy-entry backstop |
| EOD entry cutoff | 15:15 IST | Absolute last order-placement deadline |
| MIS square-off cutoff | 15:09 IST | Primary protective MIS deadline |
| EOD square-off / backstop | 15:17 IST | EOD accounting / emergency backstop |
| Market close | 15:30 IST | NSE regular-session close |
| Service window end | 17:35 IST | Configured service-window boundary |

## 3. Entry window
- FACT: entry_start=10:00 is a global floor over per-strategy windows. Both the global window and the strategy's own window must permit an entry. (Rama's example: a strategy with an 11:00-11:30 window rejects its signals outside that window even while the global window is open.)
- FACT: entry_end=15:00 and eod_entry_cutoff=15:15 are distinct. The first governs strategy-entry eligibility; the second is the absolute last order-placement deadline.
- HISTORICAL (Rama): the entry cut-off applied to both intraday and delivery.
- HISTORICAL (Rama): the entry cut-off was operated and described as a "soft-kill" trigger at 14:45. The historical record conflated entry_end, eod_entry_cutoff, MIS square-off and the EOD backstop (a documentation issue per the 23-Sep reconciliation). Whether the code used the SOFT_KILL kill-switch state for the cut-off is not established.

## 4. MIS / EOD
- FACT: mis_squareoff_cutoff=15:09 is the production protective MIS deadline, with configured pre-cutoff checks 6 and 3 minutes before it (15:03, 15:06).
- FACT: eod_squareoff_time=15:17 is the EOD accounting/emergency backstop, not the primary MIS square-off.
- FACT: main.py has a 15:15 force-close callback that cancels pending entry orders; EOD square-off closes INTRADAY (MIS/CO) positions at 15:17 while DELIVERY (CNC) positions are carried.
- FACT: main.py has a 14:45 EOD pre-alert. It is an alert, not entry_end.
- HISTORICAL (Rama): square-off success was checked twice after execution (about 30 seconds after, and again 15:16-15:18), with Telegram and email alerts on failure. This differs from the configured pre-cutoff checks; the two may be different mechanisms or code generations, and their relation is not established (section 14).
- HISTORICAL (Rama, rationale): the system squared off earlier than the broker's own auto square-off (broker time recalled by Rama as 15:20; not a legacy config value) to stay safe.

## 5. Service start and end
- RECORD-DERIVED (file map; unverified, Q-19): service start was permitted only between 08:00 and 18:15; outside it, main.py exited with code 0 before loading configuration.
- HISTORICAL (Rama): the system booted at 08:15 IST on trading days and did not boot on weekends or holidays.
- FACT: production config sets service_window_end=17:35; its comments tie this to the 17:00 PB-01 EOD scanner plus a 35-minute buffer. Distinct from market close (15:30).
- Three different end-of-day values exist and are recorded separately, not merged: 17:35 (production config service_window_end, FACT); 18:15 (upper bound of the start-up guard and validator per the file map, RECORD-DERIVED; it is not service_window_end); 18:30 (Rama's recollection of the system stop, HISTORICAL).

## 6. Shutdown and open positions
- FACT per the production baseline document (repository-derived; not re-verified here): the service self-exits after the service window only when no active positions remain; if positions remain, or a HARD_KILL flatten is running, it stays alive.
- FACT (A-002): delivery holdings were intended to carry overnight.
- KNOWN DEFECT (USER-REPORTED): carried delivery holdings could therefore prevent the self-exit. Rama reports stopping the service manually (systemctl stop); if it was not stopped, the next-day start-up could fail and that trading day could be lost.
- UNKNOWN: how often this happened (no logs survive). It did not necessarily happen every day.
- INTERPRETATION: the self-exit check treated carried delivery holdings as active positions.

## 7. Signal queue / out-of-window signals
- FACT: production config has a signal queue with intraday expiry_sec=600. This alone does not establish the fate of every out-of-window signal.
- HISTORICAL (Rama): signals before entry start (10:00) were not processed (dropped); signals after the entry cut-off were dropped or rejected; some signals were queued, and whether every queued signal was processed or silently lost is unknown. "Dropped" is not shown to mean rejected with a recorded reason.
- UNKNOWN: the exact fate of every out-of-window signal (signal-processing chapter).

## 8. Clock / timezone
- FACT: production config states HH:MM values are IST; main.py uses the IST time authority in timing-sensitive code; config has broker clock-skew thresholds and periodic server-time probing.
- HISTORICAL (Rama): cron times on the VM were converted from UTC to IST.
- UNKNOWN: database timestamp timezone; whether comparisons used timezone-aware values.

## 9. Holidays / calendar
- RECORD-DERIVED (file map): main.py had a holiday guard reading a holidays file whose year is interpolated.
- HISTORICAL (Rama; not verified in source): a 2026 NSE calendar (with computed weekends) existed; there was no 2027 file.
- UNKNOWN: yearly-file selection (record-derived only) and the behaviour when a year's file is missing.
- INTERPRETATION: a failure from 1-Jan-2027 is possible but not proven.

## 10. Pre-market and post-market schedule (RECORD-DERIVED, not verified against executable cron source)
- Preserved overview: 05:00 token deletion; 08:15 token refresh; 08:20 monitoring canary; 08:30, 09:14, 09:15 pre-flight phases; 08:35 F&O ban list; 09:00 instrument refresh; 09:20 morning briefing.
- Rama's recollection differs in places: monitoring canary at 08:15; three pre-flight phases between 08:20 and 09:15; Cron Officer briefing at 08:55; an AI pre-market briefing (Gemini).
- Post-market: jobs from about 15:40 into the evening; some jobs, including midnight jobs, ran after the service had shut down (Rama).
- Cron size, not reconciled, possibly counting different units: cron_registry.yaml references 38 Python scripts (storage record); SYSTEM_MAP lists 44 command lines (baseline); Rama counts about 45 jobs (cron_officer). No single job count is a FACT until the registry is counted with a defined unit.
- Job authority and the cron vs in-process split belong to sections B and M.

## 11. PB-01 EOD scanner
- Existence and timing (FACT, config comment): the comments on service_window_end associate it with a 17:00 PB-01 EOD scanner plus a 35-minute buffer. This does not show that PB-01 controlled shutdown.
- Status (per ChatGPT's A-008 evidence, Q-17): PB01 was marked SHADOW.
- HISTORICAL (Rama): PB-01 was a placeholder; nothing was traded from it.
- PB-01 is not used here to settle A-002's next-day trading question or A-008's strategy-count question.

## 12. Time ordering
- FACT (production config): 09:15 <= 10:00 < 15:00 <= 15:15 <= 15:17 <= 15:30 <= 17:35.
- RECORD-DERIVED (file map; unverified, Q-19): validation chain market_open <= entry_start < entry_end <= eod_entry_cutoff <= eod_squareoff_time <= market_close <= service_window_end < 18:15.
- UNKNOWN: the exact validator and what happens on an invalid order.

## 13. Source to handoff
config/system_config.yaml, then HH:MM interpreted as IST, then configuration/schema validation, then time-window and EOD consumers, then entry eligibility, order deadline, MIS protection, EOD backstop and service-window termination, then order-management, scheduler, reporting and operational consumers. Function-level consumers are deferred to their chapters.

## 14. Historical conflicts (recorded, not resolved)
- entry_end: Rama 14:45 vs production 15:00.
- eod_entry_cutoff: Rama 14:45 vs production 15:15.
- MIS cutoff: Rama 15:15 vs production 15:09 (A-002 records "around 15:10" as recollection).
- EOD square-off: Rama 15:15 vs production 15:17.
- Service end: Rama 18:30 vs production 17:35 vs file-map 18:15.
- entry_start: testing VM 09:30 vs production 10:00.
- Square-off checks: config pre-cutoff 15:03 / 15:06 vs Rama's post-execution checks.
- Monitoring canary: overview 08:20 vs Rama 08:15.
The exact date or version of the earlier values is UNKNOWN unless commit evidence establishes it. Production config values form the frozen production baseline. External current broker timings (V2_DESIGN_LOG Q-18) are not legacy evidence and do not explain away these values.

## 15. Unknown / not established
- Exact systemd boot boundary (file-map claim unverified).
- Complete cron registry authority map and job count.
- Exact fate of every out-of-window signal.
- Complete EOD retry, promotion and sweep behaviour.
- Database timestamp timezone semantics.
- Complete validator function and invalid-config failure path.
- Behaviour when the current year's holiday file is missing.
- How often the delivery-holding shutdown defect occurred.
- The relation between the configured pre-cutoff checks and the recalled post-execution checks.
- The fate of silently expired queued signals.

## 16. Governance
Legacy factual baseline only. It does not prescribe V2 trading hours. The original manual's line "Additional request: in the new system the entry cut-off or soft-kill applies only to intraday, not delivery" is a V2 request; it is removed here and recorded as VP-19. Other V2 ideas from the A-003 discussion are VP-20 to VP-26 and an extension of VP-16 in V2_DESIGN_LOG.md.

## Changes from the original (23-Sep-2026)
Revision 2 (after ChatGPT review): scope statement; soft-kill conflation kept as a historical observation; end-of-day values labelled by evidence class; shutdown predicate re-labelled and defect frequency UNKNOWN; holiday and PB-01 claims split by evidence class; cron counts flagged as possibly different units; external broker timings excluded; earlier-value dates UNKNOWN; unknown list extended.
Revision 1 removed: the V2 request line (now VP-19). Added: per-strategy window example and both-books cut-off (3); post-execution checks and early square-off rationale (4); service-start window, 08:15 boot and the three end-of-day values (5); shutdown defect with delivery holdings (6); Rama's out-of-window signal account (7); UTC-to-IST cron conversion (8); holiday file detail and 2027 gap (9); pre/post-market schedule and cron counts (10); PB-01 placeholder (11); file-map validation chain (12); new conflicts (14); new unknowns (15).
