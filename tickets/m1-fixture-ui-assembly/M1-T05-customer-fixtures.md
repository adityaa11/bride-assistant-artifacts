# M1-T05 — Assemble customer journey fixture screens without authority

- State: `superseded`; batch/milestone: `3 / M1 / M1-W05`; replaced by T08–T11 after sizing review; depends on T02/H-02 and T03/H-03.
- Source authority: M1-W05; Project Context customer scope, lifecycle, availability, WhatsApp deferral.
- Successors/handoffs: `H-05` to M2-W02, M3-W05, M4-W02–04 and T07. Activation: `active-in-this-ticket` (fixture routes only).
- Reuse: shared T02/T03; customer journey/navigation and availability presentation stay local (RD-03/04). Legacy: `forbidden`; SQL: `not-applicable`; empty state: customer has no bookings.

## Scope, entrypoint, endpoint, and handoff

Render registration (`daftar`) and sign-in (`masuk`) placeholders, home, new-booking workflow, Booking Saya, help, availability, canonical status, proof/exception placeholders, and WhatsApp-unavailable states. Registration and sign-in may show fixture validation, loading, error, and success-result states, but never create an account or session. Entry: customer route receives named fixture scenario. Endpoint: an accessible review view whose submit/action ends in an explicitly non-authoritative command-result state; no account/session, write, upload, WhatsApp link activation, or official status change occurs. `H-05` documents customer registration/sign-in/read/result fields, scenario IDs/version, and backend successor; in-process typed handoff has no retries/idempotency. EUP: customer route → adapter/primitives → registration/sign-in/workflow/list/help → reviewer.

Empty Booking Saya explains absence and offers allowed next navigation; it does not create a booking. Customer availability displays only availability, never another customer identity/detail. The true path replaces prototype local account/booking storage and any direct visible `Dikonfirmasi`/noncanonical statuses with canonical M1 labels; no compatibility path.

## Review Contract

| ID | Authority | PASS iff / evidence / forbidden substitute / regression boundary |
| --- | --- | --- |
| RC-06 | M1-W05 + lifecycle | PASS iff registration, sign-in, all canonical customer states, and non-authoritative submit results have named scenarios; route/state matrix capture; a fixture account/session or changing fixture state as an official transition is forbidden; lifecycle/auth seam. |
| RC-07 | Project availability/privacy | PASS iff customer calendar/list says availability only and never contains fixture customer identity or an availability guarantee; negative assertions + screenshot; client-hidden private fields are forbidden; ownership/availability seam. |
| RC-08 | D-04 | PASS iff unavailable WhatsApp state is rendered when no approved configuration exists and no live deep link is produced; scenario test; blank/guessed number is forbidden; WhatsApp activation seam. |
| VIS-05 | visual contract/VIS-002..011 | PASS iff booking workflow order, form states, status meaning, empty/error/loading, keyboard and mobile transformation are visually inspected; unit test only is forbidden; customer task flow. |
| SEC-05 | SEC-M1-01/02 | PASS iff registration/sign-in placeholders and actions cannot be read as an account, authenticated session, or authorization, and rendering remains CSP-safe; copy/boundary test + scan; client role/state flag as proof is forbidden; future auth boundary. |

## Validation / hard stop

Security readiness: `applicable`; preserve the future server-side identity/ownership seam and enforce M1 safe-rendering discipline now.

Capture registration and sign-in validation/loading/error/success-result states, selected/unavailable availability fixtures, each canonical status, no-bookings, unavailable WhatsApp, and narrow layout. Do not invent the deferred rejection customer label; omit that state until authority exists.

## Amendment M1-T05-A1 — shared customer chrome/footer authority

- Decision authority: explicit user direction on 2026-11-21.
- Across the customer journey surfaces covered by this superseded umbrella ticket, the shared reference-derived `CustomerFrame` chrome, including its expanded footer composition, is the approved reusable customer shell. Its use is not a visual-fidelity defect.
- Any successor ticket remains responsible for its own workflow-specific visual fidelity, states, responsive composition, and security boundaries. This does not grant prototype runtime behavior, external links, storage, writes, or business authority.
