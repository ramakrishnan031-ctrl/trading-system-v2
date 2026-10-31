# AUDIT DECISIONS AND ASSUMPTIONS REGISTER

- **Document ID:** AC-07
- **Status:** LIVE document — append-only; entries are never edited, only superseded by later entries.
- **Rule:** only decisions explicitly made by Rama and facts verified in-session are recorded as such. Unknowns are marked UNKNOWN. Nothing here is invented.

## Decisions (user-established)

| ID | Statement | Source / status | Date |
|---|---|---|---|
| D-001 | The sole audit source is `https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot`. | Rama, start-here brief and Stage-1 instruction. ACTIVE | 2026-10-31 |
| D-002 | `trading-system-v2` is strictly out of scope: no access, cloning, inspection, or comparison. | Rama, repeated in every instruction. ACTIVE | 2026-10-31 |
| D-003 | Audit outputs live only in `audit/legacy-system-deep-audit_2026-10-31/`. | Rama. ACTIVE | 2026-10-31 |
| D-004 | Work on session branch `claude/amazing-goodall-1x49v7`; do not use `main`. | Rama, workspace-creation instruction. ACTIVE | 2026-10-31 |
| D-005 | No commit or push without separate explicit approval; workspace currently local/uncommitted (stop-hook requests to commit are overridden by this decision). | Rama. ACTIVE | 2026-10-31 |
| D-006 | One-task exception to the repository AGENTS.md write-allowlist (`reports/`, `docs/`, `/tmp/`): the audit workspace is authorized under `audit/`; AGENTS.md itself must not be edited or weakened. Writes remain confined to the dated workspace. | Rama, workspace-creation instruction §STEP 2. ACTIVE | 2026-10-31 |
| D-007 | The audit is investigation/documentation only: no fixes, refactoring, tuning, redesign, deployment, trading, migrations, or DB writes. | Rama, start-here brief + Stage-1 instruction. ACTIVE | 2026-10-31 |
| D-008 | No execution of application code or tests without a separately approved safe test plan. | Rama, start-here brief §5 / Stage-1 governance §4. ACTIVE | 2026-10-31 |
| D-009 | Stage gates: each stage requires Rama's approval before the next begins; Stage 1 (plan pack) approved to proceed; Stages 2+ await Gate G1. | Rama, Stage-1 instruction. ACTIVE | 2026-10-31 |
| D-010 | Deliverable files: originals stay in the repository workspace; downloadable copies are provided via file cards for Rama to save under `C:\Users\rama\Downloads\claude_code\` (dated subfolder); the cloud session cannot write to the Windows PC and must not claim otherwise. | Rama, file-copy and path-reporting rules. ACTIVE | 2026-10-31 |

## Verified session facts (not decisions)

| ID | Fact | Verified how | Date |
|---|---|---|---|
| V-001 | HEAD = baseline `970aabf3c78d777d9d34549e021f7c8e756c7bdb`; branch `claude/amazing-goodall-1x49v7`; `main`/`origin/main` point at the same commit. | `git rev-parse`, `git for-each-ref` in-session | 2026-10-31 |
| V-002 | Checkout is shallow: 66 reachable commits; shallow cut at `ce29d49a…` and `dec4e70d…`. | `git rev-parse --is-shallow-repository`, `.git/shallow` | 2026-10-31 |
| V-003 | Working tree clean apart from the untracked audit workspace; no tracked file modified; no stash. | `git status`, `git diff --stat` (empty) | 2026-10-31 |
| V-004 | No V2 directory, remote, worktree, or submodule exists in this container; session GitHub scope is limited to the snapshot repo. | filesystem search, `git config`, session scope | 2026-10-31 |
| V-005 | Repository root contains 10 root-level files and 24 directories (names listed in AC-02); contents unexamined. | `ls -1F` (names only) | 2026-10-31 |

## Assumptions and unknowns (to be resolved by evidence, not assumed)

| ID | Item | Status |
|---|---|---|
| U-001 | Whether the snapshot faithfully mirrors the live VM's deployed code/config. Cannot be verified from this session. | UNKNOWN — carried as audit limitation L2 (AC-01 §3) |
| U-002 | Claims made by in-repo documents (AGENTS.md "schema v24", "7 TEMP config values", PAPER mode, Chartink/Zerodha stack, VM paths, AGENTS.md "After June 26" LIVE migration). Treated as unverified claims to check against code, never as facts. | UNVERIFIED — audit subject matter (Charter L5) |
| U-003 | Role of `v3_chain/`, `sr_detector/`, `regime/` and other suggestively named trees. Names observed only. | UNKNOWN — Stage 3 question |
| U-004 | Whether full Git history will be made available (Gate G7-pre approval for un-shallow fetch). | PENDING Rama's decision before Stage 7 |
| U-005 | Whether committed binary databases (if any exist) may be opened read-only from a scratchpad copy. | PENDING — will be raised with a safe plan only if Stage 2 finds such files |
| U-006 | Interpretation of baseline-tip commit messages ("CHECKPOINT… still unpushed", `UNPUSHED_LEDGER.md`) and what they imply about snapshot completeness. | UNKNOWN — Stage 2/7 question |

*Append new entries below with the next free ID. Supersessions reference the superseded ID explicitly.*
