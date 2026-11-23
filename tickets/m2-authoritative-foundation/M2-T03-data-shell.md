# M2-T03 — deployable environment and migration shell

- State: `planned`; batch 3; work package `M2-W01`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `d5fbececd2dd57598ba1d5630a9f85d86a4f5990` (ERD contract amendment; implementation baseline remains `79b3478995737dd73165afaba50369dcb5242a08`).
- Authority: [M2-W01](../../contexts/implementation/milestones/M2-foundation.md), [D-02/05/06](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md). Dependencies: T01 `H-M2-01`, T02 `H-M2-02`. Successors: T04/T05/T06 via `H-M2-03`.
- Activation: `active-in-this-ticket` for deployable server/database shell. Legacy: forbidden. SQL: intended migrations only for a minimal schema/migration ledger needed by auth; ephemeral test database/rollback is separate; no domain schema or production seed. Empty state: fresh database migrates with zero business records and remains empty.

## Outcome and exact scope

Define validated environment names and server-only secrets, a deployable Next server shell, PostgreSQL connection with TLS certificate verification in production, repeatable migration runner, isolated test database and rollback/teardown. Verify connection against T01 topology and a fresh Docker database. Excludes catalogue, booking, content models, auth behavior, real data imports, and an ORM without an accepted integration decision.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: application server startup and migration command, owned by server/data composition; inputs are named database URL/secret references, environment mode, migration directory and connection policy. Missing/invalid secrets, unsafe production TLS, wrong database or failed migration yield explicit startup/command failure with no secret echo.
- Endpoint: deployed shell starts and connects; fresh migration ends at recorded version with no domain rows, repeat run is safe, test teardown removes isolated state.
- `H-M2-03`: validated server environment schema and connection/migration/test harness contract, versioned migration inventory, connection error semantics; T04 consumes for Better Auth tables/sessions, T05 for server utilities, T06 for repository contracts. No raw connection strings in artifacts.
- `EUP-M2-03`: CI/deployment → server startup/migration command → Neon connection and migration state → T04 persistent auth. Local Docker first-use path must also work without preloaded records.

## True path and invariants

One server-side PostgreSQL connection path is authoritative. Remove test/local fallbacks from production composition; fixture routes may remain explicitly review-only. Secrets stay server-side and out of Git/client bundles. No hidden default DB, test seed migration, or `public/` upload path. T01's preparation reaches application use in this ticket.

## ERD review contract

The baseline fresh database has no business tables. Before implementing a schema migration, record the proposed affected tables and relationships in a visible planning ERD; do not invent auth/domain tables beyond the accepted T03 scope. GO must render and commit an SVG or PNG of the actual fresh migrated database schema, embed it in the checkpoint, and display it in the user-facing handoff. Show the before and after schema for any structural change; for migration-runner-only work, show the current schema and state that it is unchanged. Label tables, primary and foreign keys, and relationship cardinalities where present. Map each migration version/file to the rendered state and compare it with fresh database introspection. CK must inspect the displayed image and comparison. A migration inventory, source-only Mermaid block, or path-only citation does not close this obligation.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-03A` | Fresh local database and configured production-like target start, connect and migrate; repeat migration is idempotent and a missing/invalid secret fails safely. | Clean-volume Docker run plus deployed/preview server connection probe; migration version and redacted error logs. | Mock-only DB test; wrong-target or secret-exposure regression. |
| `RC-M2-03B` | Production DB connection verifies TLS certificate and reads only server environment; client artifact and committed files contain no credential. | Config assertion, negative connection test, bundle/Git secret scan. | Merely setting a TLS flag or copying `.env` into build. |
| `RC-M2-03C` | Intended SQL graph contains only authorized minimal shell/auth prerequisites, and test data uses disposable DB/schema or transaction rollback with cleanup; empty first-use has zero business rows. | Migration inventory, fresh DB row counts, isolated test setup/teardown and post-test comparison. | Durable fixture seed or conditional migration branch. |
| `RC-M2-03D` | The deployed shell invokes the documented connection/migration path at this code revision and publishes the exact contract T04 will consume. | End-to-end startup/config trace and handoff record. | Unit-only connection helper with no deployed caller. |
| `RC-M2-03E` | The actual post-migration schema is shown in a rendered ERD, with before/after states for structural changes or an explicit unchanged-schema statement for runner-only work; all affected keys and relationships agree with a fresh migrated database and every migration version is mapped. | Committed SVG/PNG embedded in GO/CFC checkpoint and shown in user handoff; fresh database introspection comparison; CK visual inspection. | Migration list, Mermaid source, or artifact path without a displayed, verified diagram. |

## Security readiness and validation

- `SEC-M2-03` `implement-now`: server secret isolation, verified database communication and migration/test separation; Project/Stack and D-05/06, ASVS configuration/communication/data protection, OWASP misconfiguration/injection, NIST CSF Protect/Recover and SP 800-53 SC/CM/SI. `SRB-M2-03` binds `RC-M2-03A/B/C`.
- `SEC-M2-03P` `preserve-seam`: retain future domain migrations and restore ownership outside this shell; M5 performs recovery rehearsal. No production data before T01 account proof.
- Validation is new migration and deployed-shell evidence at T03 revision; M1 fixture data is not a replacement. Hard stop: unsupported TLS/connection or unapproved ORM/schema change returns to planning.
