# M1-T14 rendered comparison and interaction evidence

- Code repository: `https://github.com/adityaa11/bride-assistant.git`; code commit: `15b6b9c68fa74e0502a0acced13fca120d7185d8`.
- Source: `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html`, Customers panel. Implementation: `/admin/customers` and `/admin/customers/{empty,error}`.
- Capture harness: `node scripts/t14-visual-evidence.mjs` against `npm run start -- -p 3314`, production build; PASS, exit 0. Browser contexts use 900px height, CSS viewport widths 1440, 1251/1250, 931/930, 486/485, and reduced motion.
- Files `source-directory-N.png` / `app-directory-N.png` and `source-detail-N.png` / `app-detail-N.png` pair every width. `measurements.json` records main bounds, six card counts, and no horizontal overflow at every width.

## Visual comparison

The ordered route content matches the source: heading/action, two-column alphabetized customer grid, individual cards with initial/name/booking count/phone/chevron, MVP hint; selected detail has name/back action, booking-history heading/count, one booking row with date/package/status/time/location. Desktop main region starts at exactly `(285.5, 97.5)` at 1440px in both captures. Customer card width, gap, padding, palette, type roles, icon roles, and wrap match. At 485px the customer grid becomes one column, with no clipped names or contact text. The final detail card geometry was corrected by matching the source heading line height; final `app-detail-1440.png` and `source-detail-1440.png` show the aligned card and row.

Expected differences under frozen authority:

- `AdminWorkspaceFrame` owns the desktop collapse control and narrow right-side drawer; the source prototype uses a different narrow sidebar. A4 explicitly supersedes that shell treatment. At 930/485 the main-region vertical offsets therefore differ, while the content panel geometry follows the reference. The source prototype footer/hint is not copied into the shared shell.
- The ticket requires search/no-result. The Customers source has no search row, so the control is assembled from the same reference's booking search input and filter CSS; it adds 56px to the directory content height. `app-search-match.png` and `app-search-no-result.png` show the required derived states.
- The source prototype labels the selected Aulia booking `Requested`; the T02 canonical fixture value is `Menunggu Konfirmasi`. The badge retains source geometry and color role but displays the T02 field, as required by the ticket's lifecycle and fixture boundary. The source's customer-creation hint remains visible as reference copy, but the action only navigates to the read-only fixture booking workspace.

No other material content mismatch was observed after the final capture. No local photograph or embedded image is used in this panel. The sole visible local asset is the admin SVG symbol sprite `public/reference-assets/ui-reference/bride-assistant-planner-standalone-v3-content-management-inline-svg-01-e88f0a139761.svg` (plus/search/arrow/chevron/navigation); it is listed in the extraction manifest. No asset extraction rerun was needed because the source assets did not change.

## State, keyboard, and boundary checks

- `app-empty.png`: clean customer scenario has zero records, a visible empty message, and no cards. `app-error.png`: error scenario has zero records, a visible alert, and no cards. `app-search-match.png` shows Nabila only; `app-search-no-result.png` shows no cards and the empty status. No data is generated or persisted by these reads.
- `app-keyboard-focus.png` shows initial skip-link focus. The harness focuses Aulia's card, presses Enter, sees Booking history, clicks the booking row, and verifies `/admin/bookings/detail?booking=BA-005` renders Aulia's booking. `app-mobile-drawer.png` shows the shared drawer; Escape closes it. Native search has an accessible name and focus style.
- `shell-{bookings,availability,customers}-{1440,930,720,485}.png` show the shared frame after the Customers navigation destination change. The harness asserts a Customers destination and no horizontal overflow for every route/width. The drawer and reduced-motion contexts are checked without route CSS overrides of shell selectors.
- `npm run check`: PASS, 13 Node tests, lint, typecheck, production build. The new boundary test verifies the projection runs in the admin server route and that public/customer routes and the client customer component do not import the customer model. `rg` source scan of the new route and component for `dangerouslySetInnerHTML|innerHTML|localStorage|sessionStorage|eval\(|new Function|https?://` returned no matches. React text rendering and fixed local links are used; no prototype script or external origin runs.
