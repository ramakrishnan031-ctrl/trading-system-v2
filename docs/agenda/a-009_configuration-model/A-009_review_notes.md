# A-009 CONFIGURATION MODEL: REVIEW NOTES

Date: 2026-10-08
Status: CLOSED 2026-10-08 (final). ChatGPT's value-added review applied (section 12). Legacy manual FROZEN and recorded as DR-024 together with the A-008 erratum E-1; V2 inputs remain PROPOSED / OPEN. No question for Rama (Q-33 waits for the A-010 and A-011 answers). Erratum E-1 to the A-009 manual (sections 5 and 21: exit 5 restarted by the token watcher's policy) added 2026-10-09 during A-011 (A-011_review_notes.md section 6), reviewed and accepted by ChatGPT on 09-Oct-2026 and recorded in DR-026 together with the A-011 freeze. It corrects only those lines; the A-009 freeze (DR-024) is unchanged. Erratum E-2 to the A-009 manual (sections 3, 5, 6, 17 and 18: the usage tracker's coverage and its unwired read instrumentation; the readers of symbol_aliases.yaml and expected_managers.yaml) added 2026-10-12 during B-002 (B-002_review_notes.md section 6), revised 2026-10-13 after ChatGPT's value-added review, accepted in ChatGPT's final acceptance check and approved by Rama on 13-Oct-2026, and recorded in DR-037 together with the B-002 freeze. It corrects only those passages; the A-009 freeze (DR-024) is unchanged.
Place at: docs/agenda/a-009_configuration-model/A-009_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-009_manual.md; V2 inputs in A-009_v2_design_inputs.md and V2_DESIGN_LOG. External facts labelled per W-5.
Security: no credential values, mailbox addresses, account IDs, host addresses or key names. Source was read from raw files and a blob-less sparse clone of explicitly listed paths; .env files, accounts.csv, accounts_multi_example.csv, security.yaml, reference_data, docs/web_claude, databases and logs were not opened. A process incident with the first clone is disclosed in section 8.

## 1. Sources reviewed
- docs/inputs/chatgpt_originals_and_replies/A-009_CONFIGURATION_MODEL_DISCUSSION_CHECKLIST_29-SEP-2026.txt (Rama's answers to checklist Q1 to Q8 and the Part 2 file purposes; Q9 onward blank)
- docs/inputs/chatgpt_originals_and_replies/A-009_CONFIGURATION_MODEL_REMAINING_CLARIFICATIONS_30-SEP-2026.txt (Rama's answers Q1 to Q44 and the additional note)
- docs/inputs/reconciliation/A-009_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_30-SEP-2026.txt
- docs/inputs/manuals/A-009_CONFIGURATION_MODEL_LEGACY_MANUAL_30-SEP-2026.md
- Production source and configuration at 970aabf (list in A-009_manual.md section 2); the testing snapshot (branch main, 2026-10-08); the project records WHERE_EVERYTHING_IS_STORED_18-Sep-2026.txt and CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt; Zerodha's charges page and the Kite Connect margins documentation (checked 2026-10-08).

## 2. Overall
ChatGPT's 30-Sep set gets the architecture right: an 8-file strict AppConfig and a separate strategy loader, all-or-nothing loading, no hot reload, snapshots of the resolved configuration, the other configuration files in config/, production and testing differences, and change-governance risk. It keeps its UNKNOWNs honest. Direct source reading corrects five points (section 5), adds the failure behaviour, defaults, values held in code, broker values and a full production-versus-testing comparison, and found one defect in the frozen A-008 manual (erratum E-1, section 6). None of Rama's answers needs his confirmation.

## 3. Findings

| # | Finding | Evidence | Manual |
|---|---|---|---|
| F-1 | The eighth AppConfig file is nse_holidays_<current year>.yaml, chosen at import; only 2026 exists, so a 2027 start would fail to load (audit lead). The holiday guard ignores a missing file and load_all then fails. | config_loader.py _CONFIG_FILES; main.py; holiday_guard.py | 4 |
| F-2 | The repository's systemd unit runs main.py --mode live with no --config, so the service read config/ relative to its working directory; the installed unit is UNKNOWN. | deploy/systemd/trading-system.service; main.py | 4 |
| F-3 | An AppConfig error exits 5 and is not restarted (RestartPreventExitStatus=3 4 5); start-up check failures exit 3 (including the A4/A5 auditor re-run with strategies). Alerting differs by path: no main.py alert identified for exit 5 (the notifier is built later); no alert call on the A4/A5 guards; invalid strategy YAML has its own Telegram and email alert; the 08:30 pre-flight (09:20 report) re-runs the load. | main.py Phase 0b, 0d, 0e; unit file; pre-flight checks | 5 |
| F-4 | The S10 scanner-map cross-check never runs on the boot or pre-flight paths read: five callers, none passes scan_webhook_map_path. YAML 1.2.2 requires unique mapping keys, but the pinned parser (PyYAML 6.0.3) accepted a duplicate scanner key in a test and kept the last occurrence without error; the legacy path did not reject duplicates. The map header's "validated at startup" and "duplicate names: CRITICAL startup failure" are not enforced. At the baseline the 16 keys are unique and every mapped strategy has a YAML, so no legacy outcome changes. | strategies/loader.py; main.py; startup_checks.py; config_sanity.py; config_integrity.py; requirements.txt | 5; A-008 E-1 |
| F-5 | 23 SystemConfig fields and 15 scanner_type entries took schema defaults in production; the loader docstring says it does not fill defaults. | AST analysis of config_loader.py against the production files | 4, 8 |
| F-6 | The usage tracker only warned about unread keys (strict=False), covered AppConfig only, and CV2 (no constructor defaults) was not followed everywhere. | config_validator.py; main.py; signal_processor.py | 6 |
| F-7 | The auditor runs groups A, C and G at load and A to G at pre-flight; A1's message is stale (A-007 E-1). | config_auditor.py; config_sanity.py | 6 |
| F-8 | The change alert (CONFIG_DIFF) covered the 8 AppConfig files only; the session hashes were rewritten before the start-up checks, so a failed start could hide a change from the next alert (audit lead). | startup_checks.py; main.py | 11 |
| F-9 | Snapshots hold the resolved AppConfig with secret-looking values redacted; strategy files excluded; a comment-only edit changes the hash (NI-6). | config_snapshotter.py | 11 |
| F-10 | Values held in code include the gap window, GTT cap 50, trigger distance, quote failures, the start window, a second copy of the risk caps (5 and 10) in the pre-flight, and literal session values. | files named in the manual | 9 |
| F-11 | slippage_model.yaml's identified reader applied adverse slippage to synthesised paper fills only (wired after the 26-Apr CFG-6 audit, in paper mode; tiers from the F&O flag; "small" never assigned; the header's capital/cost_calculator.py does not exist). Live entry slippage was guarded by entry_gate (1% abort; slippage_control). | broker/slippage_engine.py; system_config.yaml | 13 |
| F-12 | Broker charges match Zerodha's current page for equity except the NSE transaction charge (0.00297% against 0.00307%); a "CNC SELL" brokerage comment does not align directly with the zero delivery rate, and the actual legacy treatment is UNKNOWN (calculator and contract notes not read; Zerodha classifies charges by resulting trades, FIFO); F&O rates differ (never traded). | broker_costs.yaml; zerodha.com/charges and the virtual contract note pages (2026-10-08) | 10 |
| F-13 | Paper-only settings: the paper section, telegram_alerts_in_paper_mode, slippage_model.yaml, paper_capital; basis of the caution note Rama asked for (Q22). | system_config.yaml; slippage_engine.py; account_registry.py | 13 |
| F-14 | Snapshot comparison: the other 7 AppConfig files, all 16 strategy YAMLs and three more files are byte-identical in production and testing; system_config.yaml has 18 differing leaf settings (start time 1, force_qty 1, two caps 2, sr_shadow 11, SMTP 3). Separate evidence layer (18-Sep record): the force_qty change never ran, and two testing config files were VM-local. | yaml diff; 18-Sep record sections 5, 8, 12.5 | 14 |
| F-15 | Deployment: a git push to a bare repository on the production VM, checked out by post-receive with no restart (the hook recorded armed and identical to the repository copy on 17-Jul-2026, re-armed by hand after edits); pre-receive (market-hours guard) not installed by default; pre-commit secret scan; a report-only nightly tree check. On 18-Sep the bare repository matched neither deployed tree. | deploy/hooks; system_manager.py; DEPLOYMENT.md; 18-Sep record | 15 |
| F-16 | accounts.csv holds environment-variable names per its reader; the direction registry holds direction, status, health and scanner evidence; the GUI's local config holds a password hash and a TOTP secret (record). | account_registry.py; strategy_direction_registry.yaml; 18-Sep record | 3, 16 |
| F-17 | Organisation: one 753-line master file mixing intraday, delivery, shadow and disabled features; an unreachable delivery cap kept for the "no-fallback contract"; the same 16 scanner URLs in two files, one of them headed "URLs are placeholders". | system_config.yaml; scan_webhook_map.yaml; chartink_scanners.yaml | 17 |
| F-18 | Q-19 answered: the start guard permits starts only in [08:00, 18:15) IST and exits 0 before logging and before loading configuration; the schema requires market_close <= service_window_end < 18:15 (production 17:35). | main.py; config_loader.py | 5 |
| F-19 | Configuration sources beyond YAML: the command line (mode defaults to paper), TS_IGNORE_MARKET_WINDOW, the systemd unit, .env, the GUI's local config, and two database records. | main.py; unit file; 18-Sep record | 3 |

## 4. Rama's answers against the source

| Question | Rama | Source (manual section) |
|---|---|---|
| Checklist Q1 architecture (29-Sep) | YES | Consistent (3, 4) |
| Checklist Q2, Q3 other sources | No | Corrected by Rama on 30-Sep (Q3); the source adds the command line, an environment variable, the unit, .env and the GUI config (3) |
| Checklist Q4 all files used | YES (guess) | All loaded at start; field-level use not established; slippage_model paper only (13) |
| Checklist Q5 files not loaded by config_loader | UNKNOWN | Yes: nine more files and a reference-data folder in config/, read by other modules or by none (3) |
| Checklist Q6 to Q8 (--config, change method, hot reload) | UNKNOWN | Answered by source and by his 30-Sep answers (4, 15) |
| Q1, Q2 files used, dormant files | Used in theory; none dormant | No reader identified in the inspected source for expected_managers.yaml; slippage_model's identified reader applies it in paper only (3, 13) |
| Q3 additional files | Listed | Consistent; accounts.csv and the registry descriptions sharpened (3) |
| Q4 --config | UNKNOWN | Repository unit: none; installed unit UNKNOWN (4) |
| Q5 change methods | All of them | Consistent (15) |
| Q6 hot reload | None | Consistent (4) |
| Q7 failure on missing or invalid files | No incidents | Exit 5, no restart, no alert from main.py (5); his V2 rule in V2 inputs section 2 |
| Q8 warnings at start | UNKNOWN | Warnings logged, start continued (5, 6) |
| Q9 observed rejection | NO | No record found |
| Q10 validation defects | UNKNOWN | Several found (19) |
| Q11, Q12 master and secondary | Master control panel | Consistent with the per-consumer relationships; no general rule (7) |
| Q13 broker values | From Zerodha's site; probably unchanged | Equity mostly matches; NSE transaction charge differs; CNC-sell treatment UNKNOWN; F&O differ (10) |
| Q14 hardcodes | "I Guess No" | Superseded: values held in code exist; 28-Apr-2026 record (9) |
| Q15, Q16 defaults and fallbacks | UNKNOWN | 23 defaults identified; trade impact UNKNOWN (8) |
| Q17, Q18 snapshots | UNKNOWN | Mechanism confirmed (11) |
| Q19 disk and running mismatch | YES | Supported by the 18-Sep record (15) |
| Q20, Q21 one file set for both modes | YES; no separate values | Consistent; some settings applied in one mode only (13) |
| Q22 a key behaving differently by mode | UNKNOWN | Yes, the paper-only settings; caution note added (13) |
| Q23 paper and live results differed | YES | Consistent with the paper-fill path (13) |
| Q24 to Q26 testing as a copy | YES, with changes | 18 differing leaf settings in system_config.yaml; S&R in the testing config is log-only; S&R targets and stops in testing code NOT ESTABLISHED (14) |
| Q27 testing-only files | NO | Files the same; testing-only settings exist (14) |
| Q28 to Q32 governance | As stated | Consistent; partial controls in source (15) |
| Q33 values computed at runtime | UNKNOWN (guess) | Documented in their topics (12) |
| Q34 nothing in SQLite | Guess | Superseded: config_snapshots and the session row (12) |
| Q35 broker values authoritative at runtime | UNKNOWN | A-006 (12) |
| Q36 to Q43, additional note | V2 inputs | A-009_v2_design_inputs.md sections 2 to 13 |
| Q44 freeze | CLOSED / FROZEN | His questionnaire disposition; the documentation freeze is DR-024 (section 5, point 8) |

## 5. ChatGPT's 30-Sep set: corrections and additions
1. The eighth file is listed as nse_holidays_2026.yaml; it is chosen by the current year (F-1).
2. "Historical explicit --config usage: UNKNOWN": resolved for the repository unit (F-2).
3. The trace matrix labels every consumer FACT, but several were not read: broker_costs' calculator was not read; no reader was identified in the inspected source for expected_managers.yaml; slippage_model.yaml's identified reader feeds paper fills only, not a "paper/cost path" (F-11). The manual's section 3 now names only readers identified in source.
4. The clarification file's repository finding 5, "scan_webhook_map.yaml ... is startup-validated against strategy YAMLs", is not true of the boot path (F-4; A-008 erratum E-1).
5. "The core loader does not fill missing required keys" is true for required keys only; 23 optional fields defaulted silently (F-5).
6. Added from source and records: failure exits and the restart policy, the auditor groups and usage tracker, the change alert and its gap, values held in code, broker values against current charges, the full production-versus-testing comparison, deployment and drift controls, organisation findings and the 18-Sep record (F-3, F-6 to F-19).
7. V2 material moved to the V2 inputs file (W-1): manual section 17, the "Future-only requirement" in section 8, "V2 input: no Paper mode" in section 11 and the FUTURE line in section 18.
8. Status: Rama marked A-009 CLOSED / FROZEN in Q44 on 30-Sep. This was his questionnaire disposition, not the final documentation freeze record. The permanent A-009 freeze occurs after Claude applies ChatGPT's review, under DR-024. (Wording from ChatGPT's 08-Oct review, correction 1; Q44 itself is not altered.)

## 6. Correction to a frozen manual: A-008 erratum E-1
- A-008_manual.md (FROZEN, DR-023) said the scanner map is "cross-validated (S10: every mapped strategy must exist)" at load and that "Duplicate scanner names -> CRITICAL startup failure"; its review notes (F-5, Q14 row) and its Changes list said the same. Those lines were taken from the loader function and the map file's header, not from the boot call path.
- Source: the boot and pre-flight callers never pass the map path, the schema has no duplicate or existence check, and the pinned parser keeps the last of two duplicate keys without error, although YAML 1.2.2 requires unique mapping keys (F-4).
- Impact: none on legacy outcomes; at the baseline the 16 scanner keys are unique and every mapped strategy has a YAML. Only the description of the safeguard changes. The unknown-scanner 404 statement stays correct.
- Applied: the original lines stay visible, struck through, with an "## Erratum (2026-10-08, found during A-009)" section in A-008_manual.md and matching edits in A-008_review_notes.md. Accepted by ChatGPT on 08-Oct-2026 (its accepted point 9) and recorded in DR-024 (as A-006 E-1 went into DR-022 and A-007 E-1 into DR-023).
- Lesson added (V2_DESIGN_LOG, 2026-10-08): reading a function is not reading its call path.

## 7. Points for Rama
- None blocking. Rama's three "note in the Manual" requests (Q13, Q14, Q22) are handled in two layers: the legacy facts are in the manual (sections 9, 10, 13, each marked "NOTE REQUESTED BY RAMA"), and his new-system rules are in A-009_v2_design_inputs.md sections 5, 6 and 11.
- Q-33 (same-symbol rule): the A-009 answers were checked on 2026-10-08 and do not address it; the A-010 and A-011 answers remain to check before asking.
- Q11 against A-008 Q26.2 (where caps live): settled by his later A-005 Q-22 (06-Oct): maximums at both levels, the lower binds. No question needed (Q-32 lesson).

## 8. Process note: partial-clone incident (2026-10-08)
- A tree-wide `git grep HEAD` run on the blob-less partial clone lazily fetched blobs into the local object store, including four paths on the never-open list (.env.example, config/accounts.csv, config/accounts_multi_example.csv, config/security.yaml). None was displayed, printed or opened; no docs/web_claude blob was fetched (0 of 80). The clone was deleted and re-created with an explicit no-cone sparse path list, and a check confirmed that no blob of those paths is present.
- Rule added (V2_DESIGN_LOG lesson, 2026-10-08): no tree-wide or history git command on the legacy partial clone; search only checked-out paths with local tools.

## 9. Cross-topic evidence found while verifying A-009
- Q-19 (A-003, B-001): answered by F-18; proposed CLOSED.
- A-008 section 20: the testing strategy YAMLs are identical to production (the frozen text is not changed; noted in manual section 14).
- A-007: the auditor's A1 message still describes the pre-10-Jul load-time rewrite (consistent with A-007 E-1).
- A-006: the quote rate was raised from 1 to 3 per second in April 2026 (DEPLOYMENT.md H-24, and the file's own comment); broker_limits.yaml is identical in production and testing (orders 8/s, quotes 3/s, historical 2/s, margins 8/s).
- A-005: delivery_max_position_value_pct is commented "MEASURED UNREACHABLE" while the 70/30 split holds.
- A-010: environment-variable names in config; WEBHOOK_SECRET and require_hmac false; plain-text SMTP addresses; the GUI's local config; the pre-commit secret scan (C-1, 02-Jul-2026); accounts.csv schema.
- A-011: exit codes and restart policy; holiday and start-window exits; the end-of-day self-exit stays alive while positions remain (21-Sep record); session-row literals.
- Q-20: the symbol_aliases.yaml reader was not traced.

## 10. V2 items (to V2_DESIGN_LOG, PROPOSED)
- VP-46 (change control and evidence), VP-47 (boot gate), VP-48 (configuration structure, cleanliness and no hardcodes), VP-49 (broker values from official sources with check dates); VP-15, VP-32, VP-42 and VP-45 extended.
- Q-35 opened (A-009 audit leads); Q-33 annotated; Q-19 proposed CLOSED.
- A-009 was a ratification point for VP-13, VP-15, VP-32, VP-42, VP-43 and VP-45: none ratified here. At the freeze (DR-024) their configuration parts moved to B-002 and the configuration design step in the build phase, as proposed in A-009_v2_design_inputs.md section 16 and not objected to in ChatGPT's review; scheduling only, no V2 decision.

## 11. Next step (first pass)
Send this pass to ChatGPT for its value-added review. Then apply it, mark the manual FROZEN, add DR-024 (A-009 freeze with A-008 erratum E-1) and give the save list and push commands. Then A-010. Done: section 12.

## 12. Disposition after ChatGPT's review (08-Oct-2026)

ChatGPT's review (docs/agenda/a-009_configuration-model/A-009_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_08-OCT-2026.txt) accepted the A-009 findings as evidence-disciplined (its section 2, points 1 to 19), including the A-008 erratum E-1, the process-incident disclosure and the two-layer handling of Rama's three manual notes. It reversed no core finding, left no question for Rama and asked for ten precision and governance corrections before the freeze. All applied:

| ChatGPT correction | Outcome |
|---|---|
| 1. DR number and freeze status | Section 5 point 8 uses ChatGPT's wording: Q44 was Rama's questionnaire disposition; the permanent freeze is DR-024. Q44 is unchanged. Manual status, these notes, the V2 inputs and the design-log agenda row all say A-009 = FROZEN, DR-024. |
| 2. "18 differences" wording | "18 differing leaf settings in system_config.yaml" with the breakdown (1 + 1 + 2 + 11 + 3) in manual sections 14, 19 and 21, F-14, the answers table, V2 inputs section 12 and the design log; the 18-Sep record kept as a separate evidence layer. |
| 3. Source versus consumer | "Reader identified", "no reader identified in the inspected source" and "not traced" used for expected_managers.yaml, symbol_aliases.yaml, index_universe.yaml, security.yaml, broker_costs.yaml (calculator not read), the slippage model and the paper settings (manual sections 3, 13, 17; F-11; section 5 point 3; V2 inputs section 13). No "unused", "inert" or "has no effect" claim remains for a path not fully inspected. |
| 4. CNC SELL wording | ChatGPT's wording adopted in manual section 10: the CNC SELL brokerage statement does not align directly with the current zero delivery rate; the actual legacy treatment is UNKNOWN (calculator not inspected; no contract notes here). Also in sections 18, 19 and 21, F-12, the Q13 row, V2 inputs section 6, VP-49 and Q-35 (c). |
| 5. Broker API "authority" | V2 inputs section 6 and VP-49: /margins/orders and /charges/orders are broker calculation APIs for the requested orders (/margins/orders also considers existing positions and open orders); broker state endpoints give live account state. PROPOSED only. |
| 6. YAML duplicate keys | Three statements kept apart in manual section 5, the A-008 erratum and V2 inputs section 15: YAML 1.2.2 requires unique keys (FACT); the pinned PyYAML 6.0.3 accepted a duplicate and kept the last occurrence (FACT, tested); the legacy path did not reject duplicates (KNOWN DEFECT). Neither "YAML allows duplicate keys" nor "PyYAML is wrong" is written. |
| 7. Configuration error alerting | Manual section 5 lists alerting per path: exit 5 (no main.py alert identified; notifier built later), A4/A5 exit 3 (no alert call), invalid strategy YAML (own Telegram and email alert), scheduled pre-flight (reports later). Sections 19 and 21, F-3, V2 inputs section 2 and VP-47 match. |
| 8. Current external broker facts | Zerodha's charges page, its virtual contract note pages (trade-based FIFO classification: Z-Connect 3-Jul-2023 and the support article) and the Kite margins documentation are labelled CURRENT EXTERNAL BROKER FACT with the 2026-10-08 check date and stated not to prove legacy charges (manual section 10; V2 inputs sections 6 and 15). |
| 9. Final status | A-009_manual.md FROZEN (revision 1, DR-024); these notes CLOSED; A-009_v2_design_inputs.md PROPOSED / OPEN; V2_DESIGN_LOG synchronised; A-008 manual and review notes carry erratum E-1, recorded in DR-024. No V2 decision is ratified. |
| 10. Save list and register | Save list as ChatGPT gave it, plus this review file (topic folder, W-3) and V2_DECISION_REGISTER.md with DR-024. No duplicate copies in other folders. |

Also applied: the process incident stays a tooling incident, not a credential exposure (ChatGPT section 13). Q-33 stays OPEN until the A-010 and A-011 answers are checked (its sections 1 and 15). The ratification points of VP-13, VP-15, VP-32, VP-42, VP-43 and VP-45 moved from A-009 to B-002 and the configuration design step, as proposed in V2 inputs section 16 and not objected to (scheduling only, no V2 decision). Two lessons added to V2_DESIGN_LOG.

Result: A-009 legacy manual FROZEN (revision 1, DR-024); these notes CLOSED; A-009_v2_design_inputs.md PROPOSED / OPEN. Register: one row, DR-024, is the A-009 freeze record (made without a separate approval, as DR-017 allows) and also records the A-008 erratum E-1. No V2 decision is entered. Next: A-010, after the A-009 files are stored and pushed.
