# M3-T03 CK review — published package reads

- Ticket: [M3-T03](../tickets/m3-booking-core/M3-T03-public-package-reads.md)
- Batch: 2; milestone/work package `M3 / M3-W03`; ticket state at review: `awaiting_review`.
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `25782a2446253f4991840da74d7544d5e76c3622`
- GO checkpoint: [M3-T03 published package reads](../checkpoints/M3-T03-public-package-reads-25782a2.md)
- Review type: first consolidated CK review of the superseding checkpoint. The earlier `ae60f35` checkpoint remains immutable history; this review uses the current ticket contract and the corrected true path at `25782a2`.
- Result: `PASS`

## Review preconditions

The active-ticket tuple is unambiguous: ticket `M3-T03`, batch 2, state
`awaiting_review`, checkpoint `M3-T03-public-package-reads-25782a2.md`, and
review target `25782a2446253f4991840da74d7544d5e76c3622`. The checkpoint
commit matches the checked-out code revision. The code worktree contains only
the pre-existing user-provided unstaged `AGENTS.md` modification; it does not
alter the reviewed code or make the target ambiguous.

The artifact store resolved from tracked `atlas-workflow.yaml` and the ignored
local checkout override to the Git worktree at
`C:/Workspace/bride-assistant-artifacts`. Its origin matches
`https://github.com/adityaa11/bride-assistant-artifacts.git`, and
`.atlas/atlas-artifacts.yaml` declares the `atlas-workflow` layout. Before this
review artifact was written, the artifact repository was at commit
`c079a30780d3114e84ccccee6b6b07c63bc16f2f`; the index was clean and the only
pre-existing untracked paths were `checkpoints/.gitkeep`, `evidence/.gitkeep`,
`feedback/.gitkeep`, and `tickets/.gitkeep`. None was modified or staged.

## Review Contract verification

| Row | Ticket authority | Review result and proof | Status |
| --- | --- | --- | --- |
| `RC-M3-T03A` | `AC-M3-T03-A`: only active/published packages and currently eligible package-specific promotions appear; drafts and admin fields do not leak. | Inspected the live public page/API → `loadPublicPackages` → `readPublicPackages` → PostgreSQL path and the typed public projection. The fresh Docker contract run created draft, inactive, expired, wrong-package, valid, and non-public/non-active offer cases. It observed only the active published package and valid offer in repository, API, and rendered HTML results; the API key set was exactly the display-only projection. `node scripts/m3-t03-contract.mjs .local-storage/m3-t03-ck-contract` passed with all named visibility and negative payload checks. | `PROVEN` |
| `RC-M3-T03B` | `AC-M3-T03-B`: authoritative base price/composition, separate eligible offer, package-ID-only selection, and no invalid booking when no package is published. | The contract run compared public composition with T01 `resolvePackage`, observed the unchanged base price and separate offer, verified the public link contains only `packageId`, and reloaded after unpublishing to observe empty DB/API/route state. The browser run authenticated a disposable customer, exercised selection and reload, checked the selected UUID path, and observed the customer empty state after unpublishing. `node scripts/m3-t03-browser.mjs .local-storage/m3-t03-ck-browser` passed 1/1. The booking layout preserves the customer authorization boundary, while the read projection remains side-effect free. | `PROVEN` |
| `RC-M3-T03C` | `AC-M3-T03-C`: changed discovery/selection UI preserves hierarchy, responsive behavior, and accessible states. | Inspected current desktop/mobile captures for public and customer routes, plus public empty/error and customer empty captures. The browser harness passed focus, selection, reload, accessible empty/error roles, and 390px no-horizontal-overflow assertions. The current captures show the changed live package cards and selected handoff while preserving the established public/customer shells. `npm run lint`, `npm run typecheck`, `npm test` (15/15), and `npm run build` all passed. | `PROVEN` |

## Actual use path

The ticket-defined `EUP-M3-T03` is implemented and exercised as:

`anonymous /` or `GET /api/public/packages`, and authenticated
`/booking?packageId=<uuid>` → `loadPublicPackages` → `readPublicPackages` →
T01 `resolvePackageData` → PostgreSQL `master_package`, membership, catalogue,
and promotion tables → typed `PublicPackageDetail` → public cards/API or
customer selection.

