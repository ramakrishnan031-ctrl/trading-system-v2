# AUDIT CHARTER — Legacy Production Trading System Deep Audit

- **Document ID:** AC-01
- **Audit date (workspace):** 2026-10-31
- **Prepared:** 2026-10-31 (IST), Stage 1
- **Status:** DRAFT — pending Rama's approval (Gate G1)
- **Related documents:** AUDIT_SCOPE_AND_COVERAGE_MATRIX.md (AC-02), AUDIT_METHODOLOGY.md (AC-03), FINDING_CLASSIFICATION_AND_SEVERITY.md (AC-04), EVIDENCE_AND_CITATION_STANDARD.md (AC-05), AUDIT_EXECUTION_SEQUENCE.md (AC-06), AUDIT_DECISIONS_AND_ASSUMPTIONS.md (AC-07), AUDIT_PROGRESS_LOG.md (AC-08)

---

## 1. Purpose

Conduct a comprehensive, evidence-based deep audit of the legacy production
trading system, as snapshotted in the repository below, in order to **discover
and document** the full set of identifiable defects, inconsistencies,
duplications, contradictions, architectural weaknesses, data-integrity risks,
trading-logic risks, operational risks, security weaknesses, testing gaps,
and historical regression patterns.

The audit produces knowledge, not fixes. Its deliverable is a findings
register and final assessment that Rama (and later development sessions) can
use to decide what to repair, redesign, or retire.

## 2. Objectives

1. Build a complete, verifiable inventory of what the system actually contains
   (code, configuration, documentation, data artifacts, operational scripts).
2. Map the real architecture: modules, entry points, data flows, integrations,
   dependencies — as evidenced by the code, not by the documentation alone.
3. Identify and record issues in every domain listed in the Scope and Coverage
   Matrix (AC-02), and any issue found outside those domains.
4. Trace historical change patterns (regressions, recurring fixes, abandoned
   paths) from Git history once full history is available.
5. Classify every finding by class, severity and confidence under AC-04, with
   evidence meeting AC-05.
6. State explicitly what was NOT examined, what could not be verified, and
   which questions remain open.

## 3. Audit source and baseline identity

| Item | Value | Verification status |
|---|---|---|
| Source repository | `https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot` | Verified in-session: sole `origin` remote for fetch and push |
| Baseline commit | `970aabf3c78d777d9d34549e021f7c8e756c7bdb` | Verified in-session: HEAD equals baseline; commit dated 2026-09-11 02:05 +0530 |
| Working branch | `claude/amazing-goodall-1x49v7` | Verified in-session; local/remote `main` point at the same commit |
| Checkout depth | **Shallow** — 66 reachable commits; history cut at `ce29d49a…` and `dec4e70d…` | Verified in-session via `git rev-parse --is-shallow-repository` |

### Known snapshot limitations (facts, recorded now)

- **L1 — Shallow history.** Only 66 commits are reachable. Stage 7 (Git
  history analysis) cannot be completed against this checkout; it requires a
  user-approved full-history fetch (see §6, Gate G7-pre).
- **L2 — Snapshot, not the live VM.** The repository is a snapshot. The live
  production environment (Oracle Cloud VM, live databases at
  `/home/ubuntu/systems/trading-system/...`, logs, cron state, systemd units
  as installed) is NOT in this session. Findings about runtime behaviour on
  the VM can only ever be *inferred* from code/config/docs here, never
  *confirmed* — and must be classified accordingly (AC-04).
- **L3 — No execution.** Application code and tests will not be run (see §5).
  All behavioural conclusions are static-analysis conclusions.
- **L4 — Database content absent/unexaminable as live data.** Any `.db` or
  data files present in the snapshot are point-in-time artifacts; they do not
  represent current production state.
- **L5 — Repository self-description may be wrong.** Documentation inside the
  snapshot (README.md, AGENTS.md, DEPLOYMENT.md, PATHS.md, GEMINI.md,
  UNPUSHED_LEDGER.md, docs/) is itself audit *subject matter*, not trusted
  ground truth. Contradictions between documents and code are findings, not
  tie-breakers.

## 4. Scope boundaries

