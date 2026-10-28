# D-005 Candle analysis - Stage 1, S1-0 safety preflight and S1-1 count-only log

Date of work: 28-Oct-2026, 10:53 to 11:24 IST. Prepared by Claude for ChatGPT's read-list checkpoint. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_count_log.md; nothing is written to any repository now.

Revision 1: corrections A to D of D005_STAGE1_CHECKPOINT_READ_AUTHORIZATION_RESHARED.txt (received from Rama at 11:22 IST) applied (section 6). Revision 0 was delivered at 11:03 IST. No count was rerun and no observed count changed.

Authority: D005_CHATGPT_STAGE0_REVIEW_AND_LIMITED_STAGE1_AUTHORIZATION_28-OCT-2026.txt (received from Rama at 10:50 IST; 66 lines, 9,752 bytes, SHA-256 A0927C4EEF37E7D90ADB125ED318ED8DE950C97966EAAED96F2F09AAEDA52B6F). It authorises only S1-0 and S1-1: the count-only expressions S01, S02, S04a, S04b, S04c, S04d and S05 in their named scopes, with controls. No content read was made: no legacy line was printed, displayed or used; no read tool was run in this step, and the gate's refusal of reads (READS_AUTHORISED False) was exercised only by its self-test. S03 and S06 were not run.

Labels: FACT (METADATA) for git metadata and object counts; FACT (COUNT) for a count result, true only for its expression, scope and the pin; FACT (V2 RECORD) for a V2 record; INTERPRETATION where marked. A count line number says that the expression matched that line; it says nothing about what the line does, whether it is code, a comment or a string, or whether it ran.

## 1. S1-0 safety preflight: PASS

Run at 10:56:16 to 10:56:17 IST, before any count; repeated at 11:00:12 to 11:00:13 IST after the last count. Both runs: PASS. `git status` was not run.

| Check | Before the counts | After the counts |
|---|---|---|
| Legacy remote HEAD and refs/heads/main (ls-remote) | 970aabf3c78d777d9d34549e021f7c8e756c7bdb, no other ref | 970aabf3c78d777d9d34549e021f7c8e756c7bdb, no other ref |
| V2 local HEAD and GitHub main | 2364a9976a6634134549ad1ddb36ef30f65d3034 | 2364a9976a6634134549ad1ddb36ef30f65d3034 |
| Reading clone HEAD, tree | 970aabf3c78d777d9d34549e021f7c8e756c7bdb, 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa | the same |
| Reading clone `ls-tree -r HEAD` SHA-256 = saved listing = recorded value | 1F195D3D516AB4F194BCB7AF105FB193A606EE241E2E1437102EF495D8E647A5 | the same |
| sensitive_check.py | exit 0 (it exits 1 when a never-open blob is present); printed "never-open blobs present locally: 0" at 10:53:58 | exit 0; the same printed line at the final check |
| Object counts (cat-file --batch-all-objects --batch-check) | {'blob': 816, 'tree': 101, 'commit': 1} | {'blob': 816, 'tree': 101, 'commit': 1} |
| Working files of the authorised scopes (s_rt, s_cfg, s_tests; the S02 files are in s_rt) present and equal to the pinned blob (git hash-object --no-filters --stdin-paths) | 808 paths, 0 missing, 0 mismatched | 808 paths, 0 missing, 0 mismatched |

- The reading clone is /home/claude/legacy/prod, as OQ-09 accepts; no checkout, fetch or filter change was made. Its 816 blobs are the D-004 Stage 1 checkout (814) plus DV-01's two ignore files; unchanged through this step.
- Scopes: scope_rules.py (SHA-256 241736B0E0DE12A4BF535E699210D75354DBCA9E8F3FF06306C4B534FC556840) rebuilt s_rt 281, s_cfg 41, s_tests 486 (and s_all 1,260, s_code 869, not used) from the listing; each list is byte-identical to the list used at D-004.
- Gate test (gate_test_d005.py), on synthetic input before any count (lesson of 2026-10-24, V2_DESIGN_LOG.md:139):
  - 1 gate self-test: PASS
  - 2 refusal S03 s_rt : PASS / refused: expression ID not authorised; nothing searched
  - 2 refusal S06 s_tests : PASS / refused: expression ID not authorised; nothing searched
  - 2 refusal S01 s_everything : PASS / refused: unknown scope; nothing searched
  - 2 refusal S02 s_rt : PASS / refused: scope not authorised for this expression; nothing searched
  - 3 synthetic S01: PASS designed [1, 2, 3] pipeline [1, 2, 3] direct [1, 2, 3]
  - 3 synthetic S02: PASS designed [1, 2, 3, 4] pipeline [1, 2, 3, 4] direct [1, 2, 3, 4]
  - 3 synthetic S04a: PASS designed [1, 2, 3, 4, 5, 6, 7, 8, 9] pipeline [1, 2, 3, 4, 5, 6, 7, 8, 9] direct [1, 2, 3, 4, 5, 6, 7, 8, 9]
  - 3 synthetic S04b: PASS designed [1, 2, 3, 4, 5, 6] pipeline [1, 2, 3, 4, 5, 6] direct [1, 2, 3, 4, 5, 6]
  - 3 synthetic S04c: PASS designed [1, 2, 3, 4, 5, 6, 7, 8] pipeline [1, 2, 3, 4, 5, 6, 7, 8] direct [1, 2, 3, 4, 5, 6, 7, 8]
  - 3 synthetic S04d: PASS designed [1, 2, 3, 4, 5, 6, 7, 8] pipeline [1, 2, 3, 4, 5, 6, 7, 8] direct [1, 2, 3, 4, 5, 6, 7, 8]
  - 3 synthetic S05: PASS designed [1, 2, 3, 4] pipeline [1, 2, 3, 4] direct [1, 2, 3, 4]
  - GATE TEST: PASS
