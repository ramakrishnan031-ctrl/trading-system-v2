# V2 SESSION HANDOVER: D-003 -> D-004

Date: 27-Oct-2026, 15:20 IST; updated 16:10 IST after the D-003 push (this copy replaces the 15:20 one). Prepared by
Claude at the end of the D-003 chat, for a new ChatGPT window (and a new Claude chat if needed) that starts D-004
Market-data requirements.

---

## 1. Where things stand

| Topic | Status | Where |
|---|---|---|
| D-001 Pre-filter | Frozen and stored | GitHub `trading-system-v2`, `docs/agenda/d-001_pre-filter/` |
| D-002 Hard rejection gates | Frozen and stored (DR-068) | GitHub main `8591526` ("D-002 Hard rejection gates frozen (DR-068), no erratum") |
| D-003 Secondary screening | Frozen and stored (DR-069); push verified on GitHub | GitHub main `c33466b` ("D-003 Secondary screening frozen (DR-069), no erratum"); section 2 |
| D-004 Market-data requirements | Not started | Master agenda, section D: `docs/governance/MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt:295` ("D-004  Market-data requirements") |

All D-001 to D-003 files (with the rest of the agenda completed so far) are in the GitHub repo at `c33466b`. That is the
source to read, verify or cite from in D-004.

---

## 2. D-003: pushed and verified (27-Oct)

- Pushed by Rama: commit `c33466b94700eeccace555a145b88a3e49bf3196`, commit time 16:05 IST on 27-Oct, on parent
  `85915267f38c0d653cf43d1be9425b87fb946566`.
- Verified by Claude at 16:07 IST on a fresh clone. RESULT: PASS.
  - 75 files: 73 added and 2 modified (`V2_DECISION_REGISTER.md` with DR-069, `V2_DESIGN_LOG.md`).
  - Each file matches the manifest by blob, SHA-256 and size.
  - Tree `e535a4765ed180ade44c155f62b874dce41fe988`, as rehearsed.
  - The message is the subject and the Co-Authored-By line only.
  - The two D-002 records are now stored.
- How it ran: PART 0 and PART 1 passed. The first paste of PART 2 stopped two lines short, so PART 2 did not run and
  GitHub was unchanged. After Esc and Ctrl+C, PART 2 was pasted again from `D-003_PUSH_PART2_RERUN_27-OCT-2026.txt` (the
  same text) and passed.
- The frozen manual is `docs/agenda/d-003_secondary-screening/D-003_manual.md` (FROZEN 2026-10-27, DR-069). The reviewed
  draft stays `docs/inputs/manuals/D-003_SECONDARY_SCREENING_REV0_26-OCT-2026.md`, DRAFT / NOT FROZEN.

**Owed to D-004's storage package** (as D-002's records were stored with D-003)
1. The file `D-003_POST_PUSH_GITHUB_VERIFICATION_RESULT_27-OCT-2026.txt`, to
   `docs/inputs/chatgpt_originals_and_replies/` (3,114 bytes; SHA-256
   `AEF37661015BCF903CC1C76A664E2FCC28D6508EE11E0A30A8F8CF9DE5010F1B`).
2. This line, once, at the end of `V2_DESIGN_LOG.md` section 6 under `### 2026-10-27`, exactly:

```
- D-003 package pushed by Rama (c33466b, 75 files, commit time 16:05 IST on 27-Oct): PART 0 and PART 1 passed at the first run; the first paste of PART 2 stopped two lines short of its end, so PART 2 did not run and GitHub main stayed 8591526 (checked by Claude), and PART 2 pasted again in the same window passed (D-003_PUSH_PART2_RERUN_27-OCT-2026.txt, the same PART 2 text); confirmed independently by Claude at 16:07 IST on a fresh clone: GitHub main c33466b on parent 8591526, the 75 files equal to the manifest blob by blob, the tree equal to the rehearsal's (e535a476), the commit message the subject and the Co-Authored-By line only; the two D-002 records stored.
```

3. PROPOSED lesson row for `V2_DESIGN_LOG.md` section 1 (add if ChatGPT and Rama agree):

