# INVENTORY LIMITATIONS AND OPEN QUESTIONS

- **Date:** 2026-10-31 · **Baseline:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb`
- Open questions here use inventory-local IDs (IOQ-nn); those still open at
  register time will be promoted to global OQ-nnn IDs.

## Limitations of this inventory

| # | Limitation | Detail / what would resolve it |
|---|---|---|
| L-i1 | **Head-only content inspection.** "Apparent role" came from the first 4 KB of each text file (docstring/heading/first comment). Files may do more or other than their self-description. | Stages 3–6 full reads of application source. |
| L-i2 | **Binaries not content-reviewed.** 34 PNG, 2 webp, 3 docx (and any images' embedded data) were metadata-only. Screenshots may show sensitive operational data. | Optional screenshot/docx review if Rama approves (INV-007). |
| L-i3 | **No databases opened.** No `.db`/SQLite files are tracked at baseline (verified by extension search over `git ls-files`), so nothing was skipped — but any DB referenced by code lives outside this snapshot and stays unexamined (Charter L2/L4). | VM-side evidence via Rama, out of band. |
| L-i4 | **Category rules are conventions.** Classification used path/extension rules (e.g., all `tests/**` = test, `docs/**/*.py` = documentation code sample). Misfiled content (production code living under `docs/` or `tests/`) would be miscategorized until read. | Stage 3 import graph shows what the app actually uses. |
| L-i5 | **Duplicate detection = exact blobs only.** Only byte-identical files were marked DUPLICATE-CONTENT. Near-duplicates (edited copies) are not yet detected. | Stage 5 near-duplicate scan (AC-03 §7). |
| L-i6 | **Anomaly regexes are heuristics.** SECRET-SHAPED-NAME / BACKUP-ALT flags over- and under-fire by design; the curated truth is INVENTORY_ANOMALIES.md. | — (by design) |
| L-i7 | **Ignored-file enumeration is checkout-relative.** `git status --ignored` reported 0 ignored items *in this fresh clone*; the production VM checkout may contain ignored runtime files (.env, DBs, logs) that simply don't exist here. | VM-side listing via Rama, if wanted. |
| L-i8 | **Secret-value assessment limited by the no-copy rule.** For `.env.example` only length/charset/distinctness/pattern-verdicts were computed (INV-001); real-vs-placeholder cannot be settled from shape alone. | Rama checks values offline against live credentials. |
| L-i9 | **Shallow history.** Inventory is of the baseline tree; nothing here says when/why files appeared. 66-commit window until Gate G7-pre. | Approved un-shallow fetch (Stage 7). |
| L-i10 | **docs/SYSTEM_MAP.md (705 KB) and other oversized docs not read.** Their classification is from headers only. | Targeted reads in Stages 3/6. |

## Open questions from inventory

| ID | Question | Why it matters | Likely evidence |
|---|---|---|---|
| IOQ-01 | Are any of the 4 distinct `.env.example` values real credentials (esp. the chat-ID-shaped one)? | Secret exposure in a public-ish repo = potential Critical (INV-001). | Rama offline check; Stage 7 history of `.env*`. |
| IOQ-02 | Is `v3_chain/` live in the production flow, gated shadow-mode, or dormant? | Determines weight of duplicated-pipeline risk (INV-004). | Stage 3 import/entry-point graph; `config/` gates; `docs/v3/`. |
| IOQ-03 | Do the three `AGENTS.md` variants and `GEMINI.md` agree with each other and with reality (paths, DB schema version, write rules)? | Contradictory operator instructions are an operational hazard (INV-002/016). | Stage 5 docs-vs-docs/docs-vs-code comparison. |
| IOQ-04 | Have `tests/fixtures/reference_data/*` drifted from `config/reference_data/*`? | Tests may validate against a stale universe (INV-005). | Content diff (safe, read-only) in Stage 6/testing. |
| IOQ-05 | What exactly are the "7 TEMP config values" now, and does `docs/temp_config_tracker.md` match `scripts/revert_temp_config.py` and the live YAMLs? | Risk-relaxations leaking into LIVE (INV-011; U-002). | Stage 4/6 config audit. |
| IOQ-06 | Why are runtime-style outputs tracked under `reports/` (and what writes there)? | Source/artifact blurring; accidental data commits (INV-015). | Stage 3 write-path trace; Stage 7 history of `reports/`. |
| IOQ-07 | What do `fix061…fix190` map to — is there an index of incidents, and which fixes recur on the same component? | Core Stage 7 regression question (INV-010). | Full Git history; `docs/` fix notes. |
| IOQ-08 | Do `UNPUSHED_LEDGER.md` and the baseline-tip "CHECKPOINT … still unpushed" commits imply the snapshot lags the real production state? | Audit may be examining a stale production picture (U-006). | Rama confirms snapshot provenance; Stage 7 history. |
| IOQ-09 | Is the dashboard's 151 KB `login.html` hand-written, generated, or vendored (and what is embedded in it)? | Possible embedded assets/credentials/third-party code (INV-006). | Targeted read in Stage 6 (operations/security). |
| IOQ-10 | What do the copy-guard / copy-gate executables install and enforce on the VM? | Home-grown security layer: gap or lockout risk (INV-008). | Stage 6 security read of `deploy/security/*`, `scripts/copy_gate.py`. |

No question above blocks Topic 02; IOQ-01 is flagged to Rama now because
its resolution is fastest done by him offline.

---

## POST-REVIEW UPDATES — 2026-10-31 (IST)

- **IOQ-01 — largely resolved for the current file:** values are FILL-marked
  placeholders (INVENTORY_ANOMALIES E-01). Remaining question moves to
  Stage 7: did any earlier commit of this **public** repository contain
  real values?
- **IOQ-04 — reframed:** fixtures are small stubs, not copies (E-06); the
  open question is test realism, not drift.
- **IOQ-11 (new) — naming overlap.** The snapshot calls itself "Trading
  System v2" (README.md, main.py docstring, design spec). What is its
  relationship to the separate, out-of-scope `trading-system-v2`
  repository? It affects how in-repo "v2"/"v3" documents are interpreted.
  Evidence: Rama's knowledge only — **must not** be resolved by accessing V2.
- **L-i8 (updated):** the original placeholder heuristic missed the "FILL"
  marker; secret checks now also test marker words and value sharing across
  credential types (values still never printed).
- **L-i11 (new):** known CSV cell errors — 5 category cells, 6 language
  cells, 1 rules-file label — are corrected in SOURCE_FILE_INVENTORY.md
  C-2/C-3; the CSV was not regenerated, by design.
