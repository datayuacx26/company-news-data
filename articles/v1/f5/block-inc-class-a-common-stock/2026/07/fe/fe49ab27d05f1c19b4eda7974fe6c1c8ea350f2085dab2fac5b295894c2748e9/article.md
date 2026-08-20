---
schema_version: "1.0.0"
document_id: "fe49ab27d05f1c19b4eda7974fe6c1c8ea350f2085dab2fac5b295894c2748e9"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/whos-asking-identity-delegation-for-ai-agents-and-service-meshes"
published_at: "2026-07-06T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:97c506f5119824e7a70fef68ad66f9e9e2623ddf48ddf5097f305fd248cda0f0"
---

# Who's Asking? Identity Delegation for AI Agents and Service Meshes

When a request reaches a service, it should be able to answer two questions: who are you, and are you allowed to do this?


In a small system, that answer is mostly straightforward. A human signs in through their corporate identity provider, clicks a button to take an action, and a server consults a written policy: is this person allowed to do that action on this resource? The sign-in answers the first question, and the service-owned policy answers the second. We make two assumptions: that the human's identity was verified before onboarding, and the policy came from a trusted source, which is true for services owning their own policies.


Then we add modern software engineering.


An engineer responding to an incident might ask an agent, "What's going on?" The agent may check telemetry, review recent deploys, search Slack or email, open a change, and deploy a fix. The harness might run locally or in a shared cloud environment. Every system it touches still needs to answer the same questions: who is asking, and what are they allowed to do?


If the agent runs as a shared service or harness, many people can send work through the same underlying compute. In that model, we cannot rely only on the agent's backend service identity.


That service identity would need enough capabilities to act for everyone. Future users could then inherit those capabilities through agent interactions. That weakens protection against data leakage and unauthorized access, and moves us further away from least-privileged access.


If we flip the narrative and have the agent service simply assume the human identity, we have a different problem. We lose control over how systems with different security properties handle potentially sensitive data. Perhaps your telemetry stack contains data you haven't authorized for consumption by a third-party model provider, or users don't feel comfortable providing access to their Slack history. Similar to the agent service identity case, context and how we handle it matters.


Both implementations only half answer the "who" of it all. The "are you allowed?" question is muddled because "allowed" depends on a clear "who" taking the action. At Block, we designed a system to give us deeper insight into that "who" so we can better answer whether an action should be allowed. We call it User Identity Delegation.


User Identity Delegation gives downstream services a better answer than "the user" or "the agent." It lets a service evaluate: this user, through this software actor acting on their behalf, with this bounded delegated authority, for this request.


To understand why User Identity Delegation is necessary, we have to rethink how we talk about software acting on behalf of people.


## Intentional Anthropomorphism


Central to our work around User Identity Delegation is the same question that was our guidepost before widespread agent use: how do we ensure that the right people get access to the right data at the right time in the right place?


Agentic AI is not unique in its risk. Agents are units of computation that execute some operation and are called to action by a human or another service. What is unique about agentic adoption is the speed at which access can escalate. Least-privileged access is already a perennial struggle, and agentic velocity makes permission creep easier. Where a command line tool or an application is bounded by its design parameters, agents are intentionally more open-ended. When an agent receives a task and the appropriate authorizations, it will find a way to complete that task, potentially with undesirable side effects.


The agent may be enthusiastic, but that enthusiasm is precisely why it cannot be the authorization model.


Agents are also naturally seen as extensions of their users. People expect that agents should be able to do all of the same work that they can do, just faster or in parallel. This is, after all, their value proposition. From this model, delegation naturally emerges: a user delegates a task to their agent, and the agent acts on behalf of that user.


In the same way that you might delegate the delivery of your pizza to your local pizza delivery driver, you might delegate a task to your agent.


When you place the order, the driver does not become you. They get limited authority to do specific work on your behalf: pick up this pizza, use this delivery address, and complete the delivery during a bounded window. Different systems can evaluate different parts of that delegation.


At the pizza shop, the decision is about pickup:


- **Subject:** you, the person who placed the order
- **Actor:** the driver
- **Capability:** pick up
- **Resource:** pizza order
- **Delegated authority:** pick up the order you placed
- **Decision point:** the shop counter


At the address lookup or delivery step, the decision is different:


- **Subject:** you, the person receiving the delivery
- **Actor:** the driver
- **Capability:** use delivery address
- **Resource:** your address
- **Delegated authority:** use the address only to complete this delivery
- **Decision point:** address lookup or doorstep delivery


These boundaries are what make pizza delivery feel reasonable. The driver can act for you because you initiated the order. Their authority applies only to this delivery, only for the address needed to complete it, and only during the delivery window. They have delegated authority, not your full identity.


You do not expect the driver to spontaneously deliver you a pizza when you did not ask for it, and you do not expect them to use your address to sign you up for cat fact mailers.


User Identity Delegation brings that same shape into software. It preserves who is being represented, what software is acting, what authority was delegated, and whether the requested action is allowed now.


The vocabulary we use for this is:


