# D-011 - S&R STRENGTH: MANUAL MANIFEST

Status: the companion manifest of the D-011 topic manual, a V2 topic manual (DR-083), prepared on 01-Nov-2026 as documentation only (Rama, 20:22 IST, and his choice "Docs only, like D-012"; the manual, section 1), outside the repository, and added to it on 02-Nov-2026 by Rama's decision of that day (12:55 IST; docs/agenda/d011-documentation/D011_DOCUMENTATION_STORAGE_MANIFEST_02-NOV-2026.txt). D-011's status is unchanged: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079), at requirement level only. This manifest identifies the manual and its review notes, lists the sources the manual relies on with their SHA-256 at the base commit, maps them to the manual's sections and records the checks run on both files. It decides nothing and changes no status.

## The manual and its review notes

```
docs/agenda/d-011_sr-strength/D-011_manual.md  406 lines  77,839 bytes  E00145B6C315BC82999C32E708F650B346567968C61DA09DC7B5C8F5D48A8873
docs/agenda/d-011_sr-strength/D-011_review_notes.md  165 lines  39,940 bytes  6C829DA33088BD28AF461120E39BBBA40F0BC43A48C1B53BD44DE61BB826F2F9
```

- Both files are new, prepared to sit beside the topic's records; no existing file of the folder is changed. D-011_v2_requirements.md governs every requirement and decision; the manual explains it.
- Base: a4247239ac656661fbe4747430447d2500540320, GitHub main by git ls-remote at 20:23 IST on 01-Nov-2026 (Claude's discrepancy note of that time). Line numbers in the manual are those at the base.
- How they were written: drafted from Claude's drafting brief by a drafting subagent, in a staging folder outside the repository; checked and integrated by Claude; reviewed by an independent Claude reviewer that drafted none of them, and corrected by Claude after that review. The brief, the subagent's prompt, the review report and the list of Claude's corrections are stored with this documentation in docs/agenda/d011-documentation/, whose storage manifest lists them. The review notes' section 7 records this documentation.

## Sources of the manual (as they are at the base)

| # | Source | Lines | Bytes | SHA-256 at the base | Manual sections |
|---|---|---|---|---|---|
| 1 | docs/agenda/d-011_sr-strength/D-011_v2_requirements.md | 186 | 15,960 | AAE0FD3B96ECC146E2BDC66D82AFECD57E2FF4B7CAE5966A788B80C9AD5C912A | 1, 2, 3, 4, 5, 6, 7, 8, 9, 11 (notes: status, topic, 1, 2, 3, 4, 5) |
| 2 | docs/agenda/d-011_sr-strength/D-011_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 281 | 29,348 | 4145B976A45CC3C15DD9B4EC48D092FAC23128F1F985A90F4472EB0A72E914A9 | 1, 2, 3, 4, 5, 6, 7, 8, 10 (notes: 1, 2, 3, 4, 6) |
| 3 | docs/agenda/d-011_sr-strength/D-011_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 53 | 7,149 | A2A7C53F1C9BA9128DB2C3F33B5B98B0150543B16B079A21B7EDEA6F7B82EF56 | 2, 5, 7, 10 (notes: 2, 3, 4) |
| 4 | docs/agenda/d-011_sr-strength/D-011_STORAGE_MANIFEST_30-OCT-2026.txt | 51 | 4,885 | 357729B2843D5455031F9BBE18DDA0BC961193E3A927F1D146DDCCC387789BAF | 2, 5, 10 (notes: 2, 3, 5, 6) |
| 5 | docs/inputs/chatgpt_originals_and_replies/D-011_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 54 | 5,366 | 4038BD0E5CC7121C480DFD212927CAC63F54A506EAD194898BE568DF2FF033F9 | 2, 5, 7, 10 (notes: 1, 2, 3, 6) |
| 6 | docs/inputs/chatgpt_originals_and_replies/D-010_POST_PUSH_GITHUB_VERIFICATION_RESULT_30-OCT-2026.txt | 59 | 6,004 | 5E74C4BA3D9DAE23EC6421D4CD2515BE39C720B0D4603C75F1EF309F25FEAD33 | 2 (notes: 6; SHA-256 and bytes only) |
| 7 | docs/governance/V2_DECISION_REGISTER.md | 98 | 233,558 | F28236A3296B35C799A9B5A7091F2992ADB4A0A218D80E457B2CDEEF64C61EB3 | 1, 2, 4, 6, 9, 10, 11 (notes: status, 1, 2, 5, 6, 7) |
| 8 | docs/governance/V2_DESIGN_LOG.md | 752 | 514,717 | 5D9696723D63AC8ACCF5C6A52DE4C9691481E493087B8F9E9350CA92C3F256BB | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 (notes: status, 1, 2, 3, 4, 5, 6, 7) |
| 9 | docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt | 754 | 17,503 | 2ADE2FC33A640D94E23CFE6B8DFF2671DCE700F50B5A8E83A1343ECA9A2A1A9E | 4 (notes: topic line) |
| 10 | docs/agenda/d-009_support-resistance/D-009_v2_requirements.md | 372 | 35,803 | 2C6F3723C1333710052F0B5EA374C897FE0BD882F9872BACDF72E616ADC7B351 | 4, 7 (OQ9-14 and the boundary line only) |
| 11 | docs/agenda/d-010_sr-zones/D-010_v2_requirements.md | 343 | 35,336 | B707001F3D4446AB3DAB46FF78A866ACFDABC0EAF55A81D6672CC4D7B7CC0DA0 | 4, 7 (P10-03 and OQ10-07 only) |
| 12 | docs/agenda/d-012_sr-freshness/D-012_v2_requirements.md | 187 | 15,319 | 0AE0DA0B24E071364A5A1BA7CD2D75B597A8EE3F559C065162DB882DE10234BE | 5, 7, 9 (notes: 1) |
| 13 | docs/agenda/d-012_sr-freshness/D-012_STAGE0_1_PLAN_EVIDENCE_AND_DECISIONS_30-OCT-2026.txt | 215 | 22,801 | 56056DA7243B1BFFC915AC2AD622228FB25615011D1FAB1C772665F57BB29A52 | 7, 9 (OQ12-05 and OQ12-11 only) |
| 14 | docs/agenda/d-012_sr-freshness/D-012_STORAGE_MANIFEST_30-OCT-2026.txt | 52 | 5,021 | E6AF55DDF288318C393FF63EF6081E29CEBA461EA47FE7AE9D53C0E571AABE70 | 2 (notes: 6) |
| 15 | docs/agenda/d-012_sr-freshness/D-012_manual.md | 376 | 59,710 | 3F76505F8A8D93E62EA35CE76D021F46A1BA11DE9A9545E79D6DCC8F4738076F | format exemplar only |
| 16 | docs/agenda/d-012_sr-freshness/D-012_review_notes.md | 153 | 33,039 | 7BA5257B22BD315015541DD3FF395C83B11AFEC42E523FDA630914884B5074B1 | format exemplar only |
| 17 | docs/agenda/d-010_sr-zones/D-010_manual.md | 420 | 69,783 | 502E92D22095B899C159A2E66214522290AE3C1148B4934AE34DB71D0DCD828D | format exemplar only (headings) |
| 18 | docs/agenda/d-010_sr-zones/D-010_review_notes.md | 273 | 47,303 | D358AFFA17A7BC4757BEE1639BCFB6745465B3B519A778BE06509D33E3A00D25 | format exemplar only (headings) |
| 19 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_REVIEW_BY_CLAUDE_30-OCT-2026.txt | 127 | 11,322 | 4B1E17F1A0D0A6A4ADC856C330DCDDE4D565F815746D128E3CEE5200895AB925 | 2 (notes: 1) |
| 20 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_DECISION_RECORD_30-OCT-2026.txt | 325 | 37,947 | 1857F5FD8984AA1AD2179D9C0B39CD8E86E6FF93AF14C75581A40B050C9A858C | 5 (notes: 1, 2) |
| 21 | docs/agenda/topic-workflow/TOPIC_WORKFLOW_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 103 | 13,888 | 8A29677D1FEBB2B6353D2AE099659A6FA3DEF5F685C38F712A5BE1ECC7CA657F | 10 (notes: 3) |
| 22 | docs/agenda/pre-d011-settlement/D011_PREREQUISITE_RECONCILIATION_REPORT_01-NOV-2026.txt | 198 | 19,117 | A029FEA3D37FB187EB14F02C2A659D28935F28D61BA70AB708F2208830826DBB | 2, 9 (notes: 1, 2) |
| 23 | docs/agenda/pre-d011-settlement/PRE_D011_SETTLEMENT_RECORD_01-NOV-2026.txt | 380 | 35,262 | 9D2A24B74FB25343CFB71487B5F66C9608EB0AD33C327289E09BD5E0375825FF | 9 |
| 24 | docs/inputs/reconciliation/Settle_Before_D011.txt | 54 | 4,428 | F8C635F4568EFBE10CE9D68F97D15D0C38AAB06CE46B21F60AA03A2B989E9D4B | 9 (notes: 1) |
| 25 | docs/inputs/reconciliation/ChatGPT_Review_Before_D011.txt | 88 | 8,174 | 1374544C99F8A10782749359A4686AA5151FD613C1512CA8FF49512B5B2BAFB0 | 1, 9 (notes: 1, 2, 7) |
| 26 | docs/inputs/reconciliation/D011_PrePush_Audit_HOLD.txt | 68 | 5,935 | 9BEBF36F8F0592B199810BAC55BD75991B1332A78134B5F4B23243D2105C6A8A | 9 (notes: 2) |
| 27 | docs/inputs/reconciliation/D006_D010_ChatGPT_PrePush_Audit.txt | 103 | 4,615 | 23FB46C6487C754317D35783106E2821BFE6ECF34A62AF4734C65917B6EA6EEA | 9 (notes: 2; one line, by git grep) |
| 28 | docs/agenda/a-009_configuration-model/A-009_manual.md | 325 | 71,425 | F99CC290EED514EB33E965374DD9DBD02076EF654D8DA3D799A2C8826105256D | 8 (named only, inside a quoted V2_DESIGN_LOG section 8 row; not read) |
| 29 | docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md | 1,007 | 117,809 | 5945CFA1D0815608687C36305BEA528307D758C97BF64A5875B8D4406F739F34 | 8 (named only, inside a quoted V2_DESIGN_LOG section 8 row; not read) |
| 30 | docs/agenda/d-005_candle-analysis/D-005_manual.md | 242 | 29,666 | 037ADF49041E2E78E0191D5F80CBAE0860A4382BFFCB7C1501C12B5224B51ADA | 8 (named only, inside a quoted V2_DESIGN_LOG section 8 row; not read) |
| 31 | docs/agenda/d-010_sr-zones/D-010_INDEPENDENT_REVIEW_RECORD_30-OCT-2026.txt | 73 | 10,051 | CB8D5DD39C7F75CF4963C22ED89F04007DFA97DCA998E5D25F0537F5346A5F29 | 10 (named only, as V2_DESIGN_LOG's lessons name it; not read) |
| 32 | docs/inputs/reconciliation/D-010_STAGE0_RAMA_ANSWER_SHEET_30-OCT-2026.txt | 181 | 14,972 | B358835F1F3603EF829B6B0D420BF6BFF6804F2540256A980198A76F42B46197 | 2 (named only, as D-011's stage record names it; not re-read) |
| 33 | docs/inputs/reconciliation/Topic_Workflow.txt | 206 | 11,789 | 6DF2A5FBD9820DA87767636D87580C97CA335CA83D30FAE1D748AA0A2A8A3444 | 5, 10 (named only, as the topic workflow records name it; not read) |

- The manual and the review notes name four inputs that are not in the repository at the base and that no stored record names, all of 01-Nov-2026 and later than the base: Claude_D011_D012_Continuation_Instructions.txt (ChatGPT's continuation instruction), D011_D012_CONTINUATION_DISCREPANCY_NOTE_01-NOV-2026.txt (Claude's discrepancy note), D011_DOCUMENTATION_RAMA_MESSAGES_TRANSCRIPTION_01-NOV-2026.txt (Claude's transcription of Rama's 20:22 IST words and his later choice) and D011_D012_ChatGPT_Review.txt (ChatGPT's review of this package, whose R2 ruling the manual's section 1 and the notes' section 7 quote, on ChatGPT's instruction of 02-Nov-2026); each is marked "not in the repository at a4247239" where it is named. The manual's section 1 and the review notes' sections 1 and 7 use them only for the authority of this documentation, Rama's words, the later clarification of that authority and the status cross-check; every status is taken from the V2 records at the base. All four are stored with this documentation on 02-Nov-2026 (docs/inputs/reconciliation/ and docs/agenda/d011-documentation/; the storage manifest there lists them).

## Section coverage

| Section | Heading | Lines | Sources used |
|---|---|---|---|
| 1 | Document status, purpose and scope | 3 to 20 | 5 |
| 2 | Records and how to read them | 21 to 82 | 12 |
| 3 | Evidence labels | 83 to 101 | 3 |
| 4 | The question the topic answers | 102 to 114 | 7 |
| 5 | Course of the topic | 115 to 183 | 9 |
| 6 | What is recorded as decided | 184 to 247 | 4 |
| 7 | What is not decided: open questions, unknowns, dependencies | 248 to 304 | 9 |
| 8 | Legacy evidence | 305 to 326 | 6 |
| 9 | Cross-topic boundaries, dependencies and bookmarks | 327 to 382 | 11 |
| 10 | Known defects, process deviations and lessons, as recorded | 383 to 400 | 9 |
| 11 | Status | 401 to 406 | 3 |

## Completeness and checks (by Claude, on the files in the staging folder outside the repository, byte for byte the files added to it)

| Check | Result |
|---|---|
| Files of the topic folder at the base named in the manual (section 2) | 4 of 4 |
| docs/inputs/ files at the base whose names carry D-011 or D011 (docs/inputs/legacy_baseline/ and docs/legacy_system_audit/ excluded by git pathspec; Claude's report; the three settlement files of 01-Nov included), named in the manual or the review notes | 4 of 4 |
| Register rows at the base that name D-011, cited in the manual | 6 of 6 (DR-078, DR-079, DR-080, DR-081, DR-082, DR-084) |
| V2_DESIGN_LOG section 7 bookmark rows at the base that name D-011, cited in the manual | 4 of 4 (BK-80, BK-82, BK-83, BK-85) |
| RQ and OQ IDs of D-011_v2_requirements.md cited in the manual | 14 of 14 (RQ11-01 to RQ11-04 and OQ11-01 to OQ11-10) |
| Quoted fragments of 8 characters or more found word for word (whitespace normalised) in the V2 repository at the base, or in the four inputs not in the repository named in the manual's section 1 (the continuation instruction, the discrepancy note, the transcription of Rama's messages and ChatGPT's review of the package) | manual 494 of 494; review notes 193 of 193 |
| Record IDs cited, of the forms this check covers (DR-, RQ, OQ, BK-, NR, W-, VP-, L-, E-, DV-, Q-M, P, T7-, OP-, R-, F- and G- IDs), that exist in the V2 repository at the base; other forms (such as E11-, L11-, R11-, DV11-, RI-, UK-, K-) are not counted here | manual 90 of 90; review notes 78 of 78 |
| ASCII only, LF line endings, no trailing whitespace | both files |
| Labels in the manual: UNKNOWN / NOT ESTABLISHED; NOT RESEARCHED; OPEN QUESTION | 6; 7; 3 |

## Provenance and limits

- Evidence: V2 records at the base only (frozen-manual content only as D-011's stage and requirements records cite it; no frozen manual was read), with the four inputs named in the manual's section 1 (not in the repository). No legacy repository, clone, host, VM, runtime, broker or database was used (option C, DR-073). The protected folders docs/inputs/legacy_baseline/ and docs/legacy_system_audit/ were excluded by git pathspec, or only named files outside them were read, before any listing, search or read (W-15 and its note of 2026-11-01; DR-087); that nothing from them was listed, read or used is Claude's report, from its check of the drafting subagent's transcript, which is not stored, and of its own commands, not independently verified.
- Labels: the manual's section 3 lists them; each statement carries the label of its source, as the drafting brief requires.
- Process record of the drafting: the drafting subagent read the repository at a4247239 only; its git commands named only files or folders outside docs/inputs/legacy_baseline/ and docs/legacy_system_audit/, or excluded both by pathspec, except one git show --stat of a4247239 with no pathspec, cut to its first five lines (commit metadata and three paths, none in either folder), and pathless commands that printed commit metadata only; it ran no directory walk, whole-tree listing or unscoped search, and git status was not run (Claude's check of the subagent's transcript, which is not stored; not independently verified). It wrote only its three files in the staging folder (D-011_manual.md, sections 1 to 6 of D-011_review_notes.md and D-011_manual_sources.txt, the list of sources this manifest reads); the tool kept three large command outputs in the session's own tool-results store. Claude's edits before the independent review: two "not in the repository" references qualified "at a4247239" in the manual's section 1, the manual's sources sentence pointed to this manifest, six named-only sources (rows 28 to 33 of the table above; not read) added to the drafted sources list, and section 7 of the review notes added (the manual's pre-edit copy was not kept, so the manual's three edits are Claude's account, UNVERIFIED by the review; the sources-list additions and the notes' section 7 were checked by the review against the drafter's copies (its SJ-03 and O-04)). Claude's corrections after the independent review (SJ-01 to SJ-12): the manual's section 1 (the continuation instruction's authority sentences), section 3 (the basis of the 19:35 label), section 4 (the E11-06 label), section 5 (an INTERPRETATION label on the locator heading), sections 6.2 and 9.5; the review notes' sections 2.4, 4 and 7; this manifest's account of how the files were written (line 14), its source mapping, its note on the inputs outside the repository, its evidence line, its ID check row and this line; the generator now writes only outside the repository. Claude's corrections after the independent check of those (SK-01 to SK-03): the provenance of the continuation instruction in the manual's section 1 and the notes' section 7 (no label beyond its stated scope), and this line. Claude's correction after the last check (SL-01), not checked independently: the wording on the 20:22 IST screenshot in those two places, and this line. Claude's corrections after the independent audit (SM-01 to SM-10; O-1 and O-2; O-3 unchanged), as ChatGPT authorised on 02-Nov-2026 after reviewing Claude's proposal: the manual's sections 1 (SM-01, SM-04 with ChatGPT's R2 ruling, SM-05, SM-09), 2.3 (SM-10), 7.1 (SM-02), 9.2 (O-2), 9.5 (SM-03) and 11 (O-1); the review notes' sections 1 (SM-09), 4 (SM-06) and 7 (SM-04, SM-05, SM-07, SM-08, SM-09); this manifest's note on the inputs outside the repository, its quote-check row, its evidence line and this line; the generator's list of those inputs, its check of them and the texts naming them; the sources list (row 3). Claude's corrections after the independent review of those (SN-01 to SN-04; its observations OB-1 and OB-5): the attribution of the sentence after ChatGPT's R2 ruling (manual section 1, notes section 7), DR-083's ranges (manual section 2.3), the two rows of the notes' section 4, and this line. Claude's edits at publication on 02-Nov-2026, after Rama's decision of that day (12:55 IST), not checked independently (a one-time exception that the storage manifest in docs/agenda/d011-documentation/ records): the review notes' section 7 (where its records are stored; its status line, the files kept); this manifest's status line, its account of where the records are, its note on the inputs outside the repository, the heading of its checks, and this line.

## Status

- D-011: V2 REQUIREMENTS RECORDED 2026-10-30 (DR-079), at requirement level only. Unchanged by this documentation.
- Nothing in this manifest changes a decision or status. Where it and a record differ, the record governs.
