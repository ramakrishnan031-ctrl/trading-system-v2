# B-009 INTRADAY OPERATION: RECONCILIATION ADDENDUM (revision 2 of 20-Oct-2026)

Status: FROZEN 2026-10-20 with the B-009 manual (DR-046). It answers ChatGPT's reconciliation of 20-Oct-2026 (docs/inputs/reconciliation/B-009_RECONCILIATION_FOR_CLAUDE_20-OCT-2026.txt) and corrects Claude's repository answers of the same day (docs/inputs/chatgpt_originals_and_replies/B-009_CLAUDE_REPOSITORY_ANSWERS_20-OCT-2026.txt). Both stay unchanged as inputs. All source evidence is trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb, rechecked on 2026-10-20 (B-009_review_notes.md section 3).

1. Corrections to Claude's repository answers (CLAUDE ERROR, corrected here)
   a. Strategy counts. "All 15 enabled intraday strategies declare CO_PLUS_TGT; the three delivery strategies declare LIMIT_TRIPLE" (answers section 5, D2) is wrong: 15 + 3 is not 15.
      - Correct: 15 files are enabled. 12 are INTRADAY and declare CO_PLUS_TGT; 3 are DELIVERY and declare LIMIT_TRIPLE.
      - pb01_breakout_retest is INTRADAY, CO_PLUS_TGT and disabled.
      - The same correction applies to the words "15 enabled strategy files" in B9-B3 and B9-E2, which are right as a file count but are not 15 intraday strategies.
      - Manual section 4.
   b. Census figures. "1,876 over the full retention window" (B9-C2) is wrong. 1,876 is the pruned-era residue, 12-Jun to 08-Jul (census line 91). 725 is the complete era, 09-Jul to 16-Jul. Manual section 13.
   c. Search scope. "325 files" is the number of non-empty files git grep matched. The scope is all 330 Python files outside tests/, every one checked out (checked against the tree listing).
   d. Eligibility. The loader does not filter on `enabled`. The flag is applied per signal by strategy control, with trade_type BOTH, force_intraday_only false and delivery_enabled true at the baseline. Manual section 4.

2. ChatGPT's points (its sections 3.1 to 3.8), all rechecked at the baseline
   - 3.1 Strategy count: ACCEPTED, with the corrected numbers in 1a. "15 enabled" is now stated as 12 intraday plus 3 delivery, eligible by configuration. Loader, control layers and config lines are cited in manual section 4.
   - 3.2 Order protocol: VERIFIED.
     - OrderPlacer constructor default (order_placer.py 572); selection (961); main.py construction (3061-3087) passes none.
     - All three place() callers (signal_processor.py 1446, 2251, 2553) pass none.
     - FullEntryEngine falls back to its default when given none (full_entry_engine.py 84-93).
     - No runtime reader of a strategy's order_protocol.
     - Kept as KNOWN DEFECT (source path), in ChatGPT's wording, with historical orders established only by records.
   - 3.3 Entry gate: VERIFIED over all 330 files. The only add() reference is the gate's own docstring; WatchEntry is otherwise built only in store rehydration; pullback_wait_enabled is read at signal_processor.py 1082 only. KNOWN DEFECT D1, with the scope stated.
   - 3.4 Order monitor:
     - Two separate counters, each at threshold 3, both reset by any successful order poll, per monitor not per order (226, 233-234, 702-703).
     - Each reaching 3 calls HARD_KILL and sets the stop event.
     - One start (main.py 3960); no in-source restart.
     - The 15:15 force-close runs inside that cycle (614-615), so it stops too.
     - Host supervisor: outside the evidence.
     - /health visibility is cross-referenced to B-007, not reopened.
   - 3.5 Feed: ACCEPTED. Wording kept as "connects; subscribes to nothing at start; only the exit retry subscribes". The dormancy record covers its own dates only.
   - 3.6 Time controls: VERIFIED from callers, not only config. MIS pollers in B-006 TH3; placement cut-off at order_placer.py 1076-1097; force-close at order_monitor.py 614-615, 794-830 with main.py 688-724; EOD poller every 5 s.
   - 3.7 Expiry: VERIFIED. 600 s from config 127 via main.py 3613-3614 (processor) and webhook_receiver.py 679 (webhook); constructor default 60 (signal_processor.py 175). It is distinct from the order monitor's 60 s fill timeout.
   - 3.8 Quotes: VERIFIED.
     - broker_adapter.get_quote is passed to six constructors (manual section 6).
     - The live branch has no partial-reply check (zerodha_adapter.py 1771-1798); the paper provider warns (main.py 552).
     - The claim is limited to that branch.

3. Records (ChatGPT section 4): ACCEPTED. Each record is stated with its date, window and deployed commit. The census funnel's window is now explicit (09-Jul to 16-Jul). Unread records stay listed as unread.

4. Question status (ChatGPT section 5.1): ACCEPTED. Manual section 16 replaces any global "answered" label with per-ID statuses.
   - Changed: B2 is not operator-only. Its source path is answered; only "what the operator saw" is operator-only, as for D1 and D3.

5. Not accepted, with reason
   - ChatGPT section 5.3 maps "order processing and position/order lifecycle" to the D series and "R:R and slippage" to the G series. In this project's agenda D owns trade decision and screening, and G owns orders, positions and execution (the answer sheet's section 1 boundary list). The manual follows the agenda.
   - ChatGPT section 3.2's fallback label (INTERPRETATION / AUDIT LEAD) is not needed: the trace is complete within the 330 files.

6. Status: B-005 to B-008 FROZEN (DR-041, DR-043, DR-044, DR-045); B-009 FROZEN (DR-046).

7. ChatGPT's value-added review of 20-Oct-2026 (revision 1). All points ACCEPTED.
   - A: D1 price and gate contents.
     - Corrected: a Group A signal's entry is the normally derived price, with the trigger as reference and no re-anchor; not the raw trigger.
     - New source fact: start() reloads gate_state rows (entry_gate.py 156, 179-235). Their only writer, add() (332), has no caller, and EOD clears them (entry_gate.py 260-279).
     - So "empty at the baseline" is replaced by "not fed by new signals". A host's gate_state contents are UNKNOWN.
   - B: D3 is limited to the monitor's own polling and event path. No other OrderFilled emitter was traced for B-009, so "seen only by the reconciler" stays an INTERPRETATION.
   - C: LIMIT_TRIPLE is scoped to the traced placement path at the baseline.
   - D: the 15:15 callback is named by its effect, tracked-entry cancellation with SOFT_KILL. "force-close" is kept as the historical source name. The placement cut-off is stated as independent of the monitor.
   - E to G: re-read; the corrected counts and census periods hold throughout; the feed is "connected, not subscribed".
   - Questions 1 to 3: ChatGPT's dispositions accepted. D1 to D3 stay KNOWN DEFECT (source path); the per-ID table stays; the twelve records stay unread.

8. ChatGPT's revision-2 corrections of 20-Oct-2026
   - Correction 1, 38 IDs: ACCEPTED.
     - The answer sheet's IDs are A1-A4, B1-B5, C1-C4, D1-D6, E1-E5, F1-F6, G1-G4 and H1-H4, which is 38.
     - Where "44" came from: the figure entered with ChatGPT's own message accompanying the answer sheet, not from the sheet. Claude's repository answers file gives no total; only Claude's chat summary repeated "44".
   - Correction 2, gap_fade_long: PARTLY ACCEPTED. The conflict stays visible, unresolved. The 11:30 is in a docstring and a comment only (market_windows.py 162; signal_processor.py 976); the code reads the YAML (market_windows.py 169-175).
   - EOD wording caution: ACCEPTED.

