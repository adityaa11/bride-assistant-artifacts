# M2-T05 — server actor, ownership and input boundary

- State: `awaiting_review`; batch 5; work package `M2-W03`. Source code: `https://github.com/adityaa11/bride-assistant.git` at `79b3478995737dd73165afaba50369dcb5242a08`.
- Authority: [Project role/privacy rules](../../contexts/project.md), [Stack authorization/Zod](../../contexts/stack.md), [M2-W03](../../contexts/implementation/milestones/M2-foundation.md), [architecture](../../contexts/implementation/architecture.md). Dependency: T04 `H-M2-04`. Successors: T06 and M3/M4 via `H-M2-05`.
- Activation: `active-in-this-ticket` for role/session denial on existing protected workspace routes; ownership and payload contracts are preparation for T06/M3/M4 with disposable test-scoped records. Legacy: forbidden. SQL: `ephemeral-test-setup` only; no production migration. Empty state: anonymous access denies; an empty customer scope yields no record and never another customer's data.

## Outcome and exact scope

Create one server-only session-to-actor, role and optional record-ownership decision boundary plus a Zod parse/error convention for untrusted command/read inputs. Prove role denial on the existing protected routes; prove owner and payload behavior through test-scoped contract consumers until T06/M3/M4 activate real domain records. Excludes catalogue rules, booking transitions, proof-file policy and a generic global permission registry. Ownership checks must use authoritative record owner data, not route shape or supplied owner ID alone.

## Entrypoint, endpoint, handoff and expected use path

- Entrypoint: existing protected workspace route consumes the role/session decision. Future protected command/read composition consumes T04 `{subjectId, role, sessionId/expiry}` or anonymous, `requiredRole`, optional authoritative `recordOwnerId`, untrusted `payload`, and a named Zod `schema`. Caller owns where the trusted owner value is read. Malformed payload returns typed validation failure; anonymous/wrong-role/wrong-owner returns typed denial without data/side effects.
- Endpoint: existing route gives protected page or denial; reusable decision returns `{actor, parsedData}` or typed validation/authorization failure without mutation. Error schema avoids private details.
- `H-M2-05`: server-only authorization/parse contract with named parameters above, result shape, denial/validation semantics and owner provenance; T06 and M3/M4 consume it before repository or DocumentStore access. No retry/idempotency issue for pure decisions.
- `EUP-M2-05`: authenticated request → T04 session actor → T05 role decision → existing protected workspace or denial. Test-scoped record/schema consumers prove the future owner/validation contract; T06/M3/M4 must show their own production repository paths. A unit test alone does not prove active route denial.

## True path and reuse

`RD-M2-05 extract-shared`: T05 owns the focused server contract used by T06 and directly planned M3/M4 protected operations. The same actor and validation result semantics justify reuse; each consumer retains its own domain rules, record lookup and error mapping. Remove reachable local role checks based only on UI state, fixture identity or URL. No client-side enforcement is authoritative. Keep policy extension point at actor/record/resource decision without choosing future admin delegation.

## Acceptance criteria / Review Contract

| ID | PASS iff required behavior and observation | Harness and evidence | Forbidden substitute / direct regression |
| --- | --- | --- | --- |
| `RC-M2-05A` | Anonymous and customer-on-admin requests are denied on existing protected routes; another customer's authoritative owner ID fails in the test-scoped decision contract without data/side effect. | Actual route requests with T04 sessions plus disposable owner-record contract fixture; route result and no-write observation. | Hidden UI control or unit-only proof of active route denial; cross-customer exposure. |
| `RC-M2-05B` | Valid actor/owner and valid schema return the allowed typed result, while malformed payload and forged owner/role values fail safely in the test-scoped contract. | Valid/invalid payload and forged identity table against the same contract with disposable records. | Zod client-only validation or trusting query/body owner. |
| `RC-M2-05C` | `H-M2-05` is server-only, has explicit inputs/errors, and the active protected probe consumes it; T06 receives the named interface without copying authority logic. | Import/call graph, active probe trace and handoff contract at current revision. | Generic registry or unused production export. |
| `RC-M2-05D` | Empty owned-record scope returns the contract's empty result without creating a default or leaking another actor's data. | Fresh/empty disposable database plus one other-customer record, route response and no-write check. | Seed migration or hidden default. |

## Security readiness and validation

- `SEC-M2-05` `implement-now`: server role/ownership and Zod untrusted input boundary; Project/Stack, ASVS authorization/input/API, OWASP broken access control/injection, NIST CSF Protect and SP 800-53 AC/SI. `SRB-M2-05` binds `RC-M2-05A/B/D` to direct negative integration evidence.
- `SEC-M2-05P` `preserve-seam`: later admin delegation or resource-specific policy can attach to the explicit actor/resource boundary, without embedding identity in storage keys. `SRB-M2-05P` binds `RC-M2-05C`.
- Validation: current-revision route tests plus pure decision tests; test records are ephemeral. Hard stop: if an accepted actor/owner source cannot be established, return to T04 or planning rather than infer it from client input.

## GO checkpoint and migration confirmation

- Code commit: `4cac47c1cc4a3b3d0998372d23f4ba4561cc2dc0`; [GO checkpoint](../../checkpoints/M2-T05-authorization-validation-4cac47c.md).
- On 24 November 2026, after the rendered unchanged-schema preview and definitions, purpose and responsibilities of migrations `0001` and `0002` were shown, the user explicitly replied: “Confirm unchanged schema and proceed.” No production migration changed; disposable local migrations and tests followed that confirmation.