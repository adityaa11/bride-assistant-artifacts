# M1-T08 — Render registration and sign-in fixture states

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W05`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W05; Project Context separate customer identity; M1 excludes real sessions.
- Successor/handoff: `H-08` → M2-W02 and T17. Activation: `active-in-this-ticket` (fixture routes only). Reuse: T02/T03; local auth-form composition (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render `daftar` and `masuk` fixture routes with field validation, loading, error, and successful-result presentation. Entry is a named auth-fixture scenario ID from a customer route; endpoint is semantic form/status UI explicitly saying no account/session was made. H-08 transfers scenario ID/version, field/error/result display model, and M2 auth successor; static import is idempotent and contains no credential/identity authority. Use path: route → adapter → form primitives → visible fixture result → reviewer.

## UI Reference fidelity

Reproduce `project's goal/UI Reference/daftar.html` and `project's goal/UI Reference/masuk.html` exactly. Match the visible form structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend loading/error/success-result states using the same referenced treatment without inventing a new visual language. Capture both routes at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Amendment M1-T08-A1 — mapped reusable reference refactor

Use the T08 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: extract and inventory the local static assets for `daftar.html` and `masuk.html`, refactor their rendered form/responsive compositions into reusable typed Next.js components, and wire every state through T02 fixtures. Reference behaviour and auth authority remain forbidden.

## Review Contract, invariants, security/UI, validation

No account, password persistence, session, redirect based on a role, or prototype localStorage path. T02 owns model/fixture errors; T03 owns fields/status. `SEC-M1-01 preserve-seam`: UI never proves authentication; `SEC-M1-02 implement-now`: framework escaping/no unsafe sink. `VIS-02/06/08/09/10/11`: keyboard labels, focus, error status, long fields, mobile form. Review `RC-18`: PASS iff both routes show all named states through T02 with no account/session artifact; capture plus boundary scan; a UI-only “logged in” redirect is forbidden. Hard stop: actual auth belongs to M2.

## Amendment M1-T08-A2 — measured rendered-reference acceptance

- Decision authority: user request on 2026-11-21 to apply the M1-T04 visual-contract correction to the remaining UI tickets.
- Binding contract: `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit 30532594fbf730f773c07bd6c68178b75edf7540, specifically its M1-T08 row, entry ledger, asset/source boundary, and acceptance evidence. This supplements the existing A1 mapping and original ticket review contract.
- Before implementation: record the mapped page/panel's ordered visible regions, measured desktop shell/panel and type geometry, source copy, local asset roles, actual breakpoints, and required fixture states. Use the reference-specific composition; T03 reuse must not flatten it into a generic shell.
- Verification: same-viewport source/implementation captures for each mapped page or admin panel at desktop and each listed breakpoint, including the boundary itself; side-by-side findings for geometry, copy, typography, palette, art/icons, controls, navigation, focus and overflow; named T02 state captures; keyboard and negative authority/security evidence; local asset inventory and source scan. Correct each material mismatch before handoff. A build or general visual similarity cannot substitute.
- Review row VIS-REF-M1-T08: PASS iff the ledger and all required captures are recorded, no material visible mismatch remains, and the original scenario/security review row also passes. Missing evidence or a mismatch blocks this ticket's visual handoff.
- Boundary preservation: no reference executable behavior, prototype storage/seed, external media, production identity, data write, or authority is granted. Ticket-specific publication, privacy, lifecycle and successor rules remain binding.
