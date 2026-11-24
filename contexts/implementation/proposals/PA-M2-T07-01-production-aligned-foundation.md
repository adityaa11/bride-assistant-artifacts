# Proposal PA-M2-T07-01 — production-aligned configurable proof foundation

**Status:** proposed, not approved or effective. **Submitted:** 24 November 2026, Asia/Bangkok.  
**Code repository:** https://github.com/adityaa11/bride-assistant.git at `ad10fb47284517d0a77bdc39d228764f03b4c630`.  
**Artifact history preserved:** [five-minute rehearsal](../../../evidence/m2-t07/rehearsal-ad10fb4.md), [blocked GO checkpoint](../../../checkpoints/M2-T07-proof-policy-gate-ad10fb4-blocked.md), and the exact `ad10fb4` code commit remain historical evidence. No existing CK result is superseded because T07 has not entered CK.

## Planning decision requested

Approve a narrow change to T07's outcome: deliver one reusable policy schema and proof-lifecycle behavior contract, exercised with disposable adapters and controlled clocks, while recording the five D-03 decisions as unresolved production gates. T07 technical foundation may reach CK without those approvals. Production proof hardening, operational readiness, and activation may not. The production policy is authoritative; development/test profiles may shorten time and use disposable adapters only through validated non-production selection. No second development-only domain algorithm is permitted.

This is a proposal to amend the frozen contract and referenced contexts, not permission to run GO, approve legal/provider terms, enable uploads, or mark the current blocked checkpoint ready. If adopted, publish one effective-ticket revision and an append-only planning decision identifying this proposal. The user's request authorizes this proposal; revised implementation begins only on an explicit GO for that effective ticket.

## Current evidence and defect

The `ad10fb4` script uses the actual local `DocumentStore` for disposable byte put/read/remove and JSON metadata/audit/history stand-ins. Its expiry function, record shape, and five-minute assertion exist only inside `scripts/m2-t07-proof-expiry.mjs`. No application module imports the function, there is no production caller, no policy schema shared by development and production, and no hold/classification/scanner/recovery decision logic. It proves the original bounded rehearsal, not reusable application behavior or production compliance. Preserve its result as a baseline; rerun a current-revision rehearsal against the shared implementation and collect new evidence. Do not relabel the old capture as proof of the new code.

## Four readiness states

| State | Authority and pass condition | Owner and consumer |
| --- | --- | --- |
| Technical capability ready | T07 CK proves one versioned policy schema, common retention/hold decision path, validated dev/test/production profiles, disposable byte/metadata lifecycle, controlled-clock production-duration cases, and fail-closed activation validation. Pending decision fields remain explicit. | T07 produces `H-M2-07-TECH`; M4-W01/W02 consume the contract. |
| Production policy approved | Accountable business/PDP/legal/accounting/provider owners approve the five D-03 values with dates and evidence. The production profile contains no unresolved mandatory field, and its eight-month/ten-year applicability is approved. | M4-W07 records PDP, notice/contact, scanning, hold and retention determinations; M5-W03 records recovery ownership/targets. T07's decision ledger is the input, not an approval. |
| Production operational ready | M4 binds the same behavior to its production metadata/byte/scan adapters and proves private access, quarantine, fail-closed failures, deletion/hold/audit behavior; M5 proves account-backed database/object restore, RPO/RTO and rollback. | M4-W01/W02/W07 and M5-W03; local tests do not claim provider verification. |
| Production activation authorized | Explicit release decision verifies policy approval and operational evidence, then enables the production proof capability. Unresolved values, dev override, test adapter, or missing provider evidence is a hard deny. | M5-W05 go/no-go; no implicit activation from T07 or M4 CK. |

## Proposed T07 effective scope and handoff

- Own a versioned, validated policy contract and a single environment-neutral proof-lifecycle decision/coordinator path. Inputs: policy profile, classified proof record, upload/final-review timestamps, active-hold facts, controlled/current clock, and injected proof metadata/audit and byte-deletion ports. Outputs: retain/expire/hold/deny decision, reason, next review/expiry time, and idempotent deletion result. The same domain code runs in dev/test and is consumed by M4. No environment branch in domain logic.
- Profiles select values and adapters at the outer boundary. Development/test may use five-minute durations, synthetic data, local `DocumentStore`, disposable metadata stores and controlled clocks. A production profile must encode the approved operational eight-month rule for eligible proof, calendar-accurate ten-year minimum for any class determined statutorily applicable, precise start events, PDP/contact, scanner/quarantine, hold authority, recovery requirements, owners and evidence references. Unknown applicability is explicit and denies production activation/deletion where a statutory obligation could apply; it is not silently classified as eight-month eligible.
- The production validator rejects shortened development durations, disposable adapter identifiers, controlled-clock activation, missing approval provenance, missing scanner/provider/hold/recovery terms, and any override that weakens class-specific minima. Configuration is validated before side effects. An unresolved production profile may be loaded for inspection but cannot activate proof processing.
- Keep scanner, PDP, hold and recovery policy obligations in the shared schema and typed ports. T07 tests fail-closed scanner/quarantine outcomes and active-hold deletion denial using deterministic adapters; it does not choose a scanner provider, legal basis, hold criterion, RPO/RTO, or legal retention classification. M4/M5 implement and verify real adapters and controls.
- Do not alter the accepted T06 `DocumentStore` `put/read` byte contract implicitly. T07 defines a lifecycle deletion port above that byte boundary; M4-W01 owns its production Neon implementation and idempotent failure/retry semantics. This is one byte authority, not another storage backend.
- `H-M2-07-TECH` includes schema/version, module/API contract, supported profiles, readiness flags, code/evidence locators, unresolved decision IDs and consumer map. It contains no raw bytes, names, signed URLs or secrets. A later approved policy record can bind values by version without editing historical evidence. M4-W01/W02 consume behavior; M4-W07 and M5-W03 own their respective gate evidence; M5-W05 owns activation.

