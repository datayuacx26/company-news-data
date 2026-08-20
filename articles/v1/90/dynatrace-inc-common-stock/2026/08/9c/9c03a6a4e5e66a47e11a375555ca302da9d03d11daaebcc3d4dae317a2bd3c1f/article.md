---
schema_version: "1.0.0"
document_id: "9c03a6a4e5e66a47e11a375555ca302da9d03d11daaebcc3d4dae317a2bd3c1f"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/the-us-government-just-rewrote-the-rules-on-federal-logging/"
published_at: "2026-08-06T14:11:18+00:00"
first_seen_at: "2026-08-06T15:59:41.901806+00:00"
fetched_at: "2026-08-06T15:59:43.817041+00:00"
content_hash: "sha256:65aa5deb6a16a9d19d4090165978242dcbf7d6d7b460dc2ba0c3c18d15d62522"
---

# The US government just rewrote the rules on federal logging

## Key takeaways


- **The US Office of Management and Budget (OMB) has replaced its 2021 logging mandate.** M-26-14 rescinds M-21-31 and shifts federal agencies away from broad collection and retention requirements towards a risk-based, prioritized logging approach.
- **The new guidance focuses on outcomes, not log retention & collection.** Agencies are expected to prioritize telemetry that helps them detect threats in real time and investigate incidents after they happen.
- **Modern telemetry pipelines help make that shift practical.** They can reduce waste, manage sensitive data, and route the right telemetry to the right destination.
- **Context is what turns logs into action.** Connecting logs with metrics, traces, topology, and security telemetry helps teams investigate faster and support broader Zero Trust goals.


## Why M-26-14 needs modern log


management


For four years, federal civilian agencies worked toward a logging standard often not reached. On May 22, 2026, the Office of Management and Budget replaced it.


OMB Memorandum M-26-14 rescinds M-21-31, the 2021 mandate that pushed agencies to log nearly everything and retain it at scale. The reason for the shift is one that security teams will recognize immediately: collecting and keeping that volume of data proved neither operationally feasible, nor cost-effective, nor did it automatically create better security outcomes.


The new OMB’s M-26-14 federal logging guidance recognizes that real-time threat detection and post-incident investigation are different challenges that require different approaches. Rather than emphasizing log volume, the memo introduces a risk-based framework focused on achieving specific security outcomes.


This shift extends beyond government. Across industries, organizations are rethinking traditional logging strategies as telemetry volumes grow, environments become more distributed, and AI-powered workloads introduce new operational complexity.


The future isn’t about collecting more data. It’s about turning telemetry into actionable intelligence.


## Why the mandate changed


M-21-31 accelerated logging adoption across government, but implementation proved difficult. When agencies were expected to reach the highest event-logging maturity level, only a small minority met the target. Agencies cited staffing shortages, technical challenges, and difficulties sharing threat information.


More fundamentally, many organizations found themselves collecting enormous volumes of telemetry without a clear strategy for turning that data into operational outcomes. The result was often higher costs, greater complexity, and larger datasets that security teams struggled to use effectively.


