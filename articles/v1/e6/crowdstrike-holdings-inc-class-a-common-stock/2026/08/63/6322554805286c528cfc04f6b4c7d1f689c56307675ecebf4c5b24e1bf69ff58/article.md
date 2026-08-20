---
schema_version: "1.0.0"
document_id: "6322554805286c528cfc04f6b4c7d1f689c56307675ecebf4c5b24e1bf69ff58"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/secure-agent-harness-execution-preventing-escape/"
published_at: null
first_seen_at: "2026-08-04T22:25:16.860722+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:5d44e8efd6fe41d289892c434f7748948c26a6032e83d9facbe3660723f7ee08"
---

# Secure Agent Harness Execution: Preventing Escape

At CrowdStrike, we conduct extensive red-team testing of agentic systems using diverse models, tools, and adversarial evaluation harnesses designed to probe for containment failures. To date, none of our offensive agents have escaped their intended sandbox boundaries. Based on our review of public materials regarding documented incidents involving agent sandbox escapes, our defense-in-depth approach is designed to contain these classes of behavior, and we continuously validate it against evolving agent capabilities.


Our approach reflects CrowdStrike’s security-first perspective: drawing on deep adversarial expertise to identify nuanced attack paths and applying controls across every layer of the agent stack. As AI capabilities evolve, we continue to test, validate, and strengthen these safeguards alongside the broader ecosystem.


We use agents extensively to explore and assess how LLM-powered systems can perform cybersecurity tasks such as vulnerability discovery, exploit development, post-exploitation steps, capture the flag, red-teaming, and adversary emulation. Running these experiments safely requires strong agent containment.


We use a secure-by-design, defense-in-depth approach built on seven independent control layers, each designed under the assumption that others may fail. No single control is trusted to contain the agents on its own. Our framework combines established isolation controls with enforcement tailored to the ways agents operate. The specific configuration used for each experiment depends on the agent, task, environment, access, and associated risks. For any given experiment, some layers may be inactive.


*A note:* When we talk about agent containment, we are referring to preventing actions that are not intended or appropriate for the agent on a given task. In adversary emulation and related tasks, there may be targets, vulnerabilities, breakouts, and exfiltrations that are intentionally within scope. The containment approaches outlined here do not affect such in-scope actions.


In this blog, we share our framework for agent containment, built to prevent agents from taking unintended actions. All capabilities discussed here are implemented and running.


## Agent Escape: Scope and Threat Model


This architecture applies to any harness in which AI agents autonomously execute commands or invoke tools, regardless of the workload, including code analysis, software builds, security testing, data processing, and report generation.


We use the term “escape” to include any of the following unauthorized agent behaviors:


Table 1. Examples of AI agent escape threats and their potential impact **Threat** **Example**


Data exfiltration Sending working data, results, or source code to an external host


Public disclosure Publishing packages, container images, pull requests, issues, or gists


Lateral movement SSH/SCP/rsync to other systems on or beyond the network


Host compromise Escaping the sandbox to operate on the underlying host


Credential theft Reading API keys or auth material out of the environment for reuse elsewhere


Destructive action Formatting disks, deleting the filesystem, shutting down infrastructure


Resource abuse Unbounded API spend, request floods, or compute consumption


The threat model assumes the agent is capable and persistent. It treats the AI agent the way a security engineer treats untrusted code: **Contain by construction, not by trust.** This applies whether the risky behavior originates from the model itself, from adversarial content the agent ingests through prompt injection, intent misalignment, or from a defect in the harness. It also assumes the agent may act through any available channel, including a shell, tool call, or network request, which is why each enforcement exists independently.


## Defense-in-Depth: Seven Layers


Preventing these forms of escape requires controls that operate independently across the different boundaries and action channels available to an agent. The architecture, therefore, uses seven layers of containment, enforcement, and resource control, each designed to limit a distinct path or consequence of unauthorized activity.


