---
schema_version: "1.0.0"
document_id: "bdc11cd65cc93cead9c6d87b4b7335c99dcb7d00126f7b774e2f530f797f9888"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/22/advanced-bgp-design-for-multi-zone-and-hybrid-vpc-deployments/"
published_at: "2026-05-22T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:80ffd594f6fa5570bd135bcfd69ab1630ac3ac689ce2b2166b92d7edab286098"
---

# Advanced BGP Design for Multi Zone and Hybrid VPC Deployments

## Introduction


Modern enterprise networks are evolving rapidly, driven by hybrid cloud adoption, multi-zone architectures, and help drive more seamless connectivity between on-premises and cloud environments. At the heart of this transformation is BGP (Border Gateway Protocol), a proven, standards-based routing protocol that enables dynamic, scalable, and policy-driven connectivity.


In Nutanix Flow Virtual Networking (FVN), Virtual Private Clouds (VPCs) provide secure, isolated network segments for applications. But as deployments grow across multiple zones, disaster recovery sites, and hybrid environments, basic routing isn’t enough. Enterprises need advanced capabilities to:


- Control traffic flows intelligently across active and standby sites.
- Segment routing policies for different zones (e.g., corporate vs. DMZ).
- Scale out without multiplying BGP sessions, while maintaining resilience.


This blog introduces best practices for optimizing VPC connectivity with BGP in hybrid and multi-zone deployments. We’ll explore advanced features, such as AS‑PATH Prepend, Externally Routable Prefix (ERP) Filtering, and Add‑Path, and show how they solve real-world challenges like disaster recovery, zone-specific routing, and scale-out architectures.


This article is based on AOS 7.5 and the following component versions.


- AOS 7.5
- Prism Central (pc.7.5)
- AHV 11.0
- Network Controller 7.0
- Flow Controller 7.5
- Network Gateway 7.1.


## Quick refresher: Why these capabilities matter


- Traffic engineering: optimize inbound paths to your services.
- Granular policy per neighbor: advertise only what a peer should see.
- Fast convergence & visibility: multiple path advertisement improves resilience.


## 1) ERP Filtering per BGP Session


### What it is


ERP (Externally Routable Prefix) lists a group of network prefixes. Session‑scoped ERP filtering lets you select which ERP to advertise per neighbor, so each BGP session gets exactly the subset of routes intended for it.


### When to use


- Different peers should see different slices of your address space.
- You want consistent route tagging/prepend across many sessions via reusable ERP lists.
- Zone‑aware exports (e.g., corporate vs. DMZ).


### How to configure (Prism Central)


1. Create ERP filtering:


1. *Network & Security → BGP Sessions → Select a BGP Session → Update*
2. *Session Configuration → Advertised Externally Routable Prefixes*
3. You have two options:


1. *Advertise All ERPs in VPC* : which advertises all the ERPs created at the VPC level
2. *Stack a Custom list of ERPs* : which advertises the list of ERPs that you provide at the session level.


2. Observe:


1. *Network & Security → BGP Sessions → Open the BGP Session*
2. *Routes → Advertised → List of Advertised Routes*


ERP Filtering to Control Route Advertisement per Zone (Corporate vs DMZ)


### Real‑world use case: Corporate vs. DMZ zone exports


#### Scenario


A single VPC attaches to two zones: Corporate and DMZ. The enterprise wants internal apps and service networks reachable from Corporate, but only limited, hardened prefixes reachable from DMZ.


#### Policy


- ` erp-corporate` , full application ranges:` 192.168.1.0/24` and` 172.16.10.0/24`
- ` erp-dmz` , restricted and public‑facing IPs:` 172.16.10.0/24`


#### Outcome


Different routers receive different routes on the session they establish with the VPC. ERP delivers clean separation without duplicating configurations across sessions.


## 2) AS‑PATH Prepend


### What it is


` AS‑PATH` prepend repeats your ASN (Autonomous System Number) in the` AS_PATH` attribute you advertise, making a route less preferred to most external peers, who commonly select the shortest AS path. It’s a straightforward lever for inbound traffic steering.


### When to use


- In a multi-site / multi-AZ deployment, prefer Site-A for general ingress, Site-B as backup.
- Bias traffic away from lower‑capacity or maintenance links.
- DR (Active/Standby) designs across two AZs/sites (deep dive below).


