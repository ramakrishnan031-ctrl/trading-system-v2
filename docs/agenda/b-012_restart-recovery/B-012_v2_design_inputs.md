# B-012 RESTART AND RECOVERY: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-20. It stays PROPOSED / OPEN after the B-012 manual's freeze (DR-049, 2026-10-20); only this sentence changed at the freeze. The owner cell of row 4 corrected on 2026-10-21 at the Section B close (DR-052, C-1 item 2), owner text only. Nothing here is a V2 decision. Items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (B-012_manual.md). CLAUDE CONSIDERATION = a question for the owner, not a requirement.

| # | LEGACY finding | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | Two supervisors (systemd and the token watcher) decided restarts independently, with different exit-code rules and caps (sections 3, 4) | Whether V2 has one owner of start and restart decisions, with one written exit-code contract | VP-66, VP-67, O topics |
| 2 | A restart the same day after any emergency kill stops at HALT and waits for an operator (section 6.2) | How a restart with open positions and a same-day kill is handled, and who is told | VP-60, VP-61, L topics |
| 3 | Backups on the same disk; one manual off-box copy on record (section 8.1) | Where backups live, how often they leave the host, and what proves they can be restored | Operations topics |
| 4 | The restore drill records success even when it fails (section 8.3) | Whether every job reports its result separately from "it ran" | M-002, L-003, N-010 |
| 5 | Orphan broker positions are not adopted at start (section 6.3; B-005) | What a restart does with a broker position the system has no record of | VP-60, G topics |

Status and boundary: PROPOSED / OPEN. No VP row is opened or changed by this file.
