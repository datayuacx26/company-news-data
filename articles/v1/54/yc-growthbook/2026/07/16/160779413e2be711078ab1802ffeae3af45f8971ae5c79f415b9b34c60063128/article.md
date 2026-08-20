---
schema_version: "1.0.0"
document_id: "160779413e2be711078ab1802ffeae3af45f8971ae5c79f415b9b34c60063128"
company_key: "yc-growthbook"
company: "GrowthBook"
source_id: "yc-growthbook-news-import-67cec260d3a3"
canonical_url: "https://www.growthbook.io/blog/how-to-reduce-deployment-risk-with-canary-releases-and-feature-flags"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T15:19:37.358834+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:584d7de2b16d7d91896a64013cf42cb5398b4cf7c0731cfaee4108268a9bde97"
---

# How to reduce deployment risk with canary releases and feature flags

On July 19, 2024, CrowdStrike pushed a configuration update to every one of its Falcon sensors at once. A single logic error crashed 8.5 million Windows systems and cost Fortune 500 companies an estimated $5.4 billion.


It wasn’t a silly mistake that brought down global systems. CrowdStrike tested the update, *but* it didn’t use a staged rollout, so a bug that could’ve been contained to 1% of users became a global outage instead. The problem is that using a delivery process like this just opens up your surface area of risk.


