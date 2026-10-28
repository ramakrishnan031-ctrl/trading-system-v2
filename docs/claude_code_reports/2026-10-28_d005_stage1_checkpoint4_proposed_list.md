# D-005 Candle analysis - Stage 1: proposed exact list for the fourth checkpoint

Date of work: 28-Oct-2026, 12:31 to 12:40 IST. Prepared by Claude. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_checkpoint4_proposed_list.md. Status: PROPOSED. Nothing in this file has been counted or read. Basis: the third checkpoint (D005_CHECKPOINT3_CLAUDE_INSTRUCTIONS_FRESH_LINK.txt, SHA-256 2900C12585256BB85170959D00C2209E6E215BBA8747D4D7BC081DF04CEABCEA), required response item 4, and the count log (2026-10-28_d005_stage1_part3_count_log.md).

Budget now: 243 lines used of 900 planned (1,000 absolute), 657 planned lines left; 22 rows used of 50, 28 left.

## A. Count-only runs proposed (new expressions; each needs explicit authorisation)

These replace R09 and R10. Decisions 7 and 8 asked that nothing expose exit decision logic, gate conditions or a function body. Each run below narrows an S07 or S08 hit list by the shape of the line alone, so it prints no source line.

Method as before: ccount_d005.py, which runs the gate first and prints per-file counts and line numbers only, with no matched text. Object counts and sensitive_check.py are checked before and after, and the excluded-range rule applies. The read-line budget of every run is 0.

Real control: none under the agreed standard. The synthetic control verifies only the pipeline. A read-based check is a line printed by an earlier logged read that should match; it is not a frozen record. Hits are candidate locators only, and no zero or completeness conclusion is drawn. In the table, `\|` is the table's escape of the regex's `|`.

| ID | Expression (Python re) | Scope (files) | Purpose (why D-005) | Synthetic control (invented; hit / non-hit lines) | Read-based check |
|---|---|---|---|---|---|
| S07d | `^\s*def\s+_strong_close\b\|^\s*_strong_close\s*=(?!=)` (case-sensitive) | s_rt (281) | Which S07 lines define or bind the name `_strong_close` (G-03). A located line, for example in orders/structure_exit_manager.py at 70, 290 or 293, could be a second definition or an alias; which is not established without a read. | hits: `def _strong_close(c, f):`; `    def _strong_close(self, c):`; `_strong_close = lambda c, f: True`; non-hits: `x = _strong_close(c, 0.4)`; `# def _strong_close`; `def _strong_closes():`; `_strong_close == y` | `sr_detector/retest_confirm.py:130` [025]; no printed exit line should match (284 [023] does not) |
| S07i | `^\s*from\s+sr_detector\.retest_confirm\s+import\s.*\b_strong_close\b` (case-sensitive) | s_rt (281) | Which S07 lines are one-line imports of the name `_strong_close` from MC-08's module (G-03); an import is not evidence of use. | hits: `from sr_detector.retest_confirm import _strong_close`; `from sr_detector.retest_confirm import RetestConfirm, _strong_close`; non-hits: `from sr_detector.models import _strong_close`; `import sr_detector.retest_confirm`; `# from sr_detector.retest_confirm import _strong_close`; `from sr_detector.retest_confirm import _strong_closes` | none: no import line was printed |
| S08a | `(?i)\bbody_frac\b\s*=(?!=)\|^\s*def\s+\w*body_frac\w*\s*\(` | s_rt (281) | Which S08 lines assign body_frac or define a function named for it, the candidate places where the measure is computed (G-02). | hits: `body_frac = abs(c - o) / rng`; `def _body_frac(c):`; `def body_frac(c) -> float:`; `x = f(body_frac=0.4)`; non-hits: `if body_frac >= m:`; `body_frac == 0`; `min_body_frac = 0.4`; `y = body_frac` | `screening/hard_gate.py:385` [020] and `core/config_loader.py:1416` [026] match. Both matches fall in text (a line inside a string; a trailing comment), which shows that a hit need not be code. |

That makes three runs. Each synthetic control and read-based check was tested against its expression with Python's re, on the invented lines and the printed lines only. Every hit line matched and no non-hit line did (check_p3_d005.py, Q-07).

Limits, stated before the runs:
- S07d would miss a definition bound in another way: a decorator-built object, `setattr`, `async def`, an annotated, chained or tuple binding, or `import ... as _strong_close`. It would hit an alias, such as `_strong_close = other._strong_close`.
- S07i would miss a multi-line import with the names on later lines, an aliased module import, or a relative import. It would hit an import line whose trailing comment mentions the name.
- S08a would miss `_body_frac =`, `bar_body_frac =`, `body_frac_long =`, `body_frac: float =`, `d["body_frac"] =` and `async def`. It would hit `BODY_FRAC = 0.4`, a tuple target, an assignment-like line in text (as the two checks show) and a keyword argument.
- A located line in the exit method's body (290, 293) or in screening/hard_gate.py is classified only and is not proposed for reading (decisions 6 and 8).
- A located line is still unread. Any one-line read of a located line comes back as an exact list at a later checkpoint, with its path, full blob, line, hit, purpose, exclusions and budget.

## B. Reads proposed now

None. R09 and R10 are withdrawn in their earlier form (section A replaces them). R11 is not proposed again (section C).

## C. regime/engine.py (decision 7)

Proposed: leave the S04c hits 191, 207, 245 and 340 UNKNOWN / NOT ESTABLISHED and outside D-005, and add a bookmark:

| ID | Question | Owner | Basis |
|---|---|---|---|
| D5-BK-6 (proposed) | Whether any of regime/engine.py's S04c hit lines 191, 207, 245 and 340 computes a per-bar measure. 207 names true_range_series (S03); the S02r hit lines 190, 205, 233 and 328 precede the four hits. | D-008 | count log, Part 2 evidence note section 2 |

No safer typing method is proposed. Any read of those lines could expose function bodies or regime logic, and a count cannot give the functions' names without showing matched text.

## D. Totals

The three count runs of section A add no read lines and no rows. With no read proposed, the budget stays at 243 lines and 22 rows.

## E. Not proposed

- Reads at the call or body lines that S07 and S08 located: orders/structure_exit_manager.py 290 and 293 (exit decision logic), screening/hard_gate.py 378, 405 and 409-412 (gate code), v3_chain/score.py (scoring), v3_chain/pb01_entry.py and v3_chain/pb01_runner.py (entry), sr_detector/retest_confirm.py 124, and tests/unit/test_v3_pb01_gates_score.py.
- R01c, R02b, R08a, R01d, S05' and a test-name search for true range (all decided at the second checkpoint).
- Any repository record or manual draft.
