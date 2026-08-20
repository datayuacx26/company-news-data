---
schema_version: "1.0.0"
document_id: "c0c0e031cd58278084e0d26a62c88104e4ec127c9be09cbbfd58b5acc9ea2626"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/the-agent-was-trusted-it-shouldnt-have-been"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-24T16:50:44.532118+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:708c545ccaf60244d7e4ccedbb816f88514a7bb2ea1153ed6e45b13b1a87497d"
---

# The Agent Was Trusted. It Shouldn't Have Been.

*How an AI supply chain breach exposed a governance gap — and what every security leader should do about it.*


A high-profile breach is making headlines, not because of a zero-day attack, but because of something far more fundamental: **misplaced trust in an AI agent** .


The full kill chain has now emerged, and it is a case study in what happens when AI agents operate without governance.


## The Kill Chain


### Stage 1: The initial compromise


A developer at an AI productivity tool company downloaded game-cheat scripts. That single action infected their machine with commodity infostealer malware. Credentials, Google Workspace logins, and cloud service keys were exfiltrated silently.


### Stage 2: The vendor is breached


Using those stolen credentials, the attacker accessed the AI tool vendor's cloud environment and exfiltrated OAuth tokens the vendor held on behalf of its customers. These tokens had been issued to the vendor when users connected their corporate accounts.


### Stage 3: The trust exploit


An employee at a major cloud platform had connected this compromised AI tool to their enterprise Google Workspace account. The permissions granted: **Allow All** . No IT approval. No OAuth governance policy. No inventory of what AI tools held enterprise credentials.


When the vendor was breached, the attacker inherited that employee's enterprise-level Workspace access — not because they hacked the employee's account, but because the employee had already granted trust to an unverified agent.


### Stage 4: Lateral movement at scale


With valid enterprise credentials in hand, the attacker moved into production environments. They enumerated environment variables stored without encryption — variables that were never meant to be sensitive, but became the escalation path. Secrets. API keys. Internal system access. The attacker, described by the company's CEO as *"highly sophisticated and significantly accelerated by AI,"* moved with *"surprising velocity and in-depth understanding of systems."*


### Stage 5: The blast radius


Internal databases, environment configurations, and customer credentials were exposed. The company engaged a top-tier incident response team and later found the stolen data on a breach forum with a $2M asking price.


The root cause was not an advanced persistent threat. Not a zero-day. **Just an unsanctioned AI tool, an over-permissioned OAuth grant, and the absence of governance around the use of AI agents.**


## Two Root Failures Require Two Different Fixes


This breach exposes two distinct failure modes that are playing out simultaneously across enterprises adopting AI at speed.


### Failure 1: Ungoverned agents


Enterprises have an identity and access management (IAM) problem with AI agents that traditional IAM was never designed to solve. Traditional access control asks: *Who or what are you?* It validates humans and machines and enforces assigned permissions. It does not look at intent or context.


AI agents are neither humans nor machines. They act autonomously, at machine speed, continuously — and execute decisions often without human review at each step. The context can be unpredictable. An agent that holds an OAuth grant with enterprise-wide scope is not an authenticated human, nor a server or application acting on a predetermined set of instructions. It is autonomous software operating with human-level trust grants, but with digital speed and unpredictable paths to its goals.


This attack didn't target the human. It targeted the *agent* the human trusted, then walked through the front door using credentials the agent already held — and abused them for lateral access, exploitation, and compromise.


The authentication layer we need for the agentic era is fundamentally different. The question is no longer *who are you?* It is: **What code is this agent running? What policies govern its actions? Can you prove it cryptographically, in the most secure way possible?**


This is what[Anjuna AI Overwatch](https://www.anjuna.io/blog/confidential-computing-critical-enabler-ai-first-cios-2026) addresses. Before an AI agent is permitted to act — to use tools, access data, trigger enterprise workflows, or even engage its own LLM — its intentions, context, and actions must be evaluated. Not just against static rules, but through intelligent reasoning. Instead of agents using credentials directly, every access is evaluated to stay governed and on track. And not through a software assertion that can be spoofed or poisoned, but through hardware-rooted attestation. A Trusted Execution Environment (TEE) cryptographically isolates the supervisor, providing an integrity guarantee for its policies, prompts, and models. The result is an incorruptible operating environment, isolated from agents and attackers alike. Agents cannot override it. Attackers cannot bypass it.


The result: **an incorruptible supervisory layer to govern, direct, control, and manage agentic AI** . Shadow AI tools an employee adopts without IT approval cannot pass attestation. Agents with unauthorized policy modifications cannot operate. The ungoverned agent problem is structurally closed.


### Failure 2: No blast-radius containment


The second failure is architectural. When the attacker obtained valid OAuth credentials, they gained near-unrestricted lateral movement across production environments. Environment variables were readable in plaintext through standard API access. Nothing in the environment enforced the principle that a valid, authenticated session should only be able to reach what it actually needs.


This is the gap that confidential computing closes at the data-in-use layer — the layer that traditional controls leave exposed.


With[Anjuna Seaglass](https://www.anjuna.io/product/seaglass) , the compute environments that agents operate in — and the secrets they hold — are isolated inside hardware-enforced TEEs. Data processed inside an enclave is encrypted in memory. Even an attacker holding valid credentials cannot read operating code or data in memory, access plaintext secrets, or pivot across isolation boundaries. Encryption is enforced by the hardware itself, not by software policies a sophisticated attacker can enumerate around. Anjuna Seaglass enforces policy over the data flowing in and out from *within* the enclave. There is no "soft underbelly" to attack as there is in traditional compute infrastructure.


Combine this hardened runtime with policy enforcement that governs what a given application or agent can access — on what terms, under what conditions — and you have a fundamentally different blast-radius model. Not *"assume breach and detect."* But: **even at breach, limit what can be reached.**


## The New Security Primitive for the Agentic Enterprise


Today's trust model must evolve to protect enterprises from attacks *through* AI agents. That evolution comes down to two requirements:


The breach everyone is talking about was not inevitable. It was the outcome of deploying agentic AI without the governance layer that agentic AI requires.


The good news: **that governance layer exists today.**


**Read how**[Anjuna AI Overwatch](https://www.anjuna.io/solution/agentic-ai) **and**[Anjuna Seaglass](https://www.anjuna.io/product/seaglass) **address both failure modes.**


*Anjuna Security builds the Universal Confidential Computing Platform — hardware-rooted trust for AI agents, sensitive workloads, and the enterprises that run them.*[Try Anjuna Seaglass free for 30 days](https://www.anjuna.io/anjuna-free-trial) *on AWS, Azure, or Google Cloud.*
