---
schema_version: "1.0.0"
document_id: "7a80d0a4b6f2b5acea79a9f99714d899cd867df02db6dcaf1f11b04b177e9dbb"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/azure-entra-id-sso-ga/"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:bead63c76cdf48864202591b09f8c4ad4d2e7ecc504e4f9abaf8dec1d1ac2a48"
---

# Microsoft Entra ID SSO is GA in nao Enterprise

Microsoft Entra ID SSO is now generally available in **nao Enterprise** .


For companies standardizing access through Azure AD / Microsoft Entra ID, nao can now use your existing identity provider for sign-in and pass the user's identity through to runtime queries where the warehouse supports federation.


This matters for the teams deploying nao beyond the data team. Business users get the familiar "Continue with Microsoft" flow. Admins keep identity in one place. Data teams can keep warehouse access aligned with the person asking the question, not a shared application user.


## What ships in GA


The GA release adds:


- **Microsoft sign-in for nao Enterprise** using your Entra tenant, client ID, and client secret.
- **Enterprise license gating** so SSO stays part of the Enterprise feature set.
- **Identity-preserving query execution** for supported database federation flows, starting with Azure Entra ID federation for Redshift.
- **MCP support** so calls made through the nao MCP can use the same signed-in user's identity when executing SQL.


The setup is intentionally boring: configure` AZURE_AD_CLIENT_ID` ,` AZURE_AD_CLIENT_SECRET` , and` AZURE_AD_TENANT_ID` , enable the Enterprise license with the` sso` feature, and the Microsoft button appears on the login page.


## Why this matters


Analytics agents are only useful in production if access control is production-grade too.


A shared service account is easy to wire up, but it weakens the trust model. Once everyone queries through the same identity, it becomes harder to explain who accessed what, harder to map permissions to existing company groups, and harder to satisfy enterprise security reviews.


With Entra ID SSO in nao Enterprise, the flow is closer to how companies already manage access:


- identity lives in Microsoft Entra ID;
- login happens through the company tenant;
- Enterprise features are controlled by the nao license;
- runtime SQL can use the signed-in user's token when the database supports it;
- the same identity path works from the app and from headless surfaces like the nao MCP.


This is a small UI change for users, but a big deployment change for enterprise teams. nao can now fit into Microsoft-centered identity environments without asking admins to create a parallel access model.


## Available now


Microsoft Entra ID SSO is available today in nao Enterprise.


If you're self-hosting nao Enterprise, configure the Azure AD environment variables and make sure your license includes the` sso` feature. If you want identity-preserving runtime queries, configure the database with the Azure Entra ID auth mode supported by your warehouse.


We'll keep expanding the same pattern across more enterprise identity and warehouse federation setups: simple sign-in for users, centralized control for admins, and data access that follows the user's real identity.


If you want to try it out, reach out to claire@getnao.io.
