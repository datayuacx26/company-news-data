---
schema_version: "1.0.0"
document_id: "0337c2f79d945e470ad18db2890fa51d52808bdd88bd27c8792b7809739a6976"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/ai/ai-agent-identity-evaluation/"
published_at: "2026-08-14T07:00:00+00:00"
first_seen_at: "2026-08-15T19:52:53.694008+00:00"
fetched_at: "2026-08-15T19:52:55.500456+00:00"
content_hash: "sha256:dfe8dbbf58d5601cd7d816e9d8ebfca55935b446a170ca77c1b911e025776913"
---

# How to evaluate an AI agent identity layer

### Topics


---


AI Agents


,


Non-Human Identities


,


IAM


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


Key takeaways


Evaluating an AI agent identity layer requires looking beyond broad “agent security” RFPs.


Our Agent Identity Evaluation Kit provides a checklist, RFI template, and scoring matrix to help you evaluate identity security vendors.


To govern autonomous systems effectively, organizations must evaluate identity separately from gateways and observability tools by answering three core questions: Where are my agents? What can they connect to? What can they do?


## The challenge with current agent security RFPs


Most organizations are building governance for AI agents while they're still learning how AI agents work. There's no 10-year backlog of institutional knowledge to draw on. The market is inventing new categories faster than anyone can build a stable mental model of it. One consequence is that organizations issue overly broad "agent security" requests for proposals (RFPs), grading vendors across different categories on the same baseline.


Some of this confusion comes from using generative AI tools to draft RFPs: The tool has read a market where "agent security" is the loudest phrase, so it drafts questions shaped by whichever vendor's content it saw most: a gateway's marketing site, a startup's category-creation blog post, or an analyst brief using outdated taxonomy. The AI tool returns generic results because the training data it's drawing from hasn't settled on how to talk about the agent security landscape yet.


## The seven layers of the AI agent security stack


Modern agent security is made up of several distinct operational layers, each serving a specific role:


- **Model and prompt security:** Scans inputs and outputs for prompt injection and data leakage
- **Gateways:** Manages traffic routing and enforces API-level execution policies
- **Observability:** Provides post-execution tracing, logging, and evaluation of agent behavior
- **Runtime and posture monitoring:** Performs anomaly detection across active agent sessions
- **Non-human-identity vaults:** Manages secret storage and static credentials
- **Identity and access management (IAM):** Handles discovery, registration, ownership, and lifecycle governance
- **Authorization engine:** Executes fine-grained, relationship-based access decisions


Most organizations will end up buying more than one specialized tool across these layers. However, the identity layer needs its own dedicated evaluation framework, separate from your evaluation of other agent security tools. The identity layer is the system that knows who each agent is, what each agent is allowed to touch, and what each agent actually did across every platform the agent runs on, for its entire life.


The Open Worldwide Application Security Project’s (OWASP) GenAI Security Project publishes its own[State of Agentic AI Security and Governance report](https://genai.owasp.org/resource/state-of-agentic-ai-security-and-governance/) that maps the landscape for securing and governing autonomous AI systems.


## The blueprint for the secure agentic enterprise


We built the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) after hundreds of customer conversations about how they're actually securing agents today, where those efforts break down, and what they wish they'd asked their vendor before they made the purchase. It's the framework for how organizations answer the identity questions that agent deployment raises before those agents scale beyond control. We use it with customers before we ever talk product.


The blueprint revolves around three questions:


1. Where are my agents?
2. What can they connect to?
3. What can they do?


These questions are a great baseline for evaluating vendors for your own identity layer.


### 1. Where are your AI agents?


To maintain governance over your AI ecosystem, you need a single, cross-platform registry that assigns a named human owner to every agent, no matter where it was built.


When evaluating a registry solution, make sure to ask these critical questions:


- **Data sources:** How does it discover agents? (For example, platform APIs, network traffic, browser telemetry, or manual entry?)
- **Scale:** What is the highest number of agents and the largest single customer environment it has managed?
- **Battle testing:** Has it actually run in live production, and for how long?


### 2. What can they connect to?


AI agents need to connect across Model Context Protocol (MCP) servers, SaaS applications, databases, and other agents. However, that access must be scoped, short-lived, and explicitly tied back to the underlying human or system initiating the action.


When evaluating connection security, make sure to ask:


- **Credential lifetimes:** What is the maximum credential lifetime in a default configuration?
- **Scope granularity:** Is access scope bound per-tool, per-resource, or per-action?
- **Token verification:** Can the vendor demonstrate a live end-to-end token exchange?


### 3. What can they do?


Effective governance requires enforcing security policies before an agent executes an action, not just logging the damage after the fact. Furthermore, because an agent acts on behalf of a human or a system, protection must include detecting compromised credentials before an attacker can hijack a powerful agent.


When evaluating action controls, make sure to ask:


- **Enforcement timing:** Does policy enforcement sit directly in the request path (pre-execution), or does it merely detect and alert after the fact?
- **Universal revocation:** Can access be revoked across connected systems?
- **Anomaly detection:** For agents operating on behalf of human users, can the system detect underlying account anomalies—such as impossible travel, new devices, or abrupt pattern shifts—to prevent hijacked credentials from compromising the account?


