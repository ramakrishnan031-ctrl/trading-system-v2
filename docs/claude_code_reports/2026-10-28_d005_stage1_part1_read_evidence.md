# D-005 Candle analysis - Stage 1 Part 1: files-read log and source evidence

Date of work: 28-Oct-2026, 11:22 to 11:57 IST. Prepared by Claude for ChatGPT's review. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part1_read_evidence.md; nothing is written to any repository now. Not a manual; no V2 decision.

Authority: D005_STAGE1_CHECKPOINT_READ_AUTHORIZATION_RESHARED.txt (received from Rama at 11:22 IST; 67 lines, 7,712 bytes, SHA-256 AAB25B6A1B99AFD6F0AA86C864BEC0C69D606881026C6D185E9A6867DB1F7F56), decisions 1 to 5, on the corrected copies sent first: the count log revision 1 (2026-10-28_d005_stage1_count_log.md, SHA-256 1025BA280581FF47C9D9E3B0D3DADDB3E7870CEA33B1754F4833E0AE57CE9B45) and the read list revision 1 (2026-10-28_d005_stage1_read_list.md, SHA-256 3179D6AA64F7760E4F52D253C7D77BA46F8530E08DE83C19970EA0B1AEBC4E8F; its data file readlist.json, SHA-256 EC8B5824F9ED682942697B38F0293E447244FFEF50A73ED48AE4536C7A3DD27D). Reading of its item E and "CLAUDE'S NEXT RESPONSE": the corrected copies are returned first and the Part 1 reads follow in the same response; the "correction check" was taken to be Claude's final check of the corrected copies (9 of 9 PASS), which ran before the first read. ChatGPT has not yet checked the corrected copies; the reads below rest on that reading of item E and are open to ChatGPT's check. Part 2 is not begun; its exact list is proposed separately (2026-10-28_d005_stage1_part2_proposed_list.md).

Labels: FACT (SOURCE) - code at the pin, in a line a logged read printed; FACT (TEXT) - a comment or docstring, evidence of its own text only; FACT (TEST) - a test name, evidence of a tested expectation only; FACT (COUNT); HISTORICAL - a frozen V2 record, cited; INTERPRETATION - marked; UNKNOWN / NOT ESTABLISHED; OBSERVATION. Quotations are the masked output of the reads, with the source's non-ASCII characters shown in ASCII (an em or en dash as "-"); a double-quoted quotation that spans lines joins them with one space, without their leading comment marks, while a quotation in backticks is an exact substring of one printed line (parts joined by "..." each so); every legacy citation below in backticks lies inside a printed line of the read named beside it.

## 1. Safety record

- Before the reads (11:26:20 IST): preflight PASS: legacy HEAD and main = the pin; V2 main 2364a99 locally and on GitHub (git ls-remote, a read-only contact with GitHub); the reading clone's HEAD, tree and listing as recorded; sensitive_check.py exit 0; objects 816 blobs, 101 trees, 1 commit; the 808 working files of the authorised scopes equal to their pinned blobs. After the reads (11:29:13 IST): the same, PASS; objects unchanged.
- Read gate (cread_d005.py, built from cread_d004.py): only the rows of readlist.json, checked by SHA-256, can be read, by row ID and range index; no path, range or expression can be typed. Self-test before every read: PASS (the gate_d005 path cases; unknown row, bad range index, a range index on a grep row, a range touching main.py 3695-3713, the Part 1 cap, a wrong blob). Synthetic end-to-end test before the window: a range read and a grep read of an invented file through mr.py, printed-line and mask-footer parsing PASS; four refusals PASS; no log or evidence written by them.
- Every read: tree entry and working file equal to the approved full blob (git ls-tree HEAD -- path; git hash-object --no-filters); mr.py (mask_read2.py and the stage-2 lower-case mask); object counts and sensitive_check.py unchanged after it; output saved as evidence/NNN.txt and logged. No STOP occurred.
- Masking-filter inputs, from the filter's code (mask_read2.py: its EXTRA list at line 38, loaded at line 56), not from a per-read log: on each call the filter loads, value-blind, HR-1 and HR-2 as its host-token source (HR-3 absent). It ran on all 29 reads. Mask counts: a grep read's counts cover the whole file, a range read's its range only.
- Controls of the counts that located the ranges: S01 s_rt (the R07 ranges), S04b and S04c (the R06 ranges) have NO VALID REAL CONTROL (checkpoint decision 4; count log revision 1); their synthetic controls passed, which verifies the search pipeline on synthetic input only. Their hits are used here as candidate locators only; nothing below claims that the hits found are all there are.
- Budget: 21 rows of the 22 approved (R06-03 not read, section 3), 29 read calls; 96 grep-printed lines and 146 range lines, 242 lines in all, of the Part 1 ceiling of 393 (900 planned, 1,000 absolute). Rows used toward the 50-row Stage 1 cap: 21.
- Not done: no read outside the approved rows; no range of main.py 3695-3713 or core/analytics_schema.sql 28-45; no git status, checkout, fetch or filter change; no legacy code, test, import or script run; no service contact other than the git ls-remote above; no repository write.
- Disclosure: the independent review agent that checked these files wrote two temporary hash files to the scratchpad root and deleted them; no evidence file, log or legacy file was changed (each evidence hash in section 6 begins with the 12-character prefix logged at its read).

