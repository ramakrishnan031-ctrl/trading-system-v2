# Interim Independent Technical Audit Report
## Legacy Production Trading System

**Audit date:** 31 October 2026  
**Repository:** `ramakrishnan031-ctrl/trading-system-prod-snapshot`  
**Audited baseline reported by the audit team:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb`  
**Branch reported:** `claude/amazing-goodall-1x49v7`  
**Report status:** **INTERIM — NOT A COMPLETE SYSTEM AUDIT**  
**Evidence basis:** Claude Code audit activity log supplied by the user (`output(20261031-171559.txt`)

---

## 1. Executive assessment

The work recorded in the supplied activity log is materially more substantial than the earlier 21-file planning, inventory, and architecture groundwork. It contains completed static-review work from three domain auditors—data integrity/persistence, security/operations, and configuration/tests/history—plus a lead auditor's money-path review.

The recorded work has surfaced several material control weaknesses, including:
- a configuration-revert utility that can loosen the configured daily-loss limit from 3% to 5% when run as documented;
- a public-interface webhook configuration that accepts a static URL bearer token while binding to all interfaces, with actual external reachability dependent on live firewall state;
- a root-running token watcher that consumes scripts, a virtual environment, and an environment file writable by the unprivileged account;
- same-disk-only backups, leaving the financial record exposed to a VM/disk-loss single point of failure;
- data-retention behavior that conflicts with the ledger's stated immutable/audit purpose;
- silent failure in trade-journal MFE enrichment due to a query against a column absent from the schema;
- gaps in reservation attribution, entry slippage guarding, margin authority, and paper-mode reconciliation coverage.

These are serious audit leads and, where the log records independent spot-verification, they are stronger than unverified suspicions. However, this document is **not the final audit opinion**. The signal/screening auditor (DA1) had not yet reported in the supplied log, the Topic-02 architecture workflow was incomplete, several large operational scripts were only partially inspected, and live VM/database/firewall state was not examined. The supplied material therefore does not support a claim that the entire system has been audited or that the listed findings are exhaustive.

**Overall interim conclusion:** The snapshot contains multiple confirmed source-level weaknesses requiring controlled remediation planning. The most urgent areas are webhook exposure/authentication, the unsafe configuration-revert utility, root/unprivileged execution boundaries, backup isolation, and the integrity of capital/risk controls. No source-code changes, live operations, database connections, or V2-repository activity are reported in the supplied log.

## 2. Scope, method and limitations

### 2.1 Scope evidenced in the log

The work covered:
1. Money path: reservations, per-strategy caps, entry-price drift/slippage checks, fill handling, trailing-stop wiring, margin authority, paper-mode reconciliation, and selected risk controls.
2. Data integrity and persistence: schema version, table writers/readers, retention, backups, trade-journal enrichment, timestamps, candle persistence, transactions, and recovery controls.
3. Security and operations: webhook authentication, process privileges, credential storage, secret scanning, dashboard/control-plane behavior, logging, alerts, service and cron configuration, and operational scripts.
4. Configuration, tests and local Git history: TEMP configuration governance, revert tooling, validator wiring, test runner behavior, test quality, loader defaults, sizing configuration, and the available commit history.

### 2.2 Method

The supplied log describes static, read-only source inspection, exact file/line checks, targeted source verification of selected subagent claims, schema/code comparison, call-site checks, and local-history inspection. It reports no application execution, database connection, network call, or live-system operation.

### 2.3 Important limitations

- This is an audit of a repository snapshot, not verification of the currently deployed VM, installed firewall, live credentials, installed crontab, database contents, broker state, or current production behavior.
- The baseline's available Git history is shallow/limited. Earlier history and some claimed production changes cannot be independently reconstructed from the snapshot.
- The signal/screening domain audit (DA1) is not present in the supplied activity log.
- The Topic-02 architecture workflow is reported as incomplete.
- Several large operational scripts were outlined and spot-read rather than line-audited.
- Some documents and paths identified as potentially relevant (including large ledger/path documents) were not fully read.
- The report does not include the complete DA2/DA3/DA4 JSON or Markdown finding files. For those auditors, the supplied log provides summary counts and selected findings, not every finding's full evidence record.
- Runtime-dependent conclusions remain unverified where they depend on live firewall, service, sudoers, environment, timezone, installed hook, database, or credential state.

## 3. Priority findings

The following findings are the highest-impact issues explicitly described in the supplied log. Severity and confidence labels below preserve the labels reported by the audit work; they are not a fresh independent source-code verification by the author of this interim report.

### F-01 — Configuration revert utility can loosen the daily-loss limit

**Reported severity:** High  
**Reported confidence:** Confirmed  
**Audit source:** DA4-02

The utility `scripts/revert_temp_config.py` checks for the substring `"TEMP"` in file content. The word `ATTEMPT` in `config/system_config.yaml:407` satisfies that test, creating a false positive. The revert map reportedly sets `risk.daily_loss_limit_pct` to `0.05`, while the current YAML value is `0.03` and is marked permanent.

The documented `--apply --confirm` operation would therefore change the daily-loss limit from 3% to 5% and add false “REVERTED from TEMP” comments to other production lines. The lead auditor reports spot-verifying the substring match, the revert-map value, and the live YAML value.

**Current-state distinction:** The log says the YAML is currently at 3%, which is the safer setting. The confirmed hazard is the revert tool and its documented operation; the log does not say that the unsafe operation was executed.

**Implication:** Do not run this utility with `--apply --confirm` on the basis of its current documentation. This is a source-level hazard, not evidence that the live limit has already been loosened.

### F-02 — Webhook authentication configuration relies on a static URL token

**Reported severity:** High  
**Reported confidence:** Confirmed at source level; actual external reachability remains dependent on live firewall state  
**Audit source:** DA3-01

The supplied log records:
- `require_hmac: false` in `system_config.yaml:331`;
- webhook binding to `0.0.0.0:5000` at `system_config.yaml:328,335`;
- the token-query authentication path being accepted when HMAC is disabled;
- deployment firewall setup opening port 5000 in `deploy/install_vm_services.sh:33-47`;
- deployment documentation identifying TLS/nginx lockdown as unfinished.

The configuration comment reportedly says the all-interface bind relies on HMAC plus firewall protection, but the configured HMAC requirement is false. The auditor resolved an earlier apparent contradiction by checking `_authenticate`: the URL-bearer path is accepted precisely in this configuration.

**Implication:** This is a high-priority security exposure in the source configuration. Whether the endpoint is reachable from the public internet on the currently deployed VM cannot be concluded without live firewall/network evidence. The log recommends treating it as a gate before increasing live capital.

### F-03 — Root-running token watcher crosses the unprivileged-user boundary

**Reported severity:** High  
**Reported confidence:** Confirmed  
**Audit source:** DA3-03

The token-watcher service runs as root but executes a shell script and Python virtual-environment components writable by the `ubuntu` account and sources an `ubuntu`-owned `.env` file (`deploy/token_watcher.sh:53-89`). The log also cites a source assertion that `ubuntu` has passwordless sudo (`preflight/checks/services.py:13`).

**Implication:** The privilege separation is ineffective as described: compromise or modification capability under the unprivileged account may become root-level code execution through the periodic root service. The exact live permissions and sudoers configuration still require VM verification.

### F-04 — Broker takeover credentials are concentrated in one environment file

**Reported severity:** Medium  
**Reported confidence:** Confirmed in the snapshot  
**Audit source:** DA3-05

The headless 08:15 login flow reportedly keeps `user_id`, password, TOTP seed, API key, and API secret together in a `.env` file on both VM and PC (`auto_refresh_token.py:22-30,348-358`). The auditor's concern is that access to one file can defeat the practical protection expected from the second factor.

Related finding DA3-06 says the pre-commit secret scanner misses some password formats and has no PEM rule, while server-side scanning is not armed.

**Implication:** Credential concentration and incomplete secret detection create a material account-compromise risk. No actual credential values are reproduced in this report.

### F-05 — Backups remain on the same VM disk

**Reported severity:** High  
**Reported confidence:** Confirmed  
**Audit source:** DA2-01

The backup evidence script reportedly states that backup artifacts are not copied off the machine. Nightly SQLite backups of both databases therefore remain on the same VM disk.

**Implication:** VM or disk loss can destroy both the primary financial record and its backup copies. Retention and monthly restore drills do not remove this single point of failure.

### F-06 — Ledger retention conflicts with the stated immutable audit requirement

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** DA2-06

The retention job reportedly deletes `fm_ledger` rows older than 365 days despite the schema's stated “NEVER UPDATE OR DELETE” rule (`schema.sql:505-506`). The rehydration path skips an open trade if its ledger chain is missing (`fund_manager.py:2022-2031`). Separately, `screener_results` reportedly expire after 90 days without a trade-linked retention guard.

**Implication:** Historical financial/audit evidence may be removed while related trade records remain, weakening later reconstruction and auditability.

### F-07 — Trade-journal MFE enrichment queries a nonexistent column and suppresses the error

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** DA2-04

The schema for `trade_excursions` reportedly contains `updated_at` but not `created_at` (`schema.sql:824-834`). The trade-journal script queries `created_at` (`scripts/trade_journal.py:82-83`) and catches the exception with a bare `except: pass` (`:89`).

**Implication:** The MFE enrichment is reported to silently capture nothing on the scheduled 16:10 run. This is a data-quality/reporting defect, not proof that the underlying trade-excursion records are absent.

### F-08 — Reservation attribution is lost, weakening per-strategy cap accounting

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** Lead auditor money-path review

The lead auditor reports that `release_slm_buffer` and `top_up_reservation` rebuild the frozen `_Reservation` object without preserving its `strategy` field. The per-strategy counter filters on that tag (`fund_manager` line 1589), so rebuilt reservations are not attributed to the original strategy. The ledger restore path also reportedly drops strategy attribution because the ledger has no corresponding column.

The auditor qualifies the impact: the cap relies on a database floor, so the undercount window is narrow; `release_slm_buffer` reportedly has no production callers and silently holds its buffer until commit.

**Implication:** Strategy-level reservation accounting can become incomplete on affected paths. The full practical impact depends on the actual call paths and runtime concurrency.

### F-09 — Entry slippage guard has a fail-open race path

**Reported severity:** Medium  
**Reported confidence:** Confirmed, conditional trigger  
**Audit source:** Lead auditor money-path review

The lead auditor traced the FIX-075 drift path and found that the intended chase cap is 22% of stop-loss distance. However, if the LTP fetch is skipped at the guard and succeeds at the later drift check, the chase can become unbounded. The log narrows the trigger to a quote-fetch race or a falsy webhook trigger price; all three `place()` call sites reportedly pass the trigger.

**Implication:** A race/fallback path may bypass the intended entry-price bound. This is a conditional defect, not evidence that every entry is unbounded.

### F-10 — Two competing margin authorities can diverge

**Reported severity:** Medium  
**Reported confidence:** Confirmed as a design/source inconsistency  
**Audit source:** Lead auditor money-path review

The sizer reportedly uses live broker margin while the fund manager reserves against a static leverage map.

**Implication:** Order sizing and internal capital reservation may use different margin assumptions, creating reconciliation drift or incorrect available-capital calculations when broker margin differs from the static map.

### F-11 — Paper mode does not exercise key capital/margin reconciliation checks

**Reported severity:** Medium  
**Reported confidence:** Confirmed in the inspected source path  
**Audit source:** Lead auditor money-path review

A G3 docstring reportedly states that paper mode skips both capital reconciliation checks.

**Implication:** A paper-to-live transition may be the first time those checks are exercised against real trading state. The log identifies this as a migration risk; it does not quantify the likelihood of a live failure.

### F-12 — Trailing-stop feature appears unwired in production

**Reported severity:** Low  
**Reported confidence:** Confirmed wiring gap; practical impact reduced by configuration  
**Audit source:** Lead auditor money-path review

The breakeven condition reportedly cannot pass because `_FillEntry` lacks `strategy_obj` under `__slots__`, `fill_entry.strategy_obj` is not set in `place()`, and `main.py` does not build the managers. The log also says no strategy configuration enables trailing SL.

**Implication:** The FIX-132 trailing-stop feature appears inactive in production wiring. The auditor downgraded the severity because no strategy configuration currently enables it.

### F-13 — Misleading test runner can collect crash-injection scripts

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** DA4-07

The only committed `run_tests.py` reportedly runs `pytest tests/ --tb=no`, which collects four crash-injection scripts under `tests/crash_test` because they are named `test_*.py`. Those scripts are described as database-writing crash-injection programs. The intended unit/integration command is documented elsewhere, not in the committed runner.

**Implication:** The repository's apparent standard test entry point may execute hazardous test utilities or provide an incorrect view of test coverage. The audit log explicitly says these scripts were not executed.

### F-14 — Configuration validator is not wired into production callers

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** DA4-04

`config_validator.get()` reportedly has zero production call sites; the main path calls `validate_all(strict=False)`, and the auditor says a drift check is structurally unreachable. The log identifies other active controls—Pydantic validation and a config-auditor blocking path—as compensating controls.

**Implication:** The standalone validator's passing unit tests do not demonstrate that its checks protect production. The effective production control path must be documented around the validators that are actually wired.

### F-15 — TEMP configuration governance records disagree

**Reported severity:** High  
**Reported confidence:** Confirmed  
**Audit source:** DA4-01

The TEMP tracker and `AGENTS.md` reportedly claim 8 and 7 active relaxations, respectively, while the current YAML contains zero active relaxations under the auditor's classification, apart from a `min_score: 0` value that falls back to the global score. The revert script's production value also conflicts with the permanent YAML value.

**Implication:** The operator-facing governance record is stale and internally contradictory. More importantly, the unsafe revert utility in F-01 acts on these inconsistent assumptions.

### F-16 — Audit tables and dashboard readers are disconnected from writers

**Reported severity:** Medium  
**Reported confidence:** Confirmed  
**Audit source:** DA2-08

The auditor reports that `telegram_alerts`, `preflight_runs`, `_check_results`, `_autofix_log`, and `daily_symbol_stats` have readers but no effective writers in the inspected source. The documented producer for `daily_symbol_stats` reportedly does not exist.

**Implication:** Dashboard panels may present permanently empty datasets as if they were operational measurements. This needs explicit “not populated/not available” semantics rather than being mistaken for a clean state.

### F-17 — Operational diagnostics can cancel orders without broker cancellation or capital release

**Reported severity:** Medium  
**Reported confidence:** Confirmed in source; live invocation unknown  
**Audit source:** DA2-07

`scripts/check_vm_state.py` is described as read-only diagnostics, but its cleanup options reportedly raw-cancel database trades/orders without a broker cancellation and without releasing reserved capital. The same script reportedly compares IST text timestamps with SQLite UTC `datetime('now')`.

**Implication:** An operator may believe the script is diagnostic while its cleanup flags can alter internal state and strand reserved margin. The time comparison may also misclassify health state.

### F-18 — Log rotation configuration can remove logs daily and handler assumptions are inconsistent

**Reported severity:** Medium  
**Reported confidence:** Confirmed configuration/source mismatch; installation state unknown  
**Audit source:** DA3-09

The log reports a `logrotate` rule with `daily` and `rotate 0`, while the application uses a plain `FileHandler` rather than the assumed rotating handler.

**Implication:** If the reported rule is installed as written, logs may be removed daily without retained rotations, and application-side size-based rotation is not occurring. Actual installed state must be checked on the VM.

## 4. Other material observations

### 4.1 Data and schema

- The code reportedly expects schema version 45; `AGENTS.md` says version 24, which is 21 versions stale.
- `migrate_to_latest` is reported as a dead stub with zero callers.
- `daily_symbol_stats` DAO is reportedly unwired and its documented producer script is absent.
- The `trades` table has many write sites across multiple files, increasing the importance of consistent transaction and state-transition contracts.
- Multiple timestamp dialects (naive IST and ISO with `+05:30`) remain. Some readers reportedly retain UTC-shift behavior despite a documented fix.
- A candle backfill token value of zero can collide with a UNIQUE constraint and silently drop unmapped-symbol candles.
- The data auditor also recorded positive controls: write-ahead capital ledger, WAL, `BEGIN IMMEDIATE`, foreign keys enabled, synchronous FULL, explicit transactions, compensating recovery, and exactly-once release mechanisms. These controls should be preserved and regression-tested.

### 4.2 Security and operations

The security/operations review also recorded positive controls:
- dashboard bound to loopback and read-only;
- PBKDF2 password hashing and fail-closed TOTP behavior;
- parameterized SQL and safe YAML loading;
- no `shell=True` in the production code paths inspected;
- SSH key-only configuration and fail2ban in the deployment artifacts;
- stale-proof process lock, cron-integrity checks, independent liveness probe, and monitoring canary.

These are source-level observations; actual installed state remains outside the snapshot audit.

### 4.3 Configuration and tests

The configuration/tests auditor reported:
- 404 test files and 5,901 tests in the repository census;
- strong test-fixture guardrails around real DB, network, log/report writes, and sentinel redirection;
- integration tests that wire real subsystems;
- a mechanically clean AST import sweep in the sampled scope;
- test fixtures that still encode superseded configuration assumptions;
- a loader-default mismatch against current configuration;
- `dynamic_by_winrate` configured on while performance-weight sizing appears unwired;
- a 31-commit gap in the handover ledger relative to later local commits, including production-code changes.

The exact test count and coverage characterization should be independently reproduced from the repository before being used as a final assurance statement.

## 5. Verified controls and strengths

The supplied audit log does not describe a uniformly weak system. It records several meaningful defensive controls:
- write-ahead capital ledger and guarded commit/release flows;
- exactly-once guards and compensating recovery paths;
- kill-switch escalation on capital-commit failure;
- transaction discipline and database locking controls;
- read-only dashboard behavior and parameterized database access;
- strong dashboard authentication mechanisms;
- cron-integrity monitoring and independent health/liveness checks;
- meaningful test harness protections against accidental external side effects.

These controls are important. Remediation should preserve them and avoid weakening existing invariants while addressing the findings.

## 6. Recommended priority order

This is a prioritization of review/remediation planning, not authorization to change production.

| Priority | Finding(s) | Recommended next action |
|---|---|---|
| P0 — verify before any live-capital increase | F-02, F-03 | Privately verify actual webhook reachability, HMAC/token configuration, service ownership, writable paths, and sudoers boundary on the authorized VM. Do not expose credentials in reports. |
| P0 — prevent unsafe operator action | F-01, F-17, F-13 | Mark the revert utility and diagnostic cleanup options as unsafe pending correction; ensure operators do not run their mutating modes; clarify the safe test command without executing it. |
| P1 — financial record resilience | F-05, F-06 | Establish off-VM/off-disk backup protection and reconcile retention policy with financial-ledger immutability and trade-evidence retention. |
| P1 — capital and order integrity | F-08, F-09, F-10, F-11 | Reconcile strategy attribution, entry guard behavior, margin authority, and paper/live control parity through a documented technical review. |
| P1 — credential protection | F-04 and DA3-06 | Review credential concentration, secret-scanner coverage, and credential rotation requirements using a secure process. |
| P2 — data/reporting correctness | F-07, F-16 | Correct the schema/query contract and ensure dashboards distinguish absent data from healthy zero values. |
| P2 — operational observability | F-18 and DA3-08/10/15 | Verify installed log rotation, health-check exposure, cron PATH assumptions, and alert-channel redundancy. |
| P2 — governance and test quality | F-14, F-15, F-13 | Reconcile TEMP records, actual config, validator wiring, test runner, and handover ledger. |

## 7. Required follow-up before a final audit report

The supplied evidence does not support closing the full-system audit. The following work remains necessary:

1. Obtain and review DA1 (signals/screening) findings and its evidence files.
2. Complete the Topic-02 architecture/dependency analysis and reconcile it against actual call paths.
3. Obtain the full DA2, DA3, DA4, and money-path finding files—not only activity-log summaries—and consolidate duplicate/root-cause findings.
4. Re-read and line-verify every finding in the consolidated register against the stated baseline.
5. Complete the cross-module trading lifecycle review, especially scanner → filter → decision → sizing → order → fill → position → protection/exit → persistence → reporting.
6. Review the large operational scripts and the unread ledger/path documents that may alter conclusions.
7. Perform a separate, explicitly authorized live-state verification for firewall, service ownership/permissions, installed logrotate/cron/hooks, timezone, credentials, and database state. Do not mix source-snapshot facts with live-state claims.
8. Run a full secret/data-sensitivity review before publishing any audit material to a public repository. Never include secret values or sensitive account identifiers.
9. Validate the final report set, findings register, and publication diff. Publish only the dated audit folder on the dedicated audit branch; never publish to `main` or include application-source changes.

## 8. Audit status and assurance statement

**Status: PARTIALLY COMPLETE — INTERIM FINDINGS ONLY.**

The supplied activity log supports the conclusion that three domain reviews and one money-path review produced substantial findings. It does not support a complete audit of the full trading system. DA1 and the architecture workflow were still outstanding in the log; important portions of DA3 and other operational areas were not fully line-audited; and live infrastructure/database state was not examined.

No finding in this report should be interpreted as proof of a current live incident unless explicitly stated and supported by live evidence. Conversely, absence of a finding in an uncompleted or partially inspected domain is not evidence that the domain is safe.

**Change-control statement:** The supplied log reports no application source changes, no live-system operations, no database/network activity, and no V2-repository access. This interim report does not authorize remediation, deployment, or trading-system changes.

---

## Appendix A — Auditor workstreams represented in the supplied log

| Workstream | Reported status | Evidence in supplied log |
|---|---|---|
| Lead auditor — money path | Completed preliminary pass | 9 findings plus verified-controls catalogue; detailed individual evidence files not attached here |
| DA2 — data integrity/persistence | Completed; headline claims spot-verified | 19 entries including controls; JSON/Markdown paths reported in Claude scratchpad |
| DA3 — security/operations | Completed; DA3-01 spot-verified | 20 entries including controls; JSON/Markdown paths reported in Claude scratchpad |
| DA4 — configuration/tests/history | Completed; DA4-02 spot-verified | 14 entries including controls; JSON/Markdown paths reported in Claude scratchpad |
| DA1 — signals/screening | Not present as completed in supplied log | No DA1 completion report included |
| Topic-02 architecture workflow | Incomplete in supplied log | Earlier status said 5 of 14 units drafted; no final completion evidence included |
| Consolidated findings register/master report | Not evidenced as completed | Log repeatedly says consolidation and master report were still pending |

## Appendix B — Source-reference caveat

File paths and line numbers in this interim report are transcribed from the supplied Claude activity log. They have not been independently re-opened against the repository in preparing this report. They must be revalidated against the exact audited commit before this document is treated as a final, publication-ready technical audit.
