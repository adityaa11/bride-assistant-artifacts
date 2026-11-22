# Implementation Context — Bride Assistant Planner / Ranur Bridal Care

**Context ID:** `BAP-IMPLEMENTATION-CONTEXT` · **Revision:** `2.7` · **Date:** 22 November 2026
**Status:** `ready` · **Project state:** `hybrid` · **Ticket-generation readiness:** `M1 ready; M2–M5 planned behind predecessor evidence and deferred production gates`
**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `387dde9aae69fe37795935b798e324e0fa315e3d`.

## Read first

1. This routing index, then [milestone map](implementation/milestones.md).
2. For the next executable work, read [M1](implementation/milestones/M1-fixture-ui.md), [architecture](implementation/architecture.md), [decisions](implementation/decisions.md), and [verification](implementation/verification.md).
3. Do not generate M2+ tickets until their decision gates and direct predecessor evidence are satisfied.

## Current execution

| Next milestone | Eligible packages | Blocking decisions | Required predecessor evidence |
| --- | --- | --- | --- |
| M1 — Fixture-backed UI | M1-W07 (T18-A2/A3 continuation, then T17 review) | None for fixture-local work | Project Context v3.1 and Stack Context v1.3; approved T02/H-02 and T03/H-03, T18-A2/A3 ticket, and reference HTML. The prior Portfolio-only T18 checkpoint is historical. |
| M2 — Authoritative foundation | None | None | M1 exit evidence; then provision Vercel Singapore, Neon PostgreSQL/Object Storage in Singapore, Docker local services, and exact locked dependencies. |
| M3–M5 | None | D-03/D-04 as applicable | Their direct milestone gates and production-hardening evidence. |

## Target-product baseline

Ranur Bridal Care will receive one responsive web application with public discovery, a customer workspace, and a separate admin workspace. Customers can discover packages, sign in, submit one durable booking request, follow its official status, and optionally hand off payment proof; admin controls catalogue, official decisions, availability, and published content. The shared booking record—not WhatsApp—remains authoritative. Only `Booking Berhasil` blocks a date; snapshots prevent later catalogue changes from rewriting history. Claims are documented or user-intent in [Project Context](project.md) and [Stack Context](stack.md).

The system boundary is Next.js/React/TypeScript, Better Auth, PostgreSQL, Zod, `DocumentStore`, and `wa.me` handoff. Payment gateways, automated WhatsApp, native mobile, and browser-local production state are excluded. Server-enforced role/ownership, transactional availability, private proof bytes, and published-only public content are material quality constraints.

## Repository reconciliation

`hybrid` — the repository now has a Next.js fixture application, typed scenario adapter, review routes, tests, and a Portfolio-only local media seam at the source commit above. The standalone customer/admin HTML still supplies UI reference evidence, not authoritative runtime behavior; its `localStorage`, seed records, blank WhatsApp configuration, and prototype statuses are not production contracts. T18-A2 adds fixture-local Testimonials/Events media before T17 review. Real authentication, authoritative relational data, and the production `DocumentStore` adapter remain for later milestones.

## Registries

### Milestones

| ID | Status | Detail | Direct dependencies | Read when |
| --- | --- | --- | --- | --- |
| M1 | in-progress | [Fixture UI](implementation/milestones/M1-fixture-ui.md) | — | Continuing T18-A2 or reviewing M1 fixtures/contracts. |
| M2 | planned | [Foundation](implementation/milestones/M2-foundation.md) | M1 | Selecting provider/auth/data seams. |
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

## Change routing

- Package or interface change: update its milestone record, package graph, fixture-replacement map, and this registry.
- Decision change: update decisions and architecture first; mark dependent milestones `revisiting` or context `stale` where boundaries change.
- Project/Stack Context change: reassess target baseline and affected milestones before generating dependent tickets.
- Evidence that a fixture/backend contract diverges: update its M1 fixture-replacement row and successor backend package together.

## Readiness

`ready`: The end product is coherently planned and M1 is traceable and ticket-ready. Vercel application hosting plus Neon PostgreSQL/Object Storage and the development profile are confirmed; M2 follows M1’s exit gate. Proof formats, limits, retention rollout, authorization, audit, incident containment, and private-object delivery are confirmed. The payment-proof capability remains enabled in the product plan; deferred provider/legal/recovery choices are explicit production-hardening seams to resolve before release. The official WhatsApp number remains intentionally deferred and leaves the live handoff disabled.

