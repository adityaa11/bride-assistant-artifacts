# M1-T11 — Booking Saya fixture views

- Ticket: `M1-T11`
- Batch/milestone/work package: `3 / M1 / M1-W05`
- State: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target commit: `6ec49b8d64be975d39b0970ed359aa3df672c1ea` (`feat: add Booking Saya fixture views`)
- Ticket authority: [M1-T11](../tickets/m1-fixture-ui-assembly/M1-T11-booking-saya-fixtures.md), A1–A4, and the mapped `booking_saya.html` source contract.

## Bounded implementation

- Added `/booking-saya` plus named fixture-state routes for loading, empty, error, payment, cancellation, cancelled, and reschedule views.
- Refactored the mapped Booking Saya intro, overview tiles, empty composition, search/filterable list, selected detail, lifecycle message/timeline, event/package/customer detail, actions, and responsive layouts into typed React components and CSS modules.
- Added typed, owner-shaped T02 fixture records for the six canonical customer lifecycle labels. The undefined rejection label is not rendered or offered.
- Reused the A4-approved `CustomerFrame`; its shared customer chrome/footer remains outside the booking-specific visual correction boundary.
- Replaced the booking request result's plain internal link with `next/link` because the newly introduced `/booking-saya` route makes it an in-app route.
- No reference script, browser storage, clipboard write, upload, live WhatsApp link, external origin, persistence, identity/session, lifecycle mutation, cancellation, or reschedule submission runs in the application.

## Reference ledger and source-to-component map

| Source region / CSS / interaction | Converted destination | T02 fixture or React state | Asset / omission |
| --- | --- | --- | --- |
| Customer header, customer navigation, notification/profile/menu behavior, and footer | `CustomerFrame` in `shared/customer-home.tsx` | Shared local component state; `scenarioId` data attributes | A4-approved shared reference-derived customer chrome; no external assets added. |
| `.portal-intro`, breadcrumb, eyebrow, heading, copy, `intro-actions` | `CustomerBookingHistory` intro and `customer-booking-history.module.css` | Declared T02 scenario only | No page-specific bitmap artwork; source decorative star is CSS content. |
| `.overview`, `.overview-tile`, calendar/clock/check SVGs | `Overview` | `bookingHistory.records` totals | Source inline SVG geometry translated into JSX; no runtime source SVG/HTML. |
| `.empty-view`, `.empty-icon`, mapped button | `StatePanel` | `customer.bookings-empty`, loading/error states | Source empty-state SVG translated into JSX; no hidden seed or initialization. |
| `.main-layout`, `.list-panel`, search/filter/list-item status treatment | list composition and module rules | React `query`, `filter`, `selectedId`; typed records | Source search/filter/selection behavior translated to local React state, not storage. |
| `.detail-hero`, state message, timeline, info grid, package and notes regions | `BookingDetail`, `Timeline`, `DetailSection`, `Info` | typed record fields and canonical status | Prototype `innerHTML`, local storage, WhatsApp and copy-to-clipboard behavior omitted because ticket forbids authority/external/live behavior. |
| 1050px list/detail rebalance; 820px stacked list/detail/mobile chrome; 640px tile/detail/action reflow | module media queries | all fixture states | Captured at desktop plus 1051/1050, 821/820, and 641/640. |

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / validation | Status |
| --- | --- | --- | --- | --- |
| RC-21 | M1-T11 Review Contract | Every canonical lifecycle state, no-booking, proof/exception placeholder, loading, and error state is inspectable with no official change; rejection remains undefined and omitted. | Typed history fixtures and named state routes; `tests/m1-t11.visual.spec.ts` passed 9/9, including lifecycle routes, empty/loading/error, selection/filter, result, keyboard menu, and storage/link boundary checks. | PROVEN |
| VIS-REF-M1-T11 | M1-T11-A2 | Same-viewport source/implementation captures at desktop and every listed breakpoint edge; no material visual mismatch. | Captures under [`evidence/M1-T11`](../evidence/M1-T11/), inspected side-by-side at desktop and 1051/1050, 821/820, 641/640. Empty reference shell, tiles, empty panel geometry, icon treatment, footer alignment, and responsive reflow match. | PROVEN |
| SRC-REF-M1-T11 | M1-T11-A3 | Every mapped region/CSS/asset/interaction has a source-to-component map; route uses converted components and T02 fixtures, not prototype runtime. | Ledger above; `shared/customer-booking-history.tsx`, CSS module, declared T02 catalogue entries, route callers, and source scan in `tests/workspace-boundaries.test.mjs`. | PROVEN |
| SEC-M1-01 / SEC-M1-02 | Ticket security boundary | Owner-shaped fixture only; safe rendering; no upload/mutation/cross-customer data/prototype storage/unsafe sink/external origin/client authority. | Targeted T11 browser assertion found empty local/session storage and local/hash-only anchors; workspace boundary test rejects storage, unsafe sink, live WhatsApp, and fetch in the T11 component; `npm run check` passed. | PROVEN |

Internal readiness: READY_FOR_CK

## Actual Use Path

Committed caller: `app/(customer)/booking-saya/page.tsx` → `CustomerBookingHistory` → `getScenario(scenarioId)` → typed `scenarioCatalogue` customer `bookingHistory.records` → React overview/list/detail/status composition → local selection/filter/result state → reviewer-visible fixture UI. `app/(customer)/booking-saya/[state]/page.tsx` maps only declared state segments to the same component and typed scenario IDs.

The booking-request result uses the committed `/booking-saya` navigation route. No production caller imports raw scenario objects. The reference's localStorage/sessionStorage lookup, innerHTML list construction, clipboard write, live WhatsApp branch, and status mutation paths are not reachable; their visible list/detail/filter/menu behavior is the converted React path.

## Migration Inventory

Not applicable: this ticket authorizes no persistence, schema, migration, production-data, seed, test database, or migration runner.

## Actual Empty-State Evidence

`customer.bookings-empty` returns zero typed records through the same route → adapter → `CustomerBookingHistory` path. `/booking-saya/empty` renders the reference-derived no-bookings panel and initializes nothing. The browser capture and test prove no local/session storage is written; static scenario retrieval is idempotent.

## Validation

- `npm run check` — PASS: lint, TypeScript, 10 Node boundary tests, and production build all passed.
- `M1_T11_BASE_URL=http://localhost:3007 M1_T11_REFERENCE_URL=http://127.0.0.1:3001 npx playwright test tests/m1-t11.visual.spec.ts` — PASS: 9/9.
- Same-viewport reference/implementation captures for source empty state and implementation state captures are committed under `evidence/M1-T11/`.
- `git diff --check` — PASS before source commit.
