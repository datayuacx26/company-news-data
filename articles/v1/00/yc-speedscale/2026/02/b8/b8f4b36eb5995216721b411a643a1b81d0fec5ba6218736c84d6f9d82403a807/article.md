---
schema_version: "1.0.0"
document_id: "b8f4b36eb5995216721b411a643a1b81d0fec5ba6218736c84d6f9d82403a807"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/why-your-company-will-be-running-openclaw-next-year/"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:20:47.930048+00:00"
content_hash: "sha256:2df430d60e4850bf29b047e7a07a03ac875a500ef5e02a4c3c917080572c1926"
---

# Why Your Company Will Be Running OpenClaw Next Year

You’ve probably heard of[OpenClaw](https://openclaw.ai/) . Maybe you’ve seen the demos where an AI agent opens a browser, navigates to your CRM, fills in a form, and files a support ticket. No API required. Maybe you thought “that’s cool but I’d never run that at work.”


Your employees already are.


According to[Permiso’s research](https://permiso.io/blog/inside-the-openclaw-ecosystem-ai-agents-with-privileged-credentials) , 22% of enterprise customers have employees running OpenClaw without IT approval. It’s the shadow AI problem, and it’s growing faster than any security team can track. One in five organizations that reported an AI-related breach cited shadow AI as the root cause, with those breaches[costing $670K more on average](https://www.ibm.com/reports/data-breach) .


OpenClaw is the new model for AI agents. Even for business. Even though the current incarnation is a security nightmare. Here’s why your company will be running it anyway, and what needs to change before it does.


## What OpenClaw Is


OpenClaw is an open-source AI agent framework built on “computer use”: the ability for an AI model to see your screen, move a mouse, click buttons, and type text. Instead of calling APIs, it interacts with software the same way a human does. Point it at a web app, give it a goal, and it figures out the clicks.


This matters because the vast majority of enterprise software doesn’t have a good API. Think about the tools your team actually uses day-to-day: legacy ticketing systems, internal admin portals, that one compliance dashboard nobody wants to touch. These tools were built for humans to click through, and that’s exactly what OpenClaw does.


If you’ve been in the industry long enough, this sounds familiar. We’ve been here before.


## We’ve Been Here Before: The RPA Parallel


Robotic Process Automation (RPA) already solved “automate the GUI” for enterprises. UiPath, Automation Anywhere, and Blue Prism built billion-dollar businesses on the premise that you don’t need an API when you can script the mouse. Enterprise buyers already understand this frame because they’ve already paid for it.


The difference is that RPA bots are brittle. They follow hardcoded scripts. Move a button three pixels to the left and the whole workflow breaks. OpenClaw doesn’t follow a script. It *reasons* about what it sees. When the UI changes, it adapts. When the workflow is ambiguous, it makes decisions. That’s the leap from automation to agency.


[a16z’s analysis of computer-use agents](https://a16z.com/the-rise-of-computer-use-and-agentic-coworkers/) makes the bull case clearly: this is how AI finally reaches the long tail of enterprise software (Epic, SAP, Oracle) that will never have first-class AI integrations.


## Why OpenClaw Is Different


Four properties make OpenClaw fundamentally different from other AI agent frameworks, and all four are what make it dangerous.


### Full System Control Built on Decades of Vetted Technology


OpenClaw doesn’t reinvent the wheel. It composes existing tools (browsers, terminals, file systems) using interfaces that have been battle-tested for decades. This is a feature, not a bug. There’s no proprietary sandbox to learn, no DSL to master, no vendor lock-in. It’s just a computer using a computer.


### Permissive by Default


There’s no permission model. No access control layer. No “are you sure?” dialog. OpenClaw assumes you want it to do whatever it takes to accomplish the goal. This is the opposite of enterprise software design, where the default answer to “can I do this?” is “no, file a ticket.”


### Maximum Agency


Most AI tools operate in a tight loop: human asks, AI responds, human decides. OpenClaw is designed to run autonomously. You give it a goal and walk away. It chains together dozens of actions, navigates errors, and adapts to unexpected states. That’s powerful. It’s also terrifying if you think about what “unexpected states” means on a machine with access to production credentials.


### Designed for Low-Touch, Long-Running Operations


This isn’t a chatbot. It’s a daemon. OpenClaw is architected for tasks that take minutes or hours, running in the background while you do other work. That’s the right design for real business automation. It’s also exactly the wrong design for something with no audit trail.


## Don’t Use OpenClaw in Production (Your Employees Already Are)


Let’s talk about why.


In January 2026, researchers disclosed[CVE-2026-25253](https://thehackernews.com/2026/02/openclaw-bug-enables-one-click-remote.html) , a one-click remote code execution vulnerability scoring CVSS 8.8. A malicious link in any web page the agent visits can take over the host machine. That’s not a theoretical risk. That’s a weaponized link on a webpage away from full compromise.


It gets worse. Security researchers[audited ClawHub](https://unit42.paloaltonetworks.com/agentic-ai-threats/) , the community marketplace for OpenClaw skills (think npm for agents), and found **341 malicious packages** in a coordinated campaign called “ClawHavoc” that distributed Atomic Stealer malware. Another 7.1% of marketplace skills exposed credentials in plaintext. Palo Alto’s Unit 42 team coined this the[“lethal trifecta”](https://unit42.paloaltonetworks.com/agentic-ai-threats/) : private data access, untrusted content exposure, and exfiltration vectors, all present in a single agent framework.


And roughly[1,000 OpenClaw instances](https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/) are publicly accessible on the internet right now, running without authentication.


This is npm’s left-pad moment — except the packages can see your screen and type your passwords.


The[IBM Cost of a Data Breach Report](https://newsroom.ibm.com/2025-07-30-ibm-report-13-of-organizations-reported-breaches-of-ai-models-or-applications,-97-of-which-reported-lacking-proper-ai-access-controls) found that 97% of organizations reporting AI breaches lacked proper access controls. OpenClaw doesn’t even have access controls to lack. It’s not that the door is unlocked. There’s no door.


So no, you shouldn’t run OpenClaw in production. But the uncomfortable truth is that[22% of enterprises](https://permiso.io/blog/inside-the-openclaw-ecosystem-ai-agents-with-privileged-credentials) already have employees doing exactly that. If your security team hasn’t scanned for it, they should.


## Who’s Building OpenClaw 2.0 for Business?


Every major platform vendor is racing to build what they’re calling the governed version of OpenClaw. The market is projected to grow from[$7.84B to $52.62B by 2030](https://www.rtinsights.com/if-2025-was-the-year-of-ai-agents-2026-will-be-the-year-of-multi-agent-systems/) at a 46.3% CAGR. Gartner predicts[40% of enterprise apps will have embedded AI agents by end of 2026](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025) , up from less than 5% in 2025.


Here’s who’s building what:


**Microsoft** is integrating agents across the entire stack (Copilot Studio, Agent 365, and Azure AI Foundry) with centralized agent registries and identity-driven access through Entra. They’re the first mover on **agent identity** , issuing purpose-bound credentials at runtime instead of static RBAC roles.


**Salesforce Agentforce** already has 8,000+ customers with CRM-native agents priced at $0.10 per action. If your company uses Salesforce, this is probably your first exposure.


**ServiceNow** launched AI Agent Studio and AI Control Tower, chose Claude as the default model, and is pushing governance-first agent deployment for IT workflows.


**OpenAI Frontier** , launched February 2026, provides a semantic layer for siloed enterprise data, with HP, Oracle, and Uber as early customers.


**AWS Bedrock Agents and AgentCore** go deep on IAM integration, with policy controls that intercept tool calls before execution. If your infra is on AWS, this is the path of least resistance.


**Google Vertex AI Agent Builder** wraps agents in VPC Service Controls and customer-managed encryption keys (CMEK), keeping data in your perimeter.


The pattern is clear: every major cloud and SaaS vendor is building AI agents. But here’s what they’re not telling you. These are purpose-built, vendor-locked integrations. Salesforce agents work with Salesforce. ServiceNow agents work with ServiceNow. Microsoft agents work best inside the Microsoft stack. They’re adding AI to their existing platforms, not building what OpenClaw actually is.


What made OpenClaw explode wasn’t that it was another AI tool. It’s that it was the thing we all imagined but nobody had actually shipped: a general-purpose agent that works with *any* software, on *any* screen, without needing the vendor’s permission or API. None of the enterprise offerings above do that. They’re solving for governance and control, which matters, but they’re solving it within their own walled gardens. The general-purpose, cross-platform agent that OpenClaw proved was possible? Nobody in the enterprise space is building that yet.


## How Vendors Will Integrate


The interesting question isn’t who builds the agent platform. It’s what the vendor market around it looks like. Three categories are emerging.


### Security Controls


The guardrail market is already crowded:


- **Palo Alto Networks** launched[Prisma AIRS 2.0](https://venturebeat.com/security/openclaw-agentic-ai-security-risk-ciso-guide/) to discover shadow AI agents and provide real-time defense. They also acquired Protect AI to strengthen the supply chain security angle.
- **Cisco AI Defense** includes an MCP Catalog and tool exploitation guardrails, built on their AGNTCY protocol for agent-to-agent communication.
- **Zenity** provides full inventory and attribution of AI agents across the enterprise, mapped to OWASP and MITRE ATLAS frameworks.
- **CrowdStrike** published[detailed guidance](https://www.crowdstrike.com/en-us/blog/what-security-teams-need-to-know-about-openclaw-ai-super-agent/) on detecting and containing OpenClaw in the wild.


The core technical problem these vendors are solving is **agent identity** . Traditional RBAC doesn’t work when an agent’s role changes moment-to-moment. The emerging model is purpose-bound identities issued at runtime with just-in-time access and automatic credential expiration. Microsoft Entra Agent ID is the early mover, but expect every identity provider to ship something similar by year-end.


### Agent Observability for Auditing


If an agent can take autonomous action for hours, you need to know what it did and why. If you’ve worked with distributed systems, you already know the answer: tracing. Except now you’re tracing decisions instead of requests.


- **Datadog LLM Observability** integrates agent monitoring into existing infrastructure dashboards at $8 per 10K requests.
- **Arize AX Platform** is OpenTelemetry-native with a zero-copy data lake, built for high-volume agent telemetry.
- **LangSmith** (from LangChain) is the de facto standard for LangChain-based agents.
- **AgentOps** offers time-travel debugging and session replay. Think browser DevTools for agent decisions.


The predicted ratio of autonomous agents to humans is[82:1](https://hbr.org/sponsored/2025/12/6-cybersecurity-predictions-for-the-ai-economy-in-2026) . At that scale, you can’t review agent actions manually. Observability isn’t a nice-to-have. It’s the audit trail that regulators will ask for.


And regulators are coming. The EU AI Act hits full enforcement for high-risk systems in August 2026. The Texas TRAIGA took effect January 2026. Colorado’s AI Act goes live June 2026. Enterprises aren’t just choosing to add governance. They’re being legally required to.


### Competing for Attention: The Protocol Wars


Here’s the part nobody’s talking about enough. There are three competing standards for how agents communicate with tools and with each other:


- **[MCP](https://speedscale.com/blog/4-tips-for-developing-model-context-protocol-server/)** (Model Context Protocol), originally from Anthropic, now under the Linux Foundation. It defines how agents discover and invoke tools. If you’ve used Claude Code, Cursor, or OpenCode, you’ve already used MCP.
- **A2A** (Agent-to-Agent Protocol) is Google’s proposal for multi-agent orchestration, defining how agents delegate tasks to each other.
- **AGNTCY** is Cisco’s framework for agent-to-agent communication, positioning itself as the networking layer for agents.


This is the HTTP-vs-Gopher moment for AI agents. Whoever wins becomes the standard protocol for the next decade of agent infrastructure. Right now MCP has the most momentum, but the race is far from settled.


But there’s a simpler pattern hiding in plain sight: raw skills. A skill is just a markdown file with instructions, optional templates, reference material, and maybe a script or two. No server to run, no SDK to integrate, no protocol handshake. If the agent needs a tool, the skill just tells it to download one. OpenClaw already distributes extra capabilities this way through ClawHub. Anthropic’s Claude plugins are essentially a distribution network for the same idea. If you’ve already got a CLI tool that talks to your API, you probably don’t need to build a separate MCP server that does the same job. Skills won’t replace MCP for everything, but for the majority of agent integrations today, a well-written markdown file with a few shell commands gets you 80% of the way there with zero infrastructure overhead.


The practical impact is that every tool, every SaaS platform, and every internal system will need to expose an agent-compatible interface. For complex, stateful integrations that means MCP servers or something like them. For everything else, it might just mean a good CLI and a skill file that tells the agent how to use it. If you’re building developer tools or internal platforms, this is the integration point to watch. And if[AI agents are already hitting your APIs](https://speedscale.com/blog/ai-agent-is-hitting-your-apis-are-you-ready/) , you need to be ready for what comes next.


### Sandboxing: The Isolation Layer


One more category worth watching: runtime isolation for agent execution.


- **E2B** provides cloud sandboxes used by 88% of the Fortune 100, with sub-200ms spin-up times. Open source.
- **NVIDIA NeMo Guardrails** offers an open-source toolkit for constraining agent behavior at the model level.


The thesis is simple: if you can’t prevent an agent from doing something dangerous, at least make sure it’s doing it in a sandbox where the blast radius is contained.


## The Bottom Line


OpenClaw is not a product. It’s a proof of concept for the future of enterprise computing. The four properties that make it powerful (full system control, permissive defaults, maximum agency, and long-running autonomy) are exactly what business automation needs. They’re also exactly what makes the current version unsuitable for production.


But the model is right, and it’s the reason OpenClaw made the waves it did. We’ve all imagined an AI that could just sit down at a computer and do work. Not inside one vendor’s platform. Not through a blessed API. Just a computer using a computer, the same way you do. OpenClaw is the first thing that actually delivered on that vision, even if it delivered it in a way that’s wildly unsafe for production.


The API-first integration fantasy never panned out for most enterprise software. There are too many legacy systems, too many vendor-locked portals, too many tools that were built for humans to click through. The enterprise vendors building AI agents today are solving real problems, but they’re solving them inside the same walled gardens that created the integration gap in the first place. The general-purpose, cross-platform agent that actually closes that gap? That’s still an open problem.


Within the next year, your company will be running AI agents. Some will be the vendor-specific kind from Microsoft, Salesforce, or AWS, wrapped in identity controls, audit logs, and compliance frameworks. But the pressure to run something more like OpenClaw, something general-purpose that works across the stack, isn’t going away. If anything, it’s accelerating.


The question isn’t whether your company will run AI agents with full computer access — it’s whether anyone will build the governed version of the thing people actually want.


*Building AI-powered services that need to work reliably in production?[See how Speedscale validates AI agent behavior](https://speedscale.com/company/demo/) before it reaches your users.*
