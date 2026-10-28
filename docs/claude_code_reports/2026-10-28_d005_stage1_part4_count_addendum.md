# D-005 Candle analysis - Stage 1, fourth checkpoint: count runs S07d, S07i, S08a (count addendum)

Date of work: 28-Oct-2026, 12:56 to 13:08 IST. Prepared by Claude for ChatGPT's review. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part4_count_addendum.md. Nothing is written to any repository now. This is not a manual and makes no V2 decision.

Authority: D005_CHATGPT_REVIEW_PART3_AND_CHECKPOINT4_AUTHORIZATION.txt (received from Rama at 12:56 IST; 71 lines, 6,498 bytes, SHA-256 4DFE18CC451AD4CBC9E10B94B8B8BB826D2F841430AAF09577CAF7DD0B50C6D6), sections B to D. Only S07d, S07i and S08a were run, each in s_rt only. No read was made. Process facts below are Claude's records of its own runs; ChatGPT has not re-performed them.

## 1. Safety record

- Gate: gate_d005.py gained S07d, S07i and S08a for s_rt only. Each expression is character for character the checkpoint's text (compared by script). The third-checkpoint gate is kept as gate_d005_ck3.py (SHA-256 FADF43F34548A32E1EA8170EA8C7EC91D0DF0DEBA7D69091E705FF1D8E1EEB98).
- Gate self-test PASS: 19 path cases, 1 store case, 1 read case, 34 count cases and 1 range-drop case. The count cases refuse each of the three on s_tests, and S07d and S08a with altered text; there is no altered-text case for S07i.
- Synthetic controls: S07d.txt, S07i.txt and S08a.txt, with their .expect.json files, written by synth_d005_p4.py with the lines stated in the fourth-checkpoint proposal. Every earlier control file was unchanged (SHA-256 checked before and after).
- Preflight before the runs (12:58:14 IST) and after them (13:00:09 IST): PASS both.
  - Legacy HEAD and main equal the pin, and V2 main 2364a99 is the same locally and on GitHub. git ls-remote made a read-only contact with GitHub for both.
  - The reading clone's HEAD, tree and listing are as recorded. sensitive_check.py exited 0.
  - Objects: 816 blobs, 101 trees and 1 commit, unchanged.
  - The 808 working files of the scopes equal their pinned blobs.
- Each run went through the gate first and printed per-file counts and line numbers only. Object counts and the protected-store check were unchanged after each run. The excluded-range rule applied, and no hit fell inside main.py 3695-3713.
- No matched text was printed, saved or read. No hit line was read. The read log is unchanged at 30 reads.
- No repository was changed. No earlier file was altered.
- No new deviation occurred in this step.
- Not done:
  - no other expression, scope or rerun;
  - no git status, checkout, fetch or filter change, and no other operation on the reading clone;
  - no legacy code, test, import or script run;
  - no service contact other than the git ls-remote above;
  - no manual draft.

## 2. Count results (FACT (COUNT))

NO VALID REAL CONTROL exists for any of the three runs, since no frozen V2 record quotes a code-level hit for these expressions. The synthetic controls verify only the pipeline. The results are candidate locators only. They are not exhaustive, and a zero is not evidence of absence.

| Run | Expression | Time (IST) | Scope | Files searched | Files with hits | Lines | Per-file hit lines | Synthetic | Record SHA-256 |
|---|---|---|---|---|---|---|---|---|---|
| S07d | `^\s*def\s+_strong_close\b\|^\s*_strong_close\s*=(?!=)` | 12:58:52 | s_rt (281) | 281 | 1 | 1 | sr_detector/retest_confirm.py 130 | PASS (3 hit, 4 non-hit lines) | 82EDC291242D8B19483A7B10C7AD48E255FD3D17FC581C044E4AFD8C63AEDCA0 |
| S07i | `^\s*from\s+sr_detector\.retest_confirm\s+import\s.*\b_strong_close\b` | 12:59:30 | s_rt (281) | 281 | 1 | 1 | orders/structure_exit_manager.py 70 | PASS (2 hit, 4 non-hit lines) | 07FA9DAC6DA4D3C1A76EB43134441446C5761C36A7D2C615CBF9BDF72C6BCA0B |
| S08a | `(?i)\bbody_frac\b\s*=(?!=)\|^\s*def\s+\w*body_frac\w*\s*\(` | 13:00:09 | s_rt (281) | 281 | 3 | 4 | core/config_loader.py 1416; screening/hard_gate.py 385, 409; v3_chain/score.py 82 | PASS (4 hit, 4 non-hit lines) | 0A9D7BA31D8B6698A632C01E9EED4690D3C09C56AAC20915D79E897709EE1B36 |

