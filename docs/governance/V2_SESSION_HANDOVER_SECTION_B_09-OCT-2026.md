# V2 SESSION HANDOVER: SECTION A CLOSED, SECTION B NEXT

Date: 2026-10-09 (IST)
Purpose: let a new chat in the "Trading System V2 — Design & Build" project continue with Section B (B-001 to B-012) using the same method as Section A. This note is a working aid, not a decision record: decisions live in docs/governance/V2_DECISION_REGISTER.md, and rules and lessons in docs/governance/V2_DESIGN_LOG.md.
Security: no credential values, account IDs, mailbox addresses or host addresses.

## 1. State at handover
- Repository trading-system-v2 (private), branch main at fa0654b (pushed 09-Oct-2026). Today's pushes: d2730d1 (A-010 frozen), 5ab0034 (A-011 frozen), 7b6e6de (Section A end decisions), fa0654b (decision-paper wording cleanup and ChatGPT's final QA note).
- Section A is complete:
  - A-001 to A-011 legacy manuals are FROZEN (DR-013, DR-014, DR-018 to DR-026, with their errata).
  - The end-of-Section-A decisions are DR-027 to DR-033: purpose, scope, ETFs, SME, strategy count, product intent and operator boundary.
  - DR-034 records the scheduling moves and the close.
- Last numbers used: DR-034, VP-65, Q-37.
- Design-log housekeeping owed at the B-001 first pass: add the session-note line "Section A end pushed (7b6e6de); decision-paper cleanup pushed (fa0654b)." The 5ab0034 push is already noted.
- Open questions: Q-01, Q-03 to Q-06, Q-11 to Q-15, Q-20, Q-21, Q-23, Q-25, Q-26, Q-30, Q-31 and Q-34 to Q-37. Q-02 is NOTED; Q-14 is non-blocking.
- Proposals that name a Section B topic as a ratification point:
  - Configuration (B-002): VP-13, VP-15, VP-32, VP-42, VP-43, VP-45, VP-46, VP-48, VP-51.
  - Start and boot (B-001, B-002): VP-16, VP-47, VP-54.
  - Market-open and intraday (B-008, B-009): VP-19, VP-20.
  - End of day, shutdown and restart (B-010 to B-012): VP-21, VP-22, VP-52, VP-62, VP-63.
  - Startup reconciliation (B-005): VP-60.
  - Cron (B): VP-25.
  - All of B-001 to B-012: VP-57.

## 2. How each topic runs (as practised in Section A)
1. Rama uploads ChatGPT's set for the topic, with file names unchanged (DR-008):
   - the discussion checklist with his answers (docs/inputs/chatgpt_originals_and_replies/);
   - the reconciliation (docs/inputs/reconciliation/);
   - the legacy manual (docs/inputs/manuals/);
   - the current V2_DESIGN_LOG.md and V2_DECISION_REGISTER.md from main.
