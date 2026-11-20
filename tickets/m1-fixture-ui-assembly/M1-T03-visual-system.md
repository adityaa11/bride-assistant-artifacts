# M1-T03 — Create the Soft Rose UI system and accessible primitives

- State: `approved`; batch/milestone: `2 / M1 / M1-W03`; depends on T01/H-01; parallel-safe with T02.
- Source authority: M1-W03; Project Context visual direction/navigation boundary; `project's goal/UI Reference/`; frontend-awareness findings in set README.
- Successors/handoffs: `H-03` to T04–T06. Activation: `active-in-this-ticket` through shared route-shell smoke view.
- Reuse: `extract-shared` / RD-01. Legacy: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome and exact scope

Create semantic design tokens, documented project visual contract, responsive shell primitives, navigation foundations, accessible form/status/empty-state primitives, and interaction-state rules that reproduce the supplied UI Reference exactly. The rendered M1 screens must match every corresponding reference screen's visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. Do not copy or import reference source/style code; implement the same rendered result through the project UI system. Exclude screen-specific workflow/business rules, theme toggle unless approved, CSP headers, and auth semantics.

## Entrypoint, endpoint, handoff, expected use path

Entrypoint: T04–T06 route shells compose named primitive props (label, state, children, accessible description) from typed scenario data. Endpoint: rendered semantic HTML with tokenized normal/hover/focus/disabled/loading/error/success states at desktop and narrow width. `H-03`: tokens/primitives and `PROJECT_FRONTEND_CONTRACT.md`; consumers T04–T06; props carry presentation state only, no authority; imports are static and idempotent. EUP-03 ends in reviewer-visible accessible route render.

## Required visual contract

Write `docs/frontend/PROJECT_FRONTEND_CONTRACT.md` from `project's goal/UI Reference/` and the set README's derived identity, density, navigation, tokens, geometry, typography roles, semantic colors, responsive transformation, motion, and forbidden patterns. Customer/public and admin screens must reproduce their corresponding reference compositions; the contract is not a license to redesign them. Responsive grids derive from the reference composition and available shell width; long content wraps or deliberately discloses, never silently clips. States not shown in the references extend the referenced visual treatment and do not create new styling.

## UI Reference fidelity gate

The reference directory is binding for visual output. Capture every M1 route at the corresponding reference viewport and compare the rendered result against the reference. PASS requires no visible divergence in layout, hierarchy, copy, typography, color, spacing, geometry, imagery/icon treatment, navigation, or responsive composition. Functional equivalence, a generic Soft Rose interpretation, or a reviewer preference is not a substitute for the comparison. Any visible mismatch blocks the handoff to T04/T08–T16.

## Amendment M1-T03-A1 — M1 mapped reference system

The shared system must implement `docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit `e98a804eeca02efb65363339c4f7d050bd3d861a`. It owns reusable semantic primitives and responsive foundations, but must preserve each ticket's mapped reference composition rather than imposing a generic shell. Use only the corresponding locally extracted static assets recorded in `public/reference-assets/ui-reference/manifest.json`; no reference source or behaviour is authorized.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| VIS-01 | frontend State 1 / VIS-001..006 | PASS iff visual contract exists and primitives use its semantic type/spacing/radius/color roles; document + rendered inspection; arbitrary per-screen CSS is forbidden; shared visual system. |
| VIS-REF-01 | `project's goal/UI Reference/` | PASS iff every referenced M1 screen is rendered with the same visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition; reference-viewport captures plus overlay/diff record; “inspired by” or functionally equivalent UI is forbidden; exact visual fidelity. |
| VIS-02 | M1-W03 + WCAG 2.2 AA | PASS iff primitives support semantic labels, keyboard use, focus-visible, non-color statuses and all applicable interaction states; keyboard/state harness; static screenshot is insufficient; accessibility baseline. |
| VIS-03 | M1-W03 + responsive fit | PASS iff desktop and mobile shells transform navigation/content rather than squeeze; capture at representative shell widths and long-value stress cases; viewport-only card stretching is forbidden; responsive composition. |
| SEC-03 | SEC-M1-02/CSP-safe UI | PASS iff primitives avoid unsafe HTML sinks, inline execution, runtime style writes, and silent external assets; targeted scan; a client CSP workaround is forbidden; future deployed CSP. |

## Security/UI/validation

Security: `applicable`; same shared bindings. UI readiness: frontend review gate VIS-001–014 (VIS-015 only if later theme support is added). Validate shared-shell render, keyboard focus, 200%/narrow reflow, reduced-motion behavior, and long label/content cases. Hard stop: a requested external font/script/origin or CSP-header relaxation requires policy owner review.
