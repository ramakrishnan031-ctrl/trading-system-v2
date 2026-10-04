# A-008 — Strategy Model
## Legacy System Technical Manual
### Phase 1 — DEAD / REFERENCE ONLY
### Frozen: 29-Sep-2026

---


> **REVISION 1 — 29-SEP-2026:** The previous manual froze the legacy set as
> “16 strategy/scanner definitions = 16 YAMLs.” That statement was too strong.
> A preserved locked decision record lists 15 YAML strategy names, while the
> supplied Chartink inventory lists 16 definitions including PB01 SHADOW.
> The exact Production YAML inventory is therefore **UNKNOWN / NOT ESTABLISHED**
> until the repository directory and mapping are directly verified. No other
> A-008 legacy conclusions are silently changed by this revision.

## 1. Scope and evidence discipline

This chapter documents the legacy Strategy Model as it existed or is evidenced.
It does not repair, tune, redesign, or revive the legacy system.

Evidence classes used throughout:

- **FACT** — directly supported by repository/source evidence.
- **HISTORICAL** — user-confirmed historical behavior.
- **UNKNOWN / NOT ESTABLISHED** — insufficient evidence.
- **INTERPRETATION** — reasoned reconciliation explicitly labelled.
- **KNOWN DEFECT / HISTORICAL FINDING** — documented defect or observed problem.
- **OPEN QUESTION** — requires later source tracing.
- **V2 / NEW-SYSTEM INPUT** — future requirement/preference, not a legacy fact.

Production is the primary legacy baseline. Testing/Sandbox is comparison
evidence only.

---

## 2. Strategy model at a glance

The legacy strategy model was not simply a list of technical indicators.

It consisted of several layers:

```text
Chartink scanner / alert
        |
        v
scanner -> strategy mapping
        |
        v
StrategyConfig
        |
        +--> strategy identity
        +--> classification / intent
        +--> direction
        +--> enabled state
        +--> entry window
        +--> strategy parameters
        |
        v
global strategy controls
        |
        v
common secondary screening
        |
        +--> hard gates
        +--> score
        +--> tier
        |
        v
entry / SL / target derivation
        |
        v
position sizing
        |
        v
risk admission
        |
        v
capital reservation
        |
        v
order placement
```

The important architectural distinction is:

> **Chartink strategy conditions were strategy-specific, while the legacy
> secondary qualification layer was common/universal.**

This distinction is central to understanding the old system.

---

## 3. Historical strategy inventory

The supplied `Chartink_scanners.v2.txt` contains 16 named scanner/strategy definitions. The count must not be equated automatically with the number of Production YAML files:

| # | Strategy / Scanner | Historical classification |
|---:|---|---|
| 1 | POSITIONAL SWING LONG | Delivery |
| 2 | POSITIONAL SECTOR ROTATION | Delivery |
| 3 | POSITIONAL MOMENTUM LONG | Delivery |
| 4 | RANGE BREAKOUT SHORT | Intraday |
| 5 | RANGE BREAKOUT LONG | Intraday |
| 6 | GAP FADE SHORT | Intraday |
| 7 | GAP FADE LONG | Intraday |
| 8 | GAP GO SHORT | Intraday |
| 9 | GAP GO LONG | Intraday |
| 10 | VWAP REJECTION SHORT | Intraday |
| 11 | VWAP BOUNCE LONG | Intraday |
| 12 | FIRST PULLBACK SHORT | Intraday |
| 13 | FIRST PULLBACK LONG | Intraday |
| 14 | OPEN HIGH BREAKDOWN SHORT | Intraday |
| 15 | OPEN LOW BREAKOUT LONG | Intraday |
| 16 | PB01 BREAKOUT + RETEST (SHADOW) | Intraday / shadow |

The user confirmed the historical count as:

- 10 Intraday
- 5 Delivery
- 1 placeholder/shadow

The apparent repository documentation reference to 15 strategies is reconciled
as the 15 normal/active-context strategies when the PB01 shadow strategy is
excluded from such contexts.

**UNKNOWN:** which individual strategy YAMLs were active at every historical
Production date. The user explicitly does not know.

