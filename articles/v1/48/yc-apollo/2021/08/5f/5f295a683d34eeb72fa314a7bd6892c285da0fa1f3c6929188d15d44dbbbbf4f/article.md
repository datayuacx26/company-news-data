---
schema_version: "1.0.0"
document_id: "5f295a683d34eeb72fa314a7bd6892c285da0fa1f3c6929188d15d44dbbbbf4f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/audit-logs-now-available-in-apollo-studio-enterprise"
published_at: "2021-08-05T10:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:83faed676c9ef1b0694d988c6ccb10501f95d8259d77ae9074af7db766608f46"
---

# Audit Logs now available in Apollo Studio Enterprise

Today, we’re introducing audit logs in Apollo Studio Enterprise. With audit logs, you can export a data file with key actions taken within your organization.


As your graph scales to more teams in your organization, you need visibility into all of the potential change points on your graph that could affect its stability or performance. We built audit logs to give you more confidence in your graph governance and help you meet any compliance or security requirements.


**Use Cases for Audit Log**


- Investigate an incident and see what actions lead up to that incident by exporting a log for a time period and graph
- See what actions an individual has taken within a time period
- Investigate your automated systems that are changing the graph


**Some actions tracked by audit log:**


- Creating a graph or variant
- Publishing of schema/subschema
- Provisioning or inviting users
- Adjusting billing
- Provisioning or deleting an API key
- Adjustments to settings for schema checks


This is a partial list of what audit logs track, you can see the more complete list of actions tracked in our[docs](https://www.apollographql.com/docs/studio/audit-log/) . Audit logs include actions done by individuals as well as automated actions via the API or CLI. Data for the audit log is retained for up to 180 days **.**


**How to download an audit log**


Audit logs are accessible by organization admins via a new Audit tab inside Apollo Studio UI. When requesting a log, you can filter your exports to a specific actor, action on a specific graph, and/or specific time window. Once an audit log export is requested you’ll receive an on-screen indication as well as an email notification when the log is ready for download.


Create a customized audit log


View all of the audit *logs* that have been requested


**Get started** Audit logs are part of the Studio Enterprise plan. Please[reach out to us](https://www.apollographql.com/contact-sales/?referrer=blog) if you are interested in this feature.


Written by


David Isquick


[Read more by David Isquick](https://www.apollographql.com/blog/author/isquick)
