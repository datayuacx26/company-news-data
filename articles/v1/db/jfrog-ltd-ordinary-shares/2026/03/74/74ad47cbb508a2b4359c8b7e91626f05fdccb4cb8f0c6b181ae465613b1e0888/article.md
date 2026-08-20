---
schema_version: "1.0.0"
document_id: "74ad47cbb508a2b4359c8b7e91626f05fdccb4cb8f0c6b181ae465613b1e0888"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-news-import-4f455b4bb3a3"
canonical_url: "https://jfrog.com/blog/q1-2026-new-innovations-announcement/"
published_at: "2026-03-18T17:57:11+00:00"
first_seen_at: "2026-07-27T12:11:59.089588+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:e42a3a16213f32b19fdda67b86def0623a5dd651570d232c647da8a05b601aa7"
---

# 9 New Innovations. One Trust Layer.

The software supply chain is no longer just about shipping code, it is about managing intelligence and risk. As DevOps, DevSecOps, DevGovOps and AI/ML practices converge into a single AI-driven and increasingly agentic delivery pipeline, the demands on development and security teams have reached a new level.


The platform that once managed packages and artifacts now governs models, agents, and skills at enterprise scale, speed, and accountability.


Recently in NYC, during an event for some of our largest customers, we unveiled 9 new innovations that further establish JFrog as the trust layer for this new world of human and AI-driven software delivery. Spanning two domains — **Security & Governance for the Agentic Frontier** and **Enterprise Scale & Global Resilience** — these innovations give organizations the tools to govern AI, reduce risk, and move at speed.


## Secure and Govern the Agentic Frontier


AI agents are no longer experimental. They are proliferating across enterprise environments, connecting to more systems, trusted with more actions, and generating artifacts of their own. The infrastructure governing needs to keep pace. The following four innovations establish the governance, compliance, and security controls organizations need to deploy agents with confidence, covering the MCP servers agents connect to, the skills they execute, the policies that gate their behavior, and the IDE agentic workflows where it all begins.


### 1. MCP Registry


** *From a single unvetted connection to an enterprise-grade asset, the JFrog MCP Registry ensures every MCP server is secured, versioned, and ready for your AI agents*


Developers and AI agents are using malicious and vulnerable MCP servers that can take unauthorized action on enterprise data. Other solutions only monitor activity, failing to prevent agents from connecting to unverified tools.