We recommend avoiding excessive prepending and using communities instead, when supported by partners.


### Disaster Recovery (Active/Standby) Use Case


#### Scenario


A customer operates two VPCs in separate Availability Zones (AZs/sites):


- ` AZ‑1` / Site-A (Active) hosts production workloads.
- ` AZ‑2` / Site-B (Standby) is the DR site, ready to take over if` AZ‑1` fails.


Both VPCs advertise the same application prefixes to the enterprise network or external peers via BGP. The goal is to ensure inbound traffic prefers the Active site under normal conditions but automatically shifts to the Standby site during failover without manual intervention.


#### Solution


Use AS‑PATH Prepend on the Standby VPC’s BGP advertisements:


- Active VPC (` VPC-1` ): Advertises prefixes with normal (short) AS‑PATH.
- Standby VPC (` VPC-2` ): Advertises the same prefixes but prepends its ASN multiple times (e.g., 2×). This makes its path appear “longer” and less preferred by upstream routers.


When` AZ‑1` is healthy, inbound traffic flows to VPC-Active in` AZ-1` because its` AS‑PATH` is shorter. If` AZ‑1` fails and stops advertising, Standby routes remain, and traffic converges to AZ‑2 automatically. Failover typically happens within seconds depending on BGP timers and upstream policies.


AS Path Prepend for Active and Standby VPC Traffic Preference


#### Configuration Blueprint (Prism Central)


1. Create ERP Lists


1. ` erp-active` : Advertise prod prefixes with no prepend from VPC-active (` AZ-1` ).
2. ` erp-standby` : Advertise the same prefixes with` AS‑PATH` Prepend x2 from VPC-standby (` AZ-2` ).


2. Bind ERP Lists to Sessions


1. VPC-Active → BGP session to ToR1 → leave the “AS Path Prepend” field empty.
2. VPC-Standby → BGP session to ToR2 → set the “AS Path Prepend” to 65001.


3. Validate on upstream router that both sites’ routes are present, and the Active path is selected when available; simulate failover to verify automatic switchover.


**Example Peer View Route Table**


```text
Network             Next Hop         AS Path
*> 192.168.1.0/24      192.0.2.1        65001
*  192.168.1.0/24      192.0.2.2        65001 65001
```


#### Best practices


- Start with 2–3 prepends; avoid excessive values (>5) unless justified.
- Monitor impact if upstream applies LOCAL_PREF overrides, prepend may be ignored.


## 3) Add‑Path: Multiple Paths via a Single Session


### What it is


Add‑Path (RFC 7911) extends BGP so a router can advertise multiple distinct paths for the same prefix within one session. Paths are differentiated by a Path Identifier. Benefits include ECMP, BGP PIC (fast reroute), reduced path hunting and oscillation, and better visibility of alternate exits.


### When to use


- Your VPC is deployed in scaled-out mode (e.g., multiple VPC router IPs) and downstream devices should see more than one.
- You need fast failover without waiting for re‑advertisement.


### How to configure Prism Central


1. Both peers must advertise and negotiate capability code 69 for each required address family (AFI) and subsequent address family (SAFI), such as IPv4 unicast.
2. Enable Add‑Path on a BGP GW:


1. *Network & Security → Connectivity → Gateways → Create → Local*
2. *Name and required information → Next → Gateway Service BGP → Under BGP Path Advertisement Configuration Check “Advertise multiple routes toward VPC destinations using BGP Additional Paths”*


3. Create a BGP Session with a BGP peer and observe:


1. *Network & Security → Connectivity → BGP Session → Open the BGP Session*
2. *Routes → Advertised → Routes to the same ERP are advertised with several next hops*


### Real‑world use case: Scale‑out VPC with 4 gateways, single session, ECMP on ToR


#### Scenario


A VPC is deployed in scaled‑out mode with 4 gateway nodes and IPs. The design goal is to simplify the management with fewer sessions and higher scale and resilience.


#### Policy & Operation


The VPC establishes a single BGP session to the Top‑of‑Rack (ToR) router.


With Add‑Path enabled on the BGP GW, the VPC’s BGP GW advertises the same application prefixes with 4 distinct next‑hops (the 4 VPC Router IPs) through that one session.


