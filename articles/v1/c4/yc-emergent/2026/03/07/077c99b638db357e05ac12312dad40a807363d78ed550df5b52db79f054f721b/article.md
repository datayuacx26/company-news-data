---
schema_version: "1.0.0"
document_id: "077c99b638db357e05ac12312dad40a807363d78ed550df5b52db79f054f721b"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/real-environments-for-ai-agents-and-why-we-bet-on-kubernetes"
published_at: "2026-03-03T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:00.368523+00:00"
fetched_at: "2026-07-28T21:57:38.048658+00:00"
content_hash: "sha256:0d07889cac5803dff19d8334a0bfd0113c174d34349582bf06995f66e0d40569"
---

# Real Environments for AI Agents: Why We Bet on Kubernetes

*Part 1 covers the architecture: why we chose Kubernetes over VMs and lightweight sandboxes, how we solve persistence with a single content-addressed approach, and how isolation works when you give root-equivalent access to an AI agent in every pod. In Part 2, we cover the scaling layer: the warm pool, multi-cluster orchestration across multi-region production clusters, and what we learned the hard way.*


## Introduction


### What Emergent Does


Emergent is an AI-powered software development platform. You describe the product you want to build — a SaaS dashboard, a REST API, a data pipeline — and our agent takes it from there: planning the work, writing the code, installing dependencies, running tests, and deploying a live, working application. The goal is to compress the time between an idea and a deployed product from days to minutes.


When a user describes what they want to build, Emergent assigns an AI agent to a private workspace. The agent writes code, runs tests, and sets everything up automatically. Once it is ready, the user gets a live preview link to see the app in action. If everything looks good, they can deploy it to production with a single click and get a hosted link to share with anyone.


Building and running real software is not just about writing code. It requires a place to work.


## Why the Environment Matters?


Consider how a software engineer works on a remote development environment. They SSH into an environment, write and edit code directly on the filesystem, install system packages and language dependencies, run tests, check logs, and trigger deployments — all from the same environment. The environment has memory, persistent state, and a network. It behaves like developer’s local environment because it essentially *is* a close replica of it.


An agent doing software development needs the same thing. With a real environment, an agent can run the code it generates, verify assumptions, and test the output. The environment is foundational for closing the development loop and elevates the agent from a code generator into a system that can build and ship software.


‍


**The output is production-grade because the agent operates in a production-grade environment.** That’s the thesis.


## The Fundamental Challenge


We launched Emergent eight months ago. We grew faster than any of our initial infrastructure assumptions — from a few hundred concurrent environments to over 30K+ (and continuously growing). The environment management system had to evolve at the same pace.


The one conviction that held through all of it: **AI coding agents need real infrastructure, not lightweight sandboxes dressed up as infrastructure.**


Each one is a full Linux environment with its own filesystem, network isolation, and compute budget. The difficulty isn’t just provisioning at scale — it’s solving for three distinct imperatives simultaneously;


- **Startup speed** — spinning them up fast enough to keep pace with user demand
- **State durability** — reliably persisting work to survive any disaster
- **Clean teardown** — releasing resources completely when sessions end


All of this while working against the grain of Kubernetes, which is explicitly designed to treat pods as disposable. Every architectural decision in this series flows from that tension.


## Two Common Approaches


### The VM approach: accurate but slow


VMs are honest. Give an agent a full VM — a dedicated GCE instance or EC2 environment — and it gets` root` , a real filesystem, system packages, databases, the whole stack. The agent can produce real software: security headers, database migrations, SSL configuration, actual auth flows. The environment stops being the excuse.


The problem is speed. VM provisioning takes 30–120 seconds. At scale, dedicated VMs, each with its own OS image, boot sequence, and persistent disk. The cost is significant, scaling is measured in minutes, and the user experience begins with waiting before the agent writes its first line of code.


### The Lightweight sandbox approach: fast but constrained


