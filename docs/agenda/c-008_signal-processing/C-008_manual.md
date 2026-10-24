# C-008 - SIGNAL PROCESSING: LEGACY MANUAL

Status: FROZEN 2026-10-24 (revision 3, DR-061). Revision 0 (23-Oct-2026) was written by Claude under ChatGPT's C-008_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_FOR_CLAUDE_23-OCT-2026.txt (received from Rama at 21:44 IST; SHA-256 FD3E3BBB59DDEB0CBD0C091AE647B4F497ABFBD35BE44182A1AFBE4C3B7B8026). Revision 1 (24-Oct-2026) applied ChatGPT's C-008_CHATGPT_STAGE1_REVIEW_AND_OPEN_DECISIONS_23-OCT-2026.txt (received from Rama at 23:53 IST on 23-Oct-2026; SHA-256 2C72BEF8AF11E48D63258344B94B23B2B72C29F9802E48BA5B098FFE15D8BCF0) and Rama's two decisions of 23-Oct-2026 (section 19.3). Revision 2 (24-Oct-2026) applied ChatGPT's C-008_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (received from Rama at 00:29 IST on 24-Oct-2026; SHA-256 ECFD9E19E45C6E0D75C980D14C33307E73F902BEFABF882C8BCE6B4D4F8EBEB5): the reconciliation against LEGACY_SYSTEM_MANUAL_INDEX.md, supplied with it (SHA-256 DFE1AC19890DF8FFFC450ADB627B49FCEBE73D8545C91AC5FF83448DE46C3287; section 14.3). ChatGPT's review of revision 1: "ACCEPTABLE AS A GOVERNED REVISION; NOT APPROVED FOR FREEZE YET". Revision 3 applies ChatGPT's C-008_CHATGPT_REVIEW_REV2_24-OCT-2026.txt (received from Rama at 00:47 IST on 24-Oct-2026; SHA-256 8E11787DE255982E9436324075D5BB1E18D6FB78BE5782ADBC3786BFF1FA71CA; "REVIEW HOLD - NOT APPROVED FOR FREEZE"), its C-008_DECISION_AND_NEXT_STEPS_24-OCT-2026.txt (00:50 IST; SHA-256 B1A93E13222BD41D18CAB4EB8EE66B628658654C9327B73E3144695733AEE69B) and its C-008_CHATGPT_VALUE_ADDED_REVIEW_24-OCT-2026.txt (08:05 IST; SHA-256 0D25ADE6F4FA3434B5975E654BD93C1FA37D017CCFC57228BFDEC38ACE95D1F8; "REVIEW ONLY - NOT APPROVED FOR FREEZE"), with Rama's decisions of 24-Oct-2026 (about 08:08 IST; section 19.5): the statement-level reconciliation against chapters 02 and 03 of the 18-Sep manual (section 14.7). ChatGPT's review of revision 3, C-008_CHATGPT_VALUE_ADDED_REVIEW_REV3_24-OCT-2026.txt (received from Rama at 09:13 IST on 24-Oct-2026; SHA-256 5E5A9B0088159139DE529E0356C0ABEC120B6648A0F0CD86DC04454BF882ADDA; "SUBSTANTIVE REVISION-3 REVIEW: PASS, WITH THE ABOVE GOVERNANCE CONDITIONS") passed it with governance conditions, and Rama approved revision 3, exactly this manual and its two companion files as delivered, as the release candidate on 24-Oct-2026 (09:15 IST; section 1.2). Frozen under W-4 (DR-017): the freeze takes effect when Rama declares the package final and runs PART 2 of its push commands. The freeze edits are listed in section 19.6.  
Topic: C-008 Signal processing (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.  
Evidence baseline: trading-system-prod-snapshot commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), read on 23-Oct-2026 through the gated masking filter.  
Companion files, frozen with this manual and stored unchanged beside it in docs/agenda/c-008_signal-processing/: C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV3_24-OCT-2026.txt (the inventory: safety declaration, read logs, searches, claim index) and C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md (the statement-level table of section 14.7; Rama's decision D-4); their SHA-256 are in section 19.6. Revisions 0 to 2 and their companions are kept unchanged (sections 19.1, 19.3 and 19.4), and so are the cross-check as first delivered and the proposals it led to (section 19.5). Earlier inputs: C-008_CLAUDE_STAGE0_ANSWERS_23-OCT-2026.txt (SHA-256 2C31E671ED73DC455991D87A4B2D19450EB484837D37DB8D1245FFF9D77E6048) and C-008_CLAUDE_STAGE0_SOURCE_INVENTORY_AND_METHOD_23-OCT-2026.txt (SHA-256 0962A40598DF01ABC46D9B8B8D6D2C2A93203C1D8C89C34B4800DAA2FE908A93). Where they are stored (W-3), all unchanged: revisions 0 to 3 of this manual in docs/inputs/manuals/; the inventories of revisions 0 to 2, the cross-check as first delivered, the proposals, Claude's Stage 0 files and ChatGPT's Stage 0 research prompt and answer sheet in docs/inputs/chatgpt_originals_and_replies/; ChatGPT's reviews and its decision note in docs/inputs/reconciliation/; its Stage 1 authorisation in docs/agenda/c-008_signal-processing/. Not stored: LEGACY_SYSTEM_MANUAL_INDEX.md and chapters 02 and 03 of the 18-Sep manual, read as documents, which hold host and path values this project does not reproduce; they are identified by SHA-256 (this header and section 14.7) and by blob ID in the records repository (section 1.2).  
Purpose: factual record of how the legacy signal processor orchestrated a dequeued signal: its construction and entry points, the order of its stages on each path, each processor-level check and what the processor did with its result, the statuses and reasons it wrote, reservation and claim release, re-queue, exceptions, in-flight bookkeeping and its stop and kill checks. It is not a V2 design or a recommendation to revive the legacy system. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1).  
Security: no credential, token, account or client identifier, host or address, deployed path, mailbox, SSH detail, or person name other than Rama's is reproduced.

## 1. Evidence rules and boundaries

This draft rests on 125 gated reads of 22 files (91 range reads and 34 grep reads) and 18 count-only Stage 1 searches (Q36 to Q53; Q50 to Q53, added in revision 2, are positive controls, section 14.3) against the pinned commit, with the Stage 0 count-only searches Q01 to Q35. Every read matched its pinned blob and ran after the gate's self-test, and the protected-path check was zero before every read and at delivery (the inventory, sections 2, 3 and 6). Section 14.7 also rests on 18 logged reads of two documents, chapters 02 and 03 of the 18-Sep manual (section 1.2): document reads, each checked against the recorded SHA-256 of its file, and separate from the 125 legacy reads and the 18 searches. Bracketed IDs such as [ST-06] or [K-04] are this draft's claim IDs; the inventory's claim index (section 8) maps each one to its citations and to the logged reads that printed the cited lines. Every `path:lines` citation below names lines printed by a logged read of that file at its pinned blob (619 citations checked, 0 uncovered; the inventory, section 9). The check shows that the cited lines were read, not that each statement about them is right.

Evidence labels used here:

- **FACT (SOURCE-VERIFIED):** supported by source lines read at the pinned commit.
- **FACT (CONFIG):** a value in the repository's configuration files, not evidence of a deployed host's effective configuration.
- **FACT (TEST TEXT):** what test source states; tests were never run; not runtime evidence.
- **FACT (TEXT DRIFT):** a comment or docstring that differs from executable code, wiring or configuration.
- **FACT (EXTERNAL):** language or library behaviour, with the version used.
- **INTERPRETATION:** a possible consequence of a source path ("the inspected code path would ..."), not proof that it occurred.
- **HISTORICAL:** a statement in a previously frozen manual; **HISTORICAL / RECORD-DERIVED:** a figure or event stated in a code comment or in a frozen manual's record section, not verified here.
- **KNOWN DEFECT:** a defect previously recorded in a frozen manual and re-verified at the pin, to the extent stated.
- **UNKNOWN / NOT ESTABLISHED** and **OPEN QUESTION:** the evidence did not establish the answer.

A code path is not proof that a signal, order or trade took that path. Repository configuration is not proof of deployed configuration. Quotations are verbatim except that comment markers, Markdown emphasis and code marks, line breaks and runs of spaces are dropped, non-ASCII dashes, arrows and symbols are shown as ASCII ("-", "->") or left out, and "..." marks words left out or a value filled in at run time.

**Scope (authorisation, section 2 A and C):** the processor's construction, wiring and entry points; the stage order on each supported path; each processor-level check as implemented, the branch it produces and the next consumer; statuses and reasons written, reservation and claim release, re-queue, exceptions, in-flight and same-symbol bookkeeping, processor-level stop and kill checks; the configuration the processor reads. The pre-checks (kill switch, entry windows, the age check, strategy lookup and control, the strategy circuit breaker, the entry throttle and the same-symbol checks) appear by their place in the pipeline and their outcome; their rules stay with their owners.

