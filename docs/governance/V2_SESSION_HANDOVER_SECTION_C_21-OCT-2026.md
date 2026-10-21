# V2 SESSION HANDOVER: SECTION B CLOSED, SECTION C NEXT

Date: 2026-10-21 (IST)
Purpose: let a new chat in the "Trading System V2 — Design & Build" project continue with Section C (C-001 to C-011, signal lifecycle) using the method practised from B-009 to the Section B end. This note is a working aid, not a decision record: decisions live in docs/governance/V2_DECISION_REGISTER.md, and rules and lessons in docs/governance/V2_DESIGN_LOG.md. Where this note and those files differ, those files win.
Security: no credential values, account IDs, mailbox addresses or host addresses.
Supersedes, as the working aid for new chats: V2_SESSION_HANDOVER_SECTION_B_09-OCT-2026.md (kept unchanged as a record).

## 1. State at handover
- Repository trading-system-v2 (private), branch main. Section B end package pushed by Rama at 2d5ee58 (21-Oct-2026, parent b390ab0); this note is stored by the commit that follows it, with the design-log update that records it.
- Section A is complete: A-001 to A-011 FROZEN (DR-013, DR-014, DR-018 to DR-026, with errata); Section A end decisions DR-027 to DR-033; close DR-034.
- Section B is complete: B-001 to B-012 FROZEN (DR-035, DR-037, DR-039 to DR-041, DR-043 to DR-049, with errata); Section B end decisions DR-050 (trading days and the session calendar) and DR-051 (the entry cut-off applies to intraday only); close and scheduling moves DR-052.
- Last numbers used: DR-052, VP-68, Q-45.
- Open questions: Q-01, Q-03 to Q-06, Q-11 to Q-13, Q-15 (PARTLY ANSWERED), Q-20, Q-21, Q-23, Q-25, Q-26, Q-30, Q-31, Q-34 to Q-38 and Q-40 to Q-45. Q-02 is NOTED; Q-14 is non-blocking. Q-30, Q-31 and Q-34 to Q-45 are DR-016 audit leads (the full legacy audit after the agenda).
- Proposals that name a Section C topic as a ratification point (design log section 3, "Ratify at" column):
  - C-001 (Chartink strategy / scanner): VP-08 (count DECIDED, DR-031; other parts open), VP-44, VP-45.
  - C-002 (scanner to webhook): VP-45. C-010 (scanner to strategy mapping): VP-45.
  - C-004 (validation): VP-27. C-005 (deduplication): VP-12 (Q-33 answered into it).
  - C-006 to C-009 (queue, persistence, processing, expiry): VP-59; C-008 also VP-11 and VP-31; C-009 and C-008: VP-24.
  - C-011 (EOD / daily signal routing): VP-14.
  - Open question for Section C: Q-20 (signal-to-instrument lookup key and path; unknown symbols).
- Owed at the next design-log update: nothing beyond what that topic adds.

## 2. How each topic runs (as practised from B-009)
1. Rama uploads ChatGPT's set for the topic, with file names unchanged (DR-008): the answer sheet for Claude and the Claude research prompt, plus V2_DESIGN_LOG.md and V2_DECISION_REGISTER.md from main.
2. Claude answers the sheet from the production snapshot only (Rama, 20-Oct: the testing snapshot and records repositories are left out) and writes:
   - <ID>_CLAUDE_REPOSITORY_ANSWERS_<DD-MON-YYYY>.txt (each answer with file, function, line range and an evidence label);
   - <ID>_SOURCE_INVENTORY_AND_METHOD_<DD-MON-YYYY>.txt (files read, blob IDs, method, any process exception disclosed).
   Then a brief reply; Rama takes both to ChatGPT.
