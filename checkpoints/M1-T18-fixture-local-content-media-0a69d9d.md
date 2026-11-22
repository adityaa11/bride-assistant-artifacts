# M1-T18 — expanded fixture media GO checkpoint

- Ticket: `M1-T18`; batch `BAP-M1-FIXTURE-UI-SET-01 / 4`; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact review-target code commit: `0a69d9da1ac204cb23a2bcbccc1fba4171844b92`.
- Authority: [frozen T18 with A1/A2/A3](../tickets/m1-fixture-ui-assembly/M1-T18-fixture-local-content-media.md); T02/H-02 and T03/H-03 PASS. T15's historical CK result is not promoted. [Portfolio-only checkpoint](M1-T18-fixture-local-content-media-387dde9.md) is immutable historical evidence, superseded as review target by A2/A3.
- Evidence: [expanded source/route comparison](../evidence/M1-T18-A2/comparison.md), 57 captures there, 92 [T15 regression captures](../evidence/M1-T15-A2-regression/), and committed scripts `scripts/t18-media-evidence.mjs`, `scripts/t18-document-contract.mjs`, `scripts/t15-visual-evidence.mjs`.

## Bounded implementation

Portfolio, Testimonials, and Events add/edit forms now accept one JPEG/PNG/WebP up to 5 MiB with bounded alt text. Closed T02 record IDs derive the collection; add-form IDs remain proposed results only. POST/PUT, exact-key GET and record-scoped DELETE use the same fixture handler. `localContentImageStore` is the record association/lifecycle facade; the single injected `DocumentStore` local implementation owns all filesystem bytes and association files in the three ignored namespaces. The UI sees only opaque metadata and same-origin URLs. Baseline Portfolio images remain checked in; Testimonials/Events have no default image. No production media, identity, consent, publication, provider, SQL or public delivery is activated.

## Review Contract Closure

| Row | Ticket authority and pass condition | Required proof, evidence locator and exact validation outcome | Status |
| --- | --- | --- | --- |
| MEDIA-01 | T18/A2 validated opaque upload under owned namespace; original Portfolio requirement retained. | Actual three form POSTs, PNG/JPEG/WebP, type/signature/size/filename/scope negatives, hash/size/alt and isolated inventory: `node scripts/t18-media-evidence.mjs` PASS; [comparison](../evidence/M1-T18-A2/comparison.md). | PROVEN |
| MEDIA-02 | T18/A2 exact-key same-origin read with no listing, path disclosure or public/customer output. | GET byte/type/size, unknown/malformed key, absent root and public/customer negatives: T18 script PASS; comparison. | PROVEN |
| MEDIA-03 | T18/A2 atomic replace, failure preservation and bounded cleanup. | Every collection invalid/valid PUT, old-key 404, scoped DELETE, orphan cleanup, referenced sibling and outside sentinel preservation, teardown: T18 script PASS. | PROVEN |
| MEDIA-04 | H-18 opaque metadata and replaceable `DocumentStore` handoff. | `shared/document-store.ts`, facade and adapter import scan, actual UI-to-route trace: T18 script PASS; `node scripts/t18-document-contract.mjs` PASS. | PROVEN |
| MEDIA-05 | A2 Testimonials/Events actual form upload and separate association. | Both new add/edit UI callers, three namespaces, reload, typed metadata, JPEG/PNG/WebP and invalid scope/alt/type/size/signature: T18 script PASS; captures. | PROVEN |
| MEDIA-06 | A2 collection-isolated read/replace/delete/cleanup, failure preservation and no public/customer output. | Three collection GET/PUT/DELETE, cross-collection 404, referenced sibling survival and empty-root/teardown inventory: T18 script PASS. | PROVEN |
| MEDIA-07 | A3 one local `DocumentStore` byte implementation for all uploads and substitutable put/read contract. | Three actual browser EUPs reach handler → facade → local `DocumentStore`; only that adapter imports `node:fs/promises`; `scripts/t18-document-contract.mjs` in-memory substitution and injected write failure PASS. | PROVEN |
| UI-MEDIA-01-A2 | A2 three authorized add/edit controls, card/preview treatment, source fields and proposed-only add. Supersedes Portfolio-only `UI-MEDIA-01` exclusion. | Paired source/app form captures and fallback/uploaded cards at 1440/930/720/485; add proposed result without card: T18 script PASS; comparison. | PROVEN |
| UI-MEDIA-02-A2 | A2 extends T03 idle/loading/success/error/disabled/focus/keyboard/reduced-motion/no-overflow obligation to new callers. | Labeled controls/status/alert, focused file, delayed POST/disabled submit, invalid UI, reduced-motion context and four-width no-overflow: T18 script PASS; T15 harness PASS. | PROVEN |
| EMPTY-18-STORE / EMPTY-18-A2 | T18/A2 absent namespaces stay absent through unknown/empty/error reads; valid upload alone initializes. | Initial root removed, unknown key 404, six route observations, first form upload and teardown: T18 script PASS. | PROVEN |
| REG-M1-T15 | T15 fixed Portfolio images, Draft/Published, public draft exclusion, proposed result, empty/error and shared shell retained. | `node scripts/t15-visual-evidence.mjs` with stated base/evidence env PASS; 92 captures; `npm run check` PASS (13 Node tests/build). | PROVEN |
| SEC-MEDIA-01/02/03 | T18/A2 closed validation, exact serving, stable errors, namespace integrity and public/customer boundary. | T18 negative routes/source scan and response assertions PASS; only local adapter owns filesystem, no browser path/provider import. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

