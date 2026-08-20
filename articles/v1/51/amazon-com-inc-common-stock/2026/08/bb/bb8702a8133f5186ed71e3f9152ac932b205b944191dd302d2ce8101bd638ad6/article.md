---
schema_version: "1.0.0"
document_id: "bb8702a8133f5186ed71e3f9152ac932b205b944191dd302d2ce8101bd638ad6"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-dynamodb-vector-search"
published_at: "2026-08-05T16:59:00+00:00"
first_seen_at: "2026-08-05T19:58:05.832601+00:00"
fetched_at: "2026-08-05T19:58:07.748988+00:00"
content_hash: "sha256:d30fe0fa6f82a593b181cd79e8598d70288b5f07dd5d4855f53e0dc3386dfa65"
---

# Amazon DynamoDB now supports real-time vector search

# Amazon DynamoDB now supports real-time vector search


Posted on: Aug 5, 2026


Today, AWS announces the general availability of vector search for Amazon DynamoDB, a new feature to index and search vectors in real time. As vector datasets grow into the billions or trillions, vector search at scale traditionally trades off search speed, scale, and accuracy: latency climbs with vector count unless you accept lower recall or throughput. DynamoDB now supports native vector search with single-digit millisecond latency at 99%+ recall and is designed for any scale, even trillions of vectors.


With DynamoDB vector search, you store vector embeddings alongside your other attributes and generate them using a model of your choice, including models available on Amazon Bedrock. You create a vector index and run approximate nearest neighbor searches, pick the vector index partition key to scale, and filter on attributes to scope results. You get the same serverless benefits you rely on today: zero infrastructure management, zero downtime, zero maintenance windows, and pay for only what you use. You can already use DynamoDB to store memory for AI agents, and with vector search you can now add semantic retrieval over that memory for agentic grounding, along with product similarity search, personalized advertising, retrieval augmented generation, and recommendation systems, with predictable performance.


To learn more, visit the[AWS News Blog](https://aws.amazon.com/blogs/aws/amazon-dynamodb-now-supports-real-time-vector-search-at-any-scale) ,[Amazon DynamoDB product page](https://aws.amazon.com/dynamodb/) , and[Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/VectorSearch.html) .


DynamoDB vector search is available in all commercial AWS Regions and AWS GovCloud (US) Regions.