### In scope
- Everything inside the production snapshot repository at the baseline
  commit: all source code, configuration, documentation, scripts, data
  artifacts, test suites, deployment files, and (after approved un-shallowing)
  its Git history.
- Static analysis of how the system *would* behave, clearly labelled as
  inference where not directly evidenced.

### Out of scope — hard exclusions
- **The V2 repository (`trading-system-v2`) in its entirety.** No access,
  cloning, inspection, comparison, or reference beyond acknowledging its
  existence. Even "legacy vs V2" comparisons are excluded.
- The live production VM, live broker accounts, live databases, live logs,
  Telegram, Zerodha/KiteConnect live APIs, Chartink live webhooks.
- Any system or repository other than the production snapshot.
- Fixing, refactoring, redesigning, tuning, implementing, deploying,
  migrating, or trading. These are non-goals (§5).

## 5. Non-goals (explicit)

This audit does **not**:
1. Fix any defect it finds, however trivial.
2. Refactor, reformat, or "clean up" any file.
3. Redesign architecture or propose implementation-ready designs. (Findings
   may note *that* a design is weak and *why*; remediation design belongs to a
   separate, later effort.)
4. Tune strategies, parameters, or configuration.
5. Execute application code, tests, migrations, trading processes,
   deployment commands, or database writes.
6. Touch V2.
7. Certify the system as safe, correct, or production-ready. Absence of
   findings in an area is evidence only of what was inspected, by what
   method, with what limitations — never proof of absence of defects.

## 6. Independence and evidence principles

1. **Evidence before assertion.** Every finding cites exact repository
   paths/line ranges or commit hashes per AC-05. No invented line numbers,
   runtime outcomes, or test results.
2. **Facts / hypotheses / inferences / unknowns are labelled as such.**
   A reader must always be able to tell which is which.
3. **Documentation is a subject, not an authority.** When docs and code
   disagree, record the contradiction; do not silently pick a side.
4. **Confirmed means directly evidenced.** The "Confirmed defect" class is
   reserved for issues demonstrable from the artifacts themselves (AC-04 §1).
5. **Negative results are reported with scope.** "No issue found in X" must
   state what in X was inspected, how, and what wasn't.
6. **No silent resolution.** Ambiguities and contradictions become recorded
   open questions, not quiet editorial decisions.
7. **Severity ≠ confidence.** A low-confidence suspicion about capital
   handling can be Critical severity; the two axes are kept separate (AC-04).

## 7. Approval gates and change control

- The audit proceeds through Stages 0–9 defined in AC-06. **Each stage ends
  with an explicit stop; Rama's approval is required to start the next
  stage.** No stage may begin before its predecessor's outputs exist.
- **Gate G1:** Rama approves these Stage 1 documents before Stage 2 begins.
- **Gate G7-pre:** Un-shallowing the repository (full-history `git fetch`) is
  a Git operation requiring separate explicit approval before Stage 7.
- **Commit/push gate:** Nothing in the audit workspace is staged, committed,
  or pushed without separate explicit approval from Rama, each time.
- **Charter changes:** Any change to this charter, the scope matrix, the
  methodology, or the classification standard after G1 approval must be
  recorded in AUDIT_DECISIONS_AND_ASSUMPTIONS.md (AC-07) with a dated
  decision entry, and noted in AUDIT_PROGRESS_LOG.md (AC-08).
- **Write boundary:** All audit outputs are written only inside
  `audit/legacy-system-deep-audit_2026-10-31/`. No file outside that folder
  is modified. (The repository's AGENTS.md write-allowlist targets the `agy`
  VM agent; Rama granted an explicit exception locating this audit workspace
  under `audit/` — recorded as a decision in AC-07.)

## 8. Roles

- **Rama** — audit owner; sole approver of stage gates, Git operations, and
  scope changes.
- **Claude Code (this and successor sessions)** — auditor; performs read-only
  inspection, writes audit documents, reports findings with evidence.
- No other party has authority over this audit.

## 9. Completion definition

The audit is complete when Stage 9's final assessment is delivered, every
coverage-matrix area has a recorded disposition (audited with findings /
audited clean-with-limitations / not audited and why), the findings register
is consolidated and de-duplicated, and all unresolved questions and
limitations are listed. Completion does not certify correctness (§5.7).
