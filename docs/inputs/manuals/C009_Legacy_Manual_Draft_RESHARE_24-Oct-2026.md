# C-009 — Signal Expiry (Legacy Production System)

**Status:** Draft for Claude factual review  
**Evidence cut-off:** 24-Oct-2026  
**Repository:** `trading-system-prod-snapshot`  
**Pinned baseline:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
**Scope:** Signal-age expiry at webhook intake and signal processing. This chapter documents the dead legacy system as evidenced; it does not repair, tune, revive, or redesign it.

## 1. Purpose and scope

This chapter records the two expiry checks established by the authorised source excerpts: receiver-side expiry that can return `EXPIRED`, and processor-side expiry that can reject a queued signal as `REJECTED_EXPIRED`. It also records the available configuration and test-text evidence, the bounded comparison with the 18-Sep documentation, and the remaining unknowns.

**Out of scope:** scanner signal-age scoring; PB01 candidate expiry; broker-token/session expiry; order-state expiry; cache TTL; GTT lifecycle; EOD cleanup of stale signals; and any V2 design decision. Similar terminology does not make these mechanisms the same feature.

## 2. Evidence and confidence rules

The primary evidence package is `C-009_CLAUDE_STAGE1_S2-19_S2-20_P2_RESULTS_AND_COMPARISON_24-OCT-2026.txt`. It records eight bounded reads, reads 028–035, from the pinned production source and two documentation chapters. The source excerpts were masked; tests were read as text and not executed.

Use these evidence categories:
- **FACT — PINNED SOURCE:** behaviour directly supported by the approved production-source lines.
- **FACT — TEST TEXT:** what a test’s source code sets up or asserts, not proof that production behaviour occurred.
- **FACT — DOCUMENT TEXT:** what a separate manual says about its own worktree.
- **HISTORICAL:** context tied to the testing worktree or previous investigation.
- **INTERPRETATION:** a limited reading of the evidence, explicitly not a runtime conclusion.
- **KNOWN DEFECT:** only a defect established by evidence; none newly established by this batch.
- **UNKNOWN / NOT ESTABLISHED:** evidence is insufficient to assert the point.

The 18-Sep manual describes testing worktree `e7bf477`, not the pinned production tree. Its line numbers must not be compared directly with production source line numbers.

## 3. Components and source references

| Component | Evidence reference | What the approved evidence establishes |
|---|---|---|
| Webhook receiver | `signals/webhook_receiver.py`, lines 916–926 | An age check based on `triggered_at`; if `age_sec > expiry_sec`, the per-symbol result is `EXPIRED`. |
| Webhook receiver | `signals/webhook_receiver.py`, lines 960–961 | `expires_at` is prepared as `received_at + expiry_sec`. This calculation is distinct from the age check. |
| Signal processor | `signals/signal_processor.py`, lines 917–919 | If `age_sec > _signal_expiry_sec`, the processor raises `_PipelineReject("EXPIRED")`. |
| Signal processor | `signals/signal_processor.py`, lines 1201 onward | Rejection status is stored using `REJECTED_<check>`; the test text asserts `REJECTED_EXPIRED` for the expiry case. |
| Signal processor | `signals/signal_processor.py`, lines 1221–1229 | If a notifier exists, an EXPIRED warning is rate-limited to at most once per 60 seconds; the message includes the mode label. |
| Configuration model | `core/config_loader.py`, lines 192–197 | `SignalQueueConfig` contains an `expiry_sec` field. The approved comparison does not establish the deployed effective value. |

These references are the bounded line references recorded in the evidence package. Do not infer un-read surrounding control flow.

## 4. Receiver-side expiry

### 4.1 Input and decision

**FACT — PINNED SOURCE:** the approved source comparison identifies the receiver’s age calculation as being based on `triggered_at`. When `age_sec > expiry_sec`, the receiver returns a per-symbol result with status `EXPIRED` (source lines 916–926).

The evidence supports the strict greater-than comparison shown above. It does not support replacing it with greater-than-or-equal, nor does it establish all date/time parsing and clock-handling details beyond the approved lines.

### 4.2 Stored expiry timestamp

**FACT — PINNED SOURCE:** source lines 960–961 prepare `expires_at` as `received_at + expiry_sec`.

This timestamp calculation uses `received_at`, while the approved age check uses `triggered_at`. Those are different bases. The evidence does not establish that the `expires_at` column is later read to make an expiry decision.

### 4.3 Output and downstream handoff

The supported receiver outcome is a per-symbol `EXPIRED` status. The test text additionally asserts an HTTP 200 response and a `results` array, but the corresponding outer response-construction lines were not part of the approved source excerpts; those response details therefore remain **UNKNOWN / NOT ESTABLISHED at the pinned source** in this chapter.

The broader receiver sequence described in the 18-Sep manual includes request gates, price validation, in-flight claim, TTL deduplication, fingerprinting, persistence and queueing. Only the expiry-related portion of that sequence is within this chapter’s authorised evidence. The relative positions of the other steps at the pinned production commit are not established here.

