# D-007 - TIMEFRAME ANALYSIS: MANUAL MANIFEST

Status: the companion manifest of the D-007 topic manual, a topic manual of a blocked topic, added in the documentation batch of 31-Oct-2026 (DR-084) under docs/inputs/reconciliation/D006_D010_Documentation_Batch.txt. D-007's status is unchanged: BLOCKED / NOT CLOSED 2026-10-28 (DR-073); Stage 1 NOT AUTHORISED; "NO APPROVED METHOD IDENTIFIED". This manifest identifies the manual and its review notes, lists the sources the manual relies on with their SHA-256 at the base commit, maps them to the manual's sections and records the checks run on both files. It decides nothing and changes no status.

## The manual and its review notes

```
docs/agenda/d-007_timeframe-analysis/D-007_manual.md  246 lines  38,450 bytes  D92A6B5C146E24DDFCF8F4E8FC2528D67A1D09E9C6BB775A5EEE9D122B0CD78A
docs/agenda/d-007_timeframe-analysis/D-007_review_notes.md  199 lines  31,418 bytes  43CE8CA146F1AD7DB36C65236D6DF3B6BD01992ACB410E31D64479D7D7AFBD45
```

- Both files are new in this batch, beside the topic's records; no existing file of the folder is changed. The status records in the topic folder and the register row govern; the manual documents them.
- Base: 6bd80a5302c215bc91b26b3ff2da546df03b694a, GitHub main when the batch began (31-Oct-2026, 10:45 IST). Line numbers in the manual are those at the base.
- How they were written: drafted from Claude's drafting brief by the first drafting subagent, which drafted D-006 and D-007, in a staging folder outside the repository (the brief and the subagent's prompt are stored as sent in docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DRAFTING_BRIEF_AND_PROMPTS_31-OCT-2026.txt); checked and integrated by Claude; reviewed with the whole batch by independent Claude reviewers that drafted none of it (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_INDEPENDENT_REVIEW_RECORD_31-OCT-2026.txt). The review notes' section 7 records the batch's independent Claude reviews (7.2) and the findings of ChatGPT's combined pre-push audit that apply to D-007, with their disposition, pending ChatGPT's verification (7.3); ChatGPT's verdict on the revised commit goes to the batch's post-push record (OP-03).

## Sources of the manual (as they are at the base)

