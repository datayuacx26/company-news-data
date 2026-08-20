---
schema_version: "1.0.0"
document_id: "02ded4b96c0572b0631716891274df744850554782fe22ddfd4b1e481419593d"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/ai-intelligence-efficiency-dynamic-model-routing"
published_at: "2026-08-19T04:27:00+00:00"
first_seen_at: "2026-08-18T13:49:38.794299+00:00"
fetched_at: "2026-08-19T04:27:02.314405+00:00"
content_hash: "sha256:63980c1fd1415025dfdf8aa6980f57349ccb78d503a2173424678c9e1d907688"
---

# Maximize AI Impact: More Model Choice, Smarter Routing

Across my conversations with customers, a clear tension is emerging. Companies are investing aggressively in AI, but they are also becoming more rigorous about the economics. It is no longer enough to show that employees are using AI or that token consumption is growing. Usage is an input. The question that matters is what a company gets in return.


Companies need to be asking: Are we moving faster? Are we lowering costs? Are we creating better products and customer experiences? Ultimately, AI has to translate into measurable business value.


I refer to this as *intelligence efficiency* , or how effectively a company turns compute, models, data and context into business impact. More revenue. Lower costs. Faster execution. Achieving this efficiency requires model flexibility, both in the freedom to use the best models for the job and in the ability to leverage intelligent routing to match each task to the right model based on cost, speed and performance. The industry has spent the last several years optimizing the capabilities of individual models. Now, enterprises need to optimize the system around them for outcomes.


This shift comes as the rise of emerging open models is changing AI’s cost equation. For many workloads, particularly simpler well-defined tasks, the leading open models deliver strong performance at[a fraction of the cost](https://artificialanalysis.ai/articles/aa-briefcase?utm_source=chatgpt.com#cost-per-task) of frontier models. As open models continue to improve, increased model choice should drive greater competition, faster innovation and lower costs across the ecosystem.


This is where I see a huge unlock for enterprises. As the cost per task falls, the threshold for where it makes sense to apply AI falls with it. Workflows that were once too expensive to automate become economically viable, giving organizations the freedom to experiment, iterate and ultimately deploy AI across a much broader range of their business.


Yet different models occupy different points on the cost, quality and latency curve, and those positions are constantly changing. The model that is best for a task today may not be the best model six months from now. That’s why I believe intelligence efficiency will require enterprises to embrace a flexible portfolio of models.


I’m already seeing this shift in my conversations with customers. The question is moving from *“Which model should we standardize on and where should we apply it?”* to *“How do we continuously choose the best model for every task?”*


At Snowflake, we’ve built a neutral enterprise platform that brings together leading open and proprietary models from **Anthropic, Google, Mistral AI, OpenAI and SpaceXAI,** and Snowflake will be expanding that choice with support for **GLM-5.3** and **DeepSeek-V4-Flash 0731** .


This freedom and flexibility are fundamental to how we think about the agentic enterprise. As models continue to evolve, enterprises should be able to benefit from that innovation across the full scope of their data and context.


This is what we are delivering with Snowflake CoCo and Snowflake CoWork for builders and business users, respectively. CoCo and CoWork seamlessly work across all of your data, enabling you to leverage different models, agents, workflows and systems, all in one conversational interface. The model is an important part of the system, but it should not be something the user has to constantly manage.


That leads to the next challenge I see for enterprises: how to orchestrate AI effectively. That’s why, today, we’re introducing **dynamic model routing in Cortex AI Gateway** to help our customers leverage the right model for the job at hand. Customers can now define which models they approve and the tradeoffs they care about. From there, Cortex AI Gateway evaluates each task against those policies, along with real-world cost and performance data, and selects the best model for each use case.


The system also learns from the results. After one model completes a task, another evaluates the quality of the result, creating a continuous feedback loop that improves future routing decisions. As models get better and the landscape changes, the system can adapt with it, continuously improving quality, cost and latency.


Our initial benchmarks show this approach can deliver better economics at a given level of quality than relying on any single model alone. And because this capability is integrated directly into CoCo and CoWork, our customers can take advantage of price and performance optimization all within their existing agents and workflows, even as new models or capabilities emerge.


Our announcements today reflect a philosophy that has guided Snowflake from the beginning. Major technology shifts create complexity. Our job is to absorb that complexity for our customers. We did that for data infrastructure, and now we’re doing it for AI, giving enterprises the freedom to use the best intelligence for the job while Snowflake manages the complexity of model choice, routing and optimization underneath. That’s how we help our customers realize true intelligence efficiency.
