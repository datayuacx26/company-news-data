---
schema_version: "1.0.0"
document_id: "12c85b85600add0f8df04d3b798aab37950dd6f7c5f292c9ec5050f7be8755a9"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/timeline-mcp-breaches"
published_at: "2025-11-25T18:18:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:cfe56c94aba7e965dd24f2ef0d4638f75b18ab3ff016d050648b1dcf2a82742d"
---

# A Timeline of Model Context Protocol (MCP) Security Breaches

**Update:** This post was updated in May 2026 with FAQ guidance on recurring MCP breach patterns and reordered so the most recent breaches appear first. It was previously updated in April 2026 with mentions of new MCP-related breaches.


> AI fundamentally changes the interface, but not the fundamentals of security. Read on to find out why


It feels like eons ago when the **Model Context Protocol (MCP)** was introduced (it was *only*[in November 2024](https://www.anthropic.com/news/model-context-protocol) lol)


It promised to become the **USB-C of AI agents** — a universal bridge for connecting LLMs to tools, APIs, documents, emails, codebases, databases and cloud infrastructure. In just months, the ecosystem exploded: dozens of tool servers, open-source integrations, host implementations, and hosted MCP registries began to appear.


As the ecosystem rapidly adopted MCP, it presented the classic challenge of securing any new technology: developers connected powerful, sensitive systems without rigorously applying established security controls and fundamental principles to the new spec. By mid-2025, the vulnerabilities were exposed, confirming that the new AI-native world is governed by the same security principles as traditional software.


Below is the first consolidated timeline tracing the major MCP-related breaches and security failures - what happened, what data was exposed, why it happened, and what they reveal about the new threat surface LLMs bring into organisations.


# Timeline:


## Jan - Apr 2026


### **1. April 2026 - Design flaw in Anthropic's core MCP spec affected LettaAI, LangFlow, Windsurf & more**


-


**What happened:** A technical deep-dive identified a systemic design flaw in the core Anthropic Model Context Protocol (MCP) where the standard input/output (STDIO) transport allows direct configuration-to-command execution without sufficient input sanitization. This flaw, dubbed "The Mother of All AI Supply Chains," enables arbitrary OS commands to be executed via functions like` StdioServerParameters` and affects related tools like LangChain MCP adapters. The research has led to over 30 disclosures and 10 CVEs across multiple languages.[ox.security link](https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-technical-deep-dive/)


-


**Data breached & why:** The vulnerability affects over 150 million downloads. It has been weaponized to achieve authenticated Remote Code Execution (RCE) on Letta AI and unauthenticated server takeover on LangFlow (due to exposed MCP configurations), as well as bypassing security filters in Flowise. The issue is architectural, as the design permits subprocess execution, making ad hoc input filtering an insufficient defense. Researchers recommend using pre-configured lists of allowed commands or explicit "dangerous mode" flags.


### **2. March 2026 - Critical MCP Integration Flaw in nginx-ui**


-


**What happened:** A critical security vulnerability (CVE-2026-33032, CVSS 9.8) was discovered in` nginx-ui` , a web-based interface for managing NGINX configurations. The flaw was a result of insecure implementation of the Model Context Protocol (MCP), where the` nginx-ui` MCP message endpoint failed to perform authentication for command execution requests. Attackers could bypass authentication to restart the server or modify configuration files, and triggering automatic config reloads - achieving complete nginx service takeover.[CVE Link](https://nvd.nist.gov/vuln/detail/CVE-2026-33032)


-


**Data breached & why:** Compromising NGINX configuration could allow attackers to capture transit credentials or take down all backend applications. Over 2,600 publicly exposed` nginx-ui` instances were potentially vulnerable to full takeover.


### **3. Feb 2026 - Spreading malware via fake Oura MCP Project**


-


**What happened:** SmartLoader hackers cloned the legitimate Oura MCP project (a tool connecting AI assistants to Oura Ring health data) and distributed a trojanized version via public MCP registries. They manufactured credibility for their malicious repository by building a deceptive GitHub ecosystem, complete with fake contributors and multiple forks. The malicious package delivered the StealC information-stealing malware.[SecurityAffairs Link](https://securityaffairs.com/188135/ai/smartloader-hackers-clone-oura-mcp-project-to-spread-stealc-malware.html)


-


**Data breached & why:** The StealC malware is designed to harvest sensitive data, including developer credentials, browser passwords, API keys, and cryptocurrency wallets. While not a hack in the Oura MCP Server itself, this highlights how MCP servers are becoming a critical new attack surface for supply-chain attacks, requiring stronger vetting processes for AI tooling.


### **4. January 2026 - Unofficial Gemini MCP Tool 0-day Vulnerability**


-


**What happened:** A critical zero-day command injection vulnerability (CVE-2026-0755, CVSS 9.8) was found in the open-source` gemini-mcp-tool` , a utility for integrating Gemini AI models with MCP services ( **Note** : This is not an official Google Gemini MCP tool). The flaw allows an unauthenticated remote attacker to run arbitrary code with the service account's privileges. This stems from the tool's` execAsync` method passing user-supplied input directly into a system call without proper validation or sanitization.[CVE Link](https://www.cve.org/CVERecord?id=CVE-2026-0755)


-


**Data breached & why:** The vulnerability allows for remote code execution in the context of the service account, putting internet-exposed environments at high risk. As of the advisory, no official patch was available, recommending restrictions to trusted networks and monitoring for suspicious processes.


---


## Oct - Dec 2025


### **1. October 2025: figma‑developer‑mcp/Framelink MCP Vulnerability**


-


**What happened:** A command-injection flaw was discovered in the Figma/Framelink MCP integration: unsanitised user input in shell commands could lead to remote code execution.[The Hacker News Link](https://thehackernews.com/2025/10/severe-figma-mcp-vulnerability-lets.html)


-


**Data breached & why:** Because the integration allowed AI-agents to interact with Figma docs, the flaw could enable attackers to run arbitrary commands through the MCP tooling and **access design data or infrastructure** . The root cause was the unsafe use of` child_process.exec` with untrusted input in the MCP server code - essentially a lack of input sanitisation.[CVE Link](https://nvd.nist.gov/vuln/detail/CVE-2025-53967)


### **2. October 2025 – Smithery MCP Hosting Supply-Chain Breach**


-


**What happened:** While researching Smithery's hosted MCP server platform, GitGuardian found a **path-traversal bug** in the` smithery.yaml` build config. By setting` dockerBuildPath: ".."` , attackers could make the registry build Docker images from the **builder's home directory** , then exfiltrate its contents and credentials.[GitGuardian Blog](https://blog.gitguardian.com/breaking-mcp-server-hosting/)


-


**Data breached & why:** The exploit leaked the builder's **` ~/.docker/config.json`** , including a **Fly.io API token** that granted control over **>3,000 apps** , most of them hosted MCP servers. From there, attackers could run arbitrary commands in MCP server containers and tap **inbound client traffic that contained API keys and other secrets** for downstream services (e.g. Brave API keys), turning the MCP hosting service itself into a high-impact supply-chain compromise.


---


## Jul - Sept 2025


### **1. September 2025 - Flowise Critical Vulnerability via Systemic MCP Design Flaw**


-


**What happened:** A critical vulnerability in Flowise, enabling remote command execution, was disclosed. Researchers identified the issue as the systemic design flaw in the core architecture of MCP (the STDIO transport mechanism - see below). In Flowise, attackers can bypass security safeguards to access dangerous modules such as` child_process` and` fs` . This could lead to full system compromise including file system access and command execution.[CVE Link](https://nvd.nist.gov/vuln/detail/CVE-2025-59528)


-


**Data breached & why:** Unknown but unpatched instances of Flowise are compromised.


### **2. September 2025: Malicious MCP Server in the Wild**


-


**What happened:** A malicious MCP server package masquerading as a legitimate "Postmark MCP Server" was found injecting BCC copies of all email communications (including confidential docs) to an attacker's server.[IT Pro](https://www.itpro.com/security/a-malicious-mcp-server-is-silently-stealing-user-emails)


-


**Data breached & why:** Emails, internal memos, invoices — essentially all mail traffic processed by that MCP server were exposed. This was due to a **supply-chain compromise / malicious package** in MCP ecosystem, and the fact that MCP servers often run with high-privilege accesses which were exploited.


### **3. August 2025: Anthropic "Filesystem MCP Server" Vulnerabilities**


-


**What happened:** Security researchers found two critical flaws in Anthropic's Filesystem-MCP server: **sandbox escape and symlink/containment bypass** , enabling arbitrary file access and code execution.[Cymulate Link](https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/)


-


**Data breached & why:** Host filesystem access, meaning sensitive files, credentials, logs, or other data on servers could be impacted. The root cause was poor sandbox implementation and insufficient directory-containment enforcement in the MCP server's file-tool interface.


### **4. July 2025 – mcp-remote OS Command Injection**


-


**What happened:** JFrog disclosed **CVE-2025-6514** , a critical OS command-injection bug in **` mcp-remote`** , a popular OAuth proxy for connecting local MCP clients to remote servers. Malicious MCP servers could send a booby-trapped` authorization_endpoint` that` mcp-remote` passed straight into the system shell, achieving **remote code execution on the client machine** .[CVE Link](https://nvd.nist.gov/vuln/detail/CVE-2025-6514)


-


**Data at risk & why:** With over **437,000 downloads** and adoption in Cloudflare, Hugging Face, Auth0 and other integration guides, the vuln effectively turned any unpatched install into a **supply-chain backdoor** : an attacker could execute arbitrary commands, steal **API keys, cloud credentials, local files, SSH keys, and Git repo contents** , all triggered by pointing your LLM host at a malicious MCP endpoint.[Docker Blog](https://www.docker.com/blog/mcp-horror-stories-the-supply-chain-attack/)


---


## Apr - Jun 2025


### **1. June 2025 – Anthropic MCP Inspector RCE**


-


**What happened:** Researchers found that Anthropic's **MCP Inspector** developer tool allowed **unauthenticated remote code execution** via its inspector–proxy architecture. An attacker could get arbitrary commands run on a dev machine just by having the victim inspect a malicious MCP server, or even by driving the inspector from a browser.[CVE Link](https://nvd.nist.gov/vuln/detail/CVE-2025-49596)


-


**Data at risk & why:** Because the inspector ran with the user's privileges and lacked authentication while listening on localhost / 0.0.0.0, a successful exploit could expose **the entire filesystem, API keys, and environment secrets** on the developer workstation – effectively turning a debugging tool into a remote shell.[VSec Medium Link](https://medium.com/@vssec/when-local-isnt-local-the-critical-ai-security-flaw-you-probably-missed-8d99e554000e)


### **2. June 2025: Asana MCP Server Bug**


-


**What happened:** Asana discovered a bug in its MCP-server feature that could allow data belonging to one organisation to be seen by other organisations using their system.[Upguard link](https://www.upguard.com/blog/asana-discloses-data-exposure-bug-in-mcp-server) .


-


**Data breached & why:** Projects, teams, tasks and other Asana objects belonging to one customer potentially accessible by a different customer. This was caused by a logic flaw in the access control of their MCP-enabled integration (cross-tenant access not properly isolated).


### **3. May 2025 – GitHub MCP "Prompt Injection Data Heist"**


-


**What happened:** Invariant Labs uncovered a **prompt-injection attack** against the official GitHub MCP server: a malicious public GitHub issue could hijack an AI assistant and make it pull data from private repos, then leak that data back to a public repo.[Invariant Labs link](https://invariantlabs.ai/blog/mcp-github-vulnerability)


-


**Data breached & why:** With a single over-privileged Personal Access Token wired into the MCP server, the compromised agent exfiltrated **private repository contents, internal project details, and even personal financial/salary information** into a public pull request. The root cause was **broad PAT scopes combined with untrusted content (issues) in the LLM context** , letting a prompt-injected agent abuse legitimate MCP tool calls.


### **4. April 2025 – WhatsApp MCP Exploited: Chat-History Exfiltration**


-


**What happened:** Invariant Labs demonstrated that a malicious MCP server could **silently exfiltrate a user's entire WhatsApp history** by combining "tool poisoning" with a legitimate` whatsapp-mcp` server in the same agent. A "random fact of the day" tool morphed into a sleeper backdoor that rewrote how WhatsApp messages are sent.[Invariant Labs Link](https://invariantlabs.ai/blog/whatsapp-mcp-exploited)


-


**Data at risk & why:** Once the agent read the poisoned tool description, it happily followed hidden instructions to send **hundreds or thousands of past WhatsApp messages** (personal chats, business deals, customer data) to an attacker-controlled phone number – all disguised as ordinary outbound messages, bypassing typical Data Loss Prevention (DLP) tooling.


..And we're sure there are more to come. We'll keep this blog updated with the latest in security and data breaches in the MCP world.


---


## Patterns Emerging Across Incidents


Across all these breaches, common themes appear:


**1. Local AI dev tools behave like exposed remote APIs** -` MCP Inspector` ,` mcp-remote` , and similar tooling turned into Remote Code Execution (RCE) surfaces simply by trusting localhost connections.


**2. Over-privileged API tokens are catastrophic in MCP workflows** - GitHub MCP, Smithery, and WhatsApp attacks all exploited overly broad token scopes.


**3. "Tool poisoning" is a new, AI-native supply chain vector** - Traditional security tools don't monitor changes to MCP tool descriptions.


**4. Prompt injection becomes a full data breach** - The GitHub MCP incident demonstrated how natural language *alone* can cause exfiltration when MCP calls are available. It's no surprise that Prompt Injection tops[OWASP's Top 10 LLM](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) list


**5. (new) Systemic Architectural Flaws in Core Protocols** - The design flaw in the core Anthropic MCP STDIO transport allows direct configuration-to-command execution without sanitization, leading to RCE across multiple platforms like Flowise, LangFlow, and Letta AI. This single systemic issue affects over 150 million downloads.


**6. (new) Command Injection and Authentication Bypass on New MCP Endpoints** - Critical RCE vulnerabilities (like the` gemini-mcp-tool` 0-day) are caused by passing unsanitized user input directly to system calls (` execAsync` ). Other integration flaws, such as in` nginx-ui` , failed to perform basic authentication for command execution requests, allowing unauthenticated remote takeover.


---


## Conclusion


The Model Context Protocol (MCP) presents a cutting-edge threat surface, yet the breaches detailed here are rooted in timeless flaws: over-privilege, inadequate input validation, and insufficient isolation.


AI fundamentally changes the *interface* , but not the *fundamentals* of security. To secure the AI era, we must rigorously apply old-school principles of least privilege and zero-trust to these powerful new software components.


As adoption accelerates, organisations must treat MCP surfaces with the same seriousness as API gateways, CI/CD pipelines, and Cloud IAM.


Because attackers already are.


## FAQ


### What is the biggest recurring MCP breach pattern?


Over-privileged credentials combined with untrusted tool/context input remains the most repeated pattern in both incident reports and practitioner discussions.


### Does MCP itself provide security guarantees?


MCP standardizes integration interfaces; security depends on implementation controls like authorization, isolation, input validation, and least privilege.


### Why do MCP issues become supply-chain events quickly?


Shared adapters, registries, and reusable server packages let one design or package flaw propagate across many deployments.


### What should teams do before production use?


Use short-lived scoped tokens, isolate tools, enforce allowlists, monitor tool calls, and require human approval for high-impact actions.


Originally published November 25, 2025: Added FAQ guidance and reordered timeline so the most recent breaches appear first


On this page


- Timeline:
- Jan - Apr 2026
- 1. April 2026 - Design flaw in Anthropic's core MCP spec affected LettaAI, LangFlow, Windsurf & more
- 2. March 2026 - Critical MCP Integration Flaw in nginx-ui
- 3. Feb 2026 - Spreading malware via fake Oura MCP Project
- 4. January 2026 - Unofficial Gemini MCP Tool 0-day Vulnerability
- Oct - Dec 2025
- 1. October 2025: figma‑developer‑mcp/Framelink MCP Vulnerability
- 2. October 2025 – Smithery MCP Hosting Supply-Chain Breach
- Jul - Sept 2025
- 1. September 2025 - Flowise Critical Vulnerability via Systemic MCP Design Flaw
- 2. September 2025: Malicious MCP Server in the Wild
- 3. August 2025: Anthropic "Filesystem MCP Server" Vulnerabilities
- 4. July 2025 – mcp-remote OS Command Injection
- Apr - Jun 2025
- 1. June 2025 – Anthropic MCP Inspector RCE
- 2. June 2025: Asana MCP Server Bug
- 3. May 2025 – GitHub MCP "Prompt Injection Data Heist"
- 4. April 2025 – WhatsApp MCP Exploited: Chat-History Exfiltration
- Patterns Emerging Across Incidents
- Conclusion
- FAQ
- What is the biggest recurring MCP breach pattern?
- Does MCP itself provide security guarantees?
- Why do MCP issues become supply-chain events quickly?
- What should teams do before production use?


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Sohan Maheshwar · Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)
