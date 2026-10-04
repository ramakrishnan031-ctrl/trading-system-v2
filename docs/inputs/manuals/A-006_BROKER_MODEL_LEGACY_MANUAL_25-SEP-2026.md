# A-006 — BROKER MODEL
## Legacy Technical Manual + New-System Broker Design Inputs
**Date:** 25-Sep-2026

> The legacy system is DEAD / REFERENCE ONLY. This manual documents the
> broker model as evidenced and preserves the user's NEW-SYSTEM suggestions.
> Suggestions are not immutable implementation decisions.

## 1. Scope

Broker boundary:

```text
System
  -> authentication/session
  -> funds/margin
  -> instrument/eligibility
  -> order submission
  -> acknowledgement/status
  -> fills
  -> positions
  -> exits/protection
  -> reconciliation
  -> evidence
  -> Zerodha
```

Production is the primary legacy baseline. Testing/Sandbox is comparison
evidence only.

## 2. Evidence

Primary:
- https://github.com/ramakrishnan031-ctrl/trading-system-prod-snapshot

Comparison:
- https://github.com/ramakrishnan031-ctrl/trading-system-sbx-snapshot

Records:
- https://github.com/ramakrishnan031-ctrl/trading-system-records

Preserved project evidence:
- CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt
- 02_REPOSITORY_AND_FILE_MAP.md
- 03_STARTUP_RUNTIME_AND_SCHEDULER.md
- system_config.yaml
- A-004 records
- A-005 records
- completed A-006 checklist

Current external verification:
- Zerodha states BO/Bracket Order is no longer supported.
- Zerodha documents GTT for CNC, NRML and MTF, including OCO target+stoploss.
- Kite Connect exposes margins, orders/order history, positions and order
  modification/cancellation.
These current facts do not rewrite historical legacy behaviour.

# PART I — LEGACY BROKER MODEL

## 3. Broker Identity

Production configured:

```text
broker.primary = zerodha
broker.fallback = angelone
fallback_enabled = false
```

Therefore:
- Zerodha = configured primary.
- Angel One = disabled fallback configuration.
- Actual Angel One live use = UNKNOWN / NOT ESTABLISHED.

Main broker adapter:
`broker/zerodha_adapter.py`

## 4. Broker-Connected Legacy Components

| Component | Legacy role |
|---|---|
| `broker/zerodha_adapter.py` | Zerodha API adapter |
| `capital/position_sizer.py` | risk/capital quantity calculation; optional broker margin lookup |
| `capital/fund_manager.py` | capital ledger/reservation authority |
| `orders/order_placer.py` | order construction/submission |
| `orders/order_manager.py` | order lifecycle |
| `orders/order_monitor.py` | fill/status monitoring |
| `orders/order_reconciler.py` | local-vs-broker reconciliation |
| `config/system_config.yaml` | broker/product/capital settings |

The preserved baseline establishes broker interaction for order placement,
fills, reconciliation, capital lifecycle, margin lookup where wired and
rate limiting.

## 5. Legacy Product Model

```text
INTRADAY       -> MIS
DELIVERY       -> CNC
COVER_ORDER    -> CO
BRACKET_ORDER  -> empty / unsupported
```

Configuration proves configured capability/handling, not actual historical
use of each product.

## 6. Legacy Paper/Live

Legacy repository contains paper/live implementation.

NEW SYSTEM requirement recorded separately:
`LIVE ONLY`.

Paper mode is historical evidence only.

# PART II — NEW SYSTEM BROKER MODEL

## 7. Broker Selection

User suggestion:
`ZERODHA ONLY`.

Reason: Angel One was initially planned but dropped.

Classification: V2 design input.

## 8. Broker Authority

User suggestion:
Broker is authoritative for live:
- cash/funds
- opening balance
- orders/status
- fills
- positions/average price
- realised/unrealised P&L
- margin/required funds
- product/eligibility
- broker IDs

Reason recorded: broker is final authority at EOD.

## 9. Local Database Authority

User did not decide whether DB is audit/state-only or co-equal.

Instruction:
Choose best fit during implementation with Claude.

Classification:
**OPEN IMPLEMENTATION DECISION.**

## 10. Token / Session

Legacy has Zerodha token/session handling and token monitoring/update.

User suggestion:
- retain Zerodha workflow subject to current API requirements;
- historical workflow used headless + manual creation/upload;
- redesign to headless/no-manual workflow if technically possible.

No credentials are reproduced.

## 11. Authentication Failure

Suggested behaviour:

```text
authentication failure
 -> reject new entries
 -> continue protection/reconciliation of existing positions where possible
 -> immediate Telegram + Email
```

Global alert suggestion:
where an alert is required, use both Telegram and Email.

# PART III — PRODUCTS / GTT

## 12. Intraday

Intended product: MIS, subject to current broker eligibility.

User deliberately did not freeze the exact eligibility rule.

## 13. Delivery