The[JFrog MCP Registry](https://jfrog.com/ai-catalog/mcp-registry/) is the unified control plane that serves as the single source of truth for all your MCP servers. It treats MCP servers as first-class artifacts helping you securely scale your agentic software supply chain.


- **Value:** Enable AI-driven development while automatically blocking unvetted MCP servers at the perimeter. Granular tool-level permissions ensure agents and developers only do what they are explicitly authorized to do.
- **Result:** Secure IDE-to-server connectivity with full governance, unifying MCP servers, AI models, and software artifacts into a single trusted control plane.
- **Availability:** GA, part of JFrog AI Catalog


**[Learn More](https://jfrog.com/ai-catalog/mcp-registry/)**[Book a Demo](https://jfrog.com/ai-catalog/demo/)


###


### 2. Agent Skills Registry


*Secured, signed, and vetted skills are centralized and delivered to agents and developers*


AI agents lack a dedicated infrastructure layer to enforce the security and privacy controls needed to make their skills safe for enterprise use, creating major breach and compliance risks. As[recent OpenClaw manipulations](https://jfrog.com/blog/giving-openclaw-the-keys-to-your-kingdom-read-this-first/#:~:text=OpenClaw%20offers%20powerful%20AI%20agent%20automation%2C%20but%20broad%20permissions%20create%20massive%20security%20risks.) demonstrated, an unvetted skill can guide an agent to perform harmful actions just as a malicious package can compromise an application.


The[JFrog Agent Skills Registry](https://jfrog.com/blog/agent-skills-new-ai-packages/) is the control plane for the full lifecycle management and governance of agent skills. It automatically scans, verifies, and cryptographically signs all skills upon upload, with policy-driven approval workflows that ensure agents access only permitted, verified skills allowing you to gain control without sacrificing speed or stifling innovation.


- **Value:** A single source of truth to store, scan, and govern all agentic binary assets across the software supply chain creating the gold standard for safe autonomous agent deployment at scale.
- **Result:** Organizations can confidently deploy autonomous, long-running agents knowing every skill they execute has been verified. This capability powers JFrog’s collaboration with[NVIDIA](https://jfrog.com/jfrog-and-nvidia/) as the official system of record for NVIDIA NemoClaw and the AI-Q enterprise blueprint.
- **Availability:** Beta open now


**[Learn More](https://jfrog.com/blog/agent-skills-new-ai-packages/)**[Sign Up for the Beta](https://jfrog.com/beta-signup/agent-skills-registry/)


###


### 3. JFrog Plugin for Cursor


*Add the official JFrog Plugin to your Cursor agent*


The modern developer workflow no longer starts at a terminal or a pipeline, it starts inside[AI-native IDEs like Cursor](https://www.linkedin.com/feed/update/urn:li:activity:7439687008362520577) , where agents suggest code, pull dependencies, and make supply chain decisions in real time. But they do so without any visibility into whether packages are safe, compliant, or policy-approved.


The JFrog plugin for Cursor brings the full power of the[JFrog Platform](https://jfrog.com/platform/) directly into the developer’s AI-native coding environment, no context switching, no manual lookups, no friction. It ships with four integrated components: a remote MCP server connection (authenticated via OAuth, no API keys needed), an AI skill that guides developers through JFrog Platform operations using natural language, a security rule that automatically enforces supply-chain best practices whenever dependency files are touched, and a dedicated[supply-chain security agent](https://jfrog.com/blog/agentic-software-supply-chain-security-ai-assisted-curation-remediation/) that can audit dependencies for CVEs, license risks, and curation policy violations.


- **Value:** Developers can manage artifacts, scan for vulnerabilities, and enforce organizational security policies without ever leaving their IDE, all through conversational AI interactions powered by the JFrog MCP Server.
- **Result:** Security moves from a downstream gate to an upstream guardrail, catching risks before they are committed, meeting the speed of the agentic development lifecycle.
- **Availability:** Verified by Cursor and available now on the Cursor Marketplace.


[Learn More](https://cursor.com/marketplace/jfrog/)


###


### 4. Policy-as-Code (PaC)


*Example of policy written as code to validate SBOM attestation from JFrog Xray*


Pre-defined policy templates fail to fit unique compliance requirements, forcing organizations to rely on brittle DIY scripts and manual audits that can’t scale with the pace of delivery.


[JFrog AppTrust](https://jfrog.com/apptrust/) now natively supports Policy-as-Code. By uploading industry-standard Open Policy Agent (OPA) and Rego rules directly into JFrog, organizations can “Bring Your Own Policy”. This pairs the flexibility of PaC with JFrog AppTrust’s immutable System of Record, allowing you to express any custom governance rule as code that natively and physically blocks non-compliant artifact versions.


- **Value:** Endless flexibility to customize evidence-based gates, enforce any governance rule without sacrificing developer velocity, and prove compliance cryptographically by binding evidence directly to the binary for complete audit readiness.
- **Result:** Replace brittle scripts and manual audits with a governed, auditable, code-defined compliance layer that scales as fast as your pipeline.
- **Availability:** API is GA, part of JFrog AppTrust


[Learn More](https://jfrog.com/apptrust/)


##


## Ensure Enterprise Scale and Global Resilience


Enterprise software delivery runs on infrastructure. When that infrastructure bloats, fragments, or fails, the entire pipeline stops. The five innovations outlined below ensure the JFrog platform scales cleanly across distributed teams and geographies, with consistent policy enforcement, high-performance federation, and the uptime guarantees that mission-critical delivery demands.


### 5. JFrog Curation Federation


*View curation policy synchronization status across sites*


Organizations managing distributed sites often enable governance at a siloed level, creating “governance drift” where policies diverge between sites and leave the enterprise exposed.


JFrog Curation Federation centralizes policy control with a single control plane, instantly pushing security guardrails to every global JFrog site the moment they are defined.


- **Value:** A “secure-by-default” posture maintained consistently across every pipeline, team, and environment with no manual synchronization.
- **Result:** Global teams ship software faster with confidence, knowing central policies are identical everywhere, no manual, site-by-site configurations or redundant “Allow” lists required.
- **Availability:** GA, Enterprise + customers with[JFrog Curation](https://jfrog.com/curation/) .


[Learn More](https://jfrog.com/curation/)


###


### 6. Repository Federation Updates: JFrog Bridge, Unidirectional Federation, Remote & Virtual Federation


Organizations struggle to balance hybrid-cloud adoption and global developer speed with strict one-way network security and regulatory compliance. Legacy replication tools often lack the necessary performance, and geographically distributed teams face significant latency when fetching on-demand dependencies.


To overcome these boundaries, we have introduced three targeted repository federation capabilities that ensure high-performance asset flow without compromising your compliance posture:


**JFrog Bridge** – Enables two-way federation over one-way networks through secure, encrypted connections, achieving smooth hybrid-cloud adoption without risky architecture changes.


- **Availability:** GA, Enterprise X


*JFrog Bridge creates a secure connection for federating repositories behind corporate firewalls*


[Learn More About JFrog Bridge](https://jfrog.com/help/r/jfrog-platform-administration-documentation/manage-jfrog-bridges)


**Unidirectional Repository Federation** – Enforces strict one-way asset flows with 20x faster synchronization than legacy replication, meeting compliance mandates without sacrificing speed.


- **Availability:** GA, Enterprise X


*Unidirectional federation supports multiple architectures giving flexibility in artifact synchronization*


**Remote & Virtual Repository Federation** – Extends high-performance federation across every repository type, giving global teams local-speed access on every dependency fetch.


- **Availability:** Preview


[Learn More about Unidirectional, Remote & Virtual Federation](https://jfrog.com/blog/beyond-mirroring-with-jfrog-repository-federation/)


###


### 7. Smart Retention: Hybrid Smart Archiving + Automated Cleanup


*View and manage your retention policies (cleanup and archive) in one place*


High-velocity development paired with data retention regulations create bloated repositories that slow down systems, create security and compliance risks, and confuse the AI agents that depend on them as their source of truth.


Smart Retention, combining Smart Archiving and Automated Cleanup, manages retention through automated, policy-driven rules with dry runs, and complete audit logging.


Smart Archiving provides development teams with “Archive as a Service,” enabling them to move non-production artifacts to optimized long term storage while maintaining full artifact integrity for single click restore. Both self-managed and SaaS customers can now take advantage of Smart Archiving.


- **Value:** Keep storage and database clean, comply with regulatory retention requirements, and ensure AI agents are pulling from repositories that reflect current, trusted state.
- **Result:** Leaner repositories, stronger compliance posture, and better performance, with full auditability of every archiving and cleanup action taken.
- **Availability:** Hybrid Smart Archiving: GA, Enterprise+ add-on | Automated Cleanup: GA, Enterprise X


[Learn More](https://jfrog.com/blog/the-need-for-clean-in-the-ai-era/)


###


### 8. Nix Package Manager


*Native Nix support in JFrog Artifactory*


[Nix](https://jfrog.com/learn/devops/nix/) guarantees deterministic builds, but lacks the enterprise-grade distribution, governance, and resilience that production scale demands, forcing teams to build custom caching and access control layers.


JFrog has delivered the first native Nix repository manager, bringing enterprise-grade binary caching, fine-grained access control, and immutability policies to Nix artifacts.


- **Value:** Accelerate builds with a global enterprise-grade binary cache, insulate CI/CD pipelines from public registry outages, and govern Nix binaries with the same rigor applied to every other artifact in the platform.
- **Result:** Teams that depend on deterministic builds get the enterprise reliability and governance their supply chains require, without having to build it themselves.
- **Availability:** GA, All subscriptions


[Learn More](https://jfrog.com/blog/native-nix-support-in-artifactory/)


###


### 9. Premium Availability of 99.99%


*JFrog takes software resilience to the next level with 99.99% uptime SLA*


The software supply chain is mission-critical. When your delivery pipelines stop, your business stops, and for many high-velocity and risk-averse organizations, there is a strong desire for uptimes above 99.9%.


Premium Availability offers the first in-region 99.99% uptime SLA across the JFrog Platform, delivered through premium infrastructure. JFrog handles migration, so there’s no hassle for customers.


- **Value:** Predictable performance for “always-on” software delivery, with an uptime guarantee that aligns with system retry thresholds and eliminates developer downtime.
- **Result:** Internal software delivery infrastructure matches the strict business and regulatory SLAs organizations promise to their customers.
- **Availability:** GA, Enterprise+ add-on


[Learn More](https://jfrog.com/blog/improve-resilience-with-9999-sla/)


##


## Trusted Software Delivered


All of these innovations answer a single question: how do you establish trust in an agentic software supply chain? The answer rests on three principles.


1. Every agent and the artifacts it generates must be treated as a first-class security concern, from the moment it is built to the moment it ships.
2. Trust must be computed, not assumed — security must be native, baked into the system where your binaries live and enforced at the point of request.
3. And every artifact must carry its own proof: its lineage, compliance record, provenance, and cryptographic attestations, wherever it goes.


AI can help secure the code. But it cannot govern the binary. That is where JFrog goes further — adding governance, lineage, and provenance in addition to security, baked into the same system where your binaries live. The binary is the source of truth. JFrog makes sure it carries its own proof.


This is what it means to have JFrog as the trust layer for your agentic software supply chain.


For more information, visit our[website](https://jfrog.com/) , take a[virtual tour](https://jfrog.com/start/try-cloud/) , or set up a one-on-one[demo](https://jfrog.com/schedule-an-integration-demo/) at your convenience.
