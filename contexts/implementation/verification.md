# Cross-phase Verification and Readiness

**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `387dde9aae69fe37795935b798e324e0fa315e3d`.

| Concern | Evidence | Gate |
| --- | --- | --- |
| UI/fixture contract | Named scenarios, responsive visual review, loading/empty/error states, no direct fixture import in features; T18 route-level upload/read/replace/cleanup, isolated roots and public/customer negatives for Portfolio, Testimonials and Events | M1 and regression thereafter |
| Identity/authorization | Server-side role and cross-customer denial tests | M2 onward |
| Lifecycle/availability | Valid/invalid transitions, transaction/concurrency conflict, reschedule date-release/history tests | M3/M4/M5 |
| Historic integrity | Snapshot and promotion regression after catalogue changes | M3/M5 |
| Private bytes/content | Every uploaded proof/content byte passes through the `DocumentStore` contract; metadata/file failure, private-byte denial, draft exclusion, permission/retention evidence, ordered proof/booking audit events, and an authorized PDF/download range-response regression check | M2 contract; M4/M5 production |
| Operations | Environment/secrets, migration and backup/restore/recovery rehearsal, runbooks and signals | M2/M5 |

## Milestone gates

- M1: reviewed fixture scenarios, three T18 fixture media paths and their local `DocumentStore` byte seam, UI/backend replacement map and public/customer negatives; failure means revise fixtures/contracts.
- M2: approved D-01/D-02, protected-route/role denial, migration and adapter-contract evidence; failure blocks backend-dependent tickets.
- M3: end-to-end same-record workflow plus snapshot and conflict tests; failure blocks M4/M5 progression.
- M4: proof/content privacy, request/approval and publication evidence; missing D-03 keeps proof disabled rather than waiving the gate.
- M5: every Project Context acceptance row has a result; any critical authorization, privacy, lifecycle, availability, or restore failure is no-go.

## Readiness split

Implementation-context readiness is `ready` for the bounded M1 continuation: Project v3.1, Stack v1.3 and T18-A2/A3 agree on the fixture-only `DocumentStore` seam. Ticket-generation readiness is `ready` for M1 only; M2–M5 remain gated by their direct predecessor and production decision evidence. Their deferred policy does not authorize production media access in M1.

