# A-007 PRODUCT TYPES: REVIEW NOTES

Date: 2026-10-06
Status: CLOSED 2026-10-07 (final). ChatGPT's value-added review applied (section 9). Legacy manual frozen under DR-017, recorded as DR-022 together with the A-006 erratum E-1; V2 inputs remain PROPOSED / OPEN. Q-32 closed from Rama's 06-Oct Q-27 answer. Two section 4 rows corrected on 2026-10-07 for the A-007 erratum E-1 found during A-008 (originals struck through; see the Erratum section of A-007_manual.md; recorded in DR-023).
Place at: docs/agenda/a-007_product-types/A-007_review_notes.md
Method: two layers (V2_DESIGN_LOG W-1). Legacy fact in A-007_manual.md; V2 inputs in A-007_v2_design_inputs.md and V2_DESIGN_LOG. Current broker rules labelled per W-5.
Security: no credential values. Source was read from raw files and a blob-less sparse clone that fetched only the needed paths (config/strategies, orders, core, signals, broker modules named in the manual); docs/web_claude, .env files and other credential-bearing paths were not fetched or opened. The files read contain no credential literals (checked).

## 1. Sources reviewed
- docs/inputs/chatgpt_originals_and_replies/A-007_PRODUCT_TYPES_DISCUSSION_CHECKLIST_27-SEP-2026.txt (Rama's answers)
- docs/inputs/reconciliation/A-007_FINAL_SOURCE_RECONCILIATION_AND_FREEZE_27-SEP-2026.txt
- docs/inputs/manuals/A-007_PRODUCT_TYPES_LEGACY_MANUAL_27-SEP-2026.md
- New evidence at the baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (2026-10-06): the files listed in A-007_manual.md section 2, including all 16 strategy YAMLs and main.py wiring.
- Comparison: testing snapshot config (branch main).
- External (checked 2026-10-06): Zerodha support pages and Z-Connect product updates; Kite Connect forum (list in A-007_v2_design_inputs.md section 14).

## 2. Overall
ChatGPT's set separates product intent from broker code correctly, keeps GTT apart from the products, and honestly marks CO, BO and NRML as unknown from the evidence it had (it notes that live GitHub retrieval was not available). Reading the production source settles most of those unknowns: the order placer always used the LIMIT_TRIPLE protocol, so intraday trades were regular MIS orders with system-placed SL and target, and code comments record that only MIS and CNC were ever placed. This also answers Rama's repeated question (CO or BO?): neither. The main remaining item is a difference between Rama's 27-Sep intraday suggestion (BO/CO) and his 06-Oct Q-27 answer (MIS), now Q-32.

## 3. Findings

| # | Finding | Evidence | Fix |
|---|---|---|---|
| G-1 | The 27-Sep manual carries V2 inputs (section 20) and V2 open questions (section 22, items 9-11) inside the legacy manual. | Two-layer rule W-1 | Moved, with Rama's verbatim notes, to A-007_v2_design_inputs.md; manual keeps a pointer. |
| G-2 | CO, BO and NRML use left UNKNOWN. Source settles it: OrderPlacer always uses its default protocol LIMIT_TRIPLE (OP9; main.py does not override), so CO was never selected; BO's mapping is empty, so it cannot be placed; NRML is not in product_map, so it was never placed. Two code comments record measurements: "CO never used: 805/805 regular" and "SELECT DISTINCT product FROM orders = {MIS, CNC}". | order_placer.py, main.py, product_resolver.py, config, code comments | Manual sections 5, 8, 9, 10: never placed (code path FACT; measurements RECORD-DERIVED). |
| G-3 | The checklist attributes the PRODUCT_TO_INTENT map (with NRML) to broker/product_resolver.py. It lives in core/constants.py; ProductResolver's own reverse lookup is config-driven and has no NRML. | Both files read | Manual section 3. |
| G-4 | "Was ProductResolver the single translation point?" left UNKNOWN. It is the translator for placement, but broker codes are hardcoded elsewhere (core/constants.py twice; order_placer.py _PROTOCOL_TO_PRODUCT; mis_autosquareoff.py MIS_PRODUCT). | Files read | Manual section 3. |
| G-5 | Not mentioned: 13 strategy YAMLs declare order_protocol CO_PLUS_TGT and the declaration has no runtime effect. This is the likely source of the CO/BO uncertainty. | 16 YAMLs read; order_placer.py | Manual sections 4, 5, 20. |
| G-6 | The product gates (trade_type, force_intraday_only, delivery_enabled) appear only as "business controls"; their layered logic, production values, fail-safe schema defaults and the 15-Jun-2026 unintended-CNC incident are missing. | Config and config_loader read | Manual section 4. |
| G-7 | Rama's V2 config proposal (trade_type master; strategy ON/OFF, classification, direction) is presented as new; legacy already had that structure (plus two overlapping switches). | Config, YAMLs | V2 inputs section 6 records the match; proposal stays PROPOSED. |
| G-8 | MIS square-off history missing: until 28-Aug-2026 the only closing pass was 15:17, after the CAS broker square-off (TATAPOWER closed by the broker on 27-Aug, Rs 59 charge); passes moved to 15:03 / 15:06 / 15:09 on 03-Sep; naked-position defect on 02/03-Sep fixed. | mis_autosquareoff.py header | Manual section 6. |
| G-9 | Delivery/GTT detail missing: placer (one GTT, 3% SL limit, partial-fill modify, restart hydrate), monitor action ladder, 15-minute cadence (matches Rama's 10-15 minute manual-exit alert), 50-GTT guard. | cnc_gtt.py, cnc_gtt_monitor.py | Manual section 7. |
| G-10 | NRML is treated differently on the EOD, emergency and shutdown paths (latent; never exercised). | constants.py, eod_squareoff.py, main.py | Manual section 10 (KNOWN AMBIGUITY). |
| G-11 | SmartTgtManager is registered only for CO_PLUS_TGT trades, while 13 intraday YAMLs enable smart target; with LIMIT_TRIPLE always used, the trail probably never ran. | order_placer.py, YAMLs | Manual sections 5, 20; Q-15 note; audit lead Q-31(c). |
| G-12 | The trades table does not persist product; at an exit fill it is derived from the protocol (LIMIT_TRIPLE -> MIS), and delivery trades also ran LIMIT_TRIPLE. | order_placer.py | Manual section 13; audit lead Q-31(b). |
| G-13 | Legacy concluded on 15-Jun-2026 that "Zerodha rejects SL-M orders via the API entirely"; the current Kite rule rejects SL-M only when market protection is 0, and legacy sent market protection only from 10-Sep-2026. | order_protocol_limit.py; Kite forum | Manual section 11; audit lead Q-31(a); V2 inputs section 3 (G-010). |
| G-14 | Rama's 27-Sep intraday suggestion (BO/CO "whichever supports automated simultaneous TGT & SL") meets current facts: BO discontinued since March 2020; CO carries the stoploss only. His 06-Oct Q-27 answer chose MIS. | Zerodha support; A-006 Q-27 | V2 inputs sections 1, 3; Q-32 for Rama. |
| G-15 | Precision of a current fact: the CAS-stock MIS square-off was 3:10 PM when CAS began (3-Aug-2026) and was revised to 3:12 PM (Zerodha product update, 10-Aug-2026). Q-18 and the frozen A-006 manual said "3:12 PM from 3-Aug-2026". The legacy code itself recorded both figures. | Z-Connect 10-Aug-2026; mis_autosquareoff.py | Erratum added to A-006_manual.md (struck-through line kept); A-006 V2 inputs corrected; Q-18 note; lesson. |
| G-16 | Q-17 (strategy-YAML count) can be answered from the production snapshot: 16 YAMLs (13 intraday, 12 of them enabled; 3 delivery). | config/strategies listing | Manual section 4; Q-17 closed (A-008 reviews content). |
| G-17 | Testing comparison asked for: the testing config has the same product keys and values; testing YAMLs were not compared. | sbx config | Manual section 18. |

## 4. Rama's UNKNOWN answers, now answered from source

| Checklist item | Rama | Source answer (manual section) |
|---|---|---|
| 3 CO actually traded | UNKNOWN | No: never placed (5, 8) |
| 3 BO actually traded | UNKNOWN | No: not placeable (9) |
| 3 NRML actually traded | UNKNOWN ("probably never") | No: never placed by the system (10) |
| 4 Semantic vs broker distinction intentional | UNKNOWN | Yes: locked design PR1-PR10 (3) |
| 4 ProductResolver single translation point | UNKNOWN | For placement yes; not for reverse maps and filters (3) |
| 4 Any module supplied codes directly | UNKNOWN | Yes: constants, order_placer, mis_autosquareoff (3) |
| 5 Authoritative source / how selected | (blank) | Strategy intent, gated by trade_type; ~~force_intraday_only could rewrite at load~~ force_intraday_only made delivery strategies dormant (no load-time rewrite since 10-Jul-2026; erratum E-1) (4) |
| 5 Product change during processing | UNKNOWN | ~~Only force_intraday_only at load;~~ None at the baseline except the adapter's coercion to MIS when force_intraday_only is true (erratum E-1); none per signal found (4) |
| 8 CO intraday-only, broker SL, bracket-aware exit, EOD | YES (guess) | Code agrees (CO SL built in; cancel variety co; EOD exits CO) but CO never ran (8) |
| 11 Several entry types in one product | UNKNOWN | Yes for MIS: LIMIT, MARKET for the retest entry (11) |
| 12 Variety part of product handling; distinct concepts | UNKNOWN | Distinct; variety co only for CO, regular otherwise (11) |
| 17 Eligibility before reservation / before submit; broker rejection of a valid product | UNKNOWN | No advance MIS check; reservation first; broker MIS blocks happened and were recorded, screener drop in shadow (15) |
| 19 Product changes over time; CO/BO toggled | UNKNOWN | Timeline (17); CO/BO never enabled at runtime |
| 20 Ambiguities 1-9 | "YES, I guess" | Each confirmed or refined in sections 3-15 |

## 5. Points for Rama (Q-32 closed 2026-10-07 from Q-27; see section 9)
- Q-32: your 27-Sep note asked for BO/CO "whichever supports automated simultaneous TGT & SL"; your 06-Oct Q-27 answer chose MIS. BO no longer exists, CO attaches only the stoploss, and the legacy ran intraday as regular MIS orders with system-placed SL and target. Confirm the V2 reading: MIS, with the system placing SL and target straight after the entry fill (V2 inputs section 3).
- Nothing else needs your answer for the legacy manual to freeze.

## 6. Cross-topic evidence found while verifying A-007
- A-003: the 15:03 / 15:06 square-off passes date from 28-Aug / 03-Sep-2026; before that the 15:17 routine was the only closing pass. This may explain Rama's recollection of checks at 15:16-15:18 (A-003 section 14 left it open): audit item Q-31(e).
- A-006: CAS square-off precision (G-15) handled by an erratum.
- A-008: Q-17 answered (16 YAMLs, names and intents in A-007_manual.md section 4).
- Q-15 (smart target): wired only to CO trades, so probably never ran (G-11).
- Contract notes (Rama has them) could confirm that only MIS and CNC were traded: optional audit item Q-31(d).

## 7. V2 items (to V2_DESIGN_LOG, PROPOSED)
New VP-41 (intraday mechanism: MIS with system-placed SL and target), VP-42 (product controls: config master, strategy secondary), VP-43 (product data integrity). Extended: VP-12 (restated), VP-21 (per-category broker times; design to the earliest), VP-37 (two product paths only). New Q-31 (audit leads), Q-32 (Rama). Q-17 closed; Q-15 and Q-18 annotated. Two lessons. Full text in A-007_v2_design_inputs.md.

## 8. Next step
Send these files to ChatGPT: A-007_manual.md, A-007_v2_design_inputs.md, this file, the corrected A-006_manual.md and A-006_v2_design_inputs.md, and V2_DESIGN_LOG.md (with Rama's answer to Q-32 if ready). After its review: A-007 FROZEN, register rows for the A-007 freeze and the A-006 erratum, and the commit/push commands, per DR-017.

## 9. Disposition after ChatGPT's review (07-Oct-2026)

ChatGPT's review (docs/agenda/a-007_product-types/A-007_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_07-OCT-2026.txt) called the source reconciliation strong, recommended no change to the established legacy facts, and asked only for consistency edits before the freeze. It left no question for Rama: it closed Q-32 from Rama's later 06-Oct Q-27 answer, which Claude accepts (the product intent is settled there; the leg mechanics belong to G/H). All points applied:

| ChatGPT point | Outcome |
|---|---|
| 1. Legacy manual: keep the established facts; evidence-disciplined wording for delivery | No legacy fact changed. Delivery described as "the legacy delivery execution path used CNC and, after the entry fill, constructed one broker-side OCO GTT"; evidence-boundary line added (individual GTT placements cannot be checked); intraday described as the baseline path plus recorded measurements; CO "never selected by the production placement path"; NRML "not a placement product" (manual sections 5, 7, 8, 10-12, 17, 22). |
| 2. Freeze status | Manual FROZEN 2026-10-07 (revision 1, DR-022); these notes CLOSED. |
| 3. Q-32 | Closed 2026-10-07 with ChatGPT's wording; VP-41 stays PROPOSED; SL order type, protective-leg state machine, retry/escalation and timing stay OPEN for G/H. |
| 4. MIS eligibility, three levels | Recorded in V2 inputs section 3 and VP-41: strategy intent MIS; current broker/instrument eligibility check; order-time broker acceptance as final authority; reject, never convert. |
| 5. Current broker facts labelled | Already labelled CURRENT EXTERNAL BROKER FACT (checked 2026-10-06) per W-5; references kept. |
| 6. CAS / square-off | E-1 distinction kept (3:10 PM at launch, 3:12 PM since 10-Aug-2026, 3:25 PM for others); the legacy 15:17 to 15:03 / 15:06 / 15:09 evolution stays documented separately (manual sections 6, 17). |
| 7. CO / BO / NRML | Wording aligned: CO supported by code/config but never selected; BO mapping empty, not placeable; NRML not a placement product, with the latent handling inconsistency kept as a legacy ambiguity. |
| 8. Smart target | Kept as "probably never engaged" / audit lead; no claim that it never executed. |
| 9. SL-M | Kept as an audit lead; the legacy conclusion is quoted, not corrected; current rule worded "where applicable". |
| 10. V2 boundary | V2 inputs section 15 lists the items that stay PROPOSED / OPEN and where each is decided. |
| 11. Design log | Q-32 CLOSED; VP-41 PROPOSED; no register row for Q-32; A-007 legacy manual frozen; Q-17 answered by A-007 evidence while the strategy-model review stays with A-008. |
| 12. A-006 erratum | E-1 kept visible (original line struck through); recorded in DR-022. |

Register: one row, DR-022, records the freeze of the legacy record under DR-017 (as for A-001 to A-006, and as point 2's "FROZEN (DR-0xx)" expects) together with the A-006 erratum E-1. No V2 decision is entered, matching ChatGPT's "no new decision required".
