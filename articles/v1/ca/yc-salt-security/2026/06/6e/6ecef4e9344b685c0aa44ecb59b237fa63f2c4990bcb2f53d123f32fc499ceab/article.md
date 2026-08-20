---
schema_version: "1.0.0"
document_id: "6ecef4e9344b685c0aa44ecb59b237fa63f2c4990bcb2f53d123f32fc499ceab"
company_key: "yc-salt-security"
company: "Salt Security"
source_id: "yc-salt-security-news-import-ef3d8363d45c"
canonical_url: "https://salt.security/press-releases/salt-security-launches-salt-code-the-first-agentic-security-solution-to-enforce-security-policies-inside-ai-coding-assistants"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-22T12:39:57.986974+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:230fc39abc120372eaed32166e355ba92f4e6098f922e3d3ba170770fc706905"
---

# Salt Security launches Salt Code

*The first solution that enforces security policy inside every AI coding assistant your developers use. Claude, Cursor, GitHub Copilot, Windsurf, Codex and Gemini CLI now generate policy-compliant code by default, from prompt to production.*


**PALO ALTO, Calif. — June 1, 2026 —**[Salt Security](https://salt.security/) , the leader in API and Agentic Security, today announced the launch of Salt Code, a new component of their Agentic Security Platform that enforces security policies across the full development lifecycle for AI-generated code. As enterprises standardize on AI coding assistants from Anthropic, OpenAI, Google, GitHub, Cursor, Codeium, and a growing field of new entrants, Salt Code ensures that every line of generated code, regardless of which assistant produced it, is compliant with internal standards, industry best practices, and regulatory requirements from the moment it is created.


AI coding assistants are now generating a significant share of all enterprise code. But none of these tools is trained on an organization's internal security policies, industry frameworks, or compliance requirements. Insecure patterns ship without anyone noticing. SAST and DAST tools catch problems too late in the pipeline, when every fix is a rewrite and every rewrite is a delay. Policy enforcement lives in PDFs, wikis, and tribal knowledge that the AI building the software has never read.


## By the numbers


**AI coding is the enterprise default.** GitHub Copilot is now deployed at 90% of Fortune 100 companies, and paid Copilot subscribers reached 4.7 million by January 2026, up roughly 75% year over year. (Microsoft)


**Nearly half of new code is machine-written.** GitHub reports that AI coding assistants now generate 46% of code written by developers on the platform. Sonar's 2026 developer survey puts AI-generated or AI-assisted code at 42% of all enterprise code, projected to exceed 50% by 2027. (GitHub, Sonar)


**Roughly half of AI-generated code introduces a known vulnerability.** Veracode tested more than 100 large language models on security-sensitive coding tasks and found 45% of AI-generated code samples introduce OWASP Top 10 vulnerabilities. Independent analysis from CodeRabbit found AI pull requests contain 2.74 times more vulnerabilities than human-written ones. (Veracode, CodeRabbit)


**The risk curve is accelerating.** CVE counts traced directly to AI-generated code rose nearly 6x year over year. March 2026 alone disclosed 35 new CVEs from AI coding tools, exceeding all of 2025 combined. (Georgia Tech Vibe Security Radar)


[Salt Code](https://salt.security/code) addresses this problem directly. At its core is Salt's Posture Governance Engine, a unified policy layer that defines security and compliance standards once and enforces them everywhere code is created, reviewed, deployed, and run. With Salt Code, that same policy model now spans the three dimensions that matter most in agentic systems: code, control plane configuration, and runtime behavior. By connecting the Posture Governance Engine to the tools developers already use, Salt Code makes AI coding assistants generate compliant code by default, without requiring developers to ask for it. Salt Code ships with a pre-built library of policies covering[OWASP API Top 10](https://salt.security/blog/owasp-api-security-top-10-explained) , MCP Security Top 10, LLM Security Top 10, OpenAPI/Swagger compliance, and common regulatory frameworks, with support for custom organizational policies.


The result is one security standard applied to every developer in the organization. Seasoned engineers and citizen developers produce code at the same baseline. Vibe coders, agentic workflows, and overnight prototypes all answer to the same policies the enterprise expects.


> “AI is writing code faster than organizations can govern it, whether that AI is Claude, Gemini, Copilot, or the next tool a developer downloads tomorrow. Salt Code changes the equation. For the first time, security policy travels with the code itself, from the first prompt through every stage of the pipeline and into runtime. Organizations no longer have to choose between the speed AI enables and the security their business requires.” **Roey Eliyahu,** CEO and Co-founder, Salt Security


> "I regularly point organizations toward Salt because the full Agentic Security Graph is genuinely differentiating. Salt Code is the piece that ties it together. With code-level context layered onto runtime behavior, Salt is building a multi-dimensional defense for agentic systems rather than another single-point tool. That is the direction this market needs to move." **Christopher M. Steffen** , CISSP, CISA, CCZ, VP of Research, Information Security, Risk and Compliance Management, Enterprise Management Associates


## How Salt Code works


Salt Code applies policy-driven security across five stages of the development lifecycle. For developers, it works silently in the background through their existing AI coding assistant. For security teams, it provides a central policy console where standards are defined once and enforced automatically across every AI-assisted development workflow in the organization.


- **Unified governance from code to runtime.** Salt Code gives security teams one policy model for how agentic systems are built, configured, and validated in production across APIs, MCP integrations, and agents.
- **Discover.** Salt Code identifies APIs, MCP* servers, and AI agent integrations across code repositories and cloud environments, giving security teams visibility into what is being built and how systems are connected.
- **Enforce during code generation.** Security policies are applied in real time as developers generate code. Salt Code connects to AI coding assistants through the open MCP servers. The Model Context Protocol was originally developed by Anthropic and has been adopted by OpenAI, Google, and Microsoft. This standards-based approach means Salt Code works with any MCP-compatible assistant or code review workflow.
- **Govern in the pipeline.** Policy validation extends into CI/CD workflows. Violations are blocked before they reach production. Downstream SAST and DAST findings drop dramatically because the underlying issues were never written.
- **Validate in runtime.** Salt Code continuously monitors behavior across APIs, MCP integrations, and agents in production, using Salt's existing runtime engine driven by the same policy set used at code generation, to detect policy violations, posture gaps, and anomalous activity as systems actually run, not as they were supposed to run on paper.
- **Remediate and improve.** Salt Code is designed to translate runtime findings into actionable fixes fed back into developer workflows and AI assistants, with expanded automation capabilities planned for later in 2026.


## Built for every assistant in the stack


Salt Code supports the leading AI coding assistants at general availability, including Claude Code, Cursor, GitHub Copilot, Windsurf, Kiro, Codex, Gemini CLI, and Antigravity. It plugs into the source control and pipeline tools developers already use, including GitHub, GitLab,


** MCP or Model Context Protocol is the open protocol that lets AI assistants connect to external context and tools - it's how every modern coding assistant talks to data sources*


Bitbucket, VS Code, any IDE supporting MCP server configuration, and major CI/CD platforms. Workflow integrations with Jira and ServiceNow route findings into the ticketing systems that security teams already operate. Additional integrations are on the 2026 roadmap.


## Why Salt Code is different


Existing security tools were built to review code after it is written. They catch problems downstream, generate significant noise in development pipelines, and require security teams to intervene manually. Salt Code shifts enforcement left, to the moment of code creation, making compliance a property of the code itself rather than a gate applied after the fact.


As a result of Salt’s unique architecture, Salt is the only solution capable of delivering a unified policy across the full lifecycle. The same policy that governs AI-generated code at the moment of creation also governs the APIs and agents built from that code at runtime. No point solution can replicate that continuity, because no other vendor operates at both ends of the lifecycle.


The launch of Salt Code extends Salt's Agentic Security Platform from runtime protection into the development lifecycle, giving organizations a complete security foundation that covers both how agentic systems are built and how they behave once deployed.


## Availability


Salt Code is available today. Current Salt Security customers receive it at no additional cost as part of their existing license. Non-customers can request free access through Salt's Early Access Program (EAP), available to the first 100 organizations to enroll, with all four pre-built Secure Coding Packs included: the OWASP API Top 10, MCP Security Top 10, LLM Security Top 10, and OpenAPI/Swagger compliance. Access tokens can be requested at[salt.security/salt-code](https://salt.security/salt-code)
