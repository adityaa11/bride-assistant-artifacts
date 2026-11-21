# M1-T09 — Customer home and help fixtures

- Ticket: `M1-T09-customer-home-help-fixtures`
- Code repository: `C:\Workspace\bride-assistant`
- Review target commit: `61ae616213e53abf3623a97a83b488a7c71e2006`
- Ticket state: `awaiting_review`
- Dependency basis: T02/H-02 and T03/H-03 are approved `PASS`; this work consumes only the typed scenario adapter and presentation primitives.

## Delivered scope

Refactored the mapped `home.html` and `bantuan.html` reference compositions into `shared/customer-home.tsx` and route-local source-derived CSS. `/beranda`, `/beranda/empty`, and `/bantuan` are fixture-only customer routes. The home route has declared success and explicit empty scenarios; help consumes the declared unavailable-WhatsApp scenario.

The implementation has no session, private lookup, booking form/list authority, persistence, local/session storage, external WhatsApp URL, reference executable script, or unsafe HTML sink. The help action translates the visible source control to an accessible local unavailable result: `Kontak WhatsApp belum tersedia. Silakan coba kembali nanti.`

## Reference ledger and source map

| Source page/region | Converted component and CSS | Fixture / interaction translation | Local assets |
| --- | --- | --- | --- |
| `home.html` customer chrome, logo, desktop/mobile navigation, tools, footer | `CustomerFrame`, `.header`, `.nav`, `.desktopNav`, `.mobileNav`, `.footer` | React mobile-menu state; local route links; no account, notification, or prototype action | `home-image-01-5cfb309f414c.webp` logo |
| `home.html` heading, greeting hero, stamp, botanical art, actions | `CustomerHome`, `.heading`, `.hero`, `.heroArt`, `.actions` | `customer.home-success` provides the visible summary; local fixture links replace prototype booking behavior | `home-inline-svg-06-e0db35c3285f.svg` botanical; corrected XML `viewBox` casing so the extracted source art renders |
| `home.html` summary cards, booking empty panel, journey, package cards, help band | `Stat`, `Package`, home sections and module styles | `customer.home-empty` provides intentional no-booking state; no hidden default or durable seed | no additional image asset |
| `bantuan.html` shared customer chrome, intro, help card, action/status | `CustomerHelp`, `CustomerFrame`, `.helpIntro`, `.helpCard` | React status state replaces prototype `window.open`; unavailable fixture never enables a URL | no additional image asset |

Source CSS rules and breakpoint transformations were refactored into `customer-home.module.css`: desktop shell; 1050px compact hero/navigation; 820px mobile menu and narrow shell; 640px stacked hero, reflowed cards/packages, full-width help action. Reference source structure and copy remain represented as JSX regions, with prototype storage/session/script behavior intentionally omitted under ticket authority.

## Actual Use Path

`/beranda` or `/beranda/empty` route → `CustomerHome` → `getScenario` T02 adapter → typed `CustomerReadModel` → home summary/empty-state composition → reviewer-visible fixture. `/bantuan` → `CustomerHelp` → `getScenario("customer.whatsapp-unavailable")` → local React status handler → reviewer-visible unavailable message. `CustomerFrame` is the shared local customer navigation shell for both paths.

The superseded raw-reference path is unreachable: no route serves reference HTML or executable script; no runtime code imports raw fixture objects, prototype storage, or WhatsApp origin. The only asset use is local audited reference output.

## Review Contract Closure

| Row | Ticket authority and required proof | Evidence locator / validation outcome | Status |
| --- | --- | --- | --- |
| RC-19 | Home/help, including empty and WhatsApp-unavailable, are scenario-backed; no URL is enabled without D-04. | `shared/scenarios/catalogue.ts`; `tests/workspace-boundaries.test.mjs` T09 test; Playwright on production `/bantuan` clicked `Hubungi via WhatsApp` and observed only the unavailable status; focused source scan found no `wa.me`, `whatsapp://`, storage, script, or unsafe HTML path. | PROVEN |
| SEC-M1-01 | Preserve future ownership and communication seam; never claim authority. | Typed T02 scenarios carry fixture provenance; routes use no identity/private-data input; unavailable action is local-only. `npm test` passed 9/9. | PROVEN |
| SEC-M1-02 | Safe framework rendering with no executable reference behavior or external origin. | Targeted source scan and boundary test passed; `CustomerHome`/`CustomerHelp` use JSX, local assets, and React handlers only. | PROVEN |
| VIS-002/06/08/10/11 | Semantic landmark/navigation, focusable controls, state message, long content, and responsive reflow. | Production browser inspection at 640px found `scrollWidth: 640`, `clientWidth: 640`; native buttons/links and `aria-live` status are present. Captures: local `test-results/m1-t09-home-{desktop,640}.png`, `m1-t09-home-empty-640.png`, `m1-t09-help-{desktop,640}.png`. | PROVEN |
| VIS-REF-M1-T09 | Reference ledger and same-viewport home/help comparison; no material visible mismatch. | Direct capture inspection at desktop and 640px confirmed source-derived hierarchy, shell, hero/help card geometry, type roles, palette, local art, mobile menu treatment, full-width narrow help action, and no horizontal overflow. Corrected the extracted botanical SVG `viewBox` defect discovered during capture. | PROVEN |
| SRC-REF-M1-T09 | Source-to-component/CSS/asset/interaction map, T02 fixture routes, visual/state evidence. | Mapping table above; `app/(customer)/beranda/**`, `app/(customer)/bantuan/page.tsx`, `shared/customer-home.tsx`, `shared/customer-home.module.css`, audited local assets, and production browser evidence. | PROVEN |

Executed validation, all passed:

```text
npm run check
git diff --check
npx playwright screenshot --device="Desktop Chrome" http://localhost:3320/beranda test-results/m1-t09-home-desktop.png
npx playwright screenshot --viewport-size="640,900" http://localhost:3320/beranda test-results/m1-t09-home-640.png
npx playwright screenshot --viewport-size="640,900" http://localhost:3320/beranda/empty test-results/m1-t09-home-empty-640.png
npx playwright screenshot --device="Desktop Chrome" http://localhost:3320/bantuan test-results/m1-t09-help-desktop.png
npx playwright screenshot --viewport-size="640,900" http://localhost:3320/bantuan test-results/m1-t09-help-640.png
```

## Migration inventory

Not applicable: the ticket authorizes no persistence, migration, seed, production data, or test-database setup.

## Actual Empty-State Evidence

`/beranda/empty` resolves through the same adapter path to `customer.home-empty`; its visible booking panel explicitly states the intentional absence of booking data and offers only the next review action. No hidden default, seed, mutation, or first-use write occurs.

Internal readiness: READY_FOR_CK
