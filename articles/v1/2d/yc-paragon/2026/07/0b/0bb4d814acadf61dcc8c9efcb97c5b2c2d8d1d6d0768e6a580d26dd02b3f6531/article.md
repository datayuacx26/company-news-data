---
schema_version: "1.0.0"
document_id: "0bb4d814acadf61dcc8c9efcb97c5b2c2d8d1d6d0768e6a580d26dd02b3f6531"
company_key: "yc-paragon"
company: "Paragon"
source_id: "yc-paragon-news-import-425709159824"
canonical_url: "https://www.useparagon.com/blog/security-review-rag-data-ingestion"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-13T22:52:47.126118+00:00"
fetched_at: "2026-08-13T22:52:50.044387+00:00"
content_hash: "sha256:841d22cd5ba08ee01ca40995f1f7e2d2d61bd46e48dd40e1e32e558f9448ab74"
---

# Passing a Security Review for RAG Data Ingestion

# Passing a Security Review for RAG Data Ingestion


Where does this data live, who can see it, and can you prove it? That's the first question a security reviewer asks about any RAG data ingestion vendor, and Paragon answers with evidence, not a slide: an audited cloud, a self-hosted option, or a fully on-premise instance, with SOC 2 Type II, GDPR, and HIPAA covering the cloud tier. Source permissions for file storage carry through ingestion and are checked before retrieval, and every action is logged and searchable in-product. Some of this is published for a reviewer to verify unassisted; the rest is what Paragon confirms once asked. This piece marks which is which.


## What do security reviewers ask about RAG data ingestion?


Reviewers evaluating a RAG data ingestion vendor work through the same list: where the data sits and whether it leaves; whether one customer's data can reach another's; whether source-system access controls still apply once a file is ingested; how credentials are stored and rotated; whether actions are logged and by whom; which subprocessors touch the data; and how fast it's gone after disconnect or churn.


Two mistakes recur on the vendor side: describing a control with no architecture behind it, and reaching for whatever capability a reviewer expects, shipped or not. Overclaiming costs more than a narrower true answer, since reviewers check. This is one piece of a broader evaluation — see the[full comparison of managed RAG ingestion platforms](https://www.useparagon.com/blog/best-managed-rag-ingestion-platform-multi-tenant-saas) if the vendor choice is still open.


## How do you pass a security review for RAG data ingestion?


Passing a security review for RAG data ingestion means answering the "where, who, and can you prove it" question across seven areas: data residency, tenant isolation, permission preservation, credential handling, audit logging, subprocessors, and deletion. Paragon answers each directly.[Managed Sync](https://www.useparagon.com/product/managed-sync) deploys as a certified cloud, a self-hosted option, or a fully on-premise instance. Per-tenant isolation holds at the platform level, a mechanism Paragon walks through directly rather than publishes. The[Permissions API](https://www.useparagon.com/product/permissions-api) turns a source system's own access controls into a permission graph checked before retrieval, today scoped to file storage. Credentials are encrypted at rest with tokens refreshed automatically, every action is logged and searchable in-product, and Managed Sync never stores the contents of the files it syncs.


The table below groups what reviewers ask by area, noting what's public and what Paragon confirms directly.


What reviewers ask


What satisfies them


Common failure


**Data residency** — where does data live?


On-premise, self-hosted, or a certified cloud (SOC 2 Type II, GDPR, HIPAA)


A region named on a call, unpinned in contract or config


**Tenant isolation** — can one customer's data reach another's?


Per-tenant isolation at the platform level; self-hosted/on-premise is isolated by construction — a fact Paragon confirms directly, not a published page


Asserted narratively, with no mechanism or sign-off behind it


**Permission preservation** — is a restricted file still restricted post-ingestion?


A permissions graph from the source's own access controls, checked before retrieval — file storage today


Checked only at tenant level, or assumed to cover sources it doesn't


**Credential storage & rotation** — how are tokens and keys stored?


Encrypted at rest, keys and values stored separately, tokens refreshed automatically


Stored with application data, or rotated by a support ticket


**Audit logging** — who did what, and when?


Every action logged (actor, action, object, outcome, timestamp), searchable in-product


Turns out to be a roadmap item once asked a follow-up


**Subprocessors** — who else touches this data?


A documented list on the trust portal, alongside compliance reports


A verbal list with no changelog to check later


**Deletion & retention** — how fast is data gone, and what else is in scope?


File contents never stored — the largest item resolved already; credentials, metadata, and the Event Log persist until removed on request


Described as solved without naming what still needs a request, or whose vector store holds the rest


## Data residency and where customer data actually lives


Data residency is a question about which of three documented environments the ingestion layer runs in: a[SOC 2 Type II](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2) ,[GDPR](https://gdpr-info.eu/) , and HIPAA-compliant cloud with data centers in the US and Europe; a self-hosted deployment inside a customer's own AWS, GCP, or Azure account; or a dedicated on-premise instance, for reviews in healthcare, financial services, or the public sector that won't accept a shared environment. Managed Sync proxies file contents from the source rather than storing them, so residency here is mostly about where that proxying runs, not a data store that grows elsewhere.


For the strictest reviews, on-premise is the real answer: it removes the question of where the vendor's own infrastructure sits, since the ingestion layer runs on the customer's.


## Proving tenant isolation


Proving tenant isolation means showing the mechanism, not asserting the outcome. Paragon isolates customers within its cloud environment, and a self-hosted or on-premise deployment is isolated by construction: it runs inside a customer's own infrastructure, not a shared one. That's a real architectural fact, but unlike SOC 2 or GDPR status, it isn't a page a reviewer can pull up — ask directly, and expect Paragon to walk through the mechanism rather than point at a URL.


Isolation alone answers a narrower question than most reviewers mean, though. Separating one customer's organization from another's says nothing about which individual inside it should see which document — that's a separate control, covered in[multi-tenant auth for customer-facing integrations](https://www.useparagon.com/blog/multi-tenant-auth-for-customer-facing-integrations) .


## Showing that source permissions survive ingestion


This is the most common security-review question specific to RAG: it turns an admin-level sync into something an individual end user's assistant retrieves from directly. The Permissions API maps each user's own access — across SharePoint, Google Drive, Box, Dropbox, OneDrive, and Confluence — into a permission graph built on the authorization model Google published as[Zanzibar](https://www.usenix.org/conference/atc19/presentation/pandey) , checked before a document reaches the model. How direct access, inherited folders, nested groups, and shared links resolve into that graph is the mechanism the[platform comparison covers in depth](https://www.useparagon.com/blog/best-managed-rag-ingestion-platform-multi-tenant-saas) ; what matters here is that the check happens first.
