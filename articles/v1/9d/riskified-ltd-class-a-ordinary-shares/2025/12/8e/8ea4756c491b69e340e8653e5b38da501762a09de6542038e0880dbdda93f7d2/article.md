---
schema_version: "1.0.0"
document_id: "8ea4756c491b69e340e8653e5b38da501762a09de6542038e0880dbdda93f7d2"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/building-data-you-can-trust-our-journey-to-a-single-source-of-truth-baab80277d4a"
published_at: "2025-12-02T12:46:32+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:51616679690aa87dc8f8156856115427c329d6cba0f1d437633ce28a1db75974"
---

# Building Data You Can Trust: Our Journey to a Single Source of Truth

#### Written by[Shlomit Goldenberg](https://medium.com/u/14a742d0aa5d) and[Lihi Gilboa (Aziz)](https://medium.com/u/b9a19977d206)


In today’s data-driven world, insights are only as good as the trust you have in them. At Riskified, we identified a growing gap in how our KPIs were being calculated, interpreted, and reported across different departments. What started as an internal BI frustration quickly became a cross-company challenge and eventually, a major opportunity to lead infrastructure change that impacted nearly every team.


### The Problem: Too Many Truths, Not Enough Clarity


We faced a familiar but critical pain point: there was no single source of truth for our data.


- **KPIs were being calculated in multiple places** by analysts, developers, BI teams - leading to inconsistent reports, such as customer facing vs internal reports and misalignment between different departments.
- **Logic was duplicated across pipelines** , any change in KPI logic required migration and updating code in multiple locations, a process prone to mistakes and leaving outdated logic behind.
- **We lacked the ability to track the full lifecycle of an order** , since we only had access to its final status. An order can go through multiple statuses before completion, and without visibility into those changes, we couldn’t understand the full flow. This led to internal confusion and reduced report accuracy.


### Our Mission: A Unified Source of Truth


Our goal was ambitious: to build a single, trusted source of truth for key business metrics - a centralized data infrastructure that could be used across the company.


#### Key Requirements


1. **Accuracy** We needed to move away from final status-based reporting toward **event-based** reporting. KPIs had to be computed in one central place. Updating a KPI should be done once and flow everywhere without manual fixes per department.
2. **Near Real-Time Access** Timeliness was essential in order to support **hourly refreshes** to serve near real-time needs, especially for merchant-facing insights. Our BI sources were stale. Data was pulled from replicated production tables that lagged behind. We shifted to consuming directly from the Data Lake, where events stream in real time.
3. **Data Completeness and Reliability** Many teams relied on final aggregated tables. Any delay or inconsistency led to **downstream issues in reporting, faulty product logic, and disrupted delivery flows** .


### Our Solution


#### KPI Building Blocks at the Core


The heart of our BI transformation wasn’t just adopting a data lake-centric approach, it was creating KPI Building Blocks. These are reusable, event-based components that serve as the foundation for every metric across the company.


KPI Building Blocks are boolean indications tracking each stage of an order’s lifecycle, calculated from specific events or event combinations.


> **Example: Review Rate Simplified with KPI Building Blocks**


> To illustrate, take the metric Review Rate = reviewed count / submitted count.


> In the old architecture, the logic was tightly coupled to order statuses.


> To compute the metric, analysts needed to remember which statuses counted as reviewed and which as submitted. Adding a new status or changing review flow logic required updating code in multiple places, often leading to inconsistencies. For example:


```text
reviewed_cnt = SUM(CASE WHEN order_status IN ('under_review', 'review_approved', 'review_declined') THEN 1 ELSE 0 END)  submitted_cnt = SUM(CASE WHEN order_status IN ('submitted', 'under_review', 'review_approved', 'review_declined', 'cancelled') THEN 1 ELSE 0 END)
```


> In the new model, the KPI logic is abstracted into reusable Building Blocks, calculated once at the infrastructure level:


```text
is_submitted = CASE WHEN event_name = 'order_submitted' THEN TRUE ELSE FALSE END  is_reviewed = CASE WHEN event_name = 'order_reviewed' THEN TRUE ELSE FALSE END
```


> Now, calculating the metric is as simple as:


```text
SUM(is_reviewed::integer) / SUM(is_submitted::integer)
```


