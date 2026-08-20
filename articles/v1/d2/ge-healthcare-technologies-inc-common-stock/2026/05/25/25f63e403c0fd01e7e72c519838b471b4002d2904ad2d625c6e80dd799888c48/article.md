---
schema_version: "1.0.0"
document_id: "25f63e403c0fd01e7e72c519838b471b4002d2904ad2d625c6e80dd799888c48"
company_key: "ge-healthcare-technologies-inc-common-stock"
company: "GE HealthCare Technologies Inc."
source_id: "ge-healthcare-technologies-inc-common-stock-rss-f82d3b59c758"
canonical_url: "https://research.gehealthcare.com/patient-care-pathways/removing-the-friction-in-care-where-healthcare-ai-is-headed/"
published_at: "2026-05-11T21:54:12+00:00"
first_seen_at: "2026-07-20T03:33:19.174351+00:00"
fetched_at: "2026-07-28T21:14:05.751806+00:00"
content_hash: "sha256:d1e4204b6f7c525f2571cdcaf99db31d255a06514fa82b3b4c48a46a7514d73c"
---

# Removing the Friction in Care: Where Healthcare AI Is Headed

I hear the question a lot: are we in an AI bubble?


In medical technology, the conversation feels different because AI has been driving measurable impact for more than a decade, especially in imaging and cardiology. It has been showing up inside devices and the everyday care process, helping improve throughput and consistency without requiring teams to run a separate “AI workflow.”


Here’s where the next wave of AI becomes real: it connects the steps of care end to end, with built-in checks and clear handoffs to humans when uncertainty arises. Most delays are not because we cannot generate an image or a lab value. Delays often occur in the gaps between steps: manual handoffs, re-entering the same data, waiting for the next person or system, reconciling queues, and tracking status across teams. Organizations may benefit from using AI to help care run more smoothly: fewer handoffs, fewer repeats, faster time-to-decision, and less variability from site to site. Effective AI may feel almost invisible, because it removes work instead of adding clicks and alerts.


*The technical stack that connects the steps of care end to end*


## **Improved visibility across the workflow**


You can see a similar pattern in hospital operations.


