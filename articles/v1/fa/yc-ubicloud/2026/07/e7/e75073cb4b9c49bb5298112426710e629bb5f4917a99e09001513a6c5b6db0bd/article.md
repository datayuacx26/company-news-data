---
schema_version: "1.0.0"
document_id: "e75073cb4b9c49bb5298112426710e629bb5f4917a99e09001513a6c5b6db0bd"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/ubicloud-postgresql-new-features-higher-performance"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:305630ce5cfa9665e50bedffaeb21bdd69320426eff8dc72cb670ecbf86fbae2"
---

# Ubicloud PostgreSQL: New Features, Higher Performance

## Ubicloud PostgreSQL: New Features, Higher Performance


May 20, 2025 · 5 min read


Furkan Sahin


Senior Software Engineer


Postgres holds a special place for us. As the Ubicloud team, we built four managed Postgres solutions (including Heroku Postgres and Citus) over the years. When[we launched Ubicloud Postgres last year](https://www.ubicloud.com/blog/difference-between-running-postgres-for-yourself-and-for-others) , it was in a way our fifth managed Postgres service.


From the outset, we mapped out the following feature list for our managed database. This list was based on hundreds of customer conversations; and we believed it to define a powerful managed PostgreSQL service.


Today, we are excited to announce a major upgrade to Ubicloud Postgres. The following features are now available as part of our managed service:


- Read Replicas
- Scale up/down
- High Availability (HA) across multiple availability zones (multi-AZ)
- Connection pooling
- Managed Maintenance Window
- Metrics
- External APIs


### Read Replicas


Read replicas allow you to create one or more copies of your primary PostgreSQL database instance. These replicas continuously follow your primary database using PostgreSQL’s replication mechanisms to stay up to date with changes. With Ubicloud, you can now spin up read replicas with a single click.


### Why Read Replicas Matter?


- **Scalability and isolation:** Read replicas empower you to scale your database horizontally, distributing read-heavy workloads-such as reporting, analytics, and real-time dashboards-across multiple dedicated replicas. By directing analytical and business intelligence queries to these replicas, you keep your primary instance focused and responsive for write operations and critical transactional workloads. This separation also ensures that your core application remains isolated from unpredictable or resource-intensive queries run by analysts or reporting tools.


- **Business continuity and disaster recovery (BCDR):** Read replicas can play a critical role in case of a primary database failure, a read replica can be promoted to primary, ensuring minimal downtime and data loss.


### Scale Up/Down


With Ubicloud, you can resize your database instance according to the needs of your business with just a few clicks. Scaling up or down is one of the simplest ways to keep your managed PostgreSQL database both high-performing and cost-effective, no matter how your needs evolve.


### Why Scale Up/Down?


- **Prepare for Growth:** Instantly scale up ahead of product launches, seasonal spikes, or intensive analytics workloads to ensure top performance.


- **Optimize for Cost:** Scale down during quieter periods to reduce costs, so you only pay for what you need.
- **Stay Agile:** Respond to changing trends without overprovisioning or manual intervention


### High Availability Across Multiple Datacenters (Multi-AZ)


High Availability (HA) is critical for any production database. Ubicloud Managed PostgreSQL now supports HA configurations across multiple datacenters (multi-AZ).


This means that your managed database spans across availability zones, ensuring your application stays online even if an entire datacenter experiences an outage. Ubicloud PostgreSQL’s automated failover ensures minimal downtime and seamless continuity for your applications.


### How It Works


- In the Ubicloud dashboard, enable high availability and select your preferred standby count.


- Ubicloud provisions your primary instance and up to two standby servers across separate data-centers.


- Replication is managed automatically - your data stays in sync and we automatically failover to a standby in case of a failure.


- In the event of a data center failure, we automatically redirect traffic to a healthy standby in another data center.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)


### Connection Pooling (PgBouncer)


Connection pooling is now built into Ubicloud PostgreSQL, making it more performant and scalable.


### Why Connection Pooling?


PostgreSQL performance can degrade with the number of parallel connections because PostgreSQL creates a new backend process for every client connection. This can quickly consume memory and CPU as your application grows.


Connection pooling improves database performance by managing persistent connections, instead of constantly opening and closing them. This reduces latency and increases throughput. It’s especially beneficial for applications that make numerous short, intermittent database requests.


Ubicloud uses PgBouncer, the industry-standard connection pooler for PostgreSQL. You can start using it simply by updating your application’s connection string to point to the port 6432, instead of the default port 5432. Firewall rules are applied the same way to the pooling port as well.


With built-in connection pooling, Ubicloud PostgreSQL provides a high performance, stable and reliable PostgreSQL experience.


### Managed Maintenance Windows


A managed maintenance window lets you control when essential updates-like configuration changes, minor updates, and system improvements-are applied to your managed PostgreSQL instance. Instead of unexpected interruptions, you decide the best time for maintenance, ensuring your database stays secure and up-to-date with minimal impact on your application.


### How It Works


- In the Ubicloud dashboard, you can set a preferred maintenance window for each managed PostgreSQL instance.
- Ubicloud will automatically apply updates only during your chosen window, keeping you in control of when changes happen.
- Maintenance operations are fully managed: you don’t need to monitor or manually apply patches-Ubicloud handles it for you.


### Metrics


Our UI now has all the metrics you need to keep track of your database usage. You can see metrics like CPU, memory or disk usage at 15 seconds granularity. This way, you can diagnose and resolve issues that might degrade your database performance. You can also decide to scale your database according to your needs.


### Manage Everything Through APIs


Ubicloud PostgreSQL is designed for automation and integration from the ground up. Every major operation can be performed programmatically through our APIs. These operations include creating databases, scaling resources, enabling or disabling high availability (HA), provisioning read replicas, and more.


With Ubicloud’s API-first approach, you’re in control - whether you’re managing a single instance or orchestrating multiple databases across regions.


### Conclusion


Ubicloud PostgreSQL brings together years of expertise and a focus on what matters most: reliability, performance, and control. With our new features, you have the tools you need to build on a modern PostgreSQL database.


What sets Ubicloud apart from other solutions are:


- We use local NVMes. This helps us deliver price/performance that’s 7.9 - 11.1x better than Aurora PostgreSQL.
- We have years of experience with PostgreSQL. This makes us keep reliability as a forethought when releasing new features.
- We provide the first[open-source solution](https://github.com/ubicloud/ubicloud) for managed PostgreSQL. This keeps you in control of your database.


If you have questions or feedback, our team would love to hear from you at[\[email protected\]](https://www.ubicloud.com/cdn-cgi/l/email-protection#6b181e1b1b04191f2b1e09020807041e0f45080406) .
