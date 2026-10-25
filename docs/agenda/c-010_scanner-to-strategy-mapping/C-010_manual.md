# C-010 - SCANNER-TO-STRATEGY MAPPING: LEGACY MANUAL

Status: FROZEN 2026-10-24 (revision 1, DR-063). ChatGPT's final review, C-010_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (38 lines; SHA-256 58E2BC3FBA62F9E6D68D31E4017FBB7515419C3E34F488970FF0A18ABF17EB08; sent by Rama at 21:41 IST), found revision 1 acceptable for the freeze with its package actions A to E (section 18). Revision 1 made the bounded editorial corrections of ChatGPT's C-010_CHATGPT_MANUAL_REVIEW_NOTES_24-OCT-2026.txt (38 lines; SHA-256 F2F38047EAEF903B4D75021B58B260E8A5D64D21F6D33101651BCE70AD720C79; sent by Rama at 21:15 IST) to revision 0. Revision 0 was prepared under ChatGPT's C-010_RECONCILIATION_AND_MANUAL_DRAFT_PROMPT_24-OCT-2026.txt (62 lines; SHA-256 02ED4483B27E7B37806F0980B19242AE65A26E80AA8679FE508FB59568D96368; sent by Rama at 19:43 IST and again, byte-identical, at 19:55 IST), which reconciles Stage 1E and authorises the draft from the existing evidence only. Nothing in the legacy repository was read, searched or counted for the manual. This file is revision 1 (stored unchanged as docs/inputs/manuals/C-010_SCANNER_TO_STRATEGY_MAPPING_REV1_24-OCT-2026.md; SHA-256 2350441E384B9147DC010A129E9B516D97F0E8F515ED7661D7A396C25C61C2E7) with freeze edits only: this status line, the companion-files line below and the last bullet of section 18. BK-27 stays OPEN; VP-45 stays NOT RATIFIED.  
Topic: C-010 Scanner-to-strategy mapping (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 24-Oct-2026 through the gated masking filter. Stage 0 used path names and metadata only. The five evidence stages (1, 1B, 1C, 1D and 1E) made 48 count-only searches (Q01 to Q48, 3 of them positive controls) and 59 gated reads (001 to 059; 751 printed lines). Read 010 is recorded and not relied on (D-1).  
Evidence set: the 24 files listed in section 2.3 (ChatGPT's prompts, authorisations and reconciliations; Claude's answers, proposals and evidence sheets, Stage 0 to 1E). They are to be stored unchanged with this manual at the freeze; the storage list belongs to the freeze package.  
Companion files, frozen with this manual and stored unchanged beside it in docs/agenda/c-010_scanner-to-strategy-mapping/: the five evidence sheets (files 6, 11, 15, 19 and 23 of section 2.3), the Stage 1 searched-files list (file 7), ChatGPT's Stage 1 authorisation review sheet (file 5) and C-010_REGISTER_KEY_MAP.md (the key map proposed in section 3, confirmed by ChatGPT's final review, action A). The other files of section 2.3 are stored unchanged in docs/inputs/ (section 18).  
Purpose: a factual record of how the pinned legacy source ties a Chartink scanner key to a strategy. It covers the map and the strategy registry, the identity the receiver stores in the signals row, the processor's lookup and its rejects, how the resolved strategy name travels on the main entry path to the trades row, how far the evidence goes on the other entry paths, and how later code reads the stored identities. It is not a V2 design or a recommendation to revive, repair or tune the legacy system. Governance context, not evidence: under the project's instructions, no legacy behaviour is carried into V2 by default; this manual makes no V2 decision.  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name other than Rama's is reproduced. Protected paths are counted, never named. The KV-shaped values the filter masked are counted only; they were never examined.

## 1. Purpose and scope

This chapter records, at the pin, the identity chain from the webhook path key to the strategy, and on to the trades row:

- the scanner map and the strategy registry: what keys them, and where the processor's copies come from (section 5);
- the identity the receiver writes into the signals row (section 6; BK-27);
- the processor's strategy lookup, its three rejects and its evidence helper (section 7);
- the main entry path, E1, hop by hop from the map entry and the lookup to trades.strategy (section 8);
- the other entry paths, E2 gate, E3 retest and E4 allocator, as far as the evidence goes (section 9);
- the second `create_trade(` line of orders/order_manager.py (section 10);
- the readers of the stored identities and of the map (section 11).

**Scope basis.** Stage 0 section 5.2, adopted by ChatGPT's Stage 1 authorisation (OQ-02): the identity chain from the webhook path key to the strategy object (map entry, strategy name, loader key, the lookup and its failure), the identity stored in the signals row and how later readers use it, the N:1 and renaming cases the schema allows, and text that describes the mapping. ChatGPT's reconciliations added the identity carried into the trades row (OQ-1B-05; Stages 1C and 1D) and the strategy names of the other entry paths (OQ-1D-01; Stage 1E).

**Out of scope** (cited, not re-analysed): the scanner definitions, the twin URL file and the connectivity checks (C-001); the webhook contract, the 404 for an unknown key and the alert-name check (C-002); authentication (C-003); validation and the rest of the stored signals row (C-004, C-007); the processor's stages other than the lookup (C-008); signal expiry (C-009; Stage 0 found no overlap by name, Stage 0 answers section 5.3); the EOD route (C-011); the strategy model (A-008); the signal state model (K-003); chapters 02 and 03 of the 18-Sep manual (OQ-05: only C-008's existing cross-check row CH-09 is cited, section 17); scripts/t2_cnc_gtt_realtest.py beyond its one printed line (OQ-1C-01); the masked KV-shaped values; and any V2 design (VP-45 stays NOT RATIFIED).

## 2. Evidence and confidence rules

**Method.** Claude proposed each stage; ChatGPT accepted it in writing, with or without changes; it was run; and ChatGPT reconciled it before the next stage. Searches were count-only: per-file counts of a named term over a fixed file list, no line printed, and a positive control for every zero result. Reads went through the gated, masked reader. Each read checked the path against the never-open rules and the pinned listing and the working file against its pinned blob. It then printed a fixed range, or the lines matching a named term (each grep bounded in advance by its count), logged one row and kept its output as evidence. The read lock was lifted only inside the authorised windows. It was engaged again after each window, and a read and a count were then refused.

