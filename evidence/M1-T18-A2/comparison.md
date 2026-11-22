# M1-T18-A2/A3 expanded fixture media evidence

- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact code commit: `0a69d9da1ac204cb23a2bcbccc1fba4171844b92`.
- Source: `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` at that commit; `openWebsiteEditor()`, `renderStories()`, `renderWebsiteEvents()`, `.web-media img`.
- Runtime: Next.js 16.4.6 development route at `http://localhost:3318`, isolated `.next-t18` build directory and ignored `.local-storage/t18-evidence/portfolio` test root. The test removed the entire parent root on teardown.

## Source and UI comparison

At each width 1440, 930, 720 and 485, paired files `source-{portfolio|testimonials|events}-form-{width}.png` and `{portfolio|testimonials|events}-form-{width}.png` show the mapped source fields, modal structure and shared frame. The upload field is the bounded A2 extension for Testimonials and Events. Files `{collection}-fallback-{width}.png` and `{collection}-uploaded-{width}.png` show no hidden image when absent, and the authorized `.web-media img` crop when associated: testimonial photo above quote, event photo in place of the theme symbol, Portfolio upload over its checked-in baseline. The browser asserted no horizontal overflow at all four widths. `{collection}-loading-1440.png`, `{collection}-error-1440.png`, and `{collection}-proposed-add-1440.png` show disabled submit, accessible error, and an add-form image in the proposed result without a new card.

The reference starts with empty prototype arrays; application populated cards use fictional T02 fixtures. The source's browser storage, arbitrary HTTPS URL and data-URL preview are absent. A2 authorizes only the added media field and card crop for the two panels; source copy, other fields, controls and publication badges remain.

## Route and storage observations

`M1_T18_TEST_STORAGE_ROOT=C:\Workspace\bride-assistant\.local-storage\t18-evidence\portfolio; T18_EVIDENCE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T18-A2; node scripts/t18-media-evidence.mjs` — **PASS, exit 0**. The script begins with absent three namespaces. Unknown and malformed exact keys return 404/400 and do not initialize them; empty/error/public/customer routes likewise leave them absent. Each actual form submits PNG to POST, returns opaque key/SHA-256/size/type/alt, writes `key.bin`, `key.json` and `records.json` only in its collection, reads exact bytes through same-origin GET, and shows the card after reload. It also submits a new image through each add form and checks proposed preview without card creation. JPEG and WebP route cases pass; bad signature, declared type, oversize, empty alt, forged record ID and filename traversal are rejected.

For every collection, invalid PUT leaves the old key, bytes and pointer readable. Cross-collection PUT and DELETE return 404; valid PUT changes the pointer and removes the old key. Correct record-scoped DELETE removes it. Orphan bytes/temp/association files in the three namespaces are removed by the collection cleanup route while a still-referenced event image and an outside sentinel survive. The script removes all temporary files/directories at teardown. Public/customer HTML contains no fixture-media URL. Source scan finds no `FileReader`, `readAsDataURL`, data image, client filesystem, localStorage or arbitrary external URL in the component; only the local `DocumentStore` adapter imports `node:fs/promises`.

`node scripts/t18-document-contract.mjs` — **PASS, exit 0**. An in-memory alternate `DocumentStore` performs byte `put/read` and fixture association operations for all three collections. The same facade returns identical UI metadata keys, preserves a readable pointer after an injected byte-write failure, replaces, and removes without changing any UI contract or adding provider code.

`npm run check` — **PASS, exit 0**: ESLint, TypeScript, 13 Node tests and Next production build. `T15_BASE_URL=http://localhost:3318; T15_EVIDENCE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T15-A2-regression; node scripts/t15-visual-evidence.mjs` — **PASS, exit 0**: 33 paired empty source/app panels, fixed Portfolio images, Draft/Published, public-draft exclusion, proposed result, error, keyboard, and Bookings/Availability/Content/Events shared shell at the four named widths. The 92 generated files are in the sibling evidence directory.

## H-18 to T17 and successors

T17/H-17 should review this code commit and new checkpoint for `MEDIA-01`–`MEDIA-07`, `UI-MEDIA-01-A2`, `UI-MEDIA-02-A2`, `EMPTY-18-A2`, direct T15 regressions and public/customer negatives. The older Portfolio-only checkpoint at `387dde9` is historical. M2-W04 consumes the single `DocumentStore` byte contract; M4-W01 owns its Neon Object Storage S3-compatible production adapter; M4-W05 owns durable content association/permission; M4-W06 governs public reads. None is activated here.
