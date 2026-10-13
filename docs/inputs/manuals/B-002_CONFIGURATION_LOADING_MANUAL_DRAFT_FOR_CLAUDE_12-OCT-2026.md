# B-002 — Configuration Loading

**Project:** Trading_System-v2_redesign  
**Status:** Draft factual chapter for Claude review — NOT frozen  
**Review date:** 12-Oct-2026  
**System status:** Legacy is DEAD / REFERENCE ONLY

> **Governance boundary:** This chapter records inspected source behaviour and evidence limits. It is not a repair plan, operating guide or V2 design. No source change, deployment, VM operation or V2 architecture decision is authorised.

## 1. Purpose and evidence discipline

Rama explicitly stated that the answered B-002 sheet is **probable/expected behaviour, not necessarily correct answers**. Its contents are treated as investigation leads. The chapter separates FACT (source-scoped), HISTORICAL, OPERATOR-REPORTED, INTERPRETATION, KNOWN DEFECT, UNKNOWN / NOT ESTABLISHED and OPEN QUESTION.

A checked-in repository file proves what that revision contains. It does not prove which file/value was installed or active on a host on an arbitrary historical date. Never reproduce credential values.

## 2. Repositories and limits

- Production snapshot: https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot
- Testing/sandbox snapshot: https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot
- Records snapshot: https://github.com/ramakrishnan031-ctrl/trading-system-records
- Records manifest: https://github.com/ramakrishnan031-ctrl/trading-system-records/blob/main/SNAPSHOT_MANIFEST.md

The README.md and core/config_loader.py blobs matched between production and testing in this inspection. This is not repository-wide or host-runtime parity.

The records manifest says the records snapshot was assembled in passes on 17–18 Sep 2026; some earlier-captured files were not refreshed in later passes; intentional redactions and exclusions exist; and the production VM `.env` was not read in the recorded scan. The records repo is a read-only snapshot, not a deploy target. Its limits must accompany any historical claims.

## 3. Inventory: eight main-loader files are not the full inventory

### 3.1 Eight files managed by `core/config_loader.py`

| Path | Schema class stated in loader header |
|---|---|
| `config/system_config.yaml` | `SystemConfig` |
| `config/broker_costs.yaml` | `BrokerCostsConfig` |
| `config/broker_limits.yaml` | `BrokerLimitsConfig` |
| `config/slippage_model.yaml` | `SlippageConfig` |
| `config/scoring_weights.yaml` | `ScoringConfig` |
| `config/scan_webhook_map.yaml` | `ScanWebhookMapConfig` |
| `config/chartink_scanners.yaml` | `ChartinkScannersConfig` |
| `config/nse_holidays_2026.yaml` | `NseHolidaysConfig` |

**Source:** `core/config_loader.py`, module header and `load_all()` around lines 2457–2517.

### 3.2 Additional sources visible in the checked-in repository

The inspected `config/` listing also contains `accounts.csv`, `accounts_multi_example.csv`, `cron_registry.yaml`, `expected_managers.yaml`, `index_universe.yaml`, `preflight.yaml`, `reference_data/`, `strategies/`, `security.yaml`, `strategy_direction_registry.yaml`, and `symbol_aliases.yaml`.

Other configuration-like sources in inspected code include:
- CLI arguments (`main.py::_parse_args`);
- environment variables and `.env`;
- checked-in systemd `EnvironmentFile`, `WorkingDirectory` and `ExecStart`;
- `config/accounts.csv` parsed by `core/account_registry.py`;
- `data_store/session/zerodha_token.json` parsed/used in the login/live path;
- strategy YAMLs under `config/strategies/`;
- hard-coded constants and schema defaults.

**Important correction:** the claim “all configuration sources are under config/” is incomplete. Also, the inspected main.py uses `config_dir / "strategies"`, not repository-root `strategies/*.yaml`. Presence of a file in the repo does not prove it is read on every startup; trace each consumer.

