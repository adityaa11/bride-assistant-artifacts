# M1-T15 rendered reference comparison

- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Code commit: `2e4673e3e26e31099ef9b1a4b18bb7dfe671b4eb`.
- Source: `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` in that code commit.
- Harness: `node scripts/t15-visual-evidence.mjs` against `npm run start -- -p 3315`, production build. Result: PASS, exit 0.
- Fixture and source map: `docs/frontend/M1-T15_REFERENCE_LEDGER.md` in that code commit.

## Paired captures and measured comparison

The harness captured source and application for **Portfolio**, **Testimonials**, and **Events** at 1440, 1251, 1250, 1151, 1150, 931, 930, 721, 720, 486, and 485 CSS px (33 pairs). Names are `source-{portfolio|testimonials|events}-{width}.png` and `app-{portfolio|testimonials|events}-{width}.png`. `measurements.json` records bounding boxes and overflow for every pair.

At 1440px, all three panels' main content begins at the same x coordinate as the source, and empty panel height differs by 0px. Portfolio and Testimonials empty panels begin at the same y coordinate; Events begins 4px below the source because its toolbar line box differs slightly. Ordered title, toolbar, metrics, empty panel, and hint regions, type roles, labels, palette, icons, controls, and content widths match the source. At 930px and below, the application begins content 69–125px above the source because the source adds a horizontal navigation strip while the A4 shared frame uses a mobile drawer. This is the ticket-authorized shell exception. Content grids, metric reflow, toolbar stacking, tab width, text wrapping, and no horizontal overflow are confirmed at all boundaries. No material content-specific mismatch remains.

The source initializes all three collections empty. Its CSS and render functions define the populated cards and editor. The app's populated cards use fictional T02 fixtures authorized by the user, shown in `app-portfolio-populated.png`, `app-testimonials-populated.png`, and `app-events-populated.png`. They preserve the source card/media/meta/action structure and text-plus-color Draft/Published badges. No photo bytes or media origin are present; the source's placeholder image icon and event symbol fill those slots.

`Export JSON website` is source-labeled but disabled because M1 forbids export/public delivery. The source's photo upload and HTTPS URL controls are omitted because M1 forbids media bytes and external origin. The app adds the required non-authoritative provenance below the source hint. These are scope-bound deviations, not visual replacements.

## State and interaction evidence

- `app-event-form.png`: source-derived event fields in a native keyboard dialog; Escape closes it. Portfolio and Testimonials use their source field groups with no media controls.
- `app-proposed-result.png`: Publish opens the proposed action dialog; submit shows the named T02 `admin.content-proposed-result` message. The original Draft badge remains and no record changes.
- `app-error.png`: named error fixture shows an alert with zero records. The 33 paired app captures use named empty fixture routes; no initialization or hidden data appears on repeated reads.
- `app-keyboard-focus.png`: skip link receives first Tab and visible focus. Tabs, buttons, dialog, and mobile drawer are keyboard operable; `app-mobile-drawer.png` records its open state and the harness confirms Escape closes it.
- `app-public-negative.png`: the public route lacks the fixture-only draft titles (`An intimate bridal morning`, `A thoughtful morning`, `Bridal Open House`, `Draft consultation`). This is a cross-route negative assertion, not a client publication flag.
- `shell-{bookings|availability|content|events}-{1440|930|720|485}.png`: shared frame captures and no-overflow assertions. Navigation links to Content and Events exist on every route; Bookings and Availability retain their shell.
- All browser contexts use reduced motion. The source's `prefers-reduced-motion` handling and the app's no-transition rule for ticket controls were checked.

## Asset and execution inventory

The only visual asset used by these panels is the already extracted local SVG sprite `public/reference-assets/ui-reference/bride-assistant-planner-standalone-v3-content-management-inline-svg-01-e88f0a139761.svg`. Its manifest entry points to the exact mapped HTML. The source initial state contains no bitmap image. Event theme `✦` is source text, and fixture image slots use the source image icon. No source assets changed, so extraction was not rerun.

Source scan command: `rg -n 'localStorage|sessionStorage|dangerouslySetInnerHTML|innerHTML|fetch\(|https?://|<script|on[a-z]+=' shared/admin-content-workspace.tsx shared/admin-content-workspace.module.css 'app/(admin)/admin/content' 'app/(admin)/admin/events'` — no matches. The app uses React text rendering and local static SVG references; no raw reference page/script, inline handler, unsafe sink, browser storage, external origin, upload, consent decision, or publication write is on the route. `npm run check` PASS: lint, TypeScript, 13 repository tests, production build. `git diff --cached --check` PASS before code commit.
