# A-006 BROKER MODEL: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-06 (revision 1, DR-021). ChatGPT's value-added review (06-Oct-2026) asked for no change to legacy facts; current broker facts are labelled CURRENT EXTERNAL BROKER FACT with their check date (its point 1). Rama's answers to Q-27 and Q-28 are V2 proposals and are recorded only in the V2 inputs file (its point 8). Erratum E-1 added 2026-10-06 during A-007 (section 5), reviewed by ChatGPT on 07-Oct and recorded in DR-022 (2026-10-07). Erratum E-2 added 2026-10-08 during A-010 (section 9), reviewed and accepted by ChatGPT on 09-Oct-2026 and recorded in DR-025 (2026-10-09) together with the A-010 freeze. The A-006 baseline freeze (DR-021) and erratum E-1 (DR-022) are unchanged. Erratum E-3 added 2026-10-09 during A-011 (section 10), reviewed and accepted by ChatGPT on 09-Oct-2026 and recorded in DR-026 (2026-10-09) together with the A-011 freeze.
Date: 2026-10-06
Layer: LEGACY ONLY. How the legacy system worked with the broker. Rama's new-system suggestions from the A-006 discussion are NOT here: they are kept in full in docs/agenda/a-006_broker-model/A-006_v2_design_inputs.md (PROPOSED) and summarised as VPs in docs/governance/V2_DESIGN_LOG.md.
Working copy of: docs/inputs/manuals/A-006_BROKER_MODEL_LEGACY_MANUAL_25-SEP-2026.md (original untouched; it mixed a short legacy Part I with new-system Parts II-XV). Changes are listed at the end.
Labels: FACT (SOURCE-VERIFIED) = read at the baseline commit on 2026-10-06; FACT (RECORD-DERIVED) = baseline document or earlier manuals, not re-read; HISTORICAL; UNKNOWN / NOT ESTABLISHED; INTERPRETATION / AUDIT LEAD; CURRENT EXTERNAL BROKER FACT (a current broker, exchange or regulator rule, with the date it was checked; never evidence of legacy behaviour); KNOWN DEFECT / AMBIGUITY.
Source priority: production source/config > production records/manuals > sandbox evidence > user recollection > interpretation.
Security: no credential values. Credential classes are named only.

## 1. Purpose and boundary
Document the legacy broker boundary: identity, the adapter, products and order types, delivery protection, margin and funds reads, rate limits, session/token handling, order monitoring and reconciliation.

## 2. Evidence
- Source-verified on 2026-10-06 (public production repository, baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb, raw files): config/system_config.yaml (broker, product_map, order_monitor, order_reconciler, eod_reconcile, drift_handler, capital sections), config/broker_limits.yaml, broker/zerodha_adapter.py (module design notes and the code sections cited), broker/token_monitor.py, scripts/auto_refresh_token.py (header).
- Record-derived: docs/inputs/legacy_baseline/CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt (sections 21-29); the 25-Sep ChatGPT manual and reconciliation; frozen A-003 to A-005.
- Not read here: orders/order_placer.py, order_manager.py, order_monitor.py, order_reconciler.py, broker/rate_limiter.py, scripts/zerodha_login.py, the cron registry. Claims about them stay record-derived.
- Current external broker facts checked on 2026-10-06: Zerodha support pages, Zerodha Z-Connect posts, Kite Connect developer docs and forum (URLs in A-006_v2_design_inputs.md section 18). They describe the broker now, not legacy behaviour.

## 3. Broker identity (FACT, SOURCE-VERIFIED config)
- broker.primary: zerodha; broker.fallback: angelone; fallback_enabled: false ("enable when AngelOne credentials available"); multi_account_mode: false.
- Actual Angel One use: NOT ESTABLISHED. HISTORICAL (Rama): Angel One was planned, then dropped.

