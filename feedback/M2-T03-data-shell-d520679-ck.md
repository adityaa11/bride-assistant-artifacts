# M2-T03 CK review — deployable data shell

- Ticket: [M2-T03](../tickets/m2-authoritative-foundation/M2-T03-data-shell.md)
- Batch: 3; ticket state at review: `awaiting_review`
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `d52067955f833fdf803c96f868cb4b3f67a36218`
- GO checkpoint: [M2-T03 deployable data shell](../checkpoints/M2-T03-data-shell-d520679.md)
- Result: `PASS`
- Review type: first consolidated CK review against the frozen T03 contract

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M2-T03`, batch 3, state
`awaiting_review`, checkpoint `M2-T03-data-shell-d520679.md`, and review target
`d52067955f833fdf803c96f868cb4b3f67a36218`. The checkpoint's recorded commit
matches the checked-out code revision. The code worktree was clean before and
after review, and no prior M2-T03 CK artifact or HMN authorization exists.

The artifact store resolved from the tracked `atlas-workflow.yaml` and ignored
local checkout override to the Git worktree at
`C:/Workspace/bride-assistant-artifacts`. Its origin matches
`https://github.com/adityaa11/bride-assistant-artifacts.git`, and
`.atlas/atlas-artifacts.yaml` declares the `atlas-workflow` layout. The
pre-write artifact inventory contained only the pre-existing untracked
`checkpoints/.gitkeep`, `evidence/.gitkeep`, `feedback/.gitkeep`, and
`tickets/.gitkeep`; none were staged or modified by this review.

## Review Contract verification

