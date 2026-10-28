# D-005 - CANDLE ANALYSIS (LEGACY MANUAL)

## 1. Document status, purpose and scope

- Status: FROZEN 2026-10-28 (revision 3, DR-071), as factual legacy documentation only: it makes no V2 design or implementation decision and no claim about deployed or runtime behaviour. ChatGPT accepted revision 3 as a reviewed draft (D005_CHATGPT_FINAL_REVIEW_REVISION3_DRAFT_ACCEPTED.txt: "ACCEPTED AS A REVIEWED DRAFT"; it computed revision 3's identity, 234 lines, 26,126 bytes, SHA-256 13B937825AF4BD3BF794782CEDE7E0B6CB4FD8D519C9F10939EFECDF1EC177BB). The freeze and storage step was authorised by D005_CLAUDE_PROMPT_FINAL_FREEZE_STORAGE_PUSH_28-OCT-2026.txt with D005_FREEZE_STORAGE_ANSWER_FILE_28-OCT-2026.txt (received from Rama at 14:21 IST), and the freeze takes effect with the storage action that stores this file. This file is revision 3 (stored unchanged as docs/inputs/manuals/D-005_CANDLE_ANALYSIS_REV3_28-OCT-2026.md) with the freeze edits (the title, this status line, the manifest's name in the inputs bullet below, and section 10) and one pre-freeze correction, C-1, in sections 6, 7 and 10: HISTORICAL cross-references, in sections 6 and 7, to two single-candle measures or condition sets recorded in the frozen D-003 and A-008 manuals, with the matching wording in section 10, found by the frozen-manual cross-reference search before the freeze and approved by Rama's decision, D005_CHECKPOINT_DECISION_APPROVE_C1_AND_PROCEED_TO_FREEZE.txt (received at 14:39 IST). C-1 adds no legacy read and changes no D-005 formula, finding, label, caveat, open item, erratum or deviation. Revision 1 was prepared by Claude on 28-Oct-2026, 13:48 to 13:57 IST. Revision 2 (14:02 IST onward) applied corrections A to D of D005_CHATGPT_REVIEW_DRAFT_AND_MANIFEST_REQUIRED_CORRECTIONS.txt; both the manual and its manifest were hand-edited for it. Revision 3 (14:08 IST onward) applied the two wording clarifications of D005_FINAL_EDITORIAL_CLARIFICATIONS_RESHARED.txt, by the same hand-editing of both files. The storage path follows the D-series convention: docs/agenda/d-005_candle-analysis/.
- Authority: D005_CHATGPT_STAGE1_CLOSURE_AND_MANUAL_DRAFT_AUTHORIZATION.txt (received from Rama at 13:48 IST; SHA-256 E4A260AAA18A20AE4C235EA95DD44E257B4915A2120B1C596EF43E9EC0910D7E), decisions 1 to 3.
  - Stage 1 is complete, with its open items carried (decision 2).
  - This draft is a synthesis of the authorised Stage 1 evidence and adds no new research (decision 3).
- Purpose: to document, factually, the individual-candle (per-bar) measures of the legacy system that the authorised legacy source reads establish.
- In scope: measures computed from one candle, or one candle with its previous close, and the narrow source facts the checkpoints accepted about them.
- Out of scope, by the checkpoints' scope reminder:
  - multi-candle formations and patterns (D-006);
  - timeframe hierarchy (D-007);
  - trend and regime (D-008);
  - S&R, zones, strength and freshness (D-009 to D-012);
  - entry, retest and timing (D-013 to D-016);
  - scoring, ranking and the final Trade/Reject (D-017, D-018);
  - D-004's data contracts, beyond brief cross-references.
- Inputs: the eight Stage 1 artifacts listed in the manifest (D-005_manual_manifest.md, stored beside this file) are the only substantive inputs to this synthesis. D-004 facts appear only as the HISTORICAL cross-references those artifacts already cite. The masked evidence files of reads 001 to 031 were consulted only to validate quotations and citations, as evidence generated earlier; that was not a new legacy source read (section 8).

