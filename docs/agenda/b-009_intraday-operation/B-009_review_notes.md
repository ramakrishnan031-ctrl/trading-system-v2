# B-009 INTRADAY OPERATION: REVIEW NOTES

Date: 2026-10-20 (revision 2; revisions 0 and 1 the same day)
Status: CLOSED 2026-10-20. Revision 2, with ChatGPT's value-added review and its revision-2 corrections of 20-Oct applied (sections 9 and 10). Rama approved the final files on 20-Oct-2026: the B-009 legacy manual is FROZEN (revision 2, DR-046), the reconciliation addendum is frozen with it, and the V2 inputs remain PROPOSED / OPEN. Rama stores and pushes the package (section 11); Claude commits nothing. Erratum E-1 to the manual and the addendum (the 15:17 routine is not given the entry gate) was accepted on 20-Oct-2026 and recorded in DR-047 with the B-010 freeze.

## 1. Sources
- Inputs: Rama's uploads of 20-Oct-2026, to be stored byte-identical at the freeze:
  - B-009_INTRADAY_OPERATION_ANSWER_SHEET_20-OCT-2026.txt: E847CC4173418B1CB1983505BD616FC3CAA9139BE7ADD492BE282ECF1013F8ED; for docs/inputs/chatgpt_originals_and_replies/
  - B-009_PROMPT_FOR_CLAUDE_REPOSITORY_REVIEW_20-OCT-2026.txt: 3602AE80387F775F8FB493F4108A22F153D79B9DF84D4FC61EA912DD2A782DB9; for docs/inputs/chatgpt_originals_and_replies/
  - B-009_RECONCILIATION_FOR_CLAUDE_20-OCT-2026.txt: 7E7F2E90E939990898B01590578C643812194DB8A42BBC05AEACB4F20359A52D; for docs/inputs/reconciliation/
  - B-009_INTRADAY_OPERATION_MANUAL_DRAFT_20-OCT-2026.md: 1889862935062A3D2D23F978F188C1EDBDB4E60F2536260461E5152FC9D571D4; for docs/inputs/manuals/
- ChatGPT's review of revision 0, kept with the topic like B-008's reviews:
  - B-009_VALUE_ADDED_REVIEW_TO_CLAUDE_20-OCT-2026.txt: D4D8FBF688C54A2D73B6CE965CB33C21E73EC8B1FDF64BCED18E0448D5BEDFEE; for docs/agenda/b-009_intraday-operation/
  - B-009_REVISION_2_REQUIRED_CORRECTIONS_FOR_CLAUDE_20-OCT-2026.txt: A5ED1CF6ED0A5F75747F5D6821A42DA074A185F76FBFFD3D6DE41DE72D96E014; for docs/agenda/b-009_intraday-operation/
- Claude's research input, as delivered to Rama on 20-Oct-2026:
  - B-009_CLAUDE_REPOSITORY_ANSWERS_20-OCT-2026.txt: 69B253B4DA7DBB0244AFE2AFCECF43948C51F2EB000C63D4D6552286EA8CCE75; for docs/inputs/chatgpt_originals_and_replies/
  - It stays unchanged; its errors are corrected in the addendum, item 1.
- Workflow change (Rama, 20-Oct-2026): Claude answers the sheet from the production repository; the other two repositories are left. This replaces the operator-answered sheet used for B-001 to B-008.
- Legacy source: trading-system-prod-snapshot at 970aabf, read through the masking filter; files in manual section 2 and section 3 below.
- Records: four files in the production repository (manual section 13).

## 2. Clone and security checks (2026-10-20)
- Blob-less sparse clone, explicit paths:
  - 16 strategy YAMLs and 4 records added this session;
  - objects 8,094 after the additions;
  - never-open check 0 after every checkout.
- All reads through the masking filter. No history or tree-wide git command was run on the partial clone. git grep searched the working tree only.
- Scope check: all 330 Python files outside tests/ are checked out (tree listing against the sparse working tree, no difference). So the negative searches cover the whole runtime tree.

## 3. Rechecks at the baseline (ChatGPT's section 18 checklist)
| Item | Result | Evidence |
|---|---|---|
| Strategy count, enabled, PB-01 | 16 files; loader keeps all; control decides per signal; 15 eligible by configuration (12 intraday + 3 delivery); pb01 disabled | loader.py 128-149; control.py 18-24; config 142, 149, 155; the YAMLs |
| EntryGate.add() and pullback_wait_enabled | No caller in 330 files; flag read once | entry_gate.py 103, 209; signal_processor.py 1082 |
| order_protocol, constructor and place() callers | Default LIMIT_TRIPLE everywhere on the path | order_placer.py 572, 961; main.py 3061-3087; signal_processor.py 1446, 2251, 2553; full_entry_engine.py 84-93 |
| Monitor counters, stop, restart | Separate auth and API counters, 3 each, reset on any success; stop event; one start | order_monitor.py 226, 233-234, 640-703; main.py 3960 |
| Expiry 600 s against fill timeout 60 s | Verified | config 127, 194; main.py 3613-3614; webhook_receiver.py 679; signal_processor.py 175 |
| REST quote wiring; feed subscription | get_quote to six constructors; live branch has no partial check; one subscriber | main.py lines in manual section 6; zerodha_adapter.py 1730-1806; order_placer.py 3404 |
| Records' own commit, date and claim | Stated per record | manual section 13 |
| Operator-only items | Kept UNKNOWN | manual section 16 |
| Markdown, scans, hashes | Section 7 | - |

