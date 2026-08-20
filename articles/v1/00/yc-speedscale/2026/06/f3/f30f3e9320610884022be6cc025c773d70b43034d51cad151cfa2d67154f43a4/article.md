---
schema_version: "1.0.0"
document_id: "f30f3e9320610884022be6cc025c773d70b43034d51cad151cfa2d67154f43a4"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/ai-rework-tax-which-bugs/"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:e400bf24a99551f5a06d833ba66f6b4fa01e3fd90c24bba16418913033578fb8"
---

# Which Bugs AI Agents Fix Better With Traffic

In[the first experiment](https://speedscale.com/blog/ai-rework-tax/) , I wanted a baseline: if an AI coding agent gets the same production signal a human would get, can it fix bugs in a codebase it has never seen?


Yes, but only when I gave it better context. With only an alert, the agent passed 51% of the runtime tests. When I added captured traffic, the actual request and response for the failing call, it climbed to 77%.


This post is the second pass. I wanted to know which bug types actually improve with traffic, whether a service map helps the agent choose the right microservice, and where this approach still falls over.


The short version: traffic helps most when the truth is on the wire. Service maps help a little. They do not replace payloads.


🎯 Key Takeaways


- Second run of the benchmark: alert only passed 55%, alert plus captured traffic passed 77%. The lift is uneven across bug types.
- Bugs whose truth is on the wire climb to around 90%. Race and write-path bugs went from 22% to 89%, state-machine transitions from 72% to 92%.
- Deep framework internals barely moved, 75% to 79%, because the fix hides in a buffer or state machine no request touches.
- A service map alone bought 6 points. Traffic bought 28, and still added 20+ on top of the map. It’s the payloads, not the topology.
- The whole run was about 18,000 model calls for roughly $118, with 94% of input tokens cached. Cost isn’t the reason to skip it.


## Start with the alert-only baseline


The intuitive wiring looks like this. A bug fires in production, your observability stack catches it (a Datadog-style alert with error rate, latency, the affected endpoint), and you hand that to the agent, point it at the repo, and let it work.


In this second run, I ran exactly that on 100 hand-authored bugs in a private 240-service codebase the model had never seen. A cheap model (gpt-5.4-mini) driving a real agent loop (opencode), ten runs per bug. It passed 55% of the time.


Read that as a factory and it’s rough. Just under half the time the agent ships a patch that doesn’t fix the bug, or it edits the wrong service entirely and reports success anyway. (That second one happened on a third of the alert-only runs. The agent is confidently wrong, which is the expensive kind.) A 55% first-pass rate is a coin flip with a code review bolted on.


## Traffic is the useful context


Then I changed one input. Alongside the same alert, I gave the agent the captured request and response for the failing call: what the client actually sent and what the server returned, down to the field where they diverged. Same model, same agent, everything else held.


The pass rate went to 77% on average. But the average is the boring part. Broken down by what kind of bug it was, the traffic wasn’t helping evenly. It was carrying some classes most of the way to done and doing almost nothing for others.


## The lift depends on the bug


First, what kinds of bugs even exist, so I’m not grading on a curve I drew myself.[Ray, Devanbu, and Filkov](https://dl.acm.org/doi/10.1145/2635868.2635922) went through 729 GitHub projects and sorted real defects into buckets like concurrency, memory, plain logic, and API or contract errors. A[2026 study of the TypeScript ecosystem](https://arxiv.org/abs/2601.21186) tagged 633 real bug reports and found failures clustered around tooling and configuration, API misuse, async and event handling, and type errors. My 100 bugs lean toward the part of that world that crosses the network, because that’s where I expected the traffic to earn its keep. Worth saying out loud: I stacked the deck toward concurrency and protocol bugs on purpose. A real backlog has a pile of config and UI bugs that none of this helps with.


Here’s the same 100 bugs, alert only versus alert plus traffic, by type:


Bug type Alert only + traffic


State-machine transitions 72% 92%


Race / write-path 22% 89%


Streaming / multipart framing 52% 85%


Cross-service contract drift 44% 81%


URL / path encoding 59% 78%


Schema mismatch (type ≠ wire) 49% 69%


Missing field or header 52% 69%


Deep framework internals 75% 79%


The bugs that live on the wire climb to around 90%. The one that doesn’t (deep framework internals, where the fix hides in some buffer or state machine no request ever touches) sits flat at +4 points. That’s the rule in one table: traffic is the tiebreaker when the bug’s truth is in the request or response, and dead weight when it isn’t.


## Race conditions changed the most


Race conditions went from 22% to 89%. These are bugs where two requests interleave and corrupt each other, and from an alert the agent is just guessing at ordering. The captured traffic has both requests in it with their timing, so the agent sees the actual interleave instead of inventing one. (Caveat I owe you: that row is three bugs, so the exact figure is soft. It’s the cleanest win in the set and also the smallest sample, and I’m deepening it before I lean on it.)


## Service maps helped less than expected


A sharp reader of[part 1](https://speedscale.com/blog/ai-rework-tax/) pushed on this. Maybe the traffic is only helping the agent find the right service in a 240-service haystack, and a plain service map (who calls whom) would do that job for free.


Good theory, so I tested it four ways: alert alone, alert plus a service map, alert plus traffic, alert plus both. The map alone bought 6 points. The traffic bought 28. And once the agent already had the map, the traffic still added more than 20 on top of it.


So it’s the payloads, not the topology. The reason, I think: the traffic helps the agent find the right file, but it does it through content. The failing request literally contains the broken field name, so the agent greps for that string and lands on the file in seconds. A box-and-arrow diagram doesn’t carry that string.


```text
flowchart TB
subgraph Map["Service map alone"]
M1["Who calls whom"] --> M2["No payload, no field names"] --> M3["Agent still hunting"]
end
subgraph Traffic["Captured traffic"]
T1["Failing request and response"] --> T2["Carries the broken field name"] --> T3["Agent greps it, finds the file"]
end


```


## The cost was not the hard part


The whole thing, the breakdown plus the four-way test, ran about 18,000 model calls for roughly $118. 94% of the input tokens were cached, since the codebase repeats across every run, so bolting a few captures onto each prompt is close to free. Whatever you make of the success rates, cost isn’t the reason to skip it.


## The next step is reproduction


Here’s the honest limit. Everything above is one-shot. Hand the agent the context, let it take its swing, score the result. 90% on the good classes is real, and one-shot with traffic beats one-shot with an alert by a mile. But a factory can’t run on a single swing and a prayer. You want the agent to prove it fixed the bug before it opens a PR.


That’s the missing machine, and it’s what part 3 builds. Instead of one-shotting, you reproduce: stand the service up, replay the captured traffic against it, and let the agent watch its own fix pass or fail against real behavior. The capture stops being context the agent reads and becomes a test it runs. That’s the loop behind[AI code verification](https://speedscale.com/features/ai-code-verification/) in CI and[proxymock](https://speedscale.com/proxymock/) on a developer machine.


Part 3: the reference architecture for an agent factory that reproduces bugs instead of guessing at them.


## Common questions


### Which bug types do AI agents fix better with captured traffic?


The ones whose truth is in the request or response. In my benchmark, race and write-path bugs went from 22% to 89% with traffic, state-machine transitions from 72% to 92%, streaming and multipart framing from 52% to 85%, and cross-service contract drift from 44% to 81%. Bugs that live on the wire climb to around 90%.


### Do service maps help an AI agent pick the right microservice?


A little, but far less than payloads. Testing four ways (alert alone, alert plus service map, alert plus traffic, alert plus both), the map alone added 6 percentage points while traffic added 28, and traffic still added more than 20 on top of the map. The failing request carries the broken field name, and a box-and-arrow diagram does not.


### Which bugs does captured traffic not help with?


Deep framework internals, where the fix hides in a buffer or state machine that no request ever touches. Those went from 75% to 79%, essentially flat, while wire-level bugs climbed toward 90%. Traffic is the tiebreaker when the bug’s truth is in the request or response, and dead weight when it isn’t.


### How much does it cost to give an AI agent captured traffic?


Close to free. The full second run, including the four-way test, was about 18,000 model calls for roughly $118, and 94% of the input tokens were cached because the codebase repeats across every run. Bolting a few captures onto each prompt adds little on top of that.


*100 bugs in a private 240-service codebase the model had never seen, four languages, ten runs per bug per condition, scored by a runtime test where editing the wrong service counts as a fail. Per-type numbers are directional at this sample size; race and contract-drift are the rows that clear individual significance.*
