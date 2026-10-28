# D-005 Candle analysis - Stage 1 proposed exact read list (for the ChatGPT checkpoint)

Date of work: 28-Oct-2026, 11:00 to 11:24 IST. Prepared by Claude. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_read_list.md. Companion: 2026-10-28_d005_stage1_count_log.md (the evidence for every range).

Status: revision 1. Part 1 APPROVED by D005_STAGE1_CHECKPOINT_READ_AUTHORIZATION_RESHARED.txt (decisions 1 to 5), with its corrections B to D applied here: R01b narrowed to 1-2 and 28-31, the totals and the Part 2 ceiling recomputed, R04a marked as added at the checkpoint. Part 2 is NOT authorised (decision 6). Revision 0 was delivered at 11:03 IST.

Rules applied (Stage 0 plan section 9, the authorisation's technical clarifications and the independent check): every range comes mechanically from count hit lines, by one of two rules, clipped to the file, with clusters joined only where 3 lines or fewer separate them. R07: each S01 hit line in a runtime (s_rt) file, +/- 2. R06: each S04b or S04c hit line, +/- 3; S04a, S04d and S05 hits are not used to locate ranges. A grep read prints only its matched lines (masked), at most 60. Every read goes through the gate and mr.py (mask_read2.py and the stage-2 mask), is checked against its pinned blob first and is logged with path, full blob, range, basis (this list's ID), time, mask counts with read type and saved-output SHA-256; main.py 3695-3713 and core/analytics_schema.sql 28-45 are refused. "File lines" are newline counts (wc -l), one below the Stage 0 and D-004 figures, which count the reader's lines (V2_DESIGN_LOG.md:139).

## Part 1 - proposed for reading now (exact)

22 read rows (4 grep reads and 18 range rows holding 26 ranges); 153 range lines; the grep reads print at most 240 lines. Counting the grep lines, Part 1 is at most 393 lines of the 900 planned (1,000 absolute). A read is one row (decision 5): 22 of the 50 rows allowed for the whole Stage 1 sequence.