- **Subject:** the user whose authority is involved.
- **Actor:** the workload, application, CLI, or service harness doing the work.
- **Capability:** a named permission that can be evaluated by a downstream service.
- **Delegated authority:** the subset of authority the actor is allowed to use.
- **Decision point:** the service boundary where the request is evaluated.


## What We Built


User Identity Delegation has four components:


Component Job


Edge user identity issuer Gets trusted user context into the system at the security boundary.


Token exchange Turns short-lived user context into delegated tokens for work that outlives one request.


Consent control plane Records what authority a user agreed to delegate to a specific actor, separate from the issuer because consent is user intent rather than authentication.


Authorization data plane Validates tokens at decision time and resolves the effective delegated capability set.


These pieces exist because neither "the service is calling" nor "the user is calling" is enough context for delegated work. The user has to stay visible, because hiding the user behind a service identity pushes systems toward broad standing access. The actor also has to stay visible, because hiding software behind a user identity makes auditing weaker and makes it harder for downstream services to apply different policy to direct user actions and delegated software actions.


The easiest way to see how the components fit together is to follow one request: an internal CLI or agent-backed tool calling a downstream service on behalf of an engineer.


## One Request Through User Identity Delegation


Return to the incident response flow from the opening. An engineer asks an agent-backed tool "What's going on?" The tool needs to read telemetry, search recent deploys, inspect service ownership, and eventually call a downstream service that returns data based on the engineer's permissions.


The interesting moment is not the engineer's first prompt; it is the downstream service call after the tool has gathered context and needs data scoped to the engineer's permissions.


That downstream service should not just see "the agent service" and it should not just see "the engineer" with no actor context. It should see this engineer, through this software actor acting on their behalf, with bounded authority, for this specific request.


The request looks roughly like this:


Figure 1: A synchronous request carries the signed user identity JWT to the downstream service, which validates it with the authorization data plane before applying its own policy.


The important part is not that every request has this exact shape. The important part is that User Identity Delegation keeps the subject and actor visible at the service boundary, and gives the downstream service a capability set constrained by both current authority and delegated consent. This synchronous path is the first step toward that: it carries verified user context so the service sees the actual user and their current capabilities, while actor attribution and consent clamping arrive with the token exchange.


## A Request Starts With Verified User Context


