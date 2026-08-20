---
schema_version: "1.0.0"
document_id: "a0792b3566eed0cdf87259df551f4bf5626eb49bd7fefeb1530217fcba8334a3"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/resend-mcp-supports-the-new-2026-07-28-spec"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-08T00:27:16.961021+00:00"
fetched_at: "2026-08-08T00:27:19.070862+00:00"
content_hash: "sha256:edf87ca80d2bec8cd0aa48e801583380a976ccc4b5e6d1bcf90dbd05f0cd4bfd"
---

# Remote MCP Supports the 2026-07-28 Spec

The[MCP specification just released a major upgrade](https://blog.modelcontextprotocol.io/posts/2026-07-28/) , and now Resend's remote MCP server supports this latest version.


## What should you change?


**No changes required** . If you are already using the Resend MCP server, or if youinstall it today , you should see no change.


## What's new about the spec?


The[new specification](https://blog.modelcontextprotocol.io/posts/2026-07-28/) is the largest revision of the protocol since the initial launch of MCP. It includes several changes, including:


- **A stateless core** that scales on ordinary HTTP infrastructure
- **First-class extension support** including MCP Apps and task extensions
- **Authorization improvements** to align with OAuth and OpenID Connect deployments
- **A formal deprecation policy** so the protocol can evolve without disruption


Several of these changes make it easier to **scale remote MCP servers** . Without a protocol defined session, requests can flow to any MCP server instance. Previously, with OAuth 2.0 Dynamic Client Registration (DCR), every client installation created a new registration that the remote server had to retain, allowing stored state to grow without limit.


The replacement for DCR is[Client ID Metadata Documents](https://datatracker.ietf.org/doc/draft-ietf-oauth-client-id-metadata-document/) (CIMD). Instead of registering at runtime, the client hosts a JSON document that Resend fetches during authorization, so there's no registration call. The process gives the client a stable identity whether it's a hosted app, a CLI, or an MCP server. This applies to any OAuth client against Resend, not only MCP. View more guidance on[how to create an OAuth Client for Resend](https://resend.com/docs/guides/building-a-resend-oauth-client) .


## Backwards compatibility


The update is **backwards compatible** so older MCP clients and updated MCP clients continue to work.


If your client implements the new spec, it will automatically switch over, while older clients will still be able to use the previous version.


## Install the remote MCP server


If you haven't connected your agent with the remote Resend MCP server yet you can install it with any of the following options.


Connect Resend from the[connector directory](https://claude.ai/directory/connectors/resend) . The Connector bundles the MCP server and every Resend skill.


If your client isn't shown here, the server is hosted at` https://mcp.resend.com/mcp` . For more installation options, see the[MCP server documentation](https://resend.com/docs/mcp-server) .


Want to see what you can do with the MCP server? Check out this short video:
