# M1-T11 — Render Booking Saya lifecycle, proof, and exception fixture views

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W05`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W05; Project Context customer-visible lifecycle, proof, and exception boundaries.
- Successor/handoff: `H-11` → M3-W05, M4-W02–04, T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; local lifecycle detail (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no bookings is explicit and initializes nothing.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render Booking Saya list/detail, canonical statuses, proof/exception placeholders, filters, no-bookings/loading/error states. Entry is named booking-history scenario ID; endpoint is read-only fixture detail and explicitly non-authoritative action result. H-11 gives successor scenario/version, visible lifecycle/proof/exception fields and no-write result. Use path: route → adapter → list/detail/status composition → customer-visible fixture state → reviewer.

## UI Reference fidelity

Reproduce `project's goal/UI Reference/booking_saya.html` exactly. Match the visible list/detail structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend loading, empty, error, proof, and exception states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Review Contract, invariants, security/UI, validation

Only the owner-shaped fixture view appears; no file bytes/upload, status mutation, cancellation/reschedule submission, or cross-customer data. T02 scenario contract is shared; list/detail remains local. Security preserves owner/upload seams and safe rendering. UI: Knowledge List + workflow detail, VIS-003/06/08/09/10/11. `RC-21`: PASS iff each canonical state, no-booking, proof/exception placeholder and error is inspectable with no official change; matrix capture and negative mutation test; fixture upload/transition forbidden. Hard stop: rejection label remains undefined and is omitted.
