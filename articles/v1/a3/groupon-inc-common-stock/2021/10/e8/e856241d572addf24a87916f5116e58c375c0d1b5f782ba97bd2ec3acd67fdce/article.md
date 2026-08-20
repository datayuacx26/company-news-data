---
schema_version: "1.0.0"
document_id: "e856241d572addf24a87916f5116e58c375c0d1b5f782ba97bd2ec3acd67fdce"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/pinion-the-load-framework-part-2-e6a47586e7be"
published_at: "2021-10-29T16:50:24+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:d7adc770ae6f6d3a28e98914733a8922d933a01b12f72e6f059a7dfd98e54845"
---

# Pinion — The Load Framework Part-2

# Pinion — The Load Framework Part-2


[Saurabh Jain](https://medium.com/@saujain?source=post_page---byline--e6a47586e7be---------------------------------------)


9 min read


·


Oct 29, 2021


--


This post is the 2nd part of the “Pinion — The Load Framework” series. In case you have not read the 1st post, you can read it[here](https://medium.com/groupon-eng/pinion-the-load-framework-79cc1d8bff55) .


In this post, we are going to cover the following topics.


1. How does Pinion use Delta Lake for SCD operations?
2. Small file problem with Delta Lake and its resolution.


Before we dive into the topics of this post, let’s look at the definition of DeltaLake to set the context right.


**Apache Delta Lake -**
Apache Delta Lake is an open-source framework that enables the addition of ACID transactions support to a new data lake or an existing data lake created on top of S3, GCS, and HDFS.


> In addition to this, it provides other features such as scalable metadata handling, unified interface for both batch and streaming application, schema enforcement, time travel, and a rich interface of APIs to enable complex use cases like change-data-capture (CDC) and slowly-changing-dimension (SCD) operations.


To keep the post concise and to the point, I won’t go into much detail here about Delta Lake, since there is already great documentation available about it, that you can read it[here](https://docs.delta.io/latest/index.html) .


**How does Pinion use Delta Lake for SCD operations? -**


Apache Delta Lake provides a rich set of APIs to handle slowly-changing dimensions, however, those APIs were not enough alone to build the features that we want to have in The Pinion Framework.


So, we decided to enrich the APIs provided by Delta Lake by adding the following features to it:


- Data Validation
- Compaction
- Audit
- Streamlined logging infrastructure to make the data engineer’s life easier during debugging of a failed job
- Chained APIs


Let’s dive a little further into the features that we had listed above.


1. **Data Validation** — By default, schema enforcement is enabled in Pinion for all the APIs where we have a need of inserting rows from source data(LRFs) into the target table. In case of a schema mismatch, Pinion raises an error and stops processing of further stages. It ensures the data at the target is always complete, which means all the columns in the source data should provide the value for each available column in the target. Doing this we ensure we are maintaining data quality standards that are critical for Reporting and Data Science Teams.
For APIs where data append is not needed, schema enforcement is relaxed. For example, update and delete APIs don’t enforce schema validation since for update we may be updating only a few columns, and for delete, we may need only columns in LRFs necessary to identify rows that need to be deleted.
2. **Compaction** — One of the challenges that we faced with the open-source Delta Lake version is splitting bigger parquet files into smaller files as more and more SCDs are performed on the target data in Delta Lake. This issue has not only impacted the performance of our jobs but also raised the cost exponentially of each pipeline execution. We will cover this in great detail in the next section (Small file problem with Delta Lake and its resolution)
3. **Auditing** — When we were building Pinion, we always wanted to have it support auditing needs as mandated by Data Governance. In the audit layer, we had captured information about all the jobs which use Pinion Framework. This information includes the name of the job, target table it impacts, location of the incremental data set, Pinion API name, number of new rows added, number of rows updated or deleted, execution time, etc.
4. **Streamlined Logging —** If you have ever executed a Spark job, you will agree with me that Spark jobs spit out a lot of information in the logs, and finding the required information from the logs becomes a daunting task. Apparently, it becomes more challenging when you are running your job in a distributed environment since you have to deal with logs from all the nodes where your job was executed.
In Pinion, we had internally divided the execution of a job into multiple stages and ensured that logging APIs write all the relevant information needed to identify the stage in which it is currently executing. This information also helped in debugging a failed job and made the decision process much more efficient on action to be taken on a failed job.
The below image depicts the stage distribution of a Pinion Job.


Pinion Stages & Resolution


5. **Chained APIs —** Few of the pipelines had a requirement that could not be served by the execution of a single API, and it requires the execution of multiple APIs one after another to successfully capture SCDs. One of such scenarios was to chain 7 APIs. By default Delta Lake doesn’t provide chaining of multiple APIs, so we had decided to build two new APIs called “custom” and “custom_v2” to meet the requirements.


A critical piece in this implementation was to provide transactional nature on the execution of these APIs. This means that if the execution of any of the chained APIs failed we should roll back to the state of the table as it was before the transaction was started and it will commit to the new state only when all the API executions have been completed successfully.
The below example demonstrates the use of custom_v2 API, where 7 APIs (1 delete_insert, 6 merges) were chained together to complete its SCD.


You can see the complete YAML file[here](https://github.com/jsaurav/groupon-de-public/blob/main/wf_hly_transactions_edwprod_fact_gbl_transactions_activations_s3.yaml) .


Here, comes the last and most interesting section of this post.


**Small files problem with Delta Lake and its resolution -** As we already know Delta Lake expects that your data lake is backed by parquet file format, and one of the properties of parquet files is that it doesn’t support in-place updates, which means each update operation on a file always causes the creation of new parquet files.


Delta also works on the same principle. During any SCD operation on a delta table, if data belongs to any file/files need to be changed, delta makes a new copy of the file and writes the updated content to it, and updates the metadata in .json files about this update so any read operation on the delta table knows which files to read from to build the latest state. The vacuum could be used to flush the files which contain a stale state.


> During this process, delta also splits a larger parquet file into smaller files and as more SCD operations happen on data these smaller files start accumulating in the Delta Lake.


**E.g.** One of our Delta table size was 700 GB and when we loaded it for the first time there were 700 files of 1 GB each. Fast forward 2 months and this table have grown to 770 GB and the number of files was approximately 40K. and this was just the tip of the iceberg.


> At one point in time when our Delta Lake size was just 101 TB, there were more than 17 million files present in it and the average file size was ~ 6MB. It raised a bigger concern on the scalability of our Delta Lake.


Internally we were using AWS KMS API for managing encryption and decryption of parquet files during the writing and reading phase. KMS is not free, its usage cost is directly proportional to the number of files we read and write.


This is where things started going out of control because the number of files present in our Delta Lake was in millions and at this time our spending on KMS was way higher than any other AWS services.


The below image shows the table size and number of files present in it. You will observe even for a table size of .26 GB we were having 12,996 parquet files which ideally should be 1 parquet file since this was an unpartitioned table and we had hundreds of tables like this in our Delta Lake.


Snapshot of Pinion Table.


So, we have decided that before we onboard more tables into our Delta Lake we have to fix small file issues. We started doing research to check if open source Delta Lake has a solution to fix the problem of these small files. The only solution that was available at that time was to use[Auto Optimize](https://docs.databricks.com/delta/optimizations/auto-optimize.html) and unfortunately, it was not available in the open-source Delta Lake version. At this point, we were on our own and decided to build a[compaction layer](https://docs.delta.io/latest/best-practices.html#compact-files) in the Pinion itself so that we can run the compaction process on the delta table once the SCD operation is completed.


The key thing that we had kept in mind while building this solution was that it should be non-invasive, which means the pipelines which are already using the Pinion framework should not be impacted by this new change. Implementing the compaction service in Pinion was non-trivial however the trickier part was when to run the compaction process. We had to assess the following scenarios:


- Do we want to run compaction every time an SCD is performed on a table? The answer was no since this would have been highly inefficient because there may not be enough small files available for Pinion to compact after each SCD operation.
- Do we want to run at a set frequency e.g. run once a week?
The answer was no again because all of our pipelines don’t run at the same frequency. Some of them run hourly, some of them 4 times a day, some of them daily, and a few of them weekly and monthly.
Running compaction weekly may work well for daily jobs but it may be too late for hourly jobs and too early for monthly and weekly jobs.


As a result, the solution we had adopted was custom-tailored so that each and every table gets a fair chance of compaction at the time when it is needed most. We analyzed all the pipelines and captured metrics such as — frequency of pipeline, the current size of the table, average increment in table size after each run, and new files added after each run, and used these metrics to design a function that will tell the Pinion Framework if compaction is needed or not.


To store all the metrics mentioned above, we had slightly changed the Pinion architecture to introduce a persistence layer that stores metadata about all the pipelines and tables that are already onboarded into our delta lake. We created a new table ‘Pinion’ in MySQL instance on AWS to store all of the needed information about all the pipelines.


Below is the snippet of the DDL and for the complete DDL, check out the following[link](https://github.com/jsaurav/groupon-de-public/blob/main/pinion_ddl.sql) .


Below is the updated architectural diagram of the Pinion framework. As you can see a MySQL instance has been added to the mix and S3DeltaLakeService is updated to invoke CompactionService. It is the compaction service that decides whether compaction needs to be run or not.


Press enter or click to view image in full size


Updated Pinion Architecture after the introduction of MySQL and CompactionService.


After enabling the compaction process for the pipelines we were able to reduce the cost of KMS service by approximately 80% which was a great success for our team since it clears the path of onboarding other tables in Delta Lake. The below image shows that on August 12th we crossed over $1000 in KMS costs alone for a day and after that, we first applied a temporary fix that was running compaction across all the tables and result of which we can see that KMS cost on August 19th came down by almost 80%. Finally, we applied the permanent fix on September 4th, and then we started seeing a reduction from September 6th onwards. After that, we did the same activity for the partitioned table which brought down the cost further down, as you can see in the monthly cost explorer.


**Daily Cost Explorer -**


Press enter or click to view image in full size


KMS cost distribution pattern before and after compaction.


**Monthly Cost Explorer -**


Press enter or click to view image in full size


Monthly view of KMS cost.


In December 2020, when AWS introduced bucket keys we started making changes to use bucket keys for encrypting our data lake on S3 and eventually were able to get rid of the KMS cost.


In this two-part Pinion blog series, we have seen how we built it from scratch to meet our need to handle SCDs at a petabytes scale and how we solved some of the complex problems faced during its development and post-production.


Finally, a big thanks and shout out to the entire Data Engineering team and to everyone who was directly or indirectly involved in making Pinion Framework successful.
