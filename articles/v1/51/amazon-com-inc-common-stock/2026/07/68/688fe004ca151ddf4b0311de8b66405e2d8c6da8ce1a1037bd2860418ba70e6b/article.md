---
schema_version: "1.0.0"
document_id: "688fe004ca151ddf4b0311de8b66405e2d8c6da8ce1a1037bd2860418ba70e6b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-gamelift-streams-iam/"
published_at: "2026-07-17T17:29:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:943f01f95b4901937c9b8b41d5c312953365404f6a8336213c3a7762f8954e06"
---

# Amazon GameLift Streams now supports IAM role credentials for stream sessions

Amazon GameLift Streams now supports assigning an IAM role to a stream session, enabling your application to securely access resources in your AWS account, such as Amazon S3 buckets and DynamoDB tables. With this launch, you can pass a RoleArn parameter when starting a stream session, and your application automatically receives short-lived, auto-refreshing AWS credentials through the standard AWS SDK credential resolution chain — no application code changes required.


Previously, customers who needed their streamed applications to access AWS services had to embed long-lived access keys in application bundles or pass them as environment variables, creating security and operational challenges. Now, Amazon GameLift Streams handles credential vending and automatic refresh using the same container credential provider mechanism trusted by Amazon ECS task roles and Amazon EKS Pod Identity. Role misconfigurations are validated at session start, surfacing clear errors immediately rather than during runtime.


You can also configure IAM roles directly in the Amazon GameLift Streams console, which provides a pre-filled trust policy template to simplify role setup.


IAM role support for stream sessions is available in all AWS Regions where Amazon GameLift Streams is available.


To learn more, see Session Credentials Setup in the Amazon GameLift Streams Developer Guide:[https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/session-credentials-setup.html](https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/session-credentials-setup.html)
