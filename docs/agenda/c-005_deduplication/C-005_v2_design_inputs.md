# C-005 DEDUPLICATION AND IN-FLIGHT HANDLING: V2 DESIGN INPUTS

Status: PROPOSED / OPEN (23-Oct-2026). Written with C-005_manual.md revision 1 (FROZEN, DR-058); it stays PROPOSED / OPEN after
the C-005 manual is frozen. Nothing here is a V2 decision (W-1, W-6); each item is a consideration for the whole-design review
that Rama scheduled for after the agenda (W-9), or for the owning topic named.
Sources: C-005_manual.md (legacy facts, sections cited per item); B-007_v2_design_inputs.md item 12; V2_DESIGN_LOG VP-12, Q-33
and Q-45 (h).

| # | Legacy fact (C-005_manual.md) | Consideration for V2 (PROPOSED / OPEN) | Owners |
|---|---|---|---|
| 1 | Three repeat guards with three keys and two window shapes: a claim per symbol, a cache per (symbol, scanner) anchored at first receipt, a fingerprint per (scanner, symbol, epoch bucket of the alert time) and receipt date (sections 3.2, 4.3, 4.4) | Whether a repeat is defined once (which fields, which window, which clock) and every layer uses that one definition | C-006 to C-011; whole-design review |
| 2 | Claim and cache are process memory, empty after a restart; only the database fingerprint survives (sections 6.4, 7, 8) | Whether repeat state must survive a restart, and how long | B-012, C-007 |
| 3 | The claim's heartbeat is not renewed on the inspected runtime path, so the 60-second sweeper can evict a claim whose work is still active (sections 6.3, 12.1; KNOWN DEFECT) | A claim as a lease renewed by the worker that holds it; what expiry does (alert, release or both); telling a false stall from a real one (B-007 item 12) | C-008, B-007 |
| 4 | Release is by symbol with no owner check, so an earlier signal's release could remove a later signal's claim (section 12.2, C5-L1) | A claim token checked at release | C-008 |
| 5 | Refused repeats (IN_PROCESS, cache DUPLICATE, database DUPLICATE) leave no per-symbol record; the request audit counts them only when the answer is 200 (section 5.1) | Whether every refusal is recorded with its reason and identity (the traceability requirement of DR-006) | C-007, L topics |
| 6 | A failed status flip on the re-queue path still answers ACCEPTED (section 12.5, C5-L4) | Status changes and queue hand-off as one atomic step | C-006, C-007 |
| 7 | The symbol's case reaches every key (section 4.1; C-004 L-04) | One checked, normalised symbol before any key (read with C-004's V2-1 and V2-2 and VP-28: a symbol with no broker match is rejected) | C-004_v2_design_inputs.md, VP-28, C-006 |
| 8 | Intake deduplication is not the same-symbol trade rule: the legacy rule lived in the processor and the risk engine (A-008_manual.md section 14), while the receiver refuses repeats for a window only | VP-12 (one trade per stock per day in any direction, PROPOSED) names C-005 / F-007 as its ratification point; not ratified at C-005; left for the whole-design review with F-007 (bookmark BK-02) | F-007, C-008; whole-design review |

PROPOSED / OPEN. C-005 decides nothing for V2.