Layers 1-4 are **containment boundaries** , which are nested so an action must traverse outward through all of them to escape. Layers 5-6 are **channel gates** , which are peers, each policing a distinct action pathway. Layer 7 is **cross-cutting** , bounding volume at every boundary above. Each layer is independent: A failure or misconfiguration in one is caught by the others.


Figure 1. Seven layers of defense-in-depth for secure AI agent containment


### Layer 1: Infrastructure Isolation (Network)


Harness environments sit on an isolated VLAN or segmented network with restrictive firewall policy. Egress is **default-deny** ; only explicitly required destinations, such as the model API endpoint, are reachable, and outbound traffic is routed through controlled proxies that provide inspection and logging. Higher-risk workloads additionally run in separate cloud projects/accounts with their own restricted identities, environment-specific logging, and independent access controls. Even if every inner layer fails, an agent has no general route to the internet or to adjacent internal systems.


### Layer 2: Sandbox Isolation (Virtual Machine)


Each harness runs inside a dedicated virtual machine, never on operator workstations or shared hosts. Agents never perform host operations directly. The VM boundary ensures even a full compromise of the harness environment is confined to a disposable guest that can be snapshotted, inspected, and reverted. Where a workload interacts with anything production-like, a simulated environment is substituted for the real system.


### Layer 3: OS-Level Containment (Containers and Syscall Filtering)


Within the VM, agent workloads and any software being tested run inside containers such as Docker and Kubernetes, which separates untrusted or experimental code from the harness tooling itself, with workload-specific runtime, filesystem, and resource constraints. Kernel-level controls, including seccomp syscall filtering, AppArmor profiles, and unprivileged container execution, constrain what containerized processes can ask of the operating system, limiting the blast radius of any misbehaving workload. Where a specific workload requires relaxed settings, the exception is explicit, scoped to that container, escalated for human approval (see Layer 6c), and compensated for by Layers 1-2.


### Layer 4:` ward` : Process Capability Confinement


` ward` is our capability-based sandbox and tracing telemetry capture for AI agents. Its core principle is to confine the agent process to exactly the filesystem paths and network endpoints it needs, **enforced by the OS kernel rather than by anything the agent or its harness can influence.**


Where Layers 2-3 answer *“If the agent escapes its execution environment, what can it reach?”*` ward` answers a complementary question: *“What can this agent process touch on this machine at all across filesystem, network, credentials?”* It is not a substitute for the container/VM tier and is never deployed alone; the two operate together.


Table 2. How` ward` enforces process confinement **Mechanism** **Behavior**


Kernel-enforced native sandbox Landlock on Linux, Seatbelt on macOS. The agent runs as a sandboxed child process; an unsandboxed supervisor applies the sandbox *before* the agent’s code executes, then owns all enforcement and auditing for the session. Refusals happen at the VFS layer before a syscall completes.


No escape by construction Once the sandbox is applied, permissions cannot be widened for that process; not by the agent, not by the harness.


Network enforcement (` ward-proxy` ) All outbound traffic from the agent flows through a proxy in the supervisor: a CONNECT tunnel validated against a domain allowlist and a cloud-metadata deny list, plus DNS-rebinding protection (resolve once, connect only to the resolved IP).


Credential isolation A reverse-proxy mode injects credentials into requests at the proxy layer, so API keys, secrets, and authentication material **never enter the sandboxed process** . The harness is never aware of any auth method and cannot leak what it does not hold. Test environments hold no live secrets; where credentials are needed, they are test-only or canary credentials, revocable immediately.


Tamper-evident audit Every enforcement decision and network event is written to a chained, append-only log in the supervisor, which the sandboxed agent cannot modify, giving us a trustworthy trail even if we later suspect an agent behaved adversarially during a run.


Fail-secure by policy Configuration load failures are fatal rather than falling back to an empty or permissive sandbox. Codebase standards enforce zero` .unwrap()/.expect()` outside tests, specifically so a panic cannot silently drop enforcement.


### Layer 5:` ward-mcp` : Tool-Call Mediation


