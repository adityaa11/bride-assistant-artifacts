# H-M2-07-TECH v2.0 — configurable proof foundation

- Source repository: https://github.com/adityaa11/bride-assistant.git at commit `15134132805a5dce25ebd88287f96646110530e2`.
- Owner: M2-T07 revision 2.0. State: technical capability submitted for CK; policy approval, operational readiness and activation remain false.
- Policy schema/version: `H-M2-07-TECH/2.0` in `shared/proof-policy.ts`. Profile shape and values: `config/proof-retention.profiles.json`. The JSON has only adapter classes, not endpoints or secrets.

## Shared contract

Trusted deployment code calls `identityFromDeployment`, then `validateProfile` with a profile and decision ledger. Validation rejects a forged or unknown identity, environment mismatch, incompatible adapter/data/credential/clock combinations, short production retention and missing production approvals. Only a validated runtime value reaches `runProofLifecycle`. It composes classified proof facts, clock, byte deletion, metadata/audit removal and deletion-record ports. The byte read contract remains T06 `DocumentStore`; deletion is an injected lifecycle port above it. Domain behavior has no environment-name branch.

Operational proof expiry is eight calendar months after final review or upload when unreviewed. Synthetic statutory records require ten calendar years from an explicitly supplied statutory start event. Unknown classification, absent statutory start, active hold and quarantined scanner outcomes deny deletion. Scanner clean is eligible; infected, unavailable and timeout quarantine. The five-minute development profile is disposable only. The test profile runs authoritative durations with a controlled clock without activating production.

The technical coordinator needs M4's durable implementation to make byte and metadata/audit deletion one recoverable operation. M4 owns retry/transaction semantics, production persistence, provider removal and live caller proof. It must retain booking status history independently.

## Pending D-03 decisions

| Decision ID | Owner ticket | Approved value | Accountable owner | Date | Evidence |
| --- | --- | --- | --- | --- | --- |
| `pdpBasisNoticeContact` | M4-W07 | pending | pending | pending | pending |
| `scannerProviderTerms` | M4-W07 | pending | pending | pending | pending |
| `holdCriteria` | M4-W07 | pending | pending | pending | pending |
| `neonRpoRtoBackupRestoreOwner` | M5-W03 | pending | pending | pending | pending |
| `indonesianTenYearRetentionDetermination` | M4-W07 | pending | pending | pending | pending |

These slots do not select a PDP basis, scanner, hold exception, recovery target or statutory classification. Production deletion and activation remain denied while required decisions or a real record's applicability are unknown.

## Readiness and consumers

| State | Current | Owner / consumer |
| --- | --- | --- |
| `technicalReady` | awaiting CK | M2-T07; M4-W01/W02 consume after CK PASS |
| `policyApproved` | false | M4-W07 and M5-W03 provide the five accountable approvals |
| `operationalReady` | false | M4-W01/W02 prove live caller, Neon byte/SQL adapters and controls; M5-W03 proves recovery |
| `activationAuthorized` | false | M5-W05 explicit go/no-go |

M4-W01 supplies the live private-byte deletion adapter and any SQL migration under the separate ERD confirmation contract. M4-W02 supplies proof review/audit consumers. M4-W07 owns PDP, scanner, hold and statutory controls. M5-W03 owns provider recovery evidence. M5-W05 alone authorizes release. This handoff contains no production bytes, file names, signed URLs or credentials.