**Exclusions:** production proof UI/upload, Neon byte or SQL adapter, live scanner selection, provider restore, real customer/proof data, release approval, and legal/accounting conclusions. The foundation does not claim production integration before M4.

## Proposed review-row mapping

| Existing authority | Amendment | Proposed proof at revised GO/CK |
| --- | --- | --- |
| `RC-M2-07A` retained and expanded | The five-minute byte/metadata/audit/history outcome stays; run it through the shared coordinator and local `DocumentStore`, not the standalone algorithm. | Current-revision disposable integration, before/after keys/audit/history, both origins, idempotency and cleanup; reference `ad10fb4` only as historical baseline. |
| `RC-M2-07B` retained and expanded | Eight-month eligible proof rule still starts at final review or upload if unreviewed; add controlled-clock eight-month boundary tests and a ten-year statutory-class minimum without deciding which real records qualify. | Same behavior path, calendar-boundary and start-event observations, class resolution/unknown-class fail-closed checks, no five-minute production activation. |
| `RC-M2-07C` changed | Replace "five approvals before T07 CK" with "five unresolved decisions explicitly represented and production activation denied until each approved." Preserve the original five approval obligations at M4-W07/M5-W03 and M5-W05 activation. | Decision ledger with owner/evidence slots, negative production validation for each missing value, downstream gate map. No claimed approval. |
| `RC-M2-07D` retained | Exact M4/M5 consumers and private evidence. | Inspect H-M2-07-TECH and artifacts for consumer/version mapping and absence of sensitive material. |
| New `RC-M2-07E` | One schema and implementation across validated dev/test/prod profiles; reject dev override and test adapters in production. | Profile parse/selection negatives and same-code-path traces. |
| New `RC-M2-07F` | Active holds block deletion in every profile; scanner unavailable/infected/timeout fails closed and quarantines through the same contract; booking history remains independent. | Controlled-clock lifecycle integration per profile with deterministic ports; inspect side effects and denial. |
| New `RC-M2-07G` | Technical, policy, operational and activation states remain separate; unresolved authority keeps production disabled. | Versioned handoff, production validator negative cases, explicit non-activation. |

The revised ticket should specify exact harnesses, scenarios, observations and row statuses. The old blocked checkpoint remains immutable; the effective ticket names this amendment and retains its original row IDs where possible. New GO supplies a full checkpoint for the effective contract; CK performs first full review, not CFC repair of a nonexistent T07 CK finding.

## Proposed source edits if approved

1. **M2-T07 and ticket-set README:** revise outcome, activation, EUP, row matrix, validation and state after the planning decision; name `H-M2-07-TECH` and the four readiness gates. T07 is the technical-contract owner, with no duplicate production adapter owner.
2. **D-03 and payment-proof policy template:** make production eight-month eligible retention and any applicable ten-year statutory minimum authoritative, with classification/applicability pending professional determination. Define approved-profile fields and fail-closed handling of unknowns, scanner failure, active holds and recovery. Keep all five deferred decisions and their approval evidence requirements.
3. **Implementation Context M2, M4, M5 and verification:** M2-R03 yields technical foundation plus pending decision ledger; M4-W01/W02 consume the common policy behavior and own live SQL/Neon adapter and UI integration; M4-W07 owns production policy/control evidence; M5-W03 owns account-backed restore and recovery evidence; M5-W05 alone authorizes activation. Update M4 exit language so the five-minute rehearsal is T07 evidence and live retention proof belongs to M4. Update cross-phase verification to distinguish local contract tests from provider and release evidence.
4. **Downstream handoff contracts:** T06 remains the byte-boundary baseline. M4/W01 accepts the versioned coordinator/ports and provides actual lifecycle adapters; M4/W02 calls the same path from authorized proof commands; M4/W07 binds policy approvals and scanner/hold/privacy evidence; M5/W03 binds recovery; M5/W05 checks all gates. No consumer re-approves T07's implementation.

## SQL migration and ERD boundary

This proposed T07 scope adds no table, migration, production seed, migration runner, or SQL-dependent implementation. Its disposable metadata adapter is local, and the current production schema is unchanged. Therefore T07 has no SQL migration to preview or run. M4-W01 owns future durable proof/hold/audit schema and any SQL migrations. Before that ticket writes or runs a migration, show the user the rendered before/after ERD with tables, PKs, FKs and cardinalities; explain each migration's definition, purpose and responsibilities; obtain explicit proposal confirmation; then verify and commit the actual rendered ERD against a fresh migrated local Docker database for GO/CK. No T07 approval substitutes for that confirmation.

## Processing sequence

1. Planning authority reviews this proposal, resolves the T07/M4/M5 ownership split and records approval or changes in an append-only decision. No effective contract changes before that decision.
2. Apply the approved, linked revisions to D-03, policy template, Implementation Context, T07 and ticket-set README. Record the exact changed/retained rows and predecessor/handoff effects.
3. The user invokes `GO M2-T07` against the effective revision. GO implements the shared technical foundation, reruns current-revision evidence and submits an `awaiting_review` checkpoint only when every revised technical row is proven.
4. `CK M2-T07` reviews that checkpoint. Approval unlocks M4 construction through `H-M2-07-TECH`; it does not approve production policy, operations or activation.
5. Accountable owners supply the five decisions; M4/M5 prove real controls; M5-W05 decides activation. Any applicable M4 SQL migration follows the separate ERD confirmation gate.

**Next step:** planning approval of this proposed amendment, followed by publication of the effective context/ticket revision. Do not run CK on the current blocked T07 checkpoint.
