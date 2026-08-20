---
schema_version: "1.0.0"
document_id: "4b1ef1d493ea5bed0f36e406ac1751d84448a202a4ff5f23e02d5e1b2518e280"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/dynamic-model-routing-open-models-cortex-ai"
published_at: "2026-08-18T16:00:00+00:00"
first_seen_at: "2026-08-18T13:49:38.794299+00:00"
fetched_at: "2026-08-18T16:00:02.945317+00:00"
content_hash: "sha256:fcdd0035b382fb148607c6427aad0b2b6c99953d54d06e1eec4c4c33ac9aeca9"
---

# Dynamic Model Routing & Open Models in Snowflake Cortex AI

AI investment is accelerating, but business value is not always keeping pace. As organizations deploy more agents and AI applications, using the most powerful model for every request can increase costs without meaningfully improving outcomes. The real question is not which model is best. It is which model is best suited to each task and how to choose one at scale.


Snowflake is helping organizations achieve this balance with two new capabilities in Snowflake Cortex AI. Dynamic model routing (PrPr soon) through Cortex AI Gateway provides a unified foundation for governing agent connections, intelligently directing requests and optimizing AI consumption. Snowflake is also expanding its portfolio of open models, including DeepSeek-V4-Flash (PrPr) and GLM-5.3 (coming soon), giving organizations greater flexibility to match each workload with the right model.


## Better AI economics starts with the right model for the right task


The economics of AI improve when organizations stop paying for more intelligence than a task requires. Snowflake calls this intelligence efficiency: the ability to turn compute, models, data and context into measurable business value. The goal is to match every task with the model that can deliver the required quality at the lowest appropriate cost, rather than defaulting every request to the most expensive option.


The principle is simple: Not every task requires the most powerful model available. Generating a weekly status summary does not demand the same level of intelligence as synthesizing risk across an entire portfolio. The challenge is determining which model is best suited to each task and maintaining that mapping as dozens of models continue to evolve and improve.


## Introducing dynamic model routing in Cortex AI Gateway


With dynamic model routing, Cortex AI Gateway selects the most affordable model that can confidently complete the task at each step of agent execution. Lower-complexity and repetitive tasks can be directed to more efficient models, while workloads that require deeper reasoning can be routed to frontier models. This helps customers reduce unnecessary inference spend without requiring development teams to build and maintain model-selection logic. As model choices, capabilities and pricing evolve, routing decisions can adapt without customers having to rebuild their applications or agents.


The capability operates within the enterprise governance controls already established in Snowflake. It considers only administrator-approved models and respects an organization’s existing data residency settings, helping ensure that requests remain within approved compliance boundaries. Each routing decision is also logged, giving administrators and compliance teams visibility into which model handled which request.


Early testing has demonstrated meaningful efficiency gains. In one internal evaluation, dynamic model routing completed a data build tool (dbt) pipeline workload with up to three times greater token efficiency than a frontier-model-only approach, while delivering comparable quality. In a separate coding-workload test, engineering teams maintained the same pull-request throughput while using approximately 25% fewer tokens.1


Because Snowflake maintains the routing layer, model selection can evolve as new models become available and the relative strengths of existing models change. Customers benefit from continuously improving routing without having to redesign their applications or manage complex routing logic themselves.


## Expanding open model access: DeepSeek-V4-Flash 0731 and GLM-5.3


A router is only as good as the options available to it. Dynamic model routing lowers costs by directing each request to the least expensive model that still meets the required quality bar. As the pool of capable, efficient models grows, the economics improves. With each new model addition to the pool, customers gain better performance at a lower cost. That’s why the open model ecosystem matters even more today than it did just a few months ago.


Today we are expanding customers’ access to leading open models, including **DeepSeek-V4-Flash 0731** (in private preview) and **GLM-5.3** (private preview coming soon, subject to change based on model availability3) in Snowflake Cortex AI. These join an existing portfolio that includes models from Anthropic, Google, OpenAI, SpaceXAI, Mistral AI and Meta, giving users more options to find the right combination of quality, performance and cost for each workload.


Evaluated on ADE-bench using Snowflake CoCo as the agent harness, DeepSeek-V4-Flash scores 74.4%,2 outperforming the leading proprietary model we tested. That's not a rounding error; it's a signal that the OSS frontier has caught up where it matters most for data teams. While we’re announcing GLM 5.3 today, the GLM-5.2 scores from past tests tell a compelling story: solid data engineering accuracy (66% on ADE-bench2) with the lowest token footprint of any model in the benchmark, making it a strong fit for teams running high-volume, self-hosted workloads where cost and latency matter.


DeepSeek-V4-Flash 0731 is available in CoCo today in private preview and GLM-5.3 is coming soon, self-hostable and designed to keep your data in your environment.


Snowflake serves these open models itself rather than proxying a third-party API to ensure inference takes places within a secure perimeter. We operate the full path from raw enterprise data to a completed agentic task: the data, the compute running inference, the model weights and the agent harness orchestrating all of it. Several benefits follow directly from that:


-


Inference runs next to governed data, reducing transfer cost and latency.


-


Controlling the stack lets us optimize for the access patterns enterprise workloads actually produce, rather than accepting what a general-purpose endpoint delivers.


-


Your data stays withinSnowflake’s governance boundary, so open models operate inside the same role-based access control and audit trail that already govern your data.


## AI economics that compound over time


Together, dynamic model routing and a growing portfolio of open models can create compounding gains in intelligence efficiency. Dynamic routing reduces unnecessary reliance on costly frontier models by directing routine workloads to more efficient alternatives. At the same time, a broader selection of capable open models gives the routing layer more high-quality, cost-effective options for each request.


The economics improve further as the system learns from real-world usage. As models are evaluated across different workloads, routing decisions become more precise. Tasks can be matched to the appropriate level of intelligence based on quality, latency, cost and governance requirements. As new models become available and existing models improve, a greater share of workloads can be handled by efficient models without compromising the outcomes the business requires.


The result is a system that is designed to become more efficient over time by enabling organizations to progressively lower the average cost of each business outcome while continuing to benefit from advances in model performance. They can do so without rebuilding applications, hardcoding model choices or managing complex routing logic manually. Enterprise governance helps ensure that requests remain within defined compliance, performance and cost parameters, allowing organizations to scale AI with greater control and more sustainable economics.


1 Results based on Snowflake internal testing; methodology and conditions available upon request. Individual results may vary based on workload and configuration.


2 Efficiency score based on internal testing using ADE-bench, a framework created by dbt for evaluating AI agents on real-world analytics and data engineering tasks.


3 This content contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.
