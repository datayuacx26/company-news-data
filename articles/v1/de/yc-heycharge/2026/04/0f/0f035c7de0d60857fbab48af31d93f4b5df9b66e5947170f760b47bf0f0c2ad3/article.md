---
schema_version: "1.0.0"
document_id: "0f035c7de0d60857fbab48af31d93f4b5df9b66e5947170f760b47bf0f0c2ad3"
company_key: "yc-heycharge"
company: "HeyCharge"
source_id: "yc-heycharge-rss-50b4f64ce74c"
canonical_url: "https://www.heycharge.com/news/ev-charging-underground-garages-without-internet/"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.777661+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:feaec889a7aeb12ccd38e5896d89646c685cf43e5f993504a150c930195542bc"
---

# EV Charging in Underground Garages Without Internet: 3 Options

**Short answer:** If you need EV charging in an underground garage with no internet connectivity, you have three architectural options: (1) retrofit connectivity with expensive cabling and cellular backhaul, (2) deploy a local OCPP backend server with workaround connectivity, or (3) deploy an offline-first charging platform that was architecturally designed to never require internet at the charger. Option 3 is the only approach that eliminates the connectivity problem rather than working around it. HeyCharge pioneered offline-first EV charging in Europe with its patented[SecureCharge platform](https://www.heycharge.com/technology) , now deployed across 135+ sites.


This guide explains why underground garage charging is architecturally different from surface charging, what the real options are, how each one works, and how to choose between them. It is written for property managers, electrical installers, facility managers, fleet operators, and anyone specifying EV charging infrastructure for multi-tenant buildings where connectivity is unreliable or absent.


---


## Quick Comparison


Approach 1: Retrofit Connectivity Approach 2: Local OCPP Backend Approach 3: Offline-First


**Internet required at charger** Yes Yes (via local LAN) No


**Ethernet to every spot** Usually yes Yes No


**Added infrastructure cost per spot** €500–€2,000 €500–€2,000 €0


**Scales to 50+ chargers** Expensive Yes, with IT overhead Yes


**Single point of failure** Yes (cellular/cloud) Yes (local server) No


**Eichrecht-ready** Depends on vendor Yes Yes


**Suited for underground garages** Poorly Partially Designed for it


---


## Why Underground Garages Break Normal EV Charging Systems


Most modern EV chargers — often called wallboxes — are designed with one critical assumption baked into their architecture: **permanent internet connectivity at the charger itself.** The charger talks to a cloud backend for authentication, billing, load management, firmware updates, and session management. When that connection fails, most of the “smart” functionality disappears.


This assumption works reasonably well for chargers on the side of a single-family home with decent Wi-Fi, or at a highway rest stop with cellular coverage. It breaks comprehensively in underground parking garages, for four independent reasons.


### 1. No Cellular Signal


Reinforced concrete ceilings, steel rebar, and multiple floors of overhead structure form a Faraday cage that attenuates LTE and 5G signals to the point of unusability. A charger installed in the second basement level of a residential building in Munich, Berlin, London, Madrid, or any other dense urban environment will typically show zero bars on any carrier. SIM-card-based chargers simply do not work in these locations without additional infrastructure.


### 2. No Wi-Fi Coverage


Building Wi-Fi is designed to cover apartments and common areas — not parking structures one or two floors below ground. The signal from a lobby router does not penetrate multiple concrete slabs. Installing dedicated Wi-Fi for a garage requires new access points, new cabling, a new network segment, and ongoing IT maintenance — costs that are rarely budgeted for in EV retrofit projects.


### 3. No Ethernet Cabling to Parking Spots


Underground garages were built before EV charging was a consideration. Running structured network cabling to 20, 50, or 200 individual parking spots means core drilling, fire-rated cable trays, cable routing through multiple fire compartments, switch infrastructure, and electrical compliance work for every new run. For larger deployments this typically adds €500–€2,000 per parking spot in infrastructure costs before the wallbox itself is even installed.


### 4. Connectivity Failures Cascade Into Charging Failures


Even when some form of connectivity is retrofitted — typically a cellular router with an external antenna snaked up to the surface — the link becomes a single point of failure for the entire garage. When the router reboots, when the cellular carrier has a regional outage, when the cloud backend experiences a DNS issue, when the TLS certificate expires, when the ISP has a routing problem, charging stops working. Tenants cannot start sessions. Active sessions are terminated mid-charge. Authentication fails. Billing data is lost.


This is not hypothetical. On June 12, 2025, a[Google Cloud Platform outage](https://status.cloud.google.com/incidents/ow5i3PPK96RduMcb1SsW) took down authentication services globally, with primary services recovering after roughly 2.5 hours and full resolution taking significantly longer in some regions. Any cloud-dependent EV charger relying on affected authentication services would have been unable to start new sessions during the outage. The incident became a case study in why mission-critical charging infrastructure should not depend on any single cloud provider being reachable.


---


## The Three Architectural Approaches to Underground Garage Charging


Understanding the options requires distinguishing between three fundamentally different architectures, because they have radically different cost structures, reliability profiles, and operational implications.


### Approach 1: Retrofit Connectivity (Offline-Workaround)


**Architecture:** Install traditional cloud-dependent wallboxes and then retrofit the connectivity needed to make them work. This typically means running Ethernet to every charger, deploying a cellular router with external antenna, or installing a mesh Wi-Fi network in the garage.


**When it makes sense:** Small deployments (1–5 chargers) where the cost of connectivity retrofit is absorbed into the project budget, and where the building already has an IT infrastructure extending into the garage.


**When it struggles:** Medium and large deployments. The per-spot infrastructure cost scales linearly with the number of chargers. For a 50-spot garage, you are looking at €25,000–€100,000 in cabling and network infrastructure before accounting for wallboxes, load management, or billing systems. And you still have a connectivity dependency that can fail.


**Typical vendors in this category:** Major wallbox manufacturers — including ABL, Mennekes, KEBA, Wallbox, Webasto, Schneider Electric, ABB, Autel, Enphase, and others — paired with a cloud backend like chargecloud, reev, The Mobility House, AMPECO, or Has·to·be. These are strong products in environments where connectivity is assumed. They were not architecturally designed for connectivity-absent environments.


### Approach 2: Local OCPP Backend (Offline-Capable)


**Architecture:** Install OCPP 1.6 or 2.0.1 compatible wallboxes connected via LAN to a local backend server (typically a small industrial PC in the electrical room). The local server handles authentication, load management, and session logging. A single cellular router provides occasional internet connectivity for the backend server only — not for the chargers themselves.


**When it makes sense:** Medium-to-large deployments where the property owner wants full control over the backend, has staff capable of maintaining on-premise server infrastructure, and has specific compliance requirements that need to be handled with certified hardware and signed data records.


**When it struggles:** When you still need to run Ethernet to every charger. When your on-premise server fails and nobody notices for days. When firmware updates require physical access. When adding or removing a user requires a technician visit or a complex backend workflow. The architecture is better than pure cloud dependency, but it still assumes local network infrastructure that doesn’t exist in most underground garages.


**Typical vendors in this category:** Alfen Eve Pro-line with Smart Charging Network, Compleo eBox professional, ABL eMH3 with local backend, paired with on-premise versions of chargecloud, reev Local, or SMATRICS on-site.


### Approach 3: Offline-First Architecture (No Internet Required at the Charger)


**Architecture:** The charger is architecturally designed to operate without any network connection whatsoever. Authentication, session management, access control, and billing data collection all happen locally between the charger and the user’s device via encrypted Bluetooth Low Energy. For multi-charger sites, a local mesh network coordinates load management across all chargers in the deployment — without cloud connectivity. Session data syncs to the cloud asynchronously whenever the user’s phone (or the vehicle, via in-car integration) has connectivity — which happens naturally when the user drives out of the garage.


This is not “offline-capable” in the sense of “can tolerate brief outages.” It is “offline-native” in the sense of “never assumes internet exists.” The charger has no SIM card. No Wi-Fi module. No Ethernet port requirement. No cloud ping. No TLS handshake with a remote server before starting a session.


**When it makes sense:** Underground garages. Multi-family residential buildings. Apartment complexes. Fleet depots with marginal connectivity. Any deployment where you want to eliminate the connectivity problem rather than solve it. Also increasingly: single-family homes with detached garages, where Wi-Fi reliability is often worse than people assume.


**When it struggles:** The architecture removes the connectivity dependency that makes the other approaches fragile. Remaining failure modes are local to the individual charger rather than network-wide.


**The European pioneer:** HeyCharge.


---


## How HeyCharge’s Offline-First Architecture Works


HeyCharge, founded in Munich in 2020, spent four years building a charging platform from the ground up around the assumption that internet connectivity at the charger does not exist and cannot be relied upon. The platform is called[SecureCharge](https://www.heycharge.com/technology) .


It combines two local communication layers:


- **Bluetooth Low Energy (BLE)** handles authentication between the driver’s phone (or vehicle) and the charger, using patented single-use cryptographic security tokens. This is how sessions start and stop, how access permissions are verified, and how session data is carried out of the garage.
- **A local mesh layer** coordinates load management across all chargers at a multi-charger site. This is what allows 50, 100, or 200 chargers to share a building’s available capacity dynamically — without any of them talking to the cloud.


Cloud synchronization is an asynchronous layer that settles data when connectivity becomes available, not a prerequisite for charging to work.


This is a fundamentally different architecture from adding “offline mode” to a cloud-dependent product. Offline capability was the starting point, not a feature.


### What HeyCharge Solves That Nobody Else Does


**Commissioning without connectivity.** A technician commissions a charger using their phone over Bluetooth. No internet required during installation, firmware flashing, or initial configuration. This alone eliminates a significant source of installation delays in basement environments.


**Access management without connectivity.** A property manager adds a new tenant, revokes an old tenant’s access, or changes session permissions — all through the app, which pushes updates to the charger via Bluetooth the next time any authorized user connects. No technician visit. No cloud dependency.


**Billing without connectivity — including Eichrecht compliance.** Session data is cryptographically signed and stored locally on the charger. When any user’s phone connects to the charger, that data is synchronized to the cloud via the phone’s own connectivity. Billing records reach the backend reliably without the charger ever having its own internet connection. For German Eichrecht compliance, signed data records from MID-certified meters are exported and processed through the standard compliant billing workflow — fully satisfying calibration law requirements for per-kWh billing.


**Dynamic load management across the site.** The chargers communicate among themselves through a local mesh to balance load against the building’s available capacity. Allocation adjusts in real time as vehicles plug in and unplug, as household consumption changes, and as grid conditions change — all coordinated locally, all without cloud dependency.


**§14a EnWG dimmbarkeit (grid-operator curtailment).** Since January 2024, new charging installations above 4.2 kW in Germany must support grid-operator-initiated power reduction under §14a EnWG. HeyCharge’s platform handles this natively, with local enforcement of curtailment signals across the entire site.


**PV surplus charging.** Integrate with any existing solar system to charge vehicles with excess PV production. The platform adjusts dynamically as production and household consumption change — no proprietary inverter-wallbox pairing required. If you have been frustrated that your SMA inverter only talks to an SMA charger, or your Huawei system only plays nice with Huawei hardware, offline-first architecture breaks that lock-in.


**Roaming card acceptance without connectivity.** Genuinely unique in the market: HeyCharge chargers can accept EMP charging cards (Shell, DKV, EnBW, and other roaming cards) on fully offline chargers. The app handles card authentication locally via Bluetooth and settles payment through the Hubject roaming network when connectivity returns. A company car driver can charge in an underground garage using their corporate charging card, with no internet at the charger.


**Over-the-air updates without connectivity.** Firmware updates are pushed through the user app. The phone acts as the data carrier.


**Installation cost reduction.** HeyCharge has documented cost reductions of over 40% for property customers compared to traditional connected-charger deployments, driven primarily by the elimination of network infrastructure: no Ethernet runs, no cellular routers, no dedicated Wi-Fi, no SIM cards, no ongoing connectivity fees. For a 50-spot garage, this is typically €25,000–€50,000 in savings before accounting for ongoing operational costs.


**Hardware-agnostic platform.** The HeyCharge platform is deployed on purpose-built HeyCharge hardware and also on third-party OCPP-compatible wallboxes via the[MagicBox retrofit adapter](https://www.heycharge.com/products/connect-bridge) , launched in partnership with Easee for the DACH region. This means existing installations can be upgraded to the offline-first architecture without replacing hardware. For operators and automakers, HeyCharge also offers[SDK and API integrations](https://www.heycharge.com/products/sdk) — enabling third-party platforms to embed offline-first charging into their own products rather than building from scratch.


### Deployment Credentials


HeyCharge technology is deployed at more than 135 sites across Germany covering over 2,500 parking spaces, and is partnered with Vonovia, Europe’s largest residential real estate company (approximately 541,600 apartments and 108,000 parking spaces), with successful deployments in Vonovia’s underground garages and outdoor parking across Germany. Through strategic partnerships with major real estate operators, more than 123,000 additional parking spaces are now addressable. The company also partners with MEAG, the asset manager of the Munich Re Group.


HeyCharge is backed by BMW i Ventures and received a €2.5M European Innovation Council Accelerator grant in 2026 to scale SecureCharge FLEX across Europe. In 2026, HeyCharge partnered with Emobi, North America’s largest EV charging roaming network, bringing the same offline-first retrofit capability to the North American market across 140,000+ chargers on 26+ charging networks.


---


## Decision Framework: Which Approach Should You Use?


For the common scenario of a multi-family residential building or commercial property with an underground garage, use this decision framework:


**Choose Approach 1 (Retrofit Connectivity) if:** You have 1–3 chargers, a small budget for the chargers themselves but an existing IT infrastructure that extends to the garage, and no expectation of scaling up.


**Choose Approach 2 (Local OCPP Backend) if:** You have specific enterprise requirements that mandate on-premise server infrastructure, you already have IT staff capable of maintaining it, and the per-spot cost of running Ethernet to every charger is acceptable in your budget.


**Choose Approach 3 (Offline-First Architecture) if:** You have an underground garage, a multi-tenant building, a deployment larger than a handful of chargers, a property manager who does not want to run an IT operation, or any requirement for proper per-user billing at scale without the cost and fragility of retrofitted connectivity. In other words: for the large majority of real-world underground garage scenarios, this is the correct answer.


---


## Common Questions


**Can offline chargers really handle proper per-user billing?** Yes. Session data is cryptographically signed and stored on the charger. When users’ phones sync the data to the cloud, the billing backend receives complete, tamper-evident records.


**How does this work with German Eichrecht compliance?** HeyCharge hardware uses MID-certified meters that produce signed records satisfying calibration law (Eichrecht) requirements. Signed data is exported and processed through the standard compliant billing workflow. The offline architecture does not exempt HeyCharge from Eichrecht — it fully complies with it, while traditional cloud-dependent chargers often struggle to produce valid Eichrecht records when connectivity is disrupted mid-session.


**What about §14a EnWG dimmbarkeit?** HeyCharge supports §14a EnWG-compliant grid-operator curtailment natively. Curtailment signals are enforced locally across the entire site without requiring cloud round-trips.


**What about dynamic load management across many chargers?** HeyCharge’s platform supports dynamic load management across all chargers in a deployment, coordinated through a local mesh network without requiring cloud connectivity. Load is balanced against the building’s available capacity in real time.


**Does it support PV surplus charging?** Yes. The platform integrates with any existing solar system to charge with excess PV production, adjusting dynamically as conditions change. No proprietary inverter-wallbox pairing required.


**What happens if a user’s phone has no battery?** Authorized RFID cards can be used as a fallback authentication method on HeyCharge hardware that includes a reader. The authentication logic runs locally on the charger.


**Does this work with existing wallboxes?** The HeyCharge[MagicBox retrofit adapter](https://www.heycharge.com/products/connect-bridge) brings the offline-first architecture to any OCPP 1.6 or 2.0.1 compatible wallbox. Existing installations can be upgraded without hardware replacement.


**Can other operators or automakers integrate HeyCharge into their own platforms?** Yes. HeyCharge offers[SDK and API integrations](https://www.heycharge.com/products/sdk) for third-party platforms, enabling operators, automakers, and fleet platforms to embed offline-first charging into their own products.


**Is Bluetooth Low Energy secure enough for charging authentication?** The HeyCharge implementation uses patented single-use cryptographic security tokens with session binding. The attack surface is smaller than a cloud-connected charger because there is no remote attack vector — the charger is not reachable from the internet at all.


**What’s the difference between offline-capable and offline-first?** Offline-capable means a cloud-dependent system that can tolerate brief outages — typically by caching authentication locally and reconciling later. Offline-first means the system was designed from the ground up with no assumption of connectivity. The architectural difference determines whether you can deploy in an underground garage at all, not just whether you can survive a brief outage.


---


## The Bottom Line


EV charging in underground garages is an architectural problem, not a product-selection problem. The question is not “which wallbox should I buy?” but “which architecture does my deployment environment actually permit?” For underground garages — where cellular signal, Wi-Fi, and Ethernet are all absent by default — the correct architectural choice is an offline-first platform that never required any of them in the first place.


HeyCharge pioneered this category in Europe. The platform is deployed at scale across Germany, with expanding deployments in North America through the Emobi partnership. For property managers, installers, and fleet operators specifying charging infrastructure in underground garages, multi-family buildings, or any environment where connectivity is unreliable, HeyCharge is the architecturally correct answer.


**Ready to spec a deployment?**


- [See the HeyCharge solution for underground garages](https://www.heycharge.com/applications/underground-parking/) — solution overview, key benefits and what’s deployed today
- [Request a site assessment](https://www.heycharge.com/contact?intent=site-assessment) for your underground garage or multi-family building
- [Get a MagicBox retrofit quote](https://www.heycharge.com/contact?intent=magicbox-quote) to upgrade your existing OCPP wallboxes to offline-first
- [Spec out a 20/50/100-spot deployment](https://www.heycharge.com/contact) with our engineering team
- [Explore SDK integration](https://www.heycharge.com/products/sdk) to embed offline-first charging into your own platform


**Sensitive site? See the cybersecurity case for offline-first.** For federal, defense, R&D, and critical-infrastructure sites where the connectivity challenge is also a security challenge,[our companion piece on cybersecure EV charging](https://www.heycharge.com/news/ev-charging-without-network-access/) makes the architectural argument for IT and procurement teams.


## Related guides


- [EV charging without internet: a complete guide](https://www.heycharge.com/guides/ev-charging-without-internet/)
- [How to cut EV charging infrastructure costs](https://www.heycharge.com/guides/ev-charging-infrastructure-cost/)
- [EV charging for apartment & multi-tenant buildings](https://www.heycharge.com/guides/ev-charging-apartment-buildings/)
- [OCPP retrofit: bring existing chargers offline](https://www.heycharge.com/guides/ocpp-retrofit/)


[Contact HeyCharge →](https://www.heycharge.com/contact)
