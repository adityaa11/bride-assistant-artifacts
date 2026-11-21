# M1-T12 — admin booking and availability source refactor

- Ticket: `M1-T12`
- State: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target code commit: `012de55417e39e20222dc3a42851f7ef7e736e36`
- Batch: `BAP-M1-FIXTURE-UI-SET-01 / 3`
- Dependencies: T02/H-02 and T03/H-03, both accepted before implementation.

## Bounded implementation

The Bookings and Availability panels at `/admin`, `/admin/availability`, and `/admin/bookings/empty` now use the mapped source's sidebar, topbar, title, controls, calendar/list/detail and availability card regions. The stylesheet carries the source palette, type, geometry and 1250px/930px/720px/485px transformations. The local extracted symbol sheet supplies source icons. Typed T02 admin fixtures supply customer, status, booking snapshot and blocked-date values. Search, date/status filters, reset, month and day navigation, list/detail/back and apparent actions are local presentation state or explicit non-authoritative feedback.

The source-to-component map, asset inventory, deliberate prototype omissions, viewport comparison and interaction observations are recorded in [the T12 reference ledger](../evidence/M1-T12-reference-ledger.md). Source and implementation captures are under [M1-T12 evidence](../evidence/M1-T12/).

## Review Contract Closure

| Row | Ticket authority and required proof | Evidence / exact validation command and outcome | Status |
| --- | --- | --- | --- |
| RC-22 | Populated, empty, filter and calendar states use canonical labels and non-authoritative controls; actual route capture and token scan; no role gate or mutation. | `npm run check` PASS; `M1_T08_BASE_URL=http://localhost:3000 M1_T12_REFERENCE_URL=http://127.0.0.1:3001 M1_T12_CAPTURE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T12 npx playwright test tests/m1-t12.visual.spec.ts` 2/2 PASS; `implementation-empty-scenario.png`, `implementation-empty-filter.png`, `implementation-status-filter.png`, `implementation-non-authoritative-result.png`, `implementation-availability-result.png`; source scan found no unsafe runtime path. | PROVEN |
| VIS-REF-M1-T12 | Source ledger and same-viewport source/implementation captures at desktop and breakpoint edges, with no material visible mismatch. | Reference ledger; 40 paired full-page captures at 1440, 1251/1250, 931/930, 721/720, 486/485 and 420px; visual spec asserts shell, title, filter, panel and calendar geometry within 2px and no horizontal overflow. `npx playwright test tests/m1-t12.visual.spec.ts` 2/2 PASS. | PROVEN |
| SRC-REF-M1-T12 | Mapped HTML/CSS/interaction source-to-component refactor, local asset, typed T02 props, no raw prototype execution. | Reference ledger source-to-component map; `shared/admin-booking-workspace.tsx`, `shared/admin-booking-workspace.module.css`, typed `shared/scenarios/{contracts,catalogue}.ts`; local audited inline-SVG symbol sheet; source scan and `npm run check` PASS. | PROVEN |
| SEC-M1-01/02 | Preserve server admin/transaction seams and use safe framework rendering; no role decision, persistence, prototype storage, unsafe HTML, inline execution or unreviewed origin. | `npm run check` PASS, including review-build source test; `rg -n 'dangerouslySetInnerHTML|innerHTML|localStorage|onclick=|<script|eval\(' shared/admin-booking-workspace.tsx shared/admin-booking-workspace.module.css shared/scenarios/catalogue.ts shared/scenarios/contracts.ts tests/m1-t12.visual.spec.ts` returned no matches. | PROVEN |
| VIS-003/06/08/10/11 | Admin data workspace hierarchy, readable type, canonical status text, responsive fit, keyboard/focus and state feedback. | Paired captures plus `implementation-focus.png`; Playwright list/detail/filter/month/result assertions; lint/typecheck/build PASS. | PROVEN |

Internal readiness: READY_FOR_CK. This checkpoint does not self-issue CK PASS.

## Actual Use Path

`app/(admin)/admin/page.tsx` → `AdminBookingWorkspace("admin.booking-workspace")` → `getScenario` T02 adapter → typed `adminBooking` read model → `Calendar` / `List` / `SideList` / `Detail` → reviewer browser capture. `app/(admin)/admin/availability/page.tsx` follows the same adapter path into `Availability`. `app/(admin)/admin/bookings/[state]/page.tsx` selects the named empty fixture. The apparent action consumer is local `notice` with `role="status"`; no record or availability write occurs. The Playwright spec executes each route and interaction chain. Mismatch from frozen expected path: none.

## Actual Empty-State Evidence

Clean fixture condition: `admin.booking-empty` has zero booking records and blocked dates. `/admin/bookings/empty` renders the same reference-derived shell and explicit no-result panel; `implementation-empty-scenario.png` captures it. A no-match customer query on populated `/admin` renders the list no-result state in `implementation-empty-filter.png`. Reset restores the named fixture view; no seed or initialization write occurs.

## Migration Inventory

No migration or production-data change is authorized or made. Playwright uses fresh browser contexts and typed in-memory fixtures; no test seed SQL or teardown is required.

## Limitations within the frozen boundary

The reference's `Confirmed`, `Requested`, `On Hold`, `Completed` and `Cancelled` prototype statuses are replaced with canonical T02 labels. Reference storage, admin mutation and production availability logic are excluded. The fixture provenance footer sits below the source composition so it remains explicit without changing its panel geometry.
