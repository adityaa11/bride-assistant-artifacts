# M1-T18 — Implement the fixture-local content media storage seam

- State: `ready`; batch/milestone/work package: `4 / M1 / M1-W07`; dependencies: T02/H-02 and T03/H-03 `sequence`.
- Prior Portfolio-only GO commit: `387dde9aae69fe37795935b798e324e0fa315e3d`; historical [checkpoint](../../checkpoints/M1-T18-fixture-local-content-media-387dde9.md). Amendment A2 below supersedes it as the current review target; a new GO checkpoint must bind the expanded contract before CK reviews T18.
- Effective review scope: original obligations except the exact Portfolio-only exclusions superseded by A2, plus A2's Testimonials/Events rows and A3's universal `DocumentStore` row. CK must use this effective contract for the next submitted checkpoint.
- Planning source binding: code repository `https://github.com/adityaa11/bride-assistant.git` at `387dde9aae69fe37795935b798e324e0fa315e3d`; A2 changes the ticket contract, not that code revision.
- Source authority: M1-W07 backend-replacement seam inventory; M1-T15-A7; accepted Stack `DocumentStore` boundary and D-C03/D-06; M2-W04 and M4-W01/M4-W05 handoff contracts.
- Dependencies and accepted predecessor outputs: T02/H-02 typed scenario and command-result contracts; T03/H-03 visual/form/a11y primitives. The committed T15 implementation at `8c40bf2` and its historical evidence are inspected inputs, not an approved predecessor dependency; T18 owns the direct media gap and regression checks without requiring T15/H-15 `PASS`.
- Successors and handoffs: H-18 → T17/H-17 M1 review seam inventory; H-18 → M2-W04 contract integration and M4-W01/M4-W05 server-side media activation. The later backend ticket replaces the local adapter without changing the UI's opaque-key contract.
- Activation: `active-in-this-ticket` for the admin fixture upload/read/replace/cleanup path only.
- Reuse decision: `reuse-existing` accepted `DocumentStore` byte boundary; the fixture adapter is a local implementation/facade and must not create a second production storage authority.
- Legacy path: `forbidden`; remove the T15 client-side `FileReader`/data-URL preview path and direct filesystem access from browser components.
- SQL change classification: `not-applicable`; no relational schema, migration, seed, or production-data change.
- Empty/initial-state contract: required for the absent local directory, empty Portfolio collection, unknown storage key, and first valid upload.

## Outcome

Provide a real, reviewable fixture-only Portfolio image upload path that writes to an ignored local directory, reads through a same-origin admin-fixture handler, replaces images atomically, cleans up owned files, and hands a storage-vendor-neutral contract to later backend work. The path remains non-authoritative: it does not create production content, grant consent, publish public media, or change official publication state.

## Exact scope

- Add a server-owned local adapter rooted at `.local-storage/m1-t15/content/portfolio/`, created on demand and excluded from Git. Tests may override the root with an isolated temporary directory through a test-only configuration seam.
- Accept one Portfolio image per add/edit submission as `multipart/form-data`: JPEG, PNG, or WebP only, maximum 5 MiB, with declared-type and byte-signature/size validation. Generate an opaque storage key and never use the client filename as a path.
- Store bytes and adapter metadata in the owned namespace. The adapter returns `{ storageKey, sha256, size, mediaType, altText }`; fixture metadata may associate that key with the named T02 Portfolio record without introducing a production database.
- Replace the current client-side data-URL preview with the actual upload/read path. Portfolio forms expose the upload control; Testimonials and Events retain their source-mapped forms and do not gain image upload in this ticket.
- Serve an exact opaque key through `/api/fixtures/m1-t15/content-media/[storageKey]` with no directory listing, traversal, public static path, or external origin. Unknown or malformed keys return a non-success response without filesystem disclosure.
- Preserve T15's fixed local checked-in Portfolio images as the baseline fixture records. Uploaded fixture media is an additional local fixture state and must not make the existing public route render draft content.

## Entrypoint

The caller is the Portfolio add/edit form in `AdminContentWorkspace` at `/admin/content`. It submits a trusted-by-composition but unauthenticated-in-M1 fixture form with typed record identity, descriptive alt text, and one untrusted multipart file. The server fixture handler validates the record scope, file type/signature, size, opaque-key/path boundary, and failure result before calling the local adapter. Invalid input returns an accessible fixture error and leaves the prior stored image unchanged.

