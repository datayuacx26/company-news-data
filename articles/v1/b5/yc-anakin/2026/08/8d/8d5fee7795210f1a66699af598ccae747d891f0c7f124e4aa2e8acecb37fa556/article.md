---
schema_version: "1.0.0"
document_id: "8d5fee7795210f1a66699af598ccae747d891f0c7f124e4aa2e8acecb37fa556"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/identities-credentials-sessions"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-05T00:49:53.361744+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:d72aa3c470f8aa78fb8af2608d01b8fb8a6fd7ca69fb3c33ce1d3a0de12a91c0"
---

# Identities, Credentials, and Sessions: Managing Logins Without Managing Passwords

"Store the user's login" sounds like one thing. Build it and it turns into three, and getting the boundaries right is the difference between a system that handles multiple accounts, rotating passwords and vault migrations - and one that needs a migration every time reality shows up.


Credential management for AI agents comes down to modelling it as three objects, and Wire does exactly that.


Object What it is Relationship


` identity` One account, on one site, belonging to one user holds →


` credential` A stored session, plus the binding that produced it points at →


` vault item` Lives in your vault. Never copied (end of chain)


One credential per (identity, type). The binding lives on the credential - not the identity.


## Identity: an account, not a person


An identity is "my account on this supplier's portal." Not a Wire user, not a login to Wire - one account on one site, owned by one user.


The plural matters. Agencies run the same site under many client accounts; a marketplace seller might have a US and an EU account on one platform. Each is its own identity with its own name, and tasks pick between them explicitly. Nothing about the model assumes one account per site.


## Credential: the session, and where it came from


A credential is a stored session under an identity. It holds the encrypted cookies, an expiry, and - this is the part that carries weight - a reference to the vault item that produced it.


Wire stores **one credential per (identity, credential type)** , and there are two types:


` CREDENTIALS`` BROWSER_STATE`


Produced by Logging in with a username and password Importing a browser session directly


Holds Cookies, tokens, and the proxy session that minted them The captured browser state


Pins the exit IP Yes No


Refreshable from a vault Yes Manual re-import


Because the uniqueness is on the *pair* , one identity can hold both at once without either clobbering the other. Tasks name the credential they want, so there's no ambiguity about which is in play.


The exit-IP row deserves a note, because it's the difference that bites in production. A session minted through a login knows which proxy exit it came from, and Wire pins later requests to that same exit. Plenty of sites - particularly commerce platforms and utility portals - invalidate a session that suddenly arrives from a different IP. An imported browser session has no such pin, so it can work perfectly in testing and fail in production for reasons that look like nothing.


## Why the binding lives on the credential


The obvious place to attach "this comes from 1Password item X" is the identity. It's the wrong place.


An identity is the stable thing: *this account, on this site* . How you authenticate to it is not stable. You might start with a typed password, move to a vault later, migrate from 1Password to Azure Key Vault, or re-point at a different item after a reorganisation. Attach the binding to the identity and every one of those is a change to the account itself - which means everything referencing that account has to care.


Attach it to the credential and those become what they actually are: the session was minted differently this time. The identity never moves. Tasks referencing it keep working.


There's a safety property too. If an identity is already bound to one vault item and a request arrives binding it to a different one, Wire refuses with` IDENTITY_BINDING_MISMATCH` rather than silently re-pointing it. Quietly relocating which secret backs a live account is exactly the kind of change that should require someone to say so twice.


## One binding model, any vault layout


The reference to your vault has to survive a real problem: no two vaults store credentials the same way, and we don't get to impose a convention on a vault you already use.


So the binding supports three shapes:


How your secret is stored How Wire binds to it


One item with username and password fields Point at the entry - Wire reads both


Password in the value, username in a tag Map each field to where it actually lives


Username and password as two separate secrets Map each field to its own entry


All three produce the same thing: a set of fields, resolved at login, in memory. The provider knows how its own vault is laid out; everything above it just asks for a username and a password.


Crucially, a binding names an item - **not a version.** Wire always reads the current value. That single decision is why rotation is invisible: change the password in your vault and the next login uses it, with nothing to update on our side.


## Session: the thing that actually does the work


Everything above exists to produce a session and then get out of the way.


Sessions are refreshed as they're used - the runtime writes cookies back after each action, so a site that keeps extending a session keeps it alive without anyone logging in again. When one does lapse, the next task returns` AUTH_EXPIRED` , and a login request mints a fresh one.


Be precise about that last step: it happens **on demand.** No background worker is signing into your accounts on a timer. The re-login is triggered by something asking for it - so Wire never reaches into your vault except in response to a request you made, and every one of those reads appears in your vault's own audit trail. Nothing shows up there that you didn't cause.


## What the shape buys you


- **Many accounts per site** - each an identity, chosen per task.
- **Rotation with no coordination** - bindings name items, not versions.
- **Vault migration without touching accounts** - re-bind the credential; the identity is unmoved.
- **Deletion at the granularity you need** - drop a session, an account, or a whole vault connection.
- **Nothing worth stealing** - a reference and an encrypted session, per site, self-expiring.


**Go deeper.** The identity and credential endpoints, including every field described here, are documented in full. The next post shows[how we prove a password never reaches a log line](https://anakin.io/blog/prove-password-never-reaches-log) .


→[API reference · Identities](https://anakin.io/docs/documentation)


---


[Back to blog](https://anakin.io/blog)
