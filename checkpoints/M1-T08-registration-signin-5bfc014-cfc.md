# CFC remediation — M1-T08 registration and sign-in fixtures

- Ticket: `M1-T08-registration-signin-fixtures`
- Original CK artifact: `feedback/M1-T08-registration-signin-4035d71-ck.md`
- Remediation code commit: `5bfc014b7677367df379f89bd5284d4b95e10fe5`
- Remediation scope: frozen `CK-001.a` through `CK-001.e` only.
- State: `awaiting_review`

## Working closure view

| Frozen clause | Status | Repair and evidence locator | Required command and outcome | Frozen oracle |
| --- | --- | --- | --- | --- |
| CK-001.a | PROVEN | `shared/customer-auth.tsx` adds the independent confirmation-password visibility state and labelled React control; `tests/m1-t08.visual.spec.ts` asserts its default and shown states; `evidence/M1-T08/daftar-confirmation-visible.png`. | `npx playwright test tests/m1-t08.visual.spec.ts --grep 'translates the frozen source'` — 1 passed; full 12-test suite passed. | PASS: the registration control is visible, toggles only its field, and is captured without session/storage authority. |
| CK-001.b | PROVEN | `shared/customer-auth.tsx` now renders `Sudah punya akun? Masuk di sini`; the frozen-controls assertion verifies the exact rendered copy; the refreshed `evidence/M1-T08/implementation-daftar-*.png` captures record it. | Same frozen-controls command — 1 passed; reference-pair command — 2 passed. | PASS: the mapped source copy is rendered at the switch-line location. |
| CK-001.c | PROVEN | The editorial brand is now a local Next `Link` to `/`; the frozen-controls assertion activates it and observes the local home URL. | Same frozen-controls command — 1 passed. | PASS: activation reaches the local home route and the paired capture retains the source-visible brand treatment. |
| CK-001.d | PROVEN | `BackArrow` and `ForwardArrow` inline SVG components replace text glyphs; source-derived icon sizing/placement is in `shared/customer-auth.module.css`; refreshed `implementation-{masuk,daftar}-*.png` captures cover both routes and breakpoints. | `npx playwright test tests/m1-t08.visual.spec.ts --grep 'matches the extracted reference'` — 2 passed. | PASS: mapped arrow treatment appears in the auth back and submit controls with no material paired-capture mismatch. |
| CK-001.e | PROVEN | The forgot-password fixture dialog focuses its close control on open, traps Tab, closes on Escape/backdrop/close, and restores opener focus; all state stays local and non-authoritative. `tests/m1-t08.visual.spec.ts` asserts open focus, Tab containment, Escape close, and restored focus; `evidence/M1-T08/masuk-dialog-keyboard.png`. | Same frozen-controls command — 1 passed; full 12-test suite passed. | PASS: each named keyboard/dialog observation succeeds and no recovery/session/storage behavior is introduced. |

## Expected use path and direct regressions

The repaired use path remains `customer auth route → T02 scenario adapter → AuthFixture → visible fixture UI → reviewer`. The only changed callers/effects are the registration confirmation-field control, local-home brand route, arrow artwork, and sign-in fixture dialog. No prototype script, account/session creation, storage access, external origin, or auth redirect was added.

Direct regression validation completed:

```text
npm run lint                                      # passed
npm run typecheck                                 # passed
npm test                                          # 8 passed
npx playwright test tests/m1-t08.visual.spec.ts --grep 'matches the extracted reference' # 2 passed
npx playwright test tests/m1-t08.visual.spec.ts --grep '(/masuk/(validation|loading|error|success)|/daftar/(validation|loading|error|success)|keyboard focus|translates the frozen)' # 10 passed
```

The reference-pair command refreshed the implementation captures at desktop and the 1001/1000, 701/700, and 381/380 boundaries. The second Playwright command refreshed named fixture-state and keyboard evidence, plus `daftar-confirmation-visible.png` and `masuk-dialog-keyboard.png`.

## Review contract carry-forward

The CK-recorded `PROVEN` rows RC-18, SEC-M1-01, SEC-M1-02, and VIS-02/06/08/09/10/11 were preserved. The remediation proves the previously unresolved VIS-REF-M1-T08 and SRC-REF-M1-T08 clauses only; no ticket acceptance condition changed.

Internal readiness: READY_FOR_CK
