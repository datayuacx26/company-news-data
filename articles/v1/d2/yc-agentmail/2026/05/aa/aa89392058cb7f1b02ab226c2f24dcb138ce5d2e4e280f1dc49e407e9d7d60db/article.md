---
schema_version: "1.0.0"
document_id: "aa89392058cb7f1b02ab226c2f24dcb138ce5d2e4e280f1dc49e407e9d7d60db"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/how-dossium-gives-every-agent-a-real-email-surface"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:87f685c8824655bd5745c4bdc9a1ce07ff99c400751b862b7f71ff63034b8d9f"
---

# How Dossium Gives Every Agent a Real Email Surface at Signup

**Company:**[Graphlit](https://www.graphlit.com/) | Product:[Dossium](https://www.dossium.ai/) , the Agent Experience layer for B2B work, founded by[Kirk Marple](https://x.com/KirkMarple) .


**Problem:** Every Dossium user needs a dedicated agent email surface from their first session. Provisioning that per user, at platform scale, without building custom infrastructure was the requirement.


**Result:** Every user receives a dedicated agent inbox at username@durableagents.ai, provisioned automatically before they finish signing up.


**Stack:** AgentMail +[Graphlit](https://www.graphlit.com/)


---


## What Dossium Is


Kirk has been thinking about how agents experience companies longer than most people in this space. Before Dossium, he built Graphlit, the context platform that sits underneath it. Graphlit ingests multimodal work, extracts entities and facts, resolves identity across sources, and organizes the result into a graph agents can reason over. Dossium is the agent layer on top of that graph.


The problem Dossium addresses is one most teams hit quickly when they deploy agents in production. Most agent products load a blank context window and start from zero. No memory of who you are, what your company does, what decisions were made last week, or which source is stale. The agent can call tools, but context, methodology, and judgment all still live in the user's head. Dossium loads all of that before the first token. When a run starts, the agent already has your company's context graph, your persona, your methodology, and the governed paths it can act through.


Memory and action, not just retrieval.


---


## The Problem: Email Infrastructure at Platform Scale


When you build a platform where agents act in the world, every user needs a real email surface. Not a shared sender alias. Not a notification relay. A real inbox the agent owns, sends from, and reads replies into.


At platform scale, this is a different problem than setting up email for a single team. You are provisioning inboxes per user on signup, automatically, with no configuration required from the person onboarding. That means deliverability, inbound parsing, threading, and per-identity isolation all need to hold up across every workspace on the platform.


Building that in-house means managing domains, handling MX records, maintaining a webhook layer for inbound, and keeping thread state per user. It is months of infrastructure work before shipping a single user-facing feature. And the result still requires ongoing maintenance as the platform scales.


Kirk had worked through this problem well before Dossium launched. He was using AgentMail during the Graphlit phase, which meant by the time Dossium needed per-user email provisioning, he had already validated it worked.


---


## How It Works Inside Dossium


During onboarding, Dossium provisions two agents for every new user: a messaging agent on iMessage and SMS, and an email agent with its own AgentMail inbox at username@durableagents.ai. Both are attached to the user's persona. Both are live before the user finishes signing up.


The email agent is not a setting the user enables. It is part of the platform from the first session. Every user gets a real address tied to their agent, with full send and receive capability, without any configuration on their end.


That inbox is how the agent operates in the world. Scheduled agents send morning digests. Triggered agents email meeting briefings an hour before they start. Chat agents handle inbound threads. The email surface is just there, working, from day one.


---


## Why AgentMail


AgentMail treats inboxes as programmable primitives. One API call provisions a real inbox with full send and receive capability. No credential sharing, no custom inbound parser, no domain configuration required. At platform scale, where every new Dossium user means a new provisioned inbox, that programmability matters.


The other option is building it yourself. Kirk had seen what that looked like. He chose not to.


> "I'd been watching AgentMail since their YC batch. The value was obvious. We started simple: email your Zine inbox to ingest attachments, something we wouldn't have built ourselves from scratch. With Dossium Agents, we extended that from users to agents. Now any agent can be bound to an AgentMail inbox for ingestion and async responses, grounded in the Dossium context graph. Without AgentMail, we wouldn't have shipped this. When we started, this category didn't exist."
>
>
> — Kirk Marple, Founder/CEO, Graphlit


---


**Products used:**


- Managed inboxes
- Send/Receive API
- Inbound parsing


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
