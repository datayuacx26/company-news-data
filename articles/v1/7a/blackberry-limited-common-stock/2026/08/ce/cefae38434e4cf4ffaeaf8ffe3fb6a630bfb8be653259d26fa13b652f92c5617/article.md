---
schema_version: "1.0.0"
document_id: "cefae38434e4cf4ffaeaf8ffe3fb6a630bfb8be653259d26fa13b652f92c5617"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/lessons-from-july-water-utility-attacks"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T04:30:26.930733+00:00"
fetched_at: "2026-08-11T04:30:28.925350+00:00"
content_hash: "sha256:62096c82df28a843cf8e186895cb29b1b1b16b32ed4275deadb27f723e3bb1ba"
---

# Lessons from the July Water Utility Attacks: What the Joint Advisory Tells Operators to Do Next

In late July, a series of cyberattacks struck U.S. water and wastewater utilities, prompting


[a joint advisory from the FBI, EPA, and CISA](https://www.cisa.gov/news-events/news/cisa-fbi-epa-and-us-government-partners-update-warning-iran-affiliated-threat-actors-targeting) . The advisory describes the observed activity, identifies the tradecraft involved, and lays out recommended mitigations for the sector. Per the advisory, the activity targeted the operational and administrative systems utilities depend on to deliver safe water; the federal agencies' attribution and characterization of the threat actors should be treated as the authoritative account, and this post defers to it throughout.


> ***Related:*** ***[Why OT Isolation Planning Needs Communications Continuity](https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity)***


Advisories are written as lists of mitigations. Responses happen as sequences of decisions under time pressure. The most useful thing a utility can do with the advisory is map its recommendations onto the timeline of an actual incident, because that mapping reveals which recommendations are preparation you do now and which are capabilities you must already have when the clock starts.


## **Hour Zero to Hour Six: Detection and Containment**


The advisory's near-term recommendations concentrate on identifying and evicting unauthorized access: reviewing remote access pathways, resetting credentials, and isolating affected segments. What the timeline view adds is this: every one of those actions is coordinated by people, and in the opening hours those people are working nights and weekends — and on their personal phones. The utilities that moved fastest in the July attacks are the ones that could assemble their response team in minutes, over channels they trusted, with confirmation of who was engaged. Alerting speed and communications assurance are not mentioned as line items in most advisories, but they are the substrate every recommended action runs on.


## **Day One to Day Three: Eviction and Continuity**


As containment proceeds, the advisory's guidance turns to hardening: segmenting IT from OT, removing unnecessary internet exposure of control systems, and validating backups. In this window, the practical constraint for most small utilities is not knowing what to do; it is doing it while continuing to treat water, brief regulators, coordinate with neighboring systems, and manage public communication, often with a staff measured in single digits. Command continuity, meaning a persistent, secure coordination channel for leadership and operations, is what keeps the technical work and the institutional work from colliding.


> ***Advisories list mitigations. Incidents demand coordination. The gap between the two is where small utilities struggle most, and it is closable before the next event.***


## **Week One and Beyond: Hardening and Proof**


The longer-horizon recommendations in the advisory, including credential hygiene, monitoring, and incident response planning, reward utilities that treat the July events as a rehearsal rather than a near miss. Two exercises pay for themselves immediately. First, a contact-and-alerting drill: how long does it take to reach every responder and confirm receipt, and does that process survive the loss of corporate email and telephony? Second, a trusted-channel drill: if primary networks were suspect tomorrow, over what channel would leadership discuss containment, and is that channel encrypted, authenticated, and independent of the affected infrastructure?


Most utilities that run these drills find the same three gaps: alerting depends on a manual call tree, the fallback coordination channel is consumer messaging on personal devices, and nobody can say with confidence which devices in the response chain are managed and patched. All three are addressable within a quarter and without touching the control network.


## **The Sector-Level Takeaway**


The July attacks and the joint advisory confirm what the water sector's defenders have said for years: this critical infrastructure is targeted, the targeting is documented in federal advisories rather than being hypothetical, and the operators responsible for defense are among the most resource-constrained in any sector. The right response is not to attempt everything in the advisory simultaneously. It is to sequence: secure the coordination layer first, because it is the capability every other mitigation depends on when the next advisory describes an event in progress rather than one contained.
