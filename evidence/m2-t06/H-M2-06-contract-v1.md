# H-M2-06 v1 — repository and DocumentStore contract

- Source code commit: `d806c9357a2ffb8404ad4aac0a66821449ce0a7e` in `https://github.com/adityaa11/bride-assistant.git`.
- Owner: M2-T06. Activation: preparation-only until named M3/M4 tickets implement and prove authoritative callers.

## Byte boundary

`shared/document-store.ts` defines `DocumentStore.put({ bytes, mediaType, collection })` returning `{ storageKey, sha256, size, mediaType }`, and `read(storageKey)` returning bytes plus metadata/collection or `null`. Collections are `portfolio`, `testimonials`, `events`, and `payment-proofs`; media types are JPEG, PNG, WebP, and PDF. The key is opaque and never grants read permission. The owning feature validates untrusted bytes/type/size and authorizes record access before calling the byte interface. The byte interface does not publish or sign a URL. M4-W01 implements Neon Object Storage behind it; M4-W02 owns proof metadata and access; M4-W05 owns durable content association; M4-W06 owns publication. Cleanup/retry/atomic association belong to those owners. The local fixture extension owns only disposable remove/orphan/association operations.

## Protected repository boundary

`shared/server/repository-contracts.ts` defines `AuthorizedRepositoryInput<T>` from T05's successful `AuthorizationResult<T>` and `RepositoryResult<T>` as a value or `NOT_FOUND`, `CONFLICT`, or `STORAGE_FAILURE`. The protected repository contract accepts actor plus parsed data, checks operation-specific trusted record identity and ownership, and returns typed absence without creating a default. A caller handles T05 authentication, role, ownership, and validation denials before repository access. A storage key, URL, fixture route, or client role cannot replace the actor and record identity. M3 consumes the seam for catalogue and booking repositories; M4 consumes it for content/proof records. No generic production repository or business schema is supplied here.

## Fixture boundary and next proof

`getScenario` remains only for declared review routes. Existing content fixture UI uses local `ContentImageStore` for record lifecycle above `DocumentStore`; its local files and manifests are disposable, not production data. Each M3/M4 consumer must prove its real caller, server authorization, operation-specific repository/provider implementation, result/absence, and customer/admin/public effect at activation. Production paths cannot fall back to fixture data or local bytes when a provider is unavailable.
