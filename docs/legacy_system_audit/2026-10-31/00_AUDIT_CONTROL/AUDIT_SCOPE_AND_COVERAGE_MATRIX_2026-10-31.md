# AUDIT SCOPE AND COVERAGE MATRIX

- **Document ID:** AC-02
- **Status:** DRAFT — pending Gate G1 approval
- **Charter:** AUDIT_CHARTER.md (AC-01) · **Method:** AUDIT_METHODOLOGY.md (AC-03) · **Stages:** AUDIT_EXECUTION_SEQUENCE.md (AC-06)

## How to read this matrix

- Every area below is **UNINSPECTED** as of Stage 1. Nothing here has been
  examined yet; "likely evidence sources" are directory/file names observed
  only in a top-level listing of the repository root taken on 2026-10-31.
  Their contents are unknown. Directory names may mislead; Stage 2's
  inventory corrects this matrix where reality differs.
- This matrix sets a **floor, not a ceiling**: it guarantees minimum
  coverage. Anything discovered outside these areas is still in scope
  (Charter §4) and gets added here with a dated note.
- Each area tracks a **Disposition**, maintained as the audit progresses:
  `UNINSPECTED` → `IN PROGRESS (Stage n)` → `AUDITED — findings raised` /
  `AUDITED — none found (limitations recorded)` / `NOT AUDITABLE (reason)`.
- Top-level items observed at the repository root (names only):
  `AGENTS.md, DEPLOYMENT.md, GEMINI.md, PATHS.md, README.md,
  UNPUSHED_LEDGER.md, main.py, run_tests.py, requirements.txt,
  requirements-dev.txt` and directories `alerts/, allocation/, broker/,
  capital/, config/, core/, data/, deploy/, docs/, ops/, ops_dashboard/,
  orders/, regime/, reports/, scripts/, screening/, signals/, sr_detector/,
  strategies/, tasks/, tests/, utils/, v3_chain/` (plus this `audit/`
  workspace).

---

## Area 01 — Source inventory and repository hygiene
- **Examine:** Every file at baseline: type, size, language, apparent role; generated vs hand-written; orphaned/duplicate/backup files; files that look like secrets or data dumps; tracked artifacts that shouldn't be in Git.
- **Key questions:** What does the system actually consist of? Are there stale or duplicate copies of modules? Are secrets or databases committed? What is documented nowhere?
- **Likely evidence:** full file tree; all root-level files; `reports/`, `data/`, `docs/` contents.
- **Touchpoints:** feeds every other area.
- **Output:** `01_SOURCE_INVENTORY/` inventory tables + anomaly list.
- **Prerequisites / done when:** Stage 2; done when every tracked file is listed and categorized, anomalies flagged, limitations recorded.
- **Disposition:** UNINSPECTED

## Area 02 — Architecture, module boundaries and dependencies
- **Examine:** Entry points (`main.py`, `tasks/`, `scripts/`, `deploy/`); import graph; module responsibilities; layering violations; circular dependencies; dead modules; the role of oddly-named trees (`v3_chain/`, `sr_detector/`, `regime/`).
- **Key questions:** What are the real execution entry points? Which modules own which responsibility, and where do responsibilities overlap? Is `v3_chain/` a parallel/legacy implementation living inside the legacy system itself?
- **Likely evidence:** import statements across all packages; `main.py`; `core/`, `utils/`.
- **Touchpoints:** all areas; specially 04 (cross-module) and 14 (duplication).
- **Output:** `02_SYSTEM_ARCHITECTURE/` maps: entry points, module inventory, dependency graph, data-flow narrative.
- **Prerequisites / done when:** Stage 3 after inventory; done when every package is placed on the map or explicitly marked unmapped-with-reason.
- **Disposition:** UNINSPECTED

