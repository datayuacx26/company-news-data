---
schema_version: "1.0.0"
document_id: "e6eff5d88c2a4ce8e5f90924fb69ccf93fa77d8874ad51c07c7ecc2f169eac93"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/serverless-vs-containers-vs-micro-vms-ai-agents"
published_at: "2026-04-20T11:38:01+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:54f091730b98329c7fabc0acf18665854e739a12057049f11be163c8517082df"
---

# Serverless vs. containers: why micro-VMs fit code-executing AI agent workloads better

You've built an agent that parses documents, generates code, and executes it in a sandbox. It works in development. Then you deploy to production, and the first user interaction slows down because the infrastructure cold-started. The second interaction loses all context because the execution environment was wiped between invocations.


These two problems, startup latency and state loss, are symptoms of a deeper mismatch. Serverless functions and containers were designed for web applications. Many production agent workloads that execute untrusted code maintain session context across repeated tool calls and need hardware-enforced isolation between tenants. The assumptions baked into both architectures conflict with how those workloads actually work.


The serverless vs. containers debate misses what code-executing agents actually need: both statefulness and strong isolation. Containers provide the persistent session context agents rely on, but share the host kernel. Serverless enforces hardware isolation through Firecracker but resets state on every invocation. Micro-VMs resolve this tension. Architecturally they sit closer to containers than to serverless, but they add per-workload kernel isolation that containers lack. This article breaks down how each approach handles the dimensions that matter most for production code-executing AI agent workloads.


## **How do serverless, containers, and micro-VMs compare for AI agents?**


The table below summarizes each approach across the dimensions covered in this article. Each row maps to a dedicated section below.


Dimension Serverless (AWS Lambda) Containers (Docker/Kubernetes) Micro-VMs (Firecracker-based)


Cold start latency Varies widely by runtime Mid-hundreds of milliseconds in cited Docker benchmarks Faster cold boot than cited Docker results; faster still on snapshot or standby resume


Security isolation Hardware-enforced (Firecracker under the hood) Shared host kernel; namespace-based Hardware-enforced; per-tenant kernel


State persistence Lost on crash/timeout; external store required Ephemeral by default; requires external persistence Standby resume can preserve memory and filesystem state; use Volumes for guaranteed long-term persistence


Max execution time Hard execution limit No platform limit No platform limit


Cost for bursty workloads Fine-grained billing; zero idle cost Minimum billing period; idle costs accrue Standby-only storage cost; zero compute when idle


Resource overhead Managed (not visible to user) Not formally quantified in cited sources here Low per-micro-VM VMM overhead


Operational complexity Fully managed; constrained environment Full control; significant ops burden Requires bare-metal or specialized platform


## **What is serverless for AI agents?**


Serverless functions like AWS Lambda run code in response to events without requiring you to manage servers. The platform allocates compute on demand, executes the function, and tears down the environment afterward. You pay only for the time your code runs.


Lambda uses Firecracker micro-VMs under the hood for isolation on bare-metal EC2 Nitro workers. So, the security model is strong. The problem is everything else about the execution model.


Serverless was built for short, stateless request-response cycles. Code-executing, stateful agent workloads don't work this way. An agent spins up, installs dependencies, loads context, executes code, and makes tool calls. It needs to remember what happened between steps. Serverless environments reset between invocations. Dependencies reinstall. Context disappears.


Serverless gets isolation right. Lambda's Firecracker foundation provides hardware-enforced boundaries per invocation. What it can't provide is the persistent session state agents need to function across multiple tool calls.


## **What are containers for AI agents?**


Containers package an application with its dependencies into an isolated unit that shares the host operating system kernel. Docker provides the packaging format. Kubernetes orchestrates containers across clusters. Containers give you more control over the execution environment than serverless, and there's no hard ceiling on execution time.


The tradeoffs come in two places.


First, security: every container on the same node runs against the same Linux kernel. A single kernel vulnerability can affect them all. This matters for agents executing untrusted or LLM-generated code.


Second, containers weren't designed for the start-stop-resume pattern stateful agents need. Standard Kubernetes primitives don't natively express "start fast, persist context, isolate execution, resume without cold start."


Containers get statefulness right. Sessions persist, filesystems survive restarts with the right configuration, and there's no execution ceiling. What they can't provide is hardware-enforced isolation between tenants running untrusted code.


## **Cold start latency across serverless, containers, and micro-VMs**


Cold start latency determines whether your agent feels responsive or broken. For agents making multiple sequential tool calls, cold start penalties compound across each step.


Serverless cold starts span a wide range. AWS Lambda cold starts vary substantially by runtime, from low-latency cases to runtimes that can exceed a second. The variation depends heavily on runtime choice.


