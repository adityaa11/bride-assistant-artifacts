# M1-T12 — Render admin booking and availability fixture workspace

- State: `awaiting_review`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context admin authority and canonical lifecycle/availability rule.
- Successor/handoff: `H-12` → M3-W02/M3-W06 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; admin operational calendar is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: no booking/date results are explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render admin booking list/detail, operational calendar, availability view, canonical status filters and no-result states. Entry is named admin-booking scenario ID; endpoint is a Data Workspace fixture view whose apparent controls return non-authoritative results. H-12 provides scenario/version and admin read/action-display fields to M3; no identity/record write crosses this ticket. Use path: admin route → adapter → admin calendar/list/detail → reviewer.

## UI Reference fidelity

Reproduce the corresponding admin booking and availability composition in `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` exactly. Match the visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Extend empty, filter, calendar, and non-authoritative result states using the same referenced treatment without inventing a new visual language. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Use the mapped reference HTML/CSS/JS as refactor inputs under A3; do not run the original page or its prototype script at runtime.

## Amendment M1-T12-A1 — mapped reusable reference refactor

Use the T12 mapping in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`: refactor the admin reference `Bookings` and `Availability` compositions and their 1250px/930px/485px transformations into reusable typed Next.js workspace components, using only the corresponding locally extracted static assets and T02 fixtures. Reference behaviour and admin/availability authority remain forbidden.

## Review Contract, invariants, security/UI, validation

No admin login/role decision, availability mutation, status command, customer disclosure outside this admin fixture route, or `Confirmed`/`On Hold`. Security preserves server admin/transaction seams; safe rendering now. UI: Data Workspace, desktop sidebar/narrow drawer, VIS-003/06/08/10/11. `RC-22`: PASS iff populated/empty/filter/calendar states show canonical labels and non-authoritative controls; route capture + token scan; client role gate or mutation forbidden. Hard stop: operational activation is M3.

## Amendment M1-T12-A2 — measured rendered-reference acceptance

- Decision authority: user request on 2026-11-21 to apply the M1-T04 visual-contract correction to the remaining UI tickets.
- Binding contract: `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit 30532594fbf730f773c07bd6c68178b75edf7540, specifically its M1-T12 row, entry ledger, asset/source boundary, and acceptance evidence. This supplements the existing A1 mapping and original ticket review contract.
- Before implementation: record the mapped page/panel's ordered visible regions, measured desktop shell/panel and type geometry, source copy, local asset roles, actual breakpoints, and required fixture states. Use the reference-specific composition; T03 reuse must not flatten it into a generic shell.
- Verification: same-viewport source/implementation captures for each mapped page or admin panel at desktop and each listed breakpoint, including the boundary itself; side-by-side findings for geometry, copy, typography, palette, art/icons, controls, navigation, focus and overflow; named T02 state captures; keyboard and negative authority/security evidence; local asset inventory and source scan. Correct each material mismatch before handoff. A build or general visual similarity cannot substitute.
- Review row VIS-REF-M1-T12: PASS iff the ledger and all required captures are recorded, no material visible mismatch remains, and the original scenario/security review row also passes. Missing evidence or a mismatch blocks this ticket's visual handoff.
- Boundary preservation: no reference executable behavior, prototype storage/seed, external media, production identity, data write, or authority is granted. Ticket-specific publication, privacy, lifecycle and successor rules remain binding.

## Amendment M1-T12-A3 — source-led Next.js refactor

- Decision authority: explicit user direction on 2026-11-21 to use the mapped UI Reference HTML and refactor its assets, CSS, JavaScript interactions, and components for Next.js plus fixtures. This amendment supersedes this ticket's earlier ban on using/adapting reference HTML/CSS/JS as source material; its A1/A2 visual and evidence requirements continue.
- Binding source contract: `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` and `docs/frontend/M1_REMAINING_UI_REFERENCE_CONTRACT.md` at source commit `5b45033ba06cce63962e0e7bd0171086806d281e`, applied to this ticket's existing mapped page(s) or admin panel(s).
- Required implementation path: start from the mapped HTML structure; extract its actual regions into JSX/TSX, preserve element order and visual relationships, adapt its CSS rules/media queries into project styles, use the extracted local visual assets in their mapped roles, and translate visible script-driven interactions into React state/handlers. Reuse only regions and controls evidenced by the reference or an already extracted matching region. Do not invent a substitute shell, card, form, navigation, calendar, artwork, or alternate visual language.
- Fixture integration: replace prototype data, storage, writes, and business decisions with typed T02 scenario props and explicit non-authoritative results. Preserve this ticket's route/state scope, security, identity, publication, and successor boundaries. No original HTML page, inline script/handler, unsafe HTML sink, localStorage path, external origin, or reference business authority runs at runtime.
- Review row `SRC-REF-M1-T12`: PASS iff the checkpoint contains a source-region/CSS/asset/interaction-to-component map for every mapped page or panel, the route uses those converted components and T02 fixtures, and the A2 same-viewport visual/state/keyboard evidence has no material mismatch. A generic reconstruction, missing mapped region/interaction without a recorded reason, direct prototype script execution, or build-only evidence is a FAIL.

## Amendment M1-T12-A4 — user-directed removal of two admin annotations

- Decision authority: the user's two browser comments on 2026-11-22 at `http://localhost:3000/admin`.
- Remove the visible `Fixture note:` block below the Bookings grid and the `Non-authoritative fixture` footer from the admin workspace. The footer is shared by Bookings and Availability, so remove it from both.
- This is an explicit rendered-reference exception to the source's `.hintbox` prototype note on Bookings and to the implementation-added provenance footer. It changes those two visible annotations only. The Availability panel's source-mapped transactional note remains.
- Keep named scenario IDs, typed fixture data, canonical statuses, non-authoritative action feedback, and all booking/availability authority boundaries intact. Record the visual exception and recapture the affected route.
