# CK review — M3-T05 customer booking and own read

- Result: `PASS`
- Ticket: [`M3-T05`](../tickets/m3-booking-core/M3-T05-customer-booking-flow.md)
- Batch / milestone / work package: `3 / M3 / M3-W05`
- Ticket state at review: `awaiting_review`
- Reviewed code commit: `378248c1945f3f75b5b1d67f6545d0358f57fee7`
- Frozen ticket: [`M3-T05 customer booking flow`](../tickets/m3-booking-core/M3-T05-customer-booking-flow.md)
- Reviewed checkpoint: [`M3-T05 GO checkpoint`](../checkpoints/M3-T05-customer-booking-378248c.md)
- Review type: first consolidated CK review against the frozen M3-T05 contract

## Review scope and preconditions

The active-ticket tuple is unambiguous: ticket `M3-T05`, batch 3, state
`awaiting_review`, authorized checkpoint
`M3-T05-customer-booking-378248c.md`, review target
`378248c1945f3f75b5b1d67f6545d0358f57fee7`, and no active HMN authorization.
The checkpoint commit matches `HEAD`. The code worktree contains only the
pre-existing unstaged `AGENTS.md` instruction change; it is outside the
checkpoint and was preserved. The ticket's `fcaae245...` source reference is
the planning/source baseline; the checkpoint explicitly binds this review to
the committed implementation at `378248c...`.

This review covers the prepared, local-profile-gated customer submission and
owner-scoped Booking Saya read, retry integrity, UI states/accessibility, and
the unchanged-schema migration boundary. T07 production activation,
reservation/closure, live WhatsApp, proof, cancellation, reschedule, and M4
work are not imported into scope.

## Review Contract assessment

| Row | Ticket authority and required proof | CK evidence and assessment | Status |
| --- | --- | --- | --- |
| `RC-M3-T05A` | AC-A: trusted Asia/Jakarta creation date selects only active, published, inclusive promotions; server computes final price and immutable snapshot; invalid package/date/contact/time/fields create no partial record. | Inspected the prepared route, strict server schema, service boundary, T04 transaction handoff, and route/DB evidence. Fresh Docker/browser proof covered before/start/end/after promotion dates, wrong package, draft/inactive/no offer, client price/promotion/status rejection, invalid fields, zero-row negative cases, base/final prices, and no WhatsApp URL without approved configuration. | `PROVEN` |
| `RC-M3-T05B` | AC-B: Booking Saya exposes only the owner's durable status/snapshot; other customers and anonymous readers are denied. | Traced `/booking-saya` and both customer endpoints through session authorization and owner-scoped T04 reads. Fresh two-customer/anonymous browser proof showed owner `200`, other detail `404` and list `[]`, anonymous `401`, reload persistence, and empty initial state. | `PROVEN` |
| `RC-M3-T05C` | AC-C: customer booking and Booking Saya preserve the reference hierarchy and provide accessible loading/error/success/empty states. | Inspected current desktop/mobile form, records, empty, loading, error, and success captures; checked focus/Enter interaction, live status/alert regions, and narrow-width overflow assertions. The prepared UI has no browser persistence or fixture fallback on the live paths. | `PROVEN` |
| `RC-M3-T05D` | Revision 1.8 AC-D: same owner/key replay returns one booking and one initial event; changed payload conflicts; original request snapshot survives later date/location changes. | Fresh concurrent replay and later replay returned one ID; changed note returned `409`; database counts were one booking/one event; owner detail retained the original `2027-02-14` snapshot after the row's later date/location update. | `PROVEN` |

## Actual use path

The committed prepared path is:

`/booking?packageId=<uuid>` → `CustomerPackageSelection` → `PreparedBookingForm`
with one stable per-intended-request UUID and form facts/package ID only →
local-profile-gated `POST /api/customer/bookings` →
`submitCustomerBooking` → M2-T05 authorization/Zod/session actor → T04
`createBooking` transaction → T01/T02 `resolvePackage` using trusted
`created_at` → `booking` plus one `booking_status_history` row → booking ID.

`/booking-saya` and its list/detail endpoints use the authenticated owner and
T04 owner-scoped reads. The live page no longer invokes the M1
`CustomerBookingHistory` scenario; named review-state routes remain declared
fixtures. No unit-test-only caller was used as proof of the prepared route.

## Migration and ERD review

The checkpoint records explicit user confirmation for the unchanged T05 schema
proposal. No T05 migration, production seed, or production-data change was
added. The fresh local Docker run applied `0001`–`0005`, compared the schema,
and confirmed 11 public PKs and 14 FKs. The rendered ERD names the affected
tables, PKs, FKs, and cardinalities; its before/after state is unchanged. The
checkpoint links each migration version and records that test rows are runtime
factories with Compose teardown.

## Validation and evidence performed

- `node scripts/m3-t05-browser.mjs .local-storage/m3-t05-ck` — **PASS**, fresh PostgreSQL 18.7 Docker profile, migrations `0001`–`0005`, ERD comparison, protected route/browser scenarios, and teardown.
- `npm run check` — **PASS**: ESLint, TypeScript, 15/15 repository tests, and production build.
- Inspected the committed checkpoint ledger, observations, schema catalogue, rendered ERD, changed route/service/UI sources, and committed desktop/mobile captures.
- Confirmed no Vercel/Neon probe was required: no provider path changed and no acceptance criterion requires provider-only behavior; local results make no production deployment claim.
- Confirmed no additional SQL migration, durable seed, live WhatsApp action, production date-sensitive write, legacy live fixture caller, or browser-storage fallback is present in the reviewed scope.

## Frozen Finding Closure Matrix

No blocking findings were identified. All four ticket-derived Review Contract
rows are `PROVEN`; therefore no `CK-###` findings or closure clauses are
frozen for CFC.

## Scope-change observations

None. No product, architecture, provider/runtime, deployment, policy, or
predecessor-reopening decision is required. T07's activation and reservation
closure remain explicit successor work.

## Decision

`PASS`. The committed M3-T05 checkpoint satisfies the frozen ticket contract
and its explicit review obligations. No CFC or HMN authorization is required.