Shell commands are not an agent’s only route to consequential action. Agents also act through tools, including MCP servers, API gateways, and workflow endpoints.` ward-mcp` intercepts MCP JSON-RPC tool calls and enforces allow/deny policy **at the protocol layer** , providing an independent check above the kernel layer and alongside the command layer. It closes the channel that command filtering cannot see.


Enforcement covers:


- **Allowlisted operations** : Only named tools/methods are invocable; everything else is denied by default.
- **Parameter validation** : Arguments are inspected against policy, so an allowed tool cannot be steered to a disallowed destination or scope.
- **Destination and volume constraints** : Permitted API endpoints, request rate, and payload/result volume are bounded.


Tool-call denials and sensitive-but-permitted calls are written to the same tamper-evident audit trail as kernel-layer decisions. Denial reasons are returned to the agent so it can find a compliant path rather than repeating attempts to retry the same action.


### Layer 6:` Baywatch` : Command Line Action Control


` Baywatch` is our harness-agnostic, per-command policy engine, deployed identically across all pipelines. It is enabled by default; disabling it requires an explicit opt-out flag and is not recommended. Every command receives one of four decisions: **allow, allow-and-log, escalate to a human, or deny.** These are enforced through four sub-layers.


**6a. Hard Block: Pre-Execution Hook**


A harness-level hook intercepts every shell command before it executes. The hook operates outside the agent’s control: It fires even when the agent runs in permission-skipping mode, and the agent cannot modify, disable, or route around it. Prohibited commands are rejected before execution, and the denial reason is returned to the agent.


**6b. Soft Guidance: Policy in the System Prompt**


Every agent receives the full action-control policy in its system prompt, including rules of engagement that define the evaluation scope. This is an efficiency measure, not a security boundary: Agents that know the rules don’t waste turns attempting blocked actions, but compliance is never assumed. Layer 6a enforces regardless.


**6c. Human-in-the-Loop (HITL) Escalation**


Approval for sensitive operations is a key safeguard to stopping some of the recent documented agent escapes.


Between “always allowed” and “always denied” is a class of operations that are legitimate for some runs but consequential enough that no automated policy should decide them alone. Examples include installing new system packages, modifying harness or policy configuration, relaxing a container security option for a specific workload, requesting a raise to a resource or spend ceiling, or deleting substantial portions of the working tree.


When an agent attempts one of these operations, execution pauses, and the command is presented to a designated human operator with its full context, including the agent, command, category, and rationale. The operator may approve once, approve for the remainder of the run, or deny with feedback. The denial reason is returned to the agent, steering it toward a compliant alternative rather than repeated attempts. Escalations **time out to deny** if no operator responds (fail-closed), and every decision is recorded with the approver’s identity.


**6d. Audit Log: Complete Decision Trail**


Every blocked command, every HITL escalation and resulting human decision including the approver’s identity and timestamp, and every sensitive-but-allowed operation, such as local container or Git action, is written to an audit log with a timestamp, category, and reason. The audit log is incorporated into each run’s final report and summary, giving reviewers a complete record of what agents attempted, what was denied, what a human approved, and why.


### Layer 7: Resource and Economic Containment


The six layers above bound *what* an agent may do. Layer 7 bounds *how much* it may do. It is cross-cutting rather than nested, with ceilings enforced at every boundary above.


Ceilings are **fail-closed** : When a limit is reached, the agent’s action is denied, the event is audited, and any request to raise a ceiling becomes a HITL escalation under Layer 6c, never an automatic grant. This also serves as a behavioral signal: An agent repeatedly hitting a ceiling is flagged for review rather than quietly throttled.


Table 3. How resource limits bound agent activity **Control** **Purpose**


Quotas on tool and API invocations Prevents an authorized channel being used at abusive volume


Rate limits Bounds request floods against allowlisted destinations


Concurrency limits Caps parallel agent activity and fan-out


Wall-clock time limits per run Guarantees termination; no indefinitely running agent


Spend ceilings Bounds model and cloud cost exposure per run