## 4. Startup path: the holiday file is read before the main loader

### 4.1 CLI and directory resolution

`main.py::_parse_args()` (around lines 179–205) defines `--mode paper|live` (default paper), `--config PATH`, `--resume`, `--status`, `--dry-run`, `--version` and `--interactive`.

`main.py::main()` resolves:

```python
config_dir = Path(args.config) if args.config else Path("config")
```

Therefore `--config` can override the path. Without it, `Path("config")` is relative to process current working directory.

The checked-in `deploy/systemd/trading-system.service` specifies a working directory, an EnvironmentFile and `main.py --mode live` without `--config`. This proves repository unit content only, not the installed unit at any historical time.

### 4.2 Early holiday guard

`utils/holiday_guard.py::_load_holiday_set()` directly reads `config_dir / f"nse_holidays_{year}.yaml"`, parses it with `yaml.safe_load`, converts supported entries into dates and caches by `(config_dir, year)`. This is a separate reader, not the main Pydantic loader.

`main()` calls `is_trading_day()` before logging setup, instance lock and `load_all()`. It catches `FileNotFoundError` around this guard and proceeds. This only establishes tolerance in the early guard path; the later eight-file loader also requires its holiday file. Malformed YAML or invalid dates are not covered by that specific FileNotFoundError catch.

### 4.3 High-level source-to-handoff sequence

| Stage | Source | Input and logic | Output / next consumer |
|---|---|---|---|
| 1 | `main.py::_parse_args` | CLI parsing/defaults | `args` |
| 2 | `main.py::main` | `--config` or relative `config` | `config_dir` |
| 3 | `utils/holiday_guard.py` | Direct read of year holiday YAML; weekend/holiday logic | Early trading-day decision |
| 4 | `main.py::main` | Service-window guard, flags/environment | Continue or clean exit |
| 5 | `main.py::main` | Logging setup and single-instance lock | `_main_locked(args, config_dir)` |
| 6 | `core/config_loader.py::load_all` | Eight YAML files, hashes, parsing and schemas | `AppConfig` or startup failure |
| 7 | `main.py::_main_locked` | StateStore/session/hash/startup checks | Startup state and subsystem setup |
| 8 | `main.py` | Account/mode paths and config snapshot | Resolved account/mode, DB snapshot |
| 9 | `main.py` + `strategies/loader.py` | `config_dir / "strategies"` | Strategy dictionary |
| 10 | `main.py` | Remaining subsystem startup; `validate_all(strict=False)` | Startup-complete event/session update |

This is a high-level trace. Claude should verify all branches and failure paths at the exact chosen commit.

## 5. Primary YAML loader

### 5.1 Algorithm

**Source:** `core/config_loader.py::load_all()`, around lines 2457–2517.

1. Iterate `_CONFIG_FILES`.
2. Build `path = config_dir / filename`; if it is not a file, raise `ConfigMissingError`.
3. Read raw bytes and compute SHA-256 over those bytes.
4. Parse with `yaml.safe_load(raw_bytes)`.
5. Normalise parser result `None` to `{}`.
6. Validate with the corresponding schema class’s `model_validate(data)`.
7. Wrap `yaml.YAMLError` and Pydantic `ValidationError` as `ConfigSchemaError`.
8. Return `AppConfig(**validated, file_hashes=hashes)` only after all eight succeed.

### 5.2 Failure distinctions

| Condition | Source-supported result | Limit |
|---|---|---|
| Missing required file | `ConfigMissingError` | main load handler logs CRITICAL and returns 5 |
| YAML syntax/parser error | `ConfigSchemaError` wrapper | exact output depends on exception path |
| Pydantic schema failure | `ConfigSchemaError` wrapper | schema fields/validators differ |
| Empty YAML | `None` becomes `{}`, then schema validation | may pass or fail depending on schema defaults/required fields |
| Unknown key | Models with `extra="forbid"` reject it | does not mean every known field is mandatory |
| Failure after earlier files loaded locally | No `AppConfig` returned | does not prove no earlier startup side effects occurred |

