# A-001 SYSTEM PURPOSE: REVIEW NOTES

Date: 2026-10-05
Status: CLOSED 2026-10-05. Partly superseded by the ChatGPT value-added review (section 9). The legacy A-001 manual was approved and frozen (A-001_manual.md, DR-013).
Place at: docs/agenda/a-001_system-purpose/A-001_review_notes.md
Security: no credential values.

## 1. Sources reviewed
- docs/inputs/manuals/A-001_SYSTEM_PURPOSE_FROZEN_MANUAL.md (legacy manual)
- docs/inputs/chatgpt_originals_and_replies/A-001_SYSTEM_PURPOSE_DISCUSSION_CHECKLIST.txt (ChatGPT questions + Rama's replies)
- docs/inputs/legacy_baseline/ (production deep working tree 21-Sep, storage map 18-Sep) and the Master Redesign Agenda
- V2 decisions DR-001 to DR-012
- No A-001 reconciliation file exists (confirmed by Rama).

## 2. Findings on the legacy manual (hygiene)

| # | Finding |
|---|---|
| H-1 | 9 hidden ChatGPT citation markers ("filecite...") sit in the text (lines 29, 37, 39, 49, 57, 75, 129, 145, 157). Remove in the V2 copy; the original in inputs/ stays untouched. |
| H-2 | The frozen-purpose block shows literal asterisks (escaped bold, line 172). |
| H-3 | Section 9 files a negative finding (not a portfolio platform) under the label UNKNOWN / NOT ESTABLISHED. It is a finding, not an unknown. |
| H-4 | The evidence period is not stated (first bullet of checklist item 12 was never answered). The VM termination is not recorded. |
| H-5 | Execution responsibilities were left as "re-establish after reviewing the current system" (checklist item 5). Carried to the G section (Order Execution). |

## 3. Conflicts to resolve

| # | Conflict | Evidence |
|---|---|---|
| X-1 | Intraday only, or intraday plus positional/holdings? | Baseline calls the legacy "an unattended NSE intraday trading application", yet it also documents a positional capital bucket, broker-side CNC protection and EOD scanner routing. Rama's reply says holdings were handled "but not in the expected manner". The manual never mentions intraday or positional. |
| X-2 | Manual says evidence does not support long-term portfolio management. Rama's reply to item 9 says it did perform it, badly (holdings without auto-square-off; no per-holding Telegram summary). | Manual section 9 vs checklist item 9. |
| X-3 | Manual lists 3 known defects (slippage/cushion, price ambiguity, entry timeout). Rama also reported bugs in decision-making criteria, and the Master Agenda names late entry, unsuitable filtration, incorrect S&R range and trading near high-of-day without S&R context. These are missing from the manual. | Checklist items 9-10; Master Agenda section D. |
| X-4 | Project instructions say "zero manual intervention", but Rama's replies keep manual kill-switch release and manual interruption as necessary. | Instructions 5.5 vs checklist items 7-8. |
| X-5 | Manual mentions a testing VM and a Windows PC. Rama's reply says production ran on an Oracle Ubuntu VM. Both VMs are now terminated. | Manual section 7; checklist item 7; Rama 2026-10-05. |
| X-6 | The automation lifecycle in Rama's reply ends with "prepare reports". DR-009 removed .xlsx generators in favour of a GUI DB download. | Checklist item 4 vs DR-009. |

## 4. V2 intents found in Rama's replies (outside the legacy manual by design)
- Live-only, no paper (already DR-001).
- Separate signal processors per signal type instead of one centralised secondary score, because intraday and positional use different timeframes and evaluation methods.
- Existing calculation bases (time, price, logic) must be reviewed in detail before any logic is fixed.
- Automated start-to-close day: health/pre-flight check, Zerodha token, instrument list, config check, signals, decisions, trade follow-up, capital, P&L, Telegram and email alerts, reports, shutdown, independent cron jobs, preparation for the next trading day.
- Start and stop follow the config file; trading days follow NSE (holidays and weekends).
- Windows 11 for development/debugging (read as development; correct me if production on Windows was meant), Ubuntu VM for production, no file-path collisions.
- Per-holding Telegram status summary (purchase date, quantity, entry, SL, target).

## 5. Proposed A-001 outcome by element (Master Agenda KEEP / MODIFY / CHANGE / REMOVE)

| Element | Legacy | V2 proposal | Outcome |
|---|---|---|---|
| Market / segment | NSE equity | NSE equity only | KEEP |
| Broker | Zerodha | Zerodha only | KEEP |
| Signal source | Chartink via webhook | Chartink (ingress mechanism decided in section C) | KEEP |
| Headless, unattended | Yes | Yes | KEEP |
| Operating modes | Paper + live | Live only | REMOVE paper (DR-001) |
| Trade decision | One centralised secondary score | Per-signal-type processors (decide at D / A-008) | CHANGE (proposed) |
| Reports | .xlsx generators | GUI DB download | CHANGE (DR-009) |
| Operator controls | Manual kill-switch release | Explicit, limited manual boundary | MODIFY |
| Positional / holdings | Supported, unsatisfactory | In scope; exits and alerts redesigned (A-007, A-008, L) | MODIFY |
| Environments | Ubuntu VM + Windows PC | Same classes; new production VM (legacy VMs terminated) | KEEP, reconfirm |

## 6. Proposals for approval

| # | Proposal | Recommendation |
|---|---|---|
| P-1 | Fix H-1 to H-4 in the V2 copy of the manual (clean markers and formatting, relabel section 9, add evidence period and VM termination). | Approve |
| P-2 | Record the legacy scope as "NSE equity, intraday-first, with positional/holdings handling that Rama judges unsatisfactory" (resolves X-1, X-2). | Approve |
| P-3 | Add the decision-criteria issues and the Master Agenda concerns to the KNOWN DEFECT list as user-reported, to be evidenced in section D (resolves X-3). | Approve |
| P-4 | V2 purpose statement (DR candidate): "An automated, headless, live-only trading system for NSE equities via Zerodha that consumes Chartink scanner signals and, through its configured controls and decision logic, validates, decides, executes, manages, reconciles and reports intraday and positional trades." | Approve or edit |
| P-5 | V2 scope (DR candidate): NSE equity only; Zerodha only; Chartink sole signal source; intraday and positional, with product types finalised at A-007. | Approve |
| P-6 | Per-signal-type processors replacing the centralised score. Keep PROPOSED; decide at D-017 / A-008, not at A-001. | Defer |
| P-7 | Operator boundary (DR candidate): operation is fully automated; the only manual actions are kill-switch release and operator interrupt. Resolves X-4. | Approve |
| P-8 | Environments (DR candidate): development/debug on Windows 11, production on an Ubuntu VM on Oracle (new instance), no hardcoded paths. Partly closes Q-05. | Approve |
| P-9 | Park for later topics: holdings Telegram summary (L-001/L-005), "prepare reports" under DR-009 (L-011), config-driven start/stop and NSE trading days (A-003, B), headless daily Zerodha token (A-010, M-005). | Approve |

## 7. Open items
- When were the two VMs terminated, and were the production database and logs backed up first? With the VMs gone, production runtime state can never be read, so legacy runtime claims rest on repo, records and preserved captures only.
- Dependencies: A-003 (hours), A-004 (instruments), A-007 (products), A-008 (strategy), A-010 (secrets, Q-11), L section.

## 8. Decision record (draft)
Agenda ID: A-001 | Topic: System purpose | Final decision: MODIFY | Status: PROPOSED | Date: 2026-10-05 | Approval: pending Rama

## 9. Disposition after the ChatGPT value-added review (2026-10-05)

ChatGPT's governing point is adopted: A-001 records legacy fact only. My proposals mixed legacy fact with V2 direction, so V2 items moved to V2_DESIGN_LOG.md section 3 (VP-01 to VP-07).

| My item | Outcome |
|---|---|
| H-1, H-2 | Approved. Applied in A-001_manual.md (V2 working copy); original untouched. |
| H-3 | Reworded: not established as a general-purpose portfolio platform; delivery/positional capability did exist. |
| H-4 | Recorded as evidence status (manual section 10), not a purpose decision. |
| H-5 | Deferred to section G. |
| X-1, X-2 | Resolved: intraday core, plus delivery/positional handling that Rama considers unsatisfactory. "Intraday-first" dropped. New evidence: the repo README itself calls the system an automated NSE intraday trading system. |
| X-3 | High-level defect list added to the manual (section 9); mechanics deferred to D. |
| X-4 | Resolved as unattended normal operation plus manual safety controls; the wording clash with project instruction 5.5 moved to VP-04. |
| X-5 | Recorded as historical environment and evidence status. |
| X-6 | Reporting stays a legacy responsibility; DR-009 stays a separate V2 decision. |
| P-1, P-3, P-9 | Approved. |
| P-2 | Approved in ChatGPT's wording. |
| P-4, P-5 | Not frozen in A-001: now VP-01, VP-02 (ratify at end of Section A). |
| P-6 | Deferred: VP-03. |
| P-7, P-8 | Not in the legacy manual: now VP-04, VP-05. |

Evidence period (ChatGPT Q5): README claims live since 11-May-2026; commits dated 17-18 Apr 2026 exist in the legacy record; exact first and last commit dates not retrieved (Q-14).
VM termination (ChatGPT Q6): about 1-2 weeks before 2026-10-05, exact date unknown; no backups taken.
