---
schema_version: "1.0.0"
document_id: "06391cce3c31c9e19b552ba30b882d1dd5c75b1c748c4ab9b474682a6dcd2087"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/modernize-your-historian-6-signs/"
published_at: "2026-06-01T08:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:79b81ff4689399ddf48ddd5f0c924e17a7135009b8d317fd9a9b0d1f0d7f6b0a"
---

# 6 Signs Your Historian Renewal Should Be a Modernization Conversation

Table of Contents


Renewal notices don’t arrive with a subject line that says “modernization conversation enclosed.” They show up as a line item in procurement, usually with a larger number than the previous year. Most teams sign and move on.


The ones that pause tend to find the renewal moment is a useful forcing function, not to rip out the historian, but to ask whether the current architecture can support what the business needs over the next three years.


If any of the following sounds familiar, that audit is worth running before you sign.


## Sign 1: Your renewal quote is higher, and so is your tag count


The math used to work. When the historian was first deployed, per-tag pricing was predictable. You knew the count, you knew the cost, and the line item made sense against the operational value it delivered. However, that equation breaks down as instrumentation expands.


For example, AVEVA PI customers are seeing renewal pressure even as their tag counts increase. Some organizations have seen AVEVA pricing increase 20-40% over the last 18 months. That creates the wrong incentive: collect less data, sample less often, or pay more for the visibility the business now needs.


There’s a compounding factor that often goes unexamined. Deprecated tags, from decommissioned equipment, retired sensors, or replaced instrumentation, still count toward your license. The pricing model doesn’t distinguish between a tag that’s actively collecting at 1Hz and one that’s been dark for two years. In industries where equipment turns over regularly, paying full price for data you no longer collect is an inevitability. The tag footprint grows in both directions: new assets add tags at the front, old ones don’t shed them at the back.


If your procurement team is asking why the historian line item keeps growing while your OT team can’t fully instrument what it needs to, the commercial model has already stopped working for you.


## Sign 2: Your data science team is still waiting for that export


This appears consistently across industrial organizations. Analytics and AI initiatives get funded, data scientists are hired, and then they spend the first few months writing tickets to the OT team asking for historian exports.


- PI Vision and PI DataLink weren’t built for Python, pandas, or Databricks.
- Getting data out requires proprietary connectors, manual exports, or PI Web API integrations that take weeks to stand up.
- By the time the data reaches the ML pipeline, it’s stale, downsampled, or stripped of the context it needs to be useful.
- The data science team that was supposed to be building predictive maintenance models is still, months later, doing data plumbing.


There’s a structural issue behind the export problem. Historians were built to store industrial tags, usually SCADA or PLC-process information, mapped to an ISA-95 hierarchy. What they weren’t designed to do is store metadata and context alongside raw industrial signals. That gap matters more as analytics mature.


Enriching SCADA data with MES data about production shifts, ERP data about batches and stock levels, or equipment metadata for grouping by manufacturing type requires a platform that treats context as a first-class citizen alongside telemetry. Without it, even a well-delivered data export produces a one-dimensional analysis. You see what happened, but you don’t understand why.


If “how do we give the analytics team access to OT data” is still an open question, you’re looking at a historian problem.


## Sign 3: A new workload just got bounced back to you


Someone in the business asked you to support something the historian wasn’t built for:


- Cell-level monitoring for a new BESS installation at 1Hz, when your historian’s default polling interval is 5-15 minutes.
- Vibration analysis on rotating equipment, where sub-second resolution is required, and tag economics become punishing fast.
- A new greenfield facility where the capital plan assumes modern, cloud-connected data infrastructure.
- A real-time anomaly detection model that needs data before the dashboard refresh cycle catches it.


When your answer has to be “the historian can’t do that at that frequency” or “it can, but here’s what the additional tags will cost,” the conversation has changed. The workloads the business needs have outpaced what the platform was sized for.


The throughput problem has a storage dimension too, and it’s the one that tends to catch teams off-guard. Historians weren’t built for indefinite storage of high-volume, high-frequency data. Industry 4.0 use cases, including predictive maintenance models, asset warranty tracking, and long-range efficiency analysis, need data stored over years, not rolling windows. When you try to do that in a historian, storage costs compound quickly, and data accessibility often degrades. The business case for those use cases depends on querying three years of sensor data quickly. A historian scoped for operational visibility doesn’t make that easy.


## Sign 4: Every plant is an island


Your historian keeps the plant running. It doesn’t know the fleet exists.


