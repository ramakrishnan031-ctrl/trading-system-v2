# C-008 - PROPOSED CHANGES FOR REVISION 3 (FOR REVIEW; NOT APPLIED)

Status: proposal of 24-Oct-2026 (IST); not applied. C-008 revision 2 stays as delivered (C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV2_24-OCT-2026.md, 648 lines, SHA-256 820B36B9EBE2ABA16976A3A026D9C4B208FE4F0227D23B96A6848E5FC3AA108E), and so does its inventory. Nothing is frozen, committed, pushed or published.  
Source: C-008_CHAPTER_02_03_CROSS_CHECK_24-OCT-2026.md (the cross-check; rows CH-01 to CH-61), prepared under ChatGPT's C-008_DECISION_AND_NEXT_STEPS_24-OCT-2026.txt, whose instruction 8 asks for OQ-03 to be updated only in a proposed revision for review.  
Use: after ChatGPT's review of these proposals and Rama's decisions (section 1), Claude builds revision 3 from them and fills the bracketed placeholders; nothing is built before. Line numbers below are those of revision 2. Each "Current" text occurs exactly once in revision 2 (checked when this file was written).

## 1. Decisions the proposals wait for

| ID | Decision | Options | Proposed |
|---|---|---|---|
| D-1 | C1 (CH-04): chapter 02's "reverse order" teardown against W-08 and W-09 | (a) record in section 14.7 and keep C-008's statements; (b) as (a), and pass the item to the 18-Sep manual's owner; (d) other (the cross-check, section 4) | (a) |
| D-2 | C2 (CH-11): "every decision is delegated to injected objects" against OC-02, ST-21 and ST-34 | as D-1 | (a) |
| D-3 | C3 (CH-26): "REJECTED_QUEUE_FULL" against the bare `REJECTED` (section 5.1, RQ-06) | as D-1, and (c) a bounded read of the testing snapshot's branch, which widens C-008's sources | (a) |
| D-4 | Where the 61-row table lives after the freeze | (i) a second companion file, frozen with C-008 under revision 3's date; (ii) copied into section 14.7 | (i): it keeps section 14.7 short and the table's method note beside the table |
| D-5 | P-06: may Claude re-view the saved output of C-008's read 042 (signals/signal_processor.py, lines 1690 to 1704 at the pin), an existing logged read, to settle SS-03 before revision 3? | yes; no (SS-03 unchanged, CH-34 stays UNVERIFIED) | yes |
| D-6 | P-07: point U-01 to chapter 02's statement about `is_tracking` | adopt (U-01 stays open); do not adopt (the statement stays in the cross-check only) | adopt, as a pointer |
| D-7 | The fetch from the records repository at Rama's request (00:51 IST), after the decision note's instruction 1 said not to access it | ChatGPT to confirm it acceptable for the record | - |

## 2. The changes

### P-01 Header, status (line 3)

Current:

```text
Status: DRAFT, revision 2 of 24-Oct-2026 (Stage 1 deliverable, revised after ChatGPT's review of revision 1; not frozen, not committed, not pushed).
```

Proposed:

```text
Status: DRAFT, revision 3 of [date] (Stage 1 deliverable, revised after ChatGPT's review of revision 2; not frozen, not committed, not pushed).
```

Current:

```text
Nothing in this draft takes effect until ChatGPT's review of revision 2 and Rama's approval under W-4 (DR-017).
```

Proposed:

```text
Revision 3 applies ChatGPT's C-008_CHATGPT_REVIEW_REV2_24-OCT-2026.txt (received from Rama at 00:47 IST on 24-Oct-2026; SHA-256 8E11787DE255982E9436324075D5BB1E18D6FB78BE5782ADBC3786BFF1FA71CA; "REVIEW HOLD - NOT APPROVED FOR FREEZE") and its C-008_DECISION_AND_NEXT_STEPS_24-OCT-2026.txt (00:50 IST; SHA-256 B1A93E13222BD41D18CAB4EB8EE66B628658654C9327B73E3144695733AEE69B): the statement-level reconciliation against chapters 02 and 03 of the 18-Sep manual (section 14.7), with Rama's decisions on its three conflicts ([date]). Nothing in this draft takes effect until ChatGPT's review of revision 3 and Rama's approval under W-4 (DR-017).
```

