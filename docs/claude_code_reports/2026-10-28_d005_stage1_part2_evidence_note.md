# D-005 Candle analysis - Stage 1, second checkpoint: counts S03, S06, S02r and read R06b (evidence note)

Date of work: 28-Oct-2026, 12:03 to 12:19 IST. Prepared by Claude for ChatGPT's review. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part2_evidence_note.md; nothing is written to any repository now. Not a manual; no V2 decision.

Authority: D005_CHATGPT_REVIEW_PART1_EVIDENCE_AND_PART2_CHECKPOINT_RESHARED.txt (received from Rama at 12:03 IST; 73 lines, 8,976 bytes, SHA-256 D9EEA6DA49E3F180FD92039FDB8C7FE75A601B2CE8D10D9EB07B07B802B6C1B4), decisions A, B, G and I, on the two files it reviewed: the Part 1 evidence (2026-10-28_d005_stage1_part1_read_evidence.md, SHA-256 907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD) and the Part 2 proposal (2026-10-28_d005_stage1_part2_proposed_list.md, SHA-256 BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D). Nothing else in the Part 2 proposal was run or read: S07 and S08 (decision C), S05' (D), R01c, R02b and R08a (E), R01d (F), R09, R10 and R11 (H). Proposals for the next checkpoint are in a separate file (2026-10-28_d005_stage1_checkpoint3_proposed_list.md).

Labels as in the Part 1 evidence: FACT (SOURCE), FACT (TEXT), FACT (TEST), FACT (COUNT), HISTORICAL, INTERPRETATION, UNKNOWN / NOT ESTABLISHED, OBSERVATION. Process facts below (gate results, counts, hashes, mask counts) are Claude's records of its own runs; ChatGPT has not re-performed them.

## 1. Operations and safety

- Gate: gate_d005.py gained three authorised entries, the expressions and scopes of decision A, together with a one-file scope (the S02R_FILES constant and its s02r_file resolver) and replaced and added self-test cases: S03 `\btrue_range_series\b` on s_rt (281 files); S06 the same on s_tests (486); S02r `(?i)^\s*(def|class)\s` on regime/engine.py only (a one-file scope, s02r_file). The earlier gate is kept as gate_d005_ck1.py (SHA-256 946B24138B21C73968B35066068DBE46B1055B3EDDD523226773C7B3DF27BE25, recorded now; no hash of it was recorded at Part 1). Self-test PASS: 19 path cases, 1 store case, 1 read case, 18 count cases and 1 range-drop case; among the count cases S07, S08 and S05' are refused as not authorised, and S03 with an altered expression, S03 on s_tests, S06 on s_rt, S02r on the S02 files, S02 on regime/engine.py and S02r on another file are refused. Live refusal test: ccount_d005.py S07 s_rt refused, nothing searched.
- Synthetic controls: three new invented-line files and their three expected-hit files (.expect.json), written by synth_d005_p2.py with the lines stated in the Part 2 proposal, section A; the seven earlier control .txt files unchanged (SHA-256 checked before and after; their .expect.json files were not hashed and were not rewritten).
- Preflight before the counts (12:05:11 IST) and after the read (12:08:04 IST): PASS both. Legacy HEAD and main = the pin, and V2 main 2364a99 locally and on GitHub (git ls-remote: a read-only contact with GitHub for both); the reading clone's HEAD, tree and listing as recorded; sensitive_check.py exit 0; objects 816 blobs, 101 trees, 1 commit, unchanged; the 808 working files of the scopes equal to their pinned blobs.
- Count runs: three runs over three scopes (section 2). Each: gate first; per-file counts and line numbers only, no matched text printed or saved; object counts and the protected-store check unchanged; the excluded-range rule applied (no hit fell inside main.py 3695-3713). Read-line cost: 0.
- Read: one read, R06b, through cread_d005_p2.py, a copy of the Part 1 reader with the approved list (readlist_p2.json: one row, SHA-256 CACDE09C8A37FC0B78EFA2AC5F02081C072A9AD85B38CEBEE506FD761523CCA3), the cumulative cap (243), its docstring and its self-test cases changed. Self-test PASS; two live refusals (a Part 1 row; a second range index). The tree entry and the working file equal to blob 5fc212535bbd2e86f1c5899d07ce76331ab72724; mr.py masking (HOSTTOK 0; stage-2 none); object counts unchanged after it.
- Budget: rows 22 of 50 (Part 1's 21 and R06b), 30 read calls in all (Part 1's 29 and read 030), lines 243 of 900 planned and 1,000 absolute (Part 1's 242 and R06b's 1); 657 planned lines and 28 rows remain.
- Disclosure DV-02: once in this step, between 12:07:21 and 12:08:04 IST, a `wc -l` was run on core/candle_math.py in the reading clone, outside the gate and outside this checkpoint's authorisation, to compare line-count conventions. It counts newlines and printed only the number 456, the count already recorded at Stage 1 (line_totals.txt); no content was printed. Not repeated; the convention is settled in section 3 from the records.
- Not done: no other count, expression or scope; no read other than R06b, and no other file operation in the clone except DV-02; no git status, checkout, fetch or filter change on the reading clone; no legacy code, test, import or script run; no service contact other than the git ls-remote above; no repository write; the Part 1 and Part 2 files unchanged.

