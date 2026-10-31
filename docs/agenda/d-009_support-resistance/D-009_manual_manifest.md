# D-009 - SUPPORT & RESISTANCE: MANUAL MANIFEST

Status: the companion manifest of the D-009 topic manual, a V2 topic manual (DR-083), added in the documentation batch of 31-Oct-2026 (DR-084) under docs/inputs/reconciliation/D006_D010_Documentation_Batch.txt. D-009's status is unchanged: DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076); OQ9-01 to OQ9-25 OPEN. This manifest identifies the manual and its review notes, lists the sources the manual relies on with their SHA-256 at the base commit, maps them to the manual's sections and records the checks run on both files. It decides nothing and changes no status.

## The manual and its review notes

```
docs/agenda/d-009_support-resistance/D-009_manual.md  414 lines  70,129 bytes  0E9A85E10EE9CD153E68CA5BB06B3D20221DB6EA4B0F858A8BCC0E928177773F
docs/agenda/d-009_support-resistance/D-009_review_notes.md  245 lines  42,326 bytes  03AEAB3E6EE966F39D3E91BABC437979B6486742089746BE4FCEF835DAC1B035
```

- Both files are new in this batch, beside the topic's records; no existing file of the folder is changed. D-009_v2_requirements.md governs every requirement and decision; the manual explains it.
- Base: 6bd80a5302c215bc91b26b3ff2da546df03b694a, GitHub main when the batch began (31-Oct-2026, 10:45 IST). Line numbers in the manual are those at the base.
- How they were written: drafted from Claude's drafting brief by the third drafting subagent, in a staging folder outside the repository (the brief and the subagent's prompt are stored as sent in docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DRAFTING_BRIEF_AND_PROMPTS_31-OCT-2026.txt); checked and integrated by Claude; reviewed with the whole batch by independent Claude reviewers that drafted none of it (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_INDEPENDENT_REVIEW_RECORD_31-OCT-2026.txt). The review notes' section 7 records the batch's independent Claude reviews (7.2) and the findings of ChatGPT's combined pre-push audit that apply to D-009, with their disposition, pending ChatGPT's verification (7.3); ChatGPT's verdict on the revised commit goes to the batch's post-push record (OP-03).

## Sources of the manual (as they are at the base)

