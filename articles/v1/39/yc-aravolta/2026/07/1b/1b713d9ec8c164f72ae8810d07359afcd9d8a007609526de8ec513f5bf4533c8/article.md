---
schema_version: "1.0.0"
document_id: "1b713d9ec8c164f72ae8810d07359afcd9d8a007609526de8ec513f5bf4533c8"
company_key: "yc-aravolta"
company: "Aravolta"
source_id: "yc-aravolta-news-import-85aed3e09b97"
canonical_url: "https://www.aravolta.com/blog/gpu-depreciation-curve"
published_at: null
first_seen_at: "2026-07-21T07:27:31.872668+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:6d19ee990bcbb8e9213f3d948a087b39ce0f51fc251c0474c6639929915888c5"
---

# What's the Real Depreciation Curve of a GPU? | Aravolta Blog

## The Debate Everyone's Having


There's a lot of discussion right now about *how long GPUs last.* Everyone seems to have a different answer:


- **CoreWeave** publicly assumes **~6 years** of useful life
- **Nebius** pegs useful life closer to **4 years**
- Some analysts and investors warn it might be **~3 years** or less under heavy use
- If it were up to Michael Burry, it'd probably be **6 months** before the AI hardware bubble collapses


The truth is, **everyone is wrong** because they're all using *averages* . From the telemetry patterns we see while working with lenders in the space, it's clear that **identical GPUs can age very differently** depending on workload, thermals, and utilization profiles.


The old world runs on averages. The new world runs on **telemetry** .


## Why the Old Model Fails


Today, most GPU financing deals assume a uniform, one-size-fits-all depreciation curve. The underwriting might go something like:


"This GPU will retain economic value for 6 years."


That assumption treats a GPU like a truck or an oil rig on a straight-line schedule. But GPUs don't age like trucks, servers, or drilling equipment. **Identical GPU hardware can age very differently** depending on how it's used:


- A GPU running steady inference at, 60–70% utilization, under moderate thermals, day in and day out
- **vs.** a GPU running irregular **training** workloads that repeatedly spike to 95–100% utilization and push thermal limits every afternoon


On paper these two might be the *exact same model* of GPU. In practice, their aging is **radically different** . One might still be going strong and profitable after 5+ years (indeed, some 2016-era GPUs are *still* in active cloud service), while the other might effectively be worn out - or at least no longer economically viable in 3 years or less.


Yet today many lenders **underwrite both the same.** They assume a uniform depreciation schedule regardless of workload. This leads to:


- Incorrect **salvage value** assumptions (overestimating how much a heavily used GPU will be worth in 4–5 years)
- **Negative leverage** – debt pricing that overshoots the asset's true income potential (the cost of financing can exceed the asset's ROI if life is shorter than expected)
- Increased lender exposure if **utilization assumptions** are wrong (lenders holding the bag on hardware that wore out faster)
- Borrowers stuck with covenants and loan terms designed for *fantasy* workloads, not their real use case


In short, financing GPUs on simplistic average assumptions can misprice risk by a wide margin. **Averages hide the extremes.** And in a fast-evolving market like AI infrastructure, the extremes matter.


Example Aravolta telemetry dashboard showing real-time GPU utilization, memory usage, temperature, and power signals. This is the level of hardware-level visibility lenders and operators use to compare expected workloads against how assets are actually being driven in production.


## What We Found in the Field


Consider a mid-market lender financing several GPU deployments in the **0–50MW** range (hundreds of high-end GPUs across multiple customers).


**Their original underwriting assumed:**


- ~80% **steady** utilization on each GPU (a consistent workload level)
- ~5.5 year **useful economic life** for the GPUs (before resale or obsolescence)
- No meaningful variance across different customers or workload types (every GPU in the fleet treated uniformly)


**But when real telemetry data was collected at the GPU level, here's what was actually observed:**


Factor Expected (Underwriting) Actual Observed Impact


Workload intensity (spikes to 95–100%) "Occasional" spikes assumed **Happening daily** - frequent max utilization spikes causing accelerated component stress and wear


Thermal envelope violations Not modeled (assumed nominal) **Frequent during training bursts** - many GPUs regularly ran above recommended thermal limits, reducing lifespan expectancy


Maintenance cycles (downtime, repairs) Flat schedule (routine only) **Elevated needs** under high-variance workloads - fans, thermal paste, and other components required maintenance sooner under punishing loads


