# A-005 — CAPITAL MODEL

## Legacy System Technical Manual

### DEAD / REFERENCE ONLY

### 24-SEP-2026

\---

# 1\. PURPOSE

This chapter documents the legacy trading system's capital model and records
the separately identified future capital-flow principles confirmed during the
A-005 redesign discussion.

The legacy system is DEAD / REFERENCE ONLY.

This manual does not repair, tune, redesign, revive, or recommend the legacy
capital implementation.

Where a later intended flow is recorded, it is explicitly labelled as a
future design input and is not presented as historical legacy behaviour.

\---

# 2\. EVIDENCE AND FACT DISCIPLINE

Evidence used for this chapter:

1. Preserved Production repository working-tree documentation.
2. Preserved Testing/Sandbox repository evidence.
3. Production `config/system\_config.yaml` evidence.
4. `capital/position\_sizer.py` evidence.
5. `capital/fund\_manager.py` evidence.
6. State/reconciliation architecture evidence.
7. User's A-005 clarification files and capital-flow spreadsheet interpretation.
8. Broker screenshots supplied by the user.
9. Official Zerodha API/support documentation where broker behaviour was relevant.

Direct public GitHub retrieval was unavailable in the latest web pass. Therefore
the repository claims in this chapter rely on the preserved repository evidence
already loaded in the project; no fresh GitHub retrieval is implied.

Classification vocabulary:

* **FACT** — directly supported by source evidence.
* **HISTORICAL** — true for a documented period/version but not necessarily
timeless.
* **UNKNOWN / NOT ESTABLISHED** — evidence is insufficient.
* **KNOWN DEFECT / AMBIGUITY** — source evidence reveals an inconsistency or
unclear behaviour.
* **V2 DESIGN INPUT** — user-confirmed future requirement, not a legacy fact.
* **OPEN QUESTION** — intentionally deferred to another agenda.

\---

# 3\. CAPITAL ARCHITECTURE OVERVIEW

The preserved legacy architecture separates capital/risk admission from order
execution.

High-level path:

```text
Chartink signal
    |
    v
SignalProcessor
    |
    v
Screening / qualification
    |
    v
Price construction
    |
    v
PositionSizer
    |
    v
RiskEngine
    |
    v
FundManager.reserve()
    |
    v
OrderPlacer
    |
    v
Zerodha Adapter
    |
    v
Broker order
    |
    v
Fill / lifecycle
    |
    +--> FundManager commit
    |
    +--> trade OPEN
    |
    +--> SL / Target protection
    |
    v
Exit
    |
    v
FundManager release + realised P\&L
```

The critical legacy boundary is:

**Sizing → risk approval → capital reservation → order placement.**

Capital is therefore checked before the broker order is submitted.

\---

# 4\. SOURCE → INPUT → LOGIC → GUARD → OUTPUT → HANDOFF

## 4.1 Position sizing

### SOURCE

`capital/position\_sizer.py`

### INPUT

Documented inputs include:

* symbol
* direction/side
* entry price
* stop-loss
* intent/product
* score/tier
* FundManager capital snapshot
* optional lot-size information
* optional broker live-margin information.

### CALCULATION / LOGIC

The preserved documentation records a legacy formula family:

```text
risk\_per\_trade\_rs
    = total\_capital × effective\_risk\_pct

sl\_distance
    = abs(entry\_price - sl\_price)

qty\_by\_risk
    = floor(risk\_per\_trade\_rs / sl\_distance)

effective\_entry\_price
    = entry\_price × (1 + entry\_offset\_pct)

margin\_per\_share
    = effective\_entry\_price / leverage

qty\_by\_capital
    = floor(available\_bucket / margin\_per\_share)

qty\_by\_concentration
    = floor(
        total\_capital × effective\_concentration\_pct
        / entry\_price
      )

raw\_qty
    = min(qty\_by\_risk, qty\_by\_capital, qty\_by\_concentration)

tiered\_qty
    = floor(raw\_qty × tier\_multiplier)

final\_qty
    = floor-to-lot-size(tiered\_qty)
```

The sizer can use a broker live-margin percentage when available and has a
fallback/static-leverage path in the preserved legacy evidence.

### VALIDATION / GUARD

The quantity is bounded by the applicable risk, capital, concentration, tier,
position-value and lot-size constraints represented by the legacy
implementation/configuration.

