# D-012 - S&R FRESHNESS: MANUAL MANIFEST

Status: the companion manifest of the D-012 topic manual, a V2 topic manual (DR-083), added on 01-Nov-2026 as documentation only (ChatGPT's review of 01-Nov-2026, R-03: docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt) in the settlement package before D-011. D-012's status is unchanged: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081), at requirement level only. This manifest identifies the manual and its review notes, lists the sources the manual relies on with their SHA-256 at the base commit, maps them to the manual's sections and records the checks run on both files. It decides nothing and changes no status.

## The manual and its review notes

```
docs/agenda/d-012_sr-freshness/D-012_manual.md  376 lines  59,710 bytes  3F76505F8A8D93E62EA35CE76D021F46A1BA11DE9A9545E79D6DCC8F4738076F
docs/agenda/d-012_sr-freshness/D-012_review_notes.md  153 lines  33,039 bytes  7BA5257B22BD315015541DD3FF395C83B11AFEC42E523FDA630914884B5074B1
```

- Both files are new, beside the topic's records; no existing file of the folder is changed. D-012_v2_requirements.md governs every requirement and decision; the manual explains it.
- Base: 42e653300310c901d01cc99ee48d1fa71b072bd6, GitHub main by git ls-remote at 14:48 IST on 01-Nov-2026 (docs/agenda/pre-d011-settlement/PRE_D011_SETTLEMENT_RECORD_01-NOV-2026.txt, section 1). Line numbers in the manual are those at the base.
- How they were written: drafted from Claude's drafting brief by a drafting subagent, in a staging folder outside the repository (the brief and the subagent's prompt, as sent, are in docs/agenda/pre-d011-settlement/PRE_D011_SETTLEMENT_RECORD_01-NOV-2026.txt, appendix A); checked and integrated by Claude; reviewed by an independent Claude reviewer that drafted none of it, whose brief and report go to ChatGPT with the pre-push packages; Claude proposes storing them with the next package (as for the D-006 to D-010 batch's fourth review). The review notes' section 7 records this documentation and its review.

## Sources of the manual (as they are at the base)

| # | Source | Lines | Bytes | SHA-256 at the base | Manual sections |
|---|---|---|---|---|---|
| 1 | docs/agenda/d-012_sr-freshness/D-012_v2_requirements.md | 187 | 15,319 | 0AE0DA0B24E071364A5A1BA7CD2D75B597A8EE3F559C065162DB882DE10234BE | 1, 2.1, 2.4, 3, 4, 5, 6.1, 7.1 to 7.5, 8, 9.1, 9.2, 9.4, 9.5, 11 (notes: Status, Topic, 1, 2.3, 2.4, 5) |
| 2 | docs/agenda/d-012_sr-freshness/D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 215 | 22,801 | 56056DA7243B1BFFC915AC2AD622228FB25615011D1FAB1C772665F57BB29A52 | 1, 2.1, 2.2, 2.4, 3, 4, 5, 5.1 to 5.4, 6.1, 6.2, 7.1, 7.2, 8, 10 (notes: 1, 2.1, 2.3, 3, 4) |
| 3 | docs/agenda/d-012_sr-freshness/D-012_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 51 | 6,594 | B2C42CB8593F01FD761EB15109B7AA6B251E31321003CF8C01859B66B77452D4 | 2.1, 5, 5.4, 5.5, 7.1, 7.2, 7.3, 10 (notes: 1, 2, 2.2, 2.4, 3) |
| 4 | docs/agenda/d-012_sr-freshness/D-012_STORAGE_MANIFEST_30-OCT-2026.txt | 52 | 5,021 | E6AF55DDF288318C393FF63EF6081E29CEBA461EA47FE7AE9D53C0E571AABE70 | 2.1, 2.2, 10 (notes: 2, 2.3, 3, 5, 6 (and SHA-256 / bytes checked)) |
| 5 | docs/inputs/chatgpt_originals_and_replies/D-012_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 53 | 5,248 | 47E93D3C1ADBF2372D1634109B4B562D9CC4C1BC43B5F3CC13B8D4D15CF3CA89 | 2.2, 2.3, 5, 10 (notes: 1, 6 (and SHA-256 / bytes checked; section 5 push line compared with V2_DESIGN_LOG line 618)) |
| 6 | docs/inputs/chatgpt_originals_and_replies/D-011_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 54 | 5,366 | 4038BD0E5CC7121C480DFD212927CAC63F54A506EAD194898BE568DF2FF033F9 | 2.2 (named only) (notes: 6 (SHA-256 / bytes checked; not read)) |
| 7 | docs/governance/V2_DECISION_REGISTER.md (changed by this package) | 96 | 228,415 | B3C231B7EC4CAFA6CA094FB47F6647352A6FBE1E7B4129B20481665BB4CF4DBE | 1, 2.3, 2.4, 4, 5.6, 6.1, 6.2, 9.1, 9.2, 9.5, 10, 11 (DR-078 to DR-085 read in full; status cells of DR-031, DR-072 to DR-078; row lines of DR-008, DR-031, DR-072 to DR-078; notes: Status, 1, 2.4, 5) |
| 8 | docs/governance/V2_DESIGN_LOG.md (changed by this package) | 743 | 504,152 | 2317B33F813F9118A5105D2368E39DF417B8EE9CA352CB02E90A026978C7CDC1 | 1, 2.1, 2.3, 2.4, 3, 4, 5, 5.6, 6.1, 6.2, 8, 9.1 to 9.4, 10, 11 (lines 186 to 195, 261, 371 to 377, 392 to 396, 611 to 624, 626 to 628, 707 to 744; section headings; notes: 1, 2.4, 3, 4, 5, 6) |
| 9 | docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt | 754 | 17,503 | 2ADE2FC33A640D94E23CFE6B8DFF2671DCE700F50B5A8E83A1343ECA9A2A1A9E | 4 (lines 222, 289, 290 to 319, 432 to 461) |
| 10 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt | 325 | 37,947 | 1857F5FD8984AA1AD2179D9C0B39CD8E86E6FF93AF14C75581A40B050C9A858C | 2.2, 5.6, 10 (sections 1, 2, 2A, 4; grep lines; notes: 1, 2.4, 3) |
| 11 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 103 | 13,888 | 8A29677D1FEBB2B6353D2AE099659A6FA3DEF5F685C38F712A5BE1ECC7CA657F | 10 (row G-02, line 95; grep lines; notes: 3, 6) |
| 12 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt | 127 | 11,322 | 4B1E17F1A0D0A6A4ADC856C330DCDDE4D565F815746D128E3CEE5200895AB925 | 2.1 (lines 58 to 98, R-04 to R-08; notes: 1) |
| 13 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_STORAGE_MANIFEST_30-OCT-2026.txt | 86 | 10,973 | C9712684941F4EE19563675B94FCE8CA2D7254937B1753C02EC81809745C7F0A | 2.2 (row 5 and grep lines only; notes: 6) |
| 14 | docs/inputs/reconciliation/Topic_Workflow_PrePush_Review.txt | 58 | 5,331 | 0670B86995A32C17A6AFD8B21470E97C908C937ACD8B0732E68FC73DA173696C | 2.4, 9.5 (grep lines 15, 16, 30, 31, 44 only; notes: 2.4) |
| 15 | docs/inputs/reconciliation/Topic_Workflow_Final_PrePush_Audit.txt | 86 | 5,756 | B9AE01B2908C63943B5A679A663AB58BF37749281385CEE1E8E31C9D46222F7B | 1 (grep lines 17, 20, 76, 78, 81 only; notes: 2.4) |
| 16 | docs/inputs/reconciliation/D006_D010_ChatGPT_PrePush_Audit.txt | 103 | 4,615 | 23FB46C6487C754317D35783106E2821BFE6ECF34A62AF4734C65917B6EA6EEA | 9.5 (grep line 79 only; notes: 2.4) |
| 17 | docs/agenda/d-011_sr-strength/D-011_v2_requirements.md | 186 | 15,960 | AAE0FD3B96ECC146E2BDC66D82AFECD57E2FF4B7CAE5966A788B80C9AD5C912A | 1, 4 (check of the quoted definition) (grep line 52, RQ11-04's row, only) |
| 18 | docs/agenda/d-011_sr-strength/D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 281 | 29,348 | 4145B976A45CC3C15DD9B4EC48D092FAC23128F1F985A90F4472EB0A72E914A9 | 7.1 (check that OQ11-03 and OQ11-05 exist) (grep lines 175, 213, 236, 259, 261 only) |
| 19 | docs/agenda/d-010_sr-zones/D010_STAGE1_SUPPLEMENTAL_RAMA_INPUT_REGISTER_30-OCT-2026.txt | 264 | 28,809 | A1F5DD7A53C7A678670F1082EBF231744787A2E491045850FD4FB976C0B933AC | 7.2 (check of Rama's quoted words) (grep lines 115, 120, 126, 153 only) |
| 20 | docs/agenda/d-010_sr-zones/D-010_manual.md | 420 | 69,783 | 502E92D22095B899C159A2E66214522290AE3C1148B4934AE34DB71D0DCD828D | format exemplar only (structure, labels table, section pattern) |
| 21 | docs/agenda/d-010_sr-zones/D-010_review_notes.md | 273 | 47,303 | D358AFFA17A7BC4757BEE1639BCFB6745465B3B519A778BE06509D33E3A00D25 | format exemplar only (status and topic lines, section pattern) |
| 22 | docs/agenda/d-005_candle-analysis/D-005_review_notes.md | 108 | 11,525 | FFD3662AA2703D9F592C1593E712400EBED23036E2C539350D278D0A42314431 | format exemplar only (first lines and headings) |
| 23 | docs/agenda/d-005_candle-analysis/D-005_manual.md | 242 | 29,666 | 037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA | format exemplar only (headings) |
| 24 | docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt (stored by this package) | 88 | 8,174 | 1374544C99F8A10782749359A4686AA5151FD613C1512CA8FF49512B5B2BAFB0 | 1, 2.1, 2.2, 3, 5.6, 9.5, 11 (notes: Status, 1, 2.4, 4, 7) |
| 25 | docs/agenda/a-009_configuration-model/A-009_manual.md | 325 | 71,425 | F99CC290EED514EB33E965374DD9DBD02076EF654D8DA3D799A2C8826105256D | 8 (named only, inside a quoted V2_DESIGN_LOG section 8 row; not read) |
| 26 | docs/inputs/reconciliation/Topic_Workflow.txt | 206 | 11,789 | 6DF2A5FBD9820DA87767636D87580C97CA335CA83D30FAE1D748AA0A2A8A3444 | 5 (named only, as V2_DESIGN_LOG line 619 names it; not read) |
| 27 | docs/inputs/reconciliation/D011_PrePush_Audit_HOLD.txt (stored by this package) | 68 | 5,935 | 9BEBF36F8F0592B199810BAC55BD75991B1332A78134B5F4B23243D2105C6A8A | 9.5 (notes: 2.4, 7) |

- A source marked as changed by this package is cited as it is at the base; the package adds notes, rows and lines only (docs/agenda/pre-d011-settlement/PRE_D011_SETTLEMENT_RECORD_01-NOV-2026.txt).
- A source marked as stored by this package is not at the base; its SHA-256 is that of the file as stored, byte-equal to the file received.

## Section coverage

| Section | Heading | Lines | Sources used |
|---|---|---|---|
| 1 | Document status, purpose and scope | 3 to 17 | 7 |
| 2 | Records and how to read them | 18 to 78 | 13 |
| 3 | Evidence labels | 79 to 97 | 4 |
| 4 | The question the topic answers | 98 to 110 | 6 |
| 5 | Course of the topic | 111 to 172 | 9 |
| 6 | What is recorded as decided | 173 to 230 | 4 |
| 7 | What is not decided: open questions, unknowns, dependencies | 231 to 285 | 5 |
| 8 | Legacy evidence | 286 to 306 | 4 |
| 9 | Cross-topic boundaries, dependencies and bookmarks | 307 to 355 | 7 |
| 10 | Known defects, process deviations and lessons, as recorded | 356 to 370 | 8 |
| 11 | Status | 371 to 376 | 4 |

## Completeness and checks (by Claude, on the files as placed in the working copy, not yet committed)

| Check | Result |
|---|---|
| Files of the topic folder at the base named in the manual (section 2) | 4 of 4 |
| docs/inputs/ files at the base whose names carry D-012 or D012 (docs/inputs/legacy_baseline/ and docs/legacy_system_audit/ excluded by git pathspec; Claude's report), named in the manual or the review notes | 1 of 1 |
| Register rows at the base that name D-012, cited in the manual | 7 of 7 (DR-078, DR-079, DR-080, DR-081, DR-082, DR-083, DR-084) |
| V2_DESIGN_LOG section 7 bookmark rows at the base that name D-012, cited in the manual | 4 of 4 (BK-80, BK-83, BK-84, BK-85) |
| RQ and OQ IDs of D-012_v2_requirements.md cited in the manual | 15 of 15 (RQ12-01 to RQ12-04 and OQ12-01 to OQ12-13) |
| Quoted fragments of 8 characters or more found word for word (whitespace normalised) in the V2 repository at the base, or in the inputs stored by this package | manual 365 of 365; review notes 149 of 149 |
| Record IDs cited that exist in the V2 repository at the base or are added by this package (DR-086, DR-087, W-16, BK-86) | manual 74 of 74; review notes 69 of 69 |
| ASCII only, LF line endings, no trailing whitespace | both files |
| Labels in the manual: UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION | 5; 5; 2 |

## Provenance and limits

- Evidence: V2 records and the frozen manuals (A-001 to D-005) at the base only, with the inputs stored by this package. No legacy repository, clone, host, VM, runtime, broker or database was used (option C, DR-073). The protected folders docs/inputs/legacy_baseline/ and docs/legacy_system_audit/ were excluded by git pathspec, or only named files outside them were read, before any listing, search or read (W-15 and its note of 2026-11-01; DR-087); that nothing from them was listed, read or used is Claude's report, from its check of the drafting subagent's transcript, which is not stored, and of its own commands, not independently verified.
- Labels: the manual's section 3 lists them; each statement carries the label of its source, as the drafting brief requires.
- Process record of the drafting: the drafting subagent's git commands named only files or folders outside docs/inputs/legacy_baseline/ and docs/legacy_system_audit/, or excluded both by pathspec, except one git log of commit subjects only, which named no path; it ran no directory walk, whole-tree listing or unscoped search, and git status was not run (Claude's check of the subagent's transcript, which is not stored; not independently verified). Claude's integration edits before the independent review: five sentences updated (the stored path of ChatGPT's review, twice; BK-85's note; the transcript check, twice; Claude's count, as no copy from before these edits was kept: UNVERIFIED by the reviews) and section 7 of the review notes added; after it, Claude applied the review's findings on part B and the delta review's (sentences corrected in the manual and the review notes, and this manifest's source mapping, base line and git-log wording); after ChatGPT's pre-push audit of 01-Nov (HOLD), Claude added its section 1 correction to the manual (9.5) and the review notes (2.4 and 7), and the not-stored qualifier to the transcript checks; after the final review, this manifest's statement on where the reviews are stored and the UNVERIFIED qualifier on Claude's count above. Deviations disclosed: the drafting subagent wrote two helper scripts into Claude's scratchpad folder, outside its staging folder; the part B reviewer ran one git ls-tree of docs/agenda/d-012_sr-freshness/ only, against its brief, which listed the four D-012 records and nothing else (its report).

## Status

- D-012: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-081), at requirement level only. Unchanged by this documentation.
- Nothing in this manifest changes a decision or status. Where it and a record differ, the record governs.
