# D-002 - HARD REJECTION GATES (LEGACY MANUAL)

Status: FROZEN 2026-10-26 (metadata revision M1 of revision 1, DR-068), as factual legacy documentation only: it records no V2 design decision and makes no claim about deployed or runtime behaviour. ChatGPT's final freeze-readiness review, D-002_FINAL_FREEZE_READINESS_REVIEW_26-OCT-2026.txt (36 lines; SHA-256 500AFA2B3ADFBC802AD77500362C29507109908CB4A85E56E27F7959547B19E6; sent by Rama at 14:29 IST), approved M1 for the documentation freeze, subject to a controlled freeze and storage package. Draft 1 was written under ChatGPT's correction and drafting prompt from the accepted Stage 1 package; revision 1 applied ChatGPT's shadow-comparison qualification; metadata revision M1 named the revision 1 citation audit. Nothing in the legacy repository was read, searched or counted for any of them. This file is M1 (stored unchanged as docs/inputs/manuals/D-002_HARD_REJECTION_GATES_REV1_M1_26-OCT-2026.md; SHA-256 A5787B04519F14EE025425006D4589AF8853638EFC88DEA919DEB6C6379C8998) with freeze edits only: the title, this status line, section 2.4's audit reference, two input rows in section 12.1 and section 12.5.

Revision 1 (26-Oct-2026, 13:41 IST) supersedes draft 1 as delivered, D-002_HARD_REJECTION_GATES_MANUAL_DRAFT_26-OCT-2026.md (SHA-256 5C2948165BA2246DBC8A9DDF4A1B36FDAEDA937A79C5F4EEDF52E1B20920BA23), which is kept unchanged. It makes the one correction that D-002_CHATGPT_REVIEW_OF_MANUAL_DRAFT_26-OCT-2026.txt, section 2, requires: the shadow comparison runs in shadow mode only when a HardGate instance is set (SC-05). The condition is now stated in section 1.1, the section 3 diagram (step 5), MC-06, the section 5 table (the HardGate.evaluate row), MC-19, MC-22 (with the repository wiring) and the section 9.1 shadow row; this header, sections 1.2 and 2.2 and the input table (12.1) record the revision. No citation, label, claim ID, evidence claim, finding, owner, scope or open item changed, and the citation audit is unchanged. Details: D-002_MANUAL_DRAFT_REV1_CHANGE_NOTE_26-OCT-2026.txt.

Metadata revision M1 (26-Oct-2026, 14:08 IST) supersedes revision 1 as reviewed, D-002_HARD_REJECTION_GATES_MANUAL_DRAFT_REV1_26-OCT-2026.md (SHA-256 C515CCB5A377C4FC48255FF7022B65845ECEF90F24FAB84685AAB65F3437A1FF), which is kept unchanged; the file name is kept. It makes only the reference correction that D-002_CHATGPT_FREEZE_READINESS_REVIEW_26-OCT-2026.txt requires: section 2.4 now names the revision 1 citation audit, D-002_MANUAL_CITATION_AUDIT_REV1_26-OCT-2026.txt, whose header names this manual; the draft 1 audit, D-002_MANUAL_CITATION_AUDIT_26-OCT-2026.txt, is kept unchanged as the historical audit of draft 1. This header and the input table (12.1) record the revision. No statement, citation, label, claim ID, evidence claim, finding, owner, scope or open item changed. Details: D-002_MANUAL_DRAFT_REV1_METADATA_CHANGE_NOTE_26-OCT-2026.txt.

Topic: D-002 Hard rejection gates (master agenda, section D, `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:293`). Legacy system status: DEAD / REFERENCE ONLY.

Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa), read on 26-Oct-2026 in D-002 Stage 1 through the gated masking filter: 13 count-only searches over authorised files and 50 logged reads of 16 authorised files at their pinned blobs. The evidence is the Stage 1 package, revision 1 (D-002_STAGE1_SOURCE_EVIDENCE_REV1_26-OCT-2026.txt and its companions); this draft read no legacy file.

Purpose: a factual map of the legacy mechanisms that can reject or prevent admission at the hard-gate and eligibility boundary, and of where they sit on the signal path, as the pinned source and the repository configuration define them. It is not a catalogue of every refusal in the system and not a V2 design.

Security: no credential, token, account or client identifier, host or address, deployed path, mailbox or person name from the legacy source or the host records is reproduced.

Masking-input audit (process provenance): on every Stage 1 filter run that started (61 of 64), the masking filter loaded the requested file or files in full and two legacy host records, HR-1 (a system map) and HR-2 (a deployment record), in full and value-blind as its host-token source, and attempted a third, HR-3 (a testing-snapshot manifest), which is absent. HR-1 and HR-2 were not printed, not seen and not used as evidence. The other three runs stopped on a regular-expression error before opening anything. Each run is listed with its inputs in D-002_STAGE1_FILES_READ_LOG_26-OCT-2026.txt (sections 1 to 6).

## 1. Purpose and scope

### 1.1 What this chapter documents

ChatGPT's scope decision OSQ-01: "D-002 documents the legacy mechanisms that can reject or prevent admission at the hard-gate / eligibility boundary, plus the evidence needed to map their actual place in the pipeline. It does NOT become a catalogue of every refusal in the whole system." (D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 10). Within that scope this chapter covers:

- the HardGate battery in screening/hard_gate.py and its GateVerdict contract (section 4);
- the shared circuit-proximity function and its conditional use at the screener's step 1b (sections 3 and 4);
- where and when the battery runs: the enforce branch and the shadow compare, each only in its mode and with a hard gate set (section 5);
- the construction, wiring and repository values of these mechanisms (sections 4 and 5);
- the eligibility evidence assigned to D-002 by the records - the F&O ban list and the index universe, with the MIS learned blocklist only as a cross-reference (section 6);
- the entry-window evidence relevant to hard rejection (section 7);
- the boundary with the V3 chain and the PB01 stages (section 8).

### 1.2 Scope basis (HISTORICAL / RECORD-DERIVED)

- The master agenda gives D-002 its title only: "D-002 Hard rejection gates" (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:293`), between D-001 Pre-filter (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:292`) and D-003 Secondary screening (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:294`), with D-017 Score / ranking (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:308`) and D-018 Final Trade / Reject decision (`docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:309`) later in section D.
- ChatGPT's Stage 0 review and scope decisions OSQ-01 to OSQ-10 (D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt): section 2 sets the documentation scope and the boundaries used here.
- ChatGPT's review of the Stage 1 evidence (D-002_CHATGPT_REVIEW_OF_STAGE1_EVIDENCE_26-OCT-2026.txt): conditional acceptance, the circuit-proximity wording correction (made in revision 1 of the evidence package) and the authority for this draft.
- ChatGPT's review of draft 1 (D-002_CHATGPT_REVIEW_OF_MANUAL_DRAFT_26-OCT-2026.txt): conditional acceptance and the shadow-comparison qualification, made in revision 1 of this draft under D-002_CLAUDE_PROMPT_MANUAL_DRAFT_PRECISION_FIX_26-OCT-2026.txt.
- This draft's prompt (D-002_CLAUDE_PROMPT_CORRECTION_AND_MANUAL_DRAFT_26-OCT-2026.txt), Part B: the ten required content areas.

### 1.3 Boundaries

