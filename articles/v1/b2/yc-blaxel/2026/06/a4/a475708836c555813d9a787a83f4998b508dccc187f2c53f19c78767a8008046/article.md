---
schema_version: "1.0.0"
document_id: "a475708836c555813d9a787a83f4998b508dccc187f2c53f19c78767a8008046"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/rl-agents-sandbox-security"
published_at: "2026-06-04T12:22:36+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:26274fa03a8ee79cf29e3ec8d8233e20edd850dfd169b1da4867fb19c997ee5f"
---

# Running RL agents in secure sandboxes: isolation and persistence

An engineering leader greenlights an RL-trained coding agent for production. The first major incident traces back to the execution layer. An over-eager policy runs a destructive shell command during a rollout. It wipes filesystem state and corrupts the reward signal for a training batch.


Or a checkpoint resumes into a dirty environment, producing trajectories that poison the replay buffer. Or a large batch of parallel episodes drives cloud spend into the red overnight. Idle sandboxes keep billing after work stops.


RL agents put unusual pressure on the execution layer. They explore actively, generating unpredictable actions across many parallel episodes. They produce trajectories that must survive restarts without rebuild cost. The sandbox boundary determines whether exploratory behavior stays contained. It also determines whether training stays economically viable. A weak boundary creates both security risk and corrupted training signals. These corrupted signals silently degrade the learned policy.


This guide covers isolation requirements specific to exploratory RL agents and state persistence patterns these workloads depend on. It also examines the trade-offs engineering leaders face when choosing architecture for RL training and serving.


## **What RL agents demand from execution infrastructure**


This article focuses on the execution layer where RL agents take actions. It doesn't cover the training loop where gradient updates run on GPUs. The two layers have different infrastructure profiles and different scaling constraints. Conflating them leads to architecture decisions that optimize the wrong constraint.


