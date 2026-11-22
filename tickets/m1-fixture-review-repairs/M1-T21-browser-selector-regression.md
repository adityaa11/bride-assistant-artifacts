# M1-T21 — Repair T10/T12 browser assertions and rerun M1 review checks

- State: `approved`; batch / milestone / work-package: `BAP-M1-FIXTURE-REPAIR-01 / 2`, M1 / M1-W07 verification. GO checkpoint: [H-21](../../checkpoints/M1-T21-browser-selector-regression-79b3478.md), code commit `79b3478995737dd73165afaba50369dcb5242a08`; CK review recorded `PASS`.
- Code repository and planning baseline: `https://github.com/adityaa11/bride-assistant.git` at `2ee952c9d6230849ff70e2db62efcc76454ecad9`.
- Source authority: explicit user direction on 22 November 2026 to check T10's current selected-date semantics, narrow T12's status assertion to `New Booking tidak diterapkan`, rerun the route suite and M1-T17 review; [T17 v1 F-02](../../evidence/M1-T17-v1.md), [T10 RC-20](../m1-fixture-ui-assembly/M1-T10-customer-booking-request-fixtures.md), [T12 RC-22](../m1-fixture-ui-assembly/M1-T12-admin-booking-availability-fixtures.md), and [T17 RC-27](../m1-fixture-ui-assembly/M1-T17-review-seam-inventory.md).
- Dependency: M1-T20 CK `PASS` and exact H-20 code checkpoint. Any available T19 audit findings are recorded for the later T17 re-review but do not authorize extra test or fixture changes here; an unfinished T19 remains explicitly outstanding.
- Successor/handoff: `H-21` test and route evidence to an explicit M1-T17-A7 GO re-review. T21 cannot issue T17 `PASS` or modify its frozen acceptance rows.
- Activation: `not-applicable` for production; this ticket changes test code and produces validation evidence only.
- Reuse: `RD-REP-03 reuse-existing` Playwright route harnesses and T17 matrix; no new test framework or production helper.
- Legacy: `forbidden`; remove obsolete selector expectations rather than retaining duplicate branches.
- SQL and empty state: `not-applicable` to production data; existing T10/T12 empty routes remain in the route suite.

## Outcome and exact scope

Correct the two known stale browser assertions without weakening what they prove. In `tests/m1-t10.visual.spec.ts`, after selecting 12 December 2026, assert the **current selected calendar state** and selected date in the form, then complete any required fictional form fields needed to reach submit. Keep the non-authoritative result, zero browser-storage, local-link, and no-new-record checks; consume T20's same-context regression rather than replacing it with a weaker unit test. In `tests/m1-t12.visual.spec.ts`, scope the `role="status"` assertion to the toast containing `New Booking tidak diterapkan`; if the same test's availability-result assertion has the same calendar-caption ambiguity, scope it to its exact intended `Save availability tidak diterapkan` message. Preserve the other T12 list/filter/calendar/empty assertions.

Run the affected T08–T13/T16 Playwright route suite and the M1-T17 route/scenario review checks against the exact post-T20 code commit. Record pass/fail, captured routes, scenario/version, desktop/narrow and known T17 findings. A failed T17 row remains failed and is handed to its own GO workflow; this ticket does not relabel M1 ready.

## Entrypoint, endpoint, handoff, expected use path

- Entrypoint/caller: Playwright invokes the existing T10 and T12 specs against the production-like M1 route server and mapped local reference server. Inputs are named route/scenario IDs and deterministic fictional form values. A missing route, wrong selected state, missing toast, or storage write fails the assertion; no silent retry or alternate UI is introduced.
- Endpoint: test code with precise semantic assertions; exact executed suite command/outcome and T17 review-matrix rerun evidence bound to the code commit. The observable result is a test report, not application state.
- `H-21`: test file locations, exact command/base URLs, browser/viewport, pass/fail counts, T10 date/form/result/storage observations, T12 toast/availability observations, and remaining T17 row statuses. Delivery is the GO checkpoint/evidence artifact to the T17 GO reviewer; no production transport, persistence or idempotency.
- `EUP-21`: Playwright test caller → actual `/booking` and `/admin` routes → selected calendar/result/toast → assertion report → T17 reviewer. No production `ExpectedUsePath` is created.