- Cross-site benchmarking requires custom middleware that someone has to build, maintain, and eventually explain to their replacement.
- Fleet-level analysis comparing performance across sites means extracting and normalizing data from multiple historian instances with inconsistent tag naming.
- Centralized reporting for leadership involves someone in OT running manual aggregation every Monday.
- Each new site gets the same plant-centric architecture, compounding the cross-site problem with every expansion.


The plant-island problem goes deeper than data access. Each site typically operates by its own data standard, with tag naming conventions, unit definitions, and hierarchy structures that made sense locally but weren’t designed for comparison. Cross-plant analysis requires a standardization layer built and maintained on top of the historian, and that layer tends to be fragile, undocumented, and owned by whoever happened to build it.


There’s also an architectural concern that digital transformation teams are raising with increasing frequency: reaching back into plant networks to pull data from individual historian instances creates bandwidth and security risk. The better pattern is a single, standardized interface at the enterprise layer, one that aggregates and normalizes across sites without requiring anyone to access operational technology networks to run a comparison report.


## Sign 5: The platform is a closed system, and that’s becoming a problem


Historians accumulate dependencies as much as they accumulate data. The proprietary architecture that made them reliable for single-site operational visibility also makes them difficult to extend, integrate, or evolve.


- Integrations are built through proprietary SDKs and connectors. When a vendor discontinues one, the integration breaks.
- The platform’s data model isn’t designed to connect with open standards like SQL, JDBC, or ODBC without significant middleware investment.
- Every new capability, whether a new visualization layer, analytics integration, or edge deployment pattern, requires either a vendor-supplied solution or a custom workaround.
- Legacy .NET dependencies constrain where and how the platform can be deployed, and the team managing it today is working from a roadmap they don’t control.


This is where vendor lock-in stops being an abstract concern and becomes an operational one. Every renewal locks in the architecture for another cycle. Teams that don’t own their data layer, can’t control their integration strategy, and can’t switch tools without rebuilding from scratch are paying for a constraint, not just a platform.


Open standards, SQL, JDBC, ODBC, and common client libraries, exist precisely to avoid this. They let teams retain skills, use the right tool for the right job, and avoid designing in a vendor dependency that becomes painful at exactly the moment of renewal.


## Sign 6: “Historian limitations” shows up in every initiative’s risk log


This is the one that tends to crystallize things for digital transformation and operations leaders. At some point, the pattern becomes visible:


- BESS monitoring is blocked by polling intervals.
- The ML pipeline is blocked by proprietary data access.
- Cloud migration is complicated by the on-prem historian footprint.
- Edge data collection for remote assets requires custom workarounds.
- Cross-site analytics is deferred indefinitely, pending a middleware project that never gets resourced.


When the historian appears in the constraints column on enough project charters, teams stop treating it as a given and start treating it as something to engineer around. That shift in framing is usually the start of a different conversation.


## What to do about it, before signing a contract


None of this necessarily means replacing the historian. Most industrial teams that modernize their data infrastructure augment their deployment rather than pursuing an immediate rip-and-replace.


The pattern that has worked for organizations like Terega and SP Energy Networks is to deploy InfluxDB alongside AVEVA PI. In those deployments, the historian handles legacy SCADA historization, GxP data, and established PI Vision workflows, while InfluxDB takes on high-frequency BESS monitoring, predictive maintenance pipelines, new greenfield sites, and cross-site fleet analytics.


- Compute-based pricing means unlimited tags, with no per-sensor cost penalty.
- Native SQL, 14 client libraries, and direct Grafana, Power BI, and Python integration with no proprietary SDK.
- InfluxDB 3 Core runs at the edge; Enterprise or Cloud serves as the central hub, with one SQL interface across both.


For organizations facing a significant renewal, planning a new facility, or actively evaluating a full exit from the historian architecture, the path is a complete replacement. This includes InfluxDB for time series storage, and Litmus Edge or Highbyte for industrial connectivity and asset modeling.


Terega, the French gas network operator, deployed InfluxDB alongside their historian when they were heading toward seven-figure annual OSI PI costs. They now collect 20x more data at 50% lower cost. SP Energy Networks became the first UK utility to receive Ofgem approval for a cloud-hosted historian, running on InfluxDB. That decision has since opened the door for other regulated UK operators to evaluate the same path.


The renewal notice is a useful moment. Use it to ask whether the current architecture can carry what the business needs from it next, and what it would take to find out.


\[[Get InfluxDB free](https://www.influxdata.com/products/influxdb/?utm_source=website&utm_medium=direct&utm_campaign=modernize_your_historian_6_signs&utm_content=blog&utm_content=blog) \] InfluxDB 3 Core is open source and available now. Deploy at the edge in minutes. No per-tag pricing. Native SQL. Direct Grafana and Power BI integration.