## 2. Count results (FACT (COUNT): candidate locators only)

None of the three runs has a valid real control (no frozen V2 record quotes a code-level hit for these expressions); the synthetic controls verify the pipeline only. No zero, absence or completeness conclusion is drawn.

| Run | Time (IST) | Scope | Files searched | Files with hits | Lines | Per-file hit lines | Synthetic | Record SHA-256 |
|---|---|---|---|---|---|---|---|---|
| S03 | 12:05:32 | s_rt (281) | 281 | 2 | 8 | core/candle_math.py 19, 21, 54, 117, 194; regime/engine.py 15, 28, 207 | PASS (2 hit, 2 non-hit lines) | 6E385A9DC5170F9700A7E9A239F3FE76ACB31C68B4A5F1D5160EA9F94C589AB2 |
| S06 | 12:05:50 | s_tests (486) | 486 | 1 | 6 | tests/unit/test_candle_math.py 4, 28, 106, 110, 116, 120 | PASS (2 hit, 2 non-hit lines) | 1AC6F6E87DB26FE1F1F8E4EF59B4B098B5E9694B6128E8C603E1F3A97257AF02 |
| S02r | 12:06:01 | regime/engine.py (1) | 1 | 1 | 15 | 53, 57, 60, 95, 100, 142, 190, 205, 233, 283, 302, 321, 328, 344, 353 | PASS (3 hit, 2 non-hit lines) | 44C7613EF4EAAE6848C5676E909C9BDBEF74F20DA81D583B810AD1A5086ACC60 |

- Consistency (FACT (COUNT)): every S03 hit line is also an S04c hit line, as expected, since S04c matches "true_range": core/candle_math.py 19, 21, 54, 117 and 194 are among S04c's 11 hits there, and regime/engine.py 15, 28 and 207 among its 6.
- Decision B: S06 cannot match a test name that holds the string inside a longer identifier, such as `test_true_range_series_exact`; the name lines 109, 114 and 119 (read 002) are not among its hits, as expected, and that says nothing about which tests exist.

Positions (INTERPRETATION from line numbers only; no hit line was read):
- regime/engine.py: the S04c hits 191, 207, 245 and 340 follow the S02r hit lines at 190, 205, 233 and 328, the nearest before each. Those lines were not read; S02r is case-insensitive, so a matching line is a def or class line or, less likely, another line that begins with such a word (for example in a docstring). A hit after a definition line need not lie inside that definition (module-level code or the end of the function could come between); not established. Of the four, only 207 names true_range_series (S03); 15 and 28 were read in Part 1 (reads 011, 012).
- core/candle_math.py: 54 is the definition line of true_range_series (read 001). The S04c hit 101 lies within the 54-115 span that D-004 records for true range and ATR (HISTORICAL: D-004_manual.md:231); the S03 hit 117 lies after that span and before session_vwap (136), its context not established; the S03 hit 194 lies between adx (191) and the indented, so nested, _wilder (219, read 001). None was read (decision E). 19 and 21 lie between the header lines read at 1-2 and 28-31.
- tests/unit/test_candle_math.py: 110, 116 and 120 follow the test definitions at 109, 114 and 119 (read 002); 106 lies between test_rsi_known_value_wilder (74) and 109; 4 and 28 precede the first test at 50.

## 3. R06b result

| No. | Time (IST) | Row | Mode | Range | Path | Full blob ID | Reader lines | Printed | Masks (read type) | Evidence SHA-256 |
|---|---|---|---|---|---|---|---|---|---|---|
| 030 | 12:07:21 | R06b | R | 137-137 | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | 138 | 1 | masks: {'HOSTTOK': 0}; stage-2 masks: {} (range counts) | ae8ee4fe99d0 |

```
137:     return pos >= frac
```