## 2. Files-read log

| No. | Time (IST) | Row | Mode | Range | Path | Full blob ID | Reader lines | Printed | Masks (read type) | Evidence SHA-256 |
|---|---|---|---|---|---|---|---|---|---|---|
| 001 | 11:26:24 | R01a | G | grep | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 457 | 15 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (grep (whole-file counts)) | 924c0f29d488 |
| 002 | 11:26:33 | R02a | G | grep | tests/unit/test_candle_math.py | bf47cc393815296c498515616628f5c42f3da85f | 507 | 46 | masks: {'HOSTTOK': 0, 'CLIENTID': 1}; stage-2 masks: {} (grep (whole-file counts)) | 98d00eb1bb6d |
| 003 | 11:26:43 | R03a | G | grep | sr_detector/models.py | 4f49fac5386809539578085fc51dbdee5211da00 | 208 | 19 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (grep (whole-file counts)) | 703433562636 |
| 004 | 11:26:46 | R04a | G | grep | data/candle_store.py | f44a831760e62c9d34d4ffc6d420434b0acebf4c | 369 | 16 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (grep (whole-file counts)) | 83855100ac30 |
| 005 | 11:27:01 | R01b | R | 1-2 | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 457 | 2 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 7dc91cd614e6 |
| 006 | 11:27:04 | R01b | R | 28-31 | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 457 | 4 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 8e71815bdf81 |
| 007 | 11:27:15 | R07-01 | R | 1542-1546 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | 2517 | 5 | masks: {'HOSTTOK': 3}; stage-2 masks: {} (range (range counts)) | 79ef57822b75 |
| 008 | 11:27:19 | R07-02 | R | 9-13 | core/daily_stats.py | 36fdf6b07b84888915af31bc71dc1077ef1c8167 | 75 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 0d62a05485a7 |
| 009 | 11:27:22 | R07-02 | R | 19-23 | core/daily_stats.py | 36fdf6b07b84888915af31bc71dc1077ef1c8167 | 75 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 094d3fbd7b3a |
| 010 | 11:27:25 | R07-03 | R | 5-9 | regime/__init__.py | 5522ae3af542f983bda3b45541f90556daa2064e | 54 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 91b1651378e8 |
| 011 | 11:27:28 | R07-04 | R | 13-17 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | 360 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | ce79433ecd53 |
| 012 | 11:27:32 | R07-04 | R | 24-30 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | 360 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 15644f2c0631 |
| 013 | 11:27:41 | R07-05 | R | 14-18 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | 225 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | bc20304bb739 |
| 014 | 11:27:44 | R07-05 | R | 118-122 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | 225 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 1d4a19aa12e2 |
| 015 | 11:27:47 | R07-05 | R | 152-156 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | 225 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | acfa610684c9 |
| 016 | 11:27:51 | R07-06 | R | 37-41 | v3_chain/pb01_entry.py | cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8 | 372 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 2cd6b95a6978 |
| 017 | 11:27:54 | R07-07 | R | 29-33 | v3_chain/pb01_runner.py | e87a5aa964643f1eae5fad90eeb1e0993b5829e0 | 297 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 659f00978987 |
| 018 | 11:27:57 | R07-08 | R | 15-19 | v3_chain/runner.py | a782f91f2f4f1afe12c75072708e2a26316eed3f | 426 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | f6c5cf849aff |
| 019 | 11:28:01 | R07-08 | R | 25-29 | v3_chain/runner.py | a782f91f2f4f1afe12c75072708e2a26316eed3f | 426 | 5 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | f4c2dc1366f5 |
| 020 | 11:28:17 | R06-01 | R | 382-388 | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 467 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 8c597ff5fc3b |
| 021 | 11:28:20 | R06-01 | R | 430-436 | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 467 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 355e617f35c4 |
| 022 | 11:28:23 | R06-02 | R | 8-15 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 673 | 8 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 15ea69f53740 |
| 023 | 11:28:27 | R06-02 | R | 280-288 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 673 | 9 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | c91f4959233c |
| 024 | 11:28:43 | R06-04 | R | 156-162 | sr_detector/flags.py | c5ca5b2514d8941a54090685489f13b0a66d5a39 | 187 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | a1b1475e0fe8 |
| 025 | 11:28:46 | R06-05 | R | 130-136 | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | 138 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 0ebf3315876a |
| 026 | 11:28:50 | R06-06 | R | 1413-1419 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | 2517 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | dd4ea11c1724 |
| 027 | 11:28:53 | R06-07 | R | 2548-2554 | core/state_store.py | 11866117801b9329773a1be605448faef3c96f8d | 3356 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 7d49f76c7896 |
| 028 | 11:28:57 | R06-08 | R | 593-599 | ops_dashboard/backend/services/execution_analytics.py | 5dffbcc2b05cb050281e5bb72ab99d159f66f67f | 930 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 1a1ac354ef66 |
| 029 | 11:29:00 | R06-09 | R | 157-163 | scripts/gemini_trade_coach.py | 6d42f71d5d89e6e48c0584eeeb662caf2883421d | 308 | 7 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range (range counts)) | 0c3ca4c43f4c |

