# M1-T06 — Assemble admin operational fixture screens with canonical semantics

- State: `superseded`; batch/milestone: `3 / M1 / M1-W06`; replaced by T12–T16 after sizing review; depends on T02/H-02 and T03/H-03.
- Source authority: M1-W06; Project Context admin scope/admin authority; architecture UI transition.
- Successors/handoffs: `H-06` to M3-W02/M3-W06/M4-W05 and T07. Activation: `active-in-this-ticket` (fixture routes only).
- Reuse: shared T02/T03; admin nav/data workspace stays local (RD-03/04). Legacy: `forbidden`; SQL: `not-applicable`; empty state: empty catalogue/bookings/content fixtures.

## Scope, entrypoint, endpoint, handoff, expected use path

Render admin sign-in placeholder and fixture views for catalogue/packages/prices, bookings, availability, customers, portfolio/testimonials, events, promotions, and publication state. Entry: admin route declares a scenario ID. Endpoint: operational review surface where apparent actions return non-authoritative fixture results and never mutate a record. `H-06` supplies admin read/command-result field inventory and successor map; scenario ID/version preserve correlation; static retrieval is idempotent. EUP: admin route → adapter/primitives → local data workspace/settings form → reviewer.

Empty views explain no records without creating defaults. True path retires prototype `Confirmed` and `On Hold`; use canonical status vocabulary and clearly label proposed fixture controls. Do not implement admin authentication, role checks, mutation, media upload, export, or scheduling policy.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-09 | M1-W06 + Project admin scope | PASS iff all listed admin operational areas have declared fixture routes/scenarios and non-authoritative controls; inventory/captures; prototype script port is forbidden; admin review coverage. |
| RC-10 | M1-W06 + canonical lifecycle | PASS iff no `Confirmed`/`On Hold` semantic remains and canonical statuses are visible where status is represented; text/state scan + capture; CSS renaming only is forbidden; lifecycle handoff. |
| RC-11 | Project publication/privacy | PASS iff draft/published state is visibly distinct, draft is not represented as public, and fixture customer data remains route-local to admin review; scenario assertion; shared client store/hidden public fields forbidden; publication/ownership seam. |
| VIS-06 | visual contract/VIS-003..011 | PASS iff admin uses Data Workspace/Settings Form/Entity Library patterns with desktop sidebar and narrow drawer transformation; visual + keyboard/overflow inspection; decorative dashboard cards are forbidden; operational usability. |
| SEC-06 | SEC-M1-01/02 | PASS iff admin placeholder makes no authorization claim and no fixture action becomes authority; route/copy test + unsafe-sink scan; client role gating is forbidden; future server authorization. |

## Validation / hard stop

Security readiness: `applicable`; preserve the future server-side admin authorization seam and enforce M1 safe-rendering discipline now.

Capture populated/empty/error cases, status filters, draft/published distinction, dense desktop and narrow mobile/drawer composition. Any request to activate a control or infer a role is a stop for M2 authorization planning.
