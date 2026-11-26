# CK review — M3-T06 admin queue and core status decisions

- Result: `CHANGES_REQUIRED`
- Ticket: [M3-T06](../tickets/m3-booking-core/M3-T06-admin-booking-decisions.md)
- Batch / milestone / work package: `4 / M3 / M3-W06`
- Ticket state at review: `awaiting_review`
- Reviewed code commit: `e0385d0e686871fcfe5bafe876ad1803103f7bfa`
- Frozen ticket: [M3-T06 admin queue and core status decisions](../tickets/m3-booking-core/M3-T06-admin-booking-decisions.md)
- Reviewed checkpoint: [M3-T06 GO checkpoint](../checkpoints/M3-T06-admin-booking-e0385d0.md)
- Review type: first consolidated CK review against the frozen M3-T06 contract

## Review scope and preconditions

The active-ticket tuple is unambiguous: ticket `M3-T06`, batch 4, state
`awaiting_review`, authorized checkpoint
`M3-T06-admin-booking-e0385d0.md`, review target
`e0385d0e686871fcfe5bafe876ad1803103f7bfa`, and no active HMN authorization.
The checkpoint commit matches `HEAD`. The code worktree contains only the
pre-existing unstaged `AGENTS.md` instruction change; it is outside the
checkpoint and was preserved. The ticket's `fcaae245...` source reference is
the planning/source baseline; the checkpoint explicitly binds this review to
the committed implementation at `e0385d0...`.

The configured artifact checkout is `C:/Workspace/bride-assistant-artifacts`,
a Git worktree whose origin matches tracked `atlas-workflow.yaml`; its
manifest declares `atlas-workflow`. Before writing, it had no staged or
tracked modifications and only the four pre-existing untracked `.gitkeep`
paths, which this review leaves untouched.

This review covers the local-profile-gated prepared admin queue/detail,
same-record reads, core status decisions and T07 handoff seam, UI states and
accessibility, and the unchanged-schema migration boundary. T07 production
activation/reservation, live provider behavior, M4 proof bytes/review and
exception transitions are not imported into scope.

## Review Contract verification

| Row | Status | CK evidence and assessment |
| --- | --- | --- |
| `RC-M3-T06A` | `PROVEN` | Traced the protected admin route through `authorizeRequest`, the prepared service and `transitionAdminBooking` to the locked `public.booking` row and atomic history insert. The fresh Docker/browser run proved customer `403`, malformed `422`, invalid/stale `409`, concurrent `200/409`, unchanged event counts for rejected decisions, and ordered actor/time history. Success remains gated on the T07 callback. |
| `RC-M3-T06B` | `PROVEN` | Traced admin list/detail reads to the same T05-created `public.booking` rows and compared the booking ID, submission snapshot, package snapshot and history with the customer read. The run proved empty `[]`, missing UUID `404`, anonymous `401`, customer `403`, and omission of `submission_key` and `submission_payload_hash`; no fixture or durable seed path is used. |
| `RC-M3-T06C` | `PROVEN` | Independently inspected the committed admin queue/detail path and current desktop/mobile, empty, loading, error and payment-pending captures. The browser run proved keyboard focus/Enter selection, status feedback, disabled success action and 390px no-horizontal-overflow behavior. The rendered states retain the M1 admin hierarchy and frame. |
| `RC-M3-T06D` | `PROVEN` | The prepared route rejects missing/false/true success attempts without the T07 reservation callback, and the repository callback seam rolls back a failed reservation and records one actor/time event for a successful disposable callback. The browser/DB observations show status and event counts remain unchanged on rejected success attempts and advance only through the transactional handoff seam. |
| `RC-M3-T06V` | `IMPLEMENTED_UNPROVEN` | Ticket `Validation and hard stop` requires affected lint/typecheck/test/build checks. Lint and the 15/15 Node tests pass, but `npm run check` exits 1 at `tsc --noEmit` with `tests/m3-t06.browser.spec.ts(127,25): error TS7006: Parameter 'event' implicitly has an 'any' type`; `npm run build` exits 1 on the same TypeScript error. |

## Actual use path

The committed prepared path is:

`/admin/bookings` → `AdminBookingsPage` → `listPreparedAdminBookings` →
M2-T05 `authorizeRequest` with the server session and strict input →
`listAdminBookings` → the same `public.booking` rows created by T05 →
`AdminBookingLive` queue/detail. Selection calls
`GET /api/admin/bookings/[bookingId]` → `readPreparedAdminBooking` →
`readAdminBooking` → the booking and ordered `booking_status_history`.
Decision calls `PATCH /api/admin/bookings/[bookingId]` →
`decidePreparedAdminBooking` → `transitionAdminBooking`, which locks the
current row, validates the advisory expected status and allowed transition,
and appends one actor/time event in the same transaction. The browser run
executes this UI-to-DB path. The local gate is
`DATABASE_MODE=local` plus `M3_T06_PREPARED_ROUTES=1`; production success and
queue activation remain T07 work. Named M1 status routes remain
non-authoritative fixtures and are not callers of the prepared path.

