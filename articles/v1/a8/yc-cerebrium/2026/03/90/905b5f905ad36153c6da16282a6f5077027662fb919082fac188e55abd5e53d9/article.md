---
schema_version: "1.0.0"
document_id: "905b5f905ad36153c6da16282a6f5077027662fb919082fac188e55abd5e53d9"
company_key: "yc-cerebrium"
company: "Cerebrium"
source_id: "yc-cerebrium-news-import-eb465eef12b4"
canonical_url: "https://cerebrium.ai/blog/why-serverless-compute-partners-are-now-more-important-than-ever"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-21T13:06:54.599340+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:840b4f567998837b58866499ee9b7be3b455fdfbbb43538f6e2928ad110a174e"
---

# Why Serverless Compute Partners Are Now More Important Than Ever

Why Serverless Compute Partners Are Now More Important Than Ever


AI models are getting better across language, voice, vision, and multimodal tasks - these advancements aren’t happening every few months or years, but rather every few weeks. With each new step change in capabilities, businesses can incorporate these models into more workflows to make them better, faster and cheaper. How would a company change over the course of a year if it could hire an employee who got noticeably better each week, completed its task dramatically faster, and could iterate on feedback instantly? Now how would the same company look if you had 100 or 1,000 of those employees instantly, and they could complete work 24/7?


While there are many reasons most companies are not there yet, the conversation has shifted from “we need better models” to “how can we run this at scale.” Traditional techniques aren’t well suited since these workloads are fundamentally different from the past - large Docker images, multi-GB model weights, and even the runtime has real warmup costs (torch.compile, vLLM, SGLang). This has led many companies to think about how they architect and plan compute infrastructure within their company and how they can keep up with the rapid pace of the industry.


How are these workloads fundamentally different from the past and why are traditional techniques not suitable?


## Handling Bursty Workloads


Before AI, most operations scaled with people. A customer support center, for example, could only handle as many conversations as there were agents on shift. Which imposed a natural ceiling on throughput - you staffed for typical volume, tolerated queues at peaks, and accepted idle time during troughs.


With voice agents, copilots, and automated workflows, the limiting factor is no longer “how many humans do we have on shift?” - it’s “how many requests are arriving right now, and how much compute does each request need?” And that input curve is jagged. It’s spiky. It’s bursty.


AI makes burstiness even more pronounced because work fans out. A single question can kick off multiple tool calls, parallel searches, and concurrent extraction steps before anything gets synthesized into a final response. Even something as simple as “What was Microsoft’s revenue?” can turn into a burst: gather sources, extract the relevant figures, cross-check inconsistencies, then write the answer. It’s no longer one request → one action; it’s one request → a dynamic tree of parallel work.


The same pattern shows up inside engineering teams and is happening within our team. One developer can run multiple coding sessions in parallel, keep a separate “validation” chat open, have an agent execute tests or simulations across the codebase, delegate a linear task to something like Devin, and run an automated code review on a PR - all at the same time. As new frameworks and workflows mature, the amount of parallel work per employee will only increase, which makes compute demand less predictable and more burst-driven.


Finally, many of the most important AI workloads aren’t continuous - they’re episodic. Evaluations, data processing, backfills, re-embedding jobs after a model upgrade, labeling/cleaning runs, and fine-tunes tend to happen in concentrated windows: nightly, weekly, or triggered by releases. When you combine real-time spikes from user demand with periodic internal jobs, AI infrastructure ends up facing a pattern that looks nothing like traditional steady-state services: long quiet stretches are instead punctuated by sudden, compute-heavy surges.


The obvious problem with bursts is waste: you provision for peak and pay for idle.


## **Utilization: The Gross Margin Killer**


Handling bursty workloads in the ML era is notoriously challenging. Spinning up GPU capacity often takes minutes, model weights are measured in gigabytes, and the runtime footprint is heavy - large images, large dependencies, and the initialization time of frameworks like vLLM and SGLang. This is referred to as the cold start problem.


Long cold starts lead to a service quality issue under sudden bursts:


-


Latency jumps (especially tail latency)


-


Real-time sessions start timing out


-


Queues back up, and retries amplify load


-


Autoscalers chase demand too late


Companies combat this by over-provisioning capacity and rather suffer the consequences of reduced margins in order to give users a better experience and provide a high quality of service.


