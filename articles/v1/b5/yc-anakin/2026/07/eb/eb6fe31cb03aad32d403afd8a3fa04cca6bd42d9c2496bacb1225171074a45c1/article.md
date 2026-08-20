---
schema_version: "1.0.0"
document_id: "eb6fe31cb03aad32d403afd8a3fa04cca6bd42d9c2496bacb1225171074a45c1"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/azure-key-vault-wire-rbac-scoping"
published_at: "2026-07-19T00:00:00+00:00"
first_seen_at: "2026-08-05T00:49:53.361744+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:a519e5ad5d01b28fc3e93807bbcb82e3c99864cabc9a8a0a0f61a7dda1e82a18"
---

# Using Azure Key Vault with Wire: RBAC, Scoping & the Role Everyone Gets Wrong

Azure Key Vault is a supported identity source in Wire. If your secrets are already governed by Azure, this keeps them there - Wire reads a secret at the moment of login and never holds a copy. This is what Azure Key Vault automation looks like for logins behind an account: read one secret, use it, keep only the session.


Setup takes four steps rather than one, and it's worth understanding why before you start, because the reason explains most of what follows.


## Why you supply the vault list


Other integrations discover what they can reach. Azure won't let us.


**There is no data-plane API that lists the vaults a service principal can access.** Enumerating them means Azure's management plane - a different token scope and subscription-level Reader access. That is a far larger grant than "read some secrets," and asking you for it so we could populate a dropdown would be a bad trade.


So Wire doesn't ask. You provide the vault URLs at connect time, and **that list is the scope.** Wire refuses to read from any vault outside it - enforced on every single resolution, not just at setup, so a malformed or tampered reference can't reach a vault you never named.


## The four steps


# Step What it is


1 App registration Entra ID → client ID + tenant ID


2 Client secret Copy the Value, not the ID


3 Role per vault Key Vault Secrets User


4 Vault URLs One per line - this is the scope


Two things reliably go wrong.


In step two, the portal shows a **Value** and a **Secret ID** next to each other. You need the Value, and it's shown exactly once - navigate away and you'll be generating a new one.


Step three is the one everyone gets wrong, and it deserves its own heading.


## The role everyone gets wrong


Azure offers a role called **Key Vault Reader** . It sounds correct. It is not the one you want.


Key Vault Reader grants access to *metadata* . With it, Wire can list every secret name in the vault and receives a 403 on every attempt to read a value. The failure is maddening precisely because it looks like it's working - the browse populates, the picker fills, everything appears connected, and then every sign-in fails.


The role you want is **Key Vault Secrets User** , and it must be assigned *per vault* . Access in Azure is granted on each vault individually, so if you connect four vaults and assign the role on three, the fourth fails on its own - which is why Wire verifies every vault at connect time and names the specific one that's missing a grant.


## Why Azure needs two words for "no"


Authentication and authorization are separate systems here. Microsoft Entra ID answers *who are you* and issues a token good for about an hour. Each vault's RBAC answers *what may you read* . A completely valid credential returns 403 on a vault it was never granted.


Wire treats those differently, and the distinction is load-bearing:


Azure says Meaning Connection


401 Entra rejected the principal REVOKED


403 No role on this vault KEPT


404 No vault or secret there KEPT


429 Throttled KEPT


Collapse 401 and 403 into "access denied" and one missed role assignment tears down a connection that works everywhere else. Testing against a live tenant before the role existed, the connect failed with an error naming the vault and the role to grant - not "credentials rejected," which would have sent someone off to regenerate a client secret that was never the problem.


## How to store the secret


An Azure secret is **one opaque string.** Unlike a password manager's login item, it has no username field and no password field - just a value, and some tags.


So Wire reads it three ways at once, and all three work:


How you store it What Wire does


` {"username": "…", "password": "…"}` Parses the JSON, each key becomes a field - **recommended**


Password as the value, username in a tag Maps each field to where it actually lives


Username and password as two separate secrets Maps each field to its own secret


JSON is tried regardless of whether the content type says so, because plenty of secrets carry a JSON body with the content type unset.


One caution, and it's important: **tags are metadata, not secrets.** Anyone who can list the vault can read them, and they're visible in the portal without any secret-read permission. A username in a tag is reasonable. A password in a tag is not a password you've protected.


## Secret versioning, and why rotation just works


Azure keeps every version of a secret. A reference from Wire deliberately names the secret without pinning a version, which means **latest** - always.


Rotate the value in the vault and there's nothing to update here. The current session runs until it expires; the next sign-in picks up the new value. If your rotation is automated, this is entirely hands-off.


## Every read lands in your own audit trail


Key Vault logs secret access through Azure Monitor like any other Azure resource. Point your vaults at a Log Analytics workspace and every read Wire performs shows up there - which secret, which vault, which principal, when.


That's the governance argument in one line: the evidence of what we accessed is produced by Azure, stored in your tenant, and queryable by your team using the tooling you already run. We can't edit it, we can't omit from it, and you don't need us to produce a report.


For anyone answering an audit, this is the difference between a vendor attestation and a primary record. One is a claim. The other is your own log.


## Sovereign clouds


Public Azure, US Government, and China are all supported, and there's no setting to get wrong - the cloud is derived from the vault hostname, which also selects the right Entra authority. The two can't drift out of sync because they're read from the same place.


## Things worth knowing before you commit


- **Client secrets expire.** Entra caps them at 24 months, and it doesn't report the expiry date through the token exchange - so Wire can't warn you. Put a reminder in a calendar. Surfacing this is on our list.
- **Vault firewalls need an allowlist.** If your vaults restrict network access or use private endpoints, our egress addresses need permitting. Wire detects and reports this distinctly rather than calling it a credential failure, but it can't work around it.
- **Very large vaults are paged.** Browsing lists up to 500 secrets per vault. Beyond that, reference secrets directly rather than picking from a list.


## Which is to say


Azure asks more of you at setup and gives more back in governance. Role assignments are made by the people who already make role assignments, in the system your auditors already read. Access is per-vault (scoping you define), versions are per secret, and[revocation is removing a role](https://anakin.io/blog/revoke-wire-access-what-each-deletes) - a thing your organisation already has a process for.


If your secrets live in Azure, keeping them there and letting Wire read one at a time is a smaller change to your security posture than moving them somewhere else would be.


Next in Identity & Access:[1Password vs Azure Key Vault - which vault for which team](https://anakin.io/blog/1password-vs-azure-key-vault-wire) .


**Set it up.** Have your tenant ID, client ID, client secret and vault URLs to hand, and connecting Azure Key Vault as an identity source takes one sitting.


→[Get started with Wire](https://anakin.io/products/wire) ·[Docs](https://anakin.io/docs/documentation)


---


[Back to blog](https://anakin.io/blog)
