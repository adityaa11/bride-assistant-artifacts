# CK review — M1-T04 public discovery fixtures

- Ticket: `M1-T04`
- Batch/milestone: `3 / M1 / M1-W04`
- Result: `CHANGES_REQUIRED`
- Review type: first consolidated CK review
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed commit: `16a9a3d392e91ea6fee942bc9fd48fbfbd7d309d` (`fix: finalize M1-T04 responsive and LCP behavior`)
- Frozen ticket: [M1-T04](../tickets/m1-fixture-ui-assembly/M1-T04-public-fixtures.md)
- Submitted checkpoint: [M1-T04 implementation checkpoint](../checkpoints/M1-T04-public-fixtures-16a9a3d.md)

## Active review tuple

- Ticket state: `awaiting_review`.
- Authorized checkpoint: `M1-T04-public-fixtures-16a9a3d`.
- Review target: `16a9a3d392e91ea6fee942bc9fd48fbfbd7d309d`.
- The checked-out code branch is two commits later at `5b45033ba06cce63962e0e7bd0171086806d281e`; both later commits are documentation-only, and the reviewed T04 implementation paths are unchanged. The target SHA was reviewed directly.
- The code worktree contains only the pre-existing untracked path `project's goal/`; no tracked or staged change makes the explicit review target ambiguous.

## Review Contract disposition

| Row | Frozen authority | Required condition | Evidence reviewed | Status |
| --- | --- | --- | --- | --- |
| RC-04 | M1-W04 | Public discovery, packages, promotions, and published-content scenarios render through T02/T03 fixture seams; no copied static page behavior. | `app/(public)/page.tsx`, the named empty/error routes, `shared/public-home.tsx`, `shared/public-home/sections.tsx`, `shared/scenarios/adapter.ts`, and `shared/scenarios/catalogue.ts`. The public caller reaches the adapter and renders packages/content, but the committed `public.discovery-success` promotion is never passed into or rendered by `PublicHome`/`PackagesSection`. | UNRESOLVED |
| RC-05 | Project publication invariant | Draft/unauthorized fixture content never appears in public scenarios, including empty/error states; visual hiding alone is forbidden. | Public `ScenarioResult` typing and catalogue keep public data in `publishedContent`; `PublicHome` has no draft/publication import. Browser AX review of `/`, `/fixtures/empty`, and `/fixtures/error` showed no draft/customer/admin data; empty/error showed only the declared package absence/error and non-authoritative provenance. | PROVEN |
| VIS-04 | Visual contract / VIS-002..011 | Discovery hierarchy, CTA, readable responsive cards/lists, focus and empty/error states pass visual/keyboard captures; build-only proof is forbidden. | Browser inspection of the default public route showed the editorial hero, package cards, focusable actions, FAQ controls, and readable section hierarchy. AX review of empty/error routes exposed their declared messages. The committed responsive CSS contains the frozen `830.98px` and `600.98px` inclusive transformations, 560px/430px art regions, and reduced-motion rule. | PROVEN |
| VIS-REF-04 | M1-T04-A2 rendered-reference contract | Desktop, 830px, and 600px renders match the reference with no material visible mismatch, proven by same-viewport captures/comparison. | Submitted checkpoint comparison records browser reference/implementation captures at 890px, 830px, and 600px. Direct source review confirms the mapped local assets, shell, hero layering, typography roles, breakpoint transformations, and route-local CSS; the default browser capture showed the committed editorial composition. | PROVEN |
| SEC-04 | SEC-M1-01/02 | Public UI treats fixtures as display data, safely renders text, and carries no admin/customer data or external origin. | Targeted source scan and `tests/workspace-boundaries.test.mjs` cover unsafe sinks, browser storage, external origins, adapter boundaries, lifecycle/publication distinctions, and route composition. `npm run check` exited 0. | PROVEN |

## Validation and evidence

