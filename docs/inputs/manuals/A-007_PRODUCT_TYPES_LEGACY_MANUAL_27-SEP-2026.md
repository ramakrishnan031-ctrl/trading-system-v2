# A-007 — PRODUCT TYPES — LEGACY SYSTEM MANUAL

**Date:** 27-Sep-2026  
**Status:** CLOSED / FROZEN — LEGACY DOCUMENTATION  
**System:** DEAD / REFERENCE ONLY

## 1. Scope and evidence discipline

This chapter records product behavior as established by preserved repository
and user historical evidence. It does not design V2.

Evidence labels: **FACT**, **HISTORICAL USER EVIDENCE**, **UNKNOWN / NOT
ESTABLISHED**, **KNOWN LIMITATION**, **CURRENT EXTERNAL BROKER FACT**, and
**FUTURE V2 INPUT**.

## 2. Product model

The legacy system has a semantic product layer:

```text
Semantic Trading Intent
        |
        v
ProductResolver
        |
        v
Broker Product Code
        |
        v
Zerodha
```

Preserved reverse mapping:

| Broker product | Semantic intent |
|---|---|
| MIS | INTRADAY |
| CO | COVER_ORDER |
| CNC | DELIVERY |
| NRML | DELIVERY |

Preserved configured mapping:

| Intent | Broker product |
|---|---|
| INTRADAY | MIS |
| DELIVERY | CNC |
| COVER_ORDER | CO |
| BRACKET_ORDER | empty / unsupported |

The presence of an implementation path does not prove historical Production
use.

## 3. INTRADAY / MIS

**Status:** Actually traded — historical user evidence.

Trace:

```text
Strategy
 -> INTRADAY intent
 -> ProductResolver
 -> MIS
 -> capital/risk admission
 -> Zerodha order
 -> intraday position
 -> protection/monitoring
 -> MIS/EOD squareoff
```

Repository facts:
- MIS maps to INTRADAY.
- MIS belongs to intraday capital treatment.
- MIS is eligible for emergency/EOD flattening.
- Dedicated MIS auto-squareoff and EOD mechanisms exist.

Historical user evidence:
- MIS was actually traded.
- Intraday positions were intended to close the same day.
- Intraday margin differed by stock/category.

The legacy `leverage_map` value of 5.0 is historical implementation evidence,
not a V2 margin rule.

## 4. DELIVERY / CNC / GTT

**Status:** Actually traded — historical user evidence.

Trace:

```text
Strategy
 -> DELIVERY intent
 -> CNC
 -> broker entry
 -> Delivery position
 -> CncGttPlacer
 -> broker OCO GTT
 -> CncGttMonitor / reconciliation
```

Repository facts:
- CNC maps to DELIVERY.
- CNC is deliberately absent from `EMERGENCY_FLATTEN_PRODUCTS`.
- EOD intraday squareoff does not flatten CNC.
- `FullEntryEngine` contains a Delivery-specific deferred-exit path.
- `CncGttPlacer` can create the broker-side OCO GTT.
- `CncGttMonitor` reconciles broker GTT state.

Historical user evidence:
- CNC worked correctly and was actually used.
- GTT worked correctly.
- Delivery could remain overnight.
- Manual Delivery exits were possible and later detected/reconciled.

## 5. COVER_ORDER / CO

**Status:** Implemented; actual historical Production use UNKNOWN.

Repository facts:
- CO maps to COVER_ORDER.
- Dedicated CO protocol exists.
- CO entry uses broker variety `co`.
- CO has dedicated cancellation/squareoff handling.
- CO is in the emergency flatten set.

The preserved CO protocol is a two-phase mechanism: CO entry first, followed
by target handling where the configured protocol requires it.

Historical user evidence says intraday trades had automated protective TGT/SL
behavior, but the user cannot recall whether the actual entry mechanism was CO
or BO.

Therefore actual historical CO use is not frozen as fact.

