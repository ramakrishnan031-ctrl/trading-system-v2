# A-005 CAPITAL MODEL: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-06 (revision 1, after ChatGPT's final A-005 review of 06-Oct-2026, which agreed with every legacy correction). Recorded as DR-020. Rama's 06-Oct answers to Q-22 and Q-24 are V2 inputs and are recorded only in A-005_v2_design_inputs.md, not here.
Date: 2026-10-06
Layer: LEGACY ONLY. How the legacy capital model worked. The V2 capital flow confirmed in the A-005 discussion is NOT here: it is kept in full, unshortened, in docs/agenda/a-005_capital-model/A-005_v2_design_inputs.md (PROPOSED) and summarised as VPs in docs/governance/V2_DESIGN_LOG.md.
Working copy of: docs/inputs/manuals/A-005_CAPITAL_MODEL_LEGACY_MANUAL_24-SEP-2026_REVISED_STRICT.md (original untouched). Changes are listed at the end.
Labels: FACT (SOURCE-VERIFIED) = read directly from the production repository at the baseline commit on 2026-10-06; FACT (RECORD-DERIVED) = from the production baseline document or the 24-Sep manual, not re-read here; HISTORICAL; UNKNOWN / NOT ESTABLISHED; INTERPRETATION / AUDIT LEAD; EXTERNAL CONTEXT (broker facts, never legacy evidence); KNOWN DEFECT / AMBIGUITY.
Security: no credential values. The three files read contain no credential patterns.

## 1. Purpose
Document the legacy capital model: capital basis, buckets, margin/leverage, sizing, admission, reservation, commit, release and reconciliation. It does not repair or recommend the legacy implementation.

## 2. Evidence
- Source-verified on 2026-10-06 (public production repository, baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb, raw files): config/system_config.yaml, capital/position_sizer.py, capital/fund_manager.py.
- Comparison only: sandbox repository config/system_config.yaml (branch main), which is the testing VM's VM-local version (it carries the testing-only force_qty key).
- Record-derived: docs/inputs/legacy_baseline/CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt; the 24-Sep ChatGPT manual and reconciliation.
- Rama's A-005 discussion files, the capital-flow spreadsheet and broker screenshots are evidence of V2 intent or of broker state, not of legacy code.
- Not read here: main.py, risk_engine.py, the reconciler and the strategy YAMLs. Claims about them stay record-derived.
- Source priority for every load-bearing legacy claim: production source/config > production records/manuals > sandbox/testing evidence > user recollection > interpretation. Recollection gives context but never overwrites source-verified production behaviour.

## 3. Architecture overview (FACT, RECORD-DERIVED)
Chartink signal, SignalProcessor, screening, price construction, PositionSizer, RiskEngine, FundManager.reserve(), OrderPlacer, Zerodha adapter, broker order, fill (FundManager commit, trade OPEN, SL/target protection), exit, FundManager release with realised P&L. The critical boundary is sizing, then risk approval, then capital reservation, then order placement.

## 4. Capital basis and buckets (FACT, SOURCE-VERIFIED unless marked)
- FundManager is the single source of truth for capital state: total, available, reserved, used, daily realised P&L (FM1).
- Capital basis is broker cash: initialize(broker_balance) runs once at start-up (FM13); sync_from_broker(broker_balance) later sets total = broker cash + carried positions' margin ("carry"), so carried margin is not deducted twice, and recomputes bucket availability (FM9).
- Two independent buckets: intraday 0.70 and positional 0.30 of total; cross-bucket borrowing is forbidden (FM3; config capital.intraday_bucket_pct / positional_bucket_pct). Each bucket's base is its share of cash plus its own carry. conditional_allocation_enabled: false (fixed split).
- Required margin = qty x price / leverage, not notional (FM4).
- UNKNOWN: when and how often sync_from_broker ran during a session (its call sites were not read). This matters because the V2 input locks capital for the day (A-005_v2_design_inputs.md).
- RECORD-DERIVED (Master Redesign Agenda, 21-Sep): "Real cash capital = Rs 7,000; broker MIS margin facility = 5x; effective intraday/MIS buying-power basis = Rs 35,000", to be kept explicit and not silently reinterpreted.
- INTERPRETATION: in the code read, Rs 35,000 corresponds to buying power (cash x leverage); the code keeps one real-cash total with margin-based reservations, not a separate synthetic leveraged capital pool. The 23-Sep ChatGPT value-added review's description of legacy as "capital sizing based on a synthetic leveraged basis" is not supported by the code read.
- HISTORICAL (broker screenshots, section 15): the account showed opening balances of about Rs 10,384 (23-Sep) and Rs 10,348 (24-Sep); the account and its relation to the Rs 7,000 baseline are not established.

