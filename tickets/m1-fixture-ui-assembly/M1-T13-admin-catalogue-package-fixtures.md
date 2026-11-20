# M1-T13 — Render admin catalogue, bundle, package, and price fixture views

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context catalogue/package/price hierarchy.
- Successor/handoff: `H-13` → M3-W02 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; catalogue hierarchy is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: empty catalogue has no generated default.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render services/actions, bundles, master packages, prices, search/filter, empty/error and proposed edit-result fixtures. Entry is named catalogue scenario ID; endpoint is an Entity Library/Settings Form review surface with no mutation. H-13 transfers versioned catalogue display/action-result fields to M3; fixture reads are idempotent. Use path: admin route → adapter → catalogue composition → visible fixture state → reviewer.

## Review Contract, invariants, security/UI, validation

Do not CRUD, persist prices, calculate authoritative promotions, or claim historic booking behavior. Security preserves future admin authorization and safe rendering. UI: Entity Library + Settings Form, long package names/composition cases, VIS-003/06/08/10/11. `RC-23`: PASS iff every catalogue hierarchy/price empty-error-proposed-edit scenario is routed through T02 with no mutation; capture plus import/mutation scan; prototype seed/state save forbidden. Hard stop: authoritative catalogue M3.
