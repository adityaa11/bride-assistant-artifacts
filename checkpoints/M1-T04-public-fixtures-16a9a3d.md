# M1-T04 implementation checkpoint — public discovery fixtures

- Ticket: `M1-T04`; batch/milestone: `3 / M1 / M1-W04`.
- State: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target commit: `16a9a3d392e91ea6fee942bc9fd48fbfbd7d309d` (`fix: finalize M1-T04 responsive and LCP behavior`).
- Ticket authority: [M1-T04](../tickets/m1-fixture-ui-assembly/M1-T04-public-fixtures.md), M1-T04-A1/A2/A3, `M1-W04`, and the public publication invariant.

## Bounded implementation

- Replaced the public placeholder surface with a local `PublicHome` composition: shared header/footer plus independently structured hero, published-content, packages, journey, booking, and FAQ sections.
- The public route and named empty/error routes select declared scenarios through `getScenario`; the surface renders published-only fixture data with no session, persistence, official booking write, customer identity, or external media origin.
- Reproduced the rendered reference with audited local header/footer marks, hero bride SVG, hero stamp, local palette/type roles, background geometry, navigation, and responsive composition. The LCP hero bride image alone is `loading="eager"`; below-fold artwork remains lazy by default.
- Preserved local CSS containment and the adapter/publication boundaries. At the contract boundary widths, `830.98px` and `600.98px` compensate for fractional CSS viewport reporting so the required inclusive 830px/600px transformations apply.

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / validation | Status |
| --- | --- | --- | --- | --- |
| RC-04 | M1-W04 | Public discovery, packages, promotions, and published-content scenarios use T02/T03 fixture seams; no copied static page behavior. | `app/(public)/page.tsx`, `app/(public)/fixtures/{empty,error}/page.tsx`, `shared/public-home.tsx`, `shared/scenarios/adapter.ts`; route/seam tests in `tests/workspace-boundaries.test.mjs`; `npm run check` exit 0. | PROVEN |
| RC-05 | Project publication invariant | Public states expose no drafts or unauthorized/customer/admin data, including empty/error. | Public scenario catalogue provides only `publishedContent`; `PublicHome` has no publication/draft import; empty and error browser AX captures show only the declared package absence/error and the non-authoritative label; source-boundary test and `npm run check` exit 0. | PROVEN |
| VIS-04 | visual contract/VIS-002..011 | Hierarchy, CTA, responsive composition, focusable controls, and empty/error fixtures are visibly coherent and accessible. | Browser captures on 2026-11-21: default 890px composition, 830px single-column hero/menu trigger/560px art region, 600px single-column hero/430px art region/58px headline; AX trees expose skip link, header links, hero CTAs, mobile-menu button, FAQ controls, and fixture messages. | PROVEN |
| VIS-REF-04 | M1-T04-A2 rendered-reference contract | Same-viewport comparison at desktop and required narrow widths, with no material mismatch. | Browser reference/implementation captures at 890px, 830px, and 600px on 2026-11-21. Header, blush hero, copy/type hierarchy, local bride/stamp layering, facts, and divider match. At 830px/600px the supplied reference's fractional-boundary media query stayed horizontal; implementation intentionally uses the explicit A2 contract's required one-column transformation, verified by computed grids `769.2px` and `552.8px`, 560px/430px art regions, and visible captures. | PROVEN |
| SEC-04 | SEC-M1-01/02 | Fixture display remains safe, public-only, and local-origin. | `rg` scan across `app` and `shared` found no `dangerouslySetInnerHTML`, `localStorage`, `eval`, `new Function`, or HTTP(S) origin; asset inventory and role-specific local assets under `public/reference-assets/`; `npm run check` exit 0. | PROVEN |

Internal readiness: READY_FOR_CK

## Actual Use Path

Committed public callers `app/(public)/page.tsx`, `app/(public)/fixtures/empty/page.tsx`, and `app/(public)/fixtures/error/page.tsx` pass declared scenario IDs to `PublicHome` → `getScenario` in `shared/scenarios/adapter.ts` → typed private `scenarioCatalogue` → public `ScenarioResult` → `shared/public-home/{chrome,sections}.tsx` and route-local CSS → framework-rendered public fixture screen. The success caller exposes declared packages; empty/error callers expose their declared absence/error without injecting draft or private data. Browser inspection reached all three routes on the implementation server.

The old generic `PublicDiscovery` placeholder is not called by any public route. No compatibility dispatcher, reference executable behavior, storage path, API, CMS, or public write path is retained.

## Migration Inventory

Not applicable: M1-T04 authorizes no persistence, schema, production data, seed, migration, or migration-runner change.

## Actual Empty-State Evidence

`/fixtures/empty` renders `public.catalogue-empty` through the production public route seam. The browser AX capture reports “No published packages are available in this fixture.” and “Tidak ada paket yang sedang dipublikasikan.”; no package card, draft, initial mutation, database, or seed appears. `/fixtures/error` likewise reports only “The package fixture could not be prepared.” and no package data. Static fixture retrieval remains idempotent.

## Validation

- `npm run check` — exit 0: ESLint, TypeScript, six Node boundary/scenario tests, and production `next build` passed.
- Browser visual review — reference and implementation captured at 890px, 830px, and 600px; implementation meets the explicit responsive contract at the inclusive narrow boundaries.
