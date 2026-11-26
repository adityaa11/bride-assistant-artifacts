# M3-T02 CK review — admin catalogue commands

- Ticket: [M3-T02](../tickets/m3-booking-core/M3-T02-admin-services-actions-commands.md); batch 2; state `awaiting_review`.
- Reviewed code repository: `https://github.com/adityaa11/bride-assistant.git`; exact commit `648874441f4b088bd2c8599e66d5df6f35fcf120`.
- GO checkpoint: [M3-T02 admin commands](../checkpoints/M3-T02-admin-commands-6488744.md).
- Review type: first consolidated CK review. Result: `CHANGES_REQUIRED`.

## Preconditions and authority

The active tuple is M3-T02, batch 2, `awaiting_review`, checkpoint `M3-T02-admin-commands-6488744.md`, target `648874441f4b088bd2c8599e66d5df6f35fcf120`. HEAD matches the target. The only code worktree modification is the pre-existing unstaged user `AGENTS.md` edit, which does not alter the reviewed code.

The configured artifact checkout is `C:/Workspace/bride-assistant-artifacts`, a Git worktree whose origin matches tracked `atlas-workflow.yaml`; its manifest declares `atlas-workflow`. At pre-write inspection it had no staged or tracked modifications and only four pre-existing untracked `.gitkeep` files, which this review leaves untouched. The frozen M3-T02 ticket and its explicitly consumed M3-T01 and M2 auth/repository handoffs define the review contract.

## Review Contract verification

| Row | Status | Review evidence |
| --- | --- | --- |
| `RC-M3-T02A` | `IMPLEMENTED_UNPROVEN` | The real admin UI and action create durable rows, and source inspection confirms `authorizeRequest` precedes `withDatabase`. The browser flow tests anonymous/customer page redirects but does not submit a customer or anonymous command to the action. Unknown type and customer denial in `scripts/m3-t02-contract.mjs` call `executeAdminCatalogueCommand` with a hand-built decision, bypassing the ticket-required protected action and Zod parsing. No real action-boundary malformed-value or unknown-type before/after observation is recorded. See `CK-001`. |
| `RC-M3-T02B` | `PROVEN` | Inspected the actual caller/action/command path. Catalogue, package and promotion writes target only the nine M3 catalogue tables; no booking snapshot table or command write path exists. The fresh local check observed `master_package.base_price` unchanged at `1000000.00` through promotion edits and conflict. |
| `RC-M3-T02C` | `IMPLEMENTED_UNPROVEN` | Current desktop/mobile screenshots cover the three live routes; the browser test proves first create, reload, focused editor and promotion conflict feedback. Source sets a busy label and disabled buttons, but no browser observation proves the required loading state during an in-flight command. See `CK-002`. |
| `RC-M3-T02D` | `PROVEN` | The fresh PostgreSQL run proved inclusive overlap rejection, next-day and different-package success, update and membership conflict rollback, and two concurrent creates yielding one success and one conflict. Source locks old/new package IDs in stable sorted order and checks overlap in the same transaction. |

## Use path, migration and empty state

The committed path is `app/(admin)/admin/layout.tsx` session gate → the three live server pages → `loadAdminCatalogue`/`readAdminCatalogue` → `AdminCatalogueLive` → `adminCatalogueCommand` → `authorizeRequest` → `executeAdminCatalogueCommand` transaction → durable PostgreSQL rows/result → revalidated admin page. The browser test executes the admin create/reload path. The named `[state]` routes retain review-only fixtures; production pages import the live loader and component, with no fixture or localStorage fallback.

No T02 migration was added. `migrations/0004_catalogue_packages_promotions.sql` is the confirmed T01 M3-SQL-01 schema and two system type rows. The [rendered actual ERD](../evidence/m3-t02/actual-erd.png) was visually inspected: all nine affected tables, PKs, FKs, `1 : 0..N` cardinalities and the unchanged before/after state are legible. A fresh disposable PostgreSQL 18.7 run migrated `0001`–`0004`; its introspected nine PKs and nine FKs match the committed [schema JSON](../evidence/m3-t02/schema.json) exactly. The test data and Compose volume were removed in `finally`; no test seed entered the migration graph. The empty database had two system-owned types and zero item/bundle/package/promotion rows. The live browser showed empty states and first creates on reload; missing edit returned `NOT_FOUND`.

## Evidence binding and validation

