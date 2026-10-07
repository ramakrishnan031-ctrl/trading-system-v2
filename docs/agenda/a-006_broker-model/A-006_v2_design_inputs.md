# A-006 BROKER MODEL: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Corrected 2026-10-06 during A-007: CAS square-off history (3:10 PM at launch, 3:12 PM since Zerodha's 10-Aug-2026 update) in sections 4.1 and 7. Rama: "treat all of them are 'Suggestions' only [Not fixed decisions], Why? While implementing need to discuss with Claude & Further any other issues/bugs arises accordingly we need to review again." Items become decisions only as rows in V2_DECISION_REGISTER.md, mainly at A-007 (product types), A-010 (secrets/environment), F (capital), G (order execution), H (open-position management), I (safety), K (state), L (alerts/evidence), M-005 (token management) and N (testing). Updated 2026-10-06 with Rama's answers to Q-27 and Q-28 and ChatGPT's value-added review (A-006_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_06-OCT-2026.txt). ChatGPT (point 10): do NOT turn Q-27, Q-28, the authentication preference, charges handling, retry policy, reconciliation policy, rate-limit policy or eligibility implementation into frozen V2 decisions merely because ChatGPT reviewed them. The legacy manual is frozen (DR-021); this file stays open and carries its items to later topics (section 19).
Date: 2026-10-06
Naming: checklist questions are written Q1 to Q44 (no hyphen); V2_DESIGN_LOG open questions are written Q-01 to Q-30 (with hyphen). Checklist Q27/Q28 (reconciliation) are not design-log Q-27/Q-28 (MIS, delivery entry).
Place at: docs/agenda/a-006_broker-model/A-006_v2_design_inputs.md
Why this file exists: Rama asked that nothing be missed and that points not belonging to A-006 be kept under suitable headings inside A-006 (checklist, "Any additional A-006 point"). The legacy manual records legacy fact only (V2_DESIGN_LOG W-1, W-3), so all V2 material from the A-006 discussion is kept here and summarised in V2_DESIGN_LOG (VP-34 to VP-40; VP-03, VP-21, VP-28, VP-29, VP-30 extended).
Sources: Rama's answers in docs/inputs/chatgpt_originals_and_replies/A-006_BROKER_MODEL_DISCUSSION_CHECKLIST_24-SEP-2026.txt; ChatGPT's 25-Sep manual (Parts II-XV) and reconciliation (sections 3-17); current Zerodha and Kite Connect pages checked on 2026-10-06 (section 18); Rama's 06-Oct answers to Q-27 and Q-28 (chat); ChatGPT's value-added review (docs/agenda/a-006_broker-model/A-006_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_06-OCT-2026.txt).
Labels: RAMA = Rama's suggestion (quoted or closely paraphrased). OPEN = not decided; Rama asked to note it and discuss it with Claude during design ("I don't want to take risks of giving wrong/unsuitable answers"). CLAUDE CONSIDERATION = Claude's input for that later discussion; not Rama's view and not a decision. CURRENT EXTERNAL BROKER FACT = a current broker, exchange or regulator rule, checked 2026-10-06; never evidence of legacy behaviour (ChatGPT review point 1).
Rule: no business value is hardcoded in Python. Times and numbers below are examples or current broker facts, never frozen V2 values.
Security: no credential values. Credential classes are named only.

## 1. Answer index (all 44 checklist questions)

