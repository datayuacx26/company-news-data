---
schema_version: "1.0.0"
document_id: "ca444315a324dcd1d1f0351c100724baf082bbfa4867b3b9b187dc6339ad5217"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/codecanary-uses-codecanary-to-improve-itself"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-21T08:26:03.097679+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:b59d36f5a248f4c3c28467bac546c02b2b0e3ff104832fa80049894bedca6260"
---

# CodeCanary uses CodeCanary to improve itself

CodeCanary, our AI product engineer, is also *our* product engineer. We use CodeCanary to find bugs, monitor our onboarding conversion, and identify high value leads. This has led to CodeCanary[improving itself](https://www.codecanary.ai/blog/recursive-self-improvement-is-possible-for-apps) by fixing bugs in its own onboarding while we (literally) were sleeping.


## How does CodeCanary work?


CodeCanary is an agent that lives in Slack and uses your product analytics, session replays, and your codebase to:


1. Detect and fix bugs
2. Identify conversion bottlenecks and implement A/B tests
3. Track the behavior of high value leads and customers
4. Answer any other question that involves your product analytics, either ad-hoc or on a schedule


You can configure how any of these automations run—from the frequency, to the sensitivity, to the type of patterns you want to watch for.


## CodeCanary fixed CodeCanary while we slept


### We created a bug in our onboarding


When you onboard to CodeCanary you create the automations that determine what the agent alerts you to and what it fixes. Once you go through the process of creating an automation, you are prompted to create an account.


Little did we know that when you authenticated you lost all your progress from that automation and were sent back to step 1. A user then experienced this bug, and was captured in a session replay.


[CodeCanary watches all session replays for bugs using an LLM](https://www.codecanary.ai/blog/watching-session-replays-with-ai) , which lets it spot bugs before we even knew about it.


### CodeCanary fixed the bug


We configured CodeCanary to watch our session replays for bugs and submit a PR if it thinks it's found one. At 3:37 AM, while Brendan and I were fast asleep, CodeCanary was watching bugs and spotted the progress reset.


After finding the bug, it proposed a fix and sent a PR.


When we woke up we saw the PR, shipped it, and our onboarding was a bit better for effectively no work.


## How can I use CodeCanary?


If you use PostHog you can[start using CodeCanary in less than 5 minutes](https://www.codecanary.ai/onboarding) .
