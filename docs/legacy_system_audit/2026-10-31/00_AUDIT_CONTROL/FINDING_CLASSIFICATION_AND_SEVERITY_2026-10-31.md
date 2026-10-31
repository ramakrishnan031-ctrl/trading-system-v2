# FINDING CLASSIFICATION AND SEVERITY STANDARD

- **Document ID:** AC-04
- **Status:** DRAFT — pending Gate G1 approval
- **Used by:** every finding in `14_FINDINGS_REGISTER/` and every module/domain audit note. Evidence format: AC-05.

## 1. Finding classes (exactly one per finding)

| Class | Definition | Minimum evidence bar |
|---|---|---|
| **Confirmed defect** | Behaviour or state demonstrably wrong **from the artifacts themselves** — code that cannot do what it must, config contradicting its own schema, data provably inconsistent. No reliance on assumed runtime conditions. | Direct citation(s) that a reader can follow and independently reach the same conclusion. Static certainty only (AC-03 §4). |
| **Suspected defect** | Likely-wrong behaviour that depends on a plausible but unverified runtime condition, input, or environment fact. | Citation(s) + explicit statement of the unverified precondition(s) and why they are plausible. |
| **Design weakness** | Not "wrong" on any single line, but structurally fragile, unsafe-by-design, or failure-inviting (e.g., scattered risk checks, no idempotency, shared mutable state across modules, missing recovery path). | Citation(s) showing the structure + articulated failure scenario it invites. |
| **Improvement opportunity** | Works and is not fragile, but costs quality: duplication, dead code, misleading names/docs, needless complexity. | Citation(s) showing the artifact; impact statement may be modest. |
| **Unverified question** | Something that *might* be an issue but cannot be assessed from this snapshot (needs VM logs, live DB, broker docs, or Rama's knowledge). | Precise question + where its answer likely lives. Carries no severity until reclassified; it carries a **potential severity** marker instead. |

Reclassification is allowed (and expected) as evidence improves; each
reclassification is logged on the finding with date and reason. A finding is
never deleted — superseded entries are marked `SUPERSEDED-BY: <ID>`.

## 2. Severity levels (impact if the issue is real, independent of how sure we are)

Anchored to this system's stakes: real capital (LIVE migration planned),
broker orders, and data the operator relies on.

| Level | Objective criteria (any one suffices) |
|---|---|
| **Critical** | Plausible path to unbounded or large financial loss (position without stop-loss, double order, capital accounting divergence spent as if real); silent corruption of trade/P&L records; exposed secret granting broker/account access; unauthenticated input able to cause orders. |
| **High** | Bounded but material financial impact (wrong sizing, missed exits, wrong P&L shown and acted on); loss of a safety layer with others remaining; crash/stall of the trading loop during market hours without automatic recovery; integrity risk requiring manual reconciliation. |
| **Medium** | Wrong behaviour in non-money paths (reporting, dashboards, alerts) that misleads the operator; recoverable operational failures; fragile code on a money path that currently happens to work; test gaps on critical logic. |
| **Low** | Quality defects with contained blast radius: duplication, dead code, contradictory docs, noisy logging, minor config drift — anything whose worst case is confusion or wasted effort, not money or data. |
| **Informational** | Observations worth recording with no current adverse impact: conventions, context, verified-OK notes with scope. |

Severity rationale is mandatory: one or two sentences tying the finding to
the criteria row claimed. Where impact is conditional, severity is scored on
the stated scenario, and the condition is part of the record.

## 3. Confidence levels (how sure we are the issue is real as described)

| Level | Meaning |
|---|---|
| **High** | Evidence is direct and complete; counter-explanation search (AC-03 §8.2) performed and found nothing. |
| **Medium** | Evidence is strong but an identified assumption remains unverified, or the counter-explanation search was bounded. |
| **Low** | Indicative evidence only; alternative benign explanations are live. |

## 4. Severity and confidence are separate axes

A finding's priority is read from both together, never from either alone:
- *Critical severity / Low confidence* — e.g., a suspected capital-release
  gap not yet traced through every path — is a top investigation priority,
  not a top "fix" claim.
- *Low severity / High confidence* — e.g., confirmed duplicate dead module —
  is certain but can wait.
- Class caps confidence-of-wrongness claims, not severity: an Unverified
  question may carry **potential severity: Critical**.

The Stage 8 prioritization orders findings by severity first, then
confidence, then breadth of dependency (from `15_IMPACT_AND_DEPENDENCY_ANALYSIS/`).

## 5. Register bookkeeping

- IDs: `F-nnn` (findings), `OQ-nnn` (open questions), assigned sequentially,
  never reused.
- Status field per finding: `OPEN` / `MERGED(into F-x)` / `SUPERSEDED-BY(F-x)`
  / `WITHDRAWN(reason, dated)` — withdrawal requires recording why the
  evidence failed, keeping the audit honest about its own misses.
