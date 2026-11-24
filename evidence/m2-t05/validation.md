# M2-T05 local validation and schema comparison

- Code repository: `https://github.com/adityaa11/bride-assistant.git`; exact commit `4cac47c1cc4a3b3d0998372d23f4ba4561cc2dc0`.
- User confirmed the unchanged-schema proposal on 24 November 2026: “Confirm unchanged schema and proceed.” The rendered preview and migration responsibilities were shown before implementation or local migration execution.
- Disposable Docker project `bride-m2-t05-local`, PostgreSQL 18.7, loopback port 55434. No provider probe or deployment was run; T01/T03 evidence remains the approved provider baseline.

## Executed checks

| Command / harness | Result |
| --- | --- |
| `node scripts/m2-migrate.mjs` with local database environment | PASS; fresh database returned migrations `["0001","0002"]` and PostgreSQL 18.7. No new migration file. |
| `npx playwright test tests/m2-t05.browser.spec.ts tests/m2-t04.browser.spec.ts` against the local Next server and Docker database | PASS; 2/2 tests. T05 ran actual `/admin`, `/beranda`, `/booking-saya` requests and a disposable owner-record decision table; T04 regression covered registration, customer/admin separation, sign-out, expiry and fixture boundary. |
| `npm run lint`, `npm run typecheck`, `npm test` via `npm run check` | PASS; 13/13 Node tests. |
| `npm run build` with Windows junction creation permitted | PASS; production compilation, TypeScript and route generation. The initial sandboxed build could not create a `.next/node_modules/pg-*` junction (OS error 5); rerun with filesystem permission passed without source changes. |
| `git diff --cached --check` before source commit | PASS. |

The T05 browser test observed anonymous `/admin` → `307 /admin/masuk`, anonymous `/beranda` → `307 /masuk`, customer `/admin` → `307 /admin/masuk`, a forged `role=admin` cookie still → 307, and customer `/booking-saya` → 200. Counts in `auth.user` and `auth.session` were unchanged across those requests. The owner contract fixture created one temporary record owned by another customer. It proved valid matching actor/schema success, anonymous/wrong-role/wrong-owner denial, malformed payload and forged owner/role field failure, and a missing lookup returning `record: "missing"` without private record content. Row count remained one; the transaction rolled back and dropped the temporary table.

## Actual schema and migration mapping

[Rendered current ERD](actual-erd.svg) was drawn from this fresh database's table and `pg_constraint` inventory. `information_schema.tables` returned exactly `auth.account`, `auth.session`, `auth.user`, `auth.verification`, and `public.schema_migrations`. Primary keys were `id` on each `auth` table and `version` on the ledger. `auth.session.userId` and `auth.account.userId` referenced `auth.user.id` with `ON DELETE CASCADE`, each representing user 1 → child 0..*. `auth.verification` and the ledger had no FK. The role check allowed only `customer` and `admin`.

`0001_schema_migrations.sql` owns only the unchanged `public.schema_migrations` ledger. `0002_better_auth_identity.sql` owns the unchanged identity, account, session and verification tables in `auth`. T05 owns no SQL migration or durable domain data. The test's temporary owner table was created inside a transaction and rolled back; it was never added to a migration. The diagram therefore shows the current after-0002 state, unchanged by T05.
