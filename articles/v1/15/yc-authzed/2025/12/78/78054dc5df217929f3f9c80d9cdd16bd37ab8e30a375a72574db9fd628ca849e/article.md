---
schema_version: "1.0.0"
document_id: "78054dc5df217929f3f9c80d9cdd16bd37ab8e30a375a72574db9fd628ca849e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/mcp-is-not-secure"
published_at: "2025-12-01T09:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:84aa65f58d21e49934dcb676e3e0124662b9d4efd74f89f03bbf71cffbfe8ea5"
---

# MCP is Not Secure

The[Model Context Protocol](https://modelcontextprotocol.io/) doesn't make your AI applications secure. It's a protocol for connecting AI agents to tools, data sources, and APIs. It standardizes how things connect, not whether they should.


Security? That's left to the implementers.


The 2025 breach timeline suggests many of them aren't getting it right.


## The burden is on everyone


MCP's[official documentation](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) is pretty clear about this. Authorization is described as "optional" and "strongly recommended when your server accesses user-specific data."


The spec walks through OAuth 2.1 flows, token validation, and Protected Resource Metadata. The[November 2025 spec update](https://modelcontextprotocol.io/specification/2025-11-25/changelog) added more authentication improvements: OAuth Client ID Metadata Documents for client registration, incremental scope consent, and OpenID Connect Discovery support. These are the mechanics of proving who a user is. What it doesn't address is what that authenticated user should be allowed to do.


OAuth-based auth flows can include the following:


- Identity providers authenticate who someone is
- Authorization servers mint tokens with scopes
- Resource servers accept tokens and enforce access


MCP servers are defined as resource servers in the spec. They're told to validate incoming tokens and check scopes. But what those scopes actually permit, which tools a user can call, which resources they can access, that's where the spec stops and implementation begins. The spec handles authentication. Authorization, the actual permission decisions, is left entirely to implementers.


For MCP server builders, this means integrating complex authentication protocols, deciding what authorization model fits their use case, and implementing it correctly. For users, it means making judgment calls about which MCP servers to trust, which tools to enable, and what scopes to grant.


Both sides have to get this right.


## The breach timeline speaks for itself


We've been tracking[a timeline of MCP security breaches](https://authzed.com/blog/timeline-mcp-breaches) that now spans tool poisoning attacks against WhatsApp, prompt injection exploits against GitHub's official MCP server, cross-tenant data exposure in Asana, remote code execution via mcp-remote (with over 437,000 downloads), and supply chain compromises in hosted MCP registries.


The common thread isn't sophisticated zero-day exploits. It's authorization failures. Over-privileged tokens, missing access controls, inadequate isolation, and supply chain trust assumptions that didn't hold up.


## The lethal trifecta


Even if every MCP server builder implements perfect OAuth flows and every user carefully reviews their granted scopes, there's a structural problem.


Simon Willison, who coined the term "prompt injection," describes what he calls the["lethal trifecta"](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) for AI agents. When an agent has three capabilities simultaneously, attackers can trivially steal data:


1. **Access to private data** , which is often the entire point of connecting tools in the first place
2. **Exposure to untrusted content** , meaning any mechanism by which attacker-controlled text enters the LLM context
3. **External communication ability** , or any way to exfiltrate information


MCP makes it easy to accidentally create this combination. You connect a document server for search. You add an email tool for sending summaries. You enable web browsing to pull in external context.


Each tool seems reasonable in isolation. Together, they form the conditions for data exfiltration.


The GitHub MCP breach demonstrated exactly this pattern. Prompt injection in a public issue caused a legitimate tool call that leaked private repository contents. No malware or stolen credentials. Just natural language instructions that the AI followed.


Once data enters an LLM's context window, your ability to secure it is functionally gone. The model processes everything together: your private documents, your corporate data, and whatever untrusted content made it through.


This is why permission decisions need to be precise. You want to control exactly what data enters the context window in the first place. And permissions around tool calls need to be checked at execution time, not just when a session starts.


## Why AI alone can't secure AI


One temptation is to solve this with more AI. Train a model to detect sensitive content or build an AI layer that monitors other AI systems for policy violations. These approaches can be valuable parts of a defense-in-depth strategy, but on their own they're incomplete.


AI is probabilistic by design. A 99.9% accuracy rate still leaves a meaningful number of failures, even at just thousands of requests.


AI-based detection can help identify anomalies and flag suspicious patterns. But the core authorization decisions, whether an entity has permission to access a resource, must be deterministic. The answer needs to be "no" or "yes," not "probably not" or "the model is 98.7% confident this should be denied."


## MCP authorization is a moving target


MCP solves a real problem: the challenge of integrating AI agents with the rest of your technology stack. The protocol isn't broken. In one year, MCP went from an experiment to an ecosystem with nearly 2,000 servers in the registry and official implementations from GitHub, Stripe, Notion, Hugging Face, and others. OpenAI, Microsoft, AWS, and Google Cloud have all backed it.


The MCP community also recognizes the security gap. The November 2025 spec release introduced authorization extensions, enterprise IdP controls, and secure credential handling through URL Mode Elicitation. The foundation is being built but organizations deploying MCP today need authorization infrastructure that works now.


Consider how fast things are moving beyond the spec itself. MCP usage has evolved significantly since November 2024. Developers started by running multiple MCP servers locally on dev machines, moved to remote MCP servers in mid-2025 as platforms like[GitHub](https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/) ,[Atlassian](https://www.atlassian.com/blog/announcements/remote-mcp-server) , and[Cloudflare](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/) launched hosted offerings, and by fall 2025 were shifting toward "code mode" approaches.


Cloudflare's[code mode](https://blog.cloudflare.com/code-mode/) is a good example. Instead of exposing MCP tools directly to LLMs, they convert tools into TypeScript APIs and let the model write code against them. More capable, more efficient, and yet another paradigm that authorization strategies need to accommodate.


In this environment, custom authorization code gets scrapped and rewritten with each paradigm shift. Rigid authorization models can't adapt quickly enough.


What you need is authorization infrastructure that can be reconfigured as use cases change, scaled up as your AI applications find traction, and ideally is already deployed for your existing applications.


## What AI-ready authorization looks like


AI doesn't create new authorization problems. It stress-tests authorization differently than traditional applications.


The challenges are ones of magnitude, not kind:


- **More actors.** Agents, tools, and services all need permissions, not just users.
- **More checks.** Every RAG retrieval, every tool call, every document entering a context window is a permission decision.
- **More velocity.** AI use cases evolve weekly. You can't hardcode policies for a landscape that shifts this fast.


The stakes are different too because the potential for harm scales with the number of autonomous actors.


For more actors, you need a permissions model that treats agents and tools as first-class subjects, not afterthoughts bolted onto user-based access control. For more checks, you need low-latency evaluation that can handle high volumes without becoming a bottleneck. For more velocity, you need permissions definitions that can be reconfigured without rewriting code or redeploying services.


AuthZed provides authorization infrastructure that does all of this, and already powers authorization for many AI applications in production.


## Takeaways


-


**MCP is a protocol, not a security solution.** The spec gives you the plumbing for authentication (OAuth flows, token handling, client registration) while leaving the hard authorization decisions in your hands.


-


**The[lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) is the structural reality of how LLMs process information.** Being precise about what data enters the context window matters, and permissions need to be checked at execution time.


-


**AI alone can't secure AI.** Probabilistic reasoning can supplement security, but authorization decisions need to be deterministic.


-


**Proven authorization infrastructure can adapt to rapidly evolving AI paradigms,** scale with your usage, and treat agents and tools as first-class subjects in your permissions model.


On this page


- The burden is on everyone
- The breach timeline speaks for itself
- The lethal trifecta
- Why AI alone can't secure AI
- MCP authorization is a moving target
- What AI-ready authorization looks like
- Takeaways


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. Melissa Smolensky · May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)
