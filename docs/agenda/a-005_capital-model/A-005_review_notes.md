# A-005 CAPITAL MODEL: REVIEW NOTES

Date: 2026-10-06
Status: CLOSED 2026-10-06. ChatGPT's final review and Rama's 06-Oct answers applied (section 10). Legacy manual frozen under DR-017, recorded as DR-020; V2 inputs remain PROPOSED.
Place at: docs/agenda/a-005_capital-model/A-005_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-005_manual.md; V2 inputs in A-005_v2_design_inputs.md and V2_DESIGN_LOG.
Security: no credential values. The legacy files fetched contain no credential patterns; docs/web_claude was not touched.

## 1. Sources reviewed
- docs/inputs/manuals/A-005_CAPITAL_MODEL_LEGACY_MANUAL_24-SEP-2026_REVISED_STRICT.md and A-005_CAPITAL_MODEL_VALUE_ADDED_REVIEW_ZERODHA_MARGIN_23-SEP-2026 [Not Manual].txt
- docs/inputs/reconciliation/A-005_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_24-SEP-2026_REVISED_STRICT.txt
- docs/inputs/chatgpt_originals_and_replies/: A-005_CAPITAL_MODEL_FINAL_PENDING_QUESTIONS_23-SEP-2026-1.txt, A-005_FURTHER_CLARIFICATIONS_BEFORE_RECON_AND_MANUAL_24-SEP-2026-2.txt, A-005_CAPITAL_FLOW_REVIEW_FINAL_CLARIFICATION_24-SEP-2026.txt
- A-005_FINAL_CLARIFICATION-Capital flow design.xlsx (formulas and values read)
- New evidence: three production files read directly at the baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb on 2026-10-06 from the public repository (raw.githubusercontent.com): config/system_config.yaml, capital/position_sizer.py, capital/fund_manager.py; plus the sandbox config (branch main) for comparison. The 24-Sep files say GitHub retrieval was unavailable then, so their repository claims were record-derived.
- External: Zerodha support (same-day profits, T+1).

## 2. Overall
ChatGPT's 24-Sep work captured Rama's V2 capital flow carefully and resolved most contradictions in his answers. Three problems need fixing before A-005 can freeze: V2 material fills half the legacy manual; two legacy claims attributed to the production config are not in it; and a V2 rule leaked into the legacy admission flow. Separately, the strict-scope block appended at the end reverses the V2 quantity formula written in the sections above it, so the file contradicts itself.

## 3. Findings

