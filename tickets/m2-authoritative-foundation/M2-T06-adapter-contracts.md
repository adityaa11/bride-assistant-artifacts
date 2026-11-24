# M2-T06 — repository and universal DocumentStore contracts

- State: `awaiting_review`; batch 6; work package `M2-W04`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [Project uploaded-media rule](../../contexts/project.md), [Stack DocumentStore](../../contexts/stack.md), [M2-W04](../../contexts/implementation/milestones/M2-foundation.md), [architecture](../../contexts/implementation/architecture.md), [M1.1 W02](../../evidence/M1.1-W02-disposition-79b3478.md). Dependencies: T03 `H-M2-03`, T04 `H-M2-04`, T05 `H-M2-05`. Successors: M3 catalogue/booking repositories and M4-W01/W05/06 via `H-M2-06`.
- Activation: `preparation-only`; named M3/M4 tickets activate authoritative business repositories and Neon Object Storage. Existing T18 local fixture path stays review-only. Legacy: forbidden for production; fixture routes explicitly remain non-authoritative. SQL: no intended domain migration; ephemeral contract fixtures only. Empty state: no rows/objects yields empty read/`null`, never a fixture fallback or hidden record.

## Outcome and exact scope

Define protected repository/adaptor contracts for future authoritative reads/commands and the one storage-neutral DocumentStore byte contract for *all* application uploads, including portfolio, testimonial, event and payment-proof files. Preserve opaque key, SHA-256, size and media type metadata; byte storage remains outside PostgreSQL. Compare fixture/UI contracts without promoting fixture associations to production. Excludes concrete Neon private-byte adapter (M4-W01), durable content lifecycle/publication (M4-W05/06), booking/catalogue schema or commands (M3), and a speculative generic repository implementation.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: future server-side feature adapter/repository composition called only after T05 authorization; inputs are trusted actor/validated domain input, operation-specific record identity and a DocumentStore `put({bytes, mediaType, collection})` or `read(storageKey)` call. `bytes`/metadata originate from untrusted uploads and must be validated by the owning M4 path before production put. Unsupported collection/type, malformed key and missing record return explicit errors/absence; key alone never authorizes read.
- Endpoint: typed repository result/absence and byte `put` metadata `{storageKey, sha256, size, mediaType}` or `read` bytes/absence. T06 defines contracts and contract-test results, not an active production upload endpoint.
- `H-M2-06`: versioned repository input/output/error semantics and universal DocumentStore contract; M3 consumes repository seams, M4-W01 implements Neon bytes, M4-W05 binds content records, M4-W02 binds proof metadata. Identity/correlation is actor + owning record ID at the facade/repository layer; storageKey remains opaque; M4 owns atomic association, retry and cleanup decisions. No public delivery URL in this handoff.
- `EUP-M2-06`: T05 actor/validated request → future M3/M4 feature facade → repository or DocumentStore contract → durable record/byte result → named M3/M4 customer/admin/public effect. Current T06 evidence is contract conformance against disposable adapters; successor activation must prove its actual path. This deferral is authorized by M2-W04's explicit M3/M4 handoff.

## True path, reuse and invariants

`RD-M2-01/02/03`: retain `getScenario` only for declared review routes; extend/normalize `shared/document-store.ts` as the sole byte interface and keep `ContentImageStore` association lifecycle above it. Production paths must not import fixture scenario data, local manifests, browser data URLs, static uploaded files or a second media store. Do not add a compatibility fallback to local fixture bytes when Neon is unavailable. M4 applies private-access and publication decisions before reads/delivery. T18 opaque metadata, collection isolation and local replace/cleanup remain direct fixture regressions.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-06A` | One DocumentStore byte contract covers the four named upload classes and returns opaque key/hash/size/type without storing bytes in PostgreSQL or a second store. | Contract suite across disposable content/proof adapters; byte/hash/type assertions and source dependency graph. | Image-only interface or per-feature byte writer; T18 media regression. |
| `RC-M2-06B` | Protected repository contracts require T05 actor/validated data, define typed absence/errors, and cannot infer authorization from key or fixture route. | Contract tests for allowed/denied/missing-record/empty cases and source type/import inspection. | Unit-only unguarded repository export; ownership bypass. |
| `RC-M2-06C` | Fixture UI read models for portfolio/testimonials/events and proof placeholders retain their stated contract, while production-facing seams have no direct fixture import or fallback. | M1 fixture route/contract regressions and import/call graph, with named future M3/M4 consumers. | Claiming fixture output is durable data; breaking review route. |
| `RC-M2-06D` | Fresh empty byte/record store returns absence without initialization; test setup is disposable and no test-only SQL enters migration graph. | Empty adapter and DB contract run, teardown and migration inventory. | Seed data or hidden default; polluted migration graph. |

## Security readiness and validation

- `SEC-M2-06` `implement-now`: one governed byte boundary, opaque metadata and actor-first repository contract; Project/Stack D-C03, ASVS data protection/file/API, OWASP access control/injection, NIST CSF Protect and SP 800-53 AC/SC/SI. `SRB-M2-06` binds `RC-M2-06A/B/D` to contract and dependency evidence.
- `SEC-M2-07` `preserve-seam`: private proof read, signed URL issuance, content permission/publication and scanner/retention policy remain M4/T07 gates. `SRB-M2-07` binds `RC-M2-06C` and source path review; no policy is inferred from an opaque key.
- Validation: current-revision contract and fixture-route checks; existing M1 visual baselines may be cited only if unchanged, while current fixture route observations are required for changed routes. Hard stop: if a universal byte contract cannot preserve the fixture interface and accepted Stack semantics, return to planning rather than add a second store.

## GO checkpoint

- [M2-T06 implementation checkpoint](../../checkpoints/M2-T06-adapter-contracts-d806c93.md) at code commit `d806c9357a2ffb8404ad4aac0a66821449ce0a7e` (GO conversation confirmed unchanged-schema ERD proposal before work).
