# M2-T04 CK review — persistent customer and admin authentication

- Ticket: [M2-T04](../tickets/m2-authoritative-foundation/M2-T04-persistent-auth.md)
- Batch: 4; ticket state at review: `awaiting_review`
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `af6409c41d06710d7dd14ab027c3af947ae3968f`
- GO checkpoint: [M2-T04 persistent auth](../checkpoints/M2-T04-persistent-auth-af6409c.md)
- Result: `PASS`
- Review type: first consolidated CK review against the frozen T04 contract

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M2-T04`, batch 4, state
`awaiting_review`, checkpoint `M2-T04-persistent-auth-af6409c.md`, and review
target `af6409c41d06710d7dd14ab027c3af947ae3968f`. The checkpoint's recorded
commit matches the checked-out code revision. The code worktree has the
user-provided `AGENTS.md` modification only; the reviewed target remains the
exact committed revision and is not ambiguous. No prior M2-T04 CK artifact or
HMN authorization exists.

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
| `RC-M2-04A` | T04 acceptance A and `EUP-M2-04`: customer registration/sign-in, persistence across process restart, expiry and sign-out denial. | The committed `tests/m2-t04.browser.spec.ts` browser-to-server harness uses disposable PostgreSQL and proves registration, HttpOnly session issuance, customer route access, sign-out denial, a second sign-in, and expiry denial. The checkpoint and [validation record](../evidence/m2-t04/validation.md) record the separate same-database process-restart observation: the retained customer cookie reached `/beranda` with HTTP 200 and Better Auth returned the persisted session after the process restarted. The fresh CK run executed the browser harness against a newly migrated Docker database and passed 1/1. | `PROVEN` |
| `RC-M2-04B` | T04 acceptance B and `SEC-M2-04`: customer/admin separation, forged-role resistance, and no admin on an empty database. | The fresh browser harness passed anonymous, customer, admin, wrong-role, forged `role=admin` cookie, and forged sign-up role observations. It also exercised the operator-only promotion path and expired admin session. The checkpoint's fresh-empty-database record shows zero auth rows, `/admin` redirect to `/admin/masuk`, unknown sign-in denial, and first customer registration remaining customer-only. Server role derivation is bound to `auth.user.role` in `shared/server/actor.ts`; browser role values are not consumed. | `PROVEN` |
| `RC-M2-04C` | T04 acceptance C and true path: live auth routes have no prototype/localStorage success or seed credentials; named fixture routes remain non-authoritative. | The committed route graph is `LiveAuth` → `/api/auth/[...all]` → server `getAuth()`/`pg` adapter → protected layout checks. The active routes no longer import `shared/customer-auth.tsx`; named `[state]` routes still do and retain scenario provenance. The fresh browser harness visited `/masuk/success` after sign-out, observed its fixture label, and remained denied from `/beranda`. `npm run check` passed, including the boundary test that rejects localStorage/sessionStorage auth and requires Better Auth on live routes. No test-only seed migration or public admin registration path is present. | `PROVEN` |
| `RC-M2-04D` | T04 acceptance D, the frontend contract, and pinned form guidance: reference hierarchy, labels/autofill, focus, error/loading, and narrow-width operation. | New implementation captures are committed for registration and customer/admin sign-in at desktop/mobile, error, loading, and keyboard-focus states. The fresh browser harness asserted native labels, email/password autocomplete, visible skip-link focus, disabled loading control, generic error alert, and no 380px horizontal overflow; the run passed 1/1. The checkpoint records the immutable M1-T08 reference commit/blob locators and the reason those unchanged reference captures remain applicable. `npm run check` passed lint, TypeScript, tests, and build. | `PROVEN` |
| `RC-M2-04E` | T04 acceptance E and the SQL/ERD review contract: actual before/after schema, keys/cardinalities, migration mapping, and fresh-database agreement. | I visually inspected the committed rendered PNG. It shows the T03 `public.schema_migrations` baseline before `0002`, the after-state `auth.user`, `auth.session`, `auth.account`, `auth.verification`, the unchanged ledger, primary keys, both user foreign keys, `ON DELETE CASCADE`, and `auth.user 1 → 0..*` session/account cardinalities. The checkpoint maps `0001_schema_migrations.sql` to the unchanged ledger and `0002_better_auth_identity.sql` to the `auth` schema. The committed introspection JSON records the five actual tables, columns, constraints, indexes, and zero auth rows. A fresh disposable Docker run applied `0001` and `0002` and returned the ordered inventory `0001`, `0002`; the browser run then consumed that database. The checkpoint records the user's explicit pre-migration confirmation of the revised `auth`-schema proposal. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M2-04` is proven as:

