# C-003 - LEGACY WEBHOOK AUTHENTICATION AND SECRET VALIDATION

Status: FROZEN 2026-10-22 (revision 1, DR-056). Revision 0 (Claude, 22-Oct-2026) was drafted on ChatGPT's instruction of 22-Oct-2026 (C-003_CLAUDE_INSTRUCTION_TO_DRAFT_MANUAL_REVISION_0_22-OCT-2026.txt) and finalised as a review draft on the instruction C-003_FINALISE_REVISION_0_FOR_CHATGPT_REVIEW_22-OCT-2026.txt (section 25.3); ChatGPT's independent review of revision 0 (22-Oct-2026) found it "technically strong", asked that its qualifications be kept, and gave "No freeze yet" until its freeze checklist was done under Rama's explicit instruction (section 29). Rama approved the freeze on 22-Oct-2026 ("Approved and Freeze"), recorded in DR-056 together with the four accepted errata of section 24, which the freeze applied to B-007, A-008, A-010 and C-002. Revision 1 changed only what section 29 lists; no finding changed.
Date: 2026-10-22 (revision 1; revision 0 drafted between about 15:50 and 17:08 IST and finalised as a review draft at 18:44 IST; frozen 2026-10-22, DR-056)
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1); no V2 design, remediation plan or recommendation about require_hmac is made here. No VM, service, host, broker, webhook, endpoint or Chartink operation and no change to legacy source; the legacy system is DEAD / REFERENCE ONLY (both VMs terminated and deleted and the Chartink subscription expired: Rama, 22-Oct-2026, C-002 section 4.3).
Built from: Claude's three research files of 22-Oct-2026, as confirmed by the independent final QA (C-003_CLAUDE_REPOSITORY_ANSWERS_22-OCT-2026.txt, "the answers file"; C-003_SOURCE_INVENTORY_AND_METHOD_22-OCT-2026.txt, "the source inventory"; C-003_RESEARCH_NOTES_AND_OPEN_QUESTIONS_22-OCT-2026.txt, "the notes file"); the QA report (C-003_INDEPENDENT_FINAL_QA_22-OCT-2026.txt) and the corrections it led to; the masking-method decision note; the erratum decision table and Claude's response file in their revisions of 15:40 IST; ChatGPT's value-added review (12:40 IST), its final review and its decision reply (both afternoon) and its drafting instruction; Rama's answers and decisions of 22-Oct-2026; and, for revision 1, ChatGPT's independent review of revision 0 and Rama's approval of the freeze (section 29). ChatGPT's follow-up review was withdrawn in its chat with Rama and is not used. Names as received (DR-008).
Labels: FACT (SOURCE-VERIFIED) = trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb ("the baseline", dated 2026-09-11 in C-002's header), read on 2026-10-22 through the masking filter; "path lines A to B" are line numbers there. FACT (CONFIG) = declared in a configuration or unit file at the baseline; not the installed file on a VM. FACT (TEST SOURCE) = what a test file's code says at the baseline, inspected and not run; never evidence of production behaviour (the research files label these FACT (SOURCE-VERIFIED) under the same rule, answers file section 0.1). FACT (RECORD-DERIVED) = a dated record in the baseline repository, true for its own date and scope only; it shows what the record says, not independent proof that the event happened as described; code line numbers quoted in a record refer to the code of its date. HISTORICAL (Rama) = his answers, dated. CURRENT EXTERNAL FACT = checked on 2026-10-22 outside the legacy repository (W-5): the pinned library source, or a local probe of the pinned library versions with no legacy code, no socket and no network; never evidence of what either VM ran. INTERPRETATION / AUDIT LEAD = reasoned from source, configuration or records; not observed. KNOWN DEFECT / AMBIGUITY = a defect, an inconsistency or documentation drift visible in source or records ("doc drift"; where code and a comment disagree, the code is what ran). UNKNOWN / NOT ESTABLISHED = the inspected evidence does not establish the claim. OPEN QUESTION = a question carried to a named destination (a design-log Q- or VP- row, or another topic), not answered here. Evidence is claim-specific (W-7). Frozen V2 manuals are cross-referenced by section ("A-010 section 10") and are not reopened, except through the errata of section 24 (ACCEPTED and applied at the C-003 freeze, DR-056).
Search scopes behind negative statements (answers file section 0.3, reusing C-002's lists): S-ALL = the 1,262 files checked out; S-RT = the 281 .py files outside tests/ and ops_dashboard/tests/; S-CODE = the 869 code and configuration files. A negative reads "not found in <scope>"; a text search cannot exclude behaviour reached by dynamic names, by other processes, or by installed files that differ from the repository, and nothing outside the checked-out paths was searched.
Security: no credential, token or token-derived value, token-bearing URL, account or client identifier, host or address, mailbox, SSH detail, or person name other than Rama's is reproduced. Two well-known addresses are named by their class only (the all-interfaces address and the loopback address), each confirmed value-blind in the research. Webhook URLs are described by their form only. The legacy token is treated as EXPOSED (DR-055; VP-50) and is repeated nowhere; nothing here reconstructs, hashes or tests it.

## 1. Purpose and boundary
C-003 records, at the baseline and in the dated records:
- the secret: its name and type, how it reached the trading process, the start-up and pre-flight presence checks, and what a missing or empty value did;
- require_hmac: what the configuration declares, how the receiver resolved it, and the constructor's guard;
- the authentication decision, branch by branch, with its exact predicates and comparisons;
- POST /webhook/<scanner_name> and the receiver's GET /health, each traced on its own: stage order, answers, audit rows and log lines;
- the HTTP status codes of the two routes and what each one shows;
- what the error answers reveal, and where the token appeared outside the check (the URL, the echoed body field, the stored copy, the warnings);
- transport, as far as it bears on authentication;
- the dated records of the secret's rotation and exposure, with their conflicts left standing;
- the tests (inspected, not run), the library probes, the known defects, the audit leads, the unknowns and the negative claims, each with its scope.

Out of scope, cross-referenced only: where the secret was stored and how the environment file was built (A-010, frozen); the start order and the start-up checks as a whole (A-011, B-002, B-006 and B-007, frozen); the scanner definitions and the connectivity checks (C-001, frozen); the scanner-to-webhook path, the full gate table, the request audit's coverage and the network path (C-002, frozen); field and symbol validation (C-004); deduplication and the in-flight claim (C-005); the queue, persistence, processing and expiry (C-006 to C-009); the scanner-to-strategy mapping (C-010); the EOD route beyond its place in the gate order (C-011); the separate health server on port 8080 (B-007 section 6); the dashboard's own authentication (answers file section 4.1); and V2's authentication design (Rama, Q12 (d); DR-001).

## 2. Evidence
| Evidence | What it covers | Limit |
|---|---|---|
| Baseline source, configuration and tests | The files listed in the source inventory, section 6 (blob IDs, ranges, the code read log and Claude's final reads); searches T01 to T27, count-only, with their scopes (source inventory, section 5); C-002's searches S03, S04, S09, S19 to S21 and S25, reused by citation | Source is not proof of what was installed or ran on either VM on any date (U-01) |
| Dated records in the production repository | 39 records, dated from about 21-Apr to 04-Aug-2026 (one guide undated), read by a separate records agent through the stage-3 mask; each load-bearing statement then tested by Claude value-blind (source inventory, section 7) | Each true for its own date and scope; the legacy database and logs were never opened; host records are read value-blind only since Rama's decision of 22-Oct (section 25) |
| Library source and local probes | waitress 3.0.2 and Flask 3.1.3 source; three probes under Python 3.13.16 with Flask 3.1.3, Werkzeug 3.1.9 and pydantic 2.13.0, with no legacy code, no socket and no network (source inventory, section 8) | The probed versions only; the VMs' installed versions are UNKNOWN (C-002 U-6) |
| Frozen V2 manuals at V2 main d89164d | 25 manuals, 8 addenda and the 2 section-end papers, with the design log's Q and VP rows, searched by a separate agent (203 statements in 14 of the 36 files); every cited line re-read by Claude (source inventory, section 9; section 23 here) | Not reopened except through section 24 |
| Rama's answers | The answer sheet of 22-Oct-2026, Q1 to Q12 (section 22) | His account; a recollection that is unavailable neither contradicts nor proves a record (C-13) |
| Reviews of the research | An independent check of the review copies (28 findings) and an independent final QA of the final bytes (33 findings, then 5 re-check points; all resolved; one optional suggestion, S-01, left as is by Rama) (source inventory, sections 12.5 and 12.8; the QA report) | Checks of the research files, not of this manual |
| Not inspected | The testing snapshot and the records repository (Q12 (c)); Chartink (no contact); the legacy VMs (terminated and deleted); the never-open paths (.env files, the accounts files, config/security.yaml, config/reference_data/, docs/web_claude/, token and session files, databases and logs) | - |

The process exceptions of the research (one working-output exposure in a records agent's context, which reached no file, and the deviations disclosed with it), Rama's decisions on them, and the drafting's one disclosed deviation (two value-blind operations on host records) are in section 25. A complete list of the legacy files and records cited is in section 26.

## 3. Terms
The legacy code and records use some of these words loosely; this manual uses each in one sense.

| Term | Meaning here |
|---|---|
| The secret | The value of the environment variable WEBHOOK_SECRET, which the receiver holds as self._secret |
| The token | The secret sent as the query parameter token (?token=<secret>); the method the pinned configuration names for Chartink |
| The signature | The header X-Webhook-Signature in the form "sha256=" followed by the lower-case hex of an HMAC-SHA256 made with the secret as its key |
| A credential | The token or the signature |
| Receiver | WebhookReceiver's Flask app in the trading process, served by waitress on port 5000 (C-002 section 6) |
| The receiver's /health | Its own GET /health on port 5000 (B-007's "WEBHOOK /health") |
| The health server | The separate server on port 8080 with its own GET /health (B-007 section 6); not C-003's subject |
| S, R, H, T | Secret configured; require_hmac as resolved; the signature header; the token parameter (section 6.4) |
| B0 to B4 | The branches of _authenticate, in the order they run (section 6.2) |
| Pass, refusal | _authenticate returned (True, "") or (False, reason) |
| Audited | A webhook_audit row was written: for a request that reached the handler and whose body was read, unless the insert failed (C-002 section 10.3) |
| C-2, G.1, AB-910, S4, P1, C6, S-1A, S-1B.1, S-1B.2 | Legacy change labels used in code comments and records, dated in section 13 (S-1B.1 in 11.4); not the topic IDs C-001 to C-003 or this manual's conflicts C-01 to C-13 |
| R-, K-, L-, U-, N-, C- items | Records (section 13), known defects (17), audit leads (18), unknowns (19), negative claims (21) and conflicts between sources (20); the IDs of the research files, kept unchanged |
| T01 to T27; steps 1 to 10 | The research's searches (source inventory, section 5); the rows of the trace table (16.2) |
| E-1; D-1 to D-4 | The research's disclosed process exception and deviations (25.2; source inventory, section 4); E-1 is not an erratum number |
| PC-1 to PC-7; F-32; N-01 to N-05; S-01 | Corrections to the research files that Rama approved (PC-), a finding of the final QA (F-), its re-check points (N-) and its optional suggestion (S-) (section 2; 25.3); N-01 to N-05 are not the negative claims N-1 to N-11 |

## 4. The secret: name, type, wiring and start-up checks (FACT, SOURCE-VERIFIED unless marked)

### 4.1 Name and type
The receiver's constructor takes secret_token, an optional string defaulting to None, and stores it unchanged as self._secret (both lines confirmed value-blind: the masked parts are the parameter's type annotation and the parameter name). The module docstring's WR3 lists secret_token=None as a constructor argument. Evidence: signals/webhook_receiver.py lines 152 to 162; signals/webhook_receiver.py line 190; signals/webhook_receiver.py lines 13 to 14.

### 4.2 Process wiring
- main.py constructs the receiver once, with secret_token=os.environ.get("WEBHOOK_SECRET"): the value is read from the process environment at that moment and never re-read, so a change to the environment after start-up had no effect until the next start. Evidence: main.py lines 3406 to 3415.
- Outside tests, the only literal environment read of WEBHOOK_SECRET is main.py line 3414 (search T26 in S-ALL also finds three crash-test sender tools and four records); the start-up check and the two pre-flight checks read it through lists of names (4.4, 4.5). No runtime code assigns it by name (N-5), but a name search cannot see a generic loader: the import-time load of 4.3 can add it.

### 4.3 How the value reaches the environment
- FACT (CONFIG): the repository's systemd unit names an EnvironmentFile, the deployment's .env file. Evidence: deploy/systemd/trading-system.service line 13.
- main.py has no dotenv call of its own (a filter grep), but it imports scripts.zerodha_login at module level, and that module calls load_dotenv() at module level, so the service process also loads .env as it starts, before the presence check and the construction. Evidence: main.py line 106; scripts/zerodha_login.py lines 32 to 36; requirements.txt line 14.
- A-010 erratum E-2 (accepted, DR-037) records this, with python-dotenv 1.2.2's rule as CURRENT EXTERNAL FACT checked on 2026-10-12 (not re-checked in C-003): the call keeps every variable already set and adds any that is not. Whether that ever supplied WEBHOOK_SECRET when the unit had not is NOT ESTABLISHED (A-010 section 5).
- The installed unit and the .env contents were not inspected (.env is never-open); where the secret was kept, by date, is section 13 (C-03).

### 4.4 Start-up presence check
- required_startup_secrets(primary_account_id) returns the two account API names, TELEGRAM_BOT_TOKEN and WEBHOOK_SECRET, with no mode parameter; its docstring dates WEBHOOK_SECRET's inclusion "in EVERY mode" to C-2, 02-Jul ("paper previously omitted it"; a claim in a comment, not checkable at the baseline). Evidence: main.py lines 219 to 232.
- main builds the list and passes it to run_all_startup_checks; check_required_secrets counts a name as missing when os.environ.get(name) is falsy (unset or empty), logs the name only, and the caller adds the blocking failure "missing_secrets". Evidence: main.py lines 2425 to 2430; main.py lines 2451 to 2474; utils/startup_checks.py lines 1071 to 1086; utils/startup_checks.py lines 1555 to 1558.
- A blocking failure makes main log CRITICAL and return 3 before the receiver is constructed (--dry-run returns 0 or 3 after the checks and never constructs it). How the unit treated the exit code is B-001's subject. Evidence: main.py lines 2476 to 2502.
- INTERPRETATION (from the predicate): presence means non-empty only; a value of spaces would pass. No check of length or strength was found in S-CODE (search T27; N-6), and no format check on the code path read. Presence is not strength.

### 4.5 Pre-flight presence checks
scripts/premarket_healthcheck.py and the pre-flight check RequiredSecretsCheck each test that WEBHOOK_SECRET (with two Telegram names) is set and non-empty, by name only; the second's comment reads "Ported 1:1 from premarket_healthcheck.REQUIRED_SECRETS". They run as their own processes; whether they ran on a given day is a pre-flight record question (B-007). Evidence: scripts/premarket_healthcheck.py lines 46 to 51; scripts/premarket_healthcheck.py lines 75 to 81; scripts/preflight/checks/config_integrity.py lines 26 to 31; scripts/preflight/checks/config_integrity.py lines 53 to 63.

### 4.6 A missing or empty secret: the start-up path and the receiver's test-only path
Two behaviours, kept apart:
- At a start through main.py: a missing or empty WEBHOOK_SECRET is the blocking failure "missing_secrets"; main returns 3 and the receiver is never built (4.4). On main.py's code path at the baseline, a start with a missing or empty secret ended before any receiver existed.
- In the receiver itself: if it is built with no secret (None or ""), _authenticate returns success for every request on both routes (branch B0, section 6.2). Its comment calls this "Unreachable in production ... so this is a test-only path"; on main.py's code path that holds, because main constructs the receiver only after the presence check has passed, and no code assigns the name between the two reads (the import-time load of 4.3 runs before both; 4.2, 4.4, N-5). The comment's "main.py:226" is an old line number (K-06). Evidence: signals/webhook_receiver.py lines 326 to 331.
- INTERPRETATION: the guarantee is a code-path property of main.py; it does not cover a receiver built by any other entry point (none found in S-ALL: search T09 lists main.py as the only construction outside tests; N-10). Most receiver test fixtures build it with no secret (FACT (TEST SOURCE); section 15; K-05).
- The production endpoint is therefore not described here as "open": on main.py's code path at the baseline, no receiver is built without a secret (4.4). Before C-2 (02-Jul), paper mode did not require the secret at start-up, by the docstring of 4.4 (a claim in a comment) and the 02-Jul record (R-11); the code of that period is not in the snapshot (as in U-08), so whether a receiver then ran without a secret is not established (U-01). The 04-Aug record that calls commenting the secret out "disabling an auth control" is set against the code path in section 20 (C-09).

### 4.7 No mode branch
No mode branch in _authenticate, the route registration or _process_request (inspected; a test asserts the absence of four mode words in the three functions, FACT (TEST SOURCE)); the presence check has no mode parameter (4.4). Evidence: signals/webhook_receiver.py lines 298 to 354; signals/webhook_receiver.py lines 360 to 413; signals/webhook_receiver.py lines 536 to 745; tests/unit/test_c6_auth_collapse.py lines 425 to 438. Paper and live: section 14.

## 5. require_hmac: declaration, resolution and the constructor guard

### 5.1 Declaration (FACT, CONFIG)
The webhook section declares require_hmac: false with the comment "Chartink cannot sign payloads; use ?token= auth (WEBHOOK_SECRET required in BOTH modes as of C-2 02-Jul)". The installed file's value on either VM is UNKNOWN (U-01). Evidence: config/system_config.yaml line 336.

### 5.2 Schema (FACT, SOURCE-VERIFIED)
WebhookConfig (extra="forbid") has require_hmac: bool with no default, so the YAML must declare it; SystemConfig.webhook is a required WebhookConfig; AppConfig, which main passes to the receiver, holds system and has no webhook field. Evidence: core/config_loader.py lines 617 to 621; core/config_loader.py line 2002; core/config_loader.py lines 2420 to 2434.

### 5.3 Resolution in the receiver (FACT, SOURCE-VERIFIED unless marked)
- The constructor looks for config.webhook, then config.system.webhook, and takes bool(getattr(..., "require_hmac", False)): an absent section or key means False ("Absent -> treated as non-strict"). It keeps the result as self._require_hmac for request time (G.1, 25-Apr). Evidence: signals/webhook_receiver.py lines 163 to 176; signals/webhook_receiver.py lines 196 to 200.
- CURRENT EXTERNAL FACT (pydantic 2.13.0 probe of the same model shape): getattr on the AppConfig shape for "webhook" returns the default None, and the nested value resolves as declared (false gives False).
- INTERPRETATION: a receiver that main.py builds from the pinned configuration resolves _require_hmac to False; the installed configuration is U-01.

### 5.4 The constructor guard (BL-18) (FACT, SOURCE-VERIFIED)
If require_hmac resolves True and secret_token is falsy, the constructor raises ValueError naming WEBHOOK_SECRET; with require_hmac False, construction without a secret is accepted (tests pin both: FACT (TEST SOURCE)). The guard runs at construction only; request-time enforcement is _authenticate (section 6). The unit tests' own comment says the BL-18 tests are "CONSTRUCTOR-ONLY" and do not exercise request-time enforcement (FACT (TEST SOURCE)). Evidence: signals/webhook_receiver.py lines 177 to 182; tests/unit/test_webhook_receiver.py lines 1106 to 1114; tests/unit/test_webhook_receiver.py lines 1144 to 1175.

### 5.5 Comments, defaults and tests that disagree with the declaration
The declaration is what the pinned files say; the code acts on the resolved value.
- KNOWN DEFECT / AMBIGUITY (K-02; doc drift, recorded in C-002 as K-6): the bind comment two lines above says the all-interfaces bind "relies on require_hmac=true plus firewall/security-group restrictions". Evidence: config/system_config.yaml lines 328 to 334.
- KNOWN DEFECT / AMBIGUITY (K-02; doc drift, recorded in C-002 as K-5): the G.1 comments say tokens in the URL are "logged by nginx"; no nginx is configured in the repository and a 03-Jul record found none on the VM (C-002 section 5.4). Evidence: signals/webhook_receiver.py lines 196 to 199; signals/webhook_receiver.py lines 307 to 310.
- FACT (TEST SOURCE; a test comment, 17-Jul): "LATENT when written (require_hmac: false in prod)", and "this arms the moment that flag flips, which is an open operator action." A test comment's claim about production, not a record of the installed file; and an intent (a flip) that the configuration's own comment ("Chartink cannot sign payloads") argues against. Evidence: tests/unit/test_fix134_backpressure.py lines 239 to 241.
- INTERPRETATION / AUDIT LEAD (L-03): on the code path, a flip to true with Chartink sending no signature would have answered every Chartink POST 401 ("HMAC signature required ..."); the 20-Jul record says the same (R-15; conflict C-04).
- KNOWN DEFECT / AMBIGUITY (K-01; doc drift, minor): the module docstring's WR8 says "Optional HMAC validation via X-Webhook-Signature header" and does not mention the query token, the method the configuration names. Evidence: signals/webhook_receiver.py line 19.

## 6. The authentication decision: _authenticate (FACT, SOURCE-VERIFIED unless marked)

### 6.1 Location and callers
WebhookReceiver._authenticate(hmac_payload: bytes) returns (True, "") or (False, reason). Since C6 (25-Jul) it is the only place either comparison is made: /webhook calls it with the raw body, the receiver's /health with b"". No other caller in S-ALL: search T01 finds the receiver, its tests and records and, because the search matches part of a word, a differently named dashboard function (is_authenticated), its test file and an integration-test fixture name; none calls the receiver's method, and the dashboard's authentication is not C-003's subject. Evidence: signals/webhook_receiver.py lines 298 to 325; signals/webhook_receiver.py line 387; signals/webhook_receiver.py line 547; ops_dashboard/backend/auth.py line 256.

### 6.2 Branch order and exact predicates
| Branch | Predicate, in order | Result |
|---|---|---|
| B0 | not self._secret (None or "") | (True, ""). No header or query is read. The test-only path (4.6) |
| - | then reads sig_header = request.headers.get("X-Webhook-Signature", "") and token_param = request.args.get("token", "") | - |
| B1 | sig_header.startswith("sha256=") | expected = HMAC-SHA256(key = self._secret.encode() [UTF-8], message = hmac_payload).hexdigest() (lower-case hex); compare_digest(sig_header[7:], expected): unequal gives (False, "HMAC signature mismatch"), equal gives (True, ""). Terminal: the token is not consulted, so a valid ?token= cannot rescue a wrong signature |
| B2 | self._require_hmac (no "sha256=" header) | (False, "HMAC signature required (require_hmac=True); token param is not accepted"). The token is not consulted |
| B3 | token_param non-empty | compare_digest(token_param, self._secret): unequal gives (False, "Invalid token"), equal gives (True, "") |
| B4 | otherwise | (False, "Missing auth: provide X-Webhook-Signature header or ?token= param") |

Evidence: signals/webhook_receiver.py lines 326 to 354; tests/unit/test_c6_auth_collapse.py lines 184 to 194; tests/unit/test_webhook_receiver.py lines 1269 to 1287; tests/unit/test_fix134_backpressure.py lines 315 to 322.
- B1's terminal rule: the code comment says the same; one test isolates it, with require_hmac False (the C6 test above); two tests run with require_hmac True, where B2 would refuse the token anyway, so they do not isolate it (the G.1 test and its /health mirror above). These are FACT (TEST SOURCE).

### 6.3 Predicate details
- The prefix test is case-sensitive: "SHA256=..." is not a signature and falls to B2, B3 or B4 (CURRENT EXTERNAL FACT, probe: Python's str.startswith). The hex must be lower case to match hexdigest().
- A header present without the prefix is ignored: with require_hmac False it is the token (B3) or B4 that decides.
- An empty ?token= gives "" and falls to B4; when the parameter repeats, request.args.get returns the first value (CURRENT EXTERNAL FACT, probe of Werkzeug 3.1.9).
- What is signed: on /webhook the exact bytes request.get_data() returned (the raw body as received, read before any parsing; not parsed or re-serialised JSON); on the receiver's /health the empty byte string. Neither the path, the method, the query, a time stamp nor a nonce is covered. Evidence: signals/webhook_receiver.py line 480; signals/webhook_receiver.py line 511; signals/webhook_receiver.py line 387.
- Both comparisons are hmac.compare_digest on two str values. CURRENT EXTERNAL FACT (probe, Python 3.13.16): compare_digest raises TypeError ("comparing strings with non-ASCII characters is not supported") when either string has a non-ASCII character; through Flask's test client, a query value such as %C3%A9 arrives as a non-ASCII str, and so does a header byte above 0x7F (Werkzeug decodes headers as latin-1). On the serving path waitress parses the request first; that path was not probed. _authenticate does not catch the error. What each route then answers: section 7.3 row 10 and section 8.2 row 5 (K-03, an INTERPRETATION from the source and a library probe, not an observed incident).

### 6.4 Decision table
S = secret configured; R = require_hmac as resolved; H = the X-Webhook-Signature header; T = the token query parameter. "Pass" means _authenticate returned True; what follows depends on the route.

| # | S | R | H | T | Branch | Result | /webhook | Receiver's /health |
|---|---|---|---|---|---|---|---|---|
| 1 | no | False | any | any | B0 | pass (test-only path) | continues (7.3) | 200 with state |
| 2 | yes | any | "sha256=" + correct hex | any | B1 | pass | continues | 200 with state |
| 3 | yes | any | "sha256=" + anything else | any (valid or not) | B1 | "HMAC signature mismatch" | 401, that message | 401 uniform |
| 4 | yes | any | "sha256=" + a non-ASCII character | any | B1 | TypeError raised | 500 (audited) | 500 (handler) |
| 5 | yes | True | absent or other form | any | B2 | "HMAC signature required ..." | 401, that message | 401 uniform |
| 6 | yes | False | absent or other form | non-empty, equal | B3 | pass | continues | 200 with state |
| 7 | yes | False | absent or other form | non-empty, unequal | B3 | "Invalid token" | 401, that message | 401 uniform |
| 8 | yes | False | absent or other form | non-ASCII | B3 | TypeError raised | 500 (audited) | 500 (handler) |
| 9 | yes | False | absent or other form | absent or empty | B4 | "Missing auth: ..." | 401, that message | 401 uniform |

- Correct hex is the lower-case HMAC-SHA256 of the route's bytes: the raw body on /webhook, b"" on the receiver's /health. A signature over a non-empty body does not pass /health, and the /health signature does not pass /webhook when the body is non-empty (two tests, both with a non-empty body, FACT (TEST SOURCE)); for an empty POST body the two routes sign the same bytes, so the same signature passes both (INTERPRETATION from the source, not run; 6.5).
- Rows 3 and 7 are for ASCII values; a non-ASCII value is rows 4 and 8, INTERPRETATION from the source and the probe (the legacy code was not run). Row 1 has R False: BL-18 refuses require_hmac True without a secret at construction (5.4).
- The pinned configuration gives R = False (5.3), so rows 6 to 9 were the token rows at the repository level; whether they were the rows of either installed VM is U-01.

Evidence: signals/webhook_receiver.py lines 326 to 354; signals/webhook_receiver.py lines 177 to 182; signals/webhook_receiver.py line 480; signals/webhook_receiver.py line 387; tests/unit/test_c6_auth_collapse.py lines 262 to 280.

The decision as a flow (source wiring at the baseline):
```
_authenticate(hmac_payload)                     # /webhook: raw body;  receiver's /health: b""
  |
  +-- B0  secret None or "" ?  -- yes --> PASS (test-only path: main.py builds no receiver without a secret, 4.6)
  |        no
  +-- read X-Webhook-Signature (H) and ?token= (T)
  |
  +-- B1  H starts "sha256=" ? -- yes --> compare_digest(H[7:], HMAC-SHA256(secret, payload))   [terminal]
  |        no                            equal: PASS   unequal: "HMAC signature mismatch"
  |                                      non-ASCII: TypeError, uncaught (K-03)
  +-- B2  require_hmac True ?  -- yes --> "HMAC signature required ...; token param is not accepted"
  |        no
  +-- B3  T non-empty ?        -- yes --> compare_digest(T, secret)
  |        no                            equal: PASS   unequal: "Invalid token"
  |                                      non-ASCII: TypeError, uncaught (K-03)
  +-- B4  "Missing auth: provide X-Webhook-Signature header or ?token= param"
```

### 6.5 Replay and binding (INTERPRETATION / AUDIT LEAD, L-01; from 6.2)
- The receiver's /health: the valid signature is HMAC-SHA256(secret, b""), a constant; whoever saw one header could reuse it indefinitely (the docstring says so: "a CONSTANT for a given secret and indefinitely replayable").
- /webhook: a signature binds the body, not the path, so a captured signed body would pass authentication on any /webhook/<key> path. On the intraday path, scan_name in the body, when present, must still match the path after authentication (C-002 section 8.3), and repeats are limited after authentication by the expiry check, the dedup cache and the fingerprint, not by the authentication decision. The EOD route checks none of these: it parses the body, checks three fields and triggered_at and hands each symbol to the capture worker, so a captured body with those fields is accepted again on any "eod" path (the capture worker's own handling was not read).
- Empty body: the constant /health signature also authenticates a POST to /webhook whose body is empty (6.4). Such a request gets whichever answer after authentication applies (the unknown-scanner 404, which tells whether a name is a configured scanner; the EOD route; the kill-switch 403; the backpressure 503 with its queue-depth header; the window 403), and otherwise ends with a 400 for malformed JSON (two WARNINGs on the intraday path, one on an "eod" path, each holding the empty body); no signal can be accepted this way. The docstring's "body-bound" has this exception. A source-derived reading, not observed traffic.
- The query token: a static bearer value; anyone holding the URL could authenticate any body to any path and read the receiver's /health state. Whether anyone other than Chartink and the operator's tools held it is UNKNOWN (U-06).
Evidence: signals/webhook_receiver.py lines 318 to 321; signals/webhook_receiver.py lines 547 to 593; signals/webhook_receiver.py lines 617 to 633; signals/webhook_receiver.py lines 916 to 954; signals/webhook_receiver.py lines 751 to 804.

## 7. POST /webhook/<scanner_name>: stage order, answers, audit and logs (FACT, SOURCE-VERIFIED unless marked)

### 7.1 Route and hand-off
The rule /webhook/<scanner_name> (POST) calls _handle_webhook, which reads the source address and the body, applies the shutdown and per-address gates, then calls _process_request inside try/except/finally; the finally clause writes the audit row. C-002 section 9 gives the whole gate table; this section adds what concerns authentication. Evidence: signals/webhook_receiver.py lines 406 to 408; signals/webhook_receiver.py lines 477 to 534.

### 7.2 Before authentication
- The framework's own answers (a wrong path, method or slash form; OPTIONS, answered 200) and a body over 1 MiB, raised as 413 when the body is read at signals/webhook_receiver.py line 480, come before authentication and before any audit write (C-002 section 9 rows 1 and 2; C-002 section 10.3, NR-1 and NR-2). For the framework's answers C-002 section 7.3 is a CURRENT EXTERNAL FACT (a local probe of the probed versions).
- Shutting down: 503 {"error": "Service shutting down; retry later"}, audited (503, 0, 0); no credential is read.
- Per-address limit (enabled by the pinned configuration: burst 60, refill 5 per second): 429 {"error": "Rate limit exceeded; slow down"}, audited (429, 0, 0), and a WARNING naming the scanner and the source address. The limiter instance is shared with the receiver's /health and keyed by request.remote_addr (8.1), so unauthenticated requests to either route spend the same address's tokens (INTERPRETATION from the shared attribute; L-05).
- The queue configuration is read before authentication (signals/webhook_receiver.py line 537; no request data involved).
Evidence: signals/webhook_receiver.py lines 477 to 504; signals/webhook_receiver.py line 537; config/system_config.yaml lines 340 to 342.

### 7.3 From authentication on
Bodies are JSON. "Audit row" gives (response_code, accepted, rejected).

| # | Stage | Answer | Audit row |
|---|---|---|---|
| 1 | _authenticate(raw_body) refuses (6.4) | 401 {"error": <the branch's message>}; no log line | (401, 0, 0) |
| 2 | Unknown scanner (after authentication) | 404 {"error": "Unknown scanner: '<path segment>'"} | (404, 0, 0) |
| 3 | An "eod" scanner (after authentication) | The EOD route: 400s or 200 (C-002 section 9 row 7) | As answered |
| 4 | Kill switch active | 403 {"error": "Kill switch active; signals rejected"} | (403, 0, 0) |
| 5 | Backpressure | 503, header X-Queue-Depth | (503, 0, 0) |
| 6 | Outside the entry window | 403 {"error": "Outside entry window"} | (403, 0, 0) |
| 7 | Malformed JSON | 400 {"error": "Malformed JSON: <parser message>"}; two WARNINGs | (400, 0, 0) |
| 8 | Other parse and field checks | 400 {"error": ...}; the raw-body WARNING (note b) | (400, 0, 0) |
| 9 | The batch | 200, or 503 if a symbol is QUEUE_FULL or STORE_ERROR | (200, a, r) or (503, 0, 0) |
| 10 | An exception from _process_request, including a TypeError from compare_digest (6.4 rows 4 and 8) | 500 {"error": "Internal server error"}; one CRITICAL line (note a) | (500, 0, 0) |

- (a) The CRITICAL line reads "Unhandled exception processing /webhook/<name>: <exception text>".
- (b) Four of these 400s (type cast, missing field, scan_name mismatch, invalid triggered_at) also log a WARNING of their own, without the raw body, so they leave two lines; the others leave the raw-body WARNING only. Rows 7 and 8 are the intraday path; the EOD route's 400s (row 3) log no line of their own (11.5).
- Order: authentication runs before the unknown-scanner check and the EOD route (their comment: "so neither can be reached unauthenticated"; two tests, FACT (TEST SOURCE)), and before the kill-switch, backpressure and entry-window gates; the shutdown gate and the per-address limiter, which also depend on state, run before it (7.2).
- INTERPRETATION (row 10): a non-ASCII credential is answered 500 and recorded as a 500, not as a 401, and the critical line carries the library's message, which holds no credential (K-03). No 500 is recorded in C-002's windows (U-09).
Evidence: signals/webhook_receiver.py lines 539 to 583; signals/webhook_receiver.py lines 585 to 674; signals/webhook_receiver.py lines 676 to 745; signals/webhook_receiver.py line 66; signals/webhook_receiver.py lines 510 to 528; tests/unit/test_c6_auth_collapse.py lines 361 to 379.

### 7.4 The audit row
_write_audit inserts ts, scanner_name (the path segment, unknown names included), source_ip, payload_size_bytes, response_code, signals_accepted, signals_rejected and duration_ms. It stores no query string, header, credential, failure reason or body; a 401 row therefore shows that authentication failed but not which branch. A failed insert logs an error and the answer is unchanged, so the row is attempted, not guaranteed. A test pins this (test_d4_webhook_audits_a_401_but_health_does_not; FACT (TEST SOURCE)). Evidence: signals/webhook_receiver.py lines 1126 to 1154; signals/webhook_receiver.py lines 529 to 534; tests/unit/test_c6_auth_collapse.py lines 382 to 394. Which requests leave no row at all: C-002 section 10.3 (NR-1 to NR-4). Since when 401s were recorded: U-08.

### 7.5 Logs on this route that touch credentials
The logger "webhook_receiver" (named at main.py line 3413) writes to the daily system log (INFO and above, JSON), the daily debug log (DEBUG and above) and stdout (WARNING and above), which the repository unit sends to the journal.
- No line is logged for any 401: _authenticate logs nothing and the 401 is returned directly (the receiver's log calls, a filter grep; L-05).
- The 429 WARNING names the source address.
- The 400 WARNING logs repr(raw_body[:1000]) and the malformed-JSON WARNING repr(raw_body[:500]): the raw, unsanitised bytes, after authentication passed (11.5). Four field-check 400s log a WARNING of their own that holds no raw body (7.3 note b).
- The CRITICAL of row 10 logs the exception's text.
- No receiver code reads request.url, full_path or query_string (search T07 in S-RT: the receiver's one hit is request.args.get("token")), so no receiver log line carries the query (N-2).
Evidence: signals/webhook_receiver.py lines 495 to 504; signals/webhook_receiver.py lines 518 to 528; signals/webhook_receiver.py lines 585 to 593; core/logger.py lines 399 to 421; deploy/systemd/trading-system.service lines 37 to 40.

### 7.6 Next consumer
After authentication and the gates, for each symbol that reaches the INSERT: a signals row (status QUEUED, webhook_payload = the sanitised copy; re-marked QUEUE_FULL if the queue is full) and a tuple on the in-memory queue for SignalProcessor (C-004 to C-009). Excluded, invalid, expired, in-process and duplicate symbols return before the INSERT. An eod scanner goes to the capture worker (C-011). webhook_audit is read by the shutdown census and the dashboard (C-001 section 8; C-002 section 10.7). Evidence: signals/webhook_receiver.py lines 688 to 728; signals/webhook_receiver.py lines 904 to 954; signals/webhook_receiver.py lines 963 to 980; signals/webhook_receiver.py lines 1046 to 1068.

### 7.7 Sequence (source wiring at the baseline, not host execution)
```
Chartink or any caller --HTTP POST /webhook/<key>?token=<secret>--> waitress (port 5000) --> Flask rule
  Flask: wrong path, method or slash form -> 404 / 405 / 308; OPTIONS -> 200
         (none of these enters the view; no audit row)                                        [C-002 7.3]
  _handle_webhook:
    remote_addr; request.get_data()      body > 1 MiB -> 413 at the read (no audit row)       [NR-2]
    shutting down?                       -> 503, audited
    per-address bucket empty?            -> 429, audited, WARNING with the address
    _process_request:
      _authenticate(raw body)            refusal -> 401 + branch message, audited, no log line
                                         TypeError (non-ASCII credential) -> 500, audited, CRITICAL
                                         (K-03; not probed through waitress)
      unknown key?                       -> 404, audited
      "eod" key?                         -> EOD route (400 or 200), audited as answered
      kill / backpressure / window       -> 403 / 503 / 403, audited
      parse and field checks             -> 400, audited, raw-body WARNING (first 1,000 bytes)
      per symbol: sanitised copy -> signals row (QUEUED) + queue tuple -> SignalProcessor
    finally: webhook_audit row (attempted; a failed insert logs an error)
```

## 8. The receiver's GET /health (port 5000): stage order, answers, audit and logs (FACT, SOURCE-VERIFIED unless marked)

### 8.1 Stage order
1. source_ip = request.remote_addr or "unknown".
2. The per-address limit, the same _ip_limiter instance as /webhook, "BEFORE auth, exactly as /webhook does": 429 {"error": "rate limit exceeded"} (a different text from /webhook's 429).
3. _authenticate(b""): any refusal gives 401 {"error": "authentication required"}; the reason is discarded ("naming it here would tell an anonymous caller whether require_hmac is on").
4. A pass gives 200 {"status": "ok", "kill_switch_active", "queue_size", "queue_capacity", "queue_depth"}.

There is no shutdown check (the route reads no _shutting_down) and no try block. Evidence: signals/webhook_receiver.py lines 364 to 404.

### 8.2 Answers by branch
| # | Case | Answer |
|---|---|---|
| 1 | Over the per-address limit | 429 {"error": "rate limit exceeded"} |
| 2 | 6.4 rows 3, 5, 7 and 9 (any refusal) | 401 {"error": "authentication required"}, identical for every branch |
| 3 | 6.4 rows 1, 2 and 6 (a pass) | 200 with the kill-switch state and the queue depth |
| 4 | During shutdown | As rows 1 to 3 (INTERPRETATION: no shutdown gate on this route) |
| 5 | 6.4 rows 4 and 8 (TypeError) | 500 {"error": "Internal server error"} from the app's 500 handler, which logs CRITICAL "Unhandled exception in webhook handler: ..." |

Row 5 is INTERPRETATION from the source and a probe. CURRENT EXTERNAL FACT (Flask 3.1.3 probe, TESTING False as the receiver sets it): an exception raised in a view is passed to the registered 500 handler, and Flask's own log line reads "Exception on <path> [GET]", the path without the query (K-03). Evidence: signals/webhook_receiver.py lines 410 to 413; signals/webhook_receiver.py line 250.

### 8.3 Audit and logs
The route writes no webhook_audit row and logs nothing on 429, 401 or 200 (L-05); a test pins "/health audits nothing" (FACT (TEST SOURCE)). On those answers a request to the receiver's /health is visible only as whatever waitress and Flask record; on the 500 path the app's 500 handler also logs CRITICAL through the receiver's logger (8.2 row 5; 11.2). Evidence: signals/webhook_receiver.py lines 364 to 404; tests/unit/test_c6_auth_collapse.py lines 382 to 394; tests/unit/test_c6_auth_collapse.py line 25.

### 8.4 The empty-body signature
The signed bytes are b"" because "a GET carries no body"; the docstring states the consequence (a constant, indefinitely replayable signature) and that /webhook's is body-bound, which, by INTERPRETATION from the source (not run), does not hold for an empty POST body (6.5, L-01). Evidence: signals/webhook_receiver.py lines 316 to 324; signals/webhook_receiver.py lines 382 to 387.

### 8.5 What a 200 discloses
The kill-switch state, the queue size and the capacity. Before AB-910 section 1.7 the route gave these to any caller (its comment); the dates of the change are records (section 13, R-13 and R-14). Evidence: signals/webhook_receiver.py lines 366 to 375.

### 8.6 Its callers, and how the boot self-check read its answers
- The boot self-check: two seconds after starting waitress, main calls http://<loopback address>:<bind_port>/health with no credential through check_webhook_endpoint, which counts 2xx or 401 as reachable. The check's comment says a 401 "proves the one thing this check exists to prove: Flask is listening", so the rule is a reachability test, not a health verdict. On anything else main logs CRITICAL with the URL (which carries no token) and degrades instead of stopping (27-Jul). The check's comment says passing the token "would put the secret into a URL that main.py ... logs on failure" (its main.py line numbers are old ones, K-06). Evidence: main.py lines 4002 to 4036; utils/startup_checks.py lines 783 to 826; utils/startup_checks.py lines 803 to 804.
- INTERPRETATION (health-check reading): since the self-check sends no credential, the 401 it receives shows only that a server answered GET /health on that port; it does not show that an authenticated signal would be accepted, queued or processed (B-007 section 4 records the same limit), and it cannot show reachability from outside the VM (C-002 section 6.3).
- FACT (RECORD-DERIVED, 17-Jul): before that rule, the self-check treated the new 401 as unreachable and the start stopped; the fix counted 401 as reachable (section 13, R-14).
- The repository's sender tools: the crash-test injectors (FACT (TEST SOURCE)) and the load-test script add ?token= taken from WEBHOOK_SECRET or an argument; none sends X-Webhook-Signature (filter greps). Test tools, not production traffic. Evidence: tests/crash_test/signal_injector.py lines 24 to 29; tests/crash_test/ct143_afternoon_inject.py line 25; tests/crash_test/ct145_rapid_crash.py line 16; scripts/load_test_signals.py lines 81 to 99.
- Not this route: the separate health server on port 8080 (FIX-132), whose GET /health binds the loopback address by default and carries no webhook authentication (its comment: "(unauth) payload"). It is B-007's subject; it is named here only so that the two /health routes are not confused. Evidence: scripts/healthcheck_server.py line 86; scripts/healthcheck_server.py lines 297 to 325; main.py lines 4073 to 4082.

### 8.7 Sequence (source wiring at the baseline, not host execution)
```
boot self-check (no credential) or any caller --HTTP GET /health--> waitress (port 5000) --> Flask
  remote_addr
  per-address bucket (shared with /webhook) empty?  -> 429 "rate limit exceeded"   (no audit, no log)
  _authenticate(b"")      refusal (any branch)      -> 401 "authentication required" (no audit, no log)
                          TypeError (non-ASCII)     -> 500 via the app's handler, CRITICAL (no audit)
                                                       (K-03; not probed through waitress)
                          pass                      -> 200: kill-switch state, queue size, capacity, depth
  no shutdown gate; no try block
boot self-check: 2xx or 401 = reachable; anything else -> CRITICAL, sentinel, start continues without entries
```

## 9. HTTP status codes on the two routes
What each code shows on the code path, and from which evidence. Only 401 is an authentication answer.

| Code | POST /webhook | The receiver's GET /health | What it shows | Evidence and class |
|---|---|---|---|---|
| 200 | The batch was answered (7.3 row 9), or the EOD route's 200; also the framework's own 200 for OPTIONS, with no view and no row | Authentication passed: the state of 8.5 (OPTIONS not probed for this route) | For /webhook, what the receiver answered, not that the sender received it (C-002 section 9) | signals/webhook_receiver.py lines 676 to 745; signals/webhook_receiver.py lines 364 to 404 (SOURCE-VERIFIED); OPTIONS: C-002 section 7.3 (CURRENT EXTERNAL FACT, local probe; the probed versions only) |
| 400 | A body that failed parsing or a field check, after authentication (7.3 rows 3, 7 and 8) | Not returned | Authentication had passed | signals/webhook_receiver.py lines 585 to 674 (SOURCE-VERIFIED) |
| 401 | Authentication refused; the body names the branch (10.1) | Authentication refused; one message for every branch | The only authentication answer; the 06-Jul rule "only 401 = webhook auth/token failure" (R-04) agrees with the code | signals/webhook_receiver.py lines 547 to 549; signals/webhook_receiver.py lines 379 to 393 (SOURCE-VERIFIED) |
| 403 | The kill switch or the entry window, after authentication (7.3 rows 4 and 6) | Not returned | Not an authentication answer; a 05-Jul record's "auth-rejects" for 403s is a reading the code does not support (C-07) | signals/webhook_receiver.py lines 539 to 583 (SOURCE-VERIFIED) |
| 404 | An unknown scanner, after authentication (7.3 row 2); also the framework's answer for a path outside the rule, with no view and no row | Not returned by the route | An authenticated caller learns whether a key is configured (10.4) | signals/webhook_receiver.py lines 551 to 564 (SOURCE-VERIFIED); C-002 section 7.3 (CURRENT EXTERNAL FACT, local probe) |
| 405, 308 | The framework's answers for another method and for a doubled slash, with no view and no row | Not probed for this route | Before authentication; never audited | C-002 section 7.3 (CURRENT EXTERNAL FACT, local probe; the probed versions only) |
| 413 | A body over 1 MiB, raised at the body read, before authentication and any audit write | Not returned by the route, which reads no body (from the source; not probed) | Not an authentication answer | signals/webhook_receiver.py line 251 (the 1 MiB limit); signals/webhook_receiver.py line 480 (the read) (SOURCE-VERIFIED); the 413 itself: C-002 section 9 row 2 (its probe: CURRENT EXTERNAL FACT, the probed versions only) and section 10.3, NR-2 |
| 429 | The per-address limit, before authentication; audited; a WARNING with the address | The same limiter, before authentication, with a different text; no audit row, no log | Not an authentication answer; one bucket per address for both routes (L-05) | signals/webhook_receiver.py lines 477 to 504; signals/webhook_receiver.py lines 364 to 404 (SOURCE-VERIFIED) |
| 500 | An exception in _process_request, a non-ASCII credential included; audited; CRITICAL | A non-ASCII credential, through the app's 500 handler; CRITICAL; no audit row | INTERPRETATION from the source and a library probe, not observed (K-03, U-09); the serving path through waitress was not probed | signals/webhook_receiver.py lines 510 to 528; signals/webhook_receiver.py lines 410 to 413; the probes of the source inventory, section 8 |
| 503 | Shutting down (before authentication), backpressure, or a batch with QUEUE_FULL or STORE_ERROR | Not returned (no shutdown gate) | Not an authentication answer; the code's invitation to a retry; whether Chartink retried is UNKNOWN (C-002 section 13.1) | signals/webhook_receiver.py lines 477 to 504; signals/webhook_receiver.py lines 539 to 583; signals/webhook_receiver.py lines 676 to 745 (SOURCE-VERIFIED) |

- FACT (RECORD-DERIVED; C-002 section 14, R5 and R7, as recorded there): from 12-Jun to 23-Jul 15:29 the audited answers were 200, 403 and 503 only, with 0 for 401, 404, 429, 400 and 500; from 24 to 31-Jul, zero 400, 401, 404, 429, 500 and 503 on any day. What a zero count of 401s proves depends on when 401s began to be audited (U-08).
- The records' labels: C-07 lists where a record calls 403s "auth-rejects" and where records call them kill-switch or window refusals. This manual uses the code's meaning.

## 10. What the error answers reveal

### 10.1 The four messages and the uniform one (FACT, SOURCE-VERIFIED)
/webhook returns the branch's own message to any caller: "HMAC signature mismatch", "HMAC signature required (require_hmac=True); token param is not accepted", "Invalid token", or "Missing auth: provide X-Webhook-Signature header or ?token= param"; a test pins that the four stay distinct (FACT (TEST SOURCE)). The receiver's /health returns one message for all four; a test pins that too (FACT (TEST SOURCE)). Evidence: signals/webhook_receiver.py lines 336 to 354; signals/webhook_receiver.py lines 547 to 549; tests/unit/test_c6_auth_collapse.py lines 283 to 327.

### 10.2 The records' reason (FACT, RECORD-DERIVED, 25-Jul)
The C6 scoping notes call the difference in failure messages deliberate and the receiver's /health's uniform 401 message a "deliberate anti-oracle"; the done note records that "`/webhook` keeps all four distinct messages". Evidence: docs/audit/receiver_auth_c6_scoping_2026-07-25.md line 117; docs/audit/receiver_auth_c6_scoping_2026-07-25.md line 189; docs/audit/c6_auth_collapse_25jul2026.md line 42.

### 10.3 What /webhook discloses (INTERPRETATION / AUDIT LEAD, L-02)
The posture the receiver's /health hides is shown by /webhook: an unauthenticated POST with no credential to any /webhook path answers "Missing auth: ..." when require_hmac is False and "HMAC signature required (require_hmac=True) ..." when it is True (K-04). The records give the reason for /health only; why /webhook kept granular messages on the same port is not stated in the passages read (U-07).

### 10.4 What only an authenticated caller learns (FACT, SOURCE-VERIFIED)
The 404 for an unknown scanner and the EOD route run after authentication, so neither answers an unauthenticated caller (tests pin both: FACT (TEST SOURCE)). An authenticated caller learns whether a path segment is a configured scanner. Evidence: signals/webhook_receiver.py lines 551 to 564; tests/unit/test_c6_auth_collapse.py lines 361 to 379.

### 10.5 What the receiver's /health refusals carry (FACT, SOURCE-VERIFIED)
Its 401 and 429 carry no system state (a test checks that the four state names are absent from the 401 body: FACT (TEST SOURCE)). Evidence: signals/webhook_receiver.py lines 379 to 393; tests/unit/test_c6_auth_collapse.py lines 329 to 334.

## 11. The token outside the check: URL, echoed webhook_url, stored copy and warnings

### 11.1 In the URL's query (FACT, SOURCE-VERIFIED)
The token is read from the query string (6.2). The URL form given to Chartink (plain HTTP, /webhook/<key>, ?token=) is recorded in C-002 section 4.2 (a 15-Jul specification and Rama's account) and is not repeated here. Evidence: signals/webhook_receiver.py line 334.

### 11.2 Who logs the query (FACT, SOURCE-VERIFIED for the receiver; CURRENT EXTERNAL FACT for the libraries)
- The receiver reads no full URL, path with query or query string (7.5; N-2).
- waitress 3.0.2 splits the request target into path and query, and its own log lines name request.path only ("Client disconnected while serving %s", "Exception while serving %s"); the logger calls of its server, channel and task modules are errors, warnings and a few INFO lines (the listen address, the connection-limit recovery and, per request, a client disconnect), and the per-request lines name the path only; no access-log middleware was found in S-RT (search T08; N-1).
- Flask 3.1.3 logs an unhandled view exception as "Exception on <request.path> [<method>]" (8.2).
- INTERPRETATION: on the repository's serving path, with the probed library versions, no log line written by the receiver, waitress or Flask carries the query string. The URL itself existed in plain text on the network path (section 12), in Chartink's alert configuration (C-002 section 4.3) and in the request line; what the VMs' installed libraries and any network device logged is UNKNOWN (U-04).
Evidence: waitress/parser.py split_uri and waitress/channel.py lines 434 to 437 (waitress 3.0.2; hashes in the source inventory, section 8.5); flask/app.py line 876 (Flask 3.1.3).

### 11.3 Echoed in the body (FACT, RECORD-DERIVED, 05-Jul; C-002 section 4.2)
Chartink echoed the configured URL, token included, in the body's webhook_url field, and every stored payload to that day held it (95,431 of 95,431 rows). The code's own comment says the same ("Chartink echoes the configured URL (incl. ?token=<SECRET>) inside the body's `webhook_url` field"), and a unit-test helper builds such a body (FACT (TEST SOURCE)). Evidence: docs/SYSTEM_MAP.md line 1366 (cited from C-002); signals/webhook_receiver.py lines 681 to 687; tests/unit/test_webhook_receiver.py lines 1730 to 1742.

### 11.4 The stored copy: sanitised (S-1B.1, which its code comment dates 05-Jul) (FACT, SOURCE-VERIFIED)
- After authentication and after the body has been parsed, _process_request makes stored_payload = _sanitize_payload_for_storage(raw_body decoded as UTF-8 with replacement) and passes it to every symbol's INSERT into signals.webhook_payload. The parsed body used for processing is not changed.
- The sanitiser, applied in order to the stored string: (a) replaces the literal secret wherever it appears with <REDACTED>, when a secret is configured; (b) replaces token=<value> (the value runs to the next &, quote, apostrophe, white space or the end) with token=<REDACTED>; (c) replaces the whole value of a JSON "webhook_url" string with "<REDACTED>".
- Not sanitised: the warnings of 11.5 (they log raw_body itself). webhook_audit stores no body. The EOD route stores no payload (it hands each symbol, the scanner and the time to the capture worker).
- Four unit tests pin the behaviour with a placeholder token (FACT (TEST SOURCE)).
Evidence: signals/webhook_receiver.py lines 681 to 690; signals/webhook_receiver.py lines 857 to 886; signals/webhook_receiver.py lines 963 to 980; signals/webhook_receiver.py lines 790 to 796; tests/unit/test_webhook_receiver.py lines 1717 to 1838.
- The stored copy's next consumer: the dashboard's trade-detail reader selects webhook_payload with the signal, and the analytics service returns it as "raw_payload". INTERPRETATION / AUDIT LEAD (L-04): rows stored before the fix held the token (11.3) until the 06-Jul scrub of the 95,431 rows (FACT (RECORD-DERIVED), R-05). If the reader and the view existed in that period (only the pinned version was read), the trade-detail view could have shown a token-bearing payload for a trade of that period; whether they existed then, and whether anyone viewed one, is not established. Evidence: ops_dashboard/backend/readers/db_reader.py lines 2244 to 2247; ops_dashboard/backend/services/analytics_period.py line 636.

### 11.5 The raw-body warnings: not sanitised (L-06) (FACT, SOURCE-VERIFIED unless marked)
- Every 400 answer from _process_request (the intraday checks and the EOD route's own 400s) makes _handle_webhook log a WARNING with repr(raw_body[:1000]); on the intraday path a malformed-JSON body also logs its own WARNING with repr(raw_body[:500]), so it leaves two lines (the EOD route's malformed-JSON 400 logs no line of its own).
- Both come after authentication, so only a request that had passed it could produce them: on the pinned configuration, one with the right token or a valid signature, including the constant /health signature on an empty body (6.5), whose warnings hold an empty body.
- INTERPRETATION (the condition, as C-002 section 10.6 records it): such a line held the token only when the echoed webhook_url fell within the bytes logged (the first 1,000 or 500); not every 400 line held it.
- FACT (RECORD-DERIVED; C-002 section 14, R5 and R7): no 400 answer is recorded from 12-Jun to 23-Jul 15:29 or on any day from 24 to 31-Jul; FACT (RECORD-DERIVED, R-07): the then-current secret had 0 hits in the VM's logs on 16 and 17-Jul. The logs themselves were never opened (never-open).
Evidence: signals/webhook_receiver.py lines 518 to 523; signals/webhook_receiver.py lines 585 to 593; signals/webhook_receiver.py lines 757 to 760.

### 11.6 Other places in the repository that build or hold the token (FACT, SOURCE-VERIFIED)
The crash-test injectors and the load-test script build ?token= URLs from the environment or an argument (8.6); the boot self-check deliberately sends none (8.6). The records' own content: section 13.

### 11.7 Summary: where the token was and was not written, on the code path
| Place | The token there? | Condition and evidence |
|---|---|---|
| The request's query | Yes, by design (Chartink's method) | 11.1 |
| The query string in log lines (the receiver's, waitress's and Flask's) | No: the receiver reads no query string for a log line, and waitress and Flask name the path only, with the probed versions | 11.2; installed versions U-04 |
| The body's webhook_url, as Chartink sent it | Yes, as the 05-Jul record reports | 11.3 |
| signals.webhook_payload, after S-1B.1 | Redacted by the three steps of 11.4, in order | 11.4; stored rows before the fix: R-05, L-04 |
| webhook_audit | No: no body, query, header or credential | 7.4 |
| The 400 and malformed-JSON warnings | Only when webhook_url fell within the first 1,000 or 500 bytes | 11.5; none recorded in the windows of C-002's R5 and R7 (C-002 section 14) |
| The start-up check's log | No: names only | 4.4 |

## 12. Transport: HTTP, TLS, proxy, tunnel and firewall, by state
C-002 section 5 (frozen) is the full account; the points that bear on authentication:
- FACT (SOURCE-VERIFIED and CONFIG): waitress serves plain HTTP on the configured bind host and port 5000; the bind host is the all-interfaces address (confirmed value-blind). No server TLS setting was found in S-CODE (C-002 search S19; N-3); the hits of the wider search T18 that were inspected (main.py, the system configuration and the configuration loader) are outbound URLs (an alert API, two HTTPS adapter mounts and a public data-file URL) and one URL-scheme validator, none a server TLS setting. Evidence: main.py lines 3979 to 4000; config/system_config.yaml lines 327 to 335.
- INTERPRETATION: over plain HTTP the query token, the body's echoed webhook_url and any X-Webhook-Signature header crossed the network unencrypted; an HMAC would have protected integrity and the secret itself, not the request's confidentiality. FACT (RECORD-DERIVED): on 02-Jul the token travelled "over plaintext HTTP"; on 06-Jul TLS was "deferred" (C-002 section 5.3). Whether TLS was ever added after that: U-12.
- Reverse proxy and tunnel: none configured in the repository; the nginx and ngrok mentions are comments and runbook steps (C-002 K-5 and N3; N-4 here); a 03-Jul record found no nginx on the VM (C-002 section 5.4). The records disagree on what fronted the receiver (C-05).
- Firewall: the installer inserts an iptables rule accepting tcp/5000 with no source restriction (FACT (SOURCE-VERIFIED), re-read in the research; C-002 section 5.2); a single-address allowlist applied by Rama on 05-Jul (FACT (RECORD-DERIVED)); its later state NOT ESTABLISHED (C-002 section 5.2, U-4; C-11 here). Evidence: deploy/install_vm_services.sh lines 33 to 47.
- HISTORICAL (Rama, Q7, 22-Oct): "Don't know / never checked" for any TLS, proxy, tunnel or network restriction beyond C-002's record.

## 13. Secret rotation and exposure history (FACT, RECORD-DERIVED; each for its own date and scope)
The records were read by a separate records agent through the stage-3 mask and checked by Claude value-blind (source inventory, sections 4 and 7). Each row: date, what the record says, evidence. The records conflict on several points; they are listed side by side, not reconciled (section 20). Nothing here was re-measured: the VMs and their databases are gone. The legacy token stays treated as EXPOSED in every case (DR-055; VP-50).

### 13.1 Where the secret was and how it changed
| ID | Date | What the record says | Evidence |
|---|---|---|---|
| R-01 | 02-Jul | The security audit's C-1: live credentials, WEBHOOK_SECRET included, committed in a tracked example file and in history (A-010 section 11 records the commits). C-1's inventory then lists the webhook secret "rotated + synced to VM+PC" | docs/audit/system_security_audit_02jul2026.md lines 15 to 21; docs/audit/system_security_audit_02jul2026.md lines 80 to 82; docs/audit/c1_credential_exposure_inventory_02jul2026.md line 83 |
| R-02 | 03-Jul | The system map and paths record: the secret rotated on 03-Jul, VM-only, the old token having been "exposed in chat" twice; old rejected and new accepted "verified value-blind" (the method is not recorded); one shared token for the 15 scanners; it loads at the Monday 08:15 start, and Rama is to update the token in all 15 Chartink URLs. The 05-Jul audit lists the VM's .env as modified on 03-Jul at 23:03 and says "secret rotated 03-Jul"; the same audit elsewhere says all six exposed secrets were "rotated dead 02-Jul" | docs/SYSTEM_MAP.md line 452; PATHS.md line 238; docs/audit/audit_05jul2026.md line 170; docs/audit/audit_05jul2026.md line 187; docs/audit/audit_05jul2026.md line 199 |
| R-03 | 05-Jul (Sunday) | S-1A: GATE B, a single-address allowlist for the webhook port, applied by Rama; GATE C staged: WEBHOOK_SECRET rotated (old and new identified by hashes only), the new secret to load at the next natural start, "Chartink URL updated by Rama"; its C.4 sweep found a single environment source and no drop-in shadow. Whether GATE C is the 03-Jul rotation or a further one is not stated (U-03) | docs/SYSTEM_MAP.md line 1365; PATHS.md lines 262 to 263 |
| R-04 | 06-Jul (Monday) | GATE D PASS: that day's rows were 86 answered 200 and 309 answered 403, zero 401, all from Chartink's address; the record calls this a "passive proof" that Chartink moved to the new token, records that the active test of the old token was not run in market hours, and sets the rule "only 401 = webhook auth/token failure"; TLS deferred | docs/SYSTEM_MAP.md line 1364 |
| R-05 | 06-Jul (about 18:11) | S-1B.2: the 95,431 stored rows scrubbed (token count 0), 21 secret-bearing database backups and a further artifact holding 85,765 token rows shredded; the entry says "Both historical secrets already DEAD (401 at GATE D)", which R-04 does not show (C-02). 07-Jul: the rollback anchor shredded, "no secret-bearing artifacts remain" | docs/SYSTEM_MAP.md line 1362; docs/SYSTEM_MAP.md line 1351 |
| R-06 | 12-Jul and 15-Jul | PB-01's alert: authentication by the query token, "one shared secret (VM .env only)"; the 15-Jul specification gives it "the same shared token" as the other scanners (C-002 section 4.2) | PATHS.md line 185; docs/audit/followup_investigation_15jul2026.md line 181 |
| R-07 | 16 and 17-Jul | AB-910 section 1.10: "WEBHOOK_SECRET: 0 hits in logs"; on 17-Jul every value of the VM's .env was counted, value-blind, against all 132 VM log files, with 0 hits for WEBHOOK_SECRET. INTERPRETATION: the values counted were those in the VM's .env at the time, by the record's description, so the retired values were not among them | docs/audit/audit_b_phase9_ops_phase10_security_16jul2026.md lines 202 to 208; docs/audit/batch2_done_17jul2026.md lines 79 to 85 |
| R-08 | 25-Jul | The bot-token rotation changed two keys; WEBHOOK_SECRET "present and unchanged on the VM", checked structurally | docs/audit/f2_auth_separation_25jul2026.md lines 11 to 13 |
| R-09 | 28-Jul, 01-Aug and 04-Aug | 28-Jul: the register says the test environment holds 20 live production keys, WEBHOOK_SECRET among them. 01-Aug: WEBHOOK_SECRET on both the PC's and the VM's .env; a PC compromise yields it "but cannot place an order"; a weekday re-check of the bind and require_hmac is owed. 04-Aug: 15 entries of the assistant's permission list post to the live webhook and one sed -i edit comments out WEBHOOK_SECRET ("disabling an auth control"), left open; the PC holds 20 keys and the VM 24. INTERPRETATION (from 4.4 and 4.6): on the code path, commenting out WEBHOOK_SECRET would have stopped the next start with "missing_secrets" (return 3), not opened the receiver (C-09) | docs/MASTER_PENDING_28-Jul-2026.txt lines 285 to 286; docs/audit/integrity_audit_2026.md lines 4286 to 4295; docs/audit/integrity_audit_2026.md line 4354; docs/MASTER_PENDING_01-Aug-2026.md lines 992 to 1009; docs/audit/ledger11_secrets_surface_measurement_04aug2026.md lines 46 to 50 |

HISTORICAL (Rama, Q5 and Q9, 22-Oct): "Don't know / never checked" when the secret was rotated, and "Don't know / never checked" whether the old token was used after rotation or confirmed invalid.

### 13.2 The authentication changes, by their records
| ID | Date | What the record says | Evidence |
|---|---|---|---|
| R-10 | 25-Apr | G.1 (require_hmac disables the token fallback on /webhook): dated by the source comment and, after the event, by the 25-Jul C6 records; no record of that date was found among the records read | signals/webhook_receiver.py line 196; docs/audit/receiver_auth_c6_scoping_2026-07-25.md line 119; docs/audit/c6_auth_collapse_25jul2026.md line 21 |
| R-11 | 01-Jun to 02-Jul | 01-Jun: require_hmac still false, acceptable because Chartink cannot sign, the surface limited by "UFW + ngrok" (C-05). 15-Jun: a flow review describes the intake as HMAC over the raw body and rates it CORRECT (C-06). 02-Jul C-2: all-interfaces bind, require_hmac false, the URL token over plaintext HTTP as the only live control, and in paper mode the secret not required. 02-Jul evening: C-2's Phase 2 deployed (the secret required in both modes, the per-address limit); Phase 3 (loopback bind, allowlist, a TLS proxy or HMAC) left to Rama | docs/architectural_audit_2026-06-01.md lines 483 to 487; docs/audit/system_flow_review_15jun2026.md lines 12 to 25; docs/audit/system_security_audit_02jul2026.md lines 19 to 21; docs/audit/system_security_audit_02jul2026.md lines 85 to 89; docs/audit/audit_remediation_status_02jul2026.md lines 17 to 67 |
| R-12 | 03-Jul | The source-address investigation: no nginx, no reverse proxy in front of the receiver; 58,507 requests from 12-Jun with codes 403, 200 and 503 only, the 403s "not an auth or IP problem"; 99.99% from one address, pool variation not excluded; and, in its own words, no verdict between an IP allowlist and a reverse proxy with HMAC, the decision deferred. C-001 line 263 left "the investigation's verdict" to C-003: the record gives none; the next recorded step is R-03's allowlist | docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 5 to 6; docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 13 to 39; docs/audit/c2_chartink_ip_investigation_03jul2026.md line 51 |
| R-13 | 16-Jul | AB-910 section 1.7: the receiver's /health had no authentication and no limiter and returned the halt and queue state; section 1.8: require_hmac false accepted pending C-2 Phase 3, the shared query token described as exposed to intermediaries, shoulder-surfable and replayable | docs/audit/audit_b_phase9_ops_phase10_security_16jul2026.md lines 166 to 169; docs/audit/audit_b_phase9_ops_phase10_security_16jul2026.md lines 182 to 190 |
| R-14 | 17-Jul | About 02:00, S4 shipped (the receiver's /health behind the secret, the limiter before authentication). At the 08:15 start the self-check called /health with no credential, got 401, treated it as unreachable and the service stopped at 08:16:09 with exit 0; 0 trades. The fix counted 401 as reachable; sending the token in the check was rejected because the checker logs URLs. The fixture-blindness record lists among the tested branches a bad HMAC with a valid token refused 401 ("no fall-through"); /health did not then honour require_hmac (a read-only probe), and the flip is recorded as an open action for Rama; /health's signature is a constant. Evening: P1 deployed (the require_hmac refusal on /health), "inert" while the flag is false, loading at the 20-Jul start | docs/audit/sweep_done_17jul2026.md line 54; docs/audit/sweep_done_17jul2026.md line 121; docs/audit/s4_boot_outage_17jul2026.md lines 19 to 24; docs/audit/s4_boot_outage_17jul2026.md line 84; docs/audit/s4_boot_outage_17jul2026.md lines 115 to 127; docs/audit/fixture_blindness_investigation_17jul2026.md line 72; docs/audit/fixture_blindness_investigation_17jul2026.md lines 139 to 174; docs/audit/p1_health_require_hmac_done_17jul2026.md lines 48 to 149 |
| R-15 | 20-Jul | Correction: "require_hmac is NOT safe to flip": Chartink cannot sign, so a flip makes every POST 401; it stays false unless a different signer or a signing proxy exists. The pinned system map and paths record still carry the earlier wording (C-04) | docs/audit/pre_receive_hook_investigation_20jul2026.md lines 178 to 212; docs/SYSTEM_MAP.md line 191; PATHS.md line 113 |
| R-16 | 23-Jul and 31-Jul | 23-Jul: zero 401 rows in 104,911 POSTs (89,794 to 16-Jul and 15,117 from 17-Jul to 23-Jul 15:29), which the record reads as no unauthenticated POST reaching the receiver. 31-Jul: zero 401 answers all-time; the audit table records "every outcome incl. 401", "a change since 5-Jul" (U-08); whether anything fronts the receiver is still UNCERTAIN | docs/audit/refused_posts_403_investigation_23jul2026.md lines 115 to 121; docs/audit/integrity_audit_2026.md line 43; docs/audit/integrity_audit_2026.md line 380; docs/audit/integrity_audit_2026.md line 392 |
| R-17 | 25-Jul | C6: one call site; the uniform /health 401 deliberate, /webhook keeping four messages; a brute force against /health leaves no audit row; the change loads at the next Monday start | docs/audit/receiver_auth_c6_scoping_2026-07-25.md lines 115 to 121; docs/audit/c6_auth_collapse_25jul2026.md line 42; docs/audit/c6_auth_collapse_25jul2026.md lines 104 to 106 |
| R-18 | 28-Jul | require_hmac "KEEP FALSE", verified that night (what was verified, code or VM, is not stated in the passage tested) | docs/MASTER_PENDING_28-Jul-2026.txt line 376 |

### 13.3 What the records do not establish
- How many rotations there were, and on which dates: rotations are recorded on 02-Jul (R-01) and on 03-Jul (R-02), and a staged rotation at GATE C on 05-Jul (R-03); the records conflict on where and when (C-01), and how many rotations there were is not established (U-03).
- Whether a retired token was ever refused: the 06-Jul check was passive (zero 401 that day) and the active test of the old token was not run (R-04); a passive zero-401 count does not prove that an old token was rejected, since it shows only that no audited request was refused for authentication that day, not that the old token was presented and refused. S-1B.2's "401 at GATE D" is not supported by GATE D's own entry (R-05; C-02).
- INTERPRETATION / AUDIT LEAD (L-07), the installed require_hmac: no record read quotes the VM's installed file. But Chartink sent the token in the query (C-002 section 4.2), no record or source shows a Chartink signature, the 20-Jul record says a flip would make every POST fail with 401 (R-15), and the recorded 401 count is zero on every recorded day to 31-Jul (R-04, R-16). On the code path that is consistent only with require_hmac resolving False at the running receiver on those days (or with a signature nobody recorded). It rests on the audit covering 401s on those days (U-08), and it is an inference, not a fact: the installed value stays UNKNOWN (U-01).
- Who else held the token or a token-bearing URL, and until when: U-06.

## 14. Paper and live; declared and installed
- FACT (SOURCE-VERIFIED): the authentication code has no mode branch (4.7), and the presence check has no mode parameter (4.4). The code comments date "required in BOTH paper and live" to C-2, 02-Jul.
- HISTORICAL (Rama, A-010, corrected 09-Oct; VP-56): the testing VM used different alert and broker API credentials "to avoid collision". Whether it used the same authentication mode as production: Rama, Q3: "Don't know / never checked". The testing snapshot was not used (Q12 (c)), so its configuration is not established here (U-05). No parity between the two VMs is inferred.
- Declared, not installed: everything in this manual is what the baseline declares and the dated records report. The installed require_hmac and secret on either VM, their library versions and their configuration are UNKNOWN (U-01; C-002 U-6).
- V2 is live-only (DR-001); this section records legacy fact only.

## 15. Tests inspected, not executed (FACT, TEST SOURCE)
No test was run. The receiver tests below use Flask's test client (no socket, no network; C-002 searches S20 and S21 for the webhook test files); tests/unit/test_c2_webhook_lockdown.py and tests/unit/test_s4_webhook_degrade.py do not use it and send no request (filter greps; importing main, as tests/unit/test_c2_webhook_lockdown.py does, also imports requests through scripts.zerodha_login). None exercises waitress, the network, Chartink's real requests or the installed configuration. A test shows what its code asserts, not what production did.

| Test file and lines | Asserts (by its code) | Does not prove |
|---|---|---|
| tests/unit/test_c6_auth_collapse.py lines 1 to 35; tests/unit/test_c6_auth_collapse.py lines 166 to 252; tests/unit/test_c6_auth_collapse.py lines 262 to 394; tests/unit/test_c6_auth_collapse.py lines 403 to 438 | Every branch on both routes; the four divergences (the signed bytes per route, the receiver's /health's uniform 401, the EOD early return after authentication, /webhook audits a 401 and /health nothing); compare_digest only inside _authenticate; both routes call it; no mode words | The installed require_hmac; waitress's request parsing; any request Chartink sent |
| tests/unit/test_webhook_receiver.py lines 79 to 107; tests/unit/test_webhook_receiver.py lines 134 to 145; tests/unit/test_webhook_receiver.py lines 288 to 349; tests/unit/test_webhook_receiver.py lines 1106 to 1287; tests/unit/test_webhook_receiver.py lines 1717 to 1838 | HMAC absent, mismatch, valid and no-secret cases; BL-18 (constructor only, by its own comment); G.1 (require_hmac True refuses token-only, accepts a valid signature, keeps the token when False, no fall-through); S-1B.1 (four tests) | Authenticated behaviour in most of this file: its default factory builds the receiver with no secret, so most of its tests run with authentication open (K-05) |
| tests/unit/test_fix134_backpressure.py lines 144 to 175; tests/unit/test_fix134_backpressure.py lines 225 to 241; tests/unit/test_fix134_backpressure.py lines 277 to 402 | TestHealthAuth (AB-910; its own comment says all its tests run with require_hmac False); TestHealthRequireHmac (P1, 17-Jul: the receiver's /health honours require_hmac); the S4 guard (the no-credential /health stays 401 under both settings and the boot check maps it to reachable) | That require_hmac was ever true on a VM; the self-check's result on any day |
| tests/unit/test_c2_webhook_lockdown.py lines 1 to 13; tests/unit/test_c2_webhook_lockdown.py lines 84 to 91 | The per-address limiter; WEBHOOK_SECRET in the required list | The installed environment |
| tests/integration/test_hardening_scenarios.py lines 21 to 27; tests/integration/test_hardening_scenarios.py lines 285 to 328; tests/integration/conftest.py lines 154 to 162; tests/integration/conftest.py lines 380 to 401 | The wired fixture builds the receiver with secret_token=None; a second fixture, added 17-Jul, builds it with what its comment calls "the prod-like secret" (its value masked) for the S4 boot-seam test. The file's docstring: "Every other wired case here passes secret_token=None, which is precisely why the 401 that took prod down cannot occur anywhere else in this suite." | Authenticated behaviour in the other wired cases (K-05) |
| tests/unit/test_s4_webhook_degrade.py | The degrade decision of 27-Jul (not authentication itself) | Reachability from outside the VM |

KNOWN DEFECT / AMBIGUITY (K-05, test design): authentication was open in most receiver fixtures, so a green run said little about authenticated behaviour outside the dedicated tests; the 17-Jul records found this ("fixture blindness"; R-14).

## 16. Source-to-consumer trace (source wiring at the baseline, not host execution)

### 16.1 Architecture
```
 the trading process (main.py); what each VM actually ran is not established (U-01, C-002 U-6)
 +---------------------------------------------------------------------------------------------------+
 | environment: the unit's EnvironmentFile (.env) and load_dotenv() at import of zerodha_login (4.3) |
 |      |                                                                                            |
 |      v                                                                                            |
 | start-up presence check (4.4): WEBHOOK_SECRET unset or empty --> CRITICAL, return 3, no receiver  |
 |      | set and non-empty                                                                          |
 |      v                                                                                            |
 | WebhookReceiver(secret_token = os.environ.get("WEBHOOK_SECRET"), config)        (4.2, 5.3, 5.4)   |
 |      | self._secret; self._require_hmac (False on the pinned configuration)                       |
 |      v                                                                                            |
 | waitress, port 5000, plain HTTP, 8 threads --> the receiver's Flask app                           |
 |      POST /webhook/<key> : _handle_webhook --> _authenticate(raw body) --> gates --> rows, queue  |
 |      GET  /health        : limiter --> _authenticate(b"") --> 200 state | 401 | 429 | 500         |
 |      ^                                                                                            |
 |      | GET /health on the loopback address, no credential, 2 s after waitress starts (8.6)        |
 | main: boot self-check (2xx or 401 = reachable)                                                    |
 |                                                                                                   |
 | health server, port 8080: its own GET /health, no webhook authentication (B-007; not C-003)       |
 +---------------------------------------------------------------------------------------------------+
        ^
        | POST /webhook/<key>?token=<secret>, the body echoing that URL in webhook_url (11.3)
   a Chartink alert (C-002 sections 4 and 5)
```

### 16.2 Trace table
| Step | Source | Input | Logic | Validation or guard | Output | Storage or hand-off | Next consumer |
|---|---|---|---|---|---|---|---|
| 1. The environment | deploy/systemd/trading-system.service line 13; main.py line 106; scripts/zerodha_login.py lines 32 to 36 | The unit's EnvironmentFile and .env (never opened) | Variables loaded into the process environment | None | os.environ | Process memory | The start-up check |
| 2. The presence check | main.py lines 219 to 232; main.py lines 2425 to 2430; utils/startup_checks.py lines 1071 to 1086 | The list of required names | os.environ.get(name) falsy counts as missing | "missing_secrets" blocks: CRITICAL and return 3 (main.py lines 2476 to 2502) | Pass, or the start ends | A log line with names only | The construction |
| 3. The construction | main.py lines 3406 to 3415; signals/webhook_receiver.py lines 152 to 162; signals/webhook_receiver.py lines 163 to 176; signals/webhook_receiver.py lines 177 to 182 | os.environ.get("WEBHOOK_SECRET"); AppConfig | self._secret; require_hmac resolved to self._require_hmac | BL-18: ValueError if R True and no secret | The receiver | In memory | _authenticate on every request; the sanitiser's step (a) |
| 4. Serving | main.py lines 3979 to 4000; signals/webhook_receiver.py lines 360 to 413 | A TCP connection on port 5000 | HTTP parsing; the rule match | The framework's own answers (C-002 section 7.3) | The view | None | _handle_webhook or the /health view |
| 5. _handle_webhook | signals/webhook_receiver.py lines 477 to 534 | remote_addr; the raw body | The body read; the shutdown flag; the per-address bucket | 413 at the read (no row); 503; 429 | An answer, or the call below | One webhook_audit row per answer once the body has been read, unless the insert fails | _process_request |
| 6. Authentication on /webhook | signals/webhook_receiver.py lines 326 to 354; signals/webhook_receiver.py lines 547 to 549 | The raw body, the header, the token | B0 to B4 (6.2) | compare_digest; a non-ASCII value raises TypeError | A pass, a 401 with the branch's message, or a 500 | The audit row (401, 0, 0) or (500, 0, 0); no log line for a 401 | The key check |
| 7. The gates after authentication | signals/webhook_receiver.py lines 539 to 583; signals/webhook_receiver.py lines 585 to 674 | The key, the body | Key, EOD branch, kill, backpressure, window, parse and fields | 404, 403, 503, 400 | The per-symbol loop, or the EOD route | The audit row; on a 400 the raw-body WARNING (signals/webhook_receiver.py lines 518 to 523) | _process_signal or the EOD route |
| 8. The stored copy | signals/webhook_receiver.py lines 681 to 690; signals/webhook_receiver.py lines 857 to 886; signals/webhook_receiver.py lines 963 to 980 | The raw body | Three redactions, in order (11.4) | - | stored_payload | signals.webhook_payload, in each symbol's INSERT (the queue tuple does not carry it) | The dashboard's trade detail (ops_dashboard/backend/readers/db_reader.py lines 2244 to 2247; ops_dashboard/backend/services/analytics_period.py line 636) |
| 9. The receiver's /health | signals/webhook_receiver.py lines 364 to 404 | remote_addr, the header, the token | The bucket; _authenticate(b"") | 429; 401; a 500 through the handler | 200 with the state of 8.5 | None: no audit row, and no log line on 429, 401 or 200 | The caller |
| 10. The boot self-check | main.py lines 4002 to 4054; utils/startup_checks.py lines 783 to 826 | GET /health with no credential | 2xx or 401 counts as reachable | - | Reachable, or not | CRITICAL and a sentinel when not | The start continues, degraded if not reachable |

### 16.3 End-to-end walkthrough: one Chartink POST on the pinned configuration
What any one request on any one day did is not observed; this is the code path for a request carrying the token.
1. The alert's saved URL has the form of C-002 section 4.2: plain HTTP to the VM's address, port 5000, /webhook/<key>, ?token=<secret>; Chartink's body repeated that URL in webhook_url (11.3).
2. waitress parses the request and Flask matches the rule; a wrong path, method or slash form, or an OPTIONS request, is answered by the framework (404, 405 or 308; 200 for OPTIONS) with no view and no audit row (C-002 section 7.3).
3. _handle_webhook takes remote_addr and reads the body (413 over 1 MiB, no row), then checks the shutdown flag (503) and the per-address bucket (429), both audited (7.2).
4. _authenticate(raw body): no "sha256=" header, R False, a non-empty token, so B3 compares it with the secret: equal is a pass (6.4 row 6); unequal answers 401 "Invalid token", audited, with no log line (row 7); a non-ASCII value raises TypeError and is answered 500 (row 8; K-03; the path through waitress was not probed).
5. After the pass: the unknown-scanner 404, the EOD branch, the kill-switch 403, the backpressure 503, the window 403, then parsing and field checks (400, with the raw-body WARNING) (7.3).
6. For each symbol that reaches the INSERT, the sanitised copy is stored in its signals row and a tuple goes on the queue for SignalProcessor (7.6, 11.4).
7. The finally clause attempts the webhook_audit row: no credential, query, header, reason or body (7.4).
8. The answer goes back through waitress; whether the sender received it is outside the system (C-002 section 9).

### 16.4 The three chains in one line each
- The secret: process environment (the unit's EnvironmentFile and the import-time load_dotenv() of 4.3; A-010) -> os.environ.get(name) for each name of the required list in the start-up check (presence, non-empty) -> blocking "missing_secrets", return 3 -> os.environ.get("WEBHOOK_SECRET") at construction -> self._secret -> _authenticate on every request of both routes; the S-1B.1 sanitiser (a). The code never logs self._secret, the start-up check logs names only, the stored copy is sanitised and the audit row holds no body; but the two raw-body WARNINGs log unsanitised body bytes, which held the token whenever the echoed webhook_url fell inside the logged prefix (11.3, 11.5).
- POST /webhook/<scanner_name>: Chartink (or any caller) -> waitress -> Flask rule (404, 405 or 308, and 200 for OPTIONS, before the view) -> remote_addr, get_data() (413 for a body over the limit, before authentication and any audit write) -> shutdown 503 -> per-address 429 (audited, address logged) -> _authenticate(raw body): header, require_hmac, token -> 401 with the branch's message, audited (401, 0, 0), no log; or 500 on a non-ASCII credential (K-03) -> unknown scanner 404 -> EOD route or kill / backpressure / window / parse gates -> sanitised copy -> signals rows and the queue -> SignalProcessor; webhook_audit -> census and dashboard; signals.webhook_payload -> dashboard trade detail ("raw_payload"); 400 warnings -> the system and debug logs and the journal.
- GET /health (the receiver's): the boot self-check (no credential) or any caller -> waitress -> Flask -> remote_addr -> per-address 429 -> _authenticate(b"") -> uniform 401, or 200 with kill state and queue depth, or 500 on a non-ASCII credential (K-03; CRITICAL from the 500 handler) -> no audit, and no log line on 429, 401 or 200 -> the boot self-check's reachable decision (2xx or 401 reachable; otherwise CRITICAL, sentinel, degrade).
What remains unknown along each chain: section 19.

## 17. Known defects, ambiguities and documentation drift
| ID | Finding | Label | Section |
|---|---|---|---|
| K-01 | WR8 names only the header, not the query token, the method the configuration names (signals/webhook_receiver.py line 19) | KNOWN DOC DRIFT (minor) | 5.5 |
| K-02 | The nginx comments and the bind comment's reliance on require_hmac=true (recorded in C-002 as K-5 and K-6) | KNOWN DOC DRIFT | 5.5, 12 |
| K-03 | A non-ASCII credential raises TypeError inside _authenticate, answered 500 on both routes instead of 401, and audited and logged as a 500 on /webhook. INTERPRETATION from the source and a library probe, not an observed incident; the serving path through waitress was not probed; whether one ever arrived: U-09 | KNOWN DEFECT / AMBIGUITY | 6.3, 7.3, 8.2, 9 |
| K-04 | /webhook's granular 401 messages disclose the posture that the receiver's /health hides | KNOWN DEFECT / AMBIGUITY | 10.3 (L-02) |
| K-05 | Authentication open in most receiver test fixtures | KNOWN DEFECT / AMBIGUITY (test design) | 15 |
| K-06 | Comments cite old line numbers: "main.py:226" (the list item is at line 231); "main.py:3238", "main.py:3240-3242", "main.py:3241" and "main.py:3242" (the self-check and its log are at lines 4002 to 4036); and ":410-432" (C-002 K-8). Two test comments from before C6 also describe branches that no longer exist: a request-time "elif self._require_hmac" (now an if inside _authenticate, signals/webhook_receiver.py line 343) and S4's "if receiver._secret:" (now if not self._secret, signals/webhook_receiver.py line 330). Evidence: signals/webhook_receiver.py line 328; utils/startup_checks.py lines 799 to 802; tests/integration/test_hardening_scenarios.py lines 293 to 325; tests/unit/test_fix134_backpressure.py line 358; tests/unit/test_fix134_backpressure.py line 373; signals/webhook_receiver.py line 687; signals/webhook_receiver.py line 865; tests/unit/test_webhook_receiver.py line 1111; tests/unit/test_webhook_receiver.py line 1721 | KNOWN DOC DRIFT (minor) | 4.6, 8.6 |

Already recorded and not repeated as new: C-002 K-5 (the nginx comments), K-6 (the bind comment), K-8 (the sanitiser's old line numbers) and K-9 (the runbook's /health check).

## 18. Audit leads (INTERPRETATION / AUDIT LEAD; not observed)
- L-01. Replay: the receiver's /health signature is a constant, and it also authenticates a POST to /webhook whose body is empty (which then reaches every answer after authentication but cannot get a signal accepted); a /webhook signature binds the body only, not the path, method, time or a nonce, and repeats are limited after authentication on the intraday path only (the EOD route has no expiry, dedup or fingerprint check); the query token is a static bearer value (6.4, 6.5).
- L-02. Posture disclosure: /webhook's 401 messages tell an anonymous caller whether the token fallback is open (10.3).
- L-03. A require_hmac flip would have refused every Chartink POST with 401 (5.5; R-15).
- L-04. Token-bearing stored payloads stayed in the database until the 06-Jul scrub (R-05); at the baseline, the dashboard's trade detail returns the stored payload as "raw_payload" (11.4). Whether that reader and view existed before the scrub (only the pinned version was read), and whether anyone viewed such a payload: not established. Tracking: VP-55.
- L-05. Little trace of failed authentication: no log line for a 401 on either route; the receiver's /health writes no audit row, so a guessing attack on it leaves none (R-17); one per-address limiter for both routes (7.2, 7.5, 8.1, 8.3).
- L-06. The warnings' raw bodies after authentication (the right token or a valid signature): unsanitised; no 400 recorded in the windows of C-002's R5 and R7 (12-Jun to 23-Jul 15:29; 24 to 31-Jul); the then-current secret had 0 hits in the VM logs on 16 and 17-Jul (11.5; R-07). C-002 section 10.6's lead is bounded by these records for their dates; the retired values were not counted.
- L-07. The installed require_hmac, inferred from zero 401s (13.3), with U-01 and U-08.

## 19. Unknown / not established
None of these can be measured now: the VMs, their databases and their logs are gone (Rama, 22-Oct). The accepted errata of section 24 settle none of them.
- U-01. The installed require_hmac and secret on either VM on any date. Evidence: the pinned declaration only (config/system_config.yaml line 336); records that state the value without quoting the installed file (R-11, R-13, R-18); an inference from the zero-401 records (13.3, L-07). Rama, Q2: "Don't know / never checked".
- U-02. Whether Chartink ever sent an X-Webhook-Signature header. Evidence: no code outside tests computes one (N-7); the configuration comment and the 20-Jul record say Chartink cannot sign (config/system_config.yaml line 336; docs/audit/pre_receive_hook_investigation_20jul2026.md line 181); C-002 section 4.4 (Chartink's public article states no authentication). Not assumed either way.
- U-03. How many rotations there were, on which dates, and whether a retired token was ever refused after its rotation. Evidence: rotations recorded on 02-Jul and on 03-Jul, and a staged rotation at GATE C on 05-Jul (R-01 to R-03); the 06-Jul check was passive and the active test of the old token was not run (R-04); the 03-Jul "verified value-blind" has no recorded method (R-02); S-1B.2's "401 at GATE D" is not supported by GATE D's own entry (R-05). Rama, Q5 and Q9: "Don't know / never checked". The rotation history in A-010 and C-002 gains the missing records through errata A-010 E-5 and C-002 E-1, which keep this question open (section 24). OPEN QUESTION, tracking only: VP-50 (the legacy token stays treated as exposed whatever the history).
- U-04. What the installed libraries, the cloud network or any device on the path logged (query strings, headers). Evidence: the probed library versions log the path only (11.2); the installed versions are unknown (C-002 U-6); no proxy was found (12).
- U-05. The testing VM's authentication mode and secret. Evidence: the testing snapshot was not used (Q12 (c)); Rama (09-Oct, VP-56): different alert and broker API credentials on the testing VM; the 28-Jul record says the test environment holds 20 live production keys, WEBHOOK_SECRET among them, and the 01-Aug record reads the "live keys" count as true of the VM, not the PC (R-09; docs/audit/integrity_audit_2026.md line 4295), so which machine that test environment is, is not established. Rama, Q3: "Don't know / never checked". OPEN QUESTION, tracking only: VP-56.
- U-06. Who else held the token or a token-bearing URL, and until when. Evidence: Chartink's alerts (C-002 section 4.3); the old token "exposed in chat" twice (R-02); the PC's .env (R-09); 15 permission-list entries posting to the live webhook, their URLs not described (R-09); a "Chartink-side URL Rama pasted" (docs/audit/integrity_audit_2026.md line 394; its location not stated). No clean-up is proposed (Rama's decision of 22-Oct, recorded in C-002 section 4.3 and DR-055). OPEN QUESTION, tracking only: VP-50.
- U-07. Why /webhook kept its four granular 401 messages while the receiver's /health was made uniform "on purpose". Evidence: 10.2 and 10.3; the 25-Jul records give the reason for /health only.
- U-08. When webhook_audit began recording 401 outcomes. Evidence: the pinned code audits a 401 in its finally clause (7.4); a 04-Jul record already describes a database insert for every unauthenticated request; the 31-Jul audit calls "every outcome incl. 401" "a change since 5-Jul"; the code before the baseline is not in the snapshot (no history command was run on the clone). Bearing: a zero-401 count before that change proves less than one after it (C-002 section 14 R5 counts from 12-Jun; R-16). Evidence: docs/audit/full_system_audit_04july2026.md line 141; docs/audit/integrity_audit_2026.md line 43. No erratum is proposed for C-002 section 14, which reports the recorded counts as recorded.
- U-09. Whether a non-ASCII credential ever reached the receiver (K-03). Evidence: no 500 is recorded from 12-Jun to 23-Jul 15:29 or on any day from 24 to 31-Jul (C-002 section 14, R5 and R7); outside those windows, not established.
- U-10. The 04-Aug permission items (15 webhook POST entries; a sed -i edit that comments out WEBHOOK_SECRET): resolved? Evidence: R-09; the design log's Q-36 (j) asks the same. OPEN QUESTION, tracking only: Q-36 (j).
- U-11. The deploy time of the 17-Jul fix that made the self-check accept 401. Evidence: R-14; B-007 section 4 records its first production run on 20-Jul. Listed for completeness; B-007 already records it.
- U-12. Whether TLS was ever added for the webhook after it was deferred on 06-Jul. Evidence: R-04; no later record found among the records read; no TLS in the pinned source (12; N-3).

## 20. Conflicts between sources (stated side by side; not reconciled)
- C-01. Rotation dates. 02-Jul "rotated + synced to VM+PC" (the C-1 inventory) and "rotated dead 02-Jul" (the 05-Jul audit, for the six exposed secrets) against 03-Jul "VM-only" (the system map, the paths record, the 05-Jul audit's "secret rotated 03-Jul", the .env modified 03-Jul at 23:03) and the 05-Jul GATE C entry ("rotated", staged) (R-01 to R-03). Evidence: docs/audit/c1_credential_exposure_inventory_02jul2026.md line 83; docs/audit/audit_05jul2026.md line 199; docs/audit/audit_05jul2026.md line 187; docs/audit/audit_05jul2026.md line 170; docs/SYSTEM_MAP.md line 452; PATHS.md line 238; docs/SYSTEM_MAP.md line 1365. Carried into errata A-010 E-5 and C-002 E-1, which keep it open (section 24).
- C-02. Whether a retired secret was shown dead. "Both historical secrets already DEAD (401 at GATE D)" (S-1B.2, 06-Jul evening) against GATE D's own entry: zero 401 observed, the active old-token test not run (06-Jul) (R-04, R-05; U-03).
- C-03. Where the secret was kept. VM and PC (02-Jul); VM-only (03-Jul; 12-Jul); a test environment with 20 live keys (28-Jul); PC and VM (01-Aug, the PC with 5 keys: docs/audit/integrity_audit_2026.md line 4289); PC 20 keys and VM 24 (04-Aug) (R-01, R-02, R-06, R-09). Copies and the testing machine are A-010 sections 17 and 19's subject; no erratum is proposed.
- C-04. Flipping require_hmac. Pending and "safe" after P1 (the 17-Jul records, and a test comment: FACT (TEST SOURCE)) against "NOT safe to flip" (20-Jul: Chartink cannot sign; every POST would be 401); the pinned system map and paths record keep the earlier wording (5.5, R-14, R-15). Evidence: docs/SYSTEM_MAP.md line 191; PATHS.md line 113; docs/audit/pre_receive_hook_investigation_20jul2026.md lines 178 to 212.
- C-05. What fronted the receiver. The G.1 comments and two records assume nginx; a 01-Jun audit names "UFW + ngrok"; the 03-Jul investigation found no nginx and no reverse proxy; the 05-Jul paths record says ufw was inactive and the host firewall iptables-nft; the 31-Jul audit keeps the question "UNCERTAIN" (5.5, 12, R-11, R-12, R-16; C-002 K-5 and section 5). Evidence: docs/audit/fixture_blindness_investigation_17jul2026.md line 133 and docs/audit/f2_injection_scoping_25jul2026.md line 104 (the two records that assume nginx); PATHS.md line 263 (ufw inactive; iptables-nft). C-002 holds the account; no new erratum.
- C-06. What the intake authentication was in June. A 15-Jun flow review describes HMAC over the raw body (rated CORRECT); the 01-Jun and 02-Jul audits describe require_hmac false and the URL token as the live control (R-11). The code could check a signature; the configuration named the token.
- C-07. The 403 label. The 05-Jul audit calls 20,461 answers of 403 "auth-rejects"; the 03-Jul investigation calls the 403s kill-switch or window refusals, "not an auth or IP problem"; the 06-Jul rule: "only 401 = webhook auth/token failure"; the code returns 403 only for the kill switch and the window (7.3, 9). Evidence: docs/audit/audit_05jul2026.md line 748; docs/audit/c2_chartink_ip_investigation_03jul2026.md line 26; docs/SYSTEM_MAP.md line 1364.
- C-08. Since when 401s were audited: U-08.
- C-09. The effect of removing the secret. The 04-Aug register calls commenting out WEBHOOK_SECRET "disabling an auth control"; on the pinned code path it stops the next start with "missing_secrets" (return 3), and the receiver is never built (4.4, 4.6, R-09). A receiver built some other way with no secret would be open (4.6), but no such construction exists in S-ALL outside tests (N-10; the test fixtures of 4.6).
- C-10. What a holder of the secret could do. 02-Jul and 05-Jul: inject signals (through the full pipeline); 01-Aug: "cannot place an order" (that is, no broker access). Both can hold in their own sense; the records do not discuss the other's point (R-09, R-11). Evidence: docs/audit/system_security_audit_02jul2026.md line 21; docs/audit/system_security_audit_02jul2026.md line 81; docs/audit/system_security_audit_02jul2026.md line 87; docs/audit/audit_05jul2026.md line 187; docs/audit/integrity_audit_2026.md lines 4286 to 4295.
- C-11. The allowlist's status. The 05-Jul and 06-Jul entries record a single-address allowlist applied and seen working; the 16/17-Jul AB-910 section 1.8 and the 20-Jul record still list allowlisting as a future fix. Evidence: docs/SYSTEM_MAP.md lines 1364 to 1365; docs/audit/audit_b_phase9_ops_phase10_security_16jul2026.md line 192; docs/audit/pre_receive_hook_investigation_20jul2026.md lines 211 to 214. C-002 section 5.2 holds the firewall account; its later state NOT ESTABLISHED.
- C-12. The S4 loopback reasoning. The 17-Jul outage record rejects a loopback exemption for the receiver's /health on a proxy argument it takes from S3; S3 concerned the dashboard behind a tailnet proxy, and the receiver saw Chartink's real address (C-002 section 5.4). A record's reasoning, recorded as such. Evidence: docs/audit/s4_boot_outage_17jul2026.md line 85.
- C-13. Rama's recollections and the records. The records attribute the 05-Jul allowlist and the Chartink URL update to Rama; Rama, Q5 and Q7: "Don't know / never checked". Runbooks describe /health checks; Q6: "No manual check I recall". Not a contradiction of fact; neither proves the other.

## 21. Negative claims and their scope (each "not found in" its scope only)
- N-1. No access-log middleware or request logger (TransLogger, access log, log_request) in S-RT (T08: 0 files).
- N-2. No receiver read of request.url, full_path, query_string or base_url in S-RT (T07: the receiver's one hit is request.args.get("token"); main.py's two are urllib.request.urlopen).
- N-3. No server TLS configuration in S-CODE (C-002 S19: 0 files).
- N-4. No reverse-proxy or tunnel configuration in S-CODE; comments and runbooks only (C-002 N3, searches S04 and S19; C-002 search S25 for ngrok).
- N-5. No runtime code other than main.py reads WEBHOOK_SECRET from the environment by its literal name, and none assigns it by name (T26 in S-ALL: main.py line 3414, three crash-test tools and four records; T04's S-RT hits are main.py, the receiver's error message and comment, and the two pre-flight lists). Not covered: reads through lists of names (4.4, 4.5) and generic loaders such as the import-time load_dotenv() of 4.3, which a name search cannot see.
- N-6. No length or strength check of the secret in S-CODE (T27: 0 files); no format check on the code path read (4.2, 4.4).
- N-7. No code outside tests computes an X-Webhook-Signature (T02: in code, the receiver and three test files only); the repository's own sender tools use ?token= (8.6).
- N-8. No redaction on the request path other than S-1B.1 (T16 in S-ALL: the other code hits, all off the request path, are the configuration snapshotter and its test, the pre-commit scanner, and the dashboard's logs template and its test; the first two are A-010's subjects, and none is imported by the receiver).
- N-9. No Werkzeug development server on the runtime path (T24 in S-RT: the receiver's usage docstring and the standalone scripts/test_webhook.py).
- N-10. No construction of WebhookReceiver outside main.py and tests (T09 in S-ALL).
- N-11. No redaction filter in core/logger.py (read by a grep and three ranges: its filters route by level, trade IDs and logger name, and it caps third-party HTTP loggers at INFO); not a scope search. Evidence: core/logger.py lines 98 to 104; core/logger.py lines 240 to 259.
The searches, their patterns and per-file lists: the source inventory, section 5. A text search cannot exclude behaviour reached by dynamic names, by other processes, or by installed files that differ from the repository.

## 22. Answer-sheet and research-scope status
Rama's answers (HISTORICAL; the answer sheet of 22-Oct-2026, answered through Claude's questions after 09:50 IST):

| Question | Answer | Cited in (section) |
|---|---|---|
| Q1 Was the query token intended rather than HMAC? | "Don't know / never checked" | 22 only |
| Q2 Was require_hmac changed in production? | "Don't know / never checked" | 19 (U-01) |
| Q3 Did testing use the same authentication mode? | "Don't know / never checked" | 14, 19 (U-05) |
| Q4 A production rejection for a bad credential? | "Don't know / never checked" | 22 (the paragraph below, with R-16) |
| Q5 When was the secret rotated? | "Don't know / never checked" | 13.1, 19 (U-03), 20 (C-13), 22 (the paragraph below) |
| Q6 A manual check of webhook authentication or health? | "No manual check I recall" | 20 (C-13), 22 (the paragraph below) |
| Q7 TLS, proxy, tunnel or network restriction? | "Don't know / never checked" | 12, 20 (C-13), 22 (the paragraph below) |
| Q8 A token seen in logs, payloads, alerts or screenshots? | "Don't know / never checked" | 22 only |
| Q9 The old token used after rotation, or invalidated? | "Don't know / never checked" | 13.1, 19 (U-03) |
| Q10 Other local records? | "None" | 22 only |
| Q11 A C-001 or C-002 statement to check? | "None" | 22 only |
| Q12 Scope: (a) the pin, (b) dead legacy, (c) no other snapshot, records repository, Chartink or VM, (d) no V2 work | "Confirm all four (Recommended)" | 1, 2, 14, 19 (U-05) |

How they relate to the records (stated, not reconciled): the records attribute the 05-Jul allowlist and the Chartink URL update to Rama (R-03; Q5, Q7), describe operator checks in runbooks (C-002 K-9; Q6) and record no 401 to 31-Jul (R-16; Q4). A recollection that is unavailable does not contradict a record; neither is treated as proof of the other (C-13).

The research prompt's scope, item by item:

| Scope item | This manual |
|---|---|
| Secret name and type, process wiring, environment acquisition, start-up presence checks, missing or empty behaviour, mode-specific checks | 4 |
| The pinned require_hmac declaration against comments, defaults and tests; configuration is not installed runtime | 5, 14, 19 |
| _authenticate's branch order and exact predicates: the header, the "sha256=" form, the signed bytes, the digest, constant-time comparison, the query token, missing, invalid and malformed credentials, and whether a token can rescue an invalid signature | 6 |
| The order relative to rate limiting, the scanner key, parsing, the EOD branch, the kill switch, backpressure, the window, body checks, audit writes and logs; each route on its own | 7, 8, 16 |
| Status and body by branch; the audit row; logs and redaction; whether error details reveal the mode | 7, 8, 9, 10 |
| /health: the empty body used for the HMAC, replay, the data after authentication, the rate-limit order | 6.5, 8 |
| The token in the URL and Chartink's echoed webhook_url; stored payloads; the raw-body warnings and their truncation; sanitisation; the records' dates and limits | 11 |
| HTTP or TLS; reverse proxy, tunnel and firewall claims, by state | 12, 20 (C-05, C-11) |
| Rotation and exposure history, where supported; never a secret | 13 |
| Paper and live, only where evidenced; tests are not production proof | 14, 15 |
| Negative searches with their exact scope | 21 |

## 23. Frozen manuals: cross-reference (read only; changed only through the errata of section 24)
- The sweep (source inventory, section 9) found 203 statements on the subject in 14 of the 36 files searched (25 manuals, 8 addenda, the 2 decision papers and the design log's Q and VP rows); the largest sets are C-002 (77), A-010 (35), B-007 (20) and C-001 (16). The source and records above agree with them except for the candidates CE-1 to CE-5: B-007's "signature" for the webhook control and A-008's pointer to A-006 (both C-002 NC-5, carried here); A-010's rotation history, which has 02-Jul and not 03-Jul (R-01, R-02); C-002's "rotated on 05-Jul" against R-02 and R-03; and A-010's "unauthenticated requests get 401" against 6.4 rows 4 and 8 (low). Rama's decisions on them: section 24.
- Already recorded and not repeated as new: C-002 K-5 (the nginx comments), K-6 (the bind comment), K-8 (the sanitiser's old line numbers), K-9 (the runbook's /health check); A-010 E-4 (which outcomes reach webhook_audit; ACCEPTED, DR-055); C-002 NC-3 (A-010's "by 27-Jul" for the allowlist).
- Statements confirmed by this step (no change): the gate order (A-008, A-011, B-008, B-009, C-001, C-002); the start-up presence check and exit 3 (A-010, B-002); the pre-flight's name checks (A-009, A-010); the 17-Jul outage (A-011, B-001, B-007); the receiver's /health limiter, 401 and state (B-007, C-002); WEBHOOK_SECRET's 0 log hits on 16-Jul (A-010; R-07 adds the 17-Jul count).
- Checked and not proposed (no change): A-009 line 251 and A-010 line 113 ("false in production"; "Production ran with require_hmac false"), supported by the records and the inference of 13.3, though no record quotes the installed file (U-01); C-001 line 263 (the source-address verdict left to C-003): answered in R-12 (the record gives no verdict), no change needed; B-007 line 85's dated evidence and the 17-Jul outage statements of A-011, B-001 and B-007, which agree with the records (R-14).

## 24. Corrections to frozen manuals (four errata, ACCEPTED, DR-056; exact texts)
Status: ACCEPTED (DR-056, 2026-10-22) and applied at the C-003 freeze. Four errata to four frozen manuals, found during C-003 and accepted by Rama on 22-Oct-2026, each on its own: his message of 15:14 IST ("I accepted your views": the view Claude gave on each item, one by one) and his approval at about 15:35 IST of the source named on CE-3's 03-Jul line (C-003_ERRATUM_DECISION_TABLE_22-OCT-2026.txt, final for the decision stage at 15:40 IST). ChatGPT's decision reply recommended the same positions; its drafting instruction asks for this section. No frozen manual was changed before Rama approved the freeze (section 29). The authoritative passages are this manual's sections 1 (the boundary with A-010), 5.1, 6.2 and 8.6 (the credential forms and the self-check), 13.1 (the rotation records), 19 (U-03) and 20 (C-01), with 13.2 (R-14) for a passage checked and not changed; each erratum cites them instead of restating them (V2_DESIGN_LOG lesson of 2026-10-13). As practised (C-002 section 22), the struck text stays visible with "(superseded by erratum E-n)" and the replacement follows; an added line begins "Added by erratum E-n:" (as A-011 line 124 does); each manual's status line gained one sentence, an erratum section was added at its end, and its review notes gained one status sentence; the register row DR-056 records the errata with the C-003 freeze. Removing each erratum's insertions restores the committed bytes at main d89164d (C-003_review_notes.md section 9.3). Numbers: B-007 E-1, A-008 E-6, A-010 E-5 and C-002 E-1, as the erratum-number rule gives them (CE-3 accepted, CE-5 option (a)); confirmed against the live register at 15:21 IST and again at 19:38 IST, immediately before they were written (V2 main d89164d: A-008 has E-1 to E-5, A-010 E-1 to E-4, B-007 and C-002 none; the last register row DR-055). The wording is the accepted wording; its references to C-003 items, "C-003 R-01" and the like, are pointed to this manual's sections, as the table's rule provides. Exact texts, with the unchanged text on either side for location (lines at V2 main d89164d):
- B-007 E-1 (CE-1; what the webhook control is called). Status: ACCEPTED WITH CHANGES (the wording note's text); applied at the C-003 freeze (DR-056). Reason: the word "signature" names a method that the pinned configuration does not use: it names the query token, either credential can pass while require_hmac is false, and the boot self-check sends none (sections 5.1, 6.2 and 8.6; C-002 NC-5). Impact: wording only; no finding of B-007 changes.
  - section 3, the row of the webhook's /health (H8) (B-007 line 52 at main d89164d):
    - Struck: "signature"
    - Replacement: "the webhook secret (the query token on the pinned configuration, or an HMAC signature made with it)"
    - Unchanged before it (its last 80 of 100 characters): "ort 5000 (H8) | signals/webhook_receiver.py lines 364 to 404 | Rate limiter and "
    - Unchanged after it (its first 80 of 254 characters): " | Anonymous: 401 and no state; authenticated: status, the in-memory kill flag, "
  - section 4, G16's pass condition (B-007 line 85 at main d89164d):
    - Struck: "without the signature"
    - Replacement: "without any credential"
    - Unchanged before it (its last 80 of 138 characters): "and 808) is any 2xx or 401 from the webhook /health, which the self-check calls "
    - Unchanged after it (its first 80 of 593 characters): " by design (lines 799 to 806). A 401 shows only that the webhook server answered"
  - section 4, G16's pass condition (B-007 line 85 at main d89164d):
    - Struck: "a signed signal"
    - Replacement: "an authenticated signal"
    - Unchanged before it (its last 80 of 310 characters): " answered (the comment's "Flask is listening", line 804); it does not show that "
    - Unchanged after it (its first 80 of 427 characters): " would be accepted, queued or processed end to end. Dated evidence: on 17-Jul-20"
  - section 6, row H8 (B-007 line 127 at main d89164d):
    - Struck: "signature"
    - Replacement: "the webhook secret (the query token on the pinned configuration, or an HMAC signature made with it)"
    - Unchanged before it (its last 80 of 116 characters): "ignals/webhook_receiver.py lines 364 to 404) | Behind the same rate limiter and "
    - Unchanged after it (its first 80 of 601 characters): " as /webhook since AB-910 section 1.7, which was in place at the 17-Jul start (m"
  - section 20, the unknowns (B-007 line 270 at main d89164d):
    - Struck: "a signed signal"
    - Replacement: "an authenticated signal"
    - Unchanged before it (its last 10 of 10 characters): "- Whether "
    - Unchanged after it (its first 73 of 73 characters): " was accepted after a start whose self-check passed on a 401 (section 4)."
  - Checked, not changed: no other "signature", "signed" or "HMAC" in B-007 at main d89164d.
- A-008 E-6 (CE-2; a cross-reference). Status: ACCEPTED, as proposed; applied at the C-003 freeze (DR-056). Reason: A-006 has no webhook content (C-002 NC-5, second part); webhook authentication is A-010's (the secret) and this manual's subject. Impact: a cross-reference only.
  - section 24, the cross-references (A-008 line 279 at main d89164d):
    - Struck: "Broker and webhook authentication: A-006, A-010."
    - Replacement: "Broker authentication: A-006, A-010. Webhook authentication: A-010, C-003."
    - Unchanged before it (its last 80 of 106 characters): " A-003. Instruments and universe: A-004. Capital, sizing and risk gates: A-005. "
    - Unchanged after it (its first 80 of 323 characters): " Products and protocols: A-007 (erratum E-1 found during A-008). Configuration m"
  - Checked, not changed: the other cross-references of A-008 section 24 (all on line 279).
- A-010 E-5 (CE-3; the rotation history). Status: ACCEPTED WITH CHANGES (the wording note's text for line 36, and the source of the 03-Jul line as approved at about 15:35 IST); applied at the C-003 freeze (DR-056). Reason: the section is the manual's history and lacks two dated records (03-Jul and the 05-Jul GATE C entry) and the 06-Jul check's limit (section 13.1, R-02 to R-04); the records conflict and stay in conflict (C-01), and how many rotations there were is not established (U-03). Impact: history completeness; the token stays treated as exposed (VP-50) in every case.
  - section 12, three lines added after the 02-Jul line (A-010 line 133 at main d89164d), each line marked as the route marks an addition:
    - Added: "- Added by erratum E-5: 03-Jul-2026 (system map and paths records; the 05-Jul audit): WEBHOOK_SECRET rotated, VM-only, the old token having been exposed in chat; the VM's .env modified 03-Jul at 23:03; to load at the 06-Jul start. The 02-Jul and 03-Jul records conflict on where and when (C-003_manual.md section 13.1, R-01 and R-02; section 20, C-01)."
    - Added: "- Added by erratum E-5: 05-Jul-2026 (S-1A, GATE C): a WEBHOOK_SECRET rotation staged (old and new identified by hashes only) with the Chartink URL update; whether it is the 03-Jul rotation or a further one is not stated (C-003_manual.md section 13.1, R-03; section 19, U-03)."
    - Added: "- Added by erratum E-5: 06-Jul-2026 (GATE D): a passive check only: zero 401 that day; the active test of the old token was not run (C-003_manual.md section 13.1, R-04)."
    - Unchanged before them (line 133, its last 80 of 266 characters): "ot rotated (rotation "Optional / recommended"; "non-authenticating on its own")."
    - Unchanged after them (line 134, its first 80 of 96 characters): "- 14-Jul-2026 (forensics, section 14): broker key rotation called "your call, NO"
  - section 3, the webhook secret's row (A-010 line 36 at main d89164d):
    - Struck: "rotated 02-Jul"
    - Replacement: "rotated 02-Jul and 03-Jul (records in conflict; how many rotations, not established: C-003_manual.md section 20, C-01; section 19, U-03)"
    - Unchanged before it (its last 2 of 199 characters): ", " (shorter than usual: the text before it names a commit pointer, which the C-003 files do not reproduce; the struck text occurs once in the line)
    - Unchanged after it (its first 24 of 24 characters): "; unchanged on 25-Jul. |"
  - Checked, not changed (every other "rotated 02-Jul" in A-010, by a wording search, and the section-12 lines on the webhook secret): lines 30, 31 and 34 (the rows of the Zerodha API secret, the TOTP seed and the bot token); line 133 (the 02-Jul line), line 135 (25-Jul) and line 209 (section 21: "secrets rotated 02-Jul", the 02-Jul record's date), which stay true as written.
- C-002 E-1 (CE-4, option (a); the rotation date). Status: ACCEPTED, option (a); applied at the C-003 freeze (DR-056); the exact text below was drafted by Claude at 15:21 IST, reconciled with A-010 E-5, and was reviewed with this manual. Reason: the 05-Jul date is the GATE C record's own date and stays; a separate record dates a rotation to 03-Jul (section 13.1, R-02 and R-03), and how many rotations there were is not established (U-03). Impact: dates only; C-002's finding (no 401 on 06-Jul) is unchanged.
  - section 4.2, the bullet on the 05 and 06-Jul records (C-002 line 77 at main d89164d):
    - Struck: "after the token was rotated on 05-Jul ("Chartink URL updated by Rama")"
    - Replacement: "after the token was rotated on 05-Jul ("Chartink URL updated by Rama"; that is the date of the GATE C record, which staged the new token for the next start; a rotation is also recorded on 03-Jul, VM-only, in the system map and paths records; how many rotations, not established: C-003_manual.md section 13.1, R-02 and R-03; section 19, U-03)"
    - Unchanged before it (its last 45 of 45 characters): "- FACT (RECORD-DERIVED, 05 and 06-Jul-2026): "
    - Unchanged after it (its first 80 of 240 characters): ", the first trading-day POSTs on 06-Jul were 86 answered 200 and 309 answered 40"
  - Checked, not changed: line 95, section 4.5 ("the 05-Jul rotation note" names that record by its own date, which option (a) keeps); section 14's arrival records, which name no rotation (C-002 names one only on lines 77 and 95).

A-010 E-5 and C-002 E-1 reconciled (the erratum decision table, "CE-3 AND CE-4 RECONCILED"): both give 03-Jul for the system map and paths records and 05-Jul for the GATE C record; both call GATE C staged; both say "how many rotations, not established", with U-03; A-010's 03-Jul line also names the 05-Jul audit, which gives its .env time, while C-002's clause states only the 03-Jul rotation and VM-only, which the system map and paths records give. Neither settles U-03 or C-01.

Checked, with no erratum (recorded here only):
- CE-5, A-010 line 113 ("unauthenticated requests get 401"): Rama chose option (a), no change; the sentence holds for every recorded request. The non-ASCII case is K-03 here, an INTERPRETATION from the source and a library probe, not observed (U-09). No number is used.
- A-009 line 251 and A-010 line 113 ("false in production"; "Production ran with require_hmac false"); C-001 line 263 (answered in R-12); B-007 line 85's dated evidence and the 17-Jul outage statements of A-011, B-001 and B-007 (section 23).

Done at the C-003 freeze (section 29; C-003_review_notes.md section 5): the numbers' last check against the live register (19:38 IST); the in-place edits, status-line sentences and erratum sections of B-007, A-008, A-010 and C-002, and one status sentence in each of their review notes; the register row DR-056 recording them with the freeze; and the other freeze-time items of section 25.5.

## 25. Process record: how the research was done, its exceptions, Rama's decisions and the freeze-time items

### 25.1 How the evidence was read and checked
- Code, configuration and tests: through the masking filter (mask_read2.py with a second masking stage), every read from 10:17 IST logged with its blob ID checked against the tree (earlier reads: D-2); host records: by a separate records agent through a third masking stage behind a residual counter, and by Claude's value-blind presence tests, two narrow greps through the records agent's gate and, before the read log existed, one range read through the same mask (D-1) (source inventory, sections 3, 4, 6 and 7).
- Never-open check: 8,922 local objects and 0 never-open blobs at every check, including those of revision 0's start and end (section 28; the drafting report) and of the freeze (C-003_review_notes.md sections 2 and 9.3); no forbidden blob was fetched or read.
- Checks of the research files: a citation check (no citation out of range), 206 content checks of cited ranges, every quoted phrase looked up in its source (264), value-blind scans; then an independent check of the review copies (28 findings, all applied) and an independent final QA of the final bytes (33 findings, then 5 re-check points, all resolved; S-01 optional) (source inventory, sections 12 and 13; the QA report).

### 25.2 Process exceptions and deviations, disclosed (source inventory, section 4)
- E-1, a working-output exposure in the records agent's context, no file affected: the third masking stage left three items unmasked in lines the agent printed (a vendor API host name in a quoted log line, a short fragment of an SSH public key with its key comment, and the alerting bot's account username); all residual counts were 0, because none of the three has the shapes the counter tests. The agent reported their locations only; Claude scanned its report value-blind before reading it and did not read those lines. Nothing from them is in any delivered file or in this manual. None is a webhook credential; they concern the deleted legacy VM and the legacy alert bot, whose token is already treated as exposed (VP-50).
- D-1: the records agent printed 17 records in full and 22 by ranges and greps through the third stage, after the residual counter reported 0, a deviation from the wording of the 2026-10-21 lesson (host records "only through narrow pattern matches, never by line range"). D-2: the session's context was compacted at about 10:12 IST, and reads before the code read log began (10:17 IST) are not listed one by one; every claim rests on a read logged after that point or on a saved output. D-3: one search pattern stopped the filter before it searched and was re-run. D-4: the records agent's own presence and size checks, value-blind.
- The final QA's own disclosed slips (source inventory, section 12.8): five patterns that failed to compile (nothing searched); two early patterns that carried the well-known all-interfaces and loopback addresses as alternatives (both failed; the helper's guard was then tightened); and a walk of the V2 copy that listed names under the inputs folder without opening any file.
- While drafting revision 0, one disclosed deviation from the drafting instruction's rule 5 ("No synthetic test or additional host-record work is authorised by this drafting instruction"): two value-blind operations on host records: one presence test of a plain word over the first five lines of one record, run while Claude set up the checker's tools, which printed only PASS and two line numbers; and one line count of another record, for two citations' ranges. Nothing from a record was printed and no claim rests on either; no printed read used a line range, but the presence test read its five lines inside its wrapper. The record wrapper was then not given to the checker, and the checker and its re-check read no host record. By design, and not an exception, the masking filter reads the system map (docs/SYSTEM_MAP.md) and the deployment file at the repository root on each call to build its mask and prints nothing from them (source inventory, section 4). Records named and details: the drafting report, B6 and B8.

### 25.3 Rama's decisions of 22-Oct-2026
- About 12:50 IST (C-003_DECISION_REQUIRED_MASKING_METHOD_22-OCT-2026.txt, section 6): host records are read value-blind only from then on (counts, or the presence of probe words; PASS or FAIL and line numbers; no host-record line printed), and the line-range method is not approved for further reading; the lesson row of 25.4 is approved as written, for the design log at the C-003 push; the records agent's session transcript was deleted unopened at 12:52 IST, and nothing delivered depended on it; the seven corrections PC-1 to PC-7 to the research files approved.
- About 13:43 and 14:00 IST: the 33 corrections after the final QA (with the two repository references shortened to their names, F-32 (a)) and the five re-check points N-01 to N-05 approved; both applied and re-checked by the same QA agent.
- 15:14 IST and about 15:35 IST: CE-1 to CE-5, the erratum-number rule and the CE-3 source point (section 24); S-01 left as is (C-003_CLAUDE_RESPONSE_TO_CHATGPT_REVIEW_22-OCT-2026.txt, sections 6 and 7).
- About 18:28 IST, when Rama sent it: the instruction to finalise revision 0 as a review draft (C-003_FINALISE_REVISION_0_FOR_CHATGPT_REVIEW_22-OCT-2026.txt), whose Authority line records Rama's authorisation; section 28.
- In answer to Claude's question during the drafting (recorded in the drafting report): an independent check of the draft by a separate agent before it went to ChatGPT, as the 2026-10-21 lesson asks (section 28).
- About 19:30 IST, after ChatGPT's independent review of revision 0: "Approved and Freeze"; recorded in DR-056 together with the four errata of section 24 (section 29).

### 25.4 The approved lesson row: written to the design log at the C-003 freeze
The row as Rama approved it (the decision note, section 3), written unchanged to V2_DESIGN_LOG section 1 at the freeze (DR-056):
> | 2026-10-22 | During C-003's research, the new stage-3 mask and its residual counter let three classes through in a records agent's working output (a host name in a quoted log line, an SSH key fragment with its comment, a non-e-mail account name); nothing reached a file. | Read host records value-blind only (counts, or the presence of probe words) unless a filter has passed a synthetic-fixture test for host names in quoted text, key material and comments, non-e-mail account names and other identifier shapes, and Rama has approved it; never print a host-record line through an untested filter. |

### 25.5 Freeze-time items, done in the C-003 freeze package (DR-056)
- The four errata of section 24 written into B-007, A-008, A-010 and C-002, with their numbers re-confirmed against the live register first (19:38 IST; section 24).
- The register row DR-056 for the C-003 freeze, recording the four errata; the design-log lesson row of 25.4 with the other lessons, the tracking notes, the agenda status row (V2_DESIGN_LOG section 4, the repository's only status index at main d89164d) and the session notes (C-003_review_notes.md section 7).
- README.md, which Rama held from the research delivery of 22-Oct, in the package unchanged.
- The package's diff against main d89164d checked: only the 32 files of the package change (C-003_review_notes.md section 9.3).
- The SAVE list (W-2) and the push commands (W-8: the Co-Authored-By line only, no session link), given with the final files (C-003_PUSH_COMMANDS_22-OCT-2026.txt; not stored).
Claude stored, committed and pushed nothing; Rama stores and pushes the package (C-003_review_notes.md section 11).

## 26. Cross-references and the complete list of evidence

### 26.1 Cross-references
A-001 and DR-012 (both legacy VMs terminated); A-008 section 24 (the cross-reference; erratum E-6, accepted and applied at the C-003 freeze (DR-056)); A-009 section 16 and line 251 (require_hmac and the query token); A-010 sections 3, 5, 10, 11, 12, 17 and 19 (the secret's location and the environment file; erratum E-2, the import-time load; erratum E-4, which outcomes reach webhook_audit; the rotation history, erratum E-5, accepted and applied at the C-003 freeze (DR-056)); A-011 sections 5 and 14 (the start order; the 17-Jul outage); B-001 (the unit's handling of the exit codes; the 17-Jul outage); B-002 (configuration loading and the start's exit 3; A-010 erratum E-2); B-006 (the start's threads); B-007 sections 3, 4, 6 and 20 (G16, H8 and the two /health routes; erratum E-1, accepted and applied at the C-003 freeze (DR-056)); C-001 section 8 and line 263 (webhook_audit's readers; the source-address verdict left to C-003); C-002 sections 4 to 14 and 18 to 22 (the URL form, the network path, the gates, the audit's coverage, the arrival records R5 and R7, K-5, K-6, K-8, K-9, U-6, NC-3 and NC-5; erratum E-1, accepted and applied at the C-003 freeze (DR-056)); C-004 to C-011 (the owners downstream of authentication); DR-001, DR-008, DR-016, DR-036, DR-037, DR-054 and DR-055; V2_DESIGN_LOG W-1 to W-8, the lessons of 2026-10-13 and 2026-10-21, VP-50, VP-55, VP-56 and Q-36 (j), as tracking destinations only.

### 26.2 Legacy files cited (at 970aabf; blob IDs, ranges read and reasons in the source inventory, sections 6 and 7)
Code, configuration and tests read and cited (27):
- config/system_config.yaml
- core/config_loader.py
- core/logger.py
- deploy/install_vm_services.sh
- deploy/systemd/trading-system.service
- main.py
- ops_dashboard/backend/auth.py
- ops_dashboard/backend/readers/db_reader.py
- ops_dashboard/backend/services/analytics_period.py
- requirements.txt
- scripts/healthcheck_server.py
- scripts/load_test_signals.py
- scripts/preflight/checks/config_integrity.py
- scripts/premarket_healthcheck.py
- scripts/zerodha_login.py
- signals/webhook_receiver.py
- tests/crash_test/ct143_afternoon_inject.py
- tests/crash_test/ct145_rapid_crash.py
- tests/crash_test/signal_injector.py
- tests/integration/conftest.py
- tests/integration/test_hardening_scenarios.py
- tests/unit/test_c2_webhook_lockdown.py
- tests/unit/test_c6_auth_collapse.py
- tests/unit/test_fix134_backpressure.py
- tests/unit/test_s4_webhook_degrade.py
- tests/unit/test_webhook_receiver.py
- utils/startup_checks.py

Dated records read and cited (27 of the 39; each with its date as the source inventory, section 7.1, gives it):
- PATHS.md: dated entries (03 to 17-Jul used)
- docs/MASTER_PENDING_01-Aug-2026.md: 01-Aug, with a 04-Aug addendum
- docs/MASTER_PENDING_28-Jul-2026.txt: 28-Jul
- docs/SYSTEM_MAP.md: dated change-log entries (2 to 17-Jul used)
- docs/architectural_audit_2026-06-01.md: 01-Jun
- docs/audit/audit_05jul2026.md: 05-Jul
- docs/audit/audit_b_phase9_ops_phase10_security_16jul2026.md: 16-Jul
- docs/audit/audit_remediation_status_02jul2026.md: 02-Jul
- docs/audit/batch2_done_17jul2026.md: 17-Jul
- docs/audit/c1_credential_exposure_inventory_02jul2026.md: 02-Jul
- docs/audit/c2_chartink_ip_investigation_03jul2026.md: 03-Jul
- docs/audit/c6_auth_collapse_25jul2026.md: 25-Jul
- docs/audit/f2_auth_separation_25jul2026.md: 25-Jul
- docs/audit/f2_injection_scoping_25jul2026.md: 25-Jul
- docs/audit/fixture_blindness_investigation_17jul2026.md: 17-Jul
- docs/audit/followup_investigation_15jul2026.md: 15-Jul
- docs/audit/full_system_audit_04july2026.md: 04-Jul
- docs/audit/integrity_audit_2026.md: 31-Jul to 01-Aug (by phase)
- docs/audit/ledger11_secrets_surface_measurement_04aug2026.md: 04-Aug
- docs/audit/p1_health_require_hmac_done_17jul2026.md: 17-Jul
- docs/audit/pre_receive_hook_investigation_20jul2026.md: 20-Jul
- docs/audit/receiver_auth_c6_scoping_2026-07-25.md: 25-Jul
- docs/audit/refused_posts_403_investigation_23jul2026.md: 23-Jul
- docs/audit/s4_boot_outage_17jul2026.md: 17-Jul
- docs/audit/sweep_done_17jul2026.md: 17-Jul
- docs/audit/system_flow_review_15jun2026.md: 15-Jun
- docs/audit/system_security_audit_02jul2026.md: 02-Jul

Read and not cited here: records (12): DEPLOYMENT.md; docs/06_deployment_guide.md; docs/CONFIG_GUIDE.md; docs/MASTER_PENDING_30-Jul-2026.md; docs/audit/batch_classification_16jul2026.md; docs/audit/c1_history_purge_runbook_02jul2026.md; docs/audit/f2_surface_enumeration_24jul2026.md; docs/audit/f2_surface_recheck_25jul2026.md; docs/audit/liveness_alarm_17jul2026.md; docs/audit/q8_vm_security_breach_forensics_14jul2026.md; docs/audit/security_items_scoped_25jul2026.md; docs/first_day_live_runbook.md; code (2): scripts/refresh_instruments.py; tests/conftest.py.

Named here but not read (search hits named in a negative claim): scripts/test_webhook.py.

### 26.3 Library sources and probes (CURRENT EXTERNAL FACT, 2026-10-22; hashes in the source inventory, section 8)
waitress/parser.py, waitress/channel.py, waitress/task.py and waitress/server.py (waitress 3.0.2); flask/app.py (Flask 3.1.3); the probes auth_probe.py, auth_probe2.py and pydantic_probe.py (Python 3.13.16, Flask 3.1.3, Werkzeug 3.1.9, pydantic 2.13.0).

### 26.4 Research inputs and review records (22-Oct-2026)
C-003_CLAUDE_RESEARCH_PROMPT_22-OCT-2026.txt; C-003_ANSWER_SHEET_FOR_CLAUDE_22-OCT-2026.txt; C-003_SOURCE_INVENTORY_AND_METHOD_TEMPLATE_22-OCT-2026.txt; the three research files of the header; C-003_INDEPENDENT_FINAL_QA_22-OCT-2026.txt; C-003_PROPOSED_CORRECTIONS_AFTER_FINAL_QA_22-OCT-2026.txt; C-003_DECISION_REQUIRED_MASKING_METHOD_22-OCT-2026.txt; C-003_ERRATUM_DECISION_TABLE_22-OCT-2026.txt; C-003_CLAUDE_RESPONSE_TO_CHATGPT_REVIEW_22-OCT-2026.txt; ChatGPT's reviews, reply and instruction of 22-Oct-2026 (CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_22-OCT-2026.txt; CHATGPT_VALUE_ADDED_REVIEW_TO_CLAUDE_C-003_FINAL_22-OCT-2026.txt; CHATGPT_VALUE_ADDED_REPLY_TO_CLAUDE_C-003_DECISIONS_22-OCT-2026.txt; C-003_CLAUDE_INSTRUCTION_TO_DRAFT_MANUAL_REVISION_0_22-OCT-2026.txt; C-003_CHATGPT_INDEPENDENT_REVIEW_22-OCT-2026.txt) and its withdrawn follow-up review, not used; the finalisation instruction C-003_FINALISE_REVISION_0_FOR_CHATGPT_REVIEW_22-OCT-2026.txt, which Rama sent and which names no author (25.3; C-003_review_notes.md section 1); Claude's C-003_REVISION_0_DRAFTING_REPORT_22-OCT-2026.txt ("the drafting report") and C-003_REVISION_0_DELIVERY_MANIFEST_22-OCT-2026.txt; and C-003_review_notes.md. Their exact versions (SHA-256 and git blob IDs) and where each is stored: C-003_review_notes.md section 1 (the review notes' own values: with the push commands).

## 27. Final C-003 statement
> At the baseline the legacy webhook had one secret, the environment variable WEBHOOK_SECRET, which main.py read from the process environment (the unit's environment file, and the .env file loaded when main.py imported the login script) when it built the receiver, after a start-up check that required the name to be set and non-empty in every mode; a missing or empty value ended the start with exit 3 before any receiver existed, and no check of the secret's length or strength was found in the inspected scope. One function, _authenticate, decided for both routes: with no secret configured it passed every request (a test-only path on main.py's code path, 4.6); otherwise a header "sha256=" plus hex was compared by hmac.compare_digest with HMAC-SHA256 of the route's bytes and was final either way; otherwise require_hmac true refused; otherwise a non-empty ?token= was compared with the secret by hmac.compare_digest; otherwise the request was refused. The pinned configuration declares require_hmac false, so the query token was the accepted Chartink method at the repository level; the value installed on either VM is not established, and the records support false only by inference from their zero counts of 401s. On POST /webhook, authentication ran after the body read, the shutdown gate and the per-address limit, and before the scanner key, the EOD route and the kill, backpressure, window and body checks; a refusal was answered 401 with the failing branch's message and audited without any credential (the row is attempted, not guaranteed), and the receiver logged no line for it. The receiver's GET /health signed an empty body, so its valid signature was a constant which, by a source-derived reading, also authenticates an empty-body POST; it answered one uniform 401, wrote no audit row, and gave the kill state and queue depth to an authenticated caller; the boot self-check called it without a credential and counted a 401 as reachable. A credential with a non-ASCII character would have raised a TypeError that _authenticate does not catch, answered 500 on both routes (an interpretation from the source and a library probe made through Flask's test client, not observed; on the serving path waitress parses the request first, and that path was not probed: 6.3, K-03). The token travelled over plain HTTP in the query string and, by a 05-Jul record, inside Chartink's body; the stored copy was redacted from S-1B.1 (05-Jul) on, while the 400 warnings kept the first 1,000 or 500 raw bytes, which held the token only when webhook_url fell within them, and no 400 is recorded in the windows of the records to 31-Jul. The records date rotations to 02-Jul and 03-Jul and a staged one to 05-Jul, and they conflict; how many rotations there were, and whether a retired token was ever refused, is not established, and the legacy token stays treated as exposed. What either VM installed and ran, what its libraries and network logged, and whether Chartink ever sent a signature are not established.

## 28. Revision 0 (2026-10-22)
- Drafted by Claude from its three research files (the bytes the independent final QA confirmed) and the decision records listed in the header, on ChatGPT's drafting instruction of 22-Oct-2026; there is no ChatGPT draft to work from. Input versions were checked before drafting against the hashes the delivery notes give, where a note gives one (the drafting report).
- Added beyond the research files: the HTTP status table and its two closing bullets (section 9); the token-location summary (11.7); the decision flow, the two sequences, the architecture sketch, the trace table and the end-to-end walkthrough (6.4, 7.7, 8.7, 16.1, 16.2 and 16.3); section 15's "Does not prove" column; section 22's "Cited in" column and research-scope table; sentences written for the drafting instruction's checklist (4.6, why the endpoint is not called "open"; 8.6, what the self-check's 401 shows; 13.3, what a passive zero-401 count does not show); from C-002, the framework's OPTIONS answer (7.2, 7.7, 9 and 16.3) and, for the 413, the 1 MiB limit's source line and C-002's probe (9); the labels FACT (TEST SOURCE) and OPEN QUESTION; the terms for the process IDs (section 3); the exact erratum texts with their anchors, computed from the frozen manuals at V2 main d89164d (section 24); the process record (25); and the list of evidence (26).
- No source fact, count or citation of the research files was changed, and no label of a legacy fact, except that statements about test code carry FACT (TEST SOURCE), as the drafting instruction asks, and that three statements were narrowed to what their sources show (section 15's test-client sentence, checked against the pinned source; C-09's "outside tests", as N-10 states; L-06's windows, as 11.5 states). These and the other corrections made while drafting are listed in the drafting report.
- Before delivery, an independent check by a separate agent (section 25.3); its findings and their disposition are in the drafting report.
- Finalised as a review draft at 18:44 IST, on the instruction of section 25.3, not a freeze: the two medium findings of the independent check (IC-01, the serving-path qualification; IC-02, the FACT (TEST SOURCE) labels) confirmed in place, the final statement's K-03 clause given the research's exact scope (6.3), two further statements about test code labelled (4.6; C-04), and the checks re-run; an independent re-check of the 18:31 bytes confirmed 16 of the 17 fixes and raised seven Low points (RC-01, on IC-04's new wording, and RC-02 to RC-07), all applied after it and verified by Claude's scripted checks; the agent did not re-check them (the drafting report).
- No frozen manual, register, design-log row, Project document or repository was changed; nothing was stored, staged, committed or pushed. The never-open check stood at 0 at the start and at the end of the drafting.

## 29. Revision 1 and freeze (2026-10-22)
- ChatGPT's independent review of revision 0 (C-003_CHATGPT_INDEPENDENT_REVIEW_22-OCT-2026.txt, which Rama sent at about 19:25 IST) hashed the three delivered files from their bytes (the manual and the drafting report matching the delivery manifest's values; the manifest, which cannot carry its own hash, matching a value ChatGPT had from outside the files), found the draft "technically strong", and gave "No freeze yet" until its freeze checklist was done under Rama's explicit instruction. Its qualifications are kept: A, the re-check's seven Low points were verified by Claude's scripted checks, not re-checked by the agent (section 28 now says so); B, two inputs, the corrections note and the masking-method decision note, have no reference hash in any delivery note, so the drafting report gives their local hashes only (its B1, note a), and their stored bytes are fixed in C-003_review_notes.md section 1; C, the drafting's two value-blind host-record operations are disclosed in 25.2, as in the drafting report (B6 and B8), and not simplified; D, the bounded gaps stay as 26.2 states them (scripts/test_webhook.py named and not read; 12 records and 2 code files read and not cited); E, the qualifications on the serving path, the installed configuration, the runtime, network logging and the refusal of a retired token stay as written (K-03, U-01, U-03 and U-04; sections 6.3, 13.3, 14 and 27). Its checklist was done in the freeze package (25.5; C-003_review_notes.md sections 8 and 9.3).
- Rama approved the freeze on 22-Oct-2026 at about 19:30 IST ("Approved and Freeze"); recorded in DR-056 together with the four errata of section 24, applied after their numbers were confirmed again against the live register at 19:38 IST.
- Revision 1 changed only: the status, date and built-from lines and the label line's errata clause; section 2's sentence on the process exceptions; section 23's heading; section 24's heading, status paragraph, each erratum's status, three of its "checked, not changed" lines (one of them new) and its closing note; section 25's heading, 25.1's never-open bullet, the bullet on the drafting's deviation in 25.2 (new), the freeze bullet in 25.3 (new) and the wording of the bullet before it ("during the drafting"; "went"), 25.4's heading and first line, and 25.5; 26.1's four erratum statuses; 26.4's list (ChatGPT's independent review, the drafting report, the delivery manifest and the review notes added; the finalisation instruction listed apart, as it names no author); section 28's re-check sentence (A above); and this section. No finding, number, citation or label of a legacy fact changed.