## 5. Leverage and margin (FACT, SOURCE-VERIFIED)
- leverage_map: INTRADAY 5.0, COVER_ORDER 6.0, DELIVERY 1.0 (pinned to exactly 1.0), BRACKET_ORDER 5.0. leverage_safety: min 1.0, max 10.0 (internal ceiling, not a SEBI or broker limit; code hard bound 20.0).
- Live margin: the sizer first asks the broker adapter for get_live_margin_pct(symbol, intent) and converts it to leverage = 1 / margin_pct; a margin_pct of 0 or less gives leverage 1.0 (no leverage).
- Fallback: on any API error, or in paper mode, the sizer uses the static leverage_map value (5.0 for INTRADAY).
- UNKNOWN: how often the live-margin branch actually succeeded in production (no logs survive).

## 6. Position sizing (FACT, SOURCE-VERIFIED: capital/position_sizer.py and config position_sizing)
- risk_rs = total_capital x effective risk pct (risk_per_trade_pct 0.01 intraday; delivery_risk_per_trade_pct 0.01).
- sl_distance = abs(entry - SL). Guard: reject if sl_distance < min_tick_size (0.05).
- qty_by_risk = floor(risk_rs / sl_distance). Guard: reject if qty_by_risk > max_single_order_qty (10,000).
- margin_per_share = effective_entry_price / leverage; qty_by_capital = floor(available_bucket / margin_per_share).
- qty_by_concentration = floor(total_capital x concentration pct / entry_price) (max_concentration_pct 0.10; delivery 0.10).
- raw_qty = min(qty_by_risk, qty_by_capital, qty_by_concentration).
- Then: tier multipliers (HIGH 1.0, MEDIUM 0.70, LOW 0.50) with performance weighting (dynamic_by_winrate true, multiplier 0.5 to 2.0), because position_sizing.enabled is true; the alternative flat-rupees-per-order mode (flat_value_rs) is off (commented out).
- Further guards: lot rounding with lot-skew rejection (25%, skipped when lot size is 1); min_qty_threshold 1; max_position_value_pct 0.40 of capital as a hard anomaly cap on qty x price.
- Delivery entries read only the delivery_* keys (required, no inheritance from intraday keys; a missing key stops start-up).
- Unverified claims: the 24-Sep manual said the production configuration states that maximum daily trades also served as the sizing divisor, and that risk_per_trade_pct was inert for sizing after a historical change. Neither statement appears in the production config at 970aabf or in the sandbox config (searched 2026-10-06), and the production sizer uses risk_per_trade_pct in qty_by_risk. Source not identified; not carried as FACT.

## 7. Capital admission
- FACT (RECORD-DERIVED): PositionSizer, then a protected portfolio lock, per-strategy position cap, the symbol/direction rule, RiskEngine.approve(), FundManager.reserve(), signal RESERVED, OrderPlacer. The lock prevents concurrent signal bursts from both passing stale capital checks; RiskEngine counts in-flight signals.
- FACT (SOURCE-VERIFIED, config risk): max_open_positions 5 (portfolio-wide; binds alongside each strategy's own max_concurrent_positions, default 2 per the config comment); max_daily_trades 10 ("hard cap on entries per day"); max_open_delivery_positions 3 and max_daily_delivery_trades 5 (delivery's own, tighter caps).
- FACT (SOURCE-VERIFIED): one_trade_per_symbol_direction_per_day: true. A second entry on the same symbol in the same direction is rejected for the rest of the day; the opposite direction is always allowed. The config comment says it was turned on 27-Jul-2026 and first live at the 3-Aug-2026 boot.
- Correction: the 24-Sep manual's legacy admission flow said "one trade per symbol once a day". That is the V2 rule (one trade per symbol per day, any direction; VP-12), not legacy behaviour.
- UNKNOWN: whether max_daily_trades counted entries placed or entries filled (V2 intends filled-only).

