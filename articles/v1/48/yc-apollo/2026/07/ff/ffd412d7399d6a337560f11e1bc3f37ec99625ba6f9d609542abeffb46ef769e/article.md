---
schema_version: "1.0.0"
document_id: "ffd412d7399d6a337560f11e1bc3f37ec99625ba6f9d609542abeffb46ef769e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graph-health-checks-now-a-prompt-away"
published_at: "2026-07-21T12:03:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:1909d42f48f91e22a1b4d59290ba775bd9354871a60128d5a02639a93619928a"
---

# Graph Health Checks, Now a Prompt Away

Answering “is my graph healthy?” used to mean a tour across half a dozen[Studio](https://www.apollographql.com/graphos/studio) pages: launch history, composition errors, lint reports, router status, operation latency, and error rates.


Today we’re launching **GraphOS MCP Server** , Apollo’s hosted MCP server that connects your AI agent directly to GraphOS. Point it at your graph and your agent can read launches, schema checks, router configuration, and traffic metrics. Each tool answers one focused question, and an agent can combine them to answer bigger ones like:


Is my graph healthy? Run the relevant checks and summarize findings as Attention Required, Review Recommended, or What’s Working Well.


## One endpoint, zero infrastructure


GraphOS MCP Server lives at https://mcp.apollographql.com. There is nothing to install, deploy, or upgrade. If your client speaks MCP over Streamable HTTP, you’re two minutes from a working setup.


It’s a built-in connector in Claude and Claude Desktop. For Claude Code, it’s one command:


```text
claude mcp add graphos https://mcp.apollographql.com \
--transport http --header 'x-api-key: YOUR_GRAPH_API_KEY'
```


Cursor, VS Code, Codex CLI, Gemini CLI, and GitHub Copilot are covered too. The[installation guide](https://www.apollographql.com/docs/graphos/platform/graphos-mcp-server/install) has the exact configuration for each.


If you’ve used this as GraphOS MCP Tools before for docs search or the Connectors spec, that’s the same endpoint under a new name, GraphOS MCP Server, now with graph-aware tools too. The documentation and Connectors tools still need no key. What’s new is the graph-aware tools below, which run once you bring a graph API key.


## Meet the graph-aware tools


With a graph API key in the` x-api-key` header, your agent gets a set of tools that map to the questions you’d actually ask about a production graph:


**What is my graph?** The *GetMyIdentity* tool resolves the caller’s identity from the API key, so the agent can discover your graph and its variants without you spelling out IDs.


**What is this variant running?** *The GetVariantDetails* tool returns the federation version, subgraph inventory, router status, and router configuration in one call.


**Are deployments stable?** The *GetLatestLaunch* , *GetLaunch* , and *GetLaunchHistory* tools expose launch status, composition errors, and schema diff summaries, so the agent can spot repeated failures or a suspicious streak of superseded launches.


**Is traffic healthy?** The *GetTopOperations* , *GetOperationMetrics* , and *GetSubgraphMetrics* tools surface request counts, p50/p99 latencies, and error counts as daily buckets, ranked by whichever metric matters: busiest, slowest, or most error-prone.


**Is quality slipping?** The *GetLintResults* tool pulls schema lint violations from recent checks, and the *GetPersistedQueryListStatus* tool tells you whether a variant has a PQL configured. A production variant without one may be a security gap worth flagging.


Each tool is useful on its own. The real payoff is chaining: the agent resolves your graph from the key, pulls the latest launch, checks lint results, cross-references top operations against their latency and error metrics, and assembles the picture a human would have built from six browser tabs.


## Mix and match


The tools are useful one at a time, and they’re more useful together. Because each returns focused, structured data, an agent can feed the result of one into the next and answer questions no single tool covers:


- **Cross-reference traffic with health.** Pull the busiest operations, then their latency and error metrics, and flag any high-traffic operation that’s also slow or failing.
- **Investigate a bad deploy.** Start from the latest launch, spot a composition error, then walk the launch history to see whether it’s a one-off or a pattern.
- **Run a full health check.** Combine variant details, launches, lint, and metrics into one prioritized report, the “is my graph healthy?” prompt from the top of this post.


You stay in control of the depth. Ask a narrow question and the agent calls a single tool; ask a broad one and it composes as many as it needs.


## Built for agent context, not for dashboards


Raw platform data is designed for dashboards, not context windows. A month of per-operation timeseries data as JSON runs to hundreds of kilobytes. Feed that to an agent and you’ve burned the context budget before the analysis starts.


So the metrics tools return compact, ranked CSV instead: top operations or subgraphs, ordered by the metric you care about, one row per day. The same month of data lands in about a kilobyte. The agent spends its context on reasoning about the numbers rather than parsing them.


This is the same philosophy behind[smart schema discovery](https://www.apollographql.com/blog/apollo-mcp-server/smart-schema-discovery-how-apollo-mcp-server-maximizes-ai-context-efficiency) in Apollo MCP Server: give the agent exactly what it needs in the smallest faithful form, and it gets more done with fewer tokens.


## Scoped by your key, by design


The security model is deliberately boring. Your API key travels with each request and is forwarded to the GraphOS Platform API, so every tool call is evaluated against what that key is allowed to see. The server adds no ambient permissions on top.


The graph tools accept graph API keys, not personal keys, and we recommend a least-privilege key: an **Observer** -role key can run every health check above. Your agent gets read-only eyes on the graph and nothing else.


## Try it on your own graph


Connect with your client of choice, then start with something small:


- “Show the federation version, subgraphs, and router status for my production variant.”


- “Have the recent launches on my graph been stable, or were there repeated failures?”


- “Which of my subgraphs have the highest error rates this month?”


Then ask the big one:


Scan my graph for issues across schema composition, lint, router version and config, and deployment stability, and prioritize what needs attention.


GraphOS MCP Server is available now at[https://mcp.apollographql.com](https://mcp.apollographql.com/) . The[tool reference](https://www.apollographql.com/docs/graphos/platform/graphos-mcp-server/tools) documents every tool your agent can call, with arguments and example prompts. If you have feedback, ideas, or a check you wish your agent could run, tell us in the[Apollo Community Forum](https://community.apollographql.com/) . This launch is the first batch of graph-aware tools, and what we build next depends on what you ask your agents to do.


Written by


Dale Seo


[Read more by Dale Seo](https://www.apollographql.com/blog/author/dale-seo)