2. Claude writes the first pass, checking the load-bearing claims in the production source at 970aabf:
   - <ID>_manual.md (DRAFT, legacy fact only);
   - <ID>_v2_design_inputs.md (PROPOSED; Rama's "note it for the new system" requests go here, with a pointer from the manual);
   - <ID>_review_notes.md (OPEN: sources, findings, answers against the source, corrections to ChatGPT, errata, points for Rama, reading method);
   - design-log updates: lessons, Q rows, VP rows, the agenda row and a session note;
   - errata to frozen manuals when needed: the original stays visible as ~~text~~ (superseded by erratum E-n), a corrected line follows, the status line is appended, and an "## Erratum (date, found during <ID>)" section is added.
3. Claude ends with a brief reply: key findings, any genuine question for Rama, and the instruction to send the files to ChatGPT. Then the SAVE list (W-2), noting that nothing is committed yet.
4. Rama returns ChatGPT's VALUE_ADDED_REVIEW file, with answers if any.
5. Claude applies the review and freezes the topic:
   - Status: "FROZEN <date> (revision 1, DR-0xx)";
   - review notes CLOSED, with a section mapping each review point;
   - one DR row, which also records accepted errata;
   - a status line in each earlier topic's review notes for its erratum;
   - the SAVE list and the PowerShell commands (section 6).

   No separate approval is needed (DR-017, W-4) unless ChatGPT leaves a question for Rama or disagrees; then ask Rama first, for example with a multiple-choice question.
6. Rama pushes and returns the console output; Claude confirms the commit hash.

Folders: docs/agenda/<topic folder>/ with lowercase names. Section A used names such as a-011_system-states-and-lifecycle. The Section B folders do not exist yet; agree a pattern with Rama at B-001 (for example b-001_vm-service-startup).

## 3. Standing rules (from Rama and the design log)
- Two layers (W-1): a manual records legacy fact only; V2 material is PROPOSED in design inputs and VP rows. Topic boundaries (W-6): a topic does not settle another topic's questions. A ChatGPT review alone never decides.
- Every reply that involves a file ends with a SAVE list: file, folder, new or replace (W-2).
- Live-only (DR-001); no paper mode or parity steps; mempalace retired; no .xlsx generators (DR-009); documentation only under docs/ (DR-010); no legacy replica (DR-011); no screenshots or PNGs stored (DR-008).
- Brief chat replies (2 to 3 sentences where possible); detailed deliverables.
- Search official or dated sources for any current external fact, and label it CURRENT EXTERNAL FACT with the date checked (W-5). Ask Rama when something load-bearing is ambiguous; never answer for him.
- Evidence labels:
  - FACT (SOURCE-VERIFIED at 970aabf);
  - FACT (RECORD-DERIVED, a dated record in the baseline repository);
  - SUPPLIED RECORD (INDIRECT);
  - HISTORICAL (Rama);
  - UNKNOWN / NOT ESTABLISHED;
  - INTERPRETATION / AUDIT LEAD;
  - KNOWN DEFECT / AMBIGUITY;
  - "no effect identified in the inspected path".
- Lessons: read the design log's section 1 before starting. The most relevant for Section B are those of 2026-10-07 to 2026-10-09:
  - comments and docstrings are claims;
  - reading a function is not reading its call path, including callbacks and flags;
  - check restart claims against every supervisor;
  - date behaviour across code changes;
  - summaries keep their qualifiers;
  - write a mechanism apart from its effect;
  - quote users rather than paraphrase.
- Commit attribution lines (end of every commit message):
  Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
  Claude-Session: https://claude.ai/code/session_01WjAv4HZpAv5rsHWfyPTjQb
  (A new chat may be given new attribution lines; use whatever its own instructions say.)

## 4. Security rules (no exceptions)
- Never reproduce passwords, API keys or secrets, access tokens, TOTP seeds, webhook secrets, account or client IDs, VM or host addresses, mailbox addresses, person names, bot or SSH key names, or the PC hostname or username. This applies in documents, chat and tool output. Use placeholders such as {account} or [testing client ID].
- Never open, fetch or read: .env and .env.*; config/accounts.csv; config/accounts_multi_example.csv; config/security.yaml; config/reference_data/; docs/web_claude/ (anywhere); token or session files; gui_config.local.yaml; *.db files; logs.
- Before sending, scan every output for ID, address and secret shapes with scan_ids.py (value-blind) and the push-time pattern in section 6.

## 5. Reading the legacy source safely
Sources: the production snapshot github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot (baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb, dated 2026-09-11) and the testing snapshot trading-system-sbx-snapshot (head ade89ff, dated 2026-09-17). Both are public.

1. Clone blob-less and sparse, with an explicit path list:
   git clone --filter=blob:none --no-checkout <repo-url> prod
   cd prod
   git sparse-checkout set --no-cone <one explicit file path per pattern, for example /main.py /utils/startup_checks.py>
   git checkout
   (core.sparseCheckoutCone=false; partial-clone filter blob:none.) Add paths to the sparse list as needed. Never widen it to whole folders that hold never-open files.
2. Never run a tree-wide or history command on the partial clone: no git grep with a revision, no git ls-tree -l, no git log -p, no git show. Each one fetches blobs (lesson 2026-10-08). Commit metadata for named commits only (git log -1 --format=%H%n%cI <commit>). Search only checked-out files, with local tools.
3. After every checkout, prove that no sensitive blob is in the local store. Tree reads do not fetch blobs:
   git ls-tree -r HEAD | grep -E '(^|/)\.env($|\.)|config/accounts|config/security\.yaml|config/reference_data/|docs/web_claude/|gui_config\.local\.yaml|\.db$|(^|/)logs/' | awk '{print $3}' > sensitive_oids.txt
   git cat-file --batch-all-objects --batch-check='%(objectname)' > local_oids.txt
   grep -c -x -F -f sensitive_oids.txt local_oids.txt     # must print 0
4. Read code and records only through mask_read.py, and route every derived or parsed output through it too (lesson 2026-10-09). Use --range a:b and --grep REGEX; run Python with -I.
5. Compare production and testing by object IDs from git ls-tree -r (no -l), so identical files are never fetched. Check out only the files that differ, with the same check.
6. systemd and other vendor documentation: WebFetch can time out. A sparse clone of the vendor's own documentation sources works, as done for systemd's man pages in A-011.

## 6. Commands and checks
- check_md.py: table pipe counts and balanced strike-through. Run it on every changed Markdown file.
- scan_ids.py: a value-blind scan for ID, IPv4, mailbox, credential-assignment, bot-token and long-token shapes. Two known benign hits exist in frozen manuals: YAML section numbers in A-009 and "PBKDF2-SHA256" in A-010.
- PowerShell pattern used for every push:
  $repo = "D:\Projects\trading-system-v2"
  $checks = @( @("<relative path>", "<regex that proves the new content>"), ... )
  foreach ($c in $checks) { if (Select-String -Path (Join-Path $repo $c[0]) -Pattern $c[1] -Quiet -ErrorAction SilentlyContinue) { "PASS  $($c[0])" } else { "FAIL  $($c[0])" } }
  Set-Location $repo; git add docs; git status --short
  git grep --cached -l -I -i -E "(api[_-]?key|api[_-]?secret|password|totp|access[_-]?token)\s*[:=]\s*\S{8,}"     # expect nothing
  $msg = @"<subject line>`n`n<attribution lines>"@; git commit -m $msg; git push
  Keep check patterns ASCII; Select-String can mis-read an em dash.

## 7. Next step: B-001 (VM and service startup)
- Inputs to request from Rama: ChatGPT's B-001 set (checklist with his answers, reconciliation, legacy manual), plus the current V2_DESIGN_LOG.md and V2_DECISION_REGISTER.md (main at fa0654b).
- Legacy evidence already frozen that B-001 should cite and deepen rather than repeat:
  - A-011 sections 5 (daily start and the 17-step boot table), 6 (start-up scenarios), 7 (exit codes across systemd and the token watcher), 19 (cron);
  - A-009 section 5 (configuration loading at start);
  - A-010 sections 7 and 8 (token chain and auth failure);
  - A-003 (start window).
- V2 proposals to carry: VP-16, VP-47, VP-54, VP-57 and VP-62, plus VP-64 (supervision, owned by M-001).

## 8. Helper scripts (exact copies)

### mask_read.py
```python
"""Print files with secret-like values masked. Usage: mask_read.py [--range a:b] [--grep REGEX] file..."""
import re, sys
from collections import Counter
WHITELIST = {"EMA200","EMA50","EMA20","SMA200","SMA50","SMA20","RSI14","ATR14","UTF8","SHA256","SHA1","MD5","ISO8601","RFC3339","RFC5424"}
PATS = [
    ("EMAIL", re.compile(r"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}")),
    ("IPV4", re.compile(r"\b(?:\d{1,3}\.){3}\d{1,3}\b")),
    ("TGBOT", re.compile(r"\b\d{6,12}:[A-Za-z0-9_-]{20,}")),
    ("KV", re.compile(r"(?i)((?:password|passwd|pwd|secret|token|api_key|apikey|api_secret|totp|pat|bearer)[\"']?\s*[:=]\s*[\"']?)([^\s\"',;)}]{6,})")),
    ("LONGNUM", re.compile(r"-?\b\d{9,}\b")),
    ("ALNUM", re.compile(r"\b(?=[A-Za-z0-9]*\d)(?=[A-Za-z0-9]*[A-Za-z])[A-Za-z0-9]{12,}\b")),
    ("B32", re.compile(r"\b[A-Z2-7]{16,}\b")),
    ("CLIENTID", re.compile(r"(?<![A-Za-z0-9])[A-Z]{1,3}\d{3,7}(?![A-Za-z0-9])")),
]
def mask(line, counts):
    for name, pat in PATS:
        def rep(m, name=name):
            if name == "KV":
                val = m.group(2)
                if re.fullmatch(r"[A-Z][A-Z0-9_]*", val) or val.startswith(("os.", "self.", "env", "<", "$", "{", "[")) or val.lower() in {"none","null","true","false","required","optional","redacted"}:
                    return m.group(0)
                counts[name] += 1
                return m.group(1) + "<MASKED>"
            tok = m.group(0)
            if tok in WHITELIST:
                return tok
            if name == "ALNUM" and re.fullmatch(r"[0-9a-f]{12,40}", tok) and len(tok) in (40,):
                counts["SHA40"] += 1
                return tok[:7] + "<sha>"
            counts[name] += 1
            return f"<{name}>"
        line = pat.sub(rep, line)
    return line
args = sys.argv[1:]; rng = None; grep = None
while args and args[0].startswith("--"):
    if args[0] == "--range": rng = args[1]; args = args[2:]
    elif args[0] == "--grep": grep = re.compile(args[1]); args = args[2:]
for path in args:
    counts = Counter()
    lines = open(path, encoding="utf-8", errors="replace").read().split("\n")
    a, b = (1, len(lines))
    if rng:
        x, y = rng.split(":"); a = int(x or 1); b = int(y or len(lines))
    print(f"===== {path} ({len(lines)} lines)")
    for i in range(a - 1, min(b, len(lines))):
        out = mask(lines[i], counts)
        if grep is None or grep.search(lines[i]):
            print(f"{i+1}: {out}")
    print(f"===== masks: {dict(counts)}")
```

### check_md.py
```python
import sys, re
for path in sys.argv[1:]:
    lines = open(path, encoding="utf-8").read().split("\n")
    problems = 0; i = 0
    while i < len(lines):
        if lines[i].startswith("|") and i + 1 < len(lines) and re.match(r"^\|(\s*:?-+:?\s*\|)+\s*$", lines[i+1]):
            ncols = lines[i].count("|"); j = i + 2
            while j < len(lines) and lines[j].startswith("|"):
                n = len(re.findall(r"(?<!\\)\|", lines[j]))
                if n != ncols:
                    print(f"{path}:{j+1}: {n} pipes vs header {ncols}: {lines[j][:90]}"); problems += 1
                j += 1
            i = j
        else:
            i += 1
    for k, l in enumerate(lines):
        if l.count("~~") % 2:
            print(f"{path}:{k+1}: unbalanced ~~"); problems += 1
    print(f"{path}: {'OK' if problems == 0 else str(problems)+' problems'}")
```

### scan_ids.py
```python
"""Value-blind scan for identifier, address and secret shapes.

Prints file, line, pattern name and a masked shape of each hit
(first character + length), never the matched value.
"""
import re
import sys

PATTERNS = {
    "client_id_shape": re.compile(r"(?<![A-Za-z0-9])[A-Z]{1,3}\d{3,7}(?![A-Za-z0-9])"),
    "ipv4": re.compile(r"(?<![\d.])\d{1,3}(?:\.\d{1,3}){3}(?![\d.])"),
    "email": re.compile(r"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}"),
    "credential_assignment": re.compile(
        r"(?i)(api[_-]?key|api[_-]?secret|password|passwd|totp[_-]?secret|access[_-]?token|bot[_-]?token|webhook[_-]?secret|secret)\s*[:=]\s*['\"]?[A-Za-z0-9/+_\-]{8,}"),
    "telegram_bot_token": re.compile(r"\d{8,10}:[A-Za-z0-9_-]{30,}"),
    "long_token": re.compile(r"(?<![A-Za-z0-9])[A-Za-z0-9]{32,}(?![A-Za-z0-9])"),
    "private_key": re.compile(r"-----BEGIN [A-Z ]*PRIVATE KEY-----"),
}

# Known non-secret long token: the legacy baseline commit hash (public).
ALLOWED_LONG = {"970aabf3c78d777d9d34549e021f7c8e756c7bdb"}


def shape(v):
    return f"{v[0]}…(len {len(v)})"


total = 0
for path in sys.argv[1:]:
    hits = 0
    with open(path, encoding="utf-8") as fh:
        for n, line in enumerate(fh, 1):
            for name, rx in PATTERNS.items():
                for m in rx.finditer(line):
                    v = m.group(0)
                    if name == "long_token" and v in ALLOWED_LONG:
                        continue
                    hits += 1
                    print(f"{path}:{n}: {name}: {shape(v)}")
    total += hits
    print(f"{path}: {hits} hit(s)")
print(f"TOTAL: {total}")
```
