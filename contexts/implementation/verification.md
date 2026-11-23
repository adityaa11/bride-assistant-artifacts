# Cross-phase Verification and Readiness

**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `79b3478995737dd73165afaba50369dcb5242a08`.

| Concern | Evidence | Gate |
| --- | --- | --- |
| UI/fixture contract | Named scenarios, responsive visual review, loading/empty/error states, no direct fixture import in features; T18 route-level upload/read/replace/cleanup, isolated roots and public/customer negatives for Portfolio, Testimonials and Events | M1 and regression thereafter |
| Architecture and code efficiency | M1.1 source/use-path audit, reproducible finding register, targeted measurement for efficiency claims, explicit dispositions, conditional ticket reviews, and direct M1 contract regressions | M1.1 before M2 execution |
| Identity/authorization | Server-side role and cross-customer denial tests | M2 onward |
| Lifecycle/availability | Valid/invalid transitions; admin closure without a booking, successful-booking reservation, pending-booking non-reservation, cross-surface date projection, transaction/concurrency conflict, customer privacy, and reschedule release/history tests | M3/M4/M5 |
| Historic integrity | Snapshot and promotion regression after catalogue changes | M3/M5 |
| Private bytes/content | Every uploaded proof/content byte passes through the `DocumentStore` contract; metadata/file failure, private-byte denial, draft exclusion, permission/retention evidence, ordered proof/booking audit events, and an authorized PDF/download range-response regression check | M2 contract; M4/M5 production |
| Operations | Environment/secrets, migration and backup/restore/recovery rehearsal, runbooks and signals | M2/M5 |

## Milestone gates

- M1: reviewed fixture scenarios, three T18 fixture media paths and their local `DocumentStore` byte seam, UI/backend replacement map and public/customer negatives; failure means revise fixtures/contracts.
- M1.1: audit findings are evidenced and disposed; accepted M1.1 refactors pass bounded review and preserve M1 behavior, visual fidelity, accessibility, security, and media boundaries; a justified no-refactor decision closes the conditional branch. Unresolved impact on M2 seams blocks exit.
- M2: approved D-01/D-02, protected-route/role denial, migration and adapter-contract evidence; failure blocks backend-dependent tickets.
- M3: end-to-end same-record workflow plus snapshot and conflict tests, including both admin closure and successful-booking unavailability without a pending-booking hold; failure blocks M4/M5 progression.
- M4: proof/content privacy, request/approval and publication evidence; missing D-03 keeps proof disabled rather than waiving the gate.
- M5: every Project Context acceptance row has a result; any critical authorization, privacy, lifecycle, availability, or restore failure is no-go.

## Readiness split

Implementation-context readiness is `ready` for the M1.1 audit scope: Project v3.2, Stack v1.3 and M1/H-17 evidence establish the fixture baseline; M1-W07 records T17-A7 finding dispositions. Ticket-generation readiness is `ready` for M1.1-W01 audit only. M1.1 refactor tickets require the versioned audit and dispositions first; M2–M5 remain gated by their direct predecessor and production decision evidence. Their deferred policy does not authorize production media access in M1 or M1.1.