### P-02 Header, companion files (line 6)

Current:

```text
Companion file: C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV2_24-OCT-2026.txt (the inventory: safety declaration, read log, searches, claim index). Revisions 0 and 1 and their companions are kept unchanged (sections 19.1 and 19.3).
```

Proposed (with D-4 (i)):

```text
Companion files: C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV3_[date].txt (the inventory: safety declaration, read log, searches, claim index) and C-008_CHAPTER_02_03_CROSS_CHECK_[date].md (the statement-level table of section 14.7). Revisions 0 to 2 and their companions are kept unchanged (sections 19.1, 19.3 and 19.4).
```

### P-03 Section 1 (lines 12 and 32)

Current (line 12):

```text
with the Stage 0 count-only searches Q01 to Q35.
```

Proposed:

```text
with the Stage 0 count-only searches Q01 to Q35. Section 14.7 also rests on 18 logged reads of two documents, chapters 02 and 03 of the 18-Sep manual (section 1.2); they are not legacy source and are not among the 125.
```

Current (line 32):

```text
the two excluded July records (R38) and the host records (R37) were not opened;
```

Proposed:

```text
the two excluded July records (R38) and the host records (R37) were not opened; the records repository (trading-system-records) was opened for the two chapter files of section 14.7 only (section 1.2);
```

### P-04 W-08, the assertion (line 100; from CH-07)

Current:

```text
an assertion checks that the reconcile ran (`main.py:3968-3971`); then the processor starts
```

Proposed:

```text
an assertion checks that the reconcile ran (`main.py:3968-3971`), a Python `assert` statement, for which Python generates no code when optimisation is requested, for example with the -O option (FACT (EXTERNAL): the Python 3 Language Reference, section 7.3, "The assert statement"; how the production service started Python was not read, [U-06], and the 18-Sep manual's chapter 02 leaves the same point open for its tree, section 14.7); then the processor starts
```

### P-05 Section 1.1, the Lines column (after line 59; from CH-02)

Insert after the table's last row (`v3_chain/pb01_entry.py`), with a blank line before it:

```text
Lines: the gated reader's count, the number of newline characters in the file plus one (the reader, mask_read2.py, lines 73 and 79). For a file that ends with a newline it is one more than the physical line count, the last numbered line being empty; whether each pinned file ends with a newline was not checked. Line citations are not affected. Found in revision 3, against the 18-Sep manual's physical count for signals/entry_throttle.py (section 14.7).
```

### P-06 SS-03, the paths that share the in-flight counter (line 272; from CH-34; only with D-5 "yes")

Check first: in the saved output of read 042, whether `admit_prepared`'s `finally` (about `signals/signal_processor.py:1696-1704`) decrements `_in_flight_count`.

Current:

```text
(b) the processor's `_in_flight_count`, one counter for all workers and the three paths, incremented before the lock and decremented in `finally` (`signals/signal_processor.py:253-257, 1248-1258, 1302-1305, 2133-2136, 2357-2364, 2469-2472, 2630-2636`)
```

Proposed if the check confirms it:

