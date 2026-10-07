# A-007 PRODUCT TYPES: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md: product semantics at the end of Section A (with VP-01/VP-02), configuration precedence at A-009, order mechanics at G, square-off and position handling at H. Updated 2026-10-07 after ChatGPT's value-added review (docs/agenda/a-007_product-types/A-007_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_07-OCT-2026.txt): Q-32 closed from Rama's 06-Oct Q-27 answer; MIS eligibility kept as three separate levels; legacy wording made evidence-disciplined; everything here stays PROPOSED / OPEN (section 15). The A-007 legacy manual is frozen (DR-022). Section 6 corrected 2026-10-07 for the A-007 erratum E-1 found during A-008 (force_intraday_only did not rewrite intent after 10-Jul-2026; recorded in DR-023).
Date: 2026-10-06
Place at: docs/agenda/a-007_product-types/A-007_v2_design_inputs.md
Why this file exists: Rama asked that every point be noted "under appropriate headings/area of manual file" without missing anything, including non-A-007 points (checklist sections 20-22). The legacy manual records legacy fact only (V2_DESIGN_LOG W-1, W-3), so the V2 material is kept here and summarised in V2_DESIGN_LOG.
Sources: Rama's answers in docs/inputs/chatgpt_originals_and_replies/A-007_PRODUCT_TYPES_DISCUSSION_CHECKLIST_27-SEP-2026.txt; ChatGPT's 27-Sep manual (section 20 to 22) and reconciliation (sections 19, 21 to 23); Rama's later A-006 answers Q-27 and Q-28 (06-Oct); legacy evidence in A-007_manual.md; current Zerodha pages checked 2026-10-06 (section 14).
Labels: RAMA = Rama's suggestion (quoted or closely paraphrased). OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision. CURRENT EXTERNAL BROKER FACT = a current broker, exchange or regulator rule, checked 2026-10-06, never evidence of legacy behaviour (W-5). LEGACY = what the old system did (A-007_manual.md), quoted only so the proposals can be compared with it; not a requirement.
Naming: checklist sections are cited as "checklist 8" etc.; design-log questions as Q-NN.
Rule: no business value is hardcoded in Python; times and numbers below are examples or current broker facts.
Security: no credential values.

## 1. Order of intent (latest last)
1. 27-Sep-2026: Rama's A-007 checklist answers and notes (this topic).
2. 06-Oct-2026: Rama's A-006 answers, given after an explainer that included CO: Q-27 = A ("Every intraday trade uses MIS; if the stock is not eligible for MIS, reject the signal rather than silently changing the product") and Q-28 = A ("CNC buy → OCO GTT for SL + Target").
Where the two differ (intraday mechanism, section 3), the 06-Oct answer is the latest statement. The earlier note is kept for history and for the functional need it states. Q-32 (confirmation) was closed on 2026-10-07 from the 06-Oct Q-27 answer (ChatGPT review point 3); no further question to Rama.

## 2. Delivery: CNC protected by GTT only
- RAMA (checklist 7, 8, 14, 20, 22): "Process all delivery orders thru GTT (which has automated TGT/SL hit as managed by broker till it's intended lifecycle and has no auto-square off count)"; "For Delivery use only 'GTT window'"; "No margins + No auto-square off + Only GTT window will be used"; "For Delivery/Position orders use universally 'GTT' window (ignore incorporating other windows) Why? not auto-squared off + Zerodha managed TGT & SL Process".
- RAMA (06-Oct, Q-28 = A): CNC buy, then an OCO GTT for SL + target. ChatGPT's 06-Oct sequence (A-006_v2_design_inputs.md section 5): CNC entry > confirmed fill > verify actual quantity and average price > OCO GTT for the actual position > monitor and reconcile the GTT > the broker cancels the other leg > reconcile the final position.
- LEGACY: the legacy delivery execution path used CNC and, after the entry fill, constructed one broker-side two-leg OCO GTT (SL leg limit 3% below its trigger); it modified the GTT on later partial fills, ran a 15-minute GTT monitor with a defined action for every mismatch, and excluded CNC from EOD and emergency flattening (A-007_manual.md section 7). This describes the code path; individual historical placements cannot be checked.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): up to 500 active GTTs per account (legacy guard assumed about 50); GTT orders through the API are LIMIT only; a trigger is single-use and an unfilled triggered order is cancelled at day end; a GTT lasts up to 365 days; trailing-stoploss GTT is web-only, not in the API (details: A-006_v2_design_inputs.md section 5).
- CLAUDE CONSIDERATION: "GTT window" is the protection mechanism; the product stays CNC (A-006 G-5). The legacy GTT monitor's action ladder is a ready list of cases the V2 design must cover (H-006, H-007, H-008).

