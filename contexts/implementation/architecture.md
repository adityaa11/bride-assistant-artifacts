# Architecture Overview

## Material boundaries

| Boundary | Decision / claim | Consequence |
| --- | --- | --- |
| Delivery | One responsive Next.js web application; public, customer, and admin route/surface boundaries. `documented` | No native app or separate transactional system. |
| Authority | PostgreSQL is the future authoritative store; server-side roles and record ownership guard every protected command/read. `documented`/`inferred` | Browser state, route shape, and storage keys are never authority. |
| Booking | One booking aggregate holds submitted package/promotion snapshot and status history. `user-intent` | Catalogue edits affect future selection only; status commands validate actor/current state. |
| Availability | `Booking Berhasil` and approved reschedule are transactional conflict points. `user-intent` | Customer reads expose availability, never another customer’s facts. |
| Bytes | `DocumentStore` owns immutable bytes; PostgreSQL holds metadata/linkage. `selected` | Private proof access is authorized before `read`; public media uses a separately governed path. |
| Deployment / development | Vercel hosts the application; Docker runs local PostgreSQL 18.7 and a disposable local `DocumentStore` adapter. `confirmed` | No development byte or database state is production data; local volumes are disposable and excluded from source control. |
| Production bytes | Neon Object Storage in Singapore backs `DocumentStore`. `confirmed` | A private S3-compatible bucket plus server-side authorization and short-lived presigned access supplies the production byte boundary. |
| Production data | Neon PostgreSQL 18.7 in Singapore backs authoritative relational facts. `confirmed` | Vercel server-side work uses the Neon connection path; preview deployments may use isolated Neon database branches. |
| UI transition | Typed UI-facing contracts/adapters isolate M1 fixtures from future backend read models. `proposed` | Feature components do not import fixture objects directly. |
| Runtime baseline | Node.js 24.22.3, Next.js 16.4.6, React/React DOM 19.3.1, TypeScript 5.9.3, Better Auth 1.7.26, Zod 4.6.7, `pg` 8.23.1, `@types/node` 24.6.6, and PostgreSQL 18.7. `confirmed` | This is the approved starting baseline. Lockfiles pin exact tested patches; Node/PostgreSQL receive security-patch updates within their approved majors; the managed provider must offer PostgreSQL 18.7. |

## Transition posture

The standalone HTML is replaced, not migrated as data. Preserve its Soft Rose Atelier visual direction and useful screen/field cues where compatible. Retire `localStorage`, seed credentials/data, blank WhatsApp constants, and noncanonical statuses. No live data migration is currently evidenced; before any real prototype data import, create a separate migration decision and rehearsal gate.

## Cross-cutting principles

- Zod validates untrusted inputs; database constraints/transactions and authorization enforce durable rules.
- Fixture scenarios are disposable review/test data, never production seeds or policy sources.
- Only authorized published content is publicly queryable; proof bytes and drafts have no public URL path.
- Driver/ORM, observability tool, and file-policy values remain decisions, not implied design.
- Vercel functions use Singapore (`sin1`) where accepted; Neon PostgreSQL and Object Storage use the selected Singapore project location. Vercel is not Jakarta-only hosting.

