# Payment-proof and media policy template

**Status:** `retention policy confirmed — production safeguards pending review`  
**Purpose:** Provide the policy shape for private payment proof and customer-facing media. It permits JPEG, PNG, and PDF proof files; production upload remains gated on the unresolved safeguards below.

## 1. Scope and data classification

- **In scope:** payment-proof files and metadata; portfolio/testimonial media and publication-permission evidence.
- **Classification:** `private customer evidence` for payment proof; `publishable only after authorization` for media.
- **System boundary:** local Docker development adapter; production Neon Object Storage in the selected region; PostgreSQL stores only metadata and business linkage.
- **Development-data rule:** the local adapter and volumes are disposable; do not treat development storage as production storage or a production retention system.

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
| Retention verification | Before enabling the active policy, set proof-byte and proof-audit retention to five minutes; prove that expiry deletes the object and proof-specific history while preserving booking status history. | Automated expiry/deletion/audit tests pass. |
| Active retention | Eight months from the final proof-review event, or from upload when no review occurs. | Expiry job/test demonstrates deletion or hold. |
| Legal/business hold | `deferred: conditions and authority` | Hold prevents deletion and is audited. |
| Deletion/sanitization | Delete the object and its proof-specific metadata/audit events, write `PAYMENT_PROOF_DELETED`, and document provider lifecycle/backup expiry. Do not delete the booking's status history. | Deletion record and recovery boundary are documented. |
| Backup/recovery | `deferred: Neon account-backed RPO/RTO and restore responsibility` | Restore rehearsal proves controlled recovery without broadening access. |

## 6. Incident, exceptions, and review

- Immediately contain suspected access, malware, wrong-recipient, or deletion/recovery failures; revoke active access, preserve audit evidence, and assess Indonesia PDP notification obligations, including the 3 × 24-hour failure-notification rule. The incident contact is the deferred official monitored business contact.
- Exceptions require documented criteria and expiry; no permanent bypass of validation, authorization, or retention rules.
- Review this policy before production payment-proof release, after any storage/provider change, after a security incident, and every 12 months.

## Basis for this template

OWASP requires authorization, allowlisted file types, type/size validation, generated names, and storage away from the web server. NIST media-protection guidance organizes policy around access, storage, transport, sanitization, use, and disposal. Indonesia’s PDP Law recognizes access, deletion/destruction, consent withdrawal, processing-limitation rights, and failure notification; apply it with qualified local legal advice before go-live. Applicable Indonesian tax rules may require source records to be kept ten years in Indonesia; obtain accounting/legal confirmation before setting production retention.

