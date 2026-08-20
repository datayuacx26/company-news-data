---
schema_version: "1.0.0"
document_id: "39c488e3df87515e55e58174ef15bf738dfde2d25d9a2c5c5461a5eecd0b4634"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/best-tools-to-integrate-external-apis-with-ai-coding-agents/"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T05:26:32.018631+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:ca6fe960e3fbdb91a9898be6a94aef6b7ea3f2bad7a7ee793e0d3a2c26528176"
---

# Best tools to integrate external APIs with AI coding agents in 2026

## TL;DR


Coding agents like Claude Code, Cursor, and Codex write integration code well but ship broken API integrations on their own: they work from stale training data, invent endpoints, and have no way to run OAuth or test against the real API. You can fix that by configuring a set of supporting tools: docs-context MCP servers, agent skills, auth and testing utilities, and an integration platform that gives the agent real connections to build against.


We build API integrations with coding agents every day at Nango, and this list is the toolbox that makes that work. The focus is customer-facing API integrations, where your users connect their own Salesforce, HubSpot, or Slack accounts to your product, not editor productivity plugins. Every entry is something you configure for the agent, and they work together in one workflow.


Tool What it gives the agent Best for Pricing and license


Nango Managed auth, real-API testing, and a runtime across 900+ APIs, via skills for all coding agents Building production integrations end to end Free tier, paid from $50/mo; source-available (ELv2)


Context7 Up-to-date library and SDK docs in the prompt Stopping stale-docs hallucinations Free 1,000 calls/mo, Pro $10/seat/mo; MIT server, closed backend


Official provider MCP servers (GitHub, Atlassian, AWS) Build-time operations on the provider: repos, issues, docs, infra Working with providers you already use Free; MIT / Apache 2.0


Agent skills and the skills CLI Procedural knowledge: how to build an integration, step by step Encoding repeatable workflows across agents Free; open standard (agentskills.io)


Apidog MCP server Your OpenAPI spec served as agent context Generating code against a known API contract Free; open source


Speakeasy Gram Turns an OpenAPI spec into hosted MCP tools API producers exposing their own API to agents Free tier (1,000 tool calls); open source


RedirectMeTo, ngrok, mkcert Working OAuth redirects on localhost Testing OAuth flows during development Free tiers; MIT / BSD


## What it takes for a coding agent to ship an API integration


Prompting an agent to “build a HubSpot integration” fails without scaffolding, and the failure points are predictable. A production API integration needs four things that the agent does not have out of the box:


- **Current API knowledge:** Models work from training data that lags the real API. In a 2025 benchmark of LLMs on web API integration tasks, models[hallucinated endpoint URLs up to 39% of the time and parameter names up to 31% of the time](https://arxiv.org/abs/2509.20172) . Docs-context tools close this gap.
- **Auth and credentials:** The agent cannot click through an OAuth consent screen, register an OAuth app, or store and refresh tokens. Someone has to hand it an authorized connection, and[OAuth is full of provider-specific quirks](https://nango.dev/blog/why-is-oauth-still-hard) .
- **A test loop against the real API:** Code that compiles is not an integration that works. The agent needs to execute its code against a real connection, read the actual response or error, and iterate.
- **A runtime and maintenance path:** Token refresh, webhooks, retries, pagination, rate limits, and observability have to live somewhere after the agent is done writing code.


The seven tools below cover those four jobs. Most are complementary: a real setup uses three or four of them together, and the usage example at the end shows the full workflow.


## 1. Nango


[Nango](https://nango.dev/) is the integration platform where coding agents build API integrations. Your agent writes integrations as code functions, tests them against real connections, and deploys them to a managed runtime that handles auth, syncs, webhooks, and tool calls across[900+ APIs](https://nango.dev/api-integrations) with 5,000+ reusable templates.


- **Pricing:** free tier (10 connections included), paid from $50/month
- **License:** source-available (Elastic License 2.0), self-hostable
- **Works with:** All coding agents, including Claude Code, Cursor, Codex, Gemini CLI, and OpenCode
- **Install:**` npx skills add NangoHQ/skills -s building-nango-functions-locally` for repo-based builds; a remote Function Builder skill builds with no local project. The[coding agent setup guide](https://nango.dev/docs/getting-started/coding-agent-setup) explains which fits your use case.


**Best for**


Teams whose coding agent should ship the whole integration: research the API, write the code, test it against a real connection, and deploy it to a runtime their product consumes through an MCP server or REST.


**Pros**


- **One skill covers the full build loop:** The[Nango skill](https://nango.dev/docs/guides/functions/functions-guide) gives the agent the integration patterns (auth, actions, syncs, webhooks) plus a test command.` nango dryrun` executes generated code against a real connection, so the agent iterates on real API responses instead of guessing.


- **Managed auth the agent cannot build:** A drop-in UI handles OAuth, API keys, JWT, basic auth, and[MCP Auth](https://nango.dev/docs/guides/auth/mcp-auth) across 900+ APIs. Your users authorize under your brand, and the agent only ever sees a connection ID.
- **Docs built for agents:** A public[docs MCP server](https://nango.dev/docs/getting-started/coding-agent-setup) ,[llms.txt](https://nango.dev/docs/llms.txt) , and an agent-readable API catalog, so the agent grounds itself without scraping.
- **Integrations built from a prompt alone:** The remote function builder (shipped June 1, 2026) compiles, dry-runs, and deploys over REST with no local project. This is what enables[just-in-time integrations](https://nango.dev/blog/just-in-time-integrations) , where an agent inside your product builds an integration that a customer asked for.
- **The runtime is included:** The deployed integration runs on a tenant-isolated runtime with data syncs, webhook ingestion,[full request and response logs, and OpenTelemetry export](https://nango.dev/docs/guides/platform/observability) . Your product’s agents consume it through a hosted MCP server with schema-validated tools, or REST.


**Cons**


- **Needs a Nango account:** The free tier is enough to build and test, but the workflow assumes the Nango platform end to end.
- **Built for developers:** Integrations are code in a repo. Teams that want a drag-and-drop builder are better served by a visual embedded iPaaS.


## 2. Context7


[Context7](https://github.com/upstash/context7) , by Upstash, is the most widely used docs-context MCP server. It resolves a library name to an indexed ID, then injects current, version-specific documentation and code examples into the agent’s prompt. It is mainly used to stop the agent from hallucinating deprecated or nonexistent library APIs.


- **Pricing:** free (1,000 calls/month), Pro $10/seat/month for 5,000 calls
- **License:** MIT (MCP server); the crawling backend is closed source
- **Works with:** Claude Code, Cursor, Codex, and most MCP clients, as an MCP server or a skill
- **Install:**` npx ctx7 setup` , or the remote server at` mcp.context7.com/mcp` with an API key


**Best for**


Any agent writing code against SDKs and libraries that change faster than model training data: Next.js, Stripe, Supabase, provider SDKs, and similar.


**Pros**


- **Solves the stale-docs problem for LLMs:** Version-specific docs arrive in the prompt at generation time. Community endorsement is unusually strong; a typical[Hacker News assessment](https://news.ycombinator.com/item?id=44889785) called it “so good it seems like magic.”
- **Low setup cost:** One command installs it across your agents, and it stays out of the way until the agent queries a library.


**Cons**


- **Indexing lags bleeding-edge releases:** Users report the indexed docs trail brand-new versions by days, which is exactly when you need them most.
- **The free tier shrank:** In January 2026, the free allowance dropped to 1,000 calls per month (from roughly 6,000), so using it across a team effectively requires the paid tier.
- **A hosted dependency with a security history:** Every query routes through Upstash. In March 2026, researchers disclosed[ContextCrush](https://noma.security/blog/contextcrush-context7-the-mcp-server-vulnerability/) , a prompt-injection vulnerability where unsanitized library “Custom Rules” reached every querying agent. Upstash patched it within days, but docs-context servers are part of your supply chain.


If Context7 does not fit,[Ref.tools](https://github.com/ref-tools/ref-tools-mcp) optimizes for token-efficient docs search, and[DeepWiki MCP](https://docs.devin.ai/work-with-devin/deepwiki-mcp) (by Cognition) answers questions over 50,000+ pre-indexed public GitHub repos for free.


## 3. Official provider MCP servers (GitHub, Atlassian, AWS)


When the provider you are integrating with ships an official MCP server, your agent can operate that provider directly while it builds: read API docs, inspect repos, file issues, provision infrastructure. Three matter most in 2026:


- [GitHub MCP server](https://github.com/github/github-mcp-server) : MIT-licensed, remote server GA since September 4, 2025 with OAuth 2.1, plus a local Docker option. 50+ tools across 23 toolsets covering repos, issues, pull requests, and actions.
- **Atlassian Rovo MCP server:** GA since February 4, 2026, hosted at` mcp.atlassian.com` . Exposes Jira, Confluence, and Bitbucket Cloud with tool groups that respect existing user permissions.
- **AWS MCP servers:** The[awslabs suite](https://github.com/awslabs/mcp) has 40+ Apache 2.0 servers (documentation, CDK, cost analysis), and the managed AWS MCP Server (GA May 6, 2026) exposes 15,000+ AWS API operations through a small fixed toolset, free.


**Best for**


Build-time operations on providers your team already uses: the agent researches the API, checks examples, manages the repo and tickets, and provisions the infrastructure the integration will run on.


**Pros**


- **Maintained by the provider:** Official servers track API changes, and auth is first-party OAuth rather than a scraped token in a config file.
- **Tool-context discipline is improving:** GitHub cut its default toolset from 101 tools to 52 in October 2025, and a January 28, 2026 update added OAuth scope filtering plus dynamic toolsets that start with 4 tools.


**Cons**


- **Context cost is still real:** One user[measured Claude Code’s context jumping from 34,000 to 80,000 tokens just from enabling the GitHub MCP server. Enable the minimal toolsets you need.](https://github.com/github/github-mcp-server/issues/1286)
- **They authenticate you, not your customers:** These servers run on the developer’s own OAuth token or PAT. They are build-time tools. None of them handles multi-tenant end-user auth, token refresh, or per-customer API calls in your shipped product.


If your product needs official MCP servers at runtime, Nango covers that side too: its catalog includes MCP-type integrations like HubSpot MCP,[MCP Auth](https://nango.dev/docs/guides/auth/mcp-auth) manages your users’ connections to them, and a generic MCP integration connects any spec-compliant server, all through the same auth and runtime as regular API integrations.


## 4. Agent skills and the skills CLI


A skill is a folder with a` SKILL.md` file that teaches an agent a workflow: instructions, scripts, and references it loads when relevant. Anthropic[introduced Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) on October 16, 2025 and published the format as an open standard on December 18, 2025 at[agentskills.io](https://agentskills.io/) . OpenAI added skills to Codex in December 2025, and the standard’s client list spans 40+ products, including Cursor and Gemini CLI.


- **Pricing:** free
- **License:** open standard; individual skills carry their own licenses
- **Works with:** 40+ agents and clients via one` SKILL.md` format
- **Install:**` npx skills add <owner/repo>` (the[skills CLI](https://vercel.com/changelog/introducing-skills-the-open-agent-skills-ecosystem) by Vercel, announced January 20, 2026)


**Best for**


Encoding the procedure of integration work once, so every agent on the team follows it: which auth pattern to use, how to paginate, how to test.


**Pros**


- **One artifact, every agent:** The same skill works in Claude Code, Codex, and Cursor, so the workflow does not depend on which agent a teammate runs.
- **Cheap on context:** Skills load progressively; Codex budgets around 2% of the context window for the skills list. Hence the community rule of thumb: skills for defined workflows, MCP for live data.
- **Good API-integration skills exist:** Anthropic’s[mcp-builder](https://github.com/anthropics/skills) scaffolds an MCP server that wraps an API in TypeScript or Python. Nango’s skills (covered above) go further and produce a deployed, tested integration.


**Cons**


- **Quality varies wildly:** The skills.sh registry tracks installs, not correctness, and anyone can publish.
- **A skill alone still guesses:** Instructions without a real-API test loop do not fix the hallucinated-endpoint problem. With mcp-builder, for example, you still host, auth, and maintain the resulting server yourself.


## 5. Apidog MCP server


The[Apidog MCP server](https://docs.apidog.com/conntect-api-specification-within-apidog-project-to-ai-via-apidog-mcp-server-901476m0) feeds an API specification to your coding agent: an Apidog project, a published docs site, or any plain OpenAPI/Swagger file. The agent then generates requests and types against the actual contract instead of its memory of it.


- **Pricing:** free (the Apidog platform has a free plan for up to 4 users)
- **License:** open source (MIT)
- **Works with:** Cursor, Claude Code, VS Code, and other MCP clients
- **Install:**` npx apidog-mcp-server@latest` with a spec path or project token


**Best for**


Teams that have (or can obtain) an OpenAPI spec for the API they are integrating: internal services, partner APIs, or well-documented public providers.


**Pros**


- **Grounds generation in the real contract:** Field names, required parameters, and response shapes come from the spec, which removes the most common class of integration bugs.
- **Works with bare OpenAPI files:** You do not need to adopt the Apidog platform; pointing it at a local or remote spec file is enough.


**Cons**


- **Only as good as the spec:** Long-tail SaaS APIs often publish incomplete or outdated OpenAPI files, and many publish none.
- **No execution layer:** The agent knows the contract but still cannot authenticate or verify behavior against the live API.


## 6. Speakeasy Gram


Gram, by Speakeasy, turns an OpenAPI document into a hosted MCP server with curated toolsets. It sits on the producer side of the ecosystem: if you want your own API to be callable by ChatGPT, Claude, and coding agents, Gram is the fastest path.


- **Pricing:** free tier with 1,000 tool calls, then usage-based
- **License:** open source, with a hosted service
- **Works with:** any MCP client
- **Install:** upload an OpenAPI spec, curate the toolset, get a hosted MCP URL


**Best for**


API producers who want a maintained, hosted MCP server for their own API without building one by hand.


**Pros**


- **Spec in, hosted MCP server out:** Upload an OpenAPI document and Gram hosts the resulting server, so you skip building and operating one yourself.
- **Curation is first-class:** Toolset curation and enriched descriptions are part of the workflow. Speakeasy’s own guidance notes that naive one-tool-per-endpoint conversion underperforms small, curated toolsets.


**Cons**


- **The consumer side stays unsolved:** Generating an MCP server for an API you consume still leaves auth, multi-tenant credentials, and maintenance with you. For consuming many external APIs, an integration platform is the broader tool.


## 7. RedirectMeTo, ngrok, and mkcert (OAuth on localhost)


Many providers reject` http://localhost` OAuth redirect URLs (Slack, TikTok, and Microsoft OneDrive among them), which blocks the agent’s test loop before it starts. Three utilities fix it, and we cover them in depth in[3 easy ways to do OAuth redirects on localhost](https://nango.dev/blog/oauth-redirects-on-localhost-with-https) .


- **Pricing:** RedirectMeTo is free; ngrok has a free tier, then $8/month (Hobbyist, billed annually); mkcert is free
- **License:** MIT (RedirectMeTo), proprietary (ngrok), BSD-3-Clause (mkcert)
- **Works with:** any OAuth provider and any local stack


**Best for**


Developing and testing OAuth flows locally, so the agent (and you) can complete a real authorization against a real provider during development.


**Pros**


- **RedirectMeTo is zero-setup:** Prepend` https://redirectmeto.com/` to your localhost callback URL and register that as the redirect URL. It forwards the redirect with query parameters intact.
- **ngrok gives you a stable HTTPS tunnel:** Every free account includes one static dev domain, which doubles as a webhook receiver while you test event-driven integrations.
- **mkcert makes` https://localhost` real:** It installs a local CA and mints trusted certificates, no cloud dependency involved.


**Cons**


- **Trust and limits:** A redirect service can see your authorization code in transit, ngrok’s free tier has an interstitial page and tight quotas, and mkcert’s last release was April 2022 (it still works, but it is in maintenance mode).
- **This solves redirects only:** Token storage, refresh, and multi-tenant auth remain yours. With a managed auth layer you skip the problem, since the redirect URL is the platform’s hosted HTTPS endpoint.


## Usage example: building a HubSpot sync with Claude Code and Nango


Here is how the toolbox works together on a real task: syncing HubSpot contacts into your product.


1. **Set up auth once:** Create a HubSpot integration in the Nango dashboard and add a test connection by completing the OAuth flow. No localhost redirect workarounds needed, because the redirect URL is Nango’s hosted endpoint.


1. **Give the agent its tools:** Install the Nango skill (` npx skills add NangoHQ/skills -s building-nango-functions-locally` ). The skill points the agent at Nango’s docs MCP server, and Context7 covers any SDK questions along the way.
2. **Prompt at the level of intent:**


#####


```text
/building-nango-functions-locally Build a Nango sync that fetches all HubSpot
contacts. Backfill the full contact list on first run, then fetch only new and
updated contacts incrementally. Return id, email, first name, last name, and
last modified date.
```


1. **The agent builds and tests against the real API:** It researches the HubSpot API, writes the sync with pagination and checkpoints, runs` nango dryrun` against your test connection, reads real responses, and iterates until the sync passes.
2. **Deploy and consume:**` nango deploy dev` ships the sync to the runtime. Your product reads synced records over REST, your users connect their own HubSpot accounts through the white-label auth UI, and the agent reads execution logs when something needs a fix.


Docs tools ground the agent, the skill encodes the procedure, and the platform supplies credentials, a test loop, and a runtime. For a deeper walkthrough, see[how to sync large amounts of contacts from the HubSpot API](https://nango.dev/blog/how-to-sync-large-amounts-of-contacts-from-hubspot-api) .


## Keep the toolbox small


Every MCP server you add loads its tool definitions into every conversation. Anthropic measured a typical five-server setup at[around 55,000 tokens of tool definitions](https://www.anthropic.com/engineering/advanced-tool-use) before the conversation starts, and Claude Code added MCP tool search in January 2026 to load definitions on demand instead.


Two rules of thumb from teams running this in production:


- **Install only what the task needs:** Three or four tools from this list cover an integration project. Add a docs-context server, the provider’s official MCP server if one exists, one integration skill, and the platform. Skip the rest until a task demands them.
- **Treat MCP servers as dependencies:** In September 2025, the[postmark-mcp npm package](https://www.koi.ai/blog/postmark-mcp-npm-malicious-backdoor-email-theft) shipped an update that BCC’d every email to an attacker. Prefer official servers, pin versions, and keep production credentials out of the agent’s environment.


## FAQ


### Can Claude Code build API integrations?


Yes. Claude Code can build production API integrations when paired with a skill that encodes integration patterns and a way to test against the real API. Without those, models hallucinate endpoint URLs up to[39% of the time](https://arxiv.org/abs/2509.20172) . With the Nango skill, Claude Code researches the API, writes the integration, tests it against a real connection with` nango dryrun` , and deploys it. The same applies to Cursor, Codex, and other agents.


### What MCP servers do coding agents need to integrate external APIs with your product?


Three kinds: a docs-context server (Context7 or an OpenAPI-spec server like Apidog’s) so the agent stops guessing endpoints, the provider’s official MCP server (GitHub, Atlassian, AWS) for build-time operations, and the integration platform’s server (Nango’s docs MCP server for grounding, and its hosted MCP server for the tools your product’s agents call at runtime). Keep the total small; five servers can cost around 55,000 tokens of context before any work starts.


### How do AI coding agents handle OAuth?


They don’t, directly: an agent cannot register an OAuth app or click through a consent screen. You either complete the flow yourself during development (with RedirectMeTo or ngrok solving the localhost redirect problem) or use a platform like Nango, where a human authorizes a test connection once and the agent builds and tests against that connection ID. In production, end users authorize through a hosted auth flow, and tokens stay hidden from your agent.


### Should I give my coding agent a skill or an MCP server?


Use a skill for a defined, repeatable workflow and an MCP server when the agent needs discoverability or live data. Skills load on demand and cost almost nothing in context, while MCP servers front-load their tool definitions into every conversation. For API integration work, the strongest setup is one integration skill plus one or two MCP servers for docs and provider access. For the full comparison of integration skills, see[3 best API integration skills for Claude and Codex](https://nango.dev/blog/best-api-integration-skills-for-claude-and-codex) .


## Conclusion


The gap between writing code and shipping API integrations is tooling, and in 2026, the toolbox has settled into clear layers: docs context (Context7, Apidog), procedure (agent skills), build-time provider access (official MCP servers), auth and testing utilities (RedirectMeTo, ngrok), and an integration platform that supplies real connections, a test loop, and a runtime.


Nango is the only tool that covers the last layer end to end, which is why the rest of the toolbox fits around it: the agent grounds itself with docs tools, follows a skill, and builds on Nango’s auth, testing, and runtime across 900+ APIs. To try the workflow, follow the[coding agent setup guide](https://nango.dev/docs/getting-started/coding-agent-setup) with your favorite coding agent.


## Related reading


- [Best API integration platforms to use with Claude Code, Cursor, and Codex (2026)](https://nango.dev/blog/best-api-integration-platforms-claude-code-cursor-codex)
- [3 best API integration skills for Claude and Codex in 2026](https://nango.dev/blog/best-api-integration-skills-for-claude-and-codex)
- [Best MCP servers for agent API integrations in 2026](https://nango.dev/blog/best-mcp-servers-for-agent-api-integrations)
- [Using AI coding agents for building API integrations in 2026](https://nango.dev/blog/using-ai-coding-agents-for-building-api-integrations)
- [3 easy ways to do OAuth redirects on localhost (with HTTPS)](https://nango.dev/blog/oauth-redirects-on-localhost-with-https)