## 3. Rows not read

- R06-03, `sr_detector/models.py` 75-81 (approved): not read. R03a (read 003) had already shown, before R06-03's turn, that the S04c hit line 78 follows `@property` / `def width(self) -> float:` at `sr_detector/models.py:76-77`, inside class `Zone` (`sr_detector/models.py:60`), whose next definition is at 80 (INTERPRETATION from the definition lines' positions: 78 belongs to `Zone.width`). Zone is an S&R record (D-009 to D-012); reading the range would inspect S&R logic and add nothing to D-005. R06-04 (sr_detector/flags.py) was read because no earlier read placed its hit 159 inside a definition; once read, it was classified S&R and its logic is not documented (MC-11).

## 4. Source evidence (claims MC-01 to MC-12)

**MC-01 FACT (SOURCE): the definitions in core/candle_math.py** (read 001; the grep prints definition and constant lines only, so a signature that continues on the next line is shown up to its first line).

| Line | Definition as printed |
|---|---|
| 50 | `ATR_WILDER = "wilder"` |
| 51 | `ATR_SMA = "sma"` |
| 54 | `def true_range_series(candles: Sequence["Candle"]) -> List[float]:` |
| 83 | `def atr(` |
| 136 | `def session_vwap(candles: Sequence["Candle"]) -> Optional[float]:` |
| 165 | `def ema_series(values: Sequence[float], period: int) -> List[float]:` |
| 184 | `def ema(values: Sequence[float], period: int) -> Optional[float]:` |
| 191 | `def adx(candles: Sequence["Candle"], period: int = 14) -> Optional[float]:` |
| 219 | `def _wilder(x: List[float]) -> List[float]:` |
| 250 | `def rsi(candles: Sequence["Candle"], period: int = 14) -> Optional[float]:` |
| 298 | `DAILY_TF = "day"` |
| 310 | `class ResampleResult:` |
| 328 | `def partial_last(self) -> bool:` |
| 335 | `def resample(` |
| 385 | `def _bounds_for(d: "_date") -> Tuple[time, time]:` |