| ID | Path | Full blob ID | File lines | Mode and exact range or expression | Count or grep evidence | Purpose | Lines | Cap |
|---|---|---|---|---|---|---|---|---|
| R01a | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 456 | G: `^\s*(def\|class)\s\|^[A-Z_][A-Z0-9_]*\s*=` | S02: 12 def/class lines (54, 83, 136, 165, 184, 191, 219, 250, 310, 328, 335, 385) | Q2: the function and constant map (names and signatures only) | - | 60 printed lines |
| R02a | tests/unit/test_candle_math.py | bf47cc393815296c498515616628f5c42f3da85f | 506 | G: `^\s*(def test_\|class Test)` | S01 s_tests: 1, 3, 17; S05 s_tests: 8 lines | Q7: the test map (names only) | - | 60 printed lines |
| R03a | sr_detector/models.py | 4f49fac5386809539578085fc51dbdee5211da00 | 207 | G: `^\s*(def\|class)\s\|^\s*@property` | S02: 15 def/class lines; S05: 24, 34, 36; S04c: 78 | Q1: the Candle record's members (names only); some printed lines lie in 22-45, read for D-004 (OQ-03) | - | 60 printed lines |
| R04a | data/candle_store.py | f44a831760e62c9d34d4ffc6d420434b0acebf4c | 368 | G: `^\s*(def\|class)\s\|^\s*@property` | S02: 16 def/class lines; S05: 37 lines | Q1: the CandleData record's members (names only); some printed lines lie in ranges read for D-004 (OQ-03); a grep read not in the Stage 0 plan (R04 was range reads only): DECISION | - | 60 printed lines |
| R01b | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 456 | R: 1-2, 28-31 | S01: 2; D-004 O-02 cites the header at 28-31 (D-004_manual.md:232) | Q8: the module docstring's first lines (1-2, read by no topic) and the "Wiring: NONE yet" header (28-31, re-read under OQ-03); narrowed from 1-31 by checkpoint correction B | 6 | 6 |
| R07-01 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | 2516 | R: 1542-1546 | S01 s_rt: 1544 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 5 | 30 |
| R07-02 | core/daily_stats.py | 36fdf6b07b84888915af31bc71dc1077ef1c8167 | 74 | R: 9-13, 19-23 | S01 s_rt: 11, 21 (D-004 read 1-75; OQ-03 re-read) | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 10 | 30 |
| R07-03 | regime/__init__.py | 5522ae3af542f983bda3b45541f90556daa2064e | 53 | R: 5-9 | S01 s_rt: 7 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 5 | 30 |
| R07-04 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | 359 | R: 13-17, 24-30 | S01 s_rt: 15, 26, 27, 28 (D-004 read 1-40; OQ-03 re-read) | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 12 | 30 |
| R07-05 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | 224 | R: 14-18, 118-122, 152-156 | S01 s_rt: 16, 120, 154 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 15 | 30 |
| R07-06 | v3_chain/pb01_entry.py | cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8 | 371 | R: 37-41 | S01 s_rt: 39 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 5 | 30 |
| R07-07 | v3_chain/pb01_runner.py | e87a5aa964643f1eae5fad90eeb1e0993b5829e0 | 296 | R: 29-33 | S01 s_rt: 31 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 5 | 30 |
| R07-08 | v3_chain/runner.py | a782f91f2f4f1afe12c75072708e2a26316eed3f | 425 | R: 15-19, 25-29 | S01 s_rt: 17, 27 | Q6: the import or reference line and its neighbours (which names are imported, how the module is referenced) | 10 | 30 |
| R06-01 | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 466 | R: 382-388, 430-436 | S04c: 385, 433 (S04a 382 falls inside 382-388). INTERPRETATION from D-002 MC-18's line numbers (annotations at 369, 380, 422, 431; D-002_manual.md:197): the hits lie in gate_confirm and gate_pullback, whose bodies D-002 did not read and whose detail it gives to J-010: DECISION | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 14 | 40 |
| R06-02 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 672 | R: 8-15, 280-288 | S04b: 11, 12, 283, 285 (exits: later sections) | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 17 | 40 |
| R06-03 | sr_detector/models.py | 4f49fac5386809539578085fc51dbdee5211da00 | 207 | R: 75-81 | S04c: 78 (S02 has def lines at 77 and 80; that 78 belongs to the def at 77 is an INTERPRETATION) | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-04 | sr_detector/flags.py | c5ca5b2514d8941a54090685489f13b0a66d5a39 | 186 | R: 156-162 | S04c: 159 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-05 | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | 137 | R: 130-136 | S04c: 133 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-06 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | 2516 | R: 1413-1419 | S04c: 1416 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-07 | core/state_store.py | 11866117801b9329773a1be605448faef3c96f8d | 3355 | R: 2548-2554 | S04c: 2551 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-08 | ops_dashboard/backend/services/execution_analytics.py | 5dffbcc2b05cb050281e5bb72ab99d159f66f67f | 929 | R: 593-599 | S04c: 596 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |
| R06-09 | scripts/gemini_trade_coach.py | 6d42f71d5d89e6e48c0584eeeb662caf2883421d | 307 | R: 157-163 | S04c: 160 | Q3, Q4: classify each S04b or S04c hit (single-candle formula, multi-candle predicate, series computation, or not a candle measure); record a confirmed single-candle formula at formula level only | 7 | 40 |

