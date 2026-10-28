# D-004 - MARKET-DATA REQUIREMENTS (LEGACY MANUAL) - DRAFT / NOT FROZEN

Status: DRAFT / NOT FROZEN. Draft 1, built 18:41 IST on 27-Oct-2026 for ChatGPT's review under D004_CLAUDE_PROMPT_PREPARE_CANDIDATE_MANUAL_NO_REPO_WRITES_27-OCT-2026.txt and D004_CHATGPT_RECONCILIATION_STAGE1_AND_NEXT_STEP_27-OCT-2026.txt (both received from Rama at 18:02 IST); uncommitted and not stored. Factual legacy documentation only: it records no V2 decision, uses no register number (DR-070 stays unused) and makes no claim about deployed or runtime behaviour.

Topic: D-004 Market-data requirements (master agenda, section D, `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:295`). Legacy system status: DEAD / REFERENCE ONLY.

Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa), read on 27-Oct-2026 in D-004 Stage 1 through the gated masking filter: 21 count-only search runs and 170 logged reads of 40 files at their pinned blobs (D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt and its three companions). V2 records are cited at V2 main c33466b94700eeccace555a145b88a3e49bf3196. No legacy file was read, searched or counted for this draft.

Purpose: a factual description of the legacy system's market-data contracts - where each kind of market data came from and how it was fetched, transformed, guarded, output, stored and handed to its consumers - as far as the Stage 1 evidence shows, with every gap left open. It is not a V2 design and prescribes nothing for V2.

Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name from the legacy source or the host records is reproduced, and no value the masking filter masked is reproduced.

Masking-input audit (process provenance): on every Stage 1 read (170), the masking filter loaded two legacy host records, HR-1 (a system map) and HR-2 (a deployment record), value-blind as its host-token source; HR-3 (a testing-snapshot manifest) is absent and was not sought (D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt, section 7).

## 1. Purpose, scope and boundaries

### 1.1 What this chapter documents

- the broker REST quote (section 4);
- the live tick feed (section 5) and the live minute candles built from it, with their persistence (section 6);
- historical candles fetched through the shared OHLC closure (section 7);
- the post-close 1-minute candle fetch and the candles table (section 8);
- instrument identity and reference data (section 9) and index data (section 10);
- derived daily statistics (section 11);
- time, session and calendar (section 12) and pacing, limits, caches and retries (section 13);
- other persisted market data (section 14) and the paper and live distinctions (section 15);
- interpretations, gaps, observations and open questions (section 16) and the two proposed bookmarks (section 17).

Each contract is traced as SOURCE -> FETCH -> TRANSFORM / NORMALISE -> VALIDATION / GUARD -> OUTPUT -> STORAGE / HANDOFF -> CONSUMER; section 3 gives the overview. A link the evidence does not show is written NOT ESTABLISHED.

### 1.2 Scope basis (HISTORICAL / RECORD-DERIVED)

- D-003's boundary row hands D-004 the "broader market-data contracts and requirements" (`docs/agenda/d-003_secondary-screening/D-003_manual.md:48`).
- ChatGPT accepted the Stage 0 inventory with corrections and authorised bounded Stage 1 research (D004_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_27-OCT-2026.txt). Rama's gate answers (D004_STAGE1_GATE_ANSWERS_RAMA_27-OCT-2026.txt) set: gate answer G-01, the consumer scope - dependency mapping only (call sites, fields consumed, transformations, guards, outputs, storage and handoff), with no runtime reachability inferred and no other topic redone; G-02, header-only reads of five named test fixtures; G-03, none of the 13 named records; G-04, tests in the count-only searches with positive controls, and narrow reads of listed tests only; G-05, the read limits, with no silent widening.
- ChatGPT accepted the Stage 1 evidence with documentation corrections and allowed candidate drafting for review only (D004_CHATGPT_RECONCILIATION_STAGE1_AND_NEXT_STEP_27-OCT-2026.txt).

### 1.3 Boundaries (Stage 1 boundary matrix; recorded owners kept; nothing closed, reassigned or reinterpreted)

| Topic | What stays with it (as recorded) | What D-004 records at its edge | Basis |
|---|---|---|---|
| D-001 Pre-filter (frozen, DR-067) | the processor's checks before screen() | the processor receives the instrument cache (MC-41) and a quote function for momentum LTP (MC-08) | `docs/agenda/d-003_secondary-screening/D-003_manual.md:46`; `main.py:3586-3592, 3613-3616` |
| D-002 Hard rejection gates (frozen, DR-068) | gates, windows, the F&O ban list, the index-list note | session times (MC-48); the fno_ban table (MC-58); the quote's circuit fields (MC-05) | `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:230-244` |
| D-003 Secondary screening (frozen, DR-069) | the screener's observed inputs | the quote contract (MC-01 to MC-05); a daily_symbol_stats reader named in a docstring (MC-54; proposed bookmark, section 17) | `docs/agenda/d-003_secondary-screening/D-003_manual.md:48, 262-280, 325` |
| D-005 Candle analysis | candle analysis | candle shapes and their two time conventions (MC-21 to MC-23, MC-31, MC-33, MC-38) | `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:296` |
| D-007 Timeframe analysis | timeframes in analysis | the timeframes fetched (day, 60minute, 30minute, minute) and the watchlist's 5-minute entry bars, which a configuration comment says are built by resample (MC-29, MC-33, MC-35) | `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:298` |
| D-008 Trend / market context | regime logic | the index data and the regime's data path, disabled in the repository configuration (MC-28, MC-29, MC-45) | `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:299` |
| D-009 to D-012 S&R | levels and zones | the S&R fetch contract (MC-30 to MC-32) | `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:300-303`; `docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md:872` |
| D-014, D-015 (BK-46) | retest logic | the retest monitor's 1-minute fetch (MC-32) | `docs/governance/V2_DESIGN_LOG.md:597` |
| D-016 or F-010 (BK-56) | entry gate and throttle | the entry gate's quote function (MC-08) | `docs/governance/V2_DESIGN_LOG.md:607` |
| C-004 Validation (U-03, BK-67) | symbol acceptance and lookups | the instrument cache load and identity (MC-40 to MC-44); the key case still NOT ESTABLISHED (MC-10) | `docs/agenda/c-004_validation/C-004_manual.md:67-73, 222, 232, 427` |
| C-011 EOD daily signal routing (C-011 G-06, BK-52) | the EOD capture and its fetch window | the watchlist lookback values in configuration (MC-29); BK-52 stays OPEN and unassigned | `docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md:449`; `docs/governance/V2_DESIGN_LOG.md:603` |
| B-004, B-006 | client builds, feed start, the candle-store timer | cited, not re-described (MC-12, MC-19, MC-23) | `docs/agenda/b-004_broker-connection/B-004_manual.md:34-35`; `docs/agenda/b-006_worker-initialization/B-006_manual.md:40` |
| A-006 (Q-30) | pacing and the broker's limits | the buckets and backoff in configuration (MC-50, MC-51) | `docs/agenda/a-006_broker-model/A-006_manual.md:56-60`; `docs/governance/V2_DESIGN_LOG.md:200` |
| Later exits; F-004 sizing | exit logic; sizing (BK-22, BK-31) | quote and tick-size reads by the exit guards (MC-08; `orders/sl_breach_monitor.py:75-79`) | `docs/governance/V2_DESIGN_LOG.md:573, 582` |

### 1.4 Not covered (cross-references only; Stage 0 boundary matrix, D-004_SOURCE_INVENTORY_AND_RESEARCH_PLAN_27-OCT-2026.txt section 7)

- The processor's pre-screen checks (D-001), the gates and windows (D-002) and the screener's steps, score and tier (D-003).
- The scanner payload's fields and intake (C-001 to C-003), symbol acceptance and U-03 (C-004) and the EOD route (C-011).
- Authentication and access tokens (A-010, B-002).
- The analytic use of candles, patterns, timeframes, market context and S&R (D-005 to D-012), and entry, ranking and the final verdict (D-013 to D-018): their data inputs only are recorded here.

## 2. Source baseline, method and limitations

### 2.1 Baseline (FACT (METADATA))

- Legacy: HEAD and refs/heads/main 970aabf3c78d777d9d34549e021f7c8e756c7bdb, no other ref; tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa; the saved listing has 1,438 rows, SHA-256 1F195D3D516AB4F194BCB7AF105FB193A606EE241E2E1437102EF495D8E647A5. All three were rechecked at 17:20:13 IST before any checkout or read, and again by the final check (C1).
- V2: main c33466b94700eeccace555a145b88a3e49bf3196, at which D-003 is frozen as DR-069; DR-070 is the next register number and is not used here.

### 2.2 Method (Stage 1; read log, sections 2 to 5)

