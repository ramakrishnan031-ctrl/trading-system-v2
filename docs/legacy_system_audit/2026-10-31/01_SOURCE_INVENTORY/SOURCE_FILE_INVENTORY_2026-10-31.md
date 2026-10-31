# SOURCE FILE INVENTORY — Method and Summary

- **Topic:** 01 — Complete source inventory (Stage 2 / Area 01 of AC-02)
- **Date:** 2026-10-31 (IST)
- **Baseline:** commit `970aabf3c78d777d9d34549e021f7c8e756c7bdb`, branch `claude/amazing-goodall-1x49v7`
- **Companion files:** SOURCE_FILE_INVENTORY.csv (machine-readable, one row per tracked file), DIRECTORY_STRUCTURE.md, INVENTORY_ANOMALIES.md, INVENTORY_LIMITATIONS_AND_OPEN_QUESTIONS.md, INVENTORY_VALIDATION.md

## 1. Scope and counting rules

- **Universe:** every file tracked by Git at the baseline commit, enumerated
  with `git ls-tree -r -l HEAD` (not a filesystem walk), so the inventory is
  anchored to the commit, not to checkout state. **1,438 tracked files.**
- **Excluded from the legacy inventory:** the audit workspace
  `audit/legacy-system-deep-audit_2026-10-31/` (untracked, created by this
  audit — it is audit material, not legacy source). It is the only untracked
  content in the checkout (26 files at inventory time).
- **Ignored files:** `git status --ignored --porcelain` reports **0** ignored
  items present in this checkout (method: Git's own ignore enumeration; no
  broad filesystem traversal performed).
- **Submodules / symlinks / LFS:** none. All 1,438 entries are regular blobs
  (1,432 × mode `100644`, 6 × executable `100755`); no `120000` (symlink) or
  `160000` (submodule) modes; no `.gitmodules`; no LFS patterns in
  `.gitattributes` and no LFS pointer files found.
- **Total tracked content:** 32,085,119 bytes (~30.6 MiB).

## 2. Method

1. Enumerate tracked files with sizes/modes/blob hashes from `git ls-tree -r -l HEAD`.
2. Classify each file by extension + path rules into one of ten categories
   (below); detect language/format from extension.
3. Read the first 4 KB of every text file to extract an "apparent role"
   (Python module docstring / first comment; first Markdown heading; first
   comment of configs/scripts). Binary files (by extension or NUL-byte
   sniff) and files >2 MB: metadata only. The `inspection` column records
   which treatment each file received. **This head-reading is for
   identification only — it is not a functional code review.**
4. Flag anomaly leads per file (empty, >500 KB, secret-shaped name,
   backup/alternate naming heuristic, duplicate content by identical blob
   hash, executable, binary). Heuristic flags are *leads*; the curated,
   evidence-checked list is INVENTORY_ANOMALIES.md.
5. Exact-duplicate detection used Git blob hashes: identical hash ⇒ byte-identical content (strong evidence, no heuristics).
6. Secret handling: no candidate secret value was printed or copied; checks
   on `.env.example` measured only length/charset/distinctness (INV-001).

## 3. Composition by category (CSV column `category`)

| Category | Files | Notes |
|---|---|---|
| test | 491 | `tests/` tree + `conftest.py`/`test_*` elsewhere |
| documentation | 464 | `docs/` (474 files incl. other types), root docs, `reports/`, 3 binary `.docx` |
| application source | 197 | Python outside tests/scripts/docs |
| script/tooling | 89 | `scripts/` + shell/batch/PowerShell |
| config | 45 | YAML/JSON/env-template/SQL/cron/conf + `config/` CSVs counted under data |
| web asset | 43 | `ops_dashboard/` HTML templates, JS, CSS |
| image asset | 36 | PNG/WebP — almost all are audit screenshots under `docs/audit/` |
| repo plumbing | 32 | `.gitkeep` (29), `.gitignore` (2), `.gitattributes` |
| deployment/operations | 21 | `deploy/` incl. systemd units/timer, hooks, logrotate |
| data (CSV) | 20 | reference data under `config/reference_data/` + test fixtures |

(Dependency manifests — `requirements.txt`, `requirements-dev.txt`,
`ops_dashboard/backend/requirements.txt` — are classified under their
extension rule; see CSV. Category totals sum to 1,438.)

## 4. Composition by language/format (top)

Python 734 · Markdown 387 · plain text 74 · YAML 65 · HTML 39 · PNG 34 ·
`.gitkeep` 29 · CSV 25 · shell 9 · systemd unit 7 · extensionless 6
(deploy hooks, security bins, logrotate) · JSON 4 · batch 4 · JS 3 ·
docx 3 · webp 2 · SQL 2 · others 1 each (timer, rules, ps1, cron, conf,
local, example, css, gitignore×2, gitattributes).

## 5. Composition by top-level directory (file counts)

`docs/` 474 · `tests/` 442 · `ops_dashboard/` 183 · `scripts/` 88 ·
`config/` 51 · `deploy/` 29 · `core/` 29 · `orders/` 21 · `ops/` 14 ·
`broker/` 13 · root 13 · `sr_detector/` 12 · `reports/` 10 · `capital/` 10 ·
`v3_chain/` 9 · `screening/` 7 · `alerts/` 6 · `utils/` 5 · `strategies/` 5 ·
`allocation/` 5 · `signals/` 4 · `regime/` 4 · `data/` 3 · `tasks/` 1.

Observation (fact, not judgment): documentation + tests account for 916 of
1,438 files (64%); application source proper is 197 files.

## 6. Limitations

See INVENTORY_LIMITATIONS_AND_OPEN_QUESTIONS.md for the full list. Headlines:
role extraction read only file heads (first 4 KB); binaries (34 PNG, 2 webp,
3 docx) and the oversized `docs/SYSTEM_MAP.md` (705,794 B) were not content-
reviewed; categories rest on path/extension conventions and will be refined
by the architecture stage; "apparent role" strings are self-descriptions
from the files and are **unverified claims**, not audit conclusions.
