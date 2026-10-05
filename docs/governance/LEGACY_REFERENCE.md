# LEGACY REFERENCE (pointer file)

Date: 2026-10-05
Purpose: say where the legacy system lives. No legacy code is copied into V2. Legacy is evidence, never an inherited requirement.
Security: locations and classes only. No credential values in this file.

## Locations

| ID | Location | What it is | Status / caveat |
|---|---|---|---|
| L1 | GitHub: ramakrishnan031-ctrl/trading-system-prod-snapshot (public) | Production snapshot, branch main, 1,537 commits as of 2026-10-04. Baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (per docs/inputs/legacy_baseline/CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt). | Rama treats it as the full replica of the production VM. NOT verified file by file: the legacy record says the VM was never read, and the deployed VM tree has no .git. |
| L2 | Local: D:\Projects\_legacy_ref\trading-system-prod-snapshot | Shallow clone (depth 1) of L1, no old history. | PRIMARY reading source for Claude Code. Created 2026-10-05; HEAD verified as 970aabf3c78d777d9d34549e021f7c8e756c7bdb. |
| L3 | GitHub: ramakrishnan031-ctrl/trading-system-sbx-snapshot | Testing-VM files (commit ade89ff as of 2026-09-18). | Separate system from production. |
| L4 | GitHub: ramakrishnan031-ctrl/trading-system-records | Legacy audit records and memory files (commit b67e5d7 as of 2026-09-18). | Redactions in it are cosmetic. |
| L5 | Local: D:\Projects\_restore | Legacy system restore. Two subfolders: production and trading-system-full. | Relationship to the VM not established. File-name scan on 2026-10-05 found no .env, token/session JSON or gui_config.local.yaml (only .env.example and token-related code, tests and docs). Content scan pending; not given to Claude Code until it is clean. |
| L6 | Local: D:\Projects\trading-system | Legacy main worktree. | The legacy storage record says it holds credential-bearing files. Not given to Claude Code unless Rama approves, per task. |
| L7 | Local: D:\Projects\_preservation, _snapshots, trading-system-evidence, wt-* | Legacy evidence and worktrees. | Not referenced by V2. |

## Rules for Claude Code

- Read-only. Never write to any legacy location.
- Default source is L2. L5 only after the scan is clean. L6 only with Rama's per-task approval.
- Never open: .env and .env.* files, token/session files, gui_config.local.yaml, docs/web_claude/, *.db files, logs.
- Do not run git history commands (git log -p, git show) on legacy repos unless an instruction card says so. A credential is recorded as committed in the legacy history.
- Every legacy fact in a report names its file and commit. Anything not read from L1 to L6 is marked unverified.
- No legacy code is copied into V2 without a V2_DECISION_REGISTER.md row.

## Related
- docs/inputs/legacy_baseline/WHERE_EVERYTHING_IS_STORED_18-Sep-2026.txt
- docs/inputs/legacy_baseline/CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt
