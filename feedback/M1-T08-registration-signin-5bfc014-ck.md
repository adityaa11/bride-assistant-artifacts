# CK verification — M1-T08 registration and sign-in fixtures

- Ticket: `M1-T08-registration-signin-fixtures`
- Batch/milestone/work package: `3 / M1 / M1-W05`
- Code repository: `C:\Workspace\bride-assistant`
- Reviewed code commit: `5bfc014b7677367df379f89bd5284d4b95e10fe5`
- Frozen ticket: `tickets/m1-fixture-ui-assembly/M1-T08-registration-signin-fixtures.md`
- CFC checkpoint: `checkpoints/M1-T08-registration-signin-5bfc014-cfc.md`
- Original CK: `feedback/M1-T08-registration-signin-4035d71-ck.md`
- Review type: post-CFC verification
- Result: `PASS`

## Preconditions and validation

- The ticket remains `awaiting_review`; the CFC checkpoint records the same
  reviewed commit as `HEAD`.
- The code worktree has no tracked modifications. Pre-existing untracked
  `project's goal/` and `test-results/` paths were preserved.
- The artifact store was validated from `atlas-workflow.yaml` and
  `atlas-workflow.local.yaml`; `.atlas` is a Git worktree with the configured
  artifact remote and `layout: atlas-workflow`.
- `npm run lint` — passed.
- `npm run typecheck` — passed.
- `npm test` — 8 passed.
- `npx playwright test tests/m1-t08.visual.spec.ts` — 12 passed, including
  reference pairs, all named fixture states, keyboard focus, frozen controls,
  and no-authority assertions.
- A targeted Playwright browser check against the active Next entrypoint also
  passed close-button dismissal, backdrop dismissal, focus restoration, and
  empty local/session storage checks.
- `git diff --check 4035d713d0b12710b96e120475edaaa5726283b2
  5bfc014b7677367df379f89bd5284d4b95e10fe5` — clean.

## Frozen Finding Closure Matrix

The original `CK-001` clauses and closure oracles are carried forward
unchanged. This verification checks only those clauses, the remediation diff,
and direct regressions in the affected auth fixture paths.

| Clause | Status | Evidence and closure oracle | Direct-regression boundary |
| --- | --- | --- | --- |
| `CK-001.a` | `RESOLVED` | `shared/customer-auth.tsx` uses an independent `PasswordField` state for the registration confirmation field. The T08 Playwright test proves the control is visible, starts as `password`, toggles only that field to `text`, and exposes the pressed state. Evidence: `evidence/M1-T08/daftar-confirmation-visible.png`; full T08 suite passed. | Registration confirmation visibility control, direct styling, state toggle, and related captures/tests only. |
| `CK-001.b` | `RESOLVED` | The registration switch line renders the exact source copy `Sudah punya akun? Masuk di sini`; the T08 Playwright assertion passes and the refreshed paired captures record the rendered location. Evidence: `shared/customer-auth.tsx`; `evidence/M1-T08/implementation-daftar-*.png`. | Registration switch-line copy and direct visual evidence only. |
| `CK-001.c` | `RESOLVED` | The editorial brand is a local Next `Link` to `/`; the T08 Playwright assertion activates it and observes the local home URL. Evidence: `shared/customer-auth.tsx`; frozen-controls test passed. | Auth-page brand navigation and direct route behavior only. |
| `CK-001.d` | `RESOLVED` | `BackArrow` and `ForwardArrow` inline SVG components are used in the mapped back and submit controls, with source-derived sizing in `shared/customer-auth.module.css`. The two reference-pair suites passed at desktop and all listed breakpoint boundaries. Evidence: `evidence/M1-T08/reference-*.png` and `implementation-*.png`. | Auth-page back/submit icon treatment and direct responsive rendering only. |
| `CK-001.e` | `RESOLVED` | The sign-in fixture dialog focuses its close control on open, contains Tab, handles Escape, backdrop, and close-button dismissal, restores opener focus, and remains non-authoritative. The T08 keyboard test passed; the targeted Playwright check independently passed close-button/backdrop/focus restoration and empty storage assertions. Evidence: `shared/customer-auth.tsx`; `evidence/M1-T08/masuk-dialog-keyboard.png`. | Forgot-password fixture dialog focus/keyboard behavior and direct no-authority effects only. |

## Expected use path and direct regressions

The committed path remains:

`customer auth route → T02 scenario adapter → AuthFixture → visible fixture UI → reviewer`

The remediation changes only the frozen source controls and dialog behavior.
The route still creates no account or session, performs no credential or
storage write, uses no prototype script or unsafe HTML sink, and introduces no
external origin or role-based redirect. All direct regression checks passed.

## Decision

`PASS`. Every original `CK-001` closure oracle is proven at the reviewed CFC
commit, and no direct regression was found. No new finding, scope-change
observation, contract gap, or further CFC cycle is authorized by this review.
