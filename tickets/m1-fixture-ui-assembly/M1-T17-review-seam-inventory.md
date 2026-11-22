# M1-T17 — Verify active M1 surfaces and publish the replacement-seam inventory

- State: `approved`; batch/milestone/work package: `4 / M1 / M1-W07`; [T17-A7 GO checkpoint](../../checkpoints/M1-T17-review-seam-inventory-79b3478.md) at `79b3478995737dd73165afaba50369dcb5242a08`; [CK review](../../feedback/M1-T17-review-seam-inventory-79b3478-ck.md) recorded `PASS`. Dependencies: T04/H-04, T08/H-08 through T14/H-14, T16/H-16, plus T18/H-18. The post-v1 re-review also required T19/H-19, T20/H-20 and T21/H-21 with CK `PASS` at their exact checkpoints; T21 followed T20, while T19 could run independently. T15's media continuation is consumed through H-18; its historical CK result is not promoted.
- Source authority: M1-W07; M1 exit gate and cross-phase verification M1.
- Successor/handoff: `H-17` → M2 planning. Activation: `active-in-this-ticket` for the M1 review gate only. Reuse: `keep-local` review evidence. Legacy: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Run the route/scenario review matrix and publish the fixture-to-backend seam inventory. Entry is the M1 review checklist consuming each active ticket's route, scenario ID/version and evidence. Endpoint is a versioned pass/fail evidence record and successor map. H-17 includes ticket/route/scenario, adapter version, visual/accessibility/security evidence, finding, and exact M2+ consumer. Use path: review entry → actual route render → evidence/finding → M2 planner.

## Review Contract, invariants, security/UI, validation

Review actual routes, not prototype HTML or isolated unit tests; fail any direct fixture feature import, unsafe sink, authority claim, public draft, canonical-label regression, missing responsive/accessibility evidence, or visible mismatch against `project's goal/UI Reference/`. No production acceptance or backend approval. Security verifies SEC-M1-01/02; UI verifies relevant VIS-001–014 plus VIS-REF-01. `RC-27`: PASS iff every active ticket has desktop/narrow, applicable loading/empty/error/result capture, reference-viewport comparison with no visible mismatch, and named successor seam; matrix + import scan + visual comparison record; build-only, functionally equivalent, or “inspired by” evidence is forbidden. Hard stop: any failed row leaves M1 not-ready.

## Amendment M1-T17-A1 — verify source-led refactor

- Decision authority: user direction on 2026-11-21; binding source contract `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at commit `5b45033ba06cce63962e0e7bd0171086806d281e` and T08–T16 A3 amendments.
- Extend `RC-27`: for every T08–T16 mapped page/panel, inspect the source-to-component map and actual route. Confirm reference-derived markup regions, CSS/media behavior, local asset roles, and visible interactions were converted into Next.js components and wired to typed T02 fixtures. Compare the rendered route with the source at the same viewports and required states; no material mismatch may remain.
- Reject an invented replacement design, omitted reference region/interaction without a documented ticket reason, raw prototype HTML/script execution, unsafe sink, storage/seed/write authority, or an isolated component/build check as substitute for the actual route. Keep all existing privacy, publication, authentication and successor gates.

## Amendment M1-T17-A2 — shared layout regression gate

- Decision authority: user direction on 2026-11-22. Source binding: code commit `a1ee30e493e5fab3452095db5f02450a573c3d47`.
- Verify that T13–T16 use `AdminWorkspaceFrame` from `shared/admin-workspace-frame.tsx` and keep route-specific content inside it. Reject copied or overridden sidebar, navigation, top bar, title region, mobile drawer, collapse behavior, or outer responsive width.
- Compare Bookings, Availability, and every new admin route at desktop, 930px, 720px, and 485px after any shared-shell change. Record active navigation, full-width desktop fit, no horizontal overflow, and mobile drawer behavior. A missing shared-layout check fails the M1 review row; all prior visual, fixture, accessibility, and security gates remain binding.

## Amendment M1-T17-A3 — source-trace acceptance gate

- Decision authority: user direction on 2026-11-22. Binding contracts: `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` and `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at code commit `ef7a716897ec04e29f73d4dc01ff656d2140bf32`.
- For T13–T16, audit every visible route-specific decision against its mapped HTML, CSS, assets, and interaction source; audit shared shell decisions against `AdminWorkspaceFrame` and displayed data against T02 fixture fields. Require the ledger and source-to-component map to identify those exact links.
- Fail the review for invented or approximate UI, generic replacements, untraced copy or styling, inferred breakpoints or fixture-only states, unresolved source gaps, or material same-viewport differences. An unsupported UI portion remains blocked until explicit user direction is recorded. Preserve A1/A2 and every existing fixture, accessibility, security, and authority gate.

