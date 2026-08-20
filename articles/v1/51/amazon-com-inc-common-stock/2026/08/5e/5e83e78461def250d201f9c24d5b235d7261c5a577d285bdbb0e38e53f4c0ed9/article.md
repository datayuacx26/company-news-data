---
schema_version: "1.0.0"
document_id: "5e83e78461def250d201f9c24d5b235d7261c5a577d285bdbb0e38e53f4c0ed9"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-quick/"
published_at: "2026-08-06T06:54:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:e4abc2c7f4f8565b7509b119b6c2534a9e4cf7a0294d3c02223ccc4e05d3d199"
---

# Amazon Quick supports multi-dataset analytical capabiity

Today, Amazon Quick announces supporting multi-dataset topics, enabling users to model relationships across multiple datasets in a single topic and use that model to build dashboards and answer questions in natural language. Previously, answering a question in a Quick Sight visual that spanned datasets required pre-joining the data into a single dataset—adding manual JOIN logic in the data preparation, consuming extra SPICE capacity, and users have to rebuild multiple datasets based on different use cases or when the model changed. With multi-dataset topics, a topic becomes a reusable relational data model: users add multiple datasets, define the relationships once, and Quick performs the joins at runtime.


The new capability applies for both dashboard building and natural-language Q&A. For dashboard building, the topic acts as the data model, so a single visual can draw fields from multiple datasets and Quick generates the underlying join automatically—users author the semantic model once and reuse it across every visual in the dashboard. For natural-language analytics, users can point a chat agent directly at a topic and ask questions; the agent reads the relationships defined in the topic and performs runtime joins across datasets to answer, with no need to pre-join tables or prepare data first. Because both experiences query toward the same topic, one governed semantic model serves as the single source of truth for people and agents alike. Multi-dataset topics reuse existing dataset permissions and support row-level and column-level security (RLS/CLS), so established governance carries through every cross-dataset visual and every answer.


Multi-dataset topics are now generally available in all AWS Regions[where Amazon Quick is available](https://docs.aws.amazon.com/quick/latest/userguide/regions.html#regions-qs) . To get started, see this[blog post](https://aws.amazon.com/blogs/machine-learning/build-a-unified-semantic-layer-across-datasets-with-multi-dataset-topics-in-amazon-quick/) .
