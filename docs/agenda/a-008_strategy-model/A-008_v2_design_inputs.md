# A-008 STRATEGY MODEL: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md, at the topic that owns them (W-6): scanner conditions at C-001; filtration, entry timing and ranking in Section D; stop and R:R in E; caps, risk and the circuit breaker in F; the config/strategy split at A-009; the strategy count at the end of Section A (with VP-01 and VP-02). Updated 2026-10-07 after ChatGPT's value-added review (docs/agenda/a-008_strategy-model/A-008_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_07-OCT-2026.txt): legacy references made evidence-bounded; everything here stays PROPOSED / OPEN (section 17). The A-008 legacy manual is frozen (DR-023).
Date: 2026-10-07
Place at: docs/agenda/a-008_strategy-model/A-008_v2_design_inputs.md
Why this file exists: Rama asked that every point be kept under suitable headings without missing anything, and that his 07-Oct note on the Chartink scanners be recorded. The legacy manual records legacy fact only (V2_DESIGN_LOG W-1, W-3), so the V2 material is here and summarised in V2_DESIGN_LOG.
Sources: Rama's answers in docs/inputs/chatgpt_originals_and_replies/A-008_STRATEGY_MODEL_DISCUSSION_CHECKLIST_27-SEP-2026.txt; his 07-Oct note sent with Chartink_scanners.v2.txt; ChatGPT's 29-Sep R1 manual (section 28) and reconciliation (sections 26-27); the [Correct A-006] broker checklist (section 14); legacy evidence in A-008_manual.md; Chartink and StockCharts pages checked 2026-10-07 (section 16).
Labels: RAMA = Rama's suggestion (quoted or closely paraphrased; his answers are suggestions only). OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision. LEGACY = what the old system did (A-008_manual.md), for comparison only, not a requirement. CURRENT EXTERNAL FACT = a current third-party rule with its check date.
Rule: no business value is hardcoded in Python; numbers below are examples or legacy values.

## 1. Order of intent (latest last)
- A-002 (VP-08): at most about six strategies, each with its own YAML and its own secondary filtration; S&R after strategies and processors; R:R honours S&R. Objective: quality over quantity; avoid late entries, unsuitable R:R and stocks that have already run.
- A-005 (Q-22, VP-13): risk % from the signal's strategy.yaml; YAML maximums (VP-32).
- A-006 Q10 (VP-03): Chartink > per-strategy processor (filter, validate, entry or reject) > config.yaml and strategy.yaml > position size > entry order.
- A-007 checklist 20.3 and 22.5 (VP-42): trade_type in config.yaml as the master; each strategy.yaml has ON/OFF, classification and direction. Checklist 15 (VP-12): "One Symbol per Day only ... at any cost".
- A-008 checklist (27-Sep): Q5 (Chartink naming uniformity), Q7 (remove force_intraday_only; review config contents with Claude), Q19 (SL % in each strategy.yaml), Q26 to Q28 (same-symbol rule; all caps in strategy.yaml), Part 14 (ten cross-topic inputs, sections 3 to 5 below).
- 07-Oct, sent with Chartink_scanners.v2.txt: "Current strategies - remember need to change all these Why? Being late entries which hits 98% SL as of now [So improved or bit early entry required, note it]".