| Q | Topic | Rama's answer (short) | Status | Section / log |
|---|---|---|---|---|
| Q1 | Broker | Zerodha only (Angel One planned, then dropped) | RAMA | 2; VP-02 |
| Q2 | Broker authority | YES, broker authoritative for live broker state ("final authority at EOD") | RAMA | 2; VP-35 |
| Q3 | Local DB vs broker | "No idea now"; decide the best fit with Claude during implementation; note it carefully | OPEN | 2; VP-35 |
| Q4 | Token model | Keep the Zerodha workflow subject to current API rules (legacy: headless plus manual token creation/upload by bash); V2 preferably headless with no manual step; redesign if possible | RAMA, conflicts with current rule; OPEN / DEFERRED to A-010 | 3; VP-36; Q-29 |
| Q5 | Auth failure / alerts | No new entries; keep protecting and reconciling open positions where broker access permits; Telegram and Email at once. Alerts: "Both [Consider it as Global rule in all places]" | RAMA | 3; VP-34; VP-36 |
| Q6 | Products | Intraday and delivery "with suitable options"; for now MIS, CNC/GTT, CO; BO probably unsupported (check) | RAMA | 4; VP-37 |
| Q7 | MIS for all intraday | "NO - Explain [Why? I'm unaware fully, so safer option, Explain fully then decide]". After the explainer (06-Oct, Q-27): A, every intraday trade uses MIS; reject if the stock is not MIS-eligible | RAMA (06-Oct), PROPOSED | 4.1; Q-27; VP-37 |
| Q8 | Delivery / GTT | Delivery/GTT stays (broker handles TGT/SL on later days); GTT rules documented and validated: YES, mandatory | RAMA | 5; VP-37 |
| Q9 | Entry order types | No idea; broad, best-fit options; legacy seems OK; discuss | OPEN | 6; VP-38 |
| Q10 | Entry flow | Chartink > per-strategy adapter/processor (central score/rank allocator to be modified) > config/strategy YAML > position size > entry order with a suitable window for intraday; delivery "must be in GTT". 06-Oct (Q-28): A, CNC buy, then OCO GTT for SL + target | RAMA, PROPOSED | 5; 6; VP-03; Q-28; VP-37 |
| Q11 | Exit mechanisms | No idea; broad options; legacy seems OK; delivery exits in GTT | OPEN (delivery: RAMA) | 7; VP-38 |
| Q12 | Margin authority | CONFIRMED (daily reference for sizing; order-time API final check) | RAMA | 8; VP-30 |
| Q13 | Eligibility vs margin | YES, validate separately | RAMA | 8; VP-30 |
| Q14 | No margin data | CONFIRMED (100% cash, proceed, alert) | RAMA | 8; VP-30 |
| Q15 | Margin change at order time | Reduce/adjust quantity within all existing constraints | RAMA | 8; VP-30 |
| Q16 | Funds data | Opening cash for trading; available cash for drift alerts | RAMA | 9; VP-29 |
| Q17 | Order-margin response | YES, store with date (margin can change each day) | RAMA | 9; VP-40 |
| Q18 | Charges | From contract/settlement reports; asks why charges are needed | RAMA; question answered; PROPOSED principle (ChatGPT) | 9; VP-40 |
| Q19 | "Submitted" = broker ID? | No idea; queued, cancelled and rejected orders also carry broker and exchange IDs; be cautious | OPEN | 10; VP-38 |
| Q20 | Fill authority | YES | RAMA | 10; VP-35 |
| Q21 | Partial fill | Keep waiting for the rest; if SL/TGT hits the filled part, cancel the unfilled rest at once; alert partial fills with status | RAMA | 10; VP-38 |
| Q22 | Timeout | Reconcile first, then alert (orders may wait queued at the broker) | RAMA | 10; VP-38 |
| Q23 | Rate-limit policy | No idea; discuss | OPEN | 11; VP-39 |
| Q24 | API failure per category | Not answered | OPEN | 11; VP-39 |
| Q25 | Retry policy | No idea; discuss | OPEN | 11; VP-39 |
| Q26 | Reconciliation frequency | No idea; discuss | OPEN | 12; VP-35 |
| Q27 | Reconciliation authority | No idea; discuss | OPEN | 12; VP-35 |
| Q28 | Reconciler actions | No idea; discuss | OPEN | 12; VP-35 |
| Q29 | Position source | Broker + local state, broker authoritative | RAMA | 12; VP-35 |
| Q30 | Position identity | Symbol + product (MIS/GTT) + direction (long/short) | RAMA | 12; VP-35 |
| Q31 | Manual broker intervention | Detect and reconcile, send alert | RAMA | 12; VP-35 |
| Q32 | Protective SL | YES, broker-side wherever rules permit | RAMA | 7; VP-38 |
| Q33 | Target | Intraday/MIS: strategy-dependent (R:R in strategy.yaml); delivery: GTT | RAMA | 7; VP-38 |
| Q34 | SL placement failure | Retry + bounded escalation | RAMA | 7; VP-38 |
| Q35 | EOD square-off | System-controlled before the broker cut-off; square-off > first check > wait 1-2 minutes > second check | RAMA | 7; VP-21 |
| Q36 | Pre-order eligibility | No idea; discuss | OPEN | 13; VP-37 |
| Q37 | Instrument master | YES; daily refresh before market open (legacy cron principle); alert on failure | RAMA | 13; VP-28 |
| Q38 | Live only | CONFIRMED | DECIDED (DR-001) | 14 |
| Q39 | Production vs testing broker differences | Not answered (repository review) | UNKNOWN | 16 |
| Q40 | Testing method | No idea; "all applicable" was a random answer | OPEN | 14; Q-01 |
| Q41 | Rejection record | YES (unsure all fields are available) | RAMA | 15; VP-40 |
| Q42 | Retry identity | YES | RAMA | 15; VP-39 |
| Q43 | Evidence retained | No idea; random answer: all applicable, if available | OPEN | 15; VP-40 |
| Q44 | Retention | YES, same long-term policy | RAMA | 15; VP-40 |

## 2. Broker identity, authority and the local database
- RAMA (Q1): Zerodha only. Angel One was planned, then dropped (the legacy config kept a disabled fallback entry; A-006 manual section 3).
- RAMA (Q2): the broker is authoritative for live broker state: available cash/funds, opening balance, orders, order status, fills, positions, average price, realised and unrealised P&L, margin/required funds, product/eligibility, broker order IDs. Reason: "that's the final authority at EOD".
- OPEN (Q3): the role of the local database (system audit/state record while the broker stays authoritative for broker facts; co-equal source of truth; other). Rama: "No idea now, But do it with best fitment by discussing with Claude during implementation - Now carefully note it in manual file without fail." For K-001 and K-002.
- CLAUDE CONSIDERATION (for K-001): two kinds of data have different owners. Broker facts (fills, positions, funds, broker order status) belong to the broker; a local copy is only a cache. System facts (signals, decisions and their reasons, reservations, attempt IDs, intended prices, the configuration in force) exist only locally, so the local database is their only source. One reading consistent with Q2 and Q29: local database = system record and audit trail; broker = authority for broker facts; when they disagree about a broker fact, the broker wins and the local record is corrected with evidence. Not decided.
- ChatGPT (06-Oct review): keep Q3 OPEN; the broker is authoritative for live broker facts, but conflict handling for local/audit state still needs a controlled decision.

## 3. Session, token and authentication failure
- RAMA (Q4): keep the Zerodha token/session workflow, subject to current API requirements. Legacy was "headless + manual creation and upload using bash commands". For V2 Rama is fine with headless (no manual step) and with a redesign if possible.
- RAMA (Q5): if authentication or the session is unavailable: do not accept or place new entries; continue protection and reconciliation of open positions where broker access permits; send Telegram and Email alerts immediately.
- RAMA (Q5, global rule): every alert goes to BOTH Telegram and Email, everywhere in V2 (VP-34).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06):
  - The Kite Connect access token expires at 6 AM the next day ("regulatory requirement"), or earlier on logout (Kite Connect user docs).
  - Two Zerodha staff replies on the Kite Connect developer forum (27-Jul and 26-Aug; year not shown on the page) say exchange requirements need the user to log in manually at least once a day, so Zerodha does not recommend automating the login flow. One reply adds that Zerodha is "exploring ways to make the token generation more seamless".
  - Zerodha's Kite Connect FAQ: a registered static IP is required to PLACE orders through the API; other endpoints (market data, order book, positions) work from any IP; up to two IPs (one primary, one optional backup); one change per calendar week. The FAQ text says "from 1 April 2025"; the Kite forum note on the NSE circular records the effective date as extended to 1 April 2026. Either way it applies now.
  - Broker notices on the SEBI retail-algo framework also require 2FA once every trading day (A-006 manual section 9).
