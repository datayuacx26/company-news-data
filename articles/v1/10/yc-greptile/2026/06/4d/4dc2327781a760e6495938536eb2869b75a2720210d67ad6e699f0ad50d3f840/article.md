---
schema_version: "1.0.0"
document_id: "4dc2327781a760e6495938536eb2869b75a2720210d67ad6e699f0ad50d3f840"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/trex"
published_at: "2026-06-15T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:229350946729dc3b774ba766b79ec9991c9d2443665a38dbc8d1e9739e0ddc09"
---

# Introducing TREX: Greptile Now Runs Your Code

Today we're launching TREX, Greptile's execution layer for code review. Until now, Greptile reviewed your code by reading it. TREX adds a new step: it runs the code.


Static review has a ceiling. It can reason about what the code says, not about what it does. Some bugs never show up in the diff. They only appear when the code runs: an endpoint that 500s on a real request, a page that renders broken. You can read the change perfectly and still miss them.


TREX (short for Test, Run, Execute) closes that gap. In our evals, Greptile review with TREX catches 20% more bugs.


**[Enable on your org.](https://app.greptile.com/#trex)**


## How it works


When Greptile reviews a PR, it picks out the behavior worth running and spins up a TREX agent. The agent spins up a sandbox, runs the relevant code, and reports back on issues.


Because TREX shares the review context, it already knows what the change does and where to look, so there's no setup or scaffolding to wire up.


## Artifacts


A pass or fail result is not enough on its own. When TREX runs your code, it attaches the evidence to the comment: logs, screenshots, API traces, execution scripts, or even a video of a UI change playing out.


The point is to show what happened, so you or a downstream agent can confirm it instead of taking the result on faith.


## End to end validation


With TREX, we're creating an end-to-end validation layer for every PR, and a step towards our goal of software with no bugs.
