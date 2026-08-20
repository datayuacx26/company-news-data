---
schema_version: "1.0.0"
document_id: "7eaac7e316031394d0eb0454adcb5a705ef924b8290a51379391e4a300979c0c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/kinesis/on-demand-scale-down"
published_at: "2026-07-24T18:55:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:dc3a9cb228ec7520011f51b96459b42602a82220d5af27721415969e83f2deaf"
---

# Amazon Kinesis Data Streams now supports scaling down ingest capacity with warm throughput

Amazon Kinesis Data Streams is a serverless streaming data service that makes it easy to capture, process, and store data streams at any scale. On-demand streams automatically increase ingest capacity in response to rising data ingest usage. With On-demand Advantage mode, you can proactively manage stream capacity using warm throughput to prepare streams for sudden changes in data traffic. We are extending warm throughput with the ability to also scale down ingest capacity, giving you full control to scale your stream's write throughput up or down.


To scale down, simply set a lower warm throughput value on your on-demand stream. The stream adjusts to the requested capacity or the amount needed to support peak data ingest usage in the last hour, whichever is higher. This ensures your stream always retains sufficient capacity for current traffic while releasing excess capacity you no longer need. As a result, you get optimal stream-processing performance and cost efficiency.


Warm throughput scale-down is available at no additional cost for all on-demand streams with On-demand Advantage mode enabled. For more information about On-demand Advantage, see[Choose the right mode to stream in](https://docs.aws.amazon.com/streams/latest/dev/how-do-i-size-a-stream.html) in the Amazon Kinesis Data Streams Developer Guide. To get started with the feature, see[Update a stream](https://docs.aws.amazon.com/streams/latest/dev/updating-a-stream.html) . For pricing details, see[Amazon Kinesis Data Streams pricing](https://aws.amazon.com/kinesis/data-streams/pricing/) .


The feature is available in all AWS Regions where Amazon Kinesis Data Streams On-demand Advantage is supported.