[Willie Hicks](https://www.dynatrace.com/news/blog/author/willie-hicks/) , Global Field CTO for Governments and Public Sector at Dynatrace, has seen that challenge play out repeatedly in conversations with federal agencies working to meet the original M-21-31 requirements: “Over the years I’ve had the privilege of sitting with many Government CIOs, CISOs, and security professionals tasked with meeting M-21-31. For many agencies, the mandate became a cost spiral that didn’t make them meaningfully safer. It just created a bigger haystack.”


M-26-14 reflects a shift away from volume-based logging requirements toward a risk-based framework focused on measurable security outcomes.


## Two objectives, two different requirements


At the center of M-26-14 are two operational objectives.


**Continuous Event Monitoring (CEM)** focuses on real-time detection. Security teams need to identify anomalies, detect threats, generate alerts, and understand emerging issues as they happen.


**Threat Hunting, Investigation, Response, and Forensics (THIRF)** focuses on understanding what happened after an incident occurs. Investigators need to reconstruct timelines, determine impact, trace attacker activity, and support remediation.


While both rely on telemetry, they answer different questions:


- **CEM:** Is something wrong right now?


- **THIRF:** What happened, how did it happen, and how do we prevent it from happening again?


Success under M-26-14 isn’t simply retaining data longer. It’s ensuring the right data is available, accessible, and actionable when needed.


## Telemetry pipeline intelligence is a critical first step


One of the most important shifts in M-26-14 is implicit rather than explicit.


The guidance recognizes that Continuous Event Monitoring and Threat Hunting, Investigation, Response, and Forensics have different data requirements. Searchable data must remain available for operational monitoring, while larger volumes of historical telemetry may be retained for investigation and forensics.


That creates a new architectural challenge: deciding what telemetry to collect, what to retain, what to filter, and where it should go. A telemetry pipeline is the layer that makes those decisions possible.


Modern telemetry pipelines help organizations control costs, govern sensitive data, transform telemetry, and intelligently route data across increasingly complex environments. Rather than forcing agencies to centralize all telemetry into a single repository, pipeline intelligence allows organizations to deliver the right data to the right destination based on operational requirements.


[With Dynatrace and Bindplane](https://www.dynatrace.com/hub/detail/dynatrace-bindplane/) , organizations can collect telemetry across cloud, hybrid, IT, and OT environments, enrich and transform data at the source, mask sensitive information before it reaches downstream systems, and route telemetry based on operational requirements.


That control cuts waste, lowers cost, and makes governance less brittle.


A pipeline, though, is only the first layer.


Efficient collection of data does not detect threats, find root cause, or show how an issue has spread across applications, infrastructure, and users.


Collection gives teams the raw material. Intelligence turns it into action.


## Managing cost without sacrificing visibility


So,


how


do


organizations balance cost optimization with investigative readiness


?


Pre-ingestion filtering can reduce storage costs and operational noise, particularly for CEM use cases. But forensic investigations have different requirements. Investigators can only analyze the data that exists. That necessitates smarter telemetry management.


Organizations need strategies that optimize telemetry for detection while preserving the visibility required for investigation and forensics. Intelligent routing helps achieve that balance by prioritizing high-signal telemetry for operational monitoring while retaining full-fidelity data for future investigations.


At the same time, agencies must address a growing challenge: sensitive data exposure.


As telemetry collection expands, logs increasingly contain credentials, tokens, personally identifiable information, and other sensitive data. With Dynatrace and Bindplane, teams can apply masking, filtering, and transformation before telemetry reaches downstream systems, helping enforce security and compliance at the source.


## From reactive monitoring to preventive operations


M-26-14 calls on agencies to spot anomalies, monitor identity activity, track infrastructure changes, and detect suspicious behavior across distributed environments. Static SIEM rules and manual correlation cannot carry that load for long.


Cloud-native systems move too quickly. Hybrid infrastructure adds too many seams. AI-driven workloads introduce behavior that legacy tools were not built to read.


Preventive operations require context.


With Dynatrace, logs are analyzed alongside metrics, traces, events, topology, and security telemetry, so teams can see how changes, dependencies, infrastructure conditions, and user activity fit together. Dynatrace Intelligence applies causal, predictive, and generative AI to identify anomalies, pinpoint root cause, reduce alert noise, and speed remediation.


The result is earlier detection, faster investigation, and a clearer path from signal to response.


## Investigation requires context, not just retention


THIRF is often viewed as a storage and retrieval challenge. Retain logs, make them searchable, and ensure investigators can access them when needed. That’s necessary, but not sufficient.


During an investigation, responders need to understand affected systems, exposed services, infrastructure changes, user activity, and attack progression. Logs alone rarely provide the full picture.


Dynatrace brings logs together with metrics, traces, events, topology, and security telemetry. Analysts can investigate incidents with connected context rather than manually stitching together evidence across multiple tools.


That reduces manual effort, accelerates investigations, and helps teams move more quickly from detection to remediation.


## Logging and Zero Trust are becoming the same conversation


M-26-14 explicitly aligns future logging guidance with the[CISA Zero Trust Maturity Model](https://www.cisa.gov/sites/default/files/2023-04/zero_trust_maturity_model_v2_508.pdf) , particularly the Visibility and Analytics capability that supports all Zero Trust pillars.


This means logging can no longer be treated as a standalone compliance exercise.


Risk-based logging depends on understanding assets, dependencies, applications, services, and data flows across the environment. Without that visibility, prioritization becomes guesswork.


Dynatrace automatically discovers and maps applications, services, hosts, containers, and dependencies across dynamic environments, providing the context needed for stronger Zero Trust visibility and more effective security operations.


## What agencies should do now


While agencies await the Logging Reference Architecture, there are several steps worth taking now:


- Map telemetry sources to CEM and THIRF objectives.
- Evaluate filtering strategies carefully to avoid creating investigative blind spots.
- Assess detection maturity, not just collection and retention maturity.
- Align logging strategy with broader Zero Trust initiatives.
- Explore how telemetry pipeline intelligence, unified analytics, and AI-powered observability can improve both security outcomes and operational efficiency.


Download the State of Log Management 2026 report to explore benchmark data on how AI workloads are exploding log volume and costs, and why unified observability is now essential for reliable, trustworthy AI.
[Learn more](https://www.dynatrace.com/resources/ebooks/state-of-log-management-2026/)
