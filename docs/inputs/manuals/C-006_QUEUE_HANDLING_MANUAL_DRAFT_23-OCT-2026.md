# C-006 — Queue Handling (Legacy System)

**Status:** Reconciliation draft — not frozen  
**Evidence baseline:** `trading-system-prod-snapshot` at `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
**Research date:** 23-Oct-2026 (IST)  
**Purpose:** Factual record of the legacy signal queue and its hand-offs. This is not a V2 design or a recommendation to revive the legacy system.

## 1. Evidence rules and boundaries

This chapter is based on Claude's Stage 1 repository answers and source inventory/read log dated 23-Oct-2026. The files record 73 gated source reads and 29 count-only searches against the pinned commit. Source claims have path, line-range, blob-ID and read-log references in the companion files.

Evidence labels used here:

- **FACT — SOURCE-VERIFIED:** supported by source lines read at the pinned commit.
- **FACT — CONFIG:** value in the repository's `config/system_config.yaml`, not evidence of a deployed host's effective configuration.
- **FACT — TEST TEXT:** what test source asserts; tests were not run and this is not runtime evidence.
- **FACT — TEXT DRIFT:** comment/docstring differs from executable code or configuration.
- **FACT — EXTERNAL:** external library documentation.
- **INTERPRETATION:** a possible consequence of a source path, not proof that it occurred.
- **HISTORICAL:** a statement in a previously frozen manual.
- **KNOWN DEFECT:** a defect previously recorded and re-verified.
- **UNKNOWN / NOT ESTABLISHED** and **OPEN QUESTION:** evidence did not establish the answer.

**Scope:** the in-memory signal queue between `signals/webhook_receiver.py` and `signals/signal_processor.py`, including the producer/consumer hand-off, queue-full/backpressure interface, re-queue paths, related status writes, shutdown handling and queue observability. The processor's worker-pool internals and other queue systems are outside scope. C-005 owns claims/fingerprint/cache handling; C-007 owns status/schema/retention; C-008 owns worker pipeline logic; C-009 owns expiry; A-011/B-011/B-012 own wider startup/shutdown/restart context.

The two July 2026 queue records were not read or searched. No legacy code or tests were run; no live service or host was contacted; no repository changes, commits or pushes occurred.

## 2. Architecture and ownership

### 2.1 Construction and wiring

**FACT — SOURCE-VERIFIED**

- `main.py` imports the standard-library `queue` module and creates one `queue.Queue` with `maxsize = app_config.system.signal_queue.capacity` (`main.py:3402–3404`).
- The same queue object is passed to the webhook receiver (`main.py:3407–3408`) and the signal processor (`main.py:3575–3576`).
- The repository configuration declares capacity `300`, `backpressure_pct: 0.80`, `expiry_sec: 600`, and `warning_pct: 0.60` (`config/system_config.yaml:124–128`).
- `SignalQueueConfig` declares `capacity: int`, `backpressure_pct: float`, `expiry_sec: int`, and `warning_pct: float = 0.60`; the first three fields have no class defaults. The class shown in the cited range has no validator (`core/config_loader.py:192–200`).
- The configured capacity is passed through as the queue's `maxsize`; no transformation was identified (`main.py:3402–3404`).

**FACT — EXTERNAL / INTERPRETATION:** Python's queue documentation says a `Queue` with `maxsize <= 0` is unbounded. The model accepts an integer, so a configured value at or below zero would imply an unbounded queue; the repository configuration value is 300. This describes the code/library relationship, not a recommendation.

### 2.2 Producer and consumer

| Role | Component | Behaviour |
|---|---|---|
| Producer | `signals/webhook_receiver.py` | Inserts signal state, then places a 5-tuple into the queue using `put_nowait`. It also has a retry path for an existing `QUEUE_FULL` row. |
| Consumer/dispatcher | `signals/signal_processor.py` | One daemon dispatcher thread calls `get(timeout=drain_poll_sec)` and submits each item to `_process_one_safe`. |
| Worker hand-off | `ThreadPoolExecutor` created by the processor | The dispatcher submits the item unchanged as the worker argument. Worker-pool internal queueing is outside C-006. |

Evidence: `main.py:3402–3415, 3575–3623`; `signals/webhook_receiver.py:956–1013, 1046–1072`; `signals/signal_processor.py:310–320, 377–390`.

The processor's pool is configured with `max_workers = worker_count`; the repository configuration says `worker_count: 5` (`signals/signal_processor.py:310–313`; `config/system_config.yaml:343–346`). The configuration model validates the worker count to 1–50 and the processor floors the value at 1 (`core/config_loader.py:588–593`; `signals/signal_processor.py:220`).

**INTERPRETATION:** because the dispatcher takes an item before submitting it and does not wait for a worker to become free, queue depth excludes items already submitted to the pool, including any work waiting there. C-006 did not inspect the pool's internal queue.

## 3. Configuration and thresholds

| Setting | Repository value | Use established in C-006 | Evidence |
|---|---:|---|---|
| `signal_queue.capacity` | `300` | Queue `maxsize`; basis for thresholds | `config/system_config.yaml:124–128`; `main.py:3402–3404` |
| `signal_queue.backpressure_pct` | `0.80` | Per-request backpressure threshold | `config/system_config.yaml:125–126`; `signals/webhook_receiver.py:571–578` |
| `signal_queue.warning_pct` | `0.60` | Adds `X-Queue-Warning: high` to the batch response at/above threshold | `config/system_config.yaml:128`; `signals/webhook_receiver.py:574, 741–744` |
| `signal_queue.expiry_sec` | `600` | Expiry is owned by C-009; its processing check was not read here | `config/system_config.yaml:127` |
| `signal_processor.worker_count` | `5` | Pool worker count | `config/system_config.yaml:345`; `signals/signal_processor.py:310–313` |
| `signal_processor.drain_poll_sec` | `0.1` | Dispatcher queue-get timeout | `config/system_config.yaml:346`; `signals/signal_processor.py:385–387` |
| `signal_processor.pipeline_timeout_sec` | `30` | Application of this timeout not established in C-006 | `config/system_config.yaml:347` |

Threshold arithmetic from the repository values:

- Backpressure: `int(capacity × backpressure_pct) = int(300 × 0.80) = 240`.
- Warning: `int(capacity × warning_pct) = int(300 × 0.60) = 180`.

These are source-derived calculations, not claims about the configuration active on any host.

**FACT — TEXT DRIFT:** the config-model comment gives `expiry_sec` a default of 60, but the field has no default in the cited class and the repository config says 600 (`core/config_loader.py:196`; `config/system_config.yaml:127`). C-009 owns expiry.

## 4. Receiver gates and enqueue path

### 4.1 Request-level backpressure

The receiver applies the queue-depth gate once per request inside `_process_request`:

1. shutting-down check;
2. per-IP limit;
3. authentication;
4. unknown-scanner check;
5. EOD branch;
6. kill-switch check;
7. queue-depth backpressure gate;
8. entry-window check;
9. later request parsing and per-symbol handling.

The backpressure gate is therefore **per request, not per symbol**, and occurs before the per-symbol loop. At `qsize() >= 240`, it responds with HTTP 503 and `{"error": "Signal queue at capacity; retry later"}`, with `X-Queue-Depth` (`signals/webhook_receiver.py:483–584`, especially `570–578`). Body parsing was not re-read in this topic; B-008 remains a historical pointer for that surrounding flow.

The queue warning threshold does not reject a request. After the symbol loop, if depth is at least 180, the batch answer receives `X-Queue-Warning: high` (`signals/webhook_receiver.py:574, 741–744`).

**FACT — EXTERNAL:** Python documents `qsize()` as approximate. **INTERPRETATION:** the receiver can have concurrent request threads and one admitted request can carry multiple symbols; therefore a request that passes the initial depth check does not guarantee that the queue remains below 240, and a queue can subsequently fill. Runtime frequency is **UNKNOWN / NOT ESTABLISHED**.

### 4.2 First enqueue

The source path establishes this sequence:

1. The receiver generates a new `signal_id` (`signals/webhook_receiver.py:957`).
2. It performs the surrounding validation/deduplication path, which is covered by C-004/C-005.
3. It inserts the signal row with status `QUEUED` and commits (`signals/webhook_receiver.py:963–980`).
4. It constructs the tuple `(signal_id, scanner_name, symbol, price, triggered_at)` and calls `put_nowait` (`signals/webhook_receiver.py:1046–1049`).
5. On successful enqueue, the response marks the signal accepted; the claim remains for the processor (`signals/webhook_receiver.py:1070–1072`).

This means the database insert/commit precedes the in-memory enqueue. A queue-full failure can therefore leave a database row even though no queue item was added.

### 4.3 Queue-full handling

When `put_nowait` raises `queue.Full`, the receiver attempts to update the row to `QUEUE_FULL`, removes the cache entry and releases the claim; the request's batch response becomes HTTP 503 when a symbol result is `QUEUE_FULL` or `STORE_ERROR` (`signals/webhook_receiver.py:1050–1068, 730–734`). A failure to mark `QUEUE_FULL` is logged, but the request still reports the queue-full result.

The response's `X-Queue-Depth` contains depth/capacity. The audit-row implementation records counts from 200 responses; the source-derived finding is that a 503 batch records zero accepted and zero rejected in that audit representation (`signals/webhook_receiver.py:506–517, 529–534`). Whether senders actually retried is **UNKNOWN / NOT ESTABLISHED**.

## 5. Re-queue paths and item shapes

Three re-queue paths appear in the processor/receiver source. One is present in code but not wired by `main.py` at the pinned commit.

| Path | Item | Limit/wait | Status behaviour | Evidence |
|---|---|---|---|---|
| Receiver retry for same fingerprint where existing row is `QUEUE_FULL` and enqueue succeeds | 5-tuple using the old `signal_id`, with this request's scanner/symbol/price/time | `put_nowait`; no retry counter or wait | After the put, tries to set row to `QUEUED`; failure is logged; response stays `ACCEPTED` | `signals/webhook_receiver.py:981–1013` |
| FIX-007 rate-limit pre-check | Same item put back unchanged | `put(timeout=1.0)` | No status write on successful re-queue; on `Full`, attempts `REJECTED` with reason `QUEUE_FULL` | `signals/signal_processor.py:415–461` |
| Broker rate-limit error during placement | Dict with `signal_id`, `scanner_name`, `symbol`, `trigger_price`, `triggered_at`, `retry_count` | At most 3 re-queues; `put(timeout=1.0)`; no delay in the cited code | No status write on success; on full/exhaustion, `PLACEMENT_FAILED` via outer handler | `signals/signal_processor.py:1486–1532, 1233–1238, 1260–1271` |

The consumer accepts both tuple and dict item forms (`signals/signal_processor.py:878–887`). Tuple items start with retry count 0. The docstring describes `triggered_at` as a naive IST datetime (`signals/signal_processor.py:874`).

### 5.1 Wired versus unwired path

**FACT — SOURCE-VERIFIED:** the rate-limit pre-check path is not wired at the pinned commit: `main.py` constructs the processor without `rate_limiter`, the constructor default is `None`, and the pre-check runs only when the limiter is set (`main.py:3575–3623`; `signals/signal_processor.py:189, 418`). Tests set it directly on the object; that is test setup, not evidence of production wiring.

### 5.2 Re-queue ordering and status race — bookmark

**INTERPRETATION / OPEN BOOKMARK C-006/BM-01:** the receiver's `QUEUE_FULL` retry performs the queue put before attempting to update the row to `QUEUED`. That update is unconditional on the row's current status. If the processor writes `PROCESSING` or a rejection status before the receiver's update completes, the later update could overwrite it with `QUEUED`. The source establishes the ordering; actual occurrence is **UNKNOWN / NOT ESTABLISHED**. Cross-topic owner: C-007.

**INTERPRETATION / OPEN BOOKMARK C-006/BM-02:** if the receiver's attempt to mark a full queue as `QUEUE_FULL` fails, the row may remain `QUEUED` without a queue item while the response is still 503. A later request in the same fingerprint bucket may then be treated as `DUPLICATE` rather than re-enqueued. This is a possible source-path consequence, not evidence of an observed incident. Cross-topic owner: C-007.

**FACT — TEXT DRIFT:** a comment says the placement retry path has “45s total, 15s each”, but the cited re-queue code has no such wait. What might account for the 15 seconds is **UNKNOWN / NOT ESTABLISHED** because the relevant rate-limiter/order-placer internals were not in scope (`signals/signal_processor.py:1489`; OQ-2).

**INTERPRETATION / OPEN BOOKMARK C-006/BM-06:** the unwired pre-check's full-queue branch indexes `signal_tuple[1]`. For a dict item, that would be a key lookup by integer and could raise before the status/release logic. This path is not wired at the pinned commit; it is recorded as a source-path issue, not a live failure.

## 6. Dispatch and worker hand-off

**FACT — SOURCE-VERIFIED**

- One daemon dispatcher thread (`sp-dispatcher`) is created by the processor (`signals/signal_processor.py:314–320`).
- It calls `get(timeout=drain_poll_sec)`; the repository value is 0.1 seconds. The cited code skips on any exception (`signals/signal_processor.py:385–387`; `config/system_config.yaml:346`).
- It calls `_warm_zones` as a guarded side hand-off, then `self._executor.submit(self._process_one_safe, item)` (`signals/signal_processor.py:361–390`).
- The executor is a `ThreadPoolExecutor(max_workers=worker_count, thread_name_prefix="sp-worker")`; repository `worker_count` is 5 (`signals/signal_processor.py:310–313`; `config/system_config.yaml:345`).

The zone-warmer queue and the worker pool's internal work queue were not analysed; the latter is outside the C-006 boundary.

**INTERPRETATION / OPEN BOOKMARK C-006/BM-04:** `qsize()`, the receiver's 503 gate, and the queue-depth headers only reflect items not yet taken by the dispatcher. They do not include work already submitted to the worker pool. Whether this caused under-reporting or backpressure problems in the live period is **UNKNOWN / NOT ESTABLISHED**.

## 7. Status writes and storage boundary

The queue-related status path includes `QUEUED`, `QUEUE_FULL`, `PROCESSING`, several `REJECTED_*` values including `REJECTED_SHUTDOWN` and `REJECTED_KILL_SWITCH`, `PLACEMENT_FAILED`, and `TIMEOUT`. The source answer records the cited write locations in `signals/webhook_receiver.py`, `signals/signal_processor.py`, and `core/state_store.py`.

The processor's `update_signal_status` performs an update of `status` and `rejection_reason` by `signal_id` without a condition on the row's current status (`core/state_store.py:1408–1426`). The schema's CHECK constraint accepts the listed statuses (`core/schema.sql:60–70`).

**Scope boundary:** this chapter records only status writes necessary to understand queue lifecycle. C-007 owns full status semantics, database schema and retention. Whether a deployed database's constraint exactly matches the repository schema is **OPEN QUESTION OQ-1**.

## 8. Stop, kill switch, crash and restart

### 8.1 Clean stop

**FACT — SOURCE-VERIFIED**

- `main.py` stops the receiver before the processor (`main.py:1681–1692`).
- Receiver stop sets a shutting-down flag and returns. New requests then receive HTTP 503 with a shutdown message and an audit row. The cited stop path does not wait for requests already past the initial shutdown check (`signals/webhook_receiver.py:1166–1176, 483–490`).
- Processor stop sets its stop flag. The dispatcher exits its loop and submits items remaining in the queue to the worker pool (`signals/signal_processor.py:333, 392–399`).
- Main joins the dispatcher for up to 10 seconds, then waits for submitted work without a time limit (`signals/signal_processor.py:336–340`).
- A drained item runs the normal pipeline. If it reaches the placement check, the stop flag causes `REJECTED_SHUTDOWN` and reservation release. A signal rejected at an earlier pipeline stage can receive that stage's status instead (`signals/signal_processor.py:897, 905–912, 1193–1201, 1398–1405`).

**FACT — TEXT DRIFT:** a processor docstring says “up to 5s per SP4”; the cited code joins for 10 seconds and then waits for the pool without a time limit (`signals/signal_processor.py:327–340`).

**INTERPRETATION / OPEN BOOKMARK C-006/BM-03:** a request that passed the receiver's shutdown check before the flag changed, or a worker that re-queues during shutdown, could add an item after the dispatcher's final drain. The cited dispatcher path would not process that late item; its database row could remain non-terminal. This is a code-path inference, not proof it occurred.

### 8.2 Kill switch

A new request is rejected with HTTP 403 before the queue-depth gate when the kill switch is active (`signals/webhook_receiver.py:566–568`). Items already queued are handled by the processor and may be marked `REJECTED_KILL_SWITCH` or `REJECTED_KILL_SWITCH_LATE` along the cited pipeline paths (`signals/signal_processor.py:905–907, 1396`). This describes source logic only.

### 8.3 Crash and next start

The queue is process-memory state created in `main.py`. A process crash therefore loses queued items. The source search found no next-start path that re-enqueues rows left `QUEUED`; this is bounded to the searches and files recorded in the evidence inventory. Such rows may remain `QUEUED`; rows already `PROCESSING` may also remain so, and `QUEUE_FULL` rows remain `QUEUE_FULL`.

The inspected EOD cleanup path changes stale `PROCESSING` rows to `EXPIRED` and leaves `QUEUED` and `QUEUE_FULL` unchanged (`scripts/eod_cleanup.py:105–127`). Scheduling/operation of that cleanup is owned elsewhere. The next-start conclusion is bounded by the count-only search and is not a proof about every possible uninspected mechanism.

## 9. Queue observability and known defect

| Surface | Source-derived behaviour | Evidence |
|---|---|---|
| `X-Queue-Depth` | Reports depth/capacity on the 503 gate response and batch answer | `signals/webhook_receiver.py:577, 741–742` |
| `X-Queue-Warning` | `high` when post-loop depth reaches 180 | `signals/webhook_receiver.py:574, 743–744`; config line 128 |
| Webhook `/health` | Exposes `queue_size`, `queue_capacity`, `queue_depth`; the cited route also has IP-limit/auth gates | `signals/webhook_receiver.py:364–404` |
| Health-server `/metrics` | Queue fields are fixed at 0 in the cited health-server code; main merges processor runtime metrics, which have no queue key | `scripts/healthcheck_server.py:153–154`; `main.py:4076`; `signals/signal_processor.py:683–738` |
| `SignalProcessor.stats()` | Contains `queue_depth` and `workers_active`; no caller was found in main or runtime files in the recorded searches | `signals/signal_processor.py:2663–2703`; search Q29 |
| Pre-flight check | `webhook_backpressure` reads `/metrics`, warns at 90% of reported capacity, and skips if `/metrics` is unavailable | `scripts/preflight/checks/signals.py:60–85` |
| Logs | Start/stop INFO; re-queue WARNINGs; failed status-mark ERRORs in cited paths | Receiver/processor ranges listed in the source inventory |
| Status report | Includes `QUEUED` and `QUEUE_FULL` among known statuses | `reports/signal_status.py:46` |
| Dashboard capacity service | Displays a “Signal Queue” row with configured threshold and live-size input or “trader down” | `ops_dashboard/backend/services/capacity.py:114, 171–181` |

**KNOWN DEFECT (previously recorded in B-007 item 10, re-verified):** the `/metrics` queue fields have no setter in the inspected health-server/processor path, so the pre-flight check can read capacity 0 and pass without meaningful queue-capacity data. The live period's actual pre-flight decisions are **UNKNOWN / NOT ESTABLISHED**.

## 10. Documentary test evidence

Tests were read as documents only and were never run. Their assertions include:

- graduated backpressure/header cases, including 503 at the configured test threshold (`tests/unit/test_fix134_backpressure.py`);
- `/health` auth/queue fields, receiver `QUEUE_FULL`, same-`signal_id` retry and concurrent POST cases (`tests/unit/test_webhook_receiver.py`);
- an integration scenario for the full-queue retry (`tests/integration/test_hardening_scenarios.py`);
- processor start/stop, FIX-007 re-queue and placement retry assertions (`tests/unit/test_signal_processor.py`);
- shutdown order assertions (`tests/unit/test_e4_event_loop_safety.py`);
- a crash-test state-machine model with `DROPPED_BACKPRESSURE` and `DROPPED_SHUTDOWN`, which the recorded runtime search did not find as runtime status writes.

These establish what test source asserts, not what happened in production. No sender's real retry, queue-full incident, re-queue race, or late-stop item is established by the reviewed tests.

## 11. Frozen-manual reconciliation and bookmarks

The source evidence re-verifies these frozen leads: C-002's queue capacity/backpressure/header statements; B-009's config and placement re-queue references; B-006's five-worker pool and stop timing; B-007's `/metrics` defect; A-011/B-006's in-memory queue/restart description; and C-005's failed status-flip handling.

Two corrections/qualifications are especially important:

1. **A-011 Q31 is only partly supported.** The dispatcher drain is present, but not every drained item necessarily becomes `REJECTED_SHUTDOWN`; the status is written at the placement check, and earlier-stage rejections can use other statuses. A late item may miss the final drain. Bookmark C-006/BM-03 remains open for A-011/B-011 review.
2. **C-005's rate-limit pre-check path is present but not wired at this pin.** C-005 listed the path, but `main.py` constructs the processor without the limiter. Bookmark C-006/BM-06 remains open for C-005/C-008.

All eight bookmarks remain open for their assigned topics:

- **BM-01 — C-007:** receiver retry can enqueue before an unconditional `QUEUED` status update; failed update is logged while response remains `ACCEPTED`.
- **BM-02 — C-007:** failed `QUEUE_FULL` status update can leave a `QUEUED` row without a queue item; later same-fingerprint retry may be classified `DUPLICATE`.
- **BM-03 — A-011/B-011:** clean-stop drain/status nuance and potential late items.
- **BM-04 — B-006 / whole-design review:** depth excludes worker-pool waiting work.
- **BM-05 — C-007/C-008:** retry item's price/time may differ from the persisted row's original values.
- **BM-06 — C-005/C-008:** unwired FIX-007 pre-check and its dict-item full-queue branch.
- **BM-07 — B-007 / observability topic:** `/metrics` queue fields fixed at zero and unused `stats()` queue-depth output.
- **BM-08 — C-009/B-006/C-008/test topics:** text drift around expiry default, stop duration, retry wait, and crash-test-only `DROPPED_*` labels.

Open questions also remain: deployed schema constraint (OQ-1); origin of the commented 15-second retry timing (OQ-2); whether any of these behaviours occurred during the live period (OQ-3, historical records excluded); and how `pipeline_timeout_sec` is applied (OQ-4).

## 12. Known limits and exclusions

- Four groups of candidate files were identified by count-only search but not read: `scripts/load_test_signals.py`; `tests/unit/test_fix074_numeric_cast.py`; several status readers; and unrelated queue mechanisms/tests. Their names and reason for possible future review are listed in the repository answer sheet. They are not silently treated as analysed.
- A count-only search establishes that a pattern matched a file; it does not establish the meaning of the matching lines. A file not opened is not source-verified.
- The two July 2026 queue records remain excluded. No historical incident conclusion should be made without separate approval and a properly bounded review.
- The independent verification agent was not run; Claude's delivery checks are self-checks. The source inventory records this explicitly.
- No live/host configuration, database state, runtime occurrence, or deployed schema was verified.

## 13. End-to-end source path

```text
main.py creates Queue(maxsize=300)
        |
        +--> webhook receiver (producer)
        |       request-level gates
        |       -> per-symbol validation/deduplication (C-004/C-005)
        |       -> INSERT + commit status=QUEUED
        |       -> put_nowait(5-tuple)
        |             | success: ACCEPTED
        |             | full: attempt QUEUE_FULL status, release claim, batch 503
        |             | later retry of QUEUE_FULL row: put old signal_id, then flip to QUEUED
        |
        +--> signal processor (dispatcher/consumer)
                get(timeout=0.1)
                -> guarded zone-warmer hand-off
                -> executor.submit(_process_one_safe, item)
                      -> worker pipeline (C-008)
                      -> PROCESSING/status writes (C-007 boundary)
                      -> placement rate-limit re-queue path, up to 3 (source code)
                stop: final queue drain -> submit remaining items -> await workers
                crash: in-memory queue is lost; no re-enqueue-at-start path found
```

## 14. Final factual position

The pinned source defines a single in-memory signal queue of capacity 300, a per-request backpressure gate at 240, a warning threshold at 180, one dispatcher and a five-worker executor. The receiver commits a `QUEUED` row before enqueueing; queue-full and re-queue paths can produce differences between database status and queue contents. Clean stop drains the current queue into the pool, but status completion is not uniform, and late items may miss the final drain. The `/metrics` queue fields are a previously recorded known defect. These are source-derived descriptions and code-path interpretations, not proof that any specific queue event occurred in production.

**Freeze status:** not frozen. ChatGPT/Rama reconciliation and approval are required before this chapter is finalised or stored in the V2 documentation repository.
