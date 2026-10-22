# C-001 CHARTINK STRATEGY / SCANNER: LEGACY MANUAL

Status: FROZEN 2026-10-21 (revision 0, DR-053). Revision 0 (Claude, 21-Oct-2026) was drafted from Claude's research files, ChatGPT's value-added review and Rama's answers; ChatGPT's independent review of revision 0 (21-Oct-2026) found no blocker and asked for no change to this manual. Rama approved the freeze on 21-Oct-2026 ("Freeze C-001 now (Recommended)") and accepted errata A-008 E-4 and B-002 E-2 ("Accept both (Recommended)"), recorded in DR-053 (section 17). The freeze changed only the status and date lines, the errata sentence of section 6.3, section 17's status and section 20's last item. Errata E-1 (sections 3, 6.6, 7.1 and 9.2: the request audit's coverage) and E-2 (section 18: the status of errata A-008 E-4 and B-002 E-2) to this manual added 2026-10-21 during C-002, accepted in principle in ChatGPT's value-added finalisation review of C-002 (22-Oct-2026), approved by Rama on 22-Oct-2026, each on its own, and recorded in DR-055 (2026-10-22) together with the C-002 freeze.
Date: 2026-10-21 (revision 0; frozen the same day, DR-053)
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1). Rama's input of 21-Oct-2026 applies here as a boundary: the legacy strategies are to be ignored for V2 (C-001_v2_design_inputs.md section 1). The 16 legacy definitions below are evidence, not candidates. No VM, service, host, broker, webhook or Chartink operation and no change to legacy source; the legacy system is DEAD / REFERENCE ONLY.
Built from: Claude's repository answers and source inventory of 21-Oct-2026 (research inputs, stored unchanged at the freeze), ChatGPT's value-added review of 21-Oct-2026, and Rama's answers of 21-Oct-2026 (section 9.2). ChatGPT prepared no reconciliation or manual draft for C-001; Rama chose on 21-Oct that Claude drafts revision 0 (his answer: "Claude drafts revision 0 (Recommended)"). Where this manual and the answers file differ, C-001_reconciliation_addendum.md item 2 gives the correction.
Labels: FACT (SOURCE-VERIFIED) = trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb ("the baseline", dated 2026-09-11), read on 2026-10-21 through the masking filter; "path lines A to B" are line numbers there. FACT (RECORD-DERIVED) = a dated record in the baseline repository (an audit note, a decision record, a guide, or a code or configuration comment recording a measurement or event), true for its own date and scope only; the legacy database and logs no longer exist, so none can be re-measured. HISTORICAL (Rama) = his answers of 21-Oct-2026. INTERPRETATION / AUDIT LEAD = a reasoned reading of source or records, not observed. KNOWN DEFECT / AMBIGUITY = a defect or ambiguity visible in the source. KNOWN DOC DRIFT = a comment, docstring, header, record or document that the code contradicts (the code wins). UNKNOWN / NOT ESTABLISHED. Frozen V2 manuals are cross-referenced by section ("A-011 section 6") and are not reopened, except through the errata of section 17.
Search scopes behind negative statements (answers file section 0.3): S-ALL = the 1,262 files checked out (every tree path except the never-open paths, images, ignore and keep files, the reference-data test fixtures and one file whose blob a never-open path shares); S-PY = its 734 .py files; S-RT = the 281 .py files outside tests; S-CODE = its 869 code and configuration files. A negative reads "not found in <scope>"; a pattern search cannot exclude behaviour reached by dynamic names, external processes or installed files that differ from the repository.
Security: no credential, token, account or client identifier, host or address, mailbox, or person name other than Rama's is reproduced. Webhook authentication is described by mechanism only. The screener URLs are described by their form (section 4.1), not copied.

## 1. Purpose and boundary
C-001 records, at the baseline:
- which Chartink scanner definitions the legacy system held, where, and how each named a strategy;
- how the two scanner files were loaded and validated, and which claimed checks did not run;
- what the system fetched from Chartink (connectivity checks only) and what those checks proved;
- how the scanner key was used at the webhook edge and resolved to a strategy (the C-001 end of the signal path);
- how scanner activity was attributed on the dashboard, and what dated records say arrived.

Out of scope, cross-referenced only: Chartink alert arming, the webhook URL and delivery (C-002); authentication (C-003); payload and symbol validation (C-004); deduplication (C-005); queue, persistence, processing and expiry (C-006 to C-009); the scanner-to-strategy mapping design (C-010); EOD and daily routing (C-011); start-up check classes, the pre-flight and the dashboard's readiness views (B-007, frozen); the strategy model and the Chartink conditions (A-008, frozen); configuration (A-009 and B-002, frozen). Q-20 evidence is recorded in section 15 without deciding it.

## 2. Evidence
| Evidence | What it covers | Limit |
|---|---|---|
| Baseline source and configuration | 1,262 files checked out (S-ALL); the 33-row inventory built from imports, callers and searches (answers file section 1); files read, ranges and blob IDs in the source inventory | Source is not proof of what was installed or ran on the VM on any date (U-04) |
| Records in the production repository | The integrity audit (measured 31-Jul-2026, data from 12-Jun), the audit of 05-Jul, the BK-1 analysis of 17-Jul, the C2 investigation of 03-Jul, the locked-decision record, the configuration guide, the assumptions record and dated configuration comments; read by narrow masked searches (section 9.1) | Each true for its own date and deployed code |
| Frozen V2 manuals, read at V2 main 5d575d1 on 21-Oct | A-008, A-009, A-011, B-002 and B-007 for the statements this topic touches (sections 6.3 and 17) | Not reopened except through section 17 |
| Rama's answers of 21-Oct-2026 | The two range-breakout alerts, the webhook URL form, the operator check (section 9.2) | His account. The Chartink account was not inspected: his choice of 21-Oct, "No browser; record UNKNOWN (Recommended)" |
| Not inspected | The testing snapshot and the records repository (Rama, 20-Oct-2026); Chartink itself (no live contact) | - |

## 3. Terms
The legacy code and records use some of these words loosely; this manual uses each in one sense.

| Term | Meaning here |
|---|---|
| Scanner definition | An entry under "scanners" in config/scan_webhook_map.yaml (the map), with its twin entry in config/chartink_scanners.yaml (the twin file) |
| Scanner key | The YAML key of a definition; at runtime the path segment of POST /webhook/<key> |
| Chartink alert | The screener and its alert on Chartink's side; not in the repository |
| Alert name | The optional scan_name field of a webhook body |
| Connectivity check | An HTTP GET of a configured screener URL; it says nothing about scan results, alert arming or delivery |
| Webhook delivery | A POST that reached the receiver; ~~one webhook_audit row whatever the response~~ (superseded by erratum E-1) one webhook_audit row whatever the response, when the request reaches the handler, its body is read and the insert succeeds (C-002_manual.md section 10.3) |
| Webhook acceptance | A per-symbol ACCEPTED: a signals row inserted as QUEUED and its tuple queued |
| Signal processing | SignalProcessor._process_one and the stages after it (C-008) |
| Strategy | A YAML file in config/strategies/, loaded by its name |

## 4. Scanner definitions and the strategy catalogue (FACT, SOURCE-VERIFIED unless marked)

### 4.1 The two files
- The map holds one mapping, "scanners", with 16 entries, each with strategy and chartink_url; only pb01_breakout_retest also sets scanner_type: eod. Evidence: config/scan_webhook_map.yaml lines 16 to 84; config/scan_webhook_map.yaml line 84.
- The twin file holds one mapping, "scanners", with the same 16 keys, each "key: URL". Evidence: config/chartink_scanners.yaml lines 15 to 32. Its header says "URLs are placeholders" (config/chartink_scanners.yaml lines 12 to 13) and describes a HEAD check (KNOWN DOC DRIFT, D-03).
- URL form (computed from the masked contents of both files): each of the 16 URLs is https://chartink.com/screener/ followed by the key with "_" replaced by "-"; for each key both files hold the identical string; no URL has a query string, token or numeric screener ID. Whether any of them was a live Chartink screener is UNKNOWN (U-02).
- No Chartink scan conditions were found in S-CODE: there the chartink.com literal occurs only in the two files (16 lines each), one dashboard docstring, the load-test payload builder and test or crash-test tools; the never-open paths were not searched. The conditions recorded in A-008 section 17 (frozen) came from Rama's supplied file (DR-023).
- A registration convention, not a runtime requirement: A-008 section 8 records from a 17-Jul investigation that a scanner needs three entries (its strategy YAML, a map entry and a twin-file entry), and a test asserts PB-01's presence in both files (tests/unit/test_pb01_registration.py lines 47 to 49). At the baseline the receiver and the processor route by the map entry and the strategy YAML, and the start-up check fetches the map entry's URL (sections 6 and 7). The twin file is loaded, hashed and checked for presence on the start path, but its URLs had no effect identified in the inspected path (INTERPRETATION / AUDIT LEAD, L-01; section 6.2).

### 4.2 Catalogue
Direction, intent and enabled are the strategy YAML's fields; the type is the map's scanner_type. The arrivals column is FACT (RECORD-DERIVED) for 12-Jun to 31-Jul-2026 (section 9.1).