At artifact commit `c88eb0f0089906b39a2f150cb2d7fbf9d60f38b7`, the ticket blob is `0f91470a9f56e45295afb0127c33350b292cebdb`, checkpoint blob `ea3ea25c8cddc878a546ad5428c1854b41aa03f7`, ERD PNG blob `ef5fdf0997f28342f25d972347ab1ad5ecd2b08d`, ERD SVG blob `597ee4e4aabf3985f81aa75a8f6d35e8a7482942`, and schema JSON blob `231cb9c110e3b7f322222d41909d662fec9e97d7`. The M1 visual baseline reference resolves at artifact commit `f7924cc2a62e75b91e4d14de2a701e88f7ffea89`, path `evidence/M1-T13/implementation-catalogue-1440.png`, blob `60c94054b1ec8cee8cce6362045761f7239a6aff`; it is a design comparison only. Current T02 captures provide current UI evidence.

Checks performed at the reviewed code commit:

- `node scripts/m3-t02-contract.mjs .local-storage/m3-t02-ck-evidence` — PASS in isolated Docker PostgreSQL 18.7; migration, empty state, retry, overlap, membership rollback, concurrency and base-price assertions passed.
- `node scripts/m3-t02-browser.mjs .local-storage/m3-t02-ck-browser` — PASS, 1/1 browser test through first-admin sign-in and disposable PostgreSQL.
- Compared fresh local `schema.json` with the committed evidence JSON — exact match.
- Inspected committed command, action, loader, live pages/component, contract and browser harnesses, migration, actual ERD and current screenshots. GO records passing lint, typecheck, 15/15 unit tests and build; CK did not rerun those commands.

No Vercel/Neon check was required: this ticket changes no provider path and names no provider-only criterion. The prior M2 provider evidence remains its baseline; no remote deployment claim is made.

## Findings

### CK-001 — Protected action negative proof is incomplete

`RC-M3-T02A` explicitly requires real protected-route/action integration with customer, anonymous, unknown `catalogue_type_id` and malformed cases, including DB before/after observations. `tests/m3-t02.browser.spec.ts` checks route redirects only for customer and anonymous users. `scripts/m3-t02-contract.mjs` constructs successful decisions and calls the repository command directly for customer and unknown type. Neither proves that forged requests through `adminCatalogueCommand` are denied before mutation. The committed action appears to enforce the rule, but the ticket's required harness and observations are absent. Supply a disposable action/route integration that submits these negatives through the real auth/Zod boundary and records typed result plus unchanged database state.

### CK-002 — Loading-state observation is absent

`RC-M3-T02C` and the ticket UI readiness require the changed admin UI to handle loading and provide current focus/loading/failure evidence. `shared/admin-catalogue-live.tsx` contains a `busy` state and “Menyimpan…” label, but `tests/m3-t02.browser.spec.ts` never observes an in-flight save or navigation loading state; the committed captures show settled success and error only. Capture or assert the live loading state during a deliberately pending command on a touched route, including the disabled submit/duplicate prevention and a usable focus state, then verify completion or failure recovers the UI. This is proof of the existing ticket condition, not a new visual design requirement.

## Frozen Finding Closure Matrix

| Clause | Ticket authority and unsatisfied evidence | Binary closure oracle | Direct-regression boundary |
| --- | --- | --- | --- |
| `CK-001.a` | `RC-M3-T02A`: customer and anonymous command denial before writes; current browser only observes page redirects and direct repository checks bypass action auth. | In a disposable real action/route integration, authenticated customer and anonymous command submissions return the expected denied result; database counts/rows before and after each stay equal. Evidence locator: named test assertions, executed command/outcome and DB observations in the CFC checkpoint. | Admin valid create and M2 auth/role route denials still pass. |
| `CK-001.b` | `RC-M3-T02A`: unknown type ID and malformed values through action/Zod with no mutation; current contract harness passes hand-built decisions directly to repository. | In the same real action/route integration, unknown `catalogue_type_id` and malformed submissions return typed conflict/invalid input respectively, and DB counts/rows before and after each stay equal. Evidence locator: named assertions, executed command/outcome and DB observations in the CFC checkpoint. | Valid Service/Action type selection and create/reload still pass. |
| `CK-002.a` | `RC-M3-T02C` and UI readiness: loading state on changed live UI; current browser/captures do not observe in-flight state. | A current-revision browser test/capture observes an in-flight command on one touched live route: visible loading feedback and disabled duplicate submit while pending, followed by settled success or error with usable focus. Evidence locator: named browser assertion/capture and executed command/outcome in the CFC checkpoint. | Existing empty, success, error, reload, responsive and focus behavior on the touched route remains usable. |

## Decision

`CHANGES_REQUIRED` for the three frozen clauses above. This is one consolidated first review. No scope-change observation or provider/deployment decision is needed. Return the exact clauses to the bounded remediation workflow; this review does not authorize or invoke CFC.