```
| 2026-10-27 | D-003's PART 2 paste stopped two lines short of its end, so PowerShell waited at its >> prompt and PART 2 did not run; Claude checked that GitHub main was unchanged and re-sent PART 2 as a file of its own, which passed. | A PART is fully pasted only when its end-of-PART comment line shows after a PS prompt. A >> prompt with nothing running means the paste stopped short: press Esc, then Ctrl+C (nothing runs), and paste the PART again, copied whole with Ctrl+A. |
```

**Base for D-004**: build D-004's package on `c33466b94700eeccace555a145b88a3e49bf3196`. Its first register number is
**DR-070**.

**Known limits of the D-003 package** (recorded; not to be reopened without new evidence)
- NOT VERIFIED:
  - K-3: the manual's content checks were not re-run on the final candidate (last partly re-run at 11:13 IST).
  - K-15: the legacy clone's current state and the remote refs.
  - (The live GitHub base and DR-069 at storage time are now settled by the push and the check in this section.)

---

## 3. What D-004 inherits from D-003

All references are to the D-003 manual (the frozen copy once stored). They are facts about the legacy system as
documented, not V2 decisions.

**The boundary row** (manual section 1, the boundary table)
- "D-004 Market-data requirements | broader market-data contracts and requirements | D-003 records the screener's
  observed inputs only (section 6)".
- Source: agenda line 295 and D-003 Stage 0 scope decision OSQ-04. It is one of D-003's five OPEN boundary rows.

**Screener inputs observed in D-003** (manual sections 3 and 6)
- MC-02: the signal processor calls the screener once per signal with `market_data=None`.
- MC-20 to MC-23: with no market data passed in, step 1 fetches a quote:
  - `quote_fn([symbol])`, wired to the Zerodha adapter's `get_quote` (MC-21);
  - the Quote fields are in MC-22;
  - `_build_market_data` builds the dict and sets `circuit_state` from the circuit limits (MC-23).
- MC-24: on the processor's path "every screened signal costs one quote request" at screen time. The screener holds no
  instrument cache (MC-06).
- MC-37 and the manual's qualification Q3: "atr, rsi, sector, prev_close and avg_volume_20d are always None" on that
  path. The volume, ATR, RSI and sector steps therefore score as dead or pinned (this answers Q-34 part (c)).
- MC-25, bookmark BK-67 to C-004 (U-03): the quote map is looked up by the symbol as received. Whether the broker returns
  the requested case is NOT ESTABLISHED.

**Bookmarks D-003 adds** (all OPEN; stored on push)

| Bookmark | Gap | Next topic |
|---|---|---|
| BK-64 | G-12 | G-007; C-004 (context) |
| BK-65 | G-14 | D-002; C-008 (context) |
| BK-66 | G-17 | D-017 |
| BK-67 | G-20 | C-004 (U-03) |
| BK-68 | observations | A-008 (MC-80) |

**D-003's seven open questions**, unchanged: BK-46, BK-54, BK-55, BK-56, BK-58, BK-63, Q-34.

**D-003's five OPEN boundary rows**:
- D-001 Pre-filter (BK-55);
- D-002 Hard rejection gates (BK-54, BK-58);
- **D-004 Market-data requirements**;
- D-014 and D-015, age rules and retest (BK-46);
- D-016 or F-010, entry gate and entry throttle (BK-56).

**Later scheduling, identified at D-003 and not made**
- Where the parts of VP-03 and VP-08 that named D-003 are ratified.
- Where the admission test (no subset or near-copy of another strategy) is written.

---

## 4. V2 design input: keep separate

Rama's direction: the future V2 system has a separate secondary screener for each strategy. Example: six strategies,
four intraday and two delivery, give six strategy-specific screeners.

This is a V2 design input only, recorded in the VP-03 tracking note (E-7). Its scoring, thresholds, inputs and outputs,
ownership and any shared safety layer are undecided. It is not a D-004 decision, and it does not change any legacy
manual.

---

## 5. Workflow and standing rules (carry into D-004)