## 2. Source identity and reproducibility

- Legacy source pin: commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb. Every file is identified by its full blob ID at the pin.
- Reads:
  - 31 gated reads, numbered 001 to 031, were made through Claude's read gate over the reading clone at the pin.
  - Before each read the gate checked the tree entry and the working file against the approved full blob.
  - Each read's output was masked by mask_read2.py and a second-stage mask, saved as an evidence file and logged with its SHA-256.
  - The evidence identifier of a fact is its read number; the full register is in section 9.
- Budget at Stage 1 close: 31 reads, 23 rows and 244 source lines, of the 900 planned lines (1,000 absolute) and 50 rows; 20 count-only searches.
- These are process facts as Claude recorded them. ChatGPT has not reproduced them independently (section 8).

## 3. Evidence labels

| Label | Meaning in this draft |
|---|---|
| FACT (SOURCE) | Code at the pin, in a line that a logged read printed (path, blob, line and read given). It shows what the code says, not that it ran. |
| FACT (TEXT) | A comment or docstring, printed by a logged read. Evidence of its own text only. |
| FACT (TEST) | A test name, printed by a logged read. Evidence of a tested intention only; no assertion was read. |
| FACT (COUNT) | A count-only search result (per-file counts and line numbers, with no matched text). The inputs document NO VALID REAL CONTROL for S01 s_rt, S04b and S04c (Part 1 evidence), S03, S06 and S02r (Part 2 evidence note), S07 and S08 (Part 3 count log), and S07d, S07i and S08a (Part 4 addendum). For the other first-step searches (S01 s_cfg and s_tests, S02, S04a, S04d, S05), the control status is UNKNOWN / NOT ESTABLISHED from these inputs; it is to be checked against count log revision 1 if that record is supplied through the project workflow. Whatever the control status, no search was exhaustive: every hit is a candidate locator only, and a zero is not evidence of absence. |
| HISTORICAL | A frozen V2 record (D-004), cited as the Stage 1 artifacts cite it. |
| INTERPRETATION | Claude's reasoning from the facts, marked as such. |
| OBSERVATION | A noted tension between two facts (for example, text against code), with no verdict. |
| UNKNOWN / NOT ESTABLISHED | Not shown by the authorised evidence. |
| Cross-topic bookmark | A question that belongs to another topic; recorded, not resolved. |

No label is merged with another. A test, an import, a source line or a header never shows that code executed or how production behaved.

## 4. Confirmed measure: `_strong_close`

Source: `sr_detector/retest_confirm.py` at blob 5fc212535bbd2e86f1c5899d07ce76331ab72724. Lines 130-136 come from read 025 and line 137 from read 030; nothing else in the file was read.

```
130: def _strong_close(c, frac: float, is_long: bool = True) -> bool:
131:     """Directional strong close: LONG wants the close in the UPPER frac of the 1m
132:     range, SHORT in the LOWER frac."""
133:     rng = c.high - c.low
134:     if rng <= 0:                                       # degenerate (flat) bar
135:         return c.close >= c.high if is_long else c.close <= c.low
136:     pos = (c.close - c.low) / rng if is_long else (c.high - c.close) / rng
137:     return pos >= frac
```

FACT (SOURCE):
- Inputs: one candle-like object `c` with high, low and close; a fraction `frac`; and a direction `is_long`, which defaults to True (line 130).
- Range: rng = high - low (line 133).
- Flat or non-positive range (rng <= 0): the "degenerate (flat) bar" branch returns close >= high for a long and close <= low for a short (lines 134-135).
- Otherwise:
  - pos = (close - low) / (high - low) for a long, and (high - close) / (high - low) for a short (line 136);
  - the function returns pos >= frac (line 137).
- In full:
  - long: (close - low) / (high - low) >= frac;
  - short: (high - close) / (high - low) >= frac;
  - when high - low <= 0, close >= high (long) or close <= low (short).

