# A-009 — CONFIGURATION MODEL
## LEGACY SYSTEM TECHNICAL MANUAL — 30-SEP-2026

**STATUS: CLOSED / FROZEN**  
**SYSTEM: DEAD LEGACY SYSTEM — REFERENCE ONLY**

## 1. Scope

This chapter records how configuration existed in the legacy system. It does not repair, tune, simplify or redesign it.

**Evidence hierarchy:** Production repository → Sandbox/Testing comparison → Records documentation → user historical recollection, with direct repository evidence taking precedence where conflicts exist.

## 2. Architecture

```text
                  LEGACY CONFIGURATION
                         |
             +-----------+-----------+
             |                       |
       Core AppConfig          Strategy YAMLs
             |                       |
       8 YAML files             StrategyConfig
             |                       |
     core/config_loader.py    strategies/loader.py
             |                       |
             +-----------+-----------+
                         |
                    Runtime system
```

Separate consumers also use `accounts.csv`, `symbol_aliases.yaml`, `cron_registry.yaml`, `expected_managers.yaml`, `preflight.yaml`, `security.yaml`, `index_universe.yaml` and `strategy_direction_registry.yaml`.

## 3. Core configuration files

| File | Legacy purpose / consumer |
|---|---|
| `system_config.yaml` | master/global controls |
| `broker_costs.yaml` | broker/trading cost configuration |
| `broker_limits.yaml` | client-side API/rate/token limits |
| `slippage_model.yaml` | slippage model |
| `scoring_weights.yaml` | secondary-screen weights/thresholds |
| `scan_webhook_map.yaml` | Chartink scanner → strategy |
| `chartink_scanners.yaml` | Chartink URL/reference configuration |
| `nse_holidays_2026.yaml` | NSE calendar/holiday data |

`core/config_loader.py` is authoritative for these eight.

## 4. Core loader trace

**SOURCE:** `core/config_loader.py`

**INPUT:** `config/` by default.

**LOGIC:** `load_all()` reads all eight YAMLs with `yaml.safe_load()`, validates them and constructs `AppConfig`.

**GUARD:** strict Pydantic schemas; `extra="forbid"`; missing/schema-invalid files raise errors; no partial AppConfig.

**OUTPUT:** resolved `AppConfig`.

**HANDOFF:** `main.py` passes configuration into runtime components.

**AUDIT:** SHA-256 per raw file stored in `AppConfig.file_hashes`.

**RELOAD:** no hot reload.

Historical explicit `--config` usage: UNKNOWN.

## 5. Strategy loader trace

**SOURCE:** `config/strategies/*.yaml`

**INPUT:** strategy YAMLs.

**LOGIC:** `strategies/loader.py` loads and validates `StrategyConfig`.

**GUARD:** invalid YAML causes `ConfigSchemaError`; no partial strategy set.

**OUTPUT:** validated strategy set.

**HANDOFF:** strategy control/signal/trading components.

Strategy configuration contains strategy identity, direction, intent/product, enabled state, entry, SL, target/R:R, trailing/pullback, screening and position-cap-related fields.

## 6. Global vs strategy ownership

Historical model:
- global `system_config.yaml` = Master Control Panel
- strategy YAML = strategy/trade-specific secondary layer.

Repository confirms global timing can constrain strategy timing (`entry_start` floor).

Global areas include:
- trade type
- delivery enablement
- trading hours
- capital/sizing/risk
- queue
- product map
- safety/clock controls.

Strategy areas include:
- strategy conditions/metadata
- direction
- product/intent
- entry/SL/target/R:R
- screening
- strategy-specific limits.

Universal precedence across all overlapping fields is **NOT ESTABLISHED**.

## 7. Additional configuration-like files

- `accounts.csv`: account/client credential references. Secrets excluded.
- `index_universe.yaml`: index/reference data.
- `strategy_direction_registry.yaml`: direction/health mirror; does not override StrategyConfig.
- `symbol_aliases.yaml`: symbol normalization.
- `cron_registry.yaml`: executable cron source of truth.
- `expected_managers.yaml`: manager/state metadata.
- `preflight.yaml`: separate preflight configuration; not AppConfig.
- `security.yaml`: separate security-monitor configuration; not AppConfig.

## 8. Validation / failure behavior

Core:
```text
missing / malformed / schema-invalid
             |
             v
configuration error
             |
             v
no partial AppConfig
```

Strategy:
```text
invalid strategy YAML
        |
        v
ConfigSchemaError
        |
        v
no partial strategy set
```

User does not recall deliberately testing these failure cases.

Future-only requirement: invalid required configuration should block new-system boot and alert Telegram + Email.

## 9. Defaults / fallbacks

The core loader does not fill missing required keys, although individual schema fields may define defaults.

Historical use of defaults/fallbacks in an actual Production trade: UNKNOWN.

A fallback existing in code is not evidence that it executed in Production.

## 10. Snapshot / audit model

```text
Resolved AppConfig
       ↓
canonical JSON
       ↓
SHA-256
       ↓
config_snapshots
```

