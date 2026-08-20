---
schema_version: "1.0.0"
document_id: "b9e1275cebb0eea1294f2fe5b7b2a4441e832e510d2078aa6886d0e079eb3e66"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-news-import-5e23c3cc51ca"
canonical_url: "https://tech.trivago.com/posts/2"
published_at: "2024-12-17T00:00:00+00:00"
first_seen_at: "2026-07-24T04:45:12.158507+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:494a5dfd24b8c6d270b77023cc2778efa814732e229db0507feb68d04ac157a5"
---

# How We Build: Behind the Frontend of trivago’s Website

Why should you retry all tests on failure? Why not? This article will not go into details, listing pros and cons of each approach. There are already enough[resources on the Web](https://mrslavchev.com/2023/01/04/retry-harder-why-rerunning-tests-is-a-bad-idea/) about the topic, listing valid points for both opposing views. As trivago Hotel Search frontend QA team over the last years we tried to stay away from a brute-force retry policy for failures and we rather tried to execute test retries only in selected cases. Recently, when we switched to a Continuous Deployment approach for our[new frontend Web application](https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project) (which empowers developers to merge and release some pull requests autonomously), we faced a greater need than before for understandable and stable test results. Due to that, showing as few “red flags” as possible for the automated checks on pull requests became even more important to ensure enough confidence in test results and to avoid slowing down the software development life cycle. The requirements and the balance between deterministic results and success ratio shifted, at least in some cases.
