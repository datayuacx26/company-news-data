---
schema_version: "1.0.0"
document_id: "1980a2bceea1ce88368245bb0d49c390836d24bd5c147d92f23eaf3c6da2c44d"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-f76a50baf54b"
canonical_url: "https://www.okta.com/newsroom/articles/okta-july-2026-product-innovations/"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T19:07:44.935044+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:408ee1b720731ef8f8591a2be5ce3f3b1daee57c00e03efc7847b7add9987c02"
---

# Okta announces new innovations to secure AI agents at runtime and automate ongoing agent governance

Today, Okta announced new innovations within[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) to help organizations secure agent connections to enterprise tools and other agents at runtime, while governing these connections over time. Through Agent Gateway and Agent-to-Agent Connections, organizations can enforce precise controls and policies at the exact moment an agent tries to access sensitive data or hand off a task to another agent. Resource Access Certifications for AI Agents helps maintain appropriate access by continuously reviewing and right-sizing permissions.


“Enterprises are deploying AI agents in higher-value, but more complex workflows that require deeper access to sensitive resources and collaboration with other agents,” said Ely Kahn, Chief Product Officer, Okta. “With these new innovations within Okta for AI Agents, every agent connection can be evaluated and authorized in real time, then continuously validated as projects evolve or an agent’s scope changes. This ongoing governance will be critical for helping our customers continue to build their secure agentic enterprises.”


Together with other features of Okta for AI Agents, organizations can discover and register agents as first-class identities, enforce least-privilege access, govern agent lifecycles, and deactivate access to stop rogue agents. As an independent, vendor-neutral platform, Okta enables companies to manage agents from any vendor.


### Securing agent access to enterprise tools with Agent Gateway


All types of AI agents are connecting to enterprise tools, but the greatest security challenge comes from third-party agents, as[76% of AI applications](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/) are purchased rather than built internally. Some of these agents’ code cannot be easily modified to work with an organization's identity system, so IT teams often rely on granting static credentials to connect them to enterprise applications. The result is broad access, no user attribution, and no audit trail.


[Agent Gateway](https://www.okta.com/blog/product-innovation/agent-gateway-runtime-governance/) enables Okta to sit between agents and the systems they access without requiring code changes. Unlike traditional API and MCP gateways that only route network traffic, Okta's gateway is identity-native. This means when an agent calls a tool, it validates the agent's identity and the user behind it, checks the policies governing that agent, and brokers a short-lived credential at runtime.


It works with any agent that can connect to an external MCP server—including local coding agents like Claude Code and GitHub Copilot, SaaS agents like Salesforce Agentforce, and agents from builder platforms like Amazon Bedrock AgentCore. Capabilities include:


-


**Prevent credential theft and prompt injections:** Isolate downstream credentials within Okta so they remain out of reach for the AI agent, helping prevent prompt injections or model compromises from exfiltrating credentials.


-


**Deactivate rogue agents:** Revoke a rogue agent’s access at the gateway, blocking new connections without requiring credential rotation or collateral downtime for other agents.


-


**Help eliminate compliance risks:** Log connections with the agent's identity, the initiating end user, and the transaction outcome, providing a complete audit trail.


### Enabling multi-agent workflows with Agent-to-Agent Connections


Agents are not just connecting to tools; they are increasingly connecting to other agents. Multi-agent workflows allow one agent to autonomously invoke secondary agents, such as a sales agent triggering a finance agent. This creates complex machine-to-machine connections that extend beyond traditional security boundaries.


[Agent‑to‑Agent Connections](https://www.okta.com/blog/product-innovation/securing-agent-to-agent-connections/) enable multi‑agent workflows by securing the handoff between agents. Whether a workflow is started by a human or an autonomous agent, it verifies connections are authorized, traceable, and can be tied back to the initiator.


Security teams gain visibility into how agents collaborate across the organization, while developers can build and scale multi-agent architectures without creating custom authorization logic for every agent connection. Capabilities include:


-


**Prevent unauthorized lateral movement:** Define and enforce precise connection policies, specifying exactly which agents are permitted to invoke other agents.


-


**Enforce least-privilege access:** Issue temporary runtime tokens that restrict each agent's access to only the specific data and systems required for its task.


-


**Accelerate audit readiness:** Embed the complete chain of custody directly into the token, providing delegation proof for compliance, auditing, and rapid incident response.


"Our AI Fabric is built on the principle that every AI agent should have a trusted identity, just like every employee," said Shawn Fogarty, Director of IT, LogicMonitor. "Identity is the foundation for secure AI at enterprise scale, and Okta's Agent-to-Agent Connections helps our AI agents securely authenticate, collaborate, and act on behalf of users with the visibility and governance enterprises require. That gives us the confidence to scale AI across the business while maintaining the trust, security, and compliance our customers expect."


### Preventing permission creep with Resource Access Certifications for AI Agents


While controls enforced at runtime can govern agent connections in the moment, they do not address whether an agent should continue to retain access on an ongoing basis. When projects conclude or agent scopes shift, permissions are rarely revoked, leaving behind a high-risk trail of forgotten connections and unreviewed privileges.


This challenge compounds as these connections increase, making manual oversight and proper governance increasingly difficult. In fact,[84% of organizations](https://cloudsecurityalliance.org/press-releases/2026/02/05/cloud-security-alliance-strata-survey-finds-that-enterprises-are-in-time-to-trust-phase-as-they-build-ai-autonomy-foundations?utm_source=chatgpt.com) doubt they could pass a compliance audit focused on agent behavior or access controls.


**Resource Access Certifications for AI Agents** helps ensure agent connections are right-sized over time through automated access reviews. It manages agent access to enterprise resources across platforms, closing governance gaps left by legacy identity platforms and single-vendor tools that only cover their own ecosystem. Capabilities include:


-


**Unified visibility:** Surface human users and AI agents side-by-side, allowing organizations to certify the complete identity footprint accessing their systems.


-


**Automated remediation of privilege creep:** Revoke stale connections automatically to reduce risk exposure, lower the operational burden of manual reviews, and reduce costs.


-


**Audit-ready compliance:** Log certification decisions with timestamps and attributions to show who authorized access, providing audit-ready reports.


These features help organizations address two core pillars of[the blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) by providing better control over what agents can connect to and what agents can do.


### *Availability:


-


Customers can request access to Agent Gateway’s research release now.


-


Agent-to-Agent Connections is available in General Availability.


-


Resource Access Certifications for AI Agents is available in Early Access.


Learn more about how Okta for AI Agents helps organizations transform into a secure agentic enterprise[here](https://www.okta.com/products/govern-ai-agent-identity/) , and read more on how to sign up for the Agent Gateway[here](https://www.okta.com/blog/product-innovation/agent-gateway-runtime-governance/) .


*Any mention in this article of solutions, features, functionalities, certifications, authorizations, or attestations that are not currently generally available or have not yet been obtained may not be delivered or obtained on time or at all. We assume no obligation to deliver on such items and you should not rely on them to make your purchase decisions.


About Okta


Okta


Okta, Inc. is The World’s Identity Company™. We secure AI, machine, and human identity so everyone is free to safely use any technology. Our customer and workforce solutions empower businesses and developers to protect their AI agents, users, employees, and partners while driving security, efficiencies, and innovation. Learn why the world’s leading brands trust Okta for authentication, authorization, and more at[okta.com](http://okta.com/) .
