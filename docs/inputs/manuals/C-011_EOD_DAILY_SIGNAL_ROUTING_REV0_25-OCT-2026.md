# C-011 - EOD / DAILY SIGNAL ROUTING: LEGACY MANUAL

Status: DRAFT, revision 0 (Claude, 25-Oct-2026), for ChatGPT's reconciliation; NOT FROZEN. Prepared under ChatGPT's C-011_MANUAL_DRAFTING_PROMPT_25-OCT-2026.txt (59 lines; SHA-256 2E6D873795F86D766D68C82380BB1C61041961304CB547BB1768FDA43F2BE8E1) and C-011_STAGE1_CHATGPT_RECONCILIATION_25-OCT-2026.txt (52 lines; SHA-256 2AB7F6099473F090E2D9452011B7EC2EFE92698E76D142FE3D02CB86134C3B85), both sent by Rama and received at 12:05 IST (file time). Drafted only from Claude's C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt (1,070 lines; SHA-256 47B5BF679B195ECA9E68F1D17E997FC1DFFB150C51103704149C426480D9D5B1) and that reconciliation; Claude's Stage 0 answers are cited only for governance items (VP-14 and OQ-04 to OQ-07). Nothing in the legacy repository was read, searched or counted for this draft, and no test, script or service was run. The draft goes back to ChatGPT for reconciliation; Claude does not declare it frozen, and nothing is committed or pushed. BK-08 and BK-30 stay OPEN; VP-14 stays NOT RATIFIED.  
Topic: C-011 EOD / Daily signal routing (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 25-Oct-2026 through the gated masking filter. Stage 1 made 32 count-only searches (Q01 to Q32) with 4 positive controls (Q33 to Q36) and 20 gated reads (001 to 020) of 15 files, printing 340 lines.  
Companion file: C-011_DRAFT_EVIDENCE_AND_GAP_CHECK.txt (each section's evidence, remaining unknowns and unsupported claims; the drafting interpretations DI-01 to DI-09 and drafting unknowns DU-01 to DU-05; the checks run on this draft).  
Purpose: a factual record of how the pinned legacy source routes an EOD-typed Chartink alert from the scanner map, through the receiver and the capture worker, to a pb01_watchlist row and the PB-01 entry stage's pickup of that row. It is not a V2 design or a recommendation to revive, repair or tune the legacy system. Governance context, not evidence: under the project's instructions no legacy behaviour is carried into V2 by default; this manual makes no V2 decision.  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name other than Rama's is reproduced. Protected paths are counted, never named. The values the filter masked (three class names shown as `<ALNUM>`, and the masked shapes counted in Appendix B of the evidence sheet) are not reproduced or inferred.

## 1. Purpose and scope

### 1.1 What this chapter records

At the pin, the one EOD-type route, hop by hop:

1. the scanner type in the map (section 5.2) and the watchlist settings (section 11.1);
2. the receiver's type branch, which hands an "eod"-typed key to the EOD handler (section 5.3);
3. the EOD handler: its checks, its hand-off to the capture worker and its HTTP answers (sections 5.4 and 6);
4. the capture worker: submit, the in-memory queue and the worker thread (sections 5.5 and 8);
5. the capture of one symbol: its guards, LEVEL and the trading-date stamp (section 7);
6. the store and the pb01_watchlist table, with its duplicate key (sections 5.6 and 10);
7. the pb01_capture heartbeat and the log lines (section 9);
8. the start-up wiring that builds the worker and binds it to the receiver (section 11);
9. the PB-01 entry stage's pickup of today's pending rows, where the chapter stops (section 12).

### 1.2 Scope basis

- Stage 0 raised three blocking questions; ChatGPT recommended option (a) of each, and Rama forwarded the Stage 1 prompt under his standing rule, which Claude took as the approval of OQ-01 to OQ-03 as worded (evidence sheet, header). OQ-01 (a): the eod-type route only. OQ-02 (a): the chapter ends where the PB-01 entry stage picks up a watchlist row. OQ-03 (a): count-only searches (S1-1), then bounded reads (S1-2).
- ChatGPT's Stage 1 reconciliation (section 4): "OQ-01 scope is supported"; "EOD / Daily" names "this one EOD-type route, not every daily-candle job/timeframe"; the OQ-02 boundary "remains at PB-01 entry-stage pickup; entry decisions and execution stay excluded"; "OQ-03 bounded Stage 1 is complete".
- The drafting prompt's scope: "scanner-type configuration -> receiver branch -> EOD handler -> capture-worker submission -> in-memory queue -> daily-candle LEVEL calculation/guards (as directly established) -> pb01_watchlist row/schema -> PB-01 entry-stage pickup boundary. Stop at pickup."

### 1.3 What "EOD / Daily" names here (INTERPRETATION, as approved)

The source calls the eod-typed alert "a DAILY/EOD alert" in three places: the receiver's routing comment (`signals/webhook_receiver.py:556`), the handler's docstring (`signals/webhook_receiver.py:752`) and the map line itself (`config/scan_webhook_map.yaml:84`). The capture then works on settled daily candles (section 7). On that basis, and as OQ-01 was approved, "EOD / Daily" in this chapter's title names this one route (INTERPRETATION, evidence sheet section 5). It does not name every job or timeframe that uses daily candles: S1-1 found daily_candles in 46 files of s_all, among them scripts/fetch_daily_candles.py (6 lines; a candle job by its name) (FACT, counts only); those files stay outside this chapter (OQ-01; G-09).

### 1.4 Out of scope (cited, not re-analysed)

- The PB-01 entry stage after its pickup: its decisions, the would-be records, the later status updates and any execution (OQ-02; G-05).
- The receiver before line 551 (authentication) and after line 564 (kill switch, queue depth, entry window): not read; described by the frozen manuals (HISTORICAL: C-003 7.3; C-006 4.1; B-008 3).
- The intraday signal path, documented by C-002 to C-010.
- Other users of daily candles and other "eod" mentions: the daily_candles files other than the capture worker (46 files in all) and the T8 files outside the route (G-09).
- The dated records and the schedule files that Stage 1 kept at counts only: the records R33 to R42 of the Stage 0 answers (R42 never read; G-10), config/cron_registry.yaml and deploy/cron/trading-system.cron (evidence sheet, section 8).
- What the production host ran, and which alerts, heartbeats or rows existed (G-11; Q-44 (a)).
- Any V2 design. VP-14 is not ratified here (section 14.2).

## 2. Baseline and evidence method

### 2.1 Baseline (FACT, as reported in the evidence sheet, section 1)

| Item | Value |
|---|---|
| Repository | ramakrishnan031-ctrl/trading-system-prod-snapshot |
| Pinned commit | 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11) |
| Remote check | git ls-remote showed the remote's HEAD and main at the pin at 08:47:53 IST (Stage 0) and at 11:46:39 IST |
| Tree listing | 1,438 entries; SHA-256 1F195D3D516AB4F194BCB7AF105FB193A606EE241E2E1437102EF495D8E647A5; unchanged |
| Clone | partial (blob:none) sparse clone; 8,920 local objects (1,257 blobs, 1,537 commits, 6,126 trees) before W1, after W1 and after W2; protected blobs present 0 each time |
| File integrity | all 1,260 files of s_all equal their pinned blobs (1,260 of 1,260, before the first search) |

### 2.2 Stages and execution controls (reported by Claude)

- **Stage 0** (C-011_CLAUDE_STAGE0_ANSWERS_25-OCT-2026.txt): the inventory and the scope questions OQ-01 to OQ-07. ChatGPT's Stage 0 review and decision sheet was not received by Claude (section 2.5).
- **Stage 1** under ChatGPT's C-011_STAGE1_CLAUDE_PROMPT_DRAFT_25-OCT-2026.txt, in two lock windows: W1, 11:28:57 to 11:33:26 IST, for S1-1; W2, 11:35:40 to 11:36:13 IST, for S1-2. The read lock was engaged again after each window, and a read attempt and a count attempt were then both refused.
- **Gate tests** (W1 and W2, both passed): the 102 protected paths, the two July 2026 queue records, a path sharing a protected blob, 14 rule-shaped names and a name outside the tree were refused; the 12 counts-only files were refused by name; the printed-line cap was tested on synthetic text.
- **S1-1**: 32 count-only searches, eight terms over the four C-010 scopes (s_all 1,260 files, s_rt 281, s_tests 486, s_cfg 41, each checked against its recorded SHA-256), and 4 positive controls for the 4 zero results. The unit is matching lines per file (a line with two matches counts once); each expression is case-sensitive and matched on the raw line; no line was printed.
- **S1-2**: a plan bounded in advance at 388 printed lines (ranges 298, greps 90), under the 400-line stop; 20 reads of 15 files printed 340 lines. The schema statement closed at line 1576, so the planned second schema read (O07) was skipped.
- ChatGPT's reconciliation (section 1): the stage "appears compliant with the Stage 1 prompt based on Claude's evidence sheet; this is not an independent reproduction of its local command logs."

S1-1 totals (FACT, counts only; matching lines; Appendix A of the evidence sheet has the per-file counts):

| Term | Expression | s_all files / lines | s_rt | s_tests | s_cfg |
|---|---|---|---|---|---|
| T1 | `eod_capture` | 11 / 40 | 2 / 10 | 2 / 15 | 0 / 0 (Q25) |
| T2 | `pb01_watchlist` | 30 / 95 | 8 / 34 | 4 / 23 | 2 / 3 |
| T3 | `_handle_eod` | 7 / 15 | 1 / 2 | 0 / 0 (Q19) | 0 / 0 (Q27) |
| T4 | `WatchlistCapture` | 9 / 14 | 3 / 5 | 2 / 5 | 1 / 1 |
| T5 | `pb01[_-](capture\|entry)` | 24 / 71 | 4 / 27 | 4 / 5 | 1 / 4 |
| T6 | `watchlist` | 59 / 350 | 12 / 110 | 11 / 80 | 4 / 15 |
| T7 | `daily_candles` | 46 / 119 | 7 / 19 | 2 / 8 | 3 / 7 |
| T8 | `["']eod["']` (the quoted literal eod) | 11 / 27 | 5 / 12 | 4 / 13 | 0 / 0 (Q32) |

Each zero result is FACT only for its expression and scope; each has its positive control (Q33 to Q36). 104 files of s_all match at least one term.

### 2.3 Labels

- **FACT**: what a line printed by the Stage 1 reads says or does by itself (a literal, a condition, a return, a call), or a count of S1-1. FACT (CONFIG), FACT (SCHEMA), FACT (CODE COMMENT) and FACT (TEST SOURCE) mark the kind of line. A code comment or a docstring is FACT for what it says, not proof that the code does it. A count is FACT for that count only.
- **HISTORICAL**: stated in a frozen V2 manual at the same pin; cited as the evidence sheet cites it, not re-read.
- **INTERPRETATION**: a reading that combines printed lines, or reads a name, or reasons about code that was not run; its basis is stated. Interpretations first made in this draft carry an ID, DI-01 to DI-09 (DI-09 groups the small readings by name or by position), and are listed in the companion file for ChatGPT's review; the others cite their source (the evidence sheet or the reconciliation).
- **UNKNOWN / NOT ESTABLISHED**: not shown by the approved evidence.
- **KNOWN DEFECT**: the one minor defect that ChatGPT's Stage 1 reconciliation classifies as such (section 14.1).
- **OPEN QUESTION**: a question the evidence cannot settle (G-06; section 14.3).

A statement takes the label of its weakest support. No test was run: test names are FACT (TEST SOURCE) for the names only.

### 2.4 Line references and quotations

- A reference in backticks, `path:line` or `path:first-last`, cites only lines printed by the Stage 1 reads at the pin (Appendix B of the evidence sheet); the read number is given where it helps. A bare line number beside a reference is a line of the same file and read. A line shows source at the pin, not what any host ran.
- Code excerpts in fenced blocks repeat printed lines, in the form "N: text", with the file and read named above them; only the character rule below is applied to them.
- Quotations repeat the printed words. The source's few non-ASCII characters are written in ASCII: the arrow as "->", the dash as "-", the multiplication sign as "x"; the star, warning, no-entry and bullet signs and the box-drawing rules of section comments are left out; the source's backticks inside comments are dropped. A quotation that runs over several source lines is joined with single spaces (a word hyphenated at a line end is joined without one), and a message string that the source splits into two adjacent string literals is quoted as the one string they form. No other change is made inside quotation marks.
- A printed grep line can be a fragment of a sentence; such a fragment is quoted as printed and marked "(cut by the grep)".

### 2.5 Evidence set

| # | File | From | Lines | SHA-256 |
|---|---|---|---|---|
| E1 | C-011_STAGE0_CLAUDE_RESEARCH_PROMPT_25-OCT-2026.txt | ChatGPT | 59 | 51AB0A7E6E5F8876BA6DC80D9ED2155C23504F27D2ABD57F9D1884BEC334D6CA |
| E2 | C-011_STAGE0_ANSWER_SHEET_25-OCT-2026.txt | ChatGPT | 65 | 94E5FDACA4347637E8F34F50CCA0DB2CBFF6183AB7CE95754B546513234FBA18 |
| E3 | C-011_CLAUDE_STAGE0_ANSWERS_25-OCT-2026.txt | Claude | 515 | 6B52C6B7A374ADF4005FF819D6E81B9DD9CFE00EADAC5890CF9C8245F95F0D65 |
| E4 | ChatGPT's Stage 0 review and decision sheet (its name begins C-011_STAGE0_RECONCILIATION_AND_DECISIO) | ChatGPT | NOT RECEIVED by Claude | to be supplied by Rama at the freeze |
| E5 | C-011_STAGE1_CLAUDE_PROMPT_DRAFT_25-OCT-2026.txt | ChatGPT | 69 | 7868FE4E2C65FA723F5B61D0C10D706826868974529EB17F3AFBCB05E1830515 |
| E6 | C-011_CLAUDE_STAGE1_EVIDENCE_SHEET_25-OCT-2026.txt | Claude | 1,070 | 47B5BF679B195ECA9E68F1D17E997FC1DFFB150C51103704149C426480D9D5B1 |
| E7 | C-011_STAGE1_CHATGPT_RECONCILIATION_25-OCT-2026.txt | ChatGPT | 52 | 2AB7F6099473F090E2D9452011B7EC2EFE92698E76D142FE3D02CB86134C3B85 |
| E8 | C-011_MANUAL_DRAFTING_PROMPT_25-OCT-2026.txt | ChatGPT | 59 | 2E6D873795F86D766D68C82380BB1C61041961304CB547BB1768FDA43F2BE8E1 |

Claude's local Stage 1 records (the lock, search and read logs, the S1-1 results and the S1-2 plan) are named with their SHA-256 values in E6, sections 1 to 4. The storage list belongs to the freeze package.

### 2.6 Limits of the evidence

- The reads printed 340 lines of 15 files. Everything else in those files (for example the capture worker's lines 1 to 85 and 179 to 245) and every other file is UNKNOWN unless a frozen manual is cited for it (HISTORICAL).
- Lines that hold a term in the files read but were not printed are listed per file in the evidence sheet, section 7; they are gaps, not evidence.
- Nothing shows runtime behaviour: no test, script, service or database was run or opened.
- Masked values were never examined.

## 3. End-to-end route diagram

```
  Chartink EOD alert (code comments speak of "the 17:00 alert"; host arrivals NOT ESTABLISHED, section 15.2)
        |
        v
  [1] config/scan_webhook_map.yaml:84            scanner_type: eod                        FACT (CONFIG)
        |   (the map is loaded into the receiver's config; the load itself not read)
        v
  [2] signals/webhook_receiver.py:551-564        part of _process_request                 FACT
        |   key not in the map              -> 404 "Unknown scanner" (553-554)
        |   scanner_type == "eod"           -> return self._handle_eod(...) (562-564)
        |   (nothing after line 564 runs for this request)
        v
  [3] signals/webhook_receiver.py:751-804        _handle_eod                              FACT
        |   5 checks fail                   -> 400, no heartbeat (757-773)
        |   no worker bound                 -> heartbeat FAILED/DISABLED, 200 "watchlist disabled" (778-788)
        |   per symbol: alias, submit       -> captured += 1 for each True (790-798)
        |   heartbeat "queued=N"            -> 200 {"accepted", "captured"} (799-804)
        v
  [4] v3_chain/watchlist_capture.py:105-118      submit                                   FACT
        |   put_nowait(...) -> True; full queue or other error -> False (logged)
        v
  [5] in-memory queue self._q                    (size set outside the lines read: G-03)
        v
  [6] v3_chain/watchlist_capture.py:121-178      thread "pb01-capture": _run -> _capture  FACT
        |   skip: no breakout_date / same-day breakout before the close / too few sessions
        |   LEVEL from the worker's own daily candles (compute_breakout_level, 20 sessions)
        |   trading_date = next_trading_day(after the breakout's EOD)
        |   row: status PENDING, source = the scanner key
        v
  [7] core/state_store.py:3078,3088              insert_pb01_watchlist: INSERT OR IGNORE  FACT (body not read)
        v
  [8] core/schema.sql:1559-1576                  pb01_watchlist, UNIQUE (symbol, trading_date)   FACT (SCHEMA)
        v
  [9] v3_chain/pb01_entry.py:151                 thread "pb01-entry":
        |                                        get_pb01_watchlist_for_date(today, pending_only=True)   FACT
  ======================  BOUNDARY (OQ-02): the entry stage's decisions are out of scope  ======================

  Start-up, main.py:3712-3755 (only if _watchlist_on, inside a try): build the fetcher, the capture worker,
  the would-be recorder and the entry stage; bind the worker to the receiver (3747); start both threads
  (3748-3749). Any exception: both names set back to None (3752-3755).                    FACT for the lines
```

## 4. Source-to-consumer trace table

### 4.1 The route (TR numbers are the evidence sheet's)

| Hop | TR | From (component) | Lines | What passes on | Guard or transformation | To (next consumer) | Label |
|---|---|---|---|---|---|---|---|
| H01 | TR-01 | scanner map entry | `config/scan_webhook_map.yaml:84` | an eod-typed map entry | scanner_type eod | the receiver's map lookup (552) | FACT (CONFIG); the key holding line 84 HISTORICAL (C-001 4.1; A-009 8) |
| H02 | TR-05 | receiver, key check | `signals/webhook_receiver.py:551-554` | scanner_name, raw_body | key must be in scan_webhook_map.scanners, else 404 | the type branch | FACT |
| H03 | TR-05 | receiver, type branch | `signals/webhook_receiver.py:562-564` | scanner_name, raw_body | scanner_type, default "intraday", compared with "eod" | _handle_eod; the request ends with its answer | FACT |
| H04 | TR-06 | EOD handler, checks | `signals/webhook_receiver.py:757-774` | the JSON body | five checks (400s); symbols split from stocks | the worker check | FACT |
| H05 | TR-06 | EOD handler, worker check | `signals/webhook_receiver.py:778-788` | the symbol count | no worker bound: FAILED/DISABLED heartbeat, 200 "watchlist disabled" | the HTTP client (ends) | FACT |
| H06 | TR-06 | EOD handler, hand-off | `signals/webhook_receiver.py:790-798` | (scanner_name, symbol, triggered_at) per symbol | alias map (792); submit's True counted | the capture worker's submit | FACT |
| H07 | TR-06, TR-07 | EOD handler, answer | `signals/webhook_receiver.py:799-804` | counts | info log; heartbeat (queued); 200 | the HTTP client; the heartbeat table (HISTORICAL: C-002 11, B-007 11) | FACT |
| H08 | TR-09 | submit | `v3_chain/watchlist_capture.py:106-118` | the tuple | put_nowait, else False | the in-memory queue | FACT |
| H09 | TR-10 | worker loop | `v3_chain/watchlist_capture.py:121-134` | one queued item | 1-second get; sentinel skipped; errors logged | _capture | FACT |
| H10 | TR-11 | capture of one symbol | `v3_chain/watchlist_capture.py:136-177` | scanner_name, symbol, triggered_at | breakout date; settled-only skip; LEVEL; trading_date; zone | the row dict | FACT for the lines; helpers not read (G-03) |
| H11 | TR-11, TR-12 | capture -> store | `v3_chain/watchlist_capture.py:178`; `core/state_store.py:3078`, `core/state_store.py:3088` | the row | INSERT OR IGNORE | pb01_watchlist | FACT for the lines; method body not read (G-04) |
| H12 | TR-13 | table | `core/schema.sql:1559-1576` | the stored row | UNIQUE (symbol, trading_date); status CHECK | the entry stage's read | FACT (SCHEMA) |
| H13 | TR-14, TR-12 | entry stage pickup | `v3_chain/pb01_entry.py:151`; `core/state_store.py:3094`, `core/state_store.py:3101` | today's pending rows | trading_date = today; pending_only=True (its handling not read, G-04) | the entry stage's decisions: BOUNDARY (OQ-02) | FACT for the lines |

### 4.2 Start-up and binding

| Hop | TR | From | Lines | What happens | To | Label |
|---|---|---|---|---|---|---|
| W01 | TR-02 | watchlist settings | `config/system_config.yaml:619-630` | enabled true; level_lookback_sessions 20; capture_fetch_lookback_days 60; entry window settings | main.py's wiring (through _watchlist_on, not read); the worker (`v3_chain/watchlist_capture.py:155`); the entry stage | FACT (CONFIG) |
| W02 | TR-03 | main.py wiring | `main.py:3712-3755` | only if _watchlist_on: build fetcher, worker, would-be recorder, entry stage; bind; start | the receiver (W03); the two threads | FACT for the lines; _watchlist_on's derivation UNKNOWN (G-01) |
| W03 | TR-04 | receiver binding | `signals/webhook_receiver.py:161`, `signals/webhook_receiver.py:195`, `signals/webhook_receiver.py:1156`, `signals/webhook_receiver.py:1160` | the constructor's eod_capture (default None) and set_eod_capture replace self._eod_capture | _handle_eod (778, 794) | FACT |

### 4.3 Source surfaced off the route

| TR | Source | Lines | What it shows | Label |
|---|---|---|---|---|
| TR-15 | config/expected_managers.yaml | `config/expected_managers.yaml:130-131`, `config/expected_managers.yaml:135-137`, `config/expected_managers.yaml:140` | entries pb01_capture_worker and pb01_entry_stage; the stale line pointer (section 14.1) | FACT (CONFIG); its reader not read (G-07) |
| TR-16 | scripts/v3_shadow_soak_report.py | `scripts/v3_shadow_soak_report.py:280-281`, `scripts/v3_shadow_soak_report.py:293` | a read-only SELECT of pb01_watchlist rows | FACT for the lines printed |
| TR-17 | core/migrations.py; ops_dashboard/backend/services/system_logs.py | `core/migrations.py:89`; `ops_dashboard/backend/services/system_logs.py:79` | a name in a migration line; a name in a tuple of logger names | FACT; the tuple's role INTERPRETATION |
| TR-18 | three test files | section 5.8 | test names; the end-to-end test's wiring line | FACT (TEST SOURCE) for the names and lines; not run (G-08) |

## 5. Component details

### 5.1 Component register

| Component | File (pinned blob) | Lines read (reads) | Responsibility at the pin, in the lines read | Not read |
|---|---|---|---|---|
| Scanner map | config/scan_webhook_map.yaml (54dd98d75f276f9b82027707702df2ee619b11f7) | 84 (010) | types one entry "eod" | the rest of the file (the key of line 84: HISTORICAL) |
| Receiver: key check and type branch | signals/webhook_receiver.py (0f15a0ab033529d80d9c14c993a5077690b72871) | 551-564 (001) | 404 for an unknown key; returns the EOD handler's answer for an eod-typed key | before 551 and after 564 |
| Receiver: EOD handler and heartbeat helper | same | 751-835 (002) | checks, alias, submit per symbol, heartbeat, HTTP answer | _parse_eod_triggered_at (G-02) |
| Receiver: worker binding | same | 161, 195, 754, 778, 783, 794, 1156, 1160 (003: every eod_capture line) | holds the worker in self._eod_capture | 1157-1159 (G-02) |
| Capture worker | v3_chain/watchlist_capture.py (25a1f084eadabc443a0dd22d8171b26584d4e897) | 86-178 (004) | thread start and stop; submit; loop; capture of one symbol; the insert call | 1-85 and 179-245 (G-03) |
| Store | core/state_store.py (11866117801b9329773a1be605448faef3c96f8d) | 3078, 3088, 3094, 3101, 3109, 3117 (015) | insert, select and update statements of pb01_watchlist | the three methods' bodies (G-04) |
| Table | core/schema.sql (b46d7489f6ec5e31149dd68957f3b9e45270571e) | 1540, 1559, 1578, 1579, 1590 (005); 1559-1578 (006) | the pb01_watchlist definition, key and index | lines 1541 to 1558 (DU-05) and the rest of the file |
| Start-up wiring | main.py (445fb0d8237a6d535ed00e5eb968c8176efd74f3) | 3712-3755 (007); 3706, 3747 (008) | builds, binds and starts the worker and the entry stage when _watchlist_on | before 3712 (G-01) |
| Watchlist settings | config/system_config.yaml (bf742ae5c81c60614370361998e6a102a1064ff7) | 619-630 (009) | the watchlist block | its other watchlist lines (G-12) |
| PB-01 entry stage (pickup only) | v3_chain/pb01_entry.py (cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8) | 117-125 (011); 5, 12, 18, 84, 151, 326 (012) | thread start; today's pending rows read | everything else (OQ-02; G-05) |

### 5.2 The scanner map entry (TR-01)

`config/scan_webhook_map.yaml:84` (read 010) is FACT (CONFIG):

```
config/scan_webhook_map.yaml (read 010)
84:     scanner_type: eod   # DAILY/EOD alert -> routed to the watchlist ONLY, never the intraday order path
```

- Line 84 alone does not show which map key holds it. HISTORICAL (C-001 4.1; A-009 8): it is the entry pb01_breakout_retest, and it is the map's only explicit scanner_type.
- HISTORICAL count, not repeated by C-011: scanner_type occurs in 8 files of s_all, 15 lines (C-010 S1-1, Q08).
- The receiver reads the attribute with a default: an entry without scanner_type is treated as "intraday" (`signals/webhook_receiver.py:563`, FACT).
- The watchlist block also names a scanner: playbook_scanner "pb01_breakout_retest", "the eod scanner this watchlist owns" (`config/system_config.yaml:621`, FACT (CONFIG)). In the lines read, the routing uses scanner_type, not playbook_scanner (563). HISTORICAL (C-001 4.4 and D-04): playbook_scanner has no reader. C-011 did not repeat that reader search (RC-03).

### 5.3 The receiver's type branch (TR-05)

```
signals/webhook_receiver.py (read 001)
551:         # WR4: scanner_name must be in scan_webhook_map
552:         known_scanners: dict[str, Any] = self._config.scan_webhook_map.scanners
553:         if scanner_name not in known_scanners:
554:             return jsonify({"error": f"Unknown scanner: {scanner_name!r}"}), 404
562:         _entry = known_scanners.get(scanner_name)
563:         if getattr(_entry, "scanner_type", "intraday") == "eod":
564:             return self._handle_eod(scanner_name, raw_body)
```

- FACT: a key that is not in the map is answered 404 (553-554) before the type branch. For an eod-typed key the function returns the handler's answer at 564, so nothing after line 564 runs for that request.
- FACT (CODE COMMENT), lines 556 to 561 (arrows and the dash written in ASCII):

> An "eod" scanner (a DAILY/EOD alert) is captured to the WATCHLIST only: it SKIPS the intraday entry-window gate (a post-close alert is legitimate) and is NEVER enqueued to the intraday signal_queue - so it is structurally INCAPABLE of the intraday order path (constraint #1). Default "intraday" -> this is a single skipped attribute read -> the live path is BYTE-IDENTICAL.

  The comment is FACT for what it says; its claim about the intraday path is not verified by C-011.
- The enclosing function is _process_request (HISTORICAL: C-002 17, T5); the handler's docstring names it: "Auth already ran in _process_request before this." (`signals/webhook_receiver.py:756`, FACT (CODE COMMENT)).
- Precision (reconciliation C; drafting prompt): in the inspected EOD branch, the receiver returns before normal intraday queue processing. The inspected handler (751 to 834) does not call _process_signal, does not write to signals SQL (no signals SQL is in those lines) and does not enqueue to the intraday queue. This is a statement about the inspected EOD path, not a claim that a hand-off is impossible anywhere in the system: the parser _parse_eod_triggered_at (called at 767) was not read (G-02), and other indirect paths were not traced (BK-30 stays OPEN, section 14.2).

### 5.4 The EOD handler (TR-06)

`signals/webhook_receiver.py:751-804` (read 002). Its docstring (752 to 756), FACT (CODE COMMENT):

> Route a DAILY/EOD scanner alert to the watchlist capture worker. Self-contained (does NOT touch the intraday parse, so that path stays byte-identical). Parses the Chartink payload, then hands each symbol to eod_capture.submit() - which does the heavy LEVEL fetch/compute OFF the request thread (WR1). NEVER enqueues to the intraday signal_queue. Auth already ran in _process_request before this.

The checks, the hand-off and the answers are in section 6; the heartbeat in section 9. In the lines read, the handler's only hand-off is the capture worker's submit (evidence sheet, section 5).

### 5.5 The capture worker (TR-08 to TR-11)

v3_chain/watchlist_capture.py, lines 86 to 178 (read 004):

- start (86 to 92): does nothing if a thread exists; otherwise clears the stop event and starts a daemon thread named "pb01-capture" that runs _run; logs "pb01 watchlist capture worker started" (FACT).
- stop (94 to 103): sets the stop event, puts a sentinel on the queue (a full queue is ignored), joins the thread for at most timeout = 3.0 seconds and forgets it (FACT).
- submit (105 to 118): the request thread's only call into the worker (section 8.3).
- _run (121 to 134): the loop (section 8.4).
- _capture (136 to 178): the capture of one symbol (section 7).

The section comments name the roles: "enqueue (called by the webhook EOD route; fast, guarded)" (105) and "background worker" (120) (FACT (CODE COMMENT); box-drawing characters left out).

### 5.6 The store methods (TR-12)

```
core/state_store.py (read 015)
3078:     def insert_pb01_watchlist(self, row: dict) -> bool:
3088:                 f"INSERT OR IGNORE INTO pb01_watchlist ({', '.join(cols)}) "
3094:     def get_pb01_watchlist_for_date(
3101:         q = "SELECT * FROM pb01_watchlist WHERE trading_date = ?"
3109:     def update_pb01_watchlist_status(
3117:                 "UPDATE pb01_watchlist SET status = ?, outcome_json = ?, consumed_at = ? "
```

- FACT for the six lines: the insert uses INSERT OR IGNORE and returns a bool; the select filters on trading_date; the update sets status, outcome_json and consumed_at. Each statement is read as part of the method whose def line precedes it, as the evidence sheet reads them (INTERPRETATION by position, DI-09 (g)).
- UNKNOWN (G-04): the rest of the three methods: what the bool means, how the column list is built, how pending_only is applied, and the update's WHERE clause.

### 5.7 The receiver's worker binding (TR-04)

```
signals/webhook_receiver.py (read 003)
161:         eod_capture: Any = None,   # V3 Step 10b: WatchlistCaptureWorker (None -> EOD alerts are a fail-safe miss)
195:         self._eod_capture = eod_capture
1156:     def set_eod_capture(self, eod_capture) -> None:
1160:         self._eod_capture = eod_capture
```

- FACT: the constructor takes eod_capture with default None and keeps it as self._eod_capture; set_eod_capture replaces it. The T1 grep printed all 8 eod_capture lines of this file.
- FACT (counts): in main.py eod_capture occurs on two lines only, 3706 (a comment) and 3747 (the set_eod_capture call) (`main.py:3706`, `main.py:3747`; read 008). INTERPRETATION from that count (evidence sheet, TR-04): main.py does not pass eod_capture to the receiver's constructor by that name. Whether its constructor call passes a worker by position was not read (UNKNOWN, G-01).
- UNKNOWN (G-02): lines 1157 to 1159 of set_eod_capture.

### 5.8 Other source that names the route (TR-15 to TR-18)

- config/expected_managers.yaml (TR-15, FACT (CONFIG)): the entry `- name: pb01_capture_worker` with `class: v3_chain.watchlist_capture.WatchlistCaptureWorker` (`config/expected_managers.yaml:130-131`) and acted_means "a pb01_watchlist ROW written - rows, never queued= (memory silent_failure_gaps_25jul)" (135); the entry `- name: pb01_entry_stage` (136), class name masked (137); and its effect_point (140), which carries the stale pointer of section 14.1. Who reads this file: not read (G-07).
- scripts/v3_shadow_soak_report.py (TR-16, FACT for the lines printed): _pb01_watchlist_rows (280), "Read-only load of pb01_watchlist rows (the capture/entry OUTCOME distribution)." (281), the query "SELECT symbol, trading_date, status FROM pb01_watchlist" (293), a printed note when the read fails (297), and, when there are no rows, the hint "(no pb01_watchlist rows - enable watchlist.enabled + capture a PB-01 EOD alert)" (323). It is a reader off the route.
- core/migrations.py (TR-17): one line naming "v43 pb01_watchlist (Step 10b PB-01 overnight watchlist)," (`core/migrations.py:89`); whether a migration step creates or alters the table is not shown (G-04).
- ops_dashboard/backend/services/system_logs.py (TR-17): the name "pb01_watchlist" in a tuple with "quality_scorer" and "sr_detector" (`ops_dashboard/backend/services/system_logs.py:79`). INTERPRETATION (evidence sheet, TR-17): a list of logger names; main.py's PB-01 logger is "pb01_watchlist" (`main.py:3731`).
- Tests (TR-18; FACT (TEST SOURCE) for the names; not run; bodies not read, G-08):

| Test file | Line | Test name |
|---|---|---|
| tests/unit/test_v3_eod_route.py | 59 | test_eod_route_captures_and_bypasses_entry_window |
| tests/unit/test_v3_eod_route.py | 72 | test_intraday_scanner_still_entry_window_gated |
| tests/unit/test_v3_eod_route.py | 82 | test_eod_route_failsafe_when_watchlist_disabled |
| tests/unit/test_v3_eod_route.py | 93 | test_set_eod_capture_late_binds_the_worker |
| tests/unit/test_v3_eod_route.py | 113 | test_eod_route_rejects_malformed_payload |
| tests/unit/test_v3_watchlist_capture.py | 39 | test_level_is_max_high_of_20_before_breakout |
| tests/unit/test_v3_watchlist_capture.py | 46 | test_level_ignores_breakout_day_and_later |
| tests/unit/test_v3_watchlist_capture.py | 53 | test_level_none_when_insufficient_history |
| tests/unit/test_v3_watchlist_capture.py | 72 | test_store_dedupe_unique_symbol_trading_date |
| tests/unit/test_v3_watchlist_capture.py | 81 | test_g_no_rehydration_stale_date_not_loaded |
| tests/unit/test_v3_watchlist_capture.py | 91 | test_store_update_status |
| tests/unit/test_v3_watchlist_capture.py | 124 | test_capture_settled_eod_persists_row |
| tests/unit/test_v3_watchlist_capture.py | 135 | test_capture_skips_forming_intraday_candle |
| tests/unit/test_v3_watchlist_capture.py | 146 | test_capture_dedupes_repeat_firing |

  tests/unit/test_v3_pb01_e2e.py "Wires the REAL pieces the way main.py does - WatchlistCaptureWorker (EOD) -> pb01_watchlist" (`tests/unit/test_v3_pb01_e2e.py:4`, cut by the grep), builds a WatchlistCaptureWorker (85), reads rows back by trading date (90) and asserts the status "CONSUMED" (103). TRADING, the test's trading-date value by its name, is defined in lines that were not read; so are the test's other lines.

## 6. Inputs, validations, guards and HTTP responses

### 6.1 Input

- The handler receives scanner_name and raw_body (`signals/webhook_receiver.py:751`, FACT). How the receiver takes them from the request, and the authentication before line 551, were not read (HISTORICAL: C-003 7.3).
- The body is JSON with three required fields, stocks, trigger_prices and triggered_at (763 to 765, FACT). The docstring calls it "the Chartink payload" (753 to 754).

### 6.2 Checks and answers, in order (FACT for the lines read)

| # | Lines | Check | HTTP answer (on failure, or the outcome) | Heartbeat |
|---|---|---|---|---|
| V1 | `signals/webhook_receiver.py:757-760` | json.loads(raw_body) succeeds (JSONDecodeError and ValueError caught) | 400 {"error": "Malformed JSON: ..."} with the exception text | none |
| V2 | `signals/webhook_receiver.py:761-762` | the body is a JSON object (a dict) | 400 {"error": "Request body must be a JSON object"} | none |
| V3 | `signals/webhook_receiver.py:763-765` | stocks, trigger_prices and triggered_at are present, checked in that order | 400 {"error": "Missing required field: ..."} naming the first missing field | none |
| V4 | `signals/webhook_receiver.py:767-769` | _parse_eod_triggered_at(str(body["triggered_at"]).strip()) does not return None | 400 {"error": "Invalid triggered_at"} | none |
| V5 | `signals/webhook_receiver.py:771-773` | stocks is a string | 400 {"error": "stocks must be a comma-separated string"} | none |
| X1 | `signals/webhook_receiver.py:774` | (no check) symbols = the comma-separated parts of stocks, stripped, empty parts dropped | - | - |
| X2 | `signals/webhook_receiver.py:778-788` | a worker is bound (self._eod_capture is not None) | 200 {"accepted": 0, "captured": 0, "detail": "watchlist disabled"} | one: status FAILED, functional status DISABLED (786-787) |
| X3 | `signals/webhook_receiver.py:790-798` | per symbol: alias map looked up with the upper-case symbol, the symbol itself kept when absent (792); submit(scanner_name=..., symbol=..., triggered_at=...) (793-796) | a submit exception is logged and the loop goes on (797-798) | - |
| X4 | `signals/webhook_receiver.py:799-804` | (no check) info log; heartbeat; answer | 200 {"accepted": number of symbols, "captured": number of True answers from submit} | one: status SUCCESS (the default, 807), functional status EMPTY_NO_DATA when nothing was queued, else none (801-803) |

```
signals/webhook_receiver.py (read 002)
774:         symbols = [s.strip() for s in stocks_raw.split(",") if s.strip()]
792:             symbol = self._alias_map.get(raw_symbol.upper(), raw_symbol)
794:                 if self._eod_capture.submit(
795:                         scanner_name=scanner_name, symbol=symbol, triggered_at=triggered_at):
796:                     captured += 1
804:         return jsonify({"accepted": len(symbols), "captured": captured}), 200
```

Notes:

- trigger_prices must be present, but its value is not used in lines 751 to 804 (FACT for those lines); it is not passed to submit.
- Of the three fields, only stocks is type-checked; there is no price check (consistent with C-004 T-18, HISTORICAL; RC-11).
- The alias map's content and where it is built were not read. INTERPRETATION (DI-06): a symbol that is not in the alias map is passed on in the spelling it arrived in; the lines read apply no case folding to it.
- An empty symbol list is not an error. INTERPRETATION (DI-05), from lines 774 and 790 to 804: with a worker bound, an alert whose stocks holds no symbol is answered 200 {"accepted": 0, "captured": 0} and writes one heartbeat with functional status EMPTY_NO_DATA; with no worker bound it gets the "watchlist disabled" answer.
- The submit call (793 to 798) and the heartbeat helper (824 to 834) are guarded by try blocks. Other statements of the handler are not inside a try in the lines read; what the web framework does with an exception raised elsewhere is NOT ESTABLISHED (DU-02).

### 6.3 What the handler does not check (FACT for lines 751 to 804)

- No signal expiry, dedup cache or fingerprint check is on this route (consistent with C-003 6.5 and L-01 and with C-005 10.2, HISTORICAL; RC-09, RC-12). Duplicates are left to the table's key (section 10.4); the worker's time rule is the settled-only skip (section 7.3), not a signal-expiry check (RC-15).
- No market-hours or entry-window check in the handler; the routing comment says the eod route "SKIPS the intraday entry-window gate (a post-close alert is legitimate)" (557-558, FACT (CODE COMMENT)).
- No check of the triggered_at value beyond the parser's None (V4); the parser's accepted formats and time zone are UNKNOWN (G-02).

### 6.4 HTTP answers on the route

| Answer | When | Lines | Label |
|---|---|---|---|
| 404 {"error": "Unknown scanner: ..."} | the key is not in the map (before the type branch) | 553-554 | FACT |
| 400 (five texts) | V1 to V5 fail | 757-773 | FACT |
| 200 {"accepted": 0, "captured": 0, "detail": "watchlist disabled"} | no worker bound | 788 | FACT |
| 200 {"accepted": N, "captured": Q} | the normal path; N symbols, Q of them queued | 804 | FACT |
| any other answer (for example of authentication) | before line 551 | not read | HISTORICAL: C-003 7.3 |

The 200 and 400 answers agree with C-002 9 row 7 (HISTORICAL; RC-05) and with C-001 8, A-L5 and D-06 (an EOD alert answers 200 with accepted = the symbol count; no signals row; RC-04).

### 6.5 What an answer proves

Successful submit means queued in memory, not database capture. HTTP 200 and `captured` do not prove row persistence. The response is built on the request thread right after submit (804); the level, the date stamp and the row are made later on the worker thread, where the capture may still skip or fail (sections 7 and 8; reconciliation A).

## 7. LEVEL and trading-date logic, with the uninspected helpers listed

### 7.1 Order of the capture (FACT for `v3_chain/watchlist_capture.py:136-178`, read 004)

```
v3_chain/watchlist_capture.py (read 004)
137:         now = self._safe_now()
138:         breakout_date = triggered_at.date() if triggered_at is not None else (now.date() if now else None)
146:         if now is not None and breakout_date == now.date() and not self._market_closed(now):
154:         daily = self._fetch(symbol, [_TF_DAY]).get(_TF_DAY) or []
155:         level = compute_breakout_level(daily, breakout_date, self._cfg.level_lookback_sessions)
164:         anchor = self._breakout_eod_dt(breakout_date)
165:         trading_date = self._mw.next_trading_day(anchor).isoformat()
168:         sr_zone_json = self._nearest_zone_json(symbol, level)
178:         inserted = self._store.insert_pb01_watchlist(row)
```

1. breakout date (138 to 141); 2. settled-only guard (143 to 151); 3. LEVEL (153 to 160); 4. trading_date (162 to 165); 5. S&R zone (167 to 168); 6. the row (170 to 177); 7. the insert (178).

### 7.2 breakout_date (138 to 141)

- FACT: breakout_date is triggered_at's date; if triggered_at is None, today's date from the clock; if that is also unavailable, None, and the symbol is skipped with the warning "pb01 capture %s: no breakout_date -> skip" (140).
- INTERPRETATION (DI-09 (a)): on this route the handler submits only after the parser returned a value that is not None (767 to 769, 793 to 795), so breakout_date is triggered_at's date. The type of that value and its time zone are UNKNOWN (G-02).
- FACT (CODE COMMENT, schema): breakout_date is "YYYY-MM-DD IST; the session whose daily close cleared LEVEL." (`core/schema.sql:1566`). INTERPRETATION (DI-04): lines 136 to 178 compare no close price with LEVEL; whether any helper checks that the breakout day's close cleared LEVEL was not read (G-03). In the lines read, the breakout date is the alert's date.

### 7.3 Settled-only guard (143 to 151)

- FACT (CODE COMMENT), 143 to 145: "SETTLED-ONLY: a breakout on a FORMING daily candle (intraday firing) is not yet a settled EOD result -> skip (fail-safe). Settled iff the breakout session has closed: an earlier date, or today at/after market close."
- FACT (code, 146 to 151): if the clock value is not None, the breakout date is today and _market_closed(now) is false, the symbol is skipped with the info log "pb01 capture %s: breakout_date %s is a FORMING session (now %s < close) -> SKIP premature (await settled EOD)".
- UNKNOWN (G-03): what _market_closed treats as the close, and how it handles holidays.
- INTERPRETATION (DI-03): when the clock value is None, the guard of line 146 is not applied; _safe_now's body was not read (G-03), so whether it can return None is itself UNKNOWN.
- In the lines read, there is no maximum age for a breakout date: an earlier date passes the guard (NEW DETAIL recorded by RC-09; FACT for lines 136 to 178).
- This time rule is not a signal-expiry check (RC-15).

### 7.4 LEVEL (153 to 160)

- FACT (CODE COMMENT), 153: "Compute LEVEL from OUR OWN settled daily candles (never the payload price)."
- FACT (code): the worker fetches daily candles for the symbol through self._fetch (154) and computes level = compute_breakout_level(daily, breakout_date, self._cfg.level_lookback_sessions) (155). If level is None the symbol is skipped with the warning "pb01 capture %s: < %d prior daily sessions before %s -> skip" (157 to 159).
- FACT (CONFIG): level_lookback_sessions: 20, "LEVEL = highest daily HIGH of the N sessions BEFORE the breakout day" (`config/system_config.yaml:623`). That self._cfg is this watchlist block, passed by main.py as config=_watchlist_cfg (`main.py:3735`), is INTERPRETATION (DI-09 (c)): the worker's constructor was not read (G-03).
- FACT (CODE COMMENT, schema): "retest LEVEL = highest daily HIGH of the 20 sessions BEFORE the breakout day (WE compute it; never the payload)." (`core/schema.sql:1564-1565`).
- FACT (TEST SOURCE, names only): test_level_is_max_high_of_20_before_breakout, test_level_ignores_breakout_day_and_later, test_level_none_when_insufficient_history (`tests/unit/test_v3_watchlist_capture.py:39`, `tests/unit/test_v3_watchlist_capture.py:46`, `tests/unit/test_v3_watchlist_capture.py:53`).
- Precision (reconciliation D; drafting prompt): LEVEL is computed from the worker's own daily candles, not the payload price; level_lookback_sessions is 20. The definition "highest daily HIGH of the 20 sessions BEFORE the breakout day" is what the comments state; compute_breakout_level's body was not read, so its exact computation is UNKNOWN (G-03). Post-insert code (179 to 245) was not read either.
- The payload price: trigger_prices is required but unused in lines 751 to 804, and in lines 136 to 178 LEVEL comes only from compute_breakout_level (FACT for those lines).
- The fetch window: UNKNOWN; see section 11.4 (G-06).

### 7.5 trading_date (162 to 165)

- FACT (CODE COMMENT), 162 to 163: "trading_date = the SINGLE valid next trading day (holiday-aware) = the anti-rehydration stamp. next_trading_day takes a datetime at/after the breakout EOD."
- FACT (code): anchor = self._breakout_eod_dt(breakout_date) (164); trading_date = self._mw.next_trading_day(anchor).isoformat() (165).
- FACT (CODE COMMENT, schema): trading_date is "YYYY-MM-DD IST; the SINGLE valid session (D+1). ANTI-REHYDRATION key: load discards trading_date != today." (`core/schema.sql:1562-1563`).
- UNKNOWN (G-03): _breakout_eod_dt's time of day; next_trading_day's calendar and holiday source; whether self._mw is the market_windows object that main.py passes (`main.py:3736`), since the constructor (lines 1 to 85) was not read (that it is: INTERPRETATION, DI-09 (b)).
- Precision (drafting prompt): the row is stamped for the next trading day, not for the alert's date (RC-12).

### 7.6 The S&R zone and the row (167 to 178)

- FACT: sr_zone_json = self._nearest_zone_json(symbol, level) (168), "Record the nearest 30m/1h S&R zone to LEVEL (validation programme; fail-safe)." (167, FACT (CODE COMMENT)). Its body: UNKNOWN (G-03).
- FACT: the row (172 to 177) holds symbol, trading_date, level, breakout_date (ISO text), source = scanner_name, sr_zone_json, status "PENDING", outcome_json None, and captured_at = created_at = the same clock text now_iso (170 to 171).
- INTERPRETATION (DI-03): if no clock value is available, now_iso is the empty string (171).
- FACT: inserted = self._store.insert_pb01_watchlist(row) (178). What follows (179 to 245), including what is done with inserted: UNKNOWN (G-03).

### 7.7 Uninspected helpers (each called in the lines read; none of their bodies was read)

| Helper or object | Called at | What the lines read show | Not established | Gap |
|---|---|---|---|---|
| _parse_eod_triggered_at | `signals/webhook_receiver.py:767` | takes the stripped text of triggered_at; None leads to 400 | accepted formats, time zone, the returned type | G-02 |
| self._safe_now | `v3_chain/watchlist_capture.py:137`, `v3_chain/watchlist_capture.py:170` | its value is the current time, checked against None (138, 146, 170 to 171) | its clock (main.py passes now_fn=time_authority.now_ist, `main.py:3737`); whether it can return None | G-03 |
| self._market_closed | `v3_chain/watchlist_capture.py:146` | used as the test that the session has closed (the comment at 144 to 145) | the close time; holidays | G-03 |
| self._fetch and _TF_DAY | `v3_chain/watchlist_capture.py:154` | returns a mapping from which the daily candles are taken (an empty list when absent) | the candle source, the window, _TF_DAY's value | G-03, G-06 |
| compute_breakout_level | `v3_chain/watchlist_capture.py:155` | takes the candles, the breakout date and the session count; its None leads to a skip (156) | the computation; where it is defined (imports, lines 1 to 85) | G-03 |
| self._breakout_eod_dt | `v3_chain/watchlist_capture.py:164` | gives the anchor passed to next_trading_day | the time of day used | G-03 |
| self._mw.next_trading_day | `v3_chain/watchlist_capture.py:165` | its result, in ISO form, is the trading_date | the calendar and holiday source | G-03 |
| self._nearest_zone_json | `v3_chain/watchlist_capture.py:168` | its result is stored as sr_zone_json (175) | the zone selection; failure handling | G-03 |
| self._safe_log | `v3_chain/watchlist_capture.py:92` and others | called with a level name and a format string | where the log goes | G-03 |
| self._q | `v3_chain/watchlist_capture.py:97`, `v3_chain/watchlist_capture.py:111`, `v3_chain/watchlist_capture.py:124`, `v3_chain/watchlist_capture.py:134` | put_nowait, get with a timeout, task_done | its type and size | G-03 |
| insert_pb01_watchlist | `v3_chain/watchlist_capture.py:178`; `core/state_store.py:3078`, `core/state_store.py:3088` | INSERT OR IGNORE; returns a bool | the bool's meaning; error handling | G-04 |
| get_pb01_watchlist_for_date | `v3_chain/pb01_entry.py:151`; `core/state_store.py:3094`, `core/state_store.py:3101` | selects by trading_date | how pending_only is applied | G-04 |
| record_heartbeat (utils.cron_heartbeat) | `signals/webhook_receiver.py:825-829` | called with the name "pb01_capture", the statuses and the message | its storage (HISTORICAL: C-002 11, B-007 11) and its return value | DU-03 |
| OhlcFetcher and _sr_fetch_fn | `main.py:3728-3732` | the shared fetcher is built with lookback_days and cache_ttl_sec 0.0 | the candle source and how lookback_days is applied | G-01, DU-04 |

## 8. In-memory queue, asynchronous worker and failure handling

### 8.1 Threads (FACT for the lines read)

| Thread | Started at | Role |
|---|---|---|
| the request thread (the source's term; the server itself not read) | - | runs _process_request and _handle_eod; calls submit only |
| "pb01-capture", daemon | `v3_chain/watchlist_capture.py:90` | runs _run and _capture |
| "pb01-entry", daemon | `v3_chain/pb01_entry.py:121` | the entry stage; its pickup is section 12 |

The comments state the split: submit is "fast, guarded" (105); it "Never raises into the request thread" (109); the worker "does the heavy LEVEL fetch/compute OFF the request thread (WR1)" (`signals/webhook_receiver.py:754-755`).

### 8.2 The queue

- FACT: one queue, self._q: submit puts (scanner_name, symbol, triggered_at) with put_nowait (111); stop puts a sentinel (97); _run takes items with get(timeout=1.0) (124) and calls task_done for each (134).
- UNKNOWN (G-03): the queue's type and maximum size, set outside the lines read. The code handles queue.Full at 98 and 113.
- FACT: the queue is the only storage between the request and the row in the lines read; nothing in lines 86 to 178 writes a queued item anywhere else.

### 8.3 Submit (105 to 118)

```
v3_chain/watchlist_capture.py (read 004)
106:     def submit(self, *, scanner_name: str, symbol: str, triggered_at) -> bool:
110:         try:
111:             self._q.put_nowait((scanner_name, symbol, triggered_at))
112:             return True
113:         except queue.Full:
114:             self._safe_log("warning", "pb01 capture queue full, dropping %s (fail-safe miss)", symbol)
115:             return False
116:         except Exception as exc:
117:             self._safe_log("error", "pb01 capture submit error (ignored): %s", exc)
118:             return False
```

| Case | Result | Log | Counted in the answer's "captured" |
|---|---|---|---|
| put_nowait succeeds | True | none in submit | yes |
| queue full | False | warning, "fail-safe miss" | no |
| another exception inside submit | False | error, "ignored" | no |
| an exception from the call that submit does not catch | - | the handler logs "EOD capture submit failed for %s: %s" (`signals/webhook_receiver.py:798`) | no |

The docstring (107 to 109): "Enqueue one EOD breakout symbol for capture. Returns False (logged) if the queue is full - a missed capture is fail-SAFE (an empty watchlist = no PB-01 the next day), never a bad trade. Never raises into the request thread." (FACT (CODE COMMENT)).

Precision (reconciliation A): a True from submit means queued in memory, not captured to the database.

### 8.4 The worker loop (121 to 134)

- FACT: while the stop event is not set, take an item with a 1-second timeout (an empty queue: continue); skip the sentinel; call _capture(*item); an exception is logged as "pb01 capture error (ignored): %s" with the comment "one broken symbol must NEVER kill the worker" (131 to 132); task_done in a finally block (133 to 134).
- INTERPRETATION (DI-09 (f)): the one worker thread processes one item at a time; the 1-second timeout lets the loop see the stop event while the queue is empty. The order in which items are taken depends on the queue's type (G-03).

### 8.5 Capture outcomes

| Outcome | Lines | Log | Row |
|---|---|---|---|
| no breakout date | 139 to 141 | warning "no breakout_date -> skip" | none |
| same-day breakout before the close | 146 to 151 | info "FORMING session ... SKIP premature" | none |
| too few prior sessions (LEVEL None) | 156 to 160 | warning "< %d prior daily sessions before %s -> skip" | none |
| an exception in _capture | 131 to 132 | error "pb01 capture error (ignored)" | none (INTERPRETATION: unless raised after the insert) |
| the insert is made | 178 | not in the lines read | inserted, or ignored for an existing (symbol, trading_date) (section 10.4); what follows: UNKNOWN (G-03) |

### 8.6 Stop and restart

- FACT: stop sets the event, puts the sentinel and joins for at most 3 seconds (94 to 103). Whether main.py calls stop at shutdown was not read (G-01; HISTORICAL: B-006 3, lines 1720 to 1729).
- FACT (CODE COMMENT): the entry stage is "RESTART-SAFE by RE-DERIVATION - the pb01_watchlist ROW is the durable state" (`v3_chain/pb01_entry.py:12`), and main.py starts it with the comment "the row status IS the durable state (restart-safe)" (`main.py:3749`).
- INTERPRETATION (DI-02): durability begins at the row. An item still in the in-memory queue when the process stops is not written anywhere by the lines read; once stop has set the event, the loop ends at its next check of the event (95, 122), so items still queued then are not taken. Such an item has no row, while its alert was already answered 200 and counted as queued.

### 8.7 Failure handling summary

The design rule stated in the comments is a "fail-safe miss": a lost capture means no PB-01 candidate the next day, "never a bad trade" (`signals/webhook_receiver.py:776-777`; `v3_chain/watchlist_capture.py:107-109`; FACT (CODE COMMENT)). Section 15.1 lists each failure with its source-established handling.

## 9. Heartbeat, logging and observability semantics

### 9.1 The pb01_capture heartbeat (TR-07)

```
signals/webhook_receiver.py (read 002)
806:     def _record_eod_heartbeat(self, scanner_name: str, symbols: int, queued: int,
807:                               status: str = "SUCCESS",
808:                               functional_status: Optional[str] = None) -> None:
824:         try:
825:             from utils.cron_heartbeat import record_heartbeat
826:             record_heartbeat(
827:                 "pb01_capture", status=status, functional_status=functional_status,
828:                 message=f"scanner={scanner_name} symbols={symbols} queued={queued}",
829:             )
830:         except Exception as exc:   # never let observability break the request path
```

- FACT: two call sites, 786 to 787 (no worker: status FAILED, functional status DISABLED, queued 0) and 801 to 803 (the normal path: status SUCCESS by default, functional status EMPTY_NO_DATA when nothing was queued, else None). Every exception is swallowed, with the warning "pb01_capture heartbeat failed (ignored): %s" (830 to 834).
- FACT (CODE COMMENT), the docstring (809 to 822), in part: "one cron_heartbeat row per EOD alert, so the 17:00 capture stops being invisible." and

> RECORDS \*QUEUED\*, NOT CAPTURED. submit() hands the symbol to the capture worker, which does the LEVEL fetch/compute OFF the request thread (WR1). So queued=N proves the alert ARRIVED, authenticated, parsed, and was accepted - it does NOT prove any row reached pb01_watchlist. The watchlist row count remains the only proof the worker finished. The message says queued= and not captured= precisely so nobody later reads this as capture confirmation.

> THIS IS THE SIGNAL INGRESS. Every failure mode is swallowed: record_heartbeat already returns False rather than raising, and this adds a belt-and-braces except so that no DB hiccup, no import error and no bad argument can turn a 200 into a 500. Observability must never be able to break the thing it observes.

- Storage: the heartbeat table through utils/cron_heartbeat.py, which was not read (HISTORICAL: C-002 11, B-007 11).
- Precision (reconciliation B; drafting prompt): heartbeat is attempted only after handler validation reaches a heartbeat call site. Invalid requests returning 400 do not get a heartbeat. The no-worker path records FAILED/DISABLED and still returns 200. A request answered 404 for an unknown key, or stopped before line 551, never reaches the handler. "Attempted": the write itself may fail and is then swallowed (INTERPRETATION from 819 to 834, as reconciliation B words it).

### 9.2 Heartbeat matrix (FACT for the lines read)

| Path | Heartbeat | status | functional_status | message | HTTP |
|---|---|---|---|---|---|
| a check V1 to V5 fails | none | - | - | - | 400 |
| no worker bound | attempted (786) | FAILED | DISABLED | scanner=K symbols=N queued=0 | 200, accepted 0, "watchlist disabled" |
| worker bound, at least one symbol queued | attempted (801) | SUCCESS | none | scanner=K symbols=N queued=Q | 200, accepted N, captured Q |
| worker bound, nothing queued (no symbols, or no submit returned True) | attempted (801) | SUCCESS | EMPTY_NO_DATA | scanner=K symbols=N queued=0 | 200, accepted N, captured 0 |

K is the scanner key, N the number of symbols after the split (774), Q the number of True answers from submit. In the no-worker path the heartbeat carries symbols=N while the answer's accepted is 0 (786, 788). What later readers make of these statuses is not established by C-011.

### 9.3 Log lines on the route (FACT; format strings as printed, arrows written "->")

| Where | Level | Text | When |
|---|---|---|---|
| `signals/webhook_receiver.py:779-781` | warning | "EOD alert %s: watchlist disabled (no capture worker) -> fail-safe miss for %d symbol(s)" | no worker bound |
| `signals/webhook_receiver.py:798` | error | "EOD capture submit failed for %s: %s" | an exception from submit |
| `signals/webhook_receiver.py:799-800` | info | "EOD alert %s: %d symbol(s) -> %d queued for capture" | the normal path |
| `signals/webhook_receiver.py:832` | warning | "pb01_capture heartbeat failed (ignored): %s" | the heartbeat raised |
| `v3_chain/watchlist_capture.py:92` | info | "pb01 watchlist capture worker started" | start |
| `v3_chain/watchlist_capture.py:114` | warning | "pb01 capture queue full, dropping %s (fail-safe miss)" | queue full |
| `v3_chain/watchlist_capture.py:117` | error | "pb01 capture submit error (ignored): %s" | another submit error |
| `v3_chain/watchlist_capture.py:132` | error | "pb01 capture error (ignored): %s" | an exception in _capture |
| `v3_chain/watchlist_capture.py:140` | warning | "pb01 capture %s: no breakout_date -> skip" | no breakout date |
| `v3_chain/watchlist_capture.py:148-149` | info | "pb01 capture %s: breakout_date %s is a FORMING session (now %s < close) -> SKIP premature (await settled EOD)" | same-day, before the close |
| `v3_chain/watchlist_capture.py:158` | warning | "pb01 capture %s: < %d prior daily sessions before %s -> skip" | LEVEL None |
| `v3_chain/pb01_entry.py:123` | info | "pb01 entry stage started (window %s-%s, poll %.0fs)" | the entry stage starts |
| `main.py:3750-3751` | info | "V3 Step 10b PB-01 watchlist: ENABLED - capture + entry stage started (SHADOW / ANALYSIS-ONLY, NO order path)" | wiring done |
| `main.py:3753` | error | "pb01 watchlist wiring failed (continuing without it): %s" | wiring failed |

- main.py passes logger=get_logger("pb01_watchlist") to the worker, the would-be recorder and the entry stage (`main.py:3736`, `main.py:3741`, `main.py:3746`), and to the fetcher (`main.py:3731`). The receiver's own logger (self._log) and where any of these logs are written: not read (DU-03).
- No log line for a successful or ignored insert appears in the lines read (179 to 245 not read, G-03).

### 9.4 What each signal proves

| Signal | Shows | Does not show |
|---|---|---|
| HTTP 200 with accepted and captured | the handler passed its checks; captured = symbols queued in memory | that any row was written |
| heartbeat pb01_capture, SUCCESS, queued=Q | in the docstring's words, the alert "ARRIVED, authenticated, parsed, and was accepted" | "any row reached pb01_watchlist" (docstring) |
| heartbeat pb01_capture, FAILED / DISABLED | the handler ran with no worker bound | - |
| the worker's skip and error logs | that a capture was skipped or failed | - |
| a pb01_watchlist row | in the docstring's words, "the only proof the worker finished" | that the entry stage acted on it |
| expected_managers acted_means | the configuration's own rule: "a pb01_watchlist ROW written - rows, never queued=" (`config/expected_managers.yaml:135`) | who applies the rule (G-07) |

### 9.5 Precision recorded against C-002 10.7 (RC-06)

C-002 10.7 (HISTORICAL) speaks of "the cron_heartbeat row per EOD alert (S5 and S6 for the EOD route)". C-011 records the precision of section 9.1: a heartbeat is attempted per EOD alert that passes the handler's checks and reaches a call site; the 400 answers (760 to 773) write none. Per ChatGPT's reconciliation (B and section 4) this is recorded in C-011 only; no C-002 erratum is proposed. B-007 11 (the receiver's "pb01_capture" row per EOD alert, 801 to 832) reads the same way (RC-20).

## 10. pb01_watchlist schema and duplicate key

### 10.1 Definition (FACT (SCHEMA), reads 005 and 006)

```
core/schema.sql (reads 005 and 006)
1540: -- TABLE 49: pb01_watchlist   (V3 Step 10b - PB-01 overnight watchlist, schema v43)
1559: CREATE TABLE IF NOT EXISTS pb01_watchlist (
1569:     status        TEXT NOT NULL DEFAULT 'PENDING'
1570:                   CHECK (status IN ('PENDING','CONSUMED','EXPIRED_WINDOW','INVALIDATED','SKIPPED_GAP')),
1575:     UNIQUE (symbol, trading_date)
1576: );
1578: CREATE INDEX IF NOT EXISTS idx_pb01_watchlist_trading_date
1579:     ON pb01_watchlist(trading_date);
```

### 10.2 Columns and what the capture writes

| Column | Type and constraint (1560 to 1574) | Schema comment (FACT (CODE COMMENT)) | Written by the capture (read 004) |
|---|---|---|---|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | - | not in the row (assigned by the table) |
| symbol | TEXT NOT NULL | - | the symbol after the alias map (173; 792) |
| trading_date | TEXT NOT NULL | "YYYY-MM-DD IST; the SINGLE valid session (D+1)." and "ANTI-REHYDRATION key: load discards trading_date != today." | next_trading_day(...).isoformat() (165, 173) |
| level | REAL NOT NULL | "retest LEVEL = highest daily HIGH of the 20 sessions BEFORE the breakout day (WE compute it; never the payload)." | compute_breakout_level(...) (155, 173) |
| breakout_date | TEXT NOT NULL | "YYYY-MM-DD IST; the session whose daily close cleared LEVEL." | breakout_date.isoformat() (174) |
| source | TEXT NOT NULL | "scanner name (pb01_breakout_retest)." | scanner_name (174) |
| sr_zone_json | TEXT | "nearest 30m/1h sr_detector zone to LEVEL (S&R validation programme)." | _nearest_zone_json(...) (168, 175) |
| status | TEXT NOT NULL DEFAULT 'PENDING', CHECK of five values | - | "PENDING" (175) |
| outcome_json | TEXT | "entry-stage outcome detail (confirmation candle / gap / expiry ts)." | None (175) |
| captured_at | TEXT NOT NULL | "ISO-8601 IST at capture (EOD)." | now_iso (176) |
| created_at | TEXT NOT NULL | "ISO-8601 IST row-insert." | now_iso, the same value (176) |
| consumed_at | TEXT | "ISO-8601 IST when CONSUMED/EXPIRED/INVALIDATED/SKIPPED." | not in the row; set by the update (`core/state_store.py:3117`) |

### 10.3 Status values

- FACT (SCHEMA): status is one of PENDING, CONSUMED, EXPIRED_WINDOW, INVALIDATED and SKIPPED_GAP (1570), default PENDING (1569).
- FACT: the capture writes PENDING (175); the entry stage later calls update_pb01_watchlist_status (`v3_chain/pb01_entry.py:326`, arguments not printed), whose statement sets status, outcome_json and consumed_at (`core/state_store.py:3117`).
- When each later status is set is the entry stage's logic, beyond the boundary (OQ-02). The configuration's comment for gap_guard_pct, "SKIPPED_GAP if open > LEVEL x (1 + this)" (`config/system_config.yaml:629`, FACT (CONFIG); the multiplication sign written "x"), names one of them; it is not traced here.

### 10.4 Duplicate key (BK-08)

- FACT: the table's key is UNIQUE (symbol, trading_date) (`core/schema.sql:1575`), and the store inserts with INSERT OR IGNORE (`core/state_store.py:3088`). A second row with the same symbol and trading_date is therefore ignored by the insert, and the first row stays (as the evidence sheet records, TR-12 and TR-13).
- FACT: trading_date is the next trading day after the breakout (165), so the key is the symbol with its next trading day (D+1), not with the alert's date (RC-12).
- FACT (TEST SOURCE, names only): test_store_dedupe_unique_symbol_trading_date (`tests/unit/test_v3_watchlist_capture.py:72`) and test_capture_dedupes_repeat_firing (`tests/unit/test_v3_watchlist_capture.py:146`); not run (G-08).
- Precision (reconciliation G; drafting prompt): the row is stamped for the next trading day; the schema uses UNIQUE (symbol, trading_date) and the insert uses INSERT OR IGNORE. This establishes a row-key duplicate guard. It is not proof that every duplicate scenario was handled or tested, or that every ignored or failed insert is observable. In particular:
  - INTERPRETATION (DI-08): a repeated alert is not removed before the worker: each queued item is captured again (its LEVEL computed again) up to the insert, which the key then ignores (106 to 118, 136 to 178, 3088); what follows the insert is UNKNOWN (G-03).
  - INTERPRETATION (DI-07): source is not part of the key, so two eod-typed keys capturing the same symbol for the same trading date would leave one row; at the pin only one eod-typed map entry exists (HISTORICAL: C-001 4.1).
  - INTERPRETATION (DI-06): symbol spellings that the alias map does not unify are different values of the key column; whether such variants ever arrived is NOT ESTABLISHED.
  - The same symbol captured for different trading dates gives different rows; the entry stage reads only today's (section 12).
  - Whether an ignored insert is reported (the bool) or logged: UNKNOWN (G-03, G-04).
- None of the intake guards (expiry, dedup cache, fingerprint) is on this route (section 6.3). BK-08 status: RECORD EVIDENCE / KEEP OPEN (reconciliation, section 4): the wider historical duplicate question is not closed.

### 10.5 Writers and readers in the evidence

- Writer on the route: the capture worker through insert_pb01_watchlist (178; 3078, 3088). Status writer: update_pb01_watchlist_status (3109, 3117), called by the entry stage (326).
- Readers: the entry stage's pickup (151; 3094, 3101); the soak report's read-only SELECT (`scripts/v3_shadow_soak_report.py:293`); the end-to-end test (`tests/unit/test_v3_pb01_e2e.py:90`).
- Not established (evidence sheet, section 5): other callers of submit or other writers of pb01_watchlist. In runtime files the term occurs in 8 files; every SQL line printed is in core/state_store.py (INSERT, SELECT, UPDATE) or the soak report (SELECT); six T2 lines were not printed (main.py 3, v3_chain/watchlist_capture.py 3); "submit(" was not searched.

### 10.6 Schema version and index

- FACT: "TABLE 49: pb01_watchlist" with "schema v43" (1540); core/migrations.py names "v43 pb01_watchlist (Step 10b PB-01 overnight watchlist)," (`core/migrations.py:89`); whether a migration step creates or alters the table: UNKNOWN (G-04). Line 1590, a later comment, says "same pattern as v43 pb01_watchlist / v38 retest_state" (FACT (CODE COMMENT)); what it introduces was not read.
- FACT: an index on trading_date, idx_pb01_watchlist_trading_date (1578 to 1579), the column the pickup selects on (3101).

## 11. Startup wiring and configuration

### 11.1 The watchlist block (`config/system_config.yaml:619-630`, read 009; FACT (CONFIG), its comments included)

| Line | Key | Value | Comment (transliterated) | Consumer in the lines read |
|---|---|---|---|---|
| 619 | watchlist | (block) | "V3 Step 10b - PB-01 overnight watchlist + next-morning entry stage (default-OFF; byte-identical while off)" | - |
| 620 | enabled | true | "master gate (paper+live): false = capture worker + entry-stage monitor NOT constructed -> an EOD webhook is captured to nothing (BYTE-IDENTICAL). ANALYSIS ONLY - no position/capital/CNC/GTT; PB-01 stays enabled:false (fail-closed) regardless" | main.py's _watchlist_on (derivation not read, G-01) |
| 621 | playbook_scanner | "pb01_breakout_retest" | "the eod scanner this watchlist owns (-> the pb01_breakout_retest strategy)" | none (HISTORICAL: no reader, C-001 4.4 and D-04) |
| 622 | (comment) | - | "capture (EOD, day D) - ALL SEEDS" | - |
| 623 | level_lookback_sessions | 20 | "LEVEL = highest daily HIGH of the N sessions BEFORE the breakout day" | the capture (155, 158 to 159) |
| 624 | capture_fetch_lookback_days | 60 | "how far back to fetch daily candles to compute LEVEL (calendar days)" | none in the lines read (G-06) |
| 625 | (comment) | - | "next-morning entry stage (day D+1) - ALL SEEDS (calibrate: when do retests confirm?)" | - |
| 626 | entry_start | "09:20" | the first 5-minute candle must close before any confirmation (the comment cites section 7 of a spec) | the entry stage's start log (124) |
| 627 | entry_end | "11:00" | "next-morning retest thesis expired by mid-morning" | the entry stage's start log (125) |
| 628 | entry_tf | "5minute" | "retest timeframe (built via candle_math.resample)" | the entry stage (beyond the boundary) |
| 629 | gap_guard_pct | 0.03 | "SKIPPED_GAP if open > LEVEL x (1 + this)" | the entry stage (beyond the boundary) |
| 630 | poll_interval_sec | 20.0 | "entry-stage candle poll cadence (off the hot path)" | the entry stage (its poll is logged at 125; INTERPRETATION, DI-09 (d)) |

- The block's first comment says "default-OFF"; this file sets enabled to true (FACT (CONFIG)). INTERPRETATION (DI-09 (e)): "default-OFF" describes the value used when the key is absent, which was not read.
- Paper and live: no branch in the lines read; the comment at 620 says the gate applies to both (FACT (CONFIG), its comment).
- The entry window values agree with B-008 3 and A-008 16 (HISTORICAL; RC-17, RC-18).

### 11.2 Wiring in main.py (`main.py:3712-3755`, read 007; FACT for the lines)

| Lines | Step |
|---|---|
| 3712-3713 | pb01_capture_worker and pb01_entry_stage start as None |
| 3714-3715 | only if _watchlist_on, inside a try |
| 3716-3721 | imports, three class names masked by the reader (`<ALNUM>`); WatchlistCaptureWorker from v3_chain.watchlist_capture |
| 3722 | _wl_v3_cfg = _v3_cfg if it is not None, else a new instance of a masked class (by the variable's name, a V3 configuration) |
| 3723-3724 | S&R zone knobs and scoring parameters from _sr_cfg |
| 3725-3732 | one OhlcFetcher over the shared fetch function and instrument cache: lookback_days=int(getattr(_wl_v3_cfg, "fetch_lookback_days", 180)), logger "pb01_watchlist", now_fn=time_authority.now_ist, cache_ttl_sec=0.0 |
| 3733 | effect_telemetry.register_constructed("ohlc_fetchers") |
| 3734-3737 | WatchlistCaptureWorker(config=_watchlist_cfg, store=store, fetcher=_wl_fetcher, market_windows=market_windows, logger=..., now_fn=time_authority.now_ist, zone_knobs=..., zone_scoring=...) |
| 3738-3742 | _pb01_would_be, a would-be recorder by its name (class name masked), with regime_runner=market_regime_runner ("as-of regime snapshot (None when OFF)") |
| 3743-3746 | pb01_entry_stage (class name masked) with store, fetcher, market_windows and on_confirm=_pb01_would_be.record |
| 3747 | webhook_receiver.set_eod_capture(pb01_capture_worker), "EOD alerts now captured" |
| 3748-3749 | start the worker; start the entry stage ("the row status IS the durable state (restart-safe)") |
| 3750-3751 | info log "... ENABLED ... (SHADOW / ANALYSIS-ONLY, NO order path)" |
| 3752-3755 | any exception: error log "pb01 watchlist wiring failed (continuing without it): %s"; both names set back to None |

- The comment at 3706, cut by the grep: "eod_capture=None (a fail-safe miss) + no entry-stage daemon -> BYTE-IDENTICAL. When" (`main.py:3706`, FACT (CODE COMMENT)).
- Inputs defined before line 3712 were not read: _watchlist_on, _watchlist_cfg, _v3_cfg, _sr_cfg, _sr_fetch_fn, instrument_cache, store, market_windows, market_regime_runner, time_authority, effect_telemetry and webhook_receiver (G-01).
- INTERPRETATION (evidence sheet, TR-03): a failure after the worker is built but before line 3747 (for example in the entry stage's construction) leaves the receiver unbound, because the binding comes after all three constructions (3734, 3738, 3743).
- INTERPRETATION (DI-01): a failure at lines 3748 to 3751, after the binding, sets main.py's two names back to None (3754 to 3755), but the lines read contain no call that removes the binding, so the receiver would keep the worker object; whether that worker's thread was running would depend on where the failure occurred.

### 11.3 Enabled versus wired (precision F)

Config says enabled: true, but historical runtime state is UNKNOWN. Wiring depends on _watchlist_on, whose derivation and the receiver's constructor call were not read (G-01). C-001 7.5 and T-10 (HISTORICAL) state that the route is wired when watchlist.enabled is true; the comment at 620 agrees (RC-02). Whether the host's configuration, at any date, matched this file is NOT ESTABLISHED (G-11).

### 11.4 The fetch lookback (G-06, OPEN QUESTION)

capture_fetch_lookback_days: 60 is configured (`config/system_config.yaml:624`), while main.py constructs the shared fetcher with the V3 configuration's fetch_lookback_days, default 180 (`main.py:3730`). No reader of capture_fetch_lookback_days appears in the lines read, and _fetch's body was not read. Whether the 60-day setting is consumed is UNKNOWN / NOT ESTABLISHED; it is not a proven defect (reconciliation E). G-03 (the worker's lines 1 to 85 and 179 to 245) would settle it.

### 11.5 Expected managers (TR-15)

config/expected_managers.yaml lists both components (section 5.8). Its acted_means for the capture worker, "a pb01_watchlist ROW written - rows, never queued=", agrees with the heartbeat's docstring (section 9.1). Its effect_point for the entry stage carries the stale pointer of section 14.1. Who reads the file: UNKNOWN (G-07).

### 11.6 Shutdown

The worker's stop is in section 8.6. HISTORICAL (B-006 3): stops at shutdown at main.py lines 1720 to 1729, not re-read (G-01).

## 12. PB-01 pickup boundary

### 12.1 The entry stage's start (FACT, `v3_chain/pb01_entry.py:117-125`, read 011)

start does nothing if a thread exists; otherwise it clears the stop event, starts a daemon thread named "pb01-entry" running _run, and logs "pb01 entry stage started (window %s-%s, poll %.0fs)" with entry_start, entry_end and the poll interval. main.py starts it at `main.py:3749`, after the capture worker (3748).

### 12.2 The pickup (FACT, read 012)

```
v3_chain/pb01_entry.py (read 012)
151:             rows = self._store.get_pb01_watchlist_for_date(today, pending_only=True)
```

- The entry stage reads the rows whose trading_date is today, asking for pending rows only (by the method's name and the SELECT at 3101, which lies between its def line and the next; INTERPRETATION, DI-09 (h)). The store's select is "SELECT * FROM pb01_watchlist WHERE trading_date = ?" (`core/state_store.py:3101`); how pending_only is applied: UNKNOWN (G-04).
- How the entry stage derives today, and how often line 151 runs: not printed (DU-01; the poll interval of 20.0 seconds is configured, `config/system_config.yaml:630`).
- The docstring lines, cut by the grep (FACT (CODE COMMENT)): it "monitors the symbols captured overnight (the pb01_watchlist) for the" (`v3_chain/pb01_entry.py:5`); "RESTART-SAFE by RE-DERIVATION - the pb01_watchlist ROW is the durable state" (`v3_chain/pb01_entry.py:12`); "(get_pb01_watchlist_for_date(today)); a row stamped for another trading_date can" (`v3_chain/pb01_entry.py:18`). The store argument: "StateStore (get/update pb01_watchlist)" (`v3_chain/pb01_entry.py:84`).
- With the schema's comment, "load discards trading_date != today" (`core/schema.sql:1563`): a row stamped D+1 is read on D+1 only (INTERPRETATION, evidence sheet section 5: the entry stage reads rows of the current day only).

### 12.3 What the row carries to the boundary

symbol, trading_date (D+1), level, breakout_date (D), source (the scanner key), sr_zone_json and status PENDING (section 10.2). The restart behaviour is re-derivation from the row: consistent with A-011 14 (HISTORICAL; RC-22).

### 12.4 Where C-011 stops

C-011 ends at line 151: the read of today's pending rows. The next consumer is the entry stage's own logic, excluded by OQ-02 (G-05). For orientation only, without analysis: main.py gives the entry stage on_confirm=_pb01_would_be.record (`main.py:3745`), logs the wiring as "SHADOW / ANALYSIS-ONLY, NO order path" (3751), and the configuration says "ANALYSIS ONLY - no position/capital/CNC/GTT" (620). The entry stage later updates the row's status (`v3_chain/pb01_entry.py:326`; arguments not printed). Consistent with A-008 16 (HISTORICAL: capture to a watchlist, analysis only, would-be records only; RC-18). Whether any order or trade ever followed is outside C-011 and NOT ESTABLISHED.

## 13. Reconciliation against the frozen C-001 to C-010 manuals

### 13.1 The 22 checks of the evidence sheet (section 6), with the Stage 1 reconciliation's outcome

The checks cover C-001 to C-009 (this chapter cites C-010 only for one count; see section 5.2) and six section A and B manuals (A-002, A-008, A-011, B-006, B-007 and B-008). Frozen statements are HISTORICAL and were not re-read; nothing in a frozen manual is corrected here.

| ID | Frozen statement (manual, section) | C-011 evidence | Result | After the reconciliation |
|---|---|---|---|---|
| RC-01 | C-001 7.1 step 5: an "eod" key leaves before the kill, backpressure and window gates (556 to 564) | TR-05: the return at 564 | CONSISTENT (the gates after 564 not re-read) | unchanged |
| RC-02 | C-001 7.5 and T-10: _handle_eod parses, aliases, submits (key, symbol, triggered_at) or records a fail-safe miss; rows with source = the key; wired when watchlist.enabled is true | TR-06; TR-11 (source = scanner_name); TR-03 | CONSISTENT; the code's condition _watchlist_on not read (G-01); the comment at 620 agrees | precision F applies (section 11.3) |
| RC-03 | C-001 4.4 and D-04: watchlist.playbook_scanner has no reader; the binding is made by scanner_type | TR-05 (563); TR-02 (621) | CONSISTENT (no reader search repeated) | unchanged |
| RC-04 | C-001 8, A-L5 and D-06: an EOD alert answers 200 with accepted = the symbol count; no signals row | TR-06 (788, 804); no signals SQL in 751 to 834 | CONSISTENT | unchanged |
| RC-05 | C-002 9 row 7: the route's 400s and 200s, "watchlist disabled" without a worker | TR-06 | CONSISTENT | unchanged |
| RC-06 | C-002 10.7: "the cron_heartbeat row per EOD alert (S5 and S6 for the EOD route)" | TR-06, TR-07: a heartbeat only after the body checks pass (786, 801); the 400 answers write none | PRECISION NOTE | recorded in C-011 only; no C-002 erratum (section 9.5) |
| RC-07 | C-002 11: submit per symbol; one "pb01_capture" row recording queued; a fail-safe miss without a worker | TR-06, TR-07 | CONSISTENT; detail: the fail-safe branch also writes a row (FAILED, DISABLED) | unchanged |
| RC-08 | C-002 6.1: the worker bound late by set_eod_capture (main.py 3747) | TR-03, TR-04 | CONSISTENT | unchanged |
| RC-09 | C-003 6.5 and L-01: the route checks no expiry, dedup cache or fingerprint; the worker's handling not read | TR-06; TR-09 to TR-13 | CONSISTENT; NEW DETAIL: no age check in the worker; a same-day pre-close breakout skipped; a repeat for the same next trading day ignored by the key | no change to C-003 |
| RC-10 | C-004 9.1: the constructor takes eod_capture | TR-04 (161) | CONSISTENT (the module docstring not re-read) | unchanged |
| RC-11 | C-004 T-18: own checks; only stocks type-checked; alias applied; no price check | TR-06 (757 to 774, 792) | CONSISTENT; detail: trigger_prices required present, value unused | unchanged |
| RC-12 | C-005 10.2, BM-06 = BK-08: the route bypasses the three guards; its table's own unique symbol/date key | TR-12, TR-13 | CONSISTENT; precision: the date in the key is the next trading day (D+1) | BK-08: RECORD EVIDENCE / KEEP OPEN |
| RC-13 | C-007 11.2, BM-12 = BK-30: whether the EOD route reaches _process_signal (not traced) | TR-05 (return at 564); TR-06, TR-07: no _process_signal call, no signals SQL; the parser at 767 not read | NEW EVIDENCE for BK-30 | BK-30: RECORD EVIDENCE / KEEP OPEN |
| RC-14 | C-008 9.4, EP-09: no direct hand-off from the EOD route to the processor | TR-05, TR-06 | CONSISTENT | unchanged |
| RC-15 | C-009 14 (with C-003 L-01): the EOD route has no expiry check | TR-06 | CONSISTENT; the settled-only skip is a different time rule | unchanged |
| RC-16 | B-006 3, TH9 and TH10, and 11: threads pb01-capture and pb01-entry, guards at 87 and 118, the error caught at 131, watchlist.enabled (620), poll 20 s (630), the row as durable state (3749) | TR-03, TR-08, TR-10, TR-14, TR-02 | CONSISTENT | unchanged |
| RC-17 | B-008 3: the entry stage's window 09:20 to 11:00 (configuration 619 to 627) | TR-02 (626 to 627) | CONSISTENT | unchanged |
| RC-18 | A-008 16: capture to a watchlist (ANALYSIS ONLY); next morning 09:20 to 11:00 on 5-minute candles; would-be records only | TR-02 (620, 626 to 628); TR-03 (on_confirm = the would-be recorder; the log text "NO order path") | CONSISTENT | unchanged |
| RC-19 | A-002 12: EOD scanner signals routed to a watchlist path, never the intraday queue | TR-05; TR-06 | CONSISTENT | unchanged |
| RC-20 | B-007 11: the receiver's "pb01_capture" row per EOD alert (801 to 832) | TR-07 | CONSISTENT (the precision of RC-06 applies) | unchanged |
| RC-21 | C-001 11 and C-002 16: test_v3_eod_route.py's tests at lines 39 to 119 and 59 to 113 | TR-18 (names at 59 to 113) | CONSISTENT | unchanged |
| RC-22 | A-011 14: the PB-01 stage rehydrated at start when enabled | TR-14 (12, 151) | CONSISTENT (by re-derivation from the row) | unchanged |

### 13.2 Outcome

No frozen statement conflicts with the C-011 evidence. One precision note (RC-06) is recorded in C-011 only. RC-09 adds detail for the part C-003 left unread, without changing C-003; RC-12 and RC-13 record evidence for BK-08 and BK-30, which stay OPEN. ChatGPT's reconciliation also settled the evidence sheet's questions: Q-A (draft from this evidence; no further pass now; its decision line), Q-B (RC-06 in C-011 only) and Q-C (BK-08 and BK-30 kept OPEN).

## 14. Known defect and open questions

### 14.1 KNOWN DEFECT (minor): a stale line pointer in config/expected_managers.yaml

- FACT (CONFIG): the entry stage's effect_point is "v3_chain/pb01_entry.py:318 _terminate (update_pb01_watchlist_status :322)" (`config/expected_managers.yaml:140`).
- FACT: at the pin the call update_pb01_watchlist_status( is at line 326 of v3_chain/pb01_entry.py (`v3_chain/pb01_entry.py:326`); the T2 grep printed all six pb01_watchlist lines of that file, and 326 is the only one with that call.
- Classification: KNOWN DEFECT (minor), as ChatGPT's reconciliation records it (section 4). Operational impact: UNKNOWN, because the file's reader was not established (G-07). The ":318 _terminate" part was not checked: line 318 was not printed.
- Nothing is repaired; this is a record of the legacy file.

### 14.2 Bookmarks and governance items (none closed here)

| Item | What it asks | C-011 evidence | Status |
|---|---|---|---|
| BK-08 (C-005 BM-06) | the EOD route's duplicate handling | UNIQUE (symbol, trading_date) with trading_date = D+1; INSERT OR IGNORE; no intake guard on the route (sections 6.3, 10.4) | OPEN: RECORD EVIDENCE / KEEP OPEN (reconciliation, section 4; as Stage 0's OQ-05 (a) recommended) |
| BK-30 (C-007 BM-12) | whether the EOD route reaches _process_signal | the return at 564; no _process_signal call, signals SQL or intraday enqueue in 751 to 834; the parser at 767 not read; indirect and system-wide paths not traced (section 5.3) | OPEN: RECORD EVIDENCE / KEEP OPEN (reconciliation, section 4; as Stage 0's OQ-05 (a) recommended) |
| RC-06 precision | the heartbeat per EOD alert (C-002 10.7) | section 9.5 | recorded in C-011 only; no C-002 erratum |
| VP-14 | a V2 proposal whose ratification point is C-011 (E3, sections 6 and 7) | none needed: it is a V2 decision | NOT RATIFIED. The drafting prompt excludes V2 decisions, and the reconciliation implies none; Stage 0's OQ-04 recommended not ratifying it at C-011; ChatGPT's Stage 0 decision on OQ-04 was not received by Claude |
| Q-44 (a) | whether the PB-01 workers ran on the production host, and from when | none: outside the repository (G-11) | a DR-016 audit lead (topic K-009), as Stage 0's OQ-07 (a) recommended; unchanged |

### 14.3 OPEN QUESTION G-06

Who, if anyone, consumes watchlist.capture_fetch_lookback_days (60)? Section 11.4: UNKNOWN; not a proven defect.

### 14.4 Evidence gaps (preserved in the evidence sheet's words, its internal cross-references dropped; no inference fills them)

- G-01 main.py: the lines before 3712 that set _watchlist_on and _watchlist_cfg; the receiver's constructor call; the term lines not printed, among them the stops at shutdown (HISTORICAL: B-006 3, lines 1720 to 1729).
- G-02 signals/webhook_receiver.py: _parse_eod_triggered_at (called at 767); lines 1157 to 1159 of set_eod_capture; the term lines not printed.
- G-03 v3_chain/watchlist_capture.py: lines 1 to 85 (constructor, queue size, imports) and 179 to 245 (what follows the insert; the helpers), which hold the term lines not printed.
- G-04 core/state_store.py: the three methods' bodies (3078 to about 3125); core/migrations.py: whether a step creates or alters the table.
- G-05 the entry stage beyond the pickup: excluded by OQ-02; listed only so that the boundary is visible.
- G-06 OPEN QUESTION: watchlist.capture_fetch_lookback_days (60, `config/system_config.yaml:624`): no reader in the lines read; the shared fetcher's lookback comes from the V3 configuration's fetch_lookback_days, default 180 (`main.py:3730`), and _fetch's body was not read. Whether the 60-day setting has any effect is UNKNOWN (G-03 would settle it).
- G-07 config/expected_managers.yaml: who reads it and whether the stale effect_point matters.
- G-08 the tests' bodies (what each asserts beyond its name).
- G-09 files left at their counts: the 24 files of the evidence sheet's section 3 and the T7 and T8 files outside the route.
- G-10 R33's headings (OQ-06 of Stage 0): not authorised; not read. R42 never read.
- G-11 outside the repository: what ran on the production host and which rows existed (Q-44; a DR-016 audit lead).
- G-12 config/system_config.yaml: the pb01_watchlist line and the other watchlist line outside 619 to 630.

Lines that hold a term in the files read but were not printed (counted by S1-1; evidence sheet, section 7), for example: v3_chain/watchlist_capture.py T2 3, T7 2, T8 1; main.py T2 3, T5 10, T8 4; tests/unit/test_v3_watchlist_capture.py T2 16. They remain gaps.

### 14.5 Unknowns noted in drafting (DU-01 to DU-05; not new evidence)

- DU-01: how the entry stage derives today for the pickup, and how often line 151 runs (lines of v3_chain/pb01_entry.py not printed).
- DU-02: whether the web framework turns an unguarded exception in the handler into an answer, and which (not read).
- DU-03: utils/cron_heartbeat.py (record_heartbeat's storage and return value; HISTORICAL storage only) and the receiver's logger and log destinations (not read).
- DU-04: sr_detector/fetch.py (OhlcFetcher) and the shared fetch function: the source of the daily candles and how lookback_days is applied (not read; bears on G-06).
- DU-05: core/schema.sql lines 1541 to 1558, between the table's header line (1540) and its CREATE statement (1559): not printed.

The companion file lists each with the gap it sits beside.

## 15. Source-established failure behaviour versus historical host behaviour NOT ESTABLISHED

### 15.1 Source-established behaviour (what the pinned source does; not observed at runtime)

Lines: "receiver" is signals/webhook_receiver.py, "worker" v3_chain/watchlist_capture.py, "schema" core/schema.sql, "store" core/state_store.py.

| # | Condition | Behaviour in the source | Lines | Label |
|---|---|---|---|---|
| F01 | the key is not in the map | 404 "Unknown scanner"; the handler is not reached | receiver 553-554 | FACT |
| F02 | a handler check fails | 400 with one of five texts; no heartbeat; nothing queued | receiver 757-773 | FACT |
| F03 | no worker bound | warning log; heartbeat FAILED / DISABLED; 200 {"accepted": 0, "captured": 0, "detail": "watchlist disabled"}; nothing queued | receiver 778-788 | FACT |
| F04 | the queue is full | submit returns False with a warning; the symbol is not counted in captured; the alert is still answered 200 | worker 113-115; receiver 794-804 | FACT |
| F05 | submit raises | the handler logs an error and goes on with the next symbol | receiver 797-798 | FACT |
| F06 | the heartbeat write fails | the exception is swallowed with a warning; the HTTP answer is unchanged | receiver 830-834 | FACT |
| F07 | the capture finds no breakout date | warning; skip; no row | worker 139-141 | FACT |
| F08 | a same-day breakout before the close | info log; skip; no row | worker 146-151 | FACT |
| F09 | too few prior sessions | warning; skip; no row | worker 156-160 | FACT |
| F10 | an exception in the capture | error log; the worker goes on with the next item | worker 131-132 | FACT |
| F11 | a repeat for the same (symbol, trading_date) | the insert is ignored (INSERT OR IGNORE against the UNIQUE key); whether it is reported or logged UNKNOWN | schema 1575; store 3088 | FACT; reporting UNKNOWN (G-03, G-04) |
| F12 | F07 to F11 happen on the worker thread, after the answer | the HTTP answer and the heartbeat had already reported the symbol as queued | receiver 801-804 | INTERPRETATION (reconciliation A) |
| F13 | the wiring fails before the binding | error log; both names None; the receiver stays unbound, so later alerts take F03 | main.py 3752-3755 | INTERPRETATION (evidence sheet, TR-03) |
| F14 | the wiring fails after the binding | both names None; no un-binding in the lines read (section 11.2) | main.py 3747-3755 | INTERPRETATION (DI-01) |
| F15 | the process stops with items still queued | no row is written for them by the lines read; durability begins at the row | worker 94-103, 121-134 | INTERPRETATION (DI-02) |
| F16 | the clock gives no value | the settled-only guard is not applied; captured_at and created_at are empty text | worker 146, 170-171 | INTERPRETATION (DI-03) |

The comments' rule for these cases is the "fail-safe miss" (section 8.7): a lost capture means no candidate the next day, never a bad trade. That rule is FACT (CODE COMMENT) as stated by the source; C-011 did not test it.

### 15.2 Historical host behaviour: NOT ESTABLISHED

- Whether _watchlist_on was true on the production host, from when, and whether the two threads ran (G-01, G-11; Q-44 (a), a DR-016 audit lead).
- Which EOD alerts arrived, and what answers they received. HISTORICAL records cited by the evidence sheet: 5 PB-01 POSTs, 27 to 31-Jul (C-001 4.5). What those requests returned, and whether any heartbeat or watchlist row followed: NOT ESTABLISHED.
- Whether any pb01_watchlist row, heartbeat row or log line existed on the host: NOT ESTABLISHED (no database or log was opened; G-11).
- Whether the entry stage picked up, confirmed or updated any row, and whether any order or trade followed: outside C-011 and NOT ESTABLISHED.
- Whether the host's configuration and code matched the pin at any date: NOT ESTABLISHED.
- The time of day of the alerts: the source's comments speak of "the 17:00 alert" (`signals/webhook_receiver.py:783`) and "the 17:00 capture" (`signals/webhook_receiver.py:809`) (FACT (CODE COMMENT)); actual sending times: NOT ESTABLISHED.

## 16. End-to-end walkthrough (illustrative; symbols only, no market values)

Symbols: K, the eod-typed scanner key (HISTORICAL: pb01_breakout_retest, C-001 4.1); D, the trading day of the breakout; N, the next trading day after D; X and Y, two stock symbols; L(X) and L(Y), their LEVEL values; Z(X) and Z(Y), their zone texts; T, a time on day D after the close. No market value is given or implied. Nothing below was run; each step cites the lines that would act.

| Step | What happens | Lines | Label |
|---|---|---|---|
| 1 | An alert for key K arrives after the close of D. Its body holds stocks "X,Y," (the trailing comma for illustration), a trigger_prices field and a triggered_at value for T in a form the parser accepts (G-02). | - | illustration |
| 2 | The receiver finds K in the map, reads scanner_type "eod" and returns the handler's answer; nothing after line 564 runs. | `signals/webhook_receiver.py:552-564` | FACT |
| 3 | The handler parses the body; all five checks pass; symbols = [X, Y] (the empty part after the trailing comma is dropped). | `signals/webhook_receiver.py:757-774` | FACT |
| 4 | A worker is bound. For each symbol the alias map is consulted (a symbol not in it is kept as it is), and submit queues (K, symbol, triggered_at); both return True, so captured = 2. | `signals/webhook_receiver.py:778`, `signals/webhook_receiver.py:790-796`; `v3_chain/watchlist_capture.py:111-112` | FACT |
| 5 | Info log, as formatted for this illustration, "EOD alert K: 2 symbol(s) -> 2 queued for capture"; heartbeat pb01_capture, SUCCESS, no functional status, message (formatted) "scanner=K symbols=2 queued=2"; answer 200 {"accepted": 2, "captured": 2}. No row exists yet: the answer proves queuing, not capture. | `signals/webhook_receiver.py:799-804`, `signals/webhook_receiver.py:826-828` | FACT |
| 6 | The pb01-capture thread takes (K, X, T). breakout_date = D. The clock is after the close of D, so the settled-only guard passes. | `v3_chain/watchlist_capture.py:124-130`, `v3_chain/watchlist_capture.py:137-146` | FACT; the close time UNKNOWN (G-03) |
| 7 | The worker fetches X's daily candles and computes L(X) = compute_breakout_level(candles, D, 20): by the comments, the highest daily high of the 20 sessions before D; the implementation was not read. L(X) is not None. | `v3_chain/watchlist_capture.py:154-156`; `config/system_config.yaml:623` | FACT for the call; the computation UNKNOWN (G-03) |
| 8 | anchor = the breakout's EOD time; trading_date = next_trading_day(anchor) = N (by the comment, holiday-aware; not read). Z(X) = the nearest zone text. | `v3_chain/watchlist_capture.py:164-168` | FACT for the calls; the calendar UNKNOWN (G-03) |
| 9 | The row {symbol X, trading_date N, level L(X), breakout_date D, source K, sr_zone_json Z(X), status PENDING, outcome_json None, captured_at = created_at = the capture time} is inserted with INSERT OR IGNORE. The same happens for Y with L(Y) and Z(Y). | `v3_chain/watchlist_capture.py:172-178`; `core/state_store.py:3088` | FACT; what follows the insert UNKNOWN (G-03) |
| 10 | Variant, a repeat: a second alert for X on D is answered 200 with X counted as captured (queued); the worker captures again; the insert for (X, N) is ignored, and the first row stays. | `core/schema.sql:1575`; `core/state_store.py:3088` | FACT for the key; DI-08 for the re-capture |
| 11 | Variant, before the close: if the worker ran before the close of D, the capture of X would be skipped with the "FORMING session" log and no row, while the alert was already answered 200. | `v3_chain/watchlist_capture.py:146-151` | FACT; F12 INTERPRETATION |
| 12 | Variant, no worker bound: the alert is answered 200 {"accepted": 0, "captured": 0, "detail": "watchlist disabled"} with a FAILED / DISABLED heartbeat; nothing is queued. | `signals/webhook_receiver.py:778-788` | FACT |
| 13 | On day N, the pb01-entry thread reads get_pb01_watchlist_for_date(today, pending_only=True). With today = N, the rows (X, N) and (Y, N) are returned while PENDING. | `v3_chain/pb01_entry.py:151`; `core/state_store.py:3101` | FACT for the call; today's derivation DU-01; the pending filter G-04 |
| 14 | BOUNDARY: what the entry stage does with the rows is outside C-011 (OQ-02). | - | - |

## 17. Source and line-reference index

Every line below was printed by a Stage 1 read at the pin (FACT). "Reads" are the evidence sheet's read numbers.

| File (pinned blob; lines in the file) | Lines printed (reads) | What they show | Sections |
|---|---|---|---|
| config/scan_webhook_map.yaml (54dd98d75f276f9b82027707702df2ee619b11f7; 85) | 84 (010) | scanner_type eod | 3, 4.1, 5.2 |
| signals/webhook_receiver.py (0f15a0ab033529d80d9c14c993a5077690b72871; 1,208) | 551-564 (001) | the 404 for an unknown key; the routing comment; the type branch and its return | 3, 4.1, 5.3, 15.1, 16 |
| same | 751-804 (002) | _handle_eod: docstring, checks, no-worker branch, alias, submit, log, heartbeat calls, answer | 3, 4.1, 5.4, 6, 9, 15.1, 16 |
| same | 806-835 (002) | _record_eod_heartbeat: signature, docstring, call, swallow | 9 |
| same | 161, 195, 754, 778, 783, 794, 1156, 1160 (003) | every eod_capture line: constructor parameter, attribute, set_eod_capture | 4.2, 5.7 |
| v3_chain/watchlist_capture.py (25a1f084eadabc443a0dd22d8171b26584d4e897; 245) | 86-103 (004) | start and stop | 5.5, 8.6 |
| same | 105-134 (004) | submit; _run | 8.2 to 8.4 |
| same | 136-178 (004) | _capture: breakout date, settled-only guard, LEVEL, trading_date, zone, row, insert | 7, 8.5, 10.2, 16 |
| core/state_store.py (11866117801b9329773a1be605448faef3c96f8d; 3,356) | 3078, 3088, 3094, 3101, 3109, 3117 (015) | insert (INSERT OR IGNORE), select by trading_date, status update | 5.6, 10, 12.2 |
| core/schema.sql (b46d7489f6ec5e31149dd68957f3b9e45270571e; 1,655) | 1540, 1559, 1578, 1579, 1590 (005); 1559-1578 (006) | the table header, definition, key, index; a later comment that names v43 pb01_watchlist | 10 |
| main.py (445fb0d8237a6d535ed00e5eb968c8176efd74f3; 4,294) | 3712-3755 (007) | the wiring | 4.2, 11.2, 15.1 |
| same | 3706, 3747 (008) | the two eod_capture lines | 5.7, 11.2 |
| config/system_config.yaml (bf742ae5c81c60614370361998e6a102a1064ff7; 754) | 619-630 (009) | the watchlist block | 7.4, 11.1, 11.4 |
| v3_chain/pb01_entry.py (cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8; 372) | 117-125 (011) | the entry stage's start | 12.1 |
| same | 5, 12, 18, 84, 151, 326 (012) | docstring fragments; the store argument; the pickup; the status update call | 12.2, 14.1 |
| config/expected_managers.yaml (ff4d65d5f9b5a5ddfcc8cac7b812c770da555a3c; 387) | 130, 131, 135, 136, 137, 140 (016) | the two manager entries; acted_means; the stale effect_point | 5.8, 11.5, 14.1 |
| scripts/v3_shadow_soak_report.py (f4e59d87274c35831649c01f6be61b1c23ec78bd; 427) | 280, 281, 293, 297, 308, 321, 323 (017) | the read-only load of rows | 5.8, 10.5 |
| core/migrations.py (cc39e811e48a9eec823c20b70d343278a6b33eef; 445) | 89 (018) | "v43 pb01_watchlist" | 5.8, 10.6 |
| ops_dashboard/backend/services/system_logs.py (773006336224f33ac8b3a7edc0e4e54969284c0e; 633) | 79 (019) | the name in a tuple of logger names | 5.8 |
| tests/unit/test_v3_eod_route.py (2d0dc40716187a3d7649fa8bc1ef598aec13d23d; 130) | 59, 72, 82, 93, 113 (013) | five test names | 5.8 |
| tests/unit/test_v3_watchlist_capture.py (8b50580570a92305ed5a7a6c45e726faee801768; 165) | 39, 46, 53, 72, 81, 91, 124, 135, 146 (014) | nine test names | 5.8, 7.4, 10.4 |
| tests/unit/test_v3_pb01_e2e.py (e0c81020c1614982400b8d62166b8333151d7819; 118) | 4, 20, 22, 85, 90, 103 (020) | the end-to-end test's docstring fragment, imports, worker, read-back and status assertion | 5.8, 10.5 |

The evidence sheet's Appendix B holds every printed line verbatim, with the reader's mask counts.

## 18. Glossary and cross-references

### 18.1 Glossary

| Term | Meaning in this chapter |
|---|---|
| EOD / Daily route | the one route of an eod-typed scanner key through the receiver to the PB-01 watchlist (section 1.3) |
| eod-typed key | a scan_webhook_map entry with scanner_type: eod (map line 84) |
| scanner_type | the map attribute the receiver compares with "eod"; absent means "intraday" (563) |
| _handle_eod | the receiver's EOD handler (751 to 804) |
| capture worker | WatchlistCaptureWorker in v3_chain/watchlist_capture.py, with the thread "pb01-capture" |
| submit | the worker's enqueue call; True means queued in memory |
| queued | an item placed on the worker's in-memory queue; not a row |
| captured (response field) | the number of symbols submit queued (804); not the number of rows |
| fail-safe miss | the source's term for a lost capture: no candidate next day, never a bad trade |
| breakout_date (D) | the alert's date, taken from triggered_at (138) |
| settled-only | the guard that skips a same-day breakout before the market close (143 to 151) |
| LEVEL | the value computed by compute_breakout_level from the worker's own daily candles; by the comments, the highest daily high of the 20 sessions before D |
| trading_date (D+1) | the next trading day after the breakout, stamped on the row (165) |
| anti-rehydration | the source's term for the trading_date stamp: rows of another date are not loaded (1563) |
| pb01_watchlist | the table of captured candidates (TABLE 49, schema v43) |
| PENDING | the status a captured row is written with |
| INSERT OR IGNORE | the store's insert; with UNIQUE (symbol, trading_date) a duplicate row is not inserted |
| pb01_capture heartbeat | the cron_heartbeat record the handler attempts after its checks (section 9) |
| functional status | the heartbeat's second status: DISABLED or EMPTY_NO_DATA on this route |
| entry stage | the PB-01 next-morning stage, thread "pb01-entry"; class name masked |
| pickup | the entry stage's read of today's pending rows (151); C-011's boundary |
| would-be recorder | _pb01_would_be in main.py, given to the entry stage as on_confirm (by its name) |
| _watchlist_on | main.py's condition for the wiring; its derivation was not read (G-01) |
| S1-1, S1-2 | Stage 1's count-only searches and bounded reads |
| W1, W2 | the two lock windows of Stage 1 |
| TR-, RC-, G- | the evidence sheet's trace steps, reconciliation checks and gaps |
| DI-, DU- | interpretations and unknowns first noted in this draft (companion file) |
| `<ALNUM>` | the reader's mask for an alphanumeric shape (here, three class names) |

### 18.2 Cross-references

- Frozen V2 manuals cited (HISTORICAL; as cited by the evidence sheet): A-002 12; A-008 16; A-009 8; A-011 14; B-006 3, TH9, TH10 and 11; B-007 11; B-008 3; C-001 4.1, 4.4, D-04, 4.5, 7.1, 7.5, T-10, 8, A-L5, D-06 and 11; C-002 6.1, 9, 10.7, 11, 16, 17 and T5; C-003 6.5, L-01 and 7.3; C-004 9.1 and T-18; C-005 10.2 and BM-06; C-006 4.1; C-007 11.2 and BM-12; C-008 9.4, EP-09 and Q28; C-009 14; the C-010 S1-1 count Q08.
- Governance: BK-08 and BK-30 (OPEN); VP-14 (NOT RATIFIED); Q-44 (a) (a DR-016 audit lead, topic K-009).
- C-011 records: E1 to E8 of section 2.5; the companion file C-011_DRAFT_EVIDENCE_AND_GAP_CHECK.txt.
- Neighbouring topics: the intraday path from the receiver onward, C-002 to C-010; the PB-01 entry stage's decisions and outcomes, outside C-011 (OQ-02).