User wants Delivery/GTT.

Reason:
broker-managed future-day target/SL is preferred over unnecessary application-side
long-running delivery exit logic.

Current Zerodha documentation supports GTT for CNC and OCO target+stoploss,
but exact new-system workflow still requires compatibility verification.

## 14. Cover Order

CO may be considered, subject to current eligibility and implementation fitment.

Historical CO configuration does not prove historical actual use.

## 15. Bracket Order

Legacy contains an empty/unsupported BO mapping.

Current Zerodha verification states BO is no longer supported.

Therefore:
**NEW SYSTEM must not assume BO support.**

# PART IV — ENTRY / EXIT

## 16. Entry Order Type

User intentionally did not choose MARKET/LIMIT/SL-LIMIT/SL-MARKET.

Instruction:
use best-fit/broader options and discuss with Claude during implementation.

Classification:
**OPEN.**

## 17. Proposed Entry Flow

```text
Chartink
 -> strategy-specific adapter/processor
 -> strategy filtering
 -> entry validation/reject
 -> config.yaml / strategy YAML
 -> position sizing
 -> suitable entry window
 -> broker order
```

User noted that the existing central/global score/rank allocator concept needs
reconsideration. This is future architecture input, not A-006 freeze.

## 18. Intraday Target

User suggestion:
strategy-dependent target based on strategy YAML R:R.

## 19. Delivery Target/SL

User suggestion:
GTT.

## 20. Protective SL

User suggestion:
broker-side protective SL whenever broker/product rules permit.

## 21. SL Failure

User suggestion:
retry + bounded escalation.

Exact escalation remains OPEN.

## 22. EOD Square-Off

User suggestion:
system-controlled before broker cutoff, with a two-check concept:

```text
square-off
 -> first confirmation
 -> wait about 1–2 minutes
 -> second confirmation
 -> broker cutoff
```

The interval is a suggestion, not a frozen V2 value.

# PART V — MARGIN / CAPITAL

## 23. A-005 Handoff

Carry-forward principles:
- one real-cash basis;
- 09:15 broker opening balance authoritative for the day;
- configured Intraday/Delivery buckets;
- daily capital lock;
- broker-specific margin, no invented universal leverage;
- daily margin reference authoritative;
- order-time broker margin/API final validation;
- eligibility and margin calculation separate;
- no margin data -> 100% cash requirement + Telegram/Email alert;
- entry broker margin primary;
- no separate SL/Target capital allocation unless broker requires;
- dated margin/eligibility snapshots;
- no business values hardcoded in Python;
- reconciliation may report drift but must not silently rebase today's basis;
- next day's opening balance becomes next day's basis.

## 24. Margin Change at Order Time

User suggestion:
if order-time margin differs from morning data, reduce/adjust quantity within all
existing constraints.

## 25. Funds

Opening cash:
trading basis.

Available cash:
capital-drift comparison and alerts.

## 26. Margin Evidence

Store order-specific broker margin response with date, so later the system can
explain the quantity permitted on that date.

## 27. Charges

User selected contract/settlement reports for charges/fees.

Clarification:
next morning opening balance is expected to already include prior broker
adjustments; therefore no separate intraday charge-sizing rule was frozen.

# PART VI — ORDER / FILL

## 28. Submitted State

User did not decide whether submitted means only after broker order ID.

Reason:
queued/cancelled/rejected orders may also have broker identifiers.

Classification:
**OPEN.**

## 29. Fill Authority

Broker fill data is authoritative for:
- filled quantity
- average fill price
- partial fills
- order status
- broker order ID

## 30. Partial Fill

Suggested:

```text
partial fill
 -> continue waiting for remainder
 -> manage filled quantity
 -> if SL/TGT hits for filled quantity:
      cancel unfilled remainder
 -> alert partial-fill status
```

Exact state-machine design remains open.

## 31. Timeout

Suggested:
reconcile first, then alert.

Reason:
order may remain queued at broker until intended price is reached.

Legacy 60-second timeout is not a V2 decision.

# PART VII — RATE LIMIT / API FAILURE / RETRY

## 32. Rate Limit

No user selection between centralized, endpoint-specific or broker-configured
limiting.

OPEN.

## 33. API Failure

No endpoint-specific final answers were given.

OPEN:
- authentication
- funds/margin
- order placement
- order status
- position
- market data

Global safety principle remains:
no unsafe new entry when required broker control is unavailable; protect/
reconcile existing positions where possible; alert via Telegram + Email.

## 34. Retry

Retry architecture was intentionally left open.

Do not copy legacy retry values blindly.

Retries must be designed to avoid duplicate orders.

# PART VIII — RECONCILIATION / POSITION

## 35. Legacy Reconciliation

`orders/order_reconciler.py` independently compares local state with Zerodha
broker state and participates in repair/capital paths.

Legacy fact only; not automatically retained in V2.

## 36. V2 Frequency

OPEN.

## 37. V2 Authority

