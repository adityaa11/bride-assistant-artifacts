# M1-T12 — user-directed admin annotation removal

- Ticket: `M1-T12` (A4)
- State: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target code commit: `f47e21187159d73d0a1fee4d9804af8d4930f04d`
- Predecessor checkpoint: `d0111996b392f847b3ca873a506947e89ba975ce`

## Change

Removed the two blocks selected in the user's browser comments: the Bookings `Fixture note:` below the calendar/list grid and the shared `Non-authoritative fixture` footer. The footer is absent from Bookings and Availability. The source-mapped Availability note remains. The T02 scenario adapter, fixture values, statuses, visible non-authoritative action result and operational boundaries are unchanged.

## Review Contract Closure

| Row | Authority and required proof | Evidence / exact validation command and outcome | Status |
| --- | --- | --- | --- |
| A4-ANNOTATIONS | User browser comments and ticket A4: both selected blocks absent on live Bookings; shared footer absent on Availability. | `tests/m1-t12.visual.spec.ts` asserts absent nodes; `M1_T08_BASE_URL=http://localhost:3000 M1_T12_REFERENCE_URL=http://127.0.0.1:3001 M1_T12_CAPTURE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T12-r2 npm run test:m1-t12:visual` — 2/2 PASS; [fresh paired captures](../evidence/M1-T12-r2/). | PROVEN |
| VIS-REF-M1-T12 | Preserve all unaffected source-mapped regions and responsive geometry. | Same Playwright command — 2/2 PASS across ten widths for both panels; [updated reference ledger](../evidence/M1-T12-reference-ledger.md). | PROVEN |
| RC-22 / SEC-M1-01/02 | Keep canonical fixture states, no mutation/identity/storage authority, and explicit result on apparent actions. | `npm run check` — PASS (lint, typecheck, 11 unit tests, build); visual spec interaction test — PASS. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK PASS.

## Actual Use Path

`/admin` and `/admin/availability` still reach `AdminBookingWorkspace` through their named T02 scenario IDs. Booking search, filter, calendar/list, detail and Availability controls continue to use typed fixture props and local state; apparent actions render `role="status"` feedback. The removed blocks are absent from the shared rendered tree. `/admin/bookings/empty` remains an explicit fixture state.

## Actual Empty-State Evidence

`admin.booking-empty` still renders the source-derived empty panel without creating a record. The no-match filter and reset are exercised by the visual spec. No initialization write or durable seed was added.

## Migration Inventory

No migration or production-data change.
