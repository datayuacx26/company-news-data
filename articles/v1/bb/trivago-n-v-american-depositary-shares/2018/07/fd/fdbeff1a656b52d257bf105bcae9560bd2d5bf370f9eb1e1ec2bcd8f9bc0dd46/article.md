---
schema_version: "1.0.0"
document_id: "fdbeff1a656b52d257bf105bcae9560bd2d5bf370f9eb1e1ec2bcd8f9bc0dd46"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/streaming-with-kinesis/"
published_at: "2018-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:d6896eb94a56fa05f2bbd98d3c6520b1e3008edb5764ab6da2818f82e9917d72"
---

# AWS Kinesis with Lambdas: Lessons Learned

Almost six months ago, our team started the journey to replicate some of our data stored in on-premise MySQL machines to AWS. This included over a billion records stored in multiple tables. The new system had to be responsive enough to transfer any new incoming data from the MySQL database to AWS with minimal latency.


Everything screamed out for a streaming architecture to be put in place. The solution was designed on the backbone of Kinesis streams, Lambda functions and lots of lessons learned. We use Apache Kafka to capture the changelog from MySQL tables and sink these records to AWS Kinesis. The Kinesis streams will then trigger AWS Lambdas which would then transform the data.


These are our learnings from building a fully reactive serverless pipeline on AWS.


## Fine-tuning your Lambdas


We all love to read the records as soon as they turn up in your streams. For this, you have to be absolutely sure your Lambdas are performing top notch. If you are dealing with high volume data, increasing the Lambda parameters can give you surprising results. The parameters you can finetune are


- Memory
- Batch-size
- Timeout


For us, increasing the memory for a Lambda from 128 megabytes to 2.5 gigabytes gave us a huge boost.


**The number of Lambda invocations shot up almost 40x.** But this also depends on your data volumes. If you are fine with minimal invocations per second, you can always stick with the default 128 megabytes.


You can also increase the Batch-size of each event. This means that more records will be processed per invocation. But, make sure you do not hit the max timeout of the Lambda function (5 minutes). A good number is 500–1000 records per event.


## Beware of Kinesis read throughput


While reading incoming records from Kinesis, always remember that the Kinesis stream will be your biggest bottleneck.


Kinesis streams have a read throughput of 2 megabytes per second per shard. This means that the bottleneck really lies in the number of shards you have in the Stream.


We started off with single shard streams and realised the Lambdas do not process the records fast enough. Increasing the shards from 1 to 8 easily gave us an 8 fold increase in the throughput. At this moment, we almost reached 3k Lambda invocations per second in one of our busier streams. Remember that a single Lambda invocation process records from a single shard.


Having said that, if your stream is not expected to be very busy, you don’t gain much by splitting the records into shards.


The bottleneck lies in shards per streams. Create busier streams with more shards so that the records are picked up quickly.


See the result of increasing the shards from 1 to 8.


**The number of invocations went up almost 3x.**


## Remember the Retention period


AWS will make all our lives easier if the records in Kinesis are persisted forever. But, Kinesis streams has a maximum retention period of 168 hours or 7 days (for now!). This means that when you have a new record in your Kinesis stream, you have 168 hours to process it before you lose it forever.


This also means that once you realise your Lambda failed to process a record, you have 168 hours to fix your Lambda or you lose the record.


Things are a bit easier if the incoming records are stateless, i.e the order of the records does not matter. In this case, you could configure a Dead Letter Queue or push the record back to Kinesis. But if you are dealing with database updates or records where the order is important, losing a single record result in losing the consistency of data.


Make sure you have enough safety nets and infrastructure to react to failed records.


## Monitor your IteratorAge


Make sure you monitor this very important metric, the` IteratorAge (GetRecords.IteratorAgeMilliseconds)` . This metric shows how long the last record processed by your Lambda stayed in the Kinesis stream. The higher the value, the less responsive your system. This metric is important for two reasons:


- To make sure you are processing records fast enough
- To make sure you are not losing records (record stays in the stream beyond your retention period)


A very high responsive system will have this metric always < 5000 ms, but it’s highly linked to the problem you are trying to solve.


It’s best to set up alarms on the` IteratorAge` to make sure the records are picked up on time. This can help you avoid situations where you start losing records.


In the case above, the Lambdas were really slow to pick up records. This means that the records stayed in Kinesis without being processed, and eventually got deleted. At this point, all the records fetched by the Lambda has stayed there for almost 168 hours (the maximum possible). In this case, there is a very very high chance we lost records. Be safe, set a Cloudwatch alarm.


## Lambda helps out in Error Handling


Lambdas have a special behaviour when it comes to processing Kinesis event records. When the Lambda throws an error while processing a batch of records, it automatically retries the same batch of records. No further records from the specific shard are processed.


This is very helpful to maintain the consistency of data. If the records from the other shards do not throw errors, they will proceed as normal.


But consider the case of a specific corrupt record which should be rejected by the Lambda. If this is not handled, the Lambda will retry forever, and if not monitored, the Lambda will retry until the record is eventually expired by the Kinesis steam. But at the point, all the unprocessed records in the shard are also near the expiry period and if the Lambda is not fast enough, so many records will be lost.


Trust the Lambda to retry errors on valid records, but make sure you handle cases where the record is corrupt or should be skipped.


## Conclusion


At the moment, we have almost 20 Kinesis streams and a similar number of Lambdas processing them. All of them are fully monitored and have never given us sleepless nights. We are building and innovating every single day.
