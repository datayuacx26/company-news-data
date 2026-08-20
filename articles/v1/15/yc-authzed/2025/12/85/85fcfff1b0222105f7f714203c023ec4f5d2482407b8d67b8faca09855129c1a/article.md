---
schema_version: "1.0.0"
document_id: "85fcfff1b0222105f7f714203c023ec4f5d2482407b8d67b8faca09855129c1a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/agentic-ai-is-not-secure"
published_at: "2025-12-23T15:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:5c3bec5131453cf1821695aebb25a298fcf67e5b0cc7e5f8ec97ccf5ff24ac22"
---

# Agentic AI is not Secure

*Note: This post draws on insights from two academic security analyses of agent communication protocols. References are included below for readers who want to dive deeper.*


AI agents are being adopted across enterprise environments. Organizations are deploying them to automate workflows, manage infrastructure, and handle decision-making processes.


As opposed to traditional software, which follows predefined, static rules, AI agents are autonomous. They make decisions, adapt to changing conditions, delegate work to other agents, and coordinate actions over time. That autonomy fundamentally changes the security model and introduces risks that traditional systems were never designed to handle.


Over the past year, several protocols have emerged that aim to standardize how agents interact with tools and with each other. However, while communication is being standardized, authorization remains largely undefined. This gap is becoming one of the primary blockers to deploying agentic AI safely at scale.


## **Agent Communication Protocols**


Two key protocols have emerged as standards for agent communication: the Model Context Protocol (MCP), and the Agent-to-Agent (A2A).


MCP defines how agents interact with tools. A2A focuses on how agents interact with other agents.


The Agent Communication Protocol (ACP) emerged this year as well and explored a similar problem space as the A2A protocol. Since then, it has been folded into the A2A ecosystem under the Linux Foundation.


All protocols specify authentication mechanisms but none of them fully addresses protocol-specific security concerns. **Authorization is largely treated as an external concern** , delegated to generic token mechanisms rather than modeled explicitly.


