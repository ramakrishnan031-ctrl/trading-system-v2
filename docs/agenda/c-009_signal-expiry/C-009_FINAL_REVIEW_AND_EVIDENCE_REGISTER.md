# C-009 SIGNAL EXPIRY: FINAL REVIEW AND EVIDENCE REGISTER

Status: FINAL 2026-10-24. Companion to C-009_manual.md revision 1 (FROZEN, DR-062), stored unchanged beside it in docs/agenda/c-009_signal-expiry/. Prepared by Claude as deliverable B of ChatGPT's finalisation prompt: the reviewed facts, the unknowns, the historical distinctions and the corrections made to revision 0.  
Baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb; V2 main b720bd7635ca448fa35c828cfd72bfa722b0e24e for the frozen manuals cited.  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox, SSH detail, or person name other than Rama's is reproduced.

Inputs reviewed (each checked by SHA-256 when received from Rama at 14:06 IST on 24-Oct-2026, or as delivered by Claude):

| File | Lines | SHA-256 | Role |
|---|---|---|---|
| C009_Legacy_Manual_Draft_RESHARE_24-Oct-2026.md | 208 | A38EC20146E8E05B93DD130F2876BB00797CDAD693842AF95E347E67738BB85F | ChatGPT's draft, revision 0 (the prompt calls it C-009_SIGNAL_EXPIRY_LEGACY_MANUAL_DRAFT_24-OCT-2026.md) |
| C009_Reconciliation_Review_RESHARE_24-Oct-2026.txt | 75 | 7307D1E59AED4F219D89D7797AA5C8C0A21798689A40D5BF8F2648812EF11751 | ChatGPT's reconciliation and review (the prompt calls it C-009_CHATGPT_RECONCILIATION_AND_REVIEW_24-OCT-2026.txt) |
| C009_Claude_Finalization_Prompt_RESHARE_24-Oct-2026.txt | 46 | 99B8E395F16317EA63B91A293A82DFA1AD46E11C52749CA7EF41D9A252ED6A65 | ChatGPT's finalisation prompt |
| C-009_CLAUDE_STAGE1_S1-2_READ_RESULTS_24-OCT-2026.txt | 456 | D707911FE762ED713A9FDEA34A4190D1806C2A03BED7F78DB9B883233D1C055A | reads 001 to 027, verbatim (Claude, 13:05 IST) |
| C-009_CLAUDE_STAGE1_S2-19_S2-20_P2_RESULTS_AND_COMPARISON_24-OCT-2026.txt | 286 | 05A8DE22A46884A8FD163A2C67E41080556F647C1D29F35F7EAC5422EEBD1516 | reads 028 to 035 and the comparison (Claude, 13:55 IST); the prompt's supplied results file |
| C-009_CLAUDE_STAGE1_S1-1_COUNT_ONLY_RESULTS_24-OCT-2026.txt | 1,080 | B5DA521F44BB37031B6091FF94BC0B9258C013EBDD009EC8D7F27556DF7D59BD | count-only searches Q01 to Q34 (Claude, 11:53 IST) |

## 1. Review: scope, method and result

