# Payment-proof and media policy template

**PA-M2-T07-01 planning source binding:** https://github.com/adityaa11/bride-assistant.git at `ad10fb47284517d0a77bdc39d228764f03b4c630`.

**Status:** `technical policy confirmed — production decisions and activation pending`
**Purpose:** Provide the policy shape for private payment proof and customer-facing media. It permits JPEG, PNG, and PDF proof files; production upload remains gated on the unresolved safeguards below.

## 1. Scope and data classification

- **In scope:** payment-proof files and metadata; portfolio/testimonial media and publication-permission evidence.
- **Classification:** `private customer evidence` for payment proof; `publishable only after authorization` for media.
- **System boundary:** local Docker development adapter; production Neon Object Storage in the selected region; PostgreSQL stores only metadata and business linkage.
- **Development-data rule:** local adapters/volumes, controlled clocks and synthetic data are disposable. Validation rejects production databases, object stores, credentials or real customer data in development/test, and local adapters or shortened durations in production.

## 2. Processing purpose, basis, and notice

| Required decision | Policy value | Evidence / review trigger |
| --- | --- | --- |
| Purpose | Confirm payment and resolve a booking issue. | Must be necessary for the booking workflow. |
| Processing basis / consent notice | `deferred: select a PDP basis and final plain-language notice` | The notice must state purpose, types of data, eight-month retention, access controls, and the request channel before release. |
| Data-subject request channel | `deferred: official monitored business contact` | Define contact route for access, correction, deletion, restriction, and consent withdrawal requests. |
| Publication permission | Explicit per-item opt-in record, timestamp, media scope, and withdrawal path. | No testimonial or media item may publish without this record. |

## 3. Intake and validation

| Control | Required policy value |
| --- | --- |
| Eligible uploader | Booking owner only while `Menunggu Pembayaran`; admin upload is not allowed. |
| Allowed formats and maximum size | JPEG, PNG, and PDF only; maximum 10 MiB each and three files per payment request. SVG, archives, office files, executables, and video are rejected. |
| Validation | Verify extension allowlist, detected type/signature, size, filename normalization, and authenticated/authorized request; never trust `Content-Type` alone. Generate an opaque storage key; do not preserve the supplied filename. |
| Malware handling | `deferred: scanner/service selection` is mandatory; define clean, infected, unavailable, timeout, and false-positive outcomes. Quarantine/reject on scanner failure until an explicit exception process exists. |

## 4. Access, storage, and delivery

- Store original bytes only in the private `DocumentStore`; PostgreSQL keeps opaque key, hash, size, type, timestamps, owner/booking linkage, and state.
- Require server-side ownership/admin authorization for every metadata read and byte request. Do not make storage keys public permissions.
- Use the selected region; document production store name, region, encryption/backup terms, and the approved download mechanism.
- Record a successful/denied upload, scan result, review decision, authorized download, deletion, retention hold, and status change without logging file contents, supplied filenames, signed URLs, or secrets.

### Booking and payment-proof audit trail

- Maintain an append-only, time-ordered booking history. Each proof event references its booking and proof metadata record; it must never embed the document bytes.
- Record: `PAYMENT_PROOF_UPLOADED`, `PAYMENT_PROOF_UPLOAD_DENIED`, `PAYMENT_PROOF_SCAN_COMPLETED`, `PAYMENT_PROOF_QUARANTINED`, `PAYMENT_PROOF_REVIEWED`, `PAYMENT_PROOF_REJECTED`, `PAYMENT_PROOF_ACCESS_GRANTED`, `PAYMENT_PROOF_DELETED`, and `BOOKING_STATUS_CHANGED`.
- Each event contains the booking/proof identifier, acting account and role, timestamp, event type, outcome/reason code, and request/correlation identifier. Keep file hash, detected type, and size only on proof metadata where needed for integrity and review.
- A proof review is evidence for an admin decision but never changes booking status on its own; `Booking Berhasil` still requires a separate admin `BOOKING_STATUS_CHANGED` action.

## 5. Retention, deletion, and recovery

| Required decision | Policy value | Verification evidence |
| --- | --- | --- |
| Retention verification | Exercise the same lifecycle behavior with a five-minute disposable development profile, then test actual production durations and start events under a controlled clock. Five minutes is never production retention. | Current-revision byte/metadata/audit/history observations and controlled-clock boundary tests pass. |
| Active retention | Eight calendar months from final proof review, or upload when never reviewed, only for eligible operational proof. Applicable Indonesian statutory records retain at least ten years under an approved classification/start rule. Unknown applicability blocks production deletion. | Controlled-clock class/start-event tests plus approved legal/accounting disposition and live M4 evidence. |
| Legal/business hold | `deferred: accountable criteria, authority, release and review rules`; active hold always prevents deletion in every profile. | Synthetic hold cases at T07; approved criteria and live audit at M4-W07. |
| Deletion/sanitization | Delete the object and its proof-specific metadata/audit events, write `PAYMENT_PROOF_DELETED`, and document provider lifecycle/backup expiry. Do not delete the booking's status history. | Deletion record and recovery boundary are documented. |
| Backup/recovery | `deferred: Neon account-backed database/object-storage RPO/RTO, backup terms and restore owner` | M5-W03 verifies provider/account terms and restore without broadening access. |

## Policy authority and runtime configuration

Versioned policy authority contains requirements, accountable decisions, dates and evidence; it is not a runtime settings file. Environment profiles contain runtime values and adapter selection. Validation checks those values against the applicable policy and trusted deployment identity before producing runtime configuration for the shared lifecycle behavior. Domain logic does not read approval documents or branch on environment names. Missing production decisions, unknown classification and incompatible adapters keep production proof capabilities disabled. T07 owns the technical contract and local verification; M4-W07 owns approved PDP/scanner/hold/statutory treatment, M5-W03 owns recovery proof, and M5-W05 authorizes activation. The five original D-03 decision obligations remain open until evidenced.

## 6. Incident, exceptions, and review

- Immediately contain suspected access, malware, wrong-recipient, or deletion/recovery failures; revoke active access, preserve audit evidence, and assess Indonesia PDP notification obligations, including the 3 × 24-hour failure-notification rule. The incident contact is the deferred official monitored business contact.
- Exceptions require documented criteria and expiry; no permanent bypass of validation, authorization, or retention rules.
- Review this policy before production payment-proof release, after any storage/provider change, after a security incident, and every 12 months.

## Basis for this template

OWASP requires authorization, allowlisted file types, type/size validation, generated names, and storage away from the web server. NIST media-protection guidance organizes policy around access, storage, transport, sanitization, use, and disposal. Indonesia’s PDP Law recognizes access, deletion/destruction, consent withdrawal, processing-limitation rights, and failure notification; apply it with qualified local legal advice before go-live. Applicable Indonesian tax rules may require source records to be kept ten years in Indonesia; obtain accounting/legal confirmation before setting production retention.

