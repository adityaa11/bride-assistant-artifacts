# H-21 — browser selector regression repair

- Ticket: [M1-T21](../tickets/m1-fixture-review-repairs/M1-T21-browser-selector-regression.md), batch `BAP-M1-FIXTURE-REPAIR-01 / 2`; state: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`; exact code commit: `79b3478995737dd73165afaba50369dcb5242a08` (`test: repair M1 booking browser assertions`).
- Predecessor: [H-20](M1-T20-browser-booking-store-2c17ec7.md) at `2c17ec7cf247984a88a2bb1cf90e0a318ef5e4c7`, [CK PASS](../feedback/M1-T20-browser-booking-store-2c17ec7-ck.md). The T20 code commit was this ticket's parent.
- Handoff: M1-T17-A7 GO re-review after this checkpoint receives CK `PASS`. This checkpoint does not issue T17 or M1 `PASS`.

## Bounded change

`tests/m1-t10.visual.spec.ts` selects 12 December 2026 on `/booking`, asserts the date button's rendered `Dipilih` state and the form's `Tanggal Wedding *` value, fills fictional required fields, then checks the local `Hasil pengajuan booking ♡` result, its no-send/no-save wording, absence of the fictional name and `BR-2026-003`, empty browser storage, and local links. Its obsolete `aria-pressed` and success-claim assertions are retired. `tests/m1-t12.visual.spec.ts` targets the `New Booking tidak diterapkan` and `Save availability tidak diterapkan` status messages. Its list, detail, filter, calendar, empty and non-authoritative checks remain. No production code, fixture data, persistence, SQL or UI was changed.

## Review Contract Closure

| Row | Ticket authority and PASS condition | Required proof / evidence locator | Exact validation and outcome | Status |
| --- | --- | --- | --- | --- |
| `RC-REP21-01` | T21/T10 RC-20: actual booking route selects named date, proves rendered selected state and form date, completes fictional form, reaches local result, retains storage and no-new-record proof. | `tests/m1-t10.visual.spec.ts:83` assertions; `tests/m1-t20.browser.spec.ts:20` same-context submit → home/history/reload/empty proof. | Focused `npx playwright test tests/m1-t10.visual.spec.ts tests/m1-t12.visual.spec.ts --grep "keeps date selection\|admin controls"`: 2/2 PASS. Full command below: T10 and T20 PASS. | `PROVEN` |
| `RC-REP21-02` | T21/T12 RC-22: actual admin actions target exact non-authoritative toast while preserving list/filter/calendar/empty observations. | `tests/m1-t12.visual.spec.ts:59`, including scoped role/status match at lines 82 and 90; same test's route and state checks. | Focused 2/2 PASS; full command below: both T12 tests PASS. | `PROVEN` |
| `RC-REP21-03` | T21/T17 RC-27: affected T08–T13/T16 route suite, same-context T20 regression and T17 route/scenario matrix are rerun; failures stated exactly. | Full Playwright run below; route/scenario matrix below, 55 routes × 2 viewports. Prior 30-second T12 timeout and successful rerun are disclosed below. | Full command below: 42/42 PASS. Read-only Chromium sweep: 110/110 PASS. `npm run check`: lint, typecheck, 13 Node tests and build PASS. | `PROVEN` |
| `RC-REP21-04` | T21 H-21: bind exact commit and T20 CK, T10/T12 observations, F-01/F-02 regression, route/scenario evidence and remaining T17 findings without self-issued PASS. | This checkpoint, code commit, T20 CK, [T17 v1 F-01/F-02](../evidence/M1-T17-v1.md), [H-19 candidate audit](../evidence/M1-T19-fixture-relationship-audit-2ee952c.md), route matrix and finding ledger below. | Evidence reconciliation against exact source and executed commands below: PASS for T21 handoff; T17-A7 is not executed here. | `PROVEN` |

Internal readiness: READY_FOR_CK. This is implementer readiness, not CK `PASS`.

## Actual Use Path

`EUP-21`: Playwright's T10 spec → production-like Next dev `/booking` → `CustomerBooking`/`BookingCalendar` selected `Dipilih` button and form date → local result, storage and link assertions → test report → T17 reviewer. Playwright's T12 spec → `/admin`, `/admin/bookings/empty`, `/admin/availability` → `AdminBookingWorkspace` list/filter/calendar/toast → exact status-message assertions → report → T17 reviewer. The T20 spec uses one isolated browser context across `/booking`, `/beranda`, `/booking-saya`, reloads and empty routes, then closes the context. All paths executed in the full run. No endpoint or production caller was added. The superseded test expectations (`aria-pressed="true"`, success-claim heading, unscoped `getByRole("status")`) are absent from these tests; no fallback or `first()` toast workaround remains.

## Validation and route/scenario matrix

- App server: `npm run dev -- --hostname 127.0.0.1 --port 3000`, Next.js `16.4.6`; Playwright Chromium, `baseURL` `http://localhost:3000` per `playwright.config.ts`; mapped local reference server `http://127.0.0.1:3001`. T10/T12 specs visit `booking.html` and `Bride_Assistant_Planner_Standalone_v3_Content_Management.html` at that reference origin.
- Focused selector command: `npx playwright test tests/m1-t10.visual.spec.ts tests/m1-t12.visual.spec.ts --grep "keeps date selection|admin controls"` — **2/2 PASS**. T10 selected 12 December 2026, rendered `Dipilih`, form value matched, all required fictional fields submitted, local no-save result appeared, name/`BR-2026-003` absent, both storage lengths zero, and every link was local. T12 observed the exact `New Booking tidak diterapkan` and `Save availability tidak diterapkan` toasts alongside its list/filter/calendar/empty checks.
- First full command without timeout override: `npx playwright test tests/m1-t08.visual.spec.ts tests/m1-t09.visual.spec.ts tests/m1-t10.visual.spec.ts tests/m1-t11.visual.spec.ts tests/m1-t12.visual.spec.ts tests/m1-t13.visual.spec.ts tests/m1-t16.visual.spec.ts tests/m1-t20.browser.spec.ts` — **41 PASS, 1 FAIL**. The T12 reference-shell geometry test exceeded its 30-second test timeout while navigating to `/admin/availability` with `waitUntil: "networkidle"`; no geometry assertion failed. Standalone `npx playwright test tests/m1-t12.visual.spec.ts --grep "preserve reference shell" --timeout 90000` — **1/1 PASS**, 30.7 seconds.
- Final full command: `npx playwright test tests/m1-t08.visual.spec.ts tests/m1-t09.visual.spec.ts tests/m1-t10.visual.spec.ts tests/m1-t11.visual.spec.ts tests/m1-t12.visual.spec.ts tests/m1-t13.visual.spec.ts tests/m1-t16.visual.spec.ts tests/m1-t20.browser.spec.ts --timeout 90000` — **42/42 PASS** in 3.6 minutes. T08 12, T09 4, T10 6, T11 10, T12 2, T13 4, T16 3 and T20 1 passed. The override accommodates T12's full responsive loop; it does not skip or relax assertions.
- `npm run check` — **PASS**: ESLint, TypeScript, 13 Node tests, Next production build.
- Read-only T17/T19-style route sweep: `node scripts/.t21-route-review.mjs` with a temporary Playwright Chromium script, removed after execution, visited each route below at **1440×900 and 485×900**, reduced motion, using `page.goto(..., { waitUntil: "domcontentloaded" })`. Every visit returned HTTP 200, exposed a nonempty `data-scenario-id` and `data-scenario-version="m1-fixture-v1"`, and had no document horizontal overflow: **55 routes, 110/110 visits PASS**. The named browser specs above provide source/reference breakpoint and interaction evidence; this sweep does not claim T17's full re-review.