- The gate refuses by name and by blob: 102 never-open paths (one invented name per rule tested), the two July records, HR-1 and HR-2, paths outside the tree, the shared-blob path docs/v2_design_spec.md and absolute paths; it refuses any expression ID or text other than the authorised seven, any scope other than the named ones, and any path list that differs from the authorised scope; and every content read.
- Masking-filter inputs (lesson of 2026-10-26, V2_DESIGN_LOG.md:153): every count runs mask_read2.py --grep through count_grep_lines.py. On every invocation the filter loaded the scanned files and, in full and value-blind, HR-1 and HR-2 as its host-token source (HR-3 absent). Invocations: 30 for the ten runs (a synthetic control file each, and the scope in chunks of 200 files) and 7 in the gate test. The filter's output (the masked matched lines) went only to count_grep_lines.py, which keeps line numbers and discards the text; nothing of it was printed, saved or seen.
- Line totals of the files in the proposed read list were taken with `wc -l` (newline counts; no line printed), to keep every proposed range inside its file. They are one below D-004's L01 figures and Stage 0's figures, which count the reader's lines (the newline count plus one; V2_DESIGN_LOG.md:139, C-009 ERR-1).

## 2. S1-1 runs actually made (10 expression-scope runs of 7 expression IDs)

Every expression is case-insensitive, as the Stage 0 plan states, and its text is taken from the gate's table, not typed. "|" in the expressions below is shown as "\|".

| Run | ID | Scope (files) | Expression | Files searched | Files with matches | Matching lines | Synthetic control | Real control |
|---|---|---|---|---|---|---|---|---|
| 1 | S01 | s_rt (281) | `(?i)candle_math` | 281 | 9 | 16 | PASS | NO VALID REAL CONTROL (the run printed PASS on a paraphrase; corrected, A) |
| 2 | S01 | s_cfg (41) | `(?i)candle_math` | 41 | 1 | 1 | PASS | PASS (verbatim record) |
| 3 | S01 | s_tests (486) | `(?i)candle_math` | 486 | 2 | 4 | PASS | NO VALID REAL CONTROL |
| 4 | S02 | s02_files (3) | `(?i)^\s*(def\|class)\s` | 3 | 3 | 43 | PASS | NO VALID REAL CONTROL |
| 5 | S04a | s_rt (281) | `(?i)\bbody\b\|wick\|upper_shadow\|lower_shadow\|\bdoji\b\|\bhammer\b\|engulf\|marubozu\|pin_?bar` | 281 | 75 | 353 | PASS | NO VALID REAL CONTROL |
| 6 | S04b | s_rt (281) | `(?i)bullish\|bearish\|\bis_(green\|red)\b\|close\S{0,3}\s*[<>]=?\s*\S{0,16}open\|open\S{0,3}\s*[<>]=?\s*\S{0,16}close` | 281 | 1 | 4 | PASS | NO VALID REAL CONTROL |
| 7 | S04c | s_rt (281) | `(?i)high\S{0,3}\s*-\s*\S{0,16}low\|close\S{0,3}\s*-\s*\S{0,16}open\|open\S{0,3}\s*-\s*\S{0,16}close\|true_range\|candle_range\|range_pct` | 281 | 11 | 27 | PASS | NO VALID REAL CONTROL |
| 8 | S04d | s_rt (281) | `(?i)is_synthetic\|partial\|forming\|settled\|is_closed\|closed_candle\|last_closed` | 281 | 77 | 509 | PASS | PASS (verbatim record, in the run); PASS (identifier record, on the saved output) |
| 9 | S05 | s_rt (281) | `(?i)\bCandleData\b\|\bCandle\b\|\bfrom_kite\b` | 281 | 42 | 300 | PASS | none in the run; PASS (identifier records, on the saved output) |
| 10 | S05 | s_tests (486) | `(?i)\bCandleData\b\|\bCandle\b\|\bfrom_kite\b` | 486 | 28 | 224 | PASS | NO VALID REAL CONTROL |

- Runs made between 10:56:33 and 10:57:43 IST; each run: gate self-test first, sensitive_check.py before and after, object counts before and after (unchanged in all ten), the excluded-range rule applied.
- Excluded ranges (OQ-05): hits inside main.py 3695-3713 or core/analytics_schema.sql 28-45 are dropped before anything is written. No hit of any run fell inside them (core/analytics_schema.sql is in no authorised scope).
- Real controls (OQ-10). One standard, applied to every control the Stage 0 plan named: a real control is valid only where a frozen V2 record quotes the matched text at stated lines of the pinned file, as a verbatim line or as the exact identifier the expression matches. A paraphrase does not qualify. The checkpoint confirmed this standard, both halves, and that the regime import is not a control (decision 4). The run's own control field followed the gate table; the review below was made after the runs, on the saved outputs, and changes no count.

