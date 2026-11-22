# M1-T15 — admin content publication fixture workspace

- Ticket: `M1-T15`; batch `BAP-M1-FIXTURE-UI-SET-01 / 3`; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target code commit: `2e4673e3e26e31099ef9b1a4b18bb7dfe671b4eb` (`feat: render admin content publication fixtures`).
- Dependencies T02/H-02 and T03/H-03 have CK `PASS` artifacts. Neither predecessor was reopened.
- Source ledger: `docs/frontend/M1-T15_REFERENCE_LEDGER.md` at the code commit. Captures, measurements, and side-by-side findings: [M1-T15 comparison](../evidence/M1-T15/comparison.md).
- Explicit user direction in this GO turn authorized fictional fixture records for Portfolio, Testimonials, and Events. The source panels initialize these collections empty; their card and editor compositions come from their mapped render functions.

## Bounded implementation

`/admin/content` renders Portfolio and Testimonials tabs, and `/admin/events` renders Events. Named `/empty` and `/error` routes render the source empty treatment with typed T02 states. Six clearly fictional records cover Draft and Published for the three collections. The shared frame owns navigation, title, sidebar, and mobile drawer; the ticket component owns only content composition. Add/Edit/Publish/Unpublish/Delete open source-derived forms or confirmation surfaces and expose a named non-authoritative proposed result. No fixture changes, public publication, export, media upload, remote image, storage, or consent decision occurs.

## Review Contract Closure

| Row | Ticket authority and pass condition | Required proof; evidence / exact command and outcome | Status |
| --- | --- | --- | --- |
| RC-25 | Frozen ticket: Draft/Published text and visual distinction; T04 public route cannot render a draft; no hidden DOM draft/publication flag. | `StatusBadge` and populated captures; `app-public-negative.png` and cross-route assertions in `node scripts/t15-visual-evidence.mjs` PASS, exit 0. Public route never imports admin content model. | PROVEN |
| CONTENT/H-15 | Frozen outcome: Portfolio, Testimonials, Events, empty collections, draft/published and proposed publish-result states through named adapter; no real publication or media. | `shared/scenarios/contracts.ts` and `catalogue.ts`; `/admin/content`, `/admin/events`, named empty/error routes; populated/empty/error/form/result captures. `npm run check` PASS; visual harness PASS. | PROVEN |
| VIS-REF-M1-T15 | A2: pre-implementation source ledger, same-viewport source/app captures at desktop and each listed boundary, all named states, keyboard/negative evidence, no material content mismatch. | Ledger and `evidence/M1-T15/comparison.md`; 33 source/app pairs at 1440, 1251/1250, 1151/1150, 931/930, 721/720, 486/485; 16 shared-shell captures; `measurements.json`; state/focus/drawer/negative captures. `node scripts/t15-visual-evidence.mjs` PASS. | PROVEN |
| SRC-REF-M1-T15 | A3/A5: all route-specific structure, CSS, labels, icons, breakpoints and interactions trace to the source, frame, fixture, or user direction; no prototype execution or invented visual substitute. | `docs/frontend/M1-T15_REFERENCE_LEDGER.md` source-to-component map; `shared/admin-content-workspace.tsx` and `.module.css`; comparison asset inventory and negative source scan. `npm run check` PASS. | PROVEN |
| A4-SHARED | A4: render in `AdminWorkspaceFrame`, no route shell duplication/override; Bookings/Availability/content shell regressions at desktop/930/720/485. | Frame props in content component and central link change in `shared/admin-workspace-frame.tsx`; `shell-*` captures, drawer/Escape, no-overflow assertions from visual harness PASS. | PROVEN |
| VIS-003/06/08/10/11 | Frozen UI: Entity Library/Settings Form, readable source typography, non-color status, responsive fit, keyboard/focus, reduced motion. | Paired captures, populated long and empty states, `app-event-form.png`, `app-keyboard-focus.png`, `app-mobile-drawer.png`; browser assertions and reduced-motion contexts PASS. | PROVEN |
| SEC-M1-01/02 | Frozen ticket: preserve private/public and consent seams; safe rendering; no media origin/bytes, consent claim, publish mutation, draft public output, storage or official authority. | Static source scan in comparison: no matches for storage, unsafe HTML, fetch, external origin or script; native React text rendering, fixed local SVG; public negative and unchanged Draft after proposed Publish in visual harness PASS. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

`/admin/content` or `/admin/events` server page → `getScenario("admin.content-draft-success")` and named `admin.content-proposed-result` → typed `ScenarioResult` (`m1-fixture-v1`) → `AdminContentWorkspace` → `AdminWorkspaceFrame` → portfolio/testimonial tabs or event cards → reviewer capture. `/admin/content/{empty,error}` and `/admin/events/{empty,error}` use `getScenario` with named state IDs and the same component. The Playwright harness executes the actual routes, tab, dialog, proposed Publish, empty/error, keyboard, drawer, and public route. Source prototype `db`, localStorage, inline handlers, export Blob, media processing, and publication writes are unreachable from production routes. The source's photo and remote URL controls are omitted under the ticket's explicit media boundary; its export control is visible but disabled.

## Actual Empty-State Evidence

`admin.content-empty` contains zero portfolio, testimonial and event records. The `/admin/content/empty` route displays source-based Portfolio and Testimonials empty panels; `/admin/events/empty` displays the Events empty panel. The 33 paired captures include their actual first-use response at every width; the error routes expose an alert and zero records (`app-error.png`). Repeated reads return the same typed fixture with no seed, initialization, mutation, or hidden default. `app-portfolio-1440.png`, `app-testimonials-1440.png` and `app-events-1440.png` show the first reviewer handoff.

## Migration Inventory

No SQL migration, production-data change, seed, or durable test data is authorized or present. The six sample records live only in the T02 static typed scenario catalogue. The browser harness creates isolated contexts and closes them; no database is involved.

## Limits and handoff

H-15 carries scenario/version, Draft/Published display state, non-authoritative proposed result, and M4 successor mapping. M4 owns consent, media, real publish decisions, private/public delivery and persistence. The source prototype's horizontal mobile navigation is superseded by the A4 shared drawer. No M4 behavior was implemented.
