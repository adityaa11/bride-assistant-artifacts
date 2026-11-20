# M1-T15 — Render admin content-publication fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context published-only and permission boundary.
- Successor/handoff: `H-15` → M4-W05/M4-W06 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; publication review is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: empty portfolio/testimonial/event collections are explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render fixture portfolio, testimonials, events, draft/published distinction, and proposed publish-result states. Entry is named content scenario ID; endpoint is a review workspace with no media upload, persistence, or public publication. H-15 provides scenario/version, publication display state, and M4 successor mapping. Use path: admin route → adapter → content review surface → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin content/publication composition in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend draft/published, empty, and proposed-result states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Amendment M1-T15-A1 — mapped reusable reference refactor

Use the T15 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: refactor the admin reference `Portfolio & Testimonials` and `Events` compositions and their 1250px/930px/485px transformations into reusable typed Next.js workspace components, using only the corresponding locally extracted static assets and T02 fixtures. Reference behaviour, media delivery, and publication authority remain forbidden.

## Review Contract, invariants, security/UI, validation

No bytes/media origin, consent claim, publish mutation, or draft public output. Security preserves private/public delivery and consent seams; safe rendering only. UI: Entity Library/Settings Form and state semantics, VIS-003/06/08/10/11. `RC-25`: PASS iff draft and published are visually/non-color distinguishable and public T04 cannot render a draft fixture; cross-route negative test + captures; hidden DOM draft or client publication flag forbidden. Hard stop: M4 owns real publication/media policy.
