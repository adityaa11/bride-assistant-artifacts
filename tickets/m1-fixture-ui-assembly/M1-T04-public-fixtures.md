# M1-T04 — Assemble public discovery and published-content fixture screens

- State: `approved`; batch/milestone: `3 / M1 / M1-W04`; depends on T02/H-02 and T03/H-03.
- Source authority: M1-W04; Project Context public scope and published-only invariant.
- Successor/handoff: `H-04` to M3-W03/M4-W06 and T07. Activation: `active-in-this-ticket` (fixture routes only).
- Reuse: shared T02/T03 contracts; local public discovery composition (`keep-local`). Legacy: `forbidden`; SQL: `not-applicable`; empty state: published content/packages may be empty.

## Scope, path, and state

Render public home/discovery, packages, promotions, and published-content scenarios through the public adapter. Entry: public route group receives a declared scenario ID. Endpoint: visitor-visible read-only screen with package selection CTA but no account requirement or authoritative booking write. `H-04` produces route/scenario inventory plus public read-model fields for M3/M4 successors; public-only fields have no customer identity; static scenario retrieval is idempotent. EUP: public route → adapter → public composition → visible published content → reviewer.

Empty result says no currently published content/package and exposes no draft; it initializes nothing. True path removes prototype static/local values. Do not add CMS, public API, feature flag, or unapproved media origin.

## UI Reference fidelity

Reproduce the public composition shown in `project's goal/UI Reference/index.html` exactly. Match its visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Do not invent an alternate public layout or substitute a generic card/list design. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Amendment M1-T04-A1 — reference visual assets

- Decision authority: explicit user authorization on 2026-11-20.
- Permitted: extract the static embedded visual assets supplied by `project's goal/UI Reference/index.html` into project-managed local files under `public/reference-assets/`, then render those files through ordinary framework image elements. The extracted assets may be used only by this public fixture surface and are not an external origin.
- Still forbidden: reference HTML, CSS, JavaScript, prototype storage, dynamic data, event handlers, and any other behavioral code. The adapter, typed published-only read model, non-authoritative result, and all existing security/publication boundaries remain required.
- Verification: asset inventory must identify every extracted file and its reference role; source scans must confirm there is no copied reference execution or external source.

## Amendment M1-T04-A2 — exact rendered-reference contract

- Decision authority: explicit user authorization on 2026-11-20.
- Binding contract: `docs/frontend/M1-T04_PUBLIC_REFERENCE_CONTRACT.md` at source commit `53e264743018b2e430a6b2fa76802b1296ceb55c` is the measurable rendering contract derived from `project's goal/UI Reference/index.html`. It fixes the required shell, header, hero composition, type roles, palette, extracted-asset roles, responsive transformations, and evidence standard.
- Required interpretation: “exactly look like” means the independent fixture implementation must visually match the rendered reference at equivalent desktop and mobile viewports. “Close”, “inspired by”, a generic editorial split layout, a simplified bride, or a passing build is not a substitute.
- Boundary preservation: this amendment adds visual specificity only. It does not authorize copied reference HTML/CSS/JS, reference execution, external origins, untyped prototype data, public writes, identity data, draft leakage, or a change to any T02/T03/publication/security boundary.
- Verification: record same-viewport side-by-side captures and comparison findings for desktop, 830px, and 600px; validate keyboard focus/navigation plus default, empty, error, and draft-negative scenarios; maintain the local-asset inventory and source-boundary scan. A material visible mismatch fails M1-T04.

## Amendment M1-T04-A3 — M1 mapped implementation contract

Apply the M1-wide mapping, asset extraction, reusable-component, fixture-wiring, and responsive-extraction rules in `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`. This supplements the more specific M1-T04-A1/A2 contract and does not broaden the no-reference-behaviour or public-data boundaries.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-04 | M1-W04 | PASS iff discovery/package/promotion/published-content scenarios render through T02/T03; scenario route captures; copied static HTML is forbidden; public fixture flow. |
| RC-05 | Project publication invariant | PASS iff draft/unauthorized fixture content never appears in public scenarios, including empty/error states; negative render assertion; visual hiding alone is forbidden; future publication seam. |
| VIS-04 | visual contract/VIS-002..011 | PASS iff discovery hierarchy, CTA, readable responsive cards/lists, focus and empty/error states pass visual/keyboard captures; build only is forbidden; public responsive UX. |
| VIS-REF-04 | `project's goal/UI Reference/index.html`; M1-T04-A2 rendered-reference contract | PASS iff desktop, 830px, and 600px renders match the reference with no material visible mismatch, as proven by same-viewport side-by-side captures/comparison; a functionally equivalent or merely inspired layout, simplified bride, or build-only proof is forbidden; exact public visual fidelity. |
| SEC-04 | SEC-M1-01/02 | PASS iff public UI treats fixtures as display data, safely renders text, and carries no admin/customer data or external origin; boundary scan + render test; client-side filtering is forbidden; privacy/publication seam. |

## Validation / hard stop

Security readiness: `applicable`; SEC-M1-01 is a preserve-seam and SEC-M1-02 is implemented through the review rows above.

Use desktop/mobile, empty, error, and draft-negative scenarios. No unresolved blocker; real public query/publication activation belongs to named successors.
