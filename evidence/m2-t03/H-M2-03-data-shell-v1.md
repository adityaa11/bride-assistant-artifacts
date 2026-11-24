# H-M2-03 v1.0 — deployable server and migration shell

- Ticket: [M2-T03](../../tickets/m2-authoritative-foundation/M2-T03-data-shell.md); batch `BAP-M2-SET-01 / 3`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`; exact source commit `d52067955f833fdf803c96f868cb4b3f67a36218`.
- Evidence: [validation and schema comparison](validation.md), [rendered actual ERD](actual-erd.png), [deployment metadata](deployment.json).

## Contract consumed by T04, T05 and T06

`shared/server/database.mjs` is the sole server-side PostgreSQL client and exports `databaseConfig`, `databaseClient`, `withDatabase`, and `databaseState`; TypeScript declarations are beside it. `DATABASE_MODE` (`local` or `production`), `DATABASE_URL`, and `DATABASE_EXPECTED_NAME` are required. Production also requires `DATABASE_EXPECTED_HOST` and `sslmode=verify-full` on the URL. Production config requires the named Neon host and verifies the TLS certificate. Local config permits only loopback. Invalid or missing values throw stable `DatabaseConfigError.code` values before connection; startup maps them to `DATABASE_STARTUP_FAILED` without credential text. Do not use `NEXT_PUBLIC_` for these names.

`instrumentation.ts` checks the connection and migration ledger at server startup in configured local/production runtime; `GET /api/system/database` is the deployed readiness endpoint using the same client. T04 may use the client for Better Auth persistence and may add its authorized auth migrations after `0001`. T05 can consume the server environment/connection convention, and T06 can use the same PostgreSQL boundary for repository contracts. No ORM or Better Auth adapter is selected by T03.

`scripts/m2-migrate.mjs` is the operator migration command. It takes ordered `migrations/*.sql` files, a transaction-scoped advisory lock (compatible with Neon transaction pooling), applies each version in a transaction, records a SHA-256 checksum, and rejects changed applied SQL on repeat. The versioned inventory is only `0001_schema_migrations.sql`: the ledger table, no domain/auth data. A fresh database migrates from zero public tables to this one table and zero business rows. Tests use a unique Docker database and disposable schema, with schema drop and volume teardown. No test seed belongs in `migrations/`.

## Deployed target and limits

T01's Vercel project now uses the Next.js preset and Node 24.x; its database route function is deployed to Singapore `sin1`. The current protected production deployment is `dpl_ExhMAi4Hs6pXHvJP79SHkskMHEeq`. An authenticated CLI request returned `ready`, PostgreSQL `18.7 (84da7d8)`, migration `0001`, and Node `v24.22.3` from the named Neon Singapore production branch. The project remains under Vercel Authentication and its temporary probe bypass was removed. The Vercel project is not Git-connected, so a later source change requires another explicit deployment. No raw connection string, credential, customer data or proof bytes appear in this handoff.

This shell has no Better Auth tables, persistent identities, protected customer/admin routes, domain records, production byte adapter, or fixture promotion. T04 owns the first auth migration and session behavior, T05 owns actor/ownership checks, and T06 owns repository/DocumentStore contracts.
