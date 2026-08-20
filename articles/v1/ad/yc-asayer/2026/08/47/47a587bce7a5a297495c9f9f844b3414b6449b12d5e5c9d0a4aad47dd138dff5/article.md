---
schema_version: "1.0.0"
document_id: "47a587bce7a5a297495c9f9f844b3414b6449b12d5e5c9d0a4aad47dd138dff5"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/mdn-mcp-server/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T22:46:23.853414+00:00"
fetched_at: "2026-08-09T22:46:25.551132+00:00"
content_hash: "sha256:568120b4604b8f7b723c86f046d498245b308e5b04ba503269f1c93c531da1a8"
---

# MDN Is Now an MCP Server

The MDN MCP server is Mozilla’s official, experimental[Model Context Protocol](https://modelcontextprotocol.io/) server that gives AI coding agents and IDEs direct access to MDN’s search, documentation, and browser-compatibility data.


If you’ve ever had an AI assistant flatly tell you a web feature isn’t supported in Firefox when it actually shipped a couple of releases ago, that’s the exact headache this targets. It launched in mid-June 2026, hosted at` https://mcp.mdn.mozilla.net/` with open source at[github.com/mdn/mcp](https://github.com/mdn/mcp) . Because it arrived with almost no presence in model training data, an assistant that hasn’t been pointed at it won’t even know it exists.


This article covers what the server actually is, the training-cutoff problem it targets, the one-command setup and which clients support it, what Mozilla’s own before/after test found, and the honest caveats: it’s an experiment that stores query data and may be withdrawn at any time.


## Key Takeaways


- The MDN MCP server is Mozilla’s official first-party MCP server at` https://mcp.mdn.mozilla.net/` (source at` github.com/mdn/mcp` , MPL-2.0). It’s not one of the third-party “MDN Lookup” community servers that predate it.
- It exposes three capability areas to your AI agent: MDN search, documentation, and browser-compatibility data, all over the Model Context Protocol.
- You add it with one command:` claude mcp add --transport http mdn https://mcp.mdn.mozilla.net/` , and any MCP client can point at the same remote HTTP endpoint.
- In Mozilla’s June 2026 test on Claude Opus 4.7 against four Firefox 151 features, the model with the MCP produced accurate browser-support data where the model without it got the support right in only one of the four cases, and responses were roughly twice as fast.
- The server is explicitly experimental: Mozilla stores query data during the experiment, offers an opt-out header, and may withdraw it at any time.


## What is the MDN MCP server (and what isn’t it)?


The MDN MCP server is Mozilla’s official first-party Model Context Protocol server, and it exposes three capability areas to your AI agent:[MDN’s search, documentation, and browser-compatibility data](https://developer.mozilla.org/en-US/mcp) , delivered over MCP, the open standard for connecting AI tools to external data sources. The GitHub repository labels it a “prototype” and licenses it under MPL-2.0.


The disambiguation matters. Search for “MDN MCP” and you’ll hit unofficial community wrappers with names like “MDN Lookup” that predate this release. Those are third-party projects. The server described here is Mozilla’s own, served from the` mozilla.net` domain and maintained in the` mdn/mcp` repo under the MDN organization. In this article, “the MDN MCP server” means that first-party server, not a community reimplementation.


The server exposes those three areas but doesn’t publish stable MCP tool identifiers, so treat the capability areas as the contract, not any specific tool name your client happens to surface.


## The problem: your model’s browser-support data has an expiry date


AI coding tools carry a training cutoff, and[they can surface outdated web-platform information as a result](https://developer.mozilla.org/en-US/blog/introducing-mdn-mcp-server/) . A model may not know a newer feature exists, or may guess wrong about whether it has reached a given[Baseline](https://developer.mozilla.org/en-US/docs/Glossary/Baseline/Compatibility) status and is safe to use across browsers. Mozilla’s own example is the` @view-transition` CSS at-rule: a model may not know it exists or whether it’s Widely Available.


Browser support is exactly the kind of fact that ages badly. A feature that was behind a flag when the model was trained can ship to stable weeks later, and the model has no way to know. Feeding the agent live compat data from MDN closes that gap without you leaving your editor to check a table by hand.


## How do you add the MDN MCP server?


You add it with a single command. For[Claude Code](https://docs.claude.com/en/docs/claude-code/mcp) , Mozilla documents this exact invocation on both the announcement and the` /mcp` reference page:


```text
claude   mcp   add   --transport   http   mdn   https://mcp.mdn.mozilla.net/
```


This registers a remote HTTP MCP server named` mdn` pointing at Mozilla’s hosted endpoint. Every other MCP-compatible client follows the same pattern: add a remote HTTP MCP server at` https://mcp.mdn.mozilla.net/` through that client’s own MCP configuration. Mozilla lists the supported clients as follows:


Category Clients


Editors VS Code, Zed, Cursor


Agent CLIs Claude Code, Codex CLI, Antigravity CLI (previously[Gemini CLI](https://developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli/) )


Chat apps Claude Desktop


For VS Code, Zed, and Cursor, there’s no special JSON beyond registering that same remote HTTP endpoint in the client’s MCP settings. To confirm it’s wired up, ask your agent a browser-support question about a very recent feature and check that it answers with current MDN data rather than a hedge.


## What Mozilla’s own test found


Mozilla ran its own before/after test, so the numbers here are its measurement, not an independent one: it put[Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7) up against four features from[Firefox 151](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/151) (the version Mozilla tested; current stable is 153), with and without the MCP, asking how to use each feature and what its browser support is.


Usage notes were broadly comparable either way. Browser support was not. Without the MCP, the model got support right in only one of the four cases:


Feature (Firefox 151) Without the MCP With the MCP


` light-dark()` for images No concrete support data Full table (Firefox 150, Chrome behind a flag)


` :buffering` pseudo-class Correct Correct


` shadowrootslotassignment` on` <template>` Wrong: claimed Chrome 120 / Safari 18.3 Correct: Firefox 151 first to ship


Web Serial API Wrong: claimed not implemented in Firefox Correct: Firefox 151 ships it


Two results stand out. The model conflated the declarative` shadowrootslotassignment` attribute with the imperative` slotAssignment` option, and it flatly denied Firefox supported the Web Serial API. Mozilla also reported that responses using the MCP were roughly twice as fast, because the model queried structured data instead of fetching and parsing MDN’s HTML pages.


## The catches: it’s an experiment


Read the experimental framing plainly before you commit to it. Mozilla states that the MDN MCP server is being run as an experiment, that it stores data about the queries it receives during that experiment, and that it may be withdrawn at any time. That query data isn’t tied to identifying information, but Mozilla warns that anything private you disclose to the LLM could end up in those queries, and bluntly recommends not using the server if that’s a concern.


There are two escape hatches. To keep queries out of first-party analytics, send the` X-Moz-1st-Party-Data-Opt-Out: 1` header with requests to the MCP. Or run the server yourself:` npm install` , then` npm start` , and point your client at the local endpoint instead:


```text
claude   mcp   add   --transport   http   mdn-local   http://localhost:3002/
```


Self-hosting keeps every query on your machine, which is the cleanest answer for privacy-sensitive work.


## Where this leaves you


Add the MDN MCP server if your AI assistant regularly misstates browser support and you can accept an experimental dependency that Mozilla may pull; self-host it if query privacy matters. It’s a small, one-command change that swaps guesswork about compat data for MDN’s live tables, and it targets a failure mode every training-cutoff model shares. Wire it into your editor or agent, ask it about a feature that shipped this month, and check the answer against the current MDN page.


## FAQs


Why not just let the AI agent read MDN pages directly instead of using the MCP server?


The MCP server returns structured search, documentation, and compatibility data, while direct reading forces the agent to fetch and parse MDN's full HTML pages. In Mozilla's own test, the direct-fetch approach was slower and still produced wrong browser-support answers, whereas MCP responses were roughly twice as fast because the model queried structured data instead of scraping markup. The MCP surface is purpose-built for compat lookups, so it returns clean tables rather than page bloat.


Is the MDN MCP server free to use?


Yes. Mozilla hosts the MDN MCP server at 'https://mcp.mdn.mozilla.net/' with no listed cost, and the source is open under the MPL-2.0 license at github.com/mdn/mcp. You can also self-host it for free with 'npm install' and 'npm start', pointing your client at 'http://localhost:3002/'. Note it is explicitly experimental and may be withdrawn at any time, so free access is not a guaranteed permanent service.


Does the MDN MCP server store my queries, and can I stop it?


Mozilla stores data about the queries it receives while the server runs as an experiment, and it is not tied to identifying information, but anything private you type to the LLM can end up in those queries. To keep queries out of first-party analytics, send the header 'X-Moz-1st-Party-Data-Opt-Out: 1' with requests. For full isolation, run the server locally so every query stays on your own machine and never reaches Mozilla.


How do I know if my AI agent is actually using the MDN MCP server?


Ask it a browser-support question about a feature that shipped very recently, then check whether the answer reflects current MDN compatibility data rather than a hedge or an outdated guess. A model without the MCP typically denies newer features exist or misstates which browser versions support them, as Mozilla saw with the Web Serial API and shadowrootslotassignment. Accurate, version-specific compat answers citing current MDN data indicate the connection is working.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
