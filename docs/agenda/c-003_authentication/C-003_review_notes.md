# C-003 LEGACY WEBHOOK AUTHENTICATION AND SECRET VALIDATION: REVIEW NOTES

Date: 2026-10-22 (written at the freeze; revision 0 of the manual was delivered with a drafting report and a delivery manifest instead of review notes)
Status: CLOSED 2026-10-22. The C-003 legacy manual is FROZEN (revision 1, DR-056) together with the four errata of its section 24 (B-007 E-1, A-008 E-6, A-010 E-5 and C-002 E-1), each accepted by Rama on its own; CE-5 needs no change and has no number. ChatGPT's independent review of revision 0 (22-Oct-2026) found it "technically strong", asked that its qualifications A to E be kept and gave "No freeze yet" until its freeze checklist was done under Rama's explicit instruction; revision 1 applies it (section 8). Rama approved the freeze at about 19:30 IST ("Approved and Freeze"; section 6). No V2 decision is made, and at main d89164d no VP or Q row named C-003, so no scheduling move is owed; the notes added to VP-50, VP-55, VP-56 and Q-36 at this freeze are tracking only. Nothing is stored, committed or pushed by Claude: Rama stores and pushes the package of section 11 after his checks (his rule of 21-Oct: "None of the files saved into Project or GitHub push till finalised").

