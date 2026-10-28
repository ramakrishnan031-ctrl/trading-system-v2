# D-005 manual draft - manifest

Date: 28-Oct-2026, 13:57 IST. Prepared by Claude. Authority: D005_CHATGPT_STAGE1_CLOSURE_AND_MANUAL_DRAFT_AUTHORIZATION.txt (SHA-256 E4A260AAA18A20AE4C235EA95DD44E257B4915A2120B1C596EF43E9EC0910D7E), decision 3. Both files are local only, for ChatGPT's review. No repository write and no freeze package.

## Output

```
2026-10-28_d005_manual_DRAFT_for_ChatGPT_review.md  226 lines  22,816 bytes  2D1F9251EC26A28DDA9EAF5B3BF7052978DF9F2FF0821EFE2B05FB457052D6D1
```

## Inputs (the Part 5 addendum's SHA-256 ledger, plus the addendum; all unchanged)

```
2026-10-28_d005_stage1_part1_read_evidence.md  907189EB2C33D1A909DF09B1A24044367F7C72F153EB3B5863F26EF79119D8CD
2026-10-28_d005_stage1_part2_proposed_list.md  BC85E6596D5CB38D6AEFF7AB9D0C6973FE56CC774A6448AA2F1A8B984E02D17D
2026-10-28_d005_stage1_part2_evidence_note.md  7EA0AB79894BD41331B796E2FB7204ABB6CB17A9079EE5DE33A7BD56E7D68B14
2026-10-28_d005_stage1_checkpoint3_proposed_list.md  8ACB67AFB7FCFCBFF06F59499298F00702531C98FD223F5CAD2DF52A01C0CD9A
2026-10-28_d005_stage1_part3_count_log.md  14574FE27188EFA0E4D2A7D4F145EE1016FF60CC2C0F7C1E3DE88D0FD684DB2A
2026-10-28_d005_stage1_checkpoint4_proposed_list.md  C2ED3DDDF385CCB22FB5F1290D205F88965D30FF4254839DE1597D73AB9AE16A
2026-10-28_d005_stage1_part4_count_addendum.md  702D654C06A99EE4C2FA3A8542AF93F0B29ED7911FFC33F8A9C766A765FAF834
2026-10-28_d005_stage1_part5_read_addendum.md  D3839284F4CC8578F1D27DF5579DA0D4351855862339A64670DEC142F93E2738
D005_CHATGPT_STAGE1_CLOSURE_AND_MANUAL_DRAFT_AUTHORIZATION.txt (authority)  E4A260AAA18A20AE4C235EA95DD44E257B4915A2120B1C596EF43E9EC0910D7E
```

## What was and was not used

- Content was taken from the eight inputs only. D-004 facts appear as the HISTORICAL citations the inputs already make. The D-004 manual was not opened for this draft.
- No legacy source line was read and no search was run.
  - The read register in section 9 of the draft was built by script from the read-log tables and evidence hash lists in Part 1, the Part 2 evidence note and the Part 5 addendum.
  - The draft's checks compared its quotations and citations with those inputs and with the masked evidence files that the inputs list by SHA-256: reads 001 to 031, Claude's own records of the authorised reads. No legacy file was opened.
  - The quarantined lines (screening/hard_gate.py:409, v3_chain/score.py:82, orders/structure_exit_manager.py:290 and 293) were never read, so neither the draft nor its checks hold their text.
- The checks were run by check_m_d005.py, and the draft was also reviewed by an independent agent.
- After the draft, Claude's own check tooling (check_m_d005.py) ran these metadata checks on its working copies, which ChatGPT may rule on under the "no scripts" exclusion:
  - the reading clone's object counts: 816 blobs, 101 trees and 1 commit, unchanged;
  - sensitive_check.py: exit 0;
  - git status --porcelain on the V2 clone: clean.

  No legacy code was run.
