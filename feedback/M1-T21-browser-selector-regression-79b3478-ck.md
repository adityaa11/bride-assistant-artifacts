# CK review — M1-T21 browser selector regression repair

- Result: `PASS`
- Ticket: [M1-T21](../tickets/m1-fixture-review-repairs/M1-T21-browser-selector-regression.md)
- Batch / milestone / work package: `BAP-M1-FIXTURE-REPAIR-01 / 2 / M1-W07 verification`
- Ticket state at review: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `79b3478995737dd73165afaba50369dcb5242a08` (`test: repair M1 booking browser assertions`)
- Frozen ticket: [M1-T21 browser selector regression repair](../tickets/m1-fixture-review-repairs/M1-T21-browser-selector-regression.md)
- Reviewed checkpoint: [H-21 browser selector regression repair](../checkpoints/M1-T21-browser-selector-regression-79b3478.md)
- Predecessor: [M1-T20 CK PASS](M1-T20-browser-booking-store-2c17ec7-ck.md) at `2c17ec7cf247984a88a2bb1cf90e0a318ef5e4c7`
- Review type: first consolidated CK review against the frozen T21 contract

## Review scope and preconditions

The active-ticket tuple is unambiguous: M1-T21, batch 2, state
`awaiting_review`, authorized checkpoint H-21, and review target
`79b3478995737dd73165afaba50369dcb5242a08`. The code repository `HEAD`
matches that SHA and the code worktree was clean after validation cleanup.

The artifact store resolved from the tracked `atlas-workflow.yaml` and ignored
`atlas-workflow.local.yaml` to `C:\Workspace\bride-assistant-artifacts`; its
Git origin matches the configured expected remote and
`.atlas/atlas-artifacts.yaml` declares the `atlas-workflow` layout. Existing
untracked `.gitkeep` placeholders in the artifact store were preserved and are
not part of this review artifact.

This review is limited to the frozen T21 repair: the T10 selected-date/form
assertions, the T12 exact status-message assertions, the affected route-suite
and T17 route/scenario evidence, and the bounded T17-A7 handoff. T21 does not
issue T17 or M1 `PASS`, change production behavior, repair T19 findings, or
authorize fixture, backend, storage, SQL, media, provider, or deployment work.

## Review Contract assessment

| Row | Ticket authority and required proof | CK evidence and assessment | Status |
| --- | --- | --- | --- |
| `RC-REP21-01` | T21 T10 RC-20: the actual `/booking` browser test selects 12 December 2026, proves rendered selected state and form date, completes fictional fields, reaches the local non-authoritative result, and retains zero browser storage plus T20 no-new-record proof. | `tests/m1-t10.visual.spec.ts:83-118` asserts `Dipilih`, the `Tanggal Wedding *` value, required fictional fields, non-authoritative result wording, absent fictional name/`BR-2026-003`, zero `localStorage`/`sessionStorage`, and local links. H-20's committed `tests/m1-t20.browser.spec.ts` remains in the same-context regression path. The full affected suite passed. | `PROVEN` |
| `RC-REP21-02` | T21 T12 RC-22: actual `/admin` actions identify the exact `New Booking tidak diterapkan` and `Save availability tidak diterapkan` messages without calendar-caption ambiguity; list/filter/calendar/empty and non-mutation checks remain. | `tests/m1-t12.visual.spec.ts:59-91` scopes both status assertions with `hasText` and retains the list, detail, filter, calendar, empty, and non-authoritative checks. The T12 tests passed in the full suite. | `PROVEN` |
| `RC-REP21-03` | T21 T17 RC-27: affected T08-T13/T16 route suite, T20 same-context regression, and T17 route/scenario evidence are rerun with exact outcomes and failures stated. | Independently ran the exact H-21 Playwright command with `--timeout 90000`: `42/42 PASS`. Independently ran `npm run check`: ESLint, TypeScript, 13 Node tests, and Next production build passed. Reviewed H-21's recorded read-only T17 route/scenario sweep: `55 routes × 2 viewports = 110/110 PASS`; no failure was reported. | `PROVEN` |
| `RC-REP21-04` | T21 H-21: bind the exact post-T20 commit and T20 CK, corrected T10/T12 observations, F-01/F-02 regression evidence, route/scenario evidence, and remaining T17 findings without self-issuing T17 `PASS`. | H-21 binds `79b3478`, H-20/T20 CK `PASS`, the corrected test locations, selected-date/form and exact-toast observations, same-context no-storage/no-new-record proof, the affected suite counts, the route matrix, and the unresolved T17/T19 handoff. The checkpoint explicitly leaves T17-A7 for its own GO review. | `PROVEN` |

