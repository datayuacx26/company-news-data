---
schema_version: "1.0.0"
document_id: "7b71a505f51859156db2b7d9ab5eee120b41fbae66c8ba175945088e6621acd6"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/secure-ai-agents-defense-in-depth"
published_at: null
first_seen_at: "2026-08-15T03:39:52.671267+00:00"
fetched_at: "2026-08-15T03:39:55.012890+00:00"
content_hash: "sha256:cc884535a8d7783269793cc377405440317f5d153b2a2d77148d7d7db26ccd0d"
---

# How to secure AI agents with defense in depth

## TL;DR


This post is the written companion to Konstantino Sparakis’ (Doppler Sales Engineer) talk on[Securing secrets in AI-driven systems](https://www.youtube.com/watch?v=NKz-VotOS34&t=22s)


. Any permission an agent has is one it will eventually use, so a single prompt injection can reach real credentials and systems. No one control stops that. The fix is defense-in-depth, with secrets management as the backstop: short-lived, centrally governed credentials cap the damage when another layer fails.


## Why agent permissions are a security problem


AI agents expand what software can do and what an attacker can reach. A layered security model, backed by strong[secrets management](https://www.doppler.com/)


, can shrink the blast radius.


When GitHub Copilot first shipped, developers noticed it could produce strings that looked a lot like API keys. Later research made that harder to wave off, showing coding assistants can reproduce credentials that were already exposed in public code. The model had inherited a security failure that existed before it.


That gets more urgent as AI moves from suggesting code to taking action. An agent can read repositories, investigate issues, call APIs, use MCP tools, change files, and touch production systems. Every one of those capabilities is useful, and every one also widens the set of actions an attacker might be able to influence.


One of our engineers at Doppler, Mike, framed the risk in a way that stuck with me: as an agent runs longer and its harness gets more capable, the odds that it eventually reads every line it can access and tries every action available to it climb toward 100%.


That doesn't mean every agent will turn malicious. It means you have to treat the permissions an agent has as permissions it will reach. If an agent can read a credential or send a network request, both belong in your threat model, even when nobody meant for them to be used together.


## Why agentic AI changes the security model


Traditional software mostly follows code paths that developers define ahead of time. Agents have more room to decide. They read natural-language instructions and pick tools, often while handling information that comes from sources you don't control.


Prompt injection exploits that ambiguity. An attacker plants an instruction in content the agent will read, like a GitHub issue, a pull request, a webpage, a document, or a tool response. Since the model processes both the legitimate context and the attacker's text as language, the malicious instruction can redirect what the agent does.


How bad it gets depends on what the agent can reach. A successful injection could expose source code and secrets, modify infrastructure, or use an authenticated tool on the attacker's behalf. Research into AI-powered GitHub Actions has shown how untrusted issue and pull-request content can open paths to workflow secrets when permissions and trust boundaries are drawn too wide.


[Model Context Protocol (MCP)](https://docs.doppler.com/docs/mcp)


adds another trust boundary. It gives models a standard way to connect to external tools and data, but an MCP server can also supply descriptions and outputs that steer the model. Security researchers have documented tool-poisoning attacks, where a tool hides instructions inside itself, and rug pulls, where a trusted tool later changes its behavior. Supply-chain risk grows when teams run the latest version of a community server with no review or central approval.


## Use defense in depth for AI agents


No single control reliably stops prompt injection or every new MCP attack. Defense in depth adds barriers so one failure is less likely to become a full compromise. NIST describes the idea as applying countermeasures across multiple layers to meet your security objectives.


For an AI development environment, those layers can include:


### 1. Restrict tools and permissions


Start with[least privilege](https://www.doppler.com/solutions/use-cases/least-privilege-access)


. Give the agent only the access the current task requires. Keep read access separate from write access, and require human approval for consequential operations. A tool the agent can't call sits outside the immediate blast radius.


### 2. Treat external content as untrusted


Mark the boundary between user instructions and retrieved content. Structured prompts and input isolation can help a model tell the task apart from the material it's analyzing, though they're guardrails rather than guarantees. Keep a human in the loop when an agent runs into instructions buried in external content.


### 3. Inspect prompts and tool calls


Add policy checks before sensitive actions run. An inline control can evaluate agent activity for signs of prompt injection or data exfiltration. Datadog AI Guard, for example, inspects and governs agent behavior in the critical path. Controls like this add a decision point between a compromised prompt and a real-world action.


### 4. Isolate the runtime


Run agents somewhere separate from the developer's everyday workstation. Containers give you a useful boundary, but highly capable or untrusted workloads may need stronger sandboxing. Tools such as gVisor add isolation between the container and the host kernel, which reduces the host attack surface while keeping a container-based workflow.


### 5. Control network, credential, and MCP access


Put gateways between the agent and the systems it can reach. A network proxy can restrict destinations. A credential proxy can attach credentials only to approved requests, so raw values never have to enter the model's context. An MCP gateway can centralize approved servers and their access policies. Docker's MCP Gateway is one example of this emerging control layer.


### 6. Preserve an audit trail


Record agent activity somewhere your security team can query it. Logging won't stop the first malicious action, though it supports investigation and anomaly detection, and it helps teams spot permissions that agents rarely use or never needed.


## Reduce what a leaked credential is worth


A layered setup helps, but it asks a lot of you operationally. Every layer needs configuration, ownership, and upkeep. New attack techniques keep showing up, and detection controls will always have gaps.


Secrets management gives you a backstop, because credentials often decide what an attacker can do once another defense fails. Verizon's 2025 Data Breach Investigations Report found that credential abuse was the leading initial access vector, showing up in 22% of the breaches it reviewed. That's why credential exposure deserves focused attention in an agentic environment.


The aim is to make a leaked secret less useful. Centralize credentials so teams can govern them the same way everywhere. Deliver them to approved workloads without scattering copies across repositories and local files.[Rotate them automatically](https://www.doppler.com/platform/secrets-rotation)


. Where you can, swap long-lived credentials for dynamic, short-lived access that expires on its own.


If a prompt injection exposes a static cloud key that stays valid for a year, an attacker has time to find it and reuse it. If the agent only ever holds a narrowly scoped credential with a short time to live, the window and the impact both shrink. Expiration works alongside prevention to help contain an incident.


## The five pillars of secrets management


At Doppler, we use[five pillars](https://www.doppler.com/blog/5-pillars-of-secrets-management)


to gauge whether a team's secrets setup is ready to make short-lived, governed credentials the backstop this approach relies on.


1. **Centralized secrets storage:** Pull secrets into one controlled system instead of managing disconnected copies across developer machines and infrastructure.
2. **Governance & access control:** Define who and what can access each secret, who can change it, and what approvals are required.
3. **Orchestration and secure delivery:** Deliver secrets to the applications and infrastructure that need them without anyone copying values by hand.
4. **Automation of secret lifecycles & short-lived dynamic secrets:** Automate credential replacement, and adopt[dynamic secrets](https://docs.doppler.com/docs/dynamic-secrets)


where the downstream service supports them. Doppler dynamic secrets are time-bound leases. For[AWS IAM](https://docs.doppler.com/docs/aws-iam)


, teams can issue credentials with a mandatory TTL instead of handing out a shared, long-lived key.
5. **Visibility and auditability:** Capture access and change activity, then forward the relevant events to monitoring and SIEM systems so security teams can catch unusual behavior and dig into incidents.


## What this looks like with Doppler


Doppler gives teams one place to organize secrets by project and environment. Teams can reference shared values without spawning unmanaged copies, and sync secrets into the systems that consume them. Automated rotation takes the manual work out of replacing long-lived credentials, and dynamic secrets can mint fresh, temporary access when an engineer or workload asks for it.


Take AWS access in an[AI-assisted development workflow](https://www.doppler.com/agents)


. Instead of leaving a persistent access key in a local environment file where an agent can read it, Doppler can issue a time-bound AWS credential with an appropriate IAM policy. The agent gets what it needs for the task, and the credential expires when its lease ends. Doppler activity can also flow into an observability or SIEM workflow, including Datadog, so unexpected access patterns are easier to run down.


This fits alongside the rest of the defense-in-depth stack. Tool restrictions limit what the agent can request. Runtime isolation protects the host. Proxies and gateways govern outbound actions. Centralized, short-lived credentials cap what's still possible when another boundary fails.


## AI adoption keeps going, and security has to keep up with it


Development teams aren't going to give up AI agents, and competitors won't wait for the security model to get perfect first. The workable path is to take the productivity gains while planning for agents that will run into malicious input and, now and then, make an unsafe call.


Start with least privilege and solid isolation. Add control points around tools and network access, and keep the evidence you'll need to investigate. Most of all, cut down the number of long-lived credentials an agent can reach and the damage any single credential can do.


Agent security is still taking shape. The principle under it is an old one: assume a layer can fail, and make sure the next layer is ready.


Give your agents access that expires. Doppler puts your secrets in one governed place and hands agents short-lived, scoped credentials instead of static keys that outlive the task.[See Doppler for AI agents](https://www.doppler.com/agents)


or[start free](https://www.doppler.com/)


.