## 5. Processor-side expiry

### 5.1 Input and decision

**FACT — PINNED SOURCE:** the approved comparison identifies a processor check that raises `_PipelineReject("EXPIRED")` when `age_sec > _signal_expiry_sec` (source lines 917–919).

The approved comparison reports that the processor’s rejection path stores a status using the `REJECTED_<check>` pattern (source line 1201 onward). Test text expects the resulting status `REJECTED_EXPIRED`. The tests were not run, so the observed runtime status remains unverified by test execution.

### 5.2 Warning

**FACT — PINNED SOURCE:** when the expiry rejection occurs and a notifier is available, the processor can emit a warning whose text includes the mode label and “Signal EXPIRED”. The warning is rate-limited to at most once per 60 seconds using a monotonic clock (source lines 1221–1229).

Do not generalise this into a guarantee that a notification is always delivered: the evidence states the notifier is conditional and does not establish external delivery success.

## 6. Configuration and thresholds

The approved source comparison identifies `SignalQueueConfig.expiry_sec` in `core/config_loader.py` lines 192–197. The field’s presence does not establish the effective value used by a deployed process.

The test text contains multiple explicit values:
- receiver test: `expiry=60`;
- processor test: `signal_expiry_sec=30`;
- configuration-model test: `expiry_sec=600`.

These are test inputs. They must not be presented as the deployed value. The effective deployed threshold is **UNKNOWN / NOT ESTABLISHED** from the evidence reviewed for this chapter.

## 7. Test evidence (text only)

No tests were run during this investigation. The following is a description of test source, not a test result.

| Evidence | Test text describes | What can be concluded |
|---|---|---|
| `tests/unit/test_webhook_receiver.py:940–950` | Builds a receiver with expiry 60; sends a payload whose `triggered_at` is 120 seconds old; asserts HTTP 200 and per-symbol `EXPIRED`. | The test is written to assert this result. Runtime pass/fail is unknown. |
| `tests/unit/test_signal_processor.py:572–582` | Uses a 30-second expiry, processes an aged signal, and asserts `REJECTED_EXPIRED`. | The test is written to assert this result. Runtime pass/fail is unknown. |
| `tests/unit/test_fix136_batch9.py:120–126` | Calculates `received + timedelta(seconds=600)` and compares it to a literal timestamp. | Arithmetic-only test text; it does not call the receiver’s timestamp calculation. |
| `tests/unit/test_fix136_batch9.py:128–136` | Computes an age of roughly 700 seconds and asserts that it exceeds 60. | Arithmetic-only in the lines read; it does not call the processor expiry check. |
| `tests/unit/test_fix136_batch9.py:138–145` | Computes an age of roughly 5 seconds and asserts that it is no more than 600. | Arithmetic-only in the lines read; it does not call receiver or processor expiry code. |
| `tests/unit/test_fix136_batch9.py:154–158` | Constructs `SignalQueueConfig(... expiry_sec=600 ...)` and asserts the field equals 600. | Establishes only that the test asserts the supplied value is stored in the model; its paper/live parity claim is not exercised by this body. |

**Do not state that these tests pass.** Their execution status is “not run” by design in this evidence phase.

## 8. Comparison with the 18-Sep manual

The 18-Sep `02_REPOSITORY_AND_FILE_MAP.md` describes a testing worktree, not the pinned production source.

- Its receiver description of per-signal expiry is **CONSISTENT at statement level** with the pinned excerpt.
- Its listed pipeline order is only **PARTIALLY established** at the pin: expiry before fingerprint and row preparation is supported by the approved comparison; placement of price validation, in-flight claim, TTL deduplication and request gates is not established in this evidence batch.
- Its processor description of an EXPIRED warning rate-limited to once per 60 seconds is **CONSISTENT at statement level** with the pinned excerpt. Line numbers are not comparable across the trees.
- The documented signal-age scoring step (30/60-second score bands) is a scoring mechanism, not the expiry rejection documented here.
- PB01 candidate-row expiry is a separate candidate-lifecycle rule and is outside this chapter.
- The four matched lines in `03_STARTUP_RUNTIME_AND_SCHEDULER.md` concern broker-token expiry, not signal expiry. No claim is made about unsearched wording elsewhere in that chapter.
- EOD cleanup of stale PROCESSING signals and old terminal signals belongs to C-007 and is not re-analysed here.

## 9. Open questions and known historical differences

### OQ-C1 — `SignalExpiredError`

The exception class is defined in `core/exceptions.py` according to the approved comparison. Whether any code raises it at the pinned production commit is **UNKNOWN / NOT ESTABLISHED**. The receiver’s `EXPIRED` status and the processor’s `_PipelineReject("EXPIRED")` do not prove that this exception is raised.

### OQ-C2 — consumers of `signals.expires_at`

The dashboard reader reportedly selects `expires_at` at `db_reader.py:826`. Whether any reader uses the column to decide expiry is **UNKNOWN / NOT ESTABLISHED**. Selecting a field does not prove that it drives a decision.

