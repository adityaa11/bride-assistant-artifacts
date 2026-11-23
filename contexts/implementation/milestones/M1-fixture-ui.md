# M1 — Fixture-backed UI assembly

**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `79b3478995737dd73165afaba50369dcb5242a08`.

**Status:** `complete` (exit recorded 23 November 2026) · **Advances:** discovery, separate-workspace clarity, complete workflow review, responsive quality.
**Outcome:** A navigable, responsive UI renders approved public/customer/admin scenarios against typed fixtures; it is explicitly non-authoritative. Without it, product behavior cannot be reviewed safely before backend choices.

**Entry:** Project Context v3.2, Stack Context v1.3, and HTML references remain available. **Excludes:** real auth, production persistence, payments, production storage, or business-rule execution. T18-A2 alone permits disposable, ignored, fixture-local Portfolio/Testimonials/Events upload bytes behind the local `DocumentStore` byte boundary; this is no public/customer media delivery or production data. **Handoff:** reviewed UI contracts and fixture/backend seams to M1.1 audit, then M2. **Risks:** fixture behavior becoming product policy; contain by labeling controls non-authoritative and keeping contracts typed/versioned.

## Candidate work packages

| ID | Bounded outcome / scope; exclusions | Dependencies | Interfaces and current-state transition | Evidence / handoff | Fixture → backend replacement / open trigger |
| --- | --- | --- | --- | --- | --- |
| M1-W01 | Workspace/app boundaries and shared checks; excludes deployment/provider selection. | — | Replaces no live boundary; creates workspace, app and shared-package seam. | Root checks run; successors can use package conventions. | N/A; decide route groups vs packages only if contract sharing cannot be reviewed. |
| M1-W02 | Versioned UI read-model/command-result contracts and scenario catalogue; excludes business authority. | M1-W01 `sequence` | Consumes reference fields; creates typed adapter boundary, replaces implicit `localStorage` shapes. | Contract compile/check; M1-W04–06 consume named scenarios. | Creates fixture scenario catalogue → M2-W04; invariant: visible states retain compatible contracts. |
| M1-W03 | Shared tokens, responsive layout, form/status/a11y primitives, and exact UI Reference reproduction; excludes screen-specific business rules. | M1-W01 `sequence`; M1-W02 `parallel-safe` | Creates shared UI seam; reproduces the supplied rendered visual result without importing prototype logic. | Reference-viewport desktop/mobile comparison and component-state review; handoff to M1-W04–06. | N/A. |
| M1-W04 | Public discovery/packages/promotions/published-content screens with fixtures; excludes public data backend. | M1-W02/M1-W03 `sequence` | Consumes public read contract; preserves published-vs-draft distinction. | Review public scenarios; handoff to M3-W03/M4-W06. | M1-W04 → public read scenario → M3-W03 then M4-W06; invariant: drafts never render publicly. |
| M1-W05 | Customer sign-in placeholder, home, booking, Booking Saya/help/WhatsApp-unavailable scenarios; excludes real sessions/submission. | M1-W02/M1-W03 `sequence` | Consumes customer contracts; replaces prototype local account/booking display. | Every canonical status has scenario; handoff to M2-W02, M3-W05, M4-W02–04. | M1-W05 → customer booking/proof/exception scenarios → M3-W05 and M4-W02–04; invariant: UI does not claim an official transition. |
| M1-W06 | Admin sign-in placeholder, catalogue, booking/availability/customer/content/promotion flows; excludes admin authority. | M1-W02/M1-W03 `sequence` | Preserves operational visual cues; retires `Confirmed`/`On Hold` semantics. | Review canonical labels/draft states; handoff to M3-W02/06, M4-W05. | M1-W06 → admin command/read scenarios → M3-W02/M3-W06/M4-W05; invariant: canonical lifecycle remains visible. |
| M1-W07 | Visual/interaction review fixtures, bounded T18-A2 local upload seam for Portfolio/Testimonials/Events, and backend replacement-seam inventory; excludes production acceptance. | M1-W04–06 `sequence` for final review; T18 implementation consumes T02/T03 and inspects the T15 code baseline without requiring T15 CK PASS. T17-A7 re-review additionally consumes CK PASS for T19/H-19, T20/H-20 and T21/H-21. | Consumes typed UI contracts; disposable local `DocumentStore` implementation owns uploaded bytes; content lifecycle keeps opaque record associations above the byte interface. H-19 fixture relationships remain named seams, not M1 data authority. | Actual three-form upload/read/replace/cleanup and public-negative route evidence, responsive/source review, H-19 finding-by-finding disposition and independent F-01/F-02 recheck, then approved seam inventory before M2. | T18/H-18 → M2-W04 contract integration and M4-W05 content/media ownership; M4-W06 alone activates governed public reads. H-19 FRA-01/02/04 successor mapping below. No fixture files or manifests are migrated as production data. |

### Dependency graph

`M1-W01 → M1-W02, M1-W03; M1-W02 + M1-W03 → M1-W04, M1-W05, M1-W06; M1-W04 + M1-W05 + M1-W06 → M1-W07.`  Same-tier nodes are parallel-safe.

### Fixture-replacement map

