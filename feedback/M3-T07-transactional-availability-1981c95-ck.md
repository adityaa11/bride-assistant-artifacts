# CK review — M3-T07 transactional availability and admin closure

- Result: `PASS`
- Ticket: [M3-T07](../tickets/m3-booking-core/M3-T07-transactional-availability.md)
- Batch / milestone / work package: `5 / M3 / M3-W07`
- Ticket state at review: `awaiting_review`
- Reviewed code commit: `1981c95d1d8bd487e78b830c5bab6d282a6e16d9`
- Frozen ticket: [M3-T07 transactional availability](../tickets/m3-booking-core/M3-T07-transactional-availability.md)
- Reviewed checkpoint: [M3-T07 GO checkpoint](../checkpoints/M3-T07-transactional-availability-1981c95.md)
- Review type: first consolidated CK review against the frozen M3-T07 contract

## Review scope and preconditions

The active-ticket tuple is unambiguous: ticket `M3-T07`, batch 5, state
`awaiting_review`, authorized checkpoint
`M3-T07-transactional-availability-1981c95.md`, review target
`1981c95d1d8bd487e78b830c5bab6d282a6e16d9`, and no active HMN authorization.
The checkpoint commit matches `HEAD`. The code worktree contains only the
pre-existing unstaged `AGENTS.md` instruction change; it is outside the
checkpoint and was preserved. The ticket's `fcaae245...` source reference is
the planning/source baseline; the checkpoint binds this review to the
committed implementation at `1981c95...`.

The configured artifact checkout is `C:/Workspace/bride-assistant-artifacts`,
a Git worktree whose origin matches tracked `atlas-workflow.yaml`; its
manifest declares `atlas-workflow`. Before writing, it had no staged or
tracked modifications and only the four pre-existing untracked `.gitkeep`
paths, which this review leaves untouched.

## Review Contract verification

| Row | Status | CK evidence and assessment |
| --- | --- | --- |
| `RC-M3-T07A` | `PROVEN` | Traced protected admin close/open through `authorizeRequest`, `changeClosedDate`, the shared date lock and `admin_closed_date`; customer close is forbidden. The fresh Docker/browser run proved anonymous `401`, customer `403`, first close `200`, duplicate close `409`, open `200`, absent open idempotency, admin identity persistence, and immediate cross-surface date projection. |
| `RC-M3-T07B` | `PROVEN` | Traced initial submission, admin success transition and closure through the same transaction-level date lock and durable tables. Fresh concurrent PostgreSQL/browser evidence proved two-success `200/409` with one reservation, closure-versus-success `200/409` with one winner, pending initial requests without reservation, pending cancellation/reschedule retention, rollback/retry behavior, stale retry conflict, and no partial status/history mutation. |
| `RC-M3-T07C` | `PROVEN` | Customer availability returns only `{date,state}` and no identity/details. The current customer and admin desktop/mobile, empty, error and decision captures were inspected; the browser run proved unavailable-day disabling, keyboard focus/Enter selection, status/alert feedback, live close/open refresh, and 390px no-horizontal-overflow behavior. |
| `RC-M3-T07D` | `PROVEN` | The confirmed SQL-07 migration is limited to `booking_date_reservation` and `admin_closed_date`, inserts no data, and uses disposable runtime rows. Fresh Docker migrations `0001`–`0006` matched the rendered actual ERD and schema catalogue: 13 public PKs, 16 FKs, reservation `wedding_date` UNIQUE, named PK/FK/cardinality relationships, and volume teardown with no durable test rows. |
| `RC-M3-T07E` | `PROVEN` | Traced `/booking`, `/booking-saya`, `/admin`, `/admin/bookings` and `/admin/availability` to the T05/T06 live services and the shared date decision. The workspace-boundary tests and browser admin landing assertion prove fixture/localStorage availability decisions are not production callers; named M1 review routes remain non-authoritative fixtures. |

## Actual use path

The committed production path is:

`/booking` → authenticated T05 form → `POST /api/customer/bookings` →
M2-T05 actor/input authorization → `createBooking` → shared `lockDate` and
`dateIsAvailable` → pending `public.booking` plus initial history in one
transaction. The customer calendar reads `GET /api/customer/availability` and
receives only the date-only server projection. Admin booking decisions call
`transitionAdminBooking`, which locks the booking row and invokes
`reserveSuccessfulDate` under the same date lock before committing reservation,
status and history. Admin closure calls `changeClosedDate` under that same
date lock. The fresh browser test exercises these route-to-database paths.

## Migration and ERD review

The ticket records explicit user confirmation of the revised rendered proposal
and SQL-07 definition before migration-dependent implementation. I visually
inspected `evidence/m3-t07/actual-erd.png` and its SVG; the before/after diagram
names the affected tables, primary keys, foreign keys, reservation uniqueness,
and the `booking 1 : 0..1 reservation` and `auth.user 1 : 0..N closures`
cardinalities. The fresh Docker run applied `0001`–`0006` and compared the
resulting schema to the actual ERD. Migration `0006` adds only the two
ticket-authorized tables and no seed/test rows; disposable harness factories
and Compose teardown keep test data out of the migration graph.

## Validation and evidence performed

- `npm run check` — **PASS**: ESLint, TypeScript, 15/15 Node tests, and production build.
- `node scripts/m3-t07-browser.mjs .local-storage/m3-t07-ck` — **PASS**: fresh disposable PostgreSQL 18.7 Docker profile, migrations and ERD comparison, protected route/DB/browser scenarios, concurrency, rollback/retry, date-only payload, desktop/mobile states, and volume teardown.
- Inspected the committed checkpoint, ticket, observations, schema catalogue, rendered ERD, migration, route/service/repository/UI sources, browser harness and workspace-boundary tests.
- Confirmed no Vercel/Neon probe was required: the ticket changes no provider path and has no provider-specific acceptance criterion; local results make no production deployment claim.

## Frozen Finding Closure Matrix

No blocking findings were identified. All five ticket-derived Review Contract
rows are `PROVEN`; therefore no `CK-###` findings or closure clauses are
frozen for CFC.

## Scope-change observations

None. No product, architecture, provider/runtime, deployment, policy or
predecessor-reopening decision is required. M4 reservation release/move for
approved cancellation/reschedule remains the explicit successor boundary.

## Evidence binding

The artifact-store commit before this review was
`0e357ff15d9ee64618ddd68c3f69b6dd94e506fe`. At that commit, the ticket blob is
`71aba992baca4a042e4752bb341e1c913e8e2725`, checkpoint blob is
`17699c62cc9e3f0d11cae10c614e360ed93d82e1`, actual ERD PNG blob is
`5665d657b1de892f8bf0fccddb3077c86a64a02a`, actual ERD SVG blob is
`44fb229e50dad9b8f6717b6fdaac11bcb98c2158`, observations blob is
`edeb260f5da93cc7202192ff7e74af4bf8511495`, and schema catalogue blob is
`b881d5fc0fa84499f68385c8c51868f355c823fd`.

## Decision

`PASS`. The committed M3-T07 checkpoint satisfies the frozen ticket contract
and its explicit review obligations. No CFC or HMN authorization is required.
