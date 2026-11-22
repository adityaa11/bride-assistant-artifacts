# CK review — M1-T16 admin promotion fixtures

- Ticket: `M1-T16`
- Batch/milestone: `3 / M1 / M1-W06`
- Result: `PASS`
- Review type: first consolidated CK review
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed commit: `2ee952c9d6230849ff70e2db62efcc76454ecad9` (`feat: render admin promotion fixtures`)
- Frozen ticket: [M1-T16](../tickets/m1-fixture-ui-assembly/M1-T16-admin-promotion-fixtures.md)
- Submitted checkpoint: [M1-T16 GO checkpoint](../checkpoints/M1-T16-admin-promotion-fixtures-2ee952c.md)

## Review Contract disposition

| Row | Frozen authority / pass condition | Evidence reviewed | Status |
| --- | --- | --- | --- |
| RC-26 | Original T16: named promotion list/detail/form fixture states; percentage/rupiah values; active period; package applicability; validation/error/proposed-result display; no promotion authority or package-price mutation. | `shared/scenarios/contracts.ts`, `shared/scenarios/catalogue.ts`, `shared/scenarios/adapter.ts`, `shared/admin-promotion-workspace.tsx`, both promotion route pages, and `tests/m1-t16.visual.spec.ts`. The route reaches typed `admin.promotions-*` scenarios through `getScenario`; actions only set local result state. | PROVEN |
| VIS-REF-M1-T16 | A2: the ledger and same-viewport source/implementation captures cover the mapped panel and all listed breakpoints/states; comparison has no material mismatch; scenario/security review also passes. | `docs/frontend/M1-T16_REFERENCE_LEDGER.md`, the checkpoint's 64 capture record, and a rerun of `m1-t16.visual.spec.ts` with source and implementation servers. Empty-state captures were visually inspected at 1440px and 485px; responsive overflow, focus, dialog, error, result, long-content, and drawer assertions passed. | PROVEN |
| SRC-REF-M1-T16 | A3/A5: mapped source regions, CSS, assets, and interactions are converted to typed components with T02 fixtures; no prototype runtime or invented authority. | The reference ledger maps toolbar, stats, cards, empty/error panel, modal form, controls, source CSS groups, local SVG icon roles, breakpoints, and translated interactions to `AdminPromotionWorkspace`, its subcomponents, scoped styles, and `AdminWorkspaceFrame`. Targeted source review found no runtime storage, unsafe HTML sink, prototype script, external origin, or price/date-policy calculation path. | PROVEN |
| A4-SHARED | Route-specific content is inside `AdminWorkspaceFrame`; no copied/overridden shell; shared Bookings, Availability, and Promotions behavior remains green at the specified widths. | `shared/admin-workspace-frame.tsx` owns shell/navigation/drawer behavior; promotion route supplies only frame props and unique content. The production browser suite passed shared-frame checks for `/admin`, `/admin/availability`, and `/admin/promotions` at 1440, 930, 720, and 485px. | PROVEN |
| SEC-M1-01/02 | Preserve future authorization/validation seams and safe non-authoritative fixture rendering; no live identity, persistence, publication, export, price authority, or booking mutation. | `ScenarioResult` provenance is rendered; export is disabled; proposed actions return explicit local non-authoritative messages; reload retains the original fixtures; public negative assertion excludes the admin draft. `npm run check` and the production browser suite passed; targeted scan found no prohibited runtime pattern. | PROVEN |

## Preconditions and repository resolution

- The active ticket is `awaiting_review`, and its recorded review target matches `HEAD` exactly: `2ee952c9d6230849ff70e2db62efcc76454ecad9`.
- The code worktree is clean, so the review target is unambiguous.
- `atlas-workflow.yaml` and `atlas-workflow.local.yaml` resolve the artifact store to `C:\Workspace\bride-assistant-artifacts`; its origin matches the configured expected remote and `.atlas/atlas-artifacts.yaml` declares `layout: atlas-workflow`.
- The artifact repository had only pre-existing untracked `.gitkeep` paths. They were preserved; this review stages only this feedback artifact.
- Predecessor CK artifacts for M1-T02 and M1-T03 record `PASS`, so their approved fixture and visual-system boundaries were consumed without reopening them.

## Validation and evidence

- `npm run check` — exit 0: ESLint, TypeScript, 13 Node boundary tests, and production `next build` passed.
- `$env:M1_T08_BASE_URL='http://localhost:3000'; $env:M1_T16_REFERENCE_URL='http://127.0.0.1:3001'; $env:M1_T16_CAPTURE_DIR='C:\Users\ASUS\AppData\Local\Temp\m1-t16-ck-review'; npx playwright test tests/m1-t16.visual.spec.ts` — 3/3 passed in 15.0s.
- The browser suite exercised source/implementation captures at 1440, 1251, 1250, 1151, 1150, 931, 930, 721, 720, and 485/486px; it asserted no horizontal overflow at each boundary.
- The editor scenario exercised keyboard focus, native form fields, percent validation, date validation path, package selection, proposed result, reload persistence of fixture-only state, Escape close, publish result, empty/error/long/result routes, and the public negative authority check.
- The shared-frame scenario exercised Bookings, Availability, and Promotions at 1440, 930, 720, and 485px, including mobile drawer open/close and Escape behavior under reduced motion.
- Direct source review confirmed the committed path `AdminWorkspaceFrame` navigation → promotion route → `getScenario` → typed `adminPromotions` read model → `AdminPromotionWorkspace` → rendered cards/empty state/editor/result. No legacy promotion caller, fallback storage path, compatibility wrapper, migration, or persistence path exists.
- Empty-state review confirmed `admin.promotions-empty` produces zero stats and the source-derived empty panel through the same adapter path, with no initialization or durable seed. The error route uses an explicit error state, and the long route uses the same source-derived package-selection region with wrapped fixture text.
- Visual review of the rerun captures found the route-specific empty composition aligned with the reference at desktop and narrow widths. The populated comparison intentionally uses T02 fixture records (two in the app versus one temporary source comparison record); the checkpoint records this fixture-boundary difference and the visual comparison remains focused on the source-derived card geometry, order, typography, controls, and responsive behavior.
- Migration inventory is not applicable: T16 authorizes no SQL, persistence, production data, seed, or storage migration.

## Frozen Finding Closure Matrix

No `CK-###` findings were identified. All applicable frozen Review Contract rows are proven, so there are no closure clauses to freeze.

## Scope-change observations

None. No ticket change, new provider/runtime/deployment target, architecture or product decision, policy resolution, or predecessor reopening is required to decide this review.

## Decision

`PASS`. The committed checkpoint satisfies the frozen M1-T16 scope, its A1–A5 amendments, the consumed T02/T03 dependency boundaries, and all explicit review obligations. The artifact is bound to code commit `2ee952c9d6230849ff70e2db62efcc76454ecad9` and records only checks performed for this review.
