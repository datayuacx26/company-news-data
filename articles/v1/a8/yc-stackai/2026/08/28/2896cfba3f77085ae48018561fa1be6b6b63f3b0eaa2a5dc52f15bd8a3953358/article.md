---
schema_version: "1.0.0"
document_id: "2896cfba3f77085ae48018561fa1be6b6b63f3b0eaa2a5dc52f15bd8a3953358"
company_key: "yc-stackai"
company: "StackAI"
source_id: "yc-stackai-news-import-1f4be1b3b390"
canonical_url: "https://www.stackai.com/blog/bernard-aceituno-featured-in-cio.com-your-enterprise-isn-t-ready-for-enterprise-ai"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T04:31:40.170606+00:00"
fetched_at: "2026-08-18T04:31:41.111700+00:00"
content_hash: "sha256:d3cccb250e1b68c7874d3254e9cac504b5e32345eb6079d40878243928cfb89d"
---

# Bernard Aceituno featured in CIO.com: "Your enterprise isn't ready for enterprise AI"

In a new opinion piece for[CIO.com](https://www.cio.com/) , Bernard Aceituno (co-founder of StackAI and former AI research scientist at Meta) argues that the biggest barrier to scaling enterprise AI isn't adoption, but governance and security.


Aceituno points to a Databricks and Economist survey finding that 40% of respondents believed their organization's AI governance program is insufficient, and cites Microsoft's Data Security Index showing only 47% of organizations report implementing specific GenAI security controls.


**The core argument: 8 layers of governance**


Drawing on his experience advising CIOs, Aceituno lays out eight governance layers he believes every enterprise needs before scaling AI agents, including:


1.


**Roles and groups** — granular role-based access control mapped to real departments


2.


**Scope** — private, invisible folders and allowlists for sensitive agents


3.


**Change control** — version history, edit locks, and one-click rollback


4.


**Publication** — SSO, password protection, and origin restrictions before releasing agents company-wide


5.


**Org-wide policy** — mandatory SSO, approval workflows, and controls on which tools/connectors are allowed


6.


**Data access** — checking end-user permissions at runtime rather than inheriting broad service-account access


7.


**Observability** — exportable logs of who ran what, when, and with what data


8.


**Authentication** — SSO, MFA, and defaulting new users to least-privilege roles


**On deployment models**


The piece also walks through four deployment postures (multi-tenant SaaS, dedicated single-tenant, bring-your-own-cloud, and on-premise), outlining the tradeoffs in setup time, control, and maintenance burden for each. Aceituno's take? Most organizations are well served by multi-tenant SaaS, while bring-your-own-cloud or on-prem make sense mainly for organizations with data sovereignty requirements and mature internal platform teams.


**The takeaway**


Aceituno closes with a self-diagnostic for CIOs: pick your three highest-privilege AI agents and see if you can name who owns each one, what changed recently, what data they can reach, and where the logs live. If the answer is "I'd have to ask around," he suggests that's a sign to slow down and do the governance work first. Read the full article[here](https://www.cio.com/article/4209896/your-enterprise-isnt-ready-for-enterprise-ai.html) .
