# M1-T10 — Customer booking-request fixture workflow

- Ticket: `M1-T10-customer-booking-request-fixtures`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target commit: `505f393038ac2b391070aaee2a2d7652667aee48` (`feat: add customer booking request fixture`)
- Ticket state: `awaiting_review`
- Dependencies consumed: T02/H-02 (`77b4a2bbd095b4d8b7772fbbca0af31169d53722`, CK PASS) and T03/H-03 (`d08a772a3ab52ecde64863e9d916cb8ddd799718`, CK PASS)

## Bounded implementation

The active fixture route is `/booking`, with review states `/booking/invalid`,
`/booking/unavailable`, `/booking/no-package`, and `/booking/result`. The
route calls the T02 adapter with a declared `ScenarioId`; the converted local
workflow renders package, availability-only date, form, validation, empty,
unavailable, and non-authoritative result states. Selection and submit are
React-local review state only. No record, account, session, storage, external
link, date block, package snapshot, or official transition is created.

## Source-to-component map and reference ledger

| Reference region / source rule | Converted destination | Fixture / interaction boundary |
| --- | --- | --- |
| `booking.html` customer chrome, brand, navigation, notification/profile/mobile-menu interactions | Existing reference-derived `CustomerFrame` in `shared/customer-home.tsx` | Declared scenario ID/version remains on the frame; menu/popover state is local React UI state. |
| `booking.html` breadcrumb, hero, workflow steps, calendar card/legend, and desktop/narrow grid rules | `shared/customer-booking.tsx`, `shared/customer-booking.module.css` | Adapter-supplied availability exposes only date state; local month/date selection has no availability authority. |
| `booking.html` detail form, summary, status chip, and success panel | `shared/customer-booking.tsx`, `shared/customer-booking.module.css` | T02 package/result fields only; submit changes only visible fixture state. |
| `booking.html` visible form validation, unavailable, and no-package extensions | Same local converted components | Named T02 scenarios produce the extension message/status in the reference shell. |
| Header logo asset in mapped reference | `public/reference-assets/ui-reference/home-image-01-5cfb309f414c.webp` | Manifest source asset is the same extracted image hash as `booking-image-01-5cfb309f414c.webp`; no external asset is loaded. |
| Reference inline scripts, prototype storage/seeds, booking write, WhatsApp handoff | Intentionally omitted | Replaced by React handlers and typed T02 fixtures; no raw executable source runs. |

Desktop ledger at 1280px: customer chrome → breadcrumb/hero/steps → two-column
calendar/form workflow → footer. The calendar and form panels retain their
source geometry, typography, colours, borders, controls, and source order.
The source 1050px, 1000px, 820px, and 640px breakpoint edges are carried into
the local grid, customer navigation, compact calendar cells, and one-column
form rules.

## Visual comparison and evidence

- Same-viewport source and implementation captures at desktop, 1051/1050,
  1001/1000, 821/820, and 641/640 are under
  [`evidence/M1-T10`](../evidence/M1-T10/).
- Source captures: `reference-booking-{desktop,1051,1050,1001,1000,821,820,641,640}.png`.
- Implementation captures: `implementation-booking-{desktop,1051,1050,1001,1000,821,820,641,640}.png`.
- Named state/interaction captures: `state-booking-{invalid,unavailable,no-package,result}.png` and `implementation-booking-selected-result.png`.
- Comparison result: the reference region ordering, header and form/calendar
  geometry, copy, typography, palette, navigation, controls, compact/mobile
  transformations, visible focus path, and horizontal-overflow checks passed;
  no material visible mismatch remains in the captured review surface.

## Actual Use Path

`app/(customer)/booking/page.tsx` → `CustomerBooking` in
`shared/customer-booking.tsx` → `getScenario("customer.booking-request-default")`
→ T02 `scenarioCatalogue` typed booking-request model → converted calendar and
form components → visible fixture workflow. State routes map only declared
scenario IDs through `app/(customer)/booking/[state]/page.tsx`. Evidence is
the executed Playwright route/state run below. The production path is not a
unit-test-only artifact.

## Migration Inventory

Not applicable. This ticket authorizes no persistence, production-data change,
schema migration, seed, or database test setup.

## Actual Empty-State Evidence

`/booking/no-package` invokes `customer.booking-request-no-package`, whose
typed model contains no packages and whose rendered form exposes the explicit
no-action message. The Playwright state capture proves the route response.
No default package, seed, or initialization mutation is created.

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / command and outcome | Status |
| --- | --- | --- | --- | --- |
| RC-20 | Ticket Review Contract | Required, invalid, selected, unavailable, no-package, and success-result states render through T02; success stays non-authoritative; no storage/official transition. | `tests/m1-t10.visual.spec.ts`: six Playwright tests passed, including every named route, selection, submit result, storage-empty assertion, and local-only links. | PROVEN |
| VIS-REF-M1-T10 | M1-T10-A2 | Ledger plus same-viewport reference/implementation captures at desktop and each named breakpoint; no material mismatch. | `npx playwright test tests/m1-t10.visual.spec.ts` passed 6/6; captures in `evidence/M1-T10/`. | PROVEN |
| SRC-REF-M1-T10 | M1-T10-A3 | Mapped source regions/CSS/assets/interactions are converted to components and T02 fixtures; no prototype runtime. | Source-to-component map above; `shared/customer-booking.tsx`, `shared/customer-booking.module.css`, local audited reference asset; unsafe/prototype scan included in `npm run check` boundary suite and targeted scan. | PROVEN |
| SEC-M1-01 / SEC-M1-02 | Ticket security boundary | Preserve future ownership/transaction seam; safe rendering; no unsafe sink, inline executable source, storage, external media, or client authority. | `npm run check` passed; existing boundary test passed; T10 Playwright assertion verified `localStorage`/`sessionStorage` empty and every anchor local/hash-only. | PROVEN |
| H-10 / Actual use path | Ticket outcome and handoff | Declared scenario/version, selected display state, result state, and M3 successor seam without retry/persistence/idempotency claim. | Route → adapter → local workflow → reviewer path recorded above; `ScenarioResult` provenance remains fixture-only. | PROVEN |

## Validation

- `npm run check` — passed: ESLint, TypeScript, Node boundary suite (9/9), and production build.
- `M1_T10_BASE_URL=http://localhost:3004 npx playwright test tests/m1-t10.visual.spec.ts` — passed 6/6 against the production build, with reference served at `http://127.0.0.1:3001`.
- `git diff --check` — passed before source commit.

Internal readiness: READY_FOR_CK
