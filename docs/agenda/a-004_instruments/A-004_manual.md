# A-004 INSTRUMENTS / MARKET SEGMENTS: LEGACY MANUAL (REVISED)

Status: FROZEN 2026-10-06 (revision 2, after the ChatGPT value-added review of 06-Oct-2026). Recorded as DR-019. Erratum E-1 to this manual (section 5, line 65: what the receiver checked for a symbol and a price) added 2026-10-23 during C-004, accepted by Rama on 23-Oct-2026 (option (b), line 65 only) and recorded in DR-057 (2026-10-23) together with the C-004 freeze.
Date: 2026-10-06
Layer: LEGACY ONLY. The historical instrument and segment scope, not V2's universe. V2 ideas are PROPOSED in docs/governance/V2_DESIGN_LOG.md section 3.
Working copy of: docs/inputs/manuals/A-004_INSTRUMENTS_MARKET_SEGMENTS_LEGACY_MANUAL_23-SEP-2026.md (frozen 23-Sep-2026 in the ChatGPT phase; original untouched). Changes are listed at the end.
Primary baseline: production repository. Comparison only: testing/sandbox repository.
Labels: FACT, HISTORICAL (user-reported or record), RECORD-DERIVED, KNOWN DEFECT, UNKNOWN / NOT ESTABLISHED, INTERPRETATION (including AUDIT LEAD), EXTERNAL CONTEXT (current outside facts, never legacy evidence), OPEN QUESTION.
Security: no credential values.

## 1. Purpose
Record the historical instrument and market-segment behaviour that can be established for the legacy system. It does not define V2's instrument universe.

## 2. Executive baseline and evidence levels
- The legacy production system was NSE-oriented, consumed external Chartink webhook signals and executed through Zerodha.
- The evidence establishes an NSE-equity-oriented instrument reference pipeline. Production configured both intraday/MIS and delivery/CNC; delivery/CNC was used, per Rama's direct contract-note evidence.
- "All Zerodha NSE equity instruments" is not used as the technical universe: production built its own config/instruments.csv from several sources, not the raw broker dump.
- Evidence-boundary rule (applies to this whole manual): presence in an instrument/reference file establishes reference-data presence only; it does not by itself establish eligibility, orderability, or actual historical trading.
- Configured capability, code capability, reference-data presence, strategy eligibility and actual trading are separate evidence levels. So are: the NSE universe, all Zerodha NSE instruments, all NSE securities, and all instruments actually traded.

## 3. Market / exchange scope

| Scope | Classification | Evidence |
|---|---|---|
| NSE | FACT / HISTORICAL | Production architecture; Rama |
| BSE | NOT ESTABLISHED as traded | No reviewed evidence of BSE orders |
| NFO / NSE F&O | NOT ESTABLISHED as traded | F&O reference/configuration exists; orders not established |
| CDS, BFO, MCX | NOT ESTABLISHED as traded | No reviewed evidence |

The trading baseline is NSE equity. Absence of evidence is not turned into a claim that the code never recognised another exchange.

## 4. Instrument universe

### 4.1 Production instrument-reference build (FACT per the reconciliation)
- Runtime reference: config/instruments.csv. Builder: scripts/refresh_instruments.py.
- Sources merged: (1) NSE security-master CSV restricted to equity series {eq, be, n, bt, il}; (2) sector/index reference CSVs; (3) fno_symbols.csv reference data; (4) the Zerodha Kite instrument dump, indexed for segment NSE / NSE-EQ only.
- FACT: when a broker (Kite) match was missing, the builder wrote fallback values: token 0, lot size 1, tick size 0.05, and reported the symbol as missing during the build.
- UNKNOWN: whether any row carrying these fallback values ever reached a live quote or order path. The fallback itself is not a known defect.
- Build guards: source presence checks, row-set validation, missing-token detection, atomic CSV replacement, post-write freshness assertion.

### 4.2 Refresh and start-up
- FACT: refreshed at 09:00 IST on weekdays by the refresh_instruments job (Rama confirms 09:00).
- FACT: main.py pre-loads config/instruments.csv during start-up, before the aggregate start-up checks complete.
- RECORD-DERIVED (storage record, testing VM, not the production baseline): instruments.csv there had 2,228 rows, all NSE, all lot size 1. This is never to be read as the production universe.

