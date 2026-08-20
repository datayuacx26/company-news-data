---
schema_version: "1.0.0"
document_id: "b1522e0a48f71faba2e758dec85085f9f89296a57bb9097d615b7134280def3d"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/v2-end-of-life-announcement"
published_at: "2024-08-05T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:18f2b3837717bc3cfe84fd9c381bbd50f9efc9c15267a7fe85e6b95afb512ba4"
---

# Trigger.dev v2 end-of-life and how to upgrade to v3

## The journey so far


We released Trigger.dev v2 in August 2023 and saw great early adoption. It built on our discovery that developers were desperately seeking a good way to easily build long-running background jobs on serverless infrastructure.


As adoption grew, it became clear that simulating "long-running" execution on top of serverless functions was not the right approach. This method led to a mess of code and numerous subtle issues – too many to list here. What developers really wanted was to write simple code without having to mange execution or worry about timeouts. And hence[v3 was born](https://trigger.dev/blog/v3-open-access) .


## We're shutting down v2


The[architecture for Trigger.dev v3](https://trigger.dev/blog/v3-announcement) is fundamentally different to that of v2, so we made the decision to develop v3 in parallel with v2, maintaining both platforms simultaneously. Now that v3 has matured and surpassed v2 in adoption, it's now time to announce the end of life for v2 on January 31st, 2025.


## What happens on January 31st, 2025?


On January 31st, 2025, we will be shutting down the v2 Cloud fully. This means no v2 jobs will be processed, and access to the v2 dashboard will no longer be available. We're making this announcement as early as possible to give all v2 users plenty of time to migrate to v3 or find an alternative solution.


---


> I have used every single background jobs platform on the market and trigger is amazing. I actually used v2, churned to inngest, and came back for v3. I can't wait to convert everything to trigger.


---


> I started to migrate 60 tasks from v2 to v3 yesterday and it's a great DX.


---


## Can we help?


We're on hand any time between now and January 31st, 2025 to help you migrate to v3. You can get in touch via email or book a call with us to get 1-on-1 help for your specific use case.


We also appreciate end-of-life announcements for services you're using suck and you may not want to migrate to v3 or self-host v2. Depending on your use case, there are other products available which we'll happily recommend depending on your specific requirements.


## What's next?


Trigger.dev v3 launched in January 2024 and represents the future of the platform. It has surpassed our expectations, completely eliminating timeouts, and providing a solid architecture and platform for long-term reliability and scale. We're really excited to see what you build with v3!
