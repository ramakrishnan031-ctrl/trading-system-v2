# V2 DESIGN LOG

Project: trading-system-v2
Purpose: lessons learned, open questions, V2 proposals, agenda status, working rules, session notes. Decisions go in V2_DECISION_REGISTER.md, not here.
Security: no credential values in this file.

## 1. LESSONS (rule written after any correction)

| Date (IST) | Mistake / correction | Rule going forward |
|---|---|---|
| 2026-10-04 | The first .gitignore had a *secret* pattern that would have ignored the a-010_secrets-environment folder. The browser also stripped the leading dot from the downloaded file name, so the committed file (gitignore) was inert until renamed. | Test ignore patterns against the real folder tree (including core.ignorecase=true on Windows) before shipping. After downloading any dotfile, confirm its name and run git check-ignore -v. |
| 2026-10-05 | A legacy repo was described as built to exclude credentials, based on an older record. The live repo had since gained full history and a docs/web_claude folder. | Verify a source's current state (fetch or scan) before labelling it clean or pointing Claude Code at it. |
| 2026-10-05 | Both legacy VMs were terminated and deleted without backing up the DB, logs or crontab. Legacy runtime evidence cannot be recovered. | V2 must define backup and restore (M-008/M-009) before any teardown: no machine is deleted without a verified backup. |
| 2026-10-05 | The first A-001 review notes mixed legacy fact with V2 proposals (ChatGPT review point 1). | Keep two layers apart: the topic manual records legacy fact only; V2 directions stay PROPOSED in section 3 below until decided into the register. |

## 2. OPEN QUESTIONS (not decisions)

| # | Area | Question | Raised | Status |
|---|---|---|---|---|
| Q-01 | Pre-live validation | With no paper mode (DR-001), how are decisions validated before they risk capital? Replay/scenario testing alone is not assumed to close the gap. | 2026-10-04 | OPEN |
| Q-02 | File count | One responsibility per file (DR-004) will likely produce many more, smaller files than the legacy runtime set (~200 modules). Expected, not a problem. | 2026-10-04 | NOTED |
| Q-03 | Traceability | Design the identity/traceability scheme early (DR-006 removed the old chain, not the requirement). | 2026-10-04 | OPEN |
| Q-04 | Upgrade concept 3 | Replacement for "paper trading shadow replay" depends on Q-01. | 2026-10-04 | OPEN |
| Q-05 | Environments | Does V2 get its own testing machine, or reuse the existing PC? (Both legacy Oracle VMs are terminated.) See VP-05. | 2026-10-04 | OPEN |
| Q-06 | L-010 / L-011 | GUI-based DB download (DR-009): download format, authentication/security, which tables are exposed. | 2026-10-04 | OPEN |
| Q-07 | A-001 inputs | No A-001 reconciliation file exists in the ChatGPT set. Confirmed by Rama: there is none. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-08 | A-005 inputs | The capital-flow .xlsx is a rough discussion file, not stored in docs/inputs. Rama will upload it when A-005 is discussed. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-09 | Input file names | Two input files carry bracket tags ([Correct A-006], [Not Manual]). Rama: ignore. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-10 | Legacy repos | All three legacy GitHub repos are public. Rama keeps them public (see DR-012). | 2026-10-04 | CLOSED 2026-10-05 |
| Q-11 | A-010 | The legacy Kite API secret appears in the public legacy repos (2 files in docs/web_claude matched a heuristic scan, 2026-10-05). Regenerate it before V2 uses any Kite credentials. | 2026-10-05 | OPEN |
| Q-12 | Document sharing | The V2 repo is private, so neither Web Claude nor ChatGPT can read it directly. Decide: manual upload per topic, or ChatGPT's GitHub connector (private-repo reliability not verified). | 2026-10-05 | OPEN |
| Q-13 | Claude Code setup | Add the clean reference clone (LEGACY_REFERENCE.md, L2) as an additional directory and set deny rules for credential-bearing paths in .claude/settings.json. Syntax to be verified in the first session. | 2026-10-05 | OPEN |
| Q-14 | A-001 evidence | Exact first and last commit dates of the prod snapshot and the exact VM termination date are not known. Not blocking. Optional: read the oldest and newest commit dates on GitHub. | 2026-10-05 | OPEN (non-blocking) |

## 3. V2 PROPOSALS (PROPOSED, not decided; each becomes a register row only when decided)

