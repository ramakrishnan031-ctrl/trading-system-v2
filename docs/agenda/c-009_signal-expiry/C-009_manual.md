# C-009 - SIGNAL EXPIRY: LEGACY MANUAL

Status: FROZEN 2026-10-24 (revision 1, DR-062). Revision 0 is ChatGPT's draft of 24-Oct-2026 (C009_Legacy_Manual_Draft_RESHARE_24-Oct-2026.md, stored unchanged in docs/inputs/manuals/; SHA-256 A38EC20146E8E05B93DD130F2876BB00797CDAD693842AF95E347E67738BB85F), written with its reconciliation and review (C009_Reconciliation_Review_RESHARE_24-Oct-2026.txt; SHA-256 7307D1E59AED4F219D89D7797AA5C8C0A21798689A40D5BF8F2648812EF11751) from Claude's Stage 1 results. Revision 1 applies Claude's final review under ChatGPT's finalisation prompt (C009_Claude_Finalization_Prompt_RESHARE_24-Oct-2026.txt; SHA-256 99B8E395F16317EA63B91A293A82DFA1AD46E11C52749CA7EF41D9A252ED6A65); both are stored unchanged in docs/inputs/reconciliation/. The corrections are listed with their evidence in C-009_FINAL_REVIEW_AND_EVIDENCE_REGISTER.md section 2 and summarised in section 15. Frozen under W-4 (DR-017), W-9 and W-10, with no separate approval step: the freeze takes effect when Rama runs PART 2 of its push commands.  
Topic: C-009 Signal expiry (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 24-Oct-2026 through the gated masking filter.  
Companion files, frozen with this manual and stored unchanged beside it in docs/agenda/c-009_signal-expiry/: C-009_CLAUDE_STAGE1_S1-2_READ_RESULTS_24-OCT-2026.txt (reads 001 to 027, verbatim and masked; SHA-256 D707911FE762ED713A9FDEA34A4190D1806C2A03BED7F78DB9B883233D1C055A), C-009_CLAUDE_STAGE1_S2-19_S2-20_P2_RESULTS_AND_COMPARISON_24-OCT-2026.txt (reads 028 to 035 and the bounded comparison; SHA-256 05A8DE22A46884A8FD163A2C67E41080556F647C1D29F35F7EAC5422EEBD1516) and C-009_FINAL_REVIEW_AND_EVIDENCE_REGISTER.md (the final review: corrections, evidence register, checklists). Earlier inputs, stored unchanged in docs/inputs/chatgpt_originals_and_replies/: Claude's Stage 0 answers and inventory, the S1-1 count-only results (searches Q01 to Q34; SHA-256 B5DA521F44BB37031B6091FF94BC0B9258C013EBDD009EC8D7F27556DF7D59BD), the S1-2 checklist and the P-1 results. Not stored: chapters 02 and 03 of the 18-Sep manual, which hold host and path values this project does not reproduce; they are identified by SHA-256 (section 2.1).  
Purpose: factual record of the legacy signal-age expiry: the receiver's check at intake, the processor's check, where their threshold comes from, the stored expires_at, the processor's warning, the tests as text, and the bounded comparison with the 18-Sep manual. It is not a V2 design or a recommendation to revive, repair or tune the legacy system. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1).  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox, SSH detail, or person name other than Rama's is reproduced.

## 1. Purpose and scope

This chapter records the two signal-age checks at the pin: the receiver's check at intake, which answers EXPIRED for the symbol, and the processor's check, which rejects a queued signal as REJECTED_EXPIRED. It also records where their threshold comes from, the stored expires_at, the processor's warning, the test text, the bounded comparison with chapters 02 and 03 of the 18-Sep manual, and what stays unknown.

**Out of scope:** scanner signal-age scoring; PB01 candidate expiry; broker-token and session expiry; order-state expiry; cache TTLs; the GTT lifecycle; the EOD cleanup of stale signals (C-007); and any V2 design decision. Similar terminology does not make these mechanisms the same feature (Rama's Stage 0 decisions OQ-02 (a) and OQ-08 (a), 24-Oct-2026, 11:16 IST).

**Owners of neighbouring steps** (cited, not re-analysed): C-003 (authentication and the request gates), C-004 (validation and the per-symbol order), C-005 (claims, dedup and fingerprint), C-006 (the queue), C-007 (storage, statuses, cleanup and retention), C-008 (the processor's pipeline), C-011 (the EOD route), D-003 and the D topics (screening, gate and retest rules).

## 2. Evidence and confidence rules

This chapter is based on 35 gated reads at the pin, recorded verbatim in its two companion results files: reads 001 to 027 (step S1-2: the configuration, the source, the schema, two documents and greps of four test files) and reads 028 to 035 (S2-19 and S2-20: one bounded grep of each of the two 18-Sep chapters; P-2: six test functions). Every source read matched its pinned blob and ran after the gate's self-test, with the protected-path check at zero; each chapter copy matched its recorded SHA-256 and line count before its grep (the read log in C-009_FINAL_REVIEW_AND_EVIDENCE_REGISTER.md section 3; the companion files, sections 3 and 1). Count-only searches bound the read set: S1-1's 32 searches with 2 positive controls (Q01 to Q34) and S1-2's chapter counts (Q35 to Q39); they print counts, not lines. Placeholders such as <HOSTTOK> and <CLIENTID> in the outputs stand for values the filter hid. The tests were read as text only and never run.

Line references of the form `path:lines` are the evidence package's bounded line references at 970aabf; they are pin-specific and say nothing about un-read surrounding control flow. Line totals are the masked reader's counts, as in the other manuals of the series. A statement attributed to a frozen manual of this series (for example C-004 T-14) is that manual's finding at the same pin, cited and not re-verified here unless this chapter says so (Rama's Stage 0 decision OQ-03 (a)).

Evidence labels used here:

- **FACT (PINNED SOURCE):** supported by source lines read at the pin (the series' FACT (SOURCE-VERIFIED)).
- **FACT (CONFIG):** a value in the repository's config/system_config.yaml; not evidence of a deployed host's configuration (W-7).
- **FACT (TEST TEXT):** what a test's source sets up or asserts; not proof that production behaviour occurred.
- **FACT (TEXT DRIFT):** a comment, docstring or document that differs from the code or the configuration.
- **FACT (DOCUMENT TEXT):** what a document says, about its own tree where it describes one.
- **HISTORICAL:** context from another tree, an earlier time or a recorded account.
- **INTERPRETATION:** a limited reading of the evidence, explicitly not a runtime conclusion.
- **KNOWN DEFECT:** only a defect established by evidence; none is newly established here.
- **UNKNOWN / NOT ESTABLISHED** and **OPEN QUESTION:** the evidence does not establish the point.

The 18-Sep manual describes the testing worktree e7bf477, not the pinned production tree (C-008_manual.md section 14.7). Its line numbers must not be compared with production source line numbers.

### 2.1 Sources

| File | Pinned blob | Lines | Reads | Role |
|---|---|---|---|---|
| `config/system_config.yaml` | `bf742ae5c81c60614370361998e6a102a1064ff7` | 754 | 001, 002 | the queue settings, `expiry_sec` |
| `core/config_loader.py` | `8635bca77531515561ee48cf92677883a5e30782` | 2,517 | 003, 004 | `SignalQueueConfig` |
| `main.py` | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | 4,294 | 005 | the processor's threshold argument |
| `signals/webhook_receiver.py` | `0f15a0ab033529d80d9c14c993a5077690b72871` | 1,208 | 006 to 009 | the intake check, `expires_at` |
| `signals/signal_processor.py` | `7fab0a02a302770ac72bbbd662549d5c091086c1` | 2,704 | 010 to 015 | the processor's check, its reject handler and warning, the re-queue line, the retest docstring |
| `core/schema.sql` | `b46d7489f6ec5e31149dd68957f3b9e45270571e` | 1,655 | 016, 017 | the `expires_at` column |
| `ops_dashboard/backend/readers/db_reader.py` | `f59e4f58fe51e32d151deb5e14322c1bb2cc28b5` | 4,206 | 018 | a SELECT naming `expires_at` |
| `core/exceptions.py` | `1eeac584208493425bef34c4d80b1f78413342e5` | 423 | 025 | `SignalExpiredError` |
| `core/events.py` | `6511bc40fe74413ff5498a5b90cd47fae34dff82` | 366 | 024 | order-state EXPIRED (out of scope) |
| `config/strategies/pb01_breakout_retest.yaml` | `eedf5cac5ed5132665160a1aac9cce30cf4a38ed` | 80 | 023 | a pointer (PB01 expiry rules) |
| `docs/04_db_schema_reference.md` | `0218e152b77e1765fee2aa76564d1c386c05c133` | 371 | 026 | document text |
| `docs/CONFIG_GUIDE.md` | `1e4f7ee174405fbbac753420cea75043fe1ec92f` | 819 | 027 | document text |
| `tests/unit/test_webhook_receiver.py` | `7b377abb85d644f0f3ff7956385224b43f31ed61` | 2,043 | 019, 030 | test text |
| `tests/unit/test_signal_processor.py` | `3ec0ea84392a193d18b992ca88df0da5e6235f67` | 2,501 | 020, 031 | test text |
| `tests/unit/test_fix136_batch9.py` | `20d59c50759e1cd0ef8693be1fdff437707755f9` | 469 | 021, 032 to 035 | test text |
| `tests/unit/test_config_loader.py` | `8f245c35a047794c00ffe21e53c178b5928af890` | 1,420 | 022 | test text |
| 02_REPOSITORY_AND_FILE_MAP.md (18-Sep manual) | SHA-256 8B5AEC74544C7A613CED958F90CA9DEEDB524DB321AE7101B9A577015CBF6D49 | 3,557 | 028 | document text about the testing worktree e7bf477 |
| 03_STARTUP_RUNTIME_AND_SCHEDULER.md (18-Sep manual) | SHA-256 F7D38F2D0F7EA2D61783970B1E8A81FC06D1676AB613EE4DB18178EE5CBC3087 | 321 | 029 | document text about the testing worktree e7bf477 |

The chapters' line counts are newline counts (the reader's header shows one more line).

## 3. Components and source references

| Component | Evidence reference | What the evidence establishes |
|---|---|---|
| Threshold setting | `config/system_config.yaml:124-128` | FACT (CONFIG): `signal_queue.expiry_sec: 600`, with the comment "WR6: 10 min - Chartink triggered_at is scan time, not delivery time" (127). The deployed value is NOT ESTABLISHED. |
| Configuration model | `core/config_loader.py:192-197` | `SignalQueueConfig` forbids extra keys (193) and declares `expiry_sec: int` with no default (196); its comment says "(default 60)" (FACT (TEXT DRIFT)). |
| Receiver threshold | `signals/webhook_receiver.py:679, 721, 900` | `expiry_sec: int = sq_cfg.expiry_sec` (679), passed on in the per-symbol call (721) as the parameter `expiry_sec` (900); C-004 T-14 records the check against `signal_queue.expiry_sec`. |
| Receiver check | `signals/webhook_receiver.py:916-926` | age from `triggered_at` (a naive value taken as IST) to `now_ist()`; if `age_sec > expiry_sec`, it returns `{"symbol": symbol, "status": "EXPIRED"}`. |
| Receiver expires_at | `signals/webhook_receiver.py:960-961, 969, 976` | `expires_at = received_at + timedelta(seconds=expiry_sec)`, as ISO text (960-961), named in the stored row's columns and values (969, 976). Distinct from the age check. |
| Processor threshold | `main.py:3613-3614`; `signals/signal_processor.py:175, 222` | main passes `signal_expiry_sec=app_config.system.signal_queue.expiry_sec` (3614); the constructor's default is 60 (175); the value is kept as `self._signal_expiry_sec` (222). |
| Processor check | `signals/signal_processor.py:914-921` | age from `triggered_at` taken as IST; if `age_sec > self._signal_expiry_sec`, it raises `_PipelineReject("EXPIRED", ...)`. |
| Processor reject handler | `signals/signal_processor.py:1193-1220` | stores `REJECTED_{rej.check}` with the reason (1201), so REJECTED_EXPIRED for this check; the generic reject handling around it is C-008's. |
| Processor warning | `signals/signal_processor.py:1221-1231` | for EXPIRED, and only when a notifier is set: at most one warning per 60.0 s on a monotonic clock, its text including the mode label; a send error is ignored. |
| Stored column | `core/schema.sql:49` | `expires_at TEXT NOT NULL`, with the comment "received_at + signal_expiry_sec". |
| A reader | `ops_dashboard/backend/readers/db_reader.py:826` | a SELECT list naming `s.expires_at`; whether the value decides anything is OQ-C2. |
| Exception class | `core/exceptions.py:360, 366, 368` | `class SignalExpiredError(SignalError)`, documenting `signal_id` and `max_age_seconds`; whether anything raises it is OQ-C1. |

These references are the bounded line references recorded in the evidence package. Do not infer un-read surrounding control flow.

## 4. Receiver-side expiry

### 4.1 Input and decision

**FACT (PINNED SOURCE):** under the comment "WR6: signal expiry", the receiver takes `now = now_ist()`; a `triggered_at` without a time zone is given the IST zone, the comment saying that FIX-022 already makes it IST-aware at parsing and that legacy naive values are handled "defensively"; it computes `age_sec = (now - triggered_at_aware).total_seconds()` and, when `age_sec > expiry_sec`, returns `{"symbol": symbol, "status": "EXPIRED"}` (`signals/webhook_receiver.py:916-926`).

The comparison is a strict greater-than; the evidence does not support replacing it with greater-than-or-equal. How `triggered_at` is parsed is C-004's (T-07: three formats; a time-only value takes the date of `now`; a naive result is set to IST); this chapter adds nothing to it.

**FACT (PINNED SOURCE):** the threshold is the queue setting (`signals/webhook_receiver.py:679`), 600 in the repository configuration (`config/system_config.yaml:127`; section 6).

**INTERPRETATION:** a `triggered_at` later than `now` gives a negative age, which is never greater than a non-negative threshold, so such a symbol passes this check. C-004 T-14 hands this rule to C-009 ("a future time gives a negative age and passes"), and the lines read here are consistent with it. Whether such signals occurred is NOT ESTABLISHED, and so is whether any validator bounds `expiry_sec` (`core/config_validator.py` names the setting on one line in S1-1's counts; not read).

### 4.2 Stored expiry timestamp

**FACT (PINNED SOURCE):** for a symbol that goes on to be stored, the receiver prepares `expires_at = received_at + timedelta(seconds=expiry_sec)` and its ISO text (`signals/webhook_receiver.py:960-961`), which the row's column list and values name (`signals/webhook_receiver.py:969, 976`); C-004 T-15 and C-007 (sections 2.1 and 4.3, PM-01) record the INSERT that stores it with status QUEUED. The schema comment states the same formula (`core/schema.sql:49`), and so does the schema document (`docs/04_db_schema_reference.md:97`; FACT (DOCUMENT TEXT), consistent).

This timestamp uses `received_at`, while both age checks use `triggered_at`: different bases (bookmark BK-23; C-007 K-04). An EXPIRED symbol returns before these lines and leaves no row (C-004 section 6.4; C-005 section 5; C-007 section 4.3, PM-01), so the stored value exists only on stored rows. The evidence does not establish that the column is later read to make an expiry decision (OQ-C2, section 9).

### 4.3 Output and downstream hand-off

The receiver's result for the symbol is `{"symbol": symbol, "status": "EXPIRED"}` (`signals/webhook_receiver.py:926`). C-009's reads do not include the response construction; the frozen C-004 manual records it at the pin: the per-symbol results form the batch answer, 503 if any result is QUEUE_FULL or STORE_ERROR and otherwise 200, with the body {accepted, rejected, results} (T-16); an EXPIRED symbol leaves no row and no log line and is counted in the 200's rejected (T-14; section 6.4); the request's webhook_audit row stores counts only (T-17). R-1's test text asserts the 200 and `results[0].status == "EXPIRED"`, consistent with that record (section 7; not run).

C-009's own reads place the expiry check (925) before the fingerprint (952-954) and the row preparation (957-961) (`signals/webhook_receiver.py:916-926, 949-961`). The other steps are recorded at the pin by the frozen manuals: the request gates and the request-level parsing before the per-symbol loop (C-003 section 7; C-004 section 5); per symbol, the alias and the exclusion, then the empty-symbol and price checks, then the expiry check, then the in-flight claim, the TTL dedup, the fingerprint, the INSERT and the queue put (C-004 section 6.1, T-10 to T-15; C-005 sections 3 and 5). C-009 does not re-verify them. On the intraday path, repeats after authentication are limited by this check, the dedup cache and the fingerprint (C-003 section 6.5, an INTERPRETATION / AUDIT LEAD there, confirmed at the pin in C-005 section 13.1); the EOD route has no expiry check (C-003 section 18, L-01; C-011 owns the route).

## 5. Processor-side expiry

### 5.1 Input and decision

**FACT (PINNED SOURCE):** under the comment "Signal age (defense-in-depth; receiver also checks)", the processor gives `triggered_at` the IST zone (`replace(tzinfo=ist_timezone())`), computes `age_sec` against `now` (set before the lines read) and, when `age_sec > self._signal_expiry_sec`, raises `_PipelineReject("EXPIRED", f"Signal age {age_sec:.1f}s > expiry {self._signal_expiry_sec}s")` (`signals/signal_processor.py:914-921`). C-008 places this stage, ST-06, after the PROCESSING write (ST-03), the kill-switch check (ST-04) and the global entry window (ST-05) (C-008_manual.md section 4.1; B-008 section 3).

**FACT (PINNED SOURCE):** the reject handler calls `update_signal_status(signal_id, f"REJECTED_{rej.check}", rej.reason)` (`signals/signal_processor.py:1201`), and this reject's check name is "EXPIRED" (`signals/signal_processor.py:919, 1221`; `_PipelineReject(check, reason)`, C-008 section 4.1), so the stored status is REJECTED_EXPIRED with the age text as the reason; C-007 (S-05, D-06) and C-008 (ST-06) record the same. Around it the handler logs the reject (`signals/signal_processor.py:1194-1196`), counts it (`signals/signal_processor.py:1199-1200`), hands a P2_REJECT item to the evidence capture (`signals/signal_processor.py:1202-1211`), releases a reservation if one is held (`signals/signal_processor.py:1212-1217`) and adds it to the statistics (`signals/signal_processor.py:1218-1220`): generic reject handling, which is C-008's (section 4.1). The test text expects the same status (R-2); the tests were not run and no host's database or log survives, so the observed runtime status is NOT ESTABLISHED.

**FACT (PINNED SOURCE):** a re-queued item carries the original `triggered_at` (`signals/signal_processor.py:1515`); C-008 (RQ-04, INTERPRETATION) reads this as a retry that reaches ST-06 after 600 s ending REJECTED_EXPIRED. By its docstring, the retest resume path re-checks the kill switch, the market window, the strategy control and the governor, "NOT the :607 60s expiry", calling itself "a park-and-resume" (`signals/signal_processor.py:2376-2384`; C-008 section 9.2 and D-04); its body was not read here.

### 5.2 Warning

**FACT (PINNED SOURCE):** when the reject's check is "EXPIRED" and the processor has a notifier, it reads a monotonic clock; if more than 60.0 s have passed since the last such warning, it records the time and then tries `send_warning(f"[{self._mode}] Signal EXPIRED: {symbol} age={rej.reason}")`; any exception from the send is caught and ignored (`signals/signal_processor.py:1221-1231`). **INTERPRETATION:** the time is recorded before the send, so a failed send also starts the 60-second window, and the limit belongs to the processor object (an instance attribute).

main passes a notifier among the processor's arguments (C-008_manual.md section 2.1, W-02); whether a notifier was configured on a host, and whether any warning was delivered, is NOT ESTABLISHED. Do not generalise this into a guarantee that a notification is always delivered.

## 6. Configuration and thresholds

**FACT (CONFIG):** `signal_queue.expiry_sec: 600` (`config/system_config.yaml:127`), with the comment "WR6: 10 min - Chartink triggered_at is scan time, not delivery time"; C-002 (section 13.5) records that comment as a claim about Chartink that neither the configuration nor the code establishes.

**FACT (PINNED SOURCE):** both checks take that setting at the pin: the receiver directly (`signals/webhook_receiver.py:679`), the processor through main (`main.py:3613-3614`, with the comment "Use same expiry as webhook_receiver (config signal_queue.expiry_sec)"; C-008 CF-01). With the repository configuration, both thresholds are 600 s.

**FACT (PINNED SOURCE):** the processor's constructor default is 60 (`signals/signal_processor.py:175`); it applies only where the processor is built without the argument, and main passes it (C-008 CF-01). **FACT (TEST TEXT):** a module-level helper of the processor tests (lines 355 to 427, by the P-1 structure scan) has `signal_expiry_sec=60` on its header lines (`tests/unit/test_signal_processor.py:372`) and passes the value on (`tests/unit/test_signal_processor.py:421`).

**FACT (TEXT DRIFT):** 60 appears in text that describes the expiry: the model's comment "(default 60)" (`core/config_loader.py:196`), although the field has no default (C-002 section 18, K-10; C-006 section 3); a configuration comment, "short vs the 60s signal expiry" (`config/system_config.yaml:567`); and the retest docstring's "NOT the :607 60s expiry" (`signals/signal_processor.py:2378`), while the check is at 914-921 at the pin (C-008 D-04). The configuration guide agrees with the configuration: "`expiry_sec: 600` drops signals older than 10 min." (`docs/CONFIG_GUIDE.md:363`). **HISTORICAL:** A-009 (section 6) records a usage-tracker comment in core/config_validator.py, written after "2026-04-28 paper testing" found hardcoded defaults bypassing config: "signal_expiry_sec: config=600, code default=60"; whether any host ever ran the default is NOT ESTABLISHED. Bookmark BK-17.

**FACT (TEST TEXT):** the receiver tests' helper sets `expiry_sec=expiry` (`tests/unit/test_webhook_receiver.py:72`); the expiry case uses 60 (R-1), and three other tests use 86400, "large enough to not expire during test" (`tests/unit/test_webhook_receiver.py:1578, 1636, 1683`); the processor's expiry case uses 30 (R-2); the batch-9 tests use 600 and 60 (R-3 to R-6); a configuration fixture of the config-loader tests sets 60 (`tests/unit/test_config_loader.py:75`). These are test inputs and must not be presented as the deployed value.

The deployed threshold is **UNKNOWN / NOT ESTABLISHED**: both legacy VMs were deleted, and with them their configuration, databases and logs (V2_DESIGN_LOG section 1, 2026-10-05; C-004 section 12).

## 7. Test evidence (text only)

No tests were run. The following describes test source, not a test result.

| Evidence | Test text describes | What can be concluded |
|---|---|---|
| R-1, `tests/unit/test_webhook_receiver.py:940-950` | `test_expired_signal_returns_expired`: a receiver with expiry 60; a payload whose `triggered_at` is 120 s old, posted to /webhook/gap_go_long; asserts HTTP 200 and `results[0].status == "EXPIRED"`. | The test is written to assert the per-symbol EXPIRED of section 4.1 and the 200 of C-004 T-16. Pass or fail is unknown. |
| R-2, `tests/unit/test_signal_processor.py:572-582` | `test_expired_signal_rejects`: a queued row; a processor with `signal_expiry_sec=30`; one signal aged 90 s; asserts the stored REJECTED_EXPIRED. | Written to assert the status of section 5.1. The helpers it calls were not read. Pass or fail is unknown. |
| R-3, `tests/unit/test_fix136_batch9.py:120-126` | `test_expires_at_computation`: computes `received + timedelta(seconds=expiry_sec)` with 600 and compares it with a literal time. | Arithmetic inside the test; it calls no receiver code, so it does not exercise `signals/webhook_receiver.py:960`. |
| R-4, `tests/unit/test_fix136_batch9.py:128-136` | `test_signal_processor_rejects_expired`: imports `_PipelineReject` (130), which the lines read do not use; computes an age of about 700 s and asserts that it exceeds 60. | Arithmetic only in the lines read; it does not call the processor's check that its name describes. |
| R-5, `tests/unit/test_fix136_batch9.py:138-145` | `test_fresh_signal_not_expired`: computes an age of about 5 s and asserts that it is at most 600. | Arithmetic only; no receiver or processor code. |
| R-6, `tests/unit/test_fix136_batch9.py:154-158` | `test_parity_same_expiry_paper_and_live`: docstring "Expiry logic is mode-agnostic", the same check in paper and live; builds `SignalQueueConfig(capacity=300, backpressure_pct=0.8, expiry_sec=600, warning_pct=0.6)` and asserts `cfg.expiry_sec == 600`. | Only that the model stores the value given; the docstring's paper/live claim is not exercised by the body (section 11). |

**Do not state that these tests pass.** The other test lines the greps printed are set-up (`tests/unit/test_webhook_receiver.py:72, 1578, 1636, 1683`; `tests/unit/test_signal_processor.py:372, 421`; `tests/unit/test_config_loader.py:75`) or a data row naming REJECTED_EXPIRED (`tests/unit/test_fix136_batch9.py:176`). Bookmark BK-47 (R-3 to R-6).

## 8. Comparison with the 18-Sep manual

The 18-Sep chapters describe the testing worktree e7bf477, not the pinned production source: a difference is a difference between two documents about two trees, and their line numbers are not comparable with the pin's (C-008 section 14.7). Terms: CONSISTENT, PARTIAL, NOT COMPARABLE, OUT OF SCOPE; the IDs are those of the comparison companion file, section 5.

- **CC-01** (chapter 02 line 972): the receiver "enforces queue backpressure and per-signal expiry": CONSISTENT at statement level with section 4.1.
- **CC-02** (line 977): the per-symbol order "price validation, expiry, atomic in-flight claim, TTL dedup, fingerprint, INSERT into signals, queue push": PARTIAL on C-009's own reads (expiry before the fingerprint and the row preparation; the other positions are NOT ESTABLISHED in this evidence batch). The frozen C-004 and C-005 manuals record the same order at the pin, with an empty-symbol check before the price check (section 4.3; C-004 T-12).
- **CC-03** (lines 960 and 965): an EXPIRED warning "rate-limited to one per 60s", with the mode in its text: CONSISTENT at statement level with section 5.2; the chapter's line numbers (1277-1284, 1281) are NOT COMPARABLE with the pin's.
- **C-008 CH-10** (chapter 02 line 952, "60s-age"; C-008's cross-check, frozen with DR-061): PARTIAL; the check and its place agree, the figure differs from the 600 s that main passes at the pin. C-009's grep did not match that line (its terms were expir, signal_age and max_age).
- **CC-04** (line 912): a screening step scoring signal age (up to 30 s 1.0, up to 60 s 0.5, older 0.0): NOT COMPARABLE. INTERPRETATION: a scoring step, not the expiry rejection; a pointer for D-003 (bookmark BK-46).
- **CC-05** (lines 2954, 2959 and 2961): the 15:50 cleanup sets prior-day PROCESSING signals to EXPIRED and prunes old ones: C-007's (S-19, RT-01, RT-02); not compared here.
- **CC-06** (line 378): SignalExpiredError among the classes whose live raisers are unknown: PARTIAL; the class exists at the pin, and its raise sites are OQ-C1.
- **CC-07** (line 2607): the PB01 entry poller expires PENDING rows past entry_end: NOT COMPARABLE. INTERPRETATION: a time limit on V3 PB01 candidate rows, not the signal-age rule (the strategy file's comment calls its expiry rules "V3 DECISION CONTENT", `config/strategies/pb01_breakout_retest.yaml:20`); a pointer (BK-46).
- **CC-08** (chapter 03 lines 102, 174, 179 and 194): all four concern the broker token: OUT OF SCOPE. No claim is made about unsearched wording elsewhere in that chapter.
- **CC-09** (the 26 other lines of chapter 02): closure attribution, broker authentication and token expiry, order states, the rate-limit freeze, the zone cache TTL, the CHECK1 deferral, GTT, the health token check and the login token file: OUT OF SCOPE.

## 9. Open questions and bookmarks

### OQ-C1 - SignalExpiredError (OPEN QUESTION; bookmark BK-44)

**FACT (PINNED SOURCE):** core/exceptions.py defines `class SignalExpiredError(SignalError)`, whose docstring names `signal_id` ("the signal that expired") and `max_age_seconds` ("configured expiry threshold") (`core/exceptions.py:360, 366, 368`). Whether any code raises it at the pin is **UNKNOWN / NOT ESTABLISHED**: the receiver's EXPIRED is a returned status (`signals/webhook_receiver.py:926`) and the processor's is a `_PipelineReject` (`signals/signal_processor.py:918-919`), and neither is this class. The testing-tree chapter lists its live raisers as unknown too (CC-06).

### OQ-C2 - readers of signals.expires_at (OPEN QUESTION; bookmark BK-45)

**FACT (PINNED SOURCE):** the dashboard reader's SELECT list names `s.expires_at` (`ops_dashboard/backend/readers/db_reader.py:826`). Whether any reader uses the column to decide expiry is **UNKNOWN / NOT ESTABLISHED**: selecting a field does not show that it drives a decision. What bounds the question: S1-1 counts the name in 8 runtime Python files at the pin: the receiver (4 lines, the writer), the dashboard reader (this line), scripts/zerodha_login.py (4 lines; B-004 section 4 records a token-file field of that name in that script) and five scripts not inspected (scripts/copy_gate.py, scripts/healthcheck_server.py, scripts/request_copy.py, scripts/security_monitor.py and scripts/t2_cnc_gtt_realtest.py; deferred and excluded by Rama's decision of 24-Oct-2026, 12:10 IST). INTERPRETATION: a reader could also receive the column without naming it. C-007 records that the dashboard classifies rows as expired by their status, REJECTED_EXPIRED or EXPIRED (C-007_manual.md section 8). Do not infer an answer from the class name, the selected column, comments, test names or documentation.

### Existing bookmarks (W-9; notes added at this freeze, none closed)

- **BK-23** (C-007 C7/BM-05): expires_at is stored from received_at while both age checks measure from triggered_at (C-007 K-04). Re-verified at the pin (sections 4.1, 4.2 and 5.1); the test text uses both bases too (R-3 computes expires_at from received_at; R-1, R-4 and R-5 measure age from triggered_at). OPEN.
- **BK-17** (C-006 BM-08): text drift, including the expiry default 60 in the model's comment against the 600 of the configuration. C-009 adds the other 60s of section 6 and the tests' 60 and 30. OPEN.
- **BK-18** (VP-59): VP-59 is not ratified at C-009 (Rama's Stage 0 decision OQ-04 (a)). OPEN.

These are recorded source, test and text differences. This evidence does not establish a runtime defect or which threshold a host ran. No new KNOWN DEFECT is established.

### New bookmarks

- **BK-46:** time rules on signals other than this expiry, pointed to and not read at the pin: the screener's signal-age score (CC-04; the configuration guide lists a "signal_age 10" weight, `docs/CONFIG_GUIDE.md:326`), the PB01 candidate expiry (CC-07) and the retest resume without an age check (section 5.1). For D-003, D-014 and D-015.
- **BK-47:** the batch-9 tests named for the expiry logic (R-3 to R-6) compute ages and times inside the test or check the model, and call neither the receiver's nor the processor's check (section 7). For N-001.

## 10. End-to-end view limited to established evidence

```text
Chartink POST -> receiver: request gates and parsing (C-003, C-004)
  per symbol: alias, exclusion, empty symbol, price (C-004 T-10 to T-13)
    -> AGE CHECK (signals/webhook_receiver.py:916-926)
         age_sec = now_ist() - triggered_at (a naive value taken as IST)
         age_sec > expiry_sec (signal_queue.expiry_sec; 600 in the repository configuration)
           yes -> {"symbol", "status": "EXPIRED"}: no row, no log line,
                  counted in the batch answer's rejected (C-004 T-14, T-16)
           no  -> in-flight claim, TTL dedup, fingerprint (C-005)
                  -> row prepared: expires_at = received_at + expiry_sec (960-961)
                  -> INSERT, status QUEUED (C-007) -> queue (C-006)
                       expires_at: stored; a reader deciding expiry from it: OQ-C2

SignalProcessor, per dequeued item (C-008)
  PROCESSING (ST-03) -> kill switch (ST-04) -> global entry window (ST-05)
    -> AGE CHECK, ST-06 (signals/signal_processor.py:914-921)
         age_sec = now - triggered_at, taken as IST
         age_sec > _signal_expiry_sec (main passes signal_queue.expiry_sec)
           yes -> _PipelineReject("EXPIRED")
                  -> status REJECTED_EXPIRED, the age text as reason (1201)
                  -> only if a notifier is set: at most one warning per 60.0 s,
                     "[mode] Signal EXPIRED: ...", send errors ignored (1221-1231)
           no  -> later stages (C-008)
  a re-queued retry carries the original triggered_at (1515; C-008 RQ-04)
  the retest resume has no age check, by its docstring (2376-2384; C-008 D-04)
```

This diagram shows only the checks supported by the evidence and by the frozen manuals cited. It is not a claim that every signal meets both checks, nor a full queue or persistence trace.

## 11. Paper/live differences

The batch-9 test's docstring claims that the expiry logic is mode-agnostic, but its body only constructs a configuration object and checks that `expiry_sec` equals 600 (R-6). In the lines read, neither check branches on a mode; the processor uses its mode label only in the warning's text (`signals/signal_processor.py:1228`). That bounds the lines read only: paper/live parity of signal expiry is **UNKNOWN / NOT ESTABLISHED**. HISTORICAL: paper mode is legacy context.

## 12. Failure, recovery and operational behaviour

At intake an EXPIRED symbol leaves no signals row and no log line; only the batch answer's counts and the request's webhook_audit counts show it (C-004 T-14 and T-17; C-004 section 12). At the processor a REJECTED_EXPIRED row keeps its status and reason, with an INFO log line (`signals/signal_processor.py:1194-1196`), a P2_REJECT evidence item (`signals/signal_processor.py:1202-1211`; whether evidence was recorded is C-008's) and the rate-limited warning (section 5.2). A re-queued retry keeps its original age (section 5.1). The 15:50 cleanup's EXPIRED is a different mechanism with the same status name: it changes earlier-dated PROCESSING rows (C-007 S-19, RT-01).

Not established by this evidence: the HTTP layer beyond C-004's record, the processor's handling of a failed status write (C-008 K-05), database transaction semantics, how often either check fired, and any delivered alert. No live service, VM, database, broker, webhook or Chartink endpoint was contacted; no legacy code or test was executed; no legacy source was changed.

Known limits of the read set: the five scripts above (OQ-C2) not inspected (Rama, 12:10 IST); docs/locked_decisions.yaml not read; tests/integration/test_full_signal_flow.py not read (one quoted EXPIRED in S1-1's counts); the screener, gate and retest code not read (OQ-02 (a)); no record read (OQ-06 (a)); the replay and load-test tools left out (OQ-07 (a)); chapter 01 of the 18-Sep manual not compared.

## 13. Evidence ledger

| ID | Classification | Evidence / conclusion |
|---|---|---|
| F-01 | FACT (PINNED SOURCE) | Receiver: age from `triggered_at` (a naive value taken as IST) to `now_ist()`; `age_sec > expiry_sec` returns the symbol's EXPIRED (`signals/webhook_receiver.py:916-926`; read 008). |
| F-02 | FACT (PINNED SOURCE) | Receiver: `expires_at = received_at + expiry_sec`, named in the stored row (`signals/webhook_receiver.py:960-961, 969, 976`; reads 007, 009). |
| F-03 | FACT (PINNED SOURCE) | Processor: `age_sec > _signal_expiry_sec` raises `_PipelineReject("EXPIRED")` (`signals/signal_processor.py:914-921`; read 012). |
| F-04 | FACT (PINNED SOURCE) | Processor: the reject is stored as `REJECTED_{check}`, REJECTED_EXPIRED here (`signals/signal_processor.py:1201`; read 013). |
| F-05 | FACT (PINNED SOURCE) | Processor: the EXPIRED warning only with a notifier, at most one per 60.0 s, send errors ignored (`signals/signal_processor.py:1221-1231`; read 013). |
| F-06 | FACT (PINNED SOURCE) | Wiring: the receiver takes the queue setting (`signals/webhook_receiver.py:679`; read 006); main passes it to the processor (`main.py:3613-3614`; read 005); the constructor's default is 60 (`signals/signal_processor.py:175`; read 010). |
| F-07 | FACT (PINNED SOURCE) | A re-queued item carries the original `triggered_at` (`signals/signal_processor.py:1515`; read 014); the retest docstring excludes the age check (`signals/signal_processor.py:2376-2384`; read 015). |
| F-08 | FACT (PINNED SOURCE) | Schema: `expires_at TEXT NOT NULL`, comment "received_at + signal_expiry_sec" (`core/schema.sql:49`; read 016). |
| F-09 | FACT (PINNED SOURCE) | The dashboard reader's SELECT names `s.expires_at` (`ops_dashboard/backend/readers/db_reader.py:826`; read 018). |
| F-10 | FACT (PINNED SOURCE) | `SignalExpiredError` is defined (`core/exceptions.py:360`; read 025). |
| C-01 | FACT (CONFIG) | `signal_queue.expiry_sec: 600` (`config/system_config.yaml:127`; read 001). |
| C-02 | FACT (PINNED SOURCE) | `SignalQueueConfig.expiry_sec: int`, no default, extra keys forbidden (`core/config_loader.py:192-197`; read 003). |
| D-01 | FACT (TEXT DRIFT) | 60 in the model's comment, a configuration comment and the retest docstring (`core/config_loader.py:196`; `config/system_config.yaml:567`; `signals/signal_processor.py:2378`; reads 003, 002, 015). |
| D-02 | FACT (DOCUMENT TEXT) | The two repository documents agree with the code and the configuration (`docs/04_db_schema_reference.md:97`; `docs/CONFIG_GUIDE.md:363`; reads 026, 027). |
| D-03 | FACT (DOCUMENT TEXT) | Chapters 02 and 03 of the 18-Sep manual (testing tree): section 8, CC-01 to CC-09 (reads 028, 029). |
| T-01 | FACT (TEST TEXT) | R-1 asserts 200 and the per-symbol EXPIRED for a 120 s old signal with expiry 60 (read 030); not run. |
| T-02 | FACT (TEST TEXT) | R-2 asserts REJECTED_EXPIRED for a 90 s old signal with threshold 30 (read 031); not run. |
| T-03 | FACT (TEST TEXT) | R-3 to R-6 compute times and ages inside the test or check the model; they call neither check (reads 032 to 035); not run. |
| H-01 | HISTORICAL | The 18-Sep chapters describe the testing worktree e7bf477, not the pinned production commit. |
| H-02 | HISTORICAL | The usage tracker's account of a 2026-04-28 paper-testing finding, "signal_expiry_sec: config=600, code default=60" (A-009 section 6). |
| I-01 | INTERPRETATION | A `triggered_at` later than now gives a negative age and passes both checks (section 4.1; C-004 T-14). |
| I-02 | INTERPRETATION | A failed warning send still starts the 60-second window (section 5.2). |
| U-01 | UNKNOWN | The deployed signal expiry threshold. |
| U-02 | UNKNOWN | Whether `SignalExpiredError` is raised anywhere at the pin (OQ-C1, BK-44). |
| U-03 | UNKNOWN | Whether any reader uses `signals.expires_at` to decide expiry (OQ-C2, BK-45). |
| U-04 | UNKNOWN | Runtime pass or fail of all six tests; they were not run. |
| U-05 | UNKNOWN | Paper/live parity of signal expiry. |
| U-06 | UNKNOWN | How often either check fired, and whether any warning was delivered. |
| K-01 | KNOWN DEFECT | None newly established by this evidence. |

## 14. Cross-references and reconciliation with the frozen manuals

C-009's reads re-verify these frozen statements at the pin. None conflicts with this chapter, so no erratum is proposed.

| Frozen statement | Result |
|---|---|
| A-003 section 7: the production configuration has a signal queue with intraday expiry_sec=600 | Consistent (`config/system_config.yaml:127`), as a repository value (W-7). |
| B-008 section 3: the processor's order kill (906), global window (911), expiry (917) | Consistent with the check at `signals/signal_processor.py:917`. |
| B-009 section 5: age over 600 s gives EXPIRED at the webhook (a response only) and REJECTED_EXPIRED at the processor; the EXPIRED warning at most once a minute | Consistent, with the repository's 600 (`signals/webhook_receiver.py:916-926`; `signals/signal_processor.py:914-921, 1221-1231`). |
| C-002 section 13.5: more than 600 s old on receipt is EXPIRED, and the comment's claim about Chartink is not established; section 18, K-10: the "default 60" comment | Consistent (`signals/webhook_receiver.py:916-926`; `config/system_config.yaml:127`; `core/config_loader.py:196`). |
| C-004 T-14: the expiry check after the price check and before the claims, against signal_queue.expiry_sec; "C-009 owns the rule" | Consistent; the rule is section 4.1, the future-time reading I-01. |
| C-005 section 5: an expired signal acquires no claim, sets no cache entry and leaves no row | Consistent with the return at `signals/webhook_receiver.py:926`. |
| C-006 sections 2.1 and 3: 600 in the configuration, no field default, the "default 60" comment | Consistent (`config/system_config.yaml:127`; `core/config_loader.py:192-197`). |
| C-007 section 2.1 and K-04: expires_at = received_at + expiry_sec, while the age checks measure from triggered_at | Consistent (`signals/webhook_receiver.py:960`; `core/schema.sql:49`; F-01, F-03). |
| C-008 CF-01, ST-06, RQ-04 and D-04 | Consistent (`main.py:3613-3614`; `signals/signal_processor.py:175, 914-921, 1221-1231, 1515, 2378`). |

Cross-references: C-003 (authentication; the replay limits, section 6.5; the EOD route without an expiry check, L-01); C-004 (field and symbol validation; the response and the audit row, T-16 and T-17); C-005 (claims, dedup and fingerprint after the check); C-006 (the queue and its settings); C-007 (storage, statuses, the 15:50 cleanup and retention); C-008 (the processor's stages, reject handling, re-queue and retest paths; its CH-10 comparison); C-011 (the EOD route); D-003, D-014 and D-015 (BK-46); N-001 (BK-47).

## 15. Revision 1 and freeze (2026-10-24)

Revision 0 is ChatGPT's draft, stored unchanged in docs/inputs/manuals/. Revision 1 makes the corrections listed with their evidence in C-009_FINAL_REVIEW_AND_EVIDENCE_REGISTER.md section 2; none reverses a finding of revision 0:

1. R-1: the header, status and companions in the form of the other frozen manuals; section 2.1 (sources with pinned blobs); ASCII typography; labels in the form FACT (PINNED SOURCE); revision 0's section 15 (review status) replaced by this section.
2. R-2: section 2 names the whole evidence base (reads 001 to 035 in both companion results files, and the count-only searches), not only reads 028 to 035.
3. R-3: the labels FACT (CONFIG), FACT (TEXT DRIFT) and OPEN QUESTION, and the rule for citing frozen manuals.
4. R-4: section 3's rows for the setting, the two thresholds' wiring, the schema column, the reader and the exception class; "lines 1201 onward" made line 1201; the warning's range to 1231.
5. R-5: section 4.1's time-zone handling, the threshold's source and the future-time reading (C-004 T-14).
6. R-6: section 4.2's stored row and the no-row outcome of an EXPIRED symbol.
7. R-7: section 4.3's response and per-symbol order, recorded at the pin by C-003 to C-005, cited where revision 0 called them not established "at the pinned source".
8. R-8: section 5.1's REJECTED_EXPIRED as a source fact, the stage position, the generic reject handling (C-008), the re-queue line and the retest docstring.
9. R-9: section 5.2's ignored send error, the time recorded before the send, and the notifier passed by main.
10. R-10: section 6's repository value 600 and its wiring to both checks, the constructor default, the text drift, the documents, A-009's historical note and all the test inputs.
11. R-11: section 7's test names, R-2's age of 90 s, R-4's unused import and the set-up lines.
12. R-12: section 8's labels (INTERPRETATION for CC-04 and CC-07), CC-06, CC-09 and C-008's CH-10, and CC-02 with the frozen order.
13. R-13: section 9's evidence for OQ-C1 and OQ-C2 (reads 025 and 018, not "the approved comparison" and "reportedly"), OQ-C2's bounds, BK-23 as recorded (the source's two bases, not test text), BK-17 and BK-18, the bookmark numbers BK-44 to BK-47.
14. R-14: the diagram (section 10), the mode label (section 11), the operational record and known limits (section 12), the ledger (section 13) and the reconciliation with the frozen manuals (section 14).

**Freeze.** Prepared under W-4 (DR-017), W-9 and W-10 (Rama, 24-Oct-2026, 14:06 IST: no approval step between finalisation and storage), on ChatGPT's reconciliation and its finalisation prompt, which asked for the final package without a further approval question. The three inputs, unchanged since Rama sent them at 14:06 IST:

| File | Lines | SHA-256 | Stored |
|---|---|---|---|
| C009_Legacy_Manual_Draft_RESHARE_24-Oct-2026.md (revision 0) | 208 | A38EC20146E8E05B93DD130F2876BB00797CDAD693842AF95E347E67738BB85F | docs/inputs/manuals/, unchanged; this file is its frozen revision 1 |
| C009_Reconciliation_Review_RESHARE_24-Oct-2026.txt | 75 | 7307D1E59AED4F219D89D7797AA5C8C0A21798689A40D5BF8F2648812EF11751 | docs/inputs/reconciliation/, unchanged |
| C009_Claude_Finalization_Prompt_RESHARE_24-Oct-2026.txt | 46 | 99B8E395F16317EA63B91A293A82DFA1AD46E11C52749CA7EF41D9A252ED6A65 | docs/inputs/reconciliation/, unchanged |

The freeze takes effect when Rama runs PART 2 of C-009_PUSH_COMMANDS_24-OCT-2026.txt; Claude then confirms the stored files on GitHub (C-009_review_notes.md section 7). Claude stores, commits and pushes nothing.

**Legacy remains DEAD / REFERENCE ONLY. No V2 decision is made here.**