| # | Source | Lines | Bytes | SHA-256 at the base | Manual sections |
|---|---|---|---|---|---|
| 1 | docs/agenda/d-007_timeframe-analysis/D-007_BLOCKED_STATUS_RECORD_28-OCT-2026.txt | 98 | 9,908 | C06EADB740A9226AC156358A48795B2A0E1B3A31B5677C1A6D7C9C329C33FFC0 | 1, 2.1, 2.3, 2.4, 4, 6.1, 6.2, 7.1, 9 |
| 2 | docs/agenda/d-007_timeframe-analysis/D-007_SAFETY_GATE_FEASIBILITY_DECISION_NOTE_28-OCT-2026.txt | 46 | 4,100 | 62B9FE98EFFB62F4358A289A34F223CF7B8626C230E7DC73E2C285C7907BFD5E | 2.1, 2.4, 4, 6.1, 6.2, 7.1, 7.2 |
| 3 | docs/agenda/d-007_timeframe-analysis/D007_REGISTER_ONLY_DEPENDENCY_REVIEW_28-OCT-2026.txt | 154 | 14,600 | 20BFEDB56B846C2B966DBD18DD18F74B862535F3507CD057A4C54465D157A358 | 2.1, 2.2, 2.4, 6.1, 6.2, 7.1, 8.1, 9 |
| 4 | docs/agenda/d-007_timeframe-analysis/D-007_BLOCKED_STORAGE_MANIFEST_28-OCT-2026.txt | 25 | 2,494 | 4F5CAD81F98ADD53B6D92683C4BB2FAEAEED789F443D613B9E71CB71E72519F3 | 2.1, 2.2 |
| 5 | docs/inputs/reconciliation/D007_RAMA_EXPLICIT_GOVERNANCE_DECISION_SHEET_28-OCT-2026.txt | 30 | 1,997 | 2906B721AB8FFF92015863C9CAC6555166DB9638E22178B27F4DDFA826690803 | 2.2, 2.4, 7.1 |
| 6 | docs/inputs/reconciliation/D007_CHATGPT_REVIEW_AND_RECOMMENDATION_REGISTER_REVIEW_28-OCT-2026.txt | 28 | 2,358 | 6D77A6F1331704D8EAB8FB7B4A1A691037CA5B264E03FE0770E0F9532BEF138D | 1, 2.2, 2.4, 6.1, 7.1 |
| 7 | docs/inputs/reconciliation/D007_CLAUDE_PROMPT_ROUTE_B_GOVERNANCE_PACKAGE_CORRECTIONS_28-OCT-2026.txt | 43 | 3,739 | 7B5D02F5E66BCD2E2F8178FB91EEFF4D65322AD0CC5C5EC51C3108C149A6A5BB | 2.2 |
| 8 | docs/inputs/chatgpt_originals_and_replies/D-007_BLOCKED_POST_PUSH_GITHUB_VERIFICATION_RESULT_28-OCT-2026.txt | 50 | 4,250 | 1041C5F28DE3CFBDAC8B85D9F9CDCD184A0E3B7BB3FB82AF6F2D3987B521A5AA | 2.1, 2.2, 2.4, 5 |
| 9 | docs/inputs/reconciliation/CHATGPT_REVIEW_AND_DECISIONS_ROUTE_B_PROPOSAL_28-OCT-2026.txt | 49 | 6,124 | A1CB4DA25A237C810E3735BEE5D258A968ACDB9B629CC530522CFB1B98C33CFF | 1, 2.2, 7.2 (section 2, RB-Q2; section 3, clarification F) |
| 10 | docs/agenda/d-006_formation-pattern-analysis/D-006_DEFERRAL_STATUS_RECORD_28-OCT-2026.txt | 144 | 14,847 | 38C08179E775FB9438C6EC9E5C45ED666AB531F03451948D4CF72CCC0E942619 | 5 |
| 11 | docs/governance/V2_DECISION_REGISTER.md (changed by this batch) | 94 | 222,453 | CD88B3743C815B9F30A167A3F27DE816907DB83E6B7947540E0C1348F5A87BEF | 1, 2.2, 2.4, 3, 4, 5, 6.1, 7.1, 7.2, 8.1, 9, 10 (DR-070 to DR-079, DR-081 to DR-083) |
| 12 | docs/governance/V2_DESIGN_LOG.md (changed by this batch) | 738 | 496,265 | 01C61030C6A83B1166226DD66D81947B067E5F4B22FE6EC1A45D9B263F765522 | 2.2, 2.4, 4, 5, 8.2, 9 (section 1 lines 136 and 170 to 173; section 4 lines 368 to 374; section 6 lines 598 to 601; section 7 header and lines 699, 701, 703; sections 2, 3 and 8 searched) |
| 13 | docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt | 754 | 17,503 | 2ADE2FC33A640D94E23CFE6B8DFF2671DCE700F50B5A8E83A1343ECA9A2A1A9E | 2.2, 4, 9 (lines 201 to 219, 289, 298, 311 to 317) |
| 14 | docs/agenda/route-b/V2_REQUIREMENTS_FIRST_GOVERNANCE_CHANGE_RECORD_28-OCT-2026.txt | 147 | 12,921 | CF584D1D32C52D22CD672A1CF7BE7D0156052A7DCAFA5E2E6C1E6BC23C1F9D03 | 2.2, 2.4, 4, 6.1, 7.2, 8.1, 9 (sections 1, 2, 4 and 7) |
| 15 | docs/agenda/route-b/ROUTE_B_STORAGE_MANIFEST_28-OCT-2026.txt | 31 | 3,305 | 41E19E5591C5F21FD68A900ED0E87EA1D181A216984E8009A576EC6DDA1C4871 | 2.2 |
| 16 | docs/agenda/d-005_candle-analysis/D-005_manual.md | 242 | 29,666 | 037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA | 2.2, 4, 9 (section 1 line 13; section 7 line 156) |
| 17 | docs/agenda/d-004_market-data-requirements/D-004_manual.md | 557 | 82,675 | 23873F60430B115F3C88ADD230C2CD9485C018295874F09C587326551284E18C | 2.2, 4, 9 (status line; section 1.3 line 48; section 1.4 line 64) |
| 18 | docs/agenda/d-008_trend-market-context/D-008_v2_requirements.md | 248 | 28,525 | A4A1075465EEEFA7D95A63BC427B07239BF82CFFB11097A8DBF3B971B74628F4 | 8.2 (sections 7, 9 and 10) |
| 19 | docs/agenda/d-009_support-resistance/D-009_v2_requirements.md | 372 | 35,803 | 2C6F3723C1333710052F0B5EA374C897FE0BD882F9872BACDF72E616ADC7B351 | 8.2 (sections 2, 7, 9 and 10) |
| 20 | docs/agenda/d-010_sr-zones/D-010_v2_requirements.md | 343 | 35,336 | B707001F3D4446AB3DAB46FF78A866ACFDABC0EAF55A81D6672CC4D7B7CC0DA0 | 8.2 (sections 2.1, 2.2, 3A, 7, 9.1 and 10) |
| 21 | docs/agenda/d-010_sr-zones/D010_STAGE1_REVIEW_RESPONSE_AND_OPEN_ITEMS_30-OCT-2026.txt | 152 | 17,043 | 0D1D5C9CE37E5B912753C15AB7965BE0DBC80AC093386EC3147F827AB9CCD7A2 | 8.2 (section 3.4, UK-08; section 3.5, OD-01) |
| 22 | docs/agenda/d-011_sr-strength/D-011_v2_requirements.md | 186 | 15,960 | AAE0FD3B96ECC146E2BDC66D82AFECD57E2FF4B7CAE5966A788B80C9AD5C912A | 8.2 (sections 2.1, 3 and 10) |
| 23 | docs/agenda/d-011_sr-strength/D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 281 | 29,348 | 4145B976A45CC3C15DD9B4EC48D092FAC23128F1F985A90F4472EB0A72E914A9 | 8.2 (section 6, OQ11-06 and OQ11-07) |
| 24 | docs/agenda/d-012_sr-freshness/D-012_v2_requirements.md | 187 | 15,319 | 0AE0DA0B24E071364A5A1BA7CD2D75B597A8EE3F559C065162DB882DE10234BE | 8.2 (sections 2.1, 3 and 10) |
| 25 | docs/agenda/d-012_sr-freshness/D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 215 | 22,801 | 56056DA7243B1BFFC915AC2AD622228FB25615011D1FAB1C772665F57BB29A52 | 8.2 (section 6, OQ12-06, OQ12-07 and OQ12-12) |
| 26 | README.md (changed by this batch) | 23 | 2,125 | 7DE956DA32683AEABA462AA3986DF852AA4482D8CE9CE119212A8D7903EEBCC9 | 9 (the agenda/ line) |