### 5.3 Defaults and validation

The loader’s header uses shorthand language about required fields, but actual schemas include defaults and model/field validators. Examples in `TradingHoursConfig` include defaults for `market_open`, `market_close`, `service_window_end` and MIS timing fields.

Consequently:
- `extra="forbid"` rejects unknown keys;
- it does not make every field required;
- it does not make all missing/null/invalid cases identical;
- the manual must extract required/default fields and validators by schema;
- source defaults must not be silently “corrected”.

## 6. Strategy configuration

`main.py` around line 3158 assigns `strategies_dir = config_dir / "strategies"` and calls `StrategyLoader.load_all_strategies()`.

The strategy loader sorts `*.yaml`, raises `ConfigSchemaError` if none exist, validates each file via `validate_strategy()`, and assigns `self._strategies` only after the complete loop succeeds. Its declared behaviour is fail-fast/no partial dictionary. Source: `strategies/loader.py`, around lines 102 onward.

`scan_strategy_errors()` separately validates all strategy files and returns all bad file/reason pairs. `main.py` contains a helper for aggregated Telegram/email alerting. The final manual must trace where this scan is called, whether it runs before or after the loader failure, which exceptions reach it and which exit code follows. The loader’s optional `scan_webhook_map_path` argument exists, but the inspected main.py call does not pass it; do not assert that optional check runs there without additional evidence.

## 7. Other structured sources

### 7.1 Account registry
`core/account_registry.py::AccountRegistry.load()` reads `accounts.csv` with `csv.DictReader`, validates required columns and row values, and returns an `AccountRegistry`. This is outside the eight-file `AppConfig` loader.

### 7.2 Environment and token/session input
`main.py` defines required environment-variable names and its live path reads account-specific environment values plus a JSON session token file. `scripts/zerodha_login.py` imports `dotenv.load_dotenv` and calls `load_dotenv()`; it also parses token JSON. Thus “core/config_loader.py does not read `.env`” must not be generalised to the entire application.

The checked-in systemd unit references an EnvironmentFile and working directory. `.gitignore` excludes `.env` and credential/generated artifacts in the inspected snapshot. These are source/repository facts, not proof of the exact historical host values or proof that no sensitive value ever appeared elsewhere. Do not include actual values.

### 7.3 Remaining config directory
Trace `cron_registry.yaml`, `expected_managers.yaml`, `index_universe.yaml`, `preflight.yaml`, `security.yaml`, `strategy_direction_registry.yaml`, `symbol_aliases.yaml`, `reference_data/` and any generated inputs to their consumers. “Not managed by core/config_loader.py” does not mean “unused”.

## 8. Object handoff and consumers

`load_all()` returns `AppConfig` with typed sub-configs and `file_hashes`; main holds it in local `app_config`. Examples of explicit sub-config handoff include `RateLimiter(app_config.broker_limits, ...)`, `ProductResolver(app_config.system.product_map)`, `CostCalculator(app_config.broker_costs)` and `MarketWindows` built from `app_config.system.trading_hours` plus holiday/special-session data. This supports explicit injection in these paths, not necessarily every module.

`core/config_validator.py` defines a singleton usage tracker. Main registers AppConfig values and calls `validate_all(strict=False)` near line 4164. In that call:
- unaccessed registered keys warn;
- detected tracked-value drift raises `ConfigValidationError`;
- strict mode would raise for unaccessed keys, but main does not use strict mode there.
Do not claim the validator proves every value is used or that every unaccessed key blocks startup.

`core/config_auditor.py` documents groups A–G and PASS/INFO/WARN/BLOCK severities. Some BLOCK findings can block startup; warnings/info are not all hard failures. Identify exact groups at each call site before describing the audit as a universal gate.

## 9. Hashes and snapshots — three separate mechanisms

