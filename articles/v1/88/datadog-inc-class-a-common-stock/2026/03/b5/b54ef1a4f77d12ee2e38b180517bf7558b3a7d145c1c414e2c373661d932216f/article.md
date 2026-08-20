---
schema_version: "1.0.0"
document_id: "b54ef1a4f77d12ee2e38b180517bf7558b3a7d145c1c414e2c373661d932216f"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/mcp-server-agent-tools/"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:835c15cc1dc605bc6b0b18db4c2fe0c370d94661bc4d3a316a90cac04c2e00a2"
---

# Designing MCP tools for agents: Lessons from building Datadog’s MCP server

Reilly Wood


I work on[Datadog’s official MCP (Model Context Protocol) server](https://docs.datadoghq.com/bits_ai/mcp_server/) , our first observability interface designed specifically for customers’ AI agents. Our first version was a thin wrapper around existing APIs, the kind of thing you can build in a weekend. It worked well enough to validate the idea, but then we started watching agents actually use it to solve real problems.


Agents would fill their context windows with log data and lose track of what they were doing. They’d request what seemed like a reasonable number of records, then blow their token budget because a few of those records happened to be huge. They’d try to answer questions about trends by retrieving raw samples and guessing. We found out quickly that “just expose your APIs” wasn’t going to cut it.


We’ve since rethought almost everything about our tool design to make observability actually ergonomic for agents. In this post, I’ll share some of what we learned from that process.


## Context windows fill up fast


The first thing we learned is that context efficiency matters a lot. When an agent calls a tool, the entire result ends up in the context window, and observability data can be large. Our early tools would sometimes return thousands of log records, each with dozens of fields, and agents would choke on the results.


We attacked this from a few angles.


**Format matters.** Our first prototype just returned JSON from our APIs, which is fine for programmatic consumption but often wasteful for agents. Take this:


```text
1   [    2        {    3          "firstName": "Alice",    4          "lastName": "Johnson",    5          "age": 28    6        },    7        {    8          "firstName": "Bob",    9          "lastName": "Smith",    10          "age": 35    11        }    12   ]
```


Compare to the same data in CSV:


```text
1   firstName,lastName,age    2   Alice,Johnson,28    3   Bob,Smith,35
```


CSV uses[about half as many tokens](https://david-gilbertson.medium.com/llm-output-formats-why-json-costs-more-than-tsv-ebaf590bd541) per record (the exact number varies by[tokenizer](https://platform.openai.com/tokenizer) ). For tabular data without nesting, CSV or TSV is almost always the right choice. For nested data, YAML is a good middle ground—you can usually shave around 20% off token count just by switching from JSON to YAML. If you’re familiar with TOON, this is the core insight behind it.


**Trim what you don’t need.** Our log records had dozens of fields, but agents rarely needed all of them. By trimming rarely used fields from the default output—and letting agents request them back if needed—we saw another big improvement.


The cumulative effect of these two changes was significant. For some tools, we can now fit about 5x more records in the same number of tokens. This makes a huge difference when agents are digging through large volumes of data.


**Rethink your approach to pagination.** APIs are typically paginated by record count, but records can vary widely in size. In our case, a Datadog log message might be only 100 characters or it might be 1 MB. We ran into situations where an agent would request a reasonable number of logs and then the context window was gone because a few of the logs were huge. So we switched to paginating by token budget: The server cuts off its response after a certain number of tokens and returns a cursor for more.


That said, some of this may matter less in the future. Tools like[Cursor](https://cursor.com/blog/dynamic-context-discovery) and[Claude Code](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#212) now write long tool results to disk instead of putting everything in context. This isn’t in the MCP spec yet, but if it becomes widespread, format efficiency will be less critical.


## Let agents query, not just retrieve


Agents often need to do more than retrieve raw data, so efficient formats can only get you so far.


For example, a user might ask their agent, “Which services are logging the most errors in the last hour?” Our first logs tools could only retrieve logs that matched filter criteria, so agents would attempt to answer this question by pulling some logs and inferring trends from that sample. This was wasteful and often incorrect. Even worse, some agents would try to brute-force it, repeatedly retrieving data until the context window filled up.


I’m a databases guy at heart, and I quickly realized that this is exactly the kind of problem I would try to solve with SQL—so why not let agents do the same? Instead of retrieving raw logs, agents can now write a query like:


```text
1   SELECT service, COUNT(*) as error_count    2   FROM logs    3   WHERE status = 'error'    4   GROUP BY service    5   ORDER BY error_count DESC    6   LIMIT 10
```


This gives the right answer quickly, in very few tokens. SQL has worked really well for us: Agents are quite good at writing it, and it gives them fine-grained control over what data ends up in their context window. Supporting this was a significant lift—at the scale we operate at, traditional relational databases don’t work—but it’s been worth it.


One pleasant surprise was that SQL tools didn’t just improve correctness; they also reduced costs for users. In some of our eval scenarios, runs were about 40% cheaper because agents used fewer tokens to reach answers. They could` SELECT` only the fields they needed,` LIMIT` results to a few rows, or count efficiently instead of retrieving large volumes of raw data.


## Tools aren’t free


A “just turn every API into a tool” approach doesn’t scale. As the number of tools grows, agents[struggle with accurate tool calling](https://arxiv.org/abs/2411.15399) , and each tool’s description takes up context window space. We also need to be mindful that Datadog might not be the only MCP server connected to an agent, which makes being frugal with context even more important.


We’ve tried a few approaches to keep tool count down:


-


**Flexible tools** : Rather than one tool per API endpoint, we design tools that can serve multiple use cases. This requires careful schema design, but one well-designed tool can often do the work of several narrow ones.


-


**Toolsets** : By default, connecting to our MCP server gives users a core set of tools for common workflows. Datadog is a large platform, though, so we also support opt-in[toolsets](https://docs.datadoghq.com/bits_ai/mcp_server/setup/?tab=remoteauthentication#toolsets) for more specialized needs. The downside is that users need to anticipate what capabilities their agent will require ahead of time.


-


**Layering** : We explored patterns where agents chain tool calls—one tool to ask “how do I accomplish X?” and another to actually do it. Block has written[a great blog post](https://engineering.block.xyz/blog/build-mcp-tools-like-ogres-with-layers) about this approach. The advantage is that you can expose specialized functionality without cramming it all into the context window up front. The tradeoff is latency: a task that once required one tool call now requires two, which can noticeably slow down agent sessions.


Over time, these approaches may become less necessary. Agents are getting smarter at managing their own context. Tools like Claude Code now[use tool search](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool) to avoid loading every tool up front, and[skills](https://code.claude.com/docs/en/skills) let agents load specialized knowledge on demand. Exactly how skills and MCP fit together is still an open question (we’re trying out approaches like[Kiro Powers](https://kiro.dev/powers/) ), but we’re excited to do more with skills in the future.


## Guide the agent


Early on, we saw agents fail in ways that were hard to diagnose. They’d send a malformed query, get back a generic error, and then try the exact same thing again. Or give up entirely. It took us a while to realize that the problem wasn’t the agents—it was us.


**Error messages matter more than you’d think.** Agents are surprisingly good at recovering from errors, but they need specifics. An error message like “invalid query” usually isn’t helpful; something like “unknown field ‘stauts’ – did you mean ‘status’?” gives the agent a clear next step. We put a lot of effort in making our error messages specific and actionable, and it paid off.


**Make documentation discoverable.** We have a` search_datadog_docs` tool that does a RAG-powered search over Datadog’s documentation. We encourage agents to use it (via[server instructions](https://www.mcpjam.com/blog/server-instructions) ) when they’re unsure about query syntax or available options. This lets us avoid cramming every detail into tool descriptions, while still giving agents a way to look things up on demand.


**Tool results can include guidance, not just data.** This is quite a departure from traditional REST API design, where there’s usually nothing that can reason about plain text calling your API. Our logs tools still return exactly what the agent requested, but sometimes we’ll add a short note like, “You searched for the` payment` service, did you mean to search for the` payments` service instead?”


## Specialized vs. general-purpose


Datadog also has[Bits Investigation](https://www.datadoghq.com/blog/bits-ai-sre/) , a hosted agent that investigates alerts and suggests remediations. It differs from agents using the MCP server in that it has a web UI and is purpose-built for that specific workflow.


There’s a real tradeoff here. Bits Investigation can make assumptions that a general MCP server can’t: It knows the user is investigating an alert, so it can proactively pull in related data and offer specialized tools and UI for that use case. An MCP server has to be more general—it needs to work across many workflows without making strong assumptions up front.


I don’t think one approach will win out. Specialized agents will probably always have an edge for well-defined workflows, but MCP offers flexibility—you can plug Datadog into Claude Code, a homegrown agent, or whatever comes next. We’re working on bringing these closer together, exposing Bits Investigation’s capabilities through MCP and making the specialized agent more flexible about what it can investigate. Over time, the line between “specialized agent” and “MCP server with good defaults” may get blurry.


## The takeaways


There are no textbooks for building MCP servers yet. Most of what we’ve learned has come from working closely with customers and watching agents fail in real scenarios, then trying to figure out why.


If I had to boil that experience down to a few principles, they’d look like this:


-


Don’t just wrap your APIs. Design tools around agents’ constraints.


-


Be frugal with context windows, and give agents the tools to be frugal, too. (Query languages help.)


-


Guide agents with good error messages and discoverable documentation.


This space is moving fast, and some of today’s constraints may relax over time. But you don’t have to wait for that. These lessons are what helped us ship a useful MCP server today, and they’re shaping how we think about agent-facing tools at Datadog going forward.


Interested in building agent-friendly systems at scale?[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=AI&parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-bitasai-ai&gh_src=hm4uekgj1us)


-
-
-