Sandboxes trade capability for speed. Many sandboxed runtimes — WebContainers, edge functions, browser-based runtimes — spin up in milliseconds and the agent starts immediately. For simple use cases, it feels frictionless.


In our workloads, the ceiling shows up quickly. The moment the agent needs a real database, a native Node addon,` apt-get install` , server-side rendering with a persistent backend, or an auth flow that sets secure HTTP-only cookies — typical sandbox constraints made it hard. The agent generates code fast, but the code is constrained by the environment: no real databases, no system packages, no background processes, no multi-port services.


For the current generation of lightweight sandboxes, this is a structural ceiling, not a model limitation. When an agent cannot run a MongoDB migration, cannot start Redis, cannot execute a build pipeline that requires filesystem access, it generates workarounds — mock data instead of real queries,` localStorage` instead of sessions, client-side patterns instead of server-side security. The NFRs that separate a prototype from a deployable application — security, persistence, observability, performance — are simply out of reach.


We needed a different approach: full-environment capability at near-edge speed. That meant Kubernetes ephemeral pods with a warm pool for low latency startup, content-addressed state backup for durability, and bin-packing with auto-scaling to make the economics hold.


## The Industry Is Converging


The split between the two approaches did not hold for long. By early 2026, the infrastructure market had begun moving toward stateful environments as a first-class primitive. One provider shipped Firecracker-based microVMs with persistent storage and checkpoint/restore; another sandbox provider added pause and resume; and another advertises sub-90ms sandbox creation. On GCP, Google introduced[Agent Sandbox](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox) at KubeCon NA 2025, including WarmPools via a dedicated CRD, which addresses the same pre-warming challenges we will discuss in Part 2.


Likewise, unikernels seem like a promising direction, and we’re actively evaluating them.


This convergence reflects a genuine shift in how the industry understands agent infrastructure. The microVM approach, Firecracker combined with persistent storage, is meaningful progress: real Linux environments, actual filesystems, and fast cold starts without the overhead of a full VM. For single-developer workflows and moderate concurrency, it is a strong foundation.


We’re actively evaluating microVMs and unikernels for future iterations, but Kubernetes is what best fit our workloads and reliability requirements when we made the call.


In our experience, many of the hardest problems weren’t in the isolation layer. Firecracker, gVisor, and Kubernetes pods all handle process and network isolation well enough. The harder problems emerge above the isolation boundary: persisting state cheaply across millions of snapshots, scheduling across clusters when a zone reaches capacity, pre-warming pools using demand prediction without over-provisioning, and handling graceful shutdown when a pod termination signal must trigger a state checkpoint before the process exits. These are the problems the rest of this series is about.


## Why We Bet on Kubernetes?


On Emergent, every user prompt gets its own isolated, dev environment: a Kubernetes pod with a reverse-proxied preview URL that streams updates in real time. Inside that pod, the agent can install system and language packages, run arbitrary shell commands, build and test the code, start background services, and serve the app end-to-end. Hitting our quality bar meant this environment had to stay unconstrained, fast to start, and able to scale up vertically when a workload demanded it.


*At Emergent, we made this architectural bet from the start. Instead of putting effort on using lightweight sandboxes we would rather spend that time solving the harder problems than waiting to see if the bet paid off.*


We needed real Linux with low latency startup at scale. We evaluated VMs, sandboxes, and Kubernetes pods, and chose the best fit for our use case.


When designing systems at scale, there are always tradeoffs:


- Availability
- Scalability
- Security
- Cost
- Latency


There are also other, human factors:


- Technology's maturity
- Team's familiarity with the technology
- Team bandwidth (e.g. 2-person vs 10-person)
- Buy vs build decision


With all these dimensions in consideration, we focused to solve for the current state — and we continue to iterate through our decisions whenever any of these dimensions change meaningfully. If the infra team grows, you can afford what would have been an otherwise discarded solution. A buy vs build decision can flip later. Maybe latency is not important at some point, but later it becomes critical.