### OUTPUT

A sizing result containing quantity and sizing/margin/risk breakdown information.

### HANDOFF

Sizing result → RiskEngine / FundManager → reservation.

\---

# 5\. LEGACY CAPITAL BUCKETS

The preserved system describes two separate capital domains:

1. Intraday
2. Positional / Delivery

Cross-bucket borrowing is documented as forbidden.

Production configuration contains bucket percentages and separate Delivery
controls.

The exact historical deployed percentage/value on every date is not treated as
one permanent value because the preserved configuration contains historical
comments/changes.

**UNKNOWN:** a complete dated parameter history for every deployment.

\---

# 6\. LEGACY LEVERAGE / MARGIN MODEL

The preserved legacy documentation records:

```text
margin = qty × price / leverage
```

The Production configuration contains a `leverage\_map` with product-specific
values, including entries for INTRADAY, DELIVERY, COVER\_ORDER and
BRACKET\_ORDER.

This establishes the existence of product-specific leverage configuration.

It does **not**, by itself, establish that every configured product was
historically traded.

The PositionSizer also documents a broker live-margin lookup with fallback to
static leverage when the live API path is unavailable.

### Classification

**FACT / HISTORICAL:** static leverage and live-margin lookup mechanisms existed.

**V2 DESIGN INPUT:** future sizing should use broker-required margin percentage
rather than inventing leverage.

\---

# 7\. FUND MANAGER

## SOURCE

`capital/fund\_manager.py`

## ROLE

FundManager is the documented capital-ledger authority.

## CORE INVARIANT

```text
available + reserved + used == total
```

## CAPITAL STATES

```text
AVAILABLE
    |
    | reserve
    v
RESERVED
    |
    | entry fill
    v
USED
    |
    | exit
    v
AVAILABLE + realised P\&L
```

Pre-fill cancellation/rejection:

```text
RESERVED → AVAILABLE
```

## RESERVE FLOW

The preserved source documentation records:

1. Determine bucket.
2. Calculate required margin.
3. Add configured SL-M buffer where applicable.
4. Check availability.
5. Write reservation ledger row.
6. Apply reservation.
7. Check invariant.
8. Return reservation ID.

## LEDGER

Every capital mutation writes to `fm\_ledger`.

The documented write-ahead sequence is:

```text
write ledger row
    ↓
apply state mutation
    ↓
check invariant
```

This supports recovery/replay after restart.

\---

# 8\. CAPITAL ADMISSION

The preserved signal-processing flow is:

```text
PositionSizer
   ↓
portfolio lock
   ↓
per-strategy position cap
   ↓
one trade per symbol once a day
   ↓
RiskEngine.approve()
   ↓
FundManager.reserve()
   ↓
signal RESERVED
   ↓
OrderPlacer
```

The protected/atomic region exists to prevent concurrent Chartink signal bursts
from both passing stale capital/exposure checks.

This is a documented legacy safety characteristic.

\---

# 9\. FILL / COMMIT / RELEASE

After an entry fill:

```text
RESERVED
   ↓
USED
```

The preserved architecture states that actual fill quantity and actual fill
price are used for the commit.

After an exit:

```text
USED
   ↓
release
   ↓
available + realised P\&L effect
```

If an order is rejected/cancelled before fill:

```text
RESERVED
   ↓
AVAILABLE
```

This is distinct from the future intended rule concerning daily filled-trade
count.

\---

# 10\. BROKER RECONCILIATION

The legacy `OrderReconciler` continuously compares local state with broker state.

The preserved architecture states:

* broker truth is used for reconciliation;
* local DB state can lag broker state;
* capital drift is detected;
* reconciliation can repair state/capital;
* dangerous inconsistencies can trigger protective behaviour.

The reconciler is therefore a separate protection/recovery path.

**IMPORTANT:** The future user-confirmed daily sizing rule says reconciliation
must not rebase today's locked sizing capital.

That is a future design input, not a retroactive statement about every legacy
reconciliation path.

\---

# 11\. LEGACY CONFIGURATION CONTROLS

Preserved Production configuration includes capital-related controls such as:

