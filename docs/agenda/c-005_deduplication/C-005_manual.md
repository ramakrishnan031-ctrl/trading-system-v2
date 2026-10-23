# C-005 - DEDUPLICATION AND IN-FLIGHT HANDLING: LEGACY MANUAL

Status: FROZEN 2026-10-23 (revision 1, DR-058). Revision 0 is ChatGPT's draft of 23-Oct-2026 (docs/inputs/manuals/C-005_DEDUPLICATION_AND_IN_FLIGHT_HANDLING_DRAFT_23-OCT-2026.md, stored unchanged), written with its reconciliation (docs/inputs/reconciliation/C-005_RECONCILIATION_DRAFT_23-OCT-2026.md) from Claude's Stage 1 research; revision 1 applies Claude's review corrections (C-005_review_notes.md section 2; summary in section 18). Frozen under W-4 (DR-017): the freeze takes effect when Rama declares the package final and runs PART 2 of its push commands.  
Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 23-Oct-2026 through the masking filter.  
Built from: C-005_CLAUDE_REPOSITORY_ANSWERS_23-OCT-2026.txt ("the answers", sha256 B9E66FDC0A42F13FB6EDD53334F3BEA5C8FD6146AABE6E2CC983FBF7A4ACC155) and C-005_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt ("the inventory", sha256 055D676940D164204D378FE99EAC7F28E4A6BE412703478F799C4399D7235CE6), with the Stage 0 inventory C-005_CLAUDE_STAGE0_INVENTORY_ANSWERS_23-OCT-2026.txt (sha256 8D1D351D2259107C5A7ECD702F259397BEC54D758B9C75DE84F5F6B5A2FA9A63), all stored in docs/inputs/chatgpt_originals_and_replies/.  
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1); V2 considerations are in C-005_v2_design_inputs.md (PROPOSED / OPEN).  
Security: no credential, token, account or client identifier, host or address, mailbox, SSH detail, or person name other than Rama's is reproduced. Stock symbols named here are public NSE ticker names used in code and test examples.

> This chapter records the pinned legacy implementation. It does not repair, tune, revive, or redesign the legacy system, and it does not select V2 behaviour.

## 1. Purpose and boundary

C-005 documents how the legacy webhook intake recognises repeat intraday signals and handles signals already being processed. It covers the receiver's in-flight claim, in-memory deduplication cache, database fingerprint uniqueness, claim/cache lifecycle, concurrency, restart/shutdown, the replay tool's relationship to these guards, and the relevant source-level tests.

It does **not** document the full queue implementation (C-006), the full database schema/retention contract (C-007), processor behaviour beyond claim lifecycle (C-008), expiry policy (C-009), scanner-to-strategy mapping (C-010), the EOD route's complete behaviour (C-011), authentication (C-003), or downstream same-symbol/risk rules (A-005/A-008). Those topics are interfaces and cross-references, not scope to reanalyse here.

## 2. Evidence standard and source inventory

### 2.1 Source baseline

| Alias | Path | Pinned blob ID | Lines | Use in this chapter |
|---|---|---|---:|---|
| R | `signals/webhook_receiver.py` | `0f15a0ab033529d80d9c14c993a5077690b72871` | 1,208 | Intake, guards, claim/cache lifecycle, sweeper |
| P | `signals/signal_processor.py` | `7fab0a02a302770ac72bbbd662549d5c091086c1` | 2,704 | Release, re-queue and allocator hand-off |
| M | `main.py` | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | 4,294 | Construction/wiring, server threads, shutdown |
| SQL | `core/schema.sql` | `b46d7489f6ec5e31149dd68957f3b9e45270571e` | 1,655 | `signals` columns, uniqueness/index |
| CFG | `config/system_config.yaml` | `bf742ae5c81c60614370361998e6a102a1064ff7` | 754 | Repository dedup window and allocator mode |
| ST | `core/state_store.py` | `11866117801b9329773a1be605448faef3c96f8d` | 3,356 | Database transaction wrapper |
| TWR | `tests/unit/test_webhook_receiver.py` | `7b377abb85d644f0f3ff7956385224b43f31ed61` | 2,043 | Documentary assertions for receiver outcomes |
| TSP | `tests/unit/test_signal_processor.py` | `3ec0ea84392a193d18b992ca88df0da5e6235f67` | 2,501 | Documentary assertions for release/re-queue |

