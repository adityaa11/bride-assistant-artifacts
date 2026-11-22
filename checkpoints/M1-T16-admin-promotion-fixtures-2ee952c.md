# M1-T16 GO checkpoint — admin promotion fixtures

- Ticket: `M1-T16`; state: `awaiting_review`; batch/milestone: `3 / M1 / M1-W06`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target code commit: `2ee952c9d6230849ff70e2db62efcc76454ecad9` (`feat: render admin promotion fixtures`).
- Predecessors: M1-T02/H-02 and M1-T03/H-03 have CK `PASS` and ticket state `approved`.
- Reference ledger: `docs/frontend/M1-T16_REFERENCE_LEDGER.md` at the review target commit; source is `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html`, `renderPromotions`, `openWebsiteEditor`, shared website helpers, and bound CSS.

## Bounded implementation

`/admin/promotions` and `/admin/promotions/{empty,error,long,result}` use named versioned T02 adapter scenarios and the source-derived Promotions composition inside `AdminWorkspaceFrame`. Typed records cover percentage and rupiah display, status, period, package applicability, and long package text. The modal retains the reference field order, native required inputs, source value/date/package validation messages, and a source-style toast. Add, Edit, Publish, Unpublish, and Hapus display a non-authoritative result; no collection or package price changes. The source's Export JSON control is displayed disabled because T16 has no export authority. The shared frame gained the Promotions navigation destination only.

## Review Contract Closure

| Row | Ticket authority / pass condition | Required proof and evidence locator | Exact validation / outcome | Status |
| --- | --- | --- | --- | --- |
| RC-26 | T16 original: named list/detail/form, percent/rupiah, period, package applicability, validation/error, proposed result; no price mutation or promotion authority. | `shared/scenarios/{contracts,catalogue}.ts`, `shared/admin-promotion-workspace.tsx`, named route pages; `tests/m1-t16.visual.spec.ts` form/action/reload/empty/error/long/result checks. | `npm run check` exit 0; production Playwright 3/3; source scan found no storage/HTML sink/eval pattern. | PROVEN |
| VIS-REF-M1-T16 | A2: ledger, same-viewport source/app captures at desktop and each bound breakpoint, named states, visual comparison, keyboard and negative evidence; no material content mismatch. | Ledger and 64 captures at `C:\Users\ASUS\AppData\Local\Temp\m1-t16-go-production`; source/empty/populated pairs at 1440, 1251, 1250, 1151, 1150, 931, 930, 721, 720, 486, 485px; editor, validation, result, long, error, shared shell captures. Source screenshots use one newly created prototype record solely for comparison; app uses two typed fixture records. | `$env:M1_T08_BASE_URL='http://localhost:3316'; $env:M1_T16_REFERENCE_URL='http://127.0.0.1:3317'; $env:M1_T16_CAPTURE_DIR='C:\Users\ASUS\AppData\Local\Temp\m1-t16-go-production'; npx playwright test tests/m1-t16.visual.spec.ts` — 3/3 PASS against production `next start`; manual side-by-side: no material route-specific mismatch. | PROVEN |
| SRC-REF-M1-T16 | A3/A5: exact source region, CSS, asset and interaction trace; converted typed components and fixture data; no improvised runtime prototype behavior. | `docs/frontend/M1-T16_REFERENCE_LEDGER.md` maps each region/selector/interaction; `AdminPromotionWorkspace`, `PromotionStats`, `PromotionCard`, `PromotionEditor`; source `.web-*` styles reused from `shared/admin-content-workspace.module.css`, source-specific additions in `shared/admin-promotion-workspace.module.css`; `AdminIcon` local extracted SVG. | Production browser suite 3/3; `rg -n 'localStorage|dangerouslySetInnerHTML|innerHTML|document\.write|\beval\('` on promotion route/component/scenario paths found no match. | PROVEN |
| A4-SHARED | A4: unique content in `AdminWorkspaceFrame`, no copied/overridden shell; Bookings, Availability and Promotions at desktop/930/720/485px. | Frame props in `AdminPromotionWorkspace`; only central Promotions link changed in `shared/admin-workspace-frame.tsx`; `shared-*` captures and overflow/drawer/Escape/reduced-motion assertions in Playwright test. | Production Playwright shared-frame test PASS at 1440, 930, 720, 485px. | PROVEN |
| SEC-M1-01/02 | Ticket/README: preserve future server authorization and validation seam; safe rendering, no live account, persistence, publication or price authority. | Versioned `getScenario` route inputs and `ScenarioResult` provenance; React text rendering; proposed actions are local message state only; disabled export; public negative assertion excludes admin draft. | `npm run check` exit 0 (13 Node tests and build); production Playwright form/result/public-negative checks PASS; runtime source scan no match. | PROVEN |

Internal readiness: READY_FOR_CK

## Source-to-component and visual comparison

The committed ledger identifies source `renderPromotions()` toolbar, three stats, cards, empty panel and hint, `openWebsiteEditor()` form, website control helpers, local SVG roles, measured type/color/spacing, and 1150/720/485px content rules. `AdminWorkspaceFrame` owns the shell and 1250/930/485px outer behavior. In same-width screenshots, toolbar, counters, card content/order, type roles, palette, modal geometry, field order, and single-column narrow behavior match the source regions. The source prototype initial collection is empty while the default app route intentionally displays two typed fixtures; comparison therefore uses `/admin/promotions/empty` against the initial reference and creates one source-only record to inspect populated card geometry. The source's mobile horizontal navigation is superseded by A4's shared right-side drawer. The disabled export and non-authoritative result replace prototype export/save behavior by ticket boundary. No unresolved material content mismatch or unsupported source decision remains.

## Actual Use Path

`shared/admin-workspace-frame.tsx` Promotions navigation → `/admin/promotions` or named state route page → `getScenario("admin.promotions-…")` in `shared/scenarios/adapter.ts` → versioned typed `adminPromotions` read model in `shared/scenarios/catalogue.ts` → `AdminPromotionWorkspace` → shared `AdminWorkspaceFrame` → toolbar/stats/cards/empty/editor/result → reviewer. The production Playwright suite visits each route and operates the modal and result. The prototype `websiteList`, `save`, localStorage, date-policy function and price calculations are unreachable from the app. No legacy promotion caller exists.

## Migration Inventory

None. The ticket authorizes no SQL, production data, seed, or storage migration. The scenarios are static typed in-memory fixture values. The reference server's temporary browser storage is cleared after each comparison and is not imported into the application.

## Actual Empty-State Evidence

`/admin/promotions/empty` asks the adapter for `admin.promotions-empty`, whose records array is empty. `PromotionStats` shows zero counts and the source-derived `Belum ada promotion` region, with an add control opening the same non-authoritative form. No initialization, default promotion, durable seed, or mutation occurs. The production browser test captured and asserted this route at every viewport. `/admin/promotions/error` uses an empty records array and explicit error copy in the same source region. The first handoff is the visible reviewer surface, not a persistence path.

## Validation and limitations

- `npm run check` — exit 0: ESLint, TypeScript, 13 Node boundary tests, and Next production build.
- Production `npx playwright test tests/m1-t16.visual.spec.ts` with the environment above — 3/3 PASS; 64 captures. Native required-field behavior, percent maximum, date order, package selection, proposed result/reload, keyboard focus/Escape, public draft negative, responsive overflow, shared drawer and reduced motion were exercised.
- `git diff --cached --check` before code commit — exit 0.
- No live admin identity, promotion publication, export, effective-price calculation or booking snapshot mutation is implemented by M1. The reference's operational copy is presented in the source panel, with fixture provenance and inert actions clarifying this boundary.
