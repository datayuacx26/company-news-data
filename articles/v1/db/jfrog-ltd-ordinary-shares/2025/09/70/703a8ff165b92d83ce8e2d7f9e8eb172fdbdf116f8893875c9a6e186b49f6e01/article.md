---
schema_version: "1.0.0"
document_id: "703a8ff165b92d83ce8e2d7f9e8eb172fdbdf116f8893875c9a6e186b49f6e01"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-news-import-4f455b4bb3a3"
canonical_url: "https://jfrog.com/blog/agentic-software-supply-chain-security-ai-assisted-curation-remediation/"
published_at: "2025-09-09T12:01:01+00:00"
first_seen_at: "2026-07-27T12:11:59.089588+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:2ed07ca64a82756f710cc22f163c8bc100aeb4cbd7c2630fd9de0c9b89b5d532"
---

# Agentic Software Supply Chain Security: AI-Assisted Curation and Remediation

Software supply chains are the #1 attack vector for cybercriminals, and the challenge isn’t just finding vulnerabilities; it’s fixing them fast while ensuring security, compliance, and developer productivity. As supply chains grow in complexity, traditional tools aren’t enough; organizations need intelligent, autonomous assistance embedded directly into developer workflows.


We are pleased to announce that **JFrog is introducing Agentic Software Supply Chain Security** to help organizations **reduce risk, cut costs, and accelerate delivery** . By combining JFrog’s trusted platform with **AI-driven automation** , development teams can shift from reactive security practices to proactive, agentic software supply chain security, curating safer software packages, remediating CVEs, and coding with confidence.


## Agentic Software Supply Chain Security from JFrog


Agentic Software Supply Chain Security is a culmination of various tools and capabilities within the JFrog Software Supply Chain Platform as well as integrations with external partners, and includes JFrog Catalog, Curation, SAST, GitHub Copilot, and VSCode. Here’s how they all work together to shift development teams from reactive security practices to proactive, agentic security.


### Curation: Faster and Smarter Package Selection


Open source is the foundation of modern software, but with millions of packages and varying license obligations, curating safe and compliant dependencies can be daunting.


