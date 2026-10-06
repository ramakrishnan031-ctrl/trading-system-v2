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
| 2026-10-05 | In A-001, Rama's description of how holdings work (no auto square-off) was filed as a defect. A-002 shows it was by design. | Before classifying a statement as a defect, check that it contradicts a stated objective; by-design behaviour is not a defect. |
| 2026-10-05 | In the A-002 review, "16 strategies, each with its own YAML" was labelled production FACT, though it came from Rama's recollection and a testing-VM record (ChatGPT A-002 review point 1). | Counts and inventories are FACT only when read from the production baseline; otherwise name the source and mark the claim UNKNOWN or HISTORICAL. |
| 2026-10-06 | In A-004 Claude wrote that SME was "likely excluded" from an observation about the series filter (ChatGPT A-004 review point 4). | An observation about a filter or code path is an audit lead, not evidence of what was traded; label it INTERPRETATION / AUDIT LEAD and keep the claim UNKNOWN. |
| 2026-10-06 | Claude dated NSE's 1-paisa tick change to June 2025 from a search snippet that showed no year; the change took effect 10-Jun-2024. ChatGPT's review also mis-dated the change as after the legacy period. | Confirm the date of any external fact from a dated source (exchange circular, broker bulletin, dated news) before writing it. |
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
| Q-15 | E-007 / H | Did the legacy have a "smart target manager" that adjusted target ranges (Rama's mention in A-002)? Verify from the repo. | 2026-10-05 | OPEN |
| Q-16 | A-005 | Legacy sizing conflict: Rama recalls capital per trade = capital incl. margin / max trades per day, capped by a strategy limit; the production baseline shows risk-based sizing (min of risk, capital, concentration quantities, times a tier multiplier). Resolve with code evidence. | 2026-10-05 | OPEN |
| Q-17 | A-008 | Production strategy-YAML count and membership: evidence shows 16 historical Chartink scanner definitions (PB01 marked SHADOW), a locked-decision record of 15 YAML filenames, and 16 YAMLs in the testing-VM storage record. Establish from the production snapshot. | 2026-10-05 | OPEN |
| Q-18 | A-003 / VP-21 | Zerodha's current equity MIS auto square-off time: Zerodha support pages say 3:20 PM (fresh MIS blocked after it); a Zerodha forum announcement says 3:25 PM. Check the Zerodha bulletin before V2 sets its square-off times; keep the broker time in config. | 2026-10-05 | OPEN |
| Q-19 | A-003 / B-001 | Verify in source the file-map claims: service start permitted only 08:00-18:15 (main.py exits 0 before loading config) and the validation bound service_window_end < 18:15. | 2026-10-05 | OPEN |
| Q-20 | A-004 / C | Exact signal-to-instrument lookup key and path, whether exchange came from Chartink or was inferred, and what happened to an unknown symbol. Verify in source. | 2026-10-06 | OPEN |
| Q-21 | A-004 / audit | ETF and SME treatment: the production series filter is {eq, be, n, bt, il}; Rama reports Silver/Gold ETFs traded and believes the universe included SME. Can a historical production instruments.csv, trade/order record or broker instrument snapshot establish actual SME inclusion and the ETF path? (Commonly used SME series codes not appearing in the filter is an audit lead only.) | 2026-10-06 | OPEN |
| Q-14 | A-001 evidence | Exact first and last commit dates of the prod snapshot and the exact VM termination date are not known. Not blocking. Optional: read the oldest and newest commit dates on GitHub. | 2026-10-05 | OPEN (non-blocking) |

## 3. V2 PROPOSALS (PROPOSED, not decided; each becomes a register row only when decided)

| # | Proposal | Raised in | Ratify at |
|---|---|---|---|
| VP-01 | V2 purpose: "An automated, headless, live-only trading system for NSE equities via Zerodha that consumes Chartink scanner signals and, through its configured controls and decision logic, validates, decides, executes, manages, reconciles and reports intraday and positional trades." | A-001 | End of Section A (after A-007 and A-008) |
| VP-02 | V2 scope: NSE equity only; Zerodha only; Chartink sole signal source; intraday and positional, with product types finalised at A-007. Decide whether ETFs (traded in legacy) and SME are in scope (A-004). | A-001 | End of Section A |
| VP-03 | Per-signal-type processors replace the centralised secondary score (intraday and positional use different timeframes and evaluation methods). Existing calculation bases must be reviewed in detail first. | A-001 | A-008 and D-017 |
| VP-04 | Operator boundary: routine operation is fully automated; the only manual actions are kill-switch release and operator interrupt, including a simple halt command instead of stopping the VM from the cloud console (A-002). This also clarifies the "zero manual intervention" wording in the project instructions (5.5), which targets routine moves, renames and triggers. | A-001 | A-011 (states) and I (safety) |
| VP-05 | Environments: development and debugging on Windows 11, production on an Ubuntu VM on Oracle (new instance), no hardcoded paths. | A-001 | Section M and Q-05 |
| VP-06 | Parked requirements: per-holding Telegram/email status summary (purchase date, quantity, entry, SL, target), sent each morning, consolidated or per stock; GUI only after the system is stable (A-002) at L-001/L-005; reports delivered by GUI download (DR-009) at L-011; start/stop driven by config with NSE trading days at A-003 and B; headless daily Zerodha token at A-010 and M-005. | A-001 | As listed |
| VP-07 | Backup and restore design, including a rule that no machine is torn down without a verified backup. | A-001 | M-008, M-009 |
| VP-08 | Strategy model: at most 6 strategies (legacy had 16); each has its own YAML and its own secondary filtration; the filtration method (score, ranking or other) is chosen per strategy after its conditions are fixed; S&R logic is set after strategies and processors, and R:R honours S&R. | A-002 | A-008, D-009 to D-017, J |
| VP-09 | Controlled entry: for intraday, wait for the 5-minute candle to complete, but enter early if price crosses the intended entry plus or minus slippage inside that candle; slippage slabs by LTP band; release-price rule (long: release at intended minus slippage; short: as stated by Rama, intended plus slippage); entry timeout exactly 10 minutes in the broker book, enforced by a watcher (never earlier, never later). | A-002 | D-016, E-002, E-003, E-011, G-008 |
| VP-10 | Named price and time types used consistently in every module: payload price, signal-received price, calculated price, locked entry price, candle price, release price, fill price (and matching times). | A-002 | E-009, E-010, K; links Q-03 |
| VP-11 | Every received signal reaches a recorded terminal outcome (ordered, rejected with reason, cancelled, duplicate, expired), confirmed end to end. | A-002 | C-008, L-009 |
| VP-12 | One trade per stock per day in any direction (legacy: per stock and direction); no sector or concentration caps. Consideration: without sector caps, a market-wide move can fill several slots with one sector. | A-002 | C-005, F-007 |
| VP-13 | Sizing and capital: capital per trade = the lower of (bucket capital incl. margin, 5x intraday or 1x delivery, divided by max trades per day from config.yaml) and the strategy.yaml per-trade cap; margin accounting matches the broker's (margin = notional / leverage); broker capital is the source of truth. Consideration: capital-based sizing makes the rupee loss per trade vary with SL distance; legacy used risk-based sizing (Q-16). | A-002 | A-005, F-002, F-004 |
| VP-14 | EOD "analyse today, trade tomorrow": capture Chartink signals after 17:00, store them, confirm next day, then trade. | A-002 | C-011 |
| VP-15 | Configuration: one central config.yaml plus one YAML per strategy; no hardcoded values; values may change during the trading day and the system follows them. Consideration: legacy loaded config once at start-up; define which keys may change intraday and how a change is applied safely. | A-002 | A-009 |
| VP-16 | Trading calendar and day lifecycle: maintained NSE holiday/weekend list for 2026 and 2027, failing safe when the current year's file is missing (A-003: legacy had only 2026); auto-start only on valid trading days; fresh-entry cut-off per config; intraday auto square-off at 15:10 (review legacy behaviour, then keep). | A-002 | A-003, B |
| VP-17 | Metrics to research before deciding: expectancy, execution latency (Zerodha + VM + infrastructure), rejected-signal rate, fill rate. | A-002 | L-007, N |
| VP-19 | Entry cut-off applies to intraday only; delivery may enter until 15:30 (BTST) if its strategy window allows. Consideration: keep the intraday entry cut-off a separate, named control from the SOFT_KILL kill-switch state (legacy conflated them). Zerodha blocks fresh MIS after its auto square-off time anyway. | A-003 | A-003 follow-up in B-008 / B-009, A-011 |
| VP-20 | At the intraday entry cut-off, a watcher cancels every still-pending intraday entry order at the broker and confirms the cancellation, so square-off does not have to. | A-003 | G-014, B-009 |
| VP-21 | Intraday square-off runs well before the broker's auto square-off, then two verification checks confirm the books are flat; Telegram and email alert on failure. Broker time comes from config (Q-18). | A-003 | H-011, B-010 |
| VP-22 | Clean shutdown with carried delivery holdings: the service shuts down normally when only CNC holdings remain (protected at the broker by GTT), and the next-day boot handles carried holdings without failing. Fixes the A-003 defect that stopped a trading day. | A-003 | B-010 to B-012, A-011 |
| VP-23 | Time discipline: IST everywhere including VM and cron; timezone-aware timestamps; a defined DB timestamp timezone; broker clock-skew check. | A-003 | K, I-010; links VP-10 |
| VP-24 | Signals outside the global or strategy entry window are rejected immediately with a recorded reason, not queued (Rama: a pattern seen before the window gives no assurance later). | A-003 | C-009, C-008; links VP-11 |
| VP-25 | Cron review: review cron_officer (about 45 jobs) and cron_registry; classify each job (trading lifecycle, operations, maintenance, reporting, data); keep only what V2 needs; set the service window by V2's needs (post-market only if VP-14 is adopted). | A-003 | M-002, B |
| VP-26 | AI briefings (pre-market briefing, weekend pattern analysis) move from Gemini to ChatGPT. | A-003 | L, M |
| VP-27 | Remove the legacy static symbol blacklist (excluded_symbols). This does NOT mean V2 has no instrument-eligibility controls: legacy also had a separate learned MIS blocklist in screening, and V2 decides separately whether and how eligibility controls exist (for example suspended symbols, surveillance stages, broker MIS-blocked symbols). | A-004 | D-002, C-004 |
| VP-28 | Instrument master: built from the broker dump with explicit series rules; a symbol with no broker match is rejected, never given default token, lot or tick; tick and lot size always come from the broker (exchange ticks vary: 0.01 below Rs 250 since 10-Jun-2024; price-band ticks since 15-Apr-2025). (Raised by Claude from A-004 data risks; PROPOSED only.) | A-004 | E-012, K-009 |
| VP-18 | Process: after all agenda topics, run a full legacy audit, file each issue under its topic, then plan, design, architect and build. | A-002 | DECIDED (DR-016) |

## 4. AGENDA STATUS

| Topic | Legacy manual | V2 decisions |
|---|---|---|
| A-001 | FROZEN 2026-10-05 (A-001_manual.md, DR-013); erratum DR-015 | DEFERRED to VP-01, VP-02 (end of Section A) |
| A-002 | FROZEN 2026-10-05 (A-002_manual.md revision 2, DR-014) | V2 ideas in VP-08 to VP-17; VP-18 decided (DR-016) |
| A-003 | FROZEN 2026-10-06 (A-003_manual.md revision 2, DR-018) | V2 ideas in VP-19 to VP-26; VP-16 extended |
| A-004 | FROZEN 2026-10-06 (A-004_manual.md revision 2, DR-019) | V2 ideas in VP-27, VP-28; VP-02 extended |
| A-005 to A-011 | Not yet reviewed with Claude | Not started |

## 5. WORKING RULES

- W-1 Two layers. A topic manual records legacy fact only. V2 directions are PROPOSED in section 3 and become DR rows when decided. (ChatGPT's governing rule for A-001, adopted 2026-10-05.)
- W-2 File-save rule (permanent, set by Rama 2026-10-05). Every reply that involves a file ends with a SAVE list: each file, its folder, and whether it is new or replaces an existing one.
- W-4 Approval rule (permanent, set by Rama 2026-10-05, DR-017). Once ChatGPT has reviewed a topic and its points are applied, Claude prepares the final storage-ready files (FROZEN status, register rows, commit/push commands) without a separate approval step. Exception proposed by Claude: if ChatGPT's review leaves a question for Rama or disagrees with Claude, Claude asks Rama first.
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
- A-002: review notes and revised legacy manual drafted; V2 ideas added as VP-08 to VP-18; A-001 erratum proposed (holdings line misclassified as a defect).
- A-002: ChatGPT value-added review received; all 12 points applied (revision 2). Q-17 opened (strategy-YAML count, A-008).
- Rama approved: A-002 FROZEN (DR-014), A-001 erratum (DR-015), post-agenda audit (DR-016), approval rule W-4 (DR-017). Next: A-003.
- A-003: review notes and revised legacy manual drafted; V2 request removed from the legacy manual (VP-19); VP-19 to VP-26 added; Q-18, Q-19 opened.

### 2026-10-06
- A-003: ChatGPT value-added review applied (revision 2, all 16 points). A-003 FROZEN under DR-017, recorded as DR-018. No item needed Rama's adjudication. Next: A-004.
- A-004: review notes and revised legacy manual drafted; V2 blacklist input moved out of the manual (VP-27); VP-28 added; Q-20, Q-21 opened.
- A-004: ChatGPT value-added review (R1) applied (revision 2, all points). SME reclassified UNKNOWN; tick-change dates verified (10-Jun-2024, 15-Apr-2025) and Claude's earlier June-2025 date corrected. A-004 FROZEN under DR-017, recorded as DR-019. No item needed Rama's adjudication. Next: A-005.