- Conflict (Q-29): Rama's preference for a fully headless login conflicts with the exchange requirement as Zerodha states it. The legacy scripted TOTP login follows the same headless pattern (A-006 manual section 9; audit lead Q-30). Decide at A-010 / M-005.
- ChatGPT (06-Oct review, point 5): do not force a V2 authentication decision inside A-006. Legacy: headless operation plus a manual token creation/upload fallback, as documented. Current fact: Kite access tokens are daily and the supported login flow carries a daily authentication constraint. V2 preference: fully headless, no manual step (Rama). V2 decision: OPEN / DEFERRED to A-010 / M-005, using current official broker rules and supported API behaviour. Do not promise that a fully automatic login can be built because the legacy system attempted one, and do not automate around a broker/regulatory authentication restriction.
- CLAUDE CONSIDERATION (options for A-010 / M-005; not decided):
  - (a) One manual Kite login each morning, made as light as possible (for example a link sent by Telegram and Email that opens the Kite login and returns the token to the VM); the system refuses new entries until it holds a valid token.
  - (b) Scripted login as in legacy: headless, but against the stated exchange requirement and Zerodha's advice.
  - (c) Adopt any official seamless method Zerodha releases.
  - Whatever is chosen: a static IP on the VM for order placement (M-007); token-expiry monitoring (legacy checked every 30 minutes, A-006 manual section 9); Q-11 first (regenerate the Kite API secret).
- CLAUDE CONSIDERATION: Rama's Q5 behaviour matches the legacy token monitor (SOFT_KILL: no new entries; never hard-kill, because SL and target orders stay at the broker). Without a valid token the system cannot place or modify protective orders, so protection then rests on orders already at the broker (SL legs, GTTs) and on the broker's MIS square-off. This makes Q32 (protective SL resting at the broker) more important.

## 4. Products
- RAMA (Q6): intention is intraday and delivery, "accordingly built it with suitable options"; for now MIS, CNC/GTT and CO; BO probably not supported (check).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Zerodha no longer supports BO (bracket orders) (ChatGPT's 25-Sep check; legacy config comment). CO is still listed by Zerodha for intraday: a market or limit entry with a compulsory stoploss; intraday only; not on BSE or F&O; the stoploss cannot be cancelled; exit only from the order book; stoploss trigger within a 10% range (Zerodha support, cover orders). Kite Connect order varieties: regular, amo, co, iceberg, auction.
- Clarification: GTT is not a product. CNC is the delivery product; GTT is a broker-side trigger that places an order later (used for the delivery exit, optionally for entry; section 5). Rama's "CNC/GTT" and "MIS/GTT" are read as "CNC, protected by GTT".

### 4.1 Q-27 explainer: should every intraday trade use MIS? (Rama: "Explain fully then decide")
What MIS is (CURRENT EXTERNAL BROKER FACT, Zerodha support and Z-Connect, checked 2026-10-06):
- MIS (Margin Intraday Square-off) is Zerodha's intraday product. The broker blocks only part of the order value (a per-stock margin; Rama's A-005 range 20%-50%, i.e. 2x-5x), so the same cash buys a larger position than with CNC.
- Short selling: equity can be sold first only intraday, i.e. with MIS (or CO). A CNC sell needs the shares in the demat account; selling without them causes short delivery, an exchange auction and a penalty.
- Broker square-off: Zerodha closes open MIS/CO positions itself at its auto square-off time and charges Rs 50 + 18% GST per order (orders above 30,000 quantity are split and each part is charged). Current times: 3:12 PM for CAS stocks (stocks in the F&O list; NSE's closing auction session began 3-Aug-2026 with 3:10 PM, revised to 3:12 PM by Zerodha's 10-Aug-2026 update) and 3:25 PM for other stocks. No fresh MIS/CO orders after that time.
- MIS is not allowed for: trade-to-trade stocks (NSE BE series: the exchange bars intraday), stocks under ASM, GSM or the unsolicited-SMS list, and stocks Zerodha blocks for volatility, low liquidity, narrow circuit bands or high margin. The list changes; an MIS order on a blocked stock is rejected even with full cash.

| | MIS | CNC bought and sold the same day | CO |
|---|---|---|---|
| Cash needed | Margin only (per stock, 20%-50%) | 100% of the order value | Margin (amount not checked) |
| Short trades | Yes | No (short delivery, auction) | Yes |
| Broker safety net if the system fails to exit | Yes: broker auto square-off (charged) | No: the position simply becomes a delivery holding | Yes: broker auto square-off |
| Stocks | MIS-eligible only | Delivery-eligible; not BE (trade-to-trade allows no same-day square-off) | Same blocks as MIS; NSE equity only |
| Charges | Intraday | Intraday charges when bought and sold the same day (Trading Q&A by Zerodha) | Intraday |
| Stop-loss | Placed by the system (legacy: stop-limit leg) | Placed by the system | Placed with the entry; cannot be cancelled |
| Legacy | Configured for INTRADAY (product_map) | Not established | Configured; use UNKNOWN |

Effect on risk: V2 sizing is risk-based (VP-13): quantity = MIN(risk/SL quantity, capital ceiling / price). Leverage only raises the capital ceiling. With MIS the ceiling is higher, so the risk quantity binds more often and the position can be larger than cash alone allows. The planned loss at the SL is the same, but a price gap through the SL loses more on the larger position. With CNC the position is limited to the cash.

