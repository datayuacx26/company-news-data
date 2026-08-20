---
schema_version: "1.0.0"
document_id: "c716d387e69d82c032768653e43e6f9950fbec85a20b294687eb38b2a5d4516e"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-salesforce-eliminated-single-region-risk-and-reduced-downtime-blast-radius-at-4b-metrics-min/"
published_at: "2026-08-05T19:19:18+00:00"
first_seen_at: "2026-08-05T19:23:25.323846+00:00"
fetched_at: "2026-08-05T19:23:27.403771+00:00"
content_hash: "sha256:77d7d0af10d0ad357dcef449a3fd7f7d1b2a991303df2f944c2d367feee35df8"
---

# How Salesforce Eliminated Single-Region Risk and Reduced Downtime Blast Radius at 4B Metrics/Min

*By Priyanka Singla, Aaron Stockton, and Vincent Poon*


In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Priyanka Singla, Senior Software Engineer on our Argus team, who operates and evolves the internal observability platform ingesting approximately 4 billion metrics per minute for thousands of engineering teams.


Explore how the team scaled ingestion, storage, and query performance as telemetry volume grew across Salesforce systems, while also overcoming a single-region AWS dependency, whose blast radius could take down Argus visibility globally.


##### **What is your team’s mission in building and operating Argus as Salesforce’s internal observability platform?**


The team provides Salesforce with an enterprise-grade metrics platform for monitoring, dashboards, and operational visibility. Argus ingests telemetry from services across the company and stores that data at a massive scale. This makes information queryable for teams that depend on fast, reliable insight.


That mission extends beyond storage because the platform allows for simple onboarding. Teams adopt Argus without complex integrations or heavy setup. Once they finish onboarding, these teams gain dashboards, alerting, and metrics analysis through a common internal platform.


The team also operates Argus as a continuously evolving product. As telemetry patterns shift and new workloads emerge, the architecture expands while maintaining reliability. **For thousands of internal users, Argus remains a primary source of metrics visibility across Salesforce services.**


##### **What scalability pressures emerge when ingesting and storing 4 billion metrics per minute, and how has Argus evolved to keep pace with that growth?**


The primary scalability demand stems from the fact that observability growth rarely slows. As more services launch and systems become more distributed, telemetry becomes more granular and metric volume rises. Argus now handles roughly 4 billion metrics per minute, which creates significant load across ingestion, storage, and query serving layers.


At that throughput, ingestion is only the first constraint. The platform must also store and retrieve data efficiently: Argus retains telemetry for 45 days, while a separate store ingests the same data in parallel for longer retention. On the query side, performance is helped by metadata caching. When queries contain wildcards, previously resolved metadata is cached so subsequent queries matching the same pattern skip resolution and stay fast. Without that cache, every wildcard query would pay the full metadata-resolution cost and query latency would rise.


