---
schema_version: "1.0.0"
document_id: "6d04748685b1f0719eed43ef8bd67a14959bb0549cc805d2a1797e811231373b"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/run-untrusted-llm-generated-code-safely"
published_at: "2026-08-19T16:25:45+00:00"
first_seen_at: "2026-08-19T18:44:07.874563+00:00"
fetched_at: "2026-08-19T18:44:10.590664+00:00"
content_hash: "sha256:0a13b6b48d88ab3846ec22eb4407cc6eee3d42f751ecfbb37ea4e4beb1533e43"
---

# How to run untrusted LLM-generated code safely in production

Your coding agent generates a Python script, executes it inside a container, and returns the output. It works in staging. In production, a prompt injection steers the agent toward malicious code. That code reads environment variables and opens a reverse shell to a neighboring tenant's database. The container shared a kernel with every other workload on the host.


LLM-generated code is untrusted code. The model doesn't need malicious intent to create risk. A single adversarial input or hallucinated import can exploit the execution environment. Traditional application security assumes humans review code before it runs. Agents skip that step. The infrastructure underneath has to enforce what humans no longer verify.


**TL;DR:**


- **Untrusted by definition:** LLM-generated code breaks the review-then-deploy model. The executable surface changes every request, and traditional gates can't keep up.
- **Isolation boundary first:** MicroVMs provide hardware-enforced per-workload kernels. Containers share the host kernel, giving exploits a path to neighboring tenants.
- **Runtime safeguards inside:** CPU and memory limits, execution timeouts, syscall filtering, and output validation constrain what code can do even when the boundary holds.
- **Network and credential controls:** Egress allowlists prevent data exfiltration. Proxy-based credential delivery keeps API keys out of agent code entirely.
- **Governance closes the loop:** Audit trails, compliance mapping to SOC 2 and ISO 27001, and HIPAA coverage provide the accountability chain for every agent execution.


## **Why LLM-generated code breaks traditional security models**


Traditional code execution security assumes humans controlled the code path. Continuous integration validated it. Deployment proceeded. LLM-generated code at runtime violates that assumption. The agent writes it, executes it, and acts on the output in a single session.


The threat model shifts across multiple dimensions. First, the code surface is unbounded. An LLM can generate any valid program. That includes programs that probe the host filesystem or open network connections. It can also consume resources until the environment crashes. Second, adversarial inputs steer code generation toward malicious behavior. The model doesn't choose to attack. Prompt injection is the primary vector.


