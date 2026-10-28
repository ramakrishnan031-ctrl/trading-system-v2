# D-005 Candle analysis - Stage 1 Part 2: proposed exact list (for the second checkpoint)

Date of work: 28-Oct-2026, 11:29 to 11:57 IST. Prepared by Claude. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part2_proposed_list.md. Status: PROPOSED. Nothing in this file has been counted or read; Part 2 is not authorised (checkpoint decision 6). Basis: the Part 1 evidence (2026-10-28_d005_stage1_part1_read_evidence.md, its read numbers in brackets) and the count log revision 1.

Budget after Part 1: 242 lines used of 900 planned (1,000 absolute), so at most 658 lines remain planned (758 before the absolute stop); 21 rows used of 50, so 29 remain.

## A. Count-only runs proposed (exact)

Method for each run: ccount_d005.py's method (gate first; per-file counts and line numbers only, no matched text; object counts and sensitive_check.py before and after; the excluded-range rule of OQ-05). Read-line budget of every run: 0 (a count prints no source line). Real control: none exists under the agreed standard (no frozen V2 record quotes a code-level hit for any of these expressions). The synthetic control (invented lines, written into a scratch file and searched by the same code) verifies the pipeline only. Where a line printed by a Part 1 read should match, it is offered as a read-based check (this stage's own evidence, not a frozen record), for ChatGPT to accept or not. Hits are candidate locators only; no zero or completeness conclusion.

Against decision 6: S03 and S06 are on its list, each narrowed from "the module's function names" to one name, true_range_series (the window functions' call sites are not proposed: OQ-01). S02r is on its list, as described in the read list. S07 and S08 are new expressions beyond its four (S03, S06, S05', S02r) and need explicit authorisation. S05' is not proposed (below). In the table, `\|` is the table's escape of the regex's `|`.