Options (Q-27; Rama decides):
- A. MIS for all intraday trades (as in the legacy product map, INTRADAY = MIS). A signal on an MIS-blocked stock is rejected with a recorded reason after an eligibility check (sections 8 and 13; VP-27, VP-30).
- B. MIS where allowed; CNC for long intraday signals on MIS-blocked stocks. More stocks tradable, but two products for intraday, no broker safety net on the CNC trades, and the system must still exit them the same day.
- C. CO for intraday (stoploss fixed at entry). Less flexible (SL cannot be cancelled, limited changes, exit only from the order book); same square-off as MIS.
- CLAUDE CONSIDERATION: A is the simplest and keeps the broker square-off as a backstop if the system fails. It needs the eligibility check before the order and an intraday square-off well before 3:12 PM / 3:25 PM (VP-21).

Rama's answer (06-Oct-2026): "Q-27 → A: Every intraday trade uses MIS; if the stock is not eligible for MIS, reject the signal rather than silently changing the product."
ChatGPT (06-Oct review, point 2): agreed as a V2 proposal. Recommended wording: "All V2 intraday strategies request MIS. If Zerodha does not permit MIS for the selected instrument at order time, the intraday trade is rejected rather than converted to CNC."
- "Every intraday strategy requests MIS" does not mean every instrument is MIS-eligible. Flow: intraday strategy > product request = MIS > broker/instrument eligibility check > eligible: continue; not eligible: reject; never submit as CNC automatically (that would change the business meaning of the strategy).
- The exact pre-check mechanism stays OPEN for the broker/order design (G). Broker eligibility and product configuration are separate concerns.
- Status: PROPOSED V2 input, accepted for now; not a final implementation decision. Product semantics are settled at A-007 (section 19).

## 5. Delivery and GTT (Q-28)
- RAMA (Q8, Q11, Q33): delivery stays, with GTT so that the broker executes target/SL on later trading days "instead of making complicated system codes for delivery execution from next trading day(s)". GTT compatibility (Zerodha vs system) must be documented and validated before implementation: "YES [Mandatory to check the compatibility of Zerodha Vs System".
- RAMA (Q10): delivery entries "must be in GTT".
- Legacy (A-006 manual section 6): a normal CNC entry order, then ONE two-leg OCO GTT (SL leg and target leg, both SELL LIMIT) for the filled quantity; SL-leg limit 3% beyond the trigger so a gap-down can still fill.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06):
  - GTT is available for CNC, NRML and MTF (not MIS). Types: single trigger; two-leg OCO (when one trigger fires, the other is cancelled).
  - Kite Connect GTT API: the order placed on trigger is LIMIT only (Kite web can also place a market order with market protection).
  - A GTT stays active for up to 365 days. The trigger is valid only once: if the triggered order does not execute, it is cancelled at the end of that day like any order, and a fresh GTT is needed.
  - "Buy GTT OCO" (entry plus exits in one GTT) exists only for F&O contracts, not equity.
  - Trailing-stoploss GTT (Kite web, announced 23-Jun-2026; CNC, MTF, NRML; equity sell side only) is NOT available through the Kite API.
  - GTT is not available in the Kite Connect sandbox (section 14).
- Q-28 (answered by Rama 06-Oct: A; see below): what "delivery must be in GTT" means for the ENTRY:
  - A. A normal CNC entry order during market hours, then an OCO GTT for the exit once filled (the legacy code path). The entry happens when the signal is approved; the GTT protects from the fill onward.
  - B. The entry itself as a single-trigger BUY GTT (fires when the price crosses a level), then an OCO GTT for the exit after the fill. Fits "enter only if price reaches X" and next-day entries (VP-14). Costs: LIMIT only; single-use trigger; cash is checked only when it fires; the system must still detect the fill and place the exit GTT; a second asynchronous layer.
  - CLAUDE CONSIDERATION: A matches Rama's stated reason (the broker manages exits on later days) and legacy; B only if V2 adopts next-day entries.
- CLAUDE CONSIDERATIONS for the GTT design (A-007, G-010 to G-012, H):
  - The gap between the CNC fill and the GTT placement on the entry day; a GTT placement failure follows Q34 (retry + bounded escalation).
  - A triggered SL leg that does not fill (price gaps through the LIMIT price) leaves the holding unprotected after that day: detect it and re-protect (legacy used the 3% deep SL limit).
  - Partial entry fills (Q21): the GTT quantity must follow the filled quantity (modify the GTT as fills arrive, or place it once the entry completes or is cancelled).
  - Selling holdings through GTT needs the broker's sell authorisation (DDPI per A-004).
  - Daily check that each delivery holding has exactly one active GTT with matching quantity (legacy reconciled GTTs overnight; A-006 manual section 6).

Rama's answer (06-Oct-2026): "Q-28 → A: CNC buy → OCO GTT for SL + Target. This is also consistent with the legacy behavior already established." (Legacy: the code path in A-006 manual section 6; use rests on the code and Rama's account, placement records not read.)
ChatGPT (06-Oct review, point 3): agreed; option A is the correct interpretation of the intended model. Recommended wording: "Delivery entry is a normal CNC buy. After the delivery position is confirmed as filled, the system creates/maintains a broker-side OCO GTT containing the delivery stop-loss and target."
- Sequence: approved delivery signal > CNC entry order > fill confirmation > verify the actual filled quantity and average price > create the OCO GTT for the actual position > monitor/reconcile the GTT state > target or SL triggers > the broker cancels the other OCO leg > reconcile the final position.
- Never create a GTT against an assumed quantity when the CNC entry was partially filled.
- Manual broker-side changes are detected and reconciled.
- A GTT creation failure after a delivery fill is a protection failure needing bounded retry/escalation.
- Exact GTT construction stays a later G (order design) decision.
- Status: PROPOSED V2 input, accepted for now; not a final implementation decision. Carried to A-007 and G (section 19).

