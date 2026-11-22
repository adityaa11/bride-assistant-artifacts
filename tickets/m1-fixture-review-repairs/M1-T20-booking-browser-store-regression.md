# M1-T20 — Remove browser booking store and restore fixture-only customer flow

- State: `ready`.
- Batch / milestone / work-package: `BAP-M1-FIXTURE-REPAIR-01 / 1`, M1 / M1-W05 with M1-W07 regression handoff.
- Code repository and planning baseline: `https://github.com/adityaa11/bride-assistant.git` at `2ee952c9d6230849ff70e2db62efcc76454ecad9`.
- Source authority: user direction on 22 November 2026 to remove the browser booking store from `/booking`, `/beranda` and `/booking-saya`, keep submit result non-authoritative, and prove no browser booking write or new cross-route record; [T10 RC-20](../m1-fixture-ui-assembly/M1-T10-customer-booking-request-fixtures.md), [T09 RC-19](../m1-fixture-ui-assembly/M1-T09-customer-home-help-fixtures.md), [T11 RC-21](../m1-fixture-ui-assembly/M1-T11-booking-saya-fixtures.md), [T02/H-02](../m1-fixture-ui-assembly/M1-T02-contracts-scenarios.md), [Project Context](../../contexts/project.md), and [T17 F-01](../../evidence/M1-T17-v1.md).
- Dependencies: approved T02/T03 and T09/T10/T11 fixture interfaces, current T17 failure evidence. T19 audit is independent and cannot silently widen this repair.
- Successor/handoff: `H-20` to [M1-T21](M1-T21-browser-selector-regression.md) and [M1-T17-A7](../m1-fixture-ui-assembly/M1-T17-review-seam-inventory.md#amendment-m1-t17-a7--reconcile-fixture-audit-and-regression-repairs-before-re-review); M3-W05 remains the real booking-submission successor.
- Activation: `active-in-this-ticket` on the three actual customer routes.
- Reuse: `RD-REP-01 reuse-existing` T02 adapter and customer fixture reads; `RD-REP-02 keep-local` distinct customer compositions.
- Legacy: `forbidden`; no browser-store fallback or compatibility wrapper.
- SQL: `not-applicable`; no migration, production data, durable seed or new test storage.
- Empty state: named T02 empty/no-bookings states remain empty before and after a local submit; no initialization.

## Outcome and exact scope

Retire the post-approval `shared/scenarios/customer-booking-store.ts` browser persistence path and every production import/caller of it in `CustomerBooking`, `CustomerHome`, and `CustomerBookingHistory`. `/booking` uses the named T02 scenario for package/date display and local form state, then shows only the approved non-authoritative T10 result. A submit creates no booking ID, record, package snapshot, cross-route event or storage entry. `/beranda` and `/booking-saya` render their typed T02 records and named empty/error/lifecycle states only; submitting `/booking` does not append a record to them. Keep the existing forms, route names, source-derived layout, accessible result/focus behavior and customer chrome. Remove or correct any visible copy claiming that a fixture record was saved or is available on another page, using ticket-authorized non-authoritative result wording rather than inventing a production claim.

The M1 fixture graph audit in T19 may later recommend shared cross-surface producers. T20 does not implement that wider model or make independently authored customer/admin fixtures an accepted final architecture.

## Entrypoint, endpoint, handoff, expected use path

- Entrypoint: `/booking` and named state pages pass compile-time `ScenarioId` to `CustomerBooking`; `/beranda` and `/booking-saya` pass their own declared IDs to `CustomerHome`/`CustomerBookingHistory`. Inputs are typed T02 `ScenarioResult` fields and local form values, never an authenticated or authoritative request. Invalid form input stays in local validation state; unknown scenario IDs retain the T02 typed error path.
- Endpoint: successful valid form interaction renders the T10 local result with explicit non-authoritative meaning, no side effect; home/history retain their T02 records and states after navigation/reload. No server endpoint, event bus or record write is added.
- `H-20`: exact source commit, affected route/scenario/version, retired store/import path, browser submit/no-storage/no-new-record evidence, state and visual regression results; T21 consumes it for selector and suite repair. No retry/idempotency or durable identity is claimed.
- `EUP-20-A`: `/booking` route → `getScenario` → `CustomerBooking` form → local result → reviewer. `EUP-20-B`: `/beranda` and `/booking-saya` → `getScenario` → their existing T02 customer read models → summary/list/detail → reviewer. Required proof is actual browser navigation and source/import scan, not unit-only calls.

## True code path, empty state, and invariants

The one operational path after repair is route → T02 adapter → customer component → visible fixture state/result. Retire `readCreatedBookingFixtures`, `saveCreatedBookingFixture`, `bookingFixtureId`, `useCustomerBookingRecords`, the `ranur.customer-booking-fixtures.v1` key and their browser event listeners; prove no reachable production import remains. Do not retain a dormant fallback. With zero T02 customer bookings, home/history show their existing empty state; a submit does not initialize records or alter that state. No SQL or test seed is added; the browser test uses an isolated context and clears it at teardown.

Preserve T10's availability-only customer view, canonical result label, date/form validation and no live WhatsApp; T09's home/help states; T11's owner-shaped read-only lifecycle/proof/exception states; T02 scenario/version/provenance; customer-frame visual contract. Do not activate real submission, payment, status transition, account/session, admin visibility, production storage, backend fetch, or a new fixture source. Existing admin bookings and T18 media are outside this repair.

## Review Contract

| ID | Authority/type | PASS iff / required proof | Forbidden substitute / direct regression boundary |
| --- | --- | --- | --- |
| `RC-REP20-01` | User direction, T10 RC-20; `IMPLEMENTATION/NEGATIVE_CASE` | An actual browser fills and submits `/booking`, sees the T10 non-authoritative result, and finds no customer booking key in `localStorage` or `sessionStorage`, no newly generated booking ID, and no newly submitted record after reload. A source/import scan shows all browser-store functions and callers retired. | A source-only scan, cosmetic text change, or hidden fallback; preserve form validation and selected state. |
| `RC-REP20-02` | User direction, T09/T11/T02; `INTEGRATION` | In the **same isolated browser context** after submit, `/beranda` and `/booking-saya` show only their declared T02 records; no new submitted record appears in summary/list/detail or after reload. Named empty/error/lifecycle routes still show the expected fixture state. | Separate fresh contexts that cannot detect cross-route data; do not alter T02 fixture records merely to pass. |
| `RC-REP20-03` | T09–T11 visual/accessibility contracts; `REGRESSION` | Route-level desktop/narrow evidence confirms the source-derived result, home and history compositions; keyboard/focus, status semantics, no overflow and reduced motion remain usable. Exact affected visual/route checks and `npm run check` are recorded. | Build-only proof or a new visual language. Known legacy T10/T12 selector assertions belong to T21; T20's new browser proof must still pass. |
| `SEC-REP-01` | T02/T10 no-storage and Project customer-data boundary; `SECURITY` | The route-to-component scan and actual submit prove no browser booking/contact persistence, client authority, unsafe sink or cross-customer data display is added. | A test checking only an initially empty store; direct regression includes any new browser writer or misleading official state. |
| `RC-REP20-04` | User T17 dependency direction, T17 F-01 and T17-A7; `HANDOFF` | H-20 identifies the exact code commit and T02 scenario/version, retired store/import paths, executed same-context submit/storage/home/history/reload test and result, affected visual/state evidence, and an explicit F-01 closure claim limited to the browser-store boundary. It names T21 as the test-suite consumer and T17 as the independent review consumer. | Calling F-01 closed from a source scan alone, calling the old suite passed before T21, or claiming that separate customer/admin fixture identities were repaired. |

## Security readiness and UI readiness

- `SEC-REP-01 implement-now`: remove storage of booking/contact fields in the browser; required route-level negative proof is `RC-REP20-01` and `RC-REP20-02`. Scoped lens: OWASP ASVS 5.0.0 data/browser topics, OWASP Top 10: 2025 access/integrity, NIST CSF 2.0 Protect, NIST SP 800-53 AC/SC/SI. This is a ticket-bound control, not a compliance claim.
- `SEC-REP-02 preserve-seam`: keep future server-side identity/ownership/transaction and M3-W05 booking submission separate from M1 local state; inspect no new client authority path.
- Frontend state: established Soft Rose Atelier contract; Mode A, existing workflow/result and customer home/history. Preserve reference-derived structure/copy/geometry, labeled form, visible focus, status semantics, narrow reflow and reduced motion. No redesign. Use T09–T11 source ledgers and compare affected desktop/narrow captures.

## Validation and hard stop

Add a focused Playwright browser regression with completed required form inputs, submit, storage inspection, and same-context home/history/reload assertions; use fictional values only. Run that test, affected T09/T11 route tests, `npm run check`, and the affected T10 route/state checks that do not depend on the known stale selector. Record any remaining T10/T12 selector failure accurately for T21; the focused test is the explicit alternative proof for this ticket's no-write contract, not a claim that the old suite passed. Do not hand off if any T20 Review Contract row remains unproven. If fixing the route would require a real booking record or a new shared fixture architecture, stop and return that decision to planning/T19 rather than broadening this ticket.
