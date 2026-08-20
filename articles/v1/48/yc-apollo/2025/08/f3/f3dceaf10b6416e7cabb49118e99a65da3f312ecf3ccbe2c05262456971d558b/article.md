---
schema_version: "1.0.0"
document_id: "f3dceaf10b6416e7cabb49118e99a65da3f312ecf3ccbe2c05262456971d558b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-authorization-for-apollo-mcp-server"
published_at: "2025-08-08T09:00:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:db64681e5d0dfc5c3493b2c30dc25525f672b16433e5b2c848c7c72ae06d6a44"
---

# Introducing Authorization for Apollo MCP Server: Secure AI Access to Your GraphQL APIs

When we[launched Apollo MCP Server](https://www.apollographql.com/blog/the-future-of-mcp-is-graphql) , teams started connecting their GraphQL APIs to AI agents immediately. The feedback has been enthusiastic and consistent. “This is exactly what we need. We can finally give LLMs the right context, and do it declaratively!” Quickly followed by: “But we can’t deploy this to production without appropriate auth.”


They were right. Without knowing who they are and what they are allowed to do, every MCP server user shares the same anonymous access. Your intern and your CTO see the same data. There’s no audit trail. No way to separate development from production access. For teams serious about AI in production, this was a dealbreaker.


Today, we’re shipping authorization support in Apollo MCP Server. It’s a complete OAuth 2.1 implementation following the newly ratified[MCP Authorization specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization) (as of June 18, 2025).


****This makes Apollo MCP Server production-ready**** .


## **Why Authorization Matters for AI Agents**


The security research community has been raising legitimate concerns about MCP implementations.[For example, Simon Willison recently pointed out](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/) that “Model Context Protocol has prompt injection security problems” – malicious actors could potentially manipulate AI behavior through carefully crafted responses.


While authorization doesn’t solve prompt injection directly, it’s a critical first line of defense. Without authorization:


- Any user can access your MCP server
- There’s no audit trail of who requested what
- A single compromised session affects everyone (no way to revoke individual access)
- You can’t implement rate limiting or access controls


But there’s a more fundamental issue at hand: you simply can’t deploy to production without knowing who’s making requests. Enterprise security requirements aren’t optional. Teams have been stuck running MCP servers mostly in local development, unable to unlock the real value of AI agents with access to production data. And this is where GraphQL’s declarative approach also really shines. Just as we’ve extended GraphQL’s schema-driven development to MCP tools, authorization policies can be defined declaratively alongside your graph. Instead of writing custom authorization code for each API endpoint, you define access rules that work across your entire GraphQL schema. This means consistent security policies whether you’re accessing data through traditional GraphQL queries or AI agent interactions through MCP tools.


## **What We Built**


Apollo MCP Server now implements OAuth 2.1 with full PKCE support, following the[Protected Resource Metadata specification (RFC 9728)](https://datatracker.ietf.org/doc/html/rfc9728) . It works with modern identity providers that support OAuth 2.1 or OAuth 2.0 with PKCE, including:


- Okta
- Auth0
- Google
- Microsoft Entra ID (Azure AD)
- And other standards-compliant providers


### **Security-First Features**


We designed this implementation with a security-first mindset. That means focusing not just on compatibility and standards, but on eliminating common attack vectors and enforcing safe defaults.


Here are the core features of Apollo MCP Server’s OAuth implementation and what they protect against:


**Feature** **What It Prevents**


PKCE required Authorization code interception attacks


Token validation Confused deputy vulnerabilities


No token passthrough Token leakage to downstream services


Proper token scoping Over-privileged access


### **From Localhost to Production**


#### **At the MCP Server Level**


Authorization introduces critical capabilities for identity enforcement and session isolation:


- **User identity validated** : OAuth tokens are verified and user identity extracted
- **Secure token handling** : Each user’s session is isolated
- **SSO integration** : Works with your existing identity provider
- **Remote-ready** : Deploy MCP Server beyond localhost with confidence


#### **Combined with Apollo Router’s Built-in Authorization**


When combined with Apollo Router, you get out-of-the-box field-level access control, declaratively defined within your schema


- **Field-level access control** : Use @authenticated, @requiresScopes, and @policy[directives in your schema](https://www.apollographql.com/docs/graphos/routing/security/authorization)
- **Authorization at the edge** : Access checks happen before requests reach your subgraphs
- **No custom code needed** : Authorization rules live declaratively in your GraphQL schema
- **Automatic filtering** : Router removes unauthorized fields from responses


Here’s a basic example:


```text
type Query {
publicData: String
userData: String @authenticated
adminData: String @requiresScopes(scopes: ["admin"])
}
```


## **Simplifying Configuration: Config Files Replace CLI Arguments**


Together, these enhancements give your MCP Server the security and governance controls needed for safe, reliable AI integration in production environments.


Here’s the thing about adding authorization, it requires more configuration. And our command-line arguments were already getting unwieldy. So we’re making a breaking change: moving to YAML configuration files.


### **Why This Is Actually Good News:**


- Secrets live in environment variables, not command lines
- Version controlled and shareable with your team
- Sensible defaults let you start without a config file


If you’re already using Apollo MCP Server, the move is straightforward, mostly copying your CLI args into a YAML file.


Check our[documentation](https://www.apollographql.com/docs/apollo-mcp-server/command-reference#config-options) for config file examples.


## **Join the Conversation**


The MCP Authorization specification was just ratified on June 18, 2025. This space is moving fast, and real-world usage will shape how these standards evolve. Unfortunately, this also means not all of the tooling in this space is as rock solid as we hope. For example, you may find some MCP clients are not yet working as expected when used in combination with Apollo MCP Server. Either way, we’re committed to staying close to the community and adapting our implementation based on what works in production.


If you’re implementing MCP authorization, we want to hear about it:


- What worked?
- What didn’t?
- What patterns are you discovering?


Share your experiences in our[community forum](https://community.apollographql.com/t/apollo-mcp-server-now-implements-the-mcp-auth-spec/9370) or open an issue on[GitHub](https://github.com/apollographql/apollo-mcp-server) .


## **Additional Resources**


- [Apollo MCP Server Authorization Docs](https://www.apollographql.com/docs/apollo-mcp-server/guides/auth)
- [MCP Authorization Specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [Community Forum](https://community.apollographql.com/t/apollo-mcp-server-now-implements-the-mcp-auth-spec/9370)
- [GitHub Repository](https://github.com/apollographql/apollo-mcp-server)


## **TL;DR**


- Apollo MCP Server now supports OAuth 2.1 with PKCE
- Makes production deployment possible with user-level access control
- New config file system (breaking change, but worth it)
- Security-first implementation prevents common vulnerabilities


**Ready to bring your AI agents to production?**[Get started](https://www.apollographql.com/apollo-mcp-server)


Written by


Kevin Chu


[Read more by Kevin Chu](https://www.apollographql.com/blog/author/kchu)
