# C-005 Reconciliation — Stage 1 Evidence Review
**Date:** 23-Oct-2026  
**Status:** ChatGPT reconciliation draft for discussion; not frozen  
**Legacy baseline:** `trading-system-prod-snapshot` at `970aabf3c78d777d9d34549e021f7c8e756c7bdb`

## 1. Review decision

**Stage 1 is sufficient to prepare the C-005 manual draft.** The answer sheet and research log provide a coherent pinned-source account of the receiver's three repeat guards, claim/cache/fingerprint lifecycles, processor release wiring, restart behaviour, tests as documentary evidence, and cross-topic boundaries.

The Stage 1 result does not authorise any legacy modification, execution, operational revival, V2 design choice, repository write, commit, or push. This reconciliation and the accompanying manual are drafts for review.

## 2. Evidence and method assessment

| Check | Finding | Review treatment |
|---|---|---|
| Repository identity | The answer and inventory identify the pinned production snapshot and blob IDs for cited source files. | Accept as the declared evidence baseline. |
| Read discipline | Inventory reports 46 logged reads; every read matched the pinned blob; protected-path check stayed at zero; no tests/code or live/host systems were used. | Accept subject to the documented self-check limitation. |
| Source inventory | Direct candidates, conditional rows, tests and excluded paths are enumerated with reasons. | Sufficient for the C-005 bounded scope. |
| Tests | Test source was read but not executed. | Treat only as evidence of assertions written in tests, never as runtime proof. |
| Independent verification | No independent agent check was run, as explicitly disclosed. | Record as a limitation, not a blocker. |
| Deviation | `tests/unit/test_e4_event_loop_safety.py` lines 380–410 were read to finish a test beginning at the cited range boundary. | Minor, transparent, scope-related deviation; no further expansion inferred. |
| Protected-path populations | Stage 1 clarifies that the 102 protected paths and 102 excluded name hits are distinct populations with zero overlap by construction. | Stage 0 ambiguity resolved. |

## 3. Reconciled technical account

### 3.1 Three guards and their distinct roles

The source-backed order for the intraday symbol path is:

1. **In-flight claim:** in-memory `_in_flight`, keyed by symbol only; a current claim returns `IN_PROCESS`.
2. **TTL cache:** in-memory `_dedup_cache`, keyed by `(symbol, scanner_name)`; a cache hit returns `DUPLICATE`.
3. **Database fingerprint uniqueness:** unique index on `(fingerprint, fingerprint_date)`; a collision either re-queues an earlier `QUEUE_FULL` row or returns `DUPLICATE`.

The guards are not interchangeable. The first is cross-scanner for the same exact symbol string; the second and third include scanner identity. The symbol is not case-normalised inside `_process_signal`, so casing differences can produce different keys. The strategy map and generated `signal_id` are not part of the identified keys.

**Evidence:** `signals/webhook_receiver.py` lines 892–1072, 701, 937–980; `core/schema.sql` lines 74–83; `core/state_store.py` lines 506–563; blob IDs are in the Stage 1 answer sheet.

### 3.2 Timing and persistence

- Repository config sets `webhook.dedup_window_seconds: 300`; the receiver converts the setting to an integer, falls back to 300 on `TypeError`/`ValueError`, and applies a minimum of 60 seconds.
- The cache is a `TTLCache` with maximum size 10,000; the entry is set only when absent and is not renewed by a cache hit, according to the source path. Exact expiry semantics rely on external `cachetools` behaviour; the library version was not verified.
- The fingerprint uses SHA-256 of `scanner|symbol|epoch bucket`, with the bucket derived from `triggered_at` and the configured window; `fingerprint_date` is the IST receipt date. This differs from stale schema comments describing a trigger minute.
- In-memory claim/cache state is empty after receiver construction/restart. The database fingerprint row persists subject to database retention, which belongs to C-007.
- The host's deployed configuration value and actual production event frequencies are not established by the repository source reviewed here.

### 3.3 Claim lifetime and known defect

The receiver has a sweeper that removes claims with a heartbeat older than 60 seconds, running on a 60-second interval. The only heartbeat update helper has no runtime caller in the pinned wiring: the processor's heartbeat checkpoints are no-ops because `main.py` does not supply `in_flight_heartbeat_fn`. This re-verifies the pre-existing B-007 known defect. It does **not** establish that a live claim was actually evicted in production.

The claim is released by the receiver on the documented refusal/error paths and by the processor's `finally` path, except where a signal is re-queued or handed to allocator enforcement. A claim held through a re-queue can therefore be evicted by the sweeper while work remains in progress.

The C-004 L-13 exception path is confirmed at source level: an exception inside the `IntegrityError` handler can escape, produce HTTP 500, and leave that symbol's claim and cache entry held. The claim is bounded by the sweeper under the no-heartbeat wiring; the cache entry is separately bounded by its TTL. Occurrence is not established.

### 3.4 Additional leads (not findings of occurrence)

