---
schema_version: "1.0.0"
document_id: "8d982785a18c1cf01041db8074a543c2d885cd26126d9af6c9bf70944efff35c"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/mach-1-mcp"
published_at: "2026-07-22T05:00:00+00:00"
first_seen_at: "2026-07-22T02:49:49.423618+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:1722d1cc381a87e225caba46727f2a4496d5bddb3f2284f6d82ffe1249c2e504"
---

# How Mach 1 uses Zapier MCP to run AI operations across 25 different companies

Most AI agents can complete a task inside a single tool. Running them reliably across an entire business is a different problem.


[Chris Olson](https://linkedin.com/in/christopherleeolson) is co-founder and CEO of[Mach 1,](https://mach1ai.com/) an AI operations platform that helps mid-market companies deploy agents across go-to-market, customer success, sales, support, and finance operations. He developed the approach after applying AI-driven operations at a sports technology company, helping move the business from a $9 million annual cash burn to $5 million in free cash flow within a year.


Mach 1’s platform, Tower, acts as the operational control layer for its agents. It determines which systems and tools an agent should use, applies customer-specific permissions and business rules, maintains context across a process, and records each decision and action so operators can understand what happened and why.


Tower now supports operations for 25 companies. Each company has a different technology stack, different internal processes, and different rules for how work should be completed.


## **One operating system, 25 different technology stacks**


A support escalation might begin in Gmail, require information from a CRM, trigger an internal approval, and end with updates across several other systems.


Building every connection directly would force Mach 1 to become an integrations company. Even when an API is available, authentication, permissions, security reviews, maintenance, and customer-specific configuration can make each integration a substantial project.


Gmail is one example. Direct API access can require significantly more setup than simply obtaining an API key, particularly when an application needs to operate across multiple customer environments.


[Zapier MCP](https://zapier.com/mcp) gives Mach 1 another path. Each customer can create a Zapier MCP server connected to the tools and accounts they already use. Tower’s agents can then access those systems through a standardized interface while the customer retains control over the available actions and permissions.


This lets Mach 1 focus on the intelligence and orchestration surrounding the work rather than repeatedly rebuilding the same integration infrastructure.


Mach 1 uses the same architecture internally. Its agents accessed Gmail through Zapier MCP on 87 of the last 90 days, generating 9,552 tasks. The company also uses MCP connections for[Microsoft Outlook](https://zapier.com/apps/microsoft-outlook/integrations) and[Microsoft Excel](https://zapier.com/apps/excel/integrations) .


## **Production agents need production infrastructure**


Olson distinguishes between assistants designed for individual productivity and agents responsible for ongoing business operations.


Tools such as[Claude Code](https://zapier.com/apps/anthropic-claude/integrations) and[Codex](https://zapier.com/blog/automate-codex-zapier-mcp/) can be highly effective for a person working from a laptop. But a support queue, renewal process, or inbound lead pipeline must continue running regardless of whether an employee is online or which model provider is being used.


“What happens when someone closes the lid of the laptop? What happens when you’re sending production volume through one model provider and that provider experiences downtime? You don’t want a critical business process tied to one person’s device or one model.”


Tower runs agents continuously as managed business infrastructure. It maintains the state of long-running processes, records tool calls and decisions, and gives operators a way to review, correct, and improve how the system behaves.


Zapier MCP extends that infrastructure into the customer’s existing applications.


## **Applying the same model inside Mach 1**


Mach 1 also uses Zapier to operate its own sales systems.


The company’s CRM is built in Notion.[Webhooks by Zapier](https://zapier.com/apps/webhook/integrations) connect it to renewal emails, nurture campaigns, and other sales processes. Those workflows generated 150,630 tasks in the last 90 days.


Without Zapier, many of those changes would require engineering support.


“Previously, it would have been, ‘I need to sit down with one of our engineers.’ With Zapier, those connection points already exist. The team can focus on defining the trigger, the actions, and the business logic.”


## **An operational layer for the tools companies already use**


Mach 1 does not ask customers to replace their existing software or force every business into the same predefined workflow.


Tower provides the intelligence, orchestration, state, and oversight required to operate agents across complex business processes. Zapier MCP provides a flexible bridge into the tools where that work already happens.


Together, they allow a small Mach 1 team to support AI-driven operations across 25 different companies without rebuilding every integration from scratch—and without reducing agents to a collection of disconnected automations.


---


*Chris Olson is co-founder and CEO of Mach 1, an AI operations platform that helps mid-market companies run complex business processes with AI agents.*
