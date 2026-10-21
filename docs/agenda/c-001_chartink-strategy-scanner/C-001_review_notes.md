# C-001 CHARTINK STRATEGY / SCANNER: REVIEW NOTES

Date: 2026-10-21 (revision 0; closed at the freeze the same day)
Status: CLOSED 2026-10-21. ChatGPT's independent review of revision 0 (21-Oct-2026) found the package "substantially sound", with no blocker, and asked for no change (section 11); its final consistency review of the freeze package (21-Oct-2026) found no substantive blocker and asked for no edit (section 17). Rama decided on 21-Oct-2026 (section 12): the C-001 legacy manual is FROZEN (revision 0, DR-053) with the reconciliation addendum; errata A-008 E-4 and B-002 E-2 are ACCEPTED (DR-053); the rows that name C-001 have their dispositions (DR-053, scheduling only); the repository stays public, risk accepted (DR-054). The V2 inputs remain PROPOSED / OPEN. Nothing is stored, committed or pushed until Rama says the files are final (his rule of 21-Oct: "None of the files saved into Project or GitHub push till finalised"); Rama stores and pushes the package (section 16); Claude commits nothing.

## 1. Sources
- Inputs from Rama's uploads of 21-Oct-2026, to be stored byte-identical at the freeze (SHA-256; git blob ID):
  - C-001_ANSWER_SHEET_FOR_CLAUDE_21-OCT-2026.txt: ADE2761DD8A45675BC195EDCFE1C92A613E9BC7A4CE9267C4A178B6DC17002F9; 24e836ce4a82b81c26a269c1f82101fc1c4f883b; for docs/inputs/chatgpt_originals_and_replies/
  - C-001_CLAUDE_RESEARCH_PROMPT_21-OCT-2026.txt: 9B29F6742831B1A237C2F9F976CE40068613979A24ADB002C38FF035295B9A95; 448161cee73feac68cfe5043dbd8f096c1725393; for docs/inputs/chatgpt_originals_and_replies/
- Claude's research files, as delivered to Rama on 21-Oct-2026 (his Downloads also hold "(1)" copies; only the files with these values are stored):
  - C-001_CLAUDE_REPOSITORY_ANSWERS_21-OCT-2026.txt: 624569084631797E0FDD85FA665BBAA088D0DEEFD026ACF9E7545F3B24C9B317; 3e8c8b2973e15617c71582c48d4152ab7a8bc4f3; for docs/inputs/chatgpt_originals_and_replies/
  - C-001_SOURCE_INVENTORY_AND_METHOD_21-OCT-2026.txt: 98EE0265B1499C8D250FBD335C9B862748EAFE8CFA199FB7975E0E4AE152AB29; 2ef9b6334bef5ec6ef47e9cc6ee62dec6dfa7765; for docs/inputs/chatgpt_originals_and_replies/
- ChatGPT's three reviews, kept with the topic:
  - CHATGPT_VALUE_ADDED_REVIEW_C-001_21-OCT-2026.txt (of the research files): CE8C8829C289E0580EC5464DAE6D4B3C56204E35AE2C2AD9D919BE0BA0974948; e0f5ecbb0f8f8d68c137a35708b23c232f0d6be7; for the C-001 topic folder
  - CHATGPT_INDEPENDENT_REVIEW_C-001_REV0_PACKAGE_21-OCT-2026.txt (of revision 0): 79177047F0D015A1A965D17E5F6585848D4F5800EB56DB9A9859BEC30C3F8853; bc9d2dc8cadd87e06613e01ae342dfd21b049993; for the C-001 topic folder
  - CHATGPT_FINAL_CONSISTENCY_REVIEW_C-001_FREEZE_PACKAGE_21-OCT-2026.txt (of the freeze package): 433D8CF2CDCD1482D7F863BAD6204FBE81EFAA32D77F65A84B4292E0EDF2DBBB; fb4c36ccdb731016aa0fc231e913016f857ced73; for the C-001 topic folder