For Emergent, because we want to control a tight feedback loop between agents and the underlying dev environment, we wanted to own our infra end to end. That eliminated the "buy" option for sandboxes.


Because we run long-running agents where the human is in the loop only initially, latency is not the most important metric. **Security, Reliability, Availability, and Resumability** are more important. With these in mind, we bet on tried and tested Kubernetes as the infrastructure of choice for our environments.


### The Comparison


*The numbers above tell part of the story. Two dimensions — security isolation and ecosystem depth — tell the rest.*


Dimension VMs (GCE/EC2) Edge / Sandbox Micro VMs (Firecracker) K8s Pods (Ours)


Startup time 30–120s <1s ~125ms (kernel-only) / 3–5s (image pull + init) <8s (warm pool)


Full Linux + root Yes No Yes Yes


Database support Native Managed only (Convex, Supabase) Native Native


Security isolation Hypervisor Process-level Hypervisor (KVM) eBPF + namespace + cgroups


Ecosystem maturity Mature Young Young (no scheduler) Mature (CNCF)


Scheduling / bin-packing Manual or custom N/A Custom orchestration Built-in


Relative cost per env ~4–5x 1x (baseline) ~3–4x ~1x


*‍*
* Cost figures compare published pricing of commercial offerings against our internal cost at production scale.


WHY BIN-PACKING CHANGES THE ECONOMICS


AI agents spend most of their time waiting for LLM responses. With VMs, you pay for 100% of the allocation during the wait. With bin-packing, idle resources can serve other pods.


For an agent environment, the critical path is everything that follows: fetching and unpacking a full filesystem image, then initialising Node.js, Python, PostgreSQL, and a code server. In practice, that adds 3–5 seconds of image pull and service init even on well-provisioned hosts, which closes most of the headline gap against a warm-pooled container.


### The Ecosystem Advantage


We did not build most of our infrastructure. We composed it.


Scheduling and bin-packing across heterogeneous node pools — different instance types, zones, and capacity profiles — is handled by the Kubernetes scheduler. When we need to drain a node or shift workloads between zones, we use standard primitives: cordons, taints, and pod disruption budgets. Not custom orchestration code.


The CNCF ecosystem provides service mesh, observability, policy engines, autoscalers, and admission controllers — all maintained by communities. When we needed distributed tracing, we added OpenTelemetry. When we needed cost attribution per environment tier, we used Prometheus metrics we were already collecting.


This is the compounding advantage of a mature platform: the higher-order problems get solved by the ecosystem, and the engineering team stays focused on the problems specific to Emergent.


## Architecture: How Ephemeral Pods Work


If you want low latency startup at high scale concurrent environments, you need a tight lifecycle. Every environment pod follows the same three phases. Understanding them explains most of our operational decisions — including the ones we got wrong.


### Three Phases, One Pod


Each agent session maps to exactly one Kubernetes pod. That pod runs three containers in a defined sequence, each with a distinct responsibility.


**Phase 1 — Init: Restore**


Before the main container starts, an init container pulls the user's previous working state from a content-addressed backup in object storage. If it is the user's first session, it bootstraps from the base image instead. Either way, by the time the main container starts, the filesystem looks exactly like where the user left off. This phase typically completes in 2–6 seconds and exits — it runs once and is done.


**Phase 2 — Main: Environment**


This is the development environment itself. Full Linux with root access: Node, Python, a database, a code server, and live preview serving over a reverse-proxied URL. The agent installs packages, runs builds, starts services, and executes arbitrary shell commands. Nothing is simulated. It is a real environment, and the main container runs unprivileged — root is scoped to the container, not the node.


**Phase 3 — Sidecar: Operations**


A lightweight privileged container that handles everything the main container should not have permissions for. During the pod's lifetime, it runs heartbeats, health reporting, and operational telemetry. On pod termination, its` preStop` hook performs an incremental backup of the working state back to object storage before the pod exits.


