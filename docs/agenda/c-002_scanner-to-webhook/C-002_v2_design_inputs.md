# C-002 SCANNER TO WEBHOOK: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-21 (revision 0). It stays PROPOSED / OPEN after the C-002 manual's freeze (DR-055, 2026-10-22). Changed at the freeze: this sentence; section 4, question 2's cell; item 5d; section 6 (its heading, its table header and the VP-45, VP-11 and VP-50 rows). Item 5i's owner text corrected on 2026-10-25 at the Section C close (DR-066, S-3), owner text only. Nothing here is a V2 decision (V2_DESIGN_LOG W-1, W-6); it records Rama's inputs of 21-Oct-2026, the questions carried to their owners and Claude's considerations.
Labels: LEGACY = what the old system did (C-002_manual.md). RAMA = his words, quoted. DECIDED = an existing register row, quoted. CHATGPT = a point of ChatGPT's reviews, as written there. CLAUDE CONSIDERATION = a question for the owner, not a requirement.

## 1. Rama's inputs of 21-Oct-2026
- RAMA (to ChatGPT, in the C-002 chat, shown in his screenshot; paraphrased, with placeholders for the values): one Chartink account for two systems on two VMs, identified by scan names "[CLIENT CODE 1]-<strategy name>" for one VM and "[CLIENT CODE 2]-<strategy name>" for the other; he asked how such names would appear in the payload JSON and in the system's YAML or other strategy-name files: "So ensure how we are going to deal in new system".
- RAMA (to Claude, answering the question on DR-028; verbatim): "Simple, 2 different VM's with 2 different zerodha account (So basically both VM's has their own separate resources like capital, telgram/email credentials, broker accounts, API etc) but what matters means single Chartink website for singal, So identification purposes I'll add zerodha client code in strategy name that's the only thing, So you can assure both system will trade on its own independent to other".
- RAMA (to Claude, on the testing tag in his screenshot): "I typed it myself" (C-002_manual.md section 4.2).
- His earlier inputs that these extend: VP-45's uniformity requirement (A-008 Q5; A-009 Q38: "need a uniformity whether create chartink scanners with/without broker id or any other changes inside the system"); the six strategies of C-001_v2_design_inputs.md section 1.