- **C5-L1 — release ownership:** release/pop is keyed by symbol and has no owner/generation check. After a stale claim is evicted and a new claim for the same symbol is acquired, an earlier worker's later release could remove the newer claim. This is a code-path interpretation, not an observed incident.
- **C5-L2 — different window shapes:** cache TTL starts when an entry is first set; the fingerprint uses fixed epoch-aligned buckets and receipt date. After restart, only the database fingerprint remains. A repeated alert in a later bucket may therefore not collide with the prior fingerprint even if elapsed wall-clock time is under the cache TTL; the exact outcomes depend on the full path and stored row.
- **C5-L3 — documentation/test text drift:** schema comments and some test docstrings describe older fingerprint/duplicate semantics; the crash-test helpers model stored `IN_PROCESS`/`DUPLICATE` states that the reviewed receiver path returns without writing to `signals`.
- **C5-L4 — re-queue status flip:** on one re-queue path, failure to flip the stored status is logged while the response still says `ACCEPTED`; the report says the row can remain `QUEUE_FULL` until processor state changes it. Owner: C-006/C-007.

These should remain evidence-backed bookmarks. Do not “fix” them in the legacy manual or select a V2 treatment.

## 4. BM-01 reconciliation

The four frozen manuals align when placed on one timeline:

| Earlier frozen statement | Stage 1 source finding | Reconciliation |
|---|---|---|
| B-009: processor releases on completion except re-queue/allocator hand-off | Processor `finally` release plus explicit re-queue and allocator flags | Consistent; both re-queue paths retain the claim while queued for retry. |
| C-004 L-13: receiver exception can leave claim/cache held | Exception escapes the `IntegrityError` handler; no local guard releases them | Consistent. Sweeper can later remove the claim; cache remains until TTL/restart/eviction. |
| B-007: heartbeat not renewed; 60-second sweeper | Helper exists but has no runtime caller in pinned wiring; sweeper removes stale claims | Consistent and re-verified as a latent known defect. |
| C-002: concurrent same-symbol repeat returns `IN_PROCESS` | Claim check returns `IN_PROCESS` while claim remains present | Consistent while held; after eviction/release the path can advance to cache/database checks. |

No contradiction was identified among these four prior statements. The newly identified ownerless-release race remains a separate lead.

## 5. Scope decisions and pending expansions

**Do not expand the C-005 source-reading scope at this point.**

- `scripts/eod_cleanup.py` remains unread. Its fingerprint/status references are a C-007/B-010 bookmark. The C-005 manual should say retention/pruning behaviour is not traced here and cross-reference C-007.
- `core/config_loader.py` remains unread. Its type/default/validation behaviour is not established by this C-005 review; cross-reference B-002. The receiver's own fallback and minimum are documented from its source.
- No historical host records were read. The frequency of `IN_PROCESS`, cache `DUPLICATE`, and database `DUPLICATE` in live operation remains an open records question.

The two proposed expansions do not block the factual C-005 chapter because their ownership is elsewhere and the unknowns can be stated explicitly.

## 6. Required labels for the manual

- **FACT — SOURCE-VERIFIED:** directly supported by pinned source ranges.
- **FACT — TEST TEXT:** a test assertion exists; tests were not run.
- **FACT — CONFIG:** value in the repository configuration, not necessarily the host's deployed value.
- **INTERPRETATION:** a possible consequence of the source path, not an observed event.
- **HISTORICAL:** claim carried from frozen manuals/records, with the source distinction retained.
- **KNOWN DEFECT:** the already-established B-007 heartbeat wiring defect, re-verified at the pin.
- **UNKNOWN / NOT ESTABLISHED:** host settings, occurrence/frequency, runtime effects, or uninspected owner-specific behaviour.
- **OPEN QUESTION / BOOKMARK:** cross-topic matter to retain without resolving it in C-005.

## 7. Cross-topic bookmark register

| Bookmark | Owner / cross-reference | Treatment in C-005 |
|---|---|---|
| BM-01 lifecycle alignment | B-007, B-009, C-002, C-004 | Reconciled; no contradiction. Preserve latent defect/lead distinctions. |
| BM-02 failed queue-status flip | C-006, C-007 | Record only; no correction or policy decision. |
| BM-03 EOD cleanup/status/fingerprint retention | C-007, B-010 | File not read; do not infer its exact effects. |
| BM-04 downstream handling after claim eviction/ownerless release | C-008, A-008 | Record only. |
| BM-05 processor in-flight count vs receiver claim | A-005, C-008 | Distinguish the mechanisms; do not merge their meanings. |
| BM-06 EOD route bypasses intake guards | C-011 | Record as an interface boundary. |
| C5-L1 | Future design review | Ownerless symbol release may remove a newer claim; occurrence not established. |
| C5-L2 | Future design review | Cache and fingerprint use different window shapes. |
| C5-L3 | Documentation/test drift | Preserve as historical code/documentation mismatch. |
| C5-L4 | C-006/C-007 | Status flip can fail while response is accepted. |

## 8. Conclusion / review status

The evidence is adequate for a detailed factual C-005 manual draft with explicit limitations. No source-level contradiction requires a new investigation before drafting. The only scope deviation is the narrowly extended test range, transparently recorded. The two optional expansions are not approved here and are not required to finish C-005.

**Not frozen:** this reconciliation, all C5-L1–L4 interpretations, and any proposed V2 response. Rama's discussion/approval is required before finalising/freeze.
