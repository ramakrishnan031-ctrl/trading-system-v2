# IMPORT AND DEPENDENCY GRAPH — Topic 02

- **Date:** 2026-10-31 (IST) · **Baseline:** `970aabf3c78d777d9d34549e021f7c8e756c7bdb` (snapshot, not the live VM)
- **Machine-readable companions (this folder):** `IMPORT_EDGES.csv` (every import statement, 7,351 rows), `MODULE_INDEX.csv` (every `.py` module, 734 rows), `PACKAGE_DEPENDENCY_MATRIX.csv` (component-to-component counts, 88 rows); `supporting/graph_summary.json` and `supporting/static_import_analyzer.py` (method, reproducible).
- **Status of statements:** everything in §2–§11 is a deterministic fact computed from source text unless explicitly marked *inference*. Interpretation of gates and runtime behaviour is in REACHABILITY_ANALYSIS.md and the component notes.

## 1. Method

- An audit-authored analyzer parsed every tracked `.py` file with Python's `ast` module (Python 3.13.18). Parsing reads source text only; **no repository module was imported or executed.** Result: 734 files parsed, **0 parse failures**.
- Each import is recorded with its **context**: `top-level`; `top-level-conditional` (inside module-level `if`/`for`/`with`); `try-optional` (inside a `try` whose handler catches `ImportError`/`Exception`); `class-level`; `type-checking` (inside `if TYPE_CHECKING`, never executed); `function-level` (inside a function — a *lazy* import that runs only when the function runs).
- **Resolution:** absolute imports are resolved against the repository root first, then against the importing file's own and ancestor directories (to model script-style `sys.path`); relative imports are resolved from their package. "Internal" means the import resolves to a tracked module or package directory.
- **Closures:** *eager* = transitive imports through top-level / conditional / try / class contexts (what loads when a process starts); *full* = eager + function-level (what can load during a run). Importing `a.b.c` also executes `a/__init__.py` and `a/b/__init__.py`; closures include that.
- **File kinds:** `test` = under `tests/`, any `*/tests/*`, `test_*.py` or `conftest.py`; `script` = under `scripts/`; `runtime` = everything else.

## 2. Counts

| Measure | Value |
|---|---|
| Python files (runtime / script / test) | 734 (196 / 84 / 454) |
| Import edges | 7,351 |
| — standard library | 3,857 |
| — internal module / internal package | 2,953 / 14 |
| — third-party | 527 |
| — internal but unresolvable | **0** |
| Contexts: top-level / function-level / top-level-conditional / type-checking / try-optional | 5,494 / 1,802 / 30 / 17 / 8 |

## 3. Component dependencies (non-test sources)

Heaviest component-to-component edges (from `PACKAGE_DEPENDENCY_MATRIX.csv`):

| From | To | Imports (eager / lazy) | Example |
|---|---|---|---|
| scripts | core | 161 (108 / 53) | `scripts/alert_watcher.py:68` |
| orders | core | 72 (67 / 5) | `orders/closure_classifier.py:26` |
| scripts | utils | 54 (2 / 52) | `scripts/auto_refresh_token.py:295` |
| scripts | alerts | 42 (7 / 35) | `scripts/alert_watcher.py:61` |
| ops_dashboard.backend.services | ops_dashboard.backend.readers | 36 (36 / 0) | `ops_dashboard/backend/services/analytics_period.py:32` |
| main.py | core | 27 (15 / 12) | `main.py:40` |
| broker | core | 27 (26 / 1) | `broker/clock_skew_probe.py:48` |
| capital | core | 25 (20 / 5) | `capital/drift_handler.py:57` |
| main.py | orders | 16 (13 / 3) | `main.py:71` |
| orders | broker | 13 (12 / 1) | `orders/cnc_gtt_monitor.py:44` |
| main.py | sr_detector | 12 (0 / 12) | `main.py:3493` |
| v3_chain | sr_detector | 7 (6 / 1) | `v3_chain/pb01_runner.py:37` |
| orders | capital | 6 (6 / 0) | `orders/eod_squareoff.py:55` |
| main.py | v3_chain | 4 (0 / 4) | `main.py:3677` |

- **Hub modules** (non-test importers): `core.time_authority` 99, `core.logger` 56, `utils.cron_heartbeat` 49, `alerts.telegram_notifier` 35, `core.config_loader` 35, `core.state_store` 35, `core.account_registry` 30, `core.effect_telemetry` 30, `core.exceptions` 30.
- **Highest fan-out:** `main` 102 internal import edges; then `orders.order_placer` 23, `orders.order_reconciler` 20.
- **Largest non-test modules (lines):** `orders/order_placer.py` 4,706 · `orders/order_reconciler.py` 4,621 · `main.py` 4,293 · `ops_dashboard/backend/readers/db_reader.py` 4,205 · `core/state_store.py` 3,355 · `reports/daily_trade_review.py` 2,924 · `signals/signal_processor.py` 2,703 · `capital/fund_manager.py` 2,583 · `broker/zerodha_adapter.py` 2,520 · `core/config_loader.py` 2,516.
- **Dashboard isolation (fact, scoped to imports):** no non-test file under `ops_dashboard/` imports any repository module outside `ops_dashboard/`; its third-party imports (flask, openpyxl, yaml, waitress, pyotp) are all declared in `ops_dashboard/backend/requirements.txt`, whose header states an isolation rule ("kiteconnect MUST NOT appear here"). Data-level coupling (shared DB/files) is not visible in imports — see the component notes.

