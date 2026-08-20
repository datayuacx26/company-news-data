---
schema_version: "1.0.0"
document_id: "54c674bee7e22c9d162735bd3b2d79a4f6080285030ad23fdcd455dcc46a5294"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/smart-prototypes-summit-2026"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:409fe4821948ef2330faaedcf1f69f18c9739eb346d968102a482f8ddb953f05"
---

# What test and simulation engineers told us at the Smart Prototypes Summit | MOVE. Blog

We presented at the VI-grade Smart Prototypes Summit this year. If you would like to see the presentation, there is a note at the end of this post. The most useful part for us was the conversations afterwards with the test and simulation engineers who do this work every day.


## The work that hides around the run


Engineers described the same pattern in different words. A run finishes, whether on a proving ground, a rig, a simulator, or across hundreds of simulation cases. The results land in different systems and formats, and someone spends the day exporting, aligning, and cross-checking before the actual engineering question can even be asked.


Simulation teams described the same pain from the other side. Before a model can be trusted, someone has to pull the right measured maneuvers, rerun comparable cases, overlay traces, calculate KPIs, and explain where model and reality diverge. The loop is essential, but manual enough that many teams cannot repeat it as often as they would like.


That is where AI agents can keep the loop moving: retrieving data, running VI-Car RealTime simulations, comparing metrics, flagging discrepancies, and carrying context into the next design iteration.


## Simulations only matter when they connect back


One of the strongest conversations after the presentation was about the gap between running simulations and making them useful in a real program. A team may run thousands of cases or train a surrogate model, but the value comes from connecting those results back to targets, measured data, driver feedback, cost constraints, and the next decision.


That was the point of the SUV example we showed on stage. The agent did not just run more simulations. It mapped the parameters that mattered, built and validated a surrogate model, explored handling, ride, and cost tradeoffs, and connected the options back to driver-in-the-loop feedback.


That is the gap MOVEcenter is built to close: removing the routing and reformatting between a finished run and a useful answer.


## Thanks for the conversations


If we talked at the summit and you want to pick the thread back up, or if you would like to see the presentation from the summit, email us atfounders@movedot.ai and we will send it over.
