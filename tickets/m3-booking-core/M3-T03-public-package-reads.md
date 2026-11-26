# M3-T03 — published package and eligible promotion reads

- State: awaiting_review; batch 2 parallel with T02/T04 after T01; milestone/work package: M3 / M3-W03. GO checkpoint: [M3-T03 ae60f35](../../checkpoints/M3-T03-public-package-reads-ae60f35.md).
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T01 H-M3-01.
- Successors: M3-T05 customer selection.
- Activation: `active-in-this-ticket`.
- Legacy path: forbidden for production; M1 scenarios remain declared review fixtures only.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

published package and eligible promotion reads. This ticket owns M3-W03; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp, and production activation of later M4/M5 controls unless specifically named below. It is one reviewable authority boundary; the M3 set index explains why it is separate.

## Entrypoint

Public and customer package routes call server read facades with validated query/date context; anonymous public reads receive only published/active projection. Customer session is required only for customer-owned actions, not public package facts.

## Endpoint

Typed package cards/detail and eligible package-specific offers render from PostgreSQL with no admin-only fields. Empty Services & Actions and Master Packages data produces an empty discovery state and no selectable package.

## Handoff

H-M3-03: package ID, display name/description/duration, unchanged base price, expanded composition and separately displayed eligible promotion title/type/value/inclusive dates; M3-T05 receives the package ID only and recomputes the matching promotion from its own trusted creation timestamp. The producer records exact schema/type, required fields, source identity, retry/idempotency behavior and consumer integration evidence in its GO checkpoint.

## Expected use path

`EUP-M3-T03`: Public/customer route → read facade → T01 repository published/active projection → typed view model → current package/booking selection UI; production route no longer reads scenario service/package data. GO must report the actual composition-to-effect path and CK must inspect it; a unit-only caller is insufficient.

## True path and legacy

The true path is the one above. Production fixture/scenario/localStorage paths for this capability must be removed or made unreachable at activation; declared M1 review routes may remain non-authoritative. No compatibility fallback to fixture data is authorized. Preparation-only work has no production claim until the named successor proves its handoff.

## SQL migration hygiene

No intended migration beyond confirmed T01. Disposable Docker records for visibility cases; rollback/teardown. A new SQL need reopens the ERD gate. Every SQL-dependent GO/CFC must include a rendered actual ERD, migration-file/version links and comparison against a fresh migrated database; CK inspects that evidence. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not migration authorization.

## Empty and initial state

`EMPTY-M3-T03`: Zero published packages yields no selectable package and a clear empty state; drafts and expired offers never substitute as defaults. Prove first-use and absence with isolated disposable records, never a durable test seed.

## Required invariants and non-goals

- Preserve server actor/ownership and Zod validation from H-M2-05, operation-specific repository results from H-M2-06, and the existing auth/session boundary.
- Keep package/promotion snapshots immutable, one shared booking identity, admin-only official decisions, and pending-booking non-reservation as applicable.
- No live provider probe is required unless this ticket changes a provider path or an explicit criterion requires behavior Docker cannot establish; record the reason and smallest remote check if that changes.

## Reuse and centralization

Use the set's `RD-M3-01` T05 actor/input decision and `RD-M3-02` protected repository result; preserve `RD-M3-03` distinct public/customer/admin shells. For this ticket, reuse existing contracts and keep domain-specific transaction and UI presentation local to their named owner. Do not create a generic business repository or booking/availability fallback.

## Acceptance criteria

- `AC-M3-T03-A` (Project public-content and promotion rules; M3-W03): Only active/published packages and currently eligible package-specific promotions appear; drafts and admin fields do not leak. Bound to `RC-M3-T03A`.
- `AC-M3-T03-B` (Project package/booking rule; M3-W03): Rendered base price/composition matches authoritative read, eligible promotions remain separate, selection passes only the package ID for server revalidation, and no published packages offers no invalid booking. Bound to `RC-M3-T03B`.
- `AC-M3-T03-C` (M1 UI reference; Project Soft Rose Atelier): Changed discovery/selection UI preserves visual hierarchy, responsive behavior and accessible states. Bound to `RC-M3-T03C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T03A` | Project public-content and promotion rules; M3-W03 | PASS iff Only active/published packages and currently eligible package-specific promotions appear; drafts and admin fields do not leak. | Route-level and DB integration for draft, inactive, expired, wrong-package and valid offers; inspect rendered payload. Record command, database/route observations and result in the GO checkpoint. | Fixture filter alone or client-side hiding. | Admin T02 write state. |
| `RC-M3-T03B` | Project package/booking rule; M3-W03 | PASS iff Rendered base price/composition matches authoritative read, eligible promotions remain separate, selection passes only the package ID for server revalidation, and no published packages offers no invalid booking. | Browser and server read comparison, zero-row case, reload; T05 handoff contract check. Record command, database/route observations and result in the GO checkpoint. | Trusting client price or hidden seed. | M1 public/customer visual contract. |
| `RC-M3-T03C` | M1 UI reference; Project Soft Rose Atelier | PASS iff Changed discovery/selection UI preserves visual hierarchy, responsive behavior and accessible states. | New desktop/mobile captures for changed pages and keyboard/focus/empty/error review; stable M1 baseline citation for unchanged shell. Record command, database/route observations and result in the GO checkpoint. | Build-only visual claim. | Public and customer shell. |

## Security readiness

SEC-M3-03 implement-now: published-only server projection and no admin/draft leak (Project, M3-W03; ASVS access/output, OWASP access control, NIST AC/PT). SRB-M3-03 binds A/B to negative route payload inspections. Preserve seam: private content publication remains M4. The scoped baseline is OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status: applicable; no planning-review-required finding. D-03 production proof policy is deferred to M4/M5; D-04 live WhatsApp number is deferred and disables the link, without blocking a durable booking.

## UI readiness

Frontend-awareness State 2, Mode A: preserve public editorial and customer workflow references; do not merge their shells. Inspect current touched screens and states. For changed UI, apply frontend-awareness VIS-001–014 and the M1 source/reference mapping; fresh captures are required only for touched routes and states. Preserve current CSP/browser execution boundary; no new unreviewed origin or unsafe HTML sink. For backend-only work, current M1 captures remain comparison baselines.

## Validation and hard stop

Run focused local PostgreSQL integration/route tests, direct negative/empty cases, and affected lint/typecheck/test/build checks. Check the actual use path and migration inventory. Stop on a changed business rule, unresolved schema relationship, invalid M2 handoff, or missing required ERD confirmation; return to planning rather than invent a rule.