MCP is not[secure](https://authzed.com/blog/mcp-is-not-secure) . Security in MCP is left to the implementers.


The same is true for A2A. According to the A2A[specification](https://a2a-protocol.org/latest/specification/) , the A2A protocol is an open standard designed to facilitate communication and interoperability between independent AI agent systems. Authorization and security is intentionally left as an["implementation-specific" decision](https://a2a-protocol.org/latest/topics/enterprise-ready/#authorization) .


A2A's architecture involves two agent roles: client agents that initiate requests and remote agents that fulfill them. Agents advertise their capabilities using a machine-readable AgentCard, which facilitates efficient discovery and delegation without trial-and-error probing. Tasks are delegated through structured messages that specify the intended action and its parameters. A1A supports both synchronous and asynchronous interactions, using JSON-RPC for request-response exchanges and event-based mechanisms for asynchronous updates.


The A2A specification does not mandate a specific authentication or authorization scheme. Implementations typically integrate with existing security infrastructure (such as OAuth 2.0, JWTs, or mTLS) and rely on encrypted transport (e.g., TLS) to secure communication.


ACP uses a REST-based standard for agent communication that follows a client–server model and is designed to scale in Kubernetes environments. Agents communicate over HTTP using structured messages. The protocol introduces an RBAC model with operation-specific JWTs, but there is no enforcement on the actual management of the tokens. Access Delegation is listed as an[area for future exploration](https://agentcommunicationprotocol.dev/about/whats-next#access-delegation) .


This design enables efficient collaboration between agents. What it does not define is how permissions should be scoped, reduced, or revoked as tasks move across agent boundaries.


Agentic systems introduce new authorization challenges that traditional models were never designed to handle:


- Agents act autonomously and continuously
- Agents can delegate to other agents
- Agents can operate across trust boundaries
- Permissions must change dynamically at runtime


Most implementations rely on standard OAuth scopes. OAuth is effective for human-to-service authorization with role-based permissions. It was never designed to express fine-grained, resource-level constraints that autonomous agents require.


OAuth can't handle "this agent can read document A, document C, and document D, but not document B." It lacks support for the dynamic, context-specific authorization decisions that AI agents make across distributed systems.


## **Insufficient Granularity in Authorization**


Standard OAuth scopes are too broad for agents. Agents use coarse-grained tokens that grant or deny access based on broad criteria like a user's role or blanket permissions across entire systems. For example, an agent can get Admin access to read and write all files in a drive, while, in reality we need Fine-Grained Access Control tokens that say, "This agent can modify File X in Directory Y for the next 5 minutes". This approach respects the principle of least privilege. Without it, systems are vulnerable to what's classified as[Insufficient Granularity of Access Control](https://cwe.mitre.org/data/definitions/1220.html) .


This vulnerability is clearly demonstrated in the A2A protocol. Its token model relies on broad JSON-RPC scope definitions without nested hierarchy enforcement. A single delegation token can cover unrelated API endpoints. ACP explored a more explicit approach to agent communication. It introduced RBAC and operation-specific JWTs in an attempt to improve security. This was a step in the right direction, but only on the surface. RBAC + JWT is only useful if the token scopes are verified at the right granularity. That is, each permission should represent only what the agent needs to do, and permission should not be reused or shared across unrelated operations or resources.


Access tokens should be issued with granular scopes per task and context, not broad, task-unspecific permissions. Poorly defined scopes bundle unrelated privileges into a single authorization token. This expands the impact of compromise and increases the attack surface.


Broad tokens are not only a security risk, but also violate data-minimization requirements under regulations like GDPR. Privacy laws are built on the idea that systems should only access the data they actually need, and only for a specific purpose. Broad tokens break that model by granting access to far more data than a task requires, even if that extra access is never used.


## **Token Lifetimes and Replay Attacks**


Granularity is only part of the problem. Token lifetimes matter just as much.


The A2A specification does not mandate a particular token format or expiration policy, so implementations often rely on bearer tokens with lifetimes defined by the underlying platform. If these tokens are long-lived and not bound to a specific session or context, intercepted credentials can be replayed to impersonate an agent after the original task completes.


A comparative security study notes that ACP "exhibits partial exposure. Although short-lived tokens are recommended in its RBAC guidelines, enforcement is optional, enabling replay in extended sessions lacking JWS timestamps."


## **Privilege Persistence**


In multi-agent environments, authorization changes must propagate across distributed systems. Today, they often do not.


Revoked or outdated permissions can persist due to asynchronous updates, cached authorization state or incomplete propagation of revocation signals.


This condition aligns with[CWE-284](https://cwe.mitre.org/data/definitions/284.html) (Improper Access Control), in which access restrictions are incorrectly enforced and permissions that should no longer apply continue to be honored, allowing unauthorized actors to retain access.


The A2A protocol is particularly exposed to this class of failure. Authorization state may persist in peer caches, and updates to AgentCards or related manifests are not guaranteed to propagate synchronously to all participants. Over time, this can lead to version drift, where revocation events are applied inconsistently across the system. Because A2A relies on asynchronous communication and does not define a mechanism for global state reconciliation or coordinated cache invalidation, authorization decisions may be evaluated against stale policy state. In distributed authorization systems, checks must respect the causal ordering of access control changes and resource updates. When this ordering is not enforced, systems risk applying outdated ACLs to newly created or modified content, a class of failure known as the["new enemy" problem](https://authzed.com/docs/spicedb/concepts/zanzibar#new-enemy-problem) .


## **Lack of Transparency and User Consent**


Transparency and user consent depend on explicit, informed approval for each instance of data exchange between agents. This aligns with[CWE-200: Exposure of Sensitive Information](https://cwe.mitre.org/data/definitions/200.html) and is necessary to ensure that disclosures in multi-agent workflows remain purpose-limited, revocable, and auditable throughout the full lifecycle of data propagation. In agentic systems, this enforcement typically appears as user-facing consent prompts that authorize specific operations. When such enforcement is missing, agents operate under implicit consent inherited from earlier delegations, with no opportunity for review or intervention. The A2A protocol itself does not include a standardized mechanism for user consent. As a result, data may be transferred through agent handoffs without explicit user awareness, allowing information to propagate beyond its originally intended scope.


## **Consent Fatigue and Recursive Delegation**


When user-facing approval mechanisms do exist in agentic systems, they are implemented at higher layers and can lead to consent fatigue over time. Users may approve permission requests without fully understanding their effective scope, particularly when permissions are bundled or abstracted. When agents act on behalf of users, this increases the risk of over-authorization. An API token that requests "read access" may also include write permissions. For example, an agent viewing on-call schedules and incidents might also have permissions to change on-call assignments. Bundling unrelated privileges obscures the true scope of access and undermines the principle of least privilege.


Recursive delegation amplifies this problem. When agents spawn sub-agents, they create complex authorization chains without clear scope attenuation mechanisms. Each delegation in the chain increases the attack surface and makes it increasingly difficult to determine which actor is responsible for a given action.


## **Requirements for Secure Agent Infrastructure**


Agentic systems require specific security improvements:


**Token Granularity** : Tokens should grant minimal permissions required for specific tasks rather than broad administrative access.


**Revocation Propagation** : Revocation mechanisms must synchronize across distributed systems to prevent orphaned tokens.


**Clear Consent Flows** : Permission requests should clearly communicate actual scope to users, not bundle unrelated privileges.


**Scope Attenuation** : Delegation chains need automatic scope reduction to limit cascading permissions.


MCP defines how tools are exposed. A2A defines how agents communicate. Both protocol specifications recommend to follow Authorization best practices but don't enforce them. A centralized authorization layer can define what agents are allowed to do, under what conditions, and for how long. Without that layer, agentic systems will remain powerful but unsafe.


References:


- [https://arxiv.org/pdf/2505.12490](https://arxiv.org/pdf/2505.12490)
- [https://arxiv.org/pdf/2511.03841v1](https://arxiv.org/pdf/2511.03841v1)


On this page


- Agent Communication Protocols
- Insufficient Granularity in Authorization
- Token Lifetimes and Replay Attacks
- Privilege Persistence
- Lack of Transparency and User Consent
- Consent Fatigue and Recursive Delegation
- Requirements for Secure Agent Infrastructure


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. Melissa Smolensky · May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)