An identity layer has to answer all three at once, for every agent, for its entire life. Some vendors cover one piece of this well: a registry, a credential vault, or discovery. Ask these vendors what happens at the edge of what their product does: Does it hand off cleanly to something else, or leave a gap?


## Evaluating vendor track record and scale


Managing AI agent identity is a young problem, but identity security itself isn't, and a vendor's history of securing human and machine identities at scale is evidence of how they'll handle agents.


When evaluating vendors, ask them about these qualifications:


- **Multi-identity experience:** How long has the vendor been securing identity broadly—human, workload, and now agent—and at what scale?
- **Battle-tested edge cases:** Has the vendor managed large, complex environments long enough to face failure modes?
- **Platform neutrality:** Is their cross-platform integration proven over years, or is it an untested market claim?


## Get the buyer’s checklist


We built an evaluation kit that helps you evaluate identity security vendors using our operational blueprint for securing AI agents:


- A one-page checklist
- A request for information (RFI) template with response fields for every vendor to complete
- A requirements matrix for scoring a bake-off side-by-side


[Get the Agent Identity Evaluation Kit](https://www.okta.com/resources/datasheets/the-buyers-checklist-evaluating-an-identity-layer-for-ai-agents/)


Every question in the RFI template maps directly to the standard it's grounded in, so you can see the reasoning behind each question, including:


- [OWASP's Top 10 for Agentic Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications)
- [Cloud Security Alliance's Agentic Trust Framework](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents)
- [SACR's Runtime Security for AI Agents](https://softwareanalyst.substack.com/p/runtime-security-for-ai-agents-an)


## Frequently asked questions


###


Because an identity RFP has to test something those RFPs weren't designed to ask about: whether a system knows every agent's identity, access, and actions across their entire lifecycle, on every platform they touch. Evaluate identity separately, around those three questions, rather than folding it into a broader agent-security RFP.


###


Broadly:


- **Model and prompt security:** Scanning inputs and outputs for injection and leakage
- **Gateways:** Traffic routing and policy execution
- **Observability:** Tracing and evaluating agent behavior after the fact
- **Anomaly detection:** Runtime and posture monitoring (anomaly detection)
- **Credential and secret management:** Non-human-identity vaults
- **Identity and access management:** Lifecycle-wide governance, including discovery, registry, and revocation
- **Authorization engines:** Fine-grained, relationship-based access decisions


Most enterprises end up buying more than one. This post covers the IAM layer, which governs agent identity, discovery, ownership, and cross-system access; the others need their own evaluation criteria, not covered here.


###


Discovery provides real value on its own, and for many organizations, visibility is the biggest gap. However, finding an agent is only step one. Problems arise when teams assume a discovery tool will also handle post-discovery needs like scoped access and runtime enforcement. Before buying, ask discovery vendors directly: "Once you find an agent, does your product actually enforce policies and govern access, or do you hand that off to another tool?"


###


Look at their track record, not just the current feature list.


How long has this vendor been securing identity—human, workload, and now agent—and at what scale?


Is their neutrality across platforms a proven pattern over years, or a new claim from a vendor that hasn't had to pick a side yet?


Then ask for the agent-specific version of the same evidence: the largest number of agents, including spawned sub-agents, running in production today, for how large a customer, and a named example of the same policy enforced across the different platforms you actually run.


###


Start with three questions the vendor has to answer at once:


1.


**Where are my agents:** A registry with named ownership and a stated data-source list


2.


**What can they connect to:** A stated credential time-to-live (TTL) and scope-binding method, not just "scoped and short-lived"


3.


**What can they do:** Pre-execution enforcement versus after-the-fact detection, plus detection of anomalous behavior in the human account that an agent acts on behalf of


Add the vendor's track record of securing identity broadly, proof of scale, and cross-platform enforcement.


Then add the parts that an identity RFP shares with any enterprise software purchase:


- Compliance certifications relevant to your industry
- Data residency
- Service-level agreement (SLA) and failure behavior if the identity layer itself goes down
- Compatibility with your existing identity provider
- Pricing structure as agent count grows
- Time to production for a deployment of your size


Fortunately, you don’t need to start from scratch.[Get the buyer's checklist for evaluating an identity layer for AI agents.](https://www.okta.com/resources/datasheets/the-buyers-checklist-evaluating-an-identity-layer-for-ai-agents/)


*These materials are intended for general informational purposes only and are not intended to be legal, privacy, security, compliance, or business advice. © 2026 Okta, Inc. and/or its affiliates.*


About the Author


[Mallory Sword Glenn Director, Okta for AI Agents Product Marketing Mallory Sword Glenn leads Product Marketing for Okta for AI Agents, building go-to-market strategy for a solution that helps enterprises see, manage, and govern agent identities. She's shaping how the market approaches AI agent security, a space with no established playbook. Working directly with customers, she's defining positioning, messaging, and sales strategy as organizations move from experimenting with agents to deploying them at scale. Previously at Adobe and HPE, she built GTM, pricing, and deal strategy for emerging product categories across sales, marketing, and strategy teams.](https://www.okta.com/blog/author/mallory-sword-glenn/)
