# A-008 STRATEGY MODEL: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-07 (revision 1, DR-023). ChatGPT's value-added review (07-Oct-2026) accepted the source reconciliation and the legacy findings and changed no legacy fact; it asked for evidence-bounded wording on the late-signal mechanism, Rama's 98% SL report, current Chartink facts, the range-breakout lead, duplicate and subset terms, and "no effect" claims (all applied). DR-023 also records erratum E-1 to the A-007 manual. Erratum E-1 to this manual added 2026-10-08 during A-009 (sections 4 and 8: scanner-map validation and duplicate scanner names), accepted by ChatGPT on 08-Oct-2026 (A-009 review, accepted point 9) and recorded in DR-024 (2026-10-08). Erratum E-2 (redaction, section 9) added 2026-10-08 during A-010, accepted by ChatGPT on 09-Oct-2026 (A-010 review, point 3) and recorded in DR-025 (2026-10-09); a documentation redaction only. Erratum E-3 to this manual (section 5: the side of an orphan broker position; no reconciler path adopts one) added 2026-10-16 during B-005, revised 2026-10-19 in B-005's revision 1 after ChatGPT's value-added review of B-005, accepted in ChatGPT's acceptance review of revision 1 and approved by Rama on 19-Oct-2026, and recorded in DR-041 (2026-10-19) together with the B-005 freeze.
Date: 2026-10-07
Layer: LEGACY ONLY. What a strategy was in the legacy system, which strategies existed, how a Chartink alert became a strategy candidate, which settings sat in each strategy YAML and which stayed global, and what went wrong. Rama's new-system suggestions from the A-008 discussion, including his 07-Oct note on the Chartink scanners, are NOT here: they are kept in full in docs/agenda/a-008_strategy-model/A-008_v2_design_inputs.md (PROPOSED) and summarised in docs/governance/V2_DESIGN_LOG.md.
Working copy of: docs/inputs/manuals/A-008_REVISED_STRATEGY_MODEL_LEGACY_MANUAL_29-SEP-2026_R1.md (original untouched; its section 28 held V2 inputs). Changes are listed at the end.
Labels: FACT (SOURCE-VERIFIED) = read at the baseline commit on 2026-10-06 or 2026-10-07; FACT (RECORD-DERIVED) = a dated record in the baseline repository (audit report, locked-decision record, or a code comment recording a measurement or event) that cannot be re-run, because the legacy database is gone (A-001); FACT (SUPPLIED RECORD) = Chartink_scanners.v2.txt, Rama's copy of the 16 scanner definitions (07-Oct-2026); HISTORICAL (Rama); UNKNOWN / NOT ESTABLISHED; INTERPRETATION / AUDIT LEAD; CURRENT EXTERNAL FACT (a current third-party rule with the date it was checked; never evidence of legacy behaviour; V2_DESIGN_LOG W-5); KNOWN DEFECT / AMBIGUITY.
Source priority: production source/config > production records > sandbox evidence > user recollection > interpretation.
Security: no credential values. Credential-bearing paths were not fetched or opened. The two Chartink screenshots contain webhook material; nothing from them is reproduced except the alert names.

## 1. Purpose and boundary
Document the legacy strategy model: what defined a strategy, the deployed inventory, the path from a Chartink alert to a strategy candidate, the per-strategy settings and what consumed them, the global controls above them, and the recorded weaknesses. Screening internals (D-003, D-017), price construction (E), capital and sizing (A-005, F), products (A-007) and order execution (G) are referenced, not re-documented.

