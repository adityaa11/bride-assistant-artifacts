# M1-T10 — Render the customer booking-request fixture workflow

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W05`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W05; Project Context request lifecycle and availability boundary.
- Successor/handoff: `H-10` → M3-W05 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; local booking workflow/availability presentation (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no package/available date has explicit no-action state.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render package/date/detail workflow with fixture form validation, availability-only calendar, canonical `Menunggu Konfirmasi` result, and non-authoritative submit result. Entry is declared booking scenario ID; endpoint is reviewer-visible workflow state, never a record. H-10 transfers scenario ID/version, selected display fields, result state, and M3 successor; no retry, persistence, or idempotency claim. Use path: route → adapter → workflow/form primitives → result state → reviewer.

## Review Contract, invariants, security/UI, validation

Calendar must not disclose another customer or promise availability. No booking write, date blocking, WhatsApp activation, package snapshot, or client authority. Security preserves ownership/transaction seam and renders safely. UI: Workflow Sequence, semantic validation/status, responsive two-column-to-one-column, VIS-003/06/08/09/10/11. `RC-20`: PASS iff required/invalid/selected/unavailable/no-package/success-result scenarios render through T02 and success remains non-authoritative; state capture + negative data assertion; localStorage write/official transition forbidden. Hard stop: real submission is M3.
