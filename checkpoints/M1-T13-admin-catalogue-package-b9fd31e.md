# M1-T13 — admin catalogue and package fixture views

- Ticket: `M1-T13`; batch `BAP-M1-FIXTURE-UI-SET-01 / 3`; state `awaiting_review`.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Review target code commit: `b9fd31e3bd42f2a902dd6201445f471f677ac615` (`feat: add fixture admin catalogue and package views`).
- Dependencies T02/H-02 and T03/H-03 have CK `PASS` artifacts. No predecessor was reopened.
- Source and visual ledger: [M1-T13 reference ledger](../evidence/M1-T13-reference-ledger.md). Captures: [M1-T13 evidence](../evidence/M1-T13/).

## Bounded implementation

`/admin/catalogue`, `/admin/packages`, and their named fixture-state routes now render the mapped Services & Actions and Master Packages panels inside the shared `AdminWorkspaceFrame`. Typed T02 scenario fields supply items, bundles, selections, prices, state, provenance, and result copy. Search, tabs, type filtering, selection preview, and modal controls use local presentation state. Native validation and actions produce a non-authoritative result without changing the fixture. The shared frame gained only the two active navigation destinations.

## Review Contract Closure

| Row | Ticket authority and pass condition | Required proof; evidence / exact command and outcome | Status |
| --- | --- | --- | --- |
| RC-23 | Frozen ticket: every catalogue hierarchy/price empty-error-proposed-edit scenario routes through T02 without mutation; no prototype seed/state save or M3 catalogue authority. | Named `admin.catalogue-*` fixtures and routes; Playwright empty/error/proposed captures for both panels, reload after proposed edit shows original eight records; source/import scan. `npm run check` PASS (12 Node tests, lint, typecheck, build); `npx playwright test tests/m1-t13.visual.spec.ts` PASS (4/4). | PROVEN |
| VIS-REF-M1-T13 | A2 and remaining UI contract: source ledger, same-viewport desktop/breakpoint captures and state/keyboard/security evidence, no material in-scope visual mismatch. | Ledger, paired full-page captures at 1440, 1251/1250, 931/930, 721/720, 486/485, 420px for both panels; width/overflow assertions; additional tab/editor/state/focus captures. The package-chip and 485px toolbar mismatches were corrected before final captures. `npx playwright test tests/m1-t13.visual.spec.ts` PASS (4/4). Shared shell and prohibited prototype hint wording are documented exceptions. | PROVEN |
| SRC-REF-M1-T13 | A3/A5: mapped HTML/CSS/visible interactions converted into Next.js components; every visible decision traces to source, shared frame, fixture, or explicit direction; no raw prototype script. | Ledger source-region/CSS/asset/interaction map; `AdminCatalogueWorkspace`, scoped module, frame, typed scenario fields, local SVG sheet. Source scan and `npm run check` PASS. No unsupported source gap was filled. | PROVEN |
| A4-SHARED | A4: unique content in `AdminWorkspaceFrame`; no duplicate or selector override; shared regressions at desktop/930/720/485px. | Both new routes pass frame props; only frame nav destinations changed. Playwright shared-route capture/overflow checks for Bookings, Availability and Catalogue at 1440, 930, 720 and 485px; 4/4 visual spec PASS. | PROVEN |
| VIS-003/06/08/10/11 | Frozen UI review: Entity Library/Settings Form, long names/composition, responsive reflow, labels/focus, empty/error/result/validation state. | Catalogue grid, package cards, editor forms; `/admin/packages/long` long-name capture; native required-name validation capture; keyboard focus and Escape assertion; source/implementation breakpoint captures. `npm run check` and visual spec PASS. | PROVEN |
| SEC-M1-01/02 | Preserve future admin authorization seam and safe rendering; no persistence, live business rule, unsafe sink, external media, or historical booking claim. | Adapter-only typed fixture path; apparent actions use `scenario.interaction.summary`; source scan for unsafe APIs returned no matches; boundary test PASS within `npm run check`; source hint's booking-history claim replaced by `scenario.provenance.label` in the same mapped hintbox. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK `PASS`.

## Actual Use Path

`app/(admin)/admin/catalogue/page.tsx` → `AdminCatalogueWorkspace("admin.catalogue-services")` → `getScenario` → typed T02 `adminCatalogue` read model → counters, tabs, item/bundle cards, editor and notice → reviewer browser. `/admin/packages` follows the same path for `admin.catalogue-packages` into `PackageGrid` and the package editor. Dynamic state routes select the named empty, error, proposed-result, and long-content fixtures. The effect of an apparent edit/delete/archive is only local `role="status"` text from T02 interaction provenance. Playwright executes the route, filter, dialog, validation, reload, and responsive chains. No source prototype script, compatibility path, or operational catalogue write is reachable.

## Actual Empty-State Evidence

The `admin.catalogue-empty` and `admin.catalogue-packages-empty` scenarios contain zero items, bundles, and packages. `/admin/catalogue/empty` and `/admin/packages/empty` render the source-derived card/empty treatment with each scenario's absence message; captures `implementation-empty.png` and `implementation-packages-empty.png` show the first handoff. Search no-match is a local derived empty view and reset returns the original fixture. No default data is generated for those routes; repeated reads are idempotent.

## Migration Inventory

No SQL migration, production-data change, seed, or durable test setup is authorized or present. Playwright creates isolated browser contexts; fixture arrays are in-memory typed values with no teardown or persistence path.

## Source boundary and limits

The source `hintbox` claims live bundle updates and booking snapshot history. The ticket forbids those claims, so the committed view uses the source hintbox region with T02 provenance copy. Source admin shell navigation at narrow widths is superseded by A4's right-side drawer. Those are the recorded visual differences. M3 retains authoritative catalogue, price, and promotion behavior.