Other files were searched/read selectively as recorded in `C-005_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt`. Tests and source code were not executed. Every source read was logged against its pinned blob through the masking filter (46 reads, every one matching the pinned blob); the protected-path check was zero before every read and at delivery. No host record or live service was accessed.

### 2.2 Evidence labels

- **FACT (SOURCE-VERIFIED):** directly supported by a pinned source range.
- **FACT (TEST TEXT):** an assertion appears in test source; it was not executed and is not proof of runtime behaviour.
- **FACT (CONFIG):** repository configuration only; not proof of the deployed host value.
- **INTERPRETATION:** a possible consequence of the source path, not an observed event.
- **HISTORICAL:** information retained from a frozen manual/record, not independently proven by the source read.
- **KNOWN DEFECT:** a previously established defect re-verified at the pinned source.
- **UNKNOWN / NOT ESTABLISHED:** evidence reviewed does not establish the fact.
- **OPEN QUESTION / BOOKMARK:** a cross-topic issue deliberately not resolved here.

## 3. Architecture and end-to-end intake path

### 3.1 High-level flow

```text
HTTP /webhook/<key>
        |
        v
_handle_webhook / request handling
        |
        +--> EOD-type key? ------ yes ------> _handle_eod (C-011; bypasses these guards)
        |
        no
        v
Per-symbol handling
  alias resolution -> exclusion list -> _process_signal
        |
        v
Validity / expiry checks
        |
        v
Guard 1: in-flight claim keyed by symbol
        | IN_PROCESS if already claimed
        v
Guard 2: in-memory TTL cache keyed by (symbol, scanner)
        | DUPLICATE on cache hit
        v
Set cache entry, then database INSERT
        |
        +--> INSERT succeeds -> queue signal -> ACCEPTED
        |
        +--> unique-index IntegrityError
        |       |
        |       +--> prior QUEUE_FULL row -> try to re-queue it
        |       +--> other row / no row -> DUPLICATE
        |
        +--> other storage error -> rollback / STORE_ERROR
        |
        v
Processor handles queued item and releases claim
(except while re-queued or handed to allocator enforcement)
```

**FACT (SOURCE-VERIFIED):** `signals/webhook_receiver.py` lines 477-534, 564, 696-728, 701, 709-723, 892-1072. The EOD branch is outside the three intake guards; see C-011 bookmark.

### 3.2 Three guards are separate mechanisms

| Guard | In-memory/database structure | Key | Main outcome | Lifetime |
|---|---|---|---|---|
| In-flight claim | `self._in_flight` dictionary | Symbol string | `IN_PROCESS` | Until release or sweeper eviction |
| Cache dedup | `self._dedup_cache`, a `cachetools.TTLCache` | `(symbol, scanner_name)` | `DUPLICATE` | Configured TTL, subject to cache-library semantics |
| Database fingerprint | `signals.fingerprint`, `fingerprint_date`, unique index `idx_signals_fingerprint_today` | Fingerprint plus receipt date | `DUPLICATE`, or re-queue of an earlier `QUEUE_FULL` row | Database row lifetime, subject to retention |

**FACT (SOURCE-VERIFIED):** R lines 202-208, 229-238, 937-980; SQL lines 74-83; ST lines 526-541.

The claim is keyed only by symbol, so it can temporarily block a different scanner for the same exact symbol string. The cache and fingerprint include scanner identity. The generated `signal_id` and the scanner-to-strategy map are not part of the identified deduplication keys. **FACT (SOURCE-VERIFIED):** R lines 940, 953, 957, 975, 1083-1091.

## 4. Key construction and configuration

### 4.1 Symbol handling

The alias lookup uses `raw_symbol.upper()` as the alias-map lookup key, but when no alias is found the raw symbol is retained. No further case/space normalisation occurs inside `_process_signal` for the keys documented here. Consequently, `reliance` and `RELIANCE` can be distinct claim, cache and fingerprint inputs.

**FACT (SOURCE-VERIFIED):** R lines 701, 940, 953, 1084. This re-verifies C-004 L-04. It describes source behaviour, not a claim that a real signal with a particular casing occurred.

### 4.2 Cache window