The table writes `\|` for the regex's `|`.

Real control, run by run: S07d NONE; S07i NONE; S08a NONE (each record states "real control: none").

Read-based checks:
- S07d's hit at sr_detector/retest_confirm.py 130 is the definition line printed by read 025.
- S07d did not hit exit line 284 (read 023, a docstring line), as expected.
- S08a's hits at core/config_loader.py 1416 and screening/hard_gate.py 385 fall in text, as the proposal stated: a trailing comment (read 026) and a line inside a string (read 020; INTERPRETATION, as in Part 3).

Limits, as stated in the proposal and the checkpoint:
- S07d can miss a definition or binding made through a decorator, `async def`, an annotated, chained or tuple binding, `setattr`, or `import ... as`.
- S07i can miss a multi-line, relative or aliased import. It can hit an aliased import (`... import _strong_close as x` or `... import y as _strong_close`), the first line of a bracketed multi-line import, an import line whose trailing comment names `_strong_close`, or docstring or comment text that begins with such an import.
- S08a can miss suffixed or prefixed names, annotated or dictionary assignments and `async def`. It can hit comments, text and keyword arguments.

What the counts show (INTERPRETATION from line numbers and line shapes only; no hit line was read):
- G-03, the source of the structure exit's `_strong_close`:
  - S07i's only hit is orders/structure_exit_manager.py 70. That line is shaped as an import line from sr_detector.retest_confirm that names `_strong_close`, or as comment or string text of that shape (the module has a long docstring, read 022). Whether it uses an alias or spans several lines is not established.
  - S07d's only hit is the known definition at sr_detector/retest_confirm.py 130. S07d located no `def _strong_close` or direct `_strong_close =` line in the exit file. That is not proof that none exists.
  - These counts are consistent with the exit file's `_strong_close` naming MC-08's function. This is NOT ESTABLISHED: no line was read, and an import is not evidence of use or execution.
- G-02, the computation of body_frac:
  - Besides the two text lines, S08a located screening/hard_gate.py 409 and v3_chain/score.py 82. Each is shaped as an assignment or keyword binding of body_frac, as a definition named for it, or as comment or string text containing `body_frac =` or `body_frac=` (line 385 is such text).
  - Whether either computes body_frac is NOT ESTABLISHED. Both lie in files owned elsewhere, not assessed here: the hard gate (J-010, as recorded in Part 1) and scoring (D-017 and D-018, by the agenda).
  - The other S08 hits are not located by S08a:
    - screening/hard_gate.py 378, 405, 410, 411 and 412;
    - v3_chain/pb01_entry.py 227;
    - v3_chain/pb01_runner.py 161, 168 and 196;
    - v3_chain/score.py 70, 73, 83 and 85.

    Their shape is not established beyond that, given S08a's limits.

## 3. Budget (from the logs)

- Rows: 22 of 50.
- Lines: 243 of 900 planned and 1,000 absolute, unchanged by this step, which read 0 lines. 657 planned lines and 28 rows remain.
- Count runs: 3 in this step and 20 in Stage 1 so far (10 at S1-1, 3 at the second checkpoint, 4 at the third, 3 now).

## 4. Carried forward (no earlier artifact altered)

