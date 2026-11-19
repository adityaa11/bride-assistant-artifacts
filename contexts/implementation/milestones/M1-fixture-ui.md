# M1 — Fixture-backed UI assembly

**Status:** `planned` · **Advances:** discovery, separate-workspace clarity, complete workflow review, responsive quality.  
**Outcome:** A navigable, responsive UI renders approved public/customer/admin scenarios against typed fixtures; it is explicitly non-authoritative. Without it, product behavior cannot be reviewed safely before backend choices.

**Entry:** Project/Stack contexts and HTML references remain available. **Excludes:** real auth, persistence, payments, production storage, or changed business rules. **Handoff:** reviewed UI contracts and fixture/backend seams to M2. **Risks:** fixture behavior becoming product policy; contain by labeling controls non-authoritative and keeping contracts typed/versioned.

## Candidate work packages

| ID | Bounded outcome / scope; exclusions | Dependencies | Interfaces and current-state transition | Evidence / handoff | Fixture → backend replacement / open trigger |
| --- | --- | --- | --- | --- | --- |
| M1-W01 | Workspace/app boundaries and shared checks; excludes deployment/provider selection. | — | Replaces no live boundary; creates workspace, app and shared-package seam. | Root checks run; successors can use package conventions. | N/A; decide route groups vs packages only if contract sharing cannot be reviewed. |
| M1-W02 | Versioned UI read-model/command-result contracts and scenario catalogue; excludes business authority. | M1-W01 `sequence` | Consumes reference fields; creates typed adapter boundary, replaces implicit `localStorage` shapes. | Contract compile/check; M1-W04–06 consume named scenarios. | Creates fixture scenario catalogue → M2-W04; invariant: visible states retain compatible contracts. |
| M1-W03 | Shared soft-rose tokens, responsive layout, form/status/a11y primitives; excludes screen-specific rules. | M1-W01 `sequence`; M1-W02 `parallel-safe` | Creates shared UI seam; preserves visual direction, not prototype logic. | Desktop/mobile and component state review; handoff to M1-W04–06. | N/A. |
| M1-W04 | Public discovery/packages/promotions/published-content screens with fixtures; excludes public data backend. | M1-W02/M1-W03 `sequence` | Consumes public read contract; preserves published-vs-draft distinction. | Review public scenarios; handoff to M3-W03/M4-W06. | M1-W04 → public read scenario → M3-W03 then M4-W06; invariant: drafts never render publicly. |
| M1-W05 | Customer sign-in placeholder, home, booking, Booking Saya/help/WhatsApp-unavailable scenarios; excludes real sessions/submission. | M1-W02/M1-W03 `sequence` | Consumes customer contracts; replaces prototype local account/booking display. | Every canonical status has scenario; handoff to M2-W02, M3-W05, M4-W02–04. | M1-W05 → customer booking/proof/exception scenarios → M3-W05 and M4-W02–04; invariant: UI does not claim an official transition. |
| M1-W06 | Admin sign-in placeholder, catalogue, booking/availability/customer/content/promotion flows; excludes admin authority. | M1-W02/M1-W03 `sequence` | Preserves operational visual cues; retires `Confirmed`/`On Hold` semantics. | Review canonical labels/draft states; handoff to M3-W02/06, M4-W05. | M1-W06 → admin command/read scenarios → M3-W02/M3-W06/M4-W05; invariant: canonical lifecycle remains visible. |
| M1-W07 | Visual/interaction review fixtures and backend replacement-seam inventory; excludes production acceptance. | M1-W04–06 `sequence` | Consumes all UI contracts; records successor adapters. | Approved review record and no direct feature fixture imports; M2 may begin. | Audits all above replacements; unresolved seam blocks the respective M2+ ticket. |

### Dependency graph

`M1-W01 → M1-W02, M1-W03; M1-W02 + M1-W03 → M1-W04, M1-W05, M1-W06; M1-W04 + M1-W05 + M1-W06 → M1-W07.`  Same-tier nodes are parallel-safe.

### Fixture-replacement map

`M1-W02 → typed scenario catalogue → M2-W04 → UI contracts remain versioned and typed`  
`M1-W04 → public content/package scenarios → M3-W03/M4-W06 → drafts remain absent`  
`M1-W05 → customer booking/proof/exception scenarios → M3-W05/M4-W02–04 → no fixture creates official state`  
`M1-W06 → admin scenarios → M3-W02/M3-W06/M4-W05 → admin-only actions keep canonical statuses`

**Exit gate:** inspect fixture coverage, mobile/desktop review, state boundaries, and seam inventory. On failure revise M1; do not treat the result as a live booking system.

