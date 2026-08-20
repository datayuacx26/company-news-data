---
schema_version: "1.0.0"
document_id: "da800cba448b39a61643d8c2bc39b194b0c757309be1d1991549656d8771e020"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/devgovops-enables-dora-compliance/"
published_at: "2026-07-19T09:02:41+00:00"
first_seen_at: "2026-07-20T23:22:16.766689+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:28c0cea9747ee3adc9d736dde643e858328b3d99a2a8a4385ed36097b6efedad"
---

# Governance at the Speed of AI: How DevGovOps Closes the DORA Compliance Gap

What is DevGovOps?


[DevGovOps](https://jfrog.com/learn/grc/devgovops/) is a Software Supply Chain Engineering practice that integrates continuous governance and compliance into the DevOps software delivery lifecycle. Rather than treating compliance as a retrospective, manual hurdle, DevGovOps ensures that policy enforcement, continuous auditability, and cryptographic traceability are natural outputs of every release. By shifting from point-in-time checks to continuous proof, DevGovOps allows organizations to maintain a verifiable chain of custody over their software. This practice allows teams to govern at the velocity of AI-driven development while satisfying aggressive regulatory mandates like the Cyber Resilience Act (CRA) and NIST SSDF.


As autonomous AI coding agents transition from generating autocomplete suggestions to writing, compiling, testing, and deploying entire software pipelines, the connection between human intent and production binaries is fracturing. This has created a severe structural deficit: Our software pipelines run at machine speed, but our Governance, Risk, and Compliance (GRC) frameworks still crawl at human speed.


Bridging this gap requires more than retrofitting legacy security tools. It demands a dedicated, independent engineering and operational discipline: **Development Governance Operations (DevGovOps)** .


Every era of software delivery has demanded a new discipline to match its constraints.


**Discipline** **Unites** **The Shift It Answered** **The Result**


DevOps Dev + Ops Siloed handoffs were killing velocity CI/CD, automated testing, fast delivery


DevSecOps Dev + Security + Ops Gaps and slow pace were killing speed Security scanning in the CI/CD pipeline


DevGovOps Dev + Governance + Ops Governance can’t be retrospective Governance as a natural pipeline output


Much like DevOps united development and operations, and DevSecOps embedded security into the pipeline, DevGovOps bridges the gap between Engineering, Security, and Governance. It is a collaborative practice: AppSec defines the security and governance policies, and Engineering embeds those policies directly into the pipelines they manage. This ensures that software is not only secure and meets performance criteria but continuously compliant as well.


A compelling DevGovOps use-case is continuous compliance with the latest European Digital Operational Resilience Act (DORA), which is very real for the financial institutions and the software vendors supplying them across the European Union.


DORA compliance was built on the assumption that every production change traces back to a human who can be named, questioned, and held accountable. AI coding agents have broken that assumption. Most financial institutions haven’t caught up.


[In a recent JFrog webinar](https://jfrog.com/webinar/when-speed-breaks-compliance-dora-in-the-age-of-ai-agents/) , we walked through why this gap exists, what it costs, and demonstrated how to close it. While the webinar focused on the immediate, practical steps to solve governance challenges using the JFrog Platform, these challenges also highlight the accelerating shift towards recognizing DevGovOps as an essential part of software development operations.


## Why DORA’s Enforcement Phase Changes Everything


Regulators no longer ask whether you have a framework. They ask for six months of documented attestations.


*Regulators used to ask whether you have a framework. Today they demand six months of documented attestations.*


When DORA (EU Regulation 2022/2554) took effect in January 2025, regulators initially accepted periodic attestations and point-in-time policy reviews as evidence of control. That phase is over. National Competent Authorities (NCAs) across the EU now demand proof that controls operated continuously, for the last six months.


AI coding agents commit, build, and promote artifacts 10 to 50 times faster than traditional sprint-based development. At that volume and velocity, undocumented decisions accumulate faster than retroactive review or quarterly assessments can address them.


The evidence gap is measurable. According to the[JFrog 2026 Software Supply Chain Security State of the Union](https://jfrog.com/software-supply-chain-state-of-union/)[:](https://jfrog.com/software-supply-chain-state-of-union/)


- **35%** of new enterprise code is already AI-generated or AI-assisted.
- Organizations are managing **47%** more AI packages per year than twelve months ago.
- **59%** of organizations claim full production provenance visibility.
- Yet **48%** still need a week or more to produce compliance audit proof when asked.
- Only **6%** can do it in under a day.


That gap between making claims and proving them, is exactly where DevGovOps comes in, providing continuous attestation of every artifact and process in the supply chain.


## How Does DevGovOps Close the AI Compliance Gap in DORA?


Five compliance gaps recur consistently across financial institutions running agentic pipelines. Each maps directly to a DORA obligation and how an effective DevGovOps program closes it.


**DORA Pillar** **The Gap Caused by Agentic Development** **The DevGovOps Approach**


**ICT Risk Management** *(Art. 8)* AI agents pull undocumented libraries from npm, PyPI, or Maven without policy reviews, creating unmonitored ICT assets. **Automated Ingestion Control:**
The pipeline automatically intercepts and blocks non-compliant artifacts at the gateway, registering the policy evaluation within your system of record.


**Change Management** *(Art. 9)* Commits are pushed by automated systems with no human decision-maker, breaking the accountability chain. **Cryptographic Attestations:**
Every release promotion is signed with tamper-proof metadata detailing the policy-as-code gate that authorized it.


**Vulnerability Management** Legacy scanners miss LLM-specific flaws, API logical errors, and malicious package injection. **Contextual Policy Gating:**
Checks blocked artifacts based on CVE severity, license risk, and contextual scoring, catching gaps that post-deployment scanners find too late.


**Third-Party Risk**
*(Art. 28-44)* External AI models, SaaS endpoints, and package registries are accessed dynamically without formal vendor onboarding. **Unified System of Record:**
Every dependency, including AI model components and MCP registry components, is captured in a continuously generated SBOM, giving you a live, auditable inventory of your third-party ICT footprint.


**Incident Reporting** When an incident occurs, the security team cannot quickly trace which AI agent, prompt, or model generated the vulnerable code. **Application Context and Provenance:** Every release is linked back to its generation history and application context, allowing security to map the blast radius and human ownership in minutes.


None of these gaps close on their own. Closing them requires embedding governance into the pipeline, not patching it on afterward. That is what DevGovOps means in practice.


## What Does a Real Governance Failure Look Like?


A Nordic investment firm deployed an AI coding agent to accelerate delivery. In 90 days, the agent added 67 new open source dependencies to their production codebase. Not one was assessed against the firm’s DORA policy or gated against their third-party risk register. When a supervisory review arrived, there was no record of any dependency decision having been made. Remediation took four weeks and delayed a product launch.


The accountability for those 67 packages stayed exactly where it always had: With the development, security, and compliance teams. The agent produced the risk. The humans owned it.


## Three Principles of DevGovOps for DORA Governance at Agentic Speed


The supervisory tolerance period is over. Below are the foundational principles of DevGovOps, where governance is engineered into the development lifecycle rather than assembled after the fact.


In the context of DORA regulations, these principles become essential rather than optional:


1. **Policies are code, not documents** – Governance rules must be versioned, reviewed, and machine-enforceable by the pipeline itself. A policy that lives in a Word document cannot be tied to a specific artifact or retrieved by a regulator on demand.
2. **Governance runs at pipeline speed** – Blocking checks must execute on every commit, synchronously, without manual lag. Manual reviews and quarterly assessments cannot match agentic velocity. If your controls don’t run at the speed of your pipeline, they don’t run at all.
3. **Evidence is automatic, not assembled** – Immutable, timestamped records must be generated on every run as a byproduct of delivery. Evidence assembled after the fact is not the same as evidence that existed when the decision was made.


## The Gap Is There. The Question Is When You Close It.


While the principles above provide a theoretical framework for DevGovOps, executing them requires the right tooling. In our recent[DORA in the AI Era](https://jfrog.com/webinar/when-speed-breaks-compliance-dora-in-the-age-of-ai-agents/) webinar, we showed how the[JFrog Platform](https://jfrog.com/platform/) functions as a single system of record to put these principles into practice. Using[JFrog Curation](https://jfrog.com/curation/) and[JFrog AppTrust](https://jfrog.com/apptrust) , we demonstrated how to automatically block malicious packages at ingestion, enforce policy-as-code gates, and collect the cryptographic evidence needed to keep you constantly audit-ready.


If you are running AI agents in your pipeline and your governance has not caught up, the next supervisory review will find that gap. The organizations that close it now, by embedding governance into the pipeline rather than auditing around it, will be in a fundamentally different position when that review arrives.


---


## Frequently Asked Questions


### What is DORA and who does it apply to?


DORA (EU Regulation 2022/2554) is a binding EU law that took effect in January 2025. It applies to **financial institutions operating in or serving the European Union** —including banks, insurers, investment firms, and payment providers—as well as the **ICT third-party providers** supplying them. Its five pillars cover ICT risk management, incident reporting, resilience testing, third-party risk, and information sharing.


### Does DORA apply to software vendors outside the EU?


Yes. If your software or services are used by EU-regulated financial institutions, you fall within DORA’s third-party ICT provider scope under Articles 28-44, regardless of where your company is headquartered. Any vendor in the software supply chain of an EU financial institution should review their DORA obligations, particularly around contractual requirements, audit rights, and supply chain transparency.


### What are the penalties for DORA non-compliance?


Financial institutions found non-compliant face fines of up to **2% of total annual worldwide turnover** . Critical ICT third-party providers can face penalties up to **€5 million** . Beyond financial penalties, regulators can require firms to suspend or terminate contracts with non-compliant ICT providers, making DORA exposure a severe commercial risk, not just a compliance risk. *(Note: Exact penalty structures can vary by Member State enforcement; always consult legal counsel).*


### Why do AI coding agents create a specific DORA compliance problem?


DORA was designed around human-gated development processes where every change traces back to a named, accountable person. **AI coding agents break that accountability chain** by committing, building, and promoting artifacts autonomously. They pull dependencies without policy review, generate code with vulnerability classes legacy scanners miss, and produce no audit trail by default, leaving organizations unable to satisfy DORA’s evidence requirements on demand.


### What is DevGovOps and how does it address DORA?


DevGovOps is a Software Supply Chain Engineering practice that embeds continuous governance and compliance into the software delivery lifecycle. Rather than assembling compliance evidence after the fact, DevGovOps **generates cryptographic attestations, enforces policy-as-code gates, and maintains a unified system of record** automatically at every pipeline stage. For DORA specifically, it ensures that the evidence regulators require exists at the moment each decision is made, not weeks later.


### How long does it take to produce DORA audit proof with and without DevGovOps?


According to the[JFrog 2026 Software Supply Chain Security State of the Union](https://jfrog.com/software-supply-chain-state-of-union/) , **48% of organizations need a week or more** to produce compliance audit proof when asked, and **only 6% can do it in under a day** . With a DevGovOps approach, evidence is generated automatically at every pipeline run and is retrievable on demand, replacing multi-week manual assembly exercises with proof that already exists.


---


To go deeper into DevGovOps and what continuous governance looks like in practice, visit[jfrog.com/learn/grc/devgovops](http://jfrog.com/learn/grc/devgovops) .