- Cited, not read (HISTORICAL): screening/step_executor.py 318-329 (D-003 MC-33, MC-39; the S04a hits 321, 326, 328, the S04c hit 327 and the S05 hit 321 fall inside it); config/system_config.yaml:628 (quoted verbatim in C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt:977); the candle-state words in core/candle_math.py (resample's partial flag, D-004 MC-33), data/candle_store.py (is_synthetic, MC-21, MC-23) and v3_chain/watchlist_capture.py (the settled-only guard, C-011 section 7.3).
- Re-reads under OQ-03: R01b (3-31 read for D-004), R07-02 (core/daily_stats.py, D-004 read 1-75), R07-04 (regime/engine.py, D-004 read 1-40) and the lines R03a and R04a print inside D-004's read ranges; each is logged with this list as its basis, and no D-004 fact is restated as new.
- R06-01 lies, by INTERPRETATION from D-002 MC-18's line numbers, in the playbook gates whose bodies D-002 did not read and whose detail it gives to J-010. Approved narrowly (decision 2): the two ranges classify the located hits only; a confirmed single-candle measure is recorded at formula level (expression, inputs, edge handling); a multi-candle predicate, series computation, gate condition or downstream decision is marked outside D-005 and not documented.
- R04a was added at this checkpoint by ChatGPT's explicit decision 3; it was not in the Stage 0 plan (whose R04 was range reads only). Its output and notes are limited to class, function and property names and signatures; no body, ingestion, storage, retention or lifecycle logic (D-004, K-011).

## Part 2 - after Part 1, at a second checkpoint (not proposed for reading now)

Each row's exact ranges depend on Part 1's grep output or on S03 and S06; they come back as an exact list before any of them is read.

| ID | What | Exact basis available now | Cap (lines) |
|---|---|---|---|
| S03 | count-only: the module's function names (from R01a and the R07-04 import lines) over s_rt | the expression text is logged before it runs | - |
| S06 | count-only: the same names over s_tests | as S03 | - |
| S05' | count-only: S05 case-sensitive (`\bCandleData\b\|\bCandle\b\|\bfrom_kite\b`) | a new expression; needs authorisation | - |
| S02r | count-only: def and class lines of regime/engine.py, to type the functions around its S04c hits (191, 207, 245, 340) before any read there (OQ-01, type first) | a new scope for S02; needs authorisation | - |
| R01c | definition reads of the functions R01a shows to be single-candle | the def spans from S02: 54-82, 83-135, 136-164, 165-183, 184-190, 191-218, 219-249, 250-309, 310-327, 328-334, 335-384, 385-456 | 120 |
| R01d | each series function's def line and input-guard lines only (OQ-01) | the same spans | 30 |
| R02b | test functions that name a single-candle function | R02a's printed lines | 100 |
| R03b | Candle members that compute from OHLC | S02 spans in sr_detector/models.py (for example 73-76, 77-79, 80-86) and R03a | 40 |
| R04b | CandleData members, if R04a shows any outside D-004's read ranges | R04a | 30 |
| R07b | call lines that S03 locates, +/- 2 | S03 | 60 |
| R08 | tests that assert a confirmed single-candle function, and the test files' S01 reference lines (tests/unit/test_candle_math.py 1-5 and 15-19; tests/unit/test_v3_chain.py 15-19), moved here from R07 because R07 covers runtime files only | S06, R02a; the S01 s_tests hits 1, 3, 17 and 17 | 80 |

Part 2 ceiling, recomputed from the revised Part 1 (at most 393 lines, grep lines counted): at most 507 lines within the 900 planned, and never past 607 lines against the 1,000 absolute stop. The caps above add up to 460 lines, so the planned total is at most 853. Part 2 is proposed at the second checkpoint; nothing in it is authorised now (decision 6).

## Part 3 - not proposed (counted, not read)

- S04a hits not inside an S04b or S04c range: in the 70 runtime files that have no range here and are not cited (75, less the four files with an R06 range and screening/step_executor.py), main.py among them, and in screening/hard_gate.py 14, 67, 407, 412, orders/structure_exit_manager.py 33, 336, core/state_store.py 347 and scripts/gemini_trade_coach.py 208. A narrower S04a (for example without `\bbody\b`) could be proposed at the second checkpoint.
- regime/engine.py S04c hits 191, 207, 245, 340: held until the enclosing functions are typed (S02r), because D-004 O-02 records ema and adx imports there and OQ-01 forbids reading series formulas; 15 and 28 are inside R07-04's ranges.
- S04d hits outside the cited candle files, and all S05 hits (pending S05').
- main.py: S04a 31 lines, S04d 6, S05 15; wiring is Q6's, reached through S03 and R07b, with lines 3695-3713 refused.
- core/candle_math.py S04c and S04d hit lines: not read +/- 3, because they lie inside functions whose type R01a must show first (OQ-01).

## Decisions recorded at the checkpoint

From D005_STAGE1_CHECKPOINT_READ_AUTHORIZATION_RESHARED.txt (SHA-256 AAB25B6A1B99AFD6F0AA86C864BEC0C69D606881026C6D185E9A6867DB1F7F56):

1. Part 1: approved with corrections; no other path or range.
2. R06-01: approved narrowly, to classify the located hits only.
3. R04a: approved narrowly, names and signatures only.
4. Real-control standard: the identifier half confirmed; a paraphrase is not a control; the regime import is not a valid real control.
5. Read count: one row = one read; at most 50 rows for the combined Stage 1 sequence, within 900 planned and 1,000 absolute lines, grep lines counted.
6. Part 2: a second checkpoint with an exact list; S03, S06, S05' and S02r may be proposed, none is authorised now.
