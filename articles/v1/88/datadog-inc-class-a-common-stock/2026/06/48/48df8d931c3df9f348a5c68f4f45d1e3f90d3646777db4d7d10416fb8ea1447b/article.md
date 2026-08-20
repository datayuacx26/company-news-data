---
schema_version: "1.0.0"
document_id: "48df8d931c3df9f348a5c68f4f45d1e3f90d3646777db4d7d10416fb8ea1447b"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/datadog-security/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:dda5e3731fa2faa16e2654e4dd39b03fd9b295474b54ce4d00fd3f7a1e032d19"
---

# Securing the AI era: Outpace AI-powered attacks with unified security and observability

Tim Knudsen


Security teams are dealing with a fundamentally different operating environment than they were a few years ago. AI-assisted development is rapidly pushing more code and infrastructure into production, and according to Datadog’s 2026 State of DevSecOps report,[40% of running services have an exploitable vulnerability](https://www.datadoghq.com/state-of-devsecops/#1) . Meanwhile, AI is giving attackers new ways to automate reconnaissance and accelerate exploits, which has collapsed the window between vulnerability disclosure and active exploitation from months to minutes.


Attacks are only going to increase in volume and speed. But defenders are still overwhelmed by alerts and disconnected tools, manually reconstructing context during active incidents, and chasing ownership across services. The teams that stay ahead won’t close that gap with more tools or signals—what separates useful data from noise is knowing what is actually happening in your production environments. When you have a clear picture of your attack surface, you can identify what’s truly exploitable and focus on the threats that can actually harm your business. And when a threat surfaces, you can detect, investigate, and contain it faster, with a unified system that makes ownership clear and keeps pace with AI.


We built the Datadog platform to give security and engineering teams the real-time visibility and context they need toprevent exposure ,detect and investigate threats faster , and contain attacks before they cause harm.


## Prevent exposure in dynamic environments


Periodic scans and manual remediation efforts aren’t enough when new services, APIs, and AI agents continuously change your environment’s risk profile. And with vulnerabilities accumulating faster than teams can triage them, knowing which ones matter is the difference between staying ahead of attackers and falling behind. Datadog researchers found that when you apply runtime context,[97% of critical vulnerabilities can be deprioritized](https://www.datadoghq.com/state-of-application-security/#1) because you can see which ones are actually reachable in production.


Today at DASH, we announced the[Datadog Runtime Prioritization Engine](https://www.datadoghq.com/product-preview/runtime-prioritization-engine/) to give teams real-time visibility into runtime behavior and execution context. The engine uses live APM traces, logs, and observed service dependencies to assess whether a vulnerability is truly business critical. By understanding whether an affected resource is actively running, connected to a[crown-jewel application](https://docs.datadoghq.com/security/cloud_security_management/triage_and_prioritize/runtime_prioritization_engine/#crown-jewels) , and exposed in production, teams can focus remediation efforts on the risks that matter most. The engine also identifies the resource owner based on live production data, helping organizations accelerate response and reduce time to remediation.


This visibility means your team can filter out findings that pose no real risk and focus on what’s both exploitable and relevant to business-critical systems, instead of focusing on static severity scores. In Preview, the engine has already helped customers reduce vulnerability noise by 95%.


[Bits Code](https://www.datadoghq.com/blog/bits-code/) and[Datadog’s AI-native Static Code Analysis (SAST)](https://docs.datadoghq.com/security/code_security/static_analysis/ai_enhanced_sast/#ai-native-sast) capabilities embed that deep visibility into your day-to-day development workflows. AI-native SAST reviews findings in context to assess whether they’re likely true or false positives before they reach your triage queue. To resolve a vulnerability, Bits Code generates code fixes directly, either one at a time or through bulk remediation campaigns. Security findings also surface in the IDE and in pull requests with the details that developers need in order to act, and issues automatically route to the right owners.


### Improve visibility into AI agent behavior


As more of our customers deploy AI, we’ve seen firsthand why combining security with observability is so critical for managing AI workloads. Securing AI agents specifically requires full visibility into runtime activity. To carry out their functions, AI agents often access sensitive data, read untrusted content, and communicate with data externally through HTTP calls, file writes, and commands. When[those three behaviors converge](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) , it creates conditions that make your agent a prime target for prompt injection, tool misuse, and data exfiltration.


[Datadog AI Guard](https://www.datadoghq.com/blog/ai-guard/) addresses these risks directly by providing runtime protection for agents. It evaluates the real time agent behavior, infrastructure signals, and data flows to discover unprotected agents, flag unsafe actions, and help block attacks in real time. It also maps unprotected agents across your environment with full lineage, surfacing model endpoints, data sources, and infrastructure dependencies that each agent accesses. That context is what helps teams adopt agentic workflows quickly, without security teams sacrificing oversight.


Those same protections extend to your development environment. AI Guard is expanding its capabilities to help improve the security of coding agents against[malicious skills](https://securitylabs.datadoghq.com/articles/malicious-skills-supply-chain-risks-in-coding-agents-with-dynamic-context/) , scripts, configurations, and packages. AI Guard sits inline with the agent to block indirect prompt injections, backdoor attacks, and other OWASP Top 10 threats, so your teams can use coding agents while protecting development pipelines.


## Detect, investigate, and contain threats with agentic defenses


Preventing exposure reduces your attack surface, but you also need to detect and respond when threats get through. Datadog provides AI-assisted threat detection and automated triage to help your team maintain a clear picture of the entire system as incidents unfold.


[Bits Security Analyst](https://www.datadoghq.com/blog/bits-ai-security-analyst/) enhances detection and investigation with deep behavioral insight and real-time context. It correlates signals across infrastructure, apps, identity, and network activity to surface threats the moment they emerge. Instead of your security analysts manually stitching together signals after the fact, Bit Security Analyst gives them a complete, continuously updated picture of an attack as it develops. Bits Security Analyst is now generally available as part of[Datadog Cloud SIEM](https://docs.datadoghq.com/security/cloud_siem/) , and as of DASH, we’re expanding availability so you can deploy it on third-party SIEMs without disrupting existing workflows.


For proactive defense,[Bits Threat Hunting](https://www.datadoghq.com/blog/bits-threat-hunting) —now available in Preview—lets your team surface anomalous behavior and uncover attack patterns before they escalate. When a threat is identified, Datadog isolates only the affected service, workload, identity, or agent and contains the blast radius without disrupting broader systems. Bits Threat Hunting is also linked to case management and incident response workflows, so your team can quickly move from detection to remediation, with every step captured from initial signal to closed case.


## Security that’s built for the way teams want to operate


The gap between how fast threats move and how fast teams can respond has been the defining problem in security for the last several years. That gap exists because security tooling has historically been siloed from the rest of the observability stack, forcing teams to manually correlate context across systems that have no visibility into each other.


Datadog Security is built on an observability platform that ingests and correlates telemetry data across your tech stack, including infrastructure, applications, logs, identities, APIs, AI workloads, and more. Instead of aggregating signals from disparate systems, Datadog preserves the relationships between entities and maps them to behavior for a continuously updated view of your environment. For the teams that own the systems, that means fewer handoffs, less manual triage, and a security posture that improves as your environment evolves. For teams building on AI, Datadog provides the visibility and guardrails needed to adopt agentic workflows safely. The same unified data layer that powers security detection also gives agents production-grade context, enabling observable and secure agentic operations from development to the SOC.


[See what the Datadog security platform can do for your team](https://docs.datadoghq.com/security/) , andsign up for a free 14-day trial if you’re new to Datadog.


-
-
-