- Governance inputs, as uploaded on 21-Oct and byte-identical to V2 main at 5d575d19cd8764a86162648c7697c9bb1146c5ed (21-Oct-2026 12:04 IST; still the head of main when the freeze files were made):
  - V2_DECISION_REGISTER.md: 44733B455BD81F31DD1771A75DFF504DEB12A8D427E0E9EAF0F4F2C5771496F0; 891a925c7285d1c2a53bf7d4e8efcb50f7bbbd41
  - V2_DESIGN_LOG.md: D51B49387C98D2BD03AD2C38C831DDB449F0349C338ECA1403A02DA2CE51BF91; b69b93a5bf019a977235e6777af07dae04b33ec0
  - V2_SESSION_HANDOVER_SECTION_C_21-OCT-2026.md: 11CBD728711873EFFA18F1EE9E10585627C2CAFA54ECAC8A310AC3E4C60CB569; 7dba3bd6d236be20c14ee1b63d3ce8828314bb7c
- V2 main read at 5d575d1 on 21-Oct (a shallow clone of trading-system-v2, read only; nothing written to it) for the frozen manuals of section 4, as the base of every working copy, and for the scan of section 15.
- Rama's answers of 21-Oct-2026 to Claude's questions in this chat. Revision 0: "Claude drafts revision 0 (Recommended)"; the range-breakout alerts ("Yes, Configured", with both webhook URLs); the URL form ("In previous questions shared the chartink link, every strategy name only differs"); the operator check ("Never, ..."); "No browser; record UNKNOWN (Recommended)"; the folder (the docs/agenda/<lower-case ID>_<topic> pattern of a-001_system-purpose: "Follow same"). The freeze: section 12. His screenshots of 21-Oct are not stored (DR-008).
- Legacy source: trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb, read through the masking filter in the research step. Not used: the testing snapshot, the records repository, Chartink.

## 2. Clone and security checks
- Legacy clone at the start of revision 0: HEAD = the pin; never-open blobs present locally 0. Objects 8,922, two more than the 8,920 recorded at the end of the research step (source inventory, section 11): the root .gitignore and ops_dashboard/.gitignore blobs, in two packs written at 14:13:59 and 14:14:00 IST on 21-Oct, after the inventory's count was taken. The command that fetched them is NOT ESTABLISHED; most likely a git command that read ignore rules during the research delivery checks, the mechanism the design log's lesson of 2026-10-19 names (INTERPRETATION). Neither is a never-open path; neither was read; they were identified from the pack indexes and the saved tree listing, without content. The source inventory stays unchanged as an input; this note records the difference.
- Revision 0 and the freeze made no checkout. Their only legacy read is the content re-check of section 10, through the stage-2 filter.
- The research step's process exceptions E-1 (a shared-blob fetch at the first checkout), E-2 (an account tag in working output, lower case) and E-3 (option order) are disclosed in the source inventory, section 4; their lessons are in section 8.
- Chat exposure (21-Oct): Rama's answer about the range-breakout alerts pasted both alerts' webhook URLs, which carry the legacy VM's address and the webhook token. Neither value is reproduced in any file or in Claude's replies. Claude advised treating the token as exposed and never reusing it in V2, and removing the legacy webhook URLs from the Chartink alerts, since the terminated VM's address (DR-012) could be reassigned. ChatGPT's independent review asks that the removal be recorded only after Rama confirms it (section 11); at the freeze his confirmation had not been given, and it is not a condition of the freeze.

## 3. ChatGPT's value-added review: dispositions
All points ACCEPTED. Its conditional request, to amend DR-031 only if its wording were ambiguous, needs no action: Claude's shorthand was the ambiguous text and is corrected, and DR-031 itself is unambiguous (C-001_reconciliation_addendum.md items 1 and 2). No point of the review changes a legacy fact of the research files.

## 4. The frozen-manual check (O-04)
At V2 main 5d575d1: A-011 section 6 and B-007 section 5 (rows C3 and C7) agree with the source; B-002 section 3 (row 17) and A-008 section 8 (the twin-file bullet) did not, and errata B-002 E-2 and A-008 E-4, proposed in revision 0, are ACCEPTED at the freeze (DR-053); A-009 section 3 and A-008 section 8's first bullet are sharpened in the manual without an erratum. Searches and hits: C-001_manual.md section 17; outcomes: C-001_reconciliation_addendum.md section 3.