## 6. BRACKET_ORDER / BO

**Status:** Internal intent exists; actual historical use UNKNOWN.

Repository facts:
- BRACKET_ORDER exists as an internal intent.
- A legacy leverage-map entry exists.
- Preserved configured broker mapping is empty/unsupported.

No historical Production BO use is established.

## 7. NRML

`NRML -> DELIVERY` exists in the semantic reverse mapping.

Historical user evidence says NRML was probably never submitted and Delivery
was handled through GTT.

Final classification: actual NRML use UNKNOWN / NOT ESTABLISHED.

## 8. GTT is an exit/protection mechanism, not a product code

The legacy Delivery path uses broker-side GTT protection. It should therefore
not be recorded as another broker product alongside MIS/CNC/CO/NRML.

Its role is:

```text
CNC Delivery position
        |
        v
OCO GTT
  /     \
TGT     SL
  \     /
 opposite leg cancelled
```

## 9. Strategy classification

Strategy YAMLs exist under `config/strategies/*.yaml`.

Historical user evidence states each strategy carries:
- Product classification: INTRADAY or DELIVERY/Position.
- Direction: LONG or SHORT.

The user states a strategy does not trade both product classifications.

Exact runtime precedence between global config, strategy YAML and runtime
flags remains an open source-level question.

## 10. Product vs order type

Product is distinct from order type:

```text
Product: MIS / CNC / CO / NRML
Order type: MARKET / LIMIT / SL / SL-M
```

The repository order path carries intent and entry order type separately.
The complete historical compatibility matrix is not established here and is
carried to the Order Processing agenda.

## 11. Product vs order variety

CO handling uses broker variety `co`; regular modification uses `regular`.
Thus product semantics and broker order variety are separate dimensions.
Universal historical enforcement is UNKNOWN.

## 12. Product vs capital

Legacy configuration contains:

```text
INTRADAY       5.0
COVER_ORDER    6.0
DELIVERY      1.0
BRACKET_ORDER 5.0
```

These values are legacy implementation/configuration evidence only. They are
not the V2 margin model. A-005's broker-specific margin model remains the
future design basis.

## 13. Product vs exit/protection

| Product | Legacy behavior |
|---|---|
| MIS | Intraday; EOD/squareoff eligible; emergency flatten eligible |
| CO | Intraday; dedicated handling; emergency flatten eligible |
| CNC | Delivery; excluded from intraday emergency/EOD flatten; GTT protection |
| BO | Internal intent; unsupported configured broker mapping; historical use unknown |
| NRML | Recognised as Delivery; historical use unknown |

## 14. Position identity

Historical user evidence describes the legacy rule as:

> One Symbol, One Segment (Intraday/GTT), One Direction.

Therefore the same symbol could exist separately in Intraday and Delivery.

The proposed V2 rule — **one symbol once per day only** — is not legacy fact.
It is preserved below as a future input.

## 15. Paper vs Live

The broker adapter contains live and paper paths. The user reports significant
paper/live differences/bugs, including price/time semantics.

Therefore paper behavior is not proof of live product fidelity.

## 16. Product validation / eligibility

The preserved architecture suggests:

```text
signal
 -> intent/product
 -> product resolution
 -> validation
 -> sizing/capital
 -> broker order
```

Exact historical ordering of product eligibility, reservation and final broker
eligibility remains UNKNOWN and belongs to the Order Processing/Broker Model
manuals.

## 17. Configuration controls

Historical user classification:

| Setting | Legacy role |
|---|---|
| trade_type | Business control |
| delivery_enabled | Business control |
| force_intraday_only | Business control |
| product_map | Business control |
| leverage_map | Business control |
| strategy product/intent | Business control |

This does not freeze a V2 schema.

## 18. Current Zerodha external cross-check

**Current external broker fact, not legacy evidence:** Zerodha's current
support documentation states that CO is intraday-only, BO was disabled in
March 2020, GTT currently supports CNC/NRML/MTF including OCO target/SL, and
intraday auto-squareoff timings are category-dependent and may change.

