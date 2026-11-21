# CK review — M1-T10 customer booking-request fixture workflow

- Ticket: `M1-T10-customer-booking-request-fixtures`
- Batch: `3 / M1 / M1-W05`
- Code repository: `C:\Workspace\bride-assistant`
- Reviewed commit: `505f393038ac2b391070aaee2a2d7652667aee48` (`feat: add customer booking request fixture`)
- Checkpoint: `M1-T10-customer-booking-request-fixtures-505f393.md`
- Ticket state at review: `awaiting_review`
- Review type: first consolidated CK review
- Result: `CHANGES_REQUIRED`

## Review Contract disposition

The active ticket, its A1/A2/A3 amendments, the submitted source-to-component
ledger, the committed route wiring, and the required fixture/security evidence
were reviewed. The implementation path is production-reachable through
`/booking` and the named state routes. `npm run check`, the ticket-specific
Playwright suite, and the non-authoritative boundary assertions passed. One
ticket-bound visual/source-fidelity deficiency remains.

| Row | Ticket authority | Evidence and validation | Status |
| --- | --- | --- | --- |
| RC-20 | Required, invalid, selected, unavailable, no-package, and success-result states render through T02; success stays non-authoritative; no storage/official transition. | `tests/m1-t10.visual.spec.ts` passed 6/6, including named routes, date selection, submit result, empty storage, and local-only links. | PROVEN |
| VIS-REF-M1-T10 | Ledger plus same-viewport reference/implementation captures at desktop and each named breakpoint; no material mismatch. | `tests/m1-t10.visual.spec.ts` passed 6/6 and the captured desktop comparison was inspected. The booking footer differs materially from the reference. | FAILED |
| SRC-REF-M1-T10 | Mapped source regions/CSS/assets/interactions are converted to components and T02 fixtures; no prototype runtime. | Checkpoint ledger and route/component source were inspected; booking workflow is converted and fixture-backed, but the mapped reference footer composition is not preserved. | FAILED |
| SEC-M1-01 / SEC-M1-02 | Preserve future ownership/transaction seam; safe rendering; no unsafe sink, inline executable source, storage, external media, or client authority. | `npm run check` passed; Playwright verified empty `localStorage`/`sessionStorage` and local/hash-only anchors; boundary scan passed. | PROVEN |
| H-10 / Actual use path | Declared scenario/version, selected display state, result state, and M3 successor seam without retry/persistence/idempotency claim. | `/booking` → `CustomerBooking` → `getScenario` → typed T02 catalogue → local workflow/result was traced in source and checkpoint evidence. | PROVEN |

## Frozen Finding Closure Matrix

### CK-001 — Booking footer does not preserve the mapped reference composition

| Clause | Exact ticket authority | Unsatisfied evidence | Observable correction/proof | Binary closure oracle | Direct-regression boundary |
| --- | --- | --- | --- | --- | --- |
| CK-001.a | M1-T10 UI Reference fidelity; M1-T10-A2 `VIS-REF-M1-T10`; M1-T10-A3 `SRC-REF-M1-T10` require the mapped `booking.html` regions and responsive composition to be reproduced with no material visible mismatch. | At the reviewed commit, `booking.html` has a compact centered footer (`.footer`, `© … Ranur Bridal Care · Your moment, our heart. ♡`), while `/booking` renders the shared expanded footer from `shared/customer-home.tsx` with brand copy, navigation links, and a two-row layout. This is visible in `evidence/M1-T10/reference-booking-desktop.png` versus `evidence/M1-T10/implementation-booking-desktop.png`; the implementation source is `shared/customer-home.tsx` at its footer JSX. | Adapt the booking route/frame so its footer matches the mapped `booking.html` footer's structure, copy, spacing, alignment, responsive behavior, and local-link/security boundary. Re-run the required same-viewport captures at desktop and all named breakpoint edges, inspect the side-by-side comparison, and update the source-to-component map/evidence. | `shared/customer-booking.tsx` rendered at `/booking` and every named state uses a footer whose ordered regions, visible copy, geometry, and responsive treatment match the reference footer at 1280, 1051/1050, 1001/1000, 821/820, and 641/640; the updated captures show no material footer mismatch, while `npm run check` and the 6-test T10 suite remain passing. | Only the booking workflow frame/footer, its local styles, the named booking states, and direct visual/evidence tests needed to verify this correction. Do not broaden into unrelated customer screens or backend behavior. |

## Validation performed

- `npm run check` — PASS: ESLint, TypeScript, 9/9 Node boundary tests, and production build.
- `$env:M1_T10_BASE_URL='http://localhost:3004'; $env:M1_T10_REFERENCE_URL='http://127.0.0.1:3001'; npx playwright test tests/m1-t10.visual.spec.ts` — PASS: 6/6.
- Desktop reference/implementation evidence under `.atlas/evidence/M1-T10/` — inspected; footer mismatch recorded above.
- `git show --stat` and `git diff-tree` — reviewed the committed implementation paths; unrelated untracked `project's goal/` and `test-results/` do not alter the committed review target.

## Decision

`CHANGES_REQUIRED`. The committed work satisfies the fixture, route, boundary,
and interaction obligations, but the booking surface does not yet satisfy the
ticket's exact mapped-reference footer composition. The frozen closure oracle
above is the complete admissible CFC target; no HMN authorization is created by
this review.
