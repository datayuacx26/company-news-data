---
schema_version: "1.0.0"
document_id: "835d8cdebb31d2f1011f2541a860340489306e7c8a9deb6ee29c9592a0c05f2b"
company_key: "godaddy-inc-class-a-common-stock"
company: "GoDaddy Inc."
source_id: "godaddy-inc-class-a-common-stock-news-import-cf537cccbea7"
canonical_url: "https://www.godaddy.com/resources/news/network-flow-lens-provides-real-time-network-diagnostics-at-your-fingertips"
published_at: "2026-08-04T10:30:00+00:00"
first_seen_at: "2026-08-05T02:53:00.904605+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:0e53a859842e3ea4f64b11f4b05b61f8f7995b1574c81da146f41156c5a8e614"
---

# Network Flow Lens Provides Real-Time Network Diagnostics at Your Fingertips

## The network troubleshooting problem


If you've ever deployed a new service to production and watched it fail to connect to another service, you know how the next hour goes. The containers are running, health checks pass, logs show a clean startup. But traffic between Service A and Service B never arrives.


Is it the application? The compute layer? The network?


For teams running on-premises infrastructure, that last question is the hardest to answer. Cloud environments give you security groups and VPC flow logs a click away. On-premises networks are opaque to the developers building on top of them.


The traditional troubleshooting process tends to follow a predictable pattern:


- Initial diagnosis (30–60 minutes) to isolate the problem domain
- Engage the network team (variable wait time) by submitting a support ticket
- Network engineer investigation (20–60 minutes) with manual inspection of devices
- Firewall policy check (10–30 minutes) reviewing rules across firewalls from several vendors
- Interface diagnostics (30–90 minutes) checking for errors on potentially hundreds of interfaces
- Resolution, where you finally identify root cause and implement a fix


Total cycle time runs two to four hours at a minimum, often spanning several people and systems. That's when things go smoothly.


## Network Flow Lens


GoDaddy operates significant on-premises infrastructure. We maintain direct control over performance, security, and cost at the scale required for millions of domains and websites. This infrastructure spans dozens of global sites with thousands of network devices, and that scale is exactly what makes self-service diagnostics essential.


Network Flow Lens (NFL) is a self-service network diagnostics tool that gives developers and operators direct visibility into this infrastructure. With just source and destination IP addresses, NFL can:


- Check if traffic is allowed through firewalls and ACLs across multiple vendor platforms
- Identify actively incrementing interface errors along the network path
- Map the actual route traffic takes through infrastructure using live routing table and LLDP (Link Layer Discovery Protocol) analysis
- Query the live production network rather than cached state or a CMDB snapshot


What previously took network engineers 20–60 minutes now takes NFL less than one minute.


NFL started as a solo project and grew as other engineers contributed part-time. Within three months, we had our first users running it against production infrastructure. A typical flow check traverses 3–8 devices; a path diagnostic may analyze 40+ interfaces across an 8-device path.


## What NFL does


NFL provides two primary diagnostic capabilities. The following sections walk through each one.


### Flow Check


Flow Check answers the fundamental connectivity question: given a source IP, destination IP, port, and protocol, is this traffic permitted by every device in the network path?


The analysis resolves source and destination security zones via live routing table queries, identifies all firewalls and routers in the path, and executes vendor-specific policy match commands against live device state. Depending on the vendor, this means security policy lookups with zone-to-zone matching, test commands with protocol number translation, or advanced routing support. NFL also checks router ACLs for explicit deny entries and detects same-zone scenarios where no firewall traversal occurs. The result is a definitive permit/deny verdict with full policy details.


Common use cases include pre-deployment connectivity validation, connectivity troubleshooting, security posture verification, and change impact assessment.


### Path Diagnostics


Path Diagnostics goes beyond connectivity to identify active performance issues along the network path.


The analysis maps the complete network path using LLDP neighbor discovery, then takes a baseline snapshot of interface error counters on every device in the path. After waiting 10 seconds for counters to increment, it takes a second snapshot and calculates the delta. Only interfaces where errors are actively increasing get reported. The tool tracks vendor-specific counters including input/output errors, discards, CRC errors, and interface flaps. It also renders an interactive network topology diagram showing the full path.


Common use cases include performance troubleshooting, intermittent connectivity investigation, packet loss diagnosis, and post-change validation.


## The technical architecture


Under the hood, NFL combines live device querying, a multi-vendor abstraction layer, real-time streaming, and role-based access control. The following sections describe each component.


### Live network analysis


