# D-010 - S&R ZONES: MANUAL MANIFEST

Status: the companion manifest of the D-010 topic manual, a V2 topic manual (DR-083), added in the documentation batch of 31-Oct-2026 (DR-084) under docs/inputs/reconciliation/D006_D010_Documentation_Batch.txt. D-010's status is unchanged: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077), at requirement level only. This manifest identifies the manual and its review notes, lists the sources the manual relies on with their SHA-256 at the base commit, maps them to the manual's sections and records the checks run on both files. It decides nothing and changes no status.

## The manual and its review notes

```
docs/agenda/d-010_sr-zones/D-010_manual.md  420 lines  69,783 bytes  502E92D22095B899C159A2E66214522290AE3C1148B4934AE34DB71D0DCD828D
docs/agenda/d-010_sr-zones/D-010_review_notes.md  273 lines  47,303 bytes  D358AFFA17A7BC4757BEE1639BCFB6745465B3B519A778BE06509D33E3A00D25
```

- Both files are new in this batch, beside the topic's records; no existing file of the folder is changed. D-010_v2_requirements.md governs every requirement and decision; the manual explains it.
- Base: 6bd80a5302c215bc91b26b3ff2da546df03b694a, GitHub main when the batch began (31-Oct-2026, 10:45 IST). Line numbers in the manual are those at the base.
- How they were written: drafted from Claude's drafting brief by the fourth drafting subagent, in a staging folder outside the repository (the brief and the subagent's prompt are stored as sent in docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DRAFTING_BRIEF_AND_PROMPTS_31-OCT-2026.txt); checked and integrated by Claude; reviewed with the whole batch by independent Claude reviewers that drafted none of it (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_INDEPENDENT_REVIEW_RECORD_31-OCT-2026.txt). The review notes' section 7 records the batch's independent Claude reviews (7.2) and the findings of ChatGPT's combined pre-push audit that apply to D-010, with their disposition, pending ChatGPT's verification (7.3); ChatGPT's verdict on the revised commit goes to the batch's post-push record (OP-03).

## Sources of the manual (as they are at the base)