## 2. Earlier entries: redesign every scanner (RAMA 07-Oct; VP-44)
- RAMA (07-Oct, verbatim): "remember need to change all these Why? Being late entries which hits 98% SL as of now [So improved or bit early entry required, note it]".
- RAMA (Q17): "Late entry signals, meaning a stock which already ran-out for the day received as entry signal that leads to SL hitting [No room for further stock run]". (Q32.1): "Strategies has conditions which sends signals/stocks which already ran for the day (that causes all hits SL, So success ration is around <10% only)".
- LEGACY (A-008_manual.md sections 17-18): all 16 scanners used the forming daily candle; their conditions could become true only after the current-day values crossed the thresholds, so they could produce signals after a substantial part of the move. This is a credible mechanism for late signals, but the legacy evidence does not establish that every signal was late or quantify the delay. The pipeline added its own delay (no entry before 10:00, screening, a pullback wait). Rama's "98% SL" is his report for an unspecified period; the separate BK-1 measured dataset (15-Jun to 16-Jul-2026) shows a long win rate of 36.9%, 69 stop hits against 44 target hits, and intraday longs negative even before costs.
- OPEN: the new scanner conditions. Replacing all current scanners is Rama's instruction; it stays PROPOSED until decided at C-001 and in Section D.
- CLAUDE CONSIDERATIONS (C-001, D-001, D-013 to D-016, E-002; none decided):
  - Define "late" before redesigning, in terms V2 records on every signal: time of signal; distance already travelled from the open and from VWAP, in % and as a share of the stock's average daily range; distance from the day's high or low; room left to the planned target and to the nearest S&R level. The legacy database is gone, so V2 must collect its own evidence that earlier signals do better.
  - Write intraday setups on intraday candles. Chartink offers intraday timeframes and scans 1-minute data on its paid plan (CURRENT EXTERNAL FACT, section 16), so an opening-range, VWAP or pullback setup can use 5- or 15-minute candles instead of the forming daily candle; daily conditions stay as context (trend, levels, liquidity).
  - Replace "today's volume > 1.5 x average daily volume", a cumulative threshold that may be reached late depending on the stock's intraday volume curve, with a time-of-day comparison (for example the current intraday candle's volume against its usual volume), so a surge can be caught when it starts.
  - Add an extension guard: reject a signal when price has already moved more than a configured share of its average daily range, or when the room to the target or the next S&R level is smaller than the strategy's R:R needs (links VP-08 "R:R honours S&R", D-009 to D-013, E-008).
  - Choose the Chartink alert mode together with the same-symbol rule: "Unique" sends one alert per stock per day; "Duplicate" repeats while the stock keeps matching (a legacy code comment records SENCO alerts every 5 to 6 minutes; the legacy alert setting itself is UNKNOWN).
  - Count every built-in delay (an entry start time, a pullback wait, a candle-close wait under VP-09) when judging how early a signal really is.

## 3. Strategy count and distinct strategies (RAMA; VP-08)
- RAMA (Part 14.1, from A-002): "Future V2 may use a maximum of approximately six strategies." (Part 14.9): "Future V2 should not blindly carry the legacy 16-strategy model." (Q32.3): "Almost duplicate of some strategies with different names (refer chartink_scanners.v2.txt), meaning no distinct unique strategy".
- LEGACY (A-008_manual.md section 17): 16 Chartink definitions = 15 normal trading definitions + PB01 (shadow, non-trading); the 15 can be grouped into about five condition families. One scanner-condition duplicate: POSITIONAL SWING LONG and OPEN LOW BREAKOUT LONG have identical conditions but fed different strategies (delivery and intraday), so they were not identical strategies. Scanner subsets: every GAP FADE LONG or FIRST PULLBACK LONG match was also a VWAP BOUNCE LONG match, mirrored on the short side, and MOMENTUM sits inside SECTOR ROTATION; the GAP GO and OPEN HIGH/LOW pairs differ by one condition; "sector rotation" has no sector logic; the range-breakout conditions may be self-defeating (audit lead). BK-1 (small samples, one market regime): vwap_bounce_long was the largest drain (-12.10 R, n=35); positional_sector_rotation the best (+4.46 R, n=19).
- CLAUDE CONSIDERATIONS:
  - Admit a V2 strategy only if its conditions are not a subset or near-copy of another's; check overlaps mechanically when a strategy is written or changed.
  - Name, description and conditions must describe the same setup (several legacy descriptions did not).
  - BK-1's per-strategy figures can inform which ideas to drop, but the samples are small, cover one regime and come from the late daily-candle scanners; redesigned scanners need their own evidence.
  - Each strategy declares one direction (as legacy did); a long and a short version of an idea are two strategies.

## 4. Per-strategy secondary filtration (RAMA; VP-03, VP-08)
- RAMA (Part 14.2, from A-002): "Each strategy may have its own secondary filtration mechanism." (Q32.2) legacy defect: "Global Secondary filtration irrespective of strategy/segment (Intraday/Delivery)". (Part 14.8): "Central/global score/rank allocator needs reconsideration."
- LEGACY (manual section 12): one common 10-step screener; only three per-strategy thresholds; pass score 60 for every strategy (min_score 0 in all); a code record says the score could not exceed 65, so HIGH (80) was unreachable and every passing trade was sized at 50% or 70%.
- CLAUDE CONSIDERATIONS (D-003, D-017):
  - Define each strategy's filter together with its scanner, so the filter checks what the scanner cannot (extension, room to target, structure) instead of re-checking what the scanner already guarantees.
  - No dead inputs: every scoring step must be able to score, and a tier that cannot be reached is a defect.
  - Whether V2 ranks candidates at all, and how ranking interacts with the capital and trade limits, is decided at D-017 and in F.

