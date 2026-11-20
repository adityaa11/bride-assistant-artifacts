# M1-T17 — Verify active M1 surfaces and publish the replacement-seam inventory

- State: `ready`; batch/milestone/work package: `4 / M1 / M1-W07`; dependencies: T04/H-04 and T08/H-08 through T16/H-16.
- Source authority: M1-W07; M1 exit gate and cross-phase verification M1.
- Successor/handoff: `H-17` → M2 planning. Activation: `active-in-this-ticket` for the M1 review gate only. Reuse: `keep-local` review evidence. Legacy: `forbidden`. SQL: `not-applicable`. Empty state: `not-applicable`.

## Outcome, scope, entrypoint, endpoint, handoff, use path

Run the route/scenario review matrix and publish the fixture-to-backend seam inventory. Entry is the M1 review checklist consuming each active ticket's route, scenario ID/version and evidence. Endpoint is a versioned pass/fail evidence record and successor map. H-17 includes ticket/route/scenario, adapter version, visual/accessibility/security evidence, finding, and exact M2+ consumer. Use path: review entry → actual route render → evidence/finding → M2 planner.

## Review Contract, invariants, security/UI, validation

Review actual routes, not prototype HTML or isolated unit tests; fail any direct fixture feature import, unsafe sink, authority claim, public draft, canonical-label regression, missing responsive/accessibility evidence, or visible mismatch against `project's goal/UI Reference/`. No production acceptance or backend approval. Security verifies SEC-M1-01/02; UI verifies relevant VIS-001–014 plus VIS-REF-01. `RC-27`: PASS iff every active ticket has desktop/narrow, applicable loading/empty/error/result capture, reference-viewport comparison with no visible mismatch, and named successor seam; matrix + import scan + visual comparison record; build-only, functionally equivalent, or “inspired by” evidence is forbidden. Hard stop: any failed row leaves M1 not-ready.
