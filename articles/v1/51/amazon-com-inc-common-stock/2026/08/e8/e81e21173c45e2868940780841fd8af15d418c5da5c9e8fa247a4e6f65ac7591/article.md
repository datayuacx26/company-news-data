---
schema_version: "1.0.0"
document_id: "e81e21173c45e2868940780841fd8af15d418c5da5c9e8fa247a4e6f65ac7591"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/opensearch-ui-network-access-control"
published_at: "2026-08-06T18:16:00+00:00"
first_seen_at: "2026-08-08T00:14:24.420484+00:00"
fetched_at: "2026-08-08T00:14:26.683148+00:00"
content_hash: "sha256:6ed252c157c7c8a1bdfdc373ec5c6491ad2aad77384df2860e34935495c3cd97"
---

# Amazon OpenSearch UI now supports Network Access Control

Amazon OpenSearch Service now supports network access controls for OpenSearch UI applications. OpenSearch UI is the fully managed web service for search, analytics, and unified observability across multiple AWS data sources. With network access controls, you can restrict access to your OpenSearch UI applications to approved networks using the same IAM condition keys (aws:SourceVpce, aws:SourceVpc, and aws:SourceIp) that you already use elsewhere in AWS, helping you establish a consistent data perimeter across your environment.


You can enforce network restrictions at three levels: identity-based policies for specific principals, VPC endpoint policies to control which applications users reach through an endpoint, and resource control policies (RCPs) to enforce access uniformly across every account in your AWS organization. With RCPs, you can block off-network users before they authenticate, preventing anyone outside your corporate network or VPC from reaching the login page.


Network access controls are available in all AWS Regions where OpenSearch UI is available. To learn more, see[Restricting network access to OpenSearch UI applications](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/application-network-access.html) in the *Amazon OpenSearch Service Developer Guide* . For more information about Amazon OpenSearch Service, see the[Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/) product page.
