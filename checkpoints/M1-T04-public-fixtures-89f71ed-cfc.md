# M1-T04 CFC remediation checkpoint — public promotion fixture

- Ticket: `M1-T04`; batch/milestone: `3 / M1 / M1-W04`.
- State: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Remediation commit: `89f71ed30c04373788aa42eed9305aefa116071e` (`fix: render public fixture promotions`).
- Frozen CK authority: [M1-T04 CK review](../feedback/M1-T04-public-fixtures-16a9a3d-ck.md), clause `CK-001.a`.
- Remediation scope: the one frozen unresolved clause only. No CMS/API/persistence/public write, external origin, customer/admin data, or scenario authority was added.

## Authorized closure progress

| Frozen clause | Ticket trace and frozen oracle | Repair and evidence location | Required validation and outcome | Status |
| --- | --- | --- | --- | --- |
| CK-001.a | M1-T04 `RC-04`: public discovery, packages, promotions, and published content render through T02/T03 fixture seams. The CK oracle requires the success route `/` to visibly render its adapter-provided promotion title, description, value, and end date in reference-aligned treatment; empty/error must render no promotion and direct checks must remain green. | `shared/public-home.tsx` passes `readModel.promotions` from the typed adapter to `PackagesSection`; `shared/public-home/sections.tsx` renders `PromotionShowcase` only for supplied `PublishedPromotion` rows and exposes title, description, value, and end date; `shared/public-home.module.css` implements the mapped reference showcase treatment and its narrow layout. `tests/workspace-boundaries.test.mjs` asserts the adapter path and empty/error arrays. | `npm test` — exit 0 (7 Node tests); `npm run typecheck` — exit 0; `npm run lint` — exit 0; `npm run build` — exit 0. Browser AX/text inspection of `/` showed `A little more care for your moment`, `A published seasonal offer for a considered bridal companion.`, `10%`, and `31 Desember 2026`; visual inspection showed the reference-aligned product showcase. `/fixtures/empty` and `/fixtures/error` each had zero promotion elements, while retaining their declared state messages. At 830px and 600px the browser reported the required one-column hero grids (769.2px and 552.8px) and retained the 560px/430px art-region rules. | PROVEN — frozen oracle passed. |

## Actual repaired use path

The committed default public caller `app/(public)/page.tsx` declares `public.discovery-success` → `PublicHome` → `getScenario` in `shared/scenarios/adapter.ts` → typed `scenarioCatalogue` read model in `shared/scenarios/catalogue.ts` → `readModel.promotions` → `PackagesSection` / `PromotionShowcase` → visible public product/discovery composition at `/`. The browser-visible promotion is therefore adapter-backed and remains read-only. The named empty and error callers continue through the same path with their declared empty promotion arrays, producing no promotion output.

## Direct regression and boundary check

- Publication/privacy: only the typed public promotion fields are rendered; no draft, customer, or admin field, client-side filter, or new scenario was introduced. Browser AX/text checks of empty and error routes contain no promotion text or element.
- Visual/responsive: the showcase uses the existing reference-derived products treatment; it stacks at the existing 650px narrow transformation without changing the frozen hero/header breakpoints. Browser review at 830px and 600px retained the frozen one-column/mobile-menu/art-region composition.
- Security/local-origin: no dynamic HTML, storage, HTTP(S) origin, external media, persistence, CMS/API, or public write was introduced. The existing source-boundary test and full check suite remain green.

Internal readiness: READY_FOR_CK

## Validation

- `npm test` — exit 0 (seven tests, including `the public success route renders only adapter-provided published promotions`).
- `npm run typecheck` — exit 0.
- `npm run lint` — exit 0.
- `npm run build` — exit 0 (production build and all seven static routes).
- Browser: `/` AX/text + visual review confirmed title, description, `10%`, and `31 Desember 2026`; `/fixtures/empty` and `/fixtures/error` each reported zero promotion elements; 830px and 600px responsive checks passed.

