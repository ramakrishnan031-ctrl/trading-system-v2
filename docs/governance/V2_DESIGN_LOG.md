# V2 DESIGN LOG

Project: trading-system-v2
Purpose: lessons learned, open questions, session notes. Decisions go in V2_DECISION_REGISTER.md, not here.
Security: no credential values in this file.

## 1. LESSONS (rule written after any correction)

| Date (IST) | Mistake / correction | Rule going forward |
|---|---|---|
| (none yet) | | |

## 2. OPEN QUESTIONS (not decisions)

| # | Area | Question | Raised | Status |
|---|---|---|---|---|
| Q-01 | Pre-live validation | With no paper mode (DR-001), how are decisions validated before they risk capital? Replay/scenario testing alone is not assumed to close the gap. | 2026-10-04 | OPEN |
| Q-02 | File count | One responsibility per file (DR-004) will likely produce many more, smaller files than the legacy runtime set (~200 modules). Expected, not a problem. | 2026-10-04 | NOTED |
| Q-03 | Traceability | Design the identity/traceability scheme early (DR-006 removed the old chain, not the requirement). | 2026-10-04 | OPEN |
| Q-04 | Upgrade concept 3 | Replacement for "paper trading shadow replay" depends on Q-01. | 2026-10-04 | OPEN |
| Q-05 | Environments | Does V2 get its own testing machine, or reuse the existing PC/VM pair once legacy is retired? | 2026-10-04 | OPEN |
| Q-06 | L-010 / L-011 | GUI-based DB download (DR-009): download format, authentication/security, which tables are exposed. | 2026-10-04 | OPEN |
| Q-07 | A-001 inputs | No A-001 reconciliation file exists in the ChatGPT set. Confirm whether one was ever produced. | 2026-10-04 | OPEN |
| Q-08 | A-005 inputs | The capital-flow .xlsx (A-005 final clarification) is not in docs/inputs. Confirm whether the A-005 replies depend on it. | 2026-10-04 | OPEN |
| Q-09 | Input file names | Two files still carry local bracket tags: "A-008_BROKER_MODEL_..._[Correct A-006].txt" and "A-005_..._[Not Manual].txt". Record what each reminder means in that topic's review_notes.md. | 2026-10-04 | OPEN |
| Q-10 | Legacy repos | All three legacy GitHub repos are currently public (for ChatGPT access). Decide whether to switch them to private after testing ChatGPT's connector. | 2026-10-04 | OPEN |

## 3. SESSION NOTES

### 2026-10-04
- Project instructions, Master Redesign Agenda, storage map and production baseline reviewed.
- Decisions DR-007 to DR-010 recorded (tooling, naming, reporting, layout).
- Per-topic review method: compare manual + replies + reconciliation against the legacy baseline and DR-001 to DR-010; list conflicts; propose changes; each approved change from legacy becomes a decision row.
- Next: review A-001.
