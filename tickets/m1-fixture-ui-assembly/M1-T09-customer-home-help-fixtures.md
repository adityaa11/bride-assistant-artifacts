# M1-T09 — Render customer home and help fixture surfaces

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W05`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W05; Project Context customer navigation and deferred official WhatsApp handoff.
- Successor/handoff: `H-09` → M2-W02, M3-W05, T17. Activation: `active-in-this-ticket` fixture routes. Reuse: T02/T03; local customer navigation/help meaning (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no upcoming booking/help history is explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render customer home summary/navigation and help/WhatsApp-unavailable scenarios only. Entry is named customer-home/help scenario ID; endpoint is a responsive, accessible information/next-action view with no live handoff. H-09 contains scenario ID/version and fixture-only summary/help-result fields for M2/M3; no customer data crosses to public or admin. Use path: customer route → adapter → local home/help composition → visible empty/available/unavailable state → reviewer.

## UI Reference fidelity

Reproduce `project's goal/UI Reference/home.html` and `project's goal/UI Reference/bantuan.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend empty and WhatsApp-unavailable states using the same referenced treatment without inventing a new visual language. Capture both routes at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Review Contract, invariants, security/UI, validation

Do not build booking form/list, live WhatsApp URL, notification backend, session, or private-data lookup. T03 shell is reused; navigation remains local because customer meaning differs from admin. Security: preserve future ownership seam; safe rendering now. UI: Knowledge List/empty-state pattern, VIS-002/06/08/10/11. `RC-19`: PASS iff home/help states, including empty and WhatsApp-unavailable, are scenario-backed and no URL is enabled without D-04; route captures plus link assertion; blank/guessed number forbidden. Hard stop: live communication policy remains deferred.
