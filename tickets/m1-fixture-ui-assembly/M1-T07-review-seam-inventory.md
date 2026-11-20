# M1-T07 — Complete visual review and backend replacement-seam inventory

- State: `superseded`; batch/milestone: `4 / M1 / M1-W07`; replaced by T17 after sizing review; depends on T04/H-04, T05/H-05, T06/H-06.
- Source authority: M1-W07; M1 exit gate; cross-phase verification M1; architecture UI transition.
- Successor/handoff: `H-07` to M2 entry/planning. Activation: `active-in-this-ticket` for the M1 review gate, not a product runtime.
- Reuse: `keep-local` — review inventory is M1 gate evidence. Legacy: `forbidden`; SQL: `not-applicable`; empty state: `not-applicable`.

## Outcome and exact scope

Run and record the review matrix for every named M1 route/scenario, visual state, mobile/desktop composition, accessibility interaction, fixture import boundary, and M2+ replacement seam. Record reviewer findings and exact owner successor. Exclude production acceptance, backend implementation, policy approval, and marking a fixture interaction as a live capability.

## Entrypoint, endpoint, handoff, expected use path

Entrypoint: M1 review command/checklist consumes T04–T06 route and scenario inventories. Endpoint: a versioned review record, pass/fail findings, captures/evidence locators, and replacement map. `H-07`: producer T07; consumer M2 planning; transport tracked artifact; required fields are source route/scenario, adapter contract/version, successor work package, fixture replacement risk, result/evidence; no retry semantics. EUP: review entry → route/scenario matrix → observed evidence → seam record → M2 planner.

## True path, invariants, non-goals

The only completion path is inspect actual routes through T02/T03, not reference HTML or isolated component snapshots. Any direct fixture feature import is a fail until corrected. Preserve the explicit non-authoritative labels, no live WhatsApp, no data persistence, and no public draft. Do not approve M2 merely because M1 builds.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-12 | M1 exit gate + verification | PASS iff public/customer/admin scenario matrix includes desktop/mobile, loading/empty/error/success as applicable, with captured observations; suite pass alone is forbidden; UI review gate. |
| RC-13 | M1-W07 + architecture | PASS iff every M1 feature has an adapter-mediated fixture → backend replacement entry, named successor, versioned contract, and no direct feature fixture import; inventory + import scan; vague future-backend note is forbidden; M2 seam. |
| RC-14 | frontend review gate | PASS iff each affected screen has VIS-001–014 outcome and evidence, with any failure recorded rather than waived; visual inspection; build/lint alone is forbidden; visual/accessibility quality. |
| SEC-07 | SEC-M1-01/02 | PASS iff review verifies no authority claim, unsafe sink, unreviewed origin, or prototype localStorage path survives in M1 code; search + route exercise; checklist assertion without evidence is forbidden; future security boundary. |

## Security readiness, UI/validation, and hard stop

Security readiness is `applicable`: preserve the M2 server-authorization seam and verify M1 CSP-safe rendering now. UI readiness is the full relevant frontend review gate. Hard stop: any unresolved seam, public draft, fixture-as-authority behavior, missing scenario/state evidence, or visual/accessibility FAIL leaves M1 not ready and blocks M2 ticket start.
