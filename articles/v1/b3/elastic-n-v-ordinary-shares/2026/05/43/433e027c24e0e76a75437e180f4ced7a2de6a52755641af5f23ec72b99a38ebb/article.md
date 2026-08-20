---
schema_version: "1.0.0"
document_id: "433e027c24e0e76a75437e180f4ced7a2de6a52755641af5f23ec72b99a38ebb"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:64708a863fdf525ad73511acf6007187051debeecbee519df4dbf25245ee5e02"
---

# AI-generated hunting leads: The hunt starts before you ask the question

5 May 2026 •


[Erik Huang](https://www.elastic.co/security-labs/author/erik-huang) •


[Mike Paquette](https://www.elastic.co/security-labs/author/mike-paquette)


# AI-generated hunting leads: The hunt starts before you ask the question


Introducing AI-generated hunting leads, proactive, environment-aware threat hypotheses powered by Elastic Entity analytics and integrated AI reasoning.


4 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Threat hunting has always been a human art; a practitioner staring at logs, forming a hypothesis, and patiently chasing it down. What if the hardest part of the hunt (knowing where to look) could be done for you, automatically, in milliseconds, and tuned specifically to your environment? This is where AI-generated hunting leads come in, allowing you to shift from reactive alerting to proactive defense with entity-centric, risk-based threat hunting tailored specifically to your environment's unique behavioral patterns.


##


The hunting gap no one talks about


Ask any threat hunter what slows them down, and you'll hear the same answer: it’s not the querying or the pivoting; it's the blank page. The moment before the hypothesis is formed. Most analysts know that somewhere in their telemetry there are patterns that signal compromise, lateral movement, or abuse; they just don’t know where to start.


Modern AI agents have a discovery problem: they’re brilliant at answering questions but useless if you don’t know what to ask. This "curiosity gap" traps security teams in a cycle of reactive hunting. Whether it’s waiting for a vendor threat intelligence report to drop, an alert to scream, or a CISO to grill the team during a QBR, the damage is often already done. While analysts wait for a hypothesis, AI-powered adversaries are moving at machine speed—widening an already dangerous window of opportunity.


The industry has tried to close this gap through detection rules, threat intel feeds, and user and entity behavior analytics (UEBA) scoring. These are necessary, but they're static frames applied to a dynamic reality. A UEBA anomaly tells you something is unusual. It doesn't tell you why it matters in your environment today.


###


The core problem


Detection rules tell you what to look for that’s very specific. Threat intel tells you what others found. Neither one tells you what your environment is uniquely at risk for right now, because neither one actually knows your environment.


##


Building the foundation: The entity store


Solving the hunting gap required us to first solve a data problem. Hunting leads are only as effective as their context; and in security, context is the sum of everything true about an entity over time.


We built the Elastic entity store as a purpose-built ontology for exactly this. Unlike Elastic Common Schema (ECS), which captures the state of a field at event time, the entity store is a longitudinal record, a living profile of characteristics of every user, host, and service in your environment. It tracks four dimensions that matter for security reasoning:


```text
// Entity Store Schema — Core Characteristics


entity.attributes // Who/what the entity IS
mfa_enabled: false // From AWS integration
privileged_groups: ["Domain Admins"] // From AD
asset_criticality: "high"


entity.lifecycle // Temporal facts
first_seen: "2024-09-14T08:22:00Z"
last_active: "2025-03-31T23:47:00Z"
dormancy_detected: true // Inactive 47 days, now active


entity.behavior // Anomalous signals (rolling window)
brute_force_victim: true
unusual_login_hours: true
new_geo_access: "DE" // First access from Germany


entity.risk // Scored risk aggregation
calculated_level: "Critical"
score: 94.2
```


This schema isn’t just storage; it's also a reasoning substrate. Each field represents a signal that, in combination with others, tells a coherent story about an entity's current threat posture. A user who was dormant for 47 days, is now active outside business hours, logged in from a new country, and doesn't have multifactor authentication (MFA) is not just risky in isolation; that combination is a hunting lead.


##


Reasoning over entity data


With the entity store providing rich context, we built[Entity analytics AI-hunting leads](https://www.elastic.co/docs/solutions/security/advanced-entity-analytics) . These reasoning modules traverse entity profiles and correlate data across users and hosts to surface patterns that human analysts would find meaningful, if they had the time to look everywhere at once.


These AI-generated hunting leads are automatically surfaced on our Entity analytics home page, ingesting the entity store state to identify combinations that constitute a threat hypothesis. This isn't a simple rule match; it’s a narrative hypothesis grounded in your actual environment.


##


What makes this different


Proactive hunting assistance tools are emerging across the industry. Many are useful tools, but they share a fundamental constraint: They reason over threat intelligence reported information plus event telemetry, not over accumulated entity knowledge.


The difference matters. A query-based approach can find events that match a pattern. An entity-aware approach can find entities that, given everything we know about them, are likely to be involved in something worth investigating. That's a fundamentally richer signal source, and it's one that gets sharper over time as entity history accumulates for what’s been missing in retrohunt features in modern security information and event management (SIEM).


##


Hunting as a continuous discipline


The promise of proactive security is stopping attackers before they reach their objective. Traditionally, the barrier has been analyst capacity. With the rise of AI-driven attacks, this is becoming an impossible task for humans alone.


Entity analytics AI-generated hunting leads don't replace hunters; they multiply them. A senior analyst no longer spends hours figuring out where to look. Instead, they start their shift with a prioritized set of hypotheses that the AI-generated hunting leads already curated. Their time is preserved for what only humans can do: validation, decision, escalation, and response.


##


What's next


Entity analytics AI-generated hunting leads are the first production expression of a broader capability roadmap: an Elastic Security that doesn't wait for you to ask a question. As Entity analytics matures, with expanded entity types such as tracking AI agents, the reasoning surface expands accordingly.


---


*Entity analytics is available in Elastic Security.[Learn more about advanced entity analytics, AI-hunting leads and how the entity store governs user entities.](https://www.elastic.co/docs/solutions/security/advanced-entity-analytics/overview)*


#### Jump to section


- [The hunting gap no one talks about](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#the-hunting-gap-no-one-talks-about)
- [The core problem](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#the-core-problem)
- [Building the foundation: The entity store](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#building-the-foundation-the-entity-store)
- [Reasoning over entity data](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#reasoning-over-entity-data)
- [What makes this different](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#what-makes-this-different)
- [Hunting as a continuous discipline](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#hunting-as-a-continuous-discipline)
- [What's next](https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads#whats-next)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=AI-generated%20hunting%20leads:%20The%20hunt%20starts%20before%20you%20ask%20the%20question&url=https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads&title=AI-generated%20hunting%20leads:%20The%20hunt%20starts%20before%20you%20ask%20the%20question)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/proactive-threat-hunting-ai-generated-leads&title=AI-generated%20hunting%20leads:%20The%20hunt%20starts%20before%20you%20ask%20the%20question)
