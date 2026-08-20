---
schema_version: "1.0.0"
document_id: "c55b4545c59651b6409aae0a1333c77105efb0328d07fcb1d7f5b8918e578710"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ecr-image-layers/"
published_at: "2026-08-03T15:00:00+00:00"
first_seen_at: "2026-08-03T16:24:10.172945+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:e154cb2d6cf34e1cee0868dfa67c6574babb7cd73e7dc5e83e231f42df59f569"
---

# Amazon ECR now supports image layers up to 200 GB

Amazon Elastic Container Registry (Amazon ECR) has increased the maximum image layer size limit to 200 GB, for images pushed via Docker push.


Previously, packaging assets required splitting data across multiple layers or offloading to external storage systems. With this update, customers can store up to 200 GB in a single image layer, eliminating extra complexity for use cases like embedding large language models, bundling genomics datasets, or packaging large binary dependencies directly into your container images. Images pushed using the AWS SDK or CLI; (UploadLayerPartAPI) remain limited to 50 GB.


This feature is available in all AWS Regions and partitions where Amazon ECR is available except the Middle East (Bahrain) and Middle East (UAE) Regions. To learn more, visit the[Amazon ECR product page](https://aws.amazon.com/ecr/) and refer to the[Amazon ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) . For pricing information, see the[Amazon ECR pricing page](https://aws.amazon.com/ecr/pricing/) .