- The artifact store was resolved from `atlas-workflow.yaml` and `atlas-workflow.local.yaml` to `C:\Workspace\bride-assistant-artifacts`; its `origin` matches the configured expected remote and `atlas-artifacts.yaml` declares `layout: atlas-workflow`.
- The artifact repository had no pre-existing staged or tracked modifications. Its pre-existing untracked inventory (`checkpoints/.gitkeep`, `evidence/`, `feedback/.gitkeep`, `tickets/.gitkeep`) was preserved; this review writes only the new feedback artifact.
- `npm run check` exited 0: ESLint, TypeScript, all six Node boundary/catalogue tests, and production `next build` passed.
- Browser AX review reached `/`, `/fixtures/empty`, and `/fixtures/error`. The default route showed the published package cards and no promotion text; empty showed `No published packages are available in this fixture.` and `Tidak ada paket yang sedang dipublikasikan.`; error showed `The package fixture could not be prepared.` and the same empty package result. No draft text appeared.
- Keyboard review focused the skip link and brand link in order. The header menu control exposed collapsed/expanded state, and the public page exposed focusable hero actions, package actions, FAQ controls, and fixture messages.
- `shared/scenarios/catalogue.ts:28` supplies the success promotion (`welcome-care`, `10%`, `31 Desember 2026`). `shared/public-home.tsx:30-32` passes only `publishedContent`, `packages`, and `state`; `shared/public-home/sections.tsx:46-47` accepts only `packages` and `state` for the products section. There is no promotion render path or promotion styling in the committed public-home implementation.
- The pre-existing `shared/public-discovery.tsx` still contains an unused promotion renderer, but no public route calls it. This review does not require retaining that superseded path; the finding is against the actual committed public route.
- No migration, persistence, seed, external origin, public write, or new provider/deployment decision is in scope for this ticket.

## Frozen Finding Closure Matrix

### CK-001 — Published promotion is dropped from the public success path

| Clause | Ticket authority | Unsatisfied evidence | Observable correction / proof | Direct-regression boundary |
| --- | --- | --- | --- | --- |
| CK-001.a | M1-T04 Review Contract `RC-04`: “public discovery, packages, promotions, and published-content scenarios render through T02/T03 fixture seams”; M1-T04-A2 requires the rendered public fixture to match the bound reference composition. | The typed adapter success scenario contains one published promotion at `shared/scenarios/catalogue.ts:28`, but `shared/public-home.tsx:30-32` does not pass `readModel.promotions`, and `PackagesSection` at `shared/public-home/sections.tsx:46-47` has no promotion input or output. The browser default route rendered package cards but no `A little more care for your moment`, `10%`, or `31 Desember 2026` promotion. | On the committed public success route `/`, the promotion supplied by `public.discovery-success` is rendered in the public products/discovery composition through the adapter-backed typed read model, with its title, description, value, and end date visible in the reference-aligned treatment. A browser AX/text capture of `/` must show the promotion fields, and `npm run check` must remain green. The named empty/error routes must continue to render no promotion when their adapter arrays are empty. | Only the public fixture composition, its adapter consumption, and direct visual/publication regressions are in scope. Do not add CMS/API/persistence, public writes, external media, customer/admin data, or a new scenario authority. |

The closure oracle is binary: `PASS` only when the public success route visibly renders the adapter-provided published promotion and the direct empty/error/publication/security boundary remains green; otherwise `UNRESOLVED`.

## Scope-change observations

None. The finding is implementation-repairable within the frozen public fixture contract; no ticket change, provider/runtime/deployment target, architecture or product decision, policy resolution, or predecessor reopening is required.

## Decision

`CHANGES_REQUIRED`. `RC-04` remains unproven because the committed public success path drops its adapter-provided promotion. The single finding and closure oracle above are frozen for bounded remediation. This artifact is bound to code commit `16a9a3d392e91ea6fee942bc9fd48fbfbd7d309d` and records only checks performed for this first review.
