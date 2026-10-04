# V2 DECISION REGISTER

Project: trading-system-v2 (live-only, single-responsibility, evidence-governed NSE equity system via Zerodha)
Rule: append only. Never edit or delete a row. To change a decision, add a new row that references the old one and mark the old row SUPERSEDED.
Status labels: PROPOSED > DISCUSSION > ACCEPTED > FROZEN > SUPERSEDED (DECIDED = agreed by Rama, recorded here).
Security: no credential values in this file, ever. Locations and classes only.

Row format: ID | Date | Area | Decision | Reason | Status

| ID | Date (IST) | Area | Decision | Reason | Status |
|---|---|---|---|---|---|
| DR-001 | 2026-10-04 | Operating mode | V2 is LIVE-ONLY. No paper mode, no paper/live parity concept. | Legacy already completed a paper phase; no reason to repeat it. | DECIDED |
| DR-002 | 2026-10-04 | Subagents | Cap subagent fan-out at 2 concurrent, unless a specific instruction card raises it. | Avoid API-limit / 5-hour-window pressure from uncontrolled fan-out. | DECIDED |
| DR-003 | 2026-10-04 | Bug workflow | Replace "autonomous bug fixing" with: discuss > design/implement > test > review (review/follow-up only where the bug's nature warrants it). | Autonomous "just fix it" removed the checkpoint where root cause gets agreed before code changes. | DECIDED |
| DR-004 | 2026-10-04 | File design | Each .py file has exactly ONE responsibility (was: up to six). | A file mixing responsibilities cannot be safely deleted/replaced as a unit during upgrades. | DECIDED |
| DR-005 | 2026-10-04 | Script headers | The mandatory header comment block becomes best-effort, not mandatory. | A mandate nobody maintains during edits is worse than an honest optional convention. | DECIDED |
| DR-006 | 2026-10-04 | Event identity | Do not freeze the legacy signal_id > trade_id > order_id chain. End-to-end traceability remains a requirement; the scheme is designed later. | The exact identity scheme will change in V2; freezing it now is premature. | DECIDED |
| DR-007 | 2026-10-04 | Tooling / repo | V2 is built with the Claude Code desktop app (Code tab), local sessions on the Windows PC. A private GitHub repo is offsite backup only, never a deploy path. | Same agent as the VS Code extension; local sessions use local files and credentials; private repo avoids public exposure of strategy/config. | DECIDED |
| DR-008 | 2026-10-04 | Naming | Project folder and private repo are named trading-system-v2. ChatGPT files are stored under their original names, unmodified. Chartink payload screenshots are not stored. | Keeps originals immutable; screenshots add no value and may carry sensitive content. | DECIDED (folder layout part SUPERSEDED by DR-010) |
| DR-009 | 2026-10-04 | Reporting / export | V2 has no .xlsx report generators. All data is downloaded through a GUI-based DB download. | Removes a class of generator scripts; one download path. Format, security and exposed tables are open items (see V2_DESIGN_LOG, L-010/L-011). | DECIDED |
| DR-010 | 2026-10-04 | Repo layout | All non-coded material (discussion, design, governance, inputs, reports, instruction cards) lives only under docs/. Live system files go in other root folders. Supersedes the folder layout in DR-008. | Keeps documentation separate from live system files. | DECIDED |