- A source marked as changed by this batch is cited as it is at the base; the batch adds DR-084 and dated notes only (the batch storage manifest, docs/agenda/d006-d010-documentation-batch/).
- The review notes also cite: CLAUDE.md (SHA-256 at the base F35153FDB393FEFC5DC730FBD0D18A9CA52BA6E195A680F93705AABDAF156C56); docs/agenda/topic-workflow/TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt (SHA-256 at the base 4B1E17F1A0D0A6A4ADC856C330DCDDE4D565F815746D128E3CEE5200895AB925).
- The manual and the review notes name 33 files (8 of them ZIP packages) that the stored records name but that are not in the repository; where each is named, the text says that it is not stored or was not received. Nothing in the manual depends on their content beyond what the stored records say of them.

## Section coverage

| Section | Heading | Lines | Sources used |
|---|---|---|---|
| 1 | Document status, purpose and scope | 3 to 17 | 4 |
| 2 | Records and how to read them | 18 to 76 | 16 |
| 3 | Evidence labels | 77 to 92 | 1 |
| 4 | The topic | 93 to 102 | 8 |
| 5 | What happened (28-Oct-2026, IST) | 103 to 129 | 4 |
| 6 | What the records establish and what they do not | 130 to 164 | 6 |
| 7 | The block: its recorded reasons and what the records say would be needed to lift it | 165 to 190 | 8 |
| 8 | Topics that depend on it | 191 to 229 | 12 |
| 9 | Cross-references and bookmarks | 230 to 241 | 9 |
| 10 | Status | 242 to 246 | 1 |

