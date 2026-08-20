---
schema_version: "1.0.0"
document_id: "75ab2d4f2756891d8187dbcfc9e37776b257df46c80a225983b1f18e50dd32b7"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/synthetic-battery-lab-free-trial"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T18:31:36.719222+00:00"
fetched_at: "2026-07-31T18:31:38.205519+00:00"
content_hash: "sha256:908214ed855fdc412d20376bd247d9184f42cbf21d0947e20f27b2b74a4e2e2b"
---

# A synthetic battery lab you can drive, free

The hard part of trying lab software is that you need a lab. A static sandbox tells you nothing: a scheduler with nothing to schedule looks the same whether it is smart or not, and an agent that cannot book a real channel is just a chat box. So we built a lab you can actually drive, and made it free.


Sign up with a work email and you land in your own org with a fleet of cyclers and channels already provisioned. The occupancy wall is live from the first second, the scheduler has real channels to book, and the agent has a real lab to act on.


## What it looks like


Schedule a test, or ask the agent to build a campaign. The moment it is booked, the channel flips to **occupied** and a voltage and current trace begins filling in, point by point, as the cell cycles. An estimated finish time appears. When the test completes, the channel goes **free** and the next one can start.


It is simulated in real time. A test that would take four days on a real cycler takes four days here, and you watch it progress live on the wall, the same way you would watch your own rack. Leave it running and come back tomorrow to more data on the channel.


## The cells are real models


Behind each channel is a real battery model: a 5 Ah NMC/graphite cell with throughput-based degradation. Run a long cycle-life test and you will see capacity fade and internal resistance climb over the cycles, with cell-to-cell variation across the fleet, because the traces come from physics, not a canned recording. The numbers you see are the numbers Ionworks would model with.


## You can drive the whole product


Everything works against this live lab, so you can test the claims yourself:


- Walk the[channel wall](https://ionworks.com/blog/whats-running-on-your-cyclers) across a mixed fleet and watch occupancy change.
- Book a test and see the scheduler[predict its run time](https://ionworks.com/blog/predict-battery-test-duration) and place it in a realistic start window.
- Ask the[agent](https://ionworks.com/blog/battery-lab-ai-agent) to write a degradation campaign in plain language and schedule it across free channels, then watch the channels fill.


Nothing is mocked. The wall, the scheduler, and the agent are the same ones a customer runs on their own cyclers.


## Try it


[Request access](https://ionworks.com/operate) to[Ionworks Operate](https://ionworks.com/operate) with a work email. We drop you into a seeded org, you schedule a few tests or hand the agent a campaign, and you watch the lab run. It is the fastest way to see whether the scheduling and the agent do what we say, without tying up a single real channel.
