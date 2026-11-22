# CK review — M1-T15 admin content publication fixture workspace

- Ticket: `M1-T15`
- Batch / milestone / work package: `BAP-M1-FIXTURE-UI-SET-01 / 3 / M1-W06`
- Ticket state at review: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `8c40bf2d26fe60a65b14290365317fb9b50d0bbf` (`Add fixture image previews and keep admin sidebar visible`)
- Frozen ticket: [`tickets/m1-fixture-ui-assembly/M1-T15-admin-content-publication-fixtures.md`](../tickets/m1-fixture-ui-assembly/M1-T15-admin-content-publication-fixtures.md)
- Submitted checkpoint: [`checkpoints/M1-T15-admin-content-images-2a14934.md`](../checkpoints/M1-T15-admin-content-images-2a14934.md)
- Review-target clarification: the checkpoint records `2a14934`, but the user explicitly clarified that the intended M1-T15 review commit is `8c40bf2`; this artifact is bound to that clarified committed revision.
- Review type: first consolidated CK review against the frozen M1-T15 ticket and amendments A1–A6.

## Result

`CHANGES_REQUIRED`

The named T15 route, state, public-negative, keyboard, empty/error, visual, and shared-shell checks pass at `8c40bf2`. The commit nevertheless introduces a ticket-forbidden media-upload path: the content forms render a file input and read selected files into data URLs. M1 permits only the two closed-key, checked-in fictional Portfolio images authorized by A6; it does not authorize user upload or media bytes.

## Review Contract status

| Row | Ticket authority and pass condition | Review evidence | Status |
| --- | --- | --- | --- |
| RC-25 | Draft/Published text and visual distinction; T04 public route cannot render a draft; no hidden DOM draft/publication flag. | T15 harness passed the public-negative and proposed-Publish assertions; populated captures show both text labels. | PROVEN |
| CONTENT/H-15 | Portfolio, Testimonials, Events, empty/error, and proposed-result states through typed T02 fixtures with no real publication, persistence, or media authority. | Current routes and scenario catalogue were inspected; named route/state and proposed-result checks passed. The new upload path violates the media boundary. | UNRESOLVED |
| VIS-REF-M1-T15 / SRC-REF-M1-T15 | Source-led structure, controls, assets, interactions, and responsive states with no material mismatch or invented UI. | The committed ledger and visual evidence were inspected. `8c40bf2` adds an unconditional `Foto konten` control to the form; the rendered Event form visibly contains the file picker, which is outside the ticket's allowed fixture-only UI. | UNRESOLVED |
| A4-SHARED | Use `AdminWorkspaceFrame`; no route shell duplication/override; Bookings, Availability, Content, and Events remain regression-free at 1440/930/720/485. | Rerun of `scripts/t15-visual-evidence.mjs` passed no-overflow, shell, drawer, and Escape assertions at all named widths. | PROVEN |
| VIS-003/06/08/10/11 | Entity/settings presentation, readable typography, non-color status, responsive fit, keyboard/focus, and reduced-motion evidence for the mapped states. | Existing visual harness and captures passed. The added form control is the ticket-bound source/boundary mismatch recorded below. | UNRESOLVED |
| SEC-M1-01/02 | Preserve private/public and consent seams; safe rendering; no upload, external media, storage, publication mutation, or official authority. | Static review found `FileReader`, `readAsDataURL`, `input type="file"`, and `FormData` handling in the T15 component. | UNRESOLVED |

## Validation and evidence performed

- `git status --porcelain` at the code repository: clean.
- `npm run check`: PASS — ESLint, TypeScript, 13 repository tests, and production build.
- `$env:T15_BASE_URL='http://localhost:3315'; $env:T15_EVIDENCE_DIR='C:\Users\ASUS\AppData\Local\Temp\bride-assistant-t15-ck-8c40bf2'; node scripts/t15-visual-evidence.mjs`: PASS, exit 0.
- Inspected committed T15 evidence under `evidence/M1-T15-images/`, the source ledger `docs/frontend/M1-T15_REFERENCE_LEDGER.md`, the `8c40bf2` diff, and the rendered populated Portfolio, Event form, and 485px shell captures.
- Direct source evidence: `shared/admin-content-workspace.tsx:17-55` defines the file validation, `FileReader`, data-URL preview, and file input; `shared/admin-content-workspace.tsx:112-114` reads the selected file from `FormData`; `shared/admin-content-workspace.tsx:134` mounts the field for every add/edit content form.
- The source ledger explicitly records that photo upload/URL controls were omitted under the M1 media boundary. The current rendered Event form nevertheless displays `Foto konten` and `Choose File`.

## CK-001 — Unauthorized media-upload path in the T15 forms

### CK-001.a

- Ticket authority: M1-T15 original Review Contract and boundary; Amendment A3 fixture integration; Amendment A5 source-faithful implementation gate; Amendment A6 explicitly authorizes only checked-in fictional local Portfolio images and continues to forbid uploads and media bytes.
- Unsatisfied evidence: `8c40bf2` renders `<input type="file">`, accepts user-selected JPEG/PNG/WebP files, reads them with `FileReader.readAsDataURL`, and includes the file in the proposed form result. The field is mounted unconditionally for Portfolio, Testimonials, and Events. This is observable in `shared/admin-content-workspace.tsx:17-55,112-114,134` and the rendered Event-form capture.
- Observable correction: remove the user-upload/preview path from the T15 component, including the file input, `FileReader`, data-URL handling, and `FormData` image extraction. Keep only the closed T02 `imageAsset`/`imageAlt` fields resolving to the two fixed local Portfolio assets authorized by A6. The resulting add/edit forms must not expose a file control or accept media bytes, while the populated Portfolio cards, empty/error states, proposed result, public-draft negative, and shared shell continue to pass.

#### Binary closure oracle

- Required observable state: no `/admin/content` or `/admin/events` add/edit form contains `input[type="file"]`; the committed T15 route/component path contains no `FileReader`, `readAsDataURL`, or `FormData` image-upload handling; the two fixed local Portfolio images still render with descriptive alt text; no record or publication state changes after the proposed action.
- Exact evidence or validation: source inspection of `shared/admin-content-workspace.tsx`, a Playwright assertion over both content and event forms, and a passing rerun of `npm run check` plus `scripts/t15-visual-evidence.mjs` against the remediation commit. Evidence must include the populated Portfolio, proposed-result, public-negative, form, empty/error, and shared-shell observations.
- Direct-regression boundary: only M1-T15 content/events routes, their named fixture states and interactions, the A6 local-image display, the T04 public-draft boundary, and the A4 shared shell on Bookings/Availability/Content/Events at 1440, 930, 720, and 485 are in scope for this clause.

## Scope-change observations

None. The defect is repairable within the frozen ticket and approved fixture/UI authority; no provider, runtime, deployment, architecture, product, or policy decision is required.

## Decision

`CHANGES_REQUIRED`. The single frozen clause above is the complete bounded remediation target for this first review. This CK artifact does not authorize CFC; subsequent remediation follows the existing HMN/CFC workflow and must preserve the frozen closure oracle.
