# Implementation Context — Bride Assistant Planner / Ranur Bridal Care

**Context ID:** `BAP-IMPLEMENTATION-CONTEXT` · **Revision:** `3.0` · **Date:** 23 November 2026
**Status:** `ready` · **Project state:** `hybrid` · **Ticket-generation readiness:** `M1.1 exit PASS with no refactor; M2 planning may begin behind its own gates; M3–M5 remain planned`
**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `79b3478995737dd73165afaba50369dcb5242a08`.

## Read first

1. This routing index, then [milestone map](implementation/milestones.md).
2. For the next executable work, read [M2](implementation/milestones/M2-foundation.md), the [M1.1 exit](implementation/milestones/M1.1-architecture-audit-refactor.md#w02-reconciliation-and-m11-exit--23-november-2026), [architecture](implementation/architecture.md), [decisions](implementation/decisions.md), and [verification](implementation/verification.md).
3. M1.1 has no refactor ticket after its [W02 decision](../evidence/M1.1-W02-disposition-79b3478.md). Generate M2+ tickets only when their decision gates and direct predecessor evidence are satisfied.

## Current execution

| Next milestone | Eligible packages | Blocking decisions | Required predecessor evidence |
| --- | --- | --- | --- |
| M1.1 — Architecture audit and bounded refactor | Complete; W03 skipped after W02 no-refactor decision | None | T01 CK PASS and [W02/exit record](implementation/milestones/M1.1-architecture-audit-refactor.md#w02-reconciliation-and-m11-exit--23-november-2026) at `79b3478`. |
| M2 — Authoritative foundation | Planning/ticket creation may begin; execution follows M2 entry gates | None for planning | M1 and M1.1 exit evidence; then provision Vercel Singapore, Neon PostgreSQL/Object Storage in Singapore, Docker local services, and exact locked dependencies. |
| M3–M5 | None | D-03/D-04 as applicable | Their direct milestone gates and production-hardening evidence. |

## Target-product baseline

Ranur Bridal Care will receive one responsive web application with public discovery, a customer workspace, and a separate admin workspace. Customers can discover packages, sign in, submit one durable booking request, follow its official status, and optionally hand off payment proof; admin controls catalogue, official decisions, availability, and published content. The shared booking record—not WhatsApp—remains authoritative. A date is unavailable through deliberate admin closure or a `Booking Berhasil` booking; pending bookings alone do not reserve dates. Snapshots prevent later catalogue changes from rewriting history. Claims are documented or user-intent in [Project Context](project.md) and [Stack Context](stack.md).

The system boundary is Next.js/React/TypeScript, Better Auth, PostgreSQL, Zod, `DocumentStore`, and `wa.me` handoff. Payment gateways, automated WhatsApp, native mobile, and browser-local production state are excluded. Server-enforced role/ownership, transactional availability, private proof bytes, and published-only public content are material quality constraints.

## Repository reconciliation

`hybrid` — the repository has a Next.js fixture application, typed scenario adapter, review routes, tests, and approved fixture-local Portfolio, Testimonials and Events media paths at the source commit above. The standalone customer/admin HTML still supplies UI reference evidence, not authoritative runtime behavior; its `localStorage`, seed records, blank WhatsApp configuration, and prototype statuses are not production contracts. T19 audited the fixture relationships; T20/T21 repaired the browser booking write and stale test selectors. Real authentication, authoritative relational data, and the production `DocumentStore` adapter remain for later milestones.

## Registries

### Milestones

| ID | Status | Detail | Direct dependencies | Read when |
| --- | --- | --- | --- | --- |
| M1 | complete | [Fixture UI](implementation/milestones/M1-fixture-ui.md) | — | Reviewed fixture/UI baseline and exit evidence. |
| M1.1 | complete | [Architecture audit and bounded refactor](implementation/milestones/M1.1-architecture-audit-refactor.md) | M1 | T01 audit approved; W02 no-refactor decision; conditional W03 skipped; exit PASS at `79b3478`. |
| M2 | planned | [Foundation](implementation/milestones/M2-foundation.md) | M1.1 | Selecting provider/auth/data seams after M1.1 exit. |
| M3 | planned | [Booking core](implementation/milestones/M3-booking-core.md) | M2 | Building catalogue, booking, or availability. |
| M4 | planned | [Evidence & publishing](implementation/milestones/M4-evidence-publishing.md) | M3; D-03/D-04 | Building proof, exceptions, or published content. |
| M5 | planned | [Release readiness](implementation/milestones/M5-release.md) | M3, M4 | Preparing a release. |

### Decisions

| ID | Status | Detail | Affects |
| --- | --- | --- | --- |
| D-01 | superseded | [Decision register](implementation/decisions.md#d-01-vercel-application-and-documentstore-platform) | Historical; replaced by D-06 |
| D-02 | confirmed | [Decision register](implementation/decisions.md#d-02-runtime-and-auth-data-integration) | M2–M5 / M2-R02 |
| D-03 | deferred production hardening | [Decision register](implementation/decisions.md#d-03-sensitive-document-policy) | M4 proof capability stays enabled; final provider/legal/recovery seams await M2-R03 |
| D-04 | deferred | [Decision register](implementation/decisions.md#d-04-official-whatsapp-handoff) | M3–M4; [.env.example](../../.env.example) |
| D-05 | confirmed | [Decision register](implementation/decisions.md#d-05-production-postgresql-provider-and-location) | M2–M5 / M2-R01 |
| D-06 | confirmed | [Decision register](implementation/decisions.md#d-06-production-documentstore-provider-and-location) | M2–M5 / M2-R01, M4-W01 |
| D-C05 | confirmed | [Decision register](implementation/decisions.md#confirmed-constraints) | M1-W07, M3-W07 / admin closure and successful-booking availability |

## Change routing

- Package or interface change: update its milestone record, package graph, fixture-replacement map, and this registry.
- Decision change: update decisions and architecture first; mark dependent milestones `revisiting` or context `stale` where boundaries change.
- Project/Stack Context change: reassess target baseline and affected milestones before generating dependent tickets.
- Evidence that a fixture/backend contract diverges: update its M1 fixture-replacement row and successor backend package together.
- M1.1 audit finding: record evidence and disposition before creating a refactor ticket; refresh affected context decisions if a candidate crosses an accepted boundary.

## Readiness

`ready`: The end product is coherently planned. M1's fixture/UI exit is `PASS` at `79b3478`, backed by T17-A7 CK `PASS`; M1-W07 records accepted deferrals for H-19 FRA-01/02/04, and M3-W07 owns the later admin-closure and successful-booking availability relation. M1.1-T01's audit is approved and W02 records no justified refactor; M1.1 exits `PASS` on the unchanged code baseline. Vercel application hosting plus Neon PostgreSQL/Object Storage and the development profile are confirmed; M2 planning can now consume the M1.1 handoff, with its own entry and verification gates still required. Proof formats, limits, retention rollout, authorization, audit, incident containment, and private-object delivery are confirmed. The payment-proof capability remains enabled in the product plan; deferred provider/legal/recovery choices are explicit production-hardening seams to resolve before release. The official WhatsApp number remains intentionally deferred and leaves the live handoff disabled.

