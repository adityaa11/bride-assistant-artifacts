# Project Context — Bride Assistant Planner / Ranur Bridal Care

**Context ID:** `BAP-PROJECT-CONTEXT`  
**Revision:** 3.2
**Date:** 23 November 2026
**Readiness:** `ready`  
**Scope:** Product definition and business boundary only; this is not a stack, architecture, delivery plan, backlog, or API specification.
**Source code repository / commit:** `https://github.com/adityaa11/bride-assistant.git` / `79b3478995737dd73165afaba50369dcb5242a08` (source baseline; admin date-closure rule is user direction).

## Project frame

Bride Assistant Planner is the operational system for **Ranur Bridal Care**, a single personal Bride Assistant business. Its customer-facing experience carries the Ranur Bridal Care brand; its administrative workspace supports the same business. This project is not a multi-tenant product for independent Bride Assistants and is not a full Wedding Organizer system. **[user-intent: D1]**

The product should make booking a personal bridal companion understandable and traceable for customers, while giving Ranur administrators one operational record for catalogues, booking requests, decisions, payment evidence, availability, and published website content. It must not promise availability or payment confirmation before an administrator has made the relevant decision. **[user-intent: D2; documented: S1, S2]**

## Objectives and intended outcomes

The project succeeds when:

- Visitors can discover Ranur's services and choose a package without creating an account.
- Customers can register, sign in, submit a complete booking request, follow its official status, and use an appropriate payment-proof handoff.
- Administrators can manage package/pricing information, incoming requests, booking decisions, availability, and customer-facing content from their workspace.
- A booking is a single business record shared between customer and admin surfaces; WhatsApp is communication support, not a separate source of truth.
- A date is unavailable to other customers when an admin deliberately closes it or when a booking for that date reaches **Booking Berhasil**. A pending booking alone does not reserve the date. **[user-intent: D2, D7]**
- Published website content, package information, and promotional information originate from authorized admin management rather than unmanaged prototype data. **[user-intent: D2; inferred: I1]**

No numeric growth, conversion, SLA, or service-volume target has been provided. These are intentionally deferred and do not block the initial product definition. **[unknown: U1, non-blocking]**

## Scope and non-goals

### In scope

**Public website:** Ranur's public brand and service information, including About, portfolio/testimonials, events, packages, how-it-works, FAQ, promotions, and a booking call to action. Only authorized, published content may appear publicly.

**Customer workspace:** customer registration and sign-in; personal home; new booking; Booking Saya; account-scoped status and details; optional upload of payment proof while awaiting payment; and a WhatsApp handoff to Ranur.

**Admin workspace:** separate admin authentication and post-login workspace; CRUD management of services/actions, bundles, master packages, package prices, booking requests, availability, customers, portfolio/testimonials, events, promotions, and publication state. The existing `Bride_Assistant_Planner_Standalone_v3_Content_Management.html` is the UI reference for this workspace. **[user-intent: D3; documented: S2]**

**Booking history:** the package selected at submission must be retained as a snapshot of the relevant name, price, duration, description, and composition. Later catalogue changes must not rewrite an existing booking. **[documented: S2; user-intent: D2]**

### Explicit non-goals

- A Wedding Organizer system, wedding-planning platform, vendor marketplace, or full vendor scheduler.
- Multi-business or multi-tenant Bride Assistant support.
- A payment gateway, automatic invoice/DP workflow, or automatic WhatsApp message delivery.
- Internal live chat, unrestricted website editing, and unrelated event/budget/guest management.

Customers may choose to send payment proof directly through WhatsApp; this does not create a payment-gateway requirement. **[user-intent: D2]**

## People, access, and governance

| Role | Product responsibility and boundary |
| --- | --- |
| Visitor | May browse published Ranur content and packages but cannot submit a booking. |
| Customer | Registers and signs in to the customer workspace; owns and may view only their own bookings, status, and payment-proof action. |
| Admin | Signs in separately to the admin workspace. Is the accountable decision authority for package/pricing changes, booking rules, booking status decisions, availability decisions, and publication of website content. |

The same person may initially perform all admin responsibilities. Any later division of admin duties or approval workflow requires a context review. **[user-intent: D2]**

Customer and admin authentication are separate and each redirects to its corresponding workspace. Customer data, including contact information, wedding dates, locations, booking details, and payment proof, must not be exposed to other customers. **[user-intent: D2; inferred: I2]**

## Booking lifecycle and availability policy

The canonical customer flow is:

