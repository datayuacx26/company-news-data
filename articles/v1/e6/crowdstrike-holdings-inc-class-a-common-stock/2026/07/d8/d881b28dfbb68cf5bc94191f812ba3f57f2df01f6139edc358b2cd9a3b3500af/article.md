---
schema_version: "1.0.0"
document_id: "d881b28dfbb68cf5bc94191f812ba3f57f2df01f6139edc358b2cd9a3b3500af"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/the-identity-problem-hiding-in-ai-agent-deployments/"
published_at: null
first_seen_at: "2026-07-20T23:21:19.193454+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:f417f621c4d3217a7603bb95c8ee73e4e04a37501341e9ebeed3fb2e4acaa28d"
---

# The Identity Problem Hiding in AI Agent Deployments

As organizations rush to deploy AI agents across enterprises to handle HR cases, write and execute code, and manage customer interactions, they very often need to grant them access to sensitive systems and data. Unlike a human employee who authenticates once and acts within a well-understood role, an AI agent may act on behalf of multiple users simultaneously, be instantiated and terminated dynamically, and invoke other agents to complete subtasks — all without a human in the loop to provide real-time oversight.


This creates an identity problem that the industry has not yet solved. When an AI agent accesses a resource such as a database, an API, or a file, the system receiving that request needs to know the actor and user principal behind it, and what the nature of the relationship is between the agent and the user it represents. Without this information, organizations cannot enforce fine-grained access controls, produce meaningful audit trails, or detect when an agent is acting outside its intended scope.


As AI expands across organizations, the absence of a standard way to express this identity context is an active risk that grows with every new agent deployment. In this blog post, we’ll examine the implications of the lack of standardization for information in OAuth access tokens, and steps the industry can take toward standardization.


## OAuth Tokens Today


OAuth access tokens are used to not only identify the authenticated user, AI agent, or workload; they are also used to determine what that particular access request is or is not permitted to do. The data that an application has access to at any moment in time is dependent on the scopes or other context included in the OAuth access token. This blog post focuses on identifying the principal(s) in an access request that uses an OAuth access token.


We’re used to issuing OAuth access tokens to individual principals, whether they are humans or applications. When issuing tokens to a human, it was sufficient to encode the type of client application being used within the token. When applied to AI agents, the MCP core spec recommends using OAuth 2.1 to issue access tokens. The tokens may be issued with the user as a subject (using, say, the OAuth code flow) or to the agents themselves (using, say, the client credentials grant).


## The Importance of Agent Identity


Agentic AI usage can be broken into four patterns from an identity point of view:


1.


**Interactive** : The user uses an interactive AI client that determines the actions to be taken. The user is available to provide consent if required.


2.


**Offline** : The user kicks off a task to an AI client, and the user is no longer available until the task completes.


3.


**Automated** : The AI agent(s) run autonomously; they are kicked off by automated workflows, instantiated and terminated as required, or left continuously running and assign themselves tasks based on a task queue.


4.


**Transitive** : One AI agent (using any of the three patterns above, and this fourth one) invokes another AI agent to complete its task.


The agent’s identity becomes increasingly important as its autonomy, because an interactive use case is only slightly different from a user using their browser. However, the more the agent determines its own actions, the more its identity matters for access decisions.


## Why OAuth Tokens Can’t Accurately Represent Agent and User Identities


When the access token is issued to the agent, there is no place in the commonly used JWT format ([RFC 9068](https://datatracker.ietf.org/doc/html/rfc9068) ) to insert the user on whose behalf the agent might be acting. RFC 9068 defines standard claims for JWT-formatted OAuth access tokens, including subject, client, and scope, but was designed for single-principal scenarios and defines no claims for agent instance identity or the relationship between an agent and the user it is acting for.


When the OAuth access token is issued to the user, the client identity (i.e., the agent identity) is encoded in the “client_id” field of the token. This “client_id” value identifies the registered client application — the type of agent — but OAuth has no standardized mechanism to identify a specific instance of that agent.


What we’re lacking is a standardized way to express the agent instance identity and the user identity in an OAuth access token. Even when we specify both identities, the relationship between the user and the agent is not captured anywhere. The relationship between Claude Code and the programmer who initiated a task that caused Claude Code to get a token to access GitHub, for example, is very different from the relationship an autonomous agent has with the user whose HR case it picks up in its workflow.


In the former example, the user might be delegating a specific aspect of their permissions (e.g., performing an action in GitHub) to the agent (Claude Code). In the latter example, the human’s relationship with the agent is more distant, in that if the agent has general permissions to access salary data, it should only be allowed to access the specific user’s salary when working on that user’s case.


In short: OAuth access tokens have no standardized fields for agent instance identity, user identity, and the relationship between them.


A note on what is meant by “instance” here. In conventional RESTful architectures, an instance refers to a specific running process — an API server may have multiple instances within the same data center or across regions. In the AI agent case, since the agent needs to maintain conversation context across multiple interactions, “instance” is better understood as a logical concept: a specific ongoing task or session, rather than a particular running process.


There are other relationships between users and agents that are not necessary to capture in access tokens. For example, if an HR manager requests that a set of AI agents be provisioned and employed for handling HR cases, the relationship between that manager and the AI agent is not captured in the above example. As a part of provisioning such agents, IT may determine general permissions for the agents (i.e., roles or entitlements). Those will likely overlap with the roles of the manager that requests having such agents, but they are otherwise independent. The agent, when it runs, uses permissions that it has been granted when it was provisioned, and its relationship with the person who provisioned or sanctioned the agent does not matter during its runtime.


## Why This Matters


Within these AI workflows, it is important to identify the instance of the application (or AI agent), the human on whose behalf it is acting, and what the delegation or other relationship is between this application and the human. This is because the same client software (e.g., Claude) could be running substantially different tasks in relation to many different users, so each instance may have a different set of permissions. The[RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693) standard defines token exchange, which allows one principal to obtain a token on behalf of another, but it doesn't address the richer relationship context needed for agentic workflows.


A lack of standardization of this information in the OAuth access token will result in various systems expecting this data in different places, adding processing logic differently for each system, not accounting for agent identity, user identity, or their relationship in making authorization decisions, or misinterpreting the information in the token. These will result in systems making coarse-grained authorization decisions and ultimately giving too much access. In agentic call chains, this can lead to the “[confused deputy problem](https://en.wikipedia.org/wiki/Confused_deputy_problem) ” whereby a downstream system gets more access and such greater access is abused.


## What Needs to Be Done


At the IIW April 2026 “un-conference,” where a small group of passionate participants discussed this topic, the general consensus was that AI use cases are going to need this, and unless we standardize it, there are going to be divergent implementations that cause incompatibility.


It is imperative to get together and specify how to capture agent instance identity, user identity, and their relationship, whether through fields within an OAuth token, or an adjacent mechanism such as a separate assertion or introspection extension.


It’s worth noting that the[Client ID Metadata draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-client-id-metadata-document/) also has a notion of “client id” that is separate from the client ID in RFC 8693. That draft addresses client metadata at the authorization request layer, upstream of the access token, and so is out of scope here.


#### Additional Resources


-


*Interested in learning more? Join us at[Fal.Con 2026](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) , where these conversations take center stage.*