```text
(b) the processor's `_in_flight_count`, one counter for all workers and for E1 to E3 and, through `_admit_and_place` ([ST-33]), E4, incremented before the lock and decremented in `finally` (`signals/signal_processor.py:253-257, 1248-1258, 1302-1305, [E4's lines], 2133-2136, 2357-2364, 2469-2472, 2630-2636`)
```

If the check does not confirm it, SS-03 is unchanged and CH-34 is recorded in section 14.7 as resolved, with the lines that show it.

### P-07 U-01 (line 529; from CH-49; only with D-6 "adopt")

Current:

```text
- **[U-01]** What `ShadowTracker.is_tracking` returns while the tracker is disabled (orders/shadow_tracker.py is outside the inventory and was not read).
```

Proposed:

```text
- **[U-01]** What `ShadowTracker.is_tracking` returns while the tracker is disabled (orders/shadow_tracker.py is outside the inventory and was not read). The 18-Sep manual's chapter 02, about another tree (the testing worktree e7bf477), says it "returns False when disabled" (its line 1983; section 14.7); that is not evidence for the pin, and U-01 stays open.
```

### P-08 Section 1.2, a bullet on the chapters (after line 70)

Insert as a new bullet after the LEGACY_SYSTEM_MANUAL_INDEX.md bullet:

```text
- **Chapters 02 and 03 of the 18-Sep manual** (revision 3): ChatGPT's review of revision 2 (received at 00:47 IST on 24-Oct-2026; the header) required a statement-level reconciliation against them before the freeze, said Claude was not to fetch them from the records repository "under the existing C-008 source boundary", and asked Rama for the files "(or explicitly revise the source authorisation)"; its decision note (00:50 IST; the header) set the method. At 00:51 IST Rama asked Claude to look for the chapters in the GitHub repositories and download them if found. Claude cloned the public records repository, trading-system-records, blob-less, without checkout, at depth 1 (commit ae7335cbc8ad3762a39a9ddde15eb5103e2d6b29, 18-Sep-2026 01:09 IST; no path on the protected-path list among its 1,364 tree entries) and fetched two blobs only, the chapters; Rama then uploaded the same two files from ChatGPT's project, byte-identical (SHA-256 in section 14.7). No other file of the records repository was read; LEGACY_SYSTEM_MANUAL_INDEX.md has the blob ID of that commit's copy. The chapters were read as documents, not as legacy source, only through a logged and masked reader (18 reads, X001 to X018; the inventory, section [N]). No legacy source file was read for revision 3. [D-7: ChatGPT's confirmation of the fetch.]
```

### P-09 Section 14.3 (lines 432 and 441)

Current (line 432, IX-04's C-008 cell):

```text
| W-08 and W-09 (the start and stop order around the processor) | Not reconciled:
```

Proposed:

```text
| W-08 and W-09 (the start and stop order around the processor); for chapter 02, the processor's construction, wiring, entry points and role (section 14.7) | Reconciled at statement level in revision 3, as ChatGPT's review of revision 2 required: section 14.7. Rama supplied chapters 02 and 03 on 24-Oct-2026, byte-identical to the records repository's copies (section 1.2); within the bounded overlap, three conflicts (CX-01 to CX-03), recorded with Rama's decisions. Chapter 01 was not required and was not compared. [OQ-03] is answered. Before revision 3: not reconciled:
```

(The rest of the cell, from "the chapter texts were not supplied" to "[OQ-03] |", is kept as the record of revision 2.)

Current (line 441):

```text
[OQ-02] is answered at the level of the index; what remains is [OQ-03].
```

Proposed:

```text
[OQ-02] is answered at the level of the index; [OQ-03], which remained, is answered in section 14.7.
```

### P-10 New section 14.7 (after line 522, before section 15)

```text
### 14.7 The 18-Sep manual, chapters 02 and 03 (statement level; revision 3)

