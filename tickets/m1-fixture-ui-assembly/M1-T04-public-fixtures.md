# M1-T04 — Assemble public discovery and published-content fixture screens

- State: `ready`; batch/milestone: `3 / M1 / M1-W04`; depends on T02/H-02 and T03/H-03.
- Source authority: M1-W04; Project Context public scope and published-only invariant.
- Successor/handoff: `H-04` to M3-W03/M4-W06 and T07. Activation: `active-in-this-ticket` (fixture routes only).
- Reuse: shared T02/T03 contracts; local public discovery composition (`keep-local`). Legacy: `forbidden`; SQL: `not-applicable`; empty state: published content/packages may be empty.

## Scope, path, and state

Render public home/discovery, packages, promotions, and published-content scenarios through the public adapter. Entry: public route group receives a declared scenario ID. Endpoint: visitor-visible read-only screen with package selection CTA but no account requirement or authoritative booking write. `H-04` produces route/scenario inventory plus public read-model fields for M3/M4 successors; public-only fields have no customer identity; static scenario retrieval is idempotent. EUP: public route → adapter → public composition → visible published content → reviewer.

Empty result says no currently published content/package and exposes no draft; it initializes nothing. True path removes prototype static/local values. Do not add CMS, public API, feature flag, or unapproved media origin.

## UI Reference fidelity

Reproduce the public composition shown in `project's goal/UI Reference/index.html` exactly. Match its visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Do not invent an alternate public layout or substitute a generic card/list design. Capture the route at the reference viewport and record a visual comparison; any visible mismatch fails this ticket. Do not import the reference HTML/CSS/JS.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-04 | M1-W04 | PASS iff discovery/package/promotion/published-content scenarios render through T02/T03; scenario route captures; copied static HTML is forbidden; public fixture flow. |
| RC-05 | Project publication invariant | PASS iff draft/unauthorized fixture content never appears in public scenarios, including empty/error states; negative render assertion; visual hiding alone is forbidden; future publication seam. |
| VIS-04 | visual contract/VIS-002..011 | PASS iff discovery hierarchy, CTA, readable responsive cards/lists, focus and empty/error states pass visual/keyboard captures; build only is forbidden; public responsive UX. |
| VIS-REF-04 | `project's goal/UI Reference/index.html` | PASS iff the rendered public screen matches the reference at its reference viewport with no visible mismatch; comparison capture/diff; a functionally equivalent or merely inspired layout is forbidden; exact public visual fidelity. |
| SEC-04 | SEC-M1-01/02 | PASS iff public UI treats fixtures as display data, safely renders text, and carries no admin/customer data or external origin; boundary scan + render test; client-side filtering is forbidden; privacy/publication seam. |

## Validation / hard stop

Security readiness: `applicable`; SEC-M1-01 is a preserve-seam and SEC-M1-02 is implemented through the review rows above.

Use desktop/mobile, empty, error, and draft-negative scenarios. No unresolved blocker; real public query/publication activation belongs to named successors.
