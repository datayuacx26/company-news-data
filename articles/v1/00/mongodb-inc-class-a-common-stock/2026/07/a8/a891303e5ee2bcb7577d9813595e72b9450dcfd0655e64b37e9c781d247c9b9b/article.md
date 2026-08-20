---
schema_version: "1.0.0"
document_id: "a891303e5ee2bcb7577d9813595e72b9450dcfd0655e64b37e9c781d247c9b9b"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/introducing-atlas-gen2-on-aws-m30-dedicated-clusters"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-24T04:07:24.248205+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:5e52006624db24e0337d2e9701a19a1c98cfe7c288b6038ff259a72ac1236301"
---

# Introducing Atlas Gen2 on AWS for M30+ Dedicated Clusters

Today, we’re introducing[Gen2 for Atlas M30+ Dedicated Clusters on AWS](https://www.mongodb.com/docs/atlas/manage-clusters/#aws-gen2-dedicated-clusters) : a major step forward in performance, cost control, and infrastructure flexibility for teams running production workloads on[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) .


For many teams, improving database performance comes with an expensive compromise: needing more IOPS often means buying more storage, whether you need that storage or not. The result? Overprovisioned clusters, higher cloud bills, and infrastructure that’s sized around platform limitations instead of workload requirements.


Atlas Gen2 changes that. By combining significant performance improvements with the ability to scale IOPS independently from storage, Gen2 gives teams a more efficient way to run MongoDB on AWS, without application changes, migrations, or operational disruption.


## More performance without more infrastructure


Atlas Gen2 on AWS runs on the newer ARM-based processors, delivering significant performance improvements compared to the previous generation used by many Atlas clusters. In an internal pilot evaluating Atlas clusters migrated to Gen2 infrastructure, MongoDB observed a statistically significant 15–20% reduction in CPU utilization, improved memory efficiency, and approximately 45% lower read and write latency.


For organizations, these gains can translate into faster response times, higher transaction throughput, and the ability to handle peak demand without adding infrastructure. The additional performance headroom helps teams accommodate growing workloads without automatically moving to larger cluster tiers, and in some cases, customers may be able to achieve performance targets on smaller clusters, improving efficiency and lowering the total cost of ownership.


Simply put: teams get more performance from the infrastructure they are already paying for.


## End the storage-for-IOPS tradeoff


Performance gains are only part of the story. Atlas Gen2 introduces extended standard IOPS, allowing customers to provision IOPS independently from storage capacity.


This addresses one of the most common and costly inefficiencies in cloud database infrastructure: overprovisioning storage just to get the disk performance a workload requires. Historically, many teams have had to purchase additional storage solely to unlock the disk performance their workloads required—even when that storage sat largely unused. With Gen2, storage and performance can scale independently.


That means:


-


Provision storage based on capacity needs, not performance requirements


-


Increase IOPS without overprovisioning storage


-


Gain more precise control over infrastructure costs


For platform teams and DBAs, it’s a simpler way to optimize clusters. For engineering leaders, it’s a clearer path to reducing infrastructure waste.


## Better price-performance without the migration


Gen2 is an important step in strengthening Atlas’s price-performance on AWS and reflects MongoDB’s ongoing commitment to helping customers get more value from their infrastructure investments. By continuously listening to customer feedback and investing in platform innovation, MongoDB is focused on delivering higher performance, greater efficiency, and lower operating costs without increasing operational complexity.


Customers can capture these benefits without taking on the cost and risk of migration—there are no application changes required, no vendor switch, and no rearchitecture. Teams keep the same Atlas API, connection strings, and operational model while benefiting from newer infrastructure underneath. Atlas Gen2 on AWS will be generally available on June 30 for eligible Atlas customers, with initial availability in select AWS regions. See the latest information on[supported regions, cluster eligibility, and deployment guidance.](https://www.mongodb.com/docs/atlas/reference/amazon-aws/#aws-gen2-dedicated-clusters)


## Built for long-term growth


Gen2 also helps customers move to newer AWS infrastructure designed to support future growth and avoid the capacity constraints that can emerge on older hardware generations.


Customers retain access to the Atlas capabilities they rely on today, including Global Clusters, autoscaling, PrivateLink, and VPC peering, while benefiting from the latest generation of infrastructure.


## Availability: A smarter way to scale on AWS


Gen2 is available for Atlas M30+ Dedicated Clusters on AWS and is initially supported in regions where the underlying infrastructure is available. For customers creating new clusters in supported regions, Gen2 will be the recommended default infrastructure. Existing customers running eligible M30+ clusters on AWS can evaluate upgrade opportunities and determine where Gen2 can deliver better performance and smarter cost optimization.


Atlas Gen2 for M30+ Dedicated Clusters on AWS gives teams a simpler answer to a familiar problem: how to improve performance without overpaying for infrastructure they don’t need. By combining significant performance improvements with independent IOPS scaling, Gen2 helps customers reduce waste, right-size more effectively, and improve price-performance on AWS, all while staying on Atlas.


###### Next Steps


Ready to explore Atlas Gen2 on AWS? Check the[Atlas Gen2 documentation](https://www.mongodb.com/docs/atlas/manage-clusters/#aws-gen2-dedicated-clusters) for eligibility details, supported regions, and deployment guidance to see how your workloads can benefit from improved performance and efficiency.