ChatGPT's review of revision 2 required this reconciliation before the freeze, chapter 03 covering the start, the run and the stop and chapter 02 the wiring ([OQ-03]). The chapters are 02_REPOSITORY_AND_FILE_MAP.md (3,557 lines; SHA-256 8B5AEC74544C7A613CED958F90CA9DEEDB524DB321AE7101B9A577015CBF6D49) and 03_STARTUP_RUNTIME_AND_SCHEDULER.md (321 lines; SHA-256 F7D38F2D0F7EA2D61783970B1E8A81FC06D1676AB613EE4DB18178EE5CBC3087), obtained as section 1.2 describes. They are read as documents, not as legacy source, and are not modified; "02:N" is line N of chapter 02. Both describe another tree, the testing worktree at e7bf477 with an uncommitted change set ("PRODUCTION WAS NEVER READ"), so their line numbers are not comparable with C-008's citations (IX-02). The comparison is bounded to what they say about the processor: in chapter 02, the entries for main.py and the processor and the other entries that name the processor; all of chapter 03. The statement-level table is the companion file C-008_CHAPTER_02_03_CROSS_CHECK_[date].md (CH-01 to CH-61) [D-4].

Result: 61 statements compared; 41 CONSISTENT, 8 PARTIAL, 3 CONFLICT, 9 NOT COMPARABLE.

