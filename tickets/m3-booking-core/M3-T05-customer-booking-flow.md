# M3-T05 — durable customer submission and Booking Saya read

- State: approved ([CK PASS](../../feedback/M3-T05-customer-booking-378248c-ck.md)); batch 3; milestone/work package M3 / M3-W05. [GO checkpoint](../../checkpoints/M3-T05-customer-booking-378248c.md) at code commit `378248c1945f3f75b5b1d67f6545d0358f57fee7`.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T02 H-M3-02; M3-T03 H-M3-03; M3-T04 H-M3-04; M2-T05 H-M2-05.
- Successors: M3-T07; M4 proof/exception consumers.
- Activation: `preparation-only` → M3-T07 activates customer submission and Booking Saya with authoritative date protection; legacy production fixture paths remain forbidden at activation.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

durable customer submission and Booking Saya read. Owns M3-W05; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp and later M4/M5 activation. This is one reviewable authority boundary.

## Entrypoint

Authenticated customer booking form submits untrusted name, WhatsApp, email, wedding date, start time, location, optional notes and package ID through T05 validation/actor boundary; it submits no promotion ID or price. Booking Saya uses the persisted session actor and trusted booking owner lookup; browser prices/owner/status are never authority.

## Endpoint

The prepared submission commits one Menunggu Konfirmasi record and initial history before wa.me preparation; own Booking Saya list/detail reads that record. T07 owns production activation after date protection is integrated. Typed validation/conflict/absence errors show without losing durable state.

## Handoff

H-M3-05: booking ID, owner ID, selected wedding date/location, immutable snapshot, status and history; M3-T07 checks availability and M4 attaches proof/exceptions by booking ID. Optional wa.me URL uses only approved E.164 config and encoded message; with D-04 pending, live link stays disabled while record remains durable. GO records the exact schema/type, required fields, identity/correlation, retry/idempotency and consumer integration evidence.

## Expected use path

`EUP-M3-T05`: Disposable protected customer route/command → T05 actor/input and stable submission_key → T03 package selection + T02 promotion state + T04 booking transaction → committed booking ID and own-record read. GO proves the prepared route/repository handoff and CK checks production activation remains gated. T07 binds the production route to the shared date transaction, retires localStorage/scenario submission and proves its actual route-to-effect path.

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

- `AC-M3-T05-A` (Project booking flow; M3-W05): Valid submission uses its trusted created_at timestamp and Asia/Jakarta date to automatically find the active, published promotion linked to the package and within inclusive starts_on/ends_on; it computes final_price and persists the applied snapshot or null before WhatsApp preparation. Invalid package/date/contact/start-time/field combinations fail without partial record. Review row `RC-M3-T05A`.
- `AC-M3-T05-B` (Project privacy/shared record; M2 H-M2-05): Booking Saya shows only the owner’s durable records/status/snapshot; cross-customer and anonymous reads are denied. Review row `RC-M3-T05B`.
- `AC-M3-T05-C` (M1 UI reference; M3-W05): Customer booking/Booking Saya UI preserves reference hierarchy and exposes loading/error/success/empty states accessibly. Review row `RC-M3-T05C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T05A` | Project booking flow; M3-W05 | PASS iff Valid submission uses its trusted created_at timestamp and Asia/Jakarta date to automatically find the active, published promotion linked to the package and within inclusive starts_on/ends_on; it computes final_price and persists the applied snapshot or null before WhatsApp preparation. Invalid package/date/contact/start-time/field combinations fail without partial record. | End-to-end route + DB read before/on/after both promotion dates, wrong package, draft/inactive offer, no promotion and applicable promotion; verify no client promotion/price input, unchanged base price, no-number case and retry. GO records route/DB observations; CK independently checks them. | wa.me or localStorage as record. | T03 eligibility and T04 atomic create. |
| `RC-M3-T05B` | Project privacy/shared record; M2 H-M2-05 | PASS iff Booking Saya shows only the owner’s durable records/status/snapshot; cross-customer and anonymous reads are denied. | Two-customer and anonymous route tests, list/detail payload inspection, reload and empty state. GO records route/DB observations; CK independently checks them. | Client-side filtering or ID obscurity. | M2 session/role denials. |
| `RC-M3-T05C` | M1 UI reference; M3-W05 | PASS iff Customer booking/Booking Saya UI preserves reference hierarchy and exposes loading/error/success/empty states accessibly. | Current desktop/mobile captures and keyboard/focus/state checks on changed screens. GO records route/DB observations; CK independently checks them. | Fixture status masquerading as saved result. | M1 customer visual baseline. |

