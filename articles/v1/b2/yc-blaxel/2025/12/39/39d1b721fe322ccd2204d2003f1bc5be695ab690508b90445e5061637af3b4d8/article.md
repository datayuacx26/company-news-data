---
schema_version: "1.0.0"
document_id: "39d1b721fe322ccd2204d2003f1bc5be695ab690508b90445e5061637af3b4d8"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/2025-blaxel-our-year-end-round-up"
published_at: "2025-12-18T10:06:12+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:d2c9c702a9f903b398802e40e8ad1de286938ad2c8d656a7654a82e2e9c95331"
---

# 2025 @ Blaxel: Our year-end round-up

2025 was epic for us! From being accepted to Y Combinator and getting our first round of funding to launching new products and starting to build out our vision for the future, it's been a year of growth, progress and learning. Here's a look back at some of the highlights!


## We raised $7.3M to build infrastructure for the AI era


We were accepted to[Y Combinator's Spring 2025 batch](https://www.ycombinator.com/companies/blaxel) and all 6 of our co-founders attended. This in itself was unique; most companies attend with between 1-4 co-founders. Thoughtful talks by AI and infrastructure leaders (including[Sam Altman](https://x.com/sama) ) helped us conceptualize and define our vision for Blaxel throughout the 11 weeks of YC. We built a lot during this time, including designing our network architecture, prototyping our UX, getting feedback from others in our cohort, and - of course! - preparing for Demo Day.


By the time YC ended, we had developed a clear vision for our business, and[we eventually raised $7.3M in a seed round from First Round, Y Combinator, Liquid 2, Multimodal and others](https://blaxel.ai/blog/Blaxel-Raises-7-3M-Seed-Round-led-by-First-Round-to-Build-Cloud-Infrastructure-for-the-AI-Agent-Eco-23247e47b1ea8067b923d998364e3ced) . Most importantly, infrastructure is a trust business and YC's acceptance of our vision and initial $500K funding went a long way towards convincing our users that we had the expertise and potential to succeed in a very competitive marketplace.


## We built a fast, scalable sandbox platform for AI agents


It seems hard to believe now, but we started 2025 without sandboxes! In March 2025, we started work on a new runtime for faster cold starts, for agent and MCP server hosting. However, we quickly realized that there was a need for secure, isolated cloud compute environments that AI agents could use to run arbitrary code and that humans could use to preview the results.


This realization led us to launch sandboxes as a new product line in April 2025, giving users complete, secure isolation for their agentic workloads. Our scalable architecture lets users keep millions of secure sandboxes on "warm standby" indefinitely, eliminating cold starts and state management problems.


These sandboxes also run on the same network backbone that we use for agent and MCP server hosting, eliminating latency through agent-sandbox co-location and enabling near-instant experiences for end-users. Instead of being an "agent hosting platform with sandboxes", we're now a "sandbox platform with agent hosting".


With sandboxes launched, we turned our attention to improving the developer experience. We released a new version of the[Blaxel CLI](https://docs.blaxel.ai/cli-reference/introduction) with support for sandbox management, as well as comprehensive[documentation](https://docs.blaxel.ai/) to help developers get started quickly.


## We upgraded our infrastructure to deliver sub-25ms cold starts


We started 2025 with our[Mark 2 infrastructure](https://docs.blaxel.ai/Infrastructure/Gens#mark-2-infrastructure) , which used containers to run workloads. It provided emulation of most Linux system calls but was relatively sluggish: cold starts typically took 2-10 seconds. This is too slow for agentic systems, which can chain dozens, or even hundreds, of tool calls to achieve their goals. A 2-second delay per call can quickly add up, leading to a poor end-user experience. The alternative - keeping a container always running - was not viable, as the cost for the amount of compute required was prohibitive.


Across 2025, we have worked diligently and efficiently to reduce this delay and create near-instant experiences. Our[Mark 3 infrastructure](https://docs.blaxel.ai/Infrastructure/Gens#mark-3-infrastructure) , now[in production](https://docs.blaxel.ai/changelog#2025-10-30) , leverages micro-VMs to create deployments with sub-25ms cold starts. We monitored (and continue to monitor) customer feedback and added multiple features to our Blaxel[CLI](https://docs.blaxel.ai/cli-reference/introduction) ,[SDKs](https://docs.blaxel.ai/sdk-reference/introduction) and[sandbox API](https://docs.blaxel.ai/api-reference/introduction) in response to customer requests.


Beyond the API and SDKs, we made deep technical improvements to our networking and compute stack.


- We redesigned our network infrastructure and built our own HTTP/TCP proxy in Rust to successfully[cut latency](https://docs.blaxel.ai/changelog#2025-09-28) across our network backbone to under 50ms for the vast majority of use cases.
- We forked Firecracker and replaced its networking layer with vector packet processing for higher throughput and scalability.
- We built a custom micro-VM orchestration and deployment stack on top of Kubernetes that allows our users to run thousands of instances of arbitrary, AI-generated code with low latency and complete code isolation.
- We also added support for[volumes](https://docs.blaxel.ai/changelog#2025-09-28) , which provide persistent storage for agentic workloads across VM lifecycle events.


All these improvements have made our infrastructure faster, more scalable and more feature-rich. We are currently serving more than 7.5 million requests per day and processing billions of Gb-sec per month.


## We grew our customer base...and our team!


Our sandboxes are seeing rapid adoption, with customers ranging from small startups to large tech companies now using Blaxel to power their AI workloads. Our customers love the ease of use and flexibility of being able to keep[unlimited sandboxes on automatic standby](https://docs.blaxel.ai/Sandboxes/Overview) , while only paying for what they actually use. Our sandboxes are enabling customers to build fast, innovative agentic applications at scale. As just one example,[Webflow](https://webflow.com/) uses Blaxel sandboxes to power their AI coding agent and to provide their users with real-time previews of AI-generated code.


As our customer base has grown, so has our team. We've gone from 6 co-founders at the start of 2025 to a team of 9 at the end of the year. In parallel, we're transitioning from remote to in-person work, and taking on a new office in the heart of San Francisco so we have more room. We're also hiring - check out our[job openings](https://wellfound.com/company/blaxel/jobs) if you're interested in joining us!


2025 might be almost done...but we're not! We're continuing to move forward on our mission to build the best possible infrastructure for AI agents, and we have big plans for the future. Keep watching this blog to learn more, and join our[Discord](https://discord.gg/enAfyZFWHW) to tell us what you think!
