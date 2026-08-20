---
schema_version: "1.0.0"
document_id: "8da05c372c1ac5de504c46b6b19db5bd7c49f59e314495839bb9eca775fa7407"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/webmcp/"
published_at: "2026-08-06T13:00:00+00:00"
first_seen_at: "2026-08-06T13:03:06.847303+00:00"
fetched_at: "2026-08-06T13:03:08.318505+00:00"
content_hash: "sha256:fab6191415d5320d99bd03c57cfaab41f3b6d5db4361f675281aa453e4508896"
---

# Give any website a WebMCP interface

Today we are launching a developer preview of WebMCP on Cloudflare. Switch it on and browser agents can start working with your site, with no code and nothing changed at your origin. Cloudflare adds a small bridge to your pages, which registers a set of tools for a visitor’s agent to use.


The web was built on the assumption that there is a person on the other end: someone to read the page, click buttons, and fill in the forms. But now more and more visits come from AI agents instead, to an Internet made for humans. The usual approach has been crawlers, which copy content back to a server and, too often, give the original site none of the traffic and little of the credit. There is a better way, and it does not involve scraping.


WebMCP is a new browser standard, shipping experimentally in Chrome 146, that shows up in the page as` document.modelContext` . A site can choose to expose a set of tools for agents running in the browser, meaning agents no longer have to guess their way through a page built for humans. This enables agents to have a different browsing experience from the user and use tokens on tasks, not navigation. The catch: the site has to implement it.