| Item | Source | Repository value/logic | Consumer |
|---|---|---|---|
| `webhook.dedup_window_seconds` | CFG line 337 | `300` seconds | Receiver cache TTL and fingerprint bucket |
| Conversion/fallback | R lines 232-235 | Integer conversion; `TypeError`/`ValueError` falls back to 300 | Receiver configuration handling |
| Minimum | R line 236 (used at R lines 237 and 952) | `max(60, value)` | Cache TTL and fingerprint bucket |
| Cache maximum | R line 237 | 10,000 entries | `TTLCache` |
| Claim stale threshold | R lines 208, 1195 | 60 seconds | Sweeper |
| Sweeper interval | R lines 1189-1207 | 60 seconds | Claim cleanup thread |

**FACT (CONFIG):** The value `300` is in the repository copy of `config/system_config.yaml`; the deployed host's value was not checked. The receiver resolves the webhook configuration from `config.webhook` or `config.system.webhook` (R lines 171-175); `main.py` passes `app_config` (M lines 3407-3415).

**UNKNOWN / NOT ESTABLISHED:** `core/config_loader.py`'s declaration, default, type and validation of this key were not read. That is owned by the frozen configuration topic B-002. No conclusion about the host's actual deployed configuration is made.

### 4.3 Fingerprint formula

The receiver constructs the fingerprint as SHA-256 of:

```text
scanner | symbol | epoch_bucket
```

where `epoch_bucket` is the fixed integer bucket derived from `triggered_at`'s epoch time and the configured dedup window. `fingerprint_date` is the IST date of receipt. The unique index is on `(fingerprint, fingerprint_date)`.

**FACT (SOURCE-VERIFIED):** R lines 677-678, 949-980; SQL lines 74-83.

**FACT (TEXT DRIFT):** Schema comments describe the fingerprint as a hash of scanner, symbol and trigger minute / the same minute on the same day. The reviewed code instead uses the configured epoch bucket. The source behaviour takes precedence for describing this pinned code; the stale comments are recorded as a documentation mismatch, not silently corrected in the legacy repository.

### 4.4 Window shapes are not identical

The cache entry is created only when absent, before the database insert. It is not renewed by a cache hit in the reviewed path. Its TTL is anchored to that cache set. The fingerprint uses fixed epoch-aligned buckets from the alert's `triggered_at` plus the receipt date.

The exact TTL expiry semantics depend on the external `cachetools` implementation; the library version was not checked. The statements above distinguish the receiver's own calls from external library semantics.

## 5. Intake-to-outcome matrix

The HTTP status below describes a request containing only the relevant symbol. A batch can produce an overall 503 if any symbol returns `QUEUE_FULL` or `STORE_ERROR`.

| Condition | Outcome | Claim effect | Cache effect | `signals` row effect |
|---|---|---|---|---|
| Missing symbol, bad price, or expired signal | `INVALID_SYMBOL`, `INVALID_PRICE`, `EXPIRED` (HTTP 200) | Not acquired | Not set | No row |
| Symbol already claimed | `IN_PROCESS` (HTTP 200) | Existing claim remains | Untouched | No row for the repeat |
| `(symbol, scanner)` is in cache | `DUPLICATE` (HTTP 200) | Newly acquired claim released | Existing entry kept | No row for the repeat |
| Cache set; insert and queue succeed | `ACCEPTED` (HTTP 200) | Held for processor | Kept for TTL | Row inserted as `QUEUED` |
| Insert succeeds but queue is full | `QUEUE_FULL` (HTTP 503) | Released | Removed to permit sender retry | Row set to `QUEUE_FULL` |
| Unique collision; existing row is `QUEUE_FULL`; re-queue succeeds | `ACCEPTED` (HTTP 200), existing `signal_id` | Held | Kept | Existing row set to `QUEUED`; a failed status flip is only logged |
| Unique collision; existing `QUEUE_FULL` row; queue remains full | `QUEUE_FULL` (HTTP 503) | Released | Removed | Existing row unchanged |
| Unique collision with any other existing row, or no row found | `DUPLICATE` (HTTP 200) | Released | Kept | Existing row only, if any |
| Other insert/storage failure | `STORE_ERROR` (HTTP 503) | Released | Removed | Transaction rolled back |
| Exception inside the `IntegrityError` handler, e.g. the lookup | HTTP 500 for the request | Claim remains held | Cache remains held | No row for the failing symbol; earlier symbols in the same batch may already have been queued |

