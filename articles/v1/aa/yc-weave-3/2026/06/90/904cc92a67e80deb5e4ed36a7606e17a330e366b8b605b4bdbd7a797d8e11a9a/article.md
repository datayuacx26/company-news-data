---
schema_version: "1.0.0"
document_id: "904cc92a67e80deb5e4ed36a7606e17a330e366b8b605b4bdbd7a797d8e11a9a"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/dora-core-4-metrics-quick-guide-for-engineering-leaders"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T19:33:03.286750+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:fa112d4e5137762e134fe14deb3a2a3e40b705c8c57f2d1d28b3e6033bf3012f"
---

# DORA & CORE 4 Metrics: Quick Guide for Engineering Leaders

# DORA & CORE 4 Metrics: Quick Guide for Engineering Leaders


Are you drowning in a sea of engineering metrics? Between DORA, SPACE, and now CORE 4, it's easy to feel overwhelmed by the pressure to measure productivity without knowing where to even start. You know you need data to guide your team, but navigating the "metric soup" is a full-time job in itself.


Let’s cut through the noise. **DORA metrics** and **CORE 4 metrics** are two of the most powerful frameworks available to engineering leaders today. They aren't competing; they're complementary. This guide will give you a clear, no-fluff explanation of what each framework is, how they differ, and how you can use them together to get a complete picture of your team's performance.


## What Are DORA Metrics? The Gold Standard for Delivery Health