## 6. Entry flow and entry order types
- RAMA (Q10): "Chartink fires signals > then keep each strategies has it's own adapter or signal processor or something who filters signals then validates for Entry/Reject [existing system has central/global score/rank allocator design needs to be modified] > then apply config.yaml and/or relevant signal.yaml file's settings > Position Size (already discussed capital flow + Qty determination) > Make entry Order with suitable window for Intraday? and Delivery (must be in GTT)". Links VP-03 (per-strategy processors), VP-08, VP-09 (entry window), VP-13 (sizing).
- ChatGPT (25-Sep): the central/global score/rank allocator is reconsidered in the future strategy/filtering architecture (A-008, D-017); not frozen by A-006.
- OPEN (Q9): which entry order types (MARKET, LIMIT, SL-LIMIT, SL-MARKET). Rama: consider broad, best-fit options; "current legacy system is seems to be ok too"; note carefully and discuss. For G-001.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): market orders through the API must carry market protection (a value of 0 is rejected; a custom percentage or -1 for automatic); market protection turns the order into a limit order within the exchange's price-protection range (Kite Connect FAQ and orders docs). Legacy validated market protection itself, within 0.1%-10% (A-006 manual section 5).
- CLAUDE CONSIDERATION (for G-001): legacy accepted MARKET, LIMIT, SL and SL-M but placed SL legs as stop-limit, because Zerodha rejected SL-M through the API (config comment). Re-verify current SL-M acceptance before choosing.

## 7. Exits and protection
- OPEN (Q11): which exit mechanisms (broker-side SL, broker-side target/limit, software-managed exit, GTT, emergency MARKET exit, other). Rama: broad, best-fit options; legacy seems OK; discuss. Delivery: GTT (section 5).
- RAMA (Q32): protective SL broker-side whenever the product/order rules permit.
- RAMA (Q33): intraday/MIS target strategy-dependent (the R:R in strategy.yaml); delivery target via GTT, handled by the broker once the entry fills.
- RAMA (Q34): if the entry fills but the protective SL placement fails: retry + bounded escalation. The escalation steps are OPEN (legacy: SL first; target not placed if the SL failed; HARD_KILL flattens at LTP +/- 1%; A-006 manual section 10).
- RAMA (Q35): EOD square-off system-controlled before the broker cut-off, with two checks: square-off, first confirmation check, wait 1-2 minutes, second check, all before the broker's square-off. Same idea as VP-21; the wait is a suggestion, not a frozen value.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Zerodha's current equity MIS auto square-off is 3:12 PM for CAS stocks (F&O-list stocks: continuous trading for them stops at 3:15 PM, followed by the closing auction) and 3:25 PM for other stocks (Zerodha support; Z-Connect 10-Jul-2026). CAS began on 3-Aug-2026 with 3:10 PM for CAS stocks, revised to 3:12 PM (Zerodha product update, 10-Aug-2026). The V2 square-off and both checks must finish before the earlier time for CAS stocks, or the cut-off must be set per stock category. Legacy's MIS cut-off was 15:09 at the baseline (since 03-Sep-2026; A-007_manual.md section 6). This answers Q-18.
- Related: legacy smart target (trailing) and Zerodha's trailing-stoploss GTT (web only, not API): Q-15, section H.

## 8. Margin and capital handoff from A-005
- RAMA (Q12, confirmed): the daily margin reference is authoritative for the day's sizing basis; the order-time broker margin/API is the final execution safety check and may reject or adjust.
- RAMA (Q13): validate broker/product eligibility and the order-specific margin separately; a margin calculation alone does not prove eligibility.
- RAMA (Q14, confirmed): no usable margin data: assume 100% cash, proceed, mandatory Telegram/Email incident alert.
- RAMA (Q15): if the order-time margin differs from the morning data: reduce/adjust the quantity within all existing constraints (not reject; not "only if the broker permits the exact order").
- Carried from A-005 (ChatGPT reconciliation section 6): one real-cash basis; the 09:15 broker opening balance authoritative for the day; configured intraday/delivery buckets; daily capital lock; broker-specific margin, no invented universal leverage; dated margin/eligibility snapshots; entry broker margin as the primary sizing basis; no separate SL/target capital allocation unless broker behaviour requires it; no business values hardcoded; reconciliation may report drift but never silently rebases today's locked capital; the next day's opening balance is the next day's basis.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): the Kite order-margins API "calculates margins for each order considering the existing positions and open orders", so pending orders change the order-time answer (links Q-26).
- CLAUDE CONSIDERATION: "adjust" needs a direction rule. Reduce-only (never above the morning-based quantity) keeps the daily lock; raising the quantity when the order-time margin is lower would let intraday broker data raise the per-trade size, which the A-005 lock avoids. For F-002 / F-004.

## 9. Funds, margin evidence and charges
- RAMA (Q16): read and store opening cash (the trading basis) and available cash (system-vs-broker capital drift alerts).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): the Kite user-margins API returns, for equity: net, available.cash, available.opening_balance ("opening balance at the day start"), available.live_balance ("current available balance"), utilised.debits and others. Legacy read net, available.cash and utilised.debits (A-006 manual section 7).
- CLAUDE CONSIDERATION: map Rama's "opening cash" and "available cash" to exact broker fields at F-001 (candidates: available.opening_balance, or available.cash at 09:15; available.live_balance).
- RAMA (Q17): store the order-specific broker margin response with the trade/order evidence, with its date, because broker margins can change each trading day.
- RAMA (Q18): charges captured from contract/settlement reports. Rama's question: "why do we requires that? Since each day morning system pulls and updates 'Opening Balance' which is already broker adjusted balance after expense/charges".
- Answer (Claude, 2026-10-06): charges are NOT needed for sizing; the next morning's opening balance already includes them (A-005). They matter only for: (a) true net P&L per trade and per strategy (on small trades, brokerage, STT, exchange and SEBI charges, stamp duty and GST can be a large part of a small profit, so gross P&L overstates a strategy); (b) a daily loss limit measured net, if V2 defines it that way (F-009); (c) explaining day-to-day differences between system and broker capital (F-014); (d) reports (L-011). RECORD-DERIVED: legacy computed charges for every closed trade (close_trade stored gross P&L, charges and net P&L; production baseline section 26; fund_manager.py at 970aabf refers to a shared CostCalculator). This says nothing about how legacy sizing treated charges (ChatGPT review point 8).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Kite Connect has a charges API (virtual contract note, POST /charges/orders) that returns order-wise brokerage, STT, exchange charges, SEBI fee, stamp duty and GST for executed orders (it needs the average price); not available in the sandbox. Contract notes remain the legal record.
- ChatGPT (06-Oct review, point 4), PROPOSED PRINCIPLE: "Broker charges are not a separate capital-sizing input unless explicitly configured later. Actual charges are retained for post-trade net P&L, performance reporting and any loss-limit calculation whose definition is net of charges." Sizing uses the defined capital/margin model; no estimated "charges reserve" and no estimated brokerage as a hidden sizing factor unless later governance requires it. A loss control defined as net includes actual charges; whether the daily loss limit is gross or net is defined at F-009. Actual charge evidence is retained where available. Consistent with Rama's Q18 choice (capture from contract/settlement reports).
- OPEN: the exact source (contract notes, Kite charges API) and timing of the final charges, until the order/reporting agenda (F-009, L-011).