**FACT (SOURCE-VERIFIED):** R lines 905-926, 934-947, 963-980, 981-1044, 1046-1072; ST lines 506-541; batch result handling R lines 66, 730-734.

### 5.1 What gets recorded

`IN_PROCESS` and cache `DUPLICATE` are response outcomes and do not create a `signals` row. A database uniqueness `DUPLICATE` also leaves only the earlier row. Request-level `webhook_audit` records accepted/rejected counts when the response is HTTP 200; on 503 or 500, the reviewed path records zero and zero. The refusal reason is not stored per symbol by this mechanism. The schema's status check still lists `IN_PROCESS` and `DUPLICATE` as allowed values, although this receiver path never writes them.

**FACT (SOURCE-VERIFIED):** R lines 506-517, 529-534, 934-945, 1014-1015; SQL lines 60-65. This re-verifies C-001 A-L7. Historical pruning of duplicate rows on 16-Jul-2026 is carried from C-001 A-L9 as **HISTORICAL**, not newly proven by this source read.

## 6. Claim lifecycle

### 6.1 Acquisition and release

The claim is acquired atomically under `_in_flight_lock` only if the symbol is absent. Its record contains `acquired_at` and `heartbeat_at` timestamps based on a monotonic clock. A duplicate claim attempt returns `IN_PROCESS`.

The receiver releases its claim on the documented refusal paths after acquisition: cache duplicate, database duplicate, both queue-full paths, and storage error. The processor releases in its `_process_one` `finally` path unless a re-queue or allocator hand-off flag keeps the claim.

**FACT (SOURCE-VERIFIED):** R lines 1074-1091, 944, 1000, 1014, 1031, 1067; P lines 1248-1271.

### 6.2 Re-queue and allocator hand-off

The processor intentionally keeps the claim while the signal is re-queued for retry. The reviewed paths include a rate-limit pre-check re-queue and a `BrokerRateLimitError` re-queue while retries remain. If re-queue fails because the queue is full, or retry attempts are exhausted, the claim is released: explicitly on the rate-limit pre-check path (P lines 456-460), and through `_process_one`'s `finally` on the placement path (P lines 1493-1499, 1526-1532 and 1248-1271). A timeout without re-queue also reaches release.

With allocator enforcement, the claim is held through the allocator admission/rejection hand-off unless the item is re-queued. The repository config says `portfolio_allocator.allocator_mode: "shadow"`, so the enforce hand-off is not taken on the reviewed configuration path.

**FACT (SOURCE-VERIFIED / CONFIG):** P lines 418-461, 1248-1271, 1470-1532, 1699-1703, 1733-1737; CFG lines 564-565. This is not proof of the deployed host mode.

### 6.3 Heartbeat and sweeper

The receiver has an `update_heartbeat` helper, but the count-only search found no runtime caller at the pinned commit. The processor calls its `_heartbeat` checkpoints, but `main.py` constructs it without `in_flight_heartbeat_fn`; those checkpoints are therefore no-ops. The sweeper runs at a 60-second interval and removes claims whose heartbeat is older than 60 seconds, logging at CRITICAL.

**KNOWN DEFECT (RE-VERIFIED):** This is the previously documented B-007 defect: claim heartbeats are not renewed on the inspected runtime path. It is a source-level finding. **UNKNOWN / NOT ESTABLISHED:** whether the sweeper actually evicted a live claim in production.

**INTERPRETATION:** With no heartbeat renewal, a claim still held after it becomes older than 60 seconds can be evicted on a sweeper pass. Depending on timing, the claim can therefore be removed while processing/retry work is still active. The exact occurrence is not established.

**FACT (SOURCE-VERIFIED):** R lines 1110-1120, 1182-1207; P lines 664-673 and heartbeat call sites; M lines 3575-3623.

### 6.4 Restart and shutdown

The claim dictionary is created empty during receiver initialisation; it is not rehydrated from storage. At shutdown, `stop()` makes new requests answer 503 and stops the sweeper; the server thread itself is not stopped (C-002 section 6.4). The claim is therefore process-local, not durable state.

