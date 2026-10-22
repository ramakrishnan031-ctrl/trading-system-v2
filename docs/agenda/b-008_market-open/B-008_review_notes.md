# B-008 MARKET-OPEN OPERATION: REVIEW NOTES

Date: 2026-10-20 (revision 1; first pass the same day)
Status: CLOSED 2026-10-20. Revision 1 with ChatGPT's three reviews of 20-Oct applied; Rama approved the freeze on 20-Oct-2026: the B-008 legacy manual is FROZEN (revision 1, DR-045); the reconciliation addendum is frozen with it; the V2 inputs remain PROPOSED / OPEN. Rama stores and pushes the package from his PC; Claude does not commit or push. Erratum E-1 to the B-008 manual (section 3: the request audit's coverage at ingress) added 2026-10-21 during C-002 (C-002_review_notes.md section 5), accepted in principle in ChatGPT's value-added finalisation review of C-002, approved by Rama on 22-Oct-2026 and recorded in DR-055 together with the C-002 freeze. It corrects only that cell; B-008_reconciliation_addendum.md item 2, which repeats the corrected wording, stays as recorded and is read with it; the B-008 freeze (DR-045) is unchanged.

## 1. Sources
- Inputs (Rama's upload of 20-Oct-2026; to be stored byte-identical at the freeze; byte-identical to the untracked copies already in his docs/inputs, by the hashes of the B-007 push log):
  - B-008_MARKET_OPEN_ANSWER_SHEET_17-OCT-2026.txt: BD363EFD12670C26A93D0CBEEF2DA8A2DC8A21A2DA6D19F1DD394AB204AFC64A; for docs/inputs/chatgpt_originals_and_replies/
  - B-008_RECONCILIATION_FOR_CLAUDE_19-OCT-2026.txt: 299F71D37EDFE0A786C2D4F2505DF1E4E43E8925A8CDAE6DAA4FACFAFFDAC029; for docs/inputs/reconciliation/
  - B-008_MARKET_OPEN_MANUAL_DRAFT_19-OCT-2026.md: 9072B014F523954F0368DF5353B463971688F6B0036EEBC78E1DED27FFFE4EAB; for docs/inputs/manuals/
- Reference only, not stored (Rama, 20-Oct): the 24-Sep Cron Morning Briefing PDF; it carries an account identifier, not reproduced.
- Legacy source at the baseline 970aabf through the masking filter: config/system_config.yaml lines 27 to 48 and 615 to 630; core/market_windows.py lines 140 to 180; signals/webhook_receiver.py lines 500 to 612; signals/signal_processor.py lines 900 to 990, 2030 to 2050 and 2390 to 2405; main.py lines 2316 to 2325; orders/order_placer.py line 1077; scripts/cron_officer.py lines 455 to 470; the baseline tree listing (paths only) for config/strategies/.

## 2. Findings
1. The 10:00 global floor and the strategies' 09:25 windows; exchange open 09:15 (manual section 2).
2. Before 10:00 an intraday alert is refused with 403 at the webhook before parsing, not stored or queued; later stages re-check (section 3). This answers the draft's B8-OQ4.
3. 16 strategy files; the briefing's 15 omit pb01_breakout_retest (section 4; B8-OQ5 answered in part).
4. The pre-market schedule as recorded on 24-Sep; nothing in it gated entries (section 5).

## 3. ChatGPT's 19-Oct set
Its guardrails hold. Corrected: the evidence pinned to the baseline commit, not a main.py object ID; B-005, B-006 and B-007 are frozen, not parked or separate drafts. Its open questions B8-OQ1 to OQ9: OQ4 answered, OQ5 answered in part, OQ6 answered by B-007 (alert-only), the rest stay open (manual section 9).

## 4. Checks (revision 1)
Anchors: 39 cited legacy lines re-read through the filter at 970aabf, 39 ok. check_md.py OK on the four B-008 files; scan_ids.py: only the three input hashes in section 1; no account identifier in any file; never-open blobs 0 in each clone; no legacy code executed. A scan does not prove that nothing sensitive exists.

## 5. Next step
ChatGPT's final acceptance review of revision 1; then Rama's decision on the freeze and the package.

## 6. ChatGPT's value-added review of 20-Oct: disposition
All points applied; none rejected. Review file B-008_CHATGPT_VALUE_ADDED_REVIEW_FOR_CLAUDE_20-OCT-2026.txt, to be stored unchanged in this folder at the freeze.

| Point | Disposition |
|---|---|
| 1.1 to 1.4 governance, pinning, privacy | Baseline commit throughout; B-005 to B-007 FROZEN (manual section 1); the PDF kept outside the repository, cited without its identifier |
| 2.1 pre-10:00 scope | Manual section 3: path named, audit row stated, other routes untraced, Rama's recollection kept apart |
| 2.2 open against floor | Section 2: entries only; processing before 10:00 not denied |
| 2.3 strategy count | Section 4: list-level explanation only; no enabled status or invented values |
| 2.4 to 2.7 schedule, readiness, holidays, unverified claims | Sections 5 to 7 |
| 3 reconciliation | B-008_reconciliation_addendum.md (new); the 19-Oct input unchanged |
| 4 structure | Stage table, flow diagram (section 3), evidence matrix (section 1.1) |
| 5 V2 inputs | Kept segregated, PROPOSED / OPEN, no VP row touched (as B-001 to B-007) |
| 6, 7 change control, checklist | Sections 4 and 7; hashes in the delivery reply |

## 8. ChatGPT's final acceptance review of 20-Oct: disposition
"ACCEPT WITH REQUIRED EDITS". Applied: manual section 8 item 3 scoped to the traced path, with the audit row kept; V2 inputs row 2 reworded the same way (still PROPOSED / OPEN); the final statement labels 09:15 as Rama's. Cited lines re-checked at 970aabf (39 anchors ok). Review file B-008_CHATGPT_FINAL_ACCEPTANCE_REVIEW_FOR_CLAUDE_20-OCT-2026.txt, to be stored unchanged in this folder at the freeze. Full SHA-256 values are in the delivery reply. ChatGPT's final value-added review of 20-Oct ("FINAL FOCUSED REVIEW — NOT FREEZE") asked for one table-cell clarification (the door-checks row keeps the audit row), applied; its file B-008_FINAL_VALUE_ADDED_REVIEW_FOR_CLAUDE_20-OCT-2026.txt is to be stored unchanged in this folder at the freeze.

## 7. Files of revision 1
B-008_manual.md, B-008_reconciliation_addendum.md (new), B-008_review_notes.md (changed); B-008_v2_design_inputs.md unchanged. Nothing frozen, committed or stored until Rama approves.