## 10. Order submission, fills, partial fills and timeout
- OPEN (Q19): whether an order counts as "submitted" only after a valid broker order ID. Rama: "If Order submitted means only 'Valid broker ID' then status and treatment of 'Queued/Cancelled/Rejected Orders' handling? Even such orders will have broker ID, Exchange ID etc, So be cautious".
- CURRENT EXTERNAL BROKER FACT (Kite Connect orders docs, checked 2026-10-06): a successful place-order call returns an order_id, but "successful placement of an order via the API does not imply its successful execution": the order_id means Zerodha's order system accepted the request. The order then moves through statuses: PUT ORDER REQ RECEIVED, VALIDATION PENDING (broker risk checks), OPEN PENDING, then OPEN (live at the exchange) or TRIGGER PENDING (stop order waiting), and ends COMPLETE, CANCELLED or REJECTED (MODIFY/CANCEL PENDING in between). Postbacks and WebSocket order updates report changes as they happen (section 12).
- CLAUDE CONSIDERATION (for G-003, G-004, K-004): Rama's caution is right: a broker order ID does not mean the order is live, and rejected and cancelled orders carry IDs too. A safer model keeps separate states, for example: SENT (request made, no reply yet: outcome UNKNOWN), ACCEPTED (order_id received), LIVE (OPEN or TRIGGER PENDING), PARTIALLY FILLED, FILLED, CANCELLED, REJECTED (with the broker's reason). A request that times out with no reply is UNKNOWN, not failed: look the order up before any retry (Kite's order "tag", up to 20 characters, can carry the system's attempt ID), so a retry never creates a duplicate order. Not decided.
- RAMA (Q20): broker fill data is authoritative for filled quantity, average fill price, partial fills, order status and broker order ID.
- RAMA (Q21): partial fill: continue waiting for the remaining quantity; while waiting, if SL or target hits for the already-filled quantity, immediately cancel the unfilled remainder; send alerts for partially filled quantities with status. Exact state machine OPEN (ChatGPT).
- CLAUDE CONSIDERATION: protect the filled part at once (legacy placed exits on the actual filled quantity, SL first); the wait for the remainder is bounded by the entry timeout (VP-09: 10 minutes) and the intraday entry cut-off (VP-20).
- RAMA (Q22): on timeout: reconcile first, then alert, because an order may stay queued at the broker until its price is reached. The legacy 60-second fill_timeout is not a V2 decision (audit lead Q-23).

## 11. Rate limits, API failures and retries
- OPEN (Q23): one central broker rate limiter, per-endpoint limits, or broker-reported limits.
- OPEN (Q24, not answered): failure handling per category: authentication, funds/margin, order placement, order status, position, market data.
- OPEN (Q25): retries centrally controlled and bounded, endpoint-specific, or no automatic retry for order placement.
- ChatGPT (25-Sep): do not copy legacy numbers blindly; retries must avoid duplicate orders. Global safety principle: no unsafe new entry when a required broker control is unavailable; protect and reconcile existing positions where possible; alert by Telegram and Email.
- CURRENT EXTERNAL BROKER FACT (Kite Connect docs and FAQ, checked 2026-10-06): quote 1 request/second; historical candles 3/second; order placement 10/second; all other endpoints 10/second; 400 orders per minute; 5,000 orders per day per user/API key; 25 modifications per order. Under the exchange framework a client account may send at most 10 orders per second; excess requests are rejected with HTTP 429; higher rates need algo registration. Kite exception types: TokenException, UserException, OrderException, InputException, MarginException, HoldingException, NetworkException, DataException, GeneralException.
- Legacy (A-006 manual section 8): client-side buckets order 8/s, quote 3/s ("bumped from 1"), historical 2/s, margins 8/s; on 429, backoff and then SOFT_KILL. The quote bucket is above the documented quote limit: audit lead Q-30.
- RAMA (Q42): every retry gets a new explicit attempt identity while keeping the link to the original order/trade (links DR-006, Q-03).
- CLAUDE CONSIDERATION: classify failures by whether the broker may have acted. Before it could act (input error, 429, network failure before sending): safe to retry with backoff. After it may have acted (timeout, no reply): look up first (tag), never blind-retry an order placement. Final rejections (margin, RMS block, invalid price): no retry; record and alert.
- ChatGPT (06-Oct review, point 6): keep Q23 OPEN; do not choose a central or per-endpoint scheme without reviewing actual broker limits, SDK behaviour, endpoint usage and latency needs. Keep Q24 OPEN by failure category; authentication, funds/margin, order placement, order status, position and market-data failures have different safety consequences. Keep Q25 OPEN; never blindly retry order placement, because an order may have been accepted while the caller did not receive the result.

