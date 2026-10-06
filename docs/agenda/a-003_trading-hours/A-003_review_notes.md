# A-003 TRADING HOURS: REVIEW NOTES

Date: 2026-10-05
Status: CLOSED 2026-10-06. ChatGPT's value-added review (06-Oct-2026) applied in revision 2 (section 7). A-003 frozen under DR-017, recorded as DR-018.
Place at: docs/agenda/a-003_trading-hours/A-003_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in the manual; V2 ideas in V2_DESIGN_LOG section 3.
Security: no credential values.

## 1. Sources reviewed
- docs/inputs/manuals/A-003_TRADING_HOURS_LEGACY_MANUAL_23-SEP-2026.txt (ChatGPT legacy manual)
- docs/inputs/reconciliation/A-003_TRADING_HOURS_SOURCE_RECONCILIATION_AND_FREEZE_23-SEP-2026.txt
- docs/inputs/chatgpt_originals_and_replies/A-003_TRADING_HOURS_DISCUSSION_CHECKLIST.txt (Rama's replies)
- Legacy baseline (production deep working tree, storage map); frozen A-001 and A-002; DR-001 to DR-017.
- External context: Zerodha support pages on intraday auto square-off (checked 2026-10-05).
- Hygiene: no hidden citation markers.

## 2. Overall
The ChatGPT manual's production time contract is source-based and sound, and it correctly keeps Rama's recollections as historical conflicts instead of overwriting config values. The gaps are record-derived facts the checklist itself cited but the manual left out, one significant user-reported defect, and one V2 request written inside the legacy manual.

## 3. Findings

| # | Finding | Fix in revised manual |
|---|---|---|
| G-1 | A V2 requirement sits inside the legacy manual ("Additional request: entry cut-off / soft-kill only for intraday"). Breaks the two-layer rule. | Removed from the manual; now VP-19. |
| G-2 | The service-start window is missing. The checklist cites the file map: start permitted 08:00-18:15, main.py exits 0 outside it before loading config; the validator chain ends "< 18:15". Rama: boots at 08:15 on trading days. | Added as RECORD-DERIVED, unverified against source (Q-19). |
| G-3 | Three different "end of day" values exist: 17:35 (config service_window_end), 18:15 (file-map start-window end / validator bound), 18:30 (Rama). The manual records only 17:35 vs 18:30. | All three recorded separately. |
| G-4 | Known defect missing: the service only self-exits with no active positions, but delivery holdings carry overnight by design. With holdings open, the service did not exit; Rama had to stop it manually (systemctl stop). If not stopped, the next-day boot failed and that day could not be traded. | Added as KNOWN DEFECT (user-reported), with the baseline's self-exit rule. |
| G-5 | Pre-market and post-market schedule left out entirely, though the checklist lists record-derived times. | Added as RECORD-DERIVED, unverified against cron_registry; authority stays with B and M. |
| G-6 | Holidays: only a 2026 calendar file existed (Rama); the file map says the year is interpolated. From 1-Jan-2027 the file would be missing; behaviour not established. | Added (HISTORICAL + INTERPRETATION). |
| G-7 | PB-01 17:00 EOD scanner: config ties service_window_end to it, and Rama says it was only a placeholder; nothing traded from it. Consistent with A-002 (UNKNOWN next-day trading) and Q-17 (PB01 SHADOW). | Added; cross-referenced. |
| G-8 | Square-off checks conflict: config has pre-cutoff checks at 15:03 and 15:06; Rama recalls two checks after square-off (about 30 seconds after, and 15:16-15:18). | Recorded as a conflict. |
| G-9 | Out-of-window signals: Rama says signals before 10:00 or after cut-off were dropped, but queued signals may have died silently. | Added as HISTORICAL (Rama) beside the existing UNKNOWN. |
| G-10 | Cron size: 38 Python scripts referenced by cron_registry.yaml (storage record), 44 command lines (SYSTEM_MAP via baseline), about 45 jobs (Rama, cron_officer). | Recorded with sources; not reconciled here. |

## 4. External context (not legacy config)
Rama's reason for squaring off early: the broker squares off intraday positions itself (he recalls 15:20). Zerodha's support pages state 3:20 PM for equity and that fresh MIS orders are blocked after that time; a Zerodha forum announcement says equity MIS moved to 3:25 PM. Sources differ, so V2 must check the current bulletin (Q-18).

## 5. V2 ideas from Rama's replies (to V2_DESIGN_LOG, PROPOSED)
VP-19 intraday-only entry cut-off (delivery may enter until 15:30 for BTST); VP-20 cancel pending intraday entries at the cut-off; VP-21 early square-off plus two verification checks and alerts; VP-22 clean shutdown with carried delivery holdings; VP-23 time discipline (IST everywhere, timezone-aware timestamps, DB timezone, clock skew); VP-24 reject out-of-window signals immediately with a reason; VP-25 cron review and classification; VP-26 AI briefings move from Gemini to ChatGPT. VP-16 extended (2026 and 2027 calendars, fail-safe when a year's file is missing).

Considerations recorded with the proposals:
- VP-19: keep the intraday entry cut-off a separate, named control from the SOFT_KILL kill-switch state. The legacy record shows these were conflated.
- VP-22 is the fix for the G-4 defect, which stopped a whole trading day.

## 6. Next step (done)
Sent to ChatGPT; its review is applied below.

## 7. Disposition after the ChatGPT value-added review (06-Oct-2026)
All points adopted in A-003_manual.md revision 2; none rejected.

| ChatGPT point | Outcome |
|---|---|
| 1. V2 request | Removed (VP-19). Rama's account that the cut-off ran as a "soft-kill" trigger kept as a historical observation; code use of SOFT_KILL not established. |
| 2-3. Service window values | 17:35 (FACT), 18:15 (RECORD-DERIVED start-up/validator bound, not service_window_end), 18:30 (HISTORICAL) kept separate. 08:15 boot stays recollection. |
| 4. Shutdown defect | Sequence kept; predicate labelled as baseline-derived, not re-verified; defect user-reported; frequency UNKNOWN. |
| 5. Schedules | Labelled RECORD-DERIVED, not verified against executable cron source; authority stays with B and M. |
| 6. Holidays | 2026 file per Rama; selection and missing-year behaviour UNKNOWN; 2027 failure only an interpretation. |
| 7. PB-01 | Existence/timing, SHADOW status, and Rama's recollection separated; not used to settle shutdown, A-002 or A-008 questions. |
| 8. Square-off checks | Kept separate; may be different mechanisms or generations. |
| 9. Out-of-window signals | "Dropped" not upgraded to "rejected with reason". |
| 10. Cron counts | Three populations kept; possibly different units; no single FACT count. |
| 11. Zerodha timings | External context only (Q-18); not in the legacy manual. Rama's 15:20 kept as his recollection. |
| 12-13. Production contract and conflicts | Retained unchanged; earlier-value dates UNKNOWN. |
| 14. Scope | Scope statement added; detail cross-referenced. |
| 15. VP-19 to VP-26 | Remain PROPOSED in V2_DESIGN_LOG; not approved architecture. |
| 16. Classification | Unknown list extended (defect frequency, check relation, expired queued signals). |

## 8. Remaining evidence needing Rama's adjudication
None blocks the freeze: every conflict is recorded rather than resolved. Open items stay with later topics: Q-17 (strategy-YAML count, A-008), Q-18 (current Zerodha square-off time, V2), Q-19 (service-start guard and validator bound, B-001).
