---
schema_version: "1.0.0"
document_id: "26932efcb839417e37481484eb8bb4aa07f01e4614df00db35e0856052f28798"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-expands-iam-principal-cost-allocation-bedrock-mantle/"
published_at: "2026-08-11T20:25:00+00:00"
first_seen_at: "2026-08-11T22:31:48.620839+00:00"
fetched_at: "2026-08-11T22:31:51.421235+00:00"
content_hash: "sha256:6d2354f014f51db31fa83e69749c41538fe04223fed7cac2b9af97f244ee4b81"
---

# Amazon Bedrock expands IAM principal cost allocation to the bedrock-mantle endpoint

Amazon Bedrock is a fully managed service that provides secure, enterprise-grade access to high-performing foundation models from leading AI companies, enabling you to build and scale generative AI applications. Amazon Bedrock now supports cost allocation by AWS Identity and Access Management (IAM) principal, including IAM users and roles, for model inference requests made through the bedrock-mantle endpoint. This extends the capability previously available for the bedrock-runtime endpoint, helping customers attribute inference costs across users, teams, projects, and applications.


Customers can tag IAM users and roles with attributes such as team, project, or cost center, activate them as cost allocation tags, and analyze bedrock-mantle inference costs by those tags in AWS Cost Explorer or at the line-item level in AWS Cost and Usage Report 2.0 (CUR 2.0). To get started, activate your IAM principal tags in the AWS Billing and Cost Management console. Then filter or group costs by those tags in Cost Explorer, or create a CUR 2.0 data export and select Include caller identity (IAM principal) allocation data.


This feature is available in all AWS Regions where the bedrock-mantle endpoint is available. To learn more, see[Using IAM principal for cost allocation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/iam-principal-cost-allocation.html) and[IAM principal attribution in Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/cost-mgmt-iam-principal-tracking.html) .
