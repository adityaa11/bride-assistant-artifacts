# M1-T18 Portfolio media comparison and route evidence

- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Exact code commit: `387dde9aae69fe37795935b798e324e0fa315e3d`
- Reference: `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html`, `openWebsiteEditor()` Portfolio branch, `.web-img-preview`, and T15 source ledger.
- Execution: production build on `http://localhost:3318`, isolated `M1_T18_TEST_STORAGE_ROOT=C:\Users\ASUS\AppData\Local\Temp\t18-go-media-20261122`, reduced motion browser context.

| Width | Source form | Implemented form | Observation |
| --- | --- | --- | --- |
| 1440 | [source](source-portfolio-form-1440.png) | [app](portfolio-form-1440.png) | Source field order, modal geometry, upload control, and empty preview retained. |
| 930 | [source](source-portfolio-form-930.png) | [app](portfolio-form-930.png) | Modal and shared frame contained. |
| 720 | [source](source-portfolio-form-720.png) | [app](portfolio-form-720.png) | Form remains readable and keyboard operable. |
| 485 | [source](source-portfolio-form-485.png) | [app](portfolio-form-485.png) | No horizontal overflow; footer remains in the modal. |

The source's arbitrary HTTPS URL, browser storage, and browser file preview are intentionally absent under T18's exact-key server route. The added 5 MiB help and proposed fixture copy come from T18. Source background starts with empty prototype collections; the implementation background has the T02 fictional Portfolio records and the two checked-in local images. See [loading state](portfolio-upload-loading.png) and [uploaded result](portfolio-upload-1440.png) for disabled submit and the post-upload card/non-authoritative status. No material in-scope form or card geometry mismatch remains.

`node scripts/t18-media-evidence.mjs` PASS (exit 0) on the exact commit: first unknown key returned 404 without creating the root; a bad PNG in the actual Portfolio form gave an accessible error with no bytes written; valid PNG form submission produced a SHA-256/size/type/alt result and a same-origin preview that survived reload. The route accepted JPEG/PNG/WebP signatures, rejected declared type, signature, oversize and Events record scope, and kept original bytes readable after failed PUT. Successful PUT changed the record pointer, removed the old key, and served the new key. DELETE removed the object; explicit cleanup removed orphan bytes/temp files while a sibling sentinel remained intact; teardown removed the entire temporary root. Public, customer, empty and error routes contained no fixture media URL; Testimonials and Events had no file input.

`npm run check` PASS (exit 0): ESLint, TypeScript, 13 tests, production build. `T15_BASE_URL=http://localhost:3318; T15_EVIDENCE_DIR=C:\Users\ASUS\AppData\Local\Temp\t18-t15-regression-20261122; node scripts/t15-visual-evidence.mjs` PASS (exit 0): T15 source/implementation captures at its full boundary set, fixed local images, empty/error, proposed publication, public draft exclusion, keyboard and shared shell on Bookings/Availability/Content/Events at 1440/930/720/485. No `FileReader`, `readAsDataURL`, data-image path, prototype storage, or client filesystem import remains in the T18 component; the only `node:fs` import is the server-local adapter. Existing unrelated customer fixture localStorage was not modified.
