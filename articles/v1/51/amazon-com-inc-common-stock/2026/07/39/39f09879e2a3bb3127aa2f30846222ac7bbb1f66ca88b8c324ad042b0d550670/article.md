---
schema_version: "1.0.0"
document_id: "39f09879e2a3bb3127aa2f30846222ac7bbb1f66ca88b8c324ad042b0d550670"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-redshift-data-api-longpolling-listsession-flexiblebatchexecute/"
published_at: "2026-07-29T20:45:00+00:00"
first_seen_at: "2026-07-29T21:38:06.257307+00:00"
fetched_at: "2026-07-29T21:38:07.883784+00:00"
content_hash: "sha256:c0606660c3fbc5a958ae0846d307016e1304394d8f4d81ce7004ec65a1276b5a"
---

# Amazon Redshift Data API announces long polling, session management, and flexible batch execution

Amazon Redshift Data API introduces new capabilities that reduce the number of API calls to retrieve SQL statement metadata or results, provide visibility into sessions, and allow batch statements to execute on separate transactions.


Long polling: Long polling enables you to retrieve SQL statement metadata or results without polling repeatedly until the SQL statement reaches a terminal state, by delaying returning a synchronous response until the SQL statement finishes. To use this feature, specify the WaitTimeSeconds parameter on ExecuteStatement, BatchExecuteStatement, DescribeStatement, GetStatementResult, or GetStatementResultV2.


ListSessions: Applications that reuse sessions across multiple queries can now enumerate active sessions and filter by status, compute target, or database, eliminating the need to track session identifiers externally.


Flexible batch execution: BatchExecuteStatement now supports an ExecutionMode parameter with AUTO_COMMIT mode, allowing each SQL statement in a batch to execute independently so a single failure no longer rolls back the entire batch — useful for ETL pipelines and administrative scripts where partial completion is acceptable. In addition, BatchExecuteStatement now accepts an array of SqlParameter, enabling parameter reuse across all statements in a batch: define parameters once and reference them in any statement, eliminating the need to embed literal values in each query.


These features are generally available for Amazon Redshift Provisioned and Amazon Redshift Serverless in all AWS commercial and AWS GovCloud (US) Regions that support Amazon Redshift Data API. To get started, visit the[Amazon Redshift Data API developer guide](https://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html) .
