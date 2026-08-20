---
schema_version: "1.0.0"
document_id: "61780b4caaf17d6d2e34b3623b4270dd5c26222ac6093e24fcba450e18df4db2"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-6ed493c8d31a"
canonical_url: "https://archil.com/article/ai-storage-architecture-tiers-training-inference"
published_at: null
first_seen_at: "2026-08-09T19:14:46.011593+00:00"
fetched_at: "2026-08-09T19:14:47.557372+00:00"
content_hash: "sha256:22208acbef65b076e7ef79959714725bcbd3a42c197c44e0a0d14bdea898fcf1"
---

# Storage in the AI Pipeline: From Ingestion to Training

The success of many machine learning systems hinges not only on algorithms and compute, but on the data and the data pipeline it is involved with. Storage architecture is often neglected but can impact if your training takes hours instead of days and the cost of your cloud service bill.


There are many storage considerations across the AI pipeline, from initial data ingestion through model training. We will consider everything from performance, cost, ease of use, and the tradeoffs you make when picking a solution.


## AI Pipeline Phases


Before we can talk about the different levels of storage we must first understand the different phases of the AI pipeline and how data is used in it. The AI Pipeline has very similar stages to a standard data pipeline with some small differences:


1. **Data Ingestion** : Where raw data arrives from various sources like databases, APIs, file systems, streaming sources. For AI this is continuous as more and more training data needs to be added in once available.
2. **Data Lake/Landing Zone** : Initial storage of raw, unprocessed data. The data here is rarely usable for your model.
3. **Data Processing & ETL** : Processing means making your data ready for your model to be trained on it. This includes cleaning, normalizing, and even manual labeling. Data Processing is a crucial step as poor data can ruin your model.
4. **Training Data Storage** : Optimized datasets for model training, these files are read from frequently during training.
5. **Model Training** : The processed data goes into a training workflow with repeated long runs that can take days. As more data flows through the pipeline, this step is frequently repeated.
6. **Serving Infrastructure** : Model deployment and inference. This is what deployed models use to store data for inference. From a latency and availability perspective this portion is the most valuable.


Each stage serves a different purpose but for the most part the bottleneck in AI pipelines is at the read-heavy stage like **Model Training** and the **Serving Infrastructure.**


## Storage Tiers


The different phases of the AI Pipeline create value in storage tiering. Historical data can be stored in a cold storage while inference data needs to be more readily accessible. With storage the tradeoffs are usually speed and scale for cost. When training sets can approach the petabyte range, the storage solution you choose matters a lot. For example S3 costs ~$23,000 a month for a petabyte of storage while EFS would cost ~300k monthly. The tiers are:


### Hot Tiers (High Performance SSD/NVMe)


- **Best for:** real-time inference, high-throughput training
- **Purpose:** Low latency and time sensitive workflows. This is the storage you want to use for your S **erving Infrastructure** and sometimes **Model Training** to make sure you are completing it as fast as possible. One caveat with SSDs is that they are so fast that oftentimes network latencies overshadow the performance gains you get from them so having a physically attached one gives the best performance.
- **Cost: $$$** , high performance SSDs are one of the most expensive common storage solutions.
- **Performance:** Sub-millisecond latency (` <1ms` ) with 100k+ IOPS


### **Warm Tier** (Standard SSD or high-performance HDD):


- **Purpose:** Good for workloads that still need efficiency but not extreme optimization. Some examples can be intermediate data processing where peak efficiency isn’t needed, but slower IOPS can cause jobs to take a while.
- **Cost** : **$$** , a middle ground between the other two tiers in price and function
- **Performance** : Moderate latency (5-10ms), 10k-50k IOPS


### Cold Tiers (Object Storage):


- **Best for:** Data Lake/Landing Zone, archival training datasets
- **Purpose:** This is best for storing anything and everything thats not accessed frequently. This can be historical datasets or audit and compliance logs. This is also by far the easiest to maintain as there are no storage limits and redundancy can be achieved by a single config change.
- **Cost** : **$** , the cheapest option storage and operationally
- **Performance** : Higher latency (50-100ms), but excellent throughput for large sequential reads


A practical implementation of this would be to automatically move data between data tiers depending on how recently accessed it was. If hot data has not been read for a day for example it can be moved to a warm or cold tier to save costs and free up space on your SSD. However this is easier said than done because locating your data across different storage solutions and migrating it does require overhead.


## Optimizing Storage


The theoretical cost savings from storage tiering are significant, but realizing them requires careful implementation. Moving a petabyte of training data from EFS to S3 could save $277,000 monthly, but only if you can maintain performance and manage the operational complexity. Here's how to actually optimize your storage across the AI pipeline:


### Automated Tiering Based on Access Patterns


The simplest optimization is moving data based on access patterns. This is similar to what[S3 Intelligent Tiering](https://aws.amazon.com/s3/storage-classes/intelligent-tiering/) does to let customers cut costs. In training workloads, data is accessed in bursts then left alone for a while which justifies the added cost of that come with shifting data tiers.


However using built-in cloud service provider tiers is often not good enough because they operate on coarse timeframes that cannot be configured so they likely will not match your workflow. Also, they operate between moving data within the same cloud service rather than across them. For example, AWS service automatically moves data from S3 to EFS/EBS based on access patterns. However, when implementing a tiering strategy there are a lot of factors that need to be considered:


- **Rehydration Strategy:** Once your training workflow begins, rehydrating a full NVMe can take a minutes to populate. For inference workflows this is way too long but could be fine for training
- **Data Transfer Costs:** While you could be saving money on storage costs by demoting your storage tier, transferring data back and forth has costs. Ingress and egress out of cloud service providers come with fees that can leave you paying more than you saved.
- **Data Distribution:** If many servers are reading and writing to the same storage, which they often are in AI training workflows, making sure data is deduped and consistent is no small task. When building automated tiering, you should consider that each storage solution will likely require a different access pattern.
- **Complexity Tax:** Arguably the most expensive part of tiering is the extra developer bandwidth it takes to move between different storage solutions. Preventing data loss or delays in caching for critical workflows could easily burn the cost benefits you get from using a cheaper storage tier.


For many teams, implementing custom tiering logic isn't worth the engineering effort, especially when the complexity tax outweighs the storage savings. This has created a market for tools that handle storage tiering automatically, abstracting away the data movement and cache management.


These solutions typically work by presenting a unified storage interface to your applications while intelligently moving data between fast and cheap storage tiers behind the scenes. When your training job requests data, the system ensures it's available on fast storage; when that data goes cold, it's automatically demoted to object storage without your intervention.


[Archil](https://archil.com/) volume storage uses S3 to store inactive data while using faster SSD storage when the data is active. It automatically purges the cache and writes the data back to S3 when complete. If you are looking to reduce your storage costs while maintaining ambitious latency metrics, try using[Archil](https://archil.com/) in your data pipeline.
