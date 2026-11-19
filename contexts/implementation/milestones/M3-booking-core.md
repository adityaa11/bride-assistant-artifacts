# M3 — Authoritative catalogue, booking, and availability

**Status:** `planned` · **Advances:** shared record, package/promotion control, snapshot integrity, official status, availability integrity.  
**Outcome:** A customer submits a durable request and an authorized admin manages the same record through canonical core states; only `Booking Berhasil` reserves a date. **Entry:** M2 exit and its contracts. **Excludes:** proof upload, cancellation/reschedule, and full publishing. **Handoff:** verified aggregate to M4/M5. **Risk:** race or snapshot mutation; contain with transaction/constraint/concurrency tests.

| ID | Bounded outcome / scope; exclusions | Dependencies | Interfaces and preserved/replaced boundary | Evidence / handoff | Fixture → backend replacement / open trigger |
| --- | --- | --- | --- | --- | --- |
| M3-W01 | Catalogue/packages/prices/promotion applicability model and migration; excludes booking commands. | M2-W01/M2-W04 `sequence` | Replaces prototype catalogue records; creates future-selection read/write seam. | Relationship/duplicate-expansion tests; handoff W02–05. | N/A. |
| M3-W02 | Admin catalogue/price/promotion commands; excludes retroactive booking edits. | M3-W01/M2-W03 `sequence` | Replaces M1-W06 catalogue fixtures; preserves future-only change boundary. | Authorized CRUD and snapshot regression; handoff W03/W04. | M1-W06 → M3-W02; invariant: existing snapshots do not change. |
| M3-W03 | Public/customer package and eligible-promotion reads; excludes drafts/admin fields. | M3-W01 `sequence` | Replaces M1-W04 package fixtures. | Published/active visibility tests; handoff W05. | M1-W04 → M3-W03; invariant: no admin/draft leak. |
| M3-W04 | Booking, submitted package/promotion snapshot, and status history model; excludes UI command flow. | M3-W01 `sequence` | Replaces no live data; establishes official aggregate. | Immutable snapshot/history tests; handoff W05/W06. | N/A. |
| M3-W05 | Customer submission, Booking Saya read, post-submit wa.me preparation; excludes live number if D-04 unresolved. | M3-W03/M3-W04/M2-W03 `sequence` | Replaces M1-W05 booking fixture; retires localStorage submission. | Durable record precedes link; cross-customer denial; handoff W07/M4. | M1-W05 → M3-W05; invariant: absent WhatsApp never loses booking. |
| M3-W06 | Admin queue/detail and core status commands; excludes exceptions. | M3-W04/M2-W03 `sequence` | Replaces M1-W06 booking fixture; retires prototype statuses. | Admin-only canonical transition tests; handoff W07/M4. | M1-W06 → M3-W06; invariant: one record and admin authority. |
| M3-W07 | Transactional availability protection; excludes temporary holds. | M3-W05/M3-W06 `sequence` | Replaces availability fixtures; preserves only-successful booking block rule. | Conflict/concurrency/privacy tests; handoff M4/M5. | M1 availability scenario → M3-W07; invariant: no identity disclosure and no pre-success block. |

**Graph:** `M3-W01 → M3-W02/M3-W03/M3-W04; M3-W03 + M3-W04 + M2-W03 → M3-W05; M3-W04 + M2-W03 → M3-W06; M3-W05 + M3-W06 → M3-W07.` Parallel-safe: W02/W03/W04 after W01.  
**Exit:** same-record journey, immutable snapshot, valid/invalid transition, date-conflict/concurrency evidence pass; failure returns M3 to rework.

