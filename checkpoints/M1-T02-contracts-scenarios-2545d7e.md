# M1-T02 implementation checkpoint — typed fixture scenarios and adapter

- Ticket: `M1-T02`; batch/milestone: `2 / M1 / M1-W02`.
- State: `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target commit: `77b4a2bbd095b4d8b7772fbbca0af31169d53722` (`feat: add fixture scenario adapter`).
- Ticket authority: [M1-T02](../tickets/m1-fixture-ui-assembly/M1-T02-contracts-scenarios.md), `M1-W02`, Architecture UI transition, Project Context lifecycle/publication/current-target state, and Verification M1.

## Bounded implementation

- Added the versioned `ScenarioResult` contract and typed public, customer, admin, and unknown-scenario read models under `shared/scenarios/`.
- Added one internal scenario catalogue plus `getScenario`, the sole fixture retrieval adapter. Declared IDs type-check at callers; unsupported string IDs return a typed system error result.
- Added canonical lifecycle labels; a public read model exposes only `publishedContent`, while admin fixture content can distinguish `draft` and `published`. Customer availability exposes only a date and availability state.
- Added loading, empty, error, success, WhatsApp-unavailable, and non-authoritative command-result fixtures. Every result has fixed non-authoritative provenance and no command/persistence behavior.
- Changed the three existing review routes to supply compile-time scenario IDs to the shared route seam, which retrieves through the adapter and renders its result to reviewers.
- Added import-boundary and catalogue-inventory regression tests. No browser storage, business validation, persistence, live WhatsApp link, official transition, shared JSON feature import, seed, migration, or customer-detail exposure was added.

## Review Contract Closure

| Row | Ticket authority | Required proof | Evidence / validation | Status |
| --- | --- | --- | --- | --- |
| RC-01 | M1-W02 + Architecture UI transition | Versioned adapter retrieves declared feature scenarios; feature code has no direct shared fixture import. | `shared/scenarios/{contracts,catalogue,adapter}.ts`; `shared/review-route.tsx`; `tests/workspace-boundaries.test.mjs` import-boundary assertion; `npm run check` exit 0. | PROVEN |
| RC-02 | Project Context lifecycle/publication rules | Canonical labels; public reads exclude drafts; no prototype labels. | `CanonicalBookingStatus`, surface-specific read-model types, and scenario inventory in `shared/scenarios/`; lifecycle/publication assertion in `tests/workspace-boundaries.test.mjs`; `npm run check` exit 0. | PROVEN |
| RC-03 | Verification M1 | Loading, empty, error, success, unavailable WhatsApp, and a non-authoritative command-result state are representable and render through the harness. | Scenario inventory assertion in `tests/workspace-boundaries.test.mjs`; `npm run check` exit 0; production `next start -p 3311` smoke returned HTTP 200 and rendered review-state/non-authoritative copy at `/`, `/customer`, and `/admin`. | PROVEN |
| SEC-02 | SEC-M1-01 | No fixture result can imply session, authority, persistence, or official command evidence. | `NonAuthoritativeProvenance` and `NonAuthoritativeInteraction` are required in every `ScenarioResult`; adapter and rendered copy carry that label; boundary/provenance assertion and `npm run check` exit 0. | PROVEN |

Internal readiness: READY_FOR_CK

## Actual Use Path

Committed callers: `app/(public)/page.tsx`, `app/(customer)/customer/page.tsx`, and `app/(admin)/admin/page.tsx` each pass a compile-time `ScenarioId` to `shared/review-route.tsx` → `getScenario` in `shared/scenarios/adapter.ts` → private `scenarioCatalogue` in `shared/scenarios/catalogue.ts` → versioned `ScenarioResult` → framework-rendered reviewer-visible title, state, provenance, and interaction summary. The adapter's string overload returns a typed `system` error read model for unknown IDs.

The executed production build and `next start` smoke reached the committed public, customer, and admin callers. Each returned HTTP 200 and contained both `Review state` and `Non-authoritative fixture` copy. Successor T04–T06 feature composition consumes the same `ScenarioId`/adapter endpoint; it has no direct fixture-object path.

The superseded placeholder-only route output no longer forms the runtime result path. Prototype `localStorage` shapes were never imported into the workspace, and the boundary source test still rejects them. No compatibility wrapper, fallback fixture path, or old direct caller remains.

## Migration Inventory

Not applicable: M1-T02 authorizes no persistence, schema, production-data, seed, test database, or migration-runner change. The scenario catalogue is TypeScript source and contains disposable in-memory fixture values only.

## Actual Empty-State Evidence

The catalogue includes `public.catalogue-empty`, `public.published-content-empty`, `customer.bookings-empty`, and `admin.catalogue-empty`. Each returns `state.kind: "empty"`, an absence explanation, and a next permitted review action through the identical adapter → `ScenarioResult` → shared rendering path. There is no default substitute, durable seed, or initial database/tenant state; static retrieval is idempotent and has no retry behavior.

`tests/workspace-boundaries.test.mjs` inventories the empty-state representation with the other required state kinds, and `npm run check` completed successfully. Future feature routes select their declared empty scenario via the typed ID rather than importing fixture values.

## Validation

- `npm run check` — exit 0: ESLint, TypeScript, four Node boundary/catalogue tests, and production `next build` all passed.
- `npm run start -- -p 3311` plus HTTP smoke requests — `/`, `/customer`, and `/admin` each returned 200 and exposed the rendered review-state/non-authoritative markers.
- Validation host Node: `v24.19.0`; package engine is `>=24.22.3`. No install was performed in this checkpoint; the existing lockfile/dependency tree ran all required checks successfully. This is recorded as an environment note, not a substitute for the required validation.