## 1. Sources
Stored byte-identical at the freeze (SHA-256; git blob ID; lines). Names as received (DR-008), without the upload prefixes. Attribution: ChatGPT's three reviews and its reply name ChatGPT in their titles; the research prompt, answer sheet, template and drafting instruction name no author in their text and are recorded as ChatGPT's by the project's established practice (at every earlier topic, for example the C-001 and C-002 rows of V2_DESIGN_LOG section 4); the finalisation instruction is recorded only as the instruction Rama sent, because the re-check of revision 0 found its attribution unverifiable (the drafting report, B8).
- docs/inputs/chatgpt_originals_and_replies/ (8 files):
  - C-003_CLAUDE_RESEARCH_PROMPT_22-OCT-2026.txt (ChatGPT's research prompt): 052A9209BADE0A4D4C484984948B761ED3289F566EA0944DFF211381962A0A15; a83095ed3b143d86de353bfc42dd3d1c4a710994; 88
  - C-003_ANSWER_SHEET_FOR_CLAUDE_22-OCT-2026.txt (ChatGPT's answer sheet, answered by Claude from the production repository; Rama's answers Q1 to Q12 are in manual section 22): B4A84F1AD6EB2E68D29E2C6DEB0C25F86632E9538CE20B9936EA627C757FEB50; 1010ddceef4f1a4b8c0f31a77f4d73b0b655c146; 63
  - C-003_SOURCE_INVENTORY_AND_METHOD_TEMPLATE_22-OCT-2026.txt (ChatGPT's template): 0B7EE7FC656CCCFB1B8D8F3EE4DC0B03CA2427DDFE1C66095EFAFDF248DEA7DB; 5d861d673257434664c70ab6d928fa1fe89a20fe; 52
  - C-003_CLAUDE_INSTRUCTION_TO_DRAFT_MANUAL_REVISION_0_22-OCT-2026.txt (ChatGPT's drafting instruction): A21F67E5228CDAA888722FEDB2EDBD9B978BBA2EFB85F19440FD525145BEAED6; 64c33ea6626c1342fec22e5ebb698c2f34d32935; 83
  - C-003_FINALISE_REVISION_0_FOR_CHATGPT_REVIEW_22-OCT-2026.txt (the finalisation instruction Rama sent at about 18:28 IST; it names no author and gives no time; stored with the instructions it follows): B09B80961DD7C374948824F18BE5BB0C6AB96BEE07A366EDBFD4BD3313AEFFE8; a2e5e27af6604a171dbf6f6982c81ce2a7800b96; 48
  - C-003_CLAUDE_REPOSITORY_ANSWERS_22-OCT-2026.txt (Claude's research file, the bytes the independent final QA confirmed): 86EFED418F12CE1A2C2FE923D3CAD39EA9C595D757FBFFE3AAD72DD5228FB71F; 6a367f19e26cb12add0ec45f06aa46a4b65adcd1; 926
  - C-003_SOURCE_INVENTORY_AND_METHOD_22-OCT-2026.txt (the same): 49788A07169A6458AF12FC4E745BD660B01B6A4A1F3CBB28C289483148667E92; 2f53d44728f1c02cbad203ff3a0029c689573d4b; 540
  - C-003_RESEARCH_NOTES_AND_OPEN_QUESTIONS_22-OCT-2026.txt (the same): 388E32AAD158DA6C85FA9E7624C1AE522962B637D3B0AE68008F7D989A5F2B82; 1199539c537f48ea8ad424d511f8ef9c8024793a; 273
- docs/agenda/c-003_authentication/, ChatGPT's reviews and its reply (4 files):
  - CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_22-OCT-2026.txt (of the research files of 11:47 IST; received at 12:40 IST): 035E8D450D7FF3720AB739C94E8795C6C763994F4181000C71E0F708A7B26741; 84fcb817a0cf289b92b85f8637cdb7d3c3492a9a; 118
  - CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_FINAL_22-OCT-2026.txt (of the final research files and the QA records; received at 14:19 IST): FD9A68D7B449379DEA98C08F871B27BB9606C577979F2F2A8B571E8B328C1475; 0f5eff24db956afb12fc8b4c85c77995b64d4490; 115
  - CHATGPT_VALUE_ADDED_REPLY_TO_CLAUDE_C-003_DECISIONS_22-OCT-2026.txt (on the response file and the erratum decision table): 0FFFBC4FBDE525EA6979FA3A6F390E1A7E30373FC46409F6DE70E9A2D53F86BE; 92d99ac42c194c4223c76d974a8e765996c8225d; 91
  - C-003_CHATGPT_INDEPENDENT_REVIEW_22-OCT-2026.txt (of revision 0's three files of 18:44 IST; received at about 19:25 IST): 8CC0DF5A1CE0049DBF50D65008DADD2854AEA2EEA89ECBEF184B3FC47F43558B; 437bdc29de5bbd306c12751fc292d77044d7f986; 53
- docs/agenda/c-003_authentication/, Claude's review and decision records (7 files):
  - C-003_INDEPENDENT_FINAL_QA_22-OCT-2026.txt (the report of the independent final QA, a separate agent): 22D38FC8E71A066AE2A67B0DFC5E8173FB9411F9537E746A5317FD28023E82DD; 323afcba7435b7f5f7d7cc80274c94f93d171598; 605
  - C-003_PROPOSED_CORRECTIONS_AFTER_FINAL_QA_22-OCT-2026.txt: CA3454D90D62722F553EC2AF30F79DBA22BCF9FC825C52962F3BAF14650926E7; 48734f1fb9beab5313e401049e101e12d6bb25aa; 158
  - C-003_DECISION_REQUIRED_MASKING_METHOD_22-OCT-2026.txt: 391F995CE9715AD94AD1FC93A09DA19EFC00CB9EF75B543B3A99AFFD660C1C8E; 6cf9e8b5bbfe7749a2bd6c9a438939a3ca2b919f; 93
  - C-003_ERRATUM_DECISION_TABLE_22-OCT-2026.txt (its revision of 15:40 IST): 708272D2F6D3D99326458C0ACFCC65648BBCA5C2C279D4DBCCFA727D77BAC120; bda58099a86fde983a8d2edf77e71c535e1c5fd5; 241
  - C-003_CLAUDE_RESPONSE_TO_CHATGPT_REVIEW_22-OCT-2026.txt (its revision of 15:40 IST): 79D44321ECCD55648B1DF92B9E81DC0619FB31C751D57C78DA93983A04F36EC2; 408e0c7d7b284be9b257c9505036abc5e76e9830; 387
  - C-003_REVISION_0_DRAFTING_REPORT_22-OCT-2026.txt (18:44 IST; "the drafting report"): 3B4FDCA6CEF70A75226A57E24A76B9293ED4F93A28F902D3E670E36A9E5D3D57; baeffc27a16a36ec0a528b52993119a9d1b9026c; 323
  - C-003_REVISION_0_DELIVERY_MANIFEST_22-OCT-2026.txt (18:44 IST): A3BCD0F05C25D5C1AF9E229F057F80F78898E1F139A90C349B5BD7A5706E2568; 0d1dd0328cc0b0ca5f51189359813201709ffdee; 60
- README.md (repository root), the copy Rama has held since the research delivery of 22-Oct: B373C454A9371D46F2CF0CDDF948DFEA1AAF8D11A64BD281B54E2BDAB4E1BDC3; 27a34dfb057f61435194145f61d49293e1abd8da; 22. Against main d89164d it changes the layout lines and the visibility line (DR-054).
- Reference hashes (ChatGPT's qualification B, section 8): the corrections note and the masking-method decision note have no reference hash in any delivery note; their values above are those of the one copy in Claude's working folder, the versions Rama approved (the drafting report, B1, note a). The QA report and the response file match the hashes of Claude's delivery notes in the chat only. Every other input of the drafting instruction's list matches its reference (the drafting report, B1); ChatGPT's files and the two instructions are stored as uploaded (byte-identical to the uploads) and have no separate reference hash. From this freeze the stored bytes are the reference.
- Not stored: ChatGPT's withdrawn follow-up review (not used; its SHA-256 is in the drafting report, B1); the reports of the independent check and the re-check of revision 0 (Claude's scratch files; their SHA-256 values are in the drafting report, B7 and B8); revision 0 of the manual (superseded by revision 1; its SHA-256 is in the delivery manifest); screenshots (DR-008); no V2 inputs file and no reconciliation addendum were made (V2's authentication design is not this topic's subject: Rama, Q12 (d)).
- Governance base: V2 main at d89164d9933e6cfdda8cf6a9c3e097afd9d88d42, the head of main on GitHub at the read-only checks of about 14:28, 15:21, 15:48, 17:05, 18:44, 19:38 and 19:54 IST on 22-Oct (ls-remote). Read from a local copy whose base commit equals main; nothing was pushed from it.
- Legacy source: trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb (the partial clone). Not used: the testing snapshot, the records repository, Chartink, the legacy VMs.

## 2. Clone and security checks
- Legacy clone: HEAD at the pin; 8,922 local objects and 0 never-open blobs at every check of the research step, at revision 0's start, at 17:05 and at 18:44 IST, and at the freeze (section 9.3).
- Legacy reads: the research step's (source inventory, sections 6 and 7) and the drafting's 11 reads through the masking filter (the drafting report, B4). At the freeze no legacy file was read: revision 1 changes no legacy statement, citation or quotation (manual section 29), and the freeze's checks used only the pinned tree listing, the research files, the V2 copy and the never-open check, which lists object IDs and opens no content.
- Host records: none read at the freeze, in any way. The research step's exception E-1 and deviations D-1 to D-4, and the drafting's two value-blind operations, are in manual section 25.2. The source inventory's E-1 entry, stored unchanged in docs/inputs, names the record lines where E-1's three items sit, as C-002's inventory named the lines of its own E-1; these are pointers, not values, and no such value is in any package file (section 9.3).
- The A-010 commit pointer that the C-003 files do not reproduce occurs in the package only where it already stood at main d89164d (the A-010 manual and review notes and V2_DESIGN_LOG.md, counts unchanged); it is in no new file and in no line the freeze added.
- No VM, service, host, broker, webhook, endpoint or Chartink operation; GitHub only through read-only ls-remote of the V2 repository.

## 3. Reviews and checks before the freeze: dispositions
- The research files: an independent check of the review copies (28 findings, all applied) and an independent final QA of the final bytes (33 findings, then 5 re-check points N-01 to N-05, all resolved; S-01 left as is by Rama) (source inventory, sections 12.5 and 12.8; the QA report; the corrections note).
- ChatGPT's value-added review (12:40 IST) and its final review (14:19 IST): answered point by point in Claude's response file (sections 0 to 5); the corrections PC-1 to PC-7 that the response proposed were approved by Rama at about 12:50 IST and applied (source inventory, 12.7).
- ChatGPT's decision reply: it recommended the positions Rama accepted on CE-1 to CE-5 (the erratum decision table; the response file, sections 6 and 7).
- Revision 0: an independent check by a separate agent before delivery (17 findings, 2 of them Medium; all applied) and a re-check by the same agent (16 of the 17 fixes confirmed; RC-01 to RC-07 applied after it and verified by Claude's scripted checks, not by the agent) (the drafting report, B7 and B8).
- ChatGPT's independent review of revision 0: section 8.

## 4. The frozen-manual check
- Research step: a separate agent swept the 25 manuals, 8 addenda, the 2 section-end papers and the design log's Q and VP rows at main d89164d (203 statements in 14 of the 36 files); Claude re-read every cited line (source inventory, section 9; manual section 23).
- Revision 0: the anchors of the 8 passages and of the insertion were computed from the frozen manuals at d89164d and verified, and the "checked, not changed" lists come from wording searches (the drafting report, C1).
- At the freeze: the numbers were confirmed against the live register at 19:38 IST (A-008 has E-1 to E-5, A-010 E-1 to E-4, B-007 and C-002 none; the last row DR-055). A search of docs/governance at main d89164d finds no VP or Q row that names C-003, so the 2026-10-21 lesson's list of dispositions is empty.

## 5. The errata
- Four errata to four manuals, ACCEPTED by Rama on 22-Oct-2026, each on its own (DR-056), with exact texts in manual section 24: B-007 E-1 (CE-1), A-008 E-6 (CE-2), A-010 E-5 (CE-3) and C-002 E-1 (CE-4); CE-5 has no change and no number. Their struck, replacement and unchanged texts are those of revision 0; at the freeze only their status wording changed.
- Each final manual starts from its committed file at main d89164d and differs from it only by its erratum's insertions: the status-line sentence; the struck-and-replaced passages (B-007 five, A-008 one, A-010 one, C-002 one) and A-010's three added lines after its line 133; and the erratum section at the end. Each review-notes file differs only by one status sentence. Removing them restores the committed bytes (section 9.3).
- Each erratum section gives the status of its decision ("ACCEPTED WITH CHANGES" for B-007 E-1 and A-010 E-5, "ACCEPTED, as proposed" for A-008 E-6, "ACCEPTED, option (a)" for C-002 E-1) with DR-056, as practised at DR-055.
- Committed blobs at main d89164d, the base of each change: B-007_manual.md 9d479692742450310307bbdda1875099a7f5781a; B-007_review_notes.md 40a2c0f8cab3204372318472d4254b0c8c2c5af5; A-008_manual.md 4c53a1a6fbbd9c37b1ad1cfba583bda3beb52af1; A-008_review_notes.md 2ea63ee2e077288a290e89889db31bcce4499c77; A-010_manual.md 092a711ebbb86bf2029065ea150d2ac2b5700d56; A-010_review_notes.md 988deeda5422db74272e4dbd75127b1dd2651452; C-002_manual.md 1caf4e1050787255bb38616df3e1cc391a2e7b28; C-002_review_notes.md 4312f21605597793028df659b2ea659826401fdc; V2_DECISION_REGISTER.md 8918e92daf03f9282091d8d9a1edf3dee3e90f03; V2_DESIGN_LOG.md 75e72d2285739a182d17f957cae2e1b7d6a9b8f4; README.md 770f2cdd886bffd78f2f7ab048ccb88522815648.
- The errata settle neither U-03 nor C-01 (manual section 24), and the legacy token stays treated as exposed (VP-50).

## 6. Rama's decisions (22-Oct-2026)
- About 12:50 IST (C-003_DECISION_REQUIRED_MASKING_METHOD_22-OCT-2026.txt, section 6): host records read value-blind only from then on; the line-range method not approved for further reading; the lesson row approved as written, for the design log at the C-003 push (written at this freeze, section 7); the records agent's session transcript deleted unopened at 12:52 IST; the corrections PC-1 to PC-7 approved.
- About 13:43 and 14:00 IST: the 33 corrections after the final QA and the re-check points N-01 to N-05.
- 15:14 IST ("I accepted your views") and about 15:35 IST: CE-1 to CE-5, the erratum-number rule and CE-3's source; S-01 left as is.
- During the drafting: an independent check of the draft by a separate agent before it went to ChatGPT.
- About 18:28 IST: he sent the finalisation instruction, whose Authority line records his authorisation.
- About 19:30 IST: "Approved and Freeze".
- Recorded in DR-056 (manual sections 25.3 and 29).

## 7. Design-log and register change set
- V2_DECISION_REGISTER.md: DR-056 appended.
- V2_DESIGN_LOG.md section 1: nine lessons dated 2026-10-22, after the last lesson of 2026-10-21: the row Rama approved (manual 25.4), unchanged; a final statement compared clause by clause with what it summarises (IC-01); a new label's class swept before the check (IC-02); a search's negative stated as its scope (RC-01); a check's result written only after it ran (RC-04); no tool test on a host record without authority, and process exceptions disclosed in the manual too (ChatGPT's qualification C); a file's author or time named only where the file states it; replacement downloads told apart by their hashes; commands that set their folder or use full paths (ChatGPT's section 5).
- Section 2: Q-36, a note on (j) (tracking only).
- Section 3: tracking notes on VP-50, VP-55 and VP-56 (tracking only).
- Section 4: the C-003 row (FROZEN 2026-10-22, DR-056), after the C-002 row.
- Section 6: under 2026-10-22, the C-002 push confirmation (owed from the C-002 freeze), the C-003 first pass, revision 0 and the freeze.
- No other line changed (section 9.3).

## 8. ChatGPT's independent review of revision 0 (22-Oct-2026)
C-003_CHATGPT_INDEPENDENT_REVIEW_22-OCT-2026.txt ("Review only. This is not a freeze approval and authorises no repository changes") hashed the three files of 18:44 IST from their bytes (the manual and the drafting report matching the manifest's values, and the manual the report's; the manifest, which cannot carry its own hash, matching a value ChatGPT had from outside the files, as no delivered file records it), and concluded: "No freeze yet. The draft is technically strong and preserves the required evidence discipline." Dispositions:
- Its section 2, the draft's strengths: no change.
- A, the seven Low points of the re-check: manual section 28 now says that they were verified by Claude's scripted checks and that the agent did not re-check them; DR-056 says the same.
- B, two inputs without a reference hash: kept as a stated limitation (section 1); the stored bytes are the reference from this freeze.
- C, the drafting's two value-blind host-record operations: the review took the manual to carry them, but only the drafting report did (B6 and B8). Revision 1 adds them to manual section 25.2, not simplified; a lesson records the gap (section 7).
- D, the bounded gaps: kept as manual section 26.2 states them.
- E, the qualifications on the serving path, the installed configuration, the runtime, network logging and the refusal of a retired token: kept; revision 1 changes no legacy statement (manual section 29).
- Its section 4, the freeze checklist, done under Rama's instruction of about 19:30 IST: the numbers re-confirmed (19:38 IST); the errata applied exactly as approved, with their anchors (section 9.3); DR-056 and the approved lesson row added; the agenda status row added (V2_DESIGN_LOG section 4, the repository's only status index at main d89164d, so no separate index is updated); the README held for the push included; the SAVE list and the push commands supplied with the package; the repository diff checked (section 9.3).
- Its section 5, the hash command run from another folder: a lesson (section 7); the push commands set their folder first and name each file by its path.

## 9. Checks
### 9.1 Before the freeze
- The research files: section 3; their own checks are in the source inventory, sections 12 and 13.
- Revision 0: the drafting report, B4, B7 and B8 (citations, quoted phrases, scans and the never-open check; the independent check and the re-check).

### 9.2 What the checks do not cover
- The research step's searches T01 to T27 and its legacy reads were not repeated at the freeze; revision 1 changes no legacy statement, citation or quotation, which the line diff of 9.3 shows.
- ChatGPT's reviews are documentary reviews of the files they name, not a reproduction of the research; ChatGPT did not review the freeze edits.
- The two inputs without a reference hash (section 1).
- E-1 is known from the records agent's report only; its session transcript was deleted unopened.
- The edits made after the independent check of the freeze package (9.3), applying its findings, were checked by Claude's scripted runs of 9.3, not by the agent.

### 9.3 Results on the freeze files
Run by Claude on the exact files of section 11 after the last edit, printing counts and pass or fail only; PART 1 of the push commands repeats the hash, scan and change checks on Rama's PC.
- The manual: a line diff of revision 0 (18:44 IST) and revision 1 shows 29 lines changed and 9 added (808 lines), all in the header, sections 2, 23 to 26 and 28 and the new section 29, as section 29 lists. The changed lines carry no legacy citation (the citation checker found none in them, without opening any legacy file), and the evidence list of 26.2 is unchanged (54 files cited, 27 of code and 27 records; 12 records and 2 code files read and not cited). The manual's 296 legacy citations (222 distinct) are those of revision 0: 220 occur in the research files and the other 2 were read while drafting. Section 24's 11 texts equal the decision table's accepted wording, and the anchors of its 8 passages and of the insertion are exact at main d89164d. Of the 23 quoted phrases in the changed lines, each occurs in its source except Rama's "Approved and Freeze" (his message) and the manual's own phrase that section 29 quotes. check_md.py: OK. ASCII only; no carriage return.
- The errata: applying them again to the committed files at main d89164d with revision 1's section 24 reproduces the eight files byte for byte, and removing their insertions (found from section 24's texts and the route's fixed forms only) restores the committed bytes of all eight. Their tables keep their cell counts (check_md.py: OK), and each file keeps its committed count of lines with non-ASCII characters.
- The register and the design log: the generator found each anchor once, a second run from the committed files reproduced both files byte for byte, and removing its additions restores the committed bytes of both; check_md.py: OK. The register runs DR-001 to DR-056 without a gap, and DR-056 has six cells; the agenda table ends with C-001, C-002 and C-003, then the configuration-design row.
- IDs and references: every DR, VP and Q ID named in the C-003 manual, these notes and the lines the freeze added exists; every reference of the form "C-003_manual.md section N" or "C-003_review_notes.md section N" in the 32 files resolves to a heading (each section number so cited).
- Scans (value-blind, on all 32 files): the push checks' credential and session-link patterns match nothing; no carriage return; Claude's new files are ASCII only. Every 40-character hexadecimal token in the new files and in the lines the freeze added is a git ID: a blob of the legacy tree, of main d89164d or of this package; a commit (the baseline or main d89164d); or, once in the source inventory, the root tree of main d89164d (the C-002 commit's); the modified files' other lines are as committed at main d89164d (section 5). Every 64-character token in the 32 files is the SHA-256 of a file in Claude's working folders, except three: the SHA-256 values of the research files' versions of 11:47 IST, named with their line counts in the response file (section 0). The identifier shapes are the hash name SHA256, the scratch folder's name, file-name tokens of the legacy tree and one regular-expression fragment in the source inventory; the user-at-host and bot-name shapes are the pattern's own name ("user@host") and certbot. No address, mailbox, credential assignment, bot token or private key.
- The A-010 commit pointer: section 2.
- Package: against main d89164d, the freeze copy has exactly the 32 paths of section 11 (21 new, 11 modified) and nothing else.
- Legacy clone: HEAD at the pin; 8,922 objects; never-open blobs present locally 0 (20:57 IST).
- V2: main on GitHub still d89164d at 20:58 IST (ls-remote).
- Independent check of the freeze package: a separate agent that wrote none of the files checked the 32 files from about 20:13 to 20:51 IST, verification only (no legacy file and no other file of docs/inputs opened; no network; generators re-run only into its own folder). It found 20 points, none High: 4 Medium (the basis of ChatGPT's hash match for the manifest, which cannot carry its own hash; "no VP or Q row names C-003" without its time; the VP-55 note without L-04's "not established"; DR-056's logging clause without "the receiver") and 16 Low (pointers, tense, the authoritative-passage list of section 24, the checked-passage lines, the review-notes citation form, the Q-36 note's scope, three lessons' dates and wording, two enumerations here, the attribution basis and two decisions missing from DR-056). It confirmed that no legacy fact, number, citation or label changed, that the errata are exact and reversible, and that no new text holds a credential, identifier, session link or the A-010 pointer. All 20 are applied. Its report (Claude's scratch file, not stored; SHA-256 d52d472c47e7d5a4a0565be49b426fb7ed5e104264e14719dbc196effee23d6c) was scanned value-blind before Claude read it. It could not check, by its rules, the legacy-side and network-side facts, the chat-only facts and the final push commands, which did not yet exist.
- Staging rehearsal: before delivery, Claude staged the 32 files in a scratch clone at the base and ran PART 1's checks there (nothing committed or pushed). Its result is reported with the delivery, not here, because these notes are among the staged files.

## 10. Freeze edits
- C-003_manual.md: revision 1, as its section 29 lists. No finding changed.
- C-003_review_notes.md: new (these notes).
- The four manuals B-007, A-008, A-010 and C-002: each its erratum (the in-place edits, one status-line sentence and an erratum section), as manual section 24 gives them; their review notes: one status sentence each.
- V2_DECISION_REGISTER.md: DR-056 appended.
- V2_DESIGN_LOG.md: the change set of section 7.
- README.md: the copy Rama has held since the research delivery, unchanged.
- The nineteen other files: stored unchanged (section 1).

## 11. Closed package manifest
Thirty-two files. The list is closed: nothing outside it is stored. SHA-256 values are fixed here for the nineteen files stored unchanged and for README.md; the other twelve are computed from their final bytes and given with the push commands, together with every file's git blob ID.

| # | Path | Kind | SHA-256 |
|---|---|---|---|
| 1 | docs/agenda/c-003_authentication/C-003_manual.md | New (revision 1) | From the final bytes |
| 2 | docs/agenda/c-003_authentication/C-003_review_notes.md | New | From the final bytes |
| 3 | docs/agenda/c-003_authentication/CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_22-OCT-2026.txt | New, stored unchanged | 035E8D450D7FF3720AB739C94E8795C6C763994F4181000C71E0F708A7B26741 |
| 4 | docs/agenda/c-003_authentication/CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_FINAL_22-OCT-2026.txt | New, stored unchanged | FD9A68D7B449379DEA98C08F871B27BB9606C577979F2F2A8B571E8B328C1475 |
| 5 | docs/agenda/c-003_authentication/CHATGPT_VALUE_ADDED_REPLY_TO_CLAUDE_C-003_DECISIONS_22-OCT-2026.txt | New, stored unchanged | 0FFFBC4FBDE525EA6979FA3A6F390E1A7E30373FC46409F6DE70E9A2D53F86BE |
| 6 | docs/agenda/c-003_authentication/C-003_CHATGPT_INDEPENDENT_REVIEW_22-OCT-2026.txt | New, stored unchanged | 8CC0DF5A1CE0049DBF50D65008DADD2854AEA2EEA89ECBEF184B3FC47F43558B |
| 7 | docs/agenda/c-003_authentication/C-003_INDEPENDENT_FINAL_QA_22-OCT-2026.txt | New, stored unchanged | 22D38FC8E71A066AE2A67B0DFC5E8173FB9411F9537E746A5317FD28023E82DD |
| 8 | docs/agenda/c-003_authentication/C-003_PROPOSED_CORRECTIONS_AFTER_FINAL_QA_22-OCT-2026.txt | New, stored unchanged | CA3454D90D62722F553EC2AF30F79DBA22BCF9FC825C52962F3BAF14650926E7 |
| 9 | docs/agenda/c-003_authentication/C-003_DECISION_REQUIRED_MASKING_METHOD_22-OCT-2026.txt | New, stored unchanged | 391F995CE9715AD94AD1FC93A09DA19EFC00CB9EF75B543B3A99AFFD660C1C8E |
| 10 | docs/agenda/c-003_authentication/C-003_ERRATUM_DECISION_TABLE_22-OCT-2026.txt | New, stored unchanged | 708272D2F6D3D99326458C0ACFCC65648BBCA5C2C279D4DBCCFA727D77BAC120 |
| 11 | docs/agenda/c-003_authentication/C-003_CLAUDE_RESPONSE_TO_CHATGPT_REVIEW_22-OCT-2026.txt | New, stored unchanged | 79D44321ECCD55648B1DF92B9E81DC0619FB31C751D57C78DA93983A04F36EC2 |
| 12 | docs/agenda/c-003_authentication/C-003_REVISION_0_DRAFTING_REPORT_22-OCT-2026.txt | New, stored unchanged | 3B4FDCA6CEF70A75226A57E24A76B9293ED4F93A28F902D3E670E36A9E5D3D57 |
| 13 | docs/agenda/c-003_authentication/C-003_REVISION_0_DELIVERY_MANIFEST_22-OCT-2026.txt | New, stored unchanged | A3BCD0F05C25D5C1AF9E229F057F80F78898E1F139A90C349B5BD7A5706E2568 |
| 14 | docs/inputs/chatgpt_originals_and_replies/C-003_CLAUDE_RESEARCH_PROMPT_22-OCT-2026.txt | New, stored unchanged | 052A9209BADE0A4D4C484984948B761ED3289F566EA0944DFF211381962A0A15 |
| 15 | docs/inputs/chatgpt_originals_and_replies/C-003_ANSWER_SHEET_FOR_CLAUDE_22-OCT-2026.txt | New, stored unchanged | B4A84F1AD6EB2E68D29E2C6DEB0C25F86632E9538CE20B9936EA627C757FEB50 |
| 16 | docs/inputs/chatgpt_originals_and_replies/C-003_SOURCE_INVENTORY_AND_METHOD_TEMPLATE_22-OCT-2026.txt | New, stored unchanged | 0B7EE7FC656CCCFB1B8D8F3EE4DC0B03CA2427DDFE1C66095EFAFDF248DEA7DB |
| 17 | docs/inputs/chatgpt_originals_and_replies/C-003_CLAUDE_INSTRUCTION_TO_DRAFT_MANUAL_REVISION_0_22-OCT-2026.txt | New, stored unchanged | A21F67E5228CDAA888722FEDB2EDBD9B978BBA2EFB85F19440FD525145BEAED6 |
| 18 | docs/inputs/chatgpt_originals_and_replies/C-003_FINALISE_REVISION_0_FOR_CHATGPT_REVIEW_22-OCT-2026.txt | New, stored unchanged | B09B80961DD7C374948824F18BE5BB0C6AB96BEE07A366EDBFD4BD3313AEFFE8 |
| 19 | docs/inputs/chatgpt_originals_and_replies/C-003_CLAUDE_REPOSITORY_ANSWERS_22-OCT-2026.txt | New, stored unchanged | 86EFED418F12CE1A2C2FE923D3CAD39EA9C595D757FBFFE3AAD72DD5228FB71F |
| 20 | docs/inputs/chatgpt_originals_and_replies/C-003_SOURCE_INVENTORY_AND_METHOD_22-OCT-2026.txt | New, stored unchanged | 49788A07169A6458AF12FC4E745BD660B01B6A4A1F3CBB28C289483148667E92 |
| 21 | docs/inputs/chatgpt_originals_and_replies/C-003_RESEARCH_NOTES_AND_OPEN_QUESTIONS_22-OCT-2026.txt | New, stored unchanged | 388E32AAD158DA6C85FA9E7624C1AE522962B637D3B0AE68008F7D989A5F2B82 |
| 22 | docs/agenda/b-007_readiness-health/B-007_manual.md | Modified (erratum E-1) | From the final bytes |
| 23 | docs/agenda/b-007_readiness-health/B-007_review_notes.md | Modified (one status sentence) | From the final bytes |
| 24 | docs/agenda/a-008_strategy-model/A-008_manual.md | Modified (erratum E-6) | From the final bytes |
| 25 | docs/agenda/a-008_strategy-model/A-008_review_notes.md | Modified (one status sentence) | From the final bytes |
| 26 | docs/agenda/a-010_secrets-environment/A-010_manual.md | Modified (erratum E-5) | From the final bytes |
| 27 | docs/agenda/a-010_secrets-environment/A-010_review_notes.md | Modified (one status sentence) | From the final bytes |
| 28 | docs/agenda/c-002_scanner-to-webhook/C-002_manual.md | Modified (erratum E-1) | From the final bytes |
| 29 | docs/agenda/c-002_scanner-to-webhook/C-002_review_notes.md | Modified (one status sentence) | From the final bytes |
| 30 | docs/governance/V2_DECISION_REGISTER.md | Modified (DR-056) | From the final bytes |
| 31 | docs/governance/V2_DESIGN_LOG.md | Modified (section 7's change set) | From the final bytes |
| 32 | README.md | Modified (the copy held since the research delivery) | B373C454A9371D46F2CF0CDDF948DFEA1AAF8D11A64BD281B54E2BDAB4E1BDC3 |

- Base: main at d89164d9933e6cfdda8cf6a9c3e097afd9d88d42. The push commands check that HEAD and origin/main still equal it.
- Claude computed the tree that these files give on that base, in a scratch clone (nothing committed or pushed), and compares it with the pushed commit's tree.
- Next: the push commands (PART 0, 1 and 2) and the SAVE list, delivered with this package (C-003_PUSH_COMMANDS_22-OCT-2026.txt; not stored). Rama runs PART 0 and PART 1 and sends their output; he runs PART 2 only after PART 1 has passed and he declares the package final; Claude then confirms the commit against the remote.
