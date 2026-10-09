# A-006 BROKER MODEL: REVIEW NOTES

Date: 2026-10-06
Status: CLOSED 2026-10-06. ChatGPT's value-added review and Rama's answers to Q-27 and Q-28 applied (section 9). Legacy manual frozen under DR-017, recorded as DR-021; V2 inputs remain PROPOSED / OPEN. Erratum E-2 to the A-006 manual (section 9: "never committed" and the token-monitor trigger) added 2026-10-08 during A-010 (A-010_review_notes.md section 6), reviewed and accepted by ChatGPT on 09-Oct-2026 and recorded in DR-025 together with the A-010 freeze. It corrects only those two lines; the A-006 freeze (DR-021) and erratum E-1 (DR-022) are unchanged. Erratum E-3 to the A-006 manual (section 10: the drift-kill thresholds) added 2026-10-09 during A-011 (A-011_review_notes.md section 6), reviewed and accepted by ChatGPT on 09-Oct-2026 and recorded in DR-026 together with the A-011 freeze. It corrects only that line; the A-006 freeze (DR-021) and errata E-1 (DR-022) and E-2 (DR-025) are unchanged.
Place at: docs/agenda/a-006_broker-model/A-006_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-006_manual.md; V2 inputs in A-006_v2_design_inputs.md and V2_DESIGN_LOG.
Security: no credential values. The legacy files read contain no credential patterns (checked); the token script was read for its header and flow only; docs/web_claude was not touched.

## 1. Sources reviewed
- docs/inputs/chatgpt_originals_and_replies/A-006_BROKER_MODEL_DISCUSSION_CHECKLIST_24-SEP-2026.txt (Rama's answers)
- docs/inputs/reconciliation/A-006_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_25-SEP-2026.txt
- docs/inputs/manuals/A-006_BROKER_MODEL_LEGACY_MANUAL_25-SEP-2026.md
- New evidence: production files read directly at the baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb on 2026-10-06 (public repository, raw files): config/system_config.yaml (broker, product_map, order_monitor, order_reconciler, eod_reconcile, drift_handler, capital sections), config/broker_limits.yaml, broker/zerodha_adapter.py, broker/token_monitor.py, scripts/auto_refresh_token.py (header and flow).
- External (checked 2026-10-06): Zerodha support pages and Z-Connect posts; Kite Connect developer docs and forum (list in A-006_v2_design_inputs.md section 18).

## 2. Overall
ChatGPT's set records Rama's answers faithfully, keeps every item he declined to guess OPEN, and its two current-broker checks (BO unsupported; GTT for CNC/NRML/MTF with OCO) are confirmed. Four things needed work: the legacy part was a short summary while most of the manual was V2 material; one "explain first, then decide" answer was recorded as a suggestion; Rama's question about charges was not answered; and current broker constraints that bear directly on his suggestions (daily manual login, MIS square-off times since 3-Aug-2026, static IP, mandatory market protection) were missing.

## 3. Findings

| # | Finding | Evidence | Fix |
|---|---|---|---|
| G-1 | The 25-Sep manual is mostly V2 material (Parts II-XV) inside a legacy manual; the legacy Part I has four short sections. | Two-layer rule W-1 | Parts II-XV moved, with Rama's verbatim answers, to A-006_v2_design_inputs.md (nothing dropped; answer index of all 44 questions in its section 1). Manual keeps a pointer. |
| G-2 | Part I lacks load-bearing legacy detail: adapter design and order states, order types and varieties, stop-limit SL legs, market-protection bounds, tick snapping, delivery/intraday guards, OCO-GTT structure and SL offset, funds fields, live-margin lookup, rate buckets and 429 handling, token script and monitor, monitor/reconciler/drift/EOD-reconcile settings. | Source read at 970aabf | Added (manual sections 3-10). |
| G-3 | Part XIII lists "historical CO/GTT actual use" as UNKNOWN. For GTT this is too broad: the code places OCO GTTs for CNC (delivery_enabled true in production) and Rama describes the broker handling later-day exits. | Adapter and config read; A-004 | Manual section 6: code = FACT, use = HISTORICAL (Rama); GTT placement records not read. CO use stays UNKNOWN. |
| G-4 | Rama's Q7 answer ("NO - Explain [Why? I'm unaware fully, so safer option, Explain fully then decide]") is recorded as "Suggested Intraday product: MIS". | Checklist Q7 | Recorded as EXPLAIN FIRST; explainer and options in V2 inputs section 4.1; Q-27 for Rama. Lesson added to V2_DESIGN_LOG. |
| G-5 | "Delivery (must be in GTT)" (Q10) and "symbol + product (MIS/GTT)" (Q30) treat GTT as a product. CNC is the product; GTT is the broker-side trigger. Whether the delivery ENTRY should also be a GTT is not asked. | Zerodha GTT pages | Clarified in V2 inputs sections 4, 5, 12; entry question Q-28. |
| G-6 | Rama's Q18 question ("why do we requires that?") is not answered. | Checklist Q18 | Answered in V2 inputs section 9 (not needed for sizing; needed only for net P&L, a net daily-loss limit, capital explanations and reports). |
| G-7 | Rama's Q4 preference (headless, no manual step) is recorded without the current constraint: two Zerodha staff replies state that exchange requirements need a manual login at least once a day and advise against automating it. The legacy scripted TOTP login follows the same headless pattern. | Kite forum; token script read | CURRENT EXTERNAL BROKER FACT in manual section 9 and V2 inputs section 3; Q-29 (decide at A-010); audit lead Q-30(c). |
| G-8 | Token refresh time conflict not noticed: script docstring 08:00 vs 08:15 in the A-003 record. | Script header read | Manual sections 9 and 13. |
| G-9 | Current broker facts that Rama's suggestions depend on are missing: MIS square-off 3:12 PM (CAS stocks) / 3:25 PM (others) since 3-Aug-2026; static IP for order placement; 10 orders per second with 429; mandatory market protection; order status sequence (order_id = accepted, not live); documented rate limits; GTT limits (LIMIT-only via API, single-use trigger, 365 days, buy-OCO for F&O only, trailing SL not in the API); charges API; conflicting statements on a Kite sandbox. | Zerodha and Kite pages | Added as CURRENT EXTERNAL BROKER FACT (V2 inputs; manual sections 5, 8, 9). Q-18 closed; Q-01 annotated. |
| G-10 | Legacy quote bucket 3/s ("bumped from 1") is above Kite's documented quote limit of 1 request/second. | broker_limits.yaml; Kite docs | Audit lead Q-30(a); manual sections 8 and 13. |
| G-11 | The legacy instrument filter admitted the BE series (A-004), where the exchange bars intraday, so MIS orders on BE stocks are rejected. | A-004 manual; Zerodha support | Audit lead Q-30(b); manual section 5. |
| G-12 | Q24 (API failure per category) has no answer, and Q39 (production vs testing broker differences) was left blank for repository review; neither is flagged as such. | Checklist | Q24 OPEN (V2 inputs section 11); Q39 UNKNOWN, no comparison made (manual section 11, V2 inputs section 16). |

