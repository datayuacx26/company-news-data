---
schema_version: "1.0.0"
document_id: "2132afbe4576a44f64d70e7e57b82d63ce15a882134ea4bd90df171b12c11e4b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/domain-claim"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:b787865a49d9792b4381a64c8bafa8b4a722d17400c128544d98b3bbd983e10a"
---

# Domain Claim

It's common to move domains between Resend teams.


- Test projects move to work accounts
- Teams consolidate multiple accounts
- Agencies hand off a domain to a new managed team


Starting today, you can move any domain you own between Resend teams using Domain Claim to verify ownership and transfer it.


## How it works


If you[add a domain already used by another team](https://resend.com/domains) , the dashboard will notify you.


If the original account has recent sending activity with the domain in question, you will need to[contact support to release the domain](https://resend.com/support) to prevent any issues.


To prove you own the domain, add the TXT record to your DNS, or do it in one click via DomainConnect for supported registrars. Return to the dashboard and click **I've added the records** .


Resend verifies the domain ownership, releases the domain from the previous team, and shows you the records to add to your DNS for sending or receiving.


To add the released domain to your team, add the displayed SPF and DKIM records to your DNS and click **I've added the records** .


Once verification is complete, the domain will be available to send and/or receive emails on your team.


## What's next


To get started,[add a domain today](https://resend.com/domains) .


Our future plans include expanding Domain Claim to the API and SDKs. At that time, we will also add support for the CLI, MCP, and other agentic tooling.
