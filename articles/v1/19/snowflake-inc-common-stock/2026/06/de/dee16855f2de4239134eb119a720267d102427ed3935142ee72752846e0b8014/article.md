---
schema_version: "1.0.0"
document_id: "dee16855f2de4239134eb119a720267d102427ed3935142ee72752846e0b8014"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/claude-sonnet-5-snowflake-cortex-ai"
published_at: "2026-06-30T18:22:07+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:5896bdd759a1bb1cd6aff6824f4eee9ebe4fab52b59f27192706b47dec6331dc"
---

# Announcing Claude Sonnet 5 on Snowflake Cortex AI

We are thrilled to announce the availability of Claude Sonnet 5, Anthropic’s latest model, on[Snowflake Cortex AI](https://www.snowflake.com/en/product/features/cortex/) . As a launch partner with Anthropic, we're providing same-day availability of Claude Sonnet 5 in private preview. It is available within the secure Snowflake perimeter for use with[Snowflake CoCo](https://www.snowflake.com/en/product/features/cortex-code/) ,[Cortex Agents](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) ,[Cortex AI Functions](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions) ,[Cortex Inference](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-llm-rest-api) and[Snowflake CoWork](https://ai.snowflake.com/) .


## Claude Sonnet 5


Claude Sonnet 5 is a fast model with strong general capability. In testing, it performs well on coding, debugging, deep analysis and brainstorming. And, on many agentic tasks, it approaches Opus-level quality, especially at higher effort levels. It is more agentic than Sonnet 4.6; it calls more tools and runs self-verification loops to complete tasks. The model makes better use of persisted memory which improves long-horizon execution in environments like Snowflake CoCo.


Source:[Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5) , Jun 30, 2026


## What you can do with Claude Sonnet 5 on Snowflake


Sonnet 5 joins leading AI models in Snowflake Cortex AI, enabling a wide range of enterprise use cases, all within Snowflake’s security and governance perimeter. With Sonnet 5, preview customers can:


-


**Speed up development with[Snowflake CoCo](https://www.snowflake.com/en/product/features/cortex-code/) :** Snowflake’s AI coding agent understands your enterprise data, governance and workflows, enabling teams to go from idea to production quickly without breaking trust or impacting scale. With Sonnet 5, CoCo enables stronger reasoning, code generation and support for complex, long-running agentic tasks—approaching Opus-level agentic capability at a fraction of the cost. It brings Snowflake-aware assistance directly into CoCo Desktop, Snowsight interface, CLI and local dev tools. For data engineers, analysts and builders, it turns natural language into production-ready pipelines, analytics and AI agents, collapsing weeks of glue work into minutes while staying governed, interoperable and enterprise-grade. You can try Claude Sonnet 5 in preview using the /model command within the CLI:


- **Build advanced personal work agents:**[Snowflake CoWork](https://www.snowflake.com/en/blog/snowflake-intelligence-enterprise-ai/) is the personal work agent for every knowledge worker. It understands your data and your work from Day 1, answers difficult "why" questions with cited research and runs the work on demand, on schedule and across the tools your business already uses. Every knowledge worker, regardless of technical expertise, can reason deeply over structured and unstructured enterprise data, automate routine tasks and accelerate the path from ideas to decisions to action. Claude Sonnet 5 on Snowflake Cortex AI extends this foundation with enhanced reasoning, stronger code generation and support for more sophisticated agentic workflows—approaching Opus-level capability at a fraction of the cost—enabling teams to build agents that tackle increasingly complex, multi-step business problems economically and at scale. With Deep Research for cited, multi-source investigation, proactive automations that deliver briefs and alerts before anyone asks, reusable User Skills that turn everyday work into repeatable processes across the organization and MCP connectors that enable action across Gmail, Jira, Slack and Salesforce, all inside Snowflake's governance perimeter, CoWork helps organizations turn AI into measurable business outcomes.


Snowflake CoWork: A personal work agent for every knowledge worker.


-


**Analyze structured and unstructured data with unparalleled precision:** Cortex AI Functions make it easy to build scalable AI pipelines across multimodal enterprise data using SQL. Cortex AI Functions offer high-performance processing at a lower cost than manually orchestrated AI pipelines, enabling trusted insights across the enterprise while maintaining the security and governance capabilities Snowflake is known for. Sonnet 5 raises the bar for vision tasks, including extracting precise data from charts, tables and scientific figures. Its document-parsing accuracy reaches Opus quality with high accuracy on real financial and medical documents, especially at higher effort on denser content. This strengthens document parsing and multimodal pipelines across Cortex AI. For example, with the AI_COMPLETE function, analysts can apply natural language instructions directly in SQL using models such as Sonnet 5.


-


**Build AI applications closer to your secure data:** Use Cortex Inference to build agentic enterprise applications entirely within the Snowflake security perimeter. Developers can take advantage of new capabilities like Sonnet 5's adjustable effort levels, letting the model trade off reasoning depth against latency per task—higher effort for complex problems, lower for fast, routine calls. Sonnet 5 also validates its own work through built-in self-verification loops, enabling more autonomous operation with greater consistency and less supervision.


-


Developers can get started quickly using the Anthropic SDK by pointing it to their Snowflake Cortex Inference endpoint, enabling a seamless integration with familiar Anthropic tooling.


## Enterprise-ready by design


Enterprises want to move agentic AI into production fast. With Anthropic’s Claude Sonnet 5, agents can handle complex, long-running tasks with greater consistency and reduced supervision, driving real work forward at scale.


With multiple entry points from[CoCo](https://docs.snowflake.com/en/user-guide/cortex-code/cortex-code) to[Snowflake CoWork](https://ai.snowflake.com/) ,[Cortex AI Functions](https://docs.snowflake.com/en/user-guide/snowflake-cortex/aisql) and[Cortex Inference](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-rest-api) , customers can easily adopt this capability and accelerate their path to high-impact, production-ready agentic AI at scale.


## Learn more


-


Read our Snowflake Summit 2026 launch announcement for[Snowflake CoWork](https://www.snowflake.com/en/news/press-releases/snowflake-cowork-powers-the-agentic-enterprise-as-the-personal-agent-for-knowledge-workers-to-work-smarter/)[,](https://www.snowflake.com/en/news/press-releases/snowflake-coco-redefines-enterprise-ai-development-as-the-coding-agent-built-for-faster-easier-and-more-powerful-innovation-anywhere/) and learn more about it[here](https://www.snowflake.com/en/product/snowflake-cowork/) .


-


Read our Snowflake Summit 2026 announcement for[Snowflake CoCo](https://www.snowflake.com/en/news/press-releases/snowflake-coco-redefines-enterprise-ai-development-as-the-coding-agent-built-for-faster-easier-and-more-powerful-innovation-anywhere/) , and learn more about it[here](https://www.snowflake.com/en/product/features/cortex-code/) .


-


Gain insights from structured and unstructured data with Snowflake CoWork via this[quickstart](https://www.snowflake.com/en/developers/guides/getting-started-with-snowflake-intelligence/) .


*Forward-looking statements*


*This article contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.*
