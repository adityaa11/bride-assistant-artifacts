# M1-T15 A6 local fixture image comparison

- Code repository: `https://github.com/adityaa11/bride-assistant.git`.
- Code commit: `2a1493413b083a93585b3af43250967a7f2e1c01`.
- User direction: M1-T15-A6 in the ticket. The original [M1-T15 comparison](../M1-T15/comparison.md) is historical evidence for commit `2e4673e`; it is not modified.
- Harness: `npm run check` PASS (lint, TypeScript, 13 tests, production build); `$env:T15_EVIDENCE_DIR='.atlas/evidence/M1-T15-images'; node scripts/t15-visual-evidence.mjs` against production `npm run start -- -p 3315` PASS, exit 0.

## Image result and source fit

`app-portfolio-populated.png` shows two real local images at the mapped reference `.web-media` region, with the source 1.65 aspect ratio and `object-fit: cover`. `shell-content-485.png` confirms one-column mobile cards with both crops visible and no horizontal overflow. Both are clearly fictional object-only still lifes: the published card shows flowers and blank stationery; the draft card shows a dress and flowers. The SVG image placeholder is used only if no portfolio record exists; the source's empty state remains unchanged. The positive browser assertion waits for two images to finish loading, confirms nonzero intrinsic width and nonempty alt text, and confirms both resolved URLs point to `fixture-assets/m1-t15` through Next's local image optimizer.

The two 1610×977 PNG files are `public/fixture-assets/m1-t15/garden-celebration.png` and `public/fixture-assets/m1-t15/bridal-preparation.png`. The built-in image generation tool created them from the prompts recorded in the code ledger. T02 carries only closed `imageAsset` keys and `imageAlt`; `ContentCard` maps keys to fixed local paths. No user input can select an origin or media URL, and no file upload or public image delivery was introduced.

## Regression comparison

The harness regenerated 33 source/app empty-panel pairs at 1440, 1251/1250, 1151/1150, 931/930, 721/720, and 486/485 CSS px for Portfolio, Testimonials, and Events. `measurements.json` records the bounds and no-overflow result. Empty panel heights still match the source at 1440px; narrow vertical offsets remain the A4 shared-drawer exception. Populated Testimonials and Events, form, proposed-result, error, focus, mobile drawer, and public-negative captures are included. The public route still excludes all draft fixture titles; proposed Publish still leaves the Draft badge unchanged. Bookings and Availability shared shell captures at 1440, 930, 720, and 485px show no regression.

Source scan: `rg -n 'https?://|localStorage|sessionStorage|dangerouslySetInnerHTML|innerHTML|fetch\(' shared/admin-content-workspace.tsx 'app/(admin)/admin/content' 'app/(admin)/admin/events'` — no matches. Images use React/Next safe rendering, local allowlisted paths, and descriptive alt text. No material content-specific visual mismatch remains within the A6 exception.
