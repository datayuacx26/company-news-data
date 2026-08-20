---
schema_version: "1.0.0"
document_id: "dc70235270d1b5fdf7e430e26dcdf19dcdb1b7ca267e701756c55140baf34b40"
company_key: "yc-sweetspot"
company: "Sweetspot"
source_id: "yc-sweetspot-news-import-95ae5efddaf0"
canonical_url: "https://www.sweetspot.so/articles/cmmc-level-2-age-of-ai/"
published_at: "2026-02-20T00:00:00+00:00"
first_seen_at: "2026-07-24T03:25:11.786592+00:00"
fetched_at: "2026-07-28T22:19:31.715486+00:00"
content_hash: "sha256:12467e6df6c3179baae4c443da2f63ec958a5a5ed6c044ddcac43dd135787ae9"
---

# CMMC Level 2 in the Age of AI: Which Controls Apply to LLM-Powered Platforms

CMMC Level 2 maps to 110 controls from NIST SP 800-171. These controls were written to protect Controlled Unclassified Information in defense contractor environments: file shares, email servers, endpoint workstations.


They were not written for platforms where large language models synthesize information across thousands of documents in real time.


That doesn’t make them irrelevant. Applying them requires thinking from first principles about where data flows in an AI-powered system, and the answer is everywhere. An LLM-backed platform touches storage, compute, networking, identity, and authorization in ways that a traditional document management system never did. Every inference call is a data flow that could disclose CUI, and every prompt is an access decision.


Six control families carry the most weight for AI platforms.


Control Family AI Platform Concern Key Question


**AC** — Access Control Does the AI only reference documents the user is authorized to see? Can your authorization model handle per-document permissions in real time?


**SC** — System & Comms Protection Are AI inference calls staying inside a FedRAMP® boundary? Is every cryptographic operation using FIPS-validated modules?


**IA** — Identification & Auth Are service-to-service credentials short-lived and scoped? Do any static API keys exist for AI model access?


**CM** — Configuration Management Are container images immutable and verified? Can a node image be modified after deployment?


**MP** — Media Protection Are embedding vectors and cached AI responses encrypted? Is every storage volume, including ephemeral, encrypted at rest?


**SI** — System & Info Integrity Are container images scanned before deployment? Could a scanned image be silently replaced with a compromised one?


## AC: Access Control


Controls AC.L2-3.1.1 through AC.L2-3.1.22 cover limiting system access to authorized users, controlling information flow, enforcing separation of duties, and implementing least privilege. For AI platforms, these controls carry the most weight.


When a user asks an AI assistant to summarize a set of documents, the platform must verify in real time that the user has access to every document the model will reference. Not at the collection level. At the individual resource level.


Flat role-based access control doesn’t cut it. A user might have access to a pursuit but not to a specific file within it. A team member might have read access but not write. An AI-generated response might reference information the requesting user shouldn’t see.


This requires fine-grained, attribute-based authorization that evaluates permissions hierarchically, checking access at the organization, project, and individual resource level before any data reaches the model. Permissions must cascade through organizational hierarchies with proper inheritance, not just check a single role assignment.


The authorization system also needs consistency guarantees. In a distributed system, there’s a window between when a permission is revoked and when all nodes reflect that change. For CUI, that window needs to be as close to zero as possible. Read-your-writes consistency, where a permission change is immediately reflected in subsequent requests from the same session, is the minimum acceptable standard.


## SC: System and Communications Protection


Controls SC.L2-3.13.1 through SC.L2-3.13.16 cover monitoring and controlling communications at system boundaries, employing architectural designs and cryptographic mechanisms to protect confidentiality.


Every AI inference call is a network request carrying potentially sensitive data. If your platform sends prompts containing CUI to a model endpoint outside a FedRAMP authorization boundary, you have a data flow problem that no amount of TLS will fix.


