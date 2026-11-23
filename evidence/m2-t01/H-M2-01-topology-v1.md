# H-M2-01 — verified topology and disposable local profile v1.0

- Ticket: [M2-T01](../../tickets/m2-authoritative-foundation/M2-T01-topology-local-profile.md); batch `BAP-M2-SET-01` / 1.
- Code repository: `https://github.com/adityaa11/bride-assistant.git`; exact source commit `123e6b49b5a0090ef8023e20b9289b2eb82cafc2`.
- Observation environment and time: signed-in Vercel Hobby scope `izharaditya-2800s-projects`, Neon Free organization `org-jolly-grass-00265406`, this Windows/Docker workstation; 23 November 2026, approximately 22:39–23:00 Asia/Bangkok. No customer records or proof bytes were used.
- Activation: preparation only. Vercel has no deployment or Git connection yet. M2-T03 owns the production-like application-to-Neon deployment and migration integration; this record does not claim that path exists.

## Provisioned account topology

| Surface | Observed account setting / usable handoff | Direct observation |
| --- | --- | --- |
| Vercel | Project `bride-assistant`, ID `prj_P7fKpQy42FgHOCvnCgwss4a7zDMp`, Functions region **Singapore `sin1` only**. Hobby permits one region. Setting and Production variables require a new deployment to take effect. | [Function region](vercel-functions-sin1.png); Vercel project settings `/bride-assistant/settings/functions`. |
| Neon PostgreSQL | Project `bride-assistant`, ID `tiny-mode-75452518`; production branch `br-quiet-tree-bi2t78zg`; region `aws-ap-southeast-1`; database `bride_assistant`; SQL and independent pooled-client probes returned **18.7 (84da7d8)**, zero public tables. Pooled host `ep-lingering-pine-bi1z9216-pooler.c-5.ap-southeast-1.aws.neon.tech`; connection requires TLS with `sslmode=verify-full` and `channel_binding=require`. | [SQL result](neon-postgres-version.png); Neon SQL Editor and local `pg` connection probe. |
| Neon Object Storage | Same project/production branch, `ap-southeast-1`; bucket `bride-assistant-private` is **Private**. Branch S3 endpoint host `br-quiet-tree-bi2t78zg.storage.c-5.ap-southeast-1.aws.neon.tech`. Bucket is empty after the disposable probe. | [Private bucket](neon-private-bucket.png); console list and `neon buckets object list`. |
| Recovery | Neon Free production branch showed **six-hour** point-in-time history; manual snapshots are available, scheduled snapshots require an upgrade. No restore rehearsal was run. The user accepted these account terms for this **preparation-only** handoff on 23 November 2026. Reassess recovery before real data and under M5-W03. | [Backup & Restore](neon-recovery-terms.png); account console. |

An unused, empty Neon onboarding project `sweet-salad-07133692` was automatically created in Ohio before the Singapore project was selected. It has no role in this topology or secret references. The user explicitly chose to leave it in the account; no application or storage route points to it.

## Secret references and separation

Vercel `bride-assistant` has these **Secret** variables scoped to **Production only**: `DATABASE_URL`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_ENDPOINT_URL_S3`, `AWS_REGION`. [Account capture](vercel-production-secrets.png) shows names, lock indicators and Production scope, not values. The pooled database URL targets the Singapore project and uses verified TLS; the S3 key is a branch-scoped `storage:read` + `storage:write` credential. A separate Neon **project-scoped** management API key, named `M2-T01 disposable topology probe`, was created for operator probes; it is not configured in Vercel. Neon credentials remain in ignored `.local-storage/m2-local/` on this workstation; no value appears in the committed record or source. T03's environment owner must manage rotation and future Preview/Development separation before using those environments. No `NEXT_PUBLIC_` name carries a provider credential.

The existing fixture byte path remains `app/api/fixtures/m1-t15/content-media/...` → `shared/server/local-content-image-store.ts` → `shared/server/local-document-store.ts` → ignored local files. T01 did not add another byte store, a production byte adapter, a public object URL, or application authorization; T05/M4 own authorization before signed URL issuance, and T06 owns the production-neutral byte contract.

## Executed account and negative probes

1. Neon SQL Editor on `br-quiet-tree-bi2t78zg` / `bride_assistant`: `SELECT current_setting('server_version') AS version, current_database() AS database, (SELECT count(*) FROM information_schema.tables WHERE table_schema = 'public') AS public_tables;` → `18.7 (84da7d8)`, `bride_assistant`, `0`.
2. `npx --yes neon@latest connection-string production --project-id tiny-mode-75452518 --database-name bride_assistant --pooled --ssl verify-full` saved the URL only to an ignored local file. An isolated `pg` client connected to the pooled host, returned 18.7 and zero public tables, and its client stream reported encrypted TLS. `pg_stat_ssl` on the pooled backend reported `false`, so the client stream, not that pooler-side view, is the TLS observation.
3. `npx --yes neon@latest buckets object put bride-assistant-private/m2-t01/disposable-object.txt --project-id tiny-mode-75452518 --branch production --file <ignored disposable file> --content-type text/plain` succeeded. A direct unauthenticated GET to the same S3 bucket/key returned **403**. With a branch-scoped read/write storage credential, an authenticated S3 GET and a presigned GET with `X-Amz-Expires=60` both returned **200** and the exact disposable bytes. The signed URL and credentials were never printed or saved in the artifact store. A write-only storage credential returned 403 for GET and was revoked; the read/write credential is the one referenced by Vercel Production.
4. `npx --yes neon@latest buckets object delete ...` removed the disposable object; `npx --yes neon@latest buckets object list bride-assistant-private --project-id tiny-mode-75452518 --branch production --recursive --output json` returned `"objects": []`.
5. `npm run build` passed; a post-commit byte scan of all **198 tracked files** and **27 `.next/static` client files** found none of the Neon API key, database URL, S3 access key ID, S3 secret, or service token. The code worktree was clean at handoff. This is a local bundle scan of the current source, not a claim about a future Vercel deployment bundle.

These probes used the account state on 23 November 2026 and ephemeral tooling in ignored `.local-storage/m2-local/`; the named account, project, branch, bucket, and Vercel environment above locate a repeat probe. The Neon API key and storage credential are server/operator secrets and must never be copied into a fixture, customer record, client bundle, or durable artifact.

## Fresh local profile and teardown

`node scripts/m2-local-profile.mjs verify` passed twice on fresh unique Compose projects: `postgres:18.7`, zero public tables, the existing local `DocumentStore` put/read/remove with no remaining bytes, then `docker compose down --volumes` and isolated root removal. The fixed development startup and teardown are in code-repository paths `docs/m2-local-profile.md`, `compose.m2-local.yaml`, and `scripts/m2-local-profile.mjs` at the bound commit. No migration, seed, intended SQL change, or source-controlled byte file was added. The local profile is intentionally separate from the Neon production branch.

## Handoff and limits

- **T02:** use the confirmed region and PostgreSQL 18.7 runtime constraints; it does not inherit topology provisioning work.
- **T03:** consume Vercel project `bride-assistant`, Functions `sin1`, Production secret names above, Neon project/branch/database/pooled host and local Compose profile. It must prove a deployed application-to-Neon connection and migrations; Vercel has no current deployment.
- **T07:** consume the observed six-hour PITR and manual-snapshot terms and the user's preparation-only acceptance. This is not a restore rehearsal, object-recovery guarantee, or production-data release approval.

No real customer/proof data, public proof bucket, Vercel Blob, PostgreSQL byte-column fallback, or fixture-to-production promotion is authorized by this record. The Ohio onboarding project is explicitly excluded.