## Endpoint

- `POST /api/fixtures/m1-t15/content-media` accepts the Portfolio upload and returns `201` with `{ storageKey, sha256, size, mediaType, altText }` on success; invalid type, signature, size, record scope, or write failure returns a stable non-success fixture error.
- `GET /api/fixtures/m1-t15/content-media/[storageKey]` returns the stored bytes with the recorded media type and length, or `404`/`400` for an unknown or malformed key. It never exposes the local root or a directory listing.
- `PUT /api/fixtures/m1-t15/content-media/[storageKey]` performs validated atomic replacement and returns the new metadata; a failed replacement preserves the prior readable object and pointer.
- `DELETE /api/fixtures/m1-t15/content-media/[storageKey]` removes only the referenced object in the T18 namespace and returns `204`; it never changes publication state.
- The observable UI result is a named non-authoritative proposed fixture result. Upload success may update the fixture preview and storage metadata, but it does not become a customer/public content record or official publication transition.

## Handoffs

### H-18-MEDIA

- Producer: T18 local fixture media handler and adapter.
- Consumer: M2-W04 contract integration and later M4-W01/M4-W05 server-side media implementation.
- Transport: typed adapter result and same-origin fixture handler response.
- Schema: `storageKey` (opaque string), `sha256` (hex string), `size` (positive integer), `mediaType` (`image/jpeg` | `image/png` | `image/webp`), and `altText` (non-empty bounded string).
- Integrity/identity: generated key, validated bytes, digest, size, media type, and record association; client filenames are not identity.
- Lifecycle: `put` creates, `read` retrieves, `replace` swaps atomically, and `remove` deletes within the owned namespace. Later production adapters implement the accepted `DocumentStore` `put/read` byte boundary and add authorization/policy.
- Activation: active in T18 for fixture routes; production activation is deferred to M4 and must not be claimed by this ticket.

### H-18-REVIEW

- Producer: T18 route and evidence harness.
- Consumer: T17/H-17 review matrix.
- Transport: committed checkpoint evidence and source-to-component map.
- Required fields: reviewed commit, route/scenario, adapter version, upload/read/replace/cleanup results, public-negative result, security/UI evidence, and M4 successor mapping.

## Expected use-path

`EUP-18-MEDIA`: `/admin/content` Portfolio form → `POST /api/fixtures/m1-t15/content-media` → fixture handler validation → local `DocumentStore`-compatible adapter → ignored `.local-storage/m1-t15/content/portfolio/` bytes/metadata → same-origin `GET` handler → Portfolio card/editor preview → reviewer capture. The replace and delete actions use the same adapter namespace and preserve the proposed-result/non-authoritative boundary. Required actual-use evidence is a browser or route-level integration scenario, not a unit-only adapter call.

## True code path and legacy record

- True operational path: browser `FormData` → server fixture handler → validated local adapter → opaque-key media handler → Portfolio fixture UI.
- Superseded paths to remove or make unreachable: client `FileReader`/`readAsDataURL` preview as the upload result, direct browser filesystem access, arbitrary client-provided paths/URLs, and any public/customer import of the local media handler.
- Retirement proof: source scan and route-level assertions show no data-URL persistence or direct client filesystem access; public route negative evidence shows no local fixture-media output.
- Legacy status: `forbidden`; no compatibility fallback is retained merely for tests.

## SQL migration hygiene

No SQL is authorized. The local directory and its manifest are an ephemeral fixture/development mechanism, not a migration or production-data seed. Tests create an isolated temporary storage root, run the upload/read/replace/cleanup scenarios, and remove it in teardown. A test-only SQL seed, migration branch, or durable repository asset generated by upload is forbidden.

## Empty and initial state

### EMPTY-18-STORE

- Condition: `.local-storage/m1-t15/content/portfolio/` and its adapter metadata are absent or empty.
- Expected result: the adapter creates the owned directory on the first valid upload; before that, the existing T15 empty Portfolio state remains empty and no hidden image/default is created. Unknown keys return `404` and do not initialize storage.
- Initialization: only an explicit valid Portfolio upload may create a stored object; initialization is idempotent and scoped to the owned root.
- Forbidden behavior: no durable seed, migration, public default image, directory listing, or hidden fixture record.
- Evidence: clean temporary-root integration test plus `/admin/content/empty`, `/admin/content/error`, and public-negative route observations.

## Required invariants

