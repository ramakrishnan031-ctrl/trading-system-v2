# AUDIT EXECUTION SEQUENCE — STAGE GATES

- **Document ID:** AC-06
- **Status:** DRAFT — pending Gate G1 approval
- **Rule:** a stage starts only when the previous stage's exit criteria are met **and Rama has explicitly approved proceeding**. Early-noticed defects are registered immediately regardless of stage (AC-03 §1); only systematic coverage is staged.

---

## Stage 0 — Session, repository, baseline and workspace verification  ✅ performed
- **Inputs:** fresh session; start-here brief.
- **Activities performed (verified facts only):** confirmed working directory
  `/home/user/trading-system-prod-snapshot`; sole remote
  `origin = https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot`;
  branch `claude/amazing-goodall-1x49v7`; HEAD = baseline
  `970aabf3c78d777d9d34549e021f7c8e756c7bdb`; clean tree before workspace
  creation; shallow checkout (66 commits); no V2 directory/remote/worktree
  in the container; workspace `audit/legacy-system-deep-audit_2026-10-31/`
  created with README.txt + 17 `.gitkeep` files, uncommitted.
- **Outputs:** verification reports in conversation; workspace on disk.
- **Exit criteria:** all checks pass — **met 2026-10-31**.
- **Approval:** Rama approved workspace creation and then Stage 1 — given.

## Stage 1 — Audit charter, scope, methodology, evidence standards  ◀ THIS TASK
- **Inputs:** Stage 0 facts; user-approved decisions.
- **Activities:** write AC-01…AC-08 in `00_AUDIT_CONTROL/`; cross-check them
  for contradictions; no source inspection beyond a root-level name listing.
- **Outputs:** the eight AC documents.
- **Exit criteria:** documents complete, internally consistent, free of
  invented assumptions; validation checklist reported to Rama.
- **Approval to advance → Gate G1:** Rama reviews and approves the plan
  pack. **Stage 2 must not start before G1.**

## Stage 2 — Complete source / file / configuration / documentation inventory
- **Inputs:** approved plan pack.
- **Activities:** enumerate every tracked file (path, size, type, language,
  apparent role, oddities); classify into functional groups; flag anomalies
  (duplicates, backups, committed data/DBs, secret-shaped files, orphans);
  extract checkable claims from the six root documents; update AC-02
  dispositions and "likely evidence" fields to reality.
- **Outputs:** `01_SOURCE_INVENTORY/` tables + anomaly list; corrections to
  AC-02; first register entries if defects are already evident.
- **Exit criteria:** every tracked file appears in the inventory exactly
  once with a category; anomalies and limitations recorded.
- **Approval → Gate G2.**

## Stage 3 — Architecture, entry points, data flows, integrations, dependencies
- **Inputs:** Stage 2 inventory.
- **Activities:** identify runnable entry points; build import/dependency
  map; assign responsibilities per module; draft end-to-end data-flow
  narrative (signal→…→report) as evidenced by code; locate integration
  seams (broker, webhook, Telegram, DB, filesystem, scheduler); flag
  layering violations, cycles, ambiguous ownership, suspected parallel
  implementations (`v3_chain/` hypothesis) for Stage 4/5 scrutiny.
- **Outputs:** `02_SYSTEM_ARCHITECTURE/` maps and narratives.
- **Exit criteria:** every package placed on the map or explicitly marked
  unmapped-with-reason; workflow list for Stage 5 fixed.
- **Approval → Gate G3.**

## Stage 4 — Module-by-module audit
- **Inputs:** architecture map; module list from Stage 3.
- **Activities:** per-module static review per AC-03 §3 (role/contract/
  failure/consistency passes); one audit note per module in
  `03_MODULE_AUDITS/`; findings registered as discovered; config
  key→reader→default table built as modules are read (Area 08).
- **Outputs:** module audit notes; growing findings register.
- **Exit criteria:** every module from the Stage 3 map has a note with
  scope, method, findings, limitations — or an explicit skip-with-reason
  approved by Rama.
