# C-002 SCANNER TO WEBHOOK: LEGACY MANUAL

Status: FROZEN 2026-10-22 (revision 1, DR-055). Revision 0 (Claude, 21-Oct-2026) was drafted from Claude's research files and supplement, ChatGPT's two reviews and its continuation prompt, and Rama's answers; ChatGPT's value-added finalisation review of revision 0 (22-Oct-2026) gave conditional acceptance and asked for no change of a legacy fact. Rama approved the freeze on 22-Oct-2026 ("Freeze, with VP-45 change (Recommended)") and accepted the nine errata of section 22 ("Accept all nine (Recommended)"), recorded in DR-055. Revision 1 changed only what section 26 lists; no finding changed.
Date: 2026-10-22 (revision 1; revision 0 of 2026-10-21; frozen 2026-10-22, DR-055)
Scope: legacy facts only. A legacy behaviour reaches V2 only through a row in V2_DECISION_REGISTER.md (DR-016; V2_DESIGN_LOG W-1). Rama's input of 21-Oct-2026 on two systems sharing one Chartink login is V2 material, recorded in C-002_v2_design_inputs.md; it is not a legacy fact and is not decided here. No VM, service, host, broker, webhook, endpoint or Chartink operation and no change to legacy source; the legacy system is DEAD / REFERENCE ONLY.
Built from: Claude's repository answers and source inventory of 21-Oct-2026 and its supplement of the same day (research inputs, stored unchanged at the freeze; the supplement as corrected at 20:40 IST), ChatGPT's value-added review of 21-Oct-2026, its reconciliation review of the research files with the supplement and its continuation prompt for revision 0 (both 21-Oct-2026), Rama's answers of 21-Oct-2026 (sections 4.2 and 13.1) and his three screenshots of 21-Oct-2026 (not stored, DR-008; no value from them is reproduced), and, for revision 1, ChatGPT's value-added finalisation review of revision 0 and its reply (both 22-Oct-2026) and Rama's statement and decisions of 22-Oct-2026 (sections 4.3 and 26). ChatGPT sent reviews and a prompt instead of a reconciliation sheet and a manual draft; Rama chose on 21-Oct that revision 0 uses the repository's file set with the errata as diffs (his answer: "Repository set, errata as diffs (Recommended)"). Where this manual and the research files differ, C-002_reconciliation_addendum.md section 3 gives the correction.
Labels: FACT (SOURCE-VERIFIED) = trading-system-prod-snapshot at 970aabf3c78d777d9d34549e021f7c8e756c7bdb ("the baseline", dated 2026-09-11), read on 2026-10-21 through the masking filter; "path lines A to B" are line numbers there. FACT (CONFIG) = declared in a configuration file at the baseline; not proof of use at runtime. FACT (RECORD-DERIVED) = a dated record in the baseline repository (an audit note, a system-map or paths entry, a specification, or a code or configuration comment recording a measurement or event), true for its own date and scope only; the legacy database and logs no longer exist, so none can be re-measured. HISTORICAL (Rama) = his answers, dated. CURRENT EXTERNAL FACT = checked on 2026-10-21 outside the legacy repository (W-5): the pinned library source, a local library probe, Chartink's public pages, or what Chartink displayed in Rama's screenshots; never evidence of what the legacy system or Chartink did on an earlier date. INTERPRETATION / AUDIT LEAD = a reasoned reading of source or records, not observed. KNOWN DEFECT / AMBIGUITY = a defect or ambiguity visible in the source. KNOWN DOC DRIFT = a comment, docstring, record or document that the code or other evidence contradicts or does not support (the code wins over a comment; records that disagree are listed together). UNKNOWN / NOT ESTABLISHED. Frozen V2 manuals are cross-referenced by section ("A-011 section 12") and are not reopened, except through the errata of section 22 (ACCEPTED, DR-055).
Search scopes behind negative statements (answers file section 0.3): S-ALL = the 1,262 files checked out; S-PY = its 734 .py files; S-RT = the 281 .py files outside tests; S-CODE = its 869 code and configuration files. A negative reads "not found in <scope>"; a pattern search cannot exclude behaviour reached by dynamic names, external processes or installed files that differ from the repository.
Security: no credential, token or token-derived value, account or client identifier, host or address, mailbox, SSH detail, or person name other than Rama's is reproduced. Webhook authentication is described by mechanism and location only (C-003 owns it). Webhook URLs are described by their form only; the legacy token is treated as exposed and is repeated nowhere.

## 1. Purpose and boundary
C-002 records, at the baseline and in the dated records:
- what the repository declares about the sender (Chartink) and what it cannot show;
- the network path and the HTTP server that carried the webhook, and when it listened;
- the route, what the receiver read from a request, and the answer it gave, branch by branch;
- what the request audit (webhook_audit) and the logs record, and what they miss;
- the hand-off to the next component after an accepted request (boundary only);
- what happened when the endpoint was unreachable or a request was refused, and what is known of retries;
- the dated records of arrivals and their limits.

Out of scope, cross-referenced only: the scanner definitions, the twin URL file and the connectivity checks (C-001, frozen); authentication and the secret (C-003; the secret's location: A-010 section 10, frozen); field and symbol validation (C-004); deduplication and the in-flight claim (C-005); the queue, persistence, processing and expiry (C-006 to C-009); the scanner-to-strategy mapping (C-010); the EOD route (C-011); the start order and the start-up checks (A-011, B-006 and B-007, frozen); shutdown (B-011, frozen).

## 2. Evidence
| Evidence | What it covers | Limit |
|---|---|---|
| Baseline source and configuration | S-ALL; the files read, their ranges and blob IDs in the source inventory (section 6, 76 files) and, for revision 0's later reads, C-002_review_notes.md section 2 | Source is not proof of what was installed or ran on the VM on any date (U-6) |
| Records in the production repository | The crash-test note (04-Jun-2026), the security audit and the dashboard-backend report (02-Jul), the C2 source-address investigation (03-Jul), the system-map and paths entries (05 and 06-Jul), the PB-01 specification and the day reconstruction (15-Jul), the 17-Jul and 25-Jul notes, the signal-mortality census (19-Jul), the refused-POSTs investigation (23-Jul), the service-window records (24 and 25-Jul), the integrity audit (Phase 1 measured 31-Jul; its security phase 01-Aug; a 03-Aug note), the 04-Aug records, the assumptions record and the runbooks; read through the masking filter by range or pattern, and host records by narrow patterns only after exception E-1 (C-002_review_notes.md section 2) | Each true for its own date and deployed code |
| Library source and a local probe | waitress 3.0.2 (its adjustments module; in revision 0 also its channel and server modules); a minimal Flask app with the receiver's route rule, methods and body limit, run through Flask's test client under Flask 3.1.3 and Werkzeug 3.1.9, with no legacy code, no socket and no network (source inventory, section 7) | The probed versions only; the VM's installed versions are UNKNOWN (U-6) |
| Chartink's public pages, read 21-Oct-2026 | "Webhook support for Alerts" (dated 16-Mar-2020, updated 02-Jun-2021) and the "Alerts Guide" (posted 02-Feb-2018, page dated 17-Jun-2026), through a summarising fetch | Current statements, paraphrased; not the legacy period |
| Rama's screenshots of 21-Oct-2026 | Two of Chartink (the dashboard; one testing alert's settings and displayed payload) and one of his chat with ChatGPT (section 4.3) | What Chartink displayed that day for testing scans; not production, not the live period |
| Frozen V2 manuals, read at V2 main 75c1eb7 | A-005 to A-011, B-001 to B-012, C-001 and both section-end papers, searched by a separate agent (316 statements) and re-read by Claude where cited (sections 22 and 23) | Not reopened except through section 22 |
| Rama's answers | Retries (21-Oct, research step); the testing tag (21-Oct, supplement step); earlier accounts through C-001 and A-008 | His account |
| Not inspected | The testing snapshot and the records repository (Rama, 20-Oct-2026); Chartink itself (no live contact); the legacy VMs (terminated, DR-012) | - |

## 3. Terms and the eight states
The legacy code and records use some of these words loosely; this manual uses each in one sense.

| Term | Meaning here |
|---|---|
| Sender | Chartink, posting an alert's results to the URL saved on the alert |
| Webhook URL | The URL saved on a Chartink alert; not in the repository |
| Scanner key | The YAML key of a map entry; at runtime the path segment of POST /webhook/<key> (C-001 section 3) |
| Alert name | The optional scan_name field of a webhook body (C-001 section 3) |
| Receiver | WebhookReceiver's Flask app in the trading process, served by waitress on port 5000 |
| WEBHOOK /health | The receiver's own GET /health route on port 5000 |
| /health (alone) | The separate health server's route on port 8080 (B-007's terms) |
| Audited request | A request that reached _handle_webhook, whose body was read and whose webhook_audit insert succeeded (section 10.3) |
| Webhook acceptance | A per-symbol ACCEPTED: a signals row inserted as QUEUED and its tuple queued |

| State | Meaning | Where its evidence is |
|---|---|---|
| S1 scanner configured | A definition in config/scan_webhook_map.yaml | The repository (C-001 section 4) |
| S2 alert configured | A Chartink alert and the webhook URL saved on it | Chartink; Rama's accounts; for testing alerts on 21-Oct only, his screenshots (section 4) |
| S3 alert fired | Chartink ran the scan and triggered | Chartink; one record cites Chartink's trigger history, for PB-01 (section 12, F1) |
| S4 request attempted | Chartink tried to send | Nothing in the system; a refused attempt appears only as a gap |
| S5 request reached | An HTTP request accepted by waitress and routed by Flask to POST /webhook/<key> | A webhook_audit row, with the exceptions of section 10.3 |
| S6 receiver accepted | A per-symbol ACCEPTED | The response and a QUEUED signals row (section 11) |
| S7 signal persisted | The signals row and its later statuses | C-007 |
| S8 signal processed | SignalProcessor and the stages after it | C-008 |

Evidence of one state is not evidence of the next: a webhook_audit row (S5) records the answer the receiver gave, not that Chartink received it; an ACCEPTED (S6) does not show processing.

## 4. The sender side (S1 to S3)

