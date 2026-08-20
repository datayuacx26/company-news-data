---
schema_version: "1.0.0"
document_id: "b5d14cf56a5294eac014b663a3b4108cfe5577fa6865105b593d465a205a52d8"
company_key: "commvault-systems-inc-common-stock"
company: "Commvault Systems Inc."
source_id: "commvault-systems-inc-common-stock-news-import-d7ff9e033aa3"
canonical_url: "https://www.commvault.com/blogs/stop-arguing-about-ease-of-use-start-measuring-it"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T15:08:37.191387+00:00"
fetched_at: "2026-08-17T15:08:39.050279+00:00"
content_hash: "sha256:9ba586dbc791c2847e98f63f73d2f0b0076d4518c1a31bc3c1e897c677e15455"
---

# Stop Arguing About Ease of Use. Start Measuring It.

### Key Takeaways


- Replace subjective claims about “ease of use” with a measurable data protection gearing ratio: protected capacity divided by the number of full-time administrators.
- Measuring protected capacity per FTE provides a more meaningful view of operational efficiency than legacy metrics such as backup jobs per administrator.
- The data protection gearing ratio should be used as a baseline before a platform migration and measured again afterward to validate operational improvements.
- Factors such as multi-cloud environments, cyber recovery requirements, and compliance obligations can influence the ratio, so it should be evaluated within the context of each environment.
- Organizations should ask vendors to commit to measurable operational outcomes instead of relying on qualitative claims about simplicity.


Every vendor evaluation I have sat in eventually reaches the same dead end. One side says the platform is simple to run. The other side says their platform is simpler.


Nobody can prove either claim, so the conversation drifts to the demo, the reference call, the gut feeling in the room. That is not how you should be making a decision that determines how your team will spend the next five years.


I have run production data protection environments. I have watched teams get buried under fragmented tooling that promised automation and delivered tickets instead.


“Reduced complexity” is not a feeling you should have to take on faith. It is something you should be able to calculate.


### The Metric the Industry Has Been Missing


We have started using a simple ratio internally and with customers: total protected capacity divided by the number of full-time staff required to run it. We call it the data protection gearing ratio.


**Protected Capacity (PB) / FTEs = Data Protection Gearing Ratio**


That’s it. No survey questions about satisfaction. No adjectives. A number, calculated from data you already have.


Here is why it matters more than the metrics it replaces. Calculating the number of backup jobs per person made sense a decade ago, when a job represented a discrete unit of manual effort. It does not reflect how modern platforms operate today, where automation absorbs the routine work and a single administrator can be accountable for petabytes, not job counts.


Measuring jobs per person in an automated environment tells you nothing about whether the automation is actually working.


### What It Looks Like in Practice


One clarification before the number, because it trips people up. **Protected capacity means the full, uncompressed, undeduplicated size of the applications being protected, not the physical disk behind them.**


That distinction matters because it is the whole point. Commvault’s own production environment protects 42.39 PB of application data on 9.26 PB of physical disk, an 81.91% space savings from deduplication and compression.


The ratio is not just a measure of how many petabytes a person can watch over. It is a measure of how much architecture is doing the work before headcount ever enters the picture.


With that in mind: Commvault runs its own production backup environment on 42.39 PB of protected capacity with two FTEs. That is a gearing ratio of 21.20 PB per FTE. Industry benchmarks for modern platforms typically land between 5 and 25 PB per FTE, depending on environment complexity, so that number sits at the high end of what is achievable today.


**Metric**


**Value**


**Definition**


Protected Capacity (Front-End)


42.39 PB


Full, uncompressed, undeduplicated application size protected in our environment


Total Disk Capacity


9.26 PB


Physical target storage


Total Used Space


7.89 PB


Current utilization


Total Data Written


7.67 PB


Logical data written to disk


Space Savings


81.91%


Deduplication and compression efficiency


Data Protection FTEs


2


Number of full-time admins managing Commvault’s own production backup estate


###


### Data Protection Gearing Ratio = 42.39 PB / 2 FTEs = 21.20 PB per FTE


I want to be direct about what this number does not do. It does not account for a multi-cloud footprint, cyber recovery requirements, or a compliance-heavy application mix, all of which will pull the ratio down for reasons that have nothing to do with how good the platform is.


A ratio in isolation is not a verdict. A ratio measured before and after a migration is.


That is the actual use case. Baseline your current environment on your current tools. Set a target ratio based on your growth projections and your team’s capacity. Then hold your vendor to it after the implementation is done, not just during the sales cycle.


### The Board-Level Implication


If you are the one signing off on a platform migration, you are not just being asked to trust that a new platform is easier to run. You are being asked to fund a specific operational outcome. A data protection gearing ratio target gives you a way to write that outcome into the business case and check it 12 months later.


This is the same discipline we apply to mean time to clean recovery (MTCR). Recovery capability is not something you claim, it is something you measure and re-measure until the number tells you the truth. Operational efficiency deserves the same standard.


### The Challenge


Ask your current vendor for the gearing ratio of your own environment today. If they cannot produce it, that tells you something about how well they understand what “simple to manage” means for your team.


And if you are evaluating a new platform, do not accept “easier to use” as an answer. Ask what ratio they will commit to, and ask again after year one.


### FAQs


**Q: What is the data protection gearing ratio?**


**A:** The data protection gearing ratio measures the amount of protected data capacity managed by each full-time administrator. It provides an objective way to evaluate operational efficiency rather than relying on subjective impressions of platform usability.


**Q: Why is this metric more useful than backup jobs per administrator?**


**A:** Modern data protection platforms automate much of the routine work that previously required manual effort. As a result, counting backup jobs no longer reflects the true workload or efficiency of an operations team.


**Q: What does “protected capacity” mean in this calculation?**


**A:** Protected capacity refers to the full, uncompressed, and undeduplicated size of the application data being protected. This measurement reflects the actual workload managed by the platform rather than the physical storage consumed after optimization.


**Q: Does a higher gearing ratio always indicate a better platform?**


**A:** Not necessarily. Environmental complexity, including multi-cloud deployments, cyber resilience requirements, and regulatory obligations, can reduce the ratio even when the platform performs well. The metric is most valuable when comparing the same environment before and after a migration.


**Q: How should organizations use the data protection gearing ratio during vendor evaluations?**


**A:** Organizations should establish a baseline using their current environment, define a target ratio aligned with future growth, and ask vendors to commit to achieving measurable improvements after implementation. This approach shifts the conversation from marketing claims to verifiable business outcomes.


**Q: What is the broader business value of this metric?**


**A:** The data protection gearing ratio enables executives to quantify expected operational efficiency gains and include them in the business case for a platform investment. It also provides a benchmark that can be reviewed after deployment to confirm the promised results were achieved.


[Rajiv Kottomtharayil](https://www.linkedin.com/in/rajiv-k-a8a123211/) is Chief Products Officer at Commvault.
