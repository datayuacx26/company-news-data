---
schema_version: "1.0.0"
document_id: "25dce418068dc0efbf1ee00492f343e49f3bbe539689a0be083fe8cf0498344b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-opensearch-service/"
published_at: "2026-07-30T02:30:15+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:2c668fa7c3b232ed5dededaa6f4576374ebc96ee4c43b6437a82b0227464829f"
---

# Amazon OpenSearch Service now supports OpenSearch version 3.7

You can now run OpenSearch version 3.7 on Amazon OpenSearch Service. OpenSearch 3.7 introduces improvements in vector search performance, search relevance, and Query Insights.


With this launch,[1-bit scalar quantization](https://docs.opensearch.org/latest/vector-search/optimizing-storage/faiss-scalar-quantization/#1-bit-quantization) on the Faiss and Lucene engines compresses vectors, reducing the storage and memory required by vector workloads while maintaining search accuracy. You can now retrieve vectors faster using[doc values](https://docs.opensearch.org/latest/vector-search/performance-tuning-search/#retrieve-vectors-using-doc-values/) instead of document source, with no reindexing required.[Search Relevance Workbench](https://docs.opensearch.org/latest/search-plugins/search-relevance/using-search-relevance-workbench/) adds new evaluation metrics, CSV judgment uploads, and expanded hybrid search optimization, helping you measure and improve search quality.


This launch also introduces new Query Insights capabilities, including automated query recommendations, a finished-queries cache for observing recently completed queries, and the option to export top query data to Amazon S3, helping you identify expensive queries and analyze trends over time.


For information on upgrading to OpenSearch 3.7, please see the[documentation](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/version-migration.html) . OpenSearch 3.7 is now available in all AWS Regions where Amazon OpenSearch Service is available.
