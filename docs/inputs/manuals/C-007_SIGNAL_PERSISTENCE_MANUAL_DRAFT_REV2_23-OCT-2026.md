# C-007 - SIGNAL PERSISTENCE: LEGACY MANUAL

Status: DRAFT, revision 2 (Claude's revision 0 of 23-Oct-2026 with the corrections from ChatGPT's two review notes; for ChatGPT/Rama review). NOT FROZEN: nothing is committed, pushed or saved to the Project until the review and Rama's explicit approval (W-4, DR-017).  
Topic: C-007 Signal persistence (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 23-Oct-2026 through the gated masking filter.  
Built from: C-007_CLAUDE_REPOSITORY_ANSWERS_23-OCT-2026.txt (the answers, SHA-256 E8C2C84A0EB719A050FB935CA57293E1421D18B003C0ED871844E3904DD136E5); C-007_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (the inventory, SHA-256 3C8F097AF2D16D2B21F17DCFFCFFA53928509EF1B6D59198B8DDF878FA357AAF); C-007_CLAUDE_STAGE0_ANSWERS_23-OCT-2026.txt (the Stage 0 answers, SHA-256 AB91992A6590AD3C1B479E4BE3532595CB13451A9030B327CCE8D2AF67356C16); C-007_CLAUDE_STAGE0_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (the Stage 0 inventory, SHA-256 2403AD4233CA8D247F5E44796367B139BC57379A580C46318105D072CFC0D1CE); C-007_STAGE1_RECONCILIATION_TABLE_23-OCT-2026.txt (Claude's reconciliation table, SHA-256 257EB2A1438A9B79E1DA0EBFB87EA95A09C4AB29BA1F2361CAA1DF49B52CF0F2); C-007_STAGE1_AUTHORIZATION_AND_REVIEW_NOTES_23-OCT-2026.txt (ChatGPT's Stage 1 authorisation, SHA-256 889FB22830CB004C84DEC5861BAAC51508C930C8E6FDAAC5DFEFE3A118C5B8B0); C-007_STAGE1_CHATGPT_RECONCILIATION_REVIEW_23-OCT-2026.txt (ChatGPT's reconciliation review, SHA-256 02142104EFEBB60E9E9055DE24B5E66F7FC04BC924A361572972D250C8008B20); C-007_CHATGPT_SPOTCHECK_AND_REVIEW_23-OCT-2026.txt (ChatGPT's spot-check review, SHA-256 0BBBBB786403B438CFB6B4C8850556FD487A1A4EA796F7BCBEE7D5C09450D94D); C-007_CHATGPT_REVIEW_NOTES_23-OCT-2026.txt (ChatGPT's review notes on revision 0, SHA-256 521811D71ED2C9183AFD5E7BB86A311FC43A3558B123B49579087B97B20ACD5C); C-007_REV1_FINAL_REVIEW_NOTE_23-OCT-2026.txt (ChatGPT's final review note on revision 1, SHA-256 E21F6C8929FFC8EB375C3EA9103BD59644CF271B46BC21A50F8862FFBD2E511A); and Rama's answers of 23-Oct-2026 (section 1.2).  
Purpose: factual record of how the legacy system stored signals: the signals table and its history, the stored statuses and their writers, the storage paths, what start, stop, kill and repair paths write, retention, and the statuses the next consumers expect. It is not a V2 design or a recommendation to revive the legacy system. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1); V2 considerations will be in C-007_v2_design_inputs.md (PROPOSED / OPEN), prepared with the freeze package.  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox, SSH detail, or person name other than Rama's is reproduced.

## 1. Evidence rules and boundaries

This chapter is based on Claude's Stage 1 repository answers and source inventory dated 23-Oct-2026: 120 gated reads of 37 files and 8 count-only Stage 1 searches (P38 to P45) against the pinned commit, with the Stage 0 count-only searches P01 to P37, P06b and P29b. Every read matched its pinned blob and ran after the gate's self-test, and the protected-path check was zero before every read and at delivery (the inventory, sections 2, 3 and 7). Bracketed IDs such as (Q1-b), (S-05) or (RT-02) are the answers' claim IDs; the inventory's claim index (section 6) maps each one to the logged reads that printed its lines.

Evidence labels used here:

- **FACT (SOURCE-VERIFIED):** supported by source lines read at the pinned commit.
- **FACT (CONFIG):** a value in the repository's configuration files, not evidence of a deployed host's effective configuration.
- **FACT (TEST TEXT):** what test or crash-test tool source states; never run; not runtime evidence.
- **FACT (TEXT DRIFT):** a comment or docstring that differs from executable code or configuration.
- **INTERPRETATION:** a possible consequence of a source path ("the inspected code path would ..."), not proof that it occurred.
- **HISTORICAL:** a statement in a previously frozen manual; HISTORICAL / RECORD-DERIVED: a figure or event stated in a code comment, not verified here.
- **HISTORICAL (RAMA'S STATEMENT):** Rama's answer during this topic's review (section 1.2), not checked against records.
- **KNOWN DEFECT:** a defect previously recorded in a frozen manual and re-verified at the pin, to the extent stated.
- **UNKNOWN / NOT ESTABLISHED** and **OPEN QUESTION:** the evidence did not establish the answer.

**Scope (proposal S-1, as authorised):** (1) the `signals` table: columns, keys, defaults, indexes, constraints and triggers, and only its migration history; (2) the stored status vocabulary and transitions: status, writer, condition, reason column, and whether a write checks the row's current status; (3) the storage-layer insert, update, read and delete paths for `signals` rows, with transaction boundaries as they touch those rows; (4) what start, stop, kill and repair paths write to `signals` rows; (5) retention of `signals` rows and their fingerprint key; (6) the statuses the immediate downstream readers expect. The other tables of `core/schema.sql` appear by name and owner only.

**Owners elsewhere:** B-003 (database files, opening, the migration mechanism, other tables); C-005 (claims, the cache, the fingerprint and its dedup meaning); C-006 (the queue); C-008 (why the processor writes each status); C-009 (expiry policy); C-010 (scanner-to-strategy mapping); C-011 (the EOD route); Section K (database architecture, the StateStore, signal state, retention at large, recovery from the database).

Records R43 to R47 were names only; the two excluded July records (R48) were not opened or searched. No legacy code, test or script was run; no live service, VM, database, broker, webhook or Chartink was contacted; no legacy file was changed; nothing was committed or pushed.

### 1.1 Sources at the pin

| File | Pinned blob | Lines | Role in this chapter |
|---|---|---|---|
| `core/schema.sql` | `b46d7489f6ec5e31149dd68957f3b9e45270571e` | 1,655 | the signals table: columns, CHECK, indexes, references, version stamp and history comment |
| `core/migrations.py` | `cc39e811e48a9eec823c20b70d343278a6b33eef` | 445 | the migration map and the signals rebuild steps |
| `core/state_store.py` | `11866117801b9329773a1be605448faef3c96f8d` | 3,356 | the store's signals functions: status updates, release and clear functions, readers |
| `signals/webhook_receiver.py` | `0f15a0ab033529d80d9c14c993a5077690b72871` | 1,208 | the insert, the two status flips, the per-symbol refusals and response statuses |
| `core/ids.py` | `5e8b17bdd97f7ed37d7b4229e18353cb62ecd2ff` | 125 | the signal_id format |
| `signals/signal_processor.py` | `7fab0a02a302770ac72bbbd662549d5c091086c1` | 2,704 | the processor's status writes and reject names |
| `screening/entry_gate.py` | `46b173171bdfeba48af641b67a361618e5acba58` | 538 | GATE_WAITING and GATE_RELEASED_*; the gate reload and clear |
| `screening/retest_monitor.py` | `26639d6ed5dddb1140f0868bb9f937cad5379f82` | 442 | RETEST_* and REJECTED_RETEST_DUP; the retest reload and clear |
| `screening/secondary_screener.py` | `b6f8ff7223ad803869e976f92d794dfc7c811b59` | 706 | the screening verdict statuses |
| `orders/order_manager.py` | `978d213532532d7db72b215bc8ed415b848928ae` | 770 | the trade_id link; a received_at read |
| `orders/order_placer.py` | `e3a602909befa3fd3ba4b27caeed108724c07947` | 4,707 | the caller of the trade_id link |
| `scripts/t2_cnc_gtt_realtest.py` | `4034ecd18b2d6c5ccf84419316db964a533eb7f1` | 607 | a non-test signals INSERT (one grep) |
| `main.py` | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | 4,294 | startup-time search for signal-row writes and reload paths (one grep); no signal-row writer found in the inspected scope |
| `capital/kill_switch.py` | `3eb2ece050c3eafbcf2e90ec01c824b4dff27950` | 2,152 | kill-time search for signal-row writes (one grep); no signal-row writer found in the inspected scope |
| `scripts/system_manager.py` | `04e776e60f988f47f11753c42577ffd0c428adaf` | 1,356 | the stuck-signal readiness check and counts |
| `scripts/check_vm_state.py` | `b9f362ae378b00cc968a1c09145b0843bba31d28` | 203 | signals reads (one grep) |
| `utils/startup_checks.py` | `61ff67676d8230640a2aafb6bd6d75d7a185fbb6` | 1,693 | start checks (one grep; no matching line) |
| `scripts/eod_cleanup.py` | `ab658108e443b7121d886f98b94c5058c42253b0` | 325 | the expiry and prune steps (signal-row steps only) |
| `config/system_config.yaml` | `bf742ae5c81c60614370361998e6a102a1064ff7` | 754 | signal_retention_days (repository value) |
| `core/config_loader.py` | `8635bca77531515561ee48cf92677883a5e30782` | 2,517 | the retention model and its default |
| `config/cron_registry.yaml` | `42ff638145ba8c05eba907a33d5b391ae3fc6910` | 768 | the eod_cleanup and db_retention job entries |
| `deploy/cron/trading-system.cron` | `f8399b40bfa71e35e27746e46cd463171615477a` | 152 | the same job lines (one grep; cited by line number only) |
| `scripts/db_retention.py` | `3de2dded7d813a3848dce555941f15ba5ac333f4` | 231 | the retention policy (signals not listed) |
| `reports/signal_status.py` | `98a1631f5634807ecc6cf46cc9d3767215f9871f` | 157 | the status classification (read in full) |
| `capital/shadow_engine.py` | `7d19b400f9022a392b6c876f445a3a952bc6d749` | 272 | one TRADED default (one grep) |
| `scripts/generate_screened_stocks_csv.py` | `3e6b5896bbc33a5909cbc76b719752bc8d4b9746` | 358 | DROPPED_ labels (one grep) |
| `reports/daily_trade_review.py` | `c448b7976964a14caf0061016c239c6a3d7cabad` | 2,925 | import of the shared classification (one grep) |
| `reports/daily_report.py` | `ad40268a655b7c2860c8e31da4b7f2c0ff85d168` | 1,947 | import of the shared classification (one grep) |
| `ops_dashboard/backend/readers/db_reader.py` | `f59e4f58fe51e32d151deb5e14322c1bb2cc28b5` | 4,206 | the dashboard's status family |
| `tests/unit/test_state_store.py` | `323327dd51e7028b8668e6327d55a63774860230` | 2,367 | test source (documentary) |
| `tests/unit/test_migrations.py` | `d43c9b0867497a712b48781a83f2e1fd76c13e5d` | 535 | test source (documentary) |
| `tests/unit/test_fix135_eod_cleanup.py` | `062b4d6a262f7702b259057464e66413fa7d9f9a` | 347 | test source (documentary) |
| `tests/unit/test_db_retention.py` | `cb95fda97197b42e179d69325c22c875e29b8ffa` | 156 | test source (documentary) |
| `tests/unit/test_signal_status_classification.py` | `fd05297f4fea7814297172c7e5c605ccee210698` | 213 | test source (documentary) |
| `tests/crash_test/state_machine_validator.py` | `5443e7b26d9470c8b61a9e3bffe4b044f4e70cbe` | 277 | crash-test tool source (documentary) |
| `core/evidence_contract.py` | `29fb84bc690d05b832625fb55a496f3f8d4d18d7` | 701 | the screener's statuses named for evidence (one grep) |
| `orders/eod_squareoff.py` | `54f6cd6223353df0e9ad8bc823472f28b19b3818` | 1,888 | the 15:17 routine's clears |

Every line cited below was printed by a logged read of the file at its pinned blob (the inventory, sections 3 and 6). The cron file is cited by line number only.

### 1.2 Process deviations, reconciliation and Rama's answers

The inventory (section 8) records four process deviations; Claude's reconciliation table (items A to F) disposed of them, and ChatGPT's spot-check review accepted the table as the deviation record:

- **D-1 (item A):** read 083, one gated grep of `deploy/cron/trading-system.cron` for the two job names, printed three job lines whose commands include the deployed project directory. This was evidence handling, not host access. The gate's saved evidence copy in Claude's private scratch area was redacted at 19:53 IST (an append-only correction record keeps its before and after SHA-256; the read log is unchanged). The original output stays in the session's own conversation transcript, which Claude cannot edit; Rama accepted that residual as is.
- **D-2 (item B):** three files outside the Stage 0 register were read for one authorised question each: `core/ids.py` (the key's format), `orders/order_placer.py` (the trade_id link) and `capital/shadow_engine.py` (a TRADED mention).
- **D-3 (item C):** three Stage 0 NO rows were read narrowly: `orders/eod_squareoff.py` (the 15:17 routine's clears; its Stage 0 count hit was a docstring line, `orders/eod_squareoff.py:38`; the Stage 0 record is kept unchanged), `scripts/generate_screened_stocks_csv.py` (its `DROPPED_` labels) and `scripts/t2_cnc_gtt_realtest.py` (a `signals` INSERT).
- **D-4 (item D):** six non-test follow-up reads came after the test reads; no source claim depends on a test.
- **Item E:** Claude's citation checks are self-checks against the saved output of the logged reads, and both count cited line ranges (a single line counts as one range): the Stage 1 answers' 415 ranges (checked at Stage 1) and this chapter's 454 `path:lines` ranges (re-checked when each revision was built); in both, every cited line was printed by a logged read of that file at its pinned blob. The checks show that the cited lines were read, not that each statement about them is right. ChatGPT spot-checked 13 claims at the pin on 23-Oct-2026: 11 verified; one partly verified (the receiver's re-accept range; its queue-full range was not re-fetched); and the absence claim of section 5.3, not independently verified, stays bounded to its inspected scope.
- **Item F:** the labels and wording of this chapter (KNOWN DEFECT only for re-verified prior defects; code-path consequences as INTERPRETATION).

Rama's answers (23-Oct-2026, after ChatGPT's spot-check review):

- **OQ-02, HISTORICAL (RAMA'S STATEMENT):** signals rows left non-terminal (such as `QUEUED`, `PROCESSING` or `RESERVED`) were never cleaned up by hand during the live period. The question offered SQL, a script or a database reset as ways; Rama answered "No, never".
- **Item A:** the transcript residual is accepted as is.
- **Drafting:** this chapter's drafting was approved; no freeze, commit or push.

## 2. The signals table at the pin

### 2.1 Columns and the values the insert stores

**FACT (SOURCE-VERIFIED)** (Q1-a, Q1-e): `CREATE TABLE IF NOT EXISTS signals` (`core/schema.sql:42-79`) has fourteen columns, none with a DEFAULT. The receiver's INSERT names twelve of them (`signals/webhook_receiver.py:963-980`).

| Column | Type and constraint | Value written by the insert | Evidence |
|---|---|---|---|
| `signal_id` | TEXT PRIMARY KEY (commented "UUID4") | `new_signal_id()`: "sig_" plus a 32-character UUID4 hex | `core/schema.sql:43`; `signals/webhook_receiver.py:957`; `core/ids.py:38, 50-52` |
| `symbol` | TEXT NOT NULL | the symbol | `core/schema.sql:44`; `signals/webhook_receiver.py:975` |
| `scanner` | TEXT NOT NULL | the scanner name | `core/schema.sql:45`; `signals/webhook_receiver.py:975` |
| `strategy` | TEXT NOT NULL | the scanner name again | `core/schema.sql:46`; `signals/webhook_receiver.py:975` |
| `triggered_at` | TEXT NOT NULL | ISO string of the trigger time | `core/schema.sql:47`; `signals/webhook_receiver.py:958, 976` |
| `received_at` | TEXT NOT NULL | ISO string of the receipt time (IST) | `core/schema.sql:48`; `signals/webhook_receiver.py:677, 959, 976` |
| `expires_at` | TEXT NOT NULL | `received_at + expiry_sec`, with `expiry_sec = sq_cfg.expiry_sec` | `core/schema.sql:49`; `signals/webhook_receiver.py:679, 960-961` |
| `status` | TEXT NOT NULL with a CHECK (section 2.2) | `QUEUED` | `core/schema.sql:50-70`; `signals/webhook_receiver.py:977` |
| `rejection_reason` | TEXT, nullable | not in the column list, so NULL | `core/schema.sql:71`; `signals/webhook_receiver.py:967-971` |
| `trade_id` | TEXT, nullable; FOREIGN KEY to `trades(trade_id)` | not in the column list, so NULL; set later (section 4.1) | `core/schema.sql:72, 78`; `signals/webhook_receiver.py:967-971` |
| `trigger_price` | REAL | the parsed price | `core/schema.sql:73`; `signals/webhook_receiver.py:910, 977` |
| `fingerprint` | TEXT NOT NULL | SHA-256 of the scanner, the symbol and the epoch bucket (C-005 owns its meaning) | `core/schema.sql:74`; `signals/webhook_receiver.py:952-954` |
| `fingerprint_date` | TEXT NOT NULL | the IST date of `received_at` | `core/schema.sql:75`; `signals/webhook_receiver.py:678, 977` |
| `webhook_payload` | TEXT | the sanitised body (C-003 section 11.4 owns the redaction) | `core/schema.sql:76`; `signals/webhook_receiver.py:681-690, 722` |

**KNOWN DEFECT (C-001 D-05; the stored value re-verified):** the `strategy` column receives the scanner name (`signals/webhook_receiver.py:975`); how consumers read it is C-010's and K-003's (bookmark C7/BM-09).

### 2.2 The status CHECK

**FACT (SOURCE-VERIFIED)** (Q1-b): the CHECK enumerates 20 values, `QUEUED`, `ACCEPTED`, `PROCESSING`, `PROCESSED`, `PROCESSED_NO_PLACER`, `PLACEMENT_FAILED`, `RESERVED`, `QUEUE_FULL`, `PASSED`, `TRADED`, `DUPLICATE`, `EXPIRED`, `INVALID_SYMBOL`, `INVALID_PRICE`, `OUTSIDE_HOURS`, `IN_PROCESS`, `CANCELLED`, `FAILED`, `PENDING` and `TIMEOUT`, and allows any value matching GLOB `'REJECTED*'`, `'DROPPED_*'`, `'SKIPPED_*'`, `'GATE_*'` or `'RETEST_*'` (`core/schema.sql:60-70`). The comment calls the set open "by design", "a shape/typo guard ..., NOT a closed enum" (`core/schema.sql:51-59`). GLOB `'REJECTED*'` also admits a bare `REJECTED`. **HISTORICAL:** A-011 section 15 lists the same set (re-verified).

### 2.3 Keys, indexes, triggers and references

**FACT (SOURCE-VERIFIED)** (Q1-c, Q1-d):

- Indexes: the UNIQUE `idx_signals_fingerprint_today` on `(fingerprint, fingerprint_date)` (`core/schema.sql:81-83`; its dedup meaning is C-005's), `idx_signals_status` (`core/schema.sql:85-86`), `idx_signals_received_at` (`core/schema.sql:88-89`) and `idx_signals_trade_id` (`core/schema.sql:91-93`).
- Triggers: the schema's only CREATE TRIGGER line is `trg_trades_terminal_status_guard` (`core/schema.sql:291`), and every line holding "ON signals" is one of the index lines (`core/schema.sql:83, 86, 89, 93`); no trigger is on `signals` in the repository schema.
- Six tables declare `FOREIGN KEY (signal_id) REFERENCES signals(signal_id)`: `trades` (`core/schema.sql:254`), `screener_results` (`core/schema.sql:693`), `gate_state` (`core/schema.sql:797`), `shadow_trades` (`core/schema.sql:977`), `sr_detector_results` (`core/schema.sql:1290`) and `retest_state` (`core/schema.sql:1333`). `fm_ledger` (`core/schema.sql:529`) and `order_execution_log` (`core/schema.sql:1054`) carry a `signal_id` column with no such line. The comment at `core/schema.sql:104-115` describes the mutual reference with `trades`.

## 3. Schema history of the signals table

**FACT (SOURCE-VERIFIED)** (Q2-a, Q2-b):

| Version | Change to `signals` | Evidence |
|---|---|---|
| v1 | the first tables (1 to 8) | `core/schema.sql:1613` |
| v4 | `+signals.trigger_price` | `core/schema.sql:1614` |
| v14 | `+signals.webhook_payload` | `core/schema.sql:1627-1629` |
| v25 | the status CHECK, "Applied to existing DBs via core/migrations.py table rebuild"; MIGRATION_TABLES step 25 rebuilds `signals` (O2 CHECK) | `core/schema.sql:1642-1645`; `core/migrations.py:37-38, 115-116` |
| v38 | the CHECK widened to GLOB `'RETEST_*'`; MIGRATION_TABLES step 38 rebuilds `signals` | `core/migrations.py:81-83, 132`; `core/schema.sql:70` |

- A rebuild re-applies `schema.sql`'s current CREATE TABLE and copies the intersecting columns (`core/migrations.py:24-33, 307`); the mechanism and its atomicity are B-003's (sections 6.1 and 6.2). The migration map starts at v24 to v25 (`core/migrations.py:35-37`); how v4 and v14 reached existing databases was not traced (B-003 owns it).
- **FACT (TEXT DRIFT):** the history comment ends at v27 and is headed "END OF SCHEMA v24" while the stamp is 45 (`core/schema.sql:1610, 1613, 1650-1653`); B-003 section 5 already records this. The v38 widening appears only in the migration map and in the CHECK's own comment (`core/schema.sql:70`).
- **HISTORICAL / RECORD-DERIVED** (Q2-c; code comment, undated): the v44 to v45 note records a dry run "on a copy of production (423 trades / 713 orders / 49,410 signals)" (`core/migrations.py:97-99`); B-003 section 6.4 dates the v45 deployment to 27-Jul-2026.
- **UNKNOWN / NOT ESTABLISHED:** the deployed databases' schema and CHECK (OQ-01).

## 4. Storage-layer paths and transaction boundaries

### 4.1 Writes and reads

**FACT (SOURCE-VERIFIED)** (Q3-a to Q3-g):

- **Insert:** the store has no insert function for `signals`; the receiver inserts with its own SQL in its own transaction (`signals/webhook_receiver.py:963-980`). The INSERT lines at `core/state_store.py:165, 513` are docstring usage examples (`core/state_store.py:157-167, 511-515`).
- **Status update:** `update_signal_status` runs "UPDATE signals SET status = ?, rejection_reason = ? WHERE signal_id = ?" in its own transaction (`core/state_store.py:1408-1426`); an empty reason is stored as NULL (`core/state_store.py:1425`); the `ts` argument is "accepted for SP9 API symmetry; not stored (no updated_at col)" (`core/state_store.py:1413`); a missing `signal_id` is a silent no-op (`core/state_store.py:1420`). There is no condition on the current status.
- **Release functions:** `release_gate_state` (`core/state_store.py:2982-2998`) and `release_retest_state` (`core/state_store.py:3049-3059`) delete the child row (`gate_state` or `retest_state`) and run the same unconditional status UPDATE in one transaction; both are called with a status and no reason (`screening/entry_gate.py:512`; `screening/retest_monitor.py:209`), so `rejection_reason` becomes NULL.
- **Receiver flips:** two separate transactions after the insert: `QUEUED` on the re-accept of a `QUEUE_FULL` row, after the queue put (`signals/webhook_receiver.py:992-1007`), and `QUEUE_FULL` when the put finds the queue full (`signals/webhook_receiver.py:1050-1057`). Each UPDATE has only `WHERE signal_id = ?`, and a failure is only logged (`signals/webhook_receiver.py:1008-1011, 1058-1059`). The re-accept is decided by a SELECT of the existing row's status before the UPDATE, not in the same statement (`signals/webhook_receiver.py:987-992`). C-006 sections 4.2 and 5.2 (BM-01, BM-02) record the queue side.
- **Trade link:** `link_signal_trade` sets `signals.trade_id` in its own transaction (`orders/order_manager.py:718-724`); the order placer calls it before the broker execution (`orders/order_placer.py:1017-1018, 1027-1028`); on a failure the placer releases the reservation, marks the trade FAILED and raises, and its comment says the processor then marks the signal `PLACEMENT_FAILED` (`orders/order_placer.py:1024-1034`).
- **Readers in the store:** `count_signals_today` by `SUBSTR(received_at, 1, 10)` (`core/state_store.py:895-907`) and `get_signals_for_date` by `DATE(received_at)` (`core/state_store.py:2588-2594`); an innings report joins `signals` (`core/state_store.py:2908, 2910, 2936`). `get_reservation_id_for_signal` reads `fm_ledger`, not `signals` (`core/state_store.py:1281-1298`). The order manager reads `received_at` for latency (`orders/order_manager.py:464-471`).
- **Other writers of signals SQL** (P03 to P05, P44): eod_cleanup (`scripts/eod_cleanup.py:120-125, 248-268`; section 7) and `scripts/t2_cnc_gtt_realtest.py`, a script under `scripts/` that seeds its own `signals` row (`scripts/t2_cnc_gtt_realtest.py:212, 215, 227, 229`; Stage 0 R31: test-like, never run, live reach not established). The dynamic SQL writes found by P44 target other tables (`core/state_store.py:2554-2568`, the slippage logging) or are the rebuild's copy (`core/migrations.py:307`).

### 4.2 Transaction boundaries

**INTERPRETATION** (Q3-h): in the inspected paths, each later status-update call uses its own transaction except the two release functions, which combine the child-row deletion and the status update in one transaction (section 4.1). The screener's verdict and its `screener_results` row (`screening/secondary_screener.py:615-630`), the gate's `gate_state` row and its `GATE_WAITING` status (`screening/entry_gate.py:311-312`), and the parked retest candidate and its `RETEST_WAITING` status (`screening/retest_monitor.py:371-373`) are each two separate store calls; a failure is logged and not raised (`screening/secondary_screener.py:604-607`; `screening/entry_gate.py:313-317`; `screening/retest_monitor.py:93, 103-105`), so the inspected code path would let one half stand without the other.

### 4.3 Persistence path map

| Row | Input | Logic | Validation / guard | Output | Storage / hand-off | Next consumer |
|---|---|---|---|---|---|---|
| PM-01 | one symbol of an accepted webhook (C-002 to C-004) | `_process_signal`: fingerprint, new `signal_id`, expiry time (`signals/webhook_receiver.py:892, 949-961`) | symbol, price, age, in-flight claim and cache checks, none leaving a row (`signals/webhook_receiver.py:904-947`); the unique key (`core/schema.sql:81-83`) | INSERT of 12 columns, status `QUEUED` (`signals/webhook_receiver.py:963-980`) | own transaction (`signals/webhook_receiver.py:963-964`); then the queue put (`signals/webhook_receiver.py:1046-1049`) | the processor (C-006) |
| PM-02 | a repeat that hits the unique key | SELECT the existing row; a `QUEUE_FULL` row is re-queued, any other answers `DUPLICATE` (`signals/webhook_receiver.py:981-1015`) | the status is read before the flip, not in it (`signals/webhook_receiver.py:987-992, 1005`) | status `QUEUED`, or no write | own transaction; a failure is logged (`signals/webhook_receiver.py:1002-1011`) | the processor |
| PM-03 | a full queue at the first put | UPDATE status `QUEUE_FULL` (`signals/webhook_receiver.py:1050-1057`) | no | a `QUEUE_FULL` row; the batch answers 503 (`signals/webhook_receiver.py:60, 1068`) | own transaction; a failure is logged (`signals/webhook_receiver.py:1058-1059`) | a sender retry (PM-02); the readers (section 8) |
| PM-04 | a dequeued item, a gate release, a retest confirmation | the pipelines write `PROCESSING`, `RESERVED` and one end status (section 5.1, S-04 to S-12) | no (`core/state_store.py:1408-1426`) | status and `rejection_reason` | `update_signal_status`, one transaction per call | the readers; reservation and trade paths (C-008, G topics) |
| PM-05 | a screening verdict | `_persist` (`screening/secondary_screener.py:598-630`) | no | the verdict; reason = the rejected step or the status (`screening/secondary_screener.py:614`) | `update_signal_status`, then `insert_screener_result` (`screening/secondary_screener.py:615-630`) | the processor (PASSED continues); `screener_results` readers |
| PM-06 | a gate add or release; a retest park, duplicate or release | `screening/entry_gate.py:309-312, 501-512`; `screening/retest_monitor.py:180-212, 347-373` | no | `GATE_*`, `RETEST_*`, `REJECTED_RETEST_DUP` | releases: child row and status in one transaction (`core/state_store.py:2982-2998, 3049-3059`); add and park: two calls (section 4.2) | `continue_from_gate`, `continue_from_retest` (`signals/signal_processor.py:2010, 2375`; `screening/retest_monitor.py:186-190`) |
| PM-07 | the placer's trade, before execution | `link_signal_trade` (`orders/order_placer.py:1027-1028`; `orders/order_manager.py:718-724`) | no | `signals.trade_id` | own transaction (`orders/order_manager.py:720`) | the `trades` back-reference (`core/schema.sql:104-115`); eod_cleanup's guard (`scripts/eod_cleanup.py:236`) |
| PM-08 | the 15:50 eod_cleanup run | step 1 expiry; step 4 prune (`scripts/eod_cleanup.py:105-127, 211-271`) | `WHERE status = 'PROCESSING'` and the `triggered_at` date; the status filter, the cutoff and NOT EXISTS trades (`scripts/eod_cleanup.py:234-236`) | `EXPIRED` rows; deleted rows and children | step 1 one transaction; step 4 batches of 2,000, one commit each (`scripts/eod_cleanup.py:206-208, 250-268`) | none (rows gone) |
| PM-09 | stored rows | the store's readers (section 4.1) and the next consumers (section 8) | - | counts, buckets, families | - | reports, dashboard, readiness and crash checks |

## 5. Status vocabulary and transitions

### 5.1 Status map

Every row is FACT (SOURCE-VERIFIED) unless marked. "Prior-status check" says whether the write tests the row's current status.

| Row | Status | Writer | Condition (code text) | Reason column | Prior-status check | Next |
|---|---|---|---|---|---|---|
| S-01 | `QUEUED` (insert) | receiver INSERT (`signals/webhook_receiver.py:963-980`) | a symbol that passed the receiver's checks and claims (`signals/webhook_receiver.py:904-947`) | NULL | n/a (new row); the unique index refuses a same-bucket repeat (`signals/webhook_receiver.py:981-986`) | the queue put (`signals/webhook_receiver.py:1046-1049`) |
| S-02 | `QUEUED` (flip) | receiver re-accept (`signals/webhook_receiver.py:1002-1007`) | IntegrityError, the existing row is `QUEUE_FULL`, the put succeeded (`signals/webhook_receiver.py:987-1001`) | unchanged | no (`signals/webhook_receiver.py:1005`) | response ACCEPTED with the old `signal_id` (`signals/webhook_receiver.py:1012-1013`) |
| S-03 | `QUEUE_FULL` | receiver (`signals/webhook_receiver.py:1050-1057`) | `put_nowait` raised `queue.Full` | unchanged | no (`signals/webhook_receiver.py:1055`) | response `QUEUE_FULL`; the batch answers 503 (`signals/webhook_receiver.py:59-66, 1068`) |
| S-04 | `PROCESSING` | processor (`signals/signal_processor.py:897, 2034, 2393`) | start of `_process_one`, `continue_from_gate` and `continue_from_retest` (`signals/signal_processor.py:867, 2010, 2375`) | NULL | no | the pipeline's checks |
| S-05 | `REJECTED_<check>` | processor handlers (`signals/signal_processor.py:1201, 1662, 2316-2318, 2598`) | a `_PipelineReject`; names include `KILL_SWITCH`, `OUTSIDE_ENTRY_WINDOW`, `EXPIRED`, `UNKNOWN_STRATEGY`, `STRATEGY_CIRCUIT_BREAKER`, `RESERVE_FAILED`, `KILL_SWITCH_LATE`, `SHUTDOWN`, `ENTRY_THROTTLED` (`signals/signal_processor.py:907, 912, 918-919, 935-936, 990, 1338, 1396, 1405, 1416-1417`) and computed ones (`signals/signal_processor.py:972, 1128, 1325`) | `rej.reason` | no | the report's rejected bucket (`reports/signal_status.py:72-73`) |
| S-06 | `REJECTED_<reason>` | processor `reject_prepared` (`signals/signal_processor.py:1706-1713`) | the allocator's pre-check dropped the candidate | "allocator pre-check: `<reason>`" | no | as S-05 |
| S-07 | `REJECTED`, reason `QUEUE_FULL` | processor (`signals/signal_processor.py:428-437`) | the FIX-007 rate-limit pre-check's re-queue found the queue full; not wired at the pin (C-006 section 5.1, BM-06) | "QUEUE_FULL" | no | - |
| S-08 | `PLACEMENT_FAILED` | processor (`signals/signal_processor.py:473-475, 1238, 1683, 2345, 2620`) | an unhandled or placement exception | "unhandled: ..." or the exception text | no | the reservation released (`signals/signal_processor.py:1239-1241`) |
| S-09 | `RESERVED` | processor (`signals/signal_processor.py:1344, 2171, 2501`) | the capital reservation succeeded (`signals/signal_processor.py:1337-1340`) | NULL | no | placement |
| S-10 | `PROCESSED_NO_PLACER` | processor (`signals/signal_processor.py:1360, 2186, 2509`) | no order placer; the reservation is released (`signals/signal_processor.py:1353-1359`) | NULL | no | - |
| S-11 | `TIMEOUT` | processor (`signals/signal_processor.py:1480-1482, 2278-2280, 2573-2575`) | an ambiguous placement timeout; "recovery owns the reservation" (`signals/signal_processor.py:1483`) | the timeout text | no | HISTORICAL: B-005 section 5, the in-flight recovery is not wired |
| S-12 | `PROCESSED` | processor (`signals/signal_processor.py:1543, 2290, 2583`) | success after placement | NULL | no | - |
| S-13 | `PASSED`, `REJECTED_<step>`, `SKIPPED_<reason>` | secondary screener `_persist` (`screening/secondary_screener.py:598-620`) | each verdict: `PASSED` (`screening/secondary_screener.py:359, 518`); for example `REJECTED_SCORE_<n>`, `REJECTED_SIGNAL_AGE`, `REJECTED_STEP_ERROR`, `REJECTED_<gate reason>` (`screening/secondary_screener.py:275, 323, 343, 454`); `SKIPPED_QUOTE_UNAVAILABLE`, `SKIPPED_EXECUTOR_ERROR`, `SKIPPED_SCORER_ERROR` (`screening/secondary_screener.py:181, 260, 297`) | the rejected step, else the status (`screening/secondary_screener.py:590, 614`) | no | the processor continues on `PASSED` |
| S-14 | `GATE_WAITING` | entry gate `add()` (`screening/entry_gate.py:309-312`) | `add()`; KNOWN DEFECT (B-009 section 15 item 1, D1): no runtime caller of `add()`; C-007 re-verified the writer, not the absence of a caller | NULL | no | the `gate_state` row (`screening/entry_gate.py:311`) |
| S-15 | `GATE_RELEASED_<reason>` | entry gate release (`screening/entry_gate.py:501-512`) via `core/state_store.py:2982-2998` | `PRICE_HIT`, `TIMEOUT`, `QUOTE_UNAVAILABLE` or another reason (`screening/entry_gate.py:501-506`) | NULL | no | `continue_from_gate` (S-04) |
| S-16 | `RETEST_WAITING` | retest diverter (`screening/retest_monitor.py:359-373`) | the candidate was parked; HISTORICAL: the diverter is off and the monitor not started with the repository setting (B-006 TH11; B-009 section 5) | NULL | no | the `retest_state` row (`screening/retest_monitor.py:97`) |
| S-17 | `REJECTED_RETEST_DUP` | retest diverter (`screening/retest_monitor.py:347-351`) | the symbol is already parked | "symbol already parked in WAIT_FOR_RETEST" | no | - |
| S-18 | `RETEST_CONFIRMED`, `RETEST_REJECTED_<REASON>` | retest monitor release (`screening/retest_monitor.py:180-212`) via `core/state_store.py:3049-3059` | the zone evaluation's result | NULL | no | `continue_from_retest` on CONFIRMED (`screening/retest_monitor.py:186-190`) |
| S-19 | `EXPIRED` | eod_cleanup step 1 (`scripts/eod_cleanup.py:120-125`) | status `PROCESSING` and the `triggered_at` date before the run date | unchanged | yes: `WHERE status = 'PROCESSING'` | pruned later (S-21) |
| S-20 | `trade_id` (not a status) | order manager (`orders/order_manager.py:718-724`) via the placer (`orders/order_placer.py:1027-1028`) | the placer's trade, before broker execution (`orders/order_placer.py:1017-1018`) | - | no | the `trades` link |
| S-21 | (row deleted) | eod_cleanup step 4 (`scripts/eod_cleanup.py:211-271`) | `EXPIRED`, `DUPLICATE` or `REJECTED*`, older than the window, no `trades` row (`scripts/eod_cleanup.py:234-236`) | - | the status filter | children deleted first (`scripts/eod_cleanup.py:198-204, 260-266`) |

### 5.2 Composed and dynamic names

**FACT (SOURCE-VERIFIED)** (Q4-b, Q4-c):

- Status values carry numbers and free names: `REJECTED_SCORE_<score>` (`screening/secondary_screener.py:323`), `REJECTED_<gate reason>` (`screening/secondary_screener.py:454`), `RETEST_REJECTED_<REASON>` (`screening/retest_monitor.py:192`) and `REJECTED_SIZING_<constraint>` (`signals/signal_processor.py:1128`); the status report collapses the per-score family (`reports/signal_status.py:50-53, 147-156`).
- The processor's comment says a trade-type mismatch "gets its OWN reject label 'TRADE_TYPE' (-> REJECTED_TRADE_TYPE ...)" and other strategy-control rejects keep `STRATEGY_CONTROL` (`signals/signal_processor.py:964-972`).
- `REJECTED_NO_ATR_DATA` is raised as a check name (`signals/signal_processor.py:1804-1805, 1915-1916`), and the handler adds the prefix again (`signals/signal_processor.py:1201`). **INTERPRETATION:** the inspected code path would store `REJECTED_REJECTED_NO_ATR_DATA`, reachable only with `sl_method` or `tgt_method` ATR and `atr_fallback_mode` HALT (`signals/signal_processor.py:1801-1803, 1913-1914`); latent (bookmark C7/BM-04).
- **UNKNOWN / NOT ESTABLISHED:** the full set of `REJECTED_<check>` names; computed names come from other modules (`signals/signal_processor.py:972, 1128, 1325`; C-008).

### 5.3 Allowed values with no writer found (bounded)

**FACT (SOURCE-VERIFIED), bounded** (Q4-a): no writer was found in the inspected scope for `ACCEPTED`, `TRADED`, `DUPLICATE`, `INVALID_SYMBOL`, `INVALID_PRICE`, `OUTSIDE_HOURS`, `IN_PROCESS`, `CANCELLED`, `FAILED` or `PENDING`, nor for the `DROPPED_*` family. The inspected scope: every `update_signal_status` call in the runtime files (P39: 30 lines in four files, all read, section 5.1 with `screening/entry_gate.py:312`, `screening/retest_monitor.py:349, 373` and `screening/secondary_screener.py:615`), every `release_gate_state` and `release_retest_state` call (P38), every line-level INSERT, UPDATE or DELETE on `signals` in the code files (P03 to P05) and every SQL write built from a variable table name (P44), each read at its lines, all at the pinned commit. The P41 (`DROPPED_`) and P42 (quoted `TRADED`) hits are in files with none of those writes, for example a reader's label map (`scripts/generate_screened_stocks_csv.py:65-67`), a default argument (`capital/shadow_engine.py:103`) and a docstring query (`core/state_store.py:161`). This is not a claim that no writer exists anywhere; the patterns are line-level (the inventory, section 4).

The receiver's per-symbol response values are `ACCEPTED`, `DUPLICATE`, `EXPIRED`, `INVALID_SYMBOL`, `INVALID_PRICE`, `QUEUE_FULL`, `STORE_ERROR`, `OUTSIDE_HOURS` and `IN_PROCESS` (`signals/webhook_receiver.py:21-22`); its comment says `QUEUE_FULL` and `STORE_ERROR` "never reach signals.status" as responses, the `QUEUE_FULL` row being a separate UPDATE (`signals/webhook_receiver.py:64-65`). `STORE_ERROR` is not in the CHECK and is never stored (`signals/webhook_receiver.py:1016-1044`).

## 6. Start, stop, kill, crash and repair

### 6.1 Start

**FACT (SOURCE-VERIFIED), bounded** (Q5-a, Q5-b): in `main.py` a gated grep for `UPDATE signals`, `update_signal_status`, `QUEUED`, `PROCESSING`, the repair-name pattern and the gate and retest reload calls printed three lines: a comment on queued items (`main.py:1683`), the kill state's `clear_stale_state` (`main.py:2260`; `capital/kill_switch.py:315`) and the retest monitor's `rehydrate` (`main.py:3795`); P06b found no SQL read of `signals` in `main.py`. The kill switch's grep printed no `signals` write (read 065); `utils/startup_checks.py` had no matching line (read 068). At start the entry gate reloads `gate_state` rows (`screening/entry_gate.py:179-239`) and the retest monitor reloads `retest_state` rows (`screening/retest_monitor.py:118-136`); neither writes `signals` rows. No start step that writes `signals` rows was found in the inspected scope. **HISTORICAL:** B-006 TH21 and ST8 record the gate reload at every start.

### 6.2 Stop and kill

**FACT (SOURCE-VERIFIED)** (Q5-c): stop and kill write inside the pipeline: `REJECTED_KILL_SWITCH` at the pipeline's start (`signals/signal_processor.py:906-907, 2039, 2398`), `REJECTED_KILL_SWITCH_LATE` and `REJECTED_SHUTDOWN` before placement (`signals/signal_processor.py:1396, 1405, 2213, 2222, 2523, 2528`). **HISTORICAL:** C-006 section 8.1 records which drained items reach those checks.

### 6.3 Crash residues and repair

- **INTERPRETATION** (Q5-d): after a crash, the inspected code paths would leave each row in the status it had: `QUEUED` or `QUEUE_FULL` (receiver), `PROCESSING`, `RESERVED` or `PASSED` (processor and screener), `GATE_WAITING` or `RETEST_WAITING` (parked). Nothing found re-queues `QUEUED` rows (HISTORICAL: A-011 section 12, Q32; C-006 section 8.3).
- **FACT (SOURCE-VERIFIED), bounded:** the only repair found is eod_cleanup step 1 (S-19): at its next run it changes `PROCESSING` rows whose `triggered_at` date is before the run date to `EXPIRED` (`scripts/eod_cleanup.py:105-127`), so the same day's `PROCESSING` rows wait for the next run; the job skips non-trading days (`scripts/eod_cleanup.py:311-312`). The other residues are neither repaired nor pruned by the inspected paths (`scripts/eod_cleanup.py:234-236`).
- **FACT (SOURCE-VERIFIED)** (Q5-e): the system manager's readiness check counts `PROCESSING` rows received more than 15 minutes ago as "stuck signals" and warns; it writes nothing (`scripts/system_manager.py:675-691`). Its other `signals` queries are counts (`scripts/system_manager.py:637, 640`). `scripts/check_vm_state.py` only reads `signals` (`scripts/check_vm_state.py:72-74, 77, 89, 184-186`).
- **HISTORICAL (RAMA'S STATEMENT), OQ-02:** such rows were never cleaned up by hand. **INTERPRETATION:** with no hand clean-up, the inspected paths do not repair or prune the listed non-`PROCESSING` residues (`QUEUED`, `QUEUE_FULL`, `RESERVED`, `PASSED`, `GATE_WAITING`, `RETEST_WAITING`). Earlier-dated `PROCESSING` rows are an exception: step 1 can change them to `EXPIRED`, after which eligible rows may be pruned by step 4 under RT-02 (section 7). Whether any such residue existed, its status distribution, and how long it remained are **UNKNOWN / NOT ESTABLISHED** (U-02; V2_DESIGN_LOG Q-37 (f)).

### 6.4 The 15:17 routine's clears

**FACT (SOURCE-VERIFIED)** (Q5-f): the routine's step 0 would clear the entry gate and the parked retest candidates, each guarded by a not-None check (`orders/eod_squareoff.py:340-356`); the clears delete `gate_state` and `retest_state` rows without a status write (`screening/entry_gate.py:260-279`; `core/state_store.py:3000-3007`; `screening/retest_monitor.py:107-116`; `core/state_store.py:3061-3065`). **HISTORICAL:** B-009 erratum E-1 records that `main.py` builds the routine without the gate, so its gate step is inert.

**INTERPRETATION** (Q5-g): a `gate_state` row left from an earlier version would keep its `signals` row `GATE_WAITING` and be reloaded at every start; its release status (S-15) depends on the gate's poll (C-008 and D topics; bookmark C7/BM-06).

## 7. Retention and pruning

| Row | Rule | Code | Configuration and default | Schedule |
|---|---|---|---|---|
| RT-01 | Stale `PROCESSING` rows become `EXPIRED` | `status = 'PROCESSING' AND SUBSTR(triggered_at, 1, 10) <` the run date (`scripts/eod_cleanup.py:105-127`) | none | step 1 of the 15:50 job |
| RT-02 | Noise rows are deleted | status IN (`EXPIRED`, `DUPLICATE`) OR GLOB `'REJECTED*'`, `fingerprint_date <` cutoff, the cutoff being the run date minus the window (`scripts/eod_cleanup.py:215-217`), no `trades` row (`scripts/eod_cleanup.py:234-236`); batches of 2,000 (`scripts/eod_cleanup.py:206-208`), children first (`scripts/eod_cleanup.py:198-204, 259-266`), one commit per batch (`scripts/eod_cleanup.py:268`) | FACT (CONFIG): `signal_retention_days: 90` (`config/system_config.yaml:481-484`); the model's default 90, minimum 1, extra keys forbidden (`core/config_loader.py:1976-1984, 2009`); a CLI value overrides (`scripts/eod_cleanup.py:43-48, 57-58`); a configuration failure falls back to the model default (`scripts/eod_cleanup.py:59-67`) | step 4 of the 15:50 job |
| RT-03 | The job | `main()` (`scripts/eod_cleanup.py:274-304`): the database is `data_store/trading_system.db` under the project root unless `--db` (`scripts/eod_cleanup.py:278`); the date is today unless `--date` (`scripts/eod_cleanup.py:285`) | - | FACT (CONFIG): 15:50 Monday to Friday, market days only, enabled (`config/cron_registry.yaml:453-466`; `deploy/cron/trading-system.cron:99-100`); the entry skips non-trading days and runs under a heartbeat with an alert (`scripts/eod_cleanup.py:307-320, 323-324`) |
| RT-04 | db_retention | candles, system_metrics, webhook_audit, screener_results (90 days), reconciliation_log (180), fm_ledger (365); `signals` not listed (`scripts/db_retention.py:52-70`); an override replaces a listed table's window only (`scripts/db_retention.py:94, 103-105, 181, 189, 191`) | its own defaults | FACT (CONFIG): 02:30 Monday to Saturday; Sunday with `--vacuum` (`config/cron_registry.yaml:125-139, 140, 151`; `deploy/cron/trading-system.cron:36-37, 39-40`) |

- **FACT (SOURCE-VERIFIED)** (Q6-a): no inspected path deletes rows whose status is outside RT-02's filter (`QUEUED`, `QUEUE_FULL`, `RESERVED`, `PASSED`, `PROCESSED`, `PROCESSED_NO_PLACER`, `PLACEMENT_FAILED`, `TIMEOUT`, `SKIPPED_*`, `GATE_*`, `RETEST_*`) or any trade-linked row; `PROCESSING` rows go only through RT-01. P05: eod_cleanup is the only non-test file that deletes from `signals`. Its comment says the filter keeps "the trade audit trail (QUEUED->...->TRADED / PLACEMENT_FAILED / PROCESSED)" (`scripts/eod_cleanup.py:219-221`).
- **FACT (SOURCE-VERIFIED)** (Q6-b): the fingerprint leaves with its row (RT-02); the unique key is per `fingerprint_date` (`core/schema.sql:81-83`). **INTERPRETATION:** a prune of rows older than the window does not touch the same day's key (C-005 owns the dedup).
- **HISTORICAL / RECORD-DERIVED** (Q6-c; code comments): the step 1 filter once read `'IN_PROCESS'`, "a status the pipeline NEVER persists", and stuck rows were "never cleaned" (`scripts/eod_cleanup.py:110-112`, "audit 04-Jul"); the prune's `REJECTED*` filter caused "the 15:50 rollback" until children were deleted first (`scripts/eod_cleanup.py:227-231`); a "large backlog (~108k)" (`scripts/eod_cleanup.py:207`); the 90-day default is dated "Rama, 15-Jul-2026" (`core/config_loader.py:1982`) and the setting "15-Jul-2026" (`config/system_config.yaml:481`). The dates on which these changes reached production are **UNKNOWN / NOT ESTABLISHED**.
- **UNKNOWN / NOT ESTABLISHED:** whether the installed schedule ran eod_cleanup and db_retention, and their outcomes (U-03). The repository schedule and configuration establish the intended configuration only.

## 8. Next consumers and the statuses they expect

| Reader | Its classification | Evidence |
|---|---|---|
| `reports/signal_status.py` (imported by the daily report and the trade review) | `bucket()` checks, in order: `REJECTED*` (rejected), `SKIPPED_*` (skipped), `DROPPED_*` (dropped), qualified (`PROCESSED`, `TRADED`, `PLACEMENT_FAILED`, `RESERVED`, `PROCESSED_NO_PLACER`, `PASSED`), other (`EXPIRED`, `QUEUE_FULL`, `QUEUED`, `PENDING`, `CANCELLED`, `TIMEOUT`, `IN_PROCESS`, `FAILED`, `ACCEPTED`, `PROCESSING`, `DUPLICATE`, `INVALID_SYMBOL`, `INVALID_PRICE`, `OUTSIDE_HOURS`, `GATE_*`, `RETEST_*`), and unmapped, "a real schema-drift alarm" | `reports/signal_status.py:2, 39-48, 68-82`; `reports/daily_report.py:53`; `reports/daily_trade_review.py:79` |
| Dashboard reader | in order: expired = `REJECTED_EXPIRED` or `EXPIRED`; duplicated = `DUPLICATE`; rejected = `REJECTED*`, `DROPPED_*`, `SKIPPED_*`, `QUEUE_FULL`, `PLACEMENT_FAILED`, `TIMEOUT`; accepted = everything else, `QUEUED`, `PROCESSING`, `RESERVED`, `GATE_*` and `RETEST_*` included; a `QUEUED` row with no trade shows as "Received" | `ops_dashboard/backend/readers/db_reader.py:660-681, 892-896` |
| System manager | `PROCESSING` older than 15 minutes = stuck | `scripts/system_manager.py:680` |
| eod_cleanup's prune | `EXPIRED`, `DUPLICATE`, `REJECTED*` | `scripts/eod_cleanup.py:234` |
| Crash harness, FACT (TEST TEXT) | a terminal set and a transition table; it flags any status it does not know | `tests/crash_test/state_machine_validator.py:49-110, 136-137` |
| Evidence contract | says `signals.status` "keeps a terminal verdict"; names the screener's statuses for evidence capture | `core/evidence_contract.py:8, 164-166, 174, 178` |

**INTERPRETATION** (Q7-d): no single definition of a terminal signal status was found in the readers read (section 12 lists the readers not read). `QUEUE_FULL` is "terminal" in the report's docstring (`reports/signal_status.py:88-89, 101-102`) but sits in the set the same file calls "Known non-terminal / other" (`reports/signal_status.py:43, 46`); it is rejected on the dashboard (`ops_dashboard/backend/readers/db_reader.py:679`); and the receiver re-accepts a `QUEUE_FULL` row (S-02). Bookmarks C7/BM-07 and C7/BM-08.

## 9. Text drift and code-path findings

### 9.1 Text drift

**FACT (TEXT DRIFT):**

- **D-01:** the table's header says "Immutable log of EVERY incoming webhook signal ... every signal has a final status" (`core/schema.sql:36-38`); refused symbols leave no row (`signals/webhook_receiver.py:904-947, 1014-1015, 1044`; HISTORICAL: A-011 section 12), rows are updated after the insert (section 4.1), and non-terminal rows can remain (section 6.3).
- **D-02:** "Four open prefix families are allowed via GLOB" (`core/schema.sql:53-54`); the CHECK has five (`core/schema.sql:66-70`).
- **D-03:** the writer list "webhook_receiver, signal_processor, secondary_ screener, entry_gate" (`core/schema.sql:51-53`) leaves out the retest diverter and monitor (S-16 to S-18) and eod_cleanup (S-19).
- **D-04:** `signal_id` is commented "UUID4" (`core/schema.sql:43`); the value is "sig_" plus the UUID4 hex (`core/ids.py:9-13, 50-52`).
- **D-05:** eod_cleanup's header says "Prune old signal fingerprints (>7 days)" (`scripts/eod_cleanup.py:9`); the window is 90 days by default (RT-02), and the step deletes whole `signals` rows and their children (`scripts/eod_cleanup.py:259-266`), while the header, the step's name and the setting speak of "fingerprints" (`scripts/eod_cleanup.py:9, 95`; `config/system_config.yaml:482`; `core/config_loader.py:1977-1979`). Its comment counts "seven children" of `signals` and then lists six (`scripts/eod_cleanup.py:195-197`); the schema declares six (section 2.3).
- **D-06:** `update_signal_status`'s docstring: "so every signal_id has a terminal status" (`core/state_store.py:1418-1419`). The dashboard's "DUPLICATE (post-insert race only ...)" and "legacy EXPIRED (processor age gate)" (`ops_dashboard/backend/readers/db_reader.py:666-667`): no writer of a `DUPLICATE` row was found (section 5.3), the `EXPIRED` row writer is eod_cleanup (S-19), and the processor's age gate writes `REJECTED_EXPIRED` (`signals/signal_processor.py:918-919, 1201`). FACT (TEST TEXT): the prune test's docstring says the pipeline never persists "a bare 'REJECTED'" (`tests/unit/test_fix135_eod_cleanup.py:117`); S-07 writes one on a path that C-006 records as not wired.
- **D-07, FACT (TEST TEXT):** the crash harness's model (`tests/crash_test/state_machine_validator.py:49-110`) has `ACCEPTED`, `IN_PROCESS`, `DROPPED_BACKPRESSURE` and `DROPPED_SHUTDOWN` and none of `RESERVED`, `PASSED`, `QUEUE_FULL`, `TIMEOUT`, `PLACEMENT_FAILED`, `PROCESSED_NO_PLACER`, `GATE_*` or `RETEST_*`, all of which the pinned writers use (section 5.1).
- **D-08:** the schema says `signals.trade_id` is "set if signal became a trade" and "set post-fill ... updated after the trade opens" (`core/schema.sql:72, 108-109`); the placer links it before the broker execution (`orders/order_placer.py:1017-1018`).

### 9.2 Code-path findings (the answers' K-01 to K-05)

- **K-01, FACT (SOURCE-VERIFIED):** the store's three status UPDATEs (`core/state_store.py:1424, 2996, 3057`) and the receiver's two flips (`signals/webhook_receiver.py:1005, 1055`) select the row by `signal_id` only; eod_cleanup's expiry alone filters on the current status (`scripts/eod_cleanup.py:122`); the time of a status change is not stored (`core/state_store.py:1413`). **INTERPRETATION:** the inspected code path would let a later write replace whatever status the row holds (for the queue paths, C-006 BM-01 and BM-02). Open bookmark C7/BM-01.
- **K-02, FACT (SOURCE-VERIFIED), bounded:** no inspected path repairs non-terminal rows except earlier-dated `PROCESSING` rows, which eod_cleanup step 1 changes to `EXPIRED` at its next run (section 6.3). **INTERPRETATION:** against the table's own "final status" comment (`core/schema.sql:36-38`), the inspected paths would leave the non-`PROCESSING` residues non-terminal; Rama reports no hand clean-up (section 1.2); whether any such row existed is **UNKNOWN / NOT ESTABLISHED** (U-02). Open bookmark C7/BM-07.
- **K-03, INTERPRETATION:** RT-02's filter leaves out `SKIPPED_*`, `GATE_*` and `RETEST_*` rows of any age, and the K-02 residues; where such a row is final (for example `RETEST_REJECTED_*`, after which the monitor only logs, `screening/retest_monitor.py:191-194`), no inspected path removes it, against the prune's stated aim that "the signals table + its dedup index do not grow unbounded" (`scripts/eod_cleanup.py:219-221`).
- **K-04, INTERPRETATION:** `expires_at` is stored as `received_at` plus `expiry_sec` (`signals/webhook_receiver.py:960`; `core/schema.sql:49`), while both age checks measure from `triggered_at` (`signals/webhook_receiver.py:924-926`; `signals/signal_processor.py:914-917`), so the stored column is not the expiry the code applies (C-009 owns the policy; bookmark C7/BM-05).
- **K-05, INTERPRETATION:** the doubled prefix of section 5.2 (latent; bookmark C7/BM-04).

## 10. Documentary test evidence

Tests were read as documents only and were never run (FACT (TEST TEXT)):

- **T-a:** `test_update_signal_status`: `QUEUED`, then `PROCESSING` with an empty reason stored as NULL, then `REJECTED_EXPIRED` with its reason; a missing `signal_id` is a no-op (`tests/unit/test_state_store.py:623-645`).
- **T-b (Q2-d):** `test_migrations.py` inserts each of 18 values it calls "real" (`QUEUED`, `IN_PROCESS`, `PROCESSING`, `PASSED`, `PROCESSED`, `PROCESSED_NO_PLACER`, `PLACEMENT_FAILED`, `RESERVED`, `TRADED`, `REJECTED`, `REJECTED_SCORE_45`, `REJECTED_NO_ATR_DATA`, `DROPPED_BACKPRESSURE`, `SKIPPED_QUOTE_UNAVAILABLE`, `GATE_WAITING`, `GATE_RELEASED_PRICE_HIT`, `TIMEOUT`, `EXPIRED`) and expects IntegrityError for "queued", "", "OPEN", "FILLED" and "rejected_x" (`tests/unit/test_migrations.py:360, 370-384`). `IN_PROCESS`, `TRADED` and `DROPPED_BACKPRESSURE` have no writer in the inspected scope (section 5.3). HISTORICAL: B-003 section 14 records the failsafe test.
- **T-c (Q6-d):** stale `PROCESSING` from the day before becomes `EXPIRED`, the same day's does not, a `TRADED` row is untouched (`tests/unit/test_fix135_eod_cleanup.py:85-110`); `EXPIRED`, `DUPLICATE` and `REJECTED_*` rows are pruned and `TRADED` and `PROCESSED` kept (`tests/unit/test_fix135_eod_cleanup.py:116-145`); the file also names FK-safe, trade-linked and batched prune tests (`tests/unit/test_fix135_eod_cleanup.py:185, 203, 238`). `test_db_retention.py` names no `signals` table (read 111).
- **T-d:** `test_signal_status_classification.py`'s test names cover the report's rules: the per-score family, the terminal non-rejected statuses, an unmapped status, `DUPLICATE` against `REJECTED_DUPLICATE_SYMBOL`, the trade review's delegation (`tests/unit/test_signal_status_classification.py:107, 140, 147, 153, 165`).
- **Crash harness:** its model is D-07 and its reader row is in section 8 (`tests/crash_test/state_machine_validator.py:49-110, 136-137`).
- **T-e, INTERPRETATION:** a test that builds its own store proves the logic it builds, not the production wiring (V2 design log, lesson of 16-Oct-2026); none of these tests exercises the receiver's flips or a crash.

## 11. Frozen-manual reconciliation and bookmarks

### 11.1 Frozen statements re-verified at the pin

No erratum to a frozen manual is proposed.

| Frozen statement | Result at the pin |
|---|---|
| A-011 section 15: the signals status list | Confirmed (`core/schema.sql:60-70`). |
| A-011 section 12, Q32: a crash loses the queue; rows stay non-terminal; no re-queue at start in the inspected path | Consistent (section 6.3; bounded). |
| B-003 section 5: the schema's history comment is stale against the stamp 45 | Confirmed (`core/schema.sql:1610, 1613`). |
| B-003 section 6.1: a rebuild re-applies the schema's definition and copies the intersecting columns | Confirmed for `signals` (`core/migrations.py:24-33, 115-116, 132`). |
| B-003 section 14: the failsafe test builds a database whose `signals` row violates the new CHECK | Consistent with the test source, FACT (TEST TEXT) (`tests/unit/test_migrations.py:132, 134`). |
| B-003 section 11: db_retention's policy | Confirmed; `signals` is not in it (`scripts/db_retention.py:52-70`). |
| B-005 section 5: the in-flight recovery is coded and not wired | Cited for S-11; not re-traced here. |
| B-006 TH11, TH21 and ST8: the retest monitor not started; the gate reload at every start | Consistent: the reload functions write no `signals` rows (`screening/entry_gate.py:179-239`; `screening/retest_monitor.py:118-136`). |
| B-009 section 5: `REJECTED_EXPIRED` at the processor's age gate | Confirmed (`signals/signal_processor.py:918-919, 1201`). |
| B-009 section 15 item 1 (D1): the gate's `add()` has no runtime caller | The writer re-verified (`screening/entry_gate.py:309-312`); the absence of a caller not re-traced. |
| B-009 erratum E-1: the 15:17 routine's gate clear is inert | Consistent: the routine's clears are guarded (`orders/eod_squareoff.py:340-356`); `main.py`'s construction of the routine not re-read. |
| B-010 section 10: eod_cleanup at 15:50 | Confirmed in the repository schedule (`config/cron_registry.yaml:453-466`; `deploy/cron/trading-system.cron:99-100`). |
| C-001 D-05: `signals.strategy` stores the scanner key | The stored value re-verified (`signals/webhook_receiver.py:975`). |
| C-003 section 11.4: the stored payload is sanitised | The insert stores the sanitised copy (`signals/webhook_receiver.py:681-690, 722`). |
| C-005 section 8: `signal_retention_days: 90`; pruning left to C-007 | Traced (section 7, RT-02). |
| C-006 sections 5.2 and 7: the queue paths' status updates are unconditional | Confirmed, and extended to every status writer except eod_cleanup step 1 (section 9.2, K-01). |
| C-006 section 8.3: stale `PROCESSING` rows become `EXPIRED`; `QUEUED` and `QUEUE_FULL` stay | Confirmed, with the condition that the `triggered_at` date is before the run date (`scripts/eod_cleanup.py:105-127`). |

### 11.2 Bookmarks (W-9)

Each bookmark is a hand-off for its owner topic or the whole-design review; C-007 does not resolve another topic's question.

| Bookmark | Point | Cross-check with |
|---|---|---|
| C7/BM-01 | All status writes are unconditional (K-01); re-verifies the store side of BK-04, BK-10 and BK-11 | C-006 BM-01, BM-02; C-008 |
| C7/BM-02 | The prune's `DUPLICATE` has no row writer at the pin, its `EXPIRED` comes only from step 1, and its `IN_PROCESS` is history (`scripts/eod_cleanup.py:110-112`): the C-007 part of BK-05; closing BK-05 is for reconciliation | C-005 BM-03 (BK-05); B-010 |
| C7/BM-03 | The re-accept flip changes only the status (`signals/webhook_receiver.py:1005`) while the queued item carries the retry's price and time (`signals/webhook_receiver.py:993`), so the row keeps the first attempt's values; the row side of BK-14 | C-006 BM-05 (BK-14); C-008 |
| C7/BM-04 | The doubled prefix `REJECTED_REJECTED_NO_ATR_DATA` (section 5.2) | C-008 |
| C7/BM-05 | `expires_at`'s basis differs from the age checks' basis (K-04) | C-009 |
| C7/BM-06 | The `GATE_WAITING` writer, `add()`, has no runtime caller (B-009 D1) and the EOD gate clear is inert (B-009 E-1), so an old `gate_state` row would reload at every start (section 6.4) | C-008; D topics; B-009 D1, E-1 |
| C7/BM-07 | No terminal-status definition; residues kept; no status-change time (section 8, K-01, K-02); with Rama's statement of no hand clean-up | K-003, K-011, K-012; VP-59 (BK-18); Q-37 (f); whole-design review |
| C7/BM-08 | Readers disagree on `QUEUE_FULL`, `DUPLICATE` and `EXPIRED` (section 8, D-06) | L-010, L-011 |
| C7/BM-09 | The `strategy` column stores the scanner name (section 2.1) | C-001 D-05; C-010; K-003 |
| C7/BM-10 | Ten enumerated values and `DROPPED_*` have no writer in the inspected scope; the CHECK admits them (section 5.3) | K-003 |
| C7/BM-11 | The crash harness's model is stale (D-07) | N-005 (failure testing) |
| C7/BM-12 | The receiver's `signals` SQL sits in `_process_signal` only (`signals/webhook_receiver.py:892, 967, 988, 1005, 1055`; P03, P04); whether the EOD route reaches that function was not traced | C-011 |

### 11.3 Open questions and unknowns

- **OQ-01 (OPEN QUESTION, carried from C-006 OQ-1):** whether the deployed constraint matched the repository schema.
- **OQ-02 (answered):** Rama, 23-Oct-2026: no hand clean-up of non-terminal rows (section 1.2). For the freeze package: a note to V2_DESIGN_LOG Q-37 (f), whose other half (which rows crashes left non-terminal) stays open.
- **UNKNOWN / NOT ESTABLISHED:** U-01 the deployed databases' schema and CHECK; U-02 the stored rows by status, the residues and the growth (the comment figures, 49,410 signals and about 108k, and the report's measured examples at `reports/signal_status.py:13-20, 91, 105`, are RECORD-DERIVED and not verified; HISTORICAL: B-009 section 13 records a census's leading statuses); U-03 whether the installed schedule ran eod_cleanup and db_retention, and their outcomes, including the dates of the 15:50 rollback; U-04 whether any `GATE_*` or `RETEST_*` row exists, and since when; U-05 whether a logged status-write failure ever happened (`signals/webhook_receiver.py:1008-1011, 1058-1059`; `screening/entry_gate.py:313-317`; `screening/secondary_screener.py:604-607`); U-06 the full set of `REJECTED_<check>` names (section 5.2); U-07 rows stored before the 05-Jul-2026 payload sanitisation (HISTORICAL: C-003 R-05 and L-04).

## 12. Known limits and exclusions

- A count-only search establishes that a pattern matched a file, not what the matching lines mean; a file named only by a count is not source-verified. The search patterns are line-level; the absence claims of sections 5.3 and 6 are bounded to the files, patterns and reads in the inventory (sections 3 and 4).
- Readers of `signals` found by P06b and not read (names only): `ops/control_tower/freshness.py`, `scripts/healthcheck_server.py`, `scripts/backup_restore_drill.py`, `scripts/fetch_daily_candles.py`, `scripts/forward_shadow_record.py`, `scripts/gemini_trade_coach.py`, `scripts/replay_signals.py`, `scripts/strategy_registry_officer.py`, `scripts/trade_journal.py`, and `reports/daily_trade_review.py` beyond its import; of the dashboard reader's 27 matching lines only the status-family helper and the funnel's rule were read. Section 8 is bounded to the readers read.
- `scripts/eod_cleanup.py` was read for its signal-row and fingerprint steps only; its steps 2 and 3 (orders and smart targets) were not read.
- Records R43 to R47 were names only; the two July records (R48) were excluded. No historical conclusion is drawn from them.
- Claude's checks are self-checks; ChatGPT's spot-check covered 13 claims (section 1.2). No live or host configuration, database state, runtime event, scheduled run or deployed schema was verified.

## 13. End-to-end source path

```text
webhook symbol (C-002 to C-004)
  -> refusals before the insert (symbol, price, age, in-flight, cache): no row
  -> INSERT signals ... status QUEUED (own transaction; unique key per fingerprint and day)
       -> queue put
            | full: UPDATE status QUEUE_FULL (own transaction; failure only logged)
            | a later same-bucket repeat of a QUEUE_FULL row: put, then UPDATE status QUEUED
  -> processor (C-008 owns the decisions)
       PROCESSING -> [screener: PASSED / REJECTED_<step> / SKIPPED_<reason>]
       -> [gate: GATE_WAITING -> GATE_RELEASED_<reason> | retest: RETEST_WAITING -> RETEST_<result>]
       -> REJECTED_<check> | RESERVED -> PROCESSED / PROCESSED_NO_PLACER / PLACEMENT_FAILED / TIMEOUT
       (every write unconditional; no status-change time stored)
  -> order placer: signals.trade_id set before broker execution
  -> eod_cleanup 15:50 (repository schedule)
       step 1: PROCESSING with an earlier triggered_at date -> EXPIRED
       step 4: EXPIRED / DUPLICATE / REJECTED*, older than 90 days, no trade -> deleted, children first
  -> readers: status report, dashboard, system manager, crash harness (each its own set)
  crash: rows keep their status; no re-queue at start; no hand clean-up (Rama)
```

## 14. Final factual position

The pinned source defines a 14-column `signals` table with an open status CHECK (20 values and five GLOB families), rebuilt for that CHECK at v25 and v38. The receiver inserts one `QUEUED` row per accepted symbol; refused symbols leave no row. Every later status write updates the row by `signal_id` without checking its current status, except eod_cleanup's expiry of earlier-dated `PROCESSING` rows, and the time of a status change is not stored. Retention deletes only old `EXPIRED`, `DUPLICATE` and `REJECTED*` rows without a trade; other residues are neither repaired nor pruned by the inspected paths, and Rama states that none was cleaned by hand. The next consumers classify the same statuses differently. These are source-derived descriptions and code-path interpretations, not proof that any specific row, failure or clean-up existed in production.

**Freeze status:** DRAFT, revision 2; not frozen.

## 15. Revisions

### 15.1 Revision 0 (2026-10-23)

Revision 0 is Claude's draft (C-007_SIGNAL_PERSISTENCE_MANUAL_DRAFT_23-OCT-2026.md, kept unchanged), written after ChatGPT's reconciliation review and spot-check review and Rama's answers. It carries the answers' facts unchanged and applies the reconciliation (Claude's table, items A to F):

1. Section 1.2: the four process deviations, the redaction under item A, the spot-check result under item E, and Rama's answers (OQ-02, the transcript residual, the drafting approval).
2. K-01 and K-02 relabelled: FACT (SOURCE-VERIFIED) for the missing check or repair, INTERPRETATION for the consequence, as open bookmarks (item F).
3. KNOWN DEFECT used only for prior defects re-verified here, with the extent stated: C-001 D-05 (section 2.1) and B-009 D1 (S-14).
4. FACT (TEST TEXT) for the crash harness's model and reader row (D-07, section 8) and for the prune test's docstring (D-06) (item D).
5. `scripts/t2_cnc_gtt_realtest.py` described as "a script under scripts/ that seeds its own signals row" (item C).
6. "The inspected code path would ..." wording for code-path consequences (sections 6.3 and 9.2).
7. The absence claim of section 5.3 stated as bounded to its inspected scope (item E).

### 15.2 Revision 1 (2026-10-23)

Revision 1 applies ChatGPT's review notes on revision 0 (C-007_CHATGPT_REVIEW_NOTES_23-OCT-2026.txt); no fact, label, citation or bookmark changes otherwise:

1. Required correction 1: section 1.1, the role of `main.py` reads "startup-time search for signal-row writes and reload paths (one grep); no signal-row writer found in the inspected scope", matching section 6.1. The same wording fix is applied to `capital/kill_switch.py`, whose role ("kill-time writes") had the same contradiction with section 6.1.
2. Required correction 2: section 6.3, the interpretation after OQ-02 now names the non-`PROCESSING` residues that the inspected paths do not repair or prune, and states the exception for earlier-dated `PROCESSING` rows (step 1, then RT-02); K-02 (section 9.2), which restates the same point, is aligned.
3. Optional clarification: section 4.2 scopes the transaction statement to later status-update calls in the inspected paths.
4. Header: the status line and the inputs (the review notes added).

### 15.3 Revision 2 (2026-10-23)

Revision 2 applies ChatGPT's final review note on revision 1 (C-007_REV1_FINAL_REVIEW_NOTE_23-OCT-2026.txt); nothing else changes:

1. Section 1.2, item E: the two citation counts explained. 415 is the number of cited line ranges in the Stage 1 answers; 454 is the number of `path:lines` ranges in this chapter (the same count in revisions 0 and 1). Both checks confirm only that every cited line was printed by a logged read.
2. Header: the status line and the inputs (the final review note added).