## 4. The Zerodha adapter (FACT, SOURCE-VERIFIED broker/zerodha_adapter.py)
- Single point of contact with the kiteconnect SDK; no other module imports it (ZA1).
- Methods: place_order, cancel_order, modify_order, get_order_history, get_positions, get_margins, get_quote (ZA2), plus place_gtt, modify_gtt, get_gtt, get_gtts, delete_gtt, get_holdings, get_trades, get_open_orders, get_all_orders, get_live_margin_pct, get_server_time.
- Every broker call first validates its inputs, then takes a token from the client-side rate limiter (ZA3, ZA13).
- Product codes come from a product resolver (ZA4). Kite exceptions are translated to system exceptions; rejections carry symbol, side, qty, price, intent, broker code, reason and status code (ZA5, ZA6).
- Order state: place_order registers the order PENDING; success moves it to SUBMITTED, an exception to FAILED (ZA7). Cancel does not change state itself (ZA8). A broker rejection after acceptance arrives later through order status, so a rejected order can still carry a broker order id.
- The adapter never retries (ZA11). HTTP 429 from the broker is raised as a distinct rate-limit error and freezes that rate bucket with exponential delay (BL-6); OrderPlacer retries 429-only failures, at most 3 times (config rate_limit_backoff.max_placer_retries).
- HTTP timeouts: connect 5 s, read 10 s (broker_limits.yaml).
- Paper mode simulated every broker call inside the adapter, including synthetic fills (ZA10, ZA16a). Legacy only.

## 5. Products and order types (FACT, SOURCE-VERIFIED)
- product_map (zerodha): INTRADAY MIS, DELIVERY CNC, COVER_ORDER CO, BRACKET_ORDER "" (config comment: "Zerodha discontinued BO; empty = not supported").
- Order types accepted: MARKET, LIMIT, SL, SL-M; varieties: regular, co.
- SL legs were placed as SL (stop-limit), limit = trigger +/- 0.5% (capital.sl_limit_offset_pct); the config comment says Zerodha rejects SL-M via the API.
- Market protection: place_order accepts a market_protection percentage, validated by the system itself within 0.1% to 10% (added 10-Sep-2026, because the kiteconnect 5.1.0 SDK validates nothing).
- Tick snapping: every LIMIT/SL price and trigger is snapped to a valid tick from the instrument cache before reaching Zerodha, which rejects off-tick prices (FIX-181).
- Adapter guards: delivery_enabled (when false the adapter refuses any CNC order or OCO-GTT; production config: true); force_intraday_only (when true, non-intraday intents are coerced to MIS; production config: false).
- Historical use: CO and BO use UNKNOWN (A-004).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): the exchange bars intraday trading in the BE (trade-to-trade) series, so an MIS order on a BE stock is rejected (Zerodha support). The legacy instrument filter admitted the BE series (A-004). INTERPRETATION / AUDIT LEAD: whether intraday signals on BE symbols reached the broker and were rejected is not established (Q-30).
- ~~CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): from 3-Aug-2026 (NSE closing auction session) Zerodha's equity MIS auto square-off is 3:12 PM for CAS stocks (stocks in the F&O list) and 3:25 PM for other stocks, charged Rs 50 + 18% GST per order (Zerodha Z-Connect, 10-Jul-2026; Zerodha support). The legacy production MIS cut-off (15:09, A-003) came before both times.~~ (superseded by erratum E-1)
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06; erratum E-1): NSE's closing auction session (CAS) began on 3-Aug-2026 with a 3:10 PM MIS auto square-off for CAS stocks (stocks in the F&O list); Zerodha's product update of 10-Aug-2026 revised it to 3:12 PM. Current: 3:12 PM for CAS stocks and 3:25 PM for other stocks (Zerodha support), Rs 50 + 18% GST per order. The legacy MIS cut-off was 15:09 at the baseline (since 03-Sep-2026); before 28-Aug-2026 the only legacy closing pass ran at 15:17, after the CAS square-off (A-007_manual.md section 6).

## 6. Delivery (CNC) protection (FACT, SOURCE-VERIFIED)
- A CNC position was protected overnight by ONE two-leg OCO GTT: an SL leg and a target leg, both SELL LIMIT orders (place_gtt; SLICE2.5-P1). GTTs could be modified, read and deleted; holdings were read for the overnight GTT reconcile.
- The SL leg used a deep limit offset of 3% (capital.gtt_sl_limit_offset_pct) so an overnight gap-down could still fill within the floor.
- HISTORICAL (Rama): after the GTT, Zerodha handled target/SL on later days; DDPI was required for automatic sale of holdings (A-004).
- Evidence boundary: GTT use rests on the code and Rama's account; actual GTT placement records were not read. (The 25-Sep manual listed historical GTT use as UNKNOWN; CO use does stay UNKNOWN.)