- `EUP-18-MEDIA`: `/admin/content` Portfolio add/edit form → `AdminContentWorkspace`/`ImageUploadField` → browser `FormData` → `app/api/fixtures/m1-t15/content-media/{route.ts,[storageKey]/route.ts}` → `parseContentMediaRequest` → `localContentImageStore` → local `DocumentStore.put/read` → ignored `portfolio/` bytes/association → exact-key GET → Portfolio editor/card/proposed result. Executed in T18 browser/route script; no mismatch.
- `EUP-18-TESTIMONIAL`: `/admin/content` Testimonials tab → the same form/handler/facade/DocumentStore path → ignored `testimonials/` → exact-key GET → image above quote or proposed result. The quote-only fallback remains. Executed in T18 script; no mismatch.
- `EUP-18-EVENT`: `/admin/events` → the same form/handler/facade/DocumentStore path → ignored `events/` → exact-key GET → image replacing the theme symbol or proposed result. The symbol fallback remains. Executed in T18 script; no mismatch.

PUT and record-scoped DELETE traverse the same facade and fixed namespace. Collection DELETE performs orphan cleanup. All three paths return named non-authoritative fixture results. `FileReader`/data URL and public/customer media callers are absent by committed source scan and route negative observations.

## Migration Inventory

No SQL migration, production-data change or durable seed. The test uses ignored `.local-storage/t18-evidence/` as an isolated root, removes it before first-use assertions and after the browser/route run. Runtime `.local-storage/m1-t15/content/{portfolio,testimonials,events}/` is disposable fixture data and is not migrated to M2/M4. The two checked-in Portfolio images are baseline assets, not uploaded bytes.

## Actual Empty-State Evidence

With all three test namespaces absent, exact-key unknown GET returned 404 without creating them. `/admin/content/empty`, `/admin/content/error`, `/admin/events/empty`, `/admin/events/error`, `/` and `/customer` did not initialize or render fixture media. The first valid upload for each collection created only its own namespace and manifest; reload resolved the same opaque pointer. Cleanup preserved a referenced event image while deleting three-namespace orphans; teardown removed the entire test root. T18 script PASS.

## Handoff

H-18-MEDIA carries the opaque metadata, closed record association and single `DocumentStore` put/read seam to M2-W04/M4-W01/M4-W05. H-18-REVIEW gives T17 the exact commit, three route traces, four-width source/UI captures, isolated lifecycle inventory, public/customer negatives, T15 regression, and `MEDIA-07` substitution evidence. M4-W06 alone governs later public reads.