**MC-02 INTERPRETATION (by the printed signatures only; no body read): no printed signature in core/candle_math.py takes a single candle.** The candle functions take a sequence: `true_range_series(candles: Sequence["Candle"]) -> List[float]` (`core/candle_math.py:54`), `session_vwap(candles: Sequence["Candle"]) -> Optional[float]` (`core/candle_math.py:136`), `adx(candles: Sequence["Candle"], period: int = 14) -> Optional[float]` (`core/candle_math.py:191`), `rsi(candles: Sequence["Candle"], period: int = 14) -> Optional[float]` (`core/candle_math.py:250`); `ema_series` and `ema` take `values: Sequence[float]` (`core/candle_math.py:165`, `core/candle_math.py:184`). `atr(` and `resample(` begin at `core/candle_math.py:83` and `core/candle_math.py:335` with signatures not printed, so they are not classified. The one name that suggests a per-bar measure (one bar with the previous close) is `true_range_series`; its formula and edge handling are not read (Part 2 proposal, R01c, flagged there). D-004 recorded true range and ATR at 54-115, VWAP at 136-153, RSI at 250-269 and resample at 311-456 (HISTORICAL: D-004 MC-33, D-004_manual.md:231). Whether the others are window functions is not established until their definitions are read (checkpoint, OQ-01); they are listed by name and line here only.

**MC-03 FACT (TEXT): the module header.** `core/candle_math.py:1-2` (read 005) open the header block with "# Script        : core/candle_math.py". `core/candle_math.py:28-31` (read 006): "# Wiring        : NONE yet (V3 Step 1 is build + unit-test only). Nothing in the live trading / scoring / sizing / S&R path consumes these functions; later modules (03.01 S&R, 03.02 Regime, 03.06 Sizing) wire them in and own the config defaults." The header is evidence of its own text; MC-07 sets the imports found beside it (text against code, Q8; D-004 O-02).

**MC-04 FACT (TEST): the test names in tests/unit/test_candle_math.py** (read 002; 46 names, assertions not read). Those that name the per-bar true range: `test_true_range_series_exact` (`tests/unit/test_candle_math.py:109`), `test_true_range_first_candle_is_high_minus_low` (`tests/unit/test_candle_math.py:114`), `test_true_range_series_empty` (`tests/unit/test_candle_math.py:119`). Others name, by INTERPRETATION from the names only, window behaviour (RSI 50-74, ATR 127-173, VWAP 182-209, EMA 218-242, ADX 271-287) and resample (315-501), among them `test_resample_partial_final_bucket_flagged` (`tests/unit/test_candle_math.py:359`) and `test_resample_empty_bucket_flagged_not_fabricated` (`tests/unit/test_candle_math.py:389`), and `test_session_vwap_single_candle_is_its_typical` (`tests/unit/test_candle_math.py:192`). INTERPRETATION from the name at 114 only: the first candle's true range is expected to equal its high minus its low (no previous close). A test name shows what a test intends to assert, not what the code does at runtime.

**MC-05 FACT (SOURCE): the definition lines of the historical Candle record.** In `sr_detector/models.py` (read 003) the definition lines are `class Candle:` (`sr_detector/models.py:24`), `def from_kite(row: dict) -> "Candle":` (`sr_detector/models.py:34`; body not read) and then `class Pivot:` (`sr_detector/models.py:51`); no other def line and no `@property` line between 24 and 51 was printed. INTERPRETATION: Candle defines no member by a def or `@property` line other than from_kite; whether from_kite computes any measure is not established (body not read). The other `@property` lines in the file belong, by position (INTERPRETATION), to `class Zone` (`sr_detector/models.py:60`, `sr_detector/models.py:72-73`, `sr_detector/models.py:76-77`), `class ScoredZone` (`sr_detector/models.py:87`, `sr_detector/models.py:103-104`) and `class Candidate` (`sr_detector/models.py:129`, `sr_detector/models.py:147-148`), S&R and candidate records outside D-005. The fields of Candle are D-004's (HISTORICAL: MC-31, D-004_manual.md:229). Limit: a member defined otherwise than by a `def` or `@property` line at the line start (for example a lambda attribute) would not be printed by the grep.

**MC-06 FACT (SOURCE): the definition lines around the live CandleData record.** In `data/candle_store.py` (read 004) `class CandleData:` (`data/candle_store.py:25`) is followed by `class _Accumulator:` (`data/candle_store.py:40`) with no def or `@property` line printed between them. INTERPRETATION: CandleData defines no member by a def or `@property` line (the same limit as MC-05). `_Accumulator`'s `def update(self, ltp: float, volume: int = 0) -> None:` (`data/candle_store.py:50`) and the `CandleStore` methods (`data/candle_store.py:66` to `data/candle_store.py:253`) are named here only: building, storing and closing candles is D-004's (HISTORICAL: MC-21 to MC-23; checkpoint decision 3).

