---
schema_version: "1.0.0"
document_id: "2ce2d076ad9702eceb1996ea7cd018006b58824ff5575826015cceddf407444c"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/greptile-v5"
published_at: "2026-08-05T12:00:00+00:00"
first_seen_at: "2026-08-05T20:51:56.855738+00:00"
fetched_at: "2026-08-05T20:51:59.101498+00:00"
content_hash: "sha256:c84113a27b1dc19fe719936c2869c405c27d4cdb09c02ddbbd7fa7b7a149859a"
---

# Greptile v5

Today we're releasing Greptile Agent v5, our best code review agent yet. It is faster, better at catching bugs and flags fewer false positives.


We rebuilt Greptile's agent architecture around the idea that agents perform better when their task is narrowly scoped.


Greptile V5 spins up a swarm of agents that each explore one hypothesis for a potential bug. Since these agents run in parallel, our end to end latency is much smaller. Because each agent is focused on a narrowly scoped task, it can explore much deeper when needed, making Greptile better at catching tricky bugs.


We have been A/B testing v5 for the last month across many customers. Here is how v5 does compared to v4 based on live production data from over a million PRs.


1. Median review time went down from 5:04 to 2:25 seconds.
2. **Percentage of Greptile comments addressed** by the author increased from 52% to 66%.
3. Developers often reply to Greptile's comments. **Positive replies** ("nice catch" or "fixed") increased by **28.6%** from 0.266 per pull request to 0.342 per pull request.


Greptile v5 is now enabled for all users. We’re excited for you to try it.


## Recent feedback
