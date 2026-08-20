---
schema_version: "1.0.0"
document_id: "404395ccd40b0c67a646de1b851a06cd53281d1ff483b74acbb496775d4435d8"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:2ea23acadad009aba0120a48f90a4f73b930f12dc6956b902592354489849df8"
---

# Your UEBA is lying to you: Why entity record quality decides everything

5 May 2026 •


[Erik Huang](https://www.elastic.co/security-labs/author/erik-huang) •


[Mike Paquette](https://www.elastic.co/security-labs/author/mike-paquette)


# Your UEBA is lying to you: Why entity record quality decides everything


Most entity analytics systems are confidently wrong. They track users who do not exist, generate risk scores built on noise, and call it behavioral analytics. Learn why the entities records you don't create matter as much as the ones you do and how a confidence-tiered model changes the game.


8 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


There's an uncomfortable truth in security analytics that nobody talks about at conferences: The quality of your detections, alerts, and investigations is only as good as the entity records that represent the users, hosts, and services in your environment.


Not the machine learning models. Not the anomaly detection algorithms. Not the risk scoring engine. The *entities* : the foundations to teach your system about the data and protect it.


Get the entities wrong, and everything downstream is contaminated, including AI. Your baselines are fiction. Your risk scores are noise. Your analysts are chasing ghosts. And the worst part? Most user and entity behavior analytics (UEBA) implementations get the entities wrong from day one. This blog explains why the entities you *don't* create matter and how a confidence-tiered model helps.


A note on terminology before we go further: in this piece we’ll distinguish between the real-world thing — a person, a host, a service — and the entity record Elastic Security creates to represent it. The argument that follows is about which records are worth creating, not about which things exist. We’ll use “entity record” when the distinction matters.


##


One username, hundreds of identities


Consider the simplest possible approach to creating a user entity record: Take a` user.name` field from an event log and call it an entity. A username like` deploy` appears in your telemetry, so you create a` deploy` entity record and start building a behavioral baseline.


The problem is immediate and severe. That` deploy` username might exist on 200 servers. It might be used by 12 engineers and a continuous integration and continuous deployment (CI/CD) pipeline. The behavioral baseline you're building is a smoothie blended from hundreds of different machines, used by different people, for completely different purposes. The system is treating a string match as an identity, barely one step above random.


When this entity record inevitably generates elevated risk scores, analysts investigate, only to discover that the "anomaly" was just a different engineer using the same shared account in a slightly different way. Multiply this across every common username in a large environment and you've built a system that generates investigative busywork at industrial scale.


The opposite mistake: An empty dashboard


Some security vendors recognize the problem and swing to the other extreme. They decide (correctly, in principle) that only identity-provider-backed entity records are trustworthy enough for behavioral analytics. If you can't tie an entity record to an authoritative account in a directory like Okta, Entra ID, or Active Directory, don't create one at all.


The engineering reasoning is defensible. The product experience is catastrophic.


A customer rolls out endpoint agents across their fleet, opens the security analytics dashboard, and sees nothing. Zero entities. The feature looks broken. The SOC analyst has no idea why no users are showing up, and worse, has no visibility into the activity happening on those endpoints right now. A purist entity data model has produced a blind spot.


##


The missing middle: Host-scoped identity


Most vendors building UEBA have historically picked between two unsatisfying defaults, bare usernames that blend everyone into noise, or IdP-only entities that leave most deployments with nothing. But without explicit governance over which signals come from which source, the noise still leaks through.What's missing is a middle layer: entities derived from endpoint telemetry that are tightly scoped enough to be meaningful but carefully governed enough to avoid the noise problem.


The instinct might be to simply pair a username with a host and call it an entity record. But without guardrails, this just moves the noise problem down one level.` deploy` on` prod-web-03` is still five engineers' blended activity.` root` on a shared bastion host is still everyone and no one. You've reduced the blast radius from "all servers" to "one server," but the behavioral baseline is still a fiction if the underlying account is shared, automated, or observed only through a failed brute-force attempt that never actually succeeded.


The real question isn't whether to create local host-scoped entity records. It's *which* host-scoped entity records are worth creating and with what governance over how they participate in risk scoring and identity resolution downstream.


##


The Elastic approach: Two kinds of entities, governed differently


One answer is to recognize that not all record sources are equal and to build that distinction into the architecture itself, governing how each record is created, enriched, scored, and resolved.


This is the approach Elastic Security takes. Instead of treating all entity records as interchangeable, the system draws a clear line between **identity-provider-backed entities** and **endpoint-observed local host entities** and governs each category differently under the hood.


**Identity-provider-backed entities** are created only by authoritative identity systems: Okta, Entra ID, Google Workspace, Active Directory, and other identity-provider namespaces across identity and access management (IAM), cloud platforms, software as a service (SaaS), and privileged access management (PAM) systems. These entity records represent verified accounts in systems that own and manage those accounts. They get the full analytical treatment, that is, rich behavioral baselines, cross-platform enrichment from 120+ security integrations, and full participation in person-level risk scoring.


**Endpoint-observed entities** are created from endpoint telemetry: Your endpoint detection and response (EDR) agent observes` jdoe` active on a specific host and creates a host-scoped entity record tied to that local machine. These entity records are real and useful, but the system knows they carry less identity certainty, and it governs them accordingly.


Analysts don't need to think about any of this machinery. What they see is an entity store that's populated from day one with more accurate user entity records. The governance happens in the architecture so it doesn't have to happen in the analyst's head.


(An endpoint-observed entity for sarah.chen active on her corporate MacBook. The entity name (sarah.chen@CORP-MAC-SC-2024) uses the human-readable hostname for readability. The entity ID (user:sarah.chen@b92f1e3a-7d4c-4a8b-9f2e-1c3d5e7f9012@local) uses the machine's hardware UUID instead of its hostname — this is intentional. Hostnames change when a device is renamed or reimaged; the hardware UUID is permanent. The @local suffix identifies this as an endpoint-observed entity: it represents sarah.chen's activity on this specific machine, not sarah.chen as a verified identity across your organization.)


##


From fragmented accounts to a Unified User Group.


Even when you get entity record creation right, you’re still left with a fragmentation problem no amount of per-entity discipline can solve alone.


Consider John Doe in a large enterprise. He has an Okta account for SaaS access, an Entra ID account tied to his corporate laptop, and an Active Directory account for on-prem systems. Each is a legitimate, authoritative entity record by every standard described above, and yet they’re three separate records for the same human being, each with its own risk history, each generating signals in isolation.


When John’s Entra account shows a lateral movement indicator the same day his Okta account flags a suspicious login from an anomalous location, those signals may never connect. They exist as separate entities, investigated in isolation, by analysts who have no automated way to know they belong to the same person. The cross-surface campaign that entity analytics is supposed to catch hides in plain sight between identity providers.


Elastic Security solves this through automatic entity resolution: consolidating a user’s fragmented digital footprint across Okta, Entra ID, Active Directory, and more into a single unified identity. John Doe becomes a first-class citizen in the entity store: one primary record, all associated accounts grouped together, one aggregated risk score that reflects everything happening across every identity surface he touches. Resolution runs continuously as new integrations come online, without manual curation.


In the image above, we've created one entity record per user account and grouped them together, so when an analyst reviews the record, all related identities appear in one place.


##


What this changes for analysts


For the security operations center (SOC) analyst working a 2 a.m. alerted by their Elastic agentic workflow, these two architectural decisions (confidence-tiered entity governance and unified identity resolution) change the investigation fundamentally.


Instead of opening four separate entity cards for the same person, they open one. Cross-provider risk signals are aggregated into a single score, and attack narratives that span identity systems are visible as narratives, not as disconnected data points buried in separate views.


Compare this to investigating a bare deploy entity record with a risk score of 70 that’s actually an artifact of 12 people’s blended activity across 200 servers. One investigation leads somewhere. The other erodes trust in the entire capability, which is the real cost of noisy entity records. Not just the false positive itself, but the analyst who learns to ignore entity risk scores entirely.


##


The entities records you don't create


Perhaps the most underappreciated aspect of entity analytics design is restraint. Every entity record you create has a cost: Compute for baselining, storage for history, analyst attention when it generates alerts, and potential for noise propagation into the broader analytical model.


The discipline to *not* create an entity record (to require a minimum evidence threshold) is what separates an entity analytics system that gets more useful over time from one that slowly drowns its operators in noise.


In practice, this means maintaining a configurable exclusion list for common service and shared accounts: *root* , *jenkins* , *deploy* , *postgres* , and others like them. These accounts exist on hundreds of machines, are used by automated processes and multiple humans interchangeably, and would produce baselines that mean nothing. Elastic Security ships a default list covering the most common offenders. Today the list operates at the username level — root is excluded uniformly regardless of which host it appears on — and is fixed. Future iterations will make it configurable, letting teams add their own environment-specific service accounts and, eventually, specify compound patterns that combine username and host. That would allow excluding root globally on shared infrastructure while still creating a host-scoped entity when that account appears on a personal workstation where the activity is attributable to a specific person. The architecture is already built for it: because endpoint-observed entities are keyed as` {user.name}@{host}` , compound rules are a coherent extension, not a redesign.


The higher-fidelity approach we've described directly mitigates the broader noise problem. When entity records are created from any username string in a log, your ML models end up baselining service accounts, typo'd logins, and shared kiosk accounts as if they were people — a 2 a.m. backup job looks anomalous against a human baseline, and a one-event typo'd login never accumulates enough data to baseline at all. When entity records are anchored to authoritative identity sources and resolved across their various login forms, the model learns patterns for actual humans doing actual work. Anomalies become meaningful because the baseline is meaningful.


The best entity analytics isn't the one that creates the most entities. It's the one that creates the *right* entities, governs them by what it actually knows about their source and scope, and builds its analytical investment proportionally. Everything else (risk scoring, behavioral baselines, entity resolution, anomaly detection, and AI skills) is downstream of that foundational decision. Get the entity records right, and the analytics follow. Get them wrong, and no amount of machine learning or AI can save you.


*Entity analytics is available in Elastic Security.*[Learn more about advanced entity analytics and how the entity store governs user entities.](https://www.elastic.co/docs/solutions/security/advanced-entity-analytics/entity-store)


#### Jump to section


- [One username, hundreds of identities](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#one-username-hundreds-of-identities)
- [The missing middle: Host-scoped identity](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#the-missing-middle-host-scoped-identity)
- [The Elastic approach: Two kinds of entities, governed differently](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#the-elastic-approach-two-kinds-of-entities-governed-differently)
- [From fragmented accounts to a Unified User Group.](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#from-fragmented-accounts-to-a-unified-user-group)
- [What this changes for analysts](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#what-this-changes-for-analysts)
- [The entities records you don't create](https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics#the-entities-records-you-dont-create)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Your%20UEBA%20is%20lying%20to%20you:%20Why%20entity%20record%20quality%20decides%20everything&url=https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics&title=Your%20UEBA%20is%20lying%20to%20you:%20Why%20entity%20record%20quality%20decides%20everything)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/ueba-entity-record-quality-analytics&title=Your%20UEBA%20is%20lying%20to%20you:%20Why%20entity%20record%20quality%20decides%20everything)
