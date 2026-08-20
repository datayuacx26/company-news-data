---
schema_version: "1.0.0"
document_id: "1063591ef20cc826599d9cebda8cde4b1f8abdcd79e791920c89a16219f7de5b"
company_key: "zoominfo-technologies-inc-common-stock"
company: "ZoomInfo Technologies Inc"
source_id: "zoominfo-technologies-inc-common-stock-rss-ef9b8d7bcd4f"
canonical_url: "https://engineering.zoominfo.com/zoominfo-data-platform-zdp-ingestion-process-improvement"
published_at: "2024-12-18T16:37:53+00:00"
first_seen_at: "2026-07-20T23:24:42.992125+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:9adb70c8a2f9202576d5f9e458b155ef34f34f5281e04dc5d491ed75314ae032"
---

# Zoominfo Data Platform (ZDP) ingestion process improvement

## **Background**


The Zoominfo Data Platform (ZDP) team has developed a robust data platform framework to streamline ETL processing and provide a unified source of truth for all data. Every day, numerous jobs must be executed in a specific sequence and timing to deliver vital business insights by each morning. The platform incorporates ingestion APIs leveraging the[BigQuery query API](https://cloud.google.com/java/docs/reference/google-cloud-bigquery/latest/com.google.cloud.bigquery.BigQuery#com_google_cloud_bigquery_BigQuery_query_com_google_cloud_bigquery_QueryJobConfiguration_com_google_cloud_bigquery_BigQuery_JobOption____) and[BigQueryIO](https://beam.apache.org/documentation/io/built-in/google-bigquery/) in its Dataflow jobs. However, performance issues surfaced after Beam version upgrades, revealing a significant bottleneck in transform operations involving multiple BigQuery tables ingested through ingress pipelines. As a result, some jobs experienced slow execution and delays, negatively impacting business outcomes. In an attempt to mitigate these issues, job owners increased CPU allocation by adding hundreds of workers with BigQueryIO, but this led to slot contention and did not effectively resolve the underlying performance problem.


## **Solution**


While coming up with the solution, it was critical that we increase parallelism while being modest about the cost to meet its tight deadline. After evaluating several alternative solutions, the ZDP team proposed new APIs that leverage the[BigQuery Storage API](https://cloud.google.com/bigquery/docs/reference/storage) and[GCS export](https://cloud.google.com/bigquery/docs/exporting-data) to enhance parallelism and address performance bottlenecks found in both the existing ZDP-provided API and[BigQueryIO](https://beam.apache.org/releases/javadoc/current/org/apache/beam/sdk/io/gcp/bigquery/BigQueryIO.html) . The BigQuery Storage API sends data over the network using a binary serialization format, which increases parallelism by integrating seamlessly with Dataflow jobs to create multiple streams, thereby achieving higher throughput. Similarly, the BigQuery export option allows data to be exported to GCS, enabling Dataflow jobs to perform multiple file reads to further increase throughput.


### **Performance improvement result**


In our evaluation of data processing strategies, we tested two existing approaches (BigQuery[query](https://cloud.google.com/java/docs/reference/google-cloud-bigquery/latest/com.google.cloud.bigquery.BigQuery#com_google_cloud_bigquery_BigQuery_query_com_google_cloud_bigquery_QueryJobConfiguration_com_google_cloud_bigquery_BigQuery_JobOption____) and[BigQueryIO](https://beam.apache.org/releases/javadoc/current/org/apache/beam/sdk/io/gcp/bigquery/BigQueryIO.html) ) and two new approaches ([BigQuery storage API](https://cloud.google.com/bigquery/docs/reference/storage) and[GCS export option](https://cloud.google.com/bigquery/docs/exporting-data) ). Below is a summary of the four algorithms used to execute a BigQuery query and read data:


1. **BigQuery query API** : This is an existing approach and the slowest algorithm. It executes query and outputs rows one by one and thus, does not utilize parallelism for read operation.
2. **BigQueryIO** : Another existing approach, it is faster than BigQuery query API but often encounters slot contention and failures at certain stages, requiring restarts. We employ 200 workers to maximize parallelism, making this a more costly option.
3. **BigQuery Storage API:** As one of the new approaches, we leveraged this new API to store query results in a temporary BigQuery table and use a storage API reader to read the results in multiple streams. It is faster than the BigQuery query API and performs better than BigQueryIO with fewer workers.
4. **GCS export** : This new method exports BigQuery query results to a GCS bucket and triggers a dataflow to read from the exported files in parallel. It performs the best out of all the options, requiring fewer workers, about 20 workers at peak performance.


With BigQuery query API, Apache Beam autoscaling algorithms reduce workers to a single instance, creating performance bottlenecks. This is because the query can only run on a single instance. Our dataflow jobs typically execute some complex queries that result in a large amount of data which we convert to a different type to process further to store them in BigQuery tables. Hence, one of the major critical conditions is to be able to read and process them in parallel to complete the task in time.


We optimized a few critical internal methods that read remote input files. We also learned that the “[Reshuffle](https://beam.apache.org/documentation/transforms/python/other/reshuffle/) ” step helps to trigger parallelism and out of all four different approaches, exporting the query result so we can read and process from GCS in parallel performs the best even with the smaller number of workers. The following table summarizes four different approaches at a high level.


**Algorithm** **Time / 10M files** **Autoscale algorithm** **Parallelism** **Worker count**


BigQuery query API 22.43 mins Throughput Not happening 1 (fixed)


BigQueryIO 11.83 mins None Fixed 200 (manual)


Storage API 17 mins Throughput Auto / Stream base 20 (at peak)


BigQuery export 1.67 mins Throughput Auto 20 (at peak)


In conclusion, BigQuery export is a highly efficient choice, processing data about 7 times faster than BigQueryIO and 13 times faster than BigQuery query API. Its algorithm leverages parallelism effectively compared to BigQuery query API while it uses about 10 times fewer workers compared to jobs that employ BigQueryIO.


## **Current status**


Most dataflow jobs in the team switched to use the *BigQuery Export* option, reaping the benefit of better performance. In addition, the partner team has been gradually adopting the GCS export option and seeing positive results in terms of its performance and cost management.


We are currently enhancing data ingestion by optimizing parallel file processing for formats like Parquet coming from remote data stores such as Snowflake, GCS, and S3. This involves enabling simultaneous reads across multiple workers with file partitioning and dynamic resource allocation to efficiently handle larger datasets, ensuring faster, more reliable analytics. More enhancements are on the way.