**FACT (SOURCE-VERIFIED):** R lines 206, 1166-1176, 485-490; M lines 1686, 1690. See A-011 for the wider startup/restart contract.

## 7. Cache lifecycle

| Event | Pinned behaviour |
|---|---|
| Creation | Set only if the `(symbol, scanner)` key is absent, before the insert attempt. |
| Cache hit | Returns `DUPLICATE`; the existing entry is retained. |
| Accepted insert/queue | Entry is retained. |
| Database duplicate | Entry is retained. |
| Queue full | Entry is removed so a sender retry is not immediately blocked by the cache. |
| Storage error | Entry is removed. |
| Receiver exception in the `IntegrityError` handler | Entry remains held on the C-004 L-13 path. |
| Expiry | TTL configured from the receiver's effective dedup window; exact library expiry semantics are external. |
| Restart | Cache starts empty. |

**FACT (SOURCE-VERIFIED):** R lines 937-947, 998-1030, 1065-1072, 237. The test source also asserts that after manual claim eviction, a repeat within the cache window is still `DUPLICATE`; tests were not run (TWR lines 1373-1443).

## 8. Fingerprint row lifecycle and transaction boundary

The receiver attempts to insert a `QUEUED` row with the fingerprint. A uniqueness `IntegrityError` triggers a lookup of the existing row. If the existing row is `QUEUE_FULL`, the receiver attempts to re-queue the existing signal; otherwise the request is treated as a duplicate. Other insert failures trigger rollback through the transaction wrapper.

The unique key is durable across process restart because it is in the database. Whether and when the row is pruned is outside C-005 and belongs to C-007. The repository config includes `signal_retention_days: 90` and a comment about retaining terminal "NOISE fingerprints"; the actual pruning behaviour was not traced here.

**FACT (SOURCE-VERIFIED / CONFIG):** R lines 963-1044; SQL lines 74-83; ST lines 506-541; CFG line 482.

**INTERPRETATION:** After restart, the in-memory claim and cache are empty while the fingerprint row remains. A repeat is therefore subject to the persisted unique key only for the same fingerprint bucket and receipt date. A later bucket/date may not collide with the earlier row.

## 9. Concurrency and key asymmetry

`main.py` starts the receiver using Waitress with eight threads. The claim check-and-set and cache check-and-set are protected by their respective locks. The claim key is symbol-only; cache and fingerprint keys are scanner-specific. Thus, while a claim exists, a different scanner's signal for the same exact symbol can receive `IN_PROCESS`; after the claim is released or evicted, that signal may reach the scanner-specific cache/fingerprint checks.

**FACT (SOURCE-VERIFIED):** M lines 3986-3998; R lines 934-945, 1083-1091. The test source asserts that among 50 concurrent claim attempts for one symbol exactly one wins (test text only; not executed).

## 10. Replay, EOD, and paper/live distinctions

### 10.1 Replay tool

`scripts/replay_signals.py` reads stored signal rows with `SELECT` statements and its docstring declares it read-only. The reviewed source/search did not identify insert/update operations or references to the three intake guards. It therefore does not re-submit historical rows through the intake deduplication path. Since intake refusals do not necessarily create a row, they are not available for this stored-row replay tool.

**FACT (SOURCE-VERIFIED) / INTERPRETATION:** `scripts/replay_signals.py` blob `72763c3ed8d5d8fe43258619d55840f2780cfa96`; cited lines 16, 44, 59, 107 and count-only searches in the inventory.

### 10.2 EOD route

The EOD handler bypasses the three intraday guards: the receiver's term search finds no reference to the claim, the cache or the fingerprint in R lines 751-835. The EOD capture table has its own uniqueness constraint on symbol and trading date. The exact EOD route is C-011; its details are not repeated here.

**FACT (SOURCE-VERIFIED):** R lines 564, 751-835; SQL lines 1548-1550, 1602; state-store comment at ST line 3080.

### 10.3 Paper/live

The reviewed receiver path contains no explicit paper/live branch; the inspected processor release logic also has no mode branch in those ranges. A processor comment says the paper path uses the same timeout handler, and the repository config labels allocator mode as "paper+live".

**UNKNOWN / NOT ESTABLISHED:** broader paper/live parity, actual host configuration, and runtime outcomes were not established. No parity should be inferred beyond the specific source ranges cited.

