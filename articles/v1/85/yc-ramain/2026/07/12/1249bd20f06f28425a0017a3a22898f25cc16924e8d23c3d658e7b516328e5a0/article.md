---
schema_version: "1.0.0"
document_id: "1249bd20f06f28425a0017a3a22898f25cc16924e8d23c3d658e7b516328e5a0"
company_key: "yc-ramain"
company: "RamAIn"
source_id: "yc-ramain-news-import-43a68eac1f6d"
canonical_url: "https://ramain.ai/resources/credential-vaults"
published_at: null
first_seen_at: "2026-07-24T11:17:00.876485+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:555b333ae00473cf783fb12bff32028bf2c772149217a97b2369cedfd38748a7"
---

# Credential vaults: storing secrets without hard-coding workflows

\[Secret management\]


Credentials are not workflow steps. Ramain keeps passwords, API keys, cookies, inbox addresses, and other secret material inside profile-backed storage so workflow graphs can describe what to do without embedding the secrets needed to do it.


## Secrets live behind profile access


Agent profiles support encrypted secret and cookie storage with role-aware access control. Builders can manage the profiles they own, while workspace administrators can oversee shared profile hygiene.


Sensitive details are kept out of public workflow definitions. A workflow can describe the task while credentials and session state remain inside the appropriate profile boundary.


## Why workflows should not hold credentials


A workflow graph should be shareable, reviewable, and editable. If credentials are embedded in prompts, nodes, or code snippets, every workflow export becomes a secret-leak risk.


Keeping secrets in a vault-like profile boundary allows one workflow to be versioned while the underlying credential can rotate independently.


## Cookies and session state are also credentials


For browser automation, cookies and profile state can be as sensitive as passwords. Ramain treats stored cookies as profile material and applies ownership, admin, and audit constraints to access.


This is what lets a workflow say 'open the vendor portal' without saying 'here is the password and cookie jar inside the workflow'.


Security principle


A workflow should be safe to review and edit without exposing the secrets it needs at runtime.