| # | Source | Lines | Bytes | SHA-256 at the base | Manual sections |
|---|---|---|---|---|---|
| 1 | docs/agenda/d-010_sr-zones/D-010_v2_requirements.md | 343 | 35,336 | B707001F3D4446AB3DAB46FF78A866ACFDABC0EAF55A81D6672CC4D7B7CC0DA0 | 1, 2, 4, 5, 6, 7, 8, 9, 10, 11 |
| 2 | docs/agenda/d-010_sr-zones/D-010_STAGE2_STATUS_RECORD_AND_EXECUTION_PLAN_30-OCT-2026.txt | 256 | 29,379 | 3E135C23FF331CC64E4AD6CC55CBFE0DFD3427E60A6D1891AB1DBE6A9BDE2689 | 2, 5, 6 |
| 3 | docs/agenda/d-010_sr-zones/D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 73 | 10,051 | CB8D5DD39C7F75CF4963C22ED89F04007DFA97DCA998E5D25F0537F5346A5F29 | 2, 5, 6, 7, 10 |
| 4 | docs/agenda/d-010_sr-zones/D-010_STORAGE_MANIFEST_30-OCT-2026.txt | 115 | 14,573 | 50C143D46339FC97227FF82F4632E3B78E0FC6B0E6DB3B7DBD4D650AB0F916F5 | 2 |
| 5 | docs/agenda/d-010_sr-zones/D-010_STAGE0_SOURCE_INVENTORY_AND_RESEARCH_PLAN_30-OCT-2026.txt | 273 | 34,339 | C82D6766E5796941009DD211AC3866E47D3F495A8F4FEE4C3778F9325CB9B758 | 2, 5, 10 |
| 6 | docs/agenda/d-010_sr-zones/D-010_STAGE0_CANDIDATE_SOURCE_REGISTER_30-OCT-2026.txt | 174 | 24,925 | F14D6D884B779D63529B1FFD76AC547B2F6D50AE447D0097B993605F49E552AB | 2, 4, 5 |
| 7 | docs/agenda/d-010_sr-zones/D-010_STAGE0_OPEN_QUESTIONS_AND_SCOPE_BOUNDARY_30-OCT-2026.txt | 138 | 17,345 | 6B2FFFACB9ACBDB764BAA582ED63B59C5A3106F82AACC49D270822442B8604CD | 2, 4, 5, 6, 7 |
| 8 | docs/agenda/d-010_sr-zones/D-010_STAGE1_EVIDENCE_SHEET_30-OCT-2026.txt | 574 | 49,887 | 2976A1BC9FE885CB16F17BE6FA8DD0A34A3D2017D4504F176D8D6D6F89818906 | 2, 3, 4, 5, 7, 8, 9 |
| 9 | docs/agenda/d-010_sr-zones/D-010_STAGE1_READ_LOG_30-OCT-2026.txt | 150 | 16,358 | 75A7B6CF24B5A33D788DB2AB6888DE6F7627BC13EB81E2FC328E8FBEF6E7D098 | 2, 5, 8, 10 |
| 10 | docs/agenda/d-010_sr-zones/D010_STAGE1_SUPPLEMENTAL_RAMA_INPUT_REGISTER_30-OCT-2026.txt | 264 | 28,809 | A1F5DD7A53C7A678670F1082EBF231744787A2E491045850FD4FB976C0B933AC | 2, 3, 5, 6, 7, 9 |
| 11 | docs/agenda/d-010_sr-zones/D010_STAGE1_REVIEW_RESPONSE_AND_OPEN_ITEMS_30-OCT-2026.txt | 152 | 17,043 | 0D1D5C9CE37E5B912753C15AB7965BE0DBC80AC093386EC3147F827AB9CCD7A2 | 2, 5, 6, 7, 9 |
| 12 | docs/agenda/d-010_sr-zones/D010_STAGE1_CHANGE_CONTROL_NOTE_30-OCT-2026.txt | 50 | 3,897 | 7544806916908267CBC74768422BE391E59506A3A3647E95D74D971D253ECED7 | 2, 5, 10 |
| 13 | docs/agenda/d-010_sr-zones/D-010_STAGE1_CLOSURE_RECORD_30-OCT-2026.txt | 101 | 8,293 | C1189C48B274ADF9AA6EE31796F108BD4033273703E5430E88E8EF346BA254D4 | 2, 5 |
| 14 | docs/agenda/d-010_sr-zones/D010_STAGE1_SUPPLEMENTAL_CLARIFICATIONS_30-OCT-2026.txt | 161 | 15,512 | 3C9194921EEC38EB968C0F9200CB8992476D45B69D08168756C996ACECF3E640 | 2, 3, 5, 6, 9, 10 |
| 15 | docs/agenda/d-010_sr-zones/D010_STAGE1_PACKAGE_MANIFEST_AND_HANDOFF_30-OCT-2026.txt | 129 | 13,553 | 474756128527A2DE3E7ACF21FB85BE0DBA42658D84B7E03A5069CF0A76B1083D | 2, 5, 10 |
| 16 | docs/inputs/reconciliation/D-010_CLAUDE_RESEARCH_PROMPT_STAGE0_30-OCT-2026.txt | 158 | 10,084 | AB2B21DD745B6CBFA846731FA5E40BB81F143EF9499453DE9C974B4CDF2C9D84 | 2, 4, 5 |
| 17 | docs/inputs/reconciliation/D-010_STAGE0_CHATGPT_REVIEW_CHECKLIST_30-OCT-2026.txt | 28 | 2,199 | CDC4785755A60DFDE559403C91A2962D7C3A57F8BA88D4A89B733A5721287009 | 2, 5 |
| 18 | docs/inputs/reconciliation/D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026.txt | 181 | 14,972 | B358835F1F3603EF829B6B0D420BF6BFF6804F2540256A980198A76F42B46197 | 2, 4, 5 |
| 19 | docs/inputs/reconciliation/D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026_BLANK_AS_ISSUED.txt | 173 | 6,662 | 0260A1C4F73145A8694EC892AD89978A574C3772C68ACDB5611B705D9FA338C2 | 2 |
| 20 | docs/inputs/reconciliation/D010_Stage0_Review_Stage1_Authorization.txt | 163 | 10,318 | 238EDFDC52398E0C126B308DFCC6738C4BF182DA3A71AFEA93D0FE53B01BDFE3 | 2, 4, 5, 7, 10 |
| 21 | docs/inputs/reconciliation/D010_Stage1_Review.txt | 163 | 16,223 | D42AEBE15AAFF136F0B94DC320C906486D18AB8BAE6B3B565B26AB28576BBCCB | 2, 5 |
| 22 | docs/inputs/reconciliation/D010_Supplemental_Package_Review_30Oct2026.txt | 137 | 10,101 | 0F24C4F244979FED667E13E96112AE6027A8213F67862DEC4537C8F8B61EED00 | 2, 5, 7, 10 |
| 23 | docs/inputs/reconciliation/D010_OQ10-18_Administrative_Closure_Instruction.txt | 49 | 2,597 | 685FF0108451D82143F7F523DB53DCA50649BA553E3F4E4AA19F7D3CF3E608B1 | 2, 5 |
| 24 | docs/inputs/reconciliation/D010_Claude_Instruction_Step1_Closure_Only.txt | 62 | 3,380 | EAF4642C48E78FAA1BD2694C4FB9A2F81515FC2FE38B6242BEE2B20312EA0132 | 2, 5 |
| 25 | docs/inputs/reconciliation/D010_Step2_Reconciliation_Review_30Oct2026.txt | 214 | 15,087 | 0852522F9C212D04F2A3B556BA25E8FB1B8C03680BCC27B441587FAFF346BCCB | 1, 2, 4, 5, 7, 8 |
| 26 | docs/inputs/reconciliation/D010_Claude.txt | 111 | 8,123 | 18B4BB875189AEF5EFED71BD42A82A674BFEBC675C257DB191D1A83A69B81584 | 2, 5 |
| 27 | docs/inputs/reconciliation/D010Review.txt | 53 | 3,080 | 2D8AD03F9E54B25F71C3C8DA33ACB50229323D15824A1FB39B330CA1A1C70D87 | 2, 5, 10 |
| 28 | docs/inputs/reconciliation/D010_Next_Instructions.txt | 104 | 4,720 | FA210A2CDFB5FC56EFBF5D4EFA1AB3EDE70C178FA818CC889480E8129BB19F7D | 2, 5 |
| 29 | docs/inputs/reconciliation/D010_Correction_Only_Instructions.txt | 44 | 2,254 | 3BE73E7C5AEEF4847E880F694DBE3E8B32FABB81D90BFF2E0983CEB52B7BDF44 | 2, 5, 10 |
| 30 | docs/inputs/reconciliation/TS_D010_Authorise.txt | 119 | 9,147 | BAC99AD3AB0FC28EAB0EFEF2D9DDAB5F1FBCCA2E64F9558A3D0827192A87386D | 2, 5, 6, 7 |
| 31 | docs/inputs/reconciliation/EQUITY_L_AS_RECEIVED_2026-10-30.csv | 2,621 | 185,023 | E99E5163B9EDD041D3BED2C31C60C26BA954E741CA2D87A4594FFC3FEA9C4CEC | 2 (named only; its content as the register, section 5, records it; not opened for this manual) |
| 32 | docs/inputs/chatgpt_originals_and_replies/D-010_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 59 | 6,004 | 5E74C4BA3D9DAE23EC6421D4CD2515BE39C720B0D4603C75F1EF309F25FEAD33 | 2, 5, 9 |
| 33 | docs/governance/V2_DECISION_REGISTER.md (changed by this batch) | 94 | 222,453 | CD88B3743C815B9F30A167A3F27DE816907DB83E6B7947540E0C1348F5A87BEF | 1, 2, 5, 6, 7, 8, 9, 10, 11 |
| 34 | docs/governance/V2_DESIGN_LOG.md (changed by this batch) | 738 | 496,265 | 01C61030C6A83B1166226DD66D81947B067E5F4B22FE6EC1A45D9B263F765522 | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 |
| 35 | docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt | 754 | 17,503 | 2ADE2FC33A640D94E23CFE6B8DFF2671DCE700F50B5A8E83A1343ECA9A2A1A9E | 4, 5, 8 |
| 36 | docs/agenda/d-009_support-resistance/D-009_v2_requirements.md | 372 | 35,803 | 2C6F3723C1333710052F0B5EA374C897FE0BD882F9872BACDF72E616ADC7B351 | 1, 4, 6, 7, 9, 10, 11 (OQ9- rows as D-010's records quote them; their status checked) |
| 37 | docs/agenda/d-011_sr-strength/D-011_v2_requirements.md | 186 | 15,960 | AAE0FD3B96ECC146E2BDC66D82AFECD57E2FF4B7CAE5966A788B80C9AD5C912A | 9 |
| 38 | docs/agenda/d-012_sr-freshness/D-012_v2_requirements.md | 187 | 15,319 | 0AE0DA0B24E071364A5A1BA7CD2D75B597A8EE3F559C065162DB882DE10234BE | 9 |
| 39 | docs/agenda/a-001_system-purpose/A-001_manual.md | 101 | 9,713 | 6FC9D13B36E8243C8BE2600C61654EE19DB011DA0D56C449D17D0B22DB39D05D | 4, 8 (section 9, line 63, cited through NR9-11 and E10-17; not re-read) |
| 40 | docs/agenda/b-006_worker-initialization/B-006_manual.md | 255 | 56,839 | 3FA676C753ADA3D5D5AC0FB6930AFA6F6DA9B4FEBD7CFA20804093EF21371C10 | 8 (row TH11, cited through E9-30 and E10-25; not re-read) |
| 41 | docs/agenda/c-008_signal-processing/C-008_manual.md | 727 | 145,246 | 43B2DDBC9DD5B6362CB81B8AE72B2A0E6720CF90100B09975C2EC71CD227592B | 8 (section 9.2, cited through E9-30 and E10-25; not re-read) |
| 42 | docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md | 1,007 | 117,809 | 5945CFA1D0815608687C36305BEA528307D758C97BF64A5875B8D4406F739F34 | 8 (sections 7.6 and 10.2, cited through E9-21 and E10-24; not re-read) |
| 43 | docs/agenda/d-004_market-data-requirements/D-004_manual.md | 557 | 82,675 | 23873F60430B115F3C88ADD230C2CD9485C018295874F09C587326551284E18C | 8 (section 1.3, line 50, and section 7, line 230, cited through E10-19 and E10-26; not re-read) |
| 44 | docs/agenda/d-005_candle-analysis/D-005_manual.md | 242 | 29,666 | 037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA | 8 (section 1, line 15, and section 7, line 158, cited through E10-21 and E10-22; not re-read) |

- A source marked as changed by this batch is cited as it is at the base; the batch adds DR-084 and dated notes only (the batch storage manifest, docs/agenda/d006-d010-documentation-batch/).
- The review notes also cite: CLAUDE.md (SHA-256 at the base F35153FDB393FEFC5DC730FBD0D18A9CA52BA6E195A680F93705AABDAF156C56); docs/agenda/topic-workflow/TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt (SHA-256 at the base 1857F5FD8984AA1AD2179D9C0B39CD8E86E6FF93AF14C75581A40B050C9A858C); docs/inputs/reconciliation/Topic_Workflow.txt (SHA-256 at the base 6DF2A5FBD9820DA87767636D87580C97CA335CA83D30FAE1D748AA0A2A8A3444); docs/inputs/reconciliation/Topic_Workflow_Final_PrePush_Audit.txt (SHA-256 at the base B9AE01B2908C63943B5A679A663AB58BF37749281385CEE1E8E31C9D46222F7B); docs/inputs/reconciliation/Topic_Workflow_PrePush_Review.txt (SHA-256 at the base 0670B86995A32C17A6AFD8B21470E97C908C937ACD8B0732E68FC73DA173696C).
- The manual and the review notes name 2 files that the stored records name but that are not in the repository; where each is named, the text says that it is not stored or was not received. Nothing in the manual depends on their content beyond what the stored records say of them.

## Section coverage

| Section | Heading | Lines | Sources used |
|---|---|---|---|
| 1 | Document status, purpose and scope | 3 to 17 | 5 |
| 2 | Records and how to read them | 18 to 89 | 34 |
| 3 | Evidence labels | 90 to 108 | 4 |
| 4 | The question the topic answers | 109 to 126 | 12 |
| 5 | Course of the topic | 127 to 188 | 32 |
| 6 | What is recorded as decided | 189 to 244 | 11 |
| 7 | What is not decided: open questions, provisional positions, raw ideas, unknowns | 245 to 317 | 13 |
| 8 | Legacy evidence | 318 to 341 | 13 |
| 9 | Cross-topic boundaries, dependencies and bookmarks | 342 to 390 | 11 |
| 10 | Known defects, process deviations and lessons, as recorded | 391 to 414 | 14 |
| 11 | Status | 415 to 420 | 4 |

## Completeness and checks (by Claude, on the files as committed)

| Check | Result |
|---|---|
| Files of the topic folder at the base named in the manual (section 2) | 15 of 15 |
| docs/inputs/ files at the base whose names carry D-010 or D010 (docs/inputs/legacy_baseline/ excluded by git pathspec; Claude's report), named in the manual or the review notes | 16 of 16 |
| Register rows at the base that name D-010, cited in the manual | 4 of 4 (DR-077, DR-078, DR-079, DR-083) |
| V2_DESIGN_LOG section 7 bookmark rows at the base that name D-010, cited in the manual | 4 of 4 (BK-79, BK-80, BK-81, BK-85) |
| RQ and OQ IDs of D-010_v2_requirements.md cited in the manual | 25 of 25 (RQ10-01 to RQ10-05 and OQ10-01 to OQ10-20) |
| Quoted fragments of 8 characters or more found word for word (whitespace normalised) in the V2 repository at the base, in the batch's stored inputs or in the reviewers' reports that the batch's independent review record reproduces | manual 329 of 329; review notes 178 of 178 |
| Record IDs cited that exist in the V2 repository at the base or are added by this batch (DR-084, DR-085, W-15) | manual 88 of 88; review notes 90 of 90 |
| ASCII only, LF line endings, no trailing whitespace | both files |
| Labels in the manual: UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION | 9; 6; 2 |

## Provenance and limits

- Evidence: V2 records and the frozen manuals (A-001 to D-005) at the base only, with the batch's stored inputs. No legacy repository, clone, host, VM, runtime, broker or database was used (option C, DR-073), and that nothing under docs/inputs/legacy_baseline/ was used for the manual or the review notes is Claude's report, not independently verified (PD-01); how file lists and searches touched that folder is in the last bullet of this section.
- Labels: the manual's section 3 lists them; each statement carries the label of its source, as the drafting brief requires.
- Process record of the drafting and checks (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DECISION_RECORD_31-OCT-2026.txt, section 5): PD-01: three git ls-files pipes passed docs/inputs/legacy_baseline/ names through memory, and one git grep over docs/inputs read the text of that folder's files; that its output named no file there and that nothing from the folder was displayed or used is Claude's report, not independently verified. Batch-wide, Claude's own completeness scripts and its first runs of this generator listed the base tree with git ls-tree before dropping docs/inputs/legacy_baseline/ (PD-01). Found or recognised after Rama's acceptance, and waiting for his and ChatGPT's decision (OB-05): Claude's routine untracked-file, unstaged-change and rebuild checks ran without the exclusion, and commit-to-commit diffs ran without a pathspec, Claude's until 13:22 IST and the first reviewer's (NOT ESTABLISHED whether git read inside the folder). This manifest was generated with the folder excluded by git pathspec (Claude's report). Rama accepted PD-01 as a disclosed, one-time process deviation with corrective controls (DR-085; W-15); the facts found or recognised after his acceptance wait for his and ChatGPT's decision. Minor: the drafting subagent wrote helper scripts into Claude's scratchpad folder, outside its staging folder.

## Status

- D-010: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-077), at requirement level only. Unchanged by this batch.
- Nothing in this manifest changes a decision or status. Where it and a record differ, the record governs.
