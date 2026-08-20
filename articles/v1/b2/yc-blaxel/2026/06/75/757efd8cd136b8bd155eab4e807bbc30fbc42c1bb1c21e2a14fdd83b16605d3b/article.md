---
schema_version: "1.0.0"
document_id: "757efd8cd136b8bd155eab4e807bbc30fbc42c1bb1c21e2a14fdd83b16605d3b"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/sandbox-egress-control-outbound-allow-listing"
published_at: "2026-06-30T15:21:23+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:ca8db35659cae133b7866075c2f230c58668d22a7f706fb931ebe8ac657e0e86"
---

# How to add outbound allow-listing and egress control to an agent sandbox

A coding agent finishes reviewing a pull request, then POSTs its results to an internal webhook. The sandbox running that agent also has unrestricted outbound access. The same execution environment that talks to your webhook can reach any endpoint on the internet. Through a prompt injection or malicious generated code, it can send your data somewhere you never approved.


Enterprise security teams won't sign off on agent infrastructure that can't answer one question. What outbound connections can this sandbox make? When agent infrastructure reaches the security review stage of a procurement cycle, an undefined egress posture stalls the deal. Network controls move from a "nice to have" line item to a blocking requirement blocking the contract.


Production egress control starts with a default-deny posture and routing rules that match compliance requirements without breaking agent performance.


## **Why unrestricted sandbox egress creates enterprise risk**


