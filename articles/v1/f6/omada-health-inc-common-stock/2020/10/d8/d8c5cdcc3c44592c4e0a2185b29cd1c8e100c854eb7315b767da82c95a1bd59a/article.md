---
schema_version: "1.0.0"
document_id: "d8c5cdcc3c44592c4e0a2185b29cd1c8e100c854eb7315b767da82c95a1bd59a"
company_key: "omada-health-inc-common-stock"
company: "Omada Health Inc."
source_id: "omada-health-inc-common-stock-rss-9e1c3b1ccfca"
canonical_url: "https://medium.com/omada-health-tech/standardization-self-service-7a72e5355706"
published_at: "2020-10-12T16:43:46+00:00"
first_seen_at: "2026-07-20T23:18:22.449098+00:00"
fetched_at: "2026-07-28T22:26:38.956502+00:00"
content_hash: "sha256:15246567c153f1e54dc0f3dc18936d1186cd89ed95ea3576fb6afafa609bb682"
---

# Standardization = Self-Service

Software Development


Database


AWS


Health Technology


# Standardization = Self-Service


[Omada Health Engineering](https://medium.com/@OmadaEng?source=post_page---byline--7a72e5355706---------------------------------------)


6 min read


·


Oct 12, 2020


--


*Written by*[Jake Smith](https://www.linkedin.com/in/jacob-smith-871812149/) *, Platform Engineer II*


**Platform & Developer Productivity Creating Self-Service for Devs**


At Omada Health, we like shipping software fast. It allows us to inspire and engage participants and work them more effectively for healthy lifestyle changes. In order to do that, we need to have a team that is well-integrated and responsive to the needs of our developers so that developers can do what they do best — develop! The platform team at Omada fills that need and is constantly looking out for how to streamline our processes and code to enable collaboration and understanding. Over the past year, we have identified several areas that we believe need improvement and this blog post, the first in a series, will focus on SNS/SQS.


**SNS/SQS**


SNS (Simple Notification Service) and SQS (Simple Queue Service) are two Amazon services that allow for services to communicate with each other by passing messages through a publish-subscribe pattern. As Omada has continued to scale up, SNS/SQS has been invaluable as a way for microservices to communicate with one another.


In the early days of our use of SNS/SQS everything was hand rolled via UI in AWS and for the most part it worked pretty well. There were not that many apps, and our topic-queue mappings were fairly straightforward. As we started to scale, everything still worked pretty — for a time — as the service itself scales well. But as we began to add new applications, it became harder and harder to get it “right” every time. When we began using terraform, we simply imported what we had and it allowed us to continue on in more or less the same way for a number of years.


Then a new spate of new applications needed to be built for a big microservices push. Our old process required team members to wait for the platform team to dedicate time. Even on the platform team itself, this knowledge was not widely available, so only one or two team members could build the resources correctly. It became clear that the process needed to scale to meet the demands of our growing business.


If waiting on a platform engineer had been the only pitfall, some documentation would have sufficed, and business as usual could continue. But the further we explored the problem, additional issues arose. First, every queue and topic was not named for a specific environment, but for whether or not it was a production queue or topic. For production queues and topics, it mapped cleanly to production, but the system broke down when various non-production environments had a generic name assigned instead of associating it with the type of environment it was meant to be used for. Second, permissioning needed to be more granularly handled without causing any unintended consequences during deployment. Additional security policies necessitated more refined queue management. Finally, every time we attempted to fix issues, it became clear we had to rethink *how* we addressed resources without inadvertently impacting other applications.


Press enter or click to view image in full size


**The solution**


The solution lay in a bit of trial and error. We had to revoke certain permission to see what broke! For the first time, we had to define how we expected applications to use the service. Here were the results:


1. Each topic had to be owned by a single application and its associated environment. This requires a well defined structure such as <APP>_<TOPIC>_<ENV> so that we can establish a clear responsibility for that resource in any given associated application
2. Each queue can subscribe to m/any topic that it wants to, but each queue is only consumed by a single application
3. All sqs information for an application + environment must be defined in a single place
4. Permissioning
- Every application has the ability to publish to APP_*_ENV, this is defined when an application user is created
- Every application is only allowed to publish to queues that are created


With those parameters in mind, we created a terraform module that would adequately fit the needs of all applications. We failed. A LOT. But we persevered — and eventually, cracked it!


Before, developers had to book time on a platform team member’s calendar with one to two weeks notice. Depending on the scope of the change, it might take multiple sessions. Now, developers can create new queues and topics that can be merged the same day with minimal feedback from a platform team member. With the conclusion of these changes, we have a streamlined process for developers to make changes and we are able to audit the path of each message with the clear permissions adding more security to our applications.


Press enter or click to view image in full size


**Databases**


Some of the first databases at Omada were on dedicated Linux hosts, if you can believe it. Back then, we had two databases: one for “The App,” and one for everything else. This worked for a number of years. When we moved our databases to RDS, we continued that model. RDS freed us from the overhead of running the system the database ran on and allowed us to better tune our databases and optimize costs. We frequently ran into two problems: the time it would take to spin up a new application — and noisy neighbors.


Like with SNS/SQS, developers frequently had to wait on the platform team members to allocate them a new database inside the host that held everything else. There were some attempts at automating this, but it was a fairly manual process, and required an ansible commit at its conclusion. The second problem: noisy neighbors proved to be a persistent problem up through the end of the standardization work. To start, seemingly innocuous code changes in one app could significantly degrade the performance of another app if the database usage changed *ever* so slightly. Along with that, we would sometimes have somewhat random outages for issues out of our control — in one memorable case, an update to Grafana caused it to continually read JSON dashboards out of the database and cause the ETL to fail. We tried two mitigations: outscaling the problem and splitting out some databases. Outscaling worked for a time, but it became clear that we would need to continually double our database spend. The second, splitting out databases, worked to some degree and generally helped with performance; however, the Grafana issue happened after the worst offenders were split out. Furthermore, after we had split out a few it became hard to track what databases were on which hosts so we started thinking that a certain application was the cause of the problem only to find out it was not in the database anymore. It eventually became clear that we needed a general case solution.


The solution was to split each database out to its own host with a robust terraform module. The first thing we had to do was to create a terraform module, and like with any other groundbreaking foundational changes we failed at first. One notable early failure was creating two parameter groups per database so that we could update the database to PG11 at some point in the future and quickly running into AWS resource limits. Eventually we had a robust module and generated enough documentation that developers could easily make a merge request for their new database. Now, we have each database independent of each other and they can grow as their apps dictate.


**Closing**


These features above are just a couple of examples of enabling our internal development teams to scale and be productive by being able to declare complex series of resources without needing to have detailed knowledge of the overall underneath systems. This way we are able to decrease ramp up time for developers, increase their velocity, significantly reduce deployment complexity and free up the Platform team to continue looking for future improvements.