| # | Scanner key (= strategy name) | Type | Direction | Intent | Enabled | Arrivals, 12-Jun to 31-Jul-2026 |
|---|---|---|---|---|---|---|
| 1 | open_low_breakout_long | intraday (default) | LONG | INTRADAY | true | Posted (R-04) |
| 2 | first_pullback_long | intraday (default) | LONG | INTRADAY | true | Posted (R-04) |
| 3 | vwap_bounce_long | intraday (default) | LONG | INTRADAY | true | Posted (R-04) |
| 4 | gap_go_long | intraday (default) | LONG | INTRADAY | true | Posted (R-04) |
| 5 | gap_fade_long | intraday (default) | LONG | INTRADAY | true | Posted (R-04) |
| 6 | range_breakout_long | intraday (default) | LONG | INTRADAY | true | None (R-01) |
| 7 | open_high_breakdown_short | intraday (default) | SHORT | INTRADAY | true | Posted (R-04) |
| 8 | first_pullback_short | intraday (default) | SHORT | INTRADAY | true | Posted (R-04) |
| 9 | vwap_rejection_short | intraday (default) | SHORT | INTRADAY | true | Posted (R-04) |
| 10 | gap_go_short | intraday (default) | SHORT | INTRADAY | true | Posted (R-04) |
| 11 | gap_fade_short | intraday (default) | SHORT | INTRADAY | true | Posted (R-04) |
| 12 | range_breakout_short | intraday (default) | SHORT | INTRADAY | true | None (R-01) |
| 13 | positional_momentum_long | intraday (default) | LONG | DELIVERY | true | Posted; the strategy-control rejections of 24 to 31-Jul were these three scanners' (R-03) |
| 14 | positional_sector_rotation | intraday (default) | LONG | DELIVERY | true | Posted; the strategy-control rejections of 24 to 31-Jul were these three scanners' (R-03) |
| 15 | positional_swing_long | intraday (default) | LONG | DELIVERY | true | Posted; the strategy-control rejections of 24 to 31-Jul were these three scanners' (R-03) |
| 16 | pb01_breakout_retest | eod (explicit) | LONG | INTRADAY | false (v3_playbook true) | 5 POSTs, 27 to 31-Jul (R-02) |