## 2. The scope question against DR-028 (Q-46; OPEN)
- DECIDED (DR-028, 09-Oct-2026): "Zerodha only, with one Zerodha account trading in production (development and pre-live testing are left to Q-01 and VP-56)".
- Rama's statement describes two independent systems, each trading its own Zerodha account. Read per system, the two agree; read for V2 as a whole, two accounts would trade in production, which DR-028's words do not cover. DR-028 is not edited either way; a new register row will record which reading holds.
- Where it is settled: the Section C end decision paper, with VP-45 and VP-56 (Rama's choice of 21-Oct: "Section C end paper (Recommended)"). C-002 only records it, as Q-46.

## 3. Legacy facts the design starts from (LEGACY; C-002_manual.md)
- The address in an alert's webhook URL decided which VM received the alert, and its path segment decided the scanner key (sections 4.2 and 7.2). CURRENT EXTERNAL FACT (a Chartink support reply of 28-May-2024, read 21-Oct-2026 through a summarising fetch; section 4.4): one webhook is allowed per scan and alert.
- An alert's name reached the receiver only in the payload (scan_name, scan_url); the receiver read scan_name alone, stripped the primary account's tag (WR4b) and cross-checked the rest against the path; no client-ID shape occurs in the map or the strategy YAMLs (section 8.3).
- Plain HTTP; the token in the query string and repeated in the body's webhook_url; stored payloads held it to 05-Jul; a 400 or malformed-JSON warning could log it (sections 4.2, 5.3 and 10.6). One shared token served every alert (the 15-Jul specification; section 4.2).
- The framework's own answers, a failed body read and a failed insert left no audit row, and every answer but 200 recorded zero counts (sections 10.2 and 10.3).
- The receiver listened only while the process ran; an alert meeting a closed port was lost unless the sender retried, and no evidence shows that Chartink retried (sections 6.5, 12 and 13.1).
- The alerts repeated a matching symbol about once a minute (section 13.4).

## 4. Questions carried (OPEN; CHATGPT, as written there: questions 1 to 9 from the reconciliation review, section 5; questions 10 and 11 from the value-added review, section 5)
| # | Question | Carried to (the row, with its "Ratify at" cell at main) |
|---|---|---|
| 1 | Is DR-028 scoped per system or across the entire V2 deployment? | Q-46: the Section C end decision paper |
| 2 | Are client codes necessary in human-readable scan names, or should a non-sensitive stable system alias be used? | VP-45: C-010, configuration design (build phase); C-002 dropped at the freeze (DR-055; section 6) |
| 3 | What is the authoritative system identity: endpoint/path namespace, configuration mapping, payload check, or multiple checks? | VP-45 |
| 4 | How should a system reject a scanner name/payload that indicates the other system? | VP-45 |
| 5 | Must each system have separate webhook secrets and other credentials? VP-56 already proposes per-environment separation; per-system isolation must be explicitly considered. | VP-56: Q-05, M-001, Q-01 |
| 6 | How will external scan names map to immutable internal strategy IDs and YAML/config entries? | VP-45 |
| 7 | How are renames, duplicate names, case, separators and migrations handled? | VP-45 (U-9) |
| 8 | Can the Chartink subscription support the required number of alerts? This remains U-10. | VP-45 (U-10) |
| 9 | How will a common Chartink outage be detected and handled across both systems? | VP-34: L-001 to L-004 |
| 10 | From the value-added review (its question 1, as written there): "Is the external scanner/alert name required to be globally unique across both systems, or only unique within each system?" | VP-45 (U-9) |
| 11 | From the value-added review (its question 8): "How are historical events attributed if a scanner is renamed or moved between systems?" | VP-45 |

The value-added review's other six questions are covered by questions 2 to 7 above.

## 5. Considerations carried to their owners (CLAUDE CONSIDERATION)
- a. Identity (VP-45): legacy routing came from each alert's URL (its address and path), never from the display name. A design that keeps the name for the operator and uses the URL and a per-system secret for routing would let a name prefix help people without carrying routing (C-002_manual.md sections 7.2 and 8.3).
- b. A misdirected alert (VP-45): should a system refuse an alert whose name carries another system's tag, and record and alert it on both channels (VP-34)?
- c. The real client code in names (VP-45, VP-51, VP-55): it would arrive in every payload (scan_name, scan_url) and so in logs and stored payloads unless V2 maps or redacts it; VP-51 keeps account IDs out of tracked files and VP-55 redacts logs. CHATGPT, the value-added review (question 7): "Prefer a non-sensitive, stable system alias over embedding a real broker client identifier in scanner names, logs, payloads or filenames. Client identifiers should not be exposed merely for convenience." CHATGPT, the reconciliation review (section 5): "This is not a prohibition or a final decision; it is a privacy/security consideration for VP-45. A non-sensitive alias is an option to evaluate with Rama." Rama's stated plan is the client code; the choice stays with VP-45.
- d. Secrets per system (VP-56, VP-50): VP-56 proposes per-environment credentials, a separate webhook secret per environment among them; Rama's two systems raise the same question per system. VP-50 proposes that the exposed legacy token is never reused; on 21-Oct a token-bearing legacy URL was still saved on at least one Chartink testing alert (C-002_review_notes.md section 2). RAMA (22-Oct-2026): both legacy VMs terminated and deleted, the Chartink subscription expired, and the current scanners and their URLs to be treated as placeholders; the clean-up was closed as not needed (not as done), and the token stays treated as exposed (VP-50; C-002_manual.md section 4.3).
- e. Secrets in the body (VP-55; C-003): the legacy body repeated the webhook URL, token included, so a secret carried in a URL would also travel in Chartink's body; redaction would then have to cover bodies, stored payloads and warning lines, not only query strings (C-002_manual.md sections 4.2 and 10.6).
- f. Arrival evidence (VP-11, VP-59): the legacy audit missed the framework's own answers and failed body reads, and recorded refusals by response code only; whether V2 records every arrival at the edge, framework-level refusals included, is for those rows' owners (C-002_manual.md sections 10.2 and 10.3).
- g. Sender retries (VP-58): no evidence shows that Chartink retried, so a design cannot rely on a sender's retry: an alert that meets a closed port is lost unless the sender tries again (C-002_manual.md sections 12 and 13.1).
- h. The shared dependency (VP-34): one Chartink login would feed both systems; its outage, a plan limit (U-10) or an edited alert would affect both at once.
- i. Transport security (whole-design review, with VP-50, VP-55, VP-56): plain HTTP and a query token were the legacy facts (C-002_manual.md sections 5.3 and 7.4); C-003 recorded the legacy only (DR-056), and V2's TLS and webhook authentication go to the whole-design review.

## 6. VP-45, which names C-002, and the rows annotated (V2_DESIGN_LOG.md sections 2 and 3)
| Row | Status and "Ratify at" (after the freeze) | Tracking note added |
|---|---|---|
| VP-45 | PROPOSED; C-010, configuration design (build phase), C-002 dropped at the freeze (DR-055; scheduling only) | The legacy identity facts (section 3), Rama's inputs (section 1) and the questions of section 4. At the freeze (DR-055, scheduling only): C-002 dropped from the ratification points, as C-001 was at DR-053 |
| VP-56 | PROPOSED; Q-05, M-001, Q-01 | Per-system as well as per-environment separation (5d) |
| VP-11 | PROPOSED; C-008, L-009 | The audit row's counts and coverage (5f); errata A-011 E-8 and C-001 E-1, accepted (DR-055) |
| VP-59 | PROPOSED; C-006 to C-009, I-009 | As VP-11 |
| VP-50 | PROPOSED; M-005, before any V2 Kite use | The token in the body and in warning lines; the legacy URL still saved on a testing alert, its clean-up closed as not needed by Rama on 22-Oct (5d) |
| VP-55 | PROPOSED; L-008, M-003, build-phase standards | The body repeats the URL and token (5e) |
| VP-58 | PROPOSED; I-001, I-008, I-013, L-005, M-010 | Chartink's retries UNKNOWN (5g) |
| Q-46 | OPEN (new); the Section C end decision paper | DR-028's scope (section 2) |
