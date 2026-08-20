---
schema_version: "1.0.0"
document_id: "0354ee5ab1a01a8491ec9ae52972c4b71fd854d421a718bf3c8db4c564fbad5d"
company_key: "yc-sweetspot"
company: "Sweetspot"
source_id: "yc-sweetspot-news-import-95ae5efddaf0"
canonical_url: "https://www.sweetspot.so/articles/zero-static-credentials/"
published_at: "2026-02-20T00:00:00+00:00"
first_seen_at: "2026-07-24T03:25:11.786592+00:00"
fetched_at: "2026-07-28T22:19:36.583779+00:00"
content_hash: "sha256:bdab08dd195b25d5bc7d623e1b4890fd24f6cf2e93e0cd6f681ded4734e7afeb"
---

# Zero Static Credentials: What Passwordless Infrastructure Looks Like in Practice

Static credentials are the most common root cause of cloud breaches. Leaked API keys. Hardcoded database passwords. Long-lived service account tokens committed to source control. Shared secrets that were “temporary” three years ago and now have access to everything.


CMMC Level 2 controls AC.L2-3.1.1 (limit system access) and IA.L2-3.5.2 (authenticate devices and users) demand better. “Better” isn’t stronger passwords or more frequent rotation. It’s eliminating static credentials entirely, everywhere the architecture allows it.


This is what that looks like in practice for a cloud-native AI platform.


## AI Model Authentication


This is the highest-stakes credential in an AI platform. Every inference call carries potentially sensitive data: prompts built from CUI, document contents, user queries. The credential that authenticates these calls must be tightly scoped, short-lived, and auditable.


The path of least resistance is an API key. Every cloud AI provider offers them: generate a key, store it somewhere, reference it at runtime. API keys have three problems that compound over time.


- They’re static. The same credential is used for every request, from every service, for the duration of its lifetime. If it’s compromised, the blast radius is total until someone rotates it.
- They’re shared. Multiple services often use the same key, making it impossible to attribute usage to a specific workload from the key alone.
- They’re stored. They have to live somewhere (an environment variable, a secrets manager, a config file), and each storage location is an attack surface.


Workload identity federation eliminates all three problems. Each service instance authenticates using a short-lived token issued by the orchestration platform (a projected service account token in Kubernetes, for example). This token is exchanged with the cloud provider’s identity service for a scoped credential that expires automatically.


Short-lived credentials expire in minutes to hours, not months or years. A compromised token has a narrow window of utility. Scoped credentials give each workload its own identity with its own permissions. Your document indexing service can invoke embedding models but can’t deploy new ones. Your chat service can invoke LLMs but can’t access storage buckets. And every model invocation is attributable to a specific workload identity, not a shared API key.


Setting this up across multiple cloud AI providers (Azure AI Foundry, Google Vertex AI, AWS Bedrock), each with their own federation protocol, is significant engineering work. The security posture improvement is worth it: zero static credentials for the most sensitive operations in your platform.


## Cloud Service Authentication


The same pattern extends to every cloud service your platform consumes. Storage, databases, message queues, key management: each can be accessed through workload identity rather than static credentials.


On AWS, this means IAM Roles for Service Accounts (IRSA) or EKS Pod Identity. On GCP, Workload Identity. On Azure, Azure Workload Identity. The mechanism differs, but the principle is the same: pods authenticate using their Kubernetes service account, which is federated to the cloud provider’s IAM system via OIDC.


Each pod assumes only the permissions it needs. A pod that writes to a storage bucket can’t read from a database. A pod that invokes an LLM can’t modify infrastructure. The blast radius of any compromised workload is limited to exactly its granted permissions, which, if you’ve done least privilege correctly, is the minimum set required for its function.


## The Secrets That Must Exist


Not every credential can be eliminated. Database passwords, third-party API keys for external integrations, OAuth client secrets: some static credentials are unavoidable because the services they authenticate to don’t support federated identity.


For these, the approach is defense in depth.


- Cloud-native secrets management meeting FedRAMP® and SOC 2 security standards, with access controls, audit logging, and automatic rotation where supported.
- Operator-based synchronization that syncs secrets from the cloud secrets manager into the orchestration platform. Secrets are never stored in source control, environment variable definitions, or deployment manifests.
- Short rotation cycles for any credential that supports rotation. Automated, not manual. If a human has to remember to rotate a credential, it won’t happen on time.
- Encryption at rest for any credential that must be stored, using managed key services with automatic key rotation on top of the secrets manager’s own encryption.


The goal isn’t zero secrets. It’s zero unnecessary secrets, and defense in depth for the rest.


## User Authentication


Users are the other half of the credential equation. For a CMMC-compliant platform, user authentication has three requirements.


- Server-enforced session management. The server tracks session activity independently and forces re-authentication after a period of inactivity. Client-side session timeouts (JavaScript timers) are a UX convenience, not a security control. They’re trivially bypassed.
- Token validation on every request. Authentication tokens are validated server-side on every API call, not just on initial login. Revoked or expired sessions are caught immediately, not on the next page refresh.
- Internal MFA for all personnel. Engineers, operators, and anyone with access to infrastructure or production systems authenticate through a centralized identity provider with MFA enforced universally.


This creates a clean separation: user sessions are managed and enforced server-side with short inactivity windows, and internal access to production systems requires multi-factor authentication through a centralized provider.


## Fine-Grained Authorization


Authentication answers “who are you?” Authorization answers “what can you do?”


For AI platforms handling CUI, authorization must be fine-grained and evaluated on every operation. When a user asks an AI assistant to analyze a set of documents, the platform must verify in real time that the user has access to every document that will feed the model’s response. Not at the project level. At the individual resource level.


This requires an authorization system that supports:


- Hierarchical permissions. Access cascades through organizational structures (organization, project, resource) with proper inheritance.
- Attribute-based policies. Permissions based on user attributes, resource attributes, and the relationship between them.
- Consistency guarantees. When a permission is revoked, the revocation is reflected immediately in subsequent requests from the same session. In a distributed system, this requires explicit consistency mechanisms, not eventual consistency with an undefined propagation window.


Flat RBAC (three roles: admin, editor, viewer) doesn’t meet this bar. When your platform handles thousands of documents across dozens of projects for organizations with complex access hierarchies, the authorization model must match that complexity.


## The Full Credential Architecture


The credential architecture of a CMMC-compliant AI platform should look like this:


Credential Type Approach


AI model access Workload identity federation. Short-lived tokens. Per-service scoping.


Cloud service access Workload identity federation. Pod-level IAM. Least privilege.


Database / third-party credentials Cloud-native secrets management. Operator sync. Automatic rotation.


User authentication Server-enforced sessions. Token validation on every request. Internal MFA.


Authorization Fine-grained, attribute-based, hierarchically inherited. Real-time evaluation.


Every credential is either eliminated (replaced with federation) or defended in depth (managed, rotated, encrypted, audited). No static API keys for AI models. No shared service account passwords. No long-lived tokens with broad permissions.


---


At Sweetspot, we audited every credential in our system and asked: can this be replaced with a short-lived federated token? For the vast majority, the answer was yes. For the rest, we use managed secrets infrastructure with automatic rotation and application-level encryption.


The result: zero static API keys for AI model access across all three cloud providers we integrate with. Zero long-lived tokens for cloud service authentication. Server-enforced sessions for every user. Fine-grained authorization on every resource.


This runs in production today, serving defense contractors who use GPT-5, Claude Opus 4.6, and Gemini 3.1 Pro through federated credentials that expire in under an hour.
