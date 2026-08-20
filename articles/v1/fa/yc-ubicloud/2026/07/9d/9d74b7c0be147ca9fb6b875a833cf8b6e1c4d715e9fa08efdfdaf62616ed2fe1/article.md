---
schema_version: "1.0.0"
document_id: "9d74b7c0be147ca9fb6b875a833cf8b6e1c4d715e9fa08efdfdaf62616ed2fe1"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/clickhouse-postgresql-powered-by-ubicloud"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:9a24e0ffc3e79cb1df60b41450e3fee5202fe9cd3e718ccc7dbc92020384ad58"
---

# ClickHouse PostgreSQL powered by Ubicloud

## **ClickHouse PostgreSQL powered by Ubicloud**


January 22, 2026 · 4 min read


Umur Cubukcu


Co-founder / Co-CEO


Today we're excited to announce our partnership with ClickHouse: Managed Postgres as part of the ClickHouse Cloud platform, powered by Ubicloud Postgres. ClickHouse customers can now use Postgres for their transactional workloads, tightly integrated with ClickHouse for analytics.


This gives you a unified data stack: Postgres for transactions, ClickHouse for analytics, with native capabilities to sync and query data across datastores. Ubicloud Postgres, backed by fast, local NVMe storage, powers the Postgres experience.


This partnership also expands from the product experience to an open source collaboration: Ubicloud and ClickHouse teams build with and contribute to the Ubicloud open source project. You can already see the ClickHouse team’s[contributions on GitHub](https://github.com/ubicloud/ubicloud/issues?q=is%3Apr%20author%3Aserprex%20OR%20author%3AiamKunalGupta%20OR%20author%3Aheavycrystal) .


### The Product


Ubicloud PostgreSQL powers our customers on bare metal clouds, and more recently, also on[AWS infrastructure](https://www.ubicloud.com/postgresql-on-aws-high-iops) .


Among many of its key features, Ubicloud Postgres uses NVMe disks for strong Postgres price-performance. Our benchmarks show up to 9x faster transaction speeds compared to AWS RDS, and our customers report[even more significant gains](https://www.ubicloud.com/customer-stories/hatchet) .


‍ *Ubicloud on AWS: Performance comparison (pg_bench and TPC-H)*


Starting today, ClickHouse customers can provision PostgreSQL directly in the ClickHouse Cloud platform. Ubicloud Postgres, including all customer data, runs inside the ClickHouse Cloud environment.


The integration with ClickHouse also includes native Change Data Capture, so data flows from Postgres to ClickHouse without you having to build and maintain that pipeline yourself.


This matters for many workloads, including also AI applications. It is very common to need operational data stores for serving AI apps, and analytical capabilities for insights on operational data. With Postgres and ClickHouse working together, you can build these systems without stitching together multiple vendors and dealing with the operational overhead.


### Why this Partnership


Postgres is the world's most advanced open source database. ClickHouse is the leading, fast open source analytics database. Combining them makes sense.


There's also a history here. My co-founders, our team and I spent years building managed Postgres at scale. We created Citus Data together, which brought distributed Postgres to mainstream before Microsoft acquired us in 2019. At Microsoft Azure, we ran one of the largest managed Postgres services, serving thousands of enterprises and startups with Postgres and Citus. The roots of our managed Postgres service go back even earlier, to our co-founder Daniel's time building Heroku Postgres more than 15 years ago.


When ClickHouse was looking for a partner for their managed Postgres, they needed a team that could deliver enterprise-grade reliability and performance. We knew their Postgres team closely: Sai at ClickHouse was a key member of our Citus Data team. After Microsoft, he and Kaushik started PeerDB—a change data capture tool connecting Postgres to other data stores—which ClickHouse acquired.


Beyond the trust between the teams, the products complement each other very well. Ubicloud is among the fastest PostgreSQL services in the market today (if not the fastest; but we'll leave that for another day). It has an enterprise-grade feature-set, hardened through a decade of iterations, including high-availability, backups, point-in-time restore, connection pooling, upgrades, encryption, private networking, APIs, and more. And importantly,[Ubicloud is open source](https://github.com/ubicloud/ubicloud) , making development collaborative and transparent.


### How it Works


At Ubicloud, our control plane runs and manages our cloud services, including Postgres. The control plane orchestrates the data plane, which is where your actual Postgres instances run.


When the data plane sits on bare metal machines, we run your Postgres databases on high-performance servers that we virtualize. When the data plane runs on AWS, we use EC2 instances with NVMe disk.


ClickHouse uses the Ubicloud control plane within their cloud environment to power the ClickHouse Postgres service. Because the Ubicloud control plane is open source, they contribute directly to Ubicloud.


Our engineering teams work together daily—sharing code, optimizing performance, building features. You can see this in our open source repositories, where ClickHouse engineers already[contribute](https://github.com/ubicloud/ubicloud/pulls?q=is%3Apr+author%3Aserprex) to the Ubicloud project.


This is how we think open source should work: transparently, with shared code and aligned incentives.


### What this means for Ubicloud Customers


This partnership accelerates our PostgreSQL roadmap. We're investing more in enterprise features, performance optimizations, and deployment options. We're building capabilities that serve both our direct customers and the ClickHouse ecosystem.


The partnership also strengthens the Ubicloud open source project. With ClickHouse team members contributing, we're expanding our community and ensuring the platform evolves to meet production needs.


### Looking Forward


This partnership demonstrates how open source companies can work together to deliver more value than either could alone. One solution, two teams working together, shared code and value for teams worldwide.


ClickHouse Postgres service is available today in private preview, and you can sign up for it[here](https://clickhouse.com/cloud/postgres) . To use Ubicloud PostgreSQL on AWS, Hetzner, Leaseweb or other clouds, you can use the[Ubicloud console](https://console.ubicloud.com/) , across regions in Virginia (us-east), Oregon (us-west), Germany (Frankfurt, Falkenstein), Singapore and Sao Paulo.


Ubicloud remains the only open source solution for managed Postgres that you can run anywhere. With ClickHouse as our partner, we're bringing that to more users.


We're looking forward to seeing what you build with it.