- Gate tools: cread_d004.py, ccount_d004.py and clines_d004.py, rebuilt from `docs/governance/V2_SESSION_HANDOVER_C-009_24-OCT-2026.md` section 8 with the D-004 limits (gate answers G-01 to G-05) built in; helper scripts from `docs/governance/V2_SESSION_HANDOVER_SECTION_C_21-OCT-2026.md` section 8. gate_test_d004.py passed at 17:20 IST (before the checkout), at 17:21:35 IST (after it) and in the final check (C5); it refuses the 102 never-open paths by name, the two July records, docs/v2_design_spec.md (a shared blob) and the 13 named records.
- Checkout: one anchored no-cone sparse pattern per path for 817 paths (the count scopes s_rt, s_cfg and s_tests, the two schema files, the 114-path read list, HR-1 and HR-2). 813 distinct blobs were expected and 814 were present: the extra one is the root .gitattributes, fetched by git's attribute lookup, not checked out and not read. Every working file equals its pinned blob; never-open blobs present: 0 throughout.
- Counts: S01 to S13 as authorised - S10 and S11 on the two schema files only, the others on 808 files (s_rt 281, s_cfg 41, s_tests 486) - plus seven strict narrowings of them (S02a, S03a, S03b, S04a, S06a, S06c, S08a) and one disclosed deviation (S06b, section 2.4). Count output holds per-file counts and line numbers only, never text.
- Reads: 170 logged reads of 40 files, 3,485 lines printed by the filter. 12 rest on basis K - a range a frozen manual cites (the fixed ranges of main.py and the adapter) or, for the five fixture headers, gate answer G-02 - and 158 on the count search that located them.

### 2.3 Read ranges and the disclosed exceptions

Every read has one of five bases (read log, sections 4 and 5):

