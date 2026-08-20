---
schema_version: "1.0.0"
document_id: "f01b342517ab0b8c9a766fb2c5c1c726a287f830f8fa282c13da5c965cdb11d5"
company_key: "jabil-inc-common-stock"
company: "Jabil Inc."
source_id: "jabil-inc-common-stock-news-import-e505bb03b417"
canonical_url: "https://jabil.com/blog/liquid-cooling-higher-density-racks.html"
published_at: null
first_seen_at: "2026-07-22T00:41:31.165126+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:120979a1247111115926ae226b1ba346f0b76a3ad46ca7d5dad7db9daa3f8497"
---

# Scaling Up: Liquid Cooling for Higher Density Racks

## Where Liquid Cooling Is and Where It's Going


Three liquid cooling approaches are currently in active deployment and will continue to evolve as data center cooling products mature.


### 1. Single-phase DLC


In single-phase direct liquid cooling, the coolant circulates through cold plates mounted directly to high-power chips and switches, absorbing heat from these electronics and carrying it to a coolant distribution unit (CDU). These closed-loop systems recycle coolant continuously. The coolant remains liquid throughout the process.


Single-phase DLC is compatible with brownfield and next-gen data center environments because installation is the least complicated of the three approaches. It can be highly efficient, with a few vendors, like[Mikros Technologies](https://www.mikrostechnologies.com/) , a Jabil company, capable of cooling chips up to 5,000+ W in single-phase applications. For the vast majority of current and near-term chip TDPs (thermal design power), single-phase DLC provides sufficient thermal headroom without the infrastructure complexity of alternative approaches.


### 2. Two-phase DLC


Similarly, two-phase cooling uses a cold plate, often called an evaporator, that is mounted directly to electronics. The liquid coolant flows into the cold plate, absorbing heat and boiling it into a vapor that is then cooled in a condenser and circulated. In these closed-loop systems, the coolant goes through two phases, changing from liquid to gas and then back into a liquid. The phase change absorbs more energy per unit of coolant when compared to most single-phase solutions.


In practice, the thermodynamic advantages become operationally relevant at thermal densities beyond what current chip and next-gen architectures produce. Two-phase infrastructure is more complex and costly, and the engineering maturity gap with single-phase remains significant. As chip TDPs push past current thresholds, two-phase will have a role; for today's deployments, it's a future consideration for most organizations rather than a present requirement.


### 3. Immersion


Immersion cooling remains niche, accounting for a minority of installs. It involves submerging entire racks, including chips, in non-conductive fluid that draws heat from all components simultaneously. But the operational challenges with this setup are significant. Material compatibility, hardware maintenance procedures, fluid management, added safety protocols, staff training, and risk management all come into play with immersion cooling. Dielectric fluid chemistry isn't fully standardized, and the approach often limits users to a single-vendor commitment.


For greenfield builds with specific workload profiles, immersion is worth evaluating, and some operators are pairing it with direct-to-chip cold plates in[hybrid configurations](https://www.mikrostechnologies.com/learn/trends-and-insights/direct-to-chip-cold-plates-and-hybrid-immersion-systems-for-data-center-liquid-cooling.html) that improve bath efficiency and localized thermal management. It remains a specialized path rather than a default for most.


Liquid-to-liquid cooling systems are becoming the standard for new high-density builds because they can handle rack densities exceeding 100 kW while significantly reducing environmental impact. These solutions address the growing "water-energy nexus" by enabling closed-loop heat rejection that can virtually eliminate the massive water consumption associated with traditional evaporative cooling towers.


Picking the right approach solves the thermal problem. Done well, it's also a pivotal step toward PUE improvements and sustainability wins.