The engineer starts from an authenticated environment, and at our security boundary the issuer mints a short-lived, signed[JWT](https://datatracker.ietf.org/doc/html/rfc7519) containing verified user context from that session.


The token can be forwarded through service calls and verified later. A downstream service can verify the first identity in the flow from a trusted cryptographic credential, not just from a caller saying "trust me, this is really Alice."


For ordinary synchronous calls, that signed identity can travel with the request. The agent-backed tool might call a service that needs to know the user's current capabilities before returning data. The service mesh asks the authorization data plane to validate the token and resolve the user's current capabilities.


The downstream service still owns the final authorization decision: whether this user can view this resource, update it, trigger the operation, or violate some highly opinionated NoPineapplesOnPizzaOrders policy. User Identity Delegation does not decide every service's policy. It gives that service verified user context to evaluate its own policy.


## Token Exchange Preserves Both Identities


Some delegated work does not fit into one short request. An agent may need to continue after the original session has ended, poll for status, update a ticket, prepare a pull request, or perform follow-up work. That work may happen after the original signed-in request has ended, and the software actor doing the work should still act on behalf of the user that started the automation.


That is where the token exchange comes in.


Figure 2: For work that outlives one request, the token exchange converts the user identity JWT into a delegated token with actor attribution, bounded by the user's consent.


The token exchange turns the short-lived user identity token into delegated authority with actor attribution. Depending on the shape of the caller, that exchange can use standard OAuth flows such as[OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693) for service-to-service flows,[Authorization Code with PKCE](https://datatracker.ietf.org/doc/html/rfc7636) for user-facing apps,[Device Authorization](https://datatracker.ietf.org/doc/html/rfc8628) for CLIs and headless tools, and[refresh tokens](https://datatracker.ietf.org/doc/html/rfc6749#section-6) for renewal. It's worth noting that token renewal refreshes require capability and consent validation before completion.


The actor is not an LLM deciding to work on its own. It is the workload, application, CLI, or service harness executing the delegated task, tied back to the user-originated command.


The shape is intentionally familiar: OAuth 2.0 Token Exchange gives us the language of exchanging one token for another while preserving delegation context. User Identity Delegation uses that pattern in Block's internal environment, where the exchanged token preserves the user identity and records the actor in an` act` claim anchored in workload identity. In other words, the token can say: this is the user, and this is the software acting on their behalf. That distinction is what makes both auditing and consent enforcement possible.


## Consent Narrows What Can Be Used


The consent control plane records what the user agreed to delegate to that software actor. In the incident response flow, the user might allow the agent-backed tool to read build status, inspect service metadata, or create a draft change, while withholding permission to deploy to production or read unrelated data.


This is where the distinction between authority and delegation matters. The control plane is deliberately not the source of truth for what the user can do. Our existing capability grant system remains that. The control plane records the user's intent: which capabilities the user agreed to delegate to this particular actor. It also gives the user a place to view and revoke active delegations.


Consent does not create authority. It only narrows authority that already exists and an agent cannot delegate authority they do not possess. That boundary is easy to blur in delegated systems, so User Identity Delegation keeps consent separate from current capability grants. We also restrict which capabilities can be granted and to which types of actors. Consents are only ever a strict subset of a user's capabilities.


For example, an incident-response actor might be delegated permission to read build status, inspect service metadata, and create draft changes. Deploying to production, reading unrelated Slack history, or accessing customer-impacting data can remain outside that delegation even if the user has some of those permissions in another context.


## The Data Plane Computes Effective Authority


The authorization data plane enforces all of this at decision time. This is the point where many systems have already lost the thread: the downstream service may not know a user existed at all. When the tool calls the downstream service, the service mesh asks the data plane to validate the token. The data plane verifies the JWT, checks audience and freshness, identifies both the user and the software actor, resolves the user's current capability grants, and computes **effective delegated capabilities** as the intersection of two sets:


Figure 3: Effective delegated capabilities is the intersection of what the user can do now and what they consented to delegate to this actor.


The intersection is the core security property.


If the user consented to a capability they no longer hold, it drops out because the grant no longer exists. If the user holds a capability but never consented to delegate it to this actor, it drops out because consent does not grant it. If the delegated token has no consent record at all, the effective set is empty. Authority and delegation must both be present.


The downstream service still owns its own authorization behavior. User Identity Delegation does not make every service's policy correct. What it provides is a better input: verified user context, actor attribution, and a capability set already clamped to both current authority and delegated consent. A service still has to decide whether the subject, actor, action, resource, and runtime context are acceptable for the operation being attempted.


The user, meanwhile, gets a revocation path. Revoking a consent record removes the published consent state used by downstream evaluation. A delegation system that cannot answer "is this still allowed right now?" is just a permission grant wearing a different hat.


With those pieces in place, we stop collapsing "the user" and "the tool" into one ambiguous identity for a growing class of flows.


## What User Identity Delegation Gives Us


The pieces above give User Identity Delegation four properties:


- Identity context is verified cryptographically.
- Authority is constrained by consent and current grants.
- Delegation can survive service and workload boundaries.
- The resulting action is auditable as subject plus actor rather than one ambiguous identity.


Now we can give a more precise answer to "who's asking?" In this flow, the answer is not just "the engineer" and not just "the agent." It is this engineer, through this software actor acting on their behalf, with this bounded authority, for this request.


From the user's perspective, the model is also easier to reason about: this request came from me, I delegated this actor to do a bounded set of work, and it cannot use more authority than I have right now or more authority than I agreed to delegate.


## Beyond Agents


We accelerated building this system as work with AI agents saw broader adoption. There was a clear need to answer the question posed above across the wide surface of AI users here at Block. But this is one piece of a broader story.


Any system that wants to make a request to another service where what is returned depends on what the initiator of the call has access to can use this model. That includes agent-backed tools, internal CLIs, remote build environments, cloud development environments, async jobs, and service-to-service workflows where downstream access depends on the original user.


The failure mode is not unique to AI. Broad service identities are tempting anywhere software needs to act for people. They are easy to wire up, but they are hard to reason about later. User Identity Delegation gives us a path that is still ergonomic for developers while preserving the context that downstream systems need.


## Where We Are Going


The destination is still the first-principles model, where **effective authority** is the intersection of every layer of context available at decision time. Each constraint can only narrow the set, never expand it:


Figure 4: Effective authority is the intersection of every layer of context available at decision time; each constraint can only narrow the set, never expand it.


We are still extending this model across more of our flows. What is already in place is useful on its own: signed user identity, delegated tokens with actor attribution, consented capability grants, downstream validation, capability intersection, and user-visible revocation.


What we are building toward is a richer runtime context: services reasoning not only about which user and which capabilities, but about the origin of the request, the workload doing the work, the action being attempted, the resource being touched, and the assurance level the operation requires.


Resource owners should also be able to configure what can never be delegated. Some capabilities should be available only to the user directly, or only through actors that meet stronger requirements. For example, an owner might decide that read-only investigation capabilities can be delegated to an incident-response actor, but administrative capabilities cannot.


That future direction follows from what User Identity Delegation has already made clearer: broad service identities and pure user impersonation both remove context that downstream systems need. The model gets stronger as more of that context becomes available at decision time: subject, actor, origin, invocation, resource, action, assurance, and consent.


The future version should shrink the need for broad standing service identities, make agent actions easier to explain, and let sensitive operations demand stronger assurance without forcing every workflow into one rigid pattern: powerful automation without super-identities.


We have the beginning of the right shape: authority that is delegated explicitly, carried in a verifiable token, constrained by consent and current grants, and visible enough for downstream services to make better decisions.


That is already a better answer to "who's asking?"
