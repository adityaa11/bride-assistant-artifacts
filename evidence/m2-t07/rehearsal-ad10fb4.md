# M2-T07 five-minute expiry rehearsal — v1.0

- Code repository: https://github.com/adityaa11/bride-assistant.git
- Exact code commit: ad10fb47284517d0a77bdc39d228764f03b4c630
- Environment: Windows, Node 24.19.0, disposable local DocumentStore under ignored .local-storage/m2-t07/<random>; 24 November 2026 Asia/Bangkok.
- Entrypoint: node scripts/m2-t07-proof-expiry.mjs; development config: config/proof-retention.development.json.
- This is a clock-controlled local rehearsal. It does not activate uploads, set production retention to five minutes, or verify a production worker.

| Observation | Before | At expiry | Final |
| --- | ---: | --- | ---: |
| Private proof byte objects | 2 | Unreviewed at upload + 5 min; reviewed at final review + 5 min | 0 |
| Proof metadata rows | 2 | Matching rows removed with bytes | 0 |
| Proof-specific audit events | 3 | Matching events removed with bytes | 0 |
| Booking status-history rows | 2 | Preserved after both expiries | 2 |

The run checked one millisecond before the first expiry (no deletion), each separate origin, exact byte reads before expiry and null reads after, zero remaining .bin objects, a second no-op expiry, and removal of the isolated root. Output: `{"result":"PASS","rehearsal":"five-minute-clock-controlled","cases":["upload-origin","final-review-origin"],"before":{"bytes":2,"metadata":2,"proofAudit":3,"bookingHistory":2},"after":{"bytes":0,"metadata":0,"proofAudit":0,"bookingHistory":2},"repeatedExpiry":"no-op","disposableStoreRemoved":true}`.

Validation at this code revision: `node scripts/m2-t07-proof-expiry.mjs` PASS; `npm run lint` PASS; `npm run typecheck` PASS; `npm test` PASS 13/13; `git diff --cached --check` PASS. Metadata and history were isolated JSON stand-ins; no SQL migration, seed, production proof adapter, provider probe, real proof content, name, URL, or secret was used. The existing local DocumentStore supplied actual byte put/read/remove.

The approved production retention design remains eight months from final proof review, or upload when never reviewed, conditional on this successful rehearsal and the still-open production controls. The five-minute value is scoped to the disposable development config. No active production retention or alternate duration is enabled.
