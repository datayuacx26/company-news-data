---
schema_version: "1.0.0"
document_id: "0538c361098ff308db47e572daa5bcea9081f74e1c2b4af6ef99d365ce56bce4"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/growthbook-version-4-3"
published_at: "2026-02-04T00:00:00+00:00"
first_seen_at: "2026-07-31T19:45:33.762968+00:00"
fetched_at: "2026-07-31T19:45:34.191941+00:00"
content_hash: "sha256:2c5bb9560aa0f1a547d4e5fd315ae0f06d02c99579dada9c695d02c7a29ff1f6"
---

# GrowthBook 4.3: Post-Stratification & Faster Experiments | Growthbook Blog

At GrowthBook, we're focused on helping you learn faster and ship with confidence. GrowthBook 4.3 delivers on both fronts, with **post-stratification** to reach statistical significance sooner, **metric drilldowns** to understand results more deeply, and **feature evaluation diagnostics** to verify your flags are working correctly in production.


GrowthBook 4.3 is now available to all cloud and self-hosted users.


## Experiment analysis


### [Post-stratification](https://docs.growthbook.io/statistics/cuped) **(enterprise only)**


Experiment analysis now supports **post-stratification** , a powerful variance reduction technique that produces more precise results.


Here's the idea: if you know revenue varies by country, post-stratification uses that information to isolate the[treatment effect](https://www.growthbook.io/blog/a-practitioners-guide-to-treatment-effects-in-experimentation-ate-cate-itt-late-att-explained) from between-group noise. The result is tighter confidence intervals from your existing traffic. In the right conditions, CUPED + post-stratification can be equivalent to running your experiment with 20%+ more traffic!


Configure post-stratification at the organization level under **Settings** → **General** , or override it at the metric or experiment level. To enable it, you'll need to have pre-computed dimensions configured in your experiment assignment query.


*Post-stratification is available to Enterprise customers. CUPED (without post-stratification) is available to Pro and Enterprise customers.*


### [Experiment Metric Drilldowns](https://docs.growthbook.io/app/experiment-results#metric-drilldown) (all editions)


GrowthBook experiment metric drilldown panel showing goal metric timeseries data across experiment variations


Understanding experiment results just got a lot easier. Click any metric row to open a **Metric Drilldown,** a focused view with everything you need to interpret that metric without jumping between pages:


- The **Overview** tab shows metric details, time series, and a results table with analysis controls.
- The **Slices** tab lets you see how your metric breaks down across different values.


GrowthBook metric slices view showing average Largest Contentful Paint (LCP) broken out by browser and country for an A/B test


- The **Debug** tab reveals how CUPED, post-stratification, capping, and priors are affecting your numbers.


GrowthBook experiment results debug view showing pre- and post-CUPED variance reduction and metric capping applied to a goal metric


*Metric slices are an Enterprise feature. See*[Metric Slices](https://docs.growthbook.io/app/metrics#metric-slices) *for configuration details.*


### Experiment result filters (all editions)


Experiments with dozens or hundreds of metrics can be overwhelming to review. You can now **filter results by tag, slice, or metric group** to focus on what matters.


Once you find a view you like, use **Add to Dashboard** to save it for later and share with your team. We also cleaned up the results UI to reduce clutter and keep the focus on your data.


### [Daily participation metrics](https://docs.growthbook.io/app/metrics#daily-participation-metrics) (all editions)


We added a brand new metric type: **Daily Participation** . For each user, this measures the fraction of days they were active while enrolled in the experiment (active days ÷ days exposed), then averages that value across users in each variation.


Think of it as DAU normalized per user and exposure window, but more stable for experiments than raw daily active user counts.


This is a really valuable metric for any website or app that is trying to grow daily usage.


### Better fact table filters (all editions)


GrowthBook fact metric filter interface for defining row-level filters on a fact table without writing SQL


Metrics are built on Fact Tables, and often you only need a subset of rows. This release adds a powerful **filtering UI** to define exactly which rows to include, without writing SQL.


## Feature flags


### [Feature evaluation diagnostics](https://docs.growthbook.io/features/diagnostics) (All editions)


GrowthBook feature flag evaluation diagnostics table showing SDK evaluation events from a data warehouse for troubleshooting targeting rules in production


When a flag isn't behaving as expected, debugging can be frustrating: you're left guessing whether the issue is in your targeting rules, SDK configuration, or something else entirely.


**Feature evaluation diagnostics** solves this by querying SDK evaluation events stored in your data warehouse. See exactly what evaluated in production, not just what the rules say *should* happen. Troubleshoot targeting conditions, rollouts, and experiment rules with real data instead of guesswork.


### Nested saved groups (all editions)


Saved Groups now support **nesting** , letting you define groups in terms of other groups. Build complex targeting logic while keeping base definitions centralized and reusable.


For example, combine "Beta Users" AND "Enterprise Plan" to create "Beta Enterprise Users." Update the base group, and nested groups update automatically.


This makes it faster and easier to create targeting rules for feature flags.


### Case-insensitive regex targeting (all editions)


New targeting options for **case-insensitive regex and "in list" matches** —useful for matching email addresses and other values where case shouldn't matter.


Available now in the latest JavaScript, React, Node, and Python SDKs. More SDKs coming soon.


### Rust and Roku SDKs (all editions)


We're excited to announce two new official SDKs:[Rust](https://docs.growthbook.io/lib/rust) and[Roku](https://github.com/growthbook/growthbook-roku) .


Rust is the language of choice for modern performance-critical applications. Special shout out to the[community](https://github.com/will-bank/growthbook-rust-sdk) , who authored the initial version of this SDK.


GrowthBook, now on your TV? That’s right, the next time you watch your favorite show, GrowthBook might be working behind the scenes with the launch of our official Roku SDK, a leading smart TV platform that powers millions of streaming devices and TVs worldwide.


With these additions, GrowthBook now offers **24+ SDKs** spanning client-side, server-side, mobile, and edge.


## Quality-of-life improvements


Big thanks to all of our users who reported bugs, shared feedback, and contributed ideas to this release on[GitHub](https://github.com/growthbook/growthbook/releases) or[Slack](https://join.slack.com/t/growthbookusers/shared_invite/zt-2xw8fu279-Y~hwnfCEf7WrEI9qScHURQ) .


Many small improvements add up to a big boost in usability:


- Improved query performance for fact metrics
- Cleaner experiment results UI with fewer distractions
- OR targeting conditions
- Updated SDK support
- New API endpoints to manage experiment dashboards and custom fields
- New Project Admin role to make it easier to manage a large distributed team
- New Custom Hook option to only validate incremental changes
- Kerberos auth support for Trino/Presto
- Option to auto-update metric slice values
- Support for additional AI models from Anthropic, Mistral, xAI, and Gemini


Plus dozens of smaller fixes and performance improvements.
