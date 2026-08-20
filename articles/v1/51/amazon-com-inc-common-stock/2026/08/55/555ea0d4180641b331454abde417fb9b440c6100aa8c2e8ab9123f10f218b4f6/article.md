---
schema_version: "1.0.0"
document_id: "555ea0d4180641b331454abde417fb9b440c6100aa8c2e8ab9123f10f218b4f6"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-iam-european-sovereign-cloud/"
published_at: "2026-08-18T22:30:00+00:00"
first_seen_at: "2026-08-19T00:16:28.993170+00:00"
fetched_at: "2026-08-19T00:16:30.402166+00:00"
content_hash: "sha256:90c45f105cb11e15ed0af2d2bae73d77e504c2998377f2c9e7a0ad4fcbf6cf9d"
---

# AWS IAM identity federation to external services is now available in AWS European Sovereign Cloud Region

AWS Identity and Access Management (IAM) now enables AWS workloads in the AWS European Sovereign Cloud (Germany) Region to securely authenticate with external services using short-lived JSON Web Tokens (JWTs). The AWS European Sovereign Cloud is an independent cloud for Europe entirely located within the European Union (EU), designed to help customers meet their evolving sovereignty requirements.


With outbound identity federation, AWS workloads can securely authenticate with third-party cloud providers, SaaS providers, and self-hosted applications without using long-term credentials or implementing complex workarounds. Customers can exchange their AWS IAM credentials for cryptographically signed, short-lived JWTs, providing a simple and secure mechanism to access external services. These tokens contain rich context about the AWS workloads, enabling external services to implement fine-grained access control. Administrators can control access to token generation and enforce token properties (such as lifetime, audience, and signing algorithms) using IAM policies and audit token usage using CloudTrail logs, allowing them to meet their organization's security and compliance requirements.


To learn more, visit the[outbound identity federation product page](https://aws.amazon.com/identity/federation/outbound-federation/) , see the[IAM user guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_outbound.html) , or read the[AWS News Blog Post](https://aws.amazon.com/blogs/aws/simplify-access-to-external-services-using-aws-iam-outbound-identity-federation/) .