| # | Source | Lines | Bytes | SHA-256 at the base | Manual sections |
|---|---|---|---|---|---|
| 1 | docs/agenda/d-009_support-resistance/D-009_v2_requirements.md | 372 | 35,803 | 2C6F3723C1333710052F0B5EA374C897FE0BD882F9872BACDF72E616ADC7B351 | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 |
| 2 | docs/agenda/d-009_support-resistance/D-009_STAGE0_SOURCE_INVENTORY_AND_RESEARCH_PLAN_29-OCT-2026.txt | 154 | 13,549 | 13B3E805F286D437C375E2D0891734E21EC09DA75EFD3FF02E9339A139920FCB | 2, 5 |
| 3 | docs/agenda/d-009_support-resistance/D-009_STAGE0_CANDIDATE_SOURCE_REGISTER_29-OCT-2026.txt | 170 | 26,323 | EA9BA594936B84F59A4E5BEAA2B0071117D9B23F608CCE4AA57957DBD3B34778 | 2 |
| 4 | docs/agenda/d-009_support-resistance/D-009_STAGE0_OPEN_QUESTIONS_AND_SCOPE_BOUNDARY_29-OCT-2026.txt | 91 | 11,211 | DE24DC9D68BEAA6A529F8F493662C1ED070B0D2FCA90FE2183334F793D07A19B | 2, 7 |
| 5 | docs/agenda/d-009_support-resistance/D-009_STAGE1_EVIDENCE_SHEET_29-OCT-2026.txt | 275 | 58,389 | 1B0373ACE862169539D3AC5DF915F46D42F0CA0EFEB94F0847B92F4DF50791E2 | 2, 4, 8, 10 |
| 6 | docs/agenda/d-009_support-resistance/D-009_STAGE1_READ_LOG_29-OCT-2026.txt | 229 | 24,438 | 1A86F8E060C1064B29AA8484F336BBFF4AF3B6BE23D754BC3A392B0D0DCDAD16 | 2, 5, 10 |
| 7 | docs/agenda/d-009_support-resistance/D-009_STAGE1_CANDIDATE_RAMA_ANSWER_SHEET_QUESTIONS_29-OCT-2026.txt | 28 | 6,751 | 183A91020048CB92969616503117C0B2E21FB4D32D504D319271E480AB951335 | 2, 4 |
| 8 | docs/agenda/d-009_support-resistance/D-009_CLAUDE_REVIEW_NOTES_ON_RAMA_RAW_IDEAS_29-OCT-2026.txt | 113 | 8,483 | FFA067EE0A003E594639FD3FEA92C2A0F1FB8A6A9FA9FFEE26583743F63AE4EA | 2, 5 |
| 9 | docs/agenda/d-009_support-resistance/D009_RAW_IDEAS_STATUS_AND_V2_DESIGN_DISCUSSION_REGISTER_REV1_29-OCT-2026.txt | 223 | 16,301 | 48B822516B8C56262E52B8DF062DB6BC16643E765FE280722AB1E612392F9519 | 2, 7, 10 |
| 10 | docs/agenda/d-009_support-resistance/D-009_RAW_IDEAS_REGISTER_REV1_CHANGE_LOG_29-OCT-2026.txt | 47 | 9,745 | B5F751D97953A49048BB14848055309C7FDB13E5AD5651CFD29C57832D9F9B6F | 2, 5 |
| 11 | docs/agenda/d-009_support-resistance/D-009_RAW_IDEAS_REGISTER_REV1_PROVENANCE_CHECKLIST_29-OCT-2026.txt | 23 | 3,407 | FB3E6B874F474FF4ABE21FE2345C75D96BEEC304D842D7CD731C197FB742A492 | 2, 10 |
| 12 | docs/agenda/d-009_support-resistance/D-009_STORAGE_PLAN_30-OCT-2026.txt | 306 | 47,434 | 3C79283384134F13127458EACBB12D5D30CCFFB4180D6F95CA5A8380024FBF83 | 2, 5, 6, 9 |
| 13 | docs/agenda/d-009_support-resistance/D-009_PACKAGE_CHANGE_LOG_30-OCT-2026.txt | 272 | 30,773 | 52DA800C8EE14A14581873EABFA0D7C444CBAED7E48E10C60610C1C0206497A4 | 2, 5, 6, 7, 9, 10 |
| 14 | docs/agenda/d-009_support-resistance/D-009_STORAGE_MANIFEST_30-OCT-2026.txt | 96 | 12,902 | 66C386828131A08E435E738BE6C1CC27DA0169F687760804C19A381178EF55F4 | 2, 10 |
| 15 | docs/inputs/reconciliation/CHATGPT_REVIEW_D008_POST_PUSH_AND_D009_NEXT_STEP_29-OCT-2026.txt | 53 | 3,261 | 5A7CE9D155F856A1B0ABC050FC43065349989B577150AC27350B945733845FC6 | 2, 5 |
| 16 | docs/inputs/reconciliation/D009_CLAUDE_PROMPT_STAGE0_INVENTORY_ONLY_29-OCT-2026.txt | 46 | 3,011 | 980827D11C72F6C3B068F8323FD1696751DE9833AC321D7BC60F955F45945EE9 | 2 |
| 17 | docs/inputs/reconciliation/D009_STAGE0_STANDING_SCOPE_AND_WORKING_ANSWERS_29-OCT-2026.txt | 34 | 2,308 | 400F828B31432EE5E8FC34D4DB248BABA233D0839F4BEB363D03E77328A70932 | 2 |
| 18 | docs/inputs/reconciliation/CHATGPT_REVIEW_D009_STAGE0_AND_STAGE1_DECISION_29-OCT-2026.txt | 44 | 5,231 | BA74C81F967EED922351FA1518E7B1364B04EF47A5D588905C9E6BC917B5EBA2 | 2, 5 |
| 19 | docs/inputs/reconciliation/D009_CLAUDE_PROMPT_STAGE1_BOUNDED_EVIDENCE_READS_29-OCT-2026.txt | 61 | 4,885 | 7422ADD78CF27CF955E4FB8A481021D3C6A917D85CDAC36BC9C09B8A2406BF29 | 2 |
| 20 | docs/inputs/reconciliation/CHATGPT_REVIEW_D009_STAGE1_EVIDENCE_PACKAGE_29-OCT-2026.txt | 41 | 4,087 | 2F5D92DF30D51469BAF61EDE3056150881A9F4A6868A6F88C4D6EF011B1F76D8 | 2, 5, 10 |
| 21 | docs/inputs/reconciliation/D009_CLAUDE_HANDOFF_AFTER_STAGE1_REVIEW_29-OCT-2026.txt | 27 | 1,552 | 275A732E619A5BA5E1EED3FAA2011949B98AAE5D974F1899122479BE164D1091 | 2 |
| 22 | docs/inputs/reconciliation/D-009_RAMA_ANSWER_SHEET_29-OCT-2026.txt | 118 | 9,305 | 3C9C23F75B67DC7EDF4328614F34DF014F21F89BA1566C6BA4DE559DF8533710 | 2, 4 |
| 23 | docs/inputs/reconciliation/D009_RAMA_ANSWER_GUIDE_REAL_MARKET_EXAMPLES_AND_CHATGPT_SUGGESTIONS_29-OCT-2026.txt | 168 | 17,314 | 00291993845DDFF0480AFFD6744594A8F443595CAE5E4CC0292BD44E4A56B831 | 1, 2, 4, 6 |
| 24 | docs/inputs/reconciliation/D009_RAW_IDEAS_STATUS_AND_V2_DESIGN_DISCUSSION_REGISTER_29-OCT-2026.txt | 89 | 8,220 | 9748C879C1C03F224D7D081D2347A9EEA131EA72F6D9E47C288F017570DE3E23 | 2 |
| 25 | docs/inputs/reconciliation/CHATGPT_REVIEW_D009_CLAUDE_RAW_IDEAS_NOTES_29-OCT-2026.txt | 74 | 6,866 | C0AB93420189CD06BA7D0A6774C83BB416802893503EB0979EF14EA532D92DF6 | 2, 5, 10 |
| 26 | docs/inputs/reconciliation/CHATGPT_REVIEW_D009_RAW_IDEAS_REGISTER_REV1_29-OCT-2026.txt | 35 | 3,091 | D33B2FC4EC8665AA9DD9DF50E3E3073BCE9A241D9E0589B58865CB11AA83E3F5 | 2, 5 |
| 27 | docs/inputs/reconciliation/D009_FINAL_REVIEW.doc | 9 | 14,848 | E23884184849D75A0D688D8442E6E8AA7E872A188CBC28FC062ED8BC3C015450 | 2, 5 |
| 28 | docs/inputs/reconciliation/D009_NEXT_STEP_CLAUDE_HANDOFF_30-OCT-2026_RESHARE.txt | 62 | 5,144 | CAB27ABF1614DB4D87DBFE5190D42D8A35E24285F179008A230BE1A0908C1015 | 2, 5 |
| 29 | docs/inputs/reconciliation/CHATGPT_REVIEW_D009_THREE_DRAFTS_AND_PACKAGE_DECISIONS_30-OCT-2026.txt | 141 | 10,774 | 4C629FF36F959D4EBB09A0E60B130613F1BD57E71A3ABB5CFAA6FF125F5EA91D | 2, 5, 6 |
| 30 | docs/inputs/reconciliation/D009_CLAUDE_PROMPT_CORRECT_THREE_DRAFTS_30-OCT-2026.txt | 58 | 3,451 | 73C9869EE42C1E9D1A2EAEB785212F85BA91F443F127FF028C70164078AAF15B | 2 |
| 31 | docs/inputs/reconciliation/CHATGPT_FINAL_REVIEW_D009_REVISION1_AND_STORAGE_AUTHORIZATION_30-OCT-2026.txt | 91 | 5,553 | 482C7119ECBA70744C133F798DA1D92125974786EDD4E5A56C9270FB02EE31D5 | 2, 5, 10 |
| 32 | docs/inputs/reconciliation/D009_CLAUDE_PROMPT_STORAGE_EXECUTION_30-OCT-2026.txt | 48 | 3,167 | 66506399F3D44D06224FBA5B61D5389BEC77F3055735F873051A6CDB470CFFB1 | 2, 5 |
| 33 | docs/inputs/reconciliation/CHATGPT_POST_PUSH_REVIEW_D009_30-OCT-2026.txt | 151 | 6,386 | 9CFE71AE0ECF997E55A5FFE05B5571FE853B7BDC9CCF7A250710760C049C7E5B | 2, 5, 10 |
| 34 | docs/inputs/chatgpt_originals_and_replies/D-009_STORAGE_PLAN_DRAFT_REV0_30-OCT-2026.txt | 206 | 22,569 | 3D21EFD26551E93642D32EF7775095280908A8FC94244DADB3F2A2CEFC2776B8 | 2, 5 |
| 35 | docs/inputs/chatgpt_originals_and_replies/D-009_PACKAGE_CHANGE_LOG_DRAFT_REV0_30-OCT-2026.txt | 77 | 9,765 | 51549D87C77987FEDC22660BE4392F9F072EDA2B966539697890646EC4A138AE | 2, 5 |
| 36 | docs/inputs/chatgpt_originals_and_replies/D-009_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 152 | 16,387 | 07CEC3DC505E16243F559DC67632BC24A60EE32C2F92F27C9412FCA9C43F7A0D | 2, 5, 10 |
| 37 | docs/inputs/manuals/D-009_SUPPORT_RESISTANCE_V2_REQUIREMENTS_REV0_30-OCT-2026.md | 356 | 33,765 | 28A95C1D9FD3AFBDFD753446446AED5B172B5E9719249D2A76A32485A835C46C | 2, 5 |
| 38 | docs/governance/V2_DECISION_REGISTER.md (changed by this batch) | 94 | 222,453 | CD88B3743C815B9F30A167A3F27DE816907DB83E6B7947540E0C1348F5A87BEF | 1, 2, 3, 4, 5, 6, 7, 9, 11 |
| 39 | docs/governance/V2_DESIGN_LOG.md (changed by this batch) | 738 | 496,265 | 01C61030C6A83B1166226DD66D81947B067E5F4B22FE6EC1A45D9B263F765522 | 1, 2, 3, 4, 7, 8, 9, 10, 11 |
| 40 | docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt | 754 | 17,503 | 2ADE2FC33A640D94E23CFE6B8DFF2671DCE700F50B5A8E83A1343ECA9A2A1A9E | 1, 4 |
| 41 | docs/agenda/route-b/V2_REQUIREMENTS_FIRST_GOVERNANCE_CHANGE_RECORD_28-OCT-2026.txt | 147 | 12,921 | CF584D1D32C52D22CD672A1CF7BE7D0156052A7DCAFA5E2E6C1E6BC23C1F9D03 | 1, 5 |
| 42 | docs/agenda/d-008_trend-market-context/D-008_v2_requirements.md | 248 | 28,525 | A4A1075465EEEFA7D95A63BC427B07239BF82CFFB11097A8DBF3B971B74628F4 | 4, 9 |
| 43 | docs/agenda/d-010_sr-zones/D-010_v2_requirements.md | 343 | 35,336 | B707001F3D4446AB3DAB46FF78A866ACFDABC0EAF55A81D6672CC4D7B7CC0DA0 | 9 |
| 44 | docs/agenda/a-001_system-purpose/A-001_manual.md | 101 | 9,713 | 6FC9D13B36E8243C8BE2600C61654EE19DB011DA0D56C449D17D0B22DB39D05D | 4, 8 |
| 45 | docs/agenda/a-008_strategy-model/A-008_manual.md | 325 | 64,907 | 9588F79202912E224CE0B8D86ADF9B1365AB49ADC8A18E5689CD14A63E142219 | 10 |
| 46 | docs/agenda/a-009_configuration-model/A-009_manual.md | 325 | 71,425 | F99CC290EED514EB33E965374DD9DBD02076EF654D8DA3D799A2C8826105256D | 4, 8, 10 |
| 47 | docs/agenda/b-002_configuration-loading/B-002_manual.md | 339 | 93,568 | 461D8F1154FAC8C202CA4DDFF3CAFC000295AE15C9C7683FB6E087060EE1BF40 | 8 |
| 48 | docs/agenda/b-006_worker-initialization/B-006_manual.md | 255 | 56,839 | 3FA676C753ADA3D5D5AC0FB6930AFA6F6DA9B4FEBD7CFA20804093EF21371C10 | 8 |
| 49 | docs/agenda/c-004_validation/C-004_manual.md | 840 | 173,226 | D39339BDA5854F1AA90A1E33A099A555B605D34CE47CCF8E0B817F8D64352A35 | 8 |
| 50 | docs/agenda/c-008_signal-processing/C-008_manual.md | 727 | 145,246 | 43B2DDBC9DD5B6362CB81B8AE72B2A0E6720CF90100B09975C2EC71CD227592B | 3, 8, 10 |
| 51 | docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md | 1,007 | 117,809 | 5945CFA1D0815608687C36305BEA528307D758C97BF64A5875B8D4406F739F34 | 8 |
| 52 | docs/agenda/d-003_secondary-screening/D-003_manual.md | 799 | 123,490 | 84E09A0F9EE6EE9A06123CAF3A6E9344C37BD19DDB14B6E0F151E2E96C09C7A5 | 8 |
| 53 | docs/agenda/d-004_market-data-requirements/D-004_manual.md | 557 | 82,675 | 23873F60430B115F3C88ADD230C2CD9485C018295874F09C587326551284E18C | 8 |
| 54 | docs/agenda/d-005_candle-analysis/D-005_manual.md | 242 | 29,666 | 037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA | 8 |
| 55 | docs/agenda/a-008_strategy-model/A-008_v2_design_inputs.md | 122 | 22,323 | 3B5E7F3F4D88AC1A0C1B589018BB0E5528E6A0909C1BF1CED9B34BC1C9FCAC96 | 8, 9 |
| 56 | docs/agenda/a-009_configuration-model/A-009_v2_design_inputs.md | 143 | 29,759 | 43F4472853DEF9AFC93B4A47882F407E26AB924F8BEF77B8EBA74F659E786DA6 | 4 |
| 57 | docs/agenda/c-001_chartink-strategy-scanner/C-001_v2_design_inputs.md | 41 | 7,908 | FFD0C465157F7C81FF0F6EAB1589F106F10A642D16BFA0FC034E41E8494A0BAB | 8 |
| 58 | docs/agenda/c-008_signal-processing/C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md | 186 | 55,502 | 9B1878B02869C7700B1F86A3BC41348F9CE93CA4341D7C679AC8C2CF079B4347 | 8 |
| 59 | docs/inputs/reconciliation/D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt | 36 | 4,756 | EE88D2FF0DAD5C62919B0A7AB7B2FAC8D7D3CE205F8D731968D4F4F78DCD10F2 | 8 |
| 60 | docs/inputs/reconciliation/Topic_Workflow.txt | 206 | 11,789 | 6DF2A5FBD9820DA87767636D87580C97CA335CA83D30FAE1D748AA0A2A8A3444 | 10 |