* `capital.intraday\_bucket\_pct`
* `capital.positional\_bucket\_pct`
* `conditional\_allocation\_enabled`
* `capital.slm\_margin\_buffer\_pct`
* `capital.sl\_limit\_offset\_pct`
* `capital.gtt\_sl\_limit\_offset\_pct`
* `capital.emergency\_exit\_buffer\_pct`
* `leverage\_map`
* intraday maximum open positions
* intraday maximum daily trades
* delivery maximum open positions
* delivery maximum daily trades
* risk-per-trade percentage
* concentration limits
* position-value limits
* tier multipliers
* daily loss limits
* consecutive-loss limits
* Delivery-specific sizing controls.

The exact runtime effect of every key must be taken from the dated source/config
when needed. Comments in the configuration also record historical changes.

A key historical point is that the preserved Production configuration states
that maximum daily trades also served as the sizing divisor in the legacy model.

\---

# 12\. HISTORICAL CONFIGURATION CHANGES

The preserved Production configuration contains historical notes including:

* intraday maximum open positions changed from earlier values;
* intraday maximum daily trades changed from an earlier value;
* delivery maximum open positions changed;
* delivery maximum daily trades changed;
* sizing-divisor semantics were explicitly introduced/changed in the documented
historical period;
* `risk\_per\_trade\_pct` is documented as inert for sizing after a historical
change.

These must be treated as historical evidence rather than silently converted
into one permanent rule.

\---

# 13\. RISK AND CAPITAL CONTROLS

Legacy capital-related constraints include:

### Risk

* risk percentage
* daily loss limit
* consecutive-loss limit
* tier multiplier.

### Exposure

* concentration percentage
* position-value percentage
* open-position limits.

### Capital

* bucket allocation
* capital availability
* margin/leverage
* reservation.

### Classification

These are **legacy controls**.

They are not automatically approved for V2.

\---

# 14\. FINAL CONFIRMED FUTURE CAPITAL FLOW

The user supplied a separate capital-flow design explanation and confirmed it.

It is recorded here as a **V2 DESIGN INPUT**, not as a legacy fact.

```text
BROKER 09:15 OPENING CASH
          ↓
CONFIGURED INTRADAY / DELIVERY SPLIT
          ↓
LOCK SEGMENT CAPITAL FOR DAY
          ↓
MAX FILLED-TRADE DIVISOR
          ↓
BASE MAX CASH ALLOCATION / TRADE
          ↓
ACTUAL CASH ALLOCATION
= MIN(BASE MAX, CURRENT AVAILABLE SEGMENT CAPITAL)
          ↓
BROKER REQUIRED MARGIN %
          ↓
PERMITTED NOTIONAL
= CASH ALLOCATION / MARGIN %
          ↓
QUANTITY
= FLOOR(PERMITTED NOTIONAL / ENTRY PRICE)
          ↓
ORDER
          ↓
FILLED?
  ├── NO → no filled-trade slot consumed; capital remains available
  └── YES
       ↓
      TRADE
       ↓
   EXIT / RESULT
       ↓
SEGMENT CAPITAL AVAILABLE UPDATED
       ↓
NEXT TRADE
```

\---

# 15\. FUTURE CAPITAL FORMULAS

## 15.1 Base maximum

```text
base\_max\_cash\_per\_trade
    = locked\_segment\_capital
      / configured\_max\_filled\_trade\_divisor
```

## 15.2 Actual cash allocation

```text
actual\_cash\_allocation
    = MIN(
        base\_max\_cash\_per\_trade,
        current\_available\_segment\_capital
      )
```

This is the final Q3 resolution.

It means:

* if available capital is greater than the normal allocation, only the normal
allocation is used;
* if available capital is lower, only the lower available amount is used.

This prevents the last trade(s) from assuming capital that no longer exists
after earlier realised losses.

## 15.3 Broker permitted notional

```text
permitted\_notional
    = actual\_cash\_allocation
      / broker\_required\_margin\_pct
```

The spreadsheet wording “Capital available after Margin” is therefore
documented here as **permitted notional**, not remaining cash.

## 15.4 Quantity

```text
quantity
    = FLOOR(
        permitted\_notional / intended\_entry\_price
      )
```

Then apply the actual instrument/lot constraints required by the applicable
implementation.

\---

# 16\. DAILY FILLED-TRADE LIMIT

Confirmed future rule:

* maximum daily trade count = broker-filled trades;
* pending/queued orders do not count as filled;
* cancelled orders do not count;
* rejected orders do not count;
* the system may continue processing later candidates after cancellation/rejection;
* processing stops for new trades once the configured filled-trade maximum is
reached.

