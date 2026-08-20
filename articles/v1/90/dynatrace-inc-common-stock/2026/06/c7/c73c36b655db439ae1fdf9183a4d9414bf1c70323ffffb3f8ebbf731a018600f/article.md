---
schema_version: "1.0.0"
document_id: "c73c36b655db439ae1fdf9183a4d9414bf1c70323ffffb3f8ebbf731a018600f"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/dynatrace-release-radar-05-26/"
published_at: "2026-06-16T18:49:01+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:30c1e04192688a80f7a1c19cdba24fbd1d971fcf3b7f2483a8039ff3212c9c10"
---

# Dynatrace Release Radar 05.26

This series covers recent Dynatrace releases and updates, focusing on what’s new, what’s changed, and how these recent enhancements can benefit you and your organization. Each post covers newly available capabilities and points you toward where to explore them.


This edition of Release Radar covers the Dynatrace releases from May, 2026. Here are the changes that should matter right away to practitioners:


- AI tooling
- AI coding agent monitoring
- Jira integration
- Dashboard productivity
- Pipeline grouping


To see them in action, head over to our[release radar launchpad](https://dt-url.net/release-radar) on the Dynatrace Playground.


## Dynatrace Assist: At your side with more context


Dynatrace Assist gets four upgrades in sprint 1.338 that deepen its usefulness during active investigations.


**Side-by-side mode** puts the chat interface in a collapsible panel alongside your current view — dashboard, notebook, or any other app page stays visible while you work with Assist. Chat and investigate at the same time without losing your place.


**Reference files and skills** give Assist access to a curated knowledge base built on Dynatrace documentation and product expertise, following the Anthropic Claude Agent Skills format. Responses are designed to draw on structured Dynatrace knowledge, so answers are grounded in how the platform actually works.


**Anthropic Claude Sonnet 4.6 as the foundation model** can help bring stronger multi-step reasoning and improved performance on complex, multi-tool investigations — the same model used in the latest Anthropic API and Claude Code.


**A purpose-built NL2DQL model** makes natural-language-to-DQL generation more accurate. The capability now uses a fine-tuned foundation model based on Llama 3.1 8B, trained specifically on Dynatrace query patterns. Write a question in plain language and get a working DQL query with fewer iterations.


Dynatrace Assist now works side by side with your current view, with stronger reasoning, grounded reference skills, and more accurate natural-language-to-DQL generation.


## AI coding agents get unified monitoring


Dynatrace now provides observability for five major AI coding agents: Claude Code, Google Gemini CLI, OpenAI Codex CLI, OpenCode, and GitHub Copilot SDK.


As your team adopts multiple AI agents in parallel, you need shared visibility into what they cost, how they behave, and what they produce. This release gives platform teams, engineering leaders, and security teams a unified observability across all five agents, all built on OpenTelemetry.


What teams get across the supported agents:


- **Adoption and token tracking** — session counts, token consumption, and cost trends across agents and teams.
- **Tool behavior visibility** — which tools each agent calls, how often, and where runs slow down or fail.
- **Production context in the IDE** — engineers can query live Dynatrace data through the Dynatrace MCP Server without leaving their coding environment.
- **Engineering outcome correlation** — connect agent activity to downstream delivery signals like commits and pull requests (available for Claude Code).


Pre-configured dashboards are available for each agent. For the full breakdown by agent and setup details, see[Dynatrace expands AI coding agent monitoring](https://www.dynatrace.com/news/blog/dynatrace-expands-ai-coding-agent-monitoring/) .


Dynatrace brings unified observability to five major AI coding agents, helping teams track adoption, token usage, tool behavior, and cost across environments.


## Investigate production problems without leaving Jira


The Dynatrace MCP Server now integrates with Atlassian Rovo, bringing observability context directly into Jira and JSM tickets.


When an incident or issue is open in Jira or JSM, Rovo can now call Dynatrace tools in natural language — querying metrics, traces, logs, and topology — and post the results as ticket comments. Root cause analysis and dependency mapping happen inside the ticket, so engineers stay in context instead of switching between platforms.


Key points for practitioners:


- **No context switching** — investigate and document findings without leaving Jira or JSM.
- **Per-user OAuth 2.1** — every Dynatrace call runs as the requesting user, with a full audit trail across both platforms.
- **Admin-controlled tool exposure** — administrators choose which Dynatrace tools Rovo can access.
- **Included with Dynatrace SaaS** — no additional cost, and adding users doesn’t change the pricing.


It’s designed for fast setup: authenticate via the Rovo admin UI, select the tools to expose, and the integration is live across Jira, JSM, and Confluence.


For the full walkthrough, see[Dynatrace MCP Server for Atlassian Rovo](https://www.dynatrace.com/news/blog/dynatrace-mcp-server-for-atlassian-rovo-investigate-production-problems-without-leaving-jira-or-jsm/) .


With the Dynatrace MCP Server for Atlassian Rovo, teams can investigate incidents and add observability findings directly inside Jira and JSM tickets. (Video)


## Dashboards: build faster, navigate better


Releases 1.338 and 1.339 bring a focused set of dashboard improvements that add up across a day of analysis work.


**Ready-made tiles and sections** expand the dashboard and notebook library with pre-configured visualizations and built-in drill-downs to other Dynatrace apps. Browse or search the tile library to find components that are ready to use, and customize from there rather than starting from a blank canvas.


**Direct JSON editing** lets power users open and edit the full dashboard definition as JSON from the dashboard **Actions** menu. The format matches the Dashboard API, so configuration changes, bulk tile edits, and version-controlled workflows are all faster in the editor than in the visual UI.


**URL-driven variables** make it possible to configure hidden dashboard variables through URL parameters, enabling pre-configured views to be linked directly with filters already applied — useful for sharing context-specific dashboards with specific teams or stakeholders.


**Launcher link reordering** lets users drag links between sections in the launcher, making personal navigation layouts easy to maintain.


**Active tile tab persistence** keeps the last-active editing tab visible when switching between tiles, so the configuration state is preserved while navigating across a dashboard.


New dashboard enhancements — including ready-made tiles, direct JSON editing, and URL-driven variables — make it faster to build, refine, and share analysis views.


## Pipeline groups get a configuration UI


Pipeline groups, which let central teams enforce shared policies across multiple OpenPipeline pipelines, now have a dedicated configuration interface in Early Access (sprint 1.339).


Platform teams can now configure group-level policies in the UI, including cost allocation and sensitive data scanning. Pipeline teams still control their own parsing and extraction logic, so central governance doesn’t come at the cost of local flexibility. No direct API access required.


This UI makes the feature accessible to a wider set of platform operators and can help reduce setup costs for organizations running large, multi-team pipeline environments.


For background on pipeline groups and the governance model they enable, see[Pipeline Groups in Dynatrace OpenPipeline](https://www.dynatrace.com/news/blog/pipeline-groups-in-dynatrace-openpipeline-enterprise-grade-governance-explained/) .


Pipeline groups now include a dedicated configuration interface in Early Access, making shared governance policies easier to manage across OpenPipeline pipelines.


## Latest UX improvements


Investigation workflows in logs get sharper. Join the[Log pattern analysis preview](https://docs.dynatrace.com/docs/shortlink/preview-program#log-pattern-analysis) and see how selected log patterns now open a dedicated deep-dive panel showing behavior over time and associated records — inspect pattern-level context without losing the overview, and drill into individual log records from the same panel without navigating away. Log attributes open in full-screen mode for reading long or nested JSON in place. When Logs is opened from a contextual link in a dashboard or alert, the query runs automatically — no extra click required.


Navigation, filtering, and performance also improve across several surfaces. In the Session List, a tooltip on the Session Replay icon shows replay availability — full, partial, or none — so you can triage which sessions have usable replay before opening them. The Services explorer gains dynamic tag filters for Kubernetes namespace annotations and labels, expanding filter coverage for k8s-heavy environments. Infrastructure inventory in large network environments now loads in 3–5 seconds in typical environments, with status and reachability columns loading progressively so the view is immediately usable at scale. Press **Shift + ?** anywhere in the platform to open the keyboard shortcut reference.


Recent UX updates improve investigation workflows across logs, session replay, services, and infrastructure, helping teams move faster with less context switching.


## Why these changes matter


The May releases extend capabilities across the workflows that practitioners use most. AI tooling stays present during investigations. Coding agents become observable assets, not black boxes. Dashboards get faster to build and easier to manage. Cost data supports annual planning. And pipeline governance reaches more teams through a UI.


These are the kinds of changes that add up across a week of real work.


Check out the updates in action on our[Release Radar Launchpad](https://dt-url.net/release-radar) .
