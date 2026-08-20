---
schema_version: "1.0.0"
document_id: "2781f43eed68fb20d61a6849df7732472180e3f14aa08e6e9834962d9b0c3fb5"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/building-scalable-real-time-event-processing-with-airstream-at-roku"
published_at: "2025-06-18T00:00:59+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:89c2c158fd65e990da4ccff1071eb684818c5617c2510edc618300c915e07c33"
---

# Building Scalable Real-Time Event Processing with Airstream at Roku

**Authors: Krish Narukulla, Yahor Paulikau**


One of the central tasks in Advertisement Engineering at Roku is processing Ad Impressions. An **impression** is counted every time an ad is loaded and displayed to a user—whether or not the user interacts with it. It’s a basic metric that shows how many times an ad was *seen* (or at least had the chance to be seen). **Attribution** is the process of determining which ad (or set of ads) influenced a user’s **action** , such as a purchase, sign-up, or app install.


At Roku, real-time data processing at scale is crucial for optimizing advertising measurement performance and ensuring accurate impression attribution. With billions of user interaction (conversion) events processed daily, delays in processing can stretch from **48 to 72 hours** using an existing transactional dataset-based execution approach, impacting revenue and analytics. To address this challenge, we developed **Airstream,** a high-performance event processing framework built on **Apache Flink** and **Apache** **Kafka** , enabling near-real-time (NRT) impression attribution and data aggregation.


# **Challenges in Event Processing for Advertising**


Our advertising ecosystem relies on timely and accurate event processing to measure user engagement and optimize ad delivery. However, several challenges existed:


1. **High Latency in Existing Pipelines** – Traditional batch processing involving many upstream dependencies introduced delays, making optimization hard and less predictable.
2. **Fragmented Aggregations** – Multiple Directed Acyclic Graphs (DAGs) in Airflow handled the same data in inefficient ways, creating redundant processing steps.
3. **Data Platform Dependencies** – Attribution required only a fraction of broader data flows, yet it was tied to larger, slower-moving pipelines.


To address these challenges, the Roku Advertisement Engineering team chose to leverage Apache Flink, enhancing it with additional features. Apache Flink, a robust and mature real-time event processing framework, provides exceptional flexibility and scalability, making it an ideal choice for our needs.


# **Airstream Architecture and Design**


Airstream is an in-house streaming framework based on **Apache Flink** that provides a **declarative** and **scalable** approach to defining Extract, Transform and Load (ETL) workflows. It simplifies streaming pipeline development while offering powerful integration with Roku’s data infrastructure. The diagram below displays the top-level architecture and data flow.


## **Key Components**


Let’s deep dive into the key components of data processing with Airstream.


1. **Event Ingestion with Kafka**


1. Airstream consumes events from Kafka allowing it to process high-throughput event streams with low latency.
2. Data can alternatively be read and stored in cloud filesystems using either streaming or batch methods, all while maintaining low latency. This approach offers greater flexibility in data retention, enabling data to be readily available for reprocessing. Additionally, cloud file storage typically presents a more cost-effective option compared to Kafka.


2. **Flink Processing with SQL & UDFs**


1. Supports **SQL-based transformations** and custom **Java/Scala UDFs** to enable flexible event processing.


3. **Custom connectors**


1. For Airstream we designed custom connectors for SQL, allowing developers to utilize asynchronous access to popular storage engines like ScyllaDB and Aerospike.
2. A new Kafka connector has been introduced, enabling the use of the Cloudera Schema Registry. The standard Flink framework supports the Confluent Schema Registry, which we leverage to ensure seamless schema management and transitions.


4. **Multi-Mode Execution (Batch + Streaming)**


1. Users can **switch between batch and streaming** modes, enabling reprocessing of delayed events and supporting various use cases.


5. **Predefined Data Sinks**


1. Outputs can be efficiently stored in a **Data Lake (Parquet)** , transmitted to **Kafka** , or pushed to **Druid/Aerospike** for advanced analysis.


## **Declarative Pipeline Configuration**


Airstream simplifies ETL definitions using **YAML-based pipeline configuration** , reducing engineering overhead while maintaining flexibility.


Example configuration:


```text
version: v1
pipeline:
name: AttributionPipeline
mode: Streaming
steps:
- description: Kafka source
sql: >-
CREATE TEMPORARY TABLE impressions (
`event_time` TIMESTAMP,
`device_id` STRING,
`ad_id` STRING
) WITH (
'connector' = 'kafka',
'topic' = 'impression-events',
'properties.bootstrap.servers' = 'kafka-brokers.roku.com:9092',
'value.format' = 'avro'
)


- description: Parquet sink
sql: >-
CREATE TEMPORARY TABLE impression_sink (
`event_time` TIMESTAMP,
`device_id` STRING,
`ad_id` STRING
) WITH (
'connector' = 'filesystem',
'path' = 'gs://roku-ad-data/impressions',
'format' = 'parquet'
)


- description: Data transformation
sql: >-
INSERT INTO impression_sink
SELECT event_time, device_id, ad_id FROM impressions
```


This declarative approach allows teams to define and modify pipelines **without deep Flink expertise** , accelerating development cycles.


# **NRT Pipeline Design Strategies**


Transitioning to low-latency processing was an exciting yet challenging endeavor. Streaming processing often provides immediate access to preliminary data, while batch processing can follow after a delay of several hours, producing a final, more accurate dataset. For Ad Attribution, we opted to utilize streaming processing exclusively, foregoing subsequent batch settlement. This decision required designing a system capable of generating data with maximum precision, while also accepting a reasonable degree of consistency tolerance.


The primary challenge with upstream data is handling delayed events. Typically, streaming processing employs watermarks based on event-time


to manage late-arriving data. These watermarks are established using timestamp fields originating from Roku devices. Each Roku device contains a buffer where events accumulate before being sent to ingestion pipelines. If network disruptions occur or a device is abruptly powered off, the buffer retains events, resulting in delayed transmissions. Such delayed events are a common issue in real-time processing environments. To tackle this issue, we established side pipelines to handle late events independently. This approach guarantees that the main data flow remains unaffected by any delays in event processing.


Let’s say the main pipeline sets the watermark to ‘-2 hours,’ which acts as a filter for events older than 2 hours in Apache Flink’s session aggregation windows. However, there may be events with delays of 2 hours or more


. These events will be processed using a ‘-9 hours’ watermark. Any event exceeding the 9-hour threshold will be discarded.


The second challenge involves maintaining the currency of lookup data. Traditional batch-processing approaches offer greater control over lookup data availability as pipelines can be restarted once metadata becomes available. In contrast, streaming approaches depend on lookup sources that must provide immediate and accurate data or otherwise require implementing retry (waiting) logic to handle temporary unavailability.


The diagram below shows the main aspects of delayed events processing.


Finally, a long


session aggregation window requires a lot of resources to process streaming data. To mitigate memory demands, the


application’s state is kept in RocksDB located on local SSDs. Local-SSD drives are attached directly to the host instance and accessed through the pod’s volume. Several drives are added to satisfy performance requirements.


# **Infrastructure and Deployment**


Airstream leverages an internal


Roku cloud platform


which runs on top of Kubernetes. The platform provides robust control over application deployment, execution, and monitoring. Airstream pipeline configuration profiles are stored as **FlinkDeployment** CRD and managed through **Helm charts** and **Kustomize** . CI/CD is implemented through Bazel and Jenkins.


### **Example: Real-Time Attribution Pipeline**


1. **Kafka Ingestion** – Collects impression events.
2. **Flink Processing** – Aggregates and filters data for attribution.
3. **Cloud Storage Output** – Stores results in Parquet format for further analysis.


This allows us to **process millions of events per second** , reducing QSS delays from **72 hours to near real-time** .


# **Impact and Future Roadmap**


Since launching Airstream, we have seen significant improvements


in our ad attribution pipelines:


**Reduced Processing Delays** – NRT attribution improved ad optimization.


**Increased Developer Productivity** – YAML-based pipelines enabled faster iterations.


**Scalable and Reliable Processing** – Flink ensured high availability and fault tolerance.


**Comparable to batch processing costs –** Using spot instances and auto-scaling.


Looking ahead, we plan to enhance **stateful processing** for advanced attribution models, introduce **real-time alerting** , and expand **multi-cloud support** .


# **Conclusion**


Airstream has transformed Roku’s advertising data processing by enabling real-time insights and improving efficiency. By leveraging **Kafka, Flink and Kubernetes** , we built a robust system that scales with our needs while reducing operational complexity.


The post[Building Scalable Real-Time Event Processing with Airstream at Roku](https://engineering.roku.com/building-scalable-real-time-event-processing-with-airstream-at-roku) appeared first on[Engineering Blog](https://engineering.roku.com/) .
