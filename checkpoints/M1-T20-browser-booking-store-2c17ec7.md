# H-20 — browser booking store removal

- Ticket: `M1-T20`, batch `BAP-M1-FIXTURE-REPAIR-01 / 1`; state: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Exact code commit: `2c17ec7cf247984a88a2bb1cf90e0a318ef5e4c7` (`fix: remove browser booking fixture store`).
- T02 adapter version: `m1-fixture-v1`; `/booking`: `customer.booking-request-default` and named invalid, unavailable, no-package, result states; `/beranda`: `customer.home-success` and `customer.home-empty`; `/booking-saya`: `customer.booking-history-success` and named empty, loading, error, payment, cancellation, cancelled, reschedule states.
- Handoff consumers: M1-T21 test-suite repair and independent M1-T17-A7 re-review, each under its own authorization. M3-W05 remains the real booking-submission successor.

## Bounded change

`shared/scenarios/customer-booking-store.ts` was deleted. `CustomerBooking` no longer reads or writes a browser booking record, creates an ID, or derives a package snapshot from submitted form data. It validates local input and shows an explicitly simulated `Menunggu Konfirmasi` result. `CustomerHome` and `CustomerBookingHistory` now read only their declared T02 `ScenarioResult` records. Result, hero, note, and form copy no longer claim a request was received, saved, or added to another route. Existing component structure, customer chrome, form controls, route names, and CSS were preserved. The named `/booking/result` assertion was updated for the corrected result heading; the known stale date-selection selector remains for T21.

## Review Contract Closure

| Row | Ticket authority and PASS condition | Required proof and evidence locator | Exact validation and outcome | Status |
| --- | --- | --- | --- | --- |
| `RC-REP20-01` | M1-T20 Review Contract; actual submit renders only a local result, with no new ID, record, or local/session storage entry; store callers retired. | `tests/m1-t20.browser.spec.ts` fills all required fields with fictional values, selects 12 December 2026, submits, checks live result, no `BR-2026-003`, no input echo, and empty storage; source/import scan and deleted module in code commit. | `npx playwright test tests/m1-t20.browser.spec.ts`: 1/1 PASS. `rg` scan for store symbols/key and browser storage in `app shared`: no match. | `PROVEN` |
| `RC-REP20-02` | M1-T20 Review Contract, T02/T09/T11; same isolated context keeps only declared records across home/history/reload and named states. | Same focused spec creates one context and uses its page for submit, home, history, reload, both empty routes; checks `BR-2026-001/002`, count `2 booking`, no `BR-2026-003` or fictional name, and empty storage. T09/T11 specs cover named home/help and history lifecycle/error routes. | Focused spec 1/1 PASS; `npx playwright test tests/m1-t20.browser.spec.ts tests/m1-t09.visual.spec.ts tests/m1-t11.visual.spec.ts tests/m1-t10.visual.spec.ts --grep-invert 'keeps date selection, focus, and submission'`: 20/20 PASS. | `PROVEN` |
| `RC-REP20-03` | M1-T20 Review Contract, T09–T11 visual/accessibility; affected route/state, desktop/narrow, focus, status, overflow, reduced motion and project checks. | T09/T10/T11 source-reference route tests passed. Focused spec asserts skip-link keyboard focus, live result/status, 640px overflow absence, reduced-motion preference, and captures six desktop/narrow images in `evidence/M1-T20/`. Visually inspected result captures: hierarchy and wrapping remain readable; no material change to layout. | 20/20 affected Playwright PASS; final focused run 1/1 PASS; `npm run check`: lint, typecheck, 13 Node tests, build PASS. | `PROVEN` |
| `SEC-REP-01` | M1-T20 security row; no browser booking/contact persistence, client authority, unsafe sink or cross-customer data display. | Same-context storage assertions before/after submit and after each route/reload; no submitted name or generated ID in customer views; production source/import scan confirms no store writer, listener, unsafe HTML sink, or browser storage call in `app shared`. T02 provenance remains fixture. | Focused spec 1/1 PASS; `rg -n 'customer-booking-store|readCreatedBookingFixtures|saveCreatedBookingFixture|bookingFixtureId|useCustomerBookingRecords|ranur.customer-booking-fixtures|localStorage|sessionStorage|dangerouslySetInnerHTML' app shared --glob '!*.css'`: no match. | `PROVEN` |
| `RC-REP20-04` | M1-T20 H-20, T17 F-01; exact commit, scenario/version, retired path, browser and visual/state evidence, bounded closure and consumers. | This checkpoint and code commit; focused browser spec; `evidence/M1-T20/` screenshots; T09/T10/T11 route checks. F-01 is closed only for the browser booking-store boundary. Separate customer/admin fixture identity is not claimed repaired. | Evidence and code binding above; affected 20/20 PASS and `npm run check` PASS. | `PROVEN` |

Internal readiness: READY_FOR_CK. This is an implementer readiness statement, not CK `PASS`.

## Actual Use Path

- `EUP-20-A`: `app/(customer)/booking/page.tsx` and `app/(customer)/booking/[state]/page.tsx` pass compile-time scenario IDs → `getScenario` in `shared/scenarios/adapter.ts` → `CustomerBooking` in `shared/customer-booking.tsx` → local form validation/state → live result region and reviewer. `tests/m1-t20.browser.spec.ts` exercised the real route and form. No server endpoint, event, record writer, or storage call follows submit.
- `EUP-20-B`: `app/(customer)/beranda/page.tsx`, `app/(customer)/beranda/[state]/page.tsx`, `app/(customer)/booking-saya/page.tsx`, and its state page pass declared IDs → T02 `getScenario` → `CustomerHome` or `CustomerBookingHistory` → T02 booking-history records or named empty/error/lifecycle state → reviewer. The focused spec navigated these routes in the same context after submit; T09/T11 route specs exercised their state matrix.
- Superseded path: all three production imports of `customer-booking-store` were removed and the module deleted. A production `app shared` scan returned no old function/key/browser storage reference. No fallback or compatibility branch remains. No expected-use-path mismatch was found.

## Actual Empty-State Evidence

In the focused isolated browser context, `/beranda/empty` and `/booking-saya/empty` showed `Belum ada booking, dear.` after the local submit and again after reload. No storage entry appeared and no initialization occurred. This is the named T02 zero-booking path, with no hidden seed or first-use handoff.

## Migration Inventory

No SQL migration, production-data change, durable seed, or test storage was introduced; M1-T20 marks SQL not applicable. The Playwright spec creates an isolated browser context and closes it in `finally`, including on assertion failure.

## Validation limits and T21 handoff

The old T10 submit test was run separately and still fails at `tests/m1-t10.visual.spec.ts:94`: it expects `aria-pressed="true"` on the selected date button and receives no attribute. Its later old heading assertion is also stale after the corrected non-authoritative copy and was not reached. The old T12 admin controls test was run separately and still fails at `tests/m1-t12.visual.spec.ts:82`: unscoped `getByRole('status')` resolves to both calendar caption and result toast. These are the named T21 selector/assertion repairs. Neither failure is counted as a passing suite or as proof of T20's no-write behavior; the focused browser regression supplies that ticket-defined proof.