An RL agent generates trajectories by interacting with an environment over many steps. Each step can be a shell command, an API call, code execution, or a tool invocation that touches real systems. The[Relax paper](https://arxiv.org/html/2604.11554v2) specifies that agentic RL imposes new requirements on training engines. These include variable-length trajectories, extended context windows, and highly variable response latencies.


Several properties separate RL execution from standard agent execution. First, exploration generates unpredictable actions, including destructive ones. The policy network has no concept of host security or filesystem integrity. Second, training requires large batches of parallel episodes to converge.


Algorithms like PPO and GRPO need trajectories in large enough batches to estimate policy gradients reliably. Third, learning depends on[reproducible state across restarts](https://openreview.net/pdf/6cad89df1fcfe3ce8a5247a390e984b74730369b.pdf) . Rewards computed in a dirty environment don't reflect actual policy behavior.


Traditional infrastructure breaks here. Web app patterns assume long-lived processes serving deterministic requests. Traditional serverless assumes short stateless invocations. RL workloads sit between the two: stateful, parallel, bursty, and running code that no human has pre-validated.


The[VerlTool paper](https://arxiv.org/html/2509.01055v1) reports lower throughput for synchronous tool processing than for asynchronous processing. Synchronous request-response patterns can become a constraint in RL training loops that depend on frequent tool use.


RL is moving into production for coding agents, browser automation, and tool-use systems. The infrastructure choice affects training cost and incident exposure for years. Getting it wrong means either migrating later or accepting a permanent tax on every training run.


## **The isolation problem when RL agents explore**


Isolation matters for any sandbox, but RL raises the stakes because exploration actively rewards finding edge cases. A policy searching for high-reward behavior will probe boundaries that a deployed agent never would. A[SoK paper](https://arxiv.org/html/2602.08690v1) on deep RL for cybersecurity discusses reward hacking. In RL training with real OS access, reward hacking becomes a consequence of the exploration mechanism, not a hypothetical.


### **Why exploratory policies need stronger isolation than deployed ones**


A deployed agent runs the actions it was trained to run. An RL agent during training samples actions across the full distribution. That includes malformed system calls, unbounded resource requests, and combinations the training team never anticipated. The policy network has no concept of host security. The boundary has to come from the runtime.


The problem is one of defense in depth. Fork bombs and resource exhaustion are well-documented container threats. Containers' shared-kernel model increases risk to the host in both cases. An RL agent discovering that unbounded` fork()` calls produce unique environment states can trigger this as part of normal exploration.


Containers share the host kernel, and the CVE record shows this boundary fails repeatedly.[CVE-2024-21626](https://nvd.nist.gov/vuln/detail/CVE-2024-21626) allowed complete[container escape](https://blaxel.ai/blog/container-escape) through a file descriptor leak in runc.[CVE-2025-31133](https://nvd.nist.gov/vuln/detail/CVE-2025-31133) bypassed bind-mount verification in runc, affecting Docker, Kubernetes, and managed services using runc.


[AWS's security bulletin](https://aws.amazon.com/security/security-bulletins/rss/aws-2025-024/) states directly: "AWS does not consider containers a security boundary." Hardware-virtualized boundaries, microVMs, generally hold up better under this pressure than shared-kernel approaches. Even if the agent generates a guest-kernel exploit, reaching the host requires a separate hypervisor or VMM escape.


The[Firecracker NSDI paper](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) describes a KVM-based architecture where the guest kernel runs in a separate hardware-enforced protection domain. Host kernel system calls aren't directly reachable from the guest. Containers are operationally simpler and faster to start, and they work fine for trusted, first-party code. RL training workloads don't qualify as trusted code by definition.


### **Tenant isolation across many concurrent rollouts**


RL training fans out across many parallel episodes, often on shared infrastructure. One rollout's exploit attempt should not leak into another rollout's environment, into the replay buffer, or into the gradient signal.


A[University of Washington paper](http://faculty.washington.edu/wlloyd/courses/tcss591/papers/ReinforcementLearningforResourceManagementinMultiTenantPlatforms.pdf) frames this as a correctness issue. Multi-tenancy makes the environment non-stationary from each agent's perspective. Other agents' actions affect the shared environment, breaking assumptions that single-agent RL algorithms depend on.


Sandboxes built on Firecracker, Cloud Hypervisor, or similar microVM technology give each rollout its own kernel and memory boundary. The[Firecracker design documentation](https://github.com/firecracker-microvm/firecracker/blob/main/docs/design.md) treats guest code as adversarial: "all vCPU threads are considered to be running malicious code."


Each microVM uses[fewer than 5 MiB](https://aws.amazon.com/blogs/opensource/firecracker-open-source-secure-fast-microvm-serverless/) of overhead, which helps preserve density with strong per-rollout isolation. gVisor offers a middle ground by intercepting system calls at the user-space level. It provides stronger isolation than containers without requiring bare-metal KVM hosts. Tencent deployed gVisor for[millions of RL sandboxes](https://gvisor.dev/blog/2026/04/23/scaling-agentic-rl-sandboxes-to-the-millions-with-gvisor-at-tencent/) .


Their motivation was running inside regular VMs with lower startup and resource costs than microVM approaches that require bare-metal hosts. For workloads running untrusted, model-generated code, gVisor reduces the attack surface significantly. It still shares an underlying kernel, unlike hardware-virtualized microVMs.


## **State persistence patterns RL agents depend on**


RL training depends on state. Checkpoints, replay buffers, environment snapshots, and trajectory history all need to survive across restarts. Sandbox platforms that delete state after a fixed window force teams to rebuild expensive setup on every resume. That cost compounds across many rollouts.


### **Checkpointing and resume semantics**


Episode-level state matters: filesystem changes, loaded datasets, and process state inside the sandbox. The Crab paper identifies two main layers required for correct resume: filesystem state and process state. Application-level checkpointing alone misses the OS-side effects, producing inconsistent environments on resume.


When a rollout pauses mid-trajectory or resumes after a node failure, the resume cost adds up. It's boot time plus rebuild time. The[MoEtion paper](https://arxiv.org/html/2412.15411v3) documents that existing checkpointing techniques roll all workers back to a common checkpoint, amplifying recomputation overhead. That makes resume semantics an infrastructure cost issue, not a reliability detail.


Most sandbox providers cap standby duration. E2B deletes paused sandboxes after 30 days, requiring full environment recreation. Daytona archives stopped environments after 30 days. Restoring an archived environment takes more time than starting a stopped one. Modal caps sandbox standby at 7 days, with filesystem and memory snapshots deleted after that window. The feature remains in alpha as of March 2026.


CodeSandbox hibernates after inactivity, with a 2 to 7 day window at CodeSandbox's discretion based on infrastructure strain. Cloudflare Sandboxes are ephemeral, with all files deleted when the sandbox pauses.


Perpetual sandbox platforms like[Blaxel](https://blaxel.ai/sandbox) offer an architectural alternative for RL workloads. Sandboxes stay in standby indefinitely with sub-25ms resume and zero compute cost while idle. That matters for RL because repeated setup work turns every resume into extra rollout latency and extra compute spend. For guaranteed long-term persistence that needs to survive sandbox deletion,[volumes](https://docs.blaxel.ai/Sandboxes/Volumes) provide the durable layer.


### **Trajectory branching for exploration**


Search-based methods like Monte Carlo tree search (MCTS) and tree-search policies branch from a single state into many continuations. Running this on infrastructure that doesn't support fast snapshotting forces teams to replay from scratch for each branch. The Crab paper tests tree-based RL rollouts with a small number of branches per trial. Its checkpoint/restore system reduces rollout tokens by[40 to 64%](https://arxiv.org/html/2604.28138v1) compared with re-executing rollouts without snapshotting.


At the[$200 to $2,000](https://epoch.ai/gradient-updates/state-of-rl-envs) per-task cost reported by Epoch AI, that waste compounds rapidly across many parallel environments. Sandboxes that snapshot filesystem and memory together let a parent state spawn child rollouts. Each child diverges from the same starting point. The question for infrastructure evaluation is concrete. Does the platform expose snapshot and restore primitives? What does the per-branch cost look like when tree-search becomes viable?


### **Replay buffer and trajectory storage boundaries**


The replay buffer itself usually lives outside the sandbox. The canonical architecture, established in the[IMPALA paper](https://proceedings.mlr.press/v80/espeholt18a/espeholt18a.pdf) , separates actor nodes from the learner node. Actors generate trajectories inside sandboxes. The learner trains from the replay buffer. Actors produce fixed-length unrolls and ship them to the learner via a queue.


The execution-layer point is straightforward: the sandbox's job is to produce trajectories and ship them out cleanly. Sandbox state is for the rollout, not for the training corpus. The[RLAX paper](https://arxiv.org/html/2512.06392v1) describes storing controller snapshots for deterministic rollout regeneration. These include model checkpoint step, prompt mixture progress, RNG stream state, and metadata.


Transient artifacts like KV caches stay inside the sandbox. Volumes inside a sandbox handle persistent per-rollout data, while the broader replay infrastructure sits separately on the platform's object store.


## **Scaling parallel rollouts for RL agents**


RL training throughput depends on running many environments in parallel. The economics depend on what happens when those environments aren't actively executing. Two factors dominate: whether idle time bills at compute rates, and whether restarting costs seconds or milliseconds.


### **Fan-out across many environments**


Modern RL methods run many parallel environments to generate enough trajectories per training step.[Together.ai's DeepSWE](https://www.together.ai/blog/deepswe) scales beyond 1,000 CPU cores, collecting trajectories at large volume with Kubernetes autoscaling.


The[INTELLECT-3 report](https://arxiv.org/pdf/2512.16144) documents running more than 4,000 concurrent sandboxes on a 512 H200 cluster, sustained over two months. Those counts show that rollout infrastructure has to handle sustained concurrency, not short benchmark spikes.


The infrastructure question is whether the platform can handle that count without per-environment provisioning friction. Warm pool patterns need to work at the required concurrency. Per-rollout isolation matters for security, while batch-style parallel execution patterns matter for cost.


Evaluate whether the platform exposes both primitives. For fan-out workloads,[Batch Jobs](https://docs.blaxel.ai/Jobs/Overview) in Blaxel provide the scheduling and parallelism layer. They scale from zero to thousands of parallel sandboxes for asynchronous trajectory collection.


### **Idle cost across long training runs**


Training runs span hours to days, and not every environment is busy every second. RL training is inherently bursty: high-parallelism rollout collection phases interspersed with synchronous policy update phases.


An[academic analysis](https://arxiv.org/html/2506.01283v2) of serverless costs notes that serverless platforms can impose billing granularity or minimum billing cutoffs. Azure Functions, for example, enforces a 100ms minimum billable duration on its legacy Consumption plan. That disadvantages high-frequency, short-duration invocations.


Platforms with minimum billing periods of one minute or fifteen minutes charge for compute the agent isn't using. Per-second billing combined with fast shutdown when network connections close compresses the bill to actual work. The calculation that matters for engineering leaders is cost per million trajectories, not cost per sandbox-hour. A sandbox platform that returns sandboxes to standby after 15 seconds of network inactivity charges zero compute during standby. That matches RL's bursty rollout pattern.


### **Cold start compounding across episodes**


A coding-RL agent making several tool calls per step compounds boot time across each call. Boot times measured in seconds become accumulated wall-clock waste across repeated calls. The[Firecracker project](https://firecracker-microvm.github.io/) can initiate user space or application code in as little as 125 milliseconds. Lower startup and resume latency reduce accumulated delay across repeated tool calls. They also keep interactive workflows feeling immediate.


Jakob Nielsen's research identifies about[100 milliseconds](https://www.nngroup.com/articles/response-times-3-important-limits/) as the instantaneous-response threshold. Below that limit, users perceive the system as reacting immediately. Above one second, users notice the delay and flow degrades.


## **Trade-offs between security, speed, and operational cost**


No architecture wins on every axis. The engineering leader's job is matching the trade-off profile to the workload.


The following trade-offs apply specifically to RL workloads that execute untrusted, model-generated code. Workloads running only trusted first-party code face a different calculus:


- **MicroVM isolation versus container speed:** Hardware-virtualized boundaries hold up under exploratory policies but carry slightly higher per-instance overhead than containers. Containers start faster and use fewer resources for trusted code. For RL training, the trade-off often favors stronger isolation once sandbox boot times are already low.
- **Perpetual standby versus rebuild on resume:** Infinite standby preserves expensive environment setup with zero idle compute cost. Providers without persistent standby require reinitialization on restart, adding startup overhead. RL workloads with cloned repositories or loaded datasets feel this most. Audit your average environment setup cost per rollout to determine the break-even point.
- **Per-second billing versus minimum-period billing:** Per-second billing aligned with actual network activity matches the bursty pattern of RL rollouts. One-minute or fifteen-minute floors charge for idle time the agent isn't using. Multiply your average idle gap between rollout batches by the billing floor to estimate the waste.
- **Managed networking versus self-hosted plumbing:** Custom domains, dedicated outbound IPs (in private preview on some platforms), and proxy-based secrets injection ship with some platforms. Building these in-house adds engineering weeks to months. Check whether your workload requires static outbound IPs for API whitelisting before committing to a self-hosted approach.
- **Compliance posture upstream versus DIY:** SOC 2 Type II, ISO 27001, and HIPAA BAA in place at the vendor shorten enterprise sales cycles. Building SOC 2 Type II in-house takes[6 to 18 months](https://www.konfirmity.com/blog/iso-27001-audit-timeline) with a non-compressible three-month observation window. ISO 27001 certification adds[$10,000 to $50,000](https://rhymetec.com/iso-27001-certification-cost-breakdown-2025/) for most startups, with ongoing maintenance costs after certification. Using a pre-certified vendor compresses this to weeks of due diligence review.


The right combination depends on whether the workload is exploratory training, deployed inference, or something in between. Exploratory training maximizes the value of microVM isolation and perpetual standby. Deployed inference tolerates lighter isolation but demands the lowest possible latency.


## **How to evaluate sandbox infrastructure for RL agents**


For engineering leaders, the key evaluation question is specific. Which sandbox removes the most risk when training and serving RL agents in production? A small set of questions covers most of the decision:


- **Isolation model under exploratory action distributions:** Hardware-virtualized boundaries hold up better than shared-kernel approaches when the policy is actively probing edge cases. Ask the vendor whether their isolation is container-based, user-space kernel (gVisor), or hardware-virtualized (microVM). Then cross-reference against the[runc CVE record](https://www.cncf.io/blog/2025/11/28/runc-container-breakout-vulnerabilities-a-technical-overview/) to assess your exposure.
- **Standby duration and resume cost:** Stateful rollouts need filesystem and memory preserved across idle periods, not deleted after fixed retention windows. Resume time under 25 milliseconds keeps episode boundaries cheap. Calculate your average rollout setup cost and multiply by your daily rollout count to size the rebuild tax from fixed-window providers.
- **Concurrency ceiling and fan-out semantics:** The platform should support the level of concurrency your rollout system actually needs, with primitives for parallel batch execution. Request a load test at your target concurrency before signing a contract.
- **Compliance certifications already in place:** SOC 2 Type II, ISO 27001, and HIPAA BAA in hand at the vendor shorten enterprise procurement and remove blockers for regulated customers. Deals[stall without attestation](https://www.konfirmity.com/blog/soc-2-what-changed-in-2026) .
- **Observability designed for agent execution:** OpenTelemetry traces that span rollouts and tool calls in the same view, with per-rollout metrics for cost attribution and post-hoc analysis. The[GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) provide a standardized schema for token tracking and LLM call instrumentation. If the workload also depends on centralized LLM routing and token controls, a model gateway becomes relevant alongside the sandbox layer.


The answers to these questions determine whether the platform supports the workload or stalls the project.


## **Build production-ready infrastructure for RL agents**


The sandbox decision is a multi-quarter commitment that shapes incident exposure, training throughput, and unit economics. Picking a platform optimized for ephemeral developer workflows creates problems when the actual workload is stateful exploratory RL.


The mismatch surfaces later as either a migration project or a permanent tax on training cost. Evaluate against the actual workload pattern: bursty parallelism, untrusted code execution, and state that must survive across many rollouts.


Blaxel is the perpetual sandbox platform built for[AI agents](https://blaxel.ai/blog/ai-sandbox) that execute code in production. Sandboxes stay in standby indefinitely with sub-25ms resume and zero compute cost while idle. They run on microVMs inspired by the technology behind[AWS Lambda](https://blaxel.ai/blog/aws-lambda-gpu) for hardware-enforced tenant isolation.


For RL workloads, that maps directly to the problems discussed above. It keeps exploratory code isolated, preserves rollout state between pauses, and avoids rebuild work when collection resumes. For guaranteed long-term persistence across sandbox deletion, volumes provide the durable storage layer.


For adjacent parts of the same stack, Batch Jobs fit fan-out trajectory collection.[Agents Hosting](https://blaxel.ai/agents-hosting) fits co-locating policy code with environments to reduce network roundtrip latency. MCP Servers Hosting fits tool execution patterns. Model Gateway fits centralized LLM routing, telemetry, and token cost control. Those products become relevant when the RL system extends beyond the sandbox. Agent serving, large-batch orchestration, tool execution, and model access each have a matching product.


Book a demo at[blaxel.ai/contact](https://blaxel.ai/contact) , or start free at[app.blaxel.ai](https://app.blaxel.ai/) .


Train and serve RL agents on Blaxel


Run exploratory rollouts in microVM sandboxes that resume in under 25ms, stay in standby indefinitely with zero idle compute cost, and scale from zero to thousands of parallel environments.


[Start free](https://app.blaxel.ai/)


## **Frequently asked questions**


### **Why do RL agents need stronger isolation than deployed agents?**


Deployed agents run a fixed set of trained actions. RL agents during training sample from the full action distribution, including destructive commands. The policy network has no concept of host security. It will probe filesystem boundaries, spawn unbounded processes, and attempt combinations no human anticipated. Containers share the host kernel and fail under this pressure. MicroVMs provide hardware-enforced boundaries that contain exploratory behavior.


### **What happens when an RL sandbox deletes state between episodes?**


The team loses all environment setup work. Cloned repositories, loaded datasets, installed dependencies, and cached artifacts all need rebuilding. That rebuild cost multiplies across every rollout in a training run. For algorithms that resume from checkpoints, a dirty or missing environment produces inconsistent rewards. Those inconsistent rewards corrupt the gradient signal and silently degrade the policy.


### **How does idle sandbox billing affect RL training economics?**


RL training alternates between high-parallelism collection phases and synchronous policy updates. During updates, rollout sandboxes sit idle. Platforms with one-minute or fifteen-minute billing minimums charge for compute the agent isn't using. Per-second billing with fast auto-shutdown compresses the bill to actual work. The metric that matters is cost per million trajectories, not cost per sandbox-hour.


### **Why aren't containers safe enough for RL training workloads?**


Containers share the host kernel with the processes they isolate. The CVE record shows repeated container escapes through runc and other runtime components. AWS states it doesn't consider containers a security boundary. An RL agent actively exploring edge cases creates higher escape risk than a deployed agent running predictable actions. MicroVMs run a separate kernel per workload, adding a hardware-enforced boundary.


### **What concurrency does RL training infrastructure need to support?**


Modern RL methods run hundreds to thousands of parallel environments per training step. Published systems report sustained operation at over 1,000 CPU cores and more than 4,000 concurrent sandboxes. The infrastructure needs to handle that count without per-environment provisioning friction. It also needs to scale back to zero between runs without carrying idle costs.