The committed T21 diff from the exact H-20 code checkpoint contains only the
authorized changes in `tests/m1-t10.visual.spec.ts` and
`tests/m1-t12.visual.spec.ts`; no production code or fixture source changed.
The obsolete T10 `aria-pressed` expectation and broad T12 status expectations
are absent from the reviewed T21 assertions. No fallback `or` assertion or
`first()` toast workaround is present.

## Actual use path

`EUP-21` is proven as the test path: Playwright T10 invokes the existing
production-like `/booking` route, observes the selected calendar button and
form date, submits fictional values to the local result, and records the
assertion report. Playwright T12 invokes the existing `/admin` and
`/admin/availability` routes, observes the exact status messages and retained
fixture states, and records the report. The T20 browser spec independently
exercises the same isolated context across `/booking`, `/beranda`,
`/booking-saya`, reloads, and named empty routes. The path ends at the T17
reviewer; no production use path or endpoint was added.

## Validation and evidence performed

- Verified the frozen ticket, H-21 checkpoint, H-20 predecessor/CK PASS, exact
  code SHA, clean code worktree, artifact-store manifest, configured remote,
  and pre-existing artifact-store placeholder inventory.
- Inspected the T21 diff, current T10/T12 tests, relevant source selectors,
  and H-21's route/scenario and finding handoff evidence.
- Confirmed the T21 code diff is test-only relative to H-20 and that the
  obsolete selected-state/status assertions are not retained in the repaired
  tests.
- `npm run check` — PASS: ESLint, TypeScript, 13 Node tests, and Next
  production build.
- `npx playwright test tests/m1-t08.visual.spec.ts tests/m1-t09.visual.spec.ts tests/m1-t10.visual.spec.ts tests/m1-t11.visual.spec.ts tests/m1-t12.visual.spec.ts tests/m1-t13.visual.spec.ts tests/m1-t16.visual.spec.ts tests/m1-t20.browser.spec.ts --timeout 90000` — PASS, `42/42`.
- Reviewed H-21's exact-command record for the focused selector run (`2/2`),
  first-run timeout disclosure and successful rerun, and read-only T17 route /
  scenario sweep (`110/110`). The T17-A7 review itself was not claimed or
  executed by this checkpoint.

## Frozen Finding Closure Matrix

No blocking ticket-bound deficiencies were identified. Therefore there are no
`CK-###` findings and no `CK-###.a` closure clauses to freeze for CFC. The
four proven Review Contract rows above are the complete first-review
assessment. The separate T17/T19 findings and handoff observations are not
T21 implementation-repair findings.

## Scope-change observations

None. No product, architecture, provider/runtime, deployment, policy, or
predecessor-reopening decision is required. H-19's unresolved/planning-required
fixture-relationship findings remain outside T21 and are carried to T17-A7 as
the checkpoint states.

## Decision

`PASS`. The committed M1-T21 checkpoint satisfies the frozen ticket contract
and its explicit review obligations. The corrected T10/T12 browser assertions,
the T20 same-context negative regression, and the affected route validation
are proven at the exact reviewed commit. H-21 may proceed to the explicit
M1-T17-A7 GO re-review; this CK does not issue T17 or M1 `PASS`.
