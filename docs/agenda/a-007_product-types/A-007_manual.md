# A-007 PRODUCT TYPES: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-07 (revision 1, DR-022). ChatGPT's value-added review (07-Oct-2026) changed no legacy fact; it asked for evidence-disciplined wording (code path versus recorded use) and the freeze status. Erratum E-1 to the A-006 manual is recorded in the same register row.
Date: 2026-10-06
Layer: LEGACY ONLY. Which broker products the legacy system defined, how it chose them and how it actually placed them. Rama's new-system suggestions from the A-007 discussion are NOT here: they are kept in full in docs/agenda/a-007_product-types/A-007_v2_design_inputs.md (PROPOSED) and summarised in docs/governance/V2_DESIGN_LOG.md.
Working copy of: docs/inputs/manuals/A-007_PRODUCT_TYPES_LEGACY_MANUAL_27-SEP-2026.md (original untouched; its sections 20-22 held V2 inputs and open questions). Changes are listed at the end.
Labels: FACT (SOURCE-VERIFIED) = read at the baseline commit on 2026-10-06; FACT (RECORD-DERIVED) = baseline document, earlier manuals, or a measurement recorded in a production code comment that cannot be re-run (the legacy database is gone, A-001); HISTORICAL (Rama); UNKNOWN / NOT ESTABLISHED; INTERPRETATION / AUDIT LEAD; CURRENT EXTERNAL BROKER FACT (a current broker, exchange or regulator rule, with the date it was checked; never evidence of legacy behaviour; V2_DESIGN_LOG W-5); KNOWN DEFECT / AMBIGUITY.
Source priority: production source/config > production records/manuals > sandbox evidence > user recollection > interpretation.
Security: no credential values. Credential-bearing paths were not fetched.

## 1. Purpose and boundary
Document the legacy product model: semantic intents and broker product codes, how a strategy's product was chosen and gated, which entry/exit protocol each product actually used, how each product was squared off or protected, and what the evidence shows about actual use of MIS, CNC, CO, BO and NRML.

