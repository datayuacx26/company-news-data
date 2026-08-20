---
schema_version: "1.0.0"
document_id: "d227655bf9754cbc19cb7808de60757d1366062d75dc94f860abbffe300fba8a"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/incident-report-july-22-2026"
published_at: "2026-08-14T17:06:07+00:00"
first_seen_at: "2026-08-14T19:36:38.229357+00:00"
fetched_at: "2026-08-14T19:36:39.533001+00:00"
content_hash: "sha256:f6d2a623f8c8d0f24e58d5311198b330a90617ab51c37cb83665cac02ffbf4ea"
---

# Incident Report: July 22, 2026

On Wednesday, July 22, between approximately 3:15 and 4:10 AM EDT, customers in our European region experienced interruptions in accessing CRM tools. No data was lost during this incident and services in non-European regions operated without interruption. We have completed a thorough analysis of this incident. Below is a detailed account of what happened, why it occurred, and the actions we are taking to prevent similar issues in the future.


# What Happened


On Monday, July 21, our engineering team applied a routine configuration change intended to fix a known bug regarding background worker threads. While the fix was correct in its intent, it inadvertently triggered a separate, latent code defect in a core CRM service that serves data for many HubSpot tools and pages.


This defect caused the service to consume significantly more computational resources than usual. The system had enough spare capacity to absorb this additional load during the low-traffic overnight hours. However, as the European business day began and traffic increased the following morning, the service exhausted its available resources, leading to severe performance degradation for customers navigating the CRM pages.


# Customer Impact


On the morning of July 22, HubSpot customers in European regions experienced disruption in accessing CRM tools for approximately 55 minutes. Disruption included slow load times, timeouts, and “There was a problem loading this page” errors on some UIs. Impacted areas of the product included automation workflows, forms, email management, help desk, and campaigns. Non-CRM tools (marketing website pages, landing pages, etc.) were not directly affected.


# Technical Impact Details


Under the new configuration, the service failed to reuse a shared internal communication client. Instead, it unnecessarily reconstructed a network client on every single incoming request. Executing this expensive operation at high frequency triggered a cascade of capacity issues:


- **Memory Allocation Pressure:** This client instantiation pattern caused an immediate 10x spike in memory allocations. While the service absorbed this extra work during low-traffic overnight hours, it led to severe resource pressure as morning requests surged.


- **Capacity Exhaustion:** As overloaded instances failed automated health checks, the system removed them from the pool. This further exacerbated the capacity deficit, creating a downward spiral instead of a self-healing response.


# Timeline of Events


July 21


- **5:10 PM EDT:** A configuration change is deployed to address a bug with background worker threads.


July 22


- **3:15 AM EDT:** As business-day traffic increases in the European region, customer facing impact begins.


- **3:21 AM EDT:** Internal incident is officially declared after increased latency on the CRM service triggers our automated alert thresholds.


- **4:10 AM EDT:** Our engineering team manually scales up the service’s capacity, stabilizing the acute resource capacity issues and ending immediate customer impact.


- **6:45 AM EDT:** After tracing the root cause, the engineering team reverted the configuration change.


# What We’re Doing to Improve


# Our Commitment to Reliability


We know that CRM data is central to your HubSpot experience. We sincerely apologize for the ways this incident impacted your morning workflows. This incident highlighted critical opportunities to improve compile-time safety checks, configuration change guardrails, and automated resource monitoring. Reliability is a core tenet of HubSpot Engineering and we are committed to applying these critical learnings for the resilience of the HubSpot platform.
