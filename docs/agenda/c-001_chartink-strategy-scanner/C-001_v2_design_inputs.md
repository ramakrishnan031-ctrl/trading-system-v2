# C-001 CHARTINK STRATEGY / SCANNER: V2 DESIGN INPUTS (PROPOSED)

Status: PROPOSED / OPEN, first pass 2026-10-21 (revision 0). It stays PROPOSED / OPEN after the C-001 manual's freeze (DR-053, 2026-10-21). Changed at the freeze: this sentence; the owner cells of rows 2, 4 and 7 (owner text only, after the check of every owner cell against the Master Agenda's topic titles; C-001_review_notes.md section 14); section 3's disposition column. The owner cell of row 11 corrected on 2026-10-25 at the Section C close (DR-066, S-3), owner text only. Nothing here is a V2 decision. Items become decisions only as register rows at their owning topics (W-6).
Labels: LEGACY = what the old system did (C-001_manual.md). RAMA = his words, quoted. DECIDED = an existing register row, quoted. CLAUDE CONSIDERATION = a question for the owner, not a requirement.

## 1. Rama's input of 21-Oct-2026
- RAMA (to ChatGPT, 21-Oct-2026, in the C-001 chat, shown in his screenshot): "From my side additional Input: Ignore existing legacy system's strategies since we need to implement new system with 4 intraday (2 short+2 long) and 2 delivery (2 long) strategies".
- DECIDED already. DR-031: "V2 starts, for the time being, with six strategies: four intraday (two long, two short) and two delivery (long only, DR-032)" and "The legacy 16-strategy model is not carried forward." DR-032: delivery is long only. DR-028: "Chartink scanner alerts are the only signal source (no internal signal generation)".
- Relation: the input agrees with DR-031 and DR-032 in count and mix, and with VP-08's A-008 record ("do not carry the 16-strategy model"). It opens no row and changes no decision.
- Effect on C-001: the 16 legacy definitions, with their names, URLs, conditions and behaviour, are evidence only; no legacy strategy is a V2 candidate by default (DR-031: "The legacy 16-strategy model is not carried forward"; W-1). Which six strategies V2 has, and their scanner conditions, stay with the owners DR-031 names ("Which strategies, their scanner conditions (VP-44), filtration (VP-03), S&R, R:R and the admission test (no subset or near-copy) stay with C-001, D-003, D-009 to D-017 and J"). C-001 records no V2 strategy; where those questions go after the C-001 freeze is a scheduling point for Rama (C-001_review_notes.md section 6, point 4).

## 2. Considerations carried to their owners
CLAUDE CONSIDERATION throughout: questions for the owner, not requirements.