## 3. Intraday mechanism (Q-32)
- RAMA (27-Sep; checklist 8, 14, 20, 21, 22): use "either 'BO/CO' order window which one is supports automated or simultaneously processing TGT & SL leg orders [Check Zerodha website/Internet] then lock that window only to avoid complications"; "Once entry order is processed then simultaneous TGT & SL orders must be created + Must have auto-square off before broker does"; "For Intraday: Applicable margin applied + Auto-Square off + Suitable CO/BO window for entry + simultaneous TGT & SL Orders". Rama could not recall which window legacy used.
- RAMA (06-Oct, Q-27 = A): every intraday trade uses MIS; if the stock is not MIS-eligible, reject the signal; never change the product silently.
- LEGACY (A-007_manual.md section 5): neither CO nor BO. The baseline intraday path placed regular MIS orders: LIMIT entry, then the system placed a stop-limit SL and a LIMIT target as soon as the entry filled, sized to the filled quantity, with software OCO. Thirteen YAMLs declared the CO protocol but the order placer ignored it; code comments record only MIS and CNC in the orders table. This matches the "TGT and SL automatically created by the system" Rama remembers.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06):
  - BO: discontinued by Zerodha in March 2020 and still unavailable ("Bracket orders create issues in times of increased volatility"; target and stoploss could both execute and leave an unintended position).
  - CO: a market or limit entry with a compulsory stoploss only; there is no target leg (a target is a separate order; the legacy CO protocol also placed it separately). Intraday only; not on BSE or F&O; the stoploss cannot be cancelled; exit only from the order book; stoploss trigger within a 10% range; same broker square-off as MIS.
  - None of the Zerodha equity options checked places entry, stoploss and target together: BO is gone, CO carries the stoploss only, and "buy GTT OCO" is for F&O only.
- Q-32 closed 2026-10-07 (ChatGPT review point 3): "Q-32 closed from Rama's later 06-Oct Q-27 answer. The V2 intraday product intent is PROPOSED as MIS-only; MIS-ineligible symbols are rejected rather than converted. Exact protective-leg mechanics remain open for G/H."
- MIS eligibility, three levels kept separate (ChatGPT review point 4; PROPOSED until Section A is ratified): A. strategy product intent = MIS; B. instrument eligibility = a current broker/instrument eligibility check; C. order-time broker acceptance = the final authority. If MIS is unavailable at B or C, the trade is rejected, never silently converted to CNC. This does not mean every intraday stock is MIS-eligible.
- Candidate mechanics for G/H (OPEN, not decided): the legacy pattern, in which the system places the SL and the target immediately after the entry fills, for the filled quantity, keeps them linked (one fills, the other is cancelled) and squares off before the broker. No CO, no BO. This would meet the 27-Sep functional need (automatic SL and target after entry; square-off before the broker). Open with it: the SL order type, the protective-leg state machine, retry/escalation and timing.
- CLAUDE CONSIDERATION (why not CO): CO gives an SL that is attached from the moment of entry, but still needs a separate target, its SL cannot be cancelled, exits must go through the order book (cancel variety "co"), and legacy has no production experience with it. Its one advantage (no gap between fill and SL) can be narrowed with MIS by placing the SL straight after the fill and alerting if it fails (Q34 in A-006: retry + bounded escalation).
- OPEN for G (not product questions): the SL leg's order type. LEGACY: stop-limit with the limit 0.5% beyond the trigger, because SL-M was rejected from the first live day. CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): SL-M can be accepted on NSE equity, where applicable, when market protection is non-zero (Kite forum, from 1 April); NSE stopped SL-M for options from 27-Sep-2021; BSE blocked SL-M (circular 21-Sep-2023). Decide at G-001 / G-010.

