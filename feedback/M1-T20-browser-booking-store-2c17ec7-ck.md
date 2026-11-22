# CK review — M1-T20 browser booking store removal

- Result: `PASS`
- Ticket: `M1-T20`
- Batch / milestone / work package: `BAP-M1-FIXTURE-REPAIR-01 / 1 / M1-W05 with M1-W07 regression handoff`
- Ticket state at review: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Reviewed code commit: `2c17ec7cf247984a88a2bb1cf90e0a318ef5e4c7` (`fix: remove browser booking fixture store`)
- Frozen ticket: [M1-T20](../tickets/m1-fixture-review-repairs/M1-T20-booking-browser-store-regression.md)
- Reviewed checkpoint: [H-20 browser booking store removal](../checkpoints/M1-T20-browser-booking-store-2c17ec7.md)
- Reviewed evidence: [M1-T20 browser captures](../evidence/M1-T20/)
- Review type: first consolidated CK review against the frozen T20 contract

## Review scope and preconditions

The active-ticket tuple is unambiguous: M1-T20, batch 1, state
`awaiting_review`, authorized checkpoint
`M1-T20-browser-booking-store-2c17ec7.md`, and review target
`2c17ec7cf247984a88a2bb1cf90e0a318ef5e4c7`. The code repository `HEAD`
matches that SHA and the code worktree was clean. The artifact store resolved
from `atlas-workflow.yaml` and `atlas-workflow.local.yaml` to the configured
Git worktree at `.atlas`; its origin matches the configured artifact remote and
`atlas-artifacts.yaml` declares the `atlas-workflow` layout. Pre-existing
untracked `.gitkeep` placeholders in the artifact store were preserved and are
not part of this review artifact.

This review is limited to the frozen T20 repair: retiring the browser booking
store, preserving the T02 customer fixture reads and named empty/lifecycle
states, proving the non-authoritative local result and same-context no-write
behavior, and checking the T09–T11 visual/accessibility regression boundary.
T19's independent audit, T21's selector repairs, real booking submission,
account/session behavior, backend storage, and the wider fixture graph are not
imported into scope.

## Review Contract assessment

| Row | Ticket authority and required proof | CK evidence and assessment | Status |
| --- | --- | --- | --- |
| `RC-REP20-01` | T20 user direction/T10 RC-20: actual `/booking` submission shows only the approved non-authoritative result, creates no booking ID or record, writes no `localStorage`/`sessionStorage`, and retires all browser-store functions and callers. | `tests/m1-t20.browser.spec.ts` exercised the real route with completed fictional inputs, selected 12 December 2026, submitted, checked the result wording and `Menunggu Konfirmasi`, and found empty browser storage with no fictional name or `BR-2026-003`. The committed diff deletes `shared/scenarios/customer-booking-store.ts` and removes the three production imports/callers. The production source scan found no store symbol, key, browser-storage call, or unsafe sink in `app`/`shared`. | `PROVEN` |
| `RC-REP20-02` | T20 user direction/T09/T11/T02: in the same isolated browser context, `/beranda` and `/booking-saya` retain only declared T02 records after submit and reload; named empty/error/lifecycle states remain fixture-backed. | The focused spec used one browser context across submit, home, history, reload, and both empty routes. It retained `BR-2026-001`/`BR-2026-002`, showed `2 booking`, found no fictional name or `BR-2026-003`, and kept storage empty. The affected T09/T11 route matrix passed, including named empty, loading, error, payment, cancellation, cancelled, and reschedule states. | `PROVEN` |
| `RC-REP20-03` | T09–T11 visual/accessibility contracts: affected route compositions remain usable on desktop/narrow widths with focus, status semantics, reduced motion, and no overflow; required project checks remain green. | The focused browser proof passed its skip-link focus, live result/status, reduced-motion, and 640px no-overflow assertions. The committed six T20 captures were visually inspected for booking result, home, and history at desktop/narrow widths. The affected browser matrix passed 20/20, and `npm run check` passed lint, typecheck, 13 Node tests, and production build. | `PROVEN` |
| `SEC-REP-01` | T20 security boundary: no browser booking/contact persistence, client authority, unsafe sink, or cross-customer display is added. | Same-context storage checks ran before and after submit and after route reloads. The fictional name and generated ID were absent from customer views. The `app`/`shared` scan found no browser-store symbols, `localStorage`, `sessionStorage`, `dangerouslySetInnerHTML`, or old event/listener path. T02 scenario provenance remains the only customer data source. | `PROVEN` |
| `RC-REP20-04` | T20 H-20/T17 F-01 handoff: exact commit, scenario/version, retired path, browser no-write proof, visual/state evidence, bounded F-01 closure, and T21/T17 consumers are identified. | The checkpoint binds the exact commit, `m1-fixture-v1`, all affected route/scenario IDs, retired module/import path, focused same-context proof, six captures, and T09/T10/T11 checks. It limits F-01 closure to the browser-store boundary, names T21 and T17-A7 as consumers, and explicitly does not claim separate customer/admin fixture identity repair. | `PROVEN` |

## Validation and evidence performed

- Verified the ticket state, checkpoint binding, exact code SHA, clean code
  worktree, artifact-store manifest, configured remote, and artifact-store
  pre-existing untracked placeholder inventory.
- Inspected the frozen ticket, H-20 checkpoint, implementation diff, focused
  Playwright spec, package scripts, and the six committed T20 captures.
- Traced the actual paths: booking route → typed T02 adapter → `CustomerBooking`
  → local validation/result, and home/history routes → typed T02 adapter →
  `CustomerHome`/`CustomerBookingHistory` → declared fixture records/states.
- Confirmed the deleted browser-store module and absence of its imports,
  functions, key, event listeners, browser-storage calls, and unsafe sink in
  production `app`/`shared` source.
- `npx playwright test tests/m1-t20.browser.spec.ts tests/m1-t09.visual.spec.ts tests/m1-t11.visual.spec.ts tests/m1-t10.visual.spec.ts --grep-invert "keeps date selection, focus, and submission"` — PASS, 20/20. The run used the repository's local Next.js development server because `playwright.config.ts` intentionally supplies a base URL without a `webServer` launcher.
- `npm run check` — PASS: ESLint, TypeScript, 13 repository tests, and the
  Next.js production build.
- The known stale T10/T12 selector assertions remain explicitly handed to
  M1-T21 by the frozen checkpoint; they were not counted as T20 proof or
  silently presented as a full legacy-suite PASS.

## Frozen Finding Closure Matrix

No blocking findings were identified. Therefore no `CK-###` findings or
closure clauses are frozen for CFC. The five proven Review Contract rows above
are the complete first-review assessment; the checkpoint's handoff and known
T21 selector observations are not CK implementation-repair findings for T20.

## Scope-change observations

None. No product, architecture, provider/runtime, deployment, policy, or
predecessor-reopening decision is required to resolve this checkpoint. The
separate fixture-identity model and T21 selector work remain outside T20 as
frozen.

## Decision

`PASS`. The committed M1-T20 checkpoint satisfies the frozen ticket contract
and its explicit review obligations. The browser booking-store boundary is
closed for T17 F-01 handoff; T21 may consume H-20 under its own authorization.
No CFC or HMN authorization is required.