### 4.3 Interpretation
- The reference file is shared infrastructure; it does not prove every row was eligible for every strategy or product (section 2 rule).
- Historical exact eligible universe: UNKNOWN / NOT ESTABLISHED.
- Factual statement: production used a centrally built NSE-equity-oriented instrument reference file derived from the NSE security master, reference metadata and Zerodha Kite data.

### 4.4 Instrument-data risks (INTERPRETATION / AUDIT LEADS, not known defects)
Tick size:
- LEGACY FACT: a missing broker match could receive tick size 0.05 (4.1).
- EXTERNAL CONTEXT (not legacy evidence): from 10-Jun-2024 NSE set a Rs 0.01 tick for securities priced below Rs 250 (ETFs excluded); from 15-Apr-2025 NSE moved to price-band tick sizes (for example 0.10 for Rs 1,001 to 5,000, rising to 5.00 above Rs 20,000; exact coverage not verified here). Both changes predate the legacy live period (live from 11-May-2026 per the README).
- INTERPRETATION: where the exchange tick is larger than 0.05, prices on a 0.05 grid could fall between valid ticks and be rejected; where the exchange tick is 0.01, a 0.05 grid stays valid but is coarser than needed.
- UNKNOWN: whether this ever happened in production or caused a rejected or mispriced order.
- Scope note: this concerns the instrument-builder fallback only. Any separate tick default in order-price arithmetic belongs to E-012 and section G and is not assessed here.
Token 0:
- LEGACY FACT: a missing broker match could produce token 0 in the reference file.
- UNKNOWN: whether any token-0 row reached quote or order execution.
Both leads go to the post-agenda audit (DR-016).

## 5. Signal to instrument
- Path: Chartink, webhook POST, webhook receiver (validates symbol, price, request), signal persistence and queue, signal processor, screening/decision, sizing/risk/capital, order placement, Zerodha.
- FACT: ~~the receiver validates symbols/prices, and the instrument master is preloaded at start-up.~~ (superseded by erratum E-1) the receiver checks a symbol only for being non-empty, after its alias lookup and the static exclusion list, and a price only for parsing as a number and not being zero or less; it does not look the symbol up in the instrument master, which is preloaded at start-up (C-001_manual.md section 15; C-004).
- UNKNOWN: the exact signal-to-token lookup function and key, whether exchange came from Chartink or was inferred, and the full invalid-symbol rejection path (signal and order chapters).
- HISTORICAL (Rama): an unknown symbol would not be processed (belief, not verified).

## 6. Symbol eligibility controls
Three things are kept separate:
- A. Legacy static symbol exclusion (documented here):
  - FACT: production system_config.yaml held excluded_symbols: E2E, GVPIL, BIRLACABLE, SHANKARA, MCLEODRUSS. The config comment says they were rejected at the webhook edge.
  - FACT: a static symbol-exclusion mechanism existed. Rama's answer "no blacklist" is corrected by this evidence.
  - UNKNOWN: origin, reason and lifetime of each exclusion.
  - No separate positive whitelist was established.
- B. Legacy learned MIS blocklist (pointer only): per the production baseline (section 12), secondary screening could reject symbols recently blocked by the broker for MIS (REJECTED_NOT_MIS_TRADABLE), or log the would-reject in shadow mode. Its detail belongs to section D.
- C. Whether and how V2 has instrument-eligibility controls is a V2 decision (V2_DESIGN_LOG VP-27), not part of this manual.

## 7. F&O / derivatives
- HISTORICAL (Rama): no F&O was traded; Chartink signals were NSE equity; no expiry or Greeks code existed.
- FACT: F&O-related reference/configuration existed: an F&O symbol reference set and an NSE F&O ban-list fetch (08:35, A-003).
- Actual F&O trading: NOT ESTABLISHED. "No F&O code exists" is rejected as too strong.
- FACT: the regime component uses NIFTY 50 index metadata; this does not show the index was traded.

## 8. Equity instrument types

