---
schema_version: "1.0.0"
document_id: "76970146861d39f8ef9b35e16cad799b782f55a9562a8648f24bd0e42fbf89df"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/07/what-it-takes-to-secure-claude-cowork-across-the-ai-enterprise/"
published_at: "2026-07-07T22:28:27+00:00"
first_seen_at: "2026-07-25T01:08:05.069426+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b0f76763a021c1fcda0f5c7c38e14b336eb2199bdfea1f2d7fb2f6cc2bdc3281"
---

# What It Takes to Secure Claude Cowork Across the AI Enterprise

[Agent Security](https://www.paloaltonetworks.com/blog/ai-security/category/agent-security/)


[AI Security](https://www.paloaltonetworks.com/blog/category/ai-security/)


[Secure GenAI Use](https://www.paloaltonetworks.com/blog/ai-security/category/secure-genai-use/)


You've watched the demos. Whether it's Claude Cowork, ChatGPT Enterprise, GitHub Copilot, Cursor, or internally developed agents, AI systems are no longer answering questions. They are connecting to enterprise data, invoking tools, making decisions, and executing multi-step workflows across applications without human intervention. The capability is real, and organizations are rapidly moving from experimentation to deployment.


*Teams are no longer asking if they should use this, they have accepted agentic tools as the reality. But the board and the infosec team are asking a different question:* ***can this capability be secured and controlled at enterprise scale?*** *Can security teams prevent sensitive company data from being exchanged without oversight?*


[Anthropic built meaningful access controls into Cowork](https://claude.com/blog/cowork-for-enterprise) — role-based permissions, group spend limits, usage analytics and connector restrictions — so the answer is a qualified yes. Those controls handle who can use the tool and what they can connect to, but they don't answer whether a specific action inside a given session is safe. That gap is the one standing between a successful pilot and a successful org-wide rollout.


# The Gap That a Demo Doesn’t Expose


The organization’s admin assigns roles, sets spend ceilings per user group and restricts which connectors have access to write to your database. Anthropic's OpenTelemetry support even lets your team pipe session events into your SIEM. These controls cover real ground, but they operate at the permissions level — answering whether a person is authorized to use the tool rather than whether what's happening inside a session is safe.


Consider what that gap looks like in practice. Let’s consider two scenarios. Your finance analyst has full Cowork access and uploads a quarterly forecast containing unannounced acquisition figures. The access controls confirm she is authorized to use the tool, but nothing evaluates whether that information should be exposed to a model.


*That's an AI data loss prevention risk, and access controls are blind to it.*


The risk becomes greater when agents move beyond information retrieval and begin taking actions.


Let’s say a scheduled Cowork automation is set up to pull weekly competitor pricing from the web. A target site embeds hidden instructions in its page content. The agent, running unattended, reads them as legitimate commands and begins modifying local files and triggering actions your team never authorized. By the time anyone notices, the agent has already acted.


*The first scenario exposes a governance problem because your security team has no visibility into what data is flowing through AI tools across the organization. The second is a runtime security problem as there is nothing evaluating whether an action in progress is safe, regardless of whether the user was authorized to start it. Neither gap is addressed with the predefined controls in Cowork; both need to be solved before you can say yes to Cowork adoption in the whole organization.*


## Why Traditional Controls Break Down


Traditional enterprise software behaves predictably. Access controls work because administrators can reasonably anticipate what an authorized user or application will do once access is granted.


AI systems operate differently. Agents combine models, tools, data sources, and reasoning paths dynamically at runtime. An authorized user may start with a simple request, but the resulting chain of actions may evolve in ways that were never explicitly programmed or anticipated. The challenge is no longer controlling who can access a system. The challenge is securing and governing what happens after access has been granted.


# The Missing Layer is Runtime Security


Anthropic's access controls establish who can use Cowork and what they can connect to. But as the examples above show, they don't protect against what happens inside a session: a finance analyst uploading sensitive acquisition data to the model, or a scheduled automation being hijacked by a malicious instruction embedded in a webpage it was directed to visit.


What organizations working with Cowork need is a layer that enforces data and security controls and gives complete visibility at runtime across all Cowork agents in the enterprise every interaction boundary.


An AI runtime security layer that sits between your teams and the model providers such as Anthropic, AWS Bedrock, Google Vertex or any combination, and evaluates risk in every interaction. It inspects every request, every tool call and detects sensitive data like client names, financial projections, internal pricing and contract terms. It enforces agent identity controls, so every automated action is traceable to a specific workflow and owner.


**Your CISO gets the audit trail and your Infosec team gets the evidence.**


# The AI Enterprise Needs a Control Plane


The CIO needs the observability for all Cowork activity and costs.


An AI control plane allows the CIO to set spending limits per team and use case across every AI tool from a single console.


Procurement asks for a quarterly forecast across all AI spend, and you pull it from one place instead of aggregating reports from four different vendor dashboards. If you need to move providers for cost or compliance, the gateway reroutes traffic without disrupting your teams or breaking your workflows.


Claude Cowork may be where organizations begin scaling their AI journey, but it won't be the only AI tool your teams use. Developers will use coding assistants, business teams will leverage the AI built into SaaS applications and data science teams will deploy custom agents for their workflows. New models, new providers and new workflows will continue to appear.


**The challenge isn't just governing one AI application; it’s governing AI activity across the entire AI enterprise.**


Everyone looks to secure each tool individually: configure Cowork's controls, configure your coding assistant's controls, configure your internal agents separately. But this approach doesn't scale.


This is the sole purpose of the control plane. It sits above individual tools, applications and models and enforces security policies, across every AI interaction.


Prisma AIRS AI Gateway provides that centralised control plane. Organizations that deploy Cowork behind our gateway get runtime security, data protection, agent identity controls, and full visibility, applied consistently, without changing how teams use the tool. The same gateway secures every other AI tool in your environment on the same terms.


Cowork may be where the journey begins, the gateway is what allows it to scale and secure the AI Enterprise.