### Existing bookmarks

The evidence package references previously recorded bookmarks:
- **BK-23:** test text uses `received_at` for the `expires_at` calculation, while several other test bodies calculate age from `triggered_at`.
- **BK-17:** test/source text contains 60- versus 600-second thresholds.

These are documented source/test-text differences. This evidence does not establish a runtime defect or which threshold was deployed. No new known defect is established by this batch.

## 10. End-to-end view limited to established evidence

```text
Webhook receiver
    |
    | age check based on triggered_at
    | if age_sec > expiry_sec
    v
Per-symbol result: EXPIRED

Receiver also prepares:
    expires_at = received_at + expiry_sec
    |
    | downstream use of this stored timestamp is not established
    v
UNKNOWN / NOT ESTABLISHED

Signal processor (for a queued signal)
    |
    | if age_sec > _signal_expiry_sec
    v
_PipelineReject("EXPIRED")
    |
    v
Rejection status pattern: REJECTED_<check>
    |
    +--> EXPIRED warning, at most once per 60 seconds,
         only when notifier is available
```

This diagram shows only the bounded checks supported by the authorised source excerpts. It is not a claim that every signal follows both checks, nor a full queue or persistence trace.

## 11. Paper/live differences

The batch-9 test docstring claims the expiry logic is mode-agnostic, but its body only constructs a configuration object and checks that `expiry_sec` equals 600. It does not exercise both modes. Therefore, paper/live parity for signal expiry is **UNKNOWN / NOT ESTABLISHED** by this test evidence.

## 12. Failure, recovery and operational behaviour

The approved evidence supports the receiver’s expired per-symbol outcome and the processor’s expiry rejection/warning path. It does not establish the complete HTTP error envelope, retry behaviour, queue recovery, database transaction semantics, or deployed alert delivery for these paths. Those details remain outside the authorised read set and must not be inferred.

No live service, VM, database, broker, webhook or Chartink endpoint was contacted. No legacy code or test was executed and no legacy source was changed.

## 13. Evidence ledger

| ID | Classification | Evidence / conclusion |
|---|---|---|
| F-01 | FACT — PINNED SOURCE | Receiver age check uses `triggered_at`; `age_sec > expiry_sec` returns per-symbol `EXPIRED` (lines 916–926). |
| F-02 | FACT — PINNED SOURCE | Receiver prepares `expires_at = received_at + expiry_sec` (lines 960–961). |
| F-03 | FACT — PINNED SOURCE | Processor rejects when `age_sec > _signal_expiry_sec` by raising `_PipelineReject("EXPIRED")` (lines 917–919). |
| F-04 | FACT — PINNED SOURCE | Processor expiry warning is conditional on notifier and rate-limited to once per 60 seconds (lines 1221–1229). |
| F-05 | FACT — CONFIG MODEL | `SignalQueueConfig` contains `expiry_sec` (core/config_loader.py:192–197). Effective deployed value unknown. |
| T-01 | FACT — TEST TEXT | Receiver test asserts EXPIRED for a 120-second-old signal with expiry 60; not run. |
| T-02 | FACT — TEST TEXT | Processor test asserts REJECTED_EXPIRED for an aged signal with threshold 30; not run. |
| T-03 | FACT — TEST TEXT | Four batch-9 tests calculate timestamps/ages or check a model field; they do not establish runtime behaviour in the lines read. |
| H-01 | HISTORICAL | 18-Sep chapters describe testing worktree `e7bf477`, not the pinned production commit. |
| U-01 | UNKNOWN | Effective deployed signal expiry threshold. |
| U-02 | UNKNOWN | Whether `SignalExpiredError` is raised anywhere at the pin. |
| U-03 | UNKNOWN | Whether `signals.expires_at` is used by any reader to decide expiry. |
| U-04 | UNKNOWN | Runtime pass/fail of all six tests; they were not run. |
| U-05 | UNKNOWN | Paper/live parity of signal expiry. |
| K-01 | KNOWN DEFECT | None newly established by this evidence batch. |

## 14. Cross-references

- **C-003:** authentication; separate from signal-age expiry.
- **C-004:** field and symbol validation; earlier ingress gates, not re-analysed here.
- **C-005:** claims/deduplication; C-009 does not establish the complete ordering of those steps.
- **C-006:** queue; only expiry-related queue handoff is mentioned where supported.
- **C-007:** persistence and EOD cleanup; owns stale-signal cleanup and retention.
- **D topics:** signal-age scoring, screener, and PB01 candidate expiry remain separate topics.

## 15. Review status

This is a factual draft based on the supplied bounded-read results. Claude should perform a focused consistency review against the recorded evidence and pinned repository metadata, correct only demonstrable transcription or factual errors, and return the final manual and complete C-009 package. Do not add new behaviour claims or reopen settled scope. Keep OQ-C1 and OQ-C2 as open bookmarks unless a material contradiction is found in the existing authorised evidence.

**Legacy remains DEAD / REFERENCE ONLY. No V2 decisions are made here.**