## 4. Products in scope ("only two windows")
- RAMA (checklist 22.4): "Basically we are going to use only 2 windows (Intraday: BO/CO & Delivery: GTT) ignore incorporating other windows like NRML, SL, SL-M etc [No complications]".
- Reading after section 3 (PROPOSED): two product paths only: intraday = MIS; delivery = CNC protected by OCO GTT. NRML, MTF, CO and BO are not used. LEGACY: the placement path produced only MIS and CNC, and code comments record only those two in the orders table.
- Clarification: SL and SL-M are order types, not products. V2 still needs an order type for each leg (section 3, last point); "ignore SL, SL-M" is read as "no extra product paths", not "no stop orders".
- RAMA (checklist 10): NRML never used; F&O is outside the system (Chartink sends no F&O signals).
- CLAUDE CONSIDERATION: a broker position in a product V2 never places (NRML, CO, MTF, from a manual trade) should be handled by one rule on every path (detect, alert, never adopt silently). LEGACY handled NRML differently on its EOD, emergency and shutdown paths (A-007_manual.md section 10). Links VP-35 (manual broker intervention) and VP-43.

## 5. Square-off before the broker
- RAMA (checklist 22.2): "For Intraday orders (be it in any window, all orders must be auto-squared off by system before zerodha officially does) I Guess zerodha has stock classification wise auto-squared off process (Not universal time of 15:20 for all stocks), Check the website internet at which time zerodha executes first square off, Before that 'First' cut-off time our system must square off then makes first confirmation then after 1-2 minutes another second confirmation too [Refer legacy system's behaviour]".
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Rama is right that the time depends on the stock. Since NSE's closing auction session (3-Aug-2026): 3:12 PM for CAS stocks (the F&O list; it was 3:10 PM at launch and revised by 10-Aug-2026), 3:25 PM for other stocks; no fresh MIS/CO orders after it; Rs 50 + 18% GST per auto-squared order.
- LEGACY: two passes (15:03 price-seeking, 15:06 market) before an internal 15:09 cutoff, with verification and SL restore on failure; the code adopted the rule "never design to the LATER of two conflicting broker deadlines" and noted that one universal 15:09 cutoff gives up about 16 minutes on non-CAS stocks, naming a per-symbol CAS/non-CAS deadline as the correct end state (A-007_manual.md section 6).
- CLAUDE CONSIDERATION (VP-21): keep both broker times in config; either one cutoff before the earlier time (simple; legacy) or per-category cutoffs (needs the F&O list each day); design to the earliest published time when sources differ; Rama's first check, 1-2 minute wait and second check must all finish before the cutoff. Exact times and state machine at H-011.

