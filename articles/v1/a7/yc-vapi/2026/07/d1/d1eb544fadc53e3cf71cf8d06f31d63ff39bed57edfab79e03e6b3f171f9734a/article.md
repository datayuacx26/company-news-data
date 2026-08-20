---
schema_version: "1.0.0"
document_id: "d1eb544fadc53e3cf71cf8d06f31d63ff39bed57edfab79e03e6b3f171f9734a"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/model-intelligence"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:37a3c79238506144b3150e81f1cf98eb178cfa580ad7a0796411178cbc8dc43a"
---

# Introducing Vapi Model Intelligence: model recommendations backed by production data - Vapi AI Blog

To help builders get started faster and ship better agents, we're launching **Vapi Model Intelligence** , a bundle of features that make it easier to choose the right model combination for your use case. **Model Presets** are curated configurations of transcribers, LLMs, and voice models that are already selected for you and tuned to specific goals, such as low latency or high intelligence. Additionally, updated **model performance metrics** display current cost, latency, and quality data for every model in the catalog based on Vapi production data, so you can see the trade-offs between models and be better equipped to pick the best models for your use case. Both are built on the same foundation: the production data from the calls running through Vapi every day.


Here's why we built it, how it works, and what it changes for you.


## Why we built it


The number one question we get from builders, in webinars, in the community, in support tickets, is the same: "Which models should I use?" Builders in the Vapi Dashboard are excited to combine and customize model configurations, but without guidance, it's hard to know which combinations actually win. And when your use case or goal changes, the best configuration changes with it.


Vapi is model-agnostic: assemble an agent by choosing your own transcriber, LLM, and voice from an ever-expanding catalog of providers and models. That configurability is one of the reasons teams choose Vapi, but while the catalog grows, the range of choices can slow down decisions. New builders don't want to become experts in transcriber word error rates or TTS latency. They just want a working agent. Even power users who want to compare models can struggle without data they can trust.


We're in a rare position to fix that. Builders have run over 1 billion calls on Vapi with use cases spanning appointment scheduling, patient intake, collections, driver dispatch, and dozens of other real workloads. We see how models actually behave in production, not in a spec sheet, and that's the data our own engineers use to select the models inside each preset. Neutrality without guidance just shifts the burden onto the builder. Model Intelligence adds the guidance: a recommendation for every use case, grounded in real deployments, plus the data to go deeper when you want it.


## What is Vapi Model Intelligence? One bundle, two features.


Model Intelligence is a bundle that includes two features solving the same problem from different directions.


### Model Presets


Model Presets recommend a model selection for you, tuned to a specific goal, whether that's cost, speed, or intelligence. Each preset is a curated configuration that bundles a best-in-class transcriber, an LLM, and a voice:


-


**Balanced.** Strong across virtually any use case. This is the new platform default for all new assistants.


-


**High Intelligence.** The most capable models, for complex or high-stakes tasks where accuracy is the priority.


-


**Ultra Fast.** Optimized for the lowest latency, for use cases where speed matters most.


-


**Cost Saver.** Optimized for per-minute cost, so high-volume agents don't break the bank.


Pick the preset that matches your goal and ship, without worrying that you're leaving something on the table. You never have to open a dropdown. If you do edit any component, the assistant moves to a Customized state, so presets never lock you in.


Presets are also designed to be improved over time. Because Vapi stores which preset an agent is on, we can update the underlying models as better ones emerge and suggest the upgrade to everyone on that preset. Nothing changes without your confirmation, so agents already in production stay put. If you opt in, your models are updated, and your configuration improves without a total rebuild.


### Model Performance Metrics


For builders in the dashboard who want control, every model now carries the data to assess it: cost, latency, and a quality metric fit to the job. For transcribers, compare Word Error Rate (accuracy). For LLMs, compare Intelligence scores. And for voices, see our proprietary[Humanness Index](https://humannessindex.vapi.ai/) ™ directly on the platform: a 0 to 100 score of how human a voice model sounds in real-life deployments, as evaluated by the humans who hear them. "Sounds natural in a demo clip" and "sounds natural across thousands of live calls" are different claims, and it's a metric we will use to empower builders that no other platform can surface.


Benchmarks appear in cards and dropdowns, so you can compare models side by side without leaving Vapi. And performance metrics are only useful if you can trust them, so here's how they're measured:


**Latency** is measured on live Vapi calls, not vendor specs. Most performance metrics come from controlled tests: one request, a clean network, no concurrent load, no real conversation around it. Those numbers look fast on a slide and rarely survive contact with production. Our figures are P50 medians from real traffic, so they reflect how a model actually performs in deployment. One thing we learned building this: a number that looks high on paper often feels natural in a real call, so use our numbers to compare models on equal footing, then test perceived latency yourself. It is important for users considering latency to look at model performance in real, live deployments.


**Cost** metrics for the models reflect assumptions based on typical usage in real Vapi calls, but your actual spend may vary depending on factors such as call length, complexity, prompt size, caching rates, tool responses, and more.


**Quality** metrics vary by model type but are pulled from industry benchmarks plus Vapi's own proprietary data, including the Humanness Index for voices, a metric no other platform can surface.


The data is refreshed on a regular cadence, so you're comparing current scores and metrics. The same measurements inform how our engineers select the models inside each preset, so the defaults are chosen by data, not by habit. This data is collected from hundreds of thousands of live voice agent deployments and used to empower our builders to go even further.


## Who is Vapi Model Intelligence for?


**The non-technical PM** building an appointment scheduler and selects Ultra Fast to get more meetings on the books, faster. She ships a low-latency agent without ever comparing transcribers or LLMs.


**The healthcare team.** A care coordinator handling sensitive patient intake calls needs to navigate complex situations, so the team starts on High Intelligence. Later, they compare voices by Humanness Index™ to find the most natural option, rather than running call after call to hear the difference.


**The cost-conscious operator.** A high-volume feedback survey is a repetitive, low-complexity workload. Cost Saver keeps per-minute costs down without a custom build.


**The team without time to chase model releases.** Months from now, when better models are available in the Balanced preset, they'll receive a suggested upgrade. Their agent improves without anyone rebuilding it.


**Everyone else.** Whether you want to start from a stronger baseline or make more informed decisions when you customize, Model Intelligence meets you where you are: better defaults out of the box, and better data when you're ready to go deeper.


## Get started


Model Intelligence, including Model Presets and updated performance metrics, is now live in your account. New assistants start on Balanced. Switch presets or hand-pick models anytime.


Pick the right models for your use case, without the guesswork.


[Open the dashboard](https://dashboard.vapi.ai/login)
