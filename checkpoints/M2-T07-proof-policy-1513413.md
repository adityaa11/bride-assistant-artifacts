# M2-T07 GO checkpoint — configurable proof foundation

- Ticket: [M2-T07 revision 2.0](../tickets/m2-authoritative-foundation/M2-T07-proof-policy-gate.md); batch 2P; state `awaiting_review`.
- Code repository: https://github.com/adityaa11/bride-assistant.git; exact implementation commit `15134132805a5dce25ebd88287f96646110530e2`.
- Artifact store: `C:/Workspace/bride-assistant-artifacts`; origin `https://github.com/adityaa11/bride-assistant-artifacts.git`; `atlas-workflow` layout. Pre-write staged index was empty; only four pre-existing `.gitkeep` paths were untracked.
- Predecessor T01 CK PASS and T06 CK PASS. The historical `ad10fb4` rehearsal and blocked checkpoint remain unchanged; PA-M2-T07-01 defines the effective revision-2 contract.

## Bounded implementation

`shared/proof-policy.ts` owns one versioned validation and lifecycle path. `config/proof-retention.profiles.json` contains development, isolated test and production profile values. The former standalone five-minute expiry algorithm was removed from `scripts/m2-t07-proof-expiry.mjs`; that harness now invokes the shared coordinator with actual local `DocumentStore` bytes and disposable metadata/audit/history ports. No application proof endpoint, production worker, provider adapter, scanner service, UI, SQL migration or production-data change was added.

## Review Contract Closure

| Row | Ticket authority and required proof | Evidence, exact validation command and outcome | Status |
| --- | --- | --- | --- |
| `RC-M2-07A` | Ticket A: five-minute reviewed/upload origins; before/pre/after bytes, metadata, audit, history; idempotent teardown through shared path. | [current harness observations](../evidence/m2-t07/current-1513413.json), `five-minute-reviewed-and-upload`: bytes 2→2→1→0→0, metadata same, audit 3→3→2→0→0, history 2 throughout. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |
| `RC-M2-07B` | Ticket B, D-03: eight calendar months; two start events; synthetic ten-year minimum; unknown classification denial in isolated controlled-clock test. | Same evidence, `authoritative-calendar-and-statutory`: 4 bytes before boundary, 2 at 30 Sep 2026, 2 before 31 Jan 2036, 1 after; unknown remains. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |
| `RC-M2-07C` | Ticket C, PA-M2-T07-01: five pending decision slots; one denial per missing approval; no approved-policy claim. | [H-M2-07-TECH](../evidence/m2-t07/H-M2-07-TECH-v2.md) ledger and current evidence's five `missing ...` denials. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |
| `RC-M2-07D` | Ticket D: exact M4/M5 consumer map and no sensitive payload in handoff/evidence. | Handoff maps M4-W01/W02/W07 and M5-W03/W05; source/artifact inspection found no raw proof bytes, names, signed URLs or secrets. `rg` scan and handoff inspection PASS. | `PROVEN` |
| `RC-M2-07E` | Ticket E: one policy schema/domain path, trusted identity and bidirectional profile/adapter isolation before effects. | Versioned source/profile plus current evidence: unknown/forged identity, environment override, production short duration/clock/disposable adapter, four test-side production resource/data denials; unvalidated runtime causes zero port calls. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |
| `RC-M2-07F` | Ticket F: hold prevents deletion, scanner infected/unavailable/timeout quarantined, booking history intact. | Current evidence `hold-and-scanner-fail-closed`: 5→4 bytes/metadata/audit, 5→5 history, only eligible proof deleted; shared coordinator tested with local bytes. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |
| `RC-M2-07G` | Ticket G: four distinct readiness states; unresolved approval/config/operations/release deny activation. | Current evidence activation matrix: pending approvals, approved without operations, and operations without release all keep activation false; synthetic all-true matrix demonstrates separate final gate. `node scripts/m2-t07-proof-expiry.mjs` PASS. | `PROVEN` |

Additional validation at the code commit: `npm run lint` PASS; `npm run typecheck` PASS; `npm test` PASS 13/13; `npm run build` PASS after rerun with Windows junction permission; `git diff --check` PASS. The first sandbox build attempt could not create a Turbopack junction and did not reflect a code failure. No Vercel or Neon probe was required; no provider path changed.

Internal readiness: READY_FOR_CK. CK alone determines PASS.

## Actual Use Path

`EUP-M2-07`: trusted test identity in `scripts/m2-t07-proof-expiry.mjs` → `validateProfile` → branded validated runtime → `runProofLifecycle` → local `DocumentStore` deletion/read plus disposable metadata/audit ports → before/after snapshots in current evidence → [H-M2-07-TECH](../evidence/m2-t07/H-M2-07-TECH-v2.md) → M4-W01/W02 production adapter/caller ownership. The standalone script's former expiry implementation is retired. T07 is technical preparation; M4 proves its live caller and provider persistence.

## Actual Empty-State Evidence

The unique disposable root is absent before the harness. A no-record `list` returns zero deletions without creating a byte directory; the first synthetic `put` creates only local test bytes. Each scenario's final snapshots show byte/metadata/audit removal or deliberate retained records; booking history survives. The harness removes the entire unique root in `finally` and asserts its absence.

## Migration Inventory

No intended SQL migration, production-data change or seed. Only ignored local byte fixtures and in-memory metadata/audit/history are used; `finally` removes the byte directory. No Docker PostgreSQL is needed by this chosen adapter. M4-W01 owns future SQL and must obtain its own rendered ERD confirmation before migration work.
