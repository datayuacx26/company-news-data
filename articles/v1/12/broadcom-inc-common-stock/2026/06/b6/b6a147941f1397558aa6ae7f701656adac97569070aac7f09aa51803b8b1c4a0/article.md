---
schema_version: "1.0.0"
document_id: "b6a147941f1397558aa6ae7f701656adac97569070aac7f09aa51803b8b1c4a0"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/broadcom-knights/unified-platform-unified-team-private-cloud"
published_at: "2026-06-02T19:03:23+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:62a6968ac63e262059bcc7d37ccadcd80be1093c7b87ec64969eb00efa94d055"
---

# Broadcom Knight’s partner blog: Silo busters – a unified platform needs a unified team

-
-
-


*An ongoing blog series by our Broadcom Knights, an elite group of Partner Technical Professionals with deep technical expertise in Broadcom’s portfolio and recognized as experts in their field.*


## The infrastructure speed trap


In my experience of architecting private cloud solutions, the greatest barrier to success is rarely the software itself – it is the way organisations choose to divide the people responsible for running it. Many invest in VMware Cloud Foundation (VCF) 9 as a high-performance cloud platform, yet they attempt to steer it with five separate hands on the wheel: Compute, Storage, Network, Security, and DevOps. Each team pulling in a slightly different direction, each optimising for their own domain, none of them responsible for the outcome as a whole.


I have worked with many organisations where the problem wasn’t the technology, it was the operating model around it. Different internal teams and external contractors each delivering their part of the infrastructure, then handing it to a central operations team that has no single owner and no shared view of how the pieces fit together. When something broke, there was no single person or team who could understand the whole picture.


This is the infrastructure speed trap. Not a technology problem, an organisational one. When operational tempo increases or a capability needs to scale quickly, a fragmented team cannot respond at the speed of the need. IT becomes a chain of hand-offs rather than a single engine of delivery. To break out of it, the organisation must make a deliberate choice: stop organising around technology disciplines and start organising around the service being delivered.


## Breaking the silo tax through platform engineering


The fundamental shift required is moving engineers away from discipline-specific siloes toward becoming ‘platform engineers’ - people who understand and operate the full stack, not just one slice of it. This is not about making network engineers learn storage or forcing security specialists to write virtualisation scripts. It is about creating a team with shared ownership, shared visibility, and a shared definition of success.


The cost of not doing this is what I call the ‘silo tax’ – the 70% of project lead time that is not engineering work at all, it’s wait time between departments. For example, change requests queued for the wrong team, approval requests sitting in inboxes, or meetings to agree on whose responsibility it is. For one organisation I worked with, provisioning a new isolated network segment used to trigger a chain of change requests across multiple teams and physical devices. A single mistyped subnet could mean hours of cross-team troubleshooting with no-one holding the thread.


What changed things for that customer wasn’t a new tool; it was embedding their own engineers into the design and build process – across disciplines, together – so that knowledge could be shared from day one rather than siloed and then lost. They did not just receive a platform. They understood it. And that understanding is what allowed them to operate it as a team. Weeks of manual coordination became minutes of collaborative execution.


## Networking and security as software attributes


One reason siloes have historically been so entrenched is that the underlying technology demanded them. Networking required network engineers with physical switch access. Security required firewall specialists with change control privileges. Storage required its own team with its own tooling. Each domain was genuinely complex enough to justify its own headcount - and its own ticket queue.


VCF 9 dissolves this justification. By integrating networking and security natively into the platform, through Virtual Private Clouds (VPCs) and VMware vDefend, these capabilities become software attributes that a unified team can manage through a single interface. The networking team no longer needs to be the gatekeeper for every new segment. In our project, they set the guardrails once within the platform, and other teams could then operate within them autonomously. The security function shifted from issuing tickets to defining policy, - from being a bottleneck to being an architect.


This matters because it removes the structural excuse for fragmentation. When a single platform surfaces all the controls a unified team needs, there is no longer a technical reason to keep five separate teams in five separate rooms. The organisational argument for siloes collapses alongside the technical one.


## Proactive operations: ending the firefighting


Nothing reveals the cost of siloed teams quite like an outage. When an application fails in a fragmented IT department, the server team checks their console, the networking team checks their switches, and the storage team audits their arrays - each in isolation, each using different tools, each looking at a different slice of the truth. The result is almost always the same: extended downtime, and a blame game that consumes more time than the fix.


VCF Operations changes this dynamic by providing a single management plane, one source of truth that the entire team looks at together. For the organisation I worked with, this was the moment the culture began to shift. When the network specialist and the DevOps engineer are both looking at the same consolidated telemetry, the question stops being “whose fault is it?” and starts being “how do we fix this together?” That is not a small change. It is the difference between having specialists who happen to work in the same building, and having a truly unified team.


## Conclusion: a blueprint for strategic growth


VCF 9 demands a different mindset. This change is human: you cannot run a unified platform with a fragmented team. The technology can surface everything in one place, automate the hand-offs, and remove the historical excuses for working in isolation – but only people can decide to stop acting as siloed specialists and start acting as a single, accountable unit.


The organisations that get the most from VCF 9 are not necessarily the ones with the most skilled engineers in each individual discipline. They are the ones who have built a team. A team that owns the platform end-to-end, that speaks a common language, shares a common view, and is collectively responsible for the service it delivers to the business.


A unified platform team is not a structural nicety. It is the operating condition that determines whether VCF 9 becomes a competitive advantage or an expensive complexity. The platform is ready. The question is whether the team around it is.


About the Author


About the Author


[Harry Thambi](https://news.broadcom.com/author/harry-thambi)


Strategic Pre-Sales Consultant at Xtravirt
