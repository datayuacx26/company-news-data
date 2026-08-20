---
schema_version: "1.0.0"
document_id: "022398cb4fc44cf7e2f541088cae7933bc65bace6bf0a6c017951c25bdb0c760"
company_key: "bitdeer-technologies-group-class-a-ordinary-shares"
company: "Bitdeer Technologies Group"
source_id: "bitdeer-technologies-group-class-a-ordinary-shares-rss-2127a671e698"
canonical_url: "https://www.bitdeer.ai/en/blog/from-safer-runtime-to-scalable-agent-deployment-with-nvidia-openshell-on-bitdeer-ai-cloud/"
published_at: "2026-04-06T05:58:08+00:00"
first_seen_at: "2026-07-24T08:23:43.073205+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:0e9e34d6db6118570c9b89cd03eeca17a0a7dee8310d858127afb105a75f9dba"
---

# ​​From Safer Runtime to Scalable Agent Deployment with NVIDIA OpenShell on Bitdeer AI Cloud

Recently, the emergence of OpenClaw drew attention to a growing demand for a new class of AI systems. Often referred to as “claws,” these systems represent long-running, autonomous, self-evolving agents that can plan and execute multi-step workflows with limited user intervention. Unlike traditional AI assistants, claws are not limited to single-turn interactions. They can access local files, interact with applications and external tools, and dynamically orchestrate sub-agents to break down complex tasks. More importantly, they are able to continuously refine strategies, decide where tasks should be executed, and optimize outcomes over time.


At the same time, this evolution introduces new challenges. As agents are granted broader access to data, tools, and infrastructure, concerns around security, privacy, and governance become significantly more critical. Questions around access control, data flow, model usage, and execution boundaries are no longer theoretical, but essential for deploying these systems safely in production.