## Completeness and checks (by Claude, on the files as committed)

| Check | Result |
|---|---|
| Files of the topic folder at the base named in the manual (section 2) | 4 of 4 |
| docs/inputs/ files at the base whose names carry D-007 or D007 (docs/inputs/legacy_baseline/ excluded by git pathspec; Claude's report), named in the manual or the review notes | 4 of 4 |
| Register rows at the base that name D-007, cited in the manual | 9 of 9 (DR-072, DR-073, DR-074, DR-075, DR-076, DR-077, DR-078, DR-079, DR-081) |
| V2_DESIGN_LOG section 7 bookmark rows at the base that name D-007, cited in the manual | 3 of 3 (BK-75, BK-77, BK-79) |
| Quoted fragments of 8 characters or more found word for word (whitespace normalised) in the V2 repository at the base, in the batch's stored inputs or in the reviewers' reports that the batch's independent review record reproduces | manual 148 of 148; review notes 64 of 64 |
| Record IDs cited that exist in the V2 repository at the base or are added by this batch (DR-084, DR-085, W-15) | manual 48 of 48; review notes 36 of 36 |
| ASCII only, LF line endings, no trailing whitespace | both files |
| Labels in the manual: UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION | 3; 2; 3 |

## Provenance and limits

- Evidence: V2 records and the frozen manuals (A-001 to D-005) at the base only, with the batch's stored inputs. No legacy repository, clone, host, VM, runtime, broker or database was used (option C, DR-073), and that nothing under docs/inputs/legacy_baseline/ was used for the manual or the review notes is Claude's report, not independently verified (PD-01); how file lists and searches touched that folder is in the last bullet of this section.
- Labels: the manual's section 3 lists them; each statement carries the label of its source, as the drafting brief requires.
- Process record of the drafting and checks (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DECISION_RECORD_31-OCT-2026.txt, section 5): none by the first drafting subagent, whose file lists and searches excluded docs/inputs/legacy_baseline/ by pathspec or named their folders; it did not run the git ls-files pipe that its prompt suggested (Claude's check of the subagent's transcript, not stored). Batch-wide, Claude's own completeness scripts and its first runs of this generator listed the base tree with git ls-tree before dropping docs/inputs/legacy_baseline/ (PD-01). Found or recognised after Rama's acceptance, and waiting for his and ChatGPT's decision (OB-05): Claude's routine untracked-file, unstaged-change and rebuild checks ran without the exclusion, and commit-to-commit diffs ran without a pathspec, Claude's until 13:22 IST and the first reviewer's (NOT ESTABLISHED whether git read inside the folder). This manifest was generated with the folder excluded by git pathspec (Claude's report). Rama accepted PD-01 as a disclosed, one-time process deviation with corrective controls (DR-085; W-15); the facts found or recognised after his acceptance wait for his and ChatGPT's decision. Minor: the drafting subagent wrote helper scripts into Claude's scratchpad folder, outside its staging folder.

## Status

- D-007: BLOCKED / NOT CLOSED 2026-10-28 (DR-073); Stage 1 NOT AUTHORISED; "NO APPROVED METHOD IDENTIFIED". Unchanged by this batch.
- Nothing in this manifest changes a decision or status. Where it and a record differ, the record governs.