Unlike many network tools that work from cached topology data or CMDB exports, NFL queries the actual production network in near-real-time. Every flow check and path diagnostic runs live commands against real devices.


NFL communicates with network devices through a centralized internal command execution platform, a common pattern in large-scale network automation. This platform abstracts away the complexity of SSH sessions, credential management, and device connection pooling across thousands of devices. NFL sends batch API requests specifying which devices to query and which commands to run, and the platform executes them safely.


This architecture means results reflect current network state (including changes made seconds ago), rate limiting and retry logic are handled at the infrastructure layer, every command executed is logged through a central system, and NFL never holds device credentials or opens direct SSH sessions.


The system has four layers:


- NFL Web Interface (React). The user provides source IP, destination IP, port, and protocol. Real-time SSE log streaming displays analysis progress.
- NFL Backend (Flask). Receives user input and orchestrates the analysis through five-phase path discovery, multi-vendor device coordination, zone-based policy analysis, and two-snapshot error detection.
- Device Command Execution Platform. Our internal centralized automation platform receives batch API calls from the NFL backend and executes commands safely against devices with credential management, connection pooling, and audit logging.
- Network Devices. The execution platform communicates with production devices via SSH/CLI, including firewalls, routers, and switches from multiple vendors.


### The five-phase path discovery algorithm


Discovering the actual network path between two IPs is the hardest problem NFL solves. Unlike traceroute (which shows L3 hops but misses firewalls and switches), NFL reconstructs the full Layer 2/3 path by querying device routing tables and LLDP neighbor information.


Path resolution breaks into five phases, source to destination.


**Phase 1** (Site Distribution Router Query). NFL identifies the site distribution-layer routers at both the source and destination, the first L3 hop within each site's fabric. It then queries their routing tables to find initial next-hop information for both IPs.


**Phase 2** (Backbone PE Mapping). For IPs that traverse the backbone, NFL uses BGP community-to-device configuration mappings to identify which Provider Edge (PE) routers handle that traffic.


**Phase 3** (Backbone PE Follow-up). NFL queries the PE routers' routing information base using vendor-specific commands to trace the path across the backbone.


**Phase 4** (Route Merging). Results from site distribution and PE queries are merged into a unified topology, with equal-cost multi-path deduplication to avoid redundant firewall policy checks.


**Phase 5** (LLDP Discovery). Starting from the merged route data, NFL iteratively queries each device's LLDP neighbors to discover the full chain of switches, routers, and firewalls between source and destination.


The result is a complete device-by-device map of the network path, including every switch, firewall, and router the traffic actually traverses.


### Multi-vendor abstraction


GoDaddy's network includes devices from several vendors, each with different CLI syntax, different output formats, and different approaches to concepts like security zones and routing tables.


NFL handles this through a Registry Pattern with vendor-specific analyzers. A` DeviceCoordinator` groups discovered devices by vendor type, determined from the device inventory which stores vendor metadata per FQDN. Each vendor has a dedicated analyzer class (Arista, Cisco, Juniper, Palo Alto) registered in a factory. The coordinator looks up the appropriate analyzer by vendor ID and delegates command generation and output parsing to it. All analyzers implement the same interface (` get_routes()` ,` get_interfaces()` ,` get_firewall_policies()` ) but translate to vendor-specific CLI commands and parse vendor-specific output formats. The flow checker and path diagnostics engines work with normalized data structures, unaware of vendor differences.


This means adding support for a new vendor requires implementing a parser and analyzer. The core analysis logic remains unchanged.


### Real-time streaming with Server-Sent Events


Network analysis can take 30–60 seconds as NFL queries devices in sequence. Rather than showing a spinner and dumping results at the end, NFL streams every step of the analysis to the browser in real-time using Server-Sent Events (SSE).


The streaming flow works as follows:


1. The frontend initiates an analysis and opens an SSE connection with a unique client ID.
2. The backend spawns a background thread for the analysis task.
3. As the task progresses, it calls` sse_log()` to push messages into a per-client Queue.
4. A Flask streaming response generator yields messages from the queue as SSE data frames.
5. The browser's EventSource API receives each message and appends it to the log panel.
6. When analysis completes, a terminal message (` TASK_COMPLETE` or` TASK_FAILED` ) closes the stream.
7. Abandoned connections are cleaned up via an LRU eviction policy. If a client disconnects without closing cleanly, their queue slot is reclaimed when capacity is needed.


The following output shows what users see during an analysis:


` \[1\] Identifying source and destination zones... Source zone: trust-zone-dc1 Destination zone: dmz-zone-dc2 \[2\] Tracing network path... Found 3 firewalls in path \[3\] Analyzing firewall policies... Firewall 1: Checking policies... Firewall 2: Checking policies... Firewall 3: Checking policies... \[4\] Generating results...`


This gives users immediate feedback that the system is working, and helps them understand the analysis process. It's especially useful when a query takes longer than expected due to device timeouts.


### Reliability and caching


NFL queries live network state, but it doesn't re-discover device inventory on every request. The caching architecture balances freshness with performance.


Network device metadata (FQDNs, roles, sites, vendor types) is sourced from GoDaddy's device management platform and cached in memory. A file system watcher detects inventory updates and triggers background reloads without requiring a restart. Vendor-specific analyzer objects are instantiated once and reused across requests through a singleton factory pattern. Thread-local storage holds authenticated HTTP sessions with connection pooling, eliminating repeated TCP handshakes and authentication overhead to the device execution platform. An LRU cache (max 1,000 entries) manages per-client SSE message queues, automatically evicting inactive clients.


Critically, routing tables and firewall policies are never cached. Every analysis queries live device state to ensure results reflect reality.


Querying production network devices also requires careful error handling. Failed device queries are retried up to three times with exponential backoff (1s, 2s, 4s). Sixty-second timeouts on device API calls prevent hung queries from blocking analysis. If a device is unreachable, NFL reports what it can determine from reachable devices rather than failing entirely. Known problematic devices can also be excluded from analysis via configuration.


What failure looks like in practice: if a firewall in the path is unreachable, NFL doesn't silently skip it. It reports the results it could determine and explicitly flags that one device was unavailable. The user sees something like "2 of 3 firewalls responded, 1 timed out after 60s." The verdict in this case is marked as incomplete rather than permit or deny, ensuring users don't make decisions based on partial data.


### The two-snapshot delta


A naive approach to interface diagnostics would be to query error counters and report any non-zero values. The problem is that network interfaces accumulate errors over their entire uptime. A counter showing 50,000 input errors might represent a burst from six months ago that was resolved the same day. Reporting it as a current problem creates noise and erodes trust in the tool.


NFL's two-snapshot approach solves this by measuring the rate of change:


1. Capture all interface error counters (Snapshot A)
2. Wait 10 seconds
3. Capture the same counters (Snapshot B)
4. Report only interfaces where Snapshot B - Snapshot A > 0


If an interface had 50,000 errors in both snapshots, it's not reported. If it went from 50,000 to 50,847, those 847 new errors in 10 seconds represent an active problem worth investigating.


This simple technique eliminates false positives and ensures every reported issue is actionable right now.


### Role-based access control


NFL implements two-tier access control to balance self-service with network security.


Exposing full network topology details (device hostnames, interface names, firewall policy names, zone structures) to every user in the organization creates risk. Someone with detailed knowledge of firewall rule structures and device naming conventions gains a significant advantage in a breach scenario. At the same time, most developers don't need that detail. They just need to know whether their traffic is blocked or not.


The two-tier model gives everyone the diagnostic power they need while keeping sensitive infrastructure details appropriately scoped:


- **Developer Access** provides a simple permit/deny verdict with aggregated results. No device FQDNs, policy names, or zone details are exposed. Path diagnostics shows only summary statistics.
- **Operations Access** provides full diagnostic detail including device FQDNs, interface names, firewall policy names, line numbers, zone information, per-firewall results, and downloadable reports.


Access is controlled via Active Directory group membership, validated against SSO on every request. The backend enforces filtering regardless of what the frontend requests. The role check and data filtering happen server-side before any response is sent.


## Seeing it in action


The following examples show what NFL output looks like for both access tiers.


### Flow Check example


A developer needs to validate that HTTPS traffic will be permitted between two internal services before deployment. They enter Source IP:` 10.50.100.25` , Destination IP:` 10.60.200.50` , Port:` 443` , Protocol:` TCP` .


The following output shows the developer access result:


` FLOW PERMITTED This traffic is allowed through the network. - 3 firewalls checked - 0 explicit denies found - Flow permitted by all devices`


The following output shows what the operations team sees for the same query:


` FLOW PERMITTED Firewall 1: firewall-01.example.net Policy: INTER-ZONE-POLICY-247 | Action: PERMIT Rule: trust-to-dmz-https Source Zone: trust-zone-dc1 -> Dest Zone: dmz-zone-dc2 Firewall 2: firewall-02.example.net Policy: INBOUND-POLICY-89 | Action: PERMIT Rule: allow-https-internal Firewall 3: firewall-03.example.net Policy: APP-TIER-POLICY-12 | Action: PERMIT Rule: backend-https-access Router ACLs: No blocking rules found`


