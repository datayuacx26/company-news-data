---
schema_version: "1.0.0"
document_id: "16f4157dd5204e7ad6d15ac7629c9171eb32dbda47a1e834ac8499cfa63cda44"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-cloudtrail-filter-useridentity-advance-selectors/"
published_at: "2026-07-20T17:34:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:2504e913b5cad295c6e1a3d19f1e12e3b84ede18626f58e597b213904db93dc1"
---

# Selectively log network activity events by identity in AWS CloudTrail

Today, AWS launches enhanced event filtering for network activity events for VPC end points, a CloudTrail event type that captures actions transmitted through a[Virtual Private Cloud Endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html) . Customers can now control which network activity events are logged based on the IAM user identity making the API call. For example, you can configure selectors to log only access denied events when the calling user identity is not on a known safe list. This lets you capture unauthorized access attempts while excluding routine traffic from trusted identities, reducing both logging costs and noise.


With UserIdentity filtering, customers building a data perimeter strategy can focus on network activity event logging for scenarios that matter most in security. You can configure selectors to log only VpceAccessDenied events from identities outside a trusted set of IAM roles. This enables detection of potential data exfiltration attempts through VPC endpoints without the cost of logging every successful API call from approved principals. You can combine UserIdentity conditions with existing fields like eventName or vpcEndpointId for fine-grained control over what gets recorded.


You can use this feature via the AWS Management Console, AWS Command Line Interface, and AWS SDKs. This feature is available in all AWS Regions where CloudTrail network activity events are supported. To learn more about Network Activity events, visit the[AWS CloudTrail user guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) or read[AWS Blog on how to enable Network Activity Events](https://aws.amazon.com/blogs/mt/announcing-aws-cloudtrail-network-activity-events-for-vpc-endpoints/) .