## 5. A-009 section 3: sharpened, no erratum
- The row names the twin file's reader ("AppConfig; pre-flight scanner check") and its role ("for the pre-flight reachability check"). The reader does receive the file: utils/startup_checks.py lines 680 to 688 is the one place both scanner files are read together. What the reader does with it, a fallback for a map entry without a URL that the schema does not allow, is new detail, now in C-001_manual.md section 6.3.
- A-008 section 8, by contrast, gives a method (HEAD) and a failure class (CRITICAL if unreachable) that the code does not have, so it got an erratum (E-4).
- ChatGPT's independent review, question 3: "ACCEPT SHARPENING; NO ERRATUM NEEDED".

## 6. Points for Rama at the freeze
Decided by Rama on 21-Oct-2026 (section 12). As put to him: (1) the freeze; (2) and (3) each erratum on its own, asked as one question whose four options covered both, either one alone and neither; (4) the dispositions of the rows that name C-001, with Claude's and ChatGPT's suggestion (VP-08 and VP-44 keep C-001 for the Section C end decision paper, because DR-031 places "Which strategies, their scanner conditions (VP-44) ..." with C-001 among others and no other C topic holds that choice; VP-45 drops C-001; Q-20 and Q-34 unchanged) and the alternative (drop C-001 from all three); (5) separately, the repository's visibility (section 15). Storage as practised: the answer sheet, the research prompt and Claude's two research files unchanged in docs/inputs/chatgpt_originals_and_replies/; ChatGPT's two reviews in the topic folder docs/agenda/c-001_chartink-strategy-scanner/.

## 7. The errata
Each working copy starts from the committed file at 5d575d1 and differs from it only by its erratum. Removing the erratum's three insertions (the status-line sentence, the struck-and-replaced passage and the erratum section) restores the committed bytes exactly (section 10).
- A-008_manual.md (E-4): the status line gains one sentence; section 8's twin-file bullet shows "~~for a start-up HTTP HEAD reachability check (CRITICAL if unreachable)~~ (superseded by erratum E-4)" and the replacement; the erratum section "Erratum (2026-10-21, found during C-001)" at the end reads "Status: ACCEPTED" since the freeze.
- B-002_manual.md (E-2): the status line gains one sentence; section 3 row 17 shows "~~skipped on a COLD start~~ (superseded by erratum E-2)" and the replacement; the erratum section at the end reads "Status: ACCEPTED" since the freeze.
- A-008_review_notes.md and B-002_review_notes.md each gain one status sentence at the freeze, as practised at B-007.
- Not changed: either manual's other text. The struck and replacement texts are those of revision 0, which ChatGPT's independent review read.

