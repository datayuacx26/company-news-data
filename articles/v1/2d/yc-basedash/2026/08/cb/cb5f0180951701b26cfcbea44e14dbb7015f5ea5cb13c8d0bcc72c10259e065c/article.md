---
schema_version: "1.0.0"
document_id: "cb5f0180951701b26cfcbea44e14dbb7015f5ea5cb13c8d0bcc72c10259e065c"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-audit-logs/"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-01T01:24:44.421921+00:00"
fetched_at: "2026-08-01T01:24:47.890042+00:00"
content_hash: "sha256:d4466232527b2e3e5a92068c550471a86afcc9f751df1ff28ee5501fa3d24586"
---

# Introducing Basedash audit logs

Today we’re launching **audit logs for Basedash** , a native record of everything that happens in your organization: who signed in, who viewed what, which queries ran, and how important settings changed.


For security, IT, and compliance teams, audit logs are the difference between trusting a BI tool and being able to prove it. When the security review asks who accessed customer data last quarter, the answer should take seconds, not an investigation.


Every action, on the record.


## Why analytics needs a record


Analytics touches the most sensitive data a company has. Revenue, customers, headcount, and margins all flow through dashboards and queries — and as adoption grows, so does the number of people, and now AI agents, interacting with that data every day.


Most BI tools have no memory of any of it. Someone views a dashboard, exports a CSV, or changes who can query a data source, and there is no durable record that it happened. That gap becomes real the moment a security review, a compliance audit, or an incident asks a simple question: who did what, and when?


Basedash audit logs close that gap. Access, queries, and configuration changes are recorded as they happen, so the answer to “who saw this?” is always in the log.


*One filter turns the log into an answer: who did what, when.*


## Who did what, when


The audit log is a single, chronological record of activity across your Basedash organization:


- **Access events:** sign-ins through your identity provider, dashboards viewed, tables opened, and exports created
- **Query events:** every query that runs against your data, whether a person or the AI ran it
- **Configuration events:** changes to groups, permissions, data source access, and organization settings


Each event carries the actor, the action, the resource, and the timestamp. Filter by person, action, or time window and the log becomes an answer instead of a haystack.


## AI is on the record too


Basedash is an AI-native BI platform, which means the question security teams ask first is the right one: what is the AI doing with our data?


Audit logs make that answerable. When Basedash AI answers a question, the query it ran is logged, attributed, and traceable — the question asked, the SQL that ran, and who the AI was working for. AI activity is held to the same standard of accountability as any analyst on the team.


*Every query the AI runs is logged, attributed, and traceable.*


## Built for the security review


Audit logs are designed to slot into the review processes and tooling your security team already runs:


1. **Export:** stream events to your SIEM or export them on demand
2. **Retention:** configure log retention to match your compliance policy
3. **API:** pull audit events programmatically through the[Basedash developer platform](https://www.basedash.com/blog/introducing-the-basedash-developer-platform)


They join the rest of Basedash’s enterprise controls — SSO,[SCIM provisioning](https://www.basedash.com/blog/introducing-basedash-scim) , role-based access control, and row-level security — and complement our SOC 2 Type II compliance. Together, the model is simple: central identity, explicit authorization, governed data access, and now a complete record of it all.


*Export, retention, and API access — built for the security review.*


## Getting started


Audit logs are available on the Basedash Enterprise plan.[Talk to the Basedash team](https://www.basedash.com/book/book-a-demo) to plan your rollout or walk through a security review together.


You can also explore Basedash’s broader[enterprise controls](https://www.basedash.com/enterprise) and[security model](https://www.basedash.com/security) .


**Roll out analytics your security team can stand behind — every action, on the record.**