## Migration and ERD review

The checkpoint records the user's explicit confirmation for the unchanged
T06 schema proposal. No T06 migration, production-data change, seed SQL or
migration-runner branch was added. I inspected the rendered actual ERD:
`evidence/m3-t06/actual-erd.png` and `.svg` name the 11 affected public
tables, primary keys, foreign keys and cardinalities, identify the external
`auth.user(id PK)` target, and show the unchanged before/after state. The
fresh Docker run applied migrations `0001`–`0005`, generated the schema
catalogue, and passed the ERD comparison with 11 PKs and 14 FKs. Test data
was created by runtime factories and the Compose volume was removed during
teardown; no test-only SQL entered the migration graph.

## Evidence binding and validation performed

The artifact-store commit used for this review is
`0eb9169fb6bdc0a1f303bf2c3dc145ba0db73353`. At that commit, the ticket blob
is `be148859aa4e107dcea04f9dcabfee79b88f2bfc`, checkpoint blob is
`5bbe844253ea58e2b882bb2486696fa9b1712cd4`, actual ERD PNG blob is
`39273f87f7b12cc08677e4a990365134aa1a7212`, actual ERD SVG blob is
`cbd5897407f4bb509ac5fdfb2ed8d625d8cdb96b`, observations blob is
`21b2c04521947249a75ba6b9ced6d2b983fc9f17`, and schema catalogue blob is
`7146260d8680233c397fcc569d693e96cf7731ee6`.

Checks performed at the reviewed code commit:

- `node scripts/m3-t06-browser.mjs .local-storage/m3-t06-ck` — **PASS**, fresh disposable PostgreSQL 18.7 Docker profile, migrations `0001`–`0005`, actual ERD comparison, protected route/DB/browser scenarios, captures and volume teardown.
- `npm run check` — **FAIL** at TypeScript after lint; `tests/m3-t06.browser.spec.ts:127` has an implicit-`any` callback parameter.
- `npm run lint` — **PASS**.
- `npm test` — **PASS**, 15/15 tests.
- `npm run build` — **FAIL** during Next.js TypeScript checking on the same `tests/m3-t06.browser.spec.ts:127` error.
- Inspected the committed checkpoint, ticket, observations, schema catalogue, rendered ERD, changed route/service/repository/UI sources, browser harness and current desktop/mobile captures.
- Confirmed no Vercel/Neon probe was required: this ticket changes no provider path and has no provider-specific acceptance criterion. Local results make no production deployment claim.

## Findings

### CK-001 — Required typecheck/build validation is not green

`RC-M3-T06V` traces to the ticket's explicit `Validation and hard stop`
requirement to run the affected lint/typecheck/test/build checks. The
checkpoint claims `npm run check` passed, but the current reviewed commit
fails the required validation: `npm run typecheck` (as the failing step of
`npm run check`) reports `TS7006` at
`tests/m3-t06.browser.spec.ts:127`, and `npm run build` repeats the same
failure. The behavioral browser/DB gate, lint and Node tests passing does not
substitute for the failed TypeScript/build checks.

## Frozen Finding Closure Matrix

| Clause | Ticket authority and unsatisfied evidence | Binary closure oracle | Direct-regression boundary |
| --- | --- | --- | --- |
| `CK-001.a` | Ticket `Validation and hard stop` / `RC-M3-T06V`: affected lint, typecheck, test and build checks must pass. Current evidence is `npm run check` exit 1 and `npm run build` exit 1 at `tests/m3-t06.browser.spec.ts:127` with `TS7006` for the untyped `event` parameter. | At a current revision, `npm run check` exits 0, including TypeScript, 15/15 Node tests and production build; the CFC checkpoint records the command, output and commit. This exact error is absent. | Preserve all proven `RC-M3-T06A`–`D` behavior, the protected route and T07 reservation callback boundary, the unchanged schema/ERD, empty-state behavior and the existing UI responsive/focus/error states. |

## Scope-change observations

None. No product, architecture, provider/runtime, deployment, policy or
predecessor-reopening decision is required. T07 activation and reservation
closure remain explicit successor work.

## Decision

`CHANGES_REQUIRED` for the single frozen clause `CK-001.a`. The prepared
admin behavior and migration boundary are otherwise proven, but the required
typecheck/build validation is not green at the submitted commit. This is one
bounded implementation-repair finding; no scope-change observation or
provider/deployment decision is needed. Return the exact clause to the bounded
remediation workflow; this review does not authorize or invoke CFC.