To keep pace, the team continuously re-architects Argus rather than relying on a fixed design.[OpenTSDB](https://opentsdb.net/) and[HBase](https://hbase.apache.org/) serve as core storage layers while metadata caching keeps wildcard query resolution fast on the serving path. Horizontal scaling and targeted tuning across each layer ensure the system handles the volume because solving one limit usually means preparing for the next.


*A look at the previous architecture.*


##### **What reliability risks did Argus face operating in a single AWS region?how did that model risk complete unavailability during incidents?**


For years, the biggest reliability risk involved operating Argus in a single AWS region. While the team solved most internal software issues directly, a regional infrastructure outage created a dependency outside its control. If that AWS region failed, Argus visibility could suffer as a result.


Beyond availability, the single-region model also drove up data transfer cost and latency, since telemetry from all other FIs/DCs/regions had to be shipped into this one central FI/region for processing.


Argus powers dashboards and metrics across Salesforce, so teams rely on it during incidents to isolate failures and coordinate recovery. The risk was not a regional outage itself, rather, it was that one region could take down
visibility globally, which is precisely the blast radius geo-local was designed to contain.


A regional incident in that one region could render the entire platform unavailable — its blast radius extended across all of Argus at once, with no isolation between what failed and what remained available. Providing high availability by keeping multiple copies of Argus data across the regions was expensive at Argus scale because the system already consumed substantial AWS resources. Reducing that blast radius required a redesign that was both technically and economically viable.


##### **What architectural hurdles did the team face redesigning Argus into a multi-geo platform while controlling infrastructure cost?**


The transition to a multi-geo architecture required a major redesign rather than a simple deployment expansion. It was a six-month long effort that brought three teams together — Argus, Argus India, and the HBase team — spanning storage, and ingestion layers. The team required a model that improved resilience without making blast radius improvements too expensive.


Instead of full replication, Argus introduced a geo-local model that distributes data across multiple production geographies. Metrics undergo processing and storage closer to their origin, which reduces cross-region dependencies and improves fault isolation.


This shift required rethinking data placement, ingestion ownership, and regional responsibilities. The system also had to preserve existing functionality so internal dashboards and workflows continued without disruption. By distributing data intelligently rather than duplicating everything, the team created a practical path towards blast radius reduction while maintaining cost discipline.


This is now live across 4 of 5 production regions, with only the final region remaining, completing our production rollout.


*A look at the latest architecture.*


##### **What distributed systems complexities emerged when queries began spanning multiple geographies, especially around latency, partial results, and consistent user experience?**


When data spans multiple geographies, read paths become more complex than in a centralized model. A previous request succeeded or failed against one region. Now, some geographies may respond while another remains unavailable. This creates an operating model that requires the correct handling of partial data.


The team introduced a federation query layer to address this challenge. This layer routes requests to regional query services, retrieves local results, and aggregates them for the caller. To route intelligently rather than broadcasting every query to every geography, the federation layer maintains metric scope to region-level metadata from all geographies, giving it a global view of where data actually lives. Backed by[Elasticsearch](https://www.elastic.co/elasticsearch?utm_campaign=G-TXT-AMER-US-Core-EN-Lead_Gen-CloudTrials-BR&utm_content=Brand-Core&utm_source=google&utm_medium=cpc&device=c&utm_term=elasticsearch&utm_id=701610000005lJVAAY&gad_source=1&gad_campaignid=22934802687&gbraid=0AAAAADrDgoIXHO17JIqKWj0INdZEro1hc&gclid=CjwKCAjwvsvTBhBaEiwAmf-3nifVpgbOsQEjELqvVcElMpZzOktkPLpHSuoyUZN0XkaOWrPcPcbMyRoC44wQAvD_BwE) mappings, this lets the federation layer resolve a query to only the relevant geographies and skip the rest. This avoids unnecessary cross-region fan-out, reduces load, and cuts latency on every request.


Latency also presented a concern. Cross-region hops introduce delays for near-real-time workloads. The team optimized performance so users gained resiliency without facing unacceptable response times.


The user experience evolved to match these technical shifts. Instead of silent failures, the system introduced partial-response semantics and HTTP 206 responses. UI signals now inform users when a geography becomes unavailable. This combination of federation, transparency, and graceful degradation makes multi-geo observability practical.


##### **What engineering demands remain to further shrink blast radius as telemetry volume continues to grow?**


With geo-local now live in production across nearly all regions, the focus shifts from rollout to sustaining isolation as volume grows. As demand rises, isolation boundaries require constant maintenance to keep any single failure from widening its reach. Each year brings more services, distributed workloads, and telemetry sources that increase the load on ingestion and storage.


Recent AI adoption accelerates this trend. As AI workloads scale, existing instrumented code paths are exercised far more heavily, generating a much higher volume of signals. The observability platform must grow alongside the systems it monitors.


Keeping blast radius small requires continuous capacity planning and architecture reviews. The team identifies future bottlenecks in storage and network paths before users feel an impact.


This work requires disciplined operations. Multi-region systems reduce the blast radius of any single regional incident but introduce challenges around geo balancing and cross-region traffic costs. Sustaining fault isolation at this scale remains an ongoing engineering practice of constant adaptation.


##### **Learn more**


- Stay connected — join our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) !
- Check out our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to learn how you can get involved.
