---
schema_version: "1.0.0"
document_id: "fa15220938839933332fdf5d8d06dce2e7aa99b1f6dcc8fa01b76a79afd9ebc8"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/how-anakin-handles-credentials-and-data"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-21T06:45:54.754455+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:44e21554b5d325d8cfb741b3185e9a38fd3101de5e62cc4435e8b7c1f193adf7"
---

# How Anakin Handles Your Credentials and Data

Authentication-dependent automation is a category where trust is earned, not assumed. Every time you connect an account to an automation platform, you are making a decision about what that platform can see, store, and do with your credentials. This page explains exactly how Anakin approaches that responsibility.


## The short answer


Anakin does not store your passwords. Whether you are connecting a personal account or a company-managed login, the password is used once to establish a session, then discarded immediately. What we keep is the session. The secret is never ours to hold.


## How account connections work


When you connect an account to Anakin - to run automations through Wire, our authenticated web automation product - you enter a username and password once. Anakin uses those credentials to log in on your behalf, stores the resulting encrypted session, and discards the password immediately. It is never written to disk, never appears in a log line, and is not retrievable from our systems in any form.


**What Anakin keeps:** the encrypted session (browser cookies and local storage, including any JWT/refresh tokens the site stores there), scoped to your account.


**What Anakin discards:** the password, at the moment login completes.


When the session expires, you will be prompted to reconnect. Because the password is not stored, reconnecting is a deliberate action - there is nothing to auto-refresh from.


For teams with stricter governance or compliance requirements, Anakin also supports vault-backed authentication through Identity Sources - allowing you to connect accounts without ever entering a site password into Anakin directly. We cover that flow in detail in our Identity Sources guide.


## What we store vs. what we never store


We store We never store


Encrypted session cookies (AES-256-GCM) Your password in any form


An encrypted, scoped, revocable vault token (Identity Sources only) Your vault master password


A reference pointer to the vault item, not its contents Usernames or passwords beyond the brief in-memory login operation


Connection metadata: scope, status, timestamps Any secret in a form that is replayable or retrievable


One-way digests for abuse prevention Anything that would be usable if exfiltrated


## Security architecture


### Encryption at rest and in transit


All session material, vault tokens, and credential metadata are encrypted at rest using AES-256-GCM. All data in transit is protected by HTTPS/TLS. Anakin holds SOC 2 Type II and ISO 27001:2022 certifications.


### Strict tenant isolation


Every credential and session is scoped to the account that created it. Cross-user credential access is blocked at the gateway on every request. No other Anakin customer can reference your credentials regardless of how a request is formed.


### Zero exposure in API responses


Secrets are stripped at the database layer before any API response is constructed. No credential value - password, session cookie, API key - ever appears in an API response, error message, or frontend payload. Only metadata and status handles are returned.


### Redacted logs and telemetry


Error messages and internal telemetry are scrubbed so that secrets cannot surface in support tooling, observability systems, or debug output.


### Session lifetime


Browser sessions are retained for 90 days from the date of creation and then automatically and permanently deleted. Any session can be revoked from the dashboard at any time; revocation takes effect immediately on all subsequent task runs.


### Isolated, ephemeral browser environments


Wire runs every authenticated task in a sandboxed browser environment scoped to your account. There is no cross-customer session reuse - browser state from one run is not carried into another customer's environment or a future run in a different context. The environment is torn down at the end of its lifecycle. Scraped content and output data are stored separately from account and credential metadata, under standard cloud-provider isolation, with configurable retention policies.


### Rate limits and access controls


Operations that touch credentials or vault sources are rate-limited more aggressively than standard metadata calls. Access checks are enforced at identity selection, task execution, and audit log write time.


## For your security team


Common questions from IT, SecOps, Legal, and Procurement reviewers:


### Does Anakin store passwords?


No. In both the manual and vault-backed flows, the password is used once to establish a session and is immediately discarded. In the vault-backed flow, it is never typed into Anakin at all.


### Can Anakin agents see or reuse passwords?


No. Wire's automation agents operate on sessions, not raw credentials. The credential exists in memory only during the brief login operation and is never accessible to the agent running the task.


### What does Anakin actually store?


Encrypted sessions (AES-256-GCM), encrypted vault token references where applicable, and connection metadata such as scope, status, and timestamps. Nothing that constitutes a usable credential if exfiltrated.


### How long are sessions retained?


Users can revoke any session from the dashboard at any time.


### What certifications does Anakin hold?


SOC 2 Type II and ISO 27001:2022.


### Can another Anakin customer access my credentials?


No. Tenant isolation is enforced at the gateway. Credential ownership is validated on every request. Cross-customer access is architecturally blocked.


### Can we run a security review?


Yes. For prospects with specific compliance, data-residency, or zero-retention requirements, we are happy to walk through a detailed security review. Reach out to your account contact to schedule one.


## The model in one line


Anakin reads credentials only when needed, uses them once to establish a session, and keeps the session - never the secret. That is the security commitment behind every account you connect to Wire.


If you are evaluating Anakin for your team or need documentation for a security review, the full details are in our[docs](https://anakin.io/docs) - or reach out to your account contact directly.


*Anakin is SOC 2 Type II certified and ISO 27001:2022 certified. Browser sessions are encrypted at rest using AES-256-GCM and retained for 90 days. Read our full[privacy policy](https://anakin.io/privacy) at anakin.io/privacy.*


---


[Back to blog](https://anakin.io/blog)