If demand is relatively predictable then you are looking at 75% utilization on a good day however if you have a peaks and troughs during the day you are looking at 50% or less.[The State of AI Infrastructure at Scale 2024](https://ai-infrastructure.org/wp-content/uploads/2024/03/The-State-of-AI-Infrastructure-at-Scale-2024.pdf) report shows that 68% of surveyed companies have GPU utilization below 70% even during peak periods.


Let us take a look at a simple example of how utilization affects your gross margins:


You pay for GPUs per hour whether they’re busy or idle (allocated hours). But you usually earn revenue only when they’re doing work (served hours). Utilization is what converts paid time into billable time.


Start with a basic margin view per GPU-hour you pay for:


` **Gross Margin ≈ 1 − (Cost per allocated hour ÷ Revenue per allocated hour)**`


Now connect revenue to utilization:


` **Revenue per allocated hour = Revenue per served hour × Utilization**`


Substitute that in:


` **Gross Margin ≈ 1 − (Cost per allocated hour ÷ (Revenue per served hour × Utilization))**`


Real example:


-


you charge **$10 per GPU-hour of actual usage**


-


your blended GPU cost is **$6 per GPU-hour allocated** (what you pay, regardless of usage)


Then:


-


at **50% utilization** : revenue per allocated hour = $10 × 0.50 = **$5.00** → GM = 1 − 6/5 = **−20%**


-


at **75% utilization** : revenue per allocated hour = $10 × 0.75 = **$7.50** → GM = 1 − 6/7.5 = **20%**


-


at **90% utilization** : revenue per allocated hour = $10 × 0.90 = **$9.00** → GM = 1 − 6/9 = **33%**


A 15% increase in utilization can be the difference between you making money or wiping out your profits from the day.


Besides the affect on utilization, over-provisioning isn’t just simply implemented because of long cold starts but is becoming a strategy for companies to secure compute.


## **Capacity constraints: over-provisioning isn’t just cold starts anymore**


Cold starts push teams to keep spare capacity online, but there’s a second, increasingly common driver of over-provisioning: **GPU scarcity and fragmentation** . Compute supply has stayed constrained across multiple hardware cycles. The market moved from A100 shortages to pressure on L40S, H100, H200 - and now B200s - and there’s little reason to believe the underlying complexity disappears soon. As demand grows and hardware cycles accelerate, planning gets harder: companies reserve capacity 1–3 years out to secure supply, even when they can’t confidently forecast demand, and those long commitments can lock them out of newer chips as they arrive.


This breaks a core assumption many teams inherited from traditional cloud architecture: *if you’re willing to pay, you can usually get capacity when you need it.* With AI infrastructure, availability is shaped by vendor production cycles, cloud allocation strategies, regional concentration of demand, and long lead times. Capacity is also unevenly distributed—access to one GPU type in one region tells you very little about access to another GPU type in another region. For customer-facing AI products, compute availability becomes a product dependency: if the right hardware isn’t available where you need it, latency increases, reliability degrades, and rollouts stall.


Most organizations then hit an operational mismatch. They’re built around one primary cloud and a small set of regions, because even “simple” multi-region deployments are hard to operate well. But the practical workaround for shortages is diversification: spread workloads across clouds and regions to tap into different capacity pools.


The catch is that multi-cloud + multi-region architectures are meaningfully more complex—especially when you must support heterogeneous GPUs, driver/runtime differences, and workload-specific scaling behavior. Yet businesses are increasingly global, and customers increasingly expect services to run close to users and close to data. So teams end up pulled in two directions at once: diversify to access capacity, but simplify to stay operable.


Besides architecting your system to be multi-region/multi-cloud for access to capacity, the other reason you need it is in order to serve customers globally.


## Be global from day 1


For many AI systems, infrastructure location is no longer only a technical detail; it is a purchasing criterion. Organizations are processing voice data, customer conversations, sensitive documents, healthcare data, financial data, and other regulated information. As a result, enterprise buyers increasingly ask where inference runs, where data transits, what is stored, what remains in-region, and what controls exist around tenant isolation and auditability. These questions now appear early in the sales cycle because they directly affect legal review, risk assessment, and deployment feasibility. In many cases, the inability to support specific regions or residency requirements becomes a go-to-market blocker before it becomes a scaling problem.


For platform and IT leaders, these requirements can’t be satisfied with documentation alone. They require architectural enforcement: region-aware deployment, in-region data handling, auditable controls, and failover behavior that doesn’t accidentally violate the intended topology. The hard part isn’t hosting a model — it’s operating a distributed compute system that can meet performance, compliance, and reliability constraints at the same time.


And beyond compliance, geography directly impacts product quality. Real-time and interactive AI systems are latency sensitive; serving users in India from US-based compute can add hundreds of milliseconds of network latency before your application does any work. That pushes experiences from “instant” to “laggy,” and customers feel it immediately.


So the challenge for modern AI businesses is clear: they need multi-region capability for go-to-market and product experience as well as having access to more pools of capacity.


So how are businesses suppose to manage all these requirements - clearly infrastructure is a vital component for companies and effects everything from unit economics to product experience but its not their core focus/differentiator but rather a expected requirement from customers.


## Choosing the right infrastructure partner is a product dependency


As model capabilities improve, AI will move deeper into production-critical systems. For many teams, the constraint won’t be access to models—it will be the ability to run them with the right economics, in the right regions, while meeting strict performance and compliance requirements. Demand will remain bursty, which makes costs harder to predict and reliability easier to break: you need headroom to handle spikes, but that headroom immediately shows up in utilization and margins. At the same time, GPU capacity is fragmented across chips, regions, and providers, and global customers increasingly treat deployment topology - where inference runs, where data stays, and how failover behaves—as a purchasing requirement, not an implementation detail.


This combination creates a structural mismatch for most teams. Your product team should be focused on model quality, workflow design, safety, and shipping features - not on chasing GPU allocations, renegotiating 1–3 year reservations, maintaining heterogeneous drivers, tuning autoscalers for every framework, and operating multi-region deployments that won’t violate compliance boundaries during failover. Yet those infrastructure details increasingly decide whether you can scale, whether you can sell, and whether your unit economics hold.


That’s why partnering with a serverless compute platform is becoming a default option for most teams over traditional clouds. A good partner absorbs the complexity: dynamic capacity across providers, region-aware orchestration, faster cold starts, production-grade autoscaling, and the operational controls needed for enterprise buyers - while you keep ownership of the parts that differentiate you: product experience, data, models, and customer outcomes. This is exactly what we’re building at **Cerebrium** : a serverless AI infrastructure layer designed for real-time and batch workloads, so teams can deploy globally, adapt to changing hardware, and scale without turning compute into their primary engineering project.