| Type | Classification |
|---|---|
| NSE equity | HISTORICAL / FACTUAL BASELINE |
| ETFs | HISTORICAL (user-reported: Silver ETF and Gold ETF were traded). UNKNOWN: whether those symbols passed through the documented series filter and build path. |
| SME | FACT: the build applied the series filter {eq, be, n, bt, il}. HISTORICAL (user-reported): Rama believes the universe was all Zerodha NSE equity instruments, which he describes as including EQ, SME and ETF; no specific SME trade was reported. UNKNOWN: whether any SME instrument entered the production eligible universe or was traded. INTERPRETATION / AUDIT LEAD only: commonly used SME series codes (SM / ST, to verify) do not appear in the filter. OPEN QUESTION: can a historical production instruments.csv, trade/order record or broker instrument snapshot establish SME inclusion? |
| NSE index | NOT ESTABLISHED as traded (Rama: none) |
| NSE futures / options | NOT ESTABLISHED as traded |
| BSE equity | NOT ESTABLISHED as traded |
| Commodity futures / options | NOT ESTABLISHED as traded |

## 9. Delivery / CNC
- FACT (production config): trade_type = BOTH; delivery_enabled = true; force_intraday_only = false; INTRADAY to MIS; DELIVERY to CNC.
- HISTORICAL: delivery/CNC trades occurred (Rama, Zerodha contract notes).
- HISTORICAL (user-reported): intraday and delivery used one common instrument file; delivery positions carried overnight with the broker handling SL/target after a GTT; DDPI was required for the workflow of automatic next-day sale of delivery holdings. Mechanics belong to A-006 and A-007.
- UNKNOWN: the exact delivery instrument universe, and whether every row was eligible for both books.

## 10. Cover order / bracket order
- FACT / CONFIGURATION OBSERVATION: COVER_ORDER maps to CO; BRACKET_ORDER maps to an empty/unsupported product, yet the leverage map still holds a BRACKET_ORDER value (5.0), alongside COVER_ORDER 6.0, INTRADAY 5.0 and DELIVERY 1.0. Not a known defect: no evidence shows the retained value affected runtime behaviour. Product implications belong to A-006 and A-007.
- Historical CO use: UNKNOWN. Historical BO use: UNKNOWN. Rama's observation (SL/target legs appearing as pending orders after entry) does not identify the product protocol without order-path proof.

## 11. Strategy relationship
- Strategy/product classification was present in the legacy system; exact deployed strategy membership is addressed under A-008.
- HISTORICAL (user-reported, given during the A-004 discussion): 16 strategies, 10 intraday, 5 delivery, 1 placeholder (marked intraday), each trading one direction only. This is not a verified production strategy inventory.
- UNKNOWN / NOT ESTABLISHED: the exact production YAML count and deployed strategy membership (Q-17, A-008). Per-strategy instrument or product restrictions are not enumerated here.

## 12. Production vs testing
- Production is the authoritative baseline; the sandbox is comparison evidence only.
- HISTORICAL (Rama): the two were mostly the same apart from S&R changes and config values.
- A complete A-004 difference matrix: UNKNOWN. No sandbox difference overwrites production.

## 13. Historical changes
- HISTORICAL (Rama): exchange fixed at NSE; delivery enabled/disabled at times; instrument lookup unchanged.
- OPEN QUESTION: none of these is recorded as a dated change (no date/commit/before/after evidence): universe evolution, excluded_symbols history, delivery enable/disable history, F&O-related changes, lookup changes.

## 14. Cross-references
Broker fallback (angelone configured, fallback_enabled = false; actual live fallback use not established): A-006. Product types, GTT and DDPI: A-006 and A-007. Strategy count and membership: A-008 (Q-17). Configuration: A-009. Signal lookup and rejection: section C. Learned MIS blocklist: section D. Order products and tick handling: sections E and G.

