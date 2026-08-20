---
schema_version: "1.0.0"
document_id: "8195f95d5497c58f5dd1d418a1e4dec040c67b0ef1cdf1d58cd9227abedb1d2b"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/hosted-mcp-cosmic-in-cursor-claude-and-codex-with-zero-install"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T22:15:29.906392+00:00"
content_hash: "sha256:0bd40951e7a1fa623aafa912d2aeb6839191e86e629b45ad7c12d786f90cc963"
---

# Hosted MCP: Cosmic in Cursor, Claude, and Codex with Zero Install

Cosmic now has an officially hosted Model Context Protocol (MCP) endpoint. Point any MCP-compatible AI assistant at , paste in your bucket keys, and your assistant can read, write, and generate Cosmic content in seconds. No , no Docker, no local processes to babysit.


## What's New


- **Hosted MCP endpoint.** A managed streamable-HTTP MCP server at . One URL, scoped per bucket via , multi-tenant by design.
- **Works with Cursor, Claude Desktop, and Codex CLI.** Any client that speaks MCP over streamable-HTTP can connect, including Cursor, Claude Desktop, the Codex CLI, and custom agents built on the MCP SDKs.
- **18 tools out of the box.** Object types, objects, media, plus AI text, image, video, and audio generation, all callable directly from your AI assistant.
- **Bucket-scoped bearer auth.** Authenticate with your existing Cosmic read and write keys. Read-only tools are available with a read key alone; full access just adds the write key after a colon.
- **Zero install path.** No npm package, no local runtime, no environment variables to manage. Just a URL and a header.
- **Self-hosted is still here.** The package on npm continues to ship the stdio binary for offline work or fully local setups, and is now updated to v1.2.0 with the same 18 tools.


## Why This Matters


MCP is quickly becoming the standard way AI assistants pick up real-world tools. Until now, connecting an assistant to Cosmic meant installing an npm package, juggling environment variables, and keeping a local process alive on every machine that wanted access. That's fine for one developer, but it doesn't scale to a team or to non-technical users who just want their AI editor to "see" their content.


The hosted endpoint changes that. Onboarding a new teammate is now: copy a URL, paste a bearer token, done. Your AI assistant immediately gains structured access to your Cosmic bucket, with the same per-bucket security model you already use everywhere else.


## How It Works


The hosted MCP is a streamable-HTTP transport endpoint. Each bucket gets its own URL, and authentication uses a single header you assemble from your bucket's read and write keys.


### Endpoint


```text


```


### Authentication


Cosmic uses separate read and write keys per bucket. Combine them in the header:


Access level Authorization header


Read-only tools


Full access (read + write)


Omit the colon and write key for read-only access. Write tools (, , , AI generation) return a clear error when only a read key is presented; read tools work as normal.


If your client can't include a colon-packed token, send the write key out-of-band via the header instead.


You can find your bucket slug, read key, and write key in the[Cosmic dashboard](https://app.cosmicjs.com/) under **Settings → API Access** .


## Connect Your AI Assistant


### Cursor


Add to in your project (or globally):


```text


```


### Claude Desktop


Add to your Claude Desktop config ( on macOS, on Windows):


```text


```


### Codex CLI


Add to :


```text


```


## What You Can Do


The hosted MCP exposes 18 tools across four areas:


- **Object types.** List, get, create, update, delete object types.
- **Objects.** Search, list, get, create, update, delete, and bulk-fetch your content.
- **Media.** List, upload, and delete media assets.
- **AI generation.** Generate text, images, video, and audio with Cosmic AI, all attributed to your bucket.


Every tool maps cleanly onto the Cosmic SDK, so anything your assistant does through MCP is identical to what your app can do via the API.


## Examples


- "Find every blog post tagged 'launch' that hasn't been published, summarize them, and create a draft Object summarizing what's coming up next month."
- "Upload this image to my bucket and create a new Team Member Object with the name, role, and bio I'll paste below."
- "Look at the last five Support Tickets, generate a short voiceover summary with Cosmic AI audio, and save it as a media asset."
- "Audit my Object Types: list any Metafields that aren't documented, then propose a clean schema diff."


## Self-hosted Is Still First-Class


If you'd rather keep everything local, v1.2.0 is on npm with the same 18 tools and the same code paths. Run it via and configure it with , , and environment variables. The hosted and self-hosted servers share a single source of truth, so you'll never feel a feature gap between the two.


## Try It


Hosted MCP is live for every Cosmic bucket today. Grab your keys from the[Cosmic dashboard](https://app.cosmicjs.com/) , drop the snippet above into your assistant of choice, and start using Cosmic from the same window you're already coding or chatting in.


New to Cosmic?[Create your free account](https://app.cosmicjs.com/signup) , connect your AI assistant, and ship content faster than you thought possible.


Full setup guide:[MCP Server documentation](https://www.cosmicjs.com/docs/mcp-server) .