GE HealthCare Command Center includes AI-powered features for[census forecast and staffing.](https://research.gehealthcare.com/across-the-enterprise/the-science-behind-ge-healthcares-command-center-census-forecast-and-staffing-helping-hospitals-predict-resource-needs-with-astonishing-accuracy-jb33676xx/) Duke Health, using predictive staffing and flow tools, reported achieving 95% accuracy in forecasting staffing needs up to 14 days in advance and reduced temporary labor requirements by 50%.1


The Queen’s Health Systems, using a more connected operating model, reported a 22.2% increase in patient transfer admissions in the first 10 months, a 41.2% decrease in emergency department length of stay, a 1.07-day reduction in overall length of stay, and an estimated $20 million in first-year savings.2 Similarly, Providence Swedish reported an increase in system-wide patient admission volume by 15%, and was able to create capacity for approximately 19,000 additional patients annually.3


The common thread across these examples was improved visibility across the care process, earlier signals, and faster operational decisions that help reduce friction before it compounds.


You can see what “invisible AI” looks like when deep learning is embedded directly into imaging. When intelligence sits inside the scanner and the normal acquisition process, it can help shorten exams and improve image quality without creating a new set of steps around it. For example, in MRI, deep learning has enabled scans up to 86% faster (compared to fully sampled MR datasets) and improved image resolution by up to 55% in specific applications and under defined conditions. These improvements could translate into usable capacity. Exams may complete faster and more consistently within the normal scanner workflow, which could support increased throughput and reduce burden on radiology staff.


You can also see the same pattern when systems reduce friction between steps. One public case study that illustrates this is the University of Debrecen in radiation therapy. Radiation oncology involves several sequential stages, from diagnosis to follow-up, and even minor inefficiencies at any step can result in delays. In addition, creating a treatment plan often involves working across multiple disconnected platforms, with redundant data entry and communication gaps that can delay approval and slow down the transition from imaging to treatment.


With GE HealthCare’s Intelligent Radiation Therapy (which includes integration with AI-powered MVision technology for organ-at-risk segmentation,) the University of Debrecen moved from fragmented tracking, including Excel sheets and custom scripts, to a more integrated process where key steps are visible, time-stamped, and routed across systems. As gaps between steps are reduced, scaling becomes more feasible. In that environment, VMAT, a targeted radiation therapy technique, increased by 30x, from 35 cases to 1,022, and AI-assisted contouring reduced contouring time by up to 95% across more than 300 structures in this specific implementation.4 This reflects a care process becoming more predictable and scalable.


This is also a useful lens for thinking about how baseline scans and change-over-time assessment could become more actionable within the medical record. Baseline plus longitudinal comparison becomes more practical when imaging acquisition is fast and consistent, interpretation is supported, and follow-up remains guided by appropriate clinical oversight. The goal is not to increase data volume for its own sake, but to help clinicians move from imaging signal to informed clinical decision more efficiently, while recognizing that results depend on specific clinical context and implementation.


That is why research directions such as full-body 3D MRI and medical multimodal AI are being explored: moving from “one organ, one task” tools toward systems that may support multiple tasks across the body and connect model outputs to the underlying data they reference. Grounding outputs to underlying data is an important consideration for clinical trust and real-world adoption at scale.


> *The next wave: AI that integrates into care processes by reducing friction, improving consistency, and supporting trust at scale.* – Dr. Taha Kass-Hout


### **The importance of “subtracting” work**


If there is one reason pilots fail, it is when the tool does not reduce workload. If a solution adds clicks and alerts without removing a step, it becomes a parallel process. People still complete the original work end to end, while also managing additional systems, queues, and documentation. Early validation and double-checking are appropriate. However, if these steps occur outside existing workflows, throughput can decline. If signals are not well-calibrated, users may become desensitized. A pilot may appear promising, but the real process may not improve. The key question is: what is removed from the team’s workload? If nothing is removed, the solution may remain in a pilot state.


Trust and regulation influence whether this can scale. Healthcare already operates in an environment where vetting before release is expected. The question is how systems improve over time while maintaining safety. A structured approach includes continuous improvement with defined guardrails: specifying the types of changes that may be made, how those changes will be implemented and validated against a baseline, assessing their potential impact, monitoring real-world performance, and enabling rollback if needed. This is consistent with FDA’s Predetermined Change Control Plan (PCCP) approach for AI-enabled device software functions.


### **Digital twins, reimbursement and more**


Evidence should cover both benchmark performance and real-world behavior: drift, overrides, escalation patterns, downstream callbacks, and operational burden. Digital twins, simulation, and synthetic data could support testing of rare edge and repeatable validation before clinical deployment, particularly in high-volume environments where small changes can have downstream impact.


Adoption and reimbursement follow similar principles. Reimbursement can be a bottleneck. Without predictable coverage, coding, and payment, even cleared tools may struggle to scale. A practical path to reimbursement is demonstrating value in operational terms that stakeholders can evaluate: capacity created, delays reduced, rework avoided, overtime reduced, repeat exams prevented, and reduced reliance on temporary labor. When value is clear within real workflows, adoption discussions may progress more effectively.


A future scenario includes supervised, assistive systems that help make outcomes less dependent on location or timing, while maintaining appropriate human oversight. A patient receives an exam, quality is verified promptly, results move efficiently toward clinical review, and the system identifies when human input is required. Innovation can then progress with appropriate governance: bounded updates, supporting evidence, real-world monitoring, and the ability to adjust when needed.


That is the next wave: AI that integrates into care processes by reducing friction, improving consistency, and supporting trust at scale.


\[1\] Data provided is for the institution listed. These results might not be replicable for other institutions and settings. Data available on file.


\[2\] All metrics provided by The Queen’s Health Systems. Length of stay, ED Admit LOS, ED Boarding, and Transfer metrics are based on data from January 2024 to October 2024. Savings calculation is based on YoY ALOS metrics from 2023 to 2024 and assume a $1200 cost per patient per day.


\[3\] Metrics provided by Providence Puget Sound based on data collected from 2023-2024.


\[4\] This data is unique to the University of Debrecen. The numbers here pertain to two patients treated at the University of Debrecen’s Radiotherapy department in 2025. Quantitative and qualitative results in this case study are unique to the University of Debrecen. These results might not be replicable for other institutions and settings. Data provided by University of Debrecen for 2016-2025 time period.
