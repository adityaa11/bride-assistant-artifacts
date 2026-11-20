# M1-T01 implementation checkpoint — workspace boundaries

- Ticket: `M1-T01`; batch/milestone: `1 / M1 / M1-W01`.
- State: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target commit: `2cb26dc1236955f18768385a5cd2a5ac7fac60fd` (`feat: establish fixture review workspace`).
- Ticket authority: [M1-T01](../tickets/m1-fixture-ui-assembly/M1-T01-workspace-boundaries.md), `M1-W01`, Architecture delivery/UI transition, and Project Context current/target state.

## Bounded implementation

- Added the minimal pinned Next.js/React/TypeScript review workspace with root `check` commands and a lockfile.
- Added public (`/`), customer (`/customer`), and admin (`/admin`) route groups. Each imports the shared presentation-only `shared/review-route.tsx` seam.
- Added boundary tests that reject prototype `localStorage`, unsafe HTML rendering, dynamic string compilation, and external origins in review-build source.
- No auth, database, storage, API route, deployment/provider configuration, CSP-header change, prototype import, or compatibility path was added.

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / validation | Status |
| --- | --- | --- | --- | --- |
| RC-01 | M1-W01 | Root checks run from an installed clean dependency state and report result. | `npm ci && npm run check` from repository root: exit 0; lint, TypeScript, 2 boundary tests, and production build passed. | PROVEN |
| RC-02 | Architecture delivery/UI transition | Public/customer/admin route groups share a seam without prototype HTML or browser-local code; import scan and route render prove it. | `tests/workspace-boundaries.test.mjs`: 2/2 pass; `next build` statically rendered `/`, `/customer`, `/admin`; `next start` smoke requests returned `200` and fixture text at all three routes. | PROVEN |
| SEC-01 | SEC-M1-02 | Client code uses framework rendering with no HTML sink, inline executable code, string compilation, or unreviewed external origin. | Targeted source boundary test passed in `npm run check`; review of `app/` and `shared/` at commit `2cb26dc`. | PROVEN |

Internal readiness: READY_FOR_CK

## Actual Use Path

`package.json` root `check` script → ESLint / `tsc --noEmit` / Node boundary tests / `next build` → process exit status → developer or CI feedback. Executed `npm ci && npm run check` exits 0.

Route path: `app/(public)/page.tsx`, `app/(customer)/customer/page.tsx`, or `app/(admin)/admin/page.tsx` → `shared/review-route.tsx` → framework-rendered non-authoritative fixture placeholder → reviewer. `next build` generated all three static routes; `next start` requests to `/`, `/customer`, and `/admin` each returned HTTP 200 and the expected fixture review text.

There is no superseded runtime production caller: standalone reference HTML is not imported, and its direct-script/browser-local paths are unreachable from this workspace.

## Migration Inventory

Not applicable: this ticket authorizes no persistence, schema, production-data, seed, or test-database change.

## Actual Empty-State Evidence

Not applicable: this ticket authorizes no data-dependent entrypoint or initial-state behavior.

## Environment note

The local validation host ran Node `v24.19.0`; the workspace declares the approved minimum `>=24.22.3`, so npm emitted an engine warning but completed `npm ci` and every required check successfully. This is informational only; it does not change the ticket contract.