3. ChatGPT reconciles and drafts: a reconciliation (stored in docs/inputs/reconciliation/) and a manual draft (docs/inputs/manuals/).
4. Claude writes revision 0, checking the draft's load-bearing claims against the source:
   - <ID>_manual.md (legacy fact only; DRAFT until frozen);
   - <ID>_reconciliation_addendum.md (where Claude's answers and ChatGPT's draft differ, and what was decided);
   - <ID>_v2_design_inputs.md (PROPOSED / OPEN; Rama's "note it for the new system" requests);
   - <ID>_review_notes.md (sources, findings, corrections, errata, points for Rama);
   - design-log updates: lessons, Q and VP rows, the agenda row, a session note; errata to frozen manuals when needed.
5. ChatGPT reviews (its review file is stored in the topic folder). Claude applies it and freezes: status "FROZEN <date> (revision n, DR-0xx)", review notes CLOSED, one DR row, a closed package manifest (each file with SHA-256 and blob ID), and the push commands (section 7). Rama's decisions are asked as multiple-choice questions (DR-017, W-4); Rama often pre-approves the final files.
6. Rama runs PART 0, PART 1 and PART 2 and returns each output; Claude confirms the commit against the remote (parent, file list, each blob, message).
Section end: a decision paper (ChatGPT's prompt and answer sheet answered by Claude; revisions after ChatGPT's reviews; Rama's answers into DR rows; scheduling moves recorded one by one with "scheduling only" sentences).
Folders: docs/agenda/<topic folder>/, lowercase. Proposed for Section C: c-0nn_<topic>, for example c-001_chartink-strategy-scanner; confirm with Rama at C-001.

## 3. Standing rules (from Rama and the design log)
- Two layers (W-1): a manual records legacy fact only; V2 material is PROPOSED in design inputs and VP rows. Topic boundaries (W-6): a topic does not settle another topic's questions. A ChatGPT review alone never decides (W-6).
- Evidence is claim-specific (W-7, DR-036). Labels: FACT (SOURCE-VERIFIED at 970aabf); FACT (RECORD-DERIVED); SUPPLIED RECORD (INDIRECT); HISTORICAL (Rama); UNKNOWN / NOT ESTABLISHED; INTERPRETATION / AUDIT LEAD; KNOWN DEFECT / AMBIGUITY; KNOWN DOC DRIFT; PROPOSED / OPEN; CURRENT EXTERNAL FACT with the date checked (W-5).
- File-save rule (W-2): once a step's files are final, the reply ends with a SAVE list (file, folder, new or replaces).
- Live-only (DR-001); no paper mode or parity steps; mempalace retired; no .xlsx generators (DR-009); documentation only under docs/ (DR-010); no legacy replica (DR-011); no screenshots or PNGs stored (DR-008); docs/inputs/ is never edited (adding files is the practice).
- The legacy system is DEAD / REFERENCE ONLY. No V2 implementation is authorised (DR-016: the full legacy audit follows the agenda).
- Brief chat replies (2 to 3 sentences where possible); detailed deliverables. Search official or dated sources for current external facts. Ask Rama when something load-bearing is ambiguous; never answer for him.
- Commit attribution (W-8, DR-036, DR-038): every commit message ends with this one line only:
  Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
  No Claude session link in any commit message or project record, even when a chat's own default instructions add one; Rama's rule takes precedence. Claude never commits or pushes; Rama does.
- Lessons: read the design log's section 1 before starting. Most relevant to Section C: comments, docstrings, check names and config comments are claims; reading a function is not reading its call path (callbacks, flags, optional dependencies passed at the production call site); inventories come from every file, not a module's description; negatives carry their searched scope ("not found in the inspected scope"); "never/always/every" needs evidence over its whole scope; quote Rama and keep verdict and caveat together; copy every blob ID, line number and DR number from a tool output of the same session.

## 4. Security rules (no exceptions)
- Never reproduce passwords, API keys or secrets, access tokens, TOTP seeds, webhook secrets, account or client IDs, VM or host addresses, mailbox addresses, person names, bot or SSH key names, or the PC hostname or username, in documents, chat or tool output. Use placeholders such as {account}. Section C touches webhook authentication: describe the mechanism and where a secret is configured (its location and type), never a value.
- Never open, fetch or read: .env and .env.*; config/accounts.csv; config/accounts_multi_example.csv; config/security.yaml; config/reference_data/; docs/web_claude/ (anywhere); token or session files; gui_config.local.yaml; *.db files; logs. sensitive_check.py (section 8) encodes this list.
- Do not contact a VM, host, broker, database, service manager, live endpoint or external account. Do not open broker book exports.
- Before sending, scan every output with scan_ids.py (value-blind) and check that no session link is present. Known benign scan hits: hashes and blob IDs (long_token), "SHA256", the attribution address, Q-B001-style IDs, the 3 "A…(len 4)" hits on register row DR-042, and the "Claude-Session" text in DR-038 (not a link); compare any other hit with the previous delivery's scan before sending.

## 5. Reading the legacy source safely
Source: the production snapshot github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot, baseline commit 970aabf3c78d777d9d34549e021f7c8e756c7bdb (dated 2026-09-11), public.
1. Clone blob-less and sparse, with explicit, anchored paths:
   git clone --filter=blob:none --no-checkout https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot.git /home/claude/legacy/prod
   cd /home/claude/legacy/prod
   git sparse-checkout set --no-cone /main.py /signals/webhook_receiver.py   (one anchored path per pattern; every non-cone pattern starts with "/")
   git checkout                (main is at the baseline; confirm with git rev-parse HEAD = 970aabf3c78d777d9d34549e021f7c8e756c7bdb)
2. Save the tree listing once (tree reads fetch no blobs): git ls-tree -r HEAD > prod_lstree.txt. Use it for paths and blob IDs.
3. Never run a tree-wide or history command on the partial clone: no git grep with a revision, no git ls-tree -l, no git log -p, no git show, no git log on a path. Commit metadata for a named commit only (git log -1 --format=%H%n%cI <commit>), read through the filter.
4. After every checkout, at the start of each session and before each delivery: python3 -I sensitive_check.py /home/claude/legacy/prod must print "never-open blobs present locally: 0"; note the local object count.
5. Read code and records only through mask_read2.py, options before the file names (mask_read2.py --range a:b FILE, mask_read2.py --grep REGEX FILE); route every derived output (counts, parses, diffs) through it as well. Run Python with -I.
6. Records that may name hosts or keys (system maps, deployment notes, audit notes): narrow --grep reads only. In Section C, docs/audit/c2_chartink_ip_investigation_03jul2026.md is such a record.
7. Check line citations with cite_check.py before delivery.

## 6. Next step: C-001 (Chartink strategy / scanner)
- Inputs to request from Rama: ChatGPT's C-001 answer sheet and Claude research prompt; V2_DESIGN_LOG.md and V2_DECISION_REGISTER.md from main.
- Frozen records to cite and deepen, not repeat: A-008 (strategy model; the Chartink scanner set, Q-17), A-009 (configuration: chartink_scanners.yaml, scan_webhook_map.yaml), A-004 (instruments; Q-20), B-009 (intraday operation; the signal path in the session), B-006 (workers), B-007 (readiness), B-002 (configuration loading).
- Production paths likely in scope (names from the tree listing; read only through the filter): signals/webhook_receiver.py, signals/signal_processor.py, signals/entry_throttle.py, signals/__init__.py, config/chartink_scanners.yaml, config/scan_webhook_map.yaml, scripts/preflight_scanner_check.py, scripts/test_webhook.py, ops_dashboard/backend/services/scanner_attribution.py; tests: tests/unit/test_webhook_receiver.py, test_signal_processor.py, test_c2_webhook_lockdown.py, test_s4_webhook_degrade.py, test_preflight_signals.py, test_signal_status_classification.py, test_fix133_replay_signals.py, test_load_test_signals.py, tests/integration/test_full_signal_flow.py; records: docs/audit/bk1_long_short_scanner_analysis_17jul2026.md and the C2 investigation note (narrow reads). Build the actual inventory from every file that imports or calls these, not from this list.
- Proposals to carry: VP-08, VP-44, VP-45 for C-001 (section 1 lists the rest of Section C).

## 7. Push commands (pattern used from the Section B end, 21-Oct-2026)
Three parts, run by Rama in one PowerShell window, each pasted whole; Claude delivers PART 1 and PART 2 as separate files when asked.
- PART 0 copies each file from Downloads into the repository only if its SHA-256 matches (handles "(1)", "(2)" duplicates).
- PART 1 changes nothing until every check has passed: Git 2.23 or later; fetch; branch main; HEAD = origin/main = base; a clean index (otherwise stop and list it); each file's content marker and SHA-256; no other tracked or untracked change under docs; the credential pattern and session-link scans on the package files and on HEAD (the one known match, the B-004 answer sheet line 466, a link whose api_key parameter is empty, is accepted only by its blob ID dd7c6c3a0b965b3c7d2ecb95155a138666ac59d2, its line number and a single matching line). Then it stages the allowlist only and verifies the staged list, each staged blob ID and the scans on the index; on failure it unstages only those paths (git restore --staged).
- PART 2 re-checks the start state and the staged blob IDs before committing; commits; verifies parent, files, blobs and message; fetches again before pushing; pushes without force; verifies the remote by ls-remote and fetch. A failure after the commit is reported with the local commit ID and nothing is pushed or undone.
Template (fill <BASE>, the file rows and the subject; keep patterns ASCII):

```powershell
# ===== PART 0: copy each file from Downloads into the repo, only if its SHA-256 matches =====
$repo = "D:\Projects\trading-system-v2"
$dl   = Join-Path $env:USERPROFILE "Downloads"
$want = @(
  @("docs\<folder>\<file>", "<SHA-256 in upper case>")
)
foreach ($w in $want) {
  $dest = Join-Path $repo $w[0]
  $leaf = Split-Path $w[0] -Leaf
  $base0 = [System.IO.Path]::GetFileNameWithoutExtension($leaf)
  $ext  = [System.IO.Path]::GetExtension($leaf)
  $src  = Get-ChildItem -LiteralPath $dl -File -Filter ($base0 + "*" + $ext) -ErrorAction SilentlyContinue | Where-Object { (Get-FileHash -Algorithm SHA256 -LiteralPath $_.FullName).Hash -eq $w[1] } | Select-Object -First 1
  New-Item -ItemType Directory -Force -Path (Split-Path $dest -Parent) | Out-Null
  if ($src) { Copy-Item -LiteralPath $src.FullName -Destination $dest -Force; "COPIED     $($w[0])" }
  elseif ((Test-Path -LiteralPath $dest) -and ((Get-FileHash -Algorithm SHA256 -LiteralPath $dest).Hash -eq $w[1])) { "IN PLACE   $($w[0])" }
  else { "NOT FOUND  $($w[0])" }
}

# ===== PART 1: all checks first; then stage exactly the listed files and verify the index; no commit. Send its whole output =====
$repo = "D:\Projects\trading-system-v2"
$base = "<BASE: the full commit ID that HEAD and origin/main must equal>"
Set-Location $repo
$fail = 0; $part1Passed = $false
# path, content marker, SHA-256 of the file, git blob ID of the file
$files = @(
  @("docs/<folder>/<file>", "<ASCII regex that proves the new content>", "<SHA-256 in upper case>", "<git blob ID>")
)
$expected = @($files | ForEach-Object { $_[0] })
$credFile = "docs/inputs/chatgpt_originals_and_replies/B-004_BROKER_CONNECTION_ANSWER_SHEET_14-OCT-2026.txt"
$credBlob = "dd7c6c3a0b965b3c7d2ecb95155a138666ac59d2"
$credLine = "466"
$credPat  = "(api[_-]?key|api[_-]?secret|password|totp|access[_-]?token)\s*[:=]\s*\S{8,}"
$linkPat  = "claude\.ai/code/session"

# Step 1: start state and index (read only)
$gv = git --version
$gm = [regex]::Match("$gv", "(\d+)\.(\d+)")
$gitOk = $gm.Success -and (([int]$gm.Groups[1].Value -gt 2) -or ([int]$gm.Groups[1].Value -eq 2 -and [int]$gm.Groups[2].Value -ge 23))
"INFO 1: $gv; PowerShell $($PSVersionTable.PSVersion)"
if (-not $gitOk) { $fail++; "STOP 1: Git 2.23 or later is needed (git restore --staged)" }
git fetch origin
if ($LASTEXITCODE -ne 0) { $fail++; "STOP 1: git fetch origin failed" }
$branch = git symbolic-ref --short -q HEAD
$head = git rev-parse --verify -q HEAD
$orig = git rev-parse --verify -q origin/main
if ($branch -ne "main" -or $head -ne $base -or $orig -ne $base) { $fail++; "STOP 1: start state differs: branch $branch, HEAD $head, origin/main $orig; expected main at $base" } else { "OK   1: on main; HEAD = origin/main = $base" }
$pre = @(git diff --cached --name-only)
if ($pre.Count -gt 0) { $fail++; "STOP 1: the index already holds staged changes (left as they are):"; $pre } else { "OK   1: index clean, nothing staged" }

# Step 2: the listed files in the working tree: content marker and SHA-256
foreach ($f in $files) {
  $p  = Join-Path $repo ($f[0] -replace "/", "\")
  $ok = Select-String -LiteralPath $p -Pattern $f[1] -Quiet -ErrorAction SilentlyContinue
  $h  = (Get-FileHash -Algorithm SHA256 -LiteralPath $p -ErrorAction SilentlyContinue).Hash
  if (-not $ok -or $h -ne $f[2]) { $fail++ }
  "{0,-5} {1,-9} {2}" -f $(if ($ok) {"PASS"} else {"FAIL"}), $(if ($h -eq $f[2]) {"HASH-OK"} else {"HASH-DIFF"}), $f[0]
}

# Step 3: changes under docs: only the listed files, and each of them changed
$changed = @(git status --porcelain --untracked-files=all -- docs | ForEach-Object { $_.Substring(3) })
$extra   = @($changed | Where-Object { $expected -notcontains $_ })
$same    = @($expected | Where-Object { $changed -notcontains $_ })
if ($extra.Count -gt 0) { $fail++; "STOP 3: other changes under docs (left as they are):"; $extra } else { "OK   3: no other change under docs" }
if ($same.Count -gt 0) { $fail++; "STOP 3: package files that show no change against HEAD:"; $same }

# Step 4: scans before staging, on the package files and on HEAD (matched lines are never printed)
$candCred  = @($files | ForEach-Object { if (Select-String -LiteralPath (Join-Path $repo ($_[0] -replace "/", "\")) -Pattern $credPat -Quiet) { $_[0] } })
$candLinks = @($files | ForEach-Object { if (Select-String -LiteralPath (Join-Path $repo ($_[0] -replace "/", "\")) -Pattern $linkPat -Quiet) { $_[0] } })
if ($candCred.Count -gt 0) { $fail++; "STOP 4: credential pattern in package files (lines not printed):"; $candCred } else { "OK   4: credential pattern: none in the package files" }
if ($candLinks.Count -gt 0) { $fail++; "STOP 4: session link in package files:"; $candLinks } else { "OK   4: session links: none in the package files" }
$headCred  = @(git grep -l -I -i -E $credPat HEAD)
$knownHits = @(git grep -n -I -i -E $credPat HEAD -- $credFile)
$knownLine = if ($knownHits.Count -eq 1) { ($knownHits[0] -split ":")[2] } else { "" }
$knownHits = $null
$knownBlob = git rev-parse --verify -q "HEAD:$credFile"
if ($headCred.Count -eq 1 -and $headCred[0] -eq ("HEAD:" + $credFile) -and $knownLine -eq $credLine -and $knownBlob -eq $credBlob) { "OK   4: credential pattern in HEAD: only the known B-004 answer-sheet line $credLine (blob unchanged; a link whose api_key parameter is empty)" } else { $fail++; "STOP 4: credential pattern in HEAD differs from the known line (files listed, lines not printed):"; $headCred }
$headLinks = @(git grep -l $linkPat HEAD)
if ($headLinks.Count -gt 0) { $fail++; "STOP 4: session link in HEAD:"; $headLinks } else { "OK   4: session links: none in HEAD" }

# Steps 5 to 8: stage only if every check passed; verify the index; if that fails, unstage only the listed paths
if ($fail -ne 0) {
  "PART 1 FAILED - no new files staged: $fail check(s) failed above; the index was not changed. Do not run PART 2; send this output to Claude."
} else {
  $v = 0
  git add -- $expected
  if ($LASTEXITCODE -ne 0) { $v++; "CHECK 5: git add reported an error" }
  $staged = @(git diff --cached --name-only)
  $d = $null; if ($staged.Count -gt 0) { $d = Compare-Object -ReferenceObject ($expected | Sort-Object) -DifferenceObject ($staged | Sort-Object) }
  if ($staged.Count -ne $expected.Count -or $d) { $v++; "CHECK 6: the staged list differs from the $($expected.Count)-file allowlist:"; $d } else { "OK   6: staged list = the $($expected.Count)-file allowlist" }
  $bad = 0
  foreach ($f in $files) {
    $entry = @(git ls-files -s -- $f[0])
    $blob  = if ($entry.Count -eq 1) { ($entry[0] -split "\s+")[1] } else { "" }
    if ($blob -ne $f[3]) { $bad++; "CHECK 6: staged content differs: $($f[0]) (staged $blob, expected $($f[3]))" }
  }
  if ($bad -gt 0) { $v += $bad } else { "OK   6: the $($expected.Count) staged blob IDs equal the package's" }
  $kEntry   = @(git ls-files -s -- $credFile)
  $kBlob    = if ($kEntry.Count -eq 1) { ($kEntry[0] -split "\s+")[1] } else { "" }
  $idxCred  = @(git grep --cached -l -I -i -E $credPat)
  $idxCredN = @(git grep --cached -c -I -i -E $credPat -- $credFile)
  if ($kBlob -eq $credBlob -and $idxCred.Count -eq 1 -and $idxCred[0] -eq $credFile -and $idxCredN.Count -eq 1 -and $idxCredN[0] -eq ($credFile + ":1")) { "OK   6: credential pattern in the index: only the known B-004 line" } else { $v++; "CHECK 6: credential pattern in the index differs from the known line (files listed, lines not printed):"; $idxCred }
  $idxLinks = @(git grep --cached -l $linkPat)
  if ($idxLinks.Count -gt 0) { $v++; "CHECK 6: session link in the index:"; $idxLinks } else { "OK   6: session links: none in the index" }
  if ($v -eq 0) {
    $part1Passed = $true
    git status --short
    git diff --cached --stat
    "PART 1 PASSED - exactly $($expected.Count) intended files staged; no commit made. Send this whole output; run PART 2 only after ChatGPT's check and your approval."
  } else {
    $fail += $v
    git restore --staged -- $expected
    $after   = @(git diff --cached --name-only)
    $touched = @($files | Where-Object { (Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $repo ($_[0] -replace "/", "\")) -ErrorAction SilentlyContinue).Hash -ne $_[2] } | ForEach-Object { $_[0] })
    if ($after.Count -eq 0 -and $touched.Count -eq 0) { "PART 1 FAILED - no new files staged: the index check failed, so the $($expected.Count) paths were unstaged again; the index is empty and the $($expected.Count) working files are unchanged (SHA-256 re-checked). Send this output to Claude." }
    else { "PART 1 FAILED - unstaging did not finish cleanly. Still staged:"; $after; "Working files differing from the package:"; $touched; "Send this output to Claude; do not reset." }
  }
}

# ===== PART 2: pre-commit guard, commit, verify, push, verify. Run only after PART 1 PASSED in this window and your approval =====
$msg = @"
<subject line>

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
"@
& {
  if (-not $expected -or -not $files -or -not $base -or $fail -ne 0 -or -not $part1Passed) { "PART 2 STOPPED - PART 1 did not pass in this window: nothing committed. Send the output to Claude."; return }

  # A. Pre-commit guard (read only)
  git fetch origin
  if ($LASTEXITCODE -ne 0) { "PART 2 STOPPED - git fetch origin failed: nothing committed; the $($expected.Count) files stay staged. Send the output to Claude."; return }
  $branch = git symbolic-ref --short -q HEAD
  $head   = git rev-parse --verify -q HEAD
  $orig   = git rev-parse --verify -q origin/main
  if ($branch -ne "main" -or $head -ne $base -or $orig -ne $base) { "PART 2 STOPPED - start state changed since PART 1: branch $branch, HEAD $head, origin/main $orig; expected main at $base. Nothing committed; the $($expected.Count) files stay staged. Send the output to Claude."; return }
  "OK   A: on main; HEAD = origin/main = $base"
  $s2 = @(git diff --cached --name-only)
  $d2 = $null; if ($s2.Count -gt 0) { $d2 = Compare-Object -ReferenceObject ($expected | Sort-Object) -DifferenceObject ($s2 | Sort-Object) }
  if ($s2.Count -ne $expected.Count -or $d2) { "PART 2 STOPPED - the staged list changed since PART 1: nothing committed. Send the output to Claude."; $d2; return }
  foreach ($f in $files) {
    $entry = @(git ls-files -s -- $f[0])
    $blob  = if ($entry.Count -eq 1) { ($entry[0] -split "\s+")[1] } else { "" }
    if ($blob -ne $f[3]) { "PART 2 STOPPED - staged content changed since PART 1: $($f[0]); nothing committed. Send the output to Claude."; return }
  }
  "OK   A: staged list and the $($expected.Count) staged blob IDs unchanged since PART 1"

  # B. Commit
  git commit -m $msg
  $commitExit = $LASTEXITCODE
  $new = git rev-parse --verify -q HEAD
  if ($new -eq $base) { "PART 2 STOPPED - git commit did not create a commit: nothing committed, nothing pushed; the $($expected.Count) files stay staged. Send the output to Claude."; return }
  $afterCommit = { param($why) "PART 2 STOPPED AFTER COMMIT - $why. Local commit $new is on main and was NOT pushed; this script changed nothing on GitHub. Do not reset, amend, re-run or push; send this output to Claude." }
  if ($commitExit -ne 0) { & $afterCommit "git commit reported an error"; return }

  # C. Verify the commit
  $parent = git rev-parse --verify -q "HEAD~1"
  $cfiles = @(git diff-tree --no-commit-id --name-only -r HEAD)
  $d3 = $null; if ($cfiles.Count -gt 0) { $d3 = Compare-Object -ReferenceObject ($expected | Sort-Object) -DifferenceObject ($cfiles | Sort-Object) }
  if ($parent -ne $base -or $cfiles.Count -ne $expected.Count -or $d3) { & $afterCommit "the commit's parent or file list differs"; $d3; return }
  foreach ($f in $files) {
    $cb = git rev-parse --verify -q ("HEAD:" + $f[0])
    if ($cb -ne $f[3]) { & $afterCommit "the committed content of $($f[0]) differs"; return }
  }
  $wantMsg = @($msg -split "\r?\n" | Where-Object { $_ -ne "" }) -join "|"
  $gotMsg  = @(git log -1 --format=%B | Where-Object { $_ -ne "" }) -join "|"
  if ($gotMsg -ne $wantMsg) { & $afterCommit "the commit message differs"; return }
  "OK   C: commit $new on parent $base; $($expected.Count) files; each blob ID and the message as expected"

  # D. Pre-push check: GitHub main must still be the base
  git fetch origin
  $fetchExit = $LASTEXITCODE
  $orig2 = git rev-parse --verify -q origin/main
  if ($fetchExit -ne 0 -or $orig2 -ne $base) { & $afterCommit "origin/main is now $orig2, not the base (or the fetch failed)"; return }
  "OK   D: origin/main is still $base"

  # E. Push (no force)
  git push origin HEAD:refs/heads/main
  if ($LASTEXITCODE -ne 0) { & $afterCommit "git push failed"; return }

  # F. Verify GitHub
  $remote = ((git ls-remote origin refs/heads/main) -split "\s+")[0]
  git fetch origin
  $orig3 = git rev-parse --verify -q origin/main
  if ($remote -ne $new -or $orig3 -ne $new) { "PART 2 STOPPED AFTER PUSH - GitHub main shows $remote (fetch: $orig3), expected $new. Do not force-push, reset or re-run; send this output to Claude."; return }
  "PART 2 PASSED - commit $new pushed to GitHub main and verified (ls-remote and fetch). Send this output to Claude."
}
```

## 8. Helper scripts (exact copies; place them in /home/claude/tools/)
### mask_read_lib.py
```python
"""Print files with secret-like values masked. Usage: mask_read.py [--range a:b] [--grep REGEX] file..."""
import re, sys
from collections import Counter
WHITELIST = {"EMA50","EMA20","SMA50","SMA20","RSI14","ATR14","UTF8","SHA1","MD5"}
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
```

### mask_read2.py
```python
"""mask_read.py plus host, SSH-alias, key-file and fingerprint masking.

Usage: mask_read2.py [--range a:b] [--grep REGEX] file...

Before printing, it scans the requested files and the legacy host records
(docs/SYSTEM_MAP.md, DEPLOYMENT.md in the production clone) for host-like
tokens (tailnet names, ssh targets, ssh config hosts, user@host, key files,
key names, fingerprints) and masks every occurrence of each token that is
not a common word. Values are never printed.
"""
import os
import re
import sys
from collections import Counter

sys.path.insert(0, os.path.dirname(os.path.abspath(__file__)))
import mask_read_lib as base  # noqa: E402

COMMON = set("""are and any all app add api ask config confirmed host sudo shell
start stop status session sessions into from the user root ubuntu sync setup
access keys key agent server tunnel port command commands login copy""".split())

CATS = [
    re.compile(r"\b([A-Za-z0-9-]+(?:\.[A-Za-z0-9-]+)*\.ts\.net)\b"),
    re.compile(r"\bssh\s+(?:-[A-Za-z]\s+\S+\s+)*([A-Za-z0-9_.@-]{3,})"),
    re.compile(r"^\s*Host\s+([A-Za-z0-9_.-]{3,})", re.M),
    re.compile(r"\b([a-z_][a-z0-9_-]*@[A-Za-z0-9.-]{3,})"),
    re.compile(r"\.ssh/([A-Za-z0-9_.-]{3,})"),
    re.compile(r"\b(id_[a-z0-9_]{3,}|[A-Za-z0-9_-]{3,}\.(?:pem|ppk|key))\b"),
    re.compile(r"(SHA256:[A-Za-z0-9+/=]{4,})"),
    re.compile(r"\b([0-9a-f]{2}(?::[0-9a-f]{2}){5,})"),
    re.compile(r"(?i)\bhostname\s*[`'\"]?([A-Za-z0-9][A-Za-z0-9_.-]{2,})"),
    # 2026-10-13 (B-003): also `hostname=<name>` / `host: <name>` forms (a boot-proof
    # record printed `hostname=<name>`, which the pattern above did not cover).
    re.compile(r"(?i)\bhost(?:name)?\s*[=:]\s*[`'\"]?([A-Za-z0-9][A-Za-z0-9_.-]{2,})"),
    re.compile(r"(?i)\bVM\s+`([A-Za-z0-9][A-Za-z0-9_.-]{2,})`"),
]
EXTRA = [
    "/home/claude/legacy/prod/docs/SYSTEM_MAP.md",
    "/home/claude/legacy/prod/DEPLOYMENT.md",
    "/home/claude/legacy/sbx/SNAPSHOT_MANIFEST.md",
]

args = sys.argv[1:]
rng = None
grep = None
while args and args[0].startswith("--"):
    if args[0] == "--range":
        rng = args[1]
        args = args[2:]
    elif args[0] == "--grep":
        grep = re.compile(args[1])
        args = args[2:]

tokens = set()
for p in list(args) + EXTRA:
    try:
        t = open(p, encoding="utf-8", errors="replace").read()
    except OSError:
        continue
    for rx in CATS:
        for m in rx.finditer(t):
            v = m.group(1)
            if v.lower() not in COMMON:
                tokens.add(v)
tok_rx = None
if tokens:
    alts = sorted(tokens, key=len, reverse=True)
    tok_rx = re.compile("|".join(re.escape(v) for v in alts))

for path in args:
    counts = Counter()
    lines = open(path, encoding="utf-8", errors="replace").read().split("\n")
    a, b = 1, len(lines)
    if rng:
        x, y = rng.split(":")
        a = int(x or 1)
        b = int(y or len(lines))
    print(f"===== {path} ({len(lines)} lines)")
    for i in range(a - 1, min(b, len(lines))):
        raw = lines[i]
        line = raw
        if tok_rx is not None:
            line, n = tok_rx.subn("<HOSTTOK>", line)
            counts["HOSTTOK"] += n
        line = base.mask(line, counts)
        if grep is None or grep.search(raw):
            print(f"{i + 1}: {line}")
    print(f"===== masks: {dict(counts)}")
```

### sensitive_check.py
```python
"""Prove that no never-open blob is in a partial clone's local object store.

Usage: python -I sensitive_check.py <repo_dir>

Matches the never-open list against each tree PATH (not the whole
ls-tree line), so root-level paths such as .env.example and logs/ are
anchored correctly. Prints counts and path classes only, never contents.
"""
import re
import subprocess
import sys

repo = sys.argv[1]
NEVER_OPEN = re.compile(
    r"(^|/)\.env($|\.)"
    r"|^config/accounts"
    r"|^config/security\.yaml$"
    r"|^config/reference_data/"
    r"|(^|/)docs/web_claude/"
    r"|(^|/)gui_config\.local\.yaml$"
    r"|\.db$|\.db-wal$|\.db-shm$|\.sqlite3?$"
    r"|(^|/)logs/"
    r"|(^|/)data_store/session/"
    r"|(^|/)[^/]*token[^/]*\.json$"
)

tree = subprocess.run(["git", "-C", repo, "ls-tree", "-r", "-z", "HEAD"],
                      capture_output=True, check=True).stdout.decode("utf-8", "replace")
sensitive = {}
for entry in tree.split("\0"):
    if not entry:
        continue
    meta, path = entry.split("\t", 1)
    oid = meta.split()[2]
    if NEVER_OPEN.search(path):
        sensitive[oid] = path

local = set(subprocess.run(["git", "-C", repo, "cat-file", "--batch-all-objects",
                            "--batch-check=%(objectname)"],
                           capture_output=True, check=True).stdout.decode().split())

classes = {}
for path in sensitive.values():
    key = path.split("/")[0] if "/" in path else path.split(".")[0] + "." + "*"
    classes[key] = classes.get(key, 0) + 1
present = [oid for oid in sensitive if oid in local]
print(f"never-open paths in tree: {len(sensitive)}; by top-level class: {dict(sorted(classes.items()))}")
print(f"local objects: {len(local)}")
print(f"never-open blobs present locally: {len(present)}  (must be 0)")
sys.exit(1 if present else 0)
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

### cite_check.py
```python
"""Value-blind check of legacy source line citations in V2 docs.

Extracts (file, start, end) citations from the given Markdown files, resolves each file
against the production tree listing at 970aabf, and checks that every cited line lies
within the checked-out file. Prints counts, unresolved paths and out-of-range citations
only; never prints file contents.

Usage: python3 -I cite_check.py LSTREE PROD_ROOT DOC [DOC ...] [--only-lines REGEX]
"""
import os
import re
import sys

LSTREE, PROD = sys.argv[1], sys.argv[2]
args = sys.argv[3:]
only = None
if "--only-lines" in args:
    k = args.index("--only-lines")
    only = re.compile(args[k + 1])
    args = args[:k] + args[k + 2:]
DOCS = args

tree = []
for line in open(LSTREE, encoding="utf-8"):
    if "\t" in line:
        tree.append(line.rstrip("\n").split("\t", 1)[1])
tree_set = set(tree)
by_base = {}
for p in tree:
    by_base.setdefault(os.path.basename(p), []).append(p)

PATH_RX = re.compile(r"(?<![\w/.-])((?:[\w.-]+/)*[\w-]+(?:\.[\w-]+)*\.(?:py|sql|md|cron|service|timer|yaml|sh))(?![\w/])")
NUM = r"\d+(?:\s+to\s+\d+)?"
LIST = NUM + r"(?:(?:\s*,\s*|\s+and\s+)" + NUM + r")*"
# a range list right after a path: "x.py: 1 to 2", "x.py 3", "x.py lines 4 to 5", "x.py line 6"
AFTER_PATH = re.compile(r"^(?:'s\s+\w+\s+)?\s*[:]?\s*\(?\s*(?:lines?\s+)?(" + LIST + r")(?=[\s;,).|]|$)")
# a free "line(s) N ..." anywhere
FREE = re.compile(r"(?<![\w-])(lines?)\s+(" + LIST + r")(?=[\s;,).|:]|$)")
# a bare parenthesised range "(N to M)" or "(N)" closed by ")" or ";"
BARE = re.compile(r"\((" + LIST + r")(?=[);])")
TOPIC = re.compile(r"(?:A|B)-0\d\d\s+$")


def resolve(tok):
    if tok in tree_set:
        return tok
    if "/" in tok:
        cands = [p for p in tree if p.endswith("/" + tok)]
        return cands[0] if len(cands) == 1 else None
    c = by_base.get(tok, [])
    return c[0] if len(c) == 1 else None


def parse_list(s):
    out = []
    for part in re.split(r"\s*,\s*|\s+and\s+", s.strip()):
        m = re.match(r"(\d+)(?:\s+to\s+(\d+))?$", part.strip())
        if m:
            a = int(m.group(1))
            b = int(m.group(2) or m.group(1))
            out.append((a, b))
    return out


line_counts = {}


def nlines(p):
    if p not in line_counts:
        fp = os.path.join(PROD, p)
        if os.path.isfile(fp):
            with open(fp, encoding="utf-8", errors="replace") as f:
                line_counts[p] = len(f.read().split("\n"))
        else:
            line_counts[p] = None
    return line_counts[p]


cites = []          # (doc, lineno, file, a, b, how)
unresolved = set()
unattributed = []   # (doc, lineno, text)

for doc in DOCS:
    section_default = None
    for i, text in enumerate(open(doc, encoding="utf-8").read().split("\n"), 1):
        if only and not only.search(text):
            continue
        # section defaults for the manual's two tables with an implied file
        if text.startswith("### 4.1"):
            section_default = "core/state_store.py"
        elif text.startswith("### 4.2") or text.startswith("## 8.") or text.startswith("## 5."):
            section_default = None
        elif text.startswith("## 7."):
            section_default = "main.py"
        spans = []  # (start, end, file) for each path token
        for m in PATH_RX.finditer(text):
            tok = m.group(1)
            r = resolve(tok)
            if r is None:
                if not re.match(r"^(B|A)-0\d\d_|^V2_|^LEGACY_|^CLAUDE", os.path.basename(tok)):
                    unresolved.add(tok)
                spans.append((m.start(), m.end(), None))
                continue
            spans.append((m.start(), m.end(), r))
            mm = AFTER_PATH.match(text[m.end():])
            if mm:
                for a, b in parse_list(mm.group(1)):
                    cites.append((doc, i, r, a, b, "after-path"))
        used = set()
        for s, e, r in spans:
            mm = AFTER_PATH.match(text[e:])
            if r and mm:
                used.add((e + mm.start(1), e + mm.end(1)))

        def last_path_before(pos):
            best = None
            for s, e, r in spans:
                if e <= pos:
                    best = r
            return best

        for m in FREE.finditer(text):
            span = (m.start(2), m.end(2))
            if any(u[0] <= span[0] and span[1] <= u[1] for u in used):
                continue
            if TOPIC.search(text[:m.start()]):
                continue  # a frozen manual's line, checked separately
            r = last_path_before(m.start()) or section_default
            if r is None:
                unattributed.append((doc, i, m.group(0)))
                continue
            for a, b in parse_list(m.group(2)):
                cites.append((doc, i, r, a, b, "free"))
            used.add(span)
        for m in BARE.finditer(text):
            span = (m.start(1), m.end(1))
            if any(u[0] <= span[0] and span[1] <= u[1] for u in used):
                continue
            r = last_path_before(m.start()) or section_default
            if r is None:
                continue
            for a, b in parse_list(m.group(1)):
                cites.append((doc, i, r, a, b, "bare"))

bad = []
missing = set()
for doc, i, r, a, b, how in cites:
    n = nlines(r)
    if n is None:
        missing.add(r)
        continue
    if not (1 <= a <= b <= n):
        bad.append((os.path.basename(doc), i, r, a, b, n, how))

print(f"citations extracted: {len(cites)} (files: {len(set(c[2] for c in cites))})")
by_doc = {}
for c in cites:
    by_doc[os.path.basename(c[0])] = by_doc.get(os.path.basename(c[0]), 0) + 1
print("per document:", by_doc)
print(f"out of range: {len(bad)}")
for x in bad:
    print("  BAD", x)
print(f"cited files not checked out: {sorted(missing)}")
print(f"path tokens not resolved in the tree: {sorted(unresolved)}")
print(f"free 'line(s)' references with no file in context: {len(unattributed)}")
for x in unattributed:
    print("  UNATTRIBUTED", os.path.basename(x[0]), x[1], x[2])
```