First up are the **DORA metrics** . Born out of the extensive DevOps Research and Assessment (DORA) program, now part of Google, this[research-backed toolkit](https://weaveos.com/blog/dora-metrics-reveal-bottlenecks-drive-faster-delivery) has become the industry standard for measuring the health of a software delivery process. Think of DORA as the vital signs for your CI/CD pipeline.


The beauty of DORA is its balance. It doesn't just look at how fast you ship; it measures whether you're shipping safely and reliably. The four core metrics are divided into two categories: speed and stability.


### Measuring Speed (Throughput)


These two metrics tell you how quickly your team can move an idea from a developer's keyboard into the hands of your users.


-


**Deployment Frequency:** This is simply how often your team successfully releases code to production. It's a great proxy for batch size and indicates your team's ability to deliver value incrementally and quickly. According to DORA's own research, elite performers deploy on-demand multiple times a day, while low-performing teams may deploy less than once per month[\[1\]](https://dora.dev/guides/dora-metrics) .


-


**Lead Time for Changes:** This measures the time it takes from a code commit to that code running successfully in production. A short lead time means your delivery pipeline is efficient, automated, and has minimal friction.


### Measuring Stability (Quality)


Speed without quality is just a fast track to disaster. These next two metrics provide the crucial counterbalance, ensuring you don't sacrifice reliability for velocity.


-


**Change Failure Rate (CFR):** This is the percentage of deployments that cause a failure in production, requiring a rollback, hotfix, or other remediation. According to Datadog, elite teams keep this rate below 15%[\[2\]](https://www.datadoghq.com/knowledge-center/dora-metrics) . A low CFR is a direct measure of your release process's quality.


-


**Mean Time to Recovery (MTTR):** When a failure inevitably occurs, how long does it take your team to restore service? That's MTTR. It’s a key indicator of your team's resilience and its ability to detect and resolve incidents before they impact users.


## What Are CORE 4 Metrics? A Holistic View of Productivity


While DORA is fantastic for understanding your delivery *process* , it doesn't tell the whole story. What about developer experience? Or business impact? This is where the **CORE 4 metrics** come in. CORE 4 is a newer, opinionated framework designed to provide a more holistic view of engineering performance by unifying concepts from[established frameworks like DORA, SPACE, and CORE 4](https://weaveos.com/blog/how-leading-teams-are-rethinking-engineering-analytics) .


Introduced by experts from the DevEx and DORA communities, CORE 4 offers a balanced "report card" for your entire engineering organization[\[3\]](https://www.lennysnewsletter.com/p/introducing-core-4-the-best-way-to) . It’s designed to answer the question: "How *effective* are we as a whole?"


Here are the four pillars:


-


**Speed (Diffs per Engineer):** This tracks the volume of code changes (diffs) per developer over a period of time. It serves as a simple measure of raw output velocity, showing how much activity is happening within the codebase.


-


**Effectiveness (Developer Experience Index - DXI):** This metric moves beyond code to quantify the developer's journey. It’s typically measured through surveys and asks about friction in the development process: Are tools slow? Are code reviews a bottleneck? Is it easy to find documentation? A high DXI score means developers can stay in a flow state.


-


**Quality (Change Failure Rate):** Sound familiar? CORE 4 wisely borrows the **Change Failure Rate** directly from DORA. It acts as the essential counterbalance to the Speed metric, ensuring that an increase in output doesn't come at the cost of stability.


-


**Impact (% of Time on New Capabilities):** This is arguably the most business-centric metric of the bunch. It measures the percentage of engineering effort dedicated to building new, value-adding features versus "keeping the lights on" with maintenance, bug fixes, and paying down tech debt. It helps connect engineering work directly to business outcomes.


## DORA vs. CORE 4: Key Differences at a Glance


So, how do you choose? The good news is, you don't have to. They serve different, complementary purposes. Here's a quick breakdown:


Feature


DORA Metrics


CORE 4 Metrics


**Primary Focus**


Health of the DevOps delivery pipeline.


Holistic engineering productivity and effectiveness.


**Core Question**


"How *efficiently* are we shipping software?"


"How *effective* is our engineering organization as a whole?"


**Scope**


Measures process outputs (speed and stability).


Measures process, people, and business impact.


**Best For**


Optimizing CI/CD, improving release velocity and reliability.


Gaining a balanced view of performance, developer experience, and business value.


While DORA metrics are essential, they can be[incomplete on their own](https://weaveos.com/blog/problem-with-dora) . They tell you *what* is happening in your pipeline, but not always *why* .


## The Smart Strategy: Using DORA and CORE 4 Together


The most effective engineering leaders don't pick one framework over the other; they use them together to get the full story. This "best of both worlds" approach allows you to diagnose your delivery system and understand the human and business context around it. Here's a simple, three-step strategy to get started.


1.


**Start with DORA as your foundation.**


Before you do anything else, you need a baseline for your delivery pipeline's health. Implement the four DORA metrics to get an objective look at your speed and stability[\[1\]](https://dora.dev/guides/dora-metrics) . Remember, DORA is meant to diagnose the *system* , not judge individual developers. Use it to find bottlenecks in your CI/CD process or release strategy.


1.


**Layer in CORE 4 for deeper context.**


Once you have your DORA baseline, use CORE 4 to answer the "why" behind the numbers. For example, if your **Lead Time for Changes** (DORA) is high, the **Developer Experience Index** (CORE 4) might reveal that developers are frustrated with slow build times or a cumbersome code review process. CORE 4 adds the human element to the process data.


1.


**Connect metrics to find optimization opportunities.**


Now, look for patterns and connections between the two frameworks.


-


Is **Speed (Diffs per Engineer)** high but so is your **Change Failure Rate** ? This is a classic red flag. It suggests your team is shipping a lot of code, but the quality is suffering. It’s a signal to focus on improving testing and review practices before pushing for more speed.


-


Is your **Deployment Frequency** low and so is your **% of Time on New Capabilities** ? This could indicate your team is bogged down by technical debt or operational toil, preventing them from focusing on feature work.


## Conclusion


Navigating engineering metrics doesn't have to be complicated. By understanding the distinct roles of DORA and CORE 4, you can build a comprehensive measurement strategy that drives real improvement.


To put it simply:


-


**DORA** is the essential health check for your delivery *process* .


-


**CORE 4** is the balanced report card for your overall engineering *performance* .


Start with DORA to master your delivery fundamentals. Then, layer in CORE 4 to connect that process health to the bigger picture of developer experience and business impact. By combining them, you move beyond simple output tracking to build a complete, actionable story of your team's success.


Are you ready to stop guessing and start building a complete picture of your team's success?