Compute/storage resource caps Enforced at the container and cloud-project level


## Policy Baseline: What Is Blocked, Escalated, and Allowed


The seven layers define the boundaries within which an agent operates. The policy baseline below translates those boundaries into consistent decisions about which actions are blocked, escalated for human approval, or allowed.


The policy distinguishes between the agent’s legitimate local workspace and any action that reaches outside it. The baseline below applies to every harness; individual harnesses may tighten it further, but may not silently relax it.


Table 4. High-risk actions blocked before execution **Blocked (denied before execution)**


**Category** **Examples**


Container publishing` docker push` ,` docker login` , tagging to remote registries


Git remote operations` git push` , adding or modifying remotes


Public GitHub actions Creating PRs, issues, releases, gists; API write operations


Network exfiltration` curl` /` wget` /` nc` /` socat` to any non-localhost destination


Remote access` ssh` ,` scp` ,` rsync` to remote hosts


Package publishing` npm publish` ,` twine upload` ,` cargo publish` ,` gem push`


Messaging` sendmail` ,` mail` ,` mutt` , Slack CLI


Destructive operations` rm` on filesystem root,` mkfs` ,` dd` to raw devices, shutdown/reboot


Non-allowlisted tool calls Any MCP method or API destination not explicitly permitted


Cloud metadata access Requests to instance metadata endpoints (deny-listed at` ward-proxy` )


Table 5. Sensitive agent actions requiring human approval **Escalated (paused pending human approval; deny on timeout)**


**Category** **Examples**


System modification Installing/removing system packages, changing system services


Privilege elevation` sudo` beyond a pre-approved allowlist, changing file ownership/permissions systemwide


Container security exceptions Running privileged containers, relaxing seccomp/AppArmor profiles, host mounts outside the workspace


Sandbox scope changes Requests to widen` ward` path or endpoint policy for a run


Bulk deletion Removing large directory trees within the workspace


Ceiling increases Raising quota, rate, time, or spend limits mid-run


Policy/harness configuration Any modification to hook scripts, policy files, or harness settings


Table 6. Permitted agent actions within enforced boundaries **Allowed (with sensitive operations logged)**


**Category** **Examples**


Local containers` docker run` /` exec` /` build` /` stop` against local targets (logged)


Local Git` add` ,` commit` ,` diff` ,` log` ,` status` (logged)


Local network` curl` /` wget` to localhost and container bridge addresses only (logged)


Allowlisted egress Model API endpoints and named destinations, via proxy, credentials injected (logged)


Allowlisted tool calls Named MCP methods with validated parameters, within quota (logged)


File operations Reads/writes confined to` ward` -permitted working directories


Build and analysis tools Compilers, interpreters, linters, debuggers, and analysis utilities


## Residual Risk and Known Limitations


No single layer is claimed to be perfect, which is precisely why the layers are independent:


- **Command filtering is pattern-based.** Layer 6a matches known-dangerous command patterns. A sufficiently novel invocation could evade a pattern, which is why exfiltration is also impossible at the network layer (Layer 1), egress is independently constrained at` ward-proxy` (Layer 4), and consequences are confined by Layers 2-3.
- ` ward` **is not kernel-exploit-proof.**` ward` controls what an agent process can touch: filesystem, network, credentials. It is not a machine-boundary control. A kernel exploit inside the sandbox is a different threat, addressed by the container and VM tiers (Layers 2-3), which is why` ward` is never deployed alone.
- **Channel gates are per-channel by design.**` ward-mcp` polices tool calls; Baywatch polices shell commands. Neither backstops the other, and an action channel we have not anticipated would bypass both. This is why enforcement does not rest on the gates: Every action, regardless of channel, is still bounded by` ward` ’s kernel-enforced path and endpoint policy, container and VM isolation, and default-deny egress. Adding a new agent action channel requires a corresponding gate before that harness is approved.
- **Prompt guidance is advisory.** Layer 6b improves efficiency but is never relied upon for enforcement.
- **Human approval is a judgment point, not a formality.** HITL escalation concentrates risk decisions on a person; its value depends on operators reviewing context rather than approving reflexively. Approver-identity logging and per-run (not permanent) approvals are designed to keep that judgment engaged.
- **Ceilings bound volume, not intent.** Layer 7 prevents runaway consumption and abusive volume; it does not distinguish a legitimate high-volume run from a malicious one. Its security value is as a bound and a signal, not a classifier.
- **The sandbox may intentionally contain sensitive material.** Depending on the workload, the environment may hold proprietary code, test data, or security research artifacts. Live production secrets and long-lived credentials are excluded by policy (auth material is injected at the proxy and never enters the agent process) but the design goal is not to prevent all sensitive material from existing inside the environment. It is to guarantee that such material cannot leave, and that agents cannot act beyond it.