**Reported execution evidence.** Claude's evidence sheets record the integrity checks, synthetic tests, gate tests and lock refusals. ChatGPT reconciled those reports; each reconciliation states that ChatGPT did not rerun Claude's tools or checks (the Stage 1B reconciliation adds that ChatGPT checked the SHA-256 of that stage's accepted authorisation and proposal against its own copies).

**Line references.** A reference in backticks, `path:lines`, cites lines printed by C-010's own reads at the pin, with the read number; bare line numbers beside it, or in a row that names its read, are lines of the same reads. A line number taken from a frozen manual is marked HISTORICAL with the manual and section, usually written "path lines A to B". Line totals are the masked reader's counts. A line shows source at the pin. It does not show what any deleted host ran or what any signal or trade did.

**Labels** (ChatGPT's fact discipline for this manual):

- **FACT:** directly shown by C-010's bounded source evidence (a printed line, a count) or by a recorded execution artifact. A count is FACT for the count, within its term's limits. It is discovery evidence, not proof of use.
- **HISTORICAL:** stated in a frozen V2 manual at the same pin, cited and not re-verified here unless this manual says so.
- **INTERPRETATION:** a reasoned reading of source lines, not a runtime observation.
- **KNOWN DEFECT:** only where a frozen manual already records it as such (section 13).
- **UNKNOWN / NOT ESTABLISHED:** not demonstrated by the approved evidence.
- **OPEN QUESTION:** an unresolved item retained for the record (section 14).

**Chain rule** (fixed in advance in the Stage 1D accepted authorisation, section 8 and Part 3): what `trades.strategy` holds is FACT only if every hop from the processor's lookup to the 5th bound value is FACT. Otherwise it carries the weakest hop's label, and a missing hop makes it NOT ESTABLISHED. No conditional statement is turned into a finding.

**Masking.** A placeholder such as <CLIENTID> stands for a value the filter hid. A grep's mask counts cover the whole file; a range read's counts cover its range (R-2). This manual quotes no line that carries a placeholder.

**Not used.** No legacy code, test, script or migration was run. No host, VM, broker, Chartink account, service or database was accessed. No protected path was opened, and the two July 2026 queue records stayed excluded. The chapter 02 and 03 search of the 18-Sep manual was not run (OQ-05).

### 2.1 Stages

| Stage | Controlling file | Windows (lock steps) | Searches | Reads (printed lines; cap) | Tests before the windows | Reconciliation |
|---|---|---|---|---|---|---|
| 0 | C010_Stage0_Claude_Prompt_RESHARE.txt; C010_Stage0_Answer_Sheet_RESHARE.txt | none: path names and metadata only | none | none | - | C-010_STAGE1_AUTHORISATION_REVIEW_SHEET_24-OCT-2026.txt: Stage 0 reviewed; OQ-01 to OQ-05 decided |
| 1 | C-010_STAGE1_AUTHORISATION_REVIEW_SHEET_24-OCT-2026.txt, with C-010_STAGE1_CLAUDE_RESEARCH_PROMPT_24-OCT-2026.txt | W1 counts (E-1 to E-4); W2 reads (E-5 to E-8) | Q01 to Q35 (32 searches, 3 controls) | 001 to 017 (176) | synthetic tool tests, all passed | reconciled with follow-up required: bounded findings accepted with limits; F incomplete; D-1 kept; G-1 added to the register |
| 1B | C-010_STAGE1B_CHATGPT_ACCEPTED_AUTHORISATION_24-OCT-2026.txt (D1 to D4) | W3 (E-9 to E-12); W4 (E-13 to E-16) | Q36, Q37 | 018 to 042 (292; 717) | 27 synthetic checks | bounded findings accepted; the 57 consumer lines NOT ESTABLISHED |
| 1C | C-010_STAGE1C_ACCEPTED_AUTHORIZATION_AND_PROMPT_24-OCT-2026.txt (D5 to D8; D9 declined) | W5 (E-17 to E-20); W6 (E-21 to E-24) | Q38 to Q42 | 043 to 050 (76; 191) | 21 synthetic checks (D-2) | accepted with qualifications; D-2, O-6, R-1 and R-2 kept |
| 1D | C-010_STAGE1D_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (D10 to D12) | W7 (E-25 to E-28); W8 (E-29 to E-32) | Q43 to Q48 | 051 to 058 (205; 310) | TC-01 to TC-30, each ok | the E1 finding accepted as INTERPRETATION; O-7 and O-8 left open |
| 1E | C-010_STAGE1E_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt (D13 and D14; D15 declined) | W9 (E-33 to E-36) | none | 059 (2; 2) | TC-31 to TC-40, each ok | reconciled in the manual-draft prompt; R-3 kept |

### 2.2 Stage 1E execution controls and results (reported by Claude; not independently reproduced by ChatGPT)

From C-010_CLAUDE_STAGE1E_EVIDENCE_SHEET_24-OCT-2026.txt, sections 1, 2, 4, 8 and 9:

- **Hashes first.** At 19:20:14 IST, before any check, search or read, the run log recorded the accepted file's SHA-256 (C268255F29A68F3BA82F6666943E70D98EA7251AC83497BC1F63D09EFE2E49B7; 219 lines, 20,512 bytes) and the proposal's (20D0F773CCD2AB3796B52E92FAFBE23B50ACF5306E407279AA04C9A4DDD7B845, the value Part 3 records). The accepted file's first 17,100 bytes equal the proposal byte for byte.
- **Integrity.** The clone's HEAD and the remote's HEAD and main were the pin. The tree listing had 1,438 entries (SHA-256 1F195D3D516AB4F194BCB7AF105FB193A606EE241E2E1437102EF495D8E647A5). orders/order_manager.py was equal to its pinned blob, 978d213532532d7db72b215bc8ed415b848928ae. The object store held 1,257 blobs, 1,537 commits and 6,126 trees before and after; protected blobs present: 0. The reader and the count tool were unchanged since Stage 1.
- **Decision file.** c010/s1e/decisions.json was written as {"D15": false} at 19:22:29 IST, so the approved read list was orders/order_manager.py alone.
- **Synthetic tests.** TC-31 to TC-40 each printed ok by its own ID against the accepted file (12 checks, 0 FAIL), before W9.
- **W9.** E-33 unlock at 19:23:44 IST. E-34 gate test PASSED: the 102 protected paths, the 2 July records, the shared blob, 14 rule-shaped names, a name not in the tree and a malformed range were refused; the count tool refused 4 bad lists; exactly the 1 approved path was admitted. E-35: G17 = read 059, a grep of T23 (\bcreate_trade\() in orders/order_manager.py, printed exactly 2 lines, 12 and 175 (bound 2; line 175 among them). Its masks: CLIENTID 1 and HOSTTOK 0 for the whole file, the same as read 051's, none on a printed line; stage-2 masks: none. E-36 relock at 19:23:53 IST, after which a read and a count were both refused.
- **After W9** (19:24:56 IST): the same integrity results; the read log at 59 rows, the search log at 48 (no new search) and the lock log at 18.
- **Not run** (D15 declined): C12, C13, G18 and G19. Nothing in allocation/portfolio_allocator.py was counted or read.
- **Deviation:** R-3, clerical (section 15).

### 2.3 Evidence set

| # | File | Lines | SHA-256 | From |
|---|---|---|---|---|
| 1 | C010_Stage0_Claude_Prompt_RESHARE.txt | 58 | 95A9B9F94661F60B386B245FD9B013CBCAF38E1DB193FC3ACF79C4088B49678B | ChatGPT |
| 2 | C010_Stage0_Answer_Sheet_RESHARE.txt | 76 | ABAFE72D03C382BDB6D9F9A97EEF38599DE8D5E461DC948D2BFC675665C95529 | ChatGPT |
| 3 | C-010_CLAUDE_STAGE0_ANSWERS_24-OCT-2026.txt | 261 | 364FD378C5877E47E14707727E3BFBA276C07820F5F98B32D95BD021B1F609D7 | Claude |
| 4 | C-010_STAGE1_CLAUDE_RESEARCH_PROMPT_24-OCT-2026.txt | 24 | DCC277DA8F3BB3F04D4CE3718798C75AD2095B75CFA7D8B1323EB8E76BF1D87C | ChatGPT |
| 5 | C-010_STAGE1_AUTHORISATION_REVIEW_SHEET_24-OCT-2026.txt | 74 | 6C2CAD96C1A595F6EC67CDD11CB62B600F8D79637682373034D15BF2B92F5732 | ChatGPT |
| 6 | C-010_CLAUDE_STAGE1_EVIDENCE_SHEET_24-OCT-2026.txt | 1,496 | DDE20A654CAE184D5F9F277592CF1D6C0EDFC63A66C918F12F7C5AA136AE6F80 | Claude |
| 7 | C-010_STAGE1_SEARCHED_FILES_PINNED_BLOBS_24-OCT-2026.txt | 1,269 | D7FBC24C01F615A3DE9CC9AB15CE89B912F2A109C09C953FCD145287DC2D79AB | Claude |
| 8 | C-010_STAGE1_CHATGPT_RECONCILIATION_AND_NEXT_STEP_24-OCT-2026.txt | 101 | 6BADCDF165671C785F54856132F6DE9C5052035251DD46EAFAE95F234621B3B8 | ChatGPT |
| 9 | C-010_STAGE1B_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 193 | EE4A09E7B5B4A7BF4B6BA4A5ACC03E92A5C5DB87A30E669E5DB5F7A66BEC6A3A | Claude |
| 10 | C-010_STAGE1B_CHATGPT_ACCEPTED_AUTHORISATION_24-OCT-2026.txt | 54 | A7A554AB5D30530D2C43512B045370A2AAF57744B6A3E025F4F8042DE0AA39F9 | ChatGPT |
| 11 | C-010_CLAUDE_STAGE1B_EVIDENCE_SHEET_24-OCT-2026.txt | 882 | C8B89D42E6C72B67DADB0E39629B396C64AEE854A2F952327A253E4E2A983F52 | Claude |
| 12 | C-010_STAGE1B_CHATGPT_RECONCILIATION_24-OCT-2026.txt | 79 | 33C3432D952A3981C285055BDBF70833321C3E401F504B52C894C3E57061F1BB | ChatGPT |
| 13 | C-010_STAGE1C_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 155 | 60E4045BE3112CB6FCCB4F8A9011F9F7450474B96345219817BA89F5D504FEFE | Claude |
| 14 | C-010_STAGE1C_ACCEPTED_AUTHORIZATION_AND_PROMPT_24-OCT-2026.txt | 190 | 3B0E95C1EA368A690755CC903966B13F446F428331B9FFCFF8C52208CB1FF753 | ChatGPT |
| 15 | C-010_CLAUDE_STAGE1C_EVIDENCE_SHEET_24-OCT-2026.txt | 566 | 5BE8CE7FED0564EB2561F338700833B17AA03325E98DBD87AA409B7015A98BC1 | Claude |
| 16 | C-010_STAGE1C_CHATGPT_RECONCILIATION_24-OCT-2026.txt | 84 | A6D698ECA83FBF8DC4953B561919DAAFE2AA4BA0255202898CF6D55C34A8576D | ChatGPT |
| 17 | C-010_STAGE1D_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 267 | 82C9E265D37A97C3D48C7664B372C4712BE77C2ADB6A330D123456AED32EF46E | Claude |
| 18 | C-010_STAGE1D_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 303 | 3F4ED2D24DA9C8307A7BDA8915336AEA91E018DD3837C20A139D75F7DFF58895 | ChatGPT |
| 19 | C-010_CLAUDE_STAGE1D_EVIDENCE_SHEET_24-OCT-2026.txt | 681 | A5106FA2BF4CB68024D76F239DB0E44BCAA546A2B87F7B8233BDAA92FA57DA71 | Claude |
| 20 | C-010_STAGE1D_RECONCILIATION_AND_NEXT_STEP_24-OCT-2026.txt | 52 | 89FD59673CB5D53DE135BD0B50079A7B33555DBBE226B089BD718D3C6C0C4132 | ChatGPT |
| 21 | C-010_STAGE1E_PROPOSED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 188 | 20D0F773CCD2AB3796B52E92FAFBE23B50ACF5306E407279AA04C9A4DDD7B845 | Claude |
| 22 | C-010_STAGE1E_ACCEPTED_AUTHORISATION_AND_PROMPT_24-OCT-2026.txt | 219 | C268255F29A68F3BA82F6666943E70D98EA7251AC83497BC1F63D09EFE2E49B7 | ChatGPT |
| 23 | C-010_CLAUDE_STAGE1E_EVIDENCE_SHEET_24-OCT-2026.txt | 361 | 162E54B20C955FA9D2CD2D7C306641D2124F59699B4CA20A763B849329CA69DE | Claude |
| 24 | C-010_RECONCILIATION_AND_MANUAL_DRAFT_PROMPT_24-OCT-2026.txt | 62 | 02ED4483B27E7B37806F0980B19242AE65A26E80AA8679FE508FB59568D96368 | ChatGPT |

The verbatim masked outputs of reads 001 to 059 are in the evidence sheets (files 6, 11, 15, 19 and 23); the per-file counts, with each searched file's pinned blob, are in files 6 and 7. The screenshots that came with some of these messages are not part of the evidence set.

## 3. Source register

Reproduced as recorded in the Stage 1E evidence sheet, section 3: Stage 1D's rows, with what each stage added marked "1C:", "1D:" or "1E:". The blobs are the pinned listing's. Section numbers inside the rows (such as 6.1 and 6.2) are that sheet's own; "the Stage 1 companion file" is file 7 of the evidence set (section 2.3). R35 to R37 were added in Stage 1E and were not read.

**Register issues retained** (flagged for clerical reconciliation at the freeze; nothing is renumbered or rewritten here; Stage 1D reconciliation, item 4; manual-draft prompt, item 7):

- **Unresolved clerical collisions.** The IDs R13, R14, R17, R19, R20, R23 and R24 each cover more than one file: 19 rows under 7 IDs. They are kept exactly as recorded and stay UNRESOLVED in this draft.
- **Freeze-time reconciliation action** (proposed for ChatGPT to confirm; not applied here): store the register below unchanged as the historical record, and store beside it a key map that gives each colliding row a distinct key made from its recorded ID and its place in the recorded order (table after this list). No recorded row, ID or text changes; a later reference cites the distinct key together with the recorded ID. The three test-file rows share the marker T as recorded and can be keyed the same way (T.1 to T.3).
- **Role texts, qualified and kept as recorded.** Three recorded role texts are narrower than the evidence now held: R15 reads "dashboard queries on signals", while reads 038 to 040 also show its queries on trades (section 11.3); R28 reads "two comment lines", while the Stage 1B ledger records audit.py's two lines as a docstring line and a comment (section 11.5); R33 places the create_trade call at 996 in OrderPlacer.place, a membership that is INTERPRETATION (section 8.1, H9). The key map can carry these qualifications as notes; the recorded texts stay unchanged.

| Recorded ID | Rows, in recorded order | Proposed distinct key at the freeze |
|---|---|---|
| R13 | config/strategy_direction_registry.yaml; core/strategy_direction.py | R13.1; R13.2 |
| R14 | scripts/replay_signals.py; scripts/strategy_registry_officer.py | R14.1; R14.2 |
| R17 | ops_dashboard/backend/services/strategy_meta.py; ops_dashboard/backend/services/strategy_tower.py; ops_dashboard/backend/services/strategy_ranking.py; ops_dashboard/backend/services/strategy_health.py; ops_dashboard/backend/services/strategy_score.py; ops_dashboard/backend/api/strategies.py | R17.1 to R17.6, in that order |
| R19 | scripts/strategy_status.py; scripts/compute_strategy_metrics.py | R19.1; R19.2 |
| R20 | strategies/__init__.py; strategies/taxonomy.py | R20.1; R20.2 |
| R23 | scripts/preflight_scanner_check.py; utils/startup_checks.py | R23.1; R23.2 |
| R24 | core/account_registry.py; orders/eod_squareoff.py; signals/entry_throttle.py | R24.1 to R24.3, in that order |

The register as recorded:

| ID | Path | Pinned blob | Role in the C-010 chain | Inclusion reason | Evidence held |
|---|---|---|---|---|---|
| R01 | signals/signal_processor.py | 7fab0a02a302770ac72bbbd662549d5c091086c1 | map copy, Step 2 lookup, the evidence helper; 1C: the place(...) call; 1D: strategy_name from Step 2 through _process_one and _admit_and_place; 1E: the E2, E3 and E4 lines recorded in 6.2 | named; C-001 7.4, C-008 ST-08; 1C: C-008 ST-29; 1D: C-008 4, 18; 1E: C-008 2.2, 9 | reads 001, 002, 031; 1C: read 043; 1D: reads 057, 058 (Q45, Q46); 1E: no new read |
| R02 | signals/webhook_receiver.py | 0f15a0ab033529d80d9c14c993a5077690b72871 | the INSERT of signals.scanner and signals.strategy | named; C-001 7.2 (BK-27) | read 004; counts |
| R03 | main.py | 445fb0d8237a6d535ed00e5eb968c8176efd74f3 | where the map is passed | C-008 W-02 | read 003 |
| R04 | strategies/loader.py | 99e83b192021297a3df472840728b88123c7b891 | strategy registration key | A-008, B-002 M22 | read 005 |
| R05 | config/scan_webhook_map.yaml | 54dd98d75f276f9b82027707702df2ee619b11f7 | the map entries | C-001 4.2, 4.3 | counts only (HISTORICAL for content) |
| R06 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | the map schema | C-001 5.2 | counts only |
| R07 | config/strategies/*.yaml (16 files) | see the Stage 1 companion file | each strategy's name | C-001 4.4, B-002 M22 | counts only (blobs in the Stage 1 companion file) |
| R08 | strategies/schema.py | 22530161a5ee5a24bd85a7701b45269b3f2bacdb | the strategy model | A-008 | counts only |
| R09 | config/system_config.yaml | bf742ae5c81c60614370361998e6a102a1064ff7 | settings near the map | C-001 4.4 (D-04) | counts only |
| R10 | core/schema.sql | b46d7489f6ec5e31149dd68957f3b9e45270571e | the signals columns | C-007 2.1 | counts only |
| R11 | core/state_store.py | 11866117801b9329773a1be605448faef3c96f8d | later writers of signals; 1C: in rule R's writer list | C-001 7.2; 1C: C-007, C-008 | counts only (T10 4); 1C: no T14 line (Q38); 1D: no create_trade( line (Q47) |
| R12 | config/chartink_scanners.yaml | de82a8269c2c913e027dad5bd3a92c59222d8eaa | the twin URL file | C-001 6 | counts only |
| R13 | config/strategy_direction_registry.yaml | b03f4ed0c90043840f339fc9b5dac6cca93bf736 | a second name-keyed list | C-001 4.3 | counts only |
| R13 | core/strategy_direction.py | 0e6f8b6686f393dcf38ddf036e565190cec0863a | the same | C-001 4.3 | counts only |
| R14 | scripts/replay_signals.py | 72763c3ed8d5d8fe43258619d55840f2780cfa96 | reader of the stored identity | C-001 7.2 | reads 007, 027, 033; 1C: reads 049, 050 (Q41, Q42) |
| R14 | scripts/strategy_registry_officer.py | c013518bc49c2323922292af5fa9eeacb3cc4d0b | reader of the stored identity | C-001 7.2 | reads 008, 032 |
| R15 | ops_dashboard/backend/readers/db_reader.py | f59e4f58fe51e32d151deb5e14322c1bb2cc28b5 | dashboard queries on signals | C-001 7.2, 8 | reads 009, 028, 034 to 042 (read 010 not relied on) |
| R16 | ops_dashboard/backend/services/scanner_attribution.py | 8503d3c1c6e038a7309d52e9d5492e8332ed3624 | screen 21 | C-001 8 | reads 011, 022, 029; 1C: read 047 (Q39) |
| R17 | ops_dashboard/backend/services/strategy_meta.py | 8d7324317a3a44ec610647daf9dced6079a64d77 | per-strategy view | path name | counts only (no T6/T7) |
| R17 | ops_dashboard/backend/services/strategy_tower.py | 6effe8732f5284d37a318d9fcc9ddedd9e288070 | the tower build | C-001 8 | reads 012, 023 |
| R17 | ops_dashboard/backend/services/strategy_ranking.py | 61f44251c094be199a9f6cb7dd26863aa4c461f7 | per-strategy ranking | path name | read 013 |
| R17 | ops_dashboard/backend/services/strategy_health.py | 8a7264e5db2a2aacade9a4bbabe1ef99e5a5ebe6 | per-strategy health | path name | read 014 |
| R17 | ops_dashboard/backend/services/strategy_score.py | 79c55db1987d956e2943e5710deadfa68b43b2e1 | per-strategy score | path name | counts only (no T6/T7) |
| R17 | ops_dashboard/backend/api/strategies.py | 8fac396665d64d425a28944eec27744247fc5c6c | strategy API | path name | read 015 |
| R18 | capital/strategy_governor.py | d17193c569c1d2e2890d2047b03cf9d0ae825e8d | per-strategy rule key | C-008 | read 016 |
| R19 | scripts/strategy_status.py | d15942ccde89d30e45df310a643ba0dacec17bce | per-strategy report | path name | counts only (no T6/T7) |
| R19 | scripts/compute_strategy_metrics.py | c081a0a8f76804301ada8d07733cf583f1c99287 | per-strategy metrics | path name | reads 017, 030 |
| R20 | strategies/__init__.py | ee3a2879f0fa022d22fdd62d9aed206964b3e64b | package | path name | counts only (no T6/T7) |
| R20 | strategies/taxonomy.py | 069644bc1bdb464ffe5faec31f710acdc02e4837 | taxonomy | path name | counts only (no T6/T7) |
| R21 | strategies/control.py | b6eecb480b83471149fb9362bafc980c967ff6c9 | the step after the lookup | C-008 ST-09 | none (C-008's) |
| R22 | v3_chain/watchlist_capture.py | 25a1f084eadabc443a0dd22d8171b26584d4e897 | the EOD route's stored key | C-001 T-10 | none (C-011's) |
| R23 | scripts/preflight_scanner_check.py | 3151263d6e247b04884e9fbe99f3cd0e44725167 | connectivity checks | C-001 6 | none (C-001's) |
| R23 | utils/startup_checks.py | 61ff67676d8230640a2aafb6bd6d75d7a185fbb6 | the same | C-001 6 | none (C-001's) |
| R24 | core/account_registry.py | d2bdc836cc9a04a99d4d179dba4f46e78213c84c | alert-name prefix | C-001 4.3 | none (other topic) |
| R24 | orders/eod_squareoff.py | 54f6cd6223353df0e9ad8bc823472f28b19b3818 | EOD direction map | C-001 7.4 | none (other topic) |
| R24 | signals/entry_throttle.py | b887a4be78a4dcfe3c2541d7528bd3b96ec351ea | throttle key | C-001 7.4 | none (other topic) |
| T | tests/unit/test_signal_processor.py | 3ec0ea84392a193d18b992ca88df0da5e6235f67 | the key-differs test | C-001 7.2 | read 006 |
| T | tests/unit/test_webhook_receiver.py | 7b377abb85d644f0f3ff7956385224b43f31ed61 | a plain-dict test map | C-001 11 | counts only |
| T | tests/unit/test_config_loader.py | 8f245c35a047794c00ffe21e53c178b5928af890 | the map model | C-001 5.2 | counts only |
| R25 | ops_dashboard/backend/services/analytics_period.py | 594236faaef703a556f9e8f902fcaad18cf426c6 | older per-scanner attribution | C-001 8 (G-1) | reads 018, 024; no counted SQL (C1 0, C2 0) |
| R26 | ops_dashboard/backend/api/analytics2.py | 8ab626ef7de17a79a057c494fa3ca1aaf05193ad | attribution endpoints | C-001 8, 12 (G-1) | read 019; no counted SQL |
| R27 | ops_dashboard/backend/readers/config_reader.py | 624bc8009771cb210ba3ac5eec9b70dc57bde537 | the dashboard's own map load; 1C: its function list | C-001 5.1 (G-1) | reads 020, 021; 1C: read 048 (Q40) |
| R28 | ops_dashboard/backend/services/audit.py | f772aa60f53ad4a40407918f5b9b85ef3b146f59 | none in the chain (two comment lines) | T1 and T2 lines (G-1) | read 025 |
| R29 | ops_dashboard/backend/services/config_view.py | 0d4929a1c9908a18af771efe33f55d5a0ad04d9c | displays the map | C-004 (G-1) | read 026 |
| R30 | scripts/preflight/checks/config_integrity.py | bff5d236a75441043ec98969170f91a830d2f9a3 | pre-flight file presence | C-001 5.3, 6.5 (G-1) | none: HISTORICAL suffices (D3) |
| R31 | scripts/premarket_healthcheck.py | c81fc21e57497e5b4b33bbf5d1a8270de44988e2 | deprecated presence check | C-001 6.5 (G-1) | none: HISTORICAL suffices (D3) |
| R32 | orders/order_manager.py | 978d213532532d7db72b215bc8ed415b848928ae | 1C: the INSERT INTO trades (strategy column); 1D: inside OrderManager.create_trade (175), which binds its strategy parameter 5th; 1E: its other create_trade( line, 12, is a string line before the first definition, most likely the module docstring, so the file holds no create_trade( call line (INTERPRETATION, 6.1) | rule R's writer list (C-007, C-008); C-007 1.1 lists the same blob (770 lines); 1E: D13 | 1C: reads 044, 046 (Q38); 1D: reads 051, 052, 053 (Q43; Q47, Q48); 1E: read 059 (G17) |
| R33 | orders/order_placer.py | e3a602909befa3fd3ba4b27caeed108724c07947 | 1C: OrderPlacer.place, the callee of the processor's place(...) (HISTORICAL); 1D: its create_trade call (996) with strategy=strategy (1000); 1E: line 996 the only create_trade( call line in s_rt (INTERPRETATION, 6.1) | rule R's writer list; C-008 ST-29, 17 | 1C: counts only (Q38); 1D: reads 054, 055, 056 (Q44; Q47); 1E: no new read |
| R34 | scripts/t2_cnc_gtt_realtest.py | 4034ecd18b2d6c5ccf84419316db964a533eb7f1 | 1C: a lead: a second "INSERT INTO trades" string; no part in the chain shown | G6 by its rule (an s_rt file with a C3 count of 1); C-007 1.1, 4.1 | 1C: read 045 (1 line; rule R: a lead); 1D, 1E: not inspected |
| R35 | screening/entry_gate.py | 46b173171bdfeba48af641b67a361618e5acba58 | 1E: where E2's WatchEntry comes from: the gate_state rows reloaded at start and released (HISTORICAL) | C-008 2.2 (E2), EP-02 (KNOWN DEFECT); D14 | none: not read (D14); UNKNOWN / NOT ESTABLISHED |
| R36 | screening/retest_monitor.py | 26639d6ed5dddb1140f0868bb9f937cad5379f82 | 1E: where E3's parked comes from: the divert's parking write and the confirm hand-off (HISTORICAL) | C-008 2.2 (E3), ST-15, EP-05; D14 | none: not read (D14); NOT ESTABLISHED |
| R37 | allocation/portfolio_allocator.py | cb27a82e757f2d8aba4400ec0949611b0c340e2d | 1E: E4's allocator hand-off between the candidate the processor builds and the one admit_prepared receives (HISTORICAL) | C-008 2.2 (E4), ST-19, EP-07; D15 declined | none: not counted or read (D15 declined); NOT ESTABLISHED |

## 4. Identities, terms and the end-to-end view

### 4.1 Terms

| Term | Meaning here | Label and evidence |
|---|---|---|
| Scanner key | the key of an entry under "scanners" in config/scan_webhook_map.yaml; in the receiver's route, the path segment of POST /webhook/<scanner_name> | HISTORICAL (C-001 4.3) |
| scanner_name | the variable named in the receiver's INSERT and in the processor's lookup | FACT for the name in both lines (`signals/webhook_receiver.py:975`, read 004; `signals/signal_processor.py:933`, read 001); HISTORICAL that it carries the webhook path key (C-001 4.3: the route POST /webhook/<scanner_name>; C-001 12, T-8: a signals row with scanner = strategy = the key) and that the processor's value comes from the receiver's queue tuple (C-001 12, T-8 and T-9) |
| Map entry | the value under a key: in the loaded configuration a schema object with strategy, chartink_url and scanner_type; in the processor test, a plain dict | HISTORICAL for the schema (C-001 5.2); FACT for the test's dict (`tests/unit/test_signal_processor.py:1936`, read 006) |
| Strategy name (strategy_name) | the entry's strategy value, resolved by Step 2 | FACT (`signals/signal_processor.py:939-942`, read 001) |
| Strategy object (strategy_obj) | a loaded strategy, registered under its name, as Step 2 takes it from the processor's strategies | FACT for the lines (`strategies/loader.py:149`, read 005: strategies[cfg.name] = cfg; `signals/signal_processor.py:948`, read 001: strategy_obj = self._strategies.get(strategy_name)); HISTORICAL that self._strategies is the processor's own copy of the loaded strategies, made at construction (C-008 2.1, W-02, W-04) |
| signals.scanner, signals.strategy | two columns of the signals row written by the receiver | FACT (`signals/webhook_receiver.py:967-968`, read 004) |
| trades.strategy | the 5th column of the trades row written by OrderManager.create_trade | FACT (`orders/order_manager.py:215`, read 046) |
| E1 to E4 | the processor's entry points: E1 the dispatcher's _process_one; E2 continue_from_gate(entry, release_ltp); E3 continue_from_retest(parked); E4 admit_prepared(candidate) | HISTORICAL (C-008 2.2) |

### 4.2 Identity cases

- **At the pin, every key equals its strategy** (HISTORICAL, C-001 4.3; C-010 did not read the map file, Stage 1 U-10). Then signals.scanner, signals.strategy and the resolved strategy name are the same string, and stored data cannot show which identity a step used; only source lines can (Stage 0 section 5.4; INTERPRETATION).
- **A key different from its strategy** (a renamed mapping). The schema allows it (HISTORICAL, C-001 4.3), and the processor test's fixture uses one, gap_go_short to gap_go_short_v1 (FACT, read 006). In that case signals.strategy is supplied with the key (section 6) while E1's trades.strategy is supplied with the strategy name (section 8): INTERPRETATION.
- **Several keys on one strategy (N:1).** The schema allows it; there is none at the pin (HISTORICAL, C-001 4.3). Each signals row would hold its own key, and E1's trades rows would share the strategy name: INTERPRETATION.
- **A mapped strategy that is not loaded.** Step 2's third check rejects it (FACT, `signals/signal_processor.py:948-953`, read 001). For one such case, a map strategy with no YAML, C-001 5.2 records that the start succeeds and each of its signals is later rejected as REJECTED_UNKNOWN_STRATEGY (HISTORICAL). Whether a mapped strategy can be missing from the loaded set for any other reason is NOT ESTABLISHED.
- **A key that is not in the map.** The receiver answers 404 before any symbol is accepted, so no signals row is written; its request-audit row records (404, 0, 0) (HISTORICAL, C-001 4.3; C-002 7.1, 7.2 and 9, row 6); the processor's first check would reject it (FACT, `signals/signal_processor.py:933-938`, read 001).

### 4.3 End-to-end view (bounded by the evidence)

```text
config/scan_webhook_map.yaml: "scanners", 16 entries, key = strategy value at the pin   (HISTORICAL C-001 4.2 to 4.4)
config/strategies/*.yaml: one strategy per file, keyed by its name field                (HISTORICAL A-008 4)
  loaded once per start (load_all)                                                      (HISTORICAL C-001 5.1)
    main.py:2458  scan_webhook_map=app_config.scan_webhook_map            (read 003; callee HISTORICAL C-001 5.1)
    main.py:3585  scan_webhook_map=app_config.scan_webhook_map.scanners   (read 003; the processor, HISTORICAL C-008 W-02)
    signal_processor.py:208  self._scan_webhook_map = dict(scan_webhook_map)   (read 002; the only assignment in the file)
    loader.py:149            strategies[cfg.name] = cfg                         (read 005)

POST /webhook/<scanner_name>: the key must be in the map, else 404                (HISTORICAL C-001 4.3; C-002)
  scanner_name = the path key                                                     (HISTORICAL C-001 4.3; C-001 12, T-8)
  receiver, per stored symbol: INSERT INTO signals (scanner, strategy) = (scanner_name, scanner_name), QUEUED
                                                         (webhook_receiver.py:963-980, read 004)   BK-27: OPEN
  queue tuple to the processor                                                    (HISTORICAL C-001 12, T-8, T-9)

E1  SignalProcessor._process_one, Step 2                         (signal_processor.py:930-953, read 001)
      map_entry = self._scan_webhook_map.get(scanner_name)        missing -> UNKNOWN_STRATEGY
      strategy_name = the entry's "strategy" (dict key or attribute)  None -> UNKNOWN_STRATEGY
      strategy_obj = self._strategies.get(strategy_name)          missing -> UNKNOWN_STRATEGY
      (stored as REJECTED_UNKNOWN_STRATEGY: HISTORICAL C-008 ST-08)
  -> self._admit_and_place(..., strategy_name=strategy_name, ...)               (1182, 1184; reads 057, 058)
  -> self._placer.place(..., strategy=strategy_name, ...)                       (1446-1460; read 043)
  -> OrderPlacer.place(strategy) -> self._om.create_trade(strategy=strategy)    (order_placer.py:882, 996-1015; reads 054 to 056)
  -> OrderManager.create_trade(*, strategy): INSERT INTO trades, 5th column = 5th value = strategy
                                                         (order_manager.py:175-271; reads 052, 046, 053)
  => trades.strategy is supplied with the resolved strategy name, not the scanner key   INTERPRETATION (section 8)

E2  gate:      strategy_name = entry.strategy_name (2051) -> place(strategy=strategy_name) (2260)    UNKNOWN / NOT ESTABLISHED
E3  retest:    strategy_name = parked.strategy (2387)     -> place(strategy=strategy_name) (2557)    NOT ESTABLISHED beyond the processor
E4  allocator: candidate built with strategy_name (1619, 1640) -> [allocator hand-off: NOT ESTABLISHED]
               -> admit_prepared -> _admit_and_place(strategy_name=candidate.strategy_name) (1654, 1656)
```

The diagram shows only steps the evidence supports, with their labels. It is not a full trace of the receiver, the queue or the processor, and it is not a claim that any signal took any path.

## 5. Configuration: the map and the strategy registry

### 5.1 The map file

- **HISTORICAL** (C-001 4.2 to 4.4 and 5.2): config/scan_webhook_map.yaml holds one mapping, "scanners", with 16 entries. Each key equals its strategy value and its webhook path, and the 16 values are distinct; only pb01_breakout_retest is EOD-type; the 16 strategy names equal their YAML file stems. The schema allows several keys to name one strategy (N:1).
- **FACT** (count-only; Stage 1, section 3.3): C-010 did not read the map file. S1-1 counted in it 1 line of T1 (scan_webhook_map), 2 of T2 (scanner_name|scan_name), 1 of T3 (strategy_name) and 1 of T8 (scanner_type).
- **UNKNOWN / NOT ESTABLISHED** by C-010's own reads: the entries themselves and the key-strategy equality (Stage 1 U-10).

### 5.2 Loading and the processor's copy

- **FACT** (read 003): main.py's only two T1 lines (S1-1 count 2) are `main.py:2458` (scan_webhook_map=app_config.scan_webhook_map,) and `main.py:3585` (scan_webhook_map=app_config.scan_webhook_map.scanners,). The calls they belong to are not in the printed lines.
- **HISTORICAL:** the map is loaded once per start, at Phase 0b, by load_all, with no reload (C-001 5.1: main.py lines 2196 to 2202). main.py lines 2458 to 2460 pass it to run_all_startup_checks, and the receiver is given the whole AppConfig (main.py lines 3407 to 3415) and reads the map at webhook_receiver.py line 552 (C-001 5.1). Line 3585 is inside the processor's construction (C-008 2.1, W-01 and W-02: main.py lines 3574 to 3623, "the scanner map's scanners").
- **FACT** (read 002): the constructor's parameter is scan_webhook_map: Dict[str, Any], commented "# SPW2: {scanner_name: {"strategy": name}}" (`signals/signal_processor.py:166`). It is kept as self._scan_webhook_map: Dict[str, Any] = dict(scan_webhook_map) (`signals/signal_processor.py:208`), a shallow copy of the mapping passed. The module notes list the map among the constructor's required inputs (`signals/signal_processor.py:29-30`). `signals/signal_processor.py:143` reads scan_webhook_map=webhook_map, then, after six spaces, the comment # {scanner: {"strategy": name}} (FACT for the text). INTERPRETATION: an argument line whose context is not printed. HISTORICAL (C-001 5.3): the mention at signal_processor.py lines 142 to 143 is a docstring example.
- **FACT** (read 002, which printed all 9 T1 lines of the file): within signals/signal_processor.py, line 208 is the only assignment to self._scan_webhook_map. Whether anything outside the file writes it is NOT ESTABLISHED (OQ-1B-04).
- **HISTORICAL** (C-008 2.1, W-04): the processor keeps its own copies of the strategies and the scanner map, made at construction (signal_processor.py lines 207 to 208).
- **INTERPRETATION** (Stage 1, answer G): the processor works on a dict copy of app_config.scan_webhook_map.scanners made at construction, so the map in force for processing is the one loaded at the start. The map values there are schema objects (HISTORICAL, C-001 4.3), which Step 2's attribute branch serves (section 7.3).

### 5.3 The strategy registry

- **FACT** (read 005): `strategies/loader.py:149` is strategies[cfg.name] = cfg, so each strategy object is registered under cfg.name. `strategies/loader.py:151-153` read "# S10: cross-validate against scan_webhook_map", then if scan_webhook_map_path is not None: self._validate_scan_webhook_map(scan_webhook_map_path, strategies): the map cross-check runs only when a map path is passed. `strategies/loader.py:145-148` end a call whose arguments include cfg.name and cfg.intent, with a message about intent enforcement at the entry gate and the broker product chokepoint; the call's start is outside the range.
- **HISTORICAL:** cfg.name is the strategy YAML's name field; every *.yaml is loaded in sorted order, and a later file with the same name replaces the earlier one without an error (A-008 4; B-002 M22, citing line 149). The 16 names equal their file stems (C-001 4.4). No production caller passes the map path, so the S10 cross-check does not run (C-001 5.3, D-01; A-008 erratum E-1; A-009).
- **INTERPRETATION:** registration by key replaces an earlier entry of the same name (a dictionary assignment), which agrees with B-002 M22.
- **UNKNOWN / NOT ESTABLISHED:** the enclosing method's name. Line 153 calls self._validate_scan_webhook_map, so the code is in a method, but its def line is outside the range.
- **HISTORICAL** (C-008 2.1, W-04): the processor's self._strategies is its own copy of the loaded strategies, made at construction.

### 5.4 Text that describes the mapping

FACT for the text only; none of these lines is evidence of behaviour:

- the constructor comments "SPW2: {strategy_name: StrategyConfig}" (`signals/signal_processor.py:165`, read 057) and "SPW2: {scanner_name: {"strategy": name}}" (`signals/signal_processor.py:166`, read 002);
- the Step 2 header "# Step 2: Strategy lookup (SPW3)" (`signals/signal_processor.py:931`, read 001);
- the evidence helper's docstring (section 7.4);
- the dashboard reader's docstring, which says get_scan_webhook_map parses config/scan_webhook_map.yaml read-only into {scanner: strategy} (`ops_dashboard/backend/readers/config_reader.py:93`, read 021; paraphrased because the line holds a non-ASCII arrow);
- the comment "# ADDITIVE: `get_scan_webhook_map` above is byte-unchanged and keeps its callers." (`ops_dashboard/backend/readers/config_reader.py:246`, read 021);
- a module docstring stating that the map file maps 16 scanners (`ops_dashboard/backend/services/scanner_attribution.py:8`, read 022; paraphrased because the line holds a non-ASCII character).

## 6. Intake: the identity stored in the signals row

### 6.1 The key at the receiver (HISTORICAL)

- The route is POST /webhook/<scanner_name>. The segment must be a map key, matched exactly and case-sensitively, else HTTP 404 (C-001 4.3; C-002 7.1, 7.2 and 9: webhook_receiver.py lines 406 to 408 and 551 to 554). The optional alert name (scan_name) is only a cross-check against the segment (C-001 4.3; C-002 8.3).
- **FACT** (count-only; Stage 1, Q01 and Q02): signals/webhook_receiver.py has 2 T1 lines and 58 T2 lines; none of them was read. The route and the 404 are therefore UNKNOWN / NOT ESTABLISHED by C-010's own reads (Stage 1 U-01).

### 6.2 The INSERT (FACT, read 004)

`signals/webhook_receiver.py:963-980`: inside with self._store.transaction() as cur:, the statement INSERT INTO signals (signal_id, symbol, scanner, strategy, triggered_at, received_at, expires_at, status, fingerprint, fingerprint_date, trigger_price, webhook_payload), with twelve placeholders and the values (signal_id, symbol, scanner_name, scanner_name, triggered_at_iso, received_at_iso, expires_at_iso, "QUEUED", fingerprint, today_iso, price, webhook_payload). Both identity columns receive scanner_name (975); no mapped strategy name appears in the statement.

- Enclosing function: UNKNOWN / NOT ESTABLISHED by C-010's read (its def line is outside 963 to 980). HISTORICAL: C-001 12 (T-8) places the insert in the per-symbol acceptance, webhook_receiver.py lines 892 to 1072.

### 6.3 BK-27: the strategy column stores the scanner key (KNOWN DEFECT / AMBIGUITY, latent; OPEN)

- Recorded by C-001 as D-05 (KNOWN DEFECT / AMBIGUITY, latent), re-verified by C-007 2.1, and bookmarked as BK-27, "strategy column stores the scanner name" (ChatGPT's Stage 0 answer sheet; its owners include K-003: Stage 0 answers, OQ-04). C-010 re-verified the INSERT at the source line (read 004: scanner_name into both columns); that scanner_name is the webhook path key stays HISTORICAL (C-001 4.3; C-001 12, T-8). BK-27 stays OPEN (OQ-04; manual-draft prompt, item 8).
- **HISTORICAL** (C-001 7.2; not re-verified by C-010): no runtime writer updates signals.strategy after the insert (core/state_store.py lines 1424, 2996 and 3057), and there is no effect at the pin because each key equals its strategy (C-001 4.3). C-010 counted 2 T6 lines and 4 T3 lines in core/state_store.py and read none (Stage 1 U-11).
- The operational impact on any day is NOT ESTABLISHED (Stage 1 and Stage 1B reconciliations).

### 6.4 The processor test with a key different from its strategy (FACT, read 006; test text, not run)

`tests/unit/test_signal_processor.py:1933-1964`: the processor is built with strategies {"gap_go_short_v1": _SELL_STRATEGY} and scan_webhook_map {"gap_go_short": {"strategy": "gap_go_short_v1"}} (1933 to 1937). A WatchEntry is built with strategy_name "gap_go_short_v1" and scanner_name "gap_go_short" (1939 to 1954). The test then writes its own signals row, INSERT OR IGNORE INTO signals (signal_id, symbol, scanner, strategy, ...), with scanner "gap_go_short", strategy "gap_go_short_v1" and status "PROCESSING" (1956 to 1964).

- **FACT:** the fixture's signals row stores the mapped strategy name, which the receiver's INSERT does not (6.2); the row is written by the test, not by the receiver.
- **UNKNOWN / NOT ESTABLISHED:** the test's name and what it asserts, both outside the range (Stage 1 U-06). HISTORICAL (C-001 7.2): no test covers the production insert.
- **INTERPRETATION:** the WatchEntry points to a test of the gate path, E2, where processing takes "the strategy named in the WatchEntry" (HISTORICAL, C-008 9.1).

### 6.5 Hand-over to the processor (HISTORICAL)

The receiver queues a tuple for each stored symbol (C-001 12, T-8), and the processor's resolution starts from that queue tuple (C-001 12, T-9: signal_processor.py lines 867 to 887). C-010 did not re-read the hand-over (Stage 1 U-02).

## 7. Resolution: the processor's Step 2

### 7.1 The lookup (FACT, read 001)

`signals/signal_processor.py:930-953`, under the comment "# Step 2: Strategy lookup (SPW3)" (931):

| Check | Lines | Code | On failure |
|---|---|---|---|
| 1. The map entry by key | `signals/signal_processor.py:933-938` | map_entry = self._scan_webhook_map.get(scanner_name) | if None: _PipelineReject("UNKNOWN_STRATEGY", f"No entry in scan_webhook_map for scanner {scanner_name!r}") |
| 2. The strategy name from the entry | `signals/signal_processor.py:939-947` | strategy_name = map_entry.get("strategy") if isinstance(map_entry, dict) else getattr(map_entry, "strategy", str(map_entry)) | if None: _PipelineReject("UNKNOWN_STRATEGY", f"scan_webhook_map entry for {scanner_name!r} has no 'strategy' key") |
| 3. The strategy object by name | `signals/signal_processor.py:948-953` | strategy_obj = self._strategies.get(strategy_name) | if None: _PipelineReject("UNKNOWN_STRATEGY", f"Strategy {strategy_name!r} not in loaded strategies") |

The map lookup uses the scanner key; the strategy lookup uses the entry's strategy value.

### 7.2 What a failed lookup becomes

- **FACT:** each failure raises _PipelineReject with the code "UNKNOWN_STRATEGY" and its own message (7.1). FACT (count-only; Stage 1, section 3.4): S1-1 counted 5 UNKNOWN_STRATEGY (T5) lines in signals/signal_processor.py. Four are printed: 936, 945 and 951 (read 001) and 2409, on E3 (read 057, section 9). The fifth is not printed.
- **HISTORICAL** (C-008 ST-08; C-001 7.4): each failure ends as REJECTED_UNKNOWN_STRATEGY. How the code becomes the stored status is UNKNOWN / NOT ESTABLISHED by C-010's reads (Stage 1 U-04).
- **HISTORICAL** (C-008 4.1): Step 2 is stage ST-08 of the main path, and C-008 names C-010 as the rule's owner; strategy control follows as ST-09 (C-001 7.4; C-008 ST-09).

### 7.3 The two branches (INTERPRETATION)

The dict branch serves a plain-dict map (the test of 6.4 passes one); the attribute branch serves objects such as the schema's entries. For an object without a strategy attribute, the default str(map_entry) becomes the name, and the third check would reject it unless a strategy had that name. The second check's message names a 'strategy' key, so its wording fits a dict entry.

### 7.4 The evidence helper (FACT for the code and the text, read 031)

`signals/signal_processor.py:557-580`: a section comment marks forward evidence capture as an observer that never affects a decision (557 to 559). Under it, _evidence_strategy_for(self, scanner_name) (560) looks the same map up by scanner_name (573), takes the strategy as Step 2 does (576 to 577), and returns None, not a reject, when the entry is missing or the name is empty (574 to 575, 578) or on any exception (579 to 580). Its docstring (561 to 571) says the helper is "never read by a decision", that its answer is resolved exactly as _process_one's Step 2 resolves it (563 to 564), and that the map is "never written after __init__".

- These claims are FACT for the text only. Whether any decision reads the helper is NOT ESTABLISHED: no caller search was run (D9 declined; OQ-1B-04). A comment alone is not proof of behaviour.

## 8. E1: from the map entry to trades.strategy

HISTORICAL (C-008 18): at the pin, main.py builds one processor and wires one live entry path, the dispatcher's _process_one (E1). This describes the source and the repository wiring, not what ran on any day. Rows H0a to H0c are the inputs to Step 2, from sections 5 and 6; they come before the processor's lookup, where the chain rule (section 2) starts. Rows H1 to H13 are the Stage 1D chain (reads 043, 046 and 051 to 058) with the Stage 1E finding of section 10. "Within T3's reach" means that every line spelling strategy_name in the file was printed (Q45 and read 057, 58 of 58 lines); "within T25's reach" means the same for the word strategy in orders/order_placer.py (Q44 and read 056, 26 of 26).

### 8.1 Hop by hop

| Hop | Source and lines (read) | Input | Logic | Guard / validation | Output / hand-off | Next consumer | Label |
|---|---|---|---|---|---|---|---|
| H0a Map load and the processor's copy | `main.py:3585` (003); `signals/signal_processor.py:166, 208` (002) | app_config.scan_webhook_map.scanners (HISTORICAL: loaded once per start, at Phase 0b, by load_all, and validated with the map's schema class: C-001 5.1, 5.2) | passed as scan_webhook_map= at 3585, inside the processor's construction (HISTORICAL: C-008 2.1, W-01, W-02); the constructor's parameter (166) is kept as dict(scan_webhook_map) at 208 | HISTORICAL: a schema failure stops the start with exit 5, and a duplicate key is not detected (C-001 5.2); no check in the processor's printed lines | self._scan_webhook_map, assigned only at 208 within the file (FACT within T1's reach, read 002) | Step 2's first check (933, read 001) and the evidence helper (573, read 031) | FACT for the lines; the call holding 3585 HISTORICAL; a writer outside the file NOT ESTABLISHED (OQ-1B-04) |
| H0b Strategy registration | `strategies/loader.py:149, 151-153` (005) | each loaded strategy configuration, cfg (HISTORICAL: every *.yaml, in sorted order: A-008 4; B-002 M22) | strategies[cfg.name] = cfg | the S10 map cross-check runs only when a map path is passed (151 to 153, FACT), and no production caller passes one (HISTORICAL: C-001 5.3, D-01); a later file with the same name replaces the earlier one without an error (HISTORICAL: B-002 M22) | the loaded strategies, keyed by name | the processor's self._strategies, its own copy made at construction (HISTORICAL: C-008 2.1, W-02, W-04), read by Step 2's third check (948, read 001) | FACT for the lines; the enclosing method NOT ESTABLISHED (5.3); the hand-on to the processor HISTORICAL |
| H0c Intake | `signals/webhook_receiver.py:963-980` (004) | the path segment of POST /webhook/<scanner_name> (HISTORICAL: C-001 4.3) | INSERT INTO signals with scanner_name as both scanner and strategy (975) and status QUEUED | HISTORICAL: the key must be a map key, else HTTP 404 (C-001 4.3; C-002 7.1, 7.2 and 9); the optional alert-name cross-check (C-002 8.3); the per-symbol acceptance (C-001 12, T-8) | a signals row; a queue tuple per stored symbol (HISTORICAL: C-001 12, T-8) | _process_one, whose resolution starts from the queue tuple (HISTORICAL: C-001 12, T-9, signal_processor.py lines 867 to 887) | FACT for the INSERT; HISTORICAL for the key, the checks and the hand-over (Stage 1 U-01, U-02); the enclosing function NOT ESTABLISHED by C-010 (6.2) |
| H1 Step 2 | `signals/signal_processor.py:930-953` (001) | scanner_name from the queue item (HISTORICAL, C-001 12, T-9) | the map entry by key; the strategy name from the entry; the strategy object by name | three UNKNOWN_STRATEGY rejects (7.1) | strategy_name, strategy_obj | the later stages of _process_one (HISTORICAL, C-008 ST-09 onward) | FACT; the input and the stored status HISTORICAL |
| H2 Through _process_one | `signals/signal_processor.py:939, 943, 948, 952, 987, 1046, 1139, 1155, 1165, 1184` (057); `signals/signal_processor.py:867, 1182, 1277` (058) | strategy_name set at 939 | before line 1184, line 939 is the only assignment statement to strategy_name; the T3 lines between are a condition (943), parts of other statements (948, 952, 987, 1139, 1155, 1165) and a keyword-argument line (1046) | - | strategy_name, unchanged | the call at 1182 | FACT within T3's reach; INTERPRETATION that 1046 is a keyword argument (its call's first line not printed) and that 939 and 1182 lie in _process_one (definition order: _process_one's def at 867 and _admit_and_place's at 1277 are the def lines T26 shows; the definitions between 867 and 1455 were not read, OQ-1B-05) |
| H3 The call | `signals/signal_processor.py:1182, 1184` (057, 058) | strategy_name | self._admit_and_place( at 1182; strategy_name=strategy_name at 1184 | - | a keyword argument | _admit_and_place | FACT for both lines; INTERPRETATION that 1184 is an argument of the call opened at 1182 (1183 not printed) |
| H4 The parameter | `signals/signal_processor.py:1277-1278` (058, 057) | the keyword argument | def _admit_and_place( with "self, ac, *, signal_id, symbol, scanner_name, strategy_name, strategy_obj, ..." | keyword-only parameter | the strategy_name parameter | the body | FACT |
| H5 Through _admit_and_place | `signals/signal_processor.py:1311, 1330, 1373, 1428, 1455` (057) | the parameter | no assignment statement to strategy_name between 1278 and 1455; the T3 lines between are an argument (1311), keyword arguments (1330: strategy=strategy_name,; 1373: strategy_name=strategy_name,) and a dictionary value (1428) | - | strategy_name, unchanged | the placement call | FACT within T3's reach; INTERPRETATION that 1373 is a keyword argument and that 1455 lies in _admit_and_place (after 1277, before the next definition T3 shows, 1578) |
| H6 Placement call | `signals/signal_processor.py:1446-1460` (043) | strategy_name, strategy_obj, signal_id | self._placer.place(...) with strategy=strategy_name (1455), signal_id=signal_id (1453), intent=strategy_obj.intent (1452) and the strategy's risk-reward taken from strategy_obj (1459) | - | the strategy argument | OrderPlacer.place (HISTORICAL: C-008 4.1, ST-29, which cites order_placer.py lines 871 to 958 for place(); B-009 7, one of three callers of place()) | FACT |
| H7 The placer's parameter | `orders/order_placer.py:882` (056) | the argument | strategy: str = "" | - | the parameter | the body of place() | FACT for the line; INTERPRETATION that it is place()'s parameter (HISTORICAL: place() begins at 871, and 882 lies inside C-008's cited range, 871 to 958) |
| H8 Through place() | `orders/order_placer.py:887, 991, 1000` (056) | the parameter | between 882 and 1000 the only other T25 lines are 887 (a comment on another parameter) and 991 (an argument of resolve_slippage_fraction); neither binds strategy | - | strategy, unchanged | the create_trade call | FACT within T25's reach |
| H9 The create_trade call | `orders/order_placer.py:986-1016` (055); `orders/order_placer.py:996` (054) | strategy | under the comment "OP4: create trade row FIRST (status=PENDING_FILL)" (993), trade_id = self._om.create_trade( (996) with keyword arguments to 1015, among them strategy=strategy (1000), signal_id=signal_id (997) and mode=self._mode (1010) | - | a keyword argument | OrderManager.create_trade | FACT for the lines; INTERPRETATION that 996 lies in OrderPlacer.place, by definition order: place() begins at 871 (HISTORICAL: C-008 4.1, ST-29) and its docstring at 891 (C-008 8, SS-02); C-008's cited range, 871 to 958, does not reach 996, and the definitions between 871 and 996 were not read (OQ-1B-05) |
| H10 The call target | `orders/order_manager.py:175` (051); Q48 | self._om.create_trade | the only definition of create_trade in s_rt (Q48: 1 line, in order_manager.py); the only create_trade( call line in s_rt is order_placer.py 996 (section 10) | - | - | create_trade | INTERPRETATION: a unique definition by name; the type of self._om not read |
| H11 The function | `orders/order_manager.py:175-198` (051, 052) | keyword arguments | create_trade(self, *, signal_id, symbol, direction, strategy: str, ...) at 175, strategy at 181; docstring "Insert a new trade row with status=PENDING_FILL. Returns trade_id." (198) | keyword-only parameter | the strategy parameter | the INSERT | FACT |
| H12 No rebinding | `orders/order_manager.py:182-213` (052); `orders/order_manager.py:214-244` (046) | the parameter | strategy is not assigned between 181 and the statement; 214 to 244 are the SQL text | - | strategy, unchanged | the argument tuple | FACT |
| H13 The INSERT | `orders/order_manager.py:212-262` (052, 046, 053) | the parameters | cur.execute opened at 212; INSERT INTO trades with 35 columns and 35 values (214 to 244); the argument tuple (246 to 261) binds 30 values to the 30 "?" in order; the 5th column, strategy (215), takes the 5th value, a "?" (230), bound to the tuple's 5th item, strategy (247); 'PENDING_FILL' is a literal value (236) | - | a trades row; return trade_id (270) | trades.strategy | FACT, by position |

The whole of create_trade, from its def at 175 to the next def at 272, is printed by reads 052, 046 and 053 (175 to 271).

### 8.2 What trades.strategy is supplied with on E1

- **INTERPRETATION** (accepted as such in ChatGPT's Stage 1D reconciliation and in the manual-draft prompt, item 3): at the pin, the documented E1 path supports the conclusion that trades.strategy is supplied with the strategy name resolved from the scanner map entry, not the scanner key. That is the name Step 2 resolves, which must be a key of the processor's loaded strategies (948 to 953), carried unchanged through _process_one, _admit_and_place, OrderPlacer.place and OrderManager.create_trade to the 5th bound value.
- **Why INTERPRETATION** under the chain rule: every hop's own lines are FACT, but these parts rest on INTERPRETATION: H2 and H5 (lines 1046 and 1373 read as keyword arguments; function membership by definition order), H3 (line 1184 read without 1183), H7 and H9 (the membership of 882 and 996 in place(): its start at 871 is HISTORICAL, and C-008's cited range, 871 to 958, holds 882 but does not reach 996) and H10 (the call target by its unique name). No hop on E1 is NOT ESTABLISHED.
- **HISTORICAL agreement:** A-008 21 lists trades.strategy among the consumers of the strategy YAML's name field ("Identity").
- **Not a runtime observation:** no stored trades row, deployed host or trading day was examined, and no trade is claimed to have occurred.
- **Later changes:** whether any code updates trades.strategy after the insert is UNKNOWN / NOT ESTABLISHED; no search for such writes was authorised.

### 8.3 INSERT INTO trades in the runtime files (FACT, count-only; Stage 1C, Q38)

T14 (\bINTO\s+trades\b) found 2 lines in s_rt's 281 files: orders/order_manager.py 214 (H13) and scripts/t2_cnc_gtt_realtest.py 234, the string piece "INSERT INTO trades " (read 045). The second is recorded as a lead and was not inspected further (OQ-1C-01). HISTORICAL (C-007 4.1): a script under scripts/ that seeds its own signals row; test-like, never run, live reach not established. core/state_store.py and orders/order_placer.py have no T14 line. T14 does not see run-time or f-string table names, lower-case SQL, or a statement split across string pieces before the table name, so a count of 0 is not proof that a file never inserts into trades.

## 9. The other entry paths: E2 gate, E3 retest and E4 allocator

**HISTORICAL** (C-008 2.2): E2 is continue_from_gate(entry, release_ltp), called by main's gate-release callback only for a release with reason PRICE_HIT. E3 is continue_from_retest(parked), called by the retest monitor's on_confirm, only on CONFIRMED, after it releases the parking row. E4 is admit_prepared(candidate); C-008 2.2 names the portfolio allocator's enforce worker as its caller and records that main.py passes E4's two callbacks while the worker that calls them was not read (this manual adopts none of C-008's statements about that worker: 9.2). B-009 7 lists three callers of place(): signal_processor.py lines 1446, 2251 and 2553. No read was made for E2, E3 or E4 in Stage 1E (D14; D15 declined); every E2, E3 and E4 line below was printed by read 057 or 058 in Stage 1D. The open question is OQ-1D-01, with one sub-item per path: OQ-1D-01a (E2), OQ-1D-01b (E3) and OQ-1D-01c (E4) (section 14.1).

### 9.1 The four paths side by side

| Path | Entry point (HISTORICAL, C-008 2.2) | Where strategy_name comes from (FACT) | Lookup in the loaded strategies | Hand-on towards the placer | Status of the name's source |
|---|---|---|---|---|---|
| E1 | _process_one | Step 2, from the map entry (`signals/signal_processor.py:939`, read 057) | 948 to 953 (read 001) | `signals/signal_processor.py:1446-1460` through _admit_and_place (read 043) | established at INTERPRETATION (section 8) |
| E2 gate | continue_from_gate | strategy_name = entry.strategy_name (`signals/signal_processor.py:2051`, read 057) | 2052 and 2056 (read 057; the reject read as INTERPRETATION) | strategy=strategy_name (`signals/signal_processor.py:2260`, read 057; the call at 2251, HISTORICAL) | UNKNOWN / NOT ESTABLISHED (OQ-1D-01a) |
| E3 retest | continue_from_retest | strategy_name = parked.strategy (`signals/signal_processor.py:2387`, read 057) | 2407 and 2409 (read 057) | strategy=strategy_name (`signals/signal_processor.py:2557`, read 057; the call at 2553, HISTORICAL) | NOT ESTABLISHED beyond the processor-side lines held (OQ-1D-01b) |
| E4 allocator | admit_prepared | strategy_name=candidate.strategy_name (`signals/signal_processor.py:1656`, read 057) | not in the lines held | through the call at `signals/signal_processor.py:1654` (read 058), then _admit_and_place to 1455 | the allocator hand-off NOT ESTABLISHED (OQ-1D-01c) |

### 9.2 Each path as decided (D14 and D15)

| Path | C-010's own FACT lines (read 057 unless stated) | HISTORICAL spans and INTERPRETATION | Recorded as | Reason (the Stage 1E authorisation, Part 3) |
|---|---|---|---|---|
| E2 gate | `signals/signal_processor.py:2051` strategy_name = entry.strategy_name  # type: ignore[attr-defined]; `signals/signal_processor.py:2052` strategy_obj = self._strategies.get(strategy_name); `signals/signal_processor.py:2056` f"Strategy {strategy_name!r} not in loaded strategies",; `signals/signal_processor.py:2260` strategy=strategy_name,. No assignment statement to strategy_name between 2051 and 2260: the T3 lines between are a lookup (2052), a message (2056), arguments (2090, 2141), keyword arguments (2160; 2194: strategy_name=strategy_name,) and a dictionary value (2242). FACT within T3's reach. | C-008 9.1 (EP-01): lines 2050 to 2093 of E2 take "the strategy named in the WatchEntry", then strategy control, the strategy window and the governor, mirroring ST-08 to ST-11 (ST-08: a strategy not loaded gives REJECTED_UNKNOWN_STRATEGY). INTERPRETATION: 2051 and 2052 lie in that span; 2056 is the message of that reject (2053 to 2055 not printed; the same pattern as E1's 948 to 953, read 001); 2194 is a keyword argument (its call's first line not printed); 2260 is an argument of the place() call at 2251 (B-009 7). C-008 EP-02 (KNOWN DEFECT): no runtime file calls EntryGate.add(); E2 runs only for a gate_state row reloaded at start and released with PRICE_HIT; whether such rows existed is not established. | UNKNOWN / NOT ESTABLISHED (OQ-1D-01a) | D14: the stored gate-state values cannot be established by the authorised source reads, and the relevant writer has no runtime caller in the frozen historical record |
| E3 retest | `signals/signal_processor.py:1046` sl_price=sl_price, strategy_name=strategy_name,; `signals/signal_processor.py:2387` strategy_name = parked.strategy; `signals/signal_processor.py:2407` strategy_obj = self._strategies.get(strategy_name); `signals/signal_processor.py:2409` raise _PipelineReject("UNKNOWN_STRATEGY", f"Strategy {strategy_name!r} not loaded"); `signals/signal_processor.py:2557` reservation_id=reservation_id, strategy=strategy_name,. No assignment statement to strategy_name between 2387 and 2557: the T3 lines between are a lookup (2407), a reject (2409), arguments (2428, 2477), keyword arguments (2493; 2515: symbol=symbol, strategy_name=strategy_name, score=None,) and a dictionary value (2544). FACT within T3's reach. | C-008 ST-15: the retest divert is signal_processor.py lines 1037 to 1056 (with screening/retest_monitor.py lines 303 to 379), only with a diverter whose enabled flag is true, and none is set. C-008 2.2: E3's entry point, continue_from_retest(parked), at signal_processor.py lines 2375 to 2384. C-008 EP-04: lines 2407 to 2430 mirror ST-08 to ST-11. INTERPRETATION: 1046 is an argument of the divert call (its first line not printed); 2387 and 2407 to 2409 are on E3; 2515 holds keyword arguments (its call's first line not printed); 2557 is an argument of the place() call at 2553 (B-009 7). C-008 EP-05: with the repository configuration, E3 has no caller. | NOT ESTABLISHED beyond the processor-side lines held (OQ-1D-01b) | D14: tracing the value requires multiple module and storage hops (the divert's parking write, the stored parking row, the confirm hand-off) |
| E4 allocator | `signals/signal_processor.py:1155` and `signals/signal_processor.py:1165` signal_id, symbol, scanner_name, strategy_name, strategy_obj,; `signals/signal_processor.py:1619` def _build_candidate(self, signal_id, symbol, scanner_name, strategy_name,; `signals/signal_processor.py:1640` signal_id=signal_id, symbol=symbol, strategy_name=strategy_name, side=side,; `signals/signal_processor.py:1654` self._admit_and_place( (read 058); `signals/signal_processor.py:1656` scanner_name=p.scanner_name, strategy_name=candidate.strategy_name,. No T3 line between 1619 and 1640 (FACT within T3's reach). | C-008 ST-19: the allocator hook is signal_processor.py lines 1145 to 1172. C-008 OB-02: the candidate copy is built at lines 1619 to 1644. C-008 2.2 and EP-08: E4 is admit_prepared(candidate), lines 1646 to 1704, which reuses _admit_and_place. INTERPRETATION: 1155 and 1165 are arguments of calls in the hook, in the order of _build_candidate's parameters (1619); 1640 gives the candidate the parameter's strategy_name; 1656 is an argument of the call at 1654 (1655 not printed). | From held evidence only; the allocator hand-off NOT ESTABLISHED (OQ-1D-01c) | D15 declined: allocation/portfolio_allocator.py was not counted or read |

**No mode and no execution is inferred for E4.** C-008 (EP-07, and section 18) records the processor's side of the hand-off; its statement about the allocator's worker rests, as C-008 itself says, on comments. This manual does not adopt that statement and makes no claim about whether E4 ran or in which mode.

### 9.3 What sections 8 to 10 leave for trades.strategy on the other paths (INTERPRETATION; no new read)

- On E1, E2 and E3, a strategy name passed on to place() has passed the processor's lookup in its loaded strategies (E1 948 to 953, read 001; E2 2052 and 2056; E3 2407 and 2409; for E2 and E3 as read in 9.2). From place() on, the hops are those of E1 (section 8.1, H7 to H13), with the one create_trade( call line of section 10. So if a trade were placed on E2 or E3, trades.strategy would take that path's strategy_name, a loaded strategy's name. Which name, and where the stored row got it, stays NOT ESTABLISHED.
- On E4, the hook's calls pass the strategy_name of the E1 run they are in (1155, 1165; no assignment statement to strategy_name between 939 and 1184, H2); _build_candidate takes strategy_name as a parameter (1619) and gives it to the candidate (1640), with no T3 line between (9.2); and admit_prepared passes candidate.strategy_name on to _admit_and_place (1656), whose tail to the column is E1's (H4 to H13). Whether the candidate admitted is the candidate built, with its name unchanged, is the allocator hand-off that stays NOT ESTABLISHED.
- None of this is a statement that E2, E3 or E4 ran on any day, or in which mode.

## 10. The second create_trade( line in orders/order_manager.py (Stage 1E, G17)

Stage 1D counted 2 lines of T23 (\bcreate_trade\() in orders/order_manager.py (Q47) and knew only one, the definition at 175 (O-8; OQ-1D-02). T23 also matches the name in a comment or a docstring, by design (the Stage 1E proposal, section 3). Read 059 printed both lines.

| Item | Claim | Label | Evidence |
|---|---|---|---|
| The two lines | Q47's two T23 lines in order_manager.py are 12 and 175; 175 is the definition of create_trade. | FACT | read 059; Q47; read 051 |
| The text | Line 12 as printed: four spaces, then "OMgr2 -- create_trade(", an ellipsis character (U+2026), ") ", an arrow character (U+2192) and " trade_id (trd_<hex32>). Assigns trade_id". The verbatim line is in the Stage 1E evidence sheet, section 5; "<hex32>" is source text, not a masking placeholder. | FACT | read 059 |
| Not a definition, not a comment | Its first non-blank characters are "OMgr2", not "def", "class" or "#"; the line holds no quote character. | FACT (its characters) | read 059 |
| A string line, not a call | Python accepts an ellipsis character or an arrow only inside a string or a comment. As the line is not a comment, it is a line of a multi-line string, so its "create_trade(" is text, not a call. | INTERPRETATION: Python's syntax applied to the FACT line; the file was not compiled or run, and the string's first and last lines (its boundaries) were not read | read 059 |
| Its position | Line 12 precedes the file's first definition or class line (67, class OrderInsertSpec); read 051 printed all 18 such lines (Q43 counted 18). So it lies in no function or class, and a string at that place is most likely the module docstring. | INTERPRETATION (definition order; position) | reads 051, 059; Q43 |
| Its use | What line 12 refers to, or how its text continues, beyond the line itself. | NOT ESTABLISHED (no further read) | - |

**Consequence (INTERPRETATION within T23's term and scope).** Q47 counted 3 T23 lines in s_rt's 281 files: order_manager.py 12 (a string line, as above), order_manager.py 175 (the definition, FACT) and order_placer.py 996 (trade_id = self._om.create_trade(, FACT, read 054). So within s_rt, order_placer.py 996 is the only create_trade( call line. This rests on the reading of line 12 and on T23's reach: a call written with a space before the parenthesis, or made under another name, would not be counted. It does not change the E1 finding (section 8.2) or hop H10 (the call target by its unique name; the type of self._om not read). O-8 stays in the record with this outcome.

## 11. Readers of the stored identities and of the map

Stage 1 found ten R14 to R20 files whose printed lines key rows or map entries by "strategy" or "scanner", and no printed line showed the table (Stage 1, answer F). Stage 1B's rule P paired consumer lines with the nearest counted query within 30 lines above them, and Stage 1C added three narrow follow-ups. The tables below hold what those reads show. HISTORICAL (C-001 7.2 and 8) places reads of signals.strategy and signals.scanner in the replay tool, the registry officer and db_reader.py at ranges that C-010's lines fall inside.

### 11.1 Readers of signals.strategy

| File and lines (read) | What the lines show | Label |
|---|---|---|
| `ops_dashboard/backend/readers/db_reader.py:617-621` (037) | SELECT strategy, COUNT(*) AS n FROM signals WHERE received_at LIKE ? GROUP BY strategy; returns {strategy: n} | FACT for the query and its use; INTERPRETATION for the variable binding (the assignment above 617 not read) |
| `ops_dashboard/backend/readers/db_reader.py:684-704` (034) | strategy_signal_funnel: the per-strategy stored-signal funnel for today, from signals grouped by strategy and bucket | FACT |
| `ops_dashboard/backend/readers/db_reader.py:1058-1061` (038) | per-strategy risk and capital rejects, from signals grouped by strategy | FACT; INTERPRETATION for the binding |
| `ops_dashboard/backend/readers/db_reader.py:2093-2101` (035) | strategy_signal_counts_range: the per-strategy stored-signal count over a date range | FACT |
| `ops_dashboard/backend/readers/db_reader.py:3979-4002` (040) | signals for today grouped by strategy, put into the merged view (11.4) | FACT; INTERPRETATION for the binding |
| `ops_dashboard/backend/readers/db_reader.py:4050-4055` (041) | per-strategy signal counts by day | FACT; INTERPRETATION for the binding |
| `ops_dashboard/backend/readers/db_reader.py:4100-4105` (042) | rejected signals for today by strategy and status | FACT; INTERPRETATION for the binding |
| `scripts/strategy_registry_officer.py:53-60` (032) | _distinct_signal_names reads SELECT DISTINCT scanner and SELECT DISTINCT strategy from signals into one set of names; its docstring says it collects the names that have ever appeared in signals | FACT |
| `scripts/replay_signals.py:41, 46, 91-95, 191, 195, 196` (049, 027, 033, 050) | fetch_signals is defined at 41 and the signals query is at 46; the consumer lines 91 to 95 take strategy_name = signal["strategy"] and skip the signal when that name is not a loaded strategy ("not in current config"); run_replay assigns signals = fetch_signals(...) (191), loops over it (195) and passes each item to replay_one_signal (196) | FACT for the lines; INTERPRETATION that 46 lies in fetch_signals and 91 to 95 in replay_one_signal (definition order); NOT ESTABLISHED: fetch_signals's return line and replay_one_signal's parameter binding (OQ-1B-02); HISTORICAL (C-001 7.2): lines 91 to 95 read signals.strategy |

### 11.2 Readers of signals.scanner

| File and lines (read) | What the lines show | Label |
|---|---|---|
| `ops_dashboard/backend/readers/db_reader.py:2072-2090` (035) | signals_scanner_funnel_range: the per-scanner stored-signal funnel over a date range, from signals grouped by scanner | FACT |
| `ops_dashboard/backend/readers/db_reader.py:1651-1666` (036) | scanner_for_trades: {trade_id: scanner} from the string pieces "SELECT t.trade_id AS trade_id, s.scanner AS scanner ", "FROM trades t JOIN signals s ON s.signal_id = t.signal_id " and "WHERE t.trade_id IN (" (1661 to 1663); its docstring calls this the exact attribution path, shared with the scanner-attribution screen (paraphrased: the docstring holds non-ASCII characters) | FACT for the code and the text |
| `scripts/strategy_registry_officer.py:57-59` (032) | the DISTINCT scanner values, in the same set as the strategy values (11.1) | FACT |
| `ops_dashboard/backend/services/analytics_period.py:513, 525, 534, 658` (018) | per-scanner trade aggregates (513 to 534) and rows with "scanner": scanners.get(t["trade_id"]) (658) | FACT for the lines; INTERPRETATION: consistent with scanner_for_trades (1666), but the call is not shown (Stage 1B); NOT ESTABLISHED: the source of the same row's "strategy": t["strategy"] (658), one of the 57 lines of OQ-1B-01, so the file is not listed as a reader of trades.strategy (11.3) |

### 11.3 Readers of trades.strategy

| File and lines (read) | What the lines show | Label |
|---|---|---|
| `ops_dashboard/backend/readers/db_reader.py:1065-1081` (038) | strategy_loss_streaks: SELECT strategy, net_pnl FROM trades, then a streak per strategy | FACT |
| `ops_dashboard/backend/readers/db_reader.py:1669-1681` (039) | strategy_sltgt_hits: per-strategy SL_HIT and TGT_HIT counts from trades grouped by strategy | FACT |
| `ops_dashboard/backend/readers/db_reader.py:3981-3989` (040) | entry orders for today through orders JOIN trades, grouped by t.strategy; trades for today grouped by strategy | FACT |
| `scripts/compute_strategy_metrics.py:167` (030); `scripts/compute_strategy_metrics.py:173` (017) | SELECT DISTINCT strategy FROM trades (167); [r["strategy"] for r in rows if r["strategy"]] (173) | FACT for the lines; INTERPRETATION that 173 consumes the query at 167 (6 lines apart) |

### 11.4 The merged view (FACT for the code; INTERPRETATION for the consequence)

`ops_dashboard/backend/readers/db_reader.py:3979-4008` (read 040) puts three counts into slots keyed by one name: signals for today by strategy (3979), entry orders for today by t.strategy through orders JOIN trades (3981 to 3985), and trades for today by strategy (3986 to 3989). The slots are made at 3993 to 3995 and filled at 3997 to 4008; `ops_dashboard/backend/readers/db_reader.py:4010` (read 009) is the trades count's assignment, in the loop over trades opened at 4007 (INTERPRETATION only; O-4).

INTERPRETATION: with a key different from its strategy, a scanner's signals would be counted under the key (signals.strategy is supplied with the key: section 6) and its orders and trades under trades.strategy (the resolved name on E1: section 8), in two different slots. At the pin every key equals its strategy (HISTORICAL), so the two coincide. No runtime claim is made.

### 11.5 Readers of the map outside the trading process (the dashboard)

- **FACT** (read 048): config_reader.py's module-level functions, in order: get_system_config 28, get_strategies 56, get_scan_webhook_map 92, get_min_pass_score 118, get_cron_jobs 150, load_accounts 180, active_account 196, dotted 219, config_meta 229, get_scanner_registry 249, get_scoring_weights 298 and get_broker_costs 311 (T19 sees module-level def lines only).
- **FACT for the lines** (reads 020, 021): after def get_scan_webhook_map (`ops_dashboard/backend/readers/config_reader.py:92`), a path is built from cfg["paths"]["config_dir"] and "scan_webhook_map.yaml" (99), and entry.get("strategy") is taken for a dict entry, else the entry itself (112). After def get_scanner_registry (249), which follows the "ADDITIVE" comment (246), the printed lines name the same file under the same config_dir (252, 267), read each entry's strategy and chartink_url (281) and append entries with "scanner" and "strategy" (284 to 285). INTERPRETATION that 99 and 112 belong to get_scan_webhook_map (no module-level def between 92 and 118) and 252 to 285 to get_scanner_registry (next bullets).
- **FACT** (reads 023, 024, 026): get_scan_webhook_map is called by `ops_dashboard/backend/services/strategy_tower.py:96`, `ops_dashboard/backend/services/analytics_period.py:504` and `ops_dashboard/backend/services/config_view.py:138`; the configuration view labels the map "from config/scan_webhook_map.yaml; excluded from drift" (`ops_dashboard/backend/services/config_view.py:137`).
- **FACT** (reads 047, 022, 011): scanner_attribution.py's only call of the form config_reader.NAME( is `ops_dashboard/backend/services/scanner_attribution.py:293`, registry = config_reader.get_scanner_registry(cfg) (Q39; within that pattern's reach). Within T1's reach the file never names get_scan_webhook_map: read 022 printed its four T1 lines (8, 480, 662 and 682), which are prose or a source label. Lines 298 to 299 build strategy-to-scanners lists from entries, allowing several scanners per strategy (`ops_dashboard/backend/services/scanner_attribution.py:298-299`), and the mapping rows show each entry's scanner, URL and strategy (`ops_dashboard/backend/services/scanner_attribution.py:672, 674, 675, 732`).
- **INTERPRETATION** (Stage 1C, 8.2): lines 252 to 285 are get_scanner_registry's body, by definition order (a nested def or a class would not be seen by T19), and the entries at 298 to 299 are the items of registry, 5 lines below the call (294 to 297 not read). So screen 21's strategy-to-scanners entries come from the dashboard's second, additive reader of the map file, not from get_scan_webhook_map.
- **HISTORICAL** (C-001 8, 5.1 and A-L8): screen 21 is built by build_scanner_attribution_screen (scanner_attribution.py lines 275 to 423). The dashboard reads the map file on each call, from its own config_dir, with no schema. Which file the production dashboard read is UNKNOWN.
- **FACT** (reads 025, 026): the two audit.py lines are a docstring line and a comment (one says that webhook_audit.scanner_name is not selected; one lists configuration names), and config_view.py shows the map in the configuration view. INTERPRETATION: neither takes part in resolving an identity.

### 11.6 What stays unknown about the readers

- **OQ-1B-01:** 57 consumer lines whose feeding query was not shown under rule P (Stage 1B, section 7). They are in db_reader.py, scanner_attribution.py, strategy_ranking.py, strategy_health.py, compute_strategy_metrics.py, analytics_period.py, analytics2.py, config_reader.py and replay_signals.py. For five of them (replay_signals.py 75, 76 and 91; scanner_attribution.py 298 and 299), Stage 1C adds the qualified readings above.
- **FACT** (count-only, within the bounded patterns of Stage 1B: Q36 with T10, \b(FROM|JOIN)\s+signals\b, and Q37 with T11, \b(FROM|JOIN)\s+[a-z_][a-z0-9_]*): strategy_ranking.py, strategy_health.py, analytics_period.py and analytics2.py have no T10 or T11 line; scanner_attribution.py has no T10 line and one T11 line, 542, a comment, not a query (O-2). The patterns do not count lower-case SQL, split lines or run-time table names (Stage 1B, clarification 5).
- **UNKNOWN / NOT ESTABLISHED:** where the rows of these five files come from. A bounded zero count does not establish a data lineage.
- **OQ-1B-06:** whether the 9 runtime files that name the signals table without T6 or T7 lines, and the 6 such leads among the 31, read the scanner or strategy columns (unquoted column names are not counted by T6 or T7).
- **INTERPRETATION** (Stage 1, answer F; HISTORICAL C-001 7.2): a view that attributes by signals.strategy attributes by the stored scanner key, which equals the strategy name only while each key equals its strategy.

## 12. Paper/live and mode differences

- **FACT** (reads 052, 046, 053): create_trade, printed in full (175 to 271), contains no if statement. Its argument tuple holds strategy (247) and mode (254), bound by position to the strategy and mode columns (215, 222). The mode parameter carries the comment "v14: PAPER | LIVE" (`orders/order_manager.py:191`, read 052): comment text only, not evidence of the values passed.
- **INTERPRETATION:** with no branch in create_trade, the strategy value it writes does not depend on the mode value.
- **FACT** (read 055): the placer passes mode=self._mode (`orders/order_placer.py:1010`). The value of self._mode is NOT ESTABLISHED by C-010.
- **FACT:** neither Step 2 (`signals/signal_processor.py:930-953`, read 001) nor the receiver's INSERT (`signals/webhook_receiver.py:963-980`, read 004) branches on a mode in the lines read, and the signals INSERT has no mode column.
- **UNKNOWN / NOT ESTABLISHED:** paper/live parity of the mapping chain beyond these lines. None of the frozen-manual passages cited by the evidence sheets records a paper/live difference for this chain. Paper mode is legacy context.
- No mode is inferred for E4 (section 9.2).

## 13. Known defects and ambiguities (as already recorded; none new)

| Recorded as | Finding | Label (as recorded) | C-010 evidence |
|---|---|---|---|
| C-001 D-05; BK-27 | signals.strategy stores the scanner key; consumers read it as the strategy | KNOWN DEFECT / AMBIGUITY (latent) | the INSERT re-verified at the source line (read 004: scanner_name into both columns; section 6.3); BK-27 OPEN |
| C-001 D-01 | the S10 map-to-strategy cross-check is run by no production caller (A-008 erratum E-1; A-009) | KNOWN DEFECT / AMBIGUITY | FACT: it runs only when a map path is passed (`strategies/loader.py:151-153`, read 005; section 5.3) |
| C-008 EP-02 | no runtime file calls EntryGate.add(); E2 runs only for a reloaded gate_state row released with PRICE_HIT | KNOWN DEFECT | the E2 lines held (section 9.2) |

No new KNOWN DEFECT is established by C-010. Its findings are source facts, interpretations and unknowns at the pin, not demonstrated runtime defects.

## 14. Open questions and bookmarks

### 14.1 Open questions (kept open; none closed here)

OQ-1D-01 is one recorded question; the Stage 1E sheet (section 7) records it in three rows, OQ-1D-01 (E2), (E3) and (E4). This manual keeps the parent ID and labels the three sub-items OQ-1D-01a, OQ-1D-01b and OQ-1D-01c; the sub-labels are this manual's, and the same labels are used in sections 9, 14, 15, 16 and 19.

| ID | Question | Status after Stage 1E | Why it stays open |
|---|---|---|---|
| OQ-1D-01 (parent) | Where the strategy names passed on the gate, retest and allocator paths come from (O-7; Stage 1D reconciliation, item 3). | OPEN, in three sub-items; none closed | as each sub-item |
| OQ-1D-01a (E2 gate) | The source of entry.strategy_name (2051) on the gate path. | UNKNOWN / NOT ESTABLISHED | D14; C-008 EP-02 (KNOWN DEFECT) |
| OQ-1D-01b (E3 retest) | The source of parked.strategy (2387) on the retest path. | NOT ESTABLISHED beyond the processor-side lines (1046, 2387 to 2409, 2557) | D14: several module and storage hops |
| OQ-1D-01c (E4 allocator) | The allocator hand-off between the candidate built at 1619 to 1640 and candidate.strategy_name at 1656. | NOT ESTABLISHED; no mode or execution inferred | D15 declined |
| OQ-1D-02 | The second create_trade( line in order_manager.py. | ANSWERED: line 12, a string line before the first definition (INTERPRETATION); its use beyond its text NOT ESTABLISHED | D13 (section 10) |
| OQ-1B-05 | What trades.strategy holds. | E1: the resolved strategy name (INTERPRETATION, section 8); E2 to E4: as section 9.3 | What would make E1 FACT is unchanged: the first lines of the calls holding signal_processor.py 1046, 1184 and 1373; the definitions between signal_processor.py 867 and 1455; the definitions between order_placer.py 871 and 996; the type of self._om. No read is proposed. |
| OQ-1C-01 | What scripts/t2_cnc_gtt_realtest.py writes to trades (line 234 and after; the lead). | OPEN; not inspected | out of scope (the manual-draft prompt: reconciliation item 5 and its DO NOT list) |
| OQ-1B-04 | Whether any decision reads _evidence_strategy_for, and whether anything outside signal_processor.py writes self._scan_webhook_map. | NOT ESTABLISHED (D9 declined) | callers and other files not read; the docstring is not proof |
| OQ-1B-03 | How registry reaches the entries at scanner_attribution.py 298 to 299 (lines 294 to 297). | OPEN (narrowed by Stage 1C) | a read outside the accepted rules |
| OQ-1B-02 | fetch_signals's return line (between 46 and 54) and replay_one_signal's parameter list (from 66). | OPEN (narrowed by Stage 1C) | a read outside the accepted rules |
| OQ-1B-01 | The source of the 57 consumer lines of Stage 1B section 7. | OPEN | reads outside rule P |
| OQ-1B-06 | Whether the 9 runtime files that name the signals table without T6 or T7 lines, and the 6 such leads among the 31, read the scanner or strategy columns. | OPEN | unquoted column names are not counted by T6 or T7 |
| OQ-1B-07 | Carried from Stage 1: U-01 (the route and the 404), U-02 (the queue hand-over), U-03 (the enclosing functions of reads 004 to 006), U-04 (the status written for UNKNOWN_STRATEGY), U-06 (the test's assertions), U-10 (the map's key-strategy equality), U-11 (later writers of signals.strategy) and U-12 (the deleted hosts). | OPEN | as recorded in the Stage 1B sheet, section 9 |

### 14.2 Existing bookmarks (none closed)

- **BK-27** ("strategy column stores the scanner name": ChatGPT's Stage 0 answer sheet; C-001 D-05, re-verified by C-007 2.1; its owners include K-003: Stage 0 answers, OQ-04): OPEN. Proposed note for the freeze: C-010 re-verified the INSERT at the source line (`signals/webhook_receiver.py:975`, read 004). On E1, trades.strategy is supplied with the resolved strategy name (INTERPRETATION, section 8), so wherever a key differs from its strategy the signals row and the trades row would carry different identities (INTERPRETATION). The dashboard's per-strategy views read signals.strategy and trades.strategy separately, and one view merges them (section 11.4).
- **VP-45** (which names C-010 as a ratification point: Stage 0 answers, OQ-03): NOT RATIFIED. This manual makes no V2 decision and does not present VP-45 as settled.

### 14.3 Proposed new bookmarks (for ChatGPT's review; BK numbers would be assigned at the freeze)

- **PB-1:** C-010 OQ-1D-01 (sub-items OQ-1D-01a to OQ-1D-01c), OPEN QUESTION: the sources of the strategy name on E2 (entry.strategy_name, 2051), E3 (parked.strategy, 2387) and E4 (candidate.strategy_name after the allocator hand-off, 1656), not established and not read (D14, D15; section 9). Owners: for ChatGPT to assign at the freeze.
- **PB-2:** C-010 OQ-1B-01, OQ-1B-03 and OQ-1B-06, OPEN QUESTIONS: the sources of the dashboard's strategy and scanner fields not shown under rule P, screen 21's registry-to-entries link, and the runtime files not grepped (section 11.6). Owner: for ChatGPT to assign at the freeze.
- **PB-3:** C-010 OQ-1C-01, OPEN QUESTION: what scripts/t2_cnc_gtt_realtest.py writes to trades (line 234 and after), a lead not inspected (section 8.3). Owner: for ChatGPT to assign at the freeze.

## 15. Deviation, gap and record-correction log (kept as recorded; nothing overwritten)

| ID | Stage | Kind | Record | Treatment |
|---|---|---|---|---|
| D-1 | 1 | process deviation | Rule O8 took any printed line containing the word "signals" as showing use of the signals table. At db_reader.py line 3994 the word is a dictionary key, so read 010 (lines 3989 to 3999, 11 lines) was made although the authorisation's condition was not met. | Read 010 stays recorded and is not relied on. Lesson: a mechanical trigger must test the condition itself and be tried before the window on a likely false positive as well as a true case. |
| G-1 | 1 | register gap | The Stage 0 register left out analytics_period.py, analytics2.py and config_reader.py (cited by C-001 for this topic) and four runtime files naming the map. | Closed in Stage 1B: R25 to R31 added with their blobs; R28 and R29 classified; R30 and R31 not read (D3). |
| O-1 | 1B | observation | db_reader.py's T11 count (184) was above 60, so G3 used T10 and listed only signals queries; reads 038 to 040 also showed queries on trades or orders between that query and some consumer lines. | Those lines are reported with the source their read shows. |
| O-2 | 1B | observation | scanner_attribution.py line 542 is a comment matched by T11 on "JOIN trades"; it is not a query and caused no read. | The six consumer lines below it have no SQL above them. |
| O-3 | 1B | observation | The driver's results file records only the first table named on a G3 line (line 1662 names trades and signals). | Rule P used the whole line; the Stage 1B sheet shows every table named. |
| O-4 | 1B | observation | Line 4010 is one line past the 30-line bound and was not read under rule P; it is in the same loop as 4008. | Its source is INTERPRETATION only (section 11.4). |
| O-5 | 1B | observation | The consumer-line pattern also selects filter and request arguments and map entries. | Listed with their outcome; no table is claimed for them. |
| D-2 | 1C | process deviation | The Stage 1C synthetic tests did not run three named cases as worded (a non-identifier name; a T21 count of 0 or of exactly 9). | No result depends on them (the real run had identifier names and a count of 4). Lesson: tick every named test case against a named check before the window. From Stage 1D, every named TC-ID had to print ok by its own ID. |
| O-6 | 1C | control and scope-definition inconsistency | G6's dynamic rule reached scripts/t2_cnc_gtt_realtest.py, which was outside the gate test's seven-file list; the reader's own gate admitted it, and one line was printed and recorded as a lead. | Kept visible; from Stage 1D, one approved read list was shared by the gate test and the driver, and the driver refused any path outside it before reading. |
| R-1 | 1C | record correction | The Stage 1C proposal's header gave a preparation time of 17:40 IST; the file was last written at 17:30:41 IST, and Claude's notes had 17:45. | Only the stated time was wrong; the content is unchanged, verified by SHA-256. |
| R-2 | 1C | description correction | The Stage 1 and 1B sheets called the reader's mask counts whole-file counts; they cover the whole file for a grep and the range for a range read. | No count, mask or printed line changes. |
| O-7 | 1D | observation | Read 057, printed for the E1 chain, also shows the strategy names that the gate, retest and allocator paths pass. | Recorded as lines only (OQ-1D-01; its sub-items OQ-1D-01a to OQ-1D-01c, section 14.1); section 9. |
| O-8 | 1D | observation | Q47 counted 2 create_trade( lines in order_manager.py; only the definition at 175 was known. | Answered in Stage 1E (section 10). |
| R-3 | 1E | clerical / process deviation | The Stage 1E run log's synthetic-test row gives "19:21 IST", a time typed by hand; the test output was written at 19:22:46 IST, after the decision file (19:22:29 IST) and before the unlock (19:23:44 IST). The row was left as written, and a correction row was added at 19:25:08 IST. In the same log, the E-33 and E-36 rows repeat their step ID (cosmetic). Claude's working notes gave 19:20 IST for the tests against the proposal, which ran from 19:13:54 to 19:14:45 IST; a correction row was added to the notes at 19:37:17 IST. | The event order is unaffected. The original record and the corrections are preserved, not normalised. Lesson: take every time in a log or a note from the clock or a file time, never by typing it. |
| Register keys | 1D, 1E | clerical | The repeated register IDs R13, R14, R17, R19, R20, R23 and R24 (19 rows under 7 IDs). | UNRESOLVED clerical collision; kept as recorded, not renumbered; a freeze-time key map is proposed (section 3). |

## 16. Evidence ledger

The IDs below are this manual's own. "Stage 1 U-NN" elsewhere in the manual refers to the Stage 1 sheet's unknowns, carried in OQ-1B-07.

| ID | Classification | Evidence / conclusion |
|---|---|---|
| F-01 | FACT | The receiver's INSERT writes scanner_name into both signals.scanner and signals.strategy, status QUEUED (`signals/webhook_receiver.py:963-980`; read 004). |
| F-02 | FACT | Step 2 looks the map up by scanner_name, takes the entry's strategy, then the loaded strategy by that name; each failure raises UNKNOWN_STRATEGY (`signals/signal_processor.py:930-953`; read 001). |
| F-03 | FACT | The loader registers strategies[cfg.name] = cfg; the S10 cross-check runs only with a map path (`strategies/loader.py:149, 151-153`; read 005). |
| F-04 | FACT | main.py passes the map at 2458 and its scanners at 3585 (read 003); the processor keeps dict(scan_webhook_map) at 208, the file's only assignment to it within T1's reach (read 002). |
| F-05 | FACT | The processor test maps gap_go_short to gap_go_short_v1 and writes its own signals row with the mapped name (`tests/unit/test_signal_processor.py:1933-1964`; read 006). |
| F-06 | FACT | The evidence helper resolves as Step 2 does and returns None instead of rejecting (`signals/signal_processor.py:557-580`; read 031); its docstring's claims are text only. |
| F-07 | FACT | Within T3's reach: line 939 is the only assignment statement to strategy_name before 1184; def _admit_and_place takes strategy_name as a keyword-only parameter (1277 to 1278); there is none between 1278 and 1455; strategy=strategy_name at 1455, in the place call opened at 1446 (reads 057, 058, 043). Function membership: INTERPRETATION (H2, H5). |
| F-08 | FACT | orders/order_placer.py: the parameter strategy at 882; within T25's reach, no other binding of strategy between 882 and 1000; trade_id = self._om.create_trade( at 996 with strategy=strategy (1000) and mode=self._mode (1010) (reads 054, 055, 056). That these lines lie in OrderPlacer.place: INTERPRETATION (H7, H9). |
| F-09 | FACT | create_trade (175 to 271): keyword-only strategy (181); INSERT INTO trades (214 to 244); its 5th value is strategy (247); return trade_id (270) (reads 051, 052, 046, 053). |
| F-10 | FACT | Counts, within their terms: Q47 found 3 T23 lines in s_rt (order_manager.py 12 and 175; order_placer.py 996), and Q48 one definition line, in order_manager.py (reads 051, 054, 059). That 996 is the only call line: INTERPRETATION (I-02). |
| F-11 | FACT | Line 12 of order_manager.py, as printed (read 059; section 10). |
| F-12 | FACT | The E2, E3 and E4 strategy-name lines of section 9 (reads 057, 058). |
| F-13 | FACT | db_reader.py's queries on signals.strategy, signals.scanner and trades.strategy, and the merged view's code (reads 034 to 042); the variable bindings and line 4010 (read 009) as labelled in sections 11.1 to 11.4. |
| F-14 | FACT | The registry officer reads DISTINCT scanner and strategy from signals into one set (read 032). |
| F-15 | FACT | The replay tool: the signals query (46), the skip of a strategy that is not loaded (91 to 95), the calls at 191 and 196 (reads 027, 033, 049, 050). |
| F-16 | FACT | config_reader.py's get_scan_webhook_map (92) and get_scanner_registry (249) and the lines printed after them; the calls of get_scan_webhook_map in strategy_tower.py (96), analytics_period.py (504) and config_view.py (138); scanner_attribution.py's call of get_scanner_registry (293) (reads 020, 021, 023, 024, 026, 047, 048). Which body lines belong to which function, and the link to screen 21: INTERPRETATION and HISTORICAL (section 11.5). |
| F-17 | FACT | Count-only, within T14's reach (\bINTO\s+trades\b over s_rt's 281 files; it does not see run-time or f-string table names, lower-case SQL or a statement split before the table name): INSERT INTO trades only in order_manager.py 214 and the lead's 234 (Q38; reads 044, 045; section 8.3). |
| H-01 | HISTORICAL | Each of the 16 keys equals its strategy and its webhook path; the schema allows N:1 (C-001 4.3). |
| H-02 | HISTORICAL | The route and the 404 for an unknown key (C-001 4.3; C-002 7.1, 7.2, 9). |
| H-03 | HISTORICAL | No runtime writer updates signals.strategy after the insert (C-001 7.2). |
| H-04 | HISTORICAL | A failed lookup is stored as REJECTED_UNKNOWN_STRATEGY (C-008 ST-08). |
| H-05 | HISTORICAL | place() is OrderPlacer.place, beginning at order_placer.py line 871; C-008 4.1 (ST-29) cites lines 871 to 958 for it, a range that does not reach the create_trade call at 996; three callers of place() (B-009 7). |
| H-06 | HISTORICAL | E1 is the one live entry path wired at the pin (C-008 18). |
| H-07 | HISTORICAL | E2 runs only for reloaded gate rows; EntryGate.add() has no runtime caller (C-008 EP-02, KNOWN DEFECT). |
| H-08 | HISTORICAL | With the repository configuration, E3 has no caller (C-008 EP-05). |
| H-09 | HISTORICAL | The S10 cross-check is run by no production caller (C-001 5.3, D-01). |
| H-10 | HISTORICAL | trades.strategy is among the consumers of the strategy YAML's name field (A-008 21). |
| H-11 | HISTORICAL | cfg.name is the YAML name field; files load in sorted order; a later duplicate name replaces the earlier one (A-008 4; B-002 M22). |
| H-12 | HISTORICAL | The dashboard reads the map on each call from its own config_dir; which file production read is UNKNOWN (C-001 5.1, A-L8). |
| I-01 | INTERPRETATION | On E1, trades.strategy is supplied with the resolved strategy name, not the scanner key (section 8.2). |
| I-02 | INTERPRETATION | Line 12 is a string line, not a call; within s_rt and T23's reach, order_placer.py 996 is the only create_trade( call line (section 10). |
| I-03 | INTERPRETATION | With a key different from its strategy, signals.strategy and trades.strategy would differ, and the merged view would split them into two slots (sections 4.2, 11.4). |
| I-04 | INTERPRETATION | On E2 and E3 a name passed to place() has passed the loaded-strategies lookup; if a trade were placed there, trades.strategy would take that path's strategy_name (section 9.3). |
| I-05 | INTERPRETATION | Step 2's dict branch serves plain-dict maps and its attribute branch schema objects (section 7.3). |
| I-06 | INTERPRETATION | Screen 21's entries are get_scanner_registry's items; the replay's consumer lines read fetch_signals's items (section 11). |
| K-01 | KNOWN DEFECT / AMBIGUITY (latent) | C-001 D-05 = BK-27: signals.strategy stores the scanner key; OPEN (section 6.3). |
| K-02 | KNOWN DEFECT / AMBIGUITY | C-001 D-01: the S10 cross-check is run by no production caller (section 5.3). |
| K-03 | KNOWN DEFECT | C-008 EP-02: no runtime caller of EntryGate.add() (section 9.2). |
| UN-01 | UNKNOWN / NOT ESTABLISHED | The sources of the strategy name on E2 (OQ-1D-01a), on E3 beyond the processor side (OQ-1D-01b) and on E4 across the allocator hand-off (OQ-1D-01c). |
| UN-02 | UNKNOWN / NOT ESTABLISHED | Whether any code updates trades.strategy after the insert. |
| UN-03 | UNKNOWN / NOT ESTABLISHED | What the lead script writes to trades (OQ-1C-01). |
| UN-04 | UNKNOWN / NOT ESTABLISHED | The sources of 57 consumer lines; the readers not grepped (OQ-1B-01, OQ-1B-06). |
| UN-05 | UNKNOWN / NOT ESTABLISHED | The Stage 1 items carried in OQ-1B-07 (U-01 to U-04, U-06, U-10 to U-12 of the Stage 1 sheet). |
| UN-06 | UNKNOWN / NOT ESTABLISHED | Paper/live parity of the chain beyond the lines read (section 12). |
| UN-07 | UNKNOWN / NOT ESTABLISHED | What any deleted host ran, and what any stored signals or trades row holds. |

## 17. Cross-references

Only the frozen manuals and sections that the C-010 evidence sheets cite:

- **C-001:** 4.2 to 4.4 (the 16 entries; key = strategy = path; N:1), 5.1 (loading; the dashboard's own read; A-L8), 5.2 (the schema; a mapped strategy with no YAML), 5.3 (D-01), 7.2 (D-05 = BK-27; the readers of signals.strategy; no test of the production insert), 7.4 (Step 2), 8 (screen 21 and the attribution endpoints) and 12 (T-8, T-9, T-14).
- **C-007:** 1.1 (the same blobs for order_manager.py and the lead script), 2.1 (BK-27 re-verified) and 4.1 (the lead script).
- **C-008:** 2.1 (W-01, W-02, W-04), 2.2 (E1 to E4), 4.1 (ST-08, ST-09, ST-15, ST-19, ST-29; with 8 SS-02 and 17), 9 (EP-01, EP-02, EP-04, EP-05, EP-07, EP-08), 10 (OB-02), 18, and the chapter 02/03 cross-check row CH-09 (C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md: chapter 02 line 952 lists "scanner->strategy lookup ... scan_webhook_map" in the pipeline, CONSISTENT with C-008 4.1; cited as that existing cross-check, not fresh verification; OQ-05).
- **B-009:** 7 (the three callers of place()).
- Also cited by the evidence sheets, and kept only where they cite them: A-008 4, 21 and erratum E-1; A-009; B-002 M22; C-002 7.1, 7.2, 8.3 and 9.
- Owners of neighbouring steps: C-002 to C-004, C-007 to C-009, C-011, A-008 and K-003 (section 1).

## 18. Draft status and points for ChatGPT's review

- Revision 0 (returned for review at 20:49 IST), drafted by Claude from the evidence set of section 2.3 under the manual-draft prompt.
- Revision 1, under C-010_CHATGPT_MANUAL_REVIEW_NOTES_24-OCT-2026.txt: section 11.6 splits the bounded count (FACT within its patterns) from the rows' origin (NOT ESTABLISHED); OQ-1D-01 is a parent with the sub-items OQ-1D-01a to OQ-1D-01c, used alike in sections 9, 14, 15, 16 and 19; the register's repeated IDs are marked as an unresolved clerical collision with a proposed freeze-time key map (section 3); the masked-value counts in section 19 are tied to their whole-file greps, and main.py's missing count in the Stage 1E list is described as a record discrepancy; ledger rows F-04, F-07, F-08, F-10, F-13 and F-16 and one sentence of section 11.5 now state their terms' reach or their INTERPRETATION parts. No finding, label or open question was changed otherwise.
- For neither revision was any source read, searched or counted, and neither repository was changed.
- Points kept for the review, not resolved here:
  1. The repeated register IDs, an UNRESOLVED clerical collision, and three recorded role texts that are narrower than the evidence (section 3): kept as recorded. The freeze-time action proposed in section 3, a key map beside the unchanged register, is for ChatGPT to confirm.
  2. The cross-references (section 17) are limited to what the evidence sheets cite. The sheets also cite A-008, A-009, B-002 and C-002, which are kept where cited and not extended. Sections the sheets do not cite are not used: for example C-001 section 10 (paper and live), so section 12 rests on C-010's own lines.
  3. The proposed bookmarks PB-1 to PB-3 and the BK-27 note (section 14), for ChatGPT to keep, change or drop.
  4. The file name follows the manual-draft prompt (C-010_SCANNER_TO_STRATEGY_MAPPING.md); the series' earlier manuals are stored as C-0NN_manual.md in docs/agenda/c-0NN_<topic>/. The stored name and path are for the freeze package.
- At the freeze (after ChatGPT's review; governance context, not evidence): the status lines, a decision-register row, the evidence set stored unchanged with ChatGPT's review notes for revision 1, the register's key map if confirmed, the BK-27 note and any accepted bookmarks, and the design-log lessons D-1, D-2, R-2 and R-3.
- Frozen 2026-10-24 (DR-063) under ChatGPT's final review (C-010_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt), whose package actions are applied in the freeze package and settle the review points above: A, the key map of section 3 confirmed and stored beside this manual as C-010_REGISTER_KEY_MAP.md, the register itself unchanged; B, PB-1 to PB-3 stored as bookmarks BK-48 to BK-50 in V2_DESIGN_LOG section 7, OPEN, their owners not assigned; C, BK-27 OPEN and VP-45 NOT RATIFIED, unchanged; D, revision 1, the final review, the evidence set of section 2.3 and the key map stored together, with both completion notes as provenance; E, the Stage 1E record discrepancy (section 19) and the lessons D-1, D-2, R-2 and R-3 recorded in V2_DESIGN_LOG section 1, the records themselves unchanged. The manual is stored as C-010_manual.md (W-3); the course of the topic and the package are in C-010_review_notes.md.

## 19. Remaining unknowns and final statement

**Remaining unknowns** (sections 9, 11.6 and 14):

- the sources of the strategy name on E2 (OQ-1D-01a: UNKNOWN / NOT ESTABLISHED), on E3 beyond the processor-side lines (OQ-1D-01b: NOT ESTABLISHED) and on E4 across the allocator hand-off (OQ-1D-01c: NOT ESTABLISHED), with no mode or execution inferred for E4;
- whether any code updates trades.strategy after the insert, and what scripts/t2_cnc_gtt_realtest.py writes to trades (OQ-1C-01);
- the sources of the 57 consumer lines, screen 21's registry-to-entries link, the replay tool's return and parameter binding, and the readers not grepped (OQ-1B-01 to OQ-1B-03, OQ-1B-06);
- the callers of _evidence_strategy_for, and any writer of the processor's map copy outside its file (OQ-1B-04);
- the Stage 1 items carried in OQ-1B-07: the route and the 404, the queue hand-over, three enclosing functions, the stored status for UNKNOWN_STRATEGY, the test's assertions, the map's key-strategy equality, later writers of signals.strategy, and the deleted hosts;
- paper/live parity of the chain beyond the lines read;
- the masked KV-shaped values: counted only, never examined, and not to be identified or inspected; no value is inferred to be a credential, and whether any is one is NOT ESTABLISHED and outside C-010. Each count is the reader's mask count for a whole-file grep (R-2), as recorded in the read log and the evidence sheets: main.py, 10 (read 003, the grep of T1; Stage 1 sheet); scripts/t2_cnc_gtt_realtest.py, 1 (read 045, the grep of T14; Stage 1C sheet); orders/order_placer.py, 4 (reads 054 and 056, the greps of T23 and T25; Stage 1D sheet), the same whole-file count twice, not 8. **Record discrepancy, not a new finding:** the Stage 1E sheet's security record (its section 7) lists only the last two counts; the main.py count has been in the Stage 1 sheet since read 003, and this manual adds it to the list without examining any value;
- BK-27 stays OPEN, and VP-45 stays NOT RATIFIED.

**Final statement.** At the pin, the receiver's INSERT writes its scanner_name into both signals.scanner and signals.strategy (FACT), and that scanner_name is the webhook path key (HISTORICAL, C-001 4.3; BK-27, OPEN). The processor's Step 2 looks its scanner_name up in its copy of the scanner map, takes the entry's strategy name and then the loaded strategy of that name, rejecting UNKNOWN_STRATEGY at any of three failures (FACT); that this scanner_name is the receiver's, handed over on the queue, is HISTORICAL (C-001 12, T-9). On the main entry path, E1, the resolved name is passed through _admit_and_place, OrderPlacer.place and OrderManager.create_trade to the 5th bound value of the INSERT INTO trades, so trades.strategy is supplied with the strategy name, not the scanner key (INTERPRETATION under the chain rule). The other create_trade( line in order_manager.py is text inside a string, not a call (INTERPRETATION). On the gate, retest and allocator paths, where the strategy name comes from stays UNKNOWN / NOT ESTABLISHED as decided (OQ-1D-01a to OQ-1D-01c). Because every key equals its strategy at the pin (HISTORICAL), these identities coincide there; they would differ for a renamed or N:1 mapping.

This manual is a historical description of the pinned legacy source (trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb), read through bounded, masked evidence. It is not evidence of actual runtime signals, trades, orders or database rows: no stored row, deployed host, log or trading day was examined, and none is claimed.

**Legacy remains DEAD / REFERENCE ONLY. No V2 decision is made here.**
