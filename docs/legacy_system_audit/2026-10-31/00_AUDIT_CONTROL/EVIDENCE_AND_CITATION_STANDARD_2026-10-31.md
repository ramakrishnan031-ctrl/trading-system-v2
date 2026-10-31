# EVIDENCE AND CITATION STANDARD

- **Document ID:** AC-05
- **Status:** DRAFT — pending Gate G1 approval
- **Companion to:** FINDING_CLASSIFICATION_AND_SEVERITY.md (AC-04)

## 1. Required fields for every finding record

Every entry in `14_FINDINGS_REGISTER/` uses this template. Fields marked (†)
may be "N/A — reason" but never silently omitted.

```
ID:              F-nnn (or OQ-nnn for open questions)
Title:           one line, concrete, no hedging words doing load-bearing work
Class:           Confirmed defect | Suspected defect | Design weakness |
                 Improvement opportunity | Unverified question
Severity:        Critical | High | Medium | Low | Informational
                 (Unverified question → "Potential severity: X")
Severity rationale: 1–2 sentences mapped to AC-04 §2 criteria
Confidence:      High | Medium | Low, with the limiting factor named
Affected module(s): repository-relative directories/packages
Location:        exact repo-relative path(s) + line range(s), e.g.
                 capital/allocator.py:142-158   (†)
Commit/date:     hash + author date where history is relevant   (†)
Evidence:        what was literally observed — quoted lines or precise
                 description; enough for independent re-derivation
Expected vs observed: what correct behaviour would be and on what authority
                 (doc, config, sibling code, domain necessity) vs what the
                 artifact actually does
Verification:    safe reproduction/verification method and its RESULT if
                 actually performed; otherwise "not performed"   (†)
Basis:           static certainty | bounded inference (+preconditions) |
                 unknown   (per AC-03 §4)
Impact:          operational / trading / data / security consequence
Dependencies:    cross-module touchpoints that amplify or contain it
Related:         linked finding IDs; duplicate/overlap assessment
Status:          OPEN | MERGED(F-x) | SUPERSEDED-BY(F-x) | WITHDRAWN(why)
Limitations/open questions: what remains unverified about this finding
Recorded:        date + stage; reclassification log appended below if any
```

## 2. Citation precision rules

1. **Paths** are always repository-relative from the snapshot root
   (`orders/...`, never `/home/user/...`), exactly as spelled on disk.
2. **Line ranges** come from the actual file at the baseline commit, checked
   at the moment of writing. If a range cannot be pinned (generated file,
   binary, sprawling pattern), say so and cite the narrowest true locator
   (function name, key name, section heading).
3. **Never invent**: no guessed line numbers, no paraphrased "quotes," no
   assumed runtime outcomes, no imagined test results, no fabricated commit
   hashes or dates. A missing fact is written as missing.
4. **Quotes are verbatim** and minimal — enough context to be checkable,
   short enough not to bloat the record. Ellipses mark elisions explicitly.
5. **Commits** are cited as full 40-char hash at least once per record
   (short form thereafter), with author date as recorded by Git.
6. **Docs/config claims** cite file + section/key, same precision as code.
7. **Secrets exception:** credential-shaped content is cited by location and
   shape only (`config/x.yaml:12 — 32-char token-like value`) — the value is
   never copied into any audit document (Area 11 rule).
8. **External references** (e.g., broker API documentation), if used, carry
   source + access date and are marked as outside-the-snapshot evidence.

## 3. Verification honesty

- The `Verification` field records only methods **actually executed** in
  session (e.g., "grep across tree for other writers of this column — none
  found; scope: *.py") with their real output characterized accurately.
- Execution of application code/tests is off-limits (AC-03 §2); therefore
  no finding may claim "reproduced at runtime" during this audit unless Rama
  approved a specific safe plan, and then the approval is cited in the record.
- Negative searches are evidence too and are recorded with their scope and
  pattern, because "we looked and didn't find a guard" is only as strong as
  where we looked.

## 4. Evidence storage

- Narrative findings live in the register; bulky supporting extracts
  (tables, long listings, scan outputs) live beside their stage folder
  (e.g., `01_SOURCE_INVENTORY/`, `13_GIT_HISTORY_AND_CHANGE_ANALYSIS/`) and
  are referenced by relative path from the finding.
- Nothing evidentiary is stored outside the dated audit workspace.
- Scratchpad files are disposable working notes only; anything load-bearing
  must be promoted into the workspace or it does not count as evidence.
