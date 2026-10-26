# D-001 - PRE-FILTER: BOUNDARY MAP (LEGACY MANUAL, DRAFT 1)

Status: DRAFT 1 for ChatGPT's review, 26-Oct-2026 (IST). Not accepted and not frozen; nothing here is committed or pushed.
Topic: D-001 Pre-filter (master agenda, section D). Scope: Option C, a boundary-map chapter, as recorded in D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt. Legacy system status: DEAD / REFERENCE ONLY.
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb, read on 25-Oct-2026 in D-001 Stage 1 through the gated masking filter: 20 count-only searches with 2 positive controls, then 60 logged narrow reads of 16 source and configuration files at their pinned blobs. This draft makes no new legacy access: every source citation below is one already made in the Stage 1 package, and every claim ID (P-, S-, H-, E-, AF-, B-, W-, X-) is a claim of D-001_STAGE1_SOURCE_EVIDENCE_25-OCT-2026.txt.
Companion files: D-001_STAGE2_CHANGE_NOTE_26-OCT-2026.txt and D-001_STAGE2_FINAL_CHECK_26-OCT-2026.txt.
Purpose: a factual map of the checks on the legacy main path before the secondary screener's ten-step scoring, as the pinned source defines them, and of the separate HardGate.evaluate battery, which the screener calls in a shadow compare after the score: where each check lives, its order, its hand-offs, its status on the repository configuration, and which chapter owns its detail. It is not a V2 design, a recommendation, or a second manual for any mapped mechanism.
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name from the legacy source or the host records is reproduced; Rama is named only as the maker of the scope decision (section 1.2).
Process provenance: in Stage 1 the masking filter also loaded two legacy host records in memory as its host-token source on every run; nothing from them was printed or used as evidence, and Stage 1's "16 files were read" counts only the files read for evidence. The erratum is D-001_STAGE1_PROVENANCE_CLARIFICATION_REV1_25-OCT-2026.txt, accepted in the scope decision record. Stage 2 opened no legacy file and ran no filter.

## 1. Purpose and scope

### 1.1 What this chapter maps

Three layers of checks on the intraday main path at the pin (the first in two parts, Layer 1 and Layer 1b), and the hand-offs between them:

1. Layer 1: the processor's pre-flight checks, the block the code labels "Step 1: Pre-flight checks (cheap, no I/O)" (section 4).
2. Layer 1b: the processor's strategy checks, "Step 2: Strategy lookup (SPW3)" (section 5).
3. Layer 2: the early checks inside SecondaryScreener.screen(), before its ten-step executor and scoring path (section 6).
4. Layer 3: the separate, broader V3 HardGate.evaluate battery, which runs in a shadow comparison on the repository configuration (section 7).

"Layer 1", "Layer 1b", "Layer 2" and "Layer 3" are this chapter's labels, not names in the source.

### 1.2 Scope basis (HISTORICAL / RECORD-DERIVED)

- D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt: Rama selected Option C on 26-Oct-2026. The record says: "D-001 is a map, not an ownership transfer. Detailed rules remain with their existing recorded owners (including Section C, D-002, D-003, J and other assigned topics)."
- D-001_CLAUDE_PROMPT_STAGE2_MANUAL_DRAFT_26-OCT-2026.txt authorises this first draft and sets its sections.
- D-001_CHATGPT_STAGE1_RECONCILIATION_REVIEW_25-OCT-2026.txt accepted the Stage 1 evidence package with one process-provenance clarification, answered by the revision 1 erratum (see Process provenance above).

### 1.3 What "Pre-filter" names here

- FACT (SOURCE): the processor calls its first block "pre-flight" (`signals/signal_processor.py:6`, `signals/signal_processor.py:902`) and uses "pre-check" for the FIX-007 rate limiter and the allocator's "portfolio pre-checks"; no legacy source read in Stage 1 calls any step a "pre-filter" (W-02).
- FACT (COUNT): "pre" followed by "filt" occurs in 3 of the 281 runtime files: 2 lines in capital/kill_switch.py, 1 in core/config_snapshotter.py and 1 in core/state_store.py; it occurs in none of the four authorised configuration files and none of the seven authorised tests (W-01; count-only searches Q10 to Q12, the zero results Q11 and Q12 each with a positive control). The two kill-switch lines are comments in the emergency-flatten code about a product default, not a signal check (B-06; `capital/kill_switch.py:1850`, `capital/kill_switch.py:1983`; the context of line 1850 at `capital/kill_switch.py:1838-1852`). The other two lines were not read (G-10).
- INTERPRETATION: "Pre-filter" is the master agenda's label for this chapter. No legacy source inspected in Stage 1 names a single component "pre-filter" (W-01, W-02; two lines not read, G-10); this chapter uses the label for the map of the three layers above.

### 1.4 Not covered here (cited, not re-analysed)

- The detailed rules of each mapped mechanism: they stay with their recorded owners (section 10).
- The ten-step executor, the score, the tier and the minimum score (D-003 and D-017 by the records; M14, M15).
- What happens after the screener: price derivation, the retest divert, the entry gate, sizing, risk, reservation, the entry throttle and placement (section 10.3).
- The EOD / PB01 route (C-011; M21).
- Deployed or runtime behaviour: no deployed configuration, host, log or database was read.
- V2 retention or design: this chapter makes no V2 decision or recommendation.

## 2. Evidence status legend

