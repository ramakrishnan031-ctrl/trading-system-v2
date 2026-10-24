# V2 SESSION HANDOVER: C-001 TO C-008 FROZEN, C-009 NEXT

Date: 2026-10-24 (IST)
Purpose: let a new chat in the "Trading System V2 - Design & Build" project continue Section C from C-009 (then C-010, C-011 and the Section C end) with the method practised at C-005 to C-008. This note is a working aid, not a decision record: decisions live in docs/governance/V2_DECISION_REGISTER.md, rules and lessons in docs/governance/V2_DESIGN_LOG.md. Where this note and those files differ, those files win.
Security: no credential values, account or client IDs, host or VM addresses, deployed paths, mailbox addresses or person names other than Rama's.
Builds on V2_SESSION_HANDOVER_SECTION_C_21-OCT-2026.md (Project doc; also docs/governance/ in the repository). Its section 4 (security), section 5 (reading the legacy source safely) and section 8 (helper scripts, exact copies) still apply and are not repeated here; its section 2 (topic workflow) and section 7 (push commands) are superseded by sections 2 and 7 below.

## 1. State at handover

- Repository ramakrishnan031-ctrl/trading-system-v2, branch main, public until production starts (DR-054). main = b720bd7635ca448fa35c828cfd72bfa722b0e24e (C-008 push by Rama, commit time 10:03 IST on 24-Oct-2026), parent dce31648fb06ef4a52bbef4508fcabfc99b38748, tree 793b8f9451dd2756b958b6db7aed7aa421bb1709. This is the C-009 base: check it with git ls-remote before anything else.
- Section A and Section B complete (DR-013 to DR-052). Section C: C-001 FROZEN (DR-053), C-002 (DR-055), C-003 (DR-056), C-004 (DR-057), C-005 (DR-058), C-006 (DR-059), C-007 (DR-060), C-008 Signal processing (DR-061, with erratum B-009 E-3); DR-054 is the repository-visibility row. Next in agenda order: C-009 Signal expiry, C-010 Scanner -> strategy mapping, C-011 EOD / Daily signal routing, then the Section C end.
- Last numbers used: DR-061, VP-68, Q-46, BK-43. Next: DR-062, Q-47, VP-69, BK-44.
- Open questions: Q-01, Q-03 to Q-06, Q-11 to Q-13, Q-14 (non-blocking), Q-15 (PARTLY ANSWERED), Q-20, Q-21, Q-23, Q-25, Q-26, Q-30, Q-31, Q-34 to Q-38, Q-40 to Q-46; Q-02 NOTED. Q-30, Q-31 and Q-34 to Q-45 are DR-016 audit leads.
- Proposals whose Ratify-at cell names a remaining Section C topic: VP-24 (C-009, C-008; links VP-11), VP-59 (C-006 to C-009, I-009), VP-45 (C-010, configuration design), VP-14 (C-011). Earlier topics did not ratify their VPs (VP-12 at C-005, VP-27 at C-004, VP-59 at C-006 to C-008, VP-11/24/31/59 at C-008): each was left for the whole-design review or a later decision, its cell unchanged, by the topic's authorisation or Rama's choice. Ask, do not assume.
- Bookmarks naming a remaining topic: BK-17 (text drift incl. the expiry default 60 vs 600), BK-18 (VP-59), BK-23 (expires_at stored from received_at while both age checks measure from triggered_at) for C-009; BK-27 (strategy column stores the scanner name) for C-010; BK-08 and BK-30 (the EOD route) for C-011. BK-43 holds C-008's OQ-01 (agenda ownership of the entry throttle's rule and the shadow tracker), OPEN for the master agenda review. All bookmarks BK-01 to BK-43 are OPEN.
- Owed at the C-009 package (record or store, then strike from this list):
  1. In V2_DESIGN_LOG section 6, under the date of the C-009 package, this line verbatim:
     "- C-008 package pushed by Rama (b720bd7, 28 files, commit time 10:03 IST): PART 0, PART 1 and PART 2 passed at the first run, PART 2 after ChatGPT's check of the package; confirmed independently by Claude at 10:04 IST on a fresh clone: GitHub main b720bd7 on parent dce3164, the 28 files equal to the manifest blob by blob, the tree equal to the rehearsal's, the commit message the subject and the Co-Authored-By line only."
  2. ChatGPT's check of the C-008 freeze package, C-008_PART1_REVIEW_AND_PART2_AUTHORIZATION_24-OCT-2026.txt (23 lines, SHA-256 D018A689D0477E35136372E60C26F9C914C211237412ECBA436BC0EEFCEE5E51; "APPROVED TO RUN PART 2"; it accepted the flagged B-009 review-notes sentence), stored unchanged in docs/inputs/reconciliation/ (W-3). Rama attaches it in the new chat; check the SHA-256.
  3. This note, stored unchanged in docs/governance/ with the C-009 package (as the Section B and C handovers were), with a session-note line saying so.

## 2. How each topic runs (as practised at C-005 to C-008; W-9)

