# M3-T01 — service, action, bundle, package and promotion persistence

- State: planned; batch 1; milestone/work package: M3 / M3-W01.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `15134132805a5dce25ebd88287f96646110530e2`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M2-T03 H-M2-03 and M2-T06 H-M2-06.
- Successors: M3-T02, M3-T03 and M3-T04.
- Activation: `preparation-only`.
- Legacy path: forbidden for production; M1 scenarios remain declared review fixtures only.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

service, action, bundle, package and promotion persistence. This ticket owns M3-W01; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp, and production activation of later M4/M5 controls unless specifically named below. It is one reviewable authority boundary; the M3 set index explains why it is separate.

## Entrypoint

The M3-T02/T03 server composition later supplies a T05 authorized actor and validated service/action, bundle, package or promotion input to operation-specific PostgreSQL repositories; the migration runner applies proposed service/action, bundle and package schema to a fresh disposable database. Invalid IDs, relationships or prices yield typed invalid/conflict results.

## Endpoint

Durable service/action, bundle, package and promotion tables and repository read/write contracts for action, bundle, package, price and promotion applicability. Empty reads return empty collections; missing IDs return NOT_FOUND. No production Services & Actions / Master Packages UI is activated here.

## Handoff

H-M3-01: action/bundle/package/promotion IDs, published/active flags, price and duration, effective promotion window and package applicability; PostgreSQL FKs and unique relationships preserve identity; M3-T02 owns admin writes, M3-T03 owns public reads, M3-T04 uses stable package/promotion identity for snapshots. The producer records exact schema/type, required fields, source identity, retry/idempotency behavior and consumer integration evidence in its GO checkpoint.

## Expected use path

`EUP-M3-T01`: M3-T02/T03 server caller → T05 authorization/validation → M3-T01 service/package repository → PostgreSQL result → admin/public consumer; this ticket proves repository behavior locally and successor tickets prove production composition. GO must report the actual composition-to-effect path and CK must inspect it; a unit-only caller is insufficient.

## True path and legacy

The true path is the one above. Production fixture/scenario/localStorage paths for this capability must be removed or made unreachable at activation; declared M1 review routes may remain non-authoritative. No compatibility fallback to fixture data is authorized. Preparation-only work has no production claim until the named successor proves its handoff.

## SQL migration hygiene

M3-SQL-01 intended migration: proposed service/action, bundle, package and promotion tables and joins in [migration proposal](migration-proposal.md); no production seed. Tests create records inside disposable Docker database/transactions and remove the volume or roll back. ERD confirmation is required before migration or migration-dependent implementation. Every SQL-dependent GO/CFC must include a rendered actual ERD, migration-file/version links and comparison against a fresh migrated database; CK inspects that evidence. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not migration authorization.

## Empty and initial state

`EMPTY-M3-T01`: New database has zero service/action, bundle, package or promotion rows; no sample packages or promotions appear. First authorized admin create yields an ID and makes only explicitly published/active records eligible to M3-T03. Prove first-use and absence with isolated disposable records, never a durable test seed.

## Required invariants and non-goals

- Preserve server actor/ownership and Zod validation from H-M2-05, operation-specific repository results from H-M2-06, and the existing auth/session boundary.
- Keep package/promotion snapshots immutable, one shared booking identity, admin-only official decisions, and pending-booking non-reservation as applicable.
- No live provider probe is required unless this ticket changes a provider path or an explicit criterion requires behavior Docker cannot establish; record the reason and smallest remote check if that changes.

## Reuse and centralization

Use the set's `RD-M3-01` T05 actor/input decision and `RD-M3-02` protected repository result; preserve `RD-M3-03` distinct public/customer/admin shells. For this ticket, reuse existing contracts and keep domain-specific transaction and UI presentation local to their named owner. Do not create a generic business repository or booking/availability fallback.

## Acceptance criteria

- `AC-M3-T01-A` (Project Service/Action and Package invariant; M3-W01): Service and Action share one `catalogue` table with required PostgreSQL `public.catalogue_type` enum; bundle/package relationships expand without double-counting an item; invalid types, references and prices are rejected. Bound to `RC-M3-T01A`.
- `AC-M3-T01-B` (Project promotion rule; M3-W01): Promotion type/value/window and package applicability persist with valid constraints; no draft or inactive offer is implicitly eligible. Bound to `RC-M3-T01B`.
- `AC-M3-T01-C` (AGENTS.md SQL contract; M2-T03): Fresh/empty migration and repository behavior match the proposed rendered ERD; actual GO ERD is checked against a freshly migrated database. Bound to `RC-M3-T01C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T01A` | Project Service/Action and Package invariant; M3-W01 | PASS iff Service and Action share one `catalogue` table with required PostgreSQL `public.catalogue_type` enum; bundle/package relationships expand without double-counting an item; invalid types, references and prices are rejected. | Fresh migrated database, `public.catalogue_type` introspection, Service/Action type filter, duplicate-expansion and invalid-type/FK/price tests; inspect rows and computed result. Record command, database/route observations and result in the GO checkpoint. | Fixture seed or application-only uniqueness. | M1 Services & Actions fixture remains review-only. |
| `RC-M3-T01B` | Project promotion rule; M3-W01 | PASS iff Promotion type/value/window and package applicability persist with valid constraints; no draft or inactive offer is implicitly eligible. | Local repository integration for percentage/rupiah, package join and time bounds. Record command, database/route observations and result in the GO checkpoint. | Global promotion without applicability or test-only seed migration. | M1 promotion fixture behavior remains isolated. |
| `RC-M3-T01C` | AGENTS.md SQL contract; M2-T03 | PASS iff Fresh/empty migration and repository behavior match the proposed rendered ERD; actual GO ERD is checked against a freshly migrated database. | Docker migrate/inspect, schema diff, rendered actual SVG/PNG in checkpoint, migration inventory and empty read. Record command, database/route observations and result in the GO checkpoint. | Mermaid source or file path alone; unconfirmed migration. | Existing auth schema and migration ledger. |

## Security readiness

SEC-M3-01 implement-now: PostgreSQL constraints, parameterized queries and T05 actor/validated-input boundary (Project/Stack, M2 H-M2-05/06; ASVS business logic/input, OWASP injection, NIST SI/AC). SRB-M3-01 binds A–C to local DB integration and unauthorized-input negatives. Preserve seam: no fixture fallback or provider-specific data policy. The scoped baseline is OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status: applicable; no planning-review-required finding. D-03 production proof policy is deferred to M4/M5; D-04 live WhatsApp number is deferred and disables the link, without blocking a durable booking.

## UI readiness

Not applicable: schema/repository preparation only. For changed UI, apply frontend-awareness VIS-001–014 and the M1 source/reference mapping; fresh captures are required only for touched routes and states. Preserve current CSP/browser execution boundary; no new unreviewed origin or unsafe HTML sink. For backend-only work, current M1 captures remain comparison baselines.

## Validation and hard stop

Run focused local PostgreSQL integration/route tests, direct negative/empty cases, and affected lint/typecheck/test/build checks. Check the actual use path and migration inventory. Stop on a changed business rule, unresolved schema relationship, invalid M2 handoff, or missing required ERD confirmation; return to planning rather than invent a rule.
