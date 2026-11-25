# First-admin bootstrap local validation

- Code repository: https://github.com/adityaa11/bride-assistant.git
- Exact code commit: `fcaae2455308f10f2e46a0092fa097135b56994d`
- Migration: `migrations/0003_admin_bootstrap_state.sql` at that code commit.
- Scope: fresh disposable PostgreSQL 18.7 only; no Neon or production database was touched.

## User confirmation

On 25 November 2026, after the rendered proposed admin-bootstrap ERD and its definition, purpose, and responsibilities were shown, the user explicitly selected “Confirm 0003 proposal (Recommended).” The separate instruction to commit the bootstrap was also explicit.

## Fresh database and actual ERD comparison

A unique Docker Compose project started with an empty PostgreSQL 18.7 volume, then `node scripts/m2-migrate.mjs` applied versions `0001`, `0002`, and `0003`. The local check inspected `app.admin_bootstrap_state` columns, primary key, and foreign key to `auth."user"`, and the existing `auth.account → auth."user"` relationship. Those observations match the [rendered actual ERD](actual-erd.svg). It created one disposable admin through Better Auth, observed the sealed marker and credential account, signed in with that credential, and rejected a second bootstrap and reconciliation after sealing. Result: PASS. The Compose volume and network were removed.

A second fresh disposable PostgreSQL 18.7 project applied the same three versions, started the production Next build, and ran `tests/admin-bootstrap.browser.spec.ts` against `/admin/masuk`. Result: 1 passed; the admin reached `/admin`. That project's volume and network were removed.

Static checks: `npm run lint` PASS; `npm run typecheck` PASS; `npm test` 15/15 PASS; `npm run build` PASS after granting Turbopack the Windows junction permission it needs. The earlier sandboxed build failed only at junction creation with OS access denied.

## Boundary

`0003` creates schema `app` and the one-row `app.admin_bootstrap_state` marker with its guarded UNINITIALIZED → RESERVED → SEALED transition. It references `auth.user` but does not create users or store passwords. The CLI creates a credential through Better Auth, promotes only the returned user ID, and seals the marker. This evidence is local; deployment/provider behavior is not claimed.