## 5. Config master, strategy secondary (RAMA; VP-42, VP-32, VP-15; A-009)
- RAMA (Q6): "Config.yaml is global/universal controller and Strategy.yaml are sub or secondary controllers [Assume: In config.yaml: BOTH (Intraday and Delivery) is enabled then and Delivery has 5 strategies, But if want to trade only 3 straegies, simply I'll turn/enable on those 3 strategies only & At the same time if I want to block full delivery trades then 'Config.yaml=INTRADAY']".
- RAMA (Q7): "in new system remove these kind of complications why? If Config.yaml=INTRADAY means it's already overridden condition, So again adding force_intraday_only has no meaning, So Config.yaml=INTRADAY/DELIVERY?BOTH then each strategy.yaml=enabled/disabled is sufficient & Take a careful note in manual 'While implementing Config.yaml and strategy.yaml files and it's contents needs to discuss/review again carefully with Claude'".
- RAMA (Q24): in legacy, major controls (maximum, minimum, time) were global in config; each strategy.yaml held R:R, classification and similar.
- RAMA (Q26.2): "each strategy.yaml should have all caps on it's own (instead of config.yaml which will have only global settings like alerts (on/off), master start & end time etc)".
- RAMA (Part 14.6, 14.7, 14.10): strategy YAML holds strategy-specific controls while global config provides global bounds and standards; trade_type is the master segment control and the strategy YAML carries classification and direction; all business and decision values are YAML-driven, not hardcoded in Python.
- RECORDED as Rama asked: "While implementing Config.yaml and strategy.yaml files and it's contents needs to discuss/review again carefully with Claude" (carried to A-009 and the build phase).
- LEGACY (manual sections 6 and 10-13): trade_type, force_intraday_only and delivery_enabled gated products; force_intraday_only made delivery strategies dormant and had the adapter coerce products to MIS (no intent rewrite after 10-Jul-2026; A-007 erratum E-1); each YAML held its stop %, R:R, entry offset, pullback behaviour, three screening thresholds and a position cap; the risk %, portfolio limits, pass score and tiers were global; for many YAML keys no effect was identified in the inspected production path.
- CLAUDE CONSIDERATIONS (A-009):
  - Remove force_intraday_only: trade_type INTRADAY already rejects every delivery strategy, and the adapter's coercion to MIS changes a product silently, against Q-27 / VP-41. Decide separately whether delivery_enabled (a capability lock at the broker boundary) survives.
  - Every YAML key must take effect or fail at load; legacy carried order_protocol, smart_tgt_enabled, ATR multipliers and a gap buffer with no effect identified in the inspected path, and per-strategy start times that never bound.
  - A value identical in every strategy (legacy R:R 1.5, min_score 0) is either a global setting or should genuinely vary; decide which.
  - Rama's "all caps in strategy.yaml" and "config only for global settings" need one boundary: account-wide limits (maximum open positions, daily trades, daily loss, kill switch) cannot live only in strategy files, because no single strategy sees the whole account. VP-32 (effective maximum = the lower of config and strategy) and the single risk engine (project instructions 6.5) apply.
  - Per-strategy entry windows, if kept, sit inside the global window and are validated at load so they can actually bind (legacy start times never did).

## 6. Stop % and risk per strategy (RAMA; Q-25)
- RAMA (Q19): "ensure in new system each strategy wise .yaml file(s) has this settings [SL %]". (A-005, Q-22): risk % from the signal's strategy.yaml.
- LEGACY: two separate values: sl_pct per strategy was the stop distance from entry; risk_per_trade_pct was global, the share of capital at risk (a per-strategy risk % was deleted on 24-Jun-2026 because nothing used it).
- OPEN (Q-25 (b)): is V2's strategy "SL %" the stop distance, the risk budget, or are both needed as two keys?
- CLAUDE CONSIDERATION: if both live in strategy.yaml, give them names that cannot be confused (for example stop_distance_pct and risk_per_trade_pct) and state which capital the risk % applies to.

