# M2-T05 CK review — server actor, ownership and input boundary

- Ticket: [M2-T05](../tickets/m2-authoritative-foundation/M2-T05-authorization-validation.md)
- Batch: 5; ticket state at review: `awaiting_review`
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `4cac47c1cc4a3b3d0998372d23f4ba4561cc2dc0`
- GO checkpoint: [M2-T05 authorization validation](../checkpoints/M2-T05-authorization-validation-4cac47c.md)
- Result: `PASS`
- Review type: first consolidated CK review against the frozen T05 contract

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M2-T05`, batch 5, state
`awaiting_review`, checkpoint `M2-T05-authorization-validation-4cac47c.md`, and
review target `4cac47c1cc4a3b3d0998372d23f4ba4561cc2dc0`. The checkpoint's
recorded commit matches the checked-out code revision. The code worktree has
the user-provided `AGENTS.md` modification only; it is outside the reviewed
commit and does not make the target ambiguous.

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
| `RC-M2-05A` | T05 acceptance A and `EUP-M2-05`: anonymous and customer-on-admin requests are denied on existing protected routes; another customer's authoritative owner ID denies without data or side effects. | The committed browser harness requests `/admin` and `/beranda` anonymously, requests `/admin` with a customer session, adds a forged `role=admin` cookie, and requests `/booking-saya` with the customer session. It also reads the owner from a disposable PostgreSQL lookup and proves anonymous, wrong-role, and wrong-owner denials with unchanged `auth.user`/`auth.session` counts and unchanged temporary-record count. Fresh local run: `npx playwright test tests/m2-t05.browser.spec.ts tests/m2-t04.browser.spec.ts` — 2/2 passed. | `PROVEN` |
| `RC-M2-05B` | T05 acceptance B: matching actor/owner and valid schema produce the typed success; malformed input and forged owner/role values fail safely. | `decideActorInput` returns the typed success for a matching actor/owner and parsed Zod data; the committed disposable contract table covers anonymous, wrong-role, wrong-owner, empty payload, and extra forged role/owner fields. Failure codes are safe and field names only. The same fresh browser run passed 2/2. | `PROVEN` |
| `RC-M2-05C` | T05 acceptance C, `H-M2-05`, and `EUP-M2-05`: server-only explicit boundary is consumed by the active protected probe without copied authority logic or a generic registry. | `shared/server/authorization.ts` declares `server-only`, obtains the T04 `sessionActor`, and delegates to `decideActorInput`; all four protected layouts call `authorizeRequest` with explicit role and Zod inputs. The inspected committed call graph has no reachable layout-local role comparison, UI-state authority, URL-derived authority, or unused-only production export. The fresh route run reached the active layouts. `npm run lint`, `npm run typecheck`, `npm test` (13/13), and `npm run build` passed. | `PROVEN` |
| `RC-M2-05D` | T05 acceptance D: an empty owned-record scope returns the explicit empty result without a default or another actor's data. | The committed owner contract uses a fresh disposable database, one other-customer record, and a missing lookup. It observes `record: "missing"`, no private value, and unchanged row count; `/booking-saya` remains reachable under the customer session without an auth-row write. Fresh local browser validation passed, and the disposable PostgreSQL volume was removed afterward. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M2-05` is proven as:

`Authenticated GET /admin, /beranda, /booking or /booking-saya → T04 sessionActor() from request headers and persisted Better Auth session → corresponding protected layout → authorizeRequest({ requiredRole, payload: undefined, schema: z.undefined() }) → decideActorInput → protected workspace or existing audience redirect`.

The future record path is explicitly bounded as:

`T06/M3/M4 command or read caller → authoritative record lookup → authorizeRequest({ requiredRole, recordOwnerId, payload, schema }) → typed success or typed denial/validation failure → caller-owned repository, DocumentStore, domain and response mapping`.

The disposable owner fixture is not a production repository path. T06/M3/M4
retain responsibility for lookup, domain rules, empty-result mapping and
effects. No superseded local role-check path or client-side authoritative path
remains in the committed protected layouts.

