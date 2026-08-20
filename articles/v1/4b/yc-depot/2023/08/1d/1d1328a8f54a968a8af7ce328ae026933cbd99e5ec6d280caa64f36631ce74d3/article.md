---
schema_version: "1.0.0"
document_id: "1d1328a8f54a968a8af7ce328ae026933cbd99e5ec6d280caa64f36631ce74d3"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/usage-caps"
published_at: "2023-08-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:fda941bb21c4670131970cf952798725571444e008e22ff65d7a6d00f28cf313"
---

# Now available: Usage caps

Today, we're announcing the availability of usage caps for all organizations 🎉


Organization owners can now configure a usage cap inside their organization for the maximum number of build minutes they want to allow monthly. Once the limit is exceeded, builds will fail to start.


## How to configure a usage cap


To configure a usage cap, you **must** be an owner of the organization. You can configure a usage cap by going to the organization settings page by clicking` Settings` at your Projects list.


Once on the Settings page, you can scroll down to the` Current usage` section.


There are two options for configuring a usage cap:


1. The default is unlimited build minutes (i.e., no usage cap) so you can use as many build minutes per month.
2. The usage cap option allows you to define a maximum number of build minutes per month. Once the limit is exceeded, builds will fail to start.


Usage caps allow you to control your monthly spending on Depot and avoid runaway costs. We recommended organizations that are unclear on how many build minutes they need to start with a usage cap and increase it as needed.


If there are additional features you'd like to see added around usage caps, please hop into our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