Errata (corrected in the Part 2 evidence note and accepted by the checkpoints):
- E-1 (Part 1 MC-08): pos is measured from the unfavourable end toward the favourable one, that is up from the low for a long and down from the high for a short. It is not measured "from the favourable end".
- E-2 (Part 1 MC-08's INTERPRETATION): line 133 uses high and low, not close, so a missing close does not raise at 133.

INTERPRETATION (arithmetic on lines 133-137 only; not runtime-tested; limited to the stated assumptions and branches, and not generalised to malformed candles or to any caller):
- Assuming numeric high, low and close, with high > low (the non-flat branch) and low <= close <= high: pos lies in [0, 1], and pos >= frac means the close lies in the top (1 - frac) of the range for a long, or the bottom (1 - frac) for a short.
- No check on frac appears in 130-137. Under the same assumptions, frac <= 0 lets any such bar pass and frac > 1 lets none pass.
- In the flat branch (high - low <= 0), if the close equals the high and the low, the function returns True in both directions.
- Under ordinary Python semantics for the printed expressions, a None or absent high or low would raise at 133, and a None or absent close at 135 or 136.
- Under ordinary floating-point NaN propagation through the printed arithmetic and comparisons, a NaN in high, low or close would make the function return False without raising. This is a mathematical interpretation, not a source fact, and it has not been runtime-tested.

FACT (TEXT): the docstring (lines 131-132) opens "Directional strong close:" and continues "LONG wants the close in the UPPER frac of the 1m range, SHORT in the LOWER frac."

OBSERVATION (text against code):
- If "UPPER frac" means "at or above the frac point of the range", the docstring agrees with `pos >= frac`.
- If it means "within the top frac of the range", the code would need pos >= 1 - frac. The two readings coincide only at frac = 0.5.
- No intended strategy meaning is inferred.

UNKNOWN / NOT ESTABLISHED:
- the value or default of `frac` passed by any caller;
- the callers, and how the result is used (D-015 owns the retest confirmation's use);
- whether any production path executed the function;
- whether text follows line 137. The file has 137 newline characters by the Stage 1 record (the reader's header shows 138 by its own convention, the count of newline-split parts); line 137 is a return statement, so a later line in the same function, if any, would not run (INTERPRETATION).

FACT (COUNT), candidate locators only: S07 (`\b_strong_close\b`, case-sensitive) found the name in s_rt at sr_detector/retest_confirm.py 124 and 130 and at orders/structure_exit_manager.py 70, 284, 290 and 293. It found no line in s_tests, which is not evidence of absence. S07d's only hit is line 130.

## 5. Import fact: `orders/structure_exit_manager.py:70`

FACT (SOURCE), read 031, blob 6e4c58d8d2e8aa0438a9461067ae070ed1118950: line 70 contains the literal text `from sr_detector.retest_confirm import _strong_close`. It is shaped as a one-line import naming `_strong_close` from sr_detector.retest_confirm. The line begins at column 0, with no alias, parenthesis, continuation or trailing comment. The mask found nothing on it.

NOT ESTABLISHED (checkpoint decision 1):
- that the line executes;
- that it lies outside a multi-line string;
- that the imported name resolves to the definition at sr_detector/retest_confirm.py:130;
- that the exit uses the name;
- that any production path ran.

The exit's other S07 hits are as follows; this draft describes no exit behaviour:
- 290 and 293: unread candidate locators;
- 284: a docstring line, read 023.

## 6. Non-findings and unresolved questions

- body_frac (G-02): UNKNOWN / NOT ESTABLISHED.
  - FACT (TEXT): in the lines read, the formula appears only in text. Line 385 of `screening/hard_gate.py` states `body_frac = |close-open| / (high-low)` (read 020; inside a string by its context, INTERPRETATION), and the trailing comment on line 1416 of `core/config_loader.py` states `body_frac=|close-open|/(high-low)` (read 026).
  - No code that computes body_frac was read. Its zero-range and missing-value handling are UNKNOWN.
  - Uncontrolled counts (S08, S08a) located two candidate lines, screening/hard_gate.py 409 and v3_chain/score.py 82. They were not read (fifth checkpoint, decision 3) and are not formula evidence.
  - The gate and configuration text around these lines is not documented.
- Single-candle measures recorded in other frozen manuals (HISTORICAL; not re-read for D-005, and outside its authorised reads):
  - D-003 records the secondary screener's step 5, price_action: min(1.0, 2 x body / range), with body = abs(ltp - open) and range = day_high - day_low + 1e-9; a key absent defaults to ltp, and a key present with None makes the body arithmetic raise, which the executor turns into an error step (D-003_manual.md section 8, step 5; MC-33, MC-39).
  - A-008 records the Chartink scanner definitions as a supplied record (A-008_manual.md section 17), with conditions on the daily candle such as Close > Open, Open = Low and today's high-low range above 0.5% of close. These are the external scanners' conditions, not legacy code (C-001).
- true_range_series (G-04): outside D-005 (second checkpoint, decision E).
  - FACT (SOURCE): its first line is `def true_range_series(candles: Sequence["Candle"]) -> List[float]:` (core/candle_math.py:54, read 001), a function over a sequence that returns a list.
  - Its body and its tests' assertions were not read for D-005. Read 002 printed test names only; three of them name the true range, at 109, 114 and 119 of tests/unit/test_candle_math.py (FACT (TEST), Part 1 MC-04).
  - D-004 records true range and ATR at core/candle_math.py:54-115 (HISTORICAL: D-004 MC-33, D-004_manual.md:231, as cited in Part 1).
- atr and resample (G-05): only their first lines were printed, `def atr(` (83) and `def resample(` (335) (read 001). Their signatures and bodies stay unread, a gap with no read needed now (second checkpoint, decision F). D-004 records their facts (HISTORICAL, MC-33).
- The other definitions in core/candle_math.py (read 001; INTERPRETATION by printed signature only, Part 1 MC-02):
  - session_vwap (136), adx (191) and rsi (250) take a candle sequence;
  - ema_series (165) and ema (184) take a sequence of floats.

  No printed signature takes a single candle. Whether they are window functions is not established. They are listed here by name and line only.
- The candle records' definition lines (FACT (SOURCE)):
  - Candle (sr_detector/models.py, read 003): definition lines at 24 (class Candle) and 34 (from_kite, body not read), and the next class at 51 (Pivot).
  - CandleData (data/candle_store.py, read 004): class at 25, and the next class at 40 (_Accumulator).
  - No other def or @property line was printed inside either record. INTERPRETATION: no member measure is defined on them by such a line. A member defined otherwise would not show.
  - Candle's fields are D-004's (HISTORICAL: MC-31); building, storing and closing candles is D-004's (HISTORICAL: MC-21 to MC-23).
- Execution (G-06): whether any importing or calling path ran is NOT ESTABLISHED. No runtime evidence was in scope. Text such as the regime package's "SHADOW - gates nothing in this step" (regime/__init__.py:6-7, read 010, FACT (TEXT)) is evidence of its own text only.
- Header text against imports:
  - FACT (TEXT), core/candle_math.py:28-30, read 006: the module header says "Wiring        : NONE yet (V3 Step 1 is build + unit-test only). Nothing in the live trading / scoring / sizing / S&R path consumes these functions".
  - OBSERVATION: among the 16 S01-located runtime lines, imports of its functions appear in core/daily_stats.py, regime/engine.py, sr_detector/zone_builder.py and the V3 chain (Part 1 MC-07). This is text against imports only (D-004 O-02, extended); whether those paths are live is not established.
- Searches in general: S01 to S08a were count-only.
  - The inputs document NO VALID REAL CONTROL for S01 s_rt, S04b, S04c, S03, S06, S02r, S07, S08, S07d, S07i and S08a (no frozen V2 record quotes a code-level hit). For the other first-step searches the control status is UNKNOWN / NOT ESTABLISHED from these inputs (section 3). Synthetic controls tested the pipeline only.
  - No search was exhaustive. No zero has been read as absence, and no candidate has been promoted to a confirmed formula.
  - The broad S04a, S04d and S05 hits (G-08) and the other hits (G-07a) remain candidate locators.

## 7. Cross-topic boundaries and bookmarks

| Question or hit | Owner | Status in D-005 |
|---|---|---|
| Candle's fields (MC-31); building, storing and closing candles (MC-21 to MC-23); daily stats, which import atr and rsi (MC-53) | D-004 | cross-reference only |
| multi-candle formations and patterns (scope boundary only) | D-006 | not documented |
| resample and the entry timeframe (a configuration comment at core/config_loader.py:1544 names resample, read 007) | D-007 (and D-016, as Part 1 records) | not documented |
| regime/engine.py S04c hits 191, 207, 245, 340 (UNKNOWN / NOT ESTABLISHED); its imports of adx, atr, ema_series and true_range_series (Part 1 MC-07) | D-008 | outside D-005. D5-BK-6 is a proposed deferred bookmark only, not stored (fifth checkpoint, decision 4). |
| Zone and ScoredZone properties, sr_detector/flags.py 159-162, and zone_builder's session_vwap import | D-009 to D-012 | classified S&R; not documented |
| the retest confirmation's use of `_strong_close` and its frac value | D-015 (entry, retest and timing: D-013 to D-016) | not documented |
| the structure exit's use of the imported name (S07 hits 290, 293) and its logic | outside D-005; owner not set by the inputs | not read |
| the gate text around body_frac in screening/hard_gate.py (reads 020, 021) and its line 409 | J-010, as recorded in Part 1 | 409 not read; the gate text not documented |
| v3_chain/score.py 82 and the scoring use of body_frac | D-017, D-018 | not read |
| the S08 hits in v3_chain/pb01_entry.py, v3_chain/pb01_runner.py and tests/unit/test_v3_pb01_gates_score.py | entry (D-013 to D-016; C-011 and J-010 as recorded in Part 1) and gate/score tests, by file name only | not read |
| the V3 chain's imports of atr and ema | C-011, J-010 (BK-57, BK-60, as recorded in Part 1) | not documented |
| price_action, the secondary screener's body-to-range step (D-003 MC-33, MC-39) | D-003 (frozen; scoring use D-017, D-018) | cross-reference only (HISTORICAL) |
| the Chartink scanners' single-candle conditions (A-008 section 17) | C-001, A-008 (frozen) | cross-reference only (HISTORICAL) |

## 8. Method and process notes

- Gate, masking and logging as in section 2. The masking filter loads HR-1 and HR-2 value-blind as its host-token source, by its code; HR-3 is absent. No host-record value appears here.
- Count runs: a gate first; per-file counts and line numbers only; object counts and the protected-store check unchanged; the excluded-range rule applied, and no hit fell inside main.py 3695-3713. Reads: no range of main.py 3695-3713 or core/analytics_schema.sql 28-45 was read.
- Checkpoints: each Stage 1 read and search ran only after a ChatGPT checkpoint authorised it; the checkpoint files are named in each Stage 1 report. The Part 1 reads rested on Claude's reading of that checkpoint's item E (Part 1, authority paragraph). Rejected or unauthorised items were not run: R01c, R01d, R02b, R08a, R09, R10, R10a, R10b, R11 and S05'.
- Process deviations and corrections, preserved as recorded:
  - DV-02: one `wc -l` on core/candle_math.py was run outside the gate and outside the authorised operations, between 12:07:21 and 12:08:04 IST. It printed only a number already recorded and was not repeated. ChatGPT has not verified it.
  - DV-03 (draft stage), a separate deviation that does not replace DV-02:
    - After the draft was written, Claude's check script check_m_d005.py ran. It ran sensitive_check.py, the object counts of the reading clone (a git object listing) and `git status --porcelain` on the V2 clone. Claude reports the results as exit 0, 816 blobs, 101 trees and 1 commit unchanged, and clean.
    - Revision 1 of the draft's register and of the manifest was generated by Claude's build script, build_m_docs_d005.py, from the inputs. Revisions 2 and 3 of both files were hand-edited; the script was not rerun.
    - These actions were outside the draft-stage authorisation, which excluded scripts and operational checks. As Claude reports, no repository write occurred. ChatGPT has not reproduced them. None was rerun for revision 2.
    - Related disclosure: the check scripts for the earlier Stage 1 reports also ran sensitive_check.py, the object counts and `git status --porcelain` on the V2 clone, never on the legacy reading clone. Claude had read the earlier checkpoints' `git status` prohibition as applying to the reading clone.
  - E-1 and E-2: as in section 4.
  - The Part 1 evidence also discloses that its independent review agent wrote two temporary hash files to the scratchpad and deleted them.
- Inputs and validation: the eight artifacts named in the manifest were the only substantive inputs. The masked evidence files of reads 001 to 031 were consulted only to validate quotations and citations, as evidence generated earlier; that was not a new legacy source read. The post-draft tooling is DV-03 and is not presented as authorised.
- Revisions 2 and 3 were made by hand-editing this file and the manifest. Each revision's identity (lines, bytes, SHA-256) was computed with `wc` and `sha256sum` on the local files only, as the correction file's item 6 requires. Text searches of this draft (a `grep` for non-ASCII characters, and for stale wording) were also run. No script, repository or clone check, status command, legacy read or source search was run.
- Independence: every process fact (gate results, counts, hashes, mask counts, preflight results) is Claude's record of its own runs. ChatGPT reviewed the reports and did not re-perform them. Part 1 records an independent review agent; the second- and fourth-checkpoint lists record script tests of their synthetic controls (check_p2_d005.py, check_p3_d005.py).

## 9. Source and evidence register, and open items

Reads (the read number is the evidence ID; R = range read, G = grep read of definition or test-name lines):

| Read | Row | Mode | Lines | Path | Full blob ID | Evidence SHA-256 |
|---|---|---|---|---|---|---|
| 001 | R01a | G | grep | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 924C0F29D48801A6DA74DB0B6AA68EE5CE16FCCEC0C85606982F585BEEE49EF0 |
| 002 | R02a | G | grep | tests/unit/test_candle_math.py | bf47cc393815296c498515616628f5c42f3da85f | 98D00EB1BB6D512706ACB47C9E25BC165EFEEFA71A988EFA4F6514811F7C5BA7 |
| 003 | R03a | G | grep | sr_detector/models.py | 4f49fac5386809539578085fc51dbdee5211da00 | 7034335626360DA8A0E640EC6E584B738D7D6C346CA8579289EF0EEFB4E58124 |
| 004 | R04a | G | grep | data/candle_store.py | f44a831760e62c9d34d4ffc6d420434b0acebf4c | 83855100AC307D340BDE0F760529A615DE5B7599FDCF5181D544AC33511D3899 |
| 005 | R01b | R | 1-2 | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 7DC91CD614E6F99257EEAC3D7B2E7AF25915B769D53FE32835FBA97D8DE01A2D |
| 006 | R01b | R | 28-31 | core/candle_math.py | 57e69c5973ae29a04633bce90537448c7b3778f0 | 8E71815BDF818FA76D6B7EB716A56E8D14A68BA124EADD112A0DA067071A1A7A |
| 007 | R07-01 | R | 1542-1546 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | 79EF57822B75B8AAD7D50EB25D4E003F370CCD99395E0A18F04AAABD15100EF7 |
| 008 | R07-02 | R | 9-13 | core/daily_stats.py | 36fdf6b07b84888915af31bc71dc1077ef1c8167 | 0D62A05485A70A15D2E02619916DE8AF3A1C2CF23B5A93D6B9E1A7B4E9D54A23 |
| 009 | R07-02 | R | 19-23 | core/daily_stats.py | 36fdf6b07b84888915af31bc71dc1077ef1c8167 | 094D3FBD7B3A8866098149A7260FFDFE2F2A2C741910C5F3DCF399E914BCF06C |
| 010 | R07-03 | R | 5-9 | regime/__init__.py | 5522ae3af542f983bda3b45541f90556daa2064e | 91B1651378E8E06DE3068A17D4E8DA929E18AA124C43448374DDBF8F22E49346 |
| 011 | R07-04 | R | 13-17 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | CE79433ECD53AEDC5A898451FA16BC84C35505F668B3D984A780DDAC0F85D290 |
| 012 | R07-04 | R | 24-30 | regime/engine.py | d9cdd9366857163ba5db1358585c57ea49d82c2f | 15644F2C06315C6B9B2F958B0A06943697EC33E987653A7AB871B8502F98B507 |
| 013 | R07-05 | R | 14-18 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | BC20304BB7399C9B2695D19C3D91EF0C08E6423E3FE0709772E0FBE7265CBA1E |
| 014 | R07-05 | R | 118-122 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | 1D4A19AA12E21A576C679F469EE24CED459246622D86A8D1D579771B52A84E2E |
| 015 | R07-05 | R | 152-156 | sr_detector/zone_builder.py | 976721417531869b88502577247f529474cb82ee | ACFA610684C9A5C662580883BC8B21AC7C8113C7C8A136A102E9F0B3EE5FD2A5 |
| 016 | R07-06 | R | 37-41 | v3_chain/pb01_entry.py | cbfd4752525fe1f5d6c5e0959a950ea4a8c303f8 | 2CD6B95A697867D0366D0CC34E3CC584E9004BBBE817F3BCAC7AAD67DBA6ECBD |
| 017 | R07-07 | R | 29-33 | v3_chain/pb01_runner.py | e87a5aa964643f1eae5fad90eeb1e0993b5829e0 | 659F009789872569EBFDBE8BBBC3735847680DD4CCB15495AA2659BE60E6B57F |
| 018 | R07-08 | R | 15-19 | v3_chain/runner.py | a782f91f2f4f1afe12c75072708e2a26316eed3f | F6C5CF849AFF7D3B8E28017A686F3D6CE7051484A4E26A2DB00CEF4F5700208F |
| 019 | R07-08 | R | 25-29 | v3_chain/runner.py | a782f91f2f4f1afe12c75072708e2a26316eed3f | F4C2DC1366F5A620E8EFDD0280F426BE8BC61921B1CF18E5A3D7698E126E949C |
| 020 | R06-01 | R | 382-388 | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 8C597FF5FC3B33EE2CA34104CFA2A5570CD6B0E1DC524B1F908C4285E6119C0A |
| 021 | R06-01 | R | 430-436 | screening/hard_gate.py | a8e7e2e49675ca46989898174adf8e0afe419a61 | 355E617F35C4A08987310EB981F92A2E03F7D1BAB09B87EF6969FB35107CB245 |
| 022 | R06-02 | R | 8-15 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | 15EA69F53740411F47DCD955DC52FE790A703FF0CF5DA1559842B7365EB8B26F |
| 023 | R06-02 | R | 280-288 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | C91F4959233C980ED065BE600671F30072FA621BA97ABB71CF3F77A4DFCBDA18 |
| 024 | R06-04 | R | 156-162 | sr_detector/flags.py | c5ca5b2514d8941a54090685489f13b0a66d5a39 | A1B1475E0FE893461E918793CD11C1227357F08CAE24A5B922631E37BB3334D2 |
| 025 | R06-05 | R | 130-136 | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | 0EBF3315876A1613E826B2DE8D662F0CA2671909F9C56EF99E1E5A36DB7A72AF |
| 026 | R06-06 | R | 1413-1419 | core/config_loader.py | 8635bca77531515561ee48cf92677883a5e30782 | DD4EA11C17249A9F6C11E2E0F5E83B106B6A667A59C3D734B7671DDE79B13AFC |
| 027 | R06-07 | R | 2548-2554 | core/state_store.py | 11866117801b9329773a1be605448faef3c96f8d | 7D49F76C789653E856A0D91B676E7E05A86D6B01BB196D2FBF58EDE37D4EE430 |
| 028 | R06-08 | R | 593-599 | ops_dashboard/backend/services/execution_analytics.py | 5dffbcc2b05cb050281e5bb72ab99d159f66f67f | 1A1AC354EF66AAF4ACC7CB508D615E96D56B84CD3737CD394DC3109F2C21C4C6 |
| 029 | R06-09 | R | 157-163 | scripts/gemini_trade_coach.py | 6d42f71d5d89e6e48c0584eeeb662caf2883421d | 0C3CA4C43F4CF1BA3D18ADC1C62901FE75F9BA1A6CF9FC27007AC5B67C1CD263 |
| 030 | R06b | R | 137-137 | sr_detector/retest_confirm.py | 5fc212535bbd2e86f1c5899d07ce76331ab72724 | AE8EE4FE99D04DB4E3EDAF9FD512375633487B79337C7E97B419B41CCA72806C |
| 031 | R09a | R | 70-70 | orders/structure_exit_manager.py | 6e4c58d8d2e8aa0438a9461067ae070ed1118950 | B53434E8C56FC781945683773780B57F207B8BA0CBF79F46572E34595D020CC5 |

Open items carried from Stage 1 (decision 2):

| ID | Item | Status |
|---|---|---|
| G-01 | the comparison in `_strong_close` | resolved: pos >= frac (read 030) |
| G-02 | the body_frac computation and its edge handling | UNKNOWN / NOT ESTABLISHED; two unread candidate locators only |
| G-03 | the exit module's import of `_strong_close` | a line-level source fact (read 031); execution and use NOT ESTABLISHED |
| G-04 | the true_range_series formula | outside D-005; unresolved |
| G-05 | the atr and resample signatures | unresolved; no read needed now |
| G-06 | whether any path ran | NOT ESTABLISHED; outside source documentation |
| G-07 | the regime/engine.py hits 191, 207, 245 and 340 | UNKNOWN / NOT ESTABLISHED; outside D-005; D5-BK-6 proposed, not stored |
| G-07a, G-08 | the other hits (control status as in section 3) | candidate locators only |

## 10. Status

- FROZEN 2026-10-28 (DR-071) by the storage action that stores this file, after ChatGPT's acceptance of revision 3 as a reviewed draft and under the freeze and storage authorisation named in section 1, as factual legacy documentation only. It documents one individual-candle measure confirmed by its own reads (`_strong_close`), one narrow import fact, and the questions left open; section 6 also cross-references two measures or condition sets recorded in other frozen manuals (HISTORICAL; not re-read or confirmed by D-005). It makes no V2 design, implementation, tuning or production conclusion. At the freeze the open items of section 9 keep their statuses, D5-BK-6 stays proposed and is not stored, and no erratum is made to any frozen manual.
- Stored beside it in docs/agenda/d-005_candle-analysis/: the companion manifest (D-005_manual_manifest.md), the review notes (D-005_review_notes.md) and the freeze records (D-005_FREEZE_PREREQUISITE_BLOCKER_28-OCT-2026.txt, D-005_FREEZE_STORAGE_ACTION_CHANGE_NOTE_28-OCT-2026.txt and D-005_FREEZE_STORAGE_ACTION_FINAL_CHECK_28-OCT-2026.txt).
- Stored unchanged in docs/claude_code_reports/ (OQ-11): Claude's Stage 0 reports and Stage 1 reports, the eight inputs among them.
- Stored unchanged in docs/inputs/: revisions 1 to 3 as reviewed, in docs/inputs/manuals/; the manifests of revisions 1 and 3 and ChatGPT's Stage 0 set, in docs/inputs/chatgpt_originals_and_replies/; ChatGPT's reviews, decisions and prompts, in docs/inputs/reconciliation/.
