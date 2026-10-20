# B-010 END-OF-DAY OPERATION: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-20. It stays PROPOSED / OPEN after the B-010 manual's freeze (DR-047, 2026-10-20); only this sentence changed at the freeze. Nothing here is a V2 decision. Items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (B-010_manual.md). CLAUDE CONSIDERATION = a question for the owner, not a requirement.

| # | LEGACY finding | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | Two square-off routines (MIS passes and the 15:17 routine) with different eligibility, ownership rules and order types, and no link between them (sections 5, 6, D-1 to D-3) | Whether V2 has one end-of-day owner with one eligibility rule, and how a second safety pass would know what the first did | G topics, I topics |
| 2 | "Completed" meant orders placed, not a flat broker book (section 6.5) | Whether end of day is complete only when the broker confirms a flat intraday book, and what happens when it does not | G-015, L-008 |
| 3 | A status value written by one module and queried under another name by another (D-2) | Whether V2 keeps one named set of trade states, enforced in one place, with tests that go through the real transitions | O-004, N-001 |
| 4 | MARKET exits sent without the band the broker requires (D-3) | Whether every exit path uses one order-construction function that the broker's rules are tested against | G topics, N-005 |
| 5 | The daily P&L reset was written before the day's last exits filled (section 7.3) | When the day's accounting closes relative to the last fill | F topics, L-008 |

Status and boundary: PROPOSED / OPEN. No VP row is opened or changed by this file.
