# H-M2-02 v1.0 — locked runtime and dependency handoff

- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact code commit: `ccf351ba15075bd10d523d35e8583bfa94b3c22b`.
- Ticket: [M2-T02](../../tickets/m2-authoritative-foundation/M2-T02-runtime-lock.md); successor: M2-T03.
- Validation log: [validation-ccf351b.log](validation-ccf351b.log), generated from two fresh `npm ci` runs under Node 24.22.3 (first before commit, second at this exact commit). No existing `node_modules` was used for the acceptance run.

## Exact baseline

| Component | Locked version and locator |
| --- | --- |
| Node | `24.22.3` in `.node-version`; SHA256-verified official Windows archive used for local checks. `package.json` engine `>=24.22.3 <25` confines deployment to Node 24. |
| npm | `11.19.1` packaged with the tested Node archive. |
| Next / eslint-config-next | `16.4.6` / `16.4.6` in manifest and lock. |
| React / React DOM | `19.3.1` / `19.3.1` in manifest and lock. |
| TypeScript | `5.9.3` in manifest and lock. |
| Better Auth | `1.7.26` in manifest and lock. No provider or route is enabled by this ticket. |
| Zod / `pg` | `4.6.7` / `8.23.1` in manifest and lock. No parser or database caller is added by this ticket. |
| `@types/node` | `24.6.2` in manifest and lock; user-approved correction on 24 Nov 2026 because `24.6.6` was never published. |
| PostgreSQL | T01 verified `18.7 (84da7d8)` in the named Neon branch and fresh Docker profile. T02 makes no database change. |

Every direct manifest dependency is exact. Every registry-resolved lock package has an integrity hash (zero missing); `npm ls --all --json` reports zero peer-resolution problems. Better Auth's optional adapter packages occur in its published dependency graph, but this commit selects no ORM, adapter, or additional provider in application code.

## Executed checks and compatibility

The validation log records `node --version`, `npm --version`, `npm ci --no-fund` (385 packages, zero vulnerabilities), `npm run check` (lint, TypeScript, 13/13 fixture-boundary tests, Next production build of 25 generated pages), `npm audit --audit-level=moderate` (zero vulnerabilities), and `npm ls --all --json` (zero problems). The first `npm ci` also succeeded, so lock installation was repeatable. An additional production `next start -p 3197` probe returned HTTP 200 with HTML for `/`, `/fixtures/empty`, `/fixtures/error`, `/beranda`, `/booking/invalid`, `/daftar/validation`, `/masuk/error`, and `/admin/catalogue/empty`; the server was stopped after the probe.

Vercel [supports Node 24 for builds and Functions](https://vercel.com/docs/functions/runtimes/node-js/node-js-versions) and chooses the latest 24.x patch automatically. Therefore `.node-version` and CI prove exact `24.22.3` locally; a future Vercel deployment must verify its actual `process.version` and cannot assume the patch is fixed. [Better Auth supports Next.js 16](https://better-auth.com/docs/integrations/next) and its [PostgreSQL adapter accepts `pg.Pool`](https://better-auth.com/docs/adapters/postgresql). [Neon lists PostgreSQL 18.7](https://neon.com/blog/category/changelog); T01 proved the selected pooled TLS endpoint. T03 owns deployed application-to-Neon reachability.

## Advisory and defect ledger (24 Nov 2026)

| Subject | Review result and boundary |
| --- | --- |
| Entire locked npm graph | `npm audit --audit-level=moderate`: zero vulnerabilities. Lock integrity fields present for all registry packages. Audit is a point-in-time check. |
| Next `16.4.6` | Above the [published security patched floors](https://github.com/vercel/next.js/security). The [open Content-Length Route Handler report](https://github.com/vercel/next.js/issues/101854) still affects `16.4.6`; D-02 already chose authorized short-lived direct Neon Object Storage URLs for later private document delivery. T02 adds no document proxy. |
| Better Auth `1.7.26` | Above the fixed floors in [published Better Auth advisories](https://github.com/better-auth/better-auth/security/advisories); no Magic Link, OAuth, Expo, SSO, or auth route is activated here. Recheck configured features at T04. |
| Zod `4.6.7`, `pg` `8.23.1`, `@types/node` `24.6.2` | No matching npm-audit advisory at the locked graph. `@types/node` is declaration-only; it is not a substitute for the Node runtime security release. |
| Node `24.22.3` / PostgreSQL `18.7` | Approved patched baselines in D-02; T01 observed PostgreSQL 18.7. Runtime and database advisory review must recur at deployment/integration. |

## T03 consumption and limits

Use `npm ci` from the committed lock and `npm run check` on Node 24.22.3. The CI workflow `.github/workflows/runtime-lock.yml` repeats clean install, full checks, and npm advisory audit for pushes and pull requests. Keep the Vercel deployment on Node 24.x and inspect its actual patch. T03 consumes T01's redacted Neon endpoint and PostgreSQL 18.7 proof; it must prove its own deployed connection and migration shell. This handoff does not authorize auth wiring, an ORM, SQL, production data, or proof-byte access.

The reused T01 PostgreSQL observation is artifact commit `763dc8a527562e4ee89823d90733733d723486d5`, `evidence/m2-t01/H-M2-01-topology-v1.md`, blob `06a05e83dbb15ba266c05426c6d666eb0fb9612e`, observed at code commit `123e6b49b5a0090ef8023e20b9289b2eb82cafc2`. It covers the named Neon production branch and fresh Docker profile on 23 Nov 2026. The T02 code commit changes only package controls and CI; it does not alter the provider/database configuration or the observation subject.
