# D-005 Candle analysis - Stage 1, third checkpoint: count runs S07 and S08 (count log)

Date of work: 28-Oct-2026, 12:26 to 12:40 IST. Prepared by Claude for ChatGPT's review. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part3_count_log.md; nothing is written to any repository now. Not a manual; no V2 decision.

Authority: D005_CHECKPOINT3_CLAUDE_INSTRUCTIONS_FRESH_LINK.txt (received from Rama at 12:26 IST; 71 lines, 7,492 bytes, SHA-256 2900C12585256BB85170959D00C2209E6E215BBA8747D4D7BC081DF04CEABCEA), decisions 5, 6 and 9. Only S07 and S08 were run, each in s_rt and s_tests. No read was made: R11 (decision 7) and R09 and R10 (decision 8) are not authorised. The next exact list is a separate file (2026-10-28_d005_stage1_checkpoint4_proposed_list.md). Process facts below are Claude's records of its own runs; ChatGPT has not re-performed them.

## 1. Safety record

- Gate: gate_d005.py gained S07 `\b_strong_close\b` and S08 `(?i)body_frac`, each for s_rt and s_tests only; the second-checkpoint gate is kept as gate_d005_ck2.py (SHA-256 3A3BC1D3B82832843B23064D874057311C956AC7AA7435AF9F2A11F6E2D864C1). Self-test PASS: 19 path cases, 1 store case, 1 read case, 26 count cases and 1 range-drop case. Among the count cases, these are refused:
  - S07 and S08 with altered expressions;
  - S07 on s_cfg, S08 on s_all and S08 on regime/engine.py;
  - an unlisted expression ID.
- Synthetic controls: the files S07.txt and S08.txt, with their .expect.json files, written by synth_d005_p3.py with the lines stated in the third-checkpoint proposal. Every earlier control file (.txt and .expect.json) was unchanged (SHA-256 checked before and after).
- Preflight before the counts (12:28:12 IST) and after them (12:30:27 IST): PASS both.
  - Legacy HEAD and main equal the pin, and V2 main 2364a99 matches locally and on GitHub. git ls-remote made a read-only contact with GitHub for both.
  - The reading clone's HEAD, tree and listing are as recorded; sensitive_check.py exit 0.
  - Objects: 816 blobs, 101 trees and 1 commit, unchanged; the 808 working files of the scopes equal their pinned blobs.
- Each run:
  - gate first;
  - per-file counts and line numbers only, with no matched text printed or saved;
  - object counts and the protected-store check unchanged;
  - the excluded-range rule applied (no hit fell inside main.py 3695-3713).
- Read log: unchanged at 30 reads. No source line was read in this step.
- Not done:
  - no other expression or scope;
  - no read;
  - no git status, checkout, fetch or filter change on the reading clone, and no other file operation in it;
  - no legacy code, test, import or script run;
  - no service contact other than the git ls-remote above;
  - no repository write;
  - no earlier file altered.
- No new deviation occurred in this step.

## 2. Count results (FACT (COUNT))

There was NO VALID REAL CONTROL for any of the four runs: no frozen V2 record quotes a code-level hit for these expressions. The synthetic controls verify only the pipeline. The results are candidate locators only. They are not exhaustive, and a zero is not evidence of absence.

| Run | Time (IST) | Scope | Files searched | Files with hits | Lines | Per-file hit lines | Synthetic | Record SHA-256 |
|---|---|---|---|---|---|---|---|---|
| S07 | 12:28:44 | s_rt (281) | 281 | 2 | 6 | orders/structure_exit_manager.py 70, 284, 290, 293; sr_detector/retest_confirm.py 124, 130 | PASS (3 hit, 3 non-hit lines) | 3D347788638BD5CD13C0CDCB0D0A0674371B8F8B6AFD45959CA1F96017C68DC0 |
| S07 | 12:29:21 | s_tests (486) | 486 | 0 | 0 | - | PASS (3 hit, 3 non-hit lines) | F024A966A764440232E40D1C08086B7547226CD5D2E2FDE1F60331DFE4B7E8CC |
| S08 | 12:29:53 | s_rt (281) | 281 | 5 | 17 | core/config_loader.py 1416; screening/hard_gate.py 378, 385, 405, 409, 410, 411, 412; v3_chain/pb01_entry.py 227; v3_chain/pb01_runner.py 161, 168, 196; v3_chain/score.py 70, 73, 82, 83, 85 | PASS (3 hit, 2 non-hit lines) | 2D41BAD5655C698F394B1FD3213000809DE26B3ACFC20650BAB3249B4B64C720 |
| S08 | 12:30:26 | s_tests (486) | 486 | 1 | 7 | tests/unit/test_v3_pb01_gates_score.py 25, 39, 47, 57, 60, 128, 130 | PASS (3 hit, 2 non-hit lines) | 390226643A6DE227B830DE295808D5800DF3B77B7523E3732C630565A22CD3F4 |