---

## 4. Strategy configuration source

### Source

```text
config/strategies/*.yaml
```

### Loader

Repository-derived evidence identifies:

```text
strategies/loader.py
strategies/schema.py
```

The loader:

1. reads strategy YAML files;
2. validates them using `strategies.schema.validate_strategy`;
3. creates `StrategyConfig` objects;
4. keys the loaded dictionary by `cfg.name`;
5. optionally validates `scan_webhook_map` references.

The preserved repository map identifies `StrategyLoader.load_all_strategies`
and states that strategy loading is **once at startup** with no hot reload.

### Validation

`strategies/schema.py` uses a Pydantic model with extra fields forbidden.
The schema covers strategy identity, direction/intent/order protocol,
taxonomy, enabled state, entry/SL/target/trailing/pullback parameters,
screening thresholds, position-cap-related information, and entry windows.

### Output

```text
Dict[str, StrategyConfig]
```

keyed by strategy name.

### Consumers

The loaded strategy definition feeds:

- strategy control;
- direction handling;
- signal processing;
- screening;
- status/reporting;
- downstream entry/risk/order paths.

### Important limitation

A YAML existing in the repository does not prove that the corresponding
strategy actually traded in Production.

---

## 5. Strategy identity

### FACT

Strategy configuration has a strategy name.

The direction registry builds a canonical map:

```text
strategy_name -> LONG | SHORT
```

from validated strategy YAMLs.

### UNKNOWN

The following exact identity contract is not established:

```text
YAML filename
    == YAML name
    == Chartink scanner name
    == webhook strategy field
    == DB strategy field
```

Therefore the manual does not assume these identifiers were universally
identical.

---

## 6. Strategy direction

### FACT

The repository recognizes:

```text
LONG
SHORT
```

as direction values.

### HISTORICAL

The user confirms that each legacy strategy generated only one side:

```text
LONG-only
or
SHORT-only
```

The Chartink inventory is consistent with this design for most explicitly
directional strategy names.

Examples:

- RANGE BREAKOUT SHORT / LONG
- GAP FADE SHORT / LONG
- GAP GO SHORT / LONG
- VWAP REJECTION SHORT
- VWAP BOUNCE LONG
- FIRST PULLBACK SHORT / LONG
- OPEN HIGH BREAKDOWN SHORT
- OPEN LOW BREAKOUT LONG

### Important qualification

The user stated that the YAML itself did not necessarily label the direction;
the strategy/scanner generated the corresponding directional signal.

Therefore the permanent manual must not claim that every YAML literally
contained a direction label without per-file source evidence.

---

## 7. Strategy classification / intent

The legacy strategy model carried Intraday versus Delivery intent.

Historical user classification:

```text
INTRADAY
DELIVERY / POSITION
```

A strategy was not intended to trade both product classes.

The global control layer also considered:

```text
trade_type
force_intraday_only
strategy enabled/disabled
```

### Classification

- **FACT:** strategy intent/classification exists.
- **HISTORICAL / USER-REPORTED:** 10 Intraday + 5 Delivery + 1 placeholder/shadow.
- **SOURCE-RECORD:** 15 strategy YAML names are listed in locked decision S12.
- **SOURCE-RECORD:** 16 named scanner/strategy definitions appear in the supplied
  Chartink inventory.
- **UNKNOWN:** exact precedence among all classification/product-control
  inputs.

---

## 8. Global control vs strategy control

The user's historical description is:

```text
system_config.yaml
    = global/universal controller

strategy YAML
    = secondary/sub-controller
```

Example operating model:

```text
trade_type = BOTH
    |
    +--> Intraday strategies may proceed
    +--> Delivery strategies may proceed

strategy enabled = false
    |
    +--> that individual strategy does not proceed
```

A global `INTRADAY` setting therefore suppresses Delivery strategy eligibility.

Repository evidence confirms the strategy control layer evaluates:

- global `trade_type`;
- `force_intraday_only`;
- per-strategy enabled state.

### V2 input

The user considers `force_intraday_only` redundant if `trade_type` already
provides the master segment control.

