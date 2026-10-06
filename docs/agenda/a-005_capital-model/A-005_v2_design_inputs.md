# A-005 CAPITAL MODEL: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md, mainly at section F (capital/risk), A-009 (configuration) and G (order execution). Updated 2026-10-06 with Rama's answers to Q-22 and Q-24 and ChatGPT's final A-005 review.
Date: 2026-10-06
Place at: docs/agenda/a-005_capital-model/A-005_v2_design_inputs.md
Why this file exists: Rama asked that every point discussed in A-005, including points belonging to other topics, be kept with A-005 and nothing skipped (A-005 further-clarifications file, item 11). The legacy manual records legacy fact only (two-layer rule, V2_DESIGN_LOG W-1), so the V2 material is kept here in full and summarised in V2_DESIGN_LOG (VP-12, VP-13, VP-29 to VP-33). ChatGPT's final review accepted this layering.
Sources: Rama's three A-005 reply files (23-24 Sep); the capital-flow spreadsheet (docs/inputs/chatgpt_originals_and_replies/A-005_FINAL_CLARIFICATION-Capital flow design.xlsx); the 24-Sep reconciliation and manual (V2 sections and the appended strict-scope block); ChatGPT's 23-Sep Zerodha-margin review; Rama's 06-Oct answers (chat and the message shown to ChatGPT); ChatGPT's final review (A-005_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_06-OCT-2026_FINAL.txt).
Rule: every number below is an EXAMPLE, never a frozen value. No business value is hardcoded in Python; all user/business values come from config.yaml or strategy.yaml (Rama Q19: "100% mandatory").

## 1. Order of intent (latest last)
1. 23-Sep pending-questions answers (Q1 to Q20 and Steps 1 to 17).
2. 24-Sep further clarifications (resolve Q7/Q8, Q9 to Q11, Q4, Q5, Q6, Q7 timing).
3. 24-Sep capital-flow review (MIN rule confirmed).
4. "NEW SYSTEM CHANGES — STRICT SCOPE" block appended to the 24-Sep reconciliation and manual.
5. Rama's 06-Oct answers: confirm the strict-scope quantity rule (Q-22) and give the pending-order rule (Q-24). These are the latest statements.

Superseded earlier answers (kept for history):
- Q8 "daily reference is authoritative" became: daily margin DB is authoritative for the day's sizing basis; the order-time API is the final execution safety check and can reject or adjust (further clarifications Q1).
- Q10 "no API data, reject" became: no margin data means 100% cash, proceed, alert (further clarifications Q2).
- Q12 and Step 10 notional-based quantity (quantity = cash / margin % / price) became risk/SL-based quantity with the capital amount as a ceiling (strict-scope item 3, confirmed 06-Oct).
- Steps 7-9 compared the strategy cap with the CASH amount before margin (Rs 700 vs Rs 500, then Rs 500 / 33.33%); Rama's 06-Oct statement compares the YAML maximums AFTER margin, on trading capital (section 3).
- Step 10 "divide by 3 for entry + SL + target": not adopted (section 8).
- Step 12 "ignore the smaller last trade": superseded by the MIN rule (section 3).
- Q6 margin refresh at 09:00 became 08:30 (further clarifications Q7).

## 2. Capital basis and daily lock
- Basis: broker-reported real cash at the 09:15 opening (Q1). Broker is the source of truth because charges and profits settle after the close.
- Split: config.yaml sets the intraday and delivery percentages (Q2; example 70/30).
- Lock: segment capital is locked for the whole day; intraday broker syncs must not raise the per-trade basis (Q8 reasoning, Step 11, further Q8).
- Reconciliation: periodic system-vs-broker capital comparison with Telegram and email drift alerts; it reports drift but never rebases today's sizing capital; the next day's opening balance is the new basis (Step 10, Q17, further Q8).
- Principle (Q17): one real-cash basis, explicit system reservations, broker-side reconciliation.
- Final principle confirmed (Q20): "V2 will use one real-cash capital basis. Intraday and Delivery cash budgets are derived from configuration. Intraday quantity is constrained using current broker eligibility and broker-specific required margin rather than a universal leverage assumption. Broker order-margin information may provide order-time validation. No broker margin data means no invented leverage. All configurable business values remain YAML/configuration-driven."