| Topic | What stays with it (as recorded) | D-002 relation in this chapter | Basis |
|---|---|---|---|
| D-001 Pre-filter (frozen) | the map of the processor's checks before screen(), the early checks inside screen() and the HardGate shadow battery | cross-reference; its findings are HISTORICAL here | DR-067 (`docs/governance/V2_DECISION_REGISTER.md:78`); OSQ-10 |
| D-003 Secondary screening | the screener's own step refusals: step errors, the step-7 signal-age check, the step-9 circuit score, SKIPPED_ results; with C-004 and G-007, the MIS pre-drop | boundary references only | OSQ-03 |
| D-017 Score / ranking | the total, the tier and the re-scale helpers | outside; noted only | OSQ-03, OSQ-08 |
| D-018 Final Trade / Reject decision | the minimum score and the final verdict | D-002 stops at the gate result and the hand-off | OSQ-08 |
| C-004 Validation | the static exclusion list at the webhook edge; the MIS eligibility record (section 10.3) | cross-reference | OSQ-05 |
| C-008 Signal processing | the processor's pre-flight and strategy checks, including the two entry-window refusals and the strategy circuit breaker | cross-reference; the window layer is shared by B-008 row 4 with D-002 (owner open) | OSQ-06 |
| J-010 V3 chain | the V3 chain and its generic playbook gates (by A-008, J topics also take the battery's details; owner OPEN) | mapped to callers only | OSQ-04 |
| C-011 EOD / Daily signal routing | the PB01 route and its would-be gates | mapped to callers only | OSQ-09 |

### 1.4 Not covered (cross-references only)

- the kill switch (I topics), the capital and risk gates (F topics), the entry throttle and the consecutive-losses gate: outside D-002 by OSQ-07 and not read in Stage 1;
- the entry gate, by its docstring a watchlist release gate after screening, not a hard rejection gate (section 9, MC-44);
- the detail of the ten steps, the score, the tier and the minimum score (D-003, D-017, D-018).

## 2. Source baseline, method and limitations

### 2.1 Baseline

- Legacy repository: trading-system-prod-snapshot (DEAD / REFERENCE ONLY), branch main, commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb; tree 080cf3ea6f3c825d144aa2eff9882c88b4d0fdaa (1,438 files, 125 folders), rebuilt from the saved listing and matched to the pin; ls-remote showed HEAD and main at the pin and no other ref.
- V2 governance baseline: main at e16352901a3c8327d03e4074503a670376f4d3fc (the D-001 freeze). Every V2 record cited here is at that commit.
- The clone Claude read from: HEAD the pin, 8,920 local objects before and after Stage 1, never-open blobs present 0.

### 2.2 Method

- Stage 0 (inventory by path name only), reviewed and accepted by ChatGPT with scope decisions OSQ-01 to OSQ-10.
- Stage 1: a safety gate first (the filter chain and every token source it loads, disclosed by role before any read); the read lock lifted as a step of its own; a gate test with a trace self-test; 13 count-only searches over authorised files with a positive control for the one zero; 50 logged reads of 16 authorised files at their pinned blobs; the lock re-engaged. Every filter run was traced and its masking inputs logged.
- Revision 1 of the Stage 1 package corrected one summary sentence (circuit proximity) at ChatGPT's request; no claim changed.
- This draft: written from the revision 1 evidence and frozen V2 records only. No legacy file, test or script was opened or run for it.
- Revision 1 of this draft: the shadow-comparison qualification required by ChatGPT's review of draft 1, from the same evidence (SC-05). No legacy file was opened, and no statement's citations, labels or evidence claims changed.

### 2.3 Evidence labels

| Label | Meaning in this chapter |
|---|---|
| FACT (SOURCE) | code lines read at the pin in Stage 1 |
| FACT (TEXT) | a comment or docstring: what the text says, not behaviour |
| FACT (CONFIG) | a repository configuration value, not a deployed host's value |
| FACT (COUNT) | a count-only search over named authorised files: file names and counts, no line printed |
| FACT (METADATA) | listing, gate or log facts |
| HISTORICAL | a V2 record or a governing input, cited at e163529 or by file and line; never a legacy-source fact |
| INTERPRETATION | a reading that joins facts; the facts it rests on are cited |
| UNKNOWN / NOT ESTABLISHED, NOT READ | not established by the bounded Stage 1 evidence; a search that found nothing is not proof of absence beyond its files |
| OPEN | an owner or scope question that the evidence does not settle |
| KNOWN DEFECT | not used in this chapter |

Status discipline: a comment is not behaviour; a function existing is not it being called; a call on a branch is not the branch being taken; a configured value is not a deployed one; a shadow or log-only path is not an effect on live decisions.

### 2.4 Citation conventions

- A legacy citation `path:lines` names lines printed by a logged Stage 1 read of that file at its pinned blob; the blob of each file is in section 12.2.
- Each material statement carries an ID (MC-01 to MC-44) and names the Stage 1 evidence claims it rests on (HG, SC, WS, BD, EL, WN, VC, OT and NR IDs of D-002_STAGE1_SOURCE_EVIDENCE_REV1_26-OCT-2026.txt); D-002_MANUAL_CITATION_AUDIT_FREEZE_26-OCT-2026.txt maps every ID to its evidence claims, paths, blobs and line ranges.
- V2 records are cited as `path:line` at e163529; governing inputs by file name and line.
- Quotations are verbatim except that comment markers and line breaks are dropped, non-ASCII dashes, arrows and symbols are shown in ASCII, and "..." marks words left out.

### 2.5 Audit log

D-002_STAGE1_FILES_READ_LOG_26-OCT-2026.txt lists the safety gate (section 1), the lock, the gate test and the trace self-test (section 2), every count-only search (section 3) and read (section 4) with its masking inputs, the totals over the 64 filter invocations (section 5) and two deviations (section 6): DV-01, an unintended one-line read that repeated an already printed line, and DV-02, three reads that failed on a regular-expression error before opening anything; neither touches a claim.

### 2.6 Limitations

- Repository configuration only: it is not proof of deployed or runtime state.
- Bounded evidence: reads were ranges or targeted lines; searches covered named authorised files only.
- Not opened: the Group E records (including the two hard-gate plan and parity records), the five optional tests and scripts/v3_hardgate_parity_recompute.py (NR-01 to NR-03).
- Not established: G-10 to G-14 (section 10.2).

## 3. The signal path, in source order

A simplified view of the path as read (statuses as the source writes them; see the MC claims for each step):

```
processor (C-008; mapped by D-001)
  global entry window ................................ OUTSIDE_ENTRY_WINDOW
  per-strategy entry window .......................... OUTSIDE_ENTRY_WINDOW
  strategy circuit breaker (when a governor is set) .. STRATEGY_CIRCUIT_BREAKER
  -> screen(..., market_data=None)
SecondaryScreener.screen()
  step 0  MIS learned-blocklist pre-drop (when enabled) REJECTED_NOT_MIS_TRADABLE only when shadow is false
  step 1  quote ...................................... SKIPPED_QUOTE_UNAVAILABLE
  enforce branch (mode enforce and a hard gate set):
          HardGate.evaluate first .................... REJECTED_<reason>, before any scored step
  otherwise:
  step 1b circuit-proximity check .................... REJECTED_CIRCUIT_PROXIMITY only if a reason
  steps 2 to 4  thresholds, signal dict, run_all
  step 4b step errors ................................ REJECTED_STEP_ERROR
  step 5  score; in shadow mode with a hard gate set, HardGate.evaluate is then logged, with no effect
  step 6  minimum score .............................. REJECTED_SCORE_<total>
  step 7  signal-age check ........................... REJECTED_SIGNAL_AGE
  step 8  PASSED
processor: on SKIPPED_ or a rejection it returns; on PASSED, step 4 (prices)
```

### 3.1 Processor checks before screen()

**MC-01** FACT (SOURCE); HISTORICAL. The processor refuses outside the global entry window with OUTSIDE_ENTRY_WINDOW `signals/signal_processor.py:909-912`, then outside the per-strategy window with the same status `signals/signal_processor.py:975-982`, then, when a governor is set, with STRATEGY_CIRCUIT_BREAKER on a reported pause `signals/signal_processor.py:984-990`; then it calls screen() with market_data=None `signals/signal_processor.py:997-1004`. These checks and the processor's other pre-screen checks are mapped by D-001 (for example M02, `docs/agenda/d-001_pre-filter/D-001_manual.md:278`), which this chapter cross-references. (Evidence: WN-02, OT-01, SC-07.)

### 3.2 screen(): step 0 and the quote

**MC-02** FACT (SOURCE). Step 0, the MIS learned-blocklist pre-drop, runs first: only when mis_filter_enabled and a blocklist is set; it rejects REJECTED_NOT_MIS_TRADABLE only when not in shadow, and in shadow "the would-drop is logged inside the helper; fall through." `screening/secondary_screener.py:142-165`. Section 6.1 treats it as a cross-reference. (Evidence: EL-01.)

**MC-03** FACT (SOURCE). Step 1: with no market data passed in, screen() fetches a quote; no quote, or a failure, gives SKIPPED_QUOTE_UNAVAILABLE `screening/secondary_screener.py:167-196`. Boundary only (D-003). (Evidence: SC-08.)

### 3.3 The enforce branch

**MC-04** FACT (SOURCE). After the quote and before step 1b, screen() takes the V3 enforce branch only when the mode is "enforce" and a hard gate is set `screening/secondary_screener.py:200-206`. Section 5 sets out what that branch does. (Evidence: SC-03.)

### 3.4 Step 1b: the conditional circuit-proximity check

**MC-05** FACT (SOURCE). When the HardGate enforce branch is not taken, screen() invokes the circuit-proximity check at step 1b `screening/secondary_screener.py:213`, through a helper that delegates to hard_gate.circuit_proximity_reason with the screener's own flag `screening/secondary_screener.py:420-431`. It rejects only if circuit_proximity_reason returns a reason: then the status is REJECTED_CIRCUIT_PROXIMITY, score 0, tier LOW, rejected_step circuit_proximity, persisted with a warning log `screening/secondary_screener.py:214-232`, before the thresholds and the step executor `screening/secondary_screener.py:234, 241, 251, 253`. The function can fail open - for example, when disabled, when entry data is missing, or when neither usable circuit band is available (section 4.5). FACT (TEXT): the comment calls it "Hard reject, not a soft score - runs before the 10-step pipeline." `screening/secondary_screener.py:211-212`. (Evidence: SC-04, HG-03.)

### 3.5 The scored steps and the shadow compare (boundary)

**MC-06** FACT (SOURCE). After step 1b: thresholds, the signal dict and the step executor's run_all `screening/secondary_screener.py:234, 241, 251, 253`; REJECTED_STEP_ERROR for step errors `screening/secondary_screener.py:265, 275`; the score `screening/secondary_screener.py:288, 290`; in shadow mode with a hard gate set, after the score and before the minimum-score check, the V3 shadow compare `screening/secondary_screener.py:305-314`; REJECTED_SCORE_<total> below the effective minimum `screening/secondary_screener.py:316-337`; REJECTED_SIGNAL_AGE when the signal_age step result is 0.0 `screening/secondary_screener.py:339-354`; PASSED `screening/secondary_screener.py:356`. Owners per OSQ-03 and OSQ-08: D-003, D-017 and D-018. (Evidence: SC-02, SC-05, SC-08.)

**MC-07** FACT (SOURCE). The scorer's ScoreResult.passed is commented "total_score >= min_pass_score", min_pass_score coming from configuration `screening/quality_scorer.py:30, 33`, and is computed that way `screening/quality_scorer.py:134`; the screener applies the strategy's min_score when above 0 and otherwise that value (MC-06). Boundary only. (Evidence: BD-04.)

### 3.6 The hand-off

**MC-08** FACT (SOURCE). A SKIPPED_ status, or a result not passed, returns from the processor's path with the screener's status standing `signals/signal_processor.py:1008-1020`; a pass goes on to step 4, entry and stop-loss price derivation `signals/signal_processor.py:1022-1030`. FACT (TEXT): "Screener writes signal status (PASSED / REJECTED_<step>). Processor does NOT double-write those statuses." `signals/signal_processor.py:993-995`. (Evidence: SC-07.)

**MC-09** INTERPRETATION. On this path, D-002's main subject is the hard-gate and eligibility-admission boundary: the HardGate result on the enforce branch (section 5), the conditional step-1b circuit-proximity check (MC-05) and the log-only HardGate verdict of the shadow compare (MC-19), with the eligibility evidence of section 6. D-002 stops at the gate result and the hand-off to downstream screening and scoring (OSQ-08: D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 24). (Evidence: none; see the text.)

## 4. The HardGate module contract (screening/hard_gate.py)

### 4.1 What the module says it is

**MC-10** FACT (TEXT). The header: "The pre-scoring binary Hard-Gate battery. For the CURRENT LIVE flow it houses only the rules relocated out of the score: at-circuit, circuit-proximity, and freshness (signal_age, 60s). Liquidity is a NO-OP", and the V3 playbook gates "are PLAYBOOK-shadow scope - evaluated + logged on the V3 path only, they NEVER reject a live order in this step." `screening/hard_gate.py:3-8`. It also "owns the shared re-scale helpers" `screening/hard_gate.py:10-12`, and circuit_proximity_reason "is the RELOCATED body of secondary_screener._circuit_proximity_reason" `screening/hard_gate.py:14-16`. Comment text, not behaviour. (Evidence: HG-01.)

### 4.2 The return contract: GateVerdict

**MC-11** FACT (SOURCE). GateVerdict is a frozen dataclass of three fields: passed (bool), reason (an optional string, None on a pass) and evidence (a dict) `screening/hard_gate.py:40-46`. FACT (TEXT): its docstring says "passed=False short-circuits screen() to REJECTED_<reason>." `screening/hard_gate.py:42-43`; in the source read that holds on the enforce branch only (MC-12, MC-19). (Evidence: HG-02.)

### 4.3 Reject reasons and statuses

**MC-12** FACT (SOURCE); INTERPRETATION. The three reject reasons are the strings AT_CIRCUIT, CIRCUIT_PROXIMITY and SIGNAL_AGE (GATE_AT_CIRCUIT, GATE_CIRCUIT_PROXIMITY, GATE_STALE), commented "screen() maps these to REJECTED_<reason>" `screening/hard_gate.py:31-34`. On the enforce branch a failing verdict becomes status REJECTED_{gate.reason}, score 0, tier LOW, rejected_step the reason in lower case `screening/secondary_screener.py:448-459`. INTERPRETATION: so the enforce-branch statuses are REJECTED_AT_CIRCUIT, REJECTED_CIRCUIT_PROXIMITY and REJECTED_SIGNAL_AGE. (Evidence: HG-02, SC-03.)

### 4.4 Construction and the ordered checks

**MC-13** FACT (SOURCE). HardGate takes, keyword-only, now_fn, logger, freshness_max_sec (default DEFAULT_FRESHNESS_MAX_SEC, 60.0) and circuit_proximity_reject_enabled (default True) `screening/hard_gate.py:37, 123-134`. main.py builds it unconditionally, freshness_max_sec from app_config.scoring.v3_freshness_max_sec and the flag from app_config.system.entry_gate.circuit_proximity_reject_enabled `main.py:3241-3247`, and injects it into SecondaryScreener with scoring_config=app_config.scoring `main.py:3281-3298`. FACT (TEXT): "Consumed by the screener only when scoring.v3_hardgate_mode != off (default off -> dormant); built always so shadow/enforce need no re-wiring." `main.py:3238-3240`. (Evidence: HG-04, WS-01.)

**MC-14** FACT (SOURCE). evaluate(*, trigger_price, triggered_at, direction, market_data) returns a GateVerdict after these checks, in this order, the first failure returning: (1) at-circuit - circuit_state upper_circuit or lower_circuit gives (False, AT_CIRCUIT) `screening/hard_gate.py:145-148`; (2) circuit proximity - circuit_proximity_reason with the constructor flag gives (False, CIRCUIT_PROXIMITY) `screening/hard_gate.py:150-155`; (3) freshness - when triggered_at is given, an age above freshness_max_sec seconds gives (False, SIGNAL_AGE), and a missing triggered_at admits `screening/hard_gate.py:157-168`; (4) "liquidity = NO-OP" - GateVerdict(True, None, {}) `screening/hard_gate.py:170-171`. Each call increments a telemetry counter `screening/hard_gate.py:137, 144`. (Evidence: HG-04.)

**MC-15** FACT (TEXT); UNKNOWN / NOT ESTABLISHED. The class docstring says "evaluate() never raises." `screening/hard_gate.py:120-121`; the method holds no try block `screening/hard_gate.py:139-171`, so the claim rests on its inputs and is not verified. (Evidence: HG-04.)

### 4.5 circuit_proximity_reason and its fail-open conditions

**MC-16** FACT (SOURCE). circuit_proximity_reason(trigger_price, direction, market_data, *, enabled=True) returns a reason string (reject) or None (admit) `screening/hard_gate.py:53-55, 112`. It returns None - admits - when the flag is off `screening/hard_gate.py:70-71`, and when there is no usable entry or neither usable band: it keeps upper_circuit, lower_circuit and the trigger price only when each is a positive number `screening/hard_gate.py:79-83` (FACT (TEXT): the docstring calls this "Fail-open" `screening/hard_gate.py:66-67`). The margin is DEFAULT_CIRCUIT_MARGIN_PCT from orders.price_math `screening/hard_gate.py:29, 85-87`, commented as the "same constant the post-fill placeability gate uses" `screening/hard_gate.py:26-27`; orders/price_math.py was not read (G-13). LONG or BUY: reject when the entry is at or above the upper ceiling (no profitable target fits the band) or at or below the lower floor (no valid stop fits); SHORT: the mirror `screening/hard_gate.py:88-111`. (Evidence: HG-03.)

### 4.6 Components noted for the boundary only

**MC-17** FACT (SOURCE); FACT (TEXT). The re-scale helpers rescaled_total, tier_for and rescale_min_score `screening/hard_gate.py:178-212` are scoring, not gating (D-003, D-017; OSQ-03); rescale_min_score says "All 15 strategies are currently 0, so this is inert today." `screening/hard_gate.py:207-209`. (Evidence: HG-05.)

**MC-18** FACT (SOURCE); FACT (TEXT). The playbook gate functions gate_rr, gate_htf, gate_extreme, gate_confirm and gate_pullback are each annotated to return a GateVerdict `screening/hard_gate.py:240, 249, 297, 304, 340, 369, 380, 422, 431`, with reason names `screening/hard_gate.py:231-233` and `screening/hard_gate.py:365-366`. Their comments: the generic three are what "Step 10a runs in SHADOW over live signals", and "on the V3 path, LOG-ONLY: the caller records the verdict and NEVER rejects/delays/alters a live order" `screening/hard_gate.py:215-223`; the confirm and pullback gates "evaluate ONLY for a v3_playbook candidate" `screening/hard_gate.py:356, 358-362`. Their bodies were not read; the detail belongs to J-010 and the route to C-011 (OSQ-04, OSQ-09; section 8). (Evidence: HG-06.)

## 5. Modes: enforce, shadow and off

| | Enforce branch | Any other case (mode shadow or off, or no hard gate set) |
|---|---|---|
| Condition | mode "enforce" and a hard gate set `screening/secondary_screener.py:200-206` | otherwise |
| HardGate.evaluate | first, before any scored step; a failure gives REJECTED_<reason> `screening/secondary_screener.py:443-459` | shadow mode with a hard gate set: after the score, logged only `screening/secondary_screener.py:305-314`; mode off, or no hard gate set: no call (each call needs its mode and a hard gate set) |
| Step 1b circuit proximity | not run (the battery has its own circuit-proximity check) | run; rejects only if a reason is returned `screening/secondary_screener.py:213` |
| Gate-owned steps | excluded from the scored steps `screening/secondary_screener.py:472-476` | all steps run |
| Signal-age check after scoring | none, "freshness is now the gate" `screening/secondary_screener.py:516-528` | step 7: REJECTED_SIGNAL_AGE when the signal_age step is 0.0 `screening/secondary_screener.py:339-354` |
| Minimum score | rescale_min_score of the strategy's min_score, else v3_min_pass_score `screening/secondary_screener.py:500-514` | the strategy's min_score when above 0, else the scorer's min_pass_score `screening/secondary_screener.py:316-337` |

**MC-19** FACT (SOURCE); FACT (TEXT). On the enforce branch HardGate.evaluate runs first `screening/secondary_screener.py:443-447`; survivors run the step executor without the gate-owned steps `screening/secondary_screener.py:472-476` - run_all's docstring: "In v3 mode the screener passes {circuit_check, signal_age} so only the 8 scored steps run." `screening/step_executor.py:129-130` - and are scored on the re-scaled total and V3 tier `screening/secondary_screener.py:496-498`. In shadow mode, when a HardGate instance is set, screen() runs the shadow comparison after scoring: _log_v3_shadow_compare runs HardGate.evaluate and logs the old score and tier against the new with the gate's reason or PASS `screening/secondary_screener.py:530-552`; it returns nothing and screen() uses no result from it `screening/secondary_screener.py:311-314`, so the result does not change the existing screening decision. If no HardGate instance is set, HardGate.evaluate cannot run through this path (the constructor's default is None, MC-20). FACT (TEXT): "The LIVE decision below is UNCHANGED (follows OLD); this is a pure side-effect for the old-vs-new soak compare." `screening/secondary_screener.py:305-307`. (Evidence: SC-03, SC-05, BD-03.)

**MC-20** FACT (SOURCE). The screener's constructor defaults: hard_gate None and scoring_config None `screening/secondary_screener.py:78-79`; the mode from scoring_config.v3_hardgate_mode, default off `screening/secondary_screener.py:95-96`; the gate-owned steps default circuit_check and signal_age `screening/secondary_screener.py:97-99`; the V3 thresholds default 50, 75 and 56 `screening/secondary_screener.py:100-102`. FACT (TEXT): when the mode is off "(or no scoring_config is injected, as in most tests) NONE of the v3 code runs" `screening/secondary_screener.py:92-94`. (Evidence: SC-01.)

**MC-21** FACT (CONFIG). Repository values: v3_hardgate_mode: "shadow", commented "SHADOW 12-Jul: compute+log OLD-vs-NEW; LIVE follows OLD (byte-identical)" `config/scoring_weights.yaml:39`; v3_gate_steps [circuit_check, signal_age] `config/scoring_weights.yaml:40`; v3_freshness_max_sec 60.0 `config/scoring_weights.yaml:41`; V3 thresholds 50, 75 and 56 `config/scoring_weights.yaml:42-44`; entry_gate.circuit_proximity_reject_enabled: true `config/system_config.yaml:661`. These are repository values, not deployed truth. (Evidence: WS-02, WS-03.)

**MC-22** INTERPRETATION; UNKNOWN / NOT ESTABLISHED. On the repository values (mode shadow, MC-21) and the repository wiring (main.py builds a HardGate and passes it to the screener, MC-13), the enforce branch is not taken and HardGate.evaluate runs only in the shadow compare, so the battery logs only; the step-1b circuit-proximity check can reject when the function's price and band conditions are met. Repository configuration is not proof of deployed or runtime state; the deployed values were not read. HISTORICAL: the same reading as D-001's H-06 (`docs/agenda/d-001_pre-filter/D-001_manual.md:214`). (Evidence: WS-04.)

**MC-23** FACT (SOURCE); FACT (TEXT); HISTORICAL. Signal-age rules are distinct and are not merged here: the HardGate freshness check (60 s, MC-14); the executor's step 10, scoring 1.0 up to 30 s, 0.5 up to 60 s, 0.0 beyond and 0.5 when triggered_at is missing `screening/step_executor.py:406-423`, which the screener's step 7 turns into REJECTED_SIGNAL_AGE at 0.0 (MC-06); and the processor's own earlier expiry check `signals/signal_processor.py:914-918` (HISTORICAL: D-001 and BK-55, `docs/governance/V2_DESIGN_LOG.md:588`). The scoring file's comment on the step-10 weight says "(now - triggered_at <= 90s, P9a)" `config/scoring_weights.yaml:20` against the code's 60 s: an observation for D-003. (Evidence: BD-02.)

## 6. Eligibility evidence

### 6.1 The MIS learned blocklist (cross-reference only)

**MC-24** FACT (SOURCE); FACT (CONFIG); INTERPRETATION; HISTORICAL. Step 0 (MC-02) asks the helper _is_mis_blocked_decision, which acts only when the resolved product is MIS (fail-open on resolver errors), asks the blocklist is_blocked, logs the would-drop with the mode and returns True in both modes `screening/secondary_screener.py:554-581`; is_blocked is true when the symbol was blocked within the re-test TTL, fail-open on a corrupt date `core/mis_blocklist.py:102-114`. main.py builds the blocklist with ttl_days from mis_filter `main.py:3055-3058` and passes it and both switches to the screener `main.py:3290-3292`. Repository values: enabled true, shadow true, ttl_days 1 `config/system_config.yaml:179-181`. INTERPRETATION: on the repository values the pre-drop logs only. Owners as recorded: D-003 and G-007, by C-004 section 10.3 (`docs/agenda/c-004_validation/C-004_manual.md:273`); D-002 cross-refers and takes no ownership (OSQ-05). (Evidence: EL-01.)

**MC-25** FACT (TEXT); HISTORICAL. The blocklist module's docstring says no "clean/stable/authoritative machine-readable MIS-eligibility source exists: Zerodha has no API for it; the list is RMS-dynamic intraday", and that the module therefore learns from the broker's own HTTP-400 `core/mis_blocklist.py:5-9`; it states a default re-test TTL of 5 calendar days `core/mis_blocklist.py:12-13`, against the repository value of 1 (comment drift, for its owners). HISTORICAL: DR-032 (DECIDED) names D-002 for "the eligibility pre-check mechanism" (`docs/governance/V2_DECISION_REGISTER.md:43`); that is a V2 requirement, not legacy evidence. (Evidence: EL-02.)

### 6.2 The F&O ban list (bounded to the authorised files)

**MC-26** FACT (TEXT); FACT (SOURCE); FACT (CONFIG). scripts/fetch_fno_ban.py states its purpose, "Fetch today's NSE F&O ban list and store it in the DB.", that "Cash equity (NSE-EQ) trades are unaffected." and that it runs daily at 08:35 IST `scripts/fetch_fno_ban.py:5-10`. It stores the symbols in a table fno_ban `scripts/fetch_fno_ban.py:171-194` and offers is_symbol_fno_banned(store, symbol, date) `scripts/fetch_fno_ban.py:220-237`, with a fetch-failed sentinel used only when fno_ban.fail_closed is on `scripts/fetch_fno_ban.py:197-217`. Its note: "The ban list currently has NO runtime consumer that gates NSE-EQ entries (is_symbol_fno_banned() is referenced only by tests)." `scripts/fetch_fno_ban.py:29-31` (comment text). Repository value: fail_closed false, "FAIL-OPEN: ban-fetch failure does NOT block NSE-EQ; WARN only" `config/system_config.yaml:748-750`. (Evidence: EL-03.)

**MC-27** FACT (COUNT); INTERPRETATION; UNKNOWN / NOT ESTABLISHED. Search Q04 over the 16 authorised files: only fetch_fno_ban.py and system_config.yaml match the F&O-ban terms; the screener, the processor, main.py, the hard gate, the entry gate and the V3 and PB01 files do not. INTERPRETATION: in the authorised files the F&O ban list takes no part in admission. Whether any file outside them consumes it is UNKNOWN / NOT ESTABLISHED (G-10); this chapter makes no repository-wide absence claim. (Evidence: EL-03.)

### 6.3 The index universe (not an admission list)

**MC-28** FACT (TEXT); FACT (COUNT); INTERPRETATION. config/index_universe.yaml describes itself as "index instruments ingested for the regime-ranking feature (Phase 0, 17-Jul-2026). DATA ONLY - no regime is computed from these yet." `config/index_universe.yaml:2-3`, and its comments say a candle-fetch script resolves each entry at fetch time `config/index_universe.yaml:5-7`; its one top-level key is indices `config/index_universe.yaml:18`. Search Q05: no other authorised file names it. INTERPRETATION: within the authorised evidence it is regime data, not treated here as an admission list. (Evidence: EL-04.)

### 6.4 Other eligibility records (HISTORICAL)

**MC-29** HISTORICAL. The static exclusion list at the webhook edge stays C-004's (C-004 section 8, `docs/agenda/c-004_validation/C-004_manual.md:202`; OSQ-05). VP-27 (`docs/governance/V2_DESIGN_LOG.md:238`), pending by Rama's choice, and VP-37 (`docs/governance/V2_DESIGN_LOG.md:248`) are V2 items held as PROPOSED inputs; they are not legacy facts. (Evidence: EL-05.)

## 7. Entry-window evidence

**MC-30** FACT (SOURCE); FACT (CONFIG). The window refusals on the main path are the processor's, before screen(): the global window, is_entry_allowed(now), refusing OUTSIDE_ENTRY_WINDOW `signals/signal_processor.py:909-912`, then the per-strategy window, is_entry_allowed_for_strategy(now, strategy_obj), with the same status `signals/signal_processor.py:975-982`. is_entry_allowed is false on a trading holiday and otherwise entry_start <= t < entry_end `core/market_windows.py:143-150`; is_entry_allowed_for_strategy requires that and the strategy's own entry_start_time and entry_end_time, falling back to the global result when those are missing or malformed `core/market_windows.py:152-175`. Repository values: entry_start "10:00", whose comment calls the value "a FLOOR OVER the per-strategy windows" `config/system_config.yaml:27-29`; entry_end "15:00" and eod_entry_cutoff "15:15" `config/system_config.yaml:47-48`. (Evidence: WN-02.)

**MC-31** FACT (COUNT). No entry-window term (window, entry_start, entry_end, trading_hours, is_entry_allowed) occurs in screening/hard_gate.py or screening/secondary_screener.py (search Q11: 0 lines; positive control P08, the same expression over core/market_windows.py: 30 lines). Bounded to those two files: neither the hard gate nor the screener checks an entry window. (Evidence: WN-01.)

**MC-32** HISTORICAL; OPEN. D-001 maps the two processor checks as M02 (`docs/agenda/d-001_pre-filter/D-001_manual.md:278`) and M07 (`docs/agenda/d-001_pre-filter/D-001_manual.md:283`). The B-008 V2 inputs, row 4, have the window "checked at the door and again in the pipeline", owners C-008 and D-002 (`docs/agenda/b-008_market-open/B-008_v2_design_inputs.md:11`); that ownership is OPEN and is not decided here. (Evidence: WN-02.)

**MC-33** FACT (SOURCE); UNKNOWN / NOT READ. The door check (the receiver) was not among the authorised files (G-12). The processor calls the same two window APIs on two further paths `signals/signal_processor.py:2043, 2078, 2400, 2420`; those paths were not read (G-14). (Evidence: WN-02.)

## 8. The V3 and PB01 boundary

**MC-34** FACT (COUNT); FACT (SOURCE). Over the 16 authorised files: the five playbook gate names occur in hard_gate.py and the three v3_chain files (search Q03); hard_gate or HardGate in main.py, hard_gate.py, the screener and the same three files (Q01); circuit_proximity only in system_config.yaml, main.py, hard_gate.py and the screener (Q02); ".evaluate(" only in the screener, at its two calls, and in hard_gate.py's GateVerdict docstring (Q12; `screening/secondary_screener.py:444, 538`, `screening/hard_gate.py:42`). (Evidence: VC-01, SC-06.)

**MC-35** FACT (SOURCE); FACT (TEXT). v3_chain/runner.py imports the three generic gates and their names `v3_chain/runner.py:29-30`; under the comment "5. Gates (LOG-ONLY - recorded, never enforced)." it calls them, records each verdict and names a would-be verdict, WOULD_PASS_GATES or WOULD_REJECT_<name> for the first failure in the order EXTREME, HTF, RR `v3_chain/runner.py:207-226`. (Evidence: VC-02.)

**MC-36** FACT (SOURCE); FACT (TEXT). v3_chain/pb01_entry.py imports gate_confirm and gate_pullback `v3_chain/pb01_entry.py:41`; for a candidate it calls gate_pullback, then gate_confirm, and on a pass calls self._confirm `v3_chain/pb01_entry.py:213-232`, whose docstring says it hands the frozen context "to the place-free would-be runner. The runner writes a record only; it holds NO order path (G-NO-ORDER)." `v3_chain/pb01_entry.py:238-242`. (Evidence: VC-03.)

**MC-37** FACT (SOURCE); FACT (TEXT). v3_chain/pb01_runner.py calls itself "the PB-01 PLACE-FREE runner" and says "PB-01 is enabled: false, so it is rejected before the _process_one observe hook" `v3_chain/pb01_runner.py:2-7`; it says it holds no fund manager, order placer, broker, reserve or queue reference `v3_chain/pb01_runner.py:9-13`, and that it reuses screening.hard_gate's gates `v3_chain/pb01_runner.py:16-17`. It imports all five gate functions `v3_chain/pb01_runner.py:33-36`, calls them `v3_chain/pb01_runner.py:139-174` and names a would-be verdict, the first failure in the order EXTREME, HTF, RR, PULLBACK, CONFIRM `v3_chain/pb01_runner.py:176-186`. (Evidence: VC-04.)

**MC-38** INTERPRETATION; UNKNOWN / NOT ESTABLISHED. Within the files read, the V3 chain and the PB01 stages use hard_gate.py's playbook gates directly, as log-only or would-be verdicts by their own comments, and do not call HardGate.evaluate or the circuit-proximity function. What calls the runner and the PB01 stages, and whether they run on a deployed host, is UNKNOWN (G-11; J-010 and C-011; BK-57, `docs/governance/V2_DESIGN_LOG.md:590`). (Evidence: VC-05.)

## 9. Boundary and ownership

### 9.1 Mechanisms, in path order

| Mechanism | Where | Can refuse on the repository values | Owner as recorded (HISTORICAL) | D-002 relation | Claims |
|---|---|---|---|---|---|
| Global entry window | processor, before screen() | yes | C-008; D-001 M02; B-008 row 4: C-008, D-002 | cross-reference (OSQ-06); owner OPEN | MC-01, MC-30, MC-32 |
| Per-strategy entry window | processor, before screen() | yes | C-008; D-001 M07 | cross-reference | MC-01, MC-30 |
| Strategy circuit breaker | processor, before screen() | when a governor is set (its settings not read) | F topics | outside (OSQ-07) | MC-01 |
| MIS learned-blocklist pre-drop | screen() step 0 | no: logs only (shadow true) | D-003, G-007 (C-004) | cross-reference (OSQ-05) | MC-02, MC-24 |
| Quote unavailable | screen() step 1 | SKIPPED_ status | D-003 (C-004) | outside | MC-03 |
| HardGate battery, enforce | screen(), enforce branch | no: the mode is shadow | J by A-008; D-002 by the agenda name | documented (OSQ-04); owner OPEN | MC-04, MC-12 to MC-14, MC-19 |
| Circuit-proximity check | screen() step 1b, when the enforce branch is not taken | yes, only when circuit_proximity_reason returns a reason (flag true) | OWNER UNRESOLVED (D-001 M12; BK-54) | in the gate map (OSQ-02); owner OPEN | MC-05, MC-16 |
| Step errors | screen() step 4b | yes | D-003 | outside (OSQ-03) | MC-06 |
| Step 9 (circuit) and step 10 (age) scores | the step executor | they score, they do not refuse | D-003; BK-58 (D-002, D-003) | boundary reference; BK-58 OPEN | MC-23, MC-39 |
| HardGate battery, shadow | after the score, in shadow mode with a hard gate set | no: logs only | as the enforce row | documented (OSQ-04) | MC-19 |
| Minimum score | screen() step 6 | yes | D-003, D-017, D-018 | outside (OSQ-08) | MC-06, MC-07 |
| Signal-age check | screen() step 7 | yes | D-003 | outside (OSQ-03) | MC-06, MC-23 |
| PASSED and the hand-off | the processor | - | D-018 | D-002 stops before | MC-08 |
| F&O ban list | a script (daily at 08:35 IST by its docstring) and a table | no consumer in the authorised files | DR-032 names D-002 for the V2 pre-check mechanism | evidence only (OSQ-05); beyond the files UNKNOWN | MC-26, MC-27 |
| Index universe | configuration | no: regime data; no other authorised file names it | the regime feature | outside | MC-28 |
| V3 generic playbook gates | v3_chain/runner.py | log-only by its comment | J-010 | cross-reference (OSQ-04) | MC-35, MC-38 |
| PB01 confirm, pullback and would-be gates | the PB01 entry and runner | would-be by their comments | C-011, J-010 | cross-reference (OSQ-09) | MC-36 to MC-38 |
| Entry gate (watchlist release) | after screening | no: by its docstring, a release gate | C-007, C-008 | outside (OSQ-07) | MC-44 |

### 9.2 Unresolved ownership (kept open; not decided by inference)

**MC-39** FACT (SOURCE); FACT (TEXT); HISTORICAL; OPEN. BK-58 (`docs/governance/V2_DESIGN_LOG.md:591`; D-002, D-003): the executor's step 9 returns 0.0 for upper_circuit or lower_circuit and 1.0 otherwise `screening/step_executor.py:397-404`, a score, though its docstring says "Reject if upper or lower circuit (P9a add)." `screening/step_executor.py:400`; HardGate makes the same test a reject, commented "mirrors step_executor._step_9_circuit_check: hard reject" `screening/hard_gate.py:145`. INTERPRETATION, bounded to the screener lines read: on the off and shadow paths a 0.0 step 9 lowers the score and is not by itself a reject. D-001 recorded this as its G-14 (`docs/agenda/d-001_pre-filter/D-001_manual.md:350`). (Evidence: BD-01.)

**MC-40** HISTORICAL; OPEN. BK-54 (`docs/governance/V2_DESIGN_LOG.md:587`): the step-1b circuit-proximity reject has no recorded owner (D-001 M12, `docs/agenda/d-001_pre-filter/D-001_manual.md:288`); OSQ-02 puts it in D-002's factual gate map for documentation only and leaves ownership open (D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 12). (Evidence: none; see the text.)

**MC-41** HISTORICAL; OPEN. The HardGate battery: J by A-008 and D-002 by the agenda name, as D-001 M13 records them (`docs/agenda/d-001_pre-filter/D-001_manual.md:289`); OSQ-04 documents the battery in D-002 to the extent of the evidence without transferring ownership (D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 16). (Evidence: none; see the text.)

**MC-42** HISTORICAL; OPEN. BK-57 (`docs/governance/V2_DESIGN_LOG.md:590`): the PB01 wiring stays with C-011 and J-010 (OSQ-09); section 8 records which hard_gate.py functions the PB01 files call and leaves their callers UNKNOWN (G-11). (Evidence: VC-05.)

**MC-43** HISTORICAL; OPEN. B-008 V2 inputs, row 4: the window layer, owners C-008 and D-002 (MC-32); the door and pipeline split stays visible and the ownership open (OSQ-06: D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 20). (Evidence: WN-02.)

**MC-44** FACT (TEXT); FACT (COUNT); INTERPRETATION. screening/entry_gate.py's docstring describes it as holding screened signals in a price watchlist and releasing them to a downstream callback on a price trigger, a hard timeout or quote failures `screening/entry_gate.py:4-8`, and says it "Does not wire into signal_processor (Module 33's job)" `screening/entry_gate.py:30`; no hard-gate or circuit-proximity term occurs in it (Q01, Q02, Q10). INTERPRETATION: a release gate after screening, not a hard rejection gate; owners as recorded C-007 and C-008 (BK-24, `docs/governance/V2_DESIGN_LOG.md:557`). (Evidence: OT-02.)

## 10. Open items and limitations

### 10.1 Open ownership items

- BK-54: the step-1b circuit-proximity reject - OWNER UNRESOLVED (MC-40).
- The HardGate battery - J by A-008 against D-002 by the agenda name (MC-41).
- BK-58: the step-9 circuit wording - D-002 and D-003 (MC-39).
- BK-57: the PB01 wiring - C-011 and J-010 (MC-42).
- B-008 row 4: the window-layer ownership - C-008 and D-002 (MC-43).

### 10.2 Not established (Stage 1 register, G-10 to G-14; no further read made)

- G-10 - UNKNOWN / NOT ESTABLISHED: consumers of the F&O ban list outside the authorised files; the script's own note says tests only (MC-26, MC-27).
- G-11 - UNKNOWN / NOT ESTABLISHED: what calls v3_chain/runner.py and the PB01 stages, and whether they run (MC-38).
- G-12 - NOT READ: the door (receiver) window check of B-008 row 4 (MC-33).
- G-13 - NOT READ: orders/price_math.py, the circuit margin's value and the post-fill placeability gate (MC-16).
- G-14 - NOT READ: the processor's two further window-check call pairs (MC-33).

### 10.3 Other limits and observations

- Not opened: the 15 Group E records, among them the two hard-gate plan and parity records (a value-blind method for prose records is not guaranteed); the five optional tests; scripts/v3_hardgate_parity_recompute.py (its role is stated in source comments; not needed) (NR-01 to NR-03).
- Deployed and runtime state: not read; every repository value here is FACT (CONFIG) only.
- Observations on comment text, for their owners (no finding): the step-9 docstring and hard_gate.py call the at-circuit test a reject while step 9 scores it (MC-39); the scoring file's step-10 comment gives 90 s against the code's 60 s (MC-23); the blocklist docstring gives a 5-day TTL against the repository's 1 day (MC-25).
- No V2 retain, modify or remove recommendation is made here.

## 11. Cross-references

- Section C: C-004 section 8 (the static exclusion list) and section 10.3 (the MIS pre-drop, D-003 and G-007) (MC-24, MC-29); C-008 (the processor's checks and both window refusals) (MC-01, MC-30); C-007 and C-008 BK-24 (the entry gate) (MC-44); C-011 (the PB01 route) (MC-36 to MC-38, MC-42).
- Section B: B-008 V2 design inputs, row 4 (the window layer) (MC-32, MC-43).
- Section D: D-001 (frozen; M02, M07, M12, M13, H-06, G-14) (MC-01, MC-22, MC-32, MC-39 to MC-41); D-003, D-017 and D-018 (the screener's refusals, the score and the verdict) (MC-06, MC-07, MC-17).
- Section F and I topics: the strategy circuit breaker, the capital and risk gates, the kill switch (MC-01; outside D-002).
- Section J: J-010 (the V3 chain) and, by A-008, J for the battery's details (MC-35, MC-38, MC-41).
- Governance: DR-032 (MC-25), DR-067 (section 1.3), VP-27 and VP-37 (MC-29), BK-24, BK-54, BK-55, BK-57 and BK-58.

## 12. Evidence appendix

### 12.1 Inputs to this draft

| File | SHA-256 |
|---|---|
| D-002_STAGE1_SOURCE_EVIDENCE_REV1_26-OCT-2026.txt | F450EEC6FAB040DE974C86F1F41755EDC2AB994CF03BB8F9B19FB492072310D9 |
| D-002_STAGE1_BOUNDARY_MATRIX_REV1_26-OCT-2026.txt | 9EAFD078B58C017EBB9260E4D482A5C4557A9B22B0436E565C96CE3B571E8D71 |
| D-002_STAGE1_CHATGPT_REVIEW_HANDOFF_REV1_26-OCT-2026.txt | 6A579B79BF8DB7AB0EEA408B0CA66C9E32940A9F8E082EB866C267E7B2A96D15 |
| D-002_STAGE1_FILES_READ_LOG_26-OCT-2026.txt | E58739AE34CADBBC5A31B82FB80F29D618A1DCEEA6892368A069D073363E8244 |
| D-002_STAGE1_GAP_AND_BOUNDARY_REGISTER_26-OCT-2026.txt | 452E11A9576BCBF37943898D37DF48DCD595780C34BB3F66AA188EFD07BBB5AA |
| D-002_STAGE1_CHANGE_NOTE_26-OCT-2026.txt | 8566F99A46FEA31FADB72C3B2C7A55B7F11CE30F94ABA7B5565394C4B72A39EE |
| D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt | F81F98CAA7B0570AAA08A988205E4D7485FB6CF1CB6AA0B0187C744233EE5784 |
| D-002_CHATGPT_REVIEW_OF_STAGE1_EVIDENCE_26-OCT-2026.txt | FFFF12691A5F872B3F672456634F09D65FF27E6068E7422FBA3702A31CDBE9E7 |
| D-002_CLAUDE_PROMPT_CORRECTION_AND_MANUAL_DRAFT_26-OCT-2026.txt | 36AB3257149964FE04CEE72A35110474A96FBFF075D5D2E01F2D09101B0F32A1 |
| D-002_CHATGPT_REVIEW_OF_MANUAL_DRAFT_26-OCT-2026.txt | C2BEEF1A55C2E2F74F13EA4B6DD565F96332ABC8A03EDE4669737338C75788F2 |
| D-002_CLAUDE_PROMPT_MANUAL_DRAFT_PRECISION_FIX_26-OCT-2026.txt | 89EBACADABBAC7DF8F69223720BED31896B58C1D7BB3913B62BD7E6B699EC6DD |
| D-002_CHATGPT_REVIEW_OF_MANUAL_DRAFT_REV1_26-OCT-2026.txt | F0D2C0649F607F637530E1CD304F21B84D77C3B7F20AAC054DA02C3592582500 |
| D-002_CHATGPT_FREEZE_READINESS_REVIEW_26-OCT-2026.txt | 9D5A26A4C91D0DA43318FAAD3DA093477F409014F2527A94A63C479D14D53EEB |
| D-002_CLAUDE_PROMPT_CITATION_AUDIT_METADATA_FIX_26-OCT-2026.txt | 5209B0679B5B263A9039FDBCF9190AD85AB8FE3643AEF234496795E02D0C945E |
| D-002_FINAL_FREEZE_READINESS_REVIEW_26-OCT-2026.txt | 500AFA2B3ADFBC802AD77500362C29507109908CB4A85E56E27F7959547B19E6 |
| D-002_CLAUDE_PROMPT_PREPARE_FREEZE_STORAGE_CANDIDATE_26-OCT-2026.txt | 5B890D780A99E36B24C1D9FAAEC85F7EA51144CBF101264DF0D106361FB1D79A |

### 12.2 Legacy files cited (read in Stage 1 at the pin; none read for this draft)

| File | Pinned blob | Lines | Stage 1 reads |
|---|---|---|---|
| config/index_universe.yaml | 416d1eb723c5 | 29 | 029 |
| config/scoring_weights.yaml | e9d6ede4c02d | 45 | 013 |
| config/system_config.yaml | bf742ae5c81c | 754 | 014, 015, 016, 017, 018 |
| core/market_windows.py | 25625e8cd109 | 334 | 024, 025 |
| core/mis_blocklist.py | 1f91fedfbdd4 | 161 | 030, 031, 032, 033, 034 |
| main.py | 445fb0d8237a | 4294 | 019, 020 |
| screening/entry_gate.py | 46b173171bdf | 538 | 039, 040 |
| screening/hard_gate.py | a8e7e2e49675 | 467 | 001, 002, 003, 004 |
| screening/quality_scorer.py | b125e9ce3243 | 151 | 038 |
| screening/secondary_screener.py | b6f8ff7223ad | 706 | 005, 006, 007, 008, 009, 010, 011, 012 |
| screening/step_executor.py | d83b04860f0f | 424 | 035, 036, 037 |
| scripts/fetch_fno_ban.py | 8416cd9c71b1 | 379 | 026, 027, 028 |
| signals/signal_processor.py | 7fab0a02a302 | 2704 | 021, 022, 023 |
| v3_chain/pb01_entry.py | cbfd4752525f | 372 | 044, 047 |
| v3_chain/pb01_runner.py | e87a5aa96464 | 297 | 045, 048, 049 |
| v3_chain/runner.py | a782f91f2f4f | 426 | 046, 050 |

Not read: scripts/v3_hardgate_parity_recompute.py, the five optional tests and the Group E records.

### 12.3 Claim table

Every MC claim with its label, the Stage 1 evidence claims it rests on and its legacy citations (path, pinned blob, lines). The citation audit file gives the same mapping range by range, with the Stage 1 reads that printed each range.

| Claim | Label | Evidence claims | Legacy citations (path, blob: lines) |
|---|---|---|---|
| MC-01 | FACT (SOURCE); HISTORICAL | WN-02, OT-01, SC-07 | signals/signal_processor.py 7fab0a02a302: 909-912; signals/signal_processor.py 7fab0a02a302: 975-982; signals/signal_processor.py 7fab0a02a302: 984-990; signals/signal_processor.py 7fab0a02a302: 997-1004 |
| MC-02 | FACT (SOURCE) | EL-01 | screening/secondary_screener.py b6f8ff7223ad: 142-165 |
| MC-03 | FACT (SOURCE) | SC-08 | screening/secondary_screener.py b6f8ff7223ad: 167-196 |
| MC-04 | FACT (SOURCE) | SC-03 | screening/secondary_screener.py b6f8ff7223ad: 200-206 |
| MC-05 | FACT (SOURCE) | SC-04, HG-03 | screening/secondary_screener.py b6f8ff7223ad: 213; screening/secondary_screener.py b6f8ff7223ad: 420-431; screening/secondary_screener.py b6f8ff7223ad: 214-232; screening/secondary_screener.py b6f8ff7223ad: 234, 241, 251, 253; screening/secondary_screener.py b6f8ff7223ad: 211-212 |
| MC-06 | FACT (SOURCE) | SC-02, SC-05, SC-08 | screening/secondary_screener.py b6f8ff7223ad: 234, 241, 251, 253; screening/secondary_screener.py b6f8ff7223ad: 265, 275; screening/secondary_screener.py b6f8ff7223ad: 288, 290; screening/secondary_screener.py b6f8ff7223ad: 305-314; screening/secondary_screener.py b6f8ff7223ad: 316-337; screening/secondary_screener.py b6f8ff7223ad: 339-354; screening/secondary_screener.py b6f8ff7223ad: 356 |
| MC-07 | FACT (SOURCE) | BD-04 | screening/quality_scorer.py b125e9ce3243: 30, 33; screening/quality_scorer.py b125e9ce3243: 134 |
| MC-08 | FACT (SOURCE) | SC-07 | signals/signal_processor.py 7fab0a02a302: 1008-1020; signals/signal_processor.py 7fab0a02a302: 1022-1030; signals/signal_processor.py 7fab0a02a302: 993-995 |
| MC-09 | INTERPRETATION | - | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-10 | FACT (TEXT) | HG-01 | screening/hard_gate.py a8e7e2e49675: 3-8; screening/hard_gate.py a8e7e2e49675: 10-12; screening/hard_gate.py a8e7e2e49675: 14-16 |
| MC-11 | FACT (SOURCE) | HG-02 | screening/hard_gate.py a8e7e2e49675: 40-46; screening/hard_gate.py a8e7e2e49675: 42-43 |
| MC-12 | FACT (SOURCE); INTERPRETATION | HG-02, SC-03 | screening/hard_gate.py a8e7e2e49675: 31-34; screening/secondary_screener.py b6f8ff7223ad: 448-459 |
| MC-13 | FACT (SOURCE) | HG-04, WS-01 | screening/hard_gate.py a8e7e2e49675: 37, 123-134; main.py 445fb0d8237a: 3241-3247; main.py 445fb0d8237a: 3281-3298; main.py 445fb0d8237a: 3238-3240 |
| MC-14 | FACT (SOURCE) | HG-04 | screening/hard_gate.py a8e7e2e49675: 145-148; screening/hard_gate.py a8e7e2e49675: 150-155; screening/hard_gate.py a8e7e2e49675: 157-168; screening/hard_gate.py a8e7e2e49675: 170-171; screening/hard_gate.py a8e7e2e49675: 137, 144 |
| MC-15 | FACT (TEXT); UNKNOWN / NOT ESTABLISHED | HG-04 | screening/hard_gate.py a8e7e2e49675: 120-121; screening/hard_gate.py a8e7e2e49675: 139-171 |
| MC-16 | FACT (SOURCE) | HG-03 | screening/hard_gate.py a8e7e2e49675: 53-55, 112; screening/hard_gate.py a8e7e2e49675: 70-71; screening/hard_gate.py a8e7e2e49675: 79-83; screening/hard_gate.py a8e7e2e49675: 66-67; screening/hard_gate.py a8e7e2e49675: 29, 85-87; screening/hard_gate.py a8e7e2e49675: 26-27; screening/hard_gate.py a8e7e2e49675: 88-111 |
| MC-17 | FACT (SOURCE); FACT (TEXT) | HG-05 | screening/hard_gate.py a8e7e2e49675: 178-212; screening/hard_gate.py a8e7e2e49675: 207-209 |
| MC-18 | FACT (SOURCE); FACT (TEXT) | HG-06 | screening/hard_gate.py a8e7e2e49675: 240, 249, 297, 304, 340, 369, 380, 422, 431; screening/hard_gate.py a8e7e2e49675: 231-233; screening/hard_gate.py a8e7e2e49675: 365-366; screening/hard_gate.py a8e7e2e49675: 215-223; screening/hard_gate.py a8e7e2e49675: 356, 358-362 |
| MC-19 | FACT (SOURCE); FACT (TEXT) | SC-03, SC-05, BD-03 | screening/secondary_screener.py b6f8ff7223ad: 443-447; screening/secondary_screener.py b6f8ff7223ad: 472-476; screening/step_executor.py d83b04860f0f: 129-130; screening/secondary_screener.py b6f8ff7223ad: 496-498; screening/secondary_screener.py b6f8ff7223ad: 530-552; screening/secondary_screener.py b6f8ff7223ad: 311-314; screening/secondary_screener.py b6f8ff7223ad: 305-307 |
| MC-20 | FACT (SOURCE) | SC-01 | screening/secondary_screener.py b6f8ff7223ad: 78-79; screening/secondary_screener.py b6f8ff7223ad: 95-96; screening/secondary_screener.py b6f8ff7223ad: 97-99; screening/secondary_screener.py b6f8ff7223ad: 100-102; screening/secondary_screener.py b6f8ff7223ad: 92-94 |
| MC-21 | FACT (CONFIG) | WS-02, WS-03 | config/scoring_weights.yaml e9d6ede4c02d: 39; config/scoring_weights.yaml e9d6ede4c02d: 40; config/scoring_weights.yaml e9d6ede4c02d: 41; config/scoring_weights.yaml e9d6ede4c02d: 42-44; config/system_config.yaml bf742ae5c81c: 661 |
| MC-22 | INTERPRETATION; UNKNOWN / NOT ESTABLISHED | WS-04 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-23 | FACT (SOURCE); FACT (TEXT); HISTORICAL | BD-02 | screening/step_executor.py d83b04860f0f: 406-423; signals/signal_processor.py 7fab0a02a302: 914-918; config/scoring_weights.yaml e9d6ede4c02d: 20 |
| MC-24 | FACT (SOURCE); FACT (CONFIG); INTERPRETATION; HISTORICAL | EL-01 | screening/secondary_screener.py b6f8ff7223ad: 554-581; core/mis_blocklist.py 1f91fedfbdd4: 102-114; main.py 445fb0d8237a: 3055-3058; main.py 445fb0d8237a: 3290-3292; config/system_config.yaml bf742ae5c81c: 179-181 |
| MC-25 | FACT (TEXT); HISTORICAL | EL-02 | core/mis_blocklist.py 1f91fedfbdd4: 5-9; core/mis_blocklist.py 1f91fedfbdd4: 12-13 |
| MC-26 | FACT (TEXT); FACT (SOURCE); FACT (CONFIG) | EL-03 | scripts/fetch_fno_ban.py 8416cd9c71b1: 5-10; scripts/fetch_fno_ban.py 8416cd9c71b1: 171-194; scripts/fetch_fno_ban.py 8416cd9c71b1: 220-237; scripts/fetch_fno_ban.py 8416cd9c71b1: 197-217; scripts/fetch_fno_ban.py 8416cd9c71b1: 29-31; config/system_config.yaml bf742ae5c81c: 748-750 |
| MC-27 | FACT (COUNT); INTERPRETATION; UNKNOWN / NOT ESTABLISHED | EL-03 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-28 | FACT (TEXT); FACT (COUNT); INTERPRETATION | EL-04 | config/index_universe.yaml 416d1eb723c5: 2-3; config/index_universe.yaml 416d1eb723c5: 5-7; config/index_universe.yaml 416d1eb723c5: 18 |
| MC-29 | HISTORICAL | EL-05 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-30 | FACT (SOURCE); FACT (CONFIG) | WN-02 | signals/signal_processor.py 7fab0a02a302: 909-912; signals/signal_processor.py 7fab0a02a302: 975-982; core/market_windows.py 25625e8cd109: 143-150; core/market_windows.py 25625e8cd109: 152-175; config/system_config.yaml bf742ae5c81c: 27-29; config/system_config.yaml bf742ae5c81c: 47-48 |
| MC-31 | FACT (COUNT) | WN-01 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-32 | HISTORICAL; OPEN | WN-02 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-33 | FACT (SOURCE); UNKNOWN / NOT READ | WN-02 | signals/signal_processor.py 7fab0a02a302: 2043, 2078, 2400, 2420 |
| MC-34 | FACT (COUNT); FACT (SOURCE) | VC-01, SC-06 | screening/secondary_screener.py b6f8ff7223ad: 444, 538; screening/hard_gate.py a8e7e2e49675: 42 |
| MC-35 | FACT (SOURCE); FACT (TEXT) | VC-02 | v3_chain/runner.py a782f91f2f4f: 29-30; v3_chain/runner.py a782f91f2f4f: 207-226 |
| MC-36 | FACT (SOURCE); FACT (TEXT) | VC-03 | v3_chain/pb01_entry.py cbfd4752525f: 41; v3_chain/pb01_entry.py cbfd4752525f: 213-232; v3_chain/pb01_entry.py cbfd4752525f: 238-242 |
| MC-37 | FACT (SOURCE); FACT (TEXT) | VC-04 | v3_chain/pb01_runner.py e87a5aa96464: 2-7; v3_chain/pb01_runner.py e87a5aa96464: 9-13; v3_chain/pb01_runner.py e87a5aa96464: 16-17; v3_chain/pb01_runner.py e87a5aa96464: 33-36; v3_chain/pb01_runner.py e87a5aa96464: 139-174; v3_chain/pb01_runner.py e87a5aa96464: 176-186 |
| MC-38 | INTERPRETATION; UNKNOWN / NOT ESTABLISHED | VC-05 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-39 | FACT (SOURCE); FACT (TEXT); HISTORICAL; OPEN | BD-01 | screening/step_executor.py d83b04860f0f: 397-404; screening/step_executor.py d83b04860f0f: 400; screening/hard_gate.py a8e7e2e49675: 145 |
| MC-40 | HISTORICAL; OPEN | - | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-41 | HISTORICAL; OPEN | - | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-42 | HISTORICAL; OPEN | VC-05 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-43 | HISTORICAL; OPEN | WN-02 | none (HISTORICAL, COUNT or INTERPRETATION only) |
| MC-44 | FACT (TEXT); FACT (COUNT); INTERPRETATION | OT-02 | screening/entry_gate.py 46b173171bdf: 4-8; screening/entry_gate.py 46b173171bdf: 30 |

### 12.4 V2 records and inputs cited (HISTORICAL)

V2 records at e163529: `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:293`; `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:292`; `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:294`; `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:308`; `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:309`; `docs/governance/V2_DECISION_REGISTER.md:78`; `docs/agenda/d-001_pre-filter/D-001_manual.md:278`; `docs/agenda/d-001_pre-filter/D-001_manual.md:214`; `docs/governance/V2_DESIGN_LOG.md:588`; `docs/agenda/c-004_validation/C-004_manual.md:273`; `docs/governance/V2_DECISION_REGISTER.md:43`; `docs/agenda/c-004_validation/C-004_manual.md:202`; `docs/governance/V2_DESIGN_LOG.md:238`; `docs/governance/V2_DESIGN_LOG.md:248`; `docs/agenda/d-001_pre-filter/D-001_manual.md:283`; `docs/agenda/b-008_market-open/B-008_v2_design_inputs.md:11`; `docs/governance/V2_DESIGN_LOG.md:590`; `docs/governance/V2_DESIGN_LOG.md:591`; `docs/agenda/d-001_pre-filter/D-001_manual.md:350`; `docs/governance/V2_DESIGN_LOG.md:587`; `docs/agenda/d-001_pre-filter/D-001_manual.md:288`; `docs/agenda/d-001_pre-filter/D-001_manual.md:289`; `docs/governance/V2_DESIGN_LOG.md:557`.

Governing inputs: D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 10; D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 24; D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 12; D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 16; D-002_CHATGPT_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_26-OCT-2026.txt, line 20.

### 12.5 Freeze

- Frozen 2026-10-26 (DR-068) under ChatGPT's final freeze-readiness review (D-002_FINAL_FREEZE_READINESS_REVIEW_26-OCT-2026.txt: "APPROVED FOR DOCUMENTATION FREEZE, SUBJECT TO A CONTROLLED FREEZE/STORAGE PACKAGE"), as factual legacy documentation only: no V2 design decision and no claim about deployed or runtime behaviour. At the freeze the five ownership items stay OPEN (section 10.1) and G-10 to G-14 UNKNOWN / NOT ESTABLISHED or NOT READ (section 10.2); G-10 to G-14 and the step-10 and blocklist-TTL observations of section 10.3 are also stored as bookmarks BK-59 to BK-63; the step-9 observation is BK-58, which carries a tracking note, as BK-54 does (V2_DESIGN_LOG section 7); all are OPEN; no owner moved; no erratum to any frozen manual; no V2 design-inputs file. The manual is stored as D-002_manual.md (W-3); the course of the topic and the package are in D-002_review_notes.md.
- Stored unchanged beside it in docs/agenda/d-002_hard-rejection-gates/: the citation audit of this frozen text (D-002_MANUAL_CITATION_AUDIT_FREEZE_26-OCT-2026.txt), the freeze change note and final check (D-002_FREEZE_CHANGE_NOTE_26-OCT-2026.txt and D-002_FREEZE_FINAL_CHECK_26-OCT-2026.txt), M1's change note and final check (D-002_MANUAL_DRAFT_REV1_METADATA_CHANGE_NOTE_26-OCT-2026.txt and D-002_MANUAL_DRAFT_FINAL_CHECK_REV1_METADATA_26-OCT-2026.txt), and the Stage 1 package it cites: D-002_STAGE1_SOURCE_EVIDENCE_REV1_26-OCT-2026.txt, D-002_STAGE1_FILES_READ_LOG_26-OCT-2026.txt, D-002_STAGE1_BOUNDARY_MATRIX_REV1_26-OCT-2026.txt, D-002_STAGE1_GAP_AND_BOUNDARY_REGISTER_26-OCT-2026.txt, D-002_STAGE1_CHANGE_NOTE_26-OCT-2026.txt, D-002_STAGE1_FINAL_CHECK_REV1_26-OCT-2026.txt and D-002_CORRECTION_AND_MANUAL_DRAFT_CHANGE_NOTE_26-OCT-2026.txt.
- Stored unchanged in docs/inputs/: draft 1, revision 1 and M1 of this manual in docs/inputs/manuals/ (D-002_HARD_REJECTION_GATES_REV0_26-OCT-2026.md, D-002_HARD_REJECTION_GATES_REV1_26-OCT-2026.md and D-002_HARD_REJECTION_GATES_REV1_M1_26-OCT-2026.md; draft 1 was delivered as D-002_HARD_REJECTION_GATES_MANUAL_DRAFT_26-OCT-2026.md, revision 1 and M1 both as D-002_HARD_REJECTION_GATES_MANUAL_DRAFT_REV1_26-OCT-2026.md); the Stage 0 files, Stage 1 revision 0, the superseded audits, change notes, final checks and review handoffs, and ChatGPT's Stage 0 and Stage 1 prompts in docs/inputs/chatgpt_originals_and_replies/; ChatGPT's reviews and the later prompts in docs/inputs/reconciliation/.
