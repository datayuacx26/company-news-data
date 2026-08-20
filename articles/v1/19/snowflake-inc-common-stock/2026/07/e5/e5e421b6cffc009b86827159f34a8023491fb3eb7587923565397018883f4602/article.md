---
schema_version: "1.0.0"
document_id: "e5e421b6cffc009b86827159f34a8023491fb3eb7587923565397018883f4602"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/enterprise-ai-security-agentic-mcp-governance"
published_at: "2026-07-28T12:58:00+00:00"
first_seen_at: "2026-07-28T15:26:54.414544+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:0d55df959744c3a5a9f687e342b37adbde617afd0c531422dc61c5c741d79b99"
---

# Enterprise AI Security: Agentic Controls and MCP Governance

AI security concerns have[surged from 17% in 2024 to 48% in 2026](https://www.linuxfoundation.org/press/linux-foundation-report-finds-greatest-obstacle-for-ai-adoption-and-innovation-is-a-security-readiness-crisis) according to The Linux Foundation’s[2026 State of Tech Talent Report](https://www.linuxfoundation.org/research/open-source-jobs-report-2026) — a critical shift given that while 97% of organizations are committed to implementing AI, 57% face a significant capacity gap in security and risk management. By combining data access, system execution and data movement into a single profile, autonomous agents have been dramatically expanding the enterprise attack surface. A patchwork of application-layer fixes and legacy monitoring tools is no longer enough. To safely scale enterprise AI, security must be built directly into the data and control planes.


At Black Hat 2026, Snowflake is delivering that foundation and announcing Cortex AI Gateway and major production-ready AI security advancements.


## Agent interoperability layer: Extending Snowflake governance with Cortex AI Gateway


With the rapid rise of autonomous agents, teams are quickly leveraging standards like MCP to connect LLMs to databases, internal tools and SaaS environments. However, decentralized adoption creates unmanaged sprawl, fractured user experiences and severe security liabilities, leaving organizations vulnerable to unvetted servers, tool hijacking and data exfiltration. As enterprises scale autonomous agents across models, tools and platforms, they're running into a hard problem: fragmented access, no visibility into what agents are doing and AI costs spiraling out of control.


By integrating[Natoma](https://www.snowflake.com/en/blog/snowflake-acquire-natoma-governed-agentic-access/) — a centralized MCP gateway that enforces identity, policy and audit at the tool-call level — into the Snowflake ecosystem, the **Cortex AI Gateway** is the connective layer for all trusted agent activity. It enables enterprises to realize new levels of governed agent interoperability, while giving enterprises visibility and control over AI consumption costs.


Cortex AI Gateway governs how AI agents — both first-party tools (like Snowflake CoCo and CoWork) and third-party ecosystems (such as Amazon Bedrock, Azure AI Foundry, ChatGPT, Claude Code, Cursor, custom LangChain or LlamaIndex apps, and others) — access models, data, MCP servers and enterprise tools. Enterprises can govern MCP tool usage with **Tools by Cortex AI Gateway** through a centralized gateway, giving teams control and visibility into who requested the action, what permissions they have and whether the action is allowed.


As the first milestone on our AI gateway roadmap, Cortex AI Gateway provides the core infrastructure needed to unify tool access, security and governance for enterprise agents. It extends Snowflake’s rigorous data governance framework to agent traffic.


Cortex AI Gateway delivers three things enterprises need to scale agents safely:


**Control:** Grant, restrict and audit model and tool access from a single endpoint. Instead of manually configuring each new agent type, teams can manage permissions centrally with fine-grained authorization.


- **Wide Model Catalog (private preview):** Bring the models you need under one roof — GPT, Gemini, Claude, Grok, Mistral, GLM and more — and run them in your geography, helping keep data within the region it needs to stay in.
- **Access Governance and Sprawl Control (private preview):** This reduces the need for administrators to manually configure connections for dozens of emerging agent types. Instead, teams can grant, restrict and audit tool access from a single endpoint.


- **Govern Every Agent Connection (private preview):** Experience streamlined access and data policies, authentication, fine-grained authorization and permissions across 100+ MCP servers (including BYO and VPC connect), automatic discovery and monitoring of shadow AI and MCPs.


**Visibility:** Agent actions are captured in real time: which tool was called, which system it touched, in what order and by whom. Audit trails give security and compliance teams the evidence they need without instrumenting each agent individually.


- **Observability and Tracing (private preview):** Agent tool calls can be securely captured in real time, providing the comprehensive audit trails required for usage tracking, troubleshooting and forensics.
- **Agent Action Auditability (private preview):** Access an end-to-end record of agent actions, including which systems it touched and in what sequence.


**Cost and performance:** Route requests automatically to the right model based on cost, latency, capability and other requirements. Enforce spending limits by team, agent or workload before costs run away.


- **AI Cost Control (private preview):** Get a unified view of AI consumption by team, agent or workload; manage and apply budget guardrails; route to cheaper models for simpler tasks without sacrificing quality.
- **Intelligent Model Routing (private preview):** Automatically route agent requests to the right model based on cost, latency, capability and data residency requirements, so enterprises get better output without overpaying or sending sensitive data to the wrong region including prompt management.


## AI security stack: Transitioning to production-grade


Complementing our new AI MCP gateway capabilities, we are transitioning a set of native, enterprise-grade security capabilities to general availability (GA) and public preview. These defense-in-depth features help your AI workloads and data remain protected in production environments.


### Securing agent identity and sessions


-


**Agent Identity (GA):** Security and governance teams can now have greater visibility into agent activity. Teams can enforce data access policies that apply specifically when an agent is in the session, helping keep sensitive data protected even when the agent runs on behalf of a privileged user. We have also added dedicated agent identity tracking in Account Usage views for auditing.


-


**Third-Party Agent Identity:** We are extending these robust identity frameworks to third-party agents through integrations with leading security innovators such as 1Password, Aembit, Cyera, Linx Security, Okta, SailPoint and Saviynt. This means the same governance policies you apply to Snowflake-native agents can be extended to cover external AI tools.


-


**Restricted Session Scope (GA soon):** Restricted Session Scope limits what an agent session can do to only what the task requires, so a read-only analysis stays read-only, even if the user's role normally allows much more.


- **Context-aware access policies (private preview):** Our new Context-Aware Access Policies are a zero-trust Snowflake control that allow security admins to author a single policy that evaluates identity, network and client context jointly in one expression.


### Proactive AI security posture management


**Native AI Security Posture Management (GA):** Managing the continuous configuration risks of complex AI workloads is now fully integrated into the[Snowflake Trust Center](https://trust.snowflake.com/) . Security operations teams gain a comprehensive dashboard to proactively scan for AI-specific risks, assess compliance postures against emerging global regulations and deploy programmatic remediations to lock down misconfigurations.


### Zero-trust data exfiltration control


-


**Advanced Data Exfiltration Prevention:** Snowflake has launched its Data Exfiltration Prevention (DXP) package into preview via the Trust Center. By pairing real-time telemetry with strict data movement policies (GA), organizations can detect and help intercept unauthorized data flows before they exit the ecosystem. The platform can proactively flag and block:


-


Sensitive data fetches triggered by AI agents


-


Unauthorized data routing to internal or external stages


-


Mass data downloads via user interfaces


-


**Client-side CoCo CLI VM Sandbox (private preview):** Run AI-assisted development workflows while minimizing exposure of credentials, local storage or networks to client-side AI workloads. Virtual Machine (VM) Sandboxing isolates each CoCo session in a separate Linux kernel, isolated from the host operating system, so developers get the full CoCo experience while your security and compliance teams get the isolation they require. Available as an admin-enforced or self-managed control, currently on macOS.


### Multi-party resiliency controls


**Ransomware Protection via Multi-Party Approval (MPA)** is now GA, removing single points of failure from your most sensitive architecture. It requires two or more authorizations before any destructive system change can proceed, so even if top-tier administrative credentials are hijacked, ransomware actors cannot unilaterally wipe data or alter configurations, helping to improve your enterprise’s resilience against extortion.


## Securing the next era of enterprise innovation


Securing the agentic enterprise begins with trust; to empower agents to reason and act safely across systems, security must be built into the core data and AI infrastructure. That is why Snowflake addresses the entire workflow: protecting data, securing models and governing agents. By placing security controls directly alongside enterprise data and context allows organizations to innovate without compromise. Teams can move confidently from prototype to production and deploy AI agents where they belong: inside the secure enterprise.


**Join Snowflake at Black Hat USA 2026:** Visit the **Snowflake booth #8206** to experience demonstrations of the Cortex AI Gateway, AI Agent Identity controls and the automated threat scanners built directly into the Snowflake Trust Center. If you’re interested in more details, you can also talk to one of our product team members.


Start your AI transformation journey by[trying Snowflake for yourself](https://signup.snowflake.com/) .


Learn more about[how to secure the agentic enterprise](https://www.snowflake.com/en/blog/securing-the-agentic-enterprise/) .


*Forward-looking statements: This content contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.*