## Amendment M1-T17-A4 — T18 fixture-local media seam review

- Decision authority: explicit user planning direction on 2026-11-22; binding ticket [M1-T18](M1-T18-fixture-local-content-media.md) and T15-A7.
- Extend `RC-27` to consume H-18: verify the actual Portfolio upload → fixture handler → local adapter → same-origin read path, isolated replace/cleanup evidence, no public/customer media output, T18 source/UI evidence, the direct T15 publication/media regressions, and the M2/M4 `DocumentStore` successor mapping. T18's local fixture directory is an M1 seam, not production acceptance.

## Amendment M1-T17-A5 — review the expanded H-18 media paths

- Explicit user direction on 2026-11-22 and [M1-T18-A2](M1-T18-fixture-local-content-media.md#amendment-m1-t18-a2--fixture-image-upload-for-testimonials-and-events) expand the H-18 review binding from Portfolio to Portfolio, Testimonials at `/admin/content`, and Events at `/admin/events`. Planning source is `https://github.com/adityaa11/bride-assistant.git` at code commit `387dde9aae69fe37795935b798e324e0fa315e3d`. This supersedes only A4's Portfolio-only review phrasing; its public-negative, T15 regression, source/UI, storage-seam and M2/M4 successor checks remain active.
- T17's `RC-27` must inspect T18's **new** CK-reviewed checkpoint against `MEDIA-05`, `MEDIA-06`, `UI-MEDIA-01-A2`, `UI-MEDIA-02-A2`, and `EMPTY-18-A2`, including separate collection namespaces, actual UI-to-handler-to-read paths, cross-collection failure, cleanup isolation, fixture-only previews, and source/implementation evidence. The historical Portfolio-only T18 checkpoint is not sufficient for this expanded handoff; T17 does not issue T18's CK result.

## Amendment M1-T17-A6 — verify the universal media byte seam

- Explicit user direction and [M1-T18-A3](M1-T18-fixture-local-content-media.md#amendment-m1-t18-a3--one-documentstore-byte-boundary-for-every-upload) add `MEDIA-07` to H-18 review. T17 checks that Portfolio, Testimonials and Events uploads use one local `DocumentStore` byte implementation and that M2-W04/M4-W01 can replace it with the Neon Object Storage S3-compatible production adapter without changing UI opaque metadata. Content lifecycle association may wrap the byte interface but cannot become a second byte writer. This extends only the H-18 successor-seam check; no production provider is activated in M1.

## Amendment M1-T17-A7 — reconcile fixture audit and regression repairs before re-review

- Decision authority: explicit user direction on 2026-11-22 to make T19–T21 dependencies and their acceptance useful to T17. This amendment applies to a re-review after the [v1 failed evidence](../../evidence/M1-T17-v1.md); it does not rewrite that record or authorize implementation in T17. Source binding for this planning amendment: code repository `https://github.com/adityaa11/bride-assistant.git` at `2ee952c9d6230849ff70e2db62efcc76454ecad9`.
- Re-review entry requires CK `PASS` and exact code/evidence checkpoints for [T19/H-19](../m1-fixture-relationship-audit/M1-T19-fixture-relationship-audit.md), [T20/H-20](../m1-fixture-review-repairs/M1-T20-booking-browser-store-regression.md), and [T21/H-21](../m1-fixture-review-repairs/M1-T21-browser-selector-regression.md). T19 is parallel-safe with T20; T21 remains sequenced after T20. A T19 candidate requiring an M1 correction must have a separately authorized, CK-approved repair before H-17; a finding explicitly deferred under accepted M2+ authority needs a named successor and preserved seam. An unknown or unapproved disposition keeps T17 not ready.
- Extend `RC-27` with `RC-27-A7`: PASS iff the new T17 evidence binds all three handoffs to the inspected code commit; independently rechecks F-01 browser no-write/no-new-record and F-02 selected-date/toast suite results; maps every H-19 finding ID to `M1-required and closed`, `accepted M2+ deferral`, or `unresolved/FAIL` with authority, evidence and successor; and reruns the full active-route, scenario, responsive, source/reference, accessibility, security and H-18 seam review at the resulting code checkpoint. A passing T20/T21 test or a T19 candidate list alone is not H-17. Any remaining M1-required, unknown or failed row blocks H-17 and M1 exit.
