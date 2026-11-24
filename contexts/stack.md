# Stack Context — Bride Assistant Planner / Ranur Bridal Care

**PA-M2-T07-01 planning source binding:** https://github.com/adityaa11/bride-assistant.git at `ad10fb47284517d0a77bdc39d228764f03b4c630`.

**Context ID:** `BAP-STACK-CONTEXT`  
**Revision:** 1.5
**Date:** 24 November 2026
**Readiness:** `ready`  
**Source:** [Project Context](project.md)  
**Scope:** Technology selections and their rationale only. This is not an implementation design, delivery plan, schema, API specification, or deployment recipe.
**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `15134132805a5dce25ebd88287f96646110530e2` (M2 technical exit baseline).

## Decision frame

Bride Assistant Planner needs one responsive, web-only application for Ranur Bridal Care's public site, customer workspace, and admin workspace. It must replace prototype browser-local data with authoritative, server-protected business facts while preserving the existing HTML as UI reference rather than a production contract. It needs separate customer and admin identity, transactional booking and availability rules, historic package snapshots, optional private payment-proof handling, and published-content control. **[documented: Project Context S1, S2; user-intent: Project Context D2–D4]**

Indonesia/SEA data residency is a user preference. The selected providers place the application on Vercel and authoritative PostgreSQL plus private object storage on Neon; the selected production locations are Singapore. Final account-level recovery, transfer, and data-processing terms still require verification before real customer data is introduced. **[user-intent: 19 Nov 2026; documented: B4–B7]**

## Production-aligned environment principle

Production requirements are authoritative across the project. Versioned policy authority records requirements, decisions, approvals and evidence; environment configuration selects runtime values and infrastructure adapters; boundary validation binds configuration to applicable policy and trusted deployment identity; shared domain behavior consumes only validated runtime configuration and ports. Development/test may use controlled clocks, synthetic data, shorter intervals and disposable adapters without changing business or security semantics. Production rejects development/test overrides and adapters; development/test reject production databases, object stores, credentials and real customer data. Unknown or incompatible combinations fail closed. This project-wide principle does not make T07 the owner of unrelated capabilities. [PA-M2-T07-01](implementation/decisions/PA-M2-T07-01-approved.md).

## Selected baseline

| Capability | Technology or boundary | Status | Project need addressed | Claim state and rationale |
| --- | --- | --- | --- | --- |
| Web application | TypeScript, React, and Next.js | selected | Responsive public, customer, and admin surfaces in one web-only first release | user-intent / inferred. TypeScript and React/Next.js are a coherent web baseline for the requested single responsive application; native mobile is out of scope for the first release. |
| Authentication | Better Auth with persistent relational backing | selected | Separate customer and admin identities, durable sessions, and controlled access | user-intent. Better Auth is the chosen authentication library and requires persistent database support for its core data. **[documented: B1]** |
| Authoritative data | Neon PostgreSQL 18.7 | selected | Booking lifecycle, availability integrity, package snapshots, catalogue, promotions, publication state, and reschedule history | user-intent / documented. Neon is the managed PostgreSQL provider; standard PostgreSQL remains the application data model. |
| Input validation | Zod | selected | Validate untrusted customer/admin form input, request payloads, and document metadata consistently | user-intent. Zod defines runtime input contracts at application boundaries; it is not the database schema or authorization system. |
| Uploaded-byte boundary | `DocumentStore` application interface | selected | Every uploaded media/document byte, including payment proof and portfolio, testimonial, and event images, crosses one storage-vendor-neutral boundary | explicit user-intent, 22 Nov 2026. Its stable byte operations are `put(...)`—returning `storageKey`, SHA-256 hash, size, and media type—and `read(storageKey)`; content-specific replace/remove and record association are lifecycle operations above that byte boundary, not another byte store. |
| Payment-proof and media storage | Neon Object Storage behind `DocumentStore` | selected | Private optional payment proofs and governed website media | user-intent / documented. The production `DocumentStore` adapter uses Neon Object Storage's S3-compatible API. Neon Object Storage supports private buckets and presigned URLs and is selected alongside Neon PostgreSQL rather than Vercel Blob. Public media delivery remains a separately authorized publication path. **[documented: B5, B6]** |
| Application hosting | Vercel | selected | Deploy the Next.js application and its server-side operations | user-intent / documented. Vercel hosts the web application and connects to Neon; it is not the PostgreSQL or production object-storage authority. **[documented: B4]** |
| Authorization boundary | Server-side role and record-ownership enforcement | selected | Customers may access only their own records; admin controls official status, availability, and publication | inferred from Project Context's access and governance rules. A storage key is never authorization by itself. |
| WhatsApp integration | `wa.me` deep-link handoff | selected | Customer/admin communication support without competing transactional state | user-intent. WhatsApp remains outside the official booking record. |