- **Approval → Gate G4.**

## Stage 5 — Cross-module and end-to-end workflow audit
- **Inputs:** module notes; workflow list from Stage 3.
- **Activities:** trace each workflow across boundaries per AC-03 §5 with
  handoff-contract comparison; duplication/contradiction sweep per AC-03 §7
  (symbol map, calculation census, code↔config↔docs comparison);
  consolidate the config-contradiction table.
- **Outputs:** `04_CROSS_MODULE_AUDIT/` traces and tables; register growth.
- **Exit criteria:** all listed workflows traced; sweep scope documented;
  every cross-boundary assumption marked verified/contradicted/unknown.
- **Approval → Gate G5.**

## Stage 6 — Focused domain deep-dives
- **Inputs:** everything above.
- **Activities & outputs:** domain audits building on (not repeating)
  Stages 4–5, written to their folders:
  data/database → `05_DATA_AND_DATABASE/`; trading/signal logic →
  `06_TRADING_AND_SIGNAL_LOGIC/`; order lifecycle →
  `07_ORDER_EXECUTION_AND_MANAGEMENT/`; capital/risk →
  `08_CAPITAL_AND_RISK/`; integrations → `09_EXTERNAL_INTEGRATIONS/`;
  operations → `10_OPERATIONS_AND_INFRASTRUCTURE/`; security (tree-level)
  → `11_SECURITY_AND_ACCESS/`; testing → `12_TESTING_AND_QUALITY/`.
- **Exit criteria:** each domain document answers its Area's key questions
  (AC-02) or records why it cannot; dispositions updated.
- **Approval → Gate G6.**

## Stage 7 — Git history, regression and recurring-fix analysis
- **Pre-gate G7-pre:** Rama explicitly approves the full-history fetch
  (un-shallowing) — the only sanctioned state-changing Git operation, scoped
  to fetch/deepen only. Without it, Stage 7 runs on the 66-commit window and
  every statement carries that caveat.
- **Activities:** method per AC-03 §6 — hot-file analysis, revert chains,
  fix-quality reads, abandonment scan, secrets-in-history scan; cross-link
  history evidence to current-tree findings; mark regressions.
- **Outputs:** `13_GIT_HISTORY_AND_CHANGE_ANALYSIS/` analysis; register
  updates (`REGRESSION` flags).
- **Exit criteria:** analysis complete over the available window;
  limitations (including any remaining shallowness) recorded.
- **Approval → Gate G7.**

## Stage 8 — Consolidation, de-duplication, validation, prioritization
- **Activities:** full-register validation per AC-03 §8 (re-derivation,
  counter-explanation, classification, duplicate checks); merge/link
  overlapping findings; build impact/dependency analysis
  (`15_IMPACT_AND_DEPENDENCY_ANALYSIS/`); rank per AC-04 §4; reconcile open
  questions (resolved / carried forward).
- **Outputs:** validated `14_FINDINGS_REGISTER/`; impact analysis.
- **Exit criteria:** zero unvalidated findings; zero orphaned open
  questions; every AC-02 area has a final disposition.
- **Approval → Gate G8.**

## Stage 9 — Final assessment
- **Activities:** write the final report: system-level risk picture,
  findings by domain and priority, systemic patterns, what was not audited
  and why, unresolved questions, and the audit's own limitations. No
  remediation design (Charter §5), though findings' impact statements stand.
- **Outputs:** `16_FINAL_REPORT/`.
- **Exit criteria:** Charter §9 completion definition met.
- **Approval → Gate G9:** Rama accepts the report; separate decisions on
  committing/pushing the workspace remain his.

---

### Standing rules across all stages
- No commit/push at any stage without separate explicit approval.
- V2 untouched throughout.
- Writes only inside the dated workspace.
- Progress and gate decisions logged in AUDIT_PROGRESS_LOG.md (AC-08);
  scope/method changes logged in AUDIT_DECISIONS_AND_ASSUMPTIONS.md (AC-07).
