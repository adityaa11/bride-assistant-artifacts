# M3-T06 — admin queue and core status decisions

- State: planned; batch 3 parallel with T05 after T04; milestone/work package M3 / M3-W06.
- Code repository / exact source commit: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: [Project Context](../../contexts/project.md), [Stack Context](../../contexts/stack.md), [Implementation Context](../../contexts/implementation.md), [M3 milestone](../../contexts/implementation/milestones/M3-booking-core.md), [architecture](../../contexts/implementation/architecture.md), [decisions](../../contexts/implementation/decisions.md), [verification](../../contexts/implementation/verification.md), and [M2 exit](../../contexts/implementation/milestones/M2-foundation.md#m2-exit--24-november-2026).
- Local verification: disposable Docker PostgreSQL 18.7 and isolated rollback/factory data; local `DocumentStore` only if bytes are involved. Reuse M2-T01/T03 provider evidence unless this ticket changes a provider path or explicitly requires a remote behavior check.
- Dependencies and accepted outputs: M3-T04 H-M3-04; M2-T05 H-M2-05.
- Successors: M3-T07; M4 proof/exception consumers.
- Activation: `active-in-this-ticket`; legacy production fixture paths forbidden.
- SQL classification: intended-migration + ephemeral-test-setup.

## Outcome and exact scope

admin queue and core status decisions. Owns M3-W06; excludes proof upload/review, cancellation/reschedule, full publishing, automated WhatsApp and later M4/M5 activation. This is one reviewable authority boundary.

## Entrypoint

Protected admin booking queue/detail and status action call T05 with admin actor, trusted booking ID and validated target core status. Current status is fetched in the transaction; request-supplied current state is advisory only.

## Endpoint

Admin sees the same booking ID/snapshot/history; allowed core transitions persist status plus one ordered history event atomically. Invalid, stale, missing and unauthorized transitions return typed result without mutation.

## Handoff

H-M3-06: booking ID, canonical current status, actor/time and ordered transition event; M3-T07 enforces successful-date conflict in the same transaction. M4 owns proof review and cancellation/reschedule exceptions, not this ticket. GO records the exact schema/type, required fields, identity/correlation, retry/idempotency and consumer integration evidence.

## Expected use path

`EUP-M3-T06`: Protected admin route → T05 actor/input → T04 booking repository/current-state transaction → status/history result → existing admin queue/detail; remove production fixture status mutations and noncanonical labels. GO reports the actual composition-to-effect path; CK inspects it. Unit-only callers are insufficient.

## True path and legacy

The path above is authoritative. Remove or make unreachable production scenario/localStorage behavior for this capability. Declared M1 review routes remain non-authoritative; no fixture fallback is allowed.

## SQL migration hygiene

No intended migration beyond confirmed T04. Docker rows are disposable; new SQL requires revised ERD confirmation. SQL-dependent GO/CFC must commit a rendered actual ERD, link migration files/versions and compare against a fresh migrated database; CK inspects this. The [planning proposal](migration-proposal.md) and [rendered ERD](proposed-erd.svg) are for review, not authorization.

## Empty and initial state

`EMPTY-M3-T06`: Empty queue renders a genuine no-record state; absent booking returns NOT_FOUND. First customer submission from T05 appears by shared ID, without admin seed data. Use disposable records and teardown, never a durable seed.

## Required invariants and non-goals

Preserve H-M2-05 actor/ownership/Zod validation, H-M2-06 repository errors, one shared booking ID, immutable snapshots, admin-only official decisions, and pending-booking non-reservation as applicable. Use local Docker verification and existing M2 provider baseline; add a remote check only for a changed provider path or explicit provider-specific criterion, recording its reason and minimum scope.

## Reuse and centralization

Apply set decisions `RD-M3-01` T05 actor/input, `RD-M3-02` protected repository result, and `RD-M3-03` separate shells. Domain transactions and audience-specific presentation stay with their ticket owner. No generic business repository or duplicate availability decision.

## Acceptance criteria

- `AC-M3-T06-A` (Project canonical flow/admin authority; M3-W06): Only admin may apply valid Menunggu Konfirmasi → Menunggu Pembayaran → Booking Berhasil core transitions; stale/invalid/customer actions leave status/history unchanged. Review row `RC-M3-T06A`.
- `AC-M3-T06-B` (Project shared-record rule; M3-W06): Admin list/detail shows the same persisted booking and snapshot as customer; no duplicate aggregate or leaked fields. Review row `RC-M3-T06B`.
- `AC-M3-T06-C` (M1 admin reference; M3-W06): Changed admin queue/detail preserves operational hierarchy, responsive fit, keyboard and state feedback. Review row `RC-M3-T06C`.

## Review Contract

| Row | Authority | Required behavior | Harness, observations and evidence | Forbidden substitute | Direct regression |
| --- | --- | --- | --- | --- | --- |
| `RC-M3-T06A` | Project canonical flow/admin authority; M3-W06 | PASS iff Only admin may apply valid Menunggu Konfirmasi → Menunggu Pembayaran → Booking Berhasil core transitions; stale/invalid/customer actions leave status/history unchanged. | Direct route + DB tests for valid/invalid/concurrent stale commands, actor/time/history ordering. GO records route/DB observations; CK independently checks them. | Client state machine or fixture status update. | M2 authorization and T04 history. |
| `RC-M3-T06B` | Project shared-record rule; M3-W06 | PASS iff Admin list/detail shows the same persisted booking and snapshot as customer; no duplicate aggregate or leaked fields. | T05 submit then admin read and customer read by same ID; compare facts; empty queue. GO records route/DB observations; CK independently checks them. | Separate admin copy or scenario row. | T05 own-record flow. |
| `RC-M3-T06C` | M1 admin reference; M3-W06 | PASS iff Changed admin queue/detail preserves operational hierarchy, responsive fit, keyboard and state feedback. | Fresh desktop/mobile captures for changed routes, focus/error/loading/empty review. GO records route/DB observations; CK independently checks them. | Build-only UI claim. | Admin frame and M1 booking baseline. |

## Security readiness

SEC-M3-06 implement-now: admin-only transitions, validated target, transactional current-state check and audit history (Project/M2 H-M2-05; ASVS access/business logic, OWASP access control, NIST AC/AU/SI). SRB-M3-06 binds A/B to adversarial/concurrent DB evidence. Preserve seam: exception transitions remain M4. Scoped baseline: OWASP ASVS 5.0.0 / Top 10:2025 and NIST CSF 2.0 / SP 800-53 Release 5.2.0. Status applicable; no planning-review-required finding. D-03 production proof approvals remain M4/M5 gates; D-04 keeps the live WhatsApp link disabled until configured.

## UI readiness

Frontend-awareness State 2, Mode A: keep M1 admin booking workspace and shared frame; use existing canonical status labels and current rendered review. For changed UI, apply frontend-awareness VIS-001–014 and M1 reference mapping; capture only touched routes/states at the code revision. Preserve CSP/browser execution boundary and avoid new unsafe HTML sinks or unreviewed origins. Backend-only work may cite unchanged M1 captures.

## Validation and hard stop

Run focused disposable PostgreSQL integration/route tests, direct negative/empty cases, affected lint/typecheck/test/build checks, use-path inspection and migration inventory. Stop for a changed business rule, unresolved schema relationship, invalid M2 handoff or missing ERD confirmation.
