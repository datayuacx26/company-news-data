---
schema_version: "1.0.0"
document_id: "cabd6988021c7d9b63a9037eee1e45e461bdd54edf241ef86b7877cb842828b5"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/run-ai-wherever-your-compliance-framework-demands"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-24T04:07:24.248205+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:e3d5cc88fa8f8015435f0d4a75c8218bb4c9545430c829443dcaaad88c36f12e"
---

# Run AI Wherever Your Compliance Framework Demands

The CTOs I talk to in regulated industries aren’t debating whether to build with AI. That decision has been made. What they're navigating is a harder question: how do you build AI at the standard your compliance framework requires, when most of the tooling was designed for environments you're not allowed to use?


That's not a hypothetical constraint. Banks, healthcare providers, insurance firms, and telecommunications companies face strict mandates over where their data resides and who has legal jurisdiction over it. For many of them, certain workloads can't be run in the public cloud. And until recently, that meant accepting a gap between what AI could do for them and what it could do for organizations running similar use cases without regulatory constraints.


Today at MongoDB .local Bengaluru, we introduced capabilities to help customers close that gap.


## **Compliance shouldn't determine your AI capabilities**


The assumption that every enterprise workload will eventually migrate to the public cloud is being challenged by the way regulated industries actually operate. Cost, latency, and data residency mandates don't bend for cloud migration timelines. Many of our customers run[MongoDB Atlas](https://www.mongodb.com/products/platform) across cloud providers and[MongoDB Enterprise Advanced](https://www.mongodb.com/products/self-managed/enterprise-advanced) on premises simultaneously. They need a platform that works consistently across both, without forcing a choice between infrastructure control and modern AI capabilities.


[Search](https://www.mongodb.com/products/platform/atlas-search) **and**[Vector Search](https://www.mongodb.com/products/platform/atlas-vector-search) **are now generally available as an add-on for MongoDB Enterprise Advanced.** These are the same capabilities that have powered semantic and hybrid retrieval in MongoDB Atlas for years, now available to teams running their own infrastructure, so AI-ready retrieval is within reach, whether a workload sits in Atlas or on premises.


Full-text search, vector search, and hybrid retrieval run next to the database, with no need to stand up separate search engines or standalone vector databases alongside the operational store. Because it's a single architecture across every footprint, search behaves the same on-premises, in a private cloud, or across hybrid deployments. Customer data is designed to stay within the environment the enterprise controls, without requiring external synchronization pipelines or public internet exposure to move it.


## **Compliance-first, production-ready**


****[Xlrt](https://www.mongodb.com/solutions/customer-case-studies/xlrt) builds contract and procurement analysis platforms for banks, handling large volumes of confidential customer data that can't leave the environments their clients control. For financial institutions under strict data residency requirements, that constraint isn't negotiable, so Xlrt chose MongoDB Enterprise Advanced: reliability and robust performance were requirements for their banking clients—not preferences—and self-managed deployment meant sensitive data stayed within the client's environment.


As a result of choosing MongoDB Enterprise Advanced, Xlrt reduced procurement processes from 15 days to just three, inside the environment the bank controls. With Search and Vector Search now available for Enterprise Advanced, the semantic retrieval Xlrt has planned for its platform can run inside that same compliant infrastructure, rather than requiring a separate system alongside it.


Xlrt is one of many. Ahead of this release, more than 20 of the world's largest banks and financial institutions have been evaluating Search for Enterprise Advanced, drawn by the same thing: AI-ready retrieval that runs inside the infrastructure they control. Companies today need one AI stack, no matter where their data lives, with one platform, one API, and one set of developer skills. Enterprise Advanced now brings the same proven search and vector capabilities that Atlas customers rely on into the environments they control.


## **One foundation, managed or self-managed**


**Search and Vector Search are also now generally available for MongoDB Community Edition** , giving developers in self-managed environments full-text search, vector search, and hybrid retrieval at zero cost and with far less complexity. When those workloads are ready to scale, they move to MongoDB Atlas for fully managed infrastructure or to MongoDB Enterprise Advanced for self-managed, enterprise-grade deployments, with minimal application rework.


Capabilities that used to require the cloud now run on self-managed MongoDB, giving customers real choice over where they build and run AI. A developer experimenting locally on MongoDB Community Edition and a regulated enterprise running production AI behind a compliance firewall build on the same foundation. What differs is the operational model they choose: Atlas delivers a proven platform with the services and scale of managed cloud infrastructure, while Enterprise Advanced and Community Edition put control of the environment in the customer's hands.


Compliance infrastructure only matters if the AI running on it is accurate. My colleague Pablo Stern-Plaza covers what we shipped on that front. Read his post:[Retrieval Accuracy Is Now a Competitive Advantage.](https://www.mongodb.com/company/blog/product-release-announcements/retrieval-accuracy-is-now-competitive-advantage)
