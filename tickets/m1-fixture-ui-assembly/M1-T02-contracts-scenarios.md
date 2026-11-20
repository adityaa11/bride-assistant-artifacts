# M1-T02 — Publish typed fixture scenario contracts and adapter

- State: `ready`; batch/milestone: `2 / M1 / M1-W02`; depends on T01/H-01.
- Source authority: M1-W02; architecture UI transition; Project Context lifecycle, publication, and current/target state.
- Successors/handoffs: `H-02` to T04–T06 and M2-W04. Activation: `active-in-this-ticket` through T04–T06.
- Reuse: `extract-shared` / RD-02. Legacy: `forbidden`. SQL: `not-applicable`. Empty state: required for empty catalogue/booking/content fixture scenarios.

## Outcome and exact scope

Define versioned TypeScript UI read models, non-authoritative command-result models, and a scenario catalogue behind one adapter. Include canonical lifecycle labels, published/draft distinction, availability-only customer view, WhatsApp-unavailable result, loading/empty/error/success fixtures. Do not encode business validation, persist commands, use browser storage, fabricate an official transition, or expose customer details across fixture surfaces.

## Entrypoint, endpoint, handoff, expected use path

Entrypoint: route composition asks the adapter for declared `scenarioId`; input is a compile-time named ID supplied by each feature route, not user authority. Unknown/unsupported IDs return a typed not-found/error state. Endpoint: discriminated `ScenarioResult` containing version, read model, and explicitly non-authoritative interaction/result state. `H-02`: producer T02 adapter; consumers T04/T05/T06; in-process typed import; fields include scenario ID/version, state kind, and only surface-permitted display data; static fixture retrieval has no retry/idempotency. EUP-02 ends at rendered reviewer-visible scenario.

## Empty and true path

For empty catalogue, no customer bookings, and no published content, render an explicit empty state explaining absence and next permitted review action; no hidden default, seed migration, or durable data is allowed. The only path is route → adapter → typed result → feature; direct fixture imports and prototype `localStorage` shapes are superseded and forbidden.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-01 | M1-W02 + architecture | PASS iff every T04–T06 scenario is retrieved through a versioned adapter and type-checks; import-boundary test/search; a shared JSON import into feature is forbidden; adapter seam. |
| RC-02 | Project lifecycle/publication rules | PASS iff fixture states use canonical labels and public reads distinguish published from draft; scenario assertions; prototype `Confirmed`/`On Hold` or public draft is forbidden; lifecycle/publication meaning. |
| RC-03 | Verification M1 | PASS iff loading, empty, error, unavailable-WhatsApp, and non-authoritative command-result scenarios are representable; render harness; a happy-path-only mock is forbidden; review-state coverage. |
| SEC-02 | SEC-M1-01 | PASS iff models cannot be mistaken for authenticated/session/official command evidence: types and copy carry non-authoritative provenance; assertion + review; client role flag as authority is forbidden; future server auth seam. |

## Security/UI/validation

Security: `applicable`; SEC-M1-01 preserve-seam, SEC-M1-02 implement-now. UI is consumed later. Evidence: type check, adapter-only import test, scenario catalogue inventory. Hard stop: unresolved canonical customer-visible rejection label remains a future product decision; use no invented rejection scenario.