- Local bytes stay outside `public/`, outside PostgreSQL, outside customer/public adapters, and outside source control.
- The adapter is the only filesystem owner; browser components consume typed metadata and same-origin URLs, never filesystem paths.
- Record identity and publication state remain fixture-only; upload, replace, delete, and proposed Publish never create an official transition.
- Write and replacement failures preserve the last readable object; cleanup cannot traverse outside the owned directory.
- The production `DocumentStore`/Neon decision remains a later adapter choice; T18 does not select credentials, provider, retention, malware scanning, backup, or recovery behavior.

## Explicit non-goals

No real authentication or role enforcement, customer photos or consent, public media delivery, production persistence, Neon/Object Storage adapter, PostgreSQL metadata, malware scanner, retention/deletion policy, backups/recovery, upload presigned URLs, content publication authority, testimonial/event image upload, or M4 production acceptance.

## Reuse and centralization decision

- `RD-18-01`: `reuse-existing` the accepted `DocumentStore` byte boundary from Stack/D-C03/D-06. The local adapter owns only the fixture filesystem and lifecycle wrapper; it must return the accepted opaque-key/hash/size/media-type contract and remain replaceable by M4.
- Owner: T18's server fixture adapter for local implementation; M2-W04/M4-W01 owns the production contract/adapter handoff.
- Error semantics: typed validation, not-found, traversal, write, replace, and cleanup failures return stable non-success results without local path disclosure.
- Centralization boundary: do not create a global media registry or share this fixture adapter with booking proof/customer media; those have different authority, privacy, lifecycle, and consumer boundaries.

## Review Contract

| ID | Authority and binary pass condition | Required harness/scenarios/observations | Evidence and forbidden substitutes | Direct-regression boundary |
| --- | --- | --- | --- | --- |
| MEDIA-01 | M1-T15-A7 write behavior and H-18-MEDIA: PASS iff one valid Portfolio upload is accepted only after type/signature/size validation and stored under an opaque key in the owned local namespace. | Actual `/admin/content` form → POST handler; valid JPEG/PNG/WebP; invalid type/signature/oversize; traversal and filename cases. | Route/integration evidence, storage inventory, metadata assertion, and source scan. Unit-only validation or a data-URL preview is not a substitute. | T18 Portfolio upload path only. |
| MEDIA-02 | A7 read/serving visibility: PASS iff the same-origin fixture handler reads the stored object by exact opaque key and never exposes a directory, local root, public static URL, or external origin. | GET success, unknown key, malformed/traversal key, public route negative, customer route negative. | Browser/route assertions and response headers/status; build-only evidence is forbidden. | T18 media handler and T04/public direct paths only. |
| MEDIA-03 | A7 replace/cleanup: PASS iff replacement is atomic, failed replacement preserves the old object, successful replacement removes the old object, and explicit cleanup stays inside the T18 namespace. | Replace success/failure, delete, orphan cleanup, teardown, and filesystem-boundary observations. | Isolated temp directory inventory before/after and route-level result; generic file existence alone is insufficient. | T18 adapter lifecycle only. |
| MEDIA-04 | A7 backend handoff: PASS iff the UI/route consumes opaque metadata through the adapter seam compatible with `DocumentStore` and no component imports filesystem/provider details. | Import scan, interface/consumer inspection, actual EUP-18-MEDIA trace. | Source map and route evidence; speculative future provider code is forbidden. | T18-to-M2/M4 handoff only. |
| UI-MEDIA-01 | T15-A7 and T03 visual contract: PASS iff only Portfolio exposes the source/user-authorized upload control, with the existing AdminWorkspaceFrame, card crop, copy, and non-authoritative result intact. | Portfolio populated/empty/error/add/edit states; Testimonials and Events confirm no upload control. | Same-viewport source/implementation captures and source-to-component map; generic visual similarity is insufficient. | T18 Portfolio content composition and direct T15 routes. |
| UI-MEDIA-02 | T03 UI/a11y contract: PASS iff upload idle, loading, success, invalid/error, disabled, focus, keyboard, reduced-motion, and narrow responsive states are understandable and usable without color-only meaning or overflow. | Keyboard focus, accessible label/status, invalid/oversize, upload progress/result, 1440/930/720/485 widths. | Playwright/browser evidence and captures; lint/build alone is forbidden. | T18 upload control and direct shell regressions. |
| REG-M1-T15 | T15 accepted rows remain proven: PASS iff Draft/Published distinction, public-draft exclusion, existing fixed images, empty/error/proposed result, and A4 shared shell remain green. | Existing T15 harness plus refreshed public-negative, proposed-result, empty/error, and shell checks. | Existing proven T15 evidence is carried forward; only direct regressions are rechecked. | T15 routes and Bookings/Availability/Content/Events shell at named widths. |