If the flow were denied, the output pinpoints exactly which firewall, which policy, and which rule is blocking, along with the reason:


` FLOW DENIED Firewall 2: firewall-02.example.net Policy: INBOUND-POLICY-89 | Line: 156 | Action: DENY Rule: block-untrusted-https Reason: Source zone not in allowed list`


### Path Diagnostics example


An operations engineer is investigating intermittent packet loss between two services.


They enter Source IP:` 10.50.100.25` , Destination IP:` 10.60.200.50` .


The following output shows the developer access result:


` ISSUES DETECTED - Devices Checked: 8 - Interfaces Analyzed: 47 - Interfaces with Active Errors: 2 - Total Error Count: 1,247 Suggested Action: Contact network team for further investigation.`


The following output shows the operations access result:


` ISSUES DETECTED Devices Checked: 8 | Interfaces Analyzed: 47 Interfaces with Active Errors: 2 | Total Error Delta (10s window): 1,247 Problematic Interfaces: router-backbone-01.example.net — TenGigE0/0/1 Input Errors (delta): +847 | CRC Errors (delta): +847 router-backbone-01.example.net — TenGigE0/0/2 Input Errors (delta): +400 Recommendation: Active input errors on backbone router interfaces suggest physical layer issues (cable/transceiver) or congestion.`


The key difference is that developers know something is wrong in the network (so they don't waste time debugging their application), but device-specific details are reserved for the operations team.


## Technical decisions and tradeoffs


The following sections discuss some of the technical decisions and tradeoffs we had to make when developing NFL.


### Why use a centralized device execution platform?


We considered having NFL communicate directly with network devices using libraries like Netmiko or Paramiko, but chose to go through a centralized platform for several reasons. NFL never holds device credentials because the platform handles authentication via secrets management systems. Hundreds of NFL users don't each open their own SSH sessions to the same devices. Every command executed is logged centrally rather than scattered across application logs. And the platform can throttle requests to protect device control planes.
The tradeoff is that an additional layer adds latency per query, and NFL depends on the platform's availability. We accepted this because the safety guarantees outweigh the performance cost for a diagnostic tool.


### Why SSE instead of WebSockets?


SSEs are simpler than WebSocket for our use case because we only need server-to-client streaming, never client-to-server. SSE works over standard HTTP, requires no special proxy configuration, and automatically reconnects on network interruption. The browser's native EventSource API handles all of this with zero library dependencies.


### Why not cache routing/policy data?


Many network tools maintain a cached model of the network and answer queries against the cache. This is faster, but introduces a fundamental accuracy problem: network state changes constantly. A firewall rule added 30 seconds ago won't appear in a cached model until the next sync cycle.


For a diagnostics tool, accuracy trumps speed. A sub-second response that's wrong is worse than a 45-second response that's correct. Users are troubleshooting real problems and they need to trust the results.


We do cache device inventory (which changes infrequently) to avoid re-discovering what devices exist on every query.


## Real-world impact and quantifiable savings


The following table compares diagnostic workflows before and after NFL:


Metric Before NFL After NFL


Flow validation time 20–60 minutes < 1 minute


Path diagnostics time 30–90 minutes 1–3 minutes


Human error risk Medium-High Eliminated


Coordination overhead Multiple teams Self-service


Knowledge requirement Deep network expertise IP addresses


Time savings are estimated by comparing average network ticket resolution times (from our ticketing system) against NFL's measured analysis duration across hundreds of queries.


## Conclusion


Network Flow Lens democratizes network diagnostics at GoDaddy. What was once the exclusive domain of network engineers (requiring deep knowledge of vendor CLIs, routing protocols, and firewall policy syntax) is now available to any developer or operator who knows two IP addresses.


The impact is measurable:


- 100+ engineer-hours saved per month
- Sub-minute diagnostics replacing hour-long investigations
- Self-service troubleshooting reducing cross-team dependencies
- Real-time visibility into production network state


NFL changes how teams think about network troubleshooting. Filing a ticket and waiting becomes checking NFL right now. Guessing at the problem becomes pointing to a specific firewall policy match. Data-driven troubleshooting, available instantly, for everyone.


*Thanks to Steven Fair, Waqas Ahmad, and Fabio Marino for their contributions to Network Flow Lens.*
