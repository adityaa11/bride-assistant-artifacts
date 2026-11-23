# M2-T02 GO checkpoint — exact runtime lock

- Ticket: [M2-T02](../tickets/m2-authoritative-foundation/M2-T02-runtime-lock.md), batch 2, state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact implementation commit: `ccf351ba15075bd10d523d35e8583bfa94b3c22b`; clean code worktree at handoff.
- Accepted predecessor: T01 CK `PASS` at `123e6b49b5a0090ef8023e20b9289b2eb82cafc2` and [H-M2-01](../evidence/m2-t01/H-M2-01-topology-v1.md). No predecessor was reopened.
- Successor handoff: [H-M2-02 v1.0](../evidence/m2-t02/H-M2-02-runtime-v1.md); [exact validation log](../evidence/m2-t02/validation-ccf351b.log).
- D-02 correction: npm does not publish `@types/node@24.6.6`; after advisory and Vercel/Neon research, the user approved the existing exact `24.6.2` on 24 Nov 2026. D-02 and the ticket now record that narrow correction; all other version and acceptance rows are retained.

## Bounded changes

The code commit adds the approved Better Auth, Zod and `pg` exact dependencies, fixes all direct manifest dependency ranges to exact versions, and regenerates one npm lockfile. `.node-version` selects Node 24.22.3 for local/CI runs; the engine range `>=24.22.3 <25` excludes Node 26 and remains compatible with Vercel's major-version selection. `.github/workflows/runtime-lock.yml` runs `npm ci`, the existing full `check` script, and `npm audit` under `.node-version`. No application route, provider configuration, auth feature, ORM, SQL, or byte path changed.

## Review Contract Closure

| Row / ticket authority | Required proof | Evidence and exact validation outcome | Status |
| --- | --- | --- | --- |
| `RC-M2-02A` / T02 Outcome, D-02 | Exact manifest, Node engine/selection and lock resolve approved versions; clean checkout `npm ci`, runtime output and peer graph. | `package.json`, `package-lock.json`, `.node-version`, CI workflow; H-M2-02 `Exact baseline`. At code commit, `node --version` = `v24.22.3`, `npm --version` = `11.19.1`; `npm ci --no-fund` PASS twice (first precommit identical source, second postcommit, 385 packages); `npm ls --all --json` zero problems. T01 PostgreSQL 18.7 evidence is referenced by verified artifact commit/path/blob in H-M2-02; same named provider configuration remains unchanged. | `PROVEN` |
| `RC-M2-02B` / T02 Acceptance, EUP | Lint, TypeScript, tests and production build pass from clean lock; M1 fixture routes/scenarios retained. | Postcommit `npm run check` PASS: lint, typecheck, 13/13 `workspace-boundaries` fixture tests, Next 16.4.6 build with 25 generated pages; exact stdout in validation log. Production `next start -p 3197` probe returned 200/HTML for eight public/customer/admin fixture routes (listed in H-M2-02). CI caller committed. | `PROVEN` |
| `RC-M2-02C` / T02 Acceptance, SEC-M2-02 | Integrity/provenance and version-scoped advisory review; no unapproved runtime/provider/auth feature. | Every registry lock entry has integrity (zero missing); `npm audit --audit-level=moderate` PASS, zero vulnerabilities; `npm ls --all --json` zero peer problems. H-M2-02 advisory ledger cites Next/Better Auth published advisories and Next Content-Length defect, with D-02 direct-URL boundary retained. Diff has no application imports, providers, auth route or ORM selection; Node engine excludes 26. | `PROVEN` |

Internal readiness: `READY_FOR_CK`. CK alone decides `PASS`.

## Actual Use Path

`EUP-M2-02`: clean checkout/CI → `.github/workflows/runtime-lock.yml` → `actions/setup-node` reads `.node-version` → `npm ci` reads `package.json` and `package-lock.json` → `npm run check` runs lint/type/tests/Next build → deployable `.next` artifact for T03. The local execution of the same `npm ci`/`check` commands and eight `next start` route probes is in the evidence. No cached developer install is used as proof. No old alternative lockfile/runtime selector or production caller remains. The CI workflow is committed but its hosted GitHub run is not claimed; the local clean-checkout-equivalent commands were executed at the exact code commit.

## Migration Inventory

No intended migration, production-data change, test-only seed, or ephemeral database setup in T02. PostgreSQL 18.7 is the unchanged T01 dependency observation. The AGENTS.md rendered ERD requirement is not triggered because this ticket neither creates nor depends on a SQL migration.

## Validation and limitations

- Postcommit `npm ci --no-fund` — PASS, 385 packages, 0 vulnerabilities; repeated the successful precommit clean install.
- Postcommit `npm run check` — PASS, lint/typecheck, 13/13 tests, 25 generated pages.
- Postcommit `npm audit --audit-level=moderate` — PASS, 0 vulnerabilities; `npm ls --all --json` — PASS, 0 peer problems.
- Lock scan — PASS, 0 registry packages without integrity; `git diff --check` — PASS. Production `next start` fixture route probe — 8/8 HTTP 200 with HTML.
- Vercel documents automatic patch updates within Node 24.x. Exact 24.22.3 is proven locally/CI configuration; T03 must observe the actual deployed patch. Next 16.4.6's Content-Length defect remains a documented delivery constraint already addressed in D-02's future direct-URL design. The pre-existing ESLint 9.39.1 deprecation warning is not an npm advisory and this ticket does not authorize an unrelated upgrade.
