# B-012 RESTART AND RECOVERY: RECONCILIATION ADDENDUM (revision 0 of 20-Oct-2026)

Status: FROZEN 2026-10-20 with the B-012 manual (DR-049). It answers ChatGPT's reconciliation of 20-Oct-2026 (docs/inputs/reconciliation/B-012_RECONCILIATION_FOR_CLAUDE_20-OCT-2026.txt). That reconciliation and Claude's repository answers and source inventory stay unchanged as inputs. All source evidence is trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb.

1. Its confirmations (section 1, items 1.1 to 1.6): ACCEPTED.

2. Its wording adjustments A to H (section 2): ACCEPTED; all are already in its draft and kept.
   - A, B, C: they correct Claude's chat summary of 20-Oct ("systemd restarts a crash every 10 seconds with no limit"; "the watcher restarts at most 3 times an hour"). The answers file itself said "no cap found in the unit" and kept the two supervisors separate (B12-B1, B12-B5, B12-B6); the chat summary overstated both.
   - D to H: kept as drafted.

3. Items kept open (its section 3): ACCEPTED as UNKNOWN / NOT ESTABLISHED.

4. Additions by Claude in revision 0 (manual section 16): the watcher's crash branch covers exit 5; the liveness probe's window ends 15:55; the migration-refusal exit; two line ranges. All are from source already cited in the answers file; none changes a conclusion.

5. Not accepted: none. Frozen chapters: no conflict (B12-R0); no erratum proposed.

6. Status: B-001 to B-011 FROZEN; B-012 FROZEN (DR-049).

7. ChatGPT's review of 20-Oct-2026 (B-012_CHATGPT_REVIEW_AND_FREEZE_READINESS_20-OCT-2026.txt): ACCEPTED. No correction required; no change to the manual followed from it.
