---
schema_version: "1.0.0"
document_id: "240ad310a64bd136fcea92036c7cd1d5d47cb5122db58387362787831036f509"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/aws-spot-management-xosphere"
published_at: "2020-07-27T19:00:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:a816f373532b952d6e1a61e338cc68213ca88123fea886d3f1ff060693f792ec"
---

# An easy button for AWS Spot Management? A conversation with Taloflow’s new partner, Xosphere

## About the Partnership


At Taloflow, we’re proud to introduce to you our newest partner,[Xosphere](https://www.xosphere.io/) . The Xosphere™ Instance Orchestrator™ intelligently manages AWS Spot instances to give your applications 100% reliability at a fraction of the EC2 cost. **Customers of Taloflow can use Xosphere in conjunction with Taloflow’s services to seamlessly identify candidates for Spot Management and monitor improvements in EC2 efficiency.** Below is our conversation with Xosphere’s CEO, Alan Hand.


You can read more about this product in the[Taloflow Spot Management features page](https://www.taloflow.ai/cloud-cost-management/features) .


## Interview with Alan Hand, CEO of Xosphere


**LV Jadavji (Taloflow CEO):** Hi Alan, in a few words, what does Xosphere do?


**Alan Hand (Xosphere CEO):** We developed Xosphere to ensure that applications are always running in the right place at the right time producing unparalleled savings and availability. Our cloud orchestration engine optimizes the use of AWS Spot instances transforming them into robust resources that can deliver mission critical and highly available applications at a fraction of the cost. Xosphere effortlessly moves applications to cost-effective Spot instances whenever they are available. It also ensures reliability by seamlessly moving those applications back to On-Demand or Reserved instances whenever a Spot instance is reclaimed by AWS. The end result is typically between 40-80% saved on AWS EC2.


Alan Hand, Xosphere's CEO


### Working with AWS Spot instances


**LV:** What is Spot on AWS?


**Alan:** AWS has the largest global infrastructure footprint of any cloud provider, and they continuously add new capacity to stay ahead of future demand. This results in a significant amount of unused EC2 resources that Amazon markets as Spot. Amazon enables customers to take advantage of this unused capacity at a substantial discount of up to 90% compared to On-Demand. The one caveat when using Spot instances is that at any point Amazon can reclaim and terminate the instances following a 2-minute warning message. Due to possible disruption people believe Spot instances are unreliable and only suitable for certain workloads, such as machine learning or big data analytics. However, Xosphere enables customers to use Spot safely and easily for a majority of applications, including mission critical production services.


**LV:** What kind of setup makes a company a good candidate for using Spot on AWS?


**Alan:** While Xosphere works with the majority of applications, we suggest starting with what we call the “ABC’s of Xosphere” - Auto Scaling, Beanstalk, and Containers. These offer an ideal fit for successful first deployments of Spot instances with Xosphere and are “no-brainer”.


**Auto Scaling groups** are typically stateless, and are already aware of the potential for scale-in and scale-out events. Because of this, no additional changes are required and Xosphere will work out-of-the-box. When On-Demand instances are replaced with Spot instances, applications will behave just like a scale-out operation followed by a scale-in operation. Immediately following installation Xosphere delivers instant return on investment (ROI).


**Beanstalk** leverages Auto Scaling groups under the hood, so all the features and benefits of using Xosphere with Auto Scaling groups also apply.


**Container environments** are another perfect fit for Xosphere. We are fully integrated with Kubernetes, Amazon Elastic Container Service (ECS) and Amazon Elastic Container Service for Kubernetes (EKS). You can deploy Xosphere in turnkey fashion and will realize efficiency and immediate savings.


One exciting addition is Xogroups, which supports Spot usage beyond the ABCs to include stateful applications. Ideal candidates for Xogroups include stateful applications that are resilient and do not use ephemeral storage or sticky load balancer sessions. Whether it’s a NoSQL datastore such as Cassandra or Elasticsearch, a message bus such as Kafka, or even a legacy homegrown monolith, Xogroups makes it easy to use Spot instances for stateful applications.


**LV:** What are the challenges of using AWS Spot?


**Alan:** Some clients are simply not aware of Spot or best practices for use. Others may use Spot on a very limited basis for projects like QA, development and testing, while favoring On-Demand for a majority of their usage due to Spot’s potential volatility. Still, others may understand the full potential of Spot but simply do not have the time nor the resources for proper implementation. In summary, the key challenges to Spot usage include awareness and strategies on how to effectively maximize its use.


### The "easy button" for Spot


**LV:** How does Xosphere solve these challenges? What makes it the “easy button” for AWS Spot?


**Alan:** Spot awareness and understanding is essential. We start by educating clients about Spot, including its advantages, challenges, and how to effectively and reliably leverage Spot to reduce EC2 expenses. From a technical perspective Spot instances are virtually identical to On-Demand instances, and in time will be added to the On-Demand instance pool. So, the differentiation is really the result of a business event - Spot reclaiming, rather than differences in the technology. The financial advantages of Spot are significant, so our next goal is to prove that Spot can be both economical and highly reliable. We accomplish this by demonstrating how Xosphere orchestration moves applications between Spot and On-Demand to maintain optimal Spot usage and savings. When clients see how simple it is to give an Auto Scaling group a Xosphere tag and watch it work, they get very excited. And for those companies with stateful apps, we demonstrate Xogroups and its ability to support stateful application usage of Spot.


**LV:** What advantages are there to working with Xosphere, over other tools, like Spot.io (formerly Spotinst)?


**Alan:** In today’s world data security is paramount, so we developed Xosphere to be highly secure while avoiding the vulnerabilities plaguing traditional SaaS providers. The secure design model also contributes to our superior reliability and ease of use, amongst other advantages. Allow me to elaborate...


1. **Security** is perhaps the topmost reason why customers choose Xosphere over other options. The orchestrator functions autonomously of and without dependencies on any 3rd party cloud infrastructure typically required by SaaS-based services. Because the orchestration occurs within the customers cloud environment, we eliminate the need for added network exposure and its inherent increased security risk.
‍
2. **Reliability** is the second most reason why customers choose Xosphere over other options. The orchestrator operates within the customer’s AWS environment. This comes with the same high standards of availability, reliability, performance and security of the AWS infrastructure running the customer’s applications. Xosphere does not mandate the use of a proprietary platform nor do we replace customer Auto Scaling groups with proprietary services. This eliminates the added complexity and interdependency of 3rd party technology and the additional points of failure.
‍
3. **Ease of Use.** Xosphere customers continue working with the management tools and procedures they already use. Some competitive options require replacing those tools and processes with proprietary technology. As a result, there is no learning curve or additional requirement to adapt to new tools and methodologies with Xosphere. Simply install, tag auto scaling groups or instances, and Xosphere automation does the rest.
‍
4. **Use More Spot.** In many cases, Xosphere customers will run 100% of their applications on Spot. They get the confidence of knowing that Xosphere's automation will seamlessly failover from Spot to On-Demand in response to AWS reclaiming notification. This opens the potential to use Spot for mission critical production applications that are normally reserved for On-Demand.
‍
5. **Operational Awareness.** Xosphere is operationally aware of the entire customer AWS ecosystem. For example, prior to moving applications from On-Demand to Spot, Xosphere performs numerous checks and verifications. This includes determining if application movement would result in underutilization of Reserved Instances for optimal performance.
‍
6. **Core Competency.** Xosphere is focused on providing the optimal solution to move applications to the right place at the right time producing unparalleled cost savings and reliability.


Our newest customer truly said it best:


> ‍ *“You folks have built a great product. All that’s required is to set it and forget it. Meanwhile, the savings flood in.”*


## Partnering with Taloflow


**LV:** Why did you decide to work with Taloflow? Why does the relationship make sense to you?


**Alan:** Taloflow has an amazing product that raises the bar in the cloud cost monitoring market. The anomaly detection and predictive capabilities are quite impressive and are well aligned with our Spot orchestration capabilities. In addition to product alignment it was very clear when we first met that Taloflow embraces a similar ideology as Xosphere. We have both built great products that are easy to use and truly help our customers. This is clearly a case where the whole is greater than the sum of its parts.


**LV:** How can Taloflow's monitoring and Xosphere's Spot management combine to save companies money on AWS?


**Alan:** Taloflow and Xosphere combine to form an extremely comprehensive cloud cost optimization strategy that companies can easily deploy to maximize savings, faster.


**LV:** How tightly does Taloflow integrate with Xosphere today, and what does the future look like?


**Alan:** Taloflow and Xosphere are naturally aligned from the get go. Taloflow’s predictions and recommendations can be used to identify opportunities to leverage Xosphere’s Spot orchestration. Savings produced by Xosphere will automatically be reported and visualized in the Taloflow dashboard. I am looking forward to the continued collaboration between our product lines to further streamline the experience including the possibility for autonomous operation.


### Xosphere's backstory


**LV:** Who is behind Xosphere?


**Alan:** Xosphere is powered by a world-class team of software professionals that have proven track records of bringing high value products to market. We are also grateful and privileged to be supported by a remarkable team of investors including[Idealab](https://www.idealab.com/) and[eLab Ventures](https://elabvc.com/) .


**LV:** What inspired you to pursue building an AWS Spot Management solution?


**Alan:** Prior to founding Xosphere, I was a VP of Engineering. I faced a lot of the same challenges that our customers face on a daily basis. At the time, we had a strong focus on cloud cost optimization and were able to successfully reduce our costs by a significant percentage. Despite our efforts and success in this area, we knew we could do more. While taking a closer look at Spot instances and realizing their savings potential, we discovered that using Spot instances required a lot of time and resources. Likewise, we learned that Spot instances were limited in the scope of use cases that they could address. We knew we would make Spot incredibly easy to use as well as drastically expand the number of use cases where Spot instances are viable.


‍