## 4. What `main.py` loads

- **Eager closure: 87 modules** — core 21, orders 18, broker 12, capital 7, screening 6, alerts 5, strategies 4, utils 4, data 3, scripts 3, signals 3, main 1.
- **Full closure: 130 modules.** The 43 modules that load only through lazy imports: `allocation` (5 modules), `regime` (4), `sr_detector` (12), `v3_chain` (8), `scripts.preflight` (base, sentinel, startup_hook, package), `alerts.cron_alerts`, `capital.strategy_governor`, `core.candle_math`, `core.cron_registry`, `core.evidence_contract`, `core.strategy_direction`, `orders.structure_exit_manager`, `screening.retest_monitor`, `signals.entry_throttle`, `utils.cron_heartbeat`.
- **First lazy entry into each optional tree:** `main.py:3493` → `sr_detector`; `main.py:3518` → `regime`; `main.py:3635` → `allocation`; `main.py:3677` → `v3_chain`. What gates those calls is established from code reading in REACHABILITY_ANALYSIS.md.
- **Never reachable from `main.py`** (static, any context): `ops_dashboard`, `ops`, `reports`. They run as separate processes (see ENTRY_POINTS_AND_PROCESS_TOPOLOGY.md).

## 5. Import cycles

- **Module level, eager imports only: none.**
- **Module level, including lazy imports:**
  1. A 12-module cycle: `broker.{cost_calculator, order_monitor, rate_limiter, slippage_engine, zerodha_adapter}`, `core.{config_auditor, config_loader}`, `orders.{full_entry_engine, order_placer, order_protocol_co, order_protocol_limit, smart_tgt_manager}`. Its eager edges include `orders/order_placer.py:222-248` (→ broker.cost_calculator, broker.order_monitor, core.config_loader, orders.full_entry_engine, orders.smart_tgt_manager) and `broker/zerodha_adapter.py:111-122` (→ cost_calculator, rate_limiter, slippage_engine, core.config_loader). The cycle closes only through two lazy imports: `core/config_loader.py:2072` → `core.config_auditor` and `core/config_auditor.py:568` → `orders.order_placer`.
  2. `scripts/check_cron_drift.py:50` → `scripts.cron_officer` and `scripts/cron_officer.py:630` (lazy) → `scripts.check_cron_drift`.
- **Component level, eager imports:**
  - `{broker, capital, orders}`: broker → orders 1 edge (`broker/zerodha_adapter.py:121` → `orders.price_math`); orders → capital 6 (e.g. `orders/order_placer.py:226-227`, `orders/eod_squareoff.py:55-56`); capital → broker 2 (`capital/kill_switch.py:73`; `:897` lazy → `broker.auth_recovery`); orders → broker 13.
  - `{scripts, scripts.preflight}`.
- *Inference:* configuration loading/auditing reaches order-placement code at run time, and the broker adapter depends on an orders helper, so the layers are not strictly stratified. Python's import system tolerates this because the closing edges are lazy, but it is a coupling lead for later topics (ARCHITECTURE_LEADS_AND_OPEN_QUESTIONS.md).

## 6. Layering observations (facts)

| Direction | Evidence |
|---|---|
| runtime → `scripts/` | `main.py:91` (scripts.healthcheck_server), `main.py:106` and `:1875` (scripts.zerodha_login), `main.py:4088` (scripts.preflight.startup_hook) |
| broker → orders | `broker/zerodha_adapter.py:121` (orders.price_math) |
| core → orders | `core/config_auditor.py:568` (lazy, orders.order_placer) |
| capital → broker | `capital/kill_switch.py:73` (broker.position_helpers); `:897` lazy (broker.auth_recovery) |
| scripts → runtime internals | scripts → core 161 imports, → utils 54, → alerts 42, → broker 8 |

## 7. Static wiring gaps (candidates, not conclusions)

- **No importers anywhere:** `capital/performance_allocator.py`.
- **Imported only by tests:** `broker/angelone_adapter.py` (1 test importer), `capital/shadow_engine.py` (1), `orders/breakeven_manager.py` (3), `orders/sl_breach_monitor.py` (1).
- Caveat: `core/config_auditor.py:626` calls `importlib.import_module` with a computed (non-literal) name, so static analysis alone cannot rule out dynamic loading. These candidates were re-checked by the component verifiers (see REACHABILITY_ANALYSIS.md).

## 8. Import roots, `sys.path` and `__main__` blocks

