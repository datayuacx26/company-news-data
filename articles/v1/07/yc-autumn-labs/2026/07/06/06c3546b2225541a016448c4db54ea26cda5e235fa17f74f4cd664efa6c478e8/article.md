---
schema_version: "1.0.0"
document_id: "06c3546b2225541a016448c4db54ea26cda5e235fa17f74f4cd664efa6c478e8"
company_key: "yc-autumn-labs"
company: "Autumn Labs"
source_id: "yc-autumn-labs-news-import-b84a0821a8a2"
canonical_url: "https://autumnlabs.io/blog/manufacturing-telemetry"
published_at: null
first_seen_at: "2026-07-22T23:46:11.218193+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:8d2ad23acad1fdc335e2e1b3ebac923d070423107ce4cb59b52fab2798902e4f"
---

# Manufacturing Telemetry

Collecting data inside a factory is only half the battle.
The harder problem is **getting that data out** reliably, securely, and without disrupting production.


Factories are not cloud-native environments. They are constrained by legacy networks, strict IT security policies, segmented ownership models, and operational requirements that tolerate zero downtime. Designing a telemetry pipeline that works here requires a very different approach than traditional SaaS ingestion.


This post dives into the technical realities of how manufacturing data leaves the factory floor and why this layer is often the most complex part of manufacturing observability. When it's done right, data flows invisibly. When it's done wrong, nothing moves at all.


---


## Telemetry Sources on the Factory Floor


Manufacturing telemetry can originate from many different places:


- Station PCs running test or assembly software
- PLCs and industrial controllers
- Vision systems and sensors
- Manual operator inputs
- External equipment provided by vendors


Each source differs in protocols, data rates, reliability guarantees, and ownership. Some systems stream continuously; others emit events sporadically or only support batch exports.


A telemetry pipeline must ingest heterogeneous data without assuming control over the system producing it.


💡 Insight


In most factories you don't get to redesign the data source, only observe it.


---


## Network Architectures in Manufacturing


Factory networks are intentionally restrictive. Unlike cloud environments, connectivity is designed around safety, uptime, and risk reduction — not convenience. The way computers, PLCs, robots, test stands, and servers are interconnected varies widely by plant. Common architectures include the following.


### Flat, Firewall-ed Networks


Older factories may operate large flat LANs where most devices — PLCs, HMIs, industrial PCs, cameras, and test stations — sit on the same shared network. These environments are typically protected by a strict perimeter firewall, with outbound traffic limited and requiring explicit IT approval per destination.
*Analogy: like one big open factory floor where every machine can shout to every other machine, but there’s a single guarded gate controlling anything that leaves the building.*


### Segmented or Zoned Networks


Modern plants typically separate networks into zones to isolate critical equipment and reduce blast radius:


- Production zones (robots, PLCs, line controllers)
- Quality or test zones (vision systems, measurement rigs)
- Corporate IT zones (ERP, dashboards, employee devices)


Traffic between zones may be filtered, proxied, or fully blocked, meaning systems in one zone cannot automatically reach another without deliberate network rules.
*Analogy: like a facility split into secured rooms — devices may communicate freely within a room, but crossing into another requires passing through locked doors and checkpoints.*


### Air-Gapped and Semi–Air-Gapped Environments


Some factories are fully air-gapped, where production computers and control networks have no connection to corporate IT or the internet at all. Others allow outbound-only connectivity (for telemetry or updates) while strictly forbidding inbound access. In these setups, moving data often requires controlled gateways or even manual transfer.
*Analogy: like a factory with no roads leading in — machines can operate internally, but sharing information externally requires physically carrying it out rather than sending it over the network.*


⚠️ Reality Check


"Just open a port" is rarely an option in a factory.


---


## Security Considerations


Security is non-negotiable in manufacturing environments and differs significantly from typical SaaS assumptions. Unlike cloud-first systems, factory networks are designed to prevent disruption above all else — a single breach or misconfiguration can impact an entire production line, halt operations, or compromise sensitive intellectual property.


As a result, plants adopt strict controls that align with the network architectures described above — from firewalled perimeters, to segmented zones, to fully air-gapped environments.


### Outbound-Only Communication


Factories overwhelmingly prefer outbound-only connectivity to reduce attack surface. Rather than allowing external systems to “reach into” the plant, devices inside the factory may be permitted to push telemetry outward through tightly controlled egress paths. Inbound access is often prohibited entirely.


### Identity and Authentication


Persistent, long-lived credentials on station PCs are frequently disallowed. Authentication must be designed for constrained industrial environments, with access that is:


- Rotatable
- Minimally scoped
- Auditable


This ensures compromised endpoints cannot silently retain broad or permanent permissions.


### Data Sensitivity


Manufacturing telemetry can include:


- Proprietary process parameters
- Yield and throughput metrics
- Customer or product identifiers


Encryption in transit is mandatory, and encryption at rest is often required even on local systems.


---


## Latency, Bandwidth, and Reliability Constraints


Manufacturing telemetry pipelines must tolerate:


- Unreliable connectivity (maintenance windows, switch reboots, equipment changes)
- Low-bandwidth site links
- Strict latency requirements for real-time visibility
- Backpressure when downstream systems slow down


Dropping data is not an option and when that dropped data blocks production software, it's just unacceptable.


This requires careful handling of:


- Local buffering
- Backpressure-aware ingestion mechanisms
- Retry and replay semantics
- Graceful degradation under network stress


---


## Telemetry Pipeline Design Patterns


A common and effective approach is **local collection with controlled egress** :


1. Local agents collect and normalize data close to the source
2. Data is buffered locally to absorb network instability
3. A single controlled egress path sends data out of the factory
4. Downstream systems handle aggregation, storage, and analytics


This design minimizes operational risk while preserving real-time observability.


#### Single Egress Beats Many Exceptions


From an operational standpoint, firewall management is often the biggest blocker to deployment. So creating multiple exception for data to leave the firewall is a pain to manage and thats why Factory IT teams strongly prefer:


- A single destination
- A single IP address
- A single protocol


Every additional endpoint increases review time, risk, and friction.


#### On-Prem, Hybrid, and Cloud Deployments


Sometimes the simplest way to work within these restrictions is to keep data inside the factory entirely. Instead of sending telemetry to a remote cloud server, systems can aggregate and store data locally on on-prem infrastructure. This allows factory personnel to access analytics and reporting without requiring external connectivity.


Common deployment models include:


- Fully on-prem (local aggregation and storage)
- Hybrid (local collection with remote analytics)
- Cloud-native (for less restricted environments)


A production-grade observability system must support all three without requiring architectural rewrites.


---


## Why is this telemetry layer so hard?


Telemetry pipelines in manufacturing sit at the intersection of:


- OT systems
- IT security policies
- Vendor-controlled software
- Real-time operational requirements


As a result, there are many potential points of failure that can impact data reliability and fidelity. In rare cases, telemetry issues don’t just affect visibility — they can disrupt production software itself and even cause an entire line to halt.


---


## How Autumn Labs Handles This


Autumn Labs was designed around these constraints from the start. We quickly learned that manufacturing observability lives or dies at the crossroads of data collection, network security, and operational reliability.


That’s why, out of the box, we support:


- Station PC ingestion with local buffering
- Flexible deployment across cloud, hybrid, or fully on-prem environments
- Single-IP egress through regional static proxies
- Outbound-only secure tunnels aligned with factory security models
- Strictly scoped credentials and encrypted transport


All of this is built to minimize burden on factory IT teams while preserving real-time visibility.


---


Want to see how this works in your environment? Request access to explore Autumn Labs.