| # | Proposal | Raised in | Ratify at |
|---|---|---|---|
| VP-01 | V2 purpose: "An automated, headless, live-only trading system for NSE equities via Zerodha that consumes Chartink scanner signals and, through its configured controls and decision logic, validates, decides, executes, manages, reconciles and reports intraday and positional trades." | A-001 | End of Section A (after A-007 and A-008) |
| VP-02 | V2 scope: NSE equity only; Zerodha only; Chartink sole signal source; intraday and positional, with product types finalised at A-007. | A-001 | End of Section A |
| VP-03 | Per-signal-type processors replace the centralised secondary score (intraday and positional use different timeframes and evaluation methods). Existing calculation bases must be reviewed in detail first. | A-001 | A-008 and D-017 |
| VP-04 | Operator boundary: routine operation is fully automated; the only manual actions are kill-switch release and operator interrupt. This also clarifies the "zero manual intervention" wording in the project instructions (5.5), which targets routine moves, renames and triggers. | A-001 | A-011 (states) and I (safety) |
| VP-05 | Environments: development and debugging on Windows 11, production on an Ubuntu VM on Oracle (new instance), no hardcoded paths. | A-001 | Section M and Q-05 |
| VP-06 | Parked requirements: per-holding Telegram status summary (purchase date, quantity, entry, SL, target) at L-001/L-005; reports delivered by GUI download (DR-009) at L-011; start/stop driven by config with NSE trading days at A-003 and B; headless daily Zerodha token at A-010 and M-005. | A-001 | As listed |
| VP-07 | Backup and restore design, including a rule that no machine is torn down without a verified backup. | A-001 | M-008, M-009 |

## 4. AGENDA STATUS

| Topic | Legacy manual | V2 decisions |
|---|---|---|
| A-001 | FROZEN 2026-10-05 (A-001_manual.md, DR-013) | DEFERRED to VP-01, VP-02 (end of Section A) |
| A-002 to A-011 | Not yet reviewed with Claude | Not started |

## 5. WORKING RULES

- W-1 Two layers. A topic manual records legacy fact only. V2 directions are PROPOSED in section 3 and become DR rows when decided. (ChatGPT's governing rule for A-001, adopted 2026-10-05.)
- W-2 File-save rule (permanent, set by Rama 2026-10-05). Every reply that involves a file ends with a SAVE list: each file, its folder, and whether it is new or replaces an existing one.
- W-3 Where files go:

| What | Folder |
|---|---|
| Original ChatGPT set (questionnaires and replies, manuals, reconciliations, legacy baseline) | docs/inputs/... (already placed, immutable) |
| Topic discussion records (Claude review notes, ChatGPT value-added reviews, replies exchanged during a topic) | docs/agenda/<topic folder>/ |
| Final topic manual | docs/agenda/<topic folder>/<ID>_manual.md |
| Decision register, design log, legacy reference | docs/governance/ |
| Instruction cards for Claude Code | docs/web_claude_instructions/yyyy-mm-dd_<task>.txt |
| Claude Code reports | docs/claude_code_reports/yyyy-mm-dd_<task>.md |
| Screenshots and images | Not stored (DR-008) |
| Anything containing credential values | Never stored |

## 6. SESSION NOTES

### 2026-10-04
- Project instructions, Master Redesign Agenda, storage map and production baseline reviewed.
- Decisions DR-007 to DR-010 recorded (tooling, naming, reporting, layout).
- Per-topic review method: compare manual + replies + reconciliation against the legacy baseline and DR-001 to DR-012; list conflicts; propose changes; each approved change from legacy becomes a decision row.
- Repo trading-system-v2 created (private) and first commit pushed.

### 2026-10-05
- .gitignore fixed (renamed from gitignore, *secret* pattern replaced) and pushed.
- LEGACY_REFERENCE.md added. Reference clone of the prod snapshot created; HEAD verified as 970aabf3c78d777d9d34549e021f7c8e756c7bdb.
- D:\Projects\_restore scanned: no .env, token/session JSON or gui_config.local.yaml by file name; docs/web_claude present in both subfolders.
- docs/web_claude found at HEAD of the public prod snapshot. DR-011 and DR-012 recorded. Q-11 to Q-13 opened.
- A-001: Claude review notes, then ChatGPT value-added review. Adopted ChatGPT's two-layer rule; V2 items moved to section 3. Rama: both VMs deleted about 1-2 weeks earlier, no backups taken. Legacy manual draft prepared. Rama approved it: A-001 legacy manual FROZEN (DR-013). Next: A-002.
