---
schema_version: "1.0.0"
document_id: "918dd97c914582bdcd4138f82aab0afea68961210dfe665512a9889878c1cade"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-b1ffd1321f3f"
canonical_url: "https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/02/announcing-the-new-work-iq-apis/"
published_at: "2026-06-02T17:00:15+00:00"
first_seen_at: "2026-07-20T04:34:28.173713+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:724d4b7edf0d6072295af6d5a0985a8da759b41eb04f0875eacd150086042f2e"
---

# Announcing the new Work IQ APIs

Software is moving from applications built for people to agents that can reason, retrieve context, and even act on a user’s behalf. That shift calls for a different kind of API surface. Today we are announcing that the **Work IQ APIs will be generally available on June 16, 2026** . This new API set will be the best way for agents to interact with Microsoft 365 data and apps.


Work IQ is the intelligence layer behind how work gets done. It builds a semantic understanding of your business by continuously processing content from email, calendar, meetings, chats, files, people, collaboration patterns, and your line of business systems. Work IQ provides intelligence you can’t get from the data alone: a real-time model of how your organization operates. With the Work IQ APIs, you can bring that understanding directly into your own agents, so they can work with business context, not just raw data, and take actions across your organization.


## **The Work IQ APIs: 5 distinct advantages**


Traditional interfaces were designed for human interaction, but agents work differently. They need richer context, simpler tool surfaces, lower latency, and enterprise controls built in from the start. This is what the Work IQ APIs are built for, with five core advantages: intelligence, speed, efficiency, scale, and security.


### 1. Intelligence


The Work IQ APIs give agents higher quality results because they do more than retrieve or search content. Work IQ builds on a semantic index with ultra-low latency, personal memory, personal and organizational skills, structured schema on top of files, and business-specific knowledge tuning. The result is that agents can work from the most relevant and continuously updated context with a comprehensive understanding of people, organizational structures, roles, and collaboration patterns.


### 2. Speed


The Work IQ APIs operate at the speed of agents. They use an agent-optimized retrieval system that reduces round trips to the service and lowers access latency to rich context. For tool calling, Work IQ APIs collapses operations into just 10 generic tools with progressive disclosure through model context protocol (MCP), so developers do not need to teach agents hundreds of data-specific tools. That simpler surface helps agents move from reasoning to action much faster, and gives agents higher throughput, accelerating response tokens per second.


### 3. Efficiency


The Work IQ APIs reduce the total number of tokens needed to retrieve context and use tools by moving more of the AI processing into the Work IQ runtime. Instead of returning raw data that an orchestration layer has to read, stitch together, and interpret, specialized large language models (LLMs) and agents within Work IQ package relevant context and data in a structure that is easier for agents to consume. It also trims file record strings, message IDs, and app IDs during tool calling to save tokens.


### 4. Scale


Work IQ has been optimized to support the scale of agent usage. When people use software or interact with systems, it can be intermittent and relatively shallow. Agents execute continuous, high-frequency, multi-step operations that are broader, deeper, and more systematic than how people use software. Work IQ is designed for that usage pattern and the much higher throughput that comes with it. This will allow Microsoft 365 to scale as hundreds of millions of agents come online over the next few years.


### 5. Security


With Work IQ, agent operations are secure by design. Data, context, and insights stay within the Microsoft 365 tenant trust boundary, and actions that an agent takes are auditable and discoverable. This gives developers and IT the controls they need to build enterprise agents without adding a separate governance layer.


## Work IQ API architecture


The Work IQ APIs consist of four domains that map how agents work:


- **Chat** . Programmatic access to the full power and intelligence of[Microsoft 365 Copilot](https://www.microsoft.com/en-us/microsoft-365-copilot) . The Chat API returns the response (including citations) that Copilot would return to a user and provides programmatic access to agents in Copilot.
- **Context** . Programmatic access to agent-ready context and source data. The Context API aggregates the content that Copilot would use to respond to a query but instead of synthesizing it into a response, it returns the context in a format designed for agent consumption.
- **Tools** . Agentic access to Microsoft 365 entities and actions through a simple set of verbs with complementary resource paths to define work scope. Examples include sending emails, scheduling meetings, uploading documents, and more. This gives agents a stable, adaptable way to work across new data and changing scenarios without expanding the API surface each time.
- **Workspaces** . As agents work, they need somewhere to safely stash intermediate state and data during its execution. Work IQ digital workspaces are within the Microsoft 365 tenant boundary, and store data, files, memory, progress, and intermediate outputs as agents reason through work. As we have built out[Copilot Cowork](https://www.microsoft.com/en-us/microsoft-365/blog/2026/05/05/copilot-cowork-from-conversation-to-action-across-skills-integrations-and-devices/) ,[Microsoft Scout](https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/02/introducing-microsoft-scout-your-always-on-personal-agent/) , and other long running agents, this has been a critical enabler for longer running agents.


You can already start to build with these domains now through the[public preview on GitHub](https://github.com/microsoft/work-iq) , ahead of general availability on June 16.


## Work IQ APIs **pricing**


The Work IQ APIs use consumption-based pricing with a fixed component for Tools and a variable component for Chat and Context. Pricing is denominated in Copilot Credits. Learn more about[Work IQ pricing](https://aka.ms/WorkIQ/licensing) .


We are also launching a new cost management dashboard in the Microsoft 365 admin center. IT admins will be able to review AI credit usage, configure Copilot Credit billing between prepaid or pay-as-you-go, set spending limits for tenants, groups, and users across agents and services, and monitor credit requests from users. Work IQ APIs will be the first product managed through this experience, and over time we will bring in additional products that use Copilot Credits, including[Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio/) .


## **Get started with Work IQ APIs**


Work IQ is a new intelligence layer for Microsoft 365, designed to understand how work gets done across your organizations. The Work IQ APIs give developers and IT administrators a practical way to build enterprise agents with the context, tools, scale, and controls that those agents need to operate inside Microsoft 365. With general availability on June 16, organizations can start building on the same intelligence layer that powers Copilot.
