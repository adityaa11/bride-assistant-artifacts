# M1-T16 — Render admin promotion fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context promotion representation and catalogue boundary.
- Successor/handoff: `H-16` → M3-W02/M4-W05 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; promotion rule presentation is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no promotion is explicit without a default.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render promotion list/detail/form fixture states for applicable packages, percentage/rupiah representation, active period, validation/error, and proposed-result display. Entry is named promotion scenario ID; endpoint is a non-authoritative Settings Form review surface. H-16 transfers versioned promotion display/action-result fields to successors, never a price calculation authority. Use path: admin route → adapter → promotion composition → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin promotion composition in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend validation, error, empty, long-content, and proposed-result states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Amendment M1-T16-A1 — mapped reusable reference refactor

Use the T16 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: refactor the admin reference `Promotions` composition and its 1250px/930px/485px transformations into reusable typed Next.js workspace components, using only the corresponding locally extracted static assets and T02 fixtures. Reference behaviour and promotion/price authority remain forbidden.

## Review Contract, invariants, security/UI, validation

No promotion CRUD, effective-price calculation, booking snapshot mutation, or date-policy decision. Security preserves admin authorization and server validation seam; safe rendering now. UI: Settings Form, clear currency/percentage labels and responsive long package selections, VIS-003/06/08/09/10/11. `RC-26`: PASS iff all specified fixture states render with explicit non-authoritative result and no package price is mutated; capture + mutation scan; client calculation/seed save forbidden. Hard stop: authoritative promotion logic belongs to M3/M4.

## Amendment M1-T16-A2 — measured rendered-reference acceptance

- Decision authority: user request on 2026-11-21 to apply the M1-T04 visual-contract correction to the remaining UI tickets.
- Binding contract: `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit 30532594fbf730f773c07bd6c68178b75edf7540, specifically its M1-T16 row, entry ledger, asset/source boundary, and acceptance evidence. This supplements the existing A1 mapping and original ticket review contract.
- Before implementation: record the mapped page/panel's ordered visible regions, measured desktop shell/panel and type geometry, source copy, local asset roles, actual breakpoints, and required fixture states. Use the reference-specific composition; T03 reuse must not flatten it into a generic shell.
- Verification: same-viewport source/implementation captures for each mapped page or admin panel at desktop and each listed breakpoint, including the boundary itself; side-by-side findings for geometry, copy, typography, palette, art/icons, controls, navigation, focus and overflow; named T02 state captures; keyboard and negative authority/security evidence; local asset inventory and source scan. Correct each material mismatch before handoff. A build or general visual similarity cannot substitute.
- Review row VIS-REF-M1-T16: PASS iff the ledger and all required captures are recorded, no material visible mismatch remains, and the original scenario/security review row also passes. Missing evidence or a mismatch blocks this ticket's visual handoff.
- Boundary preservation: no reference executable behavior, prototype storage/seed, external media, production identity, data write, or authority is granted. Ticket-specific publication, privacy, lifecycle and successor rules remain binding.