## Area 03 — Signal generation, screening and decision logic
- **Examine:** `signals/`, `screening/`, `strategies/`, `regime/`, `sr_detector/`: signal intake (Chartink webhooks per AGENTS.md — unverified), scoring, screening pass/reject rules, strategy selection, regime filters, support/resistance logic; thresholds and their config sources; duplicate or contradictory rules between scanner/screener/strategy layers.
- **Key questions:** Is the decision pipeline internally consistent? Are the same conditions computed differently in different places? Do screening rules contradict strategy assumptions? Are thresholds hard-coded, duplicated, or drifting from config?
- **Likely evidence:** the four directories above; `config/`; signal-related entries in `docs/`.
- **Touchpoints:** 04 (orders), 05 (capital), 08 (config), 06 (data model).
- **Output:** `06_TRADING_AND_SIGNAL_LOGIC/` logic audit + findings.
- **Prerequisites / done when:** Stage 4 (module passes) + Stage 6 (domain pass); done when each decision step's rule set is documented with contradictions/duplications listed.
- **Disposition:** UNINSPECTED

## Area 04 — Order lifecycle: entry, modification, SL/target, exits
- **Examine:** `orders/`, relevant parts of `broker/`, `core/`: order placement, order types, fill handling, slippage handling, SL/TGT placement and trailing, modification logic, exit reasons, partial fills, rejects/cancels, retry behaviour, idempotency, race windows between signal→order→fill→protection.
- **Key questions:** Can an entry exist without protection (SL/TGT) under any code path? Are modifications atomic/consistent? What happens on broker errors mid-lifecycle? Are exits guaranteed to release state?
- **Likely evidence:** `orders/`, `broker/`, `core/`; order-related tests in `tests/`.
- **Touchpoints:** 03, 05, 07 (broker API), 06 (persistence of order state).
- **Output:** `07_ORDER_EXECUTION_AND_MANAGEMENT/` lifecycle audit + findings.
- **Prerequisites / done when:** Stage 4 + Stage 6; done when the full lifecycle is traced through code with every failure branch catalogued or marked unknown.
- **Disposition:** UNINSPECTED

## Area 05 — Capital allocation, position sizing, margin and risk controls
- **Examine:** `capital/`, `allocation/`: capital reservation/release, sizing formulas, max-position/exposure limits, daily loss limits, margin assumptions, leak scenarios (reserved-but-never-released), double-reservation, rounding, PAPER vs LIVE differences, the "7 TEMP config values" mentioned in AGENTS.md (unverified).
- **Key questions:** Can capital tracking drift from reality? Are risk limits enforced at one choke point or scattered (and skippable)? Do sizing and reservation use the same numbers? What exactly do the TEMP values relax?
- **Likely evidence:** `capital/`, `allocation/`, `config/`; capital tests.
- **Touchpoints:** 03, 04, 06, 08.
- **Output:** `08_CAPITAL_AND_RISK/` audit + findings.
- **Prerequisites / done when:** Stage 4 + Stage 6; done when every reserve/release path is paired or a leak path is recorded, and every risk limit's enforcement point is identified.
- **Disposition:** UNINSPECTED

## Area 06 — Data model, database, persistence and reconciliation
- **Examine:** `data/`, `core/` persistence code: schema (AGENTS.md claims "schema v24" — unverified), migrations history, write paths, transaction/locking discipline (SQLite), integrity constraints, reconciliation of system P&L vs broker P&L, candle-data integrity, backup/restore assumptions, committed `.db` artifacts.
- **Key questions:** Is there one schema source of truth? Do multiple writers race on SQLite? Are there unconstrained columns where constraints are assumed elsewhere? How is broker↔system reconciliation supposed to work, and can it silently diverge?
- **Likely evidence:** `data/`, schema/migration files wherever they live, DB-touching code across modules.
- **Touchpoints:** nearly all areas; specially 04, 05, 07.
- **Output:** `05_DATA_AND_DATABASE/` audit + findings.
- **Prerequisites / done when:** Stage 4 + Stage 6; done when schema, every write path, and every reconciliation mechanism are documented with risks. No DB writes/queries are executed against any live DB; committed DB artifacts may be opened read-only only with an approved safe plan (AC-03 §6).
- **Disposition:** UNINSPECTED