## 7. Same-symbol rule (RAMA; VP-12; Q-33)
- RAMA (A-002; A-005 strict-scope item 1): one trade per stock per day in any direction. (A-007 checklist 15): "in new system it should be 'One Symbol per Day only' meaning no more second trading of same symbol again at any cost".
- RAMA (A-008 Q26.1): "legacy one stock + one direction once a day per segment (meaning Intraday & Delivery has 2 independent pipelines) Vs new system legacy one stock + one direction once a day (irrespective of intraday or Delivery pipeline)". (Q28): "Future system should be 'one stock + one direction once a day'".
- LEGACY (manual section 14): from 3-Aug-2026 one executed trade per symbol and direction per day, counted per book from 9-Aug-2026; the opposite direction always allowed; an open position blocked the symbol account-wide while open.
- OPEN (Q-33): the statements agree that the rule spans intraday and delivery but differ on one point: may the same stock trade again the same day in the opposite direction (exit a long, then go short)? "One stock + one direction once a day" allows it; "One Symbol per Day only ... at any cost" forbids it. The A-007 and A-008 checklists are both dated 27-Sep. Rama's A-009 to A-011 answers (29-Sep to 01-Oct) are later and will be checked first; Rama is asked only if they do not settle it (ChatGPT's 07-Oct review, point 9, agrees).
- CLAUDE CONSIDERATION (already in VP-12): decide whether a delivery holding carried from an earlier day blocks a new intraday trade in the same symbol.

## 8. Chartink identity and naming (RAMA; VP-45)
- RAMA (Q5, paraphrased with the account IDs left out): two VMs, production and testing, each consumed Chartink signals; production strategy names, URLs and scanner names did not use the Zerodha client ID; testing names used the testing client ID, which then appeared in the Chartink payload; "We manually adjusted something in testing system about not to consider" it. Verbatim: "So in new system need uniformity (either create strategies with client id or without client id and adjust somewhere is system or any other simplified but universally to be followed semantics/common approach [meaning no more one time adjustments like this as done in testing vm (refer testing vm repo in GitHub)". And: "carefully take this note into manual with suitable headings/area if it's relevant or not relevant to topic A-008" (done: A-008_manual.md section 9 and this section).
- LEGACY (manual section 9): the fix was shared code (WR4b, 09-Sep-2026) that strips the account tag from the alert name; the code says Chartink names alerts per account on a shared login and the account name "cannot be dropped in the Chartink UI"; before the fix every testing signal was rejected.
- CLAUDE CONSIDERATIONS (C-001, C-002, C-010, A-009, A-010):
  - One naming rule for every environment: the webhook path, or a payload field the system controls, carries the strategy key; an alert's display name is never used for routing.
  - The environment comes from configuration, never from the alert name. If Chartink forces an account prefix, the handling rule is documented once and tested in the testing environment before production.
  - An unknown or mismatched scanner is rejected, recorded and alerted by Telegram and email (VP-34, VP-11).
  - Webhook secrets stay out of documents and screenshots (A-010).

## 9. Direction and strategy identity (CLAUDE CONSIDERATION; links Q-03)
- LEGACY: direction was a required YAML field and the only source of the order side; a registry mirrored it and flagged conflicts; one strategy key was used for the file name, scanner map, webhook path and database.
- CLAUDE CONSIDERATIONS: keep direction and intent as declared fields; use one stable strategy key everywhere (file, route, database, logs) so traceability (Q-03) survives renames; a direction-conflict check matters only where a side can arise outside the strategy (for example an adopted broker position).

## 10. Strategy circuit breaker (CLAUDE CONSIDERATION; F-009, I)
- RAMA (Q10): UNKNOWN whether it was active.
- LEGACY: enabled; paused a strategy for the day after a large early loss; the pause was held in memory, so a restart could lift it (ruled wrong on 04-Aug-2026); its alerts were lost until 24-Jun-2026.
- CLAUDE CONSIDERATIONS: decide at F-009 / I whether V2 needs a per-strategy daily pause besides the account-level loss limit; if kept, persist the pause, define gross or net P&L (VP-40) and the minimum history needed.