- Read-based checks: the hit lists include the lines that earlier reads printed and that contain the strings:
  - S07: sr_detector/retest_confirm.py 130 (read 025) and orders/structure_exit_manager.py 284 (read 023);
  - S08: screening/hard_gate.py 385 (read 020) and core/config_loader.py 1416 (read 026).
- S07 in s_tests found no line. That is not evidence that no test exercises `_strong_close`, for example under another name.

Positions (INTERPRETATION from line numbers only; no hit line was read except where a read is named):
- sr_detector/retest_confirm.py:
  - 130 is the definition line (read 025);
  - 124 precedes it; its context is not established.
- orders/structure_exit_manager.py:
  - 284 is a docstring line of `_is_confirmed_break` (read 023);
  - 290 and 293 follow that method's definition line (281) and the docstring's closing line (285). By position they may lie in the method's body, which is exit decision logic (decision 8, not inspected);
  - 70 precedes 281; its context is not established.
- screening/hard_gate.py:
  - 385 is a text line inside a string (read 020; INTERPRETATION from its context, since no string delimiter lies in 382-388);
  - 378 precedes the range read at 382-388;
  - 405 and 409-412 lie between that range and 430-431, the last lines of a signature whose first line is not established (read 021). They may belong to the function holding 382-388, to that signature, or to other code: not established. Any of them is to be classified only (decision 6).
- core/config_loader.py 1416 is a configuration field line with a trailing comment (read 026).
- v3_chain/pb01_entry.py, v3_chain/pb01_runner.py, v3_chain/score.py and tests/unit/test_v3_pb01_gates_score.py: not read; their context is not established. By the file names only, they are entry, scoring and gate/score test code (entry: D-013 to D-016, and C-011 and J-010 as recorded in Part 1, BK-57 and BK-60; scoring: D-017 and D-018; not assessed).

## 3. Budget (from the logs)

- Rows: 22 of 50.
- Lines: 243 of 900 planned and 1,000 absolute. 657 planned lines and 28 rows remain.
- Unchanged by this step, which read 0 lines.
- Count runs: 4 in this step, and 17 in Stage 1 so far (10 at S1-1, 3 at the second checkpoint, 4 now).

## 4. Carried forward (no earlier artifact altered)

The earlier files are unchanged; their SHA-256 values were re-checked now:
- Part 1 evidence: 907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD
- Part 2 proposal: BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D
- Part 2 evidence note: 7EA0AB79894BD41331B796E2FB7204ABB6CB17A9079EE5DE33A7BD56E7D68B14
- third-checkpoint list: 8ACB67AFB7FCFCBFF06F59499298F00702531C98FD223F5CAD2DF52A01C0CD9A

Their corrections stand in the Part 2 evidence note, as accepted by decisions 2 and 4 (the labels E-1 and E-2 are assigned here):
- Erratum E-1 to Part 1 MC-08. pos is measured from the low for a long and from the high for a short, which is the unfavourable end. It is not measured "from the favourable end".
- Erratum E-2 to Part 1 MC-08's INTERPRETATION. Line 133 uses only high and low. A None or absent close raises at 135 or 136, not at 133.
- DV-02 is a process deviation. A `wc -l` was run on core/candle_math.py outside the gate and the authorised operations, between 12:07:21 and 12:08:04 IST. It printed only a number already recorded and was not repeated. ChatGPT has not verified it.

## 5. Gaps after this step

| ID | Gap | Status | Next |
|---|---|---|---|
| G-02 | the code that computes body_frac, with its zero-range and missing-value handling | UNKNOWN. Candidate locators in five runtime files and one test file (section 2). No hit outside the two lines read has been read. | S08a, proposed (checkpoint 4 list) |
| G-03 | the source of the structure exit's `_strong_close` | NOT ESTABLISHED. Candidate locators at 70, 290 and 293. | S07d and S07i, proposed |
| G-07 | regime/engine.py S04c hits 191, 207, 245 and 340 | UNKNOWN / NOT ESTABLISHED. R11 was rejected (decision 7). | proposed: leave them outside D-005 and bookmark them for D-008 (checkpoint 4 list, section C) |

G-04 and G-05 stand as decided at the second checkpoint: G-04 outside D-005 (decision E); G-05 a gap, with no read needed now (decision F). G-06 lies outside Stage 1. G-07a and G-08 are unchanged from the Part 2 evidence note. No list here is complete.

## 6. Records

```
search/S07_s_rt.txt      3D347788638BD5CD13C0CDCB0D0A0674371B8F8B6AFD45959CA1F96017C68DC0
search/S07_s_tests.txt   F024A966A764440232E40D1C08086B7547226CD5D2E2FDE1F60331DFE4B7E8CC
search/S08_s_rt.txt      2D41BAD5655C698F394B1FD3213000809DE26B3ACFC20650BAB3249B4B64C720
search/S08_s_tests.txt   390226643A6DE227B830DE295808D5800DF3B77B7523E3732C630565A22CD3F4
```