To address the growing need for safer and more controllable agent execution, at NVIDIA GTC 2026, NVIDIA introduced[NVIDIA NemoClaw](https://www.nvidia.com/en-us/ai/nemoclaw/?ref=bitdeer.ai) for OpenClaw, an open source stack that adds privacy and security controls to long-running, autonomous agents. Built on[NVIDIA Agent Toolkit](https://developer.nvidia.com/nemo-agent-toolkit?ref=bitdeer.ai) , NemoClaw serves as an integration layer that enables OpenClaw agents to run within the newly introduced[NVIDIA OpenShell](https://docs.nvidia.com/openshell/index.html?ref=bitdeer.ai) runtime, combining open models such as[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/?ref=bitdeer.ai) with controlled execution environments to make always-on agents safer and more practical to deploy.


## **NVIDIA OpenShell: Powering Safer Agent Execution for NemoClaw**


[NVIDIA OpenShell](https://developer.nvidia.com/blog/run-autonomous-self-evolving-agents-more-safely-with-nvidia-openshell/?ref=bitdeer.ai) is an open-source runtime designed to make autonomous AI agents safer and more practical to deploy. It addresses this by acting as a control layer between the agent and the underlying infrastructure, governing how the agent executes, what it can access, and where inference is routed.


Instead of relying solely on prompts or built-in guardrails inside the agent, OpenShell enforces policies at the runtime level. It helps bridge the gap between experimentation and production by allowing developers to deploy autonomous, self-evolving agents with clearer boundaries around privacy, security, and infrastructure access.


## **How OpenShell Is Built for Safer Agents**


OpenShell uses a gateway to manage one or more isolated sandboxes where agents run. To understand how it works, it helps to look at it from two complementary angles: core components and protection layers. The components describe how the system is structured and managed, while the protection layers describe where policy enforcement is applied.


- The gateway acts as the control plane, coordinating sandbox lifecycle, authentication, providers, and policy management.
- The sandbox is the isolated runtime where the agent runs. The policy engine enforces constraints across filesystem, network, and process behavior.
- The privacy router governs inference routing, helping keep sensitive context under tighter control and directing model traffic according to defined privacy and cost policies.


These components enforce control across four protection layers: filesystem, network, process, and inference. Filesystem and process controls are locked when the sandbox is created, while network and inference controls can be updated at runtime. Together, this gives OpenShell a defense-in-depth model that supports long-running agents while maintaining clearer privacy, security, and governance boundaries.


To better understand how OpenShell enables safer autonomous agents, it helps to look at how its architecture is structured. The diagram below illustrates the key components that work together to govern execution, enforce policy, and control inference.


*OpenShell’s architecture for safer autonomous agents, illustrating the core components: the sandbox, the policy engine, and the privacy router*


## **Deploy AI Agent Workloads with Bitdeer AI Cloud**


Bitdeer AI Cloud is among the first NVIDIA Cloud Partners (NCPs) in Asia Pacific available as a model provider in the region, helping developers support the model deployment and inference behind OpenShell-based agent workloads


Bitdeer AI Cloud provides access to a wide range of high-performance NVIDIA AI infrastructure, including[NVIDIA GB200 NVL72](https://www.nvidia.com/en-us/data-center/gb200-nvl72/?ref=bitdeer.ai) ,[NVIDIA HGX H200](https://www.nvidia.com/en-us/data-center/hgx/?ref=bitdeer.ai) . Workloads can be deployed across multiple GPU configurations, with built-in support for load balancing and dynamic scaling to optimize performance, cost, and availability.


This enables developers to power the model deployment and inference required by agent workloads with the performance and reliability needed for production.


**How to Get Started**


Running models with Bitdeer AI Cloud is straightforward. Start building on[build.nvidia.com](http://build.nvidia.com/?ref=bitdeer.ai) , deploy to Bitdeer AI Cloud:


First, select a model from the NVIDIA model catalog, such as[Kimi K2.5](https://build.nvidia.com/moonshotai/kimi-k2.5/deploy.?ref=bitdeer.ai) or Nemotron 3 Super, and navigate to the Deploy section.


Then, under Partner Endpoints, choose Bitdeer AI as your model provider. This allows the underlying model deployment and inference for agent workloads to be powered by Bitdeer AI Cloud, backed by scalable, production-grade NVIDIA AI infrastructure.


Once selected, you can proceed with deployment using serverless endpoints or dedicated GPU-backed infrastructure, depending on your workload requirements.


From there, OpenShell-based agents can leverage this setup to execute tasks, call tools, and operate within controlled environments, while running on scalable, production-grade compute.


By combining NVIDIA OpenShell’s secure runtime with Bitdeer AI’s infrastructure, developers can:


- Run long-running agents on dedicated, high-performance GPU clusters
- Scale workloads without managing underlying infrastructure
- Maintain stronger control over privacy, security, and inference routing


This integration bridges the gap between agent development and real-world deployment, making it easier to move from experimentation to scalable production systems.


## **Closing Thoughts**


What stood out at NVIDIA GTC 2026 is that AI is shifting from prompt-based interactions to long-running, autonomous systems. As token consumption continues to grow, it is increasingly driven by agents themselves, reasoning over longer contexts, coordinating tools, and executing tasks over time.


This evolution brings new requirements beyond model performance. It becomes equally important how agents are executed, how data is governed, and how systems remain controlled in production environments. NVIDIA OpenShell reflects this shift by introducing a runtime layer designed for safer and more structured agent execution.


While OpenShell introduces a safer runtime layer for agent execution, Bitdeer AI Cloud serves as a model provider, delivering the scalable compute foundation needed to support real-world deployment of these systems. By providing scalable, high-performance AI compute, we aim to support developers in building and operating long-running agents within environments that prioritize privacy, security, and reliability.


**Resource:**


1. NVIDIA Developer Guide:[https://docs.nvidia.com/openshell/latest/about/overview.html](https://docs.nvidia.com/openshell/latest/about/overview.html?ref=bitdeer.ai)
2. NVIDIA blog: https://developer.nvidia.com/blog/run-autonomous-self-evolving-agents-more-safely-with-nvidia-openshell/