The earlier files are unchanged. Their SHA-256 values were re-checked now:
- Part 1 evidence: 907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD
- Part 2 proposal: BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D
- Part 2 evidence note: 7EA0AB79894BD41331B796E2FB7204ABB6CB17A9079EE5DE33A7BD56E7D68B14
- Third-checkpoint list: 8ACB67AFB7FCFCBFF06F59499298F00702531C98FD223F5CAD2DF52A01C0CD9A
- Part 3 count log: 14574FE27188EFA0E4D2A7D4F145EE1016FF60CC2C0F7C1E3DE88D0FD684DB2A
- Fourth-checkpoint list: C2ED3DDDF385CCB22FB5F1290D205F88965D30FF4254839DE1597D73AB9AE16A

The corrections stand as accepted:
- Erratum E-1 to Part 1 MC-08: pos is measured from the unfavourable end.
- Erratum E-2 to Part 1 MC-08's INTERPRETATION: line 133 does not use the close.
- DV-02 remains a process deviation. It was a `wc -l` on core/candle_math.py, run outside the gate between 12:07:21 and 12:08:04 IST, and ChatGPT has not verified it.

D5-BK-6 (regime/engine.py hits 191, 207, 245 and 340 for D-008) is held as a deferred bookmark, neither created nor stored (checkpoint A6).

## 5. Proposed exact list for a fifth checkpoint (nothing here is authorised or done)

Each is a one-line read through the existing gated reader. The reader checks the tree entry and the working file against the full blob, applies the mask_read2.py and stage-2 masks, checks the object counts and sensitive_check.py after the read, and logs the read. Each read below is one row and one line.

| ID | Path | Full blob ID | Line | Evidence | Purpose | Exclusions |
|---|---|---|---|---|---|---|
| R10a | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 409 | S08 and S08a hit | G-02. Classify the line. If it computes body_frac, record its expression and any zero-range or missing-value handling on that line, at formula level only. | No other line. A gate condition, threshold, verdict or downstream decision on the line is not documented. If the line uses a value defined elsewhere (for example a range variable), record only that value's name and stop with a narrow question. Stop rule: if the line holds a comparison, threshold, verdict, return, weight or rank, or is a partial statement, record only its class, quote nothing and stop; no range extension. |
| R10b | v3_chain/score.py | 8270d49812543497e3ef37b4d4444e9165138541 | 82 | S08 and S08a hit | G-02. As R10a. | As R10a. A score weight or ranking use on the line is not documented (D-017, D-018). |
| R09a | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 70 | S07 and S07i hit | G-03. Confirm whether the line is a one-line import of `_strong_close` from sr_detector.retest_confirm. Optional: the counts already indicate this. | No other line. Nothing about the exit's use of the name (decision 8 of the third checkpoint). If line 70 is text or an indented import, record only that class. |

Totals: 3 lines and 3 rows, giving at most 246 lines and 25 rows in all. ChatGPT may approve any subset.

Risks, stated before any read:
- Line 409 or line 82 may turn out to be a keyword argument, a result field or a comment rather than a computation, and S08a's limits allow for this. Either way the read records only what kind of line it is.
- A computation may rest on values defined on other lines. In that case only their names are recorded, and the analysis stops at a narrow question.
- Line 409 probably lies in a function body of the hard gate: the read list (R06-01) placed the nearby S04c hits in gate_confirm and gate_pullback by INTERPRETATION from D-002 MC-18's line numbers. Line 82 may lie in a body of the scoring code. The third and fourth checkpoints withdrew R09 and R10 so as not to expose such bodies (decisions 7 and 8; the fourth-checkpoint list, section A). R10a and R10b therefore need ChatGPT's explicit ruling under decision 8. If ChatGPT rules against them, G-02 stays UNKNOWN.

If no read is approved, G-02 stays UNKNOWN with the two candidate locators above, and G-03 stays at the count-based INTERPRETATION. Stage 1 could then close with those two as open items for ChatGPT's decision.

## 6. Records

```
search/S07d_s_rt.txt   82EDC291242D8B19483A7B10C7AD48E255FD3D17FC581C044E4AFD8C63AEDCA0
search/S07i_s_rt.txt   07FA9DAC6DA4D3C1A76EB43134441446C5761C36A7D2C615CBF9BDF72C6BCA0B
search/S08a_s_rt.txt   0A9D7BA31D8B6698A632C01E9EED4690D3C09C56AAC20915D79E897709EE1B36
```
