---
schema_version: "1.0.0"
document_id: "8ef00df1c2b8c79f43ad61578da9bbdd56e1a726006955987d6e0c3b017def8e"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/sequential-testing-ab-test-photoroom"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:238a08d6fcde1cd6e86e5ffe8df0c127a43eb9dd46526e16b40356c8586a408a"
---

# Why sequential testing is the right way to experiment at the speed of PLG apps like Photoroom

## 1. The confusion


**At Photoroom, experimentation is part of how we build.**


We run dozens of A/B tests in parallel to constantly iterate on our product and deliver the best-in-class creative experience powered by AI.


As our data team started to scale (we’re now three people!), one of our core missions became fostering a truly data-driven culture across the company. That meant defining clear guidelines for all data-related topics, including experimentation.


I had always used Amplitude for analytics, but the growth team owned experimentation, and to be honest, it hadn’t been my favorite topic. Until suddenly, it was part of my job — and that’s when the confusion began.


Amplitude offers **two A/B testing methodologies** .


The team had been using what we called the “classic” approach: creating feature flags, filtering exposed populations in funnel charts, and manually checking statistical significance.


But in Amplitude’s “Experiments” section, there was another option — experiments using a built-in methodology that looked… different.


classic AB testing methodology


What was confusing was that there were “experiments” in the “experiments” section along with “feature flags”. And the results for the same experiments didn’t match.


We started asking around:


-


“What’s the actual difference between these two?”


-


“Which one should we trust?”


-


“Why does Amplitude even show both?”


At Photoroom, that kind of uncertainty doesn’t fly.


We base many product decisions—ML models, onboarding tweaks, pricing tests—on A/B testing outcomes. To share best practices internally, we needed to master the methodology ourselves and be 100% confident in our recommendations.


So we dug in. We read Amplitude’s documentation line by line, asked dozens of questions to their experimentation team, organized meetings with their experts, reached out to similar data teams (thanks Daniel from the Voodoo team), and ran small replications of our own. Eventually, we understood what was going on—and, more importantly, which method actually fit the way we build products.


## 2. T-Test vs Sequential Testing


It turned out that our “classic funnel” approach was relying on the **t-test** methodology.


In short, a t-test:


-


Compares the average result in both groups (control vs. treatment).


-


Checks how spread out the data is (variance).


-


Calculates a number called the **t-statistic** → it tells you how big the difference is, relative to the noise.


-


From that, it gives you a **p-value** → the probability that a difference as big as the one we observed would appear just by chance, if control and treatment were truly the same.


How to intepret significance in a T-Test


It’s straightforward—but t-test assumes you will:


-


decide your sample size in advance, and


-


look at the data **only once** , at the end.


If you look at results midway—something everyone does in a fast-paced environment—you dramatically increase the risk of **false positives** . And when you’re running dozens of A/B tests at a time to guide product decisions, that’s exactly what you want to avoid.


> If you check repeatedly and stop as soon as p < 0.05, each look with T-test is another lottery ticket for a rare event. The more tickets you buy, the higher the chance that at least one will win by luck.


That’s where **sequential testing** comes in.


The beauty of sequential testing is that it **doesn’t replace the t-test; it extends it.**


Both rely on the same core idea: comparing two groups (control vs treatment) to decide if the difference you observe is bigger than what random noise could explain. What changes is **when and how often** you’re allowed to look at the data **.** It allows **peeking:** it means checking experiment results before the data collection is complete, and potentially making a decision based on that partial data; because sequential testing updates probabilities continuously and doesn’t inflate false positives.


> **Sequential testing is designed for continuous evaluation.**


In practice:


-


You can check progress anytime— **no penalty for peeking** .


-


You don’t need to pre-define a sample size.


-


The test can stop early if results are decisive.


-


Significance thresholds adjust dynamically to maintain accuracy.


> Peeking: checking experiment results before the data collection is complete


In a **product-led growth (PLG)** company like Photoroom, this is crucial.


Our releases are continuous, traffic fluctuates daily, and decisions often need to be made mid-run. Product managers check dashboards daily; leadership might ask for insights before an experiment technically “ends.” Even if we set out to run a four-week test, we’ll inevitably look earlier—and the moment we do, we violate the t-test’s assumptions.


That’s why so many teams see contradictory p-values or “significant” results that later disappear. It’s not a platform bug, it’s a **methodology mismatch** between how we work and how the math expects us to behave.


Sequential testing fixes exactly that.


It’s built for teams that look at data continuously: teams that are curious, iterative, and agile.


> We eventually realized something simple but powerful: None of our experiments actually had a clear end date. That’s why sequential testing made perfect sense.


## **3. Why we standardized on Sequential Testing at Photoroom**


Once we fully understood sequential testing, we made it our standard for every product experiment at Photoroom.


It fits perfectly with how we operate:


-


We ship fast, monitor continuously, and make decisions iteratively.


-


Product managers can safely peek at results without analysts worrying about inflated false positives.


-


Analysts can focus on defining the right success metrics and guardrails rather than policing when someone opens a dashboard.


-


Our decision-making became both faster *and* more rigorous.


We now follow a clear framework for every test:


1.


**One exposure event:** to clearly define who’s actually part of the experiment.


2.


**One success metric:** to measure the primary outcome.


3.


**Guardrail metrics:** to ensure we don’t harm key parts of the product.


4.


**Sequential testing:** as the standard methodology that allows us to monitor continuously and decide confidently.


This shared approach gave us a **common language** across teams—a way to interpret results that everyone could trust, from analysts to PMs to leadership.


Sequential testing isn’t just a statistical choice; it’s a mindset shift. It reflects how PLG teams work today: iterative, data-driven, and curious. We no longer treat experiments as isolated studies but as **living processes** that evolve with our product.


> Data excellence isn’t about running more experiments - it’s about understanding how those experiments are measured. For PLG teams like Photoroom, sequential testing is the only approach that truly matches the pace of our product.


---


This marks the start of our *Experiments series* — where the PhotoRoom data team shares everything we’ve learned about Amplitude experiments. More to come soon!


**If you’re interested in joining Photoroom, take a look at our[open positions](https://jobs.ashbyhq.com/photoroom) .**
