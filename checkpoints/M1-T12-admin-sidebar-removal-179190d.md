# M1-T12 — user-directed admin sidebar removal

- Ticket: `M1-T12` (A5)
- State: `awaiting_review`
- Code repository: `https://github.com/adityaa11/bride-assistant.git`
- Review target code commit: `179190d6292efcf654ac6d5d4f0aa054ae7a1b6b`
- Predecessor checkpoint: `92b0cf016504ecf4ac23ec6981efd2db74a2a4ed`

## Change

Removed the user-selected offline sidebar line and the entire Customer experience group, including its label, Public Booking Page link and divider, from the shared admin shell. The studio card and other navigation groups remain.

## Review Contract Closure

| Row | Authority and required proof | Evidence / exact validation command and outcome | Status |
| --- | --- | --- | --- |
| A5-SIDEBAR | User browser comments and ticket A5: offline line and Customer experience group absent on both admin routes. | `tests/m1-t12.visual.spec.ts` asserts the three named texts/links are absent; `M1_T08_BASE_URL=http://localhost:3000 M1_T12_REFERENCE_URL=http://127.0.0.1:3001 M1_T12_CAPTURE_DIR=C:\Workspace\bride-assistant-artifacts\evidence\M1-T12-r3 npm run test:m1-t12:visual` — 2/2 PASS. | PROVEN |
| VIS-REF-M1-T12 | Retained source-mapped regions keep their geometry and responsive behavior after sidebar removal. | [Fresh paired captures](../evidence/M1-T12-r3/) at 1440, 1251/1250, 931/930, 721/720, 486/485 and 420px; same visual command — 2/2 PASS. Narrow content geometry is compared relative to content origin per A5. | PROVEN |
| RC-22 / SEC-M1-01/02 | Canonical fixture states, no mutation/identity/storage authority, explicit action result. | `npm run check` — PASS (lint, typecheck, 11 unit tests, build); visual interaction check — PASS. | PROVEN |

Internal readiness: READY_FOR_CK. This is not a self-issued CK PASS.

## Actual Use Path

`/admin`, `/admin/availability`, and `/admin/bookings/empty` still reach `AdminBookingWorkspace` through named T02 scenario IDs. The shared sidebar now renders Workspace and Website Content navigation plus the studio card. Booking and Availability controls retain typed fixture props and local non-authoritative results. The omitted sidebar group has no runtime caller.

## Actual Empty-State Evidence

`admin.booking-empty` and no-match filter states remain explicit and are exercised by the visual spec. No record or seed is created.

## Migration Inventory

No migration or production-data change.
