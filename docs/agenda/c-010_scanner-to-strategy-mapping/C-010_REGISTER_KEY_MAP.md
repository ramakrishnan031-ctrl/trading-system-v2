# C-010 - SOURCE REGISTER: COMPANION KEY MAP

Status: FROZEN 2026-10-24 with C-010_manual.md (DR-063). Made under action A of ChatGPT's final review, C-010_CHATGPT_FINAL_REVIEW_24-OCT-2026.txt (SHA-256 58E2BC3FBA62F9E6D68D31E4017FBB7515419C3E34F488970FF0A18ABF17EB08), which confirmed the key map proposed in C-010_manual.md section 3.  
Topic: C-010 Scanner-to-strategy mapping (master agenda, section C). Legacy system status: DEAD / REFERENCE ONLY.

## 1. What this file is

- The C-010 source register is recorded in C-010_CLAUDE_STAGE1E_EVIDENCE_SHEET_24-OCT-2026.txt, section 3, lines 97 to 150: the header, the separator and 52 rows (SHA-256 of those 54 lines: EE1D615C0FB2127535F08F15FA0A5E2DCA79FAB98EC67A6CA7C941D73C00490D). The sheet is stored unchanged beside this file (SHA-256 162E54B20C955FA9D2CD2D7C306641D2124F59699B4CA20A763B849329CA69DE), and C-010_manual.md section 3 reproduces the same 54 lines byte for byte. This file changes neither.
- Seven recorded IDs each cover more than one file: R13, R14, R17, R19, R20, R23 and R24 (19 rows). The three test-file rows share the marker T. In the record these stay UNRESOLVED clerical collisions (Stage 1D reconciliation, item 4; manual-draft prompt, item 7). This file gives each such row a distinct key beside its recorded ID, for reference only.
- Rule: a reference to one of these rows gives its distinct key together with its recorded ID, for example "R17.3 (recorded R17: ops_dashboard/backend/services/strategy_ranking.py)". Every other ID in the register is unique and is its own key. No recorded row, ID or text changes.

## 2. Distinct keys (in recorded order)

| Distinct key | Recorded ID | Path, as recorded | Pinned blob, as recorded |
|---|---|---|---|
| R13.1 | R13 | config/strategy_direction_registry.yaml | b03f4ed0c90043840f339fc9b5dac6cca93bf736 |
| R13.2 | R13 | core/strategy_direction.py | 0e6f8b6686f393dcf38ddf036e565190cec0863a |
| R14.1 | R14 | scripts/replay_signals.py | 72763c3ed8d5d8fe43258619d55840f2780cfa96 |
| R14.2 | R14 | scripts/strategy_registry_officer.py | c013518bc49c2323922292af5fa9eeacb3cc4d0b |
| R17.1 | R17 | ops_dashboard/backend/services/strategy_meta.py | 8d7324317a3a44ec610647daf9dced6079a64d77 |
| R17.2 | R17 | ops_dashboard/backend/services/strategy_tower.py | 6effe8732f5284d37a318d9fcc9ddedd9e288070 |
| R17.3 | R17 | ops_dashboard/backend/services/strategy_ranking.py | 61f44251c094be199a9f6cb7dd26863aa4c461f7 |
| R17.4 | R17 | ops_dashboard/backend/services/strategy_health.py | 8a7264e5db2a2aacade9a4bbabe1ef99e5a5ebe6 |
| R17.5 | R17 | ops_dashboard/backend/services/strategy_score.py | 79c55db1987d956e2943e5710deadfa68b43b2e1 |
| R17.6 | R17 | ops_dashboard/backend/api/strategies.py | 8fac396665d64d425a28944eec27744247fc5c6c |
| R19.1 | R19 | scripts/strategy_status.py | d15942ccde89d30e45df310a643ba0dacec17bce |
| R19.2 | R19 | scripts/compute_strategy_metrics.py | c081a0a8f76804301ada8d07733cf583f1c99287 |
| R20.1 | R20 | strategies/__init__.py | ee3a2879f0fa022d22fdd62d9aed206964b3e64b |
| R20.2 | R20 | strategies/taxonomy.py | 069644bc1bdb464ffe5faec31f710acdc02e4837 |
| R23.1 | R23 | scripts/preflight_scanner_check.py | 3151263d6e247b04884e9fbe99f3cd0e44725167 |
| R23.2 | R23 | utils/startup_checks.py | 61ff67676d8230640a2aafb6bd6d75d7a185fbb6 |
| R24.1 | R24 | core/account_registry.py | d2bdc836cc9a04a99d4d179dba4f46e78213c84c |
| R24.2 | R24 | orders/eod_squareoff.py | 54f6cd6223353df0e9ad8bc823472f28b19b3818 |
| R24.3 | R24 | signals/entry_throttle.py | b887a4be78a4dcfe3c2541d7528bd3b96ec351ea |
| T.1 | T | tests/unit/test_signal_processor.py | 3ec0ea84392a193d18b992ca88df0da5e6235f67 |
| T.2 | T | tests/unit/test_webhook_receiver.py | 7b377abb85d644f0f3ff7956385224b43f31ed61 |
| T.3 | T | tests/unit/test_config_loader.py | 8f245c35a047794c00ffe21e53c178b5928af890 |

## 3. Notes carried beside the register (the recorded texts unchanged)

| Recorded ID | Recorded role text, verbatim | Note |
|---|---|---|
| R15 | dashboard queries on signals | Narrower than the evidence held: reads 038 to 040 also show this file's queries on trades (C-010_manual.md section 11.3). |
| R28 | none in the chain (two comment lines) | The Stage 1B ledger records the two lines as a docstring and a comment (C-010_manual.md section 11.5). |
| R33 | 1C: OrderPlacer.place, the callee of the processor's place(...) (HISTORICAL); 1D: its create_trade call (996) with strategy=strategy (1000); 1E: line 996 the only create_trade( call line in s_rt (INTERPRETATION, 6.1) | That line 996 lies in OrderPlacer.place is INTERPRETATION: place() begins at line 871 (HISTORICAL, C-008 4.1, ST-29); C-008's cited range, 871 to 958, does not reach 996, and the definitions between 871 and 996 were not read (C-010_manual.md section 8.1, H9). |

Legacy remains DEAD / REFERENCE ONLY. No V2 decision is made here.
