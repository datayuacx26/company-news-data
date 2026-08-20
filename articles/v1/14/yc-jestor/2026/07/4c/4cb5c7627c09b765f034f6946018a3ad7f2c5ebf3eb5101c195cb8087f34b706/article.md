---
schema_version: "1.0.0"
document_id: "4cb5c7627c09b765f034f6946018a3ad7f2c5ebf3eb5101c195cb8087f34b706"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/time-stuck-kanban-phase-jestor/"
published_at: "2026-07-22T15:51:58+00:00"
first_seen_at: "2026-07-22T15:56:08.729685+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:f55ab2f0ff7a8b88c12e49563d6ad9bfd93ea273e8d2df3fd4a66beb9b0f12f2"
---

# How Do You Measure the Time a Card Stays Stuck in Each Kanban Phase Using a Single-Select Field?

In **Jestor** , creating a single-select field (like status) and turning on log metrics makes the system automatically track how long each card stays stuck in each phase, and how long it takes to move between stages.


### Why measuring this time matters


Without this metric, it's hard to pinpoint which stage of the process is causing bottlenecks. Measuring the time stuck per phase helps point exactly to where the flow is getting stuck, instead of relying on the team's subjective impression.


### What log metrics show


- How long a card stayed stuck in each specific phase
- How long it takes to move between one stage and another
- Aggregated data that helps identify recurring bottlenecks
- A basis for deciding where to invest improvement effort in the process


### How to turn on log metrics in Jestor


1. Create or edit a single-select field, like "status"
2. In the field's settings, find the log metrics option
3. Turn it on to start tracking time per phase
4. Check the data later on the Kanban or in linked reports


### Process automation with real bottleneck data


Measuring time stuck per phase turns **process automation** into something data-driven, letting you prioritize improvements in Jestor based on where the process actually gets stuck, instead of just guessing.


### Table Summary


Log metric What it shows


**Time stuck in phase** How long the card stayed in that stage


**Move time** Time between one stage and the next


### Video Tutorial: Step by Step


*Video: Ep 12: Refining the Registry With Categories — video tutorial showing this feature in practice, right inside the Jestor interface.*


## Frequently Asked Questions


**Are log metrics turned on by default?** No, you need to turn them on manually in the single-select field's settings.


**Does this feature work on any Kanban?** Yes, as long as the status field is a single-select type with logging turned on, on[jestor.com](https://jestor.com/?ref=blog.jestor.com) .


**Does the measured time include weekends and holidays?** The metric tracks elapsed time between phase changes, following the field's standard behavior.


## Get to Know Jestor


With Jestor, you can automate workflows, connect different areas, and build internal systems your way — all without code and with AI support. Check out Jestor at[jestor.com](https://jestor.com/?ref=blog.jestor.com) and discover how to take your company's management to a new level of efficiency and integration.
