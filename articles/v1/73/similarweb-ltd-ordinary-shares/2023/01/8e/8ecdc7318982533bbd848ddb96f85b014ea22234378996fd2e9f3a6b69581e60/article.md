---
schema_version: "1.0.0"
document_id: "8ecdc7318982533bbd848ddb96f85b014ea22234378996fd2e9f3a6b69581e60"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/how-we-cut-our-databricks-costs-by-50-7c60d6b6c069"
published_at: "2023-01-09T07:01:00+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:aa22f8e92d12107c58c890ee11775da53f7c0dfc480d015411ac5fce67c9ed0a"
---

# How We Cut Our Databricks Costs by 50%

Big Data


Databricks


AWS


Cost Optimization


# How We Cut Our Databricks Costs by 50%


[Ran Sasportas](https://medium.com/@ran729?source=post_page---byline--7c60d6b6c069---------------------------------------)


6 min read


·


Jan 9, 2023


--


One of the main drivers of R&D cost is the use of cloud resources, particularly when it comes to big data processing tools like Databricks.


2.5 years ago we decided to use Databricks clusters as the compute for our Batch API *— if you are interested in what’s Databricks and how we are using it you can check out this*[blog post](https://medium.com/similarweb-engineering/how-databricks-scaled-our-batch-api-50x-8fded8d8fe53) *—* Since then it is our primary tool for generating Similarweb Data reports for our clients, today we are generating 70K reports a month for our clients.


In this post, I’ll share how we were able to reduce our monthly Databricks costs from $25,000 to just $12,500 by making a few key changes to our setup.


Below is a graph describing our Databricks and AWS costs over 5 months.


Press enter or click to view image in full size


Databricks + AWS operational costs


It’s worth mentioning that during those months the demand for our service has increased and we have served more data to more clients.


## 💰 Analyzing Costs


Before making any changes to our Databricks setup, we first needed to understand where our costs were coming from. The total is determined by both Databricks and AWS costs, let's take a look at how these two bills are calculated —


**AWS Monthly Cost** = (Number of Worker Nodes * Cost per Worker Node Hour * Active Seconds) + (Number of Driver Nodes * Cost per Driver Node Hour * Active Seconds) + (Storage Cost) + (Data Transfer Cost)


**Databricks Monthly Cost** = (Number of Nodes * DBU’s Per Node per Hour * Active Second * Price per DBU)


> *** A Databricks Unit (DBU) is **a normalized unit of processing power on the Databricks Lakehouse Platform used for measurement and pricing purposes** . *DBU pricing varies and can be found on the*[official website](https://www.databricks.com/product/aws-pricing) *.*


So as you can see, both monthly costs are derived mainly from the number of nodes and active hours, which means — if we will optimize the AWS compute costs, Databricks will go as follow.


The first step of this optimization is analyzing the cost breakdown, we used the AWS Cost Explorer to get visibility into our AWS bill.


Press enter or click to view image in full size


Our costs in the US EAST 1 region (AWS Costs explorer)


- In order to simplify the cost breakdown chart, I filtered it to show data for only 1 region out of the 2 regions we are operating on.


I'll add here a little bit of translation —


**BoxUsage** — On-demand instances


**SpotUsage** — Spot instances


**c5d.2xlarge** — the workers' node type we use


**i3.4xlarge** — driver’s node type we use


**EBS: VolumeUsage** — EC2 Storage


## The On-demand Fiasco


One thing that immediately stood out was the price of the on-demand instances, the nodes in our cluster are configured to prefer spot instances and fall back to on-demand. This was the most prominent chunk of our bill, and it was clear that we needed to optimize this if we wanted to bring our costs down.


> Spot instances are a cost-effective way to use spare capacity in the cloud, but they can be unpredictable. If the demand for spot instances exceeds the available supply, they can be terminated and replaced with on-demand instances. This can result in higher costs if it happens frequently.


We dealt with it with two courses of action:


1. Reduce the number of needed nodes, fewer nodes = fewer fallbacks.
2. Optimize the availability of spot instances.


### 🤖 1. Re-configuring the aggressive Auto-scaler


An optimizer's best friend is his monitoring tool, in Databricks’ case it's Ganglia UI.


Press enter or click to view image in full size


Press enter or click to view image in full size


Ganglia UI’s cluster’s CPU and Memory Monitoring graphs.


As you can see here in the above graphs — once the cluster gets work, the auto-scaler kicks in and up-scales the cluster as he sees fit.


Auto-scaling is a great feature that allows you to automatically add or remove worker nodes as needed to meet the demands of your workload. However, it can also be a major contributor to costs if you’re not careful.


When analyzing our cluster via the Ganglia UI it seems that the Auto-scaler has been upscaling aggressively, most times after an upscale our cluster’s CPU and memory are oversized and not utilized enough.


This cluster usually deals with in-frequent short jobs (1–2 minutes average execution time), this means that frequently when scaling up, the job would already be completed, and the new nodes would not be used.


With this analysis, we have decided to reduce the maximum number of workers that we allowed Databricks to scale up from a maximum of 500 workers to 250 workers, which made a big difference, and that's why -


- Reducing the number of active nodes — Fewer nodes = less money.
- Reducing the number of fallbacks to on-demand — fewer spot instances = fewer on-demand fallbacks.


### 🔋 2. Utilizing the power of Multi-AZ


Another course of action was using the Multi-AZ feature in Databricks. This allowed us to automatically switch to the availability zone with the most available spot instances, which helped us reduce costly fallbacks to on-demand.


### 📈 Results


Press enter or click to view image in full size


Costs Breakdown for December 2022 in US EAST 1 region


As you can see those 2 actions helped us to successfully reduce the cost of **BoxUsage: c5d** nodes by **80%** (from 2800$ to 500$ in this region), and as you can see the **SpotUsage: c5d** cost did not change significantly.


## Bonus Round


### ☠️ ️Terminating the Driver


Those of you with keen eyes might have noticed that we also managed to bring driver costs down too significantly (BoxUsage:i3.4xlarge) — around 50%.


This was due to a decision to terminate the driver after 30 minutes of inactivity (also a great Databricks feature).


This decision has an undeniable upside — when the cluster is not being used, terminate it — therefore — stop paying for it. The tradeoff is — Cluster initialization usually takes up to 3–5 minutes in our case, which means that our response time will be impacted from time to time.


### 🔀 The EBS switch·er·oo


We recognized one more opportunity for optimization.


EBS volumes are used to store data that is persisted beyond the life of an EC2 (Elastic Compute Cloud) instance, and they can be a significant contributor to costs if not adequately managed. Databricks provisions EBS volumes for every worker node to support operations like shuffles for example.


Initially, we were using gp2 volumes for our EBS storage. After some[research](https://aws.amazon.com/blogs/storage/migrate-your-amazon-ebs-volumes-from-gp2-to-gp3-and-save-up-to-20-on-costs/) , it was clear that switching to gp3 volumes is the right decision, which offers higher performance and more cost-effective pricing, and it's just a click of a button away.


Press enter or click to view image in full size


On the Databricks Admin Console Page


This little checkbox reduced the EBS costs by almost 50%.


## Bottom lines


We managed to drive down EC2 costs and usage on AWS, which impacted directly on the Databricks Costs and resulted in a monthly **12,500$** cost saving. Yearly, it's — **150,000$** , significant, right?


What should you take from here?


- **Analysis** — An optimizer’s best friend is his monitoring tool, master it.
- **Initiative** — As a Software Engineer, it is within your power to impact the price of the company’s software. Do it.
- **Patience** — Cost optimization takes time for — research, experiments, waiting for results, and then again. As you’ve seen in my case, it took 5 months of cycling, be patient!
- **Click** on the damn gp3 checkbox.


I would like to thank the brilliant Oded Fried for working with me on this.