Economic vs. physical obsolescence Assumed identical timeline **Economic obsolescence ~18–30 months earlier** for some workloads - i.e. GPUs became uneconomical (too slow or power-hungry per $ of output) long before they actually died


**Result:** The fleet's effective depreciation curve **varied by 30–45%** across different end-customers, *even though the GPUs were identical models.* In other words, certain customer workloads drove their hardware to lose value almost **half again faster** than others.


To put it in perspective, one cohort of GPUs that was expected to last ~5.5 years of useful life was trending toward more like **3.7 years** under real use. This nearly **two-year gap** in effective life versus expectation has cascading effects on salvage value, loan terms, covenant triggers, and the overall debt vs. equity mix.


## Key Factors and Events That Impact GPU Lifespan


Different operational events and stressors affect how fast a GPU "ages" or loses reliable performance. **Thermal stress, power stress, and workload intensity** are chief among them:


Stress Factor Description Impact on Lifespan


Sustained High Utilization GPU running near full capacity continuously (24/7 ~98% load) Accelerated wear of components. Industry data indicates that at ~60–70% average utilization, top data-center GPUs may only last 1–3 years.


Thermal Overload Frequent overheating events (85°C–100°C), causing thermal throttling Major lifespan reduction. For every 10°C increase in operating temperature, electronic component life is roughly cut in half.


Power Spikes Sudden surges in power draw or supply fluctuations Power spikes strain voltage regulators and capacitors. High-end GPUs can surge past 500W. Repeated surges cause electromigration and long-term wear.


Thermal Cycling Frequent on-off or load variance cycles Repeated heating/cooling causes materials to expand and contract, stressing solder joints. Bursty workloads accelerate failure vs. steady 24/7 use.


Inadequate Maintenance Dust buildup, aging thermal paste, worn fans GPU fans last ~5 years but fail sooner under constant high RPM. Regular cleaning, fan swaps, and paste renewal (twice yearly) are essential.


Overclocking/Overvolting Running GPU beyond factory specs for extra performance Sharply increases power draw and heat. Even small overvolt accelerates thermal stress. Trades short-term speed for years of reliability.


Sample GPU health and performance metrics generated within Aravolta — including temperature, power draw, memory behavior, and utilization trends. These workload signatures make it clear why identical GPUs can diverge in lifespan depending on how they're operated day-to-day.


## The Pattern


The core question for GPU asset longevity isn't:


"How long does an H100 last?"


It's far more specific:


"How was this GPU used? By who? Doing what? At what utilization and thermal profile over its life?"


In other words, identical hardware **≠** identical useful life. Two GPUs that rolled off the same factory line can have **drastically different** economic lifespans depending on whether they lived a pampered life or a punishment test.


**Telemetry is the new truth.** Granular, real-time data about how each GPU is actually operating is the key to understanding its true depreciation curve. It moves us from debating generalities to quantifying specifics.


## Implications for Lenders


If you are financing GPUs and your underwriting doesn't include telemetry-based, asset-level monitoring, you are likely missing the single biggest risk in the category:


### 1. Losing principal without warning


If a borrower overruns their GPUs with constant 90–100% utilization and high thermals, the hardware can economically die far earlier than your model assumes. A fleet underwritten for 5–6 years may effectively have only 2–3 years of economic runway, leaving you under-collateralized and over-leveraged.


### 2. Overpricing some deals


You'll charge too high a rate on borrowers who manage their GPUs well. Good operators running steady, optimized workloads will have longer-lasting hardware. They'll resent being overcharged and seek cheaper capital elsewhere.


### 3. Underpricing other deals


You'll take on extra risk where GPUs are being run to death. Without telemetry, that risk hides in plain sight until it causes surprise write-downs. You end up under-collateralized and under-compensated.


### 4. Leaving money on the table with deal structure


Granular data enables creative financing structures like sale-leasebacks with performance triggers, or usage-based financing where payments flex based on actual GPU-hour utilization. Without data, you're forced into conservative, inflexible terms.


## Where Things Go From Here


The next generation of compute financing is being built *right now* . Lenders and operators no longer need to fly blind or rely on industry folklore for GPU longevity. We now have the tools to **monitor GPU fleets at the hardware-signal level** , in real time, and produce **real depreciation curves** tailored to each workload and environment.


We're working with forward-looking financiers and mid-market operators (in the **1–100MW** segment) to support innovative structures like sale-leasebacks, usage-based leases, and revolving GPU credit lines – all anchored by actual performance data. It's lending based on **actual risk, not averages** .