## 11. Tests and documentary evidence

Tests were read but not executed. They assert examples including queue-full retry, storage-error release, fingerprint duplicates, TTL behaviour, same-symbol `IN_PROCESS`, sweeper eviction, concurrent claims, processor release paths, and re-queue handling.

The test source is not uniform with the pinned receiver implementation: some test names/docstrings describe "readmission" after claim eviction while the assertions expect `DUPLICATE` from the still-live cache; other comments describe trigger-minute fingerprinting; crash-test helpers model `IN_PROCESS`/`DUPLICATE` as stored states even though the reviewed receiver path returns these outcomes without inserting such rows.

**FACT (TEST TEXT):** TWR lines 402-462, 524-590, 625-873, 878-936, 1294-1443; concurrency test lines 380-408; TSP lines 832-933, 2144-2167, 2301-2317 and 2324-2340. These assertions are not runtime evidence.

## 12. Known defects, interpretations, and open questions

### 12.1 Known defect: missing heartbeat renewal

The runtime wiring does not provide the processor's in-flight heartbeat callback, so claim heartbeats are not renewed on the reviewed path. The 60-second sweeper can evict a claim whose work may still be active. This is the previously established B-007 known defect, re-verified at the pinned commit.

### 12.2 C5-L1 - ownerless release (interpretation; occurrence not established)

The claim release helper pops by symbol and the claim record stores timestamps, not an owner/generation identifier. If a claim is evicted, a newer claim is acquired for the same symbol, and the earlier worker later releases by symbol, that release could remove the newer claim. This is a possible source-code consequence, not evidence of an incident.

### 12.3 C5-L2 - distinct cache and fingerprint windows

The cache TTL starts from its cache set, while the database fingerprint uses an epoch-aligned `triggered_at` bucket and receipt date. Restart discards the cache and claims but not the database row. This can make the guards disagree about whether two signals are repeats. No V2 decision is made here.

### 12.4 C5-L3 - stale explanatory text

Schema comments, test docstrings, and crash-test tools contain descriptions or models that do not fully match the receiver path read at this pin. Preserve this as a historical documentation/test mismatch.

### 12.5 C5-L4 - failed status flip on re-queue

The receiver logs a failed status update but still returns `ACCEPTED` on the documented re-queue path; the row may remain `QUEUE_FULL` until later processor activity changes its status. This is handed to C-006/C-007; C-005 does not resolve it.

### 12.6 Historical occurrence questions

- Whether a stale claim was ever evicted while its signal remained active: **UNKNOWN / NOT ESTABLISHED** from source; B-007 records this as a records question.
- How often `IN_PROCESS`, cache `DUPLICATE`, and database `DUPLICATE` occurred in the historical live period: **UNKNOWN / NOT ESTABLISHED** from this source review. C-002's approximately 80% expected-duplicate figure remains **HISTORICAL**, not recalculated here.
- Host configuration and actual paper/live deployment parity: **UNKNOWN / NOT ESTABLISHED**.

## 13. Cross-topic handoffs

| Bookmark | Destination | C-005 treatment |
|---|---|---|
| BM-01 - claim lifetime across release, exception, heartbeat and sweeper | B-007, B-009, C-002, C-004 | Reconciled; statements align, with latent defect/lead distinctions retained. |
| BM-02 - re-queue status flip | C-006, C-007 | Record only. |
| BM-03 - EOD cleanup references fingerprint/status | C-007, B-010 | `scripts/eod_cleanup.py` not read; no claim about its exact behaviour. |
| BM-04 - downstream behaviour after claim eviction/release race | C-008, A-008 | Record only; do not infer downstream outcome. |
| BM-05 - processor in-flight count vs receiver claim | A-005, C-008 | Separate mechanisms; not interchangeable. |
| BM-06 - EOD route's own deduplication | C-011 | EOD path bypasses these three guards and uses a separate table key. |
| Configuration loader defaults/type/validation | B-002 | `core/config_loader.py` not read; receiver fallback is documented independently. |
| Q-45 (h), B-007 audit lead: did a second signal for an evicted symbol pass the later guards? | C-005, C-008 (V2_DESIGN_LOG Q-45) | Code path only: after an eviction the cache still refuses the same `(symbol, scanner)` within its window (TWR lines 1373-1443, test text); another scanner, or a repeat after the window, would pass the claim and the cache (section 9). Whether evictions occurred and how long claims were held are records questions; the lead stays OPEN. |

