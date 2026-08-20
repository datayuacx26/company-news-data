---
schema_version: "1.0.0"
document_id: "d1a954339a0cca4c3c849f8a03ef9f7d6a40c26a5590be9d431fc8d47450077a"
company_key: "yc-altrina"
company: "Altrina"
source_id: "yc-altrina-news-import-a8da914aea6c"
canonical_url: "https://www.altrina.com/blog/evolving-our-state-of-the-art-browsing-agent"
published_at: "2025-08-17T00:00:00+00:00"
first_seen_at: "2026-07-21T06:06:15.390227+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:cac0d35d5467d8d6efee9bbaff78a98bfa288603b8068f95f7bd7569c864ad39"
---

# Evolving Our State-of-the-Art Browsing Agent

**Note: This blog was published prior to our[rebrand from Tessa AI to Altrina](https://www.altrina.com/blog/tessa-ai-is-now-altrina)**


2025 is the year of AI agents. Over the past eight months, we have continued to lead the competition in browsing agents with[Tessa](https://www.heytessa.ai/) , our first product based on our novel architecture of[Large Neurosymbolic Cognitive Models](https://www.altrina.com/blog/introducing-large-neurosymbolic-cognitive-models) . Today, we're excited to share that Tessa achieves a new state-of-the-art performance on[REAL bench](https://www.theagi.company/blog/introducing-real-bench) , maintaining our status as the leading web browsing agent in the market.


‍


## Tessa on REAL bench


We are proud to announce that Tessa sets a new state-of-the-art performance of 54.5% on[REAL bench](https://www.realevals.xyz/) . Tessa outperforms all web browsing frameworks with publicly available API endpoints, including Anthropic computer use (42.9%), Browser Use (34.8%), and OpenAI CUA (8.0%).


We selected REAL bench because it captures the kinds of challenges that real-world users face— **long-horizon, history-dependent tasks with complex state changes** —rather than just simple navigation or one-off queries. While prior benchmarks such as WebVoyager (on which we remain state-of-the-art with[93% performance](https://www.heytessa.ai/blog/introducing-large-neurosymbolic-cognitive-models) ) remain strong options for evaluating information-seeking and multi-page navigation, they do not cover these richer action and planning-heavy workflows.


REAL bench is a recent evaluation benchmark for web agents, focusing on complex, realistic web browsing tasks that include both information retrieval queries and **history-dependent, state-changing** action flows–such as comparing multiple product options, planning complex itineraries, assembling and placing multi-item orders, and using in-browser messaging–capabilities that were largely absent in earlier benchmarks. Agents operate in high-fidelity replicas of real online platforms spanning 11 sites and 112 tasks. We used the official harness and evaluation SDK to run Tessa, ensuring direct comparability with other publicly released results.


Notably, our state-of-the-art results are a reflection of the native capability of our base agentic framework, and were achieved without memory and learning features that we have been actively researching. These features give Tessa the ability to reason and adapt to the unique features of different users, websites, and tasks, becoming increasingly personalized and better at anticipating your needs over time. We have been seeing further improvements in reliability and generalization with these additional features turned on, and look forward to sharing our findings in an upcoming paper.


‍


## Evaluating models for browsing compatibility


We compared the performance of a wide variety of frontier models in our framework to better understand their suitability for web browsing tasks.


We found that the Claude family of models generally offered the best performance, and models with reasoning abilities generally performed better than their non-reasoning counterparts.


We also measured the cost of running each of our models on REAL bench tasks, and show a performance-cost tradeoff frontier of different models. We hope that this will be informative for developers choosing which model to use for their workflows with the[Tessa Browser Agent API](https://docs.altrina.com/api-reference/quickstart) .


‍


## RAVE: our approach to inference time scaling


Tessa is powered by[Tesseract](https://www.altrina.com/blog/introducing-large-neurosymbolic-cognitive-models) , our implementation of *Large Neurosymbolic Cognitive Models* (LNCMs), and is an example of what we term as the **RAVE** framework. RAVE stands for Reason, Act, Verify, and Evaluate, and is an inference-time control loop that decomposes complex tasks into coordinated stages. Like[ReAct](https://arxiv.org/abs/2210.03629) agents, it integrates reasoning and acting, but extends the paradigm with explicit verification and evaluation phases, enabling stronger self-correction and robustness. Each stage is executed by specialized nodes in our compute graph that draw inspiration from distinct cognitive subsystems in the brain—such as sensorimotor control, working memory, predictive coding, and feedback learning. These components are orchestrated through our storytelling framework, which maintains a coherent, goal-directed narrative across actions. The result is a *world-aware* agent that is grounded in its environment, robust to unexpected states while efficiently tracking hidden or partially-observable states, capable of self-correction, and designed to scale gracefully with advances in frontier models.


‍


## Getting started


We're making Tessa's capabilities available through a public[API](https://docs.altrina.com/api-reference/quickstart) and[SDK](https://docs.altrina.com/sdk/quickstart) , giving you direct access to our state-of-the-art browsing agent with **real-time observability** and **flexible cost controls** . Whether you're automating a single workflow or orchestrating complex, multi-site processes, Tessa integrates seamlessly into your stack.


- **Developers:** spin up tasks in minutes with our SDK and detailed docs.
- **Enterprises:**[book a demo](https://calendar.app.google/tVM9kMmNagXkpmNp6) to explore tailored solutions for large-scale, high-complexity use cases.


Start building with the best-in-class browsing agent today, and stay tuned for our upcoming paper release!
