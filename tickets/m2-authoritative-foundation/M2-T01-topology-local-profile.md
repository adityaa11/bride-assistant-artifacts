# M2-T01 — verified production topology and disposable local profile

- State: `awaiting_review`; batch 1; work package `M2-R01`. Planning baseline source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`. GO implementation checkpoint: [M2-T01 / `123e6b4`](../../checkpoints/M2-T01-topology-local-profile-123e6b4.md).
- Authority: [M2-R01](../../contexts/implementation/milestones/M2-foundation.md), [D-05/D-06](../../contexts/implementation/decisions.md), [Stack](../../contexts/stack.md), [architecture](../../contexts/implementation/architecture.md). Dependency: M1.1 exit PASS. Successors: T02/T03/T07 via `H-M2-01`.
- Activation: `preparation-only`, T03 activates the deployed shell connection. Legacy: forbidden. SQL: `ephemeral-test-setup` for disposable local PostgreSQL only; no intended migration. Empty state: fresh database/bucket has no customer rows or uploads; connection and privacy checks must work without seeding. Reuse: `RD-M2-02` local DocumentStore byte boundary.

## Outcome and exact scope

Provision and verify Vercel application region `sin1`, Neon PostgreSQL 18.7 and Neon Object Storage in Singapore, a private bucket, server-only credentials, permitted short-lived signed access after application authorization, and account-backed connection/backup/PITR terms. Establish Docker local PostgreSQL 18.7 and a disposable local DocumentStore profile. Record the region and environment separation without credential values. Excludes real customer data, production proof upload, authorization implementation, and a second byte store.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: deployment/operator configuration owned by the environment owner; inputs are provisioned Vercel/Neon account identifiers, selected regions, secret references and Docker profile, all operator-controlled but unverified until probed. Reject mismatched region/version, public bucket, missing private-access capability, or failed connection rather than marking topology ready.
- Endpoint: versioned, redacted topology verification record plus usable local profile and deployment secret references; success is observed region/version, private-bucket denial without authorization, server credential isolation, connection and documented backup/PITR terms. No runtime user capability is claimed yet.
- `H-M2-01`: configuration names and verified account/region/version/bucket/connection/recovery observations, no raw secrets or customer data; T02 consumes runtime constraints, T03 consumes deployment/local connection profile, T07 consumes recovery facts. Environment owner maintains values; no retry or idempotency contract applies to the record itself.
- `EUP-M2-01`: operator/CI configuration → topology probes and Docker profile → redacted record/local service → T03 deployment and migration integration. T03 must prove the actual production-like application-to-Neon path; a local-only test is insufficient for that successor.

## Invariants, exclusions and true path

Neon owns production relational data and uploaded bytes, Vercel only hosts application work, and every upload byte will cross DocumentStore. Local volumes are disposable, ignored, and never receive real data. True path after T03: Vercel server-side application → Neon; Docker → isolated local services. No Vercel Blob, public proof bucket, fixture directory, or PostgreSQL byte-column fallback. Backup/PITR capability evidence is not a restore rehearsal; M5-W03 owns that.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-01A` | Provisioned Vercel `sin1`, Neon PostgreSQL 18.7 and Object Storage Singapore match D-05/D-06, with reachable endpoint and account-backed backup/PITR terms recorded. | Provider console/API inspection and redacted connection/version probe at the provisioned account; evidence ledger names environment and time. | Marketing/docs claim or local Docker version alone; wrong-region/data-authority regression. |
| `RC-M2-01B` | Bucket is private by default, unauthenticated access fails, signed-access mechanism is limited and server credentials never appear in client bundle or committed files. | Negative object request, signed-access capability probe with disposable object, configuration/client-bundle/secret scan. | Merely naming a private bucket; credential/public-byte leak. |
| `RC-M2-01C` | Fresh Docker profile starts PostgreSQL 18.7 plus disposable local DocumentStore with zero records/objects; teardown leaves no intended migration/seed or source-controlled bytes. | Clean-volume startup, connection/read/put probe, teardown and Git inventory. | Preloaded fixture or test seed in migration graph; fixture isolation regression. |
| `RC-M2-01D` | `H-M2-01` gives T02/T03/T07 the exact redacted connection, region, bucket, credential-reference and recovery facts needed to continue. | Inspect versioned handoff and repeat a probe using named environment. | Unverified diagram or secret in artifact; successor cannot reproduce. |

## Security readiness and validation

- `SEC-M2-01` `implement-now`: provider region, private storage, server credential isolation and environment separation; Project/Stack D-05/D-06, ASVS configuration/data-protection and communication topics, OWASP misconfiguration, NIST CSF Protect and SP 800-53 CM/SC. `SRB-M2-01` reviews `RC-M2-01A/B/C` with account probes and negative access evidence.
- `SEC-M2-07` `preserve-seam`: application authorization precedes future signed private access; T01 proves capability only, T05/M4 own actor policy and issuance. No public URL is inferred from storage key. `SRB-M2-07` inspects the configuration boundary.
- Validation: fresh local startup and account-backed probes at current provisioned state; no prior M1 screenshot proves topology. Hard stop: unavailable account access, incompatible Singapore/version/private capability, or unacceptable recovery terms requires D-05/D-06 planning review before downstream activation.
