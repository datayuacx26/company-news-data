---
schema_version: "1.0.0"
document_id: "d01fff293767d81cbcb3ef0fbac2620efbdb15bbcc548ec9654380f2d2c57971"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/browser-agent-benchmarks"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:31e2bf6d8516bdfb3e106e7186427abc57976725ba5770671a3778988d00d724"
---

# Browser Agent Benchmarks in 2026

Browser agent evaluation has split into three problems that people keep collapsing into one leaderboard:


1. Can the agent **plan and act** in a browser UI?
2. Does that skill survive the **live, adversarial web** ?
3. Is failure coming from the **model** , or from **proxy / captcha / auth infrastructure** ?


Most “web agent benchmark” comparisons mix those questions. The result is overconfident claims: a strong WebArena number gets treated like production readiness, or a live-web failure gets blamed on the LLM when the session never cleared Cloudflare.


This post is the field map we wish we had when we started building[Web Bench](https://www.halluminate.ai/blog/benchmark) ,[BrowserBench](https://www.halluminate.ai/blog/browserbench) , and[Westworld](https://www.halluminate.ai/blog/westworld) .


## The landscape


Benchmark What it actually tests Environment Rough scale


[WebArena](https://webarena.dev/) Long-horizon navigation + CRUD on realistic clones Self-hosted sites 812 tasks


[BrowserGym](https://github.com/ServiceNow/BrowserGym) +[WorkArena](https://github.com/ServiceNow/WorkArena) Multimodal agents on enterprise / gym tasks Framework + ServiceNow Large instance space over ~33 L1 task types


[WebVoyager](https://arxiv.org/abs/2401.13919) Early live-web, mostly information seeking Live sites Smaller, read-heavy


[Web Bench](https://www.halluminate.ai/blog/benchmark) Read **and** write on the public web Live sites ~2,454 open tasks / 452 sites


[BrowserBench](https://www.halluminate.ai/blog/browserbench) Whether infrastructure lets the agent act at all Live sites chosen for friction 292 sites


[Westworld](https://www.halluminate.ai/blog/westworld) Reproducible training + eval without live-web noise High-fidelity sims 100 tasks / 5 environments


## Controlled environments: WebArena and BrowserGym


[WebArena](https://webarena.dev/) made academic web-agent research possible. You get self-hosted clones (shopping, forum, GitLab-style collaboration, CMS, maps), **812** outcome-graded tasks, and a stack every lab can reproduce. If two papers disagree on WebArena, they are at least arguing about the same world.


[BrowserGym](https://github.com/ServiceNow/BrowserGym) is the harness many of those papers now share: gym APIs, multimodal observations, high-level actions.[WorkArena](https://github.com/ServiceNow/WorkArena) puts that harness on ServiceNow knowledge-work flows—the “BrowserGym Salesforce benchmark” searches people run are usually pointing here, even when the product name in the query is wrong.


These systems are excellent for ablation. They are **not** a substitute for the open web. Frozen clones do not fire captchas the way Dick’s Sporting Goods does. They do not rotate inventory, A/B test checkouts, or ban your proxy mid-trajectory. We wrote the longer comparisons here:


- [WebArena vs Web Bench](https://www.halluminate.ai/blog/webarena-vs-web-bench)
- [BrowserGym vs real-website benchmarks](https://www.halluminate.ai/blog/browsergym-vs-web-bench)


## Live web: WebVoyager, then Web Bench


WebVoyager mattered because it left the Docker compose file. It also left a lot of the hard part on the table: login, form writes, destructive actions, multi-step account state.


[Web Bench](https://www.halluminate.ai/blog/benchmark) is our attempt to measure what production teams actually ship. The open set is ~2,454 tasks across 452 real sites, sampled from a larger ~5,750-task pool, with READ plus CREATE / UPDATE / DELETE / file work. The launch study’s useful headlines:


- Fully automated overall SOTA: **Anthropic Computer Use at 66.0%**
- Best fully automated **non-read** score: **Skyvern 2.0 at 46.6%**
- Strong agents often clear **>70%** on READ
- A large error bucket is infrastructure, not planning


Agent-specific notes:


- [Skyvern Web Bench scores](https://www.halluminate.ai/blog/skyvern-webbench-scores)
- [Browser Use on Web Bench](https://www.halluminate.ai/blog/browser-use-benchmark)


## Infrastructure as its own axis: BrowserBench


In Web Bench, swapping browser infrastructure moved accuracy by **25–50%** on the same agent. That is not a footnote. If your eval does not separate “model failed to click Submit” from “session never got past the bot wall,” you will optimize the wrong layer.


[BrowserBench](https://www.halluminate.ai/blog/browserbench) is deliberately easy for models and hard for stealth: 292 read tasks on sites chosen for proxy and captcha friction. The metric is stealth failure rate, not clever planning.


## Training without burning the web: Westworld


Live sites are hostile to RL. Captchas are stochastic, prices move, and a bad action can create a real reservation.[Westworld](https://www.halluminate.ai/blog/westworld) is our task-centric simulator suite for flight booking and ecommerce-style workflows—reproducible state, verifiable rewards, and a path into post-training (see also the[sim-to-real study](https://www.halluminate.ai/blog/sim-to-real-webagents) ).


Use Westworld when you need volume and clean credit assignment. Use Web Bench when you need an external, live-web claim.


## A sane evaluation stack


For most teams building browser agents in 2026:


1. **Iterate** in WebArena / BrowserGym (or your own staging apps).
2. **Train** in simulators when you need RL scale ([Westworld](https://www.halluminate.ai/blog/westworld) ).
3. **Validate** on[Web Bench](https://webbench.ai/) before you call a release production-ready.
4. **Diagnose** live-web collapses with[BrowserBench](https://www.halluminate.ai/blog/browserbench) —agent skill and infrastructure are different bugs.


If a paper or vendor only reports one of those layers, ask which of the three questions they answered. Usually it is only the first.


One adjacent confusion worth naming:[WebSailor](https://www.halluminate.ai/blog/websailor-benchmark) is often described as a “benchmark,” but it is mainly a training-data and method line for deep web search agents—not a frozen UI exam like WebArena. Different job, different numbers.
