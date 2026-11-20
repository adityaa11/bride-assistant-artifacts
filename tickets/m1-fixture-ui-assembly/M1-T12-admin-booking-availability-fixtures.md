# M1-T12 — Render admin booking and availability fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context admin authority and canonical lifecycle/availability rule.
- Successor/handoff: `H-12` → M3-W02/M3-W06 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; admin operational calendar is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no booking/date results are explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render admin booking list/detail, operational calendar, availability view, canonical status filters and no-result states. Entry is named admin-booking scenario ID; endpoint is a Data Workspace fixture view whose apparent controls return non-authoritative results. H-12 provides scenario/version and admin read/action-display fields to M3; no identity/record write crosses this ticket. Use path: admin route → adapter → admin calendar/list/detail → reviewer.

## Review Contract, invariants, security/UI, validation

No admin login/role decision, availability mutation, status command, customer disclosure outside this admin fixture route, or `Confirmed`/`On Hold`. Security preserves server admin/transaction seams; safe rendering now. UI: Data Workspace, desktop sidebar/narrow drawer, VIS-003/06/08/10/11. `RC-22`: PASS iff populated/empty/filter/calendar states show canonical labels and non-authoritative controls; route capture + token scan; client role gate or mutation forbidden. Hard stop: operational activation is M3.