## 12. Reconciliation and positions
- OPEN (Q26): reconciliation frequency (continuous polling; event-driven where possible plus periodic full reconciliation; periodic only).
- OPEN (Q27): conflict authority when broker and local state differ. Rama's Q2 and Q29 make the broker authoritative for live broker facts; the detailed resolution rules are open (ChatGPT).
- OPEN (Q28): which actions the reconciler may take automatically (update order status, fill data, position state; release/reserve capital; adopt an unknown broker position; close an orphan local record; cancel broker orders; place protective orders; emergency exit; alert only). Legacy actions (A-006 manual section 10) are not promoted by default.
- RAMA (Q29): position truth from broker + local state, broker authoritative.
- RAMA (Q30): position identity = symbol + product (MIS/GTT) + direction (long/short). Clarification: the product is MIS or CNC; GTT marks how a CNC position is protected. VP-12 (one trade per stock per day, any direction) makes symbol + day unique for new entries, but the identity still needs product and direction for broker matching (a carried CNC holding and a same-day MIS trade in the same symbol are different positions).
- RAMA (Q31): manual broker intervention (a human changes, cancels or closes an order or position at the broker): detect, reconcile, send alert.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Kite sends order updates by postback (HTTP POST to a registered URL on COMPLETE, CANCEL, REJECTED and UPDATE, which covers modifications and partial fills; the payload carries a checksum to verify) and over the WebSocket (recommended for individual developers). Orders, trades, positions and holdings can also be polled.
- CLAUDE CONSIDERATION (for Q26): event-driven updates (WebSocket) for speed, a periodic full broker-vs-local comparison as the safety net (updates can be missed during disconnects), and reconciliation at start-up and at EOD. Not decided.
- ChatGPT (06-Oct review, point 6): keep Q26 OPEN until order/position state and the broker's event and polling capabilities are reviewed. Keep checklist Q27/Q28 (reconciliation authority and actions) OPEN at design level, keeping the stated principle that the broker is authoritative for live broker facts; exact automatic repair actions need design (G/H).

## 13. Eligibility and instrument master
- OPEN (Q36): which pre-order eligibility checks (symbol, exchange/segment, product, quantity, price/tick, order type, MIS/delivery eligibility, other). To be set from current broker/API rules.
- RAMA (Q37): broker instrument metadata refreshed daily before market open and treated as a prerequisite for orders; follow the legacy cron principle (scheduled after boot); Telegram and Email alert on failure. Exact V2 time and mechanism open (M-002).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): the exchange bars intraday in the BE (trade-to-trade) series; Zerodha blocks MIS for ASM, GSM and unsolicited-SMS stocks and for some stocks on risk grounds (section 4.1). The legacy instrument filter admitted the BE series (A-004): audit lead Q-30.
- CLAUDE CONSIDERATION: the eligibility check sits after the instrument master (tick, lot, series) and before sizing; it can use the broker's own block information where available, and records "not eligible" as a status (VP-30). Links VP-27 (static blacklist removed; eligibility controls decided separately) and VP-28.
- ChatGPT (06-Oct review, point 6): keep Q36 OPEN at implementation level; broker eligibility, system configuration, strategy request and actual broker acceptance are distinct. Q37: broker instrument metadata is an important dependency for order construction; exact refresh timing and validation stay open until the relevant agenda topics.

## 14. Live only and testing
- CONFIRMED (Q38): V2 is live only; paper mode is not required (DR-001). Legacy paper simulation is evidence only.
- OPEN (Q40): how broker integration is tested (API mocks; recorded broker responses; a broker sandbox if one exists; controlled live-account tests). Rama's "all applicable" was explicitly a random answer.
- CURRENT EXTERNAL BROKER FACT (conflicting sources, checked 2026-10-06): the Kite Connect developer docs describe a sandbox (sandbox.kite.trade, demo credentials, no real money) supporting market data, LIMIT orders, portfolio, profile and WebSocket, but not margin calculations, the charges API, GTT or MARKET orders. Zerodha's support FAQ says "Zerodha does not offer a sandbox environment for Kite Connect". Verify before relying on it (Q-01, N-003); never assume a sandbox behaves like the live exchange.

## 15. Errors and audit evidence
- RAMA (Q41): record every broker rejection with: broker error code, error message/category, symbol, strategy, intended order, quantity, price, product, margin/eligibility context, timestamp, broker order ID if available, and the system's decision/recovery action. Rama is unsure all fields exist; never fabricate a missing field (ChatGPT).
- RAMA (Q42): a new attempt identity for every retry, linked to the original order/trade.
- OPEN (Q43): which evidence is kept per order/trade. Rama's random answer: all applicable, if available: broker order ID; exchange order ID; submission request; broker acknowledgement; order status history; fill history; margin response; broker position snapshot; funds snapshot; broker rejection/error; reconciliation result; timestamps.
- RAMA (Q44): broker evidence follows the same long-term audit/retention policy as the rest of V2 (K-011).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Kite order history gives each status change with timestamps; rejected orders carry the broker's status message.
- ChatGPT (06-Oct review, point 6): "all applicable" (Q41/Q43) is reasonable as a proposal; the exact mandatory evidence schema and the retention period belong to later evidence/governance work (L-008, L-009, K-011).

## 16. Production versus testing (Q39)
- Not answered; for repository review. No broker code or config comparison between the production and testing snapshots was made in A-006 (manual section 11). The testing snapshot is comparison evidence only and never overrides production.