## 2. Evidence
- Source-verified at the baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (public production repository; raw files plus a blob-less sparse clone that fetched only the paths listed; docs/web_claude, .env files and other credential-bearing paths were not fetched or opened): config/strategies/*.yaml (all 16, every key); strategies/schema.py, loader.py, control.py; core/strategy_direction.py; config/strategy_direction_registry.yaml (tracked seed); config/scan_webhook_map.yaml; config/chartink_scanners.yaml; signals/webhook_receiver.py (authentication order, WR4, WR4b, EOD routing); signals/signal_processor.py (strategy lookup, control gate, windows, governor hook, per-strategy cap, symbol-direction rule, price derivation, momentum re-anchor, gate continuation); screening/secondary_screener.py; screening/quality_scorer.py; screening/entry_gate.py; config/scoring_weights.yaml; capital/strategy_governor.py; capital/risk_engine.py (duplicate and contrary checks); core/config_loader.py (product-gate and scoring schema); main.py (gate-release and governor wiring); config/system_config.yaml (trading_hours, product gates, position_sizing, risk, watchlist, portfolio_allocator, v3_chain, structure_exit, strategy_circuit_breaker).
- Record-derived, in the baseline repository: docs/locked_decisions.yaml (S6, S12 to S15); docs/audit/bk1_long_short_scanner_analysis_17jul2026.md ("BK-1", 17-Jul-2026, a read-only analysis of the live database); docs/audit/strategy_direction_investigation_17jul2026.md (17-Jul-2026); dated code comments, named where used.
- Comparison: the testing snapshot config (sbx_system_config.yaml, branch main, fetched 2026-10-06). Testing strategy YAMLs and the testing scanner map were not compared.
- Supplied record: Chartink_scanners.v2.txt (Rama, 07-Oct-2026), the conditions of the 16 Chartink scanners as he recorded them.
- User evidence: Rama's answers in the 27-Sep A-008 checklist; two Chartink screenshots (production and testing alert names).
- Not read: screening/step_executor.py (which screening steps could score), the Chartink alert settings and plan, the runtime direction-registry state (data_store/, not in the repository), and the legacy git history (LEGACY_REFERENCE rule).

## 3. Strategy inventory and count
- FACT (SOURCE-VERIFIED): config/strategies holds 16 YAMLs at the baseline; each file name equals its `name` key.
- The 15-versus-16 question is resolved:
  - FACT (RECORD-DERIVED): locked decision S12, "The 15 strategy YAML file names (locked)", lists the original 15 (6 intraday long, 6 intraday short, 3 positional; rationale "Names locked to prevent scan_webhook_map drift"). S14 and S15 also say "All 15 scanners".
  - FACT (SOURCE-VERIFIED): the 16th YAML, pb01_breakout_retest, is a later V3 playbook addition: enabled false, v3_playbook true, horizon NEXT_DAY; its description reads "WATCHLIST-bound, SHADOW / non-trading (enabled:false); would-be records only" (section 16).
  - The scanner map, the direction-registry seed (17-Jul-2026) and Chartink_scanners.v2.txt all list the same 16.
- FACT (SOURCE-VERIFIED): split at the baseline: 13 intraday (12 enabled, pb01 disabled) and 3 delivery (all enabled, all LONG); 10 LONG and 6 SHORT. Rama's "10 intraday + 5 delivery + 1 placeholder" (Q1, Q3) is HISTORICAL recollection; the source supersedes it for the baseline.

| # | Strategy (YAML name) | Chartink definition | Direction | Intent | Enabled | Closed trades 15-Jun to 16-Jul (BK-1) |
|---|---|---|---|---|---|---|
| 1 | open_low_breakout_long | XV OPEN LOW BREAKOUT LONG | LONG | INTRADAY | true | 28 |
| 2 | first_pullback_long | XIII FIRST PULLBACK LONG | LONG | INTRADAY | true | 24 |
| 3 | vwap_bounce_long | XI VWAP BOUNCE LONG | LONG | INTRADAY | true | 35 |
| 4 | gap_go_long | IX GAP GO LONG | LONG | INTRADAY | true | 13 |
| 5 | gap_fade_long | VII GAP FADE LONG | LONG | INTRADAY | true | 8 |
| 6 | range_breakout_long | V RANGE BREAKOUT LONG | LONG | INTRADAY | true | 0 |
| 7 | open_high_breakdown_short | XIV OPEN HIGH BREAKDOWN SHORT | SHORT | INTRADAY | true | 0 |
| 8 | first_pullback_short | XII FIRST PULLBACK SHORT | SHORT | INTRADAY | true | 5 |
| 9 | vwap_rejection_short | X VWAP REJECTION SHORT | SHORT | INTRADAY | true | 3 |
| 10 | gap_go_short | VIII GAP GO SHORT | SHORT | INTRADAY | true | 0 |
| 11 | gap_fade_short | VI GAP FADE SHORT | SHORT | INTRADAY | true | 6 |
| 12 | range_breakout_short | IV RANGE BREAKOUT SHORT | SHORT | INTRADAY | true | 0 |
| 13 | positional_momentum_long | III POSITIONAL MOMENTUM LONG | LONG | DELIVERY | true | 4 |
| 14 | positional_sector_rotation | II POSITIONAL SECTOR ROTATION | LONG | DELIVERY | true | 19 |
| 15 | positional_swing_long | I POSITIONAL SWING LONG | LONG | DELIVERY | true | 10 |
| 16 | pb01_breakout_retest | XVI PB01 BREAKOUT + RETEST (SHADOW) | LONG | INTRADAY | false | 0 |

- FACT (RECORD-DERIVED, BK-1 and the 17-Jul direction investigation): 11 of the 16 had closed trades by 16-Jul-2026; none for range_breakout_long, range_breakout_short, open_high_breakdown_short, gap_go_short and pb01. The trade counts are from BK-1's per-scanner table (155 trades); later activity is not recorded.
- UNKNOWN: which strategies were enabled on each historical date (Rama Q2: "No idea"). HISTORICAL (Rama, Q5): strategies were only added; none was renamed, disabled or materially changed.

## 4. What a strategy was: schema and loader (FACT, SOURCE-VERIFIED)
- StrategyConfig (strategies/schema.py) is a Pydantic model with extra="forbid", so an unknown key fails validation. Field groups:
  - Identity and model: name, display_name, description, direction (required; LONG or SHORT), intent (INTRADAY or DELIVERY), order_protocol (CO_PLUS_TGT or LIMIT_TRIPLE), pipeline and horizon (metadata), v3_playbook (default false), enabled (default true).
  - Entry: entry_method, entry_offset_pct.
  - Stop: sl_method (FIXED_PCT or ATR), sl_pct, sl_atr_multiplier, sl_min_pct and sl_max_pct (defaults 0.003 and 0.05), sl_gap_buffer_pct.
  - Target: tgt_method (FIXED_PCT, RISK_REWARD or ATR), tgt_pct, tgt_risk_reward (default 2.0), tgt_atr_multiplier; smart_tgt_enabled with its trail trigger and step; trailing_sl_enabled (default false; no YAML sets it).
  - Pullback: pullback_wait_enabled, pullback_wait_timeout_sec (default 180), pullback_wait_tolerance_pct (default 0.005).
  - Screening thresholds: min_score (0 means "use the global pass score"), min_volume_surge (default 1.3), min_adr_pct, max_spread_pct.
  - Position: lot_size, max_concurrent_positions (default 2; "FIX-135 Item 42: per-strategy position cap").
  - Time: entry_start_time (default "10:00"), entry_end_time (default "15:00"), active_days.
  - Removed: per-strategy max_risk_pct ("BUILD 1 (#11, 24-Jun): ... DELETED. It was dead in the live path — the sizer always uses the global position_sizing.risk_per_trade_pct").
- Loader (strategies/loader.py): loads every *.yaml in sorted order, once at start-up (no hot reload); "ANY invalid file raises ConfigSchemaError immediately — no partial loads"; an empty folder is also an error; strategies are keyed by name; ~~the scanner map is cross-validated (S10: every mapped strategy must exist).~~ (superseded by erratum E-1) the loader can cross-validate the scanner map (S10: every mapped strategy must exist), but only when it is given the map path, and none of the boot or pre-flight callers read passes it (erratum E-1). A separate describe-only pass, scan_strategy_errors (18-Jul-2026), lists every bad file so one consolidated alert can name them all; it never changes loading.
- The YAML is the only source of a strategy's direction and intent; no per-signal override exists (A-007 section 4).

## 5. Identity and direction (FACT, SOURCE-VERIFIED unless marked)
- Direction is a required, enum-validated field; all 16 YAMLs declare it (10 LONG, 6 SHORT). Rama's answer that direction "is not labelled/mentioned in .yaml files" (Q4) is superseded by the source; his point that each strategy sent one side only agrees with it.
- The Chartink webhook carries no side: the payload is stocks, trigger_prices, triggered_at and an optional scan_name. The signal processor computes the order side from StrategyConfig.direction (BUY for LONG, SELL for SHORT); it is persisted in orders.transaction_type and trades.direction, not on the signal.
- core/strategy_direction.py is the canonical accessor; it reads direction from the validated YAML field, never from the _long/_short suffix. The registration and health registry (tracked seed config/strategy_direction_registry.yaml, 17-Jul-2026; runtime state in data_store/, gitignored) mirrors the field with registration_status (PENDING or CONFIRMED) and health (OK or DIRECTION_CONFLICT); "nothing reads direction FROM the registry to place a trade". A daily officer script owns its writes and notifies a NEW strategy or a DIRECTION_CONFLICT by Telegram and email. Runtime state: UNKNOWN.
- FACT (RECORD-DERIVED, 17-Jul-2026 investigation): for the 11 strategies that had traded, realised trade direction matched the declared direction and no strategy had traded both sides. ~~The only side not derived from the YAML is an orphan broker position adopted by the reconciler.~~ (superseded by erratum E-3) The record names an orphan broker position "adopted by `order_reconciler.py:21`" as the one side not derived from the YAML; line 21 is the reconciler's header docstring (the check name ORPHAN_ADOPTION), and no reconciler path creates a trade from a broker position: CHECK2 logs a position with no local trade as a human order and leaves it alone (FIX-182), and the in-flight adoption keeps an existing local trade row and was not wired in any version read (B-005_manual.md sections 5 and 7). So no trade side independent of the YAML was identified in the inspected path. A name-suffix parse existed only in a GUI grouping helper (display).
- Identity at the baseline: YAML file name = name key = scanner-map key = webhook URL path, for all 16. The Chartink alert name is free text that must normalise to that path (section 9).

## 6. Intent and the strategy controls (FACT, SOURCE-VERIFIED unless marked)
- One intent per strategy (INTRADAY or DELIVERY); MIS or CNC as per A-007.
- strategies/control.py strategy_will_trade ("Pure: no I/O, no mode branch — paper and live evaluate identically") checks, in order:
  1. enabled false: WON'T TRADE (disabled switch).
  2. force_intraday_only true and intent DELIVERY: WON'T TRADE (delivery strategy dormant).
  3. trade_type INTRADAY with a non-INTRADAY intent, or trade_type DELIVERY with a non-DELIVERY intent: WON'T TRADE.
  4. Otherwise WILL TRADE.
- Rejection labels: a trade_type mismatch is REJECTED_TRADE_TYPE; a disabled switch or the force-breaker branch is REJECTED_STRATEGY_CONTROL. The same resolver drives the strategy status table.
- force_intraday_only since 10-Jul-2026 (loader "Option A"): the declared intent is preserved; when the flag is true, a DELIVERY strategy is dormant at the resolver and the adapter coerces the product to MIS at the broker chokepoint; delivery_enabled false is a separate CNC/GTT backstop. Before 10-Jul-2026 the loader rewrote DELIVERY to INTRADAY when the flag was true ("the old Bug-D destructive rewrite"). The config and schema comments above the flag still describe the old rewrite (stale; this corrects the frozen A-007 manual by erratum E-1).
- Production values at the baseline: trade_type BOTH, force_intraday_only false, delivery_enabled true (A-007 section 4). The flag was added after the 15-Jun-2026 unintended-CNC incident and was false by 22-Aug-2026 (A-007); the date it was switched off: UNKNOWN.
- INTERPRETATION / AUDIT LEAD: BK-1's 33 "POSITIONAL" long trades (15-Jun to 16-Jul) came from the three delivery strategies. While force_intraday_only was true before 10-Jul-2026, the loader rewrote their intent to INTRADAY, so they probably traded as MIS; whether the flag was true throughout that window is not established (Q-34).
- HISTORICAL (Rama, Q6 to Q8): config trade_type was the master control and each strategy YAML a sub-controller, and each strategy could be enabled or disabled on its own. Consistent with source.

## 7. Entry windows (FACT, SOURCE-VERIFIED)
- Two layers: the global window (trading_hours.entry_start and entry_end) and the per-strategy window (entry_start_time and entry_end_time; "CFG-5"). Both must allow entry; a signal outside the strategy window is REJECTED_OUTSIDE_ENTRY_WINDOW.
- Values at the baseline: global entry_start 10:00 in production and 09:30 in testing; entry_end 15:00 in both (aligned on 29-Jun-2026, "T5"; it was 15:15); eod_entry_cutoff 15:15 (absolute order-placement deadline). Per-strategy: 09:25 to 15:00 in 15 YAMLs; pb01 09:25 to 11:00 (disabled).
- Effect: the per-strategy start never bound, because the global start (10:00; testing 09:30) was later; the config itself calls the mismatch "cosmetic" (04-Jul audit). The per-strategy end equals the global end, so it had no separate effect after 29-Jun-2026; before that date (global end 15:15) it was the binding end.
- Comments that disagree with the deployed values: the signal processor's CFG-5 comment says "gap_fade_long cuts off at 11:30" (YAML: 15:00); locked S6 sets positional entry_end_time 14:30 (YAMLs: 15:00); the testing config's comment still says "while this says 10:00" above its 09:30 value.
- HISTORICAL (Rama, Q9): whether per-strategy windows controlled entry was UNKNOWN to him; at the baseline they did not.
- active_days is MON to FRI in all 16 (trading calendar: A-003).

## 8. Scanner-to-strategy mapping and webhook routing (FACT, SOURCE-VERIFIED)
- A scanner needs three config entries (17-Jul investigation): the strategy YAML, a scan_webhook_map entry and a chartink_scanners entry.
- config/scan_webhook_map.yaml (S14): 16 entries of the form `<scanner_name>: {strategy, chartink_url}`; one scanner maps to one strategy; every key equals its strategy name. ~~"Duplicate scanner names -> CRITICAL startup failure"; S10 cross-validation at load.~~ (superseded by erratum E-1) The file's header says "Validated at startup (P17, S10)" and "Duplicate scanner names -> CRITICAL startup failure"; neither was enforced on the boot path: S10 runs only when the loader is given the map path, which no boot or pre-flight caller read passes, the map's schema has no duplicate or existence check, and the pinned YAML parser kept the last of two identical keys without error, although YAML 1.2.2 requires unique keys (erratum E-1). The routing key "MUST equal the /webhook/<scanner_name> URL path" entered in Chartink.
- Webhook receiver, POST /webhook/<scanner_name>, in this order after the shutdown and per-IP rate checks: authentication (HMAC or a shared token; no values recorded here); WR4: a scanner name not in the map gets HTTP 404 "Unknown scanner", so an unmapped alert is refused before any processing; an "eod" scanner is routed to the watchlist (below); kill switch active gives 403; then queue backpressure. If the body has scan_name, it is normalised (lower case, spaces to underscores, account prefix stripped; section 9) and must equal the path, or the request gets 400.
- Signal processor (defence in depth): no map entry, no strategy key, or strategy not loaded gives REJECTED_UNKNOWN_STRATEGY.
- scanner_type eod (pb01 only): the alert is captured to the watchlist only; it skips the intraday entry-window gate and is never queued for the order path (section 16).
- config/chartink_scanners.yaml (S15, P17): 16 slug-style Chartink URLs for a start-up HTTP HEAD reachability check (CRITICAL if unreachable). The file's own note says "URLs are placeholders". Whether they matched the real screeners: UNKNOWN; the check proves reachability, not the scanner conditions.
- HISTORICAL (Rama, Q11 to Q14): each scanner fed one strategy; strategies did not cross-feed; no unmapped-scanner or mapping-collision incident is known. The source answers Q13 (404) and Q14 ~~(a collision fails start-up)~~ (superseded by erratum E-1: a duplicated scanner key is not rejected; the pinned parser keeps its last occurrence, and no start-up check detects it).

## 9. Chartink alert names: production versus testing
- HISTORICAL (Rama, Q5; screenshots): production alert names had no client ID (for example "VWAP BOUNCE LONG"); testing alert names carried the testing account's client ID (for example "[testing client ID]-VWAP BOUNCE LONG"; client ID replaced by a placeholder under erratum E-2), so the payload carried it too. Rama recalls a manual adjustment in the testing system so the marker was ignored.
- FACT (SOURCE-VERIFIED, webhook_receiver.py WR4b, 09-Sep-2026): the adjustment is in the shared code, not testing-only. _strip_account_prefix removes a leading "<primary account tag>-" or "<tag>_" from the normalised scan_name before WR4 compares it with the URL path. The tag comes from accounts.csv (AR12), not hardcoded; only a prefix is stripped, so an alert pointed at the wrong scanner is still rejected. Code comment: "Chartink alert names are per-account on a shared Chartink login ... The account name cannot be dropped in the Chartink UI, so the receiver absorbs it." Production's tag never appears in its alert names, so the strip does nothing there.
- KNOWN DEFECT (RECORD-DERIVED, same comment; fixed 09-Sep-2026): before WR4b the mismatch returned 400, "which stopped every signal on that machine" (the testing VM). When that outage began: UNKNOWN.

## 10. Per-strategy parameters at the baseline (FACT, SOURCE-VERIFIED)

| Strategy | sl_pct | Gap buffer | R:R | Pullback wait | min_volume_surge | Max concurrent |
|---|---|---|---|---|---|---|
| first_pullback_long / _short | 1.5% | - | 1.5 | on | 1.3 | 2 |
| gap_fade_long / _short | 1.0% | 0.3 | 1.5 | off | 1.3 | 3 |
| gap_go_long / _short | 1.2% | 0.3 | 1.5 | off | 2.0 | 3 |
| open_high_breakdown_short | 1.0% | - | 1.5 | on | 1.3 | 2 |
| open_low_breakout_long | 1.0% | - | 1.5 | on | 1.3 | 2 |
| range_breakout_long / _short | 1.0% | - | 1.5 | off | 1.8 | 2 |
| vwap_bounce_long / vwap_rejection_short | 0.8% | - | 1.5 | on | 1.5 | 2 |
| pb01_breakout_retest (disabled) | 1.0% | - | 1.5 | off | 1.3 | 2 |
| positional_momentum_long | 2.0% | - | 1.5 | off | 1.5 | 2 |
| positional_sector_rotation | 2.0% | - | 1.5 | off | 1.3 | 2 |
| positional_swing_long | 2.0% | - | 1.5 | off | 1.2 | 2 |

- Common to all 16: entry_method LIMIT; sl_method FIXED_PCT; tgt_method RISK_REWARD; min_score 0; min_adr_pct 0.005; max_spread_pct 0.005; lot_size 1; active_days MON to FRI; pullback timeout 180 s and tolerance 0.5%. Intraday YAMLs: entry_offset_pct 0.001, SL bounds 0.3% to 5%, smart_tgt_enabled true (trail trigger 0.5%, step 0.3%), ATR multipliers 1.5 and 2.5. Delivery YAMLs: entry_offset_pct 0.002, SL bounds 0.5% to 8%, smart_tgt_enabled false, ATR multipliers 2.0 and 3.0.
- How the values were used (signal_processor._derive_prices and downstream):
  - Entry (LIMIT): LONG entry = trigger x (1 - entry_offset_pct); SHORT entry = trigger x (1 + entry_offset_pct).
  - Stop (FIXED_PCT): LONG SL = entry x (1 - sl_pct); SHORT SL = entry x (1 + sl_pct); then clamped to sl_min_pct and sl_max_pct. sl_method ATR was "not implemented" (falls back to FIXED_PCT with a warning, or rejects in HALT mode); no YAML used it, and no effect of the ATR multipliers was identified in the inspected path.
  - Gap buffer (FIX-130 Item 4): widens the SL by sl_gap_buffer_pct / 100 (0.3 means 0.3%) only from 09:15 to 09:30 inclusive. With the global start at 10:00 no production signal could reach that window, and in testing (09:30) only one at exactly 09:30:00; no effect identified at the baseline start times.
  - Target: R:R 1.5 on the actual entry and SL (target derivation and fill recalculation: E and G). Entry-gate guards such as min_effective_rr 1.0 and max_entry_slippage_pct 1% are global (VP-09 evidence).
  - Pullback wait on (first_pullback pair, open_high/open_low, vwap pair): after screening, the EntryGate holds the signal and polls the LTP; it continues to sizing and order only on PRICE_HIT (LTP within the entry price +/- 0.5%); a 180-second timeout or three quote failures end it with no order ("no action", main.py MAIN18).
  - Pullback wait off (gap, range, delivery; "momentum"): the processor fetches a fresh LTP and re-anchors entry, SL, target, size and reserved capital on it (FIX-067 / M-S1; the earlier code had silently fallen back to the stale trigger price "in BOTH modes").
  - smart_tgt_enabled: SmartTgtManager is registered only for CO_PLUS_TGT trades and every trade ran LIMIT_TRIPLE, so no effect of the 13 intraday "true" values was identified in the inspected production path (A-007 section 5; audit lead Q-31(c)).
  - trailing_sl_enabled: schema default false and no YAML sets it; in the inspected registration path the BreakevenManager is registered only when it is true (A-007 section 5), so no registration was identified (this answers the "schema default not read" note there).
  - order_protocol, pipeline, horizon: no production consumer identified in the inspected path (A-007 section 5: the order placer always used its own default protocol).
  - Screening thresholds and min_score: section 12. max_concurrent_positions: section 13.
- FACT (RECORD-DERIVED): the locked records differ from the deployed YAMLs. S13 sets intraday R:R 2.0 (gap_go 2.5, gap_fade 1.5) and S6 sets positional ATR SL 2.0x, ATR target 3.0x and entry_end 14:30; the YAMLs use R:R 1.5 everywhere, a fixed 2% positional SL and 15:00; gap_go's 1.2% SL is not in S13. The YAMLs are what ran; the locked file was not kept current.
- HISTORICAL (Rama, Q19 to Q21): SL % and target R:R were strategy-specific and came from each strategy YAML. Source: both sit in every YAML (sl_pct as the stop distance); R:R is 1.5 in all 16, so it was per-strategy in structure but uniform in value.

## 11. What stayed global (FACT, SOURCE-VERIFIED)
- Risk budget: position_sizing.risk_per_trade_pct 0.01 (1% of total capital per trade) and delivery_risk_per_trade_pct 0.01. No per-strategy risk % existed after 24-Jun-2026. A strategy's sl_pct set the stop distance; the global risk % set the rupee risk (sizing: A-005).
- Portfolio and day limits: max_open_positions 5 (binds alongside each strategy's cap), max_daily_trades 10, delivery caps (max_open_delivery_positions 3, max_daily_delivery_trades 5), daily loss, consecutive losses, sector exposure in observe mode (A-005 section 7).
- Also global: the screening pass score, tier thresholds and tier size multipliers (section 12); the same-symbol rules (section 14); trading hours (section 7); product gates (section 6); the strategy circuit-breaker settings (section 15).
- HISTORICAL (Rama, Q23 and Q24): major controls (maximum, minimum, time) were global in config; each strategy YAML held its R:R, classification and similar. Correction from source: each YAML also held a position cap (section 13) and three screening thresholds (section 12).

## 12. Common secondary screening, score and tier (FACT, SOURCE-VERIFIED unless marked)
- After the strategy gate, the strategy window and the circuit breaker, every signal went through one common SecondaryScreener: a 10-step pipeline scored by QualityScorer with weights from config/scoring_weights.yaml: volume_surge 15, vwap_position 10, atr_filter 10, rsi_range 10, price_action 15, sector_strength 10, time_of_day 5, spread_check 5, circuit_check 10, signal_age 10 (total 100). A missing step is left out of the denominator (FIX-042).
- Pass score 60 (min_pass_score); a lower score is REJECTED_SCORE_<n>. The per-strategy override min_score was 0 in all 16, so every strategy used 60. Tiers: HIGH at 80 or more, MEDIUM at 65 or more, otherwise LOW; size multipliers HIGH 1.0, MEDIUM 0.70, LOW 0.50 (position_sizing.tier_multipliers).
- Per-strategy inputs to the common pipeline: min_volume_surge (1.2 to 2.0), min_adr_pct and max_spread_pct. These three thresholds were the only strategy-specific part of screening; the steps and weights were common. HISTORICAL (Rama, Q15): one score/rank-based universal filtration for all strategies. Consistent.
- FACT (RECORD-DERIVED, quality_scorer.py comment, "frozen contract A2.3"): "25/100 pts dead-at-0 + 20 pinned-at-half make >65 algebraically impossible today", so HIGH (80) was unreachable (IA-P2-03). A passing signal therefore scored 60 to 65: LOW (half size) at 60 to 64 or MEDIUM (70% size) at 65. Which steps were dead or pinned was not read (audit lead, Q-34).
- HISTORICAL (Rama, Q25): the score restricted candidates and the tier multiplier restricted size. Consistent; with HIGH unreachable, every trade that passed was sized at 50% or 70%.
- Shadow layers that, per their config and code comments, did not change live decisions at the baseline: the V3 hard-gate re-scale (scoring v3_hardgate_mode "shadow"; "LIVE follows OLD"), the portfolio allocator (allocator_mode "shadow"; enforce_scope "v3_only"), the V3 decision chain (v3_chain_mode "shadow", "LOG-ONLY") and structure_exit (disabled). Details belong to J.
- The MIS learned blocklist in the screener was in shadow mode (A-004 section 6).

## 13. Per-strategy position cap (FACT, SOURCE-VERIFIED)
- max_concurrent_positions is 2 in 12 YAMLs and 3 in the four gap strategies. The signal processor enforces it inside the portfolio lock as STRATEGY_POSITION_LIMIT ("H-7"): effective = max(open + partial + live reservations, open + partial + pending) + 1 for this candidate; reject when effective exceeds the cap. It binds alongside the portfolio-wide max_open_positions 5.
- HISTORICAL (Rama, Q27): "Legacy/Old system don't have this settings". Superseded by source: every YAML had the cap and it was enforced.

## 14. Same-symbol rules (FACT, SOURCE-VERIFIED unless marked)
- DUPLICATE_SYMBOL (risk engine): reject while an active position or in-flight order exists for the symbol, whatever the strategy, book or direction. This is account-wide while the position is open: "a delivery holding still blocks intraday WHILE IT IS OPEN".
- CONTRARY_POSITION (FIX-019): reject an entry opposite to an active position (wash-trade prevention).
- SYMBOL_DIRECTION_DAILY_LIMIT (risk.one_trade_per_symbol_direction_per_day: true; enforced in the processor next to the per-strategy cap): at most one executed trade per symbol and direction per day; the opposite direction is always allowed ("a reversal, not a re-entry"); broker-rejected or failed entries do not count.
  - FACT (RECORD-DERIVED, config and code comments): turned on by Rama on 27-Jul-2026 after SENCO re-entered 80 seconds after a target exit and stopped out (the scanner had fired the symbol "every ~5-6 minutes all morning", and the open-position guard released the moment the position closed). First live boot with the rule: Monday 3-Aug-2026. From 09-Aug-2026 ("TICK 2") the count is per book (intraday or delivery), so a closed delivery trade no longer uses the intraday slot; an unknown intent counts account-wide. Production and testing configs both set it true.
- HISTORICAL (Rama, Q26 and Q28): "one stock + one direction once a day per segment (meaning Intraday & Delivery has 2 independent pipelines)". Consistent with source from 9-Aug-2026, with one qualification: an open position in either book blocked the other book while it was open. Before 3-Aug-2026 there was no day-scoped rule; a symbol could re-enter as soon as its position closed.
- Two strategies could receive the same symbol on the same day (scanners with identical or overlapping conditions, section 17). The first candidate admitted held the symbol; the others were rejected while it was open and, from 3-Aug-2026, for the rest of the day in the same direction and book.

## 15. Strategy circuit breaker (governor) (FACT, SOURCE-VERIFIED unless marked)
- capital/strategy_governor.py (FIX-130 Item 6), built in main.py; config strategy_circuit_breaker: enabled true, loss_multiplier 2.0, cutoff_time 12:00, lookback_days 10.
- Checked on every signal after the strategy window: before 12:00, the strategy is paused for the day when today's realised gross P&L from closed trades is below 2 x the average P&L of its losing days in the previous 10 days. No losing history means no pause. The pause is held in memory; a database error fails open. Rejection: REJECTED_STRATEGY_CIRCUIT_BREAKER, with a WARNING alert.
- KNOWN DEFECTS (RECORD-DERIVED, code comments): until 24-Jun-2026 the pause alert raised a TypeError and was "silently lost". Ledger #9 / D3 (04-Aug-2026) ruled the behaviour itself wrong (the pause should persist; cutoff_time does two jobs): a restart before 12:00 re-derives the loss and re-pauses, while a restart after 12:00 lets the strategy resume "however large the loss". At the baseline only the alert wording had changed.
- Whether it ever paused a production strategy: UNKNOWN (Rama Q10: UNKNOWN; no logs survive).

## 16. PB01 breakout and retest (FACT, SOURCE-VERIFIED unless marked)
- YAML pb01_breakout_retest: LONG, INTRADAY, enabled false (fail-closed until a "spec-13 promotion gate"), v3_playbook true, horizon NEXT_DAY, window 09:25 to 11:00; scanner_type eod in the scanner map.
- Flow: the Chartink end-of-day alert is captured to a watchlist (watchlist.enabled true; "ANALYSIS ONLY — no position/capital/CNC/GTT"). Next morning, from 09:20 to 11:00 on 5-minute candles, a retest-and-confirm stage evaluates it and writes would-be records only (data_store/v3/pb01_would_be.jsonl). A raw firing on the intraday path would be rejected at the strategy-control gate.
- Not in S12 (added later); Chartink definition XVI is marked "(SHADOW)". HISTORICAL (Rama, Q16): a placeholder.

## 17. Chartink scanner conditions (FACT, SUPPLIED RECORD unless marked)
- All 16 definitions use only the Daily timeframe (daily open, high, low, close, VWAP and volume; daily EMAs; volume SMAs). None uses an intraday candle. Shared filters: average daily volume (SMA20 or SMA50) above 50,000; close between 100 and 5,000; today's volume above 1.5 x that average (all 16); today's high-low range above 0.5% of close (13; not II, III or XVI).

| # | Scanner | Conditions besides the shared filters |
|---|---|---|
| I | POSITIONAL SWING LONG [Delivery] | Open = Low; Close > Open, > VWAP, > previous-day High, > EMA200 |
| II | POSITIONAL SECTOR ROTATION [Delivery] | Close > previous-day 20-day high; Close > Open, > EMA50; volume vs SMA50 |
| III | POSITIONAL MOMENTUM LONG [Delivery] | Close > previous-day 252-day high; Close > Open, > EMA200, > EMA50; volume vs SMA50 |
| IV | RANGE BREAKOUT SHORT | 15-day high-low range < 2% of Close; Close < 15-day low; Close < Open, < VWAP, < EMA20 |
| V | RANGE BREAKOUT LONG | 15-day range < 2% of Close; Close > 15-day high; Close > Open, > VWAP, > EMA20 |
| VI | GAP FADE SHORT | Open > 1.01 x previous Close; Close < Open, < VWAP, < EMA20, < previous Close |
| VII | GAP FADE LONG | Open < 0.99 x previous Close; Close > Open, > VWAP, > EMA20, > previous Close |
| VIII | GAP GO SHORT | Open < 0.99 x previous Close; Close < Open, < VWAP, < EMA200, < previous-day Low |
| IX | GAP GO LONG | Open > 1.01 x previous Close; Close > Open, > VWAP, > EMA200, > previous-day High |
| X | VWAP REJECTION SHORT | Close < VWAP; High >= VWAP; Close < Open, < previous Close, < EMA20 |
| XI | VWAP BOUNCE LONG | Close > VWAP; Low <= VWAP; Close > Open, > previous Close, > EMA20 |
| XII | FIRST PULLBACK SHORT | Close < VWAP, < EMA20; High >= EMA20; Close < Open, < previous Close |
| XIII | FIRST PULLBACK LONG | Close > VWAP, > EMA20; Low <= EMA20; Close > Open, > previous Close |
| XIV | OPEN HIGH BREAKDOWN SHORT | Open = High; Close < Open, < VWAP, < previous-day Low, < EMA200 |
| XV | OPEN LOW BREAKOUT LONG | Open = Low; Close > Open, > VWAP, > previous-day High, > EMA200 |
| XVI | PB01 BREAKOUT + RETEST (SHADOW) | Close > previous-day 20-day high; Close > EMA50; EMA50 > EMA200; Close > Open; volume vs SMA20; no range filter |

- Overlaps (the conditions are FACT from the supplied file; the subset readings are INTERPRETATION by logic). Terms used: "scanner-condition duplicate" means identical supplied conditions; "scanner subset" or "scanner overlap" means one scanner's matches are contained in, or largely shared with, another's. Neither means the strategies were duplicates, which would need equivalent downstream behaviour.
  - Scanner-condition duplicate: I (POSITIONAL SWING LONG) and XV (OPEN LOW BREAKOUT LONG) have identical supplied conditions. They fed two different strategies, positional_swing_long (delivery: 2% stop, 0.2% entry offset, no pullback wait) and open_low_breakout_long (intraday: 1% stop, 0.1% offset, pullback wait), so they are not identical strategies.
  - Scanner subsets: XI VWAP BOUNCE LONG is the long "core": its extra condition Low <= VWAP is always true, because a day's VWAP lies between that day's low and high. VII GAP FADE LONG and XIII FIRST PULLBACK LONG are the same core plus one condition each (a gap-down open; the low touching EMA20), so every VII or XIII match was also an XI match. Short mirror: X VWAP REJECTION SHORT (High >= VWAP is always true) contains VI GAP FADE SHORT and XII FIRST PULLBACK SHORT.
  - Scanner overlaps: IX GAP GO LONG and XV OPEN LOW BREAKOUT LONG share 9 of 10 conditions and differ only in the open condition (gap-up open versus Open = Low); likewise VIII GAP GO SHORT and XIV OPEN HIGH BREAKDOWN SHORT.
  - Scanner subset: III POSITIONAL MOMENTUM LONG is contained in II POSITIONAL SECTOR ROTATION: a close above the 252-day high is above the 20-day high, and III repeats II's other conditions. XVI PB01 is II with EMA50 > EMA200 and an SMA20 volume base.
  - II "SECTOR ROTATION" has no sector condition.
  - AUDIT LEAD: the range-breakout conditions (IV, V) may be self-defeating if the 15-day maximum or minimum includes the current day, because a close cannot be beyond an extreme that includes its own day (II and XVI use "1 day ago Max(20, ...)" instead). BK-1's zero closed trades for both range strategies is consistent with this suspicion but does not prove that the scanners generated no signals. Chartink's exact implementation is not established; the StockCharts discussion cited in A-008_v2_design_inputs.md section 16 is context, not proof.
  - Grouping: of the 16 Chartink definitions, 15 are normal trading definitions and XVI (PB01) is the 16th, shadow and non-trading. The 15 normal definitions can be grouped into about five condition families: the long core above VWAP, EMA20 and the previous close (XI, VII, XIII) and its short mirror (X, VI, XII); a close beyond the previous day's high or low with an EMA200 trend and an open condition (XV and I, IX; short XIV, VIII); 20-day and 52-week high breakouts (II, III; PB01 is a variant); and the 15-day range breakouts (IV, V).
- KNOWN AMBIGUITY (SOURCE-VERIFIED descriptions): several YAML descriptions do not match the scanners: open_low_breakout_long "breaks above the opening range low after initial consolidation" (scanner: daily Open = Low); range_breakout "consolidated intraday range" (scanner: 15-day daily range); positional_swing_long "swing reversal setups at key support levels" (scanner conditions identical to XV's); positional_sector_rotation "outperforming sector leaders" (no sector condition); positional_momentum_long "ATR-based risk management" (sl_method FIXED_PCT).
- Whether these conditions ran unchanged on every date: Rama reports no material changes (Q5). UNKNOWN, and not inferred from Chartink's current documentation: the legacy Chartink plan, the actual alert frequency, the Unique or Duplicate setting, any historical scanner changes, and whether the stored legacy URLs matched the live screeners. FACT (RECORD-DERIVED, 27-Jul-2026 SENCO comment): one scanner fired the same symbol every 5 to 6 minutes all morning, so repeat alerts did reach the system.
- CURRENT EXTERNAL FACT (Chartink, checked 2026-10-07): intraday scan timeframes exist (from 1 minute on the paid plan, 5 minutes on the free plan); on the paid plan scan data update every minute (a 9:20 scan uses data up to 9:19:59; the free plan is 5 minutes delayed); alerts run at a configurable frequency (every 1 to 240 minutes, or at market open or close), either "Duplicate" (repeat alerts when a stock matches again) or "Unique" (one alert per stock per day), and can be delivered by webhook. These are current Chartink facts, not evidence of the legacy settings.

## 18. Late signals and outcomes
- HISTORICAL (Rama, Q17, Q32 and his 07-Oct note): the highest-importance defect: signals arrived for stocks that had already run for the day, leaving no room, so trades hit the stop. Rama reported approximately 98% SL outcomes and a success ratio below 10% ("98% SL"; "success ration is around <10% only") for an unspecified period; the measurement population and date range were not supplied. These are attributed historical figures, not a measured system KPI.
- FACT: the scanners used the forming current-day daily values (during the session the Daily candle is the day's forming candle; Chartink: values change while the market is open), so each condition became true when those values crossed its threshold. The legacy system also had a global 10:00 entry floor and downstream processing delays (screening; for six strategies a pullback wait of up to 180 seconds).
- INTERPRETATION / AUDIT LEAD: the scanners' conditions could become true only after the current-day values crossed the specified thresholds and therefore could produce signals after a substantial part of the move. This is a credible mechanism for late signals; the combination of conditions may delay some signals, and the volume threshold may be reached later depending on the stock's intraday volume curve. The legacy evidence does not establish that every signal was late or quantify the delay.
- UNKNOWN: whether every signal was late; whether the volume condition could only become true late; the exact delay attributable to the scanners; how much of the poor performance came from scanner timing rather than screening, price construction, entry timing or market regime.
- FACT (RECORD-DERIVED, BK-1, 17-Jul-2026; 155 closed trades from 15-Jun to 16-Jul-2026, 23 trading days; net figures are optimistic because 36 manual closes were recorded with zero charges):
  - LONG: 141 trades (91%), net -0.150 R, win rate 36.9%, 69 stop hits against 44 target hits. SHORT: 14 trades, net +0.298 R, win rate 64.3% (sample too small to rely on).
  - Intraday longs -0.239 R (n=108); positional longs +0.136 R (n=33).
  - Worst: vwap_bounce_long -12.10 R (n=35), the only loser with a large sample; best: positional_sector_rotation +4.46 R (n=19). "The big losers are negative on GROSS too — they are negative-edge, not cost-killed."
  - Short orders filled 19% of the time against 49% for longs. With no index data stored, the July market effect could not be separated.
- KNOWN AMBIGUITY: Rama's reported figures (about 98% SL; success below 10%; unspecified period) and the BK-1 measured dataset (15-Jun to 16-Jul-2026, 155 closed trades: 36.9% long win rate, 49% of long trades stopped) are separate datasets and are not merged; neither overrides the other. Performance after 16-Jul-2026 is unmeasured and the database is gone (Q-34).

## 19. Paper versus live
- FACT (SOURCE-VERIFIED): the strategy resolver has no mode branch ("paper and live evaluate identically"); YAMLs, scanner map and screening are shared by both modes.
- HISTORICAL (Rama, Q29 and Q30): same strategy configuration and logic; paper-only differences UNKNOWN. Paper and live differences elsewhere (A-007 section 14) are not strategy-layer differences. Legacy only (DR-001).

## 20. Production versus testing
- FACT (both configs): entry_start 10:00 in production, 09:30 in testing. entry_end, eod_entry_cutoff, the product gates and the symbol-direction rule are the same. Testing strategy YAMLs and scanner map were not compared.
- Alert names: section 9 (testing names carried the client-ID prefix; the shared code strips it since 09-Sep-2026; before that every testing signal was rejected).
- HISTORICAL (Rama, Q31): no other strategy difference known. No testing difference overrides production.

## 21. Strategy field to consumer trace (FACT, SOURCE-VERIFIED)
"No effect identified" means none was found in the inspected production path; the full caller graph was not inspected.

| Field | Consumer | Effect at the baseline |
|---|---|---|
| name | loader key; scanner map; trades.strategy; caps; circuit breaker | Identity |
| direction | order side; trades.direction; registry mirror | BUY for LONG, SELL for SHORT |
| intent | strategy gate (trade_type); product resolver; capital bucket | MIS or CNC (A-007) |
| enabled | strategy gate | false only for pb01 |
| order_protocol, pipeline, horizon | No production consumer identified in the inspected path | No effect identified |
| v3_playbook | PB01 shadow path; allocator scope (shadow) | Marks the playbook |
| entry_method, entry_offset_pct | price derivation | LIMIT, 0.1% (delivery 0.2%) better than the trigger |
| sl_method, sl_pct, sl_min_pct, sl_max_pct | price derivation | Fixed % stop, clamped |
| sl_atr_multiplier, tgt_atr_multiplier | No consumer identified in the inspected path (ATR stop not implemented; tgt_method RISK_REWARD in all) | No effect identified |
| sl_gap_buffer_pct | price derivation (09:15 to 09:30 only) | No effect identified at the baseline start times |
| tgt_method, tgt_risk_reward | target derivation | R:R 1.5 |
| smart_tgt_* | SmartTgtManager (registered for CO trades only) | No effect identified in the inspected path |
| trailing_sl_enabled (schema default false; set by no YAML) | BreakevenManager registration | No registration identified |
| pullback_wait_* | EntryGate, or fresh-LTP re-anchor when off | Pullback or momentum entry |
| min_score | screener | 0, so the global 60 |
| min_volume_surge, min_adr_pct, max_spread_pct | screener steps | Per-strategy thresholds |
| max_concurrent_positions | processor cap (H-7) | 2 or 3 |
| lot_size | sizing | 1 |
| entry_start_time, entry_end_time | strategy window | Start never bound; end equals the global end |
| active_days | strategy window | MON to FRI |

## 22. Unknown / not established
- Which strategies were enabled on each historical date; the full change history of the YAMLs and scanners (git history not read, per LEGACY_REFERENCE).
- Chartink plan and alert settings (frequency; duplicate or unique); whether the slug URLs matched the real screeners; whether Chartink's 15-day extremes include the current day (range breakouts).
- Whether the circuit breaker ever paused a strategy.
- Which screening steps were dead or pinned (the 65 ceiling).
- Runtime direction-registry state.
- Performance after 16-Jul-2026; stop-hit and win rates per strategy over the whole live period.
- When force_intraday_only was switched off; when the testing-VM prefix outage began.

## 23. Known defects / ambiguities
1. Late signals: a HISTORICAL defect reported by Rama. The scanners' use of the forming daily candle is a credible mechanism (INTERPRETATION); the delay and its share in the poor results are not established (section 18).
2. One common screener whose score could not exceed 65: HIGH tier unreachable; per-strategy filtering limited to three thresholds.
3. Scanner-condition duplicate and scanner subset/overlap relations (I and XV identical conditions, feeding different strategies; VII and XIII inside XI; VI and XII inside X; 9-of-10 pairs; III inside II); the range-breakout conditions may be self-defeating (audit lead).
4. Settings declared with no effect identified in the inspected production path: order_protocol CO_PLUS_TGT (13 YAMLs), smart_tgt_enabled (13), ATR multipliers (16), sl_gap_buffer_pct (4). The per-strategy entry_start_time (16) never bound at the baseline values, and entry_end_time added nothing after 29-Jun-2026.
5. R:R 1.5 and min_score 0 in every YAML: per-strategy in structure, uniform in effect.
6. Records not kept current: locked S6, S13, S14 and S15 against the deployed YAMLs; YAML descriptions that do not match their scanners; a scanner URL file of placeholders.
7. Stale comments about strategy behaviour: force_intraday_only "forced to intent=INTRADAY at load" (config and schema); CFG-5 "gap_fade_long cuts off at 11:30"; the testing config's "while this says 10:00".
8. Circuit breaker: in-memory pause with restart gaps (D3); alerts lost until 24-Jun-2026.
9. Every testing-VM signal rejected before 09-Sep-2026 (alert-name prefix); fixed in shared code.

## 24. Cross-references
Trading hours and windows: A-003. Instruments and universe: A-004. Capital, sizing and risk gates: A-005. Broker and webhook authentication: A-006, A-010. Products and protocols: A-007 (erratum E-1 found during A-008). Configuration model: A-009. Signal intake and mapping: C-001 to C-011. Screening and score: D-003, D-017. Entry timing: D-016. Price construction: E. Shadow engines: J-010 to J-013. V2 inputs from this discussion: A-008_v2_design_inputs.md and V2_DESIGN_LOG.

## 25. Final A-008 statement
> At the baseline the legacy strategy model had 16 strategy YAMLs (the 15 locked names plus the later PB01 shadow playbook): 13 intraday (12 enabled) and 3 delivery, each with one declared direction and one intent, mapped one to one to Chartink scanners through scan_webhook_map, whose key had to equal the webhook path. The webhook carried no side; direction, intent and parameters came from the YAML, gated by the per-strategy switch and the global trade_type (force_intraday_only made delivery strategies dormant; it no longer rewrote intent after 10-Jul-2026). Each YAML set a fixed stop %, an R:R (1.5 in all), an entry offset, pullback or momentum entry, three screening thresholds and a position cap of 2 or 3; the risk %, portfolio limits, pass score and tiers were global. All strategies then shared one secondary screener whose score could not exceed 65, so passing trades scored 60 to 65 and were sized at 50% or 70%. Same-symbol rules blocked a symbol while it was open and, from 3-Aug-2026, allowed one executed trade per symbol and direction per day (per book from 9-Aug). Every Chartink scanner used the forming daily candle; one pair had identical conditions and several were subsets of others. Rama reports that signals arrived after the move and mostly hit the stop (about 98% SL, period unspecified); the separate BK-1 record (15-Jun to 16-Jul-2026) measured a 36.9% long win rate.

## Changes from the original (29-Sep-2026 R1)
- Status: the original said both "Frozen: 29-Sep-2026" and "OPEN / REOPENED"; this copy was a draft for ChatGPT's review and is now FROZEN (revision 1, DR-023).
- Moved out (kept in full in A-008_v2_design_inputs.md): section 28 (V2 inputs) and the V2 parts of sections 8, 18 and 26.
- Corrected from direct source reads: the count (16 YAMLs at the baseline: the 15 S12 names plus PB01; 13 intraday and 3 delivery, not 10 / 5 / 1); direction is declared in every YAML; the webhook carries no side; the direction registry only mirrors the YAML; force_intraday_only no longer rewrote intent after 10-Jul-2026; the per-strategy window's start never bound and its end equals the global end; an unmapped scanner gets 404 ~~and a duplicate scanner name fails start-up~~ (superseded by erratum E-1: a duplicate scanner key is not rejected; the pinned parser keeps the last occurrence); the testing alert-name fix is shared code (WR4b, 09-Sep-2026), not a testing-only manual adjustment; the per-strategy position cap existed and was enforced; risk % was global and SL % per strategy; the circuit breaker was enabled and wired (only its effect is unknown).
- Added: per-strategy parameter table and the price formulas; the locked-record drift; the common screener's steps, weights, pass score, tiers and the 65 ceiling; the same-symbol rules with their dates; the circuit-breaker logic and defects; the PB01 watchlist path; the pullback gate and the momentum re-anchor; the Chartink condition table, overlaps and description mismatches; the BK-1 performance record and its conflict with Rama's figures; the field-to-consumer trace; production versus testing from both configs; a current Chartink external fact.
- UNKNOWNs closed: Q9 (windows), Q13 (unknown scanner), Q14 (mapping collision), Q22 (strategy-to-order fields: direction, intent, entry offset, SL, R:R; products per A-007), Q27 (position cap), the governor's activation (its effect stays UNKNOWN).
- After ChatGPT's value-added review (07-Oct-2026): no legacy fact changed. Wording made evidence-bounded: the late-signal mechanism split into FACT, INTERPRETATION / AUDIT LEAD and UNKNOWN (section 18); Rama's ~98% SL kept as attributed historical evidence for an unspecified period, separate from the BK-1 measured dataset (sections 18, 25); current Chartink documentation kept apart from the unknown legacy plan and alert settings (section 17); the range-breakout self-defeat kept as an audit lead (sections 17, 23); scanner-condition duplicate, scanner subset/overlap and strategy duplication distinguished (sections 17, 23); 16 Chartink definitions = 15 normal trading definitions + PB01 shadow, with the 15 grouped into about five families (section 17); "inert" replaced by "no effect identified in the inspected path" where the full caller graph was not inspected (sections 10, 21, 23). Status set to FROZEN (DR-023).

## Erratum (2026-10-08, found during A-009)
- E-1 (sections 4 and 8, scanner-map validation): four lines said the scanner map was cross-validated at load (S10: every mapped strategy must exist) and that a duplicate scanner name failed start-up ("the scanner map is cross-validated"; "Duplicate scanner names -> CRITICAL startup failure"; "S10 cross-validation at load"; "a collision fails start-up"). They were taken from the loader function and the map file's header, not from the boot call path. Reading the callers for A-009 showed that strategies/loader.py runs S10 only when given scan_webhook_map_path, and none of the five callers read passes it (main.py Phase 0e; utils/startup_checks.py check_strategy_configs; the pre-flight config_sanity, strategy_configs_valid and long_strategies_enabled checks). The map's schema has no duplicate or existence check and the signal processor copies the map without validating it. YAML 1.2.2 requires mapping keys to be unique, but the pinned parser (PyYAML 6.0.3, yaml.safe_load) accepted a duplicated key in a test and kept the last occurrence without error; so the legacy path did not reject a duplicated scanner name, and its last entry would have been used. A mapped strategy without a YAML would have been rejected per signal at runtime (REJECTED_UNKNOWN_STRATEGY). At the baseline the 16 scanner keys are unique and every mapped strategy has a YAML, so no legacy outcome in this manual changes; only the description of the safeguard does. The unknown-scanner 404 (WR4) stays correct. The original lines stay visible, struck through. Details: A-009_manual.md section 5 and A-009_review_notes.md section 6. Accepted by ChatGPT on 08-Oct-2026 (A-009 review, accepted point 9) and recorded in DR-024 (2026-10-08).

## Erratum (2026-10-08, found during A-010)
- E-2 (section 9, redaction): an alert-name example quoted the testing account's Zerodha client ID. A client ID is the account's login ID, which the legacy system kept in .env as a credential (A-010_manual.md section 3), so V2 documents do not reproduce it (project instructions 8.3). The ID is replaced by a placeholder and, unlike other errata, not kept visible, because keeping it would defeat the correction. No legacy fact changes. This is a documentation redaction only, not eradication: the value remains in this private repository's history and in the stored input files, which are immutable, and the exposure of an identifier already published in the legacy repositories is a separate credential-risk matter (A-010_v2_design_inputs.md section 2; VP-50). The same placeholder is applied in A-008_review_notes.md (rows F-6 and "5 Changes; naming note"). Accepted by ChatGPT on 09-Oct-2026 (A-010 review, point 3) and recorded in DR-025 (2026-10-09).

## Erratum (2026-10-16, found during B-005; revised 2026-10-19)
- E-3 (section 5, the side of an orphan broker position). Status: ACCEPTED. Proposed during B-005 (2026-10-16), revised 2026-10-19 in B-005's revision 1 after ChatGPT's value-added review of 19-Oct, accepted in ChatGPT's acceptance review of revision 1 (19-Oct-2026), approved by Rama with the B-005 freeze and recorded in DR-041 (2026-10-19). The struck sentence stays visible; the replacement is the text in force. Authoritative passage: B-005_manual.md section 7 (matching; CHECK2) and section 5 (the in-flight adoption, not wired in any version read; rows W3 and W9).
- What changed: the 17-Jul direction record (docs/audit/strategy_direction_investigation_17jul2026.md lines 73 to 79) cites `order_reconciler.py:21` for an adoption "from the **broker's** actual side"; that line is the reconciler's header docstring (RC5's "(b) ORPHAN_ADOPTION — broker position, no local trade"). The branch it names logs the position as a human order and adopts nothing (orders/order_reconciler.py lines 1796 to 1885), as the 22-Jul census found for every ORPHAN_ADOPTION row ("trade_id = NULL", docs/audit/orphan_adoption_22jul2026.md lines 77 to 80); the only adoption code (state_store adopt_recovery_trade_to_open, called from the in-flight recovery, orders/order_reconciler.py line 4436) flips an existing recovery-state trade to OPEN and did not run in any version read (B-005_manual.md section 5).
- Revision 1 (2026-10-19): "never wired" and "never ran" now read "not wired in any version read" and "did not run in any version read" (B-005_manual.md section 5, row W9). No struck text changed.
- Passages checked and not changed: section 5's other lines (the webhook carries no side; the direction accessor and registry); section 14 (the same-symbol rules, DUPLICATE_SYMBOL among them); section 23; A-008_v2_design_inputs.md (PROPOSED material, not edited: its direction-conflict consideration stands for V2, and its example, "an adopted broker position", has no legacy path behind it; B-005_v2_design_inputs.md section 3).
- Reason: a docstring or a check name is a claim about the code; the record cited the module header, not the branch (V2_DESIGN_LOG lesson of 2026-10-16).
- Impact: wording only for the strategy model: realised direction stays derived from the YAML in every inspected path; the record's DIRECTION_CONFLICT premise (an independent broker-side source) has no inspected path behind it. No V2 decision depends on the old wording.
