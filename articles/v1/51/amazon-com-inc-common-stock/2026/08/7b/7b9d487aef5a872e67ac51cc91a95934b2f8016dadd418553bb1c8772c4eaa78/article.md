---
schema_version: "1.0.0"
document_id: "7b9d487aef5a872e67ac51cc91a95934b2f8016dadd418553bb1c8772c4eaa78"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-sagemaker/"
published_at: "2026-08-19T20:25:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:ace1425dd19ee9127f22e003e74cbf629b9dd5a3e495bc6472ea7048aaaa3c62"
---

# Amazon SageMaker notebooks now support trusted identity propagation

Amazon SageMaker Notebooks now support Trusted Identity Propagation (TIP) with Amazon Athena, Amazon Redshift, and Amazon EMR Serverless, enabling per-user access control for data analytics.


When connected to a TIP-enabled compute in a TIP-enabled Project, each notebook user's IAM Identity Center identity flows through to AWS Lake Formation, ensuring they see only the tables, columns, and rows their permissions allow, without sharing a single broad execution role. With TIP, enterprises get per-user data boundaries enforced based on who is running the query, full audit attribution with CloudTrail recording which user accessed data, and reduced admin friction since identity propagates automatically through the existing compute connection with no extra login, token, or role management required.


To get started, use a notebook in a TIP enabled Project with the supported engines. This feature is available in[all AWS Regions where Amazon SageMaker Unified Studio is available](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/supported-regions.html) . To learn more, see[Trusted identity propagation](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/trusted-identity-propagation.html) in the Amazon SageMaker Unified Studio Administrator Guide and[Notebooks](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/notebooks.html) in the Amazon SageMaker Unified Studio User Guide.
