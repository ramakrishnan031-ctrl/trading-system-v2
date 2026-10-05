# A-002 TRADING OBJECTIVES: REVIEW NOTES

Date: 2026-10-05
Status: CLOSED 2026-10-05. Revision 2 applied the ChatGPT value-added review (section 8). Rama approved P-1 (A-002 frozen, DR-014), P-2 (A-001 erratum, DR-015) and P-3 (post-agenda audit, DR-016).
Place at: docs/agenda/a-002_trading-objectives/A-002_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). The manual records legacy fact only; V2 ideas go to V2_DESIGN_LOG section 3.
Security: no credential values.

## 1. Sources reviewed
- docs/inputs/manuals/A-002_TRADING_OBJECTIVES_FROZEN_MANUAL.md (ChatGPT legacy manual, frozen 22-Sep-2026)
- docs/inputs/reconciliation/A-002_RECONCILIATION_FINAL_22-SEP-2026.txt
- docs/inputs/chatgpt_originals_and_replies/A-002_TRADING_OBJECTIVES_DISCUSSION_CHECKLIST.txt (Rama's replies)
- Legacy baseline (production deep working tree 21-Sep, storage map 18-Sep); frozen A-001 manual; DR-001 to DR-013.
- Hygiene check: no hidden citation markers or formatting faults in any A-002 file.

## 2. Overall
The ChatGPT manual already separates legacy objectives from future requests well (its sections 15 and 18). The gaps are legacy facts that Rama's replies, the reconciliation file or the baseline establish but the manual leaves out, plus one evidence-boundary conflict.

## 3. Findings

| # | Finding | Evidence | Fix in revised manual |
|---|---|---|---|
| G-1 | Three known defects in the reconciliation file are missing from the manual: secondary-filter criteria that were only placeholders; no end-to-end confirmation of what happened to each signal; inconsistent price/time semantics across modules. | Reconciliation item 3; checklist items 2, 15 | Add to sections 3, 4, 10 and 15 as KNOWN DEFECT (user-reported). |
| G-2 | The legacy mechanism structure is missing: one central config; two independent pipelines (intraday, delivery); 16 strategies, each with its own YAML; one centralised secondary screening (score/tier, minimum score) for all strategies; centralised order management, alerts and reports. | Checklist item 15; storage map (16 strategy YAMLs); baseline sections 12-15 | Add to section 4 as FACT. |
| G-3 | Duplicate avoidance is missing as a legacy objective: signal deduplication, and one trade per symbol and direction per day. | Baseline sections 6, 18; checklist item 3 | Add to section 3 as FACT. |
| G-4 | EOD routing is missing: EOD scanner signals went to a watchlist path, never the intraday queue. Whether the system traded next day on stored EOD signals is not established. | Baseline section 7; checklist item 11 | Add to section 12 (FACT + UNKNOWN). |
| G-5 | The manual's evidence boundary says its primary basis was the PC worktree / testing-VM evidence, while its own checklist names the production snapshot as primary. It also predates the VM deletion. | Manual section 16 vs checklist source basis; A-001 section 10 | Rewrite section 16 to match A-001 (production baseline 970aabf; VMs deleted; no backups); record the conflict. |
| G-6 | Holding-status reporting was a later request, not a legacy objective. | Checklist item 12 | State it in section 13. |

## 4. Conflicts (recorded, not silently resolved)

| # | Conflict | Proposed handling |
|---|---|---|
| X-1 | Sizing. Rama recalls capital per trade = capital incl. margin / max trades per day, capped by a strategy limit. The production baseline shows risk-based sizing: the minimum of risk, capital and concentration quantities, times a tier multiplier. | Record both in section 6; resolve with code evidence at A-005 (Q-16). |
| X-2 | Smart target manager. Rama mentions a function that adjusts the target range; the manual and baseline do not establish it. | OPEN; verify from the repo at E-007 / H (Q-15). |

## 5. A-001 erratum (my error)
In the frozen A-001 manual (sections 8 and 9) I classified "holdings had no auto square-off" as a user-reported defect. Rama's A-001 reply described how holdings work (they carry without auto square-off) plus an expectation of a per-holding status alert. A-002 confirms both: delivery positions carry overnight by design, and the status alert was a later request.

Proposed correction, recorded as a new register row (history not edited):
- Section 8: replace the KNOWN DEFECT line with "Rama considers the holdings handling unsatisfactory in visibility: there was no per-holding status alert (purchase date, quantity, entry, SL, target). Per A-002 this alert is a later request, not a legacy objective. Delivery positions carrying overnight without auto square-off was by design."
- Section 9: remove the bullet "Holdings handling (section 8)".

## 6. V2 ideas from Rama's replies (moved to V2_DESIGN_LOG, PROPOSED)
VP-08 strategy model (max 6 strategies, own YAML and own filtration each, S&R after strategies); VP-09 controlled entry (5-minute candle confirmation, slippage slabs by LTP, release-price rule, exact 10-minute timeout with a watcher); VP-10 named price and time types; VP-11 terminal outcome for every signal; VP-12 one trade per stock per day, no sector/concentration caps; VP-13 capital-per-trade sizing and broker-matching margin accounting; VP-14 EOD "analyse today, trade tomorrow"; VP-15 config changeable during the trading day; VP-16 trading calendar and day lifecycle; VP-17 metrics to research (expectancy, latency, rejection rate, fill rate); VP-18 full legacy audit after all agenda topics. VP-04 and VP-06 extended.

Two considerations recorded with the proposals (not objections):
- VP-12: with no sector caps, a market-wide move can fill several slots with one sector. Discuss at F-007.
- VP-13 and VP-15: capital-based sizing makes the rupee loss per trade vary with SL distance; changing config mid-session needs a rule for which keys may change and how. Discuss at F-004 and A-009.

## 7. Approvals needed

| # | Proposal | Recommendation |
|---|---|---|
| P-1 | A-002_manual.md (revised legacy manual with G-1 to G-6 and X-1, X-2 recorded) becomes the frozen A-002. | Approve after ChatGPT's review |
| P-2 | A-001 erratum (section 5), recorded as a new register row referencing DR-013. | Approve |
| P-3 | VP-18 (full legacy audit after all agenda topics, issues filed under their topics before design) recorded as a process decision, since Rama stated it as the plan. | Approve |

## 8. Disposition after the ChatGPT value-added review (05-Oct-2026)

All twelve points adopted in A-002_manual.md revision 2.

| ChatGPT point | Outcome |
|---|---|
| 1. Strategy count | Adopted. My G-2 wording "16 strategies, each with its own YAML" overclaimed: it combined Rama's recollection and the storage record (which describes the testing VM) into a production FACT. Now UNKNOWN, owned by A-008 (Q-17). |
| 2. Duplicate rule | Adopted: qualified by segment/product context; cross-segment behaviour to A-007. The once-per-stock-per-day rule stays V2 only (VP-12). |
| 3. 15:10 | Adopted: recorded as recollection ("around 15:10"); exact timing to A-003. |
| 4. 5x / 1x | Adopted: context only, not an objective; no universal 5x implied; semantics to A-005, A-006, A-007. |
| 5. Sizing conflict | Adopted: A-002 states it does not resolve the formula (Q-16). |
| 6. Independent pipelines | Adopted: distinct processing paths with shared central components; Rama's "independent pipelines" kept as his description. |
| 7. A-001 erratum | Agreed by ChatGPT; still to be recorded as a new register row on Rama's approval (P-2). |
| 8. Evidence boundary | Kept as written. |
| 9. Known defects | Kept as legacy defects; not turned into V2 requirements. |
| 10. EOD watchlist | Kept; "analyse today, trade tomorrow" explicitly marked VP-14, not legacy. |
| 11. Execution quality | Adopted: controls existed; achievement not implied. |
| 12. Performance targets | Kept as not established. |

Independent check: I tried to read the strategy YAML list directly from the production snapshot at 970aabf, but GitHub's API rate-limited my side. If useful for A-008, this lists the file names in your local reference clone (names only): (Get-ChildItem D:\Projects\_legacy_ref\trading-system-prod-snapshot\config\strategies -File).Name. INTERPRETATION, not fact: the storage record's 16 YAMLs describe the testing VM, which may explain part of the 15-vs-16 difference.
