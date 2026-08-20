---
schema_version: "1.0.0"
document_id: "bbec329ebc9159304b82f39dff2145d9af39777ce004ed2b27118c583e2f021a"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/building-the-validation-stack-for-ai-product-development"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:765d47a5b3cd1f2df9c58d534ad7d21e2904b1329dabb5d4d29c8a0b4f82c703"
---

# Building the Validation Stack for AI Product Development

A lot has happened in a year in the world of experimentation. A year ago my company, Eppo, which offered warehouse-native experimentation, was bought by Datadog. A year later and my company, Amplitude,[is welcoming Statsig](https://amplitude.com/blog/amplitude-and-statsig-partnership) , its customers, and its brand to its platform.


The team at Statsig built a strong product. They recognized early that engineers had a need for better tools for rollouts and to understand the value of what they were shipping. They developed a builder-first approach to feature flags, experiments, metrics, and rollout controls, that clearly resonated in the market.


At Amplitude, we believe, just like Statsig does, that experimentation is core infrastructure and a foundational part of how products get built. This is even more important in an AI world. Partnering with Statsig is an opportunity to accelerate a shared vision for the future of product development.


## How building products has changed


The bottleneck in product development has moved. It used to be writing code. But now, with a majority of developers using AI coding tools, code generation is only getting faster. PMs write code while designers build and ship full UX flows. The code barrier to getting something built has fully collapsed.


But the gap between shipping a new feature and knowing that it’s good for users has actually gotten wider. Teams are shipping faster than ever, and while the volume of changes going out the door has exploded, the infrastructure to validate those changes hasn't kept pace. Existing bottlenecks in the experimentation process compound when shipping velocity increases.


With non-deterministic products like LLMs, it has become even harder to determine if you’re shipping the right thing. Whether you’re working on a chatbot, a recommendation engine, or something else, non-deterministic outputs give you a different response every time. Unit tests can’t give you the confidence you need. Experimentation can.


Additionally, the people building these products aren't necessarily the same people who ran experiments five years ago. The number of people capable of writing code or shipping new features has exploded, but the number who deeply understand how to validate those features has not. Modern experimentation tooling needs to support a much broader range of AI builders.


## Building the validation stack for AI product development


Internally, we’re thinking about what the “2.0” of experimentation needs to become.


Version 1.0 is a known loop: ship with feature flags, measure impact with experiments, understand usage with analytics. That loop still works. But teams building AI products need another layer of validation and rigor. You need offline evaluation, live experimentation, and continuous monitoring working together.


The starting point for Experimentation 2.0 is offline evals. Instead of manually checking a few outputs and hoping for the best, you run prompts and models through thousands of labeled test cases before anything reaches production. The goal is to catch regressions early and avoid surprises in production.


Say you’re running an AI support ticket classifier. You have a prompt that triages tickets to billing, technical support, or sales. You update the prompt to handle edge cases better. Is the new version actually better? Offline evals let you run both versions against a labeled dataset of a thousand tickets, score them against graders (including LLM-as-a-Judge for cases where string matching doesn’t work), and see exactly where the new version wins and where it regresses. You iterate on this loop rapidly before any user sees the change.


From there, you move to progressive rollout with gradual deployment and instant rollbacks, tied to service metrics, business KPIs, and LLM-specific observability signals. If latency spikes or error rates climb, the system responds before the issue spreads.


Then onto online experimentation. A/B tests on live traffic with statistical confidence. Shadow-mode evals that grade model output against production scenarios without exposing users to risk. Every rollout should measure impact, not just reduce risk.


Running through this entire 2.0 loop is LLM observability, which gives you real-time logging, monitoring, and anomaly alerting in a single view alongside business metrics and user engagement. When something goes wrong with your AI product, you shouldn’t need four dashboards to figure out where.


## Amplitude + Statsig will get there faster


Statsig and Amplitude were already building toward the same future; one where flags, experiments, and analytics aren’t separate products you have to stitch together, but layers in a single system that covers the full product development lifecycle.


This partnership accelerates that vision. Amplitude has been building out[Agent Analytics](https://amplitude.com/blog/agent-analytics) to connect observability and evals with product analytics, while Statsig’s roadmap has been focused on building capabilities like AI Configs for controlling prompts and model parameters without redeploying, and an MCP server integration that embeds experimentation directly into AI coding workflows.


We’re continuing to invest in both platforms with a focus on maintaining the existing Statsig platform across cloud and warehouse deployments and supporting current customers through the transition. We’re also building a shared roadmap that moves both platforms forward together.


## Experimentation at the speed of shipping


A year ago, no one knew how the evaluation loop needed to change for probabilistic products. Now we do. AI coding assistants generate more changes than any team can manually validate. LLM-powered products introduce non-deterministic behavior that demands continuous evaluation and validation. The cost of shipping a bad change keeps climbing as products get more complex.


The teams that will outperform with AI aren’t necessarily the ones shipping the most features, but the ones learning what worked and feeding that answer back into the next decision. This creates a feedback loop that accelerates product velocity.


Amplitude spent years making experimentation faster and more accessible. Statsig spent years making it more powerful and more developer-native. Together, we’re building the validation layer that closes the gap between shipping and understanding value.


Try Statsig


Explore the future of warehouse-native experimentation.[Create a free Statsig account](https://console.statsig.com/sign_up?_gl=1*fvtm7r*_gcl_au*MTgwMjQ1NzMwMC4xNzc1MDY2NzI1LjEzNTQxNTcwMzguMTc3ODYwNDQ0OC4xNzc4NjA0NDQ3*_ga*NDI1NjI3NTA1LjE3NzUwNjY3MjU.*_ga_EM5RHE1RHW*czE3Nzg3NzE3OTckbzEyJGcwJHQxNzc4NzcxNzk3JGo2MCRsMCRoMA..) in minutes or[get a live demo](https://statsig.com/contact/demo) .


(PS Yes, this is a little odd for us too.)
