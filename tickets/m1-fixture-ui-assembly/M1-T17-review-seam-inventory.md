# M1-T17 — Verify active M1 surfaces and publish the replacement-seam inventory

- State: `ready`; batch/milestone/work package: `4 / M1 / M1-W07`; dependencies: T04/H-04 and T08/H-08 through T16/H-16.
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
