# M1-T16 — Render admin promotion fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context promotion representation and catalogue boundary.
- Successor/handoff: `H-16` → M3-W02/M4-W05 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; promotion rule presentation is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no promotion is explicit without a default.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render promotion list/detail/form fixture states for applicable packages, percentage/rupiah representation, active period, validation/error, and proposed-result display. Entry is named promotion scenario ID; endpoint is a non-authoritative Settings Form review surface. H-16 transfers versioned promotion display/action-result fields to successors, never a price calculation authority. Use path: admin route → adapter → promotion composition → reviewer.

## Review Contract, invariants, security/UI, validation

No promotion CRUD, effective-price calculation, booking snapshot mutation, or date-policy decision. Security preserves admin authorization and server validation seam; safe rendering now. UI: Settings Form, clear currency/percentage labels and responsive long package selections, VIS-003/06/08/09/10/11. `RC-26`: PASS iff all specified fixture states render with explicit non-authoritative result and no package price is mutated; capture + mutation scan; client calculation/seed save forbidden. Hard stop: authoritative promotion logic belongs to M3/M4.
