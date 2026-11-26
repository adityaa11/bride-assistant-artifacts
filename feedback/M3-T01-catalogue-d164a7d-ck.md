# M3-T01 CK review — catalogue and package persistence

- Ticket: [M3-T01](../tickets/m3-booking-core/M3-T01-services-actions-model.md)
- Batch: 1; milestone/work package `M3 / M3-W01`; ticket state at review: `awaiting_review`.
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `d164a7d4363e8cbe047921706cb0cd354195626d`
- GO checkpoint: [M3-T01 catalogue and package persistence](../checkpoints/M3-T01-catalogue-d164a7d.md)
- Result: `PASS`
- Review type: first consolidated CK review against the frozen M3-T01 contract.

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M3-T01`, batch 1, state
`awaiting_review`, checkpoint `M3-T01-catalogue-d164a7d.md`, and review target
`d164a7d4363e8cbe047921706cb0cd354195626d`. The checkpoint commit matches the
checked-out code revision. The code worktree contains only the pre-existing
user-provided `AGENTS.md` modification; the reviewed target remains the exact
committed revision and is not ambiguous.

The artifact store resolved from tracked `atlas-workflow.yaml` and the ignored
local checkout override to the Git worktree at
`C:/Workspace/bride-assistant-artifacts`. Its origin matches
`https://github.com/adityaa11/bride-assistant-artifacts.git`, and
`.atlas/atlas-artifacts.yaml` declares the `atlas-workflow` layout. The
pre-write artifact inventory contained only the pre-existing untracked
`checkpoints/.gitkeep`, `evidence/.gitkeep`, `feedback/.gitkeep`, and
`tickets/.gitkeep`; no staged or tracked modifications existed and none were
modified by this review.

## Review Contract verification

