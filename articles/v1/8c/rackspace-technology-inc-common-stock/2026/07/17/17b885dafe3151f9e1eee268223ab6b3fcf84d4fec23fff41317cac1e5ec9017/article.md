---
schema_version: "1.0.0"
document_id: "17b885dafe3151f9e1eee268223ab6b3fcf84d4fec23fff41317cac1e5ec9017"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/rds-instance-class"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-22T10:47:01.830546+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:260cb87f75620f28bb215067ef671b485f140f41c4b5f3d132873fc2f3c37f89"
---

# RDS Instance Class Explained: Types, Specs, How to Choose

Choosing the right[Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/) instance class affects your database's performance, scalability, and cost. Whether you're launching a new database or resizing an existing one, understanding RDS instance classes helps you select the right balance of compute and memory for your workload.


If you've opened the Amazon RDS console before, you've probably seen names like *db.m5.large* , *db.r6g.xlarge* , and *db.t3.medium* . At first glance, they don't tell you much, making it difficult to know which instance class your workload actually needs.


The decision matters more than ever. AWS regularly introduces new instance generations while retiring older ones, with families such as *db.m4* , *db.m3* , *db.r4* , *db.r3* , and *db.t2* already reaching end-of-support.


In this guide, you'll learn what RDS instance classes are, how they're named, the differences between each instance family, how to choose the right one for your workload, and how to upgrade an existing database instance.


## What Is an RDS DB Instance Class?


An[Amazon RDS instance class](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html) determines the compute and memory resources allocated to an RDS DB instance. Every instance class combines an instance family with a size, and not every class is supported across every database engine, version, or AWS Region.


That distinction is important. An instance class that's available for MySQL in one AWS Region may not be supported for SQL Server or Oracle in another, so checking compatibility before provisioning can help avoid deployment errors.


‍


### Understanding the RDS Instance Class Naming Convention


Every RDS instance class follows the same naming pattern:


**db.** + **family** + **generation** + **modifier (optional)** + **size**


For example, consider the instance class *db.r6g.2xlarge* :


- **Family letter** – Identifies the workload category, such as **m** (general purpose), **r** (memory-optimized), **t** (burstable), **c** (compute-optimized), or **x** and **z** (high-memory families).
- **Generation number** – Indicates the hardware generation. Higher numbers generally represent newer AWS hardware.
- **Modifier letters** – Additional letters describe specific hardware features, such as **g** for AWS Graviton processors, **i** for Intel processors, **d** for local NVMe storage, and **n** for enhanced networking.
- **Size suffix** – Specifies the relative amount of vCPUs and memory allocated to the instance, such as *large* , *xlarge* , or *2xlarge* .


In *db.r6g.2xlarge* , **r** identifies a memory-optimized family, **6** indicates the sixth generation, **g** represents AWS Graviton processors, and **2xlarge** defines the instance size.


Component Example Meaning


Service` db` Amazon RDS database instance.


Family` r` Memory-optimized instance family.


Generation` 6` Sixth-generation AWS hardware.


Modifier` g` AWS Graviton processor.


Size` 2xlarge` Relative CPU and memory allocation.


It's also worth noting that RDS instance classes and Amazon EC2 instance types use similar naming conventions but aren't interchangeable. For example, *db.r6g.2xlarge* and *r6g.2xlarge* are built on similar underlying hardware, but one is provisioned through Amazon RDS while the other is an Amazon EC2 instance.


Now that you know how instance class names are structured, the next step is understanding what each instance family is designed to do.


## RDS Instance Class Types


Amazon RDS instance classes are grouped into families based on the type of workload they're designed to support. Choosing the right family is just as important as choosing the right instance size, since each one is optimized for different CPU, memory, and storage requirements.


‍


### General Purpose (db.m)


General purpose instance classes balance compute, memory, and network performance, making them the default choice for many production databases. They're well suited to medium-sized databases, web applications, content management systems, ecommerce platforms, and development or test environments.


For example, a *db.m5.large* provides **2 vCPUs** and **8 GiB** of memory, offering enough resources for many small-to-medium production workloads without unnecessary overprovisioning.


Current-generation general purpose families include *db.m7g* and *db.m8g* , both powered by AWS Graviton processors. Older families such as *db.m4* and *db.m3* have reached end-of-support.


