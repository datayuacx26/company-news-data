---
schema_version: "1.0.0"
document_id: "fba748b5b413ef7c93ed963a6ec4cb18d3edb66a9d6bbac6b3c237558551b778"
company_key: "yc-prodigal"
company: "Prodigal"
source_id: "yc-prodigal-rss-445231af9883"
canonical_url: "https://www.prodigaltech.com/blog/aws-cost-optimization-how-we-saved-30-on-aws"
published_at: "2024-06-27T16:49:53+00:00"
first_seen_at: "2026-07-25T19:50:44.288158+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:b0d237e4c70f5c40de2ded5a02839a16f9e3ff4eca06ddc98892833e805d252d"
---

# AWS cost optimization: How we saved 30% on AWS -

This is a guest post from Prodigal DevSecOps Engineer Prashant Banthia. It[originally appeared on Medium](https://prashantbanthia.medium.com/aws-cost-optimisation-how-we-saved-30-cost-on-aws-47e539f16567) - follow him there or[on LinkedIn](https://www.linkedin.com/in/prashantbanthia/) !


## How to save on AWS


In the fast-paced environment of startups, cloud costs often get overlooked in the rush to build and deploy products, leading to inflated expenses that become a burden for management. This was the case for us at Prodigal, where our cloud costs ballooned and became unsustainable for our scale.


Taking inspiration from Benjamin Franklin’s quote, “A penny saved is a penny earned,” recently, we started cloud cost optimization measures and tackled our growing AWS bill to successfully cut our monthly cloud expenses by 30%.


In this blog post, I’ll share the strategies we implemented to optimize our AWS services, which could help other companies manage their cloud infrastructure more efficiently.


S3 storage can quickly become costly if not managed properly. We used various storage classes and lifecycle policies to optimize costs.


## 1. S3 optimization: Storage classes and lifecycle policies


S3 is the cheapest and go-to storage option in AWS, but over time if not taken care it can become a dump yard with a lot of data, including critical customer data, logs and test data, etc. lying around in hundreds of S3 buckets, leaving you clueless on what to do with it.


To tackle this, first target the buckets that are highest contributors in costs and classify data stored in these S3 buckets based on for how long the data is used actively and if it can be deleted or archived.


Based on the analysis, you can utilize S3 storage classes to save costs. Amazon S3 offers various storage classes designed to cater to different storage needs and usage patterns. Each storage classes (Standard, Standard-Infrequent Access, One Zone — Infrequent Access, Intelligent Tiering, Glacier, Glacier Deep Archive) may have variations in storage costs, retrieval fees, data transfer costs, and minimum storage durations which is published on[AWS S3 Pricing](https://aws.amazon.com/s3/pricing/) .


We implemented lifecycle policies to transition objects between storage classes and eventually delete them if they were no longer needed. This ensured that we were only paying for the storage we actually used.


***Tip-*** *Before directly applying lifecycle policies on the data, calculate the lifecycle transition costs, as it is based on the number of objects and can be significantly high in case of large number of objects. In that case you can implement methods like downloading the data on an EC2, zip it as an archive file and upload it to different storage classes.*


## 2. EC2 optimization: Right-sizing instances


We analyzed our EC2 instances and identified those that were over-provisioned. By right-sizing them, we ensured that we only paid for the resources we actually needed.


Steps taken:


- Monitor usage: Used AWS Compute Optimizer for recommendations and CloudWatch metrics to monitor CPU, memory, and I/O usage.
- Right-size instances: Switched to smaller instance types where applicable.


Also consider changing the instance families if applicable, for instance if you are using t or m type instances and the memory utilization of the instance is more as compared to CPU and switch to r type instances.


If your workloads supports ARM-based CPUs, consider moving to[AWS Graviton](https://aws.amazon.com/pm/ec2-graviton/) instances as they provide better price performance, leading to significant cost savings.


If your workloads does not support ARM based CPUs then you can consider using instance types with AMD processors like r5a, t3a which are significantly cheaper than standard instance types like r5, t3 respectively.


# 3. Removing idle resources


Regular audits helped us identify and remove unused or idle resources such as AMIs and snapshots older than a year, unattached EBS volumes, and load balancers and Elastic IPs. This step alone contributed to a significant reduction in our monthly AWS spend.


# 4. Disabling inessential services


We evaluated all active services and realized some, like AWS Config and AWS Inspector, weren’t essential for our current operations and were significant part of our AWS bill. Disabling these services cut down our costs without impacting our performance or security.


Also, see if you have two Management CloudTrail running in different AWS regions. AWS provides one Management CloudTrail free per account and charges twice if you create a second one. If you are using multiple Management CloudTrails, consider keeping only one of them.


# 5. Embracing spot instances


A Spot Instance is an instance that uses spare EC2 capacity that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. The only downside is that they can be interrupted by AWS at any time.


Spot instances when used along with autoscaling groups or with EKS/ECS can help you save a lot of costs. In our case we were using EKS, and we significantly reduced cost by leveraging solutions like Karpenter and Pod Disruption Budgets to utilize spot instances without affecting availability.[Check out my blog on EKS cost savings](https://prashantbanthia.medium.com/save-costs-on-aws-eks-clusters-using-karpenter-fba08db9488b) for more details.


## 6. Reducing data transfer costs


We optimized data transfer costs by keeping data transfers within the same AWS region and using AWS CloudFront to reduce costs associated with data transfers out of AWS. Keeping high bandwidth workloads in public subnets to bypass NAT gateways can also help in some cases. To reduce inter-Az data transfers, you can also create a NAT Gateway Per Availability Zone, so that the internet data stays in same Availability Zone. Utilize VPC endpoints as much as possible as they directly talk with AWS.


Also, try to leverage VPC Endpoints as much as possible, as they bypass NAT gateway.[VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) enables creation of a private connection between VPC to supported AWS services like S3, DynamoDB, etc., and VPC endpoint services powered by PrivateLink using its private IP address. Traffic between VPC and AWS service does not leave the Amazon network.


# 7. Use reservations and savings plans


Once you have the EC2 instance optimized, do capacity planning on the capacity you need to run the workloads, based on the assessment procure savings plan and reservations.


Reserved Instances provide you with significant savings on your Amazon EC2 costs compared to On-Demand Instance pricing. Reserved Instances are not physical instances, but rather a billing discount applied to the use of On-Demand Instances in your account when you commit the capacity of a certain instance to AWS for 1 or 3 year period.


Savings Plans offer significant savings over On-Demand Instances, just like EC2 Reserved Instances, in exchange for a commitment to use a specific amount of compute power (measured in $/hour) for a 1 or 3 year period.


AWS recommends Savings Plans over Reserved Instances as with Reserved Instances, you make a commitment to a specific instance configuration, whereas with Savings Plans, you have the flexibility to use the instance configurations that best meet your needs


Summary table — RIs vs Savings Plans


# 8. AWS cloudwatch optimization


Amazon CloudWatch is an essential tool for monitoring and managing your AWS resources and applications. However, its costs can quickly add up if not managed properly. Here’s a breakdown of the different CloudWatch costs and some strategies to optimize and save on these expenses.


CloudWatch charges are primarily based on three factors:


i. Data ingestion


ii. Storage


iii. Queries


## i. Data ingestion costs


Data ingestion costs are incurred when you send custom metrics, logs, or events to CloudWatch. The pricing for data ingestion is as follows:


- Custom metrics: $0.30 per metric per month.
- Logs ingestion: $0.50 per GB ingested.


To save on data ingestion costs:


- Reduce unnecessary metrics: Only monitor essential metrics. Avoid sending redundant or low-value metrics.
- Optimize log levels: Adjust log levels to minimize the volume of log data. For example, use ERROR level instead of DEBUG in production environments.
- Batch data: Where possible, batch data before sending it to CloudWatch to reduce the number of API calls.


## ii. Storage costs


Storage costs are incurred for retaining your logs and metrics in CloudWatch. The pricing is:


- Logs storage: $0.03 per GB per month.
- Metrics storage: $0.05 per 1,000 metrics per month.


To save on storage costs:


- Implement retention policies: Configure retention policies to automatically delete old logs and metrics that are no longer needed.
- CloudWatch IA: Use recently announced CloudWatch IA storage class to store the logs that are not frequently accessed to save storage costs.
- Archive data: Move infrequently accessed log data to Amazon S3 or Glacier for cheaper long-term storage.


## iii. Query costs


Query costs are incurred when you query your metrics and logs in CloudWatch. The pricing is:


- Logs insights queries: $0.005 per GB of data scanned.
- GetMetricData API calls: First 1 million API calls per month are free, then $0.01 per 1,000 requests.


To save on query costs:


- Optimize queries: Write efficient queries to minimize the amount of data scanned. Use filters and aggregation to reduce data volume.
- Schedule queries: Schedule queries during off-peak hours to take advantage of lower costs, if applicable.
- Use dashboards wisely: Limit the number of real-time dashboards that query data frequently.


*If you are seeing high query costs, the possibility is that there is a log group with large amount of data and a lot of queries are being run on the log group. To mitigate this, try to reduce the retention period of log group, and if possible, try to break the large log group in smaller ones to reduce the size and hence the query costs.*


Search the cost explorer and group Cloudwatch costs by API operation to identify which factor is attributing to the costs.


## Conclusion


Optimizing AWS costs requires a continuous and strategic approach. By understanding your current spending, rightsizing resources, leveraging cost-saving plans, and regularly reviewing your infrastructure, you can significantly reduce your AWS expenses without compromising on performance or scalability.


Effective AWS cost management not only helps in reducing expenses but also in reinvesting savings into other business areas. Stay proactive, use the tools and services AWS provides, and make cost optimization a fundamental part of your cloud strategy. By doing so, you’ll ensure your organization gets the maximum value from its AWS investment.


‍
