# M1-T14 — Render admin customer fixture list and detail views

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context separate admin/customer access boundary.
- Successor/handoff: `H-14` → M2-W02/M3-W02 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; customer operational information is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no customers has an explicit empty view.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render fixture customer list/search/detail only. Entry is named admin-customer scenario ID; endpoint is a reviewer-visible read-only Data Workspace surface with no admin mutation. H-14 carries scenario/version and display-safe customer fields for later protected reads; it is not a permission contract. Use path: admin route → adapter → local list/detail → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin customer workspace in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend empty, search, detail, and error states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Review Contract, invariants, security/UI, validation

No real customer lookup, export, edit, customer-to-public leak, or role enforcement in client. Security preserves server role/record-ownership seam and prohibits shared client store; safe rendering is implement-now. UI: Data Workspace and empty/search states, VIS-002/06/08/10/11. `RC-24`: PASS iff fixture data remains route-local, list/detail/empty/error states render, and public/customer routes cannot import the model; boundary test + captures; CSS-hidden private data forbidden. Hard stop: protected data access is M2/M3.
