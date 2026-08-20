---
schema_version: "1.0.0"
document_id: "052f1590b312a210ccc372042940db7048f841be3e23e88d7e29da06e55741b5"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/open-secure-ai-alliance-contributions/"
published_at: "2026-08-04T13:00:47+00:00"
first_seen_at: "2026-08-04T14:19:09.624764+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:c2152a08a6f3961d6fea6e2a99697ac87c84cd18129244556974c734f01d79aa"
---

# AI Leaders Propose SAFE Guidelines for Cybersecurity Transparency

Members of the


[Open Secure AI Alliance](https://blogs.nvidia.com/blog/open-secure-ai-alliance/) — now more than


120


organizations strong — are developing new guidelines to strengthen agentic AI cybersecurity as the annual Black Hat conference begins in Las Vegas today.


[The Linux Foundation](https://www.linuxfoundation.org/blog/proposing-the-safe-working-group-an-open-community-effort-to-improve-ai-security) today shared a


[Request for Comments](https://github.com/OpenSecureAIAlliance/RFCs) on Shared AI Findings Exchange (SAFE), a proposed set of guidelines designed to turn agentic cybersecurity incidents into shared protection for the entire ecosystem.


The SAFE guidelines are being drafted by an Open Secure AI Alliance working group. NVIDIA, Cisco,


CrowdStrike


,


Hugging Face


and


Red Hat


are among Open Secure AI Alliance members working with the Linux Foundation to contribute to the initial proposal.


The SAFE guidelines include proposals to confidentially collect and analyze AI incidents and near misses, inform those impacted, identify recurring control failures and publish evidence-based operating recommendations that reduce systemic risk.


Cybersecurity is a race without a finish line. Every major technology shift has created new potential attack surfaces. Defenders must move now at agent speed to respond rapidly to protect infrastructure and intellectual property — and the best way to do that is together. When trusted ecosystems share threat intelligence openly, collective defense becomes a force multiplier.


## Open Secure AI Alliance Delivers More Tools for AI Cybersecurity


The SAFE framework adds to technology contributions Open Secure AI Alliance members are making as part of a shared commitment to building and sharing open, inspectable tools across the full AI security stack.


An AI agent isn’t just a model. It’s a system — identity controls, harnesses, guardrails, logs and evaluation — and securing it requires more than vulnerability scanning.


Security has always been strongest in the layers — and in the community’s willingness to share what it knows. The hardest problems get solved when defenders learn from each other, openly and at speed.


## Full Stack of Open NVIDIA Cybersecurity Software and Models


NVIDIA’s contributions run the length of the stack, starting with the


[NVIDIA Labs Object-Oriented Agent (NOOA)](https://developer.nvidia.com/blog/six-agent-harness-capabilities-for-higher-model-performance/?ncid=prsy-823400) research harness, on


[GitHub](https://github.com/NVIDIA-NeMo/labs-OO-Agents/tree/main) — which makes agent behavior easier to test, trace, audit and govern.


The


[NVIDIA OpenShell](https://developer.nvidia.com/blog/run-autonomous-self-evolving-agents-more-safely-with-nvidia-openshell/) runtime restricts what an agent can see, touch and do — enforcing security and privacy controls at the agent level, so an agent can’t reach what it shouldn’t.


NVIDIA’s open model families —


[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) for agentic AI,


[NVIDIA Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) for physical AI,


[NVIDIA Isaac GR00T](https://developer.nvidia.com/project-gr00t) for robotics,


[NVIDIA BioNeMo](https://www.nvidia.com/en-us/industries/healthcare-life-sciences/) for healthcare and life sciences, and


[NVIDIA Alpamayo](https://blogs.nvidia.com/blog/alpamayo-2-super-open-model-now-available) , the world’s largest model for autonomous vehicles licensed for commercial use — ship with open weights, datasets and training techniques.


NVIDIA open source


[verified agent skills](https://developer.nvidia.com/blog/nvidia-verified-agent-skills-provide-capability-governance-for-ai-agents/) extend that trust to the capability layer.


Each skill provides portable instruction sets — cataloged, scanned for risks such as prompt injection and tools poisoning, cryptographically signed and documented with a skill card. Defenders know exactly what an agent skill does, where it came from and whether it was modified after publication.


NeMo Guardrails, NeMo Anonymizer and NeMo Safe Synthesizer help enforce safety policies, protect sensitive data and generate privacy-safe synthetic data.


And


[Garak](https://github.com/NVIDIA/garak) , NVIDIA’s open source LLM vulnerability scanner, lets security teams check models for data leaks, prompt injections and jailbreak scenarios before they ship.


## Alliance Members Expand Tools for Open Ecosystem Development


Other members of the Open Secure AI Alliance have also been building across the full defensive stack, spanning identity and permissions, harnesses, runtime guardrails, security AI models, observability and evaluation, data security and privacy, availability and resilience, and more.


Some of the latest contributions across different layers of the stack are highlighted below, with more continuing to arrive.


## Identity and Permissions — Who Gets to Act


You can’t secure what you can’t identify.


Okta


is developing reference implementations for agent identity and access, showing how


[Cross App Access](https://xaa.dev/) (XAA), an open protocol, enables AI agents operating in OpenShell sandbox environments to securely connect to enterprise applications.


Palo Alto Networks


has contributed open source tools from Idira, its next-generation identity security platform, including


[Agent Guard](https://github.com/cyberark/agent-guard) and


[Agent Watch](https://github.com/cyberark/agentwatch) . These tools help developers and agent builders apply identity security best practices and safe guards such as securely retrieving secrets for agentic workflows.


A new open source project founded by


Red Hat


,


[asago](https://redhat.com/en/about/press-releases/red-hat-launches-asago-community-automate-ai-safety-and-governance-policy-production) takes an organization’s custom governance requirements — such as those referenced in NIST, OWASP and the EU AI Act — and maps them directly to what agents are allowed to do at runtime, with a single audit trail from policy clause to live control.


## Harnesses and Tooling — How Security Work Gets Orchestrated


AI agents are more than a model – they tap into systems of open and closed models, harnesses, tools and runtimes to get work done.


If a model is the agent’s brain, the harness is the body that takes action by using tools. The harness surrounding the model acts like an orchestrator that determines how agents are deployed, coordinated and constrained.


Alliance members are contributing tooling, harnesses and supporting technologies across this emerging layer of the AI security stack.


Amazon


, which today became one of the newest members of the Open Secure AI Alliance, contributes


[Strands Agents](https://github.com/strands-agents) , an open source toolkit for building AI agents that is open at every layer, giving developers full visibility into agent behavior and the ability to evaluate agentic systems in production. Amazon also contributes


[Cedar](https://cedarpolicy.com/en) , an open source authorization language that enforces deterministic, verifiable boundaries on what AI agents are permitted to do, giving customers fine-grained, analyzable access controls to help ensure only authorized actions reach enterprise resources.


[Capital One](https://www.capitalone.com/tech/open-source/announcing-vulnhunter/) open sourced


[VulnHunter](https://github.com/capitalone/vulnhunter) for agentic AI code security.


Cloudflare


is offering its


[Vulnerability Discovery Harness](https://blog.cloudflare.com/build-your-own-vulnerability-harness/) as an open source skill to add security to agent systems.


Microsoft


AI Red Team has open sourced several tools and harnesses.


[PyRIT – Python Risk Identification toolkit](https://github.com/microsoft/PyRIT) enables AI red teamers to run automated red teaming, with built in memory, supporting common targets, as well as custom endpoints.


[RAMPART](https://www.microsoft.com/en-us/security/blog/2026/05/20/introducing-rampart-and-clarity-open-source-tools-to-bring-safety-into-agent-development-workflow/) turns red-team findings and real-world incidents into repeatable tests that run as software changes.


[Clarity](https://www.microsoft.com/en-us/security/blog/2026/05/20/introducing-rampart-and-clarity-open-source-tools-to-bring-safety-into-agent-development-workflow/) helps teams question design assumptions and identify potential failures before code is written.


Microsoft has also open sourced


[Assert](https://github.com/responsibleai/ASSERT) , which converts natural language requirements and expected AI safety and security behaviors into executable evaluations.


[Atlas](https://www.wiz.io/blog/atlas-ai-vulnerability-researcher) is


Wiz’s


autonomous vulnerability research engine that orchestrates specialized AI agents to discover and validate security flaws across code and open source packages.


Visa


has also joined the Open Secure AI Alliance, contributing its open sourced


[Visa Vulnerability Agentic Harness](https://corporate.visa.com/en/sites/visa-perspectives/security-trust/visa-cybersecurity-mythos-project-glasswing.html) to help teams identify issues, support remediation and validation, quickly and safely.


## Models — Intelligence Built for AI Safety and Defense


Not every security or safety task calls for a general-purpose model. Specialized security and safety models are purpose-built for defense: trained to understand code, locate vulnerabilities and reason about threats at scale. They can work to support agentic workflows as systems of models, with both open and closed models working together to get the job done efficiently.


Cisco


DefenseClaw is an open source agentic governance layer that sits on top of NVIDIA OpenShell to provide robust, automated security at the runtime level when scaling agentic workforces. Cisco has also released two of its


[Antares](https://blogs.cisco.com/ai/introducing-antares-the-most-efficient-open-weight-ai-models-for-vulnerability-localization) security small language models to help pinpoint where known vulnerabilities exist within a codebase; and


[Project CodeGuard](https://blogs.cisco.com/ai/announcing-new-framework-securing-ai-generated-code) to embed secure-by-default practices directly into AI coding workflows.


CrowdStrike


is fine-tuning the NVIDIA Nemotron Nano model for cyber defense. Internal testing achieved 96% accuracy in generating investigation queries within Falcon LogScale, delivering a natural-language interface that boosts agent investigative efficiency. CrowdStrike has also published research demonstrating how a specialized NVIDIA Nemotron Nano reasoning model outperforms much larger models on Security Operations Center detection triage while introducing calibrated logit-based confidence to enable measurable, tunable, and auditable autonomous security decisions.


Seeing what an agent did is only part of the picture. Defenders also need to understand why it acted, whether the system behaves safely and how attacks are evolving in the real world.


[Akamai](https://www.akamai.com/blog/news/thinking-outside-black-box-defenders-open-source-ai) brings insights from its


[State of the Internet reports](https://www.akamai.com/security-research/the-state-of-the-internet) and


[Security Intelligence Group research](https://www.akamai.com/security-research) , drawing on real-world data to illuminate AI-era threats and explain how emerging exploits work so defenders can learn, adapt and respond.


Cognition


has released a


[trustworthiness evaluation](https://cognition.com/blog/measuring-open-source-model-trustworthiness) , which measures alignment and security risks of open source-derived models. The evaluation demonstrates these risks can be mitigated via post-training.


[Numbat](https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat) is


Perplexity’s


open source agent security suite for client endpoints. It detects, investigates, and prevents agent activity across macOS, Linux and Windows — giving defenders a structured record of what agents actually did.


Uber


open sourced key components of


[ADR (Agentic AI Detection and Response)](https://nam11.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Fuber%2FADR&data=05%7C02%7Csmcphee%40nvidia.com%7C68be605362cc4f1e085f08def1a8d7cd%7C43083d15727340c1b7db39efd9ccc17a%7C0%7C0%7C639213905000463800%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=Zo5onsG8wuh8mzvJdeh2bD8VS%2FCptVzEWiarAAi5TJE%3D&reserved=0) , a production system that reconstructs the full causal chain of AI agent activity -– from prompt to reasoning, tool calls, and outcomes -– to help security teams detect threats. Today, ADR supports more than 200,000 agent sessions per day across 30,000 endpoints, using a two-tier analysis approach that combines efficient detection with deeper investigation for high-confidence threats.


## Availability and Resilience — Rapid Recovery When Moments Count


Agent systems must remain dependable under disruption, contain failures and recover safely without losing critical state or exposing the broader environment.


LangChain


is adding resilience capabilities to its open source frameworks — Deep Agents, LangGraph and LangChain — enabling agents to retry interrupted work, follow a safe recovery path, resume from a saved state instead of starting over and automatically fall back to alternative models when the primary model fails.


[Veeam](https://www.veeam.com/blog/veeam-open-secure-ai-alliance.html) helps organizations keep the data and infrastructure behind AI resilient and recoverable with technologies such as


[Kanister](https://nam11.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.veeam.com%2Fcompany%2Fpress-release%2Fkasten-by-veeams-kanister-accepted-by-cloud-native-computing-foundation-cncf-as-sandbox-project.html&data=05%7C02%7Cjenniec%40nvidia.com%7C38865c9cbaec44f0c21c08def16d8455%7C43083d15727340c1b7db39efd9ccc17a%7C0%7C0%7C639213650000221567%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=SOiI%2B5zuQf2qrT8GhOAQ8Rhs509KIDGKX0Svf7jN2gk%3D&reserved=0) , its open source framework for data protection on Kubernetes. It helps teams protect and recover AI workloads, vector databases, and data to a verified known-good state.


More contributions are coming. When members publish reusable mitigations, defenders across the ecosystem can inspect, adapt and improve them, helping security practices evolve as AI advances.


Join members of the Open Secure AI Alliance at Black Hat today, Tuesday, Aug. 4, at 5:15pm PT, for a group photo outside the Main Stage, Business Hall at the Mandalay Bay Convention Center.


[Learn more or share interest](https://www.nvidia.com/en-us/open-secure-ai-alliance-contact-us/) *in joining the Open Secure AI Alliance.* **