**MC-07 FACT (SOURCE) and FACT (TEXT): the runtime references to core.candle_math in the 16 S01-located lines** (S01 s_rt: 16 lines in 9 files; NO VALID REAL CONTROL, candidate locators only). Line 2 of core/candle_math.py is the module's own header (read 005, MC-03); the other 15 lines were read in reads 007 to 019.

| File | Import line (FACT (SOURCE)) | Text beside it (FACT (TEXT)) | Owning topic (by the agenda; not assessed) |
|---|---|---|---|
| core/daily_stats.py | `from core.candle_math import atr, rsi` (`core/daily_stats.py:21`) | "Composes the existing core.candle_math.atr / rsi (no parallel indicators)." (`core/daily_stats.py:10-11`) | D-004 MC-53 (daily stats) |
| regime/engine.py | `from core.candle_math import adx as _adx`, `... import atr as _atr`, `... import ema_series, true_range_series` (`regime/engine.py:26-28`) | "REUSE (no duplication): core.candle_math (ema/adx/atr/true_range_series)" (`regime/engine.py:15`) | D-008 |
| sr_detector/zone_builder.py | `from core.candle_math import session_vwap` (`sr_detector/zone_builder.py:16`) | "Pure functions; stdlib + core.candle_math only." (`sr_detector/zone_builder.py:120`); "vwap  = core.candle_math.session_vwap over the scoped bars;" (`sr_detector/zone_builder.py:154`) | D-009 to D-012 |
| v3_chain/pb01_entry.py | `from core.candle_math import atr` (`v3_chain/pb01_entry.py:39`) | - | C-011, J-010 (BK-57, BK-60) |
| v3_chain/pb01_runner.py | `from core.candle_math import ema` (`v3_chain/pb01_runner.py:31`) | - | C-011, J-010 |
| v3_chain/runner.py | `from core.candle_math import atr, ema` (`v3_chain/runner.py:27`) | "the core.candle_math indicators" (`v3_chain/runner.py:16-17`) | C-011, J-010 |
| regime/__init__.py | no import in the lines read | "Reuses the Common-Utilities substrate (core.candle_math)" (`regime/__init__.py:7`) | D-008 |
| core/config_loader.py | no import: a field comment | `entry_tf: <HOSTTOK> = "5minute"` and, after it, `# the retest timeframe (built via candle_math.resample)` (`core/config_loader.py:1544`) | D-007, D-016 |

- FACT (COUNT) and FACT (SOURCE), within the 16 S01-located lines only: the names imported are atr (4 files), ema (2), rsi, adx, ema_series, true_range_series and session_vwap (1 each). None of the 16 lines imports resample; a reference to it outside those lines, under another name or through getattr, is not covered (not found in the inspected lines only).
- INTERPRETATION: of the names imported in those lines only true_range_series suggests a per-bar measure, and among them only regime/engine.py imports it. Whether any importing path ran is NOT ESTABLISHED (for example `regime/__init__.py:6-7` reads "SHADOW - gates nothing in this step", FACT (TEXT)).
- OBSERVATION (text against code, Q8): the header's "Nothing in the live trading / scoring / sizing / S&R path consumes these functions" (`core/candle_math.py:28-30`) stands beside imports in an S&R module (zone_builder), the regime engine, the V3 chain and core/daily_stats.py. Whether those paths are "live" is not established here; the observation is of text against imports only (D-004 O-02 extended).

**MC-08 FACT (SOURCE): a confirmed single-candle measure, `_strong_close`** (read 025):

```
130: def _strong_close(c, frac: float, is_long: bool = True) -> bool:
131:     """Directional strong close: LONG wants the close in the UPPER frac of the 1m
132:     range, SHORT in the LOWER frac."""
133:     rng = c.high - c.low
134:     if rng <= 0:                                       # degenerate (flat) bar
135:         return c.close >= c.high if is_long else c.close <= c.low
136:     pos = (c.close - c.low) / rng if is_long else (c.high - c.close) / rng
```

