---
schema_version: "1.0.0"
document_id: "e843c8f04ab80cc865c94b61786fc7ce23d10bbe9afbdc041d16d48c0e2e9d86"
company_key: "yc-paragon"
company: "Paragon"
source_id: "yc-paragon-news-import-425709159824"
canonical_url: "https://www.useparagon.com/blog/multi-tenant-oauth-token-storage-architecture"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-13T22:52:47.126118+00:00"
fetched_at: "2026-08-13T22:52:50.044387+00:00"
content_hash: "sha256:f14c1c5efe2d71bd5e887aaa6f781bff2786c8b074707532fae65a84ad651912"
---

# Multi-tenant OAuth architecture: token storage, isolation, and key rotation

# Multi-tenant OAuth architecture: token storage, isolation, and key rotation


Multi-tenant OAuth architecture is the layer underneath[how multi-tenant auth fits together end to end](https://www.useparagon.com/blog/multi-tenant-auth-for-customer-facing-integrations) . Building it yourself means three architecture decisions: a spec-compliant authorization flow, an isolation model that holds a real boundary between tenants under load, and an encrypted storage layer that survives a key rotation without a maintenance window — get any one wrong and the other two don’t save you. A spec-correct flow on top of a leaky isolation model is still a breach waiting to happen; a well-isolated store holding unencrypted tokens is a compromised Redis instance away from the same outcome. This page works through all three in the order a build decision needs them: the flow and the spec that governs it (RFC 9700), the isolation model that decides how far one tenant’s compromise reaches, the token record schema that makes that isolation model enforceable in a database, and the encryption mechanics — envelope encryption, per-tenant keys, key rotation — that keep a compromised store from also being a readable one.


## How do you build and manage OAuth flows for many third-party SaaS integrations?


Paragon’s answer is SOC 2 Type II–audited, per-tenant-isolated token storage underneath a spec-compliant OAuth flow — the same three-layer model most integration platforms in this category converge on, whether or not they publish the mechanics. The rest of this page works through each layer: what[RFC 9700](https://www.rfc-editor.org/rfc/rfc9700) actually requires for the flow; the pool, silo, and bridge isolation models and where each breaks; the fields a token record needs to hold up that isolation model; and the DEK/KEK mechanics that let you rotate a key without touching a single stored token value.


## The flow and the spec: PKCE, RFC 9700, and what “MUST” actually means


The default flow for a multi-tenant integration platform is the authorization code grant with PKCE.[RFC 6749](https://www.rfc-editor.org/rfc/rfc6749) defines the authorization code grant, but it never mentions PKCE — the term doesn’t appear anywhere in the document. PKCE is a separate specification,[RFC 7636](https://www.rfc-editor.org/rfc/rfc7636) , published just under three years later, in September 2015. A page that cites RFC 6749 for PKCE is citing the wrong document.


[RFC 9700](https://www.rfc-editor.org/rfc/rfc9700) — the OAuth 2.0 Security Best Current Practice, published as BCP 240 in January 2025 — is what actually pins down when PKCE is required. Per §2.1.1, public clients face a hard **MUST** ; confidential clients only get a **RECOMMENDED** — not a universal mandate either way. Authorization servers themselves must support PKCE regardless of client type. The confidential-client recommendation exists because PKCE also defends against authorization-code injection and misuse, not only the interception attack it was originally designed for.


The implicit grant gets similarly overstated. RFC 9700 §2.1.2 says clients **SHOULD NOT** use it — a strong discouragement, not a prohibition. “Effectively dead in practice” is accurate; “banned” overstates the normative language.


OAuth 2.1 gets treated as a ratified standard in a lot of copy on this topic. It isn’t. As of August 2026, it’s still an Internet-Draft — currently[draft-ietf-oauth-v2-1-15](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/) , dated March 2026 — not an RFC. It folds RFC 6749 and RFC 9700’s recommendations into one document (PKCE built into the authorization code grant, implicit and Resource Owner Password Credentials grants dropped), but it’s a dated snapshot, re-issued periodically. Treat it as “the OAuth 2.1 direction,” not the current standard.


### The state parameter’s downgrade, and what carries CSRF protection now


Under RFC 6749 §4.1.1,` state` is RECOMMENDED, not required — but §10.12 separately says the client MUST implement CSRF protection for its redirection URI, and` state` has historically been the typical mechanism for that. OAuth 2.1 draft-15 §7.9 downgrades` state` itself to OPTIONAL where PKCE’s` code_verifier` or an OIDC` nonce` provides equivalent protection — not unconditionally. RFC 9700 is explicit that a client can rely on PKCE alone for this only after confirming the authorization server actually supports it; without that confirmation,` state` or` nonce` still has to carry the CSRF defense. Under OAuth 2.1 that obligation moves onto whichever mechanism the client can actually verify is in place.


Skipping or mis-implementing` state` under the still-widely-deployed RFC 6749 model isn’t theoretical. A missing or predictable` state` value lets an attacker bind their own authorization code or access token into a victim’s session — a login-CSRF pattern where the victim ends up authenticated as, or linked to, the attacker’s account. RFC 9700 formalizes this as an active attack class in §4.5 (authorization code injection) and §4.7 (CSRF), which is why a spec-compliant flow still treats` state` as load-bearing today, even under the OAuth 2.1 draft’s conditional downgrade.


What happens after the token is issued — how long it lives, when it rotates, what a provider does on reuse detection — is a separate set of mechanics, covered in[refresh token rotation and expiry mechanics at scale](https://www.useparagon.com/blog/oauth-token-refresh-expiry-at-scale) .


## Three isolation models for per-tenant OAuth tokens


Once the flow issues a token, the next decision is how it’s stored relative to every other tenant’s tokens. There’s no single standardized name for this spectrum, but a common way to describe it — grounded in AWS’s own[multi-tenancy isolation guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/multi-tenancy-amazon-neptune/silo-model.html) rather than a formal, universal taxonomy — is pool, silo, and bridge.


**Pool** stores every tenant’s tokens in a shared table under a shared key, with tenant boundaries enforced only at the application and row-query level — no cryptographic barrier, just a` WHERE tenant_id = ?` clause. **Silo** is the opposite extreme: a fully separate store and a fully separate key per tenant, the strong-isolation end of the spectrum AWS documents for Neptune multi-tenancy. **Bridge** sits between the two — shared storage infrastructure with a per-tenant key or cryptographic boundary layered on top, so tenants share the database without sharing the thing that decrypts their tokens.


Pool


Silo


Bridge


Isolation strength


Logical only — no cryptographic tenant boundary


Cryptographic and physical — separate store and key per tenant


Cryptographic — shared store, per-tenant key boundary


Operational overhead


Lowest — one store, one key to manage


Highest — store and key count scale linearly with tenant count


Moderate — one store, but key management scales with tenant count


Where it breaks


A single row-level bug or a leaked shared key exposes every tenant at once


Key and vault sprawl hits provider-side limits (request-rate, role-assignment ceilings) as tenant count grows


Silently collapses back into pool if the per-tenant key boundary is enforced only in application code, not at the IAM/encryption-context level


Pool’s failure mode is direct: with no cryptographic wall between tenants, one leaked shared key or compromised row-level check exposes every tenant sharing that store. Bridge’s failure mode is subtler — it only works if the per-tenant key boundary is enforced by the platform (an IAM policy, a KMS grant, an encryption-context check), not by application code that’s supposed to remember which key to use. A bug in that check doesn’t announce itself; it quietly turns a bridge deployment back into a pool deployment with extra steps.


## The token record: what to store, what never to store


The fields below are a **recommended model, not an industry standard** — no vendor publishes a literal “OAuth token record schema.” This is a synthesis of patterns with first-party sources: AWS Secrets Manager’s approach to storing secret values, and[OWASP’s Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) guidance on logging a secret’s lifecycle.


-


` **tenant_id**` **/**` **customer_id**` — the row-level and cryptographic isolation key, required regardless of which model above you pick.


-


` **provider_id**` **/**` **integration_id**` — which third-party OAuth provider and connection type this record belongs to.


-


` **provider_account_id**` — the provider’s own identifier for the connected account, distinct from` provider_id` . Without it, a reconnected account or a support ticket referencing “the Salesforce connection” can’t be matched back to the same upstream account a refresh error is failing against.


-


` **encrypted_access_token**` — ciphertext only, never plaintext, mirroring how Secrets Manager stores a secret value: never the plaintext, only the encrypted form plus a reference to the key that wrapped it.


-


` **encrypted_refresh_token**` — the same ciphertext-only treatment, handled at least as strictly given how much longer it typically lives.


-


` **wrapped_dek**` — the record’s DEK, itself encrypted by the tenant’s KEK, stored beside the ciphertext it protects. Without it there’s nothing to unwrap on the next read — the plaintext DEK is discarded from memory right after encryption.


-


` **encryption_context**` — the tenant and record identifiers bound directly into the KMS wrap/unwrap call, mirroring Secrets Manager’s practice of binding` SecretARN` /` SecretVersionId` to every` GenerateDataKey` /` Decrypt` call. Without it, a wrapped DEK copied into the wrong tenant or record context still decrypts for any caller that holds KMS permission; with it, the decrypt call fails unless the same tenant and record context is supplied, instead of depending on an application-layer check a bug can skip.


-


` **expires_at**` — needed for refresh logic; low sensitivity, fine to leave unencrypted and queryable, consistent with Secrets Manager leaving rotation metadata unencrypted while encrypting only the secret value.


-


` **scope**` — the granted OAuth scope(s); low sensitivity, typically unencrypted and queryable.


-


` **key_id**` **/**` **key_version**` — which KEK and version wrapped this record’s DEK. This is what makes key rotation possible without re-encrypting every row — more below.


-


` **refresh_error**` — the last refresh failure’s error code and timestamp. Without it, a revoked token and one that hit a transient provider outage look identical from the outside — meaning either needless re-prompts or a missed revocation until a customer notices.


-


**Audit fields** —` created_at` ,` last_rotated_at` ,` last_used_at` , and` created_by` /` rotated_by` , following OWASP’s §2.11 guidance to record when a secret was created, consumed, rotated, or deleted, and by whom.


-


**Revocation metadata** —` revoked_at` ,` revoked_by` , and` revocation_reason` (user-disconnected, tenant-offboarded, provider-force-revoked, admin-initiated). The reason is what determines whether the right response is prompting a reconnect, deleting the row, or alerting the tenant’s admin that the provider revoked access on its own.


-


` **status**` — active, revoked, or expired; not directly sourced, but a natural complement to the audit trail.
