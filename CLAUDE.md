# CLAUDE.md — trading-system-v2

Live-only NSE equity trading system via Zerodha. Design phase: discuss norms > freeze norms > design > build > test > controlled live.

## Read first (every session)
1. docs/governance/V2_DECISION_REGISTER.md (what V2 has actually decided)
2. docs/governance/V2_DESIGN_LOG.md (lessons and open questions; review the lessons)
3. The instruction card named in the task, from docs/web_claude_instructions/
4. At the start of each agenda topic: the topic workflow, V2_DESIGN_LOG.md W-14 (full text: docs/inputs/reconciliation/Topic_Workflow.txt, with its applicability addendum beside it), and the topic's start package from ChatGPT

## Hard rules
- No code is written against a norm that is not frozen. Do not carry legacy behaviour forward by default; a retained legacy element needs a recorded decision.
- V2 is live-only: no paper mode, no paper/live parity. No .xlsx report generators (DR-009).
- docs/inputs/ is immutable: never edit, rename or overwrite anything there.
- docs/inputs/legacy_baseline/ is out of scope for all work (option C, DR-073; W-15, DR-085): exclude it by git pathspec, or name only the files a step needs, before any search, listing, indexing or read, in every command Claude or a subagent runs for its own work and in every subagent's task scope; never filter output after access. If it is accessed by accident: stop, disclose it and seek a new decision. Ordinary git metadata operations are distinct from inspecting its contents, but a full-clone checkout materialises it and must be avoided: verify with a fresh partial clone and a sparse checkout that leaves it and docs/legacy_system_audit/ out; the current local working copy is not converted or cleaned (W-15, note of 2026-11-01; DR-087, OB-06 and R-06).
- docs/legacy_system_audit/ is a protected folder too: run no command that inspects it. Its 2026-10-31/ archive belongs to the earlier legacy-production audit: leave it untouched and outside V2 agenda work unless Rama separately authorises action (DR-087, OB-09; W-15, note of 2026-11-01).
- Never write credential values (passwords, API secrets, tokens, TOTP secrets) into any file, report or log. Name the location/type only.
- Do not add a file before checking for an existing one that does the job.
- Investigate before acting: report what exists first, then change only what is needed.
- Bug workflow: discuss root cause > design/implement > test > review. Root cause, not a patch.
- At most 2 concurrent subagents unless the instruction card says otherwise.
- One agenda topic at a time; stop at each topic boundary. No push to GitHub before an independent Claude review and ChatGPT's explicit OK TO PUSH (W-14, DR-082).
- Cross-check factual or externally verifiable information from Rama against reliable sources where reasonably possible, and report a discrepancy with the best-supported suggestion before acting; his preferences, intentions and choices are his to state; clarify ambiguity where needed. Before writing a current date or time into any new record, commit, report or file, check today's India date and IST time from a reliable time source, whatever date a prompt gives; never alter historical dates (W-16, DR-086; it binds ChatGPT too).

## Code standards (when building starts)
- One responsibility per .py file, one clear entry point. snake_case functions, PascalCase classes, UPPER_CASE constants, lowercase file/folder names.
- No hardcoded paths, dates, symbols or settings: config / CLI args / env vars only. IST time, ISO dates (YYYY-MM-DD).
- No bare except, no dynamic imports, no hidden global state. Logs, not prints. Exit 0 = success, 1 = failure.
- Resume-safe, idempotent, deterministic. Write to .tmp, validate, then rename.

## Output
- Write every report to docs/claude_code_reports/yyyy-mm-dd_task.md, then state the path.
- Report facts found (file, function, evidence), not assumptions. Mark anything unverified as unverified.