## Security readiness

Status: `applicable`

Inherited boundaries: `SEC-M1-01` future server identity/role/ownership seam; `SEC-M1-02` safe framework rendering/no unsafe sink/external origin/client authority; Stack `DocumentStore` keeps bytes outside PostgreSQL and storage keys are not authorization.

Trust boundaries: untrusted multipart file and client metadata → server fixture handler; handler → local filesystem namespace; media handler → browser; T18 fixture surface → public/customer route boundary.

Sensitive assets: local fixture image bytes, opaque storage keys, hashes, media metadata, and descriptive alt text. No real customer or payment-proof bytes are allowed.

Implement-now controls:

- `SEC-MEDIA-01` (ASVS input/file/API; OWASP upload/injection/misconfiguration; NIST SI/SC): allowlist type and signature, size limit, generated opaque names, path traversal rejection, bounded alt text, and stable non-disclosing errors. Evidence binds to MEDIA-01/02.
- `SEC-MEDIA-02` (ASVS data/output/API; NIST SC/SI): exact-key reads only, no directory listing, no public/static or external origin, recorded media type/length, and no client filesystem/provider coupling. Evidence binds to MEDIA-02/04.
- `SEC-MEDIA-03` (ASVS business logic/data integrity; NIST SI/CP): atomic replace, failure preservation, namespace-bounded delete/cleanup, and isolated teardown. Evidence binds to MEDIA-03.

Preserve seams:

- `SEC-MEDIA-SEAM-01`: future server authentication, admin role/ownership, consent, private/public policy, audit, retention, malware scanning, backup, and recovery attach at the handler/DocumentStore boundary; T18 must not claim or bypass them.

Deferred policy: `SEC-MEDIA-POLICY-01` keeps D-03 production file scanning, retention, legal basis, recovery, and provider/account controls for M2/M4/M5. No production media capability is activated by T18.

## UI readiness

Project state: established T03 Soft Rose Atelier visual contract; task mode: refactor/extend the existing T15 admin content workspace.

- `UI-MEDIA-01` reuses `AdminWorkspaceFrame`, T15 source-derived form/card geometry, and T03 form/status primitives; it does not create a new shell or visual language.
- `UI-MEDIA-02` defines upload idle/loading/success/error/disabled/focus states, accessible names/status, keyboard operation, reduced motion, and responsive containment at 1440, 930, 720, and 485px.
- `UI-MEDIA-03` requires same-viewport Portfolio form/card captures, no upload control in Testimonials/Events, no overflow, and direct T15 shell/publication regression evidence.

## Validation and evidence

- Add a route-level Playwright suite for actual `/admin/content` upload/read/replace/cleanup, invalid files, failure preservation, public/customer negatives, and accessible UI states. Use an isolated temporary storage root and remove it in teardown.
- Add the source/import scan for `FileReader`, `readAsDataURL`, direct client `fs`, arbitrary URL/path input, public media imports, and prototype storage.
- Run `npm run check`, the refreshed T15 visual/public-draft/shared-shell harness, and the T18 suite. Capture same-viewport source/implementation evidence at 1440, 930, 720, and 485px and update the T15 ledger/T17 seam evidence with H-18.
- GO must record the actual caller → handler → adapter → local directory → read handler → UI path, exact commit, temporary-root cleanup, and all Review Contract outcomes before `READY_FOR_CK`.

## Hard stop / planning findings

- Do not add production auth, production `DocumentStore`/Neon configuration, public media delivery, PostgreSQL metadata, or M4 policy controls.
- Stop for planning if the requested behavior requires customer/public visibility, real consent, retention/legal policy, provider credentials, durable production records, or a second storage abstraction that conflicts with `DocumentStore`.

## Amendment M1-T18-A1 — remove circular T15 approval dependency