**Best for:** General-purpose production workloads, web applications, development, and testing.


‍


### Memory-Optimized (db.r, db.x, db.z)


Memory-optimized instance classes provide significantly more memory per vCPU than general purpose instances. They're designed for applications that cache large amounts of data in memory or maintain many simultaneous database connections.


For example, a *db.r6g.large* includes **2 vCPUs** and **16 GiB** of memory—twice the memory of a comparable *db.m5.large* . This makes the family well suited to high-concurrency OLTP databases, in-memory analytics, and enterprise workloads such as SAP HANA (X family).


Older families including *db.r4* and *db.r3* have reached end-of-support.


**Best for:** PostgreSQL, MySQL, high-concurrency OLTP, analytics, and memory-intensive applications.


‍


### Burstable Performance (db.t)


Burstable instance classes use a CPU credit model, allowing instances to accumulate CPU credits while idle and spend them during short bursts of higher utilization. This makes them an economical option for workloads with occasional spikes in activity.


When CPU usage exceeds the available credit balance, Unlimited mode allows workloads to continue bursting, with additional CPU usage billed separately instead of throttling performance.


Burstable instances work well for development environments, proof-of-concept deployments, and lightly used applications. However, they're generally not recommended for sustained high-CPU production workloads because continuous CPU usage can exhaust available credits and increase costs.


Current burstable families include *db.t3* and *db.t4g* . The older *db.t2* family has reached end-of-support.


**Best for:** Development, testing, staging, and workloads with intermittent CPU demand.


‍


### Compute-Optimized (db.c)


Compute-optimized instance classes prioritize CPU performance over memory capacity. Within Amazon RDS, they're currently available only for a limited set of workloads, primarily Multi-AZ DB cluster deployments.


Unlike general purpose or memory-optimized families, compute-optimized classes aren't intended as a default choice for most database workloads.


**Best for:** Specialized, CPU-intensive Multi-AZ database clusters.


‍


### Optimized Reads


Optimized Reads instance classes, including *db.m8gd* , *db.r8gd* , *db.r6gd* , and *db.r6id* , use local NVMe storage to accelerate read-heavy database workloads.


Rather than providing a general performance improvement, these classes are designed to reduce read latency and improve throughput for workloads that frequently access cached or temporary data.


**Best for:** Read-heavy databases, analytics, and latency-sensitive workloads.


‍


### Deprecated and End-of-Support Classes


AWS regularly retires older hardware generations as newer instance families become available.


Instance classes including *db.m4* , *db.m3* , *db.r4* , *db.r3* , and *db.t2* have reached end-of-support or been fully retired. AWS automatically upgrades supported instances to the nearest equivalent generation during scheduled maintenance, but upgrading proactively allows you to test performance and compatibility on your own schedule.


Knowing the instance family is only the first step. Next, let's compare the specifications of the most commonly used RDS instance classes to help you choose the right size for your workload.


## RDS Instance Class Comparison Table


The following table compares some of the most commonly used Amazon RDS instance classes across different instance families. It highlights their vCPU count, memory allocation, network performance, and the workloads they're typically best suited for.


**Note:** Specifications vary by database engine and AWS Region. Always verify the latest supported instance classes in the AWS documentation before deploying production workloads.


Instance Class Family vCPUs Memory (GiB) Network Performance Typical Use Case


` db.t3.small` Burstable 2 2 Up to 5 Gbps Development, testing, and low-traffic applications.


` db.t3.medium` Burstable 2 4 Up to 5 Gbps Development, testing, and staging environments.


` db.t4g.medium` Burstable 2 4 Up to 5 Gbps Development and testing workloads on AWS Graviton.


` db.m5.large` General Purpose 2 8 Up to 10 Gbps Small-to-medium production databases.


` db.m6g.large` General Purpose 2 8 Up to 10 Gbps General production workloads on AWS Graviton.


` db.m7g.large` General Purpose 2 8 Up to 12.5 Gbps Current-generation production workloads.


` db.r6g.large` Memory-Optimized 2 16 Up to 10 Gbps High-concurrency OLTP databases.


` db.r6g.xlarge` Memory-Optimized 4 32 Up to 10 Gbps In-memory analytics and larger OLTP workloads.