That is a V2 design input, not a legacy correction.

---

## 9. Strategy enable / disable

### HISTORICAL

The user confirms strategies could be independently enabled/disabled.

### FACT

The repository-derived strategy-control path evaluates strategy enable/disable
before:

- sizing;
- capital reservation;
- order placement.

### UNKNOWN

Exact rejection status and reporting behavior for disabled strategies.

---

## 10. Strategy entry windows

The legacy system has two time layers:

```text
GLOBAL ENTRY WINDOW
        +
STRATEGY-SPECIFIC ENTRY WINDOW
```

Production configuration documentation states that the global `entry_start`
acts as a floor over the per-strategy start time.

Example documented configuration:

```text
global entry_start = 10:00
strategy entry_start_time = 09:25
effective start = 10:00
```

The 09:25 strategy value remains its declared strategy intent; it is not
silently rewritten to 10:00.

### Historical use

The user answered that actual material Production use of per-strategy entry
windows is **UNKNOWN**.

Therefore:

- configuration mechanism = FACT;
- actual historical effect = UNKNOWN.

### Testing difference

User reports:

```text
Production start: 10:00
Testing/Sandbox start: 09:30
```

This is retained as a historical configuration difference.

---

## 11. Strategy → Chartink mapping

The repository contains:

```text
config/scan_webhook_map.yaml
```

The loader validates mapped scanners and ensures the referenced strategy has
a corresponding YAML.

Normal signal flow:

```text
Chartink webhook
    |
    v
scanner identification
    |
    v
scan_webhook_map.yaml
    |
    v
strategy
```

### Historical user confirmation

- every scanner mapped to one strategy;
- a strategy could receive multiple signals;
- signals were not cross-fed between unrelated strategies.

### UNKNOWN

- unmapped scanner behavior;
- duplicate mapping behavior;
- complete historical mapping inventory;
- historical mapping changes.

The user reports no unmapped-scanner incident was tested/experienced.

---

## 12. Chartink strategy conditions

The supplied Chartink record provides concrete evidence that the signal-generation
strategies were materially different.

### 12.1 Positional Swing Long

The source requires, among other things:

- Open = Low;
- Close > Open;
- Close > VWAP;
- Close > previous-day High;
- Close > EMA200;
- current volume > 1.5 × 20-day volume SMA;
- 20-day volume SMA > 50,000;
- price > 100;
- price < 5000;
- daily range > 0.5% of Close.

These are source facts from the supplied scanner record.

### 12.2 Positional Sector Rotation

Conditions include:

- Close > previous-day 20-day High maximum;
- Close > Open;
- Close > EMA50;
- volume > 1.5 × 50-day volume SMA;
- 50-day volume SMA > 50,000;
- price 100–5000.

### 12.3 Positional Momentum Long

Conditions include:

- Close > previous-day 252-day High maximum;
- Close > Open;
- Close > EMA200;
- Close > EMA50;
- volume > 1.5 × 50-day volume SMA;
- 50-day volume SMA > 50,000;
- price 100–5000.

### 12.4 Range Breakout Short / Long

The pair uses:

- 15-day range < 2% of Close;
- directional close beyond the 15-day range;
- directional Open;
- directional VWAP;
- EMA20 confirmation;
- volume filter;
- price filter;
- minimum daily range condition.

### 12.5 Gap Fade Short / Long

The pair uses approximately ±1% opening gap conditions followed by reversal
confirmation, VWAP/EMA20/prior-close relationships, volume and price filters.

### 12.6 Gap Go Short / Long

The pair uses approximately ±1% opening gap conditions followed by continuation
confirmation, EMA200 and prior-day level relationships, volume and price
filters.

### 12.7 VWAP Rejection Short

Key structure:

- Close < VWAP;
- High >= VWAP;
- Close < Open;
- Close < previous Close;
- Close < EMA20;
- volume and price filters.

### 12.8 VWAP Bounce Long

Key structure:

- Close > VWAP;
- Low <= VWAP;
- Close > Open;
- Close > previous Close;
- Close > EMA20;
- volume and price filters.

### 12.9 First Pullback Short / Long

