# M1 fixture review regression repairs — two-ticket set

- Set ID: `BAP-M1-FIXTURE-REPAIR-01`; status: `ready`; artifact revision: `1.1`.
- Code repository and planning baseline: `https://github.com/adityaa11/bride-assistant.git` at `2ee952c9d6230849ff70e2db62efcc76454ecad9`.
- Selected milestone/work packages: M1-W05 customer fixture flow and M1-W07 review. User direction on 22 November 2026 authorizes two bounded tickets: remove the unauthorized browser booking store, then correct the T10/T12 browser assertions and rerun the route and M1-T17 review checks.
- Authority: [Project Context v3.1](../../contexts/project.md), [Stack Context v1.3](../../contexts/stack.md), [M1 milestone](../../contexts/implementation/milestones/M1-fixture-ui.md), [M1 verification](../../contexts/implementation/verification.md), [original M1 set](../m1-fixture-ui-assembly/README.md), [T10](../m1-fixture-ui-assembly/M1-T10-customer-booking-request-fixtures.md), [T09](../m1-fixture-ui-assembly/M1-T09-customer-home-help-fixtures.md), [T11](../m1-fixture-ui-assembly/M1-T11-booking-saya-fixtures.md), [T12](../m1-fixture-ui-assembly/M1-T12-admin-booking-availability-fixtures.md), and the [T17 v1 failed review](../../evidence/M1-T17-v1.md). The [T19 cross-surface fixture audit](../m1-fixture-relationship-audit/M1-T19-fixture-relationship-audit.md) is a parallel planning input, not permission to redesign the fixture graph in these repair tickets.

## Registry and order

| Ticket | State | Dependency | Outcome / handoff |
| --- | --- | --- | --- |
| [M1-T20 — remove booking browser store](M1-T20-booking-browser-store-regression.md) | `approved` | Approved T02/T09/T10/T11 interfaces and T17 F-01; H-20 CK `PASS`; no T19 completion dependency | `H-20`: fixture-only submit and T02-backed home/history, with browser no-write proof. |
| [M1-T21 — repair browser selectors](M1-T21-browser-selector-regression.md) | `planned` | T20 `PASS` and its exact code checkpoint | `H-21`: updated T10/T12 assertions, passing affected route suite, and fresh T17 review evidence/handoff. |

`T17 F-01 → T20/H-20 CK PASS → T21/H-21 CK PASS → explicit M1-T17-A7 GO re-review.` T19 runs independently of T20/T21, but T17-A7 also requires T19/H-19 CK `PASS` and finding-by-finding disposition. M1-required T19 findings need separately authorized CK-approved repairs before H-17; accepted M2+ deferrals need named successors. T21 does not issue a T17 checkpoint or CK `PASS` itself.

## Activation, expected paths, and reuse

- T20 is `active-in-this-ticket`: `/booking` named scenario → T02 adapter → `CustomerBooking` form → local non-authoritative result; `/beranda` and `/booking-saya` named scenarios → T02 adapter → their existing customer compositions. The browser store is removed or unreachable, with no compatibility fallback.
- T21 is `not-applicable` for production activation: test callers exercise the existing routes and produce versioned validation evidence; no application behavior is changed. Its expected review use path ends at the T17 GO reviewer, not a product route.
- `RD-REP-01 reuse-existing`: T02 `getScenario` and `defaultCustomerBookingRecords` already supply approved customer fixture reads; T20 consumes them without inventing a new source or user session. Owner T02, consumers T09/T10/T11.
- `RD-REP-02 keep-local`: customer home summary, booking request and Booking Saya detail remain separate compositions with different UI meaning; this repair shares approved data access, not one generic screen.
- `RD-REP-03 reuse-existing`: Playwright route harnesses and T17 review matrix are the validation seams. T21 makes assertions target the actual selected date and intended status message; it does not weaken the negative storage assertion.

## Security, UI, and planning boundary

T20 security readiness is `applicable`: remove the browser write of contact and booking fields now (`SEC-REP-01 implement-now`), preserve the future server ownership/transaction seam (`SEC-REP-02 preserve-seam`), and keep T18's unrelated media `DocumentStore` boundary intact. Scoped lens: OWASP ASVS 5.0.0 browser/data protection, OWASP Top 10: 2025 access/integrity, NIST CSF 2.0 Protect, NIST SP 800-53 AC/SC/SI. T21 security relevance is limited to preserving test assertions for these boundaries. T20 frontend readiness uses the established `docs/frontend/PROJECT_FRONTEND_CONTRACT.md` in the code repository and the mapped T09–T11 source-led visuals; no redesign is authorized. T21 changes tests only. No SQL, production data, account/session, backend fetch, booking transaction, public content, or provider change is authorized.

The T20 correction restores the approved M1 fixture boundary; it does not decide the wider one-source fixture model under T19. If T19 reveals a required new cross-surface behavior or interface, route that separately through planning. M1-T20 is approved after H-20 CK `PASS`; M1-T21 is eligible for its own bounded work.