This is separate from capital/margin calculation.

A pending order can still be relevant to broker margin/execution safety even
though it is not a completed trade for the daily count.

\---

# 17\. DAILY MARGIN SNAPSHOT

Confirmed future sequence:

```text
BOOT
 ↓
TOKEN / INSTRUMENT UPDATE
 ↓
MARGIN DB REFRESH (\~08:30)
 ↓
SUCCESS / FAILURE ALERT
 ↓
09:15 OPENING BALANCE
 ↓
CAPITAL SPLIT
 ↓
LOCK
 ↓
TRADING
```

The daily margin snapshot is the sizing reference.

The order-time broker API is the final execution safety/validation authority.

If both sources fail to provide usable margin information:

```text
required cash = 100%
```

No leverage is invented.

An incident alert is required.

\---

# 18\. ENTRY VS EXIT CAPITAL

Confirmed future rule:

**Entry broker margin is the primary capital-sizing basis.**

SL and Target do not automatically receive independent capital allocations.

However, actual broker/product behaviour may require different treatment.
Therefore exact Zerodha product/order behaviour must be verified before
implementation.

The legacy manual must not freeze a simplistic “divide by three” rule.

\---

# 19\. DAILY CAPITAL LOCK

Confirmed future rule:

```text
09:15 broker opening balance
        ↓
segment allocation
        ↓
daily sizing basis locked
```

Later reconciliation:

```text
detect/report drift
```

but:

```text
DO NOT rebase today's sizing capital
```

Next day:

```text
new broker opening balance
        ↓
new daily basis
```

This preserves the user's stated requirement that the morning broker state is
the new day's source of truth.

\---

# 20\. CONFIG VS STRATEGY YAML

Confirmed governance input:

### Maximum

```text
effective maximum
    = LOWER(global/config maximum,
            strategy maximum)
```

### Minimum

```text
effective minimum
    = HIGHER(global/config minimum,
             strategy minimum)
```

This is a generic precedence principle.

It must be applied only where the corresponding setting actually exists at both
levels and has the same semantic scope.

Before implementation, every existing config and strategy setting must be
reviewed and deliberately assigned to:

* global/config scope;
* strategy scope;
* broker/instrument scope;
* operational scope;
* or removed/deferred.

No Python hardcoding of business values.

\---

# 21\. PAPER VS LIVE

The preserved legacy evidence indicates:

* a broker-margin live lookup path exists;
* static leverage can be used as fallback;
* the broker adapter has a paper/live boundary.

However:

**UNKNOWN:** complete behavioural parity between paper and live for every
capital path.

The manual therefore does not claim parity without specific evidence.

\---

# 22\. BROKER SCREENSHOT EVIDENCE

## 23-Sep-2026 \~20:00 IST

Funds:

* opening balance: ₹10,384.30
* available margin/cash: ₹10,351.12
* used margin: ₹33.18
* commodity: zero.

Positions:

* five displayed instruments;
* quantity 0 on the displayed positions;
* CNC and MIS examples;
* total P\&L: -₹33.18.

## 24-Sep-2026 \~12:25 IST

Funds:

* opening balance: ₹10,347.80
* available margin/cash: ₹10,124.90
* used margin: ₹222.90
* commodity: zero.

Position:

* RELTD, EQ, MIS;
* quantity -3;
* average ₹147.76;
* LTP ₹148.02;
* P\&L -₹0.78.

Orders:

* open MIS orders;
* completed/cancelled orders;
* CNC and MIS examples.

These are evidence of broker state at the stated times only.

They do not establish the future capital model.

\---

# 23\. UNKNOWN / NOT ESTABLISHED

The following are intentionally not invented:

1. Complete dated history of every capital configuration value.
2. Exact deployed value of every optional PositionSizer setting.
3. Exact runtime wiring of every live-margin branch in every deployment.
4. Complete historical `fm\_ledger` schema/event taxonomy.
5. Complete paper/live parity.
6. Exact historical use of every configured product type.
7. Exact historical use of every concentration/tier control.
8. Exact historical relationship between every config comment and deployed
runtime behaviour.
9. Exact product-specific broker margin behaviour for every supported order
path.

\---

# 24\. KNOWN DEFECTS / AMBIGUITIES

### 24.1 Terminology

The spreadsheet's phrase “Capital available after Margin” can be misleading.
The calculation represents permitted/derived notional.