These facts are retained for V2 consideration and are not used to rewrite the
legacy history.

## 19. Evidence gaps

Available:
- Production contract notes.
- Repository/config evidence.
- Strategy YAMLs.
- GTT implementation and monitor.

Not available in A-007:
- screenshots/logs showing historical product values.
- definitive historical evidence identifying CO vs BO for every intraday trade.

Contract notes may later resolve CO/BO/NRML history.

# 20. FUTURE V2 INPUTS PRESERVED FROM A-007

These are deliberately NOT frozen as legacy decisions.

### Delivery
- Prefer GTT as the Delivery protection/exit mechanism.
- Avoid unnecessary application-side long-running Delivery TGT/SL logic.
- Do not apply intraday auto-squareoff to Delivery.

### Intraday
- Select one suitable current Zerodha-supported mechanism for entry plus
  required protective TGT/SL behavior.
- Do not blindly carry BO/CO from legacy.
- Verify current broker/API support before implementation.

### Master segment control
Proposed `Config.yaml` master setting:

```yaml
trade_type: INTRADAY | DELIVERY | BOTH
```

### Strategy secondary control
Proposed strategy fields:

```yaml
strategy: ON/OFF
classification: INTRADAY | DELIVERY
direction: LONG | SHORT
```

Global segment disable should suppress relevant strategies safely.

### Mode
V2 intended LIVE-only; paper mode is not required.

### Margin
Use current broker/product/stock eligibility and actual margin data rather
than a fixed legacy leverage value.

### Squareoff
System should square off intraday before broker/RMS cutoff and verify the
result. Exact timing/state machine remains open.

### Symbol rule
Proposed V2 rule: one symbol once per day only.

### Protective legs
The final V2 mechanism must explicitly handle protective-leg creation,
partial fills, opposite-leg cancellation and orphan-leg recovery.

# 21. CROSS-TOPIC CAPTURE

The following points from A-007 are intentionally preserved for later agendas
to avoid repeated user clarification.

## Order Management
- Explicit entry/protective-leg state machine.
- Opposite-leg cancellation after one leg executes where required.
- Partial-fill and orphan-leg handling.

## Trading Hours / Squareoff
- System squareoff before applicable broker/RMS cutoff.
- User wants first confirmation and another confirmation after a short delay.
- Exact delay remains OPEN.
- Current broker cutoff must be checked during V2 design.

## Capital / Margin
- Intraday margin is stock/product dependent.
- Delivery and Intraday capital treatment are distinct.
- A-005 broker-margin model remains the future basis.

## Configuration Governance
- Config.yaml proposed as master.
- Strategy YAML proposed as secondary.
- Avoid contradictory global/strategy settings.
- Master segment OFF must safely suppress relevant strategies.

## Broker Governance
- Do not select BO/CO merely because legacy code contains them.
- Current Zerodha/API eligibility must be verified before implementation.

## Evidence
- Contract notes are available.
- No screenshots/logs showing product values were available for A-007.

# 22. OPEN QUESTIONS

1. Exact historical Production use of CO.
2. Exact historical Production use of BO.
3. Whether NRML ever appeared only as broker-returned state.
4. Exact product-selection precedence.
5. Exact product-eligibility ordering.
6. Exact product/order-type compatibility matrix.
7. Historical product enable/disable timeline.
8. Whether contract notes conclusively identify CO/BO.
9. Final V2 intraday mechanism.
10. Final V2 squareoff timing/state machine.
11. Final Config.yaml vs Strategy YAML precedence.

# 23. GOVERNANCE RECORD

**Agenda:** A-007  
**Topic:** Product Types  
**Legacy status:** CLOSED / FROZEN  
**V2 status:** FUTURE INPUTS + OPEN QUESTIONS ONLY  
**Date:** 27-Sep-2026

END OF A-007 MANUAL