## 3. Three different amounts (terminology)
- A. Cash allocation: real broker cash allocated (and, if decided, reserved) for one trade.
- B. Trading capital per stock (permitted notional): cash allocation / broker required margin %. Rama calls this "trading capital per stock"; the 24-Sep spreadsheet column "Capital available after Margin" is this amount, not remaining cash.
- C. Order quantity: shares, from risk/SL, capped by B (section 4).
Never compare a cash amount with a notional amount without converting first (ChatGPT final review, point 2).

## 4. Capital-to-quantity sequence (Rama, 06-Oct; PROPOSED)
1. Opening cash, split by config.yaml into intraday and delivery segment cash; locked for the day.
2. Base cash per trade = locked segment cash / that segment's configured maximum FILLED trades (Q4, Step 6, Q-24). Example: Rs 7,000 / 10 = Rs 700. (For delivery, Step 6 reads "Rs 3k / 2 = Rs 500", where the arithmetic gives Rs 1,500; the spreadsheet uses Rs 3,000 / 3 = Rs 1,000. Examples only.)
3. Cash allocation = MIN(base cash per trade, current available segment cash) (capital-flow review, confirmed "YES"). Later trades can be smaller after earlier losses; if available is higher, spending stays capped at the base.
4. Trading capital per stock = cash allocation / broker required margin % (2x-5x, i.e. 20%-50%, per stock). No usable margin data means 100% cash (section 7).
5. Compare with the YAML maximums: capital ceiling = MIN(trading capital per stock, config.yaml maximum, strategy.yaml maximum), "whichever is low" (Rama 06-Oct; spreadsheet rows 28-29). On this reading the YAML maximums are expressed in trading capital (notional), consistent with legacy notional caps such as max_position_value_pct (qty x price).
6. Quantity from risk/SL: risk_qty = FLOOR(risk budget / SL distance per share), with the risk-per-trade % (or SL %, name to be fixed) taken from the signal's strategy.yaml; Rama: "refer current system behaviour in GitHub if required" (legacy: risk budget = total capital x risk_per_trade_pct; A-005 manual section 6).
7. capital_qty = FLOOR(capital ceiling / intended entry price).
8. Final quantity = MIN(risk_qty, capital_qty, any YAML maximum quantity), then approved instrument, lot, tick and other guards. A maximum quantity can bind on its own (Step 15: max qty 1 means 1 share).
- Global rounding (Step 9): round rupees and quantity down to whole numbers.
- Do not revert to cash / margin / price as the primary quantity rule; it is only the ceiling (ChatGPT final review, point 1).

Points still open (Q-25, for F-004 and A-009):
- Ordering of the YAML cap. ChatGPT's final review placed the strategy cap BEFORE margin, on cash (MIN of config cash, strategy cash cap and available cash, then divide by margin %). Rama's 06-Oct wording places the YAML comparison AFTER margin, on trading capital. The same YAML number gives different results. Example: cash Rs 700, strategy cap 1,500, stock margin 20%: cap-after-margin gives MIN(3,500, 1,500) = Rs 1,500 notional; a cash cap of Rs 500 before margin gives Rs 500 / 0.20 = Rs 2,500. This file records Rama's latest wording; the units of each YAML key must be defined explicitly before implementation.
- Risk budget basis: which capital the strategy risk % applies to (legacy: total capital; spreadsheet: SL% x margin-adjusted capital), and whether the strategy "SL %" is the risk budget % or the stop distance %. If the risk % equals the stop-distance %, risk_qty equals the notional quantity; they differ only when the two percentages differ.
- Risk-based adjustment (Q14): the per-trade amount may also be adjusted by risk metrics such as a tier multiplier or an improved version; the legacy PositionSizer is not assumed retained.
- Spreadsheet note: its quantity column (capital / SL amount = 66.67 on every row) is not a share count; the sheet itself says to ignore quantity there (row 30).

