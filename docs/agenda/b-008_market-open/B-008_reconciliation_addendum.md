# B-008 MARKET-OPEN OPERATION: RECONCILIATION ADDENDUM (revision of 20-Oct-2026)

Status: DRAFT, with the B-008 manual. It corrects ChatGPT's reconciliation of 19-Oct-2026 (docs/inputs/reconciliation/B-008_RECONCILIATION_FOR_CLAUDE_19-OCT-2026.txt), which stays unchanged as an input. Asked for by ChatGPT's value-added review of 20-Oct-2026 (its section 3).

1. Source pinning: all code evidence is trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb. The main.py object ID in the 19-Oct file identifies one file's content, not a commit.
2. Pre-10:00 alerts: on the intraday webhook path an alert outside the global entry window is refused with 403 "Outside entry window" before its body is parsed (signals/webhook_receiver.py lines 580 to 583); no signal row is stored and nothing is queued; the webhook audit row is written for every request (lines 529 to 534). Later stages re-check the window (signals/signal_processor.py lines 911, 977, 2043, 2400). B-008_manual.md section 3.
3. Strategies: 16 files in config/strategies/ at the baseline; the 24-Sep briefing shows 15 rows, without pb01_breakout_retest. This does not show which strategies were enabled, eligible or meant to trade.
4. Open questions: OQ4 ANSWERED for the intraday webhook path (other routes untraced); OQ5 ANSWERED AT INVENTORY LEVEL; OQ6 answered by B-007 (FROZEN, DR-044) to the extent of its section 7: the pre-flight was alert-only; OQ1 to OQ3 and OQ7 to OQ9 stay OPEN.
5. Status: B-005 FROZEN (DR-041), B-006 FROZEN (DR-043), B-007 FROZEN (DR-044); not "parked" or "separate drafts".
6. Generated answers removed or reclassified (Rama's sheet): NFO options, CE/PE and option chains (Rama's correction; no source); idling on holidays (Rama's correction; holiday guard exits 0); ticks and candles from 09:15 (nothing subscribed up to 25-Jul, B-007 section 9); every failed check calling sys.exit(1) (B-007 section 5); browser OAuth at 08:30 (earlier stage only); incidents with HTTP 429, WebSocket drops, SQLite locks and restarts (no record found).
7. No account identifier, credential or token appears here or in the B-008 files.