The pair uses VWAP and EMA20 relationships plus directional pullback,
previous-close, volume and price conditions.

### 12.10 Open High Breakdown Short

Key structure:

- Open = High;
- Close < Open;
- Close < VWAP;
- Close < previous-day Low;
- Close < EMA200;
- volume and price filters.

### 12.11 Open Low Breakout Long

Key structure:

- Open = Low;
- Close > Open;
- Close > VWAP;
- Close > previous-day High;
- Close > EMA200;
- volume and price filters.

### 12.12 PB01 Breakout + Retest (Shadow)

The supplied source defines:

- Close > previous-day 20-day High maximum;
- Close > EMA50;
- EMA50 > EMA200;
- Close > Open;
- volume > 1.5 × 20-day volume SMA;
- 20-day volume SMA > 50,000;
- price 100–5000.

It is explicitly marked **SHADOW**.

### Architectural meaning

These conditions prove that **signal generation was strategy-specific**.

They do not prove that the downstream secondary screening was strategy-specific.

---

## 13. Common secondary filtration

The legacy pipeline used a common secondary screening mechanism.

Repository-derived flow:

```text
strategy lookup
    ->
strategy control
    ->
strategy entry window
    ->
secondary screener
    ->
price derivation
    ->
sizing
    ->
risk
    ->
reservation
    ->
order
```

The secondary screener produced:

- passed/rejected;
- status;
- score;
- tier;
- rejected step;
- step results;
- step statuses;
- errors;
- latency;
- market-data snapshot.

### Historical user confirmation

All legacy strategies used the same universal/common score/rank filtration.

Therefore the architecture is best represented as:

```text
strategy-specific signal generation
              +
common secondary qualification
              =
legacy candidate selection
```

This is one of the most important A-008 findings.

---

## 14. Score / ranking / tier

### FACT

The screening result contains score and tier.

The legacy sizing path consumes tier/performance multipliers.

### HISTORICAL USER CONFIRMATION

- score/ranking restricted entry candidates;
- candidates below the required score could be rejected;
- tier multiplier influenced position size.

### Important boundary

Score/rank is not itself the Strategy Model.

It is a downstream qualification/sizing mechanism shared across strategies.

---

## 15. Strategy-specific risk / target / R:R

The user stated:

- SL should be strategy-wise;
- target/R:R is strategy-specific;
- target/R:R is derived from Strategy YAML.

Repository-derived evidence confirms that StrategyConfig contains entry/SL/target
related fields and the downstream entry engine consumes target/R:R-related
strategy information.

### Final classification

**FACT**

Strategy configuration contains risk/target-related fields.

**HISTORICAL USER CONFIRMATION**

Target/R:R was strategy-specific.

**UNKNOWN**

The exact field-by-field precedence between strategy YAML and global config
for every historical strategy.

The future requirement that every V2 strategy have its own SL% is recorded as
a V2 input only.

---

## 16. Strategy → order parameters

The exact degree to which StrategyConfig directly selected broker/order
parameters versus passing intent downstream is not fully established in A-008.

The model clearly passes strategy identity/intent into downstream processing.

### UNKNOWN

Do not state that strategy YAML directly supplied every broker order field.

Product/order semantics are documented separately in A-007/A-006.

---

## 17. Strategy + capital / position sizing

The user stated that major capital and sizing constraints were global
`system_config.yaml` controls.

Repository evidence requires a qualification:

`StrategyConfig` includes a per-strategy position-cap-related field.

Therefore:

- **FACT:** the schema is capable of strategy-level position constraints.
- **HISTORICAL USER REPORT:** the deployed legacy business controls were
  primarily global.
- **UNKNOWN:** whether the Production YAMLs populated/enforced the
  strategy-level position-cap field.

A-005 remains authoritative for the detailed capital/sizing mechanics.

---

## 18. Duplicate and concurrency rules

### Historical legacy rule

```text
one stock + one direction once a day per segment
```

Intraday and Delivery were independent for this rule.

### Repository evidence

The runtime includes:

- one-trade-per-symbol-direction;
- portfolio/risk checks;
- concurrency-aware admission.

