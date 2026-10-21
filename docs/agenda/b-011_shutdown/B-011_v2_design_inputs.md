# B-011 SHUTDOWN: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-20. It stays PROPOSED / OPEN after the B-011 manual's freeze (DR-048, 2026-10-20); only this sentence changed at the freeze. The owner cells of rows 3 and 4 corrected on 2026-10-21 at the Section B close (DR-052, C-1 item 2), owner text only. Nothing here is a V2 decision. Items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (B-011_manual.md). CLAUDE CONSIDERATION = a question for the owner, not a requirement.

| # | LEGACY finding | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | Self-exit checks positions and a running flatten; an external stop checks neither and waits at most 15 s (sections 3, 7) | Whether any stop request, internal or external, passes through the same exposure check, and what a stop does when exposure remains | I topics, L topics |
| 2 | Cleanup returns exit 0 whatever failed; a hang blocks later steps (sections 5, 11 E) | Whether shutdown has a total time budget and reports a partial cleanup as a failure | O topics |
| 3 | The SHUTDOWN record has no reason or mode; a self-exit, an operator stop and a failure look alike (section 5.2; 17-Jul records) | Whether every stop records why it happened, so monitoring can tell a planned stop from an unplanned one | O topics, VP-64, VP-65 (M-006, L-008, L-009) |
| 4 | The service end time is absolute and is not moved by special sessions (section 8) | Whether session times come from one calendar source that every time-based control reads | VP-16 (M-002), VP-23 (K, I-010) |
| 5 | Exposure gate reads the local DB, not the broker book (section 3.3) | Whether "safe to stop" is decided on broker truth | G topics, L-008 |

Status and boundary: PROPOSED / OPEN. No VP row is opened or changed by this file.
