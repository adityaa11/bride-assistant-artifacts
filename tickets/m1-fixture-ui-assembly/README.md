# M1 Fixture-backed UI assembly — resized ticket set

- Set ID: `BAP-M1-FIXTURE-UI-SET-01`; status: `ready`; artifact revision: `2.0`.
- Authority: Project Context rev 3.0; Stack Context rev 1.2; M1 fixture-ui milestone; architecture, decisions, and verification records; `project's goal/UI Reference/` as the binding visual source of truth.
- Revision reason: original T05/T06 crossed unrelated screen, information, and review boundaries. They are retained as `superseded`; no ticket ID is reused.
- Artifact inventory remains unchanged: only this set's files are staged; pre-existing `checkpoints/`, `evidence/`, `feedback/`, and `tickets/.gitkeep` are untouched.

## Frozen M1 boundary

All active tickets render named typed fixtures only. No real account/session, server authorization, persistence, upload, payment, official transition, public query, or live WhatsApp link exists in M1. Adapter/model version and an explicit non-authoritative result are required at every interactive surface. Prototype localStorage, static seed scripts, `Confirmed`, and `On Hold` are not carried forward.

## Active registry and sized batches

| Batch | Tickets | Relation |
| --- | --- | --- |
| 1 | T01 workspace | sequence |
| 2 | T02 scenario adapter; T03 visual system | both need T01; parallel-safe |
| 3 | T04 public; T08 registration/sign-in; T09 customer home/help; T10 booking request; T11 Booking Saya; T12 admin booking/availability; T13 admin catalogue/packages; T14 admin customers; T15 admin content publication; T16 admin promotions | each needs T02 + T03; parallel-safe by surface |
| 4 | T17 review and seam inventory | requires every active batch-3 ticket |

Superseded: T05 customer bundle → T08–T11; T06 admin bundle → T12–T16; T07 review → T17.

## Handoff and expected-use graph

`T01 → H-01 → T02 + T03`<br>
`T02/H-02 + T03/H-03 → T04, T08–T16`<br>
`T04/H-04 + T08/H-08 + T09/H-09 + T10/H-10 + T11/H-11 + T12/H-12 + T13/H-13 + T14/H-14 + T15/H-15 + T16/H-16 → T17/H-17 → M2 planning`.

Each surface EUP is: named review route → scenario adapter (`scenarioId`, version) → its local composition plus T03 primitives → explicit fixture UI state → reviewer capture. T17 EUP is review matrix → evidence/findings → successor seam map. Runtime activation is limited to those fixture routes.

## Reuse and readiness decisions

- `RD-01 extract-shared`: T03 owns visual tokens, shell, form/status/a11y primitives; all surfaces consume presentation-only props.
- `RD-02 extract-shared`: T02 owns scenario lookup/read and command-result contract; all surfaces consume it and no feature imports raw fixture objects.
- `RD-03 keep-local`: public, customer, and admin navigation retain different information/authority meaning; only shell geometry is shared.
- `RD-04 keep-local`: customer availability is availability-only; admin booking/availability uses operational records. A generic calendar model would erase the privacy boundary.

Security readiness for all UI tickets is `applicable`: `SEC-M1-01` preserve the future server-side identity/role/ownership seam; `SEC-M1-02` implement safe framework rendering, no HTML sinks/inline execution/unreviewed origin/client CSP authority (ASVS browser/input-output; OWASP 2025 A01/A03/A05/A08; NIST AC/SC/SI/SR). No ticket has an unresolved planning-security decision.

UI readiness: State 1 / Build New UI. T03 creates the Soft Rose Atelier project visual contract. All surface tickets use only its tokens and relevant patterns—public discovery, customer workflow/knowledge list, admin data workspace/settings form/entity library—and pass relevant VIS-001–014 review rows, keyboard, focus, state, long-content, responsive, and reduced-motion evidence.

## Binding UI Reference requirement

`project's goal/UI Reference/` is a mandatory visual specification for M1. Every screen represented there must be reproduced with the same visible structure, hierarchy, copy, typography, colors, spacing, geometry, imagery/icon treatment, navigation, and responsive composition. A screen is not complete when it is merely inspired by the reference or when it passes functional tests with a different visual design.

Reference HTML/CSS/JS and prototype storage must not be imported or copied. The implementation must reproduce the rendered result through the project UI system. States not explicitly shown in the reference must extend the same referenced shell, component geometry, tokens, and interaction treatment; they must not introduce a new visual language or arbitrary per-screen styling. Each surface ticket must include reference-viewport captures and a visual comparison record; any visible mismatch fails the ticket and M1 review.

## Amendment BAP-M1-UI-A1 — mapped implementation and asset extraction

Explicit user authorization (2026-11-20) binds every active M1 UI ticket to
the exact reference-page mapping and implementation rules in
`docs/frontend/M1_UI_REFERENCE_IMPLEMENTATION_CONTRACT.md` at source commit
`e98a804eeca02efb65363339c4f7d050bd3d861a`. This amendment requires an
independent reusable Next.js refactor wired through T02 fixtures, exact
rendered fidelity for each mapped HTML reference, extraction and local
inventory of its static image/SVG assets, and extraction of its actual
responsive transformations. It adds no authority or reference behaviour:
reference HTML/CSS/JS, localStorage, seed data, events, and external origins
remain forbidden.

## Finalization result

Each active ticket has one coherent review surface/contract boundary, a direct route-to-visible-state proof, and a named handoff. T08–T16 may run in parallel only after T02/T03. T17 is the next eligible ticket only after their captures and seam records exist. T01 remains the next eligible ticket now.