## 7. Margin and funds reads (FACT, SOURCE-VERIFIED)
- get_margins: equity net, available cash and utilised debits.
- get_live_margin_pct: asks kite.order_margins() for the margin on 1 share at market price for the product; returns margin / price; cached 5 minutes per (symbol, intent); cache invalidated after a margin rejection (error 16388); uses the order rate bucket; in paper mode it raises, so the sizer uses static leverage (A-005).

## 8. Rate limits (FACT, SOURCE-VERIFIED config/broker_limits.yaml)
- Client-side token buckets: order 8/s (burst 8), quote 3/s (burst 3), historical 2/s (burst 2), margins 8/s (burst 8).
- On HTTP 429: backoff 1 s, 5 s, 30 s, then SOFT_KILL (4th step in code); bucket freeze 0.2 s doubling to a 5 s cap with +/- 0.05 s jitter.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Zerodha's Kite Connect FAQ: at most 10 orders per second per client account; excess requests are rejected with HTTP 429. Kite Connect docs (checked 2026-10-06): quote 1 request/second, historical candles 3/second, order placement 10/second, all other endpoints 10/second; 400 orders per minute; 5,000 orders per day; 25 modifications per order.
- INTERPRETATION / AUDIT LEAD: the legacy quote bucket (3/s; config comment "bumped from 1 to relax quote serialization under burst") is above the currently documented quote limit of 1 request/second. The limit in force when legacy ran, and whether quote calls drew 429s, are not established (Q-30).

## 9. Session and token (FACT, SOURCE-VERIFIED unless marked)
- scripts/auto_refresh_token.py: fully headless daily token refresh: scripted Zerodha web login with user id and password, TOTP generated in code, request token obtained from the Kite Connect login redirect and exchanged for an access token, saved as a token file. Its docstring says it ran at 08:00 IST Mon-Fri via cron, after a 05:00 token cleanup. Credentials came from .env (classes: user id, password, TOTP secret, API key, API secret~~; never committed~~) (superseded by erratum E-2: the .env file itself was never committed, but the API key and earlier API secret and TOTP values were committed elsewhere; A-010_manual.md section 11).
- broker/token_monitor.py: polls the broker profile every 30 minutes during market hours; ~~on expiry or failure logs CRITICAL, sends a Telegram alert and fires SOFT_KILL;~~ (superseded by erratum E-2) on a genuine token-invalid result (kiteconnect TokenException, or a message classified as token expiry) logs CRITICAL, sends one Telegram alert per episode and fires SOFT_KILL; network, server and rate-limit errors only log a warning (H-13); never hard-kills (open SL/target orders stay at the broker); never re-logs in.
- HISTORICAL (Rama): headless, plus manual token creation and upload with bash commands (the storage record lists scripts/copy_token_to_vm.bat, later retargeted to the testing VM).
- Conflict: A-003 records the token refresh at 08:15 (record-derived from the overview); the script docstring says 08:00; the cron registry was not read.
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): two Zerodha staff replies on the Kite Connect forum (27-Jul and 26-Aug; year not shown on the page) say exchange requirements need users to log in manually at least once each day, so Zerodha does not recommend automating the login flow. The Kite Connect access token expires at 6 AM the next day ("regulatory requirement") (Kite Connect docs).
- CURRENT EXTERNAL BROKER FACT (checked 2026-10-06): Zerodha's Kite Connect FAQ requires a registered static IP to place orders through the API (up to two IPs; one change per calendar week) and market protection on every market order (a value of 0 is rejected). The FAQ text says "from 1 April 2025"; the Kite forum note on the NSE circular records the effective date as extended to 1 April 2026; a Fyers notice also gives 1-Apr-2026 for the SEBI retail-algo framework, which further requires 2FA once every trading day.
- INTERPRETATION / AUDIT LEAD: the legacy scripted login and its compliance with the daily-login and static-IP requirements are not assessed here; for the audit (DR-016, Q-30).