### 9.1 Raw file hashes
`load_all()` hashes raw bytes per file. `utils/startup_checks.py::check_config_hash()` compares the current mapping with JSON in session `last_config_hash`. If no previous usable mapping exists, it returns `changed=False`; otherwise it returns and logs changed filenames. Follow all uses of `hash_result.changed`; the comparison function alone does not establish a blocking gate.

A comment-only edit changes raw bytes/hash even if effective values do not change.

### 9.2 Database configuration snapshot
`core/config_snapshotter.py::snapshot_config()`:
1. converts AppConfig to a JSON-native form;
2. redacts populated string values whose keys contain `password`, `secret`, `token`, `api_key` or `apikey`, except keys ending `_env`;
3. serialises stable JSON (sorted keys, compact separators);
4. hashes that JSON;
5. skips if `config_snapshots` already has the same date+hash; otherwise inserts a row with date, timestamp, account, mode, trade type, hash and JSON.

Main catches snapshot exceptions and logs them as non-fatal. Because the AppConfig dump includes `file_hashes`, a comment-only edit may change snapshot hash despite unchanged effective values. Compare redacted `config_json` to assess value differences; do not infer value change from hash alone.

### 9.3 Usage validation
`config_validator.validate_all(strict=False)` is not a hash check or snapshot. Unaccessed keys warn in this path; tracked value drift raises. It has different coverage and failure semantics.

## 10. Paper/live and production/testing

### 10.1 Paper/live
The CLI defaults to paper; the checked-in systemd unit invokes live. The inspected main path has different branches: paper sets `kite_client=None` at an early point, while live loads account registry data, resolves account-specific environment variables, reads token JSON and builds a live client.

This proves mode-specific code paths, not that every YAML value differs or which mode ran on each VM on every date.

### 10.2 Production/testing
Only README.md and core/config_loader.py were verified to have matching blobs in this inspection. The full config directory, all strategy files, service units, host overrides and active environment values have not been compared exhaustively.

Rama reported manual test-host changes to market times and trade limits. Until dated host captures corroborate the specific host, timestamp, file and key, label this **OPERATOR-REPORTED / HISTORICAL**, not source-proven FACT.

## 11. Failure matrix

| Case | Supported behaviour | Still open |
|---|---|---|
| Missing one of eight YAMLs | `ConfigMissingError`; no AppConfig; main handler logs CRITICAL and returns 5 | exact outer error formatting |
| Invalid YAML syntax | wrapped as `ConfigSchemaError` | alert/log details by path |
| Schema/type/cross-field failure | Pydantic `ValidationError` wrapped as `ConfigSchemaError` | complete validator catalogue |
| Empty YAML | normalised to `{}`, then model validation | outcome per schema |
| No strategy YAMLs | strategy loader raises `ConfigSchemaError` | exact catch/exit |
| First invalid strategy | loader fails fast/no partial dict | exact scan/alert ordering |
| Holiday file absent in early guard | `main()` catches `FileNotFoundError` and proceeds from that stage | later main loader still requires the holiday file |
| Snapshot write fails | main logs failure as non-fatal | historical occurrence evidence |
| Unaccessed config keys | warning under `strict=False` | host logs and coverage |
| Config usage drift | `ConfigValidationError` raised | outer catch/exit code in exact path |
| Changed raw hash | changed filenames returned/logged | follow downstream flag use |

## 12. Historical claims that must remain open

### Host-level manual edits
The answer sheet reports testing-host edits. Corroborate with a dated capture identifying host, timestamp, path/key, and whether it records a file, loaded process state or later copy. Otherwise keep OPERATOR-REPORTED / HISTORICAL.

### “Misleading PASS”
A repository test passing while host-local config differs is a plausible risk, not yet a proven defect. To call it KNOWN DEFECT, identify the check and input path, the actual host state, its PASS output and a dated record linking them.

