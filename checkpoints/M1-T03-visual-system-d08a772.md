# M1-T03 implementation checkpoint — Soft Rose visual system

- Ticket: `M1-T03`; batch/milestone: `2 / M1 / M1-W03`.
- State: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target commit: `d08a772a3ab52ecde64863e9d916cb8ddd799718` (`feat: add soft rose visual system`).
- Ticket authority: [M1-T03](../tickets/m1-fixture-ui-assembly/M1-T03-visual-system.md), `M1-W03`, the Project Context visual/navigation boundary, and the M1 ticket-set README.

## Bounded implementation

- Added `docs/frontend/PROJECT_FRONTEND_CONTRACT.md`, deriving the frozen Soft Rose Atelier personality, semantic token roles, responsive transformations, typography, accessibility, motion, and forbidden patterns from project authority and supplied references as grammar only.
- Added semantic visual tokens and responsive shell styles in `app/globals.css`; Root Layout now loads the shared system. No external font, script, image, runtime style mutation, or CSP/header change was added.
- Replaced the placeholder review render with public/customer top-navigation and admin rail shells. Each shell composes shared presentation-only status, empty-state, and form primitives from the typed scenario result.
- Added `shared/ui-primitives.tsx`: native labelled read-only fixture form, disabled fixture control, keyboard-visible native controls, live status/alert treatment, and an explicit empty-state primitive. These controls perform no account, data, WhatsApp, or service operation.
- No screen-specific workflow/business logic, theme toggle, auth semantics, persistence, migration, seed, compatibility path, or legacy runtime caller was added.

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / validation | Status |
| --- | --- | --- | --- | --- |
| RC-01 / VIS-01 | State 1 / VIS-001–006; Required visual contract | Visual contract exists; semantic type, spacing, radius, surface, and color roles drive shared primitives; no per-screen CSS substitute. | `docs/frontend/PROJECT_FRONTEND_CONTRACT.md`; `app/globals.css`; shared shell in `shared/review-route.tsx`; rendered desktop inspection of `/`. `npm run check` exit 0. | PROVEN |
| RC-02 / VIS-02 | M1-W03 + WCAG 2.2 AA | Semantic labels, keyboard operation, focus-visible, non-color states, and applicable interaction states. A static screenshot alone is forbidden. | `shared/ui-primitives.tsx` uses native form/label/input/button plus `role=status` / `role=alert` and `aria-live`; `app/globals.css` supplies `:focus-visible`, disabled, hover, and reduced-motion states. Browser keyboard review gave first focus to “Skip to review content”; route accessibility tree exposes semantic navigation, headings, labelled form, status, and buttons. `npm run check` exit 0. | PROVEN |
| RC-03 / VIS-03 | M1-W03 + responsive fit | Desktop and narrow shell transform navigation/content; long values reflow without silent clipping; viewport-only stretching is forbidden. | Desktop rendered inspection of `/` shows two meaningful panels and top navigation; 360px inspection gives a one-column review grid, wrapped navigation, and `scrollWidth: 360`, `clientWidth: 360`, `gridColumns: 328px`. CSS `@media (max-width:48rem)` transforms header/admin/navigation/grid. `npm run check` exit 0. | PROVEN |
| RC-04 / SEC-03 | SEC-M1-02 / CSP-safe UI | Avoid unsafe HTML sinks, inline execution, runtime style writes, and silent external assets. Client CSP workaround is forbidden. | Targeted `rg` scan across `app`, `shared`, and `docs/frontend` found no `dangerouslySetInnerHTML`, HTML sinks, `eval`, `Function`, inline script/style, `style=`, or `http(s)` origin. Direct source review of `app/globals.css`, `shared/review-route.tsx`, and `shared/ui-primitives.tsx` confirms JSX/classes only. `npm run check` exit 0. | PROVEN |

Internal readiness: READY_FOR_CK

## Actual Use Path

Committed callers `app/(public)/page.tsx`, `app/(customer)/customer/page.tsx`, and `app/(admin)/admin/page.tsx` pass typed `ScenarioId` props to `shared/review-route.tsx` → `getScenario` in `shared/scenarios/adapter.ts` → private typed catalogue → `ScenarioResult` → shared `StatusNotice`, `EmptyState`, and `FixtureForm` in `shared/ui-primitives.tsx` → semantic, tokenized React/Next HTML. Public/customer use a top-navigation shell; admin uses an operational rail; all consume presentation-only props and fixed non-authoritative copy.

Executed integration evidence: production build plus `next start -p 3312` returned HTTP 200 at `/`; browser inspection confirmed the public rendered shell, status, content item, provenance panel, narrow reflow, and keyboard skip-link focus. The three route callers remain static and idempotent with no direct feature fixture import. The prior placeholder-only `ReviewRoute` output is no longer reachable as the production render path; no fallback, duplicate dispatcher, or legacy caller remains.

## Migration Inventory

Not applicable: M1-T03 authorizes no persistence, schema, production-data, seed, test database, or migration-runner change.

## Actual Empty-State Evidence

The `EmptyState` primitive renders the typed scenario absence with an explicit explanation; it does not manufacture data, default state, or durable seed. For a declared empty scenario, `ReviewRoute` consumes the same route → adapter → `ScenarioResult` path and renders the absence message, labelled fixture form, and non-authoritative provenance. No database, tenant, query, initialization, or first-handoff mutation exists; static scenario retrieval remains idempotent.

## Validation

- `npm run check` — exit 0: ESLint, TypeScript, four boundary/catalogue tests, and production `next build` passed.
- `next start -p 3312` plus HTTP smoke — `/` returned HTTP 200 and rendered the shared public review shell.
- Browser visual/accessibility review — desktop hierarchy and narrow 360px reflow inspected; no horizontal overflow; skip link received first keyboard focus; reduced-motion styles are guarded by `prefers-reduced-motion`.
- CSP-safe targeted scan — no unsafe sinks, inline execution/styles, runtime style writes, or external origins in changed/directly-related UI source.
- Validation host Node: `v24.19.0`; package engine is `>=24.22.3`. Required checks completed successfully with the existing dependency tree; this is an environment note, not a substitute for validation.
