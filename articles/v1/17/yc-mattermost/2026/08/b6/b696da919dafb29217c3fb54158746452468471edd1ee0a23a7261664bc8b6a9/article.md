---
schema_version: "1.0.0"
document_id: "b696da919dafb29217c3fb54158746452468471edd1ee0a23a7261664bc8b6a9"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/what-is-an-air-gapped-network/"
published_at: "2026-08-10T13:00:00+00:00"
first_seen_at: "2026-08-10T13:53:52.450834+00:00"
fetched_at: "2026-08-10T13:53:53.753590+00:00"
content_hash: "sha256:3af4e9f7372ce12d0c6dc0812a7f0f9f894ee5a13d855fddc3dc844c86c72204"
---

# What Is an Air-Gapped Network?

**Key Takeaways**


- An air-gapped network has no physical or logical connection to the internet or to any other outside network; not a restricted connection, no connection at all.
- “Air-gapped” is stricter than “isolated” or “segmented.” Those setups still route through a gateway or firewall; air-gapped systems have no route out to sever in the first place.
- On-premises and private clouds are not automatically air-gapped. Both can still reach the internet unless that connectivity is deliberately removed.
- Everything a system depends on, including software, updates, models, and credentials, has to be staged inside the boundary in advance, since nothing can be fetched on demand.
- Organizations choose air-gapping for classified data, regulatory data-residency requirements, and protection of critical infrastructure where the cost of a breach outweighs the convenience of connectivity.


An air-gapped network is a computing environment that is physically and logically isolated from the internet and from any other network, with no wired or wireless route for data to enter or leave. There’s no NAT, no external DNS, no cloud authentication call, and no background update check. If something needs to get in or out, it travels on physical media, carried by a person, through a controlled process.


The name comes from the literal gap of air between the network and everything else. It’s a deliberate design choice, usually made because the data or systems involved are too sensitive to risk any exposure, however small or well-defended they may be.


## Who Uses Air-Gapped Networks, and Why?


Not every organization needs an air-gapped network. But for a specific set of sectors, it’s non-negotiable.


Air-gapped networks show up wherever the consequences of a breach are catastrophic or even fatal, and organizations adopt them when the risk of any data leaving the environment, even briefly or by accident, outweighs the convenience of staying connected. These include:


- **Defense and intelligence agencies** rely on air-gapping for classified systems and mission networks, where it’s typically a hard requirement rather than a preference.
- **Government bodies** turn to air-gapping where data residency or sovereignty rules demand that sensitive data never traverse infrastructure outside a given jurisdiction or ownership structure; air-gapping is the most direct way to guarantee that.
- **Critical infrastructure operators** , including power grids, water systems, and manufacturing plants, air-gap the operational technology (OT) that controls physical equipment, since a remote attack there can cause physical damage, not just data loss.
- **Financial institutions and R&D-heavy enterprises** sometimes air-gap specific high-value systems, like core transaction ledgers or proprietary engineering environments, even without a regulatory mandate driving the decision. They just want to ensure their sensitive, confidential data stays that way; a leak would be commercially catastrophic regardless of any legal requirement to prevent it.


## Air-Gapped vs. Isolated vs. Segmented vs. DDIL Networks


These four terms get used interchangeably, but they describe different levels or different kinds of separation and are not perfectly interchangeable terms:


- An **isolated network** typically still has a restricted gateway — a firewall or proxy that allows specific outbound traffic under tight control. In higher-security environments, particularly critical infrastructure like power grids or industrial control systems, this gateway may instead be a data diode: a hardware device that physically enforces one-way data flow, making it impossible for traffic to pass back into the isolated network regardless of software configuration. Either way, the network is cut off from general internet access, but not from the outside world entirely.


- A **segmented or microsegmented network** divides a larger network into smaller zones, usually for security or performance reasons, but segments can often still reach each other or a shared gateway, and sometimes the internet, depending on how the segmentation is configured.


- An **air-gapped network** has none of that. There’s no gateway to restrict because there’s no path out to restrict it on. This distinction changes what’s operationally possible. An isolated network can still receive an emergency patch over its restricted connection. An air-gapped network can’t; every update has to physically cross the boundary.


- A **DDIL (Denied, Degraded, Intermittent, or Limited-bandwidth) network** describes a state of unreliable connectivity, usually in a contested or remote operational environment. The goal is to keep the network operational even as the connection itself fluctuates rather than assuming it’s fully present or fully absent.