Cloudflare has been building both ends of this.[BrowserRun](https://developers.cloudflare.com/browser-run/) , our remote browser, already added WebMCP support, so an agent can discover and call the tools a site exposes.[Cloudflare Radar](https://radar.cloudflare.com/) will soon offer WebMCP tools of its own. This preview is about the other side: a way to give any site on Cloudflare those tools with a single switch, and no code.


## A developer preview of WebMCP on Cloudflare


Implementing WebMCP by hand is a small project: design the tools to expose, wire them into your interface, and keep them working as the standard evolves. We wanted it to be simpler than that: just toggle a setting to enable tools.


These tools come in packs — groups of related tools that can be turned on together. These are built to grow: as we add packs, a site can opt in to more just by turning them on, no redeploy needed. We are including two tool packs in this developer preview, which both run entirely in the browser.


## What this does and how it works


Our implementation comprises two parts, both in front of your origin. Neither touches your site’s code and both work the same way whether your site is static or a single-page app.


First, an injection at the edge. When your site has WebMCP switched on in your Cloudflare Dashboard, we use HTMLRewriter to add one line to each HTML response: a small reference to a bridge script that we also serve. Both the tag and script it loads come from the edge, same origin, so nothing else about the page changes:


```text
<!-- Cloudflare injects this at the edge. Same origin, and your HTML is otherwise untouched. -->
<  script   type  =  "module"
src  =  "/.webmcp/bridge.js"
data-packs  =  "c2pa,mcp-server-client"
data-mcp-url  =  "/mcp"  ></  script  >
```


The` data-packs` attribute is the list of packs to activate. If you have an existing Model Context Protocol (MCP) server, the` data-mcp-url` points at your own MCP server (defaulting to the same origin` /mcp` ).


Second, the bridge. This runs in the page and finds the WebMCP surface. If the browser does not have one, it returns and does nothing, so the page behaves exactly as before.


From there, the bridge composes the packs named in` data-packs` into one tool list and registers each with` .registerTool` . A pack is just a set of MCP tool descriptors and their handlers. Static packs, such as Content Credentials, declare their tools up front. A dynamic pack, such as the Site MCP Server pack, discovers its tools at boot before registering anything.


In this preview, every tool runs entirely in the visitor’s browser. There is no round trip to a server of ours. The Content Credentials pack fetches an image and parses its first few kilobytes of content provenance metadata locally. The Site MCP Server pack talks straight to your MCP server endpoint from the page, on the visitor's origin and with their existing session.


The bridge code is served by a worker running at the edge. This leaves us room to grow the offering — future packs will be able to call this worker for tasks the page cannot do alone, like summarizing a sitemap with Workers AI or querying an AI Search index.


To an agent, all of these are ordinary MCP tools. We use Model Context Protocol’s own` Tool` and` CallToolResult` types, so an agent that already talks to MCP servers can drive a page with nothing special added. The browser is just another place MCP runs. The example below shows how the bridge turns one of your own MCP tools into a tool the visitor’s agent can call.


```text
// For each tool the site's own MCP server advertises (via tools/list),
// registering a proxy whose execute() calls the site back on the
// visitor's origin, with their session.
document.modelContext.  registerTool  ({
name: tool.name,                   // e.g. "search_products"
description: tool.description,
inputSchema: tool.inputSchema,     // taken straight from tools/list
execute  :   async   (  args  )   =>   {
const   res   =   await   fetch  (mcpUrl, {     // same-origin /mcp
method:   "POST"  ,
credentials:   "same-origin"  ,
headers: {   "content-type"  :   "application/json"   },
body:   JSON  .  stringify  ({
jsonrpc:   "2.0"  , id:   1  , method:   "tools/call"  ,
params: { name: tool.name, arguments: args },
}),
});
const   {   result   }   =   await   res.  json  ();
return   result;     // an MCP CallToolResult, passed straight through
},
});
```


## Checking out content metadata


We are also developing packs to read different types of metadata. For example, credentials for participants of the[C2PA](https://c2pa.org/) program can be retrieved using the Content Credentials pack.` scan_images_c2pa` sweeps every image and returns a short summary of each:


```text
{
"imageCount"  :   12  ,
"scanned"  :   12  ,
"withC2pa"  :   8  ,
"results"  : [
{
"src"  :   "https://example.com/hero.jpg"  ,
"hasC2pa"  :   true  ,
"format"  :   "image/jpeg"  ,
"manifestCount"  :   1  ,
"claimGenerator"  :   "Adobe Firefly"  ,
"title"  :   "sunrise over the bay"  ,
"signedBy"  :   "Adobe Inc."
},
{   "src"  :   "https://example.com/logo.png"  ,   "hasC2pa"  :   false  ,   "format"  :   "image/png"   }
]
}
```


For a closer look,` inspect_image_c2pa` decodes one image’s full manifest: its edit history, the stated author, and the signing certificate. It is a plain TypeScript reader that touches only a few kilobytes of the metadata at the front of the image, not the image itself. For now, it reads and reports the credential, rather than cryptographically verifying it: every result carries` signatureVerified: false` , so an agent won’t mistake a decoded claim for a checked one.


## Try it out


Get started with WebMCP by going to[Agent Readiness > Labs in the Cloudflare Dashboard](https://dash.cloudflare.com/?to=/:account/:zone/agent-readiness/labs) . Here you can toggle on WebMCP for a domain, and pick which packs to add: both Content Credentials and Site MCP Server are on by default, and more packs will show up here as we ship them. That's the whole setup. There's nothing to deploy and nothing to change at your origin, and the next HTML your site sends will include the bridge.


To confirm it’s live, ask your site for any HTML page and look for the line Cloudflare injected:


```text
curl   -  s https  :  //your-site.example | grep webmcp
```


You do not need your own agent to see the tools work. Point[BrowserRun](https://developers.cloudflare.com/browser-run/features/webmcp/) , Cloudflare’s remote browser, at your URL, and it will discover and call the tools your packs registered, exactly as a visitor’s agent would. That is the whole loop: BrowserRun gives agents a browser to act on, this preview gives your site the tools to be acted on, and they meet using the open standard. The tools behave in the same way whether the browser is on someone’s laptop or running headless in the cloud.


## Why we built this


Our job is to help make the Internet better, and as the Internet changes we need to provide domain owners with tools that allow new visitors, AI agents, to interact without a full rebuild. It is one step towards a web that can still thrive when visitors are not always human.


This is a developer preview, and we want your feedback. Turn it on, try it against your own site, and tell us how it goes in the[Cloudflare Developers Discord](https://discord.cloudflare.com/) or on the[Community forum](https://community.cloudflare.com/) .
