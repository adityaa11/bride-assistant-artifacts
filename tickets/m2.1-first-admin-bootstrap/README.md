# M2.1 first-admin bootstrap — ticket set

- Set ID: `BAP-M2.1-SET-01`; revision `1.1`; status `one committed ticket approved by CK`; milestone `M2.1`, work package `M2.1-W01`.
- Code repository / exact review target: `https://github.com/adityaa11/bride-assistant.git` / `fcaae2455308f10f2e46a0092fa097135b56994d`.
- Authority: user request for a proper first-admin registration CLI and explicit 0003 migration confirmation; [Project](../../contexts/project.md) separate admin access, [Stack](../../contexts/stack.md) Better Auth/PostgreSQL, [Implementation Context](../../contexts/implementation.md), [M2.1](../../contexts/implementation/milestones/M2.1-first-admin-bootstrap.md), and accepted M2-T03/T04/T05 migration, identity and authorization handoffs.
- Provenance: the code commit and [original checkpoint](../../checkpoints/admin-bootstrap-fcaae24.md) preceded this ticket. This retrospective review contract was requested on 25 November 2026. It does not claim a pre-implementation ticket freeze; independent CK later recorded `PASS` for M2.1-T01.
- Pre-write inventory: untracked `checkpoints/.gitkeep`, `evidence/.gitkeep`, `feedback/.gitkeep`, `tickets/.gitkeep`; staged index empty; no tracked modifications. Reserved new paths: this README, the ticket, and a checkpoint addendum. Historical evidence and checkpoints stay unchanged.

## Registry and dependency

| Batch | Ticket | State | Dependency | Activation |
| --- | --- | --- | --- | --- |
| 1 | [M2.1-T01 first-admin bootstrap](M2.1-T01-first-admin-bootstrap.md) | approved | M2-T03/T04/T05 approved; user-confirmed 0003 proposal | active-in-this-ticket |

One ticket owns the single operational outcome: a trusted operator creates exactly one first administrator who can sign in to the existing admin workspace. The migration marker, CLI coordinator, Better Auth credential, and sign-in proof form one atomic review boundary. M2-T04 remains approved for ordinary persistent auth; this ticket owns only the new bootstrap capability. M3-T02 and M3-T06 consume the approved admin entry handoff after M2.1 CK PASS.

## Entrypoint, endpoint, handoff and reuse

`trusted interactive terminal → npm run admin -- bootstrap → scripts/admin-command.mjs → scripts/admin-bootstrap-core.mjs → app.admin_bootstrap_state + Better Auth auth.user/auth.account → /admin/masuk → /admin`. A reserved failed attempt may use `npm run admin -- reconcile --attempt-id ...`; it may only verify and seal that attempt. `H-M2.1-01` is the sealed marker plus a Better Auth admin credential and working admin sign-in; M3-T02 and M3-T06 consume the protected admin session, not a bootstrap bypass.

`RD-M2.1-01 reuse-existing`: Better Auth sign-up/credential verification and M2 server database/session conventions remain the identity and access path. The bootstrap coordinator owns only the first-admin reservation, promotion of the newly created ID, and sealing. `RD-M2.1-02 keep-local`: the CLI's hidden interactive password prompt has a different trust boundary from customer registration and admin sign-in; no shared browser registration form or public endpoint is introduced.

SecurityReadiness `applicable`: `SEC-M2.1-01` first-admin exclusivity and fail-closed reservation, `SEC-M2.1-02` credential secrecy and Better Auth ownership, `SEC-M2.1-03` server-derived role and protected sign-in. These bind the ticket's RC rows. OWASP ASVS 5.0.0 authentication/access and Top 10:2025 access-control topics, NIST CSF 2.0 Protect, and NIST SP 800-53 Release 5.2.0 AC/IA/CM are scoped lenses, not extra product requirements. UI design is unchanged; `/admin/masuk` is an existing M2-T04 surface, so this ticket verifies its function rather than revising its design.

## Review target and next action

The [checkpoint addendum](../../checkpoints/M2.1-T01-first-admin-bootstrap-fcaae24.md) binds the frozen ticket to code commit `fcaae24` and immutable original evidence. The [CK review](../../feedback/M2.1-T01-first-admin-bootstrap-fcaae24-ck.md) independently inspected the rendered actual ERD, fresh migrated database, CLI-to-sign-in path, reconciliation boundaries, and each ticket row, recording `PASS`. Local disposable Docker evidence remains the verification basis; no production/provider deployment claim is made.