The public route no longer passes a scenario ID and therefore does not call
`getScenario`; its named `/fixtures/empty` and `/fixtures/error` routes remain
review-only fixture routes. The customer booking route validates the UUID and
rechecks the current published list on the server. Its serialized selection
fact is only `packageId`; displayed price, composition, and offer are not
submission authority. No fixture, scenario, localStorage, client-price, or
booking-write fallback is reachable from the live path.

## Migration and empty-state assessment

T03 adds no intended SQL migration and no production-data change. Migration
`0004_catalogue_packages_promotions.sql` is the confirmed T01 dependency.
The fresh Docker run applied `0001`–`0004` and introspected nine affected table
primary keys and nine foreign keys. The committed [actual ERD](../evidence/m3-t03-v2/actual-erd.png)
and [SVG](../evidence/m3-t03-v2/actual-erd.svg) name all affected tables,
PKs, FKs, and `1 : 0..N` cardinalities, and explicitly show the unchanged
before/after state. The fresh generated [schema JSON](../evidence/m3-t03-v2/schema.json)
matched the committed evidence exactly. The disposable harness removed its
database volume and network in `finally`; no test seed or migration-runner
branch is present.

The empty-state contract is proven independently. A fresh database returned
zero published packages; after the sole package was unpublished, the API
returned `{packages:[]}`, the public route rendered
“Belum ada paket yang dipublikasikan,” and the authenticated customer route
rendered “Booking belum dapat dimulai” with no selection link. Draft and
expired offers were not used as defaults.

## Evidence binding and validation performed

At artifact-repository commit
`c079a30780d3114e84ccccee6b6b07c63bc16f2f`, the reviewed paths resolve to:

| Artifact | Blob |
| --- | --- |
| `tickets/m3-booking-core/M3-T03-public-package-reads.md` | `dd22b03f1055de2cccdb60bb032fe176fc45ffda` |
| `checkpoints/M3-T03-public-package-reads-25782a2.md` | `479bc52c7f7ffe450c923006d6d7f9c3aa9e963a` |
| `evidence/m3-t03-v2/schema.json` | `231cb9c110e3b7f322222d41909d662fec9e97d7` |
| `evidence/m3-t03-v2/actual-erd.png` | `2cd9023900c278c011cb4a061dbabe168c8e0ac5` |
| `evidence/m3-t03-v2/actual-erd.svg` | `c296ac70cf7fcece8cb6f69a6f220a93f04c8c25` |

Validation performed against the reviewed code commit:

- `node scripts/m3-t03-contract.mjs .local-storage/m3-t03-ck-contract` — **PASS**, fresh disposable Docker PostgreSQL 18.7; migrations, empty state, package/offer visibility negatives, display-only payload inspection, composition, base price, reload, handoff, and cleanup passed.
- `node scripts/m3-t03-browser.mjs .local-storage/m3-t03-ck-browser` — **PASS**, 1/1 Playwright; public/customer live paths, selection, reload, focus, responsive, empty, error, and cleanup passed.
- `npm run lint` — **PASS**.
- `npm run typecheck` — **PASS** on the serial rerun after the production build completed.
- `npm test` — **PASS**, 15/15.
- `npm run build` — **PASS**, production compilation, TypeScript, page generation, and route generation.
- Fresh generated `schema.json` compared with the committed evidence — **exact match**.
- Visually inspected the rendered actual ERD and all current public/customer desktop/mobile/empty/error captures.

The first parallel typecheck/build attempt briefly raced on Next-generated
`.next` files; the required typecheck was rerun serially and passed. This is a
validation-environment note, not a code finding.

No Vercel or Neon probe was run. T03 changes no provider path and names no
provider-only acceptance criterion; this review makes no remote deployment
claim and reuses the approved M2 provider baseline.

## Frozen Finding Closure Matrix

No ticket-bound deficiencies were identified. No `CHANGES_REQUIRED` finding or
closure clause was frozen.

## Scope-change observations

None. The review stayed within published/active public reads, eligible
package-specific promotions, the T05 selection handoff, empty/initial state,
the M1 visual boundary, the T01 schema dependency and ERD evidence, local
validation, and the explicit no-provider-probe boundary. No later booking
write, proof upload/review, cancellation/reschedule, or production activation
was required.

## Decision

`PASS`. All three ticket-derived Review Contract rows are proven at
`25782a2446253f4991840da74d7544d5e76c3622`; the corrected production path is
reachable, the required local and rendered ERD evidence is current and
verifiable, and no direct in-scope regression, unapproved fallback,
test-only migration, or authority ambiguity remains.

