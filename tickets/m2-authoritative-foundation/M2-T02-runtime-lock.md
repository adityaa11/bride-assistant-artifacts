# M2-T02 — exact approved runtime and dependency lock

- State: `planned`; batch 2; work package `M2-R02`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [M2-R02](../../contexts/implementation/milestones/M2-foundation.md), [D-02](../../contexts/implementation/decisions.md), [architecture](../../contexts/implementation/architecture.md). Dependency: T01 `H-M2-01`. Successor: T03 via `H-M2-02`.
- Activation: `active-in-this-ticket` for the build/test toolchain, not an auth capability. Legacy: forbidden. SQL and empty state: not applicable; no persistence behavior. Reuse: existing Next/React/TypeScript package and scripts, not a new framework.

## Outcome and exact scope

Apply the confirmed starting versions: Node 24.22.3; Next 16.4.6; React/React DOM 19.3.1; TypeScript 5.9.3; Better Auth 1.7.26; Zod 4.6.7; `pg` 8.23.1; `@types/node` 24.6.6; PostgreSQL 18.7 from T01. Pin exact dependency and lockfile resolution, verify peer compatibility and advisory state, and run clean installation, lint/type/test/build. Excludes arbitrary upgrades, Node 26, enabling additional Better Auth providers, auth wiring, and provider changes. A necessary security patch within accepted majors still requires the D-02 recheck and repeat evidence.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: project package manifest, lockfile and CI/build scripts; caller is clean checkout/CI; inputs are the approved version list and T01 runtime constraints. Invalid peer graph, integrity failure or advisory conflict stops T03.
- Endpoint: reproducible clean install and passing check/build at the exact lock plus version/integrity ledger; no user-facing state change.
- `H-M2-02`: exact manifest/lockfile revisions, Node/PostgreSQL version, executed commands/results and any integration constraints; T03 consumes them. CI owns repeatability; fresh install must be idempotent.
- `EUP-M2-02`: clean checkout/CI → package manager install → project check/build → deployable artifact consumed by T03. A cached developer install alone does not prove this path.

## True path and invariants

One committed lockfile supplies the toolchain; remove stale version/config paths that could select a conflicting runtime. Existing application fixture routes remain visually and behaviorally intact. Do not introduce Node 26, an ORM, or a different auth provider by inference. Read relevant `node_modules/next/dist/docs/` guides before Next code changes.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-02A` | Manifest, engine constraints and lockfile resolve the D-02 exact starting versions or a documented D-02-approved patched equivalent. | `npm ci`, package/lock inventory, runtime version output and peer resolution at a clean checkout. | Existing `node_modules`; version drift or mixed lockfile. |
| `RC-M2-02B` | Lint, TypeScript, tests and production build pass on the clean lock with the current fixture routes retained. | Command logs from the same code revision and Node runtime; direct M1 route/scenario regression. | Install-only success; broken M1 fixture path. |
| `RC-M2-02C` | Dependency integrity/advisory review records any relevant Next/Better Auth/Zod/pg issue and no unapproved provider, auth feature or Node major is activated. | Lockfile integrity/provenance and version-scoped advisory ledger. | Generic assertion of safety; new vulnerable or unapproved dependency path. |

## Security readiness and validation

- `SEC-M2-02` `implement-now`: D-02 lock integrity, compatibility and advisory recheck; ASVS dependency/configuration, OWASP supply-chain/misconfiguration, NIST CSF Identify/Protect and SP 800-53 SA/SR/CM. `SRB-M2-02` maps to `RC-M2-02A/C` with clean install and dependency ledger.
- `SEC-M2-02P` `preserve-seam`: avoid binding future auth/data interfaces to a package that this ticket has not approved. The exact Better Auth adapter/ORM remains a T04 integration choice under the accepted `pg` boundary; an incompatible choice triggers planning.
- Validation uses new logs at the T02 code revision. Hard stop: unresolved peer conflict or relevant security advisory requires D-02 review; no silent version substitution.
