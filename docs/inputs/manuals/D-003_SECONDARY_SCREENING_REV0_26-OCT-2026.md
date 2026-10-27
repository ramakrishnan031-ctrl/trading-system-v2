# D-003 - SECONDARY SCREENING (LEGACY MANUAL) - DRAFT / NOT FROZEN

Status: DRAFT / NOT FROZEN. Draft 1, built 18:27 IST on 26-Oct-2026 for ChatGPT's independent review under D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt and D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt; uncommitted and not stored. Factual legacy documentation only: it records no V2 design decision and makes no claim about deployed or runtime behaviour.

Topic: D-003 Secondary screening (master agenda, section D, `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:294`). Legacy system status: DEAD / REFERENCE ONLY.

Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa), read on 26-Oct-2026 in D-003 Stage 1 through the gated masking filter: 22 count-only search runs over authorised files (one of them void) and 79 logged reads of the 39 authorised files at their pinned blobs. The evidence is the Stage 1 package (D-003_STAGE1_SOURCE_EVIDENCE_26-OCT-2026.txt and its six companions), accepted with documented deviations by D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt ("STAGE 1 ACCEPTED WITH DOCUMENTED DEVIATIONS", D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 3). This draft read, searched or counted no legacy file.

Purpose: a factual technical description of the legacy secondary screener - from the processor's call, through its inputs, its broker quote, its ten scored steps and its score, threshold and tier, to the outcome it persists itself and the immediate hand-offs - as the pinned source and the repository configuration define it. It is not a V2 design, and it does not own the final Trade / Reject verdict.

Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name from the legacy source or the host records is reproduced.

Masking-input audit (process provenance): on every Stage 1 filter run (102), the masking filter loaded the requested file or files in full and two legacy host records, HR-1 (a system map) and HR-2 (a deployment record), in full and value-blind as its host-token source, and attempted a third, HR-3 (a testing-snapshot manifest), which is absent; in one void count run (P03) it opened no requested file. HR-1 and HR-2 were not printed, not seen and not used as evidence. Each run is listed with its inputs in D-003_STAGE1_FILES_READ_LOG_26-OCT-2026.txt (sections 1 to 5).

## 1. Purpose, scope and boundaries

### 1.1 What this chapter documents

ChatGPT's scope decision OSQ-01: "D-003 documents the screener's own stage end-to-end, from invocation/input through steps, score/threshold/tier, internal outcome and status write, only as proven by source. It does not own the full downstream trade decision." (D-003_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 15). The reconciliation sets the drafting scope: "Scope: processor invocation through screener return and its own persisted outcome." (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 16). Within it this chapter covers:

- the architecture, the call chain and the input contract (section 3);
- the strategy fields and the configuration the screener reads (section 4);
- step 0, the MIS pre-drop, on the read side of the learned blocklist (section 5);
- step 1, the broker quote and the market data built from it (section 6);
- the D-002 mechanisms inside screen(), as boundary references (section 7);
- the ten steps (section 8) and the step executor, its shared pool, timeout and exceptions (section 9);
- the score arithmetic, the configured min_score against the effective pass threshold, the per-strategy thresholds and the tiers (section 10);
- the outcomes, statuses, persistence and error paths (section 11);
- the immediate downstream hand-offs (section 12) and the S&R, V3 and regime boundary (section 13);
- an illustrative end-to-end example derived only from the source rules (section 14);
- known defects, interpretations, unknowns and open questions (section 15) and cross-reference observations for D-001 and D-002 (section 16);
- the claim-to-evidence matrix and the residual-gap table (sections 18.3 and 15.3).

### 1.2 Scope basis (HISTORICAL / RECORD-DERIVED)

- The master agenda gives D-003 its title only, "D-003 Secondary screening" (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:294`), between D-002 Hard rejection gates (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:293`) and D-004 Market-data requirements (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:295`), with D-017 Score / ranking (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:308`), D-018 Final Trade / Reject decision (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:309`) and J-006 S&R use in screening (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:442`).
- ChatGPT's Stage 0 review and scope decisions OSQ-01 to OSQ-11 (D-003_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, lines 15 to 25).
- Rama's Stage 1 answer sheet: "Rama decisions requested now: NONE." (D-003_RAMA_ANSWER_SHEET_STAGE1_SCOPE_26-OCT-2026.txt, line 15).
- ChatGPT's Stage 1 reconciliation: acceptance with documented deviations (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 3) and the drafting decisions A to L (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, lines 16 to 27).
- This draft's prompt: the scope and its twelve content areas (D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt, lines 22 to 34), the fact discipline (D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt, line 37) and the mandatory qualifications (D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt, lines 39 to 53).

### 1.3 Boundaries

| Topic | What stays with it (as recorded) | D-003 relation in this chapter | Basis |
|---|---|---|---|
| D-001 Pre-filter (frozen) | the map of the processor's checks before screen() and of steps 0, 1 and 1b | cross-reference; its findings HISTORICAL; one interpretation refined (section 16) | DR-067 (`docs/governance/V2_DECISION_REGISTER.md:78`); OSQ-02 |
| D-002 Hard rejection gates (frozen) | the HardGate battery, the step-1b rule, the enforce branch, the shadow compare and the re-scale helpers | boundary references only (section 7); one observation (section 16) | DR-068 (`docs/governance/V2_DECISION_REGISTER.md:79`); OSQ-03 |
| D-004 Market-data requirements | broader market-data contracts and requirements | D-003 records the screener's observed inputs only (section 6) | agenda (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:295`); OSQ-04 |
| D-014 Breakout / breakdown conditions; D-015 Retest / reclaim | time rules other than the screener's signal-age score: the PB01 candidate expiry past entry_end and the retest resume without an age check (BK-46) | cross-reference (section 15.4) | agenda (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:305`; `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:306`); BK-46 (`docs/governance/V2_DESIGN_LOG.md:586`) |
| D-016 Entry timing or F-010 | the entry throttle (owner open, BK-56); the entry gate (BK-24) | hand-off references only (section 12) | agenda (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:307`); BK-56 (`docs/governance/V2_DESIGN_LOG.md:596`); BK-24 (`docs/governance/V2_DESIGN_LOG.md:564`) |
| D-017 Score / ranking | downstream ranking | outside; the screener's own score arithmetic is documented here | agenda (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:308`); OSQ-05 |
| D-018 Final Trade / Reject decision | the final verdict | outside; PASSED is internal (section 12) | agenda (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:309`); OSQ-05 |
| F-004 (sizing) | the use of the tier in sizing | hand-off reference (section 12) | C-008 ST-17 (`docs/agenda/c-008_signal-processing/C-008_manual.md:157`); A-005 (`docs/agenda/a-005_capital-model/A-005_manual.md:47`) |
| G-007 Broker rejection feedback | the blocklist writer and the rejection detection | cross-reference; D-003 documents the read side (section 5) | OSQ-06; C-004 (`docs/agenda/c-004_validation/C-004_manual.md:273`) |
| J (S&R, V3, regime) | the S&R, V3 and regime engines | boundary; none consumed by the screener (section 13) | OSQ-08; J-006 (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:442`) |
| C-004, C-007, C-008, C-009 | validation, persistence, processing, expiry | cross-references; their D-003 carry items addressed (sections 15, 16) | section 17 |

### 1.4 Not covered (cross-references only)

