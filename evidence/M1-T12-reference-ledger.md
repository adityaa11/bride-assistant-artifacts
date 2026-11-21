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

## Source-to-component map and deliberate boundaries

| Source region / rule group | Converted component and stylesheet | Interaction / fixture field |
| --- | --- | --- |
| `.app`, `.sidebar`, `.brand`, `.navigation`, `.nav`, `.sidebar-bottom` | `AdminBookingWorkspace` sidebar in `shared/admin-booking-workspace.tsx`; matching rules in `shared/admin-booking-workspace.module.css` | Bookings and Availability are route links; later-ticket destinations remain fixture-only anchors. |
| `.topbar`, `.section-title`, `.eyebrow`, `.btn` | `AdminBookingWorkspace` topbar and heading | Source copy and icon roles; actions announce a non-authoritative result. |
| `.filters`, `.field-search`, `.date-lookup`, `.select`, `.viewtoggle` | Booking controls in `AdminBookingWorkspace` | Customer search, date lookup, canonical status filter, reset and calendar/list toggle use React state; source `Requested`/`Confirmed`/`On Hold` labels are replaced by T02 canonical status labels. |
| `.main-grid`, `.card-pad`, `.calendar-grid`, `.calendar-day`, `.legend` | `Calendar` | Monday-first cells, leading and trailing blanks, selected day, blocked dates, dots and counts read typed `adminBooking` fixture; month arrows and day selection update view state. |
| `.listtable`, `.table-wrap`, `.table-info` | `List` | Filtered booking rows open the detail panel via keyboard-accessible buttons. |
| `.side-head`, `.side-inner`, `.booking-item`, `.detail-section` | `SideList` and `Detail` | Selected-date list and source-order detail sections read typed snapshot fields; back button and apparent status action remain local/non-authoritative. |
| `.availability-grid`, `.form-row`, `.form-field`, `.blocked-list` | `Availability` | Time, buffer and blocked-date controls preserve source geometry; save/block/unblock report no mutation. |
| `#i-*` icon symbols | `Icon` reads `public/reference-assets/ui-reference/bride-assistant-planner-standalone-v3-content-management-inline-svg-01-e88f0a139761.svg` | Audited source symbols only, with decorative `aria-hidden`. Other extracted SVG files are not visible in the mapped states. |

Omitted prototype behavior: inline scripts, `innerHTML`, browser storage, seed mutation, customer availability decisions, admin status transitions, production identity and external links. The source's final status command row is represented by a non-authoritative review action; no direct transition is implied.

## Rendered comparison

- Captures: `evidence/M1-T12/{reference,implementation}-{bookings,availability}-{1440,1251,1250,931,930,721,720,486,485,420}.png`. Each pair uses the same CSS viewport width and 960px viewport height, with full-page capture. The 1250px, 930px, 720px and 485px boundaries are captured on both sides; 420px checks the compact calendar rule.
- Desktop 1440px: source and implementation have the same 238px sidebar, 31px content inset, 72px filter bar, booking grid start at y=313px, calendar inner grid start at y=407px, 89px day cells and 117px selected-date header. Sidebar, topbar, title, action, cards, icons and palette were compared side by side. The initial 10px filter offset from the global button minimum was corrected.
- At 930px, both stack the sidebar above content and retain a two-column booking grid. At 720px, both change to one column. At 485px, both use a single search row, date/status/reset row, full-width toggle and compact calendar. Long canonical status names wrap in the legend and badge; all source geometry and controls remain readable with no horizontal document overflow at any captured width.
- `reference-list.png` / `implementation-list.png` and `reference-detail.png` / `implementation-detail.png` show the alternate table and selected customer sections. `implementation-empty-filter.png`, `implementation-empty-scenario.png`, `implementation-status-filter.png`, `implementation-focus.png`, `implementation-non-authoritative-result.png`, and `implementation-availability-result.png` show the ticket-specific fixture states.
- Intentional visible differences: canonical Indonesian statuses and their longer wrapping; non-authoritative status action. The source prototype's `Completed` and `On Hold` are not copied into runtime. The user-directed removal of two annotations is recorded in A4 below.

## User-directed annotation removal (A4)

The user selected the Bookings `Fixture note:` block and the shared `Non-authoritative fixture` footer in the live browser and requested both be removed. Commit `f47e21187159d73d0a1fee4d9804af8d4930f04d` removes those rendered nodes and their unused styles; the typed fixture, `data-scenario-id`, and `role="status"` interaction result remain. The `Availability` source note remains because it was not selected.

Fresh paired screenshots at 1440, 1251/1250, 931/930, 721/720, 486/485 and 420px are under `evidence/M1-T12-r2/`. The Playwright check asserts the two removed blocks are absent, preserves the matching shell/panel/calendar geometry within 2px, and passes interaction checks. The original comparison above records the prior checkpoint; A4 supersedes its Bookings note/footer observation.

## Interaction, accessibility and security observation

Playwright exercised list/detail/back, customer search with no result, reset, canonical status filter, calendar/list toggle, month navigation, date selection and New Booking result. Availability Save produced a status message without changing the fixture. All captured widths had `document.documentElement.scrollWidth <= innerWidth`. The search input has an accessible name and visible solid focus outline. The changed runtime files have no `innerHTML`, inline handler/script, `localStorage`, or executable reference code. Source and implementation use the extracted local icon symbol sheet; no external media origin was added.
