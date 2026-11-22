# M1-T15 A6 — local fictional portfolio fixture imagery

- Ticket: `M1-T15`, amendment A6; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target code commit: `2a1493413b083a93585b3af43250967a7f2e1c01` (`feat: add local portfolio fixture imagery`).
- Prior checkpoint [M1-T15 content publication](M1-T15-admin-content-publication-2e4673e.md) remains historical at commit `2e4673e3e26e31099ef9b1a4b18bb7dfe671b4eb`; this checkpoint supersedes it as the current review target after explicit user correction.
- Evidence: [A6 comparison and captures](../evidence/M1-T15-images/comparison.md); code ledger `docs/frontend/M1-T15_REFERENCE_LEDGER.md` at the review commit.

## Bounded change

Two generated object-only PNG still lifes are checked into `public/fixture-assets/m1-t15/` for the Draft and Published Portfolio records. The T02 `adminContent.portfolio` type carries closed image asset keys and alt text. `ContentCard` maps keys to fixed local paths through `next/image` and the reference's `.web-media img` crop. The source image region, card order, statuses, empty states, proposed-result flow, shared frame, event symbols and testimonial cards remain as previously reviewed. No upload, remote origin, real customer image, consent assertion, public delivery, publication state change, or persistence was added.

## Review Contract Closure

| Row | Ticket authority and pass condition | Required proof; evidence / exact command and outcome | Status |
| --- | --- | --- | --- |
| A6-IMAGE | Explicit user correction in A6: actual fictional local imagery in populated Portfolio, typed fixed asset keys, descriptive alt, source crop; no other media authority. | Two files and T02 fields; `app-portfolio-populated.png`, `shell-content-485.png`; browser `naturalWidth`, alt and local URL assertions. `$env:T15_EVIDENCE_DIR='.atlas/evidence/M1-T15-images'; node scripts/t15-visual-evidence.mjs` PASS, exit 0. | PROVEN |
| RC-25 | Draft/Published distinction and no public draft output remain intact. | Populated captures; `app-public-negative.png`; proposed Publish unchanged Draft assertion in visual harness PASS. | PROVEN |
| VIS-REF/SRC-REF | Source card geometry, mapped image region and responsive treatment; paired captures and source trace; no material mismatch. | Code ledger, A6 comparison, 33 new source/app pairs, `measurements.json`; visual harness PASS. | PROVEN |
| A4-SHARED | Shared frame and Bookings/Availability regressions at desktop, 930, 720, 485. | 16 `shell-*` captures, no-overflow and drawer assertions; visual harness PASS. | PROVEN |
| SEC-M1-01/02 | No upload, arbitrary URL, external origin, unsafe rendering, consent or publication authority. | Closed typed key mapping, `next/image` local paths, no-match source scan, public negative and proposed-result assertions; `npm run check` PASS. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

`/admin/content` → server `getScenario("admin.content-draft-success")` → typed `imageAsset`/`imageAlt` on two Portfolio records → `AdminContentWorkspace` / `ContentCard` → fixed local image map → `next/image` in source `.web-media` region → reviewer. The production browser harness loaded both images and captured desktop/mobile cards. The superseded icon-only Portfolio path is unreachable for these populated records; empty and non-Portfolio source treatments remain intact. The proposed action path still uses `admin.content-proposed-result` and changes no record.

## Actual Empty-State Evidence

The `admin.content-empty` scenario retains zero Portfolio, Testimonial and Event records, and the empty routes remain identical. The regenerated source/app pairs show the same source empty panel height at desktop and no mobile overflow. No image is introduced by a hidden default or seed.

## Migration Inventory

No SQL migration, production-data change, seed or test-only database fixture. Two static image files are repository assets. The browser harness creates isolated contexts and closes them.