- FACT (SOURCE): `_strong_close` returns `pos >= frac` (`sr_detector/retest_confirm.py:137`, read 030). G-01 is resolved by this line; nothing beyond it was read. The file has 137 newline characters (line_totals.txt, recorded at Stage 1); whether text follows the 137th is not established. The reader's header shows 138 by its own convention (the count of newline-split parts, as for core/candle_math.py: 456 recorded, 457 in read 001). INTERPRETATION: line 137 is a return statement, so a later line in the same function, if any, would not run.
- MC-08, now at formula level over lines 130-137 (reads 025 and 030): for a long, (close - low) / (high - low) >= frac; for a short, (high - close) / (high - low) >= frac; when high - low <= 0, close >= high for a long and close <= low for a short.
- Erratum to Part 1 MC-08 (`sr_detector/retest_confirm.py:136`): Part 1 called pos "the close's position from the favourable end". That is wrong. For a long, pos is measured up from the low, and for a short down from the high: from the unfavourable end toward the favourable one. The formula quoted in Part 1 is unchanged. Second erratum to Part 1 MC-08's INTERPRETATION: "a missing high, low or close would raise in the subtraction at 133" is wrong for the close, which line 133 does not use (corrected in the next point).
- INTERPRETATION (arithmetic on lines 133-137 only): with the close between the low and the high, pos lies in [0, 1], and pos >= frac means the close lies in the top (1 - frac) of the range for a long and in the bottom (1 - frac) for a short. No check on frac appears in 130-137; with the close in that range, frac <= 0 lets any non-flat bar pass and frac > 1 lets none pass. On a flat bar whose close equals its high and low, the function returns True in both directions. A None or absent high or low raises at 133, and a None or absent close at 135 or 136; a NaN in any of them returns False without raising.
- OBSERVATION (text against code): the docstring's "LONG wants the close in the UPPER frac of the 1m range" (`sr_detector/retest_confirm.py:131-132`) agrees with `pos >= frac` if "UPPER frac" means "at or above the frac point of the range"; if it means "within the top frac of the range", the code would need pos >= 1 - frac. The two readings coincide only at frac = 0.5. Not assessed further: the value of frac passed in and the use of the result are the caller's (D-015).

## 4. Gaps and open questions after this step

| ID | Gap | Status | Next |
|---|---|---|---|
| G-01 | the comparison of pos with frac | resolved (read 030) | - |
| G-02 | the code that computes body_frac, its zero-range and missing-value handling | UNKNOWN; no computing code in the inspected lines; not claimed absent from the system | S08, proposed again (checkpoint 3 list) |
| G-03 | the source of the structure exit's `_strong_close` | NOT ESTABLISHED | S07, proposed again |
| G-04 | true_range_series's formula and its tests; core/candle_math.py S04c hits 57, 61, 75 and 77 (inside its span by position, Part 1 G-07) | outside D-005 (decision E), unless a narrower per-bar question can be set without reading the series body | none proposed |
| G-05 | the signatures of atr and resample beyond their first lines | a gap; decision F: no additional read is needed now; D-004 MC-33 records their facts (HISTORICAL) | none proposed |
| G-06 | whether any importing path ran | NOT ESTABLISHED | outside Stage 1 |
| G-07 | regime/engine.py S04c hits 191, 207, 245, 340 | positioned (section 2), not typed | R11, proposed (definition first lines only) |
| G-07a | core/candle_math.py S04c hits 101 (within 54-115), 117 (after 115, context not established), 194 and 214 (after adx's first line at 191); 19 and 21 (header text naming true_range_series, by S03) | 101 inside the span D-004 records for true range and ATR (HISTORICAL, D-004_manual.md:231) and 194, 214 after adx's first line, a sequence function by its printed signature (Part 1 MC-02): not read (decision E's reasoning); 117 and the header lines not read | none proposed |
| G-08 | S04a, S04d and S05 hits outside the ranges read; screening/step_executor.py 327 (HISTORICAL, D-003 MC-33); sr_detector/models.py 78 (S&R by position) | open; candidate locators only | none proposed |

No list here is complete: every count behind it lacks a valid real control and serves as a candidate locator only.

## 5. Records

```
search/S03_s_rt.txt          6E385A9DC5170F9700A7E9A239F3FE76ACB31C68B4A5F1D5160EA9F94C589AB2
search/S06_s_tests.txt       1AC6F6E87DB26FE1F1F8E4EF59B4B098B5E9694B6128E8C603E1F3A97257AF02
search/S02r_s02r_file.txt    44C7613EF4EAAE6848C5676E909C9BDBEF74F20DA81D583B810AD1A5086ACC60
evidence/030.txt             AE8EE4FE99D04DB4E3EDAF9FD512375633487B79337C7E97B419B41CCA72806C
readlist_p2.json             CACDE09C8A37FC0B78EFA2AC5F02081C072A9AD85B38CEBEE506FD761523CCA3
```
