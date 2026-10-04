# A-004 — Instruments / Market Segments
## Legacy Technical Manual
### Dead System — Factual Baseline

**Date:** 23-Sep-2026  
**Status:** Frozen with explicit UNKNOWN / NOT ESTABLISHED items  
**Primary baseline:** Production repository  
**Comparison:** Testing/Sandbox repository

---

## 1. Purpose

This chapter records the historical instrument and market-segment behavior
that can be established for the dead legacy system.

It does not define the V2 instrument universe and does not recommend which
instruments should be traded in a replacement system.

---

## 2. Executive baseline

The legacy Production system was an NSE-oriented automated trading system
consuming external Chartink webhook signals and executing through Zerodha.

The available evidence establishes an NSE-equity-oriented instrument
reference pipeline. Production also configured both Intraday/MIS and
Delivery/CNC, and Delivery/CNC was historically used according to direct
user contract-note evidence.

The exact phrase "all Zerodha NSE equity instruments" is NOT used as the
technical universe definition because the Production repository shows that
the system constructed its own `config/instruments.csv` from multiple
sources rather than simply using the complete broker instrument dump.

---

## 3. Market / exchange scope

| Scope | Classification | Evidence |
|---|---|---|
| NSE | FACT / HISTORICAL | Production architecture and user historical record |
| BSE | UNKNOWN / NOT ESTABLISHED as traded | No reviewed evidence establishes actual BSE orders |
| NFO / NSE F&O | UNKNOWN / NOT ESTABLISHED as traded | F&O reference/configuration exists, but actual orders are not established |
| CDS | UNKNOWN / NOT ESTABLISHED | No reviewed evidence of actual trading |
| BFO | UNKNOWN / NOT ESTABLISHED | No reviewed evidence of actual trading |
| MCX | UNKNOWN / NOT ESTABLISHED | No reviewed evidence of actual trading |

The historical trading baseline is therefore documented as **NSE equity**,
while absence of evidence is not converted into an absolute statement that
the repository never recognized another exchange.

---

## 4. Instrument universe

### 4.1 Production instrument-reference build

The repository file map identifies:

`config/instruments.csv`

as the runtime instrument reference.

The builder is:

`scripts/refresh_instruments.py`

The documented build process merges:

1. NSE security-master CSV, restricted to equity series
2. sector/index reference CSVs
3. `fno_symbols.csv` reference data
4. Zerodha Kite instrument dump

The Kite lookup is explicitly built for `NSE` / `NSE-EQ`.

Missing Kite matches are represented during the build with a zero token and
default lot/tick values and are reported as missing.

### 4.2 Refresh

The instrument dump is refreshed at **09:00 IST on weekdays** by the
`refresh_instruments` scheduled job.

### 4.3 Startup handoff

During Production startup, `main.py` pre-loads:

`config/instruments.csv`

before the aggregate startup checks complete.

### 4.4 Important interpretation

The reference file is shared infrastructure. That does not by itself prove
that every row was eligible for every strategy or every product.

Therefore:

> **Historical exact eligible universe = UNKNOWN / NOT ESTABLISHED.**

The correct factual statement is:

> Production used a centrally built NSE-equity-oriented instrument reference
> file derived from the NSE security master, reference metadata and Zerodha
> Kite data.

---

## 5. Signal → instrument relationship

The documented external signal path is:

```text
Chartink
   |
   | webhook POST
   v
Webhook Receiver
   |
   | validate symbol / price / request
   v
Signal persistence + queue
   |
   v
Signal Processor
   |
   v
Screening / decision
   |
   v
Sizing / risk / capital
   |
   v
Order placement
   |
   v
Zerodha
```

The repository establishes that the webhook receiver validates symbols/prices
and that the instrument master is preloaded at startup.

The exact final signal-to-token lookup function and complete invalid-symbol
rejection path are not sufficiently established in the A-004 evidence set.
They should therefore remain an explicit UNKNOWN until covered by the
signal/order chapters.

---

## 6. Symbol exclusions

### 6.1 Production fact

