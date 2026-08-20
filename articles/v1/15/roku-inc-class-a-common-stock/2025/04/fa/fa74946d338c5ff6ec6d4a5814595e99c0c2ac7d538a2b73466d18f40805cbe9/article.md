---
schema_version: "1.0.0"
document_id: "fa74946d338c5ff6ec6d4a5814595e99c0c2ac7d538a2b73466d18f40805cbe9"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/apache-pinot-and-trino-scalable-architecture-to-help-advertisers-plan-their-spend-on-the-roku-platform"
published_at: "2025-04-22T16:42:59+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:92bbccaeb10cdd499f0761d1270b585ffc672e250d05e03b8fb3d47ffbc6f36e"
---

# Apache Pinot and Trino – Scalable Architecture to help advertisers plan their spend on the Roku platform

#### Author: Suraj Sheshadri


### **1. Introduction:**


The purpose of this blog is to discuss and share the learnings of working


with Apache Pinot for low latency querying and horizontal scaling requirements for offline use-cases of the Ad Avails Forecasting team at Roku. This team helps advertisers determine how much they spend on the Roku platform. We will also discuss Apache Trino and its learnings while querying Pinot for table joins use-cases.


### **2. Objective:**


Roku’s Avails Forecasting tool is used to estimate how much an advertiser can spend on Roku’s platform. Internal client service teams use the tool to plan and book upcoming advertiser budgets. The Ad Avails Forecasting team at Roku needed to scale their current sample size from 1X to 5X to improve the prediction accuracy, reduce the Mean Absolute Percentage Error (MAPE) and improve accuracy for narrow targeting use-cases by having more accurate representation of the data in the sample. This would also help reduce the day-over-day variance when pulling forecasts for same criteria. Previously, the team used the roaring bitmap architecture


