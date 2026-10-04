# trading-system-v2

Live-only, single-responsibility, evidence-governed trading system for NSE equities via Zerodha.

Status: design phase. No code is written against a norm that has not been frozen.

## Layout
- docs/ holds all non-coded material only:
  - governance/ : V2_DECISION_REGISTER.md, V2_DESIGN_LOG.md, master agenda
  - agenda/ : one folder per agenda topic (a-001 to a-011 so far), with the revised manual and review notes
  - inputs/ : immutable source material (ChatGPT originals and replies, manuals, reconciliations, legacy baseline). Never edited.
  - web_claude_instructions/ : instruction cards for Claude Code (yyyy-mm-dd_task.txt)
  - claude_code_reports/ : Claude Code reports (yyyy-mm-dd_task.md)
- Live system files will be added in other root folders once the design is frozen.

## Rules in one line each
- Decisions live only in docs/governance/V2_DECISION_REGISTER.md (append-only).
- No secrets in this repo, ever: no passwords, API secrets, tokens or TOTP secrets.
- V2 is live-only: no paper mode, no .xlsx generators.
- Private repo, offsite backup only. It is never a deploy path.

See CLAUDE.md for the rules Claude Code follows each session.
