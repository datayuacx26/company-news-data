---
schema_version: "1.0.0"
document_id: "6df18e6c9ad773feb8b5a1ba2e10e3c6d9c23b5e32f98ac6287b17ecfa0bbe60"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/battery-lab-ai-agent"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T10:48:53.744019+00:00"
fetched_at: "2026-07-30T10:48:55.452344+00:00"
content_hash: "sha256:0943e8de163f21d7ef4b5791e6aee7099c39668bd495a416b18262e9f4a623f4"
---

# An AI agent that runs your battery test lab

## Ask it what is happening


The simplest use is status. Instead of opening four vendor UIs and a spreadsheet, ask:


- "What's running on channel 17?"
- "When will the test on channel 84 finish?"
- "What's free on the Maccor, and for how long?"


The agent reads the same occupancy and queue as the channel wall, so its answers match what the lab is actually doing, down to the estimated finish time, which comes from simulation. See[predict how long a test will take](https://ionworks.com/blog/predict-battery-test-duration) for how that number is computed.


## Generate protocols from natural language


Writing a cycler protocol by hand is slow, and small mistakes cost weeks of channel time. The agent takes a description in plain language and produces a validated protocol in Ionworks' universal format, which exports to Arbin, Maccor, Neware, and BioLogic. "HPPC at 1C and 2C, 10-second pulses, 5% SOC steps, on a 5 Ah NMC cell" becomes a runnable schedule in seconds. This builds on the protocol generator we wrote about in[generate cycler protocols from natural language](https://ionworks.com/blog/generate-cycler-protocols-from-natural-language) ; the agent now runs it as one step inside a larger workflow.


## Simulate, then schedule


A protocol is only useful if it runs and finishes when you need it to. Before booking, the agent simulates each protocol against the cell's model to predict its run time, then places it in the next-available window on a free channel, accounting for the time left on tests already running. A campaign becomes a set of booked channels with realistic start and end times.


## Why grounding matters


Because the agent operates on structured measurements and physics models rather than free text, its plans are checkable. You see the drafted protocols, the predicted durations, and the channel assignments before anything runs, and you approve. The agent removes the busywork and leaves the engineering judgment to you.


## Where this runs


The agent is part of[Ionworks Operate](https://ionworks.com/operate) , the scheduling and status layer for multi-brand cycler fleets. It works on the same data path as[Measure](https://ionworks.com/solutions/measure) and the[channel wall](https://ionworks.com/blog/whats-running-on-your-cyclers) , so the numbers it schedules with are the numbers you model with.


Try it free on a live synthetic lab.[Request access](https://ionworks.com/operate) with a work email, ask the agent to build and schedule a campaign, and watch the channels fill in real time.