| Row | Ticket authority | Review result and proof | Status |
| --- | --- | --- | --- |
| `RC-M3-T01A` | `AC-M3-T01-A`: Service and Action share one typed catalogue, valid bundle/package relationships expand without double-counting, and invalid type/reference/price inputs are rejected. | Inspected `migrations/0004_catalogue_packages_promotions.sql`, `shared/server/catalogue-repository.mjs`, its declaration, and the committed contract harness. A fresh disposable PostgreSQL 18.7 run inspected all nine table PKs and nine FKs, returned exactly the two system-owned Service/Action rows, filtered Service and Action, rejected invalid type/FK/duplicate-membership/negative-price/zero-duration inputs, and expanded one directly linked item plus one bundle-linked item to two distinct items. Results were typed `CONFLICT`/`NOT_FOUND` as applicable. No fixture or fallback path is used by the repository. | `PROVEN` |
| `RC-M3-T01B` | `AC-M3-T01-B`: promotion title/description/type/value/date window and package applicability persist; only published/active offers are eligible; promotion writes leave `master_package.base_price` unchanged. | The fresh local run created percentage and rupiah promotions linked to a package, exercised inclusive Asia/Jakarta start/end boundaries, rejected invalid percentage/date-order inputs, excluded draft and inactive offers, and observed the unchanged `1000000.00` package base price after promotion writes. Source inspection confirmed parameterized writes, package joins, and no global promotion read path. | `PROVEN` |
| `RC-M3-T01C` | `AC-M3-T01-C`: confirmed M3-SQL-01, actual ERD, fresh/empty database, and repository behavior agree with the proposed rendered ERD. | The checkpoint records the explicit user confirmation on 26 November 2026 before `0004` was written or run. Visually inspected the rendered [actual ERD](../evidence/m3-t01/actual-erd.png): it names the before/after state, all nine affected tables, primary keys, foreign keys, and `1 : 0..N` relationship cardinalities. The fresh run applied `0001`–`0004`, observed two type rows and zero other catalogue/package/promotion rows, and matched the nine-table/nine-FK schema recorded in [schema evidence](../evidence/m3-t01/schema.json) and the proposed ERD. No test-only seed or migration runner branch was added. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M3-T01` is preparation-only. The current conformance
path is:

`node scripts/m3-t01-contract.mjs → operation-specific functions in
shared/server/catalogue-repository.mjs → parameterized PostgreSQL statements →
freshly migrated Docker tables → typed IDs/rows, unique item expansion,
promotion eligibility and empty/missing results`.

The repository accepts a successful M2-T05-shaped authorization decision and
returns the M2-T06 result shape. No production T02/T03 caller is authorized by
this ticket; successor tickets own
`server caller → T05 authorization/validation → repository → admin/public
consumer`. The M1 fixture routes remain declared review fixtures and
non-authoritative. No production fixture or localStorage fallback was added.

## Migration and empty-state assessment

`migrations/0004_catalogue_packages_promotions.sql` is the authorized
M3-SQL-01 migration. It owns the nine public catalogue, bundle, package and
promotion tables, their PK/FK/unique/check constraints, and exactly the two
system-owned production reference rows `(1, service, Service)` and
`(2, action, Action)`. Migrations `0001`–`0003` remain unchanged. Test records
are created only inside the disposable Docker database and the Compose volume
is removed in `finally`; no sample catalogue, package, promotion, booking, or
test-only migration seed is present.

The empty-state contract is proven independently: after a fresh migration,
`catalogue_types` contains exactly the two system rows and all eight remaining
M3-T01 tables are empty. Catalogue, bundle, promotion, and published-package
reads return empty collections; a missing package returns `NOT_FOUND`; the
first admin-shaped create returns a UUID; and only the explicitly published
and active package is eligible for the published-package read. The ERD and
fresh database comparison cover the required SQL review boundary.

## Evidence binding and validation performed

The checkpoint and referenced evidence were resolved at artifact-repository
commit `f1a0943b2cad7c83bc35f3c89521fcfc50ca6135` before this review artifact
was written. The reviewed paths resolve to these blobs at that commit:

| Artifact | Blob |
| --- | --- |
| `tickets/m3-booking-core/M3-T01-services-actions-model.md` | `f5d83d7ad6385b6cf2c7b0655bb838d79364f91f` |
| `checkpoints/M3-T01-catalogue-d164a7d.md` | `df629843091a1765b858408552b3806d5fa9f6e2` |
| `evidence/m3-t01/schema.json` | `3f08e4c2450bfa663046a2c70134d1d7c73997e4` |
| `evidence/m3-t01/actual-erd.png` | `b1ac242b529257a36ca2e834a5f2a7543dfe6c2e` |
| `evidence/m3-t01/actual-erd.svg` | `38a7bc95a444ed18e6a8b17054c216ff52420556` |
| `tickets/m3-booking-core/proposed-erd.svg` | `5da2c468d0d62f37aad06953517dabb2150fbfcf` |

Validation performed at the reviewed code revision:

- `node scripts/m3-t01-contract.mjs` — **PASS**, fresh disposable Docker PostgreSQL 18.7; migrations `0001`–`0004`, empty state, PK/FK introspection, typed negatives, unique expansion, promotion eligibility, Jakarta boundaries, and cleanup all passed.
- `npm run lint` — **PASS**.
- `npm run typecheck` — **PASS**.
- `npm test` — **PASS**, 15/15.
- `npm run build` — **PASS**, production compilation, TypeScript, static generation, and route generation. The initial sandbox attempt was blocked only by Windows junction permission; the unchanged command passed through the approved elevated filesystem path.
- `git diff --cached --check` — **PASS**; the code worktree remains unchanged except for the pre-existing unstaged `AGENTS.md` modification.
- Inspected the committed migration, repository and declaration, contract harness, migration inventory, empty-state observations, actual/proposed ERDs, checkpoint, and M3-T01 source authority.

No Vercel or Neon probe was run. The ticket is preparation-only, no provider
path changed, and its local-first contract reuses the approved M2 provider
baseline; this review makes no deployment or production-provider claim.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding or
closure clause was frozen.

## Scope-change observations

None. The review stayed within M3-T01's catalogue/package/promotion
persistence, M2 authorization/result boundaries, preparation-only use path,
empty-state contract, migration hygiene, actual ERD comparison, and explicit
successor handoff. No T02 command-level overlap policy, T03 public route
activation, T04 booking model, provider deployment, or UI activation was
required.

## Decision

`PASS`. All three ticket-derived Review Contract rows are proven at
`d164a7d4363e8cbe047921706cb0cd354195626d`; required current local migration,
repository, static, build, empty-state, and ERD/database validations pass, and
no direct in-scope regression, unapproved fallback, test-only migration, or
authority ambiguity remains.