### Rollback
Do not conclude “no rollback existed” from the loader alone. Search deployment scripts, hooks, service units, restore scripts, runbooks and dated incident records. Distinguish no loader-level rollback, no application rollback found, no deployment rollback anywhere, and a manual recovery actually used in a dated incident.

## 13. Required source-to-consumer table

| Source | Reader | Parse/validation | Output/handoff | Current evidence status |
|---|---|---|---|---|
| Eight main YAMLs | `core/config_loader.py::load_all` | `yaml.safe_load`, Pydantic, validators | `AppConfig`, raw-file hashes | FACT for inspected source |
| Holiday YAML | `utils/holiday_guard.py` before main loader | direct YAML/date parsing/cache | early trading-day decision | FACT for inspected source |
| Strategy YAMLs | `config_dir/strategies`, `StrategyLoader` | `validate_strategy`, strategy schema | strategy dictionary | FACT for loader/path; alert sequence OPEN |
| Account CSV | `AccountRegistry.load` | `csv.DictReader`, column/row checks | AccountRegistry | FACT for inspected source |
| Environment/.env | main and `scripts/zerodha_login.py` | env lookup / `load_dotenv` | auth/notification/webhook inputs | code path FACT; historical values UNKNOWN |
| Token JSON | `load_token` and live main path | `json.load` and path/key handling | session token data | code path FACT; token contents UNKNOWN |
| Extra YAML/CSV/reference data | consumer not fully mapped | OPEN | OPEN | OPEN QUESTION |
| Systemd unit | checked-in `deploy/systemd/trading-system.service` | OS/service manager | CWD, env file, invocation/restart policy | source file FACT; installed state UNKNOWN |
| Session hash | `check_config_hash` | JSON decode + file-hash compare | changed-files result/logs | function FACT; downstream effect OPEN |
| DB config snapshot | `snapshot_config` | redaction, canonical JSON, hash, dedupe | `config_snapshots` row | function FACT; failure non-fatal |
| Usage tracker | `config_validator` | key-access/drift tracking | warnings or drift exception | call path partly FACT |
| Semantic audit | `config_auditor.audit` callers | groups/severities | findings and some blocking paths | exact caller/group map OPEN |

## 14. Required Claude work before freeze

1. Record exact production/testing refs/commit SHAs and compare all relevant code/config paths.
2. Search for `yaml.safe_load`, `yaml.load`, `json.load`, `csv.DictReader`, `load_dotenv`, `os.environ`, `getenv`, `Path("config")`, `config_dir` and all extra config filenames.
3. Extract required/default fields, validators, ranges, cross-field rules and exception mapping for all eight schemas and strategy schema.
4. Trace main.py from argument parsing through early holiday guard, primary load, state/hash checks, startup checks, broker/account branches, snapshot, strategy load/error scan/alert, usage validation, startup marker and all exits.
5. Inspect StateStore schema for session `last_config_hash` and `config_snapshots`; follow all uses of `hash_result.changed`.
6. Map every config-auditor invocation to groups/severities and blocking behaviour.
7. Use dated records for host-level claims; preserve manifest capture/redaction/exclusion limits.
8. Search deploy and recovery evidence before asserting no rollback or a known false-PASS defect.
9. Recheck line numbers against the exact chosen commit before freezing.

## 15. Interim conclusion

The broad primary-loader behaviour is established: eight YAML files are read from a configurable directory, raw-byte hashes are computed, YAML is parsed, Pydantic schemas validate each file, and `AppConfig` is returned only after all eight succeed.

The overall configuration lifecycle is broader: an early holiday reader, strategy loader, account CSV, environment/.env, token JSON, service settings, audit utilities, raw-file hash comparison, database snapshots and usage tracking all exist in inspected source.

**Not ready to freeze.** The complete source inventory, per-key consumer map, exact strategy failure/alert sequence, full production/testing comparison, hash/audit side effects and historical host values still need Claude review. No V2 design decision is implied.