## Explicit exclusions

| Choice | Status | Reason |
| --- | --- | --- |
| Native mobile application in the first release | excluded | The approved first-release boundary is a responsive web-only application. **[user-intent: stack-confirmation]** |
| Payment gateway, automatic invoice/DP flow, and automatic WhatsApp delivery | excluded | These are explicit Project Context non-goals. **[user-intent: Project Context D2]** |
| Browser-local storage as the production system of record | excluded | The existing standalone pages are prototypes and cannot meet shared-record, privacy, or availability requirements. **[documented: Project Context S1, S2]** |
| Raw uploaded document bytes in PostgreSQL | excluded | `DocumentStore` owns immutable bytes; PostgreSQL owns only file metadata and its business linkage. **[user-intent: stack-confirmation]** |
| A separate byte store for content images, browser data URLs, or `public/` upload files | excluded | All uploaded media uses `DocumentStore`; a content image lifecycle facade may associate opaque keys with records but cannot become a second storage authority. **[user-intent: 22 Nov 2026]** |

## Deferred decisions

| Decision | Status | Boundary for later work |
| --- | --- | --- |
| Account-level production assurance | partially verified; remaining decisions deferred | M2-T01/T03 verified the named Singapore topology and deployed database shell. M5-W03 owns account-backed recovery targets and restore ownership; transfer terms and account limits still need review before real customer data. |
| `DocumentStore` adapter details | local contract verified; production adapter deferred | M2-T06 approved the common put/read contract and disposable local adapter. M4-W01 must implement and verify the Neon Object Storage adapter and deletion port; M4-W05/W06 own content association and publication. |
| ORM/database driver and Better Auth adapter | `pg` and Better Auth PostgreSQL integration verified; ORM deferred | M2-T03 established the server PostgreSQL client and M2-T04 connected Better Auth through `pg.Pool` with persisted sessions. No ORM was selected; a later ORM choice requires a separate need and review. |
| Package versions and deployment configuration | M2 runtime lock verified; later deployment checks remain | M2-T02 locked and clean-installed the approved Node 24, Next.js, React, TypeScript, Better Auth, Zod, and `pg` baseline. M2-T03 verified a deployed database shell; later changed deployments and provider adapters require their own scoped checks. |
| File validation, malware scanning, retention/deletion, backups, and recovery | technical proof foundation verified; production controls deferred | M2-T07 approved profile validation and the shared proof lifecycle, retention, hold, scanner-outcome, and environment-isolation contract using disposable data. M4 must prove live integration and obtain four D-03 policy/control decisions; M5-W03 owns the fifth recovery decision and restore evidence before M5-W05 activation. Project Context `U4` remains open for production use. |
| Email, analytics, observability, notifications, and calendar UI library | deferred | No project-level operational or notification requirement yet establishes a baseline choice. |

