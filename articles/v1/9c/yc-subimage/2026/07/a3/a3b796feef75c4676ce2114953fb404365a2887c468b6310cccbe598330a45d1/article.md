---
schema_version: "1.0.0"
document_id: "a3b796feef75c4676ce2114953fb404365a2887c468b6310cccbe598330a45d1"
company_key: "yc-subimage"
company: "SubImage"
source_id: "yc-subimage-news-import-96b948f674dd"
canonical_url: "https://www.subimage.io/blog/corelight-case-study/"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:23.667341+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:061781f914f2faf9f42e702423034bfc9e21587f7fd14590c0b16e4828ca57da"
---

# How Corelight automated security alert triage with SubImage

## At a glance


- Up to an hour of manual triage per alert, **reduced to minutes**
- **No confirmed malicious alerts missed** in the alert types evaluated to date
- **One full day per week** of manual infrastructure work eliminated
- **20% of all security AI agent tool calls** go to SubImage – 2x the next most-used tool


**Impact** : Alerts that used to wait in a queue for an analyst now get investigated the moment they fire, and the team only spends time on the ones that genuinely need human judgment.


---


## Every alert investigated in minutes, around the clock


[Corelight](https://corelight.com/) , a leader in network detection and response, manages a large cloud environment where a security alert used to cost an analyst 30 to 60 minutes of context-gathering before the real investigation could even begin.


The responder first had to determine who owned the affected asset, what identities and permissions were attached to it, and what other systems it could reach – all while several more alerts arrived in the meantime.


"Most teams don't have enough people to look into all the things coming into their security pipeline. It's not because the team isn't capable, it's a scaling issue,” said security engineer Jordan Hair, who built an autonomous triage agent on top of the SubImage graph.


Now, every alert is investigated within minutes of firing, 24 hours a day. Of the alert types Corelight has evaluated to date, the system has not missed a confirmed malicious alert. When Hair reviewed the logs, SubImage was its most-used tool, receiving twice as many calls as the next-most-used tool.


---


## The expensive part of triage was gathering context


Before Corelight automated the process, every investigation began with the same manual excavation across consoles. "Just to get that starting information could take up to 30 minutes. Sometimes up to an hour," Hair said. And while the responder was following one thread, "you probably had 10 other alerts fired for completely different things."


Bernard Brantley, who wears dual CISO/CIO hats over security, IT, and AI, framed the deeper gap: "There are hundreds of tools out there, hundreds of different workflows. But that does not actually teach you how to understand what's going on in the broader business."


## Corelight had already proven that a graph could answer those questions


Corelight had long believed that security investigations should begin with a map of the environment rather than a collection of isolated tool outputs.


Chandan Chowdhury, a senior security engineer and former[Cartography](https://github.com/cartography-cncf/cartography) open-source maintainer, had built and operated a self-managed graph connecting Corelight’s cloud resources, devices, identities, and security data. It proved the approach worked, but keeping it running required a full day of engineering time every week for data syncs, database operations, and manual correlation.


Chowdhury's build-versus-buy threshold was straightforward: "You don't want to be dependent on a single person for maintaining critical infrastructure."


Corelight adopted SubImage as its central security knowledge graph, replacing the self-managed deployment with a continuously updated platform that both humans and autonomous systems could query. Moving to SubImage eliminated the full day of engineering work Chowdhury had previously spent maintaining the graph each week.


## SubImage became the context layer for autonomous triage


Jordan Hair built Corelight's alert triage agent with access to roughly 30 tools spanning endpoint detection, threat intelligence, identity, and cloud infrastructure.


For each alert, the agent first runs a structured playbook, then enters an autonomous investigation phase where it chooses which additional tools and searches to use. "I'm not telling it what to do anymore," Hair said. "I just show it what the playbook found, and it decides if it wants to continue making additional searches."


That autonomy created a natural experiment: nobody told the agent which tools to use, so its usage patterns reveal what it actually finds helpful during investigations. Hair reviewed approximately 2,000 autonomous tool calls and found that 20% went to the SubImage graph – twice as many as the next-most-used tool.


> *"My agent is deciding to use your tool the most, by far, out of all the tools it has. And there are zero false negatives, which is probably my biggest concern with autonomous triage."*
>
>
> **- Jordan Hair, Security Engineer**


The agent keeps returning to the graph because triage is built out of relationship questions – the same ones a human analyst would ask, now answered immediately against a continuously updated map of the environment:


- **Ownership and access:** Who owns this asset? Which identities and permissions are connected to it?
- **Blast radius:** If this asset is compromised, what could an attacker reach from here?
- **Vulnerability exposure:** Is the vulnerable service actually deployed? Are there other assets with the same exposure?
- **Closure evidence:** Is there enough context to close the alert, or should it be escalated?


When an alert does require human judgment, Corelight’s responders investigate the same graph using natural language.


"You just ask natural language questions how you would to an analyst if you're tasking them to go do something,” says Jordan Hair, “it almost feels like it unlocked the potential that SubImage always had with the data."


## The graph also changed how Corelight communicates risk


The same relationships that make autonomous triage possible give Corelight a more direct way to explain risk outside the security team. Rather than presenting an executive with a list of findings, Brantley can begin with a critical system and show who can reach it, how they can reach it, and what could change its behavior.


> *"I can start at a point in my infrastructure and ask, what has access to this thing? I look down at my feet and see three paths. This one is admin access. This one is user access. I walk down the admin path. Who are the people here? More paths light up and I can walk it."*
>
>
> *"I walk into an executive's office and say, take a walk with me. Let's look at who can actually change the way this system behaves. That conversation is much different."*
>
>
> **- Bernard Brantley, CISO**


---


## From alert detection to preventive security


Corelight now uses the same context layer for security work well beyond alert triage. Chowdhury has used SubImage to identify issues such as disabled accounts still consuming licenses, or EDR-reported locations mismatching employee home countries. Once he identifies a recurring issue, he builds automation to prevent it from returning.


> *"SubImage gives detective control where I'm looking for bad things happening. Automation is the preventative control, and they keep each other in check."*
>
>
> **- Chandan Chowdhury, Sr. Security Infrastructure Engineer**


Instead of maintaining the underlying graph, Chowdhury can spend that time building controls on top of it. The triage system gives Corelight immediate coverage, the graph gives both the system and the team the context to make decisions, and the automation built from those findings helps prevent recurring problems from producing another alert.


> *"We actually have a chance to be on top of it and actually be very proactive and very confident in our security posture. That completely changes everything."*
>
>
> **- Jordan Hair, Security Engineer**
