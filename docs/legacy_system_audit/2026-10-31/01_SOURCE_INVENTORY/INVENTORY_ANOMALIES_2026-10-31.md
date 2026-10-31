# INVENTORY ANOMALIES AND LEADS

- **Date:** 2026-10-31 · **Baseline:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb`
- These are **inventory-level leads**, not confirmed system defects. Each
  entry states the observation (fact) and why it merits later investigation.
  Classification uses AC-04 terms only where evidence already suffices;
  most entries are leads feeding Stages 3–7. IDs INV-001 onward; cross-links
  into the findings register will be added when the relevant stage examines
  them.
- The CSV's `BACKUP/ALT-NAMING` flag is a deliberately loose regex
  (it also matches "templates", "final", "fix###"); this document is the
  curated list. CSV flags not curated here were assessed as naming-heuristic
  noise, with one standing exception: the `fix###` test-naming pattern
  (INV-010).

---

**INV-001 — `.env.example` contains non-placeholder-shaped values** · *Confirmed observation; interpretation tentative; potential severity: Critical if real*
- Path: `.env.example` (root, 50 lines, tracked).
- Evidence: 20 filled assignments (Zerodha API key / secret / TOTP for 5
  account-suffixed variable names — `LFL836`, `DR6114`, `D351962`, `ZA004`,
  `ZA005` — plus Telegram bot token, 2 channel IDs, a personal chat ID, and
  `WEBHOOK_SECRET`). Only **4 distinct values** among the 20 (same value
  reused across fields), lengths 12–17, mixed charset; **none matches common
  placeholder patterns** (your-/xxx/replace/changeme/<>/dummy…). Values were
  shape-checked only and are not reproduced anywhere in the audit.
- Why it matters: an "example" env file is expected to hold obvious
  placeholders. Repetition across unrelated fields suggests placeholders;
  the non-placeholder shapes (e.g., a chat-ID-shaped 12-char value) leave
  open that one or more values are real. Real account identifiers appear in
  the variable *names* regardless.
- Later investigation: Rama can confirm offline whether any value is live;
  Stage 7 history scan checks whether a real `.env` or earlier values were
  ever committed. Area 11.

**INV-002 — Exact duplicate root/docs copies of agent charters** · *Confirmed observation (byte-identical blobs)*
- `AGENTS.md` ≡ `docs/AGENTS.md` (4,442 B, same blob hash); `GEMINI.md` ≡
  `docs/GEMINI.md` (12,967 B, same blob). A third, different `AGENTS.md`
  exists at `deploy/tools/claude/AGENTS.md`.
- Why: duplicate sources of operator/agent instructions can drift and then
  contradict (the third copy may already differ — unexamined). Area 14;
  docs-vs-docs contradiction check.

**INV-003 — V2 design material stored inside the legacy repository, including an exact duplicate** · *Confirmed observation*
- `docs/v2_design_spec.md` ≡ `docs/web_claude/00_source_documents/v2_design_spec.md`
  (52,144 B, identical blobs); plus `docs/web_claude/03_audit_responses/trading_system_v2_audit_report.md`
  (175,224 B), `system_audit_v2_21042026.md`, and
  `docs/web_claude/00_source_documents/locked_decisions.yaml` duplicated at
  `docs/locked_decisions.yaml` (not yet compared — may differ).
- Why: these in-repo files are in scope (they are part of this snapshot),
  but the audit's V2-isolation rule means we analyze them only as *legacy-repo
  documentation*, never as a window into the V2 repository. Flag for Rama:
  confirm this treatment. Also a duplication/contradiction lead (Area 14).