| Row | Ticket authority | Review result and proof | Status |
| --- | --- | --- | --- |
| `RC-M2-03A` | T03 acceptance A and `EUP-M2-03`: fresh local and production-like targets connect and migrate, repeat safely, and fail safely for missing/invalid secrets. | The committed Docker evidence starts PostgreSQL 18.7 from a clean volume with zero public tables, applies and repeats migration `0001`, probes the built Next server at HTTP 200 `ready`, and records the invalid-URL startup failure as redacted `DATABASE_STARTUP_FAILED`. The missing-secret command exits nonzero with `DATABASE_URL_MISSING`; the named Neon migration is repeated successfully and records version `0001`; the deployment record and authenticated route probe bind the live shell to the reviewed code commit. The isolated schema and Compose volume were removed. | `PROVEN` |
| `RC-M2-03B` | T03 acceptance B and `SEC-M2-03`: production TLS is verified, configuration is server-only, and credentials do not enter Git or client artifacts. | `shared/server/database.mjs` requires the named production target and `sslmode=verify-full`, sets `rejectUnauthorized: true`, and reads only server environment names. The Neon evidence observes an encrypted, authorized stream and rejects an invalid CA with `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`. The checkpoint records zero T01 secret matches in tracked/client artifacts and excludes local secrets and the artifact junction from deployment input. | `PROVEN` |
| `RC-M2-03C` | T03 acceptance C: only the authorized minimal migration graph exists, test state is disposable, and first use has zero business rows. | `migrations/0001_schema_migrations.sql` creates only the authorized `public.schema_migrations` ledger. Fresh Docker and Neon introspection each show exactly that table, primary key `version`, no foreign keys, one ledger row, and zero business rows. The Docker harness creates and drops its disposable test schema and tears down its isolated volume; no fixture seed, conditional migration, auth/domain table, or production data import is present. | `PROVEN` |
| `RC-M2-03D` | T03 acceptance D and `EUP-M2-03`: the deployed shell invokes the documented one-connection migration path and publishes the T04 handoff. | The committed path is `instrumentation.ts:register` → `shared/server/database.mjs:withDatabase/databaseState` → the named Neon PostgreSQL target; the readiness route and `scripts/m2-migrate.mjs` use the same connection boundary. The production deployment is `READY`, its route runs on `nodejs24.x` in `sin1`, and the authenticated route probe returned `ready`, database `bride_assistant`, version `0001`, and Node `v24.22.3`. H-M2-03 versions the environment names, error codes, connection/migration contract, and T04/T05/T06 consumer boundary. | `PROVEN` |
| `RC-M2-03E` | T03 acceptance E, the ERD review contract, and repository `AGENTS.md`: display the actual fresh migrated schema, map every migration, compare before/after state, and show keys/relationships/cardinalities. | I inspected the committed rendered PNG. It visibly shows the before state with no public tables and the after state with `public.schema_migrations`, `version` as the primary key, `checksum`, and `applied_at`; it explicitly states no foreign keys or relationships/cardinalities. The checkpoint embeds the image, maps `0001_schema_migrations.sql` to the after state, and links the rendered SVG/PNG and comparison evidence. Fresh Docker and named Neon introspection agree with the displayed schema. Because the actual schema has no relationships, there are no relationship cardinalities to label beyond the explicit none statement. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M2-03` is proven as:

`clean committed source → Next Node runtime instrumentation startup →
server-only database configuration and one pg connection path → verified TLS
Neon PostgreSQL and migration ledger `0001` → deployed
GET /api/system/database readiness result → T04 handoff`.

The separate operator path is:

`scripts/m2-migrate.mjs → shared/server/database.mjs:withDatabase → ordered
SQL inventory and transaction-scoped advisory lock → schema_migrations ledger`.

The readiness route and migration command are committed callers, not unit-test-
only seams. No superseded production PostgreSQL path, hidden database default,
test seed migration, public upload path, ORM, auth behavior, or domain-data
consumer was introduced. T04 remains the owner of the first auth migration and
persistent identity/session behavior.

## Evidence binding and validation performed

The checkpoint and evidence were resolved at artifact-repository commit
`48085630cc0e3d290f2881820b3c77cfddd379bd`. The reviewed paths resolve to these
blobs at that commit:

| Artifact | Blob |
| --- | --- |
| `checkpoints/M2-T03-data-shell-d520679.md` | `0e0fd6a72c67b3cae59d0bf90c87fc944b045a80` |
| `evidence/m2-t03/H-M2-03-data-shell-v1.md` | `e5eee3335b2db3029e5522d05daea1efd3970f84` |
| `evidence/m2-t03/actual-erd.png` | `658e8f7a2d53a2461973ac3ff5b9bde18f87ad98` |
| `evidence/m2-t03/actual-erd.svg` | `14406d7fe309c15aa5718d0cc6d33c39234ee225` |
| `evidence/m2-t03/validation.md` | `899c2950fc84733d62e5cf44d55242f7eb190454` |
| `evidence/m2-t03/docker.log` | `81af6ad3b8946809e693fa056cdecd63a3523e09` |
| `evidence/m2-t03/neon.log` | `8489e180a008d94c90b3ab9cb1021e9ced84c7f9` |
| `evidence/m2-t03/neon-migrate-repeat.log` | `e61e4ba1764cb9e4d4354b8ab8cace114934531c` |
| `evidence/m2-t03/missing-secret.log` | `c3d68310923760aaf3f838b55ea02e47e06a1361` |
| `evidence/m2-t03/deployment.json` | `76de80c2894a57dbd62580a9e4c9b1ea58a0b6a0` |

The checkpoint records the exact code commit and the evidence observation
context. The actual ERD was independently opened and visually inspected; it is
not being treated as a source-only Mermaid block or path-only citation.

Validation performed at the reviewed code revision:

- `npm run check` — **PASS**: lint, TypeScript, 13/13 tests, and production
  build.
- `git diff --check` — **PASS**.
- Reviewed the committed database client, startup hook, migration runner,
  migration SQL, readiness route, deployment exclusion, migration inventory,
  and evidence scripts.
- Independently verified the checkpoint/evidence paths and blobs at artifact
  commit `4808563`.
- Inspected the fresh-schema ERD and compared its fields, key, absent foreign
  keys, and before/after states with the Docker and Neon introspection logs.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding or
closure clause was frozen.

## Scope-change observations

None. The review stayed within the frozen T03 outcome, its five acceptance
rows, named use path, ERD requirement, and T01/T02 dependency boundaries.
The deployment is explicitly Vercel CLI-managed rather than Git-connected, as
recorded by the checkpoint; no additional deployment integration is required
by this ticket.

## Decision

`PASS`. All applicable ticket-derived Review Contract rows are proven at
`d52067955f833fdf803c96f868cb4b3f67a36218`, the required deployed-shell,
migration, TLS, secret-isolation, empty-state, disposable-test, and rendered
ERD evidence is recorded and independently checked, and no direct in-scope
regression or unapproved path remains.
