# M1-T08 — Registration and sign-in fixtures

- Ticket: `M1-T08-registration-signin-fixtures`
- Code repository: `C:\Workspace\bride-assistant`
- Review target commit: `4035d713d0b12710b96e120475edaaa5726283b2`
- Ticket state: `awaiting_review`
- Dependency basis: T02/H-02 and T03/H-03 accepted `PASS` checkpoints; this work consumes only the typed scenario adapter and visual field/status contracts.

## Delivered scope

Converted the mapped `masuk.html` and `daftar.html` form compositions into the reusable `shared/customer-auth.tsx` Next component with local CSS and extracted local assets. The routes `/masuk`, `/daftar`, and their named `validation`, `loading`, `error`, and `success` fixture routes render through the typed T02 scenario adapter. The home/header and booking entry controls route into the two fixture pages; their return controls route to `/`.

No form submission writes state, no account/session is created, and no password, localStorage, sessionStorage, role redirect, external media, executable reference script, or unsafe HTML path is present.

## Reference mapping and comparison ledger

| Reference region | Converted implementation | Source-derived treatment | Evidence |
| --- | --- | --- | --- |
| Editorial shell, logo, eyebrow, heading, copy, footer | `CustomerAuth` editorial section | Original element order, serif/sans hierarchy, gradient, arcs, copy and responsive rules retained | `evidence/M1-T08/reference-{masuk,daftar}-{desktop,1001,1000,701,700,381,380}.png`; corresponding `implementation-*` captures |
| Flower line art | `.flower` in `customer-auth.module.css` | Extracted inline source SVG is used as a local CSS mask, retaining source geometry and source colour role | Same viewport captures above |
| Form side/top navigation/field geometry | `AuthFixture`, `Field`, module CSS | Source layout/CSS media queries converted into JSX/module styles; local links replace prototype navigation | Same viewport captures above |
| Password visibility and forgot-password affordance | React `useState` handlers | Original visible controls retained; dialog explicitly says recovery is unavailable and creates no authority | named-state captures and keyboard capture |
| Fixture feedback | `CustomerAuthReadModel.feedback` plus scenario `state` | T02 typed fixture state replaces prototype writes/storage; status and alert semantics retained | `evidence/M1-T08/state-*.png` |

At the common 1280×960 desktop viewport, computed `.form-wrap` top positions were 198.80px (reference) and 188.31px (implementation), a non-material 10.49px difference. The desktop and boundary captures were visually inspected for form structure, source copy, type hierarchy, palette, art, navigation, controls, focus, and horizontal overflow. No material visible mismatch remains.

## Actual Use Path

`shared/public-home/chrome.tsx` header link and `shared/public-home/sections.tsx` booking link → `/masuk` or `/daftar` entry pages → `CustomerAuth` → `getScenario` typed T02 adapter → `AuthFixture` and semantic form/status UI → named non-authoritative result visible to reviewer. The state routes map only `validation`, `loading`, `error`, and `success` to declared scenario IDs. Playwright exercised each route and route state over the running Next entrypoint.

The original static HTML/prototype scripts are not imported or served by runtime routes; reference assets were extracted into `/public/reference-assets/ui-reference`, and the only source-derived art dependency is a local file. The previous prototype storage/write path is unreachable from the application.

## Review Contract Closure

| Row | Ticket authority and required proof | Evidence locator / executed validation | Status |
| --- | --- | --- | --- |
| RC-18 | Both routes show all named T02 states; no account/session artifact. | `tests/m1-t08.visual.spec.ts`, `npx playwright test tests/m1-t08.visual.spec.ts --grep 'renders the named'` — 8 passed; `evidence/M1-T08/state-*.png`; `npm test` boundary scan — 8 passed. | PROVEN |
| SEC-M1-01 | UI never proves authentication or creates authority. | Playwright keyboard/boundary test checks empty local/session storage and local-only hrefs; `npx playwright test tests/m1-t08.visual.spec.ts --grep 'keyboard focus'` — 1 passed; source boundary scan from `npm test` — 8 passed. | PROVEN |
| SEC-M1-02 | Framework escaping/no unsafe sink. | `tests/workspace-boundaries.test.mjs` safe-render boundary scan via `npm test` — 8 passed; no reference script or unsafe HTML path is imported. | PROVEN |
| VIS-02/06/08/09/10/11 | Labels, focus, error/status presentation, long-field treatment, and mobile reflow. | Keyboard focus capture `evidence/M1-T08/keyboard-masuk-skip-link.png`; state captures; `implementation-*-700/381/380.png`; Playwright overflow assertions passed for all listed widths. | PROVEN |
| VIS-REF-M1-T08 | Ledger and same-viewport reference/implementation captures at desktop and each breakpoint; no material mismatch. | `npx playwright test tests/m1-t08.visual.spec.ts --grep 'matches the extracted reference'` — 2 passed; 28 paired images in `evidence/M1-T08/`; computed desktop geometry recorded above. | PROVEN |
| SRC-REF-M1-T08 | Source region/CSS/asset/interaction map, converted components, T02 fixtures, and visual/state/keyboard evidence. | Mapping table above; `shared/customer-auth.tsx`, `shared/customer-auth.module.css`, route pages, local extracted assets; complete Playwright evidence listed above. | PROVEN |

Regression commands, all successful:

```text
npm run lint
npm run typecheck
npm test
npx playwright test tests/m1-t08.visual.spec.ts --grep 'matches the extracted reference'
npx playwright test tests/m1-t08.visual.spec.ts --grep 'renders the named'
npx playwright test tests/m1-t08.visual.spec.ts --grep 'keyboard focus'
```

`npm run build` is not included in this closure run because the user-owned dev server holds `.next` on Windows; an earlier full `npm run check` had passed before the active server. This does not substitute for the ticket-required actual route captures, which were executed against the active Next entrypoint.

## Migration and empty-state inventory

Not applicable: this ticket authorizes no database, schema, production data, seed, identity, session, or empty-data behavior.

Internal readiness: READY_FOR_CK
