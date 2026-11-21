# M1-T12 reference ledger — pre-implementation

- Ticket: `M1-T12`; authority: frozen ticket A1/A2/A3 and the M1 UI reference contracts.
- Code baseline inspected: `6056e77b79495231d0510638064fdd0dfdf37eb6` in `https://github.com/adityaa11/bride-assistant.git`.
- Binding source: `Bride_Assistant_Planner_Standalone_v3_Content_Management.html`, rendered at `http://127.0.0.1:3001/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` on 2026-11-22.

## Bookings

- Desktop source composition: 238px soft-rose sidebar; 31px content inset; breadcrumb/top-actions; eyebrow `YOUR BRIDAL WORKSPACE / BOOKINGS`; `Booking Calendar.` serif title; subordinate explanatory copy and `New Booking` action.
- Ordered regions: full navigation (workspace, website content, customer experience) → topbar → heading/action → single-row filter card (search, date lookup, status select, reset, calendar/list toggle) → 1.68fr calendar card / .9fr selected-date panel → prototype note.
- Calendar: Monday-first seven columns, leading/ending empty cells, 89px desktop day cells, status dots/counts, blocked chips, month arrows, selected 12 November state.
- Default side panel: `SELECTED DATE`, title `12 November 2026`, count, then individual booking cards. Detail replaces that panel only after selection. List is the alternate table state.
- Source interactions: search, date/status filters, reset, calendar/list toggle, month navigation, day selection, booking selection/detail/back, New Booking command. Runtime replacements must return an explicit fixture/non-authoritative result; no prototype storage/write/authority.

## Availability

- Ordered regions: same sidebar/topbar/title/action shell → two equal cards for working hours and blocked dates → transactional-server prototype note.
- Controls: start/end time, buffer selection, save availability; date input, block-date action, removable blocked-date chips. Fixture controls report a non-authoritative result and do not mutate.

## Responsive / assets

- Binding admin transforms: 1250px narrowed shell/content; 930px stacked app shell with horizontally scrollable navigation; 485px one-column controls/panels and compact calendar/table; inspect 420px compact calendar rule.
- Visual assets: the source uses its inline SVG icon system only. Local audited inventory is `public/reference-assets/ui-reference/bride-assistant-planner-standalone-v3-content-management-inline-svg-01` through `13`; no raster artwork is visible in T12’s mapped states.
