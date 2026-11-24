# PA-M2-T07-01 — approved planning amendment

- Decision: APPROVED WITH AMENDMENTS by the user as planning authority, 24 November 2026 (Asia/Bangkok).
- Code repository: https://github.com/adityaa11/bride-assistant.git; exact planning source commit: `ad10fb47284517d0a77bdc39d228764f03b4c630`.
- Proposal: [PA-M2-T07-01](../proposals/PA-M2-T07-01-production-aligned-foundation.md), submitted at artifact commit `125cf2a754e16633db019c38fecb08f188d90eab`.
- Effective authority: this decision plus the linked revised contexts and [M2-T07 revision 2.0](../../../tickets/m2-authoritative-foundation/M2-T07-proof-policy-gate.md). The proposal is historical and is superseded where this record is more specific.
- Preserved history: code commit `ad10fb4`, [five-minute rehearsal](../../../evidence/m2-t07/rehearsal-ad10fb4.md), and [blocked GO checkpoint](../../../checkpoints/M2-T07-proof-policy-gate-ad10fb4-blocked.md) remain immutable. No T07 CK result exists.

## Approved changes and limits

1. Production-aligned environment configurability is a project-wide Stack/Implementation Context principle: production requirements are authoritative; validated profiles and infrastructure adapters vary execution conditions, while domain behavior and security invariants remain shared. T07 implements only the proof-policy subset.
2. Keep four layers separate: versioned policy authority and evidence; environment configuration/adapter selection; validation against policy and trusted deployment identity; validated runtime configuration consumed by domain behavior. Domain logic receives validated values and ports, not legal documents, approval artifacts, or environment-name switches.
3. Validate both directions. Production rejects development/test durations, controlled clocks, disposable adapters, missing policy decisions, and unauthorized overrides. Development/test reject production databases, object stores, credentials, real customer data, and incompatible or unknown environment identities. Establish identity at a trusted deployment boundary; fail closed.
4. T07 owns the shared policy contract, profile validation, reusable retention and lifecycle behavior, hold/scanner outcome contracts, activation validation, and disposable verification. M4 owns live upload/scan/Neon/SQL integration; M5 owns provider recovery and release. No production UI, scanner infrastructure, Neon adapter, recovery infrastructure, or SQL migration enters T07.
5. Preserve all five D-03 decisions. Applicable Indonesian ten-year statutory retention is an authoritative minimum; eight months is only for professionally determined eligible operational proof. Unknown classification or absent approval cannot authorize production deletion. Test actual production durations/start events with a controlled clock as well as shortened disposable profiles.
6. This planning approval authorizes publication, not GO or CK. Revised implementation starts only after explicit `GO M2-T07` against revision 2.0.

## Contract and ownership mapping

| Obligation | Changed authority and owner |
| --- | --- |
| `RC-M2-07A` | Retain five-minute expiry, byte/proof-audit removal and booking-history preservation; execute the same shared behavior through disposable adapters at the new code revision. T07 owns proof. |
| `RC-M2-07B` | Retain eight-month start events and conditional rollout; add controlled-clock production-duration tests and statutory-class minimum without deciding proof applicability. T07 owns technical proof. |
| `RC-M2-07C` | Replace "all five approvals before T07 CK" with explicit unresolved decision slots and fail-closed production activation. The five approvals are **not waived**: M4-W07 owns PDP, scanner, holds and legal/accounting disposition; M5-W03 owns Neon recovery terms/ownership; M5-W05 checks all approvals before activation. |
| `RC-M2-07D` | Retain consumer map and sensitive-evidence exclusion; T07 hands off the versioned technical contract and pending decision ledger. |
| `RC-M2-07E/F/G` | Add shared profile/isolation, hold/scanner outcome, and activation-gate proof as specified in revised T07. |

T07 depends on approved T01 topology and T06 byte-contract interfaces. Its technical CK approval makes `H-M2-07-TECH` consumable by M4-W01/W02 and does not approve production policy or activation. M4-W07 and M5-W03 own their distinct production evidence; M5-W05 alone decides activation. The dependency graph is acyclic: T01/T06 → T07 technical foundation → M4-W01/W02/W07 → M5-W03 → M5-W05. M4 construction may proceed before final policy approval, but proof activation cannot.

## SQL boundary

T07 introduces no schema, migration, SQL data change or migration-dependent behavior; its local metadata and byte fixtures are disposable. M4-W01 owns any future proof/hold/audit tables and SQL migrations. Before an M4 migration is written or run, the user must see and explicitly confirm the rendered proposed ERD and migration definition/purpose/responsibilities under `AGENTS.md`. M4 GO/CK must compare a committed rendered actual ERD with a fresh locally migrated Docker database. T07 approval does not satisfy that later confirmation.

## Publication record

This decision amends the Stack Context principle; Implementation Context, architecture, M2/M4/M5 milestones and verification; D-03 and the payment-proof policy template; M2-T07 and its ticket-set index. It does not reopen T01–T06 or authorize implementation. The existing blocked checkpoint remains a historical result against ticket revision 1.0. Revision 2.0 returns T07 to `planned` and eligible for an explicit GO; CK must review a new current-code checkpoint against all effective rows.