## 8. Reservation (FACT, RECORD-DERIVED; mechanism consistent with FM5, FM6, FM10)
Determine bucket; calculate required margin; add the SL-M margin buffer (capital.slm_margin_buffer_pct 0.05) where applicable; check availability; write the RESERVE ledger row; apply the reservation; check the invariant; return a 16-hex reservation id. Reserve, release and commit are atomic under one lock (FM5).

## 9. Ledger and invariant (FACT, SOURCE-VERIFIED)
- Invariant: available + reserved + used == total, checked inside every mutation (FM2), with a per-bucket guard.
- A violation raises CapitalInvariantViolation (CRITICAL) and calls the kill switch's hard_kill first, so open orders are cancelled (FM11, FM19).
- Write-ahead ledger: every mutation writes an fm_ledger row before the in-memory change; if the ledger write fails the mutation is skipped (FM10). On start-up, rehydrate_from_open_trades rebuilds state from fm_ledger, trades and orders, checking the invariant once at the end (FM18).
- reset_daily_pnl() runs at EOD (FM14).

## 10. Fill, commit and release (FACT)
- RESERVED to USED on entry fill, using actual fill quantity and price (RECORD-DERIVED; commit_to_used exists, SOURCE-VERIFIED).
- RESERVED to AVAILABLE when an order is rejected or cancelled before fill (RECORD-DERIVED).
- On exit, release_used credits margin plus net P&L (gross P&L minus round-trip costs) to the bucket's available capital and to total immediately, for profits and losses alike (SOURCE-VERIFIED; E4 contract).
- EXTERNAL CONTEXT: Zerodha states intraday profits are not available for trading until settlement (T+1); the original capital is available after exit.
- INTERPRETATION / AUDIT LEAD: on profitable days the legacy system's available capital could exceed the broker's, a possible system-vs-broker drift source. Not established as a defect; for the audit (DR-016).

## 11. Risk and other capital controls (FACT, SOURCE-VERIFIED config)
- daily_loss_limit_pct 0.03 is the sole daily-loss authority (the absolute rupee limit was deleted on 24-Jun); delivery_daily_loss_limit_pct 0.03 applies to the pre-trade gate only; the post-close breach in FundManager is account-wide (3% of current total).
- max_consecutive_losses 4 (shared). daily_loss_include_unrealized: false (shadow: logs only).
- Sector cap: max_sector_exposure_pct 0.40 with sector_cap_mode: observe (logs would-rejects only, does not reject).
- price_drift_threshold 0.005 (margin top-up if price drifts before placement).
- Exit pricing: sl_limit_offset_pct 0.005 (SL legs are stop-limit; the config comment says Zerodha rejects SL-M via the API); gtt_sl_limit_offset_pct 0.03 for CNC OCO-GTT SL legs; emergency_exit_buffer_pct 0.01.
- These are legacy controls, not approved for V2.

## 12. Broker reconciliation (FACT, RECORD-DERIVED)
The OrderReconciler compares local state with broker state, uses broker truth, detects capital drift, can repair state/capital, and can trigger protective behaviour. A drift handler escalates capital drift (config drift_handler section exists, SOURCE-VERIFIED; behaviour not read).

## 13. Sizing basis: Q-16 resolved for legacy
- Production code at 970aabf sizes by risk: the minimum of the risk, capital and concentration quantities, then tier and performance multipliers (section 6).
- Rama's A-002 recollection (capital per trade = capital / maximum trades per day, capped by a strategy limit) is not reflected in the production sizer. It matches the V2 flow discussed in A-005.
- The strict-scope note appended to the 24-Sep files states that legacy quantity was risk/SL based; this agrees with the code.