This split — unprivileged main container, privileged sidecar — keeps the agent's attack surface small while still allowing the operations that require elevated access. The pattern was inspired by how GCS FUSE itself runs as a sidecar with its own credential scope.


### The Persistent Volume


Each pod mounts a fast network-attached SSD as its working volume. Modern cloud SSDs are performant enough that the traditional split between local cache for speed and network storage for durability is no longer necessary — a single network-attached disk gives us both. The pod reads and writes directly to this volume at near-local speeds.


Critically, the volume survives node failures. If a node dies mid-session, the data remains intact. Kubernetes reschedules the pod to a healthy node, reattaches the volume, and the init container picks up from the last backup. From the user's perspective, the session resumes where it left off.


### Security Isolation at Scale


This is where the decision stops being close.


GKE Dataplane V2 gives us eBPF-based networking — kernel-level packet processing that replaces traditional iptables-based service routing with in-kernel service routing and policy enforcement. At scale, kube-proxy's iptables chains become operationally painful: rule reloads on endpoint changes can take minutes, and service routing requires linear rule evaluation. Dataplane V2's eBPF programs use hash-map lookups for service routing and per-endpoint policy maps for network enforcement, eliminating reload overhead entirely.


This single primitive gives us both network policy enforcement and security observability across the fleet:


- **Zero cross-talk between environments.** Namespace-level NetworkPolicies ensure no pod can reach any other pod. Every environment is a network island — no lateral movement, no accidental service discovery, no data leakage between sessions.
- **Egress restricted to the public internet only.** Pods can reach the open internet — they need to for package installs, API calls, and serving live previews — but all access to private infrastructure, internal services, and cloud metadata endpoints is blocked at the kernel level.
- **Signature-based abuse detection.** eBPF telemetry feeds pattern matching in user space: cryptocurrency mining signatures, unexpected outbound connections, anomalous resource consumption — all flagged in near-real-time without per-pod sidecar agents. At scale where each agent has root access, proactive detection is not optional.


Above the network layer, admission controllers enforce Pod Security Standards on every pod spec before it is admitted to the cluster. Resource containment is handled by cgroups: CPU limits, memory limits, ephemeral storage caps to prevent any single pod from filling a node, and PID limits that make fork bombs a non-event for neighbouring pods.


The agent has root inside its container — it needs root for` apt-get install` , database setup, and system configuration. That will make any infrastructure engineer pause. Here is why it does not concern us: the container boundary, namespace-level network policies, eBPF kernel enforcement, eBPF detection rules, cgroup resource limits, and admission controllers each address a distinct attack vector. No single layer is the wall. The stack is the security model.


## What's Next?


Part 2 is about the problem we had to solve first: making environments at scale start with low latency — reliably, cheaply, and when the cloud is having a bad day.


- **The warm pool** — why the obvious "just keep pods around" approach explodes your bill long before it improves your latency
- **VolumeSnapshot scaling limits (millions of snapshots)** — the quiet scaling limit that turned our first persistence strategy into a ticking clock
- **Object storage-based backup** — how we replaced VolumeSnapshot with a content-addressed backup system that scales without an upper bound
- **Three-tier instance fallback** — what happens when your preferred node type sells out at 2 AM
- **Moving to multi-region and multi-cluster architecture** — how we broke etcd, why we moved off VolumeSnapshot, and the multi-cluster architecture that two engineers keep running
- **Full app-fork and environment cloning** — how we solved the problem of spinning up an exact copy of a running environment, and where that capability opens up for users.


## Acknowledgements


Building and operating infrastructure at this scale is never easy. This system exists because of the entire Emergent engineering team - the on-call engineers who debugged pod scheduling failures at 2 AM, the platform work that made multi-cluster operations manageable, and every person who shipped a fix under pressure and wrote down what they learned.


If you are passionate about AI and Technology and love solving deep problems,


[We are hiring](https://emergent.sh/careers) .