## 10. Order monitoring, reconciliation and drift (FACT, SOURCE-VERIFIED config; behaviour RECORD-DERIVED)
- order_monitor: polls the broker every 2 s for fills; fill_timeout_sec 60 cancels an unfilled OPEN/SUBMITTED order after 60 s (audit lead Q-23 for the user-reported early entry cancellations).
- order_reconciler: 15 s cadence; capital drift tolerance Rs 50 out of session and max(Rs 50, 10% of expected) in session; Rs 5,000 extra allowance when human or untracked orders are present; repeat drift alerts at most every 30 minutes; trades stuck in EXITING for 30 minutes are resolved (flat to CLOSED_MANUAL, still held to OPEN).
- ~~drift_handler escalation: log at Rs 250, SOFT_KILL at Rs 1,000, HARD_KILL at Rs 2,500, after 3 consecutive cycles.~~ (superseded by erratum E-3)
- drift_handler escalation (erratum E-3; capital/drift_handler.py read in A-011): only drift events from the fund manager's own checks (its broker sync, the reconciler's ledger self-check, bucket overflow) escalate. A single event of Rs 1,000 or more trips SOFT_KILL and one of Rs 2,500 or more HARD_KILL; Rs 250 to 1,000 logs CRITICAL and becomes SOFT_KILL after 3 consecutive such events. The reconciler's own capital-drift check (G3) alerts but does not escalate. Code comments record that the drift kill rungs never fired in production (A-011_manual.md section 8.3).
- eod_reconcile (scripts/eod_broker_reconcile.py at 15:58): authoritative: false (shadow; runs alongside the older eod_verify and gates nothing); P&L tolerance Rs 100.
- RECORD-DERIVED (baseline sections 24-29): entry fill commits capital and places SL first, then target, sized to the actual filled quantity; if SL placement fails the target is not placed; a TGT retry manager re-places a missing target only while SL stands; software OCO cancels the sibling leg; the reconciler never flattens from DB state (broker positions first) and can trigger kill behaviour; HARD_KILL flattens with marketable LIMIT orders at LTP +/- 1% (capital.emergency_exit_buffer_pct).

## 11. Production versus testing VM
- Broker code was not compared file by file. The storage record notes the testing VM's cron registry account tag differed from production. No sandbox difference overwrites production.

## 12. Unknown / not established
Angel One use; CO and BO use; the exact submitted/acknowledged state semantics beyond ZA7; reconciler action set in code (not read); cron schedule of token refresh and cleanup; whether the live-margin call succeeded in practice; compliance of the scripted login and static-IP setup with the exchange framework; how often fill_timeout cancelled entries.

## 13. Known defects / ambiguities
- fill_timeout_sec 60 versus the intended 10-minute entry timeout (A-001/A-002 user-reported defect): audit lead Q-23, not yet a declared defect.
- Token refresh time conflict (08:00 docstring vs 08:15 overview).
- The adapter accepts SL-M, but production placed SL legs as stop-limit because Zerodha rejects SL-M via the API (config comment); consistent, recorded for clarity.
- eod_reconcile was still in shadow mode; the older eod_verify remained the authority.
- Quote rate bucket (3/s) above the currently documented quote limit (1/s): audit lead Q-30, not a declared defect.

## 14. Cross-references
Capital and margin: A-005. Instruments, tick sizes and the learned MIS blocklist: A-004. Hours and broker square-off: A-003. Products and GTT in detail: A-007. Secrets and environment: A-010 (Q-11). Order execution: section G. Position management and HARD_KILL: sections H and I. V2 inputs from this discussion: A-006_v2_design_inputs.md and V2_DESIGN_LOG VP-34 to VP-40.