[Agent sandboxes](https://blaxel.ai/blog/ai-sandbox) execute untrusted or AI-generated code. That code makes outbound HTTP calls to APIs and package registries as part of normal operation. Without controls on those calls, an attacker who manipulates the agent's instructions can redirect them. Research on indirect prompt injection shows that retrieved prompts[can function as arbitrary code execution vectors](https://export.arxiv.org/pdf/2302.12173v2.pdf) . An adversary can control how and whether external APIs get called.


The exfiltration path is short and well-documented. Adversarial content reaches the model. The model interprets it as an instruction. The generated code makes an outbound call, and data leaves the perimeter. One security paper documents how malicious text can hijack an agent during a benign task.


The hijacked agent[executes arbitrary code](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_chang.pdf) and exfiltrates data from the user's environment. Experiments on computer-use agents measured[end-to-end exfiltration success rates up to 85%](https://arxiv.org/pdf/2603.11862v1) , consistent across multiple programming languages. A single unrestricted sandbox becomes the exit point for all of it.


For the VP or CTO, the same gap shows up in procurement. SOC 2 Type II and enterprise vendor questionnaires ask how outbound traffic is controlled. The Cloud Security Alliance's Consensus Assessments Initiative Questionnaire[documents and assesses what security controls exist](https://ea.cloudsecurityalliance.org/display.php?id=data_sec6081) in cloud services.


Enterprise security reviews examine egress filtering and default-deny outbound policies as exfiltration safeguards. "We don't restrict it" kills the deal. Egress controls determine whether agent infrastructure passes the security review between a proof of concept and a signed contract. The controls that defend against prompt-injection exfiltration are the same ones auditors look for. The security spend and the compliance spend are one investment.


## **Egress control patterns for agent sandboxes**


Egress policy answers two questions. Which destinations can the sandbox reach? Which source identity should downstream services trust? Credentialed traffic adds another requirement: authenticated requests need a path that does not expose secrets to sandbox code. Most production deployments combine multiple patterns, because each pattern leaves a gap the others cover.


### **Restrict outbound traffic with domain-level allow-lists**


Domain-level allow-listing works at the network layer. The sandbox can only resolve and route to pre-approved domains, and everything else gets dropped. Most implementations inspect the Server Name Indication (SNI) field in the TLS handshake. SNI stays unencrypted in the traffic flow and indicates the destination hostname. Some implementations filter on the resolved IP addresses of the allowed domains instead.


This pattern fits agents that call a known, stable set of APIs. A PR review agent that talks to GitHub, an internal webhook, and a package registry has a predictable destination list. Define it once. Define an allow-list that names those domains and drops the rest by default.


The limitation shows up when agents need to reach changing or user-specified URLs. A research agent that fetches arbitrary pages can't operate behind a fixed allow-list without breaking.


Traffic using TLS 1.3 Encrypted Client Hello (ECH) also creates a blind spot. ECH hides the hostname, so SNI-based filtering can't see the destination. For agents with a stable destination set, domain filtering gives you default-deny posture. It maps directly to the network restriction controls auditors require.


### **Assign static IPs for firewall integration**


Static egress IPs make all outbound traffic from your sandboxes exit through a dedicated, publicly routable IP address. A network address translation (NAT) gateway performs source address translation. It replaces each sandbox's private IP with the fixed one. The customer's firewall then allow-lists that fixed IP.


Third-party APIs that enforce IP-based access control can whitelist it too. Predictable source IPs let external services, partners, and on-premises firewalls add your addresses to their authorized lists.


Use this pattern when enterprise customers need to integrate sandbox traffic with existing perimeter rules. It also applies when a third-party vendor requires IP-based access. Financial data providers and other regulated APIs often gate access on a fixed source IP. Give your customer's network team a fixed IP to add to their allow-list instead of an opaque range.


Static IPs establish source identity. Domain filtering controls destinations, and proxy policy can inspect authenticated requests. A downstream firewall can confirm that a request originated from a known IP. It can't inspect the domain, the URL path, or the credentials. Pair static egress with domain filtering for full coverage, since the two patterns control different ends of the connection.


### **Route through a managed proxy with credential injection**


A managed proxy routes outbound calls through a layer that injects API keys and tokens at the network boundary. The sandbox code never holds the credential. The proxy reads the request, adds the authentication header server-side, and forwards it upstream. For HTTPS, the proxy terminates TLS, inspects the request, injects the header, then re-encrypts before sending.


This pattern fits agents calling authenticated APIs where you need to prevent credential leakage. Environment variables are a poor fit here. OWASP notes that environment variables are[generally accessible to all processes](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) and may end up in logs or system dumps.


Any secret in the environment is one crash dump or stdout leak from exposure when the agent runs untrusted code. NIST SP 800-190 recommends that secrets be[stored outside of images](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf) and provided at runtime.


Perpetual sandbox platforms like Blaxel offer[managed proxy routing with secrets injection](https://docs.blaxel.ai/Sandboxes/Proxy-secrets-injection) so credentials never touch agent code. The proxy injects headers and secrets server-side, after the request leaves the sandbox. The executing code has no access to the secret value. This eliminates the credential exposure risk that comes with environment variable injection. Route credentialed calls through the proxy and keep API keys out of the execution environment.


## **How to choose the right egress model for your workloads**


The right combination depends on two factors: how predictable your agent's outbound communication is, and what compliance standard applies. Audit traffic before writing rules, then align the results with the compliance framework your buyers require.


### **Map agent communication patterns before writing rules**


Start by auditing outbound calls during staging. Catalog every external domain and port the agent contacts across realistic workloads, including paths beyond the happy path. An agent that calls three APIs in development often reaches dozens of registries and resolvers under real tasks.


Distinguish between categories of calls. Agent-initiated calls are the API requests triggered by tool use, like a GitHub fetch or a webhook POST. System-level calls are the package installs, DNS resolutions, and health checks the sandbox makes to function. Rules that block system-level calls break the sandbox itself, so your allow-list has to account for both. PR review agents like the ones Delty and Jazzberry run make both kinds during a review. Catching only agent-initiated calls leaves system traffic to fail in production.


Run staging traffic long enough to capture recurring workloads. Group every connection by destination, then define allow-list tiers from the result. Separate stable system-level destinations from agent-specific ones. This tells you which rules rarely change and which need frequent updates. The audit output becomes the input to your allow-list, and skipping it means writing rules from guesses.


### **Align egress scope with your compliance framework**


Match your egress controls to the standard your buyers require, because the framework dictates which controls satisfy the review. SOC 2 generally requires organizations to demonstrate security monitoring and network controls. Frameworks like the Cloud Security Alliance (CSA) questionnaire address network restrictions and logging or monitoring as separate areas.


HIPAA environments raise the bar. A deployment handling protected health information requires safeguards appropriate to its HIPAA risk profile. Business Associate Agreement (BAA) obligations may add contractual controls. The controls address different leak paths, so HIPAA workloads need both types. NIST SP 800-53 names the underlying controls directly: SC-7(10) covers[preventing exfiltration](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf) , and SI-4(4) covers monitoring inbound and outbound traffic.


Enterprise customers running their own review will ask for a network architecture diagram showing every egress path. Draw that diagram before the review starts, while there is still time to resolve gaps. A diagram that shows default-deny egress and named allow-list destinations answers most of the questionnaire before anyone asks.


## **Operational tradeoffs of sandbox egress controls**


Every egress control adds a layer between the sandbox and the public internet. The operational cost shows up in latency and in the work required to maintain and debug rules.


### **Account for latency when routing through proxies**


Proxy-based routing adds a network hop to every outbound call. For an agent making many calls per task, the cumulative cost adds up fast. Official Istio benchmarks measured proxy-added latency of[3 milliseconds at the median and 20 milliseconds at P90](https://istio.io/latest/blog/2019/performance-best-practices/) under higher concurrency.


Cloud-routed proxies produce mixed results. Some add no measurable latency; others increase average download times compared to direct connections. That overhead may be acceptable for isolated calls but material for sequential tool chains.


Measure round-trip time through your proxy path during staging and compare it with direct egress. If the difference exceeds the threshold your SLA allows, consider domain filtering as a lower-overhead control for non-credentialed endpoints. Domain filtering operates at the network layer and adds far less per-call overhead than a full proxy that terminates TLS.


The math matters when you present it to your team. Proxy overhead compounds across sequential tool calls, and long tool chains can make a[coding agent](https://blaxel.ai/blog/llm-coding-benchmarks) slower. When users expect fast feedback, that delay separates a responsive tool from a sluggish one. Route only calls that need credential injection through the proxy. Let direct egress with domain filtering handle the rest.


### **Plan for allow-list maintenance as agent capabilities grow**


Static allow-lists need an update every time an agent gains a new integration. When the list lives in infrastructure config like Terraform or CloudFormation, each change requires a deploy. An agent that ships a new integration weekly turns the allow-list into a recurring deploy dependency. The friction compounds as the integration count grows.


Split your allow-list into tiers to reduce that friction. Keep a base tier for system-level traffic that rarely changes. Add a separate application-managed tier for agent-specific integrations. An application owner can update it without an infrastructure deploy. The people who add integrations manage the destinations they add. The stable system rules stay locked down.


Watch for the most common failure: an integration ships but its domain never gets added to the allow-list. The feature works in staging where restrictions are loose, then breaks in production where they're enforced. Add allow-list verification to your integration deployment checklist so a new destination can't ship without its corresponding rule. The check costs one checklist item and saves a production incident.


## **How to build outbound controls into your agent sandbox architecture**


Start with an audit of your agent's outbound traffic. Layer the patterns that match your destinations and compliance framework. For stable destination sets, domain filtering gives the default-deny baseline. If downstream firewalls or vendors require fixed source addresses, add static egress IPs. Credentialed calls belong behind proxy injection so secrets stay out of the execution environment. The combination depends on your workload, but the audit always comes first.


[Agent sandboxes should have defined egress controls](https://blaxel.ai/blog/ai-runtime-security) to meet common enterprise security expectations. Undefined egress controls usually appear during procurement. By then, development has succeeded in an unrestricted environment and your team has committed engineering time to the integration. The cost of fixing it then is a stalled deal and a scramble to retrofit network controls under deadline.


Blaxel's[perpetual sandbox platform](https://blaxel.ai/sandbox) is organized around compute, storage, and networking primitives. For egress-heavy deployments, the relevant networking features are domain filtering, proxy secrets injection, and custom domains.


[Dedicated egress gateways](https://docs.blaxel.ai/Infrastructure/Dedicated-egress-gateways) , currently in private preview, provide static outbound IPs. These run as part of the platform instead of infrastructure your team builds. That matters most for[coding agents](https://blaxel.ai/blog/humaneval-benchmark) and PR review agents executing untrusted code in production.


Talk to the Blaxel team about your egress requirements at[blaxel.ai/contact](https://blaxel.ai/contact) , or create a sandbox with networking controls at[app.blaxel.ai](https://app.blaxel.ai/) .


Talk to us about egress controls


Domain filtering, proxy secrets injection, and dedicated egress gateways for agent sandboxes running untrusted code.


[Get in touch](https://blaxel.ai/contact)


## **Frequently asked questions**


**What is egress control in the context of agent sandboxes?**


Egress control restricts which external destinations a sandbox can reach when making outbound network calls. For sandboxes executing untrusted code, egress controls prevent data exfiltration. They limit outbound traffic to pre-approved domains or managed proxy routes, with static egress IPs where downstream firewalls require them. Enterprise security reviews commonly check for egress filtering and default-deny traffic policies.


**Do egress controls add latency to agent tool calls?**


Domain-level filtering adds minimal overhead because it operates at the DNS and network layer. Proxy-based routing can add latency to outbound calls, depending on the provider's infrastructure and concurrency. For agents making many sequential tool calls, this cumulative latency matters. Measure the impact during staging before committing to a proxy-only approach for all traffic.


**Can agents function with a fully restricted allow-list?**


Agents that call a known, stable set of APIs work well with static allow-lists. Agents that reach user-specified URLs or runtime-discovered endpoints need more flexible controls. Proxy-based routing with per-request authorization handles these cases. Map the agent's outbound communication pattern first, then choose the control model that matches the destinations it actually contacts.