- Planning correction on 2026-11-22: T15's latest CK result is `CHANGES_REQUIRED`, and T18 exists to address its media-boundary gap. T18 therefore must not require a T15/H-15 CK `PASS` before GO.
- T18 consumes only the approved T02/H-02 and T03/H-03 contracts. It uses T15's committed implementation/evidence as a bounded inspection baseline, preserves T15's historical outcomes, and proves the direct T15 media/publication regressions in `REG-M1-T15`.
- T17 consumes H-18 for the T15 continuation evidence; no historical T15 artifact is rewritten or silently promoted to `PASS`.

## Amendment M1-T18-A2 — fixture image upload for Testimonials and Events

### Planning authority and supersession

- Explicit user direction on 2026-11-22 expands this ticket to image upload for the Testimonials tab at `/admin/content` and Events at `/admin/events`, in addition to Portfolio at `/admin/content`. This is a bounded fixture-only scope change within M1-W07. It does not authorize public delivery, production storage, customer media, real-person testimonial bytes, consent assertion, official publication, or a new provider.
- A2 supersedes the Portfolio-only phrases in this ticket's Outcome, Exact scope, Entrypoint, Endpoint POST, EUP-18-MEDIA, True code path, Empty/initial-state contract, Explicit non-goals, `UI-MEDIA-01`, `UI-MEDIA-03`, and Validation that exclude Testimonials/Events upload. The original Portfolio behavior, `MEDIA-01`–`MEDIA-04`, `UI-MEDIA-02`, `REG-M1-T15`, security controls, and non-authoritative boundaries remain binding. The superseded text is historical; the effective conditions below take precedence for the next CK review. No old Review Contract ID is renumbered.
- T15-A7 says Testimonials/Events keep non-image forms **unless a later ticket explicitly expands them**. This A2 is that later explicit expansion. T15's previous CK finding and T18's Portfolio-only GO checkpoint remain immutable history. T15 CK-001.a's historical no-file-input oracle under A6 is not an active T18 acceptance gate; CK reviews T18's effective A2 scope and the retained non-authority/publication regressions. The old Portfolio-only checkpoint is insufficient evidence for A2 and must not be reviewed as if it implemented these new obligations.
- Ticket state is `ready` for one bounded GO continuation. After implementation, GO records a new exact code commit and checkpoint, sets the ticket to `awaiting_review`, and hands the whole effective contract to CK. This planning amendment is not a CK `PASS` or a substitute for a committed implementation.

### Extended entrypoints, storage and endpoint

- Existing `AdminContentWorkspace` owns all three callers: Portfolio add/edit, Testimonials add/edit on `/admin/content`, and Events add/edit on `/admin/events`. Each may submit at most one untrusted JPEG, PNG, or WebP file with a 5 MiB limit and a non-empty bounded alt text when a file is supplied. The handler accepts only closed T02 fixture record IDs `portfolio-draft`, `portfolio-published`, `testimonial-draft`, `testimonial-published`, `event-draft`, `event-published`, plus `portfolio-new`, `testimonial-new`, and `event-new` for the respective add-form proposed results. It derives the collection from the validated record ID; arbitrary client collection names, paths and URLs have no authority. Add-form uploads may update only the local proposed-result preview/association; they do not create a T02 content record.
- The existing `ContentImageStore` typed metadata and `put/read/replace/remove` byte seam serve all three collections. Preserve Portfolio's current `.local-storage/m1-t15/content/portfolio/` objects and manifest; add sibling owned namespaces `.local-storage/m1-t15/content/testimonials/` and `.local-storage/m1-t15/content/events/`, created only on first valid upload for that collection. The adapter alone resolves these fixed paths. A key's collection/record association is kept in adapter metadata. Cross-collection replacement or deletion must fail without changing either object or pointer. Exact-key GET may resolve across only these three fixed namespaces and must reveal neither the root nor a listing.
- The existing POST, exact-key GET/PUT/DELETE, collection-metadata GET, and orphan-cleanup endpoint semantics extend to the validated Testimonials and Events record scopes. The JSON media result stays `{ storageKey, sha256, size, mediaType, altText }`; collection and record association remain server-side fixture metadata. PUT uses the validated multipart `recordId`; DELETE supplies the same closed `recordId` with the exact key, and the handler rejects a mismatch before removal. Upload, replace and cleanup failures preserve the last readable object/pointer; one collection's cleanup cannot remove another collection's files. Unknown or malformed keys retain stable non-disclosing errors.
- The two checked-in Portfolio images remain the fallback for its named records. Testimonials and Events have **no hidden/default image**: absent uploaded metadata leaves their source-mapped cards unchanged. Their empty/error routes stay empty/error and do not initialize a namespace. The public/customer routes must not import, render or link any of the three fixture media namespaces.

