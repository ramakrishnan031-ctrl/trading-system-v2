# A-010 SECRETS AND ENVIRONMENT: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN. Nothing in this file is a V2 decision. Items become decisions only as rows in V2_DECISION_REGISTER.md, at the topic that owns them (W-6). Updated 2026-10-09 after ChatGPT's value-added review (docs/agenda/a-010_secrets-environment/A-010_VALUE_ADDED_REVIEW_CHATGPT_TO_CLAUDE_09-OCT-2026.txt): exposure stated pending owner-side review, not as a breach (section 2); systemd credentials kept as one option with host and scheduled-job checks (section 3); the approval code not the sole control (section 4); broker-side protection recorded as a dependency for G/H (section 9); Rama's Q-29 answer recorded (section 10); history scans and a scan of the V2 repository (section 11); the static-IP date trail (section 13). Everything here stays PROPOSED / OPEN. The A-010 legacy manual is frozen (DR-025).
Date: 2026-10-09 (first pass 2026-10-08)
Place at: docs/agenda/a-010_secrets-environment/A-010_v2_design_inputs.md
Why this file exists: Rama asked that five points be noted "in the Manual" (Q20, Q22, Q25, Q30, Q33) and that suggestions and improvements be added "with suitable headings" (additional note). The legacy manual records legacy fact only (V2_DESIGN_LOG W-1, W-3), so the legacy facts behind those notes are in A-010_manual.md sections 4, 6, 8, 14 and 15, and the new-system material is here (sections 4, 6, 7, 8 and 9) and summarised in V2_DESIGN_LOG.
Sources: Rama's answers in docs/inputs/chatgpt_originals_and_replies/A-010_SECRETS_ENVIRONMENT_DISCUSSION_CHECKLIST_30-SEP-2026_RESHARED.txt (Q1 to Q36 and the additional note); his earlier A-006 (Q4, Q5) and A-009 (Q36) answers; ChatGPT's 30-Sep manual (section 18) and reconciliation (section 4); legacy evidence in A-010_manual.md; external pages checked 2026-10-08 (section 16).
Labels: RAMA = Rama's suggestion (quoted or closely paraphrased; his answers are suggestions only). OPEN = not decided. CLAUDE CONSIDERATION = Claude's input for later discussion, not a decision. LEGACY = what the old system did (A-010_manual.md), for comparison only, not a requirement. CURRENT EXTERNAL BROKER FACT / CURRENT EXTERNAL FACT = a current broker rule or vendor documentation with its check date.
Security: no credential values, account IDs, mailbox addresses or host addresses.

## 1. Order of intent (latest last)
- A-006 Q4, Q5 (24-Sep; VP-36, VP-34): keep the Zerodha token workflow subject to current rules, preferably headless with no manual step; on authentication failure: no new entries, keep protecting and reconciling open positions where broker access permits, alert at once on Telegram and email.
- A-009 Q36 (30-Sep): "maintain all system/broker/other secrets in .evn files only [No cross-function placement or Collision etc]".
- A-010 (09-Oct): Rama answered Q-29 with (b), a scripted login as in legacy (section 10), and clarified three legacy facts: secrets provisioned on the PC and pushed to the VM, separate alert and broker credentials on the testing VM, one account loaded into .env (A-010_manual.md sections 4, 12, 19).
- A-010 (30-Sep): Q20 (prior authorisation), Q21 (change handling), Q22 (copy protection), Q25 (credential bank), Q30, Q32 and Q33 (failure behaviour), Q34 (".env" is "raw information only, Not Decision"; better mechanisms welcome), Q35 (Zerodha only, live only), Q36 and the additional note (suggestions welcome).

