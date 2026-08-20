---
schema_version: "1.0.0"
document_id: "b376d34ce61f48080000ec411a8e212e20a228f18525b39c704717ff41cbe6c6"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/claude-in-microsoft-foundry-is-now-generally-available/"
published_at: "2026-06-29T17:00:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:310519be9baefd7174cc426543d5375e1bbdaa8598e67d406d682a12d595ec1e"
---

# Claude in Microsoft Foundry is now generally available

***Claude in Microsoft Foundry is the production path enterprises have been asking for: true frontier model choice, Azure-native controls, simplified procurement, and faster time to value.***


---


Most enterprise AI projects do not stall because of model quality. They stall because of everything around the model: procurement, governance, networking, and data. Claude in Microsoft Foundry is now generally available, hosted on Azure, giving teams a faster path from agent experimentation to production.


Enterprises can build with Claude through[their existing Azure account](https://ms.portal.azure.com/auth/login/) , using the authentication, billing, networking, governance, and data controls their teams already trust. Instead of solving for infrastructure, teams can focus on building agentic applications that run their work with Claude, in the environment where they already operate.


This is a real step forward for customers building agentic applications and want to move from AI experimentation to production.[Claude brings leading capabilities](https://claude.com/partners/microsoft-foundry) for coding, agentic workflows, and complex reasoning. Microsoft Foundry brings the enterprise harness to build, evaluate, deploy, and scale those agents on Azure. Together, they give teams a trusted path to production AI with frontier model quality and the Azure controls they already trust.


Today’s announcement builds on[the strategic partnership Microsoft, NVIDIA, and Anthropic announced in November 2025](https://blogs.microsoft.com/blog/2025/11/18/microsoft-nvidia-and-anthropic-announce-strategic-partnerships/) to expand enterprise access to Claude on NVIDIA accelerated computing. Claude runs on NVIDIA Blackwell Ultra systems, connected by InfiniBand networking, bringing the rack-scale AI infrastructure designed for inference performance and efficiency.


## **Build with Claude through your Azure account**


Developers can access Claude through the Messages API and use core capabilities including prompt caching, extended thinking, and tool streaming. For teams building agents,[Foundry Agent Service](https://azure.microsoft.com/en-us/products/ai-foundry/agent-service/) uses Claude as the reasoning core to orchestrate multi-step planning, tool use, and task execution across enterprise systems.


Inference is processed in Azure, and customers can choose between Global and US data zones, for teams with data residency requirements. Anthropic operates the inference and is the data processor and SLA provider. Because Claude is available natively through Foundry, teams can work inside the Azure environment they already use. They can authenticate with[Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/) , apply Azure role-based access controls, manage access through existing governance policies, and track usage through familiar Azure management experiences.


For high-sensitivity workloads,[zero data retention](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy?tabs=azure-portal) is also available, so prompts and completions are not retained by Anthropic after the API call completes. For commercial teams, it also simplifies how Claude is purchased and consumed. Claude usage is billed in Claude Consumption Units (CCU), a single, consolidated line on your Azure bill, with MACC drawdown and per-model detail in Foundry unchanged.


For many enterprises, that matters as much as model capability. The barrier to production isn’t only whether a model is powerful enough, it’s whether teams can procure it, govern it, secure it, and operate it at scale inside their existing cloud. With Claude in Foundry, they get frontier capabilities in an Azure environment that aligns with enterprise requirements for security, compliance posture, governance, and data residency.


> Running Anthropic’s models on Azure has given us the sustained throughput and reliability our enterprise customers expect. The combination of frontier model quality and enterprise-grade infrastructure is what makes Bolt viable for the Fortune 500.
>
>
> —Gary Ballabio, Vice President, Partnerships, Bolt


## **Customers are already building with Claude in Foundry**


Enterprises aren’t just running isolated pilots; they’re building production systems and agents that need throughput, reliability, governance, security, and scale.


> At NVIDIA, we use autonomous AI agents every day to help our teams move faster and think bigger. Anthropic’s Claude models bring strong reasoning, coding and enterprise capabilities that are valuable for complex technical work. With Claude now available in Microsoft Foundry running on NVIDIA GB300 GPUs, more organizations can run advanced, specialized AI agents with the performance, scale and security needed for production.
>
>
> —Justin Boitano, Vice President and GM of Enterprise Computing, NVIDIA


> Our customers describe their tests in plain English, and Momentic runs through the interface to verify everything works before a release ships. We found Claude’s Opus models especially suited to this, and running them on Microsoft Foundry we now serve millions of tokens per minute with the reliability our customers depend on.
>
>
> —Jeff An, Co-Founder and CEO, Momentic


## **Built for coding, agents, and complex reasoning**


[Claude models](https://ai.azure.com/catalog/publishers/anthropic) are especially well-suited to some of the fastest-growing enterprise AI workloads. For software teams, Claude supports code generation, refactoring, debugging, test creation, and large-scale development workflows. For teams building agents, it powers multi-step reasoning, tool use, planning, and task execution. For business teams, it supports document-heavy analysis, research synthesis, and complex decision support.


In Microsoft Foundry, these capabilities connect to the broader[Azure ecosystem](https://azure.microsoft.com/en-us/products/) . With Foundry Agent Service, teams orchestrate multi-step, goal-driven agents that use Claude as their reasoning core, planning, calling tools, and executing tasks across enterprise systems. Features like **[model router](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/model-router)** enable customers to automatically route queries to the most appropriate Claude model, **saving up to 50% while improving user satisfaction** . All this governed and monitored by[Foundry Control Plane](https://learn.microsoft.com/en-us/azure/foundry/control-plane/overview) which continuously runs evaluations to ensure agent responses match customer expectations, even blocking responses that violate rules before they reach users.


> Between Anthropic and Azure, we get the best capabilities in the world and we get the best security in the world. And that’s exactly what nuclear needs. It’s how we compressed a safety analysis that would have taken 200 human days into a single day.
>
>
> —Matt Huang, Founding Product Lead, Everstar


And with[Microsoft IQ](https://www.microsoft.com/en-us/ai/microsoft-iq) , agents have access to live enterprise context which radically improves value per token, and helps Foundry amplify the impact customers can have: tools like **agent optimizer in Foundry Agent Service** tune the prompts which define agents so they perform better regardless of what model is under the hood.


## **A stronger foundation for enterprise AI on Azure**


The next phase of enterprise AI will be defined by production systems: coding agents, business process agents, research assistants, customer-facing applications, and domain-specific workflows that operate reliably at scale. That takes more than access to a model. It takes a platform.


With[Claude now generally available in Microsoft Foundry](https://aka.ms/ClaudeGAdocumentation) and hosted on Azure, customers can build with Anthropic’s leading models, orchestrating them as agents with Foundry Agent Service and grounding them in enterprise knowledge with Microsoft IQ, while using the Azure controls, commitments, and infrastructure they already trust.


## Try Claude in Foundry today


Build with Anthropic’s leading models in the Azure ecosystem you know and trust.


[Get started](https://ai.azure.com/catalog/publishers/anthropic)