## 4. Rama's answers needing follow-up
- Q7 (MIS): explain first, then decide: Q-27 (V2 inputs section 4.1).
- Q8, Q10, Q11 (delivery "must be in GTT"): exit by OCO GTT is clear; the entry pattern is not: Q-28 (V2 inputs section 5).
- Q18 (charges): question answered (V2 inputs section 9); whether V2 records charges, and how, stays OPEN for F-009 / L-011.
- Q4 (headless token): conflicts with the stated exchange requirement: Q-29, for A-010 / M-005.
- Declined to guess, kept OPEN with notes (as Rama asked): Q3, Q9, Q11, Q19, Q23 to Q28, Q36, Q40, Q43.

## 5. Cross-topic evidence found while verifying A-006
- MIS square-off times since 3-Aug-2026 (NSE closing auction session): 3:12 PM for CAS stocks, 3:25 PM for others: closes Q-18; VP-21 extended (Rama's two-check square-off must finish before the earlier time). Legacy's 15:09 cut-off came before both (manual section 5).
- Quote rate limit and BE-series intraday bar: audit leads Q-30(a), (b); BE also bears on VP-27, VP-28 and Q-21 (A-004).
- Kite sandbox: developer docs describe one (LIMIT orders only; no margins, charges, GTT or MARKET orders); Zerodha's FAQ says none exists: noted on Q-01 (pre-live validation) for N-003.
- Zerodha's trailing-stoploss GTT (Kite web, 23-Jun-2026) is not available through the API: relevant to Q-15 (smart target) at H.
- Charges API (virtual contract note) exists: relevant to F-009 and L-011.
- Order-margins API counts existing positions and open orders: relevant to Q-26 (pending-order reservation).

## 6. V2 items (to V2_DESIGN_LOG, PROPOSED)
New VP-34 (global Telegram + Email alerts), VP-35 (broker authority, positions, local DB role OPEN), VP-36 (session/token), VP-37 (products and eligibility), VP-38 (order lifecycle and exits), VP-39 (rate limits, API failures, retries), VP-40 (broker evidence, retention, charges). Extended: VP-03 (entry flow restated in Q10), VP-21 (two checks; CAS times), VP-28 (daily refresh with failure alert), VP-29 (opening cash / available cash), VP-30 (Q12-Q15, Q17). New Q-27 to Q-30; Q-18 closed; Q-01 annotated. Full text in A-006_v2_design_inputs.md.

## 7. Questions for Rama (answered 06-Oct; see section 9)
- Q-27: should every intraday trade use MIS? Options A (MIS for all intraday, reject MIS-blocked stocks), B (MIS plus CNC for long signals on MIS-blocked stocks), C (CO). Explainer: V2 inputs section 4.1.
- Q-28: for delivery, is the entry a normal CNC order followed by an OCO GTT for the exit (legacy), or is the entry itself a GTT? V2 inputs section 5.
- Q-29 (later, at A-010): the token workflow, given the daily manual-login requirement. No answer needed now.
Neither Q-27 nor Q-28 blocks freezing the legacy manual; they affect only PROPOSED V2 material.

## 8. Next step
Send the four files (A-006_manual.md, A-006_v2_design_inputs.md, this file, V2_DESIGN_LOG.md) to ChatGPT, with Rama's answers to Q-27 and Q-28 if ready. After its review: A-006 FROZEN, DR-021 and the commit/push commands, per DR-017.

## 9. Disposition after ChatGPT's review and Rama's answers (06-Oct-2026)

Rama's answers (chat, 06-Oct):
- "Q-27 → A: Every intraday trade uses MIS; if the stock is not eligible for MIS, reject the signal rather than silently changing the product."
- "Q-28 → A: CNC buy → OCO GTT for SL + Target. This is also consistent with the legacy behavior already established."

ChatGPT's review (docs/agenda/a-006_broker-model/A-006_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_06-OCT-2026.txt) agreed with the review direction and the three-way separation (legacy manual / reconciliation evidence / proposed V2 inputs), asked for no change to legacy facts, left no question for Rama and did not disagree with Claude. All points applied:

| ChatGPT point | Outcome |
|---|---|
| 1. Label current broker rules "CURRENT EXTERNAL BROKER FACT, checked 06-Oct-2026"; never present them as legacy evidence | Applied to every A-006 file (replaces EXTERNAL CONTEXT). Adopted for later topics as working rule W-5 in V2_DESIGN_LOG; manuals frozen before A-006 keep their label. |
| 2. Q-27 = A agreed as a V2 proposal | Recorded with ChatGPT's wording ("rejected rather than converted to CNC"); pre-check mechanism OPEN (G); PROPOSED, carried to A-007 (V2 inputs section 4.1; VP-37). Q-27 closed. |
| 3. Q-28 = A agreed | Recorded with ChatGPT's wording and sequence (GTT only for the confirmed actual quantity; GTT creation failure = protection failure; manual broker changes reconciled; GTT construction at G); PROPOSED (V2 inputs section 5; VP-37, VP-38). Q-28 closed. |
| 4. Charges | PROPOSED principle recorded (not a sizing input unless configured later; actual charges kept for net P&L, reporting and any net-defined loss limit); source and timing OPEN (V2 inputs section 9; VP-40). |
| 5. Authentication | Kept OPEN / DEFERRED to A-010 / M-005 (Q-29) with ChatGPT's guidance: no promise of automatic login because legacy attempted one; do not automate around a broker/regulatory authentication restriction (V2 inputs section 3; VP-36). |
| 6. Other OPEN items | Kept OPEN with ChatGPT's notes: Q3, Q23, Q24, Q25, Q26, checklist Q27/Q28, Q36, Q37, Q41/Q43 (V2 inputs sections 2, 11, 12, 13, 15). |
| 7. Topic boundaries (A-006 / A-007 / G-H / A-010-M) | Recorded (V2 inputs section 19); adopted as working rule W-6. |
| 8. Statements that must stay outside legacy conclusions | Manual checked: none of the listed statements is made. Final statement now worded as code paths, with GTT use attributed to Rama's account; V2 inputs comparison table changed from "legacy use: yes" to "configured" / "not established"; legacy charge recording labelled RECORD-DERIVED. |
| 9. Security | No credential values (scan clean); external references kept apart from legacy repository evidence. |
| 10-11. Final status | Legacy manual FROZEN (DR-021, revision 1). DR-021 records only the freeze of the legacy record under DR-017, as for A-001 to A-005; no V2 proposal is entered in the register as a decision (point 11.9). ChatGPT's 25-Sep reconciliation stays as filed in docs/inputs. V2 inputs PROPOSED / OPEN. |

Not received: the 27-Sep checklist named "[Correct A-006]". The freeze rests on the 24-Sep answers; if that file holds newer answers, they are added to the V2 inputs file (PROPOSED). The legacy manual changes only if it holds legacy evidence.
