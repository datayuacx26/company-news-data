---
schema_version: "1.0.0"
document_id: "28c23f216ed55229d02915437b4a7b7c611ad37ced83da7fe11a4bbd7c728604"
company_key: "yc-pump-co"
company: "Pump.co"
source_id: "yc-pump-co-news-import-86a46b79533f"
canonical_url: "https://www.pump.co/blog/aws-ec2-pricing-update/"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-25T20:15:23.871807+00:00"
fetched_at: "2026-08-19T19:04:30.281743+00:00"
content_hash: "sha256:d4450e23a95a44aa52a65b79dfcbc64405cbeb9fa01f170af7c0ba308dec5be4"
---

# AWS EC2 Pricing Update 2025: GPU Cuts & New Instances

AWS has made one of the most notable updates to[EC2 Pricing](https://aws.amazon.com/ec2/pricing/) . In June 2025, *Amazon reduced the prices of GPU instances by 45%* . Also, Amazon introduced additional high-performance instances and made adjustments to the Reserved Instances framework. These updates are beneficial regardless of the scope of your operations, whether you are processing workload on AI, scaling a startup, or managing enterprise infrastructure.


*In this article* , I will provide all the relevant information regarding the[AWS EC2 Pricing Update 2025](https://aws.amazon.com/ec2/pricing/) . It includes GPU discounts, new instances, different regional prices, and cost optimization measures.


## **What is AWS EC2 and How Does Pricing Work?**


Before we break down the updates, let us quickly recap what[AWS EC2](https://www.pump.co/) is. EC2 offers resizable compute capacity in the cloud. Virtual servers, or instances, can be launched in minutes and scaled based on demand. A variety of workloads can be supported, which include web hosting, databases, machine learning training, and high-performance computing.


**EC2 Pricing Models:**


-


**On-Demand** : Pay per second (minimum of 60 seconds for Linux or per hour for Windows instances). There is no upfront commitment for this option, yet it is the most expensive option, running 35% higher than the reserved alternatives.


-


**Reserved Instances** : Discounts of up to 70% can be obtained here, for which you can commit for 1 or 3 years. You will pay an upfront fee plus a reduced hourly rate.


-


**Savings Plans** : Flexible discount pricing of up to 72% is obtained in exchange for a consistent usage commitment of a dollar per hour.


-


**Spot Instances** : You can manually bid on unused EC2 capacity and save up to 90%. This option is great for workloads that are flexible and fault-tolerant.


-


**Dedicated Hosts** : For compliance or licensing needs, physical servers can be reserved just for you.


## **What Changed in EC2 Pricing in June 2025**


[AWS announced significant price changes and strategic adjustments on June 1, 2025](https://aws.amazon.com/blogs/aws/announcing-up-to-45-price-reduction-for-amazon-ec2-nvidia-gpu-accelerated-instances/) . Here’s everything you need to keep in mind:


### **GPU Instance Price Cuts (Up to 45%)**


Discounts on NVIDIA GPU-accelerated instances certainly drew attention. Here’s a breakdown:


**Instance Family**


**GPU Type**


**On-Demand Discount**


**1-Year Savings Plan**


**3-Year Savings Plan**


**P4d**


A100


33%


31%


25%


**P4de**


A100


33%


31%


25%


**P5**


H100


44%


44%


45%


**P5en**


H200


25%


25%


26%


These discounts apply to[On-Demand](https://www.pump.co/) and to[Savings Plans](https://www.pump.co/) purchased. Discounts for the P5 instances, which come with NVIDIA H100 GPUs, are the greatest, *with prices cut to almost 45% for three-year commitments.*


**Regional Availability:**


-


**P4d:** Asia Pacific (Seoul), Asia Pacific (Sydney), Canada (Central), Europe (London).


-


**P4de:** US East (N. Virginia).


-


**P5:** Asia Pacific (Mumbai), Asia Pacific (Tokyo), Asia Pacific (Jakarta), South America (Sao Paulo).


-


**P5en:** Asia Pacific (Mumbai), Asia Pacific (Tokyo), Asia Pacific (Jakarta).


### **New Instance Families: M8i, C8i, R8i**


AWS has launched[new versions of its Intel Xeon Scalable instances](https://aws.amazon.com/ec2/instance-types/) with significant improvements to performance and cost:


-


**M8i/M8i-flex** : General-purpose instances for balanced workloads with 15% better price-performance than M7i instances.


-


**C8i/C8i-flex** : Compute-optimized instances for CPU-intensive data processing and high-performance web server tasks.


-


**R8i/R8i-flex** : Memory-optimized instances focused on in-memory analytics, databases, and caching needs.


All " **flex** " versions automatically adjust and scale to your workloads, ensuring optimal cost efficiency. Also, there are no manual adjustments for resources.


**Pro Tip** : If you can migrate from earlier-generation instances (e.g., M7i to M8i or C7i to C8i), this will *lower your costs by 15–20% and increase performance per dollar spent.*


### **Reserved Instance and Savings Plan Policy Updates**


AWS will limit usage of[Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/pricing/) and[Savings Plans](https://aws.amazon.com/savingsplans/compute-pricing/) for any given customer to exclude all other customers. MSPs and resellers who had previously allowed customers to access discount sharing will be most impacted by these changes.


**Important:** These changes will affect resellers, not FinOps platforms, including[Pump](https://www.pump.co/) . Your Pump account remains compliant and unaffected, so you can still monitor usage and optimize for cost with no disruption to your workflow.


**What This Means:**


-


MSPs can no longer pool RIs and SPs across different customer accounts


-


End customers must purchase their own commitments directly


-


Resellers lose the ability to offer shared discount models


If you're an enterprise, managing your own AWS accounts in the cloud will not be affected. However, if you work with an MSP, you will need to verify how these MSPs plan to adjust to the new rules.


## **Why Did AWS Cut EC2 Prices in 2025?**


As for the aggressive pricing update, several reasons come to mind:


1.


**Competition from Azure and Google Cloud** : AWS Cloud providers are unceasingly locked in a pricing war, especially with AI infrastructure. To this end, Microsoft-Azure and Google Cloud offered competitively priced GPUs, thus AWS was compelled to respond.


2.


**Rapid Growth in AI/ML Workloads** : The demand for GPU computing has shot up as companies try to train large language models and work with inference workloads. As AWS becomes cheaper, AI startups, along with companies, are more likely to use AWS services.


3.


**Hardware Efficiency Gains** : AWS uses even more efficient and cutting-edge hardware. These custom Graviton processors, as well as the new[NVIDIA H100 and H200 GPUs,](https://aws.amazon.com/ec2/instance-types/p5/) are examples of this technology. As these processors become more efficient, AWS is able to reduce computing costs for customers.


## **EC2 Pricing 2024 vs 2025 Comparison**


**Instance Type**


**2024 Price**


**2025 Price**


**Reduction**


P4d


$32.77


$22.00


33%


P4de


$40.97/hr


$27.50


33%


P5


$98.32/hr


$54.10/hr


45%


P5en


-


75% of the former cost


25%


***Note:***[Prices](https://aws.amazon.com/ec2/pricing/) *are approximate and vary by region.*


## **Smart EC2 Cost Optimization Strategies**


### **Use Savings Plans Strategically**


With the new single-customer restriction, it's more important than ever to forecast your usage accurately. Commit to[Savings Plans for predictable workloads](https://www.pump.co/) and save up to 72%.


**Pro Tip** : Start with a one-year commitment if you're unsure about long-term usage. You can always upgrade to a three-year plan once you have better data.


### **Migrate to Graviton-Based Instances**


[AWS Graviton processors](https://aws.amazon.com/ec2/graviton/) offer up to 40% better price-performance than x86 alternatives. If your workloads are compatible (many modern applications run seamlessly on ARM architecture), switching to Graviton instances can compound your savings.


### **Use Spot Instances for Flexible Workloads**


[Spot Instances can save up to 90%](https://www.pump.co/) compared to On-Demand pricing. They're ideal for:


-


Batch processing jobs.


-


CI/CD pipelines.


-


Data analysis tasks.


-


Development and testing environments.


Just make sure your applications can handle interruptions, as AWS may reclaim Spot capacity with two minutes' notice.


### **Monitor Spending with Cost Management Tools**


You get basic cost visibility using[AWS Cost Explorer](https://www.pump.co/) and[CloudWatch](https://www.pump.co/) , but advanced analytics and cost automation benefits are provided by[Pump](https://www.pump.co/) . With Pump, you receive:


-


AI-powered usage analysis and forecasting.


-


Automated reserved instances discount purchases.


-


Volume discounts are available through group purchasing.


-


30-day risk-free money-back guarantee.


### **Audit Your Instance Families Regularly**


Don't take it for granted that your current arrangement is optimal. Because new instance types are launched constantly, there are probably benefits to periodic audits, such as:


-


More advanced and efficient instances may become available for you to upgrade to.


-


Workload consolidation may help streamline your instances.


-


Zombie resources may be hiding (instances no one recalls having started).


## **Region-Specific Pricing: Does Location Matter?**


Yes. EC2 pricing varies significantly by region. According to[recent data](https://cloudprice.net/aws/regions) , **US West (Oregon) / us-west-2** is the cheapest region at an average of $0.241 per hour, while **South America (Sao Paulo) / sa-east-1** is 1.3x more expensive.


**Sample Regional Pricing**


**Region**


**Average Price per Hour (USD)**


US West (Oregon)


$0.241


US East (Virginia)


$0.241


Asia Pacific (Mumbai)


$0.247


Europe (Ireland)


$0.252


Canada (Central)


$0.256


South America (São Paulo)


$0.313


If latency is not a big concern, it's economical to deploy workloads in lower-cost regions. For example, 30% saving on compute costs is to be made by moving a batch processing job from Sao Paulo to Oregon.


Use the[AWS Pricing Calculator](https://calculator.aws/) to compare costs across regions before making infrastructural purchases.


## **Conclusion**


Having obtained information on the latest[EC2 pricing for AWS](https://aws.amazon.com/ec2/pricing/) , it is time for a recap on why you should care. The 2025 EC2 pricing updates from AWS are the most undeniable proof yet that the cloud is getting more powerful, more affordable, and, most importantly, cheaper and more accessible.


Increased discounts on GPUs of up to 45% along with changes to Savings Plans, more instance families, and much more, the potential for optimization is unprecedented.


If it’s been a while since you last optimized your EC2 setup, it is best to do it now. Fine-tuning your cost strategy will likely reduce your EC2 cost to reflect your more efficient instance mix and use of new instance families.


## **Similar Blog Posts**


-


[Amazon EC2 Pricing - Instance Costs & Savings Guide](https://www.pump.co/)


-


[How AWS EC2 Capacity Blocks Help with GPU Shortage](https://www.pump.co/)


-


[AWS Reserved Instances - Save on Cloud Compute Costs](https://www.pump.co/)


-


[EC2 Instance Types - Compare Performance & Pricing](https://www.pump.co/)