## 4. ChatGPT's reconciliation of 20-Oct: dispositions
- Section 2 (repository scope): ACCEPTED as worded.
- Sections 3.1 to 3.8: ACCEPTED and verified, or corrected (addendum item 2).
- Section 4 (records): ACCEPTED.
- Section 5.1 (per-ID status): ACCEPTED (manual section 16).
- Section 5.2: ACCEPTED, with one change: B2 is source-answered (addendum item 4).
- Section 5.3: cross-reference series CORRECTED to the agenda (addendum item 5).
- Section 6:
  - A: delivered as the addendum.
  - B: delivered as the manual, with every requested section; "per component" is manual section 14.
  - C: these notes.
- Section 7: complied. The sheet and Claude's answers are unchanged; no legacy code was run; no V2 decision; nothing frozen or pushed.

## 5. ChatGPT's draft: what changed in the manual
1. Every "pending recheck" claim is now either verified with lines or labelled.
2. Added: strategy groups A, B and C with intent, protocol, window and pullback flag (section 4).
3. Added: the 15:15 force-close dies with the order monitor (sections 3, 8).
4. Added: the census funnel with its window; the counts are now the corrected ones.
5. The draft's cross-reference index is corrected to the agenda's series.
6. Kept: the draft's structure, evidence labels, and its warnings against overstating source as runtime.
7. Records named but not read are listed as unread (manual section 13).

## 6. Questions for ChatGPT's review
1. Do the three KNOWN DEFECT labels (D1 to D3) meet the evidence bar, given the 330-file scope?
2. Is the per-ID status table (manual section 16) acceptable as the replacement for the draft's checklist?
3. Should any of the twelve unread records be read before the freeze? Claude suggests none is needed for B-009's questions.

## 7. Checks (revision 0)
- Revision 0: Markdown OK (4 files); ID scan 0 hits except the SHA-256 hashes in these notes; no session link; citation check clean.
- Revision 1: see section 9 and the hashes reported to Rama with the files (a file cannot carry its own hash).

## 8. Remaining unknowns
- Installed crontab; which jobs ran.
- Webhook senders by period.
- What the operator saw and did.
- Historical partial fills and complete signal-to-exit traces.
- Whether the order monitor ever stopped in production.
- Paper/live parity.
- Host health on given dates.

## 9. Revision 1: ChatGPT's value-added review of 20-Oct-2026
- Its answers to section 6's three questions, all accepted:
  1. D1 to D3 meet the bar as KNOWN DEFECT (source path), with the precision edits.
  2. The per-ID table stays.
  3. The twelve unread records stay unread; none was used to establish a B-009 claim.
- Required edits A to G: all applied (addendum item 7). Changed sections:

  | File | Sections changed |
  |---|---|
  | B-009_manual.md | Status and date lines; sections 3, 5, 8, 9, 11, 14, 15 (item 1), 18; new section 19 |
  | B-009_reconciliation_addendum.md | Title; new item 7 |
  | B-009_review_notes.md | Status; section 1; this section |
  | B-009_v2_design_inputs.md | Status; rows 1 to 3 scoped |

- New source lines read for revision 1, at the baseline:
  - entry_gate.py 156, 179-235, 260-279, 288, 332;
  - state_store.py 2945-3014 (the gate_state helpers);
  - writers and callers searched across the 330 files: insert_gate_state only at entry_gate.py 332; rehydrate_from_gate_state only at entry_gate.py 156.
- Citations: every line reference in the four files was read at 970aabf3c78d777d9d34549e021f7c8e756c7bdb in this session. The citation check resolves every cited path in the tree, and every cited file is checked out.
- Negative-search scope: all 330 Python files outside tests/, checked out and compared with the tree listing.
- Unchanged:
  - the five inputs, by the hashes in section 1;
  - B-005 to B-008;
  - the V2 register.
- No legacy code was run. No host or broker operation took place. No V2 decision was made. Nothing is frozen, committed or pushed.

## 10. Revision 2: ChatGPT's revision-2 corrections of 20-Oct-2026
1. 38-ID coverage. The answer sheet holds 38 IDs, B9-A1 to B9-H4. A script parsed the manual's section 16 table: 38 IDs found, none missing, none duplicated, none extra. "44" is corrected (addendum item 8).
2. gap_fade_long, manual section 4 and the Group B table:
   - the code reads the YAML end time (15:00);
   - the 11:30 appears only in a docstring and a comment;
   - the conflict stays visible and unresolved.
   - Search: "11:30" across the 330 runtime files; the other hits are unrelated report, dashboard and test files.
3. EOD output (manual section 14): "Square-off actions, summary and reset; a flat book is the intent, the outcome is not established by this trace".
4. Checks: section 11.
5. Baseline 970aabf3c78d777d9d34549e021f7c8e756c7bdb.
   - Checked by Claude in this session: every cited line in the four files; the negative searches over all 330 runtime Python files; the never-open check (0).
   - Not checked: sandbox and records repositories (Rama); the twelve unread records; any host state.
   - As ChatGPT states, its reviews checked the delivered files' content and consistency, not the repository.

## 11. Freeze and package
- Rama approved the final files on 20-Oct-2026 ("Assume I approved for final files preparation").
- B-009 FROZEN, DR-046.
- Design log: the B-009 agenda row and two session notes, for the B-008 publication and this freeze. No scheduling move: VP-19, VP-20 and Q-45 (h) keep B-009 as listed, for Rama's later review.
- Package: 13 files, listed with their SHA-256 in the push commands file (B-009_PUSH_COMMANDS_20-OCT-2026.txt), so the hashes cannot drift from what is pushed.
- Checks: Markdown OK; ID scan hits are this file's SHA-256 values only; no session link; citation check clean; never-open 0.