These statuses reconcile the [M2 exit](implementation/milestones/M2-foundation.md#m2-exit--24-november-2026) with the approved [runtime lock](../feedback/M2-T02-runtime-lock-ccf351b-ck.md), [deployed data shell](../feedback/M2-T03-data-shell-d520679-ck.md), [persistent auth](../feedback/M2-T04-persistent-auth-af6409c-ck.md), [byte contracts](../feedback/M2-T06-adapter-contracts-d806c93-ck.md), and [proof-policy foundation](../feedback/M2-T07-proof-policy-1513413-ck.md). Their technical approval does not resolve D-03 production authority.

## Implementation simulation

This is a capability simulation, not an implementation plan.

| Selected choice | Future implementation would need it for | Conceptual role | Evidence to validate | Limitation or replacement point |
| --- | --- | --- | --- | --- |
| TypeScript + React + Next.js | Render the public website and responsive customer/admin workspaces | One web delivery surface, with server-side capability available for protected operations | Responsive behavior and parity with the approved UI reference | Native mobile remains a later product decision. |
| Better Auth + PostgreSQL | Persist customer/admin identities and support protected sessions | Authentication data sits alongside a relational application data estate | Separate sign-in, session handling, and role-bound access work against the selected database | M2 verified Better Auth persistence through the `pg` PostgreSQL client; domain data integration remains for M3/M4. |
| PostgreSQL | Preserve one official booking record, status transitions, blocked dates, snapshots, and history | Transactional system of record for relational business facts | Concurrent booking/approval tests preserve the lifecycle and availability invariant | Schema and concurrency implementation details remain for Implementation Context. |
| Zod | Reject malformed or incomplete form input, requests, and document metadata before business or storage processing | Runtime validation boundary shared where appropriate between web surfaces and server operations | Valid and invalid payload tests cover booking, admin, status, promotion, and document-metadata inputs | Zod does not replace database constraints, authorization, file-byte validation, or malware scanning. |
| `DocumentStore` | Accept and later retrieve all original uploaded media and payment-proof bytes | Stable two-operation byte boundary: `put` returns safe metadata and `read` uses opaque keys; content record lifecycle composes above it | Hash, size, media type, immutability, and authorization tests; storage implementation does not leak into feature logic | The disposable local adapter and shared proof lifecycle are verified; the Neon production adapter and live policy controls remain for M4/M5. |
| Neon Object Storage | Hold private document bytes and public media without placing bytes in relational records | S3-compatible implementation behind `DocumentStore`, with private buckets and short-lived presigned access after application authorization | Provisioned Singapore region, private-bucket access, signed upload/download, and backup/recovery evidence | File-policy and account-level recovery terms remain pre-go-live gates. |
| Server-side authorization | Enforce customer ownership and admin authority before state or byte access | The application validates role and record ownership; opaque storage keys are not permissions | Cross-customer denial tests and admin workflow tests | Detailed policy model is an Implementation Context decision. |

## Trade-offs and risks

- A single web stack reduces first-release complexity but makes responsive usability the primary mobile experience; no native-device capability is assumed.
- Better Auth keeps authentication under application control but requires deliberate database integration and upgrades.
- PostgreSQL adds relational operations compared with a document-only data model, but is selected for transactional booking, historic integrity, and availability rules.
- Zod standardizes runtime input validation but must be paired with database constraints, server-side authorization, and file-byte safety controls.
- `DocumentStore` makes storage backends replaceable, but its minimal interface does not itself establish access control, retention, malware safety, or regional compliance.
- Singapore selection is not a completed compliance assessment. Storing payment proofs introduces sensitive-data obligations; Project Context `U4` remains a pre-go-live gate.
- Neon Object Storage uses S3-compatible conventions, but the app must issue presigned URLs only after server-side ownership/admin checks. Vercel Blob is excluded from the production byte path to avoid duplicate sensitive copies.

## Compatibility notes and evidence

- Better Auth documents persistent database support for users, sessions, accounts, and verification data, and lists PostgreSQL among supported relational options. **[documented: B1]**
- Vercel documents external PostgreSQL integrations, including Neon, rather than a current first-party Vercel Postgres offering. **[documented: B4]**
- Neon documents S3-compatible Object Storage, private buckets, and presigned upload/download patterns; its Free plan currently includes 5 GB per project. **[documented: B5–B7]**
- The current UI files use standalone HTML/CSS/JavaScript and browser-local storage; they are visual/product evidence only. **[documented: Project Context S1, S2]**

| ID | Source inspected | Supports |
| --- | --- | --- |
| B1 | [Better Auth — Database](https://better-auth.com/docs/concepts/database) and [Other Relational Databases](https://better-auth.com/docs/adapters/other-relational-databases) | Persistent auth records and relational/PostgreSQL compatibility direction. |
| B4 | [Vercel Postgres](https://vercel.com/docs/postgres) | Vercel connects external PostgreSQL providers, including Neon. |
| B5 | [Neon Object Storage](https://neon.com/blog/building-neon-object-storage) | S3-compatible storage and private/public bucket controls. |
| B6 | [Neon private-object example](https://neon.com/blog/building-a-private-searchable-photo-library-on-the-neon-backend) | Private bucket and short-lived presigned URL pattern. |
| B7 | [Neon pricing](https://neon.com/pricing) | Current Free-plan Object Storage allowance. |

## Handoff boundary

Implementation Context may select deployment configuration, database access tooling, adapters, interfaces, schema, authorization policy details, file-validation rules, operational controls, and verification methods within this stack boundary. It must use Vercel for application delivery and Neon for PostgreSQL plus **all production uploaded bytes through `DocumentStore` backed by Neon Object Storage's S3-compatible API**, while preserving the Project Context booking lifecycle, official-admin authority, customer data isolation, booking snapshot rule, and the separation between PostgreSQL metadata and immutable document bytes.

It must not introduce a payment gateway, automated WhatsApp transaction flow, native first-release app, or a second source of truth for bookings without an explicit Project Context review.
