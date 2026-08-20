---
schema_version: "1.0.0"
document_id: "42957694cbbcf0e056c3c00862e5bfbc9dac913895f5b5a773530e0b3a1b339c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-mq-rabbitmq-configurable-storage/"
published_at: "2026-07-15T18:18:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:be1394fab15e6c3d6d300db431008f766f709a993348b97bd9a0598553c4732d"
---

# Amazon MQ now supports configurable storage for RabbitMQ brokers

Amazon MQ now allows you to configure the EBS Disk storage size for RabbitMQ brokers independently of instance type. When creating or updating a broker, you can define a custom storage size, allowing you to right-size storage independently of your instance size to match your specific messaging workload requirements. Configurable storage is available for RabbitMQ M7g brokers on version 4.2 or later using cluster deployments only.


With configurable storage, you can choose a storage size from the default value on M7g to the maximum allowed value depending on your instance size in increments of 5 GB. You can specify the Storage Size using the using the AWS Console, AWS CloudFormation, AWS Command Line Interface (CLI), or the AWS Cloud Development Kit (CDK). Storage changes are applied during the next broker reboot.


Standard Amazon MQ storage pricing applies based on the disk size as per[Amazon MQ pricing](https://aws.amazon.com/amazon-mq/pricing/) . Configurable storage is available in all commercial AWS Regions where Amazon MQ for RabbitMQ is offered. To learn more, see the[Amazon MQ Developer Guide](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/welcome.html) .