### V2 input

The user wants:

```text
one stock + one direction once a day
```

without separate segment duplication.

This is a future-system requirement and must not be retroactively applied to
the legacy manual.

---

## 19. Strategy governor / circuit breaker

The repository contains strategy governance components, including:

- strategy governor;
- strategy metrics;
- strategy health/reporting;
- circuit-breaker mechanisms;
- strategy/direction registry components.

Configuration includes:

```text
enabled
loss_multiplier
cutoff_time
lookback_days
```

### Classification

- implementation/configuration = FACT;
- actual Production activation/effect = UNKNOWN.

The existence of code does not prove it affected live trading.

---

## 20. Paper vs Live

User historical answer:

> Paper and live used the same strategy configuration and decision logic.

This is recorded as HISTORICAL USER CONFIRMATION.

Exact paper-only differences in:

- scoring;
- filtering;
- entry timing;
- strategy decisions

remain UNKNOWN.

Other system modules have known paper/live differences, but they should not
be incorrectly attributed to the strategy layer.

---

## 21. Production vs Testing Chartink identity

Two screenshots provide an important integration finding.

### Production

The screenshot shows a scanner/alert named:

```text
VWAP BOUNCE LONG
```

### Testing

The screenshot shows:

```text
VBB097-VWAP BOUNCE LONG
```

The user explains that:

- Production strategy/scanner names did not include the Zerodha client ID;
- Testing names included the client ID marker;
- the marker appeared in the Chartink payload;
- Testing was manually adjusted to ignore/remove the marker when resolving
  the payload.

### Classification

**HISTORICAL**

This is a Production-vs-Testing naming/integration difference.

**V2 INPUT**

Use one universal strategy/scanner naming and payload identity convention.

### Security

The screenshots contain webhook material. No token, secret or credential is
reproduced here.

---

## 22. Historical strategy defects / limitations

### 22.1 Late / overextended signals

User identifies this as the highest-importance strategy problem:

> A strategy could generate a signal after the stock had already run for the day.

The reported consequence was insufficient remaining price room and frequent SL
hits.

This is recorded as a historical user-reported defect/finding.

### 22.2 Universal secondary filter

The common score/rank filtration applied irrespective of strategy/segment.

This limited the ability to make qualification rules strategy-specific.

### 22.3 Near-duplicate strategies

User reports that some strategies were almost duplicates with different names.

The supplied Chartink source should be used as the evidence base when comparing
their actual conditions.

### 22.4 PB01 shadow

PB01 Breakout + Retest was maintained as a shadow/placeholder rather than an
ordinary strategy.

---

## 23. Strategy source → consumer trace

| Strategy field | Source | Transformation / logic | Consumer | Status |
|---|---|---|---|---|
| Name | strategy YAML | StrategyLoader → StrategyConfig keyed by name | strategy lookup/reporting | FACT |
| Direction | StrategyConfig / direction registry | canonical name → LONG/SHORT | signal/order side | FACT |
| Classification | StrategyConfig + global config | intent/control resolution | eligibility/product path | FACT; precedence partly UNKNOWN |
| Enabled | strategy YAML/control | eligibility gate | screening/sizing/order path | FACT |
| Scanner mapping | scan_webhook_map.yaml | scanner → strategy | signal processor | FACT |
| Entry window | global + strategy YAML | both must permit | entry gate | FACT; historical use partly UNKNOWN |
| SL | strategy configuration | downstream derivation | sizing/order protection | strategy-specific use supported |
| Target/R:R | strategy configuration | target derivation | order path | historical strategy-specific use confirmed |
| Screening | common screener | hard gates + score/tier | candidate admission | FACT |
| Score/tier | screener | qualification / multiplier | sizing/admission | FACT |
| Position cap | global config + StrategyConfig capability | risk gate | RiskEngine | exact deployed use UNKNOWN |
| Governor | config/code | strategy-level circuit breaker | eligibility/governance | actual Production effect UNKNOWN |

---

## 24. End-to-end legacy strategy walkthrough

### Step 1 — Chartink generates a signal

A strategy-specific Chartink scanner identifies a stock using its own
conditions.