Same date + same hash → skip duplicate.  
Same date + different hash → new row.

Snapshot stores resolved runtime configuration, not merely raw YAML.

Comment-only/raw-byte changes can alter file hashes and therefore configuration hash.

Historical operator reliance: UNKNOWN.

## 11. Paper / Live

Same YAML set was used/intended for both modes, but runtime behavior was not equivalent.

User reports historical differences in:
- fills/prices
- order timing
- P&L/results.

Classification: known historical limitation.

V2 input: no Paper mode.

## 12. Production / Testing

Production is authoritative for the legacy baseline.

Testing/Sandbox had:
- same broad file structure
- different config edits
- S&R differences
- timing/config differences
- some fixes not deployed to Production.

Therefore Testing cannot be used as silent replacement evidence for Production.

## 13. Configuration-change governance

Historical process included:
- direct VM editing
- VS Code/Claude MCP editing
- Git deployment/commits
- occasional manual/uncommitted changes
- restart after YAML edits
- no remembered formal approval/change register.

This could create local/Git/VM configuration drift.

## 14. Runtime values

Static configuration must be distinguished from derived/live values such as:
- capital/reservations
- positions/orders/fills
- broker margin
- screening output
- sizing output
- target/SL state.

Exact storage/authority belongs to module-specific manuals.

## 15. Configuration trace matrix

| Area | Source | Consumer/handoff | Status |
|---|---|---|---|
| Global controls | `system_config.yaml` | runtime modules | FACT |
| Broker costs | `broker_costs.yaml` | cost calculation | FACT |
| Broker limits | `broker_limits.yaml` | rate limiter | FACT |
| Slippage | `slippage_model.yaml` | paper/cost path | FACT |
| Secondary score | `scoring_weights.yaml` | secondary screener | FACT |
| Scanner mapping | `scan_webhook_map.yaml` | signal/strategy path | FACT |
| Chartink URLs | `chartink_scanners.yaml` | health/preflight | FACT |
| NSE holidays | `nse_holidays_2026.yaml` | calendar/session | FACT |
| Strategy rules | strategy YAMLs | StrategyLoader/runtime | FACT |
| Accounts | `accounts.csv` | account/broker setup | FACT |
| Symbol aliases | `symbol_aliases.yaml` | symbol normalization | FACT |
| Direction mirror | registry YAML | health/status | FACT |
| Cron | `cron_registry.yaml` | cron generator | FACT |
| Manager expectations | `expected_managers.yaml` | manager checks | FACT |
| Preflight | `preflight.yaml` | preflight process | FACT |
| Security | `security.yaml` | security monitor | FACT |

## 16. Historical issues

1. Configuration could be changed through multiple paths.
2. Manual/uncommitted changes occurred.
3. Local/Git/VM drift was possible.
4. Paper and Live were not behaviorally equivalent.
5. Presence of a setting does not prove material live use.
6. Testing config is not Production authority.
7. Separate process configuration can be mistaken for AppConfig if not classified correctly.

## 17. Preserved cross-topic inputs for later implementation/design

### Secrets/environment
Keep secrets separate from business configuration; never expose values.

### A-011
State/lifecycle questions are deferred to A-011.

### Chartink
Production and Testing historically differed in scanner naming because Testing used a broker-ID marker. Future system needs a uniform naming/payload convention.

### S&R / filtering / shadow
Later review must cover strategy-specific filtration, score/rank mechanism, S&R and shadow mechanism. No Phase-1 redesign is implied.

### Capital / sizing / risk
A-005 remains authoritative. Preserve its broker-specific margin, real-cash basis, bucket, sizing, tier/multiplier and failure-alert inputs as future design material, not legacy facts.

### Broker/products/GTT
Later implementation must review duplicate/irrelevant/unused/placeholder settings.

### Alerts/reports/evidence
Legacy CSV reporting exists. Future GUI reporting and configuration-change evidence tracking remain design inputs.

### Configuration cleanliness
Future configuration should be clean, non-duplicated, non-placeholder and clearly separated by functional responsibility, including Intraday/Delivery sections where appropriate.

## 18. Final classification

**FACT:** layered config, eight-file AppConfig, separate strategy loader, strict validation, atomic load, no hot reload, snapshots, additional config-like files, Production/Testing differences.

**UNKNOWN / NOT ESTABLISHED:** normal `--config` usage, material use of every field, historical snapshot reliance, universal precedence, exhaustive hardcode absence, actual Production impact of defaults/fallbacks.

**HISTORICAL / KNOWN DEFECT:** config drift risk, manual/uncommitted edits, Paper/Live mismatch, Testing/Production divergence.

**FUTURE / V2 INPUT:** live-only, no business hardcodes, clean ownership, Intraday/Delivery segregation, secure secrets, uniform scanner naming, change/evidence register, stricter boot alerting, cleanup of duplicate/irrelevant/unused/placeholder settings after separate V2 review.

## 19. Freeze

**A-009 — Configuration Model: CLOSED / FROZEN.**

This freeze establishes the legacy baseline only and does not approve any legacy configuration for V2.
