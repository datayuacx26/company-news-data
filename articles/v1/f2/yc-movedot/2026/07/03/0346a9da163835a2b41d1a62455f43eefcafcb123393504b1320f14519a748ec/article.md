---
schema_version: "1.0.0"
document_id: "0346a9da163835a2b41d1a62455f43eefcafcb123393504b1320f14519a748ec"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/agents-for-canopy-vi-carrealtime-and-correlation"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:b0f1fc4ae9fa55c544557cb981983e8de988aff7a38ec4833d8e47abe0263a37"
---

# The sim-to-test correlation agent: model validation that keeps up with your program | MOVE. Blog

Model validation is the work that keeps a vehicle model honest against test data. It is also the work teams skip most, because it runs on human time. The sim-to-test correlation agent is built to change that: it runs the validation loop for you, so a model can be checked against reality as often as it should be, not just once early in a program.


## The sim-to-test correlation agent


The correlation agent connects simulation to real measurement. In our[closing-the-loop project](https://www.movedot.ai/blog/closing-the-loop-between-simulation-driver-in-the-loop-and-real-measurements) we called this auto-validation: the agent pulls data from previous vehicles, finds and crops the relevant maneuvers, prepares the simulation files, reruns the maneuvers, overlays the traces, compares the channels the analysis cares about, and flags where simulation and measurement disagree. It even proposes the model changes that would reduce the error.


The point is to make model validation something that happens as often as it should, instead of something that gets skipped because it runs on human time. The expert defines the methodology. The agent runs it, at a scale no person has time for.


Because the loop is now cheap to run, validation stops being a milestone you hit once and becomes a check you can repeat whenever the model or the data changes. That is the difference between a model you trusted at kickoff and a model you can trust today.


## Two more agents in the same workflow


The correlation agent does not work alone. Two more MOVEdot agents handle the simulation work that feeds it, running on the same platform and the same data layer.


### The Canopy agent


The Canopy agent runs a full correlation campaign for you on[Canopy Simulations](https://www.movedot.ai/blog/agentic-correlation-a-vehicle-model-you-can-trust-built-in-hours) : it isolates one effect at a time, checks each parameter against logged test data, calibrates the model, and documents every change as it goes, with the engineer supervising. It works well on Canopy because Canopy's API lets an agent submit jobs, retrieve results, and update configs directly, while the platform launches many simulations in parallel and keeps every result tracked.


### The VI-CarRealTime agent


The VI-CarRealTime agent sets up and runs[VI-CarRealTime](https://www.movedot.ai/blog/integrations) simulations directly from inside the loop the agent is already working in, instead of a separate manual step you context-switch into and out of. Describe the run you want, and the agent prepares the simulation, executes it, and brings the results back for analysis.


## Try it on your own work


If sim-to-test validation is where your team loses time today, we would like to see how you do it now and set the correlation agent up with you. The Canopy and VI-CarRealTime agents are available to run alongside it. Get in touch:founders@movedot.ai