**Owners elsewhere (authorisation, section 2 B):** C-006 (the queue, the dispatcher, the pool, stop mechanics); C-007 (the status vocabulary, the writers' storage, retention); C-009 (expiry policy); C-010 (scanner-to-strategy mapping); C-011 (the EOD route; no direct processor hand-off was found, section 9.4); D topics (screening and the gate and retest rules); E topics (entry, stop and target prices); F topics (sizing, risk, capital, allocation); G to J topics (orders, broker, positions, kill-switch ownership, S&R, V3, allocator internals); K and L topics (database and state design, alerts, reports, evidence). The allocator and the V3 chain are described by their processor-side call and outcome only (authorisation D).

Not done: the dated records R34 to R36 were not read; the two excluded July records (R38) and the host records (R37) were not opened; the records repository (trading-system-records) was opened, at Rama's request, for the two chapter files of section 14.7 only (section 1.2); no legacy code, test or script was run; no live service, VM, database, broker, webhook or Chartink was contacted; no legacy file was changed; nothing was committed or pushed.

### 1.1 Sources at the pin

| File | Pinned blob | Lines | Reads | Role in this chapter |
|---|---|---|---|---|
| `main.py` | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | 4,294 | 22 | the processor's construction, the components passed and late-bound, the gate-release callback, start and stop order, the reconciler and risk-engine constructions (located lines only) |
| `signals/signal_processor.py` | `7fab0a02a302770ac72bbbd662549d5c091086c1` | 2,704 | 34 | the pipeline: entry points, stage order, checks, outcomes, handlers, re-queue, release |
| `config/system_config.yaml` | `bf742ae5c81c60614370361998e6a102a1064ff7` | 754 | 6 | the processor, queue-expiry, risk, allocator, V3, S&R, retest, breaker and shadow-tracker keys (repository values) |
| `core/config_loader.py` | `8635bca77531515561ee48cf92677883a5e30782` | 2,517 | 3 | the models' defaults for those keys |
| `screening/entry_gate.py` | `46b173171bdfeba48af641b67a361618e5acba58` | 538 | 3 | the gate's start reload, release protocol and docstring |
| `strategies/control.py` | `b6eecb480b83471149fb9362bafc980c967ff6c9` | 118 | 2 | the strategy-control verdict and its causes |
| `core/market_windows.py` | `25625e8cd109c935d014ad357cd6726c75737157` | 334 | 2 | the two entry-window checks |
| `capital/kill_switch.py` | `3eb2ece050c3eafbcf2e90ec01c824b4dff27950` | 2,152 | 4 | the entry check and the API-failure recorder |
| `capital/strategy_governor.py` | `d17193c569c1d2e2890d2047b03cf9d0ae825e8d` | 197 | 2 | the circuit breaker's check |
| `screening/secondary_screener.py` | `b6f8ff7223ad803869e976f92d794dfc7c811b59` | 706 | 3 | the screen call's interface, result type and quote handling |
| `capital/position_sizer.py` | `f92eace8d6e30eae2f2f57a0e244fb6873e538f3` | 823 | 6 | the sizer's interface, input checks and failure constraints |
| `capital/risk_engine.py` | `27af699b591310a15e206f7bed0a8f82c278e196` | 840 | 8 | the approval result, its ten check names, the in-flight count's use and the same-symbol checks |
| `capital/fund_manager.py` | `6f61618a5b0e8a9d67d90c7d2ee7ba3a79ad278c` | 2,584 | 6 | the portfolio lock, reserve, release and the per-strategy reservation count |
| `signals/entry_throttle.py` | `b887a4be78a4dcfe3c2541d7528bd3b96ec351ea` | 126 | 2 | the throttle (read in full to line 116 of 126) |
| `orders/order_placer.py` | `e3a602909befa3fd3ba4b27caeed108724c07947` | 4,707 | 4 | place()'s interface and contract, its R:R gate, its 429 loop and its timeout branch |
| `screening/retest_monitor.py` | `26639d6ed5dddb1140f0868bb9f937cad5379f82` | 442 | 4 | the divert decision and the confirm callback |
| `allocation/portfolio_allocator.py` | `cb27a82e757f2d8aba4400ec0949611b0c340e2d` | 337 | 2 | the allocator's processor-facing methods and lifecycle |
| `v3_chain/runner.py` | `a782f91f2f4f1afe12c75072708e2a26316eed3f` | 426 | 2 | the chain's observe hook |
| `config/strategies/gap_go_long.yaml` | `6e6656fbc6ed80f9dd749bb4837562a3e0dfdb65` | 57 | 1 | key names of a strategy file (one grep); per-file counts over all 16 files by Q43 to Q47 |
| `tests/unit/test_signal_processor.py` | `3ec0ea84392a193d18b992ca88df0da5e6235f67` | 2,501 | 7 | test source (documentary; never run) |
| `v3_chain/pb01_runner.py` | `e87a5aa964643f1eae5fad90eeb1e0993b5829e0` | 297 | 1 | one grep for the processor's entry-point names (docstring lines only; section 2.2) |
| `v3_chain/pb01_entry.py` | `cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8` | 372 | 1 | one grep for the processor's entry-point names (docstring lines only; section 2.2) |

Lines: the gated reader's count, which is the number of newline characters in the file plus one (the reader, mask_read2.py, lines 73 and 79). For a file that ends with a newline this count is one more than the physical line count. Whether each pinned file ends with a newline was not checked, so physical line counts are not stated here. Line citations are not affected. Noted in revision 3, where the 18-Sep manual's chapter 02 gives a count one lower for signals/entry_throttle.py in its own tree (section 14.7); the cause of that difference is not established.

### 1.2 Process notes, deviations and inputs not available

- **Authorisation step:** the Stage 0 lock of the read tool was lifted as a step of its own at 21:45 IST, after the authorisation arrived (tool SHA-256 before e32c38fa4d1a7030, after 7341d6edc10b76c5); the gate test was re-run and passed at 21:46 IST before the first read (the inventory, section 2).
- **Count-only searches at Stage 1 (Q36 to Q53):** they inspected source-file bytes through the masking and counting tools and printed per-file counts only, no source line (the wording ChatGPT's review asked for). Their purpose: callers of the gate's add() and of pipeline_timeout_sec, where two exception classes are raised, the class caught by the placer's 429 loop, strategy settings, and doubled reject names (section 3; the inventory, section 7); and, added in revision 2, a positive control for each zero result (Q50 to Q53, section 14.3, IX-08).
- **Files:** every file read is a register file (R01 to R33). No file outside the Stage 0 inventory was read. Three files outside it were found relevant and left unread, each recorded as UNKNOWN where it matters: orders/shadow_tracker.py ([U-01]), broker/rate_limiter.py ([U-03]) and orders/order_reconciler.py (cited from B-005, [EX-08]). CONDITIONAL rows read because the processor calls them: R15 (capital/strategy_governor.py only) and R22 (core/market_windows.py only); R19 in part: v3_chain/runner.py, which the processor calls, and one grep each of v3_chain/pb01_runner.py and v3_chain/pb01_entry.py for the processor's entry-point names (section 2.2). Rows read in part: R10 (strategies/control.py only), R16 (allocation/portfolio_allocator.py only) and R26 (one grep of one file, and count-only searches over all 16). Not read: R05, R08, R18, R20, R21, R23 (no processor call needing them was found, or the processor's own guard establishes the outcome), R24 and R25 (cross-references, frozen in C-007 and C-002 to C-007), R27, and the conditional tests R29 to R33 (R28 sufficed). The updated inventory is in the companion file, section 5.
- **The filter's masks** replaced tokens in the printed output with placeholders: `<ALNUM>` for long class names (in main.py, the processor, the configuration file, the order placer and the V3 chain's runner), `<CLIENTID>` for the codes in `noqa:` comments (main.py and the processor), `<MASKED>` for one keyword value in main.py, `<HOSTTOK>` for a short token in some lines of core/config_loader.py (a type name, and the same letters at the start of a word), and `<LCID>` for two lower-case identifiers (one in capital/kill_switch.py, one in the test file). No statement below depends on a masked token except the class caught by the order placer's retry loop, which was identified by a count-only search, not by printing it (Q42; [EX-06]).
- **Independent check before the delivery of revision 0:** a separate agent checked a snapshot of this draft (SHA-256 beginning b2d6e7518736a4ce) against the saved evidence of reads 001 to 123, the read log, the searches Q01 to Q49, the frozen manuals cited, V2_DESIGN_LOG and the authorisation, without the drafting context; it opened no legacy file and ran nothing. It reported 22 findings (2 MUST-FIX, 14 SHOULD-FIX, 6 NOTE) and found no V2 decision made or implied. Claude re-checked each finding against the evidence before applying it; all 22 are applied, and one (IC-21) needed two further gated reads, 124 and 125 (section 19.2). The applied fixes were not checked again by a separate agent: this draft has had one independent check, not two.
- **ChatGPT's Stage 1 review (23-Oct-2026):** ChatGPT reviewed the submitted documents and claims; it did not reproduce the local gate or repository checks, which are Claude's recorded checks (the inventory). It asked for no factual or editorial change, kept three items open (RC-15 for Rama's decision; the entry throttle's owner, [OQ-01]; the reconciliation against LEGACY_SYSTEM_MANUAL_INDEX.md, [OQ-02]) and listed points to preserve. **Rama's decisions (23-Oct-2026, about 23:58 IST):** RC-15 approved as erratum B-009 E-3, to be applied at the C-008 freeze (section 14.6); revision 1 to be returned now with the index reconciliation still pending. Section 19.3 lists each review item and what revision 1 does with it.
- **ChatGPT's review of revision 1 (24-Oct-2026):** it checked that the uploaded files were revision 1 (the manual's 612 lines and SHA-256 matching the inventory's), the evidence boundaries, RC-15's record and OQ-01, and named one freeze blocker: the reconciliation against LEGACY_SYSTEM_MANUAL_INDEX.md, to be recorded and re-reviewed; and the freeze-time steps for erratum B-009 E-3 (section 14.6). It reviewed the documents and their internal consistency; it did not reproduce Claude's local gate, inspect the scratch evidence or re-read the 125 source outputs, so the inventory's checks remain Claude-recorded evidence. Section 19.4 lists each item and what revision 2 does with it.
- **LEGACY_SYSTEM_MANUAL_INDEX.md** (authorisation G): not available to Claude until revision 2. It was not among the Claude Project's documents or uploads, the session's attachments, V2 main dce3164 or the legacy listing (rechecked after ChatGPT's Stage 1 review, about 23:55 IST on 23-Oct-2026), and revision 1 was returned with the reconciliation pending, by Rama's choice. Rama supplied the file at 00:29 IST on 24-Oct-2026, with ChatGPT's review of revision 1; ChatGPT identified it as the existing file in its project. 124 lines; SHA-256 in the header. It is read as a document, not as legacy source, and not modified (authorisation G); no replacement index is made. The reconciliation is section 14.3 (IX-01 to IX-11).
- **ChatGPT's review of revision 2 (24-Oct-2026, received at 00:47 IST):** "REVIEW HOLD - NOT APPROVED FOR FREEZE". It decided [OQ-03]: a statement-level reconciliation against the 18-Sep manual's chapters 02 and 03 is required before the freeze; it kept [OQ-01] open and erratum B-009 E-3 a freeze-time step, and listed what checks out in revision 2. Its decision note (00:50 IST) set the method: identify the chapters, compare only where they overlap C-008, quote and cite any contradiction and stop for a decision, and update [OQ-03] only in a proposed revision for review. Both reviewed documents; neither reproduced Claude's local checks.
- **Chapters 02 and 03 of the 18-Sep manual** (revision 3): ChatGPT's review of revision 2 said Claude was not to fetch them from the records repository "under the existing C-008 source boundary" and asked Rama to supply them "(or explicitly revise the source authorisation)"; the decision note's instruction 1 also kept the records repository and every host out of bounds. At 00:51 IST Rama instructed Claude to check the GitHub repositories and download the chapters if found. That later instruction superseded the restriction for the two chapter files only (Rama's decision D-7). Claude cloned the public records repository, trading-system-records, blob-less, without checkout, at depth 1, read its tree listing (paths only) to locate the two files and to check it against the protected-path list (no protected path among its 1,364 entries), and fetched two blobs, the chapters. The commands are not in the part of the session log that is still kept; the clone's own records show its scope: three packs, the first with the commit ae7335cbc8ad3762a39a9ddde15eb5103e2d6b29 (dated 18-Sep-2026 01:09 IST) and its 27 trees (00:52:45 IST), the others one blob each, f6aadb175eff9ed4e0c1f7db7f4ef3201b314203 (chapter 02, 00:53:26 IST) and 7be2f68f60ef06a2efd6f14a2505083df5e4ba32 (chapter 03, 00:53:27 IST), and no other blob. About 00:54 IST Rama uploaded the same two files from ChatGPT's project; they are byte-identical to the fetched blobs (SHA-256 in section 14.7). No other file of the records repository was read; LEGACY_SYSTEM_MANUAL_INDEX.md has the blob ID of that commit's copy, 75b8aea18630ee6ead49f03e1a7dfa806847aed6 (compared without fetching it). The chapters were read as documents, not as legacy source, through a logged and masked reader: 18 reads, X001 to X018 (the inventory, section 11), separate from the 125 legacy reads.
- **Saved outputs re-viewed (revision 3):** no new legacy source read was performed for revision 3; the code read log still holds 125 reads. While the cross-check was prepared, Claude re-viewed the saved outputs of three earlier logged reads, 011, 019 and 042, to confirm wording at the pin (the companion cross-check, CH-15, CH-26 and CH-55); no result there depends on them. By Rama's decision D-5 it re-viewed the saved output of read 042 once more, at `signals/signal_processor.py:1688-1705` only, for one question: whether `admit_prepared`'s `finally` decrements `_in_flight_count`. It does (`signals/signal_processor.py:1692-1698`); [SS-03] is corrected.
- **ChatGPT's review of the cross-check and the proposed changes (24-Oct-2026, received at 08:05 IST):** "REVIEW ONLY - NOT APPROVED FOR FREEZE". It found the cross-check useful and its totals right, asked that the three conflicts be recorded as differences between documents about two trees and not as proof that either tree was wrong, recommended options for D-1 to D-7 and set twelve guards for revision 3; it did not reproduce Claude's git operations, hashes, masked-reader logs or the 61-row comparison. **Rama's decisions (24-Oct-2026, about 08:08 IST, multiple choice):** D-1 to D-3, option (a) for the three conflicts; D-4, the table kept as a companion file; D-5, yes to the one check of a saved output; D-6, the pointer in [U-01]; D-7, his 00:51 instruction recorded as superseding the restriction for the two chapter files only. Section 19.5 lists each item and what revision 3 does with it.
- **ChatGPT's review of revision 3 (24-Oct-2026, received at 09:13 IST):** "SUBSTANTIVE REVISION-3 REVIEW: PASS, WITH THE ABOVE GOVERNANCE CONDITIONS" and "FREEZE / COMMIT / PUSH: NOT AUTHORISED BY THIS REVIEW". It computed the three files' SHA-256 and line counts and matched the manual's and the cross-check's to the values the inventory declares; it kept [OQ-01] open and erratum B-009 E-3 a freeze-time step (the anchor and the number verified against the live register and main, only the approved text applied, both "DR-0NN" placeholders replaced by the freeze's row), and asked that the release package identify the three hashes, confirm that the files were not edited after its review, keep the companion inventory and cross-check with the manual and record the freeze decision. It did not reproduce Claude's gated reads, masking, protected-path checks, clone and fetch commands, the 125 source reads, the 18 searches or the 61-row comparison. **Rama's approval (24-Oct-2026, 09:15 IST, multiple choice):** "Approve, prepare the freeze" for exactly the three files of revision 3, with ChatGPT to check the freeze package before the push. The freeze is section 19.6.
- **A local language probe** (Python 3.13.16 in Claude's workspace, not the legacy interpreter, not legacy code) was used for one statement about float comparison ([K-11]).

## 2. Construction, wiring and entry points (FACT, SOURCE-VERIFIED unless marked)

### 2.1 Construction

- **[W-01]** main.py imports the processor (`main.py:90`) and constructs one (`main.py:3574-3623`); the gated grep of main.py for `SignalProcessor` and `signal_processor` printed that one constructor call (`main.py:3575`) and no other. The queue it receives is the receiver's queue (`main.py:3407-3408, 3576`; C-006_manual.md section 2.1, HISTORICAL).
- **[W-02]** The arguments passed: the store, the event bus, the fund manager, the position sizer, the risk engine, the kill switch, the market windows, the loaded strategies, the scanner map's scanners, the secondary screener, the quality scorer, the order placer, the worker count and the dispatcher poll interval, the instrument cache, `atr_fallback_mode`, `tgt_min_pct`, the four entry-throttle settings, the notifier, the mode label, the shadow tracker, the S&R detector, the zone warmer, the retest SL buffer, `in_flight_release_fn=webhook_receiver.release_in_flight`, `signal_expiry_sec` from the queue's `expiry_sec`, `quote_fn=broker_adapter.get_quote`, the strategy governor, `trade_type`, `force_intraday_only` and the evidence recorder (`main.py:3575-3623`).
- **[W-03]** Not passed, so the constructor defaults apply: `in_flight_heartbeat_fn` (None), `rate_limiter` (None), `perf_weights` (None, kept as an empty map, so every strategy's performance weight is 1.0), `allocator` and `v3_chain` (None; both late-bound, [W-05]) (`signals/signal_processor.py:171-172, 189, 192, 195-196, 218-219, 233, 236, 239-240, 1120`).
- **[W-04]** The processor keeps its own copies of the strategies and the scanner map, made when it is constructed (`signals/signal_processor.py:207-208`). It builds its entry throttle itself from the four settings (`signals/signal_processor.py:259-268`). It stores the event bus (`signals/signal_processor.py:201`) and makes no other use of it: a gated grep for `self._bus`, `.publish(` and event imports printed only that line, so the processor emits no events of its own. The order placer was passed (`main.py:3061, 3588`), so the "PROCESSED_NO_PLACER" branches ([ST-22]) are not taken on this wiring.
- **[W-05]** Late binding: `set_allocator` is called when `portfolio_allocator.allocator_mode` is not "off" (`main.py:3631-3651`); on a wiring exception main logs and sets it back to None (`main.py:3656-3662`). `set_v3_chain` is called when `v3_chain.v3_chain_mode` is not "off" (`main.py:3472-3473, 3672-3694`), with the same fallback (`main.py:3696-3702`). `set_retest_diverter` is called only when `wait_for_retest_enabled` is on and the zone warmer and zone cache were built (`main.py:3423, 3769, 3789-3793`); the zone warmer is built only when the retest or the structure-exit flag is on (`main.py:3545-3549`). The three setters only store the object (`signals/signal_processor.py:356-359, 1568-1576`).
- **[W-06]** The strategy governor is built whenever the `strategy_circuit_breaker` block exists (`main.py:846-862`); the configuration model gives that block a default instance whose `enabled` defaults to True (`core/config_loader.py:1838-1844, 2023`). The shadow tracker is constructed with `enabled=sh_cfg.enabled` (`main.py:2945-2956`). The evidence recorder is built in a guarded block and is None if building fails (`main.py:3257-3268`).
- **[W-07]** The processor has no entry-gate parameter (`signals/signal_processor.py:155-198`). main builds the EntryGate after the processor, with `on_release=_make_gate_release_cb(signal_processor)` (`main.py:3839-3844`).

### 2.2 Entry points

| ID | Entry point | Called by (production wiring) | Thread | Evidence |
|---|---|---|---|---|
| E1 | `_process_one_safe` -> `_process_one` | the processor's own dispatcher, for each queue item (C-006 section 6) | the processor's pool, "sp-worker" | `signals/signal_processor.py:304-324, 377-399, 405-480` |
| E2 | `continue_from_gate(entry, release_ltp)` | main's gate-release callback, only for a release with reason PRICE_HIT; any other reason is logged "no action" | main's two-worker "gate-release" pool | `main.py:865-893`; `signals/signal_processor.py:2010-2369` |
| E3 | `continue_from_retest(parked)` | the RetestMonitor's `on_confirm`, only on CONFIRMED, after it releases the parking row | the retest monitor's thread (the processor's docstring: "RetestMonitor's poll thread") | `main.py:3784-3788`; `screening/retest_monitor.py:180-190`; `signals/signal_processor.py:2375-2384` |
| E4 | `admit_prepared(candidate)` and `reject_prepared(candidate, reason)` | the PortfolioAllocator's enforce worker (`enforce_admit_fn`, `enforce_reject_fn`) | the allocator's worker thread "allocator-window" | `main.py:3636-3649`; `allocation/portfolio_allocator.py:105-113`; `signals/signal_processor.py:1646-1737` |

Basis for the callers: main.py's wiring and the callee lines cited in the table. The Stage 0 count-only search Q04 (the names `_process_one`, `continue_from_gate`, `continue_from_retest` and `reject_prepared`, over 1,260 files) matched six runtime Python files: the processor; main.py (6 lines, all printed by the reads cited); screening/retest_monitor.py (3 lines, 2 of them printed, `screening/retest_monitor.py:51, 189`); allocation/portfolio_allocator.py (1 line, not printed); and v3_chain/pb01_runner.py (2 lines) and v3_chain/pb01_entry.py (1 line), whose gated greps, which also searched `_process_one_safe` and `admit_prepared`, printed docstring text only (`v3_chain/pb01_runner.py:6-7`; `v3_chain/pb01_entry.py:9`). E4's two callbacks are passed by main.py (`main.py:3644-3645`); the allocator's worker that calls them was not read. A caller holding an entry point under another name is found only through the wiring read.

Which of these ran on the pinned wiring and repository configuration is set out in section 9: E1 is the live path; E2 is reachable only from gate rows reloaded at start ([EP-02]); E3 is not wired with the repository setting ([EP-05]); in "shadow" mode the processor hands nothing to E4 ([EP-07]).

### 2.3 Start and stop order

- **[W-08]** Start, the steps around the processor's start (the reconcile's place among the other start steps, after the threads that construction had already started, is B-005's: its section 21 as corrected by erratum E-1, HISTORICAL): the synchronous startup reconcile runs (`main.py:3880-3881`); an active kill after it ends the start (`main.py:3894-3899`); an assertion checks that the reconcile ran (`main.py:3968-3971`), a Python `assert` statement (FACT (EXTERNAL), language level only: Python generates no code for an `assert` statement when optimisation is requested, for example with the -O option; the Python 3 Language Reference, section 7.3, https://docs.python.org/3/reference/simple_stmts.html#the-assert-statement); whether the production service ran with optimisation is UNKNOWN / NOT ESTABLISHED ([U-06]), and the 18-Sep manual's chapter 02 leaves the same point open for its own tree (section 14.7); then the processor starts, then the entry gate: "signal_processor BEFORE entry_gate (BLOCKER #5 fix): gate may call continue_from_gate() immediately on release; workers must be ready" (`main.py:3972-3975`). The processor's start creates the pool and the daemon dispatcher (`signals/signal_processor.py:304-324`; C-006 section 6).
- **[W-09]** Stop: after the flatten drain, the end-of-day census and the token step (`main.py:1643-1679`), the shutdown function stops the webhook receiver ("FIRST" in its comment), then the processor (`main.py:1681-1692`), then the S&R observer, the regime runner, the allocator, the V3 chain, the PB-01 workers, the retest monitor, the structure-exit manager, the zone warmer, and after them the entry gate (`main.py:1693-1750`). The processor's stop sets its stop event, joins the dispatcher for 10 s, waits for the pool, then shuts the screener's executor down (`signals/signal_processor.py:326-347`; C-006 section 8.1).
- **[W-10] INTERPRETATION:** E2 and E3 run on threads the processor's stop does not wait for (the table in section 2.2), and their gate and retest owners are stopped after the processor. The inspected code path would let a release or a confirmation that arrives after the processor's stop run its pipeline up to the shutdown check, which rejects it as `REJECTED_SHUTDOWN` (`signals/signal_processor.py:2215-2222, 2524-2528`). Whether this happened is not established.

## 3. Configuration the processor reads (FACT (CONFIG) and FACT, SOURCE-VERIFIED)

Repository values at the pin, with the configuration model's default where it differs. These are not evidence of a deployed host's configuration.

| ID | Setting | Repository value | Model default | Use by the processor | Evidence |
|---|---|---|---|---|---|
| CF-01 | `signal_queue.expiry_sec` | 600 | - | passed as `signal_expiry_sec` (constructor default 60); the age check ([ST-06]) | `config/system_config.yaml:127`; `main.py:3613-3614`; `signals/signal_processor.py:175, 222` |
| CF-02 | `signal_processor.worker_count`, `drain_poll_sec` | 5; 0.1 | required (1 to 50; > 0) | pool size and dispatcher poll (C-006 section 3) | `config/system_config.yaml:345-346`; `core/config_loader.py:566-567, 588-600`; `main.py:3590-3591` |
| CF-03 | `signal_processor.pipeline_timeout_sec` | 30, commented "hard per-signal processing deadline" | required (>= 1) | not passed to the processor; no runtime file outside the configuration loader names it (Q36: 281 non-test Python files; 3 lines, all in core/config_loader.py) | `config/system_config.yaml:347`; `core/config_loader.py:568, 602-607`; `main.py:3575-3623` |
| CF-04 | `signal_processor.atr_fallback_mode` | absent from the block | "WARN" (Literal WARN or HALT) | the ATR branches of the price and target helpers ([K-01]) | `config/system_config.yaml:344-353`; `core/config_loader.py:569-572`; `main.py:3593` |
| CF-05 | `signal_processor.tgt_min_pct` | 0.003 | 0.003 | the target-distance check ([ST-23]) | `config/system_config.yaml:348`; `core/config_loader.py:573-575`; `main.py:3594` |
| CF-06 | the entry throttle: `min_gap_between_entries_sec`, `entry_burst_window_sec`, `entry_burst_max`, `per_symbol_cooldown_sec` | 20; 60; 3; 300 | 0.0; 60.0; 0; 0.0 ("All default OFF") | the throttle ([ST-27]) | `config/system_config.yaml:349-353`; `core/config_loader.py:576-586`; `main.py:3595-3599` |
| CF-07 | `risk.one_trade_per_symbol_direction_per_day` | true | False | passed to the risk engine, whose private attribute the processor reads ([ST-21]) | `config/system_config.yaml:302`; `core/config_loader.py:784`; `main.py:2896, 2901`; `capital/risk_engine.py:203`; `signals/signal_processor.py:834` |
| CF-08 | `trade_type`; `force_intraday_only` | BOTH; false | "INTRADAY"; True | the strategy-control gate ([ST-09]) | `config/system_config.yaml:142, 149`; `core/config_loader.py:2030, 2044`; `main.py:3620-3621` |
| CF-09 | `portfolio_allocator.allocator_mode`; `enforce_scope` | "shadow"; "v3_only" | "off" for `allocator_mode`; `enforce_scope`'s not read | the allocator is built and late-bound; its hook observes ([ST-19]) | `config/system_config.yaml:564-566`; `core/config_loader.py:1325`; `main.py:3632-3633` |
| CF-10 | `v3_chain.v3_chain_mode` | "shadow" | "off" | the V3 chain is built and late-bound; its hook observes ([ST-18]) | `config/system_config.yaml:573-574`; `core/config_loader.py:1405`; `main.py:3472-3473` |
| CF-11 | `sr_detector.wait_for_retest_enabled`; `structure_exit.structure_exit_enabled` | false; false | False; - | no zone warmer and no retest diverter are built ([W-05], [EP-05]) | `config/system_config.yaml:529, 634`; `core/config_loader.py:1200`; `main.py:3423, 3427, 3549` |
| CF-12 | `sr_detector.enabled` | true | - | the S&R observer hook after placement ([OB-03]) | `config/system_config.yaml:509`; `main.py:3422` |
| CF-13 | `strategy_circuit_breaker.enabled` | true | True | the governor's check ([ST-11]) | `config/system_config.yaml:732-736`; `core/config_loader.py:1841` |
| CF-14 | `shadow_tracker.enabled` | false, commented "DISABLED 25-Jul-2026 - CONFIG ONLY" and "all handlers no-ops" | - | the tracker object is still passed and consulted ([ST-07]) | `config/system_config.yaml:492-504`; `main.py:2945-2956, 3605` |
| CF-15 | Strategy files (16, `config/strategies/`) | `pullback_wait_enabled: true` in 6 (first_pullback_long and _short, open_high_breakdown_short, open_low_breakout_long, vwap_bounce_long, vwap_rejection_short; Q43); `enabled: false` in 1 (pb01_breakout_retest; Q44); no `sl_method` or `tgt_method` set to ATR (Q45; its positive control Q50 finds both keys in all 16 files); `v3_playbook: true` in 1 (pb01_breakout_retest; Q46); intent DELIVERY in 3 (the three positional strategies; Q47) | - | the re-anchor ([ST-16]), the control gate ([ST-09]), the ATR branches ([K-01]), the allocator scope ([EP-07]) | count-only searches Q43 to Q47 and Q50 over the 16 files (per-file counts; no line printed); key names from `config/strategies/gap_go_long.yaml:9-11, 18, 21, 28, 37, 47` |

## 4. The main path, stage by stage (E1: `_process_one` and `_admit_and_place`; FACT, SOURCE-VERIFIED unless marked)

### 4.1 Stage table

The stages run in this order for each dequeued item. A `_PipelineReject(check, reason)` raised at any stage ends the pipeline: the processor writes `REJECTED_<check>` with the reason, then releases a reservation if one is held (unless that write raised, [K-05]), counts the reject and, for `EXPIRED`, sends a warning at most once a minute (`signals/signal_processor.py:94-100, 1193-1231`). "Claim" is the receiver's per-symbol in-flight claim (C-005 section 6); it is released in the `finally` of `_process_one` on every exit except a re-queue or an allocator hand-off (`signals/signal_processor.py:1248-1271`). The rule behind each check belongs to the owner named.

| ID | Stage | Code | Condition and outcome | Owner of the rule |
|---|---|---|---|---|
| ST-01 | Rate-limit pre-check (FIX-007) | `signals/signal_processor.py:415-461` | runs only when a rate limiter was passed; none is (`main.py:3575-3623`), so it does not run (C-006 section 5.1) | C-006 |
| ST-02 | Unpack | `signals/signal_processor.py:876-887` | a 5-tuple (retry count 0) or a re-queued dict (with its retry count) | C-004 section 6.5; C-006 |
| ST-03 | `PROCESSING` | `signals/signal_processor.py:895-899` | written by `signal_id` before any check; the runtime counter `signals_processed` is bumped; the heartbeat checkpoint is a no-op ([K-09]) | C-007 S-04 |
| ST-04 | Kill switch | `signals/signal_processor.py:905-907`; `capital/kill_switch.py:510-526` | `is_active("entry")` is true for SOFT_KILL or HARD_KILL: `REJECTED_KILL_SWITCH` | I topics |
| ST-05 | Global entry window | `signals/signal_processor.py:909-912`; `core/market_windows.py:143-150` | false on a trading holiday or outside `entry_start <= t < entry_end`: `REJECTED_OUTSIDE_ENTRY_WINDOW` | A-003 |
| ST-06 | Age | `signals/signal_processor.py:914-921` | the item's `triggered_at`, taken as IST, against now; over 600 s ([CF-01]): `REJECTED_EXPIRED`; the warning is rate-limited to one a minute (`signals/signal_processor.py:1221-1231`) | C-009 |
| ST-07 | Shadow inning | `signals/signal_processor.py:923-928, 1976-2008` | if a tracker object is set and `is_tracking(symbol)` is true: `REJECTED_SHADOW_INNING_ACTIVE`; if the call raises, it fails closed: `REJECTED_SHADOW_TRACKER_ERROR`; with no tracker object, it passes | shadow tracker owner; [U-01] |
| ST-08 | Strategy lookup | `signals/signal_processor.py:930-953` | scanner missing from the map, an entry without a strategy, or a strategy not loaded: `REJECTED_UNKNOWN_STRATEGY` | C-010; C-001 section 7.4 |
| ST-09 | Strategy control | `signals/signal_processor.py:955-972`; `strategies/control.py:67-117` | the verdict's cause TRADE_TYPE: `REJECTED_TRADE_TYPE`; any other refusal (a disabled strategy, the force breaker on a DELIVERY strategy): `REJECTED_STRATEGY_CONTROL`. With the repository values BOTH and false ([CF-08]) only a disabled strategy is refused here, if the loader keeps disabled strategies in the loaded set (strategies/loader.py not read; otherwise ST-08 refuses them) | A-008 |
| ST-10 | Strategy window | `signals/signal_processor.py:974-982`; `core/market_windows.py:152-175` | the global window again and the strategy's own times; missing or malformed times fall back to the global result: `REJECTED_OUTSIDE_ENTRY_WINDOW`. A time that parses but is out of range (an hour over 23, say) raises in `time()` outside the helper's try (`core/market_windows.py:174-175`); INTERPRETATION: the inspected code path would take it to the generic handler, `PLACEMENT_FAILED` ([EX-03]), unless the strategy loader's validation (not read) excludes it | A-003, A-008 |
| ST-11 | Strategy circuit breaker | `signals/signal_processor.py:984-990`; `capital/strategy_governor.py:49-87` | `(paused, reason)`; paused: `REJECTED_STRATEGY_CIRCUIT_BREAKER`; not paused: the path continues. When the governor pauses is its owner's rule; it catches its own database errors and answers not paused ("fail-open", `capital/strategy_governor.py:67-72`) | F topics |
| ST-12 | Secondary screening | `signals/signal_processor.py:992-1023`; `screening/secondary_screener.py:123-196` | called with `market_data=None`, so the screener fetches its own quote; it writes the verdict status itself (C-007 S-13); a `SKIPPED_*` status or a failed verdict makes the processor return with no status of its own; a pass continues | D topics |
| ST-13 | Entry and SL derivation | `signals/signal_processor.py:1025-1030, 1748-1846` | from the trigger price: `REJECTED_INVALID_DERIVED_PRICE` (entry <= 0, or an unknown SL method), `REJECTED_REJECTED_NO_ATR_DATA` (ATR method with HALT; [K-01]), `REJECTED_ZERO_SL` (FIXED_PCT with sl_pct <= 0) | E topics |
| ST-14 | Side | `signals/signal_processor.py:1032-1035` | direction LONG or BUY gives BUY, anything else SELL | - |
| ST-15 | Retest divert | `signals/signal_processor.py:1037-1056`; `screening/retest_monitor.py:303-379` | only with a diverter whose `enabled` is true; none is set ([CF-11]); when it diverts (RETEST_WAITING or REJECTED_RETEST_DUP, written by the diverter) the processor returns before sizing, holding no capital | D topics |
| ST-16 | Momentum re-anchor (FIX-067 / M-S1) | `signals/signal_processor.py:1058-1106` | only when the strategy's `pullback_wait_enabled` is false and a quote function is set (it is, `main.py:3616`): entry and SL are derived again from the live LTP; on a quote error or no LTP the trigger-based prices stay; the 6 strategies with `pullback_wait_enabled: true` ([CF-15]) skip it and wait nowhere (B-009 D1; [EP-02]) | E topics |
| ST-17 | Sizing | `signals/signal_processor.py:1108-1129`; `capital/position_sizer.py:245-339` | `calculate(...)` returns a result; not successful: `REJECTED_SIZING_<constraint>` ([OC-03]); a BrokerError: `record_api_failure` and `REJECTED_SIZING_BROKER_ERROR`; a ValueError from the sizer's input checks is not caught here ([EX-03]) | F-004 |
| ST-18 | V3 chain hook (shadow) | `signals/signal_processor.py:1131-1143, 1578-1603`; `v3_chain/runner.py:99-118` | with a chain set ([CF-10]), a snapshot is built and `observe()` called inside a guard; observe enqueues without blocking and drops the copy when its queue is full; nothing is rejected | J-010 |
| ST-19 | Allocator hook | `signals/signal_processor.py:1145-1172`; `allocation/portfolio_allocator.py:69-102` | "shadow" ([CF-09]): a candidate copy is appended to the allocator's buffer and the path continues; "enforce" with an in-scope strategy: the candidate is submitted, `handed_off` is set and the path returns, keeping the claim ([EP-07]); any allocator error is logged and ignored | J-012 |
| ST-20 | Processor in-flight count | `signals/signal_processor.py:1298-1305` | incremented before the lock; decremented in `finally` ([SS-03]) | - |
| ST-21 | Under the portfolio lock | `signals/signal_processor.py:1307-1344`; `capital/fund_manager.py:426-443` | in order: the per-strategy position cap (`REJECTED_STRATEGY_POSITION_LIMIT`, `signals/signal_processor.py:744-779`); one executed trade per symbol and direction per day, when the risk engine's flag is on ([CF-07]; `REJECTED_SYMBOL_DIRECTION_DAILY_LIMIT`, `signals/signal_processor.py:802-864`); `risk.approve(...)` (`REJECTED_<failed_check>`, [OC-03]; a BrokerError: `REJECTED_RISK_BROKER_ERROR`); `fm.reserve(...)` (`REJECTED_RESERVE_FAILED`, or `REJECTED_RESERVE_BROKER_ERROR`); then `RESERVED`, written inside the lock | F topics; A-008 section 14 |
| ST-22 | No placer | `signals/signal_processor.py:1351-1363` | with no order placer: release, `PROCESSED_NO_PLACER`; not taken on the pinned wiring ([W-04]) | - |
| ST-23 | Target derivation | `signals/signal_processor.py:1365-1368, 1901-1953` | after `RESERVED`: `REJECTED_UNKNOWN_TGT_METHOD`, `REJECTED_TGT_DISTANCE_TOO_SMALL` (distance under 0.3 % of entry, [CF-05]) or `REJECTED_REJECTED_NO_ATR_DATA`; the reject handler releases the reservation ([K-02]) | E topics |
| ST-24 | Signal alert | `signals/signal_processor.py:1370-1380, 486-555` | "INTRADAY SIGNAL" sent through the notifier in a guard that never raises; before the next three checks and before placement (B-009 section 9, HISTORICAL; [D-08]) | L topics |
| ST-25 | Kill switch, late (FIX-070) | `signals/signal_processor.py:1384-1396` | release ("kill_switch_after_pipeline"), `REJECTED_KILL_SWITCH_LATE` | I topics |
| ST-26 | Shutdown (FIX-070) | `signals/signal_processor.py:1398-1405` | the processor's stop event is set: release ("shutdown_before_placement"), `REJECTED_SHUTDOWN` | C-006 section 8.1 |
| ST-27 | Entry throttle | `signals/signal_processor.py:1407-1418`; `signals/entry_throttle.py:65-115` | `admit(symbol)`: global minimum gap, then the burst window, then the per-symbol cooldown ([CF-06]); refused: release ("entry_throttled"), the `entries_throttled` counter, `REJECTED_ENTRY_THROTTLED`; allowed: the placement is recorded at once ([K-04]) | open agenda mapping ([OQ-01]) |
| ST-28 | Dispatch record | `signals/signal_processor.py:1420-1445` | the effect-telemetry counter and the P1 evidence record ("ACCEPTED"), both before the call | L topics |
| ST-29 | Placement | `signals/signal_processor.py:1446-1463`; `orders/order_placer.py:871-958` | `place(...)` with symbol, side, quantity, entry, SL, intent, `signal_id`, `reservation_id`, strategy, target, trigger price, sizing breakdown and the strategy's R:R; it returns None and raises on failure | G topics |
| ST-30 | Placement outcomes | `signals/signal_processor.py:1461-1546` | success: the reservation handle passes to the placer, `entries_placed`, `PROCESSED` (`signals/signal_processor.py:1540-1546`); `BrokerTimeoutError`: `TIMEOUT` ([EX-05]); `BrokerRateLimitError`: re-queue ([RQ-01]); any other exception: re-raised to the outer handler, `PLACEMENT_FAILED` and an attempted release ([EX-04], [OC-04]) | G topics |
| ST-31 | S&R observer | `signals/signal_processor.py:1548-1561, 614-658` | after `PROCESSED` only: a candidate is handed to the S&R detector inside a guard ([OB-03]) | J topics |
| ST-32 | `finally` | `signals/signal_processor.py:1248-1276` | the in-flight count is decremented if it was incremented; the claim is released by symbol unless re-queued or handed off; the elapsed time is added to the stats | C-005 section 6.1 |

### 4.2 What the main path shares with the other paths

- **[ST-33]** The kill, window, lookup, control, strategy-window and breaker checks, the lock section, the late kill and shutdown checks, the throttle and the placement handlers are repeated, with their own copies of the code, in `continue_from_gate` and `continue_from_retest` (section 9). Three checks are one shared helper each, called by all three paths: the shadow-inning guard, "HOISTED (M-S5) so ALL THREE entry paths ... enforce it identically" (`signals/signal_processor.py:1976-1983`; called at `signals/signal_processor.py:928, 2048, 2405`), the per-strategy cap and the symbol-direction rule (`signals/signal_processor.py:1307-1313, 2138-2143, 2474-2479`). The allocator's enforce worker reuses `_admit_and_place` itself (`signals/signal_processor.py:1646-1660`).
- **[ST-34]** The processor reads no signals row: its only direct query is the per-strategy cap's count of trades (`signals/signal_processor.py:761-767`; the gated grep for `fetch_one`, `fetch_all`, `SELECT`, `get_signal` and `signals WHERE` printed only `signals/signal_processor.py:761-762`). Every decision uses the queue item's own values and the injected components (`signals/signal_processor.py:876-887`).

### 4.3 Trace table (SOURCE -> INPUT -> LOGIC -> GUARD -> OUTPUT -> STORAGE / HAND-OFF -> NEXT CONSUMER)

| ID | Stage | Source | Input | Logic | Guard | Output | Storage / hand-off | Next consumer |
|---|---|---|---|---|---|---|---|---|
| TR-01 | Intake to worker | the dispatcher (C-006) | queue item: 5-tuple or re-queued dict | unpack; `PROCESSING` | none before the write | status | signals.status (C-007) | ST-04 |
| TR-02 | Pre-flight checks | `signals/signal_processor.py:905-928` | the item; kill state; clock; tracker | four checks in order | kill (entry), global window, age 600 s, shadow inning | reject or continue | `REJECTED_<check>` | ST-08 |
| TR-03 | Strategy resolution | `signals/signal_processor.py:930-990` | scanner name; the processor's copies of the map and strategies | lookup; control verdict; strategy window; breaker | the four checks | strategy object, or reject | `REJECTED_<check>` | ST-12 |
| TR-04 | Screening | `screening/secondary_screener.py:123-196` | `signal_id`, symbol, scanner, trigger price and time, direction, intent, strategy | the screener's steps (D topics) | its own quote fetch and steps | ScreeningResult (passed, status, score, tier) | the screener's own status write (C-007 S-13) | ST-13, or return |
| TR-05 | Prices | `signals/signal_processor.py:1028-1106` | trigger price or live LTP; strategy | entry and SL derivation; side | entry > 0, SL method, sl_pct > 0 | entry, SL, side | none | ST-15, ST-17 |
| TR-06 | Sizing | `capital/position_sizer.py:245-339` | symbol, side, entry, SL, intent, tier, lot size, weight 1.0 | the sizer's formula (F-004) | input checks (ValueError); success flag | SizingResult (qty, constraint, breakdown) | none | ST-18 |
| TR-07 | Shadow hooks | `signals/signal_processor.py:1131-1172` | the sized candidate | V3 observe; allocator observe or submit | mode, scope | a copy queued; or a hand-off | the chain's queue; the allocator's buffer | the chain's and allocator's workers; or ST-20 |
| TR-08 | Admission | `signals/signal_processor.py:1302-1344` | the candidate; processor in-flight count | cap; symbol-direction; approve; reserve | under the portfolio lock | reservation, or reject | fund manager reservation; `RESERVED` | ST-23 |
| TR-09 | Pre-placement | `signals/signal_processor.py:1365-1418` | entry, SL, strategy | target; alert; late kill; shutdown; throttle | target distance, kill, stop event, throttle gates | target, or reject with release | notifier message; throttle record | ST-29 |
| TR-10 | Placement | `orders/order_placer.py:871-958` | the order fields and `reservation_id` | the placer's protocol (G topics) | the placer's own checks | None, or an exception | trades and orders (G topics); reservation owned by the placer | ST-30 |
| TR-11 | Outcome | `signals/signal_processor.py:1461-1546` | the call's result | success, timeout, rate limit, other | exception class | final status, or re-queue | `PROCESSED`, `TIMEOUT`, `PLACEMENT_FAILED`; queue | the order monitor and reconciler (G topics); the queue (C-006) |
| TR-12 | Clean-up | `signals/signal_processor.py:1248-1276` | flags `requeued`, `handed_off`, `in_flight_incremented` | decrement; release by symbol | the two flags | claim released or kept | the receiver's claim map (C-005) | the next signal for the symbol |

## 5. Outcomes and bookkeeping (FACT, SOURCE-VERIFIED unless marked)

### 5.1 Statuses the processor writes

- **[OC-01]** Every write goes through the store's `update_signal_status` by `signal_id`, with no check of the row's current status (C-007 K-01, HISTORICAL; BK-19).

| Status | Written at | Note |
|---|---|---|
| `PROCESSING` | `signals/signal_processor.py:897, 2034, 2393` | first write of E1, E2 and E3 |
| `RESERVED` | `signals/signal_processor.py:1344, 2171, 2501` | inside the portfolio lock |
| `PROCESSED` | `signals/signal_processor.py:1543, 2290, 2583` | after `place()` returned |
| `PROCESSED_NO_PLACER` | `signals/signal_processor.py:1360, 2186, 2509` | not reachable on the pinned wiring ([W-04]) |
| `TIMEOUT` | `signals/signal_processor.py:1480-1481, 2278-2279, 2573-2574` | reason: the exception text |
| `PLACEMENT_FAILED` | `signals/signal_processor.py:473-475, 1238, 1683, 2345, 2620` | reason: the exception text ("unhandled: ..." in `_process_one_safe`) |
| `REJECTED_<check>` | `signals/signal_processor.py:1201, 1662, 2316-2317, 2598` | reason: the reject's reason |
| `REJECTED_<reason>` | `signals/signal_processor.py:1712-1713` | the allocator's pre-check (E4), reason "allocator pre-check: ..." |
| `REJECTED` (reason `QUEUE_FULL`) | `signals/signal_processor.py:435-437` | the unwired pre-check (ST-01) |

The screener's verdicts and the retest diverter's statuses are written by those modules, not by the processor (`signals/signal_processor.py:992-996`; `screening/retest_monitor.py:349-351, 373`; C-007 S-13, S-16, S-17).

### 5.2 Reject names

- **[OC-02]** The processor's own check names, each raised as `_PipelineReject` (located by a gated grep of every `raise` in the file and read in the ranges of section 4):

| Check (stored as `REJECTED_<check>`) | Raised at |
|---|---|
| `KILL_SWITCH` | `signals/signal_processor.py:907, 2039, 2398` |
| `OUTSIDE_ENTRY_WINDOW` | `signals/signal_processor.py:912, 979, 2044, 2080, 2401, 2422` |
| `EXPIRED` | `signals/signal_processor.py:919` |
| `SHADOW_INNING_ACTIVE`; `SHADOW_TRACKER_ERROR` | `signals/signal_processor.py:1994, 2006` |
| `UNKNOWN_STRATEGY` | `signals/signal_processor.py:936, 945, 951, 2055, 2409` |
| `STRATEGY_CONTROL` | `signals/signal_processor.py:971, 2074, 2417` |
| `STRATEGY_CIRCUIT_BREAKER` | `signals/signal_processor.py:990, 2093, 2430` |
| `SIZING_BROKER_ERROR` | `signals/signal_processor.py:1125, 2121, 2463` |
| `STRATEGY_POSITION_LIMIT`; `SYMBOL_DIRECTION_DAILY_LIMIT` | `signals/signal_processor.py:775, 860` |
| `RISK_BROKER_ERROR` | `signals/signal_processor.py:1322, 2152, 2487` |
| `RESERVE_BROKER_ERROR`; `RESERVE_FAILED` | `signals/signal_processor.py:1335, 1338, 2165, 2168, 2497, 2499` |
| `KILL_SWITCH_LATE`; `SHUTDOWN`; `ENTRY_THROTTLED` | `signals/signal_processor.py:1396, 1405, 1417, 2213, 2222, 2232, 2523, 2528, 2536` |
| `INVALID_DERIVED_PRICE`; `REJECTED_NO_ATR_DATA`; `ZERO_SL` | `signals/signal_processor.py:1790, 1805, 1827, 1844, 1916` |
| `UNKNOWN_TGT_METHOD`; `TGT_DISTANCE_TOO_SMALL` | `signals/signal_processor.py:1939, 1947` |
| `RETEST_BAD_STRUCTURE` | `signals/signal_processor.py:2450` |

- **[OC-03]** Names computed from callees (the processor stores `REJECTED_` plus the value):
  - the control gate's cause: `TRADE_TYPE` (`strategies/control.py:41-44`; `signals/signal_processor.py:970-972`);
  - `SIZING_<constraint>` (`signals/signal_processor.py:1127-1128`), with the sizer's failure constraints located by a gated grep, each `success=False` line followed by its constraint as in the two ranges read: `INVALID_SL_DISTANCE`, `QTY_EXPLOSION_GUARD`, the binding constraint `CAPITAL`, `RISK` or `CONCENTRATION` when no quantity is possible, `ZERO_MULTIPLIER`, `REJECTED_LOT_SKEW`, `POSITION_VALUE_CAP` and `BELOW_MIN` (`capital/position_sizer.py:442, 447, 475, 480, 502-512, 524-539, 593, 598, 670, 675, 692-705, 739, 744, 771, 776`);
  - the risk engine's `failed_check`, the first failing of ten checks in this order: `KILL_SWITCH`, `SIZING_VALID`, `CAPITAL`, `OPEN_POSITIONS`, `DAILY_TRADES`, `CONSECUTIVE_LOSSES`, `DAILY_LOSS`, `SECTOR_EXPOSURE`, `CONTRARY_POSITION`, `DUPLICATE_SYMBOL` (`capital/risk_engine.py:84-105, 506-519, 522, 530, 544, 643, 676, 692, 735, 768, 789, 791-794`; `signals/signal_processor.py:1324-1325`);
  - the allocator's pre-check reasons (E4 only): not read ([U-08]).
  This answers C-007 U-06 for the processor and the callees read, bounded to those files. The doubled forms are in [K-01]; names shared by two sources are in [K-08].

### 5.3 Reservation, claim, counters and evidence

- **[OC-04]** Reservation by outcome on E1: none before ST-21; after it, a later reject releases it, either explicitly with its own reason (`kill_switch_after_pipeline`, `shutdown_before_placement`, `entry_throttled`; `signals/signal_processor.py:1393-1395, 1402-1404, 1412-1414`) or through the reject handler as `rejected_<check>` (`signals/signal_processor.py:1212-1217`); success hands it to the placer (`signals/signal_processor.py:1461`); a timeout drops the handle without releasing it ([EX-05]); on a re-queue the processor calls release as `requeued_transient_error` ([RQ-01]); on any other failure it calls release as `placement_failed` (`signals/signal_processor.py:1239-1243`). The explicit releases come before the reject is raised; the two handler releases (`rejected_<check>`, `placement_failed`) run only if the handler's own status write before them succeeds ([K-05]). The fund manager's release returns False for an unknown reservation, "already released or unknown", and writes a RELEASE ledger entry for a known one (`capital/fund_manager.py:637-660`; F-013).
  - On a broker failure inside `place()` (a rejection, an exhausted 429 retry, or, per B-004, any other `BrokerError`) the placer's own clean-up receives the reservation id first: its docstring says it raises "after cleanup" (`orders/order_placer.py:893-894`), and its clean-up calls pass `reservation_id`, one commented "release reservation" (`orders/order_placer.py:1390-1402, 1416-1418`); HISTORICAL (B-004 section 7.3): any other `BrokerError` is "marked FAILED with its reservation released" by the placer. The body of that clean-up was not read. INTERPRETATION: where the placer has released first, the processor's `placement_failed` call finds no reservation and returns False (`capital/fund_manager.py:651-653`); whether the placer's clean-up also runs before a client-side `BrokerRateLimitError` reaches the processor is not established ([U-03]).
- **[OC-05]** The receiver's claim, by entry point: E1 releases it in `finally` unless re-queued or handed off (`signals/signal_processor.py:1260-1271`) and the unwired pre-check releases it on its full-queue branch (`signals/signal_processor.py:456-460`); E2 never releases it (its docstring: "in_flight tracking is NOT released here -- the EntryGate already manages it", `signals/signal_processor.py:2018-2019`; its `finally`, `signals/signal_processor.py:2357-2369`); E3 never releases it (`signals/signal_processor.py:2630-2640`), the claim having been released when E1 returned after the divert (`signals/signal_processor.py:1056, 1267-1271`); E4 releases it in `admit_prepared`'s `finally` unless re-queued and in `reject_prepared` (`signals/signal_processor.py:1699-1703, 1733-1737`). The release is by symbol, through the receiver's function (C-005 section 6.1; BK-03).
- **[OC-06]** Counters: the runtime metrics `signals_processed`, `entries_rejected` (on E1 to E3, every `_PipelineReject` except `ENTRY_THROTTLED`: `signals/signal_processor.py:1197-1200, 2314-2315, 2596-2597`), `entries_throttled` and `entries_placed` (`signals/signal_processor.py:270-279, 898, 1415, 1463`), read by main's metrics provider (`main.py:4076`; C-006 section 9). On E4, `admit_prepared`'s reject handler and `reject_prepared` bump no runtime metric (`signals/signal_processor.py:1661-1680, 1706-1737`). The `PLACEMENT_FAILED` and `TIMEOUT` handlers bump none of the four (`signals/signal_processor.py:1233-1246, 1464-1484`). The stats map counts rejects by check, `PLACEMENT_FAILED`, processed and placed entries, screener outcomes and timings (`signals/signal_processor.py:281-294, 1218-1220, 1244-1246, 1273-1276, 1544-1546, 1959-1970`).
- **[OC-07]** Evidence: a P2 reject record in every reject handler and a P1 accept record at every dispatch (`signals/signal_processor.py:1202-1211, 1427-1445, 1663-1672, 1716-1727, 2241-2250, 2319-2328, 2543-2552, 2599-2606`; the unwired pre-check, `signals/signal_processor.py:443-455`); the capture never raises (`signals/signal_processor.py:582-612`). The `PLACEMENT_FAILED` and `TIMEOUT` handlers make no evidence capture (`signals/signal_processor.py:1233-1246, 1464-1484`). L-008 owns evidence.

## 6. Exceptions and failure handling (FACT, SOURCE-VERIFIED unless marked)

- **[EX-01]** A `_PipelineReject` ends the path through the reject handler (section 4.1, the note above the table; `signals/signal_processor.py:1193-1231`).
- **[EX-02]** A `BrokerError` from the sizer, the risk check or the reservation is passed to the kill switch's `record_api_failure` and turned into `SIZING_BROKER_ERROR`, `RISK_BROKER_ERROR` or `RESERVE_BROKER_ERROR` (`signals/signal_processor.py:1111-1125, 1314-1322, 1327-1335`). The kill switch counts only `BrokerTimeoutError` and `BrokerRateLimitError` toward its automatic SOFT_KILL, logs a `BrokerAuthError` at CRITICAL without counting it, and ignores other classes (`capital/kill_switch.py:791-880`; I-006).
- **[EX-03]** Any other exception inside the main path reaches the generic handler: `PLACEMENT_FAILED` with the exception text, then the release call for a held reservation ([OC-04], [K-05]), the stats counter (`signals/signal_processor.py:1233-1246`). Examples from the callees read: the sizer's ValueError for invalid inputs (`capital/position_sizer.py:300-339`); the fund manager's documented `CapitalInvariantViolation` and `RuntimeError` (`capital/fund_manager.py:536-538, 645-646`); an exception from `screen()` other than the quote errors the screener turns into `SKIPPED_QUOTE_UNAVAILABLE` itself (`screening/secondary_screener.py:167-196`). The strategy governor, by contrast, catches its own database errors and answers not paused (`capital/strategy_governor.py:67-72`), so those do not reach the handler. Which of these occurred is not established.
- **[EX-04]** From `place()`: a `BrokerError` other than the two handled classes is recorded and re-raised, and any other exception is re-raised (`signals/signal_processor.py:1533-1538`); both end as `PLACEMENT_FAILED`, with the processor's release call ([OC-04]). The placer's docstring says so ("Raises BrokerError on hard broker failure (after cleanup). signal_processor catches this and marks signal PLACEMENT_FAILED", `orders/order_placer.py:890-894`). Its R:R gate, which runs only when its minimum R:R setting is above 0 (`orders/order_placer.py:938`; the setting's value was not traced), raises `OrderRejectedError` before any broker call (`orders/order_placer.py:937-958`), so that refusal is stored as `PLACEMENT_FAILED`, not as a `REJECTED_` status.
- **[EX-05]** `BrokerTimeoutError` from `place()`: recorded (it counts toward the auto-trip), `TIMEOUT` with the exception text, the reservation handle dropped without release, return (`signals/signal_processor.py:1464-1484`). The placer, on catching that class, sets the trade `UNKNOWN_IN_FLIGHT`, adds it to its timeout-recovery queue and re-raises (`orders/order_placer.py:1433-1465`). The class is raised in the adapter (2 lines) and the reconciler (1 line) among the 281 non-test Python files (Q40). HISTORICAL (B-004 section 7.3): on the inspected adapter path a refused connection or a read timeout reaches callers as the generic `BrokerError`, not as `BrokerTimeoutError` (B-004: a latent code path, no occurrence found). INTERPRETATION: such a failure would take [EX-04]'s path (`PLACEMENT_FAILED`, the reservation released, [OC-04]) rather than `TIMEOUT`.
- **[EX-06]** The placer's own 429 loop: a count-only search found one line `except BrokerRateLimit429Error as rl_exc` in the placer (Q42); the printed loop shows the caught class masked, a retry until its limit, and on exhaustion the placer's failure clean-up and a re-raise (`orders/order_placer.py:1403-1432`). HISTORICAL (B-004 sections 7.1 and 7.4): that class is a `BrokerError` subclass distinct from the client-side `BrokerRateLimitError`, and the placer retries 429 failures at most three times. INTERPRETATION: an exhausted 429 would therefore reach the processor's [EX-04] branch (`PLACEMENT_FAILED`), not its re-queue.
- **[EX-07]** The client-side `BrokerRateLimitError` is raised in broker/rate_limiter.py (3 lines; Q39; the file was not read) and is the class the processor re-queues ([RQ-01]). Whether it reached the processor through `place()` on the production path is not established ([U-03]).
- **[EX-08] KNOWN DEFECT (B-005 sections 5 and 17 item 1; re-verified for main.py):** the timeout comments name the reconciler's 15-second recovery "the SOLE owner" of a timed-out entry (`signals/signal_processor.py:1464-1477`). B-005 found that recovery gated on an order placer that main.py does not pass; at the pin main constructs the reconciler with no `order_placer` argument (`main.py:3124-3139`). The reconciler's own gate was not re-read in C-008. Per B-005, timed-out entries were not resolved from broker evidence by that recovery; whether any such entry existed is not established.
- **[EX-09]** `_process_one_safe` catches anything that escapes `_process_one` (for example an exception raised inside a handler) and attempts `PLACEMENT_FAILED` with "unhandled: ..."; it releases no reservation (`signals/signal_processor.py:463-480`).

## 7. Re-queue and retries (FACT, SOURCE-VERIFIED unless marked)

- **[RQ-01]** The FIX-069 branch for a client-side `BrokerRateLimitError` from `place()` (`signals/signal_processor.py:1486-1532`): the failure is recorded; at a retry count of 3 or more the exception is re-raised and ends as `PLACEMENT_FAILED` (the claim released, the reservation release called, [OC-04]); otherwise the count goes up by one, `requeued` is set, a dict with `signal_id`, `scanner_name`, `symbol`, `trigger_price`, `triggered_at` and `retry_count` is put back with `put(timeout=1.0)`, the processor calls release as `requeued_transient_error`, and the path returns keeping the claim; a full queue clears `requeued` and re-raises (`PLACEMENT_FAILED`). C-006 section 5 (HISTORICAL) records the same path; re-verified.
- **[RQ-02]** The dict re-enters at ST-02: `PROCESSING` is written again and every stage runs again, screening included. The retry count is read only by the FIX-069 branch; elsewhere it is passed along (`signals/signal_processor.py:873, 884, 887, 1157, 1167, 1187, 1280, 1493-1516, 1621, 1638, 1660`).
- **[RQ-03] INTERPRETATION:** the first attempt passed the throttle, which recorded the placement before `place()` raised ([K-04]); the re-queue adds no delay (`signals/signal_processor.py:1519-1525`). The throttle's minimum gap counts from the last admitted entry of any symbol, the burst window from the last admitted entries of any symbol, and the cooldown from the last admitted entry of the same symbol (`signals/entry_throttle.py:77-107`). With the repository settings ([CF-06]) the inspected code path would therefore reject a retry that reaches ST-27 within 20 s of the last admitted entry (its own first attempt included), while 3 entries admitted in the last 60 s are on record, or within 300 s of its own first attempt, as `REJECTED_ENTRY_THROTTLED` (reservation released). **FACT (TEST TEXT):** a test docstring about the old timeout retry says a disabled throttle "would let the re-queued signal place a 2nd order" (`tests/unit/test_signal_processor.py:2347-2350`), so it reads the throttle as masking a retry; the rate-limit test checks that the item is re-queued as a dict, that the stored status is not `TIMEOUT` and that `place()` was called once (`tests/unit/test_signal_processor.py:2324-2344`). No test read runs a re-queued item through the throttle. Whether any retry happened is not established.
- **[RQ-04] INTERPRETATION:** the retry's age check uses the original `triggered_at` carried in the dict (`signals/signal_processor.py:1515, 914-921`), so a retry reaching ST-06 after 600 s would end `REJECTED_EXPIRED`.
- **[RQ-05]** The receiver's re-queue of a `QUEUE_FULL` row puts an item with the retry's price and time while the row keeps the first attempt's values (C-006 BM-05 and C-007 BM-03, HISTORICAL; BK-14, BK-21). The processor reads only the item ([ST-34]), so its age check, screening, prices and sizing use the retry's values. This is the item side of BK-14.
- **[RQ-06]** The FIX-007 pre-check (ST-01) is not wired (C-006 section 5.1; BK-15), re-verified (`signals/signal_processor.py:189, 233, 418`; `main.py:3575-3623`); its full-queue branch reads `signal_tuple[1]` for the log line and writes the bare `REJECTED` (`signals/signal_processor.py:427-437`; C-006 BM-06).
- **[RQ-07]** E2 and E3 have no re-queue: a `BrokerError` at their placement is recorded and re-raised (`signals/signal_processor.py:2283-2288, 2578-2581`) and ends as `PLACEMENT_FAILED`.

## 8. Same symbol and in-flight state (processor-level position, rules with their owners; FACT, SOURCE-VERIFIED unless marked)

- **[SS-01]** A second signal for a symbol already being processed meets, in the order E1 reaches them: (1) the receiver's claim, held through E1 until ST-32 (C-005 section 6); (2) the shadow-inning guard (ST-07); (3) the retest diverter's duplicate check (ST-15; not built at the pin); (4) the per-strategy position cap (ST-21; a strategy count, not a symbol rule); (5) one executed trade per symbol and direction per day (ST-21; on in the repository configuration, [CF-07]); (6) the risk engine's `CONTRARY_POSITION` and `DUPLICATE_SYMBOL` checks, fed by the store's `has_active_position(symbol)` and `get_active_position_direction(symbol)` (`capital/risk_engine.py:360-364, 766-794`); (7) the throttle's per-symbol cooldown (ST-27). HISTORICAL (A-008 section 14): the meaning of (5) and (6). The processor's comment at the shadow-inning guard also names a "symbol_lock" that "blocks re-entry while a real trade is OPEN" (`signals/signal_processor.py:923-926`); where that lock is held was not traced here (G topics).
- **[SS-02]** Checks (4) to (6) and the reservation run under the portfolio lock (`signals/signal_processor.py:1307-1344`); `place()`, whose docstring begins "Create trade, place entry orders" (`orders/order_placer.py:891`), runs after the lock is released (`signals/signal_processor.py:1446`). The throttle's per-symbol record (7) is set under the throttle's own lock when the candidate is admitted to placement, before `place()` is called (`signals/entry_throttle.py:13-17, 74-115`; `signals/signal_processor.py:1410`). **INTERPRETATION:** whether (5) and (6) see a candidate that holds a reservation but whose trade row the placer has not yet written depends on the store functions, not read here ([U-02]).
- **[SS-03]** Two mechanisms share the name "in-flight" (BK-07): (a) the receiver's per-symbol claim (C-005 section 6.1); and (b) the processor's `_in_flight_count`, one counter for all workers and all four entry points (E1 to E3, and E4 through `_admit_and_place`, [ST-33]; corrected in revision 3, section 14.7), incremented before the lock and decremented in `finally` (`signals/signal_processor.py:253-257, 1248-1258, 1302-1305, 1692-1698, 2133-2136, 2357-2364, 2469-2472, 2630-2636`), which the risk engine receives as `processor_in_flight_count` (`signals/signal_processor.py:1315-1318`) and uses in its `OPEN_POSITIONS` check outside the delivery branch (`capital/risk_engine.py:564-585`); how the engine combines it with its other counts is the engine's rule (F topics; `capital/risk_engine.py:587-618`). **INTERPRETATION:** (b) still counts a candidate after its `place()` has returned and until its `finally` runs, so a concurrent `approve()` could count it in (b) and, if the placer has written its trade row by then, again in the store's active count (`capital/risk_engine.py:585`); what that does to the cap is the engine's (F topics), and whether the row exists at that moment is not established (G topics).
- **[SS-04] KNOWN DEFECT (C-005 section 6.3, from B-007; re-verified for the processor):** main passes no heartbeat function (`main.py:3575-3623`), so the processor's checkpoints are no-ops (`signals/signal_processor.py:171-172, 219, 664-673, 899, 1023, 1129, 1346, 1382`). With C-005's 60-second sweeper, a claim held longer than that can be evicted while E1 is still running (C-005, INTERPRETATION). After an eviction the receiver's cache still refuses the same symbol and scanner within its window; a repeat from another scanner, or after the window, passes the claim and the cache (C-005 section 13, Q-45 (h), HISTORICAL) and would meet guards (2) to (7) of [SS-01] at the processor (BK-06; Q-45 (h), code-path part).
- **[SS-05]** The re-queue keeps the claim ([RQ-01]), so during a retry the receiver answers a same-symbol signal `IN_PROCESS` (C-005 section 6.1), unless the claim was evicted ([SS-04]).
- **[SS-06]** Q-20, the C-008 part: the processor makes no instrument lookup of its own. Its only use of the instrument cache is the sector for the V3 and allocator snapshots, "UNKNOWN" on any miss (`signals/signal_processor.py:1605-1617`; the gated grep for the cache, the blocklist and the exchange printed only `signals/signal_processor.py:176, 223, 1606`); the sizer's lot-size lookup keeps its default on a miss (`capital/position_sizer.py:307-313`). An unknown symbol therefore meets the screener's quote fetch first (ST-12; C-004: `SKIPPED_QUOTE_UNAVAILABLE`), and the processor returns.

## 9. The other entry points (FACT, SOURCE-VERIFIED unless marked)

### 9.1 E2: `continue_from_gate`

- **[EP-01]** Order of E2, with the main-path stage it mirrors:

| Step | Lines | Mirrors |
|---|---|---|
| `PROCESSING` | `signals/signal_processor.py:2033-2035` | ST-03 |
| kill; global window; shadow inning | `signals/signal_processor.py:2037-2048` | ST-04, ST-05, ST-07 |
| the strategy named in the WatchEntry; control; strategy window; governor | `signals/signal_processor.py:2050-2093` | ST-08 to ST-11 |
| entry, SL, target, direction and tier taken from the WatchEntry; side | `signals/signal_processor.py:2095-2104` | ST-13, ST-14, ST-23 |
| sizing; the in-flight count | `signals/signal_processor.py:2106-2136` | ST-17, ST-20 |
| the lock section: cap, symbol-direction rule, approval, reservation, `RESERVED` | `signals/signal_processor.py:2138-2171` | ST-21 |
| no placer; the alert, with no score | `signals/signal_processor.py:2173-2201` | ST-22, ST-24 |
| late kill; shutdown; throttle | `signals/signal_processor.py:2203-2233` | ST-25 to ST-27 |
| P1 and `place()` with `release_ltp`; `TIMEOUT`, or a re-raised `BrokerError`; `PROCESSED`; the S&R observer | `signals/signal_processor.py:2235-2307` | ST-28 to ST-31 |
| the reject and generic handlers; `finally` with no claim release | `signals/signal_processor.py:2309-2369` | ST-32 |

Not on this path: the age check, screening, the divert, the re-anchor, the V3 and allocator hooks, target derivation and the re-queue.

- **[EP-02] KNOWN DEFECT (B-009 section 15 item 1, D1; re-verified for the 281 non-test Python files):** no runtime file calls `EntryGate.add()`. Q37 found two lines, the gate's docstring example (`screening/entry_gate.py:103`) and the processor's comment that "nothing calls EntryGate.add()" (`signals/signal_processor.py:2236-2240`); Q38 found `WatchEntry(` only at that docstring and at the reload (`screening/entry_gate.py:103, 209`). The gate's start reloads gate_state rows (`screening/entry_gate.py:144-160`), and a release writes `GATE_RELEASED_<reason>` before it calls `on_release` (`screening/entry_gate.py:474-537`). So E2 runs only for a reloaded row released with PRICE_HIT (`main.py:879-892`). Whether such rows existed is not established (B-009 section 15; C-007 U-04; BK-24).
- **[EP-03]** When E2 runs, its `PROCESSING` replaces `GATE_RELEASED_PRICE_HIT` by an unconditional write ([OC-01]); its timeout branch keeps the reservation for the recovery as E1 does (`signals/signal_processor.py:2269-2282`).

### 9.2 E3: `continue_from_retest`

- **[EP-04]** Order of E3:

| Step | Lines | Mirrors |
|---|---|---|
| `PROCESSING` | `signals/signal_processor.py:2392-2394` | ST-03 |
| kill; global window; shadow inning | `signals/signal_processor.py:2396-2405` | ST-04, ST-05, ST-07 |
| the parked strategy; control; strategy window; governor | `signals/signal_processor.py:2407-2430` | ST-08 to ST-11 |
| a structure SL from the zone band and the configured buffer; an entry estimate from a live quote or the break level | `signals/signal_processor.py:2432-2443, 2642-2657` | - |
| `RETEST_BAD_STRUCTURE` when the estimate is on the wrong side of the SL | `signals/signal_processor.py:2446-2452` | - |
| sizing; the target; the in-flight count | `signals/signal_processor.py:2454-2472` | ST-17, ST-23, ST-20 |
| the lock section | `signals/signal_processor.py:2474-2501` | ST-21 |
| no placer; the alert; late kill, shutdown and throttle | `signals/signal_processor.py:2503-2536` | ST-22, ST-24 to ST-27 |
| P1 and `place()` with `entry_order_type="MARKET"`; `TIMEOUT`, or a re-raised `BrokerError`; `PROCESSED`; the S&R observer | `signals/signal_processor.py:2538-2592` | ST-28 to ST-31 |
| the handlers; `finally` with no claim release | `signals/signal_processor.py:2594-2640` | ST-32 |

There is no age check: "NOT the :607 60s expiry - this is a park-and-resume" (`signals/signal_processor.py:2376-2384`; [D-04]).

- **[EP-05]** The diverter and the monitor are built only when `wait_for_retest_enabled` is true and the zone infrastructure exists (`main.py:3769-3797`); the repository value is false ([CF-11]); HISTORICAL (B-006 TH11): with the repository configuration the zone warmer, the retest monitor and the structure-exit manager are not started. On the pinned wiring with the repository configuration, E3 has no caller.
- **[EP-06]** On E3 the target is derived before the lock (`signals/signal_processor.py:2467, 2474`), unlike E1 ([K-02]).

### 9.3 E4: the allocator's enforce worker

- **[EP-07]** The processor hands a candidate to the allocator only in "enforce" with an in-scope strategy (`signals/signal_processor.py:1147-1160`). In "shadow" ([CF-09]) it only calls `observe()`, which appends a copy to the same buffer that `submit()` feeds (`allocation/portfolio_allocator.py:88-102`), and the path continues (ST-19). The allocator's worker, which drains that buffer, and its calls of E4's two callbacks (passed at `main.py:3644-3645`) were not read; that it never admits or rejects in "shadow" rests on comments ("NEVER reserves/places", `config/system_config.yaml:565`; `main.py:3626-3628`), not on code read here (J-012). **INTERPRETATION:** with the repository configuration even "enforce" would hand off nothing: main.py's scope function reads `v3_playbook` (`main.py:3646-3648`), and the one strategy file with `v3_playbook: true`, pb01_breakout_retest, has `enabled: false` (Q44, Q46), so its signals would end at ST-09 or ST-08 before the hook (section 4.1). Scope rules belong to J-012.
- **[EP-08]** `admit_prepared` reuses `_admit_and_place` with its own handlers and `finally` (`signals/signal_processor.py:1646-1704`); `reject_prepared` writes `REJECTED_<reason>`, counts it and releases the claim, its status write guarded, unlike E1's (`signals/signal_processor.py:1706-1737`). With the repository configuration the processor makes no hand-off that would lead to either ([EP-07]).

### 9.4 The EOD route

- **[EP-09]** No direct hand-off from the EOD route to the processor was found: no file matched the Stage 0 search for EOD-route names (Q28: names beginning eod_route, eod_signal or daily_signal, over the 869 code files, tests included; its positive control Q53 finds names beginning eod_ in 115 of them), and the receiver's EOD branch submits to the watchlist capture worker (C-002 section 11 and C-001 section 7.5, HISTORICAL). C-011 owns the route.

## 10. Observers and shadow hooks (FACT, SOURCE-VERIFIED unless marked)

- **[OB-01]** The V3 chain ([ST-18]): its snapshot computes the target with the same helper inside its own guard (`signals/signal_processor.py:1586-1590`); `observe()` enqueues with `put_nowait` and drops the copy when the queue is full (`v3_chain/runner.py:99-118`). INTERPRETATION: the chain cannot reject a signal on the processor path; the time taken to build the snapshot is spent in the worker.
- **[OB-02]** The allocator in "shadow" ([ST-19]): a candidate copy built in the worker (`signals/signal_processor.py:1619-1644`) is appended to a buffer in a guard (`allocation/portfolio_allocator.py:88-96`).
- **[OB-03]** The S&R V1 observer: called only after `PROCESSED`, guarded, a no-op when the detector is not set (`signals/signal_processor.py:614-658, 1548-1561, 2295-2307, 2588-2592`); main builds it in a guarded block (`main.py:3490, 3494, 3508`, lines printed by a grep; the condition lines were not read); the repository flag is true ([CF-12]).
- **[OB-04]** The zone warmer: a guarded hand-off from the dispatcher when a warmer exists (`signals/signal_processor.py:361-375, 388`); none on the repository configuration ([W-05]).
- **[OB-05]** Evidence ([OC-07]) and alerts ([ST-24]; the `EXPIRED` warning, `signals/signal_processor.py:1221-1231`) are observers too; L topics own them.

## 11. Kill, stop and shutdown at the processor (FACT, SOURCE-VERIFIED unless marked)

- **[SD-01]** Three kill checks meet a signal on E1 before `place()`: ST-04; the risk engine's first check, fed by `is_active()` (default "entry"), the kill switch being passed to it (`capital/risk_engine.py:366-369, 515-519`; `main.py:2913`); and ST-25. The first two store the same name ([K-08]). A fourth, the placer's last-mile kill check inside `place()`, is HISTORICAL (B-009 section 14, the OrderPlacer row; main passes the kill switch to the placer, `main.py:3061, 3069`); the placer's check was not read here (G topics).
- **[SD-02]** Of the API failures the processor records ([EX-02], [EX-05], [RQ-01]), only `BrokerTimeoutError` and the client-side `BrokerRateLimitError` count toward the kill switch's automatic SOFT_KILL; a `BrokerAuthError` is logged and not counted, and any other class is ignored ([EX-02]; `capital/kill_switch.py:809-850`). The counter's threshold and the trip (`capital/kill_switch.py:860-880`) belong to I topics.
- **[SD-03]** Shutdown: ST-26 on all three paths; the processor's stop ([W-09]; C-006 section 8.1). Drained items run the pipeline: one that reaches ST-26 is rejected `REJECTED_SHUTDOWN`, and one rejected at an earlier stage gets that stage's status (`signals/signal_processor.py:392-399, 1398-1405`; C-006 sections 8.1 and 11 item 1, HISTORICAL, re-verified). That is one half of C-006 BM-03 (BK-12). The other half, an item put after the dispatcher's final drain, includes a FIX-069 re-queue during stop: the put does not check the stop event (`signals/signal_processor.py:1519-1525`), and the drain loop ends at the first empty read (`signals/signal_processor.py:392-399`). **INTERPRETATION:** such an item would not be processed; its row would keep `RESERVED`, the last status the processor wrote on that path (`signals/signal_processor.py:1344`), and the processor would not release its claim. BM-03 stays open for A-011 and B-011.
- **[SD-04]** Releases and confirmations after the processor's stop: [W-10].

## 12. Text drift and code-path findings

### 12.1 Text drift (FACT (TEXT DRIFT))

- **[D-01]** The stop docstring "up to 5s per SP4" against a 10-second join and an unbounded wait (`signals/signal_processor.py:326-340`): C-006 (BK-17), re-verified.
- **[D-02]** "Max 3 retries (45s total, 15s each)" (`signals/signal_processor.py:1489`) against a re-queue with no wait (`signals/signal_processor.py:1519-1525`): C-006 (BK-17), re-verified. The placer's 429 loop has its own backoff, its comment saying the next acquire "blocks on the frozen bucket" (`orders/order_placer.py:1431-1432`); whether that accounts for the 15 s is not established.
- **[D-03]** The re-anchor comment "Pullback strategies skip this (EntryGate already waits for current price)" (`signals/signal_processor.py:1074-1075`) against no runtime caller of `add()` ([EP-02]): B-009 D1, re-verified.
- **[D-04]** `continue_from_retest`'s docstring "NOT the :607 60s expiry" (`signals/signal_processor.py:2378`): the age check is at `signals/signal_processor.py:914-921` and uses 600 s ([CF-01]); 60 is the constructor's default (`signals/signal_processor.py:175`).
- **[D-05]** The timeout comments call the reconciler's recovery "the SOLE owner" of a timed-out entry on E1 (`signals/signal_processor.py:1468-1475`) and say "The 15s reconciler recovery owns it" on E2 and E3 (`signals/signal_processor.py:2270-2273, 2566-2569`), a recovery B-005 found not wired ([EX-08]).
- **[D-06]** `pipeline_timeout_sec`'s comments, "hard per-signal processing deadline" (`config/system_config.yaml:347`) and "hard per-signal timeout" (`core/config_loader.py:568`), against no runtime reader ([CF-03]): no effect identified in the inspected path.
- **[D-07]** The configuration model's comment '"HALT" = reject the signal with REJECTED_NO_ATR_DATA' (`core/config_loader.py:571`) against the name the processor would store ([K-01]).
- **[D-08]** The alert's fixed text: the title "INTRADAY SIGNAL" for every intent and the constant lines "Priority rank: #1 of 1 candidates | SIP: NO" and "Smart TGT: enabled | Timeout: 10 min" (`signals/signal_processor.py:533-549`), none read from configuration. Whether they matched the system's behaviour for a given signal belongs to L topics (HISTORICAL, B-009 section 5: the order monitor's unfilled-entry timeout is 60 s).
- **[D-09]** The EntryGate's docstring wires `on_release=signal_processor.on_gate_release` (`screening/entry_gate.py:98`); the processor defines no such method (the gated grep of its definitions lists none), and main wires `_make_gate_release_cb` (`main.py:3843`).
- **[D-10]** The docstrings "order_placer may be None (pipeline degrades to PROCESSED_NO_PLACER)" and the usage example "order_placer=None, # wired in Module 33" (`signals/signal_processor.py:11, 146, 169`) describe a construction main.py does not use (`main.py:3588`).
- **[D-11]** `continue_from_gate`'s docstring says steps 1 to 4 are bypassed and "Kill-switch and market-window are re-checked as a last-mile gate" (`signals/signal_processor.py:2015-2019`); the code also re-checks the shadow inning, looks the strategy up again, and re-checks the control gate, the strategy window and the governor (`signals/signal_processor.py:2046-2093`).

### 12.2 Code-path findings (FACT, SOURCE-VERIFIED unless marked)

- **[K-01] Doubled reject names.** FACT: the handler writes `REJECTED_` plus the check (`signals/signal_processor.py:1201`), and two checks already carry the prefix: `REJECTED_NO_ATR_DATA` (`signals/signal_processor.py:1802-1807, 1913-1918`) and, through ST-17, the sizer's constraint `REJECTED_LOT_SKEW` (`signals/signal_processor.py:1128`; `capital/position_sizer.py:692-705`). INTERPRETATION: the stored names would be `REJECTED_REJECTED_NO_ATR_DATA` and `REJECTED_SIZING_REJECTED_LOT_SKEW`. The first is latent twice over with the repository configuration: no `atr_fallback_mode` is set, so WARN ([CF-04]), and no strategy file sets an ATR method (Q45; positive control Q50). The second is reachable whenever the sizer's lot-skew rule fires (F-004; its threshold was not read); whether it fired is not established. No runtime file or test names either doubled form (Q48, Q49; their positive controls, Q51 and Q52, find the single names in 4 test files and 3 runtime files). Extends C-007 BM-04 (BK-22).
- **[K-02] Target after reservation.** FACT: on E1 the target is derived after `RESERVED` (`signals/signal_processor.py:1340-1344, 1365-1368`); a target reject is stored after `RESERVED` and the reject handler releases the reservation (`signals/signal_processor.py:1212-1217`). INTERPRETATION: on E1 a target failure first passes the risk checks and briefly holds capital; on E3 the order is the other way round ([EP-06]).
- **[K-03] The alert comes before the last checks.** HISTORICAL (B-009 section 9), re-verified: "INTRADAY SIGNAL" is sent before the late kill check, the shutdown check, the throttle and `place()` (`signals/signal_processor.py:1370-1418, 1446`), so the message can precede `REJECTED_KILL_SWITCH_LATE`, `REJECTED_SHUTDOWN`, `REJECTED_ENTRY_THROTTLED`, `TIMEOUT` or `PLACEMENT_FAILED`; it is not proof of an order.
- **[K-04] The throttle records before the call.** FACT: with at least one of its gates enabled, as in the repository configuration ([CF-06]), `admit()` records the placement under its lock whenever it allows one (`signals/entry_throttle.py:13-17, 109-115`); with none enabled it only counts the admission (`signals/entry_throttle.py:69-72`). The processor calls it before `place()` (`signals/signal_processor.py:1410, 1446`). INTERPRETATION: an entry that then fails, times out or is re-queued still holds the 20-second gap, a burst slot (3 in 60 s) and its symbol's 300-second cooldown ([CF-06]), so the inspected code path would throttle the entries after it ([RQ-03]). The throttle's state is in memory (`signals/entry_throttle.py:55-59`), so a restart clears it.
- **[K-05] Handler order on a store failure.** FACT: the reject handler writes the status before it releases (`signals/signal_processor.py:1201, 1212-1217`), the generic handler likewise (`signals/signal_processor.py:1238-1243`), and the timeout branch writes before dropping the handle (`signals/signal_processor.py:1480-1483`); none of these writes is guarded. INTERPRETATION: if such a write raised, the reject or generic handler would not release the reservation (the exception would reach [EX-09], which releases none). If the timeout branch's write raised, the handle would still be set (`signals/signal_processor.py:1483` not reached, synced back at `signals/signal_processor.py:1189`), and the generic handler, provided its own `PLACEMENT_FAILED` write succeeded (`signals/signal_processor.py:1238`), would release the reservation that the placer kept (`signals/signal_processor.py:1239-1243`); under a persistent store failure that write would raise too, and nothing would be released. One more case: if the `PROCESSED` write raised after a successful `place()` (`signals/signal_processor.py:1543`), the generic handler would store `PLACEMENT_FAILED` for a placed order if its own write succeeded; it would release nothing, the handle having passed to the placer, and `entries_placed` would already have been bumped (`signals/signal_processor.py:1461-1463`). Each case needs a store failure; whether one happened is not established. Related: C-007 K-01 (BK-19).
- **[K-06] No processing deadline.** FACT: [CF-03]. INTERPRETATION: nothing in the processor bounds the time a worker spends on one signal: the synchronous calls of section 4.1 (the screener's quote fetch, the governor's database reads, the sizer, the risk checks, the store writes, the alert, `place()`) run in the worker with no processor-level limit. The callees' own timeouts were not traced here: HISTORICAL (B-004 section 7.4), broker calls ran with the client's one timeout, inferred as 10 s; the screener runs its steps on an executor of its own (`signals/signal_processor.py:342-344`), whose limits were not read (D topics). C-006 OQ-4 asked how the setting is applied: at the pin no runtime file reads it (Q36).
- **[K-07] A re-queued item runs everything again** ([RQ-02]), a second screening with its own quote and status write included.
- **[K-08] One name, several sources.** FACT: `REJECTED_KILL_SWITCH` from ST-04 ("Kill switch is active", `signals/signal_processor.py:907`) and from the risk check ("Kill switch is active; no new trades permitted", `capital/risk_engine.py:519`); `REJECTED_OUTSIDE_ENTRY_WINDOW` from ST-05 and ST-10 (`signals/signal_processor.py:912, 978-982`); `REJECTED_UNKNOWN_STRATEGY` from three cases (`signals/signal_processor.py:935-953`). The reason column tells them apart.
- **[K-09] Heartbeats are no-ops:** [SS-04].
- **[K-10] A disabled tracker is still consulted.** FACT: the tracker object is passed ([CF-14]) and consulted on E1 to E3 through the shared guard ([ST-33]; `signals/signal_processor.py:1988-1997`). Its answer while disabled is not established ([U-01]). HISTORICAL / RECORD-DERIVED: a processor docstring says that "A reject raised BEFORE Step 2", "measured on production", made "4,168 of 48,934 P2 rejects (all SHADOW_INNING_ACTIVE)" (`signals/signal_processor.py:564-566`), with no date; B-009 section 13 lists `REJECTED_SHADOW_INNING_ACTIVE` 2,801 among the leading terminal statuses of its census, whose signals run from 12-Jun to 16-Jul-2026, before the 25-Jul-2026 disablement. Neither figure shows the tracker's answer after the disablement.
- **[K-11] NaN or infinite prices (C-004 U-07, the C-008 part).** FACT: the processor checks nothing about `trigger_price` before the screener (ST-12); its first price guard is `entry_price <= 0` (`signals/signal_processor.py:1786`). FACT (EXTERNAL; Python 3.13.16 in Claude's workspace, not the legacy interpreter): `nan <= 0` and `inf <= 0` are both False, and an entry and SL derived from `inf` give a NaN distance. INTERPRETATION: a NaN or +inf price that passed the screener would not be stopped by the processor's own guard. Two qualifications: the price helper's bounds step, which runs between that guard and sizing (`signals/signal_processor.py:1848-1850`; lines 1851 to 1894 not read), may treat it; and for a strategy without a pullback wait a live LTP above 0 replaces the trigger as the basis of entry and SL (ST-16; `signals/signal_processor.py:1082-1101`), while the trigger itself still reaches `place()` for the placer's slippage guard (`signals/signal_processor.py:1457`). What the screener, the bounds step, the sizer and the placer did with such a value was not traced (D, E, F and G topics); C-004's U-07 stays open for its owners.

## 13. Tests as text (FACT (TEST TEXT); never run)

| ID | Lines | What the test text states | Supports |
|---|---|---|---|
| T-01 | `tests/unit/test_signal_processor.py:493-509` | the full pipeline with a mock placer ends `PROCESSED`, the placer called once | ST-30 |
| T-02 | `tests/unit/test_signal_processor.py:512-526` | with no placer: `PROCESSED_NO_PLACER` and the reservation released | ST-22 |
| T-03 | `tests/unit/test_signal_processor.py:832-934` | the release function is called on a `SKIPPED_` result, a screener reject, a kill reject, success and a placer exception | ST-32, OC-05 |
| T-04 | `tests/unit/test_signal_processor.py:1151-1192` | with HALT the price and target helpers raise with check "REJECTED_NO_ATR_DATA"; with WARN the price helper falls back (the only WARN case in the range read) | K-01 (the tests check the check name, not the stored status) |
| T-05 | `tests/unit/test_signal_processor.py:1736-1764` | `PASSED` is in the row when the sizer runs; the final status is `PROCESSED` | ST-12, ST-30 |
| T-06 | `tests/unit/test_signal_processor.py:1809-1832` | a stop during processing finishes within 8 s | W-09 |
| T-07 | `tests/unit/test_signal_processor.py:1856-1916` | `continue_from_gate` called directly on a WatchEntry the test builds | EP-01 (test set-up, not wiring) |
| T-08 | `tests/unit/test_signal_processor.py:2255-2366` | a timeout gives `TIMEOUT`, no release, no re-queue, one attempt and one recorded failure; a client-side rate limit re-queues a dict; with the throttle replaced by an always-admit stub a timeout still gives one attempt | EX-05, RQ-01, RQ-03 |

The test names (the gated grep of the file's definitions) also show cases for the kill, window, expiry, lookup, sizer, risk, reservation, placer and unexpected-exception paths, the portfolio lock, FIX-018, the shadow-inning guard on all three paths and FIX-007 (`tests/unit/test_signal_processor.py:544, 558, 572, 585, 599, 614, 632, 651, 668, 687, 1359, 1427, 1557, 1997, 2080, 2106, 2144`); their bodies were not read. In the six ranges read, the tests build the processor with mocks through the file's own helper (for example `tests/unit/test_signal_processor.py:501, 520, 1755, 1874, 2311`), and the processor's own comment says some unit tests build a partly constructed processor with `SignalProcessor.__new__` (`signals/signal_processor.py:1984-1987`): the tests show the logic, not main.py's wiring (V2_DESIGN_LOG lesson of 2026-10-16). No test read runs a re-queued item through the throttle (no search was made for one), and no test names either doubled name (Q48, over the 486 test files; positive control Q51).

## 14. Reconciliation and bookmarks

### 14.1 Against C-001 to C-007

| ID | Frozen statement | Result at the pin |
|---|---|---|
| RC-01 | C-001 section 7.4: Step 2's lookup and the control gate; the throttle keyed by symbol across all scanners | Confirmed (ST-08, ST-09); the throttle's gap and burst gates are global and only the cooldown is per symbol (`signals/entry_throttle.py:4-11, 77-107`) |
| RC-02 | C-002 section 11: the claim is left for the processor to release; the tuple's shape | Confirmed (ST-02, ST-32, OC-05) |
| RC-03 | C-003 | No statement about the processor (the Stage 0 cross-check found none) |
| RC-04 | C-004 sections 6.5 and 16.1: the processor unpacks the tuple at lines 867 to 887; U-07 and L-01; Q-20's C-008 part | Confirmed (ST-02); U-07: K-11, still open for E-001 to E-004; Q-20: SS-06 |
| RC-05 | C-005 sections 6.1 to 6.3 and 13: release in `finally` unless re-queued or handed off; the re-queue keeps the claim; heartbeat no-ops | Confirmed (OC-05, RQ-01, SS-04) |
| RC-06 | C-006 sections 2.2, 5, 6, 7, 8.1 and 8.2; section 3 and OQ-4 (`pipeline_timeout_sec` not established); OQ-2 (the 15 s) | Confirmed (W-01 to W-09, RQ-01, RQ-06, OC-01, SD-03); OQ-4: no runtime reader (CF-03, K-06); OQ-2 still not established (D-02). C-006 recorded both as not established, so neither needs an erratum |
| RC-07 | C-007 S-04 to S-12, section 5.2, U-06, BM-01, BM-03, BM-04, BM-06 | Confirmed; BM-04 extended by K-01; U-06 answered for the processor and the callees read (OC-02, OC-03; the allocator's reasons not read) |

### 14.2 Other frozen manuals consulted

| ID | Frozen statement | Result at the pin |
|---|---|---|
| RC-08 | B-009 sections 5, 9, 14 and 15 item 1 (D1) | Confirmed; added detail: K-02, K-04, K-05, RQ-03, EP-01 |
| RC-09 | B-005 sections 5 and 17 item 1 | The main.py half re-verified (EX-08) |
| RC-10 | B-004 sections 7.1, 7.3 and 7.4 | Consistent (EX-05, EX-06, OC-04, K-06; Q42) |
| RC-11 | A-005 section 7 (RECORD-DERIVED order of admission) | Consistent at the source; it does not list the five steps between `RESERVED` and placement (ST-23 to ST-27) |
| RC-12 | A-008 section 14 (same-symbol rules) | Consistent (SS-01) |
| RC-13 | B-006 TH11 (the retest monitor not started) | Consistent (EP-05) |
| RC-14 | A-011 section 12 Q31, as qualified by C-006 BM-03 (BK-12) | Consistent with C-006's qualification (SD-03) |
| RC-15 | B-009 section 10, the row "Broker error in sizing, risk or reserve" (lines 1122-1125, 1319-1322, 1332-1335): "REJECTED_*_BROKER_ERROR; counted toward SOFT_KILL" | Qualified at the pin: the processor passes the error to `record_api_failure` (EX-02), which counts only `BrokerTimeoutError` and `BrokerRateLimitError` toward the automatic SOFT_KILL and logs any other class as "NOT counted" (`capital/kill_switch.py:809-814, 842-850`), as B-004 section 7.3 already records (FIX-191, 23-Jun). B-009 section 9's trigger table says only that the processor "records failures" at these and other lines, without the count rule. **ACCEPTED as erratum B-009 E-3:** recommended in ChatGPT's Stage 1 review and approved by Rama on 23-Oct-2026; applied at the C-008 freeze with the texts of section 14.6 (DR-061) |

One erratum was found, RC-15 (B-009 section 10), by the independent check (section 19.2); ChatGPT's Stage 1 review recommended it and Rama approved it on 23-Oct-2026 as erratum B-009 E-3 (section 14.6). Every other finding is new detail or a qualification of a code path, and no other frozen statement was found contradicted. Searched: the frozen manuals' lines that cite the processor (Stage 0 cross-check, 17 documents) and wording searches of docs/agenda in V2 main dce3164 for pipeline_timeout, deadline, INTRADAY SIGNAL, REJECTED_SIZING and LOT_SKEW. This is a bounded search, not a proof that no frozen line conflicts.

### 14.3 Master agenda and the legacy manual index

The master agenda names C-008 "Signal processing" with no further text (MASTER_REDESIGN_AGENDA_AND_WORKFLOW_21-SEP-2026.txt, line 279).

LEGACY_SYSTEM_MANUAL_INDEX.md (authorisation G), reconciled in revision 2: the file Rama supplied at 00:29 IST on 24-Oct-2026 (124 lines; SHA-256 in the header; section 1.2). It is read as a document, not as legacy source; "line N" below is a line of that file. It is not modified, and nothing here is inferred about chapters it lists but does not contain. It names the production host by its address (line 14) and a local PC path (line 12); neither is reproduced here.

| ID | The index | C-008 | Result |
|---|---|---|---|
| IX-01 | What it is: the index of the "LEGACY TRADING SYSTEM - END-TO-END MANUAL", opened 18-Sep-2026, with 25 chapters, chapter 24 being the cross-reference index (lines 1, 4). The Claude Project's WHERE_EVERYTHING_IS_STORED_18-Sep-2026.txt places it in docs/legacy_system_manual/ of the records repository, "the 25-chapter booklet" | a topic of the V2 project's series (master agenda, section C), written to the conventions of the frozen C-001 to C-007 | Two separate manual series; C-008 is not a chapter of the 18-Sep manual, which has no chapter named for signal processing. The Claude Project's instructions call ChatGPT's Phase 1 work "the 24-chapter legacy manual"; this index has 25 chapters, chapter 25 added under its item U3 (line 108). Whether the two are the same work is not established here; noted for the reviewers, not C-008's to resolve |
| IX-02 | Evidence basis (lines 10 to 14): primary, a PC worktree at commit e7bf477 with an uncommitted change set, "byte-equivalent to the TESTING VM" of 17-Sep, its line numbers holding "for the files as read, not for e7bf477 alone"; record-derived, "git at 970aabf for production-side code"; not read, production and its bare repository | the production snapshot repository at 970aabf only, read through the gated filter (section 1) | Different trees: the 18-Sep chapters' line numbers are not comparable with C-008's citations. Consistent: the index names 970aabf as the production-side code, and neither the index nor C-008 inspected a production host. The status DEAD / REFERENCE ONLY (line 3) matches C-008's header |
| IX-03 | The chapters for C-008's subject (lines 42 to 61): 05 "Signals and strategies", 06 "Filtering and trade decision", 08 "Entry price and R:R", 09 "Risk and capital allocation", 10 "Order processing and management"; for the parts C-008 touches beside the pipeline (the S&R observer, alerts, evidence, configuration, tests and failure handling), 07, 14, 15, 17, 19 and 20; and 21 (walkthrough), 22 (known issues) and 24 (cross-reference) | sections 4 to 13 | Every one is "NOT STARTED" (lines 42 to 61). There is no chapter text on C-008's subject to reconcile against, so no statement-level conflict exists there |
| IX-04 | Chapters 01 to 03 "WRITTEN" (lines 38 to 40), 03 being "Startup, runtime and scheduler" | W-08 and W-09 (the start and stop order around the processor); for chapter 02, the processor's construction, wiring, entry points and role (section 14.7) | Reconciled at statement level in revision 3, as ChatGPT's review of revision 2 required: section 14.7. Rama supplied chapters 02 and 03 on 24-Oct-2026, byte-identical to the records repository's copies (section 1.2); they describe the testing worktree, not the production snapshot. Within the bounded overlap: three conflicts between the documents (CX-01 to CX-03), each recorded with Rama's decision. Chapter 01 was not required and was not compared. [OQ-03] is answered (in revision 2: not reconciled, the texts not supplied) |
| IX-05 | Scope counts measured on the worktree (lines 21 to 29): 757 Python modules, 344 of them not tests, a runtime set of 200 import-reachable modules, 451 test files | the scopes at the pin (section 16; the Stage 0 inventory): s_rt, 281 runtime Python files; s_tests, 486 test files; s_code, 869 code and configuration files | Not comparable: different trees and different definitions. No C-008 statement depends on the index's counts |
| IX-06 | The evidence format every chapter follows, rules 1 to 8 (lines 68 to 75) | section 1 (labels, quotations), section 4.3 (the trace), sections 15 and 16 (what was not established), and the owners named throughout | Consistent in substance: the trace has the same seven steps, C-008 keeping the authorisation's names (LOGIC for "PROCESS", GUARD for "VALIDATION", and STORAGE / HAND-OFF and NEXT CONSUMER); the index's five labels (line 70) are a subset of section 1's; every source statement cites file:line (rule 4); sections 15 and 16 are the closing list of rule 7; cross-references go to master-agenda topics, not to the 18-Sep chapters (rule 8). Rules 5 and 6 differ: IX-07, IX-08 |
| IX-07 | Rule 5, "Paper vs live wherever behaviour differs" (line 72) | not applied | C-008 describes the production snapshot's wiring and repository configuration; the authorisation asks for no paper/live comparison, and V2 is live-only (DR-001). The processor code read carries paper-parity comments, for example "Parity: paper simulates the timeout through this same handler" (`signals/signal_processor.py:1476-1477`). Whether paper and live differed on the processor's path is NOT ESTABLISHED here |
| IX-08 | Rule 6, "Every zero carries a positive control run with the same flags and locale" (line 73) | the zero results Q28, Q41, Q45, Q48 and Q49 | Applied in revision 2. Each zero now has a positive control, run with the same tool, flags and file list: Q42 for Q41 (already in revision 0); Q50 for Q45 (both method keys found in all 16 strategy files); Q51 for Q48 (the single names in 4 test files); Q52 for Q49 (the single names in 3 runtime files); Q53 for Q28 (names beginning eod_ in 115 of the 869 code files). Counts only; no source line printed (the inventory, section 7). The zeros stand |
| IX-09 | Defects: chapter 22 is "the only home for every defect" (lines 59, 77) | KNOWN DEFECT entries, the K- findings and the BK and C8/BM bookmarks (sections 6, 8, 9, 12 and 14) | A convention of the 18-Sep manual, not of the V2 topic series: C-008 records defects as the frozen C-001 to C-007 do. Chapter 22 is NOT STARTED, and nothing from C-008 is registered there |
| IX-10 | Line 117: "No secret values in any chapter, at any time. These files go to GitHub." | the header's security line | The index itself carries the production host's address (line 14). Not a secret value, but this project does not reproduce host addresses; noted for Rama. C-008 reproduces neither that address nor the PC path |
| IX-11 | The open items U1 to U5 (lines 104 to 110) | - | None concerns the signal processor |

Result: no conflict between C-008 and the index, and no erratum. [OQ-02] is answered at the level of the index; [OQ-03], which remained, is answered in section 14.7.

### 14.4 Notes for the open bookmarks naming C-008 (W-9; no bookmark is closed here)

| BK | Note |
|---|---|
| BK-02 (VP-12) | the legacy same-symbol rules sat in the processor and the risk engine (SS-01); VP-12 is not ratified at C-008 |
| BK-03 | the claim's lifetime: SS-04, SS-05; release by symbol: OC-05 |
| BK-06 | SS-01, SS-04 |
| BK-07 | SS-03 |
| BK-12, BK-18 | SD-03; VP-59 is not ratified at C-008 |
| BK-14, BK-21 | RQ-05 (the item side) |
| BK-15 | RQ-06 |
| BK-17 | D-01 and D-02 re-verified; D-04 adds the retest docstring's 60 s |
| BK-19 | K-05 adds the handlers' order |
| BK-22 | K-01, with `REJECTED_SIZING_REJECTED_LOT_SKEW` |
| BK-24 | EP-02, EP-03 |

VP-11, VP-24, VP-31 and VP-59 are not ratified at C-008 (authorisation E). Facts that bear on them: VP-24, ST-05 and ST-10; VP-31, the risk engine's `DAILY_TRADES` check (OC-03; F-010); VP-11 and VP-59, OC-01 and SD-03. Q-20: SS-06. Q-45 (h): SS-04 for the code path; its records questions stay open.

### 14.5 New bookmarks

| Bookmark | Point | Cross-check with |
|---|---|---|
| C8/BM-01 | Doubled reject names (K-01) | K-003; F-004; C-007 (BK-22) |
| C8/BM-02 | The throttle records a placement before the call, so failed and re-queued entries hold its slots (K-04, RQ-03) | OQ-01 (owner); G-008, G-009 |
| C8/BM-03 | The target is derived after the reservation on E1 (K-02) | E-007, E-008; F-011 |
| C8/BM-04 | Handlers write the status before releasing, unguarded (K-05) | K-003, K-007, K-011; F-011, F-013 (BK-19) |
| C8/BM-05 | `TIMEOUT` relies on a recovery that is not wired (EX-08, D-05) | G-008, G-015; B-005 |
| C8/BM-06 | No per-signal processing deadline (K-06) | B-002; G-003 |
| C8/BM-07 | A disabled shadow tracker is still consulted; its answer unknown (K-10, U-01) | OQ-01 (owner) |
| C8/BM-08 | One stored name from several sources (K-08) | L-009, L-011; I-001, I-002 |
| C8/BM-09 | The lock boundary and the in-flight counts (SS-02, SS-03) | F-006 to F-011; A-008 section 14 (BK-02, BK-06, BK-07) |
| C8/BM-10 | Gate and retest resumptions are not awaited by the processor's stop (W-10) | B-011; D-015, D-016 |
| C8/BM-11 | NaN or +inf prices pass the processor's own guard (K-11) | E-001 to E-004 (C-004 U-07) |
| C8/BM-12 | The alert's fixed text (D-08) | L-001, L-004 |

### 14.6 Erratum B-009 E-3 (RC-15; ACCEPTED 23-Oct-2026, applied at the C-008 freeze, DR-061)

Found by the independent check of revision 0 (IC-13; RC-15), recommended in ChatGPT's Stage 1 review ("APPROVE A NARROW, TRACEABLE ERRATUM to B-009 section 10"), approved by Rama on 23-Oct-2026. Revision 1 changes no frozen file: the texts below are applied to B-009_manual.md only at the C-008 freeze and recorded in that freeze's DR row; B-009's freeze and its history are not altered. The number E-3 follows B-009's E-1 (DR-047) and E-2 (DR-055); the number and the anchors are to be rechecked against the live register and main at the freeze, as the earlier freezes did. The texts are against V2 main dce3164, where B-009_manual.md was last changed in commit d89164d. Format as in C-004 section 24: struck text, replacement, and the unchanged text on each side. The erratum section adapts the wording ChatGPT's review suggested. It records legacy behaviour; it is not a V2 decision. At the freeze (24-Oct-2026, 09:30 IST) the number and the anchor were rechecked against the live register and main, still dce3164: the register's last row was DR-060, so this freeze's row is DR-061; B-009_manual.md carries errata E-1 and E-2 only, so E-3 is the next number; its line 235 is the row quoted in item 1; and the search of item 4 found the same lines. The texts below were applied as they stand, with "DR-0NN" replaced by DR-061 and nothing else changed. By Rama's decision of 24-Oct-2026 (about 09:27 IST), one status sentence was also added to B-009_review_notes.md, as at E-1 and E-2; it is not among the texts below (C-008_review_notes.md section 5).

1. The passage: B-009 section 10 ("Failures during the session"), the table row "Broker error in sizing, risk or reserve", its third cell (line 235 at dce3164).

```text
(Each text below is one line in B-009_manual.md; it is wrapped here for reading.)
Struck:          counted toward SOFT_KILL
Replacement:     recorded with the kill switch, which counts only BrokerTimeoutError and BrokerRateLimitError
                 toward its automatic SOFT_KILL; other classes are not counted
Applied as:      ~~counted toward SOFT_KILL~~ (superseded by erratum E-3) recorded with the kill switch, which counts
                 only BrokerTimeoutError and BrokerRateLimitError toward its automatic SOFT_KILL; other classes are
                 not counted
Unchanged before it (the whole row up to the struck text):
                 | Broker error in sizing, risk or reserve | 1122-1125, 1319-1322, 1332-1335 | REJECTED_*_BROKER_ERROR;
                 (one space follows the semicolon)
Unchanged after it: " |" (the end of the row)
```

2. The erratum section, added at the end of B-009_manual.md after "## Erratum (2026-10-21, found during C-002)":

```text
(In B-009_manual.md each bullet below is one line; it is wrapped here for reading.)
## Erratum (2026-10-23, found during C-008)
- E-3 (section 10, the row "Broker error in sizing, risk or reserve"). Status: ACCEPTED. Found by the independent check of
  C-008 revision 0 (C-008_manual.md section 14.2, RC-15), recommended in ChatGPT's Stage 1 review of C-008 (23-Oct-2026),
  approved by Rama on 23-Oct-2026 and recorded in DR-061 together with the C-008 freeze. The struck
  text stays visible; the replacement is the text in force.
- What changed: a qualification. Broker errors recorded during sizing, risk approval or capital reservation do not all
  count toward the automatic SOFT_KILL. At the pinned legacy commit the signal processor passes each to the kill switch's
  record_api_failure, whose failure counter counts BrokerTimeoutError and BrokerRateLimitError only; other error classes
  are not counted by this path. FACT (SOURCE-VERIFIED at 970aabf: signals/signal_processor.py 1111-1125, 1314-1335;
  capital/kill_switch.py 809-850). See B-004 section 7.3 (FIX-191, 23-Jun-2026) and C-008_manual.md sections 6 (EX-02)
  and 14.2 (RC-15).
- Passages checked and not changed: section 9's trigger row "API failure count reaching api_failure_threshold 3", which
  says only that the processor records failures; the other rows of section 10.
- Reason: the row described the processor's call to the kill switch, not the kill switch's whitelist (FIX-191).
- Impact: wording only. This is a source-level legacy finding, not evidence that any particular error or SOFT_KILL event
  occurred. No V2 decision depends on the old wording.
```

3. The sentence added to B-009's status line, after the sentence on erratum E-2: "Erratum E-3 to this manual (section 10: which broker errors count toward the automatic SOFT_KILL) added 2026-10-23 during C-008, recommended in ChatGPT's Stage 1 review of C-008 (23-Oct-2026), approved by Rama on 23-Oct-2026 and recorded in DR-061 together with the C-008 freeze."

4. Checked, not changed: no other line under docs/agenda at dce3164 says these errors count toward SOFT_KILL. A search of all 186 files there (the manuals, addenda, review notes, V2-inputs files and the review inputs kept with them) for "BROKER_ERROR", "record_api_failure" and "counted toward" found, besides line 235, only B-004_manual.md lines 109 and 164 (an authentication error is not counted) and A-011_manual.md line 129 (RECORD-DERIVED, 18-Jun-2026, before FIX-185); B-004_manual.md line 148 states the whitelist. At the freeze, "DR-0NN" was replaced by DR-061, this freeze's register row.

### 14.7 The 18-Sep manual, chapters 02 and 03 (statement level; revision 3)

ChatGPT's review of revision 2 required this reconciliation before the freeze, chapter 03 covering the start, the run and the stop and chapter 02 the wiring ([OQ-03]). The chapters: 02_REPOSITORY_AND_FILE_MAP.md (3,557 lines; SHA-256 8B5AEC74544C7A613CED958F90CA9DEEDB524DB321AE7101B9A577015CBF6D49) and 03_STARTUP_RUNTIME_AND_SCHEDULER.md (321 lines; SHA-256 F7D38F2D0F7EA2D61783970B1E8A81FC06D1676AB613EE4DB18178EE5CBC3087), obtained as section 1.2 describes. They are read as documents, not as legacy source, and are not modified; "02:N" is line N of chapter 02.

**Two trees.** C-008 describes the production snapshot at the pin, 970aabf. Both chapters describe another tree, the testing worktree at commit e7bf477 with an uncommitted change set, byte-equivalent to the testing VM as delivered on 17-Sep-2026, and both say "PRODUCTION WAS NEVER READ" (02:3, 03:5). Their line numbers are not comparable with C-008's citations (IX-02), and their statements were not checked against their own tree. Each difference below is a difference between two documents about two trees; it does not show that either tree was other than its document says.

**Bounds.** The comparison covers only what the chapters say about the processor: chapter 02, read only in part, through its entries for main.py and the processor and the other entries that name the processor (the ranges and searches the companion file lists); chapter 03, read in full, where it speaks of the start, the run, the stop and scheduling. It claims no agreement beyond its rows. The statement-level table is the companion file C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md (CH-01 to CH-61; Rama's decision D-4).

Result, within the bounded overlap: 61 statements compared; 41 CONSISTENT, 8 PARTIAL, 3 CONFLICT, 9 NOT COMPARABLE.

The eight PARTIAL rows qualify a chapter statement without contradicting C-008: the age figure, "60s" against 600 s at the pin ([CF-01], [D-04]); the boot-order assertion under optimisation ([W-08], at language level only); the drain at stop ([SD-03]); the target's ATR branch ([K-01]); "release on every abort path" ([K-05], [EX-05], [EX-09]); the kill switch's "mutation" ([EX-02], [SD-02]; erratum B-009 E-3); chapter 03's teardown list, which leaves out the processor's stop ([W-09]); and the in-flight counter's sites, where the chapter lists a decrement on E4 that [SS-03] had left out: the saved output of read 042, re-viewed under Rama's decision D-5, shows it (`signals/signal_processor.py:1692-1698`), and [SS-03] is corrected. The nine NOT COMPARABLE rows stand: tree differences (`sr_shadow`; the line counts, section 1.1), steps C-008 did not read (the price helper's bounds step; the metrics methods, section 16) and points C-008 does not address (the dynamic imports, the stop trigger, main's thread, the scheduler, who calls the S&R detector's factory, and the shadow tracker's answer when disabled, [U-01]).

The three conflicts, and Rama's decisions:

| ID | The chapters (testing worktree) | C-008 (production snapshot at the pin) | Result and Rama's decision |
|---|---|---|---|
| CX-01 | C1, CH-04: "tears everything down in reverse order" (02:103); "Reverse-order teardown" (02:108) | the processor starts before the entry gate ([W-08]) and is also stopped before it ([W-09]); [W-10] rests on that order | CONFLICT between the two documents. Rama's decision D-1 (24-Oct-2026): option (a), recorded here; C-008's statements are kept and the 18-Sep manual is not changed. Whether the testing tree stopped the two in another order is NOT ESTABLISHED |
| CX-02 | C2, CH-11: "every decision is delegated to injected objects" (02:952) | the processor's own checks and rejects ([OC-02], [ST-21], [ST-34]) | CONFLICT between the two documents. Rama's decision D-2 (24-Oct-2026): option (a), as for CX-01. The chapter's own entry describes processor-owned rejects (02:957) |
| CX-03 | C3, CH-26: "REJECTED_QUEUE_FULL (437-439)" among the statuses the processor writes (02:959) | the bare `REJECTED` with reason `QUEUE_FULL` (section 5.1; [RQ-06]), in a branch not wired at the pin ([ST-01]) | CONFLICT between the two documents. Rama's decision D-3 (24-Oct-2026): option (a), as for CX-01; the source boundary is not widened to the testing snapshot. Whether the testing tree wrote another string is NOT ESTABLISHED |

Result: within the bounded overlap, three conflicts between the two documents, each recorded with Rama's decision; one C-008 statement corrected ([SS-03]), two made more precise ([W-08]; section 1.1) and a pointer added to [U-01] (Rama's decision D-6); no erratum to a frozen manual; nothing changed in the 18-Sep manual, which is not a manual of this series. [OQ-03] is answered.

## 15. Open questions and unknowns

- **[OQ-01] OPEN QUESTION (agenda mapping, authorisation C):** which topic owns the entry throttle's rule (for example D-016 or F-010), and which owns the shadow tracker (not named in the master agenda). Left for the master agenda review; C-008 does not resolve it.
- **[OQ-02] ANSWERED in revision 2:** the reconciliation against LEGACY_SYSTEM_MANUAL_INDEX.md, required before the freeze by ChatGPT's reviews, is section 14.3: no conflict and no erratum, at the level of the index; what remains is [OQ-03].
- **[OQ-03] ANSWERED in revision 3:** ChatGPT's review of revision 2 required a statement-level reconciliation against the 18-Sep manual's chapters 02 and 03 before the freeze. It is section 14.7: within the bounded overlap, three conflicts between the documents, which describe another tree (CX-01 to CX-03), each recorded with Rama's decision of 24-Oct-2026, and one C-008 statement corrected ([SS-03]). The chapters that would cover C-008's subject are not started (IX-03).
- **[U-01]** What `ShadowTracker.is_tracking` returns while the tracker is disabled (orders/shadow_tracker.py is outside the inventory and was not read). The 18-Sep manual's chapter 02, which describes the testing worktree e7bf477 and not the production snapshot, says it "returns False when disabled" (02:1983; section 14.7, CH-49; Rama's decision D-6). That is a statement about another tree, not evidence for the pin; U-01 stays open.
- **[U-02]** What the store's `has_active_position`, `get_active_position_direction` and `count_executed_trades_today_for_symbol_direction` count, in particular a candidate holding a reservation without a trade row (core/state_store.py, R24, cross-reference only), and when the placer writes the trade row (G topics).
- **[U-03]** Whether the client-side `BrokerRateLimitError` reached the processor through `place()` (broker/rate_limiter.py not read).
- **[U-04]** Narrowed. HISTORICAL (B-005 section 5, rows W1, W2 and W6): the placer's timeout-recovery queue is read only by the reconciler's in-flight recovery prepass, which runs only when the reconciler was given an order placer; main.py passes none, so "nothing drains the queue" (the main.py half re-verified, [EX-08]). Not established here: the reconciler's gate at the pin (orders/order_reconciler.py not read in C-008) and whether any entry was ever put in that queue ([U-07]).
- **[U-05]** Whether the notifier's send blocks the worker (L topics).
- **[U-06]** The deployed configuration of any host; repository values only here.
- **[U-07]** Whether any path described here ran on any date: no logs or records were read (R34 to R36 excluded at Stage 1).
- **[U-08]** The allocator's pre-check reasons (E4 only; not read).
- **[U-09]** Exceptions inside the screener beyond its quote fetch (D topics).

## 16. Known limits and exclusions

- Bounded reading: the processor's own methods were read in full ranges except the price helper's bounds step (`signals/signal_processor.py` lines 1851 to 1894, E topics), the runtime-metrics method (lines 684 to 743) and `stats()` (lines 2664 to 2704; C-006 section 9). Callees were read at their interface, return and the branches named here only.
- Every negative here is bounded to its search: Q36 to Q53 over the 281 non-test Python files (s_rt), the 486 test files (s_tests), one file or the 16 strategy files, as each row of the inventory, section 7, states; the Stage 0 searches as the Stage 0 inventory states them (Q04 over 1,260 files, section 2.2; Q28 over the 869 code files, tests included, for names beginning eod_route, eod_signal or daily_signal, [EP-09]); and the gated greps used as negatives (W-01, W-04, ST-34, SS-06, D-09; reads 124 and 125) cover only the file each grep read. Each zero count has a positive control with the same tool, flags and file list (IX-08: Q42 for Q41, Q50 for Q45, Q51 for Q48, Q52 for Q49, Q53 for Q28). A count-only search matches lines, not behaviour.
- Records R34 to R36 were not read; the two excluded July records and the host records were not opened. The census figures quoted from B-009 and the docstring figures are HISTORICAL / RECORD-DERIVED.
- The tests were read as text and never run; they build the processor themselves.
- One local probe (Python 3.13.16, Claude's workspace) supports [K-11]; the legacy interpreter was not used.
- The reconciliation with the 18-Sep chapters (section 14.7) compares two documents about another tree with this draft. It covers only what they say about the processor (chapter 02 read in part, by the ranges and searches the companion cross-check file lists; chapter 03 in full), claims no agreement beyond its rows and keeps its nine NOT COMPARABLE results.

## 17. End-to-end source path (E1, pinned wiring, repository configuration)

```text
queue item (5-tuple, or a re-queued dict)                       C-006
  -> _process_one_safe (FIX-007 pre-check: not wired)            ST-01
  -> PROCESSING                                                   ST-03
  -> kill (entry) -> global window -> age 600 s -> shadow inning  ST-04 to ST-07    REJECTED_<check>
  -> scanner -> strategy -> control -> strategy window -> breaker ST-08 to ST-11    REJECTED_<check>
  -> secondary screener (writes PASSED / REJECTED_* / SKIPPED_*)  ST-12  -> return unless passed
  -> entry and SL (re-anchored on a live LTP unless pullback wait) ST-13 to ST-16
  -> sizing                                                       ST-17  REJECTED_SIZING_*
  -> V3 chain observe (shadow) ; allocator observe (shadow)       ST-18, ST-19
  -> in-flight count +1
  -> [portfolio lock: per-strategy cap -> symbol+direction/day -> risk approve -> reserve -> RESERVED]   ST-21
  -> target -> "INTRADAY SIGNAL" alert -> late kill -> shutdown -> throttle (records)   ST-23 to ST-27
  -> P1 evidence -> OrderPlacer.place()                           ST-28, ST-29
       success          -> PROCESSED -> S&R observer             ST-30, ST-31
       BrokerTimeoutError -> TIMEOUT (handle dropped; the placer keeps the reservation; recovery not wired, EX-08)
       BrokerRateLimitError (client side) -> re-queue, at most 3, then PLACEMENT_FAILED (the throttle would meet a retry, RQ-03)
       anything else    -> PLACEMENT_FAILED (release called; the placer's clean-up may release first, OC-04)
  -> finally: in-flight count -1 ; claim released unless re-queued or handed off   ST-32
E2 (gate): only reloaded gate_state rows released with PRICE_HIT; no age check, no screening   EP-01, EP-02
E3 (retest): not wired with the repository flag                                                EP-05
E4 (allocator enforce): no hand-off from the processor in "shadow" (the allocator's worker not read)   EP-07
```

## 18. Final factual position

- At the pin, main.py builds one processor and wires one live entry path, the dispatcher's `_process_one`, which runs a fixed sequence: a `PROCESSING` write, four pre-flight checks, strategy resolution and three strategy checks, the screener, price derivation, sizing, two shadow observers, a portfolio-locked admission (a per-strategy cap, the symbol-direction rule, risk approval, reservation, `RESERVED`), target derivation, the alert, two late checks, the throttle and the placement (section 4). This describes the source and the repository configuration, not what ran on any day.
- The processor writes statuses by `signal_id`: `PROCESSING` at the start of each path, `RESERVED` inside the lock, `REJECTED_<check>` for a reject, and `PROCESSED`, `TIMEOUT` or `PLACEMENT_FAILED` at the end. A passing check writes nothing and a re-queue writes no status; the screener and the diverter write their own statuses (section 5).
- On E1 the processor releases the reservation, or calls the release, on every reject and failure path read except two: a placement timeout, which leaves it to a reconciler recovery that B-005 found not wired (EX-08), and a handler whose own status write raises (K-05, EX-09). On a broker failure inside `place()` the placer's own clean-up may release it first (OC-04). The receiver's claim is released on E1 on every exit except a re-queue or an allocator hand-off (OC-05).
- On a client-side `BrokerRateLimitError` from `place()` the processor re-queues the item, at most 3 times and then `PLACEMENT_FAILED`. A re-queued item runs the whole pipeline again, and the inspected code path would let the throttle, which recorded the first attempt, refuse a retry that falls within its gap, burst or cooldown (RQ-01, RQ-03; INTERPRETATION).
- Three other entry points exist: the gate path, reachable only from reloaded gate rows (the gate's `add()` has no runtime caller, B-009 D1); the retest path, not wired with the repository flag; and the allocator's enforce path, to which the processor hands nothing in "shadow" (section 9).
- The source shows no per-signal processing deadline, a disabled shadow tracker still consulted, and handlers whose unguarded status writes come before the release; it also composes two reject names that the inspected code path would store with the prefix doubled (K-01, INTERPRETATION) (section 12). Whether any of these mattered on a trading day is not established.

## 19. Revisions

### 19.1 Revision 0 (2026-10-23)

Claude's Stage 1 draft, written from the saved evidence of the logged reads and searches (the inventory). For ChatGPT's review and Rama's decision; not frozen, not committed, not pushed. Kept unchanged as delivered: C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_23-OCT-2026.md (550 lines; SHA-256 beginning 6e5e4d349b7fdbc6) and its companion C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_23-OCT-2026.txt (864 lines; SHA-256 beginning 2894a556b1e75710).

### 19.2 Independent check before the delivery of revision 0 (2026-10-23)

A separate agent checked a snapshot of the draft (SHA-256 beginning b2d6e7518736a4ce) as section 1.2 describes. Claude re-checked each finding against the saved evidence and the frozen manuals before applying it. All 22 were applied in revision 0 as delivered; none was rejected. The applied fixes were not checked again by a separate agent.

| Finding | Severity | Applied in |
|---|---|---|
| IC-01 | MUST-FIX | ST-33: the shadow-inning guard is one shared helper, listed with the cap and the symbol-direction rule |
| IC-02 | MUST-FIX | Section 18, third bullet: the case of a handler whose own status write raises (K-05, EX-09) |
| IC-03 | SHOULD-FIX | EP-07, EP-08, sections 2.2, 17 and 18: bounded to the processor's hand-off; the allocator's worker stated as not read; the label "INTERPRETATION (CONFIG)", not one of section 1's labels, replaced |
| IC-04 | SHOULD-FIX | OC-04 (its new sub-item), EX-04, ST-30, RQ-01 and section 17: the processor "calls the release"; the placer's clean-up stated, its body not read |
| IC-05 | SHOULD-FIX | OC-06: `entries_rejected` scoped to E1 to E3; E4's handlers bump no runtime metric |
| IC-06 | SHOULD-FIX | SD-01: the count scoped to the checks before `place()`; the placer's last-mile check cited as HISTORICAL |
| IC-07 | SHOULD-FIX | SD-03: C-006's qualifier restored; the late-item half of BM-03 addressed (a FIX-069 re-queue during stop) |
| IC-08 | SHOULD-FIX | K-05: the condition on the generic handler's own write; the case of a `PROCESSED` write that raises |
| IC-09 | SHOULD-FIX | K-06: "no processor-level limit"; the callees' timeouts not traced (B-004 section 7.4; the screener's executor) |
| IC-10 | SHOULD-FIX | K-10: B-009's census dated; the docstring's "BEFORE Step 2" restored; "E1 to E3" |
| IC-11 | SHOULD-FIX | K-11: the re-anchor (ST-16) and the unread bounds step |
| IC-12 | SHOULD-FIX | Section 13, closing paragraph: scoped to the six ranges read; `SignalProcessor.__new__` noted; no search made for a re-queue test |
| IC-13 | SHOULD-FIX | RC-15 added and section 14.2's closing paragraph changed: an erratum candidate for B-009 section 10, for ChatGPT and Rama, not decided in revision 0 (accepted in revision 1: section 14.6) |
| IC-14 | SHOULD-FIX | U-04 narrowed, with B-005 section 5 as HISTORICAL |
| IC-15 | SHOULD-FIX | Section 18, second bullet; section 18 now describes the source in the present tense |
| IC-16 | SHOULD-FIX | RC-10: B-004 section 7.1 added |
| IC-17 | NOTE | EX-03 (the governor as the opposite case), D-05, D-11, SS-02 (the fact moved out of the interpretation), SD-02 (the whitelist) |
| IC-18 | NOTE | K-04 (no gate enabled), ST-10 (an out-of-range time), EX-04 (the R:R gate's condition), T-04, RQ-03 (the test text; the gap counts from any symbol's entry) |
| IC-19 | NOTE | SS-01 (the "symbol_lock" comment), SS-04 (C-005's cache condition), SS-03 (two mechanisms, as in BK-07) |
| IC-20 | NOTE | Section 18, fourth and sixth bullets; section 17's re-queue line |
| IC-21 | NOTE | Section 1.2 (the masks); section 16 (Q04, Q28 and the one-file greps); section 2.2 (the callers' basis, with reads 124 and 125 of v3_chain/pb01_runner.py and v3_chain/pb01_entry.py, which printed docstring lines only); W-08 and W-09 ("first"); CF-09 |
| IC-22 | NOTE | EP-07, SS-03 and ST-11 cut back to the processor-side position, with cross-references to J-012 and F topics |

Also corrected while applying them: D-10's quotation (no words had been left out, so its "..." is removed), and section 1's quotation rule now says what "..." marks.

### 19.3 Revision 1 (2026-10-24)

Revision 1 applies ChatGPT's Stage 1 review, C-008_CHATGPT_STAGE1_REVIEW_AND_OPEN_DECISIONS_23-OCT-2026.txt (the header), and Rama's two decisions of 23-Oct-2026 (about 23:58 IST, multiple choice). No other fact, label, citation or bookmark changes, and no read or search was added: 125 reads and 14 Stage 1 searches, as in revision 0. Each review item:

| Review item | What revision 1 does |
|---|---|
| 1 (review outcome; the distinctions between labels to preserve) | Kept; nothing changed |
| 2 (RC-15, recommended erratum) | Rama approved it as erratum B-009 E-3. RC-15's result cell and the closing paragraph of section 14.2 record the decision; the new section 14.6 holds the texts to apply at the freeze, adapted from the review's suggested wording. No frozen file is changed now |
| 3A (the entry throttle's owner) | Unchanged: [OQ-01] stays open; C-008 records only the throttle's call and outcomes (ST-27, K-04, RQ-03) |
| 3B (LEGACY_SYSTEM_MANUAL_INDEX.md) | Unchanged: [OQ-02] and section 14.3 stay pending, by Rama's choice to return revision 1 first (section 1.2); no replacement index is made and its contents are not inferred. The reconciliation is required before the freeze |
| 3C (no second independent check) | Section 1.2 and section 19.2 now say that the applied fixes were not checked again by a separate agent |
| 4 (points to preserve) | Checked against the draft and kept: repository configuration is not deployed configuration (sections 1 and 3); the gate and retest paths are kept apart from the live dispatcher path and are not presented as proven runtime events (sections 2.2 and 9); the allocator and the V3 chain are bounded to the processor-side calls, their internals not read staying unknown (sections 2.2, 9.3 and 10); the timeout and reconciler mismatch stays a known defect with bounded evidence, not proof that a timeout occurred (EX-08, D-05, U-04); the throttle's owner and the index reconciliation stay unresolved, and RC-15's erratum waits for the freeze; the two V3 reads stay described as docstring-only references from the approved inventory (section 2.2) |
| 5.4 (factual or editorial issues) | The review named none; none was changed |
| 5.5 (return the revised files) | Revision 1 and its companion inventory are returned under their own names (the header); nothing is frozen, committed or pushed |

Kept unchanged as delivered: C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV1_24-OCT-2026.md (612 lines; SHA-256 beginning dc34d02f0811ba23) and its companion C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV1_24-OCT-2026.txt (877 lines; SHA-256 beginning 357c111274b3f886).

### 19.4 Revision 2 (2026-10-24)

Revision 2 applies ChatGPT's review of revision 1, C-008_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (the header), with LEGACY_SYSTEM_MANUAL_INDEX.md, which Rama supplied with it. No source file was read (125 reads, as in revision 1); four count-only searches were added, Q50 to Q53, as positive controls (IX-08). Each review item:

| Review item | What revision 2 does |
|---|---|
| What checked out (items 1 to 6) | Kept; nothing changed |
| Freeze blocker: supply the index, reconcile, record the result, re-review the changes | The index is recorded (section 1.2; SHA-256 in the header) and reconciled (section 14.3, IX-01 to IX-11): no conflict and no erratum. [OQ-02] is answered at the level of the index; [OQ-03] asks whether a statement-level reconciliation against the 18-Sep manual's chapters 02 and 03 is needed. The changes listed below are for re-review |
| The freeze-time steps for erratum B-009 E-3 | Unchanged: section 14.6 already states them (the anchor and the number rechecked against the live register and main, and the two DR-0NN placeholders replaced, at the freeze only); nothing is applied now |
| Review limitation (a document review; Claude's local checks not reproduced) | Recorded in section 1.2 |
| Decision (the revision 1 files kept as review drafts; no commit, push or freeze) | The revision 1 files are kept unchanged (section 19.3); revision 2 has its own file names; nothing is frozen, committed or pushed |

Changes for re-review: the header; section 1 (the search count, and the quotation rule now naming arrows, Markdown emphasis and code marks); section 1.2 (the searches bullet, ChatGPT's review of revision 1, the index); the positive controls in CF-15, K-01, EP-09, section 13 and section 16; section 14.3 (the reconciliation); OQ-02 answered and OQ-03 added; sections 19.3 and 19.4. Nothing else changed.

Kept unchanged as delivered: C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV2_24-OCT-2026.md (648 lines; SHA-256 beginning 820b36b9ebe2aba1) and its companion C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV2_24-OCT-2026.txt (966 lines; SHA-256 beginning d14755194a9648da).

### 19.5 Revision 3 (2026-10-24)

Revision 3 applies ChatGPT's review of revision 2, its decision note and its review of the cross-check and the proposed changes (the header), with Rama's decisions of 24-Oct-2026 (about 08:08 IST, multiple choice). No new legacy source read was performed: 125 legacy reads and 18 Stage 1 searches, as in revision 2. Added: 18 document reads of the two 18-Sep chapters (section 1.2). Three saved outputs of earlier logged reads were re-viewed for the cross-check, and one of them again under decision D-5 (section 1.2). Rama's decisions:

| Decision | Rama's answer (24-Oct-2026) | Where |
|---|---|---|
| D-1, D-2, D-3 (C1 to C3: how the conflicts are recorded) | Option (a) for all three: each recorded as a difference between the documents; C-008's statements kept; nothing changed in the 18-Sep manual; the source boundary not widened to the testing snapshot | Section 14.7, CX-01 to CX-03; the companion cross-check, section 4 |
| D-4 (where the 61-row table lives) | (i) A companion file, to be frozen with revision 3 when approved | The header; section 14.7 |
| D-5 (one check of a saved output) | Yes: the saved output of read 042, `signals/signal_processor.py:1688-1705`, for one question only | Section 1.2; [SS-03] corrected; the companion cross-check, CH-34 |
| D-6 (a pointer in U-01) | Adopted, as a statement about the testing tree only; U-01 stays open | [U-01] |
| D-7 (the fetch from the records repository) | Rama's 00:51 instruction recorded as superseding the restriction for the two chapter files only | Section 1.2; section 1, "Not done" |

ChatGPT's items and guards:

| Item | What revision 3 does |
|---|---|
| Review of revision 2: OQ-03 (a statement-level reconciliation is required before the freeze) | Section 14.7 and the companion cross-check; [OQ-03] answered; IX-04 updated |
| Review of revision 2: what checks out; OQ-01 open; erratum B-009 E-3 at the freeze only; Rama's approval under W-4 | Kept; [OQ-01] open; section 14.6 unchanged |
| Decision note, instructions 1 to 8 (the sources; identify the chapters; a bounded statement-level check; no re-audit; the table; stop on a contradiction; the wording if none; OQ-03 only in a proposed revision) | The cross-check, first delivered for review and re-issued as the companion; the proposals file; this revision after their review. Three contradictions, decided by Rama; the restriction of instruction 1 superseded by Rama for the two chapter files only (D-7) |
| Guard 1 (status DRAFT, not frozen) | The header |
| Guard 2 (each decision recorded with its date in section 14.7 and the companion file) | Section 14.7; the companion cross-check, section 4 |
| Guard 3 (D-5 settled before SS-03 changes) | D-5 answered and the check made before the change ([SS-03]) |
| Guards 4 and 5 (the comparison bounded; chapter 02 read only in part; no agreement claimed beyond the rows; the nine NOT COMPARABLE results kept) | Section 14.7, "Bounds"; section 16 |
| Guard 6 (the production and testing trees kept apart for each conflict) | Section 14.7, "Two trees" and the column heads of its table |
| Guard 7 (the assertion: a language-level fact only; production's launch flags UNKNOWN / NOT ESTABLISHED) | [W-08] |
| Guard 8 (the Lines note a reader-method note; no cause asserted) | Section 1.1 |
| Guard 9 (Rama's later instruction recorded as superseding the restriction for the two files only) | Section 1.2 (D-7) |
| Guard 10 (the artefacts and logs reconciled; the chapter reads, the legacy reads and the searches kept apart) | Section 1, section 1.2 and the inventory, section 11 |
| Guard 11 (precise wording on source reads) | Section 1.2: "no new legacy source read was performed", with the saved outputs re-viewed named |
| Guard 12 (no freeze, commit, push or publication until reviewed and approved under W-4) | The header; nothing is frozen, committed or pushed |

Kept unchanged: the cross-check as first delivered, C-008_CHAPTER_02_03_CROSS_CHECK_24-OCT-2026.md (175 lines; SHA-256 beginning 8f44607ae1c07a00), and the proposals, C-008_REV3_PROPOSED_CHANGES_24-OCT-2026.md (257 lines; SHA-256 beginning c3bcc73195bcbc96).

Changes for re-review: the header; section 1 (the document reads; the records repository); section 1.1 (the Lines note); section 1.2 (ChatGPT's review of revision 2, the chapters, the saved outputs re-viewed, ChatGPT's review of the cross-check and Rama's decisions); [W-08] (the assertion); [SS-03] (E4); section 14.3 (IX-04 and its closing line); section 14.7; [OQ-03]; [U-01]; section 16; sections 19.4 and 19.5. Nothing else changed.

### 19.6 Freeze (2026-10-24)

Approved by Rama under W-4 (DR-017) on 24-Oct-2026 (09:15 IST): exactly these three files, as delivered and as ChatGPT reviewed them. Re-hashed when the freeze files were built: none had changed.

| File | Lines | SHA-256 | Stored |
|---|---|---|---|
| C-008_SIGNAL_PROCESSING_MANUAL_DRAFT_REV3_24-OCT-2026.md (revision 3 as approved) | 714 | 39CC1AA06B4DE4761E3C3C4B9F2306E6247E2C42B43F93B7B5E4BDF30747FCE4 | docs/inputs/manuals/, unchanged; this file is its frozen form |
| C-008_STAGE1_SOURCE_INVENTORY_READ_LOG_AND_METHOD_REV3_24-OCT-2026.txt (the companion inventory) | 1,083 | 7AE9FA01BDA1823B66CE222E8746814B956265A2C89845EC3D35EC5D962BFCC3 | docs/agenda/c-008_signal-processing/, unchanged |
| C-008_CHAPTER_02_03_CROSS_CHECK_REV3_24-OCT-2026.md (the companion cross-check) | 186 | 9B1878B02869C7700B1F86A3BC41348F9CE93CA4341D7C679AC8C2CF079B4347 | docs/agenda/c-008_signal-processing/, unchanged |

No statement changed. This file is revision 3 as approved with these freeze edits only: the title ("(DRAFT FOR REVIEW)" removed); the status line (FROZEN, ChatGPT's review of revision 3 and Rama's approval); the companions line (where each file is stored, and the documents not stored); a bullet in section 1.2 (ChatGPT's review of revision 3 and Rama's approval); RC-15's result cell (applied, DR-061); section 14.6 (its heading, the freeze-time recheck, and DR-061 for "DR-0NN" in the two texts and in item 4); and this section. The citations, claim IDs and evidence labels are those of revision 3 (619 citations, 0 uncovered; 205 claim IDs). Erratum B-009 E-3 is applied with this freeze (section 14.6). At Rama's choice, ChatGPT checks the freeze package before the push (C-008_review_notes.md section 7).