## 15. Fact discipline
- FACT: NSE-oriented scope; Chartink signal source; Zerodha broker; the production instrument-reference pipeline (config/instruments.csv built by scripts/refresh_instruments.py at 09:00 IST weekdays); the exact equity series filter; instrument sources and build process; missing-match fallback values; the static exclusion mechanism; BOTH intraday and delivery configured, DELIVERY to CNC; configuration observations such as unsupported BRACKET_ORDER mapping with a retained leverage value; F&O reference/configuration exists.
- HISTORICAL (user-reported): NSE equity trading; delivery/CNC trading; Silver/Gold ETF trading; belief that the universe included SME; DDPI use; no F&O traded; the 10/5/1 strategy description.
- RECORD-DERIVED: testing-VM instruments.csv of 2,228 rows, all lot size 1.
- UNKNOWN / NOT ESTABLISHED: exact historical production instrument universe; SME inclusion; ETF path through the builder; actual F&O trading; actual use of every configured or reference instrument category; whether token-0 rows reached quote/order paths; whether the default tick caused a production failure; lookup key and path; invalid-symbol outcome; CO and BO use; production strategy count and membership (A-008); production/sandbox matrix; dated change history.
- INTERPRETATION / AUDIT LEAD: instruments.csv as a central reference layer; tick-size risk; token-0 risk; the SME series-code observation.
- KNOWN DEFECT: none newly declared under A-004 without direct runtime evidence.
- OPEN QUESTION: SME historical inclusion; A-008 strategy membership and count; other runtime instrument-path questions for the audit.

## 16. Final A-004 statement
> The legacy production trading baseline was NSE equity-oriented and consumed external Chartink signals through Zerodha. Production maintained a centrally generated NSE-equity-oriented instrument reference file, refreshed from NSE/reference data and the Zerodha Kite instrument dump. Both intraday/MIS and delivery/CNC were configured, with delivery/CNC historically used according to direct user contract-note evidence. Production also contained an explicit symbol-exclusion mechanism at the webhook edge. Actual historical F&O, CO, BO, and the complete exact instrument universe are not established by the evidence reviewed and are therefore not asserted as facts.

## 17. Freeze status
FROZEN 2026-10-06 (DR-019). The freeze does not claim every detail is known; it records the established baseline and bounds the unknowns.

## Changes from the original (23-Sep-2026)
Revision 2 (after ChatGPT review): evidence-boundary rule and evidence levels added (2); fallback values marked FACT with downstream use UNKNOWN (4.1); tick risk split into legacy fact, dated external context, interpretation and unknown, with the external dates verified (10-Jun-2024 and 15-Apr-2025; revision 1 wrongly said 2025 for the 1-paisa change) and builder-versus-order-math scope note (4.4); eligibility controls split into static exclusion, learned MIS blocklist and V2 decision (6); SME changed from "likely excluded" to UNKNOWN with the series-code point kept as an audit lead only, and Rama's statement quoted precisely (8); bracket leverage value labelled a configuration observation (10); strategy split marked user-reported, not a verified inventory (11); angelone live use marked not established (14); fact discipline aligned (15).
Revision 1: removed section 6.4 V2 input (now VP-27); added series filter and default values, testing-VM row count, data risks, Rama's unknown-symbol belief, learned MIS blocklist cross-reference, NIFTY 50 regime note, ETF and SME rows, DDPI and common-file notes, leverage-map observation, placeholder detail and Q-17 link, Rama's production/sandbox and change recollections, cross-references.

## Erratum (2026-10-23, found during C-004)
- E-1 (section 5, line 65: the receiver's symbol and price checks). Status: ACCEPTED, option (b). Proposed during C-004 (its research notes, 23-Oct-2026, candidate CE-1), decided by Rama on 23-Oct-2026 (multiple choice, about 08:05 IST: option (b), line 65 only; option (c), which would also have changed line 64, not taken), independently of the C-004 freeze and of the other erratum, and recorded in DR-057 (2026-10-23) together with the C-004 freeze. The struck text stays visible; the replacement is the text in force. Authoritative passages: C-004_manual.md sections 6 and 9.1 (the per-symbol checks; the instrument master not on the intake path) and C-001_manual.md section 15.
- What changed: "the receiver validates symbols/prices, and the instrument master is preloaded at start-up." became "the receiver checks a symbol only for being non-empty, after its alias lookup and the static exclusion list, and a price only for parsing as a number and not being zero or less; it does not look the symbol up in the instrument master, which is preloaded at start-up (C-001_manual.md section 15; C-004)."
- Passages checked and not changed: line 64 of section 5 (the path summary, option (c) not taken); the other "validat" in this manual (line 39, the builder's "row-set validation"), which is about the build.
- Reason: "validates symbols" can be read as a validity or instrument check, which did not exist at intake: the receiver's only symbol check is that the symbol is not empty, and it holds no reference to the instrument cache (C-004_manual.md sections 6.2 and 9.1).
- Impact: wording only; no frozen finding, count or decision changes, and no V2 decision depends on it.
