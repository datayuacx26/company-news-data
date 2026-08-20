---
schema_version: "1.0.0"
document_id: "d0b40c6ed69e42f8704dead344fe3e32fc6f15423902beed7e1aa83870423e2d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/17/amazon-msk-custom-domain-names/"
published_at: "2026-08-17T18:01:00+00:00"
first_seen_at: "2026-08-18T01:21:09.373952+00:00"
fetched_at: "2026-08-18T01:21:12.465123+00:00"
content_hash: "sha256:bdabcfecaf1706bf14080a9de3dfcf5936c62fce4cf64f14686875296267accd"
---

# Amazon MSK now supports configuring custom domain names for MSK Provisioned clusters

You can now easily configure custom domain names on[Amazon MSK Provisioned](https://aws.amazon.com/msk/) clusters, on either ZooKeeper or KRaft mode for metadata management. This capability helps client applications maintain the same connection endpoints, simplifying cluster migrations, disaster recovery failovers, and scaling operations without reconfiguration.


Previously, customers configured custom domain names manually on each broker. Additionally, on KRaft-based clusters, customers could not configure custom domain names. With this launch, you can now easily define a custom domain once at the cluster level, and Amazon MSK automatically applies it to every broker in the cluster, eliminating the need to configure each broker individually. The configuration persists through scaling operations and works identically on both ZooKeeper and KRaft-based clusters. This is particularly useful for customers who route traffic through Network Load Balancers, require persistent endpoints across cluster operations, or must adhere to organizational naming conventions for security and compliance.


You can configure custom domain names on all new and existing MSK Provisioned clusters, in all AWS Regions where Amazon MSK Provisioned is available, at no additional cost. To learn more, see the[Amazon MSK Developer Guide](https://docs.aws.amazon.com/msk/latest/developerguide/what-is-msk.html) .