| # | LEGACY finding (C-001_manual.md) | CLAUDE CONSIDERATION: question for the owner | Owner |
|---|---|---|---|
| 1 | Scanner identity sat in four places, the map key, the strategy name, the webhook path and the optional alert name, with a twin URL file beside them (sections 4.1 and 4.3) | Which single field is a V2 scanner's identity, and which other fields, if any, are derived from it or checked against it | VP-45; C-002, C-010; configuration design |
| 2 | The map's claimed start-up checks (S10 and the duplicate-name failure) did not run on any production path, and a duplicate YAML key would have been collapsed silently (none existed at the baseline; section 5) | Whether V2's configuration gate rejects duplicate, unmapped and orphaned scanner keys, and how that is shown to run at the production call site, not only in a test | VP-45, VP-47; configuration design; N-002 |
| 3 | Screener URLs were stored twice, marked "placeholders" by their own file and never verified; the URL check proved only that an address answered (sections 4.1 and 6) | Whether V2 stores screener addresses at all; if it does, where, how they are verified, and what a check of them would and would not prove | VP-45; C-002; configuration design |
| 4 | Two scanner keys never posted for seven weeks, which a later audit found; the dashboard's "Chartink" card tested "at least one signal today" across all scanners (sections 6.5 and 9) | Whether V2 watches arrivals per scanner (for example, an expected-arrival rule or a silence alarm), and who is told | C-002; L-003, L-004, L-007, L-010 |
| 5 | The start-up URL check only warned, and nothing acted on it; the operator script was never run, the system being headless (HISTORICAL, Rama; sections 6.2 to 6.4) | Which scanner checks V2 runs unattended, what each proves, and what each does on failure (DR-027: "Headless" means no routine manual operation) | M-006; VP-47 |
| 6 | The receiver stored the scanner key as the strategy, and attribution and replay read it as the strategy (section 7.2) | Whether V2 stores the scanner identity and the resolved strategy separately on every signal, and which of them attribution uses | C-007, C-010; K-003; L-010 |
| 7 | webhook_audit counted accepted and rejected symbols only for HTTP 200 responses, and counted EOD captures as accepted (section 8) | Whether V2 records every received symbol's outcome whatever the response, with captures that create no signal counted apart | VP-11, VP-59; C-006 to C-009; K-008, L-009 |
| 8 | The alert-name check was optional and normalised only case, spaces and an account prefix (section 7.6) | Under VP-45's single naming rule, whether the alert name is checked at all once a system-controlled field carries the identity | VP-45; C-002, C-004 |
| 9 | The webhook carried no side; direction came from the strategy YAML (section 4.3) | Whether V2 keeps direction in the strategy definition only (DR-031 fixes two long and two short intraday strategies) | C-010; VP-08 |
| 10 | No exchange field at the edge; aliases and exclusions applied there; "NSE" fixed in the code paths read (section 15) | Q-20 as it stands; no new consideration | Q-20; C-004 |
| 11 | HISTORICAL (Rama, 21-Oct): the alerts posted over plain HTTP, with the shared token in the URL's query string (section 9.2) | How V2 receives Chartink webhooks: the transport, and where the shared secret travels, against what Chartink supports at that time (a current external fact, to be checked and dated then; W-5) | whole-design review (with VP-50, VP-55, VP-56) |
| 12 | The dashboard re-read the map file while the trading process kept its start-time copy (section 5.1) | Whether the GUI shows the configuration that the running process loaded | L-010; configuration design (VP-15) |

## 3. Proposal and question rows that name C-001 (status unchanged)
| Row | Status on main | What C-001 adds (tracking only) | Disposition at the C-001 freeze (DR-053; scheduling only) |
|---|---|---|---|
| VP-08 | Count DECIDED (DR-031); the rest PROPOSED. Ratify at: count DECIDED (DR-031); C-001, D-003, D-009 to D-017, J | The legacy catalogue as evidence (manual section 4); Rama's 21-Oct input (section 1) | C-001 kept for the choice of the six strategies, reviewed at the Section C end decision paper; the other points unchanged |
| VP-44 | PROPOSED. Ratify at: C-001, D-001, D-013 to D-016, E-002 | No scan conditions found in the repository's code and configuration (S-CODE); two keys never posting in the arrival records (manual sections 4.1 and 9); nothing on signal timing beyond A-008 | C-001 kept for the six strategies' scanner conditions, reviewed at the Section C end decision paper; the other points unchanged |
| VP-45 | PROPOSED. Ratify at: C-001, C-002, C-010, configuration design (build phase) | The legacy identity facts and gaps (manual sections 4 to 7); considerations 1 to 3 and 8 | C-001 dropped; C-002, C-010 and configuration design remain |
| Q-20 | OPEN (area A-004 / C) | Edge evidence (manual section 15); stays OPEN | No change |
| Q-34 | OPEN (DR-016 audit) | (d): R-01 with Rama's account; (g): U-01 and U-02 unchanged (manual sections 9 and 15) | No change |

Rama decided the dispositions on 21-Oct-2026 (his answer: "Keep for Section C end (Recommended)"); they are recorded in DR-053 and applied in the design log. No proposal content changes, and this file opens no VP or Q row.

Status and boundary: PROPOSED / OPEN. Nothing here is a V2 decision.
