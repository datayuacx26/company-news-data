---
schema_version: "1.0.0"
document_id: "e5ef8655b171641944c654bbdb6a9a803950ff19528bac4fe53d9a17ba64abac"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-glue-data-quality-distribution-profiling"
published_at: "2026-07-27T20:23:00+00:00"
first_seen_at: "2026-07-28T00:43:02.218346+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:68e47a9610ed66400f2ec53e5aabfa5349f5baace808f3d0caf5a6fb525495db"
---

# AWS Glue Data Quality now supports distribution statistics for data profiling

AWS Glue Data Quality now supports a new Distribution Analyzer that generates frequency distribution profiles for your data. Using this new Distribution Analyzer in the Data Quality Definition Language (DQDL), you can generate histograms for numeric columns and value distributions for categorical, date, and boolean columns. With support for custom bin counts, you can explore the shape and patterns of your data at the granularity that matters most to your use case.


Understanding how data is distributed is foundational to building reliable data pipelines. Distribution statistics help you quickly identify skewness, outliers, and unexpected patterns across your datasets, without writing custom code. The capability integrates directly with your existing DQDL rulesets, so you can add distribution profiling alongside your current data quality checks in a single evaluation run. Distribution statistics are stored in Amazon S3 for future querying through services like Amazon Athena, and are also surfaced through APIs, making it easy to integrate distribution insights into monitoring workflows and visualization tools, including SageMaker Unified Studio.


AWS Glue Data Quality distribution statistics are available in all AWS commercial regions and AWS GovCloud (US) regions.


To learn more about Glue Data Quality, visit the[AWS Glue Data Quality documentation](https://docs.aws.amazon.com/glue/latest/dg/glue-data-quality.html) . To get started with using Distribution Analyzer, visit the[Analyzers documentation](https://docs.aws.amazon.com/glue/latest/dg/dqdl.html#dqdl-analyzers) .