**INV-004 — `v3_chain/` package: an in-repo parallel implementation lineage ("V3") with its own docs, scripts and tests** · *Confirmed observation of presence; role unverified*
- Paths: `v3_chain/` (9 Python files incl. `runner.py`, `pb01_entry.py`,
  `score.py`, `forward_shadow.py`); `docs/v3/` (9 planning/gap-map docs);
  `scripts/v3_hardgate_parity_recompute.py`, `scripts/v3_shadow_soak_report.py`;
  13 `tests/unit/test_v3_*`/`test_sr_v2_*` files reference v3/sr-v2 naming.
- Why: a second decision pipeline living beside the primary one is a prime
  candidate for duplicated/contradictory logic and ambiguous ownership
  (Areas 02, 03, 14). Its docstring self-describes as "V3 chain"; actual
  integration unverified. Also note `docs/audit/pb01_and_v3_gate_scoping_2026-07-24.md`.

**INV-005 — Reference-data CSVs duplicated between config and test fixtures** · *Confirmed observation (names; content not yet compared)*
- `config/reference_data/**` (18 CSVs incl. `security_master_file.csv`) vs
  `tests/fixtures/reference_data/**` (5 CSVs with 4 overlapping names:
  `fno_symbols.csv`, `nifty_bank.csv`, `nifty_it.csv`, `nifty_private_bank.csv`).
- Why: fixture copies that drift from real reference data make tests pass
  against stale universes (Area 12); content diff deferred to Stage 6.

**INV-006 — Oversized documentation artifacts** · *Confirmed observation*
- `docs/SYSTEM_MAP.md` 705,794 B (largest file in repo);
  `docs/audit/integrity_audit_2026.md` 368,033 B;
  `docs/MASTER_PENDING_01-Aug-2026.md` 345,139 B;
  `ops_dashboard/frontend/static/style.css` 438,424 B;
  `ops_dashboard/frontend/templates/login.html` 151,863 B (a login page).
- Why: machine-maintained or ever-growing docs drift from code (Area 14);
  a 150 KB login template is a lead for embedded assets or generated code in
  the dashboard (Areas 10/11). Content review deferred.

**INV-007 — 44 committed screenshots/binaries under `docs/audit/`** · *Confirmed observation*
- 34 PNG + 2 webp (e.g., `docs/audit/approval_final_19aug/S01…S22*.png`,
  ~5.9 MB total), 3 `.docx` under `docs/web_claude/06_live_operations/`.
- Why: binaries are unauditable as text and may contain sensitive dashboard
  data (account numbers, P&L, tokens in UI). Metadata-only so far; a
  screenshot-content review is an optional later step for Rama to approve
  (they render the GUI with real-looking operational data).

**INV-008 — Custom "copy-protection" security tooling in deploy** · *Confirmed observation of presence; purpose unverified*
- `deploy/security/bin/copy-guard`, `deploy/security/bin/request-copy`
  (extensionless executables), `deploy/security/install_copy_protection.sh`,
  plus `scripts/copy_gate.py`, `scripts/request_copy.py`,
  `tests/unit/test_copy_gate.py`, and polkit-style `*.rules` file.
- Why: home-grown access-control layers are a classic source of both
  security gaps and operational lockouts (Areas 10/11). Also unusual:
  Windows-side `scripts/copy_token_to_vm.bat` implies tokens are copied
  from a Windows PC to the VM — token-handling workflow to examine.

**INV-009 — Token lifecycle implemented in at least four places (names)** · *Tentative interpretation from names + docstrings only*
- `broker/token_monitor.py`, `scripts/auto_refresh_token.py`,
  `deploy/token_watcher.sh` + `deploy/systemd/token-watcher.service`,
  `scripts/copy_token_to_vm.bat`; tests reference token invalidation/relatch
  (`test_fix062_token_invalidation.py`, `test_h11…`, `test_h13…`,
  `test_fix128_token_monitor.py`, `test_fix135_auto_token.py`).
- Why: multiple cooperating token mechanisms is a duplication/race lead
  (Areas 07/09/14). Note: these files *handle* secrets; no secret values
  observed in the inventory pass.