## Area 07 — Broker/API and external integrations
- **Examine:** `broker/`, `alerts/`: Zerodha KiteConnect usage, auth/token lifecycle, rate-limit handling, error/exception taxonomy, webhook intake (Chartink), Telegram alerting, network failure behaviour, API-response validation, timezone handling of exchange timestamps.
- **Key questions:** What happens when the broker API fails, times out, returns partial data, or returns errors mid-order? Is the token refresh path safe? Are webhook inputs validated/authenticated? Can alerts fail silently?
- **Likely evidence:** `broker/`, `alerts/`, `utils/` (HTTP/session helpers), `config/`.
- **Touchpoints:** 04, 06, 10, 11.
- **Output:** `09_EXTERNAL_INTEGRATIONS/` audit + findings.
- **Prerequisites / done when:** Stage 4 + Stage 6; done when each integration's failure modes are catalogued from code, with unknowns listed. No live API calls will be made.
- **Disposition:** UNINSPECTED

## Area 08 — Configuration consistency
- **Examine:** `config/` and every place configuration is read: duplicated keys, defaults that differ from config files, PAPER/LIVE switches, the 7 TEMP values (identify them, their intended revert, and blast radius), environment-variable usage, config drift between docs and code.
- **Key questions:** Is there a single configuration authority? Which values are silently defaulted in code? Which TEMP relaxations would be dangerous if they leak into LIVE?
- **Likely evidence:** `config/`, grep of config-key usage across modules, root docs.
- **Touchpoints:** all functional areas.
- **Output:** findings land with their functional area + a consolidated config-contradiction table in `04_CROSS_MODULE_AUDIT/`.
- **Prerequisites / done when:** begun in Stage 4, consolidated in Stage 5; done when every config key's readers and defaults are tabulated or counted out of scope with reason.
- **Disposition:** UNINSPECTED

## Area 09 — Cross-module interaction and end-to-end workflows
- **Examine:** The full pipeline signal→screen→capital→order→fill→protection→trail→exit→release→P&L→reporting as it crosses module boundaries; shared state; event ordering; partial-failure states between modules; duplicate responsibilities (e.g., two modules both deciding exits).
- **Key questions:** Where do module contracts disagree (units, timezones, symbols format, status enums)? Which workflows have no single owner? Where can the pipeline stall half-done, and who notices?
- **Likely evidence:** everything mapped in Stage 3; the AGENTS.md "flow observer" event list as a *claimed* pipeline to verify against code.
- **Touchpoints:** by definition, all.
- **Output:** `04_CROSS_MODULE_AUDIT/` workflow traces + findings.
- **Prerequisites / done when:** Stage 5 after module passes; done when each end-to-end workflow is traced with every cross-boundary assumption listed as verified/contradicted/unknown.
- **Disposition:** UNINSPECTED

## Area 10 — Operations: scheduling, monitoring, recovery, deployment
- **Examine:** `ops/`, `ops_dashboard/`, `tasks/`, `deploy/`, `scripts/`, `DEPLOYMENT.md`: cron/scheduler definitions, market-hours logic, heartbeats, log rotation, crash/restart recovery, state recovery after restart mid-trade, deployment procedure vs reality, dashboard correctness.
- **Key questions:** What runs when, and what happens if it doesn't? Is recovery after a mid-session crash defined, or does state silently rot? Do deployment docs match deploy scripts?
- **Likely evidence:** the directories above + any systemd/cron files in `deploy/` or `ops/`.
- **Touchpoints:** 04, 05, 06, 07, 12.
- **Output:** `10_OPERATIONS_AND_INFRASTRUCTURE/` audit + findings.
- **Prerequisites / done when:** Stage 4 + Stage 6; done when the scheduled-job inventory is complete and each recovery scenario is traced or marked undefined.
- **Disposition:** UNINSPECTED

## Area 11 — Security, secrets, permissions, access control
- **Examine:** Secrets handling (.env patterns, tokens in code/config/Git), committed credentials anywhere in tree or history (history: Stage 7), webhook authentication, dashboard exposure/authentication, file permissions assumptions, injection risks at input boundaries (webhook payloads, symbol strings into SQL), dependency risk in `requirements*.txt`.
- **Key questions:** Are any secrets in the repository or its history? Can an unauthenticated party inject signals or reach the dashboard? Is any user input interpolated into SQL/shell?
- **Likely evidence:** whole-tree pattern scans; `config/`; `broker/` auth code; `ops_dashboard/`; requirements files.
- **Touchpoints:** 07, 10, 06.
- **Output:** `11_SECURITY_AND_ACCESS/` audit + findings. Any secret found is referenced by location only, never copied into audit documents.
- **Prerequisites / done when:** Stage 6 (tree) + Stage 7 (history); done when scan scope/patterns are documented with results and limitations.
- **Disposition:** UNINSPECTED

