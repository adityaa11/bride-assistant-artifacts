# M1-T15 — Render admin content-publication fixture workspace

- State: `ready`; batch/milestone/work package: `3 / M1 / M1-W06`; dependencies: T02/H-02, T03/H-03.
- Source authority: M1-W06; Project Context published-only and permission boundary.
- Successor/handoff: `H-15` → M4-W05/M4-W06 and T17. Activation: `active-in-this-ticket` fixture route. Reuse: T02/T03; publication review is local (`keep-local`). Legacy: `forbidden`. SQL: `not-applicable`. Empty state: empty portfolio/testimonial/event collections are explicit.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Render fixture portfolio, testimonials, events, draft/published distinction, and proposed publish-result states. Entry is named content scenario ID; endpoint is a review workspace with no media upload, persistence, or public publication. H-15 provides scenario/version, publication display state, and M4 successor mapping. Use path: admin route → adapter → content review surface → reviewer.

## Review Contract, invariants, security/UI, validation

No bytes/media origin, consent claim, publish mutation, or draft public output. Security preserves private/public delivery and consent seams; safe rendering only. UI: Entity Library/Settings Form and state semantics, VIS-003/06/08/10/11. `RC-25`: PASS iff draft and published are visually/non-color distinguishable and public T04 cannot render a draft fixture; cross-route negative test + captures; hidden DOM draft or client publication flag forbidden. Hard stop: M4 owns real publication/media policy.
