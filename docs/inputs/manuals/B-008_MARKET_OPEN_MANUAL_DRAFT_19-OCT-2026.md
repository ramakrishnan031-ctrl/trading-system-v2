# B-008 — Market-Open Handling

**Status:** Draft — not frozen  
**Prepared:** 19-Oct-2026 (IST)  
**Scope:** Factual documentation of the DEAD legacy system only. No repair, revival, tuning, operational instructions, or V2 design.

> **Evidence caution:** The returned answer sheet contains detailed generated answers that Rama explicitly says are only a “torchlight” and must be verified. This chapter separates source, record and operator evidence. The accessible `main.py` describes itself as a “Trading System v2 Entry Point”; repository content alone does not prove what was installed or executed on the historical production host.

## 1. Evidence basis

| Ref | Source | Supported use | Limitation |
|---|---|---|---|
| E1 | `B-008_MARKET_OPEN_ANSWER_SHEET_17-OCT-2026(1).txt` | Rama's corrections and historical statements | Detailed generated answers are not conclusive; verify independently |
| E2 | *Gmail — [LFL836] Cron Morning Briefing — 24-Sep.pdf*, page 1 | Dated report at 09:20 on 24-Sep-2026; reported schedule completions and preflight status | PDF does not conclusively identify host or prove an order was placed |
| E3 | Same PDF, page 2 | 15 displayed strategy rows and their per-strategy windows | Does not establish actual order activity; global 10:00 floor is separate |
| E4 | `trading-system-prod-snapshot`, `main` | Current accessible source/configuration; `main.py` blob SHA observed: `445fb0d8237a6d535ed00e5eb968c8176efd74f3` | No single whole-tree commit pinned in this review; source does not prove deployed host state |
| E5 | `config/system_config.yaml` in accessible snapshot | `trading_hours.entry_start: "10:00"` and comments describing global entry floor | Exact historical deployment/version still requires confirmation |
| E6 | `config/cron_registry.yaml` and relevant scripts | Declared cron job registry and generation path | Declaration is not proof of installed crontab or execution |
| E7 | `03_STARTUP_RUNTIME_AND_SCHEDULER.md` and `03A_CRON_REGISTRY_TABLE.md` in `trading-system-records` | Existing schedule and testing-host capture | Both explicitly state TESTING VM evidence and that production was never read |

Evidence labels:
- **FACT — SOURCE-VERIFIED:** directly supported by inspected source.
- **FACT — RECORD-DERIVED:** supported by a dated artifact within its scope.
- **HISTORICAL — RAMA:** operator statement, not independently verified unless noted.
- **CONFLICT:** accounts disagree or an original generated answer has been corrected.
- **UNKNOWN / NOT ESTABLISHED:** evidence is insufficient.
- **INTERPRETATION / AUDIT LEAD:** requires further source/record verification.

## 2. Exchange open and system entry start are different

### 2.1 Exchange regular-session open

**HISTORICAL — RAMA:** The regular NSE session opens at **09:15 IST**.

### 2.2 Configured system entry start

**FACT — SOURCE-VERIFIED:** The accessible `config/system_config.yaml` declares:

```yaml
trading_hours:
  entry_start: "10:00"
```

The adjacent comments state that this is a **global floor** over each strategy's individual entry window. They explicitly explain that strategies configured with an intended 09:25 start cannot trade at 09:25 while the global floor is 10:00; both the global and strategy-specific windows must permit entry.

**HISTORICAL — RAMA:** The system starts trading/order processing at 10:00 according to the configuration.

**CORROBORATED WITH LIMIT:** Rama's correction agrees with the inspected configuration. The exact historical deployed version and every runtime consumer still need to be traced.

The supplied 24-Sep email's page 2 lists 15 strategies with individual intended windows of 09:25–15:00. Those rows do not prove that orders could be placed at 09:25: the inspected global 10:00 floor is stricter.

Use the terms:
- **Exchange regular-session open:** 09:15 IST.
- **Configured system entry start:** 10:00 IST.

Do not describe 10:00 as the exchange open.

### 2.3 What happens to signals before 10:00?

**UNKNOWN / NOT ESTABLISHED.**

Rama says signals/orders are not processed for trading before 10:00. The available evidence does not yet establish whether:
- scanning is disabled before 10:00;
- a signal is received but not evaluated;
- a signal is evaluated and rejected by a time guard;
- a signal is queued;
- a signal is dropped/discarded; or
- only order creation/submission is blocked.

These are different behaviours. Trace the actual path from Chartink ingress through configuration loading, signal processing, entry gating and order dispatch in a pinned source revision. If paths differ, document each separately.

## 3. Market, segment and strategy scope

**HISTORICAL — RAMA:** Signal input is 100% Chartink, with NSE Equity candidates. Rama explicitly corrects the original generated answer's claims about NFO options as wrong. For this chapter, the stated scope is **NSE / Equity**, with Zerodha as the broker/instrument route.

