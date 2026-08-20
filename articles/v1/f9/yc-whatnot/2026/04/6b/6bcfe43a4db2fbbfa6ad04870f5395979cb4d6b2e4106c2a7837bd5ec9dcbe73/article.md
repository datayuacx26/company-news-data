---
schema_version: "1.0.0"
document_id: "6bcfe43a4db2fbbfa6ad04870f5395979cb4d6b2e4106c2a7837bd5ec9dcbe73"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/the-model-is-the-easy-part-building-the-llm-platform-at-whatnot-ec8730fa9bdf"
published_at: "2026-04-14T15:01:05+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:491a948d183b5ff1087fe6fffef7a627cdc0686c6a8c2c3af9eced2977b33b18"
---

# The model is the easy part: Building the LLM Platform at Whatnot

Featured


LLM


Platform Engineering


AI Agent


Llm Evaluation


Prompt Engineering


# The model is the easy part: Building the LLM Platform at Whatnot


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--ec8730fa9bdf---------------------------------------)


8 min read


·


Apr 14, 2026


--


[Stas Sajin](https://www.linkedin.com/in/stas-sajin/) **,**[Faithful Alabi](https://www.linkedin.com/in/faithfulalabi/) **,**[Peiyun Zhang](https://www.linkedin.com/in/peiyunzhang/?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base%3B%2B7Q4vL1HSCy%2FT3EgD%2F5Qmg%3D%3D) **,**[Peicheng Yu](https://medium.com/linkedin.com/in/peichengyu/?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base%3BIBdtFE2CTYGjvyKDtTD5sg%3D%3D) **|** **AI Platform**


## Introduction


A decade ago, one of the more useful ways to explain machine learning systems was with the diagram below: the ML model itself was a tiny box in the middle, surrounded by everything else you actually had to build to make it work in production.


*Figure 1* : *Complexity surfaced by ML systems.*[Source](https://papers.nips.cc/paper_files/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf) *.*


The same thing is happening with LLMs now. Making the API call *is* the small box, maybe even smaller than the one in that original diagram. Calling the model is the easy part. The hard part is everything around it: there is less stable ground truth, inputs are harder to constrain, outputs are non-deterministic, and the system is much easier for users to push in unintended directions. The hard part is giving teams the ability to iterate fast, trust that the system is working, and know that it is getting better.


From our perspective, this is what an LLM platform actually has to solve for. It has to be *reliable* enough to support real product and operational workflows. It has to enable *velocity* , because many of the highest-leverage improvements are small changes that need to move quickly. And it has to create *trust* , so teams can understand output quality, catch regressions, and ship with confidence. These pillars reinforce each other. Reliability makes teams willing to depend on the platform in production. That production usage creates the data and feedback loops needed to build trust. And trust, in turn, makes it much easier for teams to move with velocity, because they can tell whether a change actually helped. The rest of this post is about those three strategic pillars and the concrete actions behind each of them that allowed us to build the LLM Platform at Whatnot.


Press enter or click to view image in full size


*Figure 2: The LLM platform is organized around three self-reinforcing pillars: velocity, reliability, and trust, each supported by a distinct set of platform enablers.*


### We built on foundations that were already there


A big reason we were able to build the platform quickly, and with a small team, is that Whatnot had already spent years investing in the foundations underneath it. This was the result of earlier platform work across the company:[building the modern data stack](https://medium.com/whatnot-engineering/building-a-modern-data-stack-at-whatnot-afc1d03c3f9) ,[scaling investments](https://medium.com/whatnot-engineering/scaling-whatnot-behind-the-largest-live-shopping-stream-in-us-history-040a458f538c) , and strengthening shared tooling and other internal platform primitives. Those investments meant integrations, logging patterns, analytics sinks, and internal tooling foundations were already there when we needed them. Because so much of Whatnot’s business logic is consolidated in a single codebase, exposing existing capabilities through the LLM Platform was often much easier than it would have been in a more fragmented architecture. Teams operating across many languages, many microservices, and many ownership boundaries would have a much harder time following the same path.


Press enter or click to view image in full size


*Figure 3: High-level architecture of the LLM Platform at Whatnot, showing how the platform UI, core LLM service, shared middleware, and provider routing connect to the broader company tooling ecosystem.*


## Building for velocity


One of the biggest differences between traditional ML and LLMs is who gets to participate.


Traditional ML has a relatively high barrier to entry as a contributor. Even getting a first useful version usually requires specialized workflows and expertise. LLMs are different. The interface is much closer to natural language. If you can explain a task clearly, critique an answer, or describe how something should behave, you already are much closer to making a contribution.


That matters more than it might seem. In many product-facing LLM systems, a meaningful share of the improvement does not come from rebuilding the model. It comes from changing the prompt, refining the examples, attaching the right tools, tightening the evaluation, or adding better context. Those are not purely engineering problems. In many cases, the people best positioned to make those changes are PMs, Ops leads, Policy Experts, and other subject matter experts who are closest to the actual user experience. That is also how we think about the platform internally: it should work for everyone, not just a single group of people.


For us, building for velocity meant building tight iteration loops. Two examples are prompt experimentation and the tool catalog.


## Experimentation


We wanted prompt experimentation to feel like normal product work, not infrastructure work. If someone has a better idea for how the system should behave, they should be able to test it directly in the platform instead of writing code, updating an SDK, or waiting on a deployment. That matters because the people with the best ideas are often not engineers. But making experimentation self-serve does not mean making it loose. These experiments still run through the same experimentation framework and guardrails used in the our app product flow, so teams can move faster without giving up rigor.


In many LLM features, the two prompt variants return the same output most of the time. A standard A/B framework still counts all of those cases as exposures, even when the user experience did not actually change. That dilutes the signal and makes it harder to detect whether a prompt update really helped. So we changed the way exposure is logged: we only count an exposure when the two prompt variants produce different outputs. That lets us focus measurement on the cases where the experiment could actually matter and makes prompt iteration meaningfully faster (10x+ faster).


Press enter or click to view image in full size


*Figure 4: Both prompts produce the same output 80% of the time. Post-exposure logging discards those identical cases and only measures the 20% where outputs actually differ, concentrating the experiment on real signal.* You can learn more about this approach[here](https://careersatdoordash.com/blog/sharpening-the-blur-removing-dilution-to-maximize-experiment-power/) and[here](https://confidence.spotify.com/blog/trigger-analysis) . Press enter or click to view image in full size


*Figure 5: Example of a real experiment where all we did is a model upgrade. Both models generated similar outcomes in the majority of requests and the framework we implemented allows us to narrow attention on cases it matters.*


### Tool Catalog


A lot of LLM product work stops being “just prompting” pretty quickly. The moment a workflow needs live data, or needs to take an action, the prompt alone is no longer enough. The system needs access to functions: look up an order, fetch shipment details, check refund eligibility, or update a ticket. At that point, the question stops being whether the model can call a tool. The harder question is whether teams can expose those functions in a way that is easy to reuse, easy to understand, and safe to test.


That is why we built around a tool registry instead of treating tools as one-off code attached to individual features. Internally, the goal was straightforward: engineers should be able to define a tool once in Python, register it, and have it automatically show up in a shared catalog for prompt authors. From there, PMs and engineers can browse available tools, inspect their schemas, understand what they do, and attach them to prompts without having to wire up custom agent loops or read through implementation details or write access control.


Press enter or click to view image in full size


*Figure 6: The tool catalog makes reusable functions discoverable and self-serve, allowing teams to browse available tools, inspect their schemas, and attach them to prompts for safe testing against real use cases.*


## Building for Trust


When we talk about trust, we do not mean that people should simply feel good about the outputs. We mean that they should be able to look at the data and judge how the system is performing with clear eyes. LLMs are especially good at creating false confidence: a few strong examples can make a system look much better than it really is, and a fluent answer can hide a weak underlying behavior. That is why trust has to come from inspection, measurement, and repeated review of real outputs. We want teams to[stay close to the data](https://www.youtube.com/watch?v=nnhMnNh_ZvU) , because that is the only way to know whether a system is actually working and, more importantly, whether it is improving.


That belief shapes how we think about evals. Our goal is to measure whether the system is actually doing what a human wants. In practice, that means supporting both deterministic and non-deterministic evaluation. Deterministic checks (valid JSON, correct tool calls, required fields) are table stakes in many workflows, but they only tell you whether the system satisfied a mechanical requirement, not whether it did the right thing for the user. The higher-leverage work is messier. This is why we invested in LLM-as-a-judge and calibration workflows that let the domain expert teach the evaluator directly. LLM judges help because they can score open-ended responses against a rubric in a way that is much closer to human review. They replace a lot of the brittle rules, similarity metrics, and manual spot checking that do not scale well and often fail to capture what “good” actually means. Once that Judge is aligned, it becomes much more than an offline scoring tool: teams can attach it to a real production use case, monitor agreement over time, and catch degradation before it turns into product drift. The next step is just as important: using data mining to automatically surface weird, rare, or high-value examples for human review, so the evaluation set evolves with the product instead of freezing in time.


Press enter or click to view image in full size


*Figure 7* : *This workflow highlights LLM as a judge calibration, where users are able to annotate data and quickly build a judge that gets repurposed in backtesting or in online evaluation. Evaluation rubrics are owned by product teams closest to the use case who can clearly define what “good” means.*


## Building for Reliability


Reliability is what turns an LLM demo into infrastructure. If teams are going to build real workflows on top of the platform, they need more than a model that usually responds. They need high availability, predictable performance, and confidence that provider issues will not immediately take their product down. That is why we have treated reliability as a platform concern, not something each team should rebuild for itself. Concretely, that has meant adding multi-provider support, moving toward fallback behavior by default, investing in observability, and layering in platform primitives like caching, rate limiting, and guardrails. The goal is simple: when a team ships on top of the platform, they should inherit a more reliable system by default, rather than having to assemble one from scratch.


## Conclusion


If there is one theme that cuts across all three pillars, it is that good LLM products do not come from the model alone. They come from tight feedback loops, strong platform foundations, and a clear understanding of what quality means to users. We built the platform this way because the problems we are solving with LLMs are real and specific: helping a trust reviewer process a harassment report in minutes instead of hours, giving a support agent the right context to resolve a buyer’s issue on the first try, making it possible for a seller to get a clear answer about a policy without filing a ticket.


The LLM Platform exists to make those kinds of experiences reliable, improvable, and trustworthy at scale. The model is the small box. Everything around it is what makes it work for people.