## Migration and empty-state assessment

No intended SQL migration or production-data change was introduced by T05.
`0001_schema_migrations.sql` continues to own only the migration ledger and
`0002_better_auth_identity.sql` continues to own the Better Auth identity,
account, session and verification tables. The test owner table is temporary,
transaction-scoped and rolled back; no seed SQL or migration-runner branch was
added.

The user-confirmed unchanged-schema proposal was recorded before implementation
and local migration execution: “Confirm unchanged schema and proceed.” I
visually inspected the rendered [actual ERD](../evidence/m2-t05/actual-erd.svg).
It names the five current tables, primary keys, both foreign keys, cascade
behavior and the two `1 → 0..*` relationships. The fresh disposable database
returned migrations `0001`, `0002` and the same five-table inventory, matching
the diagram and the migration ownership statements.

The empty-state contract is proven: a fresh migrated database starts without
auth or owner records; anonymous protected access redirects; a missing owned
record returns the explicit `missing` result without a default, private data,
or mutation; and the existing `/booking-saya` route remains a role-authorized
fixture surface until a later ticket activates a domain repository.

## Evidence binding and validation performed

The checkpoint and referenced evidence were resolved at artifact-repository
commit `9b0ab81b6582cf16b863b3c19a7204f8fd1780f5` before this review artifact
was written. The reviewed paths resolve to these blobs at that commit:

| Artifact | Blob |
| --- | --- |
| `tickets/m2-authoritative-foundation/M2-T05-authorization-validation.md` | `89f0653fe181b1fd0d280344b2fb74a1580bfab3` |
| `checkpoints/M2-T05-authorization-validation-4cac47c.md` | `04e9e3d1e0b2ebbb5a5e80ad193e172d8124d931` |
| `evidence/m2-t05/validation.md` | `f096ed4b09bee35ebb2b56e4a540757ebf951cce` |
| `evidence/m2-t05/H-M2-05-authorization-v1.md` | `e3243cb5c4d5da7c9e0cb4062db94d5ba1d70a38` |
| `evidence/m2-t05/actual-erd.svg` | `7d9b6e1264a56b49bb033dd0791e796bf950e2d6` |

Validation performed at the reviewed code revision:

- `npm run lint` — **PASS**.
- `npm run typecheck` — **PASS**.
- `npm test` — **PASS**, 13/13 Node tests.
- `npm run build` — first sandbox attempt hit the documented Windows junction permission error; rerun with the required filesystem permission — **PASS**, including compilation, TypeScript, static generation and route generation.
- Disposable Docker PostgreSQL 18.7 profile on loopback port 55434 — **PASS**; fresh migration runner returned `{"migrations":["0001","0002"]}`. The temporary server, volume and network were removed after validation.
- `npx playwright test tests/m2-t05.browser.spec.ts tests/m2-t04.browser.spec.ts` against the fresh local database — **PASS**, 2/2 browser integration tests.
- `git diff --check` for the reviewed source commit — **PASS**.
- Inspected the committed authorization boundary, all four protected layouts, T04 actor seam, browser harness, migration inventory, handoff, validation record and rendered ERD.

No Vercel or Neon probe was run. The frozen ticket requires disposable
PostgreSQL/browser-to-server proof, and the local-first contract reuses the
approved T01/T03 provider evidence unless provider behavior or the provider
path changes; neither changed here.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding
or closure clause was frozen.

## Scope-change observations

None. The review stayed within the frozen T05 outcome, four acceptance rows,
named use path, unchanged-schema ERD contract, empty-state contract and T04
dependency boundary. No domain repository, admin delegation, generic
permission registry, provider deployment claim or future storage work was
required.

## Decision

`PASS`. All applicable ticket-derived Review Contract rows are proven at
`4cac47c1cc4a3b3d0998372d23f4ba4561cc2dc0`, the required current local
validation and rendered ERD evidence are recorded and independently checked,
and no direct in-scope regression, unapproved fallback, test-only migration,
or ambiguous authority boundary remains.