`Browser /daftar, /masuk, or /admin/masuk → shared/live-auth.tsx → Better Auth POST handler → shared/server/auth.ts → validated authDatabasePool → PostgreSQL auth.user/account/session → shared/server/actor.ts:sessionActor → customer/admin protected layout → workspace or role-appropriate redirect`.

The operator path is:

`scripts/m2-provision-admin.mjs → shared/server/database.mjs:withDatabase → existing credential identity → server-owned auth.user.role = admin`.

The named fixture path remains separate:

`/masuk/[state]` or `/daftar/[state] → shared/customer-auth.tsx → scenario adapter → visibly non-authoritative review state`.

No active production sign-in route calls the fixture auth component, no
localStorage credential branch or seed credential is reachable, and no
superseded auth success path remains in the authorized surface. T05 retains
record ownership and input-policy authority.

## Migration and empty-state assessment

`0001_schema_migrations.sql` is the accepted T03 ledger and owns only ordered
migration bookkeeping. `0002_better_auth_identity.sql` creates the `auth`
schema and Better Auth's durable identity, credential-account, session, and
verification tables; it owns the customer/admin role constraint, session and
account user foreign keys with cascading deletion, and adapter-required indexes.
It does not own domain records, fixture data, admin provisioning policy beyond
the stored role invariant, or business authorization. Better Auth through the
`pg` adapter is the operational consumer. Test identities and operator
promotion are disposable Docker/test setup, not migration data.

The empty-state contract is proven: a fresh migrated database has zero rows in
all auth tables, first customer registration succeeds, forged admin input is
ignored, no admin is implicitly created, unknown sign-in is denied, and
protected admin access redirects until an operator explicitly provisions an
admin.

## Evidence binding and validation performed

The checkpoint and referenced evidence were resolved at artifact-repository
commit `f04c06ceb8668d4ada16d83fe7ba91fa451496a4` before this review artifact
was written. The reviewed paths resolve to these blobs at that commit:

| Artifact | Blob |
| --- | --- |
| `checkpoints/M2-T04-persistent-auth-af6409c.md` | `0e63b35156232f10d72a9377ce53e812aab8ae1b` |
| `evidence/m2-t04/actual-erd.png` | `53ddbfe98f4eabe077f9fb544ea64aadf61e70b8` |
| `evidence/m2-t04/schema-after.json` | `12b75931ae692dc1271d663a3f366b2eaef64f3c` |
| `evidence/m2-t04/validation.md` | `55def0c72131b85662756b73635a177f55930a4c` |
| `evidence/m2-t04/H-M2-04-session-actor-v1.md` | `bdce6889475affbd13f131c8c6359d4ed7d345ea` |

Validation performed at the reviewed code revision:

- `npm run check` — **PASS**: lint, TypeScript, 13/13 Node tests, and production build.
- Fresh disposable Docker PostgreSQL 18.7 profile — **PASS**: migration runner returned `{"migrations":["0001","0002"]}`; the isolated volume and network were removed after validation.
- `npx playwright test tests/m2-t04.browser.spec.ts` with the fresh local database — **PASS**, 1/1 browser integration test in 19.0 seconds.
- `git diff --check af6409c41d06710d7dd14ab027c3af947ae3968f` — **PASS**.
- Inspected the committed auth implementation, protected layouts, route handler, operator provisioning script, migration SQL, migration runner, browser harness, fixture boundaries, handoff, validation record, schema introspection, and rendered ERD.

No Vercel or Neon probe was run. The frozen acceptance criteria require
disposable PostgreSQL/browser-to-server proof, and the local-first contract
explicitly reuses the approved T01/T03 provider evidence unless provider
behavior or the provider path changes.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding or
closure clause was frozen.

## Scope-change observations

None. The review stayed within the frozen T04 outcome, its five acceptance
rows, named use path, ERD requirement, empty-state contract, and T03 dependency
boundary. No deployment claim, domain ownership, OAuth/Magic Link/SSO work,
T05 authorization work, or future storage/provider work was required.

## Decision

`PASS`. All applicable ticket-derived Review Contract rows are proven at
`af6409c41d06710d7dd14ab027c3af947ae3968f`, the required current local
validation and rendered ERD evidence are recorded and independently checked,
and no direct in-scope regression, unapproved fallback, test-only migration,
or ambiguous authority boundary remains.