## Security readiness

SEC-M3-05 implement-now: owner-scoped reads, authenticated submission and server revalidation of price/promo/date; PII excluded from other customers (Project/M2 H-M2-05; ASVS access/input/data, OWASP access control, NIST AC/PT). SRB-M3-05 binds A/B to cross-account route/DB evidence. Preserve seam: no proof upload or exception workflow in M3. Scoped baseline: OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status applicable; no planning-review-required finding. D-03 production proof approvals remain M4/M5 gates; D-04 keeps the live WhatsApp link disabled until configured.

## UI readiness

Frontend-awareness State 2, Mode A: use M1 customer booking/Booking Saya reference and Soft Rose Atelier; inspect actual changed routes and accessible states. D-04 leaves link disabled without changing saved-booking feedback. For changed UI, apply frontend-awareness VIS-001–014 and M1 reference mapping; capture only touched routes/states at the code revision. Preserve CSP/browser execution boundary and avoid new unsafe HTML sinks or unreviewed origins. Backend-only work may cite unchanged M1 captures.

## Validation and hard stop

Run focused disposable PostgreSQL integration/route tests, direct negative/empty cases, affected lint/typecheck/test/build checks, use-path inspection and migration inventory. Stop for a changed business rule, unresolved schema relationship, invalid M2 handoff or missing ERD confirmation.

## Revision 1.8 amendment — retry and activation handoff

This section supersedes the earlier active route claim where it differs. T05 prepares the customer command/read and UI handoff with disposable protected-route/repository evidence; T07 is the immediate production activation owner. The customer generates one submission_key per intended request and retains it across network retries. The server validates and persists all Project Context form fields: name, WhatsApp, email, wedding date, start time, location and optional notes, plus selected package ID; it computes a canonical payload hash without trusting a client hash. The server scopes it to the authenticated customer, ignores client price/promotion/status, and uses T04's replay result. T05 does not activate a date-sensitive production submission before T07's closure/reservation transaction is present.

- AC-M3-T05-D (Project one-request integrity; M3-W05): Same-key retry returns the same booking ID and one initial history event; changed-payload key reuse conflicts without mutation. Bound to RC-M3-T05D.

| Review row | Authority | Required behavior | Harness and evidence | Forbidden substitute | Regression |
| --- | --- | --- | --- | --- | --- |
| RC-M3-T05D | Project one-request integrity; M3-W05; SEC-M3-05 | PASS iff authenticated same-key retry yields one booking and initial event, and changed-payload reuse yields CONFLICT. | Disposable protected-route sequential/concurrent retry, changed payload, replay after later date/location update and reload; inspect immutable original request snapshot, DB counts and visible result. | Client-only button disable or a second booking. | T04 unique key and atomic create. |

H-M3-05 now includes submission_key, actor-scoped replay identity, booking ID, snapshot and own-read result. EUP-M3-T05 ends at a prepared handoff to T07; T07 proves the production customer route and retires localStorage/scenario submission. RC-M3-T05A–C are exercised in disposable integration without claiming production date availability.

## GO confirmation and handoff — 26 November 2026

After the unchanged-schema rendered ERD preview and the definitions, purposes and responsibilities of migrations `0001`–`0005` were shown for this ticket, the user explicitly replied **“i confirm”**. This authorized T05's migration-dependent implementation and disposable fresh migration check; no new SQL migration or production seed was written. The actual rendered [T05 ERD](../../evidence/m3-t05/actual-erd.png), fresh [schema comparison](../../evidence/m3-t05/schema.json), and [GO checkpoint](../../checkpoints/M3-T05-customer-booking-378248c.md) bind the implementation to the confirmed unchanged schema. T07's separate SQL-07 proposal remains unconfirmed by this reply.