- 120 internal imports resolve only relative to a sub-directory: `ops_dashboard/` 99 (as `backend.*`), `ops_dashboard/tests` 20, `scripts/` 1 — consistent with `ops_dashboard/deployment/gui-dashboard.service:17-18` (WorkingDirectory `ops_dashboard`, `python -m backend.app`).
- 59 non-test files modify `sys.path` at run time: `scripts/` 54, `ops/` 4, `run_tests.py` 1.
- 70 non-test files have a `__main__` block: `scripts/` 59, `ops/` 4, `ops_dashboard/` 2, `reports/` 2, `deploy/` 1, `main.py`, `run_tests.py`.

## 9. Dynamic imports and subprocess launches (non-test files)

| File:line | Kind | Literal target (if any) |
|---|---|---|
| `core/config_auditor.py:626` | importlib.import_module | computed name |
| `signals/signal_processor.py:1745-1746` | `__import__` | `datetime` |
| `scripts/preflight/checks/broker.py:102, 108` | subprocess.run | `scripts/auto_refresh_token.py`; `scripts/refresh_instruments.py --account` |
| `scripts/preflight/checks/services.py:33, 45` | subprocess.run | `systemctl is-active`; `sudo -n systemctl start` |
| `scripts/preflight/startup_hook.py:67` | subprocess.Popen | computed |
| `ops_dashboard/backend/readers/host_reader.py:34, 88, 146` | subprocess.run | `systemctl is-active` / `systemctl show` |
| `scripts/liveness_probe.py:211`, `scripts/monitoring_canary.py:130, 235` | subprocess.run | `systemctl show` / `is-active` |
| `scripts/approve_ssh_keys.py:123` | subprocess.run | `scripts/security_monitor.py --baseline …` |
| `deploy/hooks/secret_scan.py:173, 192, 198`; `scripts/agy_runner.py:110`; `scripts/check_cron_drift.py:136`; `scripts/cron_officer.py:557, 890`; `scripts/deploy_preflight.py:57`; `scripts/forward_shadow_record.py:74`; `scripts/security_monitor.py:247, 308, 344, 544`; `scripts/system_manager.py:783, 1191, 1196, 1206` | subprocess.run | computed / non-Python commands |

No `exec`/`eval` calls exist in non-test files.

## 10. Third-party dependencies vs requirements files

| Import | Runtime / script / test importers | Declared in |
|---|---|---|
| yaml | 17 / 16 / 38 | requirements.txt; ops_dashboard/backend/requirements.txt |
| flask | 15 / 1 / 2 | both |
| openpyxl | 11 / 0 / 86 | both |
| kiteconnect | 5 / 11 / 9 | requirements.txt |
| pydantic | 5 / 0 / 8 | requirements.txt |
| requests | 3 / 8 / 7 | requirements.txt |
| waitress | 2 / 1 / 0 | both |
| pyotp | 1 / 1 / 2 | both |
| cachetools | 1 / 0 / 0 | requirements.txt |
| pytest | 1 (`run_tests.py:14`) / 0 / 241 | requirements-dev.txt; ops_dashboard/backend/requirements.txt |
| dotenv | 0 / 22 / 5 | requirements.txt (python-dotenv) |
| **ntplib** | 0 / 2 / 0 | **none** |
| **psutil** | 0 / 4 / 2 | **none** |

## 11. Duplicate top-level helper names (seeds for the cross-module topic)

Same name does not mean same logic; content comparison belongs to Stage 5.
- `_parse_hhmm` ×6: `capital/strategy_governor.py:191`, `core/cron_registry.py:22`, `core/mis_squareoff_timing.py:40`, `main.py:613`, `ops_dashboard/backend/services/freshness.py:34`, `v3_chain/pb01_entry.py:355`.
- `_send_telegram` ×6 in scripts (`capture_metrics_baseline.py:298`, `gemini_data_integrity_check.py:241`, `gemini_premarket_brief.py:168`, `gemini_trade_coach.py:199`, `gemini_weekly_patterns.py:125`, `preflight/deliver.py:44`), alongside `alerts/telegram_notifier.py`.
- `_call_gemini_cli` ×5 (`scripts/gemini_*`); `_is_long` ×4 (`orders/price_math.py:79`, `orders/slippage_recorder.py:40`, `screening/hard_gate.py:236`, `sr_detector/retest_confirm.py:50`); `export_sheets` ×8 (dashboard services); `_cron_main` ×23 (cron-script convention in `scripts/`).

## 12. Limitations

- Static only: `getattr`/registry dispatch, computed `importlib` names, string-configured plugins, run-time `sys.path` changes and shell-launched processes are not resolved by the graph itself (launchers are parsed separately; agents checked dispatch where relevant).
- Multi-root resolution can resolve an import that would fail under a different launch path, and vice versa; `resolution_root` in the CSV records which root matched.
- Closures show what **can** load, not what **does** load under a given configuration.
- Snapshot only; no live environment was examined.
