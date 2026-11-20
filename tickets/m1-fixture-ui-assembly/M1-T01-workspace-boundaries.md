# M1-T01 — Establish review-build workspace and boundary checks

- State: `awaiting_review`; batch/milestone: `1 / M1 / M1-W01`.
- Source authority: M1-W01; implementation architecture (delivery/UI transition); Project Context current/target state.
- Dependencies: none. Successors/handoff: `H-01` to T02/T03. Activation: `active-in-this-ticket` (review build only).
- Reuse decision: `keep-local` — root workspace conventions have one consumer boundary. Legacy path: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome and exact scope

Create the minimal Next.js/React/TypeScript workspace, public/customer/admin route-group boundary, shared-package convention, and root checks that can compose fixture UI. Configure no production service. Exclude auth, database, storage, API routes, deployment/provider setup, and copying prototype scripts.

## Entrypoint, endpoint, handoff, expected use path

Entrypoint: repository root package scripts, invoked by developer/CI with no business input. Endpoint: deterministic build/type/lint/test check result and route-group composition availability. `H-01`: root/package conventions and check commands; producer T01, consumers T02/T03, transport source imports; integrity is version control plus type checks; no retries/idempotency concern. EUP-01 is `root scripts → checks → exit status → developer/CI feedback`.

## True path, invariants, non-goals

The one path is route group → shared package → rendered fixture UI. Make prototype HTML's direct script/localStorage paths unreachable by not importing them. Preserve framework escaping, CSP-safe component discipline, and the fact that route grouping is not authorization. Do not add compatibility wrappers, environment secrets, or mock API endpoints.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-01 | M1-W01 | PASS iff root checks run from a clean clone and report success/failure; command output is retained; a manually opened HTML file is not evidence; workspace bootstrap boundary. |
| RC-02 | Architecture delivery/UI transition | PASS iff public/customer/admin route groups and shared seam exist without importing reference HTML/localStorage code; import scan + route render prove it; visual lookalike alone is forbidden; fixture-only boundary. |
| SEC-01 | SEC-M1-02 | PASS iff new client code uses framework rendering and has no HTML sink, inline executable code, string compilation, or unreviewed external origin; targeted search/review; passing build alone is forbidden; browser/CSP seam. |

## Security/UI/validation

Security readiness: `applicable`; SEC-M1-01 `preserve-seam` (no client authority), SEC-M1-02 `implement-now`. UI readiness: no screen; reserve T03 visual-contract ownership. Validate root checks and a route smoke render. Hard stop: any need for API/auth/storage or CSP-header change returns to planning; none is authorized here.