`M1-W02 → typed scenario catalogue → M2-W04 → UI contracts remain versioned and typed`  
`M1-W04 → public content/package scenarios → M3-W03/M4-W06 → drafts remain absent`  
`M1-W05 → customer booking/proof/exception scenarios → M3-W05/M4-W02–04 → no fixture creates official state`  
`M1-W06 → admin scenarios → M3-W02/M3-W06/M4-W05 → admin-only actions keep canonical statuses`
`M1-W05/M1-W06 → separate customer/admin availability fixtures → M3-W07 → one server-owned date decision protects admin closures and Booking Berhasil without a pending-booking hold or customer identity disclosure`
`M1-W07/T18 → Portfolio, Testimonials and Events fixture media → local DocumentStore byte implementation plus content lifecycle facade → M2-W04/M4-W05; M4-W06 governs later public reads → storage keys never grant access`

### T17-A7 H-19 planning dispositions — 23 November 2026

User direction accepts M2+ deferral of the three previously unknown H-19 findings under Project Context v3.2. This is a milestone handoff decision, not an M1 fixture repair or a claim that future backend behavior is already proven. [H-19's source audit](../../../evidence/M1-T19-fixture-relationship-audit-2ee952c.md#t17-a7-consumption-ledger) remains historical evidence; T17-A7 must independently confirm the present seam and preserve its named successor at the inspected code commit.

| H-19 finding | Accepted M1 disposition and authority | Named successor and preserved seam |
| --- | --- | --- |
| `FRA-01` | **Accepted M2+ deferral.** M1-W04/W05/W06 and T10 explicitly render typed, non-authoritative package scenarios; T10 excludes a booking snapshot. Project Context v3.2 requires shared catalogue facts and an immutable submitted snapshot in the end product, not a fixture-origin bridge in M1. No M1 canonical-ID, price or composition claim is made. | M3-W01/02 establish catalogue ownership, M3-W03 public/customer projections, M3-W04/05 capture the package/promotion snapshot. Preserve the typed package display/read seam and keep historical snapshots independent of later catalogue edits. |
| `FRA-02` | **Accepted M2+ deferral.** M1-W05/W06 exclude real session/submission/admin authority, so `BR-*` and `BA-*` are separate display fixtures rather than a claim of one operational record. Project Context v3.2 requires one booking identity and owner/admin views in the end product. | M2-W02/03 establish identity and server ownership; M3-W04/05/06 establish one booking record with owner-safe customer and full admin projections. Preserve distinct customer/admin read boundaries; never infer identity from display name or phone. |
| `FRA-04` | **Accepted M2+ deferral.** Project Context v3.2 D7 explicitly permits deliberate admin closure without a booking. M1 customer/admin date scenarios are independent, non-authoritative review states and do not assert a derived operational projection or temporary pending-booking hold. | M3-W07 replaces both date fixture views with one transactional availability decision: deliberate admin closure or `Booking Berhasil` makes the date unavailable, while pending bookings alone do not. Preserve customer date-only privacy and admin-only closure authority; prove cross-surface projection and concurrency there. |

`FRA-03` remains the M1-required browser-store repair closed by T20/H-20 CK PASS and carried through T21/H-21; T17-A7 still independently rechecks F-01. `FRA-05/06` retain H-19's accepted M2+ deferrals to M4-W05/06, and T17 F-02 still requires independent selected-date/toast evidence. These dispositions remove the planning unknown only; any failed current M1 route, security, visual, media or regression row still blocks H-17.

**Exit gate:** inspect fixture coverage, mobile/desktop review, three collection-specific upload/read/replace/cleanup paths, empty roots, public/customer negatives, state boundaries, H-19 finding dispositions, independent F-01/F-02 checks, and the `DocumentStore` successor seam. On failure revise M1; do not treat local bytes or independent fixture dates as a live media/availability system.

### M1 exit decision — 23 November 2026

**Result:** `PASS` for the fixture-backed UI milestone at code commit `79b3478995737dd73165afaba50369dcb5242a08`. The active M1 ticket registry is approved; T05–T07 remain superseded historical tickets. The final [T17-A7 checkpoint](../../../checkpoints/M1-T17-review-seam-inventory-79b3478.md) and independent [CK review](../../../feedback/M1-T17-review-seam-inventory-79b3478-ck.md) prove all nine RC-27 rows and report no blocking finding. T15's final media continuation and expanded T18 media handoff each have CK `PASS`; T19/H-19 and T20/H-20 and T21/H-21 also have CK `PASS` and were consumed by T17-A7. The earlier failed T17 v1 review remains historical.

The exit evidence includes the versioned route/scenario and successor-seam inventory, 110/110 actual-route viewport observations, 42/42 browser cases, the three collection-specific media paths and public/customer negatives, the `DocumentStore` substitution, and independent closure of F-01/F-02. H-19 `FRA-03` is repaired within M1; `FRA-01/02/04/05/06` have accepted M2+ dispositions and named successors. [Cross-phase verification](../verification.md) retains the boundary: this exit establishes a reviewed, non-authoritative fixture UI and does not accept production authentication, booking, availability, persistence, or public media delivery.

**Handoff:** M1.1 audits this exact M1 baseline for architecture and code-efficiency findings before any finding-backed refactor tickets; M2 execution follows the M1.1 exit gate. No code change or new runtime claim is part of this status decision.

