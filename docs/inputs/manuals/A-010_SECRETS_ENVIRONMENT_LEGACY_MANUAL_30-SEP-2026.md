# A-010 — SECRETS / ENVIRONMENT
## LEGACY SYSTEM TECHNICAL MANUAL — 30-SEP-2026

STATUS: CLOSED / FROZEN
SYSTEM STATUS: DEAD / REFERENCE ONLY

## 1. PURPOSE
This chapter documents the legacy credential, environment, authentication, security-monitoring and secret-handling model. It does not redesign the system.

## 2. ARCHITECTURE
```text
Tracked Git configuration
   ├─ business/system settings
   └─ credential references / env-var names
                 |
                 v
          VM environment / .env
                 |
        +--------+--------+--------+
        |        |        |        |
     Zerodha  Telegram  Email   Chartink/
     adapter  notifier  SMTP    webhook consumers
        |
        v
 broker orders / positions / funds / reconciliation

security_monitor
        |
        +--> Telegram alerts
        +--> email/sentinel where applicable
        +--> security/copy audit evidence
```

## 3. SOURCE → INPUT → LOGIC → OUTPUT → HANDOFF

### 3.1 Broker credentials
SOURCE: `config/accounts.csv` + environment/.env.
INPUT: broker metadata, env-var references, actual secret values outside tracked config.
LOGIC: broker adapter consumes environment-backed authentication material.
OUTPUT: authenticated broker API/session or authentication failure.
HANDOFF: Zerodha adapter to order, position, funds, margin and reconciliation consumers.

### 3.2 Telegram
SOURCE: environment variables.
INPUT: bot token and channel/chat identifiers.
LOGIC: `alerts/telegram_notifier.py` obtains environment-backed credentials.
OUTPUT: alert delivery/failure path.
HANDOFF: security monitor and other alert producers.

### 3.3 Email/SMTP
SOURCE: environment variables plus YAML references.
INPUT: SMTP credentials and recipient/sender configuration.
LOGIC: password resolved from environment rather than plaintext YAML.
OUTPUT: email delivery/failure path.

## 4. CREDENTIAL MODEL
Production evidence establishes account metadata and environment-variable references in tracked configuration, while actual secrets are outside Git. The user confirms the VM `.env` contained actual operational credentials.

Actual secret values, tokens, TOTP secrets, webhook secrets and passwords are intentionally omitted.

## 5. ACCOUNT MODEL
Multiple Zerodha account metadata/credential-reference records existed. User confirms only the active trading account was populated/used for actual live trading. Exact activation history is not established.

## 6. ZERODHA AUTHENTICATION
Normal mode was headless/automated. A manual token creation/upload fallback existed. User recalls automatic old-token cleanup and next-boot token creation, but approximate times are not frozen.

Authentication failure is recognized by the adapter. Exact retry/recovery behavior and exact impact on existing-position management remain UNKNOWN.

## 7. ENVIRONMENT LOADING
`core/config_loader.py` does not read `.env`. Consumer modules read environment variables. The precise VM/service mechanism that injected `.env` values into the process environment is UNKNOWN.

## 8. SECRET ROTATION
Historical operator practice:
1. temporary credential input outside repository;
2. VS Code Claude reads it;
3. values stored into `.env`;
4. temporary file deleted.
The same general method was used for rotation. This is historical practice, not proof of secure governance.

## 9. DIRECT .ENV EDITS
User reports occasional direct VM editing through VS Code Claude. Exact restart/reload/rebaseline handling is UNKNOWN.

## 10. GIT PROTECTION
`.gitignore` excludes `.env` and credential artifacts. Repository documentation describes a pre-commit secret scanner, but consistent historical activation is UNKNOWN.

## 11. SECURITY MONITOR
`scripts/security_monitor.py` checks sensitive security state including `.env`-related file hashes, SSH/authentication activity and copy-protection state. It can send Telegram alerts and critical email/sentinel notifications. It is alert-oriented; a prior-authorization gate is not established by the legacy evidence.

## 12. COPY PROTECTION
Repository security configuration includes VM↔PC copy-protection and audit mechanisms. User reports using the protected workflow. Complete historical enforcement for every transfer is NOT ESTABLISHED.

## 13. BACKUPS
User reports no intentionally maintained independent credential backup store. This is user historical information, not independently verified.

## 14. VS CODE / CLAUDE
User confirms actual secret values were sometimes supplied through temporary files outside the repository for Claude-assisted storage into `.env`, followed by deletion. No secret values are recorded.

## 15. TESTING VS PRODUCTION
User reports the same general credential model on both VMs. Repository comparison does not establish equality of actual secret values; those values are not inspected.

## 16. OPERATIONAL FAILURE BEHAVIOR
Telegram unavailable: user reports fallback worked.
Email unavailable: UNKNOWN.
Broker credential failure: UNKNOWN.
Missing environment variable at startup: UNKNOWN.
Existing-position behavior during invalid broker authentication: UNKNOWN.

## 17. LEGACY FINDINGS
FACT: credentials were kept outside tracked business configuration.
FACT: environment variables were the runtime credential interface.
FACT: security monitoring included `.env`.
FACT: copy-protection/audit controls existed.
HISTORICAL: temporary plaintext credential files were used during maintenance/rotation.
UNKNOWN: exact environment injection and several failure/reload/authorization paths.

## 18. FUTURE / V2 INPUTS — NOT DECISIONS
Retain for later governance/design:
- evaluate secret manager/vault versus `.env`;
- least-privilege secret access;
- authorized/audited secret changes;
- rotation without plaintext temporary files;
- startup validation and explicit degraded/fail-closed behavior;
- safe secret reload/restart;
- secret redaction everywhere;
- exact broker-auth failure state machine;
- auditable configuration/security change history.

## 19. CROSS-TOPIC MEMORY PRESERVATION
Chartink scanner naming differed between Production and Testing, including a Testing-side broker-ID marker. Exact normalization location is not established here. Do not record webhook tokens.

A-009 remains authoritative for configuration architecture.
A-006 remains the primary broker-model chapter.

## 20. END-TO-END
```text
Account metadata
      |
      v
Environment/secret population
      |
      v
Process environment
      |
      +--> Zerodha authentication --> broker API consumers
      +--> Telegram
      +--> Email/SMTP
      +--> Chartink/webhook consumers
      |
      v
Security monitor --> Telegram/email/sentinel + audit evidence
```

## 21. FREEZE
> The legacy system separated business configuration from actual credentials, with credentials maintained outside Git and consumed through environment variables/.env by individual modules. Zerodha authentication was intended to operate headlessly, with a manual token-upload fallback; security monitoring and copy-protection mechanisms existed. Detailed failure, reload and authorization behavior that could not be established from evidence remains UNKNOWN.

No V2 security architecture is frozen by this manual.
