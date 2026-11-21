# M1-T09 — Reference-fidelity correction

- Ticket: `M1-T09-customer-home-help-fixtures`
- Supersedes implementation evidence bound to: `61ae616213e53abf3623a97a83b488a7c71e2006`
- Review target commit: `ce1a09a2d7c090908ce151ec25783142b0de316b`
- Ticket state: `awaiting_review`

## Correction

The prior checkpoint overstated rendered-reference fidelity. This correction replaces the approximated customer-home content with the actual `home.html` hierarchy, source copy, stat labels, booking journey, package hierarchy, and embedded botanical artwork. It also applies the dedicated `bantuan.html` help-card geometry, gradient treatment, typography, artwork framing, and 640px behavior rather than a generic help card.

The raw reference HTML and scripts still do not run. JSX and local CSS preserve the mapped visible composition; typed T02 scenarios remain the sole fixture source, and the help action remains an accessible local unavailable result without a WhatsApp URL.

## Corrected visual evidence

- Source captures: `test-results/m1-t09-reference-home-desktop.png`, `test-results/m1-t09-reference-help-desktop.png`.
- Implementation captures: `test-results/m1-t09-repair-desktop.png`, `test-results/m1-t09-repair-640.png`, `test-results/m1-t09-repair-help-desktop.png`, `test-results/m1-t09-repair-help-640.png`.
- Comparison outcome: home reference hierarchy, visible copy, hero art/stamp, metric labels, and lower section hierarchy now match the mapped source; help card now matches the source card's centered composition, gradient surface, decorative arcs, type hierarchy, action treatment, and responsive full-width action.

## Review Contract Closure

| Row | Required proof | Evidence / result | Status |
| --- | --- | --- | --- |
| VIS-REF-M1-T09 | Source-led same-viewport visual match, without material mismatch. | Direct source and implementation captures listed above; source-level repair in `shared/customer-home.tsx` and `shared/customer-home.module.css`. | PROVEN |
| SRC-REF-M1-T09 | Mapped HTML structure/CSS/asset behavior converted to React with fixtures. | JSX preserves home/help region order and source copy; local React handlers replace prototype behavior. | PROVEN |
| RC-19 / SEC-M1-01 / SEC-M1-02 | Scenario backed unavailable help; no live URL or unsafe runtime. | `npm test` passed 9/9, including the T09 fixture/non-authority boundary test. | PROVEN |

Executed validation: `npm run check` passed before the final visual CSS correction; `npm test` passed after it. Production screenshots were taken from `next start -p 3321` after the final build.

Internal readiness: READY_FOR_CK
