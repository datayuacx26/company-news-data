---
schema_version: "1.0.0"
document_id: "d524b582a0dd207940aadab0dae9e0bb3cef6cb9b7a9af199dcda72b01cad65d"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/introducing-infisical-pam"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T01:10:11.205759+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:c5dfbd1d1bbffbb97c07e3c848dbad865f4730fcd3dd94b814de3ddad386086c"
---

# Introducing Infisical PAM: Secure, Fast Access to Production Infrastructure

A pager alert at two in the morning requires an on-call engineer to fix a corrupted row in the production database, but prod is locked down (as it should be). The only way to fix the incident is to reach a colleague with admin credentials and ask them to implement the fix or share the credential.


The former is inefficient and the latter dangerous. It's dangerous because admin credentials are extremely sensitive and should be frequently rotated and never shared in plain text. It's inefficient because engineers constantly need to use resources they don't normally need to access:


- An engineer needs one-time, read-only database access to see if their fix lowered event latency.
- A support engineer needs brief access to a customer's environment to reproduce a bug.
- An engineer from team A needs to access team B's infrastructure to debug a shared feature.


Using resources you normally can't access is called privileged access: temporary permissions for a sensitive resource (e.g. a server, database, cloud account, etc.) that security reasons normally bar you from. Incidents are one reason you may need access, but many are more pedestrian:


Managing these workflows manually doesn't scale: getting access relies on one person's attentiveness (or wakefulness), Slack or email are a dangerous place to keep important credentials, and hoping you'll remember to rotate them is a bad strategy. And if something *does* go wrong, it's hard to investigate because all the audit logs look the same.


This is why we built[Infisical Privileged Access Management](https://infisical.com/platform/pam) (PAM).


## Introducing Infisical PAM


If you're an engineer, you may have used some kind of PAM solution before. They get you access eventually, but usually feel clunky and slow, and force you into random browser windows instead of your normal workflow.


If you've deployed a PAM solution before, you know how annoying it is to install agents on every single piece of infrastructure so PAM can reach it, how hard it is to enforce best security practices, and how none of it really seems built for how software is built today.


We built Infisical PAM from the ground up in a way that doesn't import outdated assumptions and won't bog down engineering.


## How Infisical PAM is different


We're not saying you'll *love* being on-call with Infisical PAM, but Infisical PAM will make it a lot easier by giving you secure access to sensitive resources faster.


### One gateway, not a thousand agents


Legacy PAM tools reach infrastructure by installing an agent on every server, database, and other target you want to protect. All of those require maintenance, and if you ever spin up new infrastructure without immediately deploying the agent on it, nobody can reach it with legacy PAM.


Infisical PAM works through one lightweight gateway, deployed once per network, which makes outbound connections to Infisical. This gateway covers everything behind it: Postgres, an internal admin panel, a production server over SSH.


### Just-in-time access without ever holding the credential


Connect from a browser or the CLI and the gateway fetches credentials and connects on your behalf. You never see the credential, which means there's no separate client to install. Whatever workflow you use to query a database or SSH into a server is your PAM workflow.


Access automatically expires and is scoped to the exact resource and actions you need to enforce the principle of least privilege.


### Native recordings for every session and command


Every query and command in the session gets recorded by default (and, in some cases, a full screen recording). When it ends, the recording and the audit trail are there.


### Access policies depending on sensitivity


You can set policies to define how each resource should be gated and for whom. Some resources can be accessed by providing simple justification, others can require an extra approval. In that case, a request with a reason and a duration gets approved by someone with the authority, and access opens for exactly that window.


### What it connects to


Infisical PAM covers the most important accounts most teams need to gate: PostgreSQL, MySQL, MS SQL, MongoDB, SSH, Kubernetes, Windows and Windows AD (over RDP), plus AWS IAM roles, GCP service accounts, and Azure via a service principal.[See a full list in our docs](https://infisical.com/docs/documentation/platform/pam/overview) .


Infisical PAM offers connection in various ways. For databases, SSH, AWS, and Windows, there's a browser-based connection straight from the Infisical dashboard that has:


- A data explorer and SQL editor for databases
- An in-browser terminal for SSH
- A federated login straight into the AWS console
- A full remote desktop for Windows


Everything else runs through the CLI, which starts a local proxy so you connect with whatever client you already use (` psql` ,` kubectl` ,` gcloud` , your own RDP client) against` localhost` , with authentication handled for you.


If you already have privileged accounts scattered across your infrastructure, Discovery scans external systems (starting with Active Directory) to find them, stages what it finds, and lets you review before importing anything into PAM. That means Discovery covers infrastructure you're bringing in after the fact, not just what you set up in Infisical from day one.


### Same platform


Infisical PAM runs on Infisical Cloud or self-hosted deployments, same as the rest of the platform. It's the same login, the same Gateway, the same organization you already use for secrets management or certificate management.


The on-call engineer resolving an incident at two in the morning gets in fast and goes back to sleep even faster. But with Infisical PAM, nobody has to wake up to hand them a password, and nothing they used is still sitting in Slack the next morning.


[Start a free trial today](https://infisical.com/pricing) or[chat with one of our experts](https://infisical.com/talk-to-us) about how Infisical PAM can help make access easier for your team.If you're already using Infisical, \[check out PAM in the app today([https://app.infisical.com/pam/access](https://app.infisical.com/pam/access) )
