---
schema_version: "1.0.0"
document_id: "cde40d5f2628d70c58156ad342212ca7716337feea579ab1a51dc22ebd5a87f7"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-a76a08583e5b"
canonical_url: "https://blogs.microsoft.com/blog/2026/06/23/rethinking-cloud-operations-with-agentic-observability/"
published_at: "2026-06-23T15:45:05+00:00"
first_seen_at: "2026-07-20T04:34:25.408964+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:868acc6372311d8dd1aabbd7efdedbbe7b5b3185b81327d41ac89babef5f6c0a"
---

# Rethinking cloud operations with agentic observability

Cloud operations are entering a new era as AI-driven and autonomous agents become a larger part of modern software systems. As software becomes increasingly agentic, the challenge is no longer just managing greater scale and complexity. Operators must also contend with systems that evolve faster, act more autonomously and interact across an expanding network of dependencies.


As applications, models, APIs and infrastructure become increasingly interconnected, their behavior is harder to understand end to end. Systems no longer fail in isolation. They fail through interactions across dependencies, services and environments that are constantly changing in real time.


To help organizations operate effectively in these increasingly dynamic environments, today we’re announcing the[general availability](https://aka.ms/ObservabilityAgentGA) of the Azure Copilot Observability Agent. Built on Microsoft Azure Monitor, it correlates signals across agents, applications, infrastructure and services to provide the context needed to operate confidently in this new environment.


### **Observability becomes foundational in an agentic world**


> In a recent[survey](https://aka.ms/AgenticOps-MaterialWhitepaper) of 250 IT decision-makers, Microsoft and Material found that **84%** of organizations report increased cloud complexity, with **69%** saying it is outpacing their current operating model. The impact is most acute across security, cost management and performance, and it extends across the entire operations lifecycle.


As the pace and scale of change accelerate, no individual or team can realistically maintain the full context required to diagnose and resolve issues quickly enough. This is driving a shift toward agentic operations, where intelligence augments how systems are understood and managed.


Observability is foundational to this shift. It provides the real-time understanding of system behavior that agents depend on to reason, adapt and act. Without a connected view across signals, even the most advanced agents lack the context required to operate reliably.


### **From signals to resolution with the Observability Agent**


We designed the Observability Agent to help operators move more quickly from detection to understanding. It connects logs, metrics, traces, topology and operational context across environments, reducing the time it takes to identify the root cause of an issue.


As telemetry spreads across systems, operators are often forced to piece together context across multiple tools. The Observability Agent addresses this fragmentation by reasoning across signals in real time and unifying that context into a single operational view. These agentic capabilities are integrated directly into existing workflows, helping teams move from investigation to resolution faster with clear, actionable insight.


We’re already seeing customers use the Observability Agent to reduce manual effort, accelerate incident resolution and improve operational clarity:


> “The biggest value is speed! The \[Azure Copilot\] Observability Agent helps us resolve incidents faster and reduce operational overhead by turning logs, metrics and traces into plain English insights. These agents run deep investigations and provide remediation recommendations almost immediately, compared to hours or even days previously.
>
>
> Since adopting these capabilities, we’ve reclaimed an estimated 250 engineering hours monthly that are now redirected toward supporting new applications and features. We can use natural language to detect, diagnose and remediate issues faster than ever before.”
>
>
> — Narmada Krishnaswamy, Head of KPMG Audit Application Support and Operations
>
>
> “Azure Copilot Observability Agent helped us move from manual incident hunting to faster, AI-guided investigations. For PolicyVault, it pulls together the telemetry from our service, correlates it with Azure resource health and gives us actionable next steps based on the investigation. That means we’re not just seeing what broke; we’re getting a much clearer idea of why it happened and what to do about it, which saves us a lot of time during incidents.”
> — Vladimir Gusarov, Founder & CEO, PolicyVault
>
>
> “Azure Copilot’s Observability Agent helps us move faster from signal to insight. By bringing together our telemetry and guiding us toward likely root causes, it reduces the time and effort needed to investigate incidents and keeps our teams focused on what matters most.”
> — Theus Hossmann, Chief Technology Officer at Ontinue


Beyond improving incident response, this shift reflects a new approach to cloud operations, where systems can continuously reason across signals and act on that understanding.


Check out our[Tech Community blog post](https://aka.ms/observabilityagentga) to learn more about the Azure Copilot Observability Agent.


### **From observability to agentic operations across the cloud lifecycle**


Observability is part of a broader shift to agentic operations. As systems become more autonomous, operations expand from understanding what is happening in production to continuously improving how those systems behave over time.


In an agentic model, this forms a lifecycle. Systems generate signals, agents interpret those signals, take action and learn from outcomes. Over time, this creates a feedback loop where each operational cycle improves the next, increasing system resilience and efficiency.


This shift requires more than better visibility. It requires a coordinated approach across the lifecycle, from observability and diagnosis to optimization and remediation where insight and action are tightly connected.


As agents take on a greater role in that lifecycle, governance becomes central to how systems are trusted and controlled. Policy, auditability and guardrails ensure that actions taken by agents align with organizational intent and operate within defined boundaries. Human oversight remains essential, not as a bottleneck, but as a mechanism for building confidence and ensuring reliability as automation scales.


This is where Azure is uniquely positioned. By bringing together observability, automation and governance within a connected platform, Azure enables organizations to move from isolated tools to an integrated operational model that spans the full lifecycle.


Azure Copilot Observability Agent plays a key role in this model by grounding agentic systems in real-time operational context. As organizations build and deploy more agents, this foundation becomes critical for ensuring those systems operate effectively and responsibly.


Cloud operations are shifting from reactive management to a continuous, agent-driven lifecycle of learning, adaptation and control. This vision of agentic cloud operations is already taking shape across Azure. Read our companion[Azure Blog](https://azure.microsoft.com/en-us/blog/from-insight-to-action-the-next-phase-of-agentic-cloud-operations) post for more details.


*Brendan Burns is a co-founder of the Kubernetes open source project and corporate vice president for Azure cloud-native open source and the Azure management platform including Azure Arc. He is also the author and co-author of several books on Kubernetes and distributed systems.*


Tags:[Azure](https://blogs.microsoft.com/blog/tag/azure/) ,[Azure Copilot Observability Agent](https://blogs.microsoft.com/blog/tag/azure-copilot-observability-agent/) ,[Microsoft Azure Monitor](https://blogs.microsoft.com/blog/tag/microsoft-azure-monitor/)
