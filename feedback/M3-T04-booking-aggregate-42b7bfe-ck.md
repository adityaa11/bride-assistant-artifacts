# M3-T04 CK review — booking aggregate and immutable history

- Ticket: [M3-T04](../tickets/m3-booking-core/M3-T04-booking-model.md)
- Batch: 2; milestone/work package `M3 / M3-W04`; ticket state at review: `awaiting_review`.
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `42b7bfe282399e434c47453133785e62c2be6bad`
- GO checkpoint: [M3-T04 booking aggregate](../checkpoints/M3-T04-booking-aggregate-42b7bfe.md)
- Review type: first consolidated CK review against the frozen T04 contract
- Result: `PASS`

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M3-T04`, batch 2, state
`awaiting_review`, checkpoint `M3-T04-booking-aggregate-42b7bfe.md`, and review
target `42b7bfe282399e434c47453133785e62c2be6bad`. The checkpoint commit matches
the checked-out code revision. The code worktree contains only the pre-existing
unstaged `AGENTS.md` modification; it does not alter the reviewed commit or make
the target ambiguous.

The artifact store resolved from tracked `atlas-workflow.yaml` and the ignored
local override to `C:/Workspace/bride-assistant-artifacts`. Its origin matches
`https://github.com/adityaa11/bride-assistant-artifacts.git`, and
`.atlas/atlas-artifacts.yaml` declares the `atlas-workflow` layout. Before this
review artifact was written, the artifact repository was at commit
`22f7a6ce32ff29ea0fbdcf1fc6a5c3692d61391c`; its index was clean and the only
pre-existing untracked paths were the four `.gitkeep` files under
`checkpoints/`, `evidence/`, `feedback/`, and `tickets/`. None was modified or
staged.

## Review Contract verification

| Row | Ticket authority | Review result and proof | Status |
| --- | --- | --- | --- |
| `RC-M3-T04A` | `AC-M3-T04-A`: one durable booking identity with copied package facts, promotion-or-null snapshot, final price, and unchanged snapshots after catalogue edits. | Fresh Docker contract created promoted and unpromoted bookings, checked base/final prices and copied package/promotion/item terms, edited catalogue records, and verified the stored snapshots and IDs remained unchanged. The repository path uses persisted snapshots rather than historical live joins. | `PROVEN` |
| `RC-M3-T04B` | `AC-M3-T04-B`: canonical initial status, ordered append-only history, and atomic rollback on invalid create. | The contract observed one `Menunggu Konfirmasi` event with null previous status and customer actor, injected history-write failure, verified zero booking/history rows after rollback, read later events in order, and verified booking facts/history updates and deletes fail with `23514`. | `PROVEN` |
| `RC-M3-T04C` | `AC-M3-T04-C`: empty/missing behavior, confirmed actual schema, and no test seed in the migration graph. | Fresh migrations `0001`–`0005` returned empty own-list `[]` and missing/other-owner `NOT_FOUND`. The rendered actual ERD shows before/after schema, named PKs/FKs and cardinalities; the fresh catalog observation matches two new PKs, five FKs, scoped retry uniqueness and checks. The disposable Docker volume is removed in `finally`; no seed or test-only migration branch is present. | `PROVEN` |
| `RC-M3-T04D` | `AC-M3-T04-D`: identical same-customer/key replay returns one booking/event, changed payload conflicts, cross-customer keys remain isolated, and replay survives later date/location edits. | The contract observed repeated original ID, one initial event, changed-note `CONFLICT`, independent cross-customer creation, concurrent same-key calls returning one ID, and replay after date/location update returning the original ID. The unique key and immutable snapshot/hash were inspected. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M3-T04` is preparation-only and is implemented and
exercised as:

`M2-T05-shaped customer actor/validated payload → createBooking,
listOwnBookings, or readOwnBooking → transactional parameterized repository
queries and T01 catalogue resolution → PostgreSQL booking and
booking_status_history rows → observed identity, snapshot, history, retry, and
absence results`.

The repository verifies the trusted actor subject and scopes reads to that
subject. T05 owns route composition and customer consumption; T06 owns admin
transitions. The M1 booking fixture routes remain declared review fixtures and
are not a fallback. No later route, proof, cancellation, reschedule, or
provider activation was claimed.

## Migration and empty-state assessment

The confirmed SQL-04 revision was explicitly confirmed before migration `0005`
was written or run. Migration `0005_booking_aggregate.sql` owns the two booking
tables, five foreign keys, customer-scoped retry uniqueness, snapshot/hash and
price checks, immutable booking facts, and append-only history. It owns no
production rows. Existing migrations `0001`–`0004` remain unchanged, and no
SQL-07 reservation table or test seed was added.

I inspected the rendered [actual ERD](../evidence/m3-t04/actual-erd.svg). It
shows the before and after states, names both new tables and their primary and
foreign keys, shows relationship cardinalities, and links the after state to
the fresh PostgreSQL 18.7 migration set. The fresh catalog observations and
trigger probes agree with the diagram.

The empty-state contract is proven independently: immediately after fresh
`0001`–`0005`, own-list returned `[]`, a missing UUID returned `NOT_FOUND`, and
no booking or history row existed. The first valid submission created one
aggregate and one initial event atomically; the injected failure left both
tables empty.

## Evidence binding and validation performed

The reviewed artifact paths resolve at artifact-repository commit
`22f7a6ce32ff29ea0fbdcf1fc6a5c3692d61391c` as follows:

| Artifact | Blob |
| --- | --- |
| `tickets/m3-booking-core/M3-T04-booking-model.md` | `f4d2c9906c1276a122e1f8f38d4cfe8571b59fdf` |
| `checkpoints/M3-T04-booking-aggregate-42b7bfe.md` | `4ce30dafbc49bfc45b2073aa49101b9e752b9f09` |
| `evidence/m3-t04/observations.json` | `2fef614be716758c47d9f97de01d8177b7aca8f4` |
| `evidence/m3-t04/actual-erd.svg` | `8f6572023ed3b7c0003ce0613a2a0c0357c9c828` |

Validation performed against the reviewed code commit:

- `node scripts/m3-t04-contract.mjs` — **PASS**, fresh disposable Docker PostgreSQL 18.7; migrations, schema, empty/missing cases, snapshots, rollback, retry/concurrency, immutable history, and cleanup passed.
- `npm run lint` — **PASS**.
- `npm run typecheck` — **PASS** on the serial rerun after the production build generated the Next types.
- `npm test` — **PASS**, 15/15.
- `npm run build` — **PASS**, production compilation, TypeScript, static generation, and route generation.
- Inspected the repository implementation, migration inventory, observations, actual ERD, empty-state evidence, and true use path.

The first parallel typecheck attempt raced with Next-generated `.next` files;
the required serial rerun passed. This is a validation-environment note, not a
code finding. No Vercel or Neon probe was run: T04 changes no provider path and
has no provider-only acceptance criterion.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding or
closure clause was frozen.

## Scope-change observations

None. The review stayed within the frozen aggregate, immutable snapshots,
history, retry identity, empty/initial state, migration hygiene, ERD evidence,
preparation-only use path, and named predecessor/successor boundaries.

## Decision

`PASS`. All four ticket-derived Review Contract rows are proven at
`42b7bfe282399e434c47453133785e62c2be6bad`; the required current local
validation, rendered ERD/database comparison, retry evidence, and empty-state
proof are present and independently checked, with no direct in-scope
regression, unapproved fallback, test-only migration, or authority ambiguity.