In fact, deploying software change is one of the three top causes of an IT outage. And downtime costs[average at $14,056 per minute](https://www.bigpanda.io/ar-ema-outage-cost-2024/) .


The question is: How do you ship changes into production without turning every release into an all-or-nothing bet?


Using canary releases with[feature flags](https://www.growthbook.io/products/feature-flags) is one way to do that.


In this article, we’ll walk you through the concept of canary releases and how to implement them.


## What is a canary release?


A canary release exposes a change to a small group of users before you roll it out to everyone. The name “canary” comes from a time when coal miners brought canaries underground to detect toxic gases in the mine. If the bird showed any signs of distress, miners left before anything bad happened.


In software development, the same logic applies. A small group of users absorbs the initial deployment risk so the rest of your users can stay protected. This helps even if you have staging and test environments because production traffic behaves differently with real data under real load. Edge cases only show up when you deploy the changes to production.


### Pros and cons of canary releases


**Pros** **Cons**


Limits the blast radius of a bad release to a small, controlled group of users Adds operational complexity to your deployment process because you need clear ramp stages and ownership


Gives you a real production signal that staging environments can't replicate Requires well-defined metrics and monitoring infrastructure before you start


Enables fast rollback through traffic routing or a flag flip, without a full redeploy Low traffic volumes at early stages (1–5%) can produce noisy data, especially for low-volume metrics


Builds team confidence to ship more frequently because each release carries lower stakes Demands a documented flag cleanup process to prevent technical debt from accumulating over time


## What’s the difference between infrastructure canary vs. feature flag canary?


You can run a canary at two different layers of your stack. The layer you choose determines how fast you can roll back.


With an infrastructure canary, you route a percentage of traffic at the load balancer or service mesh level to a new version of your service. You’re releasing the entire service as one unit. And if something breaks, you shift all the traffic back to the older version, which can take several minutes.


A feature flag canary is different. Here, you deploy the code to 100% of your servers but wrap the new behavior/change behind a flag. Only users who match your rollout criteria see it. If something breaks, you flip the flag. That takes seconds.


In short: an infrastructure canary controls which *building* your users walk into. A feature flag canary controls which *rooms* are unlocked inside the same building.


**Infrastructure canary** **Feature flag canary**


**Control layer** Load balancer, K8s, service mesh Application code


**Unit of release** Entire service version Individual feature


**Targeting** Percentage of traffic (often random) User attributes (ID, region, plan, segment)


**Rollback speed** Minutes (redeploy) Seconds (flag flip)


**Best for** New services, middleware, and auth changes Feature-level changes, UI, business logic


Mature engineering teams use both models.


For instance, Facebook[deploys code continuously](https://engineering.fb.com/2017/08/31/web/rapid-release-at-massive-scale/) to all production servers but gates every feature through Gatekeeper, its internal feature flag system. Its employees see the new features first and assess their performance. Only when everything looks good do they roll out the change to 2% of production traffic and test it there.


## How to do a canary release with feature flags?


Let’s say you’re launching an[AI tutor product](https://www.growthbook.io/customers/khan-academy) within your edtech platform. It’ll call an LLM, so there are new risks to consider, like timeout errors or unpredictable token costs.


Here’s how you can do using GrowthBook’s feature flagging platform by using[feature flags](https://www.growthbook.io/blog/what-are-feature-flags) for the canary release:


### Step 1: Wrap the change in a flag


Create a new boolean flag called ai-assistant with a default value of false. It’ll decouple deployment from release, so nobody sees it unless somebody should.


Here’s what it looks like in GrowthBook:


```text
if   (gb.isOn(  "ai-assistant"  )) {


return   renderAIAssistant();     // new LLM-backed path


}


return   renderManualFlow();         // existing path
```


gb.isOn() evaluates the flag locally without a network request, so it adds zero latency to your code path.


Deploy this to 100% of your servers with the flag set to false in your production[environment](https://docs.growthbook.io/features/environments) . This step helps you deploy code to development, staging, and test environments, but it won’t appear in production unless you enable it.


### Step 2. Define your rollout ladder


A rollout ladder is the sequence of stages your flag moves through before reaching 100%. Typically, you monitor specific metrics at each stage before moving to the next.


If you’re using GrowthBook, you can use the following features to do this:


- **Internal team only** : In GrowthBook, create a[Forced Value](https://docs.growthbook.io/features/rules) rule targeted at a[Saved Group](https://docs.growthbook.io/features/targeting) of employees or design partners. You can also target by attribute (email contains yourdomain.com or plan = “internal”). Everyone else falls through to false.
- **1–5% of users** : Add a Percentage Rollout rule below the forced value. GrowthBook uses deterministic hashing based on user ID, so the same user always gets the same experience—no server-side session storage required.
- **10–25%** : Bump the rollout percentage if metrics look healthy.
- **50% → 100%** : Full rollout.


**Note:** The riskier the change, the smaller the interval should be. But make sure you add an approval step based on parameters like error rates or similar criteria to gain more control over the process.


*GrowthBook offers the ability to schedule and automate feature rollouts with approval gates*


### Step 3. Define success and guardrail metrics before you ship


Before you enable the flag, set up two categories of metrics: success metrics and guardrail metrics. While **success metrics** tell you whether the feature is doing what you built it to do, **Guardrail metrics** tell you whether it’s breaking something else.


In GrowthBook, you define these as[Fact Tables and Metrics](https://docs.growthbook.io/app/fact-tables) that are directly connected to your data warehouse. Each metric has a type:


1. **Proportion** for rates (error rate, timeout rate)
2. **Mean** for averages (cost per request)
3. **Quantile** for percentiles (p99 latency)
4. **Ratio** for derived measures like tokens per resolved query


**Type** **Measures** **AI tutor example**


Proportion % of users for whom an event happened (binomial) LLM error/timeout rate, % sessions hitting a fallback


Mean Average value per user Avg latency, cost per request (token spend), avg messages/session


Quantile A percentile of a distribution p95 / p99 latency (the metric that matters for LLM calls)


Ratio One metric/another Tokens per resolved query, cost per accepted answer


Retention Whether users return in a window Next-session return after using the[AI feature](https://www.growthbook.io/industry/ai-software)


You can then mark each metric as either a **guardrail** (a hard gate that can trigger a rollback) or a **signal** (a leading indicator you watch but don’t gate on).


### Step 4. Enable the flag, monitor, and ramp up accordingly


In GrowthBook, every flag change goes through a draft-to-review-to-publish workflow. The goal is to make sure changes don’t reach real users unless you want them to. Once you’ve defined all the changes and metrics, roll out the change and monitor your metrics.


For example, if you’re launching the AI tutor, start with the internal dev team and monitor error rates. Are the AI responses rendering correctly? Is the fallback path working when the flag is off? Is the UI responding well?


Once you’re confident, roll it to real users and ramp up slowly.


### Step 5. Automate the rollout with guardrail monitoring


You can use[Monitored Ramp-Up in GrowthBook](https://docs.growthbook.io/features/safe-rollouts) to automate this whole process. You define a ramp schedule, attach guardrail metrics, and the platform moves on, or rolls back the changes based on what the data shows.


*Monitor the rollout using the metrics you’ve defined and make better decisions*


GrowthBook ships with a built-in preset:


- Linear ramp
- Fast ramp (three steps)
- Approval-gated milestones (define any ladder and any hold time)


For the AI tutor, your ramp config might look like this:


**Stage** **Coverage** **Hold time** **Monitored** **Gate**


Step 1 10% 24 hours Yes Auto-advance if guardrails pass


Step 2 50% 24 hours Yes Requires manual approval


Full rollout 100% — — Full rollout


You can use the following metrics to monitor the rollout:


- **Guardrail metrics** (hard gates which trigger a rollback): llm_error_rate, p99_latency, cost_per_request
- **Signal metrics** (watch, don't gate): answer_acceptance_rate


**Note: If you prefer to automate this process, just use GrowthBook’s flag-monitoring AI agent skill via your MCP and create the ramp up schedule in minutes.**


The first step ramps to 10% and monitors for 24 hours. The next one ramps it up to 50% and waits for manual approval before proceeding. The guardrail failures trigger an automatic rollback.


**Tip:** When you need to pull the plug completely, GrowthBook has a[built-in environment toggle](https://docs.growthbook.io/features/environments) on every flag so if you disable it in production, the SDK stops evaluating all rules simultaneously. It acts as an instant kill switch. It’ll turn off the previous flag and stop evaluating every rule immediately. The SDK returns the default value for all users, regardless of the ramp schedule.


*Example of a kill switch for a finance app*[Source](https://www.growthbook.io/blog/5-ways-to-use-feature-flags-for-smarter-releases)


### Step 6. Clean up the flag


Once you’ve confirmed that the AI tutor feature works well, it’s time to clean up the flag. A[2022 study on feature](https://dl.acm.org/doi/10.1145/3510466.3510485) toggles found that 33% of toggles interact with other code expressions—and those interactions grow 22% each year. So, the longer a flag stays in your codebase, the more entangled it becomes.


Use GrowthBook’s[stale flag detection](https://docs.growthbook.io/features/stale-detection) to automatically surface flags that have been fully rolled out for more than two weeks. You can even use[Code References](https://www.growthbook.io/blog/code-references) to show you where the flag is within your codebase.


When you’re ready, archive it and check if there’s a change or regression in your app. If not, delete it.


**Tip:** Schedule the cleanup the minute you create the flag. You can add a note in your calendar and use[GrowthBook’s MCP](https://www.growthbook.io/platform/mcp-server) to clean it up later. In fact, you can use the[flag-cleanup skill](https://github.com/growthbook/skills) to find these flags and archive them from your AI app.


*Example result within Claude when you use GrowthBook’s AI skills to surface stale flags*


## Which metrics should you monitor during a canary release?


Here are three categories of metrics you should monitor with every canary release:


### Infrastructure metrics


These tell you whether the servers are handling the new code path.


**Metric** **What it tells you**


Error rate (HTTP 4xx/5xx) Whether the new code path is producing more server-side failures than your baseline. This is usually the first signal that something is wrong.


p95/p99 latency Whether response times are degrading. For anything involving LLM calls, it's the metric that moves first.


CPU and memory usage Whether the new code path is consuming more compute than expected. This is especially relevant for features that introduce new external API calls.


### Application metrics


These tell you whether the feature itself is working.


**Metric** **What it tells you**


Feature-specific conversion rate Whether the feature works as intended. In the AI tutor example, that's the task completion rate after seeing an AI explanation.


Client-side error events Whether the new path is throwing errors that users can actually see. Server-side logs don't always catch what happens in the browser.


Support ticket volume Often a leading indicator. Users report problems before your dashboards catch them.


### Business metrics


These tell you whether the change is moving the needle in the right direction.


**Metric** **What it tells you**


Revenue per user Whether the change is affecting business outcomes. This metric moves slowly, so track it over days.


Session engagement with the new feature Whether users are actively interacting with the change once exposed. Low engagement can signal discoverability or trust issues before conversion metrics show it.


Next-session retention Whether users come back after encountering the new feature. A drop here can signal friction that conversion metrics don't capture.


**Tip:** Always set your thresholds *before* you ship so you can act in real time rather than decide whether to act. Once your data team defines a metric in SQL, GrowthBook references it directly[in your data warehouse](https://www.growthbook.io/platform/warehouse-native) and notifies you accordingly.


## 6 mistakes to avoid while using canary releases


Even with canary releases, you can still[experience failure modes](https://www.growthbook.io/blog/12-common-feature-flag-mistakes-to-avoid) . Here’s what to avoid:


1. **Treating the canary as your test suite:** A canary is supposed to catch edge cases that pre-production testing can’t. So, if you’re using it to find issues that unit and test integrations should’ve caught, you’re just testing in production without real guardrails. Run your pre-canary gates (contract tests, integration tests, performance smoke tests) before the actual release to avoid this.
2. **Not defining rollback criteria before shipping:** You don’t want to spend time figuring out which metrics are “bad enough” to trigger an incident response. It’ll cost you time, money, and resources you’d rather spend elsewhere. Define your thresholds before the flag goes live. For example, which metrics you’ll track, when you’ll decide to act on them, and how you’ll act (your incident or remediation response).
3. **Rolling out the feature too quickly:** If you need robust test results, it doesn’t make sense to jump from 1% to 10% in a minute. Ideally, you should test the initial rollout for 30 minutes to a few hours or longer if you’re doing an infrastructure canary release. Over time, you can set your own thresholds based on previous experimental releases.
4. **Using a canary cohort that doesn’t represent your users:** If your canary group is only employees, only one region, or only power or enterprise users, you can’t generalize your results. Unless it’s something you know applies only to a specific segment, it’s best to test with a broader segment first and then drill down over time. Alternatively, run release experiments with different cohorts to see how they respond, then generalize the results.
5. **Letting a canary sit at partial rollout for weeks:** If your rollout is stuck at 10% for weeks at a time, you have a zombie canary. It creates a forked production environment with continuously diverging behavior. Avoid this by setting a maximum dwell time for each stage, especially when automating the process. Once it crosses that threshold, either promote it to the next stage or roll it back.
6. **Skipping session stickiness:** Without sticky assignment, a user might see the new version on one page load and the old experience on the next. It confuses them, and you won’t be able to realistically measure how the new version is performing.[Platforms like GrowthBook](https://www.growthbook.io/) handle this by using deterministic hashing based on user ID, so the same user sees the same flag state as long as the release is in progress.


## Use canary releases to stop treating deployment as an all-or-nothing bet


After the July 2024 outage, CrowdStrike has adopted the “phased release” approach, which is essentially a canary release. Now, updates[roll out in concentric rings](https://www.techtarget.com/whatis/feature/Explaining-the-largest-IT-outage-in-history-and-whats-next) , with monitoring at each stage, and customers can choose to adopt the change early or opt out altogether.


And this is the exact approach engineering teams should adopt.


With the pace of software development increasing by the day, especially with AI, you don’t need to[add unnecessary complexity](https://www.growthbook.io/blog/when-companies-adopt-feature-flags) around every release. A canary release simplifies this process and removes any anxiety around it.


If you’re ready to start adopting a canary release approach with feature flags,[try GrowthBook for free](https://www.growthbook.io/get-started) or[book a demo](https://www.growthbook.io/demo) .


‍
