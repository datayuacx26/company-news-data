---
schema_version: "1.0.0"
document_id: "245fcdf4ff470b7a85bb6393386d7c3c2699a7793a4fe6f7e6e6b1deb560f9d2"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/whatnot-engineerings-2025-blog-highlights-e7a63dbd0057"
published_at: "2025-12-18T20:33:36+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T22:24:52.050374+00:00"
content_hash: "sha256:99bdd28d7d38638f52e49565cf8d5990a728f5dee7afb9c0b166ddc6b312e1fe"
---

# Whatnot Engineering’s 2025 Blog Highlights

Data Science


Machine Learning


Software Engineering


Startup


Productivity


# *Whatnot Engineering’s 2025* Blog *Highlights*


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--e7a63dbd0057---------------------------------------)


3 min read


·


Dec 18, 2025


--


Press enter or click to view image in full size


In 2025, Whatnot’s marketplace more than doubled in size. By October, sellers had already moved over $6B worth of goods, up from $3B for all of 2024. Fueled by this incredible growth, the Whatnot engineering team continued building one of the fastest‑growing marketplaces in the world.


Some highlights from the year:


- In October, we announced our $225M Series F, more than doubling our valuation to $11.5B
- Black Friday was our biggest ever: over $75M in global GMV, nearly 3x year over year. At peak, buyers were purchasing 125 items per second, and one show passed $1M in a single 10-hour stream
- Every day, sellers on Whatnot create more than 5 million product listings to share what they love with the world


### On the engineering side, 2025 was a year defined by three things:


- The demands of scaling a real-time marketplace operating at massive global scale
- Delivering intelligent, real-time recommendations and seamless buyer–seller experiences
- Using AI to supercharge developer velocity


Below are the engineering stories that shaped our year.


## Hard Engineering Problems: Managing high impact, high demand events


At Whatnot, big events aren’t anomalies, they’re the norm. Our marketplace regularly experiences sudden surges when a show goes viral, a seller drops a rare item, or a community moment captures the platform’s attention. Building for this means engineering every system, ranking, retrieval, notifications, payments, chat, and streaming, to operate reliably under 3x spikes without compromising the real-time magic that defines Whatnot. Our philosophy is simple: *design for the peak, not the average* , so that every seller and buyer can trust the platform during their most important moments.


- [Whatnot’s Data-Driven Approach to Scalability & Reliability for Big On-Platform Events](https://medium.com/whatnot-engineering/whatnots-data-driven-approach-to-scalability-reliability-for-big-on-platform-events-166c0cceb171)
- [Expecting the unexpected: Managing 3x traffic surges at Whatnot](https://medium.com/whatnot-engineering/expecting-the-unexpected-managing-3x-traffic-surges-at-whatnot-c8c3d3f76891)
- [Preparing the Marketplace for Game Time](https://medium.com/whatnot-engineering/preparing-the-marketplace-for-game-time-fef70df55a09)


## Real-Time Intelligence at Scale


Our buyer and seller communities tell us what’s hard, whether it’s discovering the right shows, managing complex back-office workflows, or scaling a seller business. And increasingly, AI is the best tool to address those problems effectively. Because our marketplace runs on real-time signals, ephemeral video, and rapidly shifting buyer intent, we’ve spent years building the infrastructure that can support high-performance, low-latency AI at scale. This foundation lets us ship AI-powered listing tools, real-time ranking improvements, and developer productivity accelerators that make Whatnot faster, smarter, and more delightful for buyers, sellers, and internal teams alike.


### Feed ranking


- [Whatamix: Blendable feed construction](https://medium.com/whatnot-engineering/whatamix-blendable-feed-construction-2c94c21f6635)
- [Evolving Feed Ranking at Whatnot](https://medium.com/whatnot-engineering/evolving-feed-ranking-at-whatnot-25adb116aeb6)
- [6x Faster ML Inference: Why Online≫Batch](https://medium.com/whatnot-engineering/6x-faster-ml-inference-why-online-batch-16cbf1203947)


## Improving the developer experience with AI


AI tooling has also helped improve the software engineering experience at Whatnot. There are countless menial chores in software engineering, such as cleaning up feature flags, deprecating old code, or creating admin interfaces, and we’re exploring new ways to speed up these tasks with AI.


Readers loved the recent post about eliminating GraphQL schema bloat. Rather than ask engineers to spend hours manually validating and removing fields, we built an AI subagent that uses real traffic data to identify fields unused for 30+ days, then follows the same step s a human would: remove the field, clean up dead code, and update tests. We wired this into a GitHub Action that runs on a schedule, automatically removing one unused field at a time.


### AI Workflows


- [Eliminating GraphQL Schema Bloat with AI (So You Don’t Have To)](https://medium.com/whatnot-engineering/eliminating-graphql-schema-bloat-with-ai-so-you-dont-have-to-5f6ae84d0ee1)
- [From low code to vibe code: how we bridged the gap for internal tools](https://medium.com/whatnot-engineering/from-low-code-to-vibe-code-how-we-bridged-the-gap-a71bd3b11ddc)


## Conclusion


Thank you for reading our engineering blog this year! We’re excited to share more behind-the-scenes stories from Whatnot engineers in 2026: stories about AI-native systems, real-time commerce, and the teams building the future of community-driven shopping.


If tackling large-scale engineering challenges like these excites you, we’re hiring. Join us in building the future of social commerce at[whatnot.com/careers](http://whatnot.com/careers) .
