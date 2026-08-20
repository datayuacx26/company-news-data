---
schema_version: "1.0.0"
document_id: "fb3f43f1f5e6cc5dd7f6461362e18e1ec0168ce635c91353df58ff58df29e37f"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/05/smus-data-profiling"
published_at: "2026-08-18T18:49:00+00:00"
first_seen_at: "2026-08-18T22:19:11.708664+00:00"
fetched_at: "2026-08-18T22:19:13.543416+00:00"
content_hash: "sha256:4d768673d20de83b09a1cf8b0358d62785c6e72b216f94016c7cf5509c7741d8"
---

# Amazon SageMaker Unified Studio now supports data profiling and anomaly detection

Amazon SageMaker Unified Studio now supports data profiling and anomaly detection, powered by AWS Glue Data Quality. Data stewards, engineers and analysts can generate statistical profiles of their data to understand its shape and completeness, and track how these statistics change over time. Anomaly detection helps identify when data points drift from historical patterns without requiring predefined thresholds or custom rules. These capabilities are available for both data at rest in catalog tables and data in transit within Visual ETL jobs.


With this launch, a dedicated Data profile tab on catalog tables provides on-demand and scheduled profiling that computes dataset-level and column-level statistics. As profile history accumulates, anomaly detection builds a baseline of expected behavior and flags data points that fall outside the predicted range. This is particularly useful when you may not be aware of specific thresholds, or when expected values change over time and fixed rules could become stale. For data in transit, the same profiling statistics and anomaly detection are available on the results page of any Visual ETL job with an Evaluate Data Quality transform.


This feature is available in all AWS Regions where Amazon SageMaker Unified Studio is available. To learn more, visit the[Amazon SageMaker Unified Studio documentation.](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/data-profiling-catalog.html)
