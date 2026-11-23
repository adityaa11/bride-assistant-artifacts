# M2-T01 GO checkpoint — H-M2-01 topology and local profile

- Ticket: [M2-T01](../tickets/m2-authoritative-foundation/M2-T01-topology-local-profile.md), batch 1, state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact implementation commit: `123e6b49b5a0090ef8023e20b9289b2eb82cafc2`; clean code worktree at handoff.
- Evidence and versioned successor handoff: [H-M2-01 v1.0](../evidence/m2-t01/H-M2-01-topology-v1.md), with five current account screenshots in the same directory. No prior topology screenshot was reused.
- Accepted predecessor: M1.1 no-refactor exit with [M1.1-T01 CK PASS](../feedback/M1.1-T01-architecture-audit-79b3478-ck.md) at the planning baseline. No predecessor was reopened.
- Activation: preparation only. T03 owns deployed application-to-Neon proof. No customer data, production proof upload, auth implementation, SQL migration, or second byte store.

## Bounded changes

The code commit adds `compose.m2-local.yaml` for PostgreSQL 18.7, `scripts/m2-local-profile.mjs` for isolated fresh-volume/database/DocumentStore proof and teardown, and `docs/m2-local-profile.md` for operator startup. Vercel project `bride-assistant` was configured with Functions `sin1` and five Production-only Secret variable references. Neon project `tiny-mode-75452518` was provisioned in `aws-ap-southeast-1` with PostgreSQL 18.7 and bucket `bride-assistant-private`. The disposable object probe was deleted. The user accepted the observed six-hour PITR/manual-snapshot terms only for this preparation stage and chose to leave an unused empty Ohio onboarding project untouched. The handoff explicitly excludes that project.

## Review Contract Closure

| Row / ticket authority | Required behavior and proof | Evidence locator; exact validation and outcome | Status |
| --- | --- | --- | --- |
| `RC-M2-01A` / T01 topology and D-05/D-06 | Provisioned Vercel `sin1`, Neon PostgreSQL **18.7** and Object Storage Singapore; reachable Neon endpoint; account-backed recovery/PITR terms. Console/API and redacted connection/version observations, not documentation alone. | H-M2-01 `Provisioned account topology`, screenshots `vercel-functions-sin1.png`, `neon-postgres-version.png`, `neon-recovery-terms.png`. Neon SQL Editor query returned 18.7, database `bride_assistant`, zero tables. `npx --yes neon@latest connection-string production --project-id tiny-mode-75452518 --database-name bride_assistant --pooled --ssl verify-full` plus isolated `pg` query: PASS 18.7, TLS client stream, zero tables. Vercel setting saved and reloaded as `sin1` only; no deployment exists, as the ticket's preparation-only activation requires T03 to prove deployed use. Account UI: six-hour PITR, manual snapshots, scheduled snapshots require upgrade; user accepted preparation-only terms on 23 Nov 2026. | `PROVEN` |
| `RC-M2-01B` / T01 private bytes and `SEC-M2-01/07` | Private bucket; unauthenticated object request denied; short-lived signed access works; server credentials absent from client bundle/committed files. | H-M2-01 `Executed account and negative probes`, screenshots `neon-private-bucket.png`, `vercel-production-secrets.png`. CLI disposable object PUT PASS; direct unsigned GET **403**; branch-scoped read/write credential authenticated GET **200**; 60-second presigned GET **200**, exact bytes; CLI delete/list returned zero objects. Vercel five vars reloaded as **Secret / Production**. `npm run build` PASS; post-commit scan of 198 tracked and 27 `.next/static` files found zero secret values. No application authorization/URL issuance is claimed; T05/M4 own it. | `PROVEN` |
| `RC-M2-01C` / T01 disposable Docker and local DocumentStore | Fresh PostgreSQL 18.7 and empty local byte store; connection/put/read/remove, teardown, no migration/seed/source-controlled bytes. | Code `compose.m2-local.yaml`, `scripts/m2-local-profile.mjs`, `docs/m2-local-profile.md`; H-M2-01 `Fresh local profile and teardown`. `node scripts/m2-local-profile.mjs verify` twice: PASS 18.7, zero public tables, existing local DocumentStore put/read/remove, unique Compose volume/root removed. `git status --short` clean after commit; no intended migration or seed. | `PROVEN` |
| `RC-M2-01D` / T01 `H-M2-01` | Versioned, redacted region/connection/bucket/credential-reference/recovery facts usable by T02/T03/T07; repeat probe in named environment. | H-M2-01 v1.0 gives account/project/branch/database/bucket/host/region, five Vercel Production secret names and successor instructions, with no values. SQL Editor query and CLI/`pg`/bucket commands were executed against the named Neon branch; Vercel settings reloaded; local fresh-profile command repeated. T03 deployment activation is explicitly pending. | `PROVEN` |

Internal readiness: `READY_FOR_CK`. This is GO readiness only; CK alone decides `PASS`.

## Actual Use Path

`EUP-M2-01`: signed-in operator → Vercel project Settings/Functions and Production Secret variables + Neon project/branch/database/bucket consoles → account SQL/connection/private-object probes and `scripts/m2-local-profile.mjs` Docker probe → redacted H-M2-01 record/local service → T02 runtime constraint, T03 deployment/migration integration, T07 recovery facts. The committed local path is operator `node scripts/m2-local-profile.mjs verify` → Compose `postgres:18.7` and the existing `shared/server/local-document-store.ts` implementation → version/empty/put/read/remove observations → volume and root teardown. The production-like Vercel server → Neon connection is an explicit T03 successor path, not a T01 current-use claim. The fixture route keeps its existing local byte owner; no replacement or legacy production caller was added.

## Actual Empty-State Evidence

The new Neon `bride_assistant` database returned zero public tables in SQL Editor and independent pooled-client query, with no seeded customer record. The private bucket was empty before the test and `objects: []` after the disposable object was deleted. A unique fresh Docker profile reported zero public tables and an absent local DocumentStore root before first put; first put/read/remove succeeded and teardown removed the isolated root and volume. There is no application endpoint yet for this environment; T03 owns that first deployed handoff.

## Migration Inventory

No intended migration or production-data change. Docker's unique Compose volume and ignored `.local-storage/m2-local/<probe-project>/` root are ephemeral test setup and are removed by `verify`. The Neon disposable text object was removed and bucket emptiness observed. No test seed SQL, migration-runner conditional, real data, or source-controlled byte artifact was committed.

## Validation and limitations

- `node scripts/m2-local-profile.mjs verify` — PASS twice, including teardown.
- Neon SQL Editor version/empty query, pooled `pg` connection, object PUT/unsigned GET/signed GET/delete/list — PASS with outcomes in H-M2-01; unsigned GET's expected negative was 403.
- `npm run lint` — PASS; `npm run typecheck` — PASS; `npm test` — PASS, 13/13; `npm run build` — PASS, 25 pages; `git diff --check` — PASS.
- Post-commit secret scan — PASS, zero matches in 198 tracked files and 27 generated client files. Generated build files and operator credentials remain ignored.
- Vercel currently has no deployment and no deployed bundle or app-to-Neon observation. The saved `sin1`/Secret settings need T03's deployment. The six-hour PITR/manual snapshot account terms were accepted only for preparation. No restore was performed; M5-W03 owns restore rehearsal. The unused empty Ohio onboarding project remains by user choice and is excluded from all configured references.