- Scope: every statement of revision 0, against the saved outputs of the 35 logged reads (section 3), the S1-1 and S1-2 count-only searches, and the frozen manuals of the series at V2 main b720bd7. No new source read, search or chapter read was made, no question was put to Rama, and the read lock stayed engaged throughout (cread_c009.py SHA-256 2537a5d76e609a716e9d3599bd7c81feaaf8fbfe0c38d27cec814c16a1958b65, locked at 13:49:52 IST), as the prompt and the reconciliation (section 1) require.
- Method: each claim was matched to the read that printed its lines (section 4); each statement that something is "not established" was checked against the frozen manuals of the series at the same pin, which C-009 cites without re-reading (Rama's Stage 0 decision OQ-03 (a)); the two checklists were answered (section 6).
- Result: no finding of revision 0 is reversed, its unknowns stay unknown, and OQ-C1 and OQ-C2 stay open. Fourteen corrections were needed (section 2): five material (R-2, R-7, R-8, R-10, R-13), the others completeness, transcription or form. They were applied without a further approval question, as the prompt and Rama's rule W-10 ask.

## 2. Corrections made (revision 0 to revision 1)

| ID | Revision 0 | Correction in revision 1 | Evidence |
|---|---|---|---|
| R-1 | Header "Draft for Claude factual review"; no sources table; 54 non-ASCII characters (40 dashes, 14 curly quotes); labels "FACT - PINNED SOURCE"; section 15, review status | Header, status and companions in the form of the other frozen manuals; section 2.1, the sources with their pinned blobs; ASCII typography; labels written FACT (PINNED SOURCE) and so on; section 15, revision and freeze. No statement changed by it. | the read log (section 3); C-006 R-11, the precedent |
| R-2 (material) | Section 2: "The primary evidence package is C-009_CLAUDE_STAGE1_S2-19_S2-20_P2_RESULTS_AND_COMPARISON_24-OCT-2026.txt. It records eight bounded reads, reads 028-035" | Section 2 names both companion results files, all 35 reads and the count-only searches. Every source line revision 0 cites was printed by reads 003, 008, 009, 012 and 013, recorded in the S1-2 results file, which the reconciliation's basis line does not name. | reads 003, 008, 009, 012, 013 |
| R-3 | Labels without FACT (CONFIG), FACT (TEXT DRIFT) or OPEN QUESTION | The three labels added (needed by R-10 and section 9); the rule for citing frozen manuals stated. | Stage 0 decision OQ-03 (a) |
| R-4 | Section 3: six rows; "lines 1201 onward"; the warning at "lines 1221-1229" | Rows added for the setting (read 001), the receiver's threshold (006), the processor's threshold (005, 010), the stored column (016), the reader (018) and the exception class (025); the status write is line 1201 alone; the warning runs to 1231, where the send's exception is ignored. | reads 001, 005, 006, 010, 013, 016, 018, 025 |
| R-5 | Section 4.1: the check and the strict comparison only | The IST handling of a naive `triggered_at` and `now_ist()`; the threshold's source; the INTERPRETATION that a future time passes, which C-004 T-14 hands to C-009 ("C-009 owns the rule (a future time gives a negative age and passes; not C-004's)"). | reads 006, 008; C-004 T-14 |
| R-6 | Section 4.2: the formula and its different basis | The stored row's columns and values naming `expires_at`, the schema comment and document; an EXPIRED symbol leaves no row. | reads 007, 016, 026; C-004 section 6.4; C-005 section 5; C-007 PM-01 |
| R-7 (material) | Section 4.3: the HTTP 200 and the results array "remain UNKNOWN / NOT ESTABLISHED at the pinned source in this chapter"; "The relative positions of the other steps at the pinned production commit are not established here" | C-009's own reads indeed do not show them, but the frozen C-003, C-004 and C-005 manuals record both at the same pin. Revision 1 cites them (the batch answer and its body, T-16; no row, no log line and the rejected count, T-14 and section 6.4; the audit counts, T-17; the order, C-004 section 6.1 and C-005 sections 3 and 5) and keeps C-009's own evidence bounded: CC-02 stays PARTIAL on C-009's reads. | C-003 section 7; C-004 sections 5, 6.1, 6.4, 12 and T-14 to T-17; C-005 sections 3, 5 and 13.1 |
| R-8 (material) | Section 5.1: "Rejection status is stored using REJECTED_<check>; the test text asserts REJECTED_EXPIRED" | The source composes the status: line 1201 stores `REJECTED_{rej.check}`, and this reject's check is "EXPIRED" (919, 1221), so REJECTED_EXPIRED is FACT (PINNED SOURCE), as C-007 and C-008 record; added the stage position, the generic reject handling, the re-queue line and the retest docstring. | reads 011 to 015; C-007 S-05, D-06; C-008 section 4.1, ST-06, RQ-04, D-04; B-008 section 3 |
| R-9 | Section 5.2: the conditional, rate-limited warning | The send's exception caught and ignored (1230-1231); the time recorded before the send (INTERPRETATION: a failed send also starts the window); the notifier among main's arguments. | read 013; C-008 W-02 |
| R-10 (material) | Section 6: only the test values 60, 30 and 600, and "The field's presence does not establish the effective value" | Correct for the deployed value, but the pin establishes more: the repository configuration's 600 (FACT (CONFIG)), the receiver's use of it, main's wiring to the processor, the constructor default 60, the text drift, the configuration guide's 600 and A-009's historical note. The deployed value stays NOT ESTABLISHED (W-7); nothing is inferred from a test value. | reads 001 to 006, 010, 015, 019 to 022, 027; C-008 CF-01; A-009 section 6; C-002 section 13.5 |
| R-11 | Section 7: the six cases without names or the age of R-2 | Test names; R-2's signal aged 90 s; R-4's import of `_PipelineReject`, unused in the lines read; the set-up lines of the greps. | reads 019 to 022, 030 to 035 |
| R-12 | Section 8: CC-04 and CC-07 unlabelled; CC-06 and CC-09 absent | CC-04 and CC-07 labelled INTERPRETATION, as in the comparison file; CC-06 (chapter 02 line 378) and CC-09 (26 lines) accounted for; C-008's CH-10 (line 952, "60s-age", PARTIAL) cited, since C-009's grep terms did not match it; CC-02 with the frozen order. | reads 028, 029; C-008 cross-check CH-10 |
| R-13 (material) | Section 9: the class defined "according to the approved comparison"; the reader "reportedly selects" expires_at; "BK-23: test text uses received_at for the expires_at calculation, while several other test bodies calculate age from triggered_at"; "BK-17: test/source text contains 60- versus 600-second thresholds" | The class and the SELECT were printed (reads 025 and 018): FACT (PINNED SOURCE). OQ-C2 bounded by S1-1's counts and C-007 section 8, still OPEN. BK-23 records the source: "expires_at is stored from received_at while both age checks measure from triggered_at (C-007_manual.md section 9.2, K-04)"; revision 1 states it as recorded, re-verified at the pin, with the test text as an addition. BK-17 as recorded (C-006 BM-08). BK-18 added. OQ-C1 and OQ-C2 carried as BK-44 and BK-45 (as C-008's OQ-01 became BK-43); new BK-46 and BK-47. | reads 018, 025, 030, 032 to 034; V2_DESIGN_LOG section 7 at b720bd7; S1-1 (Q11) |
| R-14 | Sections 10 to 14: the diagram without the no-row outcome or the frozen positions; no mode note; no operational record; a short ledger; a bare list of cross-references | The diagram with the no-row outcome and the positions the frozen manuals record; the mode used only in the warning's text; the observability of each outcome, the cleanup's EXPIRED as a different mechanism and the known limits; the ledger with C-, D-, H-02, I- and U-06 rows; the reconciliation with the frozen manuals (nine rows, all consistent; no erratum). | reads 008, 012, 013; C-004 T-14, T-17; C-007 S-19, RT-01; the frozen manuals of section 14 |

