---
schema_version: "1.0.0"
document_id: "548fa946eb258eea7b157de409eb2d8ed8c1af8488b7db4e817d6beea191faeb"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/greptile-v4"
published_at: "2026-03-05T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:d57a7128e19bef97949818ca9f08196185c0d4937db69567552e994e49d01010"
---

# Greptile v4 + New Pricing

Today we're releasing Greptile Agent v4, our best code review agent yet. It's better than v3 at catching tricky bugs, and also has a far lower false positive rate.


## v3 vs v4 performance


We have been A/B testing v4 for the last month across many customers. After hundreds of thousands of PRs reviewed, we have some interesting real-life data on how v4 compares against v3:


1. **Addressed comments per PR** went from 0.92 to 1.60, a **74% increase** . "Addressed" is determined by an LLM.
2. **Percentage of Greptile comments addressed** by the author increased from 30% to 43%.
3. Developers often reply to Greptile's comments. **Positive replies** ("nice catch" or "fixed") increased by **68%** from 0.31 per pull request to 0.52 per pull request.
4. **Upvote reactions** on comments increased by **60%** , from 0.05 to 0.08 per pull request.


Metric v3 v4 Change


Addressed comments per PR


(determined by LLM-as-judge)


0.92 1.60 +74%


% of comments addressed 30% 43% +44%


Positive replies per PR 0.31 0.52 +68%


Upvote reactions per PR 0.05 0.08 +60%


## Pricing


We're modifying our pricing to a base + usage model, similar to most other AI coding tools. The pricing is **$30/developer/month** , which includes **50 reviews/month** , after which reviews cost **$1 each** .


Less than 10% of active users will exceed the included usage. Existing users will remain on the existing pricing until the next billing month.


Better coding agents have led to a drastic increase in the number of commits reviewed by Greptile. This new pricing model allows us to focus solely on providing an uncompromising AI code review experience.
