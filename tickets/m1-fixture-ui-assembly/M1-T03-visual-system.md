# M1-T03 — Create the Soft Rose UI system and accessible primitives

- State: `ready`; batch/milestone: `2 / M1 / M1-W03`; depends on T01/H-01; parallel-safe with T02.
- Source authority: M1-W03; Project Context visual direction/navigation boundary; supplied references; frontend-awareness findings in set README.
- Successors/handoffs: `H-03` to T04–T06. Activation: `active-in-this-ticket` through shared route-shell smoke view.
- Reuse: `extract-shared` / RD-01. Legacy: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome and exact scope

Create semantic design tokens, documented project visual contract, responsive shell primitives, navigation foundations, accessible form/status/empty-state primitives, and interaction-state rules. It must preserve the Soft Rose Atelier grammar without copying reference source/style code. Exclude screen-specific workflow/business rules, theme toggle unless approved, CSP headers, and auth semantics.

## Entrypoint, endpoint, handoff, expected use path

Entrypoint: T04–T06 route shells compose named primitive props (label, state, children, accessible description) from typed scenario data. Endpoint: rendered semantic HTML with tokenized normal/hover/focus/disabled/loading/error/success states at desktop and narrow width. `H-03`: tokens/primitives and `PROJECT_FRONTEND_CONTRACT.md`; consumers T04–T06; props carry presentation state only, no authority; imports are static and idempotent. EUP-03 ends in reviewer-visible accessible route render.

## Required visual contract

Write `docs/frontend/PROJECT_FRONTEND_CONTRACT.md` using the set README's derived identity, density, navigation, tokens, geometry, typography roles, semantic colors, responsive transformation, motion, and forbidden patterns. Customer/public uses readable workflow/discovery surfaces; admin uses restrained operational/data-workspace surfaces. Responsive grids derive from available shell width and item count; long content wraps or deliberately discloses, never silently clips.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| VIS-01 | frontend State 1 / VIS-001..006 | PASS iff visual contract exists and primitives use its semantic type/spacing/radius/color roles; document + rendered inspection; arbitrary per-screen CSS is forbidden; shared visual system. |
| VIS-02 | M1-W03 + WCAG 2.2 AA | PASS iff primitives support semantic labels, keyboard use, focus-visible, non-color statuses and all applicable interaction states; keyboard/state harness; static screenshot is insufficient; accessibility baseline. |
| VIS-03 | M1-W03 + responsive fit | PASS iff desktop and mobile shells transform navigation/content rather than squeeze; capture at representative shell widths and long-value stress cases; viewport-only card stretching is forbidden; responsive composition. |
| SEC-03 | SEC-M1-02/CSP-safe UI | PASS iff primitives avoid unsafe HTML sinks, inline execution, runtime style writes, and silent external assets; targeted scan; a client CSP workaround is forbidden; future deployed CSP. |

## Security/UI/validation

Security: `applicable`; same shared bindings. UI readiness: frontend review gate VIS-001–014 (VIS-015 only if later theme support is added). Validate shared-shell render, keyboard focus, 200%/narrow reflow, reduced-motion behavior, and long label/content cases. Hard stop: a requested external font/script/origin or CSP-header relaxation requires policy owner review.