**INV-010 — `fix###` naming shows ≥190 numbered fix campaigns; tests pin specific fixes** · *Confirmed observation of naming; interpretation tentative*
- ~75 test files named `test_fixNNN_*.py` with NNN observed from 061 to 190
  (non-contiguous), e.g. `tests/unit/test_fix190_exit_safety.py`,
  `tests/integration/test_fix190_incident_replay.py`; plus
  `docs/archive/CRON_FIX_2026_05_18.md`, `docs/web_claude/05_deployment/fixes/fix1…fix4`.
- Why: the numbering implies a long sequence of production incidents/fixes —
  the primary input for Stage 7 regression analysis (which fixes recur,
  which were symptom-level). Also suggests test suite is organized around
  incidents rather than behaviours (Area 12).

**INV-011 — TEMP-config apparatus is reified in the repo** · *Confirmed observation of presence*
- `docs/temp_config_tracker.md` (2,094 B), `scripts/revert_temp_config.py`,
  `tests/unit/test_revert_temp_config.py`, `tests/crash_test/revert_temp_configs.sh`.
- Why: AGENTS.md claims "7 TEMP config values active — will be reverted".
  Dedicated revert tooling confirms temporary risk-relaxations are a managed
  practice; which values, their blast radius, and whether tracker/code agree
  is Area 08's question (U-002 in AC-07).

**INV-012 — Five empty `__init__.py` files in core trading packages** · *Confirmed observation; benign-looking*
- `capital/__init__.py`, `data/__init__.py`, `orders/__init__.py`,
  `reports/__init__.py`, `signals/__init__.py` — 0 bytes (normal Python
  practice; other packages have non-empty inits, e.g. `v3_chain/__init__.py`
  645 B). Recorded for completeness; low priority (inconsistency only).

**INV-013 — Duplicate basenames across packages (drift candidates)** · *Confirmed observation (names only)*
- `models.py` ×4 (`allocation/`, `regime/`, `sr_detector/`, `v3_chain/`);
  `runner.py` ×3; `engine.py`, `freshness.py`, `capacity.py`,
  `operations.py` ×2 each — notably `ops_dashboard/backend/api/capacity.py`
  vs `.../services/capacity.py` and `ops/control_tower/freshness.py` vs
  `ops_dashboard/backend/services/freshness.py`.
- Why: same-name modules in sibling layers often share copied logic
  (Area 14 near-duplicate scan will compare bodies).

**INV-014 — Second dashboard dependency manifest** · *Confirmed observation*
- `ops_dashboard/backend/requirements.txt` beside root `requirements.txt`
  and `requirements-dev.txt`; also `docs/web_claude/05_deployment/fixes/fix4_requirements_split.txt`
  documents a historical "requirements split".
- Why: split manifests can pin conflicting versions (Areas 10/11 dependency
  review).

**INV-015 — Operational report artifacts tracked in `reports/`** · *Confirmed observation*
- 10 files incl. `reports/crash_test/day6_ct159_final.md` — runtime-style
  outputs committed to source control (the live system reportedly writes to
  `reports/`).
- Why: tracked runtime-output directories blur source vs artifact and
  invite accidental commits of operational data (Area 01/10). Contents
  unexamined.

**INV-016 — Root `GEMINI.md` + `deploy/tools/claude/` + `docs/web_claude/`: three AI-agent instruction surfaces** · *Confirmed observation of presence*
- Root/docs `GEMINI.md` (duplicated, INV-002), `deploy/tools/claude/AGENTS.md`,
  and an extensive `docs/web_claude/` instruction tree for external AI
  workflows.
- Why: multiple agent charters with write rules and operational authority
  statements may contradict each other and the repo's actual layout (this
  audit already handles one AGENTS.md conflict); docs-vs-docs comparison in
  Area 14.

---

Count: 16 curated leads. None is asserted as a system defect at inventory
level; INV-001 carries a potential-Critical marker pending verification.
