# D-005 Candle analysis - Stage 1: proposed exact list for the third checkpoint

Date of work: 28-Oct-2026, 12:08 to 12:19 IST. Prepared by Claude. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_checkpoint3_proposed_list.md. Status: PROPOSED. Nothing in this file has been counted or read. Basis: the second checkpoint (D005_CHATGPT_REVIEW_PART1_EVIDENCE_AND_PART2_CHECKPOINT_RESHARED.txt, SHA-256 D9EEA6DA49E3F180FD92039FDB8C7FE75A601B2CE8D10D9EB07B07B802B6C1B4), its required response item 4, and the evidence note (2026-10-28_d005_stage1_part2_evidence_note.md).

Budget now: 243 lines used of 900 planned (1,000 absolute), 657 planned lines left; 22 rows used of 50, 28 left.

## A. Count-only runs (proposed again, exact; decision C)

Method as before (ccount_d005.py: gate first; per-file counts and line numbers only, no matched text; object counts and sensitive_check.py before and after; the excluded-range rule). Read-line budget of every run: 0. Real control: none under the agreed standard; the synthetic control verifies the pipeline only; a read-based check is a line printed by an earlier logged read that should match (this stage's evidence, not a frozen record). Hits are candidate locators only; no zero or completeness conclusion. Both are new expressions beyond decision 6's four (checkpoint 1) and need explicit authorisation. In the table, `\|` is the table's escape of the regex's `|`.

| ID | Expression (Python re) | Scope (files) | Purpose (why D-005) | Synthetic control (invented; hit / non-hit lines) | Read-based check |
|---|---|---|---|---|---|
| S07 | `\b_strong_close\b` (case-sensitive) | s_rt (281); s_tests (486) | where the confirmed single-candle predicate is defined and imported, and whether the structure exit has its own definition (G-03): a second definition would be a second single-candle formula. Call lines are counted, not read; their use stays with its owners | hits: `def _strong_close(c, f):`; `from m import _strong_close`; `ok = _strong_close(bar, 0.4)`; non-hits: `strong_close = 1`; `x_strong_close = 2`; `def _strong_closes():` | `sr_detector/retest_confirm.py:130` [025]; `orders/structure_exit_manager.py:284` [023] |
| S08 | `(?i)body_frac` | s_rt (281); s_tests (486) | where body_frac, a single-candle measure by its stated formula (FACT (TEXT), Part 1 MC-09), is computed, read or tested (G-02); a hit in gate code would be classified, not documented (checkpoint 1, decision 2) | hits: `bf = body_frac(c)`; `MIN_BODY_FRAC = 0.4`; `x = cfg.max_body_frac`; non-hits: `body = 1`; `frac = 0.4` | `screening/hard_gate.py:385` [020]; `core/config_loader.py:1416` [026] |

Four runs (each expression in two scopes). Both synthetic controls were tested again for this list against their expressions with Python's re on the invented lines only: every hit line matched and no non-hit line did (check_p2_d005.py, P-07).

## B. Read proposed (exact now)

| ID | Path | Full blob ID | Lines | Evidence | Purpose | Lines read |
|---|---|---|---|---|---|---|
| R11 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | 190; 205; 233; 328 (one line each) | S02r: the hit lines nearest before the S04c hits 191, 207, 245 and 340 (evidence note, section 2); not yet read | to type each by its name and the first line of its signature only (decision H: signature only); no body line; a signature that continues past its first line is not read further, and any such question comes back | 4 |

One row, 4 lines. Limits, stated before the read: S02r is case-insensitive, so a hit line may, less likely, be another line beginning with "def" or "class" (for example in a docstring), which the read would show and record as such; a definition line can carry a body after its colon (a one-line function), which this read would expose; the nearest definition line before a hit need not enclose it (section 2 of the evidence note); a typed name says what a function is called, not what it computes. If a name or first line leaves the type unclear, the read stops there and returns a narrow question (checkpoint 1's execution conditions).

## C. Reads that depend on S07 and S08 (not proposed for authorisation; exact ranges come back at a later checkpoint)

| ID | Path | Basis | Purpose | Cap (estimate) |
|---|---|---|---|---|
| R09 | orders/structure_exit_manager.py (6e4c58d8d2e8aa0438a9461067ae070ed1118950) | S07 hits | G-03: whether the exit defines its own `_strong_close` and, if so, its formula at formula level only; the exit logic not documented | 20 |
| R10 | files with S08 hits outside the two lines already read (at most 3 files) | S08 hits | G-02: the body_frac computation, its zero-range and missing-value handling; any gate condition not documented | 30 |

## D. Totals

Section B: 4 lines and 1 row, giving 247 lines and 23 rows in all. Section C, if later listed at its caps: 50 lines and 2 rows more, giving at most 297 lines and 25 rows. The count runs add no read lines.

## E. Not proposed

- R01c, R02b and R08a (decision E); R01d or any signature continuation of atr or resample (decision F: no additional read is needed now).
- A test-name search for true range (it would serve G-04, which decision E places outside D-005).
- S05' (decision D).
- Reads at the S03 and S06 hit lines, at core/candle_math.py's S04c hits (evidence note G-07a), and at the S04a, S04d and S05 hits (G-08).
- Any repository record or manual draft.
