# A-008 STRATEGY MODEL: REVIEW NOTES

Date: 2026-10-07
Status: CLOSED 2026-10-07 (final). ChatGPT's value-added review applied (section 12). Legacy manual frozen under DR-017, recorded as DR-023 together with the A-007 erratum E-1; V2 inputs remain PROPOSED / OPEN. No question for Rama (Q-33 waits for his A-009 to A-011 answers). Erratum E-1 to the A-008 manual added 2026-10-08 during A-009 (F-5 and the Q14 row below, struck through; section 13), accepted by ChatGPT on 08-Oct-2026 and recorded in DR-024 (2026-10-08). Redaction E-2 applied 2026-10-08 during A-010: the testing account's client ID in rows F-6 and "5 Changes; naming note" replaced by a placeholder (A-008_manual.md erratum E-2); accepted by ChatGPT on 09-Oct-2026 and recorded in DR-025. A documentation redaction only: the value remains in the repository history and the immutable input files. Erratum E-3 to the A-008 manual (section 5: the side of an orphan broker position; no reconciler path adopts one) added 2026-10-16 during B-005 (B-005_review_notes.md section 6), revised 2026-10-19 after ChatGPT's value-added review of B-005, accepted in ChatGPT's acceptance review of revision 1, approved by Rama on 19-Oct-2026, and recorded in DR-041 together with the B-005 freeze. It corrects only that passage; the A-008 freeze (DR-023) and errata E-1 (DR-024) and E-2 (DR-025) are unchanged. Erratum E-4 to the A-008 manual (section 8: the start-up check of the twin URL file's URLs is a GET of the scan_webhook_map URLs that only warns, not a HEAD check failing CRITICAL) added 2026-10-21 during C-001 (C-001_review_notes.md section 7), recommended in ChatGPT's independent review of C-001 revision 0, approved by Rama on 21-Oct-2026 and recorded in DR-053 together with the C-001 freeze. It corrects only that passage; the A-008 freeze (DR-023) and errata E-1 to E-3 are unchanged. Erratum E-5 to the A-008 manual (section 9: Rama's account of the testing tag beside the WR4b comment; a clarification only) added 2026-10-21 during C-002 (C-002_review_notes.md section 5), accepted in principle in ChatGPT's value-added finalisation review of C-002, approved by Rama on 22-Oct-2026 and recorded in DR-055 together with the C-002 freeze. It adds one sentence and strikes nothing; the A-008 freeze (DR-023) and errata E-1 to E-4 are unchanged. Erratum E-6 to the A-008 manual (section 24: the cross-reference for webhook authentication) added 2026-10-22 during C-003 (C-003_review_notes.md section 5; C-003_ERRATUM_DECISION_TABLE_22-OCT-2026.txt, CE-2), accepted by Rama on 22-Oct-2026 as proposed and recorded in DR-056 together with the C-003 freeze. It corrects only that cross-reference; the A-008 freeze (DR-023) and errata E-1 to E-5 are unchanged.
Place at: docs/agenda/a-008_strategy-model/A-008_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-008_manual.md; V2 inputs in A-008_v2_design_inputs.md and V2_DESIGN_LOG. External facts labelled per W-5.
Security: no credential values. Source was read from raw files and a blob-less sparse clone that fetched only the needed paths (config/strategies, strategies, signals, screening, capital, core and config files named in the manual); docs/web_claude, .env files, accounts.csv and other credential-bearing paths were not fetched or opened. The Chartink screenshots contain webhook material; only the alert names are used.

## 1. Sources reviewed
- docs/inputs/chatgpt_originals_and_replies/A-008_STRATEGY_MODEL_DISCUSSION_CHECKLIST_27-SEP-2026.txt (Rama's answers)
- docs/inputs/chatgpt_originals_and_replies/A-008_BROKER_MODEL_DISCUSSION_CHECKLIST_27-SEP-2026_[Correct A-006].txt (section 6)
- docs/inputs/reconciliation/A-008_REVISED_SOURCE_RECONCILIATION_29-SEP-2026_R1.txt
- docs/inputs/manuals/A-008_REVISED_STRATEGY_MODEL_LEGACY_MANUAL_29-SEP-2026_R1.md
- Chartink_scanners.v2.txt (Rama, 07-Oct-2026) and his note: "remember need to change all these Why? Being late entries which hits 98% SL as of now [So improved or bit early entry required, note it]"
- Production source and config at 970aabf, the testing config, and two dated audit records in the repository (A-008_manual.md section 2)
- Frozen A-003 to A-007 manuals; V2_DESIGN_LOG; V2_DECISION_REGISTER

## 2. Overall
ChatGPT's R1 set has the right structure: Chartink signal generation is strategy-specific while secondary qualification is common, and legacy fact is kept apart from V2 input. But it was written without repository access, so most of its UNKNOWNs and several of Rama's recollections can now be settled from source. The 15-versus-16 count is resolved (16 YAMLs: the 15 locked names plus the later PB01 shadow). Two findings change the picture materially: the common screener could not score above 65 (HIGH tier unreachable), and the Chartink scanners all use the forming daily candle, with one scanner-condition duplicate and several scanner subsets. One correction to a frozen manual is needed (A-007 erratum E-1, section 8). Nothing here needs Rama's decision before ChatGPT's review.

## 3. Findings

| # | Finding | Evidence | Where |
|---|---|---|---|
| F-1 | Count: 16 YAMLs at the baseline: the 15 S12 names plus pb01_breakout_retest (later, disabled, shadow). Split 13 intraday (12 enabled) + 3 delivery; 10 LONG, 6 SHORT. Not 10 / 5 / 1. | config/strategies; locked S12 | Manual 3 |
| F-2 | Direction is a required field declared in all 16 YAMLs; the webhook carries no side; the order side is computed from the YAML; the registry only mirrors it. R1 said the manual "must not claim" a direction label. | schema.py, signal_processor.py, strategy_direction.py, 17-Jul investigation | Manual 5 |
| F-3 | Control order: enabled, then force_intraday_only (delivery dormant), then trade_type; labels REJECTED_STRATEGY_CONTROL / REJECTED_TRADE_TYPE. Since 10-Jul-2026 force_intraday_only no longer rewrites intent (loader "Option A"); the adapter coerces to MIS under the flag. | control.py, loader.py | Manual 6; A-007 erratum E-1 |
| F-4 | Per-strategy windows: the 09:25 start never bound (global 10:00, testing 09:30); the 15:00 end equals the global end since 29-Jun-2026. R1 left the effect UNKNOWN. | config, signal_processor.py | Manual 7 |
| F-5 | An unmapped scanner gets HTTP 404 before any processing; ~~a duplicate scanner name fails start-up;~~ (superseded by erratum E-1: a duplicate scanner key is not rejected; the pinned parser keeps the last occurrence; no start-up check) the processor has an UNKNOWN_STRATEGY backstop. R1 left both UNKNOWN. | webhook_receiver.py WR4, scan_webhook_map.yaml | Manual 8 |
| F-6 | The testing alert-name prefix ("[testing client ID]-...") is stripped by shared code (WR4b, 09-Sep-2026), not a testing-only manual change; before it, every testing signal was rejected (400). | webhook_receiver.py | Manual 9 |
| F-7 | Per-strategy parameters: fixed stop 0.8% to 2%, R:R 1.5 in all 16, entry offset 0.1% / 0.2%, pullback wait on 6 YAMLs; locked S6 / S13 values differ from the deployed YAMLs. R1 left field values UNKNOWN. | YAMLs, locked_decisions.yaml | Manual 10 |
| F-8 | Risk % was global (1%); stop % per strategy; per-strategy max_risk_pct deleted 24-Jun-2026. | schema.py, config | Manual 10-11 |
| F-9 | One common 10-step screener (weights in scoring_weights.yaml); pass 60 for all (min_score 0); only three per-strategy thresholds; a code record says the score could not exceed 65, so HIGH (80) was unreachable and passing trades were sized at 50% or 70%. | secondary_screener.py, quality_scorer.py, scoring_weights.yaml | Manual 12 |
| F-10 | A per-strategy position cap (2 or 3) existed in every YAML and was enforced (H-7). R1: UNKNOWN; Rama: none. | schema.py, YAMLs, signal_processor.py | Manual 13 |
| F-11 | Same-symbol rules from source: account-wide block while open (DUPLICATE_SYMBOL), contrary-position block, and one executed trade per symbol and direction per day from 3-Aug-2026, per book from 9-Aug-2026. Matches Rama's recollection. | risk_engine.py, signal_processor.py, config | Manual 14 |
| F-12 | Circuit breaker: enabled and wired; its logic and two recorded defects (lost alerts until 24-Jun; restart gaps ruled wrong on 04-Aug). Only its effect stays UNKNOWN. R1 left activation UNKNOWN. | strategy_governor.py, main.py, config | Manual 15 |
| F-13 | PB01: end-of-day scanner routed to a watchlist; the next-morning retest stage writes would-be records only. | webhook_receiver.py, config watchlist, YAML | Manual 16 |
| F-14 | Chartink: all 16 scanners use daily candles only; I and XV have identical supplied conditions (scanner-condition duplicate; different strategies downstream); VII and XIII inside XI, VI and XII inside X (VWAP between low and high makes the extra condition always true); GAP GO and OPEN HIGH/LOW pairs share 9 of 10 conditions; III inside II; no sector logic in "sector rotation"; the range-breakout conditions may be self-defeating (audit lead). | Chartink_scanners.v2.txt | Manual 17 |
| F-15 | BK-1 (17-Jul) gives a measured 15-Jun to 16-Jul record (long win rate 36.9%, 69 stop hits against 44 target hits). Rama's <10% / ~98% figures are attributed reports for an unspecified period. The two datasets are kept separate, not merged. | docs/audit BK-1 | Manual 18; Q-34 |
| F-16 | Settings with no effect identified in the inspected path (order_protocol, smart_tgt_enabled, ATR multipliers, gap buffer) and start times that never bound; descriptions that do not match scanners; placeholder scanner URLs; stale comments. | YAMLs, config, code | Manual 17, 23 |
| F-17 | Pullback strategies wait in the EntryGate and trade only on PRICE_HIT (LTP within entry +/- 0.5% inside 180 s); momentum strategies re-anchor on a fresh LTP. | entry_gate.py, main.py, signal_processor.py | Manual 10 |

## 4. Rama's answers against the source

| Q | Rama | Source answer (manual section) |
|---|---|---|
| 1 Number of strategies | 16 YAMLs = 10 + 5 + 1 | 16 YAMLs; 13 intraday (incl. PB01) + 3 delivery (3) |
| 2 Active vs placeholder | No idea | Baseline: 15 enabled, PB01 disabled; history UNKNOWN (3) |
| 3 Split | 10 / 5 / 1 | 12 / 3 / 1 (3) |
| 4 One-sided; not labelled in YAMLs | YES; not labelled | One-sided YES; direction is declared in every YAML (5) |
| 5 Changes; naming note | Only added; testing client-ID note | Consistent; the fix is shared code WR4b (3, 9) |
| 6 Global trade_type | YES | Consistent (6) |
| 7 force_intraday_only | YES (guess) | Global breaker: delivery dormant, no rewrite since 10-Jul-2026 (6) |
| 8 Enable/disable | YES | Consistent (6) |
| 9 Per-strategy window | UNKNOWN | Start never bound; end equals global (7) |
| 10 Governor active | UNKNOWN | Enabled and wired; effect UNKNOWN (15) |
| 11 One scanner, one strategy | YES | Consistent (8) |
| 12 One strategy, several scanners | NO | Consistent (8) |
| 13 Unknown scanner | UNKNOWN | HTTP 404 before processing (8) |
| 14 Mapping collision | UNKNOWN | ~~Start-up failure (8)~~ (superseded by erratum E-1) A duplicate scanner key is not rejected: the pinned parser keeps the last occurrence; no start-up check (8) |
| 15 Strategy-specific screening | NO, one common filter | Consistent; three per-strategy thresholds only (12) |
| 16 Placeholders | PB01 | Consistent (16) |
| 17 Late entry | YES | HISTORICAL defect; mechanism INTERPRETATION (18) |
| 18 Different timeframes etc. | YES (guess), see file | All 16 use daily candles; conditions differ, with overlaps (17) |
| 19 Strategy-specific SL | YES (guess) | sl_pct in every YAML (10) |
| 20 Strategy-specific target | YES | R:R per YAML, 1.5 in all (10) |
| 21 Target source | strategy YAML | Consistent (10) |
| 22 Strategy to order parameters | UNKNOWN | YAML supplies direction, intent, entry offset, stop %, R:R; product per A-007 (10, 21) |
| 23 Strategy-specific capital | NO | No capital or risk limits in YAMLs; but a position cap (11, 13) |
| 24 Global vs strategy | config major; YAML R:R etc. | Consistent, plus caps and screening thresholds in YAMLs (11) |
| 25 Score / tier to size | YES | Consistent; HIGH unreachable, so 50% / 70% sizing (12) |
| 26-28 Same-symbol rule | per direction, per segment | Consistent from 9-Aug-2026, with an account-wide block while open (14) |
| 27 Per-strategy position cap | NO | Existed (2 or 3) and enforced (13) |
| 29 Paper = live | YES | Resolver identical in both modes (19) |
| 30 Paper-only differences | UNKNOWN | Not at the strategy layer (19) |
| 31 Testing differences | 10:00 vs 09:30 | Confirmed from both configs (20) |
| 32 Defects | Late signals; common filter; near-duplicates | All three confirmed or explained (12, 17, 18) |

## 5. ChatGPT R1: inconsistencies to note
- Status: the manual header says "Frozen: 29-Sep-2026" while its section 29 says "OPEN / REOPENED"; the reconciliation header says "REOPENED" while its section 28 says "CLOSED / FROZEN".
- The manual's section 3 table lists 3 delivery + 12 intraday + PB01, while its text and section 29 say 10 intraday + 5 delivery + 1.
- Section 29's "frozen baseline" states 16 definitions (10 / 5 / 1) while Revision 1 says the YAML inventory is UNKNOWN.
- The reconciliation's section 26 lists "Which exact 16 YAMLs were active" as UNKNOWN against its own rule "Do NOT state 16 YAMLs", and puts "Exact historical Production YAML count remains NOT ESTABLISHED" under HISTORICAL.
- The trace table gives the direction registry as a direction source; it only mirrors the YAML field.
- "Testing was manually adjusted": the source shows a shared code change (WR4b).
All are resolved by the source read; none changes the R1 structure.

## 6. The [Correct A-006] checklist
A second-pass Broker Model (A-006) checklist, 27-Sep, filed with the A-008 inputs. Its 12 questions are unanswered and its 15 recorded new-system inputs already sit in A-006_v2_design_inputs.md (VP-29, VP-30, VP-34 to VP-40). It belongs to A-006, adds nothing new and needs no change to the frozen A-006 manual. It stays in docs/inputs as supplied (A-008_v2_design_inputs.md section 14).

## 7. Points for Rama
- None blocks ChatGPT's review.
- Q-33 (new-system same-symbol rule: may a stock trade again the same day in the opposite direction?): Rama's 27-Sep statements differ (A-007 "One Symbol per Day only ... at any cost"; A-008 "one stock + one direction once a day"). His A-009 to A-011 answers (29-Sep to 01-Oct) are later and will be checked first; Rama is asked only if they do not settle it (lesson from Q-32). ChatGPT agreed (07-Oct review, point 9).

## 8. Correction to a frozen manual: A-007 erratum E-1
- The A-007 manual (frozen, DR-022) says in section 4 that force_intraday_only rewrote every strategy's intent to INTRADAY at load ("The only intent rewrite is force_intraday_only at load time"). That came from a config comment written on 15-Jun-2026. The loader changed on 10-Jul-2026 ("Option A"): the declared intent is preserved, a DELIVERY strategy is dormant at the resolver under the flag, and the adapter coerces the product to MIS. The rewrite existed only before 10-Jul-2026; production had the flag false at the baseline, so no legacy outcome changes.
- Applied as erratum E-1: the two lines stay visible, struck through, in A-007_manual.md section 4, with an Erratum section; matching corrections in A-007_review_notes.md (section 4 rows), A-007_v2_design_inputs.md (section 6) and VP-42. Accepted by ChatGPT (07-Oct review, point 10) and recorded in DR-023 (section 12).
- Lesson added to V2_DESIGN_LOG: config and schema comments can be stale; read the implementing code path.

## 9. Cross-topic evidence found while verifying A-008
- A-003: the testing config's entry_start 09:30 is confirmed (its comment still says 10:00).
- A-005: the risk % was global; per-strategy max_risk_pct was deleted on 24-Jun-2026; with HIGH unreachable, the tier multiplier always cut size to 50% or 70%; the per-strategy cap bound alongside max_open_positions 5.
- A-007: trailing_sl_enabled defaults to false and no YAML sets it, so no BreakevenManager registration was identified in the inspected path; force_intraday_only per erratum E-1.
- C: unknown scanner 404; duplicate names fail start-up; alert-name prefix strip (WR4b); EOD routing for PB01; placeholder scanner URLs.
- D-003 / D-017: the 65 score ceiling; the shadow V3 layers (hard gate, allocator, decision chain).
- E: no gap-buffer effect identified at the baseline start times; ATR stop not implemented; momentum re-anchor; pullback gate mechanics.
- F-009 / I: circuit-breaker defects.
- J: PB01 watchlist and would-be records.

## 10. V2 items (to V2_DESIGN_LOG, PROPOSED)
- New: VP-44 (earlier signals: redesign every scanner), VP-45 (Chartink identity and naming), Q-33 (same-symbol rule), Q-34 (A-008 audit leads).
- Extended: VP-03, VP-08, VP-12, VP-15, VP-32, VP-42; Q-25 and Q-31 annotated.
- Full text: A-008_v2_design_inputs.md.

## 11. Next step (first pass)
Send to ChatGPT: A-008_manual.md, A-008_v2_design_inputs.md, A-008_review_notes.md, Chartink_scanners.v2.txt, the corrected A-007 files and V2_DESIGN_LOG.md. After its review: apply its points, freeze the A-008 manual and record DR-023 (including A-007 erratum E-1). Done: section 12.

## 12. Disposition after ChatGPT's review (07-Oct-2026)

ChatGPT's review (docs/agenda/a-008_strategy-model/A-008_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_07-OCT-2026.txt) accepted the source reconciliation and the legacy findings, called the strategy count resolved, recommended no change to the established legacy facts, accepted erratum E-1 and left no question for Rama. It asked for six wording and consistency corrections before the freeze. All points applied:

| ChatGPT point | Outcome |
|---|---|
| 1. Do not overstate the late-signal mechanism | Manual section 18 split into FACT (forming daily values; conditions true when thresholds are crossed; 10:00 floor and processing delays), INTERPRETATION / AUDIT LEAD (the conditions could produce signals after a substantial part of the move: a credible mechanism) and UNKNOWN (every signal late; volume only late; exact delay; share of the poor results), using ChatGPT's recommended wording. Sections 23 and 25, V2 inputs section 2 and VP-44 reworded to match. |
| 2. Keep "98% SL" historical | Recorded as Rama's report of approximately 98% SL outcomes for an unspecified period, with no measurement population or date range; BK-1 kept as the separate measured dataset; not merged (manual sections 18 and 25; V2 inputs section 2). |
| 3. Current Chartink documentation is not legacy evidence | Manual section 17 lists the legacy plan, actual alert frequency, Unique or Duplicate setting, historical scanner changes and the URL match as UNKNOWN and says the current Chartink facts are not evidence of them; V2 inputs section 16 likewise. |
| 4. Range breakout stays an audit lead | ChatGPT's wording used ("may be self-defeating if the 15-day maximum/minimum includes the current day"); BK-1's zero trades is consistent but not proof; StockCharts marked as context only (manual sections 17 and 23; V2 inputs sections 3 and 16; Q-34 (d)). |
| 5. Scanner duplicate versus strategy duplicate | "Scanner-condition duplicate", "scanner subset/overlap" and "strategy duplication" defined and used; I and XV described as identical conditions feeding different strategies, not identical strategies (manual sections 14, 17, 23 and 25; V2 inputs section 3; VP-08). |
| 6. 16 / 15 / five families | Manual section 17: 16 Chartink definitions = 15 normal trading definitions + PB01 (16th, shadow); the 15 grouped into about five condition families (also V2 inputs section 3 and VP-08). |
| 7. Evidence-bound "no effect" | "Inert" replaced by "no effect identified in the inspected path" or "no production consumer identified" for order_protocol, pipeline, horizon, the ATR fields, the gap buffer, smart target and trailing (manual sections 10, 21 and 23; V2 inputs section 5; VP-15; Q-31 note). |
| 8. A-008 is not V2 design | The manual carries no V2 content; V2 inputs section 17 lists every item that stays PROPOSED / OPEN with its owning topic. |
| 9. Q-33 | Not asked; Rama's A-009 to A-011 answers are checked first (Q-33 row annotated). |
| 10. A-007 erratum E-1 | Accepted; the original lines stay struck through and the corrected implementation is documented separately; recorded in DR-023; A-007 status lines updated. The production outcome is unchanged because the flag was false. |
| 11. V2 input disposition | VP-44, VP-45, VP-08 and the other inputs stay PROPOSED / OPEN. VP-03's ratification point moved from "A-008 and D-017" to D-003 and D-017 (ranking with F), since A-008 decides nothing for V2. No V2 register row. |

Result: A-008 legacy manual FROZEN (revision 1, DR-023); these notes CLOSED; A-008_v2_design_inputs.md PROPOSED / OPEN. Register: one row, DR-023, records the freeze of the legacy record under DR-017 together with the A-007 erratum E-1. No V2 decision is entered.

## 13. Erratum E-1 to the A-008 manual (2026-10-08, found during A-009)
F-5 and the Q14 row above said a duplicate scanner name failed start-up, and the manual said the scanner map was cross-validated at load (S10). Reading the callers for A-009 showed that S10 runs only when the loader is given the map path, which none of the boot or pre-flight callers read passes, and that the pinned parser (PyYAML 6.0.3) keeps the last of two identical keys without error, although YAML 1.2.2 requires unique keys. No legacy outcome changes: at the baseline the 16 scanner keys are unique and every mapped strategy has a YAML. The original text stays visible, struck through; details in A-008_manual.md (Erratum), A-009_manual.md section 5 and A-009_review_notes.md section 6. Accepted by ChatGPT on 08-Oct-2026 (A-009 review, accepted point 9) and recorded in DR-024 (2026-10-08).
