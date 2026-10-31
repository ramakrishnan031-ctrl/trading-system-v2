# AUDIT METHODOLOGY

- **Document ID:** AC-03
- **Status:** DRAFT — pending Gate G1 approval
- **Charter:** AC-01 · **Coverage:** AC-02 · **Stages:** AC-06 · **Evidence rules:** AC-05

## 1. Staged sequence

The audit runs through the ten stages defined in AUDIT_EXECUTION_SEQUENCE.md
(AC-06): verification → plan → inventory → architecture → module passes →
cross-module passes → domain deep-dives → history analysis → consolidation →
final assessment. Later stages depend on earlier outputs; no stage starts
before its predecessor's exit criteria are met **and** Rama approves. The one
sanctioned deviation: a defect noticed early (e.g., during inventory) is
recorded immediately in the findings register with whatever evidence exists
at that moment — discovery is never deferred, only *systematic coverage* is
staged.

## 2. Read-only inspection methods

Permitted on this snapshot, without further approval:
- File reading (`cat`/Read tool), directory listing, file metadata (`ls`,
  `stat`, `file`, `wc`).
- Text and structural search (`grep`/Grep, `find`/Glob), including regex
  scans for patterns (config keys, SQL strings, secret-shaped tokens,
  duplicated function names).
- Read-only Git introspection on the existing checkout: `git log`, `git
  show`, `git diff` between existing commits, `git blame` — within the
  shallow horizon until Gate G7-pre.
- Note-taking, tabulation and report writing **only** inside the dated audit
  workspace, plus disposable scratch files in the session scratchpad (never
  in the repository).

Not permitted without a separately approved safe plan:
- Executing any application code, test, script, scheduler, migration or
  notebook from the repository — including "harmless" helpers. Static reading
  only.
- Opening committed binary databases. If Stage 2 finds committed `.db`
  files, a read-only inspection plan (e.g., `sqlite3` in read-only mode on a
  scratchpad *copy*) will be proposed for approval first; default is
  not-opened, recorded as a limitation.
- Any network call to broker, webhook, Telegram or other external service.
- Any Git operation that changes state: fetch, pull, checkout, branch,
  reset, rebase, stage, commit, push. (Un-shallow fetch: Gate G7-pre only.)
- Installing packages or tools beyond what the session already provides,
  unless approved.

## 3. Static review approach (code, config, docs)

Per file/module:
1. **Role pass** — what is this, who imports it, what does it claim to do
   (docstrings/names) vs what it does.
2. **Contract pass** — inputs, outputs, units, formats, invariants assumed
   (e.g., "price is float rupees", "time is IST-naive") and whether callers
   honour them.
3. **Failure pass** — every `except`, retry, timeout, default and fallback:
   what is swallowed, what is logged, what corrupts state.
4. **Consistency pass** — compare against siblings doing similar work and
   against config/docs; divergences become findings or open questions.
5. **Record pass** — module audit note in `03_MODULE_AUDITS/` stating scope
   inspected, method, evidence, findings raised, limitations.

Configs are reviewed by building a key→readers→defaults table; docs are
reviewed by extracting each checkable claim and marking it
verified / contradicted / unverifiable.

## 4. Runtime behaviour without execution

Because nothing is executed (Charter L3), runtime claims are handled as:
- **Static certainty** — behaviour fully determined by code reading (e.g., a
  constant comparison that can never be true). Eligible for "Confirmed
  defect" if the defect is demonstrable on the artifact itself.
- **Bounded inference** — behaviour depending on plausible runtime states;
  stated with explicit preconditions ("if the broker returns X while Y…").
  Classified at most "Suspected defect", confidence per AC-04.
- **Unknown** — behaviour depending on live data, timing or environment we
  cannot see. Recorded as an unverified question with what evidence would
  settle it (a log, a DB row, a VM file) so Rama's VM-side agent can be asked
  later, out of band.
Each finding states which of the three it rests on.

## 5. Cross-module tracing method

For each end-to-end workflow (Area 09): start at the entry point, follow the
call/data chain across module boundaries, and at every boundary record the
**handoff contract**: data shape, units, timezone, symbol format, status
enums, error propagation, and state persisted. Contracts observed on each
side are compared; mismatches are findings. Where a hop is dynamic
(dispatch tables, string-built imports, queue handoffs), the trace records
the ambiguity instead of guessing. Each trace is written as a numbered
step list in `04_CROSS_MODULE_AUDIT/` with file:line references per step.

## 6. Historical Git analysis method (Stage 7, after Gate G7-pre)

1. Quantitative sweep: commit frequency per path (`git log --follow
   --stat`), identifying "bug magnet" files; revert/re-revert chains;
   fix-commit clusters on the same lines (`git log -L` on hot ranges).
2. Qualitative read: messages and diffs of fix-commits — was the cause fixed
   or the symptom; do later fixes contradict earlier ones.
3. Abandonment scan: branches/paths added then orphaned; residue still
   imported (feeds Area 14).
4. Secrets-in-history scan with pattern search across all reachable blobs;
   matches referenced by commit+path only, never copied (Area 11).
5. Everything cross-linked to current-tree findings ("this fragile code has
   been fixed 6 times since …").
Until un-shallowing is approved, only the 66-commit window may be used, and
every history statement must carry the shallow-window caveat.

## 7. Duplication / contradiction / regression investigation

- **Duplication:** symbol-definition map (where is each function/class/
  constant defined more than once); near-duplicate scan on function bodies
  (normalized-text comparison); per-calculation census for critical
  quantities — P&L, position size, capital available, prices, timestamps —
  listing every place each is computed.
- **Contradiction:** three-way comparison code ↔ config ↔ docs per claim;
  plus code↔code where two modules encode the same business rule.
- **Regression:** current-tree defects matched against history (Stage 7) to
  see whether they were previously fixed and reintroduced; such cases are
  flagged `REGRESSION` in the register.

## 8. Validation and peer review of findings

Before a finding is accepted into the register as anything stronger than
"Unverified question":
1. **Re-derivation check** — the cited evidence is re-read in a later/fresh
   pass and must still support the claim exactly as worded.
2. **Counter-explanation check** — actively search for code/config that
   would make the behaviour correct (a guard elsewhere, an overriding
   config). The search performed is recorded either way.
3. **Classification check** — class, severity and confidence re-scored
   against AC-04 definitions, not vibes.
4. **Duplicate check** — against existing register entries; overlaps are
   linked, not re-counted.
Stage 8 repeats all four checks across the whole register. Where this
audit's single-auditor setup limits true peer review, that limitation is
stated in the final report rather than papered over.

## 9. Recording limitations and unresolved questions

- Every stage output and every module note ends with a **Limitations**
  section (what was not inspected and why) and an **Open questions** list
  (ID'd `OQ-nnn`, with: question, why it matters, what evidence would answer
  it, where that evidence likely lives).
- Open questions are tracked centrally in the findings register folder and
  must each be resolved, downgraded, or carried into the final report's
  unresolved-risk list. None may silently disappear.
- "All clear" statements are always scoped: *what* was checked, *how*, and
  the standing reminder that absence of findings is not absence of defects
  (Charter §6.5).
