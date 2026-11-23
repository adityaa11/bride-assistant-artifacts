# M2-T07 — proof retention rehearsal and production policy decision

- State: `planned`; batch `2P`; work package `M2-R03`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [Project U4](../../contexts/project.md), [M2-R03](../../contexts/implementation/milestones/M2-foundation.md), [D-03](../../contexts/implementation/decisions.md), [proof policy template](../../contexts/implementation/payment-proof-policy-template.md). Dependency: T01 `H-M2-01`; parallel with T02–T06. Successors: M4-W01/02/07 production hardening and M5-W03 via `H-M2-07`.
- Activation: `not-applicable`; this is a bounded rehearsal/decision record, no production proof runtime. Legacy and SQL: not applicable; rehearsal data is disposable and never enters migration graph. Empty state: no proof exists initially; expiry run creates disposable proof bytes/audit metadata and returns to empty without deleting booking history.

## Outcome and exact scope

Run the D-03 five-minute expiry/deletion/history rehearsal, then obtain the accountable decisions needed for production proof/media controls: PDP processing basis, final notice/contact, scanner provider/terms, legal/business holds, Neon account RPO/RTO/backup/restore ownership, and accountant/legal determination of any ten-year Indonesian retention requirement. Record the adopted policy/control values and evidence, or leave an exact unresolved finding. The accepted JPEG/PNG/PDF, 10 MiB, three-file, owner-only upload, admin review, private access, append-only audit and eight-month post-review/unreviewed retention design remain the starting authority. Excludes implementing M4 proof UI/adapter and guessing legal or provider choices.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: T01 account-backed recovery facts plus D-03 policy template and disposable rehearsal harness; caller is planning/policy owner. Inputs are approved retention rule, proof/audit test objects, provider/decision-owner evidence. Missing decision authority yields an unresolved record, not a default.
- Endpoint: versioned five-minute expiry observations and signed/approved D-03 production-hardening decision record, or explicit `not-ready` with each unresolved item. This does not activate user upload.
- `H-M2-07`: approved machine-readable/human-readable policy values, decision owner/date, retention origin and deletion/hold rules, scanner/recovery/contact/legal disposition and test evidence locators. M4-W01/02/07 consume only their relevant values; M5-W03 consumes recovery ownership. No proof bytes or secrets in the record.
- `EUP-M2-07`: disposable proof upload/audit fixture → five-minute clock/expiry process → byte and proof-specific metadata removal with booking history preserved → policy decision → M4 production hardening. This is a planning/rehearsal path; M4 must prove the later live integration.

## Invariants and planning limits

The user-approved eight-month rule follows successful five-minute verification; it is not waived because unresolved production choices remain. A hold may alter deletion only after a specific approved criterion. Private bytes and audit records remain out of public URLs and artifacts. No scanner, RPO/RTO or legal conclusion is selected by this ticket author. T07's approval is a hard gate for production proof hardening/release, while D-03 explicitly allows constructing the proof capability before final values are bound.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-07A` | Five-minute rehearsal deletes proof bytes and proof-specific metadata/audit at expiry, handles reviewed and unreviewed origins, and preserves booking status history. | Disposable clock-controlled integration with actual local DocumentStore/metadata stand-ins; before/after key, audit and booking-history observations. | Unit-only timer arithmetic or durable test seed; orphan bytes/audit. |
| `RC-M2-07B` | The eight-month rollout decision records its start event and depends on `RC-M2-07A` passing; no silent alternate retention is active. | Policy ledger and expiry-result cross-reference. | Treating five-minute run as production retention; retention drift. |
| `RC-M2-07C` | Each of the five deferred D-03 choices has an accountable approved value/owner and evidence, including legal/accounting disposition and Neon recovery terms, before `H-M2-07` is marked approved. | Decision-owner record, provider/account terms and professional determination references. | Framework inference, default scanner/hold/RPO, or unsupported legal claim. |
| `RC-M2-07D` | Handoff names the exact M4/M5 consumer and no raw proof bytes, names, signed URLs or secrets are stored in evidence. | Artifact inspection and consumer map. | Redacted label over recoverable secret; privacy regression. |

## Security readiness and validation

- `SEC-M2-08A` `implement-now`: D-03's already approved five-minute expiry and proof-audit separation; ASVS data-protection/logging/business logic, OWASP logging/integrity, NIST CSF Protect/Recover and SP 800-53 AU/PT/CP. `SRB-M2-08A` binds `RC-M2-07A/B/D` to disposable integration evidence.
- `SEC-M2-08B` `deferred-policy`: processing basis/contact, scanner, holds, recovery targets and ten-year treatment need accountable decisions. `SRB-M2-08B` binds `RC-M2-07C` to owner/provider/legal records. This is the ticket's intended decision gate, not authority for implementers to guess controls.
- Validation: new five-minute rehearsal evidence at the named revision and account-backed policy record. Hard stop: if any D-03 choice is absent, mark T07 `blocked`/set gate unsatisfied and keep M4 production hardening disabled; do not mark M2's technical T01–T06 path failed solely for this parallel gate.
