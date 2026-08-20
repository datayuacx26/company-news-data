---
schema_version: "1.0.0"
document_id: "6cfa57056cb3b81fea25db04ea949781dac2ff4ba4c44240fec1af5fec118084"
company_key: "yc-testrigor"
company: "testRigor"
source_id: "yc-testrigor-rss-b60bfacb083d"
canonical_url: "https://testrigor.com/blog/audit-log-enhancements-ai-commands-on-premise-custom-certificates/"
published_at: "2026-07-20T21:00:30+00:00"
first_seen_at: "2026-07-20T23:24:14.091130+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:97f136e78fbb521971a9e594d48cfcf1e1614e11242510256f881472bebb066f"
---

# Audit Log Enhancements, AI commands, On-premise Custom Certificates

Heather Brooks


- [Releases](https://testrigor.com/blog/category/releases/)


**Weekly Newsletter**
Receive weekly testRigor newsletters packed with insights on test automation, codeless testing, and the latest advancements in AI.


## Audit Log Enhancements


With the latest update, more actions are being logged on the audit logs. This will provide better insights to the actions that are occurring within the company. These are the following actions that are now being registered:


- ` PersonalAccessTokenCreated`
- ` PersonalAccessTokenRevoked`
- ` CollaboratorInvited`
- ` InvitationResent`
- ` InvitationDeleted`
- ` InvitationDeclined`
- ` InvitationAccepted`
- ` CollaboratorRoleChanged`
- ` CollaboratorEnabled`
- ` CollaboratorDisabled`
- ` SsoAutoJoinChanged`
- ` Login`


## AI Command Changes


Our team has introduced changes that modifies the behaviour of click and grab commands that utilise AI, which will use the full context of the command being issued. Commands that contain “using AI” and fail will contain a more detailed explanation of why they failed:


## On-premise Custom Certificates


Users that utilize the on-premise version of testRigor will now be able to upload custom certificates in order to perform validations regarding certifications.
