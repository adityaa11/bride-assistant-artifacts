# M3-T02 — authorized admin catalogue and promotion commands

- State: planned; batch 2; milestone/work package: M3 / M3-W02.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `15134132805a5dce25ebd88287f96646110530e2`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T01 H-M3-01; M2-T05 H-M2-05.
- Successors: M3-T03 and M3-T05 consume future-selection state.
- Activation: `active-in-this-ticket`.
- Legacy path: forbidden for production; M1 scenarios remain declared review fixtures only.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

authorized admin catalogue and promotion commands. This ticket owns M3-W02; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp, and production activation of later M4/M5 controls unless specifically named below. It is one reviewable authority boundary; the M3 set index explains why it is separate.

## Entrypoint

Admin catalogue/package/promotion routes and actions use T04 admin session, T05 authorizeRequest({requiredRole:'admin', payload, schema}), and T01 repositories. Inputs are untrusted form values; role, IDs and publication state are server-validated.

## Endpoint

Create/update/retire commands persist future-facing catalogue state and return typed success/validation/conflict/absence to the existing admin workspace. No write changes an existing booking snapshot.

## Handoff

H-M3-02: persisted admin-managed catalogue and promotion records, stable IDs, revisions/updated times and published/active eligibility; M3-T03 reads public projection and M3-T05 snapshots selected facts. Failed writes produce no mutation; retries must not duplicate a command. The producer records exact schema/type, required fields, source identity, retry/idempotency behavior and consumer integration evidence in its GO checkpoint.

## Expected use path

`EUP-M3-T02`: Protected admin route → T05 actor/input decision → catalogue command → T01 repository transaction → durable row/result → existing admin catalogue/packages/promotions screen; remove scenario mutation from production routes. GO must report the actual composition-to-effect path and CK must inspect it; a unit-only caller is insufficient.

## True path and legacy

The true path is the one above. Production fixture/scenario/localStorage paths for this capability must be removed or made unreachable at activation; declared M1 review routes may remain non-authoritative. No compatibility fallback to fixture data is authorized. Preparation-only work has no production claim until the named successor proves its handoff.

## SQL migration hygiene

No intended migration beyond confirmed T01 schema. Ephemeral Docker records are rolled back; if a new SQL change becomes necessary, revise the ERD proposal and obtain fresh confirmation. Every SQL-dependent GO/CFC must include a rendered actual ERD, migration-file/version links and comparison against a fresh migrated database; CK inspects that evidence. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not migration authorization.

## Empty and initial state

`EMPTY-M3-T02`: Empty catalogue shows an honest admin empty state with a create action; first create appears on reload. Missing edit target is typed NOT_FOUND, never a default record. Prove first-use and absence with isolated disposable records, never a durable test seed.

## Required invariants and non-goals

- Preserve server actor/ownership and Zod validation from H-M2-05, operation-specific repository results from H-M2-06, and the existing auth/session boundary.
- Keep package/promotion snapshots immutable, one shared booking identity, admin-only official decisions, and pending-booking non-reservation as applicable.
- No live provider probe is required unless this ticket changes a provider path or an explicit criterion requires behavior Docker cannot establish; record the reason and smallest remote check if that changes.

## Reuse and centralization

Use the set's `RD-M3-01` T05 actor/input decision and `RD-M3-02` protected repository result; preserve `RD-M3-03` distinct public/customer/admin shells. For this ticket, reuse existing contracts and keep domain-specific transaction and UI presentation local to their named owner. Do not create a generic business repository or booking/availability fallback.

## Acceptance criteria

- `AC-M3-T02-A` (Project admin authority; M2 H-M2-05): Only an authenticated admin can create, change, retire and publish typed Service/Action items, bundles, packages and promotions; malformed values and customer requests fail before writes. Bound to `RC-M3-T02A`.
- `AC-M3-T02-B` (Project historic integrity; M3-W02): Admin catalogue and promotion commands change future selections only and have no booking-snapshot write path. Bound to `RC-M3-T02B`.
- `AC-M3-T02-C` (M1 UI contract; Project visual direction): Admin UI uses durable results and handles empty/loading/error/success without fixture fallback; responsive shell and focus remain usable. Bound to `RC-M3-T02C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T02A` | Project admin authority; M2 H-M2-05 | PASS iff Only an authenticated admin can create, change, retire and publish typed Service/Action items, bundles, packages and promotions; malformed values and customer requests fail before writes. | Real protected-route/action integration with allowed admin, customer, anonymous and invalid-payload cases; DB before/after. Record command, database/route observations and result in the GO checkpoint. | Client role, route shape or fixture write as authority. | M2 auth/role route denials. |
| `RC-M3-T02B` | Project historic integrity; M3-W02 | PASS iff Admin catalogue and promotion commands change future selections only and have no booking-snapshot write path. | Inspect command dependency/call path and local DB before/after catalogue edit; T04/T05 later prove stored snapshot regression. Record command, database/route observations and result in the GO checkpoint. | Retroactive booking update or command-side history rewrite. | T01 expansion/applicability. |
| `RC-M3-T02C` | M1 UI contract; Project visual direction | PASS iff Admin UI uses durable results and handles empty/loading/error/success without fixture fallback; responsive shell and focus remain usable. | Current-revision browser captures for changed catalogue/package/promotion routes at desktop and mobile; keyboard/error state; inspect data after reload. Record command, database/route observations and result in the GO checkpoint. | Old scenario action or stale local state presented as saved. | M1 admin shell visual baseline. |

## Security readiness

SEC-M3-02 implement-now: admin role and input authorization before mutation; no draft publication through client flags alone (Project, M2 H-M2-05; ASVS access/input, OWASP broken access control, NIST AC/SI). SRB-M3-02 binds A/B to route and DB negatives. Preserve seam: M4 owns content publication, not this ticket. The scoped baseline is OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status: applicable; no planning-review-required finding. D-03 production proof policy is deferred to M4/M5; D-04 live WhatsApp number is deferred and disables the link, without blocking a durable booking.

## UI readiness

Frontend-awareness State 2, Mode A: preserve Soft Rose Atelier admin frame and reference-derived catalogue/package/promotion hierarchy; current captures for touched routes, focus, loading and failure states. No redesign. For changed UI, apply frontend-awareness VIS-001–014 and the M1 source/reference mapping; fresh captures are required only for touched routes and states. Preserve current CSP/browser execution boundary; no new unreviewed origin or unsafe HTML sink. For backend-only work, current M1 captures remain comparison baselines.

## Validation and hard stop

Run focused local PostgreSQL integration/route tests, direct negative/empty cases, and affected lint/typecheck/test/build checks. Check the actual use path and migration inventory. Stop on a changed business rule, unresolved schema relationship, invalid M2 handoff, or missing required ERD confirmation; return to planning rather than invent a rule.
