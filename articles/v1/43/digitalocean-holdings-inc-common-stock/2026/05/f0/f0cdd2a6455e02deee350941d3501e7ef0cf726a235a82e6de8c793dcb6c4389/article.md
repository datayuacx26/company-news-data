---
schema_version: "1.0.0"
document_id: "f0cdd2a6455e02deee350941d3501e7ef0cf726a235a82e6de8c793dcb6c4389"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/digitalocean-opencode-inference-routers"
published_at: "2026-05-28T21:02:42.819+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:732b1122fa742e170ce9aa729193161787b30c0276e2f3ff5abb6a78abaa2280"
---

# OpenCode Now Supports DigitalOcean Inference Router for Intelligent Model Routing

Coding agents today have a massive spending problem. Every request, whether you’re designing system architecture or writing a single-line docstring, often gets routed to the same expensive frontier model. The result: unnecessary token usage, higher inference costs, and little awareness of task complexity or budget constraints.


This high cost stems from a “one-size-fits-all” approach to model usage, where premium frontier models are utilized for trivial tasks that don’t require such intensive reasoning effort. In multi-agent workflows, where orchestrators delegate work to specialized subagents, this lack of discrimination frequently leads to runaway costs and opaque failure modes. Without intelligent routing, developers can essentially be forced into closed-provider lock-in and high API usage fees, which quickly escalate during exploratory building phases.


[DigitalOcean Inference Router](https://docs.digitalocean.com/products/inference/how-to/use-inference-router/) , now in Public Preview, was built to solve this problem by dynamically routing requests to the right model for the job. As part of DigitalOcean’s AI-Native Cloud, it gives developers a unified way to control, optimize, and evaluate AI inference across models. And as of today, you can access it through[OpenCode](https://opencode.ai/docs/providers/#digitalocean) , the open-source AI coding agent, in as little as a few seconds.


## What is an Inference Router?


An Inference Router is the auto-mode pattern engineers are used to, but with deliberate control over the tradeoffs that matter: latency, cost, and output quality. Rather than statically pointing your coding agent to a single model, an Inference Router can analyze each request and route it to the model best suited for that specific task. Not the *most* powerful model available, but the *right* model. That distinction is what drives real savings without compromising on your desired quality of output.


To use DigitalOcean’s Inference Router: Create an Inference Router from the[router catalog](https://cloud.digitalocean.com/model-studio/router/catalog) —pick a preset or build a custom router via the API or UI. No GPU management, no infrastructure to run. Use it by setting “model”: “router:your-router-name” in any OpenAI-compatible API call.


## What Changed for OpenCode


OpenCode has become one of the most popular AI coding harnesses on GitHub,[earning over 160,000 stars](https://github.com/anomalyco/opencode) by embracing a simple idea: developers should not be locked into a single model provider. Its rise has shown a demand for provider agnostic AI use cases. At Deploy 2026, Tyler Gillam - a core engineer on Inference Router -[demoed our integration live on stage](https://youtu.be/HYELtHc7QRo?si=JeUlp8DRHQh00utY&t=2339) , showing exactly how OpenCode and Inference Router work together to make intelligent model selection decisions in real time. If you want to see it before diving in, the full recording is linked at the bottom of this post.


Previously, integrating DigitalOcean models into OpenCode meant manually editing your` opencode.json` , adding each model by hand, a list that would be outdated within weeks given the pace of new model launches. So, we built a native OpenCode integration that supports Inference Routers and DigitalOcean Serverless Inference models right out of the box.


Now you can run the following steps:


1. Launch OpenCode (desktop, web or TUI) and run` /connect`
2. Select **Login with DigitalOcean**
3. Your Inference Routers are shown in the Model Selection tab


That’s it. You’re plugging directly into a routing layer that’s already helping to make the cost & quality tradeoff decisions for you based on your stated needs — with our purpose-build Software Engineering preset.


## Beyond Coding Agents


This integration is part of a broader effort to bring DigitalOcean’s Inference Engine into the tools developers already use, while continuing to invest in open source and upstream contributions. OpenCode is one example of that direction.


The goal is to make intelligent, cost-aware model routing the default for coding agents, not something you have to manually configure and hope for the best. As the OSS model landscape keeps improving, routing intelligence will become more valuable, not less. The gap between “frontier” and “good enough” is closing fast, and developers who take advantage of routing will consistently come ahead on both desired quality and cost.


If you’re using OpenCode, try` /connect` today. If you want to dig deeper on what Inference Router is and how it works, the full documentation is available below.


Inference Router Resources:


- [How We Built DigitalOcean Inference Router](https://www.digitalocean.com/blog/inference-router-architecture)
- [Inference Router Documentation](https://docs.digitalocean.com/products/inference/how-to/use-inference-router/)
- [OpenCode DigitalOcean Install](https://docs.digitalocean.com/products/inference/how-to/use-with-coding-agents/#inference-coding-agents-opencode)
- [InferenceRouter OpenCode Deploy 2026 Demo](https://youtu.be/HYELtHc7QRo?si=JeUlp8DRHQh00utY&t=2339)


### About the author


Musa Malik


Author


AI/ML Engineer


[See author profile](https://www.digitalocean.com/community/users/musamalik)


Musa is an AI/ML Engineer on DigitalOcean's Agentic Inference Cloud team, working on Plano & Inference Router. He joined DigitalOcean in March 2026 through the acquisition of Katanemo Labs, where he was a core engineer. He writes about LLM infrastructure, agentic systems and developer experiences for AI applications.


[See author profile](https://www.digitalocean.com/community/users/musamalik)
