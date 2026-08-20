---
schema_version: "1.0.0"
document_id: "a87d8c55a0643193713e5e0760590017d1d8f2f4b6c6fc52dd65cc4829ad3193"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-eks-oidc-endpoint-privatelink"
published_at: "2026-07-27T18:00:00+00:00"
first_seen_at: "2026-07-28T23:43:23.890345+00:00"
fetched_at: "2026-07-28T23:43:26.468+00:00"
content_hash: "sha256:5d917a961f6a93e4c2254473efcd4ba065a615de3bad8ca5cc46e166aeee3589"
---

# Amazon EKS now supports AWS PrivateLink for the cluster OIDC endpoint

[Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS) now supports[AWS PrivateLink for the cluster OIDC discovery and JWKS endpoint](https://docs.aws.amazon.com/eks/latest/userguide/vpc-interface-endpoints.html#oidc-vpc-interface-endpoints) . You can now reach the endpoint used by[IAM roles for service accounts (IRSA)](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html) privately from your VPC without requiring internet egress.


Each EKS cluster publishes public signing keys at its OIDC endpoint for IRSA. With AWS PrivateLink for the cluster OIDC endpoint, tools running inside your VPC, such as eksctl, Terraform, or custom token validators, can now reach the OIDC discovery document and JWKS privately by creating an interface VPC endpoint for the com.amazonaws.<region>.oidc-eks service. This enables IRSA setup and token validation in VPCs without internet egress and ensures correct DNS resolution when the EKS management VPC endpoint is enabled with private DNS.


AWS PrivateLink for the cluster OIDC endpoint is available at no additional cost beyond standard[AWS PrivateLink pricing](https://aws.amazon.com/privatelink/pricing/) in all AWS Regions where Amazon EKS is available. To get started, see[Access the cluster OIDC endpoint using AWS PrivateLink](https://docs.aws.amazon.com/eks/latest/userguide/vpc-interface-endpoints.html#oidc-vpc-interface-endpoints) in the Amazon EKS User Guide.
