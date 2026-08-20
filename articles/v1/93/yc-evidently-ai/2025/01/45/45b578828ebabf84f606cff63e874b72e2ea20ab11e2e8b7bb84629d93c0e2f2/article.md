---
schema_version: "1.0.0"
document_id: "45b578828ebabf84f606cff63e874b72e2ea20ab11e2e8b7bb84629d93c0e2f2"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/evidently-api-change"
published_at: "2025-01-24T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:78618356be9d19ab5cd2dfccda9e8dd671e71875b455fb9115e4c9a413bcacc0"
---

# Upcoming Evidently API Changes

The Evidently API is evolving — and it’s getting better! Be among the first to explore the new features and improvements.


## What's happening?


We are updating the Evidently API to make it simpler, more flexible, and easier to use. As we focus more on[LLM-related evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) , we’re also working to futureproof the library for these workflows.


These changes affect both the open-source library and the way how you programmatically interact with Evidently Cloud. We invite everyone to try out the new API and share feedback to help us refine the experience!


Starting today, you can access the updated API by importing it from` evidently.future` .


> Explore the updated[documentation](https://docs.evidentlyai.com/) and examples to dive in.


**New to Evidently?** Welcome! Start with the new Quickstart Guides tailored for[LLM](https://docs.evidentlyai.com/quickstart_llm) or[ML](https://docs.evidentlyai.com/quickstart_ml) use cases.


**Already a user?** Be sure to read the “What Changed” section below to understand what’s changing and ensure a smooth transition.


These changes are paving the way for the Evidently 1.0 release, where the new API will become the default. **The current API will remain available for a while, giving you time to adapt.**


## What changed?


Here’s the breakdown of what’s new and improved.


### Metrics redesign


The Metric object in Evidently used to be pretty complex. As the library evolved, Metrics grew organically and acted like widgets, bundling multiple calculations and visualizations together. Parsing Metric results, even for our own dashboards, was tricky because each one had its own unique JSON structure.


Now, the Metrics are much simpler. Each one has a fixed structure, outputs a single computation result, and lets you specify the visualization type directly as a parameter.


No need to worry about Presets: they are here to stay. You can still use pre-built templates that combine various Metrics for common evaluation scenarios. For instance, **Data Drift Preset** is still available under the same name.


**What’s better now?**


- **Custom Metrics** . Creating your own Metrics is much easier and more intuitive. Your custom Metrics will automatically fit with the rest of the framework, allowing you to use them in Tests, track them on Dashboards, etc.
- **Parsing Metric results.** JSON outputs are now standardized. This greatly simplifies downstream processing.
- **Segmented evaluations** . The new “Group by” feature lets you compute Metrics for specific classes or labels. For example, you can analyze Data Drift for subgroups of users within your dataset.


Other quality-of-life improvements are in the works, such as an option to rename “reference” and “current” datasets.


### Unified Reports and Tests


Reports and Test Suites are now merged into a single object.


If you’ve ever wondered whether to compute a Report or a Test Suite — wonder no more. You can now handle both at once. Each still has its distinct role:


- **Reports** give a visual overview of all computed Metrics, like distribution summaries and value statistics.
- **Tests** let you set and view pass/fail conditions, such as checking for missing data or verifying that all LLM responses in a dataset meet quality thresholds.


What used to be a Test Suite is now an optional extension of Reports. If you choose it, both types of outputs will appear in different tabs of the same HTML file. This way, you don’t have to save two different files and switch between them.


You can still use the auto-generated Test conditions or define them yourself. It’s also now possible to set up relative conditions — like checking if values fall within +/-10% (or 20, or 30…) from the reference.


### Dashboard overhaul


If you’ve been using the Evidently Dashboard to organize Reports and track evaluation results over time, the latest updates improve this experience.


**Simpler API for Dashboard Panel setup** . The dashboards-as-code feature used to be a bit tricky when it came to configuring monitoring Panels. Since now each Metric’s output is standardized, you no longer have to deal with complex paths to point to specific results for plotting.


**Custom Metrics support** . The Evidently UI fully supports custom Metrics. Even if you implemented a custom render for your Metric using Plotly, you’ll now be able to see the stored Report when you save it to your workspace — whether self-hosted or cloud. That’s a big deal!


### Text descriptors redesign


Text **descriptors** are row-level text evaluations — anything from basic checks like text length to LLM-driven evals, such as determining whether an output contradicts its source.


With the increasing focus on LLM-related metrics, we’ve updated the text descriptors API to make it more logical and easier to use.


Descriptor computation is now split into two steps:


- Compute Descriptors and add them to the source table with inputs and outputs.
- Aggregate the results or run conditional checks.


This change is especially useful when you want to perform multiple Tests or aggregations (no need to recompute the results!) or simply get the dataset with scores without the Report.


## Share your feedback!


We know these updates introduce some breaking changes — we don’t make them lightly! A lot of this comes directly from the feedback we’ve gotten from the users.


We implement these updates to make the library more maintainable and pave the way for new features. Our goal is less confusion, less repetition, and more functionality — all in one place.


Check out the[documentation](https://docs.evidentlyai.com/) and examples today, and let us know what you think! If you get stuck or have questions, drop by our[Discord](https://discord.com/invite/PyAJuUD5mB) — we’re here to help! We’d love to hear your feedback! 🚀