## 11. PB01 and end-of-day signals (VP-14; C-011, J-013)
- RAMA (Q16): PB01 was a placeholder.
- LEGACY: PB01 was an end-of-day scanner captured to a watchlist, with a next-morning retest check that wrote would-be records only.
- Link: VP-14 ("analyse today, trade tomorrow") is decided at C-011; any retest engine at J-013.

## 12. Production and testing parity (project instructions 9.4)
- RAMA (Q31): the only known difference was the start time: production 10:00, testing 09:30.
- CLAUDE CONSIDERATION: V2's testing and production environments use the same strategy and config values unless a difference is deliberate, recorded and reconciled (project instructions 9.4).

## 13. Live only
- DR-001: V2 is live-only, so the paper-versus-live questions (Q29, Q30) do not apply to V2.

## 14. The [Correct A-006] broker checklist (filed with A-008)
- The file is a second-pass Broker Model (A-006) checklist dated 27-Sep and labelled A-008 by mistake. Its 12 questions have no answers, and its 15 "new-system inputs already recorded" are all in A-006_v2_design_inputs.md (VP-29, VP-30, VP-34 to VP-40). It adds nothing new and changes nothing in the frozen A-006 manual. It stays in docs/inputs as supplied.

## 15. ChatGPT's 29-Sep V2 inputs (kept so nothing is lost)
- From its R1 manual section 28 and reconciliation sections 26-27, each mapped above: do not blindly carry the 16-strategy model, and use a smaller set (section 3); per-strategy secondary filtering and a rethink of the universal score/rank allocator (section 4); strategy YAML holds strategy controls while global config is the master, force_intraday_only reconsidered, business values configuration-driven, config structure reviewed with Claude during implementation (section 5); strategy-wise SL % (section 6); one stock + one direction once a day as the stated rule (section 7, Q-33); universal Chartink naming and payload semantics (section 8).

## 16. External references (checked 2026-10-07)
- Chartink stock screener FAQ (chartink.com/articles/scanner/stock-screener-faq): values change while the market is open; on the paid plan scans use data up to the previous minute (a 9:20 scan uses data to 9:19:59); the free plan is 5 minutes delayed; intraday timeframes start at 1 minute (paid) or 5 minutes (free).
- Chartink alerts guide (chartink.com/articles/alerts/alerts-guide): alerts can run every 1 to 240 minutes or at market open or close; "Duplicate" repeats alerts when a stock matches again during the day; "Unique" sends one alert per stock per day.
- StockCharts scan discussion (scan.stockcharts.com/discussion/comment/6454): a value cannot be greater than a maximum that includes its own period ("can't be greater than the max, only equal"); compare with the maximum as of one period earlier (the convention behind "1 day ago Max(20, ...)"). Context only: it is not proof of Chartink's implementation (ChatGPT's 07-Oct review, point 4).
- These are current external facts; they are not evidence of the legacy Chartink plan or alert settings, which remain UNKNOWN (A-008_manual.md section 17).

## 17. Status and boundary
- Everything here is PROPOSED or OPEN; a ChatGPT review alone does not turn any of it into a decision (W-6), and no register row is created because a consideration looks sensible.
- ChatGPT's 07-Oct review (points 8 and 11) confirms these stay PROPOSED / OPEN and are decided by their owning topics: the final V2 strategy count (Section A decision point); scanner conditions and the earlier-signal objective (VP-44); secondary filtering (D); score and ranking (D, F); risk and SL semantics (E, F; Q-25); strategy versus account cap precedence (A-009, F); the circuit breaker (F-009, I); the same-symbol opposite-direction rule (Q-33, after the later agenda answers are checked); the Chartink alert mode and naming (VP-45, C); config and strategy precedence (A-009).
- Carried to: C-001, C-002, C-010, C-011 (scanners, naming, routing, end-of-day); D-001 to D-018 (filtration, extension guard, entry timing, ranking); E (stop distance, R:R); F (caps, risk, circuit breaker); A-009 (config split, with Rama's "review with Claude" note); A-010 (webhook secrets); end of Section A (strategy count, with VP-01 and VP-02).