| ID | The chapters | C-008 | Result |
|---|---|---|---|
| CX-01 (CH-04) | "tears everything down in reverse order" (02:103); "Reverse-order teardown" (02:108) | the processor starts before the entry gate ([W-08]) and is also stopped before it ([W-09]); [W-10] rests on that order | CONFLICT; [Rama's decision of [date]] |
| CX-02 (CH-11) | "every decision is delegated to injected objects" (02:952) | the processor's own checks and rejects ([OC-02], [ST-21], [ST-34]) | CONFLICT; [Rama's decision of [date]] |
| CX-03 (CH-26) | "REJECTED_QUEUE_FULL (437-439)" among the statuses written (02:959) | the bare `REJECTED` with reason `QUEUE_FULL` (section 5.1; [RQ-06]), in a branch not wired at the pin ([ST-01]); a tree difference is not excluded | CONFLICT; [Rama's decision of [date]] |

The PARTIAL rows qualify a chapter statement without contradicting C-008: the age figure, "60s" against 600 s at the pin ([CF-01], [D-04]); the boot-order assertion under optimisation ([W-08]); the drain at stop ([SD-03]); the target's ATR branch ([K-01]); "release on every abort path" ([K-05], [EX-05], [EX-09]); the kill switch's "mutation" ([EX-02], [SD-02]; erratum B-009 E-3); the in-flight counter's sites ([SS-03]; [P-06's outcome]); and chapter 03's teardown list, which leaves out the processor's stop ([W-09]). The NOT COMPARABLE rows are tree differences (`sr_shadow`; the line counts, section 1.1), steps C-008 did not read (the price helper's bounds step; the metrics methods, section 16) and points C-008 does not address (dynamic imports, the stop trigger, main's thread, the scheduler, the callers of the S&R facade, and the shadow tracker's answer when disabled, [U-01]). Nothing in the 18-Sep manual is changed: it is not a manual of this series.

Result: three conflicts, recorded with Rama's decisions; [if D-1 to D-3 are (a):] no erratum to C-008 and none made by C-008 to the 18-Sep manual. [OQ-03] is answered.
```

### P-11 OQ-03 (line 528)

Current:

```text
- **[OQ-03] OPEN QUESTION (for ChatGPT's review and Rama):** whether the freeze needs a statement-level reconciliation against the 18-Sep manual's written chapters 02 and 03 (IX-04). Their texts are not supplied, and their home, the records repository, is outside C-008's sources; the chapters that would cover C-008's subject are not started (IX-03).
```

Proposed:

```text
- **[OQ-03] ANSWERED in revision 3:** ChatGPT's review of revision 2 required a statement-level reconciliation against the 18-Sep manual's chapters 02 and 03 before the freeze. It is section 14.7: within the bounded overlap, three conflicts (CX-01 to CX-03), recorded with Rama's decisions of [date]; the other statements compared are consistent, partial or not comparable, and none changes a C-008 finding. The chapters that would cover C-008's subject are not started (IX-03).
```

### P-12 Section 16 (after line 545)

Insert as a new last bullet:

```text
- The reconciliation with the 18-Sep chapters (section 14.7) compares two documents about another tree with this draft. It covers what they say about the processor (chapter 02 in part, by the ranges and searches its companion file lists; chapter 03 in full) and claims no agreement beyond its rows.
```

### P-13 Section 19.5 (after line 648)

```text
### 19.5 Revision 3 ([date])

Revision 3 applies ChatGPT's review of revision 2 and its decision note (the header) and Rama's decisions of [date]. No legacy source file was read (125 reads and 18 searches, as in revision 2); 18 logged reads of the two 18-Sep chapters were added (documents; section 1.2). Each item:

| Item | What revision 3 does |
|---|---|
| Review of revision 2, item 1 (a statement-level reconciliation against chapters 02 and 03 is required before the freeze) | Section 14.7 and the companion cross-check file; [OQ-03] answered |
| Review of revision 2, item 2 (what checks out) | Kept; nothing changed |
| Review of revision 2, item 3 (OQ-01 open; erratum B-009 E-3 only at the freeze; Rama's approval under W-4) | Unchanged |
| Decision note, instruction 1 (the sources) | The two chapter files, revision 2, its inventory and the index; the fetch Rama asked for at 00:51 IST is recorded in section 1.2 [D-7] |
| Decision note, instruction 2 (identify the chapters; the copies' identity) | Section 14.7 and the companion file: titles, numbering, evidence dates and the index's descriptions match; identity with the records repository's copies established by SHA-256 |
| Decision note, instructions 3 to 5 (a bounded, statement-level check; no re-audit and no source read; the table) | The companion file, CH-01 to CH-61; section 14.7 |
| Decision note, instruction 6 (a contradiction: quote, cite, stop for a decision) | CX-01 to CX-03, decided by Rama on [date] |
| Decision note, instruction 7 (the wording when no conflict is found) | Not applicable: three conflicts were found |
| Decision note, instruction 8 (OQ-03 only in a proposed revision for review) | Proposed in C-008_REV3_PROPOSED_CHANGES_24-OCT-2026.md, applied here after that review |

Changes for re-review: the header; section 1 (the document reads; the records repository); section 1.1 (the Lines note); section 1.2 (the chapters); [W-08] (the assertion); [SS-03] [if changed]; [U-01] [if changed]; section 14.3 (IX-04 and its closing line); section 14.7; [OQ-03]; section 16; section 19.5. Nothing else changed.
```

### P-14 The companion inventory (revision 3)

- A new section, "Document reads for section 14.7": X001 to X018 from the chapter read log (time, mode, range or pattern, file, blob, lines printed, masks), the records-repository clone and its two blob fetches, the identity checks (SHA-256 of the uploaded and the fetched copies; the index's blob ID) and the state of both clones at the build.
- The Lines note of P-05 where the read log gives file lengths.
- The claim index regenerated (new or changed claims: W-08, IX-04, CX-01 to CX-03, OQ-03, and SS-03 and U-01 if changed) and the citation check re-run; the only new legacy citation expected is E4's line in SS-03 (P-06), printed by read 042.
- No change to the 125 reads or the 18 searches.

### P-15 Claude's build tools (not legacy files)

- build_manual_c008.py and build_inventory_c008.py: revision 3's date and tag; the SHA-256 of the review of revision 2 and of the decision note; CX- added to the claim-ID pattern; the cross-check file named as a second companion (D-4 (i)); the chapter read log read for P-14.
- The cross-check file re-issued under revision 3's date if D-4 is (i), with the decisions recorded in its section 4 and its status changed from review deliverable to companion.

## 3. Not proposed

- No change to sections 2 to 13, 17 or 18 other than P-04, P-06 and P-07: the cross-check found no C-008 finding to correct.
- No change to any frozen manual, to LEGACY_SYSTEM_MANUAL_INDEX.md or to the 18-Sep manual. Erratum B-009 E-3 stays a freeze-time step (section 14.6).
- No V2 decision.
