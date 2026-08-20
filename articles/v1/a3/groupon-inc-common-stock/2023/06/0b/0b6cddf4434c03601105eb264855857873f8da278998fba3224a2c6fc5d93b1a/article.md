---
schema_version: "1.0.0"
document_id: "0b6cddf4434c03601105eb264855857873f8da278998fba3224a2c6fc5d93b1a"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/cloud-cost-mindset-cc4d0a3f3a8"
published_at: "2023-06-16T20:11:47+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:cb617fce9314878a995e4230d94893685b4908c1a37c97e706f0d3639f84198b"
---

# Cloud Cost Mindset

Cloud Cost Management


# Cloud Cost Mindset


[Michael](https://medium.com/@mcarreno_23858?source=post_page---byline--cc4d0a3f3a8---------------------------------------)


9 min read


·


Jun 16, 2023


--


Groupon completed its migration to the cloud in February 2023. Shortly thereafter our data centers were completely shut down and the equipment cleaned up. The move to the cloud has brought many advantages to Groupon engineering. Our infrastructure can now dynamically scale with our traffic throughout the year and engineering resources can be easily started up or turned off as our needs change.


The move to the cloud has also brought a wealth of information regarding our infrastructure costs and utilizations. Groupon operates in both Amazon AWS and Google GCP. Our main consumer and merchant applications reside in AWS while our backend data warehouse and reporting systems are in GCP. Both clouds provide extensive data on service costs including granular usage information. As more Groupon systems migrated to the cloud it became clear there were many opportunities for making our infrastructure and systems more efficient. To do this though we wanted to ensure we were tracking the costs appropriately and making the best decisions with our infrastructure and systems.


### **Cost Tracking**


The first part of this journey involved improving our cost tracking. While the cloud provides extensive cost data it’s important to match this data with individual systems to get a more complete picture. Understanding that we’re spending a lot on databases is useful but being able to map that to particular systems and teams gives us more information on the course of actions we can take.


Tags are an oft recommended approach to tracking service costs and Groupon is no exception. Groupon applied “service” tags across all our cloud resources. This allows us to map cloud resources to individual services and with it the individual engineering teams. So an RDS database may have the tag service=payments to map that to our Orders and Payments team while an S3 bucket may have service=images to map it to our global image management team. This now allows us to see not only how much we spend overall on databases, S3 buckets and other cloud resources but also view this information on an individual service and team level.


It’s also important to set parameters and conventions when tagging. Part of our cost tracking efforts involved cleaning up existing tags that had ad hoc values. The value of tagging can be diminished as values differentiate or expand. An engineering team may have their resources tagged with service=deal-management. But if there are other resources tagged with service=deal_data or service=”Deals Galore” these could get missed in the team’s cost and resources tracking reports. Similar to how engineering teams will form coding standards and conventions the same can apply to tagging.


Groupon also considered additional tags but we realized data could be gathered by other means and the maintenance overhead may be too costly. For example we’ve heard suggestions to add “environment” and “owner” tags. In our cloud layout we have environments separated by accounts (AWS) and projects (GCP). Adding “environment” tags wouldn’t yield any data as we can already differentiate environments by the cloud layout. “Owner” has been suggested so we know who to contact for a system. Groupon maintains this mapping separately. As long as we know the service we can easily get to the team email address or engineering manager responsible for it. Having to maintain owner tags could get cumbersome too as people change roles. There is some data that can be useful to cost management that doesn’t necessarily have to be part of your cloud tagging setup. It’s important to understand the various dimensions you want to report on but at the same time these do not need to be driven solely by tagging. Cost reporting tools combined with other data sources can yield the information you need and at times for less maintenance overhead.


The final step in the cost tracking equation was to provide teams with the necessary reports for viewing their costs. Both AWS and GCP provide cost reporting tools. Engineers were actively using these tools which was great. In Groupon’s case though the situation was complicated in that we operate across two cloud providers. There are services that span the cloud providers.


To help with this issue Groupon uses[Cloudability](https://www.apptio.com/products/cloudability/) to report on our cloud costs. This allows us to combine costs across AWS and GCP into consolidated reports. Additionally, our Financial Operations team developed a set of common reports that could be shared across teams, services and even cloud providers. Teams can now go into Cloudablity and select the “Service Level Report” and then select the service they would like to report on. The latter is being driven by the service tags mentioned above. This results in a consistent reporting structure across teams and services. It also means we can ensure all engineers have access to key cost metrics when looking at their cost reports and optimizing their systems.


Lessons:


- Tag cloud resources to give teams visibility into their particular systems.
- Establish conventions for tags upfront to increase their effectiveness.
- Not all reporting dimensions require tags. Information at times can be gathered by other means which may help with reporting and maintenance.
- Have a consistent means for reporting costs across teams.


### **Optimization**


As our cost tracking improved, Groupon set out to lower our costs and improve our overall efficiency. At the start there were large swaths of resources where cost cutting could be had. Compute resources that were no longer used were running in development environments. Data that was no longer needed was sitting around in storage buckets. In one case we found logs dating back 7 years. It was excessive data that we didn’t need but were still paying for. In these cases the decision to shutdown resources and trim back data was easy and lowered our costs.


One of the common themes when looking at our initial cost cutting wins was that a lack of tooling or automation had led the situation to develop in the first place. Logs had built up over years as no mechanism had been put in place to prune the data. Compute resources may have remained unused long after the last task finished. Our cleanup efforts were yielding good results but we also didn’t want to have to continually repeat this process.


The answer here was to better utilize the automation and tooling provided by both AWS and GCP. In the case of our data in S3 and GCS, we have applied lifecycle policies across all buckets. There is no case where our data will grow unbounded. New buckets will get a default lifecycle policy until an engineering or business reason determines that a different policy is needed. But there will always be a data lifecycle policy applied.


Our compute clusters are also better utilizing tooling provided by the cloud providers. In the case of GCP, we have better configured the idle shutdown and scaling parameters. Overall compute is more efficient now and actively shutdown when no longer used. This has helped lower our costs.


This didn’t require new systems or tooling on Groupon’s part but simply better utilizing the tools already provided by AWS and GCP. It has allowed us to better optimize our cloud costs and have the tooling and automation in place to ensure those costs remain optimized in the future.


A more in depth example of our cost cutting efforts can be had with Groupon’s experience with our user deal attribute data. This service will store user events such as deal views, impressions and purchases. This information is stored at a granular hourly, daily and monthly level. Cassandra is used for the primary data store.


When the service was on-prem in data centers, the team would focus on low-latency responses and high throughput. The underlying infrastructure costs were there but were not a focus. This may have been a case of missing the costs as we weren’t appropriately monitoring and reporting the cost data.


After the system was migrated to AWS our cost data and overall understanding of the system improved. We gained a much better understanding of the costs associated with read/write throughput, storage, data TTLs, data transfer and other aspects. These costs didn’t magically appear in the cloud but became more visible and better understood.


The team used the AWS Cost Explorer tool to breakdown usage type costs and information from AWS CloudWatch to better understand the cost breakdown and improvements that could be made. The service was significantly spending on write and read operations. This led the team to optimize and flatten the data structure. Where previously there may have been 12 or 15 events per deal view the action was reduced to generating 1 event. The team also determined that strong consistency is not needed for data reads. AWS Keyspaces (Cassandra) was configured to use LOCAL_ONE consistency which helped cut back on our read capacity units. Finally, TTLs and data retention were improved to better prune data when it was no longer needed.


Better cost reporting and data has helped lead the team to better understand the system overall. Aspects that may have been glossed over on-prem gained more visibility when data became available. In this case, by optimizing the cost components of AWS Keyspaces and other parts of the service we were able to reduce the costs by 96%. This results in not just lower costs but a better functioning service.


Similar gains were had in other areas too. A good portion of our data warehouse processing was originally setup in GCP Dataproc using N1 machine types. We realized that a lot of our jobs did not necessitate the large instances, SSD and even GPU. By moving these processing jobs from N1 to E2 machine types we were able to obtain a 30% reduction in memory and compute costs.


As engineering teams were cutting costs and optimizing systems numerous improved configurations and infrastructure setups were devised. Engineers would put together the new automation and infrastructure definitions such as Terraform modules. To get the full benefit of these improvements though it’s important to share them widely across the organization. For example Groupon has various teams running GCP Dataproc clusters. Sharing best practices, optimal configurations, Terraform modules and other aspects has led to rapid cost cutting and ensured all teams are gaining the benefits.


Finally, as we progressed through the cost cutting and optimization we continually stressed that the service costs ultimately resided with the engineer team owning the system. Cost cutting and optimization isn’t a one-time effort but something that needs to become part of the engineering culture. It’s important to provide teams with the resources and tools they need and allow them to own the cost management long-term.


Lessons:


- Make use of cloud automation tools to help manage cloud costs.
- Share widely through the organization techniques and means for reducing and optimizing costs.
- Ultimately the responsibility for effectiveness cost management needs to reside with the teams owning the systems.


### **Beyond Engineering**


As Groupon engineering worked to reduce costs and optimize our infrastructure we also realized that more teams would need to be involved. Optimizing cloud costs go beyond the systems and can include aspects such as reserved instances, savings plans and enterprise contract conditions. Plus the systems themselves represent features and data for consumers, merchants and internal customers such as business analysts. Including these teams on the overall effort can help yield further gains.


Early in the process we did a review of our AWS Reserved Instances for RDS. This review yielded a few issues. In many cases we had RDS databases that were not being fully utilized. This could be exhibited through low storage, memory or CPU usage. At the same time our Financial Operations team had purchased Reserved Instances for the RDS databases to help cut costs. While the effort to reduce costs was welcomed we needed to work together to truly reach our savings potential.


In this case our infrastructure teams did a thorough review of all our RDS databases and right-sized them to the appropriate instance sizes. This led to numerous databases being scaled down with corresponding cost savings. Once we were confident that the databases were sized appropriately, engineering and financial operations then collaborated on the Reserved Instances to purchase. We were able to achieve significant savings by having engineering and financial operations work through the right-sizing and reserved instances planning together.


Similar results were achieved when working with other teams in Groupon. At one point our data warehouse and infrastructure teams were doing a review of our Kafka topics and event processing. This was a case where the Kafka clusters and event processing were appropriately sized and operating efficiently. When we included the business analysts and reporting teams in the process though we realized some of the reporting events were deprecated or no longer used. This led to the business analyst and engineering teams reviewing the various event streams and ultimately stopping some of them. With this the engineering team was able to scale down our Kafka setup and save costs.


It’s important that teams are continually reviewing their system costs not just from the perspective of the service themselves but also the end users, whether external or internal. This can help give a full understanding of the system benefits and potentially where additional savings may be achieved.


Lessons:


- Effective cost management and optimization should include teams beyond engineering.
- Cost optimization can include factors beyond the infrastructure and systems themselves.


### **Summary**


The move out of data centers and into the cloud has already yielded benefits to Groupon. At first it can be rough as more information about system costs and operations is obtained. Areas of waste and inefficiencies may come to the fore but ultimately this is beneficial in obtaining a complete picture of your systems and costs. Groupon has been able to significantly cut down infrastructure, system and operational costs through improved cost tracking and optimization efforts.


Groupon is now in a better state with our infrastructure and service costs. We’ve significantly reduced our engineering spending and at the same time improved the efficiency of a lot of our services. Finally, this isn’t an exercise we’re finished with but a mindset we’ve added to our culture to put us in a better position for the long term.