- Evidence: config/scan_webhook_map.yaml lines 17 to 84 (entries); config/chartink_scanners.yaml lines 16 to 32 (twins); config/strategy_direction_registry.yaml lines 26 to 154 (registry); config/strategies/pb01_breakout_retest.yaml lines 24 to 30 and 67 (PB-01's identity and v3_playbook). The identity-field ranges of the other 15 YAMLs are listed per entry in the answers file, section 2.2.
- Totals: 10 LONG and 6 SHORT; 13 INTRADAY (12 enabled, PB-01 disabled) and 3 DELIVERY (all enabled). The same as A-008 section 3 (frozen).
- The type is a routing type (ScannerEntry.scanner_type), not the product intent: entries 13 to 15 are DELIVERY strategies on intraday-type scanners. The default is set at core/config_loader.py lines 2352 to 2357.

### 4.3 Key, strategy, webhook path, direction and alert name
- The key equals the strategy value in all 16 entries, and the 16 strategy values are distinct. The schema allows several scanners to name one strategy (N:1): the map is a dict of ScannerEntry with no uniqueness rule on strategy values. Evidence: config/scan_webhook_map.yaml lines 17 to 84; core/config_loader.py lines 2347 to 2377.
- The webhook identifier is the path segment. The route is POST /webhook/<scanner_name>, and the segment must be a map key or the request gets HTTP 404; no other webhook identifier was found in S-ALL. Evidence: signals/webhook_receiver.py lines 406 to 408; signals/webhook_receiver.py lines 551 to 554. The map's comment says the key must equal the path "Rama pastes into Chartink" (config/scan_webhook_map.yaml lines 77 to 80). HISTORICAL (Rama, 21-Oct), as read in section 9.2: the webhook URLs had one form, differing only in the strategy name.
- The webhook carries no side. The receiver reads only stocks, trigger_prices, triggered_at and the optional scan_name; direction is taken at processing from the mapped strategy. Evidence: signals/webhook_receiver.py lines 608 to 674; signals/signal_processor.py lines 933 to 953; signals/signal_processor.py lines 997 to 1004. The same as A-008 section 5 (frozen).
- Names and directions agree. The suffix of each of the 14 names ending in _long or _short matches its declared direction; positional_sector_rotation and pb01_breakout_retest carry no suffix (both LONG). The direction registry equals the YAML direction for all 16; its header says it is never read to place a trade, and in S-PY its seed is read only through core/strategy_direction.py, by the registry officer and two tests, while the EOD square-off's direction map reads the strategy YAMLs. Evidence: config/strategy_direction_registry.yaml lines 26 to 154; config/strategy_direction_registry.yaml lines 6 to 10; core/strategy_direction.py lines 43 to 52; orders/eod_squareoff.py lines 695 to 696.
- The alert name is an optional cross-check. If scan_name is present it is lower-cased, spaces become "_", a leading "<primary account tag>-" or "<tag>_" is removed, and the rest must equal the path segment, else HTTP 400; without scan_name the check is skipped. The tag is the primary account_id from the accounts file, cached, with the fallback "UNKNOWN", which disables the strip. No other character (a hyphen, for example) is normalised (D-07). Evidence: signals/webhook_receiver.py lines 617 to 633; signals/webhook_receiver.py lines 111 to 138; core/account_registry.py lines 273 to 299.
- The receiver's dated comment (09-Sep-2026) says production alerts carried no account prefix and testing-VM alerts did (signals/webhook_receiver.py lines 115 to 123); A-008 section 9 (frozen) records Rama's matching account. Production alert names themselves: UNKNOWN (U-01).

### 4.4 Enabled, missing, duplicated and unused
- Enabled: 15 strategy YAMLs have enabled: true; pb01_breakout_retest has enabled: false and v3_playbook: true. A scanner definition has no enabled flag of its own. Evidence: config/strategies/pb01_breakout_retest.yaml lines 24 to 30; config/strategies/pb01_breakout_retest.yaml line 67. "enabled" is one gate; the strategy-control gate follows (signals/signal_processor.py lines 955 to 972; strategies/control.py not read). The three DELIVERY strategies were dormant at that gate while force_intraday_only was true (A-007 erratum E-1, DR-023); R-03 measures it for 24 to 31-Jul.
- Missing: none at the baseline (computed): every map entry names an existing strategy YAML (16 of 16), every YAML is named by a map entry, the two files hold the same 16 keys and the registry the same 16 names. Nothing on the start path enforces this (section 5.3).
- Duplicated: none at the baseline (computed): 16 key lines and 16 distinct names in each file, and the 16 strategy names equal their file stems. Nothing on the load path detects a duplicate key (section 5.2).
- Scanner-condition duplicates and subsets: recorded in A-008 section 17 (frozen) from Rama's supplied conditions (VP-08); the repository cannot confirm them (4.1).
- No effect identified in the inspected path: the twin file's URLs (L-01; section 6.2), and the setting watchlist.playbook_scanner, which has no reader in S-ALL other than two records, while its schema comment says it "binds the eod route"; the EOD binding is made by scanner_type (KNOWN DEFECT / AMBIGUITY and KNOWN DOC DRIFT, D-04). Evidence: config/system_config.yaml line 621; core/config_loader.py lines 1533 to 1535; signals/webhook_receiver.py lines 562 to 564.
- Two keys with no recorded arrival: R-01 (section 9).

### 4.5 Counts and their levels
| Level | Count | Period | Label | Evidence |
|---|---|---|---|---|
| Scanner definitions (map keys) | 16: 15 intraday-type by default, 1 eod | Baseline | FACT (SOURCE-VERIFIED) | config/scan_webhook_map.yaml lines 16 to 84 |
| Twin-file keys | 16, the same keys | Baseline | FACT (SOURCE-VERIFIED) | config/chartink_scanners.yaml lines 15 to 32 |
| Strategy YAMLs | 16; 15 enabled; 12 enabled INTRADAY | Baseline | FACT (SOURCE-VERIFIED) | Section 4.2 |
| "currently-tradeable population" | 12 strategies | 24 to 31-Jul-2026 | FACT (RECORD-DERIVED) | docs/audit/integrity_audit_2026.md lines 89 to 97 |
| Keys that posted | 14 of 16: 13 intraday-type keys, two of them from 15-Jun and the rest from 12-Jun, plus PB-01 from 27-Jul | 12-Jun to 31-Jul-2026 | FACT (RECORD-DERIVED) | docs/audit/integrity_audit_2026.md lines 84 to 99 |
| Alerts configured or active on Chartink | Not in the repository; Rama reports both range-breakout alerts configured (9.2) | - | UNKNOWN; HISTORICAL (Rama) for the two | - |

No single "active production scanner count" is established: each count names its level and period. Arrivals after 31-Jul-2026 up to the baseline: UNKNOWN (U-06).

## 5. Loading and validation (FACT, SOURCE-VERIFIED unless marked)

### 5.1 Load path, consumers and reload
- main._main_locked loads the 8 AppConfig files at Phase 0b through load_all, which walks _CONFIG_FILES (both scanner files among them): it reads the raw bytes, records their SHA-256, parses with yaml.safe_load and validates with each file's schema class. A failure raises ConfigMissingError or ConfigSchemaError; main logs CRITICAL and returns 5. Evidence: main.py lines 2196 to 2202; core/config_loader.py lines 2457 to 2516; core/config_loader.py lines 2441 to 2450; core/config_loader.py lines 2420 to 2434. The whole start order is in A-011 section 5, its configuration steps in A-009 section 5 and the exit of each configuration failure in B-002 sections 5 and 6 (frozen); A-009 and A-011 record the exit-5 supervision.
- In the trading process the loaded scanner sections go to run_all_startup_checks (the map's entries), to WebhookReceiver (the whole AppConfig) and to SignalProcessor (the map's scanners dict, which the processor copies at construction). In S-RT three files access the AppConfig scanner sections. Evidence: main.py lines 2458 to 2460; main.py lines 3407 to 3415; main.py line 3585; signals/signal_processor.py lines 207 to 208; scripts/preflight_scanner_check.py lines 208 to 214; signals/webhook_receiver.py line 552.
- Once per start: no reload path for the scanner files was found in S-RT. A change takes effect at the next start, where the hash diff reports it (a CONFIG_DIFF system event and a WARN alert naming the changed files; not blocking). Evidence: main.py lines 2295 to 2296; main.py lines 2656 to 2675; utils/startup_checks.py lines 581 to 634; strategies/loader.py lines 91 to 95 (strategies "loaded ONCE; no hot reload").
- The dashboard reads the map file itself, on each call, from its own configured config_dir, with yaml.safe_load and no schema; a missing or unparsable file gives an empty result without an error. Evidence: ops_dashboard/backend/readers/config_reader.py lines 92 to 115; ops_dashboard/backend/readers/config_reader.py lines 249 to 287. The repository's dashboard configuration points config_dir at a Windows development path (ops_dashboard/backend/config/gui_config.yaml lines 8 to 13); the production value sits in a never-open local override file, so which file the production dashboard read is UNKNOWN.
- INTERPRETATION / AUDIT LEAD (L-02): because the dashboard re-reads the file and the trading process does not, an edit during the day could make the dashboard describe a map the running process was not using. Not observed.

### 5.2 Schema and failure behaviour
| Input or case | Result | Evidence |
|---|---|---|
| ScannerEntry | strategy: str, required; chartink_url: str, required, must start "http://" or "https://"; scanner_type: default "intraday", allowed "intraday" or "eod"; extra keys forbidden | core/config_loader.py lines 2347 to 2371 |
| ScanWebhookMapConfig | scanners: a dict of ScannerEntry; extra keys forbidden; an empty dict is valid | core/config_loader.py lines 2374 to 2377; tests/unit/test_config_loader.py lines 594 to 602 |
| ChartinkScannersConfig | scanners: a dict of str to str; its URLs are not validated | core/config_loader.py lines 2385 to 2388 |
| Missing key, wrong type, bad URL scheme or bad scanner_type | ValidationError raised as ConfigSchemaError; exit 5 | core/config_loader.py lines 2506 to 2514; main.py lines 2197 to 2201 |
| Missing file | ConfigMissingError, exit 5, before the presence check, which therefore cannot find a scanner file missing at a start (B-007 section 5, row C1) | core/config_loader.py lines 2483 to 2488; utils/startup_checks.py lines 833 to 872 |
| YAML syntax error | ConfigSchemaError, exit 5 | core/config_loader.py lines 2493 to 2500 |
| The same key twice | Not detected: yaml.safe_load (pinned PyYAML 6.0.3) keeps the last of two duplicate keys without error (A-009 section 5, frozen; A-008 erratum E-1, DR-024) | core/config_loader.py lines 2490 to 2494 |
| Map keys against twin-file keys | No comparison found in S-RT | utils/startup_checks.py lines 680 to 688 (the only place both are read together) |
| A map strategy with no YAML | The start succeeds; each of its signals is later rejected as REJECTED_UNKNOWN_STRATEGY | signals/signal_processor.py lines 948 to 953 |
| A strategy YAML with no scanner | No check | strategies/loader.py lines 194 to 213 |
| Several scanners on one strategy | Allowed by the schema; none at the baseline | core/config_loader.py lines 2347 to 2377 |
| Key normalisation | None: an exact, case-sensitive path match | signals/webhook_receiver.py lines 551 to 554 |
| Defaults applied at the baseline | scanner_type on 15 of the 16 entries (A-009 section 8, frozen) | config/scan_webhook_map.yaml line 84 (the only explicit one) |

### 5.3 The S10 cross-check did not run (KNOWN DEFECT / AMBIGUITY, D-01)
- strategies/loader.py checks the map against the strategies (every mapped strategy must have a YAML) only when it is given scan_webhook_map_path; even then it checks in one direction only and has no duplicate check. Evidence: strategies/loader.py lines 151 to 153; strategies/loader.py lines 169 to 213.
- Every runtime call omits the path (S-RT): main.py lines 3157 to 3162; utils/startup_checks.py lines 1283 to 1284; scripts/preflight/checks/config_integrity.py lines 155 and 173; scripts/preflight/checks/config_sanity.py lines 37 to 40; ops/control_tower/aggregator.py lines 156 to 158; scripts/replay_signals.py lines 187 to 188. The mention at signals/signal_processor.py lines 142 to 143 is a docstring example. Only tests pass the path: tests/unit/test_strategies.py lines 357 to 368; tests/unit/test_pb01_registration.py lines 31 to 36.
- This confirms A-008 erratum E-1 and A-009 section 5 (frozen; DR-024) at the same baseline and adds two callers to those read there (the control-tower aggregator and the replay tool). The records of 05-Jul and 31-Jul say the same (R-05, R-06).
- KNOWN DOC DRIFT (D-02). The map header says the map is "Validated at startup (P17, S10)" and that duplicate scanner names cause a "CRITICAL startup failure" (config/scan_webhook_map.yaml lines 3 to 6); neither holds. Three documents and a code comment repeat the start-up validation claim: the configuration guide, for a missing strategy ("startup fail CRITICAL"; docs/CONFIG_GUIDE.md lines 648 to 653); a dashboard design note, for duplicate names (ops_dashboard/docs/G2b1_strategy_attribution.md line 9); a later section of the integrity audit (docs/audit/integrity_audit_2026.md line 4374), against that record's own finding (docs/audit/integrity_audit_2026.md lines 56 to 63); and the registry officer's comment "S10 boot-check owns the config gap" (scripts/strategy_registry_officer.py lines 126 to 136).

## 6. Screener URLs and the connectivity checks (FACT, SOURCE-VERIFIED unless marked)

### 6.1 Declared, fetched and displayed
- The URLs are declared twice (4.1). They are fetched only by check_scanner_connectivity, which run_all_startup_checks calls at start-up and the operator script calls on demand; the dashboard displays the map's URLs and never fetches them. No code that pulls Chartink scan results was found in S-CODE: the system only received pushed webhooks. Evidence: utils/startup_checks.py lines 652 to 745; utils/startup_checks.py lines 1587 to 1597; main.py lines 2451 to 2474; scripts/preflight_scanner_check.py lines 203 to 214; ops_dashboard/backend/services/scanner_attribution.py lines 660 to 682.

### 6.2 The request
Both callers use check_scanner_connectivity; each injects its own fetcher.

| Property | Start-up check (main.py's fetcher) | Operator script (its own fetcher) | Evidence |
|---|---|---|---|
| URL | Each map entry's chartink_url; the twin file's URL only for an entry without one, which the schema does not allow, so on this code path no twin-file URL is fetched (INTERPRETATION, L-01) | Same | utils/startup_checks.py lines 764 to 776; core/config_loader.py lines 2350 to 2351 |
| Construction | The configured string unchanged; no parameters added | Same | utils/startup_checks.py lines 687 to 702 |
| Method | GET (urllib Request without data) | GET (requests.get) | main.py lines 246 to 250; scripts/preflight_scanner_check.py line 101 |
| Headers | User-Agent "TradingSystem/2.0 startup-check" | User-Agent "TradingSystem/2.0 preflight-check" | main.py lines 246 to 248; scripts/preflight_scanner_check.py line 64 |
| Authentication | None | None | main.py lines 235 to 261; scripts/preflight_scanner_check.py lines 95 to 118 |
| Timeout | 10 s, the function's default (run_all passes none) | --timeout, default 10 s | utils/startup_checks.py lines 652 to 659; utils/startup_checks.py lines 1592 to 1595; scripts/preflight_scanner_check.py lines 50 to 56 |
| Delay before the first request | scanner_check_delay_sec = 5.0 | None | config/system_config.yaml lines 752 to 753; core/config_loader.py line 2026; utils/startup_checks.py lines 675 to 678 |
| Retry | None | Up to 2 retries with 1 s back-off on network errors and 5xx; none on 4xx | scripts/preflight_scanner_check.py lines 67 to 118 |
| Order and rate | One request at a time, in map order; no limiter | Same | utils/startup_checks.py lines 687 to 742 |
| Response | First 256 bytes read; HTTP error codes returned as codes; other exceptions give None | First 300 characters kept for --verbose | main.py lines 249 to 261; scripts/preflight_scanner_check.py lines 101 to 117 |
| Reachable | Status 200 to 299; response_has_content = a non-empty body | Same | utils/startup_checks.py lines 701 to 730 |
| Parsing | None ("Does NOT parse Chartink HTML -- reachability only") | None | utils/startup_checks.py lines 660 to 672 |
| Logging | The first None per scanner at WARNING, repeats at DEBUG; other non-2xx results at WARNING | A stderr logger and a stdout table | utils/startup_checks.py lines 641 to 649; utils/startup_checks.py lines 704 to 722 |
| Result | The warning "scanner_unreachable" only; no notifier call found; main reads report.warnings only under --dry-run | Exit 0 if every scanner is reachable, else 1; no alert; nothing written | utils/startup_checks.py lines 1596 to 1597; main.py lines 2476 to 2482; scripts/preflight_scanner_check.py lines 222 to 231 |
| Empty map | Vacuously all reachable | Same | utils/startup_checks.py line 744 |

- Worst case read from the code: the 5 s delay plus 16 sequential requests of up to 10 s each (INTERPRETATION, L-03; actual durations UNKNOWN).
- KNOWN DOC DRIFT (D-03). The twin file's header says the operator script "sends an HTTP HEAD request to each URL and fails CRITICAL if any is unreachable at startup" (config/chartink_scanners.yaml lines 3 to 5); the code sends GET, reads the map's URLs rather than the twin file's, and at start-up only warns. The locked decision P17 (HEAD; all 15 URLs; map validation; duplicate check; CRITICAL on any failure; docs/locked_decisions.yaml lines 370 to 376) and the configuration guide ("HEAD-checks"; docs/CONFIG_GUIDE.md lines 655 to 660) describe the same unimplemented design.
- UNKNOWN (U-03): what Chartink returned for these GETs, for an existing screener, a missing slug or a refused client. The FIX-184 docstring claims that Chartink's edge answered urllib's default User-Agent with 403 and that "any non-urllib UA returns 200" (main.py lines 238 to 244), and the guide calls a 403 "cosmetic" (docs/CONFIG_GUIDE.md lines 658 to 660); both are claims, not checked here (no live contact; W-5). A 2xx proves only that the configured address answered.

### 6.3 When the start-up check ran
- run_all_startup_checks calls detect_startup_scenario a second time and skips the scanner check only when that call reads COLD. Evidence: utils/startup_checks.py lines 1560 to 1565; utils/startup_checks.py lines 1587 to 1589.
- main.py makes its own scenario call and then writes today's session row before it calls run_all. Evidence: main.py lines 2268 to 2292; main.py lines 2304 to 2314.
- With today's row present, the second call cannot read COLD, which needs no row or an earlier session date (utils/startup_checks.py lines 289 to 382). It reads CRASH at the daily start, when no SHUTDOWN event or completed EOD square-off exists yet that day, and WARM after a clean stop or a completed EOD square-off the same day (A-011 section 6, frozen, rule 5). So, on main.py's path at the baseline, the check runs at every start that reaches the start-up checks (B-002 section 5, step 9), the daily start included. This is a code-path reading; the records cited by A-011 section 6 and B-007 section 5 (row C3) show the second call reading CRASH at the starts they recorded.
- The docstring's "skipped on COLD scenario" (utils/startup_checks.py lines 1531 to 1545) and its test, which seeds a previous-day session (tests/unit/test_startup_checks.py lines 1205 to 1215), describe a call made with no same-date row. From main.py that needs the date of the second call to differ from the row's date: a start that crosses midnight between the two, outside the start window (B-002 section 5, step 3) unless that is bypassed (INTERPRETATION of the code path). KNOWN DEFECT / AMBIGUITY (D-08): the skip branch is effectively unreachable from main.py.
- Frozen manuals that agree: A-011 section 6 records the second call logging CRASH every morning (RECORD-DERIVED, 26-Aug boot record), and B-007 section 5 (rows C3 and C7) records that the check runs at the daily start. Frozen passages that did not agree: A-008 section 8 (a start-up HEAD check, CRITICAL if unreachable) and B-002 section 3 (row 17: "skipped on a COLD start"). Errata A-008 E-4 and B-002 E-2 correct them (ACCEPTED, DR-053; section 17).
- A-009 section 3 lists the twin file as read by the pre-flight scanner check, "for the pre-flight reachability check". The check does receive the file (utils/startup_checks.py lines 680 to 688) and takes each URL from the map (6.2); this manual adds what the reader does with it and proposes no erratum there (C-001_review_notes.md section 5).
- How often the check ran and what it returned: UNKNOWN; no logs survive (U-05).

### 6.4 The operator script, scripts/preflight_scanner_check.py
- A standalone script ("Standalone CLI that operator runs BEFORE market open") with --config (default "config"), --verbose and --timeout (default 10). It checks that the configuration directory exists, runs load_all, runs check_scanner_connectivity with its requests-based fetcher, prints a table and exits 0 if every scanner is reachable, else 1. It sends no alert and writes nothing. Evidence: scripts/preflight_scanner_check.py lines 4 to 7; scripts/preflight_scanner_check.py lines 34 to 57; scripts/preflight_scanner_check.py lines 173 to 231.
- No caller or scheduler was found in S-ALL. The file name occurs only in a configuration comment (config/chartink_scanners.yaml line 4), the locked-decision record (docs/locked_decisions.yaml line 376), a comment in requirements.txt (the requests pin), the script itself and its unit test (tests/unit/test_preflight_scanner.py lines 1 to 13); it is absent from the cron file, the cron registry and the deploy/systemd/ units.
- HISTORICAL (Rama, 21-Oct-2026): never run. His words: "Never, In legacy system entire process is headless via cron jobs (oracle VM), So Booting > Shutdown is headless nothing requires human interference except clearing hardkill". This agrees with the source search; the docstring's operator step was not practised.

### 6.5 The scheduled pre-flight and the dashboard's "Chartink" card (owner B-007, frozen)
- The scheduled pre-flight Phases A, B and C (declared for 08:30, 09:14 and 09:15 on weekdays) check file presence (CRITICAL), the local webhook /health (CRITICAL; 200 or 401 counts as up), /metrics signals_received of at least 1 (WARN) and queue depth (WARN). No screener address is fetched. Evidence: deploy/cron/trading-system.cron lines 57 to 64; scripts/preflight/checks/config_integrity.py lines 16 to 50; scripts/preflight/checks/signals.py lines 1 to 89. The installed crontab: UNKNOWN.
- The dashboard's "Chartink" dependency card shows the worse of the pre-flight results webhook_responsive and signals_arrived, that is the local /health answer and "at least one signal today". It does not measure screener reachability, per-scanner arrival or scan results (ops_dashboard/backend/services/system_health.py lines 407 to 427). It reads pre-flight result tables for which B-007 section 8 (frozen) found no writer in its inspected scope.
- The deprecated pre-market check tests file presence only and is "Kept (NOT cron'd)" (scripts/premarket_healthcheck.py lines 1 to 9; scripts/premarket_healthcheck.py lines 54 to 72).

### 6.6 What each level proves
| Level | Shown by | Not shown | Evidence |
|---|---|---|---|
| 1. Screener connectivity | A 2xx to a GET of the configured URL (a start-up warning or the script's exit code) | That the scan exists, has these conditions, has an alert or will post | utils/startup_checks.py lines 701 to 730 |
| 2. Scan and alert | Nothing in the repository (Chartink's side) | - | Section 4.1; U-01 |
| 3. Webhook delivery | ~~A webhook_audit row for the POST, whatever the response~~ (superseded by erratum E-1) A webhook_audit row for the POST, whatever the response, when the request reaches the handler, its body is read and the insert succeeds (C-002_manual.md section 10.3); the key from the path | That any symbol was taken | signals/webhook_receiver.py lines 477 to 534; signals/webhook_receiver.py lines 1126 to 1154 |
| 4. Webhook acceptance | A per-symbol ACCEPTED: the signals row inserted as QUEUED and the tuple queued | That processing will pass | signals/webhook_receiver.py lines 956 to 1072 |
| 5. Signal persistence | The signals row and its status changes | An order or a trade | core/schema.sql lines 42 to 79; core/state_store.py line 1424 |
| 6. Signal processing | Statuses written by the processor and the screener (REJECTED_*, PASSED, TRADED and others) | Fills | signals/signal_processor.py lines 867 to 1004 |
| 7. Trade | trades rows | - | Outside C-001 |
| Local endpoint health | /health answering 200 or 401 (start-up self-check; pre-flight Phase C) | Anything about Chartink | scripts/preflight/checks/signals.py lines 20 to 42 |

## 7. The scanner key at the webhook edge and in processing (FACT, SOURCE-VERIFIED; C-002 to C-011 own the detail)

### 7.1 The receiver's order of steps
1. Chartink posts to /webhook/<scanner key>; the server is waitress on the configured host and port (main.py lines 3979 to 4000). Arming, URL and delivery: C-002.
2. _handle_webhook answers 503 during shutdown and 429 above the per-IP limit, then runs _process_request; ~~it writes one webhook_audit row in every case~~ (superseded by erratum E-1) it writes one webhook_audit row in every case once the body has been read, unless the insert fails; the framework's own answers and a body over 1 MiB (answered 413 at the read) leave no row (C-002_manual.md section 10.3, NR-1 to NR-4) (signals/webhook_receiver.py lines 477 to 534).
3. Authentication, else 401 (signals/webhook_receiver.py lines 547 to 549; C-003).
4. The key must be a map key, else 404 (signals/webhook_receiver.py lines 551 to 554). C-001.
5. An "eod" scanner leaves for the EOD route here, before the kill-switch, backpressure and entry-window gates (signals/webhook_receiver.py lines 556 to 564). C-001 for the routing by type; C-011 for the route.
6. Kill switch 403; backpressure 503; entry window 403 (signals/webhook_receiver.py lines 566 to 583).
7. JSON, numeric casts and required fields, each 400 on failure (signals/webhook_receiver.py lines 585 to 615; C-004).
8. The alert-name check, 400 on a mismatch (signals/webhook_receiver.py lines 617 to 633). C-001.
9. triggered_at is parsed; stocks and trigger_prices are split (signals/webhook_receiver.py lines 635 to 674).
10. Per symbol: alias, exclusion list, then _process_signal (empty symbol, price, expiry, in-flight claim, cache deduplication, fingerprint), the signals insert and the queue tuple (signal_id, scanner key, symbol, price, triggered_at). Evidence: signals/webhook_receiver.py lines 696 to 728; signals/webhook_receiver.py lines 892 to 1072.

The order agrees with A-008 section 8 and B-008 (frozen).

### 7.2 The stored identity (KNOWN DEFECT / AMBIGUITY, latent: D-05)
- The insert writes the scanner key into both signals.scanner and signals.strategy (signals/webhook_receiver.py lines 963 to 980), while processing resolves the strategy through the map (signals/signal_processor.py lines 933 to 953). No runtime writer updates signals.strategy later: in S-RT the state store updates status and rejection_reason only (core/state_store.py lines 1424, 2996 and 3057).
- Consumers that read signals.strategy as the strategy: the replay tool (scripts/replay_signals.py lines 91 to 95), the dashboard's per-strategy funnel and counts (ops_dashboard/backend/readers/db_reader.py lines 684 to 704; ops_dashboard/backend/readers/db_reader.py lines 2093 to 2101) and the registry officer (scripts/strategy_registry_officer.py lines 53 to 60).
- No effect at the baseline, because every key equals its strategy (4.3); any N:1 or renamed mapping would make these consumers attribute to the key. The processor test that uses a key different from its strategy inserts the mapped name itself (tests/unit/test_signal_processor.py lines 1933 to 1964), so no test covers the production insert.

### 7.3 Symbols at the edge (Q-20 evidence; section 15)
- config/symbol_aliases.yaml holds two aliases (config/symbol_aliases.yaml lines 9 to 10). It is not part of AppConfig: the receiver reads it once, at construction, from that relative path, with keys and values upper-cased; a missing file logs a warning and an unreadable one an error, and both give an empty map (fail-open). It is not hashed and not in the configuration-diff alert. Evidence: signals/webhook_receiver.py lines 222 to 227; signals/webhook_receiver.py lines 258 to 292.
- Lookup is by the upper-cased raw symbol: a hit returns the alias; a miss keeps the raw symbol unchanged, its case included (signals/webhook_receiver.py line 701).
- The exclusion list (five symbols) is compared case-insensitively after aliasing and gives REJECTED_EXCLUDED_SYMBOL without a signals row (config/system_config.yaml lines 130 to 136; signals/webhook_receiver.py lines 708 to 717).

### 7.4 Scanner-to-strategy resolution (the processor's Step 2)
- SignalProcessor._process_one, Step 2: lookup by scanner key in the processor's copy of the map, the strategy name from the entry, then the strategy object by name; each failure gives REJECTED_UNKNOWN_STRATEGY. Then the strategy-control gate. From here the strategy object (direction, intent, controls) drives processing (C-008). Evidence: signals/signal_processor.py lines 930 to 953; signals/signal_processor.py lines 955 to 972.
- No direction ambiguity exists at the baseline (4.3). The entry throttle is keyed by symbol only, across all scanners (signals/entry_throttle.py lines 1 to 19; signals/entry_throttle.py line 65; signals/signal_processor.py lines 262 to 268).

### 7.5 The EOD route (owner C-011)
- _handle_eod parses the body, applies the aliases and submits (scanner key, symbol, triggered_at) to the capture worker, or records a fail-safe miss when none is wired; the worker writes pb01_watchlist rows with source = the key. It is wired when watchlist.enabled is true. Evidence: signals/webhook_receiver.py lines 751 to 804; v3_chain/watchlist_capture.py lines 106 to 178; main.py lines 3712 to 3755; config/system_config.yaml lines 619 to 621.

### 7.6 The limits of the alert-name rule (KNOWN DEFECT / AMBIGUITY, D-07)
- Only case, spaces and the account prefix are normalised. An alert whose name differs from its key in any other character (a hyphen, for example) gets 400, and an alert with no scan_name is accepted on the path alone (signals/webhook_receiver.py lines 627 to 632). Whether production alert names ever hit this is UNKNOWN: the mismatch log line exists, and no logs survive.

## 8. Attribution on the dashboard (FACT, SOURCE-VERIFIED unless marked)
- Screen 21 (GET /api/scanner-attribution/screen and its XLSX export; ops_dashboard/backend/api/analytics2.py lines 626 to 651) is built by build_scanner_attribution_screen (ops_dashboard/backend/services/scanner_attribution.py lines 275 to 423) from one strategy_tower build for today (ops_dashboard/backend/services/strategy_tower.py lines 91 to 116), the map file, per-strategy daily signal counts and today's rejections by status. Per strategy it shows: the signals stored today, grouped by signals.strategy into accepted, rejected, duplicated and expired buckets (ops_dashboard/backend/readers/db_reader.py lines 659 to 704); the "received" count from webhook_audit by scanner_name, mapped to strategies through the map (ops_dashboard/backend/readers/db_reader.py lines 707 to 725); orders and trades by strategy; a four-part quality score (ops_dashboard/backend/services/scanner_attribution.py lines 228 to 267); the KPI "total scanners" = map entries (ops_dashboard/backend/services/scanner_attribution.py lines 476 to 507); and a mapping panel with each scanner's URL as written in the map (ops_dashboard/backend/services/scanner_attribution.py lines 652 to 682).
- The older endpoint GET /api/scanner-attribution (ops_dashboard/backend/api/analytics2.py lines 96 to 101) is built per scanner over a chosen period by build_scanner_attribution (ops_dashboard/backend/services/analytics_period.py lines 500 to 549): webhook_audit per scanner_name, signals per scanner, trades attributed through trades.signal_id to signals.scanner, a three-factor score, and "scanner-level (shared)" as the linked strategy of an unmapped scanner. Evidence: ops_dashboard/backend/readers/db_reader.py lines 2267 to 2281; ops_dashboard/backend/readers/db_reader.py lines 2072 to 2090; ops_dashboard/backend/readers/db_reader.py lines 1651 to 1666.

Limitations:
- A-L1. Read-only displays of a separate process: they show what the tables held, and their code shows what would be displayed, not what was (V2_DESIGN_LOG lesson of 2026-10-19).
- A-L2. Screen 21 is today-only by design, and no per-source history is stored (ops_dashboard/backend/services/scanner_attribution.py lines 282 to 287; ops_dashboard/backend/services/scanner_attribution.py lines 409 to 418).
- A-L3. Strategy attribution uses signals.strategy, which holds the scanner key (7.2).
- A-L4. KNOWN DEFECT / AMBIGUITY (D-06). webhook_audit takes its accepted and rejected counts from a response only when it is HTTP 200. A request answered 401, 403, 404, 429 or 503 records 0 and 0, so "received" (accepted plus rejected) leaves out every symbol of such a request; a 503 batch caused by one QUEUE_FULL or STORE_ERROR symbol records 0 accepted although other symbols in it were queued. Evidence: signals/webhook_receiver.py lines 510 to 517; signals/webhook_receiver.py lines 730 to 745.
- A-L5. EOD alerts answer 200 with accepted = the number of symbols, so webhook_audit counts them as accepted although no signals row is created (signals/webhook_receiver.py lines 799 to 804); recorded as IA-P1-03 (FACT (RECORD-DERIVED), docs/audit/integrity_audit_2026.md lines 187 to 209), matching the source (D-06).
- A-L6. The dashboard's "accepted" bucket means stored and not in a rejected, duplicate or expired family; it is not the receiver's ACCEPTED, not processed and not traded (ops_dashboard/backend/readers/db_reader.py lines 659 to 681).
- A-L7. Symbols refused before the insert (INVALID_SYMBOL, INVALID_PRICE, EXPIRED, IN_PROCESS, a cache DUPLICATE, REJECTED_EXCLUDED_SYMBOL) never become signals rows, so they are invisible to the stored-signal panels and appear only as webhook_audit rejected counts, when the request answered 200 (signals/webhook_receiver.py lines 708 to 717; signals/webhook_receiver.py lines 904 to 947).
- A-L8. Which map file the production dashboard read: UNKNOWN (5.1).
- A-L9. FACT (RECORD-DERIVED): the signals table was pruned on 16-Jul-2026 of pre-09-Jul REJECTED*, EXPIRED and DUPLICATE rows, so arrival claims rest on webhook_audit, which was not pruned (docs/audit/integrity_audit_2026.md lines 17 to 29).

## 9. Arrival records and Rama's accounts

### 9.1 Records (FACT, RECORD-DERIVED; each true for its own date and scope)
| ID | Record | What it says | Evidence |
|---|---|---|---|
| R-01 | Integrity audit, measured 31-Jul-2026 (IA-P1-01) | range_breakout_long and range_breakout_short received no webhook at all from 12-Jun to 31-Jul: 0 POSTs in 127,272 webhook_audit rows of every response code, 0 signals, 0 trades; 404s all-time 0 | docs/audit/integrity_audit_2026.md lines 126 to 160; docs/audit/integrity_audit_2026.md lines 84 to 99 |
| R-02 | The same audit; a configuration comment of 25-Jul | PB-01: 5 POSTs, one a night, 27 to 31-Jul; the earlier triggers since 13-Jul hit a refused connection because the service had already stopped, which is why service_window_end became 17:35 | docs/audit/integrity_audit_2026.md lines 84 to 88; config/system_config.yaml lines 105 to 112 |
| R-03 | The same audit, 24 to 31-Jul | Every day's REJECTED_STRATEGY_CONTROL rows belonged to the 3 positional scanners; "currently-tradeable population = 12 strategies" | docs/audit/integrity_audit_2026.md lines 89 to 97 |
| R-04 | The same audit, 12-Jun to 31-Jul | 13 intraday-type keys with 4,941 to 12,005 POSTs each; signals rows per scanner 488 to 20,666; 13 strategies traded | docs/audit/integrity_audit_2026.md lines 84 to 99 |
| R-05 | Audit of 05-Jul-2026 | S10 "confirmed dead in production"; no twin-file-to-map check; placeholder URLs UNCERTAIN; its line numbers refer to the code of that date | docs/audit/audit_05jul2026.md lines 446 and 457 |
| R-06 | Integrity audit, 31-Jul | S10 still unwired; the placeholder note "has never been verified against the live Chartink account" | docs/audit/integrity_audit_2026.md lines 56 to 63; docs/audit/integrity_audit_2026.md lines 145 to 149 |
| R-07 | C2 investigation, 03-Jul | 58,507 webhook requests over 15 trading days from 12-Jun; webhook_audit.source_ip the only inbound-address source (no address reproduced; the investigation's verdict belongs to C-003) | docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 8 to 17 |
| R-08 | BK-1 analysis, 17-Jul | 155 closed trades from 15-Jun to 16-Jul; 11 scanners in the per-scanner table; already used by A-008 (DR-023) | docs/audit/bk1_long_short_scanner_analysis_17jul2026.md lines 1 to 19; docs/audit/bk1_long_short_scanner_analysis_17jul2026.md lines 83 to 95 |

- Decision records, for what was decided, not what the code did (D-01, D-03): P17 (a HEAD check of all 15 URLs, map validation, the system's own /health, no duplicates, CRITICAL on any failure; docs/locked_decisions.yaml lines 370 to 376), S10 (map cross-validation at start-up; docs/locked_decisions.yaml lines 2724 to 2732), S14 and S15 (the file formats, "populated with placeholder Chartink URLs" for 15 scanners; docs/locked_decisions.yaml lines 2771 to 2791) and one YAML per Chartink scanner, 15 files (docs/locked_decisions.yaml lines 2616 to 2618; PB-01 was added later).
- The assumptions record: A14, "Chartink retries on 503", NOT_TESTED_YET (docs/SYSTEM_ASSUMPTIONS.md line 20); the behaviour itself is UNKNOWN (U-07; C-002). Its A1 cites a function, parse_chartink_payload, that has no definition in S-ALL (docs/SYSTEM_ASSUMPTIONS.md line 7; KNOWN DOC DRIFT, D-09).

### 9.2 Rama's accounts (HISTORICAL (Rama), 21-Oct-2026)
His answers to Claude's questions of 21-Oct; values are not reproduced.
- The two range-breakout alerts: "Yes, Configured". He cited both alerts' webhook URLs: plain HTTP to the legacy VM's address on port 5000, the paths /webhook/range_breakout_short and /webhook/range_breakout_long, and a ?token= query parameter, and offered browser access to "the chartink page which is opened". When the alerts were set up, and whether they were active from 12-Jun to 31-Jul, he did not say.
- The URL form, in answer to whether the committed screener URLs were the real screeners: "In previous questions shared the chartink link, every strategy name only differs". Read with his previous answer, it describes the webhook URLs (one form, only the strategy name differing), not the screener pages (INTERPRETATION); U-02 stays open.
- The operator script: never run (6.4).
- Read together with R-01 (INTERPRETATION / AUDIT LEAD): ~~at the baseline the receiver records every POST that reaches it, whatever its response (7.1, step 2)~~ (superseded by erratum E-1) at the baseline the receiver records every POST that reaches its handler and whose body is read, whatever its response, unless the insert fails (7.1, step 2; C-002_manual.md section 10.3), and the record counted webhook_audit rows of every response code and no 404 at any time, so no POST to an unknown key under /webhook/ arrived either. A POST to another path, port or host would not reach the receiver and would leave no row, and whether the deployment of June and July recorded every POST in the same way is not established (U-04). With Rama's account that the paths matched the keys, the explanations include, as an open list: the two alerts never triggered (the self-defeating-conditions lead, Q-34 (d)); they triggered but were not delivered (as recorded for PB-01 while the service was stopped, R-02; whether Chartink retried is UNKNOWN, U-07); they were not active in the period; or they pointed elsewhere then. The cause stays UNKNOWN.
- Not checked: the Chartink account itself (his choice of 21-Oct, "No browser; record UNKNOWN (Recommended)"). U-01 and U-02 stay open for the audit after the agenda (Q-34 (g); DR-016).
- The plain-HTTP transport and the token in the query string are recorded for C-002 and C-003; A-009 section 16 (frozen) records the ?token= check and A-010 the secret's location.

## 10. Paper and live (FACT, SOURCE-VERIFIED; a limited conclusion)
- Within the inventory files at the baseline, no paper/live difference was found in scanner selection, start-up connectivity, request construction, key routing, resolution or attribution: one load (main.py lines 2196 to 2202, no mode branch); the start-up check called without a mode (main.py lines 2451 to 2474); the operator script without a mode (scripts/preflight_scanner_check.py lines 34 to 57); the receiver constructed without one (main.py lines 3407 to 3415); resolution identical, the mode used only in alert titles (signals/signal_processor.py line 227); one EOD flag for both modes (core/config_loader.py lines 1516 to 1531); attribution queries without a mode split (ops_dashboard/backend/services/strategy_tower.py lines 95 to 98).
- The one mode-dependent consumer of scanner-adjacent configuration: the paper quote provider's second use of the alias file (main.py lines 473 to 538; main.py line 2394). The integration test runs in paper mode only (tests/integration/test_full_signal_flow.py lines 1 to 21).
- This is not a claim of universal parity, and it says nothing about either environment's installed configuration. Differences between production and the testing VM are a separate matter: the prefix strip depends on each machine's accounts file (4.3).

## 11. Tests: what they assert and what they do not prove (none was run)
| Test | Asserts | Does not prove |
|---|---|---|
| tests/unit/test_preflight_scanner.py lines 68 to 157 | The script exits 0 when all are reachable and 1 when one is not (load_all and the check patched) | Real reachability; that the production configuration loads; any schedule |
| tests/unit/test_preflight_scanner.py lines 284 to 394 | The script's fetcher returns status and snippet, None on a network error, honours the timeout and retries network errors and 5xx but not 4xx (requests mocked) | Chartink's real responses |
| tests/unit/test_startup_checks.py lines 544 to 620 | Classification with injected fetchers: 2xx reachable; 404 and exceptions unreachable; an empty map vacuously true | main.py's urllib fetcher; the network |
| tests/unit/test_startup_checks.py lines 1104 to 1139 | An unreachable scanner gives ok True with "scanner_unreachable" among the warnings (a same-day session seeded) | What main does with the warning (nothing; 6.2) |
| tests/unit/test_startup_checks.py lines 1205 to 1215 | A previous-day session gives COLD and no scanner result | That main.py's path can reach COLD (6.3) |
| tests/unit/test_config_loader.py lines 1036 to 1065 | The real map validates, each strategy file exists, URLs are https, at least 10 entries | That production starts ran S10; duplicate keys (safe_load collapses them) |
| tests/unit/test_strategies.py lines 357 to 396 | S10 passes on the real map and raises for a missing strategy, when the test passes the map path | That any production caller passes it (5.3) |
| tests/unit/test_pb01_registration.py lines 31 to 92 | PB-01 is in both files, loads as v3_playbook with enabled false, and is refused by strategy control under every trade_type and flag combination | The EOD route at runtime |
| tests/unit/test_v3_eod_route.py lines 39 to 119 | An eod scanner is captured with no intraday queueing and no entry-window gate; intraday scanners stay window-gated; a fail-safe miss without a worker; late binding; 400 on a bad body (receiver built with no secret) | Production wiring (main.py lines 3712 to 3755); authentication |
| tests/unit/test_webhook_receiver.py lines 63 to 133 and 220 to 242 | With a plain-dict test map (key presence only): an unknown path gives 404; a scan_name mismatch gives 400 | ScannerEntry objects; the production configuration |
| tests/unit/test_webhook_receiver.py lines 1864 to 1935 | The prefix is stripped with "-" or "_"; a genuine mismatch still gives 400; an absent scan_name skips the check; the "UNKNOWN" tag never strips (tag patched) | The real accounts file or real alert names |
| tests/unit/test_signal_processor.py lines 585 to 611 | An unmapped scanner, or a mapped strategy that is not loaded, gives REJECTED_UNKNOWN_STRATEGY | The receiver's insert of signals.strategy (7.2) |
| ops_dashboard/tests/test_screen21_scanner_attribution.py lines 85 to 102 | The real map is 1:1 (distinct strategies); skipped if the file is absent | Duplicate keys; the production dashboard's file |
| tests/integration/test_full_signal_flow.py lines 1 to 21 | End to end in paper mode with one scanner and a mocked broker | Live mode; real map objects; Chartink payload realism |

The full test list, with the tests that only import signals modules for later stages, is in the answers file, section 8.1.

## 12. Source-to-consumer trace (source wiring at the baseline, not host execution)
| Step | Source or input | Logic and guard | Output and next consumer | Missing link |
|---|---|---|---|---|
| T-1 Configuration load | The two files | core/config_loader.py lines 2457 to 2516; schema only (core/config_loader.py lines 2347 to 2388) | AppConfig; hashes into the session row (main.py lines 2304 to 2314); the checks, receiver and processor (main.py lines 2451 to 2474; main.py lines 3407 to 3415; main.py line 3585) | The installed configuration (U-04) |
| T-2 Start-up connectivity | The map's URLs (main.py lines 2458 to 2460) | utils/startup_checks.py lines 652 to 745; the fetcher main.py lines 235 to 261; runs at every start that reaches the checks (6.3) | A warning; nothing acts on it (main.py lines 2476 to 2502) | What Chartink answered (U-03); the logs |
| T-3 Operator script | --config, then load_all (scripts/preflight_scanner_check.py lines 183 to 200) | scripts/preflight_scanner_check.py lines 202 to 214; retries; the 2xx rule | A console table; exit 0 or 1 (scripts/preflight_scanner_check.py lines 216 to 231) | Never run (Rama, 9.2) |
| T-4 Scheduled pre-flight | deploy/cron/trading-system.cron lines 57 to 64 (declared) | scripts/preflight/checks/config_integrity.py lines 39 to 50; scripts/preflight/checks/signals.py lines 20 to 82 | Presence, /health, signals_received, queue depth; no screener fetched | The installed crontab; the result-table writer (B-007) |
| T-5 Receipt and key gate | POST /webhook/<key> | signals/webhook_receiver.py lines 406 to 408; signals/webhook_receiver.py lines 477 to 564 | A webhook_audit row (signals/webhook_receiver.py lines 1126 to 1154; core/schema.sql lines 581 to 593); the EOD route or the intraday gates | The Chartink-side alerts (U-01) |
| T-6 Alert-name check | The optional scan_name | signals/webhook_receiver.py lines 617 to 633; signals/webhook_receiver.py lines 111 to 138 | Pass, or 400 with a warning line | Production alert names |
| T-7 Symbols at the edge | The stocks field | signals/webhook_receiver.py lines 696 to 717 | The aliased symbol, or REJECTED_EXCLUDED_SYMBOL | The later unknown-symbol path (Q-20) |
| T-8 Acceptance and persistence | Each symbol | signals/webhook_receiver.py lines 892 to 1072; price, expiry, in-flight claim, deduplication, fingerprint (C-005) | A signals row with scanner = strategy = the key (core/schema.sql lines 42 to 79); the queue tuple | - |
| T-9 Resolution | The queue tuple (signals/signal_processor.py lines 867 to 887) | signals/signal_processor.py lines 930 to 953, on the processor's copy of the map | The strategy object; screening (C-008) | - |
| T-10 EOD route | An eod-type scanner (config/scan_webhook_map.yaml line 84) | signals/webhook_receiver.py lines 751 to 804; v3_chain/watchlist_capture.py lines 106 to 178 | A pb01_watchlist row with source = the key; the PB-01 entry stage (C-011) | - |
| T-11 Screen 21 | signals, webhook_audit, trades; the map file | ops_dashboard/backend/services/scanner_attribution.py lines 275 to 423 | JSON and XLSX views (ops_dashboard/backend/api/analytics2.py lines 626 to 651); the operator | The production dashboard configuration (A-L8) |
| T-12 Change detection | The file hashes against the session's stored hashes | main.py lines 2295 to 2296; utils/startup_checks.py lines 581 to 634 | CONFIG_DIFF event and WARN alert (main.py lines 2656 to 2675) | Content validation of the change |
| T-13 Direction registry officer | Strategy YAMLs; distinct signals.scanner and signals.strategy (scripts/strategy_registry_officer.py lines 53 to 60) | scripts/strategy_registry_officer.py lines 98 to 167 | Registry state outside the repository and notifications; declared schedule deploy/cron/trading-system.cron lines 126 to 127 | No trading consumer (4.3) |
| T-14 Replay tool | signals rows (scripts/replay_signals.py lines 41 to 51) | The strategy from signals.strategy, no map (scripts/replay_signals.py lines 91 to 95) | A CSV report | - |

## 13. Known defects, ambiguities and documentation drift
| ID | Finding | Label | Section |
|---|---|---|---|
| D-01 | The S10 map-to-strategy check is not run by any production caller; even when called it is one-directional and has no duplicate check | KNOWN DEFECT / AMBIGUITY | 5.3 |
| D-02 | The map header claims start-up map validation and a CRITICAL failure on duplicate names; three documents and a code comment repeat the start-up validation claim | KNOWN DOC DRIFT | 5.3 |
| D-03 | The twin file's header, P17 and the guide describe a HEAD check with CRITICAL failure; the code sends GET to the map's URLs and only warns at start-up | KNOWN DOC DRIFT | 6.2 |
| D-04 | watchlist.playbook_scanner has no reader; its comment says it binds the eod route | KNOWN DEFECT / AMBIGUITY (orphan setting); KNOWN DOC DRIFT | 4.4 |
| D-05 | signals.strategy stores the scanner key; consumers read it as the strategy | KNOWN DEFECT / AMBIGUITY (latent) | 7.2 |
| D-06 | webhook_audit counts nothing for a non-200 response, mixed 503 batches included, and counts EOD captures as accepted | KNOWN DEFECT / AMBIGUITY | 8 (A-L4, A-L5) |
| D-07 | The alert-name rule normalises case, spaces and the prefix only; scan_name is optional | KNOWN DEFECT / AMBIGUITY | 7.6 |
| D-08 | The start-up check's COLD skip is effectively unreachable from main.py; its docstring and test describe a case a normal start does not reach | KNOWN DEFECT / AMBIGUITY | 6.3 |
| D-09 | docs/SYSTEM_ASSUMPTIONS.md line 7 cites a function with no definition in S-ALL | KNOWN DOC DRIFT | 9.1 |

Placement: D-05 and D-06 are recorded here because they concern the scanner identity as stored and counted at the C-001 boundary; their later stages belong to C-007, C-008, C-010 and K-003 (D-05) and to L-009 and L-010 (D-06).

## 14. Unknown / not established
- U-01. The Chartink account's scanners, alerts, alert names, webhook URLs, alert frequency and duplicate-notification settings over the live period, and whether all 16 alerts were configured and active. Rama's account of 21-Oct covers the two range-breakout alerts' configuration and the URL form only (9.2).
- U-02. Whether the 16 committed screener URLs were the live screeners (the twin file's placeholder note; never verified, R-06; not checked in C-001, Rama's choice of 21-Oct).
- U-03. What Chartink answered to the connectivity GETs (6.2).
- U-04. The installed state on the production VM: the configuration files, the crontab, the dashboard's configuration, and whether the baseline's receiver (with the prefix strip of 09-Sep-2026) was deployed.
- U-05. How often the start-up check ran and what it returned (no logs). The operator script: never run (Rama; 6.4).
- U-06. Arrivals after 31-Jul-2026 up to the baseline.
- U-07. Whether Chartink retried on 503 (C-002).
- The cause of R-01 (9.2).

## 15. Q-20 and Q-34 evidence (no decision; both stay OPEN)
- Q-20 (the signal-to-instrument lookup key and path; the exchange; unknown symbols):
  - The symbol comes from the comma-separated stocks field; the receiver reads no exchange field (signals/webhook_receiver.py lines 664 to 674). Aliases and the exclusion list are applied as in 7.3.
  - The receiver does not look the symbol up in the instrument cache: its only symbol test is non-empty, and the stored and queued symbol is the aliased one (signals/webhook_receiver.py lines 904 to 906; signals/webhook_receiver.py lines 963 to 980; signals/webhook_receiver.py line 1047).
  - The instrument cache is keyed by the exact symbol string and raises InstrumentNotFoundError on a miss; its rows carry an exchange column, NSE or BSE (core/instrument_cache.py lines 13 to 18; core/instrument_cache.py line 93; core/instrument_cache.py lines 219 to 236; core/instrument_cache.py lines 62 to 64). INTERPRETATION: if two rows shared a symbol, the later row would win the symbol index (core/instrument_cache.py line 93); not checked against the instrument file, which A-004 covered.
  - AUDIT LEAD: the adapter builds quote keys as "NSE:" + symbol and passes the exchange "NSE" literally in three calls (broker/zerodha_adapter.py lines 1759 to 1760; broker/zerodha_adapter.py lines 641, 743 and 792), and the order placer builds "NSE:" quote keys (orders/order_placer.py lines 4051 to 4054; orders/order_placer.py lines 4078 to 4081). On these paths the exchange appears fixed in code, not taken from Chartink; their wider contexts were not read.
  - Unknown-symbol handling after the receiver was not traced here (C-004, C-008). The operator tool scripts/find_symbol_aliases.py suggests aliases for failed symbols against the instrument file; no caller or scheduler was found in S-ALL (scripts/find_symbol_aliases.py lines 1 to 105).
- Q-34 (A-008 audit leads, DR-016): for (d), R-01 and Rama's account (9.2) narrow the explanations without settling the cause; for (g), U-01 and U-02, with R-05 and R-06, stay open; the screener URLs were not checked (Rama, 21-Oct).

## 16. Answer-sheet status
| Sheet question | Answers file | This manual |
|---|---|---|
| Q1 Inventory and boundaries | Section 1 (rows I-01 to I-33) | Sections 1 and 2 |
| Q2 Definitions and catalogue | Section 2 | Section 4 |
| Q3 Loading and validation | Section 3 | Sections 5 and 6.3 |
| Q4 URLs and requests | Section 4 | Sections 6.1 and 6.2 |
| Q5 Pre-flight, health and attribution | Section 5 | Sections 6.4 to 6.6 and 8 |
| Q6 The signal lifecycle | Section 6 | Section 7 |
| Q7 Paper and live | Section 7 | Section 10 |
| Q8 Tests and records | Section 8 | Sections 9 and 11 |
| Q9 Evidence map | Section 9 | Section 12 |
| Q10 Findings | Section 10 | Sections 13 to 15 |

Corrections to the answers file are listed in C-001_reconciliation_addendum.md item 2; the file itself stays unchanged as an input.

## 17. Corrections to frozen manuals (errata A-008 E-4 and B-002 E-2, ACCEPTED, DR-053)
- A-008 E-4 (A-008_manual.md section 8, the bullet on config/chartink_scanners.yaml). Evidence: sections 6.2 and 6.3 (main.py lines 246 to 250; utils/startup_checks.py lines 764 to 776; core/config_loader.py lines 2350 to 2351; utils/startup_checks.py lines 1596 to 1597; main.py lines 2476 to 2482; scripts/preflight_scanner_check.py lines 222 to 231). Exact texts, each running to the end of its line here:
  - Struck: for a start-up HTTP HEAD reachability check (CRITICAL if unreachable)
  - Replacement: which its header describes for a start-up HTTP HEAD reachability check failing CRITICAL; in the code the start-up check sends GET to each scan_webhook_map entry's chartink_url (the same strings; this file's URL is used only for an entry without one, which the map's schema does not allow) and only warns, and the operator script exits 1 without an alert (C-001_manual.md section 6)
  - Unchanged before it: - config/chartink_scanners.yaml (S15, P17): 16 slug-style Chartink URLs
  - Unchanged after it: . The file's own note says "URLs are placeholders". Whether they matched the real screeners: UNKNOWN; the check proves reachability, not the scanner conditions.
- B-002 E-2 (B-002_manual.md section 3, row 17, the column "Read by, when"). Evidence: section 6.3 (utils/startup_checks.py lines 1560 to 1565; utils/startup_checks.py lines 1587 to 1589; main.py lines 2304 to 2314); A-011 section 6 and B-007 section 5, rows C3 and C7 (frozen). Exact texts, each running to the end of its line here:
  - Struck: skipped on a COLD start
  - Replacement: run at every start that reaches step 9: its COLD skip tests the checks' own second scenario call, which follows today's session row (step 6) and so reads CRASH at the daily start (B-007 section 5, rows C3 and C7; C-001_manual.md section 6.3)
  - Unchanged before it: | 17 | Scanner URLs (from AppConfig) | Start-up check 7,
  - Unchanged after it: | HTTP reachability | Warning only |
- Each manual shows the struck text with "(superseded by erratum E-n)" and the replacement, one sentence in its status line and an erratum section at its end, now with "Status: ACCEPTED"; A-008_review_notes.md and B-002_review_notes.md each carry one status sentence (C-001_review_notes.md section 7).
- Decision at the freeze (21-Oct-2026): Rama decided the errata independently of each other and of this manual's freeze and accepted both (his answer: "Accept both (Recommended)", one of four options that included accepting either one alone or neither). Both are ACCEPTED with this freeze and recorded in DR-053. The A-008 freeze (DR-023) and the B-002 freeze (DR-037) stand; each erratum corrects only the passage shown.
- Checked and not changed: A-011 section 6 and B-007 section 5 (they already record the second call's CRASH and the daily-start run); A-009 section 3 (the twin file's reader and declared role; sharpened in 6.3, no erratum proposed); A-008 section 8's first bullet (the three-entry convention of the 17-Jul record; 4.1) and its WR4 and alert-name bullets; B-002 section 5.
- Searches (V2_DESIGN_LOG lesson of 2026-10-19), at V2 main 5d575d1:
  - All of docs/agenda and docs/governance, ignoring case, for "HTTP HEAD", "HEAD check", "HEAD-check", "HEAD reachab", "CRITICAL if unreachable", "skipped on COLD", "skipped on a COLD", "COLD skip", "pre-flight reachability", "reachability check", "scanner_unreachable", "scanner connectivity", "check_scanner_connectivity" and "preflight_scanner". Hits: A-008 section 8 (the twin-file bullet; E-4), B-002 section 3 row 17 (E-2), A-009 section 3 (the twin-file row; sharpened) and B-007 section 5 row C7 (agrees), and two mentions of a file path only (A-009 section 2's list of files read; the Section C handover's list of likely paths).
  - Every frozen manual's final statement and every register row, for "scanner", "Chartink" and "reachab". The hits mention Chartink, the scanner map or other things (A-010's pre-commit secret scanner; B-004's "reachable") without describing the connectivity check. A-009's final statement carries the S10 and duplicate-key finding of A-008 erratum E-1, and the register row DR-023's "duplicate scanner name: start-up failure" is already corrected by that erratum (DR-024); register rows are not edited.
  - Read directly for the scenario reading: A-011 section 6 and B-007 section 5 row C3 (both agree).

## 18. Cross-references
A-001 and DR-012 (both legacy VMs terminated); A-004 (instruments; Q-20); A-007 (force_intraday_only; erratum E-1); A-008 (strategy model and Chartink conditions; ~~errata E-1 and, proposed, E-4~~ (superseded by this manual's erratum E-2) errata E-1 and E-4); A-009 (configuration; sections 3, 5 and 16); A-010 (the webhook secret's location); A-011 (start-up scenarios, section 6); B-002 (configuration loading; ~~erratum E-2, proposed~~ (superseded by this manual's erratum E-2) erratum E-2); B-007 (start-up checks, pre-flight and the dashboard's readiness views); B-008 and B-009 (the signal path in the session); C-002 to C-011 (section 1); K-003 (signal state); L-009 and L-010 (audit trail; dashboard); M-006 (health checks); Q-20 and Q-34 (section 15); VP-08, VP-44 and VP-45 (C-001_v2_design_inputs.md).

## 19. Final C-001 statement
> At the baseline the legacy system held 16 Chartink scanner definitions in config/scan_webhook_map.yaml, each keyed by a name equal to its strategy's name and its webhook path, and a twin URL file whose URLs had no effect identified in the inspected paths. Loading checked the schema only: the S10 cross-check and the duplicate-name failure claimed by the map's header ran on no production path, and a duplicate key would have been collapsed silently (none existed). No code that pulled scan results was found; the system received pushed webhooks. On main.py's path the code fetched each configured screener address only to test reachability, at every start that reached the start-up checks, as a warning that nothing acted on; an operator script did the same on demand, with no scheduler found, and Rama never ran it. A passing check proved that an address answered, not that a scanner existed, matched its conditions or would post. At the webhook edge the path key had to be in the map (else 404), the optional alert name had to equal it after case, space and account-prefix normalisation, and the webhook carried no side; the processor resolved the key to a strategy through the map, and the strategy's direction applied. The receiver stored the key as both scanner and strategy, which changed nothing only because every key equalled its strategy. The dashboard attributed activity by those stored names and by webhook_audit counts that left out non-200 requests. Dated records show 14 of the 16 keys posting between 12-Jun and 31-Jul-2026 and the two range-breakout keys never posting; Rama reports both of those alerts configured with matching paths, so the cause stays unknown. Whether the committed screener URLs were the real screeners was never established.

## 20. Revision 0 (2026-10-21)
- Drafted by Claude from its two research files, ChatGPT's value-added review and Rama's answers of 21-Oct; there is no ChatGPT draft to work from.
- Corrections carried from the reconciliation addendum (item 2): DR-031's wording, quoted in the addendum and the V2 inputs, replaces the answers file's shorthand; the start-up check's COLD skip is stated with the readings that A-011 and B-007 record (CRASH at the daily start; WARM after a clean same-day stop, A-011); O-01 and O-03 answered by Rama (sections 9.2 and 6.4); O-02 left UNKNOWN by his choice (U-02); O-04 answered from the frozen manuals (sections 6.3 and 17); O-05 settled as placement only (section 13); F-04's negative scoped (4.1); the missing-file row corrected (5.2).
- Added beyond the research files: the check against the frozen manuals and the two proposed errata (sections 6.3 and 17); the reading of R-01 with Rama's account (9.2); the registration convention (4.1).
- Before delivery an independent check by a separate agent compared these files with the research files, ChatGPT's review and the frozen manuals; its findings are applied (C-001_review_notes.md section 10).
- No source fact of the research files was changed beyond the corrections listed; no V2 decision is made here.
- Freeze (21-Oct-2026, DR-053): ChatGPT's independent review of revision 0 ("substantially sound"; no blocker) asked for no change to this manual; it was then frozen with both errata accepted. The freeze changed only the status and date lines, the errata sentence of section 6.3, section 17's status and this item.

## Erratum (2026-10-21, found during C-002)
- E-1 (sections 3, 6.6, 7.1 and 9.2: the request audit's coverage). Status: ACCEPTED. Proposed during C-002 (revision 0, 2026-10-21), accepted in principle in ChatGPT's value-added finalisation review of C-002 (22-Oct-2026), approved by Rama on 22-Oct-2026, independently of the C-002 freeze and of the other errata, and recorded in DR-055 (2026-10-22) together with the C-002 freeze. The struck texts stay visible; each replacement is the text in force. Authoritative passage: C-002_manual.md section 10.3 (NR-1 to NR-4).
- What changed: the four passages said that the receiver writes one webhook_audit row for every POST, whatever the response. A row is written only for a request that reaches the handler and whose body is read, and only if the insert succeeds; the framework's own answers, a body over 1 MiB and a failed insert leave none (C-002_manual.md section 10.3, NR-1 to NR-4).
- Passages checked and not changed: section 8 (A-L4 and A-L5, the counts); section 9.1 (the records' counts stand); the rest of section 9.2's reading of R-01, which holds, since Chartink's bodies are kilobyte-scale and the integrity audit reports the hole's incidence as zero; section 12, row T-5; section 13, D-06; section 19.
- Reason: a docstring and a schema comment ("regardless of outcome") are claims about the code; where a comment and the code disagree, the code wins (V2_DESIGN_LOG lesson of 2026-10-07).
- Impact: wording only; the arrival records and C-001's conclusions stand. B-008 erratum E-1, B-009 erratum E-2 and A-010 erratum E-4 cite the same passage. No V2 decision depends on the old wording; VP-11 and VP-59 are annotated.
- E-2 (section 18, the cross-references of A-008 and B-002). Status: ACCEPTED. Proposed during C-002 (revision 0, 2026-10-21), accepted in principle in ChatGPT's value-added finalisation review of C-002 (22-Oct-2026), approved by Rama on 22-Oct-2026, independently of the C-002 freeze and of the other errata, and recorded in DR-055 (2026-10-22) together with the C-002 freeze. Authoritative passages: DR-053, this manual's status line and section 17.
- What changed: section 18 still called A-008 E-4 and B-002 E-2 "proposed", although both were accepted at the C-001 freeze; that freeze changed the status lines, section 6.3's errata sentence, section 17's status and section 20's last item, not section 18 (section 20).
- Passages checked and not changed: the status line and sections 6.3, 17 and 20, which already record both errata as ACCEPTED.
- Reason: a status slip at the freeze (V2_DESIGN_LOG lesson of 2026-10-21, C-002).
- Impact: wording only; no V2 decision depends on it.