## 17. Cross-topic inputs (kept here so nothing is lost)
From the checklist (section 17) and ChatGPT's reconciliation (section 17):
1. All user/business/system decision values are configuration-driven; never hardcoded in Python.
2. The broker is the source of truth for live broker state.
3. The daily capital basis is locked; reconciliation must not silently rebase it.
4. Margin data is dated and auditable.
5. Margin refresh success and failure both alert (Telegram and Email).
6. No silent repeated broker/API failures.
7. Broker eligibility and broker margin calculation are separate.
8. Actual filled quantity/price is kept separate from intended values (VP-10).
9. Product semantics are explicit (MIS, CNC; GTT as a mechanism, not a product).
10. Legacy paper-mode code is historical evidence only; V2 is live only.
11. Legacy CO/BO/BRACKET configuration does not prove historical use.
12. Testing/sandbox is not assumed identical to production without evidence.
13. Any broker-specific rule is confirmed against current broker documentation/API behaviour before implementation.
14. Telegram + Email is the global alert pair (VP-34).
15. Rama does not want to guess technical answers to complete a checklist; use repository/API evidence and discussion with Claude.
16. Delivery future-day exits use broker-managed GTT where compatible.
17. A-005 carry-forward: one trade per symbol per day; intraday available capital reflects realised losses; prior charges and profits appear in the next opening balance; quantity from the expected SL loss/risk distance, with maximum capital per trade as the ceiling; live only (A-005_v2_design_inputs.md).
18. The central/global score/rank allocator is reconsidered in the strategy/filtering architecture (A-008, D-017; VP-03).
19. Do not turn suggestions into code until the relevant agenda topic is discussed and frozen.

## 18. External references (checked 2026-10-06)
- Zerodha support, intraday auto square-off timings: https://support.zerodha.com/category/trading-and-markets/trading-faqs/market-sessions/articles/intraday-auto-square-off-timings
- Zerodha Z-Connect, closing auction session (10-Jul-2026; effective 3-Aug-2026): https://zerodha.com/z-connect/general/everything-you-need-to-know-about-closing-auction-session-cas
- Zerodha Z-Connect, product updates 10-Aug-2026 (CAS-stock square-off revised to 3:12 PM from 3:10 PM): https://zerodha.com/z-connect/?p=454739
- Zerodha support, auto square-off charge: https://support.zerodha.com/category/account-opening/charges-at-zerodha/articles/auto-square-off
- Zerodha support, MIS not allowed on certain scrips: https://support.zerodha.com/category/trading-and-markets/trading-faqs/general/articles/mis-not-allowed-on-certain-scrips
- Zerodha support, BE series MIS block: https://support.zerodha.com/category/trading-and-markets/margin-leverage-and-product-and-order-types/articles/what-does-the-error-rms-blocked-for-be-nse-cm-mis-block-type-all-mean
- Zerodha support, cover orders: https://support.zerodha.com/category/trading-and-markets/product-and-order-types/order/articles/what-are-cover-orders-and-how-to-use-them
- Zerodha Z-Connect, short delivery: https://zerodha.com/z-connect/?p=149
- Trading Q&A by Zerodha, CNC bought and sold the same day: https://tradingqna.com/t/cnc-same-day-and-next-trading-day/49667
- Zerodha support, GTT: https://support.zerodha.com/category/trading-and-markets/gtt/articles/how-can-i-use-the-gtt-feature
- Zerodha support, GTT buy OCO (F&O only): https://support.zerodha.com/category/trading-and-markets/gtt/articles/gtt-buy-oco
- Zerodha Z-Connect, trailing stoploss on Kite web (23-Jun-2026): https://zerodha.com/z-connect/?p=450973
- Zerodha support, Kite Connect API FAQs: https://support.zerodha.com/category/trading-and-markets/general-kite/kite-api/articles/kite-connect-api-faqs
- Kite Connect forum, notes on the NSE API circular: https://kite.trade/forum/discussion/15350/notes-on-the-nse-circular-prescribing-operating-procedures-for-api-usage
- Kite Connect forum, staff replies on automated login: https://kite.trade/forum/discussion/comment/52713/ and https://kite.trade/forum/discussion/comment/52850/
- Kite Connect docs: user and login https://kite.trade/docs/connect/v3/user/ ; orders https://kite.trade/docs/connect/v3/orders/ ; GTT https://kite.trade/docs/connect/v3/gtt/ ; margins and charges https://kite.trade/docs/connect/v3/margins/ ; postbacks https://kite.trade/docs/connect/v3/postbacks/ ; rate limits and exceptions https://kite.trade/docs/connect/v3/exceptions/ ; sandbox https://kite.trade/docs/connect/v3/sandbox/

## 19. Topic boundaries and carry-forward (ChatGPT review, 06-Oct-2026, points 7 and 10)
A-006 must not silently absorb later topics' decisions. This separation stays:

| Topic | Settles | A-006 items carried there |
|---|---|---|
| A-006 Broker model | Intended V2 broker; broker authority for live broker facts; respect for broker capability and eligibility; broker-side protection where appropriate; current broker constraints verified before implementation | Recorded here as PROPOSED (sections 2, 4, 5, 7) |
| A-007 Product types | MIS/CNC/CO/other product semantics; which product belongs to which strategy class; intraday and delivery product rules | Q-27 answer (MIS for every intraday trade; reject, never convert to CNC); Q-28 answer (CNC entry, then OCO GTT); CO option; BO unsupported; product eligibility (Q36) |
| G / H Order execution, open positions | Exact order construction; partial fills; timeout; retry; SL/target; cancellation; reconciliation actions | Q9, Q11, Q19-Q22, Q25, Q26-Q28 (checklist), Q32-Q35; GTT construction; MIS pre-check mechanism |
| A-010 / M-005 Secrets, token | Authentication and token lifecycle | Q4, Q5; Q-29; static IP; Q-11 |
| F, I, K, L, N | Capital, safety, state, evidence, testing | Q12-Q18 (F); Q24 (I); Q3 (K); Q41-Q44 (L, K-011); Q40 (N) |

Final status (ChatGPT point 10): A-006 legacy manual FROZEN (DR-021); ChatGPT's 25-Sep reconciliation stays as filed in docs/inputs (immutable); this file stays PROPOSED / OPEN, with its dependencies carried to the topics above.