## 5. Realised result and available capital
- Strict-scope item 2: intraday available capital reflects realised LOSSES only; profits and trading expenses are adjusted by the broker after the close (around midnight).
- EXTERNAL CONTEXT: Zerodha states intraday profits are not available for trading until T+1 settlement; the original capital becomes available after exit.
- Cancelled, rejected or unfilled orders leave available capital unchanged (capital-flow review).
- Earlier results affect current available segment cash; the base allocation is not permanently available to every trade regardless of earlier results (ChatGPT final review, point 3).

## 6. Daily trade limit and pending entries (Rama, 06-Oct; PROPOSED)
- The daily trade limit counts broker-FILLED trades only: filled consumes a slot; queued/pending, cancelled and rejected do not (Step 16, further Q4, Q-24).
- Rama's rule (example, maximum 10): 9 trades filled; the next order is submitted; if it is still pending, a later order may be submitted; when any order fills and the count reaches 10, immediately cancel the other pending entries; if one of them fills before its cancellation is confirmed, accept it (11 filled) and block all further entries.
- Safeguards (ChatGPT final review, points 4-7):
  - Pending entries do not consume FILLED slots, but every additional submission remains subject to capital availability, pending-order controls, strategy/risk limits, duplicate rules, position limits, broker constraints and any configured maximum outstanding/pending-order policy. Never "submit unlimited orders until one fills".
  - The 11th fill is a bounded execution-race exception, not normal capacity: block new entries, reconcile broker and local state, record it as a race exception, alert, keep the evidence; never cancel or reverse a genuine fill to make the count equal 10.
  - Pending orders may still need capital reservation even though they do not count as filled trades.
- Still open (Q-26): how much capital a pending entry reserves, for how long, and how that reduces the capital for later entries; and the maximum number of simultaneous pending entry orders. Neither is decided here. Context: legacy reserved capital before every order placement (A-005 manual section 8), which bounds the capital an extra fill can use.
- The intraday entry cut-off ("soft-kill / no new entry") is a separate control from capital and trade count (spreadsheet row 20; A-003 VP-19).

## 7. Margin data
- Daily snapshot: refresh at about 08:30 after boot, token and instrument updates; replace the whole table daily to avoid stale or duplicate rows; Telegram and email alert on success AND failure; keep dated snapshots for audit ("why was this quantity calculated on that date?") (Steps 1, Q10, Q18, further Q7). Refresh time configurable (Q6).
- Authority: daily snapshot = the day's sizing reference; order-time broker margin API = final execution safety check that may reject or adjust (further Q1; Q7 secondary validation).
- Missing data: look in the daily DB, then the order-time API; if neither has usable margin, use 100% cash (zero assumed leverage), proceed, and send a per-stock incident alert so silent repeats cannot happen (Q9, Q11, Step 14, further Q2). Never fall back silently to a universal 5x (Q10, Q20).
- Delivery / CNC: nil margin, 100% cash (Step 17).
- Considerations:
  - MIS eligibility is a separate broker control from margin data: Zerodha's margin calculation does not by itself prove a stock is MIS-eligible (ChatGPT 23-Sep, citing a Zerodha developer discussion). An MIS order for an MIS-blocked stock is rejected by the broker even with 100% cash. Legacy learned this through its MIS blocklist (A-004 section 6). V2 must decide what happens to such signals (links VP-27).
  - Ordering: in legacy the instrument master refreshed at 09:00 (A-004). If the 08:30 margin snapshot is keyed to the instrument list, the instrument refresh must run first (Rama confirmed 08:30 "unless ambiguity or collisions").
  - Store "not eligible / no data" as a status, never as 0% margin (0% would mean infinite leverage) (ChatGPT 23-Sep).
  - Zerodha's Kite Connect sandbox does not support the margin APIs; validate with the production API before relying on it (ChatGPT 23-Sep).