- A source marked as changed by this batch is cited as it is at the base; the batch adds DR-084 and dated notes only (the batch storage manifest, docs/agenda/d006-d010-documentation-batch/).
- The review notes also cite: CLAUDE.md (SHA-256 at the base F35153FDB393FEFC5DC730FBD0D18A9CA52BA6E195A680F93705AABDAF156C56); docs/inputs/reconciliation/TS_D010_Authorise.txt (SHA-256 at the base BAC99AD3AB0FC28EAB0EFEF2D9DDAB5F1FBCCA2E64F9558A3D0827192A87386D).

## Section coverage

| Section | Heading | Lines | Sources used |
|---|---|---|---|
| 1 | Document status, purpose and scope | 3 to 22 | 6 |
| 2 | Records and how to read them | 23 to 72 | 39 |
| 3 | Evidence labels | 73 to 92 | 4 |
| 4 | The question the topic answers | 93 to 112 | 12 |
| 5 | Course of the topic | 113 to 150 | 24 |
| 6 | What is recorded and why nothing is decided | 151 to 223 | 6 |
| 7 | What is not decided: open questions, provisional positions, raw ideas, unknowns | 224 to 290 | 6 |
| 8 | Legacy evidence | 291 to 335 | 17 |
| 9 | Cross-topic boundaries, dependencies and bookmarks | 336 to 384 | 8 |
| 10 | Known defects, process deviations and lessons, as recorded | 385 to 407 | 17 |
| 11 | Status | 408 to 414 | 3 |

