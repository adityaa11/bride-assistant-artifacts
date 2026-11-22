# M1 — Fixture-backed UI assembly

**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `387dde9aae69fe37795935b798e324e0fa315e3d`.

**Status:** `in-progress` · **Advances:** discovery, separate-workspace clarity, complete workflow review, responsive quality.
**Outcome:** A navigable, responsive UI renders approved public/customer/admin scenarios against typed fixtures; it is explicitly non-authoritative. Without it, product behavior cannot be reviewed safely before backend choices.

**Entry:** Project Context v3.1, Stack Context v1.3, and HTML references remain available. **Excludes:** real auth, production persistence, payments, production storage, or changed business rules. T18-A2 alone permits disposable, ignored, fixture-local Portfolio/Testimonials/Events upload bytes behind the local `DocumentStore` byte boundary; this is no public/customer media delivery or production data. **Handoff:** reviewed UI contracts and fixture/backend seams to M2. **Risks:** fixture behavior becoming product policy; contain by labeling controls non-authoritative and keeping contracts typed/versioned.

## Candidate work packages

| ID | Bounded outcome / scope; exclusions | Dependencies | Interfaces and current-state transition | Evidence / handoff | Fixture → backend replacement / open trigger |
| --- | --- | --- | --- | --- | --- |
| M1-W01 | Workspace/app boundaries and shared checks; excludes deployment/provider selection. | — | Replaces no live boundary; creates workspace, app and shared-package seam. | Root checks run; successors can use package conventions. | N/A; decide route groups vs packages only if contract sharing cannot be reviewed. |
| M1-W02 | Versioned UI read-model/command-result contracts and scenario catalogue; excludes business authority. | M1-W01 `sequence` | Consumes reference fields; creates typed adapter boundary, replaces implicit `localStorage` shapes. | Contract compile/check; M1-W04–06 consume named scenarios. | Creates fixture scenario catalogue → M2-W04; invariant: visible states retain compatible contracts. |
| M1-W03 | Shared tokens, responsive layout, form/status/a11y primitives, and exact UI Reference reproduction; excludes screen-specific business rules. | M1-W01 `sequence`; M1-W02 `parallel-safe` | Creates shared UI seam; reproduces the supplied rendered visual result without importing prototype logic. | Reference-viewport desktop/mobile comparison and component-state review; handoff to M1-W04–06. | N/A. |
| M1-W04 | Public discovery/packages/promotions/published-content screens with fixtures; excludes public data backend. | M1-W02/M1-W03 `sequence` | Consumes public read contract; preserves published-vs-draft distinction. | Review public scenarios; handoff to M3-W03/M4-W06. | M1-W04 → public read scenario → M3-W03 then M4-W06; invariant: drafts never render publicly. |
| M1-W05 | Customer sign-in placeholder, home, booking, Booking Saya/help/WhatsApp-unavailable scenarios; excludes real sessions/submission. | M1-W02/M1-W03 `sequence` | Consumes customer contracts; replaces prototype local account/booking display. | Every canonical status has scenario; handoff to M2-W02, M3-W05, M4-W02–04. | M1-W05 → customer booking/proof/exception scenarios → M3-W05 and M4-W02–04; invariant: UI does not claim an official transition. |
| M1-W06 | Admin sign-in placeholder, catalogue, booking/availability/customer/content/promotion flows; excludes admin authority. | M1-W02/M1-W03 `sequence` | Preserves operational visual cues; retires `Confirmed`/`On Hold` semantics. | Review canonical labels/draft states; handoff to M3-W02/06, M4-W05. | M1-W06 → admin command/read scenarios → M3-W02/M3-W06/M4-W05; invariant: canonical lifecycle remains visible. |
| M1-W07 | Visual/interaction review fixtures, bounded T18-A2 local upload seam for Portfolio/Testimonials/Events, and backend replacement-seam inventory; excludes production acceptance. | M1-W04–06 `sequence` for final review; T18 implementation consumes T02/T03 and inspects the T15 code baseline without requiring T15 CK PASS. | Consumes typed UI contracts; disposable local `DocumentStore` implementation owns uploaded bytes; content lifecycle keeps opaque record associations above the byte interface. | Actual three-form upload/read/replace/cleanup and public-negative route evidence, responsive/source review, then approved seam inventory before M2. | T18/H-18 → M2-W04 contract integration and M4-W05 content/media ownership; M4-W06 alone activates governed public reads. No fixture files or manifests are migrated as production data. |

### Dependency graph

`M1-W01 → M1-W02, M1-W03; M1-W02 + M1-W03 → M1-W04, M1-W05, M1-W06; M1-W04 + M1-W05 + M1-W06 → M1-W07.`  Same-tier nodes are parallel-safe.

### Fixture-replacement map

`M1-W02 → typed scenario catalogue → M2-W04 → UI contracts remain versioned and typed`  
`M1-W04 → public content/package scenarios → M3-W03/M4-W06 → drafts remain absent`  
`M1-W05 → customer booking/proof/exception scenarios → M3-W05/M4-W02–04 → no fixture creates official state`  
`M1-W06 → admin scenarios → M3-W02/M3-W06/M4-W05 → admin-only actions keep canonical statuses`
`M1-W07/T18 → Portfolio, Testimonials and Events fixture media → local DocumentStore byte implementation plus content lifecycle facade → M2-W04/M4-W05; M4-W06 governs later public reads → storage keys never grant access`

**Exit gate:** inspect fixture coverage, mobile/desktop review, three collection-specific upload/read/replace/cleanup paths, empty roots, public/customer negatives, state boundaries, and the `DocumentStore` successor seam. On failure revise M1; do not treat local bytes as a live media system.