The ToR installs an ECMP route with all 4 next‑hops, distributing inbound traffic and enabling rapid convergence if one gateway is drained or fails.


Additional Path Advertisement of Multiple Next Hops over a Single BGP Session


##### Additional Path Advertisement of Multiple Next Hops over a Single BGP Session


#### Outcome


The desired result is to reduce session sprawl with one session instead of four, and the ToR maintains full path diversity for load‑sharing and failover. BGP additional path’s ability to expose multiple equal‑cost exits through a single BGP adjacency is exactly what we leverage to help improve management simplicity, scale, and availability. The behavior and benefits of Add‑Path and ECMP are documented in the standard and vendor‑neutral analyses.


## End‑to‑end configuration walkthrough


**Scenario:** Two VPCs (VPC-A, VPC-B) and two enterprise routers (Corporate and DMZ). Goals: prefer VPC-A, keep VPC-B as backup; expose multiple next‑hops with Add‑Path; implement DR Active/Standby with prepend; zone‑aware ERP (Corporate vs. DMZ).


End to End BGP Design for Multi Zone VPC with DR and Zone Aware Routing


#### Create a VPC and define ERPs


Prism Central → Network & Security → Virtual Private Clouds → Create VPC. Associate external subnets (No‑NAT and NAT as needed) and set the Number of Active Hosts to 2.


#### Add your ERPs under VPC Configuration


VPC → Externally Routable IP Prefixes. Add the list of ERPs respecting VPC subnet addresses.


#### Deploy Local and Remote BGP Gateways


Prism Central → Connectivity → Gateways → Create → Local. Create a new local gateway of type BGP and allocate an IP Address and an eBGP ASN.


Prism Central → Connectivity → Gateways → Create → Remote. Create a remote BGP peer for the corporate environment edge router, and a remote BGP peer for the DMZ environment by providing their respective peer service IP and eBGP ASNs.


#### Create BGP Sessions


Create four BGP sessions, two per Availability Zone, keeping routing policy explicit and predictable.


##### AZ-1 (Active site)


- VPC-Active to Corporate ToR


- Local Gateway: VPC-Active (` AZ-1` )
- Remote Gateway: Corporate ToR
- ERPs: erp-corporate
(` 192.168.1.0/24` ,` 172.16.10.0/24` )
- AS Path Prepend: leave empty


- VPC-Active to DMZ ToR


- Local Gateway: VPC-Active (` AZ-1` )
- Remote Gateway: DMZ ToR
- ERPs: erp-dmz
(` 172.16.10.0/24` only)
- AS Path Prepend: leave empty


##### AZ-2 (Standby site)


- VPC-Standby to Corporate ToR


- Local Gateway: VPC-Standby (` AZ-2` )
- Remote Gateway: Corporate ToR
- ERPs: erp-corporate
(` 192.168.1.0/24` ,` 172.16.10.0/24` )
- AS Path Prepend: set to` 65001`


- VPC-Standby to DMZ ToR


- Local Gateway: VPC-Standby (` AZ-2` )
- Remote Gateway: DMZ ToR
- ERP Filtering: erp-dmz
(` 172.16.10.0/24` only)


## Further reading


- [Exploring BGP Routing Inside Nutanix FVN VPC (background)](https://www.nutanix.dev/2023/08/31/exploring-bgp-routing-inside-nutanix-flow-virtual-networking-fvn-vpc/)
- [Nutanix Bible (BGP Gateway Deployment and Operations)](https://www.nutanixbible.com/12c-book-of-network-services-flow-virtual-networking.html)
- [RFC-7911: Add‑Path overview and operations](https://datatracker.ietf.org/doc/html/rfc7911)


## Conclusion


In this article, we explored how advanced BGP capabilities in Nutanix Flow Virtual Networking can address real-world enterprise networking challenges across hybrid and multi-zone VPC deployments. ERP Filtering enables precise route advertisement per zone, AS-PATH Prepend provides a simple and effective way to steer traffic between active and standby sites, and Add-Path improves scale, resilience, and operational simplicity in scale-out designs. Together, these capabilities help build VPC connectivity that is more predictable, flexible, and aligned with enterprise requirements for segmentation, disaster recovery, and growth.
