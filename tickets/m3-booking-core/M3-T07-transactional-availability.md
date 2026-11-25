# M3-T07 — transactional date protection and admin closure

- State: planned; batch 4; milestone/work package M3 / M3-W07.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T05 H-M3-05; M3-T06 H-M3-06.
- Successors: M4 exception/reschedule and M5 release.
- Activation: `active-in-this-ticket`; legacy production fixture paths forbidden.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

transactional date protection and admin closure. Owns M3-W07; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp and later M4/M5 activation. This is one reviewable authority boundary.

## Entrypoint

Customer availability read receives a validated date range and returns only date status. Admin closure/open command uses T05 admin actor and date; booking submission/Booking Berhasil transition calls the same server-owned conflict decision inside PostgreSQL transaction.

## Endpoint

Admin closure or Booking Berhasil makes a date unavailable; pending bookings do not. Conflicting closure/success/concurrent success is rejected atomically. Customer response contains dates/availability only.

## Handoff

H-M3-07: date-only unavailable projection and transactional booking/closure conflict invariant; M4 reschedule/cancellation uses booking ID and this same decision to release/retain dates; M5 verifies same-record end-to-end behavior. Ordering is transaction commit order; failed/retried commands have no partial closure or status event. GO records the exact schema/type, required fields, identity/correlation, retry/idempotency and consumer integration evidence.

## Expected use path

`EUP-M3-T07`: Customer/admin route → T05 actor/input → shared transactional availability decision → closed-date and booking state in PostgreSQL → date-only customer calendar/admin result. Retire separate fixture availability decisions in production. GO reports the actual composition-to-effect path; CK inspects it. Unit-only callers are insufficient.

## True path and legacy

The path above is authoritative. Remove or make unreachable production scenario/localStorage behavior for this capability. Declared M1 review routes remain non-authoritative; no fixture fallback is allowed.

## SQL migration hygiene

M3-SQL-07 intended migration: admin_closed_date table and successful-booking date uniqueness/index as proposed in [migration proposal](migration-proposal.md). Migrate only after rendered ERD confirmation; Docker concurrency tests use disposable rows and teardown. SQL-dependent GO/CFC must commit a rendered actual ERD, link migration files/versions and compare against a fresh migrated database; CK inspects this. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not authorization.

## Empty and initial state

`EMPTY-M3-T07`: With zero bookings/closures, valid future dates are available. First closure immediately changes customer projection; first pending booking does not; first successful booking does. An absent closure open request is idempotent. Use disposable records and teardown, never a durable seed.

## Required invariants and non-goals

Preserve H-M2-05 actor/ownership/Zod validation, H-M2-06 repository errors, one shared booking ID, immutable snapshots, admin-only official decisions, and pending-booking non-reservation as applicable. Use local Docker verification and existing M2 provider baseline; add a remote check only for a changed provider path or explicit provider-specific criterion, recording its reason and minimum scope.

## Reuse and centralization

Apply set decisions `RD-M3-01` T05 actor/input, `RD-M3-02` protected repository result, and `RD-M3-03` separate shells. Domain transactions and audience-specific presentation stay with their ticket owner. No generic business repository or duplicate availability decision.

## Acceptance criteria

- `AC-M3-T07-A` (Project D7; M3-W07): Admin-only closure/open is durable and blocks/selects dates correctly without requiring a booking; customer cannot close dates. Review row `RC-M3-T07A`.
- `AC-M3-T07-B` (Project D2/D7; M3-W07): Booking Berhasil date conflicts and closure races resolve transactionally; pending bookings alone leave date available. Review row `RC-M3-T07B`.
- `AC-M3-T07-C` (Project privacy and M1 UI contract): Customer availability exposes no identity/details and both customer/admin UI show current server decision with usable calendar states. Review row `RC-M3-T07C`.
- `AC-M3-T07-D` (AGENTS.md SQL contract): Fresh schema matches confirmed rendered ERD and all data is disposable; actual SVG/PNG, migration links and DB comparison are in GO/CK. Review row `RC-M3-T07D`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T07A` | Project D7; M3-W07 | PASS iff Admin-only closure/open is durable and blocks/selects dates correctly without requiring a booking; customer cannot close dates. | Two-role route tests, DB state and cross-surface date projection before/after. GO records route/DB observations; CK independently checks them. | Fixture blockedDates or client-only disable. | T05 auth and T06 booking decisions. |
| `RC-M3-T07B` | Project D2/D7; M3-W07 | PASS iff Booking Berhasil date conflicts and closure races resolve transactionally; pending bookings alone leave date available. | Concurrent local PostgreSQL transactions: two successes, closure versus success, pending versus selection, rollback/retry; inspect unique invariant and history. GO records route/DB observations; CK independently checks them. | Serial-only unit tests or precheck without lock/constraint. | T04 status/history and T05 submission. |
| `RC-M3-T07C` | Project privacy and M1 UI contract | PASS iff Customer availability exposes no identity/details and both customer/admin UI show current server decision with usable calendar states. | Route payload negative inspection; current customer/admin browser captures at desktop/mobile, keyboard/status/empty/error checks. GO records route/DB observations; CK independently checks them. | Leaking booking ID/name or fixture availability fallback. | M1 calendars/admin availability baseline. |
| `RC-M3-T07D` | AGENTS.md SQL contract | PASS iff Fresh schema matches confirmed rendered ERD and all data is disposable; actual SVG/PNG, migration links and DB comparison are in GO/CK. | Fresh Docker migration/schema inspection and rendered checkpoint ERD; migration inventory; no test-only seed. GO records route/DB observations; CK independently checks them. | Mermaid/file path alone; unconfirmed migration. | T01/T04 schema and M2 ledger. |

## Security readiness

SEC-M3-07 implement-now: admin-only closure, transactional conflict protection, date-only customer projection (Project D7/M3-W07; ASVS access/business logic/data, OWASP access control, NIST AC/SI/PT). SRB-M3-07 binds A–D to adversarial concurrency and payload evidence. Preserve seam: M4 reschedule release uses same transaction boundary; no temporary pending hold. Scoped baseline: OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status applicable; no planning-review-required finding. D-03 production proof approvals remain M4/M5 gates; D-04 keeps the live WhatsApp link disabled until configured.

## UI readiness

Frontend-awareness State 2, Mode A: preserve M1 customer calendar and admin availability workspace, with fresh rendered state/viewport checks for replaced data path. For changed UI, apply frontend-awareness VIS-001–014 and M1 reference mapping; capture only touched routes/states at the code revision. Preserve CSP/browser execution boundary and avoid new unsafe HTML sinks or unreviewed origins. Backend-only work may cite unchanged M1 captures.

## Validation and hard stop

Run focused disposable PostgreSQL integration/route tests, direct negative/empty cases, affected lint/typecheck/test/build checks, use-path inspection and migration inventory. Stop for a changed business rule, unresolved schema relationship, invalid M2 handoff or missing ERD confirmation.