Production `system_config.yaml` contains:

```text
excluded_symbols:
  - E2E
  - GVPIL
  - BIRLACABLE
  - SHANKARA
  - MCLEODRUSS
```

The configuration comment states that these symbols are rejected at the
webhook edge.

### 6.2 Classification

**FACT:** A static Production symbol exclusion mechanism existed.

**FACT:** The reviewed Production configuration contains the five symbols
listed above.

**UNKNOWN:** Exact historical origin, reason and lifetime of each exclusion.

### 6.3 Important correction

The submitted answer "No blacklist" was incorrect when compared with
Production evidence.

This manual records the repository fact rather than the recollected answer.

### 6.4 V2 separation

The user has stated that this type of static blacklist should be removed in
the new system.

That is a **future redesign input only**. It is not used to rewrite this
legacy manual and no replacement design is specified here.

---

## 7. F&O / derivatives

The user reported that the legacy system did not trade F&O.

Repository evidence, however, also shows F&O-related reference/configuration
components, including an F&O symbol reference set and an NSE F&O ban-list
fetch.

Therefore the technically safe classification is:

- **Actual F&O trading:** NOT ESTABLISHED by the reviewed repository evidence.
- **F&O-related code/reference/configuration:** FACT.
- **"No F&O code exists":** rejected as too strong.

The existence of F&O reference data does not prove that F&O orders were ever
placed.

---

## 8. Equity instrument types

The repository's instrument builder is equity-oriented and filters the NSE
security master to a defined equity-series set.

However, the reviewed evidence does not establish a historical trade-by-trade
classification for every possible listed security type.

Therefore:

| Type | Classification |
|---|---|
| NSE equity | HISTORICAL / FACTUAL BASELINE |
| NSE index | NOT ESTABLISHED as traded |
| NSE futures | NOT ESTABLISHED as traded |
| NSE options | NOT ESTABLISHED as traded |
| BSE equity | NOT ESTABLISHED as traded |
| Commodity futures/options | NOT ESTABLISHED as traded |
| ETFs | Not separately frozen as a distinct traded class without instrument/trade evidence |

A security being present in an instrument master is not proof that it was
traded.

---

## 9. Delivery / CNC

Production configuration establishes:

```text
trade_type = BOTH
delivery_enabled = true
force_intraday_only = false

INTRADAY  -> MIS
DELIVERY  -> CNC
```

The user also supplied direct historical evidence from Zerodha contract
notes that Delivery/CNC trades actually occurred.

Therefore:

**Configured capability:** FACT.

**Historical Delivery/CNC usage:** HISTORICAL, based on direct user record.

**Exact Delivery instrument universe:** UNKNOWN / NOT ESTABLISHED.

The common instrument reference file supports a shared reference layer, but
does not by itself prove that every instrument was eligible for both books.

---

## 10. Cover Order / Bracket Order

Production product mapping includes:

```text
INTRADAY      -> MIS
DELIVERY      -> CNC
COVER_ORDER   -> CO
BRACKET_ORDER -> unsupported / empty
```

Classification:

- Historical CO usage: **UNKNOWN / NOT ESTABLISHED**
- Historical BO usage: **UNKNOWN / NOT ESTABLISHED**
- Current Production BRACKET_ORDER support: **unsupported / empty**

Pending SL/TGT legs observed at the broker are not sufficient evidence to
identify the exact product protocol without the relevant order-path proof.

---

## 11. Strategy relationship

The user reported:

- 16 strategies
- 10 intraday
- 5 delivery
- 1 placeholder
- one signal direction per strategy

This is retained as historical user/record information.

The permanent technical statement does not claim that all strategies share
an identical instrument universe because the exact YAML-by-YAML restrictions
have not been fully enumerated in A-004.

Strategy-specific instrument/product behavior belongs in the strategy and
order-processing chapters.

---

## 12. Production vs Testing

Production is the authoritative legacy baseline.

The Sandbox repository is comparison evidence only.

The repositories are not treated as identical merely because the user
recalled that their operational behavior was mostly the same.