## Area 12 — Testing and quality assurance
- **Examine:** `tests/`, `run_tests.py`, `requirements-dev.txt`: what is tested vs what matters (orders, capital, reconciliation), assertion quality (tests that can't fail, over-mocking that hides integration defects), stale tests targeting removed code, coverage claims vs reality, CI presence/absence.
- **Key questions:** Would these tests catch the defect classes this audit worries about? Which critical paths have zero tests? Are any tests misleading (green but vacuous)?
- **Likely evidence:** `tests/` tree mapped against module inventory. Tests are read, not run, unless Rama approves a specific safe test plan.
- **Touchpoints:** all functional areas.
- **Output:** `12_TESTING_AND_QUALITY/` audit + findings.
- **Prerequisites / done when:** Stage 6 after module map exists; done when a test-to-module coverage table exists with gaps and misleading tests flagged.
- **Disposition:** UNINSPECTED

## Area 13 — Git history, regressions and recurring fixes
- **Examine:** Full commit history (requires approved un-shallowing): recurring fixes to the same code (bug magnets), reverts/re-reverts, abandoned implementations, fixes contradicting earlier fixes, large risky commits, secrets in history, the meaning of "unpushed ledger" checkpoints visible at the baseline tip.
- **Key questions:** Which components keep breaking? Were defects fixed by narrowing symptoms rather than causes? What was abandoned and does its residue remain (ties to Area 02/14)?
- **Likely evidence:** `git log`/`git log -p` after full fetch; `UNPUSHED_LEDGER.md`.
- **Touchpoints:** validates/raises findings in all areas.
- **Output:** `13_GIT_HISTORY_AND_CHANGE_ANALYSIS/` analysis + findings.
- **Prerequisites / done when:** Stage 7; **blocked** until Gate G7-pre (user-approved full-history fetch); done when history is analyzed or the limitation is recorded as final.
- **Disposition:** UNINSPECTED (currently blocked by shallow clone — L1)

## Area 14 — Duplication, contradiction and dead-code sweep
- **Examine:** Cross-cutting sweep distinct from per-module passes: duplicated functions/calculations (e.g., P&L computed in multiple places), copy-paste module variants (candidates: `v3_chain/` vs current modules — hypothesis only), contradictory constants, dead/unreachable code, docs contradicting code, docs contradicting docs (five+ root-level documents exist).
- **Key questions:** How many sources of truth exist for each key calculation (P&L, sizing, prices, time)? What is dead but still imported — or alive but undocumented?
- **Likely evidence:** similarity scans and symbol-definition maps across the tree; the root documents against each other and against code.
- **Touchpoints:** all; results feed 15 (impact) and the register.
- **Output:** consolidated tables in `04_CROSS_MODULE_AUDIT/` + findings.
- **Prerequisites / done when:** Stage 5; done when the duplicate/contradiction tables are complete for the scanned scope, scope stated.
- **Disposition:** UNINSPECTED

## Area 15 — Findings consolidation, impact and final assessment
- **Examine:** The audit's own outputs: de-duplicate findings, cross-link related ones, build impact/dependency analysis (`15_IMPACT_AND_DEPENDENCY_ANALYSIS/`), rank by severity×confidence, compile limitations and unresolved questions.
- **Key questions:** What is the prioritized risk picture? Which findings compound each other? What remains unknown and what would it take to know it?
- **Output:** `14_FINDINGS_REGISTER/` (maintained from Stage 4 onward), `15_IMPACT_AND_DEPENDENCY_ANALYSIS/`, `16_FINAL_REPORT/`.
- **Prerequisites / done when:** Stages 8–9; done at Charter §9 completion definition.
- **Disposition:** UNINSPECTED (register will open at first finding)

---

## Matrix maintenance rules

1. Update an area's Disposition only with evidence of work recorded in the
   stage outputs; cite the output document.
2. New areas discovered mid-audit are appended with a dated note and a
   decision entry in AC-07 — never silently folded into an existing row.
3. This matrix never shrinks. Descoping an area requires Rama's explicit
   decision, recorded in AC-07.
