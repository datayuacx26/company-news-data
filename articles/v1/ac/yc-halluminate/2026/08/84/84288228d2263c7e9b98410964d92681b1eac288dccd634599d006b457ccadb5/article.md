---
schema_version: "1.0.0"
document_id: "84288228d2263c7e9b98410964d92681b1eac288dccd634599d006b457ccadb5"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/browsergym-vs-web-bench"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:7b974b10d6a23562bab79164124613d695da5b0fcbe5c0926a5d21f26c0bbd36"
---

# BrowserGym vs Real-Website Benchmarks

[BrowserGym](https://github.com/ServiceNow/BrowserGym) shows up in almost every serious web-agent paper from the last two years. That is a good thing. It also creates a quiet category error: people treat “we evaluated in BrowserGym” as “we measured production browser automation.”


BrowserGym is a **framework** .[Web Bench](https://www.halluminate.ai/blog/benchmark) is a **live-web task suite** . Conflating them is like conflating Gymnasium with a specific robotics competition.


## What BrowserGym is good at


ServiceNow Research built BrowserGym as an OpenAI Gym-style environment for multimodal web agents. You get:


- Flexible observations (DOM / accessibility tree, screenshots, coordinates)
- A broad action space, including high-level primitives and code
- Chat-style interaction hooks
- Compatibility with prior benchmarks such as WebArena and MiniWoB
- Shared experiment tooling via[AgentLab](https://github.com/ServiceNow/AgentLab/)


That shared plumbing is why BrowserGym improved comparability. The WebArena paper’s GPT-4 setup and later BrowserGym-hosted runs are not the same experiment; the harness itself can change scores. Having one place to hold observation and action definitions constant is real scientific progress.


## WorkArena, and the “Salesforce” search confusion


When search queries say “BrowserGym Salesforce benchmark,” they almost always mean[WorkArena](https://github.com/ServiceNow/WorkArena) : knowledge-work tasks on a remote-hosted **ServiceNow** instance, run through BrowserGym.


WorkArena L1 focuses on atomic UI skills across the ServiceNow surface. WorkArena++ composes those skills into longer planning and memory problems. This is an excellent stress test for enterprise agents—dense forms, catalogs, filters, multi-step fulfillments.


It is still a controlled enterprise world. It is not 452 unrelated consumer domains with Cloudflare in front of half of them.


## What the harness does not measure


Even when BrowserGym hosts WebArena-style sites or WorkArena’s ServiceNow stack, you typically do **not** measure:


- Open-web bot detection and captcha loops
- Residential proxy quality and ban rates
- Marketing-site UI churn week to week
- Write actions that can affect real third-party users


Those modes dominate real deployments. In our[Web Bench](https://www.halluminate.ai/blog/benchmark) study they showed up as a first-class error class;[BrowserBench](https://www.halluminate.ai/blog/browserbench) exists specifically to isolate the infrastructure slice (where we saw **25–50%** accuracy swings from infrastructure alone).


## When to report which number


Claim you want to make Prefer


Our planner / observation / memory change helped BrowserGym + WorkArena or WebArena


Our agent handles ServiceNow-style knowledge work WorkArena


Our agent works on the public web, including writes[Web Bench](https://webbench.ai/)


Our failures are stealth / proxy, not reasoning[BrowserBench](https://www.halluminate.ai/blog/browserbench)


A clean workflow we recommend to teams:


1. Iterate fast in BrowserGym.
2. Publish WorkArena / WebArena numbers for research comparability.
3. Gate customer-facing live-web claims on Web Bench.
4. If live scores crater, measure infrastructure before rewriting the agent.


BrowserGym made the field more rigorous. Web Bench is for the harder claim: that the agent still works when the website did not agree to be a gym.


More context:[browser agent benchmarks map](https://www.halluminate.ai/blog/browser-agent-benchmarks) and[WebArena vs Web Bench](https://www.halluminate.ai/blog/webarena-vs-web-bench) .
