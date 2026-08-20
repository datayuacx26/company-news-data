---
schema_version: "1.0.0"
document_id: "f4211a61631358daaea2d8b4e58c75cdf7bb2953865f26eb8f89661a4b93cf2f"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/run-ai-generated-code-safely"
published_at: "2026-06-04T12:34:47+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:341caf6b4d9fd47d6830c54014fcbf578a578c5e8f75a4a5843d26dbecc71a47"
---

# How to Safely Run AI-Generated Code in Production

Picture this: an engineering leader signs off on shipping an agent that generates and runs Python scripts at runtime. The team celebrates. Soon after launch, a post-mortem traces the outage to a generated query. It consumed all available memory on the host.


The shell command was syntactically valid, contextually reasonable, and operationally destructive. Every team that runs AI-generated code in production faces this trajectory. The agent works in staging. Then the unreviewed output of a probability distribution meets real infrastructure.


Model-generated code is unreviewed by definition. Every line that reaches production was written by a system with no awareness of host security or data boundaries. Organizational impact doesn't factor into its output. The infrastructure layer has to treat that code as hostile until proven otherwise.


[Researchers have reported](https://arxiv.org/abs/2309.02926) remote code execution vulnerabilities across multiple agent frameworks. These often involve unsafe execution of model-generated strings via functions like` exec()` ,` eval()` , or shell APIs without validation.


This guide covers three things. First, the threat model that determines what isolation needs to do. Second, the architectural decisions that turn that model into running infrastructure. Third, the operational requirements that separate prototypes from production.


## **The threat model when you run AI-generated code in production**


Human-written code goes through review and intent. A developer writes a database query knowing which tables exist and which columns contain sensitive data. Model-generated code carries none of that context.


It's sampled from a probability distribution that includes destructive actions, infinite loops, and resource exhaustion patterns. Frontier models' success on cybersecurity tasks rose from under 10% in 2023 to roughly[50% in 2025](https://www.aisi.gov.uk/frontier-ai-trends-report) . The code your agent generates can be adversarial in capability, even when intent is benign.


The threat surface has three layers. The first is the host: kernel, filesystem, and peer tenants. A[Linux kernel vulnerability (CVE-2026-31431)](https://nvd.nist.gov/vuln/detail/CVE-2026-31431) was confirmed as actively exploited shortly after disclosure. It affected virtually all Linux distributions running kernels since 2017. The second layer is the network: outbound destinations, secrets in transit, and customer-facing endpoints.


[MITRE ATLAS research](https://www.mitre.org/sites/default/files/2026-02/PR-26-00176-1-MITRE-ATLAS-OpenClaw-Investigation.pdf) describes prompt injection, persistence, and exfiltration risks in autonomous agents. The third layer is data: replay across sessions, leakage between customers, and exfiltration to attacker-controlled endpoints.[Researchers have examined](https://arxiv.org/abs/2504.00018) sandbox security and isolation risks in LLM environments, including filesystem manipulation and data exposure.


A useful framing for engineering leaders: assume any line of generated code could be the worst case in your threat model. The infrastructure either contains that worst case or absorbs the cost. Agents that generate executable code are moving into production for coding assistants, data analytics, and tool-use systems.[McKinsey's 2025 survey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai) found that 23% of respondent organizations are scaling agentic AI. That makes the infrastructure decision immediate.


## **Isolation primitives that determine your safety floor**


The isolation choice is the foundation on which everything else is built. Three primitives dominate the market. Each makes different trade-offs between boundary strength, boot speed, and cost. This section covers what each primitive actually guarantees. The goal: a defensible choice rather than an inherited default.


### **MicroVM-based isolation**


Firecracker, Cloud Hypervisor, and similar microVM technology give each execution its own kernel. Hardware-virtualized memory boundaries separate every workload. The[USENIX NSDI '20 paper](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) from the Firecracker team states the core benefit. It moves the security-critical interface from the OS boundary to one supported in hardware and simpler software.


The host kernel is unreachable from inside the sandbox. Guest syscalls don't reach the host directly. They're mediated through the KVM hypervisor layer. The[Firecracker VMM](https://firecracker-microvm.github.io/) exposes only a small set of emulated devices compared to QEMU.


The[Firecracker specification](https://github.com/firecracker-microvm/firecracker/blob/main/SPECIFICATION.md) reports boot time of 125 milliseconds or less under specified test conditions. Memory overhead runs about 5 MiB per microVM. The spec doesn't state a guarantee that guest CPU performance stays above 95% of bare metal. Those numbers make microVMs practical for latency-sensitive agent workloads. The trade-off is slightly higher per-instance overhead than containers. That matters for cost modeling at high concurrency.


### **Container-based isolation and the shared-kernel question**


Containers (Docker, containerd, Kubernetes-managed runtimes) share the host kernel across all workloads. They're operationally familiar, fast to start, and appropriate for trusted first-party code. AI-generated code doesn't qualify as trusted.[NIST SP 800-190](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf) says directly: container runtimes don't provide isolation as strong as hypervisors.


The shared kernel widens the attack surface in two ways. First, kernel-level exploits in generated code can reach the host.[CVE-2019-5736](https://nvd.nist.gov/vuln/detail/cve-2019-5736) and[CVE-2024-21626](https://nvd.nist.gov/vuln/detail/cve-2024-21626) share the same root cause class across multiple years. Both involve file descriptor mishandling in runc. Second, peer-reviewed research at[USENIX Security '23](https://www.usenix.org/conference/usenixsecurity23/presentation/he) demonstrated eBPF-based attacks that break container isolation.


Researchers enabled[container escapes](https://blaxel.ai/blog/container-escape) and cross-container attacks in Kubernetes clusters. They compromised five online interactive shell services and Google Cloud Platform Cloud Shell. Container-based sandboxing works for development environments and trusted workloads. It's the wrong default for multi-tenant systems running model-generated code.


### **Language-level and WebAssembly sandboxes**


V8 isolates and WebAssembly runtimes restrict execution to a narrow language-level boundary. The[WebAssembly specification](https://webassembly.org/docs/security/) guarantees that applications execute independently. They can't escape the sandbox without going through appropriate APIs. Linear memory stays isolated from host runtime internals. Control-flow integrity prevents traditional exploits like code injection.


Those guarantees hold for computationally bounded, deterministic workloads. They break down for agents that need full shell access, arbitrary package installs, or process spawning. WebAssembly is deliberately host-environment-independent. It provides no ambient access to the computing environment. I/O, resource access, and operating system calls are only possible through host-provided imports.


Even within Wasm's narrower scope, documented escapes exist.[CVE-2025-43853](https://www.cve.org/CVERecord?id=CVE-2025-43853) confirmed a filesystem sandbox escape in the WebAssembly Micro Runtime via symlink traversal. Wasm works for plugin systems and function-style execution. For multi-tenant systems running AI-generated code from external prompts, cloud-side execution with hardware-enforced isolation is the safer default.


## **How to architect a production-grade execution layer for AI-generated code**


Once the isolation primitive is chosen, three sequential decisions turn it into a production architecture. Each decision constrains a different attack surface and determines a different cost profile. The order matters: each decision narrows the option space for the next one.


### **1. Choose an isolation model that matches your threat profile**


The isolation model is a decision about the worst case the team is willing to defend against. For trusted internal tools where prompts come from your own team, container-based isolation may be sufficient. For multi-tenant production agents running generated code from external prompts, microVM isolation is the practical floor. The[CNCF security whitepaper](https://www.cncf.io/wp-content/uploads/2022/06/CNCF_cloud-native-security-whitepaper-May2022-v2.pdf) makes the same distinction. For untrusted workloads in multi-tenant environments, a VM-based sandbox is the recommended approach.


Document the threat model explicitly. The choice needs to hold up during compliance reviews and post-mortems. Match the boundary to the actor. If the prompt comes from an end customer, assume the customer is the attacker. If the prompt comes from an internal tool, the boundary can relax. Start by writing down three things. Who provides the prompt? What does the worst-case generated output look like? What's the blast radius if that output executes?


### **2. Define lifecycle and state boundaries for execution sessions**


Each session running AI-generated code has a lifecycle. Start, suspend, resume, and termination all need defined semantics. State persistence across these transitions is a security decision as much as a performance one. Sessions that share state across customers leak data. Sessions that delete state aggressively force expensive rebuilds. That degrades the user experience for coding agents and data analysis workflows.


The right pattern is per-tenant isolation with explicit standby behavior. The sandbox preserves filesystem and memory state for that tenant, with no cross-tenant access at any layer.[AWS's Well-Architected Security Pillar](https://docs.aws.amazon.com/pdfs/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf) addresses workload separation directly. It rates the risk of skipping account-level isolation as "High."


Most providers cap standby duration or remove state after short idle windows. E2B deletes paused sandboxes after 30 days. Daytona auto-archives stopped environments after 30 days, and restoring them takes longer than starting a stopped one. Daytona also enforces a 15-minute default idle timeout.


Modal's standby feature is in alpha with a 7-day cap, after which snapshots are deleted. CodeSandbox hibernates inactive sandboxes within 2 to 7 days at its discretion, depending on infrastructure strain.


Cloudflare Sandboxes are ephemeral. All files are deleted when the sandbox pauses. Those lifecycle differences determine whether stateful agent sessions stay usable in production. Audit which lifecycle stage your agent sessions spend the most time in. Pick infrastructure that optimizes for that stage.


### **3. Constrain the network surface around generated code**


Network egress is where leaked data leaves and where remote-control instructions arrive. Generated code that calls out to attacker-controlled domains is the canonical exfiltration pattern.


[Research on LLM exfiltration](https://arxiv.org/html/2602.22450v1) found that keyword filtering triggered on only 23.3% of runs. Models encode or reformat sensitive strings to avoid detection. That study evaluated defenses including URL and domain-based mitigations. The finding favors architectural controls over content inspection alone.


The architecture needs explicit network controls. Allow-list outbound destinations where possible. Route all egress through a controllable gateway. Inject secrets through a proxy layer so credentials never appear in agent code.


[NIST SP 800-207A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207A.pdf) describes a reference platform using sidecar proxies and application identity infrastructure like SPIFFE to enforce policies. Inbound traffic needs the same treatment for any agent endpoint exposed to customers.


Three networking primitives make this enforceable. Custom domains let teams white-label endpoints. Dedicated outbound IPs let customers allow-list traffic. Proxy-based secrets injection keeps credentials out of agent code and environment variables.


If the architecture also exposes tools through a standardized tool layer, MCP Servers Hosting is relevant here. Tool execution and API integration need the same boundary and observability decisions.


## **Operational requirements beyond the sandbox**


The sandbox itself is necessary but not sufficient. Two operational layers determine whether the architecture survives enterprise customers and audit cycles.


### **Production-grade networking primitives**


Production networking is where managed platforms differ most from DIY approaches. Three capabilities matter for teams shipping agents to enterprise customers. Custom domains support white-labeling agent endpoints. Dedicated outbound IPs let customers allow-list traffic from your agents. Secrets injection via proxy routing keeps credentials from touching generated code.


The perpetual sandbox platform[Blaxel](https://blaxel.ai/sandbox) ships proxy-based secrets injection and custom domains natively.[Dedicated egress gateways](https://docs.blaxel.ai/Infrastructure/Dedicated-egress-gateways) are in private preview as of this writing. Some competing platforms require teams to assemble more of this networking stack themselves. E2B requires a[self-hosted workaround](https://e2b.dev/docs/sandbox/connect#custom-domain) for custom domains.


Daytona requires users to implement their own proxy-based custom domains for public URLs. The engineering cost of building these yourself is substantial. Each primitive requires its own design, implementation, and ongoing maintenance. For teams running AI-generated code in multi-tenant production, managed platforms remove that work. The team focuses on agent capabilities instead.


### **Observability and compliance posture**


OpenTelemetry traces that span the agent's reasoning loop and the executed code give visibility into what happened and why. Per-tenant metrics matter for cost attribution and post-incident analysis. Without them, the team can't answer "which customer's workload caused the spike" during an outage.


SOC 2 Type II certification often takes months end-to-end the first time. Year-one costs for startups typically start in the tens of thousands of dollars and extend higher depending on scope. ISO 27001 adds another[three to 12 months](https://www.complyjet.com/blog/iso-27001-certification-cost) .


Frame compliance as inherited posture: vendor compliance becomes your compliance during enterprise reviews. Missing certifications either block deals or push the work onto your team's roadmap. The sales cycle stalls while the team catches up. Those timelines and costs slow infrastructure decisions when the team plans to build everything itself.


## **Common pitfalls when teams build this themselves**


Build versus buy is the central decision for engineering leaders. The DIY path looks cheaper on paper and routinely overruns in practice.


Five patterns recur across teams that choose to build:


- **Underestimating the operational surface:** Teams budget for Firecracker setup and miss the networking, observability, and compliance work. The[Firecracker production host guide](https://github.com/firecracker-microvm/firecracker/blob/main/docs/prod-host-setup.md) requires disabling SMT, disabling Kernel Samepage Merging, per-instance UID/GID isolation, and jailer configuration with cgroups. Each of these is a separate workstream.
- **Hiring against the wrong skill set:** Sandboxing requires specialized kernel configuration and microVM lifecycle experience. Senior DevOps engineers at large tech companies earn[$180,000 to $304,000](https://www.levels.fyi/companies/salesforce/salaries/software-engineer/title/devops-engineer) in total compensation. AI-native startups competing for this specialization should expect expensive senior hiring.
- **Skipping the multi-tenant problem:** Single-tenant sandboxes work in development. They break the moment two customers share infrastructure. The retrofit is expensive because it touches isolation, state management, and networking simultaneously.
- **Treating compliance as a checkbox:** SOC 2 Type II takes[six to 15 months](https://www.complyjet.com/blog/soc-2-compliance-guide) the first time. Building compliance in parallel with infrastructure delays both.
- **Missing the standby economics:** Sandboxes that run continuously to avoid cold starts pay for idle compute. Managed platforms can return sandboxes to standby after brief network inactivity. Blaxel documents standby triggered after[15 seconds of inactivity](https://docs.blaxel.ai/Sandboxes/Overview) . That changes the cost profile for bursty agent workloads.


The alternative: choose a managed platform that has already done this work.


## **How to evaluate sandbox platforms for AI-generated code**


The evaluation question for engineering leaders is which sandbox removes the most risk from production. Five questions cover most of the decision.


- **Isolation model under your threat profile:** Hardware-virtualized boundaries hold up better than shared-kernel approaches for generated code from external prompts. Ask the vendor whether isolation is at the hypervisor level or the kernel level. Request documentation. NIST SP 800-190 provides security guidance for application containers but doesn't offer a specific framework for evaluating that difference.
- **Standby duration and resume cost:** Stateful agent sessions need filesystem and memory preserved across idle periods. Faster resume keeps session boundaries cheap. Ask what happens to sandbox state over longer inactivity periods. That answer determines whether agents preserve useful working state without forced rebuilds.
- **Native networking primitives:** Custom domains, dedicated outbound IPs, and proxy-based secrets injection should ship with the platform. They shouldn't require side projects on separate infrastructure. Ask whether each primitive is generally available or in preview. That determines how much networking work your team still owns.
- **Compliance certifications already in hand:** SOC 2 Type II, ISO 27001, and HIPAA BAA in place at the vendor shorten enterprise procurement. Ask for the certification date and the auditing firm. That affects whether security review accelerates deployment or becomes the next constraint.
- **Support quality and engineering access:** Direct engineering access matters more than dashboards when production breaks during a customer demo. Ask for the escalation path and response time SLA. That determines whether an outage becomes a short debugging session or a customer-facing incident.


Those five answers determine whether the platform supports production workloads or stalls the project at staging. If you're evaluating the broader stack, check whether the vendor supports agent deployment, tool execution, and model routing too. In Blaxel's stack. Model Gateway provides model access alongside sandboxes.


## **Build infrastructure to safely run AI-generated code in production**


How to run AI-generated code is a multi-year commitment. It shapes incident exposure, compliance posture, and unit economics. Picking infrastructure optimized for trusted internal workloads when the actual workload is multi-tenant generated code surfaces later. It becomes either a security incident or a migration project.[Gartner predicts](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) that over 40% of agentic AI projects will be canceled by end of 2027. Infrastructure that can't scale safely will be one reason.


[Blaxel](https://blaxel.ai/blog/ai-sandbox) is the perpetual sandbox platform built for AI agents that execute code in production. Sandboxes run on microVMs with hardware-enforced isolation. They resume from standby in under 25 milliseconds. That keeps stateful sessions responsive without forcing teams to leave compute running.


Sandboxes return to standby after 15 seconds of network inactivity. They stay in standby indefinitely and incur zero compute charges while idle. That changes the[economics of bursty workloads](https://blaxel.ai/blog/ai-agent-development-costs) . Blaxel ships with SOC 2 Type II and ISO 27001, with a HIPAA BAA available as an add-on. Native networking includes custom domains and proxy-based secrets injection. Dedicated egress gateways are currently in private preview.


Teams that need the agent runtime and execution layer on one platform benefit from co-location. Agents Hosting places agent logic alongside sandboxes. That eliminates network roundtrip latency. MCP Servers Hosting handles tool execution. Model Gateway centralizes model access and observability across providers. That combination matters when the problem isn't isolated execution alone, but[production deployment around it](https://blaxel.ai/blog/ai-runtime-security) .


Book a demo at[blaxel.ai/contact](https://blaxel.ai/contact) , or start free at[app.blaxel.ai](http://app.blaxel.ai/) .


Run AI-generated code safely on Blaxel


Execute untrusted model output in microVM sandboxes with hardware-enforced isolation, sub-25ms resume, native egress controls, and proxy-based secrets injection.


[Start free](https://app.blaxel.ai/)


### **Frequently asked questions**


**What is the safest way to run AI-generated code in production?**


MicroVM-based isolation provides the strongest security boundary for running AI-generated code. Each execution gets its own kernel and hardware-virtualized memory, preventing generated code from reaching the host or other tenants. Container-based approaches share the host kernel, which creates escape risks when code is unreviewed. Pair microVM isolation with network egress controls, proxy-based secrets injection, and per-tenant state boundaries to cover the full threat surface.


**Why can't containers safely sandbox AI-generated code?**


Containers share the host operating system kernel across all workloads. Generated code running inside a container can exploit kernel-level vulnerabilities to escape isolation and access host resources or other tenants' data. NIST SP 800-190 confirms that container runtimes provide weaker isolation than hypervisors. Multiple CVEs demonstrate this risk, with runc file descriptor mishandling enabling container escapes across years of development.


**How does network egress control prevent data exfiltration from AI agents?**


Generated code can call attacker-controlled domains to leak sensitive data. Research shows keyword filtering catches only 23.3% of exfiltration attempts because models encode strings to avoid detection. Architectural controls work better. Allow-list outbound destinations, route egress through a controllable gateway, and inject secrets via a proxy layer so credentials never appear in the agent's code or environment variables.


**What compliance certifications matter for AI code execution infrastructure?**


SOC 2 Type II, ISO 27001, and HIPAA with a Business Associate Agreement cover most enterprise procurement requirements. SOC 2 Type II takes six to 15 months the first time. ISO 27001 adds three to 12 months. Missing certifications either block enterprise deals or push compliance work onto your engineering roadmap. Inheriting certifications from a managed platform vendor shortens procurement cycles.


**What is perpetual standby and why does it matter for AI agents?**


Perpetual standby keeps a sandbox's filesystem and memory state preserved indefinitely with zero compute charges while idle. When an agent needs the sandbox again, it resumes in milliseconds instead of rebuilding from scratch. This matters for coding assistants and data analysis agents that maintain long-running sessions. Without perpetual standby, teams either pay for idle compute or force expensive environment rebuilds between interactions.