### 13.1 Frozen statements re-verified at the pin

| Frozen statement | Result at the pin |
|---|---|
| C-004 L-04: the symbol's case reaches the claim and dedup keys | Confirmed (R lines 701, 940, 953, 1084). |
| C-004 L-13: claim and cache entry held on one receiver error path | Confirmed (R lines 981-1016, 525-528); on the code path the sweeper would later remove the claim (section 6.3), while the cache entry lasts its window. |
| B-007 sections 11 and 19: heartbeat not renewed; 60-second sweeper | Confirmed (R lines 1110-1120, 1182-1207; M lines 3575-3623; P lines 664-673). |
| B-007 section 20: did the sweeper evict a live claim on production? | Not answerable from source (a records question). |
| B-009 section 5: per-symbol outcomes, the 300 s window ("TTL 237, config 337"), release except re-queue and hand-off | Confirmed: R line 237 and CFG line 337; release as in section 6. |
| B-006 row TH4: the in-flight sweeper thread | Confirmed (R lines 215-220). |
| A-011 section 14: claims and the duplicate cache not rehydrated; the database fingerprint remains | Confirmed (R lines 206, 237; SQL lines 81-83). |
| C-002 sections 6.1, 6.4 and 13.6: release wiring, sweeper stopped at shutdown, IN_PROCESS for a concurrent repeat | Confirmed (M line 3612; R lines 1166-1176; M line 3994 and R lines 928-935). Section 13.4 (about 80% expected duplicates) is a record: HISTORICAL, cited. |
| C-003 section 6.5: repeats after authentication limited by expiry, the dedup cache and the fingerprint | Confirmed (R lines 925-926, 940-947, 952-954, 981). |
| C-001 A-L7 and A-L9: IN_PROCESS and cache DUPLICATE refusals are not signals rows; DUPLICATE rows pruned on 16-Jul-2026 | A-L7 confirmed (R lines 934-945, 506-517); A-L9 is a record: HISTORICAL, cited. |

No frozen manual needs an erratum from C-005.

## 14. Legacy lessons and V2 inputs - no decisions

This section is an evidence handoff, not a design recommendation. The following are observations that a later, separately authorised V2 design review may evaluate: three guard layers with different keys/windows; process-local state lost on restart; heartbeat wiring/sweeper interaction; release by symbol without owner identity; separate cache and fingerprint windows; stale comments/tests; and a re-queue status transition that can fail while the response remains accepted.

**No retain/reject/redesign decision is made in this legacy manual.** Any such decision belongs to the later V2 governance phase.

## 15. End-to-end worked path examples

These examples are source-path illustrations, not historical incidents.

### Example A - first signal accepted

1. Receiver validates symbol/price/expiry.
2. It acquires the symbol claim.
3. No `(symbol, scanner)` cache entry exists, so it sets one.
4. It inserts a `QUEUED` row with the fingerprint and queues the signal.
5. The receiver returns `ACCEPTED`; the processor later handles the item and releases the claim unless it is re-queued or handed off.

### Example B - same symbol already being processed

1. A first signal holds the symbol claim.
2. A second signal for the same exact symbol reaches the claim check.
3. It returns `IN_PROCESS` before cache/database dedup checks.
4. The repeat does not create a `signals` row.

### Example C - claim released but cache still alive

1. A prior signal's claim is released.
2. Another signal with the same `(symbol, scanner)` arrives inside the cache window.
3. The cache answers `DUPLICATE`; the newly acquired claim is released.
4. The repeat does not create a new `signals` row.

### Example D - restart

1. Receiver restarts with empty claim and cache structures.
2. Existing database fingerprint rows remain.
3. A repeated signal colliding with the same fingerprint and receipt date is caught by the database uniqueness guard; a signal in a different bucket may not collide.
4. Exact retention/pruning depends on C-007 and is not established here.

## 16. Source-to-consumer traceability

