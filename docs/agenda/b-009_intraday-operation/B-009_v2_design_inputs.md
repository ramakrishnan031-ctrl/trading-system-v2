# B-009 INTRADAY OPERATION: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, revision 1 2026-10-20 (rows 1 to 3 reworded to match the manual's scoped D1 to D3; first pass the same day). It stays PROPOSED / OPEN after the B-009 manual's freeze (DR-046, 2026-10-20); only this sentence changed at the freeze. The owner cell of row 1 corrected on 2026-10-21 at the Section B close (DR-052, C-1 item 2), owner text only. Nothing here is a V2 decision. Items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (B-009_manual.md). CLAUDE CONSIDERATION = a question for the owner, not a requirement.

| # | LEGACY finding | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | A strategy setting (pullback wait) was declared, loaded and validated, but no source path fed signals to the mechanism it names (section 15, D1) | Whether every strategy setting must have a traced consumer, with a start-up check that refuses a setting nothing reads | configuration design (build phase), VP-68, N-001 |
| 2 | On the traced placement path the declared order protocol was not read; the placer's default was used (section 7, D2) | Whether the protocol actually used should be recorded on each order and compared with the strategy's declaration | G topics, L-008 |
| 3 | In source, the order poller stopped itself after 3 failures, and with it the 15:15 entry cancellation; whether this happened in production is unknown (section 8, D3) | Which control owns a dead poller (restart, stop entries, or both), and whether time-based controls should run on their own schedule rather than inside a poller | I-009, M-006, VP-64 |
| 4 | The signal alert was sent before placement (section 9) | Whether an operator message says "placed" only after the broker accepted the order | L-003, L-005 |
| 5 | No session state; eligibility was re-read from the clock at three gates (section 3) | Whether V2 has one session state that every gate reads (VP-57's illustrative names), and how it is recorded | VP-57, O-004 |

Status and boundary: PROPOSED / OPEN. No VP row is opened or changed by this file.