## 2. Start V2 with fresh credentials (Q-11; VP-50)
- LEGACY (A-010_manual.md sections 11, 12, 17): the published history holds real values from April 2026 (secrets rotated 02-Jul; two API keys and two Telegram identifiers not); the production API key is in plain text in docs/web_claude at the public HEAD; the PC still held a .env, the GUI's local file, key copies and credential-bearing files on 18-Sep; whether the VM disks were deleted is unknown.
- CURRENT EXTERNAL BROKER FACT (Zerodha Kite Connect FAQ, checked 2026-10-08): "Deleting your app immediately invalidates your API key, secret, and all access tokens"; "Keep your API secret secure and do not share it". The FAQ does not describe regenerating a key or secret. Kite Connect Personal is free (orders, GTT, alerts, margins and portfolio; "No live market data or historical data"); the paid plan, which adds data, is Rs 500 a month per API key. This is current product information and may change; recheck it at M-005 once V2's data architecture is known.
- CLAUDE CONSIDERATIONS:
  - Treat every legacy credential value as exposed, pending owner-side revocation or rotation review. This is not a breach finding: the 14-Jul forensics found no breach within its scope, and whether any post-rotation value was exposed is UNKNOWN. Two API keys were not rotated on 02-Jul and their later status is unknown. V2 uses none of the legacy values and never copies the legacy .env.
  - Kite: create a new Kite Connect app for V2 (new API key and secret) and delete the legacy app once nothing depends on it; deletion is the documented way to invalidate the published key. Sequence the deletion so that nothing still depends on the app; the legacy system is dead, and the legacy audit (DR-016) uses broker records. The plan (Personal or paid) follows V2's market-data needs (C, D and E topics).
  - Telegram: revoke and reissue the bot token; review who can read the alert channels.
  - New webhook secret; revoke the SMTP app password and issue a new one; new GUI password and seed.
  - Zerodha account: reset the TOTP authenticator and change the password, because both were stored on the deleted VMs and the seed on the PC (Rama's call).
  - Other accounts: the four disabled accounts had key, secret and TOTP names in the legacy .env, two of them real personal accounts; revoking their API apps is for their owners (Rama to decide whether to raise it).
  - After revocation, remove the legacy secret files from the PC (they then hold dead values) and record what was removed, by name.
- OPEN: order of operations; timing relative to the legacy audit (DR-016), which should use broker records (contract notes, console reports), not the legacy app.

## 3. Where secrets live and how they are loaded (RAMA A-009 Q36, A-010 Q34; VP-51)
- RAMA (Q34, verbatim): "YES, But at the same if there is any improved mechanism or suggestions, You're welcome, So what I was mentioned is suggest an raw information only, Not Decision".
- LEGACY (A-010_manual.md sections 3, 5, 17): one .env per host read by three parsers (systemd, bash, python-dotenv); the whole file given to every cron job, the AI monitor and the agy subprocess; a second copy of the Telegram secrets in a systemd drop-in; PC and VM copies diverged.
- CURRENT EXTERNAL FACT (systemd documentation, checked 2026-10-08): "Note that environment variables are not suitable for passing secrets (such as passwords, key material, …) to service processes. Environment variables set for a unit are exposed to unprivileged clients via D-Bus IPC ... environment variables are propagated down the process tree ... Use LoadCredential=, LoadCredentialEncrypted= or SetCredentialEncrypted= ... to pass data to unit processes securely." Credentials are files "only accessible to the user associated with the unit", located through $CREDENTIALS_DIRECTORY.
- CLAUDE CONSIDERATIONS (options, not decided):
  - (a) One secrets file per environment, outside the repository, owner-only, read by one V2 loader (one parser, no shell sourcing, no drop-in copies); each process receives only the names it needs.
  - (b) systemd credentials on the Linux host: one file per secret, readable only by the service's user, optionally encrypted at rest; the V2 loader reads $CREDENTIALS_DIRECTORY. This serves systemd units only: a credential directory does not reach cron processes, so scheduled jobs need an explicit design (for example systemd timers with their own credential grants), and the deployed systemd version and each consumer's access are verified at M-001 / M-005.
  - (c) An OS or cloud secret store (Windows Credential Manager, as the legacy already used for the GitHub token, or a cloud vault on a cloud host); more moving parts; depends on Q-05.
  - Common to all: one source of truth per secret per environment; least privilege per process (AI, reporting and backup jobs get no broker secrets); no secret in configuration, the repository, logs, reports, prompts or chat; account IDs and mailbox addresses kept out of tracked files too (A-009_v2_design_inputs.md section 8 asked); an inventory of secret NAMES with purpose, consumers, owner and last rotation date (never values).
  - Claude's lean: (b) where the host supports it, otherwise (a). Both keep Rama's principle that secrets never mix with configuration (A-009 Q36). Decide with the host (Q-05, M-001, M-005).

## 4. Prior authorisation for sensitive changes (RAMA Q20; VP-52)
- RAMA (Q20, verbatim): "YES, I Got alerts & If possible in new system not only alerts do something like get prior authorisation or something else to make changes or Any other idea? - Note it carefully into Manual file".
- LEGACY (A-010_manual.md sections 13 to 15): an alert-only monitor, by Rama's earlier choice; a changed file alerted once and became the baseline; the only deliberate gates (copy token, SSH re-baseline) were self-issued by whoever held a session.
- CLAUDE CONSIDERATIONS:
  - Define "sensitive": secrets and accounts, service units and schedules, SSH keys and access rules, security configuration, risk caps.
  - Out-of-band approval: the change tool sends a one-time code by Telegram and email and applies the change only after the code is entered. On its own this stops nothing on a host where the login user can edit files directly, so it must come with section 6's permissions (secrets writable only through the tool's service account). Telegram and email can be unavailable or compromised, so the code cannot be the sole control; the approval architecture stays OPEN.
  - One route: sensitive changes only through the scripted deploy route with a change record (VP-46), never by hand on the host.
  - Windows: no secret or unit changes during market hours (the legacy runbooks already said "rotate while the service is DOWN" and "never the night before a trading day").
  - Detection stays as the second line: a change without a matching approval record is CRITICAL on both channels.
  - Break-glass: a tested emergency path (cloud console) so prior authorisation can never lock Rama out.
