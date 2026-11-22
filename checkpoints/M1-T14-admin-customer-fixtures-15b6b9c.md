# M1-T14 — admin customer fixture directory and detail

- Ticket: `M1-T14`; batch `BAP-M1-FIXTURE-UI-SET-01 / 3`; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target code commit: `15b6b9c68fa74e0502a0acced13fca120d7185d8` (`feat: render admin customer fixture workspace`).
- Dependencies T02/H-02 and T03/H-03 have CK `PASS` artifacts. Neither predecessor was reopened.
- Source ledger: `docs/frontend/M1-T14_REFERENCE_LEDGER.md` at the code commit. Captures, measurements, and comparison: [M1-T14 evidence](../evidence/M1-T14/comparison.md).

## Bounded implementation

`/admin/customers` renders the reference-derived customer directory and selected booking history inside `AdminWorkspaceFrame`. Named empty/error routes and local search expose required states. A route-local server projection takes display-safe fields from the versioned T02 `admin.booking-workspace` scenario, preserving its canonical status and non-authoritative provenance. The shared frame's Customers link now reaches this route. A history row opens the selected read-only booking fixture. No customer creation, editing, lookup, persistence, or role decision is implemented.

## Review Contract Closure

| Row | Ticket authority and pass condition | Required proof; evidence / exact command and outcome | Status |
| --- | --- | --- | --- |
| RC-24 | Frozen ticket: route-local fixture list/detail/empty/error and search; no public/customer import of customer model or hidden private data. | Admin server projection at `app/(admin)/admin/customers/scenarios.ts`; `shared/admin-customer-workspace.tsx`; named route/state browser captures and boundary test. `npm run check` PASS (13 tests, lint, typecheck, build); `node scripts/t14-visual-evidence.mjs` PASS (exit 0). | PROVEN |
| VIS-REF-M1-T14 | A2: source ledger, paired same-viewport desktop and boundary captures, named states, keyboard/security evidence, no material in-scope mismatch. | Ledger and `evidence/M1-T14/comparison.md`; 1440, 1251/1250, 931/930, 486/485 source/app directory and detail pairs; `measurements.json`, search/empty/error/focus/drawer captures. `node scripts/t14-visual-evidence.mjs` PASS. The recorded differences follow A4 shell, ticket search, and T02 canonical data authority. | PROVEN |
| SRC-REF-M1-T14 | A3/A5: every route-specific region/label/icon/style/breakpoint/interaction traces to source, frame, or fixture; no prototype execution or invented substitute. | `docs/frontend/M1-T14_REFERENCE_LEDGER.md` maps regions, CSS, SVG roles, fixture fields and React interactions; source scan in comparison; `npm run check` PASS. | PROVEN |
| A4-SHARED | A4: content composes inside shared frame, with no shell duplication or route CSS override; shared regressions at desktop/930/720/485. | Frame props in customer component; one central navigation destination edit. `shell-{bookings,availability,customers}-{1440,930,720,485}.png`, no-overflow assertions, and drawer/Escape check from `node scripts/t14-visual-evidence.mjs` PASS. | PROVEN |
| VIS-002/06/08/10/11 | Frozen UI: Data Workspace hierarchy, readable responsive cards/history, text states, keyboard focus/menu, reduced motion. | Source/app pairs, `app-keyboard-focus.png`, `app-mobile-drawer.png`, empty/error/search captures; browser assertions for Enter selection, history link, drawer Escape, no overflow; visual harness PASS. | PROVEN |
| SEC-M1-01/02 | Preserve server role/ownership seam, safe rendering and no live customer authority, export/edit, unsafe sink, external origin, or shared client store. | Route-local server projection; client receives only display fields; boundary test and negative source scan PASS; fixed read-only route links and React text rendering. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

`/admin/customers` server page → `getCustomerScenario("admin.customers-directory")` → T02 `getScenario("admin.booking-workspace")` and version/provenance → route-local display-safe projection → client `AdminCustomerWorkspace` → `AdminWorkspaceFrame` → directory/search/detail/empty/error → reviewer. The selected history row reaches `/admin/bookings/detail?booking=BA-005` → `AdminBookingWorkspace(initialSelection)` → read-only booking detail. Empty/error state pages call the same projection with zero records. The Playwright harness executes selection, search, state routes, keyboard, drawer, and the booking handoff. Superseded prototype customer rendering, localStorage and creation handlers are unreachable from application routes.

## Actual Empty-State Evidence

`admin.customers-empty` projects zero records. `/admin/customers/empty` shows the source-derived empty treatment with the scenario message and no customer cards; `app-empty.png` captures the first handoff. `/admin/customers/error` also projects zero records and shows an alert (`app-error.png`). Repeated reads are idempotent; there is no seed, initialization, or hidden default. A no-match search shows the same empty treatment with no mutation.

## Migration Inventory

No SQL migration, production-data change, seed, or durable test setup is authorized or present. The browser harness creates and closes isolated contexts; no test database or persistence path is used.

## Limits and handoff

The customer records are fixture-only and do not confer admin permissions. H-14 carries scenario/version and the display-safe fields for later protected reads; role and record ownership remain M2/M3 work. The source prototype's narrow navigation is superseded by A4, and its `Requested` badge text is replaced by the T02 canonical field as recorded in the visual comparison.
