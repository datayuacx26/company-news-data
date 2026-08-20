---
schema_version: "1.0.0"
document_id: "efee2dad9a002b3ca4d4073acf7bec17e7c37a239066a05bd3af34f013083aa3"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/aws-glue-to-ecs"
published_at: "2020-06-10T23:00:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:a329ab48ae2cd5a5f3b2046601a3dbde96dc946182efb106807c651eff964640"
---

# Moving AWS Glue jobs to ECS on AWS Fargate led to 60% net savings

Last month, our team published a blog post titled[How we reduced the AWS costs of our streaming data pipeline by 67%](https://news.ycombinator.com/item?id=23331989) , which[went viral on HackerNews](https://news.ycombinator.com/item?id=23331989) (Top 5). Clearly, developers are hungry to learn about new AWS cost-saving strategies.


We’ve had a lot of questions about AWS cost optimization stemming from the original post. However, this question from Carl at[Klarna](https://www.klarna.com/) inspired us to write another post:


> *Hi, Louis, Can't you write another blog article on how you moved Glue jobs to ECS? Would be interesting to read since we are heavy glue users at Klarna.*


So without further delay, here’s how we approached the problem and made the switch.


## For us, what was AWS Glue *supposed* to be good for?


For some quick context, we provide an[AWS cost optimization solution](https://www.taloflow.ai/) that monitors and alerts on cost anomalies and wasted spend in real-time.


Initially, to justify the cost of[AWS Glue](https://www.amazonaws.cn/en/glue/) , these are the primary benefits we had in mind.


Benefit Description


Scalability In our case, this meant running many clients at once rather than running very large datasets.


Low Management A minimum amount of dev time spent maintaining, backing up, etc.


Familiar Tooling Ability to use familiar tools like Spark which in our case meant minimizing the amount of time coding and testing by using something we knew rather than something we would have to learn.


Integration Integration with other AWS services that we use such as Athena which in our case was how we did a lot of our ad hoc analysis and had started to work its way into our production flow as well.


## In practice, the cost-benefit of AWS Glue we envisioned was muddy


In typical AWS fashion, several issues led to us spending multiples more than expected. All while getting less out of AWS Glue than expected. Considering the work the pipeline was doing, here are some of the reasons why we decided it wasn’t worth the cost:


### Accidentally leaving things on


Testing required running the testing gateways and endpoints. It was easier with integrated[Sagemaker](https://aws.amazon.com/sagemaker/) notebooks. Both were expensive and accidentally left on all the time by our developers. 😴


### More limited than Spark


[Spark](https://spark.apache.org/) was in some sense “bastardized”. There were things that we couldn’t do in AWS Glue. We were wasting a lot of time working around some of the inherent limitations that were causing high costs. For example, while overriding some of the parquet implementations was easy in Spark, this proved difficult in AWS Glue.


### Crawling was sneakily expensive and lagged


Crawling was deceptively expensive. We had to access the data frequently, so crawling seemed almost constant. It also took much longer to crawl our directories than we anticipated. This was in part because of the large number of files being created ([parquet issues](https://www.taloflow.ai/blog/reducing-aws-costs#6) ). We made partitioning decisions that were very complicated vis-a-vis our AWS costs.


**There often was a large lag between requesting the start of a running job and the job launching.** Our event mechanism made it difficult to have enough information from events to manage a business process (e.g.: “crawler started”). We sometimes overran our jobs because we could not tell for which clients they were running for.


### We’re a Flink shop 🐿️


We were also running a[Flink](https://flink.apache.org/) pipeline. This had to do mostly with our history. In the early days of Taloflow, we had set up Flink for our streaming data,[originally on Google Cloud Platform](https://console.cloud.google.com/marketplace/details/google/flink-operator) . When we started moving some items over to AWS we started with an AWS Glue pipeline. It frankly seemed easier at the time with our limited resources. We succumbed to the siren song of AWS’s, “let us manage everything for you!”.


I am sure many of the above could have been solved if we continued to pound away at our learning curve for AWS Glue. However, we began to wonder what the point was in going through that exercise. We asked ourselves:


1. Did we make the wrong choice of deployment?
2. Can we redeploy in a more cost-efficient manner?


### It didn't fit our "total cost" framework


Spark or Glue experts may say that if we tweaked and optimized the pipelines in Glue that we would see dramatic savings. Yes, but the point is that we reviewed things within a "total cost" framework. This includes a realistic look at the available skill-sets in our dev team. We saw that we didn't have the bandwidth to carry specialists. We also didn't want to hire an outside specialist given the importance of the pipeline to our core business.


## How we moved from AWS Glue to Fargate on ECS in 5 Steps


### 1. Identifying the limitations of our processes


We reviewed the actual amount of memory that the jobs were taking while running AWS Glue and did some calculations on our data flow. The maximum[Fargate instance allows for 30GB of memory](https://aws.amazon.com/fargate/pricing/) . We knew that if we were going to move to[AWS Fargate](https://aws.amazon.com/fargate/) , we had to fit within this.


AWS Fargate instance Max Memory (GB) configuration by vCPU


We reviewed how long it took to run the job, restricting the amount of parallelism to 4 CPUs, which is the maximum amount of CPU available in AWS Fargate. These were jobs that tend to run on a schedule at regular intervals throughout the day rather than on-demand. The throughput time was not a critical factor for us as long as it was within our Quality of Service (QoS) goal. We established a guideline time to run our largest customer and used that as an outside estimate and test for our QoS.


### 2. Making use of an existing pipeline


We knew that we were not going to try to run Spark on Fargate. Has anyone tried?


We decided that some of the work would move to a Flink pipeline that we were already running. It had some unused cycles and was recently set to scale up and down with AWS Spot instances. Moving these pipelines was straightforward. We converted from Spark to Flink using essentially the same flow and logic.


There was a second set of pipelines that were more computationally challenging and had been a struggle in Spark. We decided to move them to Python and Pandas using various parallelization techniques. We did this because we felt that even though the Python runs a bit slow, it was fast enough for us. We had a lot of Python in our data science loop. We had a lot of resources to draw from to help with the port so that we could get it done fast.


There were *lots of other good choices* for the pipeline. It depends on the use case and organizational knowledge.


### 3. Rewriting with size in mind


As we ported over we reworked the pipelines to be size sensitive. For example, the Spark pipeline would work on the entire dataset. This included[all AWS service codes](https://www.techradar.com/news/aws) (monitored by our AWS cost management service) at once for a particular client.


We added an inner loop so that the Python works on one AWS service code (e.g.: Amazon EC2) at a time. We made sure that memory gets cleared between the service codes. **With some minor rewrites, we were able to dramatically reduce memory consumption. We fit on the 30GB Fargate instance!** It did take several iterations to be 100% certain that all clients would safely fit the instance.


### 4. Tweaking the alerting and monitoring


One of the benefits that we picked up in moving is we ended up with a much better alerting and monitoring flow. We can alert at a much finer level and we are no longer restricted to the AWS Glue error codes and eventing mechanism. A lot of the AWS Glue logging moved to[CloudWatch logging](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) and got picked up by our other systems. We were having issues in the past using the AWS Glue logs.


### 5. Writing the flow to launch AWS Fargate instances


For the most part, we launch AWS Fargate instances from Lambda scripts triggered by SQS queues and CloudWatch events. We recently began triggering Fargate from an SDK within a[Business Process Management and Notation (BPMN) process server](https://kogito.kie.org/) that is running as well.


As an interim step, we wrote a simple “caching-launching” service that makes sure that our pipeline steps are launched and monitored using a combination of the following:


- CloudWatch Event Bus;
- SQS messages thrown by the applications themselves;
- and, timed “check-in” queries to the databases.


We will move a bunch of this logic to the BPMN processes over time for visibility and easier maintenance.


We did write a simple utility that cleans up garbage AWS Fargate instances. For whatever reason, some failed or were caught between deploys. Luckily, our service alerted us the first time this happened. **We were running a few hundred zombie Fargate instances and we were able to address it quickly. 🧟**


Taloflow AmazonECS Anomaly Alert


## We now run at 1/3rd of the cost


Refactoring and running our jobs as Amazon ECS tasks on AWS Fargate **cost us 1/3 (or 60% less) than what the AWS Glue runs had cost.**


Fargate (ECS) vs. AWSGlue Actuals in Taloflow's Grafana UI


Thank you for the great question that inspired this post, Carl. We invite anyone to email us with any questions - it might inspire our next post!:[\[email protected\]](https://www.taloflow.ai/cdn-cgi/l/email-protection#204c4f5549536054414c4f464c4f570e4149)
