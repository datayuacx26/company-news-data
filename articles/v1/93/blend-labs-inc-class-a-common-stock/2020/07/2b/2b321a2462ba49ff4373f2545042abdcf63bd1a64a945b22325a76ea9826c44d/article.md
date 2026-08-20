---
schema_version: "1.0.0"
document_id: "2b321a2462ba49ff4373f2545042abdcf63bd1a64a945b22325a76ea9826c44d"
company_key: "blend-labs-inc-class-a-common-stock"
company: "Blend Labs Inc."
source_id: "blend-labs-inc-class-a-common-stock-rss-4631133ca4a9"
canonical_url: "https://full-stack.blend.com/a-day-in-the-life-of-a-secure-request.html"
published_at: "2020-07-27T07:00:00+00:00"
first_seen_at: "2026-07-20T23:18:43.300114+00:00"
fetched_at: "2026-08-20T00:34:47.711311+00:00"
content_hash: "sha256:51107844b7e8b79d77efc18227e29a2b8c97b3f575ca70fc8b1e5d39c8af2282"
---

# A Day in the Life of a (Secure) Request

All hops Introduction In this post, we will walk through the infrastructure components we use at Blend to secure incoming requests—a day in the life of a request, if you will. There are a variety of commonly-used mechanisms to secure cloud computing environments, which often involve load balancers and special-purpose proxy servers. As a result, requests from a client to an application server typically make a number of intermediate network hops en route to their final destination.
