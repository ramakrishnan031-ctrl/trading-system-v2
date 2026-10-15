# B-004 — Broker Connection and Authentication

**Status:** Draft for review — not frozen  
**Prepared:** 14-Oct-2026  
**Scope:** Dead legacy system; factual documentation only  
**Primary source commit:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
**Important:** The accompanying answer sheet was generated originally by Google Gemini and is treated as raw/probable input only. This manual does not authorize legacy operations or make V2 decisions.

## 1. Purpose and evidence labels

This chapter records the legacy broker boundary, session/token material, token-generation code, error translation, token monitoring and paper/live adapter split. It does not own broker-state reconciliation (B-005), worker/feed lifecycle (B-006), readiness (B-007), configuration loading (B-002), database lifecycle (B-003), shutdown/restart (B-011/B-012), or V2 token architecture (M-005).

| Label | Meaning |
|---|---|
| FACT (SOURCE-VERIFIED) | Directly inspected source/config at the named commit |
| FACT (RECORD-DERIVED) | Preserved dated record, not independently reproduced from a host |
| HISTORICAL (Rama / answer-sheet claim) | Recollection/Gemini-generated answer, not independently confirmed |
| INTERPRETATION / AUDIT LEAD | Plausible implication requiring more evidence |
| KNOWN DEFECT | Defect established against a documented contract |
| UNKNOWN / NOT ESTABLISHED | Evidence insufficient |
| CURRENT EXTERNAL FACT | Current vendor/regulatory fact, not proof of historical implementation |

Sensitive account identifiers, credential values, API secrets, TOTP seeds, access tokens and host IPs are intentionally omitted.

## 2. Source inventory

Production snapshot: [trading-system-prod-snapshot](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot), commit `970aabf3c78d777d9d34549e021f7c8e756c7bdb`.

