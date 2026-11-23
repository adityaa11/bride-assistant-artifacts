# M2-T04 — persistent separate customer and admin authentication

- State: `planned`; batch 4; work package `M2-W02`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [Project access](../../contexts/project.md), [Stack auth choice](../../contexts/stack.md), [M2-W02](../../contexts/implementation/milestones/M2-foundation.md), [D-02](../../contexts/implementation/decisions.md), and code-repository `docs/frontend/PROJECT_FRONTEND_CONTRACT.md`. Dependency: T03 `H-M2-03`. Successors: T05 and M3-W05/06 via `H-M2-04`.
- Activation: `active-in-this-ticket`. Legacy: forbidden. SQL: intended Better Auth identity/session migrations; test identities only in disposable database/transaction. Empty state: first customer can register; no admin is implicitly created and protected admin access fails closed until an explicitly provisioned admin exists.

## Outcome and exact scope

Integrate Better Auth with persistent PostgreSQL backing, separate customer and admin sign-in destinations and session-aware route protection. Replace M1 customer/admin auth placeholders in the active flows while retaining source-derived visual structure and explicit fixture review routes where still named. Decide the concrete Better Auth database adapter within accepted `pg`/PostgreSQL integration evidence; a boundary conflict triggers planning. Excludes domain record ownership, catalogue/booking logic, Magic Link/OAuth/Expo/SSO, and an invented admin self-registration path.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: customer registration/sign-in forms, admin sign-in form and protected route request; owner is auth server composition. Inputs: email/password form values, existing cookie/session and requested route from browser (untrusted); Better Auth and server checks own validation. Invalid credentials/session return nonrevealing error or redirect; never accept a role from form/query/browser storage.
- Endpoint: persisted account/session and role-appropriate protected page or denial. Customer and admin sessions remain separate in authorization outcome, even if the same underlying library stores sessions.
- `H-M2-04`: server session-to-actor contract `{subjectId, role, sessionId/expiry}` or anonymous/invalid result; T05 consumes it. Role is server-derived, never browser-asserted. Session renewal/expiry follows Better Auth's verified behavior; no silent fixture fallback.
- `EUP-M2-04`: browser form → Better Auth handler → PostgreSQL identity/session → server protected-route check → customer/admin workspace or denial. Test after process restart and session expiry; a component-only simulation is insufficient.

## True path, UI and invariants

Real auth routes use Better Auth persisted sessions; remove reachable prototype/localStorage credentials or placeholder sign-in success paths. Preserve separate customer/admin route surfaces, Soft Rose Atelier reference hierarchy, shell components, keyboard/focus and responsive form behavior. `frontend-awareness` established-language MODE A binds the existing visual contract and fresh desktop/mobile/error/loading captures. Pinned modern-web `forms`, `autofill-sign-in-form`, `validate-input-after-interaction` guide native form labels, email/password autocomplete, and interaction-timed errors; no passkey/OTP requirement follows from them. Follow local Next 16 docs before code changes. Preserve CSP-safe framework rendering.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-04A` | A new customer registers and signs in, session persists across request/process restart, and expiry/sign-out denies protected access. | Browser-to-server integration with disposable PostgreSQL, cookie/session observations, route status/redirect. | Component state or in-memory auth alone; anonymous access. |
| `RC-M2-04B` | Customer cannot enter admin protected routes, admin cannot be impersonated with a client role value, and a fresh empty DB grants no admin. | Direct route requests with anonymous/customer/admin sessions and forged role/cookie scenarios. | Hiding admin links in UI; role-confusion regression. |
| `RC-M2-04C` | Active sign-in/registration routes no longer use prototype/localStorage success or seed credentials, while review fixture routes remain visibly non-authoritative. | Actual route/call graph, browser sign-in and negative placeholder probes. | Side-by-side fallback; M1 fixture route or visual regression. |
| `RC-M2-04D` | Customer/admin forms keep reference-derived hierarchy and accessible labels, autofill, focus/error/loading and narrow-width operation. | Fresh current-revision browser captures at desktop/mobile and keyboard/form-state observations against committed M1 reference baseline. | Build pass or old implementation capture alone; visual/accessibility regression. |

## Security readiness and validation

- `SEC-M2-04` `implement-now`: persistent server-derived identity, session denial and separated role entry; Project/Stack, ASVS authentication/session/authorization, OWASP broken access control, NIST CSF Protect and SP 800-53 IA/AC. `SRB-M2-04` binds `RC-M2-04A/B/C` to real route and DB evidence.
- `SEC-M2-04P` `preserve-seam`: record owner checks belong to T05; no browser role or storage key confers access. Keep auth error details nonrevealing. `SRB-M2-04P` inspects T05 handoff.
- Validation: disposable identity data, exact migration inventory and current-revision visual/route evidence. Hard stop: no supported Better Auth adapter/integration or unresolved admin provisioning authority means planning review, not a self-serve admin bypass.
