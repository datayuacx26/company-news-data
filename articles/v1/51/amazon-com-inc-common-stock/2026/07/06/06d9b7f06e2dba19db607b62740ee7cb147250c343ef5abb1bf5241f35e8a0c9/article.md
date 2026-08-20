---
schema_version: "1.0.0"
document_id: "06d9b7f06e2dba19db607b62740ee7cb147250c343ef5abb1bf5241f35e8a0c9"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-lambda-managed-instances-logs/"
published_at: "2026-07-24T16:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:953fd33addf07ed3c9796a11b198c3886ad315566bd4491efca92b0e91908c82"
---

# AWS Lambda now publishes logs for Lambda Managed Instances capacity providers

AWS Lambda now publishes logs for Lambda Managed Instances (LMI) capacity providers to Amazon CloudWatch Logs, giving you visibility into scaling activity and instance lifecycle operations. LMI enables you to run Lambda functions on Amazon EC2 instances while maintaining serverless operational simplicity. Capacity providers are resources that let you define compute resources that Lambda provisions on your behalf. With capacity provider logs, you can monitor, troubleshoot, and optimize these managed EC2 instances, helping you quickly diagnose provisioning issues and understand scaling behavior.


Customers use LMI to operate high-volume, predictable workloads with specialized compute configurations and achieve cost efficiency through EC2 pricing options like Savings Plans and Reserved Instances. With this launch, Lambda automatically generates logs for compute resources managed by capacity providers and delivers them to CloudWatch Logs. Lambda publishes structured JSON logs capturing instance lifecycle events like launches, terminations, and health checks. This structured format lets you identify failed operations and provisioning errors through CloudWatch Logs filtering, helping you resolve issues quickly and shorten debugging cycles.


The capacity provider logs are available in all AWS Commercial Regions where LMI is available. The logs are enabled by default for all capacity providers. You can view your capacity provider logs by visiting the Lambda console's capacity provider page. You can use the Lambda API, Lambda console, AWS CLI, AWS SAM, or AWS CloudFormation to change capacity provider log configuration. Standard Amazon CloudWatch Logs charges apply. To learn more, visit the AWS Lambda Managed Instances[product page](https://aws.amazon.com/lambda/lambda-managed-instances/) and[documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-managed-instances.html) .
