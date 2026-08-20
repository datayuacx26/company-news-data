---
schema_version: "1.0.0"
document_id: "a326a314a3b301eae32bba551b0061de53102bfd2ab301381ad51b0be8460934"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/logs-beta"
published_at: "2025-12-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:713e16a1f6ddd1085722eee6a192d6c6363aab95300d055d11ca0a728a8aec09"
---

# Meet Logs (beta) – more debugging context, all in PostHog

# Meet Logs (beta) – more debugging context, all in PostHog


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Dec 23, 2025


- [Logs](https://posthog.com/blog/logs)


#### Contents


-
-
-


Every developer's debugging journey ends at the same destination. You can start with an[error](https://posthog.com/error-tracking)


and get context with a[session replay](https://posthog.com/session-replay)


, but eventually, you'll need logs to see what's actually happening in your system. This progression is so familiar, we barely think of it anymore.


We built[Logs (now in beta)](https://posthog.com/docs/logs)


for this debugging journey. Not as a new tool to adopt, but as the part of the investigation you were doing anyway. Now you can get the backend context behind your errors and session replays next to your favorite tools in PostHog, without having to leave the platform and open another tab.


##


Where context gets messy


When logs live outside the rest of your debugging workflow, they lose their most important attribute: context. You leave the error view, open another tool, recreate the timeframe, match request IDs, and hope you’re looking at the same execution path you were just investigating.


The logs are still accurate and the system is still observable. But the story you’re trying to piece together fragments, and understanding takes longer than it should. At PostHog, we don’t think logs should be something you switch to. They should be already there when you need them.


##


Debug faster with Logs next to Session Replays and Error Tracking


With Logs, the debugging journey is continuous.


- When you’re looking at an exception, the logs surrounding that failure are immediately available.
- When you’re watching a session replay, you can see what the backend was doing during that exact interaction.
- When you’re investigating a specific user or event, the relevant logs are part of the same view, not a separate search problem.


You're no longer reconstructing the timeline manually. The frontend behavior, backend activity, and failure point stay connected, making understanding faster and less error-prone.


Logs is built on OpenTelemetry, which means you don’t have to change how you log or adopt a proprietary SDK. If you’re already sending logs via OTLP, they work with PostHog out of the box.


##


What's next for Logs and the debugging journey


[Logs is in beta today](https://app.posthog.com/logs)


, free to use, and focused on tight integration with the rest of the PostHog debugging experience.


Over the coming months, we’re working on deeper connections with Error Tracking and Session Replay, better defaults for viewing logs tied to users and events, and early AI-powered investigation so you can ask questions instead of reading through lines.


The long-term goal is straightforward, even if it’s ambitious: build a logging product where you don’t need to read log lines to understand your system.


[Try it out](https://app.posthog.com/logs)


with your existing OpenTelemetry setup and let us know what you think, we’d love to have you help us shape this product.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