[https://roaringbitmap.org/](https://roaringbitmap.org/)


to compute and save the forecasted index data in-memory on a single ec2 instance (c5.24xlarge). As Roku’s business continues to grow and with newer


use-cases and inventory types, the team faced scalability challenges with the current architecture. We decided to explore Apache Pinot as a tool which stores distributed data and has various inbuilt indexing capabilities (e.g., Inverted index, Star-tree, and Dictionary) to provide low-latency querying for the increased sample size.


### **3. Solution Using Apache Pinot:**


#### **3.1 Apa che Pinot, its features and b enefits:**


Pinot is a distributed column-oriented Online Analytical Processing( OLAP)


datastore purpose-built to provide ultra-low latency analytics even at extremely-high throughput. It is horizontally scalable, fault tolerant, and supports high concurrency of queries. Pinot offers several smart indexing methods like inverted index and star tree index, and pre-aggregation techniques for low latency. It can support both single or multi-valued columns. It can ingest from batch data sources such as HDFS, S3 and GCS. Pinot offers a SQL query interface and various clients for querying. It also offers the ability to write custom aggregation functions through User-Defined Functions (UDFs). All these features made Pinot an attractive choice for our team-specific requirements.


#### **3.2** : **Forecasting team workflow architecture using Apache Pinot:**


Below is a high-leve


l diagram of our workflow architecture.


Figure 1


- Forecasted index data containing future inventory predictions are generated in storage and loaded into Pinot’s offline tables once a day. We use a Spark ingestion job to load data to Pinot, which generates the segments along with its index and metadata.
- We generate a wide data


set on storage having about 1300 columns with each Pinot segment being about 400-500MB, which is the recommended size for Pinot segments.
- The Pinot controller determines which segment loads to which server. We load the metadata for the segments to the server and the server downloads the segments from data store in the background as a decoupled process.
- We load data to two tables, main and backup, in Pinot to avoid a single point of failure. If there is any issue during data load for one table, we query the backup table. Currently Pinot does not support atomic data load for Spark ingestion.
- We perform a complete refresh of the table data on a daily basis. Therefore, it is also important to have same number of segments created at the source storage directory for a given Pinot table as each replaces the target segments on Pinot based on same name.
- We also use a replica group of two for each table. In case there is any issue with a particular server, we have another replica group to query from. This also helps with achieving higher queries per second (QPS) execution.
- It is also very easy to revert a table data to the last known good index in Pinot incase of any data issues. We integrate with Airflow to load data to Pinot using Spark operator.
- We apply an inverted index to all our dimension columns used in the query filters.
- We use Pinot’s swagger rest APIs to perform various cluster operations. We can check the status for all segments of a Pinot table after a data refresh and reset any segments in an error state.
- Our Pinot cluster then powers various Forecasting use-cases at Roku. Pinot’s availability and query latency is crucial for our Forecasting website’s success.


### **4. Pinot cluster configuration and metrics:**


#### **4.1 Pinot data load time using spark job**


We use the SegmentCreationAndMetadataPush type Spark job to generate segments and load data into Pinot. It takes between 5 mins to 20 mins for each of our tables to be loaded based on the number of segments in each table and resource allocation for the Spark load job.


#### **4.2 QPS Performance Test Results:**


We executed 1200 queries in parallel. The table below outlines the min, max and average query run times in milliseconds for various table configuration setups. We ran different sample queries with various filtering and aggregations.


Cluster


configuration used for purposes of below POC:
3 Controller with 8 CPU and 32 GB RAM
3 Broker with 8 CPU and 32 GB RAM
20 Server with 6 CPU and 32 GB RAM and 256 GB disc


**Cluster Setup** **Min** **Max** **Avg**


•Table without replication group and data spread across 30 servers


•Table with replication group of 3 and 10 servers in each replication group


•Table with replication group of 2 and 15 servers in each replication group


•Table with replication group of 2 and 10 servers in each replication group (configuration used by forecasting team)


4


5


5


6


1059


1165


1264


1589


469


407


421


706


#### **Notes:**


- Pinot is designed to scale query performance based on the number of nodes in a cluster. As you add more nodes and replication, query performance will always improve based on the expected query volume per second quota.
- We also enable Adaptive Server Selection routing capability for Pinot Brokers. This routes incoming queries to the best available server instead of following the default round robin approach, and we found it improved query performance.
- Finally, the cost of our Pinot cluster is very reasonable.


### **5. Pinot Deployment Architecture Overview:**


We run our Pinot cluster in Kubernetes so we can easily manage problems like scalability, load balancing, and operations. Now if we look at the central part of the diagram below, we are using a stateful set for all of the components as well as Zookeeper. Stateful sets provide a unique map of identifiers and stable and persistent storage volume and provides an ordered and graceful startup in addition to scaling. We deploy the application using GitLab CI/CD pipelines. The workflow begins when the developer pushes code changes to a GitLab repository with a specific tag which triggers the CI/CD pipeline and the shared runner executes various stages of the CI/CD process, such as building, testing, and deploying one of the latest versions of Pinot to its Pinot namespace.


Figure 2


### **6. Pinot optimizations**


In order to successfully use Pinot for our production use cases, we made multiple optimizations as described below.


#### **6.1 Pinot Routing optimizations**


All of the forecasting queries are filtered on a single country at a time. We can then partition each Pinot segment to contain data for only one country and inform Pinot about this partitioning for better query performance and routing. More details on this topic can be found in the Pinot documentation.[https://docs.pinot.apache.org/operators/operating-pinot/tuning/routing](https://docs.pinot.apache.org/operators/operating-pinot/tuning/routing)


#### **6.2 Pinot Star Tree Index for pre-aggregated results**


We execute several Pino


t queries at the time of the server startup for our forecaster application. The result-set is cached in memory for the server logic to use. This is a static aggregation query that runs on every server at startup and the does not change. We had initially applied only the inverted index for the filter columns for the query. We later enhanced the Pinot table config to compute a star tree index for this query to compute pre-aggregated results on Pinot, improving the query execution run time by about 600 percent as compared to the inverted index. More details on star tree index configs can be found at[https://docs.pinot.apache.org/basics/indexing/star-tree-index#example](https://docs.pinot.apache.org/basics/indexing/star-tree-index#example)


**6.3. Pinot 1.0 With Multi-Stage Engine Support**
When we started this project, Pinot’s latest version 0.11.0 did not support query joins and common table expressions (CTE). We had to rely on an Apache Trino connector for any join query execution on Pinot. We later completely migrated to Pinot 1.0, which offered joins and CTEs support in Pinot and saw a 25% query performance improvement compared to Trino.


#### **6.4 Pinot cluster tuning**


We tune each Pinot component (broker, server, controller) to optimize CPU, memory, and storage for the best query performance and cost effectiveness. Some examples are below:


- Use direct memory (outside of the JVM heap) for segment caching, reducing garbage collection overhead and improving query latency.
- Optimized Disk Storage to use SSDs for fast data retrieval and indexing.
- Increase segment replication factors to improve data redundancy, availability.
- Adjust task concurrency and worker threads to balance parallel processing efficiently.
- Experiment with Kubernetes-based Event Driven Autoscaler (KEDA) monitoring framework to scale the cluster.
- Create a background monitoring script to frequently check the status of Pinot pods and auto restart pods if servers are in dead status in Pinot UI, reducing manual efforts.
- Moved from on-demand to Spot instances to save cluster costs by 20 percent.
- Auto-scale Pinot servers (vertically and horizontally) based on time of the day and cluster usage patterns by users to optimize cost of the cluster.


### **7. Integration with Apache Trino:**


#### **7.1 Problem Statement:**


As mentioned earlier, when we started this project, Pinot’s latest version 0.11.0 did not support query joins and common table expressions (CTE). We had to rely on the Apache Trino connector for any join query execution on Pinot. This section we will discuss on some of our learnings. Also, Pinot’s multi-stage query engine is still evolving and has some limitations (mentioned below) which make Trino a reliable alternative to query Pinot for certain use-cases.


- Pinot’s multi-stage query engine is a pure in-memory system, making it unsuitable for large-scale, long-running queries designed to access and transform entire datasets.
- It’s not ideal for complex correlation, join algorithms that touch many tables or have many non-trivial join conditions.
- Long-running, complex queries such as ETL-type (extract, transform, and load) use cases are not recommended.
- Support for multi-value columns is limited to projections.
- Schema and other prefixes are not supported in queries.
- Null handling is not supported when tables use table based null storing.


#### **7.2 What is Apache Trino and its key features:**


- Trino is an open-source distributed SQL query engine designed to query large data sets distributed over one or more heterogeneous data sources. The Trino to Pinot connector also supports predicate pushdown. Filters and limits are pushed down to Pinot for faster query processing. We use the Pinot SQL Passthrough feature to leverage Pinot’s fast aggregations. The syntax for this is in the diagram below. Pinot SQL Passthrough also helps leverage Pinot’s capability to simplify multi-value column filtering syntax.


- We are aware of an open bug when using Pinot sql passthrough; when we have a filter clause, e.g., country = ‘United Kingdom’ where U and K are upper case, Trino sends ‘united kingdom’ as a lower case filter value to Pinot. To overcome this bug, we have currently lower cased all our data in Pinot tables.
Known issue:[https://github.com/trinodb/trino/issues/13024](https://github.com/trinodb/trino/issues/13024)


#### **7.3** : **Forecasting team workflow architecture using Apache Trino:**


As seen in the below diagram, any Forecaster service queries that need join capabilities are passed to Trino and Trino in-turn queries Pinot to return the results. If there is no need for joins, we query Pinot directly and return the results. Since Pinot 1.0 release we have moved completely to Pinot even for join queries using its native multi stage query engine.


Figure 3


### **8. Looker Integration** :


We can visualize Pinot data using Looker which is a Business Intelligence platform that helps users analyze and share data to make better business decisions. Looker can connect to Trino which in turn can query data stored in Apache Pinot through a Pinot connector. Users can use Looker’s features to explore data, create visualizations, and build dashboards based on Trino queries.


### **9. Conclusion** :


Roku’s Ad Avails Forecasting team successfully addressed their challenges by implementing a robust, scalable and high-performance data infrastructure using Apache Pinot and Apache Trino. By leveraging Pinot’s low-latency querying capabilities, powerful indexing capabilities and horizontal scalability, the team was able to significantly improve the accuracy and performance of their forecasting models. Additionally, Trino’s powerful query engine played a crucial role in supporting complex table joins and ensuring seamless data integration. This architecture and various optimizations performed by the team has not only addressed the immediate needs of the Forecasting team, but also positioned team to handle Roku’s future growth, use-cases, and inventory types.


### **10. References** :


- **Pinot Documentation:**[https://docs.pinot.apache.org/](https://docs.pinot.apache.org/)


- **Pinot community Slack:** •[https://communityinviter.com/apps/apache-pinot/apache-pinot](https://communityinviter.com/apps/apache-pinot/apache-pinot)


- **Pinot + Trino Integration** :[https://docs.pinot.apache.org/integrations/trino](https://docs.pinot.apache.org/integrations/trino)


- **Trino to Pinot connector:**[https://trino.io/docs/current/connector/pinot.html](https://trino.io/docs/current/connector/pinot.html)


- **Trino community Slack:**[https://trino.io/slack.html](https://trino.io/slack.html)


The post[Apache Pinot and Trino – Scalable Architecture to help advertisers plan their spend on the Roku platform](https://engineering.roku.com/apache-pinot-and-trino-scalable-architecture-to-help-advertisers-plan-their-spend-on-the-roku-platform) appeared first on[Engineering Blog](https://engineering.roku.com/) .