### UI and expected use paths

- Keep the shared `AdminWorkspaceFrame`, existing tab/navigation and T03 visual system. Add a labeled file control, alt text, help, idle/loading/error/success/disabled states and same-origin stored-image preview to Testimonials and Events add/edit forms using the Portfolio field/preview geometry already traced to the mapped reference. Preserve their existing source fields, validation, quote/event copy, action controls, and publication distinction. Do not execute the reference's browser storage, data-URL conversion, arbitrary HTTPS URL path, or prototype publication script.
- When an uploaded image is associated with a named record, its admin fixture card renders that image using the existing `.web-media img` crop: the testimonial image appears above its quote, and the event image occupies the existing media region instead of the theme symbol. Without an upload, testimonial cards keep their quote-only source composition and event cards keep their theme symbol. A new proposed add-form upload is visible in the proposed result/editor preview without silently adding a card. This visual treatment is an explicit A2 planning decision for the two source panels, which have no native image field; no other card regions or responsive rules may be invented.
- `EUP-18-TESTIMONIAL`: `/admin/content` → Testimonials tab → add/edit form → multipart POST/PUT handler → validated testimonial namespace → same-origin exact-key GET → editor/card image preview and proposed fixture result → reviewer capture. `EUP-18-EVENT`: `/admin/events` → add/edit form → the same handler → validated event namespace → exact-key GET → editor/card image preview and proposed fixture result → reviewer capture. Replace/delete/cleanup are reachable through the same fixture surface and do not change publication state. Route-level or browser integration proof is required for each path; an adapter-only test is insufficient.

### Additional Review Contract rows for CK

| ID | Authority and binary pass condition | Required harness/scenarios/observations | Evidence and forbidden substitutes | Direct-regression boundary |
| --- | --- | --- | --- | --- |
| MEDIA-05 | A2: PASS iff a valid Testimonials image and a valid Events image each traverse their actual form and handler, reach separate ignored namespaces under opaque keys, and return the existing typed metadata. | `/admin/content` Testimonials and `/admin/events` add/edit; valid JPEG/PNG/WebP across the expanded path; invalid declared type, signature, oversize, missing alt, wrong/forged record scope and filename/traversal; first upload and reload. | Route-level Playwright, namespace inventory, SHA-256/size/type/alt and record association; no unit-only or data-URL proof. | New Testimonials/Events upload paths and shared handler. |
| MEDIA-06 | A2: PASS iff exact-key read, atomic replace, delete and orphan cleanup preserve collection isolation, last readable object on failure, and no public/customer media output. | Each collection's GET/PUT/DELETE; failed and successful replacement; cross-collection key/record attack; cleanup with sibling sentinel; unknown/malformed keys; empty-root first use and teardown. | Actual routes plus isolated filesystem before/after and public/customer negatives; a generic suite pass is insufficient. | Three T18 namespaces and fixture media handler only. |
| UI-MEDIA-01-A2 | A2 replaces only `UI-MEDIA-01`'s Portfolio-only exclusion: PASS iff Portfolio, Testimonials and Events expose the authorized control in their own add/edit forms, render stored previews/cards as specified above, and preserve source fields/copy/shared frame and proposed-result semantics. | Populated, empty, error, add/edit and uploaded states for all three; absent-upload testimonial/event fallback; no card from a proposed add; source-to-component map and same-viewport captures at 1440/930/720/485. | Paired reference/implementation captures and browser assertions; Portfolio-only evidence or invented unrelated UI is insufficient. | T18 content composition and directly affected T15 shell/publication paths. |
| UI-MEDIA-02-A2 | A2 extends the existing `UI-MEDIA-02` accessibility/interaction obligation to the two new callers: PASS iff idle/loading/success/invalid/error/disabled/focus, keyboard and reduced-motion behavior are usable without color-only meaning or horizontal overflow. | Testimonials and Events file/alt labels, status/alert, keyboard focus, failed upload preservation and 1440/930/720/485 captures. | Browser/Playwright evidence; build/lint alone is insufficient. | New controls and shared form field only. |
| EMPTY-18-A2 | A2: PASS iff absent Testimonials/Events namespaces remain absent through empty/error/unknown-key reads, with first valid upload as the sole initializer and no hidden/default media. | Clean isolated root, `/admin/content/empty`, `/admin/events/empty` and error routes, unknown key, first valid upload per collection, idempotent read and teardown. | Route/UI plus filesystem observations; seeds or a prepopulated fixture are forbidden substitutes. | New T18 namespaces only. |