- the processor's checks before screen() (D-001's map; C-008);
- the HardGate battery, the step-1b rule and the V3 re-scale (D-002; the V3 engines are J's);
- everything after screen() returns - price derivation, the retest divert, sizing and reservation, the throttle, the entry gate, ranking, placement and the final verdict; section 12 names the hand-offs only;
- product resolution (A-007) and the blocklist writer and rejection detection (G-007);
- deployed configuration, runtime behaviour, broker responses and frequencies: NOT ESTABLISHED.

## 2. Source baseline, method and limitations

### 2.1 Baseline

- Legacy repository: trading-system-prod-snapshot (DEAD / REFERENCE ONLY), commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb; tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa (1,438 files and 125 folders, 1,563 entries), rebuilt from the saved listing and matched to the pinned commit's tree. The GitHub REST API request for the pinned tree returned HTTP 403 in Stage 0; ChatGPT accepted the rebuilt tree for path inventory only (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 13); Claude did not observe the API's truncated flag.
- V2 governance baseline: main at 85915267f38c0d653cf43d1be9425b87fb946566 (the D-002 freeze). Every V2 record cited here is at that commit.
- The local clone read in Stage 1: HEAD at the pin; 8,920 local objects before and after, nothing fetched; never-open blobs present locally: 0 (the read log's protected-path check).

### 2.2 Method

- Stage 0: an inventory by path name only (181 candidate paths; 14 checks PASS), accepted by ChatGPT with the scope decisions OSQ-01 to OSQ-11.
- Stage 1: a safety gate first (16:47:48 IST: the filter chain and every token source it loads, disclosed by role before any read); the read lock lifted at 16:50:28 as a step of its own; a gate test and a trace self-test; 22 count-only search runs over authorised files, with a positive control for every zero; 79 logged reads of the 39 authorised files at their pinned blobs, the five tests as text only; the lock re-engaged at 17:08:00. Every filter run was traced and its masking inputs logged; the Stage 1 final check passed 15 of 15.
- This draft: written from the seven Stage 1 files and the reconciliation only. No legacy file was opened, searched or counted for it, and nothing was run.

### 2.3 Stage 1 deviations (disclosed; not a source-evidence failure)

The reconciliation records these as process deviations and requires no repeat of Stage 1 (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, lines 10 to 13). Stage 1 was not perfectly compliant in its order of work; the source evidence itself is not in question.

- DV-01: the first search script stopped on a Python KeyError; it was corrected and resumed without re-running the searches already logged (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 12).
- DV-02: positive control P09 was added after the plan, before the first read.
- DV-03: three count-only search runs (P03, P03a, Q17) and one narrow read (079) came after earlier reads, although the prompt ordered searches first. All were limited to authorised files; P03 was void (its list held a relative path); P03a was the positive control for the S&R search; Q17 bounded the callers of screen(); read 079 explained Q17's match (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 10).
- DV-04: adapter read 044 printed unrelated lines after the Quote type; they are not cited or used (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 11).
- Scope note: five processor reads beyond the immediate call range (052, 060, 061, 077, 078), each narrow and logged with its purpose.

### 2.4 Evidence labels

| Label | Meaning in this chapter |
|---|---|
| FACT (SOURCE) | code lines read at the pin in Stage 1 |
| FACT (TEXT) | a comment or docstring: what the text says, not behaviour |
| FACT (CONFIG) | a repository configuration value, not a deployed host's value |
| FACT (TEST) | test source read as text, never run: corroboration, not runtime proof |
| FACT (COUNT) | a count-only search over named authorised files: file names and counts, no line printed |
| FACT (METADATA) | listing, gate or log facts |
| HISTORICAL | a V2 record or a governing input, cited at 8591526 or by file and line; never a legacy-source fact |
| INTERPRETATION | a reading or arithmetic that joins facts; the facts it rests on are cited |
| UNKNOWN / NOT ESTABLISHED | not established by the bounded Stage 1 evidence; a search that found nothing is not proof of absence beyond its files |
| KNOWN DEFECT | a defect recorded in a V2 record and confirmed in source (MC-56 only) |
| OPEN QUESTION | an owner, scope or wording question that the evidence does not settle (section 15.4) |

Status discipline: a comment is not behaviour; a function existing is not it being called; a call on a branch is not the branch being taken; a configured value is not a deployed one; a shadow or log-only path is not an effect on live decisions; a test assertion is not a runtime result.

### 2.5 Citation conventions

- A legacy citation `path:lines` names lines printed by a logged Stage 1 read of that file at its pinned blob. Each file's blob, as given in the evidence sheet's table of files and pinned blobs, is in section 18.2; each statement's citations, with the blob, are in the claim-to-evidence matrix (section 18.3).
- Each material statement carries an ID (MC-01 to MC-81) and names the Stage 1 evidence claims it rests on (the IN, SC, MD, SE, SQ, MS, ST, SR, HO, TS and NR IDs of D-003_STAGE1_SOURCE_EVIDENCE_26-OCT-2026.txt). Every legacy range in a statement lies within the citations of the evidence claims it names.
- V2 records are cited as `path:line` at 8591526; governing inputs by file name and line.
- Quotations are verbatim except that comment markers and line breaks are dropped, non-ASCII dashes, arrows and symbols are shown in ASCII, and "..." marks words left out.

### 2.6 Limitations

- Repository configuration only: it is not proof of deployed or runtime state.
- Bounded evidence: reads were ranges or targeted lines; searches covered named authorised files only; tests were read as text.
- Not opened (FACT (METADATA), the evidence sheet's NR-01 to NR-04): NR-01, the screener's lines 420 to 553 (the _circuit_proximity_reason wrapper, the enforce branch and the shadow compare: D-002's); NR-02, files outside the authorisation (the configuration loader, the product resolver, the order placer and the body of is_mis_block_rejection, the market-windows module, the allocation models and the processor's allocator lines, the callers of EntryGate.add()); NR-03, the 27 Group E records, the three strategy-direction registry paths and the never-open set, with the host records HR-1 and HR-2 loaded by the filter as token sources only; NR-04, count matches that the wiring reads show the screener does not consume.
- Not established: the residual gaps of section 15.3.

## 3. Architecture, call chain and input contract

### 3.1 Components

| Component | Defined or called at | Role at the pin | Built in main.py |
|---|---|---|---|
| SecondaryScreener | `screening/secondary_screener.py:3-5` | orchestrates steps 0 to 8 and persists every outcome (MC-01) | `main.py:3281-3298` |
| StepExecutor | `screening/step_executor.py:3-4` | runs the ten steps on its own pool (MC-41 to MC-44) | `main.py:3234-3237` |
| QualityScorer | `screening/quality_scorer.py:4` | score and tier from the step results (MC-48, MC-49) | `main.py:3230-3233` |
| HardGate | `screening/hard_gate.py:119` | D-002's battery, built always (section 7) | `main.py:3238-3247` |
| MisLearnedBlocklist | `core/mis_blocklist.py:85-100, 102-114, 116-123` | the learned MIS-block store read at step 0 (MC-15) | `main.py:3055-3059` |
| quote_fn | `broker/zerodha_adapter.py:1730-1755` | the adapter's get_quote, called at step 1 (MC-21) | `main.py:3285` |
| the caller | `signals/signal_processor.py:997-1004` | the signal processor, given the screener and the scorer (MC-02, MC-06) | `main.py:3586-3592` |

**MC-01** FACT (TEXT). The module header: "Orchestrates the 10-step screening pipeline: fetches market data, runs step_executor, scores via quality_scorer, applies strategy min_score override, persists result per P18." `screening/secondary_screener.py:3-5`. The class docstring: "SS1: Pure orchestration of step_executor + quality_scorer." and "SS12: Layer 5 (screening/). No broker import; quote_fn injected." `screening/secondary_screener.py:58-64`. Comment text, not behaviour. (Evidence: SC-01.)

**MC-02** FACT (SOURCE); FACT (TEXT). The processor's step 3 calls the screener once per signal: self._screener.screen(signal_id, symbol, scanner_name, trigger_price, triggered_at, direction=strategy_obj.direction, intent=strategy_obj.intent, strategy=strategy_obj, market_data=None) `signals/signal_processor.py:997-1004`, under the comment "Screener writes signal status (PASSED / REJECTED_<step>)." and "Processor does NOT double-write those statuses." `signals/signal_processor.py:992-996`; the call is timed and counted in the processor's statistics `signals/signal_processor.py:1005-1006, 1959-1970`. HISTORICAL: C-008 ST-12 records the same call (`docs/agenda/c-008_signal-processing/C-008_manual.md:152`). (Evidence: IN-01.)

**MC-03** FACT (COUNT); FACT (SOURCE). Within the 34 authorised files that are not tests, the processor's call `signals/signal_processor.py:998` is the only caller of screen(): count Q17 found one matching line in the processor and one in the screener, which read 079 shows to be the ScreeningResult docstring `screening/secondary_screener.py:44`. Callers outside the authorised files were not searched: UNKNOWN / NOT ESTABLISHED. (Evidence: IN-02.)

**MC-04** FACT (SOURCE); FACT (TEXT). screen() takes signal_id, symbol, scanner_name, trigger_price, triggered_at, direction, intent, strategy and an optional market_data (default None) and returns a ScreeningResult `screening/secondary_screener.py:123-134`. Its docstring: "SS4: Pipeline order: fetch data -> run steps -> check errors -> score -> apply min_score override -> final status." and "SS5: Persist result to state_store (P18). DB failures logged but do not prevent returning ScreeningResult." `screening/secondary_screener.py:135-141`. (Evidence: IN-03.)

**MC-05** FACT (SOURCE). Wiring in main.py: QualityScorer(weights=app_config.scoring, logger) `main.py:3230-3233`; StepExecutor(logger, market_open=market_windows.market_open), with no step_timeout_sec `main.py:3234-3237`, so the constructor default of 5.0 s applies `screening/step_executor.py:49-60`; a HardGate, built always `main.py:3238-3247`; then SecondaryScreener with the step executor, the scorer, state_store=store, quote_fn=broker_adapter.get_quote, a logger, circuit_proximity_reject_enabled from system.entry_gate, the MIS blocklist, mis_filter_enabled and mis_filter_shadow from system.mis_filter, a resolve_product closure over product_resolver.resolve(intent, "zerodha"), the hard gate, scoring_config=app_config.scoring and evidence=evidence_recorder `main.py:3281-3298`. (Evidence: IN-04.)

**MC-06** FACT (SOURCE); FACT (TEXT). The processor is given the screener, the scorer and the instrument cache, the last commented "IC: lot_size/sector lookup" `main.py:3586-3592`; its constructor comments the scorer as "kept for logging/metrics" `signals/signal_processor.py:144-145, 167-168`. The screener's constructor has no instrument-cache parameter `screening/secondary_screener.py:66-81`, and main.py passes it none `main.py:3281-3298`. (Evidence: IN-05.)

### 3.2 The input contract

| Argument | Value passed by the processor | Used by the screener for |
|---|---|---|
| signal_id | the signal tuple | the status write, logs and the evidence record (sections 5, 11) |
| symbol | the signal tuple | the blocklist lookup (step 0), the quote request (step 1), the signal dict |
| scanner_name | the signal tuple | the signal dict |
| trigger_price | the signal tuple | step 1b (D-002's rule), the signal dict, the evidence record |
| triggered_at | the signal tuple (MC-07) | the signal dict, read by signal_age (step 10); the evidence record |
| direction | strategy_obj.direction | the step-0 log; the signal dict, read by vwap_position and rsi_range |
| intent | strategy_obj.intent | product resolution at step 0; the signal dict |
| strategy | strategy_obj | the three thresholds (step 2) and min_score (step 6) |
| market_data | None | its absence triggers the quote request (step 1) |

Sources of the table: `signals/signal_processor.py:997-1004`, `screening/secondary_screener.py:123-134`, `screening/secondary_screener.py:142-165`, `screening/secondary_screener.py:167-196`, `screening/secondary_screener.py:208-232`, `screening/secondary_screener.py:234-239`, `screening/secondary_screener.py:241-249` and `screening/secondary_screener.py:554-581`.

**MC-07** FACT (SOURCE); INTERPRETATION; HISTORICAL. triggered_at: the processor takes it from the signal tuple `signals/signal_processor.py:883, 886`; its own expiry check measures now minus triggered_at (made IST-aware) against self._signal_expiry_sec `signals/signal_processor.py:914-918`; the same variable goes to screen() `signals/signal_processor.py:999`, which puts it in the signal dict `screening/secondary_screener.py:241-249` that step 10 reads against now_ist() `screening/step_executor.py:406-423`. Among the processor's lines naming triggered_at (read 077), none between the tuple and the call assigns it. INTERPRETATION: the processor's expiry (600 s, a repository value per D-001, HISTORICAL) and the executor's 30 s and 60 s bands measure from the same triggered_at value, at different instants: the processor's now, and now_ist() when step 10 runs. (Evidence: IN-07.)

**MC-08** FACT (SOURCE). Shutdown: the processor's stop() calls the screener's shutdown() `signals/signal_processor.py:342-344`, which shuts down the step executor `screening/secondary_screener.py:697-705`; the executor's shutdown() takes its pool lock and shuts the current pool down once `screening/step_executor.py:221-241`. (Evidence: IN-08.)

### 3.3 The path in source order

Statuses as the source writes them; the MC IDs give the evidence.

```
processor step 3 (C-008; D-001 map): screen(..., market_data=None) ............................. MC-02
SecondaryScreener.screen()
  step 0  MIS pre-drop (enabled, blocklist set) ... REJECTED_NOT_MIS_TRADABLE only when shadow is false   MC-13
  step 1  one quote via quote_fn ................... SKIPPED_QUOTE_UNAVAILABLE (no quote, or an exception) MC-20
          market_data built from the quote; snapshot copied                                         MC-23
  enforce branch (mode enforce, hard gate set) ..... D-002's path; not taken on the repository values MC-26
  step 1b circuit proximity (D-002's rule) ........ REJECTED_CIRCUIT_PROXIMITY when it gives a reason  MC-27
  steps 2-3 thresholds from the strategy; the signal dict                                          MC-30
  step 4  run_all: ten steps on the executor pool .. SKIPPED_EXECUTOR_ERROR (run_all raised)          MC-30
  step 4b any step raised .......................... REJECTED_STEP_ERROR                             MC-46
  step 5  score and tier ........................... SKIPPED_SCORER_ERROR (the scorer raised)          MC-51
          shadow compare (mode shadow, hard gate set): logs only                                    MC-28
  step 6  total below effective_min (60) ........... REJECTED_SCORE_<total>                          MC-51
  step 7  signal_age result 0.0 .................... REJECTED_SIGNAL_AGE                             MC-58
  step 8  PASSED, with score and tier                                                              MC-58
  every outcome persisted by _persist                                                              MC-62
processor: SKIPPED_ -> WARNING, return; not passed -> INFO, return; PASSED -> prices, divert, sizing MC-63, MC-66
```

## 4. Strategy inputs and configuration read by the screener

**MC-09** FACT (SOURCE); FACT (TEXT). The strategy schema's screening fields: min_score int 0, commented "0 = use global from scoring_weights.yaml", min_volume_surge 1.3, min_adr_pct 0.005 and max_spread_pct 0.005 `strategies/schema.py:115-119`; validators: min_adr_pct and max_spread_pct >= 0, min_score >= 0, min_volume_surge > 0 `strategies/schema.py:251, 255, 258, 260, 262, 265, 267, 269`, with no unit conversion in these lines. The loader validates each YAML (validate_strategy), in sorted order, and stores the result by name, with no change to the screening fields `strategies/loader.py:126-156`. (Evidence: SQ-06.)

**MC-10** FACT (CONFIG). All 16 strategy YAMLs set min_score 0, min_adr_pct 0.005 and max_spread_pct 0.005; min_volume_surge ranges from 1.2 to 2.0 (table below). These are repository values, not deployed ones. (Evidence: SQ-05.)

| Strategy YAML (lines) | min_volume_surge | min_score | min_adr_pct | max_spread_pct |
|---|---|---|---|---|
| `config/strategies/first_pullback_long.yaml:39-42` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/first_pullback_short.yaml:39-42` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/gap_fade_long.yaml:41-44` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/gap_fade_short.yaml:40-43` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/gap_go_long.yaml:41-44` | 2.0 | 0 | 0.005 | 0.005 |
| `config/strategies/gap_go_short.yaml:41-44` | 2.0 | 0 | 0.005 | 0.005 |
| `config/strategies/open_high_breakdown_short.yaml:40-43` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/open_low_breakout_long.yaml:39-42` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/pb01_breakout_retest.yaml:59-62` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/positional_momentum_long.yaml:40-43` | 1.5 | 0 | 0.005 | 0.005 |
| `config/strategies/positional_sector_rotation.yaml:40-43` | 1.3 | 0 | 0.005 | 0.005 |
| `config/strategies/positional_swing_long.yaml:40-43` | 1.2 | 0 | 0.005 | 0.005 |
| `config/strategies/range_breakout_long.yaml:40-43` | 1.8 | 0 | 0.005 | 0.005 |
| `config/strategies/range_breakout_short.yaml:39-42` | 1.8 | 0 | 0.005 | 0.005 |
| `config/strategies/vwap_bounce_long.yaml:39-42` | 1.5 | 0 | 0.005 | 0.005 |
| `config/strategies/vwap_rejection_short.yaml:39-42` | 1.5 | 0 | 0.005 | 0.005 |

**MC-11** FACT (CONFIG); FACT (TEXT). The scoring file: volume_surge 15, vwap_position 10, atr_filter 10, rsi_range 10, price_action 15, sector_strength 10, time_of_day 5, spread_check 5, circuit_check 10, signal_age 10, total 100 `config/scoring_weights.yaml:10-20`; min_pass_score 60 `config/scoring_weights.yaml:22`; high_score_threshold 80 and medium_score_threshold 65 `config/scoring_weights.yaml:28-29`, under the note that the scoring-side tier_multipliers block was deleted and "The thresholds below still drive tier ASSIGNMENT." `config/scoring_weights.yaml:24-27`. Its header: "NO weights are hardcoded in screening/*.py - this file is the single source." `config/scoring_weights.yaml:3-4`. The V3 keys at the end of the file are D-002's (section 7). (Evidence: SQ-01.)

**MC-12** INTERPRETATION; FACT (TEXT); HISTORICAL. Where app_config.scoring comes from: the scorer's header says "All weights and thresholds come from ScoringConfig (scoring_weights.yaml)." `screening/quality_scorer.py:4`; the screener's constructor types scoring_config as core.config_loader.ScoringConfig `screening/secondary_screener.py:79`; the keys it reads, v3_hardgate_mode, v3_gate_steps, v3_min_pass_score, v3_high_score_threshold, v3_medium_score_threshold and steps `screening/secondary_screener.py:96-106`, are keys of the scoring file `config/scoring_weights.yaml:10, 39-44`. The loader was not authorised and not read, so the link rests on these names and comments (G-10). HISTORICAL: A-009 records config/scoring_weights.yaml as loaded into AppConfig (`docs/agenda/a-009_configuration-model/A-009_manual.md:33`); D-001 left the feed NOT ESTABLISHED in its own reads (`docs/agenda/d-001_pre-filter/D-001_manual.md:347`). (Evidence: IN-06.)

## 5. Step 0: the MIS pre-drop (read side of the learned blocklist)

| Chain element | Step 0: MIS pre-drop |
|---|---|
| Source | `screening/secondary_screener.py:142-165` (step 0); `screening/secondary_screener.py:554-581` (the decision helper); `core/mis_blocklist.py:102-114` and `core/mis_blocklist.py:116-123` (the blocklist reads) |
| Input | symbol, intent, direction, signal_id; the switches mis_filter_enabled and mis_filter_shadow; the resolve_product closure; the blocklist (MC-16) |
| Logic | runs only when the filter is enabled and a blocklist is set; the intent must resolve to MIS; the symbol must be stored with a date fewer than ttl_days days old |
| Guard | a resolver exception, a product other than MIS or a corrupt date: not dropped (fail-open) |
| Output | active (shadow false): REJECTED_NOT_MIS_TRADABLE, score 0, tier LOW, rejected_step mis_tradable; shadow: a WARNING with the mode and a fall-through to step 1 |
| Persistence / hand-off | the active reject is persisted (_persist) and returned; the processor returns on a result not passed |
| Next consumer | step 1 (fall-through) or the processor (reject) |
| Repository values | enabled true, shadow true, ttl_days 1 `config/system_config.yaml:179-181` |

**MC-13** FACT (SOURCE). Step 0 runs first and only when mis_filter_enabled is true and a blocklist is set; when _is_mis_blocked_decision returns True and shadow is false, screen() persists and returns REJECTED_NOT_MIS_TRADABLE (passed false, score 0, tier LOW, rejected_step mis_tradable, empty step data and snapshot); in shadow it falls through `screening/secondary_screener.py:142-165`. (Evidence: SC-02.)

**MC-14** FACT (SOURCE). _is_mis_blocked_decision(symbol, intent, direction, signal_id): when resolve_product is set it resolves the intent, returns False on any exception (fail-open) and False when the product is not MIS; then False unless the blocklist's is_blocked(symbol); then it reads info(symbol), logs a WARNING naming REJECTED_NOT_MIS_TRADABLE with the mode, the last-blocked date, the days and the direction, and returns True in both modes `screening/secondary_screener.py:554-581`; the caller rejects only when shadow is false `screening/secondary_screener.py:148-165`. (Evidence: MS-01.)

**MC-15** FACT (SOURCE). The blocklist: record_block(symbol) stores today's ISO date under strip().upper() and persists - the writer `core/mis_blocklist.py:85-100`; is_blocked(symbol) is True when the symbol is stored and the days since its date are below ttl_days, and False on a corrupt date `core/mis_blocklist.py:102-114`; info(symbol) returns the date and the days for logging `core/mis_blocklist.py:116-123`. The module also defines is_mis_block_rejection `core/mis_blocklist.py:38`, whose body was not read (G-12; the writer side is G-007's). (Evidence: MS-02.)

**MC-16** FACT (SOURCE); FACT (TEXT). main.py builds one MisLearnedBlocklist with ttl_days from system.mis_filter.ttl_days and a path in the data-store folder (path not reproduced) `main.py:3055-3059`, under the comment that the order placer's 400 handler "RECORDS broker MIS-blocks into it; the SecondaryScreener READS it" and "Recording is always-on; the screener DROP is gated by mis_filter.enabled." `main.py:3051-3054`; it passes the blocklist, the two switches and the resolve_product closure to the screener `main.py:3290-3293`. The screener's comment: "Recording happens in the order_placer 400-handler; the screener only READS the learned set here." `screening/secondary_screener.py:111-113`. (Evidence: MS-03.)

**MC-17** FACT (CONFIG); FACT (TEXT); FACT (TEST). Repository values: mis_filter enabled true, shadow true, ttl_days 1 `config/system_config.yaml:179-181`. The comment above them records the switch "flipped false -> true in SHADOW ONLY" and forbids setting shadow false without the system owner's approval: "That is the enforcing change." `config/system_config.yaml:171-178`. A test asserts that the shipped configuration is enabled and shadow `tests/unit/test_mis_blocklist.py:238, 242-243`. Repository values, not deployed truth. (Evidence: MS-04.)

**MC-18** INTERPRETATION; HISTORICAL. On the repository values the pre-drop logs a would-drop and never rejects; with ttl_days 1 a symbol counts as blocked only on the calendar day of its last recorded block (days < 1, MC-15). Which intents resolve to MIS is A-007's: INTRADAY to MIS and DELIVERY to CNC (`docs/agenda/a-007_product-types/A-007_manual.md:23`), so the pre-drop could act only on INTRADAY-intent strategies, and a resolve error fails open (MC-14). The shadow mode is recorded by A-008 (`docs/agenda/a-008_strategy-model/A-008_manual.md:152`), A-004 (`docs/agenda/a-004_instruments/A-004_manual.md:76`) and C-004 (`docs/agenda/c-004_validation/C-004_manual.md:276`; L-11, `docs/agenda/c-004_validation/C-004_manual.md:474`); the writer, the order placer's rejection handler, was read by C-004 (`docs/agenda/c-004_validation/C-004_manual.md:275`) and is outside this chapter (G-007; OSQ-06). Deployed values are NOT ESTABLISHED. (Evidence: MS-05.)

**MC-19** FACT (TEST). The blocklist tests (text only, never run): with the filter active the screener hook rejects REJECTED_NOT_MIS_TRADABLE and logs `tests/unit/test_mis_blocklist.py:186, 190-191, 193`; a symbol not blocked passes `tests/unit/test_mis_blocklist.py:195, 199`; a non-MIS product is never dropped `tests/unit/test_mis_blocklist.py:202, 206`; shadow logs but does not reject `tests/unit/test_mis_blocklist.py:209, 213, 215`; a dormant filter does not consult the blocklist `tests/unit/test_mis_blocklist.py:217, 223`; TTL expiry allows a re-test `tests/unit/test_mis_blocklist.py:71, 77, 80`; a corrupt date fails open `tests/unit/test_mis_blocklist.py:92, 96`. (Evidence: MS-06.)

## 6. Step 1: quote acquisition and market-data construction

| Chain element | Step 1: quote and market data |
|---|---|
| Source | `screening/secondary_screener.py:167-196` (step 1); get_quote (MC-21, MC-22); _build_market_data (MC-23) |
| Input | the symbol; the injected quote_fn (the adapter's get_quote) |
| Logic | quote_fn([symbol]) and quotes.get(symbol); then _build_market_data(quote) |
| Guard | no quote: INFO and SKIPPED_QUOTE_UNAVAILABLE; an exception from quote_fn or _build_market_data: ERROR with traceback and the same status; the step runs only when market_data is None |
| Output | market_data: ltp, bid, ask, volume, circuit_state, the two bands, vwap, open, day_high, day_low; atr, rsi, sector, prev_close and avg_volume_20d None; market_data_snapshot, a copy |
| Persistence / hand-off | a SKIPPED_ result is persisted and returned; otherwise the data goes to the enforce branch or step 1b, then to the ten steps |
| Next consumer | step 1b (D-002), the step executor (section 8), the persisted snapshot (section 11) |

**MC-20** FACT (SOURCE); FACT (TEXT); HISTORICAL. Step 1, only when market_data is None: quotes = quote_fn([symbol]) and quote = quotes.get(symbol); no quote gives an INFO line and SKIPPED_QUOTE_UNAVAILABLE with an empty snapshot; an exception from quote_fn or _build_market_data gives an ERROR line with traceback and the same status; otherwise market_data = _build_market_data(quote) `screening/secondary_screener.py:167-196`. Its comment calls a missing quote "an EXPECTED, benign skip that fires ~hundreds of times a session" `screening/secondary_screener.py:173-176` (comment text; nothing measured here). HISTORICAL: B-009 gives counts of SKIPPED_QUOTE_UNAVAILABLE from its own source (`docs/agenda/b-009_intraday-operation/B-009_manual.md:279`) and records that the processor drops the candidate with no retry (`docs/agenda/b-009_intraday-operation/B-009_manual.md:156`). (Evidence: SC-03.)

**MC-21** FACT (SOURCE). The quote_fn wired in main.py is the adapter's get_quote `main.py:3285`. get_quote(symbols) returns a dict of Quote; in paper mode it calls an injected provider `broker/zerodha_adapter.py:1730-1755` (a code branch; which branch ran on a host is NOT ESTABLISHED); otherwise it takes a rate-limiter slot, builds "NSE:<symbol>" keys, calls self._kite.quote(*instrument_keys) and translates any exception `broker/zerodha_adapter.py:1757-1766`; each response key is mapped back by removing the prefix `broker/zerodha_adapter.py:1773-1774`. (Evidence: MD-01.)

**MC-22** FACT (SOURCE). Quote fields: symbol, last_price, bid, ask, volume, ts, and the optional vwap, open_price, day_high, day_low, upper_circuit and lower_circuit `broker/zerodha_adapter.py:208-222`. get_quote takes bid and ask from the first buy and sell depth levels, 0.0 when the depth or a side is empty `broker/zerodha_adapter.py:1775-1782`; last_price (default 0.0) and volume (default 0) `broker/zerodha_adapter.py:1785-1791`; vwap from average_price, open, high and low from ohlc, and the two circuit limits, each None when missing or zero `broker/zerodha_adapter.py:1792-1798`. (Evidence: MD-02.)

**MC-23** FACT (SOURCE); FACT (TEXT). _build_market_data sets circuit_state to "upper_circuit" when ltp >= upper, "lower_circuit" when ltp <= lower, else empty `screening/secondary_screener.py:386-395`; it copies ltp, bid, ask, volume, circuit_state, the raw bands, vwap, open, day_high and day_low from the quote `screening/secondary_screener.py:397-411`; atr, rsi, sector, prev_close and avg_volume_20d are always None, under the comment "Still not in Kite quote API; would need instruments cache" `screening/secondary_screener.py:412-418`. market_data_snapshot is a copy of the dict `screening/secondary_screener.py:198`. (Evidence: MD-03, SC-04.)

| market_data key | From the quote | On the processor's path |
|---|---|---|
| ltp | last_price | always set (0.0 when the quote has none) |
| bid, ask | first depth level | 0.0 when the depth or a side is empty |
| volume | volume | always set |
| vwap | average_price | None when missing or zero |
| open, day_high, day_low | ohlc | None when missing or zero (MC-39) |
| upper_circuit, lower_circuit; circuit_state | circuit limits | None when missing; circuit_state empty unless ltp is at a band |
| atr, rsi, sector, prev_close, avg_volume_20d | (none) | always None |

**MC-24** FACT (SOURCE); FACT (COUNT). On the processor's path (MC-02, market_data=None) every screened signal costs one quote request at screen time (MC-20), and the steps see only the quote-built dict, the strategy's three thresholds, the signal dict and the clock; the screener holds no instrument cache (MC-06). Counts Q06 (candle, ohlc, live_feed, historical, ltp, last_price) and Q07 (quote) over the four screener files matched 19 and 35 lines, all among the lines printed by the reads of those files (checked in Stage 1 against the saved read outputs), and none names a data source other than the quote. (Evidence: MD-04.)

**MC-25** INTERPRETATION; UNKNOWN / NOT ESTABLISHED; HISTORICAL. Quote-map case (C-004 L-04): the screener looks up quotes.get(symbol) with the symbol as received `screening/secondary_screener.py:170-171`, and get_quote keys its result by the response key minus the prefix `broker/zerodha_adapter.py:1773-1774`, with no case change in the lines read; a symbol whose case differs from the key the broker returns would therefore miss and end SKIPPED_QUOTE_UNAVAILABLE. Whether the broker returns the requested case is UNKNOWN (C-004 U-03). HISTORICAL: C-004 L-04 (`docs/agenda/c-004_validation/C-004_manual.md:467`) and U-03 (`docs/agenda/c-004_validation/C-004_manual.md:484`). (Evidence: MD-11.)

## 7. The D-002 mechanisms inside screen() (boundary references)

D-002 documents these mechanisms (frozen, DR-068); this chapter records only where they run, what screen() does with their result and what is not established.

**MC-26** FACT (SOURCE); FACT (CONFIG); HISTORICAL. After the snapshot, the V3 enforce branch, taken only when the mode is "enforce" and a hard gate is set, returns _screen_v3_enforce(...) `screening/secondary_screener.py:200-206`; its body, from line 435 (definitions seen in read 001, `screening/secondary_screener.py:420, 435, 530`), was not read (D-002 documents it: `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:210`). The repository mode is shadow `config/scoring_weights.yaml:39`, so on the repository configuration the branch is not taken; the deployed value is NOT ESTABLISHED. (Evidence: SC-04.)

**MC-27** FACT (SOURCE). Step 1b, the circuit-proximity reject: REJECTED_CIRCUIT_PROXIMITY, score 0, tier LOW, rejected_step circuit_proximity, persisted, before the ten steps `screening/secondary_screener.py:208-232`. Its rule is D-002's and its owner is open (BK-54, `docs/governance/V2_DESIGN_LOG.md:594`); its repository switch is recorded by D-002 (`docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:214`). (Evidence: SC-05.)

**MC-28** FACT (SOURCE); FACT (TEXT); UNKNOWN / NOT ESTABLISHED. The V3 shadow compare runs after the score, only in shadow mode with a hard gate set, and screen() uses nothing from it; its comment: "The LIVE decision below is UNCHANGED (follows OLD); this is a pure side-effect for the old-vs-new soak compare." `screening/secondary_screener.py:305-314`. Whether _log_v3_shadow_compare (defined at `screening/secondary_screener.py:530`) can raise into screen() is NOT ESTABLISHED: its body was not read and no try surrounds the call in the lines read (G-14). (Evidence: SC-09.)

**MC-29** FACT (SOURCE). The V3 hard-gate re-scale is the only V3 element in the screener: it imports circuit_proximity_reason, rescale_min_score, rescaled_total and tier_for from screening.hard_gate `screening/secondary_screener.py:29-34` and reads the V3 mode and thresholds from the scoring configuration `screening/secondary_screener.py:92-106`; the hard-gate module defines those helpers and HardGate `screening/hard_gate.py:53, 119, 139, 178, 197, 206`. Their internals are D-002's and the V3 engines J's. (Evidence: SR-02.)

## 8. The ten steps

**MC-30** FACT (SOURCE). Step 2 builds the thresholds from the strategy - min_volume_surge, min_adr_pct and max_spread_pct `screening/secondary_screener.py:234-239`; step 3 the signal dict - symbol, scanner_name, trigger_price, triggered_at, direction and intent `screening/secondary_screener.py:241-249`; step 4 calls run_all(signal_dict, market_data, thresholds) with no exclude_steps, so all ten steps run, and an exception from run_all gives SKIPPED_EXECUTOR_ERROR `screening/secondary_screener.py:251-263`. (Evidence: SC-06.)

The table gives, for each step, what it reads (and where that comes from on the processor's path), its logic and guard, what it returns and what it returns on that path. Every result goes to the scorer (section 10); signal_age's also to step 7 (MC-58); the step statuses go to the result and the evidence record (MC-62).

| Step (weight) | Reads (source on the processor's path) | Logic and guard | Returns | On the processor's path | Source |
|---|---|---|---|---|---|
| 1 volume_surge (15) | avg_volume_20d (None), volume (quote), min_volume_surge (strategy) | 0.0 when avg_volume_20d is missing or zero; else 1.0 if volume > avg_volume_20d x min_volume_surge (fallback 1.5) | 0.0, 1.0 | 0.0 always (dead) | `screening/step_executor.py:247-256` |
| 2 vwap_position (10) | ltp, vwap (average_price), direction | 0.0 without vwap or ltp; LONG: 1.0 if ltp > vwap; SHORT: 1.0 if ltp < vwap | 0.0, 1.0 | live | `screening/step_executor.py:258-269` |
| 3 atr_filter (10) | atr (None), ltp, min_adr_pct (strategy) | 0.0 without atr or ltp; else 1.0 if (atr / ltp) x 100 >= min_adr_pct (fallback 0.5) | 0.0, 1.0 | 0.0 always (dead) | `screening/step_executor.py:271-281` |
| 4 rsi_range (10) | rsi (None), direction | 0.5 when missing, not numeric or outside 0 to 100; LONG 1.0 inside 40 to 80, SHORT inside 20 to 60, else 0.0 | 0.0, 0.5, 1.0 | 0.5 always (pinned) | `screening/step_executor.py:283-316` |
| 5 price_action (15) | ltp; open, day_high, day_low (ohlc) | min(1.0, 2 x body / range), body = abs(ltp - open), range = day_high - day_low + 1e-9; a key absent defaults to ltp, a key present with None does not (MC-39) | up to 1.0 | live | `screening/step_executor.py:318-329` |
| 6 sector_strength (10) | sector (None) | docstring "Placeholder": 1.0 if a sector is provided, else 0.5 | 0.5, 1.0 | 0.5 always (pinned) | `screening/step_executor.py:331-336` |
| 7 time_of_day (5) | the clock (now_ist) and market_open | minutes since market_open, negative clamped to 0: below 15 gives 0.5, below 60 gives 1.0, below 180 gives 0.8, else 0.5 | 0.5, 0.8, 1.0 | live | `screening/step_executor.py:338-370` |
| 8 spread_check (5) | bid, ask (depth top), max_spread_pct (strategy) | 0.5 when bid or ask is None or the mid is not positive; else 1.0 if ((ask - bid) / mid) x 100 <= max_spread_pct (fallback 0.1), else 0.0 | 0.0, 0.5, 1.0 | live (units: MC-54) | `screening/step_executor.py:372-395` |
| 9 circuit_check (10) | circuit_state (from the circuit limits) | 0.0 for upper_circuit or lower_circuit, else 1.0 | 0.0, 1.0 | live | `screening/step_executor.py:397-404` |
| 10 signal_age (10) | triggered_at (signal dict), the clock | 0.5 without triggered_at; age up to 30 s gives 1.0, up to 60 s 0.5, beyond 0.0 | 0.0, 0.5, 1.0 | live | `screening/step_executor.py:406-423` |

**MC-31** FACT (SOURCE). volume_surge returns 0.0 when avg_volume_20d is missing or zero, otherwise 1.0 if volume > avg_volume_20d x min_volume_surge (fallback 1.5) `screening/step_executor.py:247-256`; atr_filter returns 0.0 without atr or ltp, otherwise 1.0 if (atr / ltp) x 100 >= min_adr_pct (fallback 0.5) `screening/step_executor.py:271-281`. (Evidence: MD-05.)

**MC-32** FACT (SOURCE). vwap_position returns 0.0 without vwap or ltp, otherwise 1.0 when ltp > vwap for LONG or ltp < vwap for SHORT `screening/step_executor.py:258-269`; rsi_range returns 0.5 when rsi is missing, not numeric or outside 0 to 100, otherwise 1.0 inside 40 to 80 (LONG) or 20 to 60 (SHORT) and 0.0 outside `screening/step_executor.py:283-316`. (Evidence: MD-05.)

**MC-33** FACT (SOURCE); FACT (TEXT). price_action returns min(1.0, 2 x body / range) from ltp, open, day_high and day_low, each defaulting to ltp only when its key is absent `screening/step_executor.py:318-329`; sector_strength's docstring: "Placeholder: 1.0 if sector provided, 0.5 if missing." `screening/step_executor.py:331-336`. (Evidence: MD-05.)

**MC-34** FACT (SOURCE); FACT (TEXT); FACT (CONFIG); HISTORICAL. time_of_day counts minutes since market_open (negative clamped to 0): below 15 gives 0.5, below 60 gives 1.0, below 180 gives 0.8, else 0.5 `screening/step_executor.py:338-370`. market_open: main.py passes market_windows.market_open `main.py:3236`; the executor's comment says "Production code MUST inject market_open from SystemConfig.trading_hours.market_open" and keeps 09:15 as a fallback `screening/step_executor.py:30-36`; the repository value is 09:15 `config/system_config.yaml:103`. HISTORICAL: B-008 records that main.py builds MarketWindows from the trading_hours values (`docs/agenda/b-008_market-open/B-008_manual.md:26`); the market-windows module was not read (G-13). (Evidence: MD-05, MD-10.)

**MC-35** FACT (SOURCE); INTERPRETATION; UNKNOWN / NOT ESTABLISHED. spread_check returns 0.5 when bid or ask is None or the mid is not positive, otherwise 1.0 when ((ask - bid) / mid) x 100 <= max_spread_pct (fallback 0.1), else 0.0 `screening/step_executor.py:372-395`. INTERPRETATION: on the processor's path bid and ask are never None, since get_quote sets 0.0 for an empty depth or side (MC-22); a quote without depth gives 0.0 and 0.0, so the mid is 0 and the step returns 0.5. By the formula as recorded, an empty buy side (bid 0.0) gives a spread of 200 percent of the mid and 0.0; an empty sell side (ask 0.0) gives -200 percent, which the comparison as recorded would score 1.0. The Stage 1 evidence states only "0.0 when one side is empty" and records no guard for the second case; whether the step's code has one is NOT ESTABLISHED in this draft (no further read is authorised). (Evidence: MD-05, MD-02, MD-09.)

**MC-36** FACT (SOURCE); FACT (TEXT). circuit_check returns 0.0 for upper_circuit or lower_circuit and 1.0 otherwise `screening/step_executor.py:397-404` - a score, though its docstring says "Reject if upper or lower circuit" (BK-58); signal_age returns 0.5 without triggered_at, otherwise 1.0 up to 30 s, 0.5 up to 60 s and 0.0 beyond `screening/step_executor.py:406-423`. (Evidence: MD-05.)

**MC-37** FACT (SOURCE); INTERPRETATION; HISTORICAL. Dead and pinned steps (Q-34 (c), `docs/governance/V2_DESIGN_LOG.md:198`). On the processor's path avg_volume_20d, atr, rsi and sector are always None (MC-23), so under normal step returns volume_surge returns 0.0 `screening/step_executor.py:251-253`, atr_filter 0.0 `screening/step_executor.py:275-278`, rsi_range 0.5 `screening/step_executor.py:296-298` and sector_strength 0.5 `screening/step_executor.py:335-336` on every signal: 25 of the 100 weight points dead at 0 and 20 pinned at half. The scorer's comment states the same ceiling: "25/100 pts dead-at-0 + 20 pinned-at-half make >65 algebraically impossible today" `screening/quality_scorer.py:16-19`; A-008 recorded it without naming the steps (`docs/agenda/a-008_strategy-model/A-008_manual.md:149`). The bound and its timeout caveat are in MC-55. (Evidence: MD-06.)

**MC-38** FACT (SOURCE). The live steps on that path depend on the quote and the clock only: vwap_position on vwap (average_price), price_action on the day's open, high and low (ohlc), spread_check on the top bid and ask, circuit_check on the circuit limits through circuit_state, time_of_day on the clock and market_open, signal_age on triggered_at and the clock (MC-22, MC-23, the table above). (Evidence: MD-07.)

**MC-39** INTERPRETATION; UNKNOWN / NOT ESTABLISHED; FACT (TEST). A quote without OHLC (G-16; INTERPRETATION / UNKNOWN FREQUENCY): the screener always sets the keys open, day_high and day_low `screening/secondary_screener.py:409-411`, and get_quote sets them None when ohlc gives none or zero `broker/zerodha_adapter.py:1793-1795`; md.get("open", ltp) then returns None, not ltp, and the body arithmetic would raise `screening/step_executor.py:322-326`; the executor turns a raised step into 0.0, status ERROR and an error step `screening/step_executor.py:182-195`, and screen() turns any error step into REJECTED_STEP_ERROR `screening/secondary_screener.py:265-286`. This is a source-level path, not an observed event: whether and how often it happened in production is UNKNOWN. The executor's missing-keys test passes an empty dict, keys absent rather than present with None `tests/unit/test_step_executor.py:534-538`. (Evidence: MD-08.)

**MC-40** FACT (TEST); INTERPRETATION. The screener tests build market data with atr, rsi, sector and avg_volume_20d set `tests/unit/test_secondary_screener.py:87-88, 91, 96` and pass it in as market_data `tests/unit/test_secondary_screener.py:139, 149, 176`. INTERPRETATION: they exercise the step arithmetic with inputs the processor's path never supplies (MC-23, MC-37). (Evidence: TS-01.)

## 9. The step executor

| Chain element | The step executor |
|---|---|
| Source | `screening/step_executor.py:118-154, 158-195, 197-206, 213-219` (run_all); `screening/step_executor.py:62-76, 78-112` (the pool and its rotation) |
| Input | the signal dict, market_data, the thresholds; exclude_steps (None on the default path) |
| Logic | each step submitted to the shared single-worker pool and awaited with a per-step timeout |
| Guard | timeout: 0.5, TIMEOUT, pool replaced; exception: 0.0, ERROR, error_steps; run_all never raises by its header |
| Output | step_results, step_statuses, rejected_at, error_steps, latencies_ms |
| Persistence / hand-off | none of its own; the screener reads error_steps (step 4b) and step_results (steps 5 and 7) and stores statuses and latencies in the result |
| Next consumer | the screener (MC-46) and the scorer (MC-48) |

**MC-41** FACT (TEXT). Header: "Execute all 10 screening steps. Each step is a pure function returning a raw score (0.0-1.0). Exceptions are captured per step; run_all() never raises." `screening/step_executor.py:3-4`. Class docstring, among others: "SE7: ALL 10 steps always run (no short-circuit)." and "SE8: Missing market_data keys use per-step defaults (0.0 or 0.5)." `screening/step_executor.py:40-47`. (Evidence: SE-01.)

**MC-42** FACT (SOURCE); FACT (TEXT). run_all(signal, market_data, thresholds, exclude_steps=None) runs, in this order, volume_surge, vwap_position, atr_filter, rsi_range, price_action, sector_strength, time_of_day, spread_check, circuit_check and signal_age, leaving out any step named in exclude_steps `screening/step_executor.py:118-154`; the direction is the signal's, upper-cased, default LONG `screening/step_executor.py:132`. Its docstring: "In v3 mode the screener passes {circuit_check, signal_age} so only the 8 scored steps run." `screening/step_executor.py:129-130`. (Evidence: SE-02.)

**MC-43** FACT (SOURCE). Each step is submitted to the pool and awaited with future.result(timeout=self._step_timeout_sec) `screening/step_executor.py:158-167`. A timeout scores 0.5 ("neutral score"), status TIMEOUT, logs a WARNING and rotates the pool `screening/step_executor.py:168-181`; an exception scores 0.0, status ERROR, logs an ERROR with traceback, adds the step to error_steps and, if first, sets rejected_at `screening/step_executor.py:182-195`; a returned 0.0 is status REJECTED (rejected_at if first), any other value PASSED `screening/step_executor.py:197-206`. The result carries step_results, step_statuses, rejected_at, error_steps and latencies_ms `screening/step_executor.py:20-27, 213-219`. (Evidence: SE-03.)

**MC-44** FACT (SOURCE); FACT (TEXT). The pool: one ThreadPoolExecutor with max_workers=1 (thread prefix step-exec) per StepExecutor, created once and reused across calls `screening/step_executor.py:62-76`; the comment: "run_all() may be entered by several signal workers at once." `screening/step_executor.py:67-68`. After a timeout, _rotate_executor swaps in a fresh pool and abandons the stale one, because "future.result(timeout=...) abandons the WAIT, not the WORK" `screening/step_executor.py:78-112`. The timeout is the constructor's step_timeout_sec, default 5.0 `screening/step_executor.py:49-60`; main.py passes none and builds one StepExecutor for the one screener `main.py:3234-3237, 3281-3283`. (Evidence: SE-04.)

**MC-45** INTERPRETATION; FACT (TEXT); HISTORICAL. With one shared single-worker pool, the steps of concurrent signals run one at a time, and a step's 5 s wait includes time queued behind other signals' steps. HISTORICAL: the processor runs a pool of workers, repository worker_count 5 (`docs/agenda/c-006_queue/C-006_manual.md:181`). The comment describes the queueing after a hung step: "every remaining step of THIS signal, and every step of every LATER signal, queues behind it and times out in turn" `screening/step_executor.py:83-86`. Whether contention or timeouts occurred is NOT ESTABLISHED (G-19). The quote fetch of step 1 runs outside the pool, in the calling worker, with no screener-level timeout (MC-20). (Evidence: SE-05.)

**MC-46** FACT (SOURCE). Step 4b: when any step raised, the result is REJECTED_STEP_ERROR, score 0, tier LOW, rejected_step the comma-joined names of the error steps, logged as a WARNING `screening/secondary_screener.py:265-286`. A step that returned 0.0 does not stop screen(): screen() branches only on exec_result.error_steps (step 4b) and on the signal_age result (step 7), and does not use exec_result.rejected_at `screening/secondary_screener.py:265-374`. (Evidence: SC-07.)

**MC-47** FACT (TEST). The executor tests (text only) pass thresholds 1.5, 0.5 and 0.1 `tests/unit/test_step_executor.py:53, 55-57` and include signal-age cases within 30 s, between 30 and 60 s and over 90 s `tests/unit/test_step_executor.py:458, 466, 474`, the error path `tests/unit/test_step_executor.py:486, 501, 517` and rejected_at `tests/unit/test_step_executor.py:541, 552`. (Evidence: TS-05.)

## 10. Score, thresholds and tier

| Chain element | Score, threshold and tier |
|---|---|
| Source | `screening/quality_scorer.py:79-103, 105-120, 122-134` (score and tier); `screening/secondary_screener.py:288-374` (steps 5 to 8); the scoring file (MC-11) |
| Input | step_results (ten raw scores on the default path); the weights and thresholds of the scoring configuration; strategy.min_score |
| Logic / formula | total = min(100, int(round(sum(raw x weight) / sum(weights present) x 100))); tier from 80 and 65; effective_min = strategy.min_score when above 0, else min_pass_score |
| Guard | a missing step leaves the denominator; no step present gives 0; the scorer raising gives SKIPPED_SCORER_ERROR |
| Output | total_score, tier, passed (not read by screen()), step_scores, missing_steps |
| Persistence / hand-off | score and tier written with the status (MC-62); the tier handed on (MC-67) |
| Next consumer | step 6 (the threshold), step 7, the persisted row, the processor's sizing and retest divert |

**MC-48** FACT (SOURCE). The scorer takes its step names from the configuration model's fields `screening/quality_scorer.py:56-69`. For each step: missing from step_results, it is logged, listed in missing_steps, reported as 0 and left out of the denominator; present, it adds raw x weight to the achieved total and its weight to the denominator `screening/quality_scorer.py:79-103`. total = min(100, int(round(achieved / present x 100))), or 0 when no step is present `screening/quality_scorer.py:105-120`. (Evidence: SQ-02.)

**MC-49** FACT (SOURCE); FACT (TEXT). Tier: HIGH when the total >= high_score_threshold, MEDIUM when >= medium_score_threshold, else LOW; passed = total >= min_pass_score `screening/quality_scorer.py:122-134`. ScoreResult carries total_score, tier, passed, step_scores, missing_steps, min_pass_score and tier_thresholds `screening/quality_scorer.py:25-34, 142-150`. Docstring QS7: "scorer returns tier string only; sizer applies size multiplier" `screening/quality_scorer.py:44`. (Evidence: SQ-03.)

**MC-50** FACT (SOURCE); FACT (TEXT); INTERPRETATION. Missing steps: on the default path run_all records every step - a timeout as 0.5, an exception as 0.0 (MC-43) - so no step is missing and the denominator is 100; the missing-step branch applies only to steps excluded or absent. The FIX-042 docstring's example, "If circuit_check times out, recalculate score as (achieved / weights_present) * 100" `screening/quality_scorer.py:75-77`, describes a case the executor does not produce, since a timed-out step is present with 0.5 `screening/step_executor.py:168-181`. (Evidence: SQ-04.)

**MC-51** FACT (SOURCE). Step 5: score_result = self._scorer.score(exec_result.step_results); an exception gives SKIPPED_SCORER_ERROR; total_score and tier come from the score result `screening/secondary_screener.py:288-303`. Step 6: effective_min is strategy.min_score when it is above 0, otherwise score_result.min_pass_score; a total below it gives REJECTED_SCORE_<total_score> (passed false, the score and tier kept, rejected_step None), persisted with eligible_score=effective_min `screening/secondary_screener.py:316-337`. screen() does not read score_result.passed `screening/secondary_screener.py:288-374`. (Evidence: SC-08, SC-10.)

**MC-52** FACT (CONFIG); INTERPRETATION; HISTORICAL. Configured min_score against the effective pass threshold: min_score is configured as 0 in all 16 strategy YAMLs (MC-10), meaning "use the global"; the screener's effective pass threshold on the processor's path is therefore min_pass_score, 60 (MC-11, MC-51), the same test as the scorer's passed `screening/quality_scorer.py:134`. The two values are distinct: 0 is the configured override, 60 the threshold applied. A strategy min_score above 0 would have replaced 60. HISTORICAL: A-008 recorded the same (`docs/agenda/a-008_strategy-model/A-008_manual.md:147`). (Evidence: SQ-07, SQ-05.)

**MC-53** INTERPRETATION; HISTORICAL. On the processor's path the per-strategy thresholds did not tell strategies apart: volume_surge and atr_filter return 0.0 before min_volume_surge and min_adr_pct are compared `screening/step_executor.py:251-255, 275-280`, so those two are not effectively compared; max_spread_pct is 0.005 in all 16 (MC-10). With min_score 0 everywhere (MC-52), every strategy met the same screen apart from its direction, which vwap_position uses (rsi_range is pinned). HISTORICAL: A-008 recorded these three thresholds as "the only strategy-specific part of screening" (`docs/agenda/a-008_strategy-model/A-008_manual.md:148`); see MC-80. (Evidence: SQ-11.)

**MC-54** FACT (SOURCE); FACT (CONFIG); FACT (TEST); INTERPRETATION; UNKNOWN / NOT ESTABLISHED. Spread units (G-15). The step computes spread_pct = ((ask - bid) / mid) x 100, percent arithmetic, and scores 1.0 when spread_pct <= max_spread_pct, fallback 0.1 `screening/step_executor.py:393-395`; the screener passes the strategy's value `screening/secondary_screener.py:234-239`, 0.005 in all 16 strategy YAMLs (MC-10) and the schema default `strategies/schema.py:116-119`. The executor's tests pass 0.1 with comments in percent, "spread_pct=(0.2/2510)*100=0.00796% < 0.1" `tests/unit/test_step_executor.py:53, 55-57, 357-363`, and the screener tests' mock strategy uses max_spread_pct=0.1 and min_adr_pct=0.5 `tests/unit/test_secondary_screener.py:53, 56-59`. INTERPRETATION / UNKNOWN INTENDED UNIT: compared as the code compares it, 0.005 admits a spread of at most 0.005 percent of the mid price (one part in 20,000; a bid equal to the ask scores 1.0, `tests/unit/test_step_executor.py:403-411`), against 0.1 percent in the tests. Whether 0.005 was meant as a fraction rather than a percentage is UNKNOWN; this chapter does not read 0.005 as any other value, does not assert an intent and does not call it a defect. The effect on live scores is NOT ESTABLISHED (no quote data read). min_adr_pct has the same pattern, but on the processor's path atr is None and the step returns 0.0 before comparing (MC-53). (Evidence: MD-09.)

**MC-55** INTERPRETATION. The normal-return ceiling and its timeout caveat (Claude's arithmetic on the cited source; no legacy code run). On the default path all ten steps are present, so the denominator is 100 (MC-50). Under normal step returns the total cannot exceed 65: the two normally-dead steps, volume_surge (15) and atr_filter (10), add nothing, and the two pinned steps, rsi_range and sector_strength (10 each), add 5 each (MC-37), so 100 - 25 - 10 = 65. This ceiling assumes normal returns: a step that times out scores 0.5 `screening/step_executor.py:168-181`, so if both normally-dead steps timed out the theoretical ceiling would be 65 + 7.5 + 5 = 77.5, still below the HIGH threshold of 80 `config/scoring_weights.yaml:28-29`; no step returns more than 1.0 in the lines read (MC-31 to MC-36), so no combination of returns and timeouts on this path reaches 80. 65 is therefore not an unconditional ceiling for every execution path. Whether any step ever timed out is NOT ESTABLISHED. The reconciliation states the same bounds (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 24). (Evidence: MD-06, SQ-01, SE-03.)

**MC-56** KNOWN DEFECT; FACT (TEXT); HISTORICAL. The HIGH tier was unreachable on the default path: the scorer's comment marks HIGH "unreachable today (IA-P2-03: threshold 80 > ceiling 65)" `screening/quality_scorer.py:126-127` and keeps a tripwire counter for totals above 65 `screening/quality_scorer.py:16-19, 139-140`; A-008 recorded "One common screener whose score could not exceed 65: HIGH tier unreachable" (`docs/agenda/a-008_strategy-model/A-008_manual.md:269`). Stage 1 confirms the mechanism at the pin (MC-37, MC-55): under normal returns a pass scores 60 to 65 - LOW below 65, MEDIUM at 65 - and even the timeout bound of 77.5 stays below 80. (Evidence: SQ-09, MD-06.)

**MC-57** INTERPRETATION. What a pass needed on the processor's path under normal returns (Claude's arithmetic on the formula read; no legacy code run). With the best total 65 and a pass at 60, the live steps together could lose about 5 points (a loss between 5 and 6 depends on rounding `screening/quality_scorer.py:105-108`). Losses: vwap_position, circuit_check or signal_age at 0.0 cost 10 each (no pass); signal_age 0.5 costs 5; time_of_day 0.8 costs 1 and 0.5 costs 2.5; spread_check 0.5 costs 2.5 and 0.0 costs 5; price_action p costs 15 x (1 - p). The table below gives examples; MEDIUM (65) needed every live step at 1.0. With the timeout caveat of MC-55 these bounds could rise by at most 12.5 points. (Evidence: SQ-08.)

| signal_age | time after the open | spread_check | points (of 100) | outcome |
|---|---|---|---|---|
| up to 30 s | 15 to 60 min | 1.0 | 65 | PASSED, MEDIUM |
| up to 30 s | 15 to 60 min | 0.5 | 62.5 | PASSED, LOW |
| up to 30 s | 15 to 60 min | 0.0 | 60 | PASSED, LOW |
| up to 30 s | 60 to 180 min | 1.0 | 64 | PASSED, LOW |
| up to 30 s | 60 to 180 min | 0.5 | 61.5 | PASSED, LOW |
| up to 30 s | 60 to 180 min | 0.0 | 59 | REJECTED_SCORE_59 |
| up to 30 s | under 15, or 180 and over | 1.0 | 62.5 | PASSED, LOW |
| up to 30 s | under 15, or 180 and over | 0.5 | 60 | PASSED, LOW |
| 30 to 60 s | 15 to 60 min | 1.0 | 60 | PASSED, LOW |

Rows: vwap_position, price_action and circuit_check at 1.0; only the combinations at or above 59 points are listed. Every other combination totals below 59 and ends REJECTED_SCORE_<n>. Points are the achieved weight points before the scorer turns them into an integer total (MC-48): 62.5 and 61.5 both become 62 under Python's round-half-to-even, and no outcome in the table changes.

**MC-58** FACT (SOURCE); INTERPRETATION. Step 7: when step_results.get("signal_age", 1.0) == 0.0 the result is REJECTED_SIGNAL_AGE (score and tier kept, rejected_step signal_age), persisted with eligible_score `screening/secondary_screener.py:339-354`. Step 8: otherwise PASSED, persisted with eligible_score, with an INFO line giving status, score and tier `screening/secondary_screener.py:356-374`. INTERPRETATION: step 6 runs before step 7 `screening/secondary_screener.py:316-354`, so a signal whose signal_age step returned 0.0 ends REJECTED_SIGNAL_AGE only if its total still reaches effective_min; under normal returns such a signal totals at most 55 and ends REJECTED_SCORE_<n> at step 6. Step 7 is reachable when the total still reaches 60 - for example with timed-out dead steps (MC-55) or with injected market data (MC-40). (Evidence: SC-11, SC-12.)

**MC-59** FACT (TEST). The scorer tests (text only): all ten at 1.0 give 100 `tests/unit/test_quality_scorer.py:74, 79`; tiers at 80 HIGH, 65 MEDIUM, 75 MEDIUM and 15 LOW `tests/unit/test_quality_scorer.py:90, 101-102, 105, 116-117, 120, 129-130, 133, 139-140`; 60 passes `tests/unit/test_quality_scorer.py:143, 154-155`; missing steps leave the denominator: circuit_check missing gives 61 `tests/unit/test_quality_scorer.py:249, 274-276`, seven missing give 100 and HIGH `tests/unit/test_quality_scorer.py:279, 291-293`, five missing give 64 and LOW `tests/unit/test_quality_scorer.py:296, 311-313`. (Evidence: TS-04.)

## 11. Outcomes, statuses, persistence and error paths

**MC-60** FACT (SOURCE). ScreeningResult: passed, status (commented "PASSED" | "REJECTED_<step>" | "SKIPPED_<reason>"), score 0 to 100, tier HIGH, MEDIUM or LOW, rejected_step, step_results, step_statuses, error_steps, latencies_ms and market_data_snapshot `screening/secondary_screener.py:42-54`; SKIPPED_ results come from _make_skipped: passed false, score 0, tier LOW, empty step data `screening/secondary_screener.py:583-596`. (Evidence: ST-01.)

**MC-61** FACT (SOURCE); FACT (COUNT); HISTORICAL. The statuses screen() writes on the default path, in order: REJECTED_NOT_MIS_TRADABLE (step 0, active mode only); SKIPPED_QUOTE_UNAVAILABLE (step 1); REJECTED_CIRCUIT_PROXIMITY (step 1b, D-002); SKIPPED_EXECUTOR_ERROR (step 4); REJECTED_STEP_ERROR (step 4b); SKIPPED_SCORER_ERROR (step 5); REJECTED_SCORE_<n> (step 6); REJECTED_SIGNAL_AGE (step 7); PASSED (step 8) `screening/secondary_screener.py:142-374`. Count Q13 found 22 screener lines naming REJECTED_, SKIPPED_ or PASSED; 17 lie in the lines read and the other 5 in lines 420 to 553, the enforce branch, which was not read (D-002; C-007 records a REJECTED_<gate reason> line, `docs/agenda/c-007_signal-persistence/C-007_manual.md:213`). (Evidence: ST-02.)

| Status | Step | Condition | Score, tier | rejected_step (the reason written) | eligible_score | Processor |
|---|---|---|---|---|---|---|
| REJECTED_NOT_MIS_TRADABLE | 0 | blocked, product MIS, shadow false | 0, LOW | mis_tradable | - | INFO, return |
| SKIPPED_QUOTE_UNAVAILABLE | 1 | no quote, or quote_fn or _build_market_data raised | 0, LOW | none (the status) | - | WARNING, return |
| REJECTED_CIRCUIT_PROXIMITY | 1b | D-002's function gives a reason | 0, LOW | circuit_proximity | - | INFO, return |
| SKIPPED_EXECUTOR_ERROR | 4 | run_all raised | 0, LOW | none (the status) | - | WARNING, return |
| REJECTED_STEP_ERROR | 4b | any step raised | 0, LOW | the error steps, comma-joined | - | INFO, return |
| SKIPPED_SCORER_ERROR | 5 | the scorer raised | 0, LOW | none (the status) | - | WARNING, return |
| REJECTED_SCORE_<total> | 6 | total below effective_min | total, tier | none (the status) | effective_min | INFO, return |
| REJECTED_SIGNAL_AGE | 7 | signal_age result 0.0 | total, tier | signal_age | effective_min | INFO, return |
| PASSED | 8 | otherwise | total, tier | none (the status) | effective_min | continues (section 12) |

Sources of the table: `screening/secondary_screener.py:142-374`, `screening/secondary_screener.py:583-596` and `screening/secondary_screener.py:598-637` (the reason written is rejected_step, or the status when there is none); the processor column: `signals/signal_processor.py:1008-1023`.

**MC-62** FACT (SOURCE); FACT (TEXT). Every path calls _persist: update_signal_status(signal_id, status, reason = rejected_step or status, ts) and insert_screener_result(signal_id, score, tier, status, step results, latencies, the snapshot, ts, eligible_score) in one try, a failure logged as an ERROR and not raised `screening/secondary_screener.py:598-637`; eligible_score is passed on the score, signal-age and PASSED paths only `screening/secondary_screener.py:336, 353, 369`. After that try, outside it, the P3_SCREEN evidence capture records identity, trigger, status, rejected step, score, tier, step results, step statuses and the snapshot `screening/secondary_screener.py:638-674`; its comment says step_statuses is "persisted NOWHERE else" `screening/secondary_screener.py:641-643`; _evidence_capture never raises into screening `screening/secondary_screener.py:676-695`. HISTORICAL: C-007 S-13 (`docs/agenda/c-007_signal-persistence/C-007_manual.md:199`). (Evidence: ST-03.)

**MC-63** FACT (SOURCE); FACT (TEST). The processor writes no status of its own for a screener outcome: SKIPPED_ gives a WARNING and returns; not passed gives an INFO and returns; PASSED continues `signals/signal_processor.py:1008-1023`; the outcome is counted in its statistics buckets `signals/signal_processor.py:1959-1970`. A processor test asserts that the screener's REJECTED_SCORE_40 is not overwritten `tests/unit/test_signal_processor.py:811, 818, 825, 827-828`. (Evidence: ST-04, IN-01.)

**MC-64** FACT (TEXT); HISTORICAL. Comment claims about the record corpus, not checked here: "68.99 % of the corpus is a REJECTED_SCORE_*" `screening/secondary_screener.py:638-640`. HISTORICAL: B-009 records REJECTED_SCORE as the leading terminal status in its source (`docs/agenda/b-009_intraday-operation/B-009_manual.md:278`). Neither is a frequency established by this chapter. (Evidence: ST-05.)

**MC-65** FACT (TEST). The screener tests (text only) assert: PASSED `tests/unit/test_secondary_screener.py:178-179`; REJECTED_SCORE_ under the global and the strategy minimum `tests/unit/test_secondary_screener.py:263-265, 287-288`; min_score 0 falls back to the global pass score `tests/unit/test_secondary_screener.py:291, 295, 306`; the signal-age test accepts either "REJECTED_SIGNAL_AGE" or a 0.0 signal_age `tests/unit/test_secondary_screener.py:324`; quote_fn raising gives SKIPPED_QUOTE_UNAVAILABLE with an ERROR log `tests/unit/test_secondary_screener.py:340-342`; no quote gives the same status at INFO with no ERROR `tests/unit/test_secondary_screener.py:354-356`; a step exception gives REJECTED_STEP_ERROR `tests/unit/test_secondary_screener.py:390-392`; the state-store row carries signal_id, score, tier and status `tests/unit/test_secondary_screener.py:412, 428-431`. The all-strategies test keeps the strategies without v3_playbook and asserts 15 `tests/unit/test_secondary_screener.py:548-556`, commented that "the PB-01 shadow playbook (V3 Step 10b) is exercised by its own tests" `tests/unit/test_secondary_screener.py:553-554`, and checks only that each screen returns a ScreeningResult `tests/unit/test_secondary_screener.py:587`. (Evidence: TS-02, TS-03.)

### 11.1 Error paths

| Where | What fails | Handling | Result | Evidence |
|---|---|---|---|---|
| step 1 | quote_fn or _build_market_data raises | ERROR with traceback | SKIPPED_QUOTE_UNAVAILABLE | MC-20 |
| step 4 | run_all raises | ERROR with traceback | SKIPPED_EXECUTOR_ERROR | MC-30 |
| a step | the step function raises | executor: 0.0, ERROR, error_steps | REJECTED_STEP_ERROR at step 4b | MC-43, MC-46 |
| a step | the step exceeds its timeout | executor: 0.5, TIMEOUT, pool replaced | no reject by itself | MC-43, MC-44 |
| step 5 | the scorer raises | ERROR with traceback | SKIPPED_SCORER_ERROR | MC-51 |
| step 0 | the product resolver raises | the helper returns False | not dropped (fail-open) | MC-14 |
| step 0 | a corrupt blocklist date | is_blocked returns False | not blocked (fail-open) | MC-15 |
| _persist | a state-store write fails | ERROR logged, not raised | the result is still returned | MC-62 |
| _persist | the evidence capture fails | never raises into screening | the result is still returned | MC-62 |
| shadow compare | _log_v3_shadow_compare raises | not read | NOT ESTABLISHED (G-14) | MC-28 |

## 12. Immediate downstream hand-offs

| Consumer after screen() | What it receives from the screener | Evidence | Owner as recorded |
|---|---|---|---|
| the processor's branch on the status | the status (SKIPPED_, other not passed, PASSED) | MC-63 | C-008 ST-12 (`docs/agenda/c-008_signal-processing/C-008_manual.md:152`): D topics |
| entry and SL derivation | (nothing from the screener) | MC-66 | C-008 ST-13 (`docs/agenda/c-008_signal-processing/C-008_manual.md:153`): E topics |
| retest divert | tier and score | MC-67, MC-75 | C-008 ST-15 (`docs/agenda/c-008_signal-processing/C-008_manual.md:155`): D topics |
| sizing | tier | MC-67 | C-008 ST-17 (`docs/agenda/c-008_signal-processing/C-008_manual.md:157`): F-004 |
| entry throttle | (no tier, score or status) | MC-68 | D-016 or F-010, open (BK-56, `docs/governance/V2_DESIGN_LOG.md:596`) |
| entry-gate release path | a WatchEntry's tier | MC-69 | BK-24 (`docs/governance/V2_DESIGN_LOG.md:564`): C-008; D topics; B-009 |
| ranking | NOT ESTABLISHED (G-17) | MC-70 | D-017 by title (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:308`); the allocator hook, C-008 ST-19 (`docs/agenda/c-008_signal-processing/C-008_manual.md:159`): J-012 |

The owners are those recorded in the cited V2 records; this chapter assigns none (OSQ-07: these are downstream boundary and hand-off references, not D-003 ownership).

**MC-66** FACT (SOURCE). After PASSED the processor heartbeats, derives the entry and SL prices from the trigger price and the strategy, and turns the direction into an order side `signals/signal_processor.py:1022-1035`; then the retest divert (MC-75), and later sizing with screen_result.tier `signals/signal_processor.py:1108-1121`. The screener takes no further part after its return. (Evidence: HO-01.)

**MC-67** FACT (SOURCE); FACT (CONFIG); FACT (TEXT); HISTORICAL. Where the tier goes: the processor passes screen_result.tier, with the score, to the retest diverter `signals/signal_processor.py:1042-1049` and the tier to the sizer `signals/signal_processor.py:1111-1121`; the sizer's multiplier is its tier map's value for the tier (default 1.0) times the performance weight `capital/position_sizer.py:542, 544-545`. The repository holds position_sizing.tier_multipliers HIGH 1.0, MEDIUM 0.70, LOW 0.50 `config/system_config.yaml:237-240`, and the scoring file's comment says the sizer's tier weights are owned there `config/scoring_weights.yaml:24-27`; main.py's sizer wiring was not read (HISTORICAL: `docs/agenda/a-005_capital-model/A-005_manual.md:47`). The screener stores score and tier in its analytics row `screening/secondary_screener.py:622-632`. (Evidence: SQ-10.)

**MC-68** FACT (SOURCE). The entry throttle is built in the processor `signals/signal_processor.py:261-263` and asked admit(symbol) on three paths; the lines printed on each path release a reservation ("entry_throttled") and refuse with ENTRY_THROTTLED `signals/signal_processor.py:1410, 1413, 1417, 2225, 2228, 2232, 2530, 2533, 2536`; its interface: ThrottleResult, EntryThrottle, enabled, admit(symbol) and metrics `signals/entry_throttle.py:31, 37, 62, 65, 117`. No line of entry_throttle.py names tier, score or screen (read 058, none printed), while the same expression matched 7 lines of entry_gate.py (read 057). (Evidence: HO-02.)

**MC-69** FACT (SOURCE); FACT (TEXT); HISTORICAL. The entry gate's stated purpose: "Hold screened signals in a price-watchlist. Poll LTP at fixed interval." `screening/entry_gate.py:4-8`; its WatchEntry carries "tier: str # from screener result" `screening/entry_gate.py:72`; the processor's continue_from_gate resumes the "post-screening pipeline for a WatchEntry released by EntryGate", bypassing "screening + price derivation" `signals/signal_processor.py:2010-2017`; a processor comment says "nothing calls EntryGate.add()" `signals/signal_processor.py:2236-2240` (G-18). HISTORICAL: BK-24 records the same, re-verified at C-008 (`docs/governance/V2_DESIGN_LOG.md:564`). (Evidence: HO-03.)

**MC-70** FACT (SOURCE); UNKNOWN / NOT ESTABLISHED. The ranker sorts candidates by score descending, tier rank descending, then triggered epoch and signal id ascending `allocation/ranker.py:5, 14, 18`, over ScoredCandidate objects from allocation.models `allocation/ranker.py:11`, not read. Whether the screener's score and tier fill those candidates is NOT ESTABLISHED (G-17; D-017); this chapter does not claim it. (Evidence: HO-04.)

**MC-71** FACT (COUNT). The four screener files do not name these components, apart from one comment: count Q12 (retest, entry_gate, throttle, position_siz, ranker) over those files found one line, the scoring file's comment naming position_sizing `config/scoring_weights.yaml:25`; its control P09 found 73 lines in the processor. (Evidence: HO-05.)

**MC-72** INTERPRETATION; HISTORICAL. D-003's stage ends at screen()'s return: a ScreeningResult and the status the screener persisted. PASSED and the tier are the screener's internal outcome, not final Trade approval: price derivation, the retest divert, sizing and reservation, the throttle and placement follow (MC-66 to MC-69), and the final verdict is D-018's (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:309`; D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 27). (Evidence: HO-06.)

**MC-73** FACT (TEST). The processor tests, with a mock screener (text only): a SKIPPED_ result logs a WARNING and stops with no sizing `tests/unit/test_signal_processor.py:744-745, 753`; a PASSED HIGH result reaches the sizer with HIGH `tests/unit/test_signal_processor.py:774-790`; the direction comes from the strategy `tests/unit/test_signal_processor.py:793-808`; the statistics count REJECTED_SCORE_30 `tests/unit/test_signal_processor.py:1713, 1719, 1726, 1728`. These test the processor's plumbing with an injected tier; they say nothing about which tiers the screener produced. (Evidence: TS-06.)

## 13. S&R, V3 and regime inputs

**MC-74** FACT (COUNT); FACT (SOURCE). Over the four screener files (secondary_screener.py, step_executor.py, quality_scorer.py, scoring_weights.yaml): count Q03, the S&R vocabulary (sr_detector, support, resistance, confluence, zone or zones), found one line, the homonym "to support special sessions" `screening/step_executor.py:33`; its positive control P03a found 19 lines in config/system_config.yaml (run after the reads, DV-03). Q04, regime, found none (control P04: 34 lines in main.py). Q05, v3_chain, playbook or pb01, found none (control P07: 55 lines in main.py). (Evidence: SR-01.)

**MC-75** FACT (SOURCE); FACT (TEXT); HISTORICAL. S&R appears after the screener: on PASSED, after price derivation, the processor's retest diverter, commented "SNR-V2 Phase A: WAIT_FOR_RETEST pre-placement divert" and reading "the ZoneCache SYNCHRONOUSLY", runs only when a diverter is set and enabled, with the screener's tier and score `signals/signal_processor.py:1037-1049`; maybe_divert takes tier and an optional score `screening/retest_monitor.py:303-318`. HISTORICAL: C-008 records no diverter set at the pin (`docs/agenda/c-008_signal-processing/C-008_manual.md:155`). (Evidence: SR-03.)

**MC-76** INTERPRETATION. Within the bounded authorised evidence (the lines read and the counts above), no S&R, regime or V3-chain input reaches the screener; its only V3 element is the hard-gate re-scale, a D-002 boundary reference, shadow on the repository values (MC-26, MC-29). J-006 "S&R use in screening" exists in the agenda only (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:442`). (Evidence: SR-04, SR-02.)

## 14. Illustrative end-to-end example (derived only from the source rules)

**MC-77** INTERPRETATION. Illustrative example, from the source rules only (hypothetical inputs chosen for illustration; not a recorded signal, quote or trade; repository configuration values; Claude's arithmetic, no legacy code run). A LONG, INTRADAY-intent strategy with min_score 0 and the YAML thresholds (MC-10); the processor calls screen(..., market_data=None) 20 s after triggered_at and 40 minutes after market_open; the symbol is not on the blocklist; the quote: last price 500.00, top bid 499.95 and ask 500.05, average_price 498.00, ohlc open 495.00, high 502.00 and low 494.00, circuit limits not reached. Step 0: enabled and shadow, the intent resolves to MIS (A-007), the symbol is not blocked, so no log and no drop (MC-13, MC-14). Step 1: the market data is built; atr, rsi, sector and avg_volume_20d are None (MC-23). The enforce branch is not taken (mode shadow, MC-26); step 1b is assumed to give no reason (its rule is D-002's and is not computed here, MC-27). The ten steps (MC-31 to MC-36): volume_surge 0.0; vwap_position 1.0 (500.00 > 498.00); atr_filter 0.0; rsi_range 0.5; price_action 1.0 (body 5.00, range 8.00, 2 x 0.625 capped at 1.0); sector_strength 0.5; time_of_day 1.0; spread_check 0.0 (0.10 / 500.00 x 100 = 0.02, above 0.005); circuit_check 1.0; signal_age 1.0. No step raised (MC-46). The score (MC-48, MC-49): 0 + 10 + 0 + 5 + 15 + 5 + 5 + 0 + 10 + 10 = 60 of 100, so the total is 60 and the tier LOW. The shadow compare logs only (MC-28). Step 6: effective_min is 60 (MC-52), and 60 is not below it. Step 7: signal_age is not 0.0. Step 8: PASSED, score 60, tier LOW, persisted with eligible_score 60 and passed to the P3_SCREEN evidence capture (MC-62). The processor then derives prices, diverts only if a diverter is set and enabled, and sizes with tier LOW (MC-66, MC-67). Variants are in the table below. (Evidence: MS-01, MD-03, MD-05, SC-07, SQ-02, SQ-03, SC-10, SC-11, ST-03, HO-01, SQ-10.)

| Variant | Inputs (otherwise as A) | Steps that change | Points | Outcome |
|---|---|---|---|---|
| A | as described | price_action 1.0, time_of_day 1.0, spread_check 0.0 | 60 | PASSED, tier LOW |
| B | the same, 70 minutes after the open | price_action 1.0, time_of_day 0.8, spread_check 0.0 | 59 | REJECTED_SCORE_59 |
| C | the same, with the bid equal to the ask (500.00) | price_action 1.0, time_of_day 1.0, spread_check 1.0 | 65 | PASSED, tier MEDIUM |
| D | the same, with no depth in the quote (bid and ask 0.0) | price_action 1.0, time_of_day 1.0, spread_check 0.5 | 62.5 | PASSED, tier LOW |
| E | the same, with no OHLC in the quote (open, high and low None) | price_action ERROR, time_of_day 1.0, spread_check 0.0 | - | REJECTED_STEP_ERROR |

Variant D's 62.5 achieved points become an integer total through the scorer's int(round(...)) (MC-48); Python rounds 62.5 to the even 62, and either way the outcome is PASSED with tier LOW. Variant E follows the source-level path of MC-39: price_action raises, the executor records an error step and screen() returns REJECTED_STEP_ERROR with score 0 and tier LOW; how often such a quote occurred is UNKNOWN (G-16).

## 15. Known defects, interpretations, unknowns and open questions

### 15.1 Known defect

- The HIGH tier was unreachable on the default path (MC-56; KNOWN DEFECT, recorded by A-008 and the scorer's own comment, confirmed in source).

### 15.2 Interpretations (each rests on cited facts; none is a recorded defect)

- MC-07: the processor's expiry and the executor's age bands measure from the same triggered_at value.
- MC-12: app_config.scoring comes from the scoring file (G-10).
- MC-18: on the repository values the pre-drop logs and never rejects, and could act only on INTRADAY intents.
- MC-25: a case mismatch in the quote map would end SKIPPED_QUOTE_UNAVAILABLE (G-20).
- MC-35: by the formula as recorded, an empty sell side would score spread_check 1.0; whether the code guards against it is NOT ESTABLISHED.
- MC-37 and MC-55: the normal-return ceiling of 65; the theoretical 77.5 if both normally-dead steps timed out; never 80.
- MC-39: a quote without OHLC would end REJECTED_STEP_ERROR (G-16; INTERPRETATION / UNKNOWN FREQUENCY).
- MC-40: the screener tests exercise inputs that the processor's path never supplies.
- MC-45: steps of concurrent signals queue on one worker (G-19).
- MC-50: no step is missing on the default path; the FIX-042 example does not arise.
- MC-52 and MC-53: the effective threshold of 60 for every strategy; the per-strategy thresholds did not tell strategies apart.
- MC-54: the units of max_spread_pct and min_adr_pct (G-15; INTERPRETATION / UNKNOWN INTENDED UNIT).
- MC-57 and MC-58: what a pass needed; a signal_age or circuit_check of 0.0 ends REJECTED_SCORE_<n> under normal returns.
- MC-72 and MC-76: the stage ends at the return; no S&R, regime or V3-chain input.
- MC-77: the illustrative example.
- MC-78 to MC-80: the cross-reference observations of section 16.

### 15.3 Residual-gap table (UNKNOWN / NOT ESTABLISHED and other unsettled items)

| Gap | Status | Not established | Recorded context (HISTORICAL) | Owner or next |
|---|---|---|---|---|
| G-04 | UNKNOWN / NOT ESTABLISHED | the configuration values that ran on a host | not establishable from the repository | - |
| G-10 | INTERPRETATION; HISTORICAL | the loader line that feeds app_config.scoring (MC-12) | A-009 (`docs/agenda/a-009_configuration-model/A-009_manual.md:33`) | none: HISTORICAL context suffices |
| G-11 | HISTORICAL | the product resolver's code (MC-18) | A-007 (`docs/agenda/a-007_product-types/A-007_manual.md:23`) | none: HISTORICAL context suffices |
| G-12 | HISTORICAL; cross-reference | the order placer's recording lines; is_mis_block_rejection's body (MC-15) | C-004 (`docs/agenda/c-004_validation/C-004_manual.md:275`) | G-007 |
| G-13 | HISTORICAL | the market-windows module (MC-34) | B-008 (`docs/agenda/b-008_market-open/B-008_manual.md:26`) | none: HISTORICAL context suffices |
| G-14 | UNKNOWN / NOT ESTABLISHED | whether the shadow compare can raise into screen() (MC-28) | C-008 [U-09] (`docs/agenda/c-008_signal-processing/C-008_manual.md:566`); the compare is D-002's (NR-01) | D-002 (the compare's body) |
| G-15 | INTERPRETATION / UNKNOWN INTENDED UNIT | the intended unit of max_spread_pct and min_adr_pct; the effect on live scores (MC-54) | - | - (no authorised read can settle intent) |
| G-16 | INTERPRETATION / UNKNOWN FREQUENCY | whether and how often a quote without OHLC occurred (MC-39) | - | - |
| G-17 | UNKNOWN / NOT ESTABLISHED | whether the ranker's candidates carry the screener's score and tier (MC-70) | - | D-017 |
| G-18 | FACT (TEXT); HISTORICAL | the callers of EntryGate.add() (MC-69) | BK-24 (`docs/governance/V2_DESIGN_LOG.md:564`) | D-016 |
| G-19 | UNKNOWN / NOT ESTABLISHED | contention and timeouts on the shared step pool (MC-45) | C-008 K-06 (`docs/agenda/c-008_signal-processing/C-008_manual.md:377`) | - (no log read) |
| G-20 | INTERPRETATION; UNKNOWN / NOT ESTABLISHED | the case of the broker's response keys (MC-25) | C-004 U-03 (`docs/agenda/c-004_validation/C-004_manual.md:484`) | C-004 (U-03) |
| (timeouts) | UNKNOWN / NOT ESTABLISHED | whether any step ever timed out (it bears on the ceiling, MC-55) | - | - |
| (runtime) | UNKNOWN / NOT ESTABLISHED | deployed settings, actual broker responses, paper or live operation and frequencies | D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 23 | - |

G-10 to G-13 are cited as HISTORICAL context only; no further legacy read is authorised or required (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 22). The owners are those of the Stage 1 gap register; none is assigned here.

### 15.4 Open questions (OPEN QUESTION; the V2 log rows are cited as HISTORICAL; none is decided or changed here)

- BK-46 (`docs/governance/V2_DESIGN_LOG.md:586`), time rules on signals: the screener's signal-age score, one of the rules it lists, is documented here (MC-36, MC-58, MC-07); the PB01 candidate expiry and the retest resume, its D-014 and D-015 parts, are outside this chapter. OPEN QUESTION.
- BK-54 (`docs/governance/V2_DESIGN_LOG.md:594`): the owner of the step-1b reject; this chapter adds no ownership evidence (MC-27). OPEN QUESTION.
- BK-55 (`docs/governance/V2_DESIGN_LOG.md:595`): steps 0 and 1 are documented here (sections 5 and 6) and step 1b as a D-002 boundary reference (MC-27); the recurrence of the age rule is described at the variable level (MC-07, MC-58, MC-78); its ownership part stays open. OPEN QUESTION.
- BK-56 (`docs/governance/V2_DESIGN_LOG.md:596`): the owners of the shadow-inning guard and the entry throttle (D-016 or F-010); this chapter adds the throttle's hand-off facts only (MC-68). OPEN QUESTION.
- BK-58 (`docs/governance/V2_DESIGN_LOG.md:598`): step 9 scores, though its docstring and the hard gate's comment call it a reject (MC-36, MC-79). OPEN QUESTION.
- BK-63 (`docs/governance/V2_DESIGN_LOG.md:603`): the step-10 weight comment's 90 s against the code's 60 s (OB-05) and the blocklist docstring's 5-day TTL against the repository's 1 day (OB-11; D-002 `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:226`). OPEN QUESTION.
- Q-34 (`docs/governance/V2_DESIGN_LOG.md:198`): part (c), which steps were dead at 0 or pinned at half, is answered by the evidence (MC-37, MC-55); the other parts are outside this chapter. OPEN QUESTION.

### 15.5 Observations on comments and configuration (no finding; for the owners named)

- OB-01 scoring_weights.yaml line 5, "final score = sum of weights for passed steps", against the proportional raw x weight total of the scorer (MC-48).
- OB-02 Line 15, price_action "candle structure + swing confirmation", against a body-to-range ratio only (MC-33).
- OB-03 Line 16, sector_strength "sector index momentum", against a placeholder on presence (MC-33).
- OB-04 Line 18, spread_check "bid-ask spread vs ATR", against the mid-price percentage (MC-35, MC-54).
- OB-05 Line 20, the signal_age comment "<= 90s", against 30 s and 60 s (BK-63; D-002 MC-23).
- OB-06 quality_scorer.py lines 75 to 77, the timeout example, against the executor's 0.5 for a timeout (MC-50).
- OB-07 step_executor.py line 400, "Reject if upper or lower circuit", against a score (BK-58; MC-36).
- OB-08 step_executor.py line 41, "SE2: Stateless. Constructor takes only logger.", against the market_open and step_timeout_sec parameters and the pool (MC-44).
- OB-09 secondary_screener.py line 51 lists step statuses without TIMEOUT, which the executor writes (MC-43).
- OB-10 system_config.yaml lines 172 to 173 cite "secondary_screener.py:144" and ":159"; at the pin the reject and the fall-through are at lines 150 to 164 and 165 (MC-13).
- OB-11 The blocklist docstring's 5-day TTL against the repository's 1 day (BK-63; D-002 MC-25): not re-read in D-003.

## 16. Cross-reference observations for D-001 and D-002 (no edit to them)

Per the reconciliation, these are concise labelled observations; the frozen manuals and the governance records are not edited (D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt, line 17).

**MC-78** HISTORICAL; INTERPRETATION. D-001's interpretation that "a signal that passes Layer 1's age check can still end REJECTED_SIGNAL_AGE at step 7" (`docs/agenda/d-001_pre-filter/D-001_manual.md:240`) is refined, not contradicted: on the processor's path under normal returns, a signal whose signal_age step returned 0.0 totals at most 55 and ends REJECTED_SCORE_<n> at step 6 (MC-57, MC-58); step 7 is reached only if the total still meets the threshold, for example when dead steps time out (MC-55). D-001 also left open whether the processor's expiry and the executor's bands measure from the same timestamp (same line); they measure from the same triggered_at value, at different instants (MC-07). D-001's FACT rows (`docs/agenda/d-001_pre-filter/D-001_manual.md:227`) are unaffected. (Evidence: SC-12, SQ-08, IN-07.)

**MC-79** HISTORICAL; INTERPRETATION. D-002 MC-39: "on the off and shadow paths a 0.0 step 9 lowers the score and is not by itself a reject" (`docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:291`) - confirmed: screen() does not branch on circuit_check (MC-46). Under normal returns on the processor's path, the 10-point loss caps the total at 55, so an at-circuit quote that reaches the steps ends REJECTED_SCORE_<n> (MC-57). The wording question stays with BK-58 (`docs/governance/V2_DESIGN_LOG.md:598`), OPEN. (Evidence: SQ-08, SC-07.)

**MC-80** HISTORICAL; INTERPRETATION. A-008 recorded min_volume_surge, min_adr_pct and max_spread_pct as "the only strategy-specific part of screening" (`docs/agenda/a-008_strategy-model/A-008_manual.md:148`) - true of the configuration; on the processor's path none of them told strategies apart (MC-53). Noted for A-008's owners; no edit. (Evidence: SQ-11.)

**MC-81** HISTORICAL. C-008 left the step pool's limits unread (K-06, `docs/agenda/c-008_signal-processing/C-008_manual.md:377`) and exceptions inside the screener beyond the quote fetch open ([U-09], `docs/agenda/c-008_signal-processing/C-008_manual.md:566`); this chapter documents both (MC-43, MC-44, MC-46, MC-51, MC-62), except whether the shadow compare can raise (MC-28; G-14). (Evidence: SE-04, SE-05, SC-09.)

## 17. Cross-references

- Section A: A-004 (the learned blocklist pointer, `docs/agenda/a-004_instruments/A-004_manual.md:76`); A-005 (tier multipliers in sizing, `docs/agenda/a-005_capital-model/A-005_manual.md:47`); A-007 (product resolution, `docs/agenda/a-007_product-types/A-007_manual.md:23`); A-008 (section 12, the common screener, `docs/agenda/a-008_strategy-model/A-008_manual.md:146`; its values, `docs/agenda/a-008_strategy-model/A-008_manual.md:124`); A-009 (the scoring file in AppConfig, `docs/agenda/a-009_configuration-model/A-009_manual.md:33`).
- Section B: B-008 (MarketWindows, `docs/agenda/b-008_market-open/B-008_manual.md:26`); B-009 (SKIPPED_QUOTE_UNAVAILABLE handling, `docs/agenda/b-009_intraday-operation/B-009_manual.md:156`).
- Section C: C-004 (10.2 and 10.3, `docs/agenda/c-004_validation/C-004_manual.md:271`; `docs/agenda/c-004_validation/C-004_manual.md:276`; L-04, L-11, U-03); C-006 (the processor's pool, `docs/agenda/c-006_queue/C-006_manual.md:181`); C-007 (S-13, `docs/agenda/c-007_signal-persistence/C-007_manual.md:199`); C-008 (ST-12, ST-13, ST-15, ST-17, ST-19, K-06, [U-09]); C-009 (CC-04, `docs/agenda/c-009_signal-expiry/C-009_manual.md:159`; BK-46, `docs/agenda/c-009_signal-expiry/C-009_manual.md:186`).
- Section D: D-001 (frozen; M10, M11 and M14 to M16: `docs/agenda/d-001_pre-filter/D-001_manual.md:286`; `docs/agenda/d-001_pre-filter/D-001_manual.md:287`; `docs/agenda/d-001_pre-filter/D-001_manual.md:290`; `docs/agenda/d-001_pre-filter/D-001_manual.md:291`; `docs/agenda/d-001_pre-filter/D-001_manual.md:292`; the step-1b row `docs/agenda/d-001_pre-filter/D-001_manual.md:288`; `docs/agenda/d-001_pre-filter/D-001_manual.md:186`); D-002 (frozen; `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:46`; `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:214`; `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:216`; `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:218`; `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md:224`); D-004 and D-014 to D-018 (by the agenda).
- Sections F, G and J: F-004 (sizing, C-008 ST-17); G-007 (broker rejection feedback); J-006, J-012 (the allocator hook, C-008 ST-19) and the V3 engines.

V2 governance inputs that bear on D-003's subject (HISTORICAL: governance inputs, not legacy evidence; carried as recorded and none decided here; the workflow rules W-1, `docs/governance/V2_DESIGN_LOG.md:334`, and W-6, `docs/governance/V2_DESIGN_LOG.md:338`, keep V2 directions out of this legacy chapter):

| Record | Subject | Status as recorded |
|---|---|---|
| VP-03 (`docs/governance/V2_DESIGN_LOG.md:219`) | per-signal-type processors replace the centralised secondary score | PROPOSED; its ratification point is recorded as D-003 and D-017 (ranking with F); not ratified here |
| VP-08 (`docs/governance/V2_DESIGN_LOG.md:224`); DR-031 (`docs/governance/V2_DECISION_REGISTER.md:42`) | the strategy model; the strategy count | the count DECIDED (DR-031); the rest PROPOSED or OPEN |
| DR-032 (`docs/governance/V2_DECISION_REGISTER.md:43`) | product intent: the MIS and CNC paths | DECIDED for V2; carried as an input (OSQ-06) |
| the admission test (`docs/agenda/a-section-end/SECTION-A_END_decision_paper.md:97`) | no subset or near-copy of another strategy; recorded to be written at D-003 | OPEN; a V2 rule, not legacy evidence; not written in this legacy chapter (W-1) |
| A-009 Q39 (`docs/agenda/a-009_configuration-model/A-009_v2_design_inputs.md:123`) | Rama's input on per-strategy filtration and shadow configuration | HISTORICAL input |
| A-008 V2 inputs, section 4 (`docs/agenda/a-008_strategy-model/A-008_v2_design_inputs.md:46`) | per-strategy secondary filtration, among them "No dead inputs: every scoring step must be able to score" | PROPOSED considerations |

## 18. Evidence appendix

### 18.1 Inputs to this draft

| File | SHA-256 |
|---|---|
| D-003_STAGE1_SOURCE_EVIDENCE_26-OCT-2026.txt | CE8AD053E158588BC215CAB9072B94446E5B4B92DB37778A6BA05B9784FACAE3 |
| D-003_STAGE1_FILES_READ_LOG_26-OCT-2026.txt | 28A46070BC7A4F03042DC57B8D4297458C8680779E77D0C642DD05D634556BA5 |
| D-003_STAGE1_BOUNDARY_MATRIX_26-OCT-2026.txt | F94EACF13E082C51D1DF07EEEAE838764FAA1D8C9A2C7B91409C435BA3D749B4 |
| D-003_STAGE1_GAP_REGISTER_26-OCT-2026.txt | 244D1C7B693DF72DE5DBDE2F19D93998A0C864CD4FB4DD12839D1372B6654879 |
| D-003_STAGE1_CHANGE_NOTE_26-OCT-2026.txt | 49D5514127C2B9D14EBFAFF1E51CD7E8D50136F7A777EA04A6933833AA962437 |
| D-003_STAGE1_FINAL_CHECK_26-OCT-2026.txt | B157DC363852A22886888DA7EE928E5967A47124253931F05D0AF3DA4838100A |
| D-003_STAGE1_CHATGPT_HANDOFF_26-OCT-2026.txt | CA5025FF8200955469129D582F99231AA2406A48F87D57C0EFFE3109701CF93A |
| D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt | EE88D2FF0DAD5C62919B0A7AB7B2FAC8D7D3CE205F8D731968D4F4F78DCD10F2 |
| D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt | FAFE56FD50D9793A75431D9FCDF1198DF9EC1157F139E4FF20D4A7339EFDD13D |

Received from Rama at 17:55 IST in c41793d9-D-003_STAGE1_RECONCILIATION_AND_STAGE2_PROMPT_26-OCT-2026.zip (SHA-256 33280FDD0FE805279B8737A7DF12C59CF5C8412B7C2840B082D017499D6EAC01), which holds the reconciliation and the prompt. Earlier governing inputs cited: D-003_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, D-003_CLAUDE_PROMPT_STAGE1_AUTHORISED_READS_26-OCT-2026.txt and D-003_RAMA_ANSWER_SHEET_STAGE1_SCOPE_26-OCT-2026.txt (D-003 Stage 1).

### 18.2 Legacy files cited (read in Stage 1 at the pin; none read for this draft)

Blob IDs, line counts and reads as given in the evidence sheet's table of files and pinned blobs (D-003_STAGE1_SOURCE_EVIDENCE_26-OCT-2026.txt).

| File | Pinned blob | Lines | Stage 1 reads |
|---|---|---|---|
| allocation/ranker.py | beac7be7eadb | 27 | 051 |
| broker/zerodha_adapter.py | 0c85d2d254aa | 2521 | 043, 044, 045 |
| capital/position_sizer.py | f92eace8d6e3 | 823 | 020 |
| config/scoring_weights.yaml | e9d6ede4c02d | 45 | 010 |
| config/strategies/first_pullback_long.yaml | bba1441d7cc3 | 55 | 024 |
| config/strategies/first_pullback_short.yaml | d946da39a9c5 | 55 | 025 |
| config/strategies/gap_fade_long.yaml | 6e0c95d5191d | 57 | 026 |
| config/strategies/gap_fade_short.yaml | a78b93440fe5 | 56 | 027 |
| config/strategies/gap_go_long.yaml | 6e6656fbc6ed | 57 | 028 |
| config/strategies/gap_go_short.yaml | 7e53d31871d6 | 57 | 029 |
| config/strategies/open_high_breakdown_short.yaml | 96b354653687 | 56 | 030 |
| config/strategies/open_low_breakout_long.yaml | 48012d053f33 | 55 | 031 |
| config/strategies/pb01_breakout_retest.yaml | eedf5cac5ed5 | 80 | 032 |
| config/strategies/positional_momentum_long.yaml | 7a46596fac63 | 56 | 033 |
| config/strategies/positional_sector_rotation.yaml | b110bd594203 | 56 | 034 |
| config/strategies/positional_swing_long.yaml | 597193f8076a | 56 | 035 |
| config/strategies/range_breakout_long.yaml | ddee444cade6 | 56 | 036 |
| config/strategies/range_breakout_short.yaml | 2f99b1dd5f15 | 55 | 037 |
| config/strategies/vwap_bounce_long.yaml | e279719f9e5d | 55 | 038 |
| config/strategies/vwap_rejection_short.yaml | a62177d49e35 | 55 | 039 |
| config/system_config.yaml | bf742ae5c81c | 754 | 021, 022, 023 |
| core/mis_blocklist.py | 1f91fedfbdd4 | 161 | 046, 047 |
| main.py | 445fb0d8237a | 4294 | 011, 012, 013, 014 |
| screening/entry_gate.py | 46b173171bdf | 538 | 053, 057, 059 |
| screening/hard_gate.py | a8e7e2e49675 | 467 | 050 |
| screening/quality_scorer.py | b125e9ce3243 | 151 | 009 |
| screening/retest_monitor.py | 26639d6ed5dd | 442 | 054, 056, 062 |
| screening/secondary_screener.py | b6f8ff7223ad | 706 | 001, 002, 003, 004, 005, 079 |
| screening/step_executor.py | d83b04860f0f | 424 | 006, 007, 008 |
| signals/entry_throttle.py | b887a4be78a4 | 126 | 055, 058 |
| signals/signal_processor.py | 7fab0a02a302 | 2704 | 015, 016, 017, 018, 019, 052, 060, 061, 077, 078 |
| strategies/loader.py | 99e83b192021 | 214 | 048, 049 |
| strategies/schema.py | 22530161a5ee | 376 | 040, 041, 042 |
| tests/unit/test_mis_blocklist.py | 4960f825965e | 271 | 073 |
| tests/unit/test_quality_scorer.py | f2fddb776fb3 | 333 | 072 |
| tests/unit/test_secondary_screener.py | e8e2ddb2a77d | 601 | 063, 064, 065, 066, 071 |
| tests/unit/test_signal_processor.py | 3ec0ea84392a | 2501 | 074, 075 |
| tests/unit/test_step_executor.py | 82cfe6819362 | 648 | 067, 068, 069, 070 |

Read in Stage 1 and not cited here: screening/__init__.py (read 076).

### 18.3 Claim-to-evidence matrix

Every MC statement with its label, the Stage 1 evidence claims it rests on and its legacy citations (path, pinned blob: lines). Every range lies within the citations of the named evidence claims and within lines printed by logged Stage 1 reads (final check, checks 4 and 5).

| Claim | Label | Evidence claims | Legacy citations (path, blob: lines) |
|---|---|---|---|
| MC-01 | FACT (TEXT) | SC-01 | screening/secondary_screener.py b6f8ff7223ad: 3-5, 58-64 |
| MC-02 | FACT (SOURCE); FACT (TEXT) | IN-01 | signals/signal_processor.py 7fab0a02a302: 997-1004, 992-996, 1005-1006, 1959-1970 |
| MC-03 | FACT (COUNT); FACT (SOURCE) | IN-02 | signals/signal_processor.py 7fab0a02a302: 998; screening/secondary_screener.py b6f8ff7223ad: 44 |
| MC-04 | FACT (SOURCE); FACT (TEXT) | IN-03 | screening/secondary_screener.py b6f8ff7223ad: 123-134, 135-141 |
| MC-05 | FACT (SOURCE) | IN-04 | main.py 445fb0d8237a: 3230-3233, 3234-3237, 3238-3247, 3281-3298; screening/step_executor.py d83b04860f0f: 49-60 |
| MC-06 | FACT (SOURCE); FACT (TEXT) | IN-05 | main.py 445fb0d8237a: 3586-3592, 3281-3298; signals/signal_processor.py 7fab0a02a302: 144-145, 167-168; screening/secondary_screener.py b6f8ff7223ad: 66-81 |
| MC-07 | FACT (SOURCE); INTERPRETATION; HISTORICAL | IN-07 | signals/signal_processor.py 7fab0a02a302: 883, 886, 914-918, 999; screening/secondary_screener.py b6f8ff7223ad: 241-249; screening/step_executor.py d83b04860f0f: 406-423 |
| MC-08 | FACT (SOURCE) | IN-08 | signals/signal_processor.py 7fab0a02a302: 342-344; screening/secondary_screener.py b6f8ff7223ad: 697-705; screening/step_executor.py d83b04860f0f: 221-241 |
| MC-09 | FACT (SOURCE); FACT (TEXT) | SQ-06 | strategies/schema.py 22530161a5ee: 115-119, 251, 255, 258, 260, 262, 265, 267, 269; strategies/loader.py 99e83b192021: 126-156 |
| MC-10 | FACT (CONFIG) | SQ-05 | config/strategies/first_pullback_long.yaml bba1441d7cc3: 39-42; config/strategies/first_pullback_short.yaml d946da39a9c5: 39-42; config/strategies/gap_fade_long.yaml 6e0c95d5191d: 41-44; config/strategies/gap_fade_short.yaml a78b93440fe5: 40-43; config/strategies/gap_go_long.yaml 6e6656fbc6ed: 41-44; config/strategies/gap_go_short.yaml 7e53d31871d6: 41-44; config/strategies/open_high_breakdown_short.yaml 96b354653687: 40-43; config/strategies/open_low_breakout_long.yaml 48012d053f33: 39-42; config/strategies/pb01_breakout_retest.yaml eedf5cac5ed5: 59-62; config/strategies/positional_momentum_long.yaml 7a46596fac63: 40-43; config/strategies/positional_sector_rotation.yaml b110bd594203: 40-43; config/strategies/positional_swing_long.yaml 597193f8076a: 40-43; config/strategies/range_breakout_long.yaml ddee444cade6: 40-43; config/strategies/range_breakout_short.yaml 2f99b1dd5f15: 39-42; config/strategies/vwap_bounce_long.yaml e279719f9e5d: 39-42; config/strategies/vwap_rejection_short.yaml a62177d49e35: 39-42 |
| MC-11 | FACT (CONFIG); FACT (TEXT) | SQ-01 | config/scoring_weights.yaml e9d6ede4c02d: 10-20, 22, 28-29, 24-27, 3-4 |
| MC-12 | INTERPRETATION; FACT (TEXT); HISTORICAL | IN-06 | screening/quality_scorer.py b125e9ce3243: 4; screening/secondary_screener.py b6f8ff7223ad: 79, 96-106; config/scoring_weights.yaml e9d6ede4c02d: 10, 39-44 |
| MC-13 | FACT (SOURCE) | SC-02 | screening/secondary_screener.py b6f8ff7223ad: 142-165 |
| MC-14 | FACT (SOURCE) | MS-01 | screening/secondary_screener.py b6f8ff7223ad: 554-581, 148-165 |
| MC-15 | FACT (SOURCE) | MS-02 | core/mis_blocklist.py 1f91fedfbdd4: 85-100, 102-114, 116-123, 38 |
| MC-16 | FACT (SOURCE); FACT (TEXT) | MS-03 | main.py 445fb0d8237a: 3055-3059, 3051-3054, 3290-3293; screening/secondary_screener.py b6f8ff7223ad: 111-113 |
| MC-17 | FACT (CONFIG); FACT (TEXT); FACT (TEST) | MS-04 | config/system_config.yaml bf742ae5c81c: 179-181, 171-178; tests/unit/test_mis_blocklist.py 4960f825965e: 238, 242-243 |
| MC-18 | INTERPRETATION; HISTORICAL | MS-05 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-19 | FACT (TEST) | MS-06 | tests/unit/test_mis_blocklist.py 4960f825965e: 186, 190-191, 193, 195, 199, 202, 206, 209, 213, 215, 217, 223, 71, 77, 80, 92, 96 |
| MC-20 | FACT (SOURCE); FACT (TEXT); HISTORICAL | SC-03 | screening/secondary_screener.py b6f8ff7223ad: 167-196, 173-176 |
| MC-21 | FACT (SOURCE) | MD-01 | main.py 445fb0d8237a: 3285; broker/zerodha_adapter.py 0c85d2d254aa: 1730-1755, 1757-1766, 1773-1774 |
| MC-22 | FACT (SOURCE) | MD-02 | broker/zerodha_adapter.py 0c85d2d254aa: 208-222, 1775-1782, 1785-1791, 1792-1798 |
| MC-23 | FACT (SOURCE); FACT (TEXT) | MD-03, SC-04 | screening/secondary_screener.py b6f8ff7223ad: 386-395, 397-411, 412-418, 198 |
| MC-24 | FACT (SOURCE); FACT (COUNT) | MD-04 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-25 | INTERPRETATION; UNKNOWN / NOT ESTABLISHED; HISTORICAL | MD-11 | screening/secondary_screener.py b6f8ff7223ad: 170-171; broker/zerodha_adapter.py 0c85d2d254aa: 1773-1774 |
| MC-26 | FACT (SOURCE); FACT (CONFIG); HISTORICAL | SC-04 | screening/secondary_screener.py b6f8ff7223ad: 200-206, 420, 435, 530; config/scoring_weights.yaml e9d6ede4c02d: 39 |
| MC-27 | FACT (SOURCE) | SC-05 | screening/secondary_screener.py b6f8ff7223ad: 208-232 |
| MC-28 | FACT (SOURCE); FACT (TEXT); UNKNOWN / NOT ESTABLISHED | SC-09 | screening/secondary_screener.py b6f8ff7223ad: 305-314, 530 |
| MC-29 | FACT (SOURCE) | SR-02 | screening/secondary_screener.py b6f8ff7223ad: 29-34, 92-106; screening/hard_gate.py a8e7e2e49675: 53, 119, 139, 178, 197, 206 |
| MC-30 | FACT (SOURCE) | SC-06 | screening/secondary_screener.py b6f8ff7223ad: 234-239, 241-249, 251-263 |
| MC-31 | FACT (SOURCE) | MD-05 | screening/step_executor.py d83b04860f0f: 247-256, 271-281 |
| MC-32 | FACT (SOURCE) | MD-05 | screening/step_executor.py d83b04860f0f: 258-269, 283-316 |
| MC-33 | FACT (SOURCE); FACT (TEXT) | MD-05 | screening/step_executor.py d83b04860f0f: 318-329, 331-336 |
| MC-34 | FACT (SOURCE); FACT (TEXT); FACT (CONFIG); HISTORICAL | MD-05, MD-10 | screening/step_executor.py d83b04860f0f: 338-370, 30-36; main.py 445fb0d8237a: 3236; config/system_config.yaml bf742ae5c81c: 103 |
| MC-35 | FACT (SOURCE); INTERPRETATION; UNKNOWN / NOT ESTABLISHED | MD-05, MD-02, MD-09 | screening/step_executor.py d83b04860f0f: 372-395 |
| MC-36 | FACT (SOURCE); FACT (TEXT) | MD-05 | screening/step_executor.py d83b04860f0f: 397-404, 406-423 |
| MC-37 | FACT (SOURCE); INTERPRETATION; HISTORICAL | MD-06 | screening/step_executor.py d83b04860f0f: 251-253, 275-278, 296-298, 335-336; screening/quality_scorer.py b125e9ce3243: 16-19 |
| MC-38 | FACT (SOURCE) | MD-07 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-39 | INTERPRETATION; UNKNOWN / NOT ESTABLISHED; FACT (TEST) | MD-08 | screening/secondary_screener.py b6f8ff7223ad: 409-411, 265-286; broker/zerodha_adapter.py 0c85d2d254aa: 1793-1795; screening/step_executor.py d83b04860f0f: 322-326, 182-195; tests/unit/test_step_executor.py 82cfe6819362: 534-538 |
| MC-40 | FACT (TEST); INTERPRETATION | TS-01 | tests/unit/test_secondary_screener.py e8e2ddb2a77d: 87-88, 91, 96, 139, 149, 176 |
| MC-41 | FACT (TEXT) | SE-01 | screening/step_executor.py d83b04860f0f: 3-4, 40-47 |
| MC-42 | FACT (SOURCE); FACT (TEXT) | SE-02 | screening/step_executor.py d83b04860f0f: 118-154, 132, 129-130 |
| MC-43 | FACT (SOURCE) | SE-03 | screening/step_executor.py d83b04860f0f: 158-167, 168-181, 182-195, 197-206, 20-27, 213-219 |
| MC-44 | FACT (SOURCE); FACT (TEXT) | SE-04 | screening/step_executor.py d83b04860f0f: 62-76, 67-68, 78-112, 49-60; main.py 445fb0d8237a: 3234-3237, 3281-3283 |
| MC-45 | INTERPRETATION; FACT (TEXT); HISTORICAL | SE-05 | screening/step_executor.py d83b04860f0f: 83-86 |
| MC-46 | FACT (SOURCE) | SC-07 | screening/secondary_screener.py b6f8ff7223ad: 265-286, 265-374 |
| MC-47 | FACT (TEST) | TS-05 | tests/unit/test_step_executor.py 82cfe6819362: 53, 55-57, 458, 466, 474, 486, 501, 517, 541, 552 |
| MC-48 | FACT (SOURCE) | SQ-02 | screening/quality_scorer.py b125e9ce3243: 56-69, 79-103, 105-120 |
| MC-49 | FACT (SOURCE); FACT (TEXT) | SQ-03 | screening/quality_scorer.py b125e9ce3243: 122-134, 25-34, 142-150, 44 |
| MC-50 | FACT (SOURCE); FACT (TEXT); INTERPRETATION | SQ-04 | screening/quality_scorer.py b125e9ce3243: 75-77; screening/step_executor.py d83b04860f0f: 168-181 |
| MC-51 | FACT (SOURCE) | SC-08, SC-10 | screening/secondary_screener.py b6f8ff7223ad: 288-303, 316-337, 288-374 |
| MC-52 | FACT (CONFIG); INTERPRETATION; HISTORICAL | SQ-07, SQ-05 | screening/quality_scorer.py b125e9ce3243: 134 |
| MC-53 | INTERPRETATION; HISTORICAL | SQ-11 | screening/step_executor.py d83b04860f0f: 251-255, 275-280 |
| MC-54 | FACT (SOURCE); FACT (CONFIG); FACT (TEST); INTERPRETATION; UNKNOWN / NOT ESTABLISHED | MD-09 | screening/step_executor.py d83b04860f0f: 393-395; screening/secondary_screener.py b6f8ff7223ad: 234-239; strategies/schema.py 22530161a5ee: 116-119; tests/unit/test_step_executor.py 82cfe6819362: 53, 55-57, 357-363, 403-411; tests/unit/test_secondary_screener.py e8e2ddb2a77d: 53, 56-59 |
| MC-55 | INTERPRETATION | MD-06, SQ-01, SE-03 | screening/step_executor.py d83b04860f0f: 168-181; config/scoring_weights.yaml e9d6ede4c02d: 28-29 |
| MC-56 | KNOWN DEFECT; FACT (TEXT); HISTORICAL | SQ-09, MD-06 | screening/quality_scorer.py b125e9ce3243: 126-127, 16-19, 139-140 |
| MC-57 | INTERPRETATION | SQ-08 | screening/quality_scorer.py b125e9ce3243: 105-108 |
| MC-58 | FACT (SOURCE); INTERPRETATION | SC-11, SC-12 | screening/secondary_screener.py b6f8ff7223ad: 339-354, 356-374, 316-354 |
| MC-59 | FACT (TEST) | TS-04 | tests/unit/test_quality_scorer.py f2fddb776fb3: 74, 79, 90, 101-102, 105, 116-117, 120, 129-130, 133, 139-140, 143, 154-155, 249, 274-276, 279, 291-293, 296, 311-313 |
| MC-60 | FACT (SOURCE) | ST-01 | screening/secondary_screener.py b6f8ff7223ad: 42-54, 583-596 |
| MC-61 | FACT (SOURCE); FACT (COUNT); HISTORICAL | ST-02 | screening/secondary_screener.py b6f8ff7223ad: 142-374 |
| MC-62 | FACT (SOURCE); FACT (TEXT) | ST-03 | screening/secondary_screener.py b6f8ff7223ad: 598-637, 336, 353, 369, 638-674, 641-643, 676-695 |
| MC-63 | FACT (SOURCE); FACT (TEST) | ST-04, IN-01 | signals/signal_processor.py 7fab0a02a302: 1008-1023, 1959-1970; tests/unit/test_signal_processor.py 3ec0ea84392a: 811, 818, 825, 827-828 |
| MC-64 | FACT (TEXT); HISTORICAL | ST-05 | screening/secondary_screener.py b6f8ff7223ad: 638-640 |
| MC-65 | FACT (TEST) | TS-02, TS-03 | tests/unit/test_secondary_screener.py e8e2ddb2a77d: 178-179, 263-265, 287-288, 291, 295, 306, 324, 340-342, 354-356, 390-392, 412, 428-431, 548-556, 553-554, 587 |
| MC-66 | FACT (SOURCE) | HO-01 | signals/signal_processor.py 7fab0a02a302: 1022-1035, 1108-1121 |
| MC-67 | FACT (SOURCE); FACT (CONFIG); FACT (TEXT); HISTORICAL | SQ-10 | signals/signal_processor.py 7fab0a02a302: 1042-1049, 1111-1121; capital/position_sizer.py f92eace8d6e3: 542, 544-545; config/system_config.yaml bf742ae5c81c: 237-240; config/scoring_weights.yaml e9d6ede4c02d: 24-27; screening/secondary_screener.py b6f8ff7223ad: 622-632 |
| MC-68 | FACT (SOURCE) | HO-02 | signals/signal_processor.py 7fab0a02a302: 261-263, 1410, 1413, 1417, 2225, 2228, 2232, 2530, 2533, 2536; signals/entry_throttle.py b887a4be78a4: 31, 37, 62, 65, 117 |
| MC-69 | FACT (SOURCE); FACT (TEXT); HISTORICAL | HO-03 | screening/entry_gate.py 46b173171bdf: 4-8, 72; signals/signal_processor.py 7fab0a02a302: 2010-2017, 2236-2240 |
| MC-70 | FACT (SOURCE); UNKNOWN / NOT ESTABLISHED | HO-04 | allocation/ranker.py beac7be7eadb: 5, 14, 18, 11 |
| MC-71 | FACT (COUNT) | HO-05 | config/scoring_weights.yaml e9d6ede4c02d: 25 |
| MC-72 | INTERPRETATION; HISTORICAL | HO-06 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-73 | FACT (TEST) | TS-06 | tests/unit/test_signal_processor.py 3ec0ea84392a: 744-745, 753, 774-790, 793-808, 1713, 1719, 1726, 1728 |
| MC-74 | FACT (COUNT); FACT (SOURCE) | SR-01 | screening/step_executor.py d83b04860f0f: 33 |
| MC-75 | FACT (SOURCE); FACT (TEXT); HISTORICAL | SR-03 | signals/signal_processor.py 7fab0a02a302: 1037-1049; screening/retest_monitor.py 26639d6ed5dd: 303-318 |
| MC-76 | INTERPRETATION | SR-04, SR-02 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-77 | INTERPRETATION | MS-01, MD-03, MD-05, SC-07, SQ-02, SQ-03, SC-10, SC-11, ST-03, HO-01, SQ-10 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-78 | HISTORICAL; INTERPRETATION | SC-12, SQ-08, IN-07 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-79 | HISTORICAL; INTERPRETATION | SQ-08, SC-07 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-80 | HISTORICAL; INTERPRETATION | SQ-11 | none in the statement (it rests on the evidence claims named and the statements it cites) |
| MC-81 | HISTORICAL | SE-04, SE-05, SC-09 | none in the statement (it rests on the evidence claims named and the statements it cites) |

### 18.4 V2 records and inputs cited (HISTORICAL)

V2 records at 8591526 (git show from the local V2 clone; 80 lines in 20 records):

| V2 record | Lines cited |
|---|---|
| `docs/agenda/a-004_instruments/A-004_manual.md` | 76 |
| `docs/agenda/a-005_capital-model/A-005_manual.md` | 47 |
| `docs/agenda/a-007_product-types/A-007_manual.md` | 23 |
| `docs/agenda/a-008_strategy-model/A-008_manual.md` | 124, 146, 147, 148, 149, 152, 269 |
| `docs/agenda/a-008_strategy-model/A-008_v2_design_inputs.md` | 46 |
| `docs/agenda/a-009_configuration-model/A-009_manual.md` | 33 |
| `docs/agenda/a-009_configuration-model/A-009_v2_design_inputs.md` | 123 |
| `docs/agenda/a-section-end/SECTION-A_END_decision_paper.md` | 97 |
| `docs/agenda/b-008_market-open/B-008_manual.md` | 26 |
| `docs/agenda/b-009_intraday-operation/B-009_manual.md` | 156, 278, 279 |
| `docs/agenda/c-004_validation/C-004_manual.md` | 271, 273, 275, 276, 467, 474, 484 |
| `docs/agenda/c-006_queue/C-006_manual.md` | 181 |
| `docs/agenda/c-007_signal-persistence/C-007_manual.md` | 199, 213 |
| `docs/agenda/c-008_signal-processing/C-008_manual.md` | 152, 153, 155, 157, 159, 377, 566 |
| `docs/agenda/c-009_signal-expiry/C-009_manual.md` | 159, 186 |
| `docs/agenda/d-001_pre-filter/D-001_manual.md` | 186, 227, 240, 286, 287, 288, 290, 291, 292, 347 |
| `docs/agenda/d-002_hard-rejection-gates/D-002_manual.md` | 46, 210, 214, 216, 218, 224, 226, 291 |
| `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt` | 293, 294, 295, 305, 306, 307, 308, 309, 442 |
| `docs/governance/V2_DECISION_REGISTER.md` | 42, 43, 78, 79 |
| `docs/governance/V2_DESIGN_LOG.md` | 198, 219, 224, 334, 338, 564, 586, 594, 595, 596, 598, 603 |

Governing inputs (19 lines in 4 files):

| Input | Lines cited |
|---|---|
| D-003_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt | 15, 25 |
| D-003_CHATGPT_STAGE1_RECONCILIATION_26-OCT-2026.txt | 3, 10, 11, 12, 13, 16, 17, 22, 23, 24, 27 |
| D-003_CLAUDE_PROMPT_STAGE2_DRAFT_MANUAL_26-OCT-2026.txt | 22, 34, 37, 39, 53 |
| D-003_RAMA_ANSWER_SHEET_STAGE1_SCOPE_26-OCT-2026.txt | 15 |

### 18.5 Status

- DRAFT / NOT FROZEN: draft 1 for ChatGPT's independent review; not stored, committed or pushed; no other manual and no governance record changed.
- Companions of this draft: D-003_STAGE2_DRAFT_CHANGE_NOTE_26-OCT-2026.txt, D-003_STAGE2_CLAUDE_HANDOFF_26-OCT-2026.txt and D-003_STAGE2_FINAL_CHECK_26-OCT-2026.txt.
