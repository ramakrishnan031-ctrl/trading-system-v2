# D-005 Candle analysis - Stage 1, fifth checkpoint: read R09a (orders/structure_exit_manager.py:70) - addendum

Date of work: 28-Oct-2026, 13:37 to 13:44 IST. Prepared by Claude for ChatGPT's review. Intended storage path at the freeze (OQ-11): docs/claude_code_reports/2026-10-28_d005_stage1_part5_read_addendum.md. Nothing is written to any repository now. Not a manual; no V2 decision.

Authority: D005_CHATGPT_REVIEW_PART4_AND_CHECKPOINT5_LIMITED_AUTHORIZATION.txt (received from Rama at 13:37 IST; 58 lines, 5,286 bytes, SHA-256 A2250F241780FE956C004665CAFD48981D3855ECCA0A86FEB6CEFA370A5E57F7), decisions 2 and 5. Only R09a was read. Process facts below are Claude's records of its own runs; ChatGPT has not re-performed them.

## 1. Line 70: classification

| No. | Time (IST) | Row | Mode | Range | Path | Full blob ID | Reader lines | Printed | Masks (range counts) | Evidence SHA-256 |
|---|---|---|---|---|---|---|---|---|---|---|
| 031 | 13:40:47 | R09a | R | 70-70 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 673 | 1 | masks: {'HOSTTOK': 0}; stage-2 masks: {} | b53434e8c56f |

```
70: from sr_detector.retest_confirm import _strong_close
```

- FACT (SOURCE), the literal text: `orders/structure_exit_manager.py:70` (blob 6e4c58d8d2e8aa0438a9461067ae070ed1118950, evidence 031) reads `from sr_detector.retest_confirm import _strong_close`. It is a complete one-line `from ... import ...` line naming `_strong_close`:
  - it begins at column 0, with no indentation;
  - it uses no alias, no parenthesis, no continuation and no trailing comment.
- Mask and control result: HOSTTOK 0, and the stage-2 mask found nothing. The tree entry and the working file equal the pinned blob. Object counts were unchanged after the read, and sensitive_check.py exited 0.
- Classification: an import statement, by its form. Whether the line is executable code or text inside a multi-line string cannot be settled from the line alone: no string delimiter is on it, and the module's docstring end has not been read. INTERPRETATION: an unindented one-line import is the usual shape of a module-level import.
- G-03: on its literal text, the exit module imports `_strong_close` from sr_detector.retest_confirm, the module of MC-08's definition at line 130. This is the extent of the finding.
- NOT ESTABLISHED:
  - whether the import executes;
  - whether, or how, the exit uses the name (the S07 hits at 290 and 293 are unread candidate locators; checkpoint 4, decision A3);
  - whether the name the exit binds resolves to the line-130 definition (S07d's limits);
  - whether any production path ran.
- The exit's logic is not inspected (decision 2).

## 2. Safety record and budget

- Reader: cread_d005_p3.py, a copy of cread_d005_p2.py. Only these were changed: the approved list's path and SHA (RL_PATH, APPROVED_SHA; readlist_p3.json, one row, SHA-256 8AA5AEDD2AFA25567C278AA219AFEDE5DCF120EDD2BE9D3D9E069AD7F46A7615), the cumulative cap (244), its docstring, and its self-test row, which now also checks the row's path.
- The reader's self-test PASSED, and three live refusals held: a second-checkpoint row (R06b), a rejected row (R10a) and a second range index. The refusals were observed in the session but not saved to a record; each touched no source.
- Preflight before the read (13:40:11 IST) and after it (13:40:47 IST): PASS both.
  - Legacy HEAD and main equal the pin, and V2 main 2364a99 is the same locally and on GitHub. git ls-remote made a read-only contact with GitHub for both.
  - The reading clone's HEAD, tree and listing are as recorded; sensitive_check.py exited 0.
  - Objects: 816 blobs, 101 trees and 1 commit, unchanged.
  - The 808 working files of the scopes equal their pinned blobs.
- Not done:
  - no other line read: R10a (screening/hard_gate.py:409) and R10b (v3_chain/score.py:82) were not read (decision 3);
  - no search;
  - no git status, checkout, fetch or filter change;
  - no legacy code, test, import or script run;
  - no service contact other than the git ls-remote above;
  - no repository write and no manual draft.
- No new deviation in this step.
- Budget (from the read log): 31 reads, 23 rows of 50, 244 lines of 900 planned (1,000 absolute). 656 planned lines and 27 rows remain. Count runs in Stage 1: 20.

## 3. Status of the open items

| ID | Item | Status |
|---|---|---|
| G-01 | the comparison in `_strong_close` | resolved (read 030) |
| G-02 | the code that computes body_frac | UNKNOWN / NOT ESTABLISHED. screening/hard_gate.py 409 and v3_chain/score.py 82 are candidate locators from uncontrolled counts only, and were not read (decision 3). |
| G-03 | the source of the exit's `_strong_close` | the line-level fact in section 1; use and execution NOT ESTABLISHED |
| G-04, G-05 | true_range_series's formula; atr and resample signatures | as decided at the second checkpoint (E, F) |
| G-06 | whether any path ran | NOT ESTABLISHED; outside Stage 1 |
| G-07 | regime/engine.py 191, 207, 245, 340 | UNKNOWN / NOT ESTABLISHED, outside D-005; D5-BK-6 accepted as a proposed deferred bookmark, not stored (decision 4) |
| G-07a, G-08 | the other uncontrolled hits | unchanged; candidate locators only |

Carried forward without change:
- Errata E-1 and E-2 to Part 1 MC-08, and DV-02, as in the Part 2 evidence note and the Part 3 count log.
- The earlier files, unchanged. Their SHA-256 values were re-checked now:

```
2026-10-28_d005_stage1_part1_read_evidence.md  907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD
2026-10-28_d005_stage1_part2_proposed_list.md  BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D
2026-10-28_d005_stage1_part2_evidence_note.md  7EA0AB79894BD41331B796E2FB7204ABB6CB17A9079EE5DE33A7BD56E7D68B14
2026-10-28_d005_stage1_checkpoint3_proposed_list.md  8ACB67AFB7FCFCBFF06F59499298F00702531C98FD223F5CAD2DF52A01C0CD9A
2026-10-28_d005_stage1_part3_count_log.md  14574FE27188EFA0E4D2A7D4F145EE1016FF60CC2C0F7C1E3DE88D0FD684DB2A
2026-10-28_d005_stage1_checkpoint4_proposed_list.md  C2ED3DDDF385CCB22FB5F1290D205F88965D30FF4254839DE1597D73AB9AE16A
2026-10-28_d005_stage1_part4_count_addendum.md  702D654C06A99EE4C2FA3A8542AF93F0B29ED7911FFC33F8A9C766A765FAF834
```

## 4. Proposed next action (for a new checkpoint; nothing done)

- No further read or search is proposed.
- Proposed for ChatGPT's ruling, as an exact list:
  - (a) accept G-03 at section 1's line-level finding, within its NOT ESTABLISHED limits;
  - (b) rule whether Stage 1 is complete, with section 3's open items carried as stated;
  - (c) if so, issue a separate checkpoint that sets the D-005 manual draft's scope, its inputs (the seven earlier files and this addendum) and its exclusions.

  Claude will not draft the manual, write any repository or store D5-BK-6 until a checkpoint authorises it.

## 5. Records

```
evidence/031.txt     B53434E8C56FC781945683773780B57F207B8BA0CBF79F46572E34595D020CC5
readlist_p3.json     8AA5AEDD2AFA25567C278AA219AFEDE5DCF120EDD2BE9D3D9E069AD7F46A7615
```
