# M2 authoritative foundation — ticket set

- Set ID: `BAP-M2-SET-01`; revision `1.3` (T02 CK approval); status `T01 approved; T02 approved; T03 approved; T04 approved; T07 eligible within its dependencies`; selected milestone `M2`.
- Source code repository and exact contract-amendment commit: `https://github.com/adityaa11/bride-assistant.git` at `d5fbececd2dd57598ba1d5630a9f85d86a4f5990`; original implementation planning baseline: `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [Project Context v3.2](../../contexts/project.md), [Stack Context v1.3](../../contexts/stack.md), [Implementation Context v3.0](../../contexts/implementation.md), [M2](../../contexts/implementation/milestones/M2-foundation.md), [architecture](../../contexts/implementation/architecture.md), [decisions D-02/03/05/06](../../contexts/implementation/decisions.md), and [verification](../../contexts/implementation/verification.md).
- Accepted predecessor: [M1.1 no-refactor exit PASS](../../contexts/implementation/milestones/M1.1-architecture-audit-refactor.md#w02-reconciliation-and-m11-exit--23-november-2026) at the unchanged code commit, with [T01 CK PASS](../../feedback/M1.1-T01-architecture-audit-79b3478-ck.md) and [W02 disposition](../../evidence/M1.1-W02-disposition-79b3478.md). M1 fixture contracts remain review baselines, never production authority.
- Pre-write artifact inventory: untracked `checkpoints/.gitkeep`, `evidence/.gitkeep`, `feedback/.gitkeep`, `tickets/.gitkeep`; no staged or tracked changes. Reserved paths: this index and `M2-T01` through `M2-T07` below; none existed before writing. Stage only these eight paths.

## Registry and order

| Batch | Ticket / owner outcome | State | Dependency | Activation | Parallel safety |
| --- | --- | --- | --- | --- | --- |
| 1 | [M2-T01 topology and local profile](M2-T01-topology-local-profile.md) | approved | M1.1 exit | preparation-only → T03 | T07 may start after T01 CK PASS. |
| 2 | [M2-T02 exact runtime lock](M2-T02-runtime-lock.md) | approved | T01 interface evidence | active-in-this-ticket | T07 may run in parallel. |
| 3 | [M2-T03 environment and migration shell](M2-T03-data-shell.md) | approved | T01, T02 | active-in-this-ticket | T07 may run in parallel. |
| 4 | [M2-T04 persistent auth and protected surfaces](M2-T04-persistent-auth.md) | approved | T03 | active-in-this-ticket | sequence. |
| 5 | [M2-T05 authorization and input boundary](M2-T05-authorization-validation.md) | awaiting_review | T04 | active-in-this-ticket | sequence. |
| 6 | [M2-T06 repository and document contracts](M2-T06-adapter-contracts.md) | planned | T03–T05 | preparation-only → M3/M4 | sequence. |
| 2P | [M2-T07 proof-policy decision gate](M2-T07-proof-policy-gate.md) | planned | T01; decisions by accountable owner | not-applicable | parallel with T02–T06; gates M4 production hardening, not proof feature construction. |

T01–T06 are separate because they have different verification or authority boundaries: provider/account topology, dependency provenance, SQL lifecycle, identity/session, authorization/validation, and storage/repository contracts. T07 is a policy decision and expiry rehearsal, not an implementation substitute. No other M2 ticket is allocated. Each ticket has one owner and one review matrix; successors check only their own consumption of a predecessor handoff.

T03 and T04 now require a rendered, embedded ERD in their implementation checkpoints and user-facing handoffs, with fresh database comparison and CK inspection (`RC-M2-03E`, `RC-M2-04E`). The general rule for later SQL migration tickets is in the code repository `AGENTS.md` at the contract-amendment commit. This amendment changes review evidence only; it does not add schema or authorize a new migration.

## Handoffs, use paths, and activation

`M1.1 exit → T01 H-M2-01 topology/local profile → T02 H-M2-02 locked runtime → T03 H-M2-03 deployable data shell → T04 H-M2-04 session and protected route → T05 H-M2-05 actor/validated-input seam → T06 H-M2-06 repository/DocumentStore contracts → M3/M4`.

`T01 H-M2-01 → T07 H-M2-07 approved policy record → M4-W01/02/07 production hardening`. T07 does not block T02–T06. T01's provider configuration is preparation only until T03 proves a deployed shell uses it; T06's production repository and byte contracts are preparation only until named M3/M4 consumer tickets activate them. No production helper may exist solely for a unit test. The detailed `ExpectedUsePath` and required current-code proof live in each ticket. The set does not claim provider provisioning, deployed reachability, production media access, or authentication already exists.

## Reuse and boundary decisions

| ID | Decision, owner, consumers, contract boundary |
| --- | --- |
| `RD-M2-01` | `reuse-existing`: `shared/scenarios/adapter.ts` provides typed *non-authoritative* `getScenario(scenarioId)` review results to current routes. T04/T06 preserve it only for declared fixture review routes; M3/M4 replace production-facing fixture reads with authorized adapters. Never elevate fixture provenance to session authority. |
| `RD-M2-02` | `reuse-existing`: `shared/document-store.ts` owns the existing fixture byte seam and `shared/server/local-document-store.ts` implements local bytes; T06 owns the production-neutral `put(bytes, mediaType, collection)` → opaque key/hash/size/type and `read(storageKey)` contract for all upload classes. Fixture association/cleanup methods remain fixture scoped. M4-W01 consumes this contract for Neon bytes; M4-W05 owns content record association. |
| `RD-M2-03` | `keep-local`: `shared/content-image-store.ts` and its fixture server facade own content association/replace/remove, distinct from byte storage, identity, and publication. T06 preserves that separation; M4-W05/06 own durable association and publication. |
| `RD-M2-04` | `reuse-existing`: `shared/customer-home.tsx` and `shared/admin-workspace-frame.tsx` own their separate shells. T04 uses their established visual/navigation contracts while replacing auth placeholders; no cross-audience generic shell is justified. |
| `RD-M2-05` | `extract-shared`: T05 owns a server-only actor/ownership decision result and Zod parse/error convention for T06 and M3/M4 commands and reads. Named inputs: session identity, required role, optional record owner, untrusted payload/schema. Outputs: allowed actor/parsed data or typed denial/validation error; no database mutation, catalogue rule, or policy inferred from route shape. |

## Security and web guidance

Scoped `engineering-security-refactor-readiness` uses OWASP ASVS 5.0.0, OWASP Top 10:2025, NIST CSF 2.0 and NIST SP 800-53 Release 5.2.0 as lenses. `SEC-M2-01` provider/secret isolation, `SEC-M2-02` dependency integrity, `SEC-M2-03` SQL/fixture isolation, `SEC-M2-04` persistent sessions/route denial, `SEC-M2-05` actor ownership/input validation, and `SEC-M2-06` single byte boundary are `implement-now` only to the extent adopted by the cited contexts and ticket scope. `SEC-M2-07` preserves private signed-access and publication seams for M4; `SEC-M2-08` is deferred D-03 policy, resolved through T07 before production hardening. Each ticket lists its own framework mapping, review binding, and evidence. No scoped assessment returned a planning conflict for T01–T06; external account evidence remains a gate to actual completion.

T04 uses established Soft Rose Atelier visual contract and `frontend-awareness` MODE A: preserve the reference-derived sign-in/shell hierarchy, focus, responsive flow, and accessible error/loading states; take new browser captures at T04's code revision. T01–T03/T05–T07 make no independent UI design decision. Pinned `modern-web-guidance` `forms`, `autofill-sign-in-form`, and `validate-input-after-interaction` guide the T04 browser form mechanism (native form/labels, appropriate autocomplete, interaction-timed errors); they do not authorize new auth features or a visual redesign. Next.js code changes must first consult the relevant local `node_modules/next/dist/docs/` guide under `AGENTS.md`.

## Gates and next action

T01 GO is approved at source commit `123e6b49b5a0090ef8023e20b9289b2eb82cafc2`, with CK `PASS` recorded in [`M2-T01 CK`](../../feedback/M2-T01-topology-local-profile-123e6b4-ck.md); [H-M2-01 v1.0](../../evidence/m2-t01/H-M2-01-topology-v1.md) is its redacted handoff. Do not introduce real customer/proof data until the approved regional, private-bucket, credential, connection and recovery evidence is consumed within successor boundaries. T02 follows the confirmed D-02 versions and stops for a D-02 review if a security advisory or peer conflict invalidates them. T07 cannot be approved until the five-minute expiry rehearsal and accountable decisions on the five D-03 production choices are recorded; leave M4 production hardening gated meanwhile. No M2 ticket may silently choose an ORM, scanner, legal hold, recovery target, or WhatsApp number.