### Step 2 — Webhook reaches the system

The scanner identity is used for strategy resolution.

### Step 3 — Strategy mapping

`scan_webhook_map.yaml` maps scanner to strategy.

### Step 4 — Strategy configuration

The corresponding StrategyConfig supplies the strategy definition and
associated intent/parameters.

### Step 5 — Global strategy controls

Global trade type and strategy enablement determine whether the strategy may
continue.

### Step 6 — Strategy entry window

The global and strategy-specific entry windows are evaluated.

### Step 7 — Common secondary screen

The stock passes through the universal legacy screening process.

### Step 8 — Score / tier

The screener calculates score/tier and either rejects or permits continuation.

### Step 9 — Price derivation

The system derives entry and SL and may re-anchor using a fresh quote.

### Step 10 — Position sizing

PositionSizer applies risk/capital/concentration/tier logic documented under
the capital manual.

### Step 11 — Risk admission

RiskEngine checks capital, positions, trade limits, losses, duplicates and
other configured controls.

### Step 12 — Capital reservation

FundManager reserves the required capital.

### Step 13 — Order placement

The strategy-qualified candidate reaches the downstream broker/order layer.

A-008 ends at the Strategy Model boundary; detailed broker/order state
machines belong to A-006/A-007 and later manuals.

---

## 25. Production vs Testing comparison

Known strategy-relevant difference from user evidence:

| Area | Production | Testing/Sandbox |
|---|---|---|
| Global entry start | 10:00 | 09:30 |
| Chartink strategy naming | no client-ID marker | client-ID marker present |
| Payload normalization | normal | manual marker-ignore adjustment reported |
| General strategy model | reported same | reported same |
| S&R/config | separately documented | separately documented |

The statement "same" is retained only as user historical evidence; it is not
treated as proof that every repository file was byte-identical.

---

## 26. Cross-topic retained inputs

These items are intentionally retained here because the user requested that
important related points not be lost.

### Chartink integration

- Both Production and Testing consume Chartink signals.
- Production did not embed Zerodha client ID in scanner/strategy names.
- Testing did.
- Testing required a normalization adjustment.
- Future system should have one universal naming/payload convention.
- Never expose webhook secrets.

### Instrument model

- Legacy strategy processing used the global Zerodha instrument reference.
- A separate strategy-specific instrument universe was not established.

### Capital / risk

- Score/tier affected downstream quantity.
- Capital/risk remains downstream of strategy selection.
- A-005 is authoritative for detailed capital mechanics.

### Broker/product

- Strategy intent reaches downstream product/order processing.
- Exact broker product selection is not frozen here.
- A-006/A-007 remain authoritative for broker/product semantics.

### Trading hours

- Global and strategy-specific windows coexist.
- Production and Testing global starts differed.
- This must be carried into A-009 Configuration Model.

### Secondary filtering / S&R

- Late/overextended signals were a major historical issue.
- Common filtering was not strategy-aware.
- Chartink signal generation itself was strategy-specific.
- Later filtering/S&R agendas must separate these two layers.

### Configuration governance

The user wants, for V2:

```text
Config.yaml
    = global/master controls

Strategy.yaml
    = strategy-specific controls
```

The user also wants all business/decision values YAML-driven rather than
hard-coded in Python.

These are future governance inputs, not legacy freezes.

---

## 27. Known evidence gaps

The following remain deliberately unresolved:

1. exact per-file Production activation history;
2. exact historical scanner-map inventory;
3. unmapped scanner behavior;
4. mapping collision behavior;
5. actual Production effect of strategy governor;
6. actual per-strategy position-cap population/use;
7. exact YAML/global precedence for every strategy field;
8. exact historical paper-only strategy differences;
9. complete strategy change timeline.

These gaps do not prevent A-008 from being frozen because the manual explicitly
records them as UNKNOWN.

---

## 28. V2 inputs — not legacy decisions

The following are preserved for the later redesign phase:

- Do not blindly carry the legacy 16-strategy model.
- A smaller strategy set may be used.
- Each V2 strategy may receive its own secondary filtering logic.
- Strategy-wise SL% is desired.
- Strategy YAML should contain strategy-specific controls.
- Global config should remain the master controller.
- `force_intraday_only` should be reconsidered as potentially redundant.
- Universal score/rank allocation should be reconsidered.
- One stock + one direction once a day is the stated V2 duplicate rule.
- Chartink naming/payload semantics should be universal.
- Business values should be configuration-driven rather than hard-coded.

None of these are implementation decisions in this Phase 1 manual.

---

## 29. Governance freeze

### A-008 status

**OPEN / REOPENED — STRATEGY COUNT VERIFICATION PENDING**

The frozen baseline is:

> The legacy Strategy Model consisted of externally generated, strategy-specific
> Chartink scanners mapped into validated strategy definitions, with explicit
> strategy identity, Intraday/Delivery intent, enablement and downstream
> strategy controls. The legacy system had 16 documented strategy/scanner
> definitions (10 Intraday, 5 Delivery and one PB01 shadow/placeholder).
> Strategy-specific Chartink conditions generated the initial signals, but the
> downstream secondary qualification layer used a common score/rank mechanism
> across strategies. Strategy configuration also carried risk/target-related
> parameters, while major capital/risk controls were primarily global. The
> exact historical activation and use of several strategy-level controls remain
> explicitly UNKNOWN.

This is a documentation freeze only.

It is not a V2 architecture approval.

---

## 30. Evidence references

Primary project records used:

- `A-008_STRATEGY_MODEL_DISCUSSION_CHECKLIST_27-SEP-2026(1).txt`
- `Chartink_scanners.v2.txt`
- `02_REPOSITORY_AND_FILE_MAP.md`
- `CURRENT_PRODUCTION_SYSTEM_DEEP_WORKING_TREE_21-SEP-2026.txt`
- `system_config.yaml`
- A-002 Trading Objectives record
- A-004 Instruments / Market Segments record
- A-005 Capital Model record
- A-006 Broker Model record
- A-007 Product Types record

Relevant repository modules identified by the preserved repository map:

```text
strategies/schema.py
strategies/loader.py
strategies/control.py
core/strategy_direction.py
config/scan_webhook_map.yaml
config/strategies/*.yaml
signals/signal_processor.py
screening/secondary_screener.py
capital/position_sizer.py
capital/risk_engine.py
capital/fund_manager.py
```

---

# END OF A-008 STRATEGY MODEL MANUAL
## Revision 1 — Source-count verification status

### Direct GitHub retry
The three named repositories were retried on 29-SEP-2026:
- Production snapshot
- Testing/Sandbox snapshot
- Records repository

The available web retrieval path still did not expose the repository contents
directly. No unverified GitHub content has been substituted.

### Preserved project evidence that materially changes the count conclusion
The preserved `locked_decisions.yaml` contains decision **S12 — “The 15 strategy
YAML file names (locked)”**, listing 15 strategy YAML names:
- 6 Intraday LONG
- 6 Intraday SHORT
- 3 Positional

The preserved repository map also documents:
- `config/strategies/*.yaml` as the strategy configuration source;
- `StrategyLoader` loading strategy YAMLs;
- `v3_chain.pb01_entry` as a PB01 shadow-entry component;
- strategy-status tooling that skips `v3_playbook` shadow strategies.

The supplied Chartink inventory separately contains 16 named definitions,
with `PB01 BREAKOUT + RETEST (SHADOW)` as the 16th.

### Correct current classification
| Evidence | Current classification |
|---|---|
| 16 named Chartink definitions | FACT |
| 15 YAML names in locked decision S12 | FACT — source-record |
| PB01 is marked SHADOW | FACT |
| PB01 definitely has a Production YAML | UNKNOWN |
| Exact Production YAML count | UNKNOWN / NOT ESTABLISHED |
| Exact Production YAML membership | UNKNOWN / NOT ESTABLISHED |
| Exact historical active set by date | UNKNOWN / NOT ESTABLISHED |

### Governance status
A-008 is **OPEN / REOPENED** only for this source-count verification.
This is a documentation correction, not a V2 design decision.