1. Rama uploads ChatGPT's set with names unchanged (DR-008): the Claude Stage 0 research prompt and the Stage 0 answer sheet.
2. Claude: a fresh clone of V2 main (check the base); read V2_DESIGN_LOG section 1 (lessons) and section 5 (W-1 to W-9); set up the tools and the legacy clone (section 5 below); sensitive_check.py must print "never-open blobs present locally: 0".
3. Stage 0, inventory and plan only. The read tool is LOCKED (STAGE1_AUTHORISED = False): every read is refused. Allowed: path work on the saved listing, count-only searches (per-file counts, no line printed) and line counts as metadata. Deliver <ID>_CLAUDE_STAGE0_ANSWERS_<DD-MON-YYYY>.txt and <ID>_CLAUDE_STAGE0_SOURCE_INVENTORY_AND_METHOD_<DD-MON-YYYY>.txt: baseline checks, a register of candidate files (YES / SUPPORTING / CONDITIONAL / NO, with blob IDs and line counts), every search with its scope, a scope proposal, the stop conditions met, open points. Stop for ChatGPT.
4. ChatGPT's Stage 0 review and Stage 1 authorisation (scope, owners of neighbouring rules, records rules, VP rows not to ratify). Then, as a step of its own: lift the lock (one edit; record the tool's SHA-256 before and after, and the time), run the gate test, record both.
5. Stage 1: read only through the gated reader (cread): every read logged (ID, IST time, mode, args, path, pinned blob, blob match, line total, lines printed, masks, never-open 0, self-test ok, evidence hash) and its output saved as evidence/NNN.txt. Minimum ranges; a callee read where its result matters; tests as text only. Count-only searches through ccount, each zero with a positive control (same tool, flags and file list). Any file outside the authorised inventory: stop or disclose as a deviation.
6. Revision 0: the manual draft (<ID>_<TITLE>_MANUAL_DRAFT_<date>.md: evidence rules and labels, a sources table with pinned blob, lines, reads and role, process notes, stage and trace tables, outcomes, failures, owners elsewhere, reconciliation with the frozen manuals, new bookmarks C9/BM-nn, open questions and unknowns, known limits, an end-to-end path, the final position, a revisions section) and its companion inventory (safety declaration, read log, searches, claim index: every claim ID to its citations and the reads that printed them, final checks). Built by script, with checks: every `path:lines` citation printed by a logged read at its pinned blob; backticked paths in the listing; hex IDs known; ASCII; no CR, session link, absolute path or credential pattern. An independent check by a separate agent on a snapshot is optional (C-008 did one; record how many findings and that the fixes were not re-checked if they were not).
7. Deliver for ChatGPT's review. Each revision gets its own file names (earlier files stay unchanged as delivered) and ends with "Changes for re-review" listing exactly what changed. Rama's decisions are asked as multiple-choice questions, the recommended option first with "(Recommended)".
8. W-4 (DR-017): Rama approves the exact release candidate (each file's name and SHA-256). Ask him then whether ChatGPT should check the freeze package before PART 2 (C-008: yes).
9. Freeze package (section 6 lists the parts). Rama runs PART 0 and PART 1 (safe; no commit), ChatGPT checks if Rama chose that, then PART 2 in the same window. Claude confirms on a fresh clone and records the push line for the next package.
Section C end, after C-011: a decision paper in the form of docs/agenda/a-section-end and b-section-end (ChatGPT's prompt and answer sheet answered by Claude; revisions after ChatGPT's reviews; Rama's answers into DR rows; scheduling moves recorded one by one as "scheduling only"). It is built from the repository, not from chat history: the frozen C-001 to C-011 manuals and review notes, the V2 inputs files (C-001, C-002, C-004 to C-007), the VP rows naming Section C topics (VP-08 and VP-44 kept for the Section C end by DR-053; VP-11, VP-12, VP-14, VP-24, VP-27, VP-31, VP-45, VP-59), the open questions (Q-20, Q-46) and the bookmarks.

## 3. Standing rules added or confirmed since 21-Oct

- W-9 (Rama, 23-Oct): lean topics; contradictions and cross-topic points are bookmarked, not solved; the whole proposed design after the agenda.
- Errata to frozen manuals: proposed with exact texts (struck text, replacement, unchanged text on each side), decided by Rama, applied only at the freeze after the number and anchors are rechecked against live main and the register; the struck text stays visible; one status sentence goes in the target manual's status line and one in its review notes. Put the review-notes sentence among the approved texts from the start (at C-008 it was not, and Rama approved it separately, flagged for ChatGPT).
- The records repository (trading-system-records) and the testing snapshot are outside the source boundary unless Rama explicitly instructs otherwise (C-008 decision D-7: the two 18-Sep chapters only). LEGACY_SYSTEM_MANUAL_INDEX.md and the 18-Sep manual's chapters hold a production host's address, PC paths, a deployed directory, loopback addresses and a user name: never stored in V2 and never quoted for those values; cite them by SHA-256 and blob ID. The index's chapters 01 to 03 are WRITTEN, the rest NOT STARTED; check at Stage 0 whether a written chapter overlaps the topic (C-008 lesson).
- No V2 design-inputs file unless the topic's V2 points are too large for design-log rows (W-3); C-008 had none (Rama: "bookmarks only"). Ask at the freeze if unsure.
- C-008 lessons (V2_DESIGN_LOG section 1, 24-Oct): ask Rama for any input an authorisation names before the first read; take every site of a counter or status write from a search of the name, not from reads made for other questions; record each fetch from another repository (command, time, objects) in the topic notes when it is run; for "counts toward" statements cite the counter's own rule.
- Commit attribution (W-8): every commit message is the subject line, a blank line and "Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>" only. No Claude session link anywhere, even if a chat's default instructions add one. Claude never commits or pushes; Rama does.
- Rama's PowerShell outputs can show his PC user name in the prompt: never reproduce it. The memory profile holds an account ID, a VM address and a VM path: never reproduce them.
- File-save rule (W-2): tell Rama where files go only once they are final (the push commands place them).

## 4. Security constraints (verbatim as carried through C-005 to C-008; no exceptions)

- "Never open, fetch, read, or print: .env or .env.*; config/accounts.csv or config/accounts_multi_example.csv; config/security.yaml; config/reference_data/; docs/web_claude/ anywhere; token/session files; gui_config.local.yaml; database files (*.db, *.sqlite, *.sqlite3, WAL/SHM); logs"
- "Do not expose credential values, access tokens, TOTP seeds, webhook secrets, account/client identifiers, hostnames/IP addresses, mailbox addresses, person names, bot/SSH key names, or PC username/hostname."
- "Do not contact live services, VMs, broker APIs, databases, webhooks, or external accounts."
- "Never run a tree-wide or history command on the partial clone"
- "Read code and records only through mask_read2.py, options before the file names"
- "Before sending, scan every output with scan_ids.py (value-blind) and check that no session link is present."
- "Claude never commits or pushes; Rama does."
- "None of the files saved into Project or GitHub push till finalised"
- "Never print or reproduce actual passwords, API secrets, TOTP secrets, tokens, webhook secrets, full webhook URLs containing secrets, client/account IDs, VM addresses, hostnames, SSH aliases, key paths or fingerprints."
- "Treat that token as exposed; do not copy it into research notes or outputs. Do not attempt to visit the exposed URL or send test requests."
- "Do not access Chartink UI or attempt to verify/update alerts."
- "Never run the legacy service, invoke the webhook against any real endpoint, place broker/API calls, or run tests that could trigger external effects."
- "Do not expose a value merely because a repository is public."
- Host records (deployment files, cron tables, service units, system maps) are read value-blind only: counts and matching line numbers, never whole lines (C-003 and C-007 lessons). The two July 2026 queue records (docs/audit/queue_items_decidable_25jul2026.md, docs/audit/stale_queue_entries_25jul2026.md) stay excluded unless separately approved.
- Known benign scan hits: hashes and blob IDs (long_token); "SHA256"; the attribution address; short read IDs such as X001; the "A...(len 4)" and "B...(len 4)" shapes already in the register and design log at main; the "Claude-Session" text on register line 49 (a description, not a link; PART 1's link pattern does not match it); wrapped lists of read IDs that match the cron-line shape. Compare any other hit with main before sending.

## 5. Tools in a new session (the container is new: nothing from the C-008 workspace survives)

1. Helper scripts: the exact copies in section 8 of V2_SESSION_HANDOVER_SECTION_C_21-OCT-2026.md (mask_read_lib.py, mask_read2.py, sensitive_check.py, scan_ids.py, check_md.py, cite_check.py) into /home/claude/tools/. Checked on 24-Oct-2026: identical to the copies used through C-008.
2. The legacy clone, as section 5 of that note: blob-less, no checkout, sparse with anchored paths, at 970aabf3c78d777d9d34549e021f7c8e756c7bdb; save the listing once (git ls-tree -r HEAD > /home/claude/legacy/prod_lstree.txt: 1,438 entries at the pin, SHA-256 1F195D3D516AB4F194BCB7AF105FB193A606EE241E2E1437102EF495D8E647A5; 102 never-open paths); add each file to the sparse set before reading it (the gate refuses a working file that differs from its pinned blob). Expected after each step: sensitive_check.py exit 0, "never-open blobs present locally: 0".
3. The gate tools of C-008 (exact copies in section 8 below; only the scratchpad path is shown as <SCRATCHPAD>): <SCRATCHPAD>/bin/mr.py and <SCRATCHPAD>/bin/count_grep.py; cread_c008.py, ccount_c008.py, clines_c008.py and gate_test_c008_s1.py. For C-009 copy them as *_c009.py: set S to the new scratchpad, change the c008 log and evidence folders to c009, set STAGE1_AUTHORISED = False for Stage 0 (lift it only after the Stage 1 authorisation, as step 4 of section 2), keep the two excluded July records, and regenerate the gate test's reference list of protected paths (it read c006/clar/protected_paths.txt: the 102 never-open paths of the listing).
4. Count-only search scopes: scope_rules.py (section 8) rebuilds s_all 1,260, s_rt 281, s_code 869, s_tests 486 and s_cfg 41 from the listing, identical to the lists used at C-005 to C-008.
5. Freeze tools (rewrite per topic; responsibilities as used at C-008): a manual freeze builder (the approved file checked by SHA-256, a fixed list of edits each matching once, then checks that citations, claim IDs and evidence labels are unchanged, and a diff); an erratum applier (texts parsed from the frozen manual's erratum section and compared with the approved revision; anchors at stated lines); a governance builder (register append-only, one row; design-log anchors each matching once); an input copier (each source checked by SHA-256; refuses a path already in the base commit); a push-commands generator (section 7); a deterministic zip builder (sorted entries, fixed time stamps, read back); a value-blind final scan; an insertions-only diff check on the changed base files; a check sheet for ChatGPT (hashes, manifest, diffs); a rehearsal script.
6. PowerShell 7 for the rehearsal: the linux-x64 tarball from the PowerShell GitHub releases into the scratchpad (7.4.6 was used). The rehearsal runs PART 0, 1 and 2 verbatim in one session against local stand-ins (a bare copy of main as origin, no network) in three Downloads layouts: the zip with a " (1)" copy, loose delivered files and an older package unzipped beside it; the zip's contents unzipped straight into Downloads with same-name decoys; and an "Extract All" folder. All three must pass with the same tree.

## 6. The freeze package (as at C-008: 28 files)

- docs/agenda/<topic folder>/: <ID>_manual.md (FROZEN: the approved revision plus listed freeze edits only, and a "Freeze" subsection naming them and the approved files' SHA-256); companion files stored unchanged under their delivered names; <ID>_review_notes.md (CLOSED; sections: course of the topic, from the drafts to the frozen manual, responses to ChatGPT's reviews, process record, errata, V2 governance, freeze record with the approved hashes, the closed package list); ChatGPT's Stage 1 authorisation.
- docs/inputs/chatgpt_originals_and_replies/: ChatGPT's Stage 0 prompt and answer sheet; Claude's Stage 0 files, earlier inventories, first deliveries and proposals. docs/inputs/reconciliation/: ChatGPT's reviews and decision notes. docs/inputs/manuals/: every delivered revision of the manual. All unchanged (W-3); a path already in main is refused.
- Changed files: the erratum targets (manual and review notes) and docs/governance/V2_DECISION_REGISTER.md (one row: the decision with what the manual establishes, the reconciliation, errata, VP and Q status and where every input is stored; the reason with the timeline; "No V2 decision is made"; DECIDED) and V2_DESIGN_LOG.md (lessons; tracking notes on the VP, Q and BK rows the topic touches; the agenda status row; session notes with the previous push line; new bookmarks).
- README.md is not changed (the agenda status row is the repository's only status index).
- Delivered to Rama: the zip (package files under their repository paths plus the push commands at its root), the push commands and, when ChatGPT checks the package, a check sheet.

## 7. Push commands

- Template: C-008_PUSH_COMMANDS_24-OCT-2026.txt (269 lines, SHA-256 5824540FB138BDC8B589F3864F28BC8C7286A5FBDEE678CCEB95CC6313A4B9AB), which Rama ran on 24-Oct-2026 with PART 0, PART 1 and PART 2 passing at the first run (Windows PowerShell 5.1, Git 2.53). Rama attaches it in the new chat (it is also at the root of C-008_FREEZE_PACKAGE_24-OCT-2026.zip). It supersedes the template in section 7 of the 21-Oct note: PART 0 unpacks the package zip into a temporary folder, or finds the unzipped files anywhere under Downloads, and copies each file only if its SHA-256 matches; PART 1 also checks README.md.
- For C-009 change only: the title, the date and template lines, the package count, the temporary-folder prefix, the zip name, the two file tables (PART 0: path and SHA-256; PART 1: path, an ASCII content marker regex, SHA-256 and git blob ID), the base and the subject line. Check that no C-008 text is left, ASCII only, no CR, no session link, and that the message is the subject plus the Co-Authored-By line.
- C-008's subject, for the form: "C-008 Signal processing frozen (DR-061) with erratum B-009 E-3".
- Rama runs the three parts in one window. PART 2 refuses to run unless PART 1 passed in the same window. If the window is closed after PART 1, unstage the listed paths (git restore --staged -- <paths>) before re-running PART 1. After any "STOPPED AFTER COMMIT" or "STOPPED AFTER PUSH": no retry, reset or force-push; ask for the output.

## 8. Exact copies (the gate tools of C-008 and the scope rules)

### <SCRATCHPAD>/bin/mr.py
```python
"""Stage-2 wrapper: run mask_read2.py with the given args, then mask mixed/lower-case
client-ID shapes that mask_read_lib's upper-case CLIENTID pattern does not cover.
Never reads legacy files itself; only post-processes the filter's output."""
import re, subprocess, sys
from collections import Counter
LCID = re.compile(r"(?<![A-Za-z0-9])[A-Za-z]{1,3}\d{3,7}(?![A-Za-z0-9])")
ALLOW = re.compile(r"^(?:sha\d{3}|fix\d{3})$", re.I)
out = subprocess.run(["python3", "-I", "/home/claude/tools/mask_read2.py"] + sys.argv[1:],
                     capture_output=True, text=True)
c = Counter()
def rep(m):
    t = m.group(0)
    if ALLOW.match(t):
        return t
    c["LCID"] += 1
    return "<LCID>"
for line in out.stdout.split("\n"):
    if line.startswith("===== "):
        print(line)
    else:
        print(LCID.sub(rep, line))
if out.stderr:
    print("STDERR:", LCID.sub(rep, out.stderr[:2000]))
print(f"===== stage-2 masks: {dict(c)}")
```

### <SCRATCHPAD>/bin/count_grep.py
```python
"""Run mask_read2.py --grep REGEX over files listed in a file; print per-file match counts
parsed from the FILTER's output (never reads legacy files itself)."""
import subprocess, sys, re
regex, listfile = sys.argv[1], sys.argv[2]
files = [l.strip() for l in open(listfile) if l.strip()]
counts = {}
for i in range(0, len(files), 200):
    chunk = files[i:i+200]
    out = subprocess.run(["python3", "-I", "/home/claude/tools/mask_read2.py", "--grep", regex] + chunk,
                         capture_output=True, text=True).stdout
    cur = None
    for line in out.split("\n"):
        m = re.match(r"^===== (\S+) \((\d+) lines\)$", line)
        if m:
            cur = m.group(1); counts[cur] = 0; continue
        if line.startswith("===== masks:"):
            cur = None; continue
        if cur and re.match(r"^\d+: ", line):
            counts[cur] += 1
hits = {k: v for k, v in counts.items() if v}
for k in sorted(hits):
    print(f"{hits[k]:5d}  {k.replace('/home/claude/legacy/prod/', '')}")
print(f"files searched: {len(counts)}; files with matches: {len(hits)}; matching lines: {sum(hits.values())}")
```

### cread_c008.py
```python
#!/usr/bin/env python3
"""cread_c008.py (C-008) - logged read of one legacy file through mr.py (mask_read2.py plus the stage-2 mask).

Copied from cread_c007.py (C-007 Stage 0 and Stage 1, accepted). Same never-open pattern (the 13 rules of sensitive_check.py),
same refusals and the same self-test. C-008 changes:
  - log and evidence go to c008/code_read_log.tsv and c008/evidence/NNN.txt;
  - the excluded records are still the two July records that the C-006 Stage 1 authorisation (section 3) named and the
    C-007 Stage 1 authorisation kept excluded; separate approval is required;
  - STAGE 0 LOCK (restored for C-008): while STAGE1_AUTHORISED is False, every call that passes all the gate checks is still refused
    ("refused: Stage 0 lock"), so no legacy file can be opened at Stage 0. The lock is lifted only as a step of its own,
    after a Stage 1 authorisation, with the tool's new SHA-256 recorded.
Order of refusals: self-test; path shape; never-open path; excluded July record; not in the pinned listing; shares a
never-open blob; local-store check (sensitive_check.py must exit 0); range format; Stage 0 lock; tree entry differs from
the listing; working file differs from the pin.
A refusal opens no legacy file and writes no log row.

Usage (python3 -I): cread_c008.py range A:B FILE | cread_c008.py grep REGEX FILE
"""
import datetime
import hashlib
import os
import re
import subprocess
import sys

S = '<SCRATCHPAD>'
BIN = S + '/bin'
LOG = S + '/c008/code_read_log.tsv'
EVID = S + '/c008/evidence'
REPO = '/home/claude/legacy/prod'
LSTREE = '/home/claude/legacy/prod_lstree.txt'
IST = datetime.timezone(datetime.timedelta(hours=5, minutes=30))
# Lifted at 21:45 IST on 23-Oct-2026 as a step of its own, under C-008_STAGE0_REVIEW_AND_STAGE1_AUTHORIZATION_FOR_CLAUDE_23-OCT-2026.txt
# (received 21:44 IST; its section 3, "Stage 1 reading method"). The previous file's SHA-256 began e32c38fa4d1a7030.
STAGE1_AUTHORISED = True
NEVER_OPEN = re.compile(
    r"(^|/)\.env($|\.)|^config/accounts|^config/security\.yaml$|^config/reference_data/|(^|/)docs/web_claude/"
    r"|(^|/)gui_config\.local\.yaml$|\.db$|\.db-wal$|\.db-shm$|\.sqlite3?$|(^|/)logs/|(^|/)data_store/session/"
    r"|(^|/)[^/]*token[^/]*\.json$")
# The two July records excluded by the C-006 Stage 1 authorisation (section 3); still excluded for C-007 and C-008.
EXCLUDED = {'docs/audit/queue_items_decidable_25jul2026.md', 'docs/audit/stale_queue_entries_25jul2026.md'}
SELFTEST = [
    ('config/security.yaml', None, 'refused: never-open path'),
    ('docs/audit/stale_queue_entries_25jul2026.md', None, 'refused: excluded July record (separate approval required)'),
    ('signals/no_such_file_c008.py', None, 'refused: not in the pinned tree listing'),
    ('docs/v2_design_spec.md', None, 'refused: shares a never-open blob'),
    ('main.py', False, 'refused: protected-path check is not zero'),
]


def listing():
    """Map path -> pinned blob ID from the saved tree listing; also return the never-open blob IDs."""
    oid_of = {}
    for line in open(LSTREE, encoding='utf-8'):
        meta, path = line.rstrip('\n').split('\t', 1)
        oid_of[path] = meta.split()[2]
    never_oids = {oid for path, oid in oid_of.items() if NEVER_OPEN.search(path)}
    return oid_of, never_oids


def store_clean():
    """True when sensitive_check.py exits 0 (no never-open blob in the local object store)."""
    chk = subprocess.run(['python3', '-I', '/home/claude/tools/sensitive_check.py', REPO], capture_output=True, text=True)
    return chk.returncode == 0


def gate(path, store_ok=None):
    """Return None if the path may be read, else the refusal text. Opens no legacy file.
    store_ok: None runs the local-store check; True/False is used only by the self-test."""
    if path.startswith('-') or path.startswith('/') or '..' in path:
        return 'refused: path must be relative to the clone'
    if NEVER_OPEN.search(path):
        return 'refused: never-open path'
    if path in EXCLUDED:
        return 'refused: excluded July record (separate approval required)'
    oid_of, never_oids = listing()
    if path not in oid_of:
        return 'refused: not in the pinned tree listing'
    if oid_of[path] in never_oids:
        return 'refused: shares a never-open blob'
    if not (store_clean() if store_ok is None else store_ok):
        return 'refused: protected-path check is not zero'
    return None


def selftest():
    """Return None when the gate refuses all five test cases as expected, else a description of the failure."""
    for path, store_ok, want in SELFTEST:
        got = gate(path, store_ok)
        if got != want:
            return 'self-test failed for %s: %r' % (path, got)
    return None


def git(args):
    res = subprocess.run(['git', '-C', REPO] + args, capture_output=True, text=True)
    if res.returncode != 0:
        raise SystemExit('git failed: %s' % ' '.join(args[:2]))
    return res.stdout.strip()


def main():
    if len(sys.argv) != 4 or sys.argv[1] not in ('range', 'grep'):
        print(__doc__)
        return 1
    mode, arg, path = sys.argv[1:]
    bad = selftest()
    if bad:
        print('refused: ' + bad)
        return 1
    refusal = gate(path)
    if refusal:
        print(refusal)
        return 1
    if mode == 'range' and not re.fullmatch(r'\d+:\d+', arg):
        print('refused: range must be A:B')
        return 1
    if not STAGE1_AUTHORISED:
        print('refused: Stage 0 lock (no read before a Stage 1 authorisation)')
        return 1
    oid_of, _ = listing()
    tree = git(['ls-tree', 'HEAD', '--', path]).split()
    if len(tree) < 4 or tree[1] != 'blob' or tree[2] != oid_of[path]:
        print('refused: tree entry differs from the pinned listing')
        return 1
    pinned = tree[2]
    work = git(['hash-object', '--no-filters', '--', path])
    if work != pinned:
        print('refused: working file differs from the pinned blob')
        return 1
    res = subprocess.run(['python3', '-I', os.path.join(BIN, 'mr.py'), '--' + mode, arg, path],
                         cwd=REPO, capture_output=True, text=True, timeout=180)
    out = res.stdout
    sys.stdout.write(out)
    m = re.search(r'^===== \S+ \((\d+) lines\)$', out, re.M)
    total = m.group(1) if m else '?'
    printed = len([ln for ln in out.split('\n') if re.match(r'^\d+: ', ln)])
    foot = ' | '.join(ln[6:] for ln in out.split('\n') if ln.startswith('===== masks') or ln.startswith('===== stage-2'))
    os.makedirs(EVID, exist_ok=True)
    n = 1 + (sum(1 for _ in open(LOG)) if os.path.exists(LOG) else 0)
    evid = os.path.join(EVID, '%03d.txt' % n)
    with open(evid + '.tmp', 'w', encoding='utf-8') as fh:
        fh.write(out)
    os.replace(evid + '.tmp', evid)
    esha = hashlib.sha256(out.encode('utf-8')).hexdigest()[:12]
    stamp = datetime.datetime.now(IST).isoformat(timespec='seconds')
    with open(LOG, 'a') as fh:
        fh.write('\t'.join(['%03d' % n, stamp, mode, arg, path, pinned, 'match', total, str(printed), foot,
                            'nev0=ok', 'selftest=ok', 'evidence=' + esha]) + '\n')
    return 0


if __name__ == '__main__':
    sys.exit(main())
```

### ccount_c008.py
```python
#!/usr/bin/env python3
"""ccount_c008.py (C-008) - gated count-only search: count_grep.py (mask_read2.py --grep) over a list of files.

Copied from ccount_c007.py (C-007 Stage 0 and Stage 1, accepted). The gate of cread_c008.py runs in front of every count-only search:
the same self-test first (a protected path, an excluded July record, a path outside the pinned tree, a path with a
protected blob and a local store holding a protected blob must all be refused); then it refuses the whole run if any
listed path is a never-open path, is one of the two excluded July records, is not in the saved listing of the pin, or
shares a never-open blob, or if sensitive_check.py does not exit 0. A refusal searches nothing and writes no log row.
The Stage 0 lock of cread_c008.py does not apply here: the C-008 Stage 0 answer sheet allows path/count-only searches.
Prints per-file match counts only (no matching line), and saves them as c008/search/SEARCH_ID.txt.

Usage (python3 -I): ccount_c008.py SEARCH_ID REGEX LISTFILE
"""
import datetime
import hashlib
import os
import re
import subprocess
import sys

sys.path.insert(0, os.path.dirname(os.path.abspath(__file__)))
import cread_c008 as g  # noqa: E402  (same folder; the gate, the self-test and the pattern are defined once)

COUNT_GREP = g.BIN + '/count_grep.py'
LOG = g.S + '/c008/search_log.tsv'
OUTDIR = g.S + '/c008/search'
PREFIX = g.REPO + '/'


def main():
    if len(sys.argv) != 4 or not re.fullmatch(r'[A-Z][0-9]{2}[a-z]?', sys.argv[1]):
        print(__doc__)
        return 1
    sid, regex, listfile = sys.argv[1:]
    bad = g.selftest()
    if bad:
        print('refused: ' + bad + '; nothing searched')
        return 1
    raw = open(listfile, 'rb').read()
    paths = [ln.strip() for ln in raw.decode('utf-8').split('\n') if ln.strip()]
    rel = [p[len(PREFIX):] if p.startswith(PREFIX) else p for p in paths]
    oid_of, never_oids = g.listing()
    found = {}
    for p in rel:
        if g.NEVER_OPEN.search(p):
            key = 'never-open path'
        elif p in g.EXCLUDED:
            key = 'excluded July record'
        elif p not in oid_of:
            key = 'not in the pinned tree listing'
        elif oid_of[p] in never_oids:
            key = 'shares a never-open blob'
        else:
            continue
        found[key] = found.get(key, 0) + 1
    if found:
        print('refused: the list holds ' + '; '.join(f'{n} {k}' for k, n in sorted(found.items())) + '; nothing searched')
        return 1
    if not g.store_clean():
        print('refused: protected-path check is not zero; nothing searched')
        return 1
    res = subprocess.run(['python3', '-I', COUNT_GREP, regex, listfile], capture_output=True, text=True)
    out = res.stdout
    sys.stdout.write(out)
    os.makedirs(OUTDIR, exist_ok=True)
    dest = os.path.join(OUTDIR, sid + '.txt')
    with open(dest + '.tmp', 'w', encoding='utf-8') as fh:
        fh.write(f'# {sid} | {regex} | {os.path.basename(listfile)}\n' + out)
    os.replace(dest + '.tmp', dest)
    summary = out.strip().split('\n')[-1] if out.strip() else '?'
    stamp = datetime.datetime.now(g.IST).isoformat(timespec='seconds')
    with open(LOG, 'a') as fh:
        fh.write('\t'.join([sid, stamp, regex, os.path.basename(listfile), hashlib.sha256(raw).hexdigest()[:8],
                            str(len(paths)), summary, 'nev0=ok', 'selftest=ok']) + '\n')
    return 0 if res.returncode == 0 else 1


if __name__ == '__main__':
    sys.exit(main())
```

### clines_c008.py
```python
#!/usr/bin/env python3
"""clines_c008.py (C-008) - gated metadata run: line count and byte size of each listed file, nothing else.

The gate of cread_c008.py runs first (the same self-test; the whole run is refused if any listed path is a never-open path,
one of the two excluded July records, not in the saved listing of the pin, or shares a never-open blob, or if
sensitive_check.py does not exit 0). Each working file must equal its pinned blob (git hash-object --no-filters), else the
run is refused. Line totals come from the masking filter's own file header ("===== path (N lines)") on a grep for a
pattern that cannot match, so no line of any file is printed; sizes are the working files' byte sizes (equal to the pinned
blobs'). The Stage 0 lock does not apply: the C-008 Stage 0 prompt allows line counts as metadata.
Prints "lines  bytes  path" per file; saves them as c008/lines/RUN_ID.txt and appends one row to c008/lines_log.tsv.

Usage (python3 -I): clines_c008.py RUN_ID LISTFILE
"""
import datetime
import hashlib
import os
import re
import subprocess
import sys

sys.path.insert(0, os.path.dirname(os.path.abspath(__file__)))
import cread_c008 as g  # noqa: E402  (same folder; the gate, the self-test and the pattern are defined once)

MASK = '/home/claude/tools/mask_read2.py'
NEVER_MATCH = '(?!)'
LOG = g.S + '/c008/lines_log.tsv'
OUTDIR = g.S + '/c008/lines'
PREFIX = g.REPO + '/'


def main():
    if len(sys.argv) != 3 or not re.fullmatch(r'L[0-9]{2}', sys.argv[1]):
        print(__doc__)
        return 1
    rid, listfile = sys.argv[1:]
    bad = g.selftest()
    if bad:
        print('refused: ' + bad + '; nothing counted')
        return 1
    raw = open(listfile, 'rb').read()
    rel = [ln.strip() for ln in raw.decode('utf-8').split('\n') if ln.strip()]
    rel = [p[len(PREFIX):] if p.startswith(PREFIX) else p for p in rel]
    oid_of, never_oids = g.listing()
    found = {}
    for p in rel:
        if g.NEVER_OPEN.search(p):
            key = 'never-open path'
        elif p in g.EXCLUDED:
            key = 'excluded July record'
        elif p not in oid_of:
            key = 'not in the pinned tree listing'
        elif oid_of[p] in never_oids:
            key = 'shares a never-open blob'
        else:
            continue
        found[key] = found.get(key, 0) + 1
    if found:
        print('refused: the list holds ' + '; '.join(f'{n} {k}' for k, n in sorted(found.items())) + '; nothing counted')
        return 1
    if not g.store_clean():
        print('refused: protected-path check is not zero; nothing counted')
        return 1
    for p in rel:
        work = g.git(['hash-object', '--no-filters', '--', p])
        if work != oid_of[p]:
            print('refused: working file differs from the pinned blob: ' + p + '; nothing counted')
            return 1
    totals = {}
    for i in range(0, len(rel), 200):
        chunk = rel[i:i + 200]
        out = subprocess.run(['python3', '-I', MASK, '--grep', NEVER_MATCH] + chunk, cwd=g.REPO, capture_output=True,
                             text=True).stdout
        for line in out.split('\n'):
            m = re.match(r'^===== (\S+) \((\d+) lines\)$', line)
            if m:
                totals[m.group(1).replace(PREFIX, '')] = int(m.group(2))
            elif re.match(r'^\d+: ', line):
                print('refused: the filter printed a line; output discarded')
                return 1
    missing = [p for p in rel if p not in totals]
    if missing:
        print(f'refused: no line total for {len(missing)} file(s); nothing saved')
        return 1
    rows = [f'{totals[p]:6d}  {os.path.getsize(os.path.join(g.REPO, p)):8d}  {p}' for p in rel]
    out = '\n'.join(rows) + f'\nfiles counted: {len(rel)}; lines: {sum(totals[p] for p in rel)}\n'
    sys.stdout.write(out)
    os.makedirs(OUTDIR, exist_ok=True)
    dest = os.path.join(OUTDIR, rid + '.txt')
    with open(dest + '.tmp', 'w', encoding='utf-8') as fh:
        fh.write(f'# {rid} | line totals and byte sizes | {os.path.basename(listfile)}\n' + out)
    os.replace(dest + '.tmp', dest)
    stamp = datetime.datetime.now(g.IST).isoformat(timespec='seconds')
    with open(LOG, 'a') as fh:
        fh.write('\t'.join([rid, stamp, os.path.basename(listfile), hashlib.sha256(raw).hexdigest()[:8], str(len(rel)),
                            'nev0=ok', 'selftest=ok', 'pinned=ok']) + '\n')
    return 0


if __name__ == '__main__':
    sys.exit(main())
```

### gate_test_c008_s1.py
```python
"""gate_test_c008_s1.py - Stage 1 version of gate_test_c008.py: refusal-only test of the C-008 read gate, count gate and
line-count gate. Every call made here is one the gate must
refuse before any legacy file is opened; no permitted file is read or searched. Prints counts and refusal texts only.
Adapted from gate_test_c007_s1.py (C-007 Stage 1, accepted) after the C-008 Stage 0 lock was lifted: the permitted path is checked with the gate function only
(gate() returns None; no file is opened), and the lock must now be lifted (STAGE1_AUTHORISED True).

Usage: python3 -I gate_test_c008_s1.py
"""
import ast
import collections
import hashlib
import os
import re
import subprocess
import sys

S = '<SCRATCHPAD>'
HERE = S + '/c008/gate'
CREAD7 = S + '/c008/tools/cread_c008.py'
CCOUNT7 = S + '/c008/tools/ccount_c008.py'
CLINES8 = S + '/c008/tools/clines_c008.py'
LOG7 = S + '/c008/code_read_log.tsv'
SLOG7 = S + '/c008/search_log.tsv'
LLOG8 = S + '/c008/lines_log.tsv'
REPO = '/home/claude/legacy/prod'
LSTREE = '/home/claude/legacy/prod_lstree.txt'
SHARED = 'docs/v2_design_spec.md'
EXCLUDED = ['docs/audit/queue_items_decidable_25jul2026.md', 'docs/audit/stale_queue_entries_25jul2026.md']
PERMITTED = 'core/schema.sql'
# One made-up name per rule, none of them in the pinned tree except config/security.yaml (checked below).
INVENTED = ['.env', 'x/.env.local', 'config/accounts_new.csv', 'config/security.yaml', 'config/reference_data/x.csv',
            'a/docs/web_claude/x.md', 'ops_dashboard/backend/config/gui_config.local.yaml', 'data_store/x.db',
            'data_store/x.db-wal', 'data_store/x.db-shm', 'data_store/x.sqlite3', 'logs/x.log',
            'data_store/session/x.json', 'data_store/zerodha_token.json']


def run(args):
    res = subprocess.run(['python3', '-I'] + args, capture_output=True, text=True, cwd=HERE)
    return res.returncode, (res.stdout + res.stderr).strip()


def sha(path):
    return hashlib.sha256(open(path, 'rb').read()).hexdigest()[:16] if os.path.exists(path) else 'absent'


def local_objects():
    out = subprocess.run(['python3', '-I', '/home/claude/tools/sensitive_check.py', REPO], capture_output=True, text=True)
    return out.returncode, out.stdout.strip()


def tool_pattern(path):
    """The string literal passed to re.compile for NEVER_OPEN in a tool's source (parsed, not executed)."""
    tree = ast.parse(open(path, encoding='utf-8').read())
    for node in ast.walk(tree):
        if isinstance(node, ast.Assign) and any(getattr(t, 'id', None) == 'NEVER_OPEN' for t in node.targets):
            return ast.literal_eval(node.value.args[0])
    return None


def main():
    sys.path.insert(0, S + '/c008/tools')
    import cread_c008 as g
    pats = {os.path.basename(t): tool_pattern(t) for t in (CREAD7, '/home/claude/tools/sensitive_check.py')}
    print('never-open pattern identical in cread_c008.py and sensitive_check.py:', len(set(pats.values())) == 1)
    print('STAGE1_AUTHORISED in cread_c008.py:', g.STAGE1_AUTHORISED)
    rows = [ln.rstrip('\n').split('\t', 1) for ln in open(LSTREE, encoding='utf-8')]
    tree = {p for _, p in rows}
    prot = sorted(p for p in tree if g.NEVER_OPEN.search(p))
    c006 = [ln.strip() for ln in open(S + '/c006/clar/protected_paths.txt', encoding='utf-8') if ln.strip()]
    print('protected paths in the pinned listing:', len(prot), '| same set as the C-006 clarification:', set(prot) == set(c006))
    print('excluded July records in the pinned listing:', sum(1 for p in EXCLUDED if p in tree), 'of', len(EXCLUDED))
    before = (sha(LOG7), sha(SLOG7), sha(LLOG8))
    rc, chk = local_objects()
    print('before: sensitive_check exit', rc, '|', chk.replace('\n', ' | '))
    print('before: C-008 logs', before)
    res = collections.Counter(run([CREAD7, 'range', '1:1', p]) for p in prot)
    print(f'cread_c008.py on the {len(prot)} protected paths:', {f'rc={k[0]} "{k[1]}"': v for k, v in res.items()})
    res = collections.Counter(run([CREAD7, 'range', '1:1', p]) for p in EXCLUDED)
    print(f'cread_c008.py on the {len(EXCLUDED)} excluded July records:', {f'rc={k[0]} "{k[1]}"': v for k, v in res.items()})
    print('cread_c008.py on', SHARED, '(shares a protected blob):', run([CREAD7, 'range', '1:1', SHARED]))
    print('invented names in the pinned tree:', sum(1 for p in INVENTED if p in tree), 'of', len(INVENTED),
          '(config/security.yaml is the real one)')
    res = collections.Counter(run([CREAD7, 'range', '1:1', p]) for p in INVENTED)
    print(f'cread_c008.py on {len(INVENTED)} rule-shaped names:', {f'rc={k[0]} "{k[1]}"': v for k, v in res.items()})
    print('cread_c008.py on a permitted-shaped name not in the tree:',
          run([CREAD7, 'range', '1:1', 'signals/no_such_file_c008.py']))
    permitted = g.gate(PERMITTED)
    print('gate() on a permitted path in the tree (' + PERMITTED + '; no file opened):', repr(permitted))
    lists = {
        'list_protected.txt': [REPO + '/' + p for p in prot],
        'list_excluded.txt': [REPO + '/main.py'] + [REPO + '/' + p for p in EXCLUDED],
        'list_shared.txt': [REPO + '/main.py', REPO + '/' + SHARED],
        'list_not_in_tree.txt': [REPO + '/main.py', REPO + '/signals/no_such_file_c008.py'],
    }
    for name, rows_ in lists.items():
        with open(HERE + '/' + name, 'w', encoding='utf-8') as fh:
            fh.write('\n'.join(rows_) + '\n')
        print(f'ccount_c008.py on {name} ({len(rows_)} paths):', run([CCOUNT7, 'T00', 'schema', HERE + '/' + name]))
        print(f'clines_c008.py on {name} ({len(rows_)} paths):', run([CLINES8, 'L00', HERE + '/' + name]))
    after = (sha(LOG7), sha(SLOG7), sha(LLOG8))
    rc2, chk2 = local_objects()
    print('after: sensitive_check exit', rc2, '|', chk2.replace('\n', ' | '))
    print('after: C-008 logs', after, '| unchanged:', before == after)
    ok = before == after and rc == 0 and rc2 == 0 and chk == chk2 and g.STAGE1_AUTHORISED and permitted is None
    print('GATE TEST', 'PASSED' if ok else 'FAILED')
    return 0 if ok else 1


if __name__ == '__main__':
    sys.exit(main())
```

### scope_rules.py
```python
"""scope_rules.py - rebuilds the five count-only search scopes of C-005 to C-008 from the saved listing of the pin.
At 970aabf the rules give s_all 1,260, s_rt 281, s_code 869, s_tests 486 and s_cfg 41 paths (checked 24-Oct-2026 against
the lists used at C-008: identical). Paths are written with the clone prefix, one per line, sorted.
Usage: python3 -I scope_rules.py LSTREE CLONE_DIR OUT_DIR"""
import os
import re
import sys

LSTREE, CLONE, OUT = sys.argv[1:4]
NEVER = re.compile(
    r"(^|/)\.env($|\.)|^config/accounts|^config/security\.yaml$|^config/reference_data/|(^|/)docs/web_claude/"
    r"|(^|/)gui_config\.local\.yaml$|\.db$|\.db-wal$|\.db-shm$|\.sqlite3?$|(^|/)logs/|(^|/)data_store/session/"
    r"|(^|/)[^/]*token[^/]*\.json$")
JULY = {'docs/audit/queue_items_decidable_25jul2026.md', 'docs/audit/stale_queue_entries_25jul2026.md'}
CODE = {'.py', '.yaml', '.html', '.sh', '.service', '.bat', '.js', '.sql', '.cron', '.timer', '.ps1'}
NO_EXT_CODE = {'deploy/hooks/post-receive', 'deploy/hooks/pre-commit', 'deploy/hooks/pre-receive',
               'deploy/security/bin/copy-guard', 'deploy/security/bin/request-copy'}
rows = [ln.rstrip('\n').split('\t', 1) for ln in open(LSTREE, encoding='utf-8')]
oid = {p: m.split()[2] for m, p in rows}
prot_oids = {oid[p] for p in oid if NEVER.search(p)}


def text_file(p):
    return not (p.endswith(('.png', '.webp', '.gitkeep', '.csv')) or os.path.basename(p) in ('.gitignore', '.gitattributes'))


def is_test(p):
    return p.startswith('tests/') or '/tests/' in p


s_all = {p for p in oid if not NEVER.search(p) and oid[p] not in prot_oids and p not in JULY and text_file(p)}
s_code = {p for p in s_all if os.path.splitext(p)[1] in CODE or p in NO_EXT_CODE}
scopes = {
    's_all': s_all,
    's_code': s_code,
    's_tests': {p for p in s_code if is_test(p)},
    's_rt': {p for p in s_all if p.endswith('.py') and not is_test(p)},
    's_cfg': {p for p in s_all if os.path.splitext(p)[1] in {'.yaml', '.service', '.timer', '.cron'} and not is_test(p)},
}
os.makedirs(OUT, exist_ok=True)
for name, paths in scopes.items():
    with open(os.path.join(OUT, name + '.txt'), 'w', encoding='utf-8') as fh:
        fh.write(''.join(os.path.join(CLONE, p) + '\n' for p in sorted(paths)))
    print(name, len(paths))
```