| Surface | Routes and scenario IDs (`m1-fixture-v1` throughout) | Desktop/narrow |
| --- | --- | --- |
| Public | `/` `public.discovery-success`; `/fixtures/{empty,error}` `public.catalogue-{empty,error}` | 6/6 PASS |
| Customer auth | `/daftar`, `/daftar/{validation,loading,error,success}` `customer.register-*`; `/masuk`, `/masuk/{validation,loading,error,success}` `customer.signin-*` | 20/20 PASS |
| Home/help | `/beranda` `customer.home-success`; `/beranda/empty` `customer.home-empty`; `/bantuan` `customer.whatsapp-unavailable` | 6/6 PASS |
| Booking request | `/booking` `customer.booking-request-default`; `/booking/{invalid,unavailable,no-package,result}` matching `customer.booking-request-*` | 10/10 PASS |
| Booking Saya | `/booking-saya` `customer.booking-history-success`; `/booking-saya/{loading,empty,error,payment,cancellation,cancelled,reschedule}` `customer.booking-loading`, `customer.bookings-empty` or matching `customer.booking-history-*` | 16/16 PASS |
| Admin bookings | `/admin` `admin.booking-workspace`; `/admin/bookings/empty` `admin.booking-empty`; `/admin/availability` `admin.availability-workspace` | 6/6 PASS |
| Admin catalogue/packages | `/admin/catalogue` and its `{empty,error,proposed-result}` states `admin.catalogue-*`; `/admin/packages` and its `{empty,error,proposed-result,long}` states `admin.catalogue-*` | 18/18 PASS |
| Admin customers | `/admin/customers` and `{empty,error}` states `admin.customers-*` | 6/6 PASS |
| Admin content/events | `/admin/content`, `/admin/events` and each `{empty,error}` state `admin.content-draft-success` or `admin.content-{empty,error}` | 12/12 PASS |
| Admin promotions | `/admin/promotions` and `{empty,error,long,result}` states `admin.promotions-*` (`result` is `admin.promotions-proposed-result`) | 10/10 PASS |

## T17 finding handoff

- **F-01:** T20 CK `PASS` closed the browser-store violation at its exact code checkpoint. T21's full run again passed the T20 same-context actual submit → home/history/reload/empty proof, with no storage entry or new booking record. T17-A7 must independently recheck at its resulting code checkpoint.
- **F-02:** The two stale browser assertions are corrected and the full T08–T13/T16 suite now passes. T17-A7 must independently recheck selected-date/form state and exact admin toast; this checkpoint does not relabel T17 v1.
- **H-19 `FRA-03`:** Same F-01 browser-store boundary, now T20 CK-approved and regression-proven here. **`FRA-01`, `FRA-02`, `FRA-04`:** still unknown/planning-required for M1 disposition; no fixture relation or policy was added by T21. **`FRA-05`, `FRA-06`:** H-19 records accepted M2+ deferrals with named M3/M4 successors. T17-A7 must consume the finding-by-finding disposition and any separately authorized M1 repairs; no T17 `PASS` is claimed here.

## Migration Inventory and initial state

No migration, production data, seed or durable test state was added. Playwright used isolated browser contexts. The route sweep read named empty routes, and T20's same-context test proved `/beranda/empty` and `/booking-saya/empty` remained empty after submit and reload, with zero browser storage. No initialization or first-use data handoff occurred.
