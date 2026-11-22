# M1-T18 — fixture-local Portfolio media seam

- Ticket: `M1-T18`; batch `BAP-M1-FIXTURE-UI-SET-01 / 4`; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact review-target code commit: `387dde9aae69fe37795935b798e324e0fa315e3d`.
- Frozen ticket: [M1-T18](../tickets/m1-fixture-ui-assembly/M1-T18-fixture-local-content-media.md); T02/H-02 and T03/H-03 were approved/PASS. T15 at `8c40bf2` is an inspected input under the amended T18 dependency, not a claimed PASS.
- Evidence: [source comparison and route observations](../evidence/M1-T18/comparison.md), ten captured images in the same directory, and committed `scripts/t18-media-evidence.mjs`.

## Bounded implementation

Portfolio add/edit forms now submit one file through `FormData`. The handler checks a closed record ID, bounded alt text, declared JPEG/PNG/WebP media type, byte signature, and 5 MiB limit. The server adapter generates opaque keys and stores bytes, metadata, and a record-key manifest under ignored `.local-storage/m1-t15/content/portfolio/`. Exact-key GET serves media; PUT writes a readable new object before atomically replacing the manifest and deleting the old object; DELETE clears only the named object; explicit namespace cleanup removes orphans. The original checked-in Portfolio images remain the baseline. No testimonial/event upload, publication mutation, public media route, real identity, production provider, SQL, or customer content was added.

## Review Contract Closure

| Row | Ticket authority / pass condition | Required proof, locator, exact command/outcome | Status |
| --- | --- | --- | --- |
| MEDIA-01 | T18 Review Contract and T15-A7: Portfolio-only validated upload, opaque key, owned namespace. | Actual form POST, valid PNG/JPEG/WebP, invalid signature/type/oversize/scope, filename traversal, hash/size/metadata and temp-root inventory: `node scripts/t18-media-evidence.mjs` PASS; `comparison.md`. | PROVEN |
| MEDIA-02 | T18 read/visibility: exact-key same-origin read, no listing/root/public/customer output. | GET bytes/type/recorded size, unknown/malformed keys, public/customer negatives: T18 script PASS; `comparison.md`. | PROVEN |
| MEDIA-03 | T18 lifecycle: atomic pointer update, failed replacement preservation, successful old-key removal, namespace cleanup. | Invalid PUT leaves bytes/pointer; valid PUT changes manifest and old key 404; DELETE and orphan cleanup preserve sibling sentinel; isolated root removed in teardown: T18 script PASS. | PROVEN |
| MEDIA-04 | T18 H-18-MEDIA: opaque metadata and `DocumentStore`-compatible byte seam. | `shared/content-image-store.ts`, server adapter and route imports, UI source scan, actual EUP; T18 script PASS. | PROVEN |
| UI-MEDIA-01 | T18/T15-A7: Portfolio-only source-derived upload and non-authoritative result. | Source/app form captures at 1440/930/720/485, uploaded card capture, Testimonials/Events no-file assertions; T18 script PASS. | PROVEN |
| UI-MEDIA-02 | T18/T03: accessible idle/error/loading/success/focus and responsive form. | Labeled file and alt controls; alert/status; disabled submit while request runs; keyboard/focus and reduced-motion via T15 harness; T18 four-width no-overflow/captures; both scripts PASS. | PROVEN |
| REG-M1-T15 | Direct T15 regressions: fixed images, Draft/Published, public-draft exclusion, empty/error, proposed result, shared frame. | `node scripts/t15-visual-evidence.mjs` PASS with `T15_BASE_URL=http://localhost:3318` and `T15_EVIDENCE_DIR=C:\Users\ASUS\AppData\Local\Temp\t18-t15-regression-20261122`; `npm run check` PASS. | PROVEN |
| EMPTY-18-STORE | T18 empty/first-use: absent root remains absent until valid upload; no hidden default; unknown key 404. | T18 script removes root, reads unknown key and asserts no files, rejects bad form, then validates first upload and teardown; empty/error/public route observations PASS. | PROVEN |
| SEC-MEDIA-01/02/03 | T18 security controls: validation, exact-key serving, non-disclosure, namespace deletion. | T18 route tests and source scan PASS; stable error codes, `nosniff`, no client fs or arbitrary URL. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

`EUP-18-MEDIA`: `/admin/content` → `AdminContentWorkspace`/`ImageUploadField` (`shared/admin-content-workspace.tsx`) → browser `FormData` POST/PUT → `app/api/fixtures/m1-t15/content-media/{route.ts,[storageKey]/route.ts}` → `parseContentMediaRequest` → `ContentImageStore`/`localContentImageStore` → ignored `.local-storage/m1-t15/content/portfolio/` bytes, metadata and manifest → exact-key GET → Portfolio card/editor preview and reviewer capture. DELETE uses the same namespace; `DELETE` on the collection route removes orphans only. Executed route/browser trace: `scripts/t18-media-evidence.mjs` PASS. The source's `FileReader`/data URL path is absent from the committed component; public/customer imports and output are negative. No mismatch from the frozen expected use path.

## Migration Inventory

No SQL migration or production-data change. The only runtime data is the ignored fixture media directory. The T18 script uses `M1_T18_TEST_STORAGE_ROOT` under a temporary directory, removes it before first-use assertions, creates it through valid upload, and removes it recursively in teardown. No test seed or fixture bytes enter the intended migration graph.

## Actual Empty-State Evidence

Clean temporary root absent: unknown opaque-key GET returned 404 and did not initialize storage; invalid upload produced an accessible error and no files; `/admin/content/empty` remained empty, `/admin/content/error` retained its error state, and public/customer output had no media URL. First valid Portfolio upload created the root, object and manifest; reload resolved the same record pointer and exact-key read. Repeated cleanup left no object and teardown removed the root. `scripts/t18-media-evidence.mjs` PASS.

## Handoff

H-18-MEDIA passes opaque `storageKey`, SHA-256, size, media type, alt text, record association, and `put/read/replace/remove` semantics to M2-W04/M4-W01/M4-W05. H-18-REVIEW passes the exact commit, route/scenario, typed adapter result, four-width captures, public negative and server-only boundary to T17. Future authentication, policy, provider and production delivery remain with those successors.
