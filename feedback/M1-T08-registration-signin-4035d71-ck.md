# CK review — M1-T08 registration and sign-in fixtures

- Ticket: `M1-T08-registration-signin-fixtures`
- Batch/milestone: `3 / M1 / M1-W05`
- Code repository: `C:\Workspace\bride-assistant`
- Reviewed code commit: `4035d713d0b12710b96e120475edaaa5726283b2`
- Frozen ticket: `tickets/m1-fixture-ui-assembly/M1-T08-registration-signin-fixtures.md`
- Checkpoint: `checkpoints/M1-T08-registration-signin-4035d71.md`
- Review type: first CK review
- Result: `CHANGES_REQUIRED`

## Validation and evidence

- Artifact store validated from `atlas-workflow.yaml` and `atlas-workflow.local.yaml`; `.atlas` is a Git worktree whose `origin` matches the configured expected remote and whose manifest declares `layout: atlas-workflow`.
- Checkpoint preconditions passed: ticket state is `awaiting_review`, the checkpoint commit matches `HEAD`, and the code worktree has no tracked modifications. Pre-existing untracked `project's goal/` and `test-results/` paths were preserved.
- `npm run lint` — passed.
- `npm run typecheck` — passed.
- `npm test` — 8 passed.
- `npx playwright test tests/m1-t08.visual.spec.ts` — 11 passed: both reference-route suites, eight named fixture-state routes, and the keyboard/no-authority test.
- Stored paired captures and state evidence were inspected under `evidence/M1-T08/`, including the `daftar` 700px pair where the missing confirmation-password control and copy difference are visible.
- Source comparison used the mapped local files `project's goal/UI Reference/daftar.html` and `masuk.html` and the implementation at `shared/customer-auth.tsx`.

## Review Contract status

| Row | Status | Review basis |
| --- | --- | --- |
| RC-18 | `PROVEN` | Both routes expose the named T02 fixture states and the route/test evidence shows no account or session artifact. |
| SEC-M1-01 | `PROVEN` | The implementation uses typed non-authoritative scenarios; the boundary test and keyboard test pass with empty local/session storage and local-only links. |
| SEC-M1-02 | `PROVEN` | The source boundary scan passes; no unsafe HTML sink, prototype script, external origin, or storage path is present in the reviewed route seam. |
| VIS-02/06/08/09/10/11 | `PROVEN` | Required labels, focus, state presentation, and narrow overflow checks pass in the recorded Playwright harness. |
| VIS-REF-M1-T08 | `UNRESOLVED` | The required source-visible fidelity is not complete; see `CK-001.a`–`CK-001.e`. |
| SRC-REF-M1-T08 | `UNRESOLVED` | The checkpoint mapping claims retained source interactions, but the committed component omits or changes mapped interactions; see `CK-001.a`–`CK-001.e`. |

## Finding

### CK-001 — Source-led reference fidelity and interaction translation is incomplete

The ticket's UI Reference fidelity clause requires the mapped `daftar.html` and `masuk.html` visible structure, copy, navigation, imagery/icon treatment, responsive composition, and visible script-driven interactions to be converted into the fixture-backed Next.js route. The submitted implementation and captures do not satisfy all of those explicitly frozen obligations.

## Frozen Finding Closure Matrix

| Clause | Exact ticket authority | Unsatisfied evidence / affected location | Observable correction and binary closure oracle | Direct-regression boundary |
| --- | --- | --- | --- | --- |
| CK-001.a | M1-T08 UI Reference fidelity; A3 source-led refactor; `SRC-REF-M1-T08` interaction-to-component map. The registration reference includes a visibility control for both password fields. | `project's goal/UI Reference/daftar.html:55` contains `data-toggle-password="confirmPassword"`; `shared/customer-auth.tsx:49` renders the confirmation password as a bare `Field` with no visibility button. The stored 700px pair shows `Lihat` in the reference confirmation field but not in the implementation. | The `/daftar` fixture must render a visible confirmation-password show/hide control with the reference label/pressed-state treatment and a React handler that toggles only that field. Oracle passes only when a Playwright assertion and same-viewport capture show the control in both default and relevant fixture states, with no account/session/storage authority. | Only the registration confirmation-field control, its direct styling, state toggle, and related captures/tests. |
| CK-001.b | M1-T08 UI Reference fidelity requires exact visible copy; A3 requires preserving source structure and visible treatment. | `project's goal/UI Reference/daftar.html:60` says `Masuk di sini`; `shared/customer-auth.tsx:55` renders `Masuk sekarang`. The submitted implementation capture therefore differs in visible copy. | The registration switch line must display the source copy `Sudah punya akun? Masuk di sini` at the same mapped location and treatment. Oracle passes only when the rendered `/daftar` capture and a text assertion show that exact copy without changing the fixture boundary. | Only the registration switch-line copy and its direct visual evidence. |
| CK-001.c | M1-T08 UI Reference fidelity requires source navigation and visible interaction treatment; A3 requires translating reference navigation into local Next behavior. | The reference brand link at `project's goal/UI Reference/daftar.html:43` and `masuk.html:43` targets the website home. `shared/customer-auth.tsx:32` targets `#auth-form-title`, so the visible brand navigation does not perform the mapped return-to-website action. | The brand control must navigate to the local customer/public home route while retaining the source-visible brand treatment. Oracle passes only when a route-level interaction assertion observes the local home URL after activation and the paired capture remains visually aligned. | Only the auth-page brand navigation target and direct route behavior; the existing `/` top-nav return control remains in scope for regression checking. |
| CK-001.d | M1-T08 UI Reference fidelity explicitly includes imagery/icon treatment and A3 requires preserving source-rendered relationships. | The reference uses SVG arrow artwork in the top navigation and primary submit controls at `project's goal/UI Reference/daftar.html:46,56` and `masuk.html:46,54`. `shared/customer-auth.tsx:41,51` substitutes text glyphs `←` and `→`. | The local implementation must use the extracted/local equivalent of the mapped arrow treatment in the same controls, with the source geometry/placement preserved. Oracle passes only when same-viewport captures show the mapped icon treatment in both routes and the visual comparison records no material icon mismatch. | Only the auth-page back and primary-submit icon treatment and its direct responsive rendering. |
| CK-001.e | M1-T08 A3 requires translating visible script-driven interactions; the binding rendered-reference contract requires checking dialog behavior, keyboard order, visible focus, and interaction treatment. | The reference dialog script at `project's goal/UI Reference/masuk.html:87-90` opens the modal, focuses the close control, restores focus, handles Escape, and keeps Tab within the dialog. `shared/customer-auth.tsx:20,60` only toggles render state and click-closes; it has no focus management, Escape handling, or dialog keyboard containment. The checkpoint's 11-test harness does not exercise this required dialog behavior. | The forgot-password interaction must preserve the reference dialog's observable keyboard behavior within the fixture boundary: opening moves focus to the close control, Escape/backdrop/close dismisses it, focus returns to the opener, and Tab does not escape the dialog. Oracle passes only when a Playwright keyboard/dialog test records each assertion and the implementation remains non-authoritative. | Only the sign-in forgot-password dialog, its direct focus/keyboard behavior, and related evidence; no real recovery or account authority is permitted. |

## Decision

`CHANGES_REQUIRED`. The functional fixture route, T02 adapter seam, security boundary, keyboard baseline, and general regression checks pass, but the frozen source-led visual and interaction obligations remain objectively unresolved. This is one bounded implementation-repair finding; no scope-change or authority decision is required.

