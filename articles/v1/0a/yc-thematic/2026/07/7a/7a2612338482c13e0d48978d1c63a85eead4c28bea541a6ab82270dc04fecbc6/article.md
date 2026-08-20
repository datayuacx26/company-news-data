---
schema_version: "1.0.0"
document_id: "7a2612338482c13e0d48978d1c63a85eead4c28bea541a6ab82270dc04fecbc6"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/how-fast-ai-analyze-million-feedback"
published_at: null
first_seen_at: "2026-07-30T14:00:26.580496+00:00"
fetched_at: "2026-07-30T14:00:27.349370+00:00"
content_hash: "sha256:7e565a4e6103577ddc4a92006b46501798da70ab802114e367139c4d3d94256c"
---

# How Fast Can AI Analyze a Million Pieces of Customer Feedback?

You have a million comments sitting in survey exports, support tickets, app reviews, and call transcripts. Someone senior wants to know whether analyzing all of it is an afternoon's work or a project that needs a budget line. Most vendor answers say "thousands of reviews in minutes," which is two orders of magnitude short of the question you actually asked.


Hours, not weeks, but only if you're precise about which speed you mean. Thematic's own[published benchmark](https://getthematic.com/insights/how-thematic-finds-insights-large-datasets) on a large public dataset is 130,000 reviews across 35 banking apps set up, themed, and analyzed in three hours, and that window includes a human refining the themes rather than just a machine classifying text. Atlassian included over 1 million community questions and comments in its feedback analysis. Compute is almost never the thing that makes a million comments slow.


Below is what determines the real number, how the machine time compares to the manual alternative, and why the speed that matters most after go-live isn't throughput at all.


## "Fast" is three different questions


Buyers ask one question about speed and get one answer, when there are really three, each with a different engineering budget.


- **Bulk throughput.** How long the first pass over your full history takes. This is a batch job measured in comments per hour.
- **Incremental latency.** How long a new batch takes once the model is established. Usually minutes, and the number that governs your reporting cadence.
- **Query response time.** How long you wait for an answer after you click. Measured in seconds, and the one you feel every day.


These aren't interchangeable. The industry's own benchmark suite formalizes the split:[MLPerf Inference](https://www.hpcwire.com/2025/09/10/mlperf-inference-v5-1-results-land-with-new-benchmarks-and-record-participation/) measures an Offline scenario in throughput and a separate single-stream scenario in 90th-percentile latency. In the September 2025 results, the same NVIDIA GB300 hardware delivered 5,842 tokens per second per GPU offline but 2,907 in the latency-bound server scenario. Dropping the interactive constraint roughly doubled throughput. Any vendor quoting one number for "speed" is collapsing three budgets into one.


## What the arithmetic says, and why the naive version is wrong


Start with a floor rather than a marketing claim.[Sentence-BERT](https://aclanthology.org/D19-1410.pdf) , published at EMNLP in 2019, computed 2,042 sentences per second on a single Nvidia Tesla V100. Divide a million by that and you get roughly eight minutes for the embedding step, on seven-year-old hardware.


That number is real and also misleading, which is why you should distrust anyone who quotes the multiplication and stops there. Raw model speed is not system speed. One[published analysis of production inference servers](https://arxiv.org/pdf/2403.12981) found the end-to-end system delivered just 19.5% of the throughput achievable with model inference alone, because preprocessing became the limiting factor as inputs grew.[Databricks](https://docs.databricks.com/en/machine-learning/model-inference/model-inference-performance.html) gives the same guidance from the operations side: if GPU utilization isn't continuously high, the data input pipeline is the bottleneck.


Machine classification of a million short texts is a solved, inexpensive problem. What takes time is everything around the model.


## What actually determines how long a million comments takes


**Whether it's a first run or an incremental one.** The first pass over your history is the expensive one, because that's when the theme structure gets built and reviewed. Everything after that is marginal. Benchmarking a vendor on first-run time alone measures the one number you'll experience once.


**How many sources you're unifying.** A million comments from one survey is a different job from a million spread across surveys, tickets, reviews, and transcripts. Each source arrives with its own schema and metadata, and reconciling them is preprocessing work, which the research above identifies as the usual bottleneck.


**Comment length.** A million two-word app-store ratings and a million 300-word call transcripts differ by more than an order of magnitude in tokens. Any rate quoted in "comments per hour" without a length assumption is close to meaningless.


**Whether human review sits inside the measured window.** This is what makes vendor numbers incomparable. Thematic's published 130,000-in-three-hours figure includes setting up the dataset and refining the themes. A pure machine-classification number would be far faster and far less useful, because nobody ships an[unreviewed theme model](https://getthematic.com/insights/ai-feedback-analytics-accuracy) into an executive report.


**How mature the theme model already is.** An established taxonomy classifies new feedback immediately. A[cold start](https://getthematic.com/insights/responses-needed-ai-themes-reliable) needs discovery first.


## How long the manual alternative takes


The comparison that matters isn't against another vendor's benchmark. It's against what your team does today.


[Peer-reviewed research](https://www.sciencedirect.com/science/article/pii/S0957417422022837) on classifying open-ended survey responses puts the manual baseline plainly: doing it by hand is "not only time consuming, potentially taking weeks or even months, depending on sample size, but also risks introducing human errors or inconsistencies." Speed and consistency are the same argument, not two.


Published customer stories match that. Atlassian's research team used to spend six weeks in a Miro whiteboard bucketing and categorizing feedback, producing a report quarterly, while[an estimated 60,000 pieces of feedback](https://getthematic.com/insights/building-an-infinite-feedback-loop-with-atlassian) were arriving every month. Their lead analyst for feedback noted that this covered less than 20% of the qualitative data the company received. That's the real cost of manual analysis at volume: not slowness, but abandonment of most of the data.


At[Greyhound](https://getthematic.com/insights/how-to-reducee-analytics-time-tenfold) , one analyst previously handled the post-trip survey backlog of 100 to 150 surveys a day. Analysis took between three hours and three days per cycle and consumed about 80% of that job, and by the time results were distributed the data was already three to four weeks old.


## Why response time matters more than throughput once you're live


Throughput is a procurement question you answer once. Query latency is a question you answer every day, and the human-factors research on it is unambiguous.


The[canonical thresholds](https://www.nngroup.com/articles/response-times-3-important-limits/) , which Jakob Nielsen credits to Robert Miller's[1968 paper](https://dl.acm.org/doi/10.1145/1476589.1476628) on man-computer conversational transactions, are 0.1 seconds for a system to feel instantaneous, 1.0 second to keep a user's flow of thought uninterrupted, and 10 seconds to hold their attention at all. A two-second response sits inside the attention band and needs no progress indicator, but it does not preserve uninterrupted flow of thought. That threshold is one second.


The gap between those two bands is not cosmetic. A[peer-reviewed study of exploratory visual analysis](https://idl.cs.washington.edu/files/2014-Latency-InfoVis.pdf) found that an additional delay of just 500 milliseconds "incurs significant costs, decreasing user activity and data set coverage," and specifically reduced the rate at which analysts made observations, drew generalizations, and generated hypotheses. The same study found something worse: analysts first exposed to a slow tool went on to perform worse even after being moved to a fast one. A sluggish analytics tool doesn't just waste time. It trains your team to ask fewer questions, and the habit outlasts the latency.


## What this looks like in practice


[Community Health System](https://getthematic.com/insights/community-health-systems-actionable-insights) , a not-for-profit healthcare network serving California's central San Joaquin Valley, runs an annual employee engagement survey collecting open-ended feedback from staff across 250 departments. Preparing departmental reports used to take roughly an hour each, which meant weeks of rolling work and over 250 hours of staff time per cycle. The team now produces all 250 one-page reports in a single three-day sprint, with preparation down to about 20 minutes per department. That's 160-plus hours saved per cycle and more than $10,000 in staff cost. Alexandra Clifton, Organizational Development Partner, framed the gain as scheduling rather than raw speed: "We were able to reach 250 departments. Yes, it was a lot of work, but it was all front-loaded to a single three days and everyone was able to get their results level-set at the same time."


Volume at that scale is routine rather than exceptional.[LendingTree's insights team](https://getthematic.com/insights/lendingtree-thematic-case-study) worked through well over 20,000 comments in a 90-day period, drawn from 10 continuous data sources spanning seven product verticals. A major supermarket retailer with revenue in the low billions cut time-to-insight from seven days to five hours, under 9% of its previous analysis time, while unifying four feedback sources that had each been analyzed separately before.


## The availability question nobody asks


Throughput is worthless if the system isn't up, and this is the part of the speed conversation that almost never appears in vendor content.


Availability is arithmetic, so translate any percentage before you accept it. Each additional nine is a tenfold reduction in permitted downtime.


Availability Permitted downtime per month Permitted downtime per year


99% About 7 hours About 87 hours


99.9% 43 to 44 minutes 8 hours 46 minutes


99.95% About 22 minutes About 4.4 hours


99.99% About 4 minutes About 53 minutes


Also check how the number is defined and what it covers.[Google's Cloud SLA](https://cloud.google.com/compute/sla) defines Monthly Uptime Percentage as the total minutes in a month minus downtime minutes, divided by total minutes in the month, and[Google Workspace](https://workspace.google.com/terms/sla/) commits to at least 99.9% in any calendar month. That's the mainstream commercial bar. Ask any feedback analytics vendor for their figure in writing, and ask whether it applies to the dashboard, the query API, or the ingest pipeline, because a scheduled overnight load and an interactive query have different failure consequences.


## A buyer's checklist for speed at scale


1. What's the first-run time for our actual volume, and does that window include human theme review?
2. What's the incremental time for each new batch once the model is established?
3. What's the typical query response time, and is it under one second or under ten?
4. What comment length is any quoted throughput rate based on?
5. How many separate sources can be unified in that same window?
6. What availability figure will you commit to in writing, and does it cover the dashboard, the API, or the pipeline?
7. How is downtime defined, measured, and reported?
8. Can we run a timed proof of concept on our own history rather than a demo dataset?


## The short answer


At a million comments, machine classification is not the constraint. Preprocessing, source reconciliation, and human theme review are, which is why credible figures come with a stated scope rather than a single number. Thematic's published anchor is 130,000 reviews set up, themed, and analyzed in three hours, and the first run is the slow one; incremental batches land in minutes. Ask any vendor to separate bulk throughput from incremental latency from query response time. A vendor that quotes one number for all three hasn't measured any of them.