| Component | Source | Input | Logic | Validation/guard | Output | Storage/handoff | Next consumer |
|---|---|---|---|---|---|---|---|
| Webhook intake | R 477-534 | HTTP request/key/body | Resolve route, validate request, split symbols | Authentication and request validation are C-003; expiry/field checks precede guards | Per-symbol result / HTTP response | Audit counts for HTTP 200; no per-symbol refusal reason in this mechanism | Caller/retry; receiver loop |
| Alias and symbol path | R 696-728, 701 | Raw symbol | Alias lookup, exclusion check | Raw-case handling as documented | Resolved or retained symbol | Passed in-memory | `_process_signal` |
| In-flight claim | R 1074-1091 | Symbol | Locked check-and-set | Existing claim -> `IN_PROCESS` | Claim acquired or refusal | Process-local dictionary | Cache guard / eventual processor |
| Cache dedup | R 937-947 | Symbol, scanner, window | TTL membership then set if absent | Cache hit -> `DUPLICATE` | Repeat refusal | Process-local TTL cache | Database insert if miss |
| Fingerprint insert | R 963-1044; SQL 74-83; ST 506-541 | Scanner, symbol, triggered time, receipt date | Hash scanner/symbol/epoch bucket; unique index | IntegrityError lookup; re-queue `QUEUE_FULL` or duplicate | `ACCEPTED`, `DUPLICATE`, `QUEUE_FULL`, `STORE_ERROR`, or escaped exception | SQLite row/transaction | Queue / processor |
| Claim release | P 1248-1271; R refusal paths | Symbol carried with signal | Release by symbol unless requeued/handed off | No owner/generation check identified | Claim removed | In-memory dictionary | Subsequent intake for same symbol |
| Sweeper | R 1182-1207 | Claim timestamps | Remove claim with heartbeat older than 60 s | Heartbeat not renewed in runtime wiring | Claim evicted/logged | In-memory dictionary | Subsequent intake may proceed |
| Replay | `scripts/replay_signals.py` | Stored signal rows | Read-only SELECTs | Does not use intake guards | Historical stored-row view/replay workflow | No writes identified in reviewed source | Replay consumer outside intake dedup |
| EOD route | R 564, 751-835; SQL EOD table | EOD request | Separate handler | Does not use the three guards | EOD capture | Separate unique symbol/date key | C-011 |

## 17. Manual completion checklist

- [x] Pin and blob IDs recorded.
- [x] Source inventory and bounded read log supplied.
- [x] Three guards, keys, windows, outcomes and storage described.
- [x] Claim/cache/fingerprint lifecycle and restart behaviour described.
- [x] Known defect separated from historical occurrence.
- [x] Test assertions labelled as documentary only.
- [x] Cross-topic bookmarks retained without resolution.
- [x] No V2 decisions or legacy changes made.
- [x] Claude's review corrections applied (revision 1; C-005_review_notes.md section 2).
- [x] Frozen as revision 1 under DR-058 (W-4, DR-017); the freeze takes effect when Rama runs PART 2 of the push commands.

## 18. Revision 1 (2026-10-23)

Revision 0 is ChatGPT's draft of 23-Oct-2026, stored unchanged in docs/inputs/manuals/. Revision 1 changes:

1. Header: the frozen status, the evidence baseline and the inputs' SHA-256, in the form of the other frozen manuals.
2. Section 2.1: the read discipline stated from the read log (46 reads, all matching; protected-path check zero).
3. Section 4.2: the conversion and the minimum cited at R lines 232-235 and 236 (revision 0 cited R lines 171-175, which resolve the webhook block, and R lines 230-237 for the minimum).
4. Section 5.1: the schema's status check, which still allows `IN_PROCESS` and `DUPLICATE` (SQL lines 60-65).
5. Section 6.2: where the claim is released after a failed re-queue (the pre-check path releases explicitly).
6. Section 6.4: `stop()` makes new requests answer 503 and stops the sweeper; it does not stop the server thread.
7. Section 10.2: the basis of the EOD statement (the term search of R lines 751-835).
8. Section 11: the processor test lines listed.
9. Section 13: the Q-45 (h) row; new section 13.1, the frozen statements re-verified, with no erratum.
10. Section 17: the checklist closed; this section added.
11. Typography made ASCII and labels written as FACT (SOURCE-VERIFIED) and so on, as in the other frozen manuals; no statement changed by it.
