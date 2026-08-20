---
schema_version: "1.0.0"
document_id: "856900ef4ce9394d95f94074cc46df8e90508ec09535decb7c616b8051ca3c8f"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/persons-on-events"
published_at: "2022-11-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:3ab604a9c8216edbee7815d1f2c6a9292aa38c37ec2a70f855f3ec30b650be2c"
---

# How we’re improving performance by combining persons and events

# How we’re improving performance by combining persons and events


Nov 02, 2022


- [Product updates](https://posthog.com/blog/product-updates)


In[a previous product update](https://posthog.com/blog/the-posthog-array-1-39-0)


we announced a beta for a substantial change to the way we handle persons and events on PostHog. Today, after gathering your feedback and seeing the improvements to query performance, we’re rolling this change out as part of the 1.41.0 update. It is available now for self-hosted users and PostHog Cloud users can expect to see the benefits soon as we roll it out.


> **Need to upgrade a self-hosted instance?** We've explained how to upgrade to 1.41.0 and run the necessary async migrations in[our 1.41.0 release highlights](https://posthog.com/blog/the-posthog-array-1-41-0)
>
>
> .


This change combines[persons](https://posthog.com/manual/persons)


and[events](https://posthog.com/manual/events)


into a single[ClickHouse table](https://posthog.com/blog/clickhouse-vs-postgres#olap-vs-oltp-aka-columns-vs-rows)


, adding person IDs and properties *onto* events.


You won’t see any UI changes — persons will still have their own Persons & Groups section on the sidebar, for example — and there’ll be no loss of functionality, but here are some changes you may see:


-


**Faster results for queries involving person properties and events** . Putting persons and events into a single ClickHouse table means we no longer have to join tables to get results on queries involving these data. As a result, query performance will improve by up to 400%.


-


**Faster filtering of events with person properties** . Similarly, filtering events by person properties is much faster when joining the tables is no longer necessary. Anywhere where you’re working with persons and events, PostHog will be faster.


-


**Users will no longer be merged retroactively in some situations** . When an identified user logs in from a different browser (thus becoming anonymous) we end up with separate records for their unidentified and identified behavior. We used to do database joins at query time, so that all events were tied to the same person. Now, we simply look at the events which have the data from the event processing time - meaning that in some situations anonymous events are shown as separate, unique persons in insights. Further[information is available in the docs](https://posthog.com/docs/how-posthog-works/ingestion-pipeline#merging-two-persons)


-


**You can create insights based on person properties at the time of an event.** This wasn’t previously feasible and was often requested. Now, with this change, it's possible!


-


**Self-hosting users will see a storage increase** . This is due to extra information being stored in your self-hosted ClickHouse. It's difficult to give an estimate on this as the impact will vary a lot depending on configuration and usage per organization.


While improving performance has been a major motivation for this change, this is also a crucial step in ensuring PostHog can continue to scale effectively.


Since launching in 2020, PostHog has been adopted by over 15,000 companies and has tracked over 50 billion events. In order to ensure that current and future users have the best possible experience, we need our systems to work as efficiently as possible. Adding persons on to events is an important part of this, along with supporting work such as[using materialized columns in ClickHouse](https://posthog.com/blog/clickhouse-materialized-columns)


to speed up queries even further.


This said, even though the change is now being fully deployed, we’re still eager to hear[your feedback](https://app.posthog.com/home#supportModal)


and understand how we can keep making PostHog better. If you’d like more information about any of the changes above then we’re happy to[answer your questions](https://posthog.com/questions)


.


Want to just try it already?


(Sorry for the shameless CTA.)


[Try PostHog - free](https://us.posthog.com/signup)


[Schedule a demo](https://posthog.com/talk-to-a-human)


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