- OPEN: which changes need approval, the approval channel, the break-glass procedure.

## 5. Change handling for secrets (RAMA Q21; VP-52, VP-46)
- RAMA (Q21, verbatim): "YES, Sometimes directly edited the VM file using VS Code Claude, But I can't remember how it was handled, I guess by restart - Not sure, So in new system do some improvement means to 'Change handling'".
- LEGACY (A-010_manual.md sections 12, 13): hand edits; services saw a change only after a restart, cron jobs at once; a dated plain-text backup shredded by hand; credentials tested by hand (getMe).
- CLAUDE CONSIDERATIONS: one change command that (1) validates the format; (2) tests the new value where a safe test exists (Telegram getMe, an SMTP login, the Kite profile after a token exists) before switching; (3) writes atomically, owner-only, with no plain-text backup left behind; (4) records the change (secret name, time, reason, who, approval reference; never the value); (5) restarts only the consuming processes, at a safe time (broker credentials outside market hours or with no open position); (6) confirms after the restart that each process loaded the new value without printing it (for example by comparing a non-reversible marker that each process logs at start); (7) alerts on both channels.
- OPEN: the marker method, restart timing rules (A-011).

## 6. Copy protection and host access (RAMA Q22; VP-53)
- RAMA (Q22, verbatim): "YES, Since these were the sensitive credentials not to allow unauthorized into VM to Copy files into Other's PC intentionally, So at the same time all changes done in my local pc > VM push allowed, So any improvement in new system? - Note it carefully into Manual file".
- LEGACY (A-010_manual.md sections 15, 16): the gate blocked only VM-initiated copies; a pull started from a PC was detected after the event; the token was self-issued; the login user had passwordless sudo; SSH was open to the internet; the PC key had no passphrase.
- CLAUDE CONSIDERATIONS:
  - Protect the secret at rest instead of the copy tool: secrets readable only by a dedicated service user with no login shell; the login user has no passwordless sudo, so a pull of the secret file fails on permissions; keep a copy gate only for data files if wanted.
  - SSH only over a private network (Tailscale), after the phone path and a cloud-console path are tested (the legacy's 27-Jul blocker); a passphrase or a hardware key for the SSH key (14-Jul record C3, C8); an alert on every login (C4).
  - Audit reads of secret files where the host keeps auditd (legacy audited writes only).
  - No unauthenticated listener on a public interface (legacy health server C-3; rpcbind).
  - Keep the legacy's good practice: the GitHub token in the OS credential store, never in a file.
- OPEN: host design (M-001, M-007).

## 7. Credential bank and account metadata (RAMA Q25; VP-51)
- RAMA (Q25, verbatim): "YES, Correct only [YAML/config files has business settings, .env/secret is business execution credentials and .csv file has credential bank/storage (currently only broker details stored), Any improvements in new system? -Note it".
- LEGACY (A-010_manual.md sections 4, 12): a tracked account file with five account IDs and person-name labels; credential names for all five accounts in .env; a plain-text spreadsheet of rotated seeds; values copied by hand through a temporary file.
- CLAUDE CONSIDERATIONS:
  - V2 is Zerodha-only, live-only and single-account (VP-02; Q35): one account's credentials only.
  - The secret store is the credential bank; no plain-text CSV or spreadsheet of credentials anywhere.
  - Account metadata (ID, label) lives outside tracked files (the secret store or an untracked environment file); no person names.
  - At start, the broker profile's user ID must match the configured account (wrong-account guard; legacy checked only the token file's account ID).
  - Recovery material (backup codes, the authenticator's recovery) is kept in Rama's own password manager, outside the system.
- OPEN.

## 8. Start-up validation and missing-secret behaviour (RAMA Q30, Q32; VP-54, VP-47)
- RAMA (Q30, verbatim): "UNKNOWN, Why? Till now everything worked like getting email/telegram alert, broker order passed (api worked), Chartink sends signals (secret worked), So I'm not sure all are correct or there is error/issue which I unable to found, so any improved things into new system? - Note it carefully into Manual file". (Q32): email fallback UNKNOWN; all emails arrived.
- LEGACY (A-010_manual.md sections 6, 9): presence-only checks with two different lists; exit 3 or 6; a possible 10-second restart loop on exit 6; the token watcher's warning depended on the Telegram token; the notifier's email fallback was inert.
- CLAUDE CONSIDERATIONS:
  - One required-secret list per process, derived from what the process actually reads (no second hand-kept list).
  - Check presence and format, then liveness where it is cheap and sends nothing (Telegram getMe, an SMTP login without sending, the broker profile once a token exists).
  - Classes: BLOCK (broker credentials, webhook secret: trading does not start) and DEGRADED (one alert channel down: start, alert on the other channel, show the state in the GUI). VP-34 still sends every alert to both channels when both work.
  - The alert about a failed check never depends on the failing credential: Telegram down, email; email down, Telegram; both down, a local sentinel, the console and the GUI.
  - One alert listing every finding (VP-47); a dedicated "needs operator" exit code excluded from automatic restart, so no loop.
- OPEN.

## 9. Broker authentication failure (RAMA Q33, A-006 Q5; VP-36, VP-54)
- RAMA (Q33, verbatim): "UNKNOWN, Why? No such incidents was happened, Since all the trading days trades were executed, So not sure there is any issue or Fallback mechanisms exists or Alert triggers or Something else exits? - Take note in Manual for improved things into new system [Good Catch, appreciated]".
- LEGACY (A-010_manual.md section 8): SOFT_KILL on a genuine expiry, a HARD_KILL path after repeated auth failures (record), token invalidation at shutdown, an IP-refusal classifier; no re-login; with no token the system could not cancel software-OCO legs or place exits.
- CLAUDE CONSIDERATIONS (a written state machine, designed with A-011):
  - AUTH_OK to AUTH_LOST (token invalid): no new entries; alert both channels at once; a cheap read every short interval to detect recovery; no automatic kill that itself needs the API.
  - Positions while AUTH_LOST: no API call works, so protection would have to sit at the broker already. This is a requirement for G/H to resolve, not an established capability: whether every MIS position can carry a broker-side SL, and under which market-protection rules, depends on current Zerodha constraints (G/H); CNC protection by OCO GTT is covered in A-006 and A-007. It raises the priority of A-006 Q32 and VP-38.
  - AUTH_LOST to RECOVERING (a new token): reconcile with the broker before entries resume (K, A-011).
  - IP_REFUSED (token valid, orders refused because the IP is not registered): its own state; alert with the current IP; never invalidate the token (the legacy classifier is a sound base).
  - End of day without a token: MIS positions fall to the broker's auto square-off (charged; A-006 section 5); record it.
  - Every transition recorded with its reason (VP-11).
- Carried to: A-011 (states), G/H (protective orders), I (failure handling), K (reconciliation), L (alerts).

## 10. Daily login and token workflow (Q-29; VP-36)
- RAMA (A-006 Q4): fully headless with no manual step preferred. (A-010 Q10, Q11) the legacy was fully headless, with a manual fallback.
- RAMA (09-Oct, Q-29 answer, verbatim): "One decision for you: (b) A scripted login like the legacy one. Why? I may not immediately loging when system sends link (ie, Time out issue) or may be i forgot to login etc".
- ChatGPT (09-Oct review, points 1 and 13, before Rama's answer): "do not select (b) on the current evidence"; preferred (a) with a fail-closed fallback, or (c) / defer to M-005. Claude's first-pass lean was also (a).
- CURRENT EXTERNAL BROKER FACT: Zerodha staff guidance on the Kite Connect forum states "We have always informed clients not to automate logins as per the exchange regulations" and "It is mandatory by the exchange that a user has to login manually at least once in a day" (September 2022, checked 2026-10-08); two later replies say the same, the access token expires at 6 AM, a static IP is required for orders, and broker notices on the SEBI retail-algo framework require daily 2FA (A-006 section 9, checked 2026-10-06). Zerodha's summary of NSE's retail-algo circular lists "OAuth/2FA" for API access (Z-Connect, 6-May-2025, checked 2026-10-09). This is forum guidance and broker summaries, not the circular's text.
- LEGACY (A-010_manual.md section 7): a scripted TOTP login that stored the password, the seed and the API secret together on the VM; the legacy audits flagged it ("permanently collapses the account's 2FA to 1FA"; "The second factor is not a second factor if it lives on the same machine as the first").
- OPTIONS (from A-006_v2_design_inputs.md section 3):
  - (a) One manual Kite login each morning from a link sent to Rama's phone; the Kite redirect brings the request token to the V2 host, which exchanges it with the API secret; no entries until a valid token exists; V2 never stores the Zerodha password or the TOTP seed.
  - (b) A scripted login as in legacy: headless, but against Zerodha's stated guidance, and it needs the password and the seed stored with the API secret.
  - (c) An officially supported method, if one exists or is released (none is asserted here); checked at M-005.
- STATUS: Rama's choice is (b). It is recorded as his choice and stays PROPOSED (W-6; no register row), to be ratified at M-005 under two conditions:
  - (1) Rule check: the current official Zerodha and exchange rules (their text, not only forum guidance) are read at M-005. If they do not permit an automated login, (b) is not built (VP-36: do not automate around a broker or regulatory authentication restriction), and Rama chooses again between (a) and (c).
  - (2) Safeguards if (b) is built: the password and the TOTP seed live only on the live host (never on the PC or in the repository), owned by a service user, encrypted at rest where the host allows (VP-51), never in logs, prompts or chat (VP-55), with a written rotation procedure; every failed login alerts both channels, and a day without a valid token is not traded (fail-closed; VP-54); the manual login stays as the fallback.
- CLAUDE CONSIDERATION (for M-005; not reopening Rama's choice): his two concerns about (a) can be designed for. The Kite login link is a fixed URL built from the API key (the legacy zerodha_login.py builds it the same way), so the link itself does not time out; the one-time request token returned after login is "valid for a couple of minutes" (Kite Connect forum reply, November 2019, checked 2026-10-09) and is exchanged by the host automatically at the redirect, so the timeout falls on the host, not on Rama. A forgotten login can be met with repeated reminders on both channels from early morning and a cut-off after which the day is not traded.
- A registered static IP (section 13) is a separate requirement for placing orders; it does not satisfy or replace the daily-login requirement.

## 11. Redaction and leak prevention (VP-55)
- LEGACY (A-010_manual.md sections 7, 10, 11, 12): the bot token in debug logs for two weeks; a token prefix in a log; TOTP digits in a dry-run log; the API key inside instruction documents; secrets in URLs (Telegram's path, the webhook's query); a temporary plain-text file; snapshot redaction (M-K5) and a pre-commit scanner installed by hand.
- CLAUDE CONSIDERATIONS:
  - Logging: third-party HTTP loggers above DEBUG and a redaction filter on every handler for known secret values and shapes; never log prefixes or fragments; exception text is redacted before it goes to Telegram or email.
  - Storage: configuration snapshots keep the legacy redaction pattern and hold variable names only; reports and AI prompts receive no secrets; documents (instruction cards, Claude Code reports, design files) carry names only, since the legacy key leaked through instruction documents.
  - URLs: logs never record query strings or the Telegram request path; the Chartink token stays a query parameter only if Chartink still cannot sign (re-check at C).
  - Git: the pre-commit scanner installed by the setup script and verified, plus a full-history scan before any push to a remote (the legacy 02-Jul audit suggested gitleaks or detect-secrets); a published repository is assumed public forever. Scanning the current files is not enough for a history that was already published: rotation or revocation is the remediation, and rewriting history is not a substitute. The V2 repository itself is scanned, docs/inputs included: its immutable input files carry account identifiers (A-008 erratum E-2; A-010_review_notes.md section 8).
  - Entry: secrets typed into a prompt that writes straight to the store; no temporary plain-text files (Q24).
- OPEN.

## 12. Production and testing secrets (RAMA Q27, Q28; VP-56)
- RAMA (Q27): "NOPE, Every file is is in both VM's [Exception things as mentioned above like: S&R, Config.yaml settings, some bug fixed into testing/sandbox vm]". (Q28): never mixed, "Since both files same as it is".
- LEGACY (A-010_manual.md section 19): Rama (09-Oct) clarified that the testing VM already used different alert and broker API credentials, "to avoid collision between production alerts and same broker api"; the records agree (a different SMTP mailbox and account tag); the 18-Sep record also lists the production broker key on both VMs.
- CLAUDE CONSIDERATIONS: separate credentials per environment, same NAMES, different values; a testing environment never holds the production broker secret; separate alert channel (or bot) and webhook secret per environment; every alert names its environment; environment parity (project instructions 9.4) covers code and structure, never secret values. Whether any testing environment talks to the live broker at all depends on Q-01 and DR-001.
- OPEN.

## 13. Static IP and the host (Q-05; M-007)
- CURRENT EXTERNAL BROKER FACT (Kite Connect FAQ, checked 2026-10-08): a static IP is required to place orders through the Kite API; other endpoints work from any IP; up to two static IPs (one primary, one optional secondary); one change per calendar week. The FAQ's sentence still shows "from 1 April 2025", while A-006's dated trail (section 9, checked 2026-10-06) records the extension to 1-Apr-2026; the effective date is taken from A-006's trail, not from this sentence. Zerodha's summary of NSE's retail-algo circular (6-May-2025): individuals "only need a static IP dedicated to their API key".
- LEGACY: the VM's public IP was registered; the pre-flight alerted on an IP change; an IP refusal kept the token valid. Rama's home IPs were dynamic (legacy fail2ban note).
- CLAUDE CONSIDERATION: the live V2 host needs a static public IP registered with Kite. A home PC on a dynamic IP cannot place orders without one (an ISP static IP or a cloud host). This feeds Q-05 and VP-05. A static IP serves order placement only; it does not meet the daily-login requirement (section 10).

## 14. Zerodha only, live only (RAMA Q35)
- RAMA (Q35, verbatim): "YES, 100% Future system uses Zerodha only + Live Only [So No more: AngelOne + Paper mode]".
- Already reflected: DR-001 (live-only) and VP-02 (Zerodha only). LEGACY: the AngelOne adapter was a disabled stub (A-006). V2 carries no AngelOne credentials or code. A-010 records this for cross-reference only.

## 15. ChatGPT's 30-Sep V2 inputs (kept so nothing is lost)
- Manual section 18: secret manager or vault versus .env (section 3); least-privilege access (sections 3, 6); authorised and audited secret changes (sections 4, 5); rotation without plain-text temporary files (sections 5, 11); start-up validation and explicit degraded or fail-closed behaviour (section 8); safe reload and restart (section 5); redaction everywhere (section 11); the broker-auth failure state machine (section 9); an auditable configuration and security change history (section 5; VP-46).
- Reconciliation section 4: the same list plus "authorization before sensitive changes" (section 4), redaction "in logs/prompts/reports/diagnostics" (section 11), the Zerodha-only and live-only cross-reference (section 14), Chartink naming (A-008; VP-45) and the configuration boundary (A-009).

## 16. External references (checked 2026-10-08)
- systemd.exec documentation, the systemd project's man source (main branch): Environment=, EnvironmentFile= (precedence, read at start, failure on a missing file), the note that environment variables are not suitable for secrets, LoadCredential= and the related directives.
- systemd issue #9788 (EnvironmentFile= wins over Environment=; the maintainers keep the order).
- python-dotenv on PyPI (1.2.4): variable expansion, override=False by default.
- Zerodha Kite Connect FAQ (support.zerodha.com): static IP, app deletion invalidating key, secret and tokens, Kite Connect Personal, the paid plan.
- Kite Connect forum, "Kite API apps and TOTP requirments" (staff replies, September 2022) and "Introducing Kite Connect Personal APIs" (March 2025).
- Zerodha Z-Connect, "A comprehensive overview of NSE's circular on the new retail algo trading framework" (6-May-2025), checked 2026-10-09.
- Kite Connect forum, "login automation" (reply of November 2019 on request-token validity), checked 2026-10-09.
- A-006 section 18 references (checked 2026-10-06): token expiry, daily-login staff replies, static IP extension, SEBI retail-algo framework notices.

## 17. Status and boundary
- Everything here is PROPOSED or OPEN; a ChatGPT review alone does not turn any of it into a decision (W-6).
- New proposals in V2_DESIGN_LOG: VP-50 (fresh credentials), VP-51 (secret storage, loading and account metadata), VP-52 (prior authorisation and change handling), VP-53 (copy protection and host access), VP-54 (start-up validation and auth-failure behaviour), VP-55 (redaction and leak prevention), VP-56 (per-environment credentials). Extended: VP-05, VP-34, VP-36, VP-46, VP-47.
- Questions: Q-11 refined and still open (section 2); Q-29 answered by Rama on 09-Oct with (b), recorded as PROPOSED with two conditions for M-005 (section 10); Q-36 opened (A-010 audit leads); Q-33 waits for the A-011 answers.
- Carried to: M-001, M-005 and M-007 (host, token operation, static IP), A-011 (states and restart rules), G and H (protective orders), I (failure handling), K (reconciliation), L (alerts and evidence), B-001 and B-002 (boot gate), C (Chartink signing), the legacy audit (DR-016).