A complete A-004-only Production/Sandbox difference matrix remains:

**UNKNOWN / NOT ESTABLISHED.**

No Sandbox difference is allowed to overwrite the Production baseline.

---

## 13. Historical changes

The submitted recollection that exchange scope was fixed and that Delivery
was enabled/disabled over time is not recorded as a precise historical
change because the required DATE / COMMIT / BEFORE / AFTER evidence has not
been established.

The following remain OPEN QUESTIONS:

- historical evolution of the instrument universe
- historical evolution of `excluded_symbols`
- historical Delivery enable/disable changes
- historical F&O-related changes
- historical lookup implementation changes

---

## 14. Source → Input → Logic → Validation → Output → Handoff

### Instrument-reference pipeline

**SOURCE**
- NSE security master
- Zerodha Kite instrument dump
- sector/index reference data
- F&O symbol reference data

↓

**INPUT**
- security-master rows
- broker instrument rows
- reference symbol sets

↓

**CALCULATION / LOGIC**
- filter NSE security master to equity series
- build Kite lookup restricted to NSE/NSE-EQ
- merge reference sources
- populate token/lot/tick information
- identify missing broker matches

↓

**VALIDATION / GUARD**
- source presence checks
- row-set validation
- post-write freshness assertion
- missing-token detection

↓

**OUTPUT**
- `config/instruments.csv`

↓

**STORAGE / HANDOFF**
- atomic CSV replacement
- startup preload by `main.py`

↓

**NEXT CONSUMER**
- runtime instrument-reference consumers in sizing/order/signal paths

Exact downstream lookup semantics remain an A-004/A-005+ cross-reference
unknown until those chapters are documented.

---

## 15. Fact discipline

### FACT
- NSE is the Production baseline.
- Chartink is the external signal source.
- Zerodha is the execution broker.
- `config/instruments.csv` is the Production instrument reference.
- `scripts/refresh_instruments.py` builds it.
- Refresh occurs at 09:00 IST on weekdays.
- The build uses NSE equity security-master data and Zerodha NSE/NSE-EQ data.
- Production has explicit symbol exclusions.
- Production config supports BOTH Intraday and Delivery.
- Delivery maps to CNC.
- BRACKET_ORDER is empty/unsupported.
- F&O-related reference/configuration exists.

### HISTORICAL
- User reports actual NSE equity trading.
- User reports actual Delivery/CNC trading from Zerodha contract notes.
- User reports no actual F&O trading.
- User reports the 16-strategy split.

### UNKNOWN / NOT ESTABLISHED
- Complete historical eligible instrument universe.
- Exact signal-to-token lookup key/path.
- Exact invalid-symbol rejection outcome.
- Actual historical CO use.
- Actual historical BO use.
- Complete historical F&O absence across all production history.
- Complete Production/Sandbox A-004 difference matrix.
- Exact historical changes by commit/date.

### INTERPRETATION
- `config/instruments.csv` functions as a centralized reference layer.

### KNOWN DEFECT
No new A-004 defect is declared without direct evidence.

### OPEN QUESTION
Historical evolution of the universe and exclusions.

---

## 16. Frozen A-004 statement

> **The legacy Production trading baseline was NSE equity-oriented and
> consumed external Chartink signals through Zerodha. Production maintained
> a centrally generated NSE-equity-oriented instrument reference file,
> refreshed from NSE/reference data and the Zerodha Kite instrument dump.
> Both Intraday/MIS and Delivery/CNC were configured, with Delivery/CNC
> historically used according to direct user contract-note evidence.
> Production also contained an explicit symbol-exclusion mechanism at the
> webhook edge. Actual historical F&O, CO, BO, and the complete exact
> instrument universe are not established by the evidence reviewed and are
> therefore not asserted as facts.**

---

## 17. Freeze status

**A-004 = CLOSED / FROZEN**

The freeze is not a claim that every historical implementation detail is
known. It means the established baseline is now recorded and the remaining
unknowns are explicitly bounded rather than guessed.

END OF A-004 LEGACY MANUAL