### 4.1 What the repository holds (FACT, SOURCE-VERIFIED unless marked)
- The map: the mapping "scanners", each key a scanner name, each value holding strategy, chartink_url and, for one entry, scanner_type eod (16 entries; C-001 section 4.1). FACT (CONFIG): its header calls the key "<scanner_name_as_registered_in_chartink>"; the PB-01 comment says the key "MUST equal the /webhook/<scanner_name> URL path Rama pastes into Chartink". Evidence: config/scan_webhook_map.yaml lines 8 to 13; config/scan_webhook_map.yaml lines 77 to 84.
- The parts of a webhook URL that the system defines: the port (bind_port 5000), the path rule "/webhook/<scanner_name>" and the query parameter "token" that the authentication step reads. The address a sender uses is not in the code: bind_host names the interface to listen on. Evidence: config/system_config.yaml line 335; signals/webhook_receiver.py lines 406 to 408; signals/webhook_receiver.py lines 333 to 334.
- The token, by location and type only: the WEBHOOK_SECRET environment variable, passed to the receiver as secret_token and required at every start (C-003; A-010 section 10). Evidence: main.py lines 3407 to 3415; main.py lines 219 to 232.
- Not found in S-ALL: the webhook URL configured in Chartink. The chartink_url values in both scanner files are screener-page addresses of the form https://chartink.com/screener/<key with hyphens>, not webhook URLs (C-001 section 4.1). The text "webhook_url" occurs in 8 files (search S09): variable names in main.py and utils/startup_checks.py (the local self-check), the receiver's payload sanitiser, tests and crash-test tools, and two change-log lines of the system map about the payload redaction; none configures a sender URL.
- Not found in S-CODE or S-RT: code that creates, updates, verifies or deploys Chartink alerts. The chartink.com literal occurs in 10 files of S-CODE (search S10): the two scanner files, a dashboard reader's docstring, the load-test payload builder and test or crash-test tools; browser-automation names occur only in one GUI layout record (S11); of the nine S-RT files that match HTTP-client patterns (S18), the only one that also names chartink.com is the load-test sender, whose target is the webhook. The only Chartink-facing code is the pair of screener GET connectivity checks, the start-up check and the operator script scripts/preflight_scanner_check.py (C-001 section 6; utils/startup_checks.py lines 652 to 745).
- Sender simulators, with no scheduler entry or caller found (the cron file, the cron registry and the service unit): scripts/load_test_signals.py posts synthetic payloads to a target (default http://localhost:5000), adds ?token= when given one, and warns "Do NOT run against production VM during market hours"; scripts/test_webhook.py is a stub Flask server answering only /health on port 5000; tools under tests/crash_test/ post synthetic signals for the crash-test harness. Evidence: scripts/load_test_signals.py lines 16 to 28; scripts/load_test_signals.py lines 96 to 110; scripts/test_webhook.py lines 1 to 13.

### 4.2 The URL form and the alert names: records and Rama's accounts
- FACT (RECORD-DERIVED, 15-Jul-2026): a specification "for Rama to wire" the PB-01 alert gives the form http://<VM address>:5000/webhook/pb01_breakout_retest?token=<WEBHOOK_SECRET>, with the same shared token as the other 15 scanners; authentication by the query parameter, because require_hmac is false and Chartink cannot sign payloads; the "Chartink standard JSON" fields; and a firing time before the 16:00 self-exit. A specification handed over, not proof of what was entered. Evidence: docs/audit/followup_investigation_15jul2026.md lines 173 to 192.
- FACT (SOURCE-VERIFIED; a test's assumption, not evidence of production bodies): a unit-test helper builds "A realistic Chartink body that echoes the configured URL (incl. ?token=<secret>) in webhook_url": stocks, trigger_prices, triggered_at, scan_name, scan_url, alert_name and webhook_url of the form http://<address>:5000/webhook/<key>?token=<secret>. Evidence: tests/unit/test_webhook_receiver.py lines 1730 to 1742.
- FACT (RECORD-DERIVED, 05-Jul-2026): every stored webhook payload to that day held the token (95,431 of 95,431 rows), because Chartink echoed the configured URL, token included, in the body's webhook_url field; the redaction of the stored copy was added that day. So production bodies carried webhook_url with the token in that period. Evidence: docs/SYSTEM_MAP.md line 1366.
- FACT (RECORD-DERIVED, 05 and 06-Jul-2026): after the token was rotated on 05-Jul ("Chartink URL updated by Rama"), the first trading-day POSTs on 06-Jul were 86 answered 200 and 309 answered 403, with no 401, all from the Chartink address; the record reads this as Chartink having moved to the new token. Evidence: docs/SYSTEM_MAP.md lines 1364 to 1365.
- HISTORICAL (Rama, 21-Oct-2026; C-001 section 9.2): plain HTTP to the legacy VM's address on port 5000, the paths /webhook/range_breakout_short and /webhook/range_breakout_long, a ?token= parameter, and "every strategy name only differs"; he set up the alerts in Chartink himself (C-001 sections 6.4 and 9.2).
- HISTORICAL (Rama, A-008 Q5; A-008 section 9): production alert names had no client ID; testing alert names carried the testing account's client ID. HISTORICAL (Rama, 21-Oct-2026, his answer to Claude's question): he typed that tag at the front of the testing scan names himself ("I typed it myself"); Chartink did not add it. The receiver's handling of the tag: section 8.3.
- INTERPRETATION / AUDIT LEAD (answers Q4.2): for the 14 keys that posted (C-001 section 4.5), the alerts' requests reached this receiver with path segments equal to map keys (no 404 row at any time to 31-Jul) and with an accepted token (no 401 row to 23-Jul, R5, or on any day from 24 to 31-Jul, R7, which covers PB-01's posts); so, on the recorded days, each of those alerts pointed at this receiver with the right port, path and token. This says nothing of the URL text itself, of the two range-breakout keys (0 POSTs) or of dates outside the records. Evidence: docs/audit/integrity_audit_2026.md lines 126 to 135; docs/audit/integrity_audit_2026.md lines 101 to 103; docs/audit/refused_posts_403_investigation_23jul2026.md lines 113 to 122.