## 6. Product controls: config master, strategy secondary
- RAMA (checklist 20.3): "Place 'Trade_Type (Intraday/Delivery/Both)' in Config.yaml which acts as master key then each strategy.yaml files place 'Strategy: ON/OFF, Strategy classification: INTRADAY/DELIVERY, Direction : LONG/SHORT' settings ... If number of strategies classified in Intrady/Delivery can be simply turn-off in whole segment via config.yaml and In case of few strategies of Same segment can be done via strategy.yaml by keeping 'BOTH' segments in config.yaml".
- RAMA (checklist 22.5, 21): "Config.yaml which is the 'Master' Settings and Strategy.yaml acts as 'Secondary' Settings"; product definitions exist in both config.yaml and each strategy.yaml, "So need to carefully design in new system without collision in between them".
- RAMA (checklist 5): each strategy has one product type (INTRADAY or DELIVERY) and one direction (LONG or SHORT); a strategy never trades both MIS and CNC.
- LEGACY: this structure already existed: trade_type INTRADAY/DELIVERY/BOTH as the master gate (gates, never rewrites intent), and per-strategy enabled, intent and direction. Two more switches overlapped it: force_intraday_only (~~rewrites every strategy to INTRADAY~~ makes delivery strategies dormant and has the adapter coerce products to MIS; the load-time rewrite was removed on 10-Jul-2026, A-007 erratum E-1) and delivery_enabled (a CNC/GTT lock); and the per-strategy order_protocol key had no runtime effect (A-007_manual.md sections 4-5).
- CLAUDE CONSIDERATIONS (A-009):
  - Keep the two layers Rama describes; decide whether force_intraday_only and delivery_enabled survive. A switch that changes a strategy's product (legacy: the adapter's coercion to MIS; wording corrected 2026-10-07, erratum E-1) conflicts with the Q-27 rule "never change the product silently"; turning trade_type to INTRADAY gives the same safety by rejecting delivery strategies. Rama (A-008 Q7) asks for force_intraday_only to be removed (A-008_v2_design_inputs.md section 5).
  - Every YAML key must either take effect or be rejected at load (no inert keys like the legacy order_protocol).
  - Precedence between config and strategy values follows VP-32 (maximum = lower, minimum = higher where the same setting exists at both levels).

## 7. Margin and capital by product
- RAMA (checklist 6.4, 6.5, 13, 18): intraday/MIS uses per-stock margin that changes daily (2x-5x); delivery/CNC has no margin (1x); the legacy fixed leverage_map (MIS 5x) "should be modified in new system based on stock wise eligible margins ranging from '2x-5x' as per Zerodha daily change".
- Already PROPOSED in A-005 (VP-29, VP-30, VP-33). No new item.

## 8. Position identity
- RAMA (checklist 15): legacy followed "One Symbol, One Segment (Intraday/GTT) One direction", so a symbol could trade twice (intraday and delivery); "in new system it should be 'One Symbol per Day only' meaning no more second trading of same symbol again at any cost".
- Already PROPOSED as VP-12 (restated here). With one symbol per day, the broker-matching identity still needs product and direction (A-006 Q30), for example a delivery holding carried from an earlier day plus a new trade in the same symbol (CLAUDE CONSIDERATION: decide whether a carried holding blocks a new trade in that symbol, VP-12 / C-005).

## 9. Protective legs and the order state machine
- RAMA (checklist 22.6): "Existing legacy system correctly working for Intraday and Delivery [Meaning passing TGT & SL orders simultaneously, Auto-Square off process, Cancelling other orphan order legs TGT/SL when first leg is hits (TGT/SL) Example: If TGT hits then SL and Vice-Versa automatedly cancelled, So review existing system behaviour in GitHub repo]".
- ChatGPT (27-Sep): the V2 mechanism must handle protective-leg creation, partial fills, opposite-leg cancellation and orphan-leg recovery explicitly (state machine).
- LEGACY (reviewed in source): exits placed only after the entry fill, for the filled quantity; SL first, no target if the SL fails; target retry while the SL stands; software OCO; square-off passes that restore the SL when an exit fails (A-007_manual.md sections 5-6; A-006_manual.md section 10). Known gaps: 02/03-Sep naked positions before the restore fix.
- OPEN for G-005, G-010 to G-012, H-004, H-007.

## 10. Product data integrity (CLAUDE CONSIDERATION, raised from legacy defects)
- Store product and intent on every order and trade (legacy derived the product at exit from the protocol, which mapped every LIMIT_TRIPLE trade to MIS).
- One reverse map from broker code to intent, driven by config (legacy had a config-driven one and a hardcoded one).
- One rule for broker products the system does not place, applied on every path (EOD, emergency, reconciliation, shutdown).
- Declared settings take effect or fail at load (the legacy CO protocol was declared by 13 YAMLs and never used; the smart target was enabled in 13 YAMLs but wired only to CO trades).
- For K-004, K-005, H-008, I-002, A-009.

## 11. Live only
- RAMA (checklist 16, 20.4): keep only live mode; paper behaviour was unreliable (prices, times). Decided: DR-001.

