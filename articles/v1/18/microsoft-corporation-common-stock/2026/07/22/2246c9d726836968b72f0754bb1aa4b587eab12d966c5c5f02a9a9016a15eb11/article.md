---
schema_version: "1.0.0"
document_id: "2246c9d726836968b72f0754bb1aa4b587eab12d966c5c5f02a9a9016a15eb11"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/proving-application-resilience-on-azure-with-chaos-studio/"
published_at: "2026-07-01T16:00:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:fc21499b0267f7909d404dc6e3664d7322bdb4241468f8b4069d14173671300d"
---

# Proving application resilience on Azure with Chaos Studio

***Takeaway:**[Azure Chaos Studio](https://azure.microsoft.com/en-us/products/chaos-studio) helps organizations validate application resilience by simulating outages, failovers, network disruptions, and infrastructure failures before they impact production.*


---


You don’t know with certainty that your application is resilient until that resilience is tested. Better to learn it isn’t by deliberately breaking it in a test environment and watching how it reacts, than by a failure in production.[Azure Chaos Studio](https://azure.microsoft.com/en-us/products/chaos-studio) is our managed service for doing exactly that, safely and on purpose.


Today,[Azure Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) is in public preview: a scenario-focused approach that lets you test the failure modes[Azure](https://azure.microsoft.com/en-us) customers actually see in production. We’ve been hard at work making Workspaces easy to use, with broad fault support and named scenarios that mirror real outages, instead of isolated faults.


[Explore Azure Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview)


## Why designing for resilience isn’t enough


Azure customers have invested in resilient design: multi-zone deployments, geo-redundant storage, automatic database failover, retry logic, load-balanced front ends. However, the real question is when an incident begins: when the failure arrives, do those mechanisms recover your application in the time you assumed they would?


Real outages don’t read the architecture diagram. A zone-redundant deployment can fail because a health probe was misconfigured years ago. A database with automatic failover can leave the application dead because a connection string is hard coded to a single region. Geo-redundant storage can briefly produce stale reads the application code never expected. These mistakes are common, and they only show up when the failure happens.


Reliability and resiliency on Azure are a[shared responsibility](https://learn.microsoft.com/en-us/azure/reliability/concept-shared-responsibility) . Microsoft is responsible for the platform and the resilience built into[Azure services](https://azure.microsoft.com/en-us/products/cloud-services) . Customers are responsible for configuring that resilience and the code that uses it. No layer makes up for a gap in another. The only way to know whether your architecture, configuration, and application logic will hold up in production is to prove they hold under failure before an outage tests them for you.


## How Chaos Studio Workspaces changes resilience testing


[Chaos Studio](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-overview) is Azure’s managed chaos engineering service for validating how applications behave under failure. By simulating controlled disruptions across infrastructure, networking, databases, and application dependencies, it helps teams uncover resilience gaps before customers experience them.[Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) focuses on scenarios that match what happens in production, so you start from a real outage pattern instead of assembling individual faults. You begin with[a named scenario like Zone Down, DNS Outage, or SQL failover](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-scenarios) , already sequenced against the resources in a Workspace.


Most outages exercise two layers at once. There’s the platform layer: did the service come back, did failover complete within your Recovery Time Objective, did traffic reroute. And there’s the application layer: did your code maintain data integrity, pick up in-flight transactions, retry the right things, degrade gracefully. A chaos test that only stops a Virtual Machine (VM) tells you about the platform layer. The scenarios in[Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) are designed to validate the entire stack.


Workspaces reduce the burden of getting started. The most common reason resilience testing stalls is that teams don’t know where to start. The[Workspace](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) is the new top-level resource: you point it at a subscription or resource group, and its managed identity discovers what’s in scope and recommends the scenarios that apply. Those scenarios show up inside the Workspace, ready to configure and run, and a refresh, updates the recommendations whenever your infrastructure changes.


**A library of real outage scenarios.**[Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) ships with curated scenarios informed by patterns observed in real Azure incidents, so the patterns you test against are the patterns customers actually experience. Think of these as resilience templates, a fast path to the failure modes most teams need to test, and when you need something different, design your own from the same fault library.


Available today:


- **Availability Zone Down:** Virtual Machine Scale Sets (VMSS) shutdown with per-zone targeting to validate cross-zone routing and recovery.
- **Availability Zone Down and Database failover:** Compute Zone Down composed with[Azure Database for PostgreSQL](https://azure.microsoft.com/en-us/products/postgresql) (Flexible Server) failover, to observe failover behavior against your configured recovery objectives and application-side connection handling.
- **DNS Outage:** a full DNS resolution outage via NSG rules that block resolver traffic, to validate how your application behaves when name resolution fails.
- **Microsoft Entra ID Outage:** identity-provider failure that exercises authentication retry, token caching, and fallback paths.
- **Cache Stampede:** Redis flush combined with database restart and an App Service process crash, to validate behavior under a cache-miss storm and the resulting database surge. The App Service process-crash variant currently supports Windows App Service plans.
- **Event-Driven Messaging Disruption:**[Azure Service Bus](https://azure.microsoft.com/en-us/products/service-bus) and[Event Hubs](https://azure.microsoft.com/en-us/products/event-hubs) disable, to validate dead-letter handling and backpressure.


Behind every[scenario](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-scenarios) are granular API-level actions built for[Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) :


- Zonal VMSS shutdown
- App Service process kill
- Force-failover for[Azure Database for PostgreSQ](https://azure.microsoft.com/en-us/products/postgresql) L (Flexible Server)
- [Azure Managed Redis](https://azure.microsoft.com/en-us/products/managed-redis) flush
- Network Security Group (NSG)-based network controls


Each scenario composes the right faults automatically. And when a curated scenario doesn’t match your workload, you can build your own. The new[Scenario Designer](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-scenarios#create-a-custom-scenario) is a drag-and-drop experience in the[Azure portal](https://azure.microsoft.com/en-us/get-started/azure-portal) for composing any of these faults into a custom scenario arranging steps, branches, and faults with the same flexibility as classic Chaos Studio experiments, now available directly inside Workspaces. Start with a curated template, or design from scratch using the full fault library.


VM agent faults such as Central Processing Unit (CPU) and memory pressure also run in[Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) . Each scenario sequences the right combination of faults automatically, so running Zone Down + Database Failover doesn’t mean thinking in terms of “shut down VMSS instances in zone 1, then force-failover the database primary.” The library will keep growing through public preview and into GA, with plans to explore additional scenarios over time, such as:


- Storage account failover
- [Microsoft Azure SQL Managed Instance](https://azure.microsoft.com/en-us/products/azure-sql/managed-instance/) failover
- Microsoft Azure Front Door and Microsoft Azure Application Gateway
- Partial zone degradation
- Microsoft Azure Kubernetes Service (AKS)-native pod chaos
- Customer-observed region down


That same foundation is also relevant for AI applications moving into production.[Copilots](https://www.microsoft.com/en-ie/microsoft-365-copilot) , agents, retrieval-augmented generation pipelines, and inference endpoints may introduce new AI-specific failure modes, but they still rely on the same Azure building blocks as other distributed applications: compute, databases, caches, search indexes, identity, networking, messaging, and storage.[Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) can validate that foundation today through scenarios like Zone Down, Database Failover, DNS Outage, Cache Stampede, and Event-Driven Messaging Disruption, while the catalog continues to evolve toward AI-specific behaviors such as retrieval drift, token throttling, and model behavior shifts under load as more insights are gathered fromworking closely with customers building AI on Azure.


[Scenario reports](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-scenario-reports) **.** When a run finishes, Chaos Studio Workspaces generates a structured drill report. It lays out what the scenario injected, which resources it affected, how the recovery timeline played out, which signals were attributable to the drill versus the normal baseline, and where the workload behaved differently than expected. The report reads like an internal post-incident review, which makes it useful both for the team that ran the drill and for the leaders who want to see resilience being validated regularly. Teams can export it and attach it to change tickets, audit evidence, or service health reviews.


## Bringing resilience testing into AI-powered operations


Alongside the product, we’re shipping two ways to drive Chaos Studio from the tools engineers already work in. The first is the Chaos Studio Skill for GitHub Copilot: it walks you through the whole loop in a conversation. Point a Workspace at a subscription, see the scenarios it recommends, run a drill, and get back a report of what *actually* happened, correlated against your[Azure Monitor](https://azure.microsoft.com/en-us/products/monitor) signals.


The second is an Model Context Protocol (MCP) server that exposes the same[Chaos Studio](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-overview) operations as typed tools, so other assistants and autonomous agents: Claude, Cursor, Codex, or your own, can provision a[Workspace](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) , run a scenario, and query the signals around it without a person in the loop. Both run against the same Chaos Studio APIs and your own[Azure](https://azure.microsoft.com/) sign-in, and[you can try them today](https://github.com/microsoft/chaos-studio) .


We’re shipping this on day one for one reason: When a customer asks an AI assistant about Chaos Studio, the experience should be shaped by us, not improvised by a large language model (LLM) reading our REST API. In our experience, one of the hardest parts of resilience testing is often deciding to run the drill in the first place, and that decision increasingly lives in the chat tools engineers already use, so this needs to live there too.


**Where this is headed:** The Skill becoming a step inside automated operations flows on[Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry) , and one of the ways an[Azure SRE agent](https://azure.microsoft.com/en-us/products/sre-agent) validates its own assumptions about how a workload fails. Try it and tell us what’s missing; we’ll close the gaps through public preview.


## Get started


[Azure Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) is in public preview today. General availability is currently targeted for late 2026, subject to change.


To start:


1. [Create a Workspace](https://learn.microsoft.com/azure/chaos-studio/quickstart-create-workspace) scoped to a subscription or resource group you want to test.
2. Let discovery populate the recommended scenarios for the resources it finds. Prefer to build your own? Open the Scenario Designer and compose a custom scenario from the fault library, no scripting required.
3. Run your first drill. If you’ve never run a chaos experiment, run **Zone Down** . A full availability-zone failure surfaces how compute placement, database failover, DNS resolution, and application-layer retry logic respond under stress. If your workload recovers within an acceptable time, you’ve gained evidence about how it responds to one of the most common causes of extended cloud downtime. If it doesn’t, you’ve found the gap on your terms instead of your customers’.


Resilience isn’t something a single feature, a single redundancy mechanism, or a single architecture decision will give you. It’s an engineering discipline, and the discipline requires verification. Azure[Chaos Studio Workspaces](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview) is how we’re making that verification the default for Azure workloads, including the AI workloads more of our customers are putting into production.


### Related resources


- [Azure Chaos Studio](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-overview)
- [Microsoft Azure Well-Architected Framework—Reliability](https://learn.microsoft.com/azure/well-architected/reliability/)
- [Recommendations for using availability zones and regions](https://learn.microsoft.com/azure/well-architected/reliability/regions-availability-zones)
- [Business continuity and disaster recovery](https://learn.microsoft.com/azure/reliability/concept-business-continuity-high-availability-disaster-recovery)
- [Reliability guides for Azure services](https://learn.microsoft.com/azure/reliability/overview-reliability-guidance)
- [Chaos Studio Workspaces documentation](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview)
- [Scenario catalog](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-scenarios)
- [Quickstart: create a Workspace and run your first drill](https://learn.microsoft.com/azure/chaos-studio/quickstart-create-workspace)
- [Announcing Azure Infrastructure Resiliency Manager Public Preview](https://techcommunity.microsoft.com/blog/reliability-and-resiliency-in-azure/announcing-azure-infrastructure-resiliency-manager-public-preview/4523710)


## Run your first resilience testing today


With Azure Chaos Studio Workspaces, you can simulate failures across your stack and gain practical insight into recovery behavior


[Try now](https://learn.microsoft.com/azure/chaos-studio/chaos-studio-workspaces-overview)
