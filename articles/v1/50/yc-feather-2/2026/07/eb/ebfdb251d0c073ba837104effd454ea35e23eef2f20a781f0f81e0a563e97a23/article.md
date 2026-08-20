---
schema_version: "1.0.0"
document_id: "ebfdb251d0c073ba837104effd454ea35e23eef2f20a781f0f81e0a563e97a23"
company_key: "yc-feather-2"
company: "Feather"
source_id: "yc-feather-2-news-import-072c928cc62c"
canonical_url: "https://www.featherhq.com/old-blog-2/average-handling-time-(aht)-formula-industry-benchmarks-how-to-lower-it"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-08-13T07:12:50.962488+00:00"
fetched_at: "2026-08-13T07:12:52.895889+00:00"
content_hash: "sha256:69cb1e733e7550de7b97284daa493f6d5a3300d9c378f62f8ef443b4c8254048"
---

# Average Handling Time (AHT): Formula, Industry Benchmarks & How to Lower It

Understanding how to calculate AHT is the foundation of any serious effort to lower it. The metric is straightforward in principle, and harder in practice, because accurate calculation depends on consistent instrumentation and clean data.


### AHT formula and core components


The standard **aht formula** is:


1.


Add total talk time, total hold time, and total after-call work time over the measurement period.


2.


Divide that total by the number of calls handled during the same period.


Expressed as a single equation, AHT equals:


(Total talk time + Total hold time + Total after-call work) ÷ Number of calls handled


This formula is consistent with industry definitions and vendor glossaries.


### What each component means in practice


-


**Talk time** : The total duration the agent or system spends speaking with the caller. For AI voice agents this includes both agent speech and caller speech that contributes to the transaction.


-


**Hold time** : The total time callers spend on hold, waiting for an agent, or waiting for a callback. Hold music and voicemail wait segments should be included when they occur inside a single handled call.


-


**After-call work (ACW)** : Wrap up tasks after the call, such as logging notes, updating the CRM, or scheduling follow up.


### Measurement caveats and instrumentation


-


Do not double count segments when calls are transferred. If a call is warm transferred with context attached, the receiving agent should only account for the time they actively handle the call plus their ACW, not the time the AI agent spent before transfer.


-


Decide whether to include automated pre-call IVR navigation or post-call surveys in AHT. Many teams exclude IVR navigation, while others include it because it directly affects caller experience.


-


For voice AI deployments, ensure the system logs talk-time for both the caller and the AI agent, includes hold-music detection, and records ACW events tied to CRM updates. Feather AI provides native CRM integration and hold-music and voicemail detection, which simplifies accurate measurement and reduces manual reconciliation.


### Benchmarks by vertical


Benchmarks vary by vertical and by complexity of the transaction. Use benchmarks as a sanity check, not a hard target. Examples from industry reporting show that financial services and retail AHTs are often lower than heavily technical or multi-stakeholder workflows. For instance, Zoom reports financial services and retail averaging roughly 4.7 minutes, while business and IT services approach 9 minutes for typical interactions. Other 2025 industry references note averages near 10 minutes in certain contact center contexts.


### Approaches to lowering AHT


Reducing AHT can be pursued on the following fronts, listed from least to most operational change required:


-


**Measurement hygiene** : Ensure clean instrumentation, consistent definitions, and accurate event logging. Without reliable data, optimization is dangerous.


-


**Process simplification** : Remove unnecessary ACW steps, replace manual form filling with automated CRM updates, and standardize call flows.


-


**Self-service and deflection** : Use outbound messaging, chat, and secure web portals for transactions that do not require live voice contact.


-


**Task automation on the call** : Use AI voice agents to complete routine verifications, bookings, simple diagnostics, and appointment scheduling.


-


**Smart routing and warm transfers** : Reduce unnecessary transfers by routing to the right team, and warm transfer only the most valuable calls with full context attached.


### Comparing approaches and named competitors


-


Developer-first platforms like Vapi are the most flexible for engineering teams that want to assemble every integration and custom logic point, but they require significant engineering investment.


-


Mid-point offerings such as Retell AI balance some no-code capabilities with developer extensibility.


-


Bland AI focuses on high-volume outbound workloads, but it gates warm transfer and scheduling features behind enterprise tiers, which can limit immediate AHT gains for regulated operations.


Feather AI is positioned as a business-ready platform that companies deploy without building the stack from scratch, offering features such as native CRM integration and real-time observability to measure and reduce AHT quickly.
