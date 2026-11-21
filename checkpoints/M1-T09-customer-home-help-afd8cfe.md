# M1-T09 — Customer home and help source-reference closure

- Ticket: `M1-T09-customer-home-help-fixtures`
- Code repository: `C:\Workspace\bride-assistant`
- Review target commit: `37262bdc172275e17b4aeef8ff197bd8877ff12a`
- Ticket state: `awaiting_review`
- Dependency basis: typed T02 scenario adapter and T03 local customer-shell seam.

## Source ledger and converted composition

| Mapped source | Ordered visible regions and converted destination | CSS / assets | Fixture and interaction translation |
| --- | --- | --- | --- |
| `project's goal/UI Reference/home.html` | customer header/navigation/tools and local notification/profile popovers; page heading; greeting hero with botanical/stamp art; summary cards; Booking Saya empty panel; journey panel; package cards; guidance band; footer → `CustomerFrame`, `CustomerHome`, `Stat`, `Package`, and `shared/customer-home.tsx` | `shared/customer-home.module.css` carries source palette, typography, geometry, source popover treatment, and 1050/820/640 transformations; local audited logo asset `public/reference-assets/ui-reference/home-image-01-5cfb309f414c.webp`; botanical is source-derived inline SVG | `customer.home-success` and `customer.home-empty` enter through `getScenario`. React state translates notification/profile/mobile-menu visibility; booking and package controls remain non-authoritative visible controls; no booking form/list/write was added. |
| `project's goal/UI Reference/bantuan.html` | shared customer chrome; centered help intro; dominant contact card with WhatsApp icon, decorative arcs, unavailable status, signature; footer → `CustomerFrame`, `CustomerHelp` | Same source-derived module and customer shell. No separate raster asset is required for the card artwork/icons. | `customer.whatsapp-unavailable` reaches `CustomerHelp` through `getScenario`; its `state.message` is revealed only after the local action. No URL, external origin, storage, session, or communication handoff is enabled. |

Visible prototype behaviors deliberately omitted under ticket authority: reference storage/session reads, `window.open`, live WhatsApp URL, booking writes, and reference executable scripts. The translated notification/profile and mobile-menu states preserve their mapped visible open/close behavior locally; their original storage, account, and external-site authority is not reproduced.

## Actual Use Path

- `/beranda` → `app/(customer)/beranda/page.tsx` → `CustomerHome("customer.home-success")` → `getScenario` → typed `CustomerReadModel`/state → customer home composition.
- `/beranda/empty` → `app/(customer)/beranda/[state]/page.tsx` → `CustomerHome("customer.home-empty")` → the same adapter path → explicit empty booking treatment.
- `/bantuan` → `app/(customer)/bantuan/page.tsx` → `CustomerHelp("customer.whatsapp-unavailable")` → `getScenario` → local action exposes `scenario.state.message` in the live status region.
- `CustomerFrame` is the sole customer shell for both pages. Its mobile menu is local React state; Escape closes it. Reference HTML and prototype JavaScript are not runtime entrypoints.

## Visual and interaction evidence

The committed `tests/m1-t09.visual.spec.ts` captured source and implementation routes at desktop `1258×966` and at `1051/1050`, `821/820`, and `641/640` viewport edges. Captures are in the code-worktree evidence directory `test-results/m1-t09-playwright-final/`:

- `source-home-{desktop,1051,1050,821,820,641,640}.png` and matching `implementation-home-*` captures.
- `source-help-{desktop,1051,1050,821,820,641,640}.png` and matching `implementation-help-*` captures.
- `implementation-home-empty.png`, `implementation-help-unavailable.png`, and `implementation-help-keyboard-mobile-menu.png`.

Comparison findings closed before this checkpoint: source card anatomy/action rows, palette and white CTA text, footer row structure, help-card width (`770px` at desktop), source type/spacing rules, featured-package surface, and source mobile menu Escape behavior. The capture test asserts each route's main landmark and fixture ID, tests every required viewport edge, and rejects horizontal overflow.

## Review Contract Closure

| Row | Ticket authority and required proof | Evidence / validation locator | Status |
| --- | --- | --- | --- |
| RC-19 | Home/help states, including home empty and WhatsApp unavailable, must be scenario-backed; no URL without D-04. | `shared/customer-home.tsx` calls `getScenario` for home and help; `shared/scenarios/catalogue.ts` owns unavailable copy; `tests/workspace-boundaries.test.mjs`; Playwright named-state test. | PROVEN |
| SEC-M1-01 | Preserve the deferred communication/ownership seam; no live communication, session, private lookup, persistence, booking write, or official transition. | `tests/m1-t09.visual.spec.ts` asserts local/hash-only links and local unavailable status; `tests/workspace-boundaries.test.mjs` rejects live WhatsApp URLs; source scan and `npm run check`. | PROVEN |
| SEC-M1-02 | No raw reference script, storage path, unsafe HTML sink, or external origin reaches runtime. | JSX/React handlers in `shared/customer-home.tsx`; local asset path only; `tests/workspace-boundaries.test.mjs` source scan; `npm run check`. | PROVEN |
| VIS-002/06/08/10/11 | Semantic local navigation, native controls, focus/status treatment, empty state, responsive reflow, and no clipping. | Playwright keyboard/mobile test proves skip-link focus, 820px menu open, Escape close, and overflow check. Named empty/unavailable test proves empty panel and live status. | PROVEN |
| VIS-REF-M1-T09 | Same-viewport source/implementation captures for both mapped routes at desktop and every named breakpoint edge; no material visible mismatch. | `tests/m1-t09.visual.spec.ts`, final capture inventory above, and completed comparison corrections. | PROVEN |
| SRC-REF-M1-T09 | Source-region/CSS/asset/interaction map; React/fixture conversion; no runtime prototype behavior. | Source ledger above; `shared/customer-home.tsx`, `shared/customer-home.module.css`, audited local asset, scenario catalogue, and final Playwright evidence. | PROVEN |

## Validation

```text
npm run check
# PASS: eslint, typecheck, node tests (9/9), and Next production build

M1_T09_CAPTURE_DIR=test-results/m1-t09-playwright-final \
M1_T09_REFERENCE_URL=http://127.0.0.1:3001 \
npx playwright test tests/m1-t09.visual.spec.ts
# PASS: 4/4 — source/implementation breakpoint captures, empty/unavailable state,
# keyboard focus, mobile menu/Escape behavior, local-link boundary, no overflow

git diff --check
# PASS
```

## Migration Inventory

Not applicable. This ticket creates no migrations, production data, storage, seed data, or test database setup.

## Actual Empty-State Evidence

`/beranda/empty` selects `customer.home-empty` through the typed adapter and renders the mapped empty booking panel with the fixture absence message. The route performs no initialization, durable mutation, default seed, or data lookup. Evidence: named-state Playwright test and `implementation-home-empty.png` capture.

Internal readiness: READY_FOR_CK
