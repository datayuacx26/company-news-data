---
schema_version: "1.0.0"
document_id: "a1f3f45dfd19902620df35114bb474dc11e4a7f8dbf7bfa624a66a96b2b11ef5"
company_key: "yc-dedalus-labs"
company: "Dedalus Labs"
source_id: "yc-dedalus-labs-news-import-c6fd8a49c5f3"
canonical_url: "https://www.dedaluslabs.ai/blog/dedalus-auth-launch"
published_at: "2026-01-19T09:00:00+00:00"
first_seen_at: "2026-07-21T15:59:30.110359+00:00"
fetched_at: "2026-07-28T22:23:25.248280+00:00"
content_hash: "sha256:c8b08f539d6dda702be109d036350c0d3f789950bbe2f03a70c1ead78afb8cea"
---

# Introducing Dedalus Auth

## Introducing Dedalus Auth (DAuth)


Today we’re excited to launch **Dedalus Auth (DAuth)** , a new way to secure Model Context Protocol (MCP) servers and marketplaces. Our authentication architecture protects user credentials and solves long-standing security gaps in the MCP landscape.


DAuth is designed specifically for MCP: it’s multi‑tenant, faithful to the[MCP specs](https://modelcontextprotocol.io/specification/2025-11-25) , and ensures that no one can see your secrets, not even us.


Existing auth implementations are underdeveloped and difficult for smaller developers to implement. API keys and raw secrets are still being passed around with limited protection, making them vulnerable to malicious servers that hijack user credentials.


To build MCPs with production-grade security, developers need to spend weeks becoming security experts to build authentication and security infrastructure. Most don’t bother. But we believe you shouldn't have to compromise on security.


That’s why we developed DAuth, so MCP creators don’t have to reinvent auth infra, and users never have to hand secrets to strangers.


The best thing about DAuth is that it is built in to[our SDK](https://docs.dedaluslabs.ai/sdk) , takes minutes to integrate, and doesn't require developers to be security experts.


## Security is the bottleneck for MCP adoption


The biggest barrier to deploying AI agents in production is trust.


Recent high-profile[security](https://www.docker.com/blog/mcp-horror-stories-the-supply-chain-attack/)[incidents](https://thehackernews.com/2025/09/first-malicious-mcp-server-found.html?m=1) have shown that MCP can be a vulnerable vector of attack for malicious actors. If we’re going to give agents the tools they need to be useful, their permissions must be handled accurately and securely.


When we started building our[MCP marketplace](https://www.dedaluslabs.ai/marketplace) , we were disappointed by the state of MCP auth.


MCP standardized around OAuth, but it doesn’t support dynamically passing API keys, forcing existing MCP marketplaces to be OAuth-centric and single-tenant.


In addition, the MCP specs don’t specify how non‑OAuth credentials should be handled. This is a problem because most MCP servers are non-OAuth, as many of the tools developers use are not authenticated by OAuth, like a search API or database credentials.


For developers using those tools, MCP security becomes a chore. To do things the “right way,” you need to create your own OAuth app for every tool. The easier route, storing raw credentials with the host, turns marketplaces into a single vector of attack: one malicious actor can collect thousands of secrets.


No existing MCP authentication solution serves our users security needs. No one else seems to be working on one that will. So we built the missing baseline: secure, multi-tenant auth that doesn’t depend on every tool being OAuth.


## At the core of our authentication layer is a multi-tenant infrastructure


Single-tenant authentication means that every deployed MCP server needs to deploy its own authentication layer. Multi-tenant authentication means servers using our SDK can authenticate with DAuth, serving as a single authentication layer for every MCP server we host.


Our unique architecture is zero-trust and host-blind, which means Dedalus never sees raw API keys or access tokens. Here’s what that looks like in practice:


1. The Dedalus SDK performs client-side encryption on your credentials, protecting secrets before they leave your device.
2. Whenever the SDK needs permission to do something, we initiate a secure exchange, calling our open-source auth server to validate access.
3. When a user request reaches your MCP server, the server acts as a standard OAuth 2.1 Resource Server. Access is verified without ever touching the credential.
4. Once the MCP server validates a request, we send it to The Enclave, a network-less hardware-secured enclave written in Rust.
5. Within this network-less Enclave, your credentials are checked for milliseconds before being zeroed from memory.
6. Then, the request is encrypted before being sent downstream, without ever revealing the secret to Dedalus or your MCP server.


The Enclave is a digital Fort Knox, a network-less fortress built in Rust that neither remote hackers or Dedalus can see your secrets. Best of all, this works across all auth cases. This lets us offer much stronger security guarantees than any existing marketplace or solution.


With DAuth, we are proud to offer the only secure and multi-tenant marketplace to date. If you’re interested in learning more about our system, check out our technical blog post explaining how we built it.


## Now that we can trust agents, what’s next?


The best part about DAuth is that you no longer need to be a security expert to build production-grade MCP-powered agents.


DAuth is built into the Dedalus SDK. Rather than implementing OAuth flows that change with each tool, developers define an Intent (e.g., slack_read) that ensures permissions can’t be hijacked by malicious actors. This intent-based system can be implemented in a few lines of code, removing the burden of security from the developer.


For users, this means they can trust MCPs made by anyone using the DAuth framework the same way they would trust an MCP from a big name like Google. DAuth opens up a new world of possibilities, where trust is a default on our platform.


We believe MCP won’t matter in production until auth stops being optional and secrets are no longer out in the open. Security is the key to creating a robust, community-driven agent economy, where trust is essential to both creators and end users. We’re rolling out updated docs and example use cases to show you how to use DAuth to build useful agents in the real world.


If MCP is going to power real-world agents, security can’t be optional. When you build or run on Dedalus, you’re opting into a standard you can trust.


## If you want to try Auth today


- Build with Dedalus Labs Authentication Layer for **FREE** by deploying an MCP with our SDK. **Free credits available until Jan. 27.**
- More use cases coming soon.
