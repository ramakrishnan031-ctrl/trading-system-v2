# D-005 - CANDLE ANALYSIS: MANUAL MANIFEST

Status: the companion manifest of the frozen D-005 manual (DR-071). Prepared by Claude on 28-Oct-2026, from 14:27 IST, under D005_CLAUDE_PROMPT_FINAL_FREEZE_STORAGE_PUSH_28-OCT-2026.txt and D005_FREEZE_STORAGE_ANSWER_FILE_28-OCT-2026.txt (received from Rama at 14:21 IST). It updates the accepted revision 3 manifest (stored unchanged as docs/inputs/chatgpt_originals_and_replies/D-005_MANUAL_DRAFT_MANIFEST_REV3_28-OCT-2026.md; SHA-256 A9E79B6FDF671817F8C175AE4C3ECFB397D8109BC0F1F3AD697563B8A7CA97DE, as ChatGPT verified it) to describe the frozen manual. This manifest does not record its own hash; the file map of the freeze change note (section 5) gives it.

## Frozen manual

```
docs/agenda/d-005_candle-analysis/D-005_manual.md  242 lines  29,666 bytes  037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA
```

It is revision 3 with four freeze edits and one pre-freeze correction, C-1, and nothing else.

Freeze edits (line numbers are revision 3's):
- F1: the title, line 1.
- F2: the status bullet, line 5. It records the freeze, the acceptance and authorisation, revision 3's identity and stored path, correction C-1 and the storage path.
- F3: the manifest's name in the inputs bullet, line 19. The name 2026-10-28_d005_manual_DRAFT_manifest.md becomes D-005_manual_manifest.md, this file.
- F4: section 10, from line 232. "Draft statement" becomes "Status", with the freeze statement and the places where the companions are stored.

Correction C-1 comes from the frozen-manual cross-reference search before the freeze, which followed the D-004 C-1 precedent. Claude's blocker note (D-005_FREEZE_PREREQUISITE_BLOCKER_28-OCT-2026.txt) proposed it, and Rama's decision, D005_CHECKPOINT_DECISION_APPROVE_C1_AND_PROCEED_TO_FREEZE.txt (received at 14:39 IST; "OPTION A - APPROVE C-1, WITH THE BOUNDED TEXT BELOW", there with an em dash), approved it with the text applied here:
- C-1a, section 6: one new bullet after the body_frac bullet (frozen lines 123 to 125). It cross-references D-003's price_action step (D-003 MC-33, MC-39) and A-008's Chartink scanner conditions (A-008 section 17).
- C-1b, section 7: two new rows at the end of the table (frozen lines 165 and 166).
- C-1c, section 10: "one confirmed individual-candle measure" becomes "one individual-candle measure confirmed by its own reads". The sentence also states that section 6 cross-references two measures or condition sets recorded elsewhere, not re-read or confirmed by D-005.
- Source of the text: C-1a and C-1b are the decision's text (items 1 and 2), word for word; C-1c follows its item 3. The decision drew them from the frozen D-003 manual (section 8, step 5; MC-33, MC-39) and the frozen A-008 manual (section 17), as the blocker note cites them. The text was taken from the decision, not re-derived from those manuals.

As the decision requires, C-1:
- involved no new legacy source read or search;
- adds material that is HISTORICAL and cross-reference-only, derived from the frozen D-003 and A-008 manuals;
- changes no D-005 formula, source finding, open item, erratum or deviation;
- addresses the blocker's findings F-1 and F-2 as historical cross-references, not source contradictions.

DV-02 and DV-03 remain separate and unchanged. D5-BK-6 remains proposed and not stored.

Every other line of revision 3 is unchanged, in order. The formula, the evidence claims and labels, the control-status caveats, the open items, the errata E-1 and E-2 and the deviations DV-02 and DV-03 are untouched.

## Revisions as reviewed (stored unchanged in docs/inputs/manuals/)

| Revision | Stored as | Lines | Bytes | SHA-256 |
|---|---|---|---|---|
| 1 | D-005_CANDLE_ANALYSIS_REV1_28-OCT-2026.md | 226 | 22,816 | 2D1F9251EC26A28DDA9EAF5B3BF7052978DF9F2FF0821EFE2B05FB457052D6D1 |
| 2 | D-005_CANDLE_ANALYSIS_REV2_28-OCT-2026.md | 234 | 25,825 | ADB6FD193B2D0B6DB8F5DAFA392F4D3B7459E50D2F57E70774391EBDD27FE149 |
| 3, accepted | D-005_CANDLE_ANALYSIS_REV3_28-OCT-2026.md | 234 | 26,126 | 13B937825AF4BD3BF794782CEDE7E0B6CB4FD8D519C9F10939EFECDF1EC177BB |

- Each revision was delivered under the one name 2026-10-28_d005_manual_DRAFT_for_ChatGPT_review.md. They are stored here under the REVn names that docs/inputs/manuals/ uses for D-004's drafts. This is a disclosed change of name, not of content.
- Revision 2's file had been overwritten in place by revision 3. The stored copy was rebuilt at the freeze by reversing revision 3's three recorded wording edits. Its SHA-256 equals the value ChatGPT computed for revision 2 (D005_FINAL_EDITORIAL_CLARIFICATIONS_RESHARED.txt).
- The drafts' manifests are stored unchanged in docs/inputs/chatgpt_originals_and_replies/:
  - revision 1 as D-005_MANUAL_DRAFT_MANIFEST_REV1_28-OCT-2026.md (SHA-256 E2315573BE9EDB5176AE4FC8A8357EADC55559D2EC0E081520523E61018A2474, as ChatGPT recorded it);
  - revision 3 as D-005_MANUAL_DRAFT_MANIFEST_REV3_28-OCT-2026.md.
- Revision 2's manifest was overwritten in place by revision 3, and no hash of it was recorded, so it is not stored.

## Substantive inputs (stored unchanged in docs/claude_code_reports/, OQ-11)

```
2026-10-28_d005_stage1_part1_read_evidence.md  907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD
2026-10-28_d005_stage1_part2_proposed_list.md  BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D
2026-10-28_d005_stage1_part2_evidence_note.md  7EA0AB79894BD41331B796E2FB7204ABB6CB17A9079EE5DE33A7BD56E7D68B14
2026-10-28_d005_stage1_checkpoint3_proposed_list.md  8ACB67AFB7FCFCBFF06F59499298F00702531C98FD223F5CAD2DF52A01C0CD9A
2026-10-28_d005_stage1_part3_count_log.md  14574FE27188EFA0E4D2A7D4F145EE1016FF60CC2C0F7C1E3DE88D0FD684DB2A
2026-10-28_d005_stage1_checkpoint4_proposed_list.md  C2ED3DDDF385CCB22FB5F1290D205F88965D30FF4254839DE1597D73AB9AE16A
2026-10-28_d005_stage1_part4_count_addendum.md  702D654C06A99EE4C2FA3A8542AF93F0B29ED7911FFC33F8A9C766A765FAF834
2026-10-28_d005_stage1_part5_read_addendum.md  D3839284F4CC8578F1D27DF5579DA0D4351855862339A64670DEC142F93E2738
```

The count log (revision 1) and the read list (revision 1) that the Part 1 evidence cites are stored beside them. So are Claude's three Stage 0 reports. The freeze change note lists their hashes.

## Inputs and validation (unchanged from revision 3)

- The eight artifacts above were the only substantive inputs to the synthesis. D-004 facts appear only as the HISTORICAL citations the inputs already make.
- The masked evidence files of reads 001 to 031 are evidence generated earlier and are listed by SHA-256 in the inputs. They were consulted only to validate the draft's quotations and citations. That was not a new legacy source read. They are Claude's working records and are not stored, as D-004's were not.
- The quarantined lines were never read, and neither the manual nor this package holds their text:
  - screening/hard_gate.py:409
  - v3_chain/score.py:82
  - orders/structure_exit_manager.py:290 and 293
- The freeze adds no legacy read, search, count or command on the legacy clone.

## Process deviations (as recorded; separate)

- DV-02: one `wc -l` on core/candle_math.py was run outside the gate and outside the authorised operations, between 12:07:21 and 12:08:04 IST. It printed only a number already recorded and was not repeated. ChatGPT has not verified it.
- DV-03 (draft stage):
  - After revision 1 was written, Claude's check script check_m_d005.py ran sensitive_check.py, the object counts of the reading clone and `git status --porcelain` on the V2 clone. Claude reports the results as exit 0, unchanged at 816 blobs, 101 trees and 1 commit, and clean.
  - Revision 1 of the draft's register and of its manifest was generated by Claude's build script, build_m_docs_d005.py. Revisions 2 and 3 were hand-edited.
  - These actions were outside the draft-stage authorisation. As Claude reports, no repository write occurred. ChatGPT has not reproduced them.
  - The related disclosure stands: the check scripts for the earlier Stage 1 reports also ran sensitive_check.py, the object counts and `git status --porcelain` on the V2 clone, never on the legacy reading clone.
- The freeze step runs no `git status`. Its changed-file checks use `git diff` and `git ls-files --others --exclude-standard` on the V2 clone only, as the freeze prompt prefers.