**Roles**
- ChatGPT prepares each stage's Rama answer sheet and Claude prompt, the reconciliations and the reviews.
- Claude prepares all files.
- Rama forwards files and runs the push commands.
- Claude verifies the stored files on GitHub after each push.

**Start files**
- Every stage starts with both a Rama answer sheet and a Claude prompt.
- Claude names a missing one on its first line.

**No approval gates**
- Claude never asks Rama for approvals.
- From 27-Oct: "Assume pre-approved whenever you want to prepare final files." Committing, pushing and storing remain
  Rama's own action, through the push commands.

**Delivery**
- Review files are shared as they are, not zipped.
- Only the final storage package is one zip, with its push commands.
- Every request to Rama names the exact files.
- Where to save files is said only when everything is final.

**Legacy system**
- It is dead and reference-only: document it, never fix, run or test it.
- Bookmark cross-topic issues instead of solving them.
- Do not repeat analysis of settled legacy issues.

**Legacy access**
- Only what the written stage authorisation names: authorised files and read modes through the masked read gate.
  Count-only scans are allowed only when named.
- Never open or quote credentials, tokens or session files, `.env` files, local GUI configuration, databases or logs.
  Never open or quote the host records, which are referred to only as HR-1, HR-2 and HR-3.
- No legacy code, tests or imports run.
- No host, broker, webhook, Chartink, database or remote contact.
- The legacy pin is `970aabf3c78d777d9d34549e021f7c8e756c7bdb` (trading-system-prod-snapshot).

**Commits and lessons**
- Commit messages carry the subject and the Co-Authored-By line only; no session links anywhere in records.
- After any correction, a lesson goes into `V2_DESIGN_LOG.md` section 1.

**Status labels**
- PASS, FAIL and NOT VERIFIED for checks.
- HISTORICAL for an earlier package or result.
- CARRIED for an earlier result not re-run.
- PROPOSED for a destination, status or wording not yet in effect.
- Drafts say DRAFT / NOT FROZEN.

**Reply length**: chat replies stay short.

---

## 6. Suggestions for D-004's prompts, from D-003's extra cycles (for ChatGPT; not decisions)

D-003 needed five extra rounds after its action package (09:20 to 13:57 on 27-Oct): a verification follow-up, a
reconciliation, a label fix, a DP-9 conflict and a final pass. Most could have been avoided by stating these in the
freeze-preparation prompt from the start:

1. **Authorise the storage mechanics up front.** Allow Claude to:
   - read the local V2 copy to regenerate the expected tree and push commands;
   - run a disposable local rehearsal of the push commands, with no push to GitHub.

   Without these, the tree and the commands stay NOT VERIFIED.
2. **Settle the frozen-copy labelling once.** Either keep the reviewed draft as DRAFT / NOT FROZEN and add the frozen
   copy only in the final pass, or say in the prompt that a PROPOSED frozen copy at the manual's path is acceptable. In
   D-003 this was read two ways (FV-10).
3. **Say which records go into the package.** Name which review and follow-up records are stored with the package
   (D-003's DP-9 was answered YES, then listed as open, then confirmed).
4. **One final pass and at most one independent check.** Report only genuine blockers. Record unverifiable items as
   NOT VERIFIED instead of starting a new cycle.
5. **Build on the stored D-003 commit.** D-004's package must sit on the commit that stores D-003, with DR-070 next.

---

## 7. First actions

1. **Done:** the D-003 push (16:05 IST) and Claude's GitHub check (16:07 IST, PASS), in section 2.
2. **D-004's storage package** carries the two D-003 records of section 2, builds on `c33466b`, and starts at DR-070.
3. **ChatGPT (new window):** prepare D-004's Stage 0 Rama answer sheet and Claude prompt. Starting points:
   - agenda line 295;
   - D-003's D-004 boundary row and manual sections 3 and 6 (MC-02, MC-06, MC-20 to MC-25, MC-37);
   - bookmark BK-67;
   - the D-001 and D-002 manuals on GitHub for the earlier boundary rows.