1. The customer creates an account and signs in.
2. The customer starts a booking, selects a date and package, and provides booking details.
3. On submission, the system creates one booking record with status **Menunggu Konfirmasi** and prepares a pre-made WhatsApp message to Ranur's admin contact.
4. If WhatsApp is unavailable on the customer side, the submitted record remains visible and actionable in the admin workspace.
5. The customer and admin discuss availability and relevant details.
6. Once both agree, the admin changes the status to **Menunggu Pembayaran**.
7. While in **Menunggu Pembayaran**, the customer may optionally upload **Bukti Pembayaran** in Booking Saya, or send it to admin through WhatsApp.
8. After the admin checks and approves payment proof, the admin changes the status to **Booking Berhasil**.
9. Once a booking is **Booking Berhasil**, its date is no longer available for another customer to select.

Separately from booking status, an admin may deliberately make a date unavailable even when no booking exists for it. A booking in **Menunggu Konfirmasi** or **Menunggu Pembayaran** does not, by itself, make its date unavailable. **[user-intent: D2, D7]**

### Rejection, cancellation, and rescheduling

An admin may reject a request while it is **Menunggu Konfirmasi**. The specific rejection reason and any customer-facing rejection-status label remain to be defined. **[user-intent: D4; unknown: U2, non-blocking]**

Only a customer whose booking is **Booking Berhasil** may request cancellation or rescheduling through Booking Saya:

- **Cancellation:** the customer submits a Pembatalan Booking form with **Alasan**. The booking becomes **Menunggu Pembatalan Booking**; after admin approval, it becomes **Dibatalkan**.
- **Reschedule:** the customer submits a Reschedule Booking form with **Tanggal Wedding**, **Lokasi Wedding**, and **Alasan**. The booking becomes **Menunggu Reschedule Booking**. After admin approval, it remains **Booking Berhasil**, records a reschedule-history entry, updates its wedding date and location, and releases the prior booking's reservation of its date. A separate admin closure, if present, still makes that date unavailable.

Admin can access and change all booking statuses from the admin workspace. Customer-request forms are deliberately limited to **Booking Berhasil**. **[user-intent: D4]**

**Canonical statuses:** `Menunggu Konfirmasi` → `Menunggu Pembayaran` → `Booking Berhasil`, with the exception states `Menunggu Pembatalan Booking` → `Dibatalkan` and `Menunggu Reschedule Booking` → `Booking Berhasil`. **[user-intent: D2, D4]**

The system must distinguish a submitted request from a confirmed booking. Availability surfaced to customers must never disclose another customer's identity or booking details. A date may be unavailable because an admin deliberately closed it or because a booking is **Booking Berhasil**. Pending bookings do not reserve dates; any future temporary-hold policy while awaiting payment must be approved as a new business rule. **[user-intent: D2, D7]**

## Domain rules and invariants

- Services/Actions are individual catalogue items; Bundles group reusable items; Master Packages combine individual items and/or Bundles. Duplicate items must not be counted twice when a package is expanded. **[documented: S2]**
- Master catalogue changes may affect future selections, but never mutate the snapshot held by an existing booking. **[user-intent: D2]**
- One booking has one cross-surface identity. It must not become separate customer, admin, and WhatsApp transactions. **[inferred: I3]**
- Official status is changed by the admin, is visible to the customer who owns the booking, and is not replaced by WhatsApp conversation state. **[user-intent: D2]**
- Draft or unauthorized portfolio, testimonial, event, and promotional content must not be presented as public Ranur content. Images and testimonials require a valid basis for customer permission before publication. **[documented: S2; inferred: I4]**
- Every media item uploaded **through this application**, including portfolio, testimonial, and event images and uploaded payment-proof bytes, must pass through one governed server-side byte-storage boundary. A payment proof sent separately through WhatsApp remains a communication handoff, not an application upload. Browser-local files, data URLs, public static assets, and relational rows are not production stores for application-uploaded bytes. This does not itself authorize public access or assert permission; publication and access remain governed separately. **[user-intent: D6]**
- Promotions are managed through admin CRUD and may alter the final booking price. Each promotion identifies its applicable packages, discount type (percentage or rupiah amount), discount value, and active period. **[user-intent: D4]**

## Current and target state

### Observed baseline

The customer-side HTML reference consists of standalone pages for public content, registration/sign-in, customer home, booking, Booking Saya, and help. It uses browser-local storage, has no production authentication or backend, and has empty admin WhatsApp configuration. **[documented: S1]**

The standalone admin reference includes booking/availability and catalogue management, content-management modules, browser-local interactions, and content export. It is a product/UI evidence source rather than a production system. **[documented: S2]**

### Target state

The public site, customer workspace, and admin workspace must operate over consistent business facts for customer identity, packages, booking, payment evidence, availability, decisions, and published content. The existing HTML's local storage, seed values, inline media, and prototype login behavior are not production contracts. **[user-intent: D2; inferred: I5]**

The intended visual direction is the customer-facing **Soft Rose Atelier** style: dusty/blush rose and muted terracotta, feminine but clean and professional, responsive on desktop and mobile. The customer portal should retain a clear customer navigation of Beranda, Booking Baru, Booking Saya, and Bantuan; admin-facing internal detail must not bleed into customer pages. **[documented: S1; user-intent: D2]**