## 8. Design-log change set (working copy of V2_DESIGN_LOG.md)
- Section 1: six lessons dated 2026-10-21, placed after the last lesson of that date: four from revision 0 (the shared blob, E-1; the lower-case account tag, E-2; quoting a register row; reading the frozen manuals at the research step) and two at the freeze (erratum replacements and review summaries checked as claims, with an independent check before ChatGPT's review; counts taken from one de-duplicated tool run).
- Section 2: Q-20 and Q-34 annotated (tracking only; both stay OPEN, unchanged at the freeze).
- Section 3: VP-08, VP-44 and VP-45 annotated, with their dispositions at the freeze (DR-053, scheduling only): VP-08 and VP-44 keep C-001 for the Section C end decision paper (the Ratify column says so); VP-45 drops C-001 from its Ratify column.
- Section 4: the C-001 row, now FROZEN, placed after the Section B end row and before the configuration design tracking row.
- Section 6: two session notes under 2026-10-21, the first pass and the freeze.
- No other line changed (section 10).

## 9. Questions for ChatGPT's review of revision 0
Seven questions on the labels, the errata, A-009, D-05 and D-06, the V2 inputs and their owners, the dispositions, and any remaining issue. Answered in its independent review (section 11).

## 10. Checks
### 10.1 Independent check (revision 0)
A separate agent that had not seen the drafting compared the four C-001 files and both erratum texts with the research files, ChatGPT's review, Rama's answers and the frozen manuals and register at 5d575d1 (read-only; it read nothing of the legacy clone). It confirmed the line ranges (all citations match the answers file's claims; the one new range as declared), every quote, the erratum strings and the byte-exact restoration of both frozen files, and found 20 points; none was a wrong number, count, date or range. Applied:
- Erratum A-008 E-4's replacement said the twin file "is read only for an entry without one"; the file is loaded, hashed and presence-checked at every start, and only its URL is used solely for an entry without one. Reworded in the manual, the working copy and its erratum section.
- The addendum had recorded ChatGPT's review as calling DR-031 itself ambiguous; the review called the shorthand ambiguous and asked for a confirmation. Reworded (addendum items 1.2 and 4; section 3 above; the design-log lesson).
- Unscoped or over-broad statements bounded: no scan conditions "found in S-CODE" (F-04 scoped, addendum 2.6); "no code that pulled scan results was found"; arrivals after 31-Jul UNKNOWN; the twin file's URLs, not the file, had no effect identified; "every start" stated as the baseline's code path, with the records that show CRASH; the reading of R-01 limited to POSTs that reach the receiver, and its explanations given as an open list that includes alerts triggered but not delivered; "He read them from his Chartink pages" replaced by his own words; the O-02 answer's reading labelled INTERPRETATION; the duplicate-key collapse stated as what would have happened; the exchange "appears" fixed in code.
- Citations and attributions: WARM attributed to A-011 only; the whole start order to A-011 section 5; the missing-file row corrected (addendum 2.7); D-02's documents described one by one; the R-03 wording of rows 13 to 15; D-05's owners widened, noted in addendum 2.5; the frozen-manual search's hits listed exactly (manual section 17).
- Two-layer rule: the six-strategy target and the paper-mode sentence removed from the manual; the V2 inputs carry the target.
- Kept as it is: the Scope line's "(DR-016; V2_DESIGN_LOG W-1)", the wording of the frozen B-002 and B-009 manuals for the same sentence (DR-016 is the audit-then-design row, W-1 the two-layer rule); the V2 inputs now cite DR-031 and W-1 for "no legacy strategy is a candidate by default". Rama's folder answer ("Follow same") was given in this chat on 21-Oct; the agent's brief did not include it, so it could not find it.

### 10.2 Results on the revision-0 files
- check_md.py: OK on the seven Markdown files. No carriage return; the four C-001 files ASCII only; the working copies kept their committed counts of lines with non-ASCII characters (A-008 6, B-002 13, the design log 9).
- cite_check.py: 325 citations in the four C-001 files (324 in the manual, 1 in these notes), in 58 files, 0 out of range, 0 unattributed; on the lines added to the working copies, 10 citations, 0 out of range. Unresolved path tokens: the four C-001 file names and the helper names check_md.py, mask_read2.py and scan_ids.py only.
- Against the research files: 256 distinct citations; all but one are cited in the answers file for the same claims (confirmed by the independent check). The new one is the twin file's range of its 16 entry lines, inside the twin-file range the answers file cites.
- Content re-check through the stage-2 filter: 18 of 18, covering the new range, the errata's evidence and the start-up check's scenario path; nothing printed. One expression first failed because the filter masks the type name on the chartink_url line of ScannerEntry; written for the masked form, it passed.
- Quotes: 22 quoted passages of the register, the design log, the frozen manuals and ChatGPT's review found verbatim at 5d575d1, each in the section cited; B-007 section 5 row C1 read for the missing-file correction.
- Errata: in each working copy the struck text, the replacement and the unchanged texts before and after it equalled the strings in manual section 17; with each erratum's insertions removed, both manuals equalled their committed bytes, and so did the design log without its revision-0 additions.
- scan_ids.py: 0 hits in the manual, the addendum and the V2 inputs; the hits in these notes were the hashes and blob IDs of section 1 (long_token). The working copies held the same counts as their committed files (A-008 9, B-002 1, the design log 16). Lower-case identifier shapes: 0 non-path hits in the four C-001 files; the working copies at their committed counts (9, 1 and 18). Session links, the session-link text that DR-038 describes, the push checks' credential pattern, and address and mailbox shapes: none.
- Owners and IDs: every topic ID used is in the Master Agenda; every VP and Q ID exists in the design log; the agenda table runs A-001 to A-011, Section A end, B-001 to B-012, Section B end, C-001, Configuration design. The manual's 16 catalogue rows equal the answers file's catalogue.

### 10.3 Results on the freeze files
Run on the exact files of section 16 after the freeze edits; PART 1 of the push commands repeats the hash, scan and change checks on Rama's PC.
- check_md.py: OK on the ten Markdown files of the package. No carriage return in any of them; the four C-001 files are ASCII only; the six modified files keep their committed counts of lines with non-ASCII characters.
- Restoration: removing each freeze addition restores the committed bytes of all six modified files: the two manuals without their erratum's three insertions, the two review notes without their one sentence, the register without DR-053 and DR-054, and the design log without its nine added lines and five annotations. The struck, replacement and unchanged texts of both errata equal those in manual section 17.
- Citations: unchanged from revision 0 (the same 256 distinct citations and the one new range of 10.2; none out of range; the errata's 10 citations on the added lines all in range). The content re-check through the stage-2 filter passed again, 18 of 18.
- scan_ids.py: 0 hits in the manual, the addendum and the V2 inputs; in these notes long tokens only (the hashes and blob IDs of sections 1 and 16, and the main commit ID); the six modified files hold the same counts as their committed versions. Lower-case identifier shapes: unchanged counts. Session links: none (the register's DR-038 text, unchanged, describes the link and is not one). The push checks' credential pattern: none. Address and mailbox shapes: none.
- Owners and IDs: every topic ID is in the Master Agenda, with the titles of section 14; every VP and Q ID exists in the design log; the Ratify cells of VP-08, VP-44 and VP-45 are as decided; every status line in the four C-001 files is as set at the freeze; the agenda table is in order.
- V2 main was still at 5d575d1 on GitHub when the freeze files were made.

## 11. ChatGPT's independent review of revision 0 (21-Oct-2026)
CHATGPT_INDEPENDENT_REVIEW_C-001_REV0_PACKAGE_21-OCT-2026.txt ("Recommendations only"): "The C-001 revision-0 package is substantially sound and suitable for Rama's explicit freeze decisions". Its answers to the questions of section 9:
1. Labels: "ACCEPT". HISTORICAL (Rama) for the 21-Oct answers; INTERPRETATION / AUDIT LEAD for reading "every strategy name only differs" as the webhook paths; configured kept apart from active or armed throughout 12-Jun to 31-Jul and from having delivered a POST; negatives scoped ("no scan conditions found in S-CODE"). The files already say so; no change.
2. Errata: "RECOMMEND ACCEPTING BOTH, INDEPENDENTLY", with the procedure for acceptance or rejection. Rama accepted both (section 12).
3. A-009 section 3: "ACCEPT SHARPENING; NO ERRATUM NEEDED".
4. D-05 and D-06: "ACCEPT", with the owners C-007, C-008, C-010 and K-003 (D-05) and L-009 and L-010 (D-06), as in manual section 13.
5. V2 inputs and owner cells: "ACCEPT WITH FINAL AGENDA CHECK": each owner cell compared with the Master Agenda's exact topic title. Done in section 14, with three owner-text refinements.
6. Dispositions: recommended as Claude suggested; decided by Rama (section 12).
7. "NO BLOCKER FOUND IN THE PROVIDED PACKAGE, SUBJECT TO FINAL DIFF CHECK". The full diff of the freeze files against main is delivered with this package.
- Security: treat the token as exposed; remove or disable the legacy webhook URLs in the Chartink alerts; record that only after Rama confirms it (section 2).
- Its finalisation checklist: the freeze and each erratum decided (section 12); the dispositions decided (section 12); owner cells against the Master Agenda (section 14); the files reconciled to the decisions (section 13); exact-diff and byte-restoration checks rerun, and the Markdown, citation, ID, owner and secret checks on the exact final files (section 10.3); files kept out of the Project and GitHub until Rama says they are final; the Chartink clean-up kept out of the permanent record until confirmed.
- Its decision boundary: "After Rama decides, Claude should update the final working files and show a complete diff; ChatGPT can then perform a final consistency review." The freeze package was that step, with its full diff; the final consistency review is section 17.

## 12. Rama's decisions at the freeze (21-Oct-2026)
His answers to Claude's four questions of 21-Oct (about 16:15 IST):
- The freeze: "Freeze C-001 now (Recommended)".
- The errata: "Accept both (Recommended)" (A-008 E-4 and B-002 E-2, each accepted on its own; the options also offered either one alone and neither).
- The rows that name C-001: "Keep for Section C end (Recommended)": VP-08 and VP-44 keep C-001 for the Section C end decision paper; VP-45 drops C-001; Q-20 and Q-34 unchanged.
- The repository: "Keep it public; risk accepted" (section 15).
Recorded in DR-053 (the freeze, the errata, the dispositions) and DR-054 (the visibility).

## 13. Freeze edits
- C-001_manual.md: the status and date lines; section 6.3's errata sentence; section 17's heading and its two status bullets; section 20's last item. No finding changed.
- C-001_reconciliation_addendum.md: the status line; section 5; section 6 added (ChatGPT's independent review).
- C-001_v2_design_inputs.md: the status line; the owner cells of rows 2, 4 and 7 (owner text only; section 14); section 3's disposition column and closing paragraph.
- C-001_review_notes.md: these notes (status; sections 1, 2, 4 to 9 brought up to date; sections 10.3 and 11 to 16 added).
- A-008_manual.md and B-002_manual.md: the status sentence and the erratum section now read ACCEPTED; the struck and replacement texts are unchanged from revision 0.
- A-008_review_notes.md and B-002_review_notes.md: one status sentence each.
- V2_DECISION_REGISTER.md: DR-053 and DR-054 appended.
- V2_DESIGN_LOG.md: section 8.

## 14. Owner cells against the Master Agenda (ChatGPT's question 5)
Each owner in C-001_v2_design_inputs.md section 2 and C-001_manual.md section 13, with the Master Agenda's topic title (or the VP row's subject) and why it holds the question. Three cells changed at the freeze, owner text only.

| Row | Owner cell | Titles | Fit |
|---|---|---|---|
| 1 | VP-45; C-002, C-010; configuration design | VP-45 Chartink identity; C-002 Scanner -> webhook; C-010 Scanner -> strategy mapping; the configuration design tracking row | The single identity field and what is checked against it |
| 2 | VP-45, VP-47; configuration design; N-002 | VP-47 boot gate on configuration; N-002 Integration testing | A gate on scanner keys, shown to run at the production call site. Changed from N-001 (Unit testing): the legacy gap was a check that tests ran and no production caller did |
| 3 | VP-45; C-002; configuration design | C-002 Scanner -> webhook | Whether screener addresses are kept and verified |
| 4 | C-002; L-003, L-004, L-007, L-010 | L-003 Critical alerts; L-004 Warning alerts; L-007 Metrics; L-010 Dashboard | A per-scanner arrival watch and who is told. VP-11 removed: it covers the outcome of signals received, not missing arrivals; L-007 added for per-scanner counts |
| 5 | M-006; VP-47 | M-006 health checks | Unattended scanner checks and their failure action |
| 6 | C-007, C-010; K-003; L-010 | C-007 Signal persistence; K-003 Signal state; L-010 Dashboard | The stored identity and what attribution uses |
| 7 | VP-11, VP-59; C-006 to C-009; K-008, L-009 | VP-11 terminal outcome of every received signal; VP-59 signals across stop, kill and crash; C-006 Queue to C-009 Signal expiry; K-008 Audit state; L-009 Audit trail | Every received symbol's outcome. K-008 added: the legacy counts lived in an audit table |
| 8 | VP-45; C-002, C-004 | C-004 Validation | The alert-name check |
| 9 | C-010; VP-08 | C-010 Scanner -> strategy mapping; VP-08 strategy model | Direction kept in the strategy definition |
| 10 | Q-20; C-004 | C-004 Validation | The symbol and exchange at the edge |
| 11 | C-002, C-003 | C-003 Authentication | The transport and where the shared secret travels |
| 12 | L-010; configuration design (VP-15) | L-010 Dashboard; VP-15 configuration | The GUI showing the configuration the process loaded |
| Manual section 13 | D-05: C-007, C-008, C-010, K-003; D-06: L-009, L-010 | C-008 Signal processing, with the titles above | Where the stored and counted identity is used later |

## 15. Repository visibility (DR-054)
- Rama made trading-system-v2 public on 21-Oct-2026, "till production starts", so that Claude can verify files before and after they are stored. The session also reads it through its own GitHub access, so the visibility is not needed for that.
- Claude scanned every tracked text file at 5d575d1 (233 files) value-blind, printing kinds, files and counts only:
  - both legacy Zerodha client IDs: 19 occurrences in any letter case, in 9 files, all under docs/inputs/ (discussion checklists and answer sheets with Rama's answers, ChatGPT's drafts and reconciliations that quote them, and the storage map);
  - both legacy VMs' public addresses: the storage map's two VM rows (docs/inputs/legacy_baseline/WHERE_EVERYTHING_IS_STORED_18-Sep-2026.txt);
  - a legacy SSH host alias, five times in two discussion checklists in docs/inputs/chatgpt_originals_and_replies/;
  - otherwise only benign shapes: section numbers of the YAML specification and loopback addresses (address shapes), the commit-attribution address (mailbox shapes), hashes and blob IDs (long tokens), technical terms and row IDs (identifier shapes), a prose bullet of the B-004 manual on the API secret (the words that follow it are "start-up check") and the known B-004 answer-sheet line (credential shapes); no session link, key file, fingerprint, chat ID or tailnet name.
- docs/inputs/ is never edited (W-3), and an edit would not remove a value from the history.
- Rama's decision: "Keep it public; risk accepted", recorded in DR-054 with no value. Both legacy VMs are terminated and the legacy repositories stay public (DR-012); the client IDs were already published there (A-008 erratum E-2; C-001 source inventory, exception E-2).
- Correction: Claude's chat message said the two client IDs appear "18 times across 10 files"; one de-duplicated tool run gives 19 occurrences, one of them in lower case, in 9 files (the two IDs share files). The SSH host alias was found after Rama answered and is reported to him with this package. Lesson in section 8.

## 16. Closed package manifest
Seventeen files: the sixteen of the freeze package that ChatGPT's final consistency review read, and that review itself (row 17), stored in the topic folder as W-3 requires. The list is closed: nothing outside it is stored. SHA-256 values are fixed here for the seven files stored unchanged; the other ten are computed from their final bytes and given with the push commands, together with every file's git blob ID.

| # | Path | Kind | SHA-256 |
|---|---|---|---|
| 1 | docs/agenda/c-001_chartink-strategy-scanner/C-001_manual.md | New | From the final bytes |
| 2 | docs/agenda/c-001_chartink-strategy-scanner/C-001_reconciliation_addendum.md | New | From the final bytes |
| 3 | docs/agenda/c-001_chartink-strategy-scanner/C-001_v2_design_inputs.md | New | From the final bytes |
| 4 | docs/agenda/c-001_chartink-strategy-scanner/C-001_review_notes.md | New | From the final bytes |
| 5 | docs/agenda/c-001_chartink-strategy-scanner/CHATGPT_VALUE_ADDED_REVIEW_C-001_21-OCT-2026.txt | New, stored unchanged | CE8C8829C289E0580EC5464DAE6D4B3C56204E35AE2C2AD9D919BE0BA0974948 |
| 6 | docs/agenda/c-001_chartink-strategy-scanner/CHATGPT_INDEPENDENT_REVIEW_C-001_REV0_PACKAGE_21-OCT-2026.txt | New, stored unchanged | 79177047F0D015A1A965D17E5F6585848D4F5800EB56DB9A9859BEC30C3F8853 |
| 7 | docs/inputs/chatgpt_originals_and_replies/C-001_ANSWER_SHEET_FOR_CLAUDE_21-OCT-2026.txt | New, stored unchanged | ADE2761DD8A45675BC195EDCFE1C92A613E9BC7A4CE9267C4A178B6DC17002F9 |
| 8 | docs/inputs/chatgpt_originals_and_replies/C-001_CLAUDE_RESEARCH_PROMPT_21-OCT-2026.txt | New, stored unchanged | 9B29F6742831B1A237C2F9F976CE40068613979A24ADB002C38FF035295B9A95 |
| 9 | docs/inputs/chatgpt_originals_and_replies/C-001_CLAUDE_REPOSITORY_ANSWERS_21-OCT-2026.txt | New, stored unchanged | 624569084631797E0FDD85FA665BBAA088D0DEEFD026ACF9E7545F3B24C9B317 |
| 10 | docs/inputs/chatgpt_originals_and_replies/C-001_SOURCE_INVENTORY_AND_METHOD_21-OCT-2026.txt | New, stored unchanged | 98EE0265B1499C8D250FBD335C9B862748EAFE8CFA199FB7975E0E4AE152AB29 |
| 11 | docs/agenda/a-008_strategy-model/A-008_manual.md | Modified (erratum E-4) | From the final bytes |
| 12 | docs/agenda/a-008_strategy-model/A-008_review_notes.md | Modified (one status sentence) | From the final bytes |
| 13 | docs/agenda/b-002_configuration-loading/B-002_manual.md | Modified (erratum E-2) | From the final bytes |
| 14 | docs/agenda/b-002_configuration-loading/B-002_review_notes.md | Modified (one status sentence) | From the final bytes |
| 15 | docs/governance/V2_DECISION_REGISTER.md | Modified (DR-053, DR-054) | From the final bytes |
| 16 | docs/governance/V2_DESIGN_LOG.md | Modified (section 8) | From the final bytes |
| 17 | docs/agenda/c-001_chartink-strategy-scanner/CHATGPT_FINAL_CONSISTENCY_REVIEW_C-001_FREEZE_PACKAGE_21-OCT-2026.txt | New, stored unchanged | 433D8CF2CDCD1482D7F863BAD6204FBE81EFAA32D77F65A84B4292E0EDF2DBBB |

- Base: main at 5d575d19cd8764a86162648c7697c9bb1146c5ed. The push commands check that HEAD and origin/main still equal it.
- Next: the push commands (PART 0, 1 and 2) and the SAVE list (W-2), given after ChatGPT's final consistency review (section 17). Rama runs PART 0 and PART 1 and sends their output; he runs PART 2 only after PART 1 has passed and he declares the package final; Claude then confirms the commit against the remote.

## 17. ChatGPT's final consistency review of the freeze package (21-Oct-2026)
CHATGPT_FINAL_CONSISTENCY_REVIEW_C-001_FREEZE_PACKAGE_21-OCT-2026.txt: "No substantive blocker identified in the package as presented." It read the sixteen files and the full diff, found the files aligned with DR-053 and DR-054, and concluded: "No additional documentation edit is required based on the reviewed material." It did not reproduce the local bytes or run the push commands, and asked that they be treated as required local checks.
- Its checklist and where each item is met: PART 0, PART 1 and PART 2 and the SAVE list (the push commands); the local and remote base equal to 5d575d1 (PART 1 step 1 and PART 2 steps A and D); exactly the intended paths and no extras (PART 1 steps 3 and 6; seventeen, as below); the hashes of the files stored unchanged rechecked and the other values computed from the final bytes (PART 0 and PART 1 step 2, with the values of the push commands); the full diff inspected (the freeze package's diff file, and the diff of the three record updates below); every check on the exact final files (PART 1 steps 2 to 6); nothing stored until Rama declares the package final; the Chartink clean-up recorded only after Rama confirms it (section 2: not yet confirmed; not a condition of the freeze).
- Changes after it, record-keeping only: the review itself is stored in the topic folder (W-3), which makes the package seventeen files; these notes name it (the status line, sections 1, 11, 16 and this section), as do DR-053 (where the reviews are stored, and one sentence in its reason) and the design log's C-001 agenda row and freeze session note. No finding, erratum, disposition or V2 input changed.