Broker is authoritative for live broker facts, but the detailed conflict
resolution state machine is OPEN.

## 38. Automatic Actions

User did not choose automatic reconciler actions.

OPEN. Do not silently promote legacy actions.

## 39. Position Source

User suggestion:
broker + local state, broker authoritative.

## 40. Position Identity

User suggestion:

```text
symbol + product (MIS/GTT) + direction (Long/Short)
```

## 41. Manual Broker Intervention

Suggested:
detect + reconcile + alert.

# PART IX — ELIGIBILITY / INSTRUMENT MASTER

## 42. Pre-Order Eligibility

Exact checklist intentionally left open.

Must be established from current broker/API rules rather than guessed.

## 43. Instrument Master

User confirmed:
daily broker instrument refresh before market opening, following existing cron
principle where appropriate.

Failure -> Telegram + Email alert.

Exact V2 refresh time/mechanism remains open.

# PART X — LIVE ONLY

## 44. Confirmed

```text
NEW SYSTEM = LIVE ONLY
PAPER MODE = NOT REQUIRED
```

Historical paper mode remains evidence only.

# PART XI — TESTING / SANDBOX

User did not freeze a testing method. "All applicable" was explicitly only a
random answer.

Therefore OPEN.

Do not assume Zerodha has a separate live-order sandbox.

# PART XII — ERROR / AUDIT

## 45. Broker Rejection Evidence

Where available retain:
- broker error code
- error message/category
- symbol
- strategy
- intended order
- quantity
- price
- product
- margin/eligibility context
- timestamp
- broker order ID
- system decision/recovery action

Never fabricate unavailable fields.

## 46. Retry Identity

Every retry gets a new explicit attempt identity while preserving original
order/trade linkage.

## 47. Broker Audit Evidence

Where available retain:
- broker order ID
- exchange order ID
- submission request
- broker acknowledgement
- order status history
- fill history
- margin response
- broker position snapshot
- funds snapshot
- rejection/error
- reconciliation result
- timestamps

Broker evidence follows the long-term NEW-SYSTEM audit/retention policy.

# PART XIII — FACT / UNKNOWN REGISTER

## FACT / HISTORICAL
- Zerodha primary.
- Disabled Angel One fallback configuration.
- Zerodha adapter.
- Legacy MIS/CNC/CO/unsupported BO mapping.
- Broker reconciliation/fill monitoring.
- Paper/live implementation.

## CURRENT EXTERNAL BROKER FACT
- BO is currently unsupported.
- GTT currently supports CNC/NRML/MTF and OCO target+SL.

## UNKNOWN / NOT ESTABLISHED
- Angel One historical live use.
- Historical CO/GTT actual use.
- Local DB authority.
- Exact token automation.
- MIS/CO eligibility matrix.
- Entry order type.
- Exit architecture.
- Rate limits.
- Endpoint failure actions.
- Retry policy.
- Reconciliation frequency/actions/state machine.
- Exact eligibility matrix.
- Testing method.
- Exact GTT implementation.
- SL-failure escalation.
- Order acknowledgement/status semantics.

# PART XIV — CROSS-TOPIC / FUTURE AGENDA INPUTS

The user explicitly asked that non-A-006 points not be lost.

1. All user/business-controlled values must be configuration-driven, not
   hardcoded in Python.
2. Telegram + Email is the intended global alert pair wherever an alert is
   required.
3. Broker is source of truth for live broker state.
4. Daily capital basis is locked and must not be silently rebased.
5. Margin snapshots are dated/auditable.
6. Eligibility and margin calculation are separate.
7. Actual filled quantity/price must be distinguished from intended values.
8. Product semantics must be explicit.
9. User does not want to guess technical answers merely to complete a checklist;
   use repository/API evidence and Claude discussion.
10. Delivery future-day exits should use broker-managed GTT where compatible.
11. A-005 carry-forward:
    - one trade per symbol once a day;
    - available + realised-loss effect for running capital flow;
    - prior expenses/profit reflected by broker next opening balance;
    - quantity based on expected SL loss/risk-distance, not permitted-notional /
      entry-price;
    - maximum capital per trade remains capital-flow constraint;
    - live-only.
12. Existing central/global score/rank allocator is to be reconsidered in future
    strategy/filtering architecture.
13. Do not turn suggestions into code until the relevant agenda topic is
    discussed/frozen.

# PART XV — FINAL STATEMENT

A-006 documents the DEAD legacy broker model and records the user's current
NEW-SYSTEM broker intentions. Several technical details remain deliberately
open because the user chose not to guess. They must be resolved later using
repository evidence, current Zerodha documentation/API behaviour, Claude/user
discussion and testing evidence.

**A-006 DOCUMENTATION RECORD = CLOSED/FROZEN.**

**NEW-SYSTEM TECHNICAL IMPLEMENTATION DETAILS = PARTLY OPEN.**

END OF A-006 MANUAL
