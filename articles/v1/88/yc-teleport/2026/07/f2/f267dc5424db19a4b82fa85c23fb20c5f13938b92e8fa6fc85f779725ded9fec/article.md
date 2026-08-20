---
schema_version: "1.0.0"
document_id: "f267dc5424db19a4b82fa85c23fb20c5f13938b92e8fa6fc85f779725ded9fec"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/kubernetes-agent-identity-access/"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:20377d8619916e9ca3eff98da64efa8ee86566ca6a1d1f82a19f231e091672be"
---

# Kubernetes for Agentic AI: Best Practices for Identity and Access

**Read this article to learn:**


- When agents need their own workload identity, and when inheriting a user's is sufficient
- Why Kubernetes RBAC isn't enough to isolate agents sharing infrastructure
- How to limit agent data access to the task being performed


In[Part 1 of this series](https://goteleport.com/blog/kubernetes-for-agentic-ai/) , we addressed 18 Kubernetes best practices spanning across container hardening, observability, availability, and fault tolerance. Those practices secure the containers that agents run in. But the[CNCF AI Technical Community Group's cloud-native agentic standards](https://www.cncf.io/blog/2026/03/23/cloud-native-agentic-standards/) go further, establishing that securing containers is only the beginning.


When agents run, they authenticate to infrastructure, invoke tools, query databases, and act on behalf of users. Sometimes they do this autonomously, and sometimes they operate across long-running sessions that persist beyond a user's active involvement. In these cases, the initial user's identity no longer represents the agent; the agent within the container needs its own identity layer.


## Measuring the need for a unique agent identity


Not every agent needs a dedicated identity. The CNCF article makes this distinction: if an agent operates strictly within an active user session, acts only on explicit instructions, and terminates when the user disconnects, inheriting the user’s identity is likely sufficient. But agents that act autonomously, initiate workflows outside the user's permission scope, or persist beyond a session require a distinct workload identity.


An agent with its own identity is accountable and traceable, but one borrowing a user's identity is neither. Teleport's[2026 Infrastructure Identity Survey](https://goteleport.com/resources/surveys/infrastructure-identity-survey-2026/) notes that 70% of organizations currently grant their AI systems more access than a human in the same role. This level of over-provisioning makes identity reuse across agents especially dangerous.


The CNCF recommendations for agent identity include assigning a unique workload identity to each agent instance using[SPIFFE IDs](https://goteleport.com/docs/machine-workload-identity/workload-identity/spiffe/) or Kubernetes service accounts, using short-lived OIDC tokens or SPIFFE/SVID certificates bound to the agent's execution context (for example, namespaces or pod UID), and re-authenticating before sensitive actions are taken rather than relying on session-start authentication alone. Network enforcement boundaries via service meshes, Kubernetes NetworkPolicy, and API gateways constrain which tools and services each agent can reach, limiting lateral movement if an agent is compromised or manipulated through prompt injection. For naming and discovery, the article points to the OWASP Agent Name Service (ANS), which enables cryptographically verifiable agent discovery across multi-agent deployments.


The survey also found that 67% of organizations report a high reliance on static credentials such as API keys and long-lived tokens. That reliance aligns with a 20-percentage-point increase in AI-related incidents. Using short-lived certificates is the recommended strategy for mitigating this risk. Teleport has been an active participant in this community, and members of our engineering team serve on the SPIFFE Steering Committee. We see this approach as a best practice as systems grow increasingly abstracted across cloud, container, and on-prem environments.


## Managing agent tenancy


Multi-agent environments introduce a tenancy problem that Kubernetes RBAC alone cannot solve. When multiple agents share infrastructure — including hardware resources, network paths, data stores, and agent-to-agent communication channels — boundaries must be enforced at both the identity and execution layers.


The CNCF agentic standards remind us that “every granted permission will eventually be used.” Citing the[Principle of Least Privilege](https://goteleport.com/learn/principle-of-least-privilege/) , it recommends just-in-time access provisioning, in which agents request short-lived permissions for a specific task that expire when the task completes. This is the alternative to just-in-case permissions, which can be dangerous when agents explore options using whatever tools they can access.


Beyond JIT provisioning, fine-grained tokens for each tool, using OAuth2 scopes, can prevent agents from accumulating excessive permissions. Attribute-based and policy-based access control allow permissions to reflect actual task context rather than static role assignments. For isolation, the article recommends namespace separation, container isolation, network segmentation, and hardware partitioning for agents that operate across different trust boundaries, with service mesh capabilities enforcing secure communication between them.


## Agent data access


Agents interact with a range of data stores across multiple tenants. RAG databases, vector stores, and task-specific databases can all be points of vulnerability if access is not tightly bound. The CNCF article treats data access as its own attack surface rather than an extension of the identity concerns addressed elsewhere.


The recommendations cover four areas.


1. Access to data stores must be limited to what the task actually requires. When multiple agents share a common store, like a RAG database, query-level access controls prevent one agent from reading data intended for another.
2. Prompt injection and jailbreaking require active defenses through input validation, context-aware prompt guardrails, and anomaly detection on agent behavior. Learn more about[preventing prompt injection in our recent blog.](https://goteleport.com/blog/prevent-prompt-injection/)
3. Tool invocation must be restricted to pre-approved interfaces with full audit trails on every call, since the attack vector is not only the data but also the tool that touches it.
4. Internal APIs exposed to agents should follow zero trust principles with minimal surface area, segregation by agent role or task, and[mTLS](https://goteleport.com/docs/machine-workload-identity/use-cases/workload-mwi/) for all inter-service and agent-tool communication.


For the execution environment itself, the CNCF recommends hardware-based isolation through Trusted Execution Environments (TEEs) and GPU confidential computing for agents running on shared infrastructure, with explicit controls to prevent system prompt leakage through user-facing APIs or observability tooling.


Teleport's[MCP Access Controls](https://goteleport.com/docs/enroll-resources/mcp-access/rbac/) provide tool discovery, provenance tracking, and access control for agent tool interactions. Session recording and interactive controls give engineering and security teams a full audit trail of agent activity, and identity chain visibility connects every action back to the agent, the human who authorized it, and the short-lived privileges in effect at the time.


## From containers to agents


In its[cloud-native agentic standards](https://www.cncf.io/blog/2026/03/23/cloud-native-agentic-standards/) , the CNCF AI Technical Community Group establishes best practices for securing both containers and agents. These best practices will continue to evolve as protocols mature and production deployments surface new requirements.


The[Teleport Agentic Identity Framework](https://goteleport.com/platform/agentic-identity-framework/) is designed to provide a standards-driven security architecture and reference implementations for securely deploying AI agents across infrastructure, including container, cloud, and on-prem environments. It provides the security controls and integration patterns to give agents a verifiable identity and least-privilege access, with every action authorized by policy and attributable to the human or workload that initiated it.


Read the full CNCF article,[Cloud-native agentic standards](https://www.cncf.io/blog/2026/03/23/cloud-native-agentic-standards/) , on the CNCF website.


---


## Boris Kurktchiev


Boris Kurktchiev is known for his expertise in zero trust identity solutions for cloud and AI, and his contributions to the CNCF's Cloud Native AI working group. A thought leader and KubeCon presenter, he advocates for AI in security, developing solutions like automated Kubernetes policy enforcement and ML for identity governance, and advises enterprises on their AI strategies.
