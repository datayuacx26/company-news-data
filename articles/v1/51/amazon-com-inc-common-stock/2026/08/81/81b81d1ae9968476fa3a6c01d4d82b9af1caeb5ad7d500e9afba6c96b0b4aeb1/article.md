---
schema_version: "1.0.0"
document_id: "81b81d1ae9968476fa3a6c01d4d82b9af1caeb5ad7d500e9afba6c96b0b4aeb1"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/mwaa-serverless-pythonoperator-bashoperator/"
published_at: "2026-08-18T18:00:00+00:00"
first_seen_at: "2026-08-18T22:19:11.708664+00:00"
fetched_at: "2026-08-18T22:19:13.543416+00:00"
content_hash: "sha256:1a35c374dd4b0f64ef98869c4ff530f91a1c9f068c123b64a618b78697079261"
---

# Amazon MWAA Serverless now supports PythonOperator and BashOperator

Amazon Managed Workflows for Apache Airflow (Amazon MWAA) Serverless now supports running custom Python functions and shell scripts directly in the serverless runtime using PythonOperator and BashOperator. With this launch, data engineering teams can execute the code patterns they rely on daily, including data transformations, format conversions, and data quality checks, without provisioning additional infrastructure.


Package your Python modules or shell scripts as code packages, upload them to Amazon S3, and reference them when creating or updating a workflow. The service snapshots your code at workflow creation time and uses that snapshot for all subsequent runs, ensuring consistency across executions.


This feature is available in all AWS Regions where Amazon MWAA Serverless is available. To learn more, visit[Using Python and Bash operators](https://docs.aws.amazon.com/mwaa/latest/mwaa-serverless-userguide/operators-python-bash-detail.html) .