## 15. Final A-006 statement
> The legacy system reached Zerodha only through broker/zerodha_adapter.py, which validated inputs, applied client-side rate limits, resolved products (MIS, CNC, CO; BO unsupported), snapped prices to valid ticks, bounded market protection, translated broker errors and never retried by itself. Intraday exits were built as stop-limit SL legs and targets with software OCO; the delivery (CNC) path protected positions overnight with a two-leg OCO GTT (use per Rama's account; placement records not read). Margins came from the order-margins API with a short cache, the access token from a scripted daily login monitored every 30 minutes, and broker truth was enforced by an order monitor, a 15-second reconciler with drift escalation and an EOD broker reconcile still in shadow mode. Angel One was configured but disabled.

## Changes from the original (25-Sep-2026)
Moved out (kept in full in A-006_v2_design_inputs.md): Parts II to XV (new-system broker selection, authority, local DB, token, auth failure, products/GTT, entry/exit, margin handoff, funds, charges, order/fill, rate limit/API/retry, reconciliation/position, eligibility, live-only, testing, error/audit, cross-topic list). Added from direct source reads: multi_account_mode; adapter design notes and method set; order state semantics; 429 handling and retry cap; HTTP timeouts; order types and varieties; stop-limit SL legs; market-protection bounds; tick snapping; delivery and intraday adapter guards; OCO-GTT structure and 3% SL offset; funds fields; live-margin lookup details; rate buckets; scripted token login and token monitor; reconciler, drift and EOD-reconcile settings. Added current external broker facts (exchange daily-login stance in two staff replies, static IP, 10 orders per second, market protection, documented API limits, BE-series intraday bar, MIS square-off times since 3-Aug-2026), three audit leads (quote bucket, BE series, scripted login; Q-30) and the token-time conflict.
After ChatGPT's value-added review (06-Oct-2026): no legacy fact changed; the label for current external facts renamed to CURRENT EXTERNAL BROKER FACT (checked 2026-10-06); the final statement worded as code paths, with use attributed to Rama's account (review point 8).

## Erratum (2026-10-06, found during A-007)
- E-1 (section 5, current external broker fact): the line saying Zerodha's CAS-stock MIS square-off was 3:12 PM "from 3-Aug-2026" is corrected. Zerodha's product update of 10-Aug-2026 says the square-off for CAS stocks was revised to 3:12 PM from 3:10 PM, so it was 3:10 PM when CAS began; the legacy code recorded both figures (orders/mis_autosquareoff.py). The 10-Jul-2026 Z-Connect post cited in the original line now shows 3:12 PM, apparently updated after publication. The note that the legacy 15:09 cut-off came before both times holds only from 03-Sep-2026; before 28-Aug-2026 the only closing pass was 15:17 (A-007_manual.md section 6). The original line stays visible, struck through. No legacy fact in this manual changes. Recorded in DR-022 (2026-10-07).

## Erratum (2026-10-08, found during A-010)
- E-2 (section 9, session and token): two corrections. (a) "never committed" in the token-script line came from the script's docstring ("Credentials (in .env, NEVER committed)"). It holds for the .env file (gitignored; the 02-Jul and 04-Aug records found it in no commit), not for the credentials: the production account's API key was committed in plain text in three docs/web_claude files (bcf03b5 and 37f6ba5; 18-Sep record) and hardcoded in three scripts, and commit 9f58848 put real API key, API secret and TOTP values for two accounts into the tracked .env.example (scrubbed by 7bc3367 on 02-Jul; 02-Jul records). (b) "on expiry or failure" came from the token monitor's module docstring. The H-13 code runs the CRITICAL, Telegram and SOFT_KILL sequence only for a genuine token-invalid result (TokenException, or a message that broker/auth_recovery.py classifies as token expiry); network, server and rate-limit errors log a warning and leave the latch untouched, and an IP refusal is not an expiry. The original text stays visible, struck through. No other legacy fact in this manual changes. Found during A-010 (A-010_manual.md sections 8 and 11); reviewed and accepted by ChatGPT on 09-Oct-2026 (A-010 review, point 2) and recorded in DR-025 (2026-10-09).

## Erratum (2026-10-09, found during A-011)
- E-3 (section 10, order monitoring, reconciliation and drift): the drift-handler line listed the configured thresholds with "after 3 consecutive cycles", which reads as if every escalation waited three cycles. In capital/drift_handler.py (read in A-011) the three-event rule applies only to drift between Rs 250 and Rs 1,000; Rs 1,000 and Rs 2,500 act on a single event; and only fund-manager sources escalate, while the reconciler's G3 capital-drift check sends alerts without a kill. The original line stays visible, struck through. No other legacy fact in this manual changes. Found during A-011 (A-011_manual.md section 8.3); reviewed and accepted by ChatGPT on 09-Oct-2026 (A-011 review, section 6) and recorded in DR-026 (2026-10-09).
