# C-006 QUEUE HANDLING: V2 DESIGN INPUTS

Status: PROPOSED / OPEN (23-Oct-2026). Written with C-006_manual.md revision 1 (FROZEN, DR-059); it stays PROPOSED / OPEN after
the C-006 manual is frozen. Nothing here is a V2 decision (W-1, W-6); each item is a consideration for the whole-design review
that Rama scheduled for after the agenda (W-9), or for the owning topic named.
Sources: C-006_manual.md (legacy facts, sections cited per item); C-005_v2_design_inputs.md item 6; V2_DESIGN_LOG VP-59 and
BK-04.

| # | Legacy fact (C-006_manual.md) | Consideration for V2 (PROPOSED / OPEN) | Owners |
|---|---|---|---|
| 1 | One in-memory queue; a crash loses its items, their rows stay QUEUED or PROCESSING, and nothing found re-queues them at the next start (sections 2.1, 8.3) | Whether accepted work must survive a restart (a durable queue, or closure of non-terminal rows at start with a reason), as VP-59 proposes | B-012, C-007; VP-59; whole-design review |
| 2 | Backpressure and the depth headers count only the bounded queue; work already handed to the pool is not counted (sections 2.2, 6; BM-04) | Where capacity is measured and enforced, so that every stage that can hold work is counted | B-006; whole-design review |
| 3 | The row is committed QUEUED before the put; status flips are separate, unconditional updates whose failures are only logged, one of them after the put (sections 4.2, 5.2; BM-01, BM-02) | The queue hand-off and the status change as one atomic step, with a check of the current status before a transition | C-007 (read with C-005_v2_design_inputs.md item 6) |
| 4 | Three re-queue paths with different item shapes and limits, one of them not wired (section 5; BM-06) | One re-queue path, one item shape, explicit retry limits and waits, and every path wired or removed | C-008 |
| 5 | A re-queued row keeps the first attempt's values while the item carries the retry's (section 5; BM-05) | The queue item carries only an identity, and the row stays the single source of the signal's values (DR-006 traceability) | C-007, C-008 |
| 6 | On a clean stop drained items run the whole pipeline; REJECTED_SHUTDOWN comes only at placement; items put after the final drain are not processed (section 8.1; BM-03) | Defined stop semantics: stop intake first, then drain or persist the queue, and give every queued signal a terminal status with its reason | A-011, B-011; VP-59 |
| 7 | /metrics queue fields stay 0, the depth appears only on the webhook /health, P.stats() is unused, and the pre-flight check passes on capacity 0 (section 9; BM-07) | One metrics source for queue depth, capacity and pool backlog, and checks that fail when capacity is unknown | L topics, B-007 |
| 8 | One QUEUE_FULL symbol makes the whole request answer 503 although other symbols were accepted, and the audit row then counts 0 and 0 (section 4.3) | Per-symbol outcomes the sender can act on, and an audit that counts every answer | C-001, C-002, L topics; VP-59 |
| 9 | The queue settings have no validator; a capacity of 0 or less would build an unbounded queue (sections 2.1, 3) | Validated bounds for every queue and pool setting at start | B-002; configuration design (build phase) |

PROPOSED / OPEN. C-006 decides nothing for V2.
