---
schema_version: "1.0.0"
document_id: "5efb9f99a787ce889c879869153db9fd67efaeea485bdd0ac987a66bd7d925bb"
company_key: "yc-knowlify"
company: "Knowlify"
source_id: "yc-knowlify-news-import-29601cf83fbc"
canonical_url: "https://knowlify.com/articles/ai-video-vendor-sla"
published_at: "2026-07-18T00:00:00+00:00"
first_seen_at: "2026-07-25T11:00:03.967721+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:92846f1daab3c52c756f61d3958190f835d3c8079b2b28653c715e6a5791e9f4"
---

# Questions to Ask an AI Video Vendor About Uptime, SLAs, and Reliability

**Quick answer:** Ask an AI video vendor exactly what service is covered, how availability is calculated, which failures and maintenance periods are excluded, how incidents are reported, and what remedy applies. Then test the SLA against your workflow. A dashboard that loads while every render fails may meet a narrow “uptime” promise but still stop production.


## An uptime percentage is not a reliability plan


An AI video service is a chain: sign-in, project editing, asset upload, generation queues, model or rendering dependencies, storage, export and sometimes an API. The contract may measure only one part of that chain.


NIST describes service-level objectives as measurable elements within a service agreement and stresses that appropriate metrics are essential. The UK National Cyber Security Centre (NCSC) adds a practical warning: contractual availability commitments may provide compensation, but an SLA does not prevent outages if the service design is unsuitable.


Start with the business outcome. Do you need creators to edit projects during office hours? A dependable API for overnight batch generation? Delivery before a regulated training deadline? The right questions follow from the consequence of failure.


## 15 questions to ask an AI video vendor


### 1. What is the “service” in your availability calculation?


Ask whether the metric covers:


- authentication;
- the web editor;
- generation submission;
- queue processing;
- rendering and export;
- APIs and webhooks;
- asset storage and retrieval;
- administrator functions.


Request separate objectives for critical components if one blended number would hide a production-blocking failure.


### 2. What counts as unavailable?


Does “unavailable” mean the endpoint returns an error, or also that requests time out, queue indefinitely or produce unusable output? Define thresholds: for example, the proportion of generation jobs that fail for a platform reason, or a maximum queue-to-completion time for a specified workload.


Output quality is not normally an availability metric. Keep a separate acceptance process for fidelity, accuracy and editability.


### 3. How is uptime calculated?


Ask for the formula, measurement window, data source, polling frequency and time zone. Monthly availability can conceal one long outage or several disruptive incidents. Clarify whether the vendor measures server responses, customer-visible transactions or both.


Do not convert an availability target into an assumed downtime allowance without reading the exclusions and calculation method.


### 4. Which exclusions apply?


Common exclusions include scheduled maintenance, emergency maintenance, customer configuration, internet failures, third-party services, force majeure and preview features. The issue is not whether exclusions exist, but whether they are precise and allocate risk sensibly.


Ask whether failures of the vendor’s cloud, model, voice, stock-media or payment suppliers are excluded. From your perspective, a dependency failure can still stop production.


### 5. How is scheduled maintenance handled?


Confirm notice period, maintenance window, likely impact, communication channel and whether urgent changes follow a different process. If your team operates across time zones, a “low-traffic” window for the vendor may be peak time for you.


### 6. What service levels apply to generation jobs?


Measure the workflow, not just the website. Useful indicators may include:


- successful job completion rate;
- queue latency and total generation time by job class;
- export success;
- API error rate;
- webhook delivery or retry behaviour;
- recovery of projects after a failed job.


These should be objectively measurable and tied to defined workloads. A vendor should not promise one render time for every video length and complexity unless it can support that commitment.


### 7. How are severity levels defined?


Read the definitions. “Critical” should reflect business impact, such as all production blocked, data inaccessible or a security incident affecting customer data. Establish who can declare severity and whether the vendor can downgrade it without agreement.


### 8. Is the target a response or a resolution?


“Response within one hour” may mean only acknowledgement. Ask for targets covering:


- acknowledgement;
- initial qualified human response;
- investigation update cadence;
- workaround;
- restoration;
- root-cause analysis.


Resolution times are harder to guarantee, but the contract can still define escalation and communication expectations.


### 9. When is support available?


Specify hours, holidays, time zone and channels for each severity. If production runs overnight or at weekends, weekday email support may not match the risk. Read our guide to[enterprise AI video support models](https://knowlify.com/blog/ai-video-enterprise-support) before paying for a premium tier.


### 10. How will we learn about incidents?


Ask about the public status page, named contacts, in-product notices, email distribution lists and API or webhook options. The NCSC recommends pre-planned incident processes, a defined customer reporting route and an agreement on how and when the provider will tell you about an incident affecting your data.


### 11. What evidence of reliability can you provide?


Request a defined period of historical availability and incident information, with the same calculation used in the proposed SLA. Ask for:


- status-page history;
- material incident summaries;
- restoration and post-incident review process;
- capacity and load-testing approach;
- backup and recovery scope;
- relevant independent assurance reports.


Evidence should be current and scoped to the service you will buy. A certificate is not a substitute for understanding the architecture and controls.


### 12. How resilient are the service and its dependencies?


The NCSC advises customers to understand resilience across data centres, availability zones or regions and the impact of dependent services. Ask what is redundant, what fails over automatically, what requires manual intervention and what the customer must configure.


For your own workflow, decide whether you can queue work, retain editable source material, export intermediate assets or use a documented fallback.


### 13. What is the disaster-recovery commitment?


Ask for recovery time objective (RTO) and recovery point objective (RPO), their scope, and evidence that recovery is tested. RTO concerns the target time to restore; RPO concerns the acceptable point to which data may be recovered. Ensure both cover projects and customer assets, not merely account records.


### 14. What remedy applies if a target is missed?


Service credits are common. Clarify:


- whether credits are automatic or must be claimed;
- the claim deadline and required evidence;
- caps;
- which invoice or renewal receives the credit;
- repeated-failure termination rights;
- whether credits are the exclusive remedy.


A small future credit may not address a missed launch. Your operational fallback often matters more.


### 15. How do we leave or continue operating?


Confirm export formats, data retrieval period, deletion process, assistance, fees and access after termination. Include editable project data where technically available, not only final MP4 files. Procurement should cover exit before dependence grows.