Containers land in the middle. A peer-reviewed[USENIX ATC 2020](https://www.usenix.org/system/files/atc20-ren.pdf) study measured Docker startup in the mid-hundreds of milliseconds. The paper discusses Docker startup overheads, including namespace initialization and metadata setup.


Micro-VMs are faster than cited Docker results on cold boot and faster still on snapshot resume. USENIX research reported Firecracker cold boot times lower than Docker startup times in comparable benchmark conditions. Snapshot restore drops further. An[arXiv preprint](https://arxiv.org/pdf/2604.11669) stated that restoring MicroVMs from snapshots can reduce boot time substantially.


Perpetual sandbox platforms like Blaxel push this further. By keeping sandboxes in standby indefinitely and restoring full memory and filesystem state,[Blaxel sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) resume from standby in under 25ms. For code-executing agents making repeated tool calls per interaction, this difference compounds. The gap between fast resume and container startup latency adds cumulative delay.


If your architecture also separates tool execution and model calls, the surrounding platform matters too. In the Blaxel stack, MCP Servers Hosting manages tool integration and hosts MCP servers that expose data sources as tools, while Model Gateway handles model access, routing, telemetry, and token controls across providers. Those components don't change the sandbox benchmark itself, but they matter in the same latency-sensitive workflows this article is evaluating.


## **Security isolation for running untrusted code**


Code-executing agents that run[LLM-generated code](https://blaxel.ai/blog/ai-runtime-security) face a specific threat: the code is unpredictable by definition. A prompt injection could generate code that attempts to escape the execution environment or reach neighboring tenants. The isolation model determines whether that escape attempt succeeds.


Containers rely on Linux namespaces and cgroups, which share the host kernel. Commentary cited in this article argues that namespaces were never intended as hard security boundaries. The shared-kernel design remains the central constraint for containers on the same node.


The CVE (Common Vulnerabilities and Exposures) record confirms the risk is practical, not theoretical.[CVE-2023-2163](https://nvd.nist.gov/vuln/detail/CVE-2023-2163) was a critical eBPF (extended Berkeley Packet Filter) verifier flaw; Google scored it CVSS 10.0, while NVD/NIST assigned it a CVSS 8.8. It allowed arbitrary kernel memory read/write and could enable[container escape](https://blaxel.ai/blog/container-escape) on an affected host.[CVE-2024-21626](https://nvd.nist.gov/vuln/detail/CVE-2024-21626) allowed runc container escape through a file descriptor leak.


For untrusted workloads, the article's broader point is that containers should not be treated as the strongest isolation boundary between tenants.


Micro-VMs use hardware-assisted virtualization via KVM (Kernel-based Virtual Machine), running a separate kernel per workload. Firecracker's design documentation treats all vCPU threads as running malicious code from the moment they start. Containment relies on hardware barriers, not software namespace constructs.


Even if a container escape occurs within a micro-VM, the attacker lands in their own isolated VM. Nothing else runs on it. The escape itself isn't prevented, but it becomes far less useful.


## **How serverless, containers, and micro-VMs handle agent state**


Code-executing agents accumulate state throughout a session: installed packages, loaded datasets, conversation context, file system contents. Losing that state means rebuilding it from scratch every time.


Serverless loses everything on timeout or crash. If a Lambda function times out, AWS resets the execution environment entirely. Even between successful invocations, there's no guarantee the same environment will handle the next request.


AWS documents that Lambda functions are stateless and often use external services such as S3 or DynamoDB to store session or application state, rather than relying on local in‑memory or ephemeral storage. That adds serialization overhead and round-trip latency.


Containers are ephemeral by default. A pod restart wipes the filesystem unless you configure external persistence. Kubernetes offers primitives for this, but none solve preserving in-memory state across restarts.


Micro-VMs with snapshot-based or standby resume preserve the complete runtime state for resume. Firecracker snapshots capture full guest memory and microVM state, but not disk state, which must be managed separately. Restore is fast. The state includes guest memory and microVM state, including running processes.


Blaxel extends this with a perpetual sandbox that can remain in standby indefinitely. Sandboxes can remain in standby indefinitely with complete filesystem and memory state maintained for fast resume, but Blaxel does not guarantee durable data persistence and recommends Volumes for guaranteed long-term storage.


During active use, the sandbox automatically returns to standby after inactivity. Combined with volumes for durable data, agents can preserve session state for resume while also keeping long-term data outside the runtime snapshot.


For tool-driven workflows, this state model pairs naturally with MCP Servers Hosting. The sandbox preserves the execution environment, while MCP Servers Hosting exposes tool capabilities through MCP for the agent layer above it.


## **Execution time limits for each approach**


Agent workflows vary from sub-second[tool calls](https://blaxel.ai/blog/what-is-llm-function-calling) to multi-hour data processing jobs.


Serverless imposes hard limits.[AWS Lambda caps execution](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) at 15 minutes.[Google Cloud Functions](https://cloud.google.com/functions/docs/configuring/timeout) 2nd gen allows up to 60 minutes for HTTP triggers.[Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-scale) hit a 230-second hard ceiling for HTTP-triggered functions that cannot be overridden.


A coding agent cloning a repository, installing dependencies, running tests, and iterating on failures can exceed these ceilings.


Containers have no platform-imposed time limit. A Kubernetes pod runs until the process exits or the pod is killed.


Micro-VMs also have no inherent time limit. Blaxel sandboxes can remain in standby indefinitely, but Blaxel does not guarantee durable persistence without Volumes – like storing files on your laptop versus backing them up on a cloud storage service. Blaxel's Batch Jobs support long-running parallel workloads. For code-executing agent workflows that span minutes to hours, the absence of an execution ceiling removes the need for complex workaround architectures.


## **Cost comparison for bursty AI agent workloads**


Many code-executing AI agents follow a bursty execution pattern: brief compute during tool calls separated by idle time. The billing model determines whether you pay for idle time.


Serverless billing aligns well with burst patterns but caps out quickly. Lambda charges[$0.0000166667 per GB-second](https://aws.amazon.com/lambda/pricing/) with per-millisecond granularity. Zero cost during idle periods. Lambda's execution ceiling forces longer workflows into multiple invocations.


Container billing penalizes short tasks. AWS Fargate charges[$0.04048 per vCPU-hour](https://aws.amazon.com/fargate/pricing/) with a minimum billing period. Short tasks still hit that minimum. Container deployments commonly show underutilization in production, as teams over-provision to avoid cold start delays.


Micro-VM standby eliminates the tradeoff. The standby model charges only for snapshot storage when idle. When a request arrives, the VM resumes quickly. No keep-warm tax. No minimum billing floor.


Blaxel sandboxes automatically return to standby after inactivity, and compute billing stops during standby. For agents with irregular traffic patterns, this means paying for compute only during active execution.


If those agents also make frequent model calls, Model Gateway is relevant here because it handles LLM routing, telemetry, and token cost control alongside the sandbox runtime.


## **Resource overhead, density, and per-agent cost**


Resource overhead determines how many isolated agent environments fit on a single host.


Firecracker micro-VMs carry minimal overhead. The[Firecracker specification](https://github.com/firecracker-microvm/firecracker/blob/main/SPECIFICATION.md) guarantees less than 5 MiB of memory overhead per micro-VM (VMM process only, excluding guest RAM). The[USENIX NSDI '20 paper](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) reported less than 5 MiB of memory overhead. That's far lower than the article's cited QEMU comparison.


Containers are generally lighter-weight conceptually, but this article does not rely on a quantified like-for-like density benchmark for them in this workload context. The tradeoff is that lower overhead comes with weaker isolation because containers share the host kernel.


For code-executing agents running untrusted code, the density math changes. You need isolation per agent session. With containers, achieving VM-level isolation means adding a nested VM layer, which erodes the density advantage. With micro-VMs, isolation is built in. The low overhead makes per-agent isolation economically viable at high concurrency.


Blaxel reduces the sandbox memory footprint further using an[OverlayFS + EROFS (Enhanced Read-Only File System) implementation](https://blaxel.ai/blog/how-to-slash-sandbox-memory-usage-by-75-using-overlayfs) . The key point for this comparison is simpler: lower per-sandbox memory use means more concurrent agent sessions per host without increasing infrastructure spend.


## **Operational complexity for each approach**


Serverless is fully managed but heavily constrained. Lambda handles scaling, patching, and infrastructure management. You accept the platform's constraints: execution ceiling, limited memory, no persistent state. Working around these constraints adds architectural complexity that offsets the operational simplicity.


Containers give full control with full responsibility. Running Kubernetes in production means managing cluster upgrades, node scaling, networking, storage, security patching, and monitoring. The ecosystem is mature. The operational burden is real.


Raw micro-VMs require bare-metal access and significant expertise. Running Firecracker directly means provisioning bare-metal servers, applying specialized security configuration, and building your own orchestration layer.


Managed micro-VM platforms abstract the complexity away. Perpetual sandbox platforms like Blaxel handle bare-metal infrastructure, micro-VM orchestration, snapshot management, and security configuration. Developers interact through Python and TypeScript SDKs for hosted workloads, while the Go SDK supports interaction with platform resources rather than deploying agents or MCP servers written in Go.


They also use a REST API or Model Context Protocol (MCP) server interface.[Agents Hosting](https://blaxel.ai/agents-hosting) is relevant when you want agent logic co-located with sandboxes, MCP Servers Hosting is relevant when tool execution needs to be exposed through MCP, and Model Gateway is relevant when you want unified LLM routing and observability. No Kubernetes cluster to manage. No bare-metal provisioning.


## **Choosing between serverless, containers, and micro-VMs**


**Choose serverless when** your agents make short, discrete tool calls and don't need state between invocations. If your agent calls an LLM, processes the response, and returns a result without executing untrusted code, Lambda's billing and zero infrastructure management make sense.


**Containers may suffice if** you already run Kubernetes, your agents only execute your own vetted code, and you don't need hardware-enforced isolation between tenants. Understand that any kernel vulnerability can affect workloads on the same node.


**Choose micro-VMs when** your agents execute untrusted or LLM-generated code, need session state across interactions, or require hardware-enforced isolation. This is the combination containers can't deliver and serverless can't sustain.[Coding agents](https://blaxel.ai/blog/best-ai-agents) , PR review agents, and data analyst agents all fall into this category.


For teams building these agents, perpetual sandbox platforms like Blaxel provide micro-VM isolation with managed-platform simplicity. Agents Hosting co-locates agent logic with sandboxes. MCP Servers Hosting exposes tool capabilities through standardized protocol. Model Gateway handles LLM routing, telemetry, and token cost control. Batch Jobs handle parallel workloads that run longer in the background.


## **Try micro-VM sandboxes for your AI agent infrastructure**


The gap between what code-executing AI agents need and what serverless and containers provide is structural. Serverless resets state on every invocation. Containers share a host kernel that documented CVEs have proven exploitable. Each approach solves half the problem: serverless delivers isolation, containers deliver statefulness. Neither delivers both.


Micro-VMs close this gap by combining what each approach gets right. They preserve complete session state across standby like containers do, while running a separate kernel per workload like Firecracker-backed serverless does. No execution ceiling. No shared kernel. No idle compute charges.


The benchmarks reinforce this: micro-VMs cold boot faster than containers and resume from snapshots faster still. Per-VM overhead stays low enough to support high densities of isolated agent sessions without sacrificing security.


[Blaxel](https://blaxel.ai/sandbox) , the perpetual sandbox platform, applies this architecture to[AI agents executing code in production](https://blaxel.ai/blog/how-to-deploy-ai-agents) . Sandboxes persist in standby indefinitely with zero compute cost and sub-25ms resume from standby. Agents Hosting co-locates agent logic with sandboxes. The Model Gateway routes LLM requests with observability across your entire agent fleet. Batch Jobs support long-running parallel workloads. Compliance coverage includes SOC 2 Type II and ISO 27001.


Start with[free credits](https://app.blaxel.ai/) . No credit card required. Or[book a demo](https://blaxel.ai/contact) to see how perpetual sandboxes handle your specific workload.


Try micro-VM sandboxes for your agents


Hardware-enforced isolation with sub-25ms resume from standby. Zero compute cost while idle. Up to $200 in free credits.


[Start free](https://blaxel.ai/sandbox)


## **Frequently asked questions**


### **Can serverless functions run AI agents in production?**


Serverless works for short, stateless tool calls under Lambda's 15-minute ceiling. It is a weaker fit for code-executing workloads that need session continuity, long execution, or persistent environments. AWS resets execution environments on timeout, so agents lose in-memory state. Externalizing state to S3 or DynamoDB adds serialization overhead and round-trip latency. Google Cloud Functions and Azure Functions impose their own limits as well.


### **Why are containers considered insecure for running untrusted code?**


Containers share the host kernel across workloads on the same node. That means a kernel vulnerability can expose every container on the host at once. The examples cited in this article include CVE-2023-2163 and CVE-2024-21626. The broader point is that namespaces alone are not a hard isolation boundary for untrusted multi-tenant code.


### **What makes micro-VMs different from containers?**


Micro-VMs run a separate kernel per workload using hardware-assisted virtualization (KVM). Firecracker's design treats all vCPU threads as running malicious code from the moment they start. An exploit inside a micro-VM stays contained within that VM. Nothing else runs on it. The escape itself isn't prevented, but it becomes far less useful.


### **How does Blaxel's perpetual standby work?**


Sandboxes can remain in standby indefinitely and resume in under 25ms with memory and filesystem state preserved. Blaxel does not guarantee durable data persistence in case of infrastructure fault or misuse of the sandbox, and recommends Volumes for long-term persistence. Compute billing stops during standby. During active use, a sandbox automatically returns to standby after inactivity. When a request arrives, the sandbox resumes in 25 ms with its previous state intact. For guaranteed long-term data persistence, use Volumes.


### **What types of AI agents benefit most from micro-VM isolation?**


Agents executing untrusted or LLM-generated code benefit most: coding agents, PR review agents, and data analyst agents running dynamic scripts. These workloads need both hardware-enforced isolation and session state persistence across many tool calls. Micro-VM standby or snapshot resume preserves runtime context, so agents maintain continuity without rebuilding environments between interactions.
