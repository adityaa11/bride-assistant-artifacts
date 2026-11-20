# M1-T13 — Render admin catalogue, bundle, package, and price fixture views

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context catalogue/package/price hierarchy.
- Successor/handoff: `H-13` → M3-W02 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; catalogue hierarchy is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: empty catalogue has no generated default.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render services/actions, bundles, master packages, prices, search/filter, empty/error and proposed edit-result fixtures. Entry is named catalogue scenario ID; endpoint is an Entity Library/Settings Form review surface with no mutation. H-13 transfers versioned catalogue display/action-result fields to M3; fixture reads are idempotent. Use path: admin route → adapter → catalogue composition → visible fixture state → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin catalogue/package composition in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend empty, error, long-content, and proposed-result states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Amendment M1-T13-A1 — mapped reusable reference refactor

Use the T13 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: refactor the admin reference `Services & Actions` and `Master Packages` compositions and their 1250px/930px/485px transformations into reusable typed Next.js workspace components, using only the corresponding locally extracted static assets and T02 fixtures. Reference behaviour and catalogue authority remain forbidden.

## Review Contract, invariants, security/UI, validation

Do not CRUD, persist prices, calculate authoritative promotions, or claim historic booking behavior. Security preserves future admin authorization and safe rendering. UI: Entity Library + Settings Form, long package names/composition cases, VIS-003/06/08/10/11. `RC-23`: PASS iff every catalogue hierarchy/price empty-error-proposed-edit scenario is routed through T02 with no mutation; capture plus import/mutation scan; prototype seed/state save forbidden. Hard stop: authoritative catalogue M3.
