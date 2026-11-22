# CK review — M1-T18 fixture-local content media storage seam

- Result: `PASS`
- Ticket: `M1-T18`
- Batch / milestone / work package: `BAP-M1-FIXTURE-UI-SET-01 / 4 / M1-W07`
- Ticket state at review: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `0a69d9da1ac204cb23a2bcbccc1fba4171844b92` (`feat: complete fixture media uploads across content collections`)
- Frozen ticket: [`tickets/m1-fixture-ui-assembly/M1-T18-fixture-local-content-media.md`](../tickets/m1-fixture-ui-assembly/M1-T18-fixture-local-content-media.md)
- Reviewed checkpoint: [`checkpoints/M1-T18-fixture-local-content-media-0a69d9d.md`](../checkpoints/M1-T18-fixture-local-content-media-0a69d9d.md)
- Review type: first consolidated CK review against the effective M1-T18 contract and amendments A1–A3

## Review scope and preconditions

The explicit ticket resolved to `M1-T18`, batch 4, work package `M1-W07`, and
the current expanded checkpoint at the reviewed code commit. The checkpoint
commit matches `HEAD` exactly. The code worktree was clean at review
completion. The artifact store resolved from `atlas-workflow.yaml` and
`atlas-workflow.local.yaml` to the Git worktree at `.atlas`; its origin matches
the configured artifact remote and `atlas-artifacts.yaml` declares the
`atlas-workflow` layout. Pre-existing untracked `.gitkeep` placeholders under
`checkpoints/`, `evidence/`, `feedback/`, and `tickets/` were preserved and
were not staged.

This review uses the effective A1/A2/A3 contract: Portfolio, Testimonials,
and Events fixture uploads; the single local `DocumentStore` byte boundary;
opaque metadata; exact-key same-origin reads; atomic replacement; scoped
cleanup; empty/initial-state behavior; UI and accessibility obligations; and
the direct T15 regression boundary. The historical Portfolio-only checkpoint
at `387dde9` is preserved history and is not the review target. No T15 PASS,
production provider, authentication, public delivery, consent, publication,
SQL, or deferred successor authority was imported into scope.

## Review Contract assessment

| Row | Ticket authority and required proof | CK evidence | Status |
| --- | --- | --- | --- |
| MEDIA-01 | T18/A2 validated Portfolio, Testimonials, and Events upload only after type/signature/size/scope validation, storing bytes under generated opaque keys in owned namespaces. | Actual browser form POSTs for all three collections; JPEG/PNG/WebP, invalid signature/type/size/scope, filename/traversal, hash/size/type/alt, record association, and isolated inventory passed in `node scripts/t18-media-evidence.mjs`. | PROVEN |
| MEDIA-02 | Exact-key same-origin reads must serve recorded bytes without directory listing, root disclosure, public static delivery, external origin, or customer output. | Route assertions passed for successful GET, unknown/malformed keys, recorded media headers, absent-root behavior, and `/`, `/customer`, and public-negative HTML. The `[storageKey]` handler and source scan were inspected. | PROVEN |
| MEDIA-03 | Replacement must be atomic, failed replacement must preserve the old readable object/pointer, successful replacement must remove the old object, and cleanup must stay inside the T18 namespace. | The route harness passed invalid and valid PUT for every collection, old-key 404 after replacement, record-scoped DELETE, orphan/temp/association cleanup, sibling/outside-sentinel preservation, and teardown inventory. | PROVEN |
| MEDIA-04 | H-18-MEDIA must expose opaque typed metadata through a replaceable `DocumentStore`-compatible seam; UI/components may not import filesystem/provider details. | Inspected `shared/document-store.ts`, `shared/server/local-document-store.ts`, `shared/server/local-content-image-store.ts`, route imports, and UI source. The actual caller → handler → facade → `DocumentStore` → exact-key GET → UI traces passed. | PROVEN |
| MEDIA-05 | A2 Testimonials and Events uploads must traverse their actual forms and handler, use separate ignored namespaces, return typed metadata, and preserve closed record scope. | Browser route-level evidence covered `/admin/content` Testimonials and `/admin/events` add/edit callers, three namespace inventories, reload, metadata, invalid alt/type/size/signature/scope, and traversal cases. | PROVEN |
| MEDIA-06 | Collection-isolated read/replace/delete/cleanup must preserve the last readable object on failure and prevent cross-collection or public/customer media output. | Cross-collection PUT/DELETE returned 404 without changing the object; replacement failure preserved bytes/pointer; successful replacement and cleanup were verified across all namespaces; public/customer negatives passed. | PROVEN |
| MEDIA-07 | Every Portfolio, Testimonials, and Events browser upload must reach the one local `DocumentStore` byte implementation; the lifecycle facade may only associate/replace/remove opaque keys. | `scripts/t18-document-contract.mjs` passed the in-memory alternate `put/read` substitution for all three collections, injected write-failure preservation, replacement, and removal. Import/source scan found only the local adapter importing `node:fs/promises`. | PROVEN |
| UI-MEDIA-01-A2 | All three authorized add/edit forms must expose the upload control, render stored previews/cards as specified, preserve source fields/copy/shared frame, and keep add uploads proposed-only. | Paired source/application captures and browser assertions cover populated, empty, error, add/edit, uploaded, fallback, proposed-add, and four widths for all three panels. Proposed uploads did not create cards. | PROVEN |
| UI-MEDIA-02-A2 | Testimonials and Events must satisfy the T03 idle/loading/success/error/disabled/focus/keyboard/reduced-motion/no-overflow contract without color-only meaning. | Browser evidence passed labels/status/alert, focus, delayed-submit disabled state, invalid/error, reduced-motion context, and no-horizontal-overflow at 1440/930/720/485; refreshed T15 visual harness also passed. | PROVEN |
| EMPTY-18-STORE / EMPTY-18-A2 | Absent namespaces stay absent through unknown/empty/error reads; the first valid upload is the sole initializer; no hidden/default media or durable seed is introduced. | Clean-root unknown-key, empty/error/public/customer observations, first valid upload per collection, idempotent reload, namespace isolation, cleanup, and teardown passed. No SQL migration or durable seed is present. | PROVEN |
| REG-M1-T15 | T15 fixed Portfolio images, Draft/Published distinction, public-draft exclusion, proposed result, empty/error behavior, and shared shell remain green. | `node scripts/t15-visual-evidence.mjs` exited 0 with 92 generated regression captures covering public-negative, fixed images, states, keyboard/focus, and Bookings/Availability/Content/Events shell at the named widths. | PROVEN |
| SEC-MEDIA-01/02/03 | Closed validation, exact serving, stable non-disclosing errors, namespace integrity, atomic lifecycle, isolated teardown, and future auth/policy seam preservation are required. | Route negatives and source/import scan passed; generated keys and fixed collection roots were inspected; filesystem ownership is confined to the local `DocumentStore`; no production credentials/provider/public path were added. | PROVEN |