| Label | Meaning in this chapter |
|---|---|
| FACT (SOURCE) | Code lines read at the pin in Stage 1, cited as `path:line` (lines printed by a logged read at the pinned blob). Stage 1's narrower labels are kept where they matter: FACT (TEXT) is a comment or docstring, a claim and not behaviour; FACT (CONFIG) is a repository configuration value, not a deployed host's; FACT (COUNT) is a count-only search result, with no line read. |
| HISTORICAL / RECORD-DERIVED | A V2 record: one already cited in Stage 1 (for example C-008's stage table or C-004 section 10), or one of this stage's governing records (section 1.2). Never a legacy-source fact. |
| INTERPRETATION | A reading that joins facts; the facts it rests on are named. |
| UNKNOWN / NOT ESTABLISHED | Not established by the bounded Stage 1 evidence. A bounded read that found nothing is not proof of absence. |
| KNOWN DEFECT | Not asserted anywhere in this chapter. The one defect label near this map (the entry gate's add(), B-009 D1) is HISTORICAL and was not re-verified (AF-01). |

Status discipline: a path existing is not its code being called; code being called is not it rejecting a signal; a configured rule is not an enabled or effective one in a deployed runtime; a shadow verdict is not a live decision.

Default path (Stage 1's term): the main path through screen() when the V3 enforce branch is not taken and no exclude_steps are passed, read here on the repository configuration. It is not the path under the constructor defaults (V3 mode "off", MIS filter off), which differ from the repository values.

Quotations are verbatim as Stage 1 records them: comment markers and line breaks dropped, non-ASCII dashes and arrows shown in ASCII, "..." for words left out.

## 3. Simplified historical flow

The intraday main path at the pin, on the repository configuration. "->" is an order the source shows; [inferred] is a link Stage 1 did not read. Section numbers point to the detail.

```text
webhook receiver, queue (Section C)
   |  [inferred: the dispatcher hands the item to _process_one; C-006, C-008]
   v
_process_one writes PROCESSING
   |
   v
LAYER 1   "Step 1: Pre-flight checks (cheap, no I/O)" - processor (section 4)
          kill switch (entry) -> global entry window -> signal age (600 s, repository value)
          -> shadow-inning guard
   |      a refusal: the processor's handler stores REJECTED_ plus the check name
   v
LAYER 1b  "Step 2: Strategy lookup (SPW3)" - processor (section 5)
          strategy lookup -> strategy control -> strategy window -> strategy circuit breaker
   |
   v
"Step 3": the processor calls SecondaryScreener.screen(..., market_data=None)
   |
   v
LAYER 2   inside screen(), before the ten steps - screener (section 6)
          step 0   MIS learned-blocklist pre-drop  (shadow on the repository values: logs, falls through)
          step 1   quote availability              (none or a failure -> SKIPPED_QUOTE_UNAVAILABLE)
          [V3 enforce branch: mode "enforce" only; not taken on the repository values; see LAYER 3]
          step 1b  circuit proximity via hard_gate.circuit_proximity_reason (enabled)
                   -> REJECTED_CIRCUIT_PROXIMITY
   |      the screener writes its own statuses; the processor does not double-write them
   v
TEN-STEP PIPELINE AND SCORE (D-003; outside this map)
          thresholds -> StepExecutor.run_all (ten steps) -> step error -> score (step 5)
             |
             +--> LAYER 3  V3 shadow compare (mode "shadow"): HardGate.evaluate (at-circuit,
             |             circuit proximity, freshness 60 s, liquidity no-op) and the re-scale;
             |             logged only (section 7)
             v
          min score (step 6) -> signal_age 0.0 (step 7) -> PASSED (step 8)
   |
   v
AFTER THE SCREENER (outside this map): entry and SL prices -> retest divert (not built on the
   repository values) -> [inferred: sizing, portfolio lock, risk, reservation, target; C-008 ST-16
   to ST-23] -> late kill switch -> shutdown -> entry throttle -> placement
```

Off the main path (outside this map): the entry gate holds screened signals and resumes after screening, and a source comment calls that path dormant (AF-01); the retest monitor resumes after a divert, which is not built on the repository values (AF-02, P-09); in the main.py PB01 wiring read in Stage 1 the PB01 stages are given neither the processor, the screener nor the hard gate (X-02); a PB01 hand-off is not disproved (X-03).

## 4. Layer 1: the processor's pre-flight checks

### 4.1 Where it runs

FACT (SOURCE): `_process_one` writes PROCESSING first (`signals/signal_processor.py:896-899`); then the block the code labels "Step 1: Pre-flight checks (cheap, no I/O)" runs (`signals/signal_processor.py:901-903`) (P-03). FACT (TEXT): the module docstring gives the pipeline as "pre-flight -> strategy lookup -> secondary screen -> price derivation -> position sizing -> risk approval -> capital reserve -> order placement -> status update" and says the processor "Owns orchestration only; delegates all decisions to injected modules." (`signals/signal_processor.py:4-10`, P-01). That is a docstring, not behaviour; HISTORICAL: C-008 CX-02 records processor-owned checks against that claim.

### 4.2 The four checks, in source order

| Order | Check | Call and outcome (FACT (SOURCE)) | Evidence |
|---|---|---|---|
| 1 | Kill switch, entry | `self._ks.is_active("entry")` -> KILL_SWITCH; is_active("entry") is true for SOFT_KILL or HARD_KILL | P-03 `signals/signal_processor.py:905-907`; B-01 `capital/kill_switch.py:510-520` |
| 2 | Global entry window | `self._mw.is_entry_allowed(now)` -> OUTSIDE_ENTRY_WINDOW; false on a trading holiday, else entry_start <= t < entry_end | P-03 `signals/signal_processor.py:909-912`; B-02 `core/market_windows.py:143-150` |
| 3 | Signal age (processor expiry) | the signal age against `self._signal_expiry_sec` -> EXPIRED; FACT (TEXT) comment "defense-in-depth; receiver also checks"; FACT (CONFIG) signal_queue.expiry_sec 600 (`config/system_config.yaml:127`), wired at `main.py:3614` and `signals/signal_processor.py:917` | P-03 `signals/signal_processor.py:914-921`; the value and wiring: Stage 1 evidence section 9, M03 |
| 4 | Shadow-inning guard | the shadow-inning helper: with no tracker object it returns; `is_tracking(symbol)` true -> SHADOW_INNING_ACTIVE; an exception -> SHADOW_TRACKER_ERROR, "failing closed"; FACT (TEXT): its docstring says all three entry paths enforce it | P-03 `signals/signal_processor.py:923-928`; P-07 `signals/signal_processor.py:1976-2008`, `signals/signal_processor.py:1988-1990`, `signals/signal_processor.py:1991-1997`, `signals/signal_processor.py:2000-2008`, `signals/signal_processor.py:1980-1981` |

### 4.3 How a Layer 1 or Layer 1b refusal ends

FACT (SOURCE): a _PipelineReject raised by any of these checks ends in the handler (`signals/signal_processor.py:1193-1231`): the stored status is f"REJECTED_{rej.check}" with the reason (`signals/signal_processor.py:1201`); a held reservation is released (`signals/signal_processor.py:1212-1217`); the per-check reject counter is bumped (`signals/signal_processor.py:1218-1220`); for EXPIRED a warning goes out at most once a minute (`signals/signal_processor.py:1221-1231`) (P-06).

### 4.4 A processor pre-check outside the Step 1 block, not wired

FACT (SOURCE): the FIX-007 rate-limiter "pre-check" (`signals/signal_processor.py:415-424`) runs only if `self._rate_limiter` is not None (`signals/signal_processor.py:418`), and main.py's SignalProcessor(...) call (`main.py:3575-3623`) passes no rate_limiter argument, so it does not run on the pinned wiring (P-08; consistent with C-008 ST-01, HISTORICAL). It is not part of the "Step 1" block; where it runs in the flow was not read in Stage 1 (M09).

### 4.5 Limits of Layer 1

- The window times' configuration was not read; it is A-003's subject (B-02, G-12).
- What is_tracking returns while the shadow tracker is disabled: UNKNOWN (orders/shadow_tracker.py not opened; C-008 [U-01]; P-07, G-13).

## 5. Layer 1b: the processor's strategy checks

FACT (SOURCE): after Layer 1 the same method runs "Step 2: Strategy lookup (SPW3)" (`signals/signal_processor.py:930-932`) (P-04).

| Order | Check | Call and outcome (FACT (SOURCE)) | Evidence |
|---|---|---|---|
| 1 | Strategy lookup | no scan_webhook_map entry, an entry with no strategy, or a strategy not in the loaded set -> UNKNOWN_STRATEGY | P-04 `signals/signal_processor.py:933-953` |
| 2 | Strategy control | `strategy_will_trade(...)` -> TRADE_TYPE or STRATEGY_CONTROL; inside it a disabled strategy -> DISABLED, checked first; its force-intraday and trade-type rules (FORCE_BREAKER, TRADE_TYPE) are A-008's (M06) | P-04 `signals/signal_processor.py:955-972`; B-03 `strategies/control.py:79-83`, `strategies/control.py:85-97`, `strategies/control.py:99-113` |
| 3 | Strategy window | `is_entry_allowed_for_strategy` -> OUTSIDE_ENTRY_WINDOW; the global check, then the strategy's entry_start_time and entry_end_time, missing or malformed times falling back to the global result | P-04 `signals/signal_processor.py:974-982`; B-02 `core/market_windows.py:152-175` |
| 4 | Strategy circuit breaker | `self._strategy_governor.check(...)` when a governor is set -> STRATEGY_CIRCUIT_BREAKER; the check fails open on a database error; its pause rules are F topics' (M08) | P-04 `signals/signal_processor.py:984-990`; B-05 `capital/strategy_governor.py:49-87` |

Notes:

- FACT (CONFIG) and INTERPRETATION: the repository values are force_intraday_only false (`config/system_config.yaml:142`) and trade_type BOTH (`config/system_config.yaml:149`), so on that configuration only a disabled strategy is refused by the control gate (B-03).
- FACT (SOURCE), bounded to two greps: no filter on a strategy's enabled field was found in strategies/loader.py (its one "enabled" line is a docstring about delivery_enabled, `strategies/loader.py:121`) or in main.py's hand-over of the loaded strategies (`main.py:3159`, `main.py:3584`), so a disabled strategy reaches the control gate, not the lookup (B-04).
- UNKNOWN / NOT ESTABLISHED: whether the governor is enabled on the repository configuration (_build_strategy_governor in main.py not read; B-05, G-12).
- The strategy lookup's recorded owner is C-010 (M05); the scanner map itself was not read in Stage 1 (files-read log, section 4).

### 5.1 The hand-off from Layer 1b to Layer 2

FACT (SOURCE): "Step 3: Secondary screening (SPW3, P18)" (`signals/signal_processor.py:992-996`) calls `self._screener.screen(signal_id, symbol, scanner_name, trigger_price, triggered_at, direction=..., intent=..., strategy=..., market_data=None)` (`signals/signal_processor.py:997-1004`); a status beginning "SKIPPED_" leads to a warning and a return (`signals/signal_processor.py:1008-1013`); a result that has not passed leads to a return (`signals/signal_processor.py:1015-1020`). The comment: "Processor does NOT double-write those statuses." (`signals/signal_processor.py:994-995`). Step 4, price derivation, follows (`signals/signal_processor.py:1025-1030`) (P-05).

## 6. Layer 2: the early checks inside SecondaryScreener.screen()

### 6.1 Where it runs

FACT (TEXT): the screener's header: "Orchestrates the 10-step screening pipeline: fetches market data, runs step_executor, scores via quality_scorer, applies strategy min_score override, persists result per P18." (`screening/secondary_screener.py:3-5`, S-01). FACT (SOURCE): screen() runs its steps in the order of `screening/secondary_screener.py:123-374` (S-03). Its constructor takes, among others, circuit_proximity_reject_enabled (default True), mis_blocklist, mis_filter_enabled (default False), mis_filter_shadow (default True), hard_gate and scoring_config (`screening/secondary_screener.py:66-81`); the V3 mode is scoring_config.v3_hardgate_mode, default "off" (`screening/secondary_screener.py:96`) (S-02).

### 6.2 The early steps, in source order

| Order | Step | Behaviour (FACT (SOURCE)) | Status on the repository configuration | Evidence |
|---|---|---|---|---|
| 0 | MIS learned-blocklist pre-drop | runs only when the filter is enabled and a blocklist is set; REJECTED_NOT_MIS_TRADABLE only when shadow is false; otherwise falls through | logs the would-drop and falls through: FACT (CONFIG) enabled true, shadow true, ttl_days 1 (`config/system_config.yaml:179-181`) | S-03 `screening/secondary_screener.py:142-148`, `screening/secondary_screener.py:150-164`, `screening/secondary_screener.py:165`; S-05; S-06 |
| 1 | Quote availability | when market_data is None: no quote -> SKIPPED_QUOTE_UNAVAILABLE; a failure -> the same status with an ERROR log | runs on the main path: the processor passes market_data=None (P-05) | S-03 `screening/secondary_screener.py:167-183`, `screening/secondary_screener.py:185-196` |
| - | V3 enforce branch | taken only when the mode is "enforce" and a hard gate is set | not taken: FACT (CONFIG) v3_hardgate_mode "shadow" (`config/scoring_weights.yaml:39`); see section 7 | S-03 `screening/secondary_screener.py:200-206`; H-04; H-05 |
| 1b | Circuit proximity (pre-fill) | -> REJECTED_CIRCUIT_PROXIMITY; FACT (TEXT) comment "Hard reject, not a soft score - runs before the 10-step pipeline."; the helper delegates to hard_gate.circuit_proximity_reason with enabled=self._circuit_proximity_reject_enabled | can reject: FACT (CONFIG) entry_gate.circuit_proximity_reject_enabled true (`config/system_config.yaml:661`), passed to the screener at `main.py:3288` (HardGate gets the same key at `main.py:3246`, H-02; Stage 1 evidence section 9) | S-03 `screening/secondary_screener.py:208-232`; H-01 `screening/secondary_screener.py:420-431`; H-06 |

### 6.3 Step 0: why it does not reject on the repository values

- FACT (SOURCE): for a would-drop the MIS helper logs it and returns True in shadow and active mode alike (`screening/secondary_screener.py:557-563`, `screening/secondary_screener.py:574-581`); only shadow false turns that into REJECTED_NOT_MIS_TRADABLE (`screening/secondary_screener.py:150-164`) (S-03, S-05). Its product and TTL rules are D-003's (M10).
- FACT (CONFIG): mis_filter enabled true, shadow true, ttl_days 1 (`config/system_config.yaml:179-181`); FACT (TEXT): its comment says the would-drop is logged and the signal falls through (`config/system_config.yaml:171-174`); main.py passes them as mis_filter_enabled and mis_filter_shadow (`main.py:3289-3292`) and the TTL to the blocklist (`main.py:3055`, `main.py:3057`). So on the repository configuration step 0 logs and does not reject (S-06).

### 6.4 Step 1b and hard_gate.py

FACT (SOURCE): the screener imports circuit_proximity_reason, rescale_min_score, rescaled_total and tier_for from screening.hard_gate (`screening/secondary_screener.py:29-34`), and step 1b's helper delegates to hard_gate.circuit_proximity_reason (`screening/secondary_screener.py:420-431`), so a function of hard_gate.py is on the screener's default path (H-01). This early rule is distinct from the HardGate.evaluate battery of Layer 3 (section 7.5).

### 6.5 Where Layer 2 ends

FACT (SOURCE): after step 1b, screen() takes the thresholds min_volume_surge, min_adr_pct and max_spread_pct from the strategy (`screening/secondary_screener.py:234-239`) and calls step_executor.run_all, an exception giving SKIPPED_EXECUTOR_ERROR (`screening/secondary_screener.py:251-263`) (S-03). run_all runs ten steps in a fixed order (`screening/step_executor.py:141-154`), every step runs with no stop on a 0.0, and the screener calls it without exclude_steps on the default path (`screening/secondary_screener.py:253`) (E-01). Layer 2 ends at that hand-off. Apart from Layer 3's shadow compare after the score (section 7), this chapter does not map the ten steps, the score or the minimum score; they are D-003's (with D-017 by the records; M14, M15). The screener persists every outcome itself (S-03, for example `screening/secondary_screener.py:182` and `screening/secondary_screener.py:231`), and the processor does not double-write those statuses (P-05).

## 7. Layer 3: the separate HardGate.evaluate battery

### 7.1 Construction

FACT (SOURCE): main.py always builds HardGate(now_fn, logger, freshness_max_sec from scoring.v3_freshness_max_sec, circuit_proximity_reject_enabled from system.entry_gate) (`main.py:3238-3247`) and passes it to the screener with scoring_config=app_config.scoring (`main.py:3294-3296`). FACT (TEXT): "Consumed by the screener only when scoring.v3_hardgate_mode != off (default off -> dormant); built always so shadow/enforce need no re-wiring." (`main.py:3238-3240`) (H-02).

### 7.2 What evaluate checks

FACT (SOURCE): HardGate.evaluate checks, in order: at-circuit -> AT_CIRCUIT; circuit proximity through the same circuit_proximity_reason -> CIRCUIT_PROXIMITY; age over freshness_max_sec -> SIGNAL_AGE (a missing triggered_at admits); liquidity, a no-op (`screening/hard_gate.py:139-171`). FACT (TEXT): "For the CURRENT LIVE flow it houses only the rules relocated out of the score: at-circuit, circuit-proximity, and freshness (signal_age, 60s)." and the V3 playbook gates "NEVER reject a live order in this step" (`screening/hard_gate.py:3-8`) (H-03).

### 7.3 When the screener calls it

FACT (SOURCE): the screener calls evaluate only in _screen_v3_enforce (`screening/secondary_screener.py:444`), taken only when the mode is "enforce" (`screening/secondary_screener.py:202`), and in _log_v3_shadow_compare (`screening/secondary_screener.py:538`), taken when the mode is "shadow" (`screening/secondary_screener.py:310`). The shadow compare sits after the score (step 5) and before the minimum score (step 6) (`screening/secondary_screener.py:305-314`) and only logs: "Pure side-effect; never changes the live result, never raises." (`screening/secondary_screener.py:534-536`); the log call is at `screening/secondary_screener.py:544-548` (H-04, S-03).

### 7.4 Configuration

FACT (CONFIG): v3_hardgate_mode "shadow" (`config/scoring_weights.yaml:39`), v3_gate_steps [circuit_check, signal_age] (`config/scoring_weights.yaml:40`), v3_freshness_max_sec 60.0 (`config/scoring_weights.yaml:41`), v3 thresholds 50, 75 and 56 (`config/scoring_weights.yaml:42-44`); its comment: "shadow = compute both, live follows OLD, log NEW old-vs-new." (`config/scoring_weights.yaml:34`) (H-05). UNKNOWN / NOT ESTABLISHED: which file feeds app_config.scoring (the mapping is in core/config_loader.py, not read; G-11); FACT (TEXT): the scorer's comment "All weights and thresholds come from ScoringConfig (scoring_weights.yaml)." (`screening/quality_scorer.py:4`).

### 7.5 The early circuit-proximity rule is not the HardGate battery

| | Layer 2, step 1b | Layer 3, HardGate.evaluate |
|---|---|---|
| Called from | screen() step 1b, on the default path (H-01) | the V3 shadow compare, or the enforce branch when the mode is "enforce" (H-04) |
| Rules | circuit proximity only, through hard_gate.circuit_proximity_reason | at-circuit, circuit proximity (the same function), freshness, liquidity no-op (H-03) |
| Effect on the repository configuration | rejects when enabled: REJECTED_CIRCUIT_PROXIMITY; enabled true (`config/system_config.yaml:661`) | none on the live result: computed and logged (H-04, H-05) |

INTERPRETATION (H-06): on the default path the HardGate class decides nothing, its verdict being computed and logged in shadow, while the circuit-proximity rule housed in hard_gate.py rejects when enabled. On the default path freshness decides through the executor's step 10 and the screener's step 7 (E-02, S-03), while at-circuit only lowers the score (step 9; E-01, E-04); neither goes through HardGate. "hard_gate.py is shadow-only" would therefore be wrong; the supported statement is that the HardGate.evaluate battery is shadow on the repository configuration (ChatGPT's Stage 1 reconciliation review, section 2.D; the scope decision record's explicit guards).

### 7.6 Other contents of hard_gate.py and other references to it

- FACT (SOURCE): hard_gate.py also defines gate_rr, gate_htf, gate_extreme, gate_confirm and gate_pullback (`screening/hard_gate.py:240`, `screening/hard_gate.py:297`, `screening/hard_gate.py:340`, `screening/hard_gate.py:369`, `screening/hard_gate.py:422`); the screener does not import them (`screening/secondary_screener.py:29-34`); FACT (TEXT): the module calls them "PLAYBOOK-shadow scope" (`screening/hard_gate.py:6-8`). Their callers: UNKNOWN / NOT ESTABLISHED (H-08).
- FACT (COUNT): hard_gate or HardGate also occurs in v3_chain/pb01_entry.py, v3_chain/pb01_runner.py, v3_chain/runner.py and scripts/v3_hardgate_parity_recompute.py (search Q01). Not read; what they call is UNKNOWN (H-07, G-09).
- FACT (TEXT) against the code path: hard_gate.py says its at-circuit rule "mirrors step_executor._step_9_circuit_check: hard reject" (`screening/hard_gate.py:145`). On the default path step 9 only scores 0.0 (E-01, E-02) and the screener does not reject on step 9 itself (INTERPRETATION, bounded to the screener file: S-04); whether the lost points alone end below the pass score depends on the other steps (E-04; A-008 section 12 records a ceiling of 65, HISTORICAL). Register G-14: recorded for D-002 and D-003; not a D-001 finding; not decided here.

## 8. The signal-age rules are distinct

| Rule | Where | Limit at the pin | Outcome | Missing trigger time | Status on the repository configuration | Recorded owner (HISTORICAL) | Evidence |
|---|---|---|---|---|---|---|---|
| A. Processor expiry | Layer 1, Step 1 | `self._signal_expiry_sec`; repository value: signal_queue.expiry_sec 600 (`config/system_config.yaml:127`) | EXPIRED, stored by the processor's handler; a warning at most once a minute | not stated in Stage 1 | can refuse | C-009 (M03; C-008 ST-06) | P-03 `signals/signal_processor.py:914-921`; P-06 `signals/signal_processor.py:1221-1231` |
| B. Executor step 10 and screener step 7 | the ten-step pipeline: step 10 inside run_all, the step 7 reject after the score (outside this map's Layer 2) | in the executor's step 10 (source lines; whether configurable is not shown): 1.0 up to 30 s, 0.5 up to 60 s, 0.0 beyond | a step 10 result of 0.0 -> REJECTED_SIGNAL_AGE at step 7 | step 10 gives 0.5 | can refuse: a signal older than 60 s by the screener's clock that reaches step 7 ends REJECTED_SIGNAL_AGE on the default path (a code path, not an observed event) | D-003, D-014, D-015 (BK-46; M16) | E-02 `screening/step_executor.py:406-423`; S-03 `screening/secondary_screener.py:339-354` |
| C. HardGate freshness | Layer 3 | freshness_max_sec from scoring.v3_freshness_max_sec; repository value 60.0 (`config/scoring_weights.yaml:41`) | SIGNAL_AGE in the battery's verdict | admitted | logged only (shadow compare) | J (A-008 section 12; M13) | H-03 `screening/hard_gate.py:139-171`; H-02; H-04; H-05 |

Why they must not be merged:

1. They sit at three different points: before the screener (A), inside the ten-step pipeline with the reject after the score (B), and in the shadow compare (C) (P-03, S-03, H-04).
2. Their limits differ at the pin: 600 s for A and 60.0 s for C (repository values); 30 s and 60 s bands for B (the executor's step 10).
3. Their outcomes differ: a processor rejection stored by the handler (A, P-06), a status the screener persists (B, S-03), and a verdict that "never changes the live result" (C, H-04).
4. Their handling of a missing trigger time differs where Stage 1 shows it: B scores 0.5 and C admits; for A it is not stated in Stage 1.
5. Their limits come from different places: the setting signal_queue.expiry_sec (A), the executor's step 10 (B; Stage 1 does not show whether its bands are configurable), and the setting scoring.v3_freshness_max_sec (C).
6. Their recorded owners differ: C-009 (A), D-003 with D-014 and D-015 (B), J (C).
7. FACT (TEXT): the processor's comment "defense-in-depth; receiver also checks" (`signals/signal_processor.py:914-921`) points to a further age check in the receiver (Section C in the Stage 1 flow). Stage 1 did not read it, and the Stage 1 matrix records no owner for it.

INTERPRETATION: the processor's limit (600 s, a repository value) is ten times the executor's 60 s band, so on the default path a signal that passes Layer 1's age check can still end REJECTED_SIGNAL_AGE at step 7 (P-03, E-02, S-03). Whether the two measure age from the same timestamp is not established in Stage 1. This is a reading of the code path, not an observed event.

## 9. Status at the pin, on the repository configuration

Status words: CALLED, CAN REFUSE - on the default main path at the pin and refuses or skips a signal when its condition holds; CALLED, LOGS ONLY - on the path, computes and logs, does not change the outcome; CALLED - on the path, with whether it can refuse stated in the row; NOT REACHED - in the source, but the pinned wiring or the repository configuration does not reach it; DORMANT (COMMENT) - a source comment calls the path dormant, not re-verified; UNRESOLVED - not established by the bounded evidence.

These statuses describe the pinned repository source and its repository configuration only. They do not prove deployed or runtime behaviour: no deployed configuration, host, log or database was read. Every status here that depends on a configuration value also assumes that config/system_config.yaml and config/scoring_weights.yaml feed app_config.system and app_config.scoring: the key names match (INTERPRETATION, Stage 1 evidence section 9), but the mapping is not established (G-11).

| Mechanism | Layer | Status | Evidence |
|---|---|---|---|
| Kill switch, entry | 1 | CALLED, CAN REFUSE | P-03, B-01 |
| Global entry window | 1 | CALLED, CAN REFUSE (window times not read: G-12) | P-03, B-02 |
| Signal age, processor expiry (600 s) | 1 | CALLED, CAN REFUSE | P-03; `config/system_config.yaml:127` |
| Shadow-inning guard | 1 | CALLED; whether it can refuse at the pin is UNRESOLVED (no tracker object -> it returns; is_tracking while disabled UNKNOWN) | P-07, G-13 |
| Rate-limiter pre-check (FIX-007) | outside the layers (processor) | NOT REACHED (no rate_limiter passed) | P-08 |
| Strategy lookup | 1b | CALLED, CAN REFUSE | P-04 |
| Strategy control | 1b | CALLED, CAN REFUSE (on the repository values only a disabled strategy) | P-04, B-03 |
| Strategy window | 1b | CALLED, CAN REFUSE | P-04, B-02 |
| Strategy circuit breaker | 1b | CALLED only when a governor is set; whether it is set and enabled is UNRESOLVED; fail-open on a database error | P-04, B-05, G-12 |
| MIS learned-blocklist pre-drop (step 0) | 2 | CALLED, LOGS ONLY (shadow) | S-03, S-06 |
| Quote availability (step 1) | 2 | CALLED, CAN REFUSE (as SKIPPED_QUOTE_UNAVAILABLE) | S-03, P-05 |
| V3 enforce branch | 2 / 3 | NOT REACHED (mode "shadow") | S-03, H-04, H-05 |
| Circuit proximity (step 1b) | 2 | CALLED, CAN REFUSE (enabled true) | S-03, H-01, H-06 |
| HardGate.evaluate battery | 3 | CALLED, LOGS ONLY (shadow compare); always built | H-02, H-04, H-05, H-06 |
| Playbook gates in hard_gate.py | outside the layers (module functions of hard_gate.py, not part of HardGate.evaluate) | UNRESOLVED (callers not established) | H-08 |
| hard_gate references in v3_chain and scripts | outside | UNRESOLVED (count only) | H-07, X-03 |

Neighbours outside the three layers, for the boundary only: the screener's step 7 age reject is CALLED, CAN REFUSE (E-02, S-03); the retest divert is NOT REACHED, the diverter not being built on the repository values (P-09); the entry gate's resume path is DORMANT (COMMENT) (AF-01); the entry throttle is CALLED after screening, sizing and reservation, just before placement (P-10, AF-03).

## 10. Ownership and hand-off matrix

### 10.1 Mechanisms on the map

Owners and records are as the Stage 1 boundary matrix gives them, rows M01 to M21 (HISTORICAL / RECORD-DERIVED); the Open column copies that matrix, including OSQ-01, whose status is discussed in section 12.1. D-001 treatment: MAP + CROSS-REFERENCE (this chapter shows position, order and status; the detail stays with the owner); MAP, OWNER UNRESOLVED (no owner is recorded; this chapter does not assign one); CROSS-REFERENCE ONLY (outside the three layers).

| ID | Mechanism | Layer | Stage 1 evidence | Record (Stage 1 matrix) | Recorded owner (HISTORICAL) | Open (Stage 1 matrix) | D-001 treatment |
|---|---|---|---|---|---|---|---|
| M01 | Kill switch, entry | 1 | P-03, B-01; again before placement (P-10); `signals/signal_processor.py:905-907` | C-008 ST-04, ST-25 | I topics (C-008) | OSQ-01, OSQ-02 | MAP + CROSS-REFERENCE |
| M02 | Global entry window | 1 | P-03, B-02; `signals/signal_processor.py:909-912` | C-008 ST-05; B-008 V2 inputs row 4 ("C-008, D-002") | A-003 (C-008); the window layer: C-008, D-002 (B-008 inputs) | OSQ-01, OSQ-03 | MAP + CROSS-REFERENCE |
| M03 | Signal age, 600 s (processor) | 1 | P-03; `signals/signal_processor.py:914-921`, `config/system_config.yaml:127` | C-008 ST-06; C-009 manual | C-009 | OSQ-01, OSQ-04 | MAP + CROSS-REFERENCE |
| M04 | Shadow-inning guard | 1 | P-03, P-07; `signals/signal_processor.py:923-928` | C-008 ST-07, [OQ-01], [U-01] | not named (C-008 [OQ-01]) | OSQ-05 | MAP, OWNER UNRESOLVED (OSQ-05) |
| M05 | Strategy lookup | 1b | P-04; `signals/signal_processor.py:933-953` | C-008 ST-08; C-010 | C-010 | OSQ-02 | MAP + CROSS-REFERENCE |
| M06 | Strategy control | 1b | P-04, B-03, B-04; `signals/signal_processor.py:955-972` | C-008 ST-09; A-008 | A-008 (C-008) | OSQ-02 | MAP + CROSS-REFERENCE |
| M07 | Strategy window | 1b | P-04, B-02; `signals/signal_processor.py:974-982` | C-008 ST-10 | A-003, A-008 (C-008) | OSQ-02 | MAP + CROSS-REFERENCE |
| M08 | Strategy circuit breaker | 1b | P-04, B-05; `signals/signal_processor.py:984-990` | C-008 ST-11 | F topics (C-008) | OSQ-02 | MAP + CROSS-REFERENCE |
| M09 | Rate-limiter pre-check (not wired) | outside the layers (processor) | P-08; `signals/signal_processor.py:415-424` | C-008 ST-01 | C-006 (C-008) | - | CROSS-REFERENCE ONLY (status in section 9) |
| M10 | MIS learned-blocklist pre-drop (step 0) | 2 | S-03, S-05, S-06; `screening/secondary_screener.py:142-148` | C-004 10.3; A-008 section 12 (shadow) | D-003, G-007 (C-004); the eligibility pre-check: D-002 (DR-032); VP-27: D-002 | OSQ-01, OSQ-03, OSQ-04 | MAP + CROSS-REFERENCE (D-003, as assigned) |
| M11 | Quote availability (step 1) | 2 | S-03; `screening/secondary_screener.py:167-183` | C-004 10.2 | D-003 (C-004) | OSQ-04 | MAP + CROSS-REFERENCE (D-003, as assigned) |
| M12 | Circuit proximity (step 1b) | 2 | S-03, H-01, H-06; `screening/secondary_screener.py:208-232`, `config/system_config.yaml:661` | the 21-Sep record, section 14 (as a hard-gate rule); A-008 section 12 (the V3 re-scale) | none found | OSQ-01, OSQ-03, OSQ-04 | MAP, OWNER UNRESOLVED (OSQ-03, OSQ-04) |
| M13 | HardGate.evaluate (V3 battery) | 3 | H-02 to H-05; `screening/hard_gate.py:139-171`, `config/scoring_weights.yaml:39` | A-008 section 12 ("shadow"; details to J) | J (A-008); D-002 by the agenda name (INTERPRETATION) | OSQ-03, OSQ-05 | MAP + CROSS-REFERENCE |
| M14 | The ten steps (executor) | after 2 | E-01, E-02; `screening/step_executor.py:141-154` | A-008 section 12; Q-34 (c) | D-003, D-017 (VP-03) | OSQ-04 | CROSS-REFERENCE ONLY |
| M15 | Score, tier, minimum score | after 2 | S-03 steps 5 and 6, E-03 | A-008 section 12 | D-003, D-017 (VP-03, VP-08) | - | CROSS-REFERENCE ONLY |
| M16 | Screener signal-age reject (step 7) | after 2 | S-03, E-02; `screening/secondary_screener.py:339-354` | C-009 manual; BK-46 | D-003, D-014, D-015 (BK-46) | OSQ-04 | CROSS-REFERENCE ONLY (compared in section 8) |
| M17 | Retest divert | after the screener | P-09, AF-02 | C-008 ST-15 | "D topics" (C-008); D-015 by the agenda name | OSQ-05 | CROSS-REFERENCE ONLY |
| M18 | Entry gate | after the screener | AF-01 | C-008 E2; B-009 D1; C-007 S-14; BK-24 | BK-24: C-008, D topics, B-009 | - | CROSS-REFERENCE ONLY |
| M19 | Entry throttle | after the screener | P-10, AF-03 | C-008 ST-27, [OQ-01] | open: D-016 or F-010 (C-008 [OQ-01]) | OSQ-05 | CROSS-REFERENCE ONLY (owner open) |
| M20 | V3 chain hook, allocator hook | after sizing | not read in Stage 1 | C-008 ST-18, ST-19 | J-010, J-012 (C-008) | - | CROSS-REFERENCE ONLY |
| M21 | EOD / PB01 path | separate route | X-01 to X-03 | C-011 manual | C-011 (the route) | OSQ-06 | CROSS-REFERENCE ONLY |

No row moves an owner. Where the records name more than one topic for a row, all are kept as recorded.

### 10.2 Hand-offs

| Hand-off | What passes | Who writes the outcome | Evidence |
|---|---|---|---|
| Layer 1 -> Layer 1b | the same method continues from "Step 1" to "Step 2" | a refusal in either: the processor's handler, REJECTED_ plus the check name | P-03, P-04, P-06; `signals/signal_processor.py:901-928`, `signals/signal_processor.py:930-990` |
| Layer 1b -> Layer 2 | "Step 3" calls screen(..., market_data=None) | the screener; the processor returns on "SKIPPED_" or not passed and does not double-write | P-05; `signals/signal_processor.py:997-1020` |
| Layer 2 -> the ten steps | thresholds, then StepExecutor.run_all | the screener | S-03, E-01; `screening/secondary_screener.py:234-263` |
| The screener -> Layer 3 | after the score, the shadow compare calls HardGate.evaluate | no status: the verdict is only logged | H-04; `screening/secondary_screener.py:305-314` |
| The screener -> after the screener | a passed result returns to the processor's Step 4 (prices) | outside this map; status writes after Step 4 were not read in Stage 1 | P-05; `signals/signal_processor.py:1025-1030` |

### 10.3 After the screener (boundary only)

FACT (SOURCE): the retest divert (`signals/signal_processor.py:1037-1056`) comes after price derivation and runs only if the diverter exists and is enabled; it is not built on the repository configuration (wait_for_retest_enabled false, `config/system_config.yaml:529`) (P-09). The entry throttle is built by the processor and admits right before placement, after the late kill-switch and shutdown checks (`signals/signal_processor.py:1384-1420`) (P-10). The entry gate's resume path is described in a comment as "DORMANT today: nothing calls EntryGate.add(), so only gate_state rows rehydrated at boot can reach it." (`signals/signal_processor.py:2236-2238`; HISTORICAL: B-009 D1; the caller search was not re-run) (AF-01). None of these is a check before the screener (P-09, AF-01, AF-03).

## 11. Cross-references

All as recorded in V2 records already cited in Stage 1 (HISTORICAL / RECORD-DERIVED):

- Section C: C-004 section 10 (10.2 quote availability to D-003; 10.3 MIS pre-drop to D-003 and G-007) (M10, M11); C-006 (the rate-limiter pre-check, by C-008's assignment) (M09); C-007 S-14 and C-008 BK-24 (the entry gate's add()) (M18); C-008, the processor's stage table (ST-01, ST-04 to ST-11, ST-15, ST-16 to ST-23 with the hooks ST-18 and ST-19, ST-25, ST-27; [OQ-01]; [U-01]; CX-02; E2) (M01 to M09, M17 to M20); C-009 (signal expiry) (M03, M16); C-010 (scanner-to-strategy mapping) (M05); C-011 (the EOD / PB01 route) (M21).
- Section A: A-003 (market windows) (M02, M07); A-008 (section 12 on the shadow layers, the V3 re-scale and a score ceiling of 65) (M06, M07, M10, M12 to M15).
- Section B: B-008 V2 design inputs, row 4 (the window layer: C-008, D-002) (M02); B-009 D1 (the entry gate's add() has no runtime caller) (M18).
- Section D: D-002 (named for the window check and the eligibility pre-check; DR-032; VP-27; by the agenda name for the HardGate battery, INTERPRETATION) (M02, M10, M13); whether D-002 covers the circuit-proximity rule is OSQ-03, open, and M12 has no recorded owner; D-003 (steps 0 and 1; the ten steps; the score; the step 7 age reject) (M10, M11, M14 to M16); D-004 (a market-data skip by the agenda name, INTERPRETATION) (M11); D-014 and D-015 (BK-46) (M16); D-015 (the retest divert, by the agenda name) (M17); D-016 or F-010 (the entry throttle, open) (M19); D-017 (VP-03, VP-08) (M14, M15).
- Section F: F topics (the strategy circuit breaker) (M08); F-010 (M19).
- Section I: I topics (the kill switch) (M01).
- Section J: J (the HardGate battery, per A-008) (M13); J-010 and J-012 (the V3 chain and allocator hooks) (M20).
- Other records: the 21-Sep record, section 14 (circuit proximity as a hard-gate rule; the battery's checks) (M12, M13; G-05); Q-34 (c) (M14).

## 12. Open questions and limits

### 12.1 Open scope questions (from the Stage 1 register; none is closed here)

- OSQ-01 [OPEN in the Stage 1 register] Which mechanism D-001 documents. INTERPRETATION: Rama's Option C selection (D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt) answers it for documentation scope; neither that record nor the Stage 2 prompt states OSQ-01's status, so its closure is for ChatGPT to confirm.
- OSQ-02 [OPEN] D-001 and C-008: the owners of each pre-check rule are as C-008 recorded (M01 to M09); no transfer proposed. Cross-reference bookmarks would be the mechanism if ChatGPT wants D-001 to cover them.
- OSQ-03 [OPEN] D-001 and D-002: the live circuit-proximity reject runs from hard_gate.py inside screen() (M12); the HardGate battery is shadow (M13); the window check and the eligibility pre-check already name D-002 (M02, M10).
- OSQ-04 [OPEN] D-001 and D-003: steps 0, 1 and 1b run before the ten steps but inside screen(); C-004 gave 0 and 1 to D-003 (M10, M11); the age rule repeats (M03, M16).
- OSQ-05 [OPEN] Other owners: the throttle is post-screening and pre-placement (M19); the shadow-inning guard is inside the pre-flight block with no named owner (M04); the retest divert, the V3 chain and the allocator come after (M17, M20).
- OSQ-06 [OPEN] EOD / PB01: no hand-off into the processor or the screener in main.py's PB01 wiring (X-02); hard_gate.py references in two PB01 files, count only (X-03, G-09). C-011's ownership of the route unchanged.
- OSQ-07 [DEFERRED TO THE D-001 CLOSE] VP-44: not ratified, rejected or moved.
- OSQ-08 [ANSWERED FOR STAGE 1] ChatGPT set the Stage 1 file list and read modes.

### 12.2 Evidence gaps (Stage 1 register)

- G-01 [OPEN, factual part narrowed]: what "Pre-filter" denotes; the code names its first block "Pre-flight checks", and "pre-filter" occurs in runtime code only in 4 lines of 3 files (W-01, W-02, B-06, G-10); the mechanism D-001 documents is a governance choice (OSQ-01).
- G-07 [PARTLY ADDRESSED]: re-read at the pin and consistent with the records: C-008 ST-04 to ST-12, ST-15, ST-27 and ST-01 (P-03 to P-10); C-004 10.2 and 10.3 (S-03, S-05); A-008 section 12 on the shadow layers (H-04, H-05, S-06); C-009 on the screener's age step (E-02). No conflict found in the ranges read; other cited statements stay HISTORICAL.
- G-09: what v3_chain/pb01_entry.py, v3_chain/pb01_runner.py, v3_chain/runner.py and scripts/v3_hardgate_parity_recompute.py use from hard_gate.py: UNKNOWN (count only, H-07).
- G-10: the "pre-filter" lines in core/config_snapshotter.py and core/state_store.py: not read (W-01).
- G-11: which configuration file feeds app_config.scoring and app_config.system: NOT ESTABLISHED (core/config_loader.py not read).
- G-12: the entry-window times' configuration and whether the strategy governor is enabled: NOT ESTABLISHED (A-003 and F subjects).
- G-13: is_tracking while the shadow tracker is disabled: OPEN, owner elsewhere (C-008 [U-01]).
- G-14: hard_gate.py's "hard reject" comment for step 9 against the default path, which only lowers the score: an observation for D-002 and D-003; not a D-001 finding; not decided.
- G-15: no test was read; the EntryGate.add() caller search was not re-run.
- G-03, G-04 and G-06 (unchanged): the README's statement that the system was live is repository text, not runtime state, and the README was not opened; whether chapter 06 "Filtering and trade decision" of the 18-Sep manual has been written since 24-Oct-2026 is UNKNOWN; no Group E record was opened.

### 12.3 Limits

- Repository configuration only; it does not prove deployed or runtime behaviour.
- Bounded reads: a read or search that found nothing is not proof that a behaviour never existed.
- The links marked [inferred] in section 3 were not read in Stage 1: the dispatcher's hand-off to _process_one (C-006, C-008) and sizing, the portfolio lock, risk, reservation and target (C-008 ST-16 to ST-23).
- A PB01 hand-off into the processor or the screener is NOT ESTABLISHED, not disproved (X-03).
- Assignments "by the agenda name" are INTERPRETATION, not recorded decisions.
- No V2 retain, modify or remove recommendation is made here.

## 13. Evidence and source index

### 13.1 Inputs to this draft

| File | SHA-256 |
|---|---|
| D-001_STAGE1_SOURCE_EVIDENCE_25-OCT-2026.txt | B3C0AEAC0827EC178B1282FA7EF5E2201095A0232F8D5A265D4FD282B0C9A514 |
| D-001_STAGE1_FILES_READ_LOG_25-OCT-2026.txt | A7081114226FB5D6C32AE5CCE3944C183D4E1794DD0947CB5BEA83FF5A554BBB |
| D-001_STAGE1_BOUNDARY_MATRIX_25-OCT-2026.txt | 9E9ED7F7633B80D9BA3B0267D5ADF598F6F99999271A039A1BDACD7D222D6BEE |
| D-001_STAGE1_GAP_AND_BOUNDARY_REGISTER_25-OCT-2026.txt | 6FDB2B97E31230FACC5AE0A4453199FA72A96F2CAE768403AE4E00CB9FB6FAB1 |
| D-001_STAGE1_CHANGE_NOTE_25-OCT-2026.txt | 5FE2EB6FC6E606692B3D65D5789F33BD2092C926079352B50336BABFCBD26866 |
| D-001_STAGE1_FINAL_CHECK_25-OCT-2026.txt | 8B0AF0FAA221DA7A1DF9F944F91B6DA05B9374EA66B87CD25DFE8F86BF38BABA |
| D-001_STAGE1_PROVENANCE_CLARIFICATION_REV1_25-OCT-2026.txt | 14F14810DDD1FC9836A1FCEF88DB2CCB5DD28003997ADC379E285BCCD4B8DA37 |
| D-001_CHATGPT_STAGE1_RECONCILIATION_REVIEW_25-OCT-2026.txt | 7C58BFE8D7288985C9A2516348BE9C87453E0E9D0CB7E6124A2EBFC455F37394 |
| D-001_RAMA_SCOPE_DECISION_RECORD_26-OCT-2026.txt | D2587788CE916C4F39BFE4268CD24388F198A896E33EB6765664BDC06BCFDEE8 |
| D-001_CLAUDE_PROMPT_STAGE2_MANUAL_DRAFT_26-OCT-2026.txt | 90E18370E494BA9A4C389F788573B5DE230E0009539F42A3E1BB06B5B4F1E871 |

### 13.2 Legacy source files cited (at the pin; read in Stage 1, not in Stage 2)

| File | Pinned blob | Stage 1 reads |
|---|---|---|
| capital/kill_switch.py | 3eb2ece050c3 | 042, 043, 044, 045 |
| capital/strategy_governor.py | d17193c569c1 | 052, 053 |
| config/scoring_weights.yaml | e9d6ede4c02d | 025 |
| config/system_config.yaml | bf742ae5c81c | 022, 023, 024 |
| core/market_windows.py | 25625e8cd109 | 046, 047 |
| core/mis_blocklist.py | 1f91fedfbdd4 | 054, 055 |
| main.py | 445fb0d8237a | 014 to 021, 051 |
| screening/entry_gate.py | 46b173171bdf | 037, 038 |
| screening/hard_gate.py | a8e7e2e49675 | 027, 028, 029 |
| screening/quality_scorer.py | b125e9ce3243 | 033, 034 |
| screening/retest_monitor.py | 26639d6ed5dd | 039, 040 |
| screening/secondary_screener.py | b6f8ff7223ad | 009 to 013, 026, 035, 036, 056 |
| screening/step_executor.py | d83b04860f0f | 030, 031, 032 |
| signals/signal_processor.py | 7fab0a02a302 | 001 to 008, 041, 057 to 060 |
| strategies/control.py | b6eecb480b83 | 048, 049 |
| strategies/loader.py | 99e83b192021 | 050 |

Named but not read: v3_chain/pb01_entry.py, v3_chain/pb01_runner.py, v3_chain/runner.py, scripts/v3_hardgate_parity_recompute.py, core/config_snapshotter.py, core/state_store.py (count only); core/config_loader.py and orders/shadow_tracker.py (not opened).

### 13.3 V2 records cited (HISTORICAL / RECORD-DERIVED; each already cited in Stage 1)

C-004 section 10; C-006; C-007 S-14; C-008 (the stage table and its [OQ-01], [U-01], CX-02, E2 and BK-24 notes); C-009 manual; C-010; C-011 manual; A-003; A-008 section 12; B-008 V2 design inputs, row 4; B-009 D1; DR-032; VP-03; VP-08; VP-27; VP-44 (deferred); BK-46; G-007; Q-34 (c); the 21-Sep record, section 14; the 18-Sep manual, chapter 06 (G-04); the master agenda's topic names.
