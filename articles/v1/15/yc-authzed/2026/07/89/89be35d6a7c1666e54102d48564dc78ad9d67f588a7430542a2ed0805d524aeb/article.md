---
schema_version: "1.0.0"
document_id: "89be35d6a7c1666e54102d48564dc78ad9d67f588a7430542a2ed0805d524aeb"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-secures-authorization-for-ai"
published_at: "2026-07-27T12:00:00+00:00"
first_seen_at: "2026-07-27T19:20:15.411163+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:d39611d6b0055dca0301c1372f109d848839bd8d742a191466fc5c27106c1422"
---

# How SpiceDB Secures Authorization for AI

Security is not a property you get from connecting an agent to an identity provider or placing an OAuth token in front of a tool. It comes from making the correct authorization decision every time an actor attempts to access a resource.


SpiceDB is designed for that decision.


It stores authorization relationships separately from application data, evaluates permissions against a typed[schema](https://authzed.com/docs/spicedb/concepts/schema) , and provides consistency controls for systems where access changes while requests are still in flight. This makes it a strong foundation for applications that need precise and continuously enforced permissions.


## Authorization as a relationship graph


SpiceDB models authorization through[objects](https://authzed.com/docs/spicedb/concepts/schema) ,[relations](https://authzed.com/docs/spicedb/concepts/schema#relations) , and[permissions](https://authzed.com/docs/spicedb/concepts/schema) .


Objects represent the entities in a system. These might include users, agents, tools, tasks, documents, projects, services, and organizations. In a permission check, the object requesting access is the subject, while the object being accessed is the resource.


Relations store facts about how those objects are connected. A user may operate an agent. An agent may be assigned to a task. A document may belong to a project. A tool may be available to agents working on a particular service.


Permissions are computed from those relations. The SpiceDB schema language supports union, intersection, exclusion, and graph traversal through arrows. This allows permissions to be derived through several layers of relationships without copying access rules into every application service.


Consider an agent assigned to investigate an incident. The agent may receive access to the incident through its task. The incident may inherit access rules from its parent service. A blocked relation may explicitly remove access even when another path would normally grant it.


SpiceDB evaluates the complete graph to determine whether the requested permission exists.


This matters because authorization is rarely a simple role lookup. Real permissions are inherited through teams, folders, projects, ownership, delegation, and exceptions. SpiceDB represents those paths directly and evaluates them as one model.


## Permission checks are the enforcement point


The primary enforcement API is[CheckPermission](https://authzed.com/docs/spicedb/concepts/schema#relations) .


The application sends the subject, the permission being checked, and the resource. SpiceDB evaluates the schema and relationship data, then returns whether the requested permission is allowed. Some relationships include conditions, known as caveats, that must be evaluated using information supplied with the request. If that information is missing, SpiceDB can return a conditional result rather than treating the request as simply allowed or denied.


CheckPermission is designed for high traffic workloads. When a workflow needs several decisions,[CheckBulkPermissions](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.CheckBulkPermissions) evaluates them in one request and is preferred over making many individual calls.


This is useful in agent systems because one action may require several checks.


An agent retrieving documents may need permission checks across every search result. A tool call may require permission to execute the tool and permission to modify the target resource. A delegation may require the parent agent to hold both the underlying permission and the authority to delegate it.


The application remains the policy enforcement point. It calls SpiceDB before releasing data or performing the action. SpiceDB remains the policy decision point. It evaluates the authorization graph and returns the result.


The model does neither.


*SpiceDB evaluates each agent action before it reaches a protected resource.*


## Consistency is essential


A permission system can return the correct answer and still produce a security failure if the data is out of date.


This happens when access is revoked but a replica or cache continues to evaluate requests using authorization state from before the change. SpiceDB addresses this through explicit consistency options and ZedTokens.


A ZedToken represents a point in time of SpiceDB data. After changing a relationship, an application can pass the returned token to a later request using the` At Least as Fresh` consistency option. SpiceDB then evaluates the request using data that is no older than the revision represented by that token.


This protects against the[New Enemy Problem](https://www.youtube.com/watch?v=LuoaI1zSZ1Q) .


Imagine that a user loses access to a project and another user immediately creates a sensitive document inside it. A stale authorization check could evaluate the new document using the old project membership and expose it to the removed user.


The permission logic may be correct. The revision used to evaluate it is not.


SpiceDB makes that consistency choice explicit. Applications can minimize latency when slightly stale data is acceptable, require a particular revision when causal ordering matters, or request the latest available state when necessary.


Authorization is not only about which relationships exist. It is also about which version of those relationships is used for the decision.


## Context belongs in caveats


Relationships describe durable facts, but some decisions also depend on request context.


SpiceDB caveats attach typed CEL expressions to relationships. The relationship is considered valid only when the expression evaluates to true during the permission check. Caveats can incorporate timestamps, IP addresses, request attributes, numeric limits, and application supplied context.


An agent may be allowed to use a deployment tool only within an approved environment. A support workflow may require the request to include the correct customer identifier. A financial action may remain valid only below a defined threshold.


These conditions do not need to become permanent relationships in the graph. They can be evaluated using the context of the current request.


SpiceDB also supports relationships with expiration times. Once an expiring relationship reaches its deadline, it is no longer included in permission checks even if garbage collection has not yet removed it from the datastore.


Caveats and expiration serve different purposes.


Caveats determine whether access is valid under the current conditions.


Expiration determines when the relationship stops granting access.


Together, they allow applications to express authority that is specific to an actor, resource, action, context, and period of time.


## Delegation becomes explicit


Agent delegation is difficult to secure when authority is represented only by bearer tokens.


Passing a token to another agent copies the authority encoded in that token. It does not naturally reduce access to match the delegated task, and it becomes difficult to identify why the receiving agent has permission.


SpiceDB allows delegation to be represented as relationship data instead.


A parent agent can be connected to a task. A child agent can be assigned to a narrower part of that task. The child receives access only to the resources reachable through its own relationships. Expiration can end the delegation automatically, while caveats can require the correct task context during each check.


This does not make delegation policy automatic. The application must still decide which relationships to create and which permissions may be delegated.


What SpiceDB provides is a system where that policy is visible, testable, and enforced using the same authorization model as every other request.


The Watch API can also stream relationship creations, updates, and deletions. Consumers can resume from a ZedToken and use transaction metadata to correlate a relationship change with the application operation that produced it.


This is useful for audit pipelines, permission aware indexes, and systems that maintain derived views of authorization data.


## SpiceDB secures authorization


SpiceDB secures the authorization boundary by providing consistent permission decisions across identity providers, agent runtimes, tools, and application services. It deliberately does not attempt to secure every part of an application. It does not authenticate identities, inspect prompts, isolate agent runtimes, or decide whether a requested action is sensible.


Instead, SpiceDB gives those components a shared authorization system. Regardless of where an identity was authenticated or which runtime initiated the request, the same schema and relationship data determine whether the action is permitted.


Its schema defines how access is derived. Its relationships record how subjects and resources are connected. Its APIs evaluate permissions at the point of enforcement. Caveats and expiring relationships constrain access by context and time. Its consistency model allows permission checks to observe the correct authorization revision.


These protections do not depend on whether a model recognizes malicious behavior. They are deterministic properties of the authorization system.


An agent may request the wrong document, select the wrong tool, or follow instructions introduced through untrusted content. None of those events creates a relationship in SpiceDB or changes the permissions defined by the schema.


The agent can request an action. SpiceDB determines whether the application should allow it. That is what secure authorization looks like.


Explore the[AI agents schema example in the SpiceDB Playground](https://authzed.com/learn/spicedb-schema-examples/ai-agents) to see how users, agents, documents, and delegated permissions can be modeled and tested. Run permission checks, change the relationships, and adapt the schema to your own agent workflow.


On this page


- Authorization as a relationship graph
- Permission checks are the enforcement point
- Consistency is essential
- Context belongs in caveats
- Delegation becomes explicit
- SpiceDB secures authorization


## Related


[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)[Engineering Build a production-grade Agentic RAG system using AuthZed Cloud Learn how to build a production-grade Agentic RAG system on AuthZed Cloud, where authorization is hardcoded into the LangGraph pipeline—not a prompt instruction the agent can skip. Sohan Maheshwar · Apr 15, 2026 · 7 min](https://authzed.com/blog/build-production-grade-agentic-rag-authzed-cloud)


[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)[Engineering Introducing SpiceBox and spicedb-dev AI coding agents need better permissions, and so does the code they write. SpiceBox enforces fine-grained permissions on AI coding agents using SpiceDB, while spicedb-dev gives agents the authorization context they need to generate code with proper access control from the start. Both are open source. Joey Schorr · Apr 8, 2026 · 7 min](https://authzed.com/blog/spicedb-dev-and-spicebox-add-permissions-for-ai-coding-agents)


[Engineering An Authorization Platform Built for the AI Era AI agents decide what to access, which tools to call, and what actions to take—autonomously, at volume, and in ways most permission systems weren't designed to handle. AuthZed is the authorization platform built to close that gap. Mar 19, 2026 · 3 min](https://authzed.com/blog/authorization-platform-built-for-the-ai-era)[Engineering An Authorization Platform Built for the AI Era AI agents decide what to access, which tools to call, and what actions to take—autonomously, at volume, and in ways most permission systems weren't designed to handle. AuthZed is the authorization platform built to close that gap. Sam Kim · Mar 19, 2026 · 3 min](https://authzed.com/blog/authorization-platform-built-for-the-ai-era)
