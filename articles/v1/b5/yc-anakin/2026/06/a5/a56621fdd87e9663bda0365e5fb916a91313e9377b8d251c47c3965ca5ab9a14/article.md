---
schema_version: "1.0.0"
document_id: "a56621fdd87e9663bda0365e5fb916a91313e9377b8d251c47c3965ca5ab9a14"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/wire-identity-sources-launch"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-21T06:45:54.754455+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:dc41dbf4526fbcb7d9c3bb95866afa7a9a79fa7be0c28aa96f8ce15eb4f7344f"
---

# Introducing Identity Sources for Wire: Passwords Never Enter Anakin

**How Wire now lets you run authenticated automations without handing your credentials to anyone.**


Most web automation platforms break at the same place: the login screen. The moment a workflow needs to act on a site that requires authentication - a supplier portal, a professional network, an internal tool - the platform needs your credentials. The standard answer is to store them. Wire has never done that.


But there is a step beyond not storing passwords. Identity Sources makes it so the password never reaches agents at all.


## What Wire has always done


Wire's approach to credential handling has always been more conservative than most. When you connect an account, you enter a username and password once. Wire uses it to log in, stores the resulting session cookie encrypted at rest, and discards the password immediately - it is never written to a database, never surfaced in an API response, and never accessible to the agent running your task.


Wire's agents run on sessions, not credentials. That model still holds. But for some teams - agencies running automations on behalf of clients, security teams with strict governance requirements, enterprises managing access through a central vault - the fact that a password was typed into a third-party product at all is a problem worth solving.


## Introducing Identity Sources


Identity Sources is a new connection layer for Wire that lets you point an identity directly at your existing credential vault. Wire reads the credential from your vault only when it needs to log in, uses it once, and keeps only the session. The password never touches Anakin's systems.


The first live provider is 1Password.


## How it works


You create a scoped Service Account token in 1Password - not your personal login, not your master password - and grant it access to specific vaults you choose. Wire verifies the token is live before storing anything.


When Wire needs to authenticate for a task:


1. Wire reads the username and password from your 1Password vault, in memory, at the exact moment of login
2. It logs in, stores only the resulting encrypted session (AES-256-GCM), and immediately discards the credential
3. Future runs reuse that session. When it expires, Wire re-reads from the vault and logs in again automatically


**Your agents are never exposed to your passwords, or your clients' passwords.** The agent that runs your automation sees a session - nothing more. The credential exists in memory for the duration of a single login operation, then it is gone.


There is also something you can verify yourself: because Wire accesses your vault through a 1Password Service Account, every read is logged in your 1Password activity log - not just in Anakin's systems. You can open your own 1Password account and see exactly when Wire accessed a credential and what it accessed. Anakin does not own that audit trail. You do.


## What this unlocks in practice


### Automations on behalf of clients


If you are running Wire workflows on behalf of clients, their credentials can stay in their vault. Wire reads only what it needs, only when it needs it. You are not holding a client password inside a third-party platform.


### Password rotation without broken workflows


When a password changes in 1Password, the next Wire session refresh picks it up automatically. There is nothing to update in Anakin. Workflows keep running.


### Revoke access in one click


Remove Wire's Service Account access in 1Password and it takes effect immediately. Existing sessions continue until their natural expiry; no new sessions can be established after revocation. Access governance stays in your hands.


### Auditable by design


Your security team gets a full access log through 1Password's own activity logging. What Wire accessed, when, and from which vault item - visible in the tools your team already runs, not locked inside a third-party dashboard.


## What we store vs. what we never store


We store We never store


Encrypted session cookies (AES-256-GCM) Your password in any form


An encrypted, scoped, revocable vault token Your 1Password master password


A reference pointer to the vault item, not its contents Usernames or passwords beyond the brief in-memory login


Connection metadata: scope, status, timestamps Anything replayable outside the granted vault scope


## Getting started


Setting up Identity Sources takes a few minutes:


1. Open the Identities dashboard in Wire
2. Select Add Account and choose 1Password as your source
3. Paste your Service Account token - Wire verifies it is live before saving anything
4. Browse your vaults and select the login item for the site you want to automate
5. Wire reads the credential, logs in once, and saves the resulting session


The resulting identity works like any other Wire identity. Set it as the default for a catalog, or reference its **credential_id** directly in API task calls. Revoke it from the dashboard whenever you need to.


## What's coming next


Identity Sources is built on a provider-pluggable model - new vault and identity providers slot into the same experience without changing the underlying flow. The current roadmap:


### Okta and SSO providers


For teams running Wire automations on sites protected by single sign-on, Okta support will let Wire handle SSO-gated logins using your existing identity provider. No browser workarounds.


### Additional password managers


The same pattern as 1Password, extended to other credential vaults. If your team uses a different password manager, the goal is for the experience to be identical.


### Secrets managers


For API-first platforms that authenticate with API keys - Stripe, OpenAI, Twilio - secrets manager integration will let Wire pull keys directly from your secrets store at execution time, using the same just-in-time resolution model.


These additions expand the same principle: Wire uses what it needs, when it needs it, from the vault you control.


## The model in one line


Wire reads your credential from the vault you control, uses it once to establish a session, and keeps the session - never the secret.


Identity Sources is available now in the Wire dashboard. For API-based setup or multi-vault configurations, reach out to your account contact.


*Anakin is SOC 2 Type II certified and ISO 27001:2022 certified. Read the full security model in our[privacy policy](https://anakin.io/privacy) at anakin.io/privacy.*


---


[Back to blog](https://anakin.io/blog)
