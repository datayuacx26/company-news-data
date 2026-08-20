---
schema_version: "1.0.0"
document_id: "f4696a5783b3b0081fda21ef458104b4bcf42b56ade2f869f6e5d6f41ac143d6"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/announcing-app-portal-mcp/"
published_at: "2026-08-11T12:00:00+00:00"
first_seen_at: "2026-08-13T03:27:41.985928+00:00"
fetched_at: "2026-08-13T03:27:43.342928+00:00"
content_hash: "sha256:830fc647f0a76f205225dd430e6cafb3b72b206bb1029f2a84a162058e03ca71"
---

# Announcing the App Portal MCP Server: Debug Webhooks with your Agent

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes for both[sending](https://www.svix.com/#sending) and[receiving them](https://www.svix.com/ingest/) . Want webhooks in your product without building the hard parts yourself?[Give it a try!](https://www.svix.com/)


I'm excited to announce the App Portal[MCP](https://modelcontextprotocol.io/) server! It lets your customers connect their coding agent to the[App Portal](https://docs.svix.com/app-portal) , so the agent can debug their integration for them. The agent reads their endpoints, their failed delivery attempts, the payloads you sent them, and the responses their own server gave back instead of them digging through a portal by hand.


## What the App Portal MCP server is


MCP is a standard way to give a coding agent access to something outside itself. A server offers a set of tools, the agent reads their descriptions, and it decides on its own which ones to call and when. So an MCP server is less an API than a set of capabilities you hand an agent and let it use.


Every screen your customers use to debug their webhooks is a tool their agent can call: list the endpoints, check an endpoint's success and failure counts, list the messages you sent, pull up the delivery attempts for one of them, and read the exact status code and response body their server returned. It can also read and edit an endpoint's[transformation](https://docs.svix.com/transformations) , resend a single message, and recover an endpoint to replay everything that failed since a given date.


And you don't need to host anything because it's a server we operate, and a session is scoped to one application by the token your customer generates, so their agent sees exactly what their portal session sees and nothing else.


## It shows up as your webhooks


Because the App Portal you embed is white-labeled, so is the MCP server.


The token carries your brand name, so the server introduces itself to your customer's agent as "Acme Webhooks" and describes itself as "Debug Acme webhook delivery". It also tells the agent when to reach for it: whenever the user mentions Acme, or when the codebase has an Acme webhook handler or Acme signature verification in it.


## How your customers connect their agent


Turn it on in your[organization settings](https://dashboard.svix.com/settings) , and a "Get MCP Token" button appears in your customers' App Portal. They click it, pick their agent, and paste what it gives them. For[Claude Code](https://docs.claude.com/en/docs/claude-code/mcp) that's one command:


```text
claude mcp  add    --transport   http acme-webhooks  "https://mcp.eu.svix.com/app/<app_id>"    \
--header    "Authorization: Bearer <YOUR_TOKEN>"


```


Cursor, VS Code, Codex, Gemini CLI, OpenCode, and Zed take the same URL and header, and the portal has ready-to-paste config for each. There's no OAuth flow to sit through; the token is the whole setup.


Then they just ask. "Why is my` invoice.paid` endpoint failing?" is enough; nobody has to learn which tool does what.


## Webhook debugging with your agent


Most webhook integration problems are small and legible: a field read from the wrong place, a handler returning a 500 on a payload it didn't expect, an endpoint quietly disabled after too many failures. But hopping to the App Portal to see attempt logs and then going back to the editor to feed your agent the new context is far from the best experience we could provide... And that's where the new App Portal MCP server fits.


We're just getting started with MCPs at Svix, and the App Portal server is the first piece. If you're curious where we think the protocol itself is heading, read[Fully Event-Driven MCP is Coming](https://www.svix.com/blog/stateless-mcp/) . Got any feedback or suggestions? Is anything unclear? Open an issue[here](https://github.com/svix/ai/issues) !


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
