---
schema_version: "1.0.0"
document_id: "a95b560a04e42c6286ca4949fbc42ee1f442a41776530cd8566e4af65c540453"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/dynatrace-observability-meets-nvidia-ai-q/"
published_at: "2026-07-02T23:42:51+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:06e1ee2dbf296cf94834d041dc2838bcf28ddffb9a13e75f1a714ec70f8c5280"
---

# Smarter, safer Agentic AI: Dynatrace observability meets NVIDIA AI-Q

Enterprise AI is rapidly evolving from standalone models to agentic AI systems, where multiple AI agents collaborate to gather information, reason across data sources, and generate complex outputs. These systems unlock powerful new capabilities, but they also introduce significant operational challenges. Organizations must be able to observe, govern, and optimize AI agents, models, and infrastructure in real time.


Dynatrace helps support this need by providing broad visibility across key layers of the AI stack—from agent orchestration and model inference to GPU infrastructure and enterprise applications. With Dynatrace, teams can monitor AI workflows, understand model behavior, optimize costs, and help improve reliability as agentic systems scale.


## Why agentic AI needs full-stack observability


As organizations build GPU-accelerated platforms for AI training and inference, understanding system behavior becomes increasingly complex, with bottlenecks potentially occurring anywhere – from GPU utilization, model latency, token consumption, and downstream service dependencies.


Dynatrace connects these layers through full-stack AI observability, designed to help teams monitor model performance, trace multi-agent workflows, track GPU and infrastructure utilization, detect bottlenecks across AI pipelines, and potentially accelerate troubleshooting with AI-powered root cause analysis.


This unified visibility helps organizations run AI workloads with the same reliability, efficiency, and operational confidence expected from modern enterprise systems.


This unified visibility helps organizations operate AI workloads with improved visibility and operational confidence. By integrating with NVIDIA AI–Q Blueprint and the NVIDIA Agent Toolkit, Dynatrace enriches agent reasoning with high-quality operational telemetry while at the same time helping teams govern and identify opportunities to optimize costs.


## How Dynatrace addresses Agentic AI


Dynatrace is designed to assist your team with monitoring infrastructure usage and model behavior and detecting pipeline bottlenecks and token consumption while improving reliability by accelerating troubleshooting and root cause analysis. It also provides a unified view of AI workflows from agent to model down to the infrastructure, allowing organizations to support responsible AI operations, manage cost, improve performance and support agentic workflows at scale.


Every agentic deployment is customized with different agents, tools, models, and data pipelines; therefore, observability is an important capability for understanding how these systems behave in production. The complexity arises as agents interact with multiple enterprise data sources, including:


- internal datasets
- external web and knowledge repositories
- proprietary research systems
- models served through NVIDIA NIM and Nemotron


Dynatrace can serve as operational data source for AI agents that may help improve the quality of generated insights and enable more informed decision-making. With flexible integration across customized AI-Q implementations, this architecture also lays out the groundwork for automated analysis, research, and decision making.


## How Dynatrace integrates NVIDIA AI-Q


By combining NVIDIA’s AI-Q Blueprint with[Dynatrace AI observability](https://docs.nvidia.com/aiq-blueprint/latest/index.html) , organizations gain the transparency and operational intelligence needed to govern, optimize, and scale complex AI systems.


Dynatrace integrates into AI-Q environments in two ways.


### 1. Observability and cost intelligence for Agentic AI workflows


The **NVIDIA Agent Toolkit** generates lightweight OpenTelemetry traces that Dynatrace ingests to visualize agent workflows and model interactions.


Dynatrace automatically maps the underlying infrastructure supporting AIQ deployments including **[NVIDIA NIM](https://www.dynatrace.com/hub/detail/nvidia-nim) and Nemotron microservices** and enriches telemetry with AI-specific signals such as:


- token usage
- inference latency
- model metadata
- GPU utilization


This provides comprehensive visibility across key components including:


- AI models and inference workloads
- agent orchestration pipelines
- GPU and infrastructure resources
- enterprise data interactions


With these insights, teams can quickly detect performance bottlenecks across agent pipelines, monitor GPU utilization and overall infrastructure health, and identify inefficient model usage. This visibility can help organizations identify cost optimization opportunities associated with AI workloads. Together, these capabilities position observability as important components for building reliable and scalable AI systems.


### 2. Dynatrace as a high-quality data source for AI agents


Dynatrace can also serve as an **operational intelligence source for AI agents.**


Through **Model Context Protocol (MCP)** integrations, Dynatrace exposes telemetry that agents can use in their reasoning workflows, including:


- infrastructure performance metrics
- operational incidents and problems
- deployment and reliability trends
- system behavior and resource consumption


This allows AI agents to incorporate **real-time operational insights** into their decision-making. Instead of relying solely on external data, agents gain contextual awareness of enterprise systems, which may support more informed outputs Dynatrace ingests NVIDIA Agent Toolkit OpenTelemetry traces, model telemetry, and infra metrics exposing operational context via MCP.


Together, these technologies create a powerful foundation for deploying deep research in the enterprise as reflected in the picture below.


Figure 1: Dynatrace providing AI Observability for NVIDIA AI-Q


## AI-Q use cases


The following are illustrative examples of what becomes possible when AI-Q-based research agents incorporate Dynatrace operational data and insights into their reasoning workflows. While NVIDIA AI-Q is a reference framework rather than a formal certified Dynatrace integration, these scenarios show how agentic research systems could use Dynatrace AI observability to generate richer analysis, identify patterns, and support more informed decisions.


#### Infrastructure migration analysis


AI agents combine Dynatrace operational telemetry such as performance trends, incidents, and deployment velocity with infrastructure and cloud cost data to evaluate platform migration scenarios (for example, OpenShift to AKS). The system produces data-driven recommendations with quantified tradeoffs to support strategic decisions.


#### Large-scale incident analysis


By analyzing thousands of historical problems, AI agents can identify recurring patterns, understand infrastructure behavior, and correlate technical issues with business KPIs. This enables deep operational insights and long-form analysis that would be difficult and time-consuming for humans to produce.


#### AI cost governance and optimization


Enterprises can use observability data from Dynatrace to analyze token consumption, model usage, and inefficient data interactions across AI workloads. Agents can identify patterns and suggest potential optimizations such as more efficient models or improved workflows.


#### Software delivery and reliability insights


DevOps and SRE teams can use agentic analysis to correlate deployments with incidents, assess build quality trends, forecast reliability risks, and identify engineering priorities—using Dynatrace as the trusted operational data source.


## Get started today


- Try the[Dynatrace Playground](https://playground.apps.dynatrace.com/ui/apps/dynatrace.genai.observability/overv) — Explore AI observability with sample data
- Explore NVIDIA integrations in the[Dynatrace Hub](https://www.dynatrace.com/hub/?query=nvidia)
- Stream NAT telemetry to Dynatrace —[NVIDIA Agent Toolkit OpenTeletemetry guide](https://github.com/NVIDIA/NeMo-Agent-Toolkit/blob/main/docs/source/run-workflows/observe/observe-workflow-with-dynatrace.md)
- Use Dynatrace as an operational data source —[Remote MCP Docs](https://docs.dynatrace.com/docs/shortlink/dynatrace-mcp-server)