## Validation and evidence performed

- `npm run check` — PASS: ESLint, TypeScript, 13 repository tests, and the Next.js production build.
- `node scripts/t18-document-contract.mjs` — PASS: in-memory alternate `DocumentStore` substitution preserved the UI metadata contract and lifecycle for all three collections.
- `$env:M1_T18_TEST_STORAGE_ROOT='C:\Workspace\bride-assistant\.local-storage\t18-live\portfolio'; $env:T18_BASE_URL='http://localhost:3318'; $env:T18_EVIDENCE_DIR='C:\Users\ASUS\AppData\Local\Temp\t18-ck-review-20261122'; node scripts/t18-media-evidence.mjs` — PASS, exit 0. The harness exercised actual browser callers and route-level lifecycle/negative cases for Portfolio, Testimonials, and Events; 57 temporary captures were generated.
- `$env:T15_BASE_URL='http://localhost:3318'; $env:T15_EVIDENCE_DIR='C:\Users\ASUS\AppData\Local\Temp\t15-ck-review-20261122'; node scripts/t15-visual-evidence.mjs` — PASS, exit 0; 92 temporary regression captures were generated.
- Independently inspected the submitted checkpoint, effective ticket amendments, implementation diff, `shared/document-store.ts`, the local adapter/facade, request parser, both fixture route handlers, `AdminContentWorkspace`, the A2 evidence comparison, and the T15 regression evidence.
- Traced the three actual use paths: `/admin/content` Portfolio and Testimonials forms, and `/admin/events` Event form → shared `FormData` caller → fixture handler → validated `localContentImageStore` → local `DocumentStore` byte `put/read` → fixed ignored namespace → exact-key GET → admin preview/card/proposed result. Replace/delete/cleanup use the same bounded facade and do not change publication state.
- Confirmed migration hygiene and empty-state behavior: no SQL migration, production-data change, or durable seed is present; absent namespaces were not initialized by unknown, empty, error, public, or customer reads; only valid upload created a collection namespace; teardown removed temporary data.

## Frozen Finding Closure Matrix

No blocking findings were identified. Therefore no `CK-###` findings or
closure clauses are frozen for CFC.

## Scope-change observations

None. A1/A2/A3 are already planning-authorized ticket amendments. No product,
architecture, provider/runtime, deployment, policy, or predecessor-reopening
decision is required to resolve this review.

## Decision

`PASS`. The committed checkpoint satisfies the effective M1-T18 contract and
its explicit A1–A3 review obligations. No CFC or HMN authorization is required.