**CONFLICT — rejected generated content:** Initial answer-sheet claims about NIFTY/BANKNIFTY options, CE/PE subscriptions, option-chain selection and NFO execution are not accepted as facts.

The accessible repository contains `config/chartink_scanners.yaml` and `config/scan_webhook_map.yaml`. Their presence is source evidence that these configuration files exist, but it does not alone prove the complete production ingress path or deployed values.

**HISTORICAL — RAMA:** 16 current strategies.

**FACT — RECORD-DERIVED:** The 24-Sep email displays 15 strategy rows.

**UNKNOWN / NOT ESTABLISHED:** The difference between 16 (Rama's statement) and 15 (email display) is unresolved. Confirm the strategy registry/configuration in the pinned source snapshot. Do not silently equate the email's rows with the complete inventory.

## 4. Pre-market sequence and scheduled activity

### 4.1 Operator-reported sequence

**HISTORICAL — RAMA:**
1. System boots around **08:15 IST**.
2. Token refresh follows immediately, reportedly within about 1–2 minutes.
3. Pre-flight / health checks run.
4. The system waits for the **10:00 IST** configured entry start.
5. In an earlier stage, token refresh was performed locally and uploaded/copied to the VM using a `.bat` file; later operation became headless.

This is operator-reported chronology. Exact boot-versus-token-job timestamps need dated host journal and cron/job records.

### 4.2 Dated briefing evidence

**FACT — RECORD-DERIVED:** The supplied *Cron Morning Briefing — 24-Sep* PDF, page 1, is dated Thursday 24-Sep-2026 at 09:20 AM. It reports:
- Mode: Live.
- Pre-flight: `READY · warnings`.
- 0 critical, 1 warning, 0 auto-fixed.
- Run ID `20260924_091502_c`.
- Reported completed jobs: 08:15 `auto_refresh_token`; 08:20 `monitoring_canary`; 08:30 `preflight_phase_a`; 08:35 `fetch_fno_ban`; 08:55 `gemini_premarket_brief`; 09:00 `refresh_instruments`; 09:14 `preflight_phase_b`; 09:15 `preflight_phase_c`.
- The 09:20 `cron_officer_briefing` is listed as pending at the time of the briefing.
- Overall plan: 45 total, 18 done, 23 pending, 0 skipped (40%).

This shows what the briefing reported, not the underlying command output. The PDF does not conclusively identify its host, prove all job side effects, or establish that an order was placed.

### 4.3 Declared schedule versus actual execution

**FACT — SOURCE-VERIFIED:** The accessible `config/cron_registry.yaml` describes itself as the registry read by `core/cron_registry.py` and used to generate `deploy/cron/trading-system.cron` via `scripts/generate_crontab.py --generate`.

A registry declaration, generated crontab, installed host crontab, and reported job run are distinct evidence layers.

**FACT — RECORD-DERIVED:** Existing `03_STARTUP_RUNTIME_AND_SCHEDULER.md` and `03A_CRON_REGISTRY_TABLE.md` document the testing VM and explicitly say production was never read. Their schedule broadly aligns with the supplied 24-Sep email, including the 08:15 token refresh and preflight stages. This corroborates schedule design and reported output, not production host configuration.

### 4.4 Deprecated health-check script

**FACT — SOURCE-VERIFIED:** The accessible `scripts/premarket_healthcheck.py` labels itself **DEPRECATED** (header dated 2026-06-21) and says its checks were ported to `scripts.preflight.orchestrator` Phase A. It is described as a rollback safety net.

Do not describe this script as the active 08:30 job without further evidence; the email and registry refer to `preflight_phase_a`.

## 5. Weekends and exchange holidays

**HISTORICAL — RAMA:** The system does not start/boot on weekends or exchange holidays.

**FACT — SOURCE-VERIFIED:** The accessible `main.py` imports `utils.holiday_guard` functions including `is_trading_day`, and its module documentation includes holiday/weekend as a clean no-start outcome. `config/nse_holidays_2026.yaml` exists in the accessible snapshot.

**CORROBORATED WITH LIMIT:** The current accessible source is consistent with the operator statement, but it does not prove every historical production version or the installed production scheduler.

**CONFLICT — rejected generated answer:** The original answer sheet speculated that the service might boot, connect and idle with zero ticks on a holiday. Rama corrected this: the system does not start/boot on weekends/holidays. Do not present the idle-connected scenario as settled behaviour.

## 6. Feed, candles and first signal/order

The generated answer sheet asserts active ticks at 09:15, first 1-minute/5-minute candles at 09:16/09:20, pre-warmed historical candles, event-driven signal evaluation and per-symbol suppression for missing data. No dated raw tick/candle log was attached to substantiate those exact timings for production.

These remain **INTERPRETATION / AUDIT LEADS**, not established facts. The reviewer should trace:

1. Chartink ingress / webhook.
2. Scanner and symbol mapping.
3. Configuration loading and trading-window guard.
4. Signal evaluation and any candle/time conditions.
5. Entry gate / order eligibility.
6. Order creation and broker handoff.
7. Persisted logs/database evidence and broker records.

For each transition, document **SOURCE → INPUT → LOGIC/CALCULATION → VALIDATION/GUARD → OUTPUT → STORAGE/HANDOFF → NEXT CONSUMER**. Use exact function names and line references only after pinning the relevant commit.

## 7. Readiness, paper/live, alerts and failure claims

The generated answer sheet describes paper fills, live order routing, kill-switch behaviour, SQLite checks, fail-fast exits, Telegram messages and specific incidents. These are not all established by the evidence reviewed for B-008.

- The email reports Mode: Live, but it does not prove an order was placed.
- The email reports `READY · warnings`, 0 critical and 1 warning; it does not identify the warning or prove the effect on order permission.
- No Telegram delivery record was attached to establish a specific 08:55 readiness alert or 09:15 first-tick message.
- Existing B-007 reconciliation notes that `run_all_startup_checks` collects blocking failures and warnings, while the caller determines whether to exit. Do not copy the generated answer's blanket claim that every failed check calls `sys.exit(1)` into B-008.
- Existing startup/cron manuals describe TESTING VM evidence, not production installation or production paper/live parity.
- Claims about HTTP 429, WebSocket drops, SQLite locks, manual restarts, exact log messages and recovery times lack attached dated incident logs. Keep them as audit leads, not confirmed incidents.

This is a documentation chapter, not a recovery runbook. It does not endorse potentially destructive commands from the generated answer sheet.

## 8. End-to-end view — evidenced portion only

```text
24-Sep briefing reports:
  08:15 token refresh completed
       |
  08:30 / 09:14 / 09:15 preflight phases reported completed
       |
  Exchange regular-session open: 09:15 IST (Rama's statement)
       |
  Configured global entry floor: 10:00 IST (inspected config)
       |
  Exact pre-10:00 signal handling and first-order path:
  UNKNOWN / NOT ESTABLISHED until source trace + runtime evidence
```

This is an evidence map, not proof that the reported jobs and every code path ran on the production host in this exact order.

## 9. Open questions before freeze

| ID | Question | Evidence needed |
|---|---|---|
| B8-OQ1 | Which exact source revision was deployed during the relevant production period? | Pinned commit/deployment manifest and file hashes |
| B8-OQ2 | Did the 24-Sep email originate from production or testing? | Host identity in job logs / matching host capture |
| B8-OQ3 | Exact boot-versus-token-refresh order? | systemd journal and cron start/end timestamps |
| B8-OQ4 | What happens to Chartink signals before 10:00? | Full webhook-to-order call path and dated signal/decision/order example |
| B8-OQ5 | Why 16 strategies per Rama but 15 displayed in email? | Pinned strategy registry and email-generation source |
| B8-OQ6 | What does `READY · warnings` mean for order permission? | Preflight output, warning identity and caller gating decision |
| B8-OQ7 | What are the actual tick/candle timing rules? | Feed and bar aggregation code plus dated runtime logs |
| B8-OQ8 | Which alerts were delivered at market open? | Notification source and dated delivery records |
| B8-OQ9 | Did production installed cron/systemd match repository declarations? | Dated production crontab, unit/drop-in and deployment captures |

## 10. Safe conclusions

1. Exchange regular-session open and system entry start are different: 09:15 IST versus a configured 10:00 IST entry floor.
2. Rama's stated universe is NSE Equity via Chartink, not NFO options; the original generated options claims are rejected unless independently supported by source evidence.
3. The 24-Sep-2026 email reports token refresh at 08:15 and preflight/instrument jobs through 09:15, with Mode: Live and `READY · warnings`. Host identity and order outcome are not established by the PDF alone.
4. The later operator-reported sequence is boot around 08:15 → headless token refresh shortly afterward → preflight → wait for the 10:00 entry floor. Earlier local refresh plus `.bat` copy was an earlier stage.
5. Pre-10:00 signal disposition is unknown until the actual source path is traced.
6. Existing 03/03A manuals describe testing-VM evidence and explicitly state production was not read.

## 11. Related documents

- `01_SYSTEM_OVERVIEW.md`
- `02_REPOSITORY_AND_FILE_MAP.md`
- `03_STARTUP_RUNTIME_AND_SCHEDULER.md`
- `03A_CRON_REGISTRY_TABLE.md`
- B-001 VM / service startup
- B-004 Broker Connection
- B-005 Startup Reconciliation — parked
- B-006 Worker Initialization — separate
- B-007 Readiness / Health Gating — separate

**B-008 status: DRAFT / NOT FROZEN.** Freeze only after source trace, evidence reconciliation and Rama's review.