### Air-Gapped Networks Can Still Be DDIL


Air-gapped and DDIL describe different things: air-gapped describes an inherent property of the network, whether a physical connection to outside networks exists at all, while DDIL describes a state or condition of connectivity, which can change.


An air-gapped network can still be DDIL: a shipboard network, for example, may be fully air-gapped from shore networks while its internal links are themselves denied, degraded, intermittent, or bandwidth-limited due to distance, jamming, or equipment constraints.


A DDIL system is designed to keep working when connections drop in and out; an air-gapped system is designed around the fact that a connection to the outside was never going to exist in the first place.


## What Has to Live Inside an Air-Gapped Boundary?


Everything the environment needs has to already be inside it before it’s needed, since there’s no way to reach outside and fetch something mid-task. In practice, that means:


- **Software and dependencies.** Applications, libraries, and packages all have to be pre-staged, typically through a private registry or mirror built specifically for the air-gapped environment.
- **Directory and identity services.** Authentication runs against an internal directory (AD/LDAP or similar) rather than a cloud identity provider, since there’s no path to reach one.
- **Data storage and search infrastructure.** Databases, search indexes, and file storage all run on hardware inside the boundary — there’s no managed or hosted equivalent to fall back on.
- **Any AI or automation components.** Language models, retrieval systems, and the tools they call all have to be self-hosted, since a cloud API call is, by definition, a connection to the outside.


## How Do Updates and Patches Get Into an Air-Gapped Network?


Updates cross an air-gapped boundary through a controlled physical transfer process, not an automatic download. The process will vary between organizations and jurisdictions but a typical approach looks like this:


1. **Build a bill of materials.** Catalog every package, container image, and dependency an update requires — including anything a component might quietly assume it can fetch at runtime.
2. **Stage it outside the boundary.** Assemble and verify the update package in a separate, connected environment first.
3. **Transfer it on approved media.** Move the verified package across the gap physically, on signed and audited media, following a controlled and typically infrequent cadence.
4. **Validate and scan before deployment.** Once inside, the package is checked for integrity and scanned for vulnerabilities before it reaches any live system — nothing that crosses the gap is assumed safe just because it was verified outside.


This is why urgent security patches in air-gapped environments usually have an expedited path that shortcuts the normal cadence, rather than skipping the transfer process altogether.


## Frequently Asked Questions


### Is on-premises the same as air-gapped?


No. On-premises infrastructure means the organization owns and hosts the hardware itself, but that hardware can still have an internet connection. Most on-prem deployments do for updates, licensing checks, cloud backups, or third-party integrations. Air-gapping is a separate, additional decision to remove that connectivity entirely. An organization can run fully on-prem and still not be air-gapped, and in practice, most are not.


The same logic applies to private cloud and sovereign cloud deployments. They offer more control over data location and access than public cloud, but unless connectivity is explicitly severed, they’re not air-gapped either.


### Is an air-gapped network completely immune to attacks?


No. Air-gapping removes the risk of remote network attacks, but it doesn’t remove every risk. Malware can still enter through infected removable media, and insider threats or physical access remain possible. Air-gapping reduces one attack surface — the network — not every attack surface.


### Can an air-gapped network still have internal users and devices?


Yes. Air-gapping refers to the network’s connection to the outside world, not to how many users, devices, or internal systems exist within it. Large air-gapped networks can still support full internal collaboration, messaging, and workflow tools — those systems just have to be self-hosted inside the same boundary.


### What’s the difference between DDIL and air-gapped?


DDIL (Denied, Degraded, Intermittent, or Limited-bandwidth) describes environments where connectivity is unreliable or degraded rather than permanently absent — for example, a deployed military unit that has intermittent satellite access. Air-gapped environments have no connectivity at all, by design, rather than unreliable connectivity by circumstance. Some systems are built to handle both conditions, since the operational requirements overlap.


### Do air-gapped networks ever connect to anything, ever?


Only through deliberate, controlled, and typically infrequent physical transfers — not through any live network connection. The whole point of the design is that there’s no path for an automatic or remote connection to exist.


*Building or evaluating an air-gapped deployment?*[Experience a 1-hour preview environment for Self-Sovereign Collaboration](https://mattermost.com/sign-up/?usecase=self-sovereign) *and see how it works fully disconnected.*


## Read More Collaboration Articles


## Open source news, right in your inbox


## Thanks for subscribing!
