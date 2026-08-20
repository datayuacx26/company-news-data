---
schema_version: "1.0.0"
document_id: "a62a1ad28c22b09d1d05343eb129712bcf5c94dbbead83ac0a702c558a6efe2b"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/how-upstart-cut-near-10m-in-aws-cloud-costs-using-the-80-20-rule-2cc634c59324"
published_at: "2023-06-22T21:53:28+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:7fcda3d42e802a17462f0010a14868ed8b373adeb671c0eb232638ed85c0797c"
---

# How Upstart Cut Near $10M in AWS Cloud Costs Using the 80/20 Rule

Finops


AWS


Cost Savings


80 20 Rule


# How Upstart Cut Near $10M in AWS Cloud Costs Using the 80/20 Rule


[Christopher Gray](https://medium.com/@chrisjgray?source=post_page---byline--2cc634c59324---------------------------------------)


3 min read


·


Jun 22, 2023


--


At Upstart, we’ve successfully saved approximately $10 million on our[AWS cloud](https://aws.amazon.com/) costs by using[the 80/20 rule](https://www.investopedia.com/terms/1/80-20-rule.asp) to optimize our compute, database, storage, and network expenses.


The 80/20 rule, also known as the Pareto principle, is a widely recognized principle in business that suggests 80% of a given output comes from 20% of the input. In the context of cloud cost optimization, this means that a majority of the cost savings can be achieved by focusing on a few key areas that account for a large portion of the overall expenses.


Press enter or click to view image in full size


Here’s how we were able to achieve significant cost savings in those areas while maintaining the same level of performance and reliability.


**Compute Optimization**


Upstart optimized our compute costs by analyzing our fleet of Kubernetes clusters to identify under utilization of both CPU and Memory, and to perform rightsizing efforts. To accomplish this, we took a look at the workloads and appropriately resized the CPU and Memory of applications based on historical performance. Additionally, we identified that in some cases, the fleet of workloads would be better optimized on different classes of AWS Instances, moving from the C class to R class instances, which have an 8:1 Memory ratio instead of the 2:1 Memory ratio of the C class. This helped to fit more of the applications onto the EC2 instances and reduce the size of the fleet by up to 80% per cluster.


Additionally, a review was performed on EC2 instances to identify abandoned workloads that could be safely cleaned up. In the effort of identifying the machines, monitors were put in place to alert on instances with low to no utilization to keep the cloud environment free of abandoned workloads.


**Database Optimization**


Upstart optimized our database costs by identifying and moving data out of large databases and moving them to S3, and updating the application functions to pull data from S3 buckets rather than directly out of the database. Shrinking the database storage aided in reducing the IOPS, storage costs, as well as backup costs. In addition to considering our current retention and deletion process and database cleanup, we conducted a review of our backup policies and made updates to those allowed for reducing the unit costs of our database backups with automated lifecycle policies and safely purging of nonessential redundant backups without the risk of losing any essential data.


**Storage Optimization**


Upstart optimized our storage costs by using Amazon S3 for object storage and Amazon Elastic Block Storage (EBS) for block storage. With Amazon S3, Intelligent Tiering and Lifecycle Policies were put into place to archive data to greatly reduce the unit cost at scale. For the EBS Volumes, in addition to considering our current retention and deletion process, analysis was performed to find non-attached volumes that could be purged, as well as finding rightsizing opportunities for volumes that were too large or had more IOPS than were required. A migration from GP2 to GP3 volumes was utilized in multiple cases to further reduce cost while increasing performance capabilities.


**Network Optimization**


We optimized our network costs by analyzing traffic patterns and identifying places where VPC Endpoints or PrivateLink could be implemented to reduce network fees, as well as adding to our security for the private network traffic. The implementation of VPC Endpoints and PrivateLink greatly reduced the network fees going over the NAT Gateways.


**Conclusion**


Upstart’s successful implementation of the 80/20 rule demonstrates the effectiveness of this principle in achieving cost savings in cloud infrastructure. By focusing on the key areas of compute, databases, storage and network, Upstart was able to achieve significant cost savings while maintaining the same level of performance and reliability. This approach can be applied to any cloud infrastructure to achieve similar cost savings and optimize cloud expenses.