| Path | Blob SHA at inspected commit | B-004 purpose |
|---|---|---|
| [`main.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/main.py) | `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | Mode, token gate, client/adapter wiring, auth failure handling |
| [`broker/zerodha_adapter.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/broker/zerodha_adapter.py) | `0c85d2d254aa5015bf98072ec5abf88c88cce133` | Adapter, API call boundary, exception translation, paper/live branch |
| [`broker/auth_recovery.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/broker/auth_recovery.py) | `d610cee16a1dee20ff6faee71db351e985418048` | Auth error classification and IP-related alert text |
| [`broker/token_monitor.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/broker/token_monitor.py) | `15102f2431e32f3900545a1094406313b919e07d` | Periodic profile check and expiry callback |
| [`scripts/zerodha_login.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/scripts/zerodha_login.py) | `ff6e8745e5877b33f4d445d0686f67866aea857e` | Browser-assisted login and token helpers |
| [`scripts/auto_refresh_token.py`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/scripts/auto_refresh_token.py) | `57c71f49d142b5fec7af787d30bd75caa286fa5a` | Headless TOTP token-generation flow |
| [`deploy/systemd/trading-system.service`](https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot/blob/970aabf3c78d777d9d34549e021f7c8e756c7bdb/deploy/systemd/trading-system.service) | `3d97c92caf363da0f98d1bec693e3c21631eeb5c` | Checked-in service configuration |
| `.gitignore` | `b9b49fc3fd7dfc81abfd44c3dc17b4a64bfc7813` | Excludes logs and credential-related files |
| `requirements.txt` | `3b26e2f5ebe5616edd52720d07d7b9aa8483703c` | Pins `kiteconnect==5.1.0` |

Other repositories:
- Testing/sandbox: https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot
- Records: https://github.com/ramakrishnan031-ctrl/trading-system-records
- Records manifest: https://github.com/ramakrishnan031-ctrl/trading-system-records/blob/main/SNAPSHOT_MANIFEST.md
- Frozen A-006: https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/agenda/a-006_broker-model/A-006_manual.md
- Frozen A-010: https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/agenda/a-010_secrets-environment/A-010_manual.md
- V2 design log: https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/governance/V2_DESIGN_LOG.md
- V2 decision register: https://github.com/ramakrishnan031-ctrl/trading-system-v2/blob/main/docs/governance/V2_DECISION_REGISTER.md

The records manifest describes a multi-pass snapshot and warns that some files were not refreshed later. It is not proof of final host state. A checked-in unit is not proof of installed host state. The former production/testing runtime must be described as record-derived or unknown unless preserved evidence supports the claim.

## 3. Broker identity and adapter boundary

**FACT (SOURCE-VERIFIED; cross-reference A-006):** The inspected code wraps Zerodha/Kite Connect. The adapter module describes `ZerodhaAdapter` as a typed wrapper around an injected Kite client, constructed once and injected into consumers. It defines internal result records for order, history, position, margin and quote operations.

| Lifecycle stage | Evidence-based description |
|---|---|
| Source | `broker/zerodha_adapter.py`, module header and `ZerodhaAdapter` |
| Input | Injected Kite client or paper-mode `None`/mock; rate limiter; product resolver; cost calculator; state machine; logger; optional paper dependencies |
| Logic | Validate input; map internal order/product fields; apply rate limiter on live path; call the relevant broker method; translate SDK exceptions |
| Guard | Request validation and product resolution; broker errors translated to internal exception classes |
| Output | Typed result or translated `BrokerError` subclass |
| Handoff | Order placement, monitoring and reconciliation consumers; exact caller behavior belongs to those call paths |

A-006 is frozen and owns the broker model. This chapter must not reopen whether another broker was configured as fallback.

**UNKNOWN / NOT ESTABLISHED:** Whether all production runs used the same account/app configuration, whether sandbox and production shared any credential category, or whether a token was ever reused across environments. Reconcile with frozen A-010 without reproducing identifiers or secret values.

## 4. Token file and credential lifecycle

### 4.1 Canonical token file

**FACT (SOURCE-VERIFIED):** `scripts/zerodha_login.py` defines the default token path as `data_store/session/zerodha_token.json`.

- `load_token(token_path)` parses JSON and returns `None` on OS/JSON parse errors.
- `is_token_valid(account_id, token_path)` checks loadability, account identifier match, current IST date, and non-empty access token.
- `save_token(...)` writes account ID, broker, access token, API key, date, saved timestamp and expiry timestamp. It attempts restrictive POSIX directory/file permissions.

**Limit:** This is a local file/metadata gate. It does not establish that the broker will accept the token or that network connectivity is available.

### 4.2 Credential sources

**FACT (SOURCE-VERIFIED):** `scripts/auto_refresh_token.py` describes account-specific environment-variable lookup via `config/accounts.csv`, with generic fallbacks. The checked-in systemd unit references an `.env` file. No values are reproduced here.

**UNKNOWN / NOT ESTABLISHED:** Which variables were actually populated on each former host, whether they were present for every historical run, and whether any credential category was shared across environments. Templates and source comments do not establish deployed values.

## 5. Token-generation routes

### 5.1 Browser-assisted route

**FACT (SOURCE-VERIFIED):** `scripts/zerodha_login.py` contains a browser-assisted login flow, request-token exchange and token-save helpers.

### 5.2 Headless route

**FACT (SOURCE-VERIFIED):** `scripts/auto_refresh_token.py` describes this sequence:

1. Resolve account and credential variable names from the account registry.
2. Submit login credentials.
3. Generate TOTP and submit two-factor authentication.
4. Follow the connect-login redirect to obtain a request token.
5. Exchange the request token for an access token.
6. Save through the shared token helper.

The script header describes a weekday 08:00 IST schedule after a 05:00 cleanup. This is source-comment/config intent, not proof of actual cron installation or successful historical execution. Verify the cron registry and dated host captures.

### 5.3 Mid-session refresh

**FACT (SOURCE-VERIFIED):** `broker/token_monitor.py` says it does not automatically re-login or refresh the token.

**UNKNOWN / NOT ESTABLISHED:** Whether another live path attempted autonomous re-login, whether operator intervention was always required, and when the workflow changed. The answer-sheet descriptions remain historical claims until corroborated.

## 6. Startup and connectivity probe

The answer sheet asserts a synchronous `kite.profile()` or `kite.margins()` probe before workers start, with a fail-fast exit if it fails. The reviewed source excerpts do not establish that exact sequence.

**FACT (SOURCE-VERIFIED):** In `main.py`, paper mode sets `kite_client=None`. In live mode, the visible setup reads `data_store/session/zerodha_token.json`, aliases the selected account's API-key variable and access token into the environment, and builds the Kite client. Missing token file or missing credential environment variable takes a visible return-code-6 path. The adapter is then constructed.

`is_token_valid` checks local token metadata; it does not perform a remote probe. `TokenMonitor` makes profile calls periodically, but that does not prove a startup probe.

**UNKNOWN / NOT ESTABLISHED pending full call trace:**
- whether an explicit remote probe occurs during startup;
- exact first remote method;
- ordering relative to startup reconciliation, feed and worker start;
- exception handler/exit code for that specific call;
- whether an alert is sent before termination.

Trace `main.py` from CLI/mode selection through token gate, client/adapter construction, reconciliation, worker/feed startup and monitor startup before finalizing the chronological startup description.

## 7. Broker error translation

**FACT (SOURCE-VERIFIED):** `broker/zerodha_adapter.py::_translate_kite_exception` maps:

| Exception | Internal class | Caution |
|---|---|---|
| `TokenException` | `BrokerAuthError` | Token/session failure |
| `NetworkException` | `BrokerTimeoutError` | Transport/network failure |
| `InputException` / `OrderException` | `OrderRejectedError` | Request/order rejection |
| `PermissionException` | `BrokerAuthError` | Permission failure is not automatically token expiry |
| `TimeoutError` / `socket.timeout` | `BrokerTimeoutError` | Timeout |
| `GeneralException` / unknown | `BrokerError` | General or wrapped failure |

A generic 403 or permission failure must not automatically be described as expired token.

### 7.1 IP/permission classifier

**FACT (SOURCE-VERIFIED):** `broker/auth_recovery.py::classify_broker_auth_error` classifies message text into `IP_NOT_ALLOWLISTED`, `TOKEN_EXPIRED` or `AUTH_UNKNOWN`. The module also builds an IP-related alert body.

This contradicts the answer sheet's broad claim that the codebase had no IP-allowlist handling. It does not prove that a real IP-related incident occurred, nor establish the applicable external policy at every historical date. Code handling, current vendor rules and historical incidents must remain separate evidence categories.

## 8. Retry and rate-limit boundary

**FACT (SOURCE-VERIFIED):** The adapter module says it does not retry failed calls. Its documented HTTP 429 path penalizes/freezes a rate-limit bucket and raises the error; the documented caller owns retry.

**UNKNOWN / NOT ESTABLISHED:** Whether all read callers retry, whether every operation is retried, or whether behavior changed over time. Map each caller separately. Adapter-level no-retry does not prove system-wide no-retry.

## 9. Token monitor

### 9.1 Source behavior

**FACT (SOURCE-VERIFIED):** `broker/token_monitor.py` documents:
- a daemon polling thread;
- default interval of 1800 seconds (30 minutes);
- a supplied profile function;
- optional market-hours-only checking;
- paper-mode `start()` as a no-op and `check_now()` as true;
- classification of genuine expiry separately from transient errors;
- a once-per-expiry-episode latch that is re-armed after a successful later check;
- CRITICAL logging/notification and invocation of an injected expiry callback;
- no automatic token refresh, direct position closing, hard-kill call or process exit.

### 9.2 Operational consequence

The module describes the callback as a soft-kill so the order monitor can continue managing already accepted broker-resident SL/TGT orders. This is source-level intent. It does not prove that a new exit order can be submitted after authentication fails, or guarantee that any broker-resident order will execute.

Trace main.py callback wiring, entry gating, order-monitor behavior and shutdown handling across B-004/B-005/B-006/B-012 before making an end-to-end statement.

## 10. Paper versus live

### 10.1 Live

**FACT (SOURCE-VERIFIED):** The live branch in `main.py` loads account-specific API-key material and the canonical token file, then builds a real Kite client. The checked-in service unit launches `main.py --mode live`.

### 10.2 Paper adapter

**FACT (SOURCE-VERIFIED):** The paper branch sets `kite_client=None` and passes `paper_mode=True` to `ZerodhaAdapter`. The adapter's module header says paper mode simulates adapter calls and never touches the Kite client. Paper order operations use the local simulation branch.

However, `main.py` also injects a quote provider built by `_make_paper_quote_provider()`. Trace this provider before concluding that paper mode never reads token material or makes external quote calls. The broad answer-sheet claim that the adapter itself instantiates a real KiteConnect/KiteTicker client in paper mode conflicts with the source's explicit `kite_client=None` and no-Kite adapter contract.

### 10.3 Unverified paper claims

The answer sheet claims that paper mode uses live quotes, a particular paper database, bypasses broker margins/reconciliation and suppresses most authentication alerts. These details need direct source tracing and B-003 cross-check. Do not infer all alert/recovery behavior from the token monitor's paper-mode no-op.

## 11. Authentication failure and continuity

Do not treat “application connected” as one binary fact. Separate:

1. whether a particular REST request succeeds;
2. whether the local process/workers remain active;
3. whether broker-resident accepted orders remain at the broker;
4. whether new entries or exits can be submitted;
5. whether local storage, logs and reports continue.

**FACT (SOURCE-VERIFIED):** The adapter translates error types distinctly; the token monitor invokes a callback rather than directly exiting. The exact end-to-end behavior depends on caller handling and callback wiring.

**HISTORICAL (answer-sheet claim, not independently corroborated here):** stale-token boot failures, mobile/web login invalidation, systemd crash loops, TOTP clock drift, market-open rate-limit problems, WebSocket/REST divergence and token exposure in earlier logs. Search dated records and Git history. Until corroborated, do not label these incidents “verified.”

## 12. Logs and sensitive material

The answer sheet lists candidate log filenames and token-cache names. Do not accept them as existing without source or record evidence.

**FACT (SOURCE-VERIFIED):**
- The checked-in systemd unit sends stdout/stderr to the journal.
- `.gitignore` excludes `logs/` and `*.log`.
- Token helper source defines the canonical token file.
- Token JSON contains sensitive access-token/API-key material.

**UNKNOWN / NOT ESTABLISHED:** Exact historical log names, presence of production journal exports, whether an incident log survives, whether token values/request-token parameters were ever logged in production, and when any redaction changes became active.

Never reproduce secret values even if found in a historical artifact. If exposure is corroborated, record only the category, file/path, time window and evidence reference.

## 13. Systemd and scheduler boundary

**FACT (SOURCE-VERIFIED):** `deploy/systemd/trading-system.service` contains a working directory, an EnvironmentFile path, `ExecStart` with `main.py --mode live`, `Restart=on-failure`, `RestartSec=10`, `RestartPreventExitStatus=3 4 5`, and journal output.

This is a checked-in unit, not proof of the former installed host unit. The answer sheet's claim that a token failure caused a restart flood ending in `start-limit-hit` remains a historical claim unless a dated journal/incident artifact corroborates it. The actual cron registry, cleanup schedule, token refresh execution and successful run history must be checked independently.

## 14. Production versus sandbox

The sandbox repository is a separate evidence source. Similar file paths or matching blobs in selected files do not prove matching commits, credentials, account identities, installed units, cron jobs, databases or runtime behavior.

For every claimed difference, record exact repository, commit, path and blob. Keep values redacted. The records snapshot is not a substitute for host state; its manifest says it was assembled in multiple passes.

## 15. Partial lifecycle map

This is a partial source map, not yet a complete chronology:

```text
main.py CLI / mode
       |
       +-- PAPER: kite_client=None; paper_mode=True
       |           quote_provider injected separately
       |
       +-- LIVE: account/credential env alias
                   read canonical token JSON
                   missing file/env key -> return 6
                   build Kite client
                         |
                         v
                  construct ZerodhaAdapter
                         |
                +--------+---------+
                |                  |
          adapter calls       token monitor
          translate errors    periodic profile check
                |                  |
                v                  v
           caller/order flow   alert + callback
```

The order of startup reconciliation, feed/workers and monitor must be established from a complete `main.py` call trace before treating this as an end-to-end startup diagram.

## 16. Failure matrix

| Scenario | Source-level behavior established | Historical occurrence |
|---|---|---|
| Missing token file in live setup | Critical log, close store, return code 6 in visible branch | Not established |
| Missing credential env variable | Critical log, close store, return code 6 in visible branch | Not established |
| Malformed/unreadable token JSON | loader returns None; local validity check fails | Not established |
| TokenException from adapter | translated to BrokerAuthError | Not established |
| NetworkException / timeout | translated to BrokerTimeoutError | Not established |
| PermissionException | translated to BrokerAuthError; not automatically expiry | Not established |
| HTTP 429 | bucket penalty then raise; caller owns retry | Not established |
| Periodic token expiry | token monitor logs/alerts and invokes callback | Not established |
| Paper token monitor | no-op start; check returns true | Source behavior; historical use unknown |
| IP allowlist rejection | classifier/alert helpers exist | Not established |
| Systemd restart loop | service file alone does not prove incident | Answer-sheet claim only |
| Token leakage in logs | not proven by current source alone | Answer-sheet claim only |
| WebSocket survives REST auth failure | not established as historical event | Unknown |

## 17. Open questions and required trace

1. **Startup call graph:** exact first remote API call, if any, and its position relative to reconciliation and worker start.
2. **Client construction:** exact `KiteConnect` construction and access-token assignment; distinguish construction from remote validation.
3. **Paper quote provider:** API calls, token source, separate client, timeout and error behavior.
4. **Auth callback:** trace BrokerAuthError through main.py flags, token invalidation, soft-kill and shutdown.
5. **Entry/exit behavior:** identify which callers block/retry and distinguish broker-resident orders from new requests.
6. **Market data:** locate actual KiteTicker/subscription implementation. The answer-sheet path `workers/market_data_worker.py` returned 404 at the inspected commit; find the real path before describing watchdog, reconnect or chunking.
7. **Scheduler:** compare script comments with cron registry and dated host capture.
8. **Credential sharing:** reconcile with frozen A-010 without exposing values or silently editing the frozen record.
9. **Historical incidents:** find dated sanitized evidence for stale token, restarts, session invalidation, clock drift, rate limits and possible credential exposure.
10. **Environment parity:** compare production/sandbox at identified commits; no inference about host/account parity.

## 18. Cross-references and governance

- A-006 — broker model (frozen).
- A-010 — secrets/environment (frozen).
- B-001 — VM/service startup.
- B-002 — configuration loading.
- B-003 — database/state initialization.
- B-005 — startup reconciliation.
- B-006 — worker/feed initialization.
- B-007 — readiness/health.
- B-011/B-012 — shutdown and restart/recovery.
- M-005 — V2 token/login workflow discussion.

Any V2 implication remains **PROPOSED / OPEN** in its designated input file. Do not move it to ACCEPTED/FROZEN or alter the decision register. Do not amend frozen A-006/A-010/B-001/B-002/B-003 in place; if new evidence requires correction, propose a separately identified erratum/cross-reference.

## 19. Current conclusion

**Status: Draft — source trace incomplete; not ready to freeze.**

The source supports a Zerodha adapter boundary, a canonical JSON token file and local metadata checks, browser-assisted and headless token-generation code, differentiated exception translation, a periodic token monitor, a caller-owned retry boundary, and an explicit paper/live adapter split.

Important answer-sheet statements remain unverified or contradicted in broad form: the presumed startup profile/margins probe, paper mode's alleged use of a real Kite client within the adapter, the assertion that no IP-allowlist handling existed, and specific historical incidents. The final review must narrow claims to what source proves and leave runtime events unconfirmed unless dated records support them.

No legacy code/configuration or host/broker operation was changed. Nothing has been frozen, committed or pushed.

---
**END OF B-004 MANUAL DRAFT**
