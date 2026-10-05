# V2 DESIGN LOG

Project: trading-system-v2
Purpose: lessons learned, open questions, session notes. Decisions go in V2_DECISION_REGISTER.md, not here.
Security: no credential values in this file.

## 1. LESSONS (rule written after any correction)

| Date (IST) | Mistake / correction | Rule going forward |
|---|---|---|
| 2026-10-04 | The first .gitignore had a *secret* pattern that would have ignored the a-010_secrets-environment folder. The browser also stripped the leading dot from the downloaded file name, so the committed file (gitignore) was inert until renamed. | Test ignore patterns against the real folder tree (including core.ignorecase=true on Windows) before shipping. After downloading any dotfile, confirm its name and run git check-ignore -v. |
| 2026-10-05 | A legacy repo was described as built to exclude credentials, based on an older record. The live repo had since gained full history and a docs/web_claude folder. | Verify a source's current state (fetch or scan) before labelling it clean or pointing Claude Code at it. |

## 2. OPEN QUESTIONS (not decisions)

| # | Area | Question | Raised | Status |
|---|---|---|---|---|
| Q-01 | Pre-live validation | With no paper mode (DR-001), how are decisions validated before they risk capital? Replay/scenario testing alone is not assumed to close the gap. | 2026-10-04 | OPEN |
| Q-02 | File count | One responsibility per file (DR-004) will likely produce many more, smaller files than the legacy runtime set (~200 modules). Expected, not a problem. | 2026-10-04 | NOTED |
| Q-03 | Traceability | Design the identity/traceability scheme early (DR-006 removed the old chain, not the requirement). | 2026-10-04 | OPEN |
| Q-04 | Upgrade concept 3 | Replacement for "paper trading shadow replay" depends on Q-01. | 2026-10-04 | OPEN |
| Q-05 | Environments | Does V2 get its own testing machine, or reuse the existing PC? (Both legacy Oracle VMs are terminated.) | 2026-10-04 | OPEN |
| Q-06 | L-010 / L-011 | GUI-based DB download (DR-009): download format, authentication/security, which tables are exposed. | 2026-10-04 | OPEN |
| Q-07 | A-001 inputs | No A-001 reconciliation file exists in the ChatGPT set. Confirmed by Rama: there is none. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-08 | A-005 inputs | The capital-flow .xlsx is a rough discussion file, not stored in docs/inputs. Rama will upload it when A-005 is discussed. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-09 | Input file names | Two input files carry bracket tags ([Correct A-006], [Not Manual]). Rama: ignore. | 2026-10-04 | CLOSED 2026-10-05 |
| Q-10 | Legacy repos | All three legacy GitHub repos are public. Rama keeps them public (see DR-012). | 2026-10-04 | CLOSED 2026-10-05 |
| Q-11 | A-010 | The legacy Kite API secret appears in the public legacy repos (2 files in docs/web_claude matched a heuristic scan, 2026-10-05). Regenerate it before V2 uses any Kite credentials. | 2026-10-05 | OPEN |
| Q-12 | Document sharing | The V2 repo is private, so neither Web Claude nor ChatGPT can read it directly. Decide: manual upload per topic, or ChatGPT's GitHub connector (private-repo reliability not verified). | 2026-10-05 | OPEN |
| Q-13 | Claude Code setup | Add the clean reference clone (LEGACY_REFERENCE.md, L2) as an additional directory and set deny rules for credential-bearing paths in .claude/settings.json. Syntax to be verified in the first session. | 2026-10-05 | OPEN |

## 3. SESSION NOTES

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
- Next: review A-001.
