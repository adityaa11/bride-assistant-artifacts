# Cross-phase Verification and Readiness

| Concern | Evidence | Gate |
| --- | --- | --- |
| UI/fixture contract | Named scenarios, responsive visual review, loading/empty/error states, no direct fixture import in features | M1 and regression thereafter |
| Identity/authorization | Server-side role and cross-customer denial tests | M2 onward |
| Lifecycle/availability | Valid/invalid transitions, transaction/concurrency conflict, reschedule date-release/history tests | M3/M4/M5 |
| Historic integrity | Snapshot and promotion regression after catalogue changes | M3/M5 |
| Private bytes/content | Metadata/file failure, private-byte denial, draft exclusion, consent/retention evidence, ordered proof/booking audit events, and an authorized PDF/download range-response regression check | M4/M5 |
| Operations | Environment/secrets, migration and backup/restore/recovery rehearsal, runbooks and signals | M2/M5 |

## Milestone gates

- M1: reviewed fixture scenarios and UI/backend seam map; failure means revise fixtures/contracts.
- M2: approved D-01/D-02, protected-route/role denial, migration and adapter-contract evidence; failure blocks backend-dependent tickets.
- M3: end-to-end same-record workflow plus snapshot and conflict tests; failure blocks M4/M5 progression.
- M4: proof/content privacy, request/approval and publication evidence; missing D-03 keeps proof disabled rather than waiving the gate.
- M5: every Project Context acceptance row has a result; any critical authorization, privacy, lifecycle, availability, or restore failure is no-go.

## Readiness split

Implementation-context readiness is `needs-review`: the plan safely starts M1 but has material deferred production decisions. Ticket-generation readiness is `ready` for M1 only; `not-ready` for M2–M5 until their direct dependencies and decision evidence are recorded.