## How to Choose the Right RDS Instance Class


Choosing the right RDS instance class starts with understanding your workload rather than simply selecting the newest or largest instance available. Before provisioning a database instance, consider the following factors:


- OLTP versus OLAP workload pattern
- Read/write ratio
- Number of concurrent connections
- Memory footprint
- Sustained versus bursty CPU usage
- Database engine compatibility (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Amazon Aurora don't all support every instance class)


### Choose an Instance Family Based on Your Workload


For most workloads, selecting the right instance family is straightforward.


Workload Recommended Instance Family Why


Balanced production workloads **db.m** Balanced CPU, memory, and networking for general-purpose databases.


Memory-intensive or high-concurrency workloads **db.r** Higher memory-to-vCPU ratio for analytics and large transactional databases.


Development, testing, or bursty workloads **db.t** CPU credit model keeps costs low for intermittent workloads.


Choosing the right instance class is only part of the decision. Storage, high availability, and changing workload requirements all influence your database's performance and cost, so it's worth reviewing your deployment as your application grows.


## Comparing Amazon RDS and Rackspace Spot DBaaS Costs


Choosing the right instance class is only one part of managing database costs. If you're also evaluating managed PostgreSQL platforms, it's worth comparing how different providers price compute and high availability.


Amazon RDS bills compute, storage, backups, and Multi-AZ deployments separately. For Multi-AZ deployments, AWS provisions a standby instance that's billed as an additional database instance.


[Rackspace Spot DBaaS](https://spot.rackspace.com/docs/en/databases) uses a different pricing model. It currently provides fully managed PostgreSQL 17 with one replica included in the base flavor price. A second replica can be added for a flat **$15/month** , while storage is billed separately. This pricing structure can make highly available PostgreSQL deployments more predictable compared to paying for a second full-price database instance.


### Amazon RDS vs Rackspace Spot DBaaS Pricing


Rackspace Spot Flavor vCPU / RAM Rackspace Spot Monthly Closest Amazon RDS Class Amazon RDS Single-AZ* Amazon RDS Multi-AZ*


Small 1 vCPU / 2 GB $30 db.t3.small* ~$26 ~$53


Medium 2 vCPU / 8 GB $60 db.m5.large ~$130 ~$260


Large 4 vCPU / 16 GB $120 db.m5.xlarge ~$260 ~$520


Extra Large 8 vCPU / 32 GB $240 db.m5.2xlarge ~$520 ~$1,040


Amazon RDS pricing is approximate and varies by AWS Region, database engine, licensing model, and deployment options. The db.t3.small comparison is based on the closest available Amazon RDS instance class and is not an exact hardware equivalent.


### Example Cost Comparison


For example, a **Medium** Rackspace Spot DBaaS flavor with **2 vCPUs, 8 GB of RAM, 32 GB of storage, and two replicas** costs approximately **$81.40/month** , including **$60** for compute, **$15** for the second replica, and **$6.40** for storage.


A comparable Amazon RDS deployment using a **db.m5.large** instance with **Multi-AZ** and **32 GB of gp3 storage** costs approximately **$267/month** because AWS bills both the primary and standby instances, along with their associated storage.


In this example, Rackspace Spot costs roughly **70% less** for a comparable compute and high-availability PostgreSQL deployment.


It's also worth noting where Amazon RDS has an advantage. AWS charges approximately **$0.115/GB-month** for gp3 storage (Single-AZ), compared to Rackspace Spot's flat **$0.20/GB-month** . The overall savings come primarily from Rackspace Spot's pricing for compute and high availability rather than lower storage costs.


For organizations running development environments, staging databases, or moderate-sized production PostgreSQL workloads, Rackspace Spot DBaaS can offer a simpler and more predictable pricing model, particularly when Amazon RDS Multi-AZ deployment costs make up a significant portion of the monthly bill.


Learn more about Rackspace Spot DBaaS, including available PostgreSQL flavors, storage options, and pricing[here](https://spot.rackspace.com/docs/en/databases) .


## How to Change or Upgrade Your RDS Instance Class


You can change an Amazon RDS instance class at any time through the AWS Management Console, AWS CLI, or API. In the console, open the[Modify DB Instance page](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.DBInstance.Modifying.html) , select the new instance class, and choose whether to apply the change immediately or during the next scheduled maintenance window. Using the AWS CLI, you can make the same change with the` modify-db-instance` command and the` --db-instance-class` parameter.


Applying the change immediately starts the modification right away, along with any other pending changes for the instance. Deferring the change until the next maintenance window reduces the impact on production workloads but may delay the upgrade for several days.


The amount of downtime depends on your deployment type. In a **Multi-AZ deployment** , Amazon RDS performs a failover to the standby instance, so the interruption is typically brief. In a **Single-AZ deployment** , the database instance is unavailable for the duration of the modification, which can take several minutes depending on the size of the change.


Changing the instance class does **not** migrate your data. The underlying storage volume remains unchanged, while only the compute and memory resources are updated. Before upgrading, it's also good practice to verify that your database parameter group is compatible with the new instance class.


## Conclusion


Choosing the right Amazon RDS instance class is a balance between performance, scalability, and cost. By understanding the different instance families, comparing their specifications, and matching them to your workload, you can avoid overprovisioning while ensuring your database has the resources it needs.


Because AWS regularly introduces new instance generations and retires older ones, it's worth reviewing your deployment periodically to ensure you're still using the most appropriate and cost-effective instance class.


If you're also evaluating managed PostgreSQL platforms, comparing pricing models and high-availability options can help you determine whether Amazon RDS or an alternative such as Rackspace Spot DBaaS is the better fit for your workload.


## Frequently Asked Questions


### What is an RDS instance class?


An Amazon RDS instance class determines the compute and memory resources allocated to an RDS database instance. Each instance class combines an instance family, such as **db.m** or **db.r** , with a size like **large** or **2xlarge** . Not every instance class is supported across every database engine, version, or AWS Region.


‍


### What are the different RDS instance class types?


Amazon RDS instance classes fall into five main categories:


- **General purpose (db.m)** for balanced workloads.
- **Memory-optimized (db.r, db.x, db.z)** for memory-intensive and high-concurrency databases.
- **Burstable performance (db.t)** for workloads with intermittent CPU usage.
- **Compute-optimized (db.c)** for specialized CPU-intensive Multi-AZ DB cluster deployments.
- **Optimized Reads** for read-heavy workloads that benefit from local NVMe storage.


‍


### How do I choose the right RDS instance class?


Start by evaluating your workload's CPU, memory, storage, and concurrency requirements. General-purpose workloads typically use **db.m** , memory-intensive applications benefit from **db.r** , and development or bursty workloads are often best suited to **db.t** . It's also important to consider your database engine, storage type, and high-availability requirements when selecting an instance class.


‍


### What's the difference between db.m, db.r, and db.t instance classes?


- **db.m** provides a balanced mix of compute and memory for general-purpose workloads.
- **db.r** offers a higher memory-to-vCPU ratio for memory-intensive and high-concurrency databases.
- **db.t** uses a CPU credit model, making it well suited to development, testing, and workloads with occasional CPU spikes.


‍


### Is data migrated when upgrading an RDS instance class?


No. Changing an RDS instance class does **not** migrate or move your data. The underlying storage volume remains attached, while only the compute and memory resources assigned to the database instance are updated.


‍


### How do I change or upgrade my RDS instance class?


You can change an RDS instance class through the **Modify DB Instance** page in the AWS Management Console or by using the AWS CLI. Choose whether to apply the change immediately or during the next maintenance window. Multi-AZ deployments typically experience a brief failover, while Single-AZ deployments remain unavailable for the duration of the modification.


‍


### Which RDS instance classes are being deprecated or retired?


Older instance families, including **db.m4** , **db.m3** , **db.r4** , **db.r3** , and **db.t2** , have reached end-of-support or been retired. AWS recommends upgrading to newer generations to benefit from improved performance, new features, and continued support.


‍


### What public cloud alternatives to Amazon RDS provide more predictable pricing for PostgreSQL?


[Rackspace Spot DBaaS](https://spot.rackspace.com/docs/en/databases) can cost significantly less than Amazon RDS for PostgreSQL workloads. For example, a comparable high-availability deployment costs approximately **$81/month** on Rackspace Spot versus **$267/month** on Amazon RDS. Rackspace Spot currently supports PostgreSQL only.