### 4.3 What Chartink displayed on 21-Oct-2026 (CURRENT EXTERNAL FACT; Rama's screenshots)
Three screenshots, not stored (DR-008): two of Chartink (the dashboard; one testing alert's dialog) and one of Rama's chat with ChatGPT, whose V2 input is in C-002_v2_design_inputs.md section 1. None of the values they show (the legacy VM's address, the token, account identifiers) is reproduced. The two of Chartink show what it displayed that day for testing scans; they do not establish production's alerts, the settings in force from 12-Jun to 04-Aug, whether an alert is enabled now, or that the displayed payload equals what was sent.
- The dashboard: scans named "<testing account tag>-<strategy name in capitals>" (among them open low breakout long, open high breakdown short, first pullback long and short, VWAP bounce long, VWAP rejection short, gap go long, and PB01 breakout + retest "(shadow)"), marked private and with alerts, shown as created on 9/8/2026. Another entry, a scan marked personal, shows 9/17/2026, so the order is month/day: 08-Sep-2026 and 17-Sep-2026 (INTERPRETATION of the date format).
- One alert's settings, for the open low breakout long scan: frequency 1 minute; minimum matches 1; After trigger "Continue"; Duplicates "Duplicate"; delivery Desktop/Mobile (web); a webhook URL of the form http://<VM address>:5000/webhook/open_low_breakout_long?token=<WEBHOOK_SECRET>, that is plain HTTP, port 5000, the bare strategy key as the path segment and the token as a query parameter; and a "Test webhook" control, whose use is not evidenced.
- The payload that the dialog displays: stocks, trigger_prices, triggered_at (a sample value of the form "7:53 pm"), scan_name ("<testing account tag>-OPEN LOW BREAKOUT LONG"), scan_url (the same name as a lower-case hyphenated slug), alert_name, and webhook_url, which repeats the full token-bearing URL. In that order webhook_url is the last field.
- Security: the alert's dialog shows that a token-bearing legacy webhook URL was still saved on at least one testing alert on 21-Oct. C-001's action to remove or disable the legacy webhook URLs in the Chartink alerts (C-001_review_notes.md section 2) was pending at revision 0; Rama closed it as not needed on 22-Oct-2026 (next bullet; C-002_review_notes.md section 2). It is not recorded as done.
- Present-day status (HISTORICAL, Rama, 22-Oct-2026, quoted; not evidence of the live period): "FYI Both VM's (Production and testing) has been terminated and deleted, Chartink subscription was already expired, So Current chartink scanners and it's URL treat as just a placeholder". By his statement the scanners and the URLs saved on them are to be treated as placeholders only. It does not establish whether or when any alert posted in the live period, when the subscription expired, or what Chartink does with a saved alert now. His decision on the clean-up (22-Oct): "Close: not needed (Recommended)".

### 4.4 Chartink's public pages (CURRENT EXTERNAL FACT; read 2026-10-21 through a summarising fetch; paraphrased; not the legacy period)
- "Webhook support for Alerts" (dated 16-Mar-2020, updated 02-Jun-2021): an HTTP POST is made "to the URL you have specified along with the data corresponding to the alert"; the sample body has the fields stocks, trigger_prices, triggered_at, scan_name, scan_url, alert_name and webhook_url; the article states no timeout, retry, expected response code or authentication. Support replies under it, as the fetch returned them: the request header is set to "content-type: json" (22-May-2020); one webhook is allowed per scan and alert (28-May-2024); "Our webhooks start from 9:16am" (26-Feb-2024).
- "Alerts Guide" (posted 02-Feb-2018; page dated 17-Jun-2026): frequencies of 1, 2, 3, 5, 10, 15, 20, 30, 60, 120 or 240 minutes, or at Market Open or Market Close; After trigger "Continue" (the alert keeps monitoring and triggers whenever the conditions are met again) or "Pause" (it stops after the first successful trigger); Duplicates "Unique" (one alert per stock for the day) or "Duplicate" (repeated notifications for the same stock whenever it matches again that day); a minimum number of matching stocks; delivery by email, SMS, desktop/mobile (web) or WhatsApp; a webhook makes an HTTP POST to the URL given, and a "Payload Columns" setting chooses the data fields included; no retry or timeout behaviour is described. ChatGPT's reconciliation review (21-Oct) checked the same page independently and reports that it describes alert names as user-entered.

### 4.5 What the evidence reaches
- S1: the map. S2: no configuration in the repository; only records (the 15-Jul specification, the 05-Jul rotation note), Rama's accounts and, for testing alerts on 21-Oct only, the screenshots. S3: one record names Chartink's trigger history, for PB-01 (section 12, F1). S4: nothing in the system (section 3).
- So the repository does not establish the URL saved on any production alert, the alerts' settings in the live period, or that any alert fired (U-1).

## 5. The network path

### 5.1 Port and interface
FACT (CONFIG): port 5000; bind_host is an address the filter masks, and its comment (dated 2026-04-27) says the host binds all interfaces so that the VM can accept Chartink webhooks routed through ngrok or from outside; B-001 section 3 records the listener as bound to all interfaces. Evidence: config/system_config.yaml lines 327 to 335. KNOWN DEFECT / AMBIGUITY (K-6): the same comment says the all-interfaces bind relies on require_hmac being true plus firewall or security-group restrictions, while require_hmac is false on the next line ("Chartink cannot sign payloads; use ?token= auth"). Evidence: config/system_config.yaml lines 331 to 336.

### 5.2 Firewall
- FACT (SOURCE-VERIFIED): the installer inserts an iptables rule accepting tcp/5000 from any source at INPUT position 5, before the image's REJECT, and saves it with netfilter-persistent. Evidence: deploy/install_vm_services.sh lines 33 to 47.
- FACT (RECORD-DERIVED, 05-Jul-2026; the paths record's firewall section is headed "verified 05-Jul-2026"): the tcp/5000 accept was rule #6, before a catch-all REJECT (#7); that day Rama applied a single-address allowlist restricting port 5000 to one /32 source, the sole Chartink address, at runtime and persisted; on 06-Jul the record reads that day's Chartink rows as proof that the allowlist admitted Chartink. Evidence: PATHS.md lines 262 to 263; docs/SYSTEM_MAP.md lines 1364 to 1365.
- The repository's installer was not changed to match; the rule's later state is NOT ESTABLISHED (the integrity audit's security phase, measured on 01-Aug, records the firewall rules as unverified in that session; docs/audit/integrity_audit_2026.md lines 4351 to 4355). A-010 section 10 dates the single-source rule "by 27-Jul"; the records above give 05-Jul for applying it (a narrowing, recorded here; no erratum, since "by 27-Jul" stays true). B-001 sections 3 and 14 record the installer's any-source rule, not the 05-Jul single-address rule; B-001 section 3 also cites the 05-Jul audit's "C-2 Option-B (reverse-proxy/HMAC/IP-allowlist) NOT deployed", the day of the allowlist entry, and how the two relate (a bundle not deployed, or a different time of day) is NOT ESTABLISHED.
- The cloud provider's security list is outside the repository: UNKNOWN (U-4).

### 5.3 TLS
FACT (SOURCE-VERIFIED): no TLS in the serving path; waitress serves plain HTTP; no certificate or TLS setting was found in S-CODE (search S19). FACT (RECORD-DERIVED): on 02-Jul the token travelled "over plaintext HTTP"; on 06-Jul TLS was "deferred". Evidence: docs/audit/system_security_audit_02jul2026.md line 19; docs/SYSTEM_MAP.md line 1364.

### 5.4 Reverse proxy and tunnel
- FACT (SOURCE-VERIFIED): the receiver takes request.remote_addr as the source and reads no X-Forwarded-For; waitress is started without trusted_proxy. Evidence: signals/webhook_receiver.py line 479; main.py lines 3988 to 3996. CURRENT EXTERNAL FACT (waitress 3.0.2 source): trusted_proxy defaults to None, clear_untrusted_proxy_headers to True and url_scheme to "http".
- FACT (RECORD-DERIVED): on 04-Jun a crash-test note describes the receiver as direct internet-facing with no nginx or reverse proxy; on 02-Jul the dashboard backend investigation found no reverse-proxy or TLS configuration in the repository, only a recommendation; on 03-Jul the source-address investigation records that /etc/nginx does not exist on the VM, that no reverse proxy sits in front of the receiver and that request.remote_addr is the raw TCP peer; on 31-Jul the question is carried as open ("5-Jul left it UNCERTAIN"; K-11). Evidence: docs/crash_test_info_04jun2026.md line 323; docs/gui_project/G0_BACKEND_INVESTIGATION_REPORT.md lines 38 to 39; docs/audit/c2_chartink_ip_investigation_03jul2026.md line 13; docs/audit/integrity_audit_2026.md lines 392 to 394.
- Not found in S-CODE: a reverse-proxy, tunnel, TLS or certificate configuration (searches S04 and S19). The nginx and ngrok comments and runbook steps are drift (K-5).

### 5.5 DNS
INTERPRETATION: the recorded URL forms use the VM's address, not a host name (sections 4.2 and 4.3), so no name lookup was involved on this path; not established for every alert.

### 5.6 Rate limit at the edge
FACT (SOURCE-VERIFIED): a token bucket per source address, burst 60, refill 5 per second, 429 when empty; idle buckets are evicted after 60 seconds once 8,192 addresses are held. FACT (CONFIG): per_ip_rate_limit_enabled true, per_ip_burst 60, per_ip_refill_per_sec 5.0. Evidence: signals/webhook_receiver.py lines 69 to 108; signals/webhook_receiver.py lines 240 to 247; config/system_config.yaml lines 327 to 342. The comment calls Chartink's open-bell burst "~40 signals from one IP" (a claim; signals/webhook_receiver.py lines 69 to 77). FACT (RECORD-DERIVED): no 429 answer to 23-Jul (section 14, R5).

## 6. The HTTP server and when it listened (FACT, SOURCE-VERIFIED unless marked)

### 6.1 Construction and start
- main builds the queue (maxsize = capacity, 300) and the receiver (the queue, the store, AppConfig, the market windows, the kill switch, the logger "webhook_receiver", and secret_token from the environment). Evidence: main.py lines 3402 to 3415.
- The app: WebhookReceiver.__init__ creates a Flask app, sets TESTING False and MAX_CONTENT_LENGTH to 1 MiB ("FIX-077: 1MB hard limit prevents OOM") and registers the routes. Evidence: signals/webhook_receiver.py lines 249 to 252.
- After the processor, the entry gate and the target workers have started, main runs waitress.serve(app, host=webhook.bind_host, port=webhook.bind_port, threads=8, connection_limit=100) in the daemon thread "webhook-server"; the comment says waitress replaced Flask's development server because "Werkzeug's app.run drops connections under burst load". No prefix, mount point or URL joining: waitress is given the Flask app itself. Evidence: main.py lines 3972 to 4000; main.py lines 3987 to 3996.
- Other wiring: the processor is given the receiver's release_in_flight; the EOD capture worker is bound late with set_eod_capture when the watchlist is enabled (C-001 section 7.5). Evidence: main.py line 3612; main.py line 3747.

### 6.2 Server limits
- CURRENT EXTERNAL FACT (waitress 3.0.2 source; defaults, not set in the code): channel_timeout 120 seconds of inactivity, backlog 1024, max_request_header_size 262,144 bytes, max_request_body_size 1,073,741,824 bytes; connection_limit 100 is also the default.
- The receiver sets no request timeout.

### 6.3 The self-check
- Two seconds after the thread starts, main GETs the WEBHOOK /health on a literal address (masked by the filter) at the bind port; check_webhook_endpoint's docstring calls it the local webhook endpoint, and the check counts 200 to 299 or 401 as reachable. If it is not reachable, main logs CRITICAL, writes a sentinel for the alert watcher and keeps running without entries ("DEGRADE, do not block the boot", 27-Jul). The start-up checks run earlier with webhook_url=None ("Flask not started yet"). Evidence: main.py lines 4002 to 4054; utils/startup_checks.py lines 783 to 826; main.py lines 2451 to 2461.
- INTERPRETATION / AUDIT LEAD: the check accepts 200 or 401 from whatever answers GET /health on that port (utils/startup_checks.py lines 797 to 809), so it shows that something answered, not that this process's receiver did; it cannot show reachability from outside the VM.

### 6.4 Shutdown
The handler calls webhook_receiver.stop() before signal_processor.stop(), so new webhook requests get 503 while the server thread lives on until the process exits; stop() sets the flag and stops the in-flight sweeper. Evidence: main.py lines 1681 to 1692; signals/webhook_receiver.py lines 1166 to 1176. The shutdown sequence belongs to B-011 (section 6).

### 6.5 When the port was open (frozen manuals, cited; INTERPRETATION of the cited order and records)
- The daily start: a cron job at 08:15 writes the token file and the token watcher starts the unit (B-001 section 6); the start-window guard refuses starts outside [08:00, 18:15) (A-009 section 5). The webhook server is step 15 of the start, after the startup reconcile and the processor (A-011 section 5; B-005 row S18: "Signals can arrive only from here"); the whole start takes about 11 seconds (B-006 section 4; B-003 section 7).
- The day's end: the self-exit gate from service_window_end, 17:35 since 25-Jul and 16:00 before (A-011 section 10; B-011 section 3.1; B-001 section 7.2; FACT (CONFIG): config/system_config.yaml lines 105 to 112); before 25-Aug a carried delivery position could hold the service open past it (A-011 section 10).
- So the receiver listened from step 15 of a start until the process exited; at any other time a POST met a closed port (section 12, F1 and F2).

### 6.6 The two HTTP servers
The trading process ran two waitress servers, and only one carried the webhook:
- the receiver's app on port 5000 (daemon thread webhook-server, 8 threads), with its own WEBHOOK /health route (section 7.1);
- the health server on port 8080: scripts/healthcheck_server.py runs waitress in the daemon thread healthcheck-server with 1 thread, its host defaulting to the loopback address (deployed 08-Jul per B-007 section 6), started by main after the webhook server. Evidence: main.py lines 4073 to 4082; scripts/healthcheck_server.py lines 297 to 326.
- Neither is stopped by the shutdown handler; each ends at process exit (B-006 rows TH24 and TH25; in main.py the health server appears only at its import and at that call). B-011 section 6 describes the two as one server (erratum B-011 E-1, accepted, section 22).
- B-007's terms are used here: "WEBHOOK /health" is the receiver's route on port 5000; "/health" alone means the port-8080 route. A-011 section 14, B-001 section 13 and C-001 sections 6.5 and 6.6 write "/health" for the receiver's route (a wording difference only; no erratum proposed).

## 7. Routes and the framework's own answers

### 7.1 The routes (FACT, SOURCE-VERIFIED)
_register_routes defines: GET /health, the WEBHOOK /health (the per-address limit, then authentication, then a status body with the kill-switch state and the queue depth); POST /webhook/<scanner_name>, which calls _handle_webhook; and a 500 error handler that logs CRITICAL and answers {"error": "Internal server error"}. No blueprint, URL prefix, before-request or after-request hook exists in the file. Evidence: signals/webhook_receiver.py lines 360 to 413.

### 7.2 Method, path and identity (FACT, SOURCE-VERIFIED)
POST only, on the rule "/webhook/<scanner_name>" with Flask's default converter (one path segment, no "/"). The segment is used as given: an exact, case-sensitive membership test against the map keys (C-001 section 5.2). Each scanner is addressed by its own path value; there is no common path, and the segment is the only identity the receiver takes at the edge; the body's scan_name is an optional cross-check (section 8.3). Evidence: signals/webhook_receiver.py lines 406 to 408; signals/webhook_receiver.py lines 551 to 554; signals/webhook_receiver.py lines 617 to 633.

### 7.3 The framework's answers (CURRENT EXTERNAL FACT; local probe; the probed versions only, not the VM's)
With the same rule and methods: GET answers 405 (Allow: POST, OPTIONS); OPTIONS answers 200 by itself; "/webhook/", "/webhook/a/b", "/Webhook/<key>" and a key with an encoded "/" answer 404; a trailing slash ("/webhook/<key>/") answers 404; a doubled slash ("/webhook//<key>") answers 308 with the merged path as Location. None of these answers enters the view, so none is audited (section 10.3). A query string does not affect routing: a valid path with one enters the view. Whether Chartink follows a 308: UNKNOWN.

### 7.4 The query string (FACT, SOURCE-VERIFIED)
The only query parameter read is "token", inside _authenticate (C-003). Evidence: signals/webhook_receiver.py lines 333 to 334.

## 8. What the receiver read from a request (FACT, SOURCE-VERIFIED unless marked)

### 8.1 Fields and headers
The method and path (routing); the path segment; request.remote_addr (the limiter and the audit row); the raw body (request.get_data(), read before the try block; its length is audited); the query parameter token and the header X-Webhook-Signature, both only in _authenticate (C-003). It reads no Content-Type, User-Agent or other header itself. Evidence: signals/webhook_receiver.py lines 477 to 481; signals/webhook_receiver.py lines 333 to 334.

### 8.2 The body
- Parsed with json.loads from the raw bytes, whatever the Content-Type; it must be a JSON object; stocks, trigger_prices and triggered_at are required and scan_name is optional; scan_url, alert_name and webhook_url are not read, and webhook_url is blanked in the stored copy. Evidence: signals/webhook_receiver.py lines 585 to 615; signals/webhook_receiver.py lines 857 to 886. Field rules belong to C-004.
- CURRENT EXTERNAL FACT (local probe; the probed versions only): a content type of "json", or none, does not change the bytes the view receives (Chartink's support reply says it sends "content-type: json", section 4.4).
- Size: MAX_CONTENT_LENGTH is 1 MiB (signals/webhook_receiver.py line 251). CURRENT EXTERNAL FACT (local probe; the probed versions only): a body of 1 MiB plus one byte answers 413 when request.get_data() is called, after the view is entered; a body of exactly 1 MiB is read.

### 8.3 The alert name and the account tag
- When scan_name is present it is normalised (case and spaces), a leading "<primary account tag>-" or "<tag>_" is removed (WR4b, 09-Sep-2026; the tag is read from the accounts file, which is on the never-open list and was not opened), and what remains must equal the path segment, else 400; an absent scan_name skips the check. Evidence: signals/webhook_receiver.py lines 111 to 138; signals/webhook_receiver.py lines 617 to 633. Its limits are C-001's D-07 (C-001 section 7.6).
- KNOWN DOC DRIFT (K-13, an unsupported rationale): the function's docstring explains the strip with "Chartink alert names are per-account on a shared Chartink login" and "The account name cannot be dropped in the Chartink UI, so the receiver absorbs it." The first sentence is consistent with Rama's answer of 21-Oct (section 4.2): he typed the tag on the testing scans shown in his screenshot himself. Nothing supports the second: a tag he typed is not one Chartink adds, and whether Chartink lets a scan be renamed is U-9. The code's behaviour is unaffected. Evidence: signals/webhook_receiver.py lines 111 to 128.
- FACT (SOURCE-VERIFIED, value-blind): no client-ID shape occurs in config/scan_webhook_map.yaml or in any of the 16 config/strategies/*.yaml files (masked-reader counts 0 and 0), so no account tag was part of a map key or a strategy name.
- Whether Chartink lets an existing scan or alert be renamed, and whether it requires distinct scan names within one login: UNKNOWN (U-9).

### 8.4 The time field
- triggered_at is parsed as "%Y-%m-%d %H:%M:%S", then "%I:%M %p", then "%H:%M" (the comment reads "Chartink sends "HH:MM am/pm""); a time-only value takes today's date, and a naive value is taken as IST. Evidence: signals/webhook_receiver.py lines 635 to 657.
- CURRENT EXTERNAL FACT (local check, Python 3.13.16): the screenshot's "7:53 pm" form parses under "%I:%M %p" as 19:53. Field rules belong to C-004.

## 9. The order of the gates and the response contract (FACT, SOURCE-VERIFIED unless marked)
In the order the code runs them. "Audit row" gives (response_code, accepted, rejected); bodies are JSON (jsonify) unless marked HTML.

| # | Trigger | Status and body | Audit row | Evidence |
|---|---|---|---|---|
| 1 | A path or method outside the two rules | 404, 405 or 308 from the framework, HTML; OPTIONS answered 200 by the framework | None | CURRENT EXTERNAL FACT (local probe; the probed versions only), section 7.3 |
| 2 | A body over 1 MiB | 413, raised when _handle_webhook reads the body (signals/webhook_receiver.py line 480), HTML | None: the read runs before every audit write | Probe (section 8.2); the integrity audit's IA-P1-04 (section 10.3); signals/webhook_receiver.py lines 477 to 481 |
| 3 | Shutting down | 503 {"error": "Service shutting down; retry later"} | (503, 0, 0) | signals/webhook_receiver.py lines 483 to 490 |
| 4 | Over the per-address limit | 429 {"error": "Rate limit exceeded; slow down"}; a WARNING with the address | (429, 0, 0) | signals/webhook_receiver.py lines 492 to 504 |
| 5 | Authentication fails | 401 {"error": <one of four reasons>} (C-003) | (401, 0, 0) | signals/webhook_receiver.py lines 547 to 549 |
| 6 | Unknown key | 404 {"error": "Unknown scanner: '<key>'"} | (404, 0, 0) | signals/webhook_receiver.py lines 551 to 554 |
| 7 | An eod key | 400 for malformed JSON, a body that is not a JSON object, a missing field, a bad triggered_at or non-string stocks; 200 {"accepted": 0, "captured": 0, "detail": "watchlist disabled"} without a worker; else 200 {"accepted": <symbols>, "captured": <submitted>} | A 200 records accepted = the symbol count with a worker, and 0 when the watchlist is disabled (C-001 A-L5) | signals/webhook_receiver.py lines 751 to 804 |
| 8 | Kill switch active | 403 {"error": "Kill switch active; signals rejected"} | (403, 0, 0) | signals/webhook_receiver.py lines 566 to 568 |
| 9 | Queue at or above the threshold (240 of 300) | 503 {"error": "Signal queue at capacity; retry later"}; header X-Queue-Depth | (503, 0, 0) | signals/webhook_receiver.py lines 570 to 578 |
| 10 | Outside the entry window (entry_start 10:00 and entry_end 15:00; the window rule itself is B-008's) | 403 {"error": "Outside entry window"} | (403, 0, 0) | signals/webhook_receiver.py lines 580 to 583; config/system_config.yaml lines 28 and 47 |
| 11 | Malformed JSON, not an object, a failed cast, a missing field, an alert-name mismatch, a bad triggered_at, non-string or unequal lists | 400 {"error": ...}; a WARNING with the first 1,000 bytes of the raw body | (400, 0, 0) | signals/webhook_receiver.py lines 585 to 674; signals/webhook_receiver.py lines 518 to 523 |
| 12 | The batch | 200, or 503 if any symbol is QUEUE_FULL or STORE_ERROR; body {"accepted", "rejected", "results": [{"symbol", "status"[, "signal_id"]}]}; header X-Queue-Depth and, from 60% fill (180 queued), X-Queue-Warning: high | (200, accepted, rejected); a 503 batch records (503, 0, 0) (C-001 A-L4) | signals/webhook_receiver.py lines 676 to 745; signals/webhook_receiver.py lines 510 to 517 |
| 13 | An exception inside _process_request | 500 {"error": "Internal server error"}; CRITICAL | (500, 0, 0) | signals/webhook_receiver.py lines 525 to 528 |

- Per-symbol statuses the code returns: ACCEPTED, DUPLICATE, EXPIRED, INVALID_SYMBOL, INVALID_PRICE, IN_PROCESS, QUEUE_FULL, STORE_ERROR and REJECTED_EXCLUDED_SYMBOL (owners C-004, C-005 and C-009). Evidence: signals/webhook_receiver.py lines 892 to 1072; signals/webhook_receiver.py lines 708 to 717.
- The audit row is written in the finally clause, before the response object goes back to the framework; a row with 200 therefore records what the receiver answered, not that the sender received it. Evidence: signals/webhook_receiver.py lines 529 to 534.
- The EOD branch (row 7) comes after authentication and the key check and before the kill, backpressure and window gates, so on the code path an eod-key POST under a kill still goes to the EOD route: 400 for a malformed body; otherwise 200, its symbols handed to the capture worker, or a fail-safe miss when no worker is bound (section 11; C-001 section 7.5). A-011 section 10 and B-001 section 13 summarise kill days as the webhook refusing "every signal" (errata A-011 E-8 and B-001 E-4, accepted, section 22).
- The 503 answers (rows 3, 9 and 12) are the code's invitation to a retry (its comments; signals/webhook_receiver.py lines 59 to 66); whether Chartink retried is UNKNOWN (section 13.1).
- KNOWN DOC DRIFT (K-2): the module docstring's response list (WR5) omits 429, and its status list (WR10) names OUTSIDE_HOURS, which the code never returns: the window gate answers the whole request with 403. Evidence: signals/webhook_receiver.py lines 16 and 21 to 22; signals/webhook_receiver.py lines 580 to 583.

## 10. The request audit and the logs (FACT, SOURCE-VERIFIED unless marked)

### 10.1 The webhook_audit table
Columns ts, scanner_name, source_ip, payload_size_bytes, response_code, signals_accepted, signals_rejected, duration_ms and a generated date column, with indexes on ts, date and scanner_name. _write_audit fills it: ts at the time of writing, scanner_name from the path (an unknown key included), source_ip = the TCP peer, the raw body's length, the response code, the counts and the duration. Evidence: core/schema.sql lines 575 to 602; signals/webhook_receiver.py lines 1126 to 1154.

### 10.2 What the counts record
The accepted and rejected counts are taken only from a 200 answer's JSON; every other answer records 0 and 0. So a request answered with anything but 200 (400, 401, 403, 404, 429, 500 or 503) is recorded by its response code alone, and a 503 batch records 0 accepted although other symbols in it were queued; only per-symbol refusals inside a 200 are counted (as rejected), and those inside a 503 batch by neither count. An EOD 200 records accepted = the symbol count when a capture worker is bound (0 when the watchlist is disabled), although no signals row is made. Evidence: signals/webhook_receiver.py lines 510 to 517; C-001 section 8 (A-L4, A-L5; D-06). A-011 section 12 says the kill, window and capacity refusals are recorded by the row "with accepted and rejected counts" (erratum A-011 E-8, accepted, section 22).

### 10.3 Coverage: the bounded rule
- One webhook_audit row for each request that reaches _handle_webhook and whose body is read, unless the insert fails (FACT, SOURCE-VERIFIED with the probe).
- No row for (stable IDs, cited by the errata of section 22):
  - NR-1. The framework's own answers: a wrong method, path or slash form (section 7.3).
  - NR-2. A body over 1 MiB, answered 413 when request.get_data() reads it, which runs before every audit write. Evidence: signals/webhook_receiver.py lines 477 to 481. FACT (RECORD-DERIVED, Phase 1 of the integrity audit, 31-Jul): the audit recorded this hole as IA-P1-04 and reports its incidence as zero, on its stated basis that no undesigned 4xx had been recorded and Chartink's bodies are kilobyte-scale (a request in the hole leaves no row, so the table itself cannot show it); docs/audit/integrity_audit_2026.md lines 212 to 230.
  - NR-3. Requests that never reach Flask. CURRENT EXTERNAL FACT (waitress 3.0.2 source): a channel hands a request to the application only once the request has been received in full (channel.py, received()), and connection_limit stops waitress accepting new connections without refusing them, while the waiting ones queue in the operating system's listen backlog, for which waitress asks 1,024 (adjustments.py, server.py). INTERPRETATION / AUDIT LEAD from the library, not exercised or recorded: a request cut off mid-body, malformed HTTP or oversized headers are answered or dropped by waitress and leave no row; connections over the limit wait and are served, and audited, later, and only a backlog overflow or a sender's timeout would leave no row.
  - NR-4. A failed insert, which logs an error and leaves the response unchanged. Evidence: signals/webhook_receiver.py lines 1137 to 1154.
- WEBHOOK /health requests are never audited (a test's name records this: tests/unit/test_c6_auth_collapse.py line 382).
- KNOWN DOC DRIFT (K-1): the docstring's WR13 "webhook_audit row per POST regardless of outcome", the schema comment "Written regardless of outcome" and July records say every POST. Evidence: signals/webhook_receiver.py line 25; core/schema.sql lines 576 to 577. The frozen statements that repeat the wider claim are C-001 sections 3, 6.6, 7.1 and 9.2, A-010 section 10, B-008 section 3 and B-009 section 5 (errata C-001 E-1, A-010 E-4, B-008 E-1 and B-009 E-2, accepted, section 22). C-001's reading of R-01 is unaffected: Chartink's bodies are kilobyte-scale and the audit reports the hole's incidence as zero (NR-2).

### 10.4 The time column (KNOWN DOC DRIFT, minor, K-7)
The schema comment says ts is "when request arrived"; the code takes the time the row is written, after handling, so arrival is about ts minus duration_ms (INTERPRETATION). Evidence: core/schema.sql line 583; signals/webhook_receiver.py line 1136.

### 10.5 The logs
The logger "webhook_receiver" (INFO and above to the daily system log, DEBUG and above to the daily debug log, WARNING and above to stdout; files logs/<stem>_<date>.log) records: a 429 with the source address; a 400 with the first 1,000 bytes of the raw body; malformed JSON with the first 500 bytes; an alert-name mismatch; a bad triggered_at; a missing field; EOD information and warnings; CRITICAL for an unhandled exception, a store failure and a stalled in-flight claim. Evidence: signals/webhook_receiver.py lines 495 to 504; signals/webhook_receiver.py lines 518 to 528; core/logger.py lines 354 to 421. The logs no longer exist (C-001).

### 10.6 The token in the logs (INTERPRETATION / AUDIT LEAD; for C-003)
The 400 and malformed-JSON warnings log the raw, unsanitised body (signals/webhook_receiver.py lines 518 to 523; signals/webhook_receiver.py lines 585 to 593), and Chartink's bodies carried webhook_url with the token (section 4.2). So such a line held the token whenever webhook_url fell within the bytes logged (1,000 or 500). It is the last field of the payload that Chartink displayed for one testing alert on 21-Oct (section 4.3, as transcribed) and of the public article's sample as the summarising fetch listed it (section 4.4), so a body with a long stock list may have left it out; the field order of the bodies actually delivered is not established. FACT (RECORD-DERIVED): no 400 answer is recorded from 12-Jun to 23-Jul 15:29 (R5) or on any day from 24 to 31-Jul (R7) (section 14). The testing VM answered 400 to every signal before WR4b (A-008 section 9); its logs and snapshot were not inspected.

### 10.7 Other evidence, by state
The signals rows (S6, S7); the cron_heartbeat row per EOD alert (S5 and S6 for the EOD route); the shutdown census's day count, taken from webhook_audit (main.py lines 1662 to 1675); the WEBHOOK /health for an authenticated caller (queue depth and kill state, not arrivals); the dashboard screens built on webhook_audit (C-001 section 8).

## 11. The hand-off after an accepted request (FACT, SOURCE-VERIFIED; boundary only)
- Intraday key, per symbol: the alias and the exclusion list; then the checks of _process_signal (symbol, price, expiry, the in-flight claim, a cached duplicate, the fingerprint); then INSERT into signals (status QUEUED; scanner and strategy both set to the key; the fingerprint; the stored payload sanitised); then queue.put_nowait((signal_id, key, symbol, price, triggered_at)) onto the queue built with maxsize = capacity (300). The symbol's in-flight claim is left for the processor to release. Owners: C-004 (symbol and price), C-005 (in-flight and deduplication), C-006 (queue), C-007 (row), C-008 (processing), C-009 (expiry), C-010 (mapping). Evidence: signals/webhook_receiver.py lines 956 to 1072; main.py lines 3402 to 3404; main.py line 3612.
- A full queue: the row is marked QUEUE_FULL, both claims are released and the batch answers 503; a later POST with the same fingerprint re-queues that row (the M-S2 path). A store failure: STORE_ERROR, both claims released, the batch answers 503. Evidence: signals/webhook_receiver.py lines 981 to 1015; signals/webhook_receiver.py lines 1046 to 1068. FACT (RECORD-DERIVED, 31-Jul): 18 answers of 503 at any time, none since 01-Jul, from which the audit concludes that the rollback and re-queue code of the 17-Jul era never ran in production (docs/audit/integrity_audit_2026.md lines 298 to 302).
- An eod key: eod_capture.submit(scanner_name, symbol, triggered_at) per symbol and one cron_heartbeat row ("pb01_capture"; it records "queued", not "captured"), or a fail-safe miss when no worker is bound. Evidence: signals/webhook_receiver.py lines 776 to 834. The route and the worker belong to C-011 (C-001 section 7.5).
- The webhook_audit row is C-002's own record (section 10).

## 12. Failure paths
Form: TRIGGER -> BRANCH -> RESPONSE OR STATE -> RECORD -> CONSEQUENCE -> LIMITS.
- F1. The process is not running (before the daily start, after the self-exit, during a halt or a restart) -> no listener -> the connection is refused (or times out if dropped upstream) -> nothing in the system -> the alert is lost unless the sender tries again later -> the sender's behaviour is UNKNOWN (section 13.1).
  - FACT (RECORD-DERIVED): on 19-Jun the service was down from 10:07:54 to 13:24:33 and "POSTs during it get connection-refused, no audit row"; all ten of PB-01's 17:00 alerts from 13-Jul met a closed port because the service had exited at 16:00, while their candidates "exist in Chartink's trigger history"; the self-exit moved from 16:00 to 17:35 on 25-Jul for that reason; the 15-Jul specification gives the process's hours as 08:15 to 16:00 IST only. Evidence: docs/audit/refused_posts_403_investigation_23jul2026.md lines 152 to 156; docs/audit/service_window_extension_2026-07-24.md line 55; config/system_config.yaml lines 105 to 112; docs/audit/followup_investigation_15jul2026.md lines 188 to 191.
  - INTERPRETATION / AUDIT LEAD: the first PB-01 row in webhook_audit is from 27-Jul (C-001 R-02), although the service was up on the trading days in between (intraday POSTs were recorded on them, section 14), so none of the ten refused alerts was delivered later while the port was open; whether Chartink tried again while it was closed is unknown.
- F2. The port opens late in the start: waitress starts after the workers (main.py line 4000); a request before that is refused as in F1. The start's timing: section 6.5.
- F3. The server thread dies or cannot bind -> no listener -> the self-check two seconds later fails -> CRITICAL log and an out-of-band sentinel; the process keeps running without entries (since 27-Jul) -> F1 for the sender. Evidence: main.py lines 4002 to 4054. The check's limits: section 6.3.
- F4. Shutting down -> 503 and an audit row (section 9, row 3); the shutdown sequence belongs to B-011.
- F5. Non-success answers per branch: section 9. How Chartink treated 4xx and 5xx answers: UNKNOWN. The code's comments expect a retry after a 503 and none after a 200 (signals/webhook_receiver.py lines 59 to 66; signals/webhook_receiver.py lines 730 to 734).
- F6. Unknown key: 404 with a row when authenticated; without valid authentication the answer is 401, because authentication runs first (signals/webhook_receiver.py lines 539 to 554). FACT (RECORD-DERIVED): no 404 row at any time to 31-Jul (docs/audit/integrity_audit_2026.md line 74).
- F7. Wrong path form (a trailing slash, an extra segment, another letter case) -> the framework's 404 -> no row (section 7.3). INTERPRETATION / AUDIT LEAD: an alert misconfigured in that way would leave no trace in webhook_audit, as C-001 section 9.2 noted for other paths, ports or hosts.
- F8. A body over 1 MiB -> 413 at the body read -> no row (section 9, row 2). A request cut off mid-body never reaches Flask (F10).
- F9. The audit insert fails -> an error log line, no row, the response unchanged (signals/webhook_receiver.py lines 1137 to 1154).
- F10. Requests waitress answers or drops before Flask (a request cut off mid-body, malformed HTTP, oversized headers) -> no row; connections over the connection limit wait in the listen backlog and are served later (section 10.3, NR-3). INTERPRETATION from the library; not exercised or recorded.

## 13. Retries, repeats, lateness and concurrency

### 13.1 Sender retries (Chartink): UNKNOWN / NOT ESTABLISHED
- FACT (RECORD-DERIVED): the assumptions record lists "Chartink retries on 503 (external behavior)" as NOT_TESTED_YET (A14). Evidence: docs/SYSTEM_ASSUMPTIONS.md line 20.
- CURRENT EXTERNAL FACT: neither of Chartink's pages says anything about retries or timeouts (section 4.4).
- HISTORICAL (Rama, 21-Oct-2026, his answer to Claude's one research question): "Don't know / never checked" whether Chartink ever showed a failed webhook delivery or a re-delivery.
- Claims, not evidence (K-12): code comments and June and July records assume a retry after a 503 and none after a 200. Evidence: signals/webhook_receiver.py lines 59 to 66; core/exceptions.py line 144; docs/audit/p3s14_done_17jul2026.md line 261; docs/audit/silent_failure_gaps_2026-07-25.md line 143; docs/audit/integrity_audit_2026.md line 311; docs/crash_test_info_04jun2026.md lines 238 and 992.
- INTERPRETATION / AUDIT LEAD: the ten refused PB-01 alerts were not delivered later (section 12, F1).
- A-011 section 12 states the retry as fact ("so that Chartink retries"; erratum A-011 E-8, accepted, section 22). B-008 sections 3 and 9 already record whether Chartink retried a 403 or other refused alerts as NOT ESTABLISHED; C-001's U-07 stays UNKNOWN.

### 13.2 The receiver
It sends nothing outward and retries nothing. It is built to make a sender's retry useful: QUEUE_FULL and STORE_ERROR release both claims and make the batch answer 503, and a later POST with the same fingerprint re-queues the QUEUE_FULL row (section 11). Evidence: signals/webhook_receiver.py lines 59 to 66; signals/webhook_receiver.py lines 981 to 1068.

### 13.3 Broker, scheduler and scripts
The broker is not on this path. No cron or registry entry sends or re-sends webhooks (N6). The load-test sender has no retry; the operator scanner script retries its screener GETs (C-001 section 6.2), which are not webhook deliveries.

### 13.4 Repeats by the alerts' design
FACT (RECORD-DERIVED, 19-Jul): the census records that Chartink posted each scanner roughly once a minute (370 POSTs between 09:20 and 15:29 on 09-Jul), so a symbol that stays in a scan arrives again every minute; it reads about 80% of symbol arrivals as expected duplicates (C-005). Evidence: docs/audit/signal_mortality_census_19jul2026.md lines 191 to 196. The settings Chartink displayed on 21-Oct for one testing alert (1 minute, Continue, Duplicate; section 4.3) agree with that cadence: agreement, not proof for production in June and July. By retry: UNKNOWN (13.1).

### 13.5 Late arrival
FACT (CONFIG): the queue settings' comment says "Chartink triggered_at is scan time, not delivery time", a claim about Chartink that neither the configuration nor the code establishes. FACT (SOURCE-VERIFIED): a symbol whose triggered_at is more than 600 seconds old on receipt is EXPIRED (C-009), and a time-only value takes today's date (section 8.4; C-004). Evidence: config/system_config.yaml line 127; signals/webhook_receiver.py lines 916 to 926.

### 13.6 Concurrent arrival
FACT (SOURCE-VERIFIED): up to eight requests are handled at once (threads=8); the same symbol arriving twice at once gets IN_PROCESS from the in-flight claim (C-005, C-006). Evidence: main.py line 3994; signals/webhook_receiver.py lines 928 to 935; signals/webhook_receiver.py lines 1074 to 1091. Bursts: section 5.6.

## 14. Arrival records (FACT, RECORD-DERIVED; each true for its own date and scope)
Each counts webhook_audit rows (S5) unless noted; none measures S2 to S4. Addresses are not reproduced.

| ID | Record | What it says | Evidence |
|---|---|---|---|
| R1 | C2 source-address investigation, 03-Jul-2026 | 12-Jun to 03-Jul, 15 trading days (the span's 16 weekdays less the Muharram holiday of 26-Jun-2026, FACT (CONFIG): config/nse_holidays_2026.yaml lines 34 to 35): 58,507 requests from 4 distinct addresses; one address sent 58,504 (99.99%) on every trading day (1,931 to 4,592 a day); two single requests from the same /24 carried realistic payloads; no proxy logs exist | docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 12 to 13; docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 17 and 26; docs/audit/c2_chartink_ip_investigation_03jul2026.md lines 33 to 39 |
| R2 | System-map entries, 05 and 06-Jul | 38,046 POSTs from the sole Chartink source by 05-Jul; 06-Jul: 86 x 200 and 309 x 403, no 401, all from that address; every stored payload to 05-Jul held the token (95,431 rows) | docs/SYSTEM_MAP.md lines 1364 to 1366 |
| R3 | Day reconstruction, 15-Jul | 13 scanners posting 11 to 374 times each that day; no PB-01 POST had ever arrived; the record then read the PB-01 alert as not yet activated (R6, a later record, reports PB-01 triggers from 13-Jul meeting a closed port; K-11) | docs/audit/day_reconstruction_15jul2026.md lines 28 to 29; docs/audit/day_reconstruction_15jul2026.md line 80 |
| R4 | Signal-mortality census, 19-Jul | 12-Jun to 16-Jul: 89,794 POSTs, 25,960 answered 403 and 18 answered 503; symbol-level 146,305 accepted and 643,402 rejected; Chartink posts each scanner about once a minute | docs/audit/signal_mortality_census_19jul2026.md lines 39 to 41; docs/audit/signal_mortality_census_19jul2026.md lines 191 to 196 |
| R5 | Refused-POSTs investigation, 23-Jul | To 23-Jul 15:29: 104,911 POSTs = 76,213 x 200, 28,680 x 403, 18 x 503, and 0 for 401, 404, 429, 400 and 500; the first row is 2026-06-12T09:17. In its snapshot of 12-Jun to 16-Jul (25,960 of the 403s): the 403s fell on weekdays only, 99.99% from one Chartink address, two from the same /24 and one from a local address (labelled "localhost (self/health)"), by hour from 09 to 15, plus 2 at 21. In-window 403s fell on exactly 7 dates (15 to 19-Jun, 23-Jun, 01-Jul), each inside a kill block; on 18, 19 and 23-Jun and 01-Jul POSTs were accepted again after a restart (23-Jun: the first in-window 403 at 10:08, a restart at 12:26, 1,789 answered 200 after the first refusal). The 19-Jun outage left a gap with no rows | docs/audit/refused_posts_403_investigation_23jul2026.md lines 72 to 122; docs/audit/refused_posts_403_investigation_23jul2026.md lines 131 to 169; docs/audit/refused_posts_403_investigation_23jul2026.md lines 237 to 244 |
| R6 | Service-window records, 24 and 25-Jul | All ten PB-01 alerts at 17:00 from 13-Jul met a closed port; their candidates are in Chartink's trigger history (S3, as the record reports it); the self-exit moved to 17:35 | docs/audit/service_window_extension_2026-07-24.md line 55; config/system_config.yaml lines 105 to 112 |
| R7 | Integrity audit, Phase 1 measured 31-Jul (its security phase 01-Aug; a 03-Aug note) | 127,272 rows from 12-Jun; no 404 at any time; 18 x 503, none since 01-Jul; the two range-breakout keys 0; PB-01 5 POSTs, 27 to 31-Jul; from 24 to 31-Jul, 2,853 to 3,507 in-window 200s a day and zero 400, 401, 404, 429, 500 and 503; the IA-P1-04 hole | docs/audit/integrity_audit_2026.md lines 28 to 29; docs/audit/integrity_audit_2026.md lines 84 to 106; docs/audit/integrity_audit_2026.md lines 126 to 135; docs/audit/integrity_audit_2026.md lines 212 to 230; docs/audit/integrity_audit_2026.md lines 298 to 302 |
| R8 | 04-Aug records | Test POSTs to the live webhook were pre-authorised on the development PC (15 entries of the coding assistant's PC-local permission list, "POST a hand-written payload to the live webhook"; the example posts to the local address) and called "a real diagnostic, used repeatedly in this campaign"; no counts | docs/audit/r12_allowlist_enumeration_04aug2026.md lines 1 to 8; docs/audit/r12_allowlist_enumeration_04aug2026.md lines 73 to 93; docs/audit/d4_matcher_measurement_04aug2026.md line 59 |

Coverage and limits:
- The records measure database copies on their own dates; none covers 01-Aug to the baseline except the 03-Aug note (C-001 U-06); refused or never-made attempts are in none of them (S4).
- Not every row came from Chartink: R1 and R5 record requests from other addresses, one of them local (INTERPRETATION for R8's POSTs: they would arrive from a local address; how many were sent, and when, is not recorded).
- No 400 at all in R5 (12-Jun to 23-Jul 15:29) or R7 (24 to 31-Jul), so no audited production request met the alert-name 400 in those windows; outside them, C-001 section 7.6's UNKNOWN stands (a narrowing, recorded here).
- The 23-Jun kill: A-011 section 14 and B-001 section 13 say that every later signal was refused that day; R5 shows the kill ending at the 12:26 restart, after which POSTs were accepted again (errata A-011 E-8 and B-001 E-4, accepted, section 22).
- B-009 section 13's census line "Arrived 165,111" counts signals (symbol arrivals, re-sent about once a minute), not POSTs; the POST counts are webhook_audit's above (a clarification, recorded here).

## 15. Paper and live; declared and installed
- FACT (SOURCE-VERIFIED; bounded): no paper or live branch in signals/webhook_receiver.py (a pattern search for mode and paper names: no match) or in the server start (main.py lines 3970 to 4001); the receiver is constructed without a mode (C-001 section 10; main.py lines 3407 to 3415). This is not a claim about either environment's installed configuration.
- Declared, not installed: everything in this manual is what the baseline declares and the dated records report. The VM's installed libraries (requirements.txt pins flask==3.1.3 and waitress==3.0.2 and does not pin Werkzeug), its deployed configuration and its firewall state are UNKNOWN (U-4, U-6).

## 16. Tests inspected, not executed
No test was run. In the nine webhook test files listed in the answers file (section 7) no waitress, socket, network-client or app.run call was found (search S20: 0 matches), and seven of them use Flask's test client (S21); so none exercises the server, the network, Chartink's real bodies or the framework's own answers. In the 486 test files of S-ALL no test asserts a 413 answer or its missing audit row (search S22); one asserts the limit's value (tests/unit/test_phase17_batch2.py lines 69 to 71).

| Test | Asserts | Does not prove |
|---|---|---|
| tests/unit/test_webhook_receiver.py lines 134 to 1006 | By the test names: the route's 200, 400, 401, 403 and 404 answers, the backpressure and batch 503s and the per-symbol statuses | 429, the shutdown 503, the 500 and the EOD route (other files below, or none); the framework's own answers; waitress |
| tests/unit/test_webhook_receiver.py lines 1007 to 1038 | The audit row for four outcomes (the test's name says "for every post") | Coverage of 413 or of a failed insert |
| tests/unit/test_webhook_receiver.py lines 1041 to 1071 | Five concurrent POSTs | Eight-thread waitress behaviour |
| tests/unit/test_webhook_receiver.py lines 1745 to 1776 | The token redacted from the stored payload | The log lines of section 10.6 |
| tests/unit/test_c2_webhook_lockdown.py lines 23 to 77 | The limiter's burst, refill, per-address independence, open-bell burst and eviction | Chartink's real burst |
| tests/unit/test_c2_webhook_lockdown.py line 85 | The secret required in every mode | The installed environment |
| tests/unit/test_s4_webhook_degrade.py lines 55 to 100 | An unreachable self-check degrades instead of stopping | Reachability from outside the VM |
| tests/unit/test_fix134_backpressure.py lines 80 to 135 | The queue-depth headers and the 503 threshold | A sender's reaction to them |
| tests/unit/test_fix134_backpressure.py lines 166 to 213 | The WEBHOOK /health behind the secret and the limiter | The port-8080 server |
| tests/unit/test_v3_eod_route.py lines 59 to 113 | The EOD route's capture, window bypass, fail-safe miss and malformed payload | Production wiring |
| tests/unit/test_c6_auth_collapse.py lines 337 to 382 | The EOD route after hours and under a kill, after authentication; 404 behind authentication; a 401 audited on /webhook but not on the WEBHOOK /health | The kill-day records of A-011 section 10 |
| tests/unit/test_e4_event_loop_safety.py lines 319 to 380 | 503 after stop(); the receiver stopped before the processor; the in-flight claim under concurrency | The live shutdown sequence (B-011) |
| tests/integration/test_hardening_scenarios.py line 238 | A full queue, a drain and a retry | That Chartink retries |
| tests/integration/test_hardening_scenarios.py line 301 | The self-check against an authenticated WEBHOOK /health | That the answering process was the receiver |
| tests/integration/test_end_to_end_smoke.py lines 326 and 363 | Deduplication and the kill switch at the webhook | The installed configuration |

## 17. Source-to-consumer trace (source wiring at the baseline, not host execution)
| Step | Source | Input | Logic or calculation | Validation or guard | Output | Storage or hand-off | Next consumer |
|---|---|---|---|---|---|---|---|
| T1 Chartink alert | External (S2, S3) | Its scan's results | A POST to the URL the alert holds (section 4) | None in the system | An HTTP request (S4) | None | The VM's firewall, then waitress |
| T2 waitress | main.py lines 3987 to 4000 | A TCP connection on port 5000 | HTTP parsing; 8 threads | Its own limits (sections 6.2 and 8.2) | A WSGI call | None | The Flask app |
| T3 Flask routing | signals/webhook_receiver.py lines 360 to 413 | Method and path | Rule match | 404, 405 or 308 from the framework; OPTIONS answered 200 (section 9, row 1) | The view | None | _handle_webhook |
| T4 _handle_webhook | signals/webhook_receiver.py lines 477 to 534 | The source address and the raw body | The body read; shutdown flag; per-address bucket | 413 at the body read (no row); 503 or 429 | The answer, or the call below | One webhook_audit row per answer once the body has been read, unless the insert fails (signals/webhook_receiver.py lines 1126 to 1154; section 10.3) | _process_request |
| T5 _process_request | signals/webhook_receiver.py lines 536 to 745 | Path key, body, token | Authentication (C-003), key, EOD branch, kill, backpressure, window, parse, fields, alert name | 401, 404, 403, 503 or 400 | The per-symbol loop, or the EOD route | None | _process_signal or _handle_eod |
| T6 _process_signal | signals/webhook_receiver.py lines 892 to 1072 | Symbol, price, times | Alias, exclusion, validity, expiry, in-flight claim, deduplication, fingerprint | A per-symbol status | ACCEPTED or a refusal | The signals row (QUEUED) and the queue tuple | SignalProcessor (C-006 to C-008) |
| T7 _handle_eod | signals/webhook_receiver.py lines 751 to 804 | The body | Parse and fields | 400 | 200 | eod_capture.submit and the cron_heartbeat row (signals/webhook_receiver.py lines 776 to 834) | The watchlist capture worker (C-011) |
| T8 The batch answer | signals/webhook_receiver.py lines 730 to 745 | Per-symbol results | Any retryable status | - | 200 or 503 with the queue headers | The audit row (T4) | Back through waitress to the sender; whether it was received: S4 side, not in the system |

## 18. Known defects, ambiguities and documentation drift
| ID | Finding | Label | Section |
|---|---|---|---|
| K-1 | The docstring's WR13 and the schema comment say every POST is audited; the body read and the framework's own answers come before or outside the audit write | KNOWN DOC DRIFT | 10.3 |
| K-2 | WR5's response list omits 429; WR10 names OUTSIDE_HOURS, which the code never returns | KNOWN DOC DRIFT | 9 |
| K-3 | The class docstring shows receiver.app.run(...); production serves through waitress (signals/webhook_receiver.py lines 145 to 149; main.py lines 3983 to 4000) | KNOWN DOC DRIFT | 6.1 |
| K-4 | WR11 says "each Flask request in its own thread"; under waitress requests share a pool of eight threads (signals/webhook_receiver.py line 23; main.py line 3994) | KNOWN DOC DRIFT | 13.6 |
| K-5 | Two receiver comments (G.1, 25-Apr) say a token in the URL is logged by nginx; no nginx configuration exists in S-ALL and the 03-Jul record found none on the VM. The bind comment names a route "via ngrok/external"; the daily runbook says "Check ngrok/tunnel is active" and the incident guide "Check ngrok/tunnel status"; S-ALL code holds no tunnel, and from 12-Jun the recorded TCP peer was Chartink's own address, which a tunnel would have replaced (INTERPRETATION). Evidence: signals/webhook_receiver.py lines 196 to 199; signals/webhook_receiver.py lines 307 to 310; config/system_config.yaml lines 328 to 334; docs/03_daily_operations_runbook.md lines 128 to 132; docs/05_incident_response.md line 248 | KNOWN DOC DRIFT | 5.4 |
| K-6 | The bind comment relies on require_hmac being true; require_hmac is false | KNOWN DEFECT / AMBIGUITY | 5.1 |
| K-7 | The ts column's "when request arrived"; the row is written after handling | KNOWN DOC DRIFT (minor) | 10.4 |
| K-8 | The sanitiser's comments cite authentication at old line numbers ("auth (:410-432)"); at the baseline the call is at signals/webhook_receiver.py lines 547 to 549 (the comments: signals/webhook_receiver.py lines 687 and 865) | KNOWN DOC DRIFT (minor) | 9 |
| K-9 | The runbook's first check, "curl http://localhost:5000/health", gets 401 since the WEBHOOK /health was put behind the secret (docs/03_daily_operations_runbook.md line 129; signals/webhook_receiver.py lines 382 to 393) | KNOWN DOC DRIFT | 7.1 |
| K-10 | The queue schema comment gives the expiry as "default 60"; the field has no default and the configuration says 600 (core/config_loader.py line 196; config/system_config.yaml line 127) | KNOWN DOC DRIFT | 13.5 |
| K-11 | Records disagree: on the proxy (03-Jul "Confirmed absent"; 31-Jul still "UNCERTAIN") and on PB-01 (15-Jul: the alert not yet activated; 24-Jul: all ten 17:00 alerts since 13-Jul met a closed port) | KNOWN DOC DRIFT (records) | 5.4, 14 |
| K-12 | Code comments and June and July records assume that Chartink retries; the assumptions record lists it as NOT_TESTED_YET | KNOWN DOC DRIFT (claims) | 13.1 |
| K-13 | The WR4b docstring's "The account name cannot be dropped in the Chartink UI, so the receiver absorbs it": nothing supports it, since Rama typed the tag on the testing scans shown on 21-Oct himself (whether Chartink lets a scan be renamed: U-9) | KNOWN DOC DRIFT (unsupported rationale) | 8.3 |

Related, recorded in C-001 and cited here: D-06 (webhook_audit counts nothing for a non-200 answer and counts EOD captures as accepted; section 10.2) and D-07 (the alert-name rule's limits; section 8.3).

## 19. Unknown / not established
- U-1. Chartink's side: the alerts, the URLs saved on them, the alert names, the frequency and duplicate settings, and their history over the live period, beyond the records' reports and, for testing alerts on 21-Oct-2026 only, Rama's screenshots (C-001 U-01).
- U-2. Chartink's timeout, retry and redirect behaviour, and how it treated 4xx and 5xx answers (C-001 U-07).
- U-3. Requests attempted while the port was closed, other than PB-01's (R6); the delivery or receipt of any answer.
- U-4. The VM's network state on any date: its interfaces, the iptables rule after 05-Jul, the cloud security list, DNS.
- U-5. The bind address value (masked; its comment says all interfaces) and the self-check's target (masked; its docstring says local).
- U-6. The installed state: library versions (Werkzeug is not pinned), the deployed configuration, and whether the installer ran as committed.
- U-7. Requests that waitress or Flask answered without an audit row, and failed audit inserts: no logs survive.
- U-8. Arrivals from 01-Aug-2026 to the baseline (C-001 U-06), and before 12-Jun-2026.
- U-9. Whether Chartink lets an existing scan or alert be renamed, and whether it requires distinct scan names within one login.
- U-10. Whether Rama's Chartink plan limited the number of active alerts, in the live period or now; one web search found nothing on this. Recorded because V2's two systems would need more alerts (C-002_v2_design_inputs.md section 4, question 8).
- Not C-002's to settle: why the two range-breakout keys never posted (C-001 section 9.2).

## 20. Negative claims and their scope (all at the baseline)
- N1. No configuration of the sender's webhook URL in S-ALL (section 4.1).
- N2. No code that creates, updates, verifies or deploys Chartink alerts, in S-CODE and S-RT (section 4.1).
- N3. No reverse-proxy, tunnel, TLS or certificate configuration in S-CODE (searches S04 and S19).
- N4. No use of X-Forwarded-For by the receiver; in S-ALL the forwarded-header names occur only in the dashboard backend, its test, a record and the deployment notes (search S03).
- N5. No route on the webhook app other than the WEBHOOK /health (GET) and POST /webhook/<scanner_name> (the receiver file).
- N6. No scheduler, cron or unit entry that sends or re-sends webhooks (the cron file, the cron registry and the service unit).
- N7. No reload of the webhook or map settings in S-RT (C-001 search 15).
- N8. No code path returning the per-symbol status OUTSIDE_HOURS (the receiver file).
- N9. No test that exercises waitress, a socket or the network on the webhook path: the receiver tests state that they use Flask's test client (tests/unit/test_webhook_receiver.py lines 1 to 11), and in the nine webhook test files no server, socket or network call was found (searches S20 and S21; none was run).
- N10. No paper or live branch in signals/webhook_receiver.py or in the server start (section 15).

## 21. Answer-sheet status
| Sheet question | Answers file | This manual |
|---|---|---|
| Q1 Declaring files and keys | Q1 | 4.1 |
| Q2 URL, path, names and sender metadata | Q2 | 4.1 to 4.4 |
| Q3 Map and URL files, and their readers | Q3 | 4.1 (with C-001 sections 4 and 5) |
| Q4 Configured URL against the entered URL | Q4 | 4.2 and 4.5 |
| Q5 Alert automation | Q5 | 4.1 |
| Q6 What C-001 establishes | Q6 | 1, 4 and 23 |
| Q7 Server, app and route | Q7 | 6 and 7.1 |
| Q8 Method, pattern and the key in the path | Q8 | 7.2 and 7.3 |
| Q9 Path per scanner | Q9 | 7.2 |
| Q10 Prefix, joining, slash, query, proxy | Q10 | 5.4, 6.1, 7.3 and 7.4 |
| Q11 Fields and headers | Q11 | 8 |
| Q12 Response contract | Q12 | 9 |
| Q13 Timeout, retry, redirect, TLS, DNS, port, proxy | Q13 | 5, 6.2 and 7.3 |
| Q14 Hand-off | Q14 | 11 |
| Q15 Failures | Q15 | 12 |
| Q16 Retries | Q16 | 13.1 to 13.3 |
| Q17 Repeats, lateness, concurrency | Q17 | 13.4 to 13.6 |
| Q18 Logging and audit | Q18 | 10 |
| Q19 Negative claims | Q19 | 20 |
| Q20 Arrival records | Q20 | 14 |
| Q21 Files inspected | Q21 (source inventory, section 6) | 2; C-002_review_notes.md section 1 |
| Q22 Anchors | Q22 | Throughout; 17 |
| Q23 Drift | Q23 | 18 |
| Q24 What cannot be established | Q24 | 19 |
| Q25 Frozen manuals and errata | Q25 | 22 and 23 |

Corrections to the answers file and the supplement are listed in C-002_reconciliation_addendum.md section 3; both files stay unchanged as inputs.

## 22. Corrections to frozen manuals (nine errata, ACCEPTED, DR-055; exact texts)
Status: ACCEPTED (DR-055, 2026-10-22). Nine errata to eight frozen manuals, found during C-002 and proposed in revision 0 (2026-10-21); ChatGPT's value-added finalisation review accepted them in principle, and Rama accepted each on its own on 22-Oct-2026 ("Accept all nine (Recommended)"), as practised at the C-001 freeze (DR-053: "Errata ACCEPTED, decided on their own"). No frozen manual was changed before his decision. The authoritative passages are this manual's sections 6.6, 8.3, 9, 10.2, 10.3, 13.1 and 14 (R5); each erratum cites them instead of restating them (V2_DESIGN_LOG lesson of 2026-10-13). In each manual, as practised, the struck text stays visible with "(superseded by erratum E-n)" and the replacement follows; the status line gains one sentence; an erratum section is added at the end; and the manual's review notes gain one status sentence. Removing each erratum's insertions restores the committed bytes at main 75c1eb7 (C-002_review_notes.md sections 5 and 9.4). Three records that repeat corrected claims stay unchanged and are superseded, as their manuals' erratum sections state (after the list of NC items below). Exact texts, with the unchanged text on either side for location:
- C-001 E-1 (EC-1; the request audit's coverage). Status: ACCEPTED (DR-055).
  - section 3, the term "Webhook delivery" (C-001 line 40 at main 75c1eb7):
    - Struck: "one webhook_audit row whatever the response"
    - Replacement: "one webhook_audit row whatever the response, when the request reaches the handler, its body is read and the insert succeeds (C-002_manual.md section 10.3)"
    - Unchanged before it (its last 55 of 55 characters): "| Webhook delivery | A POST that reached the receiver; "
    - Unchanged after it (its first 2 of 2 characters): " |"
  - section 6.6, level 3 (C-001 line 194 at main 75c1eb7):
    - Struck: "A webhook_audit row for the POST, whatever the response"
    - Replacement: "A webhook_audit row for the POST, whatever the response, when the request reaches the handler, its body is read and the insert succeeds (C-002_manual.md section 10.3)"
    - Unchanged before it (its last 24 of 24 characters): "| 3. Webhook delivery | "
    - Unchanged after it (its first 80 of 148 characters): "; the key from the path | That any symbol was taken | signals/webhook_receiver.p"
  - section 7.1, step 2 (C-001 line 205 at main 75c1eb7):
    - Struck: "it writes one webhook_audit row in every case"
    - Replacement: "it writes one webhook_audit row in every case once the body has been read, unless the insert fails; the framework's own answers and a body over 1 MiB (answered 413 at the read) leave no row (C-002_manual.md section 10.3, NR-1 to NR-4)"
    - Unchanged before it (its last 80 of 107 characters): "503 during shutdown and 429 above the per-IP limit, then runs _process_request; "
    - Unchanged after it (its first 48 of 48 characters): " (signals/webhook_receiver.py lines 477 to 534)."
  - section 9.2, the reading of R-01 (C-001 line 274 at main 75c1eb7):
    - Struck: "at the baseline the receiver records every POST that reaches it, whatever its response (7.1, step 2)"
    - Replacement: "at the baseline the receiver records every POST that reaches its handler and whose body is read, whatever its response, unless the insert fails (7.1, step 2; C-002_manual.md section 10.3)"
    - Unchanged before it (its last 57 of 57 characters): "- Read together with R-01 (INTERPRETATION / AUDIT LEAD): "
    - Unchanged after it (its first 80 of 758 characters): ", and the record counted webhook_audit rows of every response code and no 404 at"
- C-001 E-2 (EC-2; the status of errata A-008 E-4 and B-002 E-2). Status: ACCEPTED (DR-055).
  - section 18, the A-008 cross-reference (C-001 line 391 at main 75c1eb7):
    - Struck: "errata E-1 and, proposed, E-4"
    - Replacement: "errata E-1 and E-4"
    - Unchanged before it (its last 80 of 163 characters): "rce_intraday_only; erratum E-1); A-008 (strategy model and Chartink conditions; "
    - Unchanged after it (its first 80 of 497 characters): "); A-009 (configuration; sections 3, 5 and 16); A-010 (the webhook secret's loca"
  - section 18, the B-002 cross-reference (C-001 line 391 at main 75c1eb7):
    - Struck: "erratum E-2, proposed"
    - Replacement: "erratum E-2"
    - Unchanged before it (its last 80 of 348 characters): "location); A-011 (start-up scenarios, section 6); B-002 (configuration loading; "
    - Unchanged after it (its first 80 of 320 characters): "); B-007 (start-up checks, pre-flight and the dashboard's readiness views); B-00"
- A-008 E-5 (NC-8; Rama's account of the testing tag beside the WR4b comment; a clarification only). Status: ACCEPTED (DR-055).
  - section 9, the bullet on WR4b (an addition at its end) (A-008 line 105 at main 75c1eb7):
    - Added at the end of the line: "Clarified by erratum E-5: HISTORICAL (Rama, 21-Oct-2026), he typed the tag on the testing scans shown in his Chartink screenshot of that day himself, so nothing supports the comment's "cannot be dropped in the Chartink UI" (whether Chartink lets a scan be renamed: UNKNOWN); the code's behaviour is unchanged (C-002_manual.md section 8.3)."
    - Unchanged before it (its last 80 characters): "duction's tag never appears in its alert names, so the strip does nothing there."
- A-010 E-4 (EC-5; which /webhook outcomes reach webhook_audit). Status: ACCEPTED (DR-055).
  - section 10, the bullet on the webhook's authentication (A-010 line 113 at main 75c1eb7):
    - Struck: "every /webhook outcome, 401 included, is written to webhook_audit (RECORD-DERIVED, 25-Jul C6 record)"
    - Replacement: "every /webhook outcome, 401 included, is written to webhook_audit (RECORD-DERIVED, 25-Jul C6 record) when the request reaches the handler, its body is read and the insert succeeds (C-002_manual.md section 10.3)"
    - Unchanged before it (its last 80 of 253 characters): "mac is false), compared in constant time; unauthenticated requests get 401, and "
    - Unchanged after it (its first 80 of 138 characters): ". The code notes that "Tokens in URL are logged by nginx and weaker than HMAC". "
- A-011 E-8 (EC-7, EC-3, EC-4, EC-8; what the webhook refused under a kill, what its 503 asked of the sender, what its audit row records for refusals, and the end of the 23-Jun kill). Status: ACCEPTED (DR-055).
  - section 10, the "collision" reading (A-011 line 215 at main 75c1eb7):
    - Struck: "so the webhook refused every signal with 403 (section 12)"
    - Replacement: "so the webhook refused every intraday-type signal with 403 (section 12); on the code path an eod-type alert leaves for the EOD route before the kill gate (C-002_manual.md section 9)"
    - Unchanged before it (its last 80 of 576 characters): "ive and did nothing, and the running process kept the previous day's SOFT_KILL, "
    - Unchanged after it (its first 80 of 114 characters): ". Whether this still happened after 25-Aug: NOT ESTABLISHED (the 31-Aug plan sug"
  - section 12, the webhook gate order (A-011 line 226 at main 75c1eb7):
    - Struck: "so that Chartink retries"
    - Replacement: "intending to permit a sender's retry; whether Chartink retried is UNKNOWN (C-002_manual.md section 13.1)"
    - Unchanged before it (its last 80 of 650 characters): "d the in-memory queue; QUEUE_FULL or a store error makes the request answer 503 "
    - Unchanged after it (its first 1 of 1 characters): "."
  - section 12, the refusal bullet (A-011 line 227 at main 75c1eb7):
    - Struck: "only the request's webhook_audit row, with accepted and rejected counts, records them"
    - Replacement: "only the request's webhook_audit row records them: a kill, window or capacity refusal by its response code alone, a per-symbol refusal inside a 200 by the rejected count, and one inside a 503 batch by neither count (C-002_manual.md section 10.2)"
    - Unchanged before it (its last 80 of 191 characters): "excluded, invalid, expired, in process, cached duplicate) leave no signals row; "
    - Unchanged after it (its first 1 of 1 characters): "."
  - section 12, Q31 (A-011 line 230 at main 75c1eb7):
    - Struck: "only the request's audit counts (above)"
    - Replacement: "only the request's audit row: its response code, and inside a 200 its rejected count (above)"
    - Unchanged before it (its last 80 of 688 characters): "hree outcomes are kept apart: a refusal at the webhook door left no signal row, "
    - Unchanged after it (its first 80 of 259 characters): "; a queued signal on a clean stop, kill or expiry received a terminal rejection "
  - section 14, Q65, the 23-Jun incident (A-011 line 245 at main 75c1eb7):
    - Struck: "every later signal refused with 403 that day"
    - Replacement: "later signals refused with 403 until a restart at 12:26, after which POSTs were accepted again (the 23-Jul refused-POSTs record; C-002_manual.md section 14, R5)"
    - Unchanged before it (its last 80 of 453 characters): "false SOFT_KILL (one MIS block and two slippage aborts counted as API failures; "
    - Unchanged after it (its first 80 of 380 characters): "; FIX-191); 17-Jul boot outage (the self-check called the newly authenticated /h"
- B-001 E-4 (EC-7, EC-8; what the webhook refused under a carried kill, and the end of the 23-Jun kill). Status: ACCEPTED (DR-055).
  - section 13, the row for Q-B001-12 (the 06-Aug record) (B-001 line 224 at main 75c1eb7):
    - Struck: "refusing every signal (06-Aug record)"
    - Replacement: "refusing every intraday-type signal (06-Aug record; on the code path an eod-type alert leaves for the EOD route before the kill gate, C-002_manual.md section 9)"
    - Unchanged before it (its last 80 of 271 characters): "g a carried CNC position kept the service up with the previous day's SOFT_KILL, "
    - Unchanged after it (its first 58 of 58 characters): "; on 23-Jun a false SOFT_KILL refused every later signal |"
  - section 13, the row for Q-B001-12 (23-Jun) (B-001 line 224 at main 75c1eb7):
    - Struck: "on 23-Jun a false SOFT_KILL refused every later signal"
    - Replacement: "on 23-Jun a false SOFT_KILL refused the later signals until a restart at 12:26, after which POSTs were accepted again (the 23-Jul refused-POSTs record; C-002_manual.md section 14, R5)"
    - Unchanged before it (its last 80 of 310 characters): "ce up with the previous day's SOFT_KILL, refusing every signal (06-Aug record); "
    - Unchanged after it (its first 2 of 2 characters): " |"
- B-008 E-1 (EC-1; the request audit's coverage at ingress). Status: ACCEPTED (DR-055).
  - section 3, the stage table's Ingress row (B-008 line 41 at main 75c1eb7):
    - Struck: "audit row (531 to 534) on every request"
    - Replacement: "audit row (531 to 534) on every request whose body is read, unless the insert fails (C-002_manual.md section 10.3)"
    - Unchanged before it (its last 80 of 138 characters): "4 | HTTP POST from Chartink | shutdown flag, per-IP limit | 503 or 429, or on | "
    - Unchanged after it (its first 21 of 21 characters): " | _process_request |"
- B-009 E-2 (EC-1; the request audit's coverage). Status: ACCEPTED (DR-055).
  - section 5, the bullet on the webhook's answers (B-009 line 136 at main 75c1eb7):
    - Struck: "The webhook audit row is written for every request (B-008 section 3)."
    - Replacement: "The webhook audit row is written for every request that reaches the handler and whose body is read, unless the insert fails (B-008 section 3; C-002_manual.md section 10.3)."
    - Unchanged before it (its last 80 of 80 characters): "- The webhook's EXPIRED, IN_PROCESS and DUPLICATE answers are not signals rows. "
    - Unchanged after it (its first 0 of 0 characters): ""
- B-011 E-1 (EC-6; the webhook server and the separate health server). Status: ACCEPTED (DR-055).
  - section 6, the row "Webhook and health HTTP server" (B-011 line 173 at main 75c1eb7):
    - Struck: "`/health` is a route on the webhook Flask app (`signals/webhook_receiver.py` 364), served by waitress in a daemon thread (`main.py` 3988-4000). `_shutdown()` does not stop it; it ends at process exit"
    - Replacement: "Two servers: the webhook Flask app on port 5000, with its own `/health` route (`signals/webhook_receiver.py` 364; the WEBHOOK /health in B-007's terms), served by waitress in the daemon thread webhook-server (`main.py` 3988-4000), and the separate health server on port 8080, waitress in the daemon thread healthcheck-server (`main.py` 4073-4082; B-006 rows TH24 and TH25). `_shutdown()` stops neither; each ends at process exit (C-002_manual.md section 6.6)"
    - Unchanged before it (its last 35 of 35 characters): "| Webhook and health HTTP server | "
    - Unchanged after it (its first 27 of 27 characters): " | FACT (SOURCE-VERIFIED) |"

Recorded in this manual only, with no erratum proposed (clarifications and narrowings; the frozen statements stay true as written):
- NC-1. C-001 section 7.6 leaves UNKNOWN whether production alert names ever met the alert-name 400; R5 (12-Jun to 23-Jul 15:29) and R7 (24 to 31-Jul) record no 400 at all, so no audited production request did in those windows (section 14).
- NC-2. B-009 section 13's "Arrived 165,111" counts signals, not POSTs (section 14).
- NC-3. A-010 section 10's "by 27-Jul" for the single-source rule: the records give 05-Jul for applying it (section 5.2).
- NC-4. "/health" for the receiver's route in A-011 section 14, B-001 section 13 and C-001 sections 6.5 and 6.6, against B-007's terms (section 6.6).
- NC-5. For C-003, not C-002: B-007 calls the webhook control a "signature", while production used the query token (A-009 section 16; A-010 section 10); A-008 section 24 points to A-006 for webhook authentication, and A-006 has no webhook content. Carried to C-003, whose subject it is (W-6).
- NC-7. A lead for A-008 section 9's "When that outage began: UNKNOWN": the prefixed testing scans listed on Chartink's dashboard on 21-Oct were shown as created on 08-Sep-2026, one day before WR4b (another entry, a scan marked personal, shows 17-Sep; section 4.3). Only a lead: the date may belong to the scan rather than its alert, earlier prefixed alerts may have existed, and no record of the testing VM's 400 answers was read. Rama chose on 22-Oct-2026 to record it here only ("C-002 only (Recommended)"); A-008 E-5 stays a clarification.
- NC-6 of the research is withdrawn: the record's "15 trading days" for 12-Jun to 03-Jul is right, the span's 16 weekdays less the Muharram holiday of 26-Jun-2026 (section 14, R1; C-002_reconciliation_addendum.md section 3).

Checked and not changed: A-011 sections 5 and 11, and section 14 apart from the passage of A-011 E-8 (the start order, the receiver stopped first, the self-check; the "/health" term is NC-4); B-006 section 3 rows TH24 and TH25 and row F6; B-007 sections 4 and 6; B-008 sections 1 and 9 and section 3 apart from the passage of B-008 E-1; B-009 section 13 and section 5 apart from the passage of B-009 E-2; B-005 row S18; A-009 section 16; B-001 sections 3, 6, 7.2 and 14; C-001 sections 4 to 9 apart from the passages of C-001 E-1. Three records stay unchanged and are superseded, as their manuals' erratum sections state: B-011_reconciliation_addendum.md item 4 (B-011 E-1), B-008_reconciliation_addendum.md item 2 (B-008 E-1) and A-011_review_notes.md rows F-12 and Q86 (A-011 E-8). The frozen-manual searches behind these lists: C-002_review_notes.md section 4.

## 23. Cross-references
A-001 and DR-012 (both legacy VMs terminated); A-008 section 9 (the testing tag and WR4b; erratum E-5, accepted); A-009 sections 5 and 16 (the start-window guard; require_hmac and the query token); A-010 section 10 (the webhook secret; erratum E-4, accepted); A-011 sections 5, 10, 11, 12 and 14 (the start order, the self-exit, shutdown, the gate order, the 23-Jun incident; erratum E-8, accepted); B-001 sections 3, 6, 7.2, 13 and 14 (the listener, the daily start, the service window; erratum E-4, accepted); B-003 section 7 and B-006 sections 3 and 4 (threads and the start's timing); B-005 row S18; B-007 sections 4 and 6 (the self-check; the two HTTP servers' terms); B-008 sections 3 and 9 (the 403 before 10:00; the audit row at ingress; retries NOT ESTABLISHED; erratum E-1, accepted); B-009 sections 5 and 13 (the audit row; the census; erratum E-2, accepted); B-011 sections 3.1 and 6 (the self-exit; the servers at shutdown; erratum E-1, accepted); C-001 sections 4 to 9, 13 and 14 (the definitions, the key at the edge, attribution, the arrival records, D-06, D-07, U-01, U-06 and U-07; errata E-1 and E-2, accepted); C-003 (authentication and the secret); C-004 (fields and symbols); C-005 (in-flight claim and deduplication); C-006 to C-009 (queue, persistence, processing, expiry); C-010 (mapping); C-011 (the EOD route); VP-11, VP-45, VP-50, VP-55, VP-56, VP-58, VP-59 and Q-46 (C-002_v2_design_inputs.md).

## 24. Final C-002 statement
> At the baseline the repository did not hold the webhook URL saved on any Chartink alert; it defined only the parts the receiver checked: port 5000, the path /webhook/<scanner key> and a query token, with the key equal to the map key. Records and Rama's accounts give the form used, plain HTTP to the VM's address with the token in the query string (a test fixture assumes the same), and a record shows that Chartink's body repeated that URL, token included. No code created or checked Chartink alerts. The webhook ran in the trading process as a waitress server with eight threads on port 5000 (its bind setting's comment says all interfaces), from step 15 of each start until the process exited; at any other time a POST met a closed port, as ten PB-01 alerts did in July. From 05-Jul, by the records, a firewall rule admitted only Chartink's address, and the rule's later state is unknown; no TLS was configured, and no proxy was recorded (03-Jul; the question was still carried as open on 31-Jul). The receiver answered in a fixed order: shutdown, rate limit, authentication, unknown key, the EOD branch, kill switch, backpressure, entry window and body checks, then per-symbol results, with 503 inviting a retry that no evidence shows Chartink made. It wrote one audit row for each request whose body it read, unless the insert failed; requests the framework or waitress answered themselves left no row, and every answer but 200 recorded zero counts. An accepted symbol became a QUEUED signals row and an in-memory queue entry for the processor; an eod key went to the watchlist capture worker. Dated records count 104,911 POSTs to 23-Jul, none refused for authentication, key or format, and Chartink posting each scanner about once a minute. Chartink's own settings, retries and timeouts, the VM's later network state and the installed library versions are not established.

## 25. Revision 0 (2026-10-21)
- Drafted by Claude from its two research files and its supplement, ChatGPT's value-added review, its reconciliation review and its continuation prompt, and Rama's answers and screenshots of 21-Oct; there is no ChatGPT draft to work from. The file set and the errata as diffs: Rama's choice of 21-Oct.
- Corrections carried from the reconciliation addendum (section 3): NC-6 withdrawn (the holiday of 26-Jun-2026); the token-in-the-logs lead bounded to the bytes logged (section 10.6); the supplement's header time; and the research files' points that the independent check found (addendum items 3.6 to 3.13).
- Added beyond the research files: the screenshots' facts and Chartink's Alerts Guide (sections 4.3 and 4.4); Rama's answer on the testing tag and K-13 (section 8.3); the health server's details (section 6.6); the paper and live check (section 15); the stable IDs NR-1 to NR-4 (section 10.3); the exact erratum texts (section 22).
- Before delivery an independent check by a separate agent compared these files with the research files, ChatGPT's reviews, the frozen manuals and the source (through the filter): 43 points, 4 of them errors. All are applied (C-002_review_notes.md section 9.1); they added errata B-008 E-1 and B-009 E-2 and the 23-Jun passages of A-011 E-8 and B-001 E-4 (EC-8). A wording search of docs/agenda and docs/governance at main 75c1eb7 after the check found three records that repeat corrected claims (B-008_reconciliation_addendum.md item 2; A-011_review_notes.md rows F-12 and Q86); they stay unchanged and are recorded as superseded in the erratum sections (section 22).
- No source fact of the research files was changed beyond the corrections listed; no V2 decision is made here.

## 26. Revision 1 and freeze (2026-10-22)
- ChatGPT's value-added finalisation review of revision 0 (22-Oct-2026: "CONDITIONAL ACCEPTANCE"; "No new evidence justifies changing a legacy fact in the manual") and its reply of the same day, written after Rama's clarification, are applied (C-002_reconciliation_addendum.md section 7).
- Rama decided on 22-Oct-2026 (his answers to Claude's questions): "Freeze, with VP-45 change (Recommended)", "Accept all nine (Recommended)", "C-002 only (Recommended)" and "Close: not needed (Recommended)"; recorded in DR-055.
- Revision 1 changed only: the status, date and built-from lines and the label line's errata clause; section 4.3 (the security bullet, and Rama's present-day status of 22-Oct-2026 as a separate bullet, labelled apart from the evidence); "proposed" to "accepted" where sections 6.6, 9, 10.2, 10.3, 13.1 and 14 cite the errata; section 22's heading, status paragraph and each erratum's status line; NC-7's last sentence; section 23's erratum statuses; and this section. No finding, number, citation or label of a legacy fact changed.