| # | Finding | Evidence | Fix |
|---|---|---|---|
| G-1 | V2 content (sections 14-20, 25-26 and the strict-scope block) sits inside the legacy manual. | Two-layer rule W-1 | Moved, unshortened, to A-005_v2_design_inputs.md (Rama's "don't skip anything" kept); manual keeps a pointer. |
| G-2 | Legacy admission flow says "one trade per symbol once a day". Production config: one_trade_per_symbol_direction_per_day: true (opposite direction allowed; live from the 3-Aug-2026 boot). The wording is the V2 rule (VP-12). | Config read; A-002 frozen wording | Corrected in manual section 7. |
| G-3 | Manual says the production config states max daily trades served as the sizing divisor and risk_per_trade_pct was inert. Neither is in the production config (970aabf) or the sandbox config; the production sizer uses risk_per_trade_pct. | Config and sizer read | Downgraded to UNVERIFIED, source not identified (manual section 6). |
| G-4 | Q-16 (legacy sizing conflict) can now be closed: production code is risk-based (min of risk, capital, concentration quantities, then tier and performance multipliers). | position_sizer.py read | Manual section 13; Q-16 CLOSED. |
| G-5 | Capital basis was never stated from code. FundManager uses broker cash (initialize at start-up; sync_from_broker adds carried margin), 70/30 buckets, margin = qty x price / leverage. The ChatGPT 23-Sep review's description of legacy as a "synthetic leveraged basis" is not supported by the code read. | fund_manager.py read | Manual section 4. |
| G-6 | The Master Agenda capital baseline (Rs 7,000 cash, 5x MIS, Rs 35,000 buying power) must "remain explicit" but is absent from the manual. Screenshots show about Rs 10.3k balances on 23-24 Sep (account not identified). | Master Agenda F section | Added as RECORD-DERIVED with the conflict recorded. |
| G-7 | Legacy release_used credits net profit and loss to available capital the same day; Zerodha makes intraday profits available only on T+1. Possible drift source on profitable days (audit lead), and the evidence behind Rama's V2 loss-only rule. | fund_manager.py read; Zerodha support | Manual section 10; V2 inputs section 5. |
| G-8 | Live margin already existed: leverage = 1 / broker margin %, margin <= 0 gives 1.0; on API error or paper mode the sizer falls back to static 5x. V2 wants 100% cash instead of a 5x fallback. | position_sizer.py read | Manual section 5; V2 inputs section 7. |
| G-9 | Many verified config values were missing: leverage safety bounds, sizing guards (min tick, max order qty, lot skew, 40% value cap), delivery caps (3 open, 5 per day), daily-loss 3% sole authority, sector cap in observe mode, consecutive losses 4, exit offsets. | Config read | Manual sections 5, 6, 7, 11. |
| G-10 | Broker screenshots of 24-Sep show an open MIS position and orders, yet the storage record says the system was declared dead before the 18-Sep boot. Origin (account, system or manual) unknown. | Storage record; screenshots | Recorded as UNKNOWN (manual section 15). |
| G-11 | "Capital available after Margin" terminology is about Rama's V2 spreadsheet, listed as a legacy known defect. | Manual 24.1 | Moved to V2 inputs. |

## 4. Conflicts inside the V2 material (recorded in A-005_v2_design_inputs.md)

| # | Conflict | Handling |
|---|---|---|
| X-1 | Sections 14-15 size quantity as FLOOR(cash / margin % / price); the appended strict-scope item 3 says quantity is risk/SL-based and that formula is NOT used, with capital as a ceiling. | Q-22 to Rama: confirm the strict-scope block is the latest intent. Claude's reading: risk-based quantity, capped by the capital ceiling (which still needs notional / price to become a share count). The rupee-risk basis is open for F-004. |
| X-2 | Rama's Q13 (pending orders vs bucket) was marked "not understood, explain in plain English"; the plain explanation was never given and the question was dropped. | Q-24 with a plain example (V2 inputs section 6). |
| X-3 | "No margin data, 100% cash, proceed" cannot place an MIS order for an MIS-blocked stock; the broker rejects it. | Consideration in VP-30, linked to VP-27. |
| X-4 | Margin snapshot at 08:30 vs legacy instrument refresh at 09:00. | Ordering consideration in VP-30. |
| X-5 | YAML caps: real cash (Step 6) or notional (spreadsheet rows 28-29)? | Open for A-009 / F-004. |

## 5. Cross-topic evidence found while verifying A-005
- order_monitor.fill_timeout_sec: 60 ("cancel unfilled OPEN/SUBMITTED order after 60s"): an audit lead for the user-reported early cancellation of entry orders (intended 10 minutes). Q-23, for G-008.
- smart_tgt section exists and is enabled (trailing: arm at 0.5% toward target, 0.3% steps): partly answers Q-15.
- slippage_bands (price bands 0-100 up to 1000+) and entry_gate slippage controls exist: relevant to VP-09 (E-011).
- mis_filter (learned MIS blocklist) is enabled in shadow mode only (logs would-drops; "do not set shadow: false without Rama"): detail for A-004 section 6B and VP-27.

## 6. V2 items (to V2_DESIGN_LOG, PROPOSED)
VP-13 rewritten (per-trade cash allocation with the MIN rule; quantity per strict scope pending Q-22); new VP-29 capital basis and daily lock; VP-30 margin data; VP-31 filled-trade limit; VP-32 YAML bound governance; VP-33 entry-margin basis. VP-12 confirmed again by strict-scope item 1. Full text in A-005_v2_design_inputs.md.

## 7. File-structure proposal
A per-topic V2 inputs file (A-005_v2_design_inputs.md) is added because the A-005 V2 material is too large for design-log rows and Rama asked to keep it with A-005. Proposed as a working rule (W-3 table): use such a file only when a topic's V2 material is large; it is PROPOSED content, never decisions. ChatGPT to confirm.

## 8. Questions for Rama (answered 06-Oct, see section 10)
- Q-22: Is the "NEW SYSTEM CHANGES — STRICT SCOPE" block your latest intent, so V2 quantity is risk/SL-based and the capital amount is only a ceiling?
- Q-24: Should orders still pending at the broker count against the daily trade limit and capital while they are open? (Plain example in V2 inputs section 6.) This can also wait for F-010.

## 9. Next step
Send these files to ChatGPT (with Rama's answers if ready). After its review I finalise A-005 (FROZEN), the register rows and the commands, per DR-017.

## 10. Disposition after ChatGPT's final review and Rama's answers (06-Oct-2026)

Rama's answers (chat, and the message shown to ChatGPT):
- Q-22: total cash, split by segment, cash per trade, adjust for the stock's margin (2x-5x / 20%-50%) to get "trading capital per stock", compare that with the YAML maximums ("whichever is low"), then buy quantity from the strategy.yaml risk % / SL %, following the legacy risk formula. So: risk/SL-based quantity with a capital ceiling. YES.
- Q-24: the daily limit counts filled trades. With 9 filled, submit the next; if it waits, submit another; when the count reaches 10, cancel the other pending entries; if one fills before cancellation, accept 11 and block further entries.

ChatGPT final review: agreed with every legacy correction (its point 8) and with the source-priority rule (point 9); asked that Rama's clarifications go only into the V2 inputs file, stay PROPOSED, and carry safeguards. All applied:

| ChatGPT point | Outcome |
|---|---|
| 1. Q-22 sequence | Recorded (V2 inputs section 4) as risk/SL primary, capital ceiling; the notional formula is not the primary rule. |
| 2. Units / three amounts | Terminology block added (cash allocation, trading capital = permitted notional, order quantity). |
| 3. Later-trade capital | MIN(base, current available) kept; base not permanently available. |
| 4-6. Filled-only count, race | Rama's rule recorded with the safeguards: no unlimited submissions; the 11th fill is a bounded race exception (block, reconcile, alert, keep evidence, never reverse a genuine fill). |
| 7. Pending capital reservation, maximum pending entries | Kept OPEN as Q-26; not decided. |
| 8. Legacy corrections | Kept in the manual unchanged. |
| 9. Source priority | Added to the manual's evidence section. |
| 10-11. Keep V2 PROPOSED; do not modify the legacy manual with V2 rules | Done; no V2 rule added to the manual. |

One difference recorded, not resolved: ChatGPT's step 5 applies the strategy cap BEFORE margin, on cash, and treats "maximum capital per trade" as cash unless decided otherwise. Rama's 06-Oct wording compares the YAML maximums AFTER margin, on trading capital (consistent with his spreadsheet rows 28-29). The V2 inputs file records Rama's latest wording, shows with an example that the two orders give different results, and leaves the units and ordering for A-009 / F-004 (Q-25). This affects only PROPOSED V2 material, so it does not block freezing the legacy manual.

Q-22 closed (main rule answered); its remaining details (risk budget basis, YAML cap units and ordering) move to Q-25. Q-24 closed for the filled-count rule; pending capital reservation and maximum simultaneous pending entries move to Q-26.