The[OWASP LLM Top 10](https://genai.owasp.org/llm-top-10/) ranks it first. Indirect injection through data the agent reads is hard to detect. The code can look syntactically correct to static analysis while carrying a malicious payload.


Teams that apply traditional code review gates face a losing choice. They either block the agent from executing anything useful or review so superficially that the gate provides no protection. The security model has to move from pre-execution review to execution-time containment.


## **Isolation architecture for untrusted code execution**


Choose the isolation boundary first. It determines whether a compromised sandbox affects only the current session. Without the right boundary, the agent gets a path to the host, neighboring tenants, or shared infrastructure. The gap between isolation models isn't incremental. It separates a contained incident from a multi-tenant breach.


### **Choose the right isolation boundary**


Your choice depends on whether the code running inside has been reviewed.


Containers provide a process-level boundary while sharing the host kernel. gVisor intercepts system calls in user space to reduce the kernel surface. MicroVMs run a dedicated kernel per workload with hardware-enforced isolation. Containers work for running trusted, vendor-controlled software in multi-tenant settings. For untrusted LLM-generated code, the shared kernel creates a path from a compromised sandbox to the host. A[kernel exploit](https://blaxel.ai/blog/container-escape) in one sandbox can reach neighboring tenants.


The[Firecracker NSDI '20 paper](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf) frames this as a core tradeoff. Containers can limit syscalls to improve security. But that breaks code that needs those calls. MicroVMs enforce strong isolation using hardware virtualization. They reduce the attack surface to the host kernel. They don't eliminate all paths through hypervisor bugs, forwarded operations, or host-kernel vulnerabilities.


Use containers only when agents execute code your team has reviewed and approved. If agents execute arbitrary code generated at runtime, the isolation boundary needs to sit below the kernel. Ask potential providers what virtualization technology isolates each sandbox. Confirm whether the boundary is hardware-enforced or process-level. Verify whether one tenant's workload can access another's kernel memory.


### **Weigh persistent vs. ephemeral sandboxes for security**


Ephemeral sandboxes destroy state after each execution. Persistent sandboxes retain filesystem and runtime state across sessions. Both carry security implications worth weighing against your workflow.


Ephemeral sandboxes limit the blast radius of any single execution. They also force expensive re-initialization on every run. Persistent sandboxes improve performance and support multi-step agent workflows. They require controls around what data survives between sessions, how long state persists, and whether cleanup is time-based or event-driven.


Perpetual sandbox platforms like[Blaxel](https://blaxel.ai/products/sandboxes) use microVM isolation with sandboxes that resume from standby in under 25ms. Agent Drive, currently in private preview, shares context across sessions. Standby preserves filesystem and memory state for fast resume.


When data needs guaranteed long-term retention, use Volumes rather than relying on sandbox standby alone. This gives teams hardware-enforced isolation without sacrificing the state persistence that multi-step workflows require.


The right choice depends on your workflow. A data analysis agent that loads a large dataset benefits from persistence. A single-shot code formatter that runs once and returns output may not need it.


## **Runtime safeguards beyond the isolation boundary**


Isolation contains the environment. Runtime safeguards constrain what happens inside it. Even with hardware-enforced isolation, unchecked code can exhaust resources. It can exfiltrate data through allowed network paths. It can produce outputs that downstream systems trust without validation. These controls limit what the code can do even when the boundary holds.


### **Set resource limits and execution timeouts**


Set hard CPU and memory limits for every sandbox session. Add execution-duration timeouts. Without them, a hallucinated infinite loop or memory-intensive operation crashes the sandbox. That crash can affect the host's resource allocation.


Restrict each sandbox to a[specific memory limit](https://nvlpubs.nist.gov/nistpubs/ir/2017/nist.ir.8176.pdf) and a fixed number of CPU shares. The memory limit prevents denial-of-service attacks. CPU shares prevent resource starvation. Execution timeouts stop runaway processes from consuming compute indefinitely. Match the timeout to the expected task duration with a reasonable buffer.


One difference affects configuration. CPU-time limits measure cycles consumed. An infinite loop that sleeps between iterations evades them while still running forever in wall-clock time. Implement wall-clock timeouts at the orchestration layer for that reason. Confirm that your provider lets you set per-sandbox CPU and memory limits.


Check that execution timeouts are configurable per workload type. Verify that the provider terminates sandboxes gracefully rather than killing them without state capture. Resource limits are the cheapest safeguard with the highest return. They cost nothing to configure and stop unbounded execution before it reaches the host.


### **Validate outputs before acting on results**


Validate code output before any downstream system acts on it. An agent generates a SQL query, executes it in a sandbox, and passes the result downstream. That output path can carry malicious payloads even when the sandbox itself is secure.[OWASP LLM05](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/) documents the specific risks.


SQL injection from unparameterized queries is one. Remote code execution follows when output reaches` exec` or` eval` . Unsanitized file paths can lead to path traversal. Validate outputs at the boundary between the sandbox and the consuming system.


Check that each output matches the expected format and content. Enforce size limits. Treat sandbox outputs with the same suspicion as user inputs in a web application. Apply allow-list validation that defines exactly what structure the receiving system accepts.


System call filtering adds a layer inside the sandbox. Restrict filesystem access to the working directory. Limit network calls to allowlisted endpoints. Block privilege escalation syscalls. Defense in depth restricts both what code can do and where it can do it.


## **Network controls and credential security**


Agents inside sandboxes make outbound requests to APIs, databases, and external services. Without egress controls, a compromised agent can exfiltrate data to any endpoint. Without secure credential delivery, a compromised sandbox exposes every API key the agent needs.


### **Filter egress and control outbound traffic**


Egress allowlists restrict which external endpoints a sandbox can reach. Without them, a single compromised agent can send data anywhere on the internet. Cloud platforms don't enforce this by default.


An[AWS VPC default](https://docs.aws.amazon.com/prescriptive-guidance/latest/secure-outbound-network-traffic/restricting-outbound-traffic.html) security group allows all outbound traffic. Inverting the default to deny-all requires explicit operator action. DNS filtering is non-optional. Unit 42 documented a[DNS-based bypass](https://unit42.paloaltonetworks.com/bypass-of-aws-sandbox-network-isolation-mode/) of network isolation in a major cloud sandbox. A stated no-network mode must include DNS filtering alongside TCP/IP blocking.


Static outbound IPs matter for enterprises that need firewall rules governing sandbox traffic. Without them, teams route through self-managed proxy infrastructure. That adds latency and management overhead. Some providers offer managed egress gateways with domain filtering. Others require teams to build this layer themselves.


Blaxel's networking features include custom domains, domain filtering, and[proxy secrets injection](https://docs.blaxel.ai/Sandboxes/Proxy-secrets-injection) . Dedicated egress gateways, currently in private preview, assign static outbound IPs. Factor the work of self-hosted networking into your assessment.


Check whether your provider supports egress allowlists and static outbound IPs. Confirm that outbound traffic is logged and auditable. Verify that you can restrict egress per sandbox, not just at the account level. If the sandbox boundary fails, egress restrictions limit the blast radius.


### **Deliver credentials to sandboxes securely**


Agent workflows require service credentials: API keys, database credentials, and service tokens. Don't treat credential delivery as a single pattern. Three approaches exist, each with different security properties.


Environment variable injection places credentials in memory visible to all processes in the sandbox. This is the weakest option. Environment variables appear in logs, system dumps, and process listings. Avoid environment variables for secrets unless other methods aren't possible, as the[OWASP Kubernetes Top Ten](https://owasp.org/www-project-kubernetes-top-ten/2025/en/src/K03-Secrets-Management-Failures.html) recommends.


Vault integration fetches credentials at runtime through a secure API. This limits exposure to the moment of retrieval. Proxy routing is the strongest option for sandboxes. It injects credentials at the network layer so they never reach agent code.


Start by auditing how your current sandboxes receive secrets. If credentials live in environment variables, move them to a vault with runtime fetching or proxy injection. Check how your provider delivers secrets. Confirm whether credentials can be scoped to specific sandboxes or agents. Verify that credential access events are logged. Credential theft is a top-tier risk in agent security. The delivery mechanism matters as much as the rotation policy.


## **Governance and compliance for AI code execution**


Security controls protect the runtime. Governance protects the organization. Enterprise teams deploying agents that execute untrusted code need audit trails and review frameworks. These map controls to compliance requirements. Without governance, every agent execution is an untracked event with no accountability chain.


### **Log and trace every execution**


Every sandbox execution should produce a traceable record. Capture what code ran, who or what agent triggered it, what resources it accessed, and what output it produced. Every audit record needs to capture the event type, timestamp, environment ID, triggering identity, exit code, and resources accessed.


Log granularity matters. Sandbox creation and deletion events are the minimum. Production-grade audit trails include network requests, credential access events, filesystem changes, and execution duration. Forward logs off the source system in near real-time. Use a platform under separate administrative control. Give source-system administrators no write access to stored logs.


Confirm that you can export sandbox logs to your own SIEM system. Check that logs are retained long enough for incident investigation. Verify that credential access is logged separately from general activity. When an agent-generated script causes an incident, the audit trail reconstructs what happened and proves it to auditors.


### **Map sandbox security to compliance frameworks**


SOC 2 Type II and ISO 27001 usually form the enterprise baseline. HIPAA deployments also need a Business Associate Agreement (BAA). Verify whether the certification scope includes the sandbox compute layer or only the control plane. SOC 2 is an attestation bounded by its[system description](https://www.aicpa-cima.com/resources/download/get-description-criteria-for-your-organizations-soc-2-r-report) .


Controls outside that boundary fall outside the report. A SOC 2 report that covers the management dashboard but excludes the microVM fleet leaves the highest-risk surface unaudited. Request the full report and check the system description section.


ISO 27001 is bounded by its Information Security Management System scope and Statement of Applicability. Verify that customer workloads fall inside that scope. For HIPAA, a cloud provider handling protected health information is a[business associate](https://www.hhs.gov/hipaa/for-professionals/special-topics/health-information-technology/cloud-computing/index.html) with direct liability.


This applies even when it only stores encrypted data. Verify a BAA is available and covers data processed inside sandboxes plus data at rest. Build a review framework that maps each sandbox security control to its compliance requirement. That document becomes the artifact your security team signs off on before production deployment.


## **Deploying agents that run untrusted code**


A production breach can start when an agent-generated script escapes its sandbox. It reaches another tenant's data. It triggers an incident your existing monitoring never flagged. Every safeguard in this guide targets a failure that standard application security misses. They all assume a human reviewed the code first. Isolation contains the environment.


Runtime safeguards constrain what runs inside. Network and credential controls limit the blast radius. Governance gives you the accountability chain.


For teams building[coding agents](https://blaxel.ai/blog/llm-coding-benchmarks) , PR review agents, and data analysis agents, Blaxel provides microVM isolation. SOC 2 Type II and ISO 27001 certifications cover the compute layer. HIPAA compliance with a BAA is available. Secrets reach sandboxes through proxy routing, keeping credentials out of agent code.


[Agent Drive](https://blaxel.ai/agent-drive) , currently in private preview, shares context across agent sessions. Volumes handle guaranteed long-term data retention. Networking features include custom domains, domain filtering, and dedicated egress gateways in private preview. Blaxel is also a first-class sandbox provider in the OpenAI Agents SDK. Blaxel Sandboxes handle the execution layer beneath OpenAI's Codex harness.


Talk to the team at[blaxel.ai/contact](https://blaxel.ai/contact) or start building at[app.blaxel.ai](https://app.blaxel.ai/) .


Walk through the isolation architecture


Firecracker microVM per workload, SOC 2 Type II and ISO 27001 certifications, and HIPAA via BAA. No shared kernel between tenants.


[Book a demo](https://blaxel.ai/contact)


## **Frequently asked questions**


### **What does it mean to run untrusted LLM-generated code?**


Untrusted LLM-generated code is any program an AI model writes and executes at runtime without human review. No developer approved it before execution. Adversarial inputs, prompt injections, or model hallucinations can produce dangerous code. That code can probe the host, open network connections, or access data the agent shouldn't reach.


### **Why can't containers safely run AI-generated code?**


Containers share the host kernel with other workloads. A kernel exploit inside one container can reach neighboring tenants. For trusted, human-reviewed code, containers provide sufficient isolation. For untrusted LLM-generated code, the shared kernel is the problem. A compromised sandbox gets a path to the host.


### **What isolation model is most secure for LLM code execution?**


MicroVMs provide the strongest isolation for untrusted code execution. Each sandbox runs a dedicated kernel with hardware-enforced boundaries. That boundary prevents kernel exploits from reaching the host or neighboring tenants. gVisor intercepts system calls in user space without a full virtual machine. It offers a middle tier but doesn't match microVM isolation strength. Containers sit at the lowest isolation level, appropriate only for trusted code.


### **What runtime safeguards should complement sandbox isolation?**


CPU and memory limits prevent runaway code from crashing the environment. Execution-duration timeouts stop processes that run indefinitely. Egress allowlists restrict outbound network access. System call filtering blocks dangerous OS operations. Output validation treats sandbox results with the same suspicion as untrusted user input. Proxy-based credential delivery keeps API keys out of the agent's execution context. Together, these safeguards constrain what code can do even when the isolation boundary holds.
