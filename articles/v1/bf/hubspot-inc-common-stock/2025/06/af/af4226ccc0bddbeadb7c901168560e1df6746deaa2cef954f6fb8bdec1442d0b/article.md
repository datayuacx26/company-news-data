---
schema_version: "1.0.0"
document_id: "af4226ccc0bddbeadb7c901168560e1df6746deaa2cef954f6fb8bdec1442d0b"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-news-import-815987d58dd2"
canonical_url: "https://product.hubspot.com/blog/unlocking-deep-research-crm-connector-for-chatgpt"
published_at: "2025-06-18T00:00:00+00:00"
first_seen_at: "2026-07-21T23:16:14.524102+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:04093cab96985d46fd309dad4530047b1802d9e9b2212bc788c847a5fd55ce3b"
---

# Unlocking Deep Research for 250,000+ Businesses: How HubSpot Used MCP to Engineer the First Third-Party CRM Connector for ChatGPT

Authors:[Rishav Paul](https://www.linkedin.com/in/rishavpaul/) ,


[William Hou](https://www.linkedin.com/in/thouwi/) ,


[Chris Connors](https://www.linkedin.com/in/cconnors83/) , and[Sejal Parikh.](https://www.linkedin.com/in/sejalparikh/)
----------


HubSpot has become the first CRM to launch a Deep Research Connector with ChatGPT— and the first third-party[MCP connector available in the ChatGPT Registry](https://platform.openai.com/docs/mcp) built with a remote[Model Context Protocol](https://modelcontextprotocol.io/introduction) (MCP) server. This connector extends ChatGPT’s capabilities while eliminating the need for users to install or configure anything locally.


## Reimagining our MCP Server Infrastructure


Prior to this launch, we had released a[local MCP Server](https://developers.hubspot.com/mcp) on[NPM](https://www.npmjs.com/package/@hubspot/mcp-server) . While useful for early testing and feedback, that approach required users to manually create Private App Access Tokens and configure their MCP clients, which introduced technical complexity and limited adoption to tech-savvy developers.


Our new remote MCP server changes everything. It represents a major leap forward from local servers by eliminating the need for users to install or run anything on their machines. Instead, they can access capabilities through their browser using trusted and reliable authentication flows like[OAuth](https://developers.hubspot.com/docs/guides/apps/authentication/working-with-oauth) . It democratizes access to AI for more than 250,000 businesses, while maintaining enterprise-grade security and respecting user-level permissions.


In this blog, we will explore how HubSpot built a completely homegrown MCP Server infrastructure **in a matter of weeks** and the rationale behind building it in-house.


## Building an MCP Server: Buy vs. Build


When it came time to build our remote MCP server, we faced the classic engineering dilemma with two clear paths:


1. **(Buy):** Use a third-party solution, e.g.,[Cloudflare's turnkey MCP solution](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/)
2. **(Build):** Develop it in-house using HubSpot’s infrastructure


Option 1 was tempting. Third-party solutions had already solved the OAuth 2.1 complexities like PKCE (Proof Key for Code Exchange) and Dynamic Client Registration. Their pathway offered a fast time to market, and already resolved the trickier aspects of MCP's authorization spec. For many companies, this would've been the obvious choice.


However, our infrastructure team had already been prototyping an MCP server by extending the[MCP Java SDK](https://github.com/modelcontextprotocol/java-sdk/tree/main/mcp) inside a Dropwizard microservice. In parallel, our earlier local MCP server launch provided valuable lessons and feedback, helping us better understand the protocol’s nuances and lay the necessary groundwork.


So, the decision to build in-house became clear once we considered what hosting our own infrastructure could enable. By building on HubSpot’s platform, we could leverage enterprise-grade CI/CD, auto-scaling, observability, and even[region-aware API call routing](https://product.hubspot.com/blog/routing-api-traffic) built on Cloudflare Workers - all production ready features directly available to us without any additional setup. It also gave us direct control over how MCP interacts with our existing scopes model for OAuth, allowing us to ensure user-level permissions were respected throughout any interactions with our MCP tools.


This wasn't just about checking boxes. It was about building something that could evolve with our platform and unlock experiences in the future that third-party solutions simply couldn't match.


## Laying the Foundation: Streamable HTTP MCP Server


Our MCP server is built with the[Java MCP SDK](https://github.com/modelcontextprotocol/java-sdk) . We chose Java for long-term maintainability and development within our standard technical stack. HubSpot’s backend libraries and infrastructure tooling are already centered around Java microservices, so we focused our efforts on implementing MCP in a standard Dropwizard service.


The Java MCP SDK has worked well for us, and we’re grateful to the community that contributes to it! For our initial release, we introduced a few adaptations to better align the SDK with our Dropwizard backend. These changes demonstrate the SDK’s flexibility, even in an enterprise setting. Although these modifications are designed to fit HubSpot’s stack, we hope you’ll still find them helpful, or at least interesting.


## Implementing Streamable HTTP for MCP


A remote MCP is essentially an HTTP API spec that allows an LLM to discover and invoke endpoints automatically. Currently,[Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) defines two standard methods for responding to a remote MCP HTTP request.


1. Return a JSON response, similar to a traditional REST call.
2. Open[Server-sent events (SSE)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) connection with a unique session ID, then return the initial JSON response as an event in the stream. Subsequent requests for that session ID must return responses as events over the same SSE connection.


Before the standardization of[Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) , this was the only remote MCP support defined in the spec. Streamable HTTP now provides a formalized structure for both streaming and non-streaming use cases, making it more flexible for different workloads.


## For various reasons, we wanted to avoid supporting the SSE transport protocol. First, we didn’t immediately need the server-to-client notification support provided by SSE connections, our use case focused only on tool calls. Second, SSE requires long-lived connections. which can be challenging for load balancers to handle reliably, especially in environments like ours where servers are frequently scaled up or down. Third, managing stateless HTTP requests is far simpler than managing sessions.


When we began prototyping, we discovered that the Java SDK did not support the newly standardized[Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) transport protocol. At the time of writing, there are a few open draft pull requests, but no official solution has been implemented yet. Fortunately, the SDK was written to support custom transport protocols, which allowed us to implement[Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) directly. That said, this is a potent reminder of the value of contributing back to open source - something we plan to pursue to ensure that the MCP Java SDK remains current with the latest MCP Specification.


#### The SDK


Below is a highly simplified summary of the relevant interfaces/methods in the


Java MCP SDK:


```text


// MCP Request => MCP Session
// Unlisted methods for session/notification management
public interface McpServerTransportProvider {
void setSessionFactory(McpServerSession.Factory sessionFactory);
}


// MCP Request => MCP Response
// Unlisted methods for session/notification management
public class McpServerSession {
Mono<Void> handle(McpSchema.JSONRPCMessage message);


// Custom MCP Transport => MCP Session
public interface Factory {
McpServerSession create(McpServerTransport sessionTransport);
}
}


public interface McpTransport {


Mono<Void> closeGracefully();


// MCP Response => Client
Mono<Void> sendMessage(JSONRPCMessage message);


// HTTP Body => MCP Request
<T> T unmarshalFrom(Object data, TypeReference<T> typeRef);
}


```


These interfaces separate session management from transport implementation, allowing us to inject custom response transport while still leveraging the SDK's logic to process, route, and respond to MCP requests.


## The Streamable HTTP Transport Implementation


HubSpot Java HTTP APIs are built using Dropwizard, so we implemented a custom transport provider to bridge Dropwizard's JAX-RS async processing with the MCP SDK. Every HTTP call corresponds to a single ephemeral session:


```text
public class StreamableMcpServerTransportProvider implements McpServerTransportProvider {
private volatile McpServerSession.Factory sessionFactory;
@Override
public void setSessionFactory(McpServerSession.Factory sessionFactory) {
this.sessionFactory = sessionFactory;
}
public CompletableFuture<Void> handleMessage(JSONRPCMessage message, AsyncResponse asyncResponse) {
// Create custom transport bound to HTTP response
DropwizardAsyncResponseServerTransport transport =
new DropwizardAsyncResponseServerTransport(asyncResponse);
// Create new session for this HTTP request
McpServerSession session = sessionFactory.create(transport);
return initSession(session).then(session.handle(message)).toFuture();


}


private static Mono<Void> initSession(McpServerSession session) {
session.init(
McpSchema.ClientCapabilities.builder().build(),
new McpSchema.Implementation("standard client name/version header", "please")
);
McpSchema.JSONRPCNotification notification =
new McpSchema.JSONRPCNotification(
McpSchema.JSONRPC_VERSION,
McpSchema.METHOD_NOTIFICATION_INITIALIZED,
Map.of()
);
return session.handle(notification);
}
}
```


Our custom transport then implements


[McpServerTransport](https://github.com/modelcontextprotocol/java-sdk/blob/07e7b8fd6bac47be4527f97451f8cdd95ed31a38/mcp/src/main/java/io/modelcontextprotocol/spec/McpServerTransport.java) to send MCP responses via HTTP response bodies.


```text
class DropwizardAsyncResponseServerTransport implements McpServerTransport {


private final AsyncResponse asyncResponse;


// simplified implementation
@Override
public Mono sendMessage(McpSchema.JSONRPCMessage message) {
return Mono.fromCallable(() -> {
asyncResponse.resume(
Response
.ok()
.type(MediaType.APPLICATION_JSON_TYPE)
.entity(OBJECT_MAPPER.writeValueAsString(message))
.build()
);
return null;
});
}
}


```


The result? Full MCP protocol compliance within a stateless HTTP environment.


### Service Discovery - scaling our internal RPC framework for MCP


## CHIRP: HubSpot's Service Communication Framework


At HubSpot, we've built many of our microservices using CHIRP (backronym for Cool Hubspot Inter-process Remote Procedures 😀) - our internal RPC framework. CHIRP takes a contract-first approach where developers define microservices as Java interfaces with strongly-typed immutable request/response models, and the framework automatically generates both client and server code.


But CHIRP is more than just code generation, it standardizes much of the complexity that comes with large engineering teams: authentication, authorization, model definitions, service discovery, error handling, and more.


## Dynamic MCP Tool Discovery and Registration


We built an MCP Gateway service that automatically discovers and registers MCP tools across our entire infrastructure with the MCP Server decided by the tool owner. The magic happens through a simple annotation—services can mark their RPC methods with` @ChirpTool


` to indicate they should be exposed as MCP tools.


```text


@ChirpTool(


gateways = { ChirpMcpGatewayType.DEEP_RESEARCH_CONNECTOR },


description = "Search for resources in the HubSpot CRM"


)


MCPSearchResponse search(MCPSearchRequest request) { ... }


```


Behind the scenes, our MCP server periodically polls for new or updated services deployed with these annotations. The system then fetches their specifications and automatically converts each RPC to a MCP spec-compliant tool. Finally, these wrapped RPCs are registered with the MCP SDK, ready to be called by any new clients.


The beauty of this approach? Teams just deploy and update their services, and their MCP tools become immediately available to AI agents – no manual configuration, no gateway redeployments, no coordination needed. It just works.


We've democratized MCP integrations across HubSpot - now, hundreds of teams can now make their products AI-ready just by adding @ChirpTool


to their existing RPC methods. No new frameworks or protocols to learn.


While this distributed approach lets every team own their AI story and innovate independently, we will be implementing governance and guardrails to ensure customers always experience a cohesive, unified interface rather than a fragmented reflection of our org chart. It's the best of both worlds: rapid innovation at the edges with strategic coordination at the center.


### Security: OAuth with User-Level Access Tokens


## HubSpot Apps: A brief history


Historically, HubSpot apps (like the HubSpot deep research connector) have been account-wide integrations installed by an admin to sync data between HubSpot and the 3rd party. However, our OAuth 2.0 system has always supported user-specific access tokens. No one wants an integration to stop working if the original installer’s permissions changed.


That's why we've always required a user to have proper permissions requested by the app in order to install, but don’t use that user’s more granular permissions post-install. The API token is **not** the user. There are also various integration touchpoints within HubSpot that may ultimately trigger calls to 3rd parties and calls back to HubSpot, and we don't want the original installer to be conflated with another user that triggered some action.


That's also the reason why most of our public APIs do not enforce user-level permissions, whereas our native UI does. We intentionally avoid tying user identities to access tokens for background data syncs or automated workflows which don’t expect user-level context during execution.


## Leaning into MCP and user level access


Fast forward to today — features like UI Extensions ([App Cards](https://developers.hubspot.com/build-app-cards) ) and AI agents have changed the landscape. As a result, there are now far more real-world use cases where respecting user-level permissions is essential.


For example, when a user connects to HubSpot from within ChatGPT, they must not be able to query data they don’t have access to. The actions taken via ChatGPT should reflect the same permissions and access controls the user would experience in HubSpot’s native UI.


The[MCP spec](https://modelcontextprotocol.io/specification/2025-03-26) makes some clear recommendations and requirements, but the one that stood out to us the most is that users must consent and control data access. While the spec also includes features like dynamic client registration and OAuth 2.1 improvements including PKCE/DPoP, our primary focus was on access permissions and how we adapted our current OAuth stack and permissioning system to build the Deep Research Connector with OpenAI.


Because our public APIs were not originally designed to enforce user-level permissioning, we didn't want to overhaul our entire public API infrastructure all at once. HubSpot has thousands of public APIs that hundreds of teams at HubSpot contribute to, all of which depend on a shared middleware library for authentication and permission checks. Modifying that system to enforce user-specific access across the board would be an enormous undertaking — and a potentially breaking change for many of our existing applications. For example, the original app installer may no longer be with the company, and enforcing their permissions retroactively would create significant friction.


We are addressing this through a phased rollout, with the goal of empowering all of HubSpot’s public APIs to respect user permissions without breaking existing installs. But for now, we took a pragmatic approach: we spun up a dedicated gateway API at mcp.hubspot.com to handle CRM search, associations, and property fetching — fully instrumented to respect user permissions in the context of MCP.


An OAuth access token contains four important pieces of information:


- ` client_id


` : app/client info
- ` hub_id


` : (aka portalId/accountId)
- ` user_id


` : who installed it
- ` scopes


` : permissions the user granted the developer


Our OAuth system already requires that a user has all necessary scopes in order to install on a HubSpot account. We also support user-level apps and installs for HubSpot-built integrations like Gmail, Outlook, and Google Calendar. All we had to do was set one of these apps up for OpenAI, make it user-level, and plug in OAuth as opposed to our internal-auth we used for HubSpot-built integrations. Easy!


The last part involved ensuring compatibility with our existing public APIs. All our public APIs use Dropwizard's REST (JAX-RS) library, which makes it very easy to stand up new services.


## Permissions enforcement and scope forwarding inside MCP


As mentioned earlier, our remote MCP server binds JAX-RS endpoints on a per-session basis. Adding some basic code in the middle allowed us to:


- Extract the relevant scopes of the access_token


- Fetch user-specific scopes for the user
- Replay those permissions to internal APIs where user permissions are respected for data within the CRM


Scopes that we use within apps and access tokens only hold a limited amount of information. In contrast, HubSpot’s internal user permission model is much more granular. For example, users may have access to:


- only their records
- their teams records
- unassigned records
- and even specific properties due to[field-level permissions](https://knowledge.hubspot.com/properties/restrict-view-edit-access-for-properties)


Given the complexity, we can't reasonably store all this in an access token; which is why we need to intersect the app's permissions with the user's permissions, and send that info to the internal service to respect the permissions of the user. If we were to send the entire user, then we'd be opening up the app to have more permissions than the user who granted the app.


## Granular permissions == more access!


You'll notice that we support contacts, companies, tickets, and deals in our Deep Research Connector. We use the following scopes on apps for reading CRM record data:


- ` crm.objects.contacts.read


`
- ` crm.objects.companies.read


`
- ` crm.objects.deals.read


`
- ` crm.objects.tickets.read


`


We intentionally omitted scopes for write access or access to sensitive properties, as they aren’t required for Deep Research use cases.


Having granular scopes allows a user to only grant access to what the integration is using, rather than having one monolithic "scope" that gives broad access to pieces of functionality and could keep growing and granting more access under the hood. However, today we have seen very few (if any) scopes returned by the .well-known/oauth-authorization-server


endpoint in remote MCP servers we have examined. We believe more granular scopes and access permissions are beneficial for developers and users, as they build trust that an app only has access to very specific functionality.


## User permissions in the future


AI agent connectors are just one way that user permissions should be respected in our product. We have several UI Extension touchpoints with App Cards, as well as various other use cases where 3rd party developers want to know and respect the user who performed an action.


Earlier, we discussed what we needed to do to replay the intersection of the apps + user scopes/advanced-permissions from our remote MCP server to our internal APIs that deal with CRM records. In the future, we see a clearer and more scalable model emerging — one based on service accounts (system users) backed by every single API call. When tokens are generated, we can easily and efficiently find the intersection between the app, account, and user's permissions, and create a token that points to a service account with that access.


Because our access tokens are short-lived, we can safely assume that a token will only have the access that it should have. This can also be expanded to account-wide integrations, as we assign a service account to that token instead of the installing user. In this way, tokens still have users, but they are now backed by service accounts.


Since our service accounts are the same as users in our backend permissioning model, we can do much more granular access restrictions for any integration at HubSpot. The main reason we didn't do this for the initial Deep Research Connector is that this rollout takes time. We need to build the infrastructure to support this, and coordinate and roll out with hundreds of teams that own public APIs.


When you're moving at the speed of AI, you need to make some short-lived compromises. Currently, our public APIs on our MCP server can be a few dozen milliseconds slower due to computing user and account permissions at runtime. This is a tradeoff we were willing to make in order to ensure user-level permissions were precisely enforced since the APIs and datasets being queried by the deep research agent is nondeterministic.


Longer term, we intend to take our learnings on our MCP server and ChatGPT connector and enhance our entire developer platform to not only respect user permissioning, but also mature our OAuth stack to the latest standards in OAuth 2.1 and beyond.


### Designing a Query DSL for Deep Research


The Deep Research Connector integration search semantics required that we expose a search MCP tool that consumes a[single query field](https://platform.openai.com/docs/mcp#search-semantics) . If you're familiar with[HubSpot's Public Search API](https://developers.hubspot.com/docs/guides/api/crm/search) , you know it's a sophisticated system that allows clients to express complex requirements through rich data structures. Compressing that down to a single query string that AI models can reliably generate based on user input was an interesting challenge.


We designed a token-based DSL that balances power with simplicity (also cited in[OpenAI documentation](https://platform.openai.com/docs/mcp#teach-the-model-how-to-form-valid-queries) ). Each search query consists of space-separated tokens following a` key\[:operator\]:value


` pattern. The syntax supports everything from basic object searches (` object_type:contacts


` ) to complex filters with operators like` contains_token


, has_property


` , and` in/not_in


` for multi-value comparisons. We can handle associated object searches (` associated_companies:12345


` ), pagination (` limit:100 offset:20


` ), and sorting (` sort:lastmodifieddate:desc


` ).


What makes this work well with AI models is its predictable structure and clear examples. When implementing similar tools, we've found that providing comprehensive examples in your tool descriptions dramatically improves the model's ability to generate correct queries. For instance, showing that:


“Find contacts in companies acme & hubspot" translates to` object_type:contacts associated_companies:in:acme,hubspot


`


teaches the model to understand the pattern. We intentionally kept certain features simple—no boolean OR logic, no nested expressions, no relative date syntax like "last week"—because these constraints actually make the model more reliable at generating valid queries. The key insight is that AI models excel at pattern matching, so a consistent, example-rich DSL will outperform a more flexible but ambiguous query language every time.


The model can still make mistakes. Ensure you return helpful errors to help the model correct itself.


## Iterating at AI speed in HubSpot


Through thoughtful engineering decisions made at the rapid pace of AI development, we successfully implemented a complex protocol as a secure, customer-ready feature in a matter of weeks. This showcases how our team tackles challenging problems at the cutting edge of AI technology while delivering real customer value.


With our remote MCP Server now established, we are ready to solve for our customers continuously. If you’re a HubSpot customer, learn more about the Deep Research Connector and share your feedback at[https://www.hubspot.com/ai-tools/openai-connector](https://www.hubspot.com/ai-tools/openai-connector) .
