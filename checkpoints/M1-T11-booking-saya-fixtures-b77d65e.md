# M1-T11 — Booking Saya fixture views: visual correction

- Ticket: `M1-T11`
- State: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target commit: `b77d65e75f21bb9e03de14903bee3423ca31e1b8` (`fix: align Booking Saya intro with source`)
- Predecessor checkpoint: `c5f621b6a22ba7d9e81bd78f55b712e7f906bb15`
- Authority: M1-T11, its mapped `project's goal/UI Reference/booking_saya.html`, and the direct browser feedback identifying the intro mismatch.

## Bounded correction

The Booking Saya intro now uses the mapped source's exact type family, title treatment, color values, spacing, gradient, breakpoint width, action grouping, and action shadow/hover behavior. The scoped rules also override only the shared CustomerFrame broad paragraph/link inheritance that had changed the eyebrow and primary-action colors.

No fixture authority, route semantics, scenario data, lifecycle labels, storage, external link, browser write, or network behavior changed.

## Review contract closure

| Row | Required proof | Evidence | Status |
| --- | --- | --- | --- |
| VIS-REF-M1-T11 | Source-derived intro visual fidelity | A same-viewport computed-style test compares the reference and implementation intro background, border, padding, typography, colors, spacing, and action treatment. Source and implementation captures at desktop plus 1051/1050, 821/820, and 641/640 are in `evidence/M1-T11-r2/`. | PROVEN |
| RC-21 | Fixture behavior remains inspectable | `tests/m1-t11.visual.spec.ts` passed 10/10, including every named lifecycle fixture, empty/loading/error, selected detail, filter, result boundary, keyboard menu, and responsive captures. | PROVEN |
| SEC-M1-01 / SEC-M1-02 | No authority expansion | The correction is CSS/markup/test-only. Existing T02-only fixture and no-storage/external-origin checks remain green. | PROVEN |

## Validation

- `npm run check` — PASS: lint, TypeScript, 10 Node boundary tests, and production build.
- `M1_T11_BASE_URL=http://localhost:3000 M1_T11_REFERENCE_URL=http://127.0.0.1:3001 M1_T11_CAPTURE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T11-r2 npx playwright test tests/m1-t11.visual.spec.ts` — PASS: 10/10.
- Browser visual review at `http://localhost:3000/booking-saya` — PASS: corrected intro observed against the mapped HTML.