### Security, validation and handoff binding

- Security readiness remains `applicable`. `SEC-MEDIA-01/02/03` now protect all three collection scopes. Implement-now: closed record/collection validation, type/signature/size/alt checks, generated keys, fixed namespace path resolution, exact-key serving, stable errors, atomic pointer update, bounded cleanup, and public/customer negatives (ASVS file/input/API/data; OWASP upload/access-control/injection; NIST SC/SI). Preserve the future auth, admin ownership, consent, retention, scanning and production delivery seam at the handler/`DocumentStore` boundary. Real testimonial/customer image bytes and any claim that permission was obtained are forbidden in M1; only fictional fixture images may be used. D-03/M2/M4/M5 production policy remains deferred.
- GO must expand the T18 route-level Playwright suite and source/import scan to the two new expected paths and negative cases; use an isolated temporary root with teardown; rerun `npm run check`, the T15 visual/public-draft/shared-shell harness, and the T18 suite. Capture source and implementation at 1440/930/720/485 for both new forms, uploaded cards and fallback cards, plus keyboard/focus/error/loading evidence. Update the T15 source ledger and H-18/T17 seam evidence. The new checkpoint must identify the actual caller → handler → adapter namespace → GET → UI path for Portfolio, Testimonials and Events, exact commit, migration inventory (`none`), clean initial-state observations, and row-by-row closure before `READY_FOR_CK`.
- H-18-MEDIA keeps its opaque metadata schema and `DocumentStore`-compatible byte boundary, now with three fixture-only collection consumers. H-18-REVIEW/T17 receives the expanded route/scenario matrix and distinguishes the historical Portfolio-only checkpoint from the new current review target. No production provider, public delivery, real consent or official publication is activated.

## Amendment M1-T18-A3 — one DocumentStore byte boundary for every upload

- Explicit user direction on 2026-11-22: every uploaded media byte in this project must pass through `DocumentStore`. The fixture implementation is local and disposable; the later production implementation uses Neon Object Storage's S3-compatible API. This binds Portfolio, Testimonials, Events, and the later payment-proof path without authorizing production media delivery in M1.
- A3 clarifies A2's `ContentImageStore` wording: content-specific record association, replace, remove and orphan cleanup may be a lifecycle facade, but its byte `put/read` operations must delegate to the single `DocumentStore` contract. It is not an independent byte store or second provider authority. The local filesystem is owned only by the fixture `DocumentStore` adapter; browser components and content lifecycle code receive opaque metadata, never paths or provider details. The current `387dde9` Portfolio implementation is inspected input and must be reconciled to this boundary in the next GO checkpoint; it is not proof of A3.
- Preserve the accepted Stack `DocumentStore` `put/read` result (`storageKey`, SHA-256, size, media type), plus the T18 `altText` and collection/record association above the byte layer. Uploaded bytes stay outside PostgreSQL and `public/`. The two checked-in Portfolio baseline images are static fixtures, not uploads, and do not need to be rewritten into `DocumentStore`. No local fixture object is migrated to production.
- Review row `MEDIA-07`: PASS iff the actual Portfolio, Testimonials and Events browser uploads each reach the one local `DocumentStore` byte implementation through the server handler; the content lifecycle facade only associates/replaces/removes opaque keys; no separate content byte writer or direct browser filesystem/provider path exists. Required proof is the three route-level EUP traces, interface/import scan, isolated-root write/read inventory, and a contract-level substitution check using an in-memory alternate implementation of the same `put/read` byte interface without changing UI metadata. This proves the seam without implementing a speculative production adapter. Unit-only calls or naming an unrelated interface `DocumentStore` are insufficient for the actual upload path. Direct-regression boundary is the T18 media path and its M2/M4 handoff; production Neon credentials or provider activation are forbidden in M1.
- H-18-MEDIA/T17 must carry `MEDIA-07` as an active row. M2-W04 consumes the universal byte contract; M4-W01 owns the Neon Object Storage production adapter; M4-W05 owns durable content association and permission; M4-W06 governs public delivery. The Project Context v3.1, Stack Context v1.3 and Implementation Context v2.7 record this division.