## 8. Entry margin versus SL / target
- Size from the ENTRY order's broker margin; SL and target do not get separate capital allocations unless actual broker behaviour requires it (further Q6, option A).
- Verify on Zerodha's site before implementing (Rama's explicit note): product-specific margin; how pending/open orders affect margin; GTT creation needs no funds but funds are needed when it triggers; exit SL treatment versus new orders; API response semantics.

## 9. Configuration and strategy YAML governance
- Effective maximum = LOWER(config, strategy); effective minimum = HIGHER(config, strategy), applied only where the same setting exists at both levels with the same meaning and unit (Q15, further Q5).
- Move per-trade/strategy caps (maximum capital per trade, maximum position value, maximum quantity, and minimum mirrors) from the central config to each strategy.yaml; keep global controls (alerts on/off, maximum trades per segment, and similar) in config.yaml (Step 3, Q15).
- Before designing: deeply review the existing config.yaml and every strategy.yaml, then assign each setting to global, strategy, broker/instrument or operational scope, or remove it (further item 10).
- Before designing: ask Rama which legacy controls to remove, add or modify (Q16). Rama has already said the concentration cap is removed (Q16). Others (position-value cap, score/tier multiplier, performance multiplier, daily loss limit, consecutive-loss limit, open-position limit) stay for agenda-by-agenda review.
- Do not invent minimum settings where legacy has none.

## 10. Cross-topic inputs (kept here so nothing is lost)
- Configuration: review config.yaml and every strategy.yaml deeply; separate global and strategy settings; do not carry every legacy setting forward.
- Margin data operations: daily replace; no stale or duplicate rows; success and failure alerts; stock/trade-identifiable incidents; dated snapshots; silent repeated failures unacceptable.
- Observability: periodic capital-drift alerts; Telegram and email for critical operational events; keep evidence for forensic review (including any filled-limit race exception).
- Broker verification before implementation: current Zerodha margin behaviour; product-specific margin; pending/open-order treatment; GTT trigger funding; entry/exit order margin; API response semantics.
- Constraint governance: maximum = lower bound; minimum = higher bound where meaningful; maximum quantity can bind independently; setting-by-setting precedence and units set during the configuration review.
- One trade per symbol per day, irrespective of direction (strict-scope item 1; VP-12).
- Live operation only; no paper mode (strict-scope item 4; DR-001).
- General: review every later agenda topic again before V2 design.

## 11. Broker references
- Zerodha Equity Margin Calculator: https://zerodha.com/margin-calculator/Equity/
- Kite Connect margin calculation API: https://kite.trade/docs/connect/v3/margins/
- Kite Connect user margins API: https://kite.trade/docs/connect/v3/user/
- Kite Connect sandbox limitations: https://kite.trade/docs/connect/v3/sandbox/
- Zerodha developer discussion on MIS eligibility vs margin API: https://kite.trade/forum/discussion/8400/kite-connect-api-update
- Zerodha support, same-day profits (T+1): https://support.zerodha.com/category/trading-and-markets/margin-leverage-and-product-and-order-types/articles/same-day-profits

## 12. Worked example (EXAMPLE values only)
Opening cash Rs 10,000; intraday 70% = Rs 7,000; maximum 10 filled trades gives Rs 700 cash per trade. ITC at 20% required margin: trading capital Rs 700 / 0.20 = Rs 3,500. If the strategy.yaml maximum (trading capital) is Rs 2,500, the ceiling is Rs 2,500. With risk 1% of a Rs 10,000 basis (Rs 100; the basis is illustrative and open under Q-25) and an SL distance of Rs 2.50 on a Rs 250 stock, risk_qty = 40 and capital_qty = FLOOR(2,500 / 250) = 10, so 10 shares. A stock with no margin data: trading capital Rs 700 (100% cash). After 10 fills, new entries stop; a pending entry that fills before its cancellation is the bounded race case.
