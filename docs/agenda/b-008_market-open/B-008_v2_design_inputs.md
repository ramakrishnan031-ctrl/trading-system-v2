# B-008 MARKET-OPEN OPERATION: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-20; the owner cells of rows 1 and 3 corrected on 2026-10-21 at the Section B close (DR-052, C-1 item 2), owner text only; the owner cell of row 2 corrected on 2026-10-25 at the Section C close (DR-066, S-3), owner text only. Nothing here is a V2 decision; items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (B-008_manual.md); CLAUDE CONSIDERATION = a question for the owner, not a requirement.

| # | LEGACY finding | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | One global entry floor (10:00) over per-strategy windows (09:25), labelled temporary; the strategy values were inert (section 2) | Whether V2 has one source for each strategy's effective window, shown to the operator as effective, not declared | C-010, configuration design (build phase), VP-57 |
| 2 | On the inspected intraday webhook path, alerts before the global entry window were refused with HTTP 403 before parsing; no signal row was stored and nothing was queued, but a webhook audit row was written. Other routes were not fully traced (section 3) | Whether an early alert should be refused, recorded as refused, or held, and how the refusal is visible later | VP-24, VP-59; whole-design review |
| 3 | The briefing's strategy table omitted one of 16 strategies (section 4) | Whether every operator report derives its strategy list from the same registry as the service | L-005, C-010 |
| 4 | The window was checked at the door and again in the pipeline (section 3) | Which layer owns the window check, and whether later checks are kept as defence in depth | C-008, D-002 |

Status and boundary: PROPOSED / OPEN; no VP row is opened or changed by this file.