### 24.2 Historical leverage

Legacy static leverage and live-margin lookup coexist in the preserved
implementation evidence.

### 24.3 Historical values

Configuration comments document changes over time.

### 24.4 Capital basis ambiguity

Earlier project discussions contained different interpretations of total
capital versus bucket capital. The manual therefore relies on dated source
evidence and does not silently normalise conflicting historical statements.

\---

# 25\. CROSS-TOPIC / FUTURE AGENDA INPUTS

## RETAINED HERE TO PREVENT INFORMATION LOSS

These points were discussed during A-005 but belong to later agenda topics.

### 25.1 Configuration design

* Deeply review existing `config.yaml`.
* Deeply review every `strategy.yaml`.
* Separate global and strategy settings deliberately.
* Do not automatically retain every legacy setting.

### 25.2 Margin data operations

* Daily replace/refresh margin DB.
* Avoid stale/duplicate margin records.
* Success and failure alerts.
* Stock/trade-identifiable margin incidents.
* Dated margin/eligibility snapshots for audit.
* Silent repeated failures unacceptable.

### 25.3 Observability

* Periodic capital-drift alerts.
* Telegram/email for critical operational events.
* Preserve evidence for later forensic review.

### 25.4 Broker verification

Before implementation:

* verify current Zerodha margin behaviour;
* verify product-specific margin;
* verify pending/open-order treatment;
* verify GTT trigger funding;
* verify entry/exit order margin treatment;
* verify API response semantics.

### 25.5 Constraint governance

* Maximum = lower applicable bound.
* Minimum = higher applicable bound where a meaningful minimum exists.
* Maximum quantity can independently bind.
* Exact setting-by-setting precedence must be established during configuration
review.

### 25.6 General redesign rule

All future agenda topics should be reviewed again before V2 design.
These carry-forward notes are intentionally retained so the user does not need
to remember and repeat them later.

\---

# 26\. SOURCE → CONSUMER SUMMARY

|Source|Input|Processing|Output|Next consumer|
|-|-|-|-|-|
|Broker/account|opening balance|daily basis|opening cash|capital split|
|Config|bucket percentages|split|segment capital|trade allocator|
|Config/strategy|trade divisor|divide segment|base max/trade|allocation|
|FundManager|current available capital|MIN(base,current)|actual cash allocation|margin sizing|
|Margin DB|broker required %|cash / margin %|permitted notional|quantity|
|Market price|intended entry|notional / price|quantity|order/risk|
|Broker API|order-time margin|validate proposed order|pass/reject/adjust|execution|
|Broker fill|actual fill|commit|used capital|trade lifecycle|
|Exit/result|realised P\&L|release/update|available segment capital|next trade|
|Filled-trade counter|broker fills|count|daily count|entry gate|

\---

# 27\. FINAL LEGACY MANUAL STATEMENT

The legacy system implemented a capital/risk subsystem centred on
`PositionSizer` and `FundManager`, with separate capital buckets, margin/
leverage handling, pre-order reservation, write-ahead capital ledger mutations,
fill-based capital commitment, release after exit, and broker reconciliation.

The user's later intended flow for the redesigned system is separately captured
as:

```text
09:15 opening cash
→ segment split
→ locked segment
→ max-filled-trade divisor
→ base cash/trade
→ MIN(base cash/trade, current available segment capital)
→ broker margin %
→ permitted notional
→ quantity
→ filled trade
→ realised result
→ updated available segment capital
→ next trade
```

No part of this future flow should be treated as proof of legacy behaviour.

**A-005 — CLOSED / FROZEN.**

NO IMPLEMENTATION CHANGE IS IMPLIED.



======================================================================
NEW SYSTEM CHANGE — EXECUTION MODE
===

New system execution mode: LIVE ONLY; paper mode is not required.



NEW SYSTEM CHANGES — STRICT SCOPE

1. One trade per symbol once a day, irrespective of direction.
2. Available-capital realised P\&L effect = realised LOSS effect only; profit and trading expenses are adjusted after market EOD around midnight.
3. Quantity remains risk/SL based: expected loss at SL determines quantity (existing old/legacy same logic); maximum capital per trade remains an independent ceiling. The permitted-notional / intended-entry-price quantity formula is NOT used.
4. Live operation only; paper mode is not required.
No other A-005 logic is changed by this revision.

