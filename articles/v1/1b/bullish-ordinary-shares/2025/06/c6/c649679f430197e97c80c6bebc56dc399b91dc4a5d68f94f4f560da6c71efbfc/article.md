---
schema_version: "1.0.0"
document_id: "c649679f430197e97c80c6bebc56dc399b91dc4a5d68f94f4f560da6c71efbfc"
company_key: "bullish-ordinary-shares"
company: "Bullish"
source_id: "bullish-ordinary-shares-rss-b348dbb1f0cd"
canonical_url: "https://medium.com/bullish-engineering/optimizing-kafka-consumer-properties-for-high-volume-trading-ace215a35552"
published_at: "2025-06-09T15:12:28+00:00"
first_seen_at: "2026-07-20T04:36:16.773580+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:77bf20c90909a755e3fd2ff08ae4a7ca0f1017567fbb3e292883b8f371d0ec0d"
---

# Optimizing Kafka Consumer Properties for High-Volume Trading

Kakfa


Trading


Crypto


Software Engineering


# Optimizing Kafka Consumer Properties for High-Volume Trading


[Michael Cheung](https://medium.com/@michaelcheung_88032?source=post_page---byline--ace215a35552---------------------------------------)


4 min read


·


Jun 9, 2025


--


Press enter or click to view image in full size


TL;DR


- Processing millions of trades daily, optimizing Kafka for high throughput and reliability.
- Making informed decisions for each configuration to optimize performance and stability.
- Iteratively testing to maintain stability and performance under intense trading demands.


Bullish manages millions of trades daily, with Apache Kafka as the backbone of our real-time data pipelines. As trading volumes grow and regulatory requirements become more demanding, we focus on enhancing our pipelines for better scalability and reliability. Fine-tuning Kafka consumer properties has been key to balancing throughput, latency, and stability. In this blog, we share our experiences to help others optimize Kafka for high-throughput environments.


## Tuning and Performance Insights


Optimizing Kafka consumers for a high-throughput exchange like Bullish involves tailoring configurations to specific needs, such as low latency for real-time trade processing or high throughput for batch analytics. Through iterative testing, we discovered how each property affects performance, especially under the pressure of millions of daily trades and strict regulatory standards.


## Consumer Properties


## key.deserializer & value.deserializer


**Description** : Deserializer class for message keys/values (e.g. StringDeserializer).
**Configuration** : StringDeserializer for keys; binary format (e.g. KafkaAvroDeserializer, ByteArrayDeserializer) for values.
**Consideration** : Avro and other binary formats ensures compact, fast data transfer for high throughput; strings for keys are simple and effective. Kafka Avro schemas allow schema compatibility control and decoupling of producers and consumers. We have other binary formats for lagecy reasons and we are using byte array and custom deserializer.


## fetch.min.bytes


**Description** : Minimum data for fetch request before returning (default 1 byte).
**Configuration** : Default value.
**Consideration** : Higher values could reduce network round trips, enhancing throughput. Given our high trade volume, micro-batches fill quickly, so we maintain the default but may increase it if throughput bottlenecks arise.


## max.poll.records


**Description** : Maximum records per poll() call (default 500).
**Configuration** : Application-specific.
**Consideration** : Higher values enhance throughput for trade bursts but increase memory usage. We adjust based on application needs, balancing memory constraints and processing speed. We use 1000 as the starting value and tune it with iterative performance tests.


## heartbeat.interval.ms, session.timeout.ms


**Description** : Interval between heartbeats to group coordinator (default 3000ms); timeout for detecting client failures (default 45000ms).
**Configuration** : Default values.
**Consideration** : Lowering detects failures faster but risks unnecessary rebalances if consumer logic delays heartbeats. We don’t see any issue so far using default values.


## max.partition.fetch.bytes


**Description** : Maximum data fetched per partition (default 1MB).
**Configuration** : Default value.
**Consideration** : Increasing boosts throughput but requires sufficient memory. We monitor for bottlenecks and adjust if needed to handle volume spikes.


## allow.auto.create.topics


**Description** : Allows automatic topic creation (default true).
**Configuration** : false
**Consideration** : Prevents unintended topics, aligning with our data catalogue for regulatory compliance and lineage. Ensures controlled topic creation via data catalogue repository.


## enable.auto.commit


**Description** : Automatically commit offsets (default true).
**Configuration** : Application-specific
**Consideration** : Auto-commit is viable for most use-cases with idempotent consumer business logic to avoid duplicates. Manual commit is used for precise exception handling for large volume topic.


## isolation.level


**Description** : Isolation level for transactional messages (default: read_uncommitted).
**Configuration** : read_committed (with enable.idempotence=true on producers)
**Consideration** : Ensures closer to exactly-once delivery to avoid duplicates. According to our performance tests, read_committed gives us acceptable throughput while reducing duplicates.


## Lessons Learned


Tuning Kafka to handle millions of daily trades highlighted the importance of performance testing in assessing configuration impacts. Our configurations are based on rigorous tests aligned with projected data volumes and patterns. Ongoing testing allows us to continuously refine our settings for optimal performance.


## Conclusion


Optimizing Kafka consumer properties has revolutionized how Bullish processes millions of trades daily, preparing our data platform for increasing volumes and stricter regulations. These insights demonstrate our commitment to scalable, compliant systems.


References


- [Confluent Documentations](https://docs.confluent.io/platform/current/installation/configuration/consumer-configs.html)
- [Apache Kafka Documentations](https://kafka.apache.org/documentation/#consumerconfigs)


Feeling inspired by the cutting-edge work we’re doing at Bullish? We’re always on the lookout for talented individuals to join our team.[Explore our open roles](https://bullish.com/careers/) and be Bullish on your career. Want to stay up to date with the latest news from across Bullish? Follow us on[LinkedIn](https://www.linkedin.com/company/bebullish) and[X](https://twitter.com/Bullish) .


*Disclaimer: This material, including any material accessed through embedded links (“Information”) is for general informational purposes only and is not intended to constitute investment, tax, accounting, legal or financial advice. This Information is not an offer to buy or sell or a solicitation of an offer or to buy or sell any particular asset or to provide financial services of any kind. You should consider your own personal situation carefully and consult your professional advisors before making any investment decision.*


*Bullish makes no representation as to the accuracy, completeness, timeliness, suitability, or validity of any Information. The products, services, and solutions discussed in the Information are likely to change, so the Information may become outdated, incorrect or incomplete. Bullish is under no obligation to update Information. Bullish will not be liable for any errors or omissions in the Information. Virtual assets and related products are high risk. Access to certain assets or services referred to in the Information may not be available in your jurisdiction or to all types of investors. Information may reference the Bullish Exchange, which is licensed by the Gibraltar Financial Services Commission (DLT license: FSC1038FSA). This Information may not be quoted, deleted, or modified in any way without Bullish’s prior written consent. All rights reserved. Bullish and the Bullish Logo are trademarks of Bullish Global.*
