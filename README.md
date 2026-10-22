# trading-system-v2

Live-only, single-responsibility, evidence-governed trading system for NSE equities via Zerodha.

Status: design phase. No code is written against a norm that has not been frozen.

## Layout
- docs/ holds all non-coded material only:
  - governance/ : V2_DECISION_REGISTER.md, V2_DESIGN_LOG.md, the master agenda, LEGACY_REFERENCE.md and the session handover notes
  - agenda/ : one folder per agenda topic, named <id>_<topic> in lower case (for example c-002_scanner-to-webhook), with the topic's manual, review notes and ChatGPT's reviews, and where needed its reconciliation addendum and V2 design inputs; a-section-end/ and b-section-end/ hold the section-end decision papers
  - inputs/ : immutable source material (ChatGPT originals and replies, with each topic's checklist or answer sheet and, from B-009, its research prompt and Claude's research files; manuals; reconciliations; legacy baseline). Files are only ever added, never edited.
  - web_claude_instructions/ : instruction cards for Claude Code (yyyy-mm-dd_task.txt)
  - claude_code_reports/ : Claude Code reports (yyyy-mm-dd_task.md)
- Live system files will be added in other root folders once the design is frozen.

## Rules in one line each
- Decisions live only in docs/governance/V2_DECISION_REGISTER.md (append-only).
- No secrets in this repo, ever: no passwords, API secrets, tokens or TOTP secrets.
- V2 is live-only: no paper mode, no .xlsx generators.
- Public until production starts (DR-054); a record and offsite backup only, never a deploy path.

See CLAUDE.md for the rules Claude Code follows each session.
