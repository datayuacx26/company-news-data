---
schema_version: "1.0.0"
document_id: "ad83afd481efaaffa3b6334dd8d1a3dda343da1a6715576e62ecb5961a3c8559"
company_key: "yc-chatwoot"
company: "Chatwoot"
source_id: "yc-chatwoot-news-import-2810aa3d6c74"
canonical_url: "https://www.chatwoot.com/blog/updating-api-and-webhook-access-on-chatwoot-cloud"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T01:18:13.394314+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:e6d67c71295511c34ad5669e4e02b621694cbaa988b691a0c0ce83b1fff9579b"
---

# Updating API and Webhook Access on Chatwoot Cloud

Chatwoot has always been built with developers in mind.


From the early days, we wanted Chatwoot to be open, extensible, and easy to build on top of. That is why we made API access and webhook support available broadly across Chatwoot Cloud plans, including the Free plan.


This helped many teams experiment with Chatwoot, connect it with their internal tools, automate workflows, and build custom customer support experiences without needing to upgrade first.


We are proud of that. Being developer-friendly is a core part of how we think about Chatwoot.


At the same time, we have to make a change.


## **Why we are changing access**


Over the past few months, we have seen a significant increase in spam and misuse on. A large part of this abuse has come from free accounts using API access and webhooks at scale.


This creates problems for everyone.


It affects platform reliability. It increases infrastructure and abuse-prevention costs. It also creates unnecessary load for our team, who need to investigate, block, and clean up abusive usage instead of spending that time improving the product.


We want Chatwoot Cloud to remain reliable for real businesses and genuine developers building useful workflows. To do that, we need to put better controls around features that can be used for high-volume automation.


## **What is changing**


We are restricting API access and webhook support on Chatwoot Cloud to paid plans.


For existing users on the Free plan, API and webhook access will continue to work for a two-week transition period. After that, continued access will require an active paid subscription.


For new users on the Free plan, API and webhook access will no longer be available.


This change applies to Chatwoot Cloud. Chatwoot remains open source, and developers who self-host Chatwoot can continue to use APIs and webhooks as part of their own deployment.


## **What you need to do**


If you are currently on the Hacker plan and using APIs or webhooks, please upgrade within the two-week transition window to avoid disruption. If you are not using APIs or webhooks, there is nothing you need to do. Your Hacker plan access will continue as before, subject to the features available on that plan.


If you are using Chatwoot for production workflows, automation, or integrations, we recommend moving to a paid plan so that your setup continues to work reliably.


---


We are grateful to the developer community that has built on top of Chatwoot over the years. Your feedback, use cases, integrations, and contributions have shaped the product in many ways.