## True path, invariants and non-goals

The true test path uses one semantic selected-date assertion and one narrowly identified toast assertion for each relevant action. The old broad `getByRole("status")` and obsolete `aria-pressed="true"` expectation are removed; no fallback `or` assertion permits either state. Do not alter production calendar/button/toast code merely to satisfy an outdated test. Preserve T10 no-storage and no official booking result, T12 non-authoritative admin action, typed scenario/version, public/customer privacy, and the accepted source-derived visual contract. No UI redesign, new fixture data, backend, browser storage, official transition, SQL, media or provider work is in scope.

## Review Contract

| ID | Authority/type | PASS iff / required proof | Forbidden substitute / regression boundary |
| --- | --- | --- | --- |
| `RC-REP21-01` | User T10 selector request, T10 RC-20; `VALIDATION` | The actual `/booking` browser test selects the named date, asserts the rendered selected state and form date, fills required fields, reaches the non-authoritative result, and retains zero `localStorage`/`sessionStorage` plus T20 no-new-record proof. Test command and result recorded. | Removing selected-state or storage assertions, stale ARIA expectation, or a unit-only substitute. |
| `RC-REP21-02` | User T12 selector request, T12 RC-22; `VALIDATION` | The actual `/admin` test identifies the `New Booking tidak diterapkan` status toast without matching the calendar caption; any same-test availability status is scoped to its exact message. List/filter/calendar/empty and non-mutation checks remain and pass. | Broad `getByRole("status")`, `first()` workaround, or product code changed solely for the test. |
| `RC-REP21-03` | User rerun request, T17 RC-27; `REGRESSION/EVIDENCE` | Execute the affected T08–T13/T16 suite and T17 route/scenario matrix after T20, record exact commands/results and all remaining failed rows, then hand off to T17 GO. If the full suite fails, identify the exact failing assertion and do not claim readiness. | `npm run check` alone, selective success reported as full pass, or self-issued T17/CK `PASS`. |
| `RC-REP21-04` | User T17 dependency direction, T17 F-02 and T17-A7; `HANDOFF` | H-21 binds the exact post-T20 code commit, T20 CK result, corrected T10/T12 test locations, selected-date/form and exact-toast observations, same-context no-storage/no-new-record regression, affected suite command/counts, route/scenario matrix and remaining T17 failures. It maps F-02 to the observed test outcome and carries any available H-19 finding IDs forward as unresolved or already dispositioned, without deciding T17 `PASS`. | A green targeted selector test presented as full T17 review, omission of F-01 regression, or treating unfinished T19 as complete. |

## Security readiness, UI readiness, validation and hard stop

Security readiness: `minimal-relevance` for test-only changes. `SEC-REP21-01 preserve-seam`: retain the T10 negative browser-storage assertion, same-context no-new-record proof, and T12 non-authoritative action check; `RC-REP21-01/02` verify them. Scoped lens: OWASP ASVS 5.0.0 browser/data verification, OWASP Top 10: 2025 integrity/access, NIST CSF 2.0 Protect; no new control or policy is implemented. UI readiness: no rendered UI change; tests observe selected state and status semantics already defined by the approved frontend contract. Run `npm run check`, the named Playwright route suite with local source/app servers, and read-only T17 route/scenario checks. A remaining F-01 storage path, missing selected state, ambiguous toast, lost negative assertion, or failed direct regression blocks T21 handoff; unresolved T19 fixture-relationship findings are reported to T17/planning without being treated as authorized fixes in this ticket.