## 2. Evidence
- Source-verified on 2026-10-06 at the baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (public production repository; raw files, plus a blob-less sparse clone that fetched only the paths listed; docs/web_claude and other credential-bearing paths were not fetched or opened): broker/product_resolver.py; core/constants.py; core/config_loader.py (product-gate schema); config/system_config.yaml (product_map, leverage_map, trade_type, force_intraday_only, delivery_enabled, capital buckets, eod_squareoff, mis_filter); config/strategies/*.yaml (all 16: name, direction, intent, enabled, order_protocol, smart_tgt_enabled); main.py (wiring); orders/full_entry_engine.py; orders/order_protocol_co.py; orders/order_protocol_limit.py (header); orders/order_placer.py (protocol choice, product derivation, fill handling); orders/eod_squareoff.py (product rules); orders/mis_autosquareoff.py (header); orders/cnc_gtt.py and orders/cnc_gtt_monitor.py (headers); signals/signal_processor.py (strategy control, retest MARKET entry).
- Record-derived: measurements recorded in those code comments (named where used); docs/inputs/legacy_baseline/CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt; frozen A-003 to A-006.
- Comparison: testing snapshot config (branch main, fetched 2026-10-06): the same product keys and values as production. Testing strategy YAMLs were not compared.
- Not read: capital/kill_switch.py, orders/order_reconciler.py, orders/smart_tgt_manager.py, broker/cost_calculator.py. Claims about them come from comments in the files read and are marked.
- User evidence: Rama's answers in the 27-Sep checklist.

## 3. Product model: intents and broker codes (FACT, SOURCE-VERIFIED)
- Four semantic intents (ProductResolver, PR2): INTRADAY, DELIVERY, COVER_ORDER, BRACKET_ORDER.
- Forward map, from config product_map (zerodha): INTRADAY -> MIS; DELIVERY -> CNC; COVER_ORDER -> CO ("Cover Order -- bracket with built-in SL"); BRACKET_ORDER -> "" ("Zerodha discontinued BO; empty = not supported"). Resolving an intent with an empty code raises ProductNotSupportedError (PR10: no silent fallbacks).
- Two reverse maps exist:
  - ProductResolver.intent_for(): config-driven, so it knows MIS, CNC and CO only ("used by order_reconciler").
  - core/constants.py PRODUCT_TO_INTENT, hardcoded: MIS -> INTRADAY, CO -> COVER_ORDER, CNC -> DELIVERY, NRML -> DELIVERY ("FIX-166 F17: canonical copy now in core.constants"; imported by order_placer.py and cnc_gtt_monitor.py).
  - Correction to the 27-Sep set: PRODUCT_TO_INTENT lives in core/constants.py, not in broker/product_resolver.py.
- Single translation point? ProductResolver is the intent-to-code translator for order placement (its docstring: "Eliminates hardcoded 'MIS'/'CNC'/'CO' strings from the rest of the codebase", audit Issue #29). It is not the only place broker codes appear: core/constants.py also holds EMERGENCY_FLATTEN_PRODUCTS = {MIS, CO} (read by five call sites: kill_switch x2, eod_squareoff x2, order_reconciler x1); orders/order_placer.py holds _PROTOCOL_TO_PRODUCT (CO_PLUS_TGT -> CO, LIMIT_TRIPLE -> MIS); orders/mis_autosquareoff.py holds MIS_PRODUCT = "MIS".
- GTT is not a product: it appears in neither the intents nor product_map. GTT legs carry product CNC (section 7).

## 4. How a strategy's product was chosen and gated (FACT, SOURCE-VERIFIED)
- Each strategy YAML declares name, direction (LONG or SHORT), intent (INTRADAY or DELIVERY), enabled and order_protocol. One intent per strategy; no per-signal product choice was found. The only intent rewrite is force_intraday_only at load time.
- Production strategy YAMLs at 970aabf (16 files):

| Strategy | Direction | Intent | Enabled | order_protocol declared |
|---|---|---|---|---|
| first_pullback_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| first_pullback_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| gap_fade_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| gap_fade_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| gap_go_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| gap_go_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| open_high_breakdown_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| open_low_breakout_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| pb01_breakout_retest | LONG | INTRADAY | false ("FAIL-CLOSED: never trades until the spec-13 promotion gate") | CO_PLUS_TGT |
| range_breakout_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| range_breakout_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| vwap_bounce_long | LONG | INTRADAY | true | CO_PLUS_TGT |
| vwap_rejection_short | SHORT | INTRADAY | true | CO_PLUS_TGT |
| positional_momentum_long | LONG | DELIVERY | true | LIMIT_TRIPLE |
| positional_sector_rotation | LONG | DELIVERY | true | LIMIT_TRIPLE |
| positional_swing_long | LONG | DELIVERY | true | LIMIT_TRIPLE |

- Three config controls, layered (system_config.yaml and core/config_loader.py):
  - trade_type ("Slice 2, LAYER 1 - master product gate"): INTRADAY, DELIVERY or BOTH; gates which strategies may trade and never rewrites intent; a strategy whose intent does not match is rejected as REJECTED_TRADE_TYPE at the strategy-control check (signal_processor via strategy_will_trade). Production: BOTH. Schema default: INTRADAY.
  - force_intraday_only ("P0 2026-06-15: MIS-only safety"): when true, every strategy is forced to intent INTRADAY at load. Production: false. Schema default: true. The config auditor blocks start-up on the force_intraday_only + DELIVERY contradiction.
  - delivery_enabled ("SLICE2.5-P1: master CNC/delivery capability lock"): a real CNC order or its OCO-GTT is placed only when delivery_enabled = true AND force_intraday_only = false AND trade_type is DELIVERY or BOTH; enforced at the broker boundary (the adapter refuses CNC/GTT otherwise). Production: true. Schema default: false.
- Capital: intraday bucket 70% ("MIS/CO"), positional bucket 30% ("CNC"); leverage_map INTRADAY 5.0, COVER_ORDER 6.0, DELIVERY 1.0 ("PINNED to exactly 1.0"), BRACKET_ORDER 5.0; leverage safety bounds 1.0 to 10.0. Live broker margin with static fallback: A-005.
- FACT (RECORD-DERIVED, config comment): on the first live day, 15-Jun-2026, HARIOMPIPE and SETL orders "went CNC"; force_intraday_only was added as a MIS-only safety. Later comments (22-Aug-2026) record force_intraday_only false, delivery_enabled true, trade_type BOTH "and delivery has traded".

## 5. Order protocols: how products were actually placed
- FACT (SOURCE-VERIFIED): two entry protocols exist (FullEntryEngine routes per call):
  - LIMIT_TRIPLE (order_protocol_limit.py): phase 1 ENTRY LIMIT; phase 2, after the entry fills, SL and TGT on the exit side sized to the ACTUAL filled quantity (naked-short fix, 24-Apr-2026); SL first, and no TGT if the SL fails. Every SL leg is a stop-limit order with the limit 0.5% beyond the trigger. Its docstring calls it "the P8/P13 fallback protocol used when Cover Orders are not available for a stock, or as the v2 default before CO is validated".
  - CO_PLUS_TGT (order_protocol_co.py): a CO entry (variety "co", order_type "SL", trigger = SL price; the broker holds the SL) and, after the fill, a separate TGT LIMIT. Its docstring says "Per P8/P13: CO is the default protocol for intraday strategies".
- FACT (SOURCE-VERIFIED): the declared YAML order_protocol was not used. OrderPlacer always takes its own default ("OP9 -- order_protocol determined by intent ... LIMIT_TRIPLE by default"; code: order_protocol = self._default_protocol), and main.py builds OrderPlacer and FullEntryEngine without overriding that default. At the baseline the placement path selected LIMIT_TRIPLE for every trade, intraday and delivery.
- FACT (RECORD-DERIVED, production code comments): "CO never used: 805/805 regular" and "CO is declared by 12/15 YAMLs and discarded at :949 (X6)" (order_protocol_co.py); "CO has never been placed in production (SELECT DISTINCT product FROM orders = {MIS, CNC})" (mis_autosquareoff.py). The query date and database are not stated, and the database cannot be re-queried.
- Conclusion for Rama's question (CO or BO for intraday?): neither. The baseline intraday path placed regular-variety MIS orders: a LIMIT entry (MARKET only for the S&R retest confirmation entry, SNR-V2), then the system itself placed the stop-limit SL and the LIMIT target as soon as the entry filled, with software OCO cancelling the other leg (A-006 manual section 10). The recorded measurements (805/805 regular; orders-table products {MIS, CNC}) agree for the orders they covered. This matches the "TGT and SL automatically created by the system" that Rama remembers.
- The legacy delivery execution path used the same LIMIT_TRIPLE entry with product CNC (from intent DELIVERY) and, after the entry fill, FullEntryEngine constructed one broker-side OCO GTT for the delivery position ("the SINGLE centralized gate", section 7).
- FACT (SOURCE-VERIFIED): protocol-specific helpers: BreakevenManager is registered for LIMIT_TRIPLE trades only when the strategy object has trailing_sl_enabled (no production YAML sets the key; its schema default was not read); SmartTgtManager is registered only for CO_PLUS_TGT trades (BL-7d), although the 13 intraday YAMLs set smart_tgt_enabled: true and the 3 delivery YAMLs false.
- INTERPRETATION / AUDIT LEAD: since no trade ran CO_PLUS_TGT, the smart target trail probably never engaged in production (Q-15, Q-31).

## 6. INTRADAY / MIS
- FACT (SOURCE-VERIFIED): MIS = INTRADAY; intraday capital bucket; member of EMERGENCY_FLATTEN_PRODUCTS; the EOD square-off exits MIS and CO positions only (EOD6).
- FACT (SOURCE-VERIFIED): the MIS auto-square-off unit (orders/mis_autosquareoff.py, dated 28-Aug-2026) is MIS-only (it deliberately does not inherit CO from the emergency set). PASS 1 at 15:03 (LIMIT_THEN_MARKET, protected band 1.5%), PASS 2 at 15:06 (MARKET, protection 2.5%, never any grace), verify/restore by 15:08, internal cutoff 15:09. Role labels in the code: 15:09 configured protective cutoff; 15:15 SOFT_KILL (closes nothing); 15:17 EOD accounting and emergency backstop, not the primary square-off. One poll thread runs both passes in sequence so two exits can never be in flight for one symbol.
- FACT (RECORD-DERIVED, code comments):
  - Before 28-Aug-2026 the only position-closing pass ran at 15:17, "FIVE MINUTES LATE for any CAS stock". On 27-Aug-2026 TATAPOWER (a CAS stock) was closed by the broker at 15:12:47 with a Rs 59 "Call and Trade charges (Auto Square Off)" debit (Rs 50 + 18% GST).
  - Schedule moved on 03-Sep-2026 from 15:07 / 15:10 (cutoff 15:12) to 15:03 / 15:06 (cutoff 15:09), because Zerodha published two CAS figures (15:12 on the support page, 15:10 in a 03-Aug-2026 post). Rule adopted in the code: "never design to the LATER of two conflicting broker deadlines". Known cost recorded: 15:09 is universal and gives up about 16 minutes on non-CAS stocks (broker 15:25); a per-symbol CAS/non-CAS deadline is named as the correct end state.
  - The PASS bands (1.5% / 2.5%) were chosen by Rama on 10-Sep-2026 from a measured table of "actual-held MIS, n=289".
- KNOWN DEFECT (fixed 03/04-Sep-2026, RECORD-DERIVED): the unit cancelled the protective orders first; a failed exit then left the position with no stop. Measured twice: 02-Sep COALINDIA and 03-Sep ANANTRAJ ("stayed naked until a human closed it"). Fixes: F1 re-places the SL leg on every failure path; F2 polls cancel status to a bounded deadline; F1b bounds the restore check.
- FACT (SOURCE-VERIFIED): MIS eligibility was learned, not checked in advance: the broker's "MIS orders are currently blocked" 400 is always recorded in a learned blocklist; the screener drop is in shadow mode (logged only; "do not set shadow: false without Rama"). Config comment, live: PYRAMID was MIS-blocked twice on one day (learned 10:06, same order placed again 10:11). Details: A-004 section 6.
- HISTORICAL (Rama): MIS was actually traded; intraday positions were meant to close the same day; intraday margin differed by stock.
- Cross-reference A-003: the 15:03 / 15:06 passes date from 28-Aug / 03-Sep-2026; before then the 15:17 routine was the only closing pass. This may explain Rama's recollection of checks around 15:16-15:18, which A-003 section 14 left unrelated to the configured pre-cutoff checks (audit item, Q-31).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): CAS began on 3-Aug-2026; Zerodha's product update of 10-Aug-2026 says the MIS auto square-off for CAS stocks was revised to 3:12 PM from 3:10 PM; the support page shows 3:12 PM for CAS stocks and 3:25 PM for other stocks.

## 7. DELIVERY / CNC with OCO GTT
- FACT (SOURCE-VERIFIED): CNC = DELIVERY. CNC is deliberately outside EMERGENCY_FLATTEN_PRODUCTS, so a delivery position survives HARD_KILL ("the Q4 invariant is 'no live INTRADAY position', NOT 'no live broker position' (Rama, 30-Jul)"); the EOD square-off never touches CNC (EOD6).
- FACT (SOURCE-VERIFIED): the legacy delivery execution path used CNC and, after the entry fill, constructed ONE broker-side two-leg OCO GTT (product CNC) for the delivery position (orders/cnc_gtt.py, SLICE2.5-P1, 25-Jun-2026). SL trigger = the strategy SL; TGT trigger = the fill-recalculated target; the SL leg's limit sits 3% below its trigger so an overnight gap-down can still fill; the TGT leg's limit sits a small offset below its trigger. A minimum trigger distance of 0.25% from LTP is validated up front ("the conservative public figure"). Static SL/TGT only (delivery YAMLs have smart_tgt_enabled: false). A later partial fill modifies the existing GTT (one GTT per trade). On restart the GTT cache is rebuilt from the durable gtt_state so a surviving GTT is modified, never duplicated.
- Evidence boundary: the points above describe the code path. Individual historical GTT placements cannot be checked (the database is gone); delivery use rests on code comments and Rama's account.
- FACT (SOURCE-VERIFIED): orders/cnc_gtt_monitor.py (SLICE2.5-P2, 25-Jun-2026) reconciles each GTT at start-up and every 15 minutes in market hours; the broker GTT is the authority, gtt_state the local mirror. Actions: healthy (GTT active, holding, quantity match); GTT triggered and holding flat: GTT_EXIT, the primary, system-owned delivery exit; triggered but still held: CRITICAL re-protect; GTT missing but held: auto-recreate in hours or queue pre-open; quantity mismatch: CRITICAL once, cancel the wrong GTT, no recreate; GTT active but holding flat (for example a manual sale): orphan, forensic log, delete GTT, finalise; more than one active GTT for a trade: SOFT_KILL. A broker read failure defers the cycle and alerts; it never treats "no data" as "no positions". A cap guard assumes "Kite allows ~50 active GTTs per account" (warn at 45).
- FACT (SOURCE-VERIFIED, main.py): for shutdown with carried holdings, only CNC counts as protected overnight.
- FACT (RECORD-DERIVED, code comments): "delivery has traded" (22/23-Aug-2026); orders table products {MIS, CNC}.
- HISTORICAL (Rama): CNC/delivery was traded and worked; GTT worked; positions stayed overnight; a manual exit was detected and confirmed by Telegram/email within about 10-15 minutes (consistent with the 15-minute monitor).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): an account can have up to 500 active GTTs (Zerodha support); the legacy guard's ~50 figure is lower.

## 8. COVER ORDER / CO
- FACT (SOURCE-VERIFIED): CO maps to COVER_ORDER; leverage 6.0; intraday bucket; member of EMERGENCY_FLATTEN_PRODUCTS. CO-specific exit: cancel_order(variety="co") on the entry bracket ("Audit 3.1": a CO cannot be squared off with a reverse order; the code notes Zerodha rejects that and auto-squares with a charge). The order reconciler refuses a CO position in a local branch (constants comment).
- Supported by code and config but never selected by the production placement path: at the baseline OrderPlacer never selects CO_PLUS_TGT (FACT, SOURCE-VERIFIED, section 5), and the code comments record 805/805 regular orders and orders-table products {MIS, CNC} (RECORD-DERIVED), so no CO placement is recorded. Thirteen YAMLs declare CO_PLUS_TGT with no effect.
- HISTORICAL (Rama): intraday SL and target were created automatically after entry; he could not recall whether that was CO or BO. Section 5 explains it: the system placed both legs on regular MIS orders.

## 9. BRACKET ORDER / BO
- FACT (SOURCE-VERIFIED): the intent exists and leverage_map has an inert 5.0 entry, but the mapping is empty, so resolving it raises ProductNotSupportedError. BO could not be placed at the baseline; it is absent from the recorded orders-table products (RECORD-DERIVED).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Zerodha discontinued bracket orders in March 2020 and they remain unavailable (Zerodha support).

## 10. NRML
- FACT (SOURCE-VERIFIED): NRML is not in product_map, so it was not a placement product: the placement path could not produce it (main.py comment: "this system never PLACES NRML ... an NRML position can only arrive from outside"). PRODUCT_TO_INTENT maps NRML -> DELIVERY for reverse lookups.
- KNOWN AMBIGUITY (latent, never exercised): paths treat NRML differently. The EOD square-off filters broker positions to {MIS, CO} and its comment calls "CNC/NRML" delivery that is never touched; the emergency (HARD_KILL) sites flatten any product outside {MIS, CO} that is not CNC, including NRML, "LOUDLY (CRITICAL)"; the shutdown check counts only CNC as protected.
- HISTORICAL (Rama): NRML was never used; F&O is out of scope (no Chartink F&O signals).

## 11. Product, order type and variety (FACT, SOURCE-VERIFIED)

| Product | Entry | Exit legs | Variety |
|---|---|---|---|
| MIS (LIMIT_TRIPLE; used per records and Rama) | LIMIT; MARKET only for the S&R retest confirmation entry | After the fill: SL stop-limit (limit 0.5% beyond trigger) + TGT LIMIT, software OCO; square-off passes: protected LIMIT then MARKET; kill exits: marketable LIMIT at LTP +/- 1% | regular |
| CNC (LIMIT_TRIPLE entry; used per records and Rama) | LIMIT | One OCO GTT with two SELL LIMIT legs (product CNC) | regular entry; GTT |
| CO (CO_PLUS_TGT; never selected) | order_type SL with trigger = SL | Built-in SL + separate TGT LIMIT; exit by cancel_order(variety="co") | co |
| BO, NRML | Not placeable | - | - |

- The adapter accepts MARKET, LIMIT, SL, SL-M and the varieties regular and co (A-006 manual section 5). From the first live day (15-Jun-2026) every SL leg was a stop-limit: "Zerodha rejects SL-M orders via the API entirely (not just for CNC)" (order_protocol_limit.py comment).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): a Kite forum post by Zerodha staff (25-Mar, edited 30-Mar; enforcement from 1 April) says market orders and SL-M orders with market protection 0 are rejected; SL-M itself is not blocked on NSE equity. NSE stopped SL-M for options from 27-Sep-2021; BSE blocked SL-M (circular 21-Sep-2023).
- INTERPRETATION / AUDIT LEAD: the legacy sent market protection only from 10-Sep-2026 (A-006 manual section 5), so the June SL-M rejections may have come from the missing market protection rather than SL-M being unavailable. Not established (Q-31).

## 12. Product and exit/protection summary (FACT)

| Product | Same day | EOD square-off | Emergency (HARD_KILL) | Overnight |
|---|---|---|---|---|
| MIS | Yes | 15:03 / 15:06 passes, 15:17 backstop | Flattened | No (broker squares off) |
| CO | Yes (never selected) | Exited by cancel variety "co" | Member of the flatten set | No |
| CNC | No | Never touched | Survives | Protected by one OCO GTT |
| BO | Not placeable | - | - | - |
| NRML | Not a placement product | Not touched (filter) | Flattened as unknown, CRITICAL | Not counted as protected |

## 13. Position identity
- FACT (SOURCE-VERIFIED, A-005 section 7): one_trade_per_symbol_direction_per_day: true; intraday and delivery books are separate (intent, bucket, flatten set).
- HISTORICAL (Rama): "One Symbol, One Segment (Intraday/GTT), One Direction": the same symbol could be held as MIS and as CNC at the same time.
- FACT (SOURCE-VERIFIED, order_placer.py comment): "The trades table does not persist product"; at an exit fill the product is derived from the cached protocol (LIMIT_TRIPLE -> MIS, CO_PLUS_TGT -> CO) for costs and capital release.
- INTERPRETATION / AUDIT LEAD: delivery trades also ran LIMIT_TRIPLE, so a delivery exit passing through that path would be costed and released as MIS. GTT exits are finalised by the CNC GTT monitor instead; whether any delivery exit used the order-placer path is not established (Q-31).

## 14. Paper versus live
- FACT (SOURCE-VERIFIED): the adapter has live and paper paths; paper backs GTTs and holdings with in-memory stores. A recorded blind spot (26-Jul-2026): after a nightly restart, paper produced a clean GTT_EXIT for a position that was never held.
- HISTORICAL (Rama): paper and live behaved differently (prices, times and other bugs). Legacy only (DR-001).

## 15. Product validation and eligibility (FACT, SOURCE-VERIFIED unless marked)
- Sequence at the baseline: strategy control at the entry gate (enabled; trade_type x intent) > screening (MIS learned blocklist, shadow) > OrderPlacer (R:R gate, protocol default, margin reservation by intent through FundManager.required_margin) > adapter (delivery_enabled guard; force_intraday_only coercion; product resolution; tick snapping) > broker.
- A configured-valid product could still be rejected by the broker (MIS block, 400). The rejection was recorded and the reservation released on placement failure (OP7); because the screener drop stayed in shadow, the same symbol could be sent again the same day (PYRAMID).
- No advance broker eligibility check for MIS was found (Step 0, 30-Jun: "no clean MIS source exists", config comment).

## 16. Configuration controls
- FACT: trade_type, force_intraday_only, delivery_enabled, product_map, leverage_map and the strategy fields (intent, direction, enabled, order_protocol, smart_tgt_enabled) are the product-related controls (sections 4-5). order_protocol in the YAMLs had no runtime effect.
- HISTORICAL (Rama): all are business controls.

## 17. Timeline of product changes (FACT, RECORD-DERIVED from dated code comments)
- 24-Apr-2026: exits placed only after the entry fill (naked-short fix).
- 15-Jun-2026 (first live day): unintended CNC orders (HARIOMPIPE, SETL) led to force_intraday_only; SL-M replaced by stop-limit SL legs.
- 25-Jun-2026: delivery lifecycle (OCO GTT placer and monitor; delivery_enabled lock).
- 30-Jul / 02-Aug-2026: emergency flatten set fixed as {MIS, CO}, one shared source; CNC survives the kill.
- 22/23-Aug-2026: comments record that delivery has traded.
- 27-Aug-2026: broker auto-square-off of TATAPOWER (CAS stock).
- 28-Aug-2026: MIS auto-square-off unit added; 03-Sep-2026 schedule moved to 15:03 / 15:06 / 15:09 with F1/F2 fixes; 04-Sep F1b.
- 10-Sep-2026: market-protection bands for the square-off passes; market protection added to the adapter.
- At the baseline CO was never selected and BO could not be placed; no CO or BO placement is recorded.

## 18. Production versus testing
- The testing snapshot config (branch main) has the same product keys and values (force_intraday_only false, trade_type BOTH, delivery_enabled true, same product_map and leverage_map). Testing strategy YAMLs were not compared. No testing difference overrides production.

## 19. Unknown / not established
- The date and database of the recorded orders-table query and of the "805/805" count; the X6 audit record itself.
- Which other callers use each reverse map.
- Whether any delivery exit went through the order-placer exit-fill path.
- Products of trades placed outside the system (manual trades).
- Rama has the contract notes, which could confirm the products traded (optional audit).

## 20. Known defects / ambiguities
- Declared but inert: 13 YAMLs declare CO_PLUS_TGT; the runtime always used LIMIT_TRIPLE.
- SmartTgtManager wired only to CO trades, while the intraday YAMLs enable smart target: probably never engaged (audit lead, Q-15).
- Two reverse product maps (config-driven and hardcoded), and inconsistent NRML handling across EOD, emergency and shutdown paths (latent).
- Product not persisted on trades; derived from protocol at exit (audit lead for delivery).
- Until 28-Aug-2026 the MIS square-off ran after the CAS broker square-off; 02/03-Sep naked positions in the square-off unit (both fixed).
- GTT cap guard of ~50 against a current broker limit of 500 (conservative; external fact).

## 21. Cross-references
Capital buckets and margin: A-005. Broker adapter, order states, rate limits, token: A-006. Instruments, tick sizes, MIS blocklist: A-004. Hours and square-off times: A-003 (Q-31 item). Strategy model: A-008 (Q-17). Configuration: A-009. Order execution: section G. Position management, HARD_KILL, square-off: sections H and I. V2 inputs from this discussion: A-007_v2_design_inputs.md and V2_DESIGN_LOG.

## 22. Final A-007 statement
> The legacy system kept a semantic product layer (INTRADAY, DELIVERY, COVER_ORDER, BRACKET_ORDER) translated by ProductResolver into Zerodha codes (MIS, CNC, CO; BO unsupported), with NRML known only as a reverse mapping. Each strategy YAML fixed one intent and one direction, and three config gates (trade_type, force_intraday_only, delivery_enabled) decided which products could trade. Although thirteen intraday YAMLs declared the CO protocol, the order placer always selected LIMIT_TRIPLE: the intraday path placed regular MIS orders whose stop-limit SL and LIMIT target the system placed after the fill, and the delivery execution path used CNC and, after the entry fill, constructed one broker-side OCO GTT, reconciled every 15 minutes. Code comments record only MIS and CNC in the orders table. The MIS square-off ran before the broker's (15:03 / 15:06 passes since 28-Aug / 03-Sep-2026; a 15:17 pass before that), CNC positions were excluded from EOD and emergency flattening, CO was never selected, and BO and NRML were not placement products.

## Changes from the original (27-Sep-2026)
Moved out (kept in full in A-007_v2_design_inputs.md): section 20 (future V2 inputs), V2 parts of section 21 (cross-topic capture) and V2 questions in section 22 (final V2 intraday mechanism, square-off timing, config precedence). Corrected from direct source reads: CO, BO and NRML use (UNKNOWN in the original) now established as never placed (code path at the baseline plus recorded measurements); PRODUCT_TO_INTENT located in core/constants.py; ProductResolver shown not to be the only place broker codes appear. Added: the 16-strategy table; the three layered product gates with production values and schema defaults; the inert YAML order_protocol; LIMIT_TRIPLE and CO_PLUS_TGT mechanics; protocol-specific helpers and the smart-target lead; the MIS square-off unit, its history and the 02/03-Sep defect; GTT placer and monitor behaviour; NRML path inconsistency; product/order-type matrix; the SL-M history and market-protection lead; the product-not-persisted lead; the dated timeline; current external broker facts (BO March 2020, CAS times, SL-M rules, 500 GTTs).
After ChatGPT's value-added review (07-Oct-2026): no legacy fact changed. Wording made evidence-disciplined: code paths are described as paths ("the legacy delivery execution path used CNC and, after the entry fill, constructed one broker-side OCO GTT"), recorded use is attributed to code comments and Rama, CO is "never selected by the production placement path", NRML is "not a placement product"; evidence-boundary line added in section 7; status set to FROZEN (DR-022).