| Kind | Ranges | Basis |
|---|---|---|
| K - fixed ranges | main.py 495-506, 2927-2938, 3285, 3586-3592 and 3730; the adapter 208-222 and 1730-1798; line 1 of the five fixtures (12 reads) | gate answers G-05 and G-02 |
| A - exact additions to the fixed lists | 15 ranges, each logged with the count search that located it: main.py 390-407 (S13), 423-436 (S02, S04), 543-549 (S01), 1970-1976 (S03a), 2389-2395 (S01, S12), 2438-2446 (S04, S12), 2965-2996 (S04, S12, S01), 3030-3036 (S01), 3130-3136 (S01), 3463-3563 (S12, S02), 3613-3619 (S01), 3666-3745 (S02, S12), 3837-3861 (S01, S03b) and 3918-3950 (S12, S04); the adapter 1799-1830 (S01) | gate answer G-05: for main.py, an exact additional range where the count-only pass shows it necessary for authorised wiring; for the adapter, the smallest method ranges located by S01 to S04 |
| C - count-located ranges | every other path: each count hit plus or minus 3 lines, clusters joined across gaps of up to 10 lines (S06b's extra term excluded) | gate answer G-05: known ranges first, then minimal ranges located by the authorised count-only pass |
| D - schema table definitions | four core/schema.sql reads that begin at a CREATE TABLE hit (S10) and run through the table's definition (991-1004, 1123-1148, 1463-1487 and 1593-1609), and the core/analytics_schema.sql read 1-50, which spans the file header and the candles definition (24-45) (read log rows 106 to 110) | for core/schema.sql, the accepted Stage 0 plan's "D01 (table definitions only)"; core/analytics_schema.sql is an R2 path, and no plan line names its definitions |
| X - disclosed extensions read without a hit | the four log entries marked READ WITHOUT A HIT (table below) | disclosed in the read log, section 4; gate answer G-05 asks for no silent widening |

The 82-line total. The four X entries hold eight ranges and 82 lines, the lines read without a count hit of their own:

| Path | Ranges read without a hit | Lines | Of them outside every hit +/- 3 cluster | Reason (read log, section 4) |
|---|---|---|---|---|
| data/live_feed.py | 305-328 | 24 | 24 | completes the queue-full branch begun at 302-304 |
| core/analytics_schema.sql | 1-23, 46-50 | 28 | 22 (1-20, 46-47) | the file header (database split) before the candles definition and the next table comment after it |
| core/schema.sql | 1001-1004, 1143-1148, 1472-1487, 1607-1609 | 29 | 16 (1143-1145, 1472-1484) | leading comment lines of the next section, read with the table definitions above them |
| orders/smart_tgt_manager.py | 788 | 1 | 1 | one blank line before the count-located cluster; found by the final check |
| Total | 8 ranges in 4 entries | 82 | 63 | |

The other 19 of the 82 lines (core/analytics_schema.sql 21-23 and 48-50; core/schema.sql 1001-1004, 1146-1148, 1485-1487 and 1607-1609) have no hit of their own but lie inside the plus-or-minus-3 cluster of a neighbouring hit.

Lines read that lie neither in a kind-K fixed range nor in any hit +/- 3 cluster (computed in this step from the Stage 1 count line numbers and the read log; no file was read):

| Group | Lines | Where | Recorded in Stage 1 as |
|---|---|---|---|
| X, outside clusters | 63 | as in the table above | READ WITHOUT A HIT (disclosed) |
| D, definition lines beyond the clusters | 49 | core/schema.sql 1127-1142 (16), 1467-1471 (5) and 1597-1606 (10); core/analytics_schema.sql 28-45 (18) | S10 table definitions, not as extensions |
| A, inside one addition | 19 | main.py 3695-3713, inside the addition 3666-3745, which takes the hits 3669 to 3742 as one span; the nearest hits are 3691 and 3717 | "hit lines +/- 3", not as an extension |
| Total | 131 | | |

How the final check treated them (C6, D-004_STAGE1_FINAL_CHECK_27-OCT-2026.txt; the check tool check_s1_d004.py, SHA-256 8F90708967389779AACFB6EC30F224A552CC049B6E14184A3874BE7D552DF3B6 in the read log, section 7): kinds K and A are tested only against the fixed lists that contain them; kind C against the hit +/- 3 clusters, apart from the disclosed extensions of data/live_feed.py, core/analytics_schema.sql and orders/smart_tgt_manager.py; and a read of either schema file is accepted when it begins at a count hit, as is the core/analytics_schema.sql read 1-50 as a whole, without a test of where such a read ends. The final check's written description of C6 does not state this schema rule. C6's PASS (170 reads; problems 0) therefore does not show that every read lay inside a hit +/- 3 cluster: the 131 lines above did not. Claims whose cited ranges include any of them are listed in D-004_STAGE2_SELF_REVIEW_CHECKLIST_27-OCT-2026.txt.

### 2.4 Count-search limits

- Count results are bounded discovery evidence, not proof of absence: a zero, or a file missing from a count result, does not show that something is absent from the system (gap G-09).
- Positive controls (hits that frozen records establish; read log, section 3) passed for S01, S02, S03, S03a, S04, S05, S07, S08, S08a, S09, S10, S12 and S13. No control is recorded for S06, S11 or the sub-patterns S02a, S03b, S04a, S06a and S06c, nor for the deviation S06b.
- S11 searched only core/schema.sql and core/analytics_schema.sql, as authorised; it cannot establish who writes or reads any table elsewhere (gap G-05).
- S06b added the term ZoneInfo, which S06 does not contain: a disclosed deviation, rerun without it as S06c. Their per-file results differ only in lines that no read used, so no read depended on the extra term (read log, section 3).

### 2.5 The Stage 1 final check (D-004_STAGE1_FINAL_CHECK_27-OCT-2026.txt)

- RESULT: PASS (13/13), run from 17:48:21 to 17:48:40 IST. C1 baselines; C2 never-open blobs present locally 0; C3 local objects unchanged since the checkout (814 blobs, 101 trees, 1 commit); C4 817 working files equal their pinned blobs; C5 gate self-test, final run; C6 170 reads, problems 0, with the disclosed extensions excluded and the schema rule of section 2.3; C7 196 cited legacy ranges in 34 files resolve at the pin and lie inside their files; C8 every cited legacy range was read; C9 22 V2 citations lie inside their files at c33466b; C10 value-blind scan, hit classes only hashes and the D004 file-name prefix; C11 no never-open path, session link or host-record name, ASCII only; C12 25 quotations, none of fifteen words or more; C13 V2 clone unchanged at c33466b, status clean.
- C7 and C8 covered the legacy citations written in backticks in the evidence file. One citation written without backticks there, `orders/sl_breach_monitor.py:75-79` (boundary matrix), was not among the 196; it lies inside read log row 149.
- INFO, not a pass: cite_check.py. That tool reads "a-b" as its start line only, and it listed path tokens it could not resolve in the legacy tree: the V2 manual names C-004_manual.md, C-011_manual.md, D-002_manual.md and D-003_manual.md; the helper tools ccount_d004.py, clines_d004.py, cread_d004.py, gate_test_d004.py and mask_read2.py; and scripts/build_daily_symbol_stats.py, which is absent from the pinned tree (MC-53). Its line is separate from C7's 196 legacy ranges and C9's 22 V2 citations and is not one of the 13 checks.

### 2.6 Evidence labels

- FACT (SOURCE): code read at the pin.
- FACT (TEXT): a comment or docstring read at the pin; what the text says, not proof of behaviour.
- FACT (CONFIG): a repository configuration value at the pin; not the deployed value.
- FACT (COUNT): a count-only result (per-file counts and line numbers, no text).
- FACT (TEST): what a test or fixture says; not runtime proof.
- FACT (METADATA) and FACT (PATH NAME): repository metadata, and a path's presence or absence at the pin.
- HISTORICAL: a frozen V2 record, cited and not re-verified.
- INTERPRETATION: Claude's reading of cited facts, marked.
- UNKNOWN / NOT ESTABLISHED: not settled by the evidence.
- OBSERVATION: a mismatch between texts, or between text and code, noted without a finding.
- OPEN QUESTION: a question for another topic or a later review.

Source code, comments and docstrings, repository configuration, tests and fixtures, historical records and runtime behaviour are different kinds of evidence: the first four were read at the pin, historical records are cited from frozen V2 records, and runtime behaviour was not examined. A path, a call site or a configuration value present in the repository is not evidence that it ran, and repository configuration is not evidence of the deployed configuration.

### 2.7 Citation conventions

- Legacy citations are `path:line` at the pin; a citation that starts with ":" continues the path cited just before it. V2 citations are `path:line` at c33466b, written as full paths (the Stage 1 files use short names).
- MC-nn are the claim IDs of D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt. Another topic's IDs carry their topic, as in "D-003 MC-37" or "C-011 G-06".
- Gate answers G-01 to G-05 (Rama's sheet) and the gaps G-01 to G-10 (section 16.3) share a prefix; this chapter always writes "gate answer" or "gap" before them.
- "Count line" marks a line number from a count result (FACT (COUNT)), which shows where a pattern matched, not what the line does. "Read log row" refers to D-004_STAGE1_FILES_READ_AND_COUNT_LOG_27-OCT-2026.txt, section 5.
- Quotations are verbatim and under fifteen words.

### 2.8 Limitations

- No VM, broker, database, webhook, Chartink or service was contacted; no legacy code, test, script or import ran; nothing was written, committed or pushed in any repository.
- Deployed configuration, runtime state and database contents are NOT ESTABLISHED.
- Test and fixture evidence (MC-10, MC-44) shows what the tests assert, not runtime behaviour (gap G-10).
- One external look-up (a web search on the broker's historical timestamps) found only an unofficial forum post; it is not broker documentation (gap G-02).
- Negative statements cover only the lines read or the scopes searched (gap G-09).

## 3. The data contracts at a glance

| Contract | Source | Fetch | Transform / normalise | Validation / guard | Output | Storage / handoff | Consumer | Detail |
|---|---|---|---|---|---|---|---|---|
| Quote, live | broker REST quote | adapter.get_quote: one rate-limiter token of its mapped category, one quote call (MC-02) | "NSE:" keys; response key cut at ":"; bid and ask from depth level 0; ts = VM time at receipt (MC-02 to MC-05) | exceptions translated; 429 counter reset on success; a missing symbol is absent (MC-02, MC-04) | dict of Quote (MC-01, MC-04) | none in get_quote's lines read; a market_execution_context table exists, its writer NOT ESTABLISHED (MC-57) | screener, smart target, CNC GTT, reconciler, processor, entry gate (MC-08, MC-09) | section 4 |
| Quote, paper (history) | broker REST quote | the paper provider's own client (MC-07) | "NSE:" prefix; missing symbols counted (MC-07) | NOT ESTABLISHED beyond MC-07 | dict of Quote through get_quote (MC-06) | none read | as above | sections 4, 15 |
| Ticks | broker WebSocket (KiteTicker) | live_feed: MODE_LTP subscriptions in batches of 50 (MC-14) | token, LTP, depth bid and ask, the tick's exchange or tick timestamp, traded volume; each batch stamped at receipt (MC-16) | queue-full branch, a soft kill only when a kill switch is given, and none is passed (MC-17); hollow ticks dropped (MC-17); watchdog (MC-18) | tick dicts to the dispatcher (MC-19) | a bounded in-memory queue (MC-11) | candle store, shadow tracker (MC-19); comments describe the path as dormant (MC-14, MC-15) | section 5 |
| Live minute candles | the ticks | candle_store.on_tick (MC-22) | VM-clock minute windows; ts = close time; synthetic flat candles (MC-22, MC-23) | synthetic-only warning (MC-23) | CandleData (MC-21) | memory, 500 per token (MC-21); a database writer registered, which its comment says stays dark (MC-25) | smart-target trail (MC-26) | section 6 |
| Historical candles | broker historical data | the shared closure, one "historical" token per call (MC-27) | Candle.from_kite: ts = the row's date; volume default 0 (MC-31) | None, or the interval omitted, on failure; TTL cache (MC-30) | Candle lists per interval (MC-30) | memory cache (MC-30); regime state in a JSON file (MC-28) | S&R detector, zone warmer, retest monitor, regime, V3 chain, watchlist (MC-28, MC-32) | section 7 |
| Post-close 1-minute candles | broker historical data | the script's own client, 0.35 s sleeps (MC-34, MC-52) | rows with the date as text (MC-36) | failed-symbol list; INSERT OR IGNORE (MC-35, MC-36) | dated CSV; candles rows (MC-35, MC-36) | the analytics candles table (MC-36, MC-37) | the daily report, per the script's docstring (MC-34); the control tower's freshness check (MC-39); other readers NOT ESTABLISHED | section 8 |
| Instrument master | config/instruments.csv, generated on the VM by record (MC-42) | InstrumentCache.load at start (MC-40, MC-41) | the symbol stripped, not case-folded (MC-40) | the whole load fails on a bad row (MC-40) | symbol lookups (MC-40) | memory; a reload publishes InstrumentsRefreshed (MC-40) | processor, smart target, OHLC fetchers, the candle store's token map (MC-41) | section 9 |
| Index data | broker historical data | the post-close script (MC-35); the shared closure, which feeds the regime runner, and an index fetch by configured token (MC-28, MC-30) | index name to token at fetch time (MC-45) | the closure path fails safe (MC-30); the script lists failed symbols (MC-35) | rows; Candle lists | the candles table (MC-36) | regime, disabled in the repository configuration (MC-29) | section 10 |
| Daily statistics | daily candles | a cache job named in a header and absent from the tree (MC-53) | core/daily_stats.py computes four inputs (MC-53) | NOT ESTABLISHED (not recorded in MC-53) | prev_close, avg_volume_20d, atr14, rsi14 (MC-53) | daily_symbol_stats (MC-54) | the screener, per a docstring; the V3 forward shadow (MC-54 to MC-56) | section 11 |
| Time | the VM clock | now_ist() (MC-46) | fixed +05:30 (MC-46) | startup skew check; a probe without a broker timestamp (MC-46, MC-47) | IST datetimes | none | the timestamps above (MC-03, MC-16, MC-22, MC-23) | section 12 |

A cell is not a claim that a path ran. The path:line evidence for each cell is in the section named in the last column.

## 4. Broker REST quote (adapter.get_quote)

- FACT (SOURCE) - record: the Quote record holds symbol, last_price, bid, ask, volume and ts, plus optional vwap, open_price, day_high, day_low, upper_circuit and lower_circuit (MC-01; `broker/zerodha_adapter.py:208-222`; as D-003 MC-22 records).
- FACT (SOURCE) - fetch, live path: get_quote takes one rate-limiter token of its mapped category (`broker/zerodha_adapter.py:1757`), builds keys as "NSE:" plus each symbol (`:1759-1760`), makes one kite.quote call for all keys (`:1762`), translates any exception to a broker exception (`:1763-1766`) and resets the category's 429 counter on success (`:1768-1769`) (MC-02).
- FACT (SOURCE) - timestamp: every Quote gets ts = now_ist(), taken once after the response (`:1771`) - the VM clock at receipt; no exchange or trade timestamp from the response is read in get_quote (`:1771-1798`) (MC-03).
- FACT (SOURCE) - keys and absences: the response key is cut at its first ":" (`:1774`); the result holds only the symbols present in the response, and a requested symbol the broker leaves out is simply absent, with no error (`:1772-1806`) (MC-04).
- FACT (SOURCE) - defaults: bid and ask come from depth level 0, else 0.0 (`:1775-1782`); last_price defaults to 0.0 (`:1787`) and volume to 0 (`:1790`); vwap (from average_price), open, high, low and both circuit limits are None when absent or zero (`:1792-1797`); no previous-close field is taken from the response (`:1784-1798`) (MC-05). INTERPRETATION: this matches D-003 MC-37, where prev_close is always None on the processor's path.
- FACT (SOURCE) - paper path and raw quote: in paper mode get_quote delegates to an injected quote_provider and raises NotImplementedError without one (`:1742-1755`); get_quote_raw returns the raw response with depth through the same mapped category, and {} in paper mode (`:1808-1826`) (MC-06). The provider is described in section 15.
- FACT (SOURCE) - consumers: get_quote is wired into the screener (`main.py:3285`), the smart-target manager (`:2978`), the CNC GTT manager (`:3033`), the order reconciler (`:3133`), the signal processor ("momentum fresh LTP fetch", FIX-067; `:3613-3616`) and the entry gate (`:3839-3840`) (MC-08). Read uses: the structure-exit guard takes last_price when above zero, else a fallback (`orders/structure_exit_manager.py:634-645`); the smart-target restart check asks for one symbol and reads the result by the bare symbol (`orders/smart_tgt_manager.py:835-857`) (MC-08).
- FACT (COUNT): S01 matches in 20 runtime and configuration files, among them signals/signal_processor.py (count lines 190, 234, 1066 to 1085 and 2645 to 2650), screening/entry_gate.py (count lines 12, 23, 95, 110, 119 and 437), orders/cnc_gtt.py (count lines 66, 78, 238 and 240), orders/order_reconciler.py (17 count lines) and orders/order_placer.py (8 count lines). Those files are R3 or NP paths: their call sites are counted, not read (MC-09).
- FACT (TEST): a test docstring records a fixed bug - the order reconciler once asked for "NSE:"-prefixed symbols, so the adapter built "NSE:NSE:" keys and the lookup always missed (`tests/unit/test_mo1_flatten_quote_key.py:4-19`); the test encodes bare-symbol keys (`:31-37`) (MC-10). It shows what the test asserts, not runtime behaviour (gap G-10). OBSERVATION (O-04): the docstring's adapter line numbers (1567, 1581) do not match the pin (1760, 1774).
- UNKNOWN / NOT ESTABLISHED: the case of the broker's response keys (gap G-01; C-004 U-03; BK-67, `docs/governance/V2_DESIGN_LOG.md:618`); which call sites ran in operation, and the deployed configuration (gap G-03).

## 5. Live ticks (KiteTicker WebSocket, data/live_feed.py)

- FACT (TEXT): the module manages the KiteTicker connection, subscriptions and LTP tick distribution through a bounded queue and one consumer thread (`data/live_feed.py:1-8`); it imports KiteTicker (`:18`) and now_ist (`:23`) (MC-11).
- FACT (SOURCE) - construction: main.py builds it with the selected account's API-key variable, the access-token variable, a critical-failure callback, paper_mode and market_windows (`main.py:2927-2938`); reconnect attempts, reconnect delay and batch size are not passed, so the defaults 10, 5 s and 50 apply (`data/live_feed.py:45-53`) (MC-12). OBSERVATION (O-01): the repository's live_feed keys (backoff base 1 s, maximum 30 s; `config/system_config.yaml:743-746`) are not passed at that call; whether they are read elsewhere is NOT ESTABLISHED.
- FACT (SOURCE) - fetch: connect() returns at once in paper mode (`data/live_feed.py:136-140`); the ticker is created with the reconnect parameters and four callbacks (`:510-521`) (MC-13). subscribe() sets MODE_LTP in batches of 50 (`:173-197`), and every reconnect re-subscribes the tracked set (`:324-361`) (MC-14).
- FACT (TEXT) - dormancy as the comments describe it: a telemetry tripwire counts any subscription (`:59-62, 178-180`); its comments say the tick path is "dormant BY DECISION e754c7e" and name the order placer's exit retry as the only latent caller (`:60-61, 179`) (MC-14). The watchdog's comment says nothing subscribes in ordinary operation and quotes a record: "32 connects, 0 tokens subscribed, 0 ticks ever" (`:708-717`); that record is one of the 13 named records (gate answer G-03) and was not read (MC-15).
- FACT (COUNT): S03b matches in orders/order_placer.py (count lines 693, 697, 700 and 3404); main.py's matches are event-bus subscriptions (`main.py:3846-3858`), and so are the shadow tracker's (`orders/shadow_tracker.py:193-195`) (MC-14).
- FACT (SOURCE) - transform: tick normalisation (`data/live_feed.py:264-301`) stamps each batch with the receipt time now_ist() (`:270`); each tick keeps the instrument token, last_price (default 0.0), bid and ask from depth when present (MODE_FULL only, else 0.0) (`:279-292`), timestamp = the tick's exchange_timestamp or timestamp (`:298`) and volume = volume_traded (`:299`); the day OHLC in the tick is left out (`:301`) (MC-16).
- FACT (SOURCE) - guards: a full queue calls soft_kill("LIVEFEED_QUEUE_FULL") when a kill switch was given, else logs that none is configured (`:302-322`); main.py's construction passes no kill switch (`main.py:2927-2938`; observation O-06). The consumer discards a tick with no token, or with last_price None or 0.0, before dispatch (`data/live_feed.py:572-581`) (MC-17). This is source-path evidence, not proof of deployed operation.
- FACT (SOURCE) - watchdog: it runs in live mode only (`:681-696`), arms only after a first tick (`:731-739`) and, on a stale feed, raises one critical alert logged as forcing a reconnect (`:758-779`); the market-window condition and the reconnect flag are in its docstrings (`:102-115, 698-707`); its thresholds are 30 s and 10 s by default (`:49-50`). Reconnect exhaustion calls the critical-failure path (`:475-481`), and off-hours it is downgraded to a warning (FIX-189; `:431-437`) (MC-18).
- FACT (SOURCE) - output and handoff: main.py's tick dispatcher passes each tick to candle_store.on_tick (token, last_price, the tick timestamp or now_ist(), volume) and to shadow_tracker.on_tick (`main.py:2965-2971`); candle_store.start() runs before live_feed.connect() (`:3920-3947`); the reconnect chain marks the candle store and the smart-target manager (`:2991-2994`) (MC-19).
- UNKNOWN / NOT ESTABLISHED: whether the feed connected, subscribed or delivered ticks in operation. The comments above describe the path as dormant; this chapter infers no runtime feed activity either way (gate answer G-01).

## 6. Live minute candles (data/candle_store.py) and their persistence

- FACT (TEXT): the candle store builds minute OHLC candles from LTP ticks and calls itself the single source of truth for intraday candles (`data/candle_store.py:1-5`) (MC-20).
- FACT (SOURCE) - shape: a candle carries token, symbol, OHLC, volume (accumulated from ticks since FIX-135), ts (the IST close time), interval_sec and is_synthetic (`:24-36`); the default interval is 60 s (`:79-85`); up to 500 candles per token are kept in memory (`:75-77, 353-359`) (MC-21).
- FACT (SOURCE); FACT (TEXT) - windows: windows follow the VM clock, as on_tick floors now_ist() to the minute (`:188`); the tick's own ts is kept "INFORMATIONAL ONLY", and exchange OHLC is never used (`:170-186`) (MC-22).
- FACT (SOURCE) - close: a timer aligned on wall-clock boundaries (`:233-251`) closes all windows, stamping close_ts = now_ist() as each candle's ts (`:262, 325, 343`). A token with history but no tick in the window gets a synthetic flat candle at the last close, with volume 0 (`:253-287, 331-347`); repeated synthetic-only closes are logged as a warning (`:289-304`). The comment on the window off-by-one fix calls it harmless today, "no live reader" (`:272-275`) (MC-23).
- FACT (SOURCE) - output: get_candles returns the last N closed candles, or [] (`:211-217`); the token-to-symbol map comes from the instrument cache (`main.py:3946` and `data/candle_store.py:128-131`); dead tokens are swept on an instruments refresh (`:133-144, 161-168`) (MC-24).
- FACT (SOURCE); FACT (TEXT) - storage: main.py registers a writer that inserts every closed candle into the database (ts = the close time, interval, OHLCV, is_synthetic) (`main.py:3923-3944`); its comment says the writer "must stay dark even if the tick feed ever wires" (`:3924-3925`) (MC-25).
- FACT (SOURCE) - consumer: the smart-target manager reads closed candles from the store when it recomputes a trail after a reconnect (`orders/smart_tgt_manager.py:789-806`) (MC-26).
- UNKNOWN / NOT ESTABLISHED: whether candles were built, written or read in operation; the candles table's other writers and readers (gap G-05).

## 7. Historical candles through the shared OHLC closure

- FACT (SOURCE) - fetch: main.py's closure returns [] when no market-data client exists, else takes one "historical" token and calls historical_data(token, from, to, interval) (`main.py:423-436`). The client is the trading client in live mode and a read-only client built from the broker session file in paper mode, or None without a token (`main.py:393-407`) (MC-27).
- FACT (SOURCE) - construction and consumers: the closure is built only when one of six feature flags is on (`main.py:3479-3487`); the flags' definitions lie above `main.py:3463` and were not read (gap G-04). It feeds the S&R detector (`:3489-3508`), the regime runner (lookback default 400 days; state written to a JSON file; `:3510-3540`), the zone warmer (`:3542-3563`), the V3 chain (lookback default 180; `:3666-3702`) and the PB-01 watchlist (lookback default 180, cache 0; `:3704-3745`) (MC-28).
- FACT (CONFIG) - repository values at the pin, not the deployed values (gap G-03): sr_detector enabled true, timeframes day, 60minute and 30minute, lookback 180 days, cache 1,800 s, wait_for_retest_enabled false, a two-day 1-minute window ("respects the Zerodha minute cap"), intraday anchors off (`config/system_config.yaml:508-545`); regime enabled false, NIFTY 50 by a configured token, daily lookback 400 (`:547-562`); v3_chain_mode "shadow" with fetch lookback 180 (`:573-574, 613-615`); watchlist enabled true, capture lookback 60 days, 5-minute entry bars (`:619-630`) (MC-29).
- FACT (SOURCE); FACT (TEXT) - transform and guard: OhlcFetcher resolves the token through the instrument cache, makes one request per interval over [now - lookback, now], converts rows with Candle.from_kite, caches by (symbol, interval) for a TTL and returns None or omits the interval on any failure, never raising (`sr_detector/fetch.py:4-21, 53-151`); the index is fetched by an explicit configured token (`:85-101`) (MC-30).
- FACT (SOURCE) - shape: in Candle, ts is the historical row's "date", OHLC are floats and volume defaults to 0 (`sr_detector/models.py:23-43`); its docstring allows an aware or a naive IST ts (`:25`) (MC-31).
- FACT (SOURCE) - readers: the S&R detector's analysis fetch (`sr_detector/detector.py:167-170`) and its anchor fetch (`:219-224`); the zone warmer (`sr_detector/zone_warmer.py:130-134`); the retest monitor's short 1-minute fetch, which drops the tzinfo of its since time (`screening/retest_monitor.py:46-50, 215-219`) (MC-32).
- FACT (SOURCE) - derived values: true range and ATR (Wilder or SMA; at least period + 1 candles, else None) (`core/candle_math.py:54-115`); session VWAP (`:136-153`); Wilder RSI (`:250-269`); resample to minutes or "day", each bar stamped at its bucket START, anchored to the injected session open, with no bar for an empty slot and the session-tail bar flagged partial (`:311-456`) (MC-33).
- OBSERVATION (O-02): candle_math's header says "Wiring: NONE yet" (`core/candle_math.py:28-31`), while the regime engine imports its adx, atr, ema and true-range functions (`regime/engine.py:26-28`) and a configuration comment says the watchlist's 5-minute bars are built via resample (`config/system_config.yaml:628`) (MC-33).
- UNKNOWN / NOT ESTABLISHED: the six flags' definitions (gap G-04); which flags were on in operation (gap G-03); who else calls candle_math (gap G-08).

## 8. Post-close 1-minute candles and the candles table

- FACT (TEXT); FACT (SOURCE) - source and fetch: scripts/fetch_daily_candles.py says it fetches 1-minute OHLCV after the close (cron 15:40 IST, Monday to Friday) for the daily report (`scripts/fetch_daily_candles.py:1-5`); it builds its own client from the broker session file and an API-key variable (`:259-282`) and its own instrument map from kite.instruments("NSE") (`:284-286`) (MC-34).
- FACT (SOURCE) - scope: it fetches the configured indices first, 09:15 to 15:31 (`:66-109, 159`), then each symbol from _get_traded_symbols, whose message speaks of PROCESSED signals (`:161-165`), 09:00 to 15:31 at interval "minute" (`:167-208`); a missing token, an empty result or an error lists the symbol as failed; it sleeps 0.35 s between calls (`:106, 208`) and writes a dated CSV (`:210-216`) (MC-35).
- FACT (SOURCE) - storage: it inserts rows into the candles table with INSERT OR IGNORE, ts = the historical row's date as text, interval 60 and is_synthetic 0, in the analytics database, and skips the insert when the database is missing (`:294-321`); the default run is skipped at weekends and on Friday after 17:30 (`:229-235`); the trade date comes from today_ist() (`:249-255`) (MC-36).
- FACT (SOURCE); FACT (TEXT) - the table: candles holds symbol, token, ts ("ISO-8601 IST candle close"), interval_sec (default 60), OHLC, volume (default 0), is_synthetic and a generated date, with UNIQUE(token, ts, interval) (`core/analytics_schema.sql:21-45`), in a separate analytics database attached by name (`:2-17`) (MC-37).
- INTERPRETATION; UNKNOWN / NOT ESTABLISHED (MC-38): the two writers stamp ts differently - the candle-store writer uses the close time (MC-23, MC-25) and the script stores the historical row's date (MC-36) - while the schema comment says close (MC-37). Whether the broker's historical date marks a bar's start is not established from an official source: a user forum post, unofficial and not broker documentation, says start, and no broker staff reply was found. If it does, one minute could carry two ts values that the UNIQUE key would not merge. The candle-store writer is described as dormant (MC-14, MC-25). Gap G-02; proposed bookmark, section 17.
- FACT (SOURCE) - freshness: the control tower expects the candles stage by 15:40 IST and takes today's table maximum or the fetch marker file's time (`ops/control_tower/freshness.py:1-19, 39-78`) (MC-39). OBSERVATION (O-03): the freshness table's comment lists OK, LATE, MISSING and NA (`core/schema.sql:1463-1470`), while the code uses OVERDUE (`ops/control_tower/freshness.py:4, 60`) (MC-39).
- UNKNOWN / NOT ESTABLISHED: whether the script ran on any day; the table's other writers and readers (gap G-05); the job's cron-registry entry (gap G-06); the table's retention (gap G-07).

## 9. Instrument identity and reference data

- FACT (SOURCE); FACT (TEXT) - source: the instrument cache loads config/instruments.csv with the columns symbol, instrument_token, exchange, lot_size, tick_size, is_fno and sector (`core/instrument_cache.py:1-28, 54-68`) (MC-40).
- FACT (SOURCE) - validation and transform: a missing file, missing column, malformed row, empty symbol, lot below 1 or tick at or below 0 fails the whole load (`:98-186`); the symbol is stripped, not case-folded (`:143`); reload() builds a new cache and publishes InstrumentsRefreshed (`:188-215`); the load path read has no file-age or freshness check (`:98-186`) (MC-40).
- FACT (SOURCE) - handoff: main.py loads it at start (`main.py:2438-2446`) and passes it to the processor (`:3586-3592`), the smart-target manager (`:2989`), the OHLC fetchers (`:3519-3523, 3559-3562, 3678-3682, 3728-3732`) and the candle store's token map (`:3946`) (MC-41).
- HISTORICAL: config/instruments.csv is absent from the tree and was generated on the VM by scripts/refresh_instruments.py from the security master and the broker dump, matched by exact string (`docs/agenda/c-004_validation/C-004_manual.md:68-69, 222`); cache lookups are exact and case-sensitive, with defaults on a miss (`docs/agenda/c-004_validation/C-004_manual.md:232, 427`) (MC-42).
- FACT (COUNT): instrument-dump calls (S04a) are in scripts/fetch_daily_candles.py (count line 285), scripts/refresh_instruments.py (count line 306), scripts/forward_shadow_record.py (count line 113), scripts/reconstruct_excursions.py (count line 294) and scripts/sr_corp_action_spotcheck.py (count line 46), with two docstring mentions (config/index_universe.yaml, count line 6; sr_detector/fetch.py, count line 17); there are none in the adapter or main.py, within S04a's scope and with no positive control recorded for S04a (MC-43; gap G-09). INTERPRETATION: besides the cache, at least the candle script keeps its own name-to-token map (MC-34, MC-43).
- FACT (TEST) - fixture headers only (gate answer G-02): the four index-member fixtures named in gate answer G-02 have one column, symbol (read log rows 126 to 129); the security-master fixture has symbol, name_of_company, series, fno_stock, date_of_listing, paid_up_value, market_lot, isin_number and face_value, six of them with a leading space (`tests/fixtures/reference_data/security_master_small.csv:1`) (MC-44). These are test-fixture shapes only, not the production files' schema (gap G-10); config/reference_data/ stays never-open (gate answer G-02).
- UNKNOWN / NOT ESTABLISHED: the broker's response-key case (gap G-01); the refresh job's schedule (gap G-06); the production instrument file's content and age.

## 10. Index data

- FACT (CONFIG); FACT (TEXT): the index list is data only, resolved by name at fetch time from the INDICES segment; indices carry no volume; the regime needs the 09:15 to 10:00 OHLC (`config/index_universe.yaml:1-22`) (MC-45).
- FACT (SOURCE): the post-close script fetches the configured indices first, into the candles table (`scripts/fetch_daily_candles.py:66-109, 159`) (MC-35, MC-36). The shared closure feeds the regime runner (`main.py:3510-3540`) (MC-28), and OhlcFetcher fetches an index by an explicit configured token (`sr_detector/fetch.py:85-101`) (MC-30).
- FACT (CONFIG): regime is disabled in the repository configuration, which names NIFTY 50 by a configured token (`config/system_config.yaml:547-562`) (MC-29).
- UNKNOWN / NOT ESTABLISHED: whether any index path ran in operation (gap G-03).

## 11. Derived daily statistics (M-S4)

- FACT (TEXT); FACT (SOURCE): core/daily_stats.py computes prev_close, avg_volume_20d, atr14 and rsi14 from daily candles closed before the trading date (`core/daily_stats.py:34-74`); its header says nothing on the live path consumes it yet, behind a default-off flag (`:14-15`), and names a cache job, scripts/build_daily_symbol_stats.py (`:7`) (MC-53). FACT (PATH NAME): that script is absent from the pinned tree (observation O-05).
- FACT (SOURCE); FACT (TEXT): the daily_symbol_stats table exists (`core/schema.sql:1593-1606`); the state store's writer says the table is filled only by the about-08:30 pre-market cron (`core/state_store.py:3136-3140`), and its reader says the screener's _build_market_data uses it and falls back to 0.0 or 0.5 on a miss (`:3156-3159`) (MC-54).
- FACT (SOURCE): the V3 forward shadow recomputes the same four inputs and falls back the same way (`v3_chain/forward_shadow.py:32-45`) (MC-56).
- INTERPRETATION; OPEN QUESTION (MC-55): with the cache job absent at the pin, nothing read here fills the table, and D-003 MC-37 records these inputs as always None on the processor's path. Whether _build_market_data reads the table, and under which flag, is NOT ESTABLISHED in D-004: the screener is an R3 path, cited from frozen manuals and not read in D-004. Proposed bookmark, section 17.
- UNKNOWN / NOT ESTABLISHED: the table's other writers and readers (gap G-05); the pre-market job's schedule (gap G-06).

## 12. Time authority, session and calendar

- FACT (SOURCE): now_ist() is the VM clock at a fixed +05:30 (`core/time_authority.py:55-59, 94-99`); skew samples are recorded against it (`:171-204`); a startup check raises when the single skew reading exceeds the limit (`:263-297`) (MC-46). FACT (CONFIG): skew tiers of 2, 5 and 30 s, 30 s at startup, a probe every 60 s (`config/system_config.yaml:183-190`) (MC-46).
- FACT (TEXT): the skew probe's docstring says get_server_time() returns local time after a round trip, not a broker timestamp, so it measures path consistency rather than true broker skew; the probe is not built in paper mode (`broker/clock_skew_probe.py:19-32`) (MC-47).
- FACT (CONFIG); FACT (SOURCE): the session runs 09:15 to 15:30 (`config/system_config.yaml:103-104`); MarketWindows takes the caller's IST now and applies special sessions (`core/market_windows.py:7-13, 38-44, 194-216, 237-260`); the broker API is treated as unavailable after 17:30 on Friday (`:269-272`) (MC-48).
- FACT (SOURCE): holidays come from config/nse_holidays_<year>.yaml, cached, and a missing file raises (`utils/holiday_guard.py:21-49`); the 2026 file's content was not read (no count hit) (MC-49).
- Timestamp bases, from the facts above: quotes carry the VM time at receipt (MC-03); ticks carry the tick's exchange or tick timestamp, and each batch the receipt time (MC-16); live candles carry the VM close time (MC-21, MC-23); historical and post-close candles carry the broker row's date (MC-31, MC-36), whose start-or-end meaning is UNKNOWN (gap G-02).
- UNKNOWN / NOT ESTABLISHED: true broker-clock skew (MC-47); the deployed clock and session settings (gap G-03).

## 13. Pacing, limits, caches and retries

- FACT (CONFIG); FACT (TEXT): on HTTP 429 the configuration lists a backoff of 1, 5 and 30 s, then a soft kill in code; HTTP timeouts of 5 s to connect and 10 s to read; a bucket freeze of 0.2 s doubling to 5 s with 0.05 s jitter (`config/broker_limits.yaml:1-8, 22-41`) (MC-50). HISTORICAL: quote 3 per second and historical 2 per second buckets (`docs/agenda/a-006_broker-model/A-006_manual.md:56-58`); Q-30 stays open (`docs/governance/V2_DESIGN_LOG.md:200`) (MC-50).
- FACT (TEXT): the rate limiter has four categories, a blocking acquire up to a 30 s default followed by BrokerRateLimitError, a bucket freeze and a monotonic clock (`broker/rate_limiter.py:4-29, 45`) (MC-51).
- FACT (SOURCE); FACT (TEXT): the post-close script paces itself with fixed 0.35 s sleeps (`scripts/fetch_daily_candles.py:106, 208`), and no rate-limiter call appears in the lines read (gap G-09). A cron comment says the forward-shadow recorder reuses the limiter at up to 3 requests per second (`config/cron_registry.yaml:389-395`) (MC-52).
- HISTORICAL: the paper quote provider makes at most one call per 0.35 s (`docs/agenda/b-004_broker-connection/B-004_manual.md:34`) (MC-07).
- FACT (SOURCE); FACT (CONFIG) - caches: the OHLC fetchers cache by (symbol, interval) for a TTL (`sr_detector/fetch.py:4-21, 53-151`) (MC-30); the repository's S&R detector cache is 1,800 s (`config/system_config.yaml:508-545`) (MC-29), and the PB-01 watchlist's default cache is 0 (`main.py:3704-3745`) (MC-28). The instrument cache's load path has no file-age or freshness check (`core/instrument_cache.py:98-186`) (MC-40).
- UNKNOWN / NOT ESTABLISHED: the deployed values (gap G-03); the broker's limits beyond the HISTORICAL record (Q-30).

## 14. Other persisted market data

- FACT (SOURCE): market_execution_context stores, per order leg, ltp, bid and ask price and quantity, spread, volume_traded and recent_range_pct (`core/schema.sql:1123-1142`); its writer was not searched, because S11 is limited to the two schema files (MC-57; gap G-05).
- FACT (SOURCE): fno_ban holds symbol, ban_date and fetched_at (`core/schema.sql:991-999`); the fetch script is D-002's (`docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:230`) (MC-58).
- FACT (SOURCE): other data-bearing tables, by name only (definitions not read): screener_results (`core/schema.sql:681`), trade_excursions (`:824`), trade_slippage_log (`:1083`), sr_detector_results (`:1258`), excursion_reconstruction_runs (`:1361`) and pb01_watchlist (`:1559`); the schema version is 45 (`:1610`) (MC-59).
- FACT (TEXT); UNKNOWN: db_retention prunes the append-only tables after the nightly backup and vacuums both databases (`scripts/db_retention.py:3-9, 25-29, 143-149`); the per-table retention map was not in the located ranges, so the candles retention is UNKNOWN (MC-60; gap G-07).
- FACT (SOURCE): the regime runner writes its state to a JSON file (`main.py:3510-3540`) (MC-28).

## 15. Paper and live distinctions (legacy history only; no parity assumed)

- FACT (SOURCE) - quotes: the adapter gets a quote_provider only in paper mode (`main.py:2389-2395`); the paper provider builds its own client from the broker session file (a never-open path, named in code only) with an HTTP pool of 50 (`main.py:495-506`), and calls quote with a translated symbol list prefixed "NSE:", counting any missing (`main.py:543-549`) (MC-07); in paper mode get_quote delegates to it (`broker/zerodha_adapter.py:1742-1755`) (MC-06). Live quotes come from the adapter's kite.quote call (section 4).
- FACT (SOURCE) - other paths: paper historical data comes from a read-only client (`main.py:393-407`) (MC-27); the WebSocket is not connected in paper mode (`data/live_feed.py:136-140`) (MC-13); the skew probe is not built in paper mode (`broker/clock_skew_probe.py:19-32`) (MC-47). MC-61 records these differences together.
- V2 is live-only (DR-001): none of this is a V2 requirement, and no paper/live parity is assumed.

## 16. Known defects, interpretations, gaps, observations and open questions

### 16.1 Known defects

None is recorded as a known defect by D-004. The items below are interpretations, gaps, observations and open questions.

### 16.2 Interpretations (each rests on cited facts)

- MC-05: the quote mapping takes no previous close, which matches D-003 MC-37 (section 4).
- MC-38: the candles table could hold two ts values for one minute if both writers ran (section 8; gap G-02).
- MC-43: besides the cache, at least the candle script keeps its own name-to-token map (section 9).
- MC-55: nothing read here fills daily_symbol_stats (section 11).

### 16.3 Gap register (Stage 1 section 6, rows G-01 to G-10, carried verbatim)

| ID | Kind | Point | Why open | Next |
|---|---|---|---|---|
| G-01 | UNKNOWN | the broker's response-key case | broker behaviour; not in source (MC-10) | C-004 U-03 (BK-67) |
| G-02 | UNKNOWN | whether the historical "date" marks a bar's start; hence MC-38 | no official source found in this step | reconciliation or a later topic |
| G-03 | UNKNOWN | the deployed values of every flag and setting in MC-29, MC-46, MC-48, MC-50 | repository configuration only | none in legacy documentation (VM never read) |
| G-04 | UNKNOWN | the definitions of the six flags tested at main.py:3480 | above line 3463, not count-located | a later read, if authorised |
| G-05 | UNKNOWN | writers and readers of the candles, market_execution_context and daily_symbol_stats tables beyond MC-25, MC-36 and MC-54 | S11 limited to the schema files; no table-name search authorised | a table-name count search, if authorised |
| G-06 | UNKNOWN | the schedule of instrument refresh, candle fetch and stats jobs in the cron registry | the job entries were not count-located; times come from docstrings and the control tower (MC-34, MC-39, MC-54) | a job-name count search, if authorised |
| G-07 | UNKNOWN | candles retention value | retention map not located (MC-60) | as G-05 |
| G-08 | UNKNOWN | who calls candle_math outside the regime and the watchlist comment | no caller search in S01 to S13 (MC-33) | a caller search, if authorised |
| G-09 | LIMIT | negative statements ("no X in Y") cover only the lines read or the authorised scopes searched | bounded reading | - |
| G-10 | LIMIT | test and fixture evidence (MC-10, MC-44) shows what tests assert, not runtime | G-04 of the authorisation | - |

Reading notes, not changes: in G-04, "line 3463" is `main.py:3463`; in G-10, "G-04 of the authorisation" is gate answer G-04; in the "Next" column, "as G-05" means as gap G-05. The unresolved items the reconciliation names stay open here: the broker's response-key case (G-01), the historical timestamp semantics (G-02), the deployed values (G-03), the six flags' definitions (G-04), the broader table writers and readers (G-05), the cron schedule (G-06), the retention map (G-07) and all candle_math callers (G-08).

### 16.4 Observations (no finding; Stage 1 section 6)

| ID | Point | Why open | Next |
|---|---|---|---|
| O-01 | live_feed configuration keys not passed at main.py:2927-2938 (MC-12) | text against code | whole-design review |
| O-02 | candle_math header "Wiring: NONE yet" against the regime imports and a configuration comment (MC-33) | text against code | whole-design review |
| O-03 | freshness status vocabulary: LATE in the schema comment, OVERDUE in code (MC-39) | text against code | whole-design review |
| O-04 | stale adapter line numbers in a test docstring (MC-10) | text against code | none |
| O-05 | daily_stats names a cache job absent from the tree (MC-53) | text against tree | the proposed bookmark, section 17 |
| O-06 | main.py passes no kill switch to the live feed, whose queue-full branch then only logs (MC-17); whether one is set later is NOT ESTABLISHED | code against code | whole-design review |

The pointer "whole-design review" is the one Stage 1 recorded; it is not a V2 decision.

### 16.5 Open questions (none is decided or changed here)

- MC-55 (section 11): whether the screener reads daily_symbol_stats, and under which flag. MC-38 (section 8): the broker's historical timestamp convention. Both are the subject of the proposed bookmarks in section 17.
- Carried unchanged, as the Stage 0 report (section 7) carries them: D-003's seven open questions (BK-46, BK-54, BK-55, BK-56, BK-58, BK-63, Q-34); its bookmarks BK-64 to BK-68; and the data-related rows Q-20 and Q-21 (`docs/governance/V2_DESIGN_LOG.md:190-191`), Q-30 (`docs/governance/V2_DESIGN_LOG.md:200`), BK-22 and BK-31 (`docs/governance/V2_DESIGN_LOG.md:573, 582`), BK-52 (`docs/governance/V2_DESIGN_LOG.md:603`), BK-59, and BK-67 (`docs/governance/V2_DESIGN_LOG.md:618`).
- The V2 proposals that touch data (VP-03, VP-09, VP-10, VP-14, VP-27, VP-28, VP-30, VP-39, VP-49 and VP-68; `docs/governance/V2_DESIGN_LOG.md:225-289`) stay PROPOSED and are not legacy evidence. No owner change, no closure, no new decision number.
- Records (gate answer G-03): no named record is essential to the Stage 1 questions; MC-15 quotes a record only through a code comment.

## 17. PROPOSED - NOT WRITTEN

The two proposed bookmarks below are not written to `docs/governance/V2_DESIGN_LOG.md`, and nothing in this chapter depends on their being written. Both stay PROPOSED / OPEN until ChatGPT's and Rama's review. The wording is Stage 1's (D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt, section 6), offered for the design log's section 7 only if the review accepts it.

- BK-69 (PROPOSED - NOT WRITTEN): D-004 MC-53 to MC-55: daily_symbol_stats exists with a state-store writer and reader whose docstrings say the screener's _build_market_data reads it and that an about-08:30 cron fills it, while the named cache job is absent from the tree and D-003 MC-37 records the inputs as always None; whether the screener reads the table, and under which flag, is not established. Cross-check with: D-003; whole-design review. Status: OPEN.
- BK-70 (PROPOSED - NOT WRITTEN): D-004 MC-38: the candles table takes rows from two writers with different ts conventions (close time against the historical row's date); the broker's convention is not established. Cross-check with: D-005; whole-design review. Status: OPEN.

## 18. Cross-references (V2 records, HISTORICAL; cited, not re-verified)

- The topic: `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:295`; the neighbouring topics at lines 296 to 303 (section 1.3).
- Frozen manuals: D-001 (via `docs/agenda/d-003_secondary-screening/D-003_manual.md:46`), D-002 (`docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:230-244`), D-003 (`docs/agenda/d-003_secondary-screening/D-003_manual.md:48, 262-280, 325`).
- C-004 (`docs/agenda/c-004_validation/C-004_manual.md:67-73, 222, 232, 427`); C-011 (`docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md:449, 872`).
- B-004 (`docs/agenda/b-004_broker-connection/B-004_manual.md:34-35`); B-006 (`docs/agenda/b-006_worker-initialization/B-006_manual.md:40`); A-006 (`docs/agenda/a-006_broker-model/A-006_manual.md:56-60`).
- The design log's rows as cited in sections 1.3 and 16.5; the start files (`docs/governance/V2_DESIGN_LOG.md:160`).

## 19. Evidence appendix

### 19.1 Inputs to this draft

| File | Lines | Bytes | SHA-256 |
|---|---|---|---|
| D004_CLAUDE_PROMPT_PREPARE_CANDIDATE_MANUAL_NO_REPO_WRITES_27-OCT-2026.txt | 46 | 3,625 | D07706218997B40414AB5CEA8A92FA67E2B29E31235EC562EADC4B7CF72DAB89 |
| D004_CHATGPT_RECONCILIATION_STAGE1_AND_NEXT_STEP_27-OCT-2026.txt | 52 | 5,892 | CF73A6B5503164176D2CA4A3AE34F6A0AE6937A061D7B97BF76509030B406DC7 |
| D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt | 389 | 40,832 | E3CF8C34C6D6ABF70B689EB180C4159D6CD1AA2F16DA20CB5234340B1F2B2BEC |
| D-004_STAGE1_FILES_READ_AND_COUNT_LOG_27-OCT-2026.txt | 419 | 41,415 | A9D833E516190BE3E3D2393C04483977A199437841A495B06EFC9D62D21637BF |
| D-004_STAGE1_COUNT_RESULTS_27-OCT-2026.txt | 2616 | 191,797 | FEAE1DE42CBF37CF07CD51E371A63D630FC81B398A6613126C8B39B0DC8A06F5 |
| D-004_STAGE1_FINAL_CHECK_27-OCT-2026.txt | 41 | 3,836 | 2146D7A3635E48D59BA451E002B7EDB30F9A215839FE4AC2A55ACDA84F8AF571 |
| D-004_SOURCE_INVENTORY_AND_RESEARCH_PLAN_27-OCT-2026.txt | 753 | 72,435 | 4EFF3CAC10F8692A8B03BF83361E104EAA573BA582C36D1C40A3E2025D2DB930 |
| D004_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_27-OCT-2026.txt | 80 | 7,818 | CDDA8BCF7851163AF8E49A152FA30CB4766AA771D9A91ACBF8A53A63EA22C54B |
| D004_STAGE1_GATE_ANSWERS_RAMA_27-OCT-2026.txt | 27 | 2,452 | 96DB4D98E508FF8C6D56B775C006E28C1447897D1335C1092FEF3ED0F9C5C5F1 |

No legacy file was read, searched or counted for this draft, and no V2 record was read for its content: V2 citations come from the Stage 0 and Stage 1 files, and their line totals were checked at c33466b (D-004_STAGE2_FINAL_CHECK_27-OCT-2026.txt).

### 19.2 Legacy files cited (read in Stage 1 at the pin; none read for this draft)

Pinned blob (first 12 characters), the file's line total and the Stage 1 read rows that cover this chapter's citations, all from the read log (section 5).

| File | Pinned blob | Lines | Stage 1 read rows | Cited in sections |
|---|---|---|---|---|
| broker/clock_skew_probe.py | 749d3905a15f | 142 | 133 | 12, 15 |
| broker/rate_limiter.py | b2f688e7ed84 | 273 | 047 | 13 |
| broker/zerodha_adapter.py | 0c85d2d254aa | 2521 | 006, 007, 008 | 4, 15 |
| config/broker_limits.yaml | ad05dd83fb2b | 41 | 045, 046 | 13 |
| config/cron_registry.yaml | 42ff638145ba | 768 | 164 | 13 |
| config/index_universe.yaml | 416d1eb723c5 | 29 | 132 | 10 |
| config/system_config.yaml | bf742ae5c81c | 754 | 139, 140, 141, 142 | 5, 7, 10, 12, 13 |
| core/analytics_schema.sql | 57deb055f4db | 91 | 053, 106 | 8 |
| core/candle_math.py | 57e69c5973ae | 457 | 031, 032, 033, 034 | 7 |
| core/daily_stats.py | 36fdf6b07b84 | 75 | 035 | 11 |
| core/instrument_cache.py | 2bff50f7657f | 298 | 041, 042, 043 | 9, 13 |
| core/market_windows.py | 25625e8cd109 | 334 | 134, 135, 136, 138 | 12 |
| core/schema.sql | b46d7489f6ec | 1655 | 068, 072, 079, 083, 084, 091, 093, 099, 103, 104, 105, 107, 108, 109, 110 | 8, 11, 14 |
| core/state_store.py | 11866117801b | 3356 | 111 | 11 |
| core/time_authority.py | a29c4f0795b1 | 396 | 036, 037, 039, 040 | 12 |
| data/candle_store.py | f44a831760e6 | 369 | 024, 025, 026, 027, 028, 029, 030 | 6 |
| data/live_feed.py | 10ebd00ee728 | 791 | 009, 010, 011, 012, 013, 014, 015, 017, 018, 019, 020, 022, 023 | 5, 15 |
| main.py | 445fb0d8237a | 4294 | 001, 002, 003, 004, 005, 112, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125 | 1.3, 4, 5, 6, 7, 9, 10, 13, 14, 15, 16.3 |
| ops/control_tower/freshness.py | 04b3cd96f5f3 | 120 | 161 | 8 |
| orders/shadow_tracker.py | 850722541f41 | 897 | 143 | 5 |
| orders/sl_breach_monitor.py | 01c29e7771c5 | 296 | 149 | 1.3, 2.5 |
| orders/smart_tgt_manager.py | 4c9698d6be4d | 897 | 144, 145 | 4, 6 |
| orders/structure_exit_manager.py | 6e4c58d8d2e8 | 673 | 146 | 4 |
| regime/engine.py | d9cdd9366857 | 360 | 169 | 7 |
| screening/retest_monitor.py | 26639d6ed5dd | 442 | 147, 148 | 7 |
| scripts/db_retention.py | 3de2dded7d81 | 231 | 156, 157, 158 | 14 |
| scripts/fetch_daily_candles.py | 4a149615dc48 | 347 | 048, 049, 050, 051, 052 | 8, 10, 13 |
| sr_detector/detector.py | a8500c48c8ac | 330 | 151, 152 | 7 |
| sr_detector/fetch.py | 38636d538667 | 160 | 044 | 7, 10, 13 |
| sr_detector/models.py | 4f49fac53868 | 208 | 131 | 7 |
| sr_detector/zone_warmer.py | a47058e77cfc | 162 | 154 | 7 |
| tests/fixtures/reference_data/security_master_small.csv | f23570b3e433 | 7 | 130 | 9 |
| tests/unit/test_mo1_flatten_quote_key.py | dcb0afda7592 | 95 | 168 | 4 |
| utils/holiday_guard.py | 688679256362 | 132 | 137 | 12 |
| v3_chain/forward_shadow.py | dc96649f0a9f | 106 | 155 | 11 |

### 19.3 V2 records cited (HISTORICAL)

| V2 record (at c33466b) | Lines cited |
|---|---|
| `docs/agenda/a-006_broker-model/A-006_manual.md` | 56-60 |
| `docs/agenda/b-004_broker-connection/B-004_manual.md` | 34-35 |
| `docs/agenda/b-006_worker-initialization/B-006_manual.md` | 40 |
| `docs/agenda/c-004_validation/C-004_manual.md` | 67-73, 222, 232, 427 |
| `docs/agenda/c-011_eod-daily-signal-routing/C-011_manual.md` | 449, 872 |
| `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md` | 230-244 |
| `docs/agenda/d-003_secondary-screening/D-003_manual.md` | 46, 48, 262-280, 325 |
| `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt` | 295-296, 298-303 |
| `docs/governance/V2_DESIGN_LOG.md` | 160, 190-191, 200, 225-289, 573, 582, 597, 603, 607, 618 |
| `docs/governance/V2_SESSION_HANDOVER_C-009_24-OCT-2026.md` | - (named, no line) |
| `docs/governance/V2_SESSION_HANDOVER_SECTION_C_21-OCT-2026.md` | - (named, no line) |

### 19.4 Claim-to-section map (MC IDs of D-004_STAGE1_SOURCE_EVIDENCE_27-OCT-2026.txt)

| Claim | Stage 1 label | Sections |
|---|---|---|
| MC-01 | FACT (SOURCE) | 1.3, 3, 4 |
| MC-02 | FACT (SOURCE) | 1.3, 3, 4 |
| MC-03 | FACT (SOURCE) | 1.3, 3, 4, 12 |
| MC-04 | FACT (SOURCE) | 1.3, 3, 4 |
| MC-05 | FACT (SOURCE) | 1.3, 3, 4, 16.2 |
| MC-06 | FACT (SOURCE) | 3, 4, 15 |
| MC-07 | FACT (SOURCE) | 3, 13, 15 |
| MC-08 | FACT (SOURCE) | 1.3, 3, 4 |
| MC-09 | FACT (COUNT) | 3, 4 |
| MC-10 | FACT (TEST) | 1.3, 2.8, 4, 16.3, 16.4 |
| MC-11 | FACT (TEXT) | 3, 5 |
| MC-12 | FACT (SOURCE) | 1.3, 5, 16.4 |
| MC-13 | FACT (SOURCE) | 5, 15 |
| MC-14 | FACT (SOURCE); FACT (TEXT) | 3, 5, 8 |
| MC-15 | FACT (TEXT) | 3, 5, 16.5 |
| MC-16 | FACT (SOURCE) | 3, 5, 12 |
| MC-17 | FACT (SOURCE) | 3, 5, 16.4 |
| MC-18 | FACT (SOURCE) | 3, 5 |
| MC-19 | FACT (SOURCE) | 1.3, 3, 5 |
| MC-20 | FACT (TEXT) | 6 |
| MC-21 | FACT (SOURCE) | 1.3, 3, 6, 12 |
| MC-22 | FACT (SOURCE); FACT (TEXT) | 1.3, 3, 6 |
| MC-23 | FACT (SOURCE) | 1.3, 3, 6, 8, 12 |
| MC-24 | FACT (SOURCE) | 6 |
| MC-25 | FACT (SOURCE); FACT (TEXT) | 3, 6, 8, 16.3 |
| MC-26 | FACT (SOURCE) | 3, 6 |
| MC-27 | FACT (SOURCE) | 3, 7, 15 |
| MC-28 | FACT (SOURCE) | 1.3, 3, 7, 10, 13, 14 |
| MC-29 | FACT (CONFIG) | 1.3, 3, 7, 10, 13, 16.3 |
| MC-30 | FACT (SOURCE); FACT (TEXT) | 1.3, 3, 7, 10, 13 |
| MC-31 | FACT (SOURCE) | 1.3, 3, 7, 12 |
| MC-32 | FACT (SOURCE) | 1.3, 3, 7 |
| MC-33 | FACT (SOURCE) | 1.3, 7, 16.3, 16.4 |
| MC-34 | FACT (TEXT); FACT (SOURCE) | 3, 8, 9, 16.3 |
| MC-35 | FACT (SOURCE) | 1.3, 3, 8, 10 |
| MC-36 | FACT (SOURCE) | 3, 8, 10, 12, 16.3 |
| MC-37 | FACT (SOURCE); FACT (TEXT) | 3, 8 |
| MC-38 | INTERPRETATION; UNKNOWN | 1.3, 8, 16.2, 16.3, 16.5, 17 |
| MC-39 | FACT (SOURCE) | 3, 8, 16.3, 16.4 |
| MC-40 | FACT (SOURCE); FACT (TEXT) | 1.3, 3, 9, 13 |
| MC-41 | FACT (SOURCE) | 1.3, 3, 9 |
| MC-42 | HISTORICAL | 1.3, 3, 9 |
| MC-43 | FACT (COUNT) | 1.3, 9, 16.2 |
| MC-44 | FACT (TEST) | 1.3, 2.8, 9, 16.3 |
| MC-45 | FACT (CONFIG); FACT (TEXT) | 1.3, 3, 10 |
| MC-46 | FACT (SOURCE) | 3, 12, 16.3 |
| MC-47 | FACT (TEXT) | 3, 12, 15 |
| MC-48 | FACT (CONFIG); FACT (SOURCE) | 1.3, 12, 16.3 |
| MC-49 | FACT (SOURCE) | 12 |
| MC-50 | FACT (CONFIG); FACT (TEXT) | 1.3, 13, 16.3 |
| MC-51 | FACT (TEXT) | 1.3, 13 |
| MC-52 | FACT (SOURCE); FACT (TEXT) | 3, 13 |
| MC-53 | FACT (TEXT); FACT (SOURCE) | 2.5, 3, 11, 16.4, 17 |
| MC-54 | FACT (SOURCE); FACT (TEXT) | 1.3, 3, 11, 16.3, 17 |
| MC-55 | INTERPRETATION; OPEN QUESTION | 3, 11, 16.2, 16.5, 17 |
| MC-56 | FACT (SOURCE) | 3, 11 |
| MC-57 | FACT (SOURCE) | 3, 14 |
| MC-58 | FACT (SOURCE) | 1.3, 14 |
| MC-59 | FACT (SOURCE) | 14 |
| MC-60 | FACT (TEXT); UNKNOWN | 14, 16.3 |
| MC-61 | FACT (SOURCE) | 15 |

### 19.5 Content not verified (no statement here relies on it)

- Deployed configuration, runtime behaviour, database contents and the VM's files (gap G-03).
- Whether any described path ran, including the live feed, the candle-store writer, the shared closure and the post-close script.
- The broker's response-key case (gap G-01) and its historical timestamp convention (gap G-02).
- The six flags' definitions (gap G-04); table writers and readers beyond those cited (gap G-05); the cron schedules (gap G-06); the retention values (gap G-07); all callers of candle_math (gap G-08).
- The content of the 13 named records, every R3 and NP path, config/reference_data/ and the 2026 holiday file.
- D-003's K-3 content checks, which were not re-run on its final candidate, stay NOT VERIFIED (D004_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_27-OCT-2026.txt, section 1).

### 19.6 Status

- DRAFT / NOT FROZEN: draft 1 for ChatGPT's review; not stored, committed or pushed; no register, design-log or agenda-status edit; DR-070 unused; no V2 decision.
- Companions: D-004_STAGE2_DRAFT_CHANGE_NOTE_27-OCT-2026.txt, D-004_STAGE2_SELF_REVIEW_CHECKLIST_27-OCT-2026.txt, D-004_STAGE2_CANDIDATE_PACKAGE_MANIFEST_27-OCT-2026.txt and D-004_STAGE2_FINAL_CHECK_27-OCT-2026.txt.