> This shift eliminated logic duplication, simplified maintenance, and ensured every team across the company uses the exact same definition of Review Rate.


#### Scaling to Production


We re-architected our pipelines to consume directly from the Data Lake, where events stream is near real time. This architecture now processes tens of millions order events daily, maintaining near real-time freshness and ensuring consistent KPI computation across all data consumers. From there, we built key components of the solution:


- **Hourly ingestion and transformation**
Moved from replicated production tables to event based streams to support multiple daily runs.
- **Unified data model**
All ETL processes rely on the same building blocks, removing logic duplication, ensuring governance and simplifying maintenance.
- **Distinct-update tables & merge processes**
We used incremental tables to track all orders that needed to be updated from multiple sources. In every incremental run, the population included checks across all sources: any order updated in at least one source entered the incremental table. At the end, all downstream flows sat on top of this table, ensuring every table got the exact same updated population. This allowed us to manage high data volumes with daily and hourly merges, updating only what changed. As a result, by focusing only on records that truly required updates, we cut our pipeline runtime from three hours to an hour and a half.


#### Monitoring & Self-Healing Pipelines


At scale, bad data spreads quickly. That’s why we built monitoring and automatic fixes into every stage of the pipeline, so issues get caught and corrected early, keeping data reliable.


**Monitoring layers:**


- **Data freshness alerts** Catch latency before it hits consumers
- **Volume checks & anomaly detection** Flag unexpected drops and spikes in our Building Blocks to ensure completeness
- **Data Health Check for service consumptions**
Validate data against predefined “contracts” before publishing. If the contract isn’t met, the process stops and data isn’t pushed to the service, preventing customers from ever seeing incorrect data until it’s fixed.


**Fix layers:**


- **Ingestion completeness & fixes** Confirm all expected data from the Data Lake is loaded into the DWH. Missing records trigger a reload.
- **DWH load validation** We created a set of validations for known issues like malformed records, nulls, and value mismatches. Any order that failed was written into the incremental table and automatically corrected along the flow. New issues identified during the process are continuously added to the validation set, ensuring ongoing improvement and coverage.
- **External reporting auto-fix** For sensitive or critical data, we run ongoing checks to compare the Building Blocks used to generate metrics in external services with the source data in the DWH. Since external services may temporarily rely on real-time calculations for recent hours (before the DWH is updated), or due to missed or failed ETL runs, any mismatch triggers an automatic re-sync and correction to ensure consistency with the DWH.


With these monitoring and fix layers in place, the pipeline is self-healing, reduces manual work, and builds confidence in the data.


The complete data flow solution


### Final Thoughts: Key Learnings & Recommendations


1. **Start with KPIs, not tables** Defining metrics up front, not just datasets, guided our infrastructure decisions and kept the focus on business value.
2. **Building Blocks drive accuracy and usability**
Standardizing KPI logic into reusable blocks simplified the work across teams, ensured that every report showed the same numbers, and guaranteed that no matter where a metric is accessed, whether in internal reports, external dashboards, or team-specific calculations, the result is always consistent. This eliminated discrepancies across the organization and aligned all teams around a single source of truth.


Data infrastructure is never “done”, but with this shift, we’ve laid the foundation for scalable, accurate, and trusted insights across the company.


### What’s Next: The Future of Data with AI


By structuring KPIs as reusable, event-based components with clear lineage, we’ve built data that’s not just analytics-ready but AI-ready machine consumable, explainable, and consistent. This foundation allows us to confidently build AI agents that interact directly with trusted data, generate insights, and trigger actions based on unified KPIs. Our KPI Building Blocks will serve as the data heart of these agents, ensuring that every prediction or recommendation is grounded in the same single source of truth that powers our BI today.


*Feel free to reach out on LinkedIn —*[Lihi Gilboa (Aziz)](https://www.linkedin.com/in/lihiaziz/) *and*[Shlomit Goldenberg](https://www.linkedin.com/in/shlomit-banin-goldenberg/)


---


[Building Data You Can Trust: Our Journey to a Single Source of Truth](https://medium.com/riskified-technology/building-data-you-can-trust-our-journey-to-a-single-source-of-truth-baab80277d4a) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
