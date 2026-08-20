---
schema_version: "1.0.0"
document_id: "11e7c824d82310081089d196f27a7ccf1612904be93a8b48a2162428c9df7549"
company_key: "a10-networks-inc-common-stock"
company: "A10 Networks Inc."
source_id: "a10-networks-inc-common-stock-rss-284845abe605"
canonical_url: "https://www.a10networks.com/blog/owasp-two-ai-top-10/"
published_at: "2026-07-28T00:11:57+00:00"
first_seen_at: "2026-07-28T23:22:33.677695+00:00"
fetched_at: "2026-07-28T23:22:35.175024+00:00"
content_hash: "sha256:4749b768a2013425a07583a41ed022bc87641039402c5ff8aead39eaf18d8fa1"
---

# OWASP Now Has Two AI Top 10s: What Changed?

In 2023, when OWASP introduced the Top 10 for[LLM Applications](https://www.a10networks.com/blog/llm-security/) , it was exactly what the industry needed. Suddenly we had a common language for AI risk.


- Prompt injection
- Sensitive information disclosure
- Insecure output handling
- Training data poisoning
- Overreliance on model output


These weren’t abstract fears anymore — they were categorized, named, and made actionable.


At the time, most[“AI applications”](https://www.a10networks.com/solutions/ai-application-security/) were exactly that: applications wrapped around a large language model. These included a chat interface, a summarizer, a content generator, and maybe a retrieval pipeline bolted on. The model generated text. A human read it. The blast radius was mostly informational. The LLM Top 10 was built for that world, but we don’t live in that world anymore.


## Key Takeaways


- OWASP now maintains two separate AI frameworks: the LLM Top 10 secures the model layer, and the Agentic Top 10 secures the execution layer.
- The LLM Top 10 protects the integrity of reasoning, while the Agentic Top 10 addresses what an autonomous system is allowed to do once output becomes action.
- Authority is the dividing line, since a standalone LLM only generates suggestions, but an agent runs with credentials, API keys, and system permissions that an attacker can inherit.
- Modern AI systems require both frameworks because agentic systems introduce new risks like memory poisoning, cross-session contamination, and durable malicious goals that model-centric categories don't capture.


## The OWASP Framework for Agentic Applications


In December 2025, OWASP released a second framework: the Top 10 for Agentic Applications (2026). It didn’t merge it into the LLM list or just add a few new categories. They created a separate document entirely. That decision is the point: the LLM Top 10 secures the model layer, and the Agentic Top 10 secures the execution layer. They are different things.


The LLM Top 10 is concerned with the integrity of reasoning. Can the model be manipulated through prompt injection? Can it leak secrets through generation? Can it produce output that downstream systems mishandle? Can we over-trust what it says? These are real risks, and they haven’t gone away. Every agent still contains an LLM. Every autonomous system still inherits those vulnerabilities.


An agent, however, is not just a model that talks. An agent plans, calls tools, and reads and writes data. It uses credentials, persists memory, executes workflows, and it operates across systems. In this scenario, when output becomes action, the threat model expands.


Prompt injection is a perfect illustration. In a basic LLM application, prompt injection might cause the model to produce harmful or misleading text. That’s serious but bounded. In an agentic system, prompt injection can redirect a workflow,[trigger unauthorized API calls](https://www.a10networks.com/solutions/security/api-protection/) , exfiltrate data through tool usage, or persist malicious instructions into memory for future tasks. The root vulnerability is the same, but the consequences aren’t.


The LLM Top 10 was written for systems where output was the end of the chain. The Agentic Top 10 exists because output is now the beginning of one. Authority is the dividing line. A standalone LLM does not have authority. It generates suggestions. An agent often runs with service accounts, API keys, database access, internal system permissions. If it is compromised, the attacker doesn’t just get text. They inherit delegated power.


## The New Problem: Identity, Access Control and Runtime Enforcement


That is no longer a model security problem. That is an identity, access control, and runtime enforcement problem, driven by a probabilistic decision engine.


Persistence is another fault line. Traditional LLM interactions are largely stateless. Agentic systems maintain memory across steps and sessions. They store context, retrieve documents, and accumulate state. That creates new risks: memory poisoning, cross-session contamination, durable malicious goals, to name a few. These aren’t neatly captured by model-centric categories because they emerge from long-lived execution, not single prompt-response interactions. Time itself becomes an attack surface. The LLM Top 10 largely evaluates discrete interactions. Agentic systems operate over hours, days, sometimes continuously. They retry tasks and escalate when blocked. They chain decisions. Security can’t just inspect inputs and outputs anymore. It must monitor behavior across sequences of actions.


---


[Explore the Product: TrojAI by A10 Networks](https://www.a10networks.com/products/trojai-by-a10/)


---


## The Two OWASP Frameworks are Necessary


If you’re building a chatbot that generates summaries, the LLM Top 10 may be enough. You’ll focus on prompt injection defenses, output filtering, data handling controls, supply chain scrutiny. That’s appropriate.


If you’re deploying an AI system that can send emails, move money, modify infrastructure, create tickets, or access enterprise systems autonomously, you are in Agentic Top 10 territory. The mistake is assuming one replaces the other.


The Agentic Top 10 sits on top of the LLM Top 10. Every agent inherits model risk, but not every model integration introduces execution risk. Think of it as two layers of the same stack. The LLM Top 10 protects the cognition layer: how the system thinks, what it reveals, and how it can be manipulated. The Agentic Top 10 protects the agency layer: what the system is allowed to do, how actions are constrained, how authority is enforced, and how behavior is observed over time.


Modern AI systems require both because modern AI systems both think and act.


OWASP created both lists because the architecture changed. And when the architecture changes, the threat model follows. If your security strategy hasn’t caught up to that split, you’re still defending a chatbot while deploying an autonomous operator. That gap is where the real risk lives.


---


## FAQs


Released by OWASP in December 2025, the Top 10 for Agentic Applications (2026) is a separate framework securing the execution layer of autonomous AI systems. Rather than merging with the LLM list, it addresses risks that emerge when agents plan, call tools, use credentials, and act across systems. It focuses on identity, access control, and runtime enforcement rather than model behavior alone.


The LLM Top 10 protects the cognition layer, covering how a system thinks, what it reveals, and how it can be manipulated. The Agentic Top 10 protects the agency layer, covering what a system is allowed to do, how actions are constrained, and how authority is enforced. In short, one secures reasoning and the other secures action.


OWASP created two lists because the architecture changed, and when architecture changes, the threat model follows. A standalone LLM generates text where output is the end of the chain. An agent turns output into action, running with service accounts and system permissions. These are fundamentally different security problems, so the Agentic Top 10 sits on top of the LLM Top 10 rather than replacing it.


It depends on your system. If you’re building a chatbot that generates summaries, the LLM Top 10 may be enough, focusing on prompt injection defenses and output filtering. If you’re deploying an AI system that can send emails, move money, or access enterprise systems autonomously, you’re in Agentic Top 10 territory. Modern AI systems that both think and act require both frameworks.


### Share this post:


[Share on X (Twitter)](https://twitter.com/intent/tweet?text=OWASP%20Now%20Has%20Two%20AI%20Top%2010s%3A%20What%20Changed%3F&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fowasp-now-has-two-ai-top-10s-what-changed%2F)[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fowasp-now-has-two-ai-top-10s-what-changed%2F)[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fowasp-now-has-two-ai-top-10s-what-changed%2F&title=OWASP%20Now%20Has%20Two%20AI%20Top%2010s%3A%20What%20Changed%3F&source=https%3A%2F%2Fwww.a10networks.com)Share on Email


Categories:


[A10 News](https://www.a10networks.com/blog/category/a10-news/)[AI](https://www.a10networks.com/blog/category/ai/)


---


#### Related Resources


1. [Top 9 Generative AI Security Risks in 2026](https://www.a10networks.com/blog/generative-ai-security-risks/)
2. [DDoS Threat Map Shows Global Distribution of Top Amplifier Weapons and Bots](https://www.a10networks.com/blog/ddos-threat-map-shows-global-distribution-of-top-amplifier-weapons-and-bots/)
3. [Top Cyber War Techniques and Technologies](https://www.a10networks.com/blog/top-cyber-war-techniques-and-technologies/)


Previous Post


[A10 Joins OpenAI’s Trusted Access for Cyber](https://www.a10networks.com/blog/a10-openai-trusted-access-cyber/)


---
