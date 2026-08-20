---
schema_version: "1.0.0"
document_id: "4cc0551cbaeee40fe052ea4b2eeec99297863ebc9bb123bf956755eab86039cf"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/the-admins-guide-to-32tb-ultrastar-density/"
published_at: "2026-06-15T17:12:59+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:d6cadecbe0c2e9c4ac05434e4dcea3eb972f59776d5b22746a8e4937db7158ad"
---

# The Admin’s Guide to 32TB+ Ultrastar Density

June 15, 2026


4 min read


[Products](https://blog.westerndigital.com/category/products/)


# The Admin’s Guide to 32TB+ Ultrastar Density


[Andrew Marquez](https://blog.westerndigital.com/?guest_author=andrew-marquez)


## **What is the highest capacity enterprise HDD for AI?**


## Key takeaways


- WD’s highest shipping enterprise HDD capacity today: 32TB class (Ultrastar DC HC690)
- Primary value: Increased storage density and improved TCO (total cost of ownership) for nearline and retention tiers
- Best suited for controlled-write, sequential, or append-heavy workloads
- Forward-looking roadmap: 40TB UltraSMR ePMR drives disclosed as in qualification, not yet broadly available


## Executive summary


AI workloads are accelerating unstructured data growth across enterprises, especially within data lake storage, nearline tiers, and long-term retention environments. While high-performance flash remains critical for active AI training and inference, most AI-era data must be stored with a focus on density, predictability, and TCO (total cost of ownership).


Today, the highest generally available, shipping enterprise HDD capacity is the 32TB class, exemplified by the Ultrastar DC HC690. This density-focused HDD is designed to increase terabytes per rack for those who’ve already adopted SMR.


This guide explains what the highest-capacity enterprise HDD means in practice, how 32TB+ drives fit into AI data lake storage architectures, and what IT administrators should evaluate for predictable qualification, deployment, and long-term operations.


## What is the highest capacity Enterprise HDD for AI?


For enterprise environments deploying AI at scale, the most important question is not theoretical maximum capacity, but the highest capacity that is broadly available, qualified, and operationally predictable.


The 32TB Ultrastar DC HC690 delivers one of the industry‑leading capacities, optimized to maximize storage density and watts‑per‑terabyte efficiency in existing data center footprints.


## What ‘highest capacity’ means for IT admins


For IT administrators, “highest capacity” should be evaluated in the context of workload suitability, zoned storage/SMR management, and predictable behavior across controllers and storage software.


The 32TB Ultrastar DC HC690 delivers a well-qualified balance of TB-per-rack density, watt-per-terabyte efficiency, and compatibility with standard data center infrastructure, making it a practical choice for AI-era storage expansion.


## AI data growth and the density problem


AI pipelines can continuously generate and retain massive volumes of unstructured data, including raw training data, features, checkpoints, logs, and generated output. As this data accumulates, data lake storage and warm/cold tiers expand faster than performance tiers.


At scale, the constraints quickly become rack space, power, and cooling rather than raw drive counts. This makes higher TB-per-drive density and improved watt-per-terabyte efficiency foundational metrics for sustainable growth.


## The 32TB+ Ultrastar density class


The Ultrastar DC HC690 32TB HDD is purpose-built to increase storage density in existing footprints. Using an 11-disk platform with UltraSMR recording assisted by ePMR, it enables higher areal density without changing standard 3.5-inch form factors.


For IT admins, the outcome is straightforward: more usable capacity per slot, per shelf, and per rack, enabling higher-capacity data lake storage with fewer systems overall.


Learn more about the[Ultrastar DC HC690](https://www.westerndigital.com/products/internal-drives/data-center-drives/ultrastar-dc-hc690-hdd)


## Determining workload fit


High-capacity SMR-class drives are not designed for every workload. They perform best in environments that support zoned storage/SMR management and controlled write patterns.


- AI data lake storage with staged ingestion
- Warm and cold data retention tiers
- Object storage and append-heavy repositories


Workloads dominated by small, random overwrites or strict low-latency write requirements are typically better served by other tiers.


## Deployment checklist for IT admins


### Qualification and planning


- Validate controller, firmware, and enclosure compatibility
- Pilot the exact drive SKUs planned for fleet deployment
- Confirm storage software support for SMR/zoned write behavior


### Operational alignment


- Explicitly map workloads to warm or cold data tiers
- Implement buffering and write-shaping policies
- Tune rebuild and scrub throttles to limit operational risk


### Monitoring and economics


- Monitor latency, queue depth, and rebuild duration
- Track watt-per-terabyte efficiency as a primary KPI


## Forward-looking perspective


Western Digital has disclosed a 40TB UltraSMR ePMR HDD currently in customer qualification as part of its AI-era capacity roadmap. Admins should treat this as forward-looking context rather than assume near-term availability.


This measured approach allows organizations to plan for density growth while maintaining predictable qualification and operational stability across multiple years.


## Conclusion


For AI-era environments experiencing rapid unstructured data growth, the 32TB Ultrastar density class provides a pragmatic balance of capacity, efficiency, and operational predictability. When aligned with the right workloads, it enables scalable data lake storage growth with improved TCO and minimal disruption.


[Artificial Intelligence](https://blog.westerndigital.com/tag/ai/)[Enterprise](https://blog.westerndigital.com/tag/enterprise/)[HDD](https://blog.westerndigital.com/tag/hdd/)