## 12. Evidence available to Rama
- RAMA (checklist 21): production contract notes are available; screenshots/logs showing product values are not.
- CLAUDE CONSIDERATION: the contract notes could confirm, for the audit, that only MIS and CNC were traded (optional; Q-31).

## 13. ChatGPT's 27-Sep V2 inputs and open questions (kept so nothing is lost)
- Delivery: GTT as the broker-managed protection/exit; no intraday square-off for delivery.
- Intraday: select one suitable current Zerodha-supported mechanism; do not blindly carry BO/CO from legacy; verify current broker/API support first.
- Config.yaml master segment control (trade_type INTRADAY | DELIVERY | BOTH); strategy YAML secondary (ON/OFF, classification, direction); master OFF must safely suppress the relevant strategies.
- Live only; margin from current broker data, not fixed leverage; square-off before the broker cutoff with verification (timing and state machine open); one symbol once per day; explicit protective-leg state machine.
- ChatGPT's open questions 9-11 (V2): final intraday mechanism (Q-32, closed 2026-10-07 from Q-27: MIS-only intent PROPOSED; mechanics open for G/H); square-off timing and state machine (VP-21, H-011); config vs strategy precedence (A-009). Its questions 1-8 (legacy) are answered from source in A-007_manual.md (see A-007_review_notes.md).

## 14. External references (checked 2026-10-06)
- Zerodha support, why bracket orders were stopped: https://support.zerodha.com/category/trading-and-markets/trading-faqs/articles/why-bo-stopped
- Zerodha support, cover orders: https://support.zerodha.com/category/trading-and-markets/product-and-order-types/order/articles/what-are-cover-orders-and-how-to-use-them
- Zerodha Z-Connect product updates, 10-Aug-2026 (CAS square-off revised to 3:12 PM from 3:10 PM): https://zerodha.com/z-connect/?p=454739
- Zerodha support, intraday auto square-off timings: https://support.zerodha.com/category/trading-and-markets/trading-faqs/market-sessions/articles/intraday-auto-square-off-timings
- Zerodha support, GTT (maximum 500 active GTTs): https://support.zerodha.com/category/trading-and-markets/gtt/articles/what-is-the-good-till-triggered-gtt-feature
- Zerodha support, SL-M for options (NSE, from 27-Sep-2021): https://support.zerodha.com/category/trading-and-markets/charts-and-orders/order/articles/how-to-use-sl-l-order-like-a-sl-m-order
- Zerodha support, SL-M blocked on BSE: https://support.zerodha.com/category/trading-and-markets/alerts-and-nudges/kite-error-messages/articles/slm-discontinued-by-bse
- Kite Connect forum, preparing for SEBI's retail algo rules (market protection for market and SL-M orders): https://kite.trade/forum/discussion/15912/preparing-to-comply-with-sebis-retail-algo-rules-static-ip-ratelimits-order-types

## 15. Status and boundary (ChatGPT review, 07-Oct-2026, points 4, 10 and 11)
Everything in this file stays PROPOSED / OPEN. None of the following is decided by A-007; each is governed at a later agenda or design stage:

| Item | Where it is decided |
|---|---|
| MIS-only intraday product intent (Q-27; Q-32 closed from it), with the three-level eligibility rule | End of Section A (VP-41) |
| CNC + OCO GTT delivery (Q-28) | End of Section A; G for GTT construction |
| One symbol per day | C-005, F-007 (VP-12) |
| Config master / strategy secondary product controls | A-009 (VP-42) |
| Per-category square-off timing and the two checks | H-011 (VP-21) |
| Product data-integrity rules | K, H, I, A-009 (VP-43) |
| Protective-leg state machine, SL order type, retry/escalation | G, H (VP-38, VP-41) |
| Capital and margin by product | F (VP-29, VP-30, VP-33) |
| Live only | Already decided: DR-001 (nothing added here) |

No register row is created because Q-32 is closed (ChatGPT point 11). Q-17 (strategy-YAML count) is answered by A-007 evidence; the strategy-model review stays with A-008.
