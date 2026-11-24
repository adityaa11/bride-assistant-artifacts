# M3-T05 — durable customer submission and Booking Saya read

- State: planned; batch 3; milestone/work package M3 / M3-W05.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `15134132805a5dce25ebd88287f96646110530e2`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T03 H-M3-03; M3-T04 H-M3-04; M2-T05 H-M2-05.
- Successors: M3-T07; M4 proof/exception consumers.
- Activation: `active-in-this-ticket`; legacy production fixture paths forbidden.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

durable customer submission and Booking Saya read. Owns M3-W05; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp and later M4/M5 activation. This is one reviewable authority boundary.

## Entrypoint

Authenticated customer booking form submits untrusted date, location, package ID and optional promotion ID through T05 validation/actor boundary. Booking Saya uses the persisted session actor and trusted booking owner lookup; browser prices/owner/status are never authority.

## Endpoint

Successful submit commits one Menunggu Konfirmasi record and initial history before a prepared wa.me link is offered; own Booking Saya list/detail reads that record. Typed validation/conflict/absence errors show without losing durable state.

## Handoff

H-M3-05: booking ID, owner ID, selected wedding date/location, immutable snapshot, status and history; M3-T07 checks availability and M4 attaches proof/exceptions by booking ID. Optional wa.me URL uses only approved E.164 config and encoded message; with D-04 pending, live link stays disabled while record remains durable. GO records the exact schema/type, required fields, identity/correlation, retry/idempotency and consumer integration evidence.

## Expected use path

`EUP-M3-T05`: Protected customer route → T05 actor/input → T03 authoritative selection + T04 booking transaction → committed booking ID → Booking Saya own-record read and optional wa.me preparation. Remove production localStorage/scenario submission. GO reports the actual composition-to-effect path; CK inspects it. Unit-only callers are insufficient.

## True path and legacy

The path above is authoritative. Remove or make unreachable production scenario/localStorage behavior for this capability. Declared M1 review routes remain non-authoritative; no fixture fallback is allowed.

## SQL migration hygiene

No intended migration beyond confirmed T01/T04. Disposable Docker booking records and rollback/teardown. Any additional SQL needs revised ERD confirmation. SQL-dependent GO/CFC must commit a rendered actual ERD, link migration files/versions and compare against a fresh migrated database; CK inspects this. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not authorization.

## Empty and initial state

`EMPTY-M3-T05`: No bookings renders empty Booking Saya; after first submission, the same account sees it after reload, another account gets no details. Missing WhatsApp config does not undo submission. Use disposable records and teardown, never a durable seed.

## Required invariants and non-goals

Preserve H-M2-05 actor/ownership/Zod validation, H-M2-06 repository errors, one shared booking ID, immutable snapshots, admin-only official decisions, and pending-booking non-reservation as applicable. Use local Docker verification and existing M2 provider baseline; add a remote check only for a changed provider path or explicit provider-specific criterion, recording its reason and minimum scope.

## Reuse and centralization

Apply set decisions `RD-M3-01` T05 actor/input, `RD-M3-02` protected repository result, and `RD-M3-03` separate shells. Domain transactions and audience-specific presentation stay with their ticket owner. No generic business repository or duplicate availability decision.

## Acceptance criteria

- `AC-M3-T05-A` (Project booking flow; M3-W05): Valid submission persists one canonical record before WhatsApp preparation; invalid package/promo/date/fields fail without partial record. Review row `RC-M3-T05A`.
- `AC-M3-T05-B` (Project privacy/shared record; M2 H-M2-05): Booking Saya shows only the owner’s durable records/status/snapshot; cross-customer and anonymous reads are denied. Review row `RC-M3-T05B`.
- `AC-M3-T05-C` (M1 UI reference; M3-W05): Customer booking/Booking Saya UI preserves reference hierarchy and exposes loading/error/success/empty states accessibly. Review row `RC-M3-T05C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T05A` | Project booking flow; M3-W05 | PASS iff Valid submission persists one canonical record before WhatsApp preparation; invalid package/promo/date/fields fail without partial record. | End-to-end route + DB read, no-number case, invalid selection and retry observation. GO records route/DB observations; CK independently checks them. | wa.me or localStorage as record. | T03 eligibility and T04 atomic create. |
| `RC-M3-T05B` | Project privacy/shared record; M2 H-M2-05 | PASS iff Booking Saya shows only the owner’s durable records/status/snapshot; cross-customer and anonymous reads are denied. | Two-customer and anonymous route tests, list/detail payload inspection, reload and empty state. GO records route/DB observations; CK independently checks them. | Client-side filtering or ID obscurity. | M2 session/role denials. |
| `RC-M3-T05C` | M1 UI reference; M3-W05 | PASS iff Customer booking/Booking Saya UI preserves reference hierarchy and exposes loading/error/success/empty states accessibly. | Current desktop/mobile captures and keyboard/focus/state checks on changed screens. GO records route/DB observations; CK independently checks them. | Fixture status masquerading as saved result. | M1 customer visual baseline. |

## Security readiness

SEC-M3-05 implement-now: owner-scoped reads, authenticated submission and server revalidation of price/promo/date; PII excluded from other customers (Project/M2 H-M2-05; ASVS access/input/data, OWASP access control, NIST AC/PT). SRB-M3-05 binds A/B to cross-account route/DB evidence. Preserve seam: no proof upload or exception workflow in M3. Scoped baseline: OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status applicable; no planning-review-required finding. D-03 production proof approvals remain M4/M5 gates; D-04 keeps the live WhatsApp link disabled until configured.

## UI readiness

Frontend-awareness State 2, Mode A: use M1 customer booking/Booking Saya reference and Soft Rose Atelier; inspect actual changed routes and accessible states. D-04 leaves link disabled without changing saved-booking feedback. For changed UI, apply frontend-awareness VIS-001–014 and M1 reference mapping; capture only touched routes/states at the code revision. Preserve CSP/browser execution boundary and avoid new unsafe HTML sinks or unreviewed origins. Backend-only work may cite unchanged M1 captures.

## Validation and hard stop

Run focused disposable PostgreSQL integration/route tests, direct negative/empty cases, affected lint/typecheck/test/build checks, use-path inspection and migration inventory. Stop for a changed business rule, unresolved schema relationship, invalid M2 handoff or missing ERD confirmation.
