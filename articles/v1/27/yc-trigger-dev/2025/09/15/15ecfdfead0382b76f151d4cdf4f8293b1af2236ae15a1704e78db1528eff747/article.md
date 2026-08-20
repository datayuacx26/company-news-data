---
schema_version: "1.0.0"
document_id: "15ecfdfead0382b76f151d4cdf4f8293b1af2236ae15a1704e78db1528eff747"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/our-roadmap-for-the-next-3-months"
published_at: "2025-09-04T14:00:00+00:00"
first_seen_at: "2026-07-24T05:26:40.243460+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:c11c9d36c84d536cfcb2e04e50e34dd0e6d1a1c699b2a607296240f9579784e7"
---

# Our roadmap for the next 3 months and beyond

**[Launchweek 2](https://trigger.dev/launchweek/2) just wrapped, marking Trigger.dev v4 as[Generally Available](https://trigger.dev/launchweek/2/trigger-v4-ga) . Now we're setting our sights on what's next: making it even easier to build production-grade AI agents at scale.**


Here's what we're building next and why it matters. Some features will ship within the next few months, while others are bigger projects we're starting now.


## Lightning-fast execution with MicroVMs and sandboxes


We've begun work on moving to MicroVMs which will deliver cold start times under 500ms - fast enough that cold starts feel instant.


Traditional containers take seconds to boot. MicroVMs start in milliseconds. Combined with our existing warm starts, this means your AI agents & tasks will respond immediately, whether it's the first request or millionth.


MicroVMs also let us snapshot more of your task state, which means subsequent executions start with warm caches, making every run faster.


This will also lay the groundwork for bringing sandboxing to Trigger.dev, which will let you execute user code in secure cloud sandboxes. This is great for AI agents that need to generate and run code dynamically in isolation without compromising security.


## Full-stack Agent Toolkit


We're building a toolkit that includes common tools and patterns for AI agents. It will have pre-built tools for typical agent operations, orchestration logic, memory management, and error handling.


The goal is to reduce the setup work when building agents. Instead of integrating multiple packages and writing orchestration code, you'll be able to focus on your agent's specific behavior.


This toolkit is part of making Trigger.dev better suited for AI agent workflows, where you often need the same foundational pieces but want to customize the agent logic. Importantly, we will also be allowing you to "Bring your own Agent Framework" (BYOAF) by providing lower-level primitives for integrating Trigger.dev with existing agent frameworks like Mastra, Langchain, AI SDK, and more.


[Subscribe to this feature](https://feedback.trigger.dev/p/full-stack-ai-agent-toolkit) for updates.


## GitHub Integration


Our new GitHub integration will bring true GitOps to Trigger.dev, letting you connect your repository and automatically deploy whenever you push to your chosen branch.


Connect our GitHub App to your repository, and every push triggers an automatic deployment. Your tasks stay in sync with your codebase automatically. And you'll also be able to configure automatic[Preview branches](https://trigger.dev/launchweek/2/preview-branches) for your Pull Requests.


## Vercel Integration


Once we ship the GitHub integration, we will set our sights on an integration with Vercel. This is our[most requested feature](https://feedback.trigger.dev/p/vercel-integration-3) . Many of you run your frontend on Vercel and need to coordinate with background tasks on Trigger.dev.


We're building a Vercel integration that will sync deployments, environment variables, and preview branches automatically. When you add an API key to Vercel, it will be available in your Trigger.dev tasks. When you create a pull request, your tasks will deploy to a preview environment with the same environment variables.


This will eliminate the manual work of keeping everything in sync between the two platforms.


[Subscribe to this feature](https://feedback.trigger.dev/p/vercel-integration-3) for updates.


## Dramatically improved logging with ClickHouse


We currently have[log size limits](https://trigger.dev/docs/limits#log-size) that work great for most tasks, but complex AI workflows often generate far more logs. When you hit the limit, you have to download log files to see what happened.


ClickHouse will let us display much larger numbers of logs directly in the dashboard. You'll be able to see significantly more of your logs without downloading anything.


We're also planning to add log streaming later, which will remove practical limits entirely. But even the first ClickHouse integration will be a big improvement for debugging runs that exceed the current limits.


[Subscribe to this feature](https://feedback.trigger.dev/p/logs-view-with-global-search-and-filtering) for updates.


## Advanced metrics and alerting


We're going to be building out a much more advanced metrics dashboard. It will include CPU and memory usage tracking, cost tracking per task and run, performance analytics, configurable alerts when things go wrong, and lots more.


You'll be able to get notified through Slack, email, or webhooks when the limits or errors you've set are exceeded.


We'll be rolling this out in stages, starting with a metrics dashboard to make it easier to understand how your tasks are performing.


## The vision: AI agents


These features all work toward the same goal: making Trigger.dev the best platform for building AI agents. We want creating agents to be simple, performance to be fast, and scaling to happen automatically.


Each improvement targets a specific part of the agent development experience. Faster cold starts make agents more responsive. Better logging makes debugging easier. The agent toolkit reduces setup work. GitHub and Vercel integrations improve developer workflow.


Over the next three months, building production AI agents on Trigger.dev will get significantly better. We're looking forward to shipping these features and seeing what you build.


---


*Want to try Trigger.dev today?[Get started with our quickstart guide](https://trigger.dev/docs/quick-start) or check out an[AI agent example](https://trigger.dev/docs/guides/example-projects/vercel-ai-sdk-deep-research) to see what's possible.*
