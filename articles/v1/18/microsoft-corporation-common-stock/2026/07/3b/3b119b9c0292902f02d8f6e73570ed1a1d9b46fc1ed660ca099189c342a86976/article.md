---
schema_version: "1.0.0"
document_id: "3b119b9c0292902f02d8f6e73570ed1a1d9b46fc1ed660ca099189c342a86976"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/gpt-5-6-now-available-in-microsoft-foundry/"
published_at: "2026-07-09T17:00:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:6c082823a4e1ea25d772c4305a830e24968872f5b507d035e4bcff6420528e3c"
---

# GPT-5.6 now available in Microsoft Foundry

**What’s new today**
GPT‑5.6 is generally available in Microsoft Foundry, alongside the Asia-Pacific Data Zone, and hosted agents in Foundry Agent Service. SeeGPT-5.6 pricing .


---


AI only creates value when it shows up in real systems—systems that are reliable, observable, and aligned to business outcomes. More than 100,000 organizations are already building on[Microsoft Foundry](https://ai.azure.com/home) , and companies like **Adobe, Telefónica, and Tata Consultancy Services** are running agents in production today.


At Microsoft Build, we laid out a simple promise for the agentic era: developers should be able to build an agent where they already work, run it on infrastructure they can trust, and put it in front of the people who need it— without stitching together disconnected platforms. Today, that vision moves from roadmap to reality with three sets of updates **now** **generally available** in Microsoft Foundry:


- **OpenAI’s latest frontier model series** : GPT-5.6 Sol, GPT-5.6 Terra, and GPT-5.6—each tuned to a different workload, available in Standard Global and Standard Data Zones.
- **Asia-Pacific Data Zone** , giving APAC customers a regional option to run frontier OpenAI models while keeping data processing within the region.
- **Production agents** in Foundry Agent Service, with **hosted agents** , **toolboxes** , and **publishing to Microsoft 365 Copilot and Microsoft Teams** .


Together, these capabilities bring frontier models, production agent runtime, enterprise-grade identity, security, and compliance controls, and distribution across Microsoft 365 into a single platform—helping organizations move from experimentation to production without assembling disconnected tools and services.


## Why Foundry is the best agent platform


[Microsoft Foundry](https://ai.azure.com/home) is Microsoft’s end-to-end platform for building, running, governing, and distributing AI agents. Foundry brings together the capabilities organizations need to move agents into production across three pillars:


- **Build** : Open and flexible across models and frameworks.
- **Generate** : Connected to enterprise data, tools, and users.
- **Govern** : Secured, managed, and optimized for long-term value.


These pillars come together in the latest Foundry updates, helping organizations build, run, and scale production agents on a single platform.


## Build with any framework and model on the industry’s end-to-end AI platform


Agent development starts where developers already work—in GitHub Copilot and Microsoft Visual Studio (VS) Code—with the[Foundry Toolkit for VS Code](https://code.visualstudio.com/docs/intelligentapps/overview) and the[Foundry skill](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/use-microsoft-foundry-skill?tabs=vscode%2Cfoundry-skill-alone) handling deployment to Foundry. Whether teams build with[Microsoft Agent Framework](https://github.com/microsoft/agent-framework) ,[GitHub Copilot SDK](https://docs.github.com/en/copilot/how-tos/copilot-sdk/getting-started) (generally available), or Claude Agent SDK, Foundry is the production destination—and it all starts with the right model.


### Start with the right model for the right job


An agent is only as capable as the model reasoning behind it. Microsoft Foundry gives organizations access to industry-leading frontier, open-source, and task-specific models through a single platform, allowing teams to choose the right model for every workload.


Today, we’re making OpenAI’s GPT-5.6 series generally available in Microsoft Foundry Models and Microsoft Foundry Agent Service:


- **GPT-5.6 Sol** delivers the most advanced reasoning capabilities yet, supporting extended reasoning, agentic workflows, and code-focused scenarios, for the most demanding enterprise workloads.
- **GPT-5.6 Terra** is a balanced model for everyday work, delivering performance competitive with GPT-5.5 at a lower cost, making it ideal for scaling intelligent applications across the enterprise.
- **GPT-5.6 Luna** is the fastest and most affordable model in the family, making it well suited to high-volume, latency-sensitive workloads.


Together, the GPT-5.6 series gives organizations the flexibility to match model capability, cost, and performance requirements to specific business scenarios, rather than forcing every workload onto a single model.


Customers consistently tell us that access to new models matters as much as model quality. That’s why we are making **GPT-5.6** —the latest model—available through **Global Standard and Global Priority Processing for all the existing 28 global regions, Data Zones Standard, and Global Provisioned from day one** . This enables customers to adopt the latest frontier AI innovations where they have already built, deployed, and scaled their applications.


### GPT-5.6 pricing for Sol, Terra, and Luna


The following table outlines GPT-5.6 pricing for Sol, Terra, and Luna in Microsoft Foundry. Use this pricing information to compare model options and plan deployment costs:


Model Deployment Pricing (USD $/million tokens)


**Input** **Output**


**GPT-5.6 Sol** Standard Global 5.00 30.00


**GPT-5.6 Terra** Standard Global 2.50 15.00


**GPT-5.6 Luna** Standard Global 1.00 6.00


### Run frontier AI where your business operates


More models, in more regions, are only half of what platform expansion means. The other half is more places to run them compliantly. This is exactly what the Asia-Pacific Data Zone delivers. Today we’re also announcing the **general availability of the Asia-Pacific (APAC) Data Zone for Microsoft Foundry** , enabling APAC customers to run frontier OpenAI models while keeping data processing within the Asia-Pacific regions, with no separate environment to stitch together and no waiting for capability to catch up.


With Global, Data Zone, and Regional deployment options available in Foundry, organizations can align AI adoption with their sovereignty, compliance, performance, and scale requirements while maintaining a consistent development and operations experience across environments.


> As financial institutions adopt AI, responsible data handling becomes foundational to trust. Microsoft Foundry’s APAC Data Zone allows us to keep data processing regionally anchored while accessing advanced AI models at scale. This gives us the confidence to accelerate AI innovation responsibly and reinforces our ambition to be a leading AI-powered financial platform in Asia.
>
>
> —Hongsoo Kim, Chief Data and AI Officer (CDAO), Viva Republica (Toss)


## Generate impact with action-oriented, context aware agents


A capable model is only the starting point. To put one to work in production, an agent also needs somewhere to run, knowledge of your business, governed access to its tools, memory that carries across interactions and acts on real-world events, and a path to the people who use it. Foundry provides each of these as a built-in capability, designed to work together.


- **Where it lives:**[Hosted agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents) **in Foundry Agent Service is now generally available,** giving developers one production runtime for agents built with any framework and harness—Microsoft Agent Framework, GitHub Copilot SDK, LangGraph, OpenClaw, Hermes, and others. It’s enterprise-ready on day one: **Network isolation with Microsoft Azure Virtual Network (VNet) integration** keeps agent traffic inside your security boundary. For long-running workloads, the new **resilient task support in hosted agents (**[private preview](https://aka.ms/ResilientAgent/PrPr) **)** makes it easier to build agents that survive failures. The platform provides primitives to keep the sandbox running, your harness provides checkpointing, and together they allow an agent to resume when it restarts. The result: multi-turn conversations, reasoning loops, and human-in-the-loop approvals can pick up where they left off, without developers having to build their own recovery, retry, and state-management.
- **How it talks:**[Hosted agents with Voice Live](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-voice-live-hosted-agent-integration) **is now generally available.** Developers can add real-time voice experiences to the agents they built with the frameworks they prefer, using the Azure VoiceLive SDK.
- **What it knows** : Foundry **** gives agents access to enterprise knowledge without requiring developers to build a complex retrieval pipeline from scratch. Microsoft IQ brings together[Work IQ](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/work-iq/) for real-time awareness of your Microsoft 365 environment,[Fabric IQ](https://learn.microsoft.com/en-us/fabric/iq/overview) for your structured data, and[Web IQ](https://www.microsoft.com/en-us/webiq) for low-latency live web grounding—all unified behind[Foundry IQ](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq?tabs=portal) , now generally available as the SLA-backed knowledge layer behind every Foundry agent.
- **How it reaches its tools:**[Toolboxes in Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/toolbox?pivots=python) **is generally available.** Instead of shipping every tool definition on every request, a toolbox dynamically selects the right tool for the job—giving agents governed, curated access while dramatically reducing the token overhead of large tool sets.
- **How it remembers and responds to the world:**[Memory](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-memory?tabs=conversational-agent) **and**[routines](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/routines) **in Foundry Agent Service are in public preview.** Memory (procedural, user, and session) lets agents carry context across interactions. Routines run any agent on a recurring schedule or timer so it acts without a user prompt; new event-based triggers, powered by the connector gateway, now let the same agent wake up the moment an upstream system signals a change—a ticket filed, a file landing in storage, a workflow completing.
- **How it reaches users:**[Publishing to Microsoft Teams and Microsoft 365 Copilot](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/publish-copilot) **is generally available next week.** The agents your developers build land in the applications where hundreds of millions of people already do their work with identity, permissions, and policy flowing through automatically. And it works even for network-isolated agents: when a project runs behind a private endpoint, you publish through a[documented flow](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/publish-copilot-virtual-network?view=foundry) rather than the one-click button. The agent stays on your private network while Microsoft’s channel adapters reach it through your own firewall and reverse proxy.


## Govern and optimize the full AI lifecycle with observability and controls


An agent you can’t see, improve, or secure is an agent you can’t put in production—so Foundry treats trust as a platform priority, not a developer responsibility. What’s new in this release closes the loop around everything that happens after you build: seeing what an agent did, making it better, and proving it’s worth running.


- **How you see what it did—**[tracing](https://learn.microsoft.com/en-us/azure/foundry/observability/quickstarts/quickstart-tracing-hosted-agent?tabs=azd) **and**[evaluation](https://learn.microsoft.com/en-us/azure/foundry/observability/quickstarts/quickstart-evaluate-hosted-agent?tabs=azd) **for hosted agents, generally available.** See exactly what an agent did, why, and where it went wrong, and evaluate behavior systematically before and after you ship.
- **How it gets better and cheaper** —[agent optimizer in Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-optimize-hosted-agent) , in public preview. It tests your prompts, skills, models, and tools together and automatically identifies better configurations—often letting you hold quality while moving to a smaller, cheaper model.
- **How you prove its value** —[ROI for agents in Microsoft Foundry](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/introducing-roi-for-agents-in-foundry/4531970) , in private preview. It connects an agent’s traces, business-value evaluations, and operating cost into a single view—surfacing KPIs like net value, total cost, and current ROI in the dashboard, so customers can see whether a production agent is creating more value than it costs to run, and drill into traces when it isn’t.


As agents scale from pilots to thousands of runs a day, Foundry gives teams the levers to keep spend predictable without leaving the platform.


That starts with choice.


- **Choose how you deploy.** Foundry offers Global, Data Zone, and Regional deployments, so you can align AI to your sovereignty, compliance, and performance requirements, running frontier models while keeping data processing in-region.
- **Choose how you pay for model inference.** A full spectrum of offers—Standard, Priority Processing, Provisioned Throughput, and Batch—lets you optimize for agility, latency, throughput, and cost on one platform.


On top of that foundation,[model router](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/model-router) matches each request to the right model,[prompt caching](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/prompt-caching) cuts redundant computation, and[PTU spillover](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/spillover-traffic-management?tabs=portal) and quota optimization preserve service continuity through usage spikes. For agents, **toolboxes in Foundry** send only the tools each request needs, and **agent optimizer** tunes prompts, skills, tools, and model choice against your own evaluators.


And spend is only half the equation. **ROI for agents in Foundry** connects business value, usage, and cost in one view—so teams can see whether a production agent is creating more value than it costs to run, and where cost is outpacing value.


For a hands-on walkthrough, watch our new[Microsoft Mechanics episode](https://www.youtube.com/watch?v=mB0IyELzjRg) on token economics for agents.


## In production: What teams are building on Foundry


The organizations building on Foundry aren’t experimenting; they’re shipping, from digital natives to the world’s largest enterprises.


- **Adobe** is building with GitHub, Foundry Agent Service, and Azure Functions, deploying agents for their applications and saving time and work getting to production.
- **Telefónica** has adopted Microsoft Foundry as the core of their corporate agentic platform, with the first wave of agents tackling network operations—a telco’s most complex, strategic domain—across Microsoft Agent Framework, hosted agents, AI Gateway, and Azure Logic Apps.
- **Tata Consultancy Services** is using agent optimizer in Foundry Agent Service to improve agent performance with a more structured approach to prompt tuning, helping teams reduce manual effort while measuring gains in task adherence and execution efficiency.


The pattern is consistent: teams that once spent weeks integrating, securing, and deploying agents are now doing it in days, on infrastructure that meets their compliance bar, reaching users through tools they already trust.


## Get started


Everything in this post is live in[Microsoft Foundry](https://ai.azure.com/) .


Follow the[documentation](https://learn.microsoft.com/en-us/azure/foundry/) and Microsoft Learn courses. Developers can get started in minutes by following the[Quickstart](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent?pivots=azd) , which walks through setting up, testing, and deploying a production-ready hosted agent end to end.


Check out[AI Agents for Beginners](https://github.com/microsoft/ai-agents-for-beginners) for a 12-lesson curriculum, then go deeper with guided labs:[Develop AI Agents in Azure](https://microsoftlearning.github.io/mslearn-ai-agents/) ,[Hosted Agents Workshop (.NET)](https://github.com/microsoft/Hosted_Agents_Workshop_dotNET) , the[Foundry Toolkit for VS Code and hosted agents workshop](https://github.com/microsoft-foundry/Foundry_Toolkit_for_VSCode_Lab/) , and the[ZavaShop Supply Chain Workshop](https://github.com/microsoft/Learn-Microsoft-Agent-Framework-with-Foundry-ZavaShop-Supply-Chain-Workshop) . To put your agents on a solid quality footing, read[Evaluating AI Agents: A Practical Guide with Microsoft Foundry](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/evaluating-ai-agents-a-practical-guide-with-microsoft-foundry/4500224) .


**Watch:**[Foundry Agent Service + Microsoft Agent Framework Explained](https://www.youtube.com/watch?v=iR7_57lJOz8) —Jeff Hollan walks through how to operationalize AI agents from deployment to real-world impact.


## Start building on Foundry


The end-to-end platform for building, running, governing, and distributing AI agents.


[Visit Foundry](https://ai.azure.com/home)
