---
schema_version: "1.0.0"
document_id: "db8f03b1e9514196b56843bf7a34a5de0a2d36c9a23b4a4b689c639f84e2d371"
company_key: "coupang-inc-class-a-common-stock"
company: "Coupang Inc."
source_id: "coupang-inc-class-a-common-stock-news-import-bcd49d0279bd"
canonical_url: "https://www.coupang.jobs/en/life-at-coupang/engineering-blog/cloud-expenditure-optimization-for-cost-efficiency/"
published_at: "2024-03-21T06:18:00+00:00"
first_seen_at: "2026-07-21T15:09:49.708343+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:f72021993e8eda645cd18699533d586335238deb27c46f4c68e8b610c670c0d2"
---

# Cloud expenditure optimization for cost efficiency

# Cloud expenditure optimization for cost efficiency


*By*[Luke Travers](https://www.linkedin.com/in/luketravers) *&*[Amit Arora](https://www.linkedin.com/in/amitarora58)


In this post, we share how the finance and engineering teams at Coupang have partnered together over the past few quarters to provide a roadmap to manage and optimize cloud expenditure. We will also detail how multiple engineering teams formed a Central team to further optimize the cloud spending for on-demand cost.


The Central team’s efforts were narrowed down to the following three principles:


- Budget allocation and its compliance
- Savings as the goal
- Focus areas of credibility, sustainability, and control in line with the company’s leadership principle of “ **Hate Waste** ”


##
Table of Contents


Background & challenges


Stage 1: Forming a central team


Stage 2: Spending less & paying less


Instance generation alignment


EMR


Storage


Conclusion


## Background & challenges


As a company we were in a classic situation where:


1. engineering teams were spending more than they needed on cloud, with little understanding of cloud efficiency.
2. finance teams were struggling to understand what teams were spending on, and how to curb expenditures without impacting business growth.
3. the leadership team did not have enough analytics into cloud spending.


To bring financial accountability to the variable spending models of cloud, the leadership team aided the engineering teams by engaging the right people to find the opportunities for efficiency and cost savings.


## Stage 1: Forming a central team


Our cloud infrastructure engineers and technical program managers collaborated as a Central team to identify a few initiatives for cloud spending efficiency.


The Central team collaborated with each domain team and helped them understand that while they are the owners of cloud usage, they must also take advantage of the cloud’s variable cost models. For instance, we helped one of our domain teams to understand their data stored in Amazon S3 and how the storage structure could be optimized at rest. Also, we shed light on how we could use tools such as AWS Spot Instances and ARM-based AWS Graviton, resulting in the dramatic cost reduction on storing and processing data. The Central team made sure that the right analytics was available, helping teams to take data-driven decisions based on the value of cloud.


The Central team understood the importance of the right analytics, tools, and processes to derive cloud efficiency as a culture across the domain teams. In that sense, we created custom dashboards using Amazon CloudWatch data processed through Amazon Athena, and we also utilized our BI (Business Intelligence) dashboards for processing AWS CUR (Cost & Usage Reports) data. The finance team also partnered with us from the other end and helped us to push forward the importance of managing the domain teams to their assigned monthly and quarterly budgets.


## Stage 2: Spending Less & Paying Less


The Central team equipped with the right analytics and tools focused on optimizing in the following two interrelated yet contrasting methods:


1. **Spending Less (Expenditure Reduction by Using Less):** Automating the launch of AWS resources on non-production environment on a need basis. This helped the company save 25% in costs on non-prod environments.
2. **Paying Less (Usage Reduction by Rightsizing):** With the right data to analyze the usage patterns, the Central team worked closely with the domain teams across the company to manually eliminate unutilized and underutilized EC2 resources.


With usage optimization and cost savings as our main goals, the following initiatives helped save millions of dollars (On-Demand cost) in 2021 on AWS Cloud.


The optimization techniques we adopted not only helped us save in costs, but also unlocked more efficient cloud resources. Based on the best practices and recommendations from AWS, we implemented the following initiatives.


## Instance generation alignment


We wanted to bring every single instance in Coupang up to the current generation for improved performance, lower cost, and higher availability. This required extensive collaboration with the domain teams to test on each and every instance type for us, as well as exploring different chip architecture such as AMD and ARM. After this arduous testing process, we successfully moved entire families of internal products onto AMD CPUs, gaining 20% in better price performance in comparison to the older versions.


##


## **EMR**


We love using Spot Instances for EMR, but as we all know, it can be difficult to get ideal capacity at peak times using Spot. With our intricate and integrated EMR systems, we had to carefully ensure that each part of our toolchain was updated without causing an interruption to our service. Therefore, we had to upgrade our software versions to take advantage of the[instance fleets feature](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-instance-fleet.html) of AWS EMRs. This upgrade helped us to get 25% cost reduction on total EMR costs.


## **Storage**


In this section, we discuss how we managed to cut our AWS storage costs for EBS and S3.


- **Amazon EBS:** For storage we found that there was extensive testing required to gain internal customer trust that the newer generation of EBS, GP3 would continue to meet our needs. To gain this trust, extensive performance testing was conducted using various tools. With all tests done in our development account first, we found that we could comfortably migrate 500–1000 live volumes at a time in parallel without any tangible impact.
- **Amazon S3:** We moved 50+PB to Intelligent-Tiering (IT). During the process, we learned the hard way that not all workloads work well with IT, and you need to be very careful with object size. If the average object size is too low and you have multiple billions of objects, you can end up drastically increasing your overall S3 costs for that workload. In that case, the usage of S3 lifecycle filters is required to tune the policy. This is not a ‘one and done’ process but an ongoing cycle that requires extensive time, care and attention to not fall afoul of S3’s complex billing patterns.


**Figure 1.** Crossing trend of increasing Amazon S3 usage versus decreasing AWS cost per size


##


## Conclusion


By adopting the methods above, we were able to minimize our AWS costs by millions of dollars (On-Demand) in 2021. A lot of the process was manually done, focusing on identifying the right optimization areas and achieving them. We are still working hard to identify additional areas for cost savings and improved efficiency.


Although we managed to save the company millions in cloud costs, we are not done yet. As next steps, we wish to invest in more complex analytic tools to drive the Cloud FinOps mindset at Coupang. Additionally, we will be automating some of the monitoring and analytics processes required for cost optimization in cloud.


*If you are a passionate engineer with a deep understanding of cloud optimization and cost efficiency,*[join our talented team](https://bit.ly/3Q7AKWS) *.*


*While the technical content in this article reflects our company’s research activities, it does not necessarily indicate our actual investment plans or product development directions. This content is purely for research purposes and should not be used as a basis for investment decisions. Please refer to our official announcements on*[ir.aboutcoupang.com](http://ir.aboutcoupang.com/) *for information on our formal investment plans and product development strategies.*


### Tags


- [Coupang Tech](https://www.coupang.jobs/en/life-at-coupang/tag/coupang_tech/#content)


## Related posts


Engineering Blog


| Tuesday, October 15, 2024


## [Accelerating Coupang’s AI Journey with LLMs](https://www.coupang.jobs/en/life-at-coupang/engineering-blog/accelerating-coupang-s-ai-journey-with-llms/)


In the last couple of years, Coupang has been using machine learning \[ML/AI\] heavily to improve customer experiences in areas like search, ads, catalog and recommendations. ML drives important decision making in pricing, transportation and logistics.


Engineering Blog


| Friday, September 8, 2023


## [Meet Coupang’s Machine Learning Platform](https://www.coupang.jobs/en/life-at-coupang/engineering-blog/meet-coupang-s-machine-learning-platform/)


Coupang strives to scale machine learning development at all ML lifecycle stages, including ad-hoc exploration, training data preparation, model development, and robust production deployment of models.


Engineering Blog


| Monday, April 17, 2023


## [Coupang Rocket Delivery’s spatial index-based delivery management system](https://www.coupang.jobs/en/life-at-coupang/engineering-blog/coupang-rocket-delivery-s-spatial-index-based-delivery-management-system/)


Coupang started a project in 2021 to provide the foundation for visualizing the delivery areas on the map, enabling direct modification to the map, and providing further metrics and statistics for optimization. Read more to learn.