## Acceptance basis

At project level, the product is acceptable when it can demonstrate all of the following:

| Outcome | Observable acceptance basis |
| --- | --- |
| Discovery | A visitor can understand Ranur's personal BA offer and choose a package without an account. |
| Separate identities | A customer and an admin authenticate separately and reach their respective workspaces. |
| Booking request | An authenticated customer can submit all required booking information; one record starts as Menunggu Konfirmasi and is available to admin even if WhatsApp cannot be opened. |
| Admin decision | Admin can move an agreed request to Menunggu Pembayaran and only mark it Booking Berhasil after approving payment proof. |
| Payment-proof flexibility | A customer can optionally upload proof in Booking Saya or provide it to admin through WhatsApp. |
| Availability integrity | A date deliberately closed by an admin or attached to Booking Berhasil cannot be selected by another customer. A pending booking alone does not block selection, and customer-facing availability does not reveal other customers' data. |
| Exception handling | An admin can reject Menunggu Konfirmasi; a Booking Berhasil customer can request cancellation or rescheduling with the specified fields, and an approved reschedule preserves history and releases the prior booking's date reservation. |
| Shared record | The booking's latest official status and snapshot are consistent across customer and admin surfaces. |
| Catalogue, promotion, and content control | Admin CRUD governs packages/pricing, active package-specific percentage-or-rupiah promotions, and published web content; draft content is not public. |
| Uploaded-media boundary | Uploaded content images and payment-proof bytes are handled through the governed storage boundary; a file upload alone does not publish content or expose private bytes. |
| Historic integrity | A later package change does not alter the facts on a completed or in-progress booking record. |

## Constraints, risks, and deferred decisions

**Constraints:** customer and admin access are separated; admin is the decision authority; WhatsApp supports communication but not the official transaction state; a date is unavailable through deliberate admin closure or a Booking Berhasil booking, not merely a pending request; payment proof upload is optional; cancellation and rescheduling requests are customer actions only after Booking Berhasil. **[user-intent: D2, D4, D7]**

**Risks to manage:** unapproved media/testimonials, privacy exposure of customer wedding data, misleading availability before final payment approval, and a mismatch between WhatsApp discussion and the official booking record. **[inferred: I2–I4]**

**Deferred decisions:**

- `U1` Quantitative business or service targets after launch.
- `U2` Rejection reason requirements, customer-facing rejection label, payment expiry, and any future temporary date-hold policy.
- `U4` Privacy, consent, retention, deletion, and payment-proof handling policy before go-live.
- `U5` Official admin WhatsApp number and any delegation/handling procedure.

These items are non-blocking for the current Project Context but must be resolved before the applicable capability goes live.

## Evidence and claim ledger

| ID | Source inspected | Supports |
| --- | --- | --- |
| `S1` | `project's goal/UI Reference/{index,masuk,daftar,home,booking,booking_saya,bantuan}.html` and `README.txt` | Customer UI, standalone/local-storage baseline, portal navigation, booking and WhatsApp-reference behavior. |
| `S2` | `project's goal/UI Reference/Bride_Assistant_Planner_Standalone_v3_Content_Management.html` | Admin UI, catalogue hierarchy, availability and content-management evidence. |
| `D1` | User response, 18 Nov 2026 | Ranur-only boundary. |
| `D2` | User response, 18 Nov 2026 | Separate login, admin authority, payment-proof flow, canonical statuses, and Booking Berhasil availability rule. |
| `D3` | User clarification, 18 Nov 2026 | Customer/admin UI-reference boundary. |
| `D4` | User response, 18 Nov 2026 | Rejection, cancellation, rescheduling, promotion, and WhatsApp-number decisions. |
| `D5` | User response, 18 Nov 2026 | Drift-review communication and explicit-update approval rule. |
| `D6` | User direction, 22 Nov 2026 | All uploaded media uses one governed byte-storage boundary; Stack Context selects `DocumentStore` and its production adapter. |
| `D7` | User clarification, 23 Nov 2026 | An admin may deliberately make a date unavailable without a booking; pending bookings alone still do not reserve dates. |

### Drift watchlist

Potential drift occurs if Ranur becomes multi-business, payment becomes automated, a pending-booking date-hold policy is introduced, admin date-closure authority changes, the booking lifecycle changes, or the business expands beyond personal Bride Assistant service.

When potential drift is found, do **not** silently update this Project Context. First explain to the accountable decision owner in simple language: what changed, which recorded decision it conflicts with or extends, and why it matters. Then ask whether to update the context. Update it only after the decision owner explicitly chooses to do so; otherwise record no change to the context. **[user-intent: D5]**