## Completeness and checks (by Claude, on the files as committed)

| Check | Result |
|---|---|
| Files of the topic folder at the base named in the manual (section 2) | 14 of 14 |
| docs/inputs/ files at the base whose names carry D-009 or D009 (docs/inputs/legacy_baseline/ excluded by git pathspec; Claude's report), named in the manual or the review notes | 23 of 23 |
| Register rows at the base that name D-009, cited in the manual | 7 of 7 (DR-031, DR-069, DR-076, DR-077, DR-079, DR-081, DR-083) |
| V2_DESIGN_LOG section 7 bookmark rows at the base that name D-009, cited in the manual | 6 of 6 (BK-76, BK-77, BK-78, BK-82, BK-84, BK-85) |
| RQ and OQ IDs of D-009_v2_requirements.md cited in the manual | 36 of 36 (RQ9-01 to RQ9-11 and OQ9-01 to OQ9-25) |
| Quoted fragments of 8 characters or more found word for word (whitespace normalised) in the V2 repository at the base, in the batch's stored inputs or in the reviewers' reports that the batch's independent review record reproduces | manual 286 of 286; review notes 133 of 133 |
| Of these, quoted fragments found only in the text of a stored binary file (docs/inputs/reconciliation/D009_FINAL_REVIEW.doc, read with strings; no copy stored) | 3 |
| Record IDs cited that exist in the V2 repository at the base or are added by this batch (DR-084, DR-085, W-15) | manual 97 of 97; review notes 58 of 58 |
| ASCII only, LF line endings, no trailing whitespace | both files |
| Labels in the manual: UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION | 21; 7; 6 |

## Provenance and limits

- Evidence: V2 records and the frozen manuals (A-001 to D-005) at the base only, with the batch's stored inputs. No legacy repository, clone, host, VM, runtime, broker or database was used (option C, DR-073), and that nothing under docs/inputs/legacy_baseline/ was used for the manual or the review notes is Claude's report, not independently verified (PD-01); how file lists and searches touched that folder is in the last bullet of this section.
- Labels: the manual's section 3 lists them; each statement carries the label of its source, as the drafting brief requires.
- Process record of the drafting and checks (docs/agenda/d006-d010-documentation-batch/D006_D010_BATCH_DECISION_RECORD_31-OCT-2026.txt, section 5): PD-01: one git ls-files pipe over docs/inputs passed docs/inputs/legacy_baseline/ names through memory; that nothing from the folder was displayed or used is Claude's report, not independently verified. Batch-wide, Claude's own completeness scripts and its first runs of this generator listed the base tree with git ls-tree before dropping docs/inputs/legacy_baseline/ (PD-01). Found or recognised after Rama's acceptance, and waiting for his and ChatGPT's decision (OB-05): Claude's routine untracked-file, unstaged-change and rebuild checks ran without the exclusion, and commit-to-commit diffs ran without a pathspec, Claude's until 13:22 IST and the first reviewer's (NOT ESTABLISHED whether git read inside the folder). This manifest was generated with the folder excluded by git pathspec (Claude's report). Rama accepted PD-01 as a disclosed, one-time process deviation with corrective controls (DR-085; W-15); the facts found or recognised after his acceptance wait for his and ChatGPT's decision. Minor: the drafting subagent wrote helper scripts into Claude's scratchpad folder, outside its staging folder.

## Status

- D-009: DRAFT RECORDED - NO REQUIREMENT DECIDED 2026-10-30 (DR-076); OQ9-01 to OQ9-25 OPEN. Unchanged by this batch.
- Nothing in this manifest changes a decision or status. Where it and a record differ, the record governs.