- Inputs: one candle-like object `c` with high, low and close; a fraction `frac`; a direction `is_long` (`sr_detector/retest_confirm.py:130`). Range: `rng = c.high - c.low` (`sr_detector/retest_confirm.py:133`). Zero or negative range (high <= low): the "degenerate (flat) bar" branch returns `c.close >= c.high` for a long and `c.close <= c.low` for a short (`sr_detector/retest_confirm.py:134-135`). Otherwise the close's position from the favourable end: `(c.close - c.low) / rng` for a long, `(c.high - c.close) / rng` for a short (`sr_detector/retest_confirm.py:136`).
- UNKNOWN: how `pos` is compared with `frac` (the return after line 136, not read; Part 2 proposal R06b). The docstring says "LONG wants the close in the UPPER frac of the 1m range, SHORT in the LOWER frac." (`sr_detector/retest_confirm.py:131-132`, FACT (TEXT) only).
- INTERPRETATION: no None or type guard appears in the lines read; a missing high, low or close would raise in the subtraction at 133.
- Owner of its use: the retest confirmation (D-015); not documented here.

**MC-09 FACT (TEXT): a body-to-range measure, body_frac, named in text only** (reads 020 and 026). The line `screening/hard_gate.py:385`, text inside a docstring by its context (INTERPRETATION: the string's opening quotes lie before the range read), states `body_frac = |close-open| / (high-low)`, and the comment on `core/config_loader.py:1416` states `body_frac=|close-open|/(high-low)`. No code that computes body_frac was located: S04c's hits in screening/hard_gate.py are 385 (above) and 433, a line of the docstring opened at 432 (read 021). Its zero-range and missing-value handling are UNKNOWN (Part 2 proposal, S08). The rest of both lines and their neighbouring lines (gate conditions, a threshold and a configuration default) are outside D-005 and not documented (checkpoint decision 2).

**MC-10 FACT (TEXT): a strong-close predicate named in the structure exit's docstrings** (reads 022 and 023). The four S04b hits are text lines, each with the word bearish or bullish: `orders/structure_exit_manager.py:11-12` (the module docstring by its context, INTERPRETATION: its opening quotes lie before the range read) and `orders/structure_exit_manager.py:283` and `orders/structure_exit_manager.py:285` (the docstring of `_is_confirmed_break`, defined at `orders/structure_exit_manager.py:281`), which names `_strong_close` at `orders/structure_exit_manager.py:284`. Whether that is MC-08's function is NOT ESTABLISHED (Part 2 proposal, S07). The exit logic, from `orders/structure_exit_manager.py:286`, is outside D-005 and not documented.

**MC-11 Classified: not a single-candle measure (outside D-005; logic not documented).**

| Read | Lines | Classification |
|---|---|---|
| 024 | `sr_detector/flags.py:159`, `sr_detector/flags.py:161-162` | S&R (D-009 to D-012) |
| 021 | `screening/hard_gate.py:432-436` | a gate docstring (J-010) |
| 027 | `core/state_store.py:2551` | a column name; not a formula |
| 028 | `ops_dashboard/backend/services/execution_analytics.py:596` | not a candle measure |
| 029 | `scripts/gemini_trade_coach.py:160-161` | a report string; not a formula |
| not read | sr_detector/models.py, the S04c hit at line 78 | by position only: inside Zone.width (read 003; section 3); S&R |

**MC-12 OBSERVATION: masking.** Read 007: HOSTTOK 3; the three masks stand at the same position in `core/config_loader.py:1542-1544`, after each field name and its colon; possible over-match, not assessed. Read 002: CLIENTID 1 somewhere in tests/unit/test_candle_math.py (a grep's counts cover the whole file); no printed line shows it.

## 5. Gaps and open hits after Part 1

| ID | Gap | Status | Next |
|---|---|---|---|
| G-01 | the comparison of pos with frac in `_strong_close` | UNKNOWN | Part 2, R06b (one line) |
| G-02 | the code that computes body_frac, its zero-range and missing-value handling | UNKNOWN | Part 2, S08, then a located read at a third checkpoint |
| G-03 | the source of the structure exit's `_strong_close` | NOT ESTABLISHED | Part 2, S07 |
| G-04 | true_range_series's formula and edge handling (54-82) and what its tests assert | UNKNOWN | Part 2, R01c (flagged) and R02b |
| G-05 | the signatures of atr and resample beyond their first lines | not printed | Part 2, R01d |
| G-06 | whether any importing path ran | NOT ESTABLISHED | outside Stage 1 (no runtime evidence is in scope) |
| G-07 | S04c hits not read: regime/engine.py 191, 207, 245, 340; core/candle_math.py 19 and 21 (in the header, outside 1-2 and 28-31), 57, 61, 75 and 77 (between the def lines at 54 and 83, by position), 101 and 117 (between 83 and 136), 194 and 214 (between 191 and 219); screening/step_executor.py 327 (cited, HISTORICAL: D-003 MC-33); sr_detector/models.py 78 (not read; classified by position, section 3) | open, not typed (models.py 78: S&R by position) | regime/engine.py: Part 2, S03 and S02r (type first, OQ-01); core/candle_math.py 57 to 77: R01c; the others: none proposed |
| G-08 | S04a (353 lines in 75 files), S04d (509 in 77) and S05 (s_rt 300 in 42; s_tests 224 in 28) hits outside the ranges read | open; candidate locators only | none proposed (checkpoint: not read merely because they matched) |

No list here is complete: the counts behind it have no valid real control and were used as candidate locators only.

## 6. Evidence files (SHA-256)

```
001.txt  924C0F29D48801A6DA74DB0B6AA68EE5CE16FCCEC0C85606982F585BEEE49EF0
002.txt  98D00EB1BB6D512706ACB47C9E25BC165EFEEFA71A988EFA4F6514811F7C5BA7
003.txt  7034335626360DA8A0E640EC6E584B738D7D6C346CA8579289EF0EEFB4E58124
004.txt  83855100AC307D340BDE0F760529A615DE5B7599FDCF5181D544AC33511D3899
005.txt  7DC91CD614E6F99257EEAC3D7B2E7AF25915B769D53FE32835FBA97D8DE01A2D
006.txt  8E71815BDF818FA76D6B7EB716A56E8D14A68BA124EADD112A0DA067071A1A7A
007.txt  79EF57822B75B8AAD7D50EB25D4E003F370CCD99395E0A18F04AAABD15100EF7
008.txt  0D62A05485A70A15D2E02619916DE8AF3A1C2CF23B5A93D6B9E1A7B4E9D54A23
009.txt  094D3FBD7B3A8866098149A7260FFDFE2F2A2C741910C5F3DCF399E914BCF06C
010.txt  91B1651378E8E06DE3068A17D4E8DA929E18AA124C43448374DDBF8F22E49346
011.txt  CE79433ECD53AEDC5A898451FA16BC84C35505F668B3D984A780DDAC0F85D290
012.txt  15644F2C06315C6B9B2F958B0A06943697EC33E987653A7AB871B8502F98B507
013.txt  BC20304BB7399C9B2695D19C3D91EF0C08E6423E3FE0709772E0FBE7265CBA1E
014.txt  1D4A19AA12E21A576C679F469EE24CED459246622D86A8D1D579771B52A84E2E
015.txt  ACFA610684C9A5C662580883BC8B21AC7C8113C7C8A136A102E9F0B3EE5FD2A5
016.txt  2CD6B95A697867D0366D0CC34E3CC584E9004BBBE817F3BCAC7AAD67DBA6ECBD
017.txt  659F009789872569EBFDBE8BBBC3735847680DD4CCB15495AA2659BE60E6B57F
018.txt  F6C5CF849AFF7D3B8E28017A686F3D6CE7051484A4E26A2DB00CEF4F5700208F
019.txt  F4C2DC1366F5A620E8EFDD0280F426BE8BC61921B1CF18E5A3D7698E126E949C
020.txt  8C597FF5FC3B33EE2CA34104CFA2A5570CD6B0E1DC524B1F908C4285E6119C0A
021.txt  355E617F35C4A08987310EB981F92A2E03F7D1BAB09B87EF6969FB35107CB245
022.txt  15EA69F53740411F47DCD955DC52FE790A703FF0CF5DA1559842B7365EB8B26F
023.txt  C91F4959233C980ED065BE600671F30072FA621BA97ABB71CF3F77A4DFCBDA18
024.txt  A1B1475E0FE893461E918793CD11C1227357F08CAE24A5B922631E37BB3334D2
025.txt  0EBF3315876A1613E826B2DE8D662F0CA2671909F9C56EF99E1E5A36DB7A72AF
026.txt  DD4EA11C17249A9F6C11E2E0F5E83B106B6A667A59C3D734B7671DDE79B13AFC
027.txt  7D49F76C789653E856A0D91B676E7E05A86D6B01BB196D2FBF58EDE37D4EE430
028.txt  1A1AC354EF66AAF4ACC7CB508D615E96D56B84CD3737CD394DC3109F2C21C4C6
029.txt  0C3CA4C43F4CF1BA3D18ADC1C62901FE75F9BA1A6CF9FC27007AC5B67C1CD263
```