**The compound effect is the core security argument:** Every agent action must clear the policy gate for its channel and then survive kernel-enforced process confinement, OS-level containment, VM isolation, and network egress control. Each is an independent mechanism with an independent failure mode, and each produces an independent audit record. Because the four containment layers are channel-agnostic, defeating a single channel gate yields no path out.


## Summary for Decision-Makers


As agents advance in capabilities, safety mechanisms, robustness, and testing must evolve alongside them. For decision-makers, this boils down to what risk is acceptable vs. trust of agents. At CrowdStrike, we are constantly evolving and reinforcing our agent sandboxes while developing the cutting edge of offensive and defensive agentic capabilities. Below we provide a checklist for Frontier Labs and other entities utilizing offense agents to reduce risk of escape.


Table 7. Checklist for secure agent containment **Property** **Status**


Enabled by default on every harness ✔ Yes; opt-out requires explicit flag


Enforcement independent of agent cooperation ✔ Yes; kernel-level, proxy-level, and harness-level hooks, all outside agent control


Network egress controlled ✔ Yes; VLAN, default-deny firewall, inspecting proxies, metadata deny-list


Host protected from agents ✔ Yes; dedicated VM boundary


Process reach constrained to declared paths/endpoints ✔ Yes;` ward` , kernel-enforced, non-wideable once applied


Tool and API calls policed independently of the shell ✔ Yes;` ward-mcp` protocol-layer allow/deny and parameter validation


Credentials withheld from the agent process ✔ Yes; injected at proxy; test-only/revocable credentials


Volume-, time-, and cost-bounded ✔ Yes; quotas, rate/concurrency/time limits, spend ceilings, fail-closed


Full audit trail ✔ Yes; chained, append-only, agent-immutable; included in final report


Human-in-the-loop for sensitive operations ✔ Yes; explicit operator approval, fail-closed on timeout


Independently testable ✔ Yes; automated regression suite


Uniform across all harnesses ✔ Yes; same baseline policy and control stack; tighten permitted, relax not permitted


Secure agent execution cannot depend on model behavior, prompt instructions, or any single control. It requires independent layers that constrain what an agent can access, which actions it can take, where data can move, and how much activity it can generate, while preserving human judgment for consequential decisions.


By treating agents as untrusted code and containing them by construction, this architecture provides the foundation for CrowdStrike to safely push the boundaries of autonomous AI in cybersecurity. As agent capabilities advance, CrowdStrike is not only applying these systems to high-risk security workflows but defining the engineering discipline required to deploy them securely at scale.


#### Additional Resources


- *Learn more about AIDR and CrowdStrike's vision for securing the agentic enterprise in this video on demand:[AIDR: Defining the Next Era of Cybersecurity](https://www.crowdstrike.com/en-us/resources/crowdcasts/aidr-defining-the-next-era-of-cybersecurity/)*
- *Visit the[Falcon AI Detection and Response](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/) product page to learn how AIDR can discover, govern, and secure enterprise-developed and third-party agents.*
- *[Join us](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) at Fal.Con 2026 as we bring together cyber leaders from across the industry to help secure the AI revolution.*