With[JFrog Catalog & Curation](https://jfrog.com/curation/) , developers can now build with confidence. AI-powered agents, connected to JFrog security solutions via the JFrog remote MCP (Model Context Protocol), analyze package metadata, security posture, and compliance with organizational policies, helping teams select the best open-source libraries at speed. By ensuring developers can only use the safest, policy-compliant packages, teams avoid failed builds from vulnerabilities and keep CI/CD pipelines running smoothly, shortening release cycles and accelerating delivery.


#### Curation Workflow Example:


- **Step 1:** A developer writes code with the assistance of an AI agent (e.g., GitHub Copilot).
- **Step 2:** Copilot selects the required packages and validates with Curation through JFrog MCP.
- **Step 3:** JFrog Curation evaluates the package against security and license policies and CVE databases supported by JFrog Catalog
- **Step 4:** The AI Agent with JFrog insights (via remote MCP) replaces bad package versions with ones that pass the Curation policy.


The result: **faster innovation without sacrificing security or governance.**


### Secure and Friendly Agentic Source Code Remediation


Security shouldn’t slow developers down. Instead, it should meet them in the IDE, during coding, in a way that promotes frictionless innovation.


[JFrog SAST](https://jfrog.com/sast/) surfaces source code vulnerabilities directly in the IDE. With **agentic remediation** , developers get contextual, friendly, and actionable AI-suggested code changes in real-time so that they don’t have to sift through security logs or reports. The JFrog local SAST MCP connects the JFrog Platform to your chosen AI agent. The agent gets insights from the SAST engine, which scans the codebase and generates SAST findings.


#### Coding Workflow Example:


- **Step 1:** A developer writes new code.
- **Step 2:** JFrog scans the code and flags any vulnerable patterns, e.g., SQL injection
- **Step 3:** The developer asks the AI agent to fix any SAST issues in the code.
- **Step 4:** The AI Agent receives remediation information from the SAST engine to provide a secure code fix inline (“Convert to parameterized query”).
- **Step 5:** The developer reviews and accepts or rejects the suggested code.


This ensures teams aren’t just finding problems, but are continuously writing **secure code by default** .


### Automated Remediation or “Ask Copilot to Fix”


Vulnerabilities in open-source dependencies (CVEs) remain one of the most exploited attack vectors in the software supply chain. Identifying them is only half the battle; the real challenge is remediating them quickly and accurately.


The “Ask Copilot to Fix” feature is part of our VSCode extension and automatically suggests or applies patches, dependency upgrades, or safe alternatives. The “Ask Copilot to Fix” action can be triggered for various security findings, including those from SAST, Secrets Scanning, and IaC analysis. This makes remediation **seamless, efficient, and integrated directly into the developer experience** .


#### CVE Remediation Workflow Example:


1. The VSCode extension scans your entire codebase.


- If you have JFrog Advanced Security, the scan includes contextual analysis, SAST, secrets detection, and Infrastructure as Code (IaC) analysis.
- For example, *the scan detects a CVE in a dependency, log4j version 2.14.1* .


2. The developer then chooses the option to ‘ask Copilot to fix’ the detected issue.
3. The remediation information is passed to Copilot from JFrog.
4. Copilot generates the code fix based on the JFrog remediation information.


Instead of overwhelming teams with alerts, JFrog empowers them with **autonomous, agentic remediation** that keeps the supply chain secure without slowing delivery.


## The JFrog Advantage


JFrog helps teams shift from reactive to proactive agentic security. With JFrog’s deep security research at its core, the JFrog platform ensures comprehensive protection and actionable intelligence. By connecting AI agents to the JFrog platform via MCP servers, and by using the JFrog VSCode plugin, developers gain:


- Automated package curation to reduce supply chain risk.
- Inline, context-aware code security and remediation.
- Seamless CVE and other fixes that accelerate release cycles.


This isn’t just an AI assistant; it’s agentic, autonomous remediation that transforms DevSecOps into a self-healing software supply chain. Unlike point solutions, JFrog delivers:


- **End-to-end visibility** from code to runtime.
- **Agentic AI workflows** embedded across curation, coding, and CVE remediation.
- **Trusted security intelligence** integrated with GitHub, IDEs, and enterprise DevSecOps pipelines.


With JFrog, organizations can move from reactive patching to proactive, autonomous, and continuous security.


### Business Outcomes


Here are the outcomes organizations can expect with Agentic Software Supply Chain Security from JFrog.


#### Speed to Market


- AI-curated open-source packages reduce delays in sourcing and compliance checks.
- Developers spend less time researching libraries and more time innovating.
- This can yield **faster coding and remediation** of CVEs.


#### Risk Reduction


- Automated CVE remediation can help shrink exposure windows
- Agentic source code remediation reduces human error and ensures security by design.
- Improved license compliance reduces legal and reputational risk.
- **A significant ROI** can be achieved through avoided breaches; the average cost of a software supply chain incident exceeds[$4.4M](https://www.ibm.com/reports/data-breach) .


#### Operational Efficiency


- AI-powered remediation reduces manual triage, freeing security engineers for high-value tasks.
- Seamless IDE integration lowers developer context-switching, improving productivity.
- Automated remediation realizes **a significant reduction in time spent** evaluating open-source dependencies.


#### Cost Savings


- Faster cycles mean fewer incidents, outages, and lower breach-related costs.
- With AI-powered code generation and remediation assistance, developers can realize[up to a 2x productivity boost](https://www.businessinsider.com/ai-coding-tools-popular-github-gemini-code-assist-cursor-q-2025-7) as AI handles repetitive tasks.


## The Future of Agentic Security


The future of DevSecOps isn’t just about shifting left, it’s about **agentic AI** : autonomous security that works as fast as your developers.


With agentic AI capabilities embedded across the JFrog Platform, developers gain:


- **Speed** through AI-curated open-source packages.
- **Security** with SAST-driven agentic code remediation.
- **Seamlessness** in CVE detection and auto-fixes.
- **Confidence** to deliver software at scale, without compromise.


By combining trusted DevSecOps foundations with autonomous AI agents, JFrog is making **Agentic Software Supply Chain Security** a reality, helping organizations deliver secure, reliable, and compliant software at the pace of innovation. To learn more,[schedule a demo](https://jfrog.com/platform/schedule-a-demo/) , take an[online tour,](https://jfrog.com/start/try-github/?utm_PE=github) or head over to the[GitHub Marketplace](https://github.com/marketplace/jfrog-for-github) to connect your GitHub and JFrog instances to enjoy AI-assisted, secure coding.