Kept as revision 0 stated them: the scope and its exclusions; the strict greater-than; "Do not infer un-read surrounding control flow"; the tests as text, never run, with pass or fail unknown; the 18-Sep chapters as documents about another tree; CC-01 and CC-03 CONSISTENT, CC-02 PARTIAL, CC-05 and CC-08 out of scope; paper/live parity NOT ESTABLISHED; the deployed threshold NOT ESTABLISHED; no new KNOWN DEFECT; OQ-C1 and OQ-C2 open.

## 3. Read log and searches

The 35 logged reads (the log's time stamps; "match": the tree entry and the working file equal the pinned blob; "hash-match": the chapter copy's SHA-256 and newline count equal the record of C-008 section 14.7, the column then giving the SHA-256's first 12 characters). Every source read ran after the gate's self-test with the protected-path check at zero; the outputs are saved verbatim in the two companion results files.

| Read | Time (IST) | Step | Operation | File | Pinned blob | Result | Printed |
|---|---|---|---|---|---|---|---|
| 001 | 12:57:47 | S1-2 | range `124:128` | config/system_config.yaml | bf742ae5c81c | match | 5 |
| 002 | 12:57:47 | S1-2 | grep `(?i)expir` | config/system_config.yaml | bf742ae5c81c | match | 6 |
| 003 | 12:57:47 | S1-2 | range `192:200` | core/config_loader.py | 8635bca77531 | match | 9 |
| 004 | 12:57:48 | S1-2 | grep `expiry_sec\|signal_expiry_sec` | core/config_loader.py | 8635bca77531 | match | 1 |
| 005 | 12:57:48 | S1-2 | grep `expiry_sec\|signal_expiry_sec` | main.py | 445fb0d8237a | match | 2 |
| 006 | 12:57:49 | S1-2 | grep `expiry_sec\|signal_expiry_sec` | signals/webhook_receiver.py | 0f15a0ab0335 | match | 6 |
| 007 | 12:57:49 | S1-2 | grep `expires_at` | signals/webhook_receiver.py | 0f15a0ab0335 | match | 4 |
| 008 | 12:57:49 | S1-2 | range `916:926` | signals/webhook_receiver.py | 0f15a0ab0335 | match | 11 |
| 009 | 12:57:50 | S1-2 | range `949:961` | signals/webhook_receiver.py | 0f15a0ab0335 | match | 13 |
| 010 | 12:57:50 | S1-2 | grep `expiry_sec\|signal_expiry_sec` | signals/signal_processor.py | 7fab0a02a302 | match | 4 |
| 011 | 12:57:50 | S1-2 | grep `["']EXPIRED["']` | signals/signal_processor.py | 7fab0a02a302 | match | 2 |
| 012 | 12:57:51 | S1-2 | range `914:921` | signals/signal_processor.py | 7fab0a02a302 | match | 8 |
| 013 | 12:57:51 | S1-2 | range `1193:1231` | signals/signal_processor.py | 7fab0a02a302 | match | 39 |
| 014 | 12:57:51 | S1-2 | range `1515:1515` | signals/signal_processor.py | 7fab0a02a302 | match | 1 |
| 015 | 12:57:52 | S1-2 | range `2376:2384` | signals/signal_processor.py | 7fab0a02a302 | match | 9 |
| 016 | 12:57:52 | S1-2 | grep `expires_at` | core/schema.sql | b46d7489f6ec | match | 1 |
| 017 | 12:57:53 | S1-2 | grep `expiry_sec\|signal_expiry_sec` | core/schema.sql | b46d7489f6ec | match | 1 |
| 018 | 12:57:53 | S1-2 | grep `expires_at` | ops_dashboard/backend/readers/db_reader.py | f59e4f58fe51 | match | 1 |
| 019 | 12:57:53 | S1-2 | grep `expiry_sec\|signal_expiry_sec\|REJECTED_EXPIRED\|["']EXPIRED["']` | tests/unit/test_webhook_receiver.py | 7b377abb85d6 | match | 6 |
| 020 | 12:57:54 | S1-2 | grep (as 019) | tests/unit/test_signal_processor.py | 3ec0ea84392a | match | 6 |
| 021 | 12:57:54 | S1-2 | grep (as 019) | tests/unit/test_fix136_batch9.py | 20d59c50759e | match | 10 |
| 022 | 12:57:54 | S1-2 | grep (as 019) | tests/unit/test_config_loader.py | 8f245c35a047 | match | 1 |
| 023 | 12:57:55 | S1-2 | grep `(?i)expir` | config/strategies/pb01_breakout_retest.yaml | eedf5cac5ed5 | match | 1 |
| 024 | 12:57:55 | S1-2 | grep `(?i:expir)\|["']EXPIRED["']` | core/events.py | 6511bc40fe74 | match | 2 |
| 025 | 12:57:55 | S1-2 | grep `(?i:expir)\|signal_age\|max_age` | core/exceptions.py | 1eeac5842084 | match | 5 |
| 026 | 12:57:56 | S1-2 | grep `expiry_sec\|signal_expiry_sec\|expires_at\|signal_age\|max_age` | docs/04_db_schema_reference.md | 0218e152b77e | match | 1 |
| 027 | 12:57:56 | S1-2 | grep (as 026) | docs/CONFIG_GUIDE.md | 1e4f7ee17440 | match | 2 |
| 028 | 13:49:42 | S2-19 | grep `(?i:expir)\|signal_age\|max_age` | 02_REPOSITORY_AND_FILE_MAP.md | 8b5aec74544c | hash-match | 36 |
| 029 | 13:49:42 | S2-20 | grep (as 028) | 03_STARTUP_RUNTIME_AND_SCHEDULER.md | f7d38f2d0f7e | hash-match | 4 |
| 030 | 13:49:42 | P-2 | range `940:950` | tests/unit/test_webhook_receiver.py | 7b377abb85d6 | match | 11 |
| 031 | 13:49:43 | P-2 | range `572:582` | tests/unit/test_signal_processor.py | 3ec0ea84392a | match | 11 |
| 032 | 13:49:43 | P-2 | range `120:126` | tests/unit/test_fix136_batch9.py | 20d59c50759e | match | 7 |
| 033 | 13:49:43 | P-2 | range `128:136` | tests/unit/test_fix136_batch9.py | 20d59c50759e | match | 9 |
| 034 | 13:49:44 | P-2 | range `138:145` | tests/unit/test_fix136_batch9.py | 20d59c50759e | match | 8 |
| 035 | 13:49:44 | P-2 | range `154:158` | tests/unit/test_fix136_batch9.py | 20d59c50759e | match | 5 |

Searches, counts only (no line printed): S1-1, Q01 to Q34 (the eight terms K1 to K8 over the four scopes s_all, s_rt, s_tests and s_cfg, with two positive controls; C-009_CLAUDE_STAGE1_S1-1_COUNT_ONLY_RESULTS_24-OCT-2026.txt); S1-2, Q35 to Q39 (the two chapters; C-009_CLAUDE_STAGE1_S1-2_READ_RESULTS_24-OCT-2026.txt section 6). A structure-only scan, P1-01 to P1-04, printed line numbers of the test functions only (C-009_CLAUDE_STAGE1_P1_RESULTS_AND_S2-19_STOP_24-OCT-2026.txt). The lock log: E-1 and E-5 (S1-2), E-1b and E-5b (P-1 window), E-1c and E-5c (reads 028 to 035).

## 4. Evidence register (claim index of the frozen manual)

| Manual section | Claim | Label | Evidence |
|---|---|---|---|
| 3, 6 | `signal_queue.expiry_sec: 600` and its comment | FACT (CONFIG) | `config/system_config.yaml:124-128` (read 001) |
| 3, 6 | `SignalQueueConfig` forbids extra keys; `expiry_sec: int` without a default; the comment "(default 60)" | FACT (PINNED SOURCE); FACT (TEXT DRIFT) | `core/config_loader.py:192-197` (read 003) |
| 3, 4.1, 6 | the receiver's threshold is the queue setting, passed to the per-symbol call | FACT (PINNED SOURCE) | `signals/webhook_receiver.py:679, 721, 900` (read 006); C-004 T-14 |
| 4.1 | `now_ist()`; a naive `triggered_at` given IST; the age; the strict comparison; the EXPIRED return | FACT (PINNED SOURCE) | `signals/webhook_receiver.py:916-926` (read 008) |
| 4.1 | a future `triggered_at` passes | INTERPRETATION | `signals/webhook_receiver.py:924-925`; `signals/signal_processor.py:916-917`; C-004 T-14 |
| 4.2 | `expires_at = received_at + expiry_sec`, as ISO text, named in the row | FACT (PINNED SOURCE) | `signals/webhook_receiver.py:960-961, 969, 976` (reads 007, 009) |
| 4.2 | the schema comment and the schema document | FACT (PINNED SOURCE); FACT (DOCUMENT TEXT) | `core/schema.sql:49` (read 016); `docs/04_db_schema_reference.md:97` (read 026) |
| 4.2, 4.3 | an EXPIRED symbol leaves no row and no log line; the batch answer and the audit counts | frozen manuals | C-004 section 6.4, T-14, T-16, T-17; C-005 section 5; C-007 PM-01 |
| 4.3 | the expiry check before the fingerprint and the row preparation | FACT (PINNED SOURCE) | `signals/webhook_receiver.py:925, 952-954, 957-961` (reads 008, 009) |
| 4.3 | the other intake steps' positions; the replay limits; the EOD route without expiry | frozen manuals | C-003 sections 6.5, 7 and 18 (L-01); C-004 sections 5 and 6.1; C-005 sections 3, 5 and 13.1 |
| 5.1 | IST zone given; the age; the strict comparison; `_PipelineReject("EXPIRED", reason)` | FACT (PINNED SOURCE) | `signals/signal_processor.py:914-921` (read 012) |
| 5.1 | the stage position ST-06 | frozen manuals | C-008 section 4.1; B-008 section 3 |
| 5.1 | the stored status REJECTED_EXPIRED with the reason | FACT (PINNED SOURCE) | `signals/signal_processor.py:919, 1201, 1221` (reads 011, 013); C-008 section 4.1 |
| 5.1, 12 | the generic reject handling (log, count, evidence capture, release, statistics) | FACT (PINNED SOURCE) | `signals/signal_processor.py:1193-1220` (read 013); C-008 section 4.1 |
| 5.1 | a re-queued item carries the original `triggered_at` | FACT (PINNED SOURCE); C-008 RQ-04 INTERPRETATION | `signals/signal_processor.py:1515` (read 014) |
| 5.1 | the retest resume's docstring | FACT (PINNED SOURCE) | `signals/signal_processor.py:2376-2384` (read 015); C-008 section 9.2, D-04 |
| 5.2 | the warning's conditions, the 60.0 s monotonic window, the text, the ignored exception | FACT (PINNED SOURCE) | `signals/signal_processor.py:1221-1231` (read 013) |
| 5.2 | a failed send starts the window; the limit per object | INTERPRETATION | `signals/signal_processor.py:1224-1228` (read 013) |
| 5.2 | main passes a notifier | frozen manual | C-008 section 2.1, W-02 |
| 6 | main passes the setting to the processor, with its comment | FACT (PINNED SOURCE) | `main.py:3613-3614` (read 005); C-008 CF-01 |
| 6 | the constructor default 60, kept as the attribute | FACT (PINNED SOURCE) | `signals/signal_processor.py:175, 222` (read 010) |
| 6 | the processor tests' helper default 60 | FACT (TEST TEXT) | `tests/unit/test_signal_processor.py:372, 421` (read 020); P1-02 |
| 6 | 60 in a configuration comment and the retest docstring | FACT (TEXT DRIFT) | `config/system_config.yaml:567` (read 002); `signals/signal_processor.py:2378` (read 015) |
| 6 | the configuration guide's 600 | FACT (DOCUMENT TEXT) | `docs/CONFIG_GUIDE.md:363` (read 027) |
| 6 | the usage tracker's account | HISTORICAL | A-009 section 6 |
| 6 | the comment's Chartink claim not established | frozen manual | C-002 section 13.5 |
| 6, 7 | the tests' expiry inputs | FACT (TEST TEXT) | `tests/unit/test_webhook_receiver.py:72, 942, 1578, 1636, 1683` (reads 019, 030); `tests/unit/test_signal_processor.py:578` (read 031); `tests/unit/test_fix136_batch9.py:123, 135, 144, 157` (reads 032 to 035); `tests/unit/test_config_loader.py:75` (read 022) |
| 7 | R-1 | FACT (TEST TEXT) | `tests/unit/test_webhook_receiver.py:940-950` (read 030) |
| 7 | R-2 | FACT (TEST TEXT) | `tests/unit/test_signal_processor.py:572-582` (read 031) |
| 7 | R-3 to R-6 | FACT (TEST TEXT) | `tests/unit/test_fix136_batch9.py:120-126, 128-136, 138-145, 154-158` (reads 032 to 035) |
| 7 | the data row naming REJECTED_EXPIRED | FACT (TEST TEXT) | `tests/unit/test_fix136_batch9.py:176` (read 021) |
| 8 | CC-01 to CC-09 | FACT (DOCUMENT TEXT); INTERPRETATION for CC-04 and CC-07 | reads 028, 029; the comparison companion, section 5 |
| 8 | CH-10 | frozen manual | C-008 cross-check, CH-10 |
| 8, 9 | the PB01 strategy file's comment | FACT (CONFIG) | `config/strategies/pb01_breakout_retest.yaml:20` (read 023) |
| 9 | `SignalExpiredError` and its docstring | FACT (PINNED SOURCE) | `core/exceptions.py:360, 366, 368` (read 025) |
| 9 | the dashboard reader's SELECT | FACT (PINNED SOURCE) | `ops_dashboard/backend/readers/db_reader.py:826` (read 018) |
| 9 | `expires_at` in 8 runtime Python files (18 lines) | count only | S1-1, Q11 (C-009_CLAUDE_STAGE1_S1-1_COUNT_ONLY_RESULTS_24-OCT-2026.txt, summary and section 5) |
| 9 | the token-file field; the dashboard's classification by status | frozen manuals | B-004 section 4; C-007 section 8 |
| 9 | the test text's two bases (BK-23) | FACT (TEST TEXT) | `tests/unit/test_fix136_batch9.py:125, 131-134, 140-143` (reads 032 to 034); `tests/unit/test_webhook_receiver.py:943` (read 030) |
| 9 | the "signal_age 10" weight | FACT (DOCUMENT TEXT) | `docs/CONFIG_GUIDE.md:326` (read 027) |
| 11 | the mode used only in the warning's text | FACT (PINNED SOURCE), bounded | `signals/signal_processor.py:1228` (read 013); `signals/webhook_receiver.py:916-926` (read 008); `signals/signal_processor.py:914-921` (read 012) |
| 12 | the cleanup's EXPIRED | frozen manual | C-007 S-19, RT-01 |
| 14 | each frozen statement | as cited there | A-003 section 7; B-008 section 3; B-009 section 5; C-002 sections 13.5 and 18; C-004 T-14; C-005 section 5; C-006 sections 2.1 and 3; C-007 section 2.1, K-04; C-008 CF-01, ST-06, RQ-04, D-04 |

## 5. Unknowns, open questions and historical distinctions

| ID | Point | What bounds it |
|---|---|---|
| U-01 | The deployed signal expiry threshold | Both legacy VMs were deleted with their configuration, databases and logs; the repository value is 600 (W-7). |
| U-02, OQ-C1 (BK-44) | Whether `SignalExpiredError` is raised anywhere at the pin | The class is defined; neither check uses it in the lines read; no raise-site search was authorised. |
| U-03, OQ-C2 (BK-45) | Whether any reader uses `signals.expires_at` to decide expiry | One SELECT names it; 8 runtime files name it (S1-1), five of them not inspected; the dashboard classifies expiry by status (C-007 section 8). |
| U-04 | Pass or fail of the six tests | Never run, by design. |
| U-05 | Paper/live parity of signal expiry | The lines read have no mode branch; R-6's body does not exercise its docstring's claim. |
| U-06 | How often either check fired; whether any warning was delivered | No host database, log or alert record survives. |

Historical distinctions kept apart in the manual:
- The 18-Sep chapters describe the testing worktree e7bf477; the pin is 970aabf. Chapter statements are FACT (DOCUMENT TEXT) about that tree, and their line numbers are not comparable (C-008 section 14.7).
- C-009's own reads (FACT (PINNED SOURCE)) against the frozen manuals' findings at the same pin, which are cited and not re-verified unless the manual says so.
- Source comments that tell history: FIX-022 (the receiver's naive-time handling), the usage tracker's 2026-04-28 account (A-009 section 6) and the configuration comment's claim about Chartink (C-002 section 13.5): what the comments say, not what happened.
- Paper mode is legacy context (V2 is live-only, DR-001); the batch-9 docstring's parity claim is not exercised.
- The full text of chapter 03 that the chat app loaded (deviation D-2) was not used: only read 029's four lines are.

## 6. Checklists

### 6.1 The reconciliation's acceptance checklist (its section 7)

| Item | Result |
|---|---|
| Receiver-side EXPIRED and processor-side REJECTED_EXPIRED documented separately | Met: sections 4 and 5, the diagram (section 10). |
| The threshold comparison shown as `age_sec > expiry_sec` / `_signal_expiry_sec` only where the source supports it | Met: sections 4.1 and 5.1 (reads 008, 012). |
| `expires_at` documented separately from the age-based rejection | Met: section 4.2 and BK-23. |
| The warning described only to the extent established | Met: section 5.2; delivery NOT ESTABLISHED. |
| Tests described as test text; none claimed to have run or passed | Met: section 7, U-04. |
| The deployed threshold, the raisers of `SignalExpiredError` and the readers of `expires_at` remain unknown | Met: U-01, OQ-C1, OQ-C2. |
| The 18-Sep comparison labelled as a different worktree | Met: sections 2 and 8. |
| The signal-age score, PB01 expiry, broker-token expiry and EOD cleanup excluded or cross-referenced as out of scope | Met: sections 1, 8 and 12; BK-46. |
| No V2 decision, fix, operational action or credential value | Met: the manual's closing line; the scans of section 7. |
| Any discrepancy reported with the exact filename and section | Met: section 2 (R-2, R-7, R-8, R-10 and R-13 against C009_Legacy_Manual_Draft_RESHARE_24-Oct-2026.md sections 2, 4.3, 5.1, 6 and 9). |

### 6.2 The finalisation prompt's review checklist

| Item | Result |
|---|---|
| 1. Every manual claim supported by the supplied results | Met: section 4; the claims the results do not hold are cited to frozen manuals, never to judgment. |
| 2. Receiver and processor checks kept distinct | Met: sections 4 and 5. |
| 3. `triggered_at` age distinguished from `expires_at = received_at + expiry_sec` | Met: sections 4.2 and 9 (BK-23). |
| 4. The 60-second warning limit conditional on a notifier | Met: section 5.2. |
| 5. Test excerpts labelled TEST TEXT, never runtime evidence | Met: sections 6 and 7. |
| 6. BK-17 and BK-23 kept without upgrading them to a proven runtime defect | Met: section 9; K-01 none. |
| 7. Signal-age score, PB01 expiry, token expiry and C-007 EOD cleanup out of scope | Met: sections 1, 8, 12. |
| 8. The evidence ledger and the end-to-end diagram retained | Met: sections 13 and 10. |
| 9. Pinned line references labelled as the evidence package's bounded references | Met: sections 2 and 3. |
| 10. No unsupported caller, ordering, storage consumer, HTTP envelope, recovery path or paper/live claim | Met: the caller (main) is read 005; the ordering and the HTTP envelope are cited to C-003, C-004 and C-005; the one storage consumer named is the SELECT of read 018, with its use OPEN; the recovery paths are cited to C-007 and C-008; paper/live stays NOT ESTABLISHED. |

### 6.3 The prompt's deliverables

| Deliverable | File |
|---|---|
| A. The final C-009_SIGNAL_EXPIRY_LEGACY_MANUAL.md | docs/agenda/c-009_signal-expiry/C-009_manual.md: the repository names a final topic manual <ID>_manual.md (W-3) |
| B. C-009_FINAL_REVIEW_AND_EVIDENCE_REGISTER.md | this file |
| C. C-009_FINAL_PACKAGE_24-OCT-2026.zip | the complete package: the final manual, this register, the supplied results file with its companion, the inputs and the governance updates (C-009_review_notes.md section 8), with the push commands and the manifest at its root |
| D. C-009_FINAL_PACKAGE_MANIFEST.txt | at the zip's root: every file's exact name and SHA-256 |

## 7. Checks of the frozen manual and of this register

- Citations: every backticked `path:lines` citation in C-009_manual.md and in this file was printed by a logged read at its pinned blob (cite_check_c009.py; counts in C-009_review_notes.md section 2).
- Every backticked repository path is in the pinned listing; every 40- and 64-character hexadecimal ID is a pinned blob, the pin, V2 main or the SHA-256 of a recorded input or tool.
- ASCII only; no CR; no session link, absolute path or credential pattern; the value-blind identifier scan finds only long hexadecimal tokens.
- Tables well formed (check_md.py).