## 14. Production versus testing VM (comparison only)
The sandbox config (testing VM) differs on: force_qty 1 (testing VM only), max_position_value_pct 0.50 and its delivery twin (production 0.40), entry_start 09:30 (production 10:00). These do not overwrite production.

## 15. Broker screenshot evidence (HISTORICAL broker-state observations)
- 23-Sep-2026 about 20:00 IST: opening balance Rs 10,384.30; available Rs 10,351.12; used margin Rs 33.18; five instruments with quantity 0; total P&L -Rs 33.18.
- 24-Sep-2026 about 12:25 IST: opening balance Rs 10,347.80; available Rs 10,124.90; used margin Rs 222.90; RELTD EQ MIS quantity -3, average Rs 147.76, LTP Rs 148.02, P&L -Rs 0.78; open MIS orders and completed/cancelled CNC and MIS orders.
- UNKNOWN: which account, and whether these positions came from the production system, the testing VM or manual trading. The storage record (18-Sep) says the system was declared dead before the 18-Sep boot. These screenshots do not define V2 rules.

## 16. Paper versus live
Paper mode existed; in paper mode the sizer uses static leverage (section 5). Complete paper/live parity: UNKNOWN.

## 17. Unknown / not established
Dated history of every capital parameter; sync_from_broker call sites and frequency; whether max_daily_trades counted placed or filled entries; live-margin success rate in production; complete fm_ledger event taxonomy; paper/live parity; actual use of every configured product; account and system origin of the screenshot positions; the source of the unverified divisor and inert-risk claims.

## 18. Known defects / ambiguities
- Static leverage and live-margin branches coexist; the API-failure path falls back to 5x for intraday (section 5).
- Same-day profit credit versus broker T+1 availability (section 10): audit lead, not a declared defect.
- Configuration comments record changes over time; values are dated to the baseline commit, not timeless.
- The 24-Sep manual's "Capital available after Margin" terminology point concerns Rama's V2 spreadsheet, not legacy code; it moved to the V2 inputs file.

## 19. Cross-references
Instruments and lot sizes: A-004. Broker: A-006. Products and GTT: A-007. Strategy YAMLs (max_concurrent_positions): A-008. Configuration ownership: A-009. Risk and capital design: section F (F-001 to F-014). Order timeout and exits: sections G and H. V2 inputs from this discussion: A-005_v2_design_inputs.md and V2_DESIGN_LOG VP-12, VP-13, VP-29 to VP-33.

## 20. Final A-005 statement
> The legacy system implemented a capital and risk subsystem centred on PositionSizer and FundManager: one broker-cash capital basis split into separate intraday (70%) and positional (30%) buckets with no cross-bucket borrowing; margin computed as qty x price / leverage, using live broker margin when available and a static leverage map otherwise; risk-based sizing bounded by capital, concentration, tier/performance multipliers and anomaly caps; pre-order reservation; a write-ahead capital ledger with an always-checked invariant; fill-based commitment; release with same-day net P&L; and broker reconciliation. The V2 capital flow discussed in A-005 is recorded separately and is not evidence of legacy behaviour.

## Changes from the original (24-Sep-2026)
Freeze note (06-Oct): ChatGPT's final review agreed with all legacy corrections below; only the status line and the source-priority line were added at freeze.
Moved out (kept in full in A-005_v2_design_inputs.md): sections 14-20 and 25-26 (V2 capital flow, formulas, filled-trade limit, margin snapshot, entry-vs-exit capital, daily lock, YAML governance, cross-topic inputs, source-to-consumer table) and the appended "NEW SYSTEM CHANGES — STRICT SCOPE" block. Corrected: legacy admission rule (symbol + direction, not symbol once a day); capital basis described from code (broker cash, carry, buckets). Downgraded: the divisor and inert-risk claims (not found in source). Added from direct source reads: leverage safety bounds, live-margin conversion and fallback, full sizing guards and delivery keys, risk caps and delivery caps, daily-loss and sector-cap modes, exit offsets, invariant-to-hard-kill, write-ahead and rehydrate, same-day net P&L credit with the broker T+1 audit lead, testing-VM differences, Master Agenda Rs 7,000 / 5x / Rs 35,000 baseline, screenshot origin unknown, Q-16 resolution.