All AI model access must route through FedRAMP-authorized cloud providers, not commercial API endpoints. The consumer AI products most people use daily (ChatGPT, Claude.ai, Gemini) send data to these commercial endpoints and are not FedRAMP authorized. For CUI workloads, you need the same models accessed through FedRAMP-authorized infrastructure. The good news for CMMC contractors: FedRAMP Moderate is the requirement, and the latest frontier models (GPT-5, Claude Opus 4.6, Gemini 3.1 Pro) are all available on commercial cloud infrastructure within FedRAMP boundaries. A secure, CMMC-compliant alternative to ChatGPT doesn’t mean worse AI. It means the same models, accessed through the right infrastructure. The picture is different for ITAR. We wrote about the CMMC vs. ITAR model availability gap in[FedRAMP AI Models: How to Access GPT-5, Claude Opus 4.6, and Gemini 3.1 Pro Without Leaving a FedRAMP Boundary](https://www.sweetspot.so/articles/fedramp-ai-models/) .


Beyond model access, FIPS-validated cryptography must be enforced at every layer, not just the load balancer. Node operating systems, service-to-service communication, container image pulls, database connections, and storage encryption all need to use FIPS-validated cryptographic modules. A single layer using non-validated cryptography creates a gap in the chain. We wrote about this in[FIPS 140 Compliance Is an Architecture Decision](https://www.sweetspot.so/articles/fips-140-architecture/) .


All compute should run in private network segments with no direct public internet access. External service integrations should use private connectivity so data never traverses the public internet, even encrypted.


## IA: Identification and Authentication


Controls IA.L2-3.5.1 through IA.L2-3.5.11 cover identifying and authenticating users, devices, and services, including multi-factor authentication and replay-resistant protocols.


There are two sides to identity in a platform like this: the users accessing the platform, and the services running inside it.


For user-facing authentication, the fundamentals matter more than the features list. Session management must be server-enforced, meaning the server tracks session activity independently and forces re-authentication after a period of inactivity, regardless of what the client reports. Client-side session timeouts (JavaScript timers that redirect to a login page) are trivially bypassed and don’t count as a security control. Authentication tokens must be validated on every request, not just on initial login.


For service-to-service authentication, static credentials are unacceptable at scale. Workload identity federation, where each service authenticates using short-lived, automatically rotated tokens issued by the orchestration platform, eliminates the credential rotation burden and reduces blast radius to near zero. CMMC’s IA controls have the most impact on platform architecture at this layer. We cover this in[Zero Static Credentials: What Passwordless Infrastructure Looks Like in Practice](https://www.sweetspot.so/articles/zero-static-credentials/) .


For internal personnel (engineers, operators, anyone with access to infrastructure or production systems), MFA must be enforced universally through a centralized identity provider. This keeps the MFA implementation in one place, auditable through one set of logs, and upgradeable without touching application code.


## CM: Configuration Management


Controls CM.L2-3.4.1 through CM.L2-3.4.9 cover establishing and enforcing security configuration settings, tracking and controlling changes, and restricting nonessential functionality.


All infrastructure must be defined as code, reviewed through pull requests, and applied through automated pipelines. No console-driven changes, no SSH-and-edit, no manual configuration drift. Every production change should be traceable to a specific commit by a specific person.


Container images must be immutable and pinned by content digest (SHA256), not by mutable tags. A tag like` v1.2.3` can be overwritten to point to different content. A digest cannot. This is a supply chain integrity guarantee that most platforms skip.


The node operating system itself should be immutable, purpose-built for running containers, with no shell, no package manager, and no mechanism for runtime modification. If the OS needs to change, you build a new image and replace the node. This eliminates entire categories of configuration drift and post-exploitation persistence.


Automated image rebuild pipelines should run on a regular cadence (weekly, at minimum), pulling in the latest security patches and verifying that security controls like FIPS mode are correctly enabled before the image is promoted.


## MP: Media Protection


Controls MP.L2-3.8.1 through MP.L2-3.8.9 cover protecting system media (physical and digital), sanitizing media before disposal, and controlling access to CUI on media.


Every storage volume, both OS partitions and data partitions on every node class, must be encrypted at rest using cloud-managed keys with automatic rotation. This includes ephemeral storage on compute nodes, not just persistent database volumes.


Database encryption at rest is table stakes, but it needs to be verified, not assumed. Managed database services usually offer encryption as a checkbox, but the key management, rotation policy, and access controls around those keys matter.


Object storage must enforce HTTPS-only access policies. A permissive bucket that allows unencrypted reads is a data exfiltration path, regardless of what your application layer does.


For AI platforms specifically, consider where model context and intermediate results are stored. If your platform caches prompt-response pairs, those caches contain CUI and need the same encryption and access control treatment as primary storage. The same applies to embedding vectors. Even though they’re derived representations rather than raw text, they’re generated from CUI and must be treated accordingly.


## SI: System and Information Integrity


Controls SI.L2-3.14.1 through SI.L2-3.14.7 cover identifying and correcting system flaws, providing protection from malicious code, and monitoring system security alerts.


Every container image must be scanned for known vulnerabilities at push time, before it enters your registry, not after it’s running in production. Combined with immutable tags, this ensures that a scanned-and-approved image cannot be silently replaced.


Node images should go through the same pipeline: build, scan, verify security controls, then promote. Automated and on a regular cadence, so you’re never more than a week behind on OS-level patches.


For AI-specific integrity concerns, the platform needs guardrails around model behavior. AI responses must not leak information across authorization boundaries. Prompt injection attacks must be mitigated. Model outputs must be attributable to specific source documents so users can verify accuracy.


---


CMMC wasn’t designed for AI platforms, but its controls are more relevant than ever. AI amplifies every existing data flow concern: more data moves through more services, faster, with more complex access patterns. Getting compliance right requires treating every inference call as a controlled data flow, every AI response as a potential disclosure, and every model endpoint as a system boundary.


For CMMC contractors, compliance doesn’t have to mean compromise. The latest frontier models are available today within FedRAMP-authorized boundaries. The question is whether your platform was architected for compliance from the ground up, or whether compliance was bolted on after the fact.


We built Sweetspot so your team doesn’t have to build all of this from scratch. We’ve done the architectural work across multiple cloud providers, at every layer of the stack, so you can focus on winning contracts.
