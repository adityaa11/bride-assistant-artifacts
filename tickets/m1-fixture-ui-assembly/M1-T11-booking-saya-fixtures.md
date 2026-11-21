# M1-T11 — Render Booking Saya lifecycle, proof, and exception fixture views

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W05`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W05; Project Context customer-visible lifecycle, proof, and exception boundaries.
- Successor/handoff: `H-11` → M3-W05, M4-W02–04, T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; local lifecycle detail (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no bookings is explicit and initializes nothing.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render Booking Saya list/detail, canonical statuses, proof/exception placeholders, filters, no-bookings/loading/error states. Entry is named booking-history scenario ID; endpoint is read-only fixture detail and explicitly non-authoritative action result. H-11 gives successor scenario/version, visible lifecycle/proof/exception fields and no-write result. Use path: route → adapter → list/detail/status composition → customer-visible fixture state → reviewer.

## UI Reference fidelity

Reproduce `project's goal/UI Reference/booking_saya.html` exactly. Match the visible list/detail structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend loading, empty, error, proof, and exception states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Use the mapped reference HTML/CSS/JS as refactor inputs under A3; do not run the original page or its prototype script at runtime.

## Amendment M1-T11-A1 — mapped reusable reference refactor

Use the T11 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: extract and inventory `booking_saya.html` static assets, refactor its list/detail and responsive composition into reusable typed Next.js components, and wire every state through T02 fixtures. Reference behaviour and lifecycle authority remain forbidden.

## Review Contract, invariants, security/UI, validation

Only the owner-shaped fixture view appears; no file bytes/upload, status mutation, cancellation/reschedule submission, or cross-customer data. T02 scenario contract is shared; list/detail remains local. Security preserves owner/upload seams and safe rendering. UI: Knowledge List + workflow detail, VIS-003/06/08/09/10/11. `RC-21`: PASS iff each canonical state, no-booking, proof/exception placeholder and error is inspectable with no official change; matrix capture and negative mutation test; fixture upload/transition forbidden. Hard stop: rejection label remains undefined and is omitted.

## Amendment M1-T11-A2 — measured rendered-reference acceptance

- Decision authority: user request on 2026-11-21 to apply the M1-T04 visual-contract correction to the remaining UI tickets.
- Binding contract: `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit 30532594fbf730f773c07bd6c68178b75edf7540, specifically its M1-T11 row, entry ledger, asset/source boundary, and acceptance evidence. This supplements the existing A1 mapping and original ticket review contract.
- Before implementation: record the mapped page/panel's ordered visible regions, measured desktop shell/panel and type geometry, source copy, local asset roles, actual breakpoints, and required fixture states. Use the reference-specific composition; T03 reuse must not flatten it into a generic shell.
- Verification: same-viewport source/implementation captures for each mapped page or admin panel at desktop and each listed breakpoint, including the boundary itself; side-by-side findings for geometry, copy, typography, palette, art/icons, controls, navigation, focus and overflow; named T02 state captures; keyboard and negative authority/security evidence; local asset inventory and source scan. Correct each material mismatch before handoff. A build or general visual similarity cannot substitute.
- Review row VIS-REF-M1-T11: PASS iff the ledger and all required captures are recorded, no material visible mismatch remains, and the original scenario/security review row also passes. Missing evidence or a mismatch blocks this ticket's visual handoff.
- Boundary preservation: no reference executable behavior, prototype storage/seed, external media, production identity, data write, or authority is granted. Ticket-specific publication, privacy, lifecycle and successor rules remain binding.

## Amendment M1-T11-A3 — source-led Next.js refactor

- Decision authority: explicit user direction on 2026-11-21 to use the mapped UI Reference HTML and refactor its assets, CSS, JavaScript interactions, and components for Next.js plus fixtures. This amendment supersedes this ticket's earlier ban on using/adapting reference HTML/CSS/JS as source material; its A1/A2 visual and evidence requirements continue.
- Binding source contract: `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` and `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit `5b45033ba06cce63962e0e7bd0171086806d281e`, applied to this ticket's existing mapped page(s) or admin panel(s).
- Required implementation path: start from the mapped HTML structure; extract its actual regions into JSX/TSX, preserve element order and visual relationships, adapt its CSS rules/media queries into project styles, use the extracted local visual assets in their mapped roles, and translate visible script-driven interactions into React state/handlers. Reuse only regions and controls evidenced by the reference or an already extracted matching region. Do not invent a substitute shell, card, form, navigation, calendar, artwork, or alternate visual language.
- Fixture integration: replace prototype data, storage, writes, and business decisions with typed T02 scenario props and explicit non-authoritative results. Preserve this ticket's route/state scope, security, identity, publication, and successor boundaries. No original HTML page, inline script/handler, unsafe HTML sink, localStorage path, external origin, or reference business authority runs at runtime.
- Review row `SRC-REF-M1-T11`: PASS iff the checkpoint contains a source-region/CSS/asset/interaction-to-component map for every mapped page or panel, the route uses those converted components and T02 fixtures, and the A2 same-viewport visual/state/keyboard evidence has no material mismatch. A generic reconstruction, missing mapped region/interaction without a recorded reason, direct prototype script execution, or build-only evidence is a FAIL.