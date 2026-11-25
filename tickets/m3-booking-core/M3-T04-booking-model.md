# M3-T04 — booking aggregate, immutable snapshots and history

- State: planned; batch 2 parallel with T02/T03 after T01; milestone/work package: M3 / M3-W04.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T01 H-M3-01; M2-T05 H-M2-05 and M2-T06 H-M2-06.
- Successors: M3-T05 and M3-T06.
- Activation: `preparation-only`.
- Legacy path: forbidden for production; M1 scenarios remain declared review fixtures only.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

booking aggregate, immutable snapshots and history. This ticket owns M3-W04; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp, and production activation of later M4/M5 controls unless specifically named below. It is one reviewable authority boundary; the M3 set index explains why it is separate.

## Entrypoint

M3-T05/T06 later invoke a booking repository with T05 authorized actor, validated booking/status input and trusted record identity. The migration runner applies booking schema after T01 on disposable PostgreSQL.

## Endpoint

One booking ID owns customer ID, its trusted created_at timestamp, submitted package base-price snapshot, automatically applied promotion snapshot when one matches, actual final_price and append-only status history. New record starts Menunggu Konfirmasi; malformed/absent references return typed conflict/absence without partial state.

## Handoff

H-M3-04: booking ID, customer ID FK, package/promotion stable IDs, immutable submitted name/price/duration/description/composition and final-price snapshots, wedding date/location, canonical status, ordered history entries with actor/time; T05 owns submission/read and T06 owns admin status transition. Snapshot contents are copied into the aggregate, not live joins. The producer records exact schema/type, required fields, source identity, retry/idempotency behavior and consumer integration evidence in its GO checkpoint.

## Expected use path

`EUP-M3-T04`: T05/T06 server caller → T05 actor/validated input → booking repository transaction → PostgreSQL aggregate/history → customer/admin consumer; this ticket proves repository and immutability, successors prove routes. GO must report the actual composition-to-effect path and CK must inspect it; a unit-only caller is insufficient.

## True path and legacy

The true path is the one above. Production fixture/scenario/localStorage paths for this capability must be removed or made unreachable at activation; declared M1 review routes may remain non-authoritative. No compatibility fallback to fixture data is authorized. Preparation-only work has no production claim until the named successor proves its handoff.

## SQL migration hygiene

M3-SQL-04 intended migration: booking/status-history schema in [migration proposal](migration-proposal.md). No live prototype import or seed; local factory creates disposable rows in Docker. Confirm ERD before migration-dependent work. Every SQL-dependent GO/CFC must include a rendered actual ERD, migration-file/version links and comparison against a fresh migrated database; CK inspects that evidence. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not migration authorization.

## Empty and initial state

`EMPTY-M3-T04`: New database returns empty booking list; missing booking ID gives NOT_FOUND. First valid submission creates one aggregate and initial history event atomically. Prove first-use and absence with isolated disposable records, never a durable test seed.

## Required invariants and non-goals

- Preserve server actor/ownership and Zod validation from H-M2-05, operation-specific repository results from H-M2-06, and the existing auth/session boundary.
- Keep package/promotion snapshots immutable, one shared booking identity, admin-only official decisions, and pending-booking non-reservation as applicable.
- No live provider probe is required unless this ticket changes a provider path or an explicit criterion requires behavior Docker cannot establish; record the reason and smallest remote check if that changes.

## Reuse and centralization

Use the set's `RD-M3-01` T05 actor/input decision and `RD-M3-02` protected repository result; preserve `RD-M3-03` distinct public/customer/admin shells. For this ticket, reuse existing contracts and keep domain-specific transaction and UI presentation local to their named owner. Do not create a generic business repository or booking/availability fallback.

## Acceptance criteria

- `AC-M3-T04-A` (Project snapshot/shared-record rule; M3-W04): One durable booking contains immutable package base-price facts, the automatically applied promotion snapshot or null, the actual final_price and one cross-surface identity; later package/promotion edits do not alter them. Bound to `RC-M3-T04A`.
- `AC-M3-T04-B` (Project lifecycle; M3-W04): Initial status/history is canonical, ordered and append-only; invalid partial create rolls back. Bound to `RC-M3-T04B`.
- `AC-M3-T04-C` (AGENTS.md; M2-T03): Empty/missing cases and actual schema match the confirmed ERD; no test seed enters migration graph. Bound to `RC-M3-T04C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T04A` | Project snapshot/shared-record rule; M3-W04 | PASS iff One durable booking contains immutable package base-price facts, the automatically applied promotion snapshot or null, the actual final_price and one cross-surface identity; later package/promotion edits do not alter them. | Fresh DB create/read with and without an applicable promotion, inspect base price versus final_price, then edit package/promotion and compare snapshots/FKs. Record command, database/route observations and result in the GO checkpoint. | Live-join-only historical values. | T01 service/package identities. |
| `RC-M3-T04B` | Project lifecycle; M3-W04 | PASS iff Initial status/history is canonical, ordered and append-only; invalid partial create rolls back. | Transactional create failure/retry and history ordering tests; inspect persisted rows. Record command, database/route observations and result in the GO checkpoint. | Separate customer/admin records or mutable history. | M2 repository error semantics. |
| `RC-M3-T04C` | AGENTS.md; M2-T03 | PASS iff Empty/missing cases and actual schema match the confirmed ERD; no test seed enters migration graph. | Docker migrate/inspect, rendered actual ERD in GO/CK, migration inventory and empty query. Record command, database/route observations and result in the GO checkpoint. | Unconfirmed migration or Mermaid-only ERD. | Auth/ledger schema. |

## Security readiness

SEC-M3-04 implement-now: trusted customer FK, immutable snapshot and append-only status facts with transactional integrity (Project/M3-W04; ASVS business logic/data, OWASP integrity, NIST SI/AU). SRB-M3-04 binds A–C to fresh DB and failure tests. Preserve seam: M4 proof/exception records attach by booking ID without changing snapshot ownership. The scoped baseline is OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status: applicable; no planning-review-required finding. D-03 production proof policy is deferred to M4/M5; D-04 live WhatsApp number is deferred and disables the link, without blocking a durable booking.

## UI readiness

Not applicable: aggregate/repository preparation only. For changed UI, apply frontend-awareness VIS-001–014 and the M1 source/reference mapping; fresh captures are required only for touched routes and states. Preserve current CSP/browser execution boundary; no new unreviewed origin or unsafe HTML sink. For backend-only work, current M1 captures remain comparison baselines.

## Validation and hard stop

Run focused local PostgreSQL integration/route tests, direct negative/empty cases, and affected lint/typecheck/test/build checks. Check the actual use path and migration inventory. Stop on a changed business rule, unresolved schema relationship, invalid M2 handoff, or missing required ERD confirmation; return to planning rather than invent a rule.
