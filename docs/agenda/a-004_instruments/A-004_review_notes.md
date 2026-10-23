# A-004 INSTRUMENTS / MARKET SEGMENTS: REVIEW NOTES

Date: 2026-10-06
Status: CLOSED 2026-10-06. ChatGPT's value-added review (06-Oct-2026, R1) applied in A-004_manual.md revision 2 (section 7). A-004 frozen under DR-017, recorded as DR-019. Erratum E-1 to the A-004 manual (section 5, line 65: what the receiver checked for a symbol and a price) added 2026-10-23 during C-004 (C-004_review_notes.md section 5; C-004_manual.md section 24.1, CE-1), accepted by Rama on 23-Oct-2026 (option (b), line 65 only) and recorded in DR-057 together with the C-004 freeze. It corrects only that line; the A-004 freeze (DR-019) is unchanged.
Place at: docs/agenda/a-004_instruments/A-004_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in the manual; V2 ideas in V2_DESIGN_LOG section 3.
Security: no credential values.

## 1. Sources reviewed
- docs/inputs/manuals/A-004_INSTRUMENTS_MARKET_SEGMENTS_LEGACY_MANUAL_23-SEP-2026.md (ChatGPT legacy manual)
- docs/inputs/reconciliation/A-004_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_23-SEP-2026.txt
- docs/inputs/chatgpt_originals_and_replies/A-004_INSTRUMENTS_MARKET_SEGMENTS_DISCUSSION_CHECKLIST_23-SEP-2026.txt (Rama's replies)
- Legacy baseline (production deep working tree, storage map); frozen A-001 to A-003; DR-001 to DR-018.
- External context (not legacy evidence): NSE tick-size changes (10-Jun-2024 and 15-Apr-2025; dates verified in revision 2).
- Hygiene: no hidden citation markers.

## 2. Overall
The ChatGPT manual is strong. It corrects two of Rama's recollections with production evidence (a static symbol exclusion list existed; F&O reference code existed) and keeps "actually traded" separate from "configured" and "present in the instrument file". The gaps are details the reconciliation file records but the manual drops, two user-reported facts left out, one V2 item inside the legacy manual, and two instrument-data risks worth carrying forward.

## 3. Findings

| # | Finding | Fix in revised manual |
|---|---|---|
| G-1 | The equity series filter is in the reconciliation ({eq, be, n, bt, il}) but not the manual. | Added to section 4.1 (FACT per reconciliation). |
| G-2 | Default values for missing broker matches (token 0, lot size 1, tick size 0.05) are in the reconciliation; the manual only says "default". | Added with exact values (4.1). |
| G-3 | Rama reports ETFs were traded (Silver ETF, Gold ETF). The manual says ETFs are "not separately frozen". | Added as HISTORICAL (user-reported). Whether ETFs passed the series filter is not verified (UNKNOWN). |
| G-4 | Rama believes SME instruments were included. SME boards trade in their own series codes (commonly SM / ST), which are not in the series filter. | Recorded as a conflict: HISTORICAL belief vs INTERPRETATION that SME was likely excluded; verify in source (Q-21). |
| G-5 | The storage record describes config/instruments.csv on the testing VM: 2,228 rows, all NSE, all lot size 1. | Added as RECORD-DERIVED (testing VM, not production). |
| G-6 | Section 6.4 of the manual holds a V2 input (remove the static blacklist). Breaks the two-layer rule. | Replaced by a pointer; the input is VP-27. |
| G-7 | Config inconsistency: BRACKET_ORDER maps to an empty/unsupported product, yet the leverage map still holds BRACKET_ORDER 5.0 (and COVER_ORDER 6.0). | Added as FACT (config); no defect claimed. |
| G-8 | Missing from the manual: the regime component uses NIFTY 50 metadata (not proof of index trading); the broker fallback angelone exists but fallback_enabled = false (A-006 owns it). | Added with cross-references. |
| G-9 | Rama: DDPI activation was needed so the system could sell delivery holdings automatically next day. | Added as HISTORICAL (user-reported); mechanics to A-006 / A-007. |
| G-10 | Strategy split (10 intraday / 5 delivery / 1 placeholder marked intraday; one direction each) ties to the open strategy-count question. | Kept HISTORICAL; cross-referenced to Q-17. |

## 4. Instrument-data risks (INTERPRETATION, not declared defects)
- R-1 Default tick size 0.05 for missing broker matches. NSE moved to a 1-paisa tick for stocks under Rs 250 from June 2025 (external context). A defaulted 0.05 tick could produce prices the exchange rejects or rounds. Not shown to have occurred. [Corrected in revision 2: the 1-paisa change took effect 10-Jun-2024, not 2025; price-band ticks followed from 15-Apr-2025. A 0.05 grid is still valid where the exchange tick is 0.01; rejection risk applies only where the exchange tick is larger than 0.05. See section 7.]
- R-2 Token 0 rows. A row with token 0 sits in the reference file; whether any downstream path could quote or order such a symbol is not established (lookup path is UNKNOWN in the manual).
Both go to the audit (DR-016) and to V2 (VP-28).

## 5. V2 ideas (to V2_DESIGN_LOG, PROPOSED)
- VP-27 Remove the static symbol blacklist (Rama). Consideration: decide what, if anything, replaces it for genuinely untradable symbols (ASM/GSM stages, suspended, broker MIS-blocked); legacy also had a learned MIS blocklist (baseline section 12).
- VP-28 Instrument master: build from the broker dump with explicit series rules; a symbol with no broker match is rejected, never given defaults; tick size and lot size always come from the broker. (Raised by Claude from R-1, R-2.)
- VP-02 (scope) gains a note: ETFs within NSE equity were traded in legacy; whether V2 includes ETFs and SME is decided at A-004 ratification with VP-02.

## 6. Next step (done)
Sent to ChatGPT; its review is applied below.

## 7. Disposition after the ChatGPT value-added review (06-Oct-2026, R1)
ChatGPT approved the direction with eight edits; all applied. No point needs Rama's adjudication.

| ChatGPT point | Outcome |
|---|---|
| 1. Series filter | Kept as FACT; not generalised to "all NSE equity". Universe distinctions written into manual section 2. |
| 2. Fallback values | FACT that the builder wrote them; downstream use UNKNOWN; not a defect. |
| 3. ETFs | Kept user-reported; path through the filter UNKNOWN. |
| 4. SME | Adopted. My "likely excluded" was an overreach: an observation about the filter is not evidence of what traded. Now UNKNOWN, with the series-code point kept only as an INTERPRETATION / AUDIT LEAD and the open question recorded (Q-21). One precision added: Rama said he believed the universe was all Zerodha NSE equity instruments, which he describes as including EQ, SME and ETF; he did not report a specific SME trade, so "included/traded" is worded as his belief. |
| 5. Testing-VM 2,228 rows | Kept RECORD-DERIVED, testing VM, never production. |
| 6. Blacklist | Manual now separates A (static exclusion, documented), B (learned MIS blocklist, pointer to D) and C (V2 decision). VP-27 reworded so it cannot be read as "V2 has no eligibility controls". |
| 7. Bracket leverage | Configuration observation; not a defect. |
| 8. NIFTY 50, angelone | Cross-references only; angelone live use marked not established. |
| 9. DDPI | User-reported; mechanics to A-006 / A-007. |
| 10. Strategy split | Marked user-reported, not a verified inventory; ChatGPT's safe wording adopted; A-008 owns membership. |
| 11. Tick risk | Layered as legacy fact, external context, interpretation and unknown; builder-versus-order-math scope note added. Correction to the external layer: ChatGPT's review says NSE's tick rules changed after the legacy implementation period. Dated sources show both changes came earlier (10-Jun-2024 and 15-Apr-2025) than the legacy live period (from 11-May-2026). The verified dates are used; this changes no legacy fact or classification. My own revision-1 date (June 2025) was also wrong and is corrected. |
| 12. Token 0 | FACT possible; reach UNKNOWN; not a defect. |
| 13. VP-28 | Stays PROPOSED. |
| 14. VP-02 ETF/SME | Stays an open V2 input. |
| 15. Evidence-boundary rule | Both sentences added to manual section 2. |
| 16. Classification | Manual section 15 aligned. |

External sources for the tick dates: Zerodha bulletin of 25-May-2024 on the change for NSE stocks and F&O contracts trading below Rs 250; Business Standard, 27-May-2024; AngelOne market update on the 15-Apr-2025 price-band revision.