| Expression and scope | Planned control (Stage 0 plan section 8) | Record | Standard met? | Result on the saved output |
|---|---|---|---|---|
| S01 s_rt | regime/engine.py, imports at 26-28 | D-004 O-02, an OBSERVATION that paraphrases the imports ("imports its adx, atr, ema and true-range functions"; D-004_manual.md:232); it does not quote the module name | NO (correction A: the run labelled it "D-004 FACT (SOURCE)" and printed PASS; both are withdrawn) | NO VALID REAL CONTROL; the hits at 26, 27, 28 are an observation only |
| S01 s_cfg | config/system_config.yaml:628 | the line quoted verbatim (C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt:977) | YES | PASS (628), in the run |
| S02 three files | a def inside core/candle_math.py 54-115 | D-004 MC-33 paraphrases two functions there (D-004_manual.md:231) | NO | defs at 54 and 83; observation |
| S04a s_rt | screening/step_executor.py, "body" in 318-329 | D-003 MC-33 paraphrases the formula (D-003_manual.md:317) | NO | hits at 321, 326, 328; observation |
| S04b s_rt | synthetic only | - | - | - |
| S04c s_rt | screening/step_executor.py, day_high - day_low in 318-329 | D-003 MC-33, a paraphrase | NO | hit at 327; observation |
| S04d s_rt | v3_chain/watchlist_capture.py 143-151 | the comment and the log text quoted verbatim (C-011_manual.md:433-434) | YES | PASS (143, 144, 148, 149), in the run |
| S04d s_rt | data/candle_store.py, is_synthetic in 24-36 | D-004 MC-21 names the identifier is_synthetic at those lines (D-004_manual.md:215) | YES, by the identifier half (confirmed, decision 4) | PASS (36), evaluated after the run; dropped from the gate table at the run without a stated reason |
| S05 s_rt | sr_detector/models.py, Candle in 23-43 | D-004 MC-31: "in Candle, ts is the historical row's ..." (D-004_manual.md:229) | YES, identifier half | PASS (24, 34, 36), evaluated after the run; dropped from the gate table without a stated reason |
| S05 s_rt | sr_detector/fetch.py, Candle.from_kite in 53-151 | D-004 MC-30: "converts rows with Candle.from_kite" (D-004_manual.md:228) | YES, identifier half | PASS (53, 55, 63, 72, 88, 117, 126, 138, 149), evaluated after the run |
| S05 s_rt | data/candle_store.py, CandleData | named only in a matrix cell without lines ("CandleData (MC-21)", D-004_manual.md:176) | NO | hits at 25, 26, 34 and others; observation |
| S01 s_tests, S05 s_tests | none planned | - | - | synthetic only |

  Under the confirmed standard seven of the ten runs have NO VALID REAL CONTROL: S01 s_rt, S01 s_tests, S02, S04a, S04b, S04c and S05 s_tests. Every valid real control evaluated passed: S01 s_cfg and S04d in the run, and the S04d and S05 s_rt identifier controls on the saved outputs. The synthetic control passed for every run; it verifies the counting pipeline on synthetic input only. The positive hits are used only as candidate locators for the listed read ranges, not as a comprehensive search result, and no zero or missing-hit conclusion is drawn from any run (checkpoint decision 4).
- Synthetic controls: one invented file per expression, with lines designed to match and lines designed not to (output of synth_d005.py; no legacy content). Each run requires the pipeline's hit lines to equal the designed lines and a direct re.search over the same file. All seven passed in the gate test and again in every run.
- Not run: S03 and S06 (they need function names that only a content read can give); any other expression.

## 3. Per-file counts and line numbers (FACT (COUNT); no matched text)

### S01 over s_rt

- files in scope: 281; files searched: 281; files with matches: 9; matching lines: 16
- synthetic control: PASS (expected lines [1, 2, 3], got [1, 2, 3], direct re [1, 2, 3]; 3 hit lines and 2 non-hit lines by design)
- real control: NO VALID REAL CONTROL (correction A). The saved run output reads "PASS" and labels the record "D-004 FACT (SOURCE)"; both are withdrawn: D-004_manual.md:232 is OBSERVATION O-02, a paraphrase. The hits at 26, 27 and 28 are an observation only.

```
    1  core/candle_math.py  2
    1  core/config_loader.py  1544
    2  core/daily_stats.py  11,21
    1  regime/__init__.py  7
    4  regime/engine.py  15,26,27,28
    3  sr_detector/zone_builder.py  16,120,154
    1  v3_chain/pb01_entry.py  39
    1  v3_chain/pb01_runner.py  31
    2  v3_chain/runner.py  17,27
```

### S01 over s_cfg

- files in scope: 41; files searched: 41; files with matches: 1; matching lines: 1
- synthetic control: PASS (expected lines [1, 2, 3], got [1, 2, 3], direct re [1, 2, 3]; 3 hit lines and 2 non-hit lines by design)
- real control: PASS: config/system_config.yaml 628-628 (line 628 quoted verbatim, "(built via candle_math.resample)" (C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt:977))

```
    1  config/system_config.yaml  628
```

### S01 over s_tests