| ID | Expression (Python re) | Scope (files) | Purpose | Synthetic control (invented; hit lines / non-hit lines) | Read-based check |
|---|---|---|---|---|---|
| S03 | `\btrue_range_series\b` (case-sensitive) | s_rt (281) | which runtime lines name the per-bar function; its hits are a subset of S04c's (every such line contains "true_range"), so it separates the S04c lines that name it, among them any of regime/engine.py 191, 207, 245, 340 | hits: `from m import ema_series, true_range_series`; `tr = true_range_series(bars)`; non-hits: `def test_true_range_series_exact():`; `true_range = 1` | `core/candle_math.py:54` [001]; `regime/engine.py:15` [011]; `regime/engine.py:28` [012] |
| S06 | as S03 | s_tests (486) | which test lines name it (calls and imports) | as S03 | none: the test names at 109, 114 and 119 [002] contain it inside a longer identifier, which `\b` does not match |
| S02r | `(?i)^\s*(def\|class)\s` (S02's expression) | regime/engine.py (1 file; a new scope) | the line numbers of the file's definition lines, to place S04c's hits 191, 207, 245, 340 between them by position only (OQ-01: type first); no name is printed | hits: `def f(x):`; `    def g(self):`; `class K:`; non-hits: `# def f`; `x = define(y)` | none: no def line of the file was printed in Part 1 |
| S07 | `\b_strong_close\b` (case-sensitive) | s_rt (281); s_tests (486) | where the single-candle predicate is defined, imported and called; the structure exit's source (G-03) | hits: `def _strong_close(c, f):`; `from m import _strong_close`; `ok = _strong_close(bar, 0.4)`; non-hits: `strong_close = 1`; `x_strong_close = 2`; `def _strong_closes():` | `sr_detector/retest_confirm.py:130` [025]; `orders/structure_exit_manager.py:284` [023] |
| S08 | `(?i)body_frac` | s_rt (281); s_tests (486) | where body_frac is computed, read or tested (G-02) | hits: `bf = body_frac(c)`; `MIN_BODY_FRAC = 0.4`; `x = cfg.max_body_frac`; non-hits: `body = 1`; `frac = 0.4` | `screening/hard_gate.py:385` [020]; `core/config_loader.py:1416` [026] |

Seven runs (S07 and S08 in two scopes each). Each synthetic control above was tested beforehand against its expression with Python's re on these invented lines only: every hit line matched and no non-hit line did.

Not proposed:
- S05' (`\bCandleData\b|\bCandle\b|\bfrom_kite\b`, case-sensitive). Its purpose was to locate where the candle records are used. Its hits would be the records' use sites across the ingestion, storage, S&R, regime and entry paths, and reading them to look for a measure would inspect those topics' logic, which the checkpoint excludes; S05's counts (s_rt 300 lines in 42 files; s_tests 224 in 28) show the scale. A measure computed at a use site in the shapes D-005 looks for is targeted by S04b and S04c instead, within their limits (NO VALID REAL CONTROL; candidate locators only); a use-site measure in another shape stays uncovered (an open limit, Part 1 G-08). ChatGPT may call for S05'.
- A narrower S04a; call-site counts for atr, ema, adx, rsi and session_vwap (OQ-01).

## B. Reads proposed (exact now)

| ID | Path | Full blob ID | Range | Evidence | Purpose | Lines |
|---|---|---|---|---|---|---|
| R01c | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 54-82 | `def true_range_series(candles: Sequence["Candle"]) -> List[float]:` at 54 and `def atr(` at 83 [001]; S02 54, 83 | Q2, Q3: the per-bar true range: formula, the first candle, empty input, rounding, errors (G-04). FLAGGED, see below | 29 |
| R01d | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 83-90; 335-344 | `def atr(` at 83 and `def resample(` at 335, their signatures not printed [001] | Q2, Q9: the two signatures only (G-05; OQ-01); a body line printed in the range is not recorded | 18 |
| R02b | tests/unit/test_candle_math.py | bf47cc393815296c498515616628f5c42f3da85f | 109-126 | the three true-range tests at 109, 114, 119 and the next test at 127 [002] | Q7: what they assert (G-04) | 18 |
| R08a | tests/unit/test_candle_math.py | bf47cc393815296c498515616628f5c42f3da85f | 15-49 | S01 s_tests 17; the first test at 50 [002] | Q7: the imports and the candle builders the true-range tests use | 35 |
| R06b | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | 137 | read 025 ends at 136 inside `_strong_close`; the file has 137 lines (newline count) | Q3: how pos is compared with frac (G-01) | 1 |

Five rows, 101 lines.

- R01c is FLAGGED for ChatGPT's judgement: it reads the whole body of a function that returns a series (54-82). The checkpoint excludes series formulas and, for a function whose type is unclear, asks for a narrow question rather than a body read. The question it would answer: is each element of true_range_series computed from one candle and the previous candle's close only (a per-bar measure), and how are the first candle and empty input handled? ChatGPT may approve R01c, narrow it, or rule true_range_series outside D-005, in which case G-04 closes as outside and R02b and R08a fall with it.
- R01c and R01d are re-reads under OQ-03: D-004 recorded core/candle_math.py 54-115 and 311-456 (HISTORICAL: D-004_manual.md:231, MC-33). Each would be logged with OQ-03 as its basis, and no D-004 fact would be restated as new.

## C. Reads that depend on the counts of section A (sizing only; not proposed for authorisation)

Their exact ranges come back as an exact list at a third checkpoint, after the counts; the caps below are estimates for the budget only.

| ID | Path | Basis | Purpose | Cap |
|---|---|---|---|---|
| R09 | orders/structure_exit_manager.py (6e4c58d8d2e8aa0438a9461067ae070ed1118950) | S07 hits | G-03: which `_strong_close` the exit uses and, if it is its own, its formula; the exit logic itself not documented | 20 |
| R10 | files with S08 hits outside the two lines already read (at most 3 files) | S08 hits | G-02: the body_frac computation, its zero-range and missing-value handling; any gate condition not documented (decision 2) | 30 |
| R11 | regime/engine.py (d9cdd9366857163ba5db1358585c57ea49d82c2f) | S02r and S03 | the definition lines (signature only) that enclose the S04c hits 191, 207, 245, 340, so that each enclosing function can be typed before anything else (OQ-01) | 8 |

## D. Totals

Section B: 101 lines and 5 rows. Section C, if later listed at its caps: 58 lines and 3 rows. With Part 1 (242), the Stage 1 total would be at most 401 lines of the 900 planned, and 29 of the 50 rows. The count runs of section A add no read lines.

## E. Not proposed

- Callers' call lines of atr, ema, adx, rsi and session_vwap, and any read in regime/engine.py, the S&R detector, the V3 chain or the entry gates beyond sections B and C: not proposed now; for regime/engine.py any further read depends on R11's typing and returns at the third checkpoint.
- core/candle_math.py beyond R01c and R01d, including the S04c hits 101 and 117 (after atr's first line) and 194 and 214 (after adx's first line): open (Part 1 G-07); reading inside atr or adx would read series formulas if they are window functions, which is not established (OQ-01).
- The S04a, S04d and S05 hits outside the ranges read (Part 1 G-08).
- Any repository record (OQ-06).
