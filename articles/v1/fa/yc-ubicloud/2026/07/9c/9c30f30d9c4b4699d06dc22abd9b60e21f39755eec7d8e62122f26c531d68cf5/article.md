---
schema_version: "1.0.0"
document_id: "9c30f30d9c4b4699d06dc22abd9b60e21f39755eec7d8e62122f26c531d68cf5"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/postgresql-performance-local-vs-network-attached-storage"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:d7be177dac7cdcd997fc035a53a65a8fdc63ad4470ef46e21e95c9de8ded1fdf"
---

# PostgreSQL Performance: Local vs. Network-Attached Storage

## **PostgreSQL Performance: Local vs. Network-Attached Storage**


May 30, 2025 · 3 min read


Burak Yucesoy


Principal Software Engineer


Cloud storage was built around the limits of old hardware. Spinning hard drives (HDDs) were slow and fragile. So, early on cloud providers moved storage off of servers. They used network-attached disks to boost durability and scalability. But hardware has come a long way. Modern NVMe SSDs have eliminated many of the constraints that originally led to network-attached designs. They are also much more affordable. You can get 2.5 million IOPS from a $600 NVMe SSD \[[1](https://www.ebay.com/itm/365440186543) \]. By contrast, pushing 2.5 million IOPS through Aurora would cost you $1.3M per month. With NVMe SSDs being faster, cheaper, and more reliable, it's time to rethink PostgreSQL storage.


In this post, we’ll explore how cloud database storage architectures have evolved, how advances in hardware have changed the landscape, and why local NVMe SSDs have become a viable option for cloud databases. We’ll also present benchmarks that compare performance across different storage architectures.


### Database Storage over the Years


Before the cloud, databases typically ran on local hard drives (HDDs). This provided low latency for sequential reads but introduced two challenges. Each HDD had a single head sitting on a platter, so your random read/write performance would be terrible. These HDDs also had high annual failure rates due to rotating disks. Most database teams would buy specialized hard drives and put them into fancy RAID configurations to compensate for these problems.


Then, cloud computing happened. AWS made a bold move and popularized pooling storage remotely across many machines. They connected servers to big clusters of HDDs over a network, solving two major issues at once. There were no bottlenecks due to single disk as you could spread I/O over many drives. It also provided higher redundancy and durability by replicating data in the background. At the time, database engineers worried that the extra network hop would kill performance. Surprisingly, it worked well enough and network-attached storage became the default for cloud databases.


With SSDs, trade-offs in cloud architecture have fundamentally changed.


- **Fast speed:** SSDs offer high throughput and low latency. This removes much of the performance advantage of spreading I/O across many drives.
- **No moving parts:** SSDs are much more reliable than HDDs. This has reduced the durability advantage that centralized storage used to offer.
- **Falling prices:** High-performance storage is now affordable. This makes SSDs a cost-effective option for intensive tasks.


Over time, SSDs became more popular due to their better performance, reliability and falling prices. So, cloud providers upgraded their network-attached storage to use SSDs. However, the fundamental design, storage accessed over the network, remained unchanged. This was partly due to path dependency and partly because network-attached storage still offers advantages in certain areas, which we’ll discuss the in next section.


The advancements in SSD technology continued even further with NVMe SSDs. Traditional SSDs often used SATA or SAS interfaces, originally designed for spinning disks. In contrast, NVMe was built specifically for flash memory. It connects directly over PCIe, enabling massively parallel data paths and reducing latency.


As we mentioned in the beginning, the cost of 2.5 million IOPS on Aurora is $1.3M/month, but you can achieve the same performance with a $600 local NVMe SSD. Yet cloud providers stuck with network-attached storage, largely out of inertia. At Ubicloud, we believe it’s time for a reset.


### Advantages of Network-Attached Storage


Local NVMe SSDs deliver major performance benefits. However, network-attached storage still has two key advantages: elasticity and durability.


Elasticity allows you to scale storage independently of compute. This is especially useful for unpredictable workloads with fluctuating storage needs. In contrast, local storage is coupled with the underlying compute. Scaling it often requires moving data to a new server with larger disks. It is possible to automate this process and perform it safely, but definitely adds some operational complexity.


Durability is another strength of network-attached storage. Centralized network storage systems are usually highly durable due to their built-in replication. That said, NVMe SSDs are already far more reliable than legacy HDDs. This minimizes the risk of disk failure. Still, when using local NVMe, it's important to have a replication and backup strategy in place. Thankfully, PostgreSQL already comes with robust primitives for replication, high availability, and backups. So, building a resilient system for PostgreSQL on top of local NVMe is entirely feasible.


In summary, network-attached storage offers certain operational advantages, but modern hardware, automation and database tooling make local NVMe a compelling choice in many scenarios. At Ubicloud, this is why we’re confident in using local NVMe for our managed PostgreSQL service.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)


### Benchmarks


We wanted to know how much better PostgreSQL could be with local NVMe drives, so we ran performance benchmarks across three platforms:


- **Ubicloud PostgreSQL:** standard-8 instance (comes with 8 vCPU, 32GB RAM and local NVMe SSD)
- **Amazon RDS for PostgreSQL:** db.m8g.2xlarge instance (comes with 8 vCPU, 32GB RAM) and GP3 EBS disk
- **Amazon Aurora for PostgreSQL:** db.r8g.2xlarge instance (comes with 8 vCPU, 64GB RAM) and I/O optimized disk.


To evaluate performance, we ran two industry-standard benchmarks:


- **TPC-C** : This benchmark emulates OLTP workloads, characterized by high concurrency and small transactions. We used sysbench to run TPC-C benchmark (tables: 32, scale: 256)
- **TPC-H** : This benchmark emulates analytical workloads, characterized by large scans and complex joins. We used 100 as the scale factor.


On the TPC-C benchmark, Ubicloud with NVMe drives processes 1.4 times more queries than Aurora and 4.6 times more queries than RDS. Latency at 99th percentile was 1.9 times less compared to Aurora and 7.7 times less than RDS.


Workload Ubicloud Aurora RDS


**Transaction/s**


873.25


636.31


188.3


**Query/s**


24815.08


18076.31


5350.79


**Latency**


314.45


601.29


2405.65


Drag table left or right to see remaining content


Moreover, latency was more stable and predictable with NVMe drives.


On the TPC-H benchmark, Ubicloud with NVMe drives were faster in all TPC-H queries. On average, it was 2.42 times faster than Aurora and 2.96 times faster than RDS.


Ubicloud Aurora RDS


Q01 75.4 170.4 174.8


Q02 108.65 238.86 129.69


Q03 60.95 101.4 223.59


Q04 119.65 552.57 355.73


Q05 63.19 83.69 223.53


Q06 45.91 61.42 179.26


Q07 60.96 76.28 223.72


Q08 69.92 92.86 254.33


Q09 84.94 1596.24 275.71


Q10 91.38 123.96 314.85


Q11 35.29 76.76 102.38


Q12 65.72 119.75 240.15


Q13 48.66 80.44 83.23


Q14 46.74 74.11 181.5


Q15 97.94 161.96 365.75


Q16 100.05 107.41 107.35


Q17 235.25 542.08 559.31


Q18 225.73 605.84 2142.52


Q19 23.17 161.77 111.9


Q20 1825.97 10666.92 9866.06


Q21 103.87 729.29 213.65


Q22 9.88 30.37 10.49


Mean (Geometric) 79.72 193.23 235.61


Difference - 2.42x 2.96x


Drag table left or right to see remaining content


### The Future of Cloud Databases Is Local


We’re at an inflection point. The cloud storage model built 15 years ago no longer makes sense for today’s hardware and workloads. For data intensive applications like PostgreSQL, local SSDs are the better default.