- files in scope: 486; files searched: 486; files with matches: 2; matching lines: 4
- synthetic control: PASS (expected lines [1, 2, 3], got [1, 2, 3], direct re [1, 2, 3]; 3 hit lines and 2 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
    3  tests/unit/test_candle_math.py  1,3,17
    1  tests/unit/test_v3_chain.py  17
```

### S02 over s02_files

- files in scope: 3; files searched: 3; files with matches: 3; matching lines: 43
- synthetic control: PASS (expected lines [1, 2, 3, 4], got [1, 2, 3, 4], direct re [1, 2, 3, 4]; 4 hit lines and 4 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
   12  core/candle_math.py  54,83,136,165,184,191,219,250,310,328,335,385
   16  data/candle_store.py  25,40,50,66,79,110,120,128,133,170,197,203,211,219,233,253
   15  sr_detector/models.py  24,34,51,60,73,77,80,87,104,107,111,129,148,157,190
```

### S04a over s_rt

- files in scope: 281; files searched: 281; files with matches: 75; matching lines: 353
- synthetic control: PASS (expected lines [1, 2, 3, 4, 5, 6, 7, 8, 9], got [1, 2, 3, 4, 5, 6, 7, 8, 9], direct re [1, 2, 3, 4, 5, 6, 7, 8, 9]; 9 hit lines and 5 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
    5  alerts/critical.py  15,54,72,83,102
    2  alerts/cron_alerts.py  107,115
    2  alerts/delivery.py  80,104
    9  alerts/mis_squareoff_notifier.py  304,316,321,332,340,349,350,372,386
   36  alerts/telegram_notifier.py  22,322,331,340,352,387,390,393,419,453,473,482,496,505,512,523,536,542,545,552,571,577,581,601,606,745,748,768,779,798,821,861,873,884,893,903
    2  broker/auth_recovery.py  17,83
    1  broker/token_monitor.py  206
    1  broker/zerodha_adapter.py  652
    1  capital/fund_manager.py  60
   11  capital/kill_switch.py  154,157,648,655,663,666,920,1230,1415,1463,1512
    1  capital/risk_engine.py  31
    2  capital/strategy_governor.py  160,184
    2  core/evidence_contract.py  343,357
    1  core/state_store.py  347
    3  data/live_feed.py  401,461,498
   31  main.py  251,252,256,258,259,325,336,349,353,677,715,747,785,835,918,923,1027,1438,1463,1516,1549,1800,2194,2671,3273,3346,3347,3348,3350,4041,4194
    2  ops/control_tower/reporter.py  54,55
    4  ops_dashboard/backend/api/operations.py  62,63,67,77
    6  ops_dashboard/backend/readers/control_client.py  54,71,72,75,77,79
    1  ops_dashboard/backend/readers/db_reader.py  4193
    1  ops_dashboard/backend/readers/log_reader.py  136
    5  ops_dashboard/backend/readers/metrics_client.py  35,51,53,54,55
    3  ops_dashboard/backend/services/controls.py  125,126,226
    1  ops_dashboard/backend/services/system_health.py  719
    1  orders/breakeven_manager.py  368
    1  orders/closure_classifier.py  101
    6  orders/cnc_gtt_monitor.py  304,310,411,428,742,748
    9  orders/eod_squareoff.py  677,685,693,701,706,717,1805,1861,1872
    1  orders/mis_autosquareoff.py  946
   19  orders/order_placer.py  753,854,1177,1751,1759,1901,1908,1981,2252,2269,2273,2408,2419,4014,4113,4139,4375,4399,4430
   16  orders/order_reconciler.py  528,661,804,1444,1457,1584,1858,1971,2165,2516,2934,3137,3600,3927,4548,4612
    2  orders/shadow_tracker.py  732,740
    1  orders/sl_breach_monitor.py  188
    2  orders/structure_exit_manager.py  33,336
    6  orders/tgt_retry_manager.py  154,207,339,369,385,390
    5  screening/hard_gate.py  14,67,382,407,412
    3  screening/step_executor.py  321,326,328
    5  scripts/alert_watcher.py  23,187,213,256,423
    4  scripts/approve_ssh_keys.py  57,64,133,139
    5  scripts/backup_retention.py  234,240,298,301,302
    1  scripts/capture_metrics_baseline.py  307
    1  scripts/compute_strategy_metrics.py  221
    3  scripts/copy_gate.py  270,286,295
    6  scripts/cron_officer.py  752,753,764,800,804,847
    7  scripts/cron_report_render.py  7,544,547,550,628,630,633
    1  scripts/cron_watchdog.py  62
    1  scripts/disk_monitor.py  177
    2  scripts/fetch_fno_ban.py  112,118
    1  scripts/forward_shadow_record.py  277
    1  scripts/gemini_data_integrity_check.py  250
    3  scripts/gemini_log_review.py  254,271,287
    1  scripts/gemini_premarket_brief.py  177
    1  scripts/gemini_trade_coach.py  208
    1  scripts/gemini_watchman.py  255
    1  scripts/gemini_weekly_patterns.py  134
   11  scripts/generate_crontab.py  56,99,104,105,109,112,115,118,123,303,310
    2  scripts/healthcheck_server.py  122,130
    2  scripts/liveness_probe.py  278,286
    6  scripts/load_test_signals.py  112,117,123,125,131,140
    2  scripts/monitoring_canary.py  298,306
   11  scripts/preflight/checks/engine.py  38,54,56,57,58,59,70,74,75,76,77
    2  scripts/preflight/checks/signals.py  27,41
    1  scripts/preflight/deliver.py  80
    3  scripts/preflight/report.py  390,391,394
    2  scripts/preflight_scanner_check.py  145,166
    1  scripts/reconcile_pnl.py  262
    1  scripts/reconcile_positions.py  311
    2  scripts/reconstruct_excursions.py  404,507
    1  scripts/refresh_instruments.py  282
    8  scripts/security_monitor.py  101,1113,1114,1205,1217,1249,1371,1390
    4  scripts/strategy_registry_officer.py  193,201,211,213
    4  scripts/system_manager.py  70,910,926,932
    2  signals/signal_processor.py  533,548
   34  signals/webhook_receiver.py  199,309,318,319,321,383,518,542,585,587,595,596,600,604,610,613,617,620,629,633,637,665,666,684,685,758,761,762,764,767,771,860,861,864
    4  utils/startup_checks.py  702,729,798,818
```

### S04b over s_rt

- files in scope: 281; files searched: 281; files with matches: 1; matching lines: 4
- synthetic control: PASS (expected lines [1, 2, 3, 4, 5, 6], got [1, 2, 3, 4, 5, 6], direct re [1, 2, 3, 4, 5, 6]; 6 hit lines and 4 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
    4  orders/structure_exit_manager.py  11,12,283,285
```

### S04c over s_rt

- files in scope: 281; files searched: 281; files with matches: 11; matching lines: 27
- synthetic control: PASS (expected lines [1, 2, 3, 4, 5, 6, 7, 8], got [1, 2, 3, 4, 5, 6, 7, 8], direct re [1, 2, 3, 4, 5, 6, 7, 8]; 8 hit lines and 3 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
   11  core/candle_math.py  19,21,54,57,61,75,77,101,117,194,214
    1  core/config_loader.py  1416
    1  core/state_store.py  2551
    1  ops_dashboard/backend/services/execution_analytics.py  596
    6  regime/engine.py  15,28,191,207,245,340
    2  screening/hard_gate.py  385,433
    1  screening/step_executor.py  327
    1  scripts/gemini_trade_coach.py  160
    1  sr_detector/flags.py  159
    1  sr_detector/models.py  78
    1  sr_detector/retest_confirm.py  133
```

### S04d over s_rt

- files in scope: 281; files searched: 281; files with matches: 77; matching lines: 509
- synthetic control: PASS (expected lines [1, 2, 3, 4, 5, 6, 7, 8], got [1, 2, 3, 4, 5, 6, 7, 8], direct re [1, 2, 3, 4, 5, 6, 7, 8]; 8 hit lines and 3 non-hit lines by design)
- real control: PASS: v3_chain/watchlist_capture.py 143-151 (the comment "... a FORMING daily candle ... a settled EOD result ..." at 143-145 and the log text "FORMING session ... await settled EOD" at 146-151, quoted in C-011_manual.md:433-434)

```
    2  alerts/cron_alerts.py  17,93
    2  alerts/mis_squareoff_notifier.py  87,356
   45  broker/order_monitor.py  20,48,76,77,86,178,195,232,431,446,773,774,883,889,892,899,903,904,905,906,908,910,911,913,925,928,933,942,945,946,948,949,951,957,963,966,971,979,1026,1027,1029,1031,1032,1038,1054
   13  broker/order_state_machine.py  9,13,14,15,16,21,22,68,92,102,103,104,105
    1  broker/slippage_engine.py  37
    3  broker/zerodha_adapter.py  192,770,968
   11  capital/fund_manager.py  902,1048,1274,1386,1568,1582,1676,1729,1977,2142,2240
    3  capital/invariant.py  83,100,138
   11  capital/kill_switch.py  41,93,192,432,436,1157,1524,1687,1713,1913,2084
    1  capital/performance_allocator.py  6
   17  capital/risk_engine.py  298,307,310,395,436,440,442,460,461,480,543,594,614,615,630,662,668
   13  core/candle_math.py  316,324,328,329,332,364,365,431,432,445,447,448,454
    7  core/config_loader.py  15,40,1030,1851,1852,1860,2463
    1  core/daily_stats.py  56
    4  core/events.py  111,113,115,207
   11  core/evidence_contract.py  25,28,31,38,41,42,82,85,94,128,570
   48  core/state_store.py  620,624,642,645,654,660,706,735,829,831,832,835,838,846,851,868,875,909,916,923,937,944,955,965,1074,1127,1138,1162,1197,1209,1220,1244,1434,1463,1479,1594,1689,1696,1796,1818,2041,2049,2053,2356,2424,2433,2534,3317
   13  data/candle_store.py  36,97,178,180,220,225,257,276,302,305,306,327,345
    6  main.py  552,1052,1260,2788,3007,3940
   20  ops_dashboard/backend/readers/db_reader.py  23,29,36,504,813,1496,1879,1882,2368,2379,2393,2417,2484,2498,2499,2734,2900,2929,2930,3726
    1  ops_dashboard/backend/readers/log_reader.py  79
    3  ops_dashboard/backend/services/audit.py  59,213,333
    1  ops_dashboard/backend/services/capacity.py  261
    1  ops_dashboard/backend/services/config_view.py  541
    1  ops_dashboard/backend/services/execution_analytics.py  370
    4  ops_dashboard/backend/services/holdings.py  25,143,147,287
    3  ops_dashboard/backend/services/live_activity.py  430,894,917
    1  ops_dashboard/backend/services/strategy_ranking.py  376
    3  ops_dashboard/backend/services/system_logs.py  196,201,239
    5  ops_dashboard/backend/services/trade_logs.py  266,269,272,279,280
   23  orders/breakeven_manager.py  13,19,53,54,57,124,125,144,145,155,222,227,228,229,233,237,238,239,244,247,248,318,319
    2  orders/cnc_gtt.py  44,132
    6  orders/cnc_gtt_monitor.py  212,285,480,546,616,618
    8  orders/eod_squareoff.py  252,281,1055,1056,1133,1212,1253,1410
    1  orders/full_entry_engine.py  148
    4  orders/mis_autosquareoff.py  158,587,1190,1205
    4  orders/order_manager.py  36,352,575,661
   54  orders/order_placer.py  29,50,119,125,181,230,694,697,699,1571,1679,1788,1790,1793,1799,1806,1820,1836,1847,1857,1865,1876,1880,1887,1895,1898,1907,1912,1918,1919,1926,1927,1928,1936,2017,2018,2021,2026,2034,2038,2048,2055,2066,2077,2085,2191,2192,2672,2680,2836,2839,2844,3524,3959
    8  orders/order_protocol_limit.py  15,18,82,430,433,463,490,502
   46  orders/order_reconciler.py  20,23,28,205,208,899,911,912,920,930,952,969,1196,1481,1493,1509,2119,2349,2351,2353,2363,2375,2381,2383,2385,2396,2459,2467,2481,2495,2510,2515,2517,2528,2532,2846,3163,3474,3576,3625,3736,3823,3829,4294,4301,4303
    3  orders/sl_breach_monitor.py  137,147,266
    1  orders/slippage_recorder.py  142
    3  orders/smart_tgt_manager.py  36,792,801
    1  orders/structure_exit_manager.py  230
    1  orders/tgt_retry_manager.py  15
    7  reports/daily_report.py  174,402,654,950,953,1017,1346
    5  reports/daily_trade_review.py  114,981,988,1036,2631
    1  screening/secondary_screener.py  656
    2  scripts/backup_retention.py  24,290
    2  scripts/cron_officer.py  231,402
    1  scripts/eod_broker_reconcile.py  121
    2  scripts/eod_cleanup.py  177,185
    2  scripts/eod_verify.py  5,118
    1  scripts/fetch_daily_candles.py  308
    1  scripts/gemini_data_integrity_check.py  13
    1  scripts/gemini_log_review.py  57
    1  scripts/gemini_premarket_brief.py  50
    1  scripts/gemini_watchman.py  63
    3  scripts/healthcheck_server.py  179,209,283
    1  scripts/preflight/checks/broker.py  48
    2  scripts/preflight/checks/engine.py  92,222
    2  scripts/preflight/checks/state.py  25,26
    1  scripts/preflight/sentinel.py  75
    2  scripts/reconcile_positions.py  179,185
    1  scripts/reconstruct_excursions.py  125
    6  scripts/system_manager.py  103,341,342,371,1137,1345
   14  signals/signal_processor.py  716,718,726,729,732,754,763,764,768,772,777,1984,2323,2602
    3  strategies/loader.py  28,75,111
    2  strategies/schema.py  107,108
    2  utils/cron_heartbeat.py  70,106
    1  v3_chain/models.py  85
    3  v3_chain/pb01_entry.py  20,21,174
    1  v3_chain/pb01_runner.py  256
    1  v3_chain/runner.py  282
    5  v3_chain/score.py  9,10,127,199,201
    4  v3_chain/truncate.py  11,12,76,79
    8  v3_chain/watchlist_capture.py  11,12,143,144,148,149,153,225
```

### S05 over s_rt

- files in scope: 281; files searched: 281; files with matches: 42; matching lines: 300
- synthetic control: PASS (expected lines [1, 2, 3, 4], got [1, 2, 3, 4], direct re [1, 2, 3, 4]; 4 hit lines and 4 non-hit lines by design)
- real control in the run: none [identifier controls PASS on the saved output: section 2]

```
   23  core/candle_math.py  3,9,10,17,23,41,42,54,55,61,64,84,136,191,250,264,323,336,369,375,376,426,427
    5  core/config_loader.py  1314,1542,1546,1584,1682
    7  core/daily_stats.py  10,24,28,29,35,49,56
    1  core/db_connect.py  37
    6  core/state_store.py  81,2027,2043,2094,2112,2773
   37  data/candle_store.py  2,5,8,9,25,26,34,41,72,90,91,111,116,171,182,197,198,203,204,211,225,235,237,254,257,280,286,293,310,311,317,335,355,356,359,363,366
   15  main.py  1736,2980,3629,3927,3931,3932,3933,3934,3935,3936,3937,3938,3939,3940,3943
    1  ops/control_tower/runner.py  97
    8  orders/breakeven_manager.py  16,68,168,169,175,188,191,192
    1  orders/order_placer.py  2401
   30  orders/smart_tgt_manager.py  5,23,25,34,74,109,132,165,208,337,355,358,360,369,373,379,380,383,393,397,399,406,431,432,455,578,580,639,791,801
   41  orders/structure_exit_manager.py  6,10,19,25,32,38,39,41,51,137,175,180,184,187,188,208,209,214,220,221,224,225,257,258,260,281,284,286,290,293,300,302,310,339,512,514,517,519,548,658,659
    1  regime/__init__.py  9
    1  regime/engine.py  18
    9  reports/daily_report.py  22,385,1243,1332,1342,1343,1344,1345,1346
   10  reports/daily_trade_review.py  19,2383,2387,2558,2562,2615,2618,2631,2632,2679
    1  screening/entry_gate.py  29
    3  screening/hard_gate.py  381,401,438
    1  screening/step_executor.py  321
    8  scripts/fetch_daily_candles.py  5,74,109,111,220,239,295,326
    3  scripts/forward_shadow_record.py  15,133,212
    3  scripts/gemini_data_integrity_check.py  5,59,74
    6  scripts/gemini_trade_coach.py  7,147,158,160,161,162
    1  scripts/generate_screened_stocks_csv.py  12
   10  scripts/reconstruct_excursions.py  5,22,79,80,91,97,99,184,192,388
    1  scripts/v3_shadow_soak_report.py  396
    1  sr_detector/confluence.py  203
    1  sr_detector/detector.py  18
   10  sr_detector/fetch.py  29,53,55,63,72,88,117,126,138,149
    1  sr_detector/flags.py  14
    3  sr_detector/models.py  24,34,36
    3  sr_detector/pivots.py  21,24,26
    7  sr_detector/retest_confirm.py  6,24,25,26,28,84,120
    7  sr_detector/zone_builder.py  19,78,86,92,144,158,186
    2  sr_detector/zones.py  25,79
    2  v3_chain/forward_shadow.py  79,81
   15  v3_chain/pb01_entry.py  20,21,52,61,62,68,71,148,175,180,186,206,213,233,243
    1  v3_chain/pb01_runner.py  15
    2  v3_chain/runner.py  10,160
    2  v3_chain/score.py  71,74
    8  v3_chain/truncate.py  7,12,14,54,55,57,62,76
    2  v3_chain/watchlist_capture.py  11,143
```

### S05 over s_tests

- files in scope: 486; files searched: 486; files with matches: 28; matching lines: 224
- synthetic control: PASS (expected lines [1, 2, 3, 4], got [1, 2, 3, 4], direct re [1, 2, 3, 4]; 4 hit lines and 4 non-hit lines by design)
- real control in the run: none [standard: NO VALID REAL CONTROL]

```
    8  tests/unit/test_candle_math.py  9,31,38,39,150,379,505,506
   28  tests/unit/test_candle_store.py  47,103,122,142,200,202,215,227,253,257,334,350,351,373,374,381,390,438,450,462,474,496,497,498,499,500,507,534
    2  tests/unit/test_daily_report.py  631,780
    6  tests/unit/test_daily_stats.py  5,11,21,30,31,59
    2  tests/unit/test_daily_trade_review.py  1158,1161
    1  tests/unit/test_e2_breakeven_sl_column.py  115
    2  tests/unit/test_fetch_daily_candles_indices.py  1,134
   20  tests/unit/test_fix132_breakeven_manager.py  105,106,124,125,136,140,141,142,153,154,167,168,187,188,206,207,227,228,229,233
    1  tests/unit/test_fix136_batch9.py  380
    2  tests/unit/test_fix142_final_cleanup.py  32,37
    1  tests/unit/test_gemini_data_integrity_check.py  10
    8  tests/unit/test_h9_candle_offbyone_landmine.py  12,87,93,106,114,142,151,154
    4  tests/unit/test_market_regime.py  24,65,70,116
    1  tests/unit/test_migrations.py  302
    1  tests/unit/test_reconstruct_excursions.py  6
   63  tests/unit/test_smart_tgt_manager.py  22,121,123,141,145,147,215,216,399,404,417,421,429,433,442,629,637,641,645,652,656,678,685,693,694,935,965,969,999,1023,1027,1031,1074,1085,1087,1089,1107,1109,1111,1113,1115,1134,1147,1262,1437,1461,1462,1475,1479,1501,1502,1515,1547,1548,1561,1606,1643,1754,1767,1795,1811,1840,1888
    8  tests/unit/test_sr_anchors.py  26,48,73,82,83,113,114,115
    2  tests/unit/test_sr_detector_fetch.py  13,83
    3  tests/unit/test_sr_detector_units.py  17,25,140
    1  tests/unit/test_sr_v2_monitor.py  47
    2  tests/unit/test_sr_v2_zone_cache.py  9,68
   29  tests/unit/test_structure_exit_manager.py  6,34,202,221,231,240,250,254,257,259,266,277,292,323,338,339,352,364,379,392,405,414,423,425,435,444,499,547,548
    5  tests/unit/test_v3_chain.py  6,22,41,52,99
    2  tests/unit/test_v3_pb01_e2e.py  18,29
   11  tests/unit/test_v3_pb01_entry.py  6,19,26,29,35,37,54,117,176,182,187
    2  tests/unit/test_v3_pb01_gates_score.py  128,133
    3  tests/unit/test_v3_pb01_runner.py  18,23,32
    6  tests/unit/test_v3_watchlist_capture.py  16,24,29,31,32,49
```

## 4. What the counts show, and what they do not

- S01: the string candle_math occurs in 9 runtime files (16 lines), 1 configuration line and 2 test files (4 lines) at the pin. For D-004 gap G-08 this gives a located candidate list (name references only); which of these lines import or call the module, and what they pass, is NOT ESTABLISHED until they are read (read list, R07).
- S02: core/candle_math.py has 12 def or class lines (54, 83, 136, 165, 184, 191, 219, 250, 310, 328, 335, 385). Read beside D-004's ranges (true range and ATR at 54-115, VWAP 136-153, RSI 250-269, resample 311-456; MC-33), the defs at 165, 184, 191 and 219 lie in lines not read for D-004 (its read rows 031 to 034) and not cited by any V2 record that git grep "candle_math" over docs/agenda finds (Stage 0 inventory, section 6.2) (INTERPRETATION: they may hold the ema and adx functions the regime imports). data/candle_store.py has 16 and sr_detector/models.py 15 def or class lines.
- S04c: the range and body arithmetic expression matches 27 lines in 11 files; S04b (direction) 4 lines in one file, orders/structure_exit_manager.py. These are candidate locators only (OQ-02).
- S04a, S04d and S05 are broad: S04a matches 353 lines in 75 files, S04d 509 lines in 77 files, S05 300 lines in 42 runtime files and 224 in 28 test files. The files with the most S04a hits are alerts/telegram_notifier.py (36) and signals/webhook_receiver.py (34); those with the most S04d hits are orders/order_placer.py (54), core/state_store.py (48), orders/order_reconciler.py (46) and broker/order_monitor.py (45). INTERPRETATION, from these names only (nothing was read): there "body" is likely a message or request body, and "partial" and "settled" likely order states; and S05, being case-insensitive, may match the plain word "candle" (its synthetic control shows that it can). Their counts are therefore not taken as candle relevance: the read list reads no S04a, S04d or S05 hit on its own; an S04a hit is read only where it falls inside a range located by an S04b or S04c hit (one line: screening/hard_gate.py 382).
- Limits that apply to every result (OQ-02, OQ-10): the expressions are candidate locators, not a complete formula search; a formula written across several lines, through other variable names, or by indexing (for example row[1]) may not match; comments, docstrings and strings match like code; a function reached by getattr, a callback or a re-export may not match S01; a zero means "not found in the inspected scope" for that expression only. Counts are of the pinned files, not of anything that ran.

## 5. Not done

- No legacy content read; no matched line printed; no S03 or S06. The count runs scanned each file of their scope whole, main.py included (its lines 3695-3713 among them), as every count-only search does; a hit there would have been dropped before writing, none fell there, and no line of the range was printed or used. core/analytics_schema.sql is in no authorised scope and was not scanned.
- Git on the reading clone: only the preflight's metadata commands: rev-parse; ls-tree -r HEAD (the standing listing method, which reads tree objects only, all 101 of them stored); cat-file --batch-all-objects --batch-check (stored objects only); hash-object --no-filters --stdin-paths over the 808 working files of the authorised scopes (object IDs only). No checkout, fetch, clone, git status, git grep, ls-tree -l, log or show; ls-remote for the refs. The object counts were unchanged.
- No legacy code, test, import or script run; no host, VM, broker, database, webhook, Chartink or other service contacted; no repository write, commit or push; no governance record changed; no manual drafted.
- Independent check: before delivery a separate agent that did not draft these files checked them read-only (no legacy content). It confirmed every count, blob ID, range and total and raised 9 must-fix and 4 optional points (control labels and one standard for all controls, three unscoped or unsupported statements, the S04a selection, test reads under R07, cap arithmetic, the type-first rule for the regime engine, the git-command statement); all are applied in these bytes, and the final check ran on them.

## 6. Corrections of revision 1

From D005_STAGE1_CHECKPOINT_READ_AUTHORIZATION_RESHARED.txt (67 lines, 7,712 bytes, SHA-256 AAB25B6A1B99AFD6F0AA86C864BEC0C69D606881026C6D185E9A6867DB1F7F56):

- A: the control statuses (section 2): S01 s_rt is NO VALID REAL CONTROL and the "D-004 FACT (SOURCE)" label is withdrawn in sections 2 and 3; S02, S04a, S04b, S04c, S01 s_tests and S05 s_tests are NO VALID REAL CONTROL; the statement "No CONTROL NOT MET occurred" is replaced; hits are candidate locators only. Observed counts unchanged.
- B to D concern the read list: R01b narrowed to 1-2 and 28-31; totals and the Part 2 ceiling recomputed; R04a marked as a checkpoint addition.
- The saved run outputs (search/*.txt) and gate_d005.py are kept as they ran; their control label for S01 s_rt is superseded by this log, not edited.

## 7. Tools (SHA-256)

```
gate_d005.py             946B24138B21C73968B35066068DBE46B1055B3EDDD523226773C7B3DF27BE25
ccount_d005.py           4A1ABA74F8EAF51597579973F14B7FE70560F659271614DA6AE91E7951B2B7A9
gate_test_d005.py        7FBF39AF510957073D0DDA3065795B399BE69C5C906FF04DCBDB5A2280EC44D1
preflight_d005.py        C092A099AE8A440A90C3776F341331BD7D7484C4A6FE6CABF0B4FECD7B82E00C
synth_d005.py            1EF50AD996F9B7ABC3AB774E4C5E039349EA3D077D4D60307A3A881049222CA8
readlist_d005.py         C7F245BAE7F9FE68272D1DFDAB44C8E70C669DB62DB444CD46F1014A6CB9F016
build_s1_docs_d005.py    483A2FC72F4183DB030B34F7F818F3F24FFA4E90857706FA7DB2F084D36597B1
check_s1_d005.py         E3F4145C59586B08EB5BF249C8281AD5A6674552B620CB1ECBDCB5658439161B
count_grep_lines.py      369D3E4376A05584C2E973BA8781E64523DEB4BB8DE3D49381943BC2365488B0
mask_read2.py            BC0B715DF0267BCFD0EA23E17E52E487D16C7F447A92076E0F6AB4AB1C3F17F7
mask_read_lib.py         CCC8D5BE22AE2E928F5E2D3A42B2D70821EBE8EA318DF936DBEAC1EFE9C74BB8
sensitive_check.py       E36C1382B05129E5A2B1382AF60981B9352C8AE4DCA40D29F90629AAC998A337
scope_rules.py           241736B0E0DE12A4BF535E699210D75354DBCA9E8F3FF06306C4B534FC556840
```
count_grep_lines.py, mask_read2.py, mask_read_lib.py, sensitive_check.py and scope_rules.py are the D-004 copies (same SHA-256 as D-004_STAGE1_FILES_READ_AND_COUNT_LOG_27-OCT-2026.txt section 7). The D-005 tools are new, built from ccount_d004.py and cread_d004.py.
