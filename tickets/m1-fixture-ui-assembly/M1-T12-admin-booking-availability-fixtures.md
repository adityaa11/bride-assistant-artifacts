# M1-T12 — Render admin booking and availability fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context admin authority and canonical lifecycle/availability rule.
- Successor/handoff: `H-12` → M3-W02/M3-W06 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; admin operational calendar is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no booking/date results are explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render admin booking list/detail, operational calendar, availability view, canonical status filters and no-result states. Entry is named admin-booking scenario ID; endpoint is a Data Workspace fixture view whose apparent controls return non-authoritative results. H-12 provides scenario/version and admin read/action-display fields to M3; no identity/record write crosses this ticket. Use path: admin route → adapter → admin calendar/list/detail → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin booking and availability composition in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend empty, filter, calendar, and non-authoritative result states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Review Contract, invariants, security/UI, validation

No admin login/role decision, availability mutation, status command, customer disclosure outside this admin fixture route, or `Confirmed`/`On Hold`. Security preserves server admin/transaction seams; safe rendering now. UI: Data Workspace, desktop sidebar/narrow drawer, VIS-003/06/08/10/11. `RC-22`: PASS iff populated/empty/filter/calendar states show canonical labels and non-authoritative controls; route capture + token scan; client role gate or mutation forbidden. Hard stop: operational activation is M3.
